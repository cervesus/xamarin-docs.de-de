---
title: Installieren und Verwenden von WatchOS in Xamarin
description: Dieses Dokument beschreibt das Installieren und Verwenden von WatchOS mit Xamarin. Es wird erläutert, Installation, WatchOS-Projekt zu strukturieren, wie Sie mithilfe der iOS-Designer, die Xcode-Integration, und enthält Tipps zur Problembehandlung.
ms.prod: xamarin
ms.assetid: 69F21F15-198D-4B42-A703-21D35CAB0CCA
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 12/05/2017
ms.openlocfilehash: f986099011dbccb0eb43c62d253ee497d46ca08e
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2020
ms.locfileid: "78915475"
---
# <a name="installing-and-using-watchos-in-xamarin"></a>Installieren und Verwenden von WatchOS in Xamarin

WatchOS 4 ist MacOS Sierra (10.12) mit Xcode 9 erforderlich.

WatchOS 1 erforderlich ursprünglich OS X Yosemite (10.10) mit Xcode 7.

> [!WARNING]
> [watchos 1-Updates werden nach dem 1. April 2018 nicht mehr akzeptiert](https://developer.apple.com/news/?id=11162017a). Zukünftige Updates müssen verwenden WatchOS 2 SDK oder höher, Erstellen mit dem WatchOS empfiehlt 4 SDK.

## <a name="project-structure"></a>Projektstruktur

Eine Watch-app besteht aus drei Projekten:

- **Xamarin. IOS-iPhone-App-Projekt** : Hierbei handelt es sich um ein normales iPhone-Projekt, bei dem es sich um beliebige xamarin. IOS-Vorlagen handeln kann. Die Watch-App und ihre Erweiterung werden in diesem Projekt wichtigsten gebündelt werden.

- **Überwachungs Erweiterungsprojekt** : enthält den Code (z. b. Controller Klassen) für die Watch-app.

- **App-Projekt überwachen** : enthält die storyboarddatei für die Benutzeroberfläche mit allen Benutzeroberflächen Ressourcen für die Watch-app.

Die [Beispiel Projekt Mappe Watch Kit Catalog](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog) sieht in xamarin. Studio wie folgt aus:

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

![](installation-images/catalog-solution.png "The solution in Visual Studio")

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![](installation-images/catalog-solution-vs.png "The solution in Visual Studio")

-----

Laden Sie das [watchkitcatalog](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog) -Beispiel herunter, und führen Sie es aus.
Bildschirme aus dem Beispiel finden Sie auf der Seite "Steuer [Elemente](~/ios/watchos/user-interface/index.md) ".

## <a name="creating-a-new-project"></a>Erstellen eines neuen Projekts

Sie können nicht erstellt, eine neue "Watch Lösung"... Sie können stattdessen eine Watch-App zu einer vorhandenen iOS-Anwendung hinzufügen. Um eine Watch-app erstellen, gehen Sie wie folgt vor:

1. Wenn Sie nicht über ein vorhandenes Projekt verfügen, wählen Sie zunächst **Datei > neue Projekt Mappe** aus, und erstellen Sie eine IOS-app (z. b. eine **Einzelansicht-App**):

    [![](installation-images/cycle8-2-sml.png "Choose File > New Solution and create an iOS app")](installation-images/cycle8-2.png#lightbox)

2. Nachdem die IOS-App erstellt wurde (oder Sie die Verwendung Ihrer vorhandenen IOS-App planen), klicken Sie mit der rechten Maustaste auf die Projekt Mappe, und wählen Sie **Hinzufügen > Neues Projekt hinzufügen..** . Wählen Sie im Fenster **Neues Projekt** die Option **watchos > app > watchkit-App**aus:

    [![](installation-images/cycle8-6-sml.png "Select watchOS > App > WatchKit App")](installation-images/cycle8-6.png#lightbox)

3. Im nächste Bildschirm können Sie auswählen, welche iOS-app-Projekt die Watch-app enthalten soll:

    [![](installation-images/cycle8-7-sml.png "Choose which iOS app project should include the watch app")](installation-images/cycle8-7.png#lightbox)

4. Wählen Sie abschließend den Speicherort zum Speichern des Projekts (und optional die quellcodeverwaltung aktiviert):

    [![](installation-images/cycle8-8-sml.png "Choose the location to save the project")](installation-images/cycle8-8.png#lightbox)

5. Visual Studio für Mac automatisch [Projekt Verweise und Einstellungen für " **Info. plist** ](~/ios/watchos/get-started/project-references.md) " für Sie konfiguriert.

## <a name="creating-the-watch-user-interface"></a>Erstellen der Benutzeroberfläche sehen Sie sich

<a name="designer" />

### <a name="using-the-xamarin-ios-designer"></a>Verwendung des Xamarin.IOS-Designers

Doppelklicken Sie auf das **Interface. Storyboard** der Watch-APP, um mit dem IOS-Designer zu bearbeiten. Sie können Schnittstellen Controller und UI-Steuerelemente aus der **Toolbox** in das Storyboard ziehen und diese mit dem **eigenschaftenpad** konfigurieren:

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

[![](installation-images/iosdesigner-sml.png "The storyboard in the Designer")](installation-images/iosdesigner.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![](installation-images/iosdesigner-sml-vs.png "The storyboard in the Designer")](installation-images/iosdesigner-vs.png#lightbox)

-----

Sie sollten jedem neuen Schnittstellen Controller eine **Klasse** geben, indem Sie ihn auswählen und dann den Namen im **eigenschaftenpad** eingeben (Dadurch werden C# die erforderlichen Code Behind-Dateien automatisch erstellt):

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

![](installation-images/iosdesigner-classname.png "Give each new interface controller a Class")

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![](installation-images/iosdesigner-classname-vs.png "Give each new interface controller a Class")

-----

Erstellen Sie mithilfe von **STRG + Ziehen** von einer Schaltfläche, einem Tabellen-oder einem Schnittstellen Controller auf einen anderen Schnittstellen Controller.

### <a name="using-xcode-on-the-mac"></a>Mithilfe von Xcode auf dem Mac

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

Sie können weiterhin Xcode verwenden, um die Benutzeroberfläche zu erstellen, indem Sie mit der rechten Maustaste auf die Datei Interface. Storyboard klicken und dann **Öffnen mit > Xcode Interface Builder**auswählen:

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Visual Studio-Benutzer können auch Xcode verwenden, ihre Benutzeroberfläche zu erstellen, indem Sie direkt mit dem Mac-Buildhost wechseln.
Öffnen Sie die Projekt Mappe in Visual Studio für Mac, klicken Sie dann mit der rechten Maustaste auf die Datei Interface. Storyboard, und wählen Sie **Öffnen mit > Xcode Interface Builder**aus:

-----

![](installation-images/openwith-xcode.png "Open the Interface.storyboard in Xcode Interface Builder")

Wenn Sie Xcode verwenden, sollten Sie die gleichen Schritte wie bei der überwachen von Apps für normale [IOS-App-Storyboards](~/ios/user-interface/storyboards/index.md) ausführen (z. b. das Erstellen von Outlets und Aktionen durch **STRG + Ziehen** in die **. h** -Header Datei).

Wenn Sie das Storyboard in Xcode speichern Interface Builder werden die von C# Ihnen erstellten Outlets und Aktionen automatisch den **. Designer.cs** -Dateien im Überwachungs Erweiterungsprojekt hinzugefügt.

### <a name="adding-additional-screens-in-xcode"></a>Hinzufügen weiterer Bildschirme in Xcode

Wenn Sie dem Storyboard mithilfe von xInterface Builder Code weitere Bildschirme hinzufügen (außer der standardmäßig in der Vorlage enthalten), **müssen Sie die C# Code Dateien** für jeden neuen Schnittstellen Controller manuell hinzufügen.

Weitere Informationen finden Sie in den [erweiterten Anweisungen zum Hinzufügen neuer Schnittstellen Controller zu einem Storyboard](~/ios/watchos/troubleshooting.md#add).

*Der xamarin IOS-Designer führt dies automatisch aus. es sind keine manuellen Schritte erforderlich.*

## <a name="building"></a>Erstellen

Ein Projekt, das eine Watch-app enthält, erstellt wie andere iOS-Projekte. Der Erstellungsprozess führt eine iPhone-Anwendung (. app), die eine Watch-Erweiterung (.appex), enthält das wiederum die Code weniger Watch-Anwendung (. app).

## <a name="launching"></a>Starten

Sie können die Watch-apps im Simulator mit der Visual Studio für Mac oder Visual Studio (er beginnt auf der Mac-Buildhost) starten.

Es gibt zwei Modi für die eine WatchKit-app starten:

- normale app-Modus (Standard), und
- [Benachrichtigungen](~/ios/watchos/platform/notifications.md) (für die eine Test Benachrichtigungs Nutzlast im JSON-Format erforderlich ist).

### <a name="xcode-8-support"></a>Xcode 8-Unterstützung

Sobald Xcode 8 (oder höher) installiert ist, sind Apple Watch Simulatoren von IOS-Simulatoren getrennt (im Gegensatz zu [Xcode 6](#xcode6), wo Sie als *externe Anzeige angezeigt*werden).
Wenn Sie das App-Projekt überwachen auswählen und es als Startprojekt festlegen, werden in der simulatorliste *IOS-Simulatoren* angezeigt, aus denen Sie wählen können (siehe unten).

[![](installation-images/xs-xcode8-watchos3-sml.png "Selecting the Simulator type")](installation-images/xs-xcode8-watchos3.png#lightbox)

Wenn Sie mit dem Debuggen beginnen, sollten *zwei* Simulatoren gestartet werden: der IOS-Simulator *und* der Apple Watch Simulator. Verwenden Sie **Befehl + Umschalt + H** , um zum Menü "überwachen" und "Takt Gesicht" zu navigieren. Verwenden Sie das Menü **Hardware** , um den **Force Touch Druck**festzulegen. Scrollen auf dem Trackpad oder eine Maus simuliert mit der digitalen Crown.

#### <a name="troubleshooting"></a>Problembehandlung

Der folgende Fehler wird in der **Anwendungs Ausgabe** angezeigt, wenn Sie versuchen, in einem Simulator zu starten, der nicht über eine gekoppelte Überwachung verfügt:

```csharp
error MT0000: Unexpected error - Please file a bug report at https://github.com/xamarin/xamarin-macios/issues/new
error HE0020: Could not find a paired Watch device for the iOS device 'iPhone 6'.
```

Weitere Informationen zum Konfigurieren der Simulatoren finden Sie in den [Foren von Apple](https://forums.developer.apple.com/thread/7783) , wenn die Standardwerte nicht funktionieren.

<a name="xcode6" />

### <a name="xcode-6-and-watchos-1"></a>Xcode 6 und WatchOS 1

Sie müssen vor dem Ausführen oder Debuggen der APP das **Startprojekt** als Projekt für die *Überwachungs Erweiterung* festlegen. Sie können nicht "start" die Watch-app selbst, und bei der Auswahl der iOS-app dann beginnt im iOS-Simulator wie gewohnt.

Standardmäßig wird eine Watch-App im normalen **App** -Modus (nicht im Blick-oder Benachrichtigungs Modus) aus den Befehlen **Ausführen** oder **Debuggen** von Visual Studio für Mac gestartet.

Bei Verwendung von Xcode 6 kann nur das iPhone 5, iPhone 5S, iPhone 6 und iPhone 6 plus die externe Anzeige entweder für **Apple Watch-38mm** oder **Apple Watch-42mm** aktivieren, wo die Überwachungsanwendungen angezeigt werden.

> [!NOTE]
> Beachten Sie, dass der Bildschirm überwachen bei Verwendung von Xcode 6 nicht automatisch im IOS-Simulator angezeigt wird.
> Verwenden Sie das Menü **Hardware > externe Anzeige** , um den Bildschirm überwachen anzuzeigen.

<a name="custommodes" />

## <a name="launching-notification-mode"></a>Starten der Benachrichtigungsmodus

Informationen zum Behandeln von Benachrichtigungen im Code finden Sie auf der [Seite "Benachrichtigungen](~/ios/watchos/platform/notifications.md) ".

Visual Studio für Mac können die Watch-App mit einem Benachrichtigungs _Start Modus_ für Benachrichtigungen starten:

Klicken Sie mit der rechten Maustaste auf das App-Projekt ansehen, und wählen Sie **mit > benutzerdefinierte Konfiguration ausführen...** aus:

[![](installation-images/runwith-customparams-sml.png "Running a Custom Configuration")](installation-images/runwith-customparams.png#lightbox)

Dadurch wird das Fenster **benutzerdefinierte Parameter** geöffnet, in dem Sie die Option **Benachrichtigung** (und eine JSON-Nutzlast angeben) auswählen können. Klicken Sie anschließend auf **Ausführen** , um die Überwachungs-App im Simulator zu starten:

[![](installation-images/runwith-execargs-sml.png "Setting the Notification and Payload")](installation-images/runwith-execargs.png#lightbox)

## <a name="debugging"></a>Debuggen

Debuggen wird in Visual Studio für Mac und Visual Studio unterstützt.
Denken Sie daran, eine Benachrichtigung JSON-Datei angeben beim Debuggen im Modus für Benachrichtigungen. Dieser Screenshot zeigt einen Debughaltepunkt in eine Watch-app erreicht wird:

![](installation-images/debug-sml.png "This screenshot shows a debug breakpoint being hit in a watch app")

Nachdem Sie die Anweisungen für den Start ausgeführt haben, wird Ihre Watch-App auf dem **IOS-Simulator (Watch)** ausgeführt.
Für den Benachrichtigungs Modus können Sie **Debuggen > System Protokoll öffnen** (**cmd +/** ) auswählen und `Console.WriteLine` im Code verwenden.

### <a name="debugging-lifecycle-event-handlers"></a>Lebenszyklus-Ereignishandler Debuggen

<!--
To test the functionality in your  and 
  methods, use the **Hardware > Lock** command in the iOS Simulator.
  Locking will trigger the `DidDeactivate` method and the watch simulator
  will indicate that it has been locked. Swipe the iOS Simulator to unlock,
  which triggers the `WillActivate` method of the watch app.
-->

Die watchos-Vorlagen Dateien (z. b. `InterfaceController`, `ExtensionDelegate`, `NotificationController`und `ComplicationController`) enthalten die erforderlichen Lebenszyklus Methoden, die bereits implementiert sind. Fügen Sie `Console.WriteLine` Aufrufe hinzu, und lesen Sie die **Anwendungs Ausgabe** , um den Ereignis Lebenszyklus besser zu verstehen.

## <a name="related-links"></a>Verwandte Links

- [Watchkitcatalog (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Video zur ersten Watch-App](https://blog.xamarin.com/your-first-watch-kit-app/)
- [Watchkit-Tipps von Apple](https://developer.apple.com/watchkit/tips/)
