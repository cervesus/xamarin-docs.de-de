---
ms.openlocfilehash: 5d9d5e4eb757d6afd1c13cb4851edd23feaa6e65
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2020
ms.locfileid: "77135071"
---
Die Unterklasse [`Application`](xref:Xamarin.Forms.Application) enthält ein statisches [`Properties`](xref:Xamarin.Forms.Application.Properties)-Wörterbuch, das zum Speichern von Daten bei Zustandsänderungen des Lebenszyklus verwendet werden kann. Das Wörterbuch verwendet einen `string`-Schlüssel und speichert einen `object`-Wert. Das Wörterbuch wird automatisch auf dem Gerät gespeichert und wird bei Neustart der Anwendung erneut aufgefüllt.

> [!IMPORTANT]
> Das [`Properties`](xref:Xamarin.Forms.Application.Properties)-Wörterbuch kann nur primitive Typen für die Speicherung serialisieren.

In dieser Übung passen Sie die Anwendung an, um den Text einer [`Entry`](xref:Xamarin.Forms.Entry)-Klasse im Hintergrund beizubehalten und den Text für die `Entry`-Klasse wiederherzustellen, wenn die Anwendung erneut gestartet wird.

# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. Erweitern Sie **App.xaml** im **Projektmappen-Explorer** im Projekt **AppLifecycleTutorial**, und doppelklicken Sie dann auf die Datei **App.xaml.cs**, um sie zu öffnen. Entfernen Sie anschließend in **App.xaml.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```csharp
    using System;
    using Xamarin.Forms;

    namespace AppLifecycleTutorial
    {
        public partial class App : Application
        {
            const string displayText = "displayText";

            public string DisplayText { get; set; }

            public App()
            {
                InitializeComponent();

                MainPage = new MainPage();
            }

            protected override void OnStart()
            {
                Console.WriteLine("OnStart");

                if (Properties.ContainsKey(displayText))
                {
                    DisplayText = (string)Properties[displayText];
                }
            }

            protected override void OnSleep()
            {
                Console.WriteLine("OnSleep");
                Properties[displayText] = DisplayText;
            }

            protected override void OnResume()
            {
                Console.WriteLine("OnResume");
            }
        }
    }
    ```

    Mit diesem Code wird eine `DisplayText`-Eigenschaft und eine `displayText`-Konstante definiert. Wenn die Anwendung minimiert oder beendet wird, fügt die Überschreibung der `OnSleep`-Methode den Eigenschaftswert `DisplayText` zum [`Properties`](xref:Xamarin.Forms.Application.Properties)-Wörterbuch für einen `displayText`-Schlüssel hinzu. Wenn die Anwendung dann gestartet wird, wird die Eigenschaft `DisplayText` als Wert für den Schlüssel wiederhergestellt, sofern das `Properties`-Wörterbuch den Schlüssel `displayText` enthält.

    > [!IMPORTANT]
    > Überprüfen Sie immer, ob das [`Properties`](xref:Xamarin.Forms.Application.Properties)-Wörterbuch einen Schlüssel enthält, bevor Sie darauf zugreifen, um unerwartete Fehler zu vermeiden.

    Dies ist nicht notwendig, um Daten aus dem [`Properties`](xref:Xamarin.Forms.Application.Properties)-Wörterbuch in der `OnResume`-Methodenüberladung wiederherzustellen. Das liegt daran, dass der Zustand noch im Arbeitsspeicher vorhanden ist, wenn die Anwendung minimiert wird.

1. Doppelklicken Sie im **Projektmappen-Explorer** im Projekt **AppLifecycleTutorial** auf die Datei **MainPage.xaml**, um sie zu öffnen. Entfernen Sie dann in **MainPage.xaml** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="AppLifecycleTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Entry x:Name="entry"
                   Placeholder="Enter text here"
                   Completed="OnEntryCompleted" />
        </StackLayout>
    </ContentPage>
    ```

    In diesem Codeausschnitt wird die Benutzeroberfläche deklarativ für die Seite definiert, die ein [`Entry`](xref:Xamarin.Forms.Entry)-Element in einem [`StackLayout`](xref:Xamarin.Forms.StackLayout) enthält. Die [`Entry.Placeholder`](xref:Xamarin.Forms.InputView.Placeholder)-Eigenschaft legt einen Platzhaltertext fest, der angezeigt wird, wenn `Entry` zum ersten Mal angezeigt wurde. Außerdem wird ein Ereignishandler namens `OnEntryCompleted` mit dem Ereignis [`Completed`](xref:Xamarin.Forms.Entry.Completed) registriert. Außerdem hat die `Entry`-Klasse einen Namen, der mit dem `x:Name`-Attribut festgelegt wird. Dadurch kann die CodeBehind-Datei mithilfe des zugewiesenen Namens auf das Objekt `Entry` zugreifen.

1. Erweitern Sie **MainPage.xaml** im **Projektmappen-Explorer** im Projekt **AppLifecycleTutorial**, und doppelklicken Sie dann auf die Datei **MainPage.xaml.cs**, um sie zu öffnen. Fügen Sie dann in der Datei **MainPage.xaml.cs** eine Überschreibung für die Methode `OnAppearing` und den Ereignishandler `OnEntryCompleted` zur Klasse hinzu:

    ```csharp
    protected override void OnAppearing()
    {
        base.OnAppearing();

        entry.Text = (Application.Current as App).DisplayText;
    }

    void OnEntryCompleted(object sender, EventArgs e)
    {
        (Application.Current as App).DisplayText = entry.Text;
    }
    ```

    Die `OnAppearing`-Methode ruft den Wert der Eigenschaft `App.DisplayText` ab und legt diesen als [`Text`](xref:Xamarin.Forms.InputView.Text)-Eigenschaftswert der [`Entry`](xref:Xamarin.Forms.Entry)-Klasse fest.

    > [!NOTE]
    > Die Außerkraftsetzung der `OnAppearing`-Methode wird ausgeführt, nachdem die [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Klasse angelegt wurde, aber noch bevor sie angezeigt wird. Deshalb ist dies ein guter Ort, um den Inhalt der Xamarin.Forms-Ansichten festzulegen.

    Sobald der Text in der [`Entry`](xref:Xamarin.Forms.Entry)-Klasse mit der EINGABETASTE fertig gestellt wird, wird die Methode `OnEntryCompleted` ausgeführt, und der `Entry`-Text wird in der Eigenschaft `App.DisplayText` gespeichert.

1. Klicken Sie in der Symbolleiste von Visual Studio auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Remotesimulator oder Android-Emulator zu starten:

    Geben Sie einen Text in die [`Entry`](xref:Xamarin.Forms.Entry)-Klasse ein, und drücken Sie die EINGABETASTE. Minimieren Sie dann die Anwendung, indem Sie auf die Home-Taste tippen, um die Methode `OnSleep` aufzurufen.

    Starten Sie die Anwendung schließlich erneut über Visual Studio. Der Text, den Sie zuvor in die [`Entry`](xref:Xamarin.Forms.Entry)-Klasse eingegeben haben, sollte wiederhergestellt werden:

    [![Screenshot: Eintrag, dessen Texteigenschaft auch bei Zustandsänderungen des Lebenszyklus beibehalten wird, unter iOS und Android](../images/persist-data.png "Eintrag, dessen Texteigenschaft auch bei Zustandsänderungen des Lebenszyklus beibehalten wird")](../images/persist-data-large.png#lightbox "Eintrag, dessen Texteigenschaft auch bei Zustandsänderungen des Lebenszyklus beibehalten wird")

    Weitere Informationen zum Beibehalten von Daten im Eigenschaftenwörterbuch finden Sie unter [Eigenschaftenwörterbuch](~/xamarin-forms/app-fundamentals/application-class.md#properties-dictionary) im Leitfaden zur [Xamarin.Forms-App-Klasse](~/xamarin-forms/app-fundamentals/application-class.md).

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Erweitern Sie **App.xaml** im **Lösungspad** im Projekt **AppLifecycleTutorial**, und doppelklicken Sie dann auf die Datei **App.xaml.cs**, um sie zu öffnen. Entfernen Sie anschließend in **App.xaml.cs** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```csharp
    using System;
    using Xamarin.Forms;

    namespace AppLifecycleTutorial
    {
        public partial class App : Application
        {
            const string displayText = "displayText";

            public string DisplayText { get; set; }

            public App()
            {
                InitializeComponent();

                MainPage = new MainPage();
            }

            protected override void OnStart()
            {
                Console.WriteLine("OnStart");

                if (Properties.ContainsKey(displayText))
                {
                    DisplayText = (string)Properties[displayText];
                }
            }

            protected override void OnSleep()
            {
                Console.WriteLine("OnSleep");
                Properties[displayText] = DisplayText;
            }

            protected override void OnResume()
            {
                Console.WriteLine("OnResume");
            }
        }
    }
    ```

    Mit diesem Code wird eine `DisplayText`-Eigenschaft und eine `displayText`-Konstante definiert. Wenn die Anwendung minimiert oder beendet wird, fügt die Überschreibung der `OnSleep`-Methode den Eigenschaftswert `DisplayText` zum [`Properties`](xref:Xamarin.Forms.Application.Properties)-Wörterbuch für einen `displayText`-Schlüssel hinzu. Wenn die Anwendung dann gestartet wird, wird die Eigenschaft `DisplayText` als Wert für den Schlüssel wiederhergestellt, sofern das `Properties`-Wörterbuch den Schlüssel `displayText` enthält.

    > [!IMPORTANT]
    > Überprüfen Sie immer, ob das [`Properties`](xref:Xamarin.Forms.Application.Properties)-Wörterbuch einen Schlüssel enthält, bevor Sie darauf zugreifen, um unerwartete Fehler zu vermeiden.

    Dies ist nicht notwendig, um Daten aus dem [`Properties`](xref:Xamarin.Forms.Application.Properties)-Wörterbuch in der `OnResume`-Methodenüberladung wiederherzustellen. Das liegt daran, dass der Zustand noch im Arbeitsspeicher vorhanden ist, wenn die Anwendung minimiert wird.

1. Doppelklicken Sie im **Lösungspad** im Projekt **AppLifecycleTutorial** auf die Datei **MainPage.xaml**, um sie zu öffnen. Entfernen Sie dann in **MainPage.xaml** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="AppLifecycleTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Entry x:Name="entry"
                   Placeholder="Enter text here"
                   Completed="OnEntryCompleted" />
        </StackLayout>
    </ContentPage>
    ```

    In diesem Codeausschnitt wird die Benutzeroberfläche deklarativ für die Seite definiert, die ein [`Entry`](xref:Xamarin.Forms.Entry)-Element in einem [`StackLayout`](xref:Xamarin.Forms.StackLayout) enthält. Die [`Entry.Placeholder`](xref:Xamarin.Forms.InputView.Placeholder)-Eigenschaft legt einen Platzhaltertext fest, der angezeigt wird, wenn `Entry` zum ersten Mal angezeigt wurde. Außerdem wird ein Ereignishandler namens `OnEntryCompleted` mit dem Ereignis [`Completed`](xref:Xamarin.Forms.Entry.Completed) registriert. Außerdem hat die `Entry`-Klasse einen Namen, der mit dem `x:Name`-Attribut festgelegt wird. Dadurch kann die CodeBehind-Datei mithilfe des zugewiesenen Namens auf das Objekt `Entry` zugreifen.

1. Erweitern Sie **MainPage.xaml** im **Lösungspad** im Projekt **AppLifecycleTutorial**, und doppelklicken Sie dann auf die Datei **MainPage.xaml.cs**, um sie zu öffnen. Fügen Sie dann in der Datei **MainPage.xaml.cs** eine Überschreibung für die Methode `OnAppearing` und den Ereignishandler `OnEntryCompleted` zur Klasse hinzu:

    ```csharp
    protected override void OnAppearing()
    {
        base.OnAppearing();

        entry.Text = (Application.Current as App).DisplayText;
    }

    void OnEntryCompleted(object sender, EventArgs e)
    {
        (Application.Current as App).DisplayText = entry.Text;
    }
    ```

    Die `OnAppearing`-Methode ruft den Wert der Eigenschaft `App.DisplayText` ab und legt diesen als [`Text`](xref:Xamarin.Forms.InputView.Text)-Eigenschaftswert der [`Entry`](xref:Xamarin.Forms.Entry)-Klasse fest.

    > [!NOTE]
    > Die Außerkraftsetzung der `OnAppearing`-Methode wird ausgeführt, nachdem die [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Klasse angelegt wurde, aber noch bevor sie angezeigt wird. Deshalb ist dies ein guter Ort, um den Inhalt der Xamarin.Forms-Ansichten festzulegen.

    Sobald der Text in der [`Entry`](xref:Xamarin.Forms.Entry)-Klasse mit der EINGABETASTE fertig gestellt wird, wird die Methode `OnEntryCompleted` ausgeführt, und der `Entry`-Text wird in der Eigenschaft `App.DisplayText` gespeichert.

1. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Simulator oder Android-Emulator zu starten:

    Geben Sie einen Text in die [`Entry`](xref:Xamarin.Forms.Entry)-Klasse ein, und drücken Sie die EINGABETASTE. Minimieren Sie dann die Anwendung, indem Sie auf die Home-Taste tippen, um die Methode `OnSleep` aufzurufen.

    Starten Sie die Anwendung schließlich erneut über Visual Studio für Mac. Der Text, den Sie zuvor in die [`Entry`](xref:Xamarin.Forms.Entry)-Klasse eingegeben haben, sollte wiederhergestellt werden:

    [![Screenshot: Eintrag, dessen Texteigenschaft auch bei Zustandsänderungen des Lebenszyklus beibehalten wird, unter iOS und Android](../images/persist-data.png "Eintrag, dessen Texteigenschaft auch bei Zustandsänderungen des Lebenszyklus beibehalten wird")](../images/persist-data-large.png#lightbox "Eintrag, dessen Texteigenschaft auch bei Zustandsänderungen des Lebenszyklus beibehalten wird")

    Weitere Informationen zum Beibehalten von Daten im Eigenschaftenwörterbuch finden Sie unter [Eigenschaftenwörterbuch](~/xamarin-forms/app-fundamentals/application-class.md#properties-dictionary) im Leitfaden zur [Xamarin.Forms-App-Klasse](~/xamarin-forms/app-fundamentals/application-class.md).
