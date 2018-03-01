---
title: Setup und Installation
description: "Die Entwicklung für WatchOS einrichten"
ms.topic: article
ms.prod: xamarin
ms.assetid: 69F21F15-198D-4B42-A703-21D35CAB0CCA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/05/2017
ms.openlocfilehash: f7e511d7f0a933ab7f29369e5e5f0aa46607c8f8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="installation"></a>Installation

WatchOS 4 erfordert MacOS Sierra (10.12) mit Xcode 9.

WatchOS 1 erforderlich ursprünglich OS X Yosemite (10.10) mit Xcode 7.

> [!WARNING]
> [WatchOS 1 Updates werden nicht nach 1 April 2018 akzeptiert](https://developer.apple.com/news/?id=11162017a). Zukünftige Updates Striches WatchOS 2 SDK oder höher; Erstellen mit der WatchOS ist 4 SDK empfohlen.

## <a name="project-structure"></a>Projektstruktur

Eine Watch-app besteht aus drei Projekten:

- **IPhone-app-Projekt Xamarin.iOS** -Dies ist ein normaler iPhone-Projekt, es kann Xamarin.iOS Vorlagen. Die Watch-App und die zugehörige Erweiterung werden innerhalb dieser Hauptprojekt gebündelt werden.

- **Überwachen-Erweiterungsprojekt** -er den Code (z. B. Controllerklassen) enthält, für die Watch-App.

- **Überwachen von App-Projekt** -enthält die Benutzeroberfläche Storyboard-Datei mit der UI-Ressourcen für die Watch-App.

Die [überwachen Kit Katalog Beispiel](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/) Lösung sieht wie folgt in Xamarin.Studio:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

![](installation-images/catalog-solution.png "Die Projektmappe in Visual Studio")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](installation-images/catalog-solution-vs.png "Die Projektmappe in Visual Studio")

-----

Herunterladen und Ausführen der [WatchKitCatalog](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/) Beispiel, um zu beginnen.
Bildschirme, aus dem Beispiel befinden sich auf die [Steuerelemente](~/ios/watchos/user-interface/index.md) Seite.


## <a name="creating-a-new-project"></a>Erstellen eines neuen Projekts

Sie können nicht Projektmappe erstellen, eine neue "überwachen"... Sie können stattdessen eine Watch-App zu einer vorhandenen iOS-Anwendung hinzufügen. Führen Sie diese Schritte aus, um eine Watch-app zu erstellen:

1. Wenn Sie ein vorhandenes Projekt haben, wählen Sie zunächst **Datei > neue Projektmappe** und iOS-app erstellen (z. B. eine **einzelne Ansicht App**):

    [ ![](installation-images/cycle8-2-sml.png "Wählen Sie die Datei > neue Projektmappe, und erstellen Sie eine iOS-app")](installation-images/cycle8-2.png)

2. Sobald die iOS-app erstellt (oder Sie Ihre vorhandenen iOS-app verwenden möchten), mit der rechten Maustaste auf die Projektmappe, und wählen Sie **hinzufügen > Neues Projekt hinzufügen...** . In der **neues Projekt** aus **WatchOS > App > WatchKit App**:

    [ ![](installation-images/cycle8-6-sml.png "Wählen Sie WatchOS > App > WatchKit App")](installation-images/cycle8-6.png)

3. Im nächste Bildschirm können Sie auswählen, welche iOS-app-Projekt die Watch-app einschließen soll:

    [ ![](installation-images/cycle8-7-sml.png "Wählen Sie die iOS-app-Projekt die Watch-app enthalten soll")](installation-images/cycle8-7.png)

4. Wählen Sie abschließend auf den Speicherort zum Speichern des Projekts (und optional die quellcodeverwaltung aktiviert):

    [ ![](installation-images/cycle8-8-sml.png "Wählen Sie den Speicherort zum Speichern des Projekts")](installation-images/cycle8-8.png)

5. Automatisches Konfigurieren von Visual Studio für Mac [-Projektverweise und **"Info.plist"** Einstellungen](~/ios/watchos/get-started/project-references.md) für Sie.

## <a name="creating-the-watch-user-interface"></a>Erstellen der Benutzeroberfläche für die Überwachung

<a name="designer" />

### <a name="using-the-xamarin-ios-designer"></a>Verwenden die Xamarin-iOS-Designer

Doppelklicken Sie auf der Watch-app **Interface.storyboard** zu bearbeiten, indem die iOS-Designer. Können Sie Schnittstelle Controller und UI-Steuerelemente in das Storyboard aus ziehen die **Toolbox** und konfigurieren Sie sie mithilfe der **Eigenschaften** aufgefüllt:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![](installation-images/iosdesigner-sml.png "Das Storyboard im Designer")](installation-images/iosdesigner.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](installation-images/iosdesigner-sml-vs.png "Das Storyboard im Designer")](installation-images/iosdesigner-vs.png)

-----

Gewähren Sie jedem neuen Schnittstelle Controller eine **Klasse** auswählen, und geben Sie den Namen in der **Eigenschaften** Auffüllzeichen (Dadurch werden die erforderlichen C#-Codebehind-Dateien automatisch erstellen):

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

![](installation-images/iosdesigner-classname.png "Erteilen Sie jedem neuen Schnittstelle Controller eine Klasse")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](installation-images/iosdesigner-classname-vs.png "Erteilen Sie jedem neuen Schnittstelle Controller eine Klasse")

-----

Erstellen von segues **STRG + Ziehen** von einem Controller Schaltfläche, Tabelle oder eine Schnittstelle auf einer anderen Schnittstellencontroller.


### <a name="using-xcode-on-the-mac"></a>Mithilfe von Xcode auf dem Mac

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Sie können weiterhin Xcode zu verwenden, um Ihre Benutzeroberfläche erstellen, indem Sie mit der rechten Maustaste auf die Datei Interface.storyboard und auswählen **Öffnen mit > Xcode Schnittstelle-Generator**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio-Benutzer können auch Xcode verwenden, um ihre Benutzeroberfläche zu erstellen, durch den Wechsel über die Mac-Buildhost direkt verwenden.
Öffnen Sie die Projektmappe in Visual Studio für Mac und mit der rechten Maustaste auf die Interface.storyboard-Datei, und wählen Sie **Öffnen mit > Xcode Schnittstelle-Generator**:

-----

![](installation-images/openwith-xcode.png "Öffnen Sie die Interface.storyboard in Xcode-Schnittstelle-Generator")

Wenn Xcode verwenden möchten, klicken Sie dann befolgen Sie dieselben Schritte für Watch-apps wie bei normalen [iOS-app Storyboards](~/ios/user-interface/storyboards/index.md) (z. B. das Erstellen von Steckdosen und Aktionen von **STRG + Ziehen** in die **h**Headerdatei).

Beim Speichern des Storyboards in Xcode Schnittstelle-Generator wird automatisch hinzugefügt die Steckdosen und Aktionen, die Sie erstellen, die C#- **. designer.cs** Dateien in das Erweiterungsprojekt überwachen.


### <a name="adding-additional-screens-in-xcode"></a>Hinzufügen von zusätzlichen Bildschirme in Xcode

Wenn Sie zusätzliche Bildschirme (hinausgeht in der Vorlage in der Standardeinstellung) hinzufügen, auf das Storyboard mit Xcode Schnittstelle-Generator **C#-Code-Dateien müssen manuell hinzugefügt werden** für jeden neuen Schnittstelle Controller.

Finden Sie in der [erweiterte Anweisungen zum Hinzufügen von neuen Schnittstelle Domänencontroller an einem Storyboard](~/ios/watchos/troubleshooting.md#add).

*Xamarin iOS-Designer wird dies automatisch, es sind keine manuelle Schritte erforderlich.*


## <a name="building"></a>Erstellung

Ein Projekt mit einem Watch-app erstellt wie die anderen iOS-Projekten. Die Erstellung führt eine iPhone-Anwendung (. app), die eine Watch-Erweiterung (.appex), enthält das wiederum die Überwachung von Code weniger-Anwendung (. app).


## <a name="launching"></a>Starten

Sie können Überwachen von apps im Simulator mit entweder mithilfe von Visual Studio für Mac oder Visual Studio (wird gestartet, auf dem Mac-Build-Host) starten.

Es gibt zwei Modi für das Starten einer WatchKit-app:

 - normale app-Modus (Standard), und
- [Benachrichtigungen](~/ios/watchos/platform/notifications.md) (erfordert einen Test benachrichtigungsnutzlast im JSON-Format).

### <a name="xcode-8-support"></a>Xcode-8-Unterstützung

Wenn Xcode 8 (oder höher) installiert ist, Apple Watch-Simulatoren unterscheiden sich von iOS-Simulatoren (im Gegensatz zu [Xcode 6](#xcode6), wobei diese als enthalten ein *externen Anzeige*).
Die Simulator-Liste wird angezeigt, wenn Sie das Watch-App-Projekt wählen und sie das Startprojekt stellen, *iOS-Simulatoren* zur Auswahl (wie unten gezeigt).

[ ![](installation-images/xs-xcode8-watchos3-sml.png "Den Simulator-Typ auswählen")](installation-images/xs-xcode8-watchos3.png)

Beim Starten des Debuggens, *zwei* Simulatoren sollte gestartet werden: die iOS-Simulator *und* der Apple Watch-Simulator. Verwenden Sie **Befehl + Umschalt + H** Navigieren zu dem Menü und Uhrzeitaufgaben Zifferblatt Watch; und Verwenden der **Hardware** Menü Festlegen der **Force berühren Druck**. Scrollen auf der Trackpad oder die Maus, wird die digitale Kronenlänge mit simuliert.

#### <a name="troubleshooting"></a>Problembehandlung

Die folgende Fehlermeldung wird angezeigt, der **Anwendungsausgabe** Wenn Sie versuchen, auf dem Simulator zu starten, die nicht mit eine paarweise zugeordneten Überwachung verfügt:

```csharp
error MT0000: Unexpected error - Please file a bug report at http://bugzilla.xamarin.com
error HE0020: Could not find a paired Watch device for the iOS device 'iPhone 6'.
```

Verweisen auf [Apple Foren](https://forums.developer.apple.com/thread/7783) Anweisungen zum Konfigurieren der Simulatoren, wenn die Standardwerte nicht funktionieren.


<a name="xcode6" />

### <a name="xcode-6-and-watchos-1"></a>Xcode 6 und WatchOS 1

Stellen Sie die *Watch-Erweiterungsprojekt* der **Startprojekt** vor dem Ausführen oder Debuggen der app. Sie können nicht "starten" Watch-app selbst, und bei der Auswahl der iOS-app dann startet im iOS-Simulator wie gewohnt.

Standardmäßig beginnt eine Watch-app im normalen **app** Modus (nicht Blick oder Benachrichtigungen) von Visual Studio für Mac Computer **ausführen** oder **Debuggen** Befehle.

Bei Verwendung von Xcode 6, iPhone 5, iPhone 5ern, iPhone 6 und iPhone 6 Plus kann die Anzeige die externen entweder aktivieren **Apple Watch - 38mm** oder **Apple Watch - 42mm** , in dem sich die überwachen-Anwendungen angezeigt.

**Hinweis:** Denken Sie daran, dass der Bildschirm überwachen im iOS-Simulator nicht automatisch angezeigt wird, bei Verwendung von Xcode 6.
Verwenden der **Hardware > externe zeigt** Menü im Überwachungsfenster Bildschirm angezeigt.

<a name="custommodes" />

## <a name="launching-notification-mode"></a>Benachrichtigungsmodus starten

Finden Sie in der [Benachrichtigungsseite](~/ios/watchos/platform/notifications.md) Informationen wie Benachrichtigungen in Code behandelt.


Visual Studio für Mac kann die Watch-app starten, mit der Benachrichtigung _Startmodus_ für Benachrichtigungen:



Mit der rechten Maustaste auf das Watch-app-Projekt, und wählen Sie **ausführen mit > benutzerdefinierte Konfiguration...** :


[![](installation-images/runwith-customparams-sml.png "Ausführen einer benutzerdefinierten Konfigurations")](installation-images/runwith-customparams.png)


Daraufhin wird die **benutzerdefinierte Parameter** Fenster, in dem Sie auswählen können **Benachrichtigung** (und geben Sie eine JSON-Nutzlast), drücken Sie dann die **ausführen** Watch-app im Simulator zu starten:


[![](installation-images/runwith-execargs-sml.png "Festlegen der Benachrichtigung und Nutzlast")](installation-images/runwith-execargs.png)



## <a name="debugging"></a>Debuggen

Debuggen wird in Visual Studio für Mac und Visual Studio unterstützt.
Denken Sie daran, eine JSON-Benachrichtigungsdatei angeben beim Debuggen im Modus für Benachrichtigungen. Diese bildschirmabbildung zeigt einen Haltepunkt erreicht wird, in eine Watch-app:

![](installation-images/debug-sml.png "Diese bildschirmabbildung zeigt einen Haltepunkt erreicht wird, in eine Watch-app")

Befolgen Sie die Anweisungen zum Starten Sie werden am Ende Ihrer Watch-app unter der **iOS-Simulator (Überwachung)**.
Wählen Sie für den Benachrichtigungsmodus **Debuggen > Öffnen Systemprotokoll** (**CMD + /**) und `Console.WriteLine` im Code.

### <a name="debugging-lifecycle-event-handlers"></a>Lebenszyklus Ereignishandler Debuggen

<!--
To test the functionality in your  and 
    methods, use the **Hardware > Lock** command in the iOS Simulator.
    Locking will trigger the `DidDeactivate` method and the watch simulator
    will indicate that it has been locked. Swipe the iOS Simulator to unlock,
    which triggers the `WillActivate` method of the watch app.
-->

Die WatchOS-Vorlagendateien (z. B. `InterfaceController`, `ExtensionDelegate`, `NotificationController`, und `ComplicationController`) sind im Lieferumfang ihrer erforderlichen Lebenszyklusmethoden bereits implementiert. Hinzufügen `Console.WriteLine` Aufrufe "und" Lesen "die **Anwendungsausgabe** um den Lebenszyklus Ereignis besser zu verstehen.



## <a name="related-links"></a>Verwandte Links

- [WatchKitCatalog (Beispiel)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Erste Watch-App-video](http://blog.xamarin.com/your-first-watch-kit-app/)
- [Apple WatchKit Tipps](https://developer.apple.com/watchkit/tips/)
