---
title: Mac-Plattform-Setup
description: Xamarin.Forms verfügt jetzt über Unterstützung für die Macintosh-Plattform
ms.prod: xamarin
ms.assetid: EEC549E0-F182-4F9C-B2BA-B31D19569AA5
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/03/2017
ms.openlocfilehash: de08e686fc07595b75016b9266f57b12831e9822
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="mac-platform-setup"></a>Mac-Plattform-Setup

![Vorschau](~/media/shared/preview.png)

Bevor Sie beginnen, erstellen (oder verwenden Sie ein vorhandenes) Xamarin.Forms-Projekt.
Sie können nur Mac-apps mit Visual Studio für Mac hinzufügen.

> [!VIDEO https://youtube.com/embed/mvQ7jzaNseM]

**Hinzufügen eines MacOS-Projekts in Xamarin.Forms, indem [Xamarin University](https://university.xamarin.com/)**

## <a name="adding-a-mac-app"></a>Hinzufügen einer Mac-App

Hinzufügen eine Mac-app, die auf MacOS Sierra und Mac OS X El Capitan ausgeführt wird, gehen Sie wie folgt vor:

1. Klicken Sie in Visual Studio für Mac mit der rechten Maustaste auf die vorhandenen Xamarin.Forms-Projektmappe, und wählen Sie **hinzufügen > Neues Projekt hinzufügen...**

2. In der **neues Projekt** Fenster **Mac > App > Kakao App** , und drücken Sie **Weiter**.

3. Typ einer **Anwendungsnamen** (und optional einen anderen Namen für das Element Andocken auswählen), drücken Sie dann die **Weiter**.

4. Überprüfen Sie die Konfiguration, und drücken Sie **erstellen**. Diese Schritte werden im unten gezeigt:

  ![Animierte Anweisungen zum Hinzufügen einer Kakao-app](mac-images/add-macos-proj.gif)

5. Das Mac-Projekt mit der Maustaste auf **Pakete > Pakete hinzufügen...**  Hinzufügen der [Xamarin.Forms/2.3.5.235-pre2](https://www.nuget.org/packages/Xamarin.Forms/2.3.5.235-pre2) NuGet. Sie sollten auch andere Projekte in dieser Version aktualisieren.

6. Das Mac-Projekt mit der Maustaste auf **Verweise** und fügen Sie einen Verweis auf das Xamarin.Forms-Projekt (freigegebenes Projekt oder PCL).

  ![Fügen Sie einen Verweis auf das Projekt mit freigegebenem Code Xamarin.Forms](mac-images/references-sml.png)

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

8. Update `AppDelegate` um Xamarin.Forms zu initialisieren, erstellen Sie ein Fenster, und Laden Sie das Xamarin.Forms-Anwendung (Denken Sie daran, legen Sie eine entsprechende `Title`). _Wenn Sie andere Abhängigkeiten verfügen, die müssen initialisiert werden, holen Sie dies hier ebenfalls._

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

9. Doppelklicken Sie auf **Main.storyboard** in Xcode zu bearbeiten. Wählen Sie die **Fenster** und _deaktivieren Sie_ der **ersten Controller ist** Kontrollkästchen (handelt, da der Code oben ein Fenster erstellt wird):

  [![Deaktivieren Sie das Kontrollkästchen der erste Controller wird in Xcode](mac-images/xcode-init-controller-sml.png)](mac-images/xcode-init-controller.png#lightbox)

  Sie können das Menüsystem in das Storyboard So entfernen Sie unerwünschte Elemente bearbeiten.

10. Fügen Sie schließlich alle lokalen Ressourcen (z. b. Bilddateien) aus den vorhandenen plattformprojekten, die erforderlich sind.

11. Das Mac-Projekt sollte nun Codes Xamarin.Forms auf MacOS ausgeführt!

## <a name="next-steps"></a>Nächste Schritte

### <a name="styling"></a>Format

Mit den neuesten Änderungen zu `OnPlatform` können Sie nun eine beliebige Anzahl von Plattformen abzielen. Dazu gehören MacOS.

```xml
<Button.TextColor>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="White"/>
        <On Platform="macOS" Value="White"/>
        <On Platform="Android" Value="Black"/>
    </OnPlatform>
</Button.TextColor>
```

Beachten Sie, Sie können auch auf Plattformen wie folgt verdoppeln: `<On Platform="iOS, macOS" ...>`.

### <a name="window-size-and-position"></a>Größe und Position

Sie können anpassen, die ursprüngliche Größe und Position des Fensters in den `AppDelegate`:

```csharp
var rect = new CoreGraphics.CGRect(200, 1000, 1024, 768);  // x, y, width, height
```

## <a name="known-issues"></a>Bekannte Probleme

Dies ist eine Vorschau, damit Sie erwarten, dass nicht alles, was die Produktion bereit ist. Im folgenden finden Sie einige Dinge, die möglicherweise auftreten, wenn Sie Ihre Projekte MacOS hinzufügen:

### <a name="not-all-nugets-are-ready-for-macos"></a>Nicht alle NuGets bereitstehen macOS

Pakete müssen "xamarinmac20" als Ziel für die Zusammenarbeit in einem Projekt MacOS. Sie können möglicherweise einige der Bibliotheken, die Sie verwenden MacOS noch nicht unterstützt wird.

In diesem Fall müssen Sie zum Senden einer Anforderung an den Maintainer des Projekts, um sie hinzuzufügen. Bis diese Unterstützung verfügen, müssen Sie möglicherweise die alternativen für suchen.

### <a name="missing-xamarinforms-features"></a>Fehlende Xamarin.Forms-Funktionen

Nicht alle Xamarin.Forms-Funktionen sind vollständig in dieser Vorschau; Hier ist eine Liste der Teil der Funktionen, die noch nicht implementiert wird:

* Fußzeile
* Abbildung – Aspekt
* ListView – ScrollTo, UnevenRows zu unterstützen, aktualisieren, SeparatorColor, SeparatorVisibility
* MasterDetailPage – BackgroundColor
* Navigation – InsertPageBefore
* OpenGLRenderer
* Auswahl – Bindable/Observable-Implementierung
* TabbedPage – BarBackgroundColor, BarTextColor
* TableView – UnevenRows
* ViewCell – IsEnabled, ForceUpdateSize
* WebView – die meisten WebNavigationEvents


## <a name="related-links"></a>Verwandte Links

- [Xamarin.Mac](~/mac/index.yml)
