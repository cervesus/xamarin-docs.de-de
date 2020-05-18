---
ms.openlocfilehash: fa88f6e7844899926a194e9d0cdd455a497c2b31
ms.sourcegitcommit: bc0c1740aa0708459729c0e671ab3ff7de3e2eee
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/15/2020
ms.locfileid: "83435389"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. Ändern Sie in **MainPage.xaml** die [`Grid`](xref:Xamarin.Forms.Grid)-Deklaration, um Spalten und Zeilen zu bestimmen, und platzieren Sie Inhalt, der die folgenden Spalten und Zeilen umfasst:

    ```xaml
    <Grid Margin="20,35,20,20">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.5*" />
            <ColumnDefinition Width="0.5*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="50" />
            <RowDefinition Height="30" />
            <RowDefinition Height="30" />
        </Grid.RowDefinitions>
        <Label Grid.ColumnSpan="2"
               Text="This text uses the ColumnSpan property to span both columns." />
        <Label Grid.Row="1"
               Grid.RowSpan="2"
               Text="This text uses the RowSpan property to span two rows." />
    </Grid>
    ```

    Dieser Code gibt Spalten und Zeilen für die [`Grid`](xref:Xamarin.Forms.Grid)-Klasse an, und positioniert [`Label`](xref:Xamarin.Forms.Label)-Instanzen in bestimmten Spalten und Zeilen. Die erste `Label`-Klasse legt die angefügte [`ColumnSpan`](xref:Xamarin.Forms.Grid.ColumnSpanProperty)-Eigenschaft fest, sodass der Text mehrere Spalten umfasst. Die `ColumnSpan`-Eigenschaft ist auf 2 festgelegt, also auf die Anzahl der Spalten, die die `Label`-Klasse umfassen wird. Die zweite `Label`-Klasse legt die angefügte [`RowSpan`](xref:Xamarin.Forms.Grid.RowSpanProperty)-Eigenschaft fest, sodass der Text mehrere Zeilen umfasst. Die `RowSpan`-Eigenschaft ist auf 2 festgelegt, also auf die Anzahl der Zeilen, die die `Label`-Klasse umfassen wird.

1. Klicken Sie in der Symbolleiste von Visual Studio auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Remotesimulator oder Android-Emulator zu starten:

    [![Screenshot: Raster mit Inhalt, der sich über mehrere Spalten und Zeilen erstreckt, unter iOS und Android](../images/span-columns-rows.png "Raster mit Inhalt, der sich über mehrere Spalten und Zeilen erstreckt")](../images/span-columns-rows-large.png#lightbox "Raster mit Inhalt, der sich über mehrere Spalten und Zeilen erstreckt")

    Weitere Informationen zum Überspannen von Zeilen und Spalten finden Sie unter [Zeilen und Spalten](~/xamarin-forms/user-interface/layouts/grid.md#rows-and-columns) im Leitfaden zu [Xamarin.Forms-Rastern](~/xamarin-forms/user-interface/layouts/grid.md).

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Ändern Sie in **MainPage.xaml** die [`Grid`](xref:Xamarin.Forms.Grid)-Deklaration, um Spalten und Zeilen zu bestimmen, und platzieren Sie Inhalt, der die folgenden Spalten und Zeilen umfasst:

    ```xaml
    <Grid Margin="20,35,20,20">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.5*" />
            <ColumnDefinition Width="0.5*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="50" />
            <RowDefinition Height="30" />
            <RowDefinition Height="30" />
        </Grid.RowDefinitions>
        <Label Grid.ColumnSpan="2"
               Text="This text uses the ColumnSpan property to span both columns." />
        <Label Grid.Row="1"
               Grid.RowSpan="2"
               Text="This text uses the RowSpan property to span two rows." />
    </Grid>
    ```

    Dieser Code gibt Spalten und Zeilen für die [`Grid`](xref:Xamarin.Forms.Grid)-Klasse an, und positioniert [`Label`](xref:Xamarin.Forms.Label)-Instanzen in bestimmten Spalten und Zeilen. Die erste `Label`-Klasse legt die angefügte [`ColumnSpan`](xref:Xamarin.Forms.Grid.ColumnSpanProperty)-Eigenschaft fest, sodass der Text mehrere Spalten umfasst. Die `ColumnSpan`-Eigenschaft ist auf 2 festgelegt, also auf die Anzahl der Spalten, die die `Label`-Klasse umfassen wird. Die zweite `Label`-Klasse legt die angefügte [`RowSpan`](xref:Xamarin.Forms.Grid.RowSpanProperty)-Eigenschaft fest, sodass der Text mehrere Zeilen umfasst. Die `RowSpan`-Eigenschaft ist auf 2 festgelegt, also auf die Anzahl der Zeilen, die die `Label`-Klasse umfassen wird.

1. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Simulator oder Android-Emulator zu starten.

    [![Screenshot: Raster mit Inhalt, der sich über mehrere Spalten und Zeilen erstreckt, unter iOS und Android](../images/span-columns-rows.png "Raster mit Inhalt, der sich über mehrere Spalten und Zeilen erstreckt")](../images/span-columns-rows-large.png#lightbox "Raster mit Inhalt, der sich über mehrere Spalten und Zeilen erstreckt")

    Weitere Informationen zum Überspannen von Zeilen und Spalten finden Sie unter [Zeilen und Spalten](~/xamarin-forms/user-interface/layouts/grid.md#rows-and-columns) im Leitfaden zu [Xamarin.Forms-Rastern](~/xamarin-forms/user-interface/layouts/grid.md).
