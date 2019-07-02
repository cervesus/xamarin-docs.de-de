---
ms.openlocfilehash: 875f00b379879aa131d37018f89e475170e5320e
ms.sourcegitcommit: a153623a69b5cb125f672df8007838afa32e9edf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2019
ms.locfileid: "67277260"
---
Xamarin.Forms verfügt über ein modales Popupelement, das als „Warnung“ bezeichnet wird, um dem Benutzer eine Warnung anzuzeigen oder einfache Fragen zu stellen. In dieser Übung verwenden Sie die [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*)-Methode aus der [`Page`](xref:Xamarin.Forms.Page)-Klasse, um dem Benutzer eine Warnung anzuzeigen und eine einfache Frage zu stellen.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Für dieses Tutorial benötigen Sie das neueste Release von Visual Studio 2019 und die Workload **Mobile-Entwicklung mit .NET**. Außerdem müssen Sie über einen gekoppelten Mac verfügen, um die Tutorial-App unter iOS zu kompilieren. Informationen zur Installation der Xamarin-Plattform finden Sie unter [Installieren von Xamarin](~/get-started/installation/index.md). Informationen zum Verbinden von Visual Studio 2019 mit einem Mac-Buildhost finden Sie unter [Durchführen einer Kopplung mit einem Mac für die Xamarin.iOS-Entwicklung](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

1. Starten Sie Visual Studio, und erstellen Sie eine neue leere Xamarin.Forms-App mit dem Namen **PopupsTutorial**. Stellen Sie sicher, dass die App .NET Standard als Mechanismus für freigegebenen Code verwendet.

    > [!IMPORTANT]
    > Für die C#- und XAML-Codeausschnitte in diesem Tutorial ist es erforderlich, dass die Projektmappe **PopupsTutorial** genannt wird. Die Verwendung eines anderen Namens führt zu Buildfehlern, wenn Sie Code aus diesem Tutorial in die Projektmappe kopieren.

    Weitere Informationen zur .NET Standard-Bibliothek, die erstellt wird, finden Sie unter [Anatomy of a Xamarin.Forms application (Aufbau einer Xamarin.Forms-Anwendung)](~/get-started/first-app/index.md) im Artikel [Xamarin.Forms Quickstart Deep Dive (Weitere Details zum Xamarin.Forms-Schnellstart)](~/get-started/first-app/index.md).

1. Doppelklicken Sie im **Projektmappen-Explorer** im Projekt **PopupsTutorial** auf die Datei **MainPage.xaml**, um sie zu öffnen. Entfernen Sie dann in **MainPage.xaml** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="PopupsTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Button Text="Display alert"
                    Clicked="OnDisplayAlertButtonClicked" />
            <Button Text="Display alert question"
                    Clicked="OnDisplayAlertQuestionButtonClicked" />
        </StackLayout>
    </ContentPage>
    ```

    Dieser Code definiert deklarativ die Benutzeroberfläche für die Seite, die aus zwei [`Button`](xref:Xamarin.Forms.Button)-Objekten in einer [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse besteht. Die [`Button.Text`](xref:Xamarin.Forms.Button.Text)-Eigenschaften geben den Text an, der in jedem `Button`-Objekt angezeigt wird, und die [`Clicked`](xref:Xamarin.Forms.Button.Clicked)-Ereignisse werden als Ereignishandler festgelegt, die im nächsten Schritt erstellt werden.

1. Erweitern Sie **MainPage.xaml** im **Projektmappen-Explorer** im Projekt **PopupsTutorial**, und doppelklicken Sie dann auf die Datei **MainPage.xaml.cs**, um sie zu öffnen. Fügen Sie dann in der Datei **MainPage.xaml.cs** der Klasse die Ereignishandler `OnDisplayAlertButtonClicked` und `OnDisplayAlertQuestionButtonClicked` hinzu:

    ```csharp
    async void OnDisplayAlertButtonClicked(object sender, EventArgs e)
    {
        await DisplayAlert("Alert", "This is an alert.", "OK");
    }

    async void OnDisplayAlertQuestionButtonClicked(object sender, EventArgs e)
    {
        bool response = await DisplayAlert("Save?", "Would you like to save your data?", "Yes", "No");
        Console.WriteLine("Save data: " + response);
    }
    ```

    Wenn ein [`Button`](xref:Xamarin.Forms.Button)-Objekt angetippt wird, wird die entsprechende Ereignishandlermethode ausgeführt. Die `OnDisplayAlertButtonClicked`-Methode ruft die [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*)-Methode auf, um eine modale Warnung mit einer einzelnen Schaltfläche zum Abbrechen anzuzeigen. Sobald die Warnung geschlossen wurde, kann der Benutzer weiter mit der Anwendung interagieren.

    Die `OnDisplayAlertQuestionButtonClicked`-Methode ruft ein Überladen der [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*)-Methode auf, um eine modale Warnung mit einer Schaltfläche zum Akzeptieren und einer Schaltfläche zum Abbrechen anzuzeigen. Sobald der Benutzer eine der beiden Schaltflächen ausgewählt hat, wird die Auswahl als `boolean` zurückgegeben.

    > [!IMPORTANT]
    > Die [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*)-Methode ist asynchron und sollte immer mit dem `await`-Schlüsselwort erwartet werden.

1. Klicken Sie in der Symbolleiste von Visual Studio auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Remotesimulator oder Android-Emulator zu starten: Tippen Sie dann auf das erste [`Button`](xref:Xamarin.Forms.Button)-Objekt:

    [![Screenshot: Warnung auf iOS und Android](../images/alert.png "Warnung")](../images/alert-large.png#lightbox "Warnung")

    Schließen Sie die Warnung, und tippen Sie dann auf das zweite [`Button`](xref:Xamarin.Forms.Button)-Objekt:

    [![Screenshot: Warnung, die eine Frage stellt, auf iOS und Android](../images/alert-question.png " Warnung, die eine Frage stellt")](../images/alert-question-large.png#lightbox "Warnung, die eine Frage stellt")

    Sie können sehen, dass die Antwort als Ausgabe im **Ausgabefenster** von Visual Studio angezeigt wird, nachdem eine Antwort für die Frage ausgewählt wurde.

    Weitere Informationen zum Anzeigen von Warnungen finden Sie unter [Anzeigen einer Warnung](~/xamarin-forms/user-interface/pop-ups.md#display-an-alert) im Leitfaden [Anzeigen von Popupelementen](~/xamarin-forms/user-interface/pop-ups.md).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Für dieses Tutorial benötigen Sie Visual Studio für Mac (neuestes Release) mit Plattformunterstützung für iOS und Android. Zudem ist Xcode (neuestes Release) erforderlich. Weitere Informationen zur Installation der Xamarin-Plattform finden Sie unter [Installieren von Xamarin](~/get-started/installation/index.md).

1. Starten Sie Visual Studio für Mac, und erstellen Sie eine neue leere Xamarin.Forms-App mit dem Namen **PopupsTutorial**. Stellen Sie sicher, dass die App .NET Standard als Mechanismus für freigegebenen Code verwendet.

    > [!IMPORTANT]
    > Für die C#- und XAML-Codeausschnitte in diesem Tutorial ist es erforderlich, dass die Projektmappe **PopupsTutorial** genannt wird. Die Verwendung eines anderen Namens führt zu Buildfehlern, wenn Sie Code aus diesem Tutorial in die Projektmappe kopieren.

    Weitere Informationen zur .NET Standard-Bibliothek, die erstellt wird, finden Sie unter [Anatomy of a Xamarin.Forms application (Aufbau einer Xamarin.Forms-Anwendung)](~/get-started/first-app/index.md) im Artikel [Xamarin.Forms Quickstart Deep Dive (Weitere Details zum Xamarin.Forms-Schnellstart)](~/get-started/first-app/index.md).

1. Doppelklicken Sie im **Lösungspad** im Projekt **PopupsTutorial** auf die Datei **MainPage.xaml**, um sie zu öffnen. Entfernen Sie dann in **MainPage.xaml** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="PopupsTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Button Text="Display alert"
                    Clicked="OnDisplayAlertButtonClicked" />
            <Button Text="Display alert question"
                    Clicked="OnDisplayAlertQuestionButtonClicked" />
        </StackLayout>
    </ContentPage>
    ```

    Dieser Code definiert deklarativ die Benutzeroberfläche für die Seite, die aus zwei [`Button`](xref:Xamarin.Forms.Button)-Objekten in einer [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse besteht. Die [`Button.Text`](xref:Xamarin.Forms.Button.Text)-Eigenschaften geben den Text an, der in jedem `Button`-Objekt angezeigt wird, und die [`Clicked`](xref:Xamarin.Forms.Button.Clicked)-Ereignisse werden als Ereignishandler festgelegt, die im nächsten Schritt erstellt werden.

1. Erweitern Sie **MainPage.xaml** im **Lösungspad** im Projekt **PopupsTutorial**, und doppelklicken Sie dann auf die Datei **MainPage.xaml.cs**, um sie zu öffnen. Fügen Sie dann in der Datei **MainPage.xaml.cs** der Klasse die Ereignishandler `OnDisplayAlertButtonClicked` und `OnDisplayAlertQuestionButtonClicked` hinzu:

    ```csharp
    async void OnDisplayAlertButtonClicked(object sender, EventArgs e)
    {
        await DisplayAlert("Alert", "This is an alert.", "OK");
    }

    async void OnDisplayAlertQuestionButtonClicked(object sender, EventArgs e)
    {
        bool response = await DisplayAlert("Save?", "Would you like to save your data?", "Yes", "No");
        Console.WriteLine("Save data: " + response);
    }
    ```

    Wenn ein [`Button`](xref:Xamarin.Forms.Button)-Objekt angetippt wird, wird die entsprechende Ereignishandlermethode ausgeführt. Die `OnDisplayAlertButtonClicked`-Methode ruft die [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*)-Methode auf, um eine modale Warnung mit einer einzelnen Schaltfläche zum Abbrechen anzuzeigen. Sobald die Warnung geschlossen wurde, kann der Benutzer weiter mit der Anwendung interagieren.

    Die `OnDisplayAlertQuestionButtonClicked`-Methode ruft ein Überladen der [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*)-Methode auf, um eine modale Warnung mit einer Schaltfläche zum Akzeptieren und einer Schaltfläche zum Abbrechen anzuzeigen. Sobald der Benutzer eine der beiden Schaltflächen ausgewählt hat, wird die Auswahl als `boolean` zurückgegeben.

    > [!IMPORTANT]
    > Die [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*)-Methode ist asynchron und sollte immer mit dem `await`-Schlüsselwort erwartet werden.

1. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Simulator oder Android-Emulator zu starten. Tippen Sie dann auf das erste [`Button`](xref:Xamarin.Forms.Button)-Objekt:

    [![Screenshot: Warnung auf iOS und Android](../images/alert.png "Warnung")](../images/alert-large.png#lightbox "Warnung")

    Schließen Sie die Warnung, und tippen Sie dann auf das zweite [`Button`](xref:Xamarin.Forms.Button)-Objekt:

    [![Screenshot: Warnung, die eine Frage stellt, auf iOS und Android](../images/alert-question.png " Warnung, die eine Frage stellt")](../images/alert-question-large.png#lightbox "Warnung, die eine Frage stellt")

    Sie können sehen, dass die Antwort als Ausgabe im **Ausgabefenster** von Visual Studio für Mac angezeigt wird, nachdem eine Antwort für die Frage ausgewählt wurde.

    Weitere Informationen zum Anzeigen von Warnungen finden Sie unter [Anzeigen einer Warnung](~/xamarin-forms/user-interface/pop-ups.md#display-an-alert) im Leitfaden [Anzeigen von Popupelementen](~/xamarin-forms/user-interface/pop-ups.md).
