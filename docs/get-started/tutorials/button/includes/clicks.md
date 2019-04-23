---
ms.openlocfilehash: d1f7d209eaaca62a55b768646f51024609057a63
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61372938"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Ändern Sie in **MainPage.xaml** die [`Button`](xref:Xamarin.Forms.Button)-Deklaration, sodass ein Handler für das [`Clicked`](xref:Xamarin.Forms.Button.Clicked)-Ereignis festgelegt wird:

    ```xaml
    <Button Text="Click me"
            Clicked="OnButtonClicked" />
    ```

    Dieser Code legt das [`Clicked`](xref:Xamarin.Forms.Button.Clicked)-Ereignis auf einen Ereignishandler namens `OnButtonClicked` fest, der im nächsten Schritt erstellt wird.

1. Erweitern Sie **MainPage.xaml** im **Projektmappen-Explorer** im Projekt **ButtonTutorial**, und doppelklicken Sie dann auf die Datei **MainPage.xaml.cs**, um sie zu öffnen. Fügen Sie dann in der Datei **MainPage.xaml.cs** der Klasse den `OnButtonClicked`-Ereignishandler hinzu:

    ```csharp
    void OnButtonClicked(object sender, EventArgs e)
    {
        (sender as Button).Text = "Click me again!";
    }
    ```

    Wenn die [`Button`](xref:Xamarin.Forms.Button) angetippt wird, wird die `OnButtonClicked`-Methode ausgeführt. Das `sender`-Argument ist das `Button`-Objekt, das für das Auslösen des `Clicked`-Ereignisses verantwortlich ist und für den Zugriff auf das `Button`-Objekt verwendet werden kann. Dieser Ereignishandler aktualisiert den von der `Button` angezeigten Text.

    > [!NOTE]
    > Neben dem `Clicked`-Ereignis definiert das `Button`-Element auch die Ereignisse [`Pressed`](xref:Xamarin.Forms.Button.Pressed) und [`Released`](xref:Xamarin.Forms.Button.Released). Weitere Informationen finden Sie im Leitfaden [Xamarin.Forms Button (Xamarin.Forms-Schaltfläche)](~/xamarin-forms/user-interface/button.md#pressing-and-releasing-the-button) unter [Pressing and releasing the button (Drücken und Loslassen der Schaltfläche)](~/xamarin-forms/user-interface/button.md).

1. Klicken Sie in der Symbolleiste von Visual Studio auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Remotesimulator oder Android-Emulator zu starten: Klicken Sie auf die [`Button`](xref:Xamarin.Forms.Button), und beobachten Sie, dass sich der darauf angezeigte Text ändert:

    [![Screenshot: Schaltflächentext ändert sich nach dem Klicken in iOS und Android](../images/handle-button-click.png "Behandeln eines Schaltflächenklicks")](../images/handle-button-click-large.png#lightbox "Behandeln eines Schaltflächenklicks")

    Weitere Informationen zum Behandeln von Schaltflächenklicks finden Sie im Leitfaden [Xamarin.Forms Button (Xamarin.Forms-Schaltfläche)](~/xamarin-forms/user-interface/button.md#handling-button-clicks) unter [Handling button clicks (Behandlung von Schaltflächenklicks)](~/xamarin-forms/user-interface/button.md).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Ändern Sie in **MainPage.xaml** die [`Button`](xref:Xamarin.Forms.Button)-Deklaration, sodass ein Handler für das [`Clicked`](xref:Xamarin.Forms.Button.Clicked)-Ereignis festgelegt wird:

    ```xaml
    <Button Text="Click me"
            Clicked="OnButtonClicked" />
    ```

    Dieser Code legt das [`Clicked`](xref:Xamarin.Forms.Button.Clicked)-Ereignis auf einen Ereignishandler namens `OnButtonClicked` fest, der im nächsten Schritt erstellt wird.

1. Erweitern Sie **MainPage.xaml** im **Lösungspad** im Projekt **ButtonTutorial**, und doppelklicken Sie dann auf die Datei **MainPage.xaml.cs**, um sie zu öffnen. Fügen Sie dann in der Datei **MainPage.xaml.cs** der Klasse den `OnButtonClicked`-Ereignishandler hinzu:

    ```csharp
    void OnButtonClicked(object sender, EventArgs e)
    {
        (sender as Button).Text = "Click me again!";
    }
    ```

    Wenn die [`Button`](xref:Xamarin.Forms.Button) angetippt wird, wird die `OnButtonClicked`-Methode ausgeführt. Das `sender`-Argument ist das `Button`-Objekt, das für das Auslösen des `Clicked`-Ereignisses verantwortlich ist und für den Zugriff auf das `Button`-Objekt verwendet werden kann. Dieser Ereignishandler aktualisiert den von der `Button` angezeigten Text.

    > [!NOTE]
    > Neben dem `Clicked`-Ereignis definiert das `Button`-Element auch die Ereignisse [`Pressed`](xref:Xamarin.Forms.Button.Pressed) und [`Released`](xref:Xamarin.Forms.Button.Released). Weitere Informationen finden Sie im Leitfaden [Xamarin.Forms Button (Xamarin.Forms-Schaltfläche)](~/xamarin-forms/user-interface/button.md#pressing-and-releasing-the-button) unter [Pressing and releasing the button (Drücken und Loslassen der Schaltfläche)](~/xamarin-forms/user-interface/button.md).

1. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Simulator oder Android-Emulator zu starten. Klicken Sie auf die [`Button`](xref:Xamarin.Forms.Button), und beobachten Sie, dass sich der darauf angezeigte Text ändert:

    [![Screenshot: Schaltflächentext ändert sich nach dem Klicken in iOS und Android](../images/handle-button-click.png "Behandeln eines Schaltflächenklicks")](../images/handle-button-click-large.png#lightbox "Behandeln eines Schaltflächenklicks")

    Weitere Informationen zum Behandeln von Schaltflächenklicks finden Sie im Leitfaden [Xamarin.Forms Button (Xamarin.Forms-Schaltfläche)](~/xamarin-forms/user-interface/button.md#handling-button-clicks) unter [Handling button clicks (Behandlung von Schaltflächenklicks)](~/xamarin-forms/user-interface/button.md).
