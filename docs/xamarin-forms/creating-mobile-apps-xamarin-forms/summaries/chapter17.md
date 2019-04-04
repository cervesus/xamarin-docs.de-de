---
title: Zusammenfassung der Kapitel 17. Optimieren des Rasters
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung der Kapitel 17. Optimieren des Rasters'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 71EDEF9C-4220-4D2E-A235-43F1EC8746C1
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 3aaf8e9d1eb8e0d98ad32a6b5a1286f14c7bb906
ms.sourcegitcommit: 495680e74c72e7c570e68cde95d3d3643b1fcc8a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2019
ms.locfileid: "58869987"
---
# <a name="summary-of-chapter-17-mastering-the-grid"></a>Zusammenfassung der Kapitel 17. Optimieren des Rasters

[![Downloadliste Beispiel](~/media/shared/download.png) Herunterladen des Beispiels](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17)

Die [ `Grid` ](xref:Xamarin.Forms.Grid) ist ein leistungsstarke Layout-Mechanismus, der ordnet seine untergeordneten Elemente in Zeilen und Spalten von Zellen. Im Gegensatz zu HTML-Code ähnlich `table` -Element, das `Grid` ist ausschließlich für Zwecke der Präsentation, anstatt Layout.

## <a name="the-basic-grid"></a>Einfaches Raster

`Grid` leitet sich von [ `Layout<View>` ](xref:Xamarin.Forms.Layout`1), definiert ein [ `Children` ](xref:Xamarin.Forms.Layout`1.Children) Eigenschaft, die `Grid` erbt. Sie können diese Sammlung in XAML oder Code eingeben.

### <a name="the-grid-in-xaml"></a>Das Raster in XAML

Die Definition von einer `Grid` in XAML beginnt gewöhnlich mit füllen die [ `RowDefinitions` ](xref:Xamarin.Forms.Grid.RowDefinitions) und [ `ColumnDefinitions` ](xref:Xamarin.Forms.Grid.ColumnDefinitions) Sammlungen von der `Grid` mit [ `RowDefinition` ](xref:Xamarin.Forms.RowDefinition) und [ `ColumnDefinition` ](xref:Xamarin.Forms.ColumnDefinition) Objekte. Dies ist, wie Sie die Anzahl der Zeilen und Spalten des Einrichten der `Grid`, und deren Eigenschaften.

`RowDefinition` verfügt über eine [ `Height` ](xref:Xamarin.Forms.RowDefinition.Height) Eigenschaft und `ColumnDefinition` verfügt über eine [ `Width` ](xref:Xamarin.Forms.ColumnDefinition.Width) -Eigenschaft, beide vom Typ [ `GridLength` ](xref:Xamarin.Forms.GridLength), eine Struktur.

In XAML die [ `GridLengthTypeConverter` ](xref:Xamarin.Forms.GridLengthTypeConverter) konvertiert einfache Textzeichenfolgen in `GridLength` Werte. Hinter den Kulissen der [ `GridLength` Konstruktor](xref:Xamarin.Forms.GridLength.%23ctor(System.Double,Xamarin.Forms.GridUnitType)) erstellt die `GridLength` -Wert auf Grundlage einer Zahl und ein Wert vom Typ [ `GridUnitType` ](xref:Xamarin.Forms.GridUnitType), eine Enumeration mit drei Elementen:

- [`Absolute`](xref:Xamarin.Forms.GridUnitType.Absolute) &mdash; die Breite oder Höhe wird in geräteunabhängigen Einheiten (eine Zahl in XAML) angegeben.
- [`Auto`](xref:Xamarin.Forms.GridUnitType.Auto) &mdash; die Höhe oder Breite ist automatisch angepasst, die basierend auf den Inhalt der Zelle (in XAML "Auto")
- [`Star`](xref:Xamarin.Forms.GridUnitType.Star) &mdash; übrig gebliebene Höhe oder Breite proportional zugewiesen ist (eine Zahl mit "\*" namens *Stern*, in XAML)

Jedes untergeordnete Element von der `Grid` muss auch zugewiesen werden einer Zeile und Spalte (entweder explizit oder implizit). Umfasst Zeile und Spalte Span-Elemente sind optional. Diese werden alle mit angegeben angefügte bindbare Eigenschaften &mdash; von definierten Eigenschaften der `Grid` jedoch festgelegt, auf die untergeordneten Elemente des der `Grid`. `Grid` definiert vier statische angefügte bindbare Eigenschaften an:

- [`RowProperty`](xref:Xamarin.Forms.Grid.RowProperty) &mdash; Der nullbasierte Zeile. Der Standardwert ist 0
- [`ColumnProperty`](xref:Xamarin.Forms.Grid.ColumnProperty) &mdash; Der nullbasierte Spalte aus. Der Standardwert ist 0
- [`RowSpanProperty`](xref:Xamarin.Forms.Grid.RowSpanProperty) &mdash; die Anzahl der Zeilen der untergeordneten umfasst; Der Standardwert ist 1
- [`ColumnSpanProperty`](xref:Xamarin.Forms.Grid.ColumnSpanProperty) &mdash; die Anzahl der Spalten das untergeordnete Element erstreckt; Der Standardwert ist 1

Im Code kann ein Programm acht statische Methoden zum Festlegen und Abrufen dieser Werte verwenden:

- [`Grid.SetRow`](xref:Xamarin.Forms.Grid.SetRow(Xamarin.Forms.BindableObject,System.Int32)) und [`Grid.GetRow`](xref:Xamarin.Forms.Grid.GetRow(Xamarin.Forms.BindableObject))
- [`Grid.SetColumn`](xref:Xamarin.Forms.Grid.SetColumn(Xamarin.Forms.BindableObject,System.Int32)) und [`Grid.GetColumn`](xref:Xamarin.Forms.Grid.GetColumn(Xamarin.Forms.BindableObject))
- [`Grid.SetRowSpan`](xref:Xamarin.Forms.Grid.SetRowSpan(Xamarin.Forms.BindableObject,System.Int32)) und [`Grid.GetRowSpan`](xref:Xamarin.Forms.Grid.GetRowSpan(Xamarin.Forms.BindableObject))
- [`Grid.SetColumnSpan`](xref:Xamarin.Forms.Grid.SetColumnSpan(Xamarin.Forms.BindableObject,System.Int32)) und [`Grid.GetColumnSpan`](xref:Xamarin.Forms.Grid.GetColumnSpan(Xamarin.Forms.BindableObject))

In XAML verwenden Sie die folgenden Attribute zum Festlegen dieser Werte an:

- `Grid.Row`
- `Grid.Column`
- `Grid.RowSpan`
- `Grid.ColumnSpan`

Die [ **SimpleGridDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SimpleGridDemo) Beispiel veranschaulicht das Erstellen und initialisieren eine `Grid` in XAML.

Die `Grid` erbt die [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) Eigenschaft `Layout` und definiert zwei weitere Eigenschaften, die Abstände zwischen den Zeilen und Spalten zu ermöglichen:

- [`RowSpacing`](xref:Xamarin.Forms.Grid.RowSpacing) hat den Standardwert von 6
- [`ColumnSpacing`](xref:Xamarin.Forms.Grid.ColumnSpacing) hat den Standardwert von 6

Die `RowDefinitions` und `ColumnDefinitions` Auflistungen sind nicht unbedingt erforderlich. Falls nicht vorhanden, die `Grid` erstellt, Zeilen und Spalten für die `Grid` untergeordnete Elemente und gibt alle den Standardwert `GridLength` von "\*" (star).

### <a name="the-grid-in-code"></a>Das Raster im code

Die [ **GridCodeDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCodeDemo) Beispiel veranschaulicht, wie zum Erstellen und Auffüllen einer `Grid` im Code. Sie können die angefügten Eigenschaften festlegen, für jedes untergeordnete Element direkt oder indirekt durch Aufrufen von zusätzlichen `Add` Methoden, z. B. [ `Add` ](xref:Xamarin.Forms.Grid.IGridList`1.Add*) von definiert die [Grid.IGridList<T> ](xref:Xamarin.Forms.Grid.IGridList`1) -Schnittstelle.

### <a name="the-grid-bar-chart"></a>Das Raster-Balkendiagramm

Die [ **GridBarChart** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridBarChart) Beispiel zeigt, wie mehrere hinzuzufügende `BoxView` Elementen, die eine `Grid` mit der massenbearbeitungsfunktion [ `AddHorizontal` ](xref:Xamarin.Forms.Grid.IGridList`1.AddHorizontal*) Methode. Standardmäßig diese `BoxView` Elemente haben dieselbe Breite. Die Höhe der einzelnen `BoxView` kann dann gesteuert werden, um ein Balkendiagramm zu entsprechen.

Die `Grid` in die **GridBarChart** Freigaben Beispiel einer `AbsoluteLayout` übergeordnetes Element mit einem anfänglich unsichtbar `Frame`. Legt auch fest, das Programm eine `TapGestureRecognizer` auf jedem `BoxView` verwenden die `Frame` Informationen zu der diese Leiste angezeigt.

### <a name="alignment-in-the-grid"></a>Ausrichtung im Raster

Die [ **GridAlignment** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridAlignment) Beispiel veranschaulicht, wie die `VerticalOptions` und `HorizontalOptions` Eigenschaften an das untergeordnete Elemente in einem `Grid` Zelle.

Die [ **SpacingButtons** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SpacingButtons) Beispiel gleichermaßen Leerzeichen `Button` Elemente, der zentriert in `Grid` Zellen.

### <a name="cell-dividers-and-borders"></a>Zelle Zeilenunterteiler und Rahmen

Die `Grid` schließt sich nicht auf eine Funktion, die Zelle Zeilenunterteiler oder Rahmen zeichnet. Allerdings können Sie Ihre eigenen vornehmen.

Die [ **GridCellDividers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellDividers) wird veranschaulicht, wie zusätzliche Zeilen und Spalten speziell für die schlanke definieren `BoxView` Elementen, die Trennlinien zu imitieren.

Die [ **GridCellBorders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellBorders) Programm keine zusätzlichen Zellen erstellt, sondern stattdessen richtet `BoxView` Elemente in jeder Zelle, um einen Zellenrahmen zu imitieren.

## <a name="almost-real-life-grid-examples"></a>Fast realen Grid-Beispiele

Die [ **KeypadGrid** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/KeypadGrid) -Beispiel verwendet einen `Grid` eine Zehnertastatur angezeigt:

[![Dreifacher Screenshot der Zehnertastatur Raster](images/ch17fg12-small.png "Zehnertastatur Raster")](images/ch17fg12-large.png#lightbox "Zehnertastatur Raster")

### <a name="responding-to-orientation-changes"></a>Reagieren auf Änderungen der bildschirmausrichtung

Die `Grid` können die Struktur eines Programms auf Richtungswechsel reagiert. Die [ **GridRgbSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridRgbSliders) Beispiel wird veranschaulicht, eine Technik, die eine zweite Zeile eines Telefons Hochformat und die zweite Spalte oder einem Telefon Querformat ein Element bewegt.

Initialisiert das Programm `Slider` Elemente auf einen Bereich von 0 bis 255 und datenbindungen verwendet, um den Wert der Schieberegler im Hexadezimalformat anzuzeigen. Da die `Slider` Werte sind Gleitkomma-, und der Formatierungszeichenfolge für hexadezimale nur mit ganzen Zahlen funktioniert, .NET eine [ `DoubleToIntConvert` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DoubleToIntConverter.cs) -Klasse in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Bibliothek bietet.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 17 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch17-Apr2016.pdf)
- [Kapitel 17-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17)
- [Raster](~/xamarin-forms/user-interface/layouts/grid.md)
