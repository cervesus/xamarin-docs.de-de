---
title: Xamarin.Forms TableView
description: In diesem Artikel wird erläutert, wie die Xamarin.Forms TableView-Klasse, die zur Darstellung von Bildlaufmenüs, Einstellungen und Eingabeformulare in Anwendungen verwendet wird.
ms.prod: xamarin
ms.assetid: D1619D19-A74F-40DF-8E53-B1B7DFF7A3FB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/14/2018
ms.openlocfilehash: 558eb9f476fd6b566f1f161c01fc809498a4c4a8
ms.sourcegitcommit: a5ef4497db04dfa016865bc7454b3de6ff088554
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/15/2019
ms.locfileid: "70997995"
---
# <a name="xamarinforms-tableview"></a>Xamarin.Forms TableView

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-tableview)

[`TableView`](xref:Xamarin.Forms.TableView)ist eine Ansicht für die Anzeige von Bild lauffähigen Listen mit Daten oder Optionen, bei denen Zeilen vorhanden sind, die nicht dieselbe Vorlage verwenden. Im Gegensatz zu [ListView](~/xamarin-forms/user-interface/listview/index.md) `TableView` hat nicht `ItemsSource`das Konzept von, sodass Elemente manuell als untergeordnete Elemente hinzugefügt werden müssen.

![TableView-Beispiel](tableview-images/tableview-all-sml.png)

<a name="Use_Cases" />

## <a name="use-cases"></a>Anwendungsfälle

[`TableView`](xref:Xamarin.Forms.TableView)ist nützlich, wenn:

- eine geordnete Liste der Einstellungen,
- Sammeln von Daten in einem Formular oder
- Anzeigen von Daten, unterschiedlich aus Zeile die Zeile (z. B. Zahlen, Prozentsätze und Bilder) angezeigt werden.

[`TableView`](xref:Xamarin.Forms.TableView)behandelt das Scrollen und das Anordnen von Zeilen in attraktiven Abschnitten, eine häufige Anforderung für die oben genannten Szenarien. Die `TableView` -Steuerelement verwendet jede Plattform des zugrunde liegenden entsprechende anzeigen, sofern verfügbar, beim Erstellen eines einheitlichen Erscheinungsbilds für jede Plattform.

<a name="TableView_Structure" />

## <a name="structure"></a>Struktur

Elemente in einer [`TableView`](xref:Xamarin.Forms.TableView) sind in Abschnitte organisiert. Am Stamm des `TableView` ist das [`TableRoot`](xref:Xamarin.Forms.TableRoot), das einer oder mehreren [`TableSection`](xref:Xamarin.Forms.TableSection) -Instanzen übergeordnet ist. Jede [`TableSection`](xref:Xamarin.Forms.TableSection) besteht aus einer Überschrift und mindestens einer [`ViewCell`](xref:Xamarin.Forms.ViewCell) Instanz:

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

Der entsprechende C#-Code ist:

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

<a name="TableView_Appearance" />

## <a name="appearance"></a>Darstellung

[`TableView`](xref:Xamarin.Forms.TableView)macht die [`Intent`](xref:Xamarin.Forms.TableView.Intent) -Eigenschaft verfügbar, die auf einen [`TableIntent`](xref:Xamarin.Forms.TableIntent) der Enumerationsmember festgelegt werden kann:

- `Data`–, das beim Anzeigen von Daten Einträgen verwendet werden soll. Beachten Sie, dass [ListView](~/xamarin-forms/user-interface/listview/index.md) möglicherweise eine bessere Option für den Bildlauf von Listen mit Daten.
- `Form`– wird verwendet, wenn die TableView als Formular fungiert.
- `Menu`–, der verwendet werden soll, wenn ein Menü der Auswahl angezeigt wird.
- `Settings`–, das beim Anzeigen einer Liste von Konfigurationseinstellungen verwendet werden soll.

Der [`TableIntent`](xref:Xamarin.Forms.TableIntent) von Ihnen gewählte Wert kann sich darauf [`TableView`](xref:Xamarin.Forms.TableView) auswirken, wie auf jeder Plattform angezeigt wird. Auch wenn Sie vorhanden sind, deaktivieren die Unterschiede nicht, es ist eine bewährte Methode, wählen Sie die `TableIntent` , die am ehesten entspricht, wie Sie in der Tabelle verwenden möchten.

Außerdem kann die Farbe des jeweils [`TableSection`](xref:Xamarin.Forms.TableSection) angezeigten Texts geändert werden, indem die `TextColor` -Eigenschaft auf [`Color`](xref:Xamarin.Forms.Color)festgelegt wird.

<a name="Built-In_Cells" />

## <a name="built-in-cells"></a>Integrierte Zellen

Xamarin.Forms enthält integrierte Zellen für das Sammeln und Anzeigen von Informationen. Obwohl [`ListView`](xref:Xamarin.Forms.ListView) [`SwitchCell`](xref:Xamarin.Forms.SwitchCell) und [`TableView`](xref:Xamarin.Forms.TableView) die gleichen Zellen verwenden können, sind und [`EntryCell`](xref:Xamarin.Forms.EntryCell) für ein `TableView` Szenario am relevantesten.

Finden Sie unter [Darstellung des ListView-Zelle](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md) für eine ausführliche Beschreibung der [TextCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#textcell) und [ImageCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#imagecell).

<a name="switchcell" />

### <a name="switchcell"></a>SwitchCell

[`SwitchCell`](xref:Xamarin.Forms.SwitchCell) ist das Steuerelement zum Bereitstellen und erfassen ein/aus oder `true` / `false` Zustand. Sie definiert die folgenden Eigenschaften:

- `Text`– Text, der neben dem Switch angezeigt werden soll.
- `On`– Gibt an, ob der Schalter als ein-oder ausgeschaltet angezeigt wird.
- `OnColor`– die [`Color`](xref:Xamarin.Forms.Color) des Schalters, wenn Sie sich in der Position an befindet.

Alle diese Eigenschaften sind bindbar.

[`SwitchCell`](xref:Xamarin.Forms.SwitchCell)macht auch das `OnChanged` -Ereignis verfügbar, sodass Sie auf Änderungen im Zustand der Zelle reagieren können.

![Switchcell-Beispiel](tableview-images/switch-cell.png)

<a name="entrycell" />

### <a name="entrycell"></a>EntryCell

[`EntryCell`](xref:Xamarin.Forms.EntryCell) ist nützlich, wenn Sie müssen zum Anzeigen von Textdaten, die der Benutzer bearbeiten können. Sie definiert die folgenden Eigenschaften:

- `Keyboard`– Die Tastatur, die während der Bearbeitung angezeigt werden soll. Es gibt Optionen für Dinge wie numerische Werte, e-Mail-Adresse, Telefonnummern usw. ein. [Finden Sie in der API-Dokumentation](xref:Xamarin.Forms.Keyboard).
- `Label`– Der Bezeichnungs Text, der auf der linken Seite des Texteingabe Felds angezeigt werden soll.
- `LabelColor`– Die Farbe des Beschriftungs Texts.
- `Placeholder`– Text, der im Eingabefeld angezeigt werden soll, wenn der Wert NULL oder leer ist. Dieser Text wird ausgeblendet, wenn der Eingabe von Text beginnt.
- `Text`– Der Text im Eingabefeld.
- `HorizontalTextAlignment`– Die horizontale Ausrichtung des Texts. Center, links oder rechts kann ausgerichtet werden. [Finden Sie in der API-Dokumentation](xref:Xamarin.Forms.TextAlignment).

[`EntryCell`](xref:Xamarin.Forms.EntryCell)macht auch das `Completed` -Ereignis verfügbar, das ausgelöst wird, wenn der Benutzer beim Bearbeiten von Text auf die Schaltfläche "Done" auf der Tastatur trifft.

![Entrycell-Beispiel](tableview-images/entry-cell.png)

<a name="Custom_Cells" />

## <a name="custom-cells"></a>Benutzerdefinierte Zellen

Wenn die integrierten Zellen nicht ausreichend sind, können benutzerdefinierte Zellen verwendet werden, und Erfassen von Daten in der Weise, die für Ihre app sinnvoll ist. Beispielsweise empfiehlt es sich um einen Schieberegler, um einem Benutzer ermöglichen, wählen Sie die Deckkraft eines Bilds darzustellen.

Alle benutzerdefinierte Zellen abgeleitet müssen [ `ViewCell` ](xref:Xamarin.Forms.ViewCell), der gleichen Basisklasse an, dass alle der Zelle integrierte Typen verwenden.

Dies ist ein Beispiel für eine benutzerdefinierte Zelle:

![Beispiel für benutzerdefinierte Zelle](tableview-images/custom-cell.png)

Das folgende Beispiel zeigt den XAML-Code, der [`TableView`](xref:Xamarin.Forms.TableView) verwendet wird, um den in den obigen Screenshots zu erstellen:

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

Der entsprechende C#-Code ist:

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

Das Stamm Element unter dem [`TableView`](xref:Xamarin.Forms.TableView) ist das [`TableRoot`](xref:Xamarin.Forms.TableRoot) [`TableSection`](xref:Xamarin.Forms.TableSection) , und es `TableRoot`befindet sich unmittelbar unterhalb von. Der [`ViewCell`](xref:Xamarin.Forms.ViewCell) wird direkt unter der `TableSection`definiert, und ein [`StackLayout`](xref:Xamarin.Forms.StackLayout) wird verwendet, um das Layout der benutzerdefinierten Zelle zu verwalten, obwohl jedes Layout hier verwendet werden kann.

> [!NOTE]
> Im Gegensatz zu [`ListView`](xref:Xamarin.Forms.ListView)erfordert nicht, dass benutzerdefinierte (oder irgendwelche) `ItemTemplate`Zellen in definiert sind. [`TableView`](xref:Xamarin.Forms.TableView)

## <a name="row-height"></a>Zeilenhöhe

Die [ `TableView` ](xref:Xamarin.Forms.TableView) -Klasse verfügt über zwei Eigenschaften, die verwendet werden können, um die Höhe der Zellen ändern:

- [`RowHeight`](xref:Xamarin.Forms.TableView.RowHeight) – Legt die Höhe der einzelnen Zeilen einer `int`.
- [`HasUnevenRows`](xref:Xamarin.Forms.TableView.HasUnevenRows) – Zeilen über unterschiedliche Höhen verfügen, wenn auf festgelegt `true`. Beachten Sie, dass beim Festlegen dieser Eigenschaft auf `true`, Zeilenhöhe automatisch berechnet und xamarin.Forms angewendet werden.

Wenn die Höhe des Inhalts in einer Zelle in einer [ `TableView` ](xref:Xamarin.Forms.TableView) geändert wird, wird die Zeile Höhe implizit unter Android und die universelle Windows-Plattform (UWP) aktualisiert. Allerdings unter iOS er gezwungen werden muss, aktualisieren, indem Sie die Einstellung der [ `HasUnevenRows` ](xref:Xamarin.Forms.TableView.HasUnevenRows) Eigenschaft `true` und durch das Aufrufen der [ `Cell.ForceUpdateSize` ](xref:Xamarin.Forms.Cell.ForceUpdateSize) Methode.

Das folgende XAML-Beispiel zeigt eine [ `TableView` ](xref:Xamarin.Forms.TableView) , enthält eine [ `ViewCell` ](xref:Xamarin.Forms.ViewCell):

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

Wenn die [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) getippt wird, die `OnViewCellTapped` -Ereignishandler ausgeführt wird:

```csharp
void OnViewCellTapped(object sender, EventArgs e)
{
    _target.IsVisible = !_target.IsVisible;
    _viewCell.ForceUpdateSize();
}
```

Die `OnViewCellTapped` Ereignishandler zeigt, oder blendet Sie aus der zweiten [ `Label` ](xref:Xamarin.Forms.Label) in die [ `ViewCell` ](xref:Xamarin.Forms.ViewCell), und aktualisiert Sie der Zelle Größe explizit durch Aufrufen der [ `Cell.ForceUpdateSize` ](xref:Xamarin.Forms.Cell.ForceUpdateSize) Methode.

Die folgenden Screenshots zeigen die Zelle vor der wird beim Tippen:

![Viewcell vor der Größenänderung](tableview-images/cell-beforeresize.png)

Die folgenden Screenshots zeigen die Zelle nach wird beim Tippen:

![Viewcell nach der Größenänderung](tableview-images/cell-afterresize.png)

> [!IMPORTANT]
> Es gibt sehr wahrscheinlich, dass eine Verringerung der Leistung auf, wenn dieses Feature übermäßig beansprucht wird.

## <a name="related-links"></a>Verwandte Links

- [TableView (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-tableview)
