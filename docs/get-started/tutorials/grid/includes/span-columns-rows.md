---
ms.openlocfilehash: c826ee87c006b05322af8c9312bdf3120df8b357
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61375360"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

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

    [![Screenshot: Raster mit Inhalt, der mehrere Spalten und Zeilen umfasst, unter iOS und Android](../images/span-columns-rows.png "Raster mit Inhalt, der Spalten und Zeilen umfasst")](../images/span-columns-rows-large.png#lightbox "Raster mit Inhalt, der Spalten und Zeilen umfasst")

    Weitere Informationen zum Umfassen von Spalten und Zeilen finden Sie unter [Span-Eigenschaften](~/xamarin-forms/user-interface/layouts/grid.md#spans) im Leitfaden zu [Xamarin.Forms-Rastern](~/xamarin-forms/user-interface/layouts/grid.md).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

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

1. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Simulator oder Android-Emulator zu starten:

    [![Screenshot: Raster mit Inhalt, der mehrere Spalten und Zeilen umfasst, unter iOS und Android](../images/span-columns-rows.png "Raster mit Inhalt, der Spalten und Zeilen umfasst")](../images/span-columns-rows-large.png#lightbox "Raster mit Inhalt, der Spalten und Zeilen umfasst")

    Weitere Informationen zum Umfassen von Spalten und Zeilen finden Sie unter [Span-Eigenschaften](~/xamarin-forms/user-interface/layouts/grid.md#spans) im Leitfaden zu [Xamarin.Forms-Rastern](~/xamarin-forms/user-interface/layouts/grid.md).
