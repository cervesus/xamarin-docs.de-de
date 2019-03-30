---
title: Xamarin.Forms TableView
description: In diesem Artikel wird erläutert, wie die Xamarin.Forms TableView-Klasse, die zur Darstellung von Bildlaufmenüs, Einstellungen und Eingabeformulare in Anwendungen verwendet wird.
ms.prod: xamarin
ms.assetid: D1619D19-A74F-40DF-8E53-B1B7DFF7A3FB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/14/2018
ms.openlocfilehash: c18eba873dc1a1dae36c401507d55652ed233b00
ms.sourcegitcommit: 236a346838c421c7d8951f50abbf4f5365559372
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2019
ms.locfileid: "58641438"
---
# <a name="xamarinforms-tableview"></a>Xamarin.Forms TableView

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TableView)

[`TableView`](xref:Xamarin.Forms.TableView) ist eine Ansicht zum Anzeigen von bildlauffähigen Listen mit Daten oder Ihren Optionen angezeigt, die Zeilen, die die gleiche Vorlage gemeinsam verwenden, nicht vorhanden sind. Im Gegensatz zu [ListView](~/xamarin-forms/user-interface/listview/index.md), `TableView` verfügt nicht über das Konzept einer `ItemsSource`, sodass Elemente als untergeordnete Elemente manuell hinzugefügt werden müssen.

![](tableview-images/tableview-all-sml.png "TableView-Beispiel")

<a name="Use_Cases" />

## <a name="use-cases"></a>Anwendungsfälle

[`TableView`](xref:Xamarin.Forms.TableView) ist nützlich, wenn:

- eine geordnete Liste der Einstellungen,
- Sammeln von Daten in einem Formular oder
- Anzeigen von Daten, unterschiedlich aus Zeile die Zeile (z. B. Zahlen, Prozentsätze und Bilder) angezeigt werden.

[`TableView`](xref:Xamarin.Forms.TableView) Durchführen eines Bildlaufs und Anordnung der Zeilen in eine allgemeine Voraussetzung für die oben genannten Szenarien attraktive Abschnitten zuständig. Die `TableView` -Steuerelement verwendet jede Plattform des zugrunde liegenden entsprechende anzeigen, sofern verfügbar, beim Erstellen eines einheitlichen Erscheinungsbilds für jede Plattform.

<a name="TableView_Structure" />

## <a name="structure"></a>Struktur

Elemente in einem [ `TableView` ](xref:Xamarin.Forms.TableView) sind in Abschnitte unterteilt. Im Stammverzeichnis der `TableView` ist die [ `TableRoot` ](xref:Xamarin.Forms.TableRoot), d.h. übergeordneten an einen oder mehrere [ `TableSection` ](xref:Xamarin.Forms.TableSection) Instanzen. Jede [ `TableSection` ](xref:Xamarin.Forms.TableSection) besteht aus einer Überschrift und einer oder mehrerer [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) Instanzen:

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

[`TableView`](xref:Xamarin.Forms.TableView) macht die [ `Intent` ](xref:Xamarin.Forms.TableView.Intent) -Eigenschaft, die auf eine der festgelegt werden kann die [ `TableIntent` ](xref:Xamarin.Forms.TableIntent) Enumerationsmember:

- `Data` – für die Verwendung beim Dateneinträge anzeigen. Beachten Sie, dass [ListView](~/xamarin-forms/user-interface/listview/index.md) möglicherweise eine bessere Option für den Bildlauf von Listen mit Daten.
- `Form` – für die Verwendung bei der TableView als Formular dient.
- `Menu` – für die Verwendung bei der ein Menü der Auswahl zu präsentieren.
- `Settings` – Bei der Anzeige einer Liste von Konfigurationseinstellungen verwendet.

Die [ `TableIntent` ](xref:Xamarin.Forms.TableIntent) Wert auswirken können Sie auswählen, wie die [ `TableView` ](xref:Xamarin.Forms.TableView) auf jeder Plattform angezeigt wird. Auch wenn Sie vorhanden sind, deaktivieren die Unterschiede nicht, es ist eine bewährte Methode, wählen Sie die `TableIntent` , die am ehesten entspricht, wie Sie in der Tabelle verwenden möchten.

Darüber hinaus die Farbe des Texts angezeigt, für die einzelnen [ `TableSection` ](xref:Xamarin.Forms.TableSection) kann geändert werden, indem die `TextColor` Eigenschaft, um eine [ `Color` ](xref:Xamarin.Forms.Color).

<a name="Built-In_Cells" />

## <a name="built-in-cells"></a>Integrierte Zellen

Xamarin.Forms enthält integrierte Zellen für das Sammeln und Anzeigen von Informationen. Obwohl [ `ListView` ](xref:Xamarin.Forms.ListView) und [ `TableView` ](xref:Xamarin.Forms.TableView) können alle den gleichen Zellen, [ `SwitchCell` ](xref:Xamarin.Forms.SwitchCell) und [ `EntryCell` ](xref:Xamarin.Forms.EntryCell)die am wichtigsten sind partitionsverarbeitungsoptionen für eine `TableView` Szenario.

Finden Sie unter [Darstellung des ListView-Zelle](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md) für eine ausführliche Beschreibung der [TextCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#TextCell) und [ImageCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#ImageCell).

<a name="switchcell" />

### <a name="switchcell"></a>SwitchCell

[`SwitchCell`](xref:Xamarin.Forms.SwitchCell) ist das Steuerelement zum Bereitstellen und erfassen ein/aus oder `true` / `false` Zustand. Es definiert die folgenden Eigenschaften:

- `Text` – Text, der neben dem Switch angezeigt.
- `On` – Gibt an, ob der Schalter als on oder off angezeigt wird.
- `OnColor` – die [ `Color` ](xref:Xamarin.Forms.Color) des Schalters, wenn es in der Position ist.

Alle diese Eigenschaften sind bindbar.

[`SwitchCell`](xref:Xamarin.Forms.SwitchCell) Außerdem stellt die `OnChanged` -Ereignis, sodass Sie die Reaktion auf Änderungen in den Status der Zelle.

![](tableview-images/switch-cell.png "SwitchCell-Beispiel")

<a name="entrycell" />

### <a name="entrycell"></a>EntryCell

[`EntryCell`](xref:Xamarin.Forms.EntryCell) ist nützlich, wenn Sie müssen zum Anzeigen von Textdaten, die der Benutzer bearbeiten können. Es definiert die folgenden Eigenschaften:

- `Keyboard` – Die Tastatur, um während der Bearbeitung angezeigt werden soll. Es gibt Optionen für Dinge wie numerische Werte, e-Mail-Adresse, Telefonnummern usw. ein. [Finden Sie in der API-Dokumentation](xref:Xamarin.Forms.Keyboard).
- `Label` – Der Bezeichnungstext, auf der linken Seite des Textfelds Eintrag angezeigt werden soll.
- `LabelColor` – Die Farbe des Bezeichnungstexts.
- `Placeholder` – Text, der im Eingabefeld angezeigt wird, wenn der Wert null oder leer ist. Dieser Text wird ausgeblendet, wenn der Eingabe von Text beginnt.
- `Text` – Der Text im Eingabefeld.
- `HorizontalTextAlignment` – Die horizontale Ausrichtung des Texts. Center, links oder rechts kann ausgerichtet werden. [Finden Sie in der API-Dokumentation](xref:Xamarin.Forms.TextAlignment).

[`EntryCell`](xref:Xamarin.Forms.EntryCell) Außerdem stellt die `Completed` -Ereignis, das ausgelöst wird, wenn der Benutzer die Schaltfläche "Fertig", auf der Tastatur drückt, während der Bearbeitung von Text.

![](tableview-images/entry-cell.png "EntryCell-Beispiel")

<a name="Custom_Cells" />

## <a name="custom-cells"></a>Benutzerdefinierte Zellen

Wenn die integrierten Zellen nicht ausreichend sind, können benutzerdefinierte Zellen verwendet werden, und Erfassen von Daten in der Weise, die für Ihre app sinnvoll ist. Beispielsweise empfiehlt es sich um einen Schieberegler, um einem Benutzer ermöglichen, wählen Sie die Deckkraft eines Bilds darzustellen.

Alle benutzerdefinierte Zellen abgeleitet müssen [ `ViewCell` ](xref:Xamarin.Forms.ViewCell), der gleichen Basisklasse an, dass alle der Zelle integrierte Typen verwenden.

Dies ist ein Beispiel für eine benutzerdefinierte Zelle:

![](tableview-images/custom-cell.png "Beispiel für die benutzerdefinierte Zelle")

Das folgende Beispiel zeigt die XAML zum Erstellen der [ `TableView` ](xref:Xamarin.Forms.TableView) in den oben stehenden Screenshots:

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

Das Stammelement in der [ `TableView` ](xref:Xamarin.Forms.TableView) ist die [ `TableRoot` ](xref:Xamarin.Forms.TableRoot), und es gibt eine [ `TableSection` ](xref:Xamarin.Forms.TableSection) direkt unter der `TableRoot`. Die [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) definiert ist, direkt unterhalb der `TableSection`, und ein [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) dient zum Verwalten von des Layouts der benutzerdefinierten Zelle wird, obwohl jedes Layout hier verwendet werden kann.

> [!NOTE]
> Im Gegensatz zu [ `ListView` ](xref:Xamarin.Forms.ListView), [ `TableView` ](xref:Xamarin.Forms.TableView) ist nicht erforderlich, diese benutzerdefinierten (oder einem) Zellen werden definiert, eine `ItemTemplate`.

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

![](tableview-images/cell-beforeresize.png "ViewCell vor dem Ändern der Größe")

Die folgenden Screenshots zeigen die Zelle nach wird beim Tippen:

![](tableview-images/cell-afterresize.png "ViewCell nach dem Ändern der Größe")

> [!IMPORTANT]
> Es gibt sehr wahrscheinlich, dass eine Verringerung der Leistung auf, wenn dieses Feature übermäßig beansprucht wird.

## <a name="related-links"></a>Verwandte Links

- [TableView (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TableView)
