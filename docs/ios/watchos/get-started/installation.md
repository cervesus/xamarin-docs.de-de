---
title: Installieren und Verwenden von WatchOS in Xamarin
description: Dieses Dokument beschreibt das Installieren und Verwenden von WatchOS mit Xamarin. Es wird erläutert, Installation, WatchOS-Projekt zu strukturieren, wie Sie mithilfe der iOS-Designer, die Xcode-Integration, und enthält Tipps zur Problembehandlung.
ms.prod: xamarin
ms.assetid: 69F21F15-198D-4B42-A703-21D35CAB0CCA
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 12/05/2017
ms.openlocfilehash: a2fbb44587eed7f7158c813e45b810cf7f15d0d4
ms.sourcegitcommit: 4859da8772dbe920fdd653180450e5ddfb436718
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50234894"
---
# <a name="installing-and-using-watchos-in-xamarin"></a>Installieren und Verwenden von WatchOS in Xamarin

WatchOS 4 ist MacOS Sierra (10.12) mit Xcode 9 erforderlich.

WatchOS 1 erforderlich ursprünglich OS X Yosemite (10.10) mit Xcode 7.

> [!WARNING]
> [Updates für WatchOS 1 werden nicht akzeptiert, nach dem 1. April 2018](https://developer.apple.com/news/?id=11162017a). Zukünftige Updates müssen verwenden WatchOS 2 SDK oder höher, Erstellen mit dem WatchOS empfiehlt 4 SDK.

## <a name="project-structure"></a>Struktur des Projekts

Eine Watch-app besteht aus drei Projekten:

- **Xamarin.iOS-iPhone-app-Projekt** – Dies ist ein normaler iPhone-Projekt, kann es sein, den Xamarin.iOS-Vorlagen. Die Watch-App und ihre Erweiterung werden in diesem Projekt wichtigsten gebündelt werden.

- **Sehen Sie sich das Projekt** -enthält den Code (z. B. Controllerklassen) für die Watch-App.

- **Watch-App-Projekt** -enthält die Benutzeroberfläche Storyboard-Datei mit allen im UI-Ressourcen für die Watch-App.

Die [Watch Kit Katalog-Beispiel](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/) Lösung sieht wie folgt in xamarin.Studio die Datei aus:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![](installation-images/catalog-solution.png "Die Projektmappe in Visual Studio")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](installation-images/catalog-solution-vs.png "Die Projektmappe in Visual Studio")

-----

Herunterladen und Ausführen der [WatchKitCatalog](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/) Beispiel für den Einstieg.
Bildschirme, aus dem Beispiel finden Sie auf die [Steuerelemente](~/ios/watchos/user-interface/index.md) Seite.


## <a name="creating-a-new-project"></a>Erstellen eines neuen Projekts

Sie können nicht erstellt, eine neue "Watch Lösung"... Sie können stattdessen eine Watch-App zu einer vorhandenen iOS-Anwendung hinzufügen. Um eine Watch-app erstellen, gehen Sie wie folgt vor:

1. Wenn Sie nicht über ein vorhandenes Projekt verfügen, wählen Sie zuerst **Datei > neue Projektmappe** , und erstellen Sie eine iOS-app (z. B. eine **Einzelansicht-App**):

    [![](installation-images/cycle8-2-sml.png "Wählen Sie die Datei > neue Projektmappe, und erstellen Sie eine iOS-app")](installation-images/cycle8-2.png#lightbox)

2. Sobald die iOS-app wird erstellt (oder Sie Ihre vorhandene iOS-app verwenden möchten), mit der rechten Maustaste auf die Projektmappe, und wählen Sie **hinzufügen > Neues Projekt hinzufügen...** . In der **neues Projekt** wählen Sie im Fenster **WatchOS > App > WatchKit-App**:

    [![](installation-images/cycle8-6-sml.png "Wählen Sie die WatchOS > App > WatchKit-App")](installation-images/cycle8-6.png#lightbox)

3. Im nächste Bildschirm können Sie auswählen, welche iOS-app-Projekt die Watch-app enthalten soll:

    [![](installation-images/cycle8-7-sml.png "Wählen Sie die iOS-app-Projekt die Watch-app enthalten soll")](installation-images/cycle8-7.png#lightbox)

4. Wählen Sie abschließend den Speicherort zum Speichern des Projekts (und optional die quellcodeverwaltung aktiviert):

    [![](installation-images/cycle8-8-sml.png "Wählen Sie den Speicherort zum Speichern des Projekts")](installation-images/cycle8-8.png#lightbox)

5. Visual Studio für Mac automatisch konfiguriert [Projektverweise und **"Info.plist"** Einstellungen](~/ios/watchos/get-started/project-references.md) für Sie.

## <a name="creating-the-watch-user-interface"></a>Erstellen der Benutzeroberfläche sehen Sie sich

<a name="designer" />

### <a name="using-the-xamarin-ios-designer"></a>Verwendung des Xamarin.IOS-Designers

Doppelklicken Sie auf der Watch-app **Interface.storyboard** so bearbeiten Sie die Verwendung des iOS Designers. Können Sie Schnittstellencontroller und UI-Steuerelemente in das Storyboard aus ziehen die **Toolbox** und konfigurieren sie mithilfe der **Eigenschaften** Pad:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![](installation-images/iosdesigner-sml.png "Das Storyboard im Designer")](installation-images/iosdesigner.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](installation-images/iosdesigner-sml-vs.png "Das Storyboard im Designer")](installation-images/iosdesigner-vs.png#lightbox)

-----

Erhalten Sie jeden neuen schnittstellencontrollers eine **Klasse** auswählen, und geben Sie den Namen in der **Eigenschaften** Pad (Dadurch wird die erforderliche erstellt C# Codebehind-Dateien automatisch):

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![](installation-images/iosdesigner-classname.png "Geben Sie jedes neue schnittstellencontrollers einer Klasse")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](installation-images/iosdesigner-classname-vs.png "Geben Sie jedes neue schnittstellencontrollers einer Klasse")

-----

Erstellen von segues **STRG + Ziehen** anhand eines Controllers "Schaltfläche", "Table" oder "Schnittstelle" auf eine andere Schnittstelle.


### <a name="using-xcode-on-the-mac"></a>Mithilfe von Xcode auf dem Mac

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Sie können weiterhin Xcode zu verwenden, um Ihre Benutzeroberfläche erstellen, indem Sie mit der rechten Maustaste auf die Datei "Interface.storyboard" und auswählen **Öffnen mit > Interface Builder von Xcode**:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio-Benutzer können auch Xcode verwenden, ihre Benutzeroberfläche zu erstellen, indem Sie direkt mit dem Mac-Buildhost wechseln.
Öffnen Sie die Projektmappe in Visual Studio für Mac mit der rechten Maustaste auf die Datei "Interface.storyboard" und wählen Sie **Öffnen mit > Interface Builder von Xcode**:

-----

![](installation-images/openwith-xcode.png "Öffnen Sie die Interface.storyboard in Xcode Interface Builder")

Wenn Xcode verwenden zu können, dann befolgen Sie die gleichen Schritte für die Watch-apps wie bei normalen [iOS-app-Storyboards](~/ios/user-interface/storyboards/index.md) (z. B. das Erstellen von Outlets und Aktionen von **STRG + Ziehen** in die **h**Headerdatei).

Beim Speichern des Storyboards in Interface Builder von Xcode werden automatisch hinzugefügt die Outlets und Aktionen, die Sie erstellen, die C# **. designer.cs** Dateien im Projekt Watch-Erweiterung.


### <a name="adding-additional-screens-in-xcode"></a>Hinzufügen weiterer Bildschirme in Xcode

Wenn Sie zusätzliche Bildschirme (hinausgeht in der Vorlage in der Standardeinstellung) hinzufügen, um das Storyboard mit Xcode Interface Builder **müssen Sie manuell hinzufügen der C# Codedateien** für jedes neue schnittstellencontrollers.

Finden Sie in der [erweiterter Anweisungen zum Hinzufügen von neuen Schnittstellencontroller auf ein Storyboard](~/ios/watchos/troubleshooting.md#add).

*Xamarin.IOS-Designer führt dies automatisch, es sind keine manuellen Schritte erforderlich.*


## <a name="building"></a>Erstellung

Ein Projekt, das eine Watch-app enthält, erstellt wie andere iOS-Projekte. Der Erstellungsprozess führt eine iPhone-Anwendung (. app), die eine Watch-Erweiterung (.appex), enthält das wiederum die Code weniger Watch-Anwendung (. app).


## <a name="launching"></a>Starten

Sie können die Watch-apps im Simulator mit der Visual Studio für Mac oder Visual Studio (er beginnt auf der Mac-Buildhost) starten.

Es gibt zwei Modi für die eine WatchKit-app starten:

 - normale app-Modus (Standard), und
- [Benachrichtigungen](~/ios/watchos/platform/notifications.md) (erfordert eine Test-Notification-Nutzlast im JSON-Format).

### <a name="xcode-8-support"></a>Xcode 8-Unterstützung

Wenn Xcode 8 (oder höher) installiert ist, sind Apple Watch-Simulatoren getrennt von iOS-Simulatoren (im Gegensatz zu [Xcode 6](#xcode6), bei dem diese enthalten, als ein *externe Anzeige*).
Die Simulator-Liste wird angezeigt, wenn Sie das Watch-App-Projekt wählen und sie das Startprojekt stellen, *iOS-Simulatoren* auswählen (wie unten gezeigt).

[![](installation-images/xs-xcode8-watchos3-sml.png "Den Simulator-Typ auswählen")](installation-images/xs-xcode8-watchos3.png#lightbox)

Beim Starten des Debuggens, *zwei* Simulatoren sollte gestartet werden: der iOS-Simulator *und* der Apple Watch-Simulator. Verwenden Sie **Befehl + Umschalt + H** Navigieren zu dem Menü "und" Clock Zifferblatt; und Verwenden der **Hardware** Menü Festlegen der **Force Touch Druck**. Scrollen auf dem Trackpad oder eine Maus simuliert mit der digitalen Crown.

#### <a name="troubleshooting"></a>Problembehandlung

Der folgende Fehler wird angezeigt, der **Anwendungsausgabe** Wenn Sie versuchen, die auf einem Simulator zu starten, die nicht über eine gekoppelte Watch verfügt:

```csharp
error MT0000: Unexpected error - Please file a bug report at https://github.com/xamarin/xamarin-macios/issues/new
error HE0020: Could not find a paired Watch device for the iOS device 'iPhone 6'.
```

Finden Sie unter [Apple Foren](https://forums.developer.apple.com/thread/7783) Anweisungen zum Konfigurieren der Simulatoren, wenn die Standardwerte nicht funktionieren.


<a name="xcode6" />

### <a name="xcode-6-and-watchos-1"></a>Xcode 6 und WatchOS 1

Sie müssen, die *sehen Sie sich das Projekt* der **Startprojekt** vor dem Ausführen oder Debuggen der app. Sie können nicht "start" die Watch-app selbst, und bei der Auswahl der iOS-app dann beginnt im iOS-Simulator wie gewohnt.

Standardmäßig wird eine Watch-app im normalen beginnt **app** Modus (Modus "nicht Blick" oder "Benachrichtigungen") aus Visual Studio für Mac **ausführen** oder **Debuggen** Befehle.

Bei Verwendung von Xcode 6, nur das iPhone 5, iPhone 5, iPhone 6 und iPhone 6 Plus kann aktivieren, die externe Anzeige entweder **Apple Watch - 38mm** oder **Apple Watch - 42mm** , sodass die Anwendungen überwachen können angezeigt.

**Hinweis:** Denken Sie daran, dass auf der Bildschirm sehen Sie sich im iOS-Simulator nicht automatisch angezeigt wird, wenn Sie Xcode 6 verwenden.
Verwenden der **Hardware > externe zeigt** Menü zeigt der Bildschirm sehen Sie sich an.

<a name="custommodes" />

## <a name="launching-notification-mode"></a>Starten der Benachrichtigungsmodus

Finden Sie in der [Seite "Benachrichtigungen"](~/ios/watchos/platform/notifications.md) Informationen wie Benachrichtigungen in Code behandelt.


Visual Studio für Mac kann die Watch-app starten, mit der Benachrichtigung _Start Modi_ für Benachrichtigungen:



Mit der rechten Maustaste auf das Watch-app-Projekt, und wählen Sie **ausführen mit > benutzerdefinierte Konfiguration...** :


[![](installation-images/runwith-customparams-sml.png "Ausführen einer benutzerdefinierten Konfigurations")](installation-images/runwith-customparams.png#lightbox)


Daraufhin wird die **benutzerdefinierte Parameter** Fenster, in dem Sie auswählen können **Benachrichtigung** (und geben Sie eine JSON-Nutzlast), drücken Sie dann die **ausführen** auf die Watch-app im Simulator zu starten:


[![](installation-images/runwith-execargs-sml.png "Festlegen der Benachrichtigung und die Nutzlast")](installation-images/runwith-execargs.png#lightbox)



## <a name="debugging"></a>Debuggen

Debuggen wird in Visual Studio für Mac und Visual Studio unterstützt.
Denken Sie daran, eine Benachrichtigung JSON-Datei angeben beim Debuggen im Modus für Benachrichtigungen. Dieser Screenshot zeigt einen Debughaltepunkt in eine Watch-app erreicht wird:

![](installation-images/debug-sml.png "Dieser Screenshot zeigt einen Haltepunkt erreicht wird, in eine Watch-app")

Nach dem Ausführen der Anweisungen zum Starten Sie werden letztlich die Watch-app unter der **iOS-Simulator (Überwachung)**.
Wählen Sie für den Benachrichtigungsmodus **Debuggen > System-Protokoll öffnen** (**CMD + /**) und `Console.WriteLine` in Ihrem Code.

### <a name="debugging-lifecycle-event-handlers"></a>Lebenszyklus-Ereignishandler Debuggen

<!--
To test the functionality in your  and 
    methods, use the **Hardware > Lock** command in the iOS Simulator.
    Locking will trigger the `DidDeactivate` method and the watch simulator
    will indicate that it has been locked. Swipe the iOS Simulator to unlock,
    which triggers the `WillActivate` method of the watch app.
-->

Die WatchOS-Vorlagendateien (z. B. `InterfaceController`, `ExtensionDelegate`, `NotificationController`, und `ComplicationController`) verfügen über die erforderlichen Lebenszyklusmethoden bereits implementiert. Hinzufügen `Console.WriteLine` Aufrufe und lesen die **Anwendungsausgabe** um Seitenereignis-Lebenszyklen besser zu verstehen.



## <a name="related-links"></a>Verwandte Links

- [WatchKitCatalog (Beispiel)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Erste Watch-App-video](http://blog.xamarin.com/your-first-watch-kit-app/)
- [Apple WatchKit-Tipps](https://developer.apple.com/watchkit/tips/)
