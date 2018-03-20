---
title: Darstellung der Zelle
description: Untersuchen Sie die Optionen zum Darstellen von Daten beim Nutzen der Vorteile ListView.
ms.topic: article
ms.prod: xamarin
ms.assetid: FD45CB91-1A8F-46FB-B432-6BC20492E456
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2016
ms.openlocfilehash: 551a0de8cd4965815c67a795fb5723d4261a173c
ms.sourcegitcommit: cc38757f56aab53bce200e40f873eb8d0e5393c3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2018
---
# <a name="cell-appearance"></a>Darstellung der Zelle

ListView präsentiert bildlauffähige Listen, die durch die Verwendung der angepasst werden können `ViewCell`s. `ViewCells` Dient zum Anzeigen von Text und Bilder, der angibt, eines wahr/falsch-Status und Empfangen von Benutzereingaben.

Es gibt zwei Ansätze zum Abrufen des Aussehens von ListView Zellen gewünschten:

- **[Anpassen von integrierten Zellen](#Built_in_Cells)**  &ndash; einfacheren Implementierung und eine bessere Leistung auf Kosten der Berichtsebene.
- **[Erstellen von benutzerdefinierten Zellen](#customcells)**  &ndash; mehr Kontrolle über das Endergebnis, jedoch können Sie möglicherweise Leistungsprobleme auftreten, wenn nicht ordnungsgemäß implementiert.

<a name="Built_in_Cells" />

## <a name="built-in-cells"></a>Erstellt in Zellen
Xamarin.Forms verfügt über integrierte Zellen, die für viele einfache Anwendungen unterstützt:

- **TextCell** &ndash; für die Anzeige von Text
- **ImageCell** &ndash; zum Anzeigen eines Bilds mit Text.

Zwei zusätzliche Zellen, [ `SwitchCell` ](~/xamarin-forms/user-interface/tableview.md#switchcell) und [ `EntryCell` ](~/xamarin-forms/user-interface/tableview.md#entrycell) verfügbar sind, jedoch häufig bei Verwendung sind nicht `ListView`. Finden Sie unter [ `TableView` ](~/xamarin-forms/user-interface/tableview.md) für Weitere Informationen über diese Zellen.

<a name="TextCell" />

### <a name="textcell"></a>TextCell

[`TextCell`](http://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/) ist eine Zelle für die Anzeige von Text, optional mit einer zweiten Zeile als Detail Text.

TextCells als systemeigene Steuerelemente zur Laufzeit gerendert werden, damit die Leistung sehr gut ist im Vergleich zu einer benutzerdefinierten `ViewCell`. TextCells können angepasst werden, weil Sie damit festlegen:

- `Text` &ndash; der Text, der auf der ersten Zeile in großer Schriftart angezeigt wird.
- `Detail` &ndash; der Text, der unterhalb der ersten Zeile in einer kleineren Schriftart dargestellt wird.
- `TextColor` &ndash; die Farbe des Texts.
- `DetailColor` &ndash; die Farbe des Texts detail

![](customizing-cell-appearance-images/text-cell-default.png "Standard-TextCell-Beispiel")

![](customizing-cell-appearance-images/text-cell-custom.png "Benutzerdefinierte TextCell-Beispiel")

<a name="ImageCell" />

### <a name="imagecell"></a>ImageCell

[`ImageCell`](http://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/), z. B. `TextCell`, für die Anzeige von Text und sekundären Detail Text verwendet werden können und bietet hervorragende Leistung mithilfe von systemeigenen Steuerelementen für jede Plattform. `ImageCell` unterscheidet sich von `TextCell` insofern, dass sie ein Bild auf der linken Seite des Texts anzeigt.

`ImageCell` ist nützlich, wenn Sie eine Liste von Daten mit der ein visueller Aspekt, z. B. eine Liste der Kontakte oder Filme anzuzeigen müssen. ImageCells können angepasst werden, weil Sie damit festlegen:

- `Text` &ndash; der Text, der auf der ersten Zeile in großer Schriftart angezeigt wird
- `Detail` &ndash; der Text, der unterhalb der ersten Zeile in einer kleineren Schriftart angezeigt wird
- `TextColor` &ndash; die Farbe des Texts
- `DetailColor` &ndash; die Farbe des Texts detail
- `ImageSource` &ndash; das Bild, das neben dem Text angezeigt

Beachten Sie, dass Geschäftsgruppen mit Windows Phone 8.1, `ImageCell` wird nicht standardmäßig Skalieren von Bildern. Beachten Sie, dass Windows Phone 8.1 die einzige Plattform ist, auf welches, die Detailelement Text angezeigt wird, in der gleichen Farbe und Schriftart als primäre Text standardmäßig. Windows Phone 8.0 rendert `ImageCell` wie unten dargestellt:

![](customizing-cell-appearance-images/image-cell-default.png "Standard-ImageCell-Beispiel")

![](customizing-cell-appearance-images/image-cell-custom.png "Benutzerdefinierte ImageCell-Beispiel")

<a name="customcells" />

## <a name="custom-cells"></a>Benutzerdefinierte Zellen
Wenn die integrierte Zellen nicht das erforderliche Layout bereitstellen, implementiert benutzerdefinierte Zellen das Layout erforderliche. Beispielsweise empfiehlt es sich eine Zelle mit zwei Bezeichnungen dar, die gleiche Gewichtung zu haben. Ein `LabelCell` wäre nicht genügend da die `LabelCell` verfügt über eine Bezeichnung, die kleiner ist. Die meisten Zelle Anpassungen hinzufügen, zusätzliche schreibgeschützte Daten (z. B. zusätzliche Bezeichnungen, Bilder oder andere Anzeigeinformationen).

Alle benutzerdefinierte Zellen abgeleitet müssen [ `ViewCell` ](http://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/), die dieselbe Basisklasse an, dass alle integrierten Zelle Typen verwenden.

Xamarin.Forms 2 eingeführt, ein neues [Zwischenspeichern von Abfragezeichenfolgen](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy) auf die `ListView` steuern, welche das Durchführen eines Bildlaufs leistungsverbesserung für einige Typen von benutzerdefinierten Zellen festgelegt werden kann.

Dies ist ein Beispiel für eine benutzerdefinierte Zelle:

![](customizing-cell-appearance-images/custom-cell.png "Beispiel für benutzerdefinierte Zelle")

### <a name="xaml"></a>XAML
Der XAML-Code, der oben genannten Layout erstellt wird, lautet wie folgt:

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

Die obige XAML-Code ist deutlich machen. Schön:

- In die benutzerdefinierte Zelle geschachtelt ist ein `DataTemplate`, also in `ListView.ItemTemplate`. Dies ist die gleichen Prozess wie eine beliebige andere Zelle verwenden.
- `ViewCell` ist der Typ der benutzerdefinierten Zelle. Das untergeordnete Element eines der `DataTemplate` Element muss vom oder den Typ ableiten `ViewCell`.
- Beachten Sie, innerhalb der `ViewCell`, Layout erfolgt durch eine `StackLayout`. Dieses Layout kann wir die Farbe des Hintergrunds anpassen. Beachten Sie, dass jede beliebige Eigenschaft `StackLayout` also bindbare kann innerhalb einer benutzerdefinierten Zelle gebunden werden, auch wenn, hier nicht angezeigt wird.

### <a name="cnum"></a>C&num;

Angeben einer benutzerdefinierten Zelle in c# ist etwas ausführlicher als die Verwendung von XAML-Entsprechung. Sehen wir uns das einmal genauer an:

Zuerst definieren Sie eine benutzerdefinierte Zellenklasse mit `ViewCell` als Basisklasse:

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

Im Konstruktor für die Seite mit den `ListView`, legen Sie die ListView `ItemTemplate` Eigenschaft, um ein neues `DataTemplate`:

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

Beachten Sie, dass der Konstruktor für `DataTemplate` nimmt einen Typ an. Typeof-Operator Ruft den CLR-Typ für `CustomCell`.

<a name="binding-context-changes" />

### <a name="binding-context-changes"></a>Bindungsänderungen Kontext

Beim Binden an einen benutzerdefinierten Zellentyps [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) -Instanzen bestehen, die Anzeige von UI-Steuerelemente der `BindableProperty` Werte verwenden, sollten die [ `OnBindingContextChanged` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Cell.OnBindingContextChanged()/) Außerkraftsetzung Festlegen der anzuzeigenden jede Zelle, anstatt der Zelle Konstruktor, wie im folgenden Codebeispiel wird veranschaulicht:

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

    public string Name {
        get { return(string)GetValue (NameProperty); }
        set { SetValue (NameProperty, value); }
    }

    public int Age {
        get { return(int)GetValue (AgeProperty); }
        set { SetValue (AgeProperty, value); }
    }

    public string Location {
        get { return(string)GetValue (LocationProperty); }
        set { SetValue (LocationProperty, value); }
    }
    ...

    protected override void OnBindingContextChanged ()
    {
        base.OnBindingContextChanged ();

        if (BindingContext != null) {
            nameLabel.Text = Name;
            ageLabel.Text = Age.ToString ();
            locationLabel.Text = Location;
        }
    }
}
```

Die [ `OnBindingContextChanged` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Cell.OnBindingContextChanged()/) Außerkraftsetzung wird aufgerufen, wenn die [ `BindingContextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.BindableObject.BindingContextChanged/) Ereignis wird ausgelöst, als Antwort auf den Wert von der [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) Eigenschaft ändern. Aus diesem Grund, wenn die `BindingContext` ändert die Anzeige von UI-Steuerelemente der [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) Werte sollten ihre Daten festgelegt. Beachten Sie, dass die `BindingContext` sollte überprüft werden eine `null` -Wert fest, wie dies von Xamarin.Forms für Garbagecollection festgelegt werden kann, die wiederum führt die `OnBindingContextChanged` überschreiben aufgerufen werden.

Sie können Benutzeroberflächen-Steuerelemente binden, um die [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) Instanzen anzuzeigenden ihre Werte, die außer Kraft setzen müssen entfernt der `OnBindingContextChanged` Methode.

> [!NOTE]
> Zum Überschreiben `OnBindingContextChanged`, stellen Sie sicher, dass der Basisklasse `OnBindingContextChanged` aufgerufen wird, damit registrierte Delegaten empfangen die `BindingContextChanged` Ereignis.

In XAML kann Binden von benutzerdefinierten Zellentyps an Daten erreicht werden wie im folgenden Codebeispiel gezeigt:

```xaml
<ListView x:Name="listView">
    <ListView.ItemTemplate>
        <DataTemplate>
            <local:CustomCell Name="{Binding Name}" Age="{Binding Age}" Location="{Binding Location}" />
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Bindet die `Name`, `Age`, und `Location` bindbare Eigenschaften in der `CustomCell` Instanz, auf die `Name`, `Age`, und `Location` Eigenschaften der einzelnen Objekte in der zugrunde liegenden Auflistung.

Im folgenden Codebeispiel wird die entsprechende Bindung im C#-angezeigt:

```csharp
var customCell = new DataTemplate (typeof(CustomCell));
customCell.SetBinding (CustomCell.NameProperty, "Name");
customCell.SetBinding (CustomCell.AgeProperty, "Age");
customCell.SetBinding (CustomCell.LocationProperty, "Location");

var listView = new ListView {
    ItemsSource = people,
    ItemTemplate = customCell
};
```

Auf IOS- und Android Wenn die [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) ist Wiederverwendung Elemente und benutzerdefinierte Zelle mithilfe eines benutzerdefinierten Renderers, der benutzerdefinierte Renderer muss änderungsbenachrichtigung korrekt implementieren. Wenn Zellen wiederverwendet werden wird ihre Eigenschaftswerte ändern, wenn der Bindungskontext mit einer verfügbaren Zelle aktualisiert wird, mit `PropertyChanged` Ereignisse, die ausgelöst wird. Weitere Informationen finden Sie unter [Anpassen einer ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md). Weitere Informationen zu der Zelle wiederverwenden, finden Sie unter [Zwischenspeichern Strategie](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy).

## <a name="related-links"></a>Verwandte Links

- [Erstellt in Zellen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/BuiltInCells)
- [Benutzerdefinierte Zellen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/CustomCells)
- [Bindung der Kontext geändert (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/BindingContextChanged)
