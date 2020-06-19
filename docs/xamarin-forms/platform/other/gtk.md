---
title: 'GTK #-Platt Form Einrichtung'
description: 'Xamarin.Formsbietet jetzt eine Vorschau Unterstützung für die GTK #-Plattform.'
ms.prod: xamarin
ms.assetid: 3417FB95-3E4B-47DA-85D0-F34832747236
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a5635da9f7c083609ce1e0f120d0613fff9bd77b
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84198105"
---
# <a name="gtk-platform-setup"></a>GTK #-Platt Form Einrichtung

![Vorschau](~/media/shared/preview.png)

Xamarin.Formsbietet jetzt eine Vorschau Unterstützung für gtk #-apps. GTK # ist ein grafisches Toolkit für die Benutzeroberfläche, das das GTK +-Toolkit und eine Vielzahl von GNOME-Bibliotheken verknüpft und die Entwicklung von vollständig nativen gnome-Grafik-apps mit Mono und .NET ermöglicht. In diesem Artikel wird veranschaulicht, wie Sie ein GTK #-Projekt zu einer Projekt Mappe hinzufügen Xamarin.Forms .

> [!IMPORTANT]
> Xamarin.Formsdie Unterstützung für gtk # wird von der Community bereitgestellt. Weitere Informationen finden Sie [ Xamarin.Forms unter Platt Form Unterstützung](https://github.com/xamarin/Xamarin.Forms/wiki/Platform-Support).

Bevor Sie beginnen, erstellen Sie eine neue Projekt Mappe Xamarin.Forms , oder verwenden Sie eine vorhandene Xamarin.Forms Lösung, z. b. [**gameololife**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-gameoflife).

> [!NOTE]
> Dieser Artikel konzentriert sich auf das Hinzufügen einer GTK #-APP zu einer Xamarin.Forms Lösung in VS2017 und Visual Studio für Mac und kann auch in [monodevelop](https://www.monodevelop.com/) für Linux ausgeführt werden.

## <a name="adding-a-gtk-app"></a>Hinzufügen einer GTK #-App

GTK # für macOS und Linux wird als Teil von [Mono](https://www.mono-project.com/download/stable/)installiert. GTK # für .net kann unter Windows mit dem [gtk #-Installer](https://www.mono-project.com/download/stable/#download-win)installiert werden.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Befolgen Sie diese Anweisungen zum Hinzufügen einer GTK #-APP, die auf dem Windows-Desktop ausgeführt wird:

1. Klicken Sie in Visual Studio 2019 in **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektmappennamen, und wählen Sie **> neues Projekt hinzufügen...** aus.

2. Wählen Sie Links im Fenster **Neues Projekt** die Option **Visual c#** und **klassischer Windows-Desktop**aus. Wählen Sie in der Liste der Projekttypen die Option **Klassenbibliothek (.NET Framework)** aus, und stellen Sie sicher, dass die Dropdown Liste für das **Framework** auf mindestens .NET Framework 4,7 festgelegt ist.

3. Geben Sie einen Namen für das Projekt mit einer **gtk** -Erweiterung ein, z. b. **gameof Life. GTK**. Klicken Sie auf die Schaltfläche **Durchsuchen** , wählen Sie den Ordner mit den anderen Platt Form Projekten aus, und drücken **Sie Ordner auswählen**. Dadurch wird das GTK-Projekt in dasselbe Verzeichnis wie die anderen Projekte in der Projekt Mappe eingefügt.

    ![Hinzufügen eines neuen gtk-Projekts](gtk-images/win/add-new-project.png "Hinzufügen eines neuen gtk-Projekts")

    Klicken Sie auf die Schaltfläche **OK** , um das Projekt zu erstellen.

4. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf das neue gtk-Projekt, und wählen Sie **nuget-Pakete verwalten**. Wählen Sie die Registerkarte **Durchsuchen** aus, und suchen Sie nach **Xamarin.Forms** 3,0 oder höher.

    ![Xamarin.FormsNuget-Paket auswählen](gtk-images/win/select-forms-nuget-package.png "Wählen Sie [! Schel. No-Loc (xamarin. Forms)] nuget-Paket")

    Wählen Sie das Paket, und klicken Sie auf die Schaltfläche **Installieren** .

5. Suchen Sie jetzt nach ** Xamarin.Forms . Platform. GTK** 3,0-Paket oder höher.

    ![Wählen Sie aus Xamarin.Forms . Platform. GTK nuget-Paket](gtk-images/win/select-forms-platform-nuget-package.png "Wählen Sie [! Schel. No-Loc (xamarin. Forms)]. Platform. GTK nuget-Paket")

    Wählen Sie das Paket, und klicken Sie auf die Schaltfläche **Installieren** .

6. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf den Projektmappennamen, und wählen Sie **nuget-Pakete für**Projekt Mappe verwalten. Wählen Sie die Registerkarte **Aktualisieren** und das **Xamarin.Forms** Paket aus. Wählen Sie alle Projekte aus, und aktualisieren Sie Sie auf die Version, die Xamarin.Forms vom gtk-Projekt verwendet wird.

7. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf **Verweise** im gtk-Projekt. Wählen Sie im Dialogfeld **Verweis-Manager** die Option **Projekte** auf der linken Seite aus, und aktivieren Sie das Kontrollkästchen neben dem .NET Standard oder dem freigegebenen Projekt:

    ![Verweisen auf das freigegebene Projekt](gtk-images/win/reference-shared-project.png "Verweisen auf das freigegebene Projekt")

8. Klicken Sie im Dialogfeld **Verweis-Manager** auf die Schaltfläche **Durchsuchen** , navigieren Sie zum Ordner **c:\Programme (x86) \gtksharp\2.12\lib** , und wählen Sie die **atk-sharp.dll**, **gdk-sharp.dll**, **glade-sharp.dll**, **glib-sharp.dll**, **gtk-dotnet.dll** **gtk-sharp.dllDateien aus** .

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

    Dieser Code initialisiert GTK # und Xamarin.Forms , erstellt ein Anwendungsfenster und führt die APP aus.

11. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf das GTK-Projekt, und wählen Sie **Eigenschaften**.

12. Wählen Sie im Fenster **Eigenschaften** die Registerkarte **Anwendung** aus, und ändern Sie das Dropdown Feld **Ausgabetyp** in **Windows-Anwendung**.

    ![Ändern des Projekt Ausgabe Typs](gtk-images/win/change-project-output-type.png "Ändern des Projekt Ausgabe Typs")

13. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf das GTK-Projekt, und wählen Sie **als Startprojekt festlegen**aus. Drücken Sie F5, um das Programm mit dem Visual Studio-Debugger auf dem Windows-Desktop auszuführen:

    ![GTK #-Lebenszyklus](gtk-images/win/gtk-gameoflife.png "GTK #-Lebenszyklus")

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

Befolgen Sie diese Anweisungen zum Hinzufügen einer GTK #-APP, die auf dem Mac-Desktop ausgeführt wird:

1. Klicken Sie in Visual Studio für Mac mit der rechten Maustaste auf die Projekt Mappe, Xamarin.Forms und wählen Sie **Hinzufügen > neues Projekt hinzufügen...** aus.

2. Wählen Sie im Fenster **Neues Projekt** die Option **andere > .net > gtk # 2,0-Projekt** aus, und klicken Sie auf **weiter**.

3. Geben Sie einen Namen für das Projekt mit einer **gtk** -Erweiterung ein, z **. b. gameof Life. GTK**, und klicken Sie auf **Erstellen**.

4. Klicken Sie im **Lösungspad**mit der rechten Maustaste auf **Pakete, > Pakete hinzuzufügen...** für das GTK-Projekt, und fügen Sie das Xamarin.Forms nuget-Paket mit der Version 3,0 oder höher hinzu.

    ![Xamarin.FormsNuget-Paket auswählen](gtk-images/mac/select-forms-nuget-package.png "Wählen Sie [! Schel. No-Loc (xamarin. Forms)] nuget-Paket")

5. Klicken Sie im **Lösungspad**mit der rechten Maustaste auf **Pakete, > Pakete hinzuzufügen...** für das GTK-Projekt, und fügen Sie hinzu Xamarin.Forms . Platform. gtk 3,0 Vorabversion des nuget-Pakets oder höher.

    ![Wählen Sie aus Xamarin.Forms . Platform. GTK nuget-Paket](gtk-images/mac/select-forms-platform-nuget-package.png "Wählen Sie [! Schel. No-Loc (xamarin. Forms)]. Platform. GTK nuget-Paket")

6. Aktualisieren Sie die anderen Platt Form Projekte so, dass Sie die gleiche Version verwenden, die Xamarin.Forms vom gtk-Projekt verwendet wird.

7. Klicken Sie im **Lösungspad**mit der rechten Maustaste auf **Verweise > bearbeiten Sie Verweise...** für das GTK-Projekt, und fügen Sie einen Verweis auf das Xamarin.Forms Projekt hinzu (entweder .NET Standard oder frei gegebenes Projekt).

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

    Dieser Code initialisiert GTK # und Xamarin.Forms , erstellt ein Anwendungsfenster und führt die APP aus.

9. Klicken Sie im **Lösungspad**mit der rechten Maustaste auf das GTK-Projekt, und wählen Sie **als Startprojekt festlegen**aus.

10. Klicken Sie in der Visual Studio für Mac Symbolleiste auf die Schaltfläche **Start** (die dreieckige Schaltfläche, die einer Wiedergabe Schaltfläche ähnelt), um die APP zu starten

    ![GTK #-Lebenszyklus](gtk-images/mac/gtk-gameoflife.png "GTK #-Lebenszyklus")

-----

## <a name="next-steps"></a>Nächste Schritte

### <a name="platform-specifics"></a>Plattformeigenschaften

Mithilfe Xamarin.Forms von XAML oder Code können Sie ermitteln, auf welcher Plattform die Anwendung ausgeführt wird. Dies ermöglicht es Ihnen, die Programm Merkmale zu ändern, wenn Sie in GTK # ausgeführt werden. Vergleichen Sie im Code den Wert von `Device.RuntimePlatform` mit der- `Device.GTK` Konstante (die der Zeichenfolge "gtk" entspricht). Wenn eine Entsprechung vorliegt, wird die Anwendung in GTK # ausgeführt.

In XAML können Sie das-Tag verwenden, `OnPlatform` um einen für die Plattform spezifischen Eigenschafts Wert auszuwählen:

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

Es gibt eine Vielzahl von Designs, die für gtk # verfügbar sind, und Sie können von einer-App verwendet werden Xamarin.Forms :

```csharp
GtkThemes.Init ();
GtkThemes.LoadCustomTheme ("Themes/gtkrc");
```

### <a name="native-forms"></a>Native Formulare

Mithilfe von systemeigenen Formularen können Xamarin.Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage) von abgeleitete Seiten von systemeigenen Projekten, einschließlich gtk #-Projekten, genutzt werden. Dies kann erreicht werden, indem eine Instanz der von [`ContentPage`](xref:Xamarin.Forms.ContentPage) abgeleiteten Seite erstellt und mithilfe der-Erweiterungsmethode in den systemeigenen gtk #-Typ umgerechnet wird `CreateContainer` :

```csharp
var settingsView = new SettingsView().CreateContainer();
vbox.PackEnd(settingsView, true, true, 0);
```

Weitere Informationen zu systemeigenen Formularen finden Sie unter [native Forms](~/xamarin-forms/platform/native-forms.md).

## <a name="issues"></a>Probleme

Dies ist eine Vorschauversion, daher sollten Sie davon ausgehen, dass nicht alles in der Produktion bereit ist. Den aktuellen Implementierungs Status finden Sie unter [Status](https://github.com/jsuarezruiz/forms-gtk-progress/blob/master/Status.md)und Informationen zu den aktuellen bekannten Problemen finden Sie unter [ausstehende & bekannte Probleme](https://github.com/jsuarezruiz/forms-gtk-progress/blob/master/Issues-Pending.md).
