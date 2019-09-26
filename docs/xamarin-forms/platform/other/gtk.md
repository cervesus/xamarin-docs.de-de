---
title: 'GTK #-Platt Form Einrichtung'
description: 'Xamarin. Forms verfügt jetzt über eine Vorschau Unterstützung für die GTK #-Plattform.'
ms.prod: xamarin
ms.assetid: 3417FB95-3E4B-47DA-85D0-F34832747236
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/10/2018
ms.openlocfilehash: 56075949a5b5c01873af3ff79a4cf8f6cefcb142
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "68644551"
---
# <a name="gtk-platform-setup"></a>GTK #-Platt Form Einrichtung

![Vorschau](~/media/shared/preview.png)

Xamarin. Forms verfügt jetzt über eine Vorschau Unterstützung für gtk #-apps. GTK # ist ein grafisches Toolkit für die Benutzeroberfläche, das das GTK +-Toolkit und eine Vielzahl von GNOME-Bibliotheken verknüpft und die Entwicklung von vollständig nativen gnome-Grafik-apps mit Mono und .NET ermöglicht. In diesem Artikel wird veranschaulicht, wie Sie ein GTK #-Projekt zu einer xamarin. Forms-Projekt Mappe hinzufügen.

Bevor Sie beginnen, erstellen Sie eine neue xamarin. Forms-Projekt Mappe, oder verwenden Sie eine vorhandene xamarin. Forms-Lösung, z. b. [**gameoarlife**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-gameoflife).

> [!NOTE]
> Der Schwerpunkt dieses Artikels liegt auf dem Hinzufügen einer GTK #-APP zu einer xamarin. Forms-Lösung in VS2017 und Visual Studio für Mac. Sie kann jedoch auch in [monodevelop](http://www.monodevelop.com/) für Linux ausgeführt werden.

## <a name="adding-a-gtk-app"></a>Hinzufügen einer GTK #-App

GTK # für macOS und Linux wird als Teil von [Mono](https://www.mono-project.com/download/stable/)installiert. GTK # für .net kann unter Windows mit dem [gtk #-Installer](https://www.mono-project.com/download/stable/#download-win)installiert werden.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Befolgen Sie diese Anweisungen zum Hinzufügen einer GTK #-APP, die auf dem Windows-Desktop ausgeführt wird:

1. Klicken Sie in Visual Studio 2019 in **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektmappennamen, und wählen Sie **> Neues Projekt hinzufügen...** aus.

2. Wählen Sie im Fenster **Neues Projekt** auf der linken Seite die Option **Visual C#**  und **klassischer Windows-Desktop**aus. Wählen Sie in der Liste der Projekttypen die Option **Klassenbibliothek (.NET Framework)** aus, und stellen Sie sicher, dass die Dropdown Liste für das **Framework** auf mindestens .NET Framework 4,7 festgelegt ist.

3. Geben Sie einen Namen für das Projekt mit einer **gtk** -Erweiterung ein, z. b. **gameof Life. GTK**. Klicken Sie auf die Schaltfläche **Durchsuchen** , wählen Sie den Ordner mit den anderen Platt Form Projekten aus, und drücken **Sie Ordner auswählen**. Dadurch wird das GTK-Projekt in dasselbe Verzeichnis wie die anderen Projekte in der Projekt Mappe eingefügt.

    ![Hinzufügen eines neuen gtk-Projekts](gtk-images/win/add-new-project.png "Hinzufügen eines neuen gtk-Projekts")

    Klicken Sie auf die Schaltfläche **OK** , um das Projekt zu erstellen.

4. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf das neue gtk-Projekt, und wählen Sie **nuget-Pakete verwalten**. Wählen Sie die Registerkarte **Durchsuchen** aus, und suchen Sie nach **xamarin. Forms** 3,0 oder höher.

    ![Wählen Sie das xamarin. Forms-nuget-Paket aus.](gtk-images/win/select-forms-nuget-package.png "Wählen Sie das xamarin. Forms-nuget-Paket aus.")

    Wählen Sie das Paket, und klicken Sie auf die Schaltfläche **Installieren** .

5. Suchen Sie nun nach dem Paket **xamarin. Forms. Platform. GTK** 3,0 oder höher.

    ![Wählen Sie das nuget-Paket xamarin. Forms. Platform. GTK] aus. (gtk-images/win/select-forms-platform-nuget-package.png "Wählen Sie das nuget-Paket xamarin. Forms. Platform. GTK") aus.

    Wählen Sie das Paket, und klicken Sie auf die Schaltfläche **Installieren** .

6. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf den Projektmappennamen, und wählen Sie **nuget-Pakete für**Projekt Mappe verwalten. Wählen Sie die Registerkarte **Aktualisieren** und das **xamarin. Forms** -Paket aus. Wählen Sie alle Projekte aus, und aktualisieren Sie Sie auf dieselbe xamarin. Forms-Version, die vom gtk-Projekt verwendet wird.

7. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf **Verweise** im gtk-Projekt. Wählen Sie im Dialogfeld **Verweis-Manager** die Option **Projekte** auf der linken Seite aus, und aktivieren Sie das Kontrollkästchen neben dem .NET Standard oder dem freigegebenen Projekt:

    ![Verweisen auf das freigegebene Projekt](gtk-images/win/reference-shared-project.png "Verweisen auf das freigegebene Projekt")

8. Klicken Sie im Dialogfeld **Verweis-Manager** auf die Schaltfläche **Durchsuchen** , navigieren Sie zum Ordner **c:\Programme (x86) \gtksharp\2.12\lib** , **und wählen Sie die Datei ATK-Sharp. dll, gdk-Sharp. dll, glade-Sharp. dll aus. glib-sharp. dll**, **gtk-dotnet. dll**, **gtk-sharp. dll** -Dateien.

    ![Verweisen auf die GTK #-Bibliotheken](gtk-images/win/reference-gtk-libraries.png "Verweisen auf die GTK #-Bibliotheken")

    Klicken Sie auf die Schaltfläche **OK** , um die Verweise hinzuzufügen.

9. Benennen Sie **Class1.cs** im gtk-Projekt in **Program.cs**um.

10. Bearbeiten Sie im gtk-Projekt die Datei **Program.cs** , sodass Sie dem folgenden Code ähnelt:

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

    Dieser Code initialisiert GTK # und xamarin. Forms, erstellt ein Anwendungsfenster und führt die APP aus.

11. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf das GTK-Projekt, und wählen Sie **Eigenschaften**.

12. Wählen Sie im Fenster **Eigenschaften** die Registerkarte **Anwendung** aus, und ändern Sie das Dropdown Feld **Ausgabetyp** in **Windows-Anwendung**.

    ![Ändern des Projekt Ausgabe Typs](gtk-images/win/change-project-output-type.png "Ändern des Projekt Ausgabe Typs")

13. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf das GTK-Projekt, und wählen Sie **als Startprojekt festlegen**aus. Drücken Sie F5, um das Programm mit dem Visual Studio-Debugger auf dem Windows-Desktop auszuführen:

    ![Gtk #-Lebens] Zyklus (gtk-images/win/gtk-gameoflife.png "Gtk #-Lebens") Zyklus

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Befolgen Sie diese Anweisungen zum Hinzufügen einer GTK #-APP, die auf dem Mac-Desktop ausgeführt wird:

1. Klicken Sie in Visual Studio für Mac mit der rechten Maustaste auf die xamarin. Forms-Projekt Mappe, und wählen Sie **Hinzufügen > Neues Projekt hinzufügen...** aus.

2. Wählen Sie im Fenster **Neues Projekt** die Option **andere > .net > gtk # 2,0-Projekt** aus, und klicken Sie auf **weiter**.

3. Geben Sie einen Namen für das Projekt mit einer **gtk** -Erweiterung ein, z **. b. gameof Life. GTK**, und klicken Sie auf **Erstellen**.

4. Klicken Sie im **Lösungspad**mit der rechten Maustaste auf **Pakete, > Pakete hinzuzufügen...** für das GTK-Projekt, und fügen Sie das nuget-Paket xamarin. Forms 3,0 oder höher hinzu.

    ![Wählen Sie das xamarin. Forms-nuget-Paket aus.](gtk-images/mac/select-forms-nuget-package.png "Wählen Sie das xamarin. Forms-nuget-Paket aus.")

5. Klicken Sie im **Lösungspad**mit der rechten Maustaste auf **Pakete, > Pakete hinzuzufügen...** für das GTK-Projekt, und fügen Sie das nuget-Paket xamarin. Forms. Platform. gtk 3,0 oder höher hinzu.

    ![Wählen Sie das nuget-Paket xamarin. Forms. Platform. GTK] aus. (gtk-images/mac/select-forms-platform-nuget-package.png "Wählen Sie das nuget-Paket xamarin. Forms. Platform. GTK") aus.

6. Aktualisieren Sie die anderen Platt Form Projekte so, dass die gleiche xamarin. Forms-Version verwendet wird, die vom gtk-Projekt verwendet wird.

7. Klicken Sie im **Lösungspad**mit der rechten Maustaste auf **Verweise > Bearbeiten Sie Verweise...** für das GTK-Projekt, und fügen Sie einen Verweis auf das xamarin. Forms-Projekt (entweder .NET Standard oder ein frei gegebenes Projekt) hinzu.

    ![Verweisen auf das freigegebene Projekt](gtk-images/mac/reference-shared-project.png "Verweisen auf das freigegebene Projekt")

8. Bearbeiten Sie die Datei **Program.cs** des GTK-Projekts so, dass Sie dem folgenden Code ähnelt:

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

    Dieser Code initialisiert GTK # und xamarin. Forms, erstellt ein Anwendungsfenster und führt die APP aus.

9. Klicken Sie im **Lösungspad**mit der rechten Maustaste auf das GTK-Projekt, und wählen Sie **als Startprojekt festlegen**aus.

10. Klicken Sie in der Visual Studio für Mac Symbolleiste auf die Schaltfläche **Start** (die dreieckige Schaltfläche, die einer Wiedergabe Schaltfläche ähnelt), um die APP zu starten

    ![Gtk #-Lebens] Zyklus (gtk-images/mac/gtk-gameoflife.png "Gtk #-Lebens") Zyklus

-----

## <a name="next-steps"></a>Nächste Schritte

### <a name="platform-specifics"></a>Plattformeigenschaften

Sie können die Plattform, auf der Ihre xamarin. Forms-Anwendung ausgeführt wird, über XAML oder Code ermitteln. Dies ermöglicht es Ihnen, die Programm Merkmale zu ändern, wenn Sie in GTK # ausgeführt werden. Vergleichen Sie im Code den Wert von `Device.RuntimePlatform` mit der `Device.GTK` -Konstante (die der Zeichenfolge "gtk" entspricht). Wenn eine Entsprechung vorliegt, wird die Anwendung in GTK # ausgeführt.

In XAML können Sie das `OnPlatform` -Tag verwenden, um einen für die Plattform spezifischen Eigenschafts Wert auszuwählen:

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

Sie können das App-Symbol beim Start festlegen:

```csharp
window.SetApplicationIcon("icon.png");
```

### <a name="themes"></a>Designs

Es gibt eine Vielzahl von Designs, die für gtk # verfügbar sind, und Sie können in einer xamarin. Forms-App verwendet werden:

```csharp
GtkThemes.Init ();
GtkThemes.LoadCustomTheme ("Themes/gtkrc");
```

### <a name="native-forms"></a>Native Formulare

Mithilfe von systemeigenen Formularen können von xamarin. Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage)abgeleitete Seiten von systemeigenen Projekten, einschließlich gtk #-Projekten, genutzt werden. Dies kann erreicht werden, indem eine Instanz der von [`ContentPage`](xref:Xamarin.Forms.ContentPage)abgeleiteten Seite erstellt und mithilfe der `CreateContainer` -Erweiterungsmethode in den systemeigenen gtk #-Typ umgerechnet wird:

```csharp
var settingsView = new SettingsView().CreateContainer();
vbox.PackEnd(settingsView, true, true, 0);
```

Weitere Informationen zu systemeigenen Formularen finden Sie unter [native Forms](~/xamarin-forms/platform/native-forms.md).

## <a name="issues"></a>Probleme

Dies ist eine Vorschauversion, daher sollten Sie davon ausgehen, dass nicht alles in der Produktion bereit ist. Den aktuellen Implementierungs Status finden Sie unter [Status](https://github.com/jsuarezruiz/forms-gtk-progress/blob/master/Status.md)und Informationen zu den aktuellen bekannten Problemen finden Sie unter [ausstehende & bekannte Probleme](https://github.com/jsuarezruiz/forms-gtk-progress/blob/master/Issues-Pending.md).
