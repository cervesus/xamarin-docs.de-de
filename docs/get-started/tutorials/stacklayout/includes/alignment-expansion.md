---
ms.openlocfilehash: d6dbc82e56959399c2befb6a12f0a2cf3793ee5b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61382516"
---
Die Größe und Position untergeordneter Ansichten innerhalb einer [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse hängen von den Werten der Eigenschaften [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) und [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) der untergeordneten Ansichten ab sowie den Werten der Eigenschaften [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) und [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions).

Die Eigenschaften [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) und [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) können auf Felder der [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions)-Struktur festgelegt werden, die zwei Layouteinstellungen kapselt:

- **Ausrichten:** Die bevorzugte Ausrichtung des untergeordneten Elements, die die Position und Größe des untergeordneten Elements innerhalb des Layouts des übergeordneten Elements bestimmt
- **Erweiterung:** Gibt an, ob die Ansicht untergeordneter Elemente zusätzlichen Platz (falls vorhanden) verwenden soll (nur von einer [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse verwendet)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Ändern Sie in **MainPage.xaml** die [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Deklaration, um die Optionen für die Ausrichtung und Erweiterung für jede [`Label`](xref:Xamarin.Forms.Label)-Klasse festzulegen:

    ```xaml
    <StackLayout Margin="20,35,20,25">
        <Label Text="Start"
               HorizontalOptions="Start"
               BackgroundColor="Gray" />
        <Label Text="Center"
               HorizontalOptions="Center"
               BackgroundColor="Gray" />
        <Label Text="End"
               HorizontalOptions="End"
               BackgroundColor="Gray" />
        <Label Text="Fill"
               HorizontalOptions="Fill"
               BackgroundColor="Gray" />
        <Label Text="StartAndExpand"
               VerticalOptions="StartAndExpand"
               BackgroundColor="Gray" />
        <Label Text="CenterAndExpand"
               VerticalOptions="CenterAndExpand"
               BackgroundColor="Gray" />
        <Label Text="EndAndExpand"
               VerticalOptions="EndAndExpand"
               BackgroundColor="Gray" />
        <Label Text="FillAndExpand"
               VerticalOptions="FillAndExpand"
               BackgroundColor="Gray" />
    </StackLayout>
    ```

    Dieser Code legt die Ausrichtungseinstellungen für die ersten vier [`Label`](xref:Xamarin.Forms.Label)-Instanzen fest sowie die Erweiterungseinstellungen für die letzten vier `Label`-Instanzen. Die Felder [`Start`](xref:Xamarin.Forms.LayoutOptions.Start), [`Center`](xref:Xamarin.Forms.LayoutOptions.Center), [`End`](xref:Xamarin.Forms.LayoutOptions.End) und [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) werden verwendet, um die Ausrichtung der [`Label`](xref:Xamarin.Forms.Label)-Instanzen innerhalb der übergeordneten [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse zu definieren. Die Felder [`StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand), [`CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand), [`EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand) und [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) werden verwendet, um die Ausrichtungseinstellung zu definieren und ob die Ansicht mehr Platz innerhalb der übergeordneten `StackLayout`-Klasse belegt (falls verfügbar).

    > [!NOTE]
    > Der Standardwert der Eigenschaften [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) und [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) einer Ansicht lautet [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill).

1. Klicken Sie in der Symbolleiste von Visual Studio auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Remotesimulator oder Android-Emulator zu starten:

    [![Screenshot von Ansichten eines untergeordneten Elements in einer StackLayout-Klasse, mit festgelegten Ausrichtungs- und Erweiterungsoptionen unter Android und iOS](../images/alignment-expansion.png "StackLayout mit Bezeichnungsinstanzen, mit festgelegter Ausrichtung und Erweiterung")](../images/alignment-expansion-large.png#lightbox "StackLayout mit Bezeichnungsinstanzen, mit festgelegter Ausrichtung und Erweiterung")

    Eine [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse respektiert ausschließlich die Ausrichtungseinstellungen für Ansichten untergeordneter Elemente, die sich in entgegengesetzter Richtung zur `StackLayout`-Ausrichtung befinden. Deshalb legen die Ansichten der untergeordneten [`Label`](xref:Xamarin.Forms.Label)-Klasse innerhalb einer vertikal ausgerichteten `StackLayout`-Klasse ihre [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions)-Eigenschaften auf eines der Ausrichtungsfelder fest:

    - Feld [`Start`](xref:Xamarin.Forms.LayoutOptions.Start), das die [`Label`](xref:Xamarin.Forms.Label)-Klasse auf der linken Seite der [`StackLayout`](xref:Xamarin.Forms.StackLayout) positioniert
    - Feld [`Center`](xref:Xamarin.Forms.LayoutOptions.Center), das die [`Label`](xref:Xamarin.Forms.Label)-Klasse in der Mitte der [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse positioniert
    - Feld [`End`](xref:Xamarin.Forms.LayoutOptions.End), das die [`Label`](xref:Xamarin.Forms.Label)-Klasse auf der rechten Seite der [`StackLayout`](xref:Xamarin.Forms.StackLayout) positioniert
    - Feld [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill), das sicherstellt, dass die [`Label`](xref:Xamarin.Forms.Label)-Klasse die gesamte Breite der [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse ausfüllt

    Eine [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse kann Ansichten untergeordneter Elemente nur in die Orientierungsrichtung erweitern. Deshalb kann die vertikal ausgerichtete `StackLayout`-Klasse die Ansichten untergeordneter [`Label`](xref:Xamarin.Forms.Label)-Klassen erweitern, die ihre [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions)-Eigenschaften auf eines der Erweiterungsfelder festgelegt haben. Das bedeutet, dass jede `Label`-Klasse für die vertikale Ausrichtung den gleichen Platz innerhalb der `StackLayout`-Klasse belegt. Allerdings hat nur die letzte `Label`-Klasse, die ihre [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions)-Eigenschaft auf [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) festlegt, eine andere Größe.

    > [!IMPORTANT]
    > Wenn der gesamte Platz in einer [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse belegt ist, haben Einstellungen für die Erweiterung keine Auswirkungen.

    Weitere Informationen zur Ausrichtung und Erweiterung finden Sie unter [Layoutoptionen in Xamarin.Forms](~/xamarin-forms/user-interface/layouts/layout-options.md).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Ändern Sie in **MainPage.xaml** die [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Deklaration, um die Optionen für die Ausrichtung und Erweiterung für jede [`Label`](xref:Xamarin.Forms.Label)-Klasse festzulegen:

    ```xaml
    <StackLayout Margin="20,35,20,25">
        <Label Text="Start"
               HorizontalOptions="Start"
               BackgroundColor="Gray" />
        <Label Text="Center"
               HorizontalOptions="Center"
               BackgroundColor="Gray" />
        <Label Text="End"
               HorizontalOptions="End"
               BackgroundColor="Gray" />
        <Label Text="Fill"
               HorizontalOptions="Fill"
               BackgroundColor="Gray" />
        <Label Text="StartAndExpand"
               VerticalOptions="StartAndExpand"
               BackgroundColor="Gray" />
        <Label Text="CenterAndExpand"
               VerticalOptions="CenterAndExpand"
               BackgroundColor="Gray" />
        <Label Text="EndAndExpand"
               VerticalOptions="EndAndExpand"
               BackgroundColor="Gray" />
        <Label Text="FillAndExpand"
               VerticalOptions="FillAndExpand"
               BackgroundColor="Gray" />
    </StackLayout>
    ```

    Dieser Code legt die Ausrichtungseinstellungen für die ersten vier [`Label`](xref:Xamarin.Forms.Label)-Instanzen fest sowie die Erweiterungseinstellungen für die letzten vier `Label`-Instanzen. Die Felder [`Start`](xref:Xamarin.Forms.LayoutOptions.Start), [`Center`](xref:Xamarin.Forms.LayoutOptions.Center), [`End`](xref:Xamarin.Forms.LayoutOptions.End) und [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) werden verwendet, um die Ausrichtung der [`Label`](xref:Xamarin.Forms.Label)-Instanzen innerhalb der übergeordneten [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse zu definieren. Die Felder [`StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand), [`CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand), [`EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand) und [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) werden verwendet, um die Ausrichtungseinstellung zu definieren und ob die Ansicht mehr Platz innerhalb der übergeordneten `StackLayout`-Klasse belegt (falls verfügbar).

    > [!NOTE]
    > Der Standardwert der Eigenschaften [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) und [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) einer Ansicht lautet [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill).

1. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Simulator oder Android-Emulator zu starten:

    [![Screenshot von Ansichten eines untergeordneten Elements in einer StackLayout-Klasse, mit festgelegten Ausrichtungs- und Erweiterungsoptionen unter Android und iOS](../images/alignment-expansion.png "StackLayout mit Bezeichnungsinstanzen, mit festgelegter Ausrichtung und Erweiterung")](../images/alignment-expansion-large.png#lightbox "StackLayout mit Bezeichnungsinstanzen, mit festgelegter Ausrichtung und Erweiterung")

    Eine [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse respektiert ausschließlich die Ausrichtungseinstellungen für Ansichten untergeordneter Elemente, die sich in entgegengesetzter Richtung zur `StackLayout`-Ausrichtung befinden. Deshalb legen die Ansichten der untergeordneten [`Label`](xref:Xamarin.Forms.Label)-Klasse innerhalb einer vertikal ausgerichteten `StackLayout`-Klasse ihre [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions)-Eigenschaften auf eines der Ausrichtungsfelder fest:

    - Feld [`Start`](xref:Xamarin.Forms.LayoutOptions.Start), das die [`Label`](xref:Xamarin.Forms.Label)-Klasse auf der linken Seite der [`StackLayout`](xref:Xamarin.Forms.StackLayout) positioniert
    - Feld [`Center`](xref:Xamarin.Forms.LayoutOptions.Center), das die [`Label`](xref:Xamarin.Forms.Label)-Klasse in der Mitte der [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse positioniert
    - Feld [`End`](xref:Xamarin.Forms.LayoutOptions.End), das die [`Label`](xref:Xamarin.Forms.Label)-Klasse auf der rechten Seite der [`StackLayout`](xref:Xamarin.Forms.StackLayout) positioniert
    - Feld [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill), das sicherstellt, dass die [`Label`](xref:Xamarin.Forms.Label)-Klasse die gesamte Breite der [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse ausfüllt

    Eine [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse kann Ansichten untergeordneter Elemente nur in die Orientierungsrichtung erweitern. Deshalb kann die vertikal ausgerichtete `StackLayout`-Klasse die Ansichten untergeordneter [`Label`](xref:Xamarin.Forms.Label)-Klassen erweitern, die ihre [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions)-Eigenschaften auf eines der Erweiterungsfelder festgelegt haben. Das bedeutet, dass jede `Label`-Klasse für die vertikale Ausrichtung den gleichen Platz innerhalb der `StackLayout`-Klasse belegt. Allerdings hat nur die letzte `Label`-Klasse, die ihre [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions)-Eigenschaft auf [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) festlegt, eine andere Größe.

    > [!IMPORTANT]
    > Wenn der gesamte Platz in einer [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse belegt ist, haben Einstellungen für die Erweiterung keine Auswirkungen.

    Weitere Informationen zur Ausrichtung und Erweiterung finden Sie unter [Layoutoptionen in Xamarin.Forms](~/xamarin-forms/user-interface/layouts/layout-options.md).
