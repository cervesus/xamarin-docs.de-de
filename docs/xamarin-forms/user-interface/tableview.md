---
title: Xamarin.Forms TableView
description: In diesem Artikel wird erläutert, wie die Xamarin.Forms TableView-Klasse, die zum Darstellen von fortlaufenden Menüs, Einstellungen und als Eingabeformulare in Anwendungen verwendet werden.
ms.prod: xamarin
ms.assetid: D1619D19-A74F-40DF-8E53-B1B7DFF7A3FB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 5ad1db6a073b5a6d0199aa586230cb55a9d4a925
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35244857"
---
# <a name="xamarinforms-tableview"></a>Xamarin.Forms TableView

[TableView](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) ist eine Ansicht zum Anzeigen von Daten oder Auswahlmöglichkeiten bildlauffähiger Listen, die die gleiche Vorlage gemeinsam nutzen, keine Zeilen vorhanden sind. Im Gegensatz zu [ListView](~/xamarin-forms/user-interface/listview/index.md), TableView verfügt nicht über das Konzept einer `ItemsSource`, daher Elemente als untergeordnete Elemente manuell hinzugefügt werden müssen.

Dieses Handbuch besteht aus den folgenden Abschnitten:

- **[Anwendungsfälle](#Use_Cases)**  &ndash; TableView anstelle von ListView oder eine benutzerdefinierte Ansicht verwenden.
- **[TableView Struktur](#TableView_Structure)**  &ndash; die Hierarchie der Ansichten, die innerhalb einer TableView erforderlich ist.
- **[TableView Darstellung](#TableView_Appearance)**  &ndash; die Anpassungsoptionen für TableView.
- **[Integrierte Zellen](#Built-In_Cells)**  &ndash; integrierte Zellenoptionen, einschließlich [EntryCell](#EntryCell) und [SwitchCell](#SwitchCell).
- **[Benutzerdefinierte Zellen](#Custom_Cells)**  &ndash; wie eigene benutzerdefinierte Zellen zu machen.

![](tableview-images/tableview-all-sml.png "TableView-Beispiel")

<a name="Use_Cases" />

## <a name="use-cases"></a>Anwendungsfälle

TableView ist hilfreich, wenn:

- eine geordnete Liste der Einstellungen,
- Sammeln von Daten in einem Formular oder
- Anzeigen von Daten, die Zeile (z. B. Zahlen, Prozentsätze und Bilder) anders aus Zeile angezeigt werden.

Durchführen eines Bildlaufs und das Layout von Zeilen in attraktive Abschnitten allgemeine müssen für den oben genannten Szenarien TableView verarbeitet werden. Die `TableView` Steuerelement verwendet jede Plattform entsprechende Ansicht, soweit verfügbar, erstellen eine systemeigene Darstellung für jede Plattform zugrunde liegen.

<a name="TableView_Structure" />

## <a name="tableview-structure"></a>TableView-Struktur

Elemente in einem `TableView` in Abschnitte unterteilt sind. Im Stammverzeichnis der `TableView` ist die `TableRoot`, also in einer oder mehreren übergeordneten `TableSections`:

```csharp
Content = new TableView {
    Root = new TableRoot {
        new TableSection...
    },
    Intent = TableIntent.Settings
};
```

Jede `TableSection` besteht aus einer Überschrift und eine oder mehrere ViewCells. Hier sehen wir die `TableSection`des `Title` -Eigenschaftensatz auf *"Ring"* im Konstruktor:

```csharp
var section = new TableSection ("Ring") { //TableSection constructor takes title as an optional parameter
    new SwitchCell {Text = "New Voice Mail"},
    new SwitchCell {Text = "New Mail", On = true}
};
```

Erreichen Sie dasselbe Layout wie oben beschrieben in XAML folgendermaßen:

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

<a name="TableView_Appearance" />

## <a name="tableview-appearance"></a>TableView Darstellung

TableView macht die `Intent` Eigenschaft, die eine Enumeration der folgenden Optionen:

- **Daten** &ndash; für die Verwendung beim Dateneinträge anzeigen. Beachten Sie, dass [ListView](~/xamarin-forms/user-interface/listview/index.md) möglicherweise eine bessere Option zum Durchführen eines Bildlaufs Listen der Daten.
- **Formular** &ndash; für die Verwendung bei der TableView als ein Formular dient.
- **Menü** &ndash; für wird verwendet, wenn ein Menü der Auswahl zu präsentieren.
- **Einstellungen** &ndash; für die Verwendung bei der Anzeige einer Liste von Konfigurationseinstellungen.

Die `TableIntent` gewünscht beeinträchtigt werden kann wie die `TableView` auf jeder Plattform wird angezeigt. Auch wenn es Unterschiede nicht deaktivieren gibt, ist es eine bewährte Methode, wählen Sie die `TableIntent` , die am ehesten entspricht, wie Sie die Tabelle verwenden möchten.

<a name="Built-In_Cells" />

## <a name="built-in-cells"></a>Integrierte Zellen

Xamarin.Forms bietet integrierte Zellen für das Sammeln und Anzeigen von Informationen. Obwohl alle den gleichen Zellen ListView-Steuerelement und TableView verwenden können, lauten wie folgt den wichtigsten Faktor für ein Szenario TableView:

- **SwitchCell** &ndash; für die Präsentation und einen wahr/falsch-Zustand zusammen mit einer textbezeichnung erfassen.
- **EntryCell** &ndash; für die Präsentation und Aufzeichnen von Text.

Finden Sie unter [ListView Zelle Darstellung](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md) für eine ausführliche Beschreibung der [TextCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#TextCell) und [ImageCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#ImageCell).

<a name="switchcell" />

### <a name="switchcell"></a>SwitchCell
[`SwitchCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/) ist das Steuerelement für die Präsentation und erfassen verwenden ein/aus oder `true` / `false` Zustand.

SwitchCells haben eine Zeile der zu bearbeitenden Text und eine Eigenschaft ein-oder auszuschalten. Diese beiden Eigenschaften sind bindbar.

- `Text` &ndash; neben den Switch anzuzeigende Text.
- `On` &ndash; Gibt an, ob der Schalter als on oder off angezeigt wird.

Beachten Sie, dass die `SwitchCell` macht die `OnChanged` Ereignis, sodass Sie für die Reaktion auf Änderungen in den Status der Zelle.

![](tableview-images/switch-cell.png "SwitchCell-Beispiel")

<a name="entrycell" />

### <a name="entrycell"></a>EntryCell
[`EntryCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/) ist nützlich, wenn es sich bei müssen Sie die Textdaten anzuzeigen, die der Benutzer nicht bearbeiten kann. `EntryCell`s bieten die folgenden Eigenschaften, die angepasst werden können:

- `Keyboard` &ndash; Der Tastatur, um während der Bearbeitung angezeigt werden soll. Es gibt Optionen für Angaben wie numerische Werte, e-Mail-, Telefonnummern usw. ein. [Finden Sie unter dem API-Dokumentation](http://developer.xamarin.com/api/type/Xamarin.Forms.Keyboard/).
- `Label` &ndash; Der Bezeichnungstext, die rechts neben dem Texteingabefeld angezeigt.
- `LabelColor` &ndash; Die Farbe des Texts Bezeichnung.
- `Placeholder` &ndash; Text, der im Eingabefeld angezeigt wird, wenn der Wert null oder leer ist. Dieser Text wird ausgeblendet, wenn die Eingabe von Text beginnt.
- `Text` &ndash; Der Text im Eingabefeld.
- `HorizontalTextAlignment` &ndash; Die horizontale Ausrichtung des Texts. Center, nach links oder rechts kann ausgerichtet werden. [Finden Sie unter dem API-Dokumentation](http://developer.xamarin.com/api/type/Xamarin.Forms.TextAlignment/).

Beachten Sie, dass `EntryCell` macht die `Completed` Ereignis, das ausgelöst wird, wenn der Benutzer 'done' auf der Tastatur drückt, während der Bearbeitung von Text.

![](tableview-images/entry-cell.png "EntryCell-Beispiel")

<a name="Custom_Cells" />

## <a name="custom-cells"></a>Benutzerdefinierte Zellen
Wenn die integrierte Zellen nicht ausreichend sind, können benutzerdefinierte Zellen und Erfassen von Daten in der Weise, die sinnvoll für Ihre app verwendet werden. Beispielsweise empfiehlt es sich um einen Schieberegler, um einem Benutzer ermöglichen, wählen die Deckkraft eines Bilds darzustellen.

Alle benutzerdefinierte Zellen abgeleitet müssen [ `ViewCell` ](http://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/), die dieselbe Basisklasse an, dass alle integrierten Zelle Typen verwenden.

Dies ist ein Beispiel für eine benutzerdefinierte Zelle:

![](tableview-images/custom-cell.png "Beispiel für benutzerdefinierte Zelle")

### <a name="xaml"></a>XAML
Der XAML-Code, der oben genannten Layout erstellt wird, lautet wie folgt:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="DemoTableView.TablePage" Title="TableView">
    <ContentPage.Content>
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
    </ContentPage.Content>
</ContentPage>

```

Die obige XAML-Code ist deutlich machen. Schön:

- Der Root-Element unter den `TableView` ist die `TableRoot`.
- Es ist ein `TableSection` unmittelbar unterhalb der `TableRoot`.
- Die `ViewCell` direkt unter der Tabellenabschnitt definiert ist. Im Gegensatz zu `ListView`, `TableView` erfordert keine dieser benutzerdefinierten (oder alle) Zellen werden definiert, eine `ItemTemplate`.
- Eine StackLayout wird verwendet, um das Layout der benutzerdefinierten Zelle zu verwalten. Jedes Layout kann hier verwendet werden.

### <a name="cnum"></a>C&num;

Da `TableView` funktioniert mit statischen Daten oder Daten, die manuell geändert werden, verfügt es nicht über das Konzept einer Elementvorlage. Stattdessen können benutzerdefinierte Zellen manuell erstellt und werden in die Tabelle eingefügt. Beachten Sie, dass das Verfahren zum Erstellen einer benutzerdefiniertes, die Zelle erbt von `ViewCell`, und klicken Sie dann zum Hinzufügen der `TableView` würden Sie wie Sie eine integrierte Zelle, wird ebenfalls unterstützt.
Hier ist der c#-Code um das Layout, die oben genannten zu erreichen:

```csharp
var table = new TableView();
table.Intent = TableIntent.Settings;
var layout = new StackLayout() { Orientation = StackOrientation.Horizontal };
layout.Children.Add (new Image() {Source = "bulb.png"});
layout.Children.Add (new Label() {
    Text = "left",
    TextColor = Color.FromHex("#f35e20"),
    VerticalOptions = LayoutOptions.Center
});
layout.Children.Add (new Label () {
    Text = "right",
    TextColor = Color.FromHex ("#503026"),
    VerticalOptions = LayoutOptions.Center,
    HorizontalOptions = LayoutOptions.EndAndExpand
});
table.Root = new TableRoot () {
    new TableSection("Getting Started") {
        new ViewCell() {View = layout}
    }
};

Content = table;
```

Die C#-oben ist deutlich machen. Schön:

- Der Root-Element unter den `TableView` ist die `TableRoot`.
- Es ist ein `TableSection` unmittelbar unterhalb der `TableRoot`.
- Die `ViewCell` direkt unter der Tabellenabschnitt definiert ist. Im Gegensatz zu `ListView`, `TableView` erfordert keine dieser benutzerdefinierten (oder alle) Zellen werden definiert, eine `ItemTemplate`.
- Eine StackLayout wird verwendet, um das Layout der benutzerdefinierten Zelle zu verwalten. Jedes Layout kann hier verwendet werden.

Beachten Sie, dass eine Klasse für die benutzerdefinierte Zelle nie definiert ist. Stattdessen die `ViewCell`des View-Eigenschaft wird festgelegt, für eine bestimmte Instanz des `ViewCell`.



## <a name="related-links"></a>Verwandte Links

- [TableView (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TableView)
