---
title: GTK-Plattform-Setup
description: Xamarin.Forms verfügt jetzt über Unterstützung für die GTK-Plattform
ms.prod: xamarin
ms.assetid: 3417FB95-3E4B-47DA-85D0-F34832747236
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/10/2018
ms.openlocfilehash: a601e74cc274fd57bb2be9af3562b3a7290d7047
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="gtk-platform-setup"></a>GTK-Plattform-Setup

![Vorschau](~/media/shared/preview.png)

Xamarin.Forms verfügt jetzt über Preview-Unterstützung für GTK-apps. GTK # ist eine grafische Schnittstelle Toolkit, das das Toolkit GTK + und eine Vielzahl von GNOME-Bibliotheken verknüpft ermöglicht die Entwicklung von vollständig native GNONE-Grafiken-apps, die mit Mono und .NET. In diesem Artikel wird veranschaulicht, wie einer Xamarin.Forms-Projektmappe ein GTK-Projekt hinzugefügt.

Bevor Sie starten, erstellen Sie eine neue Xamarin.Forms-Projektmappe, oder verwenden Sie eine vorhandene Xamarin.Forms-Projektmappe z. B. [ **GameOfLife**](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife/).

> [!NOTE]
> Während dieser Artikel konzentriert sich auf einer Xamarin.Forms-Projektmappe in VS2017 und Visual Studio für Mac eine GTK-app hinzugefügt, er kann auch durchgeführt werden [MonoDevelop](http://www.monodevelop.com/) für Linux.

## <a name="adding-a-gtk-app"></a>Hinzufügen einer GTK-App

GTK # für MacOS und Linux installiert als Teil von [Mono](http://www.mono-project.com/download/stable/). GTK # für .NET können unter Windows mit installiert werden die [GTK-Installationsprogramm](http://www.mono-project.com/download/stable/#download-win).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

So fügen Sie einer GTK-app hinzu, die auf dem Windows-Desktop ausgeführt wird, gehen Sie wie folgt vor:

1. Visual Studio-2017, mit der Maustaste auf den Namen der Projektmappe in **Projektmappen-Explorer** , und wählen Sie **hinzufügen > Neues Projekt...** .

2. In der **neues Projekt** Fenster auf der linken Select **Visual C#-** und **klassische Windows-Desktop**. Wählen Sie in der Liste der Projekttypen **-Klassenbibliothek ((.NET Framework)**, und sicherstellen, dass die **Framework** Dropdown-Menü auf ein Minimum von .NET Framework 4.7 festgelegt ist.

3. Geben Sie einen Namen für das Projekt mit einem **GTK** Erweiterung, z. B. **GameOfLife.GTK**. Klicken Sie auf die **Durchsuchen** Schaltfläche, wählen Sie den Ordner, der andere Plattform enthält Projekte, und drücken Sie **Ordner auswählen**. Dadurch wird das Projekt GTK im selben Verzeichnis wie die anderen Projekte in der Projektmappe wurde.

    ![Hinzufügen ein neues Projekts für GTK](gtk-images/win/add-new-project.png "ein neues GTK-Projekt hinzufügen")

    Drücken Sie die **OK** Schaltfläche, um das Projekt zu erstellen.

4. In der **Projektmappen-Explorer**mit der rechten Maustaste auf das neue GTK-Projekt und wählen Sie **NuGet-Pakete verwalten**. Wählen Sie die **Durchsuchen** Registerkarte, klicken Sie auf die **Vorabversion einschließen** Kontrollkästchen, und suchen Sie nach **Xamarin.Forms** 3.0 oder höher.

    ![Wählen Sie das Xamarin.Forms-NuGet-Paket](gtk-images/win/select-forms-nuget-package.png "wählen Sie das Xamarin.Forms-NuGet-Paket")

    Wählen Sie das Paket, und klicken Sie auf die **installieren** Schaltfläche.

5. Suchen Sie nach der **Xamarin.Forms.Platform.GTK** 3.0-Paket oder höher.

    ![Wählen Sie das Xamarin.Forms.Platform.GTK NuGet-Paket](gtk-images/win/select-forms-platform-nuget-package.png "wählen Sie das Xamarin.Forms.Platform.GTK NuGet-Paket")

    Wählen Sie das Paket, und klicken Sie auf die **installieren** Schaltfläche.

6. In der **Projektmappen-Explorer**mit der rechten Maustaste auf den Namen der Projektmappe, und wählen Sie **NuGet-Pakete für Projektmappe verwalten**. Wählen Sie die **Update** Registerkarte und der **Xamarin.Forms** Paket. Alle Projekte auswählen und auf die gleiche Xamarin.Forms-Version vom das Projekt GTK verwendete zu aktualisieren.

7. In der **Projektmappen-Explorer**, mit der rechten Maustaste auf **Verweise** im Projekt GTK. In der **Verweis-Manager** wählen Sie im Dialogfeld **Projekte** auf der linken und aktivieren Sie das Kontrollkästchen neben dem Projekt .NET Standard, PCL oder freigegeben:

    ![Verweisen auf das freigegebene Projekt](gtk-images/win/reference-shared-project.png "verweisen auf das freigegebene Projekt")

8. In der **Verweis-Manager** Dialog, drücken Sie die **Durchsuchen** Schaltfläche aus, und navigieren Sie zu der **C:\Program Files (x86)\GtkSharp\2.12\lib** Ordner, und wählen die  **AppleTalk-sharp.dll**, **Gdk sharp.dll**, **Glade sharp.dll**, **glib sharp.dll**, **Gtk-dotnet.dll**, **Gtk sharp.dll** Dateien.

    ![Verweisen auf die GTK-Bibliotheken](gtk-images/win/reference-gtk-libraries.png "verweisen die GTK-Bibliotheken")

    Drücken Sie die **OK** Schaltfläche, um die Verweise hinzuzufügen.

9. Benennen Sie das Projekt GTK **"Class1.cs"** auf **"Program.cs"**.

10. Bearbeiten Sie im Projekt GTK der **"Program.cs"** Datei, sodass sie den folgenden Code ähnelt:

    ```csharp
    using System;
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.GTK;

    namespace GameOfLife.GTK
    {
        class MainClass
        {
            [STAThread]
            public static void Main(string[] args)
            {
                Gtk.Application.Init();
                Forms.Init();

                var app = new App();
                var window = new FormsWindow();
                window.LoadApplication(app);
                window.SetApplicationTitle("Game of Life");
                window.Show();

                Gtk.Application.Run();
            }
        }
    }
    ```

    Dieser Code initialisiert GTK- und Xamarin.Forms, ein Anwendungsfenster erstellt und führt die app.

11. In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt GTK und wählen Sie **Eigenschaften**.

12. In der **Eigenschaften** wählen die **Anwendung** Registerkarte, und ändern Sie die **Ausgabetyp** Dropdownelement **Windows-Anwendung**.

    ![Ändern den Ausgabetyp des Projekts](gtk-images/win/change-project-output-type.png "den Ausgabetyp des Projekts ändern")

13. In der **Projektmappen-Explorer**mit der rechten Maustaste auf das WPF-Projekt, und wählen Sie **als Startprojekt festlegen**. Drücken Sie F5, um das Programm auf dem Windows-Desktop mit dem Visual Studio-Debugger ausgeführt:

    ![GTK-Spiel abgelaufenen](gtk-images/win/gtk-gameoflife.png "Spiel GTK-Lebensdauer")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Hinzufügen eine GTK-app, die auf dem Macintosh-Desktop ausgeführt wird, gehen Sie wie folgt vor:

1. Klicken Sie in Visual Studio für Mac mit der rechten Maustaste auf die Xamarin.Forms-Projektmappe, und wählen Sie **hinzufügen > Neues Projekt hinzufügen...** .

2. In der **neues Projekt** Fenster **andere > .NET > Gtk-2.0-Projekt** , und drücken Sie **Weiter**.

3. Geben Sie einen Namen für das Projekt mit einem **GTK** Erweiterung, z. B. **GameOfLife.GTK**, und drücken Sie die **erstellen**.

4. In der **Lösung Pad**, mit der rechten Maustaste auf **Pakete > Pakete hinzufügen...**  für die GTK Projekt, und fügen Sie das Xamarin.Forms 3.0 Vorabversion NuGet-Paket oder höher.

    ![Wählen Sie das Xamarin.Forms-NuGet-Paket](gtk-images/mac/select-forms-nuget-package.png "wählen Sie das Xamarin.Forms-NuGet-Paket")

5. In der **Lösung Pad**, mit der rechten Maustaste auf **Pakete > Pakete hinzufügen...**  für die GTK Projekt, und fügen Sie das Xamarin.Forms.Platform.GTK 3.0 Vorabversion NuGet-Paket oder höher.

    ![Wählen Sie das Xamarin.Forms.Platform.GTK NuGet-Paket](gtk-images/mac/select-forms-platform-nuget-package.png "wählen Sie das Xamarin.Forms.Platform.GTK NuGet-Paket")

6. Aktualisieren Sie die anderen Plattformprojekte Verwendung die gleiche Xamarin.Forms-Version vom das Projekt GTK verwendete.

7. In der **Lösung Pad**, mit der rechten Maustaste auf **Verweise > Verweise bearbeiten...**  für die GTK Projekt, und fügen Sie einen Verweis auf das Xamarin.Forms-Projekt (entweder Standard .NET, PCL oder freigegebenes Projekt) hinzu.

    ![Verweisen auf das freigegebene Projekt](gtk-images/mac/reference-shared-project.png "verweisen auf das freigegebene Projekt")

8. Bearbeiten der **"Program.cs"** -Datei des Projekts GTK so, dass die It über den folgenden Code ähnelt:

    ```csharp
    using System;
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.GTK;

    namespace GameOfLife.GTK
    {
        class MainClass
        {
            [STAThread]
            public static void Main(string[] args)
            {
                Gtk.Application.Init();
                Forms.Init();

                var app = new App();
                var window = new FormsWindow();
                window.LoadApplication(app);
                window.SetApplicationTitle("Game of Life");
                window.Show();

                Gtk.Application.Run();
            }
        }
    }
    ```

    Dieser Code initialisiert GTK- und Xamarin.Forms, ein Anwendungsfenster erstellt und führt die app.

9. In der **Lösung Pad**mit der rechten Maustaste auf das GTK-Projekt, und wählen Sie **als Startprojekt festlegen**.

10. In Visual Studio für Mac-Symbolleiste, drücken Sie die **starten** Schaltfläche (die dreieckige Schaltfläche, die eine Schaltfläche Abspielen ähnelt) zum Starten der app.

    ![GTK-Spiel abgelaufenen](gtk-images/mac/gtk-gameoflife.png "Spiel GTK-Lebensdauer")

-----

## <a name="next-steps"></a>Nächste Schritte

### <a name="platform-specifics"></a>Platform-Besonderheiten

Sie können bestimmen, welche Plattform Xamarin.Forms aus XAML oder Code zur Ausführung Ihrer Anwendung auf. Dadurch können Sie die Merkmale der Anwendung zu ändern, wenn er auf GTK-ausgeführt wird. Im Code, vergleichen Sie den Wert der `Device.RuntimePlatform` mit der `Device.GTK` -Konstante (die die Zeichenfolge "GTK" ist gleich). Wenn eine Übereinstimmung vorhanden ist, wird die Anwendung auf GTK # ausgeführt.

In XAML können Sie die `OnPlatform` Tag um einen Eigenschaftswert, der spezifisch für die Plattform auszuwählen:

```xaml
<Button.TextColor>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="White" />
        <On Platform="macOS" Value="White" />
        <On Platform="Android" Value="Black" />
        <On Platform="GTK" Value="Blue" />
    </OnPlatform>
</Button.TextColor>
```

### <a name="application-icon"></a>Anwendungssymbol

Sie können das Symbol "app" beim Start festlegen:

```csharp
window.SetApplicationIcon("icon.png");
```

### <a name="themes"></a>Designs

Es stehen eine Vielzahl von Designs für GTK # und aus einer Xamarin.Forms-app verwendet werden können:

```csharp
GtkThemes.Init ();
GtkThemes.LoadCustomTheme ("Themes/gtkrc");
```

### <a name="native-forms"></a>Systemeigene Formulare

Systemeigene Forms ermöglicht Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-abgeleitete Seiten von systemeigenen Projekte, einschließlich GTK-Projekten verwendet wird. Dies kann durch Erstellen einer Instanz von der [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-abgeleitete Seite mit entsprechender Konvertierung zu den systemeigenen GTK-Typ mithilfe der `CreateContainer` Erweiterungsmethode:

```csharp
var settingsView = new SettingsView().CreateContainer();
vbox.PackEnd(settingsView, true, true, 0);
```

Weitere Informationen zu systemeigenen Formen finden Sie unter [Native Forms](~/xamarin-forms/platform/native-forms.md).

## <a name="issues"></a>Schwierigkeiten

Dies ist eine Vorschau, damit Sie erwarten, dass nicht alles, was die Produktion bereit ist. Der aktuelle Implementierungsstatus finden Sie unter [Status](https://github.com/jsuarezruiz/forms-gtk-progress/blob/master/Status.md), und die aktuell bekannte Probleme finden Sie unter [ausstehende & bekannte Probleme](https://github.com/jsuarezruiz/forms-gtk-progress/blob/master/Issues-Pending.md).
