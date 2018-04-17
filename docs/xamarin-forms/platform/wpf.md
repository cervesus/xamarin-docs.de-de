---
title: WPF-Plattform-Setup
description: Xamarin.Forms verfügt jetzt über Unterstützung für die WPF-Plattform
ms.prod: xamarin
ms.assetid: 650723F2-4279-4B7B-B0A1-D7F8FF26BF1E
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 04/05/2018
ms.openlocfilehash: 51aad1643709a96c56ccad8187a53f47a65a9dac
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="wpf-platform-setup"></a>WPF-Plattform-Setup

![Vorschau](~/media/shared/preview.png)

Xamarin.Forms verfügt jetzt über Unterstützung für Windows Presentation Foundation (WPF). In diesem Artikel wird veranschaulicht, wie einer Xamarin.Forms-Projektmappe ein WPF-Projekt hinzugefügt.

Bevor Sie starten, erstellen eine neue Xamarin.Forms-Projektmappe in Visual Studio 2017 oder verwenden Sie eine vorhandene Xamarin.Forms-Projektmappe z. B. [ **BoxViewClock**](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock/). Sie können nur WPF-apps zu einer Xamarin.Forms-Projektmappe in Windows hinzufügen.

## <a name="adding-a-wpf-app"></a>Hinzufügen von WPF-App

WPF-app hinzu, die auf dem Windows 7, 8 und 10-Desktops ausgeführt wird, gehen Sie wie folgt vor:

1. Visual Studio-2017, mit der Maustaste auf den Namen der Projektmappe in der **Projektmappen-Explorer** , und wählen Sie **hinzufügen > Neues Projekt...** .

2. In der **neues Projekt** Fenster auf der linken Select **Visual C#-** und **klassische Windows-Desktop**. Wählen Sie in der Liste der Projekttypen **WPF-App ((.NET Framework)**. 

3. Geben Sie einen Namen für das Projekt mit einem **WPF** Erweiterung, z. B. **BoxViewClock.WPF**. Klicken Sie auf die **Durchsuchen** auswählen die **BoxViewClock** Ordner, und drücken Sie **Ordner auswählen**. Dadurch wird das WPF-Projekt im selben Verzeichnis wie die anderen Projekte in der Projektmappe wurde.

    ![Ein neues WPF-Projekt hinzufügen](wpf-images/add-new-project.png "ein neues WPF-Projekt hinzufügen")

    Klicken Sie auf OK, um das Projekt zu erstellen.

4. In der **Projektmappen-Explorer**, klicken Sie rechts auf die neue **BoxViewClock.WPF** Projekt, und wählen Sie **NuGet-Pakete verwalten**. Wählen Sie die **Durchsuchen** Registerkarte, klicken Sie auf die **Vorabversion einschließen** Kontrollkästchen, und suchen Sie nach **Xamarin.Forms**.

    ![Wählen Sie das NuGet-Paket](wpf-images/select-nuget-package.png "wählen Sie das NuGet-Paket")

    Wählen Sie die Verpacken, und klicken Sie auf die **installieren** Schaltfläche.

5. Suchen Sie nach **Xamarin.Forms.Platform.WPF** Packen und installieren Sie auch, dass ein. Stellen Sie sicher, dass das Paket von Microsoft ist!

6. Klicken Sie mit der mit der rechten Maustaste auf den Namen der Projektmappe in der **Projektmappen-Explorer** , und wählen Sie **NuGet-Pakete für Projektmappe verwalten**. Wählen Sie die **Update** Registerkarte und der **Xamarin.Forms** Paket. Alle Projekte auswählen und auf die gleiche Xamarin.Forms-Version zu aktualisieren:

    ![Aktualisieren Sie das NuGet-Paket](wpf-images/update-nuget-package.png "aktualisieren Sie das NuGet-Paket") 

7. Das WPF-Projekt mit der Maustaste auf **Verweise**. In der **Verweis-Manager** wählen Sie im Dialogfeld **Projekte** auf der linken und aktivieren Sie das Kontrollkästchen neben den **BoxViewClock** Projekt:

    ![Verweisen auf das freigegebene Projekt](wpf-images/reference-shared-project.png "verweisen auf das freigegebene Projekt")

8. Bearbeiten der **"MainWindow.xaml"** -Datei des WPF-Projekt. In der `Window` zu kennzeichnen, fügen Sie eine XML-Namespacedeklaration für das **Xamarin.Forms.Platform.WPF** Assembly und den Namespace:

    ```xaml
    xmlns:wpf="clr-namespace:Xamarin.Forms.Platform.WPF;assembly=Xamarin.Forms.Platform.WPF"
    ```

    Ändern Sie nun die `Window` tag `wpf:FormsApplcationPage`. Ändern der `Title` festlegen auf den Namen der Anwendung, z. B. **BoxViewClock**. Die vollständige Verwendung von XAML-Datei sollte wie folgt aussehen:

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

9. Bearbeiten der **"MainWindow.Xaml.cs"** -Datei des WPF-Projekt. Fügen Sie zwei neuen `using` Direktiven:

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.WPF;
    ```

    Ändern Sie die Basisklasse der `MainWindow` aus `Window` auf `FormsApplicationPage`. Nach der `InitializeComponent` aufrufen, fügen Sie die folgenden beiden Anweisungen hinzu:

    ```csharp
    Forms.Init();
    LoadApplication(new BoxViewClock.App());
    ```
    
    Mit Ausnahme von Kommentaren und nicht verwendete `using` Direktiven, die vollständige **"MainWindows.Xaml.cs"** Datei sollte wie folgt aussehen:

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

10. Mit der rechten Maustaste des WPF-Projekts in der **Projektmappen-Explorer** , und wählen Sie **als Startprojekt festlegen**. Drücken Sie F5, um das Programm auf dem Windows-Desktop mit dem Visual Studio-Debugger ausgeführt:

    ![WPF-BoxView Uhr](wpf-images/wpf-boxviewclock.png "WPF BoxView Uhr" )

## <a name="next-steps"></a>Nächste Schritte

### <a name="platform-specifics"></a>Platform-Besonderheiten

Sie können bestimmen, welche Plattform Xamarin.Forms von Code oder in XAML zur Ausführung Ihrer Anwendung auf. Dadurch können Sie die Merkmale der Anwendung zu ändern, wenn er auf WPF ausgeführt wird. Im Code, vergleichen Sie den Wert der `Device.RuntimePlatform` mit der `Device.WPF` -Konstante (die die Zeichenfolge "WPF" ist gleich). Wenn eine Übereinstimmung vorhanden ist, wird die Anwendung auf WPF ausgeführt.

In XAML können Sie die `OnPlatform` Tag um einen Eigenschaftswert, der spezifisch für die Plattform auszuwählen:

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

## <a name="issues"></a>Schwierigkeiten

Dies ist eine Vorschau, damit Sie erwarten, dass nicht alles, was die Produktion bereit ist. Nicht alle NuGet-Pakete für Xamarin.Forms für WPF bereit sind, und einige Features möglicherweise nicht voll funktionsfähig.

