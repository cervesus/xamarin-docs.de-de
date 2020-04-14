---
ms.openlocfilehash: a7c2aa15521b89e4930746dc5421ce67fa26b2b9
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2020
ms.locfileid: "67559997"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. Ändern Sie in **MainPage.xaml** die Deklaration [`StackLayout`](xref:Xamarin.Forms.StackLayout), sodass diese ihre untergeordneten Elemente horizontal und nicht vertikal ausrichtet:

    ```xaml
    <StackLayout Margin="20,35,20,25"
                 Orientation="Horizontal">
        <Label Text="The StackLayout has its Margin property set, to control the rendering position of the StackLayout." />
        <Label Text="The Padding property can be set to specify the distance between the StackLayout and its children." />
        <Label Text="The Spacing property can be set to specify the distance between views in the StackLayout." />
    </StackLayout>
    ```

    Im folgenden Code wird die [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation)-Eigenschaft auf [`Horizontal`](xref:Xamarin.Forms.StackOrientation.Horizontal) festgelegt.

1. Klicken Sie in der Symbolleiste von Visual Studio auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Remotesimulator oder Android-Emulator zu starten:

    [![Screenshot: horizontal ausgerichtete untergeordnete Ansichten in StackLayout unter iOS und Android](../images/orientation.png "StackLayout mit horizontal ausgerichteten Bezeichnungsinstanzen")](../images/orientation-large.png#lightbox "StackLayout mit horizontal ausgerichteten Bezeichnungsinstanzen")

    Wie Sie sehen können, sind die [`Label`](xref:Xamarin.Forms.Label)-Instanzen in der [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse jetzt horizontal anstatt vertikal ausgerichtet.

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Ändern Sie in **MainPage.xaml** die Deklaration [`StackLayout`](xref:Xamarin.Forms.StackLayout), sodass diese ihre untergeordneten Elemente horizontal und nicht vertikal ausrichtet:

    ```xaml
    <StackLayout Margin="20,35,20,25"
                 Orientation="Horizontal">
        <Label Text="The StackLayout has its Margin property set, to control the rendering position of the StackLayout." />
        <Label Text="The Padding property can be set to specify the distance between the StackLayout and its children." />
        <Label Text="The Spacing property can be set to specify the distance between views in the StackLayout." />
    </StackLayout>
    ```

    Im folgenden Code wird die [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation)-Eigenschaft auf [`Horizontal`](xref:Xamarin.Forms.StackOrientation.Horizontal) festgelegt.

1. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Simulator oder Android-Emulator zu starten.

    [![Screenshot: horizontal ausgerichtete untergeordnete Ansichten in StackLayout unter iOS und Android](../images/orientation.png "StackLayout mit horizontal ausgerichteten Bezeichnungsinstanzen")](../images/orientation-large.png#lightbox "StackLayout mit horizontal ausgerichteten Bezeichnungsinstanzen")

    Wie Sie sehen können, sind die [`Label`](xref:Xamarin.Forms.Label)-Instanzen in der [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse jetzt horizontal anstatt vertikal ausgerichtet.
