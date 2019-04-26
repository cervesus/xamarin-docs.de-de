---
title: WatchOS Problembehandlung
description: Dieses Dokument beschreibt die bekannten Probleme und problemumgehungen für WatchOS-Entwicklung mit Xamarin. Es wird beschrieben, Bilder mit Problemen, manuellen Hinzufügen von Schnittstelle-Controller-Dateien, eine Watch-app über die Befehlszeile starten und vieles mehr.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 27C31DB8-451E-4888-BBC1-CE0DFC2F9DEC
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 70ef341c066c77e214761d75c173faef00266e4c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "60892759"
---
# <a name="watchos-troubleshooting"></a>WatchOS Problembehandlung

Diese Seite enthält zusätzliche Informationen und problemumgehungen für Probleme, die auftreten können.

- [Bekannte Probleme](#knownissues)

- [Der Alpha-Kanal entfernen aus Symbolbilder](#noalpha)

- [Manuellen Hinzufügen von Dateien für die Clientschnittstelle Controller](#add) für Xcode Interface Builder.

- [Starten den WatchApp über die Befehlszeile](#command_line).

<a name="knownissues" />

## <a name="known-issues"></a>Bekannte Probleme

### <a name="general"></a>Allgemein

<a name="deploy" />

- Frühere Versionen von Visual Studio für Mac nicht ordnungsgemäß angezeigt der **AppleCompanionSettings** Symbole als 88 x 88 Pixel; Dies führt in eine **fehlt das Symbol Fehler** Wenn Sie versuchen, die an den App Store zu übermitteln.
    Dieses Symbol muss 87 x 87-Pixeln (29 Einheiten für **@3x** Retina-Bildschirme). Sie beheben dies in Visual Studio für Mac – entweder Bearbeiten der Bildobjekt im Xcode oder manuell bearbeiten, können nicht die **Contents.json** Datei (entsprechend [in diesem Beispiel](https://github.com/xamarin/monotouch-samples/blob/master/WatchKit/WatchKitCatalog/WatchApp/Resources/Images.xcassets/AppIcons.appiconset/Contents.json#L126-L132)).

- Wenn der Watch-Erweiterung des Projekts **"Info.plist" > WKApp-Bundle-ID** nicht [richtig festgelegt](~/ios/watchos/get-started/project-references.md) entsprechend der Watch-App **Bündel-ID**, der Debugger wird keine Verbindung herstellen und Visual Studio für Mac mit der Meldung wartet *"Debugger eine Verbindung herstellt wird gewartet"*.

- Debuggen wird in unterstützt **Benachrichtigungen** Modus jedoch nicht immer zuverlässig ist. Erneuter Versuch wird in einigen Fällen funktionieren. Überprüfen Sie, ob der Watch-App **"Info.plist"** `WKCompanionAppBundleIdentifier` festgelegt ist, mit der Bündel-ID der übergeordneten/Container-app für iOS (d. h. diejenige, die auf dem iPhone ausgeführt wird).

- iOS-Designer zeigt keine Entrypoint Pfeile für Blick oder Benachrichtigung Schnittstellencontroller.

- Sie können keine zwei hinzufügen `WKNotificationControllers` auf ein Storyboard.
    Problemumgehung: Die `notificationCategory` Element im Storyboard XML wird immer mit dem gleichen eingefügt `id`. Um dieses Problem umgehen, zwei (oder mehr)-Notification-Controller hinzufügen, die Storyboard-Datei in einem Text-Editor zu öffnen und ändern Sie dann manuell, den `id` Element eindeutig sein.

    [![](troubleshooting-images/duplicate-id-sml.png "Öffnen das Storyboard-Datei in einem Text-Editor, und ändern Sie manuell das Id-Element eindeutig sein muss")](troubleshooting-images/duplicate-id.png#lightbox)

- Sie wird eine Fehlermeldung angezeigt "die Anwendung wurde nicht erstellt" beim Versuch, die app zu starten. Dieser Schritt erfolgt nach einem **Bereinigen** Wenn Startprojekt auf das Projekt der Überwachung festgelegt ist.
    Wählen Sie das Problem wird **erstellen > alle neu erstellen** und klicken Sie dann die app neu starten.

### <a name="visual-studio"></a>Visual Studio

Der iOS-Designer-Unterstützung für Überwachung Kit *erfordert* die Lösung ordnungsgemäß konfiguriert werden. Wenn die Projektverweise nicht festgelegt werden (finden Sie unter [Gewusst wie: Festlegen von verweisen](~/ios/watchos/get-started/project-references.md)) und dann auf die Entwurfsoberfläche nicht ordnungsgemäß funktionieren.

<a name="noalpha" />

## <a name="removing-the-alpha-channel-from-icon-images"></a>Der Alpha-Kanal entfernen aus Symbolbilder

Symbole dürfen keine alpha-Kanal (der Alphakanal definiert transparente Bereiche eines Bildes) enthalten, andernfalls werden die app während der App-Store-Übermittlung mit einem Fehler, die etwa wie folgt abgelehnt:

```csharp
Invalid Icon - The watch application '...watchkitextension.appex/WatchApp.app'
contains an icon file '...watchkitextension.appex/WatchApp.app/Icon-27.5@2x.png'
with an alpha channel. Icons should not have an alpha channel.
```

Es ist einfach, den alpha-Kanal auf Mac OS X verwenden, entfernen die **Vorschau** app:

1. Öffnen Sie das Symbolbild in **Vorschau** und wählen Sie dann **Datei > Export**.

2. Das angezeigte Dialogfeld umfasst eine **Alpha** Kontrollkästchen, wenn ein Alphakanal vorhanden ist.

    ![](troubleshooting-images/remove-alpha-sml.png "Daraufhin angezeigten Dialogfeld umfasst einen Alpha-Kontrollkästchen, wenn ein Alphakanal vorhanden ist")

3. *Untick* der **Alpha** Kontrollkästchen und **speichern** die Datei auf den richtigen Speicherort.

4. Das Symbolbild sollte Überprüfungen von Apple jetzt erfolgreich sein.


<a name="add" />

## <a name="manually-adding-interface-controller-files"></a>Interface-Controller-Dateien manuell hinzufügen

> [!IMPORTANT]
> WatchKit-Unterstützung für Xamarin enthält, sehen Sie sich Storyboards in der iOS-Designer (in Visual Studio für Mac und Visual Studio), der nicht über die unten beschriebenen Schritte erfordert entwerfen. Einfach benennen einem schnittstellencontrollers Klasse in Visual Studio für Mac-Eigenschaften aufzufüllen und die C# Codedateien werden automatisch erstellt.


*Wenn* Sie Interface Builder von Xcode verwenden, gehen Sie zum Erstellen von neuen Schnittstellencontroller für die Watch-app, und Aktivieren der Synchronisierung mit Xcode, damit die Outlets und Aktionen in verfügbaren C#:

1. Öffnen Sie der Watch-app **Interface.storyboard** in **Xcode Interface Builder**.
    
    ![](troubleshooting-images/add-6.png "Öffnen das Storyboard in Xcode Interface Builder")

2. Ziehen Sie ein neues `InterfaceController` auf das Storyboard:

    ![](troubleshooting-images/add-1.png "Eine InterfaceController")

3. Sie können Steuerelemente auf der Benutzeroberfläche (z. b. ziehen Sie jetzt Beschriftungen und Schaltflächen) jedoch keine erstellen Outlets oder Aktionen, da gibt es keine **h** Headerdatei. Die folgenden Schritte führt dazu, dass die erforderlichen **h** Header-Datei erstellt werden.

    ![](troubleshooting-images/add-2.png "Eine Schaltfläche im layout")

4. Das Storyboard schließen und zurück zu Visual Studio für Mac. Erstellen Sie ein neues C# Datei **MyInterfaceController.cs** (oder einen beliebigen Namen gewünscht) in der **sehen Sie sich app-Erweiterung** Projekt (nicht die Watch-app selbst, in denen das Storyboard ist). Fügen Sie den folgenden Code (aktualisieren den Namespace, Klassennamen und den Namen des Konstruktors) hinzu:

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

5. Erstellen Sie eine weitere neue C# Datei **MyInterfaceController.designer.cs** in die **sehen Sie sich app-Erweiterung** Projekt, und fügen Sie den folgenden Code hinzu. Achten Sie darauf, dass Sie zum Aktualisieren des Namespace, den Klassennamen und die `Register` Attribut:

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
    
    Tipp: Sie können (optional) machen diese einen untergeordneten Knoten der ersten Datei durch ziehen es auf die andere Datei C# -Datei in Visual Studio für Mac Lösungspad. Es wird dann wie folgt angezeigt:
    
    ![](troubleshooting-images/add-5.png "Das Pad \"Projektmappe\"")

6. Wählen Sie **erstellen > alle erstellen** so, dass Xcode-Synchronisierung die neue Klasse erkannt wird (über die `Register` Attribut), die wir verwendet.

7. Öffnen Sie das Storyboard erneut, indem Sie mit der rechten Maustaste auf die Watch-app-Storyboard-Datei und auswählen **Öffnen mit > Interface Builder von Xcode**:

    ![](troubleshooting-images/add-6.png "Öffnen das Storyboard in Interface Builder")

8. Wählen Sie Ihren neuen Schnittstelle-Controller, und geben sie den Klassennamen, die, dem Sie z. B. oben definiert. `MyInterfaceController`.
Wenn alles ordnungsgemäß funktioniert hat, es sollte angezeigt werden automatisch in die **Klasse:** Dropdown-Liste, und Sie können es auswählen, von dort aus.

    ![](troubleshooting-images/add-4.png "Festlegen einer benutzerdefinierten Klasse")

9. Wählen Sie die **Assistant Editor** anzeigen in Xcode (das Symbol mit zwei überlappende Kreise), sodass Sie das Storyboard und den Code Seite-an-Seite anzeigen können:

    ![](troubleshooting-images/add-7.png "Das Symbolleistenelement der Assistenten-Editor")

    Wenn der Fokus im Code-Bereich befindet, stellen Sie sicher sind sehen Sie sich die **h** Header-Datei, und wenn Sie nicht mit der rechten Maustaste in der Breadcrumb-Leiste, und wählen die richtige Datei (**MyInterfaceController.h**)

    ![](troubleshooting-images/add-8.png "Wählen Sie MyInterfaceController")

10. Sie können jetzt erstellen Outlets und Aktionen von **STRG + Ziehen** aus dem Storyboard in der **h** Headerdatei.

    ![](troubleshooting-images/add-9.png "Erstellen von Outlets und Aktionen")

    Wenn Sie für den Ziehvorgang freigeben, Sie werden aufgefordert, Sie wählen, ob zum Erstellen eines outlets oder eine Aktion, und wählen den Namen:

    ![](troubleshooting-images/add-a.png "Der Ausgang und eine Aktion-Dialogfeld")

11. Nachdem die Storyboard-Änderungen gespeichert werden und Xcode geschlossen ist, zurück zu Visual Studio für Mac. Sie erkennt die Änderungen des Header-Datei und fügen Sie Code automatisch die **. designer.cs** Datei:


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


Sie können jetzt das Steuerelement zu verweisen (oder implementieren Sie die Aktion) in C#!


<a name="command_line" />

## <a name="launching-the-watch-app-from-the-command-line"></a>Beim Starten der App sehen Sie sich über die Befehlszeile

> [!IMPORTANT]
> Sie können die Watch-App in normalen app-Modus starten, werden standardmäßig und auch im **Blick** oder **Benachrichtigung** pullmodi unter Verwendung von [benutzerdefinierte Execution-Parametern](~/ios/watchos/get-started/installation.md#custommodes) in Visual Studio für Mac und Visual Studio.


Die Befehlszeile können auch um die iOS-Simulator zu steuern. Ist das Befehlszeilentool verwendet, um Watch-apps starten **Mtouch**.

Hier ist ein vollständiges Beispiel (ausgeführt als einzelne Zeile im Terminal):

```bash
/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/bin/mtouch --sdkroot=/Applications/Xcode.app/Contents/Developer/ --device=:v2:runtime=com.apple.CoreSimulator.SimRuntime.iOS-8-2,devicetype=com.apple.CoreSimulator.SimDeviceType.iPhone-6
--launchsimwatch=/path/to/watchkitproject/watchsample/bin/iPhoneSimulator/Debug/watchsample.app
```

Der Parameter Sie Ihre app entsprechend aktualisieren müssen `launchsimwatch`:

### <a name="--launchsimwatch"></a>--launchsimwatch

Der vollständige Pfad zu dem Haupt-app-Bündel *für die iOS-app, die der Watch-app und die Erweiterung enthält*.

> [!NOTE]
> Der Pfad, Sie angeben müssen, ist für die *iPhone-App-Anwendungsdatei*, d. h. diejenige, die in der iOS-Simulator bereitgestellt und enthält sowohl die Watch-app die Watch-Erweiterung.

Beispiel:

```bash
--launchsimwatch=/path/to/watchkitproject/watchsample/bin/iPhoneSimulator/Debug/watchsample.app
```


## <a name="notification-mode"></a>Benachrichtigungsmodus

Zum Testen der app [ **Benachrichtigung** Modus](~/ios/watchos/platform/notifications.md)legen die `watchlaunchmode` Parameter `Notification` , und geben Sie einen Pfad zu einer JSON-Datei, die einen Test benachrichtigungsnutzlast enthält.

Der Parameter für die Nutzlast ist *erforderlichen* für den Benachrichtigungs-Modus.

Fügen Sie beispielsweise diese Argumente an die Mtouch-Befehl hinzu:

```bash
--watchlaunchmode=Notification --watchnotificationpayload=/path/to/file.json
```


## <a name="other-arguments"></a>Weitere Argumente

Die übrigen Argumente werden im folgenden erläutert:

### <a name="--sdkroot"></a>--sdkroot

Erforderlich. Gibt den Pfad zum Xcode (6.2 oder höher).

Beispiel:

```bash
 --sdkroot /Applications/Xcode.app/Contents/Developer/
```

### <a name="--device"></a>--Gerät

Der Simulator-Gerät ausgeführt werden soll. Dies kann auf zwei Arten, mit der benutzerdefinierten ein bestimmtes Gerät, oder eine Kombination aus Runtime und Gerätetyp angegeben werden.

Die genauen Werte variiert zwischen Computern und können abgefragt werden, mithilfe von Apple **Simctl** Tool:

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
