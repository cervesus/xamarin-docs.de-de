---
title: Suche in der Xamarin.Forms-Shell
description: Xamarin.Forms-Shell-Anwendungen können integrierte Suchfunktionalität über ein Suchfeld nutzen, das oben auf jeder Seite hinzugefügt werden kann.
ms.prod: xamarin
ms.assetid: F8F9471D-6771-4D23-96C0-2B79473A06D4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/25/2019
ms.openlocfilehash: 400459d2701731726c91c70e020ef375a7031169
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72695937"
---
# <a name="xamarinforms-shell-search"></a>Suche in der Xamarin.Forms-Shell

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

Die integrierte Suchfunktionalität in der Xamarin.Forms-Shell wird von der `SearchHandler`-Klasse bereitgestellt. Sie können einer Seite die Suchfunktion hinzufügen, indem Sie die angefügte `Shell.SearchHandler`-Eigenschaft auf ein untergeordnetes `SearchHandler`-Objekt festlegen. Dadurch wird am oberen Rand der Seite ein Suchfeld hinzugefügt:

[![Screenshot eines Shell-SearchHandler-Objekts unter iOS und Android](search-images/searchhandler.png "Shell-SearchHandler-Objekt")](search-images/searchhandler-large.png#lightbox "Shell-SearchHandler-Objekt")

Wird eine Abfrage in das Suchfeld eingegeben, wird die `Query`-Eigenschaft aktualisiert, und bei jeder Aktualisierung wird die `OnQueryChanged`-Methode ausgeführt. Diese Methode kann überschrieben werden, um den Suchvorschlagsbereich mit Daten zu füllen:

[![Screenshot von Suchergebnissen in einem Shell-SearchHandler-Objekt unter iOS und Android](search-images/search-suggestions.png "Suchergebnisse in einem Shell-SearchHandler-Objekt")](search-images/search-suggestions-large.png#lightbox "Suchergebnisse in einem Shell-SearchHandler-Objekt")

Wenn dann ein Ergebnis aus dem Suchvorschlagsbereich ausgewählt wird, wird die `OnItemSelected`-Methode ausgeführt. Diese Methode kann überschrieben werden, um entsprechend zu reagieren, z.B. zu einer Detailseite zu navigieren.

## <a name="create-a-searchhandler"></a>Erstellen eines SearchHandler-Objekts

Die Suchfunktionalität kann einer Shell-Anwendung hinzugefügt werden, indem die `SearchHandler`-Klasse als Unterklasse erstellt wird und die Methoden `OnQueryChanged` und `OnItemSelected` überschrieben werden:

```csharp
public class MonkeySearchHandler : SearchHandler
{
    protected override void OnQueryChanged(string oldValue, string newValue)
    {
        base.OnQueryChanged(oldValue, newValue);

        if (string.IsNullOrWhiteSpace(newValue))
        {
            ItemsSource = null;
        }
        else
        {
            ItemsSource = MonkeyData.Monkeys
                .Where(monkey => monkey.Name.ToLower().Contains(newValue.ToLower()))
                .ToList<Animal>();
        }
    }

    protected override async void OnItemSelected(object item)
    {
        base.OnItemSelected(item);

        // Note: strings will be URL encoded for navigation (e.g. "Blue Monkey" becomes "Blue%20Monkey"). Therefore, decode at the receiver.
        await (App.Current.MainPage as Xamarin.Forms.Shell).GoToAsync($"monkeydetails?name={((Animal)item).Name}");
    }
}
```

Die `OnQueryChanged`-Überschreibung verfügt über zwei Argumente: `oldValue` (für die vorherige Suchanfrage) und `newValue` (für die aktuelle Suchanfrage). Der Bereich der Suchvorschläge kann aktualisiert werden, indem die Eigenschaft `SearchHandler.ItemsSource` auf eine `IEnumerable`-Sammlung festgelegt wird, die Elemente enthält, die mit der aktuellen Suchanfrage übereinstimmen.

Wenn der Benutzer ein Suchergebnis auswählt, wird die `OnItemSelected`-Überschreibung ausgeführt und die Eigenschaft `SelectedItem` festgelegt. In diesem Beispiel navigiert die Methode zu einer anderen Seite, die Daten über das ausgewählte `Animal`-Objekt anzeigt. Weitere Informationen zur Navigation finden Sie unter [Navigation in der Xamarin.Forms Shell](navigation.md).

> [!NOTE]
> Zusätzliche `SearchHandler`-Eigenschaften können festgelegt werden, um die Darstellung des Suchfelds zu steuern.

## <a name="consume-a-searchhandler"></a>Verarbeiten eines SearchHandler-Objekts

Das `SearchHandler`-Objekt mit Unterklassen kann durch Festlegen der angefügten `Shell.SearchHandler`-Eigenschaft auf ein Objekt des Unterklassen-Typs verarbeitet werden:

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:Xaminals.Controls">
    <Shell.SearchHandler>
        <controls:MonkeySearchHandler Placeholder="Enter search term"
                                      ShowsResults="true"
                                      DisplayMemberName="Name" />
    </Shell.SearchHandler>
    ...
</ContentPage>
```

Der entsprechende C#-Code lautet:

```csharp
Shell.SetSearchHandler(this, new MonkeySearchHandler
{
    Placeholder = "Enter search term",
    ShowsResults = true,
    DisplayMemberName = "Name"
});
```

Die `MonkeySearchHandler.OnQueryChanged`-Methode gibt eine `List` von `Animal`-Objekten zurück. Die `DisplayMemberName`-Eigenschaft wird auf die `Name`-Eigenschaft jedes `Animal`-Objekts festgelegt, und so werden im Vorschlagsbereich die einzelnen Tiernamen angezeigt.

Die `ShowsResults`-Eigenschaft wird auf `true` festgelegt, sodass Suchvorschläge angezeigt werden, wenn der Benutzer eine Suchanfrage eingibt:

[![Screenshot von Suchergebnissen in einem Shell-SearchHandler-Objekt unter iOS und Android](search-images/search-results.png "Suchergebnisse in einem Shell-SearchHandler-Objekt")](search-images/search-results-large.png#lightbox "Suchergebnisse in einem Shell-SearchHandler-Objekt")

Mit jeder Änderung der Abfrage wird der Bereich der Suchvorschläge aktualisiert:

[![Screenshot von Suchergebnissen in einem Shell-SearchHandler-Objekt unter iOS und Android](search-images/search-results-change.png "Suchergebnisse in einem Shell-SearchHandler-Objekt")](search-images/search-results-change-large.png#lightbox "Suchergebnisse in einem Shell-SearchHandler-Objekt")

Wird ein Suchergebnis ausgewählt, wird zum `MonkeyDetailPage`-Objekt navigiert, und es werden Daten über den ausgewählten Affen angezeigt:

[![Screenshot von Details zum Affen unter iOS und Android](search-images/detailpage.png "Details zum Affen")](search-images/detailpage-large.png#lightbox "Details zum Affen")

## <a name="define-search-results-item-appearance"></a>Definieren der Darstellung der Suchergebnisse

Zusätzlich zur Anzeige von `string`-Daten in den Suchergebnissen kann die Darstellung jedes Suchergebniselements durch Festlegen der `SearchHandler.ItemTemplate`-Eigenschaft auf eine [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) definiert werden:

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:Xaminals.Controls">    
    <Shell.SearchHandler>
        <controls:MonkeySearchHandler Placeholder="Enter search term"
                                      ShowsResults="true">
            <controls:MonkeySearchHandler.ItemTemplate>
                <DataTemplate>
                    <Grid Padding="10">
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="0.15*" />
                            <ColumnDefinition Width="0.85*" />
                        </Grid.ColumnDefinitions>
                        <Image Source="{Binding ImageUrl}"
                               Aspect="AspectFill"
                               HeightRequest="40"
                               WidthRequest="40" />
                        <Label Grid.Column="1"
                               Text="{Binding Name}"
                               FontAttributes="Bold" />
                    </Grid>
                </DataTemplate>
            </controls:MonkeySearchHandler.ItemTemplate>
       </controls:MonkeySearchHandler>
    </Shell.SearchHandler>
    ...
</ContentPage>
```

Der entsprechende C#-Code lautet:

```csharp
Shell.SetSearchHandler(this, new MonkeySearchHandler
{
    Placeholder = "Enter search term",
    ShowsResults = true,
    DisplayMemberName = "Name",
    ItemTemplate = new DataTemplate(() =>
    {
        Grid grid = new Grid { Padding = 10 };
        grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(0.15, GridUnitType.Star) });
        grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(0.85, GridUnitType.Star) });

        Image image = new Image { Aspect = Aspect.AspectFill, HeightRequest = 40, WidthRequest = 40 };
        image.SetBinding(Image.SourceProperty, "ImageUrl");
        Label nameLabel = new Label { FontAttributes = FontAttributes.Bold };
        nameLabel.SetBinding(Label.TextProperty, "Name");

        grid.Children.Add(image);
        grid.Children.Add(nameLabel, 1, 0);
        return grid;
    })
});
```

Die im [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)-Objekt angegebenen Elemente definieren die Darstellung jedes Elements im Vorschlagsbereich. In diesem Beispiel wird das Layout innerhalb des `DataTemplate`-Objekts von einem [`Grid`](xref:Xamarin.Forms.Grid)-Objekt verwaltet. Das `Grid`-Objekt enthält ein [`Image`](xref:Xamarin.Forms.Image)-Objekt und ein [`Label`](xref:Xamarin.Forms.Label)-Objekt, die beide an die Eigenschaften der einzelnen `Monkey`-Objekte gebunden sind.

Die folgenden Screenshots zeigen das Ergebnis der Vorlagenerstellung für jedes Element im Vorschlagsbereich:

[![Screenshot von Suchergebnissen mit Vorlagen in einem Shell-SearchHandler-Objekt unter iOS und Android](search-images/search-results-template.png "Suchergebnisse mit Vorlagen in einem Shell-SearchHandler-Objekt")](search-images/search-results-template-large.png#lightbox "Suchergebnisse mit Vorlagen in einem Shell-SearchHandler-Objekt")

Weitere Informationen zu Datenvorlagen finden Sie unter [Xamarin.Forms-Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="search-box-visibility"></a>Suchfeldsichtbarkeit

Wird am oberen Rand einer Seite ein `SearchHandler` hinzugefügt, ist dieses standardmäßig sichtbar und vollständig erweitert. Allerdings kann dieses Verhalten geändert werden, indem die `SearchHandler.SearchBoxVisibility`-Eigenschaft auf eines der `SearchBoxVisibility`-Enumerationsmember festgelegt wird:

- `Hidden` – Das Suchfeld ist nicht sichtbar, oder es kann nicht darauf zugegriffen werden.
- `Collapsible` – Das Suchfeld ist ausgeblendet, bis der Benutzer eine Aktion ausführt, um es einzublenden.
- `Expanded` – Das Suchfeld sichtbar und vollständig erweitert.

Das folgende Beispiel zeigt, wie Sie das Suchfeld ausblenden:

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:Xaminals.Controls">
    <Shell.SearchHandler>
        <controls:MonkeySearchHandler SearchBoxVisibility="Hidden"
                                      ... />
    </Shell.SearchHandler>
    ...
</ContentPage>
```

## <a name="search-box-focus"></a>Suchfeldfokus

Durch Tippen in ein Suchfeld wird die Bildschirmtastatur aufgerufen, und das Suchfeld erhält den Eingabefokus. Dies kann auch programmgesteuert erreicht werden, indem die Methode `Focus` aufgerufen wird, die versucht, den Eingabefokus auf das Suchfeld zu setzen, und bei Erfolg `true` zurückgibt. Wenn ein Suchfeld den Fokus erhält, wird das Ereignis `Focus` ausgelöst und die überschreibbare Methode `OnFocused` aufgerufen.

Besitzt ein Suchfeld den Eingabefokus und wird an eine andere Stelle auf dem Bildschirm getippt, wird die Bildschirmtastatur ausgeblendet, und das Suchfeld verliert den Eingabefokus. Dies kann auch programmgesteuert durch Aufrufen der Methode `Unfocus` erreicht werden. Wenn ein Suchfeld den Fokus verliert, wird das Ereignis `Unfocused` ausgelöst und die überschreibbare Methode `OnUnfocus` aufgerufen.

Der Fokusstatus eines Suchfelds kann über die Eigenschaft `IsFocused` abgerufen werden, die `true` zurückgibt, wenn ein `SearchHandler` aktuell den Eingabefokus besitzt.

## <a name="searchhandler-appearance"></a>SearchHandler-Darstellung

Die `SearchHandler`-Klasse definiert die folgenden Eigenschaften, die ihre Darstellung steuern:

- `BackgroundColor` vom Typ `Color`: Farbe des Hintergrunds des Texts im Suchfeld.
- `CancelButtonColor` vom Typ `Color`: Farbe der Schaltfläche „Abbrechen“.
- `CharacterSpacing` vom Typ `double`: Abstand zwischen den Zeichen des `SearchHandler`-Texts.
- `FontAttributes` vom Typ `FontAttributes`: Gibt an, ob der Text im Suchfeld kursiv oder fett formatiert ist.
- `FontFamily` vom Typ `string`: Schriftartfamilie für den Text im Suchfeld.
- `FontSize` vom Typ `double`: Schriftgrad für den Text im Suchfeld.
- `HorizontalTextAlignment` vom Typ `TextAlignment`: horizontale Ausrichtung des Texts im Suchfeld.
- `PlaceholderColor` vom Typ `Color`: Farbe des Platzhaltertexts im Suchfeld.
- `TextColor` vom Typ `Color`: Farbe des Texts im Suchfeld.
- `VerticalTextAlignment` vom Typ `TextAlignment`: Vertikale Ausrichtung des Texts im Suchfeld.

## <a name="searchhandler-keyboard"></a>SearchHandler-Tastatur

Die Tastatur, die angezeigt wird, wenn Benutzer mit einem `SearchHandler` interagieren, kann programmgesteuert über die `Keyboard`-Eigenschaft auf eine der folgenden Eigenschaften der [`Keyboard`](xref:Xamarin.Forms.Keyboard)-Klasse festgelegt werden:

- [`Chat`](xref:Xamarin.Forms.Keyboard.Chat): Wird zum Schreiben von Texten verwendet und in Situationen, in denen Emojis nützlich sind.
- [`Default`](xref:Xamarin.Forms.Keyboard.Default): die Standardtastatur.
- [`Email`](xref:Xamarin.Forms.Keyboard.Email): Wird beim Eingeben von E-Mail-Adressen verwendet.
- [`Numeric`](xref:Xamarin.Forms.Keyboard.Numeric): Wird beim Eingeben von Zahlen verwendet.
- [`Plain`](xref:Xamarin.Forms.Keyboard.Plain): Wird beim Eingeben von Text verwendet, ohne Angabe von [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags).
- [`Telephone`](xref:Xamarin.Forms.Keyboard.Telephone): Wird beim Eingeben von Telefonnummern verwendet.
- [`Text`](xref:Xamarin.Forms.Keyboard.Text): Wird beim Eingeben von Text verwendet.
- [`Url`](xref:Xamarin.Forms.Keyboard.Url): Wird beim Eingeben von Dateipfaden und Webadressen verwendet.

Dies kann in XAML folgendermaßen erfüllt werden:

```xaml
<SearchHandler Keyboard="Email" />
```

Der entsprechende C#-Code lautet:

```csharp
SearchHandler searchHandler = new SearchHandler { Keyboard = Keyboard.Email };
```

Die [`Keyboard`](xref:Xamarin.Forms.Keyboard)-Klasse verfügt auch über eine [`Create`](xref:Xamarin.Forms.Keyboard.Create*)-Factorymethode, die zum Anpassen einer Tastatur durch Festlegen des Verhaltens für Groß-/Kleinschreibung, Rechtschreibprüfung und Vorschläge verwendet werden kann. [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags)-Enumerationswerte werden als Argumente der Methode festgelegt, wobei das benutzerdefinierte `Keyboard` zurückgegeben wird. Die `KeyboardFlags`-Enumeration verfügt über folgende Werte:

- [`None`](xref:Xamarin.Forms.KeyboardFlags.None): Der Tastatur werden keine Features hinzugefügt.
- [`CapitalizeSentence`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeSentence): Gibt an, dass der erste Buchstabe des ersten Worts jedes Satzes automatisch groß geschrieben wird.
- [`Spellcheck`](xref:Xamarin.Forms.KeyboardFlags.Spellcheck): Gibt an, dass die Rechtschreibprüfung für den eingegebenen Text durchgeführt wird.
- [`Suggestions`](xref:Xamarin.Forms.KeyboardFlags.Suggestions): Gibt an, dass Wortvervollständigungen für den eingegebenen Text angeboten werden.
- [`CapitalizeWord`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeWord): Gibt an, dass der erste Buchstabe von jedem Wort automatisch groß geschrieben wird.
- [`CapitalizeCharacter`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeCharacter): Gibt an, dass jedes Zeichen automatisch groß geschrieben wird.
- [`CapitalizeNone`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeNone): Gibt an, dass keine automatische Großschreibung erfolgt.
- [`All`](xref:Xamarin.Forms.KeyboardFlags.All): Gibt an, dass für den eingegebenen Text die Rechtschreibprüfung, die Vervollständigung von Wörtern und die Großschreibung am Satzanfang erfolgen.

Das folgende XAML-Codebeispiel zeigt, wie Sie den Standardwert für [`Keyboard`](xref:Xamarin.Forms.Keyboard) anpassen können, um Wortvervollständigungen anzubieten und jedes eingegebene Zeichen groß zu schreiben:

```xaml
<SearchHandler Placeholder="Enter search terms">
    <SearchHandler.Keyboard>
        <Keyboard x:FactoryMethod="Create">
            <x:Arguments>
                <KeyboardFlags>Suggestions,CapitalizeCharacter</KeyboardFlags>
            </x:Arguments>
        </Keyboard>
    </SearchHandler.Keyboard>
</SearchHandler>
```

Der entsprechende C#-Code lautet:

```csharp
SearchHandler searchHandler = new SearchHandler { Placeholder = "Enter search terms" };
searchHandler.Keyboard = Keyboard.Create(KeyboardFlags.Suggestions | KeyboardFlags.CapitalizeCharacter);
```

## <a name="searchhandler-reference"></a>SearchHandler-Referenz

Die `SearchHandler`-Klasse definiert die folgenden Eigenschaften, die ihre Darstellung und ihr steuern:

- `BackgroundColor` vom Typ `Color`: Farbe des Hintergrunds des Texts im Suchfeld.
- `CancelButtonColor` vom Typ `Color`: Farbe der Schaltfläche „Abbrechen“.
- `ClearIcon` vom Typ [`ImageSource`](xref:Xamarin.Forms.ImageSource): Symbol, das angezeigt wird, um den Inhalt des Suchfelds zu löschen.
- `ClearIconHelpText` vom Typ `string`: der barrierefreie Hilfetext für das Löschen-Symbol.
- `ClearIconName` vom Typ `string`: der Name des Löschen-Symbols zur Verwendung mit Bildschirmsprachausgaben.
- `ClearPlaceholderCommand` vom Typ `ICommand`: Wird ausgeführt, wenn `ClearPlaceholderIcon` angetippt wird.
- `ClearPlaceholderCommandParameter` vom Typ `object`: der Parameter, der an `ClearPlaceholderCommand` übergeben wird.
- `ClearPlaceholderEnabled`vom Typ `bool`: Bestimmt, ob `ClearPlaceholderCommand` ausgeführt werden kann. Der Standardwert ist `true`sein.
- `ClearPlaceholderHelpText` vom Typ `string`: der barrierefreie Hilfetext für das Löschen-Platzhaltersymbol.
- `ClearPlaceholderIcon` vom Typ [`ImageSource`](xref:Xamarin.Forms.ImageSource): das Löschen-Platzhaltersymbol, das angezeigt wird, wenn das Suchfeld leer ist.
- `ClearPlaceholderName` vom Typ `string`: der Name des Löschen-Platzhaltersymbols für die Verwendung mit Bildschirmsprachausgaben.
- `Command` vom Typ `ICommand`: Wird ausgeführt, wenn die Suchanfrage bestätigt wird.
- `CommandParameter` vom Typ `object`: der Parameter, der an `Command` übergeben wird.
- `DisplayMemberName` vom Typ `string`: Stellt den Namen oder Pfad der Eigenschaft dar, die für jedes Datenelement in der `ItemsSource`-Sammlung angezeigt wird.
- `FontAttributes` vom Typ `FontAttributes`: Gibt an, ob der Text im Suchfeld kursiv oder fett formatiert ist.
- `FontFamily` vom Typ `string`: Schriftartfamilie für den Text im Suchfeld.
- `FontSize` vom Typ `double`: Schriftgrad für den Text im Suchfeld.
- `HorizontalTextAlignment` vom Typ `TextAlignment`: horizontale Ausrichtung des Texts im Suchfeld.
- `IsFocused` vom Typ `bool`: Gibt an, ob ein `SearchHandler` derzeit über den Eingabefokus verfügt.
- `IsSearchEnabled` vom Typ `bool`: Stellt den aktivierten Zustand des Suchfelds dar. Der Standardwert ist `true`sein.
- `ItemsSource` vom Typ `IEnumerable`: Gibt die Sammlung von Elementen an, die im Vorschlagsbereich angezeigt werden sollen; Standardwert ist `null`.
- `ItemTemplate` vom Typ [`DataTemplate`](xref:Xamarin.Forms.DataTemplate): Gibt die Vorlage an, die für jedes Element in der Sammlung von Elementen gilt, die im Vorschlagsbereich angezeigt werden soll.
- `Keyboard` vom Typ `Keyboard`: Gibt die Tastatur für den `SearchHandler` an.
- `Placeholder` vom Typ `string`: der Text, der angezeigt werden soll, wenn das Suchfeld leer ist.
- `PlaceholderColor` vom Typ `Color`: Farbe des Platzhaltertexts im Suchfeld.
- `Query` vom Typ `string`: der vom Benutzer in das Suchfeld eingegebene Text.
- `QueryIcon` vom Typ [`ImageSource`](xref:Xamarin.Forms.ImageSource): das Symbol, das verwendet wird, um dem Benutzer anzuzeigen, dass die Suche verfügbar ist.
- `QueryIconHelpText` vom Typ `string`: der barrierefreie Hilfetext für das Abfragesymbol.
- `QueryIconName` vom Typ `string`: der Name des Abfragesymbols zur Verwendung mit Bildschirmsprachausgaben.
- `SearchBoxVisibility` vom Typ `SearchBoxVisibility`: die Sichtbarkeit des Suchfelds. Standardmäßig ist das Suchfeld sichtbar und vollständig erweitert.
- `SelectedItem` vom Typ `object`: das in den Suchergebnissen ausgewählte Element. Diese Eigenschaft ist schreibgeschützt; Standardwert ist `null`.
- `ShowsResults` vom Typ `bool`:Gibt an, ob bei der Texteingabe Suchergebnisse im Vorschlagsbereich zu erwarten sind. Der Standardwert ist `false`sein.
- `TextColor` vom Typ `Color`: Farbe des Texts im Suchfeld.

Alle diese Eigenschaften werden durch [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)-Objekte gestützt, was bedeutet, dass die Eigenschaften Ziele von Datenverbindungen sein können.

Darüber hinaus stellt die `SearchHandler`-Klasse die folgenden überschreibbaren Methoden zur Verfügung:

- `OnClearPlaceholderClicked`: Wird aufgerufen, wenn auf `ClearPlaceholderIcon` getippt wird.
- `OnItemSelected`: Wird aufgerufen, wenn ein Suchergebnis vom Benutzer ausgewählt wird.
- `OnFocused`: Wird aufgerufen, wenn ein `SearchHandler` den Eingabefokus erhält.
- `OnQueryChanged`: Wird aufgerufen, wenn sich die `Query`-Eigenschaft ändert.
- `OnQueryConfirmed`: Wird aufgerufen, wenn der Benutzer die EINGABETASTE drückt oder seine Anfrage im Suchfeld bestätigt.
- `OnUnfocus`: Wird aufgerufen, wenn ein `SearchHandler` den Eingabefokus verliert.

## <a name="related-links"></a>Verwandte Links

- [Xaminals (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
- [Navigation in der Xamarin.Forms-Shell](navigation.md)
