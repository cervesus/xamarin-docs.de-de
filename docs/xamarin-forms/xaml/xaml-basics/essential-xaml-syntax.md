---
title: Teil 2. Grundlegende XAML-Syntax
description: "XAML ist hauptsächlich für das Instanziieren und Initialisieren von Objekten. Aber häufig Eigenschaften müssen festgelegt werden, um komplexe Objekte, die einfach als XML-Zeichenfolgen dargestellt werden können, und in einigen Fällen müssen durch eine Klasse definierte Eigenschaften festgelegt werden, auf eine untergeordnete Klasse. Diese beiden Anforderungen erfordern die wesentlichen Features der XAML-Syntax von Eigenschaftenelemente und angefügte Eigenschaften."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1D6164F9-4ECE-43A6-B583-1F5D5EFC1DDF
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2017
ms.openlocfilehash: f99d4b177f5957b2e5f8c22171fe92799af8505a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="part-2-essential-xaml-syntax"></a>Teil 2. Grundlegende XAML-Syntax

_XAML ist hauptsächlich für das Instanziieren und Initialisieren von Objekten. Aber häufig Eigenschaften müssen festgelegt werden, um komplexe Objekte, die einfach als XML-Zeichenfolgen dargestellt werden können, und in einigen Fällen müssen durch eine Klasse definierte Eigenschaften festgelegt werden, auf eine untergeordnete Klasse. Diese beiden Anforderungen erfordern die wesentlichen Features der XAML-Syntax von Eigenschaftenelemente und angefügte Eigenschaften._

## <a name="property-elements"></a>Eigenschaftenelemente

In XAML werden Eigenschaften von Klassen normalerweise als XML-Attribute festlegen:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large"
       TextColor="Aqua" />
```

Allerdings ist eine alternative Möglichkeit, eine Eigenschaft in XAML festlegen. Probieren Sie diese Alternative mit `TextColor`, löschen Sie zuerst die vorhandene `TextColor` Einstellung:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large" />
```

Öffnen Sie das leere Element `Label` -Tag in die Start- und Endtags trennen:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large">

</Label>
```

Fügen Sie innerhalb dieser Tags Start- und Endtags, die aus den Klassennamen und einen Eigenschaftsnamen, die durch einen Punkt getrennt bestehen:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large">
    <Label.TextColor>

    </Label.TextColor>
</Label>
```

Legen Sie den Wert der Eigenschaft als Inhalt dieser neuen Tags wie folgt:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large">
    <Label.TextColor>
        Aqua
    </Label.TextColor>
</Label>
```

Diese zwei Möglichkeiten zum Angeben der `TextColor` Eigenschaft sind funktional äquivalent, aber nicht die zwei Möglichkeiten für die gleiche Eigenschaft verwenden, da, die würde effektiv werden Festlegen der Eigenschaft zweimal mehrdeutig ist.

Mit dieser neuen Syntax kann praktisch lernen Sie Terminologie kennen eingeleitet werden:

-  `Label` ist ein *Objektelement*. Es ist ein Xamarin.Forms-Objekt, ausgedrückt als ein XML-Element.
-  `Text`, `VerticalOptions`, `FontAttributes` und `FontSize` sind *Eigenschaftenattribute*. Xamarin.Forms-Eigenschaften, die als XML-Attribute ausgedrückt werden.
-  In diesem abschließenden Ausschnitt `TextColor` geworden ist ein *Eigenschaftselement*. Es ist eine Xamarin.Forms-Eigenschaft, aber es ist jetzt ein XML-Element.


Die Definition der Eigenschaft, die Elemente am möglicherweise zuerst eine Verletzung der XML-Syntax zu sein scheinen, aber es ist nicht. Der Zeitraum muss keine speziellen Bedeutung in XML. Ein XML-Decoder `Label.TextColor` ist einfach eine normale untergeordnetes Element.

In XAML ist jedoch diese Syntax sehr spezielle. Einer der Regeln für Eigenschaftenelemente ist, dass keine anderen Aufgaben im angezeigt werden, kann die `Label.TextColor` Tag. Der Wert der Eigenschaft wird immer als Inhalt zwischen Eigenschaftselement Start- und Endtags definiert.

Sie können auf mehr als eine Eigenschaft Eigenschaftenelementsyntax verwenden:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center">
    <Label.FontAttributes>
        Bold
    </Label.FontAttributes>
    <Label.FontSize>
        Large
    </Label.FontSize>
    <Label.TextColor>
        Aqua
    </Label.TextColor>
</Label>
```

Oder Sie können Eigenschaftenelementsyntax für alle Eigenschaften verwenden:

```xaml
<Label>
    <Label.Text>
        Hello, XAML!
    </Label.Text>
    <Label.FontAttributes>
        Bold
    </Label.FontAttributes>
    <Label.FontSize>
        Large
    </Label.FontSize>
    <Label.TextColor>
        Aqua
    </Label.TextColor>
    <Label.VerticalOptions>
        Center
    </Label.VerticalOptions>
</Label>
```

Zunächst Eigenschaftenelementsyntax scheint eine unnötige langwieriger Ersatz für etwas vergleichsweise einfach, und in diesen Beispielen ist immer der Fall.

Eigenschaftenelementsyntax wird jedoch wichtig, wenn der Wert einer Eigenschaft als eine einfache Zeichenfolge ausgedrückt werden zu komplex ist. Innerhalb der Property-Element Tags können Sie ein anderes Objekt zu instanziieren und seine Eigenschaften festlegen. Angenommen, Sie können explizit festlegen eine Eigenschaft wie z. B. `VerticalOptions` zu einem `LayoutOptions` Wert mit eigenschafteneinstellungen:

```xaml
<Label>
    ...
    <Label.VerticalOptions>
        <LayoutOptions Alignment="Center" />
    </Label.VerticalOptions>
</Label>
```

Ein weiteres Beispiel: die `Grid` verfügt über zwei Eigenschaften, die mit dem Namen `RowDefinitions` und `ColumnDefinitions`. Diese beiden Eigenschaften sind vom Typ `RowDefinitionCollection` und `ColumnDefinitionCollection`, wobei es sich um Auflistungen von `RowDefinition` und `ColumnDefinition` Objekte. In diesem Fall müssen Sie eine Eigenschaftenelementsyntax zu verwenden, um diese Sammlungen festzulegen.

Hier ist der Anfang der XAML-Datei für eine `GridDemoPage` -Klasse, mit der Eigenschaft Element-Tags für die `RowDefinitions` und `ColumnDefinitions` Sammlungen:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.GridDemoPage"
             Title="Grid Demo Page">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
            <RowDefinition Height="100" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="100" />
        </Grid.ColumnDefinitions>
        ...
    </Grid>
</ContentPage>
```

Beachten Sie die abgekürzte Syntax zum Definieren von Zellen automatisch angepasst, die Zellen des Pixel Breiten und Höhen und Stern Einstellungen aus.

## <a name="attached-properties"></a>Angefügte Eigenschaften

Sie habe soeben gesehen, wie die `Grid` erfordert Eigenschaftenelemente für die `RowDefinitions` und `ColumnDefinitions` Sammlungen, um die Zeilen und Spalten zu definieren. Es müssen jedoch auch sein irgendeiner der Programmierer die Zeile und Spalte anzugeben, in dem jedes untergeordnete Element des der `Grid` befindet.

Innerhalb des Tags für jedes untergeordnete Element von der `Grid` Geben Sie die Zeile und Spalte mit diesem untergeordneten mithilfe der folgenden Attribute:

-  `Grid.Row`
-  `Grid.Column`

Die Standardwerte für diese Attribute sind 0. Sie können auch angeben, wenn ein untergeordnetes Element mehr als eine Zeile oder Spalte mit diesen Attributen umfasst:

-  `Grid.RowSpan`
-  `Grid.ColumnSpan`

Diese beiden Attribute haben standardmäßig den Wert 1.

Hier ist die vollständige GridDemoPage.xaml-Datei ein:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.GridDemoPage"
             Title="Grid Demo Page">

    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
            <RowDefinition Height="100" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="100" />
        </Grid.ColumnDefinitions>

        <Label Text="Autosized cell"
               Grid.Row="0" Grid.Column="0"
               TextColor="White"
               BackgroundColor="Blue" />

        <BoxView Color="Silver"
                 HeightRequest="0"
                 Grid.Row="0" Grid.Column="1" />

        <BoxView Color="Teal"
                 Grid.Row="1" Grid.Column="0" />

        <Label Text="Leftover space"
               Grid.Row="1" Grid.Column="1"
               TextColor="Purple"
               BackgroundColor="Aqua"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />

        <Label Text="Span two rows (or more if you want)"
               Grid.Row="0" Grid.Column="2" Grid.RowSpan="2"
               TextColor="Yellow"
               BackgroundColor="Blue"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />

        <Label Text="Span two columns"
               Grid.Row="2" Grid.Column="0" Grid.ColumnSpan="2"
               TextColor="Blue"
               BackgroundColor="Yellow"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />

        <Label Text="Fixed 100x100"
               Grid.Row="2" Grid.Column="2"
               TextColor="Aqua"
               BackgroundColor="Red"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />

    </Grid>
</ContentPage>
```

Die `Grid.Row` und `Grid.Column` Einstellungen 0 sind nicht erforderlich, aber im Allgemeinen für Zwecke der Klarheit enthalten sind.

Hier ist es auf allen drei Plattformen illustriert:

[ ![](essential-xaml-syntax-images/griddemo.png "Rasterlayout")](essential-xaml-syntax-images/griddemo-large.png "Rasterlayout")

Ausschließlich von der Syntax Beurteilung diese `Grid.Row`, `Grid.Column`, `Grid.RowSpan`, und `Grid.ColumnSpan` Attribute angezeigt werden, um statische Felder oder Eigenschaften sein `Grid`, aber Interessanterweise `Grid` nichts mit dem Namen ist nicht definiert `Row`, `Column`, `RowSpan`, oder `ColumnSpan`.

Stattdessen `Grid` definiert vier bindbare Eigenschaften, die mit dem Namen `RowProperty`, `ColumnProperty`, `RowSpanProperty`, und `ColumnSpanProperty`. Dies sind spezielle Arten von bindbare Eigenschaften, die als bekannt *angefügte Eigenschaften*. Sie definiert sind, indem Sie die `Grid` "Class" jedoch festgelegt, auf die untergeordneten Elemente der `Grid`.

Wenn Sie diese verwenden möchten, angefügte Eigenschaften im Code, der `Grid` -Klasse stellt statische Methoden, die mit dem Namen `SetRow`, `GetColumn`usw. lauten. In XAML wird diese angefügten Eigenschaften sind jedoch festgelegt als Attribute in den untergeordneten Elementen des der `Grid` einfache Eigenschaften Namen verwenden.

Angefügte Eigenschaften sind immer erkennbar in XAML-Dateien als Attribute, die mit einer Klasse und einen Eigenschaftsnamen, die durch einen Punkt getrennt. Sie werden aufgerufen, *angefügte Eigenschaften* , da sie von einer Klasse definiert sind (in diesem Fall `Grid`) jedoch auf andere Objekte angefügt (in diesem Fall ist die untergeordneten Elemente der `Grid`). Während des Layouts der `Grid` können Sie die Werte dieser angefügte Eigenschaften wissen, wo Sie jedes untergeordnete Element platzieren Abfragen.

Die `AbsoluteLayout` Klasse definiert zwei angefügte Eigenschaften, die mit dem Namen `LayoutBounds` und `LayoutFlags`. Hier ist ein Schachbrettmuster realisiert, die proportional Positionierung und Sizing Funktionen von `AbsoluteLayout`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.AbsoluteDemoPage"
             Title="Absolute Demo Page">

    <AbsoluteLayout BackgroundColor="#FF8080">
        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0.33, 0, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="1, 0, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0, 0.33, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0.67, 0.33, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0.33, 0.67, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="1, 0.67, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0, 1, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0.67, 1, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

  </AbsoluteLayout>
</ContentPage>
```

Und hier ist:

[ ![](essential-xaml-syntax-images/absolutedemo-large.png "Absolute Layout")](essential-xaml-syntax-images/absolutedemo-large.png "Absolute Layout")

Für etwa so aussehen können Sie die Kenntnisse der Verwendung von XAML Frage. Sicherlich, der Wiederholung und Ordnungsmäßigkeit von der `LayoutBounds` Rechteck wird vorgeschlagen, die besser im Code realisiert werden kann.

Sicherlich legitimen relevant ist und kein Problem mit Lastenausgleich die Verwendung von Code und Markup beim Definieren Ihrer Benutzeroberflächen. Es ist einfach, einige der Visualisierungen in XAML zu definieren und verwenden Sie den Konstruktor der CodeBehind-Datei klicken Sie dann einige weitere visuelle Elemente hinzufügen, die eine bessere Leistung in Schleifen generiert werden kann.

## <a name="content-properties"></a>Content-Eigenschaften

In den vorherigen Beispielen die `StackLayout`, `Grid`, und `AbsoluteLayout` Objekte festgelegt die `Content` Eigenschaft von der `ContentPage`, und die untergeordneten Elemente für diese Layouts sind tatsächlich Elemente in der `Children` Auflistung. Noch diese `Content` und `Children` Eigenschaften sind an keiner Stelle in der XAML-Datei.

Sicherlich zählen die `Content` und `Children` Eigenschaften als Eigenschaftenelemente, wie die **XamlPlusCode** Beispiel:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.XamlPlusCodePage"
             Title="XAML + Code Page">
    <ContentPage.Content>
        <StackLayout>
            <StackLayout.Children>
                <Slider VerticalOptions="CenterAndExpand"
                        ValueChanged="OnSliderValueChanged" />

                <Label x:Name="valueLabel"
                       Text="A simple Label"
                       FontSize="Large"
                       HorizontalOptions="Center"
                       VerticalOptions="CenterAndExpand" />

                <Button Text="Click Me!"
                      HorizontalOptions="Center"
                      VerticalOptions="CenterAndExpand"
                      Clicked="OnButtonClicked" />
            </StackLayout.Children>
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Die eigentliche Frage ist: Warum diese Eigenschaftenelemente sind *nicht* in der XAML-Datei erforderlich?

Dürfen Elemente, die in Xamarin.Forms für die Verwendung in XAML definiert eine gekennzeichnete Eigenschaft haben die `ContentProperty` -Attribut für die Klasse. Wenn Sie Nachschlagen der `ContentPage` Klasse in der Onlinedokumentation von Xamarin.Forms sehen Sie dieses Attribut:

```csharp
[Xamarin.Forms.ContentProperty("Content")]
public class ContentPage : TemplatedPage
```

Dies bedeutet, dass die `Content` Property-Element Tags sind nicht erforderlich. Alle XML-Inhalte, die zwischen den Start- und Enddatum angezeigt wird `ContentPage` wird angenommen, dass Tags zugewiesen werden, die `Content` Eigenschaft.

 `StackLayout`, `Grid`, `AbsoluteLayout`, und `RelativeLayout` alle abgeleitet `Layout<View>`, und wenn Sie nachschlagen `Layout<T>` Xamarin.Forms-Dokumentation, sehen Sie in eine andere `ContentProperty` Attribut:

```csharp
[Xamarin.Forms.ContentProperty("Children")]
public abstract class Layout<T> : Layout ...
```

Mit dessen Hilfe die Inhalte des Layouts automatisch hinzugefügt werden die `Children` Auflistung ohne explizite `Children` Property-Element Tags.

Auch andere Klassen haben `ContentProperty` -Attribut Definitionen. Die Inhaltseigenschaft des `Label` ist `Text`. Überprüfen Sie die API-Dokumentation für andere.

## <a name="platform-differences-with-onplatform"></a>Plattformunterschiede mit OnPlatform

In einseitige Anwendungen, es ist üblich, legen Sie die `Padding` Eigenschaft auf der Seite, um zu vermeiden, überschreiben die iOS-Statusleiste. Im Code können Sie die `Device.RuntimePlatform` -Eigenschaft für diesen Zweck:

```csharp
if (Device.RuntimePlatform == Device.iOS)
{
    Padding = new Thickness(0, 20, 0, 0);
}
```

Sie auch einen ähnlichen Effekt erreichen in XAML mit dem `OnPlatform` und `On` Klassen. Zuerst enthalten Eigenschaftenelemente für die `Padding` Eigenschaft am oberen Rand der Seite ":

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>

    </ContentPage.Padding>
    ...
</ContentPage>
```

Innerhalb dieser Tags enthalten eine `OnPlatform` Tag. `OnPlatform` ist eine generische Klasse. Sie müssen das generische Typargument, in diesem Fall angeben `Thickness`, ist der Typ des `Padding` Eigenschaft. Glücklicherweise besteht ein XAML-Attribut speziell für das Definieren von generischer Arguments aufgerufen `x:TypeArguments`. Dies sollte der Typ der Eigenschaft entsprechen, die Sie festlegen können:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">

        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

`OnPlatform` verfügt über eine Eigenschaft mit dem Namen `Platforms` also ein `IList` von `On` Objekte. Verwenden Sie Eigenschaftselementtags für diese Eigenschaft:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <OnPlatform.Platforms>

            </OnPlatform.Platforms>
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

Nun fügen `On` Elemente. Legen Sie für jede Onem die `Platform` Eigenschaft und die `Value` Eigenschaft Markup für die `Thickness` Eigenschaft:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <OnPlatform.Platforms>
                <On Platform="iOS" Value="0, 20, 0, 0" />
                <On Platform="Android" Value="0, 0, 0, 0" />
                <On Platform="UWP" Value="0, 0, 0, 0" />
            </OnPlatform.Platforms>
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

Dieses Markup kann vereinfacht werden. Die Inhaltseigenschaft des `OnPlatform` ist `Platforms`, sodass diese Property-Element Tags entfernt werden können:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
            <On Platform="Android" Value="0, 0, 0, 0" />
            <On Platform="UWP" Value="0, 0, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

Die `Platform` Eigenschaft `On` ist vom Typ `IList<string>`, sodass Sie mehrere Plattformen einschließen können, wenn die Werte identisch sind:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
            <On Platform="Android, UWP" Value="0, 0, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

Da Android und Windows auf den Standardwert festgelegt sind `Padding`, die Tags entfernt werden kann:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

Dies ist die Standardmethode festzulegende eine plattformabhängige `Padding` Eigenschaft in XAML. Wenn die `Value` Einstellung kann nicht durch eine einzelne Zeichenfolge dargestellt werden, können Sie Eigenschaftenelemente dafür definieren:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS">
                <On.Value>
                    0, 20, 0, 0
                </On.Value>
            </On>
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

## <a name="summary"></a>Zusammenfassung

Mit Eigenschaftenelemente und angefügte Eigenschaften wurde ein Großteil der grundlegenden XAML-Syntax hergestellt. Allerdings müssen manchmal Sie Eigenschaften auf Objekte in einer indirekten Weise, z. B. über ein Ressourcenverzeichnis festgelegt. Dieser Ansatz wird in den nächsten Teil, Teil behandelt [3. Verwendung von XAML-Markuperweiterungen](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md).



## <a name="related-links"></a>Verwandte Links

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Teil 1. Erste Schritte mit XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [Teil 3. XAML-Markuperweiterungen](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Teil 4. Grundlagen der Datenbindung](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [Teil 5. Aus dem Datenbindung an MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
