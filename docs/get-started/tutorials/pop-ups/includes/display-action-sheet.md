---
ms.openlocfilehash: 87eb021e6cc571a9a5522697cde2aa11ee991308
ms.sourcegitcommit: 6ad272c2c7b0c3c30e375ad17ce6296ac1ce72b2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/23/2019
ms.locfileid: "66193826"
---

Xamarin.Forms verfügt über ein modales Popupelement – auch bekannt als Aktionsblatt –, das dazu verwendet werden kann, Benutzer durch eine Aufgabe zu führen. In dieser Übung verwenden Sie die [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*)-Methode aus der [`Page`](xref:Xamarin.Forms.Page)-Klasse, um ein Aktionsblatt anzuzeigen, das Benutzer durch eine Aufgabe führt.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Fügen Sie in der Datei **MainPage.xaml** eine neue [`Button`](xref:Xamarin.Forms.Button)-Deklaration hinzu, die ein Aktionsblatt anzeigt:

    ```xaml
    <Button Text="Display action sheet"
            Clicked="OnDisplayActionSheetButtonClicked" />
    ```

     Die [`Button.Text`](xref:Xamarin.Forms.Button.Text)-Eigenschaft gibt den Text an, der auf der `Button` angezeigt wird. Außerdem wird das [`Clicked`](xref:Xamarin.Forms.Button.Clicked)-Ereignis auf einen Ereignishandler mit dem Namen `OnDisplayActionSheetButtonClicked` festgelegt, welcher im nächsten Schritt erstellt wird.

1. Erweitern Sie **MainPage.xaml** im **Projektmappen-Explorer** im Projekt **PopupsTutorial**, und doppelklicken Sie dann auf die Datei **MainPage.xaml.cs**, um sie zu öffnen. Fügen Sie dann in der Datei **MainPage.xaml.cs** der Klasse den `OnDisplayActionSheetButtonClicked`-Ereignishandler hinzu:

    ```csharp
    async void OnDisplayActionSheetButtonClicked(object sender, EventArgs e)
    {
        string action = await DisplayActionSheet("Send to?", "Cancel", null, "Email", "Twitter", "Facebook");
        Console.WriteLine("Action: " + action);
    }
    ```

    Wenn die [`Button`](xref:Xamarin.Forms.Button) angetippt wird, wird die `OnDisplayActionSheetButtonClicked`-Methode ausgeführt. Diese Methode ruft die [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*)-Methode auf, um dem Benutzer Alternativen zum Durchlaufen einer Aufgabe anzuzeigen. Sobald der Benutzer eine der beiden Alternativen ausgewählt hat, wird die Auswahl als `string` zurückgegeben.

    > [!IMPORTANT]
    > Die [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*)-Methode ist asynchron und sollte immer mit dem `await`-Schlüsselwort erwartet werden.

1. Klicken Sie in der Symbolleiste von Visual Studio auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Remotesimulator oder Android-Emulator zu starten: Tippen Sie anschließend auf die [`Button`](xref:Xamarin.Forms.Button), die Sie der [`ContentPage`](xref:Xamarin.Forms.ContentPage) hinzugefügt haben:

    [![Screenshot: Aktionsblatt unter iOS und Android](../images/actionsheet.png "Aktionsblatt, das Benutzer durch eine Aufgabe führt")](../images/actionsheet-large.png#lightbox "Aktionsblatt, das Benutzer durch eine Aufgabe führt")

    Berücksichtigen Sie nach der Auswahl einer Alternative im Dialogfeld „Aktionsblatt“, dass die Auswahl an das **Ausgabe**-Fenster in Visual Studio ausgegeben wird.

    Weitere Informationen zum Anzeigen von Aktionsblättern finden Sie unter [Führen von Benutzern durch Aufgaben](~/xamarin-forms/user-interface/pop-ups.md#guide-users-through-tasks) im Leitfaden [Anzeigen von Popupelementen](~/xamarin-forms/user-interface/pop-ups.md).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Fügen Sie in der Datei **MainPage.xaml** eine neue [`Button`](xref:Xamarin.Forms.Button)-Deklaration hinzu, die ein Aktionsblatt anzeigt:

    ```xaml
    <Button Text="Display action sheet"
            Clicked="OnDisplayActionSheetButtonClicked" />
    ```

    Die [`Button.Text`](xref:Xamarin.Forms.Button.Text)-Eigenschaft gibt den Text an, der auf der `Button` angezeigt wird. Außerdem wird das [`Clicked`](xref:Xamarin.Forms.Button.Clicked)-Ereignis auf einen Ereignishandler mit dem Namen `OnDisplayActionSheetButtonClicked` festgelegt, welcher im nächsten Schritt erstellt wird.

1. Erweitern Sie **MainPage.xaml** im **Lösungspad** im Projekt **PopupsTutorial**, und doppelklicken Sie dann auf die Datei **MainPage.xaml.cs**, um sie zu öffnen. Fügen Sie dann in der Datei **MainPage.xaml.cs** der Klasse den `OnDisplayActionSheetButtonClicked`-Ereignishandler hinzu:

    ```csharp
    async void OnDisplayActionSheetButtonClicked(object sender, EventArgs e)
    {
        string action = await DisplayActionSheet("Send to?", "Cancel", null, "Email", "Twitter", "Facebook");
        Console.WriteLine("Action: " + action);
    }
    ```

    Wenn die [`Button`](xref:Xamarin.Forms.Button) angetippt wird, wird die `OnDisplayActionSheetButtonClicked`-Methode ausgeführt. Diese Methode ruft die [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*)-Methode auf, um dem Benutzer Alternativen zum Durchlaufen einer Aufgabe anzuzeigen. Sobald der Benutzer eine der beiden Alternativen ausgewählt hat, wird die Auswahl als `string` zurückgegeben.

    > [!IMPORTANT]
    > Die [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*)-Methode ist asynchron und sollte immer mit dem `await`-Schlüsselwort erwartet werden.

1. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Simulator oder Android-Emulator zu starten. Tippen Sie anschließend auf die [`Button`](xref:Xamarin.Forms.Button), die Sie der [`ContentPage`](xref:Xamarin.Forms.ContentPage) hinzugefügt haben:

    [![Screenshot: Aktionsblatt unter iOS und Android](../images/actionsheet.png "Aktionsblatt, das Benutzer durch eine Aufgabe führt")](../images/actionsheet-large.png#lightbox "Aktionsblatt, das Benutzer durch eine Aufgabe führt")

    Berücksichtigen Sie nach der Auswahl einer Alternative im Dialogfeld „Aktionsblatt“, dass die Auswahl an das **Anwendungsausgabe**-Fenster in Visual Studio für Mac ausgegeben wird.

    Weitere Informationen zum Anzeigen von Aktionsblättern finden Sie unter [Führen von Benutzern durch Aufgaben](~/xamarin-forms/user-interface/pop-ups.md#guide-users-through-tasks) im Leitfaden [Anzeigen von Popupelementen](~/xamarin-forms/user-interface/pop-ups.md).
