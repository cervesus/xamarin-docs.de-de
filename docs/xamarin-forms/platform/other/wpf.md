---
title: WPF-Plattform-Setup
description: Xamarin.Forms bietet jetzt Vorschauunterstützung für die WPF-Plattform
ms.prod: xamarin
ms.assetid: 650723F2-4279-4B7B-B0A1-D7F8FF26BF1E
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 04/09/2020
ms.openlocfilehash: c9ec9fec2391d7d7a24f97f2ec20208a7d69dbc1
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2020
ms.locfileid: "80992369"
---
# <a name="wpf-platform-setup"></a>WPF-Plattform-Setup

![Vorschau](~/media/shared/preview.png)

Xamarin.Forms bietet jetzt Vorschauunterstützung für die Windows Presentation Foundation (WPF). In diesem Artikel wird veranschaulicht, wie Sie einer Xamarin.Forms-Lösung ein WPF-Projekt hinzufügen.

> [!IMPORTANT]
> Xamarin.Forms-Unterstützung für WPF wird von der Community bereitgestellt. Weitere Informationen finden Sie unter [Xamarin.Forms Platform Support](https://github.com/xamarin/Xamarin.Forms/wiki/Platform-Support).

Bevor Sie beginnen, erstellen Sie eine neue Xamarin.Forms-Lösung in Visual Studio 2019, oder verwenden Sie eine vorhandene Xamarin.Forms-Lösung, z. B. [**BoxViewClock**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-boxviewclock). Sie können WPF-Apps nur zu einer Xamarin.Forms-Lösung in Windows hinzufügen.

## <a name="add-a-wpf-application"></a>Hinzufügen einer WPF-Anwendung

Befolgen Sie diese Anweisungen, um eine WPF-Anwendung hinzuzufügen, die auf Windows 7, 8 und 10 Desktops ausgeführt wird:

1. Klicken Sie in Visual Studio 2019 mit der rechten Maustaste auf den Projektmappennamen im **Projektmappen-Explorer,** und wählen Sie **> Neues Projekt hinzufügen...**.

2. Wählen Sie im Fenster **Neues Projekt hinzufügen** im Dropdown-Menü **"Sprachen"** die Option **"C"** aus, wählen Sie **Windows** im **Dropdown-Menü "Plattformen"** und im Dropdown-Menü **"Projekttyp"** die Option **Desktop** aus. Wählen Sie in der Liste der Projekttypen **WPF App (.NET Framework)** aus:

    ![Hinzufügen eines neuen WPF-Projekts](wpf-images/add-project.png "Hinzufügen eines neuen WPF-Projekts")

    Drücken Sie die Taste **Weiter.**

3. Geben Sie im Fenster **Konfigurieren des neuen Projekts** einen Namen für das Projekt mit einer **WPF-Erweiterung** ein, z. B. **BoxViewClock.WPF**. Klicken Sie auf die Schaltfläche **Durchsuchen,** wählen Sie den Ordner **BoxViewClock** aus, und drücken Sie **Ordner auswählen,** um das WPF-Projekt im selben Verzeichnis wie die anderen Projekte in der Projektmappe zu speichern:

    ![Hinzufügen eines neuen WPF-Projekts](wpf-images/configure-project.png "Hinzufügen eines neuen WPF-Projekts")

    Drücken Sie die Schaltfläche **Erstellen,** um das Projekt zu erstellen.

4. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf das neue **BoxViewClock.WPF-Projekt,** und wählen Sie **NuGet-Pakete verwalten...**. Wählen Sie die Registerkarte **Durchsuchen** aus, und suchen Sie nach **Xamarin.Forms.Platform.WPF**:

    ![Wählen Sie das NuGet-Paket](wpf-images/select-nuget-package.png "Wählen Sie das NuGet-Paket")

    Wählen Sie das Paket aus, und klicken Sie auf die Schaltfläche **Installieren.**

5. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektmappennamen, und wählen Sie **NuGet-Pakete für die Projektmappe verwalten...**. Wählen Sie die Registerkarte **Updates** und dann das Paket **Xamarin.Forms** aus. Wählen Sie alle Projekte aus und aktualisieren Sie sie auf dieselbe Xamarin.Forms-Version:

    ![Aktualisieren des NuGet-Pakets](wpf-images/update-nuget-package.png "Aktualisieren des NuGet-Pakets")

6. Klicken Sie im WPF-Projekt mit der rechten Maustaste auf **Referenzen,** und wählen Sie **Referenz hinzufügen...**. Wählen Sie im Dialogfeld **Referenz-Manager** die Option **Projekte** links aus, und aktivieren Sie das Kontrollkästchen neben dem **BoxViewClock-Projekt:**

    ![Verweisen auf das freigegebene Projekt](wpf-images/reference-shared-project.png "Verweisen auf das freigegebene Projekt")

    Drücken Sie die **OK-Taste.**

7. Bearbeiten Sie die Datei **MainWindow.xaml** des WPF-Projekts. Fügen `Window` Sie im Tag eine XML-Namespacedeklaration für die **Xamarin.Forms.Platform.WPF-Assembly** und den Namespace hinzu:

    ```xaml
    xmlns:wpf="clr-namespace:Xamarin.Forms.Platform.WPF;assembly=Xamarin.Forms.Platform.WPF"
    ```

    Ändern Sie `Window` nun `wpf:FormsApplicationPage`das Tag in . Ändern `Title` Sie die Einstellung in den Namen Ihrer Anwendung, z. B. **BoxViewClock**. Die ausgefüllte XAML-Datei sollte wie folgt aussehen:

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

8. Bearbeiten Sie die **MainWindow.xaml.cs** Datei des WPF-Projekts. Fügen Sie `using` zwei neue Direktiven hinzu:

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.WPF;
    ```

    Ändern Sie die `MainWindow` `Window` Basisklasse von in `FormsApplicationPage`. Fügen `InitializeComponent` Sie nach dem Aufruf die folgenden beiden Anweisungen hinzu:

    ```csharp
    Forms.Init();
    LoadApplication(new BoxViewClock.App());
    ```

    Abgesehen von Kommentaren `using` und nicht verwendeten Direktiven sollte die vollständige **MainWindows.xaml.cs** Datei wie folgt aussehen:

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

9. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das WPF-Projekt, und wählen Sie **Als Startprojekt festlegen**aus. Drücken Sie F5, um das Programm mit dem Visual Studio-Debugger auf dem Windows-Desktop auszuführen:

    ![WPF BoxView Uhr](wpf-images/wpf-boxviewclock.png "WPF BoxView Uhr" )

## <a name="platform-specifics"></a>Plattform-Spezifikationen

Sie können bestimmen, auf welcher Plattform Ihre Xamarin.Forms-Anwendung ausgeführt wird, entweder über Code oder XAML. Auf diese Weise können Sie die Programmeigenschaften ändern, wenn sie auf WPF ausgeführt werden. Vergleichen Sie im Code `Device.RuntimePlatform` den `Device.WPF` Wert von mit der Konstante (die der Zeichenfolge "WPF" entspricht). Wenn eine Übereinstimmung vorliegt, wird die Anwendung auf WPF ausgeführt.

In XAML können Sie `OnPlatform` das Tag verwenden, um einen plattformspezifischen Eigenschaftswert auszuwählen:

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

Sie können die Anfangsgröße des Fensters in der WPF **MainWindow.xaml-Datei** anpassen:

```xaml
Title="BoxViewClock" Height="450" Width="800"
```

## <a name="issues"></a>Probleme

Dies ist eine Vorschau, also sollten Sie erwarten, dass nicht alles produktionsreif ist. Nicht alle NuGet-Pakete für Xamarin.Forms sind für WPF bereit, und einige Funktionen funktionieren möglicherweise nicht vollständig.

## <a name="related-video"></a>Zugehörige Videos

> [!VIDEO https://youtube.com/embed/Fy9N6OSxK64]

**Xamarin.Forms 3.0 WPF-Unterstützungsvideo**
