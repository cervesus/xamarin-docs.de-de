---
title: Xamarin.Forms-Raster
description: In diesem Artikel wird erläutert, wie Sie die Xamarin.Forms-Raster-Klasse verwenden, um Ansichten in Rastern darzustellen, die Zeilen und Spalten besitzen wird.
ms.prod: xamarin
ms.assetid: 762B1802-D185-494C-B643-74EED55882FE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/26/2017
ms.openlocfilehash: 0d5df986caa01bba69b03d6502682889e78ecbc7
ms.sourcegitcommit: ee464f165eee7b4485266c11f167be557a0bacb2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58222314"
---
# <a name="xamarinforms-grid"></a>Xamarin.Forms-Raster

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)

[`Grid`](xref:Xamarin.Forms.Grid) unterstützt das Anordnen von Ansichten in Zeilen und Spalten. Zeilen und Spalten können festgelegt werden, um proportionale Größe bzw. absolute Größen zu erhalten. Die `Grid` Layout sollten nicht mit herkömmlichen Tabellen verwechselt werden und dient nicht zur Darstellung von Tabellendaten. `Grid` Es muss nicht das Konzept der Zeile, Spalte oder Zelle, die Formatierung. Im Gegensatz zu HTML-Tabellen `Grid` dient ausschließlich für das Inhaltslayout.

[![](grid-images/layouts-sml.png "Xamarin.Forms-Layouts")](grid-images/layouts.png#lightbox "Xamarin.Forms-Layouts")

Dieser Artikel behandelt:

- **[Zweck](#purpose)**  &ndash; Allgemeine Verwendungsmöglichkeiten für `Grid`.
- **[Nutzung](#usage)**  &ndash; mit `Grid` Erreichen des gewünschten Entwurfs.
  - **[Zeilen und Spalten](#rows-and-columns)**  &ndash; geben Zeilen und Spalten für die `Grid`.
  - **[Platzieren Ansichten](#placing-views-in-a-grid)**  &ndash; Ansichten in das Raster an bestimmten Zeilen und Spalten hinzufügen.
  - **[Der Abstand](#spacing)**  &ndash; die Leerzeichen zwischen den Zeilen und Spalten konfigurieren.
  - **[Spannen](#spans)**  &ndash; Konfigurieren von Elementen, die über mehrere Zeilen oder Spalten erstrecken.

![](grid-images/grid.png "Durchsuchen von Raster")

## <a name="purpose"></a>Zweck

`Grid` kann verwendet werden, um Ansichten in einem Raster anzuordnen. Dies ist in einer Reihe von Fällen nützlich:

- Anordnen von Schaltflächen in einer app Rechner
- Anordnen von Schaltflächen/Auswahl in einem Raster an, wie die IOS- oder Android Startbildschirme
- Anordnen von Ansichten, sodass sie gleicher Größe in einer Dimension (wie in einige Symbolleisten) sind

## <a name="usage"></a>Verwendung

Im Gegensatz zu herkömmlichen Tabellen `Grid` leitet nicht die Anzahl und Größe der Zeilen und Spalten aus dem Inhalt. Stattdessen `Grid` hat `RowDefinitions` und `ColumnDefinitions` Sammlungen. Diese enthalten Definitionen für wie viele Zeilen und Spalten angeordnet werden werden. Ansichten werden hinzugefügt, um `Grid` mit den angegebenen Zeilen- und Spaltenindizes, die zu identifizieren, welchen Zeilen und Spalten in eine Ansicht platziert werden soll.

### <a name="rows-and-columns"></a>Zeilen und Spalten

Zeilen-und Spalteninformationen befindet sich in `Grid`des `RowDefinitions`  &  `ColumnDefinitions` Eigenschaften, die einzelnen Sammlungen sind der [ `RowDefinition` ](xref:Xamarin.Forms.RowDefinition) und [ `ColumnDefinition` ](xref:Xamarin.Forms.ColumnDefinition)-Objekten. `RowDefinition` verfügt über eine einzelne Eigenschaft `Height`, und `ColumnDefinition` verfügt über eine einzelne Eigenschaft `Width`. Die Optionen für die Höhe und Breite sind wie folgt aus:

- **Automatische** &ndash; automatisch Größen an den Inhalt in der Zeile oder Spalte. Als angegebenen [ `GridUnitType.Auto` ](xref:Xamarin.Forms.GridUnitType) in c# oder als `Auto` in XAML.
- **Proportional(*)** &ndash; Größen von Zeilen und Spalten als Proportion des verbleibenden Speicherplatzes. Als Wert angegeben und `GridUnitType.Star` in C# geschrieben und als `#*` in XAML, mit `#` wird von den gewünschten Wert. Angeben einer Zeile/Spalte und `*` bewirkt, dass sie den verfügbaren Platz auszufüllen.
- **Absolute** &ndash; Größen der Spalten und Zeilen mit bestimmten festen Höhe und Breite Werten. Als Wert angegeben und `GridUnitType.Absolute` in C# geschrieben und als `#` in XAML, mit `#` wird von den gewünschten Wert.

> [!NOTE]
> Die Breitenwerte für Spalten werden als festgelegt `*` wird standardmäßig in Xamarin.Forms, die sicherstellt, dass es sich bei die Spalte den verfügbaren Platz ausfüllt. Die Höhenwerte für Zeilen werden auch als festgelegt `*` standardmäßig. 

Betrachten Sie eine app, die drei Zeilen und zwei Spalten aus. Die untere Zeile muss genau randleistenabschnitt 200 Pixel hoch sein, und die oberste Zeile zweimal so groß wie die mittlere Zeile werden muss. Die linke Spalte muss für den Inhalt ausreichend sein, und die rechte Spalte muss den verbleibenden Platz auszufüllen.

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

### <a name="placing-views-in-a-grid"></a>Platzieren von Ansichten in einem Raster

Platzieren von Ansichten in einer `Grid` müssen Sie deren hinzufügen als untergeordnete Elemente in das Raster, und klicken Sie dann angeben, welchen Zeilen und Spalten, die sie in gehören.

Verwenden Sie in XAML, `Grid.Row` und `Grid.Column` für jede einzelne Sicht Platzierung angeben. Beachten Sie, dass `Grid.Row` und `Grid.Column` geben an, die basierend auf die nullbasierte Listen von Zeilen und Spalten. Dies bedeutet, dass in einem 4 x 4-Raster der oberste linke Zelle ist (0,0) und die untere rechte Zelle (3,3).

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
grid.Children.Add(topRight, 1, 0);
grid.Children.Add(bottomLeft, 0, 1);
grid.Children.Add(bottomRight, 1, 1);
```

Der obige Code wird ein Raster mit vier Bezeichnungen, zwei Spalten und zwei Zeilen erstellt. Beachten Sie, dass jede Bezeichnung die gleiche Größe hat, und, dass die Zeilen erweitert werden, um die gesamte verfügbare Speicherplatz verwenden.

Im obigen Beispiel-Ansichten werden hinzugefügt, um die [ `Grid.Children` ](xref:Xamarin.Forms.Grid.Children) Sammlung mithilfe der [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/) Überladung, die linken und oberen Argumente angibt. Bei Verwendung der [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/System.Int32/System.Int32/) Überladung, der angibt, nach links, rechts, oben und unten Argumente, während Sie links und oben Argumente verweist immer auf die Zellen in der [ `Grid` ](xref:Xamarin.Forms.Grid), rechts und unten Argumente können angezeigt werden, verweisen auf Zellen, die außerhalb der `Grid`. Dies ist, da das rechte Argument immer größer als das linke Argument sein muss, und die unteren Argument muss immer größer als die Top-Argument. Das folgende Beispiel zeigt den entsprechenden Code mit `Add` Überladungen:

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

`Grid` verfügt über Eigenschaften zum Steuern der Abstand zwischen den Zeilen und Spalten. Die folgenden Eigenschaften stehen für die Anpassung der `Grid`:

- **ColumnSpacing** &ndash; den Abstand zwischen Spalten. Der Standardwert dieser Eigenschaft ist 6.
- **"RowSpacing"** &ndash; den Abstand zwischen Zeilen. Der Standardwert dieser Eigenschaft ist 6.

Gibt an, der folgende XAML ein `Grid` mit zwei Spalten und eine Zeile 5 Pixel der Abstand zwischen Spalten:

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
grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star)});
grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star)});
```

### <a name="spans"></a>Span-Eigenschaften

Häufig bei der Arbeit mit einem Raster besteht ein Element, das mehr als eine Zeile oder Spalte einnehmen sollte. Betrachten Sie eine einfachen Taschenrechner-Anwendung:

![](grid-images/calculator.png "Calulator-Anwendung")

Beachten Sie, dass die Schaltfläche "0" zwei Spalten, genau wie integrierte Rechner für jede Plattform umfasst. Dies erfolgt mithilfe der `ColumnSpan` Eigenschaft, die angibt, wie viele Spalten ein Element belegen soll. Der XAML für diese Schaltfläche:

```xaml
<Button Text = "0" Grid.Row="4" Grid.Column="0" Grid.ColumnSpan="2" />
```

Und in c#:

```csharp
Button zeroButton = new Button { Text = "0" };
controlGrid.Children.Add (zeroButton, 0, 4);
Grid.SetColumnSpan (zeroButton, 2);
```

Beachten Sie in statischen Methoden der Code die `Grid` -Klasse verwendet, um die Positionierung, einschließlich der Änderungen an Änderungen führen `ColumnSpan` und `RowSpan`. Außerdem werden Beachten Sie, dass im Gegensatz zu anderen Eigenschaften, die zu einem beliebigen Zeitpunkt festgelegt werden können, Eigenschaften festgelegt, die statische Methoden verwenden, bereits muss im Raster, bevor sie geändert werden.

Der vollständige XAML für die oben genannten Rechner-Anwendung sieht folgendermaßen aus:

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

Beachten Sie, dass sowohl die Bezeichnung am oberen Rand des Rasters und die Schaltfläche "NULL" Occuping mehr als einer Spalte. Auch wenn ein Layout ähnlich wie verschachtelte Raster erreicht werden konnte die `ColumnSpan`  &  `RowSpan` Ansatz ist einfacher.

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

- [Erstellen von mobilen Apps mit Xamarin.Forms Kapitel 17](https://developer.xamarin.com/r/xamarin-forms/book/chapter17.pdf)
- [Raster](xref:Xamarin.Forms.Grid)
- [Layout (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble-Beispiel (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
