---
title: 'Teil 2: Grundlegende XAML-Syntax'
description: In diesem Artikel wird erläutert, die wesentlichen Funktionen der XAML-Syntax von Eigenschaftenelementen und angefügte Eigenschaften.
ms.prod: xamarin
ms.assetid: 4022F1DC-3802-4635-A553-688ABD3F0D5A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2017
ms.openlocfilehash: f79a07a04eddeea1441f7938fdef210a37fb920a
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306572"
---
# <a name="part-2-essential-xaml-syntax"></a>Teil 2: Grundlegende XAML-Syntax

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)

_XAML ist größtenteils zum Instanziieren und Initialisieren von Objekten konzipiert. Häufig müssen Eigenschaften jedoch auf komplexe Objekte festgelegt werden, die nicht einfach als XML-Zeichen folgen dargestellt werden können. manchmal müssen Eigenschaften, die von einer Klasse definiert werden, für eine untergeordnete Klasse festgelegt werden. Diese beiden Anforderungen erfordern die wesentlichen XAML-Syntax Funktionen von Eigenschafts Elementen und angefügten Eigenschaften._

## <a name="property-elements"></a>Property-Elemente

In XAML werden die Eigenschaften von Klassen normalerweise als XML-Attribute festgelegt:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large"
       TextColor="Aqua" />
```

Es ist jedoch eine alternative Möglichkeit zum Festlegen einer Eigenschaft in XAML. Um diese Alternative mit `TextColor`zu testen, löschen Sie zuerst die vorhandene `TextColor` Einstellung:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large" />
```

Öffnen Sie das leere Element `Label` Tag, indem Sie es in Start-und Endtags aufteilen:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large">

</Label>
```

Fügen Sie innerhalb dieser Tags Start- und Endtags, die den Klassennamen und einen Eigenschaftsnamen, die durch einen Punkt getrennt bestehen:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large">
    <Label.TextColor>

    </Label.TextColor>
</Label>
```

Legen Sie den Wert der Eigenschaft als Inhalt von diesen neuen Tags wie folgt an:

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

Diese beiden Methoden zum Angeben der `TextColor`-Eigenschaft sind funktional äquivalent, verwenden jedoch nicht die zwei Methoden für die gleiche Eigenschaft, da dadurch die-Eigenschaft tatsächlich zweimal festgelegt werden kann und möglicherweise mehrdeutig ist.

Mit dieser neuen Syntax ist kann einige praktische Terminologie eingeführt werden:

- `Label` ist ein *Objekt Element*. Es ist ein Xamarin.Forms-Objekt, das als ein XML-Element.
- `Text`, `VerticalOptions`, `FontAttributes` und `FontSize` sind *Eigenschafts Attribute*. Xamarin.Forms-Eigenschaften, die als XML-Attribute angegeben werden.
- In diesem abschließenden Ausschnitt wurde `TextColor` zu einem *Eigenschaften Element*. Es ist eine Xamarin.Forms-Eigenschaft, aber es ist jetzt ein XML-Element.

Die Definition der Eigenschaft, die Elemente am möglicherweise zuerst anscheinend zu einer Verletzung der XML-Syntax, aber es ist nicht. Der Zeitraum hat keine besondere Bedeutung in XML-Datei. An einen XML-Decoder ist `Label.TextColor` einfach ein normales untergeordnetes Element.

In XAML ist diese Syntax jedoch sehr spezielle. Eine der Regeln für Eigenschafts Elemente besteht darin, dass im `Label.TextColor`-Tag nichts anderes vorkommen darf. Der Wert der Eigenschaft wird immer als Inhalt zwischen dem Eigenschaftenelement Start- und Endtags definiert.

Sie können auf mehr als eine Eigenschaft Eigenschaftenelement Syntax verwenden:

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

Sie können auch Eigenschaftenelement Syntax für alle Eigenschaften:

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

Auf den ersten Eigenschaftenelementsyntax mag eine unnötige langwieriger Ersatz für eine vergleichsweise einfach, und in diesen Beispielen ist sicherlich der Fall.

Eigenschaftenelement Syntax wird jedoch wichtig, wenn der Wert einer Eigenschaft zu komplex ist, als einfache Zeichenfolge ausgedrückt werden. Innerhalb der Property-Element Tags können Sie ein anderes Objekt zu instanziieren und seine Eigenschaften festlegen. Beispielsweise können Sie eine Eigenschaft wie z. b. `VerticalOptions` mit einem `LayoutOptions` Wert mit Eigenschafts Einstellungen explizit festlegen:

```xaml
<Label>
    ...
    <Label.VerticalOptions>
        <LayoutOptions Alignment="Center" />
    </Label.VerticalOptions>
</Label>
```

Ein weiteres Beispiel: der `Grid` verfügt über zwei Eigenschaften mit dem Namen `RowDefinitions` und `ColumnDefinitions`. Diese beiden Eigenschaften sind vom Typ `RowDefinitionCollection` und `ColumnDefinitionCollection`, bei denen es sich um Auflistungen von `RowDefinition`-und `ColumnDefinition` Objekten handelt. In diesem Fall müssen Sie eine Eigenschaftenelement-Syntax zu verwenden, um diese Sammlungen festzulegen.

Dies ist der Anfang der XAML-Datei für eine `GridDemoPage`-Klasse, die die Eigenschaften Element Tags für die `RowDefinitions`-und `ColumnDefinitions`-Auflistungen anzeigt:

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

Beachten Sie, dass die abgekürzte Syntax zum Definieren von automatischer Größenänderung von Zellen, Zellen Pixel Breiten und Höhen und star-Einstellungen.

## <a name="attached-properties"></a>Angefügte Eigenschaften

Sie haben gerade gesehen, dass die `Grid` Eigenschafts Elemente für die `RowDefinitions`-und `ColumnDefinitions` Auflistungen erfordert, um die Zeilen und Spalten zu definieren. Es muss jedoch auch eine Möglichkeit für den Programmierer geben, die Zeile und die Spalte anzugeben, in denen sich jedes untergeordnete Element des `Grid` befindet.

Innerhalb des-Tags für jedes untergeordnete Element des-`Grid` Sie die Zeile und die Spalte dieses untergeordneten Elements mithilfe der folgenden Attribute angeben:

- `Grid.Row`
- `Grid.Column`

Die Standardwerte dieser Attribute sind 0. Sie können auch angeben, wenn ein untergeordnetes Element über mehr als eine Zeile oder Spalte mit diesen Attributen umfasst:

- `Grid.RowSpan`
- `Grid.ColumnSpan`

Diese beiden Attribute haben Standardwerte 1.

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

Die `Grid.Row`-und `Grid.Column` Einstellungen von 0 sind nicht erforderlich, werden jedoch im Allgemeinen aus Gründen der Übersichtlichkeit berücksichtigt.

Hier ist, wie es aussieht:

[![Raster Layout](essential-xaml-syntax-images/griddemo.png)](essential-xaml-syntax-images/griddemo-large.png#lightbox)

Wenn Sie nur die Syntax beurteilen, werden diese `Grid.Row`-, `Grid.Column`-, `Grid.RowSpan`-und `Grid.ColumnSpan` Attribute als statische Felder oder Eigenschaften `Grid`angezeigt, aber interessanterweise definiert `Grid` nichts mit dem Namen `Row`, `Column`, `RowSpan`oder `ColumnSpan`.

Stattdessen definiert `Grid` vier bindbare Eigenschaften mit dem Namen `RowProperty`, `ColumnProperty`, `RowSpanProperty`und `ColumnSpanProperty`. Dies sind spezielle Typen von bindbaren Eigenschaften, die als *angefügte Eigenschaften*bezeichnet werden. Sie werden durch die `Grid`-Klasse definiert, aber auf untergeordnete Elemente der `Grid`festgelegt.

Wenn Sie diese angefügten Eigenschaften im Code verwenden möchten, stellt die `Grid`-Klasse statische Methoden mit dem Namen "`SetRow`", "`GetColumn`" usw. bereit. In XAML werden diese angefügten Eigenschaften jedoch als Attribute in den untergeordneten Elementen des `Grid` mit einfachen Eigenschaften Namen festgelegt.

Angefügte Eigenschaften sind immer erkennbar in XAML-Dateien als Attribute, die mit einer Klasse und einen Eigenschaftsnamen, die durch einen Punkt getrennt. Sie werden als *angefügte Eigenschaften* bezeichnet, da Sie von einer Klasse definiert werden (in diesem Fall `Grid`), aber an andere Objekte angefügt sind (in diesem Fall untergeordnete Elemente des `Grid`). Beim Layout können die `Grid` die Werte dieser angefügten Eigenschaften Abfragen, um zu ermitteln, wo die einzelnen untergeordneten Elemente platziert werden sollen.

Die `AbsoluteLayout`-Klasse definiert zwei angefügte Eigenschaften mit dem Namen `LayoutBounds` und `LayoutFlags`. Im folgenden finden Sie ein checkboard-Muster, das mithilfe der Features für die proportionale Positionierung und Größenanpassung von `AbsoluteLayout`realisiert

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

[![absolutes Layout](essential-xaml-syntax-images/absolutedemo-large.png)](essential-xaml-syntax-images/absolutedemo-large.png#lightbox)

Für einen solchen können Sie das Wissen der Verwendung von XAML Frage. Sicherlich deutet die Wiederholung und die Regelmäßigkeit des `LayoutBounds` Rechtecks darauf hin, dass Sie möglicherweise besser im Code realisiert werden.

Das ist sicherlich eine legitime Sorge, und besteht kein Problem für den Lastenausgleich der Verwendung von Code und Markup, wenn Sie Ihren Benutzeroberflächen zu definieren. Es ist einfach, einige der Visualisierungen in XAML zu definieren, und klicken Sie dann den Konstruktor der CodeBehind-Datei verwenden, um einige weitere visuelle Elemente hinzuzufügen, die besser in Schleifen ausgegeben werden.

## <a name="content-properties"></a>Content-Eigenschaften

In den vorherigen Beispielen werden die Objekte `StackLayout`, `Grid`und `AbsoluteLayout` auf die `Content`-Eigenschaft des `ContentPage`festgelegt, und die untergeordneten Elemente dieser Layouts sind tatsächlich Elemente in der `Children` Auflistung. Diese `Content` und `Children` Eigenschaften befinden sich jedoch nicht in der XAML-Datei.

Sie können die Eigenschaften "`Content`" und "`Children`" als Eigenschafts Elemente einschließen, wie z. b. im **xamlpluscode** -Beispiel:

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

Die eigentliche Frage ist: Warum sind diese Eigenschafts Elemente in der XAML-Datei *nicht* erforderlich?

Elemente, die in xamarin. Forms für die Verwendung in XAML definiert sind, dürfen eine Eigenschaft aufweisen, die im `ContentProperty`-Attribut der Klasse gekennzeichnet ist. Wenn Sie die `ContentPage`-Klasse in der Online Dokumentation zu xamarin. Forms nachschlagen, wird dieses Attribut angezeigt:

```csharp
[Xamarin.Forms.ContentProperty("Content")]
public class ContentPage : TemplatedPage
```

Dies bedeutet, dass die `Content`-Eigenschaften-Element-Tags nicht erforderlich sind. Alle XML-Inhalte, die zwischen dem Start-und dem End`ContentPage` Tags angezeigt werden, werden davon ausgegangen, dass Sie der `Content` Eigenschaft zugewiesen werden.

 `StackLayout`, `Grid`, `AbsoluteLayout`und `RelativeLayout` werden von `Layout<View>`abgeleitet, und wenn Sie in der xamarin. Forms-Dokumentation `Layout<T>` suchen, wird ein weiteres `ContentProperty` Attribut angezeigt:

```csharp
[Xamarin.Forms.ContentProperty("Children")]
public abstract class Layout<T> : Layout ...
```

Dadurch kann der Inhalt des Layouts automatisch der `Children` Auflistung ohne explizite `Children` Eigenschaften Element Tags hinzugefügt werden.

Andere Klassen verfügen auch über `ContentProperty` Attribut Definitionen. Beispielsweise ist die Content-Eigenschaft von `Label` `Text`. Überprüfen Sie die API-Dokumentation für andere Benutzer.

## <a name="platform-differences-with-onplatform"></a>Plattformunterschiede mit OnPlatform

In Single-Page-Anwendungen ist es üblich, die `Padding`-Eigenschaft auf der Seite festzulegen, um das Überschreiben der IOS-Statusleiste zu vermeiden. Im Code können Sie die `Device.RuntimePlatform`-Eigenschaft für diesen Zweck verwenden:

```csharp
if (Device.RuntimePlatform == Device.iOS)
{
    Padding = new Thickness(0, 20, 0, 0);
}
```

Sie können in XAML auch einen ähnlichen Vorgang ausführen, indem Sie die Klassen [`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) und [`On`](xref:Xamarin.Forms.On) verwenden. Fügen Sie zunächst Eigenschaften Elemente für die `Padding`-Eigenschaft am oberen Rand der Seite ein:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>

    </ContentPage.Padding>
    ...
</ContentPage>
```

Fügen Sie innerhalb dieser Tags ein `OnPlatform`-Tag ein. `OnPlatform` ist eine generische Klasse. Sie müssen das generische Typargument angeben, in diesem Fall `Thickness`, das den Typ der `Padding` Eigenschaft ist. Glücklicherweise gibt es ein XAML-Attribut speziell zum Definieren von generischen Argumenten namens "`x:TypeArguments`". Dies sollte dem Typ der Eigenschaft entsprechen, die Sie festlegen können:

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

`OnPlatform` verfügt über eine Eigenschaft mit dem Namen `Platforms`, die eine `IList` von `On` Objekten ist. Verwenden Sie Eigenschaftenelement-Tags für diese Eigenschaft:

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

Fügen Sie nun `On` Elemente hinzu. Legen Sie für jeden die `Platform`-Eigenschaft und die `Value`-Eigenschaft auf Markup für die `Thickness`-Eigenschaft fest:

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

Dieses Markup kann vereinfacht werden. Die Content-Eigenschaft von `OnPlatform` ist `Platforms`, sodass diese Eigenschaften Element Tags entfernt werden können:

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

Die `Platform`-Eigenschaft von `On` ist vom Typ `IList<string>`. Sie können also mehrere Plattformen einschließen, wenn die Werte identisch sind:

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

Da für Android und UWP der Standardwert `Padding`festgelegt ist, kann das Tag entfernt werden:

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

Dies ist die Standardmethode zum Festlegen einer Platt Form abhängigen `Padding`-Eigenschaft in XAML. Wenn die `Value` Einstellung nicht durch eine einzelne Zeichenfolge dargestellt werden kann, können Sie Eigenschafts Elemente dafür definieren:

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

> [!NOTE]
> Die `OnPlatform` Markup Erweiterung kann auch in XAML verwendet werden, um die Darstellung der Benutzeroberfläche plattformspezifisch anzupassen. Sie bietet die gleiche Funktionalität wie die `OnPlatform`-und `On` Klassen, bietet jedoch eine präzisere Darstellung. Weitere Informationen finden Sie unter [onplatform-Markup Erweiterung](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform).

## <a name="summary"></a>Zusammenfassung

Property-Elemente und angefügte Eigenschaften wurde ein Großteil der grundlegenden XAML-Syntax hergestellt. Allerdings müssen gelegentlich zum Festlegen von Eigenschaften zu Objekten auf indirekte Weise, z. B. aus einem Ressourcenverzeichnis. Diese Vorgehensweise wird im nächsten Teil, Teil 3, behandelt [. XAML-Markup Erweiterungen](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md).

## <a name="related-links"></a>Verwandte Links

- [Xamlsamples](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)
- [Teil 1: Einstieg in XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [Teil 3: XAML-Markup Erweiterungen](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Teil 4. Grundlagen der Datenbindung](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [Teil 5. Von der Datenbindung an MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
