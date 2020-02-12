---
title: Xamarin. Forms-Suchleiste
description: Die Suchleiste von xamarin. Forms ist ein Benutzereingabe-Steuerelement, das zum Initiieren einer Suche verwendet wird. Das Searchbar-Steuerelement unterstützt Platzhalter Text, Abfrage Eingabe, Ausführung und Abbruch. In diesem Artikel wird erläutert, wie Sie eine Suchleiste in XAML und Code verwenden.
ms.prod: xamarin
ms.assetId: F5EFEA72-CB23-4DD6-9545-D9BB755AF3CB
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 11/04/2019
ms.openlocfilehash: a48a91b886cadcbe9dfa73a524b7bfa9fb2cf5fb
ms.sourcegitcommit: ccbf914615c0ce6b3f308d930f7a77418aeb4dbc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2020
ms.locfileid: "77130975"
---
# <a name="xamarinforms-searchbar"></a>Xamarin. Forms-Suchleiste

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-searchbardemos/)

Das xamarin. Forms- [`SearchBar`](xref:Xamarin.Forms.SearchBar) ist ein Benutzereingabe-Steuerelement, das zum Initiieren einer Suche verwendet wird. Das `SearchBar`-Steuerelement unterstützt Platzhalter Text, Abfrage Eingabe, Such Ausführung und Abbruch. Der folgende Screenshot zeigt eine `SearchBar` Abfrage mit in einem `ListView`angezeigten Ergebnissen:

[![Bildschirm Abbildung von Searchbar unter IOS und Android](searchbar-images/device-searchbars-cropped.png "Searchbar unter IOS und Android")](searchbar-images/device-searchbars.png#lightbox "Searchbar unter IOS und Android")

Die `SearchBar`-Klasse definiert die folgenden Eigenschaften:

* [`CancelButtonColor`](xref:Xamarin.Forms.SearchBar.CancelButtonColor) ist ein `Color`, der die Farbe der Schaltfläche Abbrechen definiert.
* `CharacterSpacing` vom Typ `double`: Abstand zwischen den Zeichen des `SearchBar`-Texts.
* [`FontAttributes`](xref:Xamarin.Forms.SearchBar.FontAttributes) ist ein `FontAttributes` Enumerationswert, der bestimmt, ob die `SearchBar` Schriftart fett, kursiv oder keines von beiden ist.
* [`FontFamily`](xref:Xamarin.Forms.SearchBar.FontFamily) ist ein `string`, der die vom `SearchBar`verwendete Schriftfamilie bestimmt.
* [`FontSize`](xref:Xamarin.Forms.SearchBar.FontSize) kann ein `NamedSize` Enumerationswert oder ein `double` Wert sein, der bestimmte Schriftgrößen plattformübergreifend darstellt.
* [`HorizontalTextAlignment`](xref:Xamarin.Forms.SearchBar.HorizontalTextAlignment) ist ein `TextAlignment` Enumerationswert, der die horizontale Ausrichtung des Abfrage Texts definiert.
* `VerticalTextAlignment` ist ein `TextAlignment` Enumerationswert, der die vertikale Ausrichtung des Abfrage Texts definiert.
* [`Placeholder`](xref:Xamarin.Forms.InputView.Placeholder) ist ein `string`, der den Platzhalter Text definiert, z. b. "Search...".
* [`PlaceholderColor`](xref:Xamarin.Forms.InputView.PlaceholderColor) ist ein `Color`, der die Farbe des Platzhalter Texts definiert.
* [`SearchCommand`](xref:Xamarin.Forms.SearchBar.SearchCommand) ist ein `ICommand`, der die Bindung von Benutzeraktionen, wie z. b. Finger Tippen oder Klicks, auf Befehle ermöglicht, die für ein ViewModel definiert sind.
* [`SearchCommandParameter`](xref:Xamarin.Forms.SearchBar.SearchCommandParameter) ist ein `object`, der den Parameter angibt, der an die `SearchCommand`übergeben werden soll.
* [`Text`](xref:Xamarin.Forms.InputView.Text) ist ein `string`, der den Abfragetext im `SearchBar`enthält.
* [`TextColor`](xref:Xamarin.Forms.InputView.TextColor) ist ein `Color`, der die Farbe für den Abfragetext definiert.

Diese Eigenschaften werden [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekten unterstützt. das bedeutet, dass die `SearchBar` angepasst werden können und das Ziel von Daten Bindungen sind. Das Angeben von Schriftart Eigenschaften für den `SearchBar` ist mit dem Anpassen von Text in anderen [xamarin. Forms-Text Steuerelementen](~/xamarin-forms/user-interface/text/index.md)konsistent. Weitere Informationen finden Sie unter [Schriftarten in xamarin. Forms](~/xamarin-forms/user-interface/text/fonts.md).

## <a name="create-a-searchbar"></a>Erstellen einer Suchleiste

Eine `SearchBar` kann in XAML instanziiert werden. Die optionale `Placeholder`-Eigenschaft kann so festgelegt werden, dass der Hinweis Text im Eingabefeld der Abfrage definiert wird. Der Standardwert für das `Placeholder` ist eine leere Zeichenfolge, sodass kein Platzhalter angezeigt wird, wenn er nicht festgelegt ist. Im folgenden Beispiel wird gezeigt, wie ein `SearchBar` in XAML mit dem optionalen `Placeholder` Eigenschaften Satz instanziiert wird:

```xaml
<SearchBar Placeholder="Search items..." />
```

Eine `SearchBar` kann auch im Code erstellt werden:

```csharp
SearchBar searchBar = new SearchBar{ Placeholder = "Search items..." };
```

### <a name="searchbar-appearance-properties"></a>Eigenschaften der Searchbar-Darstellung

Das `SearchBar`-Steuerelement definiert viele Eigenschaften, die die Darstellung des Steuer Elements anpassen. Im folgenden Beispiel wird gezeigt, wie ein `SearchBar` in XAML instanziiert wird, wobei mehrere Eigenschaften angegeben werden:

```xaml
<SearchBar Placeholder="Search items..."
           CancelButtonColor="Orange"
           PlaceholderColor="Orange"
           TextColor="Orange"
           HorizontalTextAlignment="Center"
           FontSize="Medium"
           FontAttributes="Italic" />
```

Diese Eigenschaften können auch beim Erstellen eines `SearchBar` Objekts im Code angegeben werden:

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

Der folgende Screenshot zeigt das resultierende `SearchBar`-Steuerelement:

[![Screenshot der angepassten Suchleiste unter IOS und Android](searchbar-images/device-searchbars-styled-cropped.png "Angepasste Suchleiste unter IOS und Android")](searchbar-images/device-searchbars-styled.png#lightbox "Angepasste Suchleiste unter IOS und Android")

> [!NOTE]
> Unter IOS enthält die `SearchBarRenderer`-Klasse eine über schreibbare `UpdateCancelButton` Methode. Diese Methode steuert, wann die Schaltfläche Abbrechen angezeigt wird, und kann in einem benutzerdefinierten Renderer überschrieben werden. Weitere Informationen zu benutzerdefinierten Renderer finden Sie unter [xamarin. Forms Custom Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).

## <a name="perform-a-search-with-event-handlers"></a>Ausführen einer Suche mit Ereignis Handlern

Eine Suche kann mit dem `SearchBar`-Steuerelement ausgeführt werden, indem ein Ereignishandler an eines der folgenden Ereignisse angefügt wird:

* [`SearchButtonPressed`](xref:Xamarin.Forms.SearchBar.SearchButtonPressed) wird aufgerufen, wenn der Benutzer entweder auf die Such Schaltfläche klickt oder die EINGABETASTE drückt.
* [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) wird immer dann aufgerufen, wenn der Text im Abfragefeld geändert wird.

Das folgende Beispiel zeigt einen Ereignishandler, der an das `TextChanged`-Ereignis in XAML angehängt ist und ein `ListView` verwendet, um Suchergebnisse anzuzeigen:

```xaml
<SearchBar TextChanged="OnTextChanged" />
<ListView x:Name="searchResults" >
```

Ein Ereignishandler kann auch an eine `SearchBar` angefügt werden, die im Code erstellt wurde:

```csharp
SearchBar searchBar = new SearchBar {/*...*/};
searchBar.TextChanged += OnTextChanged;
```

Der `TextChanged`-Ereignishandler in der Code Behind-Datei ist identisch, unabhängig davon, ob die `SearchBar` über XAML oder Code erstellt wird:

```csharp
void OnTextChanged(object sender, EventArgs e)
{
    SearchBar searchBar = (SearchBar)sender;
    searchResults.ItemsSource = DataService.GetSearchResults(searchBar.Text);
}
```

Das vorherige Beispiel impliziert das vorhanden sein einer `DataService`-Klasse mit einer `GetSearchResults`-Methode, mit der Elemente zurückgegeben werden können, die einer Abfrage entsprechen. Der `Text`-Eigenschafts Wert des `SearchBar` Steuer Elements wird an die `GetSearchResults`-Methode und das Ergebnis zum Aktualisieren der `ItemsSource`-Eigenschaft des `ListView` Steuer Elements verwendet. Der Gesamteffekt besteht darin, dass die Suchergebnisse im `ListView` Steuerelement angezeigt werden.

Die Beispielanwendung stellt eine Implementierung der `DataService` Klasse bereit, die zum Testen der Such Funktionalität verwendet werden kann.

## <a name="perform-a-search-using-a-viewmodel"></a>Ausführen einer Suche mit einem ViewModel

Eine Suche kann ohne Ereignishandler ausgeführt werden, indem die Eigenschaften `SearchCommand` und `SearchCommandParameter` an `ICommand` Implementierungen gebunden werden. Das Beispiel Projekt veranschaulicht diese Implementierungen mithilfe des Model-View-ViewModel (MVVM)-Musters. Weitere Informationen zu Daten Bindungen mit MVVM finden Sie unter [Daten Bindungen mit MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md).

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
> Das ViewModel geht davon aus, dass eine `DataService` Klasse vorhanden ist, die Suchvorgänge ausführen kann. Die `DataService`-Klasse, einschließlich Beispiel Daten, ist in der Beispielanwendung verfügbar.

Der folgende XAML-Code zeigt, wie ein `SearchBar` an das ViewModel-Beispiel gebunden wird, wobei ein `ListView`-Steuerelement die Suchergebnisse anzeigt:

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

In diesem Beispiel wird die `BindingContext` als Instanz der `SearchViewModel`-Klasse festgelegt. Sie bindet die `SearchCommand`-Eigenschaft an die `PerformSearch`-`ICommand` im ViewModel und bindet die `SearchBar` `Text`-Eigenschaft an die `SearchCommandParameter`-Eigenschaft. Die `ListView.ItemsSource`-Eigenschaft ist an die `SearchResults`-Eigenschaft von "ViewModel" gebunden.

Weitere Informationen zu den `ICommand`-Schnittstellen und-Bindungen finden Sie unter [xamarin. Forms-Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md) und [die ICommand-Schnittstelle](~/xamarin-forms/app-fundamentals/data-binding/commanding.md).

## <a name="related-links"></a>Verwandte Links

* [Searchbar-Demos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-searchbardemos/)
* [Xamarin. Forms-Text Steuerelemente](~/xamarin-forms/user-interface/text/index.md)
* [Schriftarten in xamarin. Forms](~/xamarin-forms/user-interface/text/fonts.md)
* [Xamarin. Forms-Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md)
