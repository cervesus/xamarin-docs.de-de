---
ms.openlocfilehash: deb3516cc134a8b2eecba8460931003de8bb312f
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2020
ms.locfileid: "77135139"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. Ändern Sie in **MainPage.xaml** die [`Entry`](xref:Xamarin.Forms.Entry)-Deklaration, sodass ein Handler für die [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged)- und [`Completed`](xref:Xamarin.Forms.Entry.Completed)-Ereignisse festgelegt wird:

    ```xaml
    <Entry Placeholder="Enter text"
           TextChanged="OnEntryTextChanged"
           Completed="OnEntryCompleted" />
    ```

    Dieser Code legt das [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged)-Ereignis auf einen Ereignishandler mit dem Namen `OnEntryTextChanged` fest, und das [`Completed`](xref:Xamarin.Forms.Entry.Completed)-Ereignis auf einen Handler mit dem Namen `OnEntryCompleted`. Beide Ereignishandler werden im nächsten Schritt erstellt.

1. Erweitern Sie **MainPage.xaml** im **Projektmappen-Explorer** im Projekt **EntryTutorial**, und doppelklicken Sie dann auf die Datei **MainPage.xaml.cs**, um sie zu öffnen. Fügen Sie anschließend in der Datei **MainPage.xaml.cs** der Klasse die Ereignishandler `OnEntryTextChanged` und `OnEntryCompleted` hinzu:

    ```csharp
    void OnEntryTextChanged(object sender, TextChangedEventArgs e)
    {
        string oldText = e.OldTextValue;
        string newText = e.NewTextValue;
    }

    void OnEntryCompleted(object sender, EventArgs e)
    {
        string text = ((Entry)sender).Text;
    }
    ```

    Wenn sich der Text im [`Entry`](xref:Xamarin.Forms.Entry) ändert, wird die `OnEntryTextChanged`-Methode ausgeführt. Das `sender`-Argument ist das `Entry`-Objekt, das für das Auslösen des `TextChanged`-Ereignisses verantwortlich ist und für den Zugriff auf das `Entry`-Objekt verwendet werden kann. Das [`TextChangedEventArgs`](xref:Xamarin.Forms.TextChangedEventArgs)-Argument stellt die alten und neuen Textwerte, vor und nach der Textänderung, bereit.

    Wenn Sie den Text im [`Entry`](xref:Xamarin.Forms.Entry) mit der Eingabetaste abschließen, wird die `OnEntryCompleted`-Methode ausgeführt. Das `sender`-Argument ist das `Entry`-Objekt, das für das Auslösen des `TextChanged`-Ereignisses verantwortlich ist und für den Zugriff auf das `Entry`-Objekt verwendet werden kann.

    > [!IMPORTANT]
    > Jeder in einen [`Entry`](xref:Xamarin.Forms.Entry) eingegebene Text wird in der [`Text`](xref:Xamarin.Forms.InputView.Text)-Eigenschaft gespeichert.

1. Klicken Sie in der Symbolleiste von Visual Studio auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Remotesimulator oder Android-Emulator zu starten:

    [![Screenshot: Eintrag mit Text unter iOS und Android](../images/text-changes.png "Eintrag mit Text")](../images/text-changes-large.png#lightbox "Eintrag mit Text")

    Legen Sie Breakpoints in den beiden Ereignishandlern fest, geben Sie in den [`Entry`](xref:Xamarin.Forms.Entry) Text ein, und beobachten Sie das Auslösen der [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged)- und [`Completed`](xref:Xamarin.Forms.Entry.Completed)-Ereignisse.

    Weitere Informationen zu den [`Entry`](xref:Xamarin.Forms.Entry)-Ereignissen finden Sie unter [Ereignisse und Interaktivität](~/xamarin-forms/user-interface/text/entry.md#events-and-interactivity) im Artikel [Xamarin.Forms-Eintrag](~/xamarin-forms/user-interface/text/entry.md).

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Ändern Sie in **MainPage.xaml** die [`Entry`](xref:Xamarin.Forms.Entry)-Deklaration, sodass ein Handler für die [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged)- und [`Completed`](xref:Xamarin.Forms.Entry.Completed)-Ereignisse festgelegt wird:

    ```xaml
    <Entry Placeholder="Enter text"
           TextChanged="OnEntryTextChanged"
           Completed="OnEntryCompleted" />
    ```

    Dieser Code legt das [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged)-Ereignis auf einen Ereignishandler mit dem Namen `OnEntryTextChanged` fest, und das [`Completed`](xref:Xamarin.Forms.Entry.Completed)-Ereignis auf einen Handler mit dem Namen `OnEntryCompleted`. Beide Ereignishandler werden im nächsten Schritt erstellt.

1. Erweitern Sie **MainPage.xaml** im **Lösungspad** im Projekt **EntryTutorial**, und doppelklicken Sie dann auf die Datei **MainPage.xaml.cs**, um sie zu öffnen. Fügen Sie anschließend in der Datei **MainPage.xaml.cs** der Klasse die Ereignishandler `OnEntryTextChanged` und `OnEntryCompleted` hinzu:

    ```csharp
    void OnEntryTextChanged(object sender, TextChangedEventArgs e)
    {
        string oldText = e.OldTextValue;
        string newText = e.NewTextValue;
    }

    void OnEntryCompleted(object sender, EventArgs e)
    {
        string text = ((Entry)sender).Text;
    }
    ```

    Wenn sich der Text im [`Entry`](xref:Xamarin.Forms.Entry) ändert, wird die `OnEntryTextChanged`-Methode ausgeführt. Das `sender`-Argument ist das `Entry`-Objekt, das für das Auslösen des `TextChanged`-Ereignisses verantwortlich ist und für den Zugriff auf das `Entry`-Objekt verwendet werden kann. Das [`TextChangedEventArgs`](xref:Xamarin.Forms.TextChangedEventArgs)-Argument stellt die alten und neuen Textwerte, vor und nach der Textänderung, bereit.

    Wenn Sie den Text im [`Entry`](xref:Xamarin.Forms.Entry) mit der Eingabetaste abschließen, wird die `OnEntryCompleted`-Methode ausgeführt. Das `sender`-Argument ist das `Entry`-Objekt, das für das Auslösen des `TextChanged`-Ereignisses verantwortlich ist und für den Zugriff auf das `Entry`-Objekt verwendet werden kann.

    > [!IMPORTANT]
    > Jeder in einen [`Entry`](xref:Xamarin.Forms.Entry) eingegebene Text wird in der [`Text`](xref:Xamarin.Forms.InputView.Text)-Eigenschaft gespeichert.

1. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Simulator oder Android-Emulator zu starten.

    [![Screenshot: Eintrag mit Text unter iOS und Android](../images/text-changes.png "Eintrag mit Text")](../images/text-changes-large.png#lightbox "Eintrag mit Text")

    Legen Sie Breakpoints in den beiden Ereignishandlern fest, geben Sie in den [`Entry`](xref:Xamarin.Forms.Entry) Text ein, und beobachten Sie das Auslösen der [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged)- und [`Completed`](xref:Xamarin.Forms.Entry.Completed)-Ereignisse.

    Weitere Informationen zu den [`Entry`](xref:Xamarin.Forms.Entry)-Ereignissen finden Sie unter [Ereignisse und Interaktivität](~/xamarin-forms/user-interface/text/entry.md#events-and-interactivity) im Artikel [Xamarin.Forms-Eintrag](~/xamarin-forms/user-interface/text/entry.md).
