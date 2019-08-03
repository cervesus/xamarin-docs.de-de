---
title: Xamarin. Forms-Suchleiste
description: Die Suchleiste von xamarin. Forms ist ein Benutzereingabe-Steuerelement, das zum Initiieren einer Suche verwendet wird. Das Searchbar-Steuerelement unterstützt Platzhalter Text, Abfrage Eingabe, Ausführung und Abbruch. In diesem Artikel wird erläutert, wie Sie eine Suchleiste in XAML und Code verwenden.
ms.prod: xamarin
ms.assetId: F5EFEA72-CB23-4DD6-9545-D9BB755AF3CB
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/12/2019
ms.openlocfilehash: 391820cf2e94c1131f4082798ee9efa05d8489b8
ms.sourcegitcommit: c6e56545eafd8ff9e540d56aba32aa6232c5315f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2019
ms.locfileid: "68739401"
---
# <a name="xamarinforms-searchbar"></a>Xamarin. Forms-Suchleiste

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-searchbardemos/)

Xamarin. Forms [`SearchBar`](xref:Xamarin.Forms.SearchBar) ist ein Benutzereingabe-Steuerelement, das zum Initiieren einer Suche verwendet wird. Das `SearchBar` -Steuerelement unterstützt Platzhalter Text, Abfrage Eingabe, Such Ausführung und Abbruch. Der folgende Screenshot zeigt eine `SearchBar` Abfrage mit Ergebnissen, die in `ListView`einer angezeigt werden:

[ ![Screenshot von Searchbar unter IOS und Android](searchbar-images/device-searchbars-cropped.png "Searchbar unter IOS und Android") ] (searchbar-images/device-searchbars.png#lightbox "Searchbar unter IOS und Android")

`SearchBar` Definiert die folgenden Eigenschaften:

* [`CancelButtonColor`](xref:Xamarin.Forms.SearchBar.CancelButtonColor)ist eine `Color` , die die Farbe der Schaltfläche Abbrechen definiert.
* [`FontAttributes`](xref:Xamarin.Forms.SearchBar.FontAttributes)ein `FontAttributes` Enumerationswert, der bestimmt, `SearchBar` ob die Schriftart fett, kursiv oder keines von beiden ist.
* [`FontFamily`](xref:Xamarin.Forms.SearchBar.FontFamily)ist eine `string` , die die `SearchBar`von verwendete Schriftfamilie bestimmt.
* [`FontSize`](xref:Xamarin.Forms.SearchBar.FontSize)kann ein `NamedSize` Enumerationswert oder ein `double` Wert sein, der bestimmte Schriftgrößen plattformübergreifend darstellt.
* [`HorizontalTextAlignment`](xref:Xamarin.Forms.SearchBar.HorizontalTextAlignment)ein `TextAlignment` Enumerationswert, der die horizontale Ausrichtung des Abfrage Texts definiert.
* [`Placeholder`](xref:Xamarin.Forms.SearchBar.Placeholder)ist eine `string` , die den Platzhalter Text definiert, z. b. "Search...".
* [`PlaceholderColor`](xref:Xamarin.Forms.SearchBar.PlaceholderColor)ist eine `Color` , die die Farbe des Platzhalter Texts definiert.
* [`SearchCommand`](xref:Xamarin.Forms.SearchBar.SearchCommand)ist eine `ICommand` , die das Binden von Benutzeraktionen (z. b. Finger Tippen oder Klicks) auf Befehle ermöglicht, die für ein ViewModel definiert sind.
* [`SearchCommandParameter`](xref:Xamarin.Forms.SearchBar.SearchCommandParameter)ist eine `object` , die den Parameter angibt, der an den `SearchCommand`übergeben werden soll.
* [`Text`](xref:Xamarin.Forms.SearchBar.Text)ist eine `string` , die den Abfragetext in `SearchBar`der enthält.
* [`TextColor`](xref:Xamarin.Forms.SearchBar.TextColor)ist eine `Color` , die die Farbe für den Abfragetext definiert.

Diese Eigenschaften werden von [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekten unterstützt. das `SearchBar` bedeutet, dass angepasst werden kann und das Ziel von Daten Bindungen ist. Das Angeben von Schriftart Eigenschaften `SearchBar` in entspricht der Anpassung von Text in anderen [xamarin. Forms-Text Steuerelementen](~/xamarin-forms/user-interface/text/index.md). Weitere Informationen finden Sie unter [Schriftarten in xamarin. Forms](~/xamarin-forms/user-interface/text/fonts.md).

## <a name="create-a-searchbar"></a>Erstellen einer Suchleiste

Eine `SearchBar` kann in XAML instanziiert werden. Die optionale `Placeholder` -Eigenschaft kann so festgelegt werden, dass der Hinweis Text im Eingabefeld der Abfrage definiert wird. Der Standardwert für `Placeholder` ist eine leere Zeichenfolge, sodass kein Platzhalter angezeigt wird, wenn er nicht festgelegt ist. Im folgenden Beispiel wird gezeigt, wie ein `SearchBar` in XAML mit dem optionalen `Placeholder` Eigenschaften Satz instanziiert wird:

```xaml
<SearchBar Placeholder="Search items..." />
```

Ein `SearchBar` kann auch im Code erstellt werden:

```csharp
SearchBar searchBar = new SearchBar{ Placeholder = "Search items..." };
```

### <a name="searchbar-appearance-properties"></a>Eigenschaften der Searchbar-Darstellung

Das `SearchBar` -Steuerelement definiert viele Eigenschaften, die die Darstellung des Steuer Elements anpassen. Im folgenden Beispiel wird gezeigt, wie ein `SearchBar` in XAML mit mehreren angegebenen Eigenschaften instanziiert wird:

```xaml
<SearchBar Placeholder="Search items..."
           CancelButtonColor="Orange"
           PlaceholderColor="Orange"
           TextColor="Orange"
           HorizontalTextAlignment="Center"
           FontSize="Medium"
           FontAttributes="Italic" />
```

Diese Eigenschaften können auch beim Erstellen eines `SearchBar` in-Code angegeben werden:

```csharp
SearchBar searchBar = new SearchBar
{
    Placeholder = "Search items...",
    PlaceholderColor = Color.Orange,
    TextColor = Color.Orange,
    HorizontalTextAlignment = TextAlignment.Center,
    FontSize = Device.GetNamedSize(NamedSize.Medium, typeof(SearchBar)),
    FontAttributes = FontAttributes.Italic
};
```

Der folgende Screenshot zeigt das resultierende `SearchBar`:

[ ![Screenshot der angepassten Suchleiste auf IOS-und Android-](searchbar-images/device-searchbars-styled-cropped.png "Suchleiste unter IOS und Android") ] (searchbar-images/device-searchbars-styled.png#lightbox "Angepasste Suchleiste unter IOS und Android")

## <a name="perform-a-search-with-event-handlers"></a>Ausführen einer Suche mit Ereignis Handlern

Eine Suche kann mit dem `SearchBar` -Steuerelement ausgeführt werden, indem ein Ereignishandler an eines der folgenden Ereignisse angefügt wird:

* [`SearchButtonPressed`](xref:Xamarin.Forms.SearchBar.SearchButtonPressed)wird aufgerufen, wenn der Benutzer entweder auf die Such Schaltfläche klickt oder die EINGABETASTE drückt.
* [`TextChanged`](xref:Xamarin.Forms.SearchBar.TextChanged)wird aufgerufen, wenn der Text im Abfragefeld geändert wird.

Das folgende Beispiel zeigt einen Ereignishandler, der an `TextChanged` das-Ereignis in XAML angehängt `ListView` ist und einen verwendet, um Suchergebnisse anzuzeigen:

```xaml
<SearchBar TextChanged="OnTextChanged" />
<ListView x:Name="searchResults" >
```

Ein Ereignishandler kann auch an einen `SearchBar` angefügt werden, der im Code erstellt wurde:

```csharp
SearchBar searchBar = new SearchBar {/*...*/};
searchBar.TextChanged += OnTextChanged;
```

Der `TextChanged` Ereignishandler in der Code Behind-Datei ist identisch, unabhängig davon, `SearchBar` ob der über XAML oder Code erstellt wird:

```csharp
void OnTextChanged(object sender, EventArgs e)
{
    SearchBar searchBar = (SearchBar)sender;
    searchResults.ItemsSource = DataService.GetSearchResults(searchBar.Text);
}
```

Das vorherige Beispiel impliziert das vorhanden sein einer `DataService` -Klasse mit `GetSearchResults` einer-Methode, mit der Elemente zurückgegeben werden können, die einer Abfrage entsprechen. Der `SearchBar` - `GetSearchResults` `ListView` `ItemsSource` Eigenschafts Wert des Steuer Elements wird an die-Methode und das Ergebnis zum Aktualisieren der-Eigenschaft des Steuer Elements verwendet. `Text` Der Gesamteffekt besteht darin, dass die `ListView` Suchergebnisse im-Steuerelement angezeigt werden.

Die Beispielanwendung bietet eine `DataService` Klassen Implementierung, die zum Testen der Such Funktionalität verwendet werden kann.

## <a name="perform-a-search-using-a-viewmodel"></a>Ausführen einer Suche mit einem ViewModel

Eine Suche kann ohne Ereignishandler ausgeführt werden, indem die- `SearchCommand` Eigenschaft `SearchCommandParameter` und die `ICommand` -Eigenschaft an Implementierungen gebunden werden. Das Beispiel Projekt veranschaulicht diese Implementierungen mithilfe des Model-View-ViewModel (MVVM)-Musters. Weitere Informationen zu Daten Bindungen mit MVVM finden Sie unter [Daten Bindungen mit MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md).

Das ViewModel in der Beispielanwendung enthält den folgenden Code:

```csharp
public class SearchViewModel : INotifyPropertyChanged
{
    public event PropertyChangedEventHandler PropertyChanged;

    protected virtual void NotifyPropertyChanged([CallerMemberName] string propertyName = "")
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }

    public ICommand PerformSearch => new Command<string>((string query) =>
    {
        SearchResults = DataService.GetSearchResults(query);
    });

    private List<string> searchResults = DataService.Fruits;
    public List<string> SearchResults
    {
        get
        {
            return searchResults;
        }
        set
        {
            searchResults = value;
            NotifyPropertyChanged();
        }
    }
}
```

> [!NOTE]
> Das ViewModel geht davon aus, dass `DataService` eine Klasse vorhanden ist, die Suchvorgänge ausführen kann. Die `DataService` -Klasse, einschließlich Beispiel Daten, ist in der-Beispielanwendung verfügbar.

Der folgende XAML-Code zeigt, wie `SearchBar` ein an das ViewModel-Beispiel gebunden `ListView` wird, wobei ein-Steuerelement die Suchergebnisse anzeigt:

```xaml
<ContentPage ...>
    <ContentPage.BindingContext>
        <viewmodels:SearchViewModel />
    </ContentPage.BindingContext>
    <StackLayout ...>
        <SearchBar x:Name="searchBar"
                   ...
                   SearchCommand="{Binding PerformSearch}"
                   SearchCommandParameter="{Binding Text, Source={x:Reference searchBar}}"/>
        <ListView x:Name="searchResults"
                  ...
                  ItemsSource="{Binding SearchResults} />
    </StackLayout>
</ContentPage>
```

`BindingContext` In diesem Beispiel wird als eine Instanz `SearchViewModel` der-Klasse festgelegt. Sie bindet die `SearchCommand` -Eigenschaft an `PerformSearch` das `ICommand` -Objekt im ViewModel und bindet `SearchBar` die `Text` -Eigenschaft `SearchCommandParameter` an die-Eigenschaft. Die `ListView.ItemsSource` -Eigenschaft ist an die `SearchResults` -Eigenschaft von ViewModel gebunden.

Weitere Informationen `ICommand` zur-Schnittstelle und-Bindungen finden Sie unter [xamarin. Forms-Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md) und [die ICommand-Schnittstelle](~/xamarin-forms/app-fundamentals/data-binding/commanding.md).

## <a name="related-links"></a>Verwandte Links

* [Searchbar-Demos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-searchbardemos/)
* [Xamarin. Forms-Text Steuerelemente](~/xamarin-forms/user-interface/text/index.md)
* [Schriftarten in xamarin. Forms](~/xamarin-forms/user-interface/text/fonts.md)
* [Xamarin. Forms-Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md)
