---
ms.openlocfilehash: 27a3393e6eda9f26ea15003edc5022246ff4deff
ms.sourcegitcommit: 3f0e4f10e5def19122588bb05f26ab2baa9df6eb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2020
ms.locfileid: "67659811"
---
In dieser Übung erstellen Sie eine Benutzeroberfläche, um die zuvor erstellten Datenzugriffsklassen zu verwenden.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Doppelklicken Sie im **Projektmappen-Explorer** im Projekt **LocalDatabaseTutorial** auf die Datei **MainPage.xaml**, um sie zu öffnen. Entfernen Sie dann in **MainPage.xaml** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="LocalDatabaseTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Entry x:Name="nameEntry"
                   Placeholder="Enter name" />
            <Entry x:Name="ageEntry"
                   Placeholder="Enter age" />
            <Button Text="Add to Database"
                    Clicked="OnButtonClicked" />
            <ListView x:Name="listView">
                <ListView.ItemTemplate>
                    <DataTemplate>
                        <TextCell Text="{Binding Name}"
                                  Detail="{Binding Age}" />
                    </DataTemplate>
                </ListView.ItemTemplate>
            </ListView>
        </StackLayout>
    </ContentPage>
    ```

    Dieser Code definiert deklarativ die Benutzeroberfläche für die Seite, die aus zwei [`Entry`](xref:Xamarin.Forms.Entry)-Instanzen, einem [`Button`](xref:Xamarin.Forms.Button)-Objekt und einer [`ListView`](xref:Xamarin.Forms.ListView)-Klasse in einer [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse besteht. Jede `Entry`-Klasse hat ihre [`Placeholder`](xref:Xamarin.Forms.Entry.Placeholder)-Eigenschaft festgelegt, die den Platzhaltertext angibt, die vor der Benutzereingabe angezeigt wird. Das `Button`-Objekt legt sein [`Clicked`](xref:Xamarin.Forms.Button.Clicked)-Ereignis auf einen Ereignishandler namens `OnButtonClicked` fest, der im nächsten Schritt erstellt wird. Die `ListView`-Klasse legt ihre [`ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate)-Eigenschaft auf eine [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)-Klasse fest, die eine [`TextCell`](xref:Xamarin.Forms.TextCell)-Klasse verwendet, um das Auftreten jeder Zeile in der [`ListView`](xref:Xamarin.Forms.ListView)-Klasse zu bestimmen. Die Daten aus der `TextCell`-Klasse binden ihre [`Text`](xref:Xamarin.Forms.TextCell.Text)- und [`Detail`](xref:Xamarin.Forms.TextCell.Detail)-Eigenschaften jeweils an die `Name`- und `Age`-Eigenschaften jedes `Person`-Objekts.

    Außerdem haben die [`Entry`](xref:Xamarin.Forms.Entry)-Instanzen und die [`ListView`](xref:Xamarin.Forms.ListView)-Klasse Namen, die mit dem `x:Name`-Attribut angegeben werden. Dadurch kann die CodeBehind-Datei mithilfe des zugewiesenen Namens auf das Objekt zugreifen.

1. Erweitern Sie **MainPage.xaml** im **Projektmappen-Explorer** im Projekt **LocalDatabaseTutorial**, und doppelklicken Sie dann auf die Datei **MainPage.xaml.cs**, um sie zu öffnen. Fügen Sie dann in der Datei **MainPage.xaml.cs** der Klasse die `OnAppearing`-Außerkraftsetzung und den `OnButtonClicked`-Ereignishandler hinzu:

    ```csharp
    protected override async void OnAppearing()
    {
        base.OnAppearing();
        listView.ItemsSource = await App.Database.GetPeopleAsync();
    }

    async void OnButtonClicked(object sender, EventArgs e)
    {
        if (!string.IsNullOrWhiteSpace(nameEntry.Text) && !string.IsNullOrWhiteSpace(ageEntry.Text))
        {
            await App.Database.SavePersonAsync(new Person
            {
                Name = nameEntry.Text,
                Age = int.Parse(ageEntry.Text)
            });

            nameEntry.Text = ageEntry.Text = string.Empty;
            listView.ItemsSource = await App.Database.GetPeopleAsync();
        }
    }
    ```

    Die `OnAppearing`-Methode befüllt die [`ListView`](xref:Xamarin.Forms.ListView)-Klasse mit sämtlichen Daten, die in der Datenbank gespeichert sind. Die `OnButtonClicked`-Methode, die ausgeführt wird, wenn auf das [`Button`](xref:Xamarin.Forms.Button)-Objekt getippt wird, speichert die eingegebenen Daten in der Datenbank, bevor beide [`Entry`](xref:Xamarin.Forms.Entry)-Instanzen gelöscht werden, und aktualisiert die Daten in der `ListView`-Klasse.

    > [!NOTE]
    > Die Außerkraftsetzung der `OnAppearing`-Methode wird ausgeführt, nachdem die [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Klasse angelegt wurde, aber noch bevor sie angezeigt wird. Deshalb ist dies ein guter Ort, um den Inhalt der Xamarin.Forms-Ansichten festzulegen.

1. Klicken Sie in der Symbolleiste von Visual Studio auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Remotesimulator oder Android-Emulator zu starten:

    Geben Sie mehrere Datenelemente ein, und tippen Sie für jedes Datenelement auf das [`Button`](xref:Xamarin.Forms.Button)-Objekt. Dadurch werden die Daten in der Datenbank gespeichert, und die [`ListView`](xref:Xamarin.Forms.ListView)-Klasse wieder mit allen Daten der Datenbank befüllt:

    [![Screenshot: Datenpersistenz der lokalen SQLite.NET-Datenbank unter iOS und Android](../images/consume-data-access-classes.png "Datenpersistenz der lokalen Datenbank")](../images/consume-data-access-classes-large.png#lightbox "Datenpersistenz der lokalen Datenbank")

    Weitere Informationen zu lokalen Datenbanken in Xamarin.Forms finden Sie unter [Lokale Datenbanken von Xamarin.Forms](~/xamarin-forms/data-cloud/data/databases.md).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Doppelklicken Sie im **Lösungspad** im Projekt **LocalDatabaseTutorial** auf die Datei **MainPage.xaml**, um sie zu öffnen. Entfernen Sie dann in **MainPage.xaml** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="LocalDatabaseTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Entry x:Name="nameEntry"
                   Placeholder="Enter name" />
            <Entry x:Name="ageEntry"
                   Placeholder="Enter age" />
            <Button Text="Add to Database"
                    Clicked="OnButtonClicked" />
            <ListView x:Name="listView">
                <ListView.ItemTemplate>
                    <DataTemplate>
                        <TextCell Text="{Binding Name}"
                                  Detail="{Binding Age}" />
                    </DataTemplate>
                </ListView.ItemTemplate>
            </ListView>
        </StackLayout>
    </ContentPage>
    ```

    Dieser Code definiert deklarativ die Benutzeroberfläche für die Seite, die aus zwei [`Entry`](xref:Xamarin.Forms.Entry)-Instanzen, einem [`Button`](xref:Xamarin.Forms.Button)-Objekt und einer [`ListView`](xref:Xamarin.Forms.ListView)-Klasse in einer [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse besteht. Jede `Entry`-Klasse hat ihre [`Placeholder`](xref:Xamarin.Forms.Entry.Placeholder)-Eigenschaft festgelegt, die den Platzhaltertext angibt, die vor der Benutzereingabe angezeigt wird. Das `Button`-Objekt legt sein [`Clicked`](xref:Xamarin.Forms.Button.Clicked)-Ereignis auf einen Ereignishandler namens `OnButtonClicked` fest, der im nächsten Schritt erstellt wird. Die `ListView`-Klasse legt ihre [`ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate)-Eigenschaft auf eine [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)-Klasse fest, die eine [`TextCell`](xref:Xamarin.Forms.TextCell)-Klasse verwendet, um das Auftreten jeder Zeile in der [`ListView`](xref:Xamarin.Forms.ListView)-Klasse zu bestimmen. Die Daten aus der `TextCell`-Klasse binden ihre [`Text`](xref:Xamarin.Forms.TextCell.Text)- und [`Detail`](xref:Xamarin.Forms.TextCell.Detail)-Eigenschaften jeweils an die `Name`- und `Age`-Eigenschaften jedes `Person`-Objekts.

    Außerdem haben die [`Entry`](xref:Xamarin.Forms.Entry)-Instanzen und die [`ListView`](xref:Xamarin.Forms.ListView)-Klasse Namen, die mit dem `x:Name`-Attribut angegeben werden. Dadurch kann die CodeBehind-Datei mithilfe des zugewiesenen Namens auf das Objekt zugreifen.

1. Erweitern Sie **MainPage.xaml** im **Lösungspad** im Projekt **LocalDatabaseTutorial**, und doppelklicken Sie dann auf die Datei **MainPage.xaml.cs**, um sie zu öffnen. Fügen Sie dann in der Datei **MainPage.xaml.cs** der Klasse die `OnAppearing`-Außerkraftsetzung und den `OnButtonClicked`-Ereignishandler hinzu:

    ```csharp
    protected override async void OnAppearing()
    {
        base.OnAppearing();
        listView.ItemsSource = await App.Database.GetPeopleAsync();
    }

    async void OnButtonClicked(object sender, EventArgs e)
    {
        if (!string.IsNullOrWhiteSpace(nameEntry.Text) && !string.IsNullOrWhiteSpace(ageEntry.Text))
        {
            await App.Database.SavePersonAsync(new Person
            {
                Name = nameEntry.Text,
                Age = int.Parse(ageEntry.Text)
            });

            nameEntry.Text = ageEntry.Text = string.Empty;
            listView.ItemsSource = await App.Database.GetPeopleAsync();
        }
    }
    ```

    Die `OnAppearing`-Methode befüllt die [`ListView`](xref:Xamarin.Forms.ListView)-Klasse mit sämtlichen Daten, die in der Datenbank gespeichert sind. Die `OnButtonClicked`-Methode, die ausgeführt wird, wenn auf das [`Button`](xref:Xamarin.Forms.Button)-Objekt getippt wird, speichert die eingegebenen Daten in der Datenbank, bevor beide [`Entry`](xref:Xamarin.Forms.Entry)-Instanzen gelöscht werden, und aktualisiert die Daten in der `ListView`-Klasse.

    > [!NOTE]
    > Die Außerkraftsetzung der `OnAppearing`-Methode wird ausgeführt, nachdem die [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Klasse angelegt wurde, aber noch bevor sie angezeigt wird. Deshalb ist dies ein guter Ort, um den Inhalt der Xamarin.Forms-Ansichten festzulegen.

1. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Simulator oder Android-Emulator zu starten:

    Geben Sie mehrere Datenelemente ein, und tippen Sie für jedes Datenelement auf das [`Button`](xref:Xamarin.Forms.Button)-Objekt. Dadurch werden die Daten in der Datenbank gespeichert, und die [`ListView`](xref:Xamarin.Forms.ListView)-Klasse wieder mit allen Daten der Datenbank befüllt:

    [![Screenshot: Datenpersistenz der lokalen SQLite.NET-Datenbank unter iOS und Android](../images/consume-data-access-classes.png "Datenpersistenz der lokalen Datenbank")](../images/consume-data-access-classes-large.png#lightbox "Datenpersistenz der lokalen Datenbank")

    Weitere Informationen zu lokalen Datenbanken in Xamarin.Forms finden Sie unter [Lokale Datenbanken von Xamarin.Forms](~/xamarin-forms/data-cloud/data/databases.md).
