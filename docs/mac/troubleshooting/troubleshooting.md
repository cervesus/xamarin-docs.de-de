---
title: Xamarin.Mac-Tipps zur Problembehandlung
description: Dieses Dokument beschreibt die Vorgehensweisen zum Beheben von Problemen beim Entwickeln von Anwendungen Xamarin.Mac. Darüber hinaus erläutert Möglichkeiten, um Unterstützung zu erhalten.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5CBC6822-BCD7-4DAD-8468-6511250D41C4
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 5e6cd5b338034cfa9956244015d4585b4f005793
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792966"
---
# <a name="xamarinmac-troubleshooting-tips"></a>Xamarin.Mac-Tipps zur Problembehandlung

## <a name="overview"></a>Überblick

In einigen Fällen Festfahren wir alle bei der Arbeit an einem Projekt auf die Unfähigkeit, erhalten eine API zur Funktionsweise der gewünschte oder bei dem Versuch, einen Fehler zu umgehen. Ziel von Xamarin ist, damit Sie erfolgreich in Ihrer mobilen und desktop-Anwendungen schreiben können, und wir haben einige Ressourcen zum bereitgestellt.

Mit diesen Ressourcen gibt es einige Schritte der Vorbereitung, die Sie ergreifen können, können sie Ihr Problem schneller beheben:

- Bestimmen Sie die Ursache des Problems als bewährte Lösung wie möglich am Bericht Abstürze (crashes):
 
     - "Meine Anwendung abstürzt" sind schwer zu diagnostizieren. "Meine Anwendung stürzt ab, wenn ich auf diesen Aufruf ein leeres Array zurückzugeben" ist viel einfacher zu korrigieren, und bearbeiten.

     - "NSTable arbeiten erhalte darf nicht" ist weniger hilfreich, als "Keine der Methoden auf Meine NSTableDelegate scheint in diesem Fall aufgerufen werden."

- Geben Sie nach Möglichkeit eine kleine Beispielprogramm zeigt das Problem. Schlüsseln Sie durch die Seiten des Quellcodes, die für das Problem suchen akzeptiert Prozessorfehlern mehr Zeit und Aufwand.

- Wissen, welche Änderungen, die Sie für Ihre Anwendung ein Problem verursachen vorgenommen haben, angezeigt werden schnell eingrenzen der Ursache des Problems eingrenzen können. Beachten, wenn Sie Versionen von Xamarin.Mac, indem Sie die Abschnitte der Anwendung, um die Ursache des Problems gefunden kürzlich aktualisiert haben oder Tests von vorherigen zum Ermitteln haben Builds, welche Änderung eingeführt, das Problem kann sehr hilfreich sein.


### <a name="what-to-do-when-your-app-crashes-with-no-output"></a>Vorgehensweise bei die app keinen stürzt ab

In den meisten Fällen im Debugger in Visual Studio für Mac Ausnahmen abgefangen und in der Anwendung abstürzt und Sie beitragen, die Ursache zu ermitteln. Es gibt jedoch einige Fälle, in denen Ihre Anwendung auf Docks bounce und beenden Sie dann, mit wenig oder keine Ausgabe. Dazu gehören:

- Codesignatur-Probleme.
- Bestimmte Mono / Runtime stürzt ab.
- Einige Objective-c-Ausnahmen und abstürzen.
- Einige stürzt sehr frühen die Prozesslebensdauer ab.
- Einige einen Überlauf des Stapels.
- Die MacOS Version aufgeführt, die Ihrem **"Info.plist"** ist neuer als der derzeit installierten MacOS-Version oder es ungültig ist.

Diese Programme Debuggen kann frustrierend sein, als suchen die erforderlichen Informationen es schwierig sein kann. Hier sind ein paar Ansätze, die Aufschluss darüber geben könnten:

- Stellen Sie sicher, dass die MacOS Version aufgeführt, die der **"Info.plist"** dasjenige mit der Version des MacOS, die derzeit auf dem Computer installiert ist.
- Überprüfen Sie die Visual Studio für Mac-Anwendungsausgabe (**Ansicht** -> **füllt** -> **Anwendungsausgabe**) für stapelüberwachungen oder rot von Ausgabe Kakao, in denen die Ausgabe beschrieben werden kann.
- Führen Sie die Anwendung über die Befehlszeile, und sehen Sie sich die Ausgabe (in der **Terminaldienste** app) mit: 

     `MyApp.app/Contents/MacOS/MyApp` (wobei `MyApp` ist der Name der Anwendung)
- Sie können die Ausgabe erhöhen, indem Sie den Befehl in der Befehlszeile angegeben, z. B. "MONO_LOG_LEVEL" hinzugefügt: 

     `MONO_LOG_LEVEL=debug MyApp.app/Contents/MacOS/MyApp`
- Sie können einen systemeigenen Debugger anfügen (`lldb`) auf Ihrem Prozess um zu ermitteln, wenn, aus denen eine beliebige weitere (Dies erfordert eine kostenpflichtige Lizenz). Führen Sie z. B. folgende:

    1. Geben Sie `lldb MyApp.app/Contents/MacOS/MyApp` in der abschließenden.
    2. Geben Sie `run` in der abschließenden.
    3. Geben Sie `c` in der abschließenden.
    4. Wenn nach Abschluss des Debuggens zu beenden.
- Als letzte Möglichkeit, vor dem Aufruf von `NSApplication.Init` in Ihrer `Main` Methode (oder an anderen Orten nach Bedarf), Schreiben von Text kann in einer Datei an einem bekannten Speicherort zum Auffinden, an welchen Schritten der Start Ihres Systems ausgeführt werden.

## <a name="known-issues"></a>Bekannte Probleme

Die folgenden Abschnitte enthalten bekannte Probleme und ihre Lösungen.

### <a name="unable-to-connect-to-the-debugger-in-sandboxed-apps"></a>Es konnte keine Verbindung an den Debugger in Sandbox-apps

Der Debugger eine Verbindung herstellt, Xamarin.Mac-Apps über TCP, das bedeutet, dass standardmäßig beim Sandkasten, aktivieren sie für die Verbindung der App kann, sodass, wenn Sie versuchen, die app auszuführen, ohne die richtigen Berechtigungen aktiviert, Sie einen Fehler erhalten *"Es konnte keine Verbindung mit der Debugger"*. 

[![Bearbeiten die Berechtigungen](troubleshooting-images/debug01.png "die Berechtigungen bearbeiten")](troubleshooting-images/debug01-large.png#lightbox)

Die **zulassen ausgehende Netzwerkverbindungen (Client)** Berechtigung ist erforderlich, damit der Debugger, kann durch Aktivierung dieser Einstellung normalerweise Debuggen. Da Sie ohne Debuggen können, haben wir aktualisiert die `CompileEntitlements` Ziel für `msbuild` hinzuzufügende automatisch über diese Berechtigung die Berechtigungen für jede app, die für das Debuggen Sandbox ist nur builds. Releasebuilds sollten die Berechtigungen, die in der Berechtigungsdatei unverändert bleiben sollen angegeben verwenden.

### <a name="systemnotsupportedexception-no-data-is-available-for-encoding-437"></a>System.NotSupportedException: keine Daten steht für die Codierung 437
 
Wenn in Ihrer app Xamarin.Mac 3rd Party Bibliotheken einschließen möchten, erhalten Sie möglicherweise einen Fehler in der Form "System.NotSupportedException: keine Daten für die Codierung 437 verfügbar ist" beim Versuch, kompilieren und Ausführen der app. Z. B. Bibliotheken, wie z. B. `Ionic.Zip.ZipFile`, während der Operation-Ausnahme auslösen können.

Dadurch gelöst werden kann, indem Sie die Optionen für das Xamarin.Mac-Projekt, und navigieren Sie zu öffnen **Mac erstellen** > **Internationalisierung** und Überprüfung der **West** Internationalisierung:

[![Bearbeiten der Buildoptionen](troubleshooting-images/issue01.png "Editing the build options")](troubleshooting-images/issue01-large.png#lightbox)

### <a name="failed-to-compile-mm5103"></a>Fehler beim Kompilieren (mm5103)

Dieser Fehler wird normalerweise verursacht, wenn eine neue Version von Xcode Version und die neue Version installiert, jedoch noch ausgeführt hat. Bevor Sie versuchen, eine neue Version von Xcode zu kompilieren, müssen Sie zuerst diese Version mindestens einmal ausgeführt.

Beim erstmaligen eine neue Version von Xcode ausführen, installiert sie mehrere Befehlszeilentools Xamarin.Mac erforderlich sind. Darüber hinaus sollten Sie einen bereinigten Build durchführen, nach der Aktualisierung von Xcode oder Ihrem Xamarin.Mac-Version.

Wenn Sie dieses Problem nicht beheben können, wenden [einen Fehler](#filing-a-bug).

### <a name="missing-entitlementsplist"></a>Fehlende entitlements.plist

Die neueste Version von Visual Studio für Mac wurde im Abschnitt "Berechtigungen" aus entfernt die **"Info.plist"** Editor und platziert es in separaten **Entitlements.plist** -Editor (für eine bessere plattformübergreifende Unterstützung mit Xamarin.iOS).

Mit dem neuen Visual Studio für Mac installiert ist, beim Erstellen eines neuen Xamarin.Mac-app-Projekts ein **Entitlements.plist** Datei wird automatisch der Projektstruktur hinzugefügt werden:

![Auswählen von Berechtigungen](troubleshooting-images/entitlements01.png "Berechtigungen auswählen")

Wenn Sie doppelklicken Sie auf die **Entitlements.plist** Datei, die Ansprüche-Editor wird angezeigt:

[![Bearbeiten die Berechtigungen](troubleshooting-images/entitlements02.png "die Berechtigungen bearbeiten")](troubleshooting-images/entitlements02-large.png#lightbox)

Für vorhandene Xamarin.Mac Projekte müssen Sie manuell erstellen die **Entitlements.plist** -Datei, indem Sie mit der rechten Maustaste auf das Projekt in der **Lösung Pad** auswählen und **hinzufügen**  >  **Neue Datei...** . Wählen Sie als Nächstes **Xamarin.Mac** > **leeren Eigenschaftenliste**:

![Hinzufügen einer neuen Eigenschaftenliste](troubleshooting-images/entitlements03.png "hinzufügen eine neue Liste mit Eigenschaften")

Geben Sie `Entitlements` für den Namen und klicken Sie auf die **neu** Schaltfläche. Wenn das Projekt zuvor eine Berechtigungsdatei enthalten, werden Sie aufgefordert, das Projekt und erstellen eine neue Datei hinzufügen:

[![Überprüfen das Überschreiben einer Datei](troubleshooting-images/entitlements04.png "überprüfen das Überschreiben einer Datei")](troubleshooting-images/entitlements04-large.png#lightbox)

## <a name="contacting-support-business-or-enterprise-licenses"></a>Kontaktaufnahme mit dem Support (Lizenzen Business oder Enterprise)

Wenn Sie ein Geschäfts- oder Enterprise-Lizenz verfügen, können Sie zum Anfordern von Hilfe direkt von Xamarin Engineers über Support-Tickets geeignet. Finden Sie unter [xamarin.com/support](http://xamarin.com/support) für Details.

## <a name="community-support-on-the-forums"></a>Communityunterstützung in den Foren

Die Community von Entwicklern, die mithilfe von Xamarin-Produkte ist erstaunlich und viele besuchen Sie unsere [Xamarin.Mac Foren](http://forums.xamarin.com/categories/mac) Erfahrungen und ihre Kenntnisse gemeinsam nutzen. Darüber hinaus finden Sie Xamarin Engineers in regelmäßigen Abständen im Forum zu unterstützen.

<a name="filing-a-bug"/>

## <a name="filing-a-bug"></a>Ablegen eines Fehlers

Ihr Feedback ist uns wichtig. Wenn Sie Probleme bei Xamarin.Mac finden:

- Durchsuchen Sie das [Repository „Issues“](https://github.com/xamarin/xamarin-macios/issues). 
- Vor der Umstellung auf das GitHub-Repository „Issues“ wurden Xamarin-Probleme auf [Bugzilla](https://bugzilla.xamarin.com/describecomponents.cgi) nachverfolgt. Suchen Sie dort nach übereinstimmenden Problemen.
- Wenn Sie kein übereinstimmendes Problem finden können, melden Sie ein neues Problem im [GitHub-Repository „Issues“](https://github.com/xamarin/xamarin-macios/issues/new).

GitHub-Issues sind allesamt öffentlich. Es ist nicht möglich, Kommentare oder Anlagen auszublenden. 

Fügen Sie möglichst viele der folgenden Informationen hinzu:                                                                                                                                          

- Ein einfaches Beispiel, um das Problem zu reproduzieren. Dies ist von **sehr großem Nutzen**, sofern möglich. 
- Die vollständige Stapelüberwachung des Absturzes.
- Den C#-Code, der den Absturz umgibt. 
