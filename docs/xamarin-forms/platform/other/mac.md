---
title: Mac-Plattform-Setup
description: In diesem Artikel wird erläutert, wie Sie ein Mac-Projekt ein Xamarin.Forms-Projekt hinzufügen, das erzeugt eine app unter MacOS Sierra und MacOS El Capitan ausgeführt werden kann.
ms.prod: xamarin
ms.assetid: EEC549E0-F182-4F9C-B2BA-B31D19569AA5
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/03/2017
ms.openlocfilehash: 3a488b3a9f729da5d4bee8c1262190b15c2e9240
ms.sourcegitcommit: 0044d04990faa0b144b8626a4fceea0fdff95cfe
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/22/2019
ms.locfileid: "56666895"
---
# <a name="mac-platform-setup"></a>Mac-Plattform-Setup

![Vorschau](~/media/shared/preview.png)

Bevor Sie beginnen, erstellen (oder verwenden Sie ein vorhandenes) Xamarin.Forms-Projekt. Sie können nur mithilfe von Visual Studio für Mac. Mac-apps hinzufügen.

> [!VIDEO https://youtube.com/embed/mvQ7jzaNseM]

**Xamarin.Forms, von einem MacOS-Projekt hinzugefügt [Xamarin University](https://university.xamarin.com/)**

## <a name="adding-a-mac-app"></a>Hinzufügen einer Mac-App

Um eine Mac-app hinzufügen, die unter MacOS Sierra und MacOS El Capitan ausgeführt wird, gehen Sie wie folgt vor:

1. Klicken Sie in Visual Studio für Mac mit der rechten Maustaste auf die vorhandenen Xamarin.Forms-Projektmappe, und wählen Sie **hinzufügen > Neues Projekt hinzufügen...**

2. In der **neues Projekt** wählen **Mac > App > Cocoa-App** , und drücken Sie **Weiter**.

3. Typ einer **Anwendungsnamen** (und optional einen anderen Namen für das Dock-Element auswählen), drücken Sie dann die **Weiter**.

4. Überprüfen Sie die Konfiguration, und drücken Sie **erstellen**. Diese Schritte werden im unten gezeigt:

    ![Animierte Anweisungen, die zeigt, wie eine Cocoa-app hinzufügen](mac-images/add-macos-proj.gif)

5. Die Mac-Projekt mit der Maustaste auf **Pakete > Pakete hinzufügen...**  Hinzufügen der [Xamarin.Forms](https://www.nuget.org/packages/Xamarin.Forms/) NuGet. Sie sollten auch die anderen Projekte, verwenden Sie die gleiche Version von Xamarin.Forms-NuGet-Paket aktualisieren.

6. Die Mac-Projekt mit der Maustaste auf **Verweise** und Hinzufügen eines Verweises auf das Xamarin.Forms-Projekt (freigegebenes Projekt oder .NET Standard Library-Projekt).

    ![Hinzufügen eines Verweises auf das Xamarin.Forms-Projekt für freigegebenen code](mac-images/references-sml.png)

7. Update **Main.cs** zum Initialisieren der `AppDelegate`:

    ```csharp
    static class MainClass
    {
        static void Main(string[] args)
        {
            NSApplication.Init();
            NSApplication.SharedApplication.Delegate = new AppDelegate(); // add this line
            NSApplication.Main(args);
        }
    }
    ```

8. Update `AppDelegate` um Xamarin.Forms zu initialisieren, erstellen Sie ein Fenster, und Laden Sie die Xamarin.Forms-Anwendung (Denken Sie daran, legen Sie eine entsprechende `Title`). _Wenn Sie andere Abhängigkeiten, die initialisiert werden müssen verfügen, verwenden Sie hier ebenfalls._

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.MacOS;
    // also add a using for the Xamarin.Forms project, if the namespace is different to this file
    ...
    [Register("AppDelegate")]
    public class AppDelegate : FormsApplicationDelegate
    {
        NSWindow window;
        public AppDelegate()
        {
            var style = NSWindowStyle.Closable | NSWindowStyle.Resizable | NSWindowStyle.Titled;

            var rect = new CoreGraphics.CGRect(200, 1000, 1024, 768);
            window = new NSWindow(rect, style, NSBackingStore.Buffered, false);
            window.Title = "Xamarin.Forms on Mac!"; // choose your own Title here
            window.TitleVisibility = NSWindowTitleVisibility.Hidden;
        }

        public override NSWindow MainWindow
        {
            get { return window; }
        }

        public override void DidFinishLaunching(NSNotification notification)
        {
            Forms.Init();
            LoadApplication(new App());
            base.DidFinishLaunching(notification);
        }
    }
    ```

9. Doppelklicken Sie auf **"Main.Storyboard"** zur Bearbeitung in Xcode. Wählen Sie die **Fenster** und _deaktivieren_ der **ersten Controller ist** Kontrollkästchen (Dies ist daran, dass der Code oben ein Fenster erstellt):

    [![Deaktivieren Sie das Kontrollkästchen der erste Controller wird in Xcode](mac-images/xcode-init-controller-sml.png)](mac-images/xcode-init-controller.png#lightbox)

    Sie können das Menüsystem, in dem Storyboard So entfernen Sie unerwünschte Elemente bearbeiten.

10. Fügen Sie abschließend alle lokalen Ressourcen (z. b. Bilddateien) aus den vorhandenen Plattform-Projekten, die erforderlich sind.

11. Das Mac-Projekt sollte Ihrer Xamarin.Forms-Code jetzt unter MacOS ausgeführt werden.

## <a name="next-steps"></a>Nächste Schritte

### <a name="styling"></a>Format

Mit der letzten Änderungen an `OnPlatform` können Sie jetzt eine beliebige Anzahl von Plattformen abzielen. Dazu gehören MacOS.

```xml
<Button.TextColor>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="White"/>
        <On Platform="macOS" Value="White"/>
        <On Platform="Android" Value="Black"/>
    </OnPlatform>
</Button.TextColor>
```

Beachten Sie, Sie können auch verdoppeln Sie auf Plattformen wie folgt: `<On Platform="iOS, macOS" ...>`.

### <a name="window-size-and-position"></a>Fenstergröße und-Position

Sie können anpassen, die anfängliche Größe und Position des Fensters in der `AppDelegate`:

```csharp
var rect = new CoreGraphics.CGRect(200, 1000, 1024, 768);  // x, y, width, height
```

## <a name="known-issues"></a>Bekannte Probleme

Dies ist eine Vorschau, damit Sie erwarten, dass nicht alles bereit für die Produktion. Im folgenden finden Sie einige Dinge, die möglicherweise auftreten, wenn Sie MacOS zu Ihren Projekten hinzufügen:

### <a name="not-all-nugets-are-ready-for-macos"></a>Nicht alle NuGet-Pakete sind bereit für macOS

Möglicherweise, dass einige Bibliotheken, die Sie verwenden noch nicht über MacOS unterstützen. In diesem Fall müssen Sie zum Senden einer Anforderung an den Maintainer des Projekts, um es hinzuzufügen. Bis sie die Unterstützung verfügen, müssen Sie möglicherweise nach alternativen gesucht werden soll.

### <a name="missing-xamarinforms-features"></a>Fehlende Xamarin.Forms-Features

Nicht alle Xamarin.Forms-Funktionen sind vollständig in dieser Preview-Version. Weitere Informationen finden Sie unter [Plattformunterstützung MacOS Status](https://github.com/xamarin/Xamarin.Forms/wiki/Platform-Support-macOS-Status) in die [Xamarin.Forms](https://github.com/xamarin/Xamarin.Forms) GitHub-Repository.

## <a name="related-links"></a>Verwandte Links

- [Xamarin.Mac](~/mac/index.yml)
