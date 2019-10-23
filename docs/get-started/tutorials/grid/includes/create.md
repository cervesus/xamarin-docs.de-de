---
ms.openlocfilehash: b1a041f1a2baae9b06de023f6eae9c6598b80061
ms.sourcegitcommit: 5efbf5ab53532b3a74c80129ff4e0ca84b476d21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72678720"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Für dieses Tutorial benötigen Sie das neueste Release von Visual Studio 2019 und die Workload **Mobile-Entwicklung mit .NET**. Außerdem müssen Sie über einen gekoppelten Mac verfügen, um die Tutorial-App unter iOS zu kompilieren. Informationen zur Installation der Xamarin-Plattform finden Sie unter [Installieren von Xamarin](~/get-started/installation/index.md). Informationen zum Verbinden von Visual Studio 2019 mit einem Mac-Buildhost finden Sie unter [Durchführen einer Kopplung mit einem Mac für die Xamarin.iOS-Entwicklung](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

1. Starten Sie Visual Studio, und erstellen Sie eine neue leere Xamarin.Forms-App mit dem Namen **GridTutorial**. Stellen Sie sicher, dass die App .NET Standard als Mechanismus für freigegebenen Code verwendet.

    > [!IMPORTANT]
    > Für die C#- und XAML-Codeausschnitte in diesem Tutorial ist es erforderlich, dass die Projektmappe **GridTutorial** genannt wird. Die Verwendung eines anderen Namens führt zu Buildfehlern, wenn Sie Code aus diesem Tutorial in die Projektmappe kopieren.

    Weitere Informationen zur .NET Standard-Bibliothek, die erstellt wird, finden Sie unter [Aufbau einer Xamarin.Forms-Anwendung](~/get-started/first-app/index.md) im Artikel [Ausführliche Erläuterungen zum Xamarin.Forms-Schnellstart](~/get-started/first-app/index.md).

1. Doppelklicken Sie im **Projektmappen-Explorer** im Projekt **GridTutorial** auf die Datei **MainPage.xaml**, um sie zu öffnen. Entfernen Sie dann in **MainPage.xaml** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="GridTutorial.MainPage">
        <Grid Margin="20,35,20,20">
            <Label Text="The Grid has its Margin property set, to control the rendering position of the Grid." />
        </Grid>
    </ContentPage>
    ```

    In diesem Codeausschnitt wird die Benutzeroberfläche deklarativ für die Seite definiert, die ein [`Label`](xref:Xamarin.Forms.Label)-Element in einem [`Grid`](xref:Xamarin.Forms.Grid) enthält. `Grid` positioniert die untergeordneten Ansichten standardmäßig an einem einzelnen Ort. Daher sollte ein `Grid`-Element, das mehrere untergeordnete Elemente enthält, Spalten und Zeilen angeben. Dies wird in der nächsten Übung behandelt. Darüber hinaus gibt die [`Margin`](xref:Xamarin.Forms.View.Margin)-Eigenschaft die Renderingposition der `Grid`-Klasse innerhalb von [`ContentPage`](xref:Xamarin.Forms.ContentPage) an.

    > [!NOTE]
    > Neben der [`Margin`](xref:Xamarin.Forms.View.Margin)-Eigenschaft kann die [`Padding`](xref:Xamarin.Forms.Layout.Padding)-Eigenschaft auch auf eine [`Grid`](xref:Xamarin.Forms.Grid)-Klasse festgelegt werden. Der Eigenschaftswert [`Padding`](xref:Xamarin.Forms.Layout.Padding) gibt die Entfernung zwischen den Begrenzungen von `Grid` und dessen untergeordnetem Element an. Weitere Informationen finden Sie unter [Ränder und Abstände](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).

1. Klicken Sie in der Symbolleiste von Visual Studio auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Remotesimulator oder Android-Emulator zu starten:

    [![Screenshot einer Bezeichnung in einem Raster unter iOS und Android](../images/create-grid.png "Raster mit einer Bezeichnung")](../images/create-grid-large.png#lightbox "Raster mit einer Bezeichnungs")

    Weitere Informationen zur [`Grid`](xref:Xamarin.Forms.Grid)-Klasse finden Sie unter [Xamarin.Forms Grid](~/xamarin-forms/user-interface/layouts/grid.md).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Für dieses Tutorial benötigen Sie Visual Studio für Mac (neuestes Release) mit Plattformunterstützung für iOS und Android. Zudem ist Xcode (neuestes Release) erforderlich. Weitere Informationen zur Installation der Xamarin-Plattform finden Sie unter [Installieren von Xamarin](~/get-started/installation/index.md).

1. Starten Sie Visual Studio für Mac, und erstellen Sie eine neue leere Xamarin.Forms-App mit dem Namen **GridTutorial**. Stellen Sie sicher, dass die App .NET Standard als Mechanismus für freigegebenen Code verwendet.

    > [!IMPORTANT]
    > Für die C#- und XAML-Codeausschnitte in diesem Tutorial ist es erforderlich, dass die Projektmappe **GridTutorial** genannt wird. Die Verwendung eines anderen Namens führt zu Buildfehlern, wenn Sie Code aus diesem Tutorial in die Projektmappe kopieren.

    Weitere Informationen zur .NET Standard-Bibliothek, die erstellt wird, finden Sie unter [Aufbau einer Xamarin.Forms-Anwendung](~/get-started/first-app/index.md) im Artikel [Ausführliche Erläuterungen zum Xamarin.Forms-Schnellstart](~/get-started/first-app/index.md).

1. Doppelklicken Sie im **Lösungspad** im Projekt **GridTutorial** auf die Datei **MainPage.xaml**, um sie zu öffnen. Entfernen Sie dann in **MainPage.xaml** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="GridTutorial.MainPage">
        <Grid Margin="20,35,20,20">
            <Label Text="The Grid has its Margin property set, to control the rendering position of the Grid." />
        </Grid>
    </ContentPage>
    ```

    In diesem Codeausschnitt wird die Benutzeroberfläche deklarativ für die Seite definiert, die ein [`Label`](xref:Xamarin.Forms.Label)-Element in einem [`Grid`](xref:Xamarin.Forms.Grid) enthält. `Grid` positioniert die untergeordneten Ansichten standardmäßig an einem einzelnen Ort. Daher sollte ein `Grid`-Element, das mehrere untergeordnete Elemente enthält, Spalten und Zeilen angeben. Dies wird in der nächsten Übung behandelt. Darüber hinaus gibt die [`Margin`](xref:Xamarin.Forms.View.Margin)-Eigenschaft die Renderingposition der `Grid`-Klasse innerhalb von [`ContentPage`](xref:Xamarin.Forms.ContentPage) an.

    > [!NOTE]
    > Neben der [`Margin`](xref:Xamarin.Forms.View.Margin)-Eigenschaft kann die [`Padding`](xref:Xamarin.Forms.Layout.Padding)-Eigenschaft auch auf eine [`Grid`](xref:Xamarin.Forms.Grid)-Klasse festgelegt werden. Der Eigenschaftswert [`Padding`](xref:Xamarin.Forms.Layout.Padding) legt den Abstand zwischen den Ansichten im `Grid`-Element fest. Weitere Informationen finden Sie unter [Ränder und Abstände](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).

1. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Simulator oder Android-Emulator zu starten.

    [![Screenshot einer Bezeichnung in einem Raster unter iOS und Android](../images/create-grid.png "Raster mit einer Bezeichnung")](../images/create-grid-large.png#lightbox "Raster mit einer Bezeichnungs")

    Weitere Informationen zur [`Grid`](xref:Xamarin.Forms.Grid)-Klasse finden Sie unter [Xamarin.Forms Grid](~/xamarin-forms/user-interface/layouts/grid.md).
