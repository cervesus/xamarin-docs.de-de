---
title: Xamarin.Mac-Tipps zur Problembehandlung
description: Dieses Dokument beschreibt die Methoden zum Beheben von Problemen bei der Entwicklung von Xamarin.Mac-Anwendungen auftreten. Es wird auch erläutert Möglichkeiten, um Unterstützung zu erhalten.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5CBC6822-BCD7-4DAD-8468-6511250D41C4
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: f498aab5bfaffc08a22f62a318f8f9f73ab0afca
ms.sourcegitcommit: d294c967a18e6d91f3909c052eeff98ede1a21f6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/19/2018
ms.locfileid: "53609908"
---
# <a name="xamarinmac-troubleshooting-tips"></a>Xamarin.Mac-Tipps zur Problembehandlung

## <a name="overview"></a>Übersicht

In einigen Fällen abstürzen wir alle bei der Arbeit an einem Projekt auf die Unfähigkeit, erhalten eine API für die gewünschte arbeiten oder in einen Fehler umgehen möchten. Unser Ziel bei Xamarin ist es für Sie erfolgreich in Ihre mobilen und desktop-Anwendungen schreiben, und wir haben einige Ressourcen für bereitgestellt.

Mit einem der folgenden Ressourcen gibt es einige Schritte zur Vorbereitung, die Sie ergreifen können, um das Problem schnell lösen können:

- Bestimmen Sie die Ursache des Problems abstürzen so gut wie möglich:
 
     - "Meine Anwendung" abstürzt"ist nur schwer zu diagnostizieren. "Meine Anwendung stürzt ab, wenn ich ein leeres Array für diesen Aufruf zurückgeben" ist viel einfacher, an deren Behebung zu arbeiten.

     - "NSTable funktioniert mir darf nicht" ist weniger hilfreich, als "Keine der Methoden auf Meine NSTableDelegate scheint in diesem Fall aufgerufen werden."

- Geben Sie möglichst ein kleines Beispielprogramm, das das Problem angezeigt. Schlüsseln Sie durch die Seiten des Quellcodes, die das Problem nach dauert sehr viel mehr Zeit und Aufwand.

- Wissen, welche Änderungen, die Sie für Ihre Anwendung ein Problem verursachen vorgenommen haben, angezeigt werden schnell die Ursache des Problems eingrenzen können. Beachten, wenn Sie vor kurzem die Versionen von Xamarin.Mac aktualisiert haben, kürzen Sie Abschnitte der Anwendung, um den Teil der Fehlerursache zu finden oder vorherigen Tests erstellt, um welche Änderung eingeführt, das Problem zu finden, kann sehr hilfreich sein.


### <a name="what-to-do-when-your-app-crashes-with-no-output"></a>Was Sie tun, wenn Ihre app stürzt ab, über keine Ausgabe

In den meisten Fällen im Debugger in Visual Studio für Mac fängt Ausnahmen in Ihrer Anwendung abstürzt und helfen Ihnen die zugrunde liegende Ursache zu ermitteln. Es gibt jedoch einige Fälle, in dem die Anwendung in das Dock springt und beenden Sie dann, mit wenig oder keine Ausgabe. Dazu gehören:

- Code, die Probleme.
- Bestimmte mono-Laufzeit abstürzt.
- Einige Objective-c-Ausnahmen und abstürzen.
- Einige stürzt sehr früh die Prozesslebensdauer ab.
- Einige Stapeln Überläufe.
- Der MacOS-Version, die aus Ihrem **"Info.plist"** ist neuer als Ihre aktuell installierten MacOS-Version oder ungültig ist.

Debuggen diese Programme kann frustrierend sein, als suchen kann die erforderlichen Informationen kann schwierig sein. Hier sind einige Ansätze, die helfen können:

- Stellen Sie sicher, dass der MacOS-Version, die aus der **"Info.plist"** ist die gleiche wie die Version von MacOS, die derzeit auf dem Computer installiert.
- Überprüfen Sie die Visual Studio für Mac-Anwendungsausgabe (**Ansicht** -> **Pads** -> **Anwendungsausgabe**) für stapelüberwachungen aus, oder rot von Ausgabe Cocoa, die die Ausgabe beschreiben kann.
- Führen Sie die Anwendung über die Befehlszeile, und sehen Sie sich die Ausgabe (in der **Terminal** app) mit: 

     `MyApp.app/Contents/MacOS/MyApp` (wobei `MyApp` ist der Name der Anwendung)
- Sie können die Ausgabe erhöhen, indem Sie den Befehl in der Befehlszeile, z. B. "MONO_LOG_LEVEL" hinzugefügt: 

     `MONO_LOG_LEVEL=debug MyApp.app/Contents/MacOS/MyApp`
- Sie können einen systemeigenen Debugger anfügen (`lldb`) für Ihren Prozess um finden Sie unter Wenn, die es weitere Informationen zur Verfügung stellt (Dies erfordert eine kostenpflichtige Lizenz). Führen Sie z. B. die folgenden Schritte aus:

    1. Geben Sie `lldb MyApp.app/Contents/MacOS/MyApp` im Terminal.
    2. Geben Sie `run` im Terminal.
    3. Geben Sie `c` im Terminal.
    4. Beenden Sie, wenn das Debuggen abgeschlossen.
- Als letzte Möglichkeit, vor dem Aufruf von `NSApplication.Init` in Ihre `Main` Methode (oder an anderen Orten wie erforderlich), Schreiben von Text kann in einer Datei an einem bekannten Speicherort aufzuspüren, an welchen Schritten der starten Sie schwierigkeiten ausgeführt werden.

## <a name="known-issues"></a>Bekannte Probleme

Die folgenden Abschnitte enthalten bekannte Probleme und Lösungen.

### <a name="unable-to-connect-to-the-debugger-in-sandboxed-apps"></a>Es konnte keine Verbindung an den Debugger in Sandbox-apps

Der Debugger eine Verbindung herstellt, Xamarin.Mac-Apps über TCP, das bedeutet, dass standardmäßig beim Sandkasten, aktivieren sie für die app eine Verbindung herstellen kann, sodass, wenn Sie versuchen, die app ausführen, ohne die richtigen Berechtigungen, die aktiviert, Sie einen Fehler erhalten *"kann nicht für die Verbindung der Debugger"*. 

[![Bearbeiten der Berechtigungen](troubleshooting-images/debug01.png "Bearbeiten der Berechtigungen")](troubleshooting-images/debug01-large.png#lightbox)

Die **zulassen ausgehender Netzwerkverbindungen (Client)** Berechtigung ist erforderlich, damit der Debugger, aktivieren diese ermöglicht das Debuggen. Da Sie ohne Debuggen können nicht, wir haben aktualisiert die `CompileEntitlements` Ziel für `msbuild` die Berechtigungen für jede app, die als sandkastenlösung bereitgestellt, für das Debuggen wird nur builds automatisch diese Berechtigung hinzuzufügen. Releasebuilds sollten die Berechtigungen, die in der Berechtigungsdatei enthalten, die unverändert angegeben verwenden.

### <a name="systemnotsupportedexception-no-data-is-available-for-encoding-437"></a>System.NotSupportedException: keine Daten steht für die Codierung 437
 
Wenn 3. Bibliotheken von Drittanbietern in einer Xamarin.Mac-app einschließen möchten, erhalten Sie möglicherweise einen Fehler in der Form "System.NotSupportedException: Keine Daten ist für die Codierung 437" beim Versuch, kompilieren und Ausführen der app verfügbar. Z. B. Bibliotheken, wie z. B. `Ionic.Zip.ZipFile`, kann diese Ausnahme während des Betriebs.

Dies kann gelöst werden, öffnen Sie die Optionen für die Xamarin.Mac-Projekt, das auf **Mac Build** > **Internationalisierung** und die Überprüfung der **West** Internationalisierung:

[![Bearbeiten der Buildoptionen](troubleshooting-images/issue01.png "Editing the build options")](troubleshooting-images/issue01-large.png#lightbox)

### <a name="failed-to-compile-mm5103"></a>Fehler beim Kompilieren (mm5103)

Dieser Fehler wird normalerweise verursacht, wenn eine neue Version von Xcode Version ist und die neue Version installiert haben, aber nicht, aber führen Sie es. Bevor Sie versuchen, die mit einer neuen Version von Xcode zu kompilieren, müssen Sie zunächst, dass Version mindestens einmal ausgeführt.

Beim ersten Ausführen von eine neue Version von Xcode installiert es, mehrere Befehlszeilentools zur Verfügung, die von Xamarin.Mac erforderlich sind. Darüber hinaus sollten Sie einen bereinigten Build durchführen, nach der Aktualisierung von Xcode oder Ihre Version von Xamarin.Mac.

Wenn Sie dieses Problem nicht beheben können, wenden Sie [einen Fehler](#filing-a-bug).

### <a name="missing-entitlementsplist"></a>Fehlende "Entitlements.plist"

Die neueste Version von Visual Studio für Mac den Abschnitt "Berechtigungen" aus wurde entfernt, die **"Info.plist"** Editor und platziert sie in separaten **"Entitlements.plist"** -Editor (für eine bessere plattformübergreifende Unterstützung mit Xamarin.iOS).

Mit dem neuen Visual Studio für Mac installiert haben, bei der Erstellung eines neuen Xamarin.Mac-app-Projekts ein **"Entitlements.plist"** Datei wird automatisch der Projektstruktur hinzugefügt werden:

![Auswählen von Berechtigungen](troubleshooting-images/entitlements01.png "Berechtigungen auszuwählen")

Wenn Sie doppelklicken Sie auf die **"Entitlements.plist"** -Datei, die Berechtigungs-Editor wird angezeigt:

[![Bearbeiten der Berechtigungen](troubleshooting-images/entitlements02.png "Bearbeiten der Berechtigungen")](troubleshooting-images/entitlements02-large.png#lightbox)

Für vorhandene Xamarin.Mac-Projekte, müssen Sie manuell erstellen, die **"Entitlements.plist"** -Datei, indem Sie mit der rechten Maustaste auf das Projekt im der **Lösungspad** und auswählen **hinzufügen**  >  **Neue Datei...** . Wählen Sie als Nächstes **Xamarin.Mac** > **leere Eigenschaftenliste**:

![Hinzufügen einer neuen Eigenschaftenliste](troubleshooting-images/entitlements03.png "hinzufügen eine neue Liste mit Eigenschaften")

Geben Sie `Entitlements` ein, und klicken Sie auf die **neu** Schaltfläche. Wenn das Projekt zuvor keine Berechtigungsdatei enthalten, werden Sie aufgefordert werden, das Projekt und eine neue Datei hinzufügen:

[![Überprüfen das Überschreiben einer Datei](troubleshooting-images/entitlements04.png "überprüfen das Überschreiben einer Datei")](troubleshooting-images/entitlements04-large.png#lightbox)

## <a name="community-support-on-the-forums"></a>Unterstützung durch die Community-Foren

Die Community von Entwicklern, die mithilfe von Xamarin-Produkte ist fantastisch, und viele besuchen Sie unsere [Xamarin.Mac-Foren](http://forums.xamarin.com/categories/mac) Umgebungen und ihre Kenntnisse zu teilen. Darüber hinaus finden Sie Xamarin-Entwickler in regelmäßigen Abständen im Forum zu unterstützen.

<a name="filing-a-bug"/>

## <a name="filing-a-bug"></a>Erstellen einen Fehler

Ihr Feedback ist uns wichtig. Wenn Sie Probleme mit Xamarin.Mac finden:

- Durchsuchen Sie das [Repository „Issues“](https://github.com/xamarin/xamarin-macios/issues). 
- Vor der Umstellung auf das GitHub-Repository „Issues“ wurden Xamarin-Probleme auf [Bugzilla](https://bugzilla.xamarin.com/describecomponents.cgi) nachverfolgt. Suchen Sie dort nach übereinstimmenden Problemen.
- Wenn Sie kein übereinstimmendes Problem finden können, melden Sie ein neues Problem im [GitHub-Repository „Issues“](https://github.com/xamarin/xamarin-macios/issues/new).

GitHub-Issues sind allesamt öffentlich. Es ist nicht möglich, Kommentare oder Anlagen auszublenden. 

Fügen Sie möglichst viele der folgenden Informationen hinzu:                                                                                                                                          

- Ein einfaches Beispiel, um das Problem zu reproduzieren. Dies ist von **sehr großem Nutzen**, sofern möglich. 
- Die vollständige Stapelüberwachung des Absturzes.
- Den C#-Code, der den Absturz umgibt. 
