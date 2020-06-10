---
title: Tipps zur Problembehandlung bei xamarin. Mac
description: In diesem Dokument werden Ansätze zum Beheben von Problemen beschrieben, die beim Entwickeln von xamarin. Mac-Anwendungen auftreten. Außerdem wird erläutert, wie Sie Support erhalten.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5CBC6822-BCD7-4DAD-8468-6511250D41C4
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 683915d238f6c8aee10957285ad4438316e1e037
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84567122"
---
# <a name="xamarinmac-troubleshooting-tips"></a>Tipps zur Problembehandlung bei xamarin. Mac

## <a name="overview"></a>Übersicht

Manchmal bleiben wir bei der Arbeit an einem Projekt auf dem Weg, entweder wenn Sie nicht in der Lage sind, eine API so zu arbeiten, wie wir möchten, oder wenn Sie versuchen, einen Fehler zu umgehen. Unser Ziel von xamarin besteht darin, dass Sie Ihre mobilen und Desktop Anwendungen erfolgreich schreiben können. Wir haben einige Ressourcen bereitgestellt, die Ihnen helfen.

Mit jeder dieser Ressourcen sind einige Vorbereitungsschritte erforderlich, um Sie bei der schnellen Lösung Ihres Problems zu unterstützen:

- Bestimmen Sie die Ursache des Problems so am besten, dass Abstürze gemeldet werden:

  - "Meine Anwendung stürzt ab" ist schwierig zu diagnostizieren. "Meine Anwendung stürzt ab, wenn ich ein leeres Array an diesen-Befehl zurückgebe" ist viel leichter an der Korrektur zu arbeiten.

  - "Es ist nicht möglich, nstable zu arbeiten" ist weniger hilfreich als "keine der Methoden in meinem nstabledelegat wird in diesem Fall anscheinend aufgerufen."

- Geben Sie nach Möglichkeit ein kleines Beispielprogramm an, das das Problem anzeigt. Durch das Durchsuchen von Seiten des Quellcodes, die nach dem Problem suchen, werden mehr Zeit und Aufwand benötigt.

- Wenn Sie wissen, welche Änderungen Sie an Ihrer Anwendung vorgenommen haben, damit ein Problem auftritt, können Sie schnell die Ursache des Problems eingrenzen. Beachten Sie, dass Sie vor kurzem ein Upgrade von xamarin. Mac-Versionen durchgeführt haben, indem Sie Abschnitte der Anwendung kürzen, um den Teil zu finden, der das Problem verursacht hat, oder vorherige Builds testen, um herauszufinden, welche Änderung das Problem hervorgebracht hat.

### <a name="what-to-do-when-your-app-crashes-with-no-output"></a>Vorgehensweisen bei Abstürzen der APP ohne Ausgabe

In den meisten Fällen fängt der Debugger in Visual Studio für Mac Ausnahmen und Abstürze in der Anwendung ab und hilft Ihnen dabei, die Ursache zu ermitteln. Es gibt jedoch einige Fälle, in denen Ihre Anwendung auf dem Dock springt und dann mit wenigen oder gar keinen Ausgaben beendet wird. Folgende sind möglich:

- Probleme beim Code signieren.
- Bestimmte Mono-Laufzeit stürzt ab.
- Einige Ziel-c-Ausnahmen und Abstürze.
- Einige Abstürze sind sehr frühzeitig die Prozess Lebensdauer.
- Einige Stapel Überläufe.
- Die in der Datei " **Info. plist** " aufgeführte MacOS-Version ist neuer als die aktuell installierte MacOS-Version, oder Sie ist ungültig.

Das Debuggen dieser Programme kann frustrierend sein, da das Auffinden der erforderlichen Informationen schwierig sein kann. Im folgenden finden Sie einige Ansätze, die möglicherweise hilfreich sind:

- Stellen Sie sicher, dass die in der Datei " **Info. plist** " aufgeführte MacOS-Version mit der Version von MacOS übereinstimmt, die derzeit auf dem Computer installiert ist.
- Überprüfen Sie die Visual Studio für Mac Anwendungs Ausgabe (**anzeigen**  ->  der**Pads**-  ->  **Anwendungs Ausgabe**) für Stapel Überwachungen oder Ausgaben in roter Farbe aus Cocoa, die die Ausgabe beschreiben können.
- Führen Sie die Anwendung über die Befehlszeile aus, und überprüfen Sie die Ausgabe (in der **Terminal** -APP) mithilfe von:

  `MyApp.app/Contents/MacOS/MyApp`(wobei `MyApp` der Name der Anwendung ist.)
- Sie können die Ausgabe vergrößern, indem Sie "MONO_LOG_LEVEL" dem Befehl in der Befehlszeile hinzufügen, z. b.:

  `MONO_LOG_LEVEL=debug MyApp.app/Contents/MacOS/MyApp`
- Sie können einen systemeigenen Debugger ( `lldb` ) an Ihren Prozess anfügen, um festzustellen, ob dieser weitere Informationen enthält (Dies erfordert eine kostenpflichtige Lizenz). Führen Sie beispielsweise die folgenden Schritte aus:

  1. Geben Sie `lldb MyApp.app/Contents/MacOS/MyApp` im Terminal ein.
  2. Geben Sie `run` im Terminal ein.
  3. Geben Sie `c` im Terminal ein.
  4. Beenden, nachdem das Debugging abgeschlossen wurde.
- Als letztes Mittel `NSApplication.Init` können Sie vor dem Aufrufen von in Ihrer- `Main` Methode (oder an anderen Stellen wie erforderlich) Text in eine Datei an einem bekannten Speicherort schreiben, um zu verfolgen, bei welchem Schritt der Startprobleme auftreten.

## <a name="known-issues"></a>Bekannte Probleme

In den folgenden Abschnitten werden bekannte Probleme und ihre Lösungen behandelt.

### <a name="unable-to-connect-to-the-debugger-in-sandboxed-apps"></a>Die Verbindung mit dem Debugger in Sandbox-apps kann nicht hergestellt werden.

Der Debugger stellt über TCP eine Verbindung mit xamarin. Mac-apps her. Dies bedeutet, dass beim Aktivieren von Sandboxing standardmäßig keine Verbindung mit der APP hergestellt werden kann. Wenn Sie also versuchen, die APP auszuführen, ohne dass die entsprechenden Berechtigungen aktiviert sind, erhalten Sie die Fehlermeldung *"Es konnte keine Verbindung mit dem Debugger*hergestellt werden".

[![Bearbeiten der Berechtigungen](troubleshooting-images/debug01.png "Bearbeiten der Berechtigungen")](troubleshooting-images/debug01-large.png#lightbox)

Die Berechtigung **ausgehende Netzwerkverbindungen zulassen (Client)** ist die Berechtigung, die für den Debugger erforderlich ist. durch Aktivieren dieser Option wird das Debugging normal zugelassen. Da Sie nicht ohne diese Debuggen können, haben wir das `CompileEntitlements` Ziel für aktualisiert, `msbuild` um diese Berechtigung automatisch den Berechtigungen für jede APP hinzuzufügen, die nur für Debugbuilds in einem Sandkasten enthalten ist. Releasebuilds sollten die Berechtigungen unverändert verwenden, die in der Berechtigungsdatei angegeben sind.

### <a name="systemnotsupportedexception-no-data-is-available-for-encoding-437"></a>System. NotSupportedException: Es sind keine Daten für die Codierung 437 verfügbar.

Wenn Sie Drittanbieterbibliotheken in Ihre xamarin. Mac-app einschließen, erhalten Sie möglicherweise einen Fehler im Format "System. NotSupportedException: Es sind keine Daten für die Codierung 437 verfügbar", wenn Sie versuchen, die APP zu kompilieren und auszuführen. Beispielsweise können Bibliotheken, z `Ionic.Zip.ZipFile` . b., diese Ausnahme während des Vorgangs auslösen.

Dies kann behoben werden, indem Sie die Optionen für das xamarin. Mac-Projekt öffnen, zur **Mac**  >  -**buildinternationalisierung** wechseln und die **westliche** Internationalisierung überprüfen:

[![Bearbeiten der Buildoptionen](troubleshooting-images/issue01.png "Bearbeiten der Buildoptionen")](troubleshooting-images/issue01-large.png#lightbox)

### <a name="failed-to-compile-mm5103"></a>Kompilierung fehlgeschlagen (mm5103)

Dieser Fehler tritt normalerweise auf, wenn eine neue Version von Xcode veröffentlicht wird und Sie die neue Version installiert, aber noch nicht ausgeführt haben. Bevor Sie versuchen, mit einer neuen Version von Xcode zu kompilieren, müssen Sie diese Version zunächst mindestens einmal ausführen.

Wenn Sie eine neue Version von Xcode zum ersten Mal ausführen, werden mehrere Befehlszeilen Tools installiert, die von xamarin. Mac benötigt werden. Außerdem sollten Sie nach dem Aktualisieren von Xcode oder ihrer xamarin. Mac-Version einen sauberen Build durchführen.

Wenn Sie dieses Problem nicht beheben können, melden Sie [einen Fehler](#filing-a-bug).

### <a name="missing-entitlementsplist"></a>Fehlende Berechtigungen. plist

Die neueste Version von Visual Studio für Mac hat den Abschnitt "Berechtigungen" aus dem **Info. plist** -Editor entfernt und in separaten **Berechtigungen. plist** -Editor abgelegt (für eine bessere plattformübergreifende Unterstützung mit xamarin. IOS).

Wenn Sie beim Erstellen eines neuen xamarin. Mac-app-Projekts die neue Visual Studio für Mac installiert haben, wird der Projektstruktur automatisch eine Datei " **Berechtigungen. plist** " hinzugefügt:

![Auswählen von Berechtigungen](troubleshooting-images/entitlements01.png "Auswählen von Berechtigungen")

Wenn Sie auf die Datei " **Berechtigungen. plist** " doppelklicken, wird der Berechtigungs-Editor angezeigt:

[![Bearbeiten der Berechtigungen](troubleshooting-images/entitlements02.png "Bearbeiten der Berechtigungen")](troubleshooting-images/entitlements02-large.png#lightbox)

Für vorhandene xamarin. Mac-Projekte müssen Sie die Datei " **Berechtigungen. plist** " manuell erstellen, indem Sie im **Lösungspad** mit der rechten Maustaste auf das Projekt klicken und **Add**  >  **neue Datei**hinzufügen auswählen. Wählen Sie als nächstes **xamarin. Mac**  >  **leere Eigenschaften Liste**aus:

![Hinzufügen einer neuen Eigenschaften Liste](troubleshooting-images/entitlements03.png "Hinzufügen einer neuen Eigenschaften Liste")

Geben Sie `Entitlements` als Namen ein, und klicken Sie auf die Schaltfläche **neu** . Wenn Ihr Projekt zuvor eine Berechtigungsdatei enthielt, werden Sie aufgefordert, Sie dem Projekt hinzuzufügen, anstatt eine neue Datei zu erstellen:

[![Überprüfen der Datei überschreiben](troubleshooting-images/entitlements04.png "Überprüfen der Datei überschreiben")](troubleshooting-images/entitlements04-large.png#lightbox)

## <a name="community-support-on-the-forums"></a>Communityunterstützung in den Foren

Die Community von Entwicklern, die xamarin-Produkte verwenden, ist erstaunlich, und viele besuchen unsere [xamarin. Mac-Foren](https://forums.xamarin.com/categories/xamarin-mac) , um Erfahrungen und deren Fachkenntnisse zu teilen. Außerdem besuchen xamarin-Techniker in regelmäßigen Abständen das Forum, um Hilfe zu erhalten.

<a name="filing-a-bug"></a>

## <a name="filing-a-bug"></a>Einreichen eines Fehlers

Ihr Feedback ist uns sehr wichtig. Wenn Sie Probleme mit xamarin. Mac finden:

- Durchsuchen Sie das [Repository „Issues“](https://github.com/xamarin/xamarin-macios/issues).
- Vor der Umstellung auf das GitHub-Repository „Issues“ wurden Xamarin-Probleme auf [Bugzilla](https://bugzilla.xamarin.com/describecomponents.cgi) nachverfolgt. Suchen Sie dort nach übereinstimmenden Problemen.
- Wenn Sie kein übereinstimmendes Problem finden können, melden Sie ein neues Problem im [GitHub-Repository „Issues“](https://github.com/xamarin/xamarin-macios/issues/new).

GitHub-Issues sind allesamt öffentlich. Es ist nicht möglich, Kommentare oder Anlagen auszublenden.

Fügen Sie möglichst viele der folgenden Informationen hinzu:

- Ein einfaches Beispiel, um das Problem zu reproduzieren. Dies ist von **sehr großem Nutzen**, sofern möglich.
- Die vollständige Stapelüberwachung des Absturzes.
- Den C#-Code, der den Absturz umgibt.
