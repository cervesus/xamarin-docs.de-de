---
title: Debuggen
description: "Zum Debuggen von Xamarin.iOS-Anwendungen kann der integrierte Debugger in Visual Studio für Mac oder Visual Studio verwendet werden."
ms.topic: article
ms.prod: xamarin
ms.assetid: 05460010-99E1-DC38-F855-2D691EF54484
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 18f9814941c4cd7d2719f23b6102361f013ba8a9
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="debugging"></a>Debuggen

_Zum Debuggen von Xamarin.iOS-Anwendungen kann der integrierte Debugger in Visual Studio für Mac oder Visual Studio verwendet werden._

Verwenden Sie die native Debugunterstützung von Visual Studio für Mac zum Debuggen von C#-Code und Code anderer verwalteter Sprachen, und verwenden Sie [LLDB](http://lldb.llvm.org/tutorial.html), wenn Sie C-, C++- oder Objective-C-Code debuggen müssen, den Sie möglicherweise mit Ihrem Xamarin.iOS-Projekt verknüpfen.


> [!NOTE]
> **WICHTIG:** Beim Kompilieren von Anwendungen im Debugmodus generiert Xamarin.iOS langsamere und viel größere Anwendungen, da jede Codezeile instrumentiert werden muss. Stellen Sie vor der Freigabe sicher, dass Sie einen Releasebuild ausführen.

Der Xamarin.iOS-Debugger ist in die IDE integriert und ermöglicht Entwicklern das Debuggen von Xamarin.iOS-Anwendungen im Simulator sowie auf dem Gerät. Diese Anwendungen wurden mit jeder beliebigen verwalteten Sprache erstellt, die von Xamarin.iOS unterstützt wird.

Der Xamarin.iOS-Debugger verwendet den [Mono Soft-Debugger](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/), was bedeutet, dass der generierte Code und die Mono-Laufzeit mit der IDE zusammenarbeiten, um einen Debugvorgang zu gewährleisten. Dies ist anders als Hard-Debugger wie LLDB oder MDB, die ein Programm ohne das Wissen oder die Zusammenarbeit der debuggten Anwendung steuern.

## <a name="setting-breakpoints"></a>Festlegen von Haltepunkten

Wenn Sie bereit sind, das Debuggen der Anwendung zu beginnen, legen Sie im ersten Schritt [Haltepunkte für die Anwendung fest](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/set_a_breakpoint/). Dies erfolgt durch Klicken in den Randbereich des Editors neben die Zeilennummer des Codes, den Sie unterbrechen möchten:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![](debugging-in-xamarin-ios-images/debugging1.png "Haltepunkte setzen")](debugging-in-xamarin-ios-images/debugging1.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](debugging-in-xamarin-ios-images/debugging1a.png "Haltepunkte setzen")](debugging-in-xamarin-ios-images/debugging1a.png#lightbox)

-----

Sie können sich alle festgelegten Haltepunkte in Ihrem Code anzeigen lassen, indem Sie zum **Pad für Haltepunkte** navigieren:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![](debugging-in-xamarin-ios-images/image0a.png "Das Pad „Haltepunkte“")](debugging-in-xamarin-ios-images/image0a.png#lightbox)

 Wenn das Pad für Haltepunkte nicht automatisch angezeigt wird, können Sie es sichtbar machen, indem Sie Folgendes auswählen: _Ansicht > Debuggen > Fenster > Haltepunkte_
 
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](debugging-in-xamarin-ios-images/image0.png "Das Pad „Haltepunkte“")](debugging-in-xamarin-ios-images/image0.png#lightbox)

 Wenn das Pad für Haltepunkte nicht automatisch angezeigt wird, können Sie es sichtbar machen, indem Sie folgendes auswählen: _Debuggen > Fenster > Haltepunkte_
 
-----

Bevor Sie das Debuggen einer Anwendung beginnen, stellen Sie immer sicher, dass die Konfiguration auf **Debuggen** festgelegt wurde. Dies enthält einen hilfreichen Satz von Tools zum Unterstützen des Debugvorgangs, z.B. Haltepunkte, Datenschnellansichten und eine Anzeige der Aufrufliste:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![](debugging-in-xamarin-ios-images/debugging7.png "Im Simulator debuggen")](debugging-in-xamarin-ios-images/debugging7.png#lightbox)
[![](debugging-in-xamarin-ios-images/debugging7a.png "Auf einem physischen Gerät debuggen")](debugging-in-xamarin-ios-images/debugging7a.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](debugging-in-xamarin-ios-images/debugging7c.png "Im Simulator debuggen")](debugging-in-xamarin-ios-images/debugging7c.png#lightbox)
[![](debugging-in-xamarin-ios-images/debugging7d.png "Auf einem physischen Gerät debuggen")](debugging-in-xamarin-ios-images/debugging7d.png#lightbox)

-----

## <a name="start-debugging"></a>Debugging starten
Wählen Sie zum Starten des Debuggens das Zielgerät oder Ähnliches in Ihrer IDE:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![](debugging-in-xamarin-ios-images/debugging7b.png "Zielgerät auswählen")](debugging-in-xamarin-ios-images/debugging7b.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](debugging-in-xamarin-ios-images/debugging7e.png "Zielgerät auswählen")](debugging-in-xamarin-ios-images/debugging7e.png#lightbox)

-----



Stellen Sie Ihre Anwendung dann bereit, indem Sie auf die Schaltfläche **Wiedergabe** klicken.

Wenn Sie einen Haltepunkt erreichen, wird der Code gelb hervorgehoben:

[![](debugging-in-xamarin-ios-images/image2.png "Der Code wird in gelb hervorgehoben")](debugging-in-xamarin-ios-images/image2.png#lightbox)

Debugtools wie zum Beispiel das, was zur Überprüfung der Werte eines Objekts verwendet wurde, können an dieser Stelle verwendet werden, um weitere Informationen zu den Vorgängen in Ihrem Code zu erhalten:

[![](debugging-in-xamarin-ios-images/image3.png "Farbwert anzeigen")](debugging-in-xamarin-ios-images/image3.png#lightbox)

## <a name="conditional-breakpoints"></a>Bedingte Haltepunkte

Sie können auch Regeln festlegen, durch die die Umstände bestimmt werden, unter denen ein Haltepunkt auftreten soll. Dies wird als das Hinzufügen eines *bedingten Haltepunkts* bezeichnet.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Greifen Sie auf das **Fenster für Haltepunkteigenschaften** zu, um einen bedingten Haltepunkt festzulegen. Dazu gibt es zwei Möglichkeiten:


- Um einen neuen bedingten Haltepunkt hinzuzufügen, klicken Sie mit der rechten Maustaste links der Zeilennummer des Codes, an der Sie einen Haltepunkt setzen möchten, auf den Rand des Editors, und wählen Sie „Neuer Haltepunkt“:

    [![](debugging-in-xamarin-ios-images/image4.png "„Neuer Haltepunkt“ auswählen")](debugging-in-xamarin-ios-images/image4.png#lightbox)

- Klicken Sie mit der rechten Maustaste auf den Haltepunkt, und klicken Sie auf **Haltepunkteigenschaften**, oder wählen Sie im **Pad für Haltepunkte** die unten dargestellte Schaltfläche „Einstellungen“ aus, um eine Bedingung zu einem bestehenden Haltepunkt hinzuzufügen.

    [![](debugging-in-xamarin-ios-images/image5.png "Das Pad „Haltepunkte“")](debugging-in-xamarin-ios-images/image5.png#lightbox)


Anschließend können Sie die Bedingung eingeben, unter der der Haltepunkt auftreten soll:

[![](debugging-in-xamarin-ios-images/image6.png "Bedingung für den erwarteten Haltepunkt angeben")](debugging-in-xamarin-ios-images/image6.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Zum Festlegen eines bedingten Haltepunkts in Visual Studio 2015 legen Sie zuerst [einen normalen Haltepunkt fest](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/set_a_breakpoint/). Klicken Sie mit der rechten Maustaste auf den Haltepunkt, um das Kontextmenü anzuzeigen:

 [![](debugging-in-xamarin-ios-images/image4vs.png "Das Kontextmenü des Haltepunkts")](debugging-in-xamarin-ios-images/image4vs.png#lightbox)

Wählen Sie **Bedingungen...** aus, um das Menü _Haltepunkteinstellungen_ anzuzeigen:

 [![](debugging-in-xamarin-ios-images/image6vs.png "Das Menü „Haltepunkteinstellungen“")](debugging-in-xamarin-ios-images/image6vs.png#lightbox)

Hier können Sie die Bedingungen eingeben, unter denen der Haltepunkt auftreten soll

Weitere Informationen zum Verwenden von Haltepunktbedingungen in früheren Versionen von Visual Studio finden Sie in der [Visual Studio-Dokumentation](https://docs.microsoft.com/visualstudio/debugger/using-breakpoints) zu diesem Thema.

-----

## <a name="navigating-through-code"></a>Navigieren in Code

Wenn ein Haltepunkt erreicht wird, gewähren Ihnen die Debugtools die Kontrolle über die Ausführung des Programms. Die IDE zeigt vier Schaltflächen an, die es Ihnen ermöglichen, den Code auszuführen oder Schritt für Schritt zu durchzugehen.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

In Visual Studio für Mac sehen diese wie folgt aus:

 [![](debugging-in-xamarin-ios-images/image7.png "Die Debugtools ermöglichen dem Entwickler die Kontrolle über die Ausführung des Programms")](debugging-in-xamarin-ios-images/image7.png#lightbox)

Diese lauten wie folgt:

- **Wiedergabe/Anhalten**: Dadurch wird die Ausführung des Codes gestartet oder angehalten, bis der nächste Haltepunkt erreicht wird.
- **Prozedurschritt**: Dadurch wird die nächste Codezeile ausgeführt. Wenn die nächste Zeile ein Funktionsaufruf ist, wird der Prozedurschritt die Funktion ausführen und in der nächsten Codezeile _nach_ der Funktion anhalten.
- **Einzelschritt**: Dadurch wird ebenfalls die nächste Codezeile ausgeführt. Wenn die nächste Zeile ein Funktionsaufruf ist, wird der Einzelschritt in der ersten Zeile der Funktion anhalten, wodurch Sie die Funktion dann Zeile für Zeile debuggen können. Wenn die nächste Zeile keine Funktion ist, funktioniert diese Schaltfläche genauso wie der Prozedurschritt.
- **Rücksprung**: Dadurch wird zu der Zeile zurückgekehrt, in der die aktuelle Funktion aufgerufen wurde.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

In Visual Studio sehen diese wie folgt aus:

[![](debugging-in-xamarin-ios-images/image7vs.png "Die Debugtools ermöglichen dem Entwickler die Kontrolle über die Ausführung des Programms")](debugging-in-xamarin-ios-images/image7vs.png#lightbox)

Diese lauten wie folgt:

- **Wiedergabe/Anhalten**: Dadurch wird die Ausführung des Codes gestartet oder angehalten, bis der nächste Haltepunkt erreicht wird.
- **Prozedurschritt (F11)**: Dadurch wird die nächste Codezeile ausgeführt. Wenn die nächste Zeile ein Funktionsaufruf ist, wird der Prozedurschritt die Funktion ausführen und in der nächsten Codezeile _nach_ der Funktion anhalten.
- **Einzelschritt (F10)**: Dadurch wird ebenfalls die nächste Codezeile ausgeführt. Wenn die nächste Zeile ein Funktionsaufruf ist, wird der Einzelschritt in der ersten Zeile der Funktion anhalten, wodurch Sie die Funktion dann Zeile für Zeile debuggen können. Wenn die nächste Zeile keine Funktion ist, funktioniert diese Schaltfläche genauso wie der Prozedurschritt.
- **Rücksprung (Umschalttaste + F11)**: Dadurch wird zu der Zeile zurückgekehrt, in der die aktuelle Funktion aufgerufen wurde.

Detailliertere Dokumentationen zum Debuggen finden Sie unter [Navigate Code with the Visual Studio Debugger (Navigieren durch den Code mit dem Visual Studio Debugger)](https://docs.microsoft.com/en-us/visualstudio/debugger/navigating-through-code-with-the-debugger).

-----

### <a name="breakpoints"></a>Haltepunkte

Es ist wichtig, darauf hinzuweisen, dass iOS den Anwendungen nur wenige Sekunden (10) zum Start und Abschluss der `FinishedLaunching`-Methode im Anwendungsdelegaten gibt. Wenn die Anwendung diese Methode nicht innerhalb von 10 Sekunden abschließt, wird iOS den Prozess beenden.

Dies bedeutet, dass es beinahe unmöglich ist, Haltepunkte für den Startcode des Programms festzulegen. Wenn Sie den Startcode debuggen möchten, sollten Sie einen Teil der Initialisierung verzögern und in einer durch den Timer aufgerufenen Methode oder in einer anderen Form der Rückrufmethode ausführen, die ausgeführt wird, nachdem FinishedLaunching beendet wurde.

## <a name="device-diagnostics"></a>Gerätediagnose

Wenn beim Einrichten des Debuggers ein Fehler auftritt, können Sie eine detaillierte Diagnose aktivieren, indem Sie „-v -v -v“ zu den zusätzlichen Mtouch-Argumenten in den Projektoptionen hinzufügen. Dadurch werden ausführliche Fehlerinformationen an die Konsole des Geräts übergeben.

 <a name="WiFi_Debugging" />

## <a name="wireless-debugging"></a>Drahtloses Debuggen

Der Standardvorgang in Xamarin.iOS ist das Debuggen der Anwendung auf Ihren Geräten über die USB-Verbindung. In einigen Fällen ist das USB-Gerät möglicherweise erforderlich, um das Einstecken/Ausstecken des Kabels für die Entwicklung von mit ExternalAccessory unterstützten Anwendungen zu testen. In diesen Fällen können Sie den Debugvorgang über das drahtlose Netzwerk durchführen.

Weitere Informationen zur drahtlosen Bereitstellen und zum drahtlosen Debuggen finden Sie im Leitfaden zur [drahtlosen Bereitstellung](~/ios/deploy-test/wireless-deployment.md).

<a name="Technical_Details" />

## <a name="technical-details"></a>Technische Details

Xamarin.iOS verwendet den neuen Mono Soft-Debugger. Im Gegensatz zum Mono-Standarddebugger, einem Programm, das einen separaten Prozess mithilfe von Betriebssystemschnittstellen für die Steuerung von separaten Prozessen steuert, funktioniert der Soft-Debugger, indem die Mono-Laufzeit die Debuggingfunktionen über ein Versandprotokoll verfügbar macht.

Beim Start kontaktiert eine Anwendung, die debuggt werden soll, den Debugger, und der Debugger wird gestartet. In Xamarin.iOS für Visual Studio fungiert der Xamarin Mac-Agent als Mittelsmann zwischen der Anwendung (in Visual Studio) und dem Debugger.

Dieser Soft-Debugger erfordert ein kooperatives Debugschema bei der Ausführung auf dem Gerät. Dies bedeutet, dass die Binärbuilds beim Debuggen größer sein werden als der Code. Dieser ist dazu instrumentiert, an jedem Sequenzpunkt zusätzlichen Code zur Debugunterstützung zu enthalten.

<a name="Accessing_the_Console" />


## <a name="accessing-the-console"></a>Zugreifen auf die Konsole

Es werden Absturzprotokolle und die Ausgabe der Console-Klasse an die iPhone-Konsole gesendet. Sie können mit Xcode auf diese Konsole zugreifen. Verwenden Sie den „Organisator“, und wählen Sie Ihr Gerät aus.

Wenn Sie Xcode nicht starten möchten, können Sie alternativ das [iPhone-Konfigurationsprogramm](http://www.apple.com/support/iphone/enterprise/) von Apple verwenden, um direkt auf die Konsole zuzugreifen. Dies hat den zusätzlichen Vorteil, dass Sie von einem Windows-Computer aus auf Konsolenprotokolle zugreifen können, wenn Sie ein Problem im Feld debuggen.

Für Visual Studio-Benutzer stehen einige Protokolle im Ausgabefenster zur Verfügung, aber für gründlichere und ausführlichere Protokolle sollten Sie zu Ihrem Mac wechseln.

-----

<a name="Debugging_Mono's_Class_Libraries" />


## <a name="debugging-monos-class-libraries"></a>Debuggen der Mono-Klassenbibliotheken

Xamarin.iOS enthält den Quellcode für die Mono-Klassenbibliotheken, die Sie verwenden können, um in einem einzigen Schritt vom Debugger zu einer Überprüfung der Vorgänge im Hintergrund zu gelangen.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Da dieses Feature während des Debuggens mehr Arbeitsspeicher benötigt, ist es standardmäßig deaktiviert.


Um dieses Feature zu aktivieren, stellen Sie sicher, dass die Option **Nur Projektcode debuggen, keinen Einzelschritt in Frameworkcode ausführen** im Menü _Visual Studio für Mac > Einstellungen > Debugger_ deaktiviert ist, wie unten gezeigt:

[![](debugging-in-xamarin-ios-images/debugging6.png "Mono-Klassenbibliotheken debuggen")](debugging-in-xamarin-ios-images/debugging6.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Sie müssen zum Debuggen von Klassenbibliotheken in Visual Studio **Nur eigenen Code** im Menü _Debuggen > Optionen_ deaktivieren. Deaktivieren Sie im Knoten _Debugging > Allgemein_ das Kontrollkästchen **Nur meinen Code aktivieren**:

[![](debugging-in-xamarin-ios-images/debugging6vs.png "Mono-Klassenbibliotheken debuggen")](debugging-in-xamarin-ios-images/debugging6vs.png#lightbox)

-----

Sobald Sie dies tun, können Sie Ihre Anwendung und einen Einzelschritt in beliebigen Mono Core-Klassenbibliotheken starten.


## <a name="related-links"></a>Verwandte Links

- [Debuggen mit Xamarin](/visualstudio/mac/debugging/)
- [Data Visualizations (Datenvisualisierungen)](/visualstudio/mac/data-visualizations/)
- [Festlegen eines Haltepunkts](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/set_a_breakpoint/)
- [Schritt-für-Schritt-Ausführung des Codes](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/step_through_code/)
- [Von Ausgabeinformationen zum Protokollfenster](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/output_information_to_log_window/)
