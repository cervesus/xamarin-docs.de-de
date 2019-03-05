# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Bearbeiten Sie in **MainPage.xaml** die [`Button`](xref:Xamarin.Forms.Button)-Deklaration, um die visuelle Darstellung zu ändern:

    ```xaml
    <Button Text="Click me"
            Clicked="OnButtonClicked"
            TextColor="Blue"
            BackgroundColor="Aqua"
            BorderColor="Red"
            BorderWidth="5"
            CornerRadius="5"
            WidthRequest="150"
            HeightRequest="75" />
    ```

    Dieser Code legt Eigenschaften fest, die die visuelle Darstellung von [`Button`](xref:Xamarin.Forms.Button) ändern. Die [`TextColor`](xref:Xamarin.Forms.Button.TextColor)-Eigenschaft legt die Farbe des `Button`-Texts fest, und die [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor)-Eigenschaft legt die Farbe des Texthintergrunds fest. Die [`BorderColor`](xref:Xamarin.Forms.Button.BorderColor)-Eigenschaft legt die Farbe des Bereichs um `Button` fest, und die [`BorderWidth`](xref:Xamarin.Forms.Button.BorderWidth)-Eigenschaft legt die Breite des Rahmens fest. Per Standardeinstellung ist `Button` rechteckig. Jedoch können die Ecken abgerundet werden, indem die [`CornerRadius`](xref:Xamarin.Forms.Button.CornerRadius)-Eigenschaft auf einen geeigneten Wert festgelegt wird. Zusätzlich wird die Größe von `Button` geändert, indem die Eigenschaften [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) und [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) festgelegt werden.

1. Klicken Sie in der Symbolleiste von Visual Studio auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Remotesimulator oder Android-Emulator zu starten: Beachten Sie, wie sich die Darstellung von [`Button`](xref:Xamarin.Forms.Button) geändert hat:

    [![Screenshot: Schaltfläche mit geänderter visueller Darstellung unter iOS und Android](../images/change-button-appearance.png "Schaltfläche mit geänderter Darstellung")](../images/change-button-appearance-large.png#lightbox "Schaltfläche mit geänderter Darstellung")

    Weitere Informationen zur Einstellung der [`Button`](xref:Xamarin.Forms.Button)-Darstellung finden Sie im Abschnitt [Darstellung der Schaltfläche](~/xamarin-forms/user-interface/button.md#button-appearance) des Artikels [Xamarin.Forms Button (Xamarin.Forms-Schaltfläche)](~/xamarin-forms/user-interface/button.md).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Bearbeiten Sie in **MainPage.xaml** die [`Button`](xref:Xamarin.Forms.Button)-Deklaration, um die visuelle Darstellung zu ändern:

    ```xaml
    <Button Text="Click me"
            Clicked="OnButtonClicked"
            TextColor="Blue"
            BackgroundColor="Aqua"
            BorderColor="Red"
            BorderWidth="5"
            CornerRadius="5"
            WidthRequest="150"
            HeightRequest="75" />
    ```

    Dieser Code legt Eigenschaften fest, die die visuelle Darstellung von [`Button`](xref:Xamarin.Forms.Button) ändern. Die [`TextColor`](xref:Xamarin.Forms.Button.TextColor)-Eigenschaft legt die Farbe des `Button`-Texts fest, und die [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor)-Eigenschaft legt die Farbe des Texthintergrunds fest. Die [`BorderColor`](xref:Xamarin.Forms.Button.BorderColor)-Eigenschaft legt die Farbe des Bereichs um `Button` fest, und die [`BorderWidth`](xref:Xamarin.Forms.Button.BorderWidth)-Eigenschaft legt die Breite des Rahmens fest. Per Standardeinstellung ist `Button` rechteckig. Jedoch können die Ecken abgerundet werden, indem die [`CornerRadius`](xref:Xamarin.Forms.Button.CornerRadius)-Eigenschaft auf einen geeigneten Wert festgelegt wird. Zusätzlich wird die Größe von `Button` geändert, indem die Eigenschaften [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) und [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) festgelegt werden.

1. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Simulator oder Android-Emulator zu starten. Beachten Sie, wie sich die Darstellung von [`Button`](xref:Xamarin.Forms.Button) geändert hat:

    [![Screenshot: Schaltfläche mit geänderter visueller Darstellung unter iOS und Android](../images/change-button-appearance.png "Schaltfläche mit geänderter Darstellung")](../images/change-button-appearance-large.png#lightbox "Schaltfläche mit geänderter Darstellung")

    Weitere Informationen zur Einstellung der [`Button`](xref:Xamarin.Forms.Button)-Darstellung finden Sie im Abschnitt [Darstellung der Schaltfläche](~/xamarin-forms/user-interface/button.md#button-appearance) des Artikels [Xamarin.Forms Button (Xamarin.Forms-Schaltfläche)](~/xamarin-forms/user-interface/button.md).
