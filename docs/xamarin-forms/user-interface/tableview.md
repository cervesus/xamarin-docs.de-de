---
title: Xamarin.Forms TableView
description: In diesem Artikel wird erläutert, wie die Xamarin.Forms TableView-Klasse, die zur Darstellung von Bildlaufmenüs, Einstellungen und Eingabeformulare in Anwendungen verwendet wird.
ms.prod: xamarin
ms.assetid: D1619D19-A74F-40DF-8E53-B1B7DFF7A3FB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 47cd79611cfeaf48c0422772d8f3e75eb57ba771
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996052"
---
# <a name="xamarinforms-tableview"></a>Xamarin.Forms TableView

[TableView](xref:Xamarin.Forms.TableView) ist eine Ansicht zum Anzeigen von bildlauffähigen Listen mit Daten oder Ihren Optionen angezeigt, die Zeilen, die die gleiche Vorlage gemeinsam verwenden, nicht vorhanden sind. Im Gegensatz zu [ListView](~/xamarin-forms/user-interface/listview/index.md), TableView verfügt nicht über das Konzept einer `ItemsSource`, sodass Elemente als untergeordnete Elemente manuell hinzugefügt werden müssen.

Dieses Handbuch besteht aus den folgenden Abschnitten:

- **[Anwendungsfälle](#Use_Cases)**  &ndash; TableView anstelle von ListView oder eine benutzerdefinierte Ansicht zu verwenden.
- **[TableView Struktur](#TableView_Structure)**  &ndash; der Hierarchie von Ansichten, die innerhalb einer TableView erforderlich ist.
- **[TableView Darstellung](#TableView_Appearance)**  &ndash; die Anpassungsoptionen für TableView.
- **[Integrierte Zellen](#Built-In_Cells)**  &ndash; integrierte Zellenoptionen, einschließlich [EntryCell](#EntryCell) und [SwitchCell](#SwitchCell).
- **[Benutzerdefinierte Zellen](#Custom_Cells)**  &ndash; wie Sie Ihre eigenen benutzerdefinierten Zellen zu machen.

![](tableview-images/tableview-all-sml.png "TableView-Beispiel")

<a name="Use_Cases" />

## <a name="use-cases"></a>Anwendungsfälle

TableView ist nützlich, wenn:

- eine geordnete Liste der Einstellungen,
- Sammeln von Daten in einem Formular oder
- Anzeigen von Daten, unterschiedlich aus Zeile die Zeile (z. B. Zahlen, Prozentsätze und Bilder) angezeigt werden.

TableView behandelt, Durchführen eines Bildlaufs und Anordnung der Zeilen in eine allgemeine Voraussetzung für die oben genannten Szenarien attraktive Abschnitten. Die `TableView` -Steuerelement verwendet jede Plattform des zugrunde liegenden entsprechende anzeigen, sofern verfügbar, beim Erstellen eines einheitlichen Erscheinungsbilds für jede Plattform.

<a name="TableView_Structure" />

## <a name="tableview-structure"></a>TableView-Struktur

Elemente in einem `TableView` sind in Abschnitte unterteilt. Im Stammverzeichnis der `TableView` ist die `TableRoot`, d.h. übergeordneten an einen oder mehrere `TableSections`:

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

Um das gleiche Layout wie oben beschrieben in XAML zu erreichen:

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

- **Daten** &ndash; für die Verwendung beim Dateneinträge anzeigen. Beachten Sie, dass [ListView](~/xamarin-forms/user-interface/listview/index.md) möglicherweise eine bessere Option für den Bildlauf von Listen mit Daten.
- **Formular** &ndash; für die Verwendung bei der TableView als Formular dient.
- **Menü** &ndash; für die Verwendung bei der ein Menü der Auswahl zu präsentieren.
- **Einstellungen** &ndash; bei der Anzeige einer Liste von Konfigurationseinstellungen verwendet.

Die `TableIntent` auswirken können Sie auswählen, wie die `TableView` auf jeder Plattform angezeigt wird. Auch wenn Sie vorhanden sind, deaktivieren die Unterschiede nicht, es ist eine bewährte Methode, wählen Sie die `TableIntent` , die am ehesten entspricht, wie Sie in der Tabelle verwenden möchten.

<a name="Built-In_Cells" />

## <a name="built-in-cells"></a>Integrierte Zellen

Xamarin.Forms enthält integrierte Zellen für das Sammeln und Anzeigen von Informationen. Obwohl alle den gleichen Zellen ListView und TableView verwenden können, lauten wie folgt den wichtigsten Faktor für ein Szenario TableView:

- **SwitchCell** &ndash; zum präsentieren und einen True/False-Status, zusammen mit einer textbezeichnung zu erfassen.
- **EntryCell** &ndash; zum präsentieren und Erfassen von Text.

Finden Sie unter [Darstellung des ListView-Zelle](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md) für eine ausführliche Beschreibung der [TextCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#TextCell) und [ImageCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#ImageCell).

<a name="switchcell" />

### <a name="switchcell"></a>SwitchCell
[`SwitchCell`](xref:Xamarin.Forms.SwitchCell) ist das Steuerelement zum Bereitstellen und erfassen ein/aus oder `true` / `false` Zustand.

SwitchCells haben eine Zeile der zu bearbeitenden Text und eine Eigenschaft aktivieren/deaktivieren. Diese beiden Eigenschaften sind bindbar.

- `Text` &ndash; neben den Switch anzuzeigende Text.
- `On` &ndash; Gibt an, ob der Schalter als on oder off angezeigt wird.

Beachten Sie, dass die `SwitchCell` macht die `OnChanged` -Ereignis, sodass Sie die Reaktion auf Änderungen in den Status der Zelle.

![](tableview-images/switch-cell.png "SwitchCell-Beispiel")

<a name="entrycell" />

### <a name="entrycell"></a>EntryCell
[`EntryCell`](xref:Xamarin.Forms.EntryCell) ist nützlich, wenn Sie müssen zum Anzeigen von Textdaten, die der Benutzer bearbeiten können. `EntryCell`s bieten die folgenden Eigenschaften, die angepasst werden können:

- `Keyboard` &ndash; Der Tastatur, um während der Bearbeitung angezeigt werden soll. Es gibt Optionen für Dinge wie numerische Werte, e-Mail-Adresse, Telefonnummern usw. ein. [Finden Sie in der API-Dokumentation](xref:Xamarin.Forms.Keyboard).
- `Label` &ndash; Der Bezeichnungstext, die rechts neben dem Texteingabefeld angezeigt werden soll.
- `LabelColor` &ndash; Die Farbe des Bezeichnungstexts.
- `Placeholder` &ndash; Text, der im Eingabefeld angezeigt wird, wenn der Wert null oder leer ist. Dieser Text wird ausgeblendet, wenn der Eingabe von Text beginnt.
- `Text` &ndash; Der Text im Eingabefeld.
- `HorizontalTextAlignment` &ndash; Die horizontale Ausrichtung des Texts. Center, links oder rechts kann ausgerichtet werden. [Finden Sie in der API-Dokumentation](xref:Xamarin.Forms.TextAlignment).

Beachten Sie, dass `EntryCell` macht die `Completed` -Ereignis, das ausgelöst wird, wenn der Benutzer 'done' auf der Tastatur drückt, während der Bearbeitung von Text.

![](tableview-images/entry-cell.png "EntryCell-Beispiel")

<a name="Custom_Cells" />

## <a name="custom-cells"></a>Benutzerdefinierte Zellen
Wenn die integrierten Zellen nicht ausreichend sind, können benutzerdefinierte Zellen verwendet werden, und Erfassen von Daten in der Weise, die für Ihre app sinnvoll ist. Beispielsweise empfiehlt es sich um einen Schieberegler, um einem Benutzer ermöglichen, wählen Sie die Deckkraft eines Bilds darzustellen.

Alle benutzerdefinierte Zellen abgeleitet müssen [ `ViewCell` ](xref:Xamarin.Forms.ViewCell), der gleichen Basisklasse an, dass alle der Zelle integrierte Typen verwenden.

Dies ist ein Beispiel für eine benutzerdefinierte Zelle:

![](tableview-images/custom-cell.png "Beispiel für die benutzerdefinierte Zelle")

### <a name="xaml"></a>XAML
Das XAML das obige Layout erstellen, lautet wie folgt:

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

Der obige XAML macht viel. Wir unterteilen:

- Das Stammelement in der `TableView` ist die `TableRoot`.
- Es gibt eine `TableSection` direkt unter der `TableRoot`.
- Die `ViewCell` direkt unterhalb der Tabellenabschnitt definiert ist. Im Gegensatz zu `ListView`, `TableView` ist nicht erforderlich, diese benutzerdefinierten (oder einem) Zellen werden definiert, eine `ItemTemplate`.
- Einem StackLayout wird verwendet, um das Layout der die benutzerdefinierte Zelle zu verwalten. Jedes Layout kann hier verwendet werden.

### <a name="cnum"></a>C&num;

Da `TableView` funktioniert mit statischen Daten oder Daten, die manuell geändert werden, verfügt es nicht über das Konzept einer Elementvorlage. Stattdessen benutzerdefinierte Zellen können werden manuell erstellt, und fügen Sie in der Tabelle. Beachten Sie, dass das Verfahren zum Erstellen ein benutzerdefinierten, die Zelle erbt `ViewCell`, und klicken Sie dann zum Hinzufügen der `TableView` wie würden Sie eine integrierte Zelle, wird ebenfalls unterstützt.
Hier ist der c#-Code, um das Layout, die oben genannten auszuführen:

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

Die C# -Code über macht viel. Wir unterteilen:

- Das Stammelement in der `TableView` ist die `TableRoot`.
- Es gibt eine `TableSection` direkt unter der `TableRoot`.
- Die `ViewCell` direkt unterhalb der Tabellenabschnitt definiert ist. Im Gegensatz zu `ListView`, `TableView` ist nicht erforderlich, diese benutzerdefinierten (oder einem) Zellen werden definiert, eine `ItemTemplate`.
- Einem StackLayout wird verwendet, um das Layout der die benutzerdefinierte Zelle zu verwalten. Jedes Layout kann hier verwendet werden.

Beachten Sie, dass eine Klasse für die benutzerdefinierte Zelle nicht definiert ist. Stattdessen die `ViewCell`des View-Eigenschaft festgelegt ist, für eine bestimmte Instanz des `ViewCell`.



## <a name="related-links"></a>Verwandte Links

- [TableView (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TableView)
