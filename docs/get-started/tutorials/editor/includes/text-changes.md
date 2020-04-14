---
ms.openlocfilehash: 653d3677f96d7da78af61531c535b1b7db684e7e
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2020
ms.locfileid: "77135107"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. Ändern Sie in **MainPage.xaml** die [`Editor`](xref:Xamarin.Forms.Editor)-Deklaration, sodass ein Handler für die [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged)- und [`Completed`](xref:Xamarin.Forms.Editor.Completed)-Ereignisse festgelegt wird:

    ```xaml
    <Editor Placeholder="Enter multi-line text here"
            HeightRequest="200"
            TextChanged="OnEditorTextChanged"
            Completed="OnEditorCompleted" />
    ```

    Dieser Code legt das [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged)-Ereignis auf einen Ereignishandler mit dem Namen `OnEditorTextChanged` fest, und das [`Completed`](xref:Xamarin.Forms.Editor.Completed)-Ereignis auf einen Handler mit dem Namen `OnEditorCompleted`. Beide Ereignishandler werden im nächsten Schritt erstellt.

1. Erweitern Sie **MainPage.xaml** im **Projektmappen-Explorer** im Projekt **EditorTutorial**, und doppelklicken Sie dann auf die Datei **MainPage.xaml.cs**, um sie zu öffnen. Fügen Sie anschließend in der Datei **MainPage.xaml.cs** der Klasse die Ereignishandler `OnEditorTextChanged` und `OnEditorCompleted` hinzu:

    ```csharp
    void OnEditorTextChanged(object sender, TextChangedEventArgs e)
    {
        string oldText = e.OldTextValue;
        string newText = e.NewTextValue;
    }

    void OnEditorCompleted(object sender, EventArgs e)
    {
        string text = ((Editor)sender).Text;
    }
    ```

    Wenn sich der Text im [`Editor`](xref:Xamarin.Forms.Editor) ändert, wird die `OnEditorTextChanged`-Methode ausgeführt. Das `sender`-Argument ist das `Editor`-Objekt, das für das Auslösen des `TextChanged`-Ereignisses verantwortlich ist und für den Zugriff auf das `Editor`-Objekt verwendet werden kann. Das [`TextChangedEventArgs`](xref:Xamarin.Forms.TextChangedEventArgs)-Argument stellt die alten und neuen Textwerte, vor und nach der Textänderung, bereit.

    Nach Abschluss der Bearbeitung wird die `OnEditorCompleted`-Methode ausgeführt. Dies wird erreicht, indem der Fokus vom [`Editor`](xref:Xamarin.Forms.Editor) genommen oder zusätzlich auf die Schaltfläche „Fertig“ in iOS geklickt wird. Das `sender`-Argument ist das `Editor`-Objekt, das für das Auslösen des `TextChanged`-Ereignisses verantwortlich ist und für den Zugriff auf das `Editor`-Objekt verwendet werden kann.

    > [!IMPORTANT]
    > Jeder in einen [`Editor`](xref:Xamarin.Forms.Editor) eingegebene Text wird in der [`Text`](xref:Xamarin.Forms.InputView.Text)-Eigenschaft gespeichert.

1. Klicken Sie in der Symbolleiste von Visual Studio auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Remotesimulator oder Android-Emulator zu starten:

    [![Screenshot: Editor mit Text unter iOS und Android](../images/text-changes.png "Editor mit Text")](../images/text-changes-large.png#lightbox "Editor mit Text")

    Legen Sie Breakpoints in den beiden Ereignishandlern fest, geben Sie in den [`Editor`](xref:Xamarin.Forms.Editor) Text ein, und beobachten Sie das Auslösen des [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged)-Ereignisses. Nehmen Sie den Fokus vom `Editor`, um das Auslösen des [`Completed`](xref:Xamarin.Forms.Entry.Completed)-Ereignisses zu beobachten.

    Weitere Informationen zu den [`Editor`](xref:Xamarin.Forms.Editor)-Ereignissen finden Sie unter [Interaktivität](~/xamarin-forms/user-interface/text/editor.md#interactivity) im Artikel [Xamarin.Forms-Editor](~/xamarin-forms/user-interface/text/editor.md).

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Ändern Sie in **MainPage.xaml** die [`Editor`](xref:Xamarin.Forms.Editor)-Deklaration, sodass ein Handler für die [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged)- und [`Completed`](xref:Xamarin.Forms.Editor.Completed)-Ereignisse festgelegt wird:

    ```xaml
    <Editor Placeholder="Enter multi-line text here"
            HeightRequest="200"
            TextChanged="OnEditorTextChanged"
            Completed="OnEditorCompleted" />
    ```

    Dieser Code legt das [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged)-Ereignis auf einen Ereignishandler mit dem Namen `OnEditorTextChanged` fest, und das [`Completed`](xref:Xamarin.Forms.Editor.Completed)-Ereignis auf einen Handler mit dem Namen `OnEditorCompleted`. Beide Ereignishandler werden im nächsten Schritt erstellt.

1. Erweitern Sie **MainPage.xaml** im **Lösungspad** im Projekt **EditorTutorial**, und doppelklicken Sie dann auf die Datei **MainPage.xaml.cs**, um sie zu öffnen. Fügen Sie anschließend in der Datei **MainPage.xaml.cs** der Klasse die Ereignishandler `OnEditorTextChanged` und `OnEditorCompleted` hinzu:

    ```csharp
    void OnEditorTextChanged(object sender, TextChangedEventArgs e)
    {
        string oldText = e.OldTextValue;
        string newText = e.NewTextValue;
    }

    void OnEditorCompleted(object sender, EventArgs e)
    {
        string text = ((Editor)sender).Text;
    }
    ```

    Wenn sich der Text im [`Editor`](xref:Xamarin.Forms.Editor) ändert, wird die `OnEditorTextChanged`-Methode ausgeführt. Das `sender`-Argument ist das `Editor`-Objekt, das für das Auslösen des `TextChanged`-Ereignisses verantwortlich ist und für den Zugriff auf das `Editor`-Objekt verwendet werden kann. Das [`TextChangedEventArgs`](xref:Xamarin.Forms.TextChangedEventArgs)-Argument stellt die alten und neuen Textwerte, vor und nach der Textänderung, bereit.

    Nach Abschluss der Bearbeitung wird die `OnEditorCompleted`-Methode ausgeführt. Dies wird erreicht, indem der Fokus vom [`Editor`](xref:Xamarin.Forms.Editor) genommen oder zusätzlich auf die Schaltfläche „Fertig“ in iOS geklickt wird. Das `sender`-Argument ist das `Editor`-Objekt, das für das Auslösen des `TextChanged`-Ereignisses verantwortlich ist und für den Zugriff auf das `Editor`-Objekt verwendet werden kann.

    > [!IMPORTANT]
    > Jeder in einen [`Editor`](xref:Xamarin.Forms.Editor) eingegebene Text wird in der [`Text`](xref:Xamarin.Forms.InputView.Text)-Eigenschaft gespeichert.

1. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Simulator oder Android-Emulator zu starten.

    [![Screenshot: Editor mit Text unter iOS und Android](../images/text-changes.png "Editor mit Text")](../images/text-changes-large.png#lightbox "Editor mit Text")

    Legen Sie Breakpoints in den beiden Ereignishandlern fest, geben Sie in den [`Editor`](xref:Xamarin.Forms.Editor) Text ein, und beobachten Sie das Auslösen des [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged)-Ereignisses. Nehmen Sie den Fokus vom `Editor`, um das Auslösen des [`Completed`](xref:Xamarin.Forms.Entry.Completed)-Ereignisses zu beobachten.

    Weitere Informationen zu den [`Editor`](xref:Xamarin.Forms.Editor)-Ereignissen finden Sie unter [Interaktivität](~/xamarin-forms/user-interface/text/editor.md#interactivity) im Artikel [Xamarin.Forms-Editor](~/xamarin-forms/user-interface/text/editor.md).
