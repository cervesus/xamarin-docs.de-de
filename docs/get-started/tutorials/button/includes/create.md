# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Starten Sie Visual Studio, und erstellen Sie eine neue leere Xamarin.Forms-App mit dem Namen **ButtonTutorial**. Stellen Sie sicher, dass die App .NET Standard als Mechanismus für freigegebenen Code verwendet.

    > [!IMPORTANT]
    > Für die C#- und XAML-Codeausschnitte in diesem Tutorial ist es erforderlich, dass die Projektmappe **ButtonTutorial** genannt wird. Die Verwendung eines anderen Namens führt zu Buildfehlern, wenn Sie Code aus diesem Tutorial in die Projektmappe kopieren.

    Weitere Informationen zur .NET Standard-Bibliothek, die erstellt wird, finden Sie unter [Aufbau einer Xamarin.Forms-Anwendung](~/get-started/first-app/index.md) im Artikel [Xamarin.Forms Quickstart Deep Dive (Ausführliche Erläuterungen zum Xamarin.Forms-Schnellstart)](~/get-started/first-app/index.md).

1. Doppelklicken Sie im **Projektmappen-Explorer** im Projekt **ButtonTutorial** auf die Datei **MainPage.xaml**, um sie zu öffnen. Entfernen Sie dann in **MainPage.xaml** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>    
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="ButtonTutorial.MainPage">
       <StackLayout Margin="20,35,20,20">
           <Button Text="Click me" />
       </StackLayout>
    </ContentPage>
    ```

    In diesem Codeausschnitt wird die Benutzeroberfläche deklarativ für die Seite definiert, die ein [`Button`](xref:Xamarin.Forms.Button)-Element in einem [`StackLayout`](xref:Xamarin.Forms.StackLayout) enthält. Die [`Button.Text`](xref:Xamarin.Forms.Button.Text)-Eigenschaft gibt den Text an, der auf dem `Button`-Element angezeigt wird.

1. Klicken Sie in der Symbolleiste von Visual Studio auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Remotesimulator oder Android-Emulator zu starten:

    [![Screenshot: Schaltfläche unter iOS und Android](../images/create-button.png "Schaltfläche mit Text")](../images/create-button-large.png#lightbox "Schaltfläche mit Text")

    Beachten Sie, dass ein [`Button`](xref:Xamarin.Forms.Button)-Element standardmäßig die gesamte verfügbare Fläche einnimmt. In diesem Fall entspricht das der Breite des übergeordneten Elements ([`StackLayout`](xref:Xamarin.Forms.StackLayout)).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Starten Sie Visual Studio für Mac, und erstellen Sie eine neue leere Xamarin.Forms-App mit dem Namen **ButtonTutorial**. Stellen Sie sicher, dass die App .NET Standard als Mechanismus für freigegebenen Code verwendet.

    > [!IMPORTANT]
    > Für die C#- und XAML-Codeausschnitte in diesem Tutorial ist es erforderlich, dass die Projektmappe **ButtonTutorial** genannt wird. Die Verwendung eines anderen Namens führt zu Buildfehlern, wenn Sie Code aus diesem Tutorial in die Projektmappe kopieren.

    Weitere Informationen zur .NET Standard-Bibliothek, die erstellt wird, finden Sie unter [Aufbau einer Xamarin.Forms-Anwendung](~/get-started/first-app/index.md) im Artikel [Xamarin.Forms Quickstart Deep Dive (Ausführliche Erläuterungen zum Xamarin.Forms-Schnellstart)](~/get-started/first-app/index.md).

1. Doppelklicken Sie im **Lösungspad** im Projekt **ButtonTutorial** auf die Datei **MainPage.xaml**, um sie zu öffnen. Entfernen Sie dann in **MainPage.xaml** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="ButtonTutorial.MainPage">
       <StackLayout Margin="20,35,20,20">
           <Button Text="Click me" />
       </StackLayout>
    </ContentPage>
    ```

    In diesem Codeausschnitt wird die Benutzeroberfläche deklarativ für die Seite definiert, die ein [`Button`](xref:Xamarin.Forms.Button)-Element in einem [`StackLayout`](xref:Xamarin.Forms.StackLayout) enthält. Die [`Button.Text`](xref:Xamarin.Forms.Button.Text)-Eigenschaft gibt den Text an, der auf dem `Button`-Element angezeigt wird.

1. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Simulator oder Android-Emulator zu starten.

    [![Screenshot: Schaltfläche unter iOS und Android](../images/create-button.png "Schaltfläche mit Text")](../images/create-button-large.png#lightbox "Schaltfläche mit Text")

    Beachten Sie, dass ein [`Button`](xref:Xamarin.Forms.Button)-Element standardmäßig die gesamte verfügbare Fläche einnimmt. In diesem Fall entspricht das der Breite des übergeordneten Elements ([`StackLayout`](xref:Xamarin.Forms.StackLayout)).
