---
Title: "Mac-Platt Form Einrichtung" Beschreibung: "in diesem Artikel wird erläutert, wie ein Mac-Projekt zu einem Projekt hinzugefügt Xamarin.Forms wird, das eine APP erzeugt, die auf macOS Sierra und macOS El Capitan ausgeführt werden kann."
ms. Prod: xamarin ms. assetid: EEC549E0-F182-4F9C-B2BA-B31D19569AA5 ms. Technology: xamarin-Forms ms. Custom: xamu-Video Author: davidbritch ms. Author: dabritch ms. Date: 05/03/2017 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="mac-platform-setup"></a>Einrichten der Mac-Plattform

![Vorschau](~/media/shared/preview.png)

Bevor Sie beginnen, erstellen Sie ein Projekt (oder verwenden Sie ein vorhandenes) Xamarin.Forms . Sie können Mac-Apps nur mithilfe von Visual Studio für Mac hinzufügen.

> [!VIDEO https://youtube.com/embed/mvQ7jzaNseM]

**Hinzufügen eines macOS-Projekts zu einem Xamarin.Forms Video**

## <a name="adding-a-mac-app"></a>Hinzufügen einer Mac-app

Befolgen Sie diese Anweisungen zum Hinzufügen einer Mac-app, die auf macOS Sierra und macOS El Capitan ausgeführt werden kann:

1. Klicken Sie in Visual Studio für Mac mit der rechten Maustaste auf die vorhandene Projekt Mappe, Xamarin.Forms und wählen Sie **Hinzufügen > neues Projekt hinzufügen...**

2. Wählen Sie im Fenster **Neues Projekt** die Option **Mac > App > Cocoa-App** aus, und klicken Sie auf **weiter**.

3. Geben Sie einen **APP-Namen** ein (und wählen Sie optional einen anderen Namen für das Dock Element aus), und klicken Sie dann auf **weiter**.

4. Überprüfen Sie die Konfiguration, und klicken Sie auf **Erstellen**. Diese Schritte werden im folgenden dargestellt:

    ![Animierte Anweisungen zum Hinzufügen einer Cocoa-App](mac-images/add-macos-proj.gif)

5. Klicken Sie im Mac-Projekt mit der rechten Maustaste auf **Pakete, > Pakete hinzufügen...** , um nuget hinzuzufügen. [Xamarin.Forms](https://www.nuget.org/packages/Xamarin.Forms/) Sie sollten auch die anderen Projekte aktualisieren, sodass Sie dieselbe Version des Xamarin.Forms nuget-Pakets verwenden.

6. Klicken Sie im Mac-Projekt mit der rechten Maustaste auf **Verweise** , und fügen Sie einen Verweis auf das Xamarin.Forms Projekt (entweder frei gegebenes Projekt oder .NET Standard Bibliotheksprojekt) hinzu.

    ![Hinzufügen eines Verweises auf das Projekt für frei Xamarin.Forms gegebenen Code](mac-images/references-sml.png)

7. Aktualisieren Sie **Main.cs** , um zu initialisieren `AppDelegate` :

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

8. Aktualisieren `AppDelegate` Sie, um zu initialisieren Xamarin.Forms , erstellen Sie ein Fenster, und laden Sie die Xamarin.Forms Anwendung (denken Sie daran, eine geeignete festzulegen `Title` ). _Wenn Sie andere Abhängigkeiten haben, die initialisiert werden müssen, müssen Sie dies auch hier tun._

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

9. Doppelklicken Sie auf **Main. Storyboard** , um in Xcode zu bearbeiten. Wählen Sie das **Fenster** aus, und _Deaktivieren Sie_ das Kontrollkästchen **ist ursprünglicher Controller** (Dies liegt daran, dass der obige Code ein Fenster erstellt):

    [![Deaktivieren Sie das Kontrollkästchen is Initial Controller in Xcode.](mac-images/xcode-init-controller-sml.png)](mac-images/xcode-init-controller.png#lightbox)

    Sie können das Menüsystem im Storyboard bearbeiten, um unerwünschte Elemente zu entfernen.

10. Fügen Sie schließlich alle lokalen Ressourcen hinzu (z. b. Bilddateien) aus den vorhandenen Platt Form Projekten, die erforderlich sind.

11. Das Mac-Projekt sollte nun Ihren Xamarin.Forms Code unter macOS ausführen!

## <a name="next-steps"></a>Nächste Schritte

### <a name="styling"></a>Format

Mit aktuellen Änderungen an `OnPlatform` können Sie jetzt eine beliebige Anzahl von Plattformen als Ziel haben. Einschließlich macOS.

```xml
<Button.TextColor>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="White"/>
        <On Platform="macOS" Value="White"/>
        <On Platform="Android" Value="Black"/>
    </OnPlatform>
</Button.TextColor>
```

Beachten Sie, dass Sie sich auch auf Plattformen wie dem folgenden anmelden können: `<On Platform="iOS, macOS" ...>` .

### <a name="window-size-and-position"></a>Fenstergröße und-Position

Sie können die anfängliche Größe und Position des Fensters in der anpassen `AppDelegate` :

```csharp
var rect = new CoreGraphics.CGRect(200, 1000, 1024, 768);  // x, y, width, height
```

## <a name="known-issues"></a>Bekannte Probleme

Dies ist eine Vorschauversion, daher sollten Sie davon ausgehen, dass nicht alles in der Produktion bereit ist. Im folgenden finden Sie einige Dinge, die beim Hinzufügen von macOS zu Ihren Projekten auftreten können:

### <a name="not-all-nugets-are-ready-for-macos"></a>Nicht alle nugets sind für macOS bereit.

Möglicherweise stellen Sie fest, dass einige der Bibliotheken, die Sie verwenden, macOS noch nicht unterstützen. In diesem Fall müssen Sie eine Anforderung an den Maintainer des Projekts senden, um Sie hinzuzufügen. Bis Sie unterstützt werden, müssen Sie möglicherweise nach Alternativen suchen.

### <a name="missing-xamarinforms-features"></a>Fehlende Xamarin.Forms Features

Nicht alle Xamarin.Forms Funktionen sind in dieser Vorschauversion vollständig. Weitere Informationen finden Sie unter [Platt Form Unterstützung macOS-Status](https://github.com/xamarin/Xamarin.Forms/wiki/Platform-Support-macOS-Status) im [Xamarin.Forms](https://github.com/xamarin/Xamarin.Forms) GitHub-Repository.

## <a name="related-links"></a>Verwandte Links

- [Xamarin.Mac](~/mac/index.yml)
