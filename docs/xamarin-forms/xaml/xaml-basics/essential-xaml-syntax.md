---
title: Teil 2. Essential XAML Syntax
description: In diesem Artikel wird erläutert, die wesentlichen Funktionen der XAML-Syntax von Eigenschaftenelementen und angefügte Eigenschaften.
ms.prod: xamarin
ms.assetid: 4022F1DC-3802-4635-A553-688ABD3F0D5A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2017
ms.openlocfilehash: b55c9d8a65dbb4e44605295043d1b302295030ce
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2019
ms.locfileid: "70228063"
---
# <a name="part-2-essential-xaml-syntax"></a>Teil 2. Essential XAML Syntax

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)

_XAML ist hauptsächlich für das Instanziieren und Initialisieren von Objekten ausgelegt. Aber häufig Eigenschaften müssen festgelegt werden, bis zu komplexen Objekten, die einfach als XML-Zeichenfolgen dargestellt werden können, und manchmal auf eine untergeordnete Klasse von einer Klasse definierte Eigenschaften festgelegt werden müssen. Diese beiden Anforderungen erfordern die wesentlichen Funktionen der XAML-Syntax von Eigenschaftenelementen und angefügte Eigenschaften._

## <a name="property-elements"></a>Property-Elemente

In XAML werden die Eigenschaften von Klassen normalerweise als XML-Attribute festgelegt:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large"
       TextColor="Aqua" />
```

Es ist jedoch eine alternative Möglichkeit zum Festlegen einer Eigenschaft in XAML. Probieren Sie diese Alternative mit `TextColor`, löschen Sie zuerst das vorhandene `TextColor` Einstellung:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large" />
```

Öffnen Sie das leere Element `Label` Tag, indem Sie es in Start- und Endtags zu trennen:

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

Diese beiden Methoden zum Angeben der `TextColor` Eigenschaft sind funktional äquivalent, aber nicht die beiden Möglichkeiten für die gleiche Eigenschaft verwenden, da, die effektiv werden der Eigenschaft zweimal festlegen würde und mehrdeutig ist.

Mit dieser neuen Syntax ist kann einige praktische Terminologie eingeführt werden:

- `Label` ist ein *Objektelement*. Es ist ein Xamarin.Forms-Objekt, das als ein XML-Element.
- `Text`, `VerticalOptions`, `FontAttributes` und `FontSize` sind *Eigenschaftenattribute*. Xamarin.Forms-Eigenschaften, die als XML-Attribute angegeben werden.
- Im letzten Ausschnitts `TextColor` geworden ist eine *Property-Element*. Es ist eine Xamarin.Forms-Eigenschaft, aber es ist jetzt ein XML-Element.


Die Definition der Eigenschaft, die Elemente am möglicherweise zuerst anscheinend zu einer Verletzung der XML-Syntax, aber es ist nicht. Der Zeitraum hat keine besondere Bedeutung in XML-Datei. Ein XML-Decoder `Label.TextColor` einfach ein normales untergeordnetes Element.

In XAML ist diese Syntax jedoch sehr spezielle. Die Regeln für Eigenschaftenelemente ist, dass nichts in angezeigt werden, kann die `Label.TextColor` Tag. Der Wert der Eigenschaft wird immer als Inhalt zwischen dem Eigenschaftenelement Start- und Endtags definiert.

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

Eigenschaftenelement Syntax wird jedoch wichtig, wenn der Wert einer Eigenschaft zu komplex ist, als einfache Zeichenfolge ausgedrückt werden. Innerhalb der Property-Element Tags können Sie ein anderes Objekt zu instanziieren und seine Eigenschaften festlegen. Angenommen, Sie können explizit festlegen eine Eigenschaft wie z. B. `VerticalOptions` auf eine `LayoutOptions` Wert mit eigenschafteneinstellungen:

```xaml
<Label>
    ...
    <Label.VerticalOptions>
        <LayoutOptions Alignment="Center" />
    </Label.VerticalOptions>
</Label>
```

Ein weiteres Beispiel: Der `Grid` verfügt über zwei Eigenschaften `RowDefinitions` mit `ColumnDefinitions`dem Namen und. Diese beiden Eigenschaften sind vom Typ `RowDefinitionCollection` und `ColumnDefinitionCollection`, der es sich um Auflistungen von `RowDefinition` und `ColumnDefinition` Objekte. In diesem Fall müssen Sie eine Eigenschaftenelement-Syntax zu verwenden, um diese Sammlungen festzulegen.

Hier ist der Anfang der XAML-Datei für eine `GridDemoPage` -Klasse, mit dem Eigenschaftenelement-Tags für die `RowDefinitions` und `ColumnDefinitions` Sammlungen:

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

Sie haben gerade gesehen, die die `Grid` erfordert Eigenschaftenelemente für die `RowDefinitions` und `ColumnDefinitions` Sammlungen, um die Zeilen und Spalten definieren. Allerdings gibt es zudem muss eine Möglichkeit für den Programmierer an der Zeile und Spalte, in dem jedes untergeordnete Element von der `Grid` befindet.

Innerhalb des Tags für jedes untergeordnete Element von der `Grid` Geben Sie die Zeile und Spalte mit diesem untergeordneten mithilfe der folgenden Attribute:

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

Die `Grid.Row` und `Grid.Column` Einstellungen 0 sind nicht erforderlich, aber in der Regel befinden sich zur Veranschaulichung.

Hier ist, wie es aussieht:

[![Raster Layout](essential-xaml-syntax-images/griddemo.png)](essential-xaml-syntax-images/griddemo-large.png#lightbox)

Allein aufgrund der Syntax diese `Grid.Row`, `Grid.Column`, `Grid.RowSpan`, und `Grid.ColumnSpan` Attribute angezeigt werden, um statische Felder oder Eigenschaften sein `Grid`, aber es ist interessant, `Grid` definiert keine nichts mit dem Namen `Row`, `Column`, `RowSpan`, oder `ColumnSpan`.

Stattdessen `Grid` definiert vier bindbare Eigenschaften, die mit dem Namen `RowProperty`, `ColumnProperty`, `RowSpanProperty`, und `ColumnSpanProperty`. Hierbei handelt es sich um spezielle Typen von bindbare Eigenschaften, die als bekannt *angefügte Eigenschaften*. Sie definiert sind, durch die `Grid` Klasse jedoch festgelegt, auf die untergeordneten Elemente des der `Grid`.

Wenn Sie diese verwenden möchten, angefügte Eigenschaften in Code, der `Grid` -Klasse stellt statische Methoden, die mit dem Namen `SetRow`, `GetColumn`und so weiter. Aber in XAML, diese angefügten Eigenschaften festgelegt sind, als Attribute in der untergeordneten Elemente der `Grid` mithilfe von einfachen Namen.

Angefügte Eigenschaften sind immer erkennbar in XAML-Dateien als Attribute, die mit einer Klasse und einen Eigenschaftsnamen, die durch einen Punkt getrennt. Heißen sie *angefügte Eigenschaften* , da sie von einer Klasse definiert werden (in diesem Fall `Grid`) jedoch auf andere Objekte angefügt (in diesem Fall ist die untergeordneten Elemente der `Grid`). Während des Layouts der `Grid` können Sie die Werte der wissen, wo jedes untergeordnete Element platzieren diese angefügten Eigenschaften Abfragen.

Die `AbsoluteLayout` -Klasse definiert zwei angefügte Eigenschaften, die mit dem Namen `LayoutBounds` und `LayoutFlags`. Hier ist ein Schachbrettmuster realisiert, die proportional Positionierung und größenanpassung-Funktionen von `AbsoluteLayout`:

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

[![Absolutes Layout](essential-xaml-syntax-images/absolutedemo-large.png)](essential-xaml-syntax-images/absolutedemo-large.png#lightbox)

Für einen solchen können Sie das Wissen der Verwendung von XAML Frage. Natürlich die Wiederholung und regelmäßig aus der die `LayoutBounds` Rechteck zeigt an, dass es besser im Code realisiert werden kann.

Das ist sicherlich eine legitime Sorge, und besteht kein Problem für den Lastenausgleich der Verwendung von Code und Markup, wenn Sie Ihren Benutzeroberflächen zu definieren. Es ist einfach, einige der Visualisierungen in XAML zu definieren, und klicken Sie dann den Konstruktor der CodeBehind-Datei verwenden, um einige weitere visuelle Elemente hinzuzufügen, die besser in Schleifen ausgegeben werden.

## <a name="content-properties"></a>Content-Eigenschaften

In den vorherigen Beispielen die `StackLayout`, `Grid`, und `AbsoluteLayout` Objekte festgelegt die `Content` Eigenschaft der `ContentPage`, und die untergeordneten Elemente diese Layouts sind tatsächlich die Elemente in der `Children` Auflistung. Aber diese `Content` und `Children` Eigenschaften sind an keiner Stelle in der XAML-Datei.

Sie können sicherlich einschließen der `Content` und `Children` Eigenschaften als Eigenschaftenelemente, wie im der **XamlPlusCode** Beispiel:

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

Dürfen Elemente, die für die Verwendung in XAML in Xamarin.Forms definiert eine Eigenschaft, die in gekennzeichnet haben die `ContentProperty` -Attribut in der Klasse. Wenn Sie Nachschlagen der `ContentPage` Klasse in der Onlinedokumentation für die Xamarin.Forms, sehen Sie dieses Attribut:

```csharp
[Xamarin.Forms.ContentProperty("Content")]
public class ContentPage : TemplatedPage
```

Dies bedeutet, dass die `Content` Eigenschaftenelement Tags sind nicht erforderlich. XML-Inhalt, der zwischen Beginn und Ende `ContentPage` wird davon ausgegangen, dass Tags zugewiesen werden die `Content` Eigenschaft.

 `StackLayout`, `Grid`, `AbsoluteLayout`, und `RelativeLayout` alle abgeleitet `Layout<View>`, und wenn Sie nachschlagen `Layout<T>` in der Xamarin.Forms-Dokumentation, sehen Sie, eine andere `ContentProperty` Attribut:

```csharp
[Xamarin.Forms.ContentProperty("Children")]
public abstract class Layout<T> : Layout ...
```

Mit dem Inhalt des Layouts automatisch hinzugefügt werden die `Children` Auflistung ohne explizite `Children` Eigenschaftenelement Tags.

Andere Klassen auch haben `ContentProperty` Attribut Definitionen. Z. B. die Content-Eigenschaft des `Label` ist `Text`. Überprüfen Sie die API-Dokumentation für andere Benutzer.

## <a name="platform-differences-with-onplatform"></a>Plattformunterschiede mit OnPlatform

In einseitigen Anwendungen, es ist üblich, legen Sie die `Padding` Eigenschaft auf der Seite die Statusleiste iOS überschreibungsänderungen vermeiden. Im Code können Sie die `Device.RuntimePlatform` -Eigenschaft für diesen Zweck:

```csharp
if (Device.RuntimePlatform == Device.iOS)
{
    Padding = new Thickness(0, 20, 0, 0);
}
```

Mit der-Klasse und der [`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) - [`On`](xref:Xamarin.Forms.On) Klasse können Sie auch in XAML ähnliche Aktionen ausführen. Erster eingeschlossen Eigenschaftenelemente für die `Padding` Eigenschaft am oberen Rand der Seite:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>

    </ContentPage.Padding>
    ...
</ContentPage>
```

Innerhalb dieser Tags sind eine `OnPlatform` Tag. `OnPlatform` ist eine generische Klasse. Sie müssen das generische Typargument, in diesem Fall angeben `Thickness`, ist der Typ des `Padding` Eigenschaft. Es besteht jedoch ein XAML-Attribut, speziell für das Definieren von generischer Arguments aufgerufen `x:TypeArguments`. Dies sollte dem Typ der Eigenschaft entsprechen, die Sie festlegen können:

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

`OnPlatform` verfügt über eine Eigenschaft mit dem Namen `Platforms` d. h. eine `IList` von `On` Objekte. Verwenden Sie Eigenschaftenelement-Tags für diese Eigenschaft:

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

Fügen Sie jetzt hinzu `On` Elemente. Legen Sie für jede der `Platform` Eigenschaft und die `Value` Eigenschaft Markup für die `Thickness` Eigenschaft:

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

Dieses Markup kann vereinfacht werden. Die Inhaltseigenschaft `OnPlatform` ist `Platforms`, sodass diese Eigenschaftenelement Tags entfernt werden können:

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

Die `Platform` Eigenschaft `On` ist vom Typ `IList<string>`, sodass Sie mehrere Plattformen aufnehmen können, wenn die Werte identisch sind:

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

Da es sich bei Android und UWP auf den Standardwert festgelegt sind `Padding`, Tag, die entfernt werden kann:

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

Dies ist die Standardmethode zum legen Sie einer plattformabhängige `Padding` -Eigenschaft in XAML. Wenn die `Value` Einstellung kann nicht durch eine einzelne Zeichenfolge dargestellt werden, können Sie definieren Eigenschaftenelemente für sie:

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
> Die `OnPlatform` Markup Erweiterung kann auch in XAML verwendet werden, um die Darstellung der Benutzeroberfläche plattformspezifisch anzupassen. Sie bietet die gleiche Funktionalität wie die `OnPlatform` - `On` Klasse und die-Klasse, jedoch mit einer präziseren Darstellung. Weitere Informationen finden Sie unter [OnPlatform-Markuperweiterung](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform).

## <a name="summary"></a>Zusammenfassung

Property-Elemente und angefügte Eigenschaften wurde ein Großteil der grundlegenden XAML-Syntax hergestellt. Allerdings müssen gelegentlich zum Festlegen von Eigenschaften zu Objekten auf indirekte Weise, z. B. aus einem Ressourcenverzeichnis. Dieser Ansatz wird im nächsten Teil Teil behandelt [3. XAML-Markuperweiterungen](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md).

## <a name="related-links"></a>Verwandte Links

- [XamlSamples](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)
- [Teil 1. Erste Schritte mit XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [Teil 3. XAML-Markuperweiterungen](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Teil 4. Grundlagen der Datenbindung](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [Teil 5. Aus einer Datenbindung zu MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
