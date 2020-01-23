---
ms.openlocfilehash: 2185fa243d2bccea046be5c91a2b1e9ed365edfe
ms.sourcegitcommit: 3f0e4f10e5def19122588bb05f26ab2baa9df6eb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2020
ms.locfileid: "74062885"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Ändern Sie in **MainPage.xaml** die [`ListView`](xref:Xamarin.Forms.ListView)-Deklaration, sodass Handler für die [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected)- und [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped)-Ereignisse festgelegt werden:

    ```xaml
    <ListView ItemsSource="{Binding Monkeys}"
              ItemSelected="OnListViewItemSelected"
              ItemTapped="OnListViewItemTapped" />
    ```

    Dieser Code legt das [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected)-Ereignis auf einen Ereignishandler mit dem Namen `OnListViewItemSelected` fest, und das [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped)-Ereignis auf einen Handler mit dem Namen `OnListViewItemTapped`. Beide Ereignishandler werden im nächsten Schritt erstellt.

1. Erweitern Sie **MainPage.xaml** im **Projektmappen-Explorer** im Projekt **ListViewTutorial**, und doppelklicken Sie dann auf die Datei **MainPage.xaml.cs**, um sie zu öffnen. Fügen Sie dann in der Datei **MainPage.xaml.cs** der Klasse die Ereignishandler `OnListViewItemSelected` und `OnListViewItemTapped` hinzu:

    ```csharp
    void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs e)
    {
        Monkey selectedItem = e.SelectedItem as Monkey;
    }

    void OnListViewItemTapped(object sender, ItemTappedEventArgs e)
    {
        Monkey tappedItem = e.Item as Monkey;
    }
    ```

    Wenn ein Element in der [`ListView`](xref:Xamarin.Forms.ListView) angetippt wird, werden die [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected)- und [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped)-Ereignisse ausgelöst, welche die `OnListViewItemSelected`- bzw. `OnListItemTapped`-Methoden ausführen. Das `sender`-Argument an beide Methoden ist das `ListView`-Objekt, das für das Auslösen des Ereignisses verantwortlich ist und für den Zugriff auf das `ListView`-Objekt verwendet werden kann. Das [`SelectedItemChangedEventArgs`](xref:Xamarin.Forms.SelectedItemChangedEventArgs)-Argument in der `OnListViewItemSelected`-Methode stellt das ausgewählte Element bereit, und das [`ItemTappedEventArgs`](xref:Xamarin.Forms.ItemTappedEventArgs)-Argument in der `OnListViewItemTapped`-Methode stellt das angetippte Element bereit.

    > [!IMPORTANT]
    > Das [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected)-Ereignis wird nur ausgelöst, wenn ein neues Element in der [`ListView`](xref:Xamarin.Forms.ListView) ausgewählt wird. Wenn das gleiche Element zweimal angetippt wird, werden deshalb zwei [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped)-Ereignisse ausgelöst, aber nur ein `ItemSelected`-Ereignis wird ausgelöst.

1. Klicken Sie in der Symbolleiste von Visual Studio auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Remotesimulator oder Android-Emulator zu starten:

    [![Screenshot: ListView, die auf Elementauswahl und Tippen unter iOS und Android reagiert](../images/item-selection.png "ListView-Elementauswahl")](../images/item-selection-large.png#lightbox "ListView-Elementauswahl")

    Legen Sie Breakpoints in den beiden Ereignishandlern fest, und tippen Sie auf Elemente in der [`ListView`](xref:Xamarin.Forms.ListView). Beachten Sie, dass das [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected)-Ereignis nur ausgelöst wird, wenn ein neues Element in der [`ListView`](xref:Xamarin.Forms.ListView) ausgewählt wird, während das [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped)-Ereignis bei jedem Antippen eines Elements ausgelöst wird.

    Weitere Informationen zur Elementauswahl und Datenabzweigungen finden Sie unter [Auswahl & Datenabzweigungen](~/xamarin-forms/user-interface/listview/interactivity.md#selection-and-taps) im Artikel [ListView-Interaktivität](~/xamarin-forms/user-interface/listview/interactivity.md).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Ändern Sie in **MainPage.xaml** die [`ListView`](xref:Xamarin.Forms.ListView)-Deklaration, sodass Handler für die [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected)- und [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped)-Ereignisse festgelegt werden:

    ```xaml
    <ListView ItemsSource="{Binding Monkeys}"
              ItemSelected="OnListViewItemSelected"
              ItemTapped="OnListViewItemTapped" />
    ```

    Dieser Code legt das [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected)-Ereignis auf einen Ereignishandler mit dem Namen `OnListViewItemSelected` fest, und das [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped)-Ereignis auf einen Handler mit dem Namen `OnListViewItemTapped`. Beide Ereignishandler werden im nächsten Schritt erstellt.

1. Erweitern Sie **MainPage.xaml** im **Lösungspad** im Projekt **ListViewTutorial**, und doppelklicken Sie dann auf die Datei **MainPage.xaml.cs**, um sie zu öffnen. Fügen Sie anschließend in der Datei **MainPage.xaml.cs** der Klasse die Ereignishandler `OnListViewItemSelected` und `OnListViewItemTapped` hinzu:

    ```csharp
    void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs e)
    {
        Monkey selectedItem = e.SelectedItem as Monkey;
    }

    void OnListViewItemTapped(object sender, ItemTappedEventArgs e)
    {
        Monkey tappedItem = e.Item as Monkey;
    }
    ```

    Wenn ein Element in der [`ListView`](xref:Xamarin.Forms.ListView) angetippt wird, werden die [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected)- und [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped)-Ereignisse ausgelöst, welche die `OnListViewItemSelected`- bzw. `OnListItemTapped`-Methoden ausführen. Das `sender`-Argument an beide Methoden ist das `ListView`-Objekt, das für das Auslösen des Ereignisses verantwortlich ist und für den Zugriff auf das `ListView`-Objekt verwendet werden kann. Das [`SelectedItemChangedEventArgs`](xref:Xamarin.Forms.SelectedItemChangedEventArgs)-Argument in der `OnListViewItemSelected`-Methode stellt das ausgewählte Element bereit, und das [`ItemTappedEventArgs`](xref:Xamarin.Forms.ItemTappedEventArgs)-Argument in der `OnListViewItemTapped`-Methode stellt das angetippte Element bereit.

    > [!IMPORTANT]
    > Das [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected)-Ereignis wird nur ausgelöst, wenn ein neues Element in der [`ListView`](xref:Xamarin.Forms.ListView) ausgewählt wird. Wenn das gleiche Element zweimal angetippt wird, werden deshalb zwei [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped)-Ereignisse ausgelöst, aber nur ein `ItemSelected`-Ereignis wird ausgelöst.

1. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Simulator oder Android-Emulator zu starten.

    [![Screenshot: ListView, die auf Elementauswahl und Tippen unter iOS und Android reagiert](../images/item-selection.png "ListView-Elementauswahl")](../images/item-selection-large.png#lightbox "ListView-Elementauswahl")

    Legen Sie Breakpoints in den beiden Ereignishandlern fest, und tippen Sie auf Elemente in der [`ListView`](xref:Xamarin.Forms.ListView). Beachten Sie, dass das [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected)-Ereignis nur ausgelöst wird, wenn ein neues Element in der [`ListView`](xref:Xamarin.Forms.ListView) ausgewählt wird, während das [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped)-Ereignis bei jedem Antippen eines Elements ausgelöst wird.

    Weitere Informationen zu Elementauswahl und Tippen finden Sie unter [Auswählen und Tippen](~/xamarin-forms/user-interface/listview/interactivity.md#selection-and-taps).
