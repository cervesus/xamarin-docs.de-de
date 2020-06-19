---
title: 'title: "Zusammenfassung von Kapitel 17: Arbeiten mit dem Raster" description: "Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung von Kapitel 17:'
description: 'Arbeiten mit dem Raster" ms.prod: xamarin ms.technology: xamarin-forms ms.assetid: 71EDEF9C-4220-4D2E-A235-43F1EC8746C1 author: davidbritch ms.author: dabritch ms.date: 11/07/2017 no-loc: [Xamarin.Forms, Xamarin.Essentials] Zusammenfassung von Kapitel 17:'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 71EDEF9C-4220-4D2E-A235-43F1EC8746C1
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 6dd13c0f592831c6488afac6727bcac734e9136a
ms.sourcegitcommit: ea9269b5d9e3d68b61bb428560a10034117ee457
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2020
ms.locfileid: "84136720"
---
# <a name="summary-of-chapter-17-mastering-the-grid"></a>Arbeiten mit dem Raster [![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17)

Das Raster ([`Grid`](xref:Xamarin.Forms.Grid)) ist ein leistungsstarker Layoutmechanismus, dessen untergeordnete Elemente in Zeilen und Spalten aus Zellen angeordnet sind.

Im Gegensatz zu dem ähnlichen HTML-Element `table` ist das `Grid` ausschließlich für Layout- und nicht für Präsentationszwecke gedacht. Das grundlegende Raster

## <a name="the-basic-grid"></a>`Grid` wird aus [`Layout<View>`](xref:Xamarin.Forms.Layout`1) abgeleitet, welches eine [`Children`](xref:Xamarin.Forms.Layout`1.Children)-Eigenschaft definiert, die `Grid` erbt.

Sie können diese Sammlung mit XAML oder Code auffüllen. Das Raster in XAML

### <a name="the-grid-in-xaml"></a>Das Definieren von `Grid` in XAML beginnt üblicherweise mit dem Auffüllen der Sammlungen [`RowDefinitions`](xref:Xamarin.Forms.Grid.RowDefinitions) und [`ColumnDefinitions`](xref:Xamarin.Forms.Grid.ColumnDefinitions) von `Grid` mit [`RowDefinition`](xref:Xamarin.Forms.RowDefinition)- und [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition)-Objekten.

Auf diese Weise legen Sie die Anzahl von Zeilen und Spalten von `Grid` sowie deren Eigenschaften fest. `RowDefinition` verfügt über die Eigenschaft [`Height`](xref:Xamarin.Forms.RowDefinition.Height) und `ColumnDefinition` über die Eigenschaft [`Width`](xref:Xamarin.Forms.ColumnDefinition.Width), beide vom Typ [`GridLength`](xref:Xamarin.Forms.GridLength), eine Struktur.

In XAML konvertiert [`GridLengthTypeConverter`](xref:Xamarin.Forms.GridLengthTypeConverter) einfache Textzeichenfolgen in `GridLength`-Werte.

Dabei erstellt der [`GridLength` Konstruktor](xref:Xamarin.Forms.GridLength.%23ctor(System.Double,Xamarin.Forms.GridUnitType)) im Hintergrund den Wert für `GridLength` auf Grundlage einer Zahl und eines Werts für den Typ [`GridUnitType`](xref:Xamarin.Forms.GridUnitType). Dies ist eine Enumeration mit drei Elementen: [`Absolute`](xref:Xamarin.Forms.GridUnitType.Absolute): Die Breite oder Höhe wird in geräteunabhängigen Einheiten (eine Zahl in XAML) angegeben.

- [`Auto`](xref:Xamarin.Forms.GridUnitType.Auto): Die Höhe oder Breite wird abhängig vom Zelleninhalt automatisch angepasst („Auto“ in XAML).
- [`Star`](xref:Xamarin.Forms.GridUnitType.Star): Überschüssige Höhe oder Breite wird proportional verteilt (eine Zahl mit „\*“ (als *Sternvariable* bezeichnet) in XAML).
- Allen untergeordneten Elementen von `Grid` muss außerdem eine Zeile und Spalte zugeordnet werden (entweder explizit oder implizit).

Zeilen- und Spaltenweite sind optional. Diese werden alle mit angefügten bindbaren Eigenschaften festgelegt – Eigenschaften, die von `Grid` definiert, aber für untergeordnete Elemente von `Grid` festgelegt werden. `Grid` definiert vier statische angefügte bindbare Eigenschaften: [`RowProperty`](xref:Xamarin.Forms.Grid.RowProperty): die nullbasierte Zeile; der Standardwert ist 0

- [`ColumnProperty`](xref:Xamarin.Forms.Grid.ColumnProperty): die nullbasierte Spalte; der Standardwert ist 0
- [`RowSpanProperty`](xref:Xamarin.Forms.Grid.RowSpanProperty): die Anzahl der Zeilen, die das untergeordnete Element umfasst; der Standardwert ist 1
- [`ColumnSpanProperty`](xref:Xamarin.Forms.Grid.ColumnSpanProperty): die Anzahl der Spalten, die das untergeordnete Element umfasst; der Standardwert ist 1
- Im Code kann ein Programm die folgenden acht statischen Methoden verwenden, um diese Werte festzulegen und abzurufen:

[`Grid.SetRow`](xref:Xamarin.Forms.Grid.SetRow(Xamarin.Forms.BindableObject,System.Int32)) und [`Grid.GetRow`](xref:Xamarin.Forms.Grid.GetRow(Xamarin.Forms.BindableObject))

- [`Grid.SetColumn`](xref:Xamarin.Forms.Grid.SetColumn(Xamarin.Forms.BindableObject,System.Int32)) und [`Grid.GetColumn`](xref:Xamarin.Forms.Grid.GetColumn(Xamarin.Forms.BindableObject))
- [`Grid.SetRowSpan`](xref:Xamarin.Forms.Grid.SetRowSpan(Xamarin.Forms.BindableObject,System.Int32)) und [`Grid.GetRowSpan`](xref:Xamarin.Forms.Grid.GetRowSpan(Xamarin.Forms.BindableObject))
- [`Grid.SetColumnSpan`](xref:Xamarin.Forms.Grid.SetColumnSpan(Xamarin.Forms.BindableObject,System.Int32)) und [`Grid.GetColumnSpan`](xref:Xamarin.Forms.Grid.GetColumnSpan(Xamarin.Forms.BindableObject))
- In XAML werden diese Werte mit den folgenden Attributen festgelegt:

Im Beispiel [**SimpleGridDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SimpleGridDemo) wird gezeigt, wie ein `Grid` in XAML erstellt und initialisiert wird.

- `Grid.Row`
- `Grid.Column`
- `Grid.RowSpan`
- `Grid.ColumnSpan`

Das `Grid` erbt die Eigenschaft [`Padding`](xref:Xamarin.Forms.Layout.Padding) von `Layout` und definiert zwei zusätzliche Eigenschaften für Abstand zwischen den Zeilen und Spalten:

[`RowSpacing`](xref:Xamarin.Forms.Grid.RowSpacing) hat einen Standardwert von 6.

- [`ColumnSpacing`](xref:Xamarin.Forms.Grid.ColumnSpacing) hat einen Standardwert von 6.
- Die Sammlungen `RowDefinitions` und `ColumnDefinitions` sind nicht zwingend erforderlich.

Wenn sie nicht vorhanden sind, erstellt `Grid` Zeilen und Spalten für die untergeordneten Elemente von `Grid` und legt für alle diese Elemente als Standardwert für `GridLength` „\*“ (Sternvariable) fest. Das Raster in Code

### <a name="the-grid-in-code"></a>Im Beispiel [**GridCodeDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCodeDemo) wird gezeigt, wie ein `Grid` in Code erstellt und aufgefüllt wird.

Sie können die angefügten Eigenschaften für jedes untergeordnete Element direkt oder indirekt durch Aufrufen zusätzlicher `Add`-Methoden festlegen, z. B. [`Add`](xref:Xamarin.Forms.Grid.IGridList`1.Add*), wie durch die [Grid.IGridList<T>](xref:Xamarin.Forms.Grid.IGridList`1)-Schnittstelle definiert. Das Raster-Balkendiagramm

### <a name="the-grid-bar-chart"></a>Im Beispiel [**GridBarChart**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridBarChart) wird gezeigt, wie Sie mehrere `BoxView`-Elemente zu einem `Grid` hinzufügen, indem Sie die Massenmethode [`AddHorizontal`](xref:Xamarin.Forms.Grid.IGridList`1.AddHorizontal*) verwenden.

Standardmäßig sind diese `BoxView`-Elemente gleich breit. Die Höhe jedes `BoxView`-Elements kann dann so gesteuert werden, dass das Raster einem Balkendiagramm ähnelt. Das `Grid` im Beispiel **GridBarChart** verfügt zusammen mit einem ursprünglich unsichtbaren `Frame` über ein gemeinsames übergeordnetes Element `AbsoluteLayout`.

Das Programm legt außerdem einen `TapGestureRecognizer` für jede `BoxView` fest, um den `Frame` zum Anzeigen von Informationen über die angetippte Leiste zu verwenden. Ausrichtung im Raster

### <a name="alignment-in-the-grid"></a>Im Beispiel [**GridAlignment**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridAlignment) wird gezeigt, wie Sie mit den Eigenschaften `VerticalOptions` und `HorizontalOptions` untergeordnete Elemente in einer `Grid`-Zelle ausrichten.

Im Beispiel [**SpacingButtons**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SpacingButtons) werden gleichmäßige Abstände zwischen in `Grid`-Zellen zentrierte `Button`-Elemente gesetzt.

Trennlinien zwischen Zellen und Rahmen

### <a name="cell-dividers-and-borders"></a>Das `Grid` verfügt über kein Feature für Trennlinien zwischen Zellen und Rahmen.

Sie können diese allerdings selbst erstellen. In [**GridCellDividers**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellDividers) wird gezeigt, wie Sie zusätzliche Zeilen und Spalten speziell für dünne `BoxView`-Elemente definieren, um Trennlinien zu imitieren.

Das Programm [**GridCellBorders**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellBorders) erstellt keine zusätzlichen Zellen, sondern richtet `BoxView`-Elemente in jeder Zelle aus, um einen Zellenrahmen zu imitieren.

Fast aus dem Leben gegriffene Beispiele für Raster

## <a name="almost-real-life-grid-examples"></a>Im Beispiel [**KeypadGrid**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/KeypadGrid) wird ein `Grid` verwendet, um eine Zehnertastatur anzuzeigen:

[![Dreifacher Screenshot eines Rasters mit einer Zehnertastatur](images/ch17fg12-small.png "Raster mit einer Zehnertastatur")](images/ch17fg12-large.png#lightbox "Raster mit einer Zehnertastatur")

Reagieren auf Änderung der Ausrichtung

### <a name="responding-to-orientation-changes"></a>Mit dem `Grid` kann ein Programm so strukturiert werden, dass es auf eine Änderung der Ausrichtung reagiert.

Im Beispiel [**GridRgbSliders**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridRgbSliders) wird eine Technik gezeigt, mit der ein Element von der zweiten Zeile bei einem Telefon im Hochformat in die zweite Spalte bei einem Telefon im Querformat verschoben wird und umgekehrt. Das Programm initialisiert `Slider`-Elemente mit einer Spanne von 0 bis 255 und verwendet Datenbindungen, um den Wert der Schieberegler als Hexadezimalzahl anzuzeigen.

Da es sich bei den `Slider`-Werten um Gleitkommazahlen handelt und die .NET-Formatierungszeichenfolge für Hexadezimalzahlen nur mit Zahlen vom Typ Integer funktioniert, wird eine [`DoubleToIntConvert`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DoubleToIntConverter.cs)-Klasse aus der [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)-Bibliothek verwendet. Verwandte Links

## <a name="related-links"></a>[Kapitel 17: vollständiger Text (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch17-Apr2016.pdf)

- [Kapitel 17: Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17)
- [Raster](~/xamarin-forms/user-interface/layouts/grid.md)
- Raster mit einer Zehnertastatur
