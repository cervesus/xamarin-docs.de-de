---
title: WPF-Plattform-Setup
description: Xamarin.Forms verfügt jetzt über eine vorschauunterstützung für die WPF-Plattform
ms.prod: xamarin
ms.assetid: 650723F2-4279-4B7B-B0A1-D7F8FF26BF1E
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 04/05/2018
ms.openlocfilehash: 2bef13e7f465dd213649f88deb572eb661895250
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61245511"
---
# <a name="wpf-platform-setup"></a>WPF-Plattform-Setup

![Vorschau](~/media/shared/preview.png)

Xamarin.Forms verfügt jetzt über eine vorschauunterstützung für die Windows Presentation Foundation (WPF). In diesem Artikel wird veranschaulicht, wie eine Xamarin.Forms-Projektmappe ein WPF-Projekt hinzugefügt wird.

Bevor Sie beginnen, erstellen eine neue Xamarin.Forms-Projektmappe in Visual Studio-2019, oder eine vorhandene Xamarin.Forms-Projektmappe, z. B. verwenden [ **BoxViewClock**](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock/). Sie können einer Xamarin.Forms-Lösung in Windows nur WPF-apps hinzufügen.

## <a name="add-a-wpf-project-to-a-xamarinforms-app-with-xamarinuniversity"></a>Fügen Sie ein WPF-Projekt zu einer Xamarin.Forms-app mit Xamarin.University

> [!VIDEO https://youtube.com/embed/Fy9N6OSxK64]

**Xamarin.Forms 3.0 WPF unterstützen, indem [Xamarin University](https://university.xamarin.com/)**

## <a name="adding-a-wpf-app"></a>Hinzufügen einer WPF-App

Um eine WPF-app hinzufügen, die auf den Windows 7, 8 und 10-Desktops ausgeführt wird, gehen Sie wie folgt vor:

1. Visual Studio-2019, mit der Maustaste auf den Namen der Projektmappe in die **Projektmappen-Explorer** , und wählen Sie **hinzufügen > Neues Projekt...** .

2. In der **neues Projekt** Fenster auf der linken Select **Visual C#-** und **Klassischer Windows-Desktop**. Wählen Sie in der Liste der Projekttypen, **WPF-App ((.NET Framework)**. 

3. Geben Sie einen Namen für das Projekt mit einem **WPF** -Erweiterung, z. B. **BoxViewClock.WPF**. Klicken Sie auf die **Durchsuchen** Klicken die **BoxViewClock** Ordner, und drücken Sie **Ordner auswählen**. Dies versetzt das WPF-Projekt in demselben Verzeichnis wie die anderen Projekte in der Projektmappe.

    ![Hinzufügen eines neuen WPF-Projekts](wpf-images/add-new-project.png "Hinzufügen eines neuen WPF-Projekts")

    Klicken Sie auf OK, um das Projekt zu erstellen.

4. In der **Projektmappen-Explorer**, klicken Sie rechts auf die neue **BoxViewClock.WPF** Projekt, und wählen **NuGet-Pakete verwalten**. Wählen Sie die **Durchsuchen** Registerkarte, klicken Sie auf die **Vorabversion einbeziehen** Kontrollkästchen, und suchen Sie nach **Xamarin.Forms**.

    ![Wählen Sie das NuGet-Paket](wpf-images/select-nuget-package.png "wählen Sie das NuGet-Paket")

    Wählen Sie die packen, und klicken Sie auf die **installieren** Schaltfläche.

5. Suchen Sie nach **Xamarin.Forms.Platform.WPF** Packen und installieren Sie auch, dass ein. Stellen Sie sicher, dass das Paket von Microsoft ist!

6. Klicken Sie mit der rechten Maustaste auf den Namen der Projektmappe in die **Projektmappen-Explorer** , und wählen Sie **NuGet-Pakete für Projektmappe verwalten**. Wählen Sie die **Update** Registerkarte und der **Xamarin.Forms** Paket. Wählen Sie alle Projekte aus, und aktualisieren Sie diese auf die gleiche Version von Xamarin.Forms:

    ![Aktualisieren Sie das NuGet-Paket](wpf-images/update-nuget-package.png "aktualisieren Sie das NuGet-Paket") 

7. Die WPF-Projekt mit der Maustaste auf **Verweise**. In der **Verweis-Manager** wählen Sie im Dialogfeld **Projekte** an der linken Seite, und aktivieren Sie das Kontrollkästchen neben den **BoxViewClock** Projekt:

    ![Verweisen auf das freigegebene Projekt](wpf-images/reference-shared-project.png "verweisen auf das freigegebene Projekt")

8. Bearbeiten der **"MainWindow.xaml"** Datei mit dem WPF-Projekt. In der `Window` markieren, fügen Sie eine XML-Namespacedeklaration für das **Xamarin.Forms.Platform.WPF** Assembly und den Namespace:

    ```xaml
    xmlns:wpf="clr-namespace:Xamarin.Forms.Platform.WPF;assembly=Xamarin.Forms.Platform.WPF"
    ```

    Ändern Sie nun die `Window` -Tag in `wpf:FormsApplicationPage`. Ändern der `Title` festlegen, die auf den Namen der Anwendung, z. B. **BoxViewClock**. Die vollständige XAML-Datei sollte folgendermaßen aussehen:

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

9. Bearbeiten der **"MainWindow.Xaml.cs"** Datei mit dem WPF-Projekt. Hinzufügen von zwei neuen `using` Anweisungen:

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.WPF;
    ```

    Ändern Sie die Basisklasse der `MainWindow` aus `Window` zu `FormsApplicationPage`. Nach der `InitializeComponent` aufzurufen, fügen Sie die folgenden beiden Anweisungen hinzu:

    ```csharp
    Forms.Init();
    LoadApplication(new BoxViewClock.App());
    ```
    
    Mit Ausnahme von Kommentaren und nicht verwendete `using` Seitendirektiven festgelegt, und die vollständige **"MainWindow.Xaml.cs"** Datei sollte folgendermaßen aussehen:

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

10. Mit der rechten Maustaste in des WPF-Projekts in der **Projektmappen-Explorer** , und wählen Sie **als Startprojekt festlegen**. Drücken Sie F5, um das Programm mit Visual Studio-Debugger auf dem Windows-Desktop ausgeführt werden soll:

    ![WPF-BoxView Uhr](wpf-images/wpf-boxviewclock.png "BoxView WPF-Uhr" )

## <a name="next-steps"></a>Nächste Schritte

### <a name="platform-specifics"></a>Plattformeigenschaften

Sie können bestimmen, welche Plattform Ihrer Xamarin.Forms-Anwendung über Code oder XAML ausgeführt wird. Dadurch können Sie auf Programm Merkmale zu ändern, wenn es auf WPF ausgeführt wird. Im Code vergleichen Sie den Wert von `Device.RuntimePlatform` mit der `Device.WPF` -Konstante (die die Zeichenfolge "WPF" ist gleich). Wenn eine Übereinstimmung vorliegt, wird die Anwendung auf WPF ausgeführt.

In XAML, können Sie mithilfe der `OnPlatform` Tag, um einen Eigenschaftswert für die Plattform auswählen:

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

Sie können die ursprüngliche Größe des Fensters in WPF-Anwendungen anpassen **"MainWindow.xaml"** Datei:

```xaml
Title="BoxViewClock" Height="450" Width="800"
```

## <a name="issues"></a>Probleme

Dies ist eine Vorschau, damit Sie erwarten, dass nicht alles bereit für die Produktion. Nicht alle NuGet-Pakete für Xamarin.Forms für WPF bereit sind, und einige Features möglicherweise nicht vollständig ausgeführt.

