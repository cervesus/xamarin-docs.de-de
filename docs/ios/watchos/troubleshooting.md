---
title: WatchOS Problembehandlung
description: Bekannte Probleme und problemumgehungen bei der Verwendung Entwicklungsprobleme WatchOS.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 27C31DB8-451E-4888-BBC1-CE0DFC2F9DEC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 6e7a7dd09d65b88831136662d8718886aaf483c5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="watchos-troubleshooting"></a>WatchOS Problembehandlung

Diese Seite enthält zusätzliche Informationen und problemumgehungen für Features noch in Entwicklung. Einige der folgenden Vorgehensweisen gelten nur für unsere Preview-Versionen.

- [Bekannte Probleme](#knownissues)

- [Entfernen den Alpha-Kanal aus Symbolbilder](#noalpha)

- [Manuellen Hinzufügen von Schnittstelle Controller Dateien](#add) für Xcode-Schnittstelle-Generator.

- [Starten den WatchApp über die Befehlszeile](#command_line).

<a name="knownissues" />

## <a name="known-issues"></a>Bekannte Probleme

### <a name="general"></a>Allgemein

<a name="deploy" />

- Frühere Versionen von Visual Studio für Mac ist es falsch werden eines der der der **AppleCompanionSettings** Symbole als 88 x 88 Pixel; vortäuschen eine **Symbol Fehler fehlt** Wenn Sie versuchen, auf die App senden Speicher.
    Dieses Symbol muss 87 x 87 Pixel (29 Einheiten für **@3x** Retina Bildschirme). Sie können nicht in Visual Studio für Mac - entweder bearbeiten-Standardimage-Medienobjekt in Xcode beheben oder manuell bearbeiten, die **Contents.json** Datei (entsprechend [in diesem Beispiel](https://github.com/xamarin/monotouch-samples/blob/master/WatchKit/WatchKitCatalog/WatchApp/Resources/Images.xcassets/AppIcons.appiconset/Contents.json#L126-L132)).

- Wenn das Überwachungsfenster-Erweiterungsprojekt **"Info.plist" > WKApp Paket-ID** ist nicht [richtig festgelegt](~/ios/watchos/get-started/project-references.md) Watch-App entsprechend **Paket-ID**, der Debugger die Verbindung nicht und visuelle Studio für Mac mit der Nachricht warten *"Warte Debugger eine Verbindung herstellen"*.

- Debuggen wird in unterstützt **Benachrichtigungen** Modus kann jedoch nicht zuverlässig. Es funktioniert in einigen Fällen aus, und wiederholen Sie dann. Überprüfen Sie, ob die Watch-App **"Info.plist"** `WKCompanionAppBundleIdentifier` die Paket-ID der übergeordneten Container/iOS-app entsprechend festgelegt ist (ie. das Projekt, das auf dem iPhone ausgeführt wird).

- iOS-Designer zeigt keine Entrypoint Pfeile für Blick oder Benachrichtigung Schnittstelle-Controller.

- Kann nicht hinzugefügt werden, zwei `WKNotificationControllers` an einem Storyboard.
    Problemumgehung: Die `notificationCategory` Element in das Storyboard XML ist immer mit dem gleichen eingefügt `id`. Um dieses Problem umgehen, zwei (oder mehr) Benachrichtigung Controller hinzufügen, die Storyboarddatei in einem Text-Editor öffnen und dann manuell ändern, die `id` Element eindeutig sein.

    [![](troubleshooting-images/duplicate-id-sml.png "Öffnen das Storyboard in einem Text-Editor-Datei und ändern Sie manuell das Id-Element, um eindeutig sein.")](troubleshooting-images/duplicate-id.png#lightbox)

- Möglicherweise ein Fehler "die Anwendung verfügt über keine integrierte" beim Versuch, die app zu starten. Dieser Schritt erfolgt nach einem **Bereinigen** Wenn das Startup-Projekt auf das Erweiterungsprojekt Überwachung festgelegt ist.
    Die Korrektur besteht darin, wählen Sie **erstellen > Rebuild All** und starten Sie die app dann erneut.

### <a name="visual-studio"></a>Visual Studio

Die iOS-Designer zu unterstützen, für die Überwachung Kit *erfordert* die Projektmappe ordnungsgemäß konfiguriert werden. Wenn das Projekt verweist nicht festgelegt werden (finden Sie unter [Gewusst wie: Festlegen von verweisen](~/ios/watchos/get-started/project-references.md)) und dann auf die Entwurfsoberfläche nicht ordnungsgemäß funktionieren.

<a name="noalpha" />

## <a name="removing-the-alpha-channel-from-icon-images"></a>Entfernen den Alpha-Kanal aus Symbolbilder

Symbole dürfen nicht für einen alpha-Kanal (der alpha-Kanal definiert transparente Bereiche eines Bilds), andernfalls wird die app abgelehnt werden, während der Übermittlung an den App Store mit einem Fehler, die etwa wie folgt:

```csharp
Invalid Icon - The watch application '...watchkitextension.appex/WatchApp.app'
contains an icon file '...watchkitextension.appex/WatchApp.app/Icon-27.5@2x.png'
with an alpha channel. Icons should not have an alpha channel.
```

Es ist einfach, entfernen Sie den alpha-Kanal zur Verwendung von Mac OS X die **Vorschau** app:

1. Öffnen Sie das Symbolbild in **Vorschau** und wählen Sie dann **Datei > Exportieren**.

2. Daraufhin angezeigten Dialogfeld schließt eine **Alpha** Kontrollkästchen, wenn Sie ein alpha-Kanal vorhanden ist.

    ![](troubleshooting-images/remove-alpha-sml.png "Daraufhin angezeigten Dialogfeld schließt einen Alpha-Kontrollkästchen, wenn Sie ein alpha-Kanal vorhanden ist.")

3. *Untick* der **Alpha** Kontrollkästchen und **speichern** die Datei auf den richtigen Speicherort.

4. Das Symbolbild sollte jetzt Apple-validierungsüberprüfungen bestanden.


<a name="add" />

## <a name="manually-adding-interface-controller-files"></a>Manuelles Hinzufügen von Schnittstelle Controller Dateien

> [!IMPORTANT]
> Die Xamarin unterstützt WatchKit entwerfen Überwachungsfenster Storyboards in der iOS-Designers (Visual Studio für Mac und Visual Studio), die unten beschriebenen Schritte ist nicht notwendig. Einfach Benennen eines Controllers Schnittstelle eine Klasse in Visual Studio für Mac-Eigenschaften aufgefüllt, und die C#-Code-Dateien werden automatisch erstellt werden.


*Wenn* Xcode Schnittstelle-Generator verwenden, befolgen Sie diese Schritte zum Erstellen von neuen Schnittstelle Controller für die app überwachen und Synchronisierung mit Xcode aktivieren, damit, dass die Steckdosen und Aktionen in c# verfügbar sind:

1. Öffnen Sie der Watch-app **Interface.storyboard** in **Xcode Schnittstelle-Generator**.
    
    ![](troubleshooting-images/add-6.png "Öffnen das Storyboard im-Generator Xcode-Schnittstelle")

2. Ziehen Sie ein neues `InterfaceController` auf das Storyboard:

    ![](troubleshooting-images/add-1.png "A InterfaceController")

3. Sie können Steuerelemente auf dem Controller Schnittstelle (z. b. ziehen Sie jetzt Schaltflächen und Bezeichnungen) aber keine erstellen Steckdosen oder Aktionen noch, da es ist keine **h** Headerdatei. Die folgenden Schritte führt dazu, dass die erforderlichen **h** Header-Datei erstellt werden soll.

    ![](troubleshooting-images/add-2.png "Eine Schaltfläche im layout")

4. Schließen Sie das Storyboard und zurück zu Visual Studio für Mac. Erstellen Sie eine neue C#-Datei **MyInterfaceController.cs** (oder einen beliebigen Namen gewünscht) in der **Überwachen von app-Erweiterung** Projekt (nicht die Watch-app selbst, in denen das Storyboard ist). Fügen Sie den folgenden Code (aktualisieren den Namespace, Classname und den Namen des Konstruktors) hinzu:

        using System;
        using WatchKit;
        using Foundation;
        
        namespace WatchAppExtension  // remember to update this
        {
            public partial class MyInterfaceController // remember to update this
            : WKInterfaceController
            {
                public MyInterfaceController // remember to update this
                (IntPtr handle) : base (handle)
                {
                }
                public override void Awake (NSObject context)
                {
                    base.Awake (context);
                    // Configure interface objects here.
                    Console.WriteLine ("{0} awake with context", this);
                }
                public override void WillActivate ()
                {
                    // This method is called when the watch view controller is about to be visible to the user.
                    Console.WriteLine ("{0} will activate", this);
                }
                public override void DidDeactivate ()
                {
                    // This method is called when the watch view controller is no longer visible to the user.
                    Console.WriteLine ("{0} did deactivate", this);
                }
            }
        }

5. Erstellen Sie eine andere neue C#-Datei **MyInterfaceController.designer.cs** in der **Überwachen von app-Erweiterung** Projekt, und fügen Sie den folgenden Code hinzu. Achten Sie darauf, dass Sie den Namespace, der Klassenname aktualisieren und die `Register` Attribut:

    ```csharp
    using Foundation;
    using System.CodeDom.Compiler;
    
    namespace HelloWatchExtension  // remember to update this
    {
        [Register ("MyInterfaceController")] // remember to update this
        partial class MyInterfaceController  // remember to update this
        {
            void ReleaseDesignerOutlets ()
            {
            }
        }
    }
    ```
    
    Tipp: Sie können (optional) diese einen untergeordneten Knoten der ersten Datei durch ziehen es auf anderen C#-Datei in Visual Studio für Mac-Lösung mit Leerstellen auffüllen Datei vornehmen. Es wird dann wie folgt angezeigt:
    
    ![](troubleshooting-images/add-5.png "Die Lösung mit Leerstellen auffüllen")

6. Wählen Sie **erstellen > Build All** , damit Xcode-Synchronisierung die neue Klasse erkennt, (über die `Register` Attribut), die wir verwendet.

7. Öffnen Sie das Storyboard neu starten, indem Sie mit der rechten Maustaste auf die Überwachungsfenster app Storyboard-Datei, und wählen **Öffnen mit > Xcode Schnittstelle-Generator**:

    ![](troubleshooting-images/add-6.png "Öffnen das Storyboard im Schnittstelle-Generator")

8. Wählen Sie die neue Schnittstelle Controller, und weisen Sie ihm den Klassennamen, dem Sie z. B. oben definiert. `MyInterfaceController`
Wenn alles ordnungsgemäß funktioniert hat, es sollte angezeigt werden automatisch in die **Klasse:** Dropdown-Liste, und Sie können es von dort aus auswählen.

    ![](troubleshooting-images/add-4.png "Festlegen einer benutzerdefinierten Klasse")

9. Wählen Sie die **Assistant Editor** in Xcode (das Symbol mit zwei überlappende Kreise) anzeigen, sodass Sie das Storyboard und den Code Seite-an-Seite angezeigt werden:

    ![](troubleshooting-images/add-7.png "Das Symbolleistenelement der Assistant-Editor")

    Wenn der Fokus im Codebereich befindet, stellen Sie sicher sind sehen Sie sich die **h** Headerdatei und, wenn nicht mit der rechten Maustaste in der Breadcrumb-Leiste, und wählen Sie die richtige Datei (**MyInterfaceController.h**)

    ![](troubleshooting-images/add-8.png "Wählen Sie MyInterfaceController")

10. Sie können jetzt erstellen Steckdosen und Aktionen von **STRG + Ziehen** aus dem Storyboard in der **h** Headerdatei.

    ![](troubleshooting-images/add-9.png "Erstellen von Steckdosen und Aktionen")

    Beim Freigeben des Ziehvorgangs aufgefordert werden, wählen Sie, ob eine Steckdose oder eine Aktion erstellt, und wählen seinen Namen aus:

    ![](troubleshooting-images/add-a.png "Der Ausgang und eine Aktion-Dialogfeld")

11. Sobald die Storyboard-Änderungen gespeichert werden und Xcode geschlossen wird, zurück zu Visual Studio für Mac. Sie erkennt die Änderungen der Header-Datei und Code automatisch hinzufügen der **. designer.cs** Datei:


        [Register ("MyInterfaceController")]
        partial class MyInterfaceController
        {
            [Outlet]
            WatchKit.WKInterfaceButton myButton { get; set; }
        
            void ReleaseDesignerOutlets ()
            {
                if (myButton != null) {
                    myButton.Dispose ();
                    myButton = null;
                }
            }
        }


Sie können jetzt verweisen auf das Steuerelement (oder implementieren Sie die Aktion) in C# geschrieben.


<a name="command_line" />

## <a name="launching-the-watch-app-from-the-command-line"></a>Starten die Watch-App über die Befehlszeile

> [!IMPORTANT]
> Sie können die Watch-App im normalen app-Modus starten, standardmäßig sowie in **Blick** oder **Benachrichtigung** pullmodi unter Verwendung von [benutzerdefinierte Execution-Parametern](~/ios/watchos/get-started/installation.md#custommodes) in Visual Studio für Mac und Visual Studio.


Die Befehlszeile können auch um den iOS-Simulator zu steuern. Das Befehlszeilentool, mit dem Überwachen von apps gestartet ist **Mtouch**.

Hier ist ein vollständiges Beispiel (ausgeführt als eine einzelne Zeile in der Terminaldienste):

```bash
/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/bin/mtouch --sdkroot=/Applications/Xcode.app/Contents/Developer/ --device=:v2:runtime=com.apple.CoreSimulator.SimRuntime.iOS-8-2,devicetype=com.apple.CoreSimulator.SimDeviceType.iPhone-6
--launchsimwatch=/path/to/watchkitproject/watchsample/bin/iPhoneSimulator/Debug/watchsample.app
```

Der Parameter Sie entsprechend Ihrer app aktualisieren müssen ist `launchsimwatch`:

### <a name="--launchsimwatch"></a>--launchsimwatch

Den vollständigen Pfad zu dem Haupt-app-Bündel *der iOS-App, die den Watch-app und die Erweiterung enthält*.

> [!NOTE]
> Der Pfad, Sie angeben müssen, ist für die *iPhone .app Anwendungsdatei*, d. h. eine, die iOS-Simulator bereitgestellt wird und die Watch-Erweiterung und die Watch-app enthält.

Beispiel:

```bash
--launchsimwatch=/path/to/watchkitproject/watchsample/bin/iPhoneSimulator/Debug/watchsample.app
```


## <a name="notification-mode"></a>Benachrichtigungsmodus

So testen Sie die app [ **Benachrichtigung** Modus](~/ios/watchos/platform/notifications.md)legen die `watchlaunchmode` Parameter `Notification` , und geben Sie einen Pfad zu einer JSON-Datei, die eine Test-benachrichtigungsnutzlast enthält.

Der Parameter für die Nutzlast ist *erforderlichen* für Benachrichtigungsmodus.

Diese Argumente z. B. dem Mtouch-Befehl hinzufügen:

```bash
--watchlaunchmode=Notification --watchnotificationpayload=/path/to/file.json
```


## <a name="other-arguments"></a>Andere Argumente

Die übrigen Argumente werden im folgenden erläutert:

### <a name="--sdkroot"></a>--sdkroot

Erforderlich. Gibt den Pfad zum Xcode (6.2 oder höher).

Beispiel:

```bash
 --sdkroot /Applications/Xcode.app/Contents/Developer/
```

### <a name="--device"></a>--Gerät

Der Simulator-Gerät ausführen. Dies kann auf zwei Arten, entweder mithilfe der Udid eines bestimmten Geräts oder über eine Kombination von Common Language Runtime und Gerätetyp angegeben werden.

Die genauen Werte variiert zwischen Computern und können abgefragt werden, mithilfe des Apple **Simctl** Tool:

```bash
/Applications/Xcode.app/Contents/Developer/usr/bin/simctl list
```

**UDID**

Beispiel:

```bash
--device=:v2:udid=AAAAAAA-BBBB-CCCC-DDDD-EEEEEEEEEEEE
```

**Common Language Runtime und Gerätetyp**

Beispiel:

```bash
--device=:v2:runtime=com.apple.CoreSimulator.SimRuntime.iOS-8-2,devicetype=com.apple.CoreSimulator.SimDeviceType.iPhone-6
```



## <a name="related-links"></a>Verwandte Links

- [WatchKitCatalog (Beispiel)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [WatchTables (Beispiel)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchTables/)
