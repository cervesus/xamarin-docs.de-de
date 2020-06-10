---
Title: " Xamarin.Forms TableView" Description: "in diesem Artikel wird erläutert, wie Sie die Xamarin.Forms TableView-Klasse verwenden, um Scrollmenüs, Einstellungen und Eingabeformulare in-Anwendungen darzustellen."
ms. Prod: xamarin ms. assetid: D1619D19-A74F-40DF-8E53-B1B7DFF7A3FB ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 09/25/2019 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-tableview"></a>Xamarin.FormsTableView

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-tableview)

[`TableView`](xref:Xamarin.Forms.TableView)ist eine Ansicht für die Anzeige von Bild lauffähigen Listen mit Daten oder Optionen, bei denen Zeilen vorhanden sind, die nicht dieselbe Vorlage verwenden. Im Gegensatz zu [ListView](~/xamarin-forms/user-interface/listview/index.md)hat `TableView` nicht das Konzept von `ItemsSource` , sodass Elemente manuell als untergeordnete Elemente hinzugefügt werden müssen.

![TableView-Beispiel](tableview-images/tableview-all-sml.png)

## <a name="use-cases"></a>Anwendungsfälle

[`TableView`](xref:Xamarin.Forms.TableView)ist nützlich, wenn:

- eine Liste der Einstellungen wird angezeigt.
- Sammeln von Daten in einem Formular oder
- Anzeige von Daten, die von Zeile zu Zeile (z. b. Zahlen, Prozentsätze und Bilder) unterschiedlich dargestellt werden.

[`TableView`](xref:Xamarin.Forms.TableView)behandelt das Scrollen und das Anordnen von Zeilen in attraktiven Abschnitten, eine häufige Anforderung für die oben genannten Szenarien. Das `TableView` Steuerelement verwendet die zugrunde liegende äquivalente Ansicht jeder Plattform, sofern verfügbar, und erstellt eine native Betrachtung für jede Plattform.

## <a name="structure"></a>Struktur

Elemente in einer [`TableView`](xref:Xamarin.Forms.TableView) sind in Abschnitte organisiert. Am `TableView` Stamm des ist das, das [`TableRoot`](xref:Xamarin.Forms.TableRoot) einer oder mehreren-Instanzen übergeordnet ist [`TableSection`](xref:Xamarin.Forms.TableSection) . Jede [`TableSection`](xref:Xamarin.Forms.TableSection) besteht aus einer Überschrift und mindestens einer [`ViewCell`](xref:Xamarin.Forms.ViewCell) Instanz:

```xaml
<TableView Intent="Settings">
    <TableRoot>
        <TableSection Title="Ring">
            <SwitchCell Text="New Voice Mail" />
            <SwitchCell Text="New Mail" On="true" />
        </TableSection>
    </TableRoot>
</TableView>
```

Der entsprechende C#-Code lautet:

```csharp
Content = new TableView
{
    Root = new TableRoot
    {
        new TableSection("Ring")
        {
          // TableSection constructor takes title as an optional parameter
          new SwitchCell { Text = "New Voice Mail" },
          new SwitchCell { Text = "New Mail", On = true }
        }
    },
    Intent = TableIntent.Settings
};
```

## <a name="appearance"></a>Darstellung

[`TableView`](xref:Xamarin.Forms.TableView)macht die- [`Intent`](xref:Xamarin.Forms.TableView.Intent) Eigenschaft verfügbar, die auf einen der Enumerationsmember festgelegt werden kann [`TableIntent`](xref:Xamarin.Forms.TableIntent) :

- `Data`–, das beim Anzeigen von Daten Einträgen verwendet werden soll. Beachten Sie, dass [ListView](~/xamarin-forms/user-interface/listview/index.md) möglicherweise eine bessere Option für den Bildlauf von Listen mit Daten ist.
- `Form`– wird verwendet, wenn die TableView als Formular fungiert.
- `Menu`–, der verwendet werden soll, wenn ein Menü der Auswahl angezeigt wird.
- `Settings`–, das beim Anzeigen einer Liste von Konfigurationseinstellungen verwendet werden soll.

Der [`TableIntent`](xref:Xamarin.Forms.TableIntent) von Ihnen gewählte Wert kann sich darauf auswirken, wie [`TableView`](xref:Xamarin.Forms.TableView) auf jeder Plattform angezeigt wird. Auch wenn es keine klaren Unterschiede gibt, empfiehlt es sich, das auszuwählen, das `TableIntent` am ehesten mit der Verwendung der Tabelle übereinstimmt.

Außerdem kann die Farbe des jeweils angezeigten Texts [`TableSection`](xref:Xamarin.Forms.TableSection) geändert werden, indem die-Eigenschaft auf festgelegt wird `TextColor` [`Color`](xref:Xamarin.Forms.Color) .

## <a name="built-in-cells"></a>Integrierte Zellen

Xamarin.Formsenthält integrierte Zellen zum Erfassen und Anzeigen von Informationen. Obwohl [`ListView`](xref:Xamarin.Forms.ListView) und [`TableView`](xref:Xamarin.Forms.TableView) die gleichen Zellen verwenden können, [`SwitchCell`](xref:Xamarin.Forms.SwitchCell) [`EntryCell`](xref:Xamarin.Forms.EntryCell) sind und für ein Szenario am relevantesten `TableView` .

Eine ausführliche Beschreibung von [textcell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#textcell) und [ImageCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#imagecell)finden Sie unter [ListView Cell Appearance](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md) .

### <a name="switchcell"></a>Switchcell

[`SwitchCell`](xref:Xamarin.Forms.SwitchCell)ist das Steuerelement, das zum darstellen und Erfassen eines ein-/ausschalten oder-Zustands verwendet werden soll `true` / `false` . In ihr werden die folgenden Eigenschaften definiert:

- `Text`– Text, der neben dem Switch angezeigt werden soll.
- `On`– Gibt an, ob der Schalter als ein-oder ausgeschaltet angezeigt wird.
- `OnColor`– die [`Color`](xref:Xamarin.Forms.Color) des Schalters, wenn Sie sich in der Position an befindet.

Alle diese Eigenschaften sind bindbar.

[`SwitchCell`](xref:Xamarin.Forms.SwitchCell)macht auch das- `OnChanged` Ereignis verfügbar, sodass Sie auf Änderungen im Zustand der Zelle reagieren können.

![Switchcell-Beispiel](tableview-images/switch-cell.png)

### <a name="entrycell"></a>Entrycell

[`EntryCell`](xref:Xamarin.Forms.EntryCell)ist nützlich, wenn Sie Textdaten anzeigen müssen, die der Benutzer bearbeiten kann. In ihr werden die folgenden Eigenschaften definiert:

- `Keyboard`– Die Tastatur, die während der Bearbeitung angezeigt werden soll. Es gibt Optionen, wie z. b. numerische Werte, e-Mail, Telefonnummern usw., finden Sie in [der API](xref:Xamarin.Forms.Keyboard)-Dokumentation.
- `Label`– Der Bezeichnungs Text, der auf der linken Seite des Texteingabe Felds angezeigt werden soll.
- `LabelColor`– Die Farbe des Beschriftungs Texts.
- `Placeholder`– Text, der im Eingabefeld angezeigt werden soll, wenn der Wert NULL oder leer ist. Dieser Text verschwindet, wenn der Text Eintrag beginnt.
- `Text`– Der Text im Eingabefeld.
- `HorizontalTextAlignment`– Die horizontale Ausrichtung des Texts. Werte sind zentriert, Links oder rechtsbündig ausgerichtet. [Weitere Informationen finden Sie unter API docs](xref:Xamarin.Forms.TextAlignment).
- `VerticalTextAlignment`– Die vertikale Ausrichtung des Texts. Werte sind `Start` , `Center` oder `End` .

[`EntryCell`](xref:Xamarin.Forms.EntryCell)macht auch das- `Completed` Ereignis verfügbar, das ausgelöst wird, wenn der Benutzer beim Bearbeiten von Text auf die Schaltfläche "Done" auf der Tastatur trifft.

![Entrycell-Beispiel](tableview-images/entry-cell.png)

## <a name="custom-cells"></a>Benutzerdefinierte Zellen

Wenn die integrierten Zellen nicht ausreichen, können benutzerdefinierte Zellen verwendet werden, um Daten auf die Weise darzustellen und aufzuzeichnen, die für Ihre APP sinnvoll ist. Sie können z. b. einen Schieberegler darstellen, um Benutzern die Möglichkeit zu geben, die Deckkraft eines Bilds auszuwählen.

Alle benutzerdefinierten Zellen müssen von abgeleitet [`ViewCell`](xref:Xamarin.Forms.ViewCell) werden. Dies ist die Basisklasse, die von allen integrierten Zelltypen verwendet wird.

Dies ist ein Beispiel für eine benutzerdefinierte Zelle:

![Beispiel für benutzerdefinierte Zelle](tableview-images/custom-cell.png)

Das folgende Beispiel zeigt den XAML-Code, der verwendet wird, um den [`TableView`](xref:Xamarin.Forms.TableView) in den obigen Screenshots zu erstellen:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DemoTableView.TablePage"
             Title="TableView">
      <TableView Intent="Settings">
          <TableRoot>
              <TableSection Title="Getting Started">
                  <ViewCell>
                      <StackLayout Orientation="Horizontal">
                          <Image Source="bulb.png" />
                          <Label Text="left"
                                 TextColor="#f35e20" />
                          <Label Text="right"
                                 HorizontalOptions="EndAndExpand"
                                 TextColor="#503026" />
                      </StackLayout>
                  </ViewCell>
              </TableSection>
          </TableRoot>
      </TableView>
</ContentPage>
```

Der entsprechende C#-Code lautet:

```csharp
var table = new TableView();
table.Intent = TableIntent.Settings;
var layout = new StackLayout() { Orientation = StackOrientation.Horizontal };
layout.Children.Add (new Image() { Source = "bulb.png"});
layout.Children.Add (new Label()
{
    Text = "left",
    TextColor = Color.FromHex("#f35e20"),
    VerticalOptions = LayoutOptions.Center
});
layout.Children.Add (new Label ()
{
    Text = "right",
    TextColor = Color.FromHex ("#503026"),
    VerticalOptions = LayoutOptions.Center,
    HorizontalOptions = LayoutOptions.EndAndExpand
});
table.Root = new TableRoot ()
{
    new TableSection("Getting Started")
    {
        new ViewCell() {View = layout}
    }
};
Content = table;
```

Das Stamm Element unter dem [`TableView`](xref:Xamarin.Forms.TableView) ist das [`TableRoot`](xref:Xamarin.Forms.TableRoot) , und es befindet sich [`TableSection`](xref:Xamarin.Forms.TableSection) unmittelbar unterhalb von `TableRoot` . Der [`ViewCell`](xref:Xamarin.Forms.ViewCell) wird direkt unter der definiert `TableSection` , und ein [`StackLayout`](xref:Xamarin.Forms.StackLayout) wird verwendet, um das Layout der benutzerdefinierten Zelle zu verwalten, obwohl jedes Layout hier verwendet werden kann.

> [!NOTE]
> Im Gegensatz zu [`ListView`](xref:Xamarin.Forms.ListView) [`TableView`](xref:Xamarin.Forms.TableView) erfordert nicht, dass benutzerdefinierte (oder irgendwelche) Zellen in definiert sind `ItemTemplate` .

## <a name="row-height"></a>Zeilenhöhe

Die- [`TableView`](xref:Xamarin.Forms.TableView) Klasse verfügt über zwei Eigenschaften, die verwendet werden können, um die Zeilenhöhe von Zellen zu ändern:

- [`RowHeight`](xref:Xamarin.Forms.TableView.RowHeight)– legt die Höhe der einzelnen Zeilen auf einen fest `int` .
- [`HasUnevenRows`](xref:Xamarin.Forms.TableView.HasUnevenRows)– Zeilen haben unterschiedliche Höhen, wenn auf festgelegt `true` . Beachten Sie, dass die Zeilenhöhe beim Festlegen dieser Eigenschaft auf `true` automatisch von berechnet und angewendet wird Xamarin.Forms .

Wenn die Höhe des Inhalts in einer Zelle in einer [`TableView`](xref:Xamarin.Forms.TableView) geändert wird, wird die Zeilenhöhe implizit unter Android und der universelle Windows-Plattform (UWP) aktualisiert. Allerdings muss für IOS die Aktualisierung erzwungen werden, indem die-Eigenschaft auf festgelegt wird, [`HasUnevenRows`](xref:Xamarin.Forms.TableView.HasUnevenRows) `true` indem die-Methode aufgerufen wird [`Cell.ForceUpdateSize`](xref:Xamarin.Forms.Cell.ForceUpdateSize) .

Das folgende XAML-Beispiel zeigt ein-Wert [`TableView`](xref:Xamarin.Forms.TableView) , der ein enthält [`ViewCell`](xref:Xamarin.Forms.ViewCell) :

```xaml
<ContentPage ...>
    <TableView ...
               HasUnevenRows="true">
        <TableRoot>
            ...
            <TableSection ...>
                ...
                <ViewCell x:Name="_viewCell"
                          Tapped="OnViewCellTapped">
                    <Grid Margin="15,0">
                        <Grid.RowDefinitions>
                            <RowDefinition Height="Auto" />
                            <RowDefinition Height="Auto" />
                        </Grid.RowDefinitions>
                        <Label Text="Tap this cell." />
                        <Label x:Name="_target"
                               Grid.Row="1"
                               Text="The cell has changed size."
                               IsVisible="false" />
                    </Grid>
                </ViewCell>
            </TableSection>
        </TableRoot>
    </TableView>
</ContentPage>
```

Wenn das- [`ViewCell`](xref:Xamarin.Forms.ViewCell) Ereignis angetippt wird, wird der- `OnViewCellTapped` Ereignishandler ausgeführt:

```csharp
void OnViewCellTapped(object sender, EventArgs e)
{
    _target.IsVisible = !_target.IsVisible;
    _viewCell.ForceUpdateSize();
}
```

Der `OnViewCellTapped` Ereignishandler zeigt den zweiten in der an oder blendet ihn [`Label`](xref:Xamarin.Forms.Label) aus [`ViewCell`](xref:Xamarin.Forms.ViewCell) und aktualisiert die Größe der Zelle explizit durch Aufrufen der- [`Cell.ForceUpdateSize`](xref:Xamarin.Forms.Cell.ForceUpdateSize) Methode.

Die folgenden Screenshots zeigen die Zelle vor dem Tippen auf:

![Viewcell vor der Größenänderung](tableview-images/cell-beforeresize.png)

Die folgenden Screenshots zeigen die Zelle nach dem Tippen auf:

![Viewcell nach der Größenänderung](tableview-images/cell-afterresize.png)

> [!IMPORTANT]
> Wenn diese Funktion überlastet ist, besteht eine hohe Wahrscheinlichkeit einer Leistungsminderung.

## <a name="related-links"></a>Verwandte Links

- [TableView (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-tableview)
