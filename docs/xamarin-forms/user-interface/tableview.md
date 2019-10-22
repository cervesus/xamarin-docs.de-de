---
title: Xamarin. Forms-Tabellenansicht
description: In diesem Artikel wird erläutert, wie Sie die xamarin. Forms-TableView-Klasse verwenden, um Scrollmenüs,-Einstellungen und Eingabeformulare in-Anwendungen darzustellen.
ms.prod: xamarin
ms.assetid: D1619D19-A74F-40DF-8E53-B1B7DFF7A3FB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/25/2019
ms.openlocfilehash: 67625aa413880023cce6d3e5e21e4d3bd0ec8e4c
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72695988"
---
# <a name="xamarinforms-tableview"></a>Xamarin. Forms-Tabellenansicht

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-tableview)

[`TableView`](xref:Xamarin.Forms.TableView) ist eine Ansicht für die Anzeige von Bild lauffähigen Listen mit Daten oder Optionen, bei denen Zeilen vorhanden sind, die nicht dieselbe Vorlage verwenden. Anders als bei [ListView](~/xamarin-forms/user-interface/listview/index.md)hat `TableView` nicht das Konzept eines `ItemsSource`, sodass Elemente manuell als untergeordnete Elemente hinzugefügt werden müssen.

![TableView-Beispiel](tableview-images/tableview-all-sml.png)

<a name="Use_Cases" />

## <a name="use-cases"></a>Anwendungsfälle

[`TableView`](xref:Xamarin.Forms.TableView) ist nützlich, wenn Folgendes gilt:

- eine Liste der Einstellungen wird angezeigt.
- Sammeln von Daten in einem Formular oder
- Anzeige von Daten, die von Zeile zu Zeile (z. b. Zahlen, Prozentsätze und Bilder) unterschiedlich dargestellt werden.

[`TableView`](xref:Xamarin.Forms.TableView) behandelt das Scrollen und das Anordnen von Zeilen in attraktiven Abschnitten, eine häufige Anforderung für die oben genannten Szenarien. Das `TableView`-Steuerelement verwendet die zugrunde liegende äquivalente Ansicht jeder Plattform, wenn verfügbar, und erstellt eine native Betrachtung für jede Plattform.

<a name="TableView_Structure" />

## <a name="structure"></a>Struktur

Elemente in einem [`TableView`](xref:Xamarin.Forms.TableView) sind in Abschnitte organisiert. Im Stamm des `TableView` ist der [`TableRoot`](xref:Xamarin.Forms.TableRoot), der einer oder mehreren [`TableSection`](xref:Xamarin.Forms.TableSection) Instanzen übergeordnet ist. Jede [`TableSection`](xref:Xamarin.Forms.TableSection) besteht aus einer Überschrift und einer oder mehreren [`ViewCell`](xref:Xamarin.Forms.ViewCell) Instanzen:

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

<a name="TableView_Appearance" />

## <a name="appearance"></a>Darstellung

[`TableView`](xref:Xamarin.Forms.TableView) macht die [`Intent`](xref:Xamarin.Forms.TableView.Intent) -Eigenschaft verfügbar, die auf einen der [`TableIntent`](xref:Xamarin.Forms.TableIntent) -Enumerationsmember festgelegt werden kann:

- `Data` –, das beim Anzeigen von Daten Einträgen verwendet werden soll. Beachten Sie, dass [ListView](~/xamarin-forms/user-interface/listview/index.md) möglicherweise eine bessere Option für den Bildlauf von Listen mit Daten ist.
- `Form` –, der verwendet wird, wenn die TableView als Formular fungiert.
- `Menu` –, die verwendet werden, wenn ein Menü der Auswahl angezeigt wird.
- `Settings` – für die Anzeige einer Liste von Konfigurationseinstellungen.

Der von Ihnen gewählte [`TableIntent`](xref:Xamarin.Forms.TableIntent) Wert kann sich darauf auswirken, wie der [`TableView`](xref:Xamarin.Forms.TableView) auf jeder Plattform angezeigt wird. Auch wenn es keine klaren Unterschiede gibt, empfiehlt es sich, die `TableIntent` auszuwählen, die am ehesten mit der Verwendung der Tabelle übereinstimmt.

Außerdem kann die Farbe des Texts, der für jede [`TableSection`](xref:Xamarin.Forms.TableSection) angezeigt wird, geändert werden, indem die `TextColor`-Eigenschaft auf einen [`Color`](xref:Xamarin.Forms.Color)festgelegt wird.

<a name="Built-In_Cells" />

## <a name="built-in-cells"></a>Integrierte Zellen

Xamarin. Forms verfügt über integrierte Zellen zum Erfassen und Anzeigen von Informationen. Obwohl [`ListView`](xref:Xamarin.Forms.ListView) und [`TableView`](xref:Xamarin.Forms.TableView) alle gleichen Zellen verwenden können, sind [`SwitchCell`](xref:Xamarin.Forms.SwitchCell) und [`EntryCell`](xref:Xamarin.Forms.EntryCell) für ein `TableView` Szenario am relevantesten.

Eine ausführliche Beschreibung von [textcell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#textcell) und [ImageCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#imagecell)finden Sie unter [ListView Cell Appearance](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md) .

<a name="switchcell" />

### <a name="switchcell"></a>Switchcell

[`SwitchCell`](xref:Xamarin.Forms.SwitchCell) ist das Steuerelement, das zum darstellen und Erfassen eines on/off-oder `true` / `false` State verwendet werden kann. Sie definiert die folgenden Eigenschaften:

- `Text` – Text ein, der neben dem Switch angezeigt werden soll.
- `On` –, ob der Schalter als ein-oder ausgeschaltet angezeigt wird.
- `OnColor` – die [`Color`](xref:Xamarin.Forms.Color) des Schalters, wenn er sich in der Position an befindet.

Alle diese Eigenschaften sind bindbar.

[`SwitchCell`](xref:Xamarin.Forms.SwitchCell) macht auch das `OnChanged` Ereignis verfügbar, sodass Sie auf Änderungen im Zustand der Zelle reagieren können.

![Switchcell-Beispiel](tableview-images/switch-cell.png)

<a name="entrycell" />

### <a name="entrycell"></a>Entrycell

[`EntryCell`](xref:Xamarin.Forms.EntryCell) ist nützlich, wenn Sie Textdaten anzeigen müssen, die der Benutzer bearbeiten kann. Sie definiert die folgenden Eigenschaften:

- `Keyboard` – die Tastatur, die während der Bearbeitung angezeigt werden soll. Es gibt Optionen, wie z. b. numerische Werte, e-Mail, Telefonnummern usw., finden Sie in [der API](xref:Xamarin.Forms.Keyboard)-Dokumentation.
- `Label` – der Bezeichnungs Text, der auf der linken Seite des Texteingabe Felds angezeigt werden soll.
- `LabelColor` – die Farbe des Beschriftungs Texts.
- `Placeholder` – Text, der im Eingabefeld angezeigt werden soll, wenn er NULL oder leer ist. Dieser Text verschwindet, wenn der Text Eintrag beginnt.
- `Text` – der Text im Eingabefeld.
- `HorizontalTextAlignment` – die horizontale Ausrichtung des Texts. Werte sind zentriert, Links oder rechtsbündig ausgerichtet. [Weitere Informationen finden Sie unter API docs](xref:Xamarin.Forms.TextAlignment).
- `VerticalTextAlignment` – die vertikale Ausrichtung des Texts. Werte sind `Start`, `Center` oder `End`.

[`EntryCell`](xref:Xamarin.Forms.EntryCell) macht auch das `Completed`-Ereignis verfügbar, das ausgelöst wird, wenn der Benutzer beim Bearbeiten von Text auf der Tastatur auf die Schaltfläche "Done" klickt.

![Entrycell-Beispiel](tableview-images/entry-cell.png)

<a name="Custom_Cells" />

## <a name="custom-cells"></a>Benutzerdefinierte Zellen

Wenn die integrierten Zellen nicht ausreichen, können benutzerdefinierte Zellen verwendet werden, um Daten auf die Weise darzustellen und aufzuzeichnen, die für Ihre APP sinnvoll ist. Sie können z. b. einen Schieberegler darstellen, um Benutzern die Möglichkeit zu geben, die Deckkraft eines Bilds auszuwählen.

Alle benutzerdefinierten Zellen müssen von [`ViewCell`](xref:Xamarin.Forms.ViewCell)abgeleitet werden, der gleichen Basisklasse, die von allen integrierten Zelltypen verwendet wird.

Dies ist ein Beispiel für eine benutzerdefinierte Zelle:

![Beispiel für benutzerdefinierte Zelle](tableview-images/custom-cell.png)

Das folgende Beispiel zeigt den XAML-Code, der zum Erstellen der [`TableView`](xref:Xamarin.Forms.TableView) in den obigen Screenshots verwendet wird:

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

Das Stamm Element unter dem [`TableView`](xref:Xamarin.Forms.TableView) ist der [`TableRoot`](xref:Xamarin.Forms.TableRoot), und es gibt direkt unterhalb der `TableRoot` eine [`TableSection`](xref:Xamarin.Forms.TableSection) . Die [`ViewCell`](xref:Xamarin.Forms.ViewCell) wird direkt unter dem `TableSection` definiert, und ein [`StackLayout`](xref:Xamarin.Forms.StackLayout) wird verwendet, um das Layout der benutzerdefinierten Zelle zu verwalten, obwohl jedes Layout hier verwendet werden kann.

> [!NOTE]
> Anders als bei [`ListView`](xref:Xamarin.Forms.ListView)ist es [`TableView`](xref:Xamarin.Forms.TableView) nicht erforderlich, dass benutzerdefinierte (oder irgendwelche) Zellen in einem `ItemTemplate` definiert werden.

## <a name="row-height"></a>Zeilenhöhe

Die [`TableView`](xref:Xamarin.Forms.TableView) -Klasse verfügt über zwei Eigenschaften, die verwendet werden können, um die Zeilenhöhe von Zellen zu ändern:

- [`RowHeight`](xref:Xamarin.Forms.TableView.RowHeight) – legt die Höhe der einzelnen Zeilen auf einen `int` fest.
- [`HasUnevenRows`](xref:Xamarin.Forms.TableView.HasUnevenRows) – Zeilen haben unterschiedliche Höhen, wenn diese auf `true` festgelegt sind. Beachten Sie, dass die Zeilenhöhe beim Festlegen dieser Eigenschaft auf `true` automatisch von xamarin. Forms berechnet und angewendet wird.

Wenn die Höhe des Inhalts in einer Zelle in einem [`TableView`](xref:Xamarin.Forms.TableView) geändert wird, wird die Zeilenhöhe implizit auf Android und der universelle Windows-Plattform (UWP) aktualisiert. Allerdings muss für IOS die Aktualisierung erzwungen werden, indem die [`HasUnevenRows`](xref:Xamarin.Forms.TableView.HasUnevenRows) -Eigenschaft auf `true` und durch Aufrufen der [`Cell.ForceUpdateSize`](xref:Xamarin.Forms.Cell.ForceUpdateSize) -Methode festgelegt wird.

Das folgende XAML-Beispiel zeigt eine [`TableView`](xref:Xamarin.Forms.TableView) , die eine [`ViewCell`](xref:Xamarin.Forms.ViewCell)enthält:

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

Wenn die [`ViewCell`](xref:Xamarin.Forms.ViewCell) getippt wird, wird der `OnViewCellTapped`-Ereignishandler ausgeführt:

```csharp
void OnViewCellTapped(object sender, EventArgs e)
{
    _target.IsVisible = !_target.IsVisible;
    _viewCell.ForceUpdateSize();
}
```

Der `OnViewCellTapped` Ereignishandler zeigt den zweiten [`Label`](xref:Xamarin.Forms.Label) im [`ViewCell`](xref:Xamarin.Forms.ViewCell)an oder blendet ihn aus und aktualisiert die Größe der Zelle explizit durch Aufrufen der [`Cell.ForceUpdateSize`](xref:Xamarin.Forms.Cell.ForceUpdateSize) -Methode.

Die folgenden Screenshots zeigen die Zelle vor dem Tippen auf:

![Viewcell vor der Größenänderung](tableview-images/cell-beforeresize.png)

Die folgenden Screenshots zeigen die Zelle nach dem Tippen auf:

![Viewcell nach der Größenänderung](tableview-images/cell-afterresize.png)

> [!IMPORTANT]
> Wenn diese Funktion überlastet ist, besteht eine hohe Wahrscheinlichkeit einer Leistungsminderung.

## <a name="related-links"></a>Verwandte Links

- [TableView (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-tableview)
