---
ms.openlocfilehash: 45a387690792074af6a18fe3c639692863cdf4be
ms.sourcegitcommit: 83cf2a4d99546751c6394510a463a2b2a8bf75b8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2020
ms.locfileid: "83343405"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

Für dieses Tutorial benötigen Sie das neueste Release von Visual Studio 2019 und die Workload **Mobile-Entwicklung mit .NET**. Außerdem müssen Sie über einen gekoppelten Mac verfügen, um die Tutorial-App unter iOS zu kompilieren. Informationen zur Installation der Xamarin-Plattform finden Sie unter [Installieren von Xamarin](~/get-started/installation/index.md). Informationen zum Verbinden von Visual Studio 2019 mit einem Mac-Buildhost finden Sie unter [Durchführen einer Kopplung mit einem Mac für die Xamarin.iOS-Entwicklung](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

1. Starten Sie Visual Studio, und erstellen Sie eine neue leere Xamarin.Forms-App namens **StackLayoutTutorial**. Stellen Sie sicher, dass die App .NET Standard als Mechanismus für freigegebenen Code verwendet.

    > [!IMPORTANT]
    > Für die C#- und XAML-Codeausschnitte in diesem Tutorial ist es erforderlich, dass die Projektmappe **StackLayoutTutorial** genannt wird. Die Verwendung eines anderen Namens führt zu Buildfehlern, wenn Sie Code aus diesem Tutorial in die Projektmappe kopieren.

    Weitere Informationen zur .NET Standard-Bibliothek, die erstellt wird, finden Sie unter [Anatomy of a Xamarin.Forms application (Aufbau einer Xamarin.Forms-Anwendung)](~/get-started/quickstarts/deepdive.md#anatomy-of-a-xamarinforms-application) im Artikel [Xamarin.Forms Quickstart Deep Dive (Weitere Details zum Xamarin.Forms-Schnellstart)](~/get-started/quickstarts/deepdive.md).

1. Doppelklicken Sie im **Projektmappen-Explorer** im Projekt **StackLayoutTutorial** auf die Datei **MainPage.xaml**, um sie zu öffnen. Entfernen Sie dann in **MainPage.xaml** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="StackLayoutTutorial.MainPage">
        <StackLayout Margin="20,35,20,25">
            <Label Text="The StackLayout has its Margin property set, to control the rendering position of the StackLayout." />
            <Label Text="The Padding property can be set to specify the distance between the StackLayout and its children." />
            <Label Text="The Spacing property can be set to specify the distance between views in the StackLayout." />
        </StackLayout>
    </ContentPage>
    ```

    Dieser Code definiert deklarativ die Benutzeroberfläche für die Seite, die aus drei [`Label`](xref:Xamarin.Forms.Label)-Instanzen in einer [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse besteht. Die [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse positioniert ihre untergeordneten Ansichten (die `Label`-Instanzen) in einer einzelnen Zeile, die standardmäßig vertikal ausgerichtet wird. Darüber hinaus gibt die [`Margin`](xref:Xamarin.Forms.View.Margin)-Eigenschaft die Renderingposition der `StackLayout`-Klasse innerhalb von [`ContentPage`](xref:Xamarin.Forms.ContentPage) an.

    > [!NOTE]
    > Neben der [`Margin`](xref:Xamarin.Forms.View.Margin)-Eigenschaft können auch die Eigenschaften [`Padding`](xref:Xamarin.Forms.Layout.Padding) und [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing) auf eine [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse festgelegt werden. Der [`Padding`](xref:Xamarin.Forms.Layout.Padding)-Eigenschaftswert gibt den Abstand zwischen Ansichten in der `StackLayout`-Klasse an, und der [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing)-Eigenschaftswert gibt den Abstand zwischen den einzelnen untergeordneten Elementen in der `StackLayout`-Klasse an. Weitere Informationen finden Sie unter [Ränder und Abstände](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).

1. Klicken Sie in der Symbolleiste von Visual Studio auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Remotesimulator oder Android-Emulator zu starten:

    [![Screenshot der untergeordneten Ansichten in einem StackLayout-Element unter iOS und Android](../images/create-stacklayout.png "StackLayout, das Bezeichnungsinstanzen enthält")](../images/create-stacklayout-large.png#lightbox "StackLayout, das Bezeichnungsinstanzen enthält")

    Weitere Informationen zur [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse finden Sie unter [Xamarin.Forms StackLayout](~/xamarin-forms/user-interface/layouts/stacklayout.md).

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/vsmac)

Für dieses Tutorial benötigen Sie Visual Studio für Mac (neuestes Release) mit Plattformunterstützung für iOS und Android. Zudem ist Xcode (neuestes Release) erforderlich. Weitere Informationen zur Installation der Xamarin-Plattform finden Sie unter [Installieren von Xamarin](~/get-started/installation/index.md).

1. Starten Sie Visual Studio für Mac, und erstellen Sie eine neue leere Xamarin.Forms-App namens **StackLayoutTutorial**. Stellen Sie sicher, dass die App .NET Standard als Mechanismus für freigegebenen Code verwendet.

    > [!IMPORTANT]
    > Für die C#- und XAML-Codeausschnitte in diesem Tutorial ist es erforderlich, dass die Projektmappe **StackLayoutTutorial** genannt wird. Die Verwendung eines anderen Namens führt zu Buildfehlern, wenn Sie Code aus diesem Tutorial in die Projektmappe kopieren.

    Weitere Informationen zur .NET Standard-Bibliothek, die erstellt wird, finden Sie unter [Aufbau einer Xamarin.Forms-Anwendung](~/get-started/first-app/index.md) im Artikel [Xamarin.Forms Quickstart Deep Dive (Ausführliche Erläuterungen zum Xamarin.Forms-Schnellstart)](~/get-started/first-app/index.md).

1. Doppelklicken Sie im **Lösungspad** im Projekt **StackLayoutTutorial** auf die Datei **MainPage.xaml**, um sie zu öffnen. Entfernen Sie dann in **MainPage.xaml** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="StackLayoutTutorial.MainPage">
        <StackLayout Margin="20,35,20,25">
            <Label Text="The StackLayout has its Margin property set, to control the rendering position of the StackLayout." />
            <Label Text="The Padding property can be set to specify the distance between the StackLayout and its children." />
            <Label Text="The Spacing property can be set to specify the distance between views in the StackLayout." />
        </StackLayout>
    </ContentPage>
    ```

    Dieser Code definiert deklarativ die Benutzeroberfläche für die Seite, die aus drei [`Label`](xref:Xamarin.Forms.Label)-Instanzen in einer [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse besteht. Die [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse positioniert ihre untergeordneten Ansichten (die `Label`-Instanzen) in einer einzelnen Zeile, die standardmäßig vertikal ausgerichtet wird. Darüber hinaus gibt die [`Margin`](xref:Xamarin.Forms.View.Margin)-Eigenschaft die Renderingposition der `StackLayout`-Klasse innerhalb von [`ContentPage`](xref:Xamarin.Forms.ContentPage) an.

    > [!NOTE]
    > Neben der [`Margin`](xref:Xamarin.Forms.View.Margin)-Eigenschaft können auch die Eigenschaften [`Padding`](xref:Xamarin.Forms.Layout.Padding) und [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing) auf eine [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse festgelegt werden. Der [`Padding`](xref:Xamarin.Forms.Layout.Padding)-Eigenschaftswert gibt den Abstand zwischen Ansichten in der `StackLayout`-Klasse an, und der [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing)-Eigenschaftswert gibt den Abstand zwischen den einzelnen untergeordneten Elementen in der `StackLayout`-Klasse an. Weitere Informationen finden Sie unter [Ränder und Abstände](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).

1. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Simulator oder Android-Emulator zu starten.

    [![Screenshot der untergeordneten Ansichten in einem StackLayout-Element unter iOS und Android](../images/create-stacklayout.png "StackLayout, das Bezeichnungsinstanzen enthält")](../images/create-stacklayout-large.png#lightbox "StackLayout, das Bezeichnungsinstanzen enthält")

    Weitere Informationen zur [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse finden Sie unter [Xamarin.Forms StackLayout](~/xamarin-forms/user-interface/layouts/stacklayout.md).
