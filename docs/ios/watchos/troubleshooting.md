---
title: Problembehandlung bei watchos
description: In diesem Dokument werden bekannte Probleme und Problem Umgehungen für die watchos-Entwicklung mit xamarin erläutert. Es beschreibt Bilder mit Problemen, Manuelles Hinzufügen von Schnittstellen Controller Dateien, Starten einer Watch-App über die Befehlszeile und mehr.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 27C31DB8-451E-4888-BBC1-CE0DFC2F9DEC
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 06524163fadc4300d55ec90f35723fd1561bb8a0
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2020
ms.locfileid: "78915594"
---
# <a name="watchos-troubleshooting"></a>Problembehandlung bei watchos

Diese Seite enthält weitere Informationen und Problem Umgehungen für Probleme, auf die Sie möglicherweise stoßen.

- [Bekannte Probleme](#knownissues)

- [Entfernen des Alpha Kanals aus Symbolbildern](#noalpha)

- [Manuelles Hinzufügen von Schnittstellen Controller Dateien](#add) für Xcode-Interface Builder.

- [Starten der watchapp über die Befehlszeile](#command_line).

<a name="knownissues" />

## <a name="known-issues"></a>Bekannte Probleme

### <a name="general"></a>Allgemein

<a name="deploy" />

- In früheren Versionen von Visual Studio für Mac fälschlicherweise eines der **applecompanionsettings** -Symbole als 88x88 Pixel angezeigt. Dies führt zu einem **fehlenden symbolfehler** , wenn Sie versuchen, eine Übermittlung an den App Store durchführen.
    Dieses Symbol sollte 87x 87 Pixel (29 Einheiten für **@3x** Retina-Bildschirme) sein. Dies kann in Visual Studio für Mac nicht behoben werden. Bearbeiten Sie entweder das Image-Asset in Xcode, oder bearbeiten Sie die Datei " **Content. JSON** " manuell.

- Wenn die **Info. plist** -Datei des Watch-Erweiterungsprojekts > die wkapp-Bundle-ID nicht ordnungsgemäß für die **Paket-ID**der Überwachungsanwendung [festgelegt](~/ios/watchos/get-started/project-references.md) ist, kann der Debugger keine Verbindung herstellen, und Visual Studio für Mac wartet mit der Meldung "auf die Verbindung des *Debuggers wird gewartet"* .

- Debugging wird im **Benachrichtigungs** Modus unterstützt, kann jedoch unzuverlässig sein. Der Wiederholungsversuch funktioniert manchmal. Vergewissern Sie sich, dass die **Info. plist** -`WKCompanionAppBundleIdentifier` der Watch-APP so festgelegt ist, dass Sie mit der Bündel-ID der übergeordneten IOS-/Container-app (d. h. der Anwendung auf dem iPhone)

- der IOS-Designer zeigt keine EntryPoint-Pfeile für einen Blick oder Benachrichtigungs Schnittstellen Controller an.

- Sie können einem Storyboard nicht zwei `WKNotificationControllers` hinzufügen.
    Problem Umgehung: das `notificationCategory`-Element in der Storyboard-XML-Datei wird immer mit dem gleichen `id`eingefügt. Um dieses Problem zu umgehen, können Sie zwei (oder mehr) Benachrichtigungs Controller hinzufügen, die storyboarddatei in einem Text-Editor öffnen und dann das `id` Element manuell ändern, damit es eindeutig ist.

    [![](troubleshooting-images/duplicate-id-sml.png "Opening the storyboard file in a text editor and manually change the id element to be unique")](troubleshooting-images/duplicate-id.png#lightbox)

- Wenn Sie versuchen, die APP zu starten, wird möglicherweise die Fehlermeldung "die Anwendung wurde nicht erstellt" angezeigt. Dies tritt nach einer **Bereinigung** auf, wenn das Startprojekt auf das Überwachungs Erweiterungsprojekt festgelegt ist.
    Die Lösung besteht darin, den **Build auszuwählen > alle neu zu erstellen** und dann die APP neu zu starten.

### <a name="visual-studio"></a>Visual Studio

Die IOS Designer-Unterstützung für Watch Kit *erfordert* , dass die Projekt Mappe ordnungsgemäß konfiguriert wird. Wenn die Projekt Verweise nicht festgelegt werden (siehe [Festlegen von verweisen](~/ios/watchos/get-started/project-references.md)), wird die Entwurfs Oberfläche nicht ordnungsgemäß ausgeführt.

<a name="noalpha" />

## <a name="removing-the-alpha-channel-from-icon-images"></a>Entfernen des Alpha Kanals aus Symbolbildern

Symbole dürfen keinen Alphakanal enthalten (der Alphakanal definiert transparente Bereiche eines Bilds), andernfalls wird die APP während der Übermittlung des App-Stores mit einem Fehler ähnlich dem folgenden abgelehnt:

```csharp
Invalid Icon - The watch application '...watchkitextension.appex/WatchApp.app'
contains an icon file '...watchkitextension.appex/WatchApp.app/Icon-27.5@2x.png'
with an alpha channel. Icons should not have an alpha channel.
```

Es ist ganz einfach, den Alphakanal auf Mac OS X mithilfe der **Vorschau** -APP zu entfernen:

1. Öffnen Sie das Symbolbild in der **Vorschau** , und wählen Sie dann **Datei > Exportieren**aus.

2. Das Dialogfeld, das angezeigt wird, enthält ein **Alpha** -Kontrollkästchen, wenn ein Alphakanal vorhanden ist.

    ![](troubleshooting-images/remove-alpha-sml.png "The dialog that appears will include an Alpha checkbox if an alpha channel is present")

3. *Deaktivieren* Sie das Kontrollkästchen **Alpha** , und **Speichern** Sie die Datei am richtigen Speicherort.

4. Das Symbolbild sollte nun die Validierungs Überprüfungen von Apple bestehen.

<a name="add" />

## <a name="manually-adding-interface-controller-files"></a>Manuelles Hinzufügen von Schnittstellen Controller Dateien

> [!IMPORTANT]
> Die watchkit-Unterstützung von xamarin umfasst das Entwerfen von Watch-Storyboards im IOS-Designer (sowohl in Visual Studio für Mac als auch in Visual Studio), für die die unten aufgeführten Schritte nicht erforderlich sind. Geben Sie einem Schnittstellen Controller einfach einen Klassennamen im Visual Studio für Mac Properties-Pad, C# und die Code Dateien werden automatisch erstellt.

*Wenn* Sie Xcode-Interface Builder verwenden, führen Sie die folgenden Schritte aus, um neue Schnittstellen Controller für Ihre Watch-APP zu erstellen und die Synchronisierung mit Xcode zu aktivieren C#, damit die Outlets und Aktionen in verfügbar sind:

1. Öffnen Sie das **Interface. Storyboard** der Watch-app in **Xcode Interface Builder**.

    ![](troubleshooting-images/add-6.png "Opening the storyboard in Xcode Interface Builder")

2. Ziehen Sie eine neue `InterfaceController` auf das Storyboard:

    ![](troubleshooting-images/add-1.png "A InterfaceController")

3. Nun können Sie Steuerelemente auf den Schnittstellen Controller ziehen (z. b. Bezeichnungen und Schaltflächen) Sie können jedoch noch keine Outlets oder Aktionen erstellen, da keine **h** -Header Datei vorhanden ist. Die folgenden Schritte bewirken, dass die erforderliche **. h** -Header Datei erstellt wird.

    ![](troubleshooting-images/add-2.png "A button in the layout")

4. Schließen Sie das Storyboard, und kehren Sie zu Visual Studio für Mac zurück. Erstellen Sie eine C# neue Datei **MyInterfaceController.cs** (oder einen beliebigen Namen) im **App-Erweiterungs** Projekt (nicht in der Watch-APP selbst, in der sich das Storyboard befindet). Fügen Sie den folgenden Code hinzu (Aktualisieren von Namespace, Klassenname und Konstruktorname):

    ```csharp
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
    ```

5. Erstellen Sie eine C# weitere neue Datei **MyInterfaceController.Designer.cs** im **App-Erweiterungs** Projekt, und fügen Sie den folgenden Code hinzu. Achten Sie darauf, dass Sie den Namespace, den Klassennamen und das `Register` Attribut aktualisieren:

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

    > [!TIP]
    > Sie können diese Datei (optional) zu einem untergeordneten Knoten der ersten Datei machen, indem Sie Sie auf C# die andere Datei in der Visual Studio für Mac Lösungspad ziehen. Diese wird dann wie folgt angezeigt:

    ![](troubleshooting-images/add-5.png "The Solution pad")

6. Wählen Sie **Erstellen > alles erstellen** , damit die Xcode-Synchronisierung die neue Klasse (über das `Register`-Attribut) erkennt, die wir verwendet haben.

7. Öffnen Sie das Storyboard erneut, indem Sie mit der rechten Maustaste auf die storyboarddatei Watch APP und dann auf **Öffnen mit > Xcode Interface Builder**klicken:

    ![](troubleshooting-images/add-6.png "Opening the storyboard in Interface Builder")

8. Wählen Sie den neuen Schnittstellen Controller aus, und übergeben Sie ihm den von Ihnen definierten Klassennamen, z. b. [https://login.microsoftonline.com/consumers/](`MyInterfaceController`).
    Wenn alles ordnungsgemäß funktioniert hat, sollte es automatisch in der Dropdown Liste **Klasse:** angezeigt werden, und Sie können es von dort aus auswählen.

    ![](troubleshooting-images/add-4.png "Setting a custom class")

9. Wählen Sie in Xcode die Ansicht des **Assistenten-Editors** aus (das Symbol mit zwei überlappenden Kreisen), sodass Sie das Storyboard und den Code nebeneinander sehen können:

    ![](troubleshooting-images/add-7.png "The Assistant Editor toolbar item")

    Wenn sich der Fokus im Code Bereich befindet, stellen Sie sicher, dass Sie die **. h** -Header Datei betrachten, und wenn Sie nicht mit der rechten Maustaste in die Breadcrumb-Leiste klicken und die richtige Datei auswählen (**myinterfakecontroller. h).**

    ![](troubleshooting-images/add-8.png "Select MyInterfaceController")

10. Nun können Sie Outlets und Aktionen durch Drücken von **Strg + Drag** & amp; Drop aus dem Storyboard in die **. h** -Header Datei erstellen.

    ![](troubleshooting-images/add-9.png "Creating outlets and actions")

    Wenn Sie den Zieh Vorgang freigeben, werden Sie aufgefordert, auszuwählen, ob ein Outlet oder eine Aktion erstellt werden soll, und den Namen auszuwählen:

    ![](troubleshooting-images/add-a.png "The outlet and an action dialog")

11. Nachdem die storyboardänderungen gespeichert und Xcode geschlossen wurde, kehren Sie zu Visual Studio für Mac zurück. Die Header Dateiänderungen werden erkannt, und der **Designer.cs** -Datei wird automatisch Code hinzugefügt:

    ```csharp
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
    ```

Sie können jetzt auf C#das-Steuerelement verweisen (oder die-Aktion implementieren).

<a name="command_line" />

## <a name="launching-the-watch-app-from-the-command-line"></a>Starten der Überwachungs-App über die Befehlszeile

> [!IMPORTANT]
> Sie können die Watch-App standardmäßig im normalen APP-Modus starten und auch auf einen **Blick** oder **Benachrichtigungs** Modi mithilfe von [benutzerdefinierten Ausführungs Parametern](~/ios/watchos/get-started/installation.md#custommodes) in Visual Studio für Mac und Visual Studio.

Sie können auch die Befehlszeile verwenden, um den IOS-Simulator zu steuern. Das Befehlszeilen Tool, das zum Starten von Watch-Apps verwendet wird, ist **mberührungs**.

Im folgenden finden Sie ein vollständiges Beispiel (das im Terminal als einzelne Zeile ausgeführt wird):

```bash
/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/bin/mtouch --sdkroot=/Applications/Xcode.app/Contents/Developer/ --device=:v2:runtime=com.apple.CoreSimulator.SimRuntime.iOS-8-2,devicetype=com.apple.CoreSimulator.SimDeviceType.iPhone-6
--launchsimwatch=/path/to/watchkitproject/watchsample/bin/iPhoneSimulator/Debug/watchsample.app
```

Der Parameter, den Sie aktualisieren müssen, um Ihre APP widerzuspiegeln, ist `launchsimwatch`:

### <a name="--launchsimwatch"></a>--launchsimwatch

Der vollständige Pfad zum Haupt App Bundle *für die IOS-APP, die die Watch-APP und-Erweiterung enthält*.

> [!NOTE]
> Der Pfad, den Sie angeben müssen, ist die *App-Datei iPhone Application. app*, d. h. die Datei, die im IOS-Simulator bereitgestellt wird und sowohl die Watch-als auch die Watch-app enthält.

Beispiel:

```bash
--launchsimwatch=/path/to/watchkitproject/watchsample/bin/iPhoneSimulator/Debug/watchsample.app
```

## <a name="notification-mode"></a>Benachrichtigungs Modus

Legen Sie zum Testen des [ **Benachrichtigungs** Modus](~/ios/watchos/platform/notifications.md)der APP den `watchlaunchmode`-Parameter auf `Notification` fest, und geben Sie einen Pfad zu einer JSON-Datei an, die eine Test Benachrichtigungs Nutzlast enthält.

Der Nutz Last Parameter ist für den Benachrichtigungs Modus *erforderlich* .

Fügen Sie z. b. die folgenden Argumente zum mtouchscreen-Befehl hinzu:

```bash
--watchlaunchmode=Notification --watchnotificationpayload=/path/to/file.json
```

## <a name="other-arguments"></a>Weitere Argumente

Die übrigen Argumente werden im folgenden erläutert:

### <a name="--sdkroot"></a>--SDKRoot

Erforderlich. Gibt den Pfad zu Xcode an (6,2 oder höher).

Beispiel:

```bash
 --sdkroot /Applications/Xcode.app/Contents/Developer/
```

### <a name="--device"></a>--Gerät

Das auszuführende simulatorgerät. Dies kann auf zwei Arten angegeben werden, entweder mithilfe des UDID eines bestimmten Geräts oder mithilfe einer Kombination aus Laufzeit und Gerätetyp.

Die genauen Werte variieren zwischen den Computern und können mithilfe des Apple **simctl** -Tools abgefragt werden:

```bash
/Applications/Xcode.app/Contents/Developer/usr/bin/simctl list
```

**UDID**

Beispiel:

```bash
--device=:v2:udid=AAAAAAA-BBBB-CCCC-DDDD-EEEEEEEEEEEE
```

**Laufzeit und Gerätetyp**

Beispiel:

```bash
--device=:v2:runtime=com.apple.CoreSimulator.SimRuntime.iOS-8-2,devicetype=com.apple.CoreSimulator.SimDeviceType.iPhone-6
```

## <a name="related-links"></a>Verwandte Links

- [Watchkitcatalog (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Watchtables (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchtables)
