---
title: Anpassen der Zellendarstellung ListView
description: In diesem Artikel untersucht die Optionen zum Darstellen von Daten in Xamarin.Forms-Anwendungen, und nutzt die Vorteile des ListView-Steuerelements.
ms.prod: xamarin
ms.assetid: FD45CB91-1A8F-46FB-B432-6BC20492E456
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/12/2019
ms.openlocfilehash: ab54b54c9f2f7d6d7748137ea079439b7c3ddfca
ms.sourcegitcommit: a5ef4497db04dfa016865bc7454b3de6ff088554
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/15/2019
ms.locfileid: "70998116"
---
# <a name="customizing-listview-cell-appearance"></a>Anpassen der Zellendarstellung ListView

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-customcells)

Die xamarin. Forms [`ListView`](xref:Xamarin.Forms.ListView) -Klasse wird verwendet, um scrollbare Listen darzustellen, die durch die Verwendung von `ViewCell` -Elementen angepasst werden können. Ein `ViewCell` -Element kann Text und Bilder anzeigen, den Zustand true/false angeben und Benutzereingaben empfangen.

## <a name="built-in-cells"></a>Erstellt in Zellen
Xamarin. Forms verfügt über integrierte Zellen, die für viele Anwendungen funktionieren:

- [`TextCell`](#textcell)Steuerelemente werden zum Anzeigen von Text mit einer optionalen zweiten Zeile für Detail Text verwendet.
- [`ImageCell`](#imagecell)Steuerelemente ähneln s `TextCell`, enthalten aber ein Bild links neben dem Text.
- `SwitchCell`Steuerelemente werden verwendet, um den Status "ein/aus" oder "true/false" darzustellen und aufzuzeichnen.
- `EntryCell`Steuerelemente werden verwendet, um Textdaten darzustellen, die der Benutzer bearbeiten kann.

Die [`SwitchCell`](~/xamarin-forms/user-interface/tableview.md#switchcell) - [`EntryCell`](~/xamarin-forms/user-interface/tableview.md#entrycell) und-Steuerelemente werden üblicherweise im Kontext [`TableView`](~/xamarin-forms/user-interface/tableview.md)einer verwendet.

### <a name="textcell"></a>TextCell

[`TextCell`](xref:Xamarin.Forms.TextCell) ist eine Zelle für die Anzeige von Text, optional mit einer zweiten Zeile als Details Text. Der folgende Screenshot zeigt `TextCell` Elemente unter IOS und Android:

![](customizing-cell-appearance-images/text-cell-default.png "Standard-TextCell-Beispiel")

TextCells als systemeigene Steuerelemente zur Laufzeit gerendert werden, damit die Leistung sehr gut ist im Vergleich zu einer benutzerdefinierten `ViewCell`. Textcells können angepasst werden, sodass Sie die folgenden Eigenschaften festlegen können:

- `Text` &ndash; der Text, der auf der ersten Zeile, in großer Schriftart angezeigt wird.
- `Detail` &ndash; der Text, der unterhalb der ersten Zeile in einer kleineren Schriftart angezeigt wird.
- `TextColor` &ndash; die Farbe des Texts.
- `DetailColor` &ndash; die Farbe des Texts detail

Der folgende Screenshot zeigt `TextCell` Elemente mit angepassten Farbeigenschaften:

![](customizing-cell-appearance-images/text-cell-custom.png "Beispiel für benutzerdefinierte textcell")

### <a name="imagecell"></a>ImageCell

[`ImageCell`](xref:Xamarin.Forms.ImageCell), z. B. `TextCell`, für die Anzeige von Text und sekundären Detail Text verwendet werden kann und er bietet hohe Leistung aus, indem Sie mit nativen Steuerelementen der Plattformen. `ImageCell` unterscheidet sich von `TextCell` , da es ein Bild auf der linken Seite des Texts anzeigt.

Der folgende Screenshot zeigt `ImageCell` Elemente unter IOS und Android: !["Default ImageCell example"](customizing-cell-appearance-images/image-cell-default.png "ImageCell-Standardbeispiel")

`ImageCell` ist nützlich, wenn Sie eine Liste der Daten mit der ein visueller Aspekt, z. B. eine Liste der Kontakte oder Filme anzeigen müssen. `ImageCell`s sind anpassbar, sodass Sie Folgendes festlegen können:

- `Text` &ndash; der Text, der auf der ersten Zeile, in großer Schriftart angezeigt wird
- `Detail` &ndash; der Text, der unterhalb der ersten Zeile in einer kleineren Schriftart angezeigt wird
- `TextColor` &ndash; die Farbe des Texts
- `DetailColor` &ndash; die Farbe des Texts detail
- `ImageSource` &ndash; das Bild an neben dem Text angezeigt.

Der folgende Screenshot zeigt `ImageCell` Elemente mit angepassten Farbeigenschaften: !["Angepasstes ImageCell-Beispiel"](customizing-cell-appearance-images/image-cell-custom.png "Beispiel für eine angepasste ImageCell")

## <a name="custom-cells"></a>Benutzerdefinierte Zellen
Mithilfe von benutzerdefinierten Zellen können Sie Zellen Layouts erstellen, die von den integrierten Zellen nicht unterstützt werden. Beispielsweise empfiehlt es sich um eine Zelle mit zwei Bezeichnungen zu präsentieren, die gleich Gewichtung. Ein `TextCell` wäre nicht genügend, da die `TextCell` enthält eine Bezeichnung, die kleiner ist. Die meisten Anpassungen der Zelle hinzufügen zusätzliche schreibgeschützte Daten (z. B. zusätzliche Bezeichnungen, Bilder oder andere Anzeigeinformationen).

Alle benutzerdefinierte Zellen abgeleitet müssen [ `ViewCell` ](xref:Xamarin.Forms.ViewCell), der gleichen Basisklasse an, dass alle der Zelle integrierte Typen verwenden.

Xamarin. Forms bietet ein [Cachingverhalten](~/xamarin-forms/user-interface/listview/performance.md#caching-strategy) für `ListView` das Steuerelement, das die Bild Laufleistung für einige Arten von benutzerdefinierten Zellen verbessern kann.

Der folgende Screenshot zeigt ein Beispiel für eine benutzerdefinierte Zelle:

![Beispiel für eine benutzerdefinierte Zelle](customizing-cell-appearance-images/custom-cell.png "Beispiel für benutzerdefinierte Zelle")

### <a name="xaml"></a>XAML
Die im vorherigen Screenshot gezeigte benutzerdefinierte Zelle kann mit dem folgenden XAML-Code erstellt werden:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="demoListView.ImageCellPage">
    <ContentPage.Content>
        <ListView  x:Name="listView">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <StackLayout BackgroundColor="#eee"
                        Orientation="Vertical">
                            <StackLayout Orientation="Horizontal">
                                <Image Source="{Binding image}" />
                                <Label Text="{Binding title}"
                                TextColor="#f35e20" />
                                <Label Text="{Binding subtitle}"
                                HorizontalOptions="EndAndExpand"
                                TextColor="#503026" />
                            </StackLayout>
                        </StackLayout>
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </ContentPage.Content>
</ContentPage>
```

Die XAML funktioniert wie folgt:

- Die benutzerdefinierte Zelle darin geschachtelt ist eine `DataTemplate`, d.h. in `ListView.ItemTemplate`. Dies ist der gleiche Prozess wie die Verwendung einer beliebigen integrierten Zelle.
- `ViewCell` ist der Typ der benutzerdefinierten Zelle. Das untergeordnete `DataTemplate` Element des-Elements muss von der `ViewCell` -Klasse oder davon abgeleitet sein.
- Innerhalb von `ViewCell`kann das Layout von einem beliebigen xamarin. Forms-Layout verwaltet werden. In diesem Beispiel wird das Layout von einem `StackLayout`verwaltet, wodurch die Hintergrundfarbe angepasst werden kann.

> [!NOTE]
> Jede Eigenschaft von `StackLayout` , die bindbar ist, kann innerhalb einer benutzerdefinierten Zelle gebunden werden. Diese Funktion wird im XAML-Beispiel jedoch nicht angezeigt.

### <a name="code"></a>Code

Eine benutzerdefinierte Zelle kann auch im Code erstellt werden. Zuerst muss eine benutzerdefinierte Klasse, die `ViewCell` von abgeleitet wird, erstellt werden:

```csharp
public class CustomCell : ViewCell
    {
        public CustomCell()
        {
            //instantiate each of our views
            var image = new Image ();
            StackLayout cellWrapper = new StackLayout ();
            StackLayout horizontalLayout = new StackLayout ();
            Label left = new Label ();
            Label right = new Label ();

            //set bindings
            left.SetBinding (Label.TextProperty, "title");
            right.SetBinding (Label.TextProperty, "subtitle");
            image.SetBinding (Image.SourceProperty, "image");

            //Set properties for desired design
            cellWrapper.BackgroundColor = Color.FromHex ("#eee");
            horizontalLayout.Orientation = StackOrientation.Horizontal;
            right.HorizontalOptions = LayoutOptions.EndAndExpand;
            left.TextColor = Color.FromHex ("#f35e20");
            right.TextColor = Color.FromHex ("503026");

            //add views to the view hierarchy
            horizontalLayout.Children.Add (image);
            horizontalLayout.Children.Add (left);
            horizontalLayout.Children.Add (right);
            cellWrapper.Children.Add (horizontalLayout);
            View = cellWrapper;
        }
    }
```

Im Seitenkonstruktor wird die-Eigenschaft von `ItemTemplate` ListView auf eine `DataTemplate` mit dem `CustomCell` angegebenen Typ festgelegt:

```csharp
public partial class ImageCellPage : ContentPage
    {
        public ImageCellPage ()
        {
            InitializeComponent ();
            listView.ItemTemplate = new DataTemplate (typeof(CustomCell));
        }
    }
```

### <a name="binding-context-changes"></a>Binden von Kontextänderungen

Beim Binden an einen benutzerdefinierten Zellentyps [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) Instanzen, die Anzeige von UI-Steuerelemente der `BindableProperty` Werte verwenden, sollten die [ `OnBindingContextChanged` ](xref:Xamarin.Forms.Cell.OnBindingContextChanged) überschreiben, um die Daten angezeigt werden jede Zelle, anstatt die Cell-Konstruktor ein, wie im folgenden Codebeispiel gezeigt:

```csharp
public class CustomCell : ViewCell
{
    Label nameLabel, ageLabel, locationLabel;

    public static readonly BindableProperty NameProperty =
        BindableProperty.Create ("Name", typeof(string), typeof(CustomCell), "Name");
    public static readonly BindableProperty AgeProperty =
        BindableProperty.Create ("Age", typeof(int), typeof(CustomCell), 0);
    public static readonly BindableProperty LocationProperty =
        BindableProperty.Create ("Location", typeof(string), typeof(CustomCell), "Location");

    public string Name
    {
        get { return(string)GetValue (NameProperty); }
        set { SetValue (NameProperty, value); }
    }

    public int Age
    {
        get { return(int)GetValue (AgeProperty); }
        set { SetValue (AgeProperty, value); }
    }

    public string Location
    {
        get { return(string)GetValue (LocationProperty); }
        set { SetValue (LocationProperty, value); }
    }
    ...

    protected override void OnBindingContextChanged ()
    {
        base.OnBindingContextChanged ();

        if (BindingContext != null)
        {
            nameLabel.Text = Name;
            ageLabel.Text = Age.ToString ();
            locationLabel.Text = Location;
        }
    }
}
```

Die [ `OnBindingContextChanged` ](xref:Xamarin.Forms.Cell.OnBindingContextChanged) Außerkraftsetzung wird aufgerufen, wenn die [ `BindingContextChanged` ](xref:Xamarin.Forms.BindableObject.BindingContextChanged) Ereignis wird ausgelöst, als Reaktion auf den Wert des der [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) Eigenschaft zu ändern. Aus diesem Grund, wenn die `BindingContext` geändert wird, die UI-Steuerelemente, die Anzeige der [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) Werte sollten ihre Daten festgelegt. Beachten Sie, dass die `BindingContext` sollte überprüft werden eine `null` -Wert fest, wie dies von Xamarin.Forms für die Garbagecollection festgelegt werden kann, die wiederum führt die `OnBindingContextChanged` außer Kraft setzen, die aufgerufen wird.

Sie können Benutzeroberflächen-Steuerelemente binden, um die [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) Instanzen zum Anzeigen von deren Werte, außer Kraft setzen müssen entfernt die `OnBindingContextChanged` Methode.

> [!NOTE]
> Beim Überschreiben von `OnBindingContextChanged`, stellen Sie sicher, dass der Basisklasse `OnBindingContextChanged` Methode wird aufgerufen, damit registrierte Delegaten empfangen die `BindingContextChanged` Ereignis.

In XAML kann das Binden von benutzerdefinierten Zellentyps an Daten erreicht werden wie im folgenden Codebeispiel gezeigt:

```xaml
<ListView x:Name="listView">
    <ListView.ItemTemplate>
        <DataTemplate>
            <local:CustomCell Name="{Binding Name}" Age="{Binding Age}" Location="{Binding Location}" />
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Bindet die `Name`, `Age`, und `Location` bindbare Eigenschaften in der `CustomCell` Instanz, zu der `Name`, `Age`, und `Location` Eigenschaften jedes Objekts in der zugrunde liegenden Auflistung.

Die entsprechende Bindung im C# -Code wird im folgenden Codebeispiel dargestellt:

```csharp
var customCell = new DataTemplate (typeof(CustomCell));
customCell.SetBinding (CustomCell.NameProperty, "Name");
customCell.SetBinding (CustomCell.AgeProperty, "Age");
customCell.SetBinding (CustomCell.LocationProperty, "Location");

var listView = new ListView
{
    ItemsSource = people,
    ItemTemplate = customCell
};
```

Unter iOS und Android Wenn die [ `ListView` ](xref:Xamarin.Forms.ListView) wirft Elemente und die benutzerdefinierte Zelle wird einen benutzerdefinierten Renderer verwendet, der benutzerdefinierte Renderer muss eigenschaftenänderungen ordnungsgemäß implementieren. Wenn Zellen wiederverwendet werden ihre Eigenschaftswerte ändert, wenn der Bindungskontext mit einer Zelle für zur Verfügung, mit aktualisiert wird `PropertyChanged` Ereignisse, die ausgelöst wird. Weitere Informationen finden Sie unter [Anpassen einer ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md). Weitere Informationen zu der Zelle wiederverwenden, finden Sie unter [Strategie für die Zwischenspeicherung](~/xamarin-forms/user-interface/listview/performance.md#caching-strategy).

## <a name="related-links"></a>Verwandte Links

- [Erstellt in Zellen (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-builtincells)
- [Benutzerdefinierte Zellen (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-customcells)
- [Bindung der Kontext geändert (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-bindingcontextchanged)
