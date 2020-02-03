---
title: Formatieren von Xamarin.Forms-Apps, die mithilfe von Cascading Stylesheets (CSS)
description: Xamarin.Forms unterstützt visuelle Stile-Elemente mithilfe von Cascading Stylesheets (CSS).
ms.prod: xamarin
ms.assetid: C89D57A6-DAB9-4C42-963F-26D67627DDC2
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 11/04/2019
ms.openlocfilehash: 726ebd55b38460ee966113e4ee487327cd42b03d
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724193"
---
# <a name="styling-xamarinforms-apps-using-cascading-style-sheets-css"></a>Formatieren von Xamarin.Forms-apps, die mithilfe von Cascading Stylesheets (CSS)

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-monkeyappcss)

_Xamarin. Forms unterstützt das Formatieren visueller Elemente mithilfe Cascading Stylesheets (CSS)._

Xamarin.Forms-Anwendungen können mit CSS formatiert werden. Ein Stylesheet besteht aus einer Liste von Regeln, mit jeder Regel, die eine oder mehrere Auswahlzeiger und einen Deklarationsblock bestehen. Ein Deklarationsblock besteht aus einer Liste von Deklarationen in geschweiften Klammern, jede Deklaration einer Eigenschaft, einen Doppelpunkt und einen Wert aus. Wenn mehrere Deklarationen in einem Block vorhanden sind, wird eine durch Semikolons als Trennzeichen eingefügt. Im folgenden Codebeispiel wird veranschaulicht, Xamarin.Forms kompatibel CSS:

```css
navigationpage {
    -xf-bar-background-color: lightgray;
}

^contentpage {
    background-color: lightgray;
}

#listView {
    background-color: lightgray;
}

stacklayout {
    margin: 20;
}

.mainPageTitle {
    font-style: bold;
    font-size: medium;
}

.mainPageSubtitle {
    margin-top: 15;
}

.detailPageTitle {
    font-style: bold;
    font-size: medium;
    text-align: center;
}

.detailPageSubtitle {
    text-align: center;
    font-style: italic;
}

listview image {
    height: 60;
    width: 60;
}

stacklayout>image {
    height: 200;
    width: 200;
}
```

In Xamarin.Forms werden CSS-Stylesheets analysiert und Stylesheets werden erneut analysiert, bei der Verwendung zur Laufzeit statt der Zeitpunkt der Kompilierung ausgewertet.

> [!NOTE]
> Derzeit können nicht alle Teil des Designs, die mit XAML-Formatierung kann mit CSS ausgeführt werden. Allerdings können der XAML-Stile verwendet werden, CSS für Eigenschaften zu ergänzen, die derzeit von Xamarin.Forms nicht unterstützt werden. Weitere Informationen zu XAML-Formatvorlagen finden Sie unter [Formatieren von Xamarin.Forms-Apps mithilfe von XAML-Formatvorlagen](~/xamarin-forms/user-interface/styles/xaml/index.md).

Das [monkeyappcss](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-monkeyappcss) -Beispiel veranschaulicht die Verwendung von CSS zum Formatieren einer einfachen APP und ist in den folgenden Screenshots dargestellt:

[![Monkeyapp-Hauptseite mit CSS-Formatierung](css-images/MonkeyAppMainPage.png "Monkeyapp-Hauptseite mit CSS-Formatierung")](css-images/MonkeyAppMainPage-Large.png#lightbox "Monkeyapp-Hauptseite mit CSS-Formatierung")

[![Monkeyapp-Detail Seite mit CSS-Formatierung](css-images/MonkeyAppDetailPage.png "Monkeyapp-Detail Seite mit CSS-Formatierung")](css-images/MonkeyAppDetailPage-Large.png#lightbox "Monkeyapp-Detail Seite mit CSS-Formatierung")

## <a name="consuming-a-style-sheet"></a>Nutzen ein Stylesheet

Der Prozess zum Hinzufügen eines Stylesheets zu einer Lösung lautet wie folgt aus:

1. Fügen Sie eine leere CSS-Datei zu Ihrem Projekt für .NET Standard-Bibliothek hinzu.
1. Legen Sie die Buildaktion der CSS-Datei auf **EmbeddedResource**fest.

### <a name="loading-a-style-sheet"></a>Beim Laden eines Stylesheets

Es gibt eine Reihe von Ansätzen, die zum Laden eines Stylesheets verwendet werden kann.

### <a name="xaml"></a>XAML

Ein Stylesheet kann geladen und mit der [`StyleSheet`](xref:Xamarin.Forms.StyleSheets.StyleSheet) -Klasse analysiert werden, bevor es zu einem [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)hinzugefügt wird:

```xaml
<Application ...>
    <Application.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </Application.Resources>
</Application>
```

Die [`StyleSheet.Source`](xref:Xamarin.Forms.Xaml.StyleSheetExtension.Source) -Eigenschaft gibt das Stylesheet als URI relativ zum Speicherort der einschließenden XAML-Datei oder relativ zum Projektstamm an, wenn der URI mit einem `/`beginnt.

> [!WARNING]
> Die CSS-Datei kann nicht geladen werden, wenn die Buildaktion nicht auf **EmbeddedResource**festgelegt ist.

Alternativ kann ein Stylesheet mit der [`StyleSheet`](xref:Xamarin.Forms.StyleSheets.StyleSheet) -Klasse geladen und analysiert werden, bevor es zu einem [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)hinzugefügt wird, indem es in einem `CDATA` Abschnitt inlining wird:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet>
            <![CDATA[
            ^contentpage {
                background-color: lightgray;
            }
            ]]>
        </StyleSheet>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Weitere Informationen zu Ressourcen Wörterbüchern finden Sie unter [Ressourcen Wörterbücher](~/xamarin-forms/xaml/resource-dictionaries.md).

### <a name="c"></a>C\#

In C#kann ein Stylesheet aus einer `StringReader` geladen und einem [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)hinzugefügt werden:

```csharp
public partial class MyPage : ContentPage
{
    public MyPage()
    {
        InitializeComponent();

        using (var reader = new StringReader("^contentpage { background-color: lightgray; }"))
        {
            this.Resources.Add(StyleSheet.FromReader(reader));
        }
    }
}
```

Das Argument für die `StyleSheet.FromReader`-Methode ist der `TextReader`, der das Stylesheet gelesen hat.

## <a name="selecting-elements-and-applying-properties"></a>Auswählen von Elementen und Eigenschaften anwenden

CSS wird Selektoren verwendet, um zu bestimmen, welche Elemente als Ziel. Stile mit übereinstimmenden Selektoren werden nacheinander, in der Definition Reihenfolge angewendet. Für ein bestimmtes Element definierten Stile werden immer zuletzt angewendet. Weitere Informationen zu unterstützten Selektoren finden Sie unter [Selector Reference](#selector-reference).

CSS verwendet zum Formatieren eines ausgewählten Elements. Jede Eigenschaft verfügt über einen Satz möglicher Werte ein, und einige Eigenschaften können jede Art von Element wirken sich auf, während andere auf Gruppen von Elementen anwenden. Weitere Informationen zu unterstützten Eigenschaften finden Sie unter Eigenschaften [Referenz](#property-reference).

### <a name="selecting-elements-by-type"></a>Auswählen von Elementen nach Typ

Elemente in der visuellen Struktur können nach Typ ausgewählt werden, wobei die Groß-/Kleinschreibung `element` Auswahl nicht beachtet wird:

```css
stacklayout {
    margin: 20;
}
```

Dieser Selektor identifiziert alle [`StackLayout`](xref:Xamarin.Forms.StackLayout) Elemente auf Seiten, die das Stylesheet verwenden, und legt seine Ränder auf eine einheitliche Stärke von 20 fest.

> [!NOTE]
> Der `element` Selektor identifiziert keine Unterklassen des angegebenen Typs.

### <a name="selecting-elements-by-base-class"></a>Auswählen von Elementen durch Basisklasse

Elemente in der visuellen Struktur können von der Basisklasse ausgewählt werden, ohne dass die `^base` Groß-/Kleinschreibung nicht beachtet wird.

```css
^contentpage {
    background-color: lightgray;
}
```

Dieser Selektor identifiziert alle [`ContentPage`](xref:Xamarin.Forms.ContentPage) Elemente, die das Stylesheet verwenden, und legt die Hintergrundfarbe auf `lightgray`fest.

> [!NOTE]
> Der `^base` Selektor ist für xamarin. Forms spezifisch und nicht Teil der CSS-Spezifikation.

### <a name="selecting-an-element-by-name"></a>Auswählen eines Elements anhand des Namens

Einzelne Elemente in der visuellen Struktur können mit der `#id` Auswahl unter Berücksichtigung von Groß-/Kleinschreibung ausgewählt werden:

```css
#listView {
    background-color: lightgray;
}
```

Dieser Selektor identifiziert das-Element, dessen [`StyleId`](xref:Xamarin.Forms.Element.StyleId) -Eigenschaft auf `listView`festgelegt ist. Wenn die `StyleId`-Eigenschaft jedoch nicht festgelegt ist, greift der Selektor auf die `x:Name` des-Elements zurück. Im folgenden XAML-Beispiel identifiziert die `#listView` Auswahl die [`ListView`](xref:Xamarin.Forms.ListView) , deren `x:Name`-Attribut auf `listView`festgelegt ist, und legt die Hintergrundfarbe für den `lightgray`fest.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <StackLayout>
        <ListView x:Name="listView" ...>
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

### <a name="selecting-elements-with-a-specific-class-attribute"></a>Auswählen von Elementen mit einer bestimmten Klasse-Attribut

Elemente mit einem bestimmten Klassen Attribut können mit der `.class` Auswahl unter Berücksichtigung von Groß-/Kleinschreibung ausgewählt werden:

```css
.detailPageTitle {
    font-style: bold;
    font-size: medium;
    text-align: center;
}

.detailPageSubtitle {
    text-align: center;
    font-style: italic;
}
```

Eine CSS-Klasse kann einem XAML-Element zugewiesen werden, indem die [`StyleClass`](xref:Xamarin.Forms.NavigableElement.StyleClass) -Eigenschaft des-Elements auf den CSS-Klassennamen festgelegt wird. Im folgenden XAML-Beispiel werden die Stile, die von der `.detailPageTitle`-Klasse definiert werden, dem ersten [`Label`](xref:Xamarin.Forms.Label)zugewiesen, während die von der `.detailPageSubtitle`-Klasse definierten Stile der zweiten `Label`zugewiesen werden.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <ScrollView>
        <StackLayout>
            <Label ... StyleClass="detailPageTitle" />
            <Label ... StyleClass="detailPageSubtitle"/>
            ...
        </StackLayout>
    </ScrollView>
</ContentPage>
```

### <a name="selecting-child-elements"></a>Auswählen von untergeordneten Elementen

Untergeordnete Elemente in der visuellen Struktur können ohne Berücksichtigung der Groß-/Kleinschreibung `element element` Auswahl ausgewählt werden:

```css
listview image {
    height: 60;
    width: 60;
}
```

Dieser Selektor identifiziert alle [`Image`](xref:Xamarin.Forms.Image) Elemente, die untergeordnete Elemente von [`ListView`](xref:Xamarin.Forms.ListView) Elementen sind, und legt deren Höhe und Breite auf 60 fest. Im folgenden XAML-Beispiel identifiziert der `listview image` Selektor die [`Image`](xref:Xamarin.Forms.Image) , die ein untergeordnetes Element des [`ListView`](xref:Xamarin.Forms.ListView)ist, und legt seine Höhe und Breite auf 60 fest.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <StackLayout>
        <ListView ...>
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <Grid>
                            ...
                            <Image ... />
                            ...
                        </Grid>
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

> [!NOTE]
> Die `element element` Auswahl erfordert nicht, dass das untergeordnete Element ein _direkt_ untergeordnetes Element des übergeordneten Elements ist – das untergeordnete Element kann ein anderes übergeordnetes Element aufweisen. Auswahl vorgenommen wird, vorausgesetzt, dass ein Vorgänger das erste angegebene Element ist.

### <a name="selecting-direct-child-elements"></a>Auswählen von direkt untergeordneten Elementen

Direkt untergeordnete Elemente in der visuellen Struktur können ohne Berücksichtigung der Groß-/Kleinschreibung `element>element` Auswahl ausgewählt werden:

```css
stacklayout>image {
    height: 200;
    width: 200;
}
```

Dieser Selektor identifiziert alle [`Image`](xref:Xamarin.Forms.Image) Elemente, die direkt untergeordnete Elemente von [`StackLayout`](xref:Xamarin.Forms.StackLayout) Elementen sind, und legt ihre Höhe und Breite auf 200 fest. Im folgenden XAML-Beispiel identifiziert der `stacklayout>image` Selektor die [`Image`](xref:Xamarin.Forms.Image) , die ein direkt untergeordnetes Element des [`StackLayout`](xref:Xamarin.Forms.StackLayout)ist, und legt seine Höhe und Breite auf 200 fest.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <ScrollView>
        <StackLayout>
            ...
            <Image ... />
            ...
        </StackLayout>
    </ScrollView>
</ContentPage>
```

> [!NOTE]
> Der `element>element` Selector erfordert, dass das untergeordnete Element ein _direkt_ untergeordnetes Element des übergeordneten Elements ist.

## <a name="selector-reference"></a>Selector-Referenz

Die folgenden CSS-Selektoren werden von Xamarin.Forms unterstützt:

|Auswahl|Beispiel|BESCHREIBUNG|
|---|---|---|
|`.class`|`.header`|Wählt alle-Elemente mit der `StyleClass`-Eigenschaft aus, die ' Header ' enthält. Beachten Sie, dass diese Auswahl Groß-/Kleinschreibung beachtet wird.|
|`#id`|`#email`|Wählt alle Elemente aus, für die `StyleId` auf `email`festgelegt ist. Wenn `StyleId` nicht festgelegt ist, Fall Back auf `x:Name`. Bei der Verwendung von XAML wird `x:Name` als `StyleId`bevorzugt. Beachten Sie, dass diese Auswahl Groß-/Kleinschreibung beachtet wird.|
|`*`|`*`|Wählt alle Elemente aus.|
|`element`|`label`|Wählt alle Elemente vom Typ `Label`aus, aber keine Unterklassen. Beachten Sie, dass diese Auswahl Groß-/Kleinschreibung.|
|`^base`|`^contentpage`|Wählt alle Elemente mit `ContentPage` als Basisklasse aus, einschließlich `ContentPage` selbst. Beachten Sie, dass diese Auswahl wird die Groß-/Kleinschreibung und ist nicht Teil der CSS-Spezifikation.|
|`element,element`|`label,button`|Wählt alle `Button` Elemente und alle `Label` Elemente aus. Beachten Sie, dass diese Auswahl Groß-/Kleinschreibung.|
|`element element`|`stacklayout label`|Wählt alle `Label` Elemente innerhalb eines `StackLayout`aus. Beachten Sie, dass diese Auswahl Groß-/Kleinschreibung.|
|`element>element`|`stacklayout>label`|Wählt alle `Label` Elemente mit `StackLayout` als direktes übergeordnetes Element aus. Beachten Sie, dass diese Auswahl Groß-/Kleinschreibung.|
|`element+element`|`label+entry`|Wählt alle `Entry` Elemente direkt nach einem `Label`aus. Beachten Sie, dass diese Auswahl Groß-/Kleinschreibung.|
|`element~element`|`label~entry`|Wählt alle `Entry` Elemente aus, denen ein `Label`vorangestellt ist. Beachten Sie, dass diese Auswahl Groß-/Kleinschreibung.|

Stile mit übereinstimmenden Selektoren werden nacheinander, in der Definition Reihenfolge angewendet. Für ein bestimmtes Element definierten Stile werden immer zuletzt angewendet.

> [!TIP]
> Selektoren können ohne Einschränkung kombiniert werden, wie z. b. `StackLayout>ContentView>label.email`.

Folgenden Auswahlmöglichkeiten werden zurzeit nicht unterstützt:

- `[attribute]`
- `@media` und `@supports`
- `:` und `::`

> [!NOTE]
> Spezifität und Spezifität Außerkraftsetzungen werden nicht unterstützt.

## <a name="property-reference"></a>Eigenschaftsverweis

Die folgenden CSS-Eigenschaften werden von xamarin. Forms unterstützt (in der **Values** -Spalte sind die Typen _kursiv_formatiert, während Zeichen folgen Literale `gray`werden):

|Eigenschaft|Anwendungsbereich|Werte|Beispiel|
|---|---|---|---|
|`align-content`|`FlexLayout`| `stretch` \| `center` \| `start` \| `end` \| `spacebetween` \| `spacearound` \| `spaceevenly` \| `flex-start` \| `flex-end` \| `space-between` \| `space-around` \| `initial` |`align-content: space-between;`|
|`align-items`|`FlexLayout`| `stretch` \| `center` \| `start` \| `end` \| `flex-start` \| `flex-end` \| `initial` |`align-items: flex-start;`|
|`align-self`|`VisualElement`| `auto` \| `stretch` \| `center` \| `start` \| `end` \| `flex-start` \| `flex-end` \| `initial`|`align-self: flex-end;`|
|`background-color`|`VisualElement`|_Farbe_ \| `initial` |`background-color: springgreen;`|
|`background-image`|`Page`|_Zeichen_ folgen \| `initial` |`background-image: bg.png;`|
|`border-color`|`Button`, `Frame`, `ImageButton`|_Farbe_ \| `initial`|`border-color: #9acd32;`|
|`border-radius`|`BoxView`, `Button`, `Frame`, `ImageButton`|_doppelte_ \| `initial` |`border-radius: 10;`|
|`border-width`|`Button`, `ImageButton`|_doppelte_ \| `initial` |`border-width: .5;`|
|`color`|`ActivityIndicator`, `BoxView`, `Button`, `CheckBox`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `ProgressBar`, `SearchBar`, `Switch`, `TimePicker`|_Farbe_ \| `initial` |`color: rgba(255, 0, 0, 0.3);`|
|`column-gap`|`Grid`|_doppelte_ \| `initial`|`column-gap: 9;`|
|`direction`|`VisualElement`|`ltr` \| `rtl` \| `inherit` \| `initial` |`direction: rtl;`|
|`flex-direction`|`FlexLayout`| `column` \| `columnreverse` \| `row` \| `rowreverse` \| `row-reverse` \| `column-reverse` \| `initial`|`flex-direction: column-reverse;`|
|`flex-basis`|`VisualElement`|_float_ \| `auto` \| `initial`. Außerdem kann ein Prozentsatz im Bereich von 0% bis 100% mit dem `%` Vorzeichen angegeben werden.|`flex-basis: 25%;`|
|`flex-grow`|`VisualElement`|_float_ -\| `initial`|`flex-grow: 1.5;`|
|`flex-shrink`|`VisualElement`|_float_ -\| `initial`|`flex-shrink: 1;`|
|`flex-wrap`|`VisualElement`| `nowrap` \| `wrap` \| `reverse` \| `wrap-reverse` \| `initial`|`flex-wrap: wrap-reverse;`|
|`font-family`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_Zeichen_ folgen \| `initial` |`font-family: Consolas;`|
|`font-size`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_doppelte_\| _namedsize_ -\| `initial` |`font-size: 12;`|
|`font-style`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|`bold` \| `italic` \| `initial` |`font-style: bold;`|
|`height`|`VisualElement`|_doppelte_ \| `initial` |`min-height: 250;`|
|`justify-content`|`FlexLayout`| `start` \| `center` \| `end` \| `spacebetween` \| `spacearound` \| `spaceevenly` \| `flex-start` \| `flex-end` \| `space-between` \| `space-around` \| `initial`|`justify-content: flex-end;`|
|`letter-spacing`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `SearchHandler`, `Span`, `TimePicker`|_doppelte_ \| `initial`|`letter-spacing: 2.5;`|
|`line-height`|`Label`, `Span`|_doppelte_ \| `initial` |`line-height: 1.8;`|
|`margin`|`View`|_Stärke_ \| `initial` |`margin: 6 12;`|
|`margin-left`|`View`|_Stärke_ \| `initial` |`margin-left: 3;`|
|`margin-top`|`View`|_Stärke_ \| `initial` |`margin-top: 2;`|
|`margin-right`|`View`|_Stärke_ \| `initial` |`margin-right: 1;`|
|`margin-bottom`|`View`|_Stärke_ \| `initial` |`margin-bottom: 6;`|
|`max-lines`|`Label`|_int_ \| `initial`|`max-lines: 2;`|
|`min-height`|`VisualElement`|_doppelte_ \| `initial` |`min-height: 50;`|
|`min-width`|`VisualElement`|_doppelte_ \| `initial` |`min-width: 112;`|
|`opacity`|`VisualElement`|_doppelte_ \| `initial` |`opacity: .3;`|
|`order`|`VisualElement`|_int_ \| `initial`|`order: -1;`|
|`padding`|`Button`, `ImageButton`, `Layout`, `Page`|_Stärke_ \| `initial` |`padding: 6 12 12;`|
|`padding-left`|`Button`, `ImageButton`, `Layout`, `Page`|_doppelte_ \| `initial`|`padding-left: 3;`|
|`padding-top`|`Button`, `ImageButton`, `Layout`, `Page`| _doppelte_ \| `initial` |`padding-top: 4;`|
|`padding-right`|`Button`, `ImageButton`, `Layout`, `Page`| _doppelte_ \| `initial` |`padding-right: 2;`|
|`padding-bottom`|`Button`, `ImageButton`, `Layout`, `Page`| _doppelte_ \| `initial` |`padding-bottom: 6;`|
|`position`|`FlexLayout`| `relative` \| `absolute` \| `initial`|`position: absolute;`|
|`row-gap`|`Grid`| _doppelte_ \| `initial`|`row-gap: 12;`|
|`text-align`| `Entry`, `EntryCell`, `Label`, `SearchBar`|`left` \| `top` \| `right` \| `bottom` \| `start` \| `center` \| `middle` \| `end` \| `initial`. `left` und `right` sollten in Umgebungen mit von rechts nach links vermieden werden.| `text-align: right;`|
|`text-decoration`|`Label`, `Span`|`none` \| `underline` \| `strikethrough` \| `line-through` \| `initial`|`text-decoration: underline, line-through;`|
|`transform`|`VisualElement`| `none`, `rotate`, `rotateX`, `rotateY`, `scale`, `scaleX`, `scaleY`, `translate`, `translateX`, `translateY`, `initial` |`transform: rotate(180), scaleX(2.5);`|
|`transform-origin`|`VisualElement`| _Double_, _Double_ \| `initial` |`transform-origin: 7.5, 12.5;`|
|`vertical-align`|`Label`|`left` \| `top` \| `right` \| `bottom` \| `start` \| `center` \| `middle` \| `end` \| `initial`|`vertical-align: bottom;`|
|`visibility`|`VisualElement`|`true` \| `visible` \| `false` \| `hidden` \| `collapse` \| `initial`|`visibility: hidden;`|
|`width`|`VisualElement`|_doppelte_ \| `initial`|`min-width: 320;`|

> [!NOTE]
> `initial` ist ein gültiger Wert für alle Eigenschaften. Es löscht den Wert (den Standardwert zurückgesetzt), der von einem anderen Stil festgelegt wurde.

Die folgenden Eigenschaften werden derzeit nicht unterstützt:

- [https://login.microsoftonline.com/consumers/](`all: initial`).
- Layout-Eigenschaften (Feld oder Raster).
- Kurzform Eigenschaften, z. b. `font`, und `border`.

Außerdem gibt es keinen `inherit` Wert, sodass keine Vererbung unterstützt wird. Daher können Sie z. b. die `font-size`-Eigenschaft nicht für ein Layout festlegen und erwarten, dass alle [`Label`](xref:Xamarin.Forms.Label) Instanzen im Layout den Wert erben. Die einzige Ausnahme ist die `direction`-Eigenschaft, die über den Standardwert `inherit`verfügt.

Das Ausrichten von `Span` Elementen hat ein bekanntes Problem, das verhindert, dass spannen durch Element und Name (mithilfe des `#` Symbols) als Ziel der CSS-Stile verwendet werden. Das `Span`-Element wird von `GestureElement`abgeleitet, das nicht über die `StyleClass`-Eigenschaft verfügt, sodass spannen keine CSS-Klassen Ausrichtung unterstützen. Weitere Informationen finden Sie unter nicht in der Lage, CSS-Stile [auf die Span-Steuerelemente anzuwenden](https://github.com/xamarin/Xamarin.Forms/issues/5979).

### <a name="xamarinforms-specific-properties"></a>Xamarin. Forms-spezifische Eigenschaften

Die folgenden xamarin. Forms-spezifischen CSS-Eigenschaften werden ebenfalls unterstützt (in der **Values** -Spalte sind die Typen _kursiv_formatiert, während Zeichen folgen Literale `gray`werden):

|Eigenschaft|Anwendungsbereich|Werte|Beispiel|
|---|---|---|---|
|`-xf-bar-background-color`|`NavigationPage`, `TabbedPage`|_Farbe_ \| `initial` |`-xf-bar-background-color: teal;`|
|`-xf-bar-text-color`|`NavigationPage`, `TabbedPage`|_Farbe_ \| `initial` |`-xf-bar-text-color: gray`|
|`-xf-horizontal-scroll-bar-visibility`|`ScrollView`| `default` \| `always` \| `never` \| `initial` |`-xf-horizontal-scroll-bar-visibility: never;`|
|`-xf-max-length`|`Entry`, `Editor`|_int_ \| `initial` |`-xf-max-length: 20;`|
|`-xf-max-track-color`|`Slider`|_Farbe_ \| `initial` |`-xf-max-track-color: red;`|
|`-xf-min-track-color`|`Slider`|_Farbe_ \| `initial` |`-xf-min-track-color: yellow;`|
|`-xf-orientation`|`ScrollView`, `StackLayout`| `horizontal` \| `vertical` \| `both` \| `initial`. `both` wird nur auf einem `ScrollView`unterstützt. |`-xf-orientation: horizontal;`|
|`-xf-placeholder`|`Entry`, `Editor`, `SearchBar`|_Text_ \| in Anführungszeichen `initial` |`-xf-placeholder: Enter name;`|
|`-xf-placeholder-color`|`Entry`, `Editor`, `SearchBar`|_Farbe_ \| `initial` |`-xf-placeholder-color: green;`|
|`-xf-spacing`|`StackLayout`|_doppelte_ \| `initial` |`-xf-spacing: 8;`|
|`-xf-thumb-color`|`Slider`, `Switch`|_Farbe_ \| `initial` |`-xf-thumb-color: limegreen;`|
|`-xf-vertical-scroll-bar-visibility`|`ScrollView`| `default` \| `always` \| `never` \| `initial` |`-xf-vertical-scroll-bar-visibility: always;`|
|`-xf-vertical-text-alignment`|`Label`| `start` \| `center` \| `end` \| `initial`|`-xf-vertical-text-alignment: end;`|
|`-xf-visual`|`VisualElement`|_Zeichen_ folgen \| `initial` |`-xf-visual: material;`|

### <a name="xamarinforms-shell-specific-properties"></a>In xamarin. Forms shellspezifische Eigenschaften

Die folgenden xamarin. Forms-shellspezifischen CSS-Eigenschaften werden ebenfalls unterstützt (in der **Values** -Spalte sind die Typen _kursiv_formatiert, während Zeichen folgen Literale `gray`werden):

|Eigenschaft|Anwendungsbereich|Werte|Beispiel|
|---|---|---|---|
|`-xf-flyout-background`|`Shell`|_Farbe_ \| `initial` |`-xf-flyout-background: red;`|
|`-xf-shell-background`|`Element`|_Farbe_ \| `initial` |`-xf-shell-background: green;`|
|`-xf-shell-disabled`|`Element`|_Farbe_ \| `initial` |`-xf-shell-disabled: blue;`|
|`-xf-shell-foreground`|`Element`|_Farbe_ \| `initial` |`-xf-shell-foreground: yellow;`|
|`-xf-shell-tabbar-background`|`Element`|_Farbe_ \| `initial` |`-xf-shell-tabbar-background: white;`|
|`-xf-shell-tabbar-disabled`|`Element`|_Farbe_ \| `initial` |`-xf-shell-tabbar-disabled: black;`|
|`-xf-shell-tabbar-foreground`|`Element`|_Farbe_ \| `initial` |`-xf-shell-tabbar-foreground: gray;`|
|`-xf-shell-tabbar-title`|`Element`|_Farbe_ \| `initial` |`-xf-shell-tabbar-title: lightgray;`|
|`-xf-shell-tabbar-unselected`|`Element`|_Farbe_ \| `initial` |`-xf-shell-tabbar-unselected: cyan;`|
|`-xf-shell-title`|`Element`|_Farbe_ \| `initial` |`-xf-shell-title: teal;`|
|`-xf-shell-unselected`|`Element`|_Farbe_ \| `initial` |`-xf-shell-unselected: limegreen;`|

### <a name="color"></a>Color

Die folgenden `color` Werte werden unterstützt:

- `X11` [Farben](https://en.wikipedia.org/wiki/X11_color_names), die CSS-Farben, vordefinierte UWP-Farben und xamarin. Forms-Farben entsprechen. Beachten Sie, dass diese Farbwerte Groß-/Kleinschreibung.
- hexadezimale Farben: `#rgb`, `#argb``#rrggbb`, `#aarrggbb`
- RGB-Farben: `rgb(255,0,0)``rgb(100%,0%,0%)`. Werte liegen im Bereich 0 – 100 % oder 0-255.
- RGBA-Farben: `rgba(255, 0, 0, 0.8)``rgba(100%, 0%, 0%, 0.8)`. Der Durchlässigkeitswert ist im Bereich von 0,0 bis 1,0.
- HSL-Farben: `hsl(120, 100%, 50%)`. Der h-Wert wird im Bereich zwischen 0 und 360, s und l sind in den Bereich 0 – 100 %.
- HSLA-Farben: `hsla(120, 100%, 50%, .8)`. Der Durchlässigkeitswert ist im Bereich von 0,0 bis 1,0.

### <a name="thickness"></a>Stärke

Ein, zwei, drei oder vier `thickness` Werte werden unterstützt, die jeweils durch Leerzeichen voneinander getrennt sind:

- Ein einzelner Wert gibt die einheitliche Stärke an.
- Zwei Werte geben die vertikale und horizontale Breite an.
- Drei Werte geben die Breite des unteren und oberen dann Horizontal (links und rechts).
- Vier Werte geben an, oben, und klicken Sie dann rechts, unten und dann linken Stärke.

> [!NOTE]
> CSS-`thickness` Werte unterscheiden sich von XAML- [`Thickness`](xref:Xamarin.Forms.Thickness) Werten. In XAML gibt z. b. ein zwei-Wert-`Thickness` die horizontale und vertikale Dicke an, während ein vier-Wert-`Thickness` Links, dann oben, rechts und dann untere Dicke anzeigt. Außerdem sind XAML-`Thickness` Werte durch Kommas getrennt.

### <a name="namedsize"></a>NamedSize

Die folgenden `namedsize` Werte werden unterstützt:

- `default`
- `micro`
- `small`
- `medium`
- `large`

Die genaue Bedeutung der einzelnen `namedsize` Werte ist plattformabhängig und Ansichts abhängig.

## <a name="css-in-xamarinforms-with-xamarinuniversity"></a>CSS-Code in Xamarin.Forms mit Xamarin.University

> [!VIDEO https://youtube.com/embed/va-Vb7vtan8]

**Xamarin. Forms 3,0-CSS-Video**

## <a name="related-links"></a>Verwandte Links

- [Monkeyappcss (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-monkeyappcss)
- [Ressourcenverzeichnisse](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Formatieren von Xamarin.Forms-Apps mithilfe von XAML-Formatvorlagen](~/xamarin-forms/user-interface/styles/xaml/index.md)
