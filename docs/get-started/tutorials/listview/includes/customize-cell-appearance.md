---
ms.openlocfilehash: 3c88b71cea834f5e6ef20d43332904c052c6e3a6
ms.sourcegitcommit: 3f0e4f10e5def19122588bb05f26ab2baa9df6eb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2020
ms.locfileid: "61037630"
---
Zuvor wurde die [`ListView`](xref:Xamarin.Forms.ListView) mithilfe von Datenbindung mit Daten gefüllt. Trotz Datenbindung zu einer Auflistung, in der jedes Objekt der Auflistung mehrere Datenelemente definiert hat, wurde nur ein Datenelement pro Objekt angezeigt (die `Name`-Eigenschaft des `Monkey`-Objekts).

In dieser Übung ändern Sie das **ListViewTutorial**-Projekt, damit die [`ListView`](xref:Xamarin.Forms.ListView) mehrere Datenelemente in einer Zeile anzeigt.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Bearbeiten Sie in **MainPage.xaml** die [`ListView`](xref:Xamarin.Forms.Image)-Deklaration, um die Darstellung jeder Zeile anzupassen:

    ```xaml
    <ListView ItemsSource="{Binding Monkeys}"
              HasUnevenRows="true"
              ItemSelected="OnListViewItemSelected"
              ItemTapped="OnListViewItemTapped">
        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <Grid Padding="10">
                        <Grid.RowDefinitions>
                            <RowDefinition Height="Auto" />
                            <RowDefinition Height="*" />
                        </Grid.RowDefinitions>
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="Auto" />
                            <ColumnDefinition Width="*" />
                        </Grid.ColumnDefinitions>
                        <Image Grid.RowSpan="2"
                               Source="{Binding ImageUrl}"
                               Aspect="AspectFill"
                               HeightRequest="60"
                               WidthRequest="60" />
                        <Label Grid.Column="1"
                               Text="{Binding Name}"
                               FontAttributes="Bold" />
                        <Label Grid.Row="1"
                               Grid.Column="1"
                               Text="{Binding Location}"
                               VerticalOptions="End" />
                    </Grid>
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
    ```

    Dieser Code legt die [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate)-Eigenschaft auf eine [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) fest, die das Auftreten jeder Zeile in der [`ListView`](xref:Xamarin.Forms.ListView) bestimmt. Das untergeordnete Element der `DataTemplate` muss den Typ [`Cell`](xref:Xamarin.Forms.Cell) aufweisen oder von diesem abgeleitet sein. Dieser Code verwendet eine aus `Cell` abgeleitete [`ViewCell`](xref:Xamarin.Forms.ViewCell) zum Erstellen eines benutzerdefinierten Layouts für jede `ListView`-Zeile. Layout innerhalb der `ViewCell` wird hier durch ein [`Grid`](xref:Xamarin.Forms.Grid) verwaltet, das ein [`Image`](xref:Xamarin.Forms.Image)-Objekt und zwei [`Label`](xref:Xamarin.Forms.Label)-Objekte enthält. Das `Image`-Objekt bindet seine [`Source`](xref:Xamarin.Forms.Image.Source)-Eigenschaft an die `ImageUrl`-Eigenschaft von jedem `Monkey`-Objekt, während die `Label`-Objekte ihre [`Text`](xref:Xamarin.Forms.Label.Text)-Eigenschaften an die `Name`- und `Location`-Eigenschaften von jedem `Monkey`-Objekt binden.

    Standardmäßig weisen alle Zeilen in einer [`ListView`](xref:Xamarin.Forms.ListView) die gleiche Höhe auf. Dieser Code legt jedoch die [`ListView.HasUnevenRows`](xref:Xamarin.Forms.ListView.HasUnevenRows)-Eigenschaft auf `true` fest, sodass die Zeilenhöhe an deren Inhalt angepasst werden kann. Dies ist nützlich für die `Name`- und `Location`-Eigenschaften von `Monkey`-Objekten mit Zeichenfolgen variabler Länge.

    Weitere Informationen zur Darstellung der [`ListView`](xref:Xamarin.Forms.ListView)-Zelle finden Sie unter [Customizing ListView Cell Appearance (Anpassen der ListView-Zellendarstellung)](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md). Weitere Informationen zu Datenvorlagen finden Sie unter [Xamarin.Forms-Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

1. Klicken Sie in der Symbolleiste von Visual Studio auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Remotesimulator oder Android-Emulator zu starten:

    [![Screenshot: Listenansicht, deren Elemente mithilfe einer Datenvorlage erstellt werden](../images/customize-cell-appearance.png "Listenansicht mit Daten aus Vorlagen")](../images/customize-cell-appearance-large.png#lightbox "Listenansicht mit Daten aus Vorlagen")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Bearbeiten Sie in **MainPage.xaml** die [`ListView`](xref:Xamarin.Forms.Image)-Deklaration, um die Darstellung jeder Zeile anzupassen:

    ```xaml
    <ListView ItemsSource="{Binding Monkeys}"
              HasUnevenRows="true"
              ItemSelected="OnListViewItemSelected"
              ItemTapped="OnListViewItemTapped">
        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <Grid Padding="10">
                        <Grid.RowDefinitions>
                            <RowDefinition Height="Auto" />
                            <RowDefinition Height="*" />
                        </Grid.RowDefinitions>
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="Auto" />
                            <ColumnDefinition Width="*" />
                        </Grid.ColumnDefinitions>
                        <Image Grid.RowSpan="2"
                               Source="{Binding ImageUrl}"
                               Aspect="AspectFill"
                               HeightRequest="60"
                               WidthRequest="60" />
                        <Label Grid.Column="1"
                               Text="{Binding Name}"
                               FontAttributes="Bold" />
                        <Label Grid.Row="1"
                               Grid.Column="1"
                               Text="{Binding Location}"
                               VerticalOptions="End" />
                    </Grid>
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
    ```

    Dieser Code legt die [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate)-Eigenschaft auf eine [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) fest, die das Auftreten jeder Zeile in der [`ListView`](xref:Xamarin.Forms.ListView) bestimmt. Das untergeordnete Element der `DataTemplate` muss den Typ [`Cell`](xref:Xamarin.Forms.Cell) aufweisen oder von diesem abgeleitet sein. Dieser Code verwendet eine aus `Cell` abgeleitete [`ViewCell`](xref:Xamarin.Forms.ViewCell) zum Erstellen eines benutzerdefinierten Layouts für jede `ListView`-Zeile. Layout innerhalb der `ViewCell` wird hier durch ein [`Grid`](xref:Xamarin.Forms.Grid) verwaltet, das ein [`Image`](xref:Xamarin.Forms.Image)-Objekt und zwei [`Label`](xref:Xamarin.Forms.Label)-Objekte enthält. Das `Image`-Objekt bindet seine [`Source`](xref:Xamarin.Forms.Image.Source)-Eigenschaft an die `ImageUrl`-Eigenschaft von jedem `Monkey`-Objekt, während die `Label`-Objekte ihre [`Text`](xref:Xamarin.Forms.Label.Text)-Eigenschaften an die `Name`- und `Location`-Eigenschaften von jedem `Monkey`-Objekt binden.

    Standardmäßig weisen alle Zeilen in einer [`ListView`](xref:Xamarin.Forms.ListView) die gleiche Höhe auf. Dieser Code legt jedoch die [`ListView.HasUnevenRows`](xref:Xamarin.Forms.ListView.HasUnevenRows)-Eigenschaft auf `true` fest, sodass die Zeilenhöhe an deren Inhalt angepasst werden kann. Dies ist nützlich für die `Name`- und `Location`-Eigenschaften von `Monkey`-Objekten mit Zeichenfolgen variabler Länge.

    Weitere Informationen zur Darstellung der [`ListView`](xref:Xamarin.Forms.ListView)-Zelle finden Sie unter [Customizing ListView Cell Appearance (Anpassen der ListView-Zellendarstellung)](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md). Weitere Informationen zu Datenvorlagen finden Sie unter [Xamarin.Forms-Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

1. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Simulator oder Android-Emulator zu starten.

    [![Screenshot: Listenansicht, deren Elemente mithilfe einer Datenvorlage erstellt werden](../images/customize-cell-appearance.png "Listenansicht mit Daten aus Vorlagen")](../images/customize-cell-appearance-large.png#lightbox "Listenansicht mit Daten aus Vorlagen")
