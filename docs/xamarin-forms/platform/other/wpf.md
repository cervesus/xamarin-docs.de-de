---
title: Einrichten der WPF-Plattform
description: Xamarin.Formsverfügt über eine Vorschau Unterstützung für die WPF-Plattform.
ms.prod: xamarin
ms.assetid: 650723F2-4279-4B7B-B0A1-D7F8FF26BF1E
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/20/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 774ae964643b9b78f424d96b3dd382f244205dcf
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84946324"
---
# <a name="wpf-platform-setup"></a>Einrichten der WPF-Plattform

![Vorschau](~/media/shared/preview.png)

Xamarin.Formsverfügt über eine Vorschau Unterstützung für die Windows Presentation Foundation (WPF) auf .NET Framework und .net Core 3. In diesem Artikel wird veranschaulicht, wie einer Projekt Mappe ein WPF-Projekt hinzugefügt wird, das .NET Framework als Ziel hat Xamarin.Forms .

> [!IMPORTANT]
> Xamarin.Formsdie Unterstützung für WPF wird von der Community bereitgestellt. Weitere Informationen finden Sie [ Xamarin.Forms unter Platt Form Unterstützung](https://github.com/xamarin/Xamarin.Forms/wiki/Platform-Support).

Bevor Sie beginnen, erstellen Sie eine neue Projekt Mappe Xamarin.Forms in Visual Studio 2019, oder verwenden Sie eine vorhandene Projekt Mappe Xamarin.Forms , z. b. [**boxviewclock**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-boxviewclock). Sie können WPF-Apps nur in Windows zu einer Projekt Mappe hinzufügen Xamarin.Forms .

## <a name="add-a-wpf-application"></a>Hinzufügen einer WPF-Anwendung

Befolgen Sie diese Anweisungen, um eine WPF-Anwendung hinzuzufügen, die auf den Desktops Windows 7, 8 und 10 ausgeführt werden kann:

1. Klicken Sie in Visual Studio 2019 mit der rechten Maustaste auf den Projektmappennamen im **Projektmappen-Explorer** , und wählen Sie **> neues Projekt hinzufügen...** aus.

2. Wählen Sie im Fenster **Neues Projekt hinzufügen** in der Dropdown-Dropdown-Dropdown- **Datei die Option** **c#** aus, wählen Sie in der **Dropdown** -Dropdown **Fläche** **Plattformen** die Option **Windows** aus, und wählen Sie Wählen Sie in der Liste der Projekttypen **WPF-App (.NET Framework)** aus:

    ![Neues WPF-Projekt hinzufügen](wpf-images/add-project.png "Neues WPF-Projekt hinzufügen")

    Klicken Sie auf die Schaltfläche **weiter** .

    > [!NOTE]
    > Xamarin.Forms4,7 bietet Unterstützung für WPF-apps, die unter .net Core 3 ausgeführt werden.

3. Geben Sie im Fenster **Neues Projekt konfigurieren** einen Namen für das Projekt mit einer **WPF** -Erweiterung ein, z. b. **boxviewclock. WPF**. Klicken Sie auf die Schaltfläche **Durchsuchen** , wählen Sie den Ordner **boxviewclock** aus, und klicken **Sie auf Ordner auswählen** , um das WPF-Projekt in demselben Verzeichnis wie die anderen Projekte in der Projekt Mappe zu platzieren:

    ![Neues WPF-Projekt hinzufügen](wpf-images/configure-project.png "Neues WPF-Projekt hinzufügen")

    Klicken Sie auf die Schaltfläche **Erstellen** , um das Projekt zu erstellen.

4. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf das neue Projekt **boxviewclock. WPF** , und wählen Sie **nuget-Pakete verwalten...** aus. Wählen Sie die Registerkarte **Durchsuchen** aus, und suchen Sie nach ** Xamarin.Forms . Platform. WPF**:

    ![Nuget-Paket auswählen](wpf-images/select-nuget-package.png "Nuget-Paket auswählen")

    Wählen Sie das Paket, und klicken Sie auf die Schaltfläche **Installieren** .

5. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektmappennamen, und wählen Sie **nuget-Pakete für Projekt Mappe verwalten aus**. Wählen Sie die Registerkarte **Updates** , und wählen Sie dann das **Xamarin.Forms** Paket aus. Wählen Sie alle Projekte aus, und aktualisieren Sie Sie auf die gleiche Xamarin.Forms Version:

    ![Aktualisieren Sie das nuget-Paket.](wpf-images/update-nuget-package.png "Aktualisieren Sie das nuget-Paket.")

6. Klicken Sie im WPF-Projekt mit der rechten Maustaste auf **Verweise** , und wählen Sie **Verweis hinzufügen...** aus. Wählen Sie im Dialogfeld **Verweis-Manager** die Option **Projekte** auf der linken Seite aus, und aktivieren Sie das Kontrollkästchen neben dem **boxviewclock** -Projekt:

    ![Verweisen auf das freigegebene Projekt](wpf-images/reference-shared-project.png "Verweisen auf das freigegebene Projekt")

    Klicken Sie auf die Schaltfläche **OK** .

7. Bearbeiten Sie die Datei " **MainWindow. XAML** " des WPF-Projekts. `Window`Fügen Sie im-Tag eine XML-Namespace Deklaration für das hinzu ** Xamarin.Forms . Platform. WPF** -Assembly und-Namespace:

    ```xaml
    xmlns:wpf="clr-namespace:Xamarin.Forms.Platform.WPF;assembly=Xamarin.Forms.Platform.WPF"
    ```

    Ändern Sie nun das `Window` Tag in `wpf:FormsApplicationPage` . Ändern `Title` Sie die Einstellung in den Namen der Anwendung, z. b. **boxviewclock**. Die abgeschlossene XAML-Datei sollte wie folgt aussehen:

    ```xaml
    <wpf:FormsApplicationPage x:Class="BoxViewClock.WPF.MainWindow"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:local="clr-namespace:BoxViewClock.WPF"
            xmlns:wpf="clr-namespace:Xamarin.Forms.Platform.WPF;assembly=Xamarin.Forms.Platform.WPF"            
            mc:Ignorable="d"
            Title="BoxViewClock" Height="450" Width="800">
        <Grid>

        </Grid>
    </wpf:FormsApplicationPage>
    ```

8. Bearbeiten Sie die Datei **MainWindow.XAML.cs** des WPF-Projekts. Fügen Sie zwei neue `using` Anweisungen hinzu:

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.WPF;
    ```

    Ändern Sie die Basisklasse von `MainWindow` von `Window` in `FormsApplicationPage` . Fügen Sie nach dem-Befehl `InitializeComponent` die folgenden beiden-Anweisungen hinzu:

    ```csharp
    Forms.Init();
    LoadApplication(new BoxViewClock.App());
    ```

    Mit Ausnahme von Kommentaren und nicht verwendeten `using` Direktiven sollte die gesamte **MainWindows.XAML.cs** -Datei wie folgt aussehen:

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.WPF;

    namespace BoxViewClock.WPF
    {
        public partial class MainWindow : FormsApplicationPage
        {
            public MainWindow()
            {
                InitializeComponent();

                Forms.Init();
                LoadApplication(new BoxViewClock.App());
            }
        }
    }
    ```

9. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das WPF-Projekt, und wählen Sie **als Startprojekt festlegen**aus. Drücken Sie F5, um das Programm mit dem Visual Studio-Debugger auf dem Windows-Desktop auszuführen:

    ![WPF-boxview-Uhr](wpf-images/wpf-boxviewclock.png "WPF-boxview-Uhr" )

## <a name="platform-specifics"></a>Platt Form Besonderheiten

Sie können mithilfe Xamarin.Forms von Code oder XAML ermitteln, auf welcher Plattform die Anwendung ausgeführt wird. Dies ermöglicht es Ihnen, die Programm Merkmale zu ändern, wenn Sie auf WPF ausgeführt werden. Vergleichen Sie im Code den Wert von `Device.RuntimePlatform` mit der- `Device.WPF` Konstante (die der Zeichenfolge "WPF" entspricht). Wenn eine Entsprechung vorliegt, wird die Anwendung auf WPF ausgeführt.

In XAML können Sie das-Tag verwenden, `OnPlatform` um einen für die Plattform spezifischen Eigenschafts Wert auszuwählen:

```xaml
<Button.TextColor>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="White" />
        <On Platform="macOS" Value="White" />
        <On Platform="Android" Value="Black" />
        <On Platform="WPF" Value="Blue" />
    </OnPlatform>
</Button.TextColor>
```

## <a name="window-size"></a>Fenstergröße

Sie können die anfängliche Größe des Fensters in der WPF-Datei **MainWindow. XAML** anpassen:

```xaml
Title="BoxViewClock" Height="450" Width="800"
```

## <a name="issues"></a>Probleme

Dies ist eine Vorschauversion, daher sollten Sie davon ausgehen, dass nicht alles in der Produktion bereit ist. Nicht alle nuget-Pakete für Xamarin.Forms sind für WPF bereit, und einige Features funktionieren möglicherweise nicht vollständig.

## <a name="related-video"></a>Zugehörige Videos

> [!VIDEO https://youtube.com/embed/Fy9N6OSxK64]

**Xamarin.Forms3,0 WPF-Unterstützungs Video**
