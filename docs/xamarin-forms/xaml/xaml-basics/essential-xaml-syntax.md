---
title: Teil 2. Grundlegende XAML-Syntax
description: In diesem Artikel werden die wesentlichen XAML-Syntax Funktionen von Eigenschafts Elementen und angefügten Eigenschaften erläutert.
ms.prod: xamarin
ms.assetid: 4022F1DC-3802-4635-A553-688ABD3F0D5A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c400bb342568a0399e2a582496f85ead273b6994
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84572181"
---
# <a name="part-2-essential-xaml-syntax"></a>Teil 2. Grundlegende XAML-Syntax

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)

_XAML ist größtenteils zum Instanziieren und Initialisieren von Objekten konzipiert. Häufig müssen Eigenschaften jedoch auf komplexe Objekte festgelegt werden, die nicht einfach als XML-Zeichen folgen dargestellt werden können. manchmal müssen Eigenschaften, die von einer Klasse definiert werden, für eine untergeordnete Klasse festgelegt werden. Diese beiden Anforderungen erfordern die wesentlichen XAML-Syntax Funktionen von Eigenschafts Elementen und angefügten Eigenschaften._

## <a name="property-elements"></a>Eigenschaften Elemente

In XAML werden Eigenschaften von Klassen normalerweise als XML-Attribute festgelegt:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large"
       TextColor="Aqua" />
```

Es gibt jedoch eine alternative Möglichkeit, eine Eigenschaft in XAML festzulegen. Wenn Sie diese Alternative mit testen möchten `TextColor` , löschen Sie zuerst die vorhandene `TextColor` Einstellung:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large" />
```

Öffnen Sie das Tag "Empty-Element", `Label` indem Sie es in Start-und Endtags aufteilen:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large">

</Label>
```

Fügen Sie innerhalb dieser Tags Start-und Endtags hinzu, die aus dem Klassennamen und einem Eigenschaftsnamen bestehen, getrennt durch einen Zeitraum:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large">
    <Label.TextColor>

    </Label.TextColor>
</Label>
```

Legen Sie den Eigenschafts Wert als Inhalt dieser neuen Tags fest, wie folgt:

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

Diese zwei Möglichkeiten, die `TextColor` Eigenschaft anzugeben, sind funktional äquivalent, verwenden jedoch nicht die zwei Methoden für die gleiche Eigenschaft, da dadurch die Eigenschaft effektiv zweimal festgelegt werden kann und möglicherweise mehrdeutig ist.

Mit dieser neuen Syntax können einige praktische Terminologie eingeführt werden:

- `Label`ist ein *Objekt Element*. Es ist ein-Objekt, das Xamarin.Forms als XML-Element ausgedrückt wird.
- `Text`, `VerticalOptions` `FontAttributes` und `FontSize` sind *Eigenschafts Attribute*. Sie sind Xamarin.Forms Eigenschaften, die als XML-Attribute ausgedrückt werden.
- In diesem abschließenden Code Ausschnitt `TextColor` wurde zu einem *Eigenschafts Element*. Dabei handelt es sich um eine Xamarin.Forms Eigenschaft, aber es handelt sich um ein XML-Element.

Die Definition von Eigenschafts Elementen scheint zunächst eine Verletzung der XML-Syntax zu sein, aber das ist nicht der Fall. Der Zeitraum hat keine besondere Bedeutung in XML. Bei einem XML-Decoder `Label.TextColor` ist einfach ein normales untergeordnetes Element.

In XAML ist diese Syntax jedoch sehr speziell. Eine der Regeln für Eigenschafts Elemente besteht darin, dass im Tag nichts anderes vorkommen darf `Label.TextColor` . Der Wert der Eigenschaft wird immer als Inhalt zwischen den Start-und Endtags des Eigenschaften Elements definiert.

Sie können die Eigenschafts Element Syntax für mehr als eine Eigenschaft verwenden:

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

Oder Sie können die Eigenschafts Element-Syntax für alle Eigenschaften verwenden:

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

Zuerst könnte die Syntax der Eigenschaften Elemente für etwas, das vergleichsweise relativ einfach ist, ein nicht benötigter, langer Windowstext sein, und in diesen Beispielen ist dies sicherlich der Fall.

Die Syntax von Eigenschafts Elementen ist jedoch unverzichtbar, wenn der Wert einer Eigenschaft zu komplex ist, um als einfache Zeichenfolge ausgedrückt zu werden. Innerhalb der Eigenschaften-Element-Tags können Sie ein anderes Objekt instanziieren und seine Eigenschaften festlegen. Beispielsweise können Sie eine Eigenschaft wie z. b. explizit `VerticalOptions` auf einen `LayoutOptions` Wert mit Eigenschafts Einstellungen festlegen:

```xaml
<Label>
    ...
    <Label.VerticalOptions>
        <LayoutOptions Alignment="Center" />
    </Label.VerticalOptions>
</Label>
```

Ein weiteres Beispiel: der `Grid` verfügt über zwei Eigenschaften mit dem Namen `RowDefinitions` und `ColumnDefinitions` . Diese beiden Eigenschaften sind vom Typ `RowDefinitionCollection` und `ColumnDefinitionCollection` . dabei handelt es sich um Auflistungen von `RowDefinition` -und- `ColumnDefinition` Objekten. Sie müssen die Eigenschafts Element Syntax verwenden, um diese Auflistungen festzulegen.

Im folgenden finden Sie den Anfang der XAML-Datei für eine `GridDemoPage` Klasse, in der die Eigenschaften Element Tags für die-Auflistung und die-Auflistung angezeigt werden `RowDefinitions` `ColumnDefinitions` :

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

Beachten Sie die abgekürzte Syntax zum Definieren von Zellen mit automatischer Größe, Zellen mit Pixel breiten und Höhen sowie Stern Einstellungen.

## <a name="attached-properties"></a>Angefügte Eigenschaften

Sie haben gerade gesehen, dass `Grid` für die-Auflistung und die-Auflistung Eigenschaften Elemente erforderlich sind `RowDefinitions` `ColumnDefinitions` , um die Zeilen und Spalten zu definieren. Es muss jedoch auch eine Möglichkeit für den Programmierer geben, die Zeile und die Spalte anzugeben, in denen sich die einzelnen untergeordneten Elemente des befinden `Grid` .

Innerhalb des-Tags für jedes untergeordnete Element von `Grid` Geben Sie die Zeile und die Spalte dieses untergeordneten Elements mithilfe der folgenden Attribute an:

- `Grid.Row`
- `Grid.Column`

Die Standardwerte dieser Attribute sind 0. Sie können auch angeben, ob ein untergeordnetes Element mehr als eine Zeile oder Spalte mit diesen Attributen umfasst:

- `Grid.RowSpan`
- `Grid.ColumnSpan`

Diese beiden Attribute verfügen über die Standardwerte 1.

Hier ist die vollständige Datei "griddemopage. XAML":

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

Die `Grid.Row` -Einstellung und die- `Grid.Column` Einstellung 0 sind nicht erforderlich, werden jedoch im Allgemeinen aus Gründen der Übersichtlichkeit berücksichtigt.

Dies sieht wie folgt aus:

[![Rasterlayout](essential-xaml-syntax-images/griddemo.png)](essential-xaml-syntax-images/griddemo-large.png#lightbox)

Wenn Sie nur die-Syntax beurteilen `Grid.Row` , `Grid.Column` werden diese Attribute,, `Grid.RowSpan` und als `Grid.ColumnSpan` statische Felder oder Eigenschaften von angezeigt `Grid` , aber interessanterweise `Grid` definiert nichts namens `Row` , `Column` , `RowSpan` oder `ColumnSpan` .

Stattdessen `Grid` definiert vier bindbare Eigenschaften mit den Namen `RowProperty` , `ColumnProperty` , `RowSpanProperty` und `ColumnSpanProperty` . Dies sind spezielle Typen von bindbaren Eigenschaften, die als *angefügte Eigenschaften*bezeichnet werden. Sie werden von der-Klasse definiert, aber auf untergeordnete Elemente `Grid` von festgelegt `Grid` .

Wenn Sie diese angefügten Eigenschaften im Code verwenden möchten, `Grid` stellt die-Klasse statische Methoden mit dem Namen `SetRow` , `GetColumn` usw. bereit. In XAML werden diese angefügten Eigenschaften jedoch als Attribute in den untergeordneten Elementen von `Grid` mit einfachen Eigenschaften Namen festgelegt.

Angefügte Eigenschaften können in XAML-Dateien immer als Attribute erkannt werden, die eine Klasse und einen Eigenschaftsnamen enthalten, die durch einen bestimmten Zeitraum getrennt sind. Sie werden als *angefügte Eigenschaften* bezeichnet, da Sie von einer Klasse definiert werden (in diesem Fall `Grid` ), aber an andere Objekte angefügt sind (in diesem Fall untergeordnete Elemente von `Grid` ). Während des Layouts `Grid` kann die Werte dieser angefügten Eigenschaften Abfragen, um zu ermitteln, wo die einzelnen untergeordneten Elemente platziert werden.

Die `AbsoluteLayout` -Klasse definiert zwei angefügte Eigenschaften mit dem Namen `LayoutBounds` und `LayoutFlags` . Im folgenden finden Sie ein checkboard-Muster, das mithilfe der proportionalen Positions-und Größen Anpassungs `AbsoluteLayout` Funktionen von realisiert wird

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

Und hier ist es:

[![Absolutes Layout](essential-xaml-syntax-images/absolutedemo-large.png)](essential-xaml-syntax-images/absolutedemo-large.png#lightbox)

In etwa so können Sie die Verwendung von XAML hinterfragen. Sicherlich deutet die Wiederholung und die Regelmäßigkeit des `LayoutBounds` Rechtecks darauf hin, dass es im Code besser realisiert werden kann.

Das ist sicherlich ein legitimer Anliegen, und es gibt kein Problem mit dem Ausgleich der Verwendung von Code und Markup beim Definieren der Benutzeroberflächen. Es ist einfach, einige der visuellen Elemente in XAML zu definieren und dann den Konstruktor der Code Behind-Datei zu verwenden, um weitere visuelle Elemente hinzuzufügen, die möglicherweise besser in Schleifen generiert werden.

## <a name="content-properties"></a>Content-Eigenschaften

In den vorherigen Beispielen werden die `StackLayout` `Grid` Objekte, und `AbsoluteLayout` auf die `Content` -Eigenschaft von festgelegt `ContentPage` , und die untergeordneten Elemente dieser Layouts sind tatsächlich Elemente in der `Children` -Auflistung. Diese `Content` -und- `Children` Eigenschaften sind jedoch nicht in der XAML-Datei gespeichert.

Sie können die `Content` -Eigenschaft und die- `Children` Eigenschaft als Eigenschaften Elemente einschließen, wie z. b. im **xamlpluscode** -Beispiel:

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

Xamarin.FormsFür Elemente, die in für die Verwendung in XAML definiert sind, darf eine Eigenschaft im-Attribut der-Klasse gekennzeichnet werden `ContentProperty` . Wenn Sie die `ContentPage` -Klasse in der Online Dokumentation nachschlagen Xamarin.Forms , wird dieses Attribut angezeigt:

```csharp
[Xamarin.Forms.ContentProperty("Content")]
public class ContentPage : TemplatedPage
```

Dies bedeutet, dass die `Content` Eigenschaften Element Tags nicht erforderlich sind. Jeder XML-Inhalt, der zwischen den Start-und `ContentPage` Endtags angezeigt wird, wird davon ausgegangen, dass er der-Eigenschaft zugewiesen wird `Content` .

 `StackLayout`, `Grid` , `AbsoluteLayout` und `RelativeLayout` werden von abgeleitet `Layout<View>` , und wenn Sie `Layout<T>` in der Dokumentation nachschlagen Xamarin.Forms , wird ein weiteres Attribut angezeigt `ContentProperty` :

```csharp
[Xamarin.Forms.ContentProperty("Children")]
public abstract class Layout<T> : Layout ...
```

, Mit dem Inhalt des Layouts automatisch der Auflistung `Children` ohne explizite Eigenschaften Element Tags hinzugefügt werden kann `Children` .

Andere Klassen verfügen auch über `ContentProperty` Attribut Definitionen. Die Content-Eigenschaft von lautet z `Label` `Text` . b.. Weitere Informationen finden Sie in der API-Dokumentation.

## <a name="platform-differences-with-onplatform"></a>Platt Form Unterschiede mit onplatform

In Single-Page-Anwendungen ist es üblich, die- `Padding` Eigenschaft auf der Seite festzulegen, um das Überschreiben der IOS-Statusleiste zu vermeiden. Im Code können Sie die- `Device.RuntimePlatform` Eigenschaft für diesen Zweck verwenden:

```csharp
if (Device.RuntimePlatform == Device.iOS)
{
    Padding = new Thickness(0, 20, 0, 0);
}
```

Mit der [`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) -Klasse und der-Klasse können Sie auch in XAML ähnliche Aktionen ausführen [`On`](xref:Xamarin.Forms.On) . Fügen Sie zunächst Eigenschaften Elemente für die `Padding` Eigenschaft am oberen Rand der Seite ein:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>

    </ContentPage.Padding>
    ...
</ContentPage>
```

Fügen Sie innerhalb dieser Tags ein- `OnPlatform` Tag ein. `OnPlatform`ist eine generische Klasse. Sie müssen das generische Typargument angeben. in diesem Fall ist dies `Thickness` der Typ der `Padding` Eigenschaft. Glücklicherweise gibt es ein XAML-Attribut speziell zum Definieren von generischen Argumenten mit dem Namen `x:TypeArguments` . Dies sollte dem Typ der Eigenschaft entsprechen, die Sie festlegen:

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

`OnPlatform`verfügt über eine Eigenschaft `Platforms` mit dem Namen, die eine `IList` von-Objekten ist `On` . Eigenschaften Element Tags für diese Eigenschaft verwenden:

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

Fügen Sie jetzt `On` Elemente hinzu. Legen Sie für jede Eigenschaft die `Platform` -Eigenschaft und die- `Value` Eigenschaft auf Markup für die- `Thickness` Eigenschaft fest:

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

Dieses Markup kann vereinfacht werden. Die Content-Eigenschaft von `OnPlatform` ist `Platforms` , sodass diese Eigenschaften-Element-Tags entfernt werden können:

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

Die- `Platform` Eigenschaft von `On` ist vom Typ. `IList<string>` Sie können also mehrere Plattformen einschließen, wenn die Werte identisch sind:

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

Da für Android und UWP der Standardwert festgelegt `Padding` ist, kann dieses Tag entfernt werden:

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

Dies ist die Standardmethode zum Festlegen einer Platt Form abhängigen `Padding` Eigenschaft in XAML. Wenn die `Value` Einstellung nicht durch eine einzelne Zeichenfolge dargestellt werden kann, können Sie Eigenschafts Elemente dafür definieren:

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
> Die `OnPlatform` Markup Erweiterung kann auch in XAML verwendet werden, um die Darstellung der Benutzeroberfläche plattformspezifisch anzupassen. Sie bietet die gleiche Funktionalität wie die `OnPlatform` -Klasse und die- `On` Klasse, jedoch mit einer präziseren Darstellung. Weitere Informationen finden Sie unter [onplatform-Markup Erweiterung](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform-markup-extension).

## <a name="summary"></a>Zusammenfassung

Bei Eigenschafts Elementen und angefügten Eigenschaften wurde ein Großteil der grundlegenden XAML-Syntax erstellt. Manchmal müssen Sie jedoch Eigenschaften auf indirekte Weise auf Objekte festlegen, z. b. aus einem Ressourcen Wörterbuch. Diese Vorgehensweise wird im nächsten Teil, Teil 3, behandelt [. XAML-Markup Erweiterungen](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md).

## <a name="related-links"></a>Verwandte Links

- [Xamlsamples](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)
- [Teil 1: Einstieg in XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [Teil 3: XAML-Markup Erweiterungen](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Teil 4. Grundlagen der Datenbindung](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [Teil 5. Von der Datenbindung an MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
