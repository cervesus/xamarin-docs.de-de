---
title: Einrichten der WPF-Plattform
description: Xamarin. Forms verfügt jetzt über eine Vorschau Unterstützung für die WPF-Plattform.
ms.prod: xamarin
ms.assetid: 650723F2-4279-4B7B-B0A1-D7F8FF26BF1E
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 04/05/2018
ms.openlocfilehash: 8fcad4799cd53892106b3e221cff0dfbc737e10d
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70760039"
---
# <a name="wpf-platform-setup"></a>Einrichten der WPF-Plattform

![Vorschau](~/media/shared/preview.png)

Xamarin. Forms verfügt jetzt über eine Vorschau Unterstützung für die Windows Presentation Foundation (WPF). In diesem Artikel wird veranschaulicht, wie Sie ein WPF-Projekt zu einer xamarin. Forms-Projekt Mappe hinzufügen.

Bevor Sie beginnen, erstellen Sie eine neue xamarin. Forms-Projekt Mappe in Visual Studio 2019, oder verwenden Sie eine vorhandene xamarin. Forms-Projekt Mappe, z. b. [**boxviewclock**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-boxviewclock). Sie können in Windows nur WPF-apps zu einer xamarin. Forms-Projekt Mappe hinzufügen.

## <a name="add-a-wpf-project-to-a-xamarinforms-app-with-xamarinuniversity"></a>Hinzufügen eines WPF-Projekts zu einer xamarin. Forms-App mit xamarin. University

> [!VIDEO https://youtube.com/embed/Fy9N6OSxK64]

**Video zu xamarin. Forms 3,0 WPF-Unterstützung**

## <a name="adding-a-wpf-app"></a>Hinzufügen einer WPF-App

Befolgen Sie diese Anweisungen zum Hinzufügen einer WPF-App, die auf den Desktops Windows 7, 8 und 10 ausgeführt wird:

1. Klicken Sie in Visual Studio 2019 mit der rechten Maustaste auf den Projektmappennamen im **Projektmappen-Explorer** , und wählen Sie **> Neues Projekt hinzufügen...** aus.

2. Wählen Sie im Fenster **Neues Projekt** auf der linken Seite die Option **Visual C#**  und **klassischer Windows-Desktop**aus. Wählen Sie in der Liste der Projekttypen **WPF-App (.NET Framework)** aus. 

3. Geben Sie einen Namen für das Projekt mit einer **WPF** -Erweiterung ein, z. b. **boxviewclock. WPF**. Klicken Sie auf die Schaltfläche **Durchsuchen** , wählen Sie den Ordner **boxviewclock** aus, und drücken **Sie Ordner auswählen**. Dadurch wird das WPF-Projekt in dasselbe Verzeichnis wie die anderen Projekte in der Projekt Mappe eingefügt.

    ![Neues WPF-Projekt hinzufügen](wpf-images/add-new-project.png "Neues WPF-Projekt hinzufügen")

    Klicken Sie auf OK, um das Projekt zu erstellen.

4. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf das neue Projekt **boxviewclock. WPF** , und wählen Sie **nuget-Pakete verwalten**aus. Wählen Sie die Registerkarte **Durchsuchen** aus, aktivieren Sie das Kontrollkästchen **Vorabversion einschließen** , und suchen Sie nach **xamarin. Forms**.

    ![Nuget-Paket auswählen](wpf-images/select-nuget-package.png "Nuget-Paket auswählen")

    Wählen Sie das Paket aus, und klicken Sie auf die Schaltfläche **Installieren**

5. Suchen Sie nun nach **xamarin. Forms. Platform. WPF** -Paket, und installieren Sie das Paket. Stellen Sie sicher, dass das Paket von Microsoft ist!

6. Klicken Sie mit der rechten Maustaste **Projektmappen-Explorer** auf den Projektmappennamen, und wählen Sie **nuget-Pakete für**Projekt Mappe verwalten Wählen Sie die Registerkarte **Aktualisieren** und das **xamarin. Forms** -Paket aus. Wählen Sie alle Projekte aus, und aktualisieren Sie Sie auf dieselbe xamarin. Forms-Version:

    ![Aktualisieren Sie das nuget-Paket] . (wpf-images/update-nuget-package.png "Aktualisieren Sie das nuget-Paket") . 

7. Klicken Sie im WPF-Projekt mit der rechten Maustaste auf **Verweise**. Wählen Sie im Dialogfeld **Verweis-Manager** die Option **Projekte** auf der linken Seite aus, und aktivieren Sie das Kontrollkästchen neben dem **boxviewclock** -Projekt:

    ![Verweisen auf das freigegebene Projekt](wpf-images/reference-shared-project.png "Verweisen auf das freigegebene Projekt")

8. Bearbeiten Sie die Datei " **MainWindow. XAML** " des WPF-Projekts. Fügen Sie im- TageineXML-NamespaceDeklarationfürdiexamarin.Forms.Platform.WPF-Assemblyundden-Namespacehinzu:`Window`

    ```xaml
    xmlns:wpf="clr-namespace:Xamarin.Forms.Platform.WPF;assembly=Xamarin.Forms.Platform.WPF"
    ```

    Ändern Sie nun `Window` das Tag `wpf:FormsApplicationPage`in. Ändern Sie `Title` die Einstellung in den Namen der Anwendung, z. b. **boxviewclock**. Die abgeschlossene XAML-Datei sollte wie folgt aussehen:

    ```xaml
    <wpf:FormsApplicationPage x:Class="BoxViewClock.WPF.MainWindow"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:wpf="clr-namespace:Xamarin.Forms.Platform.WPF;assembly=Xamarin.Forms.Platform.WPF"
            xmlns:local="clr-namespace:BoxViewClock.WPF"
            mc:Ignorable="d"
            Title="BoxViewClock" Height="450" Width="800">
        <Grid>
        
        </Grid>
    </wpf:FormsApplicationPage>
    ```

9. Bearbeiten Sie die Datei **MainWindow.XAML.cs** des WPF-Projekts. Fügen Sie zwei `using` neue Anweisungen hinzu:

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.WPF;
    ```

    Ändern Sie die Basisklasse `MainWindow` von `Window` von `FormsApplicationPage`in. Fügen Sie `InitializeComponent` nach dem-Befehl die folgenden beiden-Anweisungen hinzu:

    ```csharp
    Forms.Init();
    LoadApplication(new BoxViewClock.App());
    ```
    
    Mit Ausnahme von Kommentaren und `using` nicht verwendeten Direktiven sollte die gesamte **MainWindows.XAML.cs** -Datei wie folgt aussehen:

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

10. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das WPF-Projekt, und wählen Sie **als Startprojekt festlegen**aus. Drücken Sie F5, um das Programm mit dem Visual Studio-Debugger auf dem Windows-Desktop auszuführen:

    ![WPF-boxview-Uhr] (wpf-images/wpf-boxviewclock.png "WPF-boxview-Uhr" )

## <a name="next-steps"></a>Nächste Schritte

### <a name="platform-specifics"></a>Plattformeigenschaften

Die Plattform, auf der Ihre xamarin. Forms-Anwendung ausgeführt wird, können Sie entweder über Code oder XAML ermitteln. Dies ermöglicht es Ihnen, die Programm Merkmale zu ändern, wenn Sie auf WPF ausgeführt werden. Vergleichen Sie im Code den Wert von `Device.RuntimePlatform` mit der `Device.WPF` -Konstante (die der Zeichenfolge "WPF" entspricht). Wenn eine Entsprechung vorliegt, wird die Anwendung auf WPF ausgeführt.

In XAML können Sie das `OnPlatform` -Tag verwenden, um einen für die Plattform spezifischen Eigenschafts Wert auszuwählen:

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

### <a name="window-size"></a>Fenstergröße

Sie können die anfängliche Größe des Fensters in der WPF-Datei **MainWindow. XAML** anpassen:

```xaml
Title="BoxViewClock" Height="450" Width="800"
```

## <a name="issues"></a>Issues

Dies ist eine Vorschauversion, daher sollten Sie davon ausgehen, dass nicht alles in der Produktion bereit ist. Nicht alle nuget-Pakete für xamarin. Forms sind für WPF bereit, und einige Features funktionieren möglicherweise nicht vollständig.
