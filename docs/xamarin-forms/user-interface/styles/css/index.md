---
title: Formatieren von Xamarin.Forms-Apps mithilfe von Cascading Stylesheets (CSS)
description: Xamarin. Forms unterstützt das Formatieren visueller Elemente mithilfe Cascading Stylesheets (CSS).
ms.prod: xamarin
ms.assetid: C89D57A6-DAB9-4C42-963F-26D67627DDC2
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 09/19/2019
ms.openlocfilehash: fdee070021b22f82cb69571f0fa2f396831b14e6
ms.sourcegitcommit: 6781967baeed4fe2c58f070476e7c21d01c25c30
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74052808"
---
# <a name="styling-xamarinforms-apps-using-cascading-style-sheets-css"></a>Formatieren von xamarin. Forms-apps mit Cascading Stylesheets (CSS)

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-monkeyappcss)

_Xamarin. Forms unterstützt das Formatieren visueller Elemente mithilfe Cascading Stylesheets (CSS)._

Xamarin. Forms-Anwendungen können mit CSS formatiert werden. Ein Stylesheet besteht aus einer Liste von Regeln, wobei jede Regel aus mindestens einem Selektoren und einem Deklarations Block besteht. Ein Deklarations Block besteht aus einer Liste von Deklarationen in geschweiften Klammern, wobei jede Deklaration aus einer Eigenschaft, einem Doppelpunkt und einem Wert besteht. Wenn in einem-Block mehrere Deklarationen vorhanden sind, wird ein Semikolon als Trennzeichen eingefügt. Im folgenden Codebeispiel wird ein xamarin. Forms-kompatibles CSS veranschaulicht:

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

In xamarin. Forms werden CSS-Stylesheets zur Laufzeit analysiert und ausgewertet, nicht als Kompilierzeit, und Stylesheets werden bei der Verwendung erneut analysiert.

> [!NOTE]
> Derzeit kann die gesamte Formatierung, die mit XAML-Formaten möglich ist, nicht mit CSS durchgeführt werden. Allerdings können XAML-Stile verwendet werden, um CSS für Eigenschaften zu ergänzen, die derzeit nicht von xamarin. Forms unterstützt werden. Weitere Informationen zu XAML-Formatvorlagen finden Sie unter [Formatieren von Xamarin.Forms-Apps mithilfe von XAML-Formatvorlagen](~/xamarin-forms/user-interface/styles/xaml/index.md).

Das [monkeyappcss](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-monkeyappcss) -Beispiel veranschaulicht die Verwendung von CSS zum Formatieren einer einfachen APP und ist in den folgenden Screenshots dargestellt:

[![Monkeyapp-Hauptseite mit CSS-Formatierung](css-images/MonkeyAppMainPage.png "Monkeyapp-Hauptseite mit CSS-Formatierung")](css-images/MonkeyAppMainPage-Large.png#lightbox "Monkeyapp-Hauptseite mit CSS-Formatierung")

[![Monkeyapp-Detail Seite mit CSS-Formatierung](css-images/MonkeyAppDetailPage.png "Monkeyapp-Detail Seite mit CSS-Formatierung")](css-images/MonkeyAppDetailPage-Large.png#lightbox "Monkeyapp-Detail Seite mit CSS-Formatierung")

## <a name="consuming-a-style-sheet"></a>Verwenden eines Stylesheets

Der Prozess zum Hinzufügen eines Stylesheets zu einer Projekt Mappe lautet wie folgt:

1. Fügen Sie Ihrem .NET Standard Bibliotheksprojekt eine leere CSS-Datei hinzu.
1. Legen Sie die Buildaktion der CSS-Datei auf **EmbeddedResource**fest.

### <a name="loading-a-style-sheet"></a>Laden eines Stylesheets

Es gibt eine Reihe von Ansätzen, die verwendet werden können, um ein Stylesheet zu laden.

### <a name="xaml"></a>XAML

Ein Stylesheet kann geladen und mit der [`StyleSheet`](xref:Xamarin.Forms.StyleSheets.StyleSheet) -Klasse analysiert werden, bevor es zu einem [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)hinzugefügt wird:

```xaml
<Application ...>
    <Application.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </Application.Resources>
</Application>
```

Die [`StyleSheet.Source`](xref:Xamarin.Forms.Xaml.StyleSheetExtension.Source) -Eigenschaft gibt das Stylesheet als URI relativ zum Speicherort der einschließenden XAML-Datei oder relativ zum Projektstamm an, wenn der URI mit einem `/` beginnt.

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

## <a name="selecting-elements-and-applying-properties"></a>Auswählen von Elementen und Anwenden von Eigenschaften

CSS verwendet Selektoren, um zu bestimmen, welche Elemente als Ziel verwendet werden. Stile mit übereinstimmenden Selektoren werden nacheinander in der Definitions Reihenfolge angewendet. Stile, die für ein bestimmtes Element definiert sind, werden immer zuletzt angewendet. Weitere Informationen zu unterstützten Selektoren finden Sie unter [Selector Reference](#selector-reference).

CSS verwendet Eigenschaften, um ein ausgewähltes Element zu formatieren. Jede Eigenschaft verfügt über einen Satz möglicher Werte, und einige Eigenschaften können sich auf beliebige Elementtypen auswirken, während andere auf Gruppen von Elementen zutreffen. Weitere Informationen zu unterstützten Eigenschaften finden Sie unter Eigenschaften [Referenz](#property-reference).

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

### <a name="selecting-elements-by-base-class"></a>Auswählen von Elementen nach Basisklasse

Elemente in der visuellen Struktur können von der Basisklasse ausgewählt werden, ohne dass die `^base` Groß-/Kleinschreibung nicht beachtet wird.

```css
^contentpage {
    background-color: lightgray;
}
```

Dieser Selektor identifiziert alle [`ContentPage`](xref:Xamarin.Forms.ContentPage) Elemente, die das Stylesheet verwenden, und legt die Hintergrundfarbe auf `lightgray` fest.

> [!NOTE]
> Der `^base` Selektor ist für xamarin. Forms spezifisch und nicht Teil der CSS-Spezifikation.

### <a name="selecting-an-element-by-name"></a>Auswählen eines Elements nach Name

Einzelne Elemente in der visuellen Struktur können mit der `#id` Auswahl unter Berücksichtigung von Groß-/Kleinschreibung ausgewählt werden:

```css
#listView {
    background-color: lightgray;
}
```

Dieser Selektor identifiziert das-Element, dessen [`StyleId`](xref:Xamarin.Forms.Element.StyleId) -Eigenschaft auf `listView` festgelegt ist. Wenn die `StyleId`-Eigenschaft jedoch nicht festgelegt ist, greift der Selektor auf die `x:Name` des-Elements zurück. Im folgenden XAML-Beispiel identifiziert die `#listView` Auswahl die [`ListView`](xref:Xamarin.Forms.ListView) , deren `x:Name`-Attribut auf `listView` festgelegt ist, und legt die Hintergrundfarbe für den `lightgray` fest.

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

### <a name="selecting-elements-with-a-specific-class-attribute"></a>Auswählen von Elementen mit einem bestimmten Klassen Attribut

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

Eine CSS-Klasse kann einem XAML-Element zugewiesen werden, indem die [`StyleClass`](xref:Xamarin.Forms.NavigableElement.StyleClass) -Eigenschaft des-Elements auf den CSS-Klassennamen festgelegt wird. Im folgenden XAML-Beispiel werden die Stile, die von der `.detailPageTitle`-Klasse definiert werden, dem ersten [`Label`](xref:Xamarin.Forms.Label)zugewiesen, während die von der `.detailPageSubtitle`-Klasse definierten Stile der zweiten `Label` zugewiesen werden.

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

### <a name="selecting-child-elements"></a>Auswählen untergeordneter Elemente

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
> Die `element element` Auswahl erfordert nicht, dass das untergeordnete Element ein _direkt_ untergeordnetes Element des übergeordneten Elements ist – das untergeordnete Element kann ein anderes übergeordnetes Element aufweisen. Die Auswahl erfolgt, wenn ein Vorgänger das angegebene erste Element ist.

### <a name="selecting-direct-child-elements"></a>Auswählen direkter untergeordneter Elemente

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

## <a name="selector-reference"></a>Auswahl Verweis

Die folgenden CSS-Selektoren werden von xamarin. Forms unterstützt:

|Auswahl|Beispiel|Beschreibung|
|---|---|---|
|`.class`|`.header`|Wählt alle-Elemente mit der `StyleClass`-Eigenschaft aus, die ' Header ' enthält. Beachten Sie, dass bei dieser Auswahl zwischen Groß-und klein|
|`#id`|`#email`|Wählt alle Elemente aus, für die `StyleId` auf `email` festgelegt ist. Wenn `StyleId` nicht festgelegt ist, Fall Back auf `x:Name`. Bei der Verwendung von XAML wird `x:Name` als `StyleId` bevorzugt. Beachten Sie, dass bei dieser Auswahl zwischen Groß-und klein|
|`*`|`*`|Wählt alle-Elemente aus.|
|`element`|`label`|Wählt alle Elemente vom Typ `Label` aus, aber keine Unterklassen. Beachten Sie, dass die Groß-/Kleinschreibung bei diesem|
|`^base`|`^contentpage`|Wählt alle Elemente mit `ContentPage` als Basisklasse aus, einschließlich `ContentPage` selbst. Beachten Sie, dass dieser Selektor die Groß-/Kleinschreibung nicht beachtet und nicht Teil der CSS-Spezifikation|
|`element,element`|`label,button`|Wählt alle `Button` Elemente und alle `Label` Elemente aus. Beachten Sie, dass die Groß-/Kleinschreibung bei diesem|
|`element element`|`stacklayout label`|Wählt alle `Label` Elemente innerhalb eines `StackLayout` aus. Beachten Sie, dass die Groß-/Kleinschreibung bei diesem|
|`element>element`|`stacklayout>label`|Wählt alle `Label` Elemente mit `StackLayout` als direktes übergeordnetes Element aus. Beachten Sie, dass die Groß-/Kleinschreibung bei diesem|
|`element+element`|`label+entry`|Wählt alle `Entry` Elemente direkt nach einem `Label` aus. Beachten Sie, dass die Groß-/Kleinschreibung bei diesem|
|`element~element`|`label~entry`|Wählt alle `Entry` Elemente aus, denen ein `Label` vorangestellt ist. Beachten Sie, dass die Groß-/Kleinschreibung bei diesem|

Stile mit übereinstimmenden Selektoren werden nacheinander in der Definitions Reihenfolge angewendet. Stile, die für ein bestimmtes Element definiert sind, werden immer zuletzt angewendet.

> [!TIP]
> Selektoren können ohne Einschränkung kombiniert werden, wie z. b. `StackLayout>ContentView>label.email`.

Die folgenden Selektoren werden zurzeit nicht unterstützt:

- `[attribute]`
- `@media` und `@supports`
- `:` und `::`

> [!NOTE]
> Spezifizität und spezifitäts Überschreitungen werden nicht unterstützt.

## <a name="property-reference"></a>Eigenschafts Verweis

Die folgenden CSS-Eigenschaften werden von xamarin. Forms unterstützt (in der **Values** -Spalte sind die Typen _kursiv_formatiert, während Zeichen folgen Literale `gray` werden):

|property|Betrifft|Werte|Beispiel|
|---|---|---|---|
|`align-content`|`FlexLayout`| `stretch` \| `center` \| `start` \| `end` \| `spacebetween` \| `spacearound` \| `spaceevenly` \| `flex-start` \| `flex-end` \| `space-between` \| `space-around` \| `initial` |`align-content: space-between;`|
|`align-items`|`FlexLayout`| `stretch` \| `center` \| `start` \| `end` \| `flex-start` \| 0 1 2 |`align-items: flex-start;`|
|`align-self`|`VisualElement`| `auto` \| `stretch` \| `center` \| `start` \| `end` \| 0 1 2 3 4|`align-self: flex-end;`|
|`background-color`|`VisualElement`|_Farbe_ \| `initial` |`background-color: springgreen;`|
|`background-image`|`Page`|_Zeichen_ folgen \| `initial` |`background-image: bg.png;`|
|`border-color`|`Button`ist `Frame`ist `ImageButton`|_Farbe_ \| `initial`|`border-color: #9acd32;`|
|`border-radius`|`BoxView`, `Button`, `Frame`, `ImageButton`|_doppelte_ \| `initial` |`border-radius: 10;`|
|`border-width`|`Button`, `ImageButton`|_doppelte_ \| `initial` |`border-width: .5;`|
|`color`|`ActivityIndicator`, `BoxView`, `Button`, `CheckBox`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `ProgressBar`, `SearchBar`, `Switch`, `TimePicker`|_Farbe_ \| `initial` |`color: rgba(255, 0, 0, 0.3);`|
|`column-gap`|`Grid`|_doppelte_ \| `initial`|`column-gap: 9;`|
|`direction`|`VisualElement`|`ltr` \| `rtl` \| `inherit` \| `initial` |`direction: rtl;`|
|`flex-direction`|`FlexLayout`| `column` \| `columnreverse` \| `row` \| `rowreverse` \| `row-reverse` \| 0 1 2|`flex-direction: column-reverse;`|
|`flex-basis`|`VisualElement`|_float_ \| `auto` \| `initial`. Außerdem kann ein Prozentsatz im Bereich von 0% bis 100% mit dem `%` Vorzeichen angegeben werden.|`flex-basis: 25%;`|
|`flex-grow`|`VisualElement`|_float_ -\| `initial`|`flex-grow: 1.5;`|
|`flex-shrink`|`VisualElement`|_float_ -\| `initial`|`flex-shrink: 1;`|
|`flex-wrap`|`VisualElement`| `nowrap` \| `wrap` \| `reverse` \| `wrap-reverse` \| `initial`|`flex-wrap: wrap-reverse;`|
|`font-family`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_Zeichen_ folgen \| `initial` |`font-family: Consolas;`|
|`font-size`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_doppelte_ \| _namedsize_ -\| `initial` |`font-size: 12;`|
|`font-style`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|`bold` \| `italic` \| `initial` |`font-style: bold;`|
|`height`|`VisualElement`|_doppelte_ \| `initial` |`min-height: 250;`|
|`justify-content`|`FlexLayout`| `start` \| `center` \| `end` \| `spacebetween` \| `spacearound` \| `spaceevenly` \| `flex-start` \| `flex-end` \| `space-between` \| `space-around` \| `initial`|`justify-content: flex-end;`|
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
|`text-align`| `Entry`, `EntryCell`, `Label`, `SearchBar`|`left` \| `top` \| `right` \| `bottom` \| `start` \| 0 1 2 3 4 5 6. `left` und `right` sollten in Umgebungen mit von rechts nach links vermieden werden.| `text-align: right;`|
|`text-decoration`|`Label`, `Span`|`none` \| `underline` \| `strikethrough` \| `line-through` \| `initial`|`text-decoration: underline, line-through;`|
|`transform`|`VisualElement`| `none`, `rotate`, `rotateX`, `rotateY`, `scale`, `scaleX`, `scaleY`, `translate`, `translateX`, `translateY`, `initial` |`transform: rotate(180), scaleX(2.5);`|
|`transform-origin`|`VisualElement`| _Double_, _Double_ \| `initial` |`transform-origin: 7.5, 12.5;`|
|`vertical-align`|`Label`|`left` \| `top` \| `right` \| `bottom` \| `start` \| 0 1 2 3 4 5 6|`vertical-align: bottom;`|
|`visibility`|`VisualElement`|`true` \| `visible` \| `false` \| `hidden` \| `collapse` \| 0|`visibility: hidden;`|
|`width`|`VisualElement`|_doppelte_ \| `initial`|`min-width: 320;`|

> [!NOTE]
> `initial` ist ein gültiger Wert für alle Eigenschaften. Der Wert (wird auf den Standardwert zurückgesetzt), der aus einem anderen Format festgelegt wurde, wird gelöscht.

Die folgenden Eigenschaften werden zurzeit nicht unterstützt:

- `all: initial`
- Layouteigenschaften (Feld oder Raster).
- Kurzform Eigenschaften, z. b. `font`, und `border`.

Außerdem gibt es keinen `inherit` Wert, sodass keine Vererbung unterstützt wird. Daher können Sie z. b. die `font-size`-Eigenschaft nicht für ein Layout festlegen und erwarten, dass alle [`Label`](xref:Xamarin.Forms.Label) Instanzen im Layout den Wert erben. Die einzige Ausnahme ist die `direction`-Eigenschaft, die über den Standardwert `inherit` verfügt.

Das Ausrichten von `Span` Elementen hat ein bekanntes Problem, das verhindert, dass spannen durch Element und Name (mithilfe des `#` Symbols) als Ziel der CSS-Stile verwendet werden. Das `Span`-Element wird von `GestureElement`abgeleitet, das nicht über die `StyleClass`-Eigenschaft verfügt, sodass spannen keine CSS-Klassen Ausrichtung unterstützen. Weitere Informationen finden Sie unter nicht in der Lage, CSS-Stile [auf die Span-Steuerelemente anzuwenden](https://github.com/xamarin/Xamarin.Forms/issues/5979).

### <a name="xamarinforms-specific-properties"></a>Xamarin. Forms-spezifische Eigenschaften

Die folgenden xamarin. Forms-spezifischen CSS-Eigenschaften werden ebenfalls unterstützt (in der **Values** -Spalte sind die Typen _kursiv_formatiert, während Zeichen folgen Literale `gray` werden):

|property|Betrifft|Werte|Beispiel|
|---|---|---|---|
|`-xf-bar-background-color`|`NavigationPage`, `TabbedPage`|_Farbe_ \| `initial` |`-xf-bar-background-color: teal;`|
|`-xf-bar-text-color`|`NavigationPage`, `TabbedPage`|_Farbe_ \| `initial` |`-xf-bar-text-color: gray`|
|`-xf-horizontal-scroll-bar-visibility`|`ScrollView`| `default` \| `always` \| `never` \| `initial` |`-xf-horizontal-scroll-bar-visibility: never;`|
|`-xf-max-length`|`Entry`, `Editor`|_int_ \| `initial` |`-xf-max-length: 20;`|
|`-xf-max-track-color`|`Slider`|_Farbe_ \| `initial` |`-xf-max-track-color: red;`|
|`-xf-min-track-color`|`Slider`|_Farbe_ \| `initial` |`-xf-min-track-color: yellow;`|
|`-xf-orientation`|`ScrollView`, `StackLayout`| `horizontal` \| `vertical` \| `both` \| `initial`. `both` wird nur auf einem `ScrollView` unterstützt. |`-xf-orientation: horizontal;`|
|`-xf-placeholder`|`Entry`ist `Editor`ist `SearchBar`|_Text_ \| in Anführungszeichen `initial` |`-xf-placeholder: Enter name;`|
|`-xf-placeholder-color`|`Entry`ist `Editor`ist `SearchBar`|_Farbe_ \| `initial` |`-xf-placeholder-color: green;`|
|`-xf-spacing`|`StackLayout`|_doppelte_ \| `initial` |`-xf-spacing: 8;`|
|`-xf-thumb-color`|`Slider`, `Switch`|_Farbe_ \| `initial` |`-xf-thumb-color: limegreen;`|
|`-xf-vertical-scroll-bar-visibility`|`ScrollView`| `default` \| `always` \| `never` \| `initial` |`-xf-vertical-scroll-bar-visibility: always;`|
|`-xf-vertical-text-alignment`|`Label`| `start` \| `center` \| `end` \| `initial`|`-xf-vertical-text-alignment: end;`|
|`-xf-visual`|`VisualElement`|_Zeichen_ folgen \| `initial` |`-xf-visual: material;`|

### <a name="xamarinforms-shell-specific-properties"></a>In xamarin. Forms shellspezifische Eigenschaften

Die folgenden xamarin. Forms-shellspezifischen CSS-Eigenschaften werden ebenfalls unterstützt (in der **Values** -Spalte sind die Typen _kursiv_formatiert, während Zeichen folgen Literale `gray` werden):

|property|Betrifft|Werte|Beispiel|
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

### <a name="color"></a>Farbe

Die folgenden `color` Werte werden unterstützt:

- `X11` [Farben](https://en.wikipedia.org/wiki/X11_color_names/), die CSS-Farben, vordefinierte UWP-Farben und xamarin. Forms-Farben entsprechen. Beachten Sie, dass bei diesen Farbwerten die Groß-/Kleinschreibung
- hexadezimale Farben: `#rgb`, `#argb` `#rrggbb`, `#aarrggbb`
- RGB-Farben: `rgb(255,0,0)``rgb(100%,0%,0%)`. Die Werte liegen im Bereich von 0-255 oder 0%-100%.
- RGBA-Farben: `rgba(255, 0, 0, 0.8)``rgba(100%, 0%, 0%, 0.8)`. Der Wert für die Deckkraft liegt im Bereich 0,0-1.0.
- HSL-Farben: `hsl(120, 100%, 50%)`. Der Wert für "h" liegt im Bereich von 0-360, während "s" und "l" im Bereich von 0%-100% liegen.
- HSLA-Farben: `hsla(120, 100%, 50%, .8)`. Der Wert für die Deckkraft liegt im Bereich 0,0-1.0.

### <a name="thickness"></a>Stärke

Ein, zwei, drei oder vier `thickness` Werte werden unterstützt, die jeweils durch Leerzeichen voneinander getrennt sind:

- Ein einzelner Wert gibt eine einheitliche Stärke an.
- Zwei Werte geben vertikale und horizontale Dicke an.
- Drei Werte geben Top, dann horizontal (Links und rechts) und dann untere Dicke an.
- Vier Werte geben Top, dann right, then Bottom und dann Left Dicke an.

> [!NOTE]
> CSS-`thickness` Werte unterscheiden sich von XAML- [`Thickness`](xref:Xamarin.Forms.Thickness) Werten. In XAML gibt z. b. ein zwei-Wert-`Thickness` die horizontale und vertikale Dicke an, während ein vier-Wert-`Thickness` Links, dann oben, rechts und dann untere Dicke anzeigt. Außerdem sind XAML-`Thickness` Werte durch Kommas getrennt.

### <a name="namedsize"></a>Namedsize

Die folgenden `namedsize` Werte werden unterstützt:

- `default`
- `micro`
- `small`
- `medium`
- `large`

Die genaue Bedeutung der einzelnen `namedsize` Werte ist plattformabhängig und Ansichts abhängig.

## <a name="css-in-xamarinforms-with-xamarinuniversity"></a>CSS in xamarin. Forms mit xamarin. University

> [!VIDEO https://youtube.com/embed/va-Vb7vtan8]

**Xamarin. Forms 3,0-CSS-Video**

## <a name="related-links"></a>Verwandte Links

- [Monkeyappcss (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-monkeyappcss)
- [Ressourcenverzeichnisse](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Formatieren von Xamarin.Forms-Apps mithilfe von XAML-Formatvorlagen](~/xamarin-forms/user-interface/styles/xaml/index.md)
