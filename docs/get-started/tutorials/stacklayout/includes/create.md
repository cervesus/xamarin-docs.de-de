# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Starten Sie Visual Studio, und erstellen Sie eine neue leere Xamarin.Forms-App namens **StackLayoutTutorial**. Stellen Sie sicher, dass die App .NET Standard als Mechanismus für freigegebenen Code verwendet.

    > [!IMPORTANT]
    > Für die C#- und XAML-Codeausschnitte in diesem Tutorial ist es erforderlich, dass die Projektmappe **StackLayoutTutorial** genannt wird. Die Verwendung eines anderen Namens führt zu Buildfehlern, wenn Sie Code aus diesem Tutorial in die Projektmappe kopieren.

    Weitere Informationen zur .NET Standard-Bibliothek, die erstellt wird, finden Sie unter [Anatomy of a Xamarin.Forms application (Aufbau einer Xamarin.Forms-Anwendung)](~/get-started/first-app/index.md) im Artikel [Xamarin.Forms Quickstart Deep Dive (Weitere Details zum Xamarin.Forms-Schnellstart)](~/get-started/first-app/index.md).

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

    [![Screenshot: untergeordnete Ansichten in einem StackLayout, unter iOS und Android](../images/create-stacklayout.png "StackLayout mit Bezeichnungsinstanzen")](../images/create-stacklayout-large.png#lightbox "StackLayout mit Bezeichnungsinstanzen")

    Weitere Informationen zur [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse finden Sie unter [Xamarin.Forms StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Starten Sie Visual Studio für Mac, und erstellen Sie eine neue leere Xamarin.Forms-App namens **StackLayoutTutorial**. Stellen Sie sicher, dass die App .NET Standard als Mechanismus für freigegebenen Code verwendet.

    > [!IMPORTANT]
    > Für die C#- und XAML-Codeausschnitte in diesem Tutorial ist es erforderlich, dass die Projektmappe **StackLayoutTutorial** genannt wird. Die Verwendung eines anderen Namens führt zu Buildfehlern, wenn Sie Code aus diesem Tutorial in die Projektmappe kopieren.

    Weitere Informationen zur .NET Standard-Bibliothek, die erstellt wird, finden Sie unter [Anatomy of a Xamarin.Forms application (Aufbau einer Xamarin.Forms-Anwendung)](~/get-started/first-app/index.md) im Artikel [Xamarin.Forms Quickstart Deep Dive (Weitere Details zum Xamarin.Forms-Schnellstart)](~/get-started/first-app/index.md).

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

1. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Simulator oder Android-Emulator zu starten:

    [![Screenshot: untergeordnete Ansichten in einem StackLayout, unter iOS und Android](../images/create-stacklayout.png "StackLayout mit Bezeichnungsinstanzen")](../images/create-stacklayout-large.png#lightbox "StackLayout mit Bezeichnungsinstanzen")

    Weitere Informationen zur [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse finden Sie unter [Xamarin.Forms StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md).
