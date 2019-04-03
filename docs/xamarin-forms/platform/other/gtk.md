---
title: GTK#-Plattform-Setup
description: Xamarin.Forms verfügt jetzt über eine vorschauunterstützung für die GTK#-Plattform
ms.prod: xamarin
ms.assetid: 3417FB95-3E4B-47DA-85D0-F34832747236
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/10/2018
ms.openlocfilehash: 7e9bfa841db9f0a76f762bab22050377830d85de
ms.sourcegitcommit: c4be32ef914465e808d89767c4d5ee72afe93cc6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2019
ms.locfileid: "58854872"
---
# <a name="gtk-platform-setup"></a>GTK#-Plattform-Setup

![Vorschau](~/media/shared/preview.png)

Xamarin.Forms verfügt jetzt über eine vorschauunterstützung für GTK#-apps. GTK#-ist ein GUI-Schnittstelle-Toolkit, die das Toolkit GTK +, und eine Vielzahl von GNOME-Bibliotheken verknüpft und so die Entwicklung von vollständig native GNOME Grafik-apps mithilfe von Mono und .NET. In diesem Artikel wird veranschaulicht, wie eine Xamarin.Forms-Projektmappe ein GTK#-Projekt hinzugefügt wird.

Bevor Sie beginnen, einer neuen Xamarin.Forms-Projektmappe erstellen, oder eine vorhandene Xamarin.Forms-Projektmappe, z. B. verwenden [ **GameOfLife**](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife/).

> [!NOTE]
> Während dieser Artikel konzentriert sich auf einer Xamarin.Forms-Projektmappe in Visual Studio 2017 und Visual Studio für Mac eine GTK#-app hinzugefügt, es kann auch durchgeführt werden [MonoDevelop](http://www.monodevelop.com/) für Linux.

## <a name="adding-a-gtk-app"></a>Hinzufügen einer GTK#-App

GTK#-für MacOS und Linux installiert ist, als Teil des [Mono](https://www.mono-project.com/download/stable/). GTK#-für .NET installiert werden kann Windows mit der [GTK#-Installer](https://www.mono-project.com/download/stable/#download-win).

# [<a name="visual-studio"></a>Visual Studio](#tab/windows)

Um eine GTK#-app hinzufügen, die auf dem Windows-Desktop ausgeführt wird, gehen Sie wie folgt vor:

1. Visual Studio-2019, mit der Maustaste auf den Projektmappennamen im **Projektmappen-Explorer** , und wählen Sie **hinzufügen > Neues Projekt...** .

2. In der **neues Projekt** Fenster auf der linken Select **Visual C#-** und **Klassischer Windows-Desktop**. Wählen Sie in der Liste der Projekttypen, **Class-Bibliothek ((.NET Framework)**, und stellen sicher, dass die **Framework** Dropdown-Liste auf ein Minimum von .NET Framework 4.7 festgelegt ist.

3. Geben Sie einen Namen für das Projekt mit einem **GTK#** Erweiterung, z. B. **GameOfLife.GTK**. Klicken Sie auf die **Durchsuchen** Schaltfläche, wählen Sie den Ordner mit der anderen Plattform-Projekte, und drücken Sie **Ordner auswählen**. Dies versetzt das GTK#-Projekt in demselben Verzeichnis wie die anderen Projekte in der Projektmappe.

    ![Hinzufügen eines neuen Projekts für GTK](gtk-images/win/add-new-project.png "Hinzufügen eines neuen GTK-Projekts")

    Drücken Sie die **OK** klicken, um das Projekt zu erstellen.

4. In der **Projektmappen-Explorer**, klicken Sie mit der rechten Maustaste auf das neue GTK-Projekt, und wählen Sie **NuGet-Pakete verwalten**. Wählen Sie die **Durchsuchen** Registerkarte, und suchen Sie nach **Xamarin.Forms** 3.0 oder höher.

    ![Wählen Sie das Xamarin.Forms-NuGet-Paket](gtk-images/win/select-forms-nuget-package.png "wählen Sie das Xamarin.Forms-NuGet-Paket")

    Wählen Sie das Paket, und klicken Sie auf die **installieren** Schaltfläche.

5. Suchen Sie nach der **Xamarin.Forms.Platform.GTK** 3.0-Paket oder höher.

    ![Wählen Sie das Xamarin.Forms.Platform.GTK NuGet-Paket](gtk-images/win/select-forms-platform-nuget-package.png "Xamarin.Forms.Platform.GTK NuGet-Paket auswählen")

    Wählen Sie das Paket, und klicken Sie auf die **installieren** Schaltfläche.

6. In der **Projektmappen-Explorer**mit der rechten Maustaste auf den Namen der Projektmappe, und wählen Sie **NuGet-Pakete für Projektmappe verwalten**. Wählen Sie die **Update** Registerkarte und der **Xamarin.Forms** Paket. Wählen Sie alle Projekte aus, und aktualisieren Sie diese auf die gleiche Version von Xamarin.Forms das GTK#-Projekt verwendet.

7. In der **Projektmappen-Explorer**, mit der rechten Maustaste auf **Verweise** im GTK-Projekt. In der **Verweis-Manager** wählen Sie im Dialogfeld **Projekte** links, und aktivieren Sie das Kontrollkästchen neben der .NET Standard "oder" Shared-Projekt:

    ![Verweisen auf das freigegebene Projekt](gtk-images/win/reference-shared-project.png "verweisen auf das freigegebene Projekt")

8. In der **Verweis-Manager** Dialog, drücken Sie die **Durchsuchen** Schaltfläche, und navigieren Sie zu der **C:\Program Files (x86)\GtkSharp\2.12\lib** Ordner, und wählen die  **AppleTalk-sharp.dll**, **Gdk-sharp.dll**, **Glade-sharp.dll**, **glib sharp.dll**, **Gtk-dotnet.dll**, **Gtk-sharp.dll** Dateien.

    ![Der GTK#-Bibliotheken verweisen](gtk-images/win/reference-gtk-libraries.png "verweisen auf die GTK#-Bibliotheken")

    Drücken Sie die **OK** , um die Verweise hinzuzufügen.

9. Benennen Sie im Projekt GTK# **"Class1.cs"** zu **"Program.cs"**.

10. Bearbeiten Sie in das GTK#-Projekt, das **"Program.cs"** Datei, sodass sie den folgenden Code ähnelt:

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

    Dieser Code initialisiert GTK# und Xamarin.Forms, ein Anwendungsfenster erstellt und führt die app.

11. In der **Projektmappen-Explorer**, klicken Sie mit der rechten Maustaste auf das GTK#-Projekt, und wählen Sie **Eigenschaften**.

12. In der **Eigenschaften** wählen Sie im Fenster der **Anwendung** Registerkarte, und ändern Sie die **Ausgabetyp** Dropdown-Liste für **Windows-Anwendung**.

    ![Ändern Sie den Ausgabetyp des Projekts](gtk-images/win/change-project-output-type.png "ändern Sie den Ausgabetyp des Projekts")

13. In der **Projektmappen-Explorer**mit der rechten Maustaste auf das GTK#-Projekt, und wählen Sie **als Startprojekt festlegen**. Drücken Sie F5, um das Programm mit Visual Studio-Debugger auf dem Windows-Desktop ausgeführt werden soll:

    ![GTK#-Spiel Leben](gtk-images/win/gtk-gameoflife.png "GTK#-Spiel Leben")

# [<a name="visual-studio-for-mac"></a>Visual Studio für Mac](#tab/macos)

Um eine GTK#-app hinzufügen, die auf dem Mac-Desktop ausgeführt wird, gehen Sie wie folgt vor:

1. Klicken Sie in Visual Studio für Mac mit der rechten Maustaste auf die Xamarin.Forms-Projektmappe, und wählen Sie **hinzufügen > Neues Projekt hinzufügen...** .

2. In der **neues Projekt** wählen **andere > .NET > GTK# 2.0-Projekt** , und drücken Sie **Weiter**.

3. Geben Sie einen Namen für das Projekt mit einem **GTK#** Erweiterung, z. B. **GameOfLife.GTK**, und drücken Sie die **erstellen**.

4. In der **Lösungspad**, mit der rechten Maustaste auf **Pakete > Pakete hinzufügen...**  für die GTK# Projekt, und fügen Sie das NuGet-Paket in Xamarin.Forms 3.0 Vorabversion oder höher.

    ![Wählen Sie das Xamarin.Forms-NuGet-Paket](gtk-images/mac/select-forms-nuget-package.png "wählen Sie das Xamarin.Forms-NuGet-Paket")

5. In der **Lösungspad**, mit der rechten Maustaste auf **Pakete > Pakete hinzufügen...**  für die GTK# Projekt, und fügen Sie das NuGet-Paket von Xamarin.Forms.Platform.GTK 3.0 Vorabversion oder höher.

    ![Wählen Sie das Xamarin.Forms.Platform.GTK NuGet-Paket](gtk-images/mac/select-forms-platform-nuget-package.png "Xamarin.Forms.Platform.GTK NuGet-Paket auswählen")

6. Aktualisieren Sie die anderen Plattformprojekte, um die gleiche Version von Xamarin.Forms verwendet das GTK#-Projekt zu verwenden.

7. In der **Lösungspad**, mit der rechten Maustaste auf **Verweise > Verweise bearbeiten...**  für die GTK# Projekt, und fügen einen Verweis auf das Xamarin.Forms-Projekt (.NET Standard oder freigegebenes Projekt).

    ![Verweisen auf das freigegebene Projekt](gtk-images/mac/reference-shared-project.png "verweisen auf das freigegebene Projekt")

8. Bearbeiten der **"Program.cs"** Datei des GTK-Projekts so, dass die It den folgenden Code ähnelt:

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

    Dieser Code initialisiert GTK# und Xamarin.Forms, ein Anwendungsfenster erstellt und führt die app.

9. In der **Lösungspad**mit der rechten Maustaste auf das GTK#-Projekt, und wählen Sie **als Startprojekt festlegen**.

10. In der Visual Studio für Mac-Symbolleiste, drücken Sie die **starten** (die dreieckige Schaltfläche, die einer Wiedergabeschaltfläche ähnelt) zum Starten der app.

    ![GTK#-Spiel Leben](gtk-images/mac/gtk-gameoflife.png "GTK#-Spiel Leben")

-----

## <a name="next-steps"></a>Nächste Schritte

### <a name="platform-specifics"></a>Plattformeigenschaften

Sie können bestimmen, welche Plattform Ihrer Xamarin.Forms-Anwendung entweder XAML oder Code ausgeführt wird. Dadurch können Sie auf Programm Merkmale zu ändern, wenn es auf GTK#-ausgeführt wird. Im Code vergleichen Sie den Wert von `Device.RuntimePlatform` mit der `Device.GTK` -Konstante (die die Zeichenfolge "GTK#" ist gleich). Wenn eine Übereinstimmung vorliegt, wird die Anwendung auf GTK# ausgeführt.

In XAML, können Sie mithilfe der `OnPlatform` Tag, um einen Eigenschaftswert für die Plattform auswählen:

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

Sie können das Symbol der app beim Start festlegen:

```csharp
window.SetApplicationIcon("icon.png");
```

### <a name="themes"></a>Designs

Es stehen eine Vielzahl von Designs für GTK#- und sie können aus einer Xamarin.Forms-app verwendet werden:

```csharp
GtkThemes.Init ();
GtkThemes.LoadCustomTheme ("Themes/gtkrc");
```

### <a name="native-forms"></a>Native Formulare

Native Formulare können Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-abgeleitete Seiten von systemeigenen Projekten, einschließlich GTK#-Projekten genutzt werden. Dies geschieht durch Erstellen einer Instanz von der [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-abgeleitete Seite und dem Konvertieren in den systemeigenen GTK#-Typ mithilfe der `CreateContainer` Erweiterungsmethode:

```csharp
var settingsView = new SettingsView().CreateContainer();
vbox.PackEnd(settingsView, true, true, 0);
```

Weitere Informationen zu systemeigenen Formen finden Sie unter [Native Formulare](~/xamarin-forms/platform/native-forms.md).

## <a name="issues"></a>Probleme

Dies ist eine Vorschau, damit Sie erwarten, dass nicht alles bereit für die Produktion. Der aktuelle Implementierungsstatus finden Sie unter [Status](https://github.com/jsuarezruiz/forms-gtk-progress/blob/master/Status.md), und die aktuell bekannte Probleme finden Sie unter [ausstehende und bekannten Probleme](https://github.com/jsuarezruiz/forms-gtk-progress/blob/master/Issues-Pending.md).
