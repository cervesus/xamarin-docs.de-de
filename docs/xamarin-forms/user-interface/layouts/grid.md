---
title: Raster
description: Ansichten werden in einem Raster dargestellt.
ms.topic: article
ms.prod: xamarin
ms.assetid: 762B1802-D185-494C-B643-74EED55882FE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/26/2017
ms.openlocfilehash: 18df6082f634d633d3f1ca8ea1e8d3f493532f47
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="grid"></a>Raster

[`Grid`](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) unterstützt das Anordnen von Ansichten in Zeilen und Spalten. Zeilen und Spalten können festgelegt werden, um proportional Größen oder absolute Größen haben. Die `Grid` Layout sollten nicht mit herkömmlichen Tabellen verwechselt werden und dient nicht zum Präsentieren von Tabellendaten. `Grid` Es muss nicht das Konzept der Zeile, Spalte oder Zelle, die Formatierung. Im Gegensatz zu HTML-Tabellen `Grid` ist ausschließlich für das Layout des Inhalt vorgesehen.

[![](grid-images/layouts-sml.png "Xamarin.Forms Layouts")](grid-images/layouts.png#lightbox "Xamarin.Forms Layouts")

Dieser Artikel umfasst folgende Themen:

- **[Zweck](#Purpose)**  &ndash; Allgemeine Verwendungsmöglichkeiten für `Grid`.
- **[Verwendung](#Usage)**  &ndash; wie `Grid` auf den gewünschten Entwurf zu erzielen.
  - **[Zeilen und Spalten](#Rows_and_Columns)**  &ndash; geben Zeilen und Spalten für die `Grid`.
  - **[Platzieren die Ansichten](#Placing_Views)**  &ndash; Ansichten im Raster in bestimmten Zeilen und Spalten hinzufügen.
  - **[Abstand](#Spacing)**  &ndash; die Leerzeichen zwischen den Zeilen und Spalten konfigurieren.
  - **[Spannen](#Spans)**  &ndash; konfigurieren Sie die Elemente, die über mehrere Zeilen oder Spalten umfassen.

![](grid-images/grid.png "Raster zum Durchsuchen von")

## <a name="purpose"></a>Zweck

`Grid` kann verwendet werden, um Sichten in einem Raster anordnen. Dies ist eine Anzahl von Fällen nützlich:

- Anordnen von Schaltflächen in einem Rechner-Anwendung
- Anordnen von Schaltflächen/Optionen in einem Raster an, wie die IOS- oder Android Startbildschirme
- Anordnen von Ansichten, damit sie gleicher Größe in einer Dimension (wie in einigen Symbolleisten) sind.

## <a name="usage"></a>Verwendung

Im Gegensatz zu herkömmlichen Tabellen `Grid` leitet die Anzahl und Größe der Zeilen und Spalten aus dem Inhalt nicht. Stattdessen `Grid` hat `RowDefinitions` und `ColumnDefinitions` Sammlungen. Diese Dateien enthalten Definitionen der, wie viele Zeilen und Spalten angeordnet werden. Ansichten werden hinzugefügt, um `Grid` mit den angegebenen Zeilen- und Spaltenindizes, die zu identifizieren, welche Zeile und Spalte eine Sicht in platziert werden soll.

<a name="Rows_and_Columns" />

### <a name="rows-and-columns"></a>Zeilen und Spalten

Zeilen-und Spalteninformationen befindet sich in `Grid`des `RowDefinitions`  &  `ColumnDefinitions` Eigenschaften, die einzelnen Sammlungen werden von [ `RowDefinition` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RowDefinition/) und [ `ColumnDefinition` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ColumnDefinition/)-Objekt zugeordnet. `RowDefinition` verfügt über eine einzelne Eigenschaft `Height`, und `ColumnDefinition` verfügt über eine einzelne Eigenschaft `Width`. Die Optionen für die Höhe und Breite werden wie folgt aus:

- **Automatische** &ndash; automatisch Größen an den Inhalt in der Zeile oder Spalte. Als angegebenen [ `GridUnitType.Auto` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridUnitType/) in c# oder als `Auto` in XAML.
- **Proportional(*)** &ndash; Größen von Zeilen und Spalten als Teile des verbleibenden Speicherplatzes. Als Wert angegeben und `GridUnitType.Star` in C# geschrieben und als `#*` in XAML mit `#` wird von den gewünschten Wert. Angeben einer Zeile/Spalte und `*` wird dazu führen, dass sie den verfügbaren Platz auszufüllen.
- **Absolute** &ndash; -Größen von Spalten und Zeilen mit bestimmten, feste Werte für Höhe und Breite. Als Wert angegeben und `GridUnitType.Absolute` in C# geschrieben und als `#` in XAML mit `#` wird von den gewünschten Wert.

> [!NOTE]
> Breitenwerte für Spalten werden festgelegt, als "*" in der Standardeinstellung in Xamarin.Forms, wodurch sichergestellt wird, dass die Spalte mit den verfügbaren Platz ausfüllen wird.

Erwägen Sie eine app, die drei Zeilen und zwei Spalten erforderlich ist. In die untersten Zeile muss genau 200px hoch sein, und die oberste Zeile muss zweimal so hoch wie die mittlere Zeile sein. Die linke Spalte breit genug, um den Inhalt angepasst werden muss, und die rechte Spalte muss den verbleibenden Platz auszufüllen.

In XAML:

```xaml
<Grid>
  <Grid.RowDefinitions>
    <RowDefinition Height="2*" />
    <RowDefinition Height="*" />
    <RowDefinition Height="200" />
  </Grid.RowDefinitions>
  <Grid.ColumnDefinitions>
    <ColumnDefinition Width="Auto" />
    <ColumnDefinition Width="*" />
  </Grid.ColumnDefinitions>
</Grid>
```

In C#:

```csharp
var grid = new Grid();
grid.RowDefinitions.Add (new RowDefinition { Height = new GridLength(2, GridUnitType.Star) });
grid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
grid.RowDefinitions.Add (new RowDefinition { Height = new GridLength(200)});
grid.ColumnDefinitions.Add (new ColumnDefinition{ Width = new GridLength (200) });
```

<a name="Placing_Views" />

### <a name="placing-views-in-a-grid"></a>Platzieren Sichten in einem Raster

Platzieren von Ansichten in einer `Grid` müssen Sie deren hinzufügen als untergeordnete Elemente in den Entwurfsbereich, und klicken Sie dann angeben, welche Zeile und Spalte, die sie in gehören.

In XAML verwenden `Grid.Row` und `Grid.Column` für jede einzelne Sicht Platzierung angeben. Beachten Sie, dass `Grid.Row` und `Grid.Column` Geben Sie an, die anhand der nullbasierten Listen mit Zeilen und Spalten. Dies bedeutet, dass in einem 4 x 4-Raster der oberste linke Zelle ist (0,0) und obersten rechten Zelle (3,3).

Die `Grid` gezeigt unten enthält vier Zellen:

![](grid-images/label-grid.png "Raster mit vier Ansichten")

In XAML:

```xaml
<Grid>
  <Grid.RowDefinitions>
    <RowDefinition Height="*" />
    <RowDefinition Height="*" />
  </Grid.RowDefinitions>
  <Grid.ColumnDefinitions>
    <ColumnDefinition Width="*" />
    <ColumnDefinition Width="*" />
  </Grid.ColumnDefinitions>
  <Label Text="Top Left" Grid.Row="0" Grid.Column="0" />
  <Label Text="Top Right" Grid.Row="0" Grid.Column="1" />
  <Label Text="Bottom Left" Grid.Row="1" Grid.Column="0" />
  <Label Text="Bottom Right" Grid.Row="1" Grid.Column="1" />
</Grid>
```

In C#:

```csharp
var grid = new Grid();

grid.RowDefinitions.Add(new RowDefinition { Height = new GridLength(1, GridUnitType.Star)});
grid.RowDefinitions.Add(new RowDefinition { Height = new GridLength(1, GridUnitType.Star)});
grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(1, GridUnitType.Star)});
grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(1, GridUnitType.Star)});

var topLeft = new Label { Text = "Top Left" };
var topRight = new Label { Text = "Top Right" };
var bottomLeft = new Label { Text = "Bottom Left" };
var bottomRight = new Label { Text = "Bottom Right" };

grid.Children.Add(topLeft, 0, 0);
grid.Children.Add(topRight, 0, 1);
grid.Children.Add(bottomLeft, 1, 0);
grid.Children.Add(bottomRight, 1, 1);
```

Der obige Code erstellt Raster mit vier Bezeichnungen, zwei Spalten und zwei Zeilen. Beachten Sie, dass jede Bezeichnung dieselbe Größe aufweisen, wird und dass die Zeilen erweitert werden, um den gesamten verfügbaren Speicherplatz zu verwenden.

Im obigen Beispiel Sichten hinzugefügt werden die [ `Grid.Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.Children/) Auflistung mithilfe der [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/) Überladung auf, die linke und obere Argumente angibt. Bei Verwendung der [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/System.Int32/System.Int32/) Überladung, der angibt, nach links, rechts, oben und unten Argumente, während er sich links und oberen Argumente verweist immer auf die Zellen innerhalb der [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/), rechts und unten Argumente scheinbar auf Zellen verweisen, die sich außerhalb der `Grid`. Dies ist, da das rechte Argument muss immer größer als das linke Argument aufweisen, und die unteren-Argument immer größer als der Top-Argument sein muss. Das folgende Beispiel zeigt den entsprechenden Code mit `Add` Überladungen:

```csharp
// left, top
grid.Children.Add(topLeft, 0, 0);
grid.Children.Add(topRight, 1, 0);
grid.Children.Add(bottomLeft, 0, 1);
grid.Children.Add(bottomRight, 1, 1);

// left, right, top, bottom
grid.Children.Add(topLeft, 0, 1, 0, 1);
grid.Children.Add(topRight, 1, 2, 0, 1);
grid.Children.Add(bottomLeft, 0, 1, 1, 2);
grid.Children.Add(bottomRight, 1, 2, 1, 2);
```

### <a name="spacing"></a>Abstand

`Grid` verfügt über Eigenschaften, um den Abstand zwischen Zeilen und Spalten zu steuern.  Die folgenden Eigenschaften stehen für die Anpassung der `Grid`:

- **ColumnSpacing** &ndash; den Abstand zwischen Spalten.
- **RowSpacing** &ndash; den Abstand zwischen Zeilen.

Der folgende XAML-Code gibt eine `Grid` mit zwei Spalten, eine Zeile, und 5 px Abstand zwischen Spalten:

```xaml
<Grid ColumnSpacing="5">
  <Grid.ColumnDefinitions>
    <ColumnDefinitions Width="*" />
    <ColumnDefinitions Width="*" />
  </Grid.ColumnDefinitions>
</Grid>
```

In C#:

```csharp
var grid = new Grid { ColumnSpacing = 5 };
grid.ColumnDefnitions.Add(new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star)});
grid.ColumnDefnitions.Add(new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star)});
```

### <a name="spans"></a>Span-Eigenschaften

Häufig bei der Arbeit mit einem Raster besteht ein Element, das mehr als eine Zeile oder Spalte einnehmen sollte. Betrachten Sie eine einfachen Rechner-Anwendung:

![](grid-images/calculator.png "Calulator-Anwendung")

Beachten Sie, dass die Schaltfläche "0" zwei Spalten, wie die integrierte Rechner für jede Plattform umfasst. Dies erfolgt mit der `ColumnSpan` Eigenschaft, die angibt, wie viele Spalten ein Element einnehmen sollte. Der XAML-Code für diese Schaltfläche:

```xaml
<Button Text = "0" Grid.Row="4" Grid.Column="0" Grid.ColumnSpan="2" />
```

Auch in c#:

```csharp
Button zeroButton = new Button { Text = "0" };
controlGrid.Children.Add (zeroButton, 0, 4);
Grid.SetColumnSpan (zeroButton, 2);
```

Beachten Sie im Code der statischen Methoden der `Grid` Klasse werden verwendet, um die Positionierung Änderungen, einschließlich Änderungen an den ausführen `ColumnSpan` und `RowSpan`. Außerdem werden Beachten Sie, dass im Gegensatz zu anderen Eigenschaften, die zu einem beliebigen Zeitpunkt festgelegt werden können, mithilfe der statischen Methoden festgelegte Eigenschaften bereits müssen im Raster, bevor sie geändert werden.

Der vollständigen XAML-Code für die oben genannten Rechner-Anwendung sieht folgendermaßen aus:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="LayoutSamples.CalculatorGridXAML"
Title = "Calculator - XAML"
BackgroundColor="#404040">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="plainButton" TargetType="Button">
                <Setter Property="BackgroundColor" Value="#eee"/>
                <Setter Property="TextColor" Value="Black" />
                <Setter Property="BorderRadius" Value="0"/>
                <Setter Property="FontSize" Value="40" />
            </Style>
            <Style x:Key="darkerButton" TargetType="Button">
                <Setter Property="BackgroundColor" Value="#ddd"/>
                <Setter Property="TextColor" Value="Black" />
                <Setter Property="BorderRadius" Value="0"/>
                <Setter Property="FontSize" Value="40" />
            </Style>
            <Style x:Key="orangeButton" TargetType="Button">
                <Setter Property="BackgroundColor" Value="#E8AD00"/>
                <Setter Property="TextColor" Value="White" />
                <Setter Property="BorderRadius" Value="0"/>
                <Setter Property="FontSize" Value="40" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <Grid x:Name="controlGrid" RowSpacing="1" ColumnSpacing="1">
            <Grid.RowDefinitions>
                <RowDefinition Height="150" />
                <RowDefinition Height="*" />
                <RowDefinition Height="*" />
                <RowDefinition Height="*" />
                <RowDefinition Height="*" />
                <RowDefinition Height="*" />
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
            </Grid.ColumnDefinitions>
            <Label Text="0" Grid.Row="0" HorizontalTextAlignment="End" VerticalTextAlignment="End" TextColor="White"
        FontSize="60" Grid.ColumnSpan="4" />
            <Button Text = "C" Grid.Row="1" Grid.Column="0"
        Style="{StaticResource darkerButton}" />
            <Button Text = "+/-" Grid.Row="1" Grid.Column="1"
        Style="{StaticResource darkerButton}" />
            <Button Text = "%" Grid.Row="1" Grid.Column="2"
        Style="{StaticResource darkerButton}" />
            <Button Text = "div" Grid.Row="1" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
            <Button Text = "7" Grid.Row="2" Grid.Column="0"
        Style="{StaticResource plainButton}" />
            <Button Text = "8" Grid.Row="2" Grid.Column="1"
        Style="{StaticResource plainButton}" />
            <Button Text = "9" Grid.Row="2" Grid.Column="2"
        Style="{StaticResource plainButton}" />
            <Button Text = "X" Grid.Row="2" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
            <Button Text = "4" Grid.Row="3" Grid.Column="0"
        Style="{StaticResource plainButton}" />
            <Button Text = "5" Grid.Row="3" Grid.Column="1"
        Style="{StaticResource plainButton}" />
            <Button Text = "6" Grid.Row="3" Grid.Column="2"
        Style="{StaticResource plainButton}" />
            <Button Text = "-" Grid.Row="3" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
            <Button Text = "1" Grid.Row="4" Grid.Column="0"
        Style="{StaticResource plainButton}" />
            <Button Text = "2" Grid.Row="4" Grid.Column="1"
        Style="{StaticResource plainButton}" />
            <Button Text = "3" Grid.Row="4" Grid.Column="2"
        Style="{StaticResource plainButton}" />
            <Button Text = "+" Grid.Row="4" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
            <Button Text = "0" Grid.ColumnSpan="2"
        Grid.Row="5" Grid.Column="0" Style="{StaticResource plainButton}" />
            <Button Text = "." Grid.Row="5" Grid.Column="2"
        Style="{StaticResource plainButton}" />
            <Button Text = "=" Grid.Row="5" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
        </Grid>
    </ContentPage.Content>
</ContentPage>
```

Beachten Sie, dass sowohl die Beschriftung oben im Raster die Schaltfläche "NULL" Occuping mehr als eine Spalte. Obwohl ähnlich aufgebaut verschachtelte Raster erreicht werden konnten die `ColumnSpan`  &  `RowSpan` Ansatz ist einfacher.

Die C#-Implementierung:

```csharp
public CalculatorGridCode ()
{
  Title = "Calculator - C#";
  BackgroundColor = Color.FromHex ("#404040");

  var plainButton = new Style (typeof(Button)) {
    Setters = {
      new Setter { Property = Button.BackgroundColorProperty, Value = Color.FromHex ("#eee") },
      new Setter { Property = Button.TextColorProperty, Value = Color.Black },
      new Setter { Property = Button.BorderRadiusProperty, Value = 0 },
      new Setter { Property = Button.FontSizeProperty, Value = 40 }
    }
  };
  var darkerButton = new Style (typeof(Button)) {
    Setters = {
      new Setter { Property = Button.BackgroundColorProperty, Value = Color.FromHex ("#ddd") },
      new Setter { Property = Button.TextColorProperty, Value = Color.Black },
      new Setter { Property = Button.BorderRadiusProperty, Value = 0 },
      new Setter { Property = Button.FontSizeProperty, Value = 40 }
    }
  };
  var orangeButton = new Style (typeof(Button)) {
    Setters = {
      new Setter { Property = Button.BackgroundColorProperty, Value = Color.FromHex ("#E8AD00") },
      new Setter { Property = Button.TextColorProperty, Value = Color.White },
      new Setter { Property = Button.BorderRadiusProperty, Value = 0 },
      new Setter { Property = Button.FontSizeProperty, Value = 40 }
    }
  };

  var controlGrid = new Grid { RowSpacing = 1, ColumnSpacing = 1 };
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (150) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });

  controlGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
  controlGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
  controlGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
  controlGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });

  var label = new Label {
    Text = "0",
    HorizontalTextAlignment = TextAlignment.End,
    VerticalTextAlignment = TextAlignment.End,
    TextColor = Color.White,
    FontSize = 60
  };
  controlGrid.Children.Add (label, 0, 0);

  Grid.SetColumnSpan (label, 4);

  controlGrid.Children.Add (new Button { Text = "C", Style = darkerButton }, 0, 1);
  controlGrid.Children.Add (new Button { Text = "+/-", Style = darkerButton }, 1, 1);
  controlGrid.Children.Add (new Button { Text = "%", Style = darkerButton }, 2, 1);
  controlGrid.Children.Add (new Button { Text = "div", Style = orangeButton }, 3, 1);
  controlGrid.Children.Add (new Button { Text = "7", Style = plainButton }, 0, 2);
  controlGrid.Children.Add (new Button { Text = "8", Style = plainButton }, 1, 2);
  controlGrid.Children.Add (new Button { Text = "9", Style = plainButton }, 2, 2);
  controlGrid.Children.Add (new Button { Text = "X", Style = orangeButton }, 3, 2);
  controlGrid.Children.Add (new Button { Text = "4", Style = plainButton }, 0, 3);
  controlGrid.Children.Add (new Button { Text = "5", Style = plainButton }, 1, 3);
  controlGrid.Children.Add (new Button { Text = "6", Style = plainButton }, 2, 3);
  controlGrid.Children.Add (new Button { Text = "-", Style = orangeButton }, 3, 3);
  controlGrid.Children.Add (new Button { Text = "1", Style = plainButton }, 0, 4);
  controlGrid.Children.Add (new Button { Text = "2", Style = plainButton }, 1, 4);
  controlGrid.Children.Add (new Button { Text = "3", Style = plainButton }, 2, 4);
  controlGrid.Children.Add (new Button { Text = "+", Style = orangeButton }, 3, 4);
  controlGrid.Children.Add (new Button { Text = ".", Style = plainButton }, 2, 5);
  controlGrid.Children.Add (new Button { Text = "=", Style = orangeButton }, 3, 5);

  var zeroButton = new Button { Text = "0", Style = plainButton };
  controlGrid.Children.Add (zeroButton, 0, 5);
  Grid.SetColumnSpan (zeroButton, 2);

  Content = controlGrid;
}
```


## <a name="related-links"></a>Verwandte Links

- [Beim Erstellen mobiler Apps mit Xamarin.Forms, Kapitel 17](https://developer.xamarin.com/r/xamarin-forms/book/chapter17.pdf)
- [Raster](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/)
- [Layout (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble-Beispiel (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
