---
title: Zusammenfassung der Kapitel 17. Das Raster Mastering
description: 'Beim Erstellen mobiler Apps mit Xamarin.Forms: Zusammenfassung der Kapitel 17. Das Raster Mastering'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 71EDEF9C-4220-4D2E-A235-43F1EC8746C1
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 55418f24b1519dcab3b107f23976d50b401c813b
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240947"
---
# <a name="summary-of-chapter-17-mastering-the-grid"></a>Zusammenfassung der Kapitel 17. Das Raster Mastering

Die [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) ist ein leistungsstarke Layout-Mechanismus, der seine untergeordneten Elemente in Zeilen und Spalten von Zellen nebeneinander anordnet. Im Gegensatz zu ähnlichen HTML `table` Element, das `Grid` wird ausschließlich zum Zweck der Layout statt der Präsentation.

## <a name="the-basic-grid"></a>Das grundlegende Raster

`Grid` leitet sich von [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/), die definiert eine [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout%3CT%3E.Children/) Eigenschaft, die `Grid` erbt. Sie können diese Sammlung in XAML oder Code eintragen.

### <a name="the-grid-in-xaml"></a>Das Raster in XAML

Die Definition eine `Grid` beginnt in der Regel in XAML mit Füllen der [ `RowDefinitions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.RowDefinitions/) und [ `ColumnDefinitions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.ColumnDefinitions/) Sammlungen von der `Grid` mit [ `RowDefinition` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RowDefinition/) und [ `ColumnDefinition` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ColumnDefinition/) Objekte. Dies ist, wie Sie die Anzahl der Zeilen und Spalten des Einrichten der `Grid`, und ihre Eigenschaften.

`RowDefinition` verfügt über eine [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.RowDefinition.Height/) Eigenschaft und `ColumnDefinition` verfügt über eine [ `Width` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ColumnDefinition.Width/) -Eigenschaft, beide vom Typ [ `GridLength` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridLength/), eine Struktur.

In XAML wird die [ `GridLengthTypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridLengthTypeConverter/) konvertiert einfache Textzeichenfolgen in `GridLength` Werte. Im Hintergrund der [ `GridLength` Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.GridLength.GridLength/p/System.Double/Xamarin.Forms.GridUnitType/) erstellt die `GridLength` Wert basierend auf eine Zahl und ein Wert vom Typ [ `GridUnitType` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridUnitType/), eine Enumeration mit drei Membern:

- [`Absolute`](https://developer.xamarin.com/api/field/Xamarin.Forms.GridUnitType.Absolute/) &mdash; die Breite oder Höhe wird in geräteunabhängigen Einheiten (eine Zahl in XAML) angegeben.
- [`Auto`](https://developer.xamarin.com/api/field/Xamarin.Forms.GridUnitType.Auto/) &mdash; die Höhe oder Breite ist automatisch angepassten basierend auf den Inhalt der Zelle ("Auto" in XAML)
- [`Star`](https://developer.xamarin.com/api/field/Xamarin.Forms.GridUnitType.Star/) &mdash; Verbleibende Höhe oder Breite proportional zugewiesen ist (eine Zahl mit "\*" namens *Stern*, in XAML)

Jedes untergeordnete Element von der `Grid` muss auch zugewiesen werden einer Zeile und Spalte (entweder explizit oder implizit). Umfasst Zeile und Spalte Spannen sind optional. Diese werden alle mit angegeben angefügte bindbare Eigenschaften &mdash; durch definierten Eigenschaften der `Grid` jedoch festgelegt, auf die untergeordneten Elemente des der `Grid`. `Grid` definiert vier statische angefügte bindbare Eigenschaften an:

- [`RowProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowProperty/) &mdash; Die nullbasierte Zeile; Standard ist 0
- [`ColumnProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnProperty/) &mdash; Die nullbasierte Spalte; Standard ist 0
- [`RowSpanProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowSpanProperty/) &mdash; die Anzahl von Zeilen das untergeordnete Element erstreckt; Der Standardwert ist 1
- [`ColumnSpanProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnSpanProperty/) &mdash; die Anzahl der Spalten das untergeordnete Element erstreckt; Der Standardwert ist 1

Im Code kann ein Programm acht statische Methoden zum Festlegen und Abrufen dieser Werte verwenden:

- [`Grid.SetRow`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetRow/p/Xamarin.Forms.BindableObject/System.Int32/) und [`Grid.GetRow`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetRow/p/Xamarin.Forms.BindableObject/)
- [`Grid.SetColumn`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetColumn/p/Xamarin.Forms.BindableObject/System.Int32/) und [`Grid.GetColumn`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetColumn/p/Xamarin.Forms.BindableObject/)
- [`Grid.SetRowSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetRowSpan/p/Xamarin.Forms.BindableObject/System.Int32/) und [`Grid.GetRowSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetRowSpan/p/Xamarin.Forms.BindableObject/)
- [`Grid.SetColumnSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetColumnSpan/p/Xamarin.Forms.BindableObject/System.Int32/) und [`Grid.GetColumnSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetColumnSpan/p/Xamarin.Forms.BindableObject/)

In XAML verwenden Sie die folgenden Attribute für diese Werte festlegen:

- `Grid.Row`
- `Grid.Column`
- `Grid.RowSpan`
- `Grid.ColumnSpan`

Die [ **SimpleGridDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SimpleGridDemo) Beispiel veranschaulicht das Erstellen und Initialisieren einer `Grid` in XAML.

Die `Grid` erbt die [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) Eigenschaft von `Layout` und definiert zwei weitere Eigenschaften, die Abstände zwischen den Zeilen und Spalten bereitstellen:

- [`RowSpacing`](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.RowSpacing/) verfügt über einen Standardwert von 6
- [`ColumnSpacing`](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.ColumnSpacing/) verfügt über einen Standardwert von 6

Die `RowDefinitions` und `ColumnDefinitions` Auflistungen sind nicht unbedingt erforderlich. Falls nicht vorhanden, die `Grid` erstellt, Zeilen und Spalten für die `Grid` untergeordneten Elemente und weist ihnen alle den Standardwert `GridLength` von "\*" (Stern).

### <a name="the-grid-in-code"></a>Das Raster im code

Die [ **GridCodeDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCodeDemo) Beispiel zeigt, wie zum Erstellen und Auffüllen einer `Grid` im Code. Angefügten Eigenschaften können festgelegt werden, für jedes untergeordnete Element direkt oder indirekt durch Aufrufen von zusätzlichen `Add` Methoden, z. B. [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/System.Int32/System.Int32/) definiert, indem Sie die [Grid.IGridList<T> ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid+IGridList%3CT%3E/) -Schnittstelle.

### <a name="the-grid-bar-chart"></a>Das Raster-Balkendiagramm

Die [ **GridBarChart** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridBarChart) Beispiel veranschaulicht das Hinzufügen mehrerer `BoxView` Elemente, die eine `Grid` mithilfe der massenverarbeitung [ `AddHorizontal` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.AddHorizontal/p/System.Collections.Generic.IEnumerable%7BXamarin.Forms.View%7D/) Methode. Wird standardmäßig diese `BoxView` Elemente haben gleicher Breite. Die Höhe der einzelnen `BoxView` kann dann gesteuert werden, um ein Balkendiagramm ähneln.

Die `Grid` in der **GridBarChart** Freigaben Beispiel ein `AbsoluteLayout` übergeordneten mit anfänglich unsichtbar `Frame`. Das Programm setzt auch eine `TapGestureRecognizer` auf jedem `BoxView` verwenden die `Frame` Informationen über die abgerufenen Leiste angezeigt.

### <a name="alignment-in-the-grid"></a>Ausrichtung im Raster

Die [ **GridAlignment** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridAlignment) Beispiel veranschaulicht, wie die `VerticalOptions` und `HorizontalOptions` Eigenschaften zum Ausrichten von untergeordneten Elementen in einem `Grid` Zelle.

Die [ **SpacingButtons** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SpacingButtons) Beispiel gleichermaßen Leerzeichen `Button` Elemente in zentriert `Grid` Zellen.

### <a name="cell-dividers-and-borders"></a>Zelle Trennblättern und Rahmen

Die `Grid` schließt sich nicht auf eine Funktion, die Zelle Karteireiter oder Rahmen gezeichnet. Allerdings können Sie eigene vornehmen.

Die [ **GridCellDividers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellDividers) veranschaulicht, wie zusätzliche Zeilen und Spalten, die speziell für schlanke definieren `BoxView` Elemente Trennlinien zu imitieren.

Die [ **GridCellBorders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellBorders) Programm keine zusätzlichen Zellen erstellt, sondern stattdessen richtet `BoxView` Elemente in jeder Zelle ein Zellenrahmen zu imitieren.

## <a name="almost-real-life-grid-examples"></a>Beispiele für fast realen-Raster

Die [ **KeypadGrid** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/KeypadGrid) -Beispiel verwendet einen `Grid` eine Zehnertastatur angezeigt:

[![Dreifacher Screenshot, der Zehnertastatur Raster](images/ch17fg12-small.png "Gitter Zehnertastatur")](images/ch17fg12-large.png#lightbox "Gitter Zehnertastatur")

### <a name="responding-to-orientation-changes"></a>Reagieren auf Änderungen der Ausrichtung

Die `Grid` Struktur ein Programms auf Orientierung reagieren können. Die [ **GridRgbSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridRgbSliders) demonstriert eine Technik, mit dem ein Element zwischen einer zweiten Zeile oder einem Telefon Hochformat und die zweite Spalte oder einem Telefon Querformat verschoben.

Initialisiert das Programm `Slider` Elemente auf einen Bereich von 0 bis 255 und datenbindungen verwendet, um den Schieberegler im Hexadezimalformat anzuzeigen. Da die `Slider` Werte sind Gleitkomma-, und die Formatierungszeichenfolge für hexadezimal nur mit ganzen Zahlen funktioniert, .NET ein [ `DoubleToIntConvert` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DoubleToIntConverter.cs) -Klasse in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Library erleichtert.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 17 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch17-Apr2016.pdf)
- [Kapitel 17-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17)
- [Raster](~/xamarin-forms/user-interface/layouts/grid.md)
