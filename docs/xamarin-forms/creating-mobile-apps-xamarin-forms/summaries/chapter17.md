---
title: Zusammenfassung der Kapitel 17. Optimieren des Rasters
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung der Kapitel 17. Optimieren des Rasters'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 71EDEF9C-4220-4D2E-A235-43F1EC8746C1
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: b71859d0848d7bf790b3cc4beddc67a5ea86d340
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935477"
---
# <a name="summary-of-chapter-17-mastering-the-grid"></a>Zusammenfassung der Kapitel 17. Optimieren des Rasters

Die [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) ist ein leistungsstarke Layout-Mechanismus, der ordnet seine untergeordneten Elemente in Zeilen und Spalten von Zellen. Im Gegensatz zu HTML-Code ähnlich `table` -Element, das `Grid` ist ausschließlich für Zwecke der Präsentation, anstatt Layout.

## <a name="the-basic-grid"></a>Einfaches Raster

`Grid` leitet sich von [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/), definiert ein [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout%3CT%3E.Children/) Eigenschaft, die `Grid` erbt. Sie können diese Sammlung in XAML oder Code eingeben.

### <a name="the-grid-in-xaml"></a>Das Raster in XAML

Die Definition von einer `Grid` in XAML beginnt gewöhnlich mit füllen die [ `RowDefinitions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.RowDefinitions/) und [ `ColumnDefinitions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.ColumnDefinitions/) Sammlungen von der `Grid` mit [ `RowDefinition` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RowDefinition/) und [ `ColumnDefinition` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ColumnDefinition/) Objekte. Dies ist, wie Sie die Anzahl der Zeilen und Spalten des Einrichten der `Grid`, und deren Eigenschaften.

`RowDefinition` verfügt über eine [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.RowDefinition.Height/) Eigenschaft und `ColumnDefinition` verfügt über eine [ `Width` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ColumnDefinition.Width/) -Eigenschaft, beide vom Typ [ `GridLength` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridLength/), eine Struktur.

In XAML die [ `GridLengthTypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridLengthTypeConverter/) konvertiert einfache Textzeichenfolgen in `GridLength` Werte. Hinter den Kulissen der [ `GridLength` Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.GridLength.GridLength/p/System.Double/Xamarin.Forms.GridUnitType/) erstellt die `GridLength` -Wert auf Grundlage einer Zahl und ein Wert vom Typ [ `GridUnitType` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridUnitType/), eine Enumeration mit drei Elementen:

- [`Absolute`](xref:Xamarin.Forms.GridUnitType.Absolute) &mdash; die Breite oder Höhe wird in geräteunabhängigen Einheiten (eine Zahl in XAML) angegeben.
- [`Auto`](xref:Xamarin.Forms.GridUnitType.Auto) &mdash; die Höhe oder Breite ist automatisch angepasst, die basierend auf den Inhalt der Zelle (in XAML "Auto")
- [`Star`](xref:Xamarin.Forms.GridUnitType.Star) &mdash; übrig gebliebene Höhe oder Breite proportional zugewiesen ist (eine Zahl mit "\*" namens *Stern*, in XAML)

Jedes untergeordnete Element von der `Grid` muss auch zugewiesen werden einer Zeile und Spalte (entweder explizit oder implizit). Umfasst Zeile und Spalte Span-Elemente sind optional. Diese werden alle mit angegeben angefügte bindbare Eigenschaften &mdash; von definierten Eigenschaften der `Grid` jedoch festgelegt, auf die untergeordneten Elemente des der `Grid`. `Grid` definiert vier statische angefügte bindbare Eigenschaften an:

- [`RowProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowProperty/) &mdash; Der nullbasierte Zeile. Der Standardwert ist 0
- [`ColumnProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnProperty/) &mdash; Der nullbasierte Spalte aus. Der Standardwert ist 0
- [`RowSpanProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowSpanProperty/) &mdash; die Anzahl der Zeilen der untergeordneten umfasst; Der Standardwert ist 1
- [`ColumnSpanProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnSpanProperty/) &mdash; die Anzahl der Spalten das untergeordnete Element erstreckt; Der Standardwert ist 1

Im Code kann ein Programm acht statische Methoden zum Festlegen und Abrufen dieser Werte verwenden:

- [`Grid.SetRow`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetRow/p/Xamarin.Forms.BindableObject/System.Int32/) und [`Grid.GetRow`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetRow/p/Xamarin.Forms.BindableObject/)
- [`Grid.SetColumn`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetColumn/p/Xamarin.Forms.BindableObject/System.Int32/) und [`Grid.GetColumn`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetColumn/p/Xamarin.Forms.BindableObject/)
- [`Grid.SetRowSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetRowSpan/p/Xamarin.Forms.BindableObject/System.Int32/) und [`Grid.GetRowSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetRowSpan/p/Xamarin.Forms.BindableObject/)
- [`Grid.SetColumnSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetColumnSpan/p/Xamarin.Forms.BindableObject/System.Int32/) und [`Grid.GetColumnSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetColumnSpan/p/Xamarin.Forms.BindableObject/)

In XAML verwenden Sie die folgenden Attribute zum Festlegen dieser Werte an:

- `Grid.Row`
- `Grid.Column`
- `Grid.RowSpan`
- `Grid.ColumnSpan`

Die [ **SimpleGridDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SimpleGridDemo) Beispiel veranschaulicht das Erstellen und initialisieren eine `Grid` in XAML.

Die `Grid` erbt die [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) Eigenschaft `Layout` und definiert zwei weitere Eigenschaften, die Abstände zwischen den Zeilen und Spalten zu ermöglichen:

- [`RowSpacing`](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.RowSpacing/) hat den Standardwert von 6
- [`ColumnSpacing`](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.ColumnSpacing/) hat den Standardwert von 6

Die `RowDefinitions` und `ColumnDefinitions` Auflistungen sind nicht unbedingt erforderlich. Falls nicht vorhanden, die `Grid` erstellt, Zeilen und Spalten für die `Grid` untergeordnete Elemente und gibt alle den Standardwert `GridLength` von "\*" (star).

### <a name="the-grid-in-code"></a>Das Raster im code

Die [ **GridCodeDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCodeDemo) Beispiel veranschaulicht, wie zum Erstellen und Auffüllen einer `Grid` im Code. Sie können die angefügten Eigenschaften festlegen, für jedes untergeordnete Element direkt oder indirekt durch Aufrufen von zusätzlichen `Add` Methoden, z. B. [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/System.Int32/System.Int32/) von definiert die [Grid.IGridList<T> ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid+IGridList%3CT%3E/) -Schnittstelle.

### <a name="the-grid-bar-chart"></a>Das Raster-Balkendiagramm

Die [ **GridBarChart** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridBarChart) Beispiel zeigt, wie mehrere hinzuzufügende `BoxView` Elementen, die eine `Grid` mit der massenbearbeitungsfunktion [ `AddHorizontal` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.AddHorizontal/p/System.Collections.Generic.IEnumerable%7BXamarin.Forms.View%7D/) Methode. Standardmäßig diese `BoxView` Elemente haben dieselbe Breite. Die Höhe der einzelnen `BoxView` kann dann gesteuert werden, um ein Balkendiagramm zu entsprechen.

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
