---
title: Installieren und Verwenden von watchos in xamarin
description: In diesem Dokument wird beschrieben, wie Sie watchos mit xamarin installieren und verwenden. Es wird die Installation, die watchos-Projektstruktur, die Verwendung des IOS-Designers, die Xcode-Integration und Tipps zur Problembehandlung erläutert.
ms.prod: xamarin
ms.assetid: 69F21F15-198D-4B42-A703-21D35CAB0CCA
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 12/05/2017
ms.openlocfilehash: 4cc321f44238a7b738e40c02656b42f1eda1155a
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86938761"
---
# <a name="installing-and-using-watchos-in-xamarin"></a>Installieren und Verwenden von watchos in xamarin

watchos 4 erfordert macOS Sierra (10,12) mit Xcode 9.

watchos 1 erforderte ursprünglich OS X Yosemite (10,10) mit Xcode 7.

> [!WARNING]
> [watchos 1-Updates werden nach dem 1. April 2018 nicht mehr akzeptiert](https://developer.apple.com/news/?id=11162017a). Für zukünftige Updates muss das watchos 2 SDK oder höher verwendet werden. Das entwickeln mit dem watchos 4 SDK wird empfohlen.

## <a name="project-structure"></a>Projektstruktur

Eine Watch-App besteht aus drei Projekten:

- **Xamarin. IOS-iPhone-App-Projekt** : Hierbei handelt es sich um ein normales iPhone-Projekt, bei dem es sich um beliebige xamarin. IOS-Vorlagen handeln kann. Die Watch-APP und ihre Erweiterung werden innerhalb dieses Hauptprojekts gebündelt.

- **Überwachungs Erweiterungsprojekt** : enthält den Code (z. b. Controller Klassen) für die Watch-app.

- **App-Projekt überwachen** : enthält die storyboarddatei für die Benutzeroberfläche mit allen Benutzeroberflächen Ressourcen für die Watch-app.

Die [Beispiel Projekt Mappe Watch Kit Catalog](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog) sieht in xamarin. Studio wie folgt aus:

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

![Die Projekt Mappe in Visual Studio](installation-images/catalog-solution.png)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![Die Projekt Mappe in Visual Studio](installation-images/catalog-solution-vs.png)

-----

Laden Sie das [watchkitcatalog](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog) -Beispiel herunter, und führen Sie es aus.
Bildschirme aus dem Beispiel finden Sie auf der Seite "Steuer [Elemente](~/ios/watchos/user-interface/index.md) ".

## <a name="creating-a-new-project"></a>Erstellen eines neuen Projekts

Sie können keine neue "Überwachungslösung" erstellen... Stattdessen können Sie eine Watch-APP zu einer vorhandenen IOS-Anwendung hinzufügen. Gehen Sie folgendermaßen vor, um eine Watch-APP zu erstellen:

1. Wenn Sie nicht über ein vorhandenes Projekt verfügen, wählen Sie zunächst **Datei > neue Projekt Mappe** aus, und erstellen Sie eine IOS-app (z. b. eine **Einzelansicht-App**):

    [![Wählen Sie Datei > neue Lösung aus, und erstellen Sie eine IOS-App](installation-images/cycle8-2-sml.png)](installation-images/cycle8-2.png#lightbox)

2. Nachdem die IOS-App erstellt wurde (oder Sie die Verwendung Ihrer vorhandenen IOS-App planen), klicken Sie mit der rechten Maustaste auf die Projekt Mappe, und wählen Sie **Hinzufügen > neues Projekt hinzufügen..**. Wählen Sie im Fenster **Neues Projekt** die Option **watchos > app > watchkit-App**aus:

    [![Auswählen der watchos-> App > watchkit-App](installation-images/cycle8-6-sml.png)](installation-images/cycle8-6.png#lightbox)

3. Im nächsten Bildschirm können Sie auswählen, welches IOS-App-Projekt die Watch-App enthalten soll:

    [![Auswählen, welches IOS-App-Projekt die Watch-App einschließen soll](installation-images/cycle8-7-sml.png)](installation-images/cycle8-7.png#lightbox)

4. Wählen Sie abschließend den Speicherort zum Speichern des Projekts (und optional die Quell Code Verwaltung) aus:

    [![Wählen Sie den Speicherort zum Speichern des Projekts aus.](installation-images/cycle8-8-sml.png)](installation-images/cycle8-8.png#lightbox)

5. Visual Studio für Mac automatisch [Projekt Verweise und Einstellungen für " **Info. plist** ](~/ios/watchos/get-started/project-references.md) " für Sie konfiguriert.

## <a name="creating-the-watch-user-interface"></a>Erstellen der Benutzeroberfläche für die Überwachung

<a name="designer"></a>

### <a name="using-the-xamarin-ios-designer"></a>Verwenden des xamarin IOS-Designers

Doppelklicken Sie auf das **Interface. Storyboard** der Watch-APP, um mit dem IOS-Designer zu bearbeiten. Sie können Schnittstellen Controller und UI-Steuerelemente aus der **Toolbox** in das Storyboard ziehen und diese mit dem **eigenschaftenpad** konfigurieren:

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

[![Das Storyboard im Designer](installation-images/iosdesigner-sml.png)](installation-images/iosdesigner.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Das Storyboard im Designer](installation-images/iosdesigner-sml-vs.png)](installation-images/iosdesigner-vs.png#lightbox)

-----

Sie sollten jedem neuen Schnittstellen Controller eine **Klasse** geben, indem Sie ihn auswählen und dann den Namen im **eigenschaftenpad** eingeben (Dadurch werden die erforderlichen c#-Code Behind-Dateien automatisch erstellt):

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

![Jedem neuen Schnittstellen Controller eine Klasse zuordnen](installation-images/iosdesigner-classname.png)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![Jedem neuen Schnittstellen Controller eine Klasse zuordnen](installation-images/iosdesigner-classname-vs.png)

-----

Erstellen Sie mithilfe von **STRG + Ziehen** von einer Schaltfläche, einem Tabellen-oder einem Schnittstellen Controller auf einen anderen Schnittstellen Controller.

### <a name="using-xcode-on-the-mac"></a>Verwenden von Xcode auf dem Mac

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

Sie können weiterhin Xcode verwenden, um die Benutzeroberfläche zu erstellen, indem Sie mit der rechten Maustaste auf die Datei Interface. Storyboard klicken und dann **Öffnen mit > Xcode Interface Builder**auswählen:

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Visual Studio-Benutzer können auch Xcode verwenden, um Ihre Benutzeroberfläche zu erstellen, indem Sie zur direkten Verwendung des Mac-buildhosts wechseln.
Öffnen Sie die Projekt Mappe in Visual Studio für Mac, klicken Sie dann mit der rechten Maustaste auf die Datei Interface. Storyboard, und wählen Sie **Öffnen mit > Xcode Interface Builder**aus:

-----

![Öffnen Sie das Interface. Storyboard in Xcode Interface Builder](installation-images/openwith-xcode.png)

Wenn Sie Xcode verwenden, sollten Sie die gleichen Schritte wie bei der überwachen von Apps für normale [IOS-App-Storyboards](~/ios/user-interface/storyboards/index.md) ausführen (z. b. das Erstellen von Outlets und Aktionen durch **STRG + Ziehen** in die **. h** -Header Datei).

Wenn Sie das Storyboard in Xcode speichern Interface Builder werden die von Ihnen erstellten Outlets und Aktionen automatisch den c# **. Designer.cs** -Dateien im Überwachungs Erweiterungsprojekt hinzugefügt.

### <a name="adding-additional-screens-in-xcode"></a>Hinzufügen zusätzlicher Bildschirme in Xcode

Wenn Sie dem Storyboard mithilfe von xInterface Builder Code weitere Bildschirme hinzufügen (außer der standardmäßig in der Vorlage enthalten), **müssen Sie die c#-Code Dateien** für jeden neuen Schnittstellen Controller manuell hinzufügen.

Weitere Informationen finden Sie in den [erweiterten Anweisungen zum Hinzufügen neuer Schnittstellen Controller zu einem Storyboard](~/ios/watchos/troubleshooting.md#add).

*Der xamarin IOS-Designer führt dies automatisch aus. es sind keine manuellen Schritte erforderlich.*

## <a name="building"></a>Erstellen

Ein Projekt, das eine Watch-APP wie andere IOS-Projekte enthält. Der erbauprozess führt zu einer iPhone-Anwendung (. app), die eine Watch-Erweiterung (. APPEX) enthält, die wiederum die Code lose Watch-Anwendung (. app) enthält.

## <a name="launching"></a>Starten

Sie können Watch-apps im Simulator entweder mithilfe von Visual Studio für Mac oder Visual Studio starten (startet auf dem Mac-buildhost).

Es gibt zwei Modi für das Starten einer watchkit-App:

- normaler APP-Modus (Standard) und
- [Benachrichtigungen](~/ios/watchos/platform/notifications.md) (für die eine Test Benachrichtigungs Nutzlast im JSON-Format erforderlich ist).

### <a name="xcode-8-support"></a>Xcode 8-Unterstützung

Sobald Xcode 8 (oder höher) installiert ist, sind Apple Watch Simulatoren von IOS-Simulatoren getrennt (im Gegensatz zu [Xcode 6](#xcode6), wo Sie als *externe Anzeige angezeigt*werden).
Wenn Sie das App-Projekt überwachen auswählen und es als Startprojekt festlegen, werden in der simulatorliste *IOS-Simulatoren* angezeigt, aus denen Sie wählen können (siehe unten).

[![Auswählen des simulatortyps](installation-images/xs-xcode8-watchos3-sml.png)](installation-images/xs-xcode8-watchos3.png#lightbox)

Wenn Sie mit dem Debuggen beginnen, sollten *zwei* Simulatoren gestartet werden: der IOS-Simulator *und* der Apple Watch Simulator. Verwenden Sie **Befehl + Umschalt + H** , um zum Menü "überwachen" und "Takt Gesicht" zu navigieren. Verwenden Sie das Menü **Hardware** , um den **Force Touch Druck**festzulegen. Der Bildlauf auf der Trackpad-oder Maus wird mithilfe des Digital Crown simuliert.

#### <a name="troubleshooting"></a>Problembehandlung

Der folgende Fehler wird in der **Anwendungs Ausgabe** angezeigt, wenn Sie versuchen, in einem Simulator zu starten, der nicht über eine gekoppelte Überwachung verfügt:

```csharp
error MT0000: Unexpected error - Please file a bug report at https://github.com/xamarin/xamarin-macios/issues/new
error HE0020: Could not find a paired Watch device for the iOS device 'iPhone 6'.
```

Weitere Informationen zum Konfigurieren der Simulatoren finden Sie in den [Foren von Apple](https://forums.developer.apple.com/thread/7783) , wenn die Standardwerte nicht funktionieren.

<a name="xcode6"></a>

### <a name="xcode-6-and-watchos-1"></a>Xcode 6 und watchos 1

Sie müssen vor dem Ausführen oder Debuggen der APP das **Startprojekt** als Projekt für die *Überwachungs Erweiterung* festlegen. Sie können die Watch-APP selbst nicht "starten", und wenn Sie die IOS-App auswählen, wird Sie im IOS-Simulator im normalen Zustand gestartet.

Standardmäßig wird eine Watch-App im normalen **App** -Modus (nicht im Blick-oder Benachrichtigungs Modus) aus den Befehlen **Ausführen** oder **Debuggen** von Visual Studio für Mac gestartet.

Bei Verwendung von Xcode 6 kann nur das iPhone 5, iPhone 5S, iPhone 6 und iPhone 6 plus die externe Anzeige entweder für **Apple Watch-38mm** oder **Apple Watch-42mm** aktivieren, wo die Überwachungsanwendungen angezeigt werden.

> [!NOTE]
> Beachten Sie, dass der Bildschirm überwachen bei Verwendung von Xcode 6 nicht automatisch im IOS-Simulator angezeigt wird.
> Verwenden Sie das Menü **Hardware > externe Anzeige** , um den Bildschirm überwachen anzuzeigen.

<a name="custommodes"></a>

## <a name="launching-notification-mode"></a>Starten des Benachrichtigungs Modus

Informationen zum Behandeln von Benachrichtigungen im Code finden Sie auf der [Seite "Benachrichtigungen](~/ios/watchos/platform/notifications.md) ".

Visual Studio für Mac können die Watch-App mit einem Benachrichtigungs _Start Modus_ für Benachrichtigungen starten:

Klicken Sie mit der rechten Maustaste auf das App-Projekt ansehen, und wählen Sie **mit > benutzerdefinierte Konfiguration ausführen...** aus:

[![Ausführen einer benutzerdefinierten Konfiguration](installation-images/runwith-customparams-sml.png)](installation-images/runwith-customparams.png#lightbox)

Dadurch wird das Fenster **benutzerdefinierte Parameter** geöffnet, in dem Sie die Option **Benachrichtigung** (und eine JSON-Nutzlast angeben) auswählen können. Klicken Sie anschließend auf **Ausführen** , um die Überwachungs-App im Simulator zu starten:

[![Festlegen der Benachrichtigung und der Nutzlast](installation-images/runwith-execargs-sml.png)](installation-images/runwith-execargs.png#lightbox)

## <a name="debugging"></a>Debuggen

Debuggen wird sowohl in Visual Studio für Mac als auch in Visual Studio unterstützt.
Stellen Sie beim Debuggen im Benachrichtigungs Modus eine JSON-Benachrichtigungs Datei bereit. Dieser Screenshot zeigt, wie ein debugbreakpoint in einer Watch-App angezeigt wird:

![Dieser Screenshot zeigt, wie ein debugbreakpoint in einer Watch-App getroffen wird.](installation-images/debug-sml.png)

Nachdem Sie die Anweisungen für den Start ausgeführt haben, wird Ihre Watch-App auf dem **IOS-Simulator (Watch)** ausgeführt.
Für den Benachrichtigungs Modus können Sie **Debuggen > System Protokoll öffnen** (**cmd +/**) auswählen und `Console.WriteLine` in Ihrem Code verwenden.

### <a name="debugging-lifecycle-event-handlers"></a>Debugger-Ereignishandler

<!--
To test the functionality in your  and 
  methods, use the **Hardware > Lock** command in the iOS Simulator.
  Locking will trigger the `DidDeactivate` method and the watch simulator
  will indicate that it has been locked. Swipe the iOS Simulator to unlock,
  which triggers the `WillActivate` method of the watch app.
-->

Die watchos-Vorlagen Dateien (z `InterfaceController` `ExtensionDelegate` . b.,, und) enthalten die `NotificationController` `ComplicationController` erforderlichen Lebenszyklus Methoden, die bereits implementiert sind. Fügen Sie `Console.WriteLine` Aufrufe hinzu, und lesen Sie die **Anwendungs Ausgabe** , um den Ereignis Lebenszyklus besser zu verstehen

## <a name="related-links"></a>Verwandte Links

- [Watchkitcatalog (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Video zur ersten Watch-App](https://blog.xamarin.com/your-first-watch-kit-app/)
- [Watchkit-Tipps von Apple](https://developer.apple.com/watchkit/tips/)
