---
title: Formatieren von Xamarin.Forms apps mit Cascading Stylesheets (CSS)
description: Xamarin.Formsunterstützt das Formatieren visueller Elemente mithilfe Cascading Stylesheets (CSS).
ms.prod: xamarin
ms.assetid: C89D57A6-DAB9-4C42-963F-26D67627DDC2
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/20/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9375c4078c75d8e4788cb31a3d6a6a3a10100f49
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84946222"
---
# <a name="styling-xamarinforms-apps-using-cascading-style-sheets-css"></a>Formatieren von Xamarin.Forms apps mit Cascading Stylesheets (CSS)

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-monkeyappcss)

_Xamarin. Forms unterstützt das Formatieren visueller Elemente mithilfe Cascading Stylesheets (CSS)._

Xamarin.FormsAnwendungen können mit CSS formatiert werden. Ein Stylesheet besteht aus einer Liste von Regeln, wobei jede Regel aus mindestens einem Selektoren und einem Deklarations Block besteht. Ein Deklarations Block besteht aus einer Liste von Deklarationen in geschweiften Klammern, wobei jede Deklaration aus einer Eigenschaft, einem Doppelpunkt und einem Wert besteht. Wenn in einem-Block mehrere Deklarationen vorhanden sind, wird ein Semikolon als Trennzeichen eingefügt. Das folgende Codebeispiel zeigt ein Xamarin.Forms kompatibles CSS:

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

In Xamarin.Forms werden CSS-Stylesheets analysiert und zur Laufzeit ausgewertet, nicht als Kompilierzeit, und Stylesheets werden bei Verwendung erneut analysiert.

> [!NOTE]
> Derzeit kann die gesamte Formatierung, die mit XAML-Formaten möglich ist, nicht mit CSS durchgeführt werden. Allerdings können XAML-Stile verwendet werden, um CSS für Eigenschaften zu ergänzen, die derzeit nicht von unterstützt werden Xamarin.Forms . Weitere Informationen zu XAML-Formatvorlagen finden Sie unter [Formatieren von Xamarin.Forms-Apps mithilfe von XAML-Formatvorlagen](~/xamarin-forms/user-interface/styles/xaml/index.md).

Das [monkeyappcss](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-monkeyappcss) -Beispiel veranschaulicht die Verwendung von CSS zum Formatieren einer einfachen APP und ist in den folgenden Screenshots dargestellt:

[![Monkeyapp-Hauptseite mit CSS-Formatierung](css-images/MonkeyAppMainPage.png "Monkeyapp-Hauptseite mit CSS-Formatierung")](css-images/MonkeyAppMainPage-Large.png#lightbox "Monkeyapp-Hauptseite mit CSS-Formatierung")

[![Monkeyapp-Detail Seite mit CSS-Formatierung](css-images/MonkeyAppDetailPage.png "Monkeyapp-Detail Seite mit CSS-Formatierung")](css-images/MonkeyAppDetailPage-Large.png#lightbox "Monkeyapp-Detail Seite mit CSS-Formatierung")

## <a name="consuming-a-style-sheet"></a>Verwenden eines Stylesheets

Der Prozess zum Hinzufügen eines Stylesheets zu einer Projekt Mappe lautet wie folgt:

1. Fügen Sie Ihrem .NET Standard Bibliotheksprojekt eine leere CSS-Datei hinzu.
1. Legen Sie die Buildaktion der CSS-Datei auf **EmbeddedResource**fest.

### <a name="loading-a-style-sheet"></a>Laden eines Stylesheets

Es gibt eine Reihe von Ansätzen, die verwendet werden können, um ein Stylesheet zu laden.

> [!NOTE]
> Es ist derzeit nicht möglich, ein Stylesheet zur Laufzeit zu ändern, und das neue Stylesheet muss angewendet werden.

### <a name="xaml"></a>XAML

Ein Stylesheet kann geladen und mit der-Klasse analysiert werden [`StyleSheet`](xref:Xamarin.Forms.StyleSheets.StyleSheet) , bevor es zu einem hinzugefügt wird [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) :

```xaml
<Application ...>
    <Application.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </Application.Resources>
</Application>
```

Die- [`StyleSheet.Source`](xref:Xamarin.Forms.Xaml.StyleSheetExtension.Source) Eigenschaft gibt das Stylesheet als URI relativ zum Speicherort der einschließenden XAML-Datei oder relativ zum Projektstamm an, wenn der URI mit einem beginnt `/` .

> [!WARNING]
> Die CSS-Datei kann nicht geladen werden, wenn die Buildaktion nicht auf **EmbeddedResource**festgelegt ist.

Alternativ kann ein Stylesheet mit der-Klasse geladen und analysiert werden [`StyleSheet`](xref:Xamarin.Forms.StyleSheets.StyleSheet) , bevor es zu einem hinzugefügt wird [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) , indem es in einen- `CDATA` Abschnitt eingefügt wird:

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

In c# kann ein Stylesheet von einem geladen `StringReader` und zu einem hinzugefügt werden [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) :

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

Das Argument für die- `StyleSheet.FromReader` Methode ist der `TextReader` , der das Stylesheet gelesen hat.

## <a name="selecting-elements-and-applying-properties"></a>Auswählen von Elementen und Anwenden von Eigenschaften

CSS verwendet Selektoren, um zu bestimmen, welche Elemente als Ziel verwendet werden. Stile mit übereinstimmenden Selektoren werden nacheinander in der Definitions Reihenfolge angewendet. Stile, die für ein bestimmtes Element definiert sind, werden immer zuletzt angewendet. Weitere Informationen zu unterstützten Selektoren finden Sie unter [Selector Reference](#selector-reference).

CSS verwendet Eigenschaften, um ein ausgewähltes Element zu formatieren. Jede Eigenschaft verfügt über einen Satz möglicher Werte, und einige Eigenschaften können sich auf beliebige Elementtypen auswirken, während andere auf Gruppen von Elementen zutreffen. Weitere Informationen zu unterstützten Eigenschaften finden Sie unter Eigenschaften [Referenz](#property-reference).

Untergeordnete Stylesheets überschreiben immer übergeordnete Stylesheets, wenn die gleichen Eigenschaften festgelegt werden. Daher werden die folgenden Rang folgen Regeln befolgt, wenn Stile angewendet werden, die die gleichen Eigenschaften festlegen:

- Ein Stil, der in den Anwendungs Ressourcen definiert ist, wird durch ein Format überschrieben, das in den Seiten Ressourcen definiert ist, wenn die gleichen Eigenschaften festgelegt werden.
- Ein in Seiten Ressourcen definierter Stil wird durch einen Stil überschrieben, der in den Steuerelement Ressourcen definiert ist, wenn die gleichen Eigenschaften festgelegt werden.
- Ein Stil, der in den Anwendungs Ressourcen definiert ist, wird durch ein Format überschrieben, das in den Steuerelement Ressourcen definiert ist, wenn die gleichen Eigenschaften festgelegt werden.

> [!IMPORTANT]
> CSS-Variablen werden nicht unterstützt.

### <a name="selecting-elements-by-type"></a>Auswählen von Elementen nach Typ

Elemente in der visuellen Struktur können nach Typ ausgewählt werden, bei dem die Groß-/Kleinschreibung nicht beachtet wird `element` :

```css
stacklayout {
    margin: 20;
}
```

Dieser Selektor identifiziert alle [`StackLayout`](xref:Xamarin.Forms.StackLayout) Elemente auf Seiten, die das Stylesheet verwenden, und legt seine Ränder auf eine einheitliche Stärke von 20 fest.

> [!NOTE]
> Der `element` Selektor identifiziert keine Unterklassen des angegebenen Typs.

### <a name="selecting-elements-by-base-class"></a>Auswählen von Elementen nach Basisklasse

Elemente in der visuellen Struktur können von der Basisklasse mit der Auswahl der Groß-/Kleinschreibung ausgewählt werden `^base` :

```css
^contentpage {
    background-color: lightgray;
}
```

Dieser Selektor identifiziert alle Elemente, die [`ContentPage`](xref:Xamarin.Forms.ContentPage) das Stylesheet verwenden, und legt die Hintergrundfarbe auf fest `lightgray` .

> [!NOTE]
> Der `^base` Selektor ist spezifisch für Xamarin.Forms , und ist nicht Teil der CSS-Spezifikation.

### <a name="selecting-an-element-by-name"></a>Auswählen eines Elements nach Name

Einzelne Elemente in der visuellen Struktur können mit der Unterscheidung nach Groß-/Kleinschreibung ausgewählt werden `#id` :

```css
#listView {
    background-color: lightgray;
}
```

Dieser Selektor identifiziert das-Element, dessen- [`StyleId`](xref:Xamarin.Forms.Element.StyleId) Eigenschaft auf festgelegt ist `listView` . Wenn die- `StyleId` Eigenschaft jedoch nicht festgelegt ist, greift der Selektor auf die `x:Name` des-Elements zurück. Im folgenden XAML `#listView` -Beispiel identifiziert der Selektor den, [`ListView`](xref:Xamarin.Forms.ListView) dessen `x:Name` -Attribut auf festgelegt ist `listView` , und legt seine Hintergrundfarbe auf fest `lightgray` .

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

Elemente mit einem bestimmten Klassen Attribut können mit der Unterscheidung nach Groß-/Kleinschreibung ausgewählt werden `.class` :

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

Eine CSS-Klasse kann einem XAML-Element zugewiesen werden, indem die- [`StyleClass`](xref:Xamarin.Forms.NavigableElement.StyleClass) Eigenschaft des-Elements auf den CSS-Klassennamen festgelegt wird. Im folgenden XAML-Beispiel werden die von der-Klasse definierten Stile `.detailPageTitle` dem ersten zugewiesen [`Label`](xref:Xamarin.Forms.Label) , während die von der-Klasse definierten Stile `.detailPageSubtitle` der zweiten zugewiesen werden `Label` .

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

Untergeordnete Elemente in der visuellen Struktur können mit der Auswahl der Groß-/Kleinschreibung ausgewählt werden `element element` :

```css
listview image {
    height: 60;
    width: 60;
}
```

Dieser Selektor identifiziert alle [`Image`](xref:Xamarin.Forms.Image) Elemente, die untergeordnete [`ListView`](xref:Xamarin.Forms.ListView) Elemente von-Elementen sind, und legt seine Höhe und Breite auf 60 fest. Im folgenden XAML-Beispiel identifiziert der Selektor den, der ein untergeordnetes Element `listview image` [`Image`](xref:Xamarin.Forms.Image) von ist [`ListView`](xref:Xamarin.Forms.ListView) , und legt seine Höhe und Breite auf 60 fest.

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
> Der `element element` Selektor erfordert nicht, dass das untergeordnete Element ein _direkt_ untergeordnetes Element des übergeordneten Elements ist – das untergeordnete Element kann über ein anderes übergeordnetes Element verfügen. Die Auswahl erfolgt, wenn ein Vorgänger das angegebene erste Element ist.

### <a name="selecting-direct-child-elements"></a>Auswählen direkter untergeordneter Elemente

Direkt untergeordnete Elemente in der visuellen Struktur können mit der Auswahl der Groß-/Kleinschreibung ausgewählt werden `element>element` :

```css
stacklayout>image {
    height: 200;
    width: 200;
}
```

Dieser Selektor identifiziert alle [`Image`](xref:Xamarin.Forms.Image) Elemente, die direkt untergeordnete [`StackLayout`](xref:Xamarin.Forms.StackLayout) Elemente von Elementen sind, und legt deren Höhe und Breite auf 200 fest. Im folgenden XAML `stacklayout>image` -Beispiel identifiziert der Selektor den, der ein direkt untergeordnetes Element [`Image`](xref:Xamarin.Forms.Image) von ist [`StackLayout`](xref:Xamarin.Forms.StackLayout) , und legt seine Höhe und Breite auf 200 fest.

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
> Der `element>element` Selektor erfordert, dass das untergeordnete Element ein _direkt_ untergeordnetes Element des übergeordneten Elements ist.

## <a name="selector-reference"></a>Auswahl Verweis

Folgende CSS-Selektoren werden von unterstützt Xamarin.Forms :

|Auswahl|Beispiel|Beschreibung|
|---|---|---|
|`.class`|`.header`|Wählt alle-Elemente mit der-Eigenschaft aus, die `StyleClass` ' Header ' enthält. Beachten Sie, dass bei dieser Auswahl zwischen Groß-und klein|
|`#id`|`#email`|Wählt alle-Elemente aus `StyleId` , deren auf festgelegt ist `email` . Wenn `StyleId` nicht festgelegt ist, Fall Back auf `x:Name` . Bei Verwendung von XAML `x:Name` wird von bevorzugt `StyleId` . Beachten Sie, dass bei dieser Auswahl zwischen Groß-und klein|
|`*`|`*`|Wählt alle-Elemente aus.|
|`element`|`label`|Wählt alle Elemente vom Typ aus `Label` , aber keine Unterklassen. Beachten Sie, dass die Groß-/Kleinschreibung bei diesem|
|`^base`|`^contentpage`|Wählt alle Elemente mit `ContentPage` als Basisklasse aus, einschließlich `ContentPage` selbst. Beachten Sie, dass dieser Selektor die Groß-/Kleinschreibung nicht beachtet und nicht Teil der CSS-Spezifikation|
|`element,element`|`label,button`|Wählt alle `Button` -Elemente und alle- `Label` Elemente aus. Beachten Sie, dass die Groß-/Kleinschreibung bei diesem|
|`element element`|`stacklayout label`|Wählt alle `Label` Elemente in einer aus `StackLayout` . Beachten Sie, dass die Groß-/Kleinschreibung bei diesem|
|`element>element`|`stacklayout>label`|Wählt alle `Label` Elemente mit `StackLayout` als direktes übergeordnetes Element aus. Beachten Sie, dass die Groß-/Kleinschreibung bei diesem|
|`element+element`|`label+entry`|Wählt alle `Entry` Elemente direkt nach einem aus `Label` . Beachten Sie, dass die Groß-/Kleinschreibung bei diesem|
|`element~element`|`label~entry`|Wählt alle `Entry` Elemente aus, denen ein vorangestellt ist `Label` . Beachten Sie, dass die Groß-/Kleinschreibung bei diesem|

Stile mit übereinstimmenden Selektoren werden nacheinander in der Definitions Reihenfolge angewendet. Stile, die für ein bestimmtes Element definiert sind, werden immer zuletzt angewendet.

> [!TIP]
> Selektoren können ohne Einschränkung kombiniert werden, z `StackLayout>ContentView>label.email` . b..

Die folgenden Selektoren werden zurzeit nicht unterstützt:

- `[attribute]`
- `@media` und `@supports`
- `:` und `::`

> [!NOTE]
> Spezifizität und spezifitäts Überschreitungen werden nicht unterstützt.

## <a name="property-reference"></a>Eigenschaftsverweis

Die folgenden CSS-Eigenschaften werden von unterstützt Xamarin.Forms : (in der **Values** -Spalte sind die Typen _kursiv_formatiert, während Zeichen folgen Literale lauten `gray` ):

|Eigenschaft|Anwendungsbereich|Werte|Beispiel|
|---|---|---|---|
|`align-content`|`FlexLayout`| `stretch` \| `center` \| `start` \| `end` \| `spacebetween` \| `spacearound` \| `spaceevenly` \| `flex-start` \| `flex-end` \| `space-between` \| `space-around` \| `initial` |`align-content: space-between;`|
|`align-items`|`FlexLayout`| `stretch` \| `center` \| `start` \| `end` \| `flex-start` \| `flex-end` \| `initial` |`align-items: flex-start;`|
|`align-self`|`VisualElement`| `auto` \| `stretch` \| `center` \| `start` \| `end` \| `flex-start` \| `flex-end` \| `initial`|`align-self: flex-end;`|
|`background-color`|`VisualElement`|_Farbe_ \|`initial` |`background-color: springgreen;`|
|`background-image`|`Page`|_Zeichenfolge_ \| `initial` |`background-image: bg.png;`|
|`border-color`|`Button`, `Frame`, `ImageButton`|_Farbe_ \|`initial`|`border-color: #9acd32;`|
|`border-radius`|`BoxView`, `Button`, `Frame`, `ImageButton`|_double_ \| `initial` |`border-radius: 10;`|
|`border-width`|`Button`, `ImageButton`|_double_ \| `initial` |`border-width: .5;`|
|`color`|`ActivityIndicator`, `BoxView`, `Button`, `CheckBox`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `ProgressBar`, `SearchBar`, `Switch`, `TimePicker`|_Farbe_ \|`initial` |`color: rgba(255, 0, 0, 0.3);`|
|`column-gap`|`Grid`|_double_ \| `initial`|`column-gap: 9;`|
|`direction`|`VisualElement`|`ltr` \| `rtl` \| `inherit` \| `initial` |`direction: rtl;`|
|`flex-direction`|`FlexLayout`| `column` \| `columnreverse` \| `row` \| `rowreverse` \| `row-reverse` \| `column-reverse` \| `initial`|`flex-direction: column-reverse;`|
|`flex-basis`|`VisualElement`|_float_ \| `auto` \| `initial`. Außerdem kann ein Prozentsatz im Bereich von 0% bis 100% mit dem Vorzeichen angegeben werden `%` .|`flex-basis: 25%;`|
|`flex-grow`|`VisualElement`|_float_ \|`initial`|`flex-grow: 1.5;`|
|`flex-shrink`|`VisualElement`|_float_ \|`initial`|`flex-shrink: 1;`|
|`flex-wrap`|`VisualElement`| `nowrap` \| `wrap` \| `reverse` \| `wrap-reverse` \| `initial`|`flex-wrap: wrap-reverse;`|
|`font-family`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_Zeichenfolge_ \| `initial` |`font-family: Consolas;`|
|`font-size`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_Double_ \| _namedsize_ \|  `initial` |`font-size: 12;`|
|`font-style`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|`bold` \| `italic` \| `initial` |`font-style: bold;`|
|`height`|`VisualElement`|_double_ \| `initial` |`min-height: 250;`|
|`justify-content`|`FlexLayout`| `start` \| `center` \| `end` \| `spacebetween` \| `spacearound` \| `spaceevenly` \| `flex-start` \| `flex-end` \| `space-between` \| `space-around` \| `initial`|`justify-content: flex-end;`|
|`letter-spacing`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `SearchHandler`, `Span`, `TimePicker`|_double_ \| `initial`|`letter-spacing: 2.5;`|
|`line-height`|`Label`, `Span`|_double_ \| `initial` |`line-height: 1.8;`|
|`margin`|`View`|_Stärke_ \|`initial` |`margin: 6 12;`|
|`margin-left`|`View`|_Stärke_ \|`initial` |`margin-left: 3;`|
|`margin-top`|`View`|_Stärke_ \|`initial` |`margin-top: 2;`|
|`margin-right`|`View`|_Stärke_ \|`initial` |`margin-right: 1;`|
|`margin-bottom`|`View`|_Stärke_ \|`initial` |`margin-bottom: 6;`|
|`max-lines`|`Label`|_INT_ \| `initial`|`max-lines: 2;`|
|`min-height`|`VisualElement`|_double_ \| `initial` |`min-height: 50;`|
|`min-width`|`VisualElement`|_double_ \| `initial` |`min-width: 112;`|
|`opacity`|`VisualElement`|_double_ \| `initial` |`opacity: .3;`|
|`order`|`VisualElement`|_INT_ \| `initial`|`order: -1;`|
|`padding`|`Button`, `ImageButton`, `Layout`, `Page`|_Stärke_ \|`initial` |`padding: 6 12 12;`|
|`padding-left`|`Button`, `ImageButton`, `Layout`, `Page`|_double_ \| `initial`|`padding-left: 3;`|
|`padding-top`|`Button`, `ImageButton`, `Layout`, `Page`| _double_ \| `initial` |`padding-top: 4;`|
|`padding-right`|`Button`, `ImageButton`, `Layout`, `Page`| _double_ \| `initial` |`padding-right: 2;`|
|`padding-bottom`|`Button`, `ImageButton`, `Layout`, `Page`| _double_ \| `initial` |`padding-bottom: 6;`|
|`position`|`FlexLayout`| `relative` \| `absolute` \| `initial`|`position: absolute;`|
|`row-gap`|`Grid`| _double_ \| `initial`|`row-gap: 12;`|
|`text-align`| `Entry`, `EntryCell`, `Label`, `SearchBar`|`left` \| `top` \| `right` \| `bottom` \| `start` \| `center` \| `middle` \| `end` \| `initial`. `left`und `right` sollten in Umgebungen mit von rechts nach links vermieden werden.| `text-align: right;`|
|`text-decoration`|`Label`, `Span`|`none` \| `underline` \| `strikethrough` \| `line-through` \| `initial`|`text-decoration: underline, line-through;`|
|`transform`|`VisualElement`| `none`, `rotate`, `rotateX`, `rotateY`, `scale`, `scaleX`, `scaleY`, `translate`, `translateX`, `translateY`, `initial` |`transform: rotate(180), scaleX(2.5);`|
|`transform-origin`|`VisualElement`| _Double_, _Double_ \|`initial` |`transform-origin: 7.5, 12.5;`|
|`vertical-align`|`Label`|`left` \| `top` \| `right` \| `bottom` \| `start` \| `center` \| `middle` \| `end` \| `initial`|`vertical-align: bottom;`|
|`visibility`|`VisualElement`|`true` \| `visible` \| `false` \| `hidden` \| `collapse` \| `initial`|`visibility: hidden;`|
|`width`|`VisualElement`|_double_ \| `initial`|`min-width: 320;`|

> [!NOTE]
> `initial`ist ein gültiger Wert für alle Eigenschaften. Der Wert (wird auf den Standardwert zurückgesetzt), der aus einem anderen Format festgelegt wurde, wird gelöscht.

Die folgenden Eigenschaften werden zurzeit nicht unterstützt:

- `all: initial`.
- Layouteigenschaften (Feld oder Raster).
- Kurzform Eigenschaften, z `font` `border` . b. und.

Außerdem gibt es keinen Wert, `inherit` sodass keine Vererbung unterstützt wird. Daher können Sie beispielsweise die `font-size` -Eigenschaft nicht für ein Layout festlegen und erwarten, dass alle [`Label`](xref:Xamarin.Forms.Label) Instanzen im Layout den Wert erben. Die einzige Ausnahme ist die- `direction` Eigenschaft, die über den Standardwert verfügt `inherit` .

Ziel `Span` Elemente haben ein bekanntes Problem, das verhindert, dass spannen durch Element und Name (mit dem Symbol) das Ziel von CSS-Stilen werden `#` . Das- `Span` Element wird von abgeleitet `GestureElement` , das nicht über die-Eigenschaft verfügt, `StyleClass` sodass spannen keine CSS-Klassen Ausrichtung unterstützen. Weitere Informationen finden Sie unter nicht in der Lage, CSS-Stile [auf die Span-Steuerelemente anzuwenden](https://github.com/xamarin/Xamarin.Forms/issues/5979).

### <a name="xamarinforms-specific-properties"></a>Xamarin.Formsbestimmte Eigenschaften

Die folgenden Xamarin.Forms spezifischen CSS-Eigenschaften werden ebenfalls unterstützt (in der Spalte **Werte** sind die Typen _kursiv_formatiert, während Zeichenfolgenliterale lauten `gray` ):

|Eigenschaft|Anwendungsbereich|Werte|Beispiel|
|---|---|---|---|
|`-xf-bar-background-color`|`NavigationPage`, `TabbedPage`|_Farbe_ \|`initial` |`-xf-bar-background-color: teal;`|
|`-xf-bar-text-color`|`NavigationPage`, `TabbedPage`|_Farbe_ \|`initial` |`-xf-bar-text-color: gray`|
|`-xf-horizontal-scroll-bar-visibility`|`ScrollView`| `default` \| `always` \| `never` \| `initial` |`-xf-horizontal-scroll-bar-visibility: never;`|
|`-xf-max-length`|`Entry`, `Editor`, `SearchBar`|_INT_ \| `initial` |`-xf-max-length: 20;`|
|`-xf-max-track-color`|`Slider`|_Farbe_ \|`initial` |`-xf-max-track-color: red;`|
|`-xf-min-track-color`|`Slider`|_Farbe_ \|`initial` |`-xf-min-track-color: yellow;`|
|`-xf-orientation`|`ScrollView`, `StackLayout`| `horizontal` \| `vertical` \| `both` \| `initial`. `both`wird nur für einen unterstützt `ScrollView` . |`-xf-orientation: horizontal;`|
|`-xf-placeholder`|`Entry`, `Editor`, `SearchBar`|Text in Anführungszeichen _quoted text_ \|`initial` |`-xf-placeholder: Enter name;`|
|`-xf-placeholder-color`|`Entry`, `Editor`, `SearchBar`|_Farbe_ \|`initial` |`-xf-placeholder-color: green;`|
|`-xf-spacing`|`StackLayout`|_double_ \| `initial` |`-xf-spacing: 8;`|
|`-xf-thumb-color`|`Slider`, `Switch`|_Farbe_ \|`initial` |`-xf-thumb-color: limegreen;`|
|`-xf-vertical-scroll-bar-visibility`|`ScrollView`| `default` \| `always` \| `never` \| `initial` |`-xf-vertical-scroll-bar-visibility: always;`|
|`-xf-vertical-text-alignment`|`Label`| `start` \| `center` \| `end` \| `initial`|`-xf-vertical-text-alignment: end;`|
|`-xf-visual`|`VisualElement`|_Zeichenfolge_ \| `initial` |`-xf-visual: material;`|

### <a name="xamarinforms-shell-specific-properties"></a>Xamarin.FormsShellspezifische Eigenschaften

Die folgenden Xamarin.Forms shellspezifischen CSS-Eigenschaften werden ebenfalls unterstützt (in der Spalte **Werte** sind die Typen _kursiv_formatiert, während Zeichenfolgenliterale lauten `gray` ):

|Eigenschaft|Anwendungsbereich|Werte|Beispiel|
|---|---|---|---|
|`-xf-flyout-background`|`Shell`|_Farbe_ \|`initial` |`-xf-flyout-background: red;`|
|`-xf-shell-background`|`Element`|_Farbe_ \|`initial` |`-xf-shell-background: green;`|
|`-xf-shell-disabled`|`Element`|_Farbe_ \|`initial` |`-xf-shell-disabled: blue;`|
|`-xf-shell-foreground`|`Element`|_Farbe_ \|`initial` |`-xf-shell-foreground: yellow;`|
|`-xf-shell-tabbar-background`|`Element`|_Farbe_ \|`initial` |`-xf-shell-tabbar-background: white;`|
|`-xf-shell-tabbar-disabled`|`Element`|_Farbe_ \|`initial` |`-xf-shell-tabbar-disabled: black;`|
|`-xf-shell-tabbar-foreground`|`Element`|_Farbe_ \|`initial` |`-xf-shell-tabbar-foreground: gray;`|
|`-xf-shell-tabbar-title`|`Element`|_Farbe_ \|`initial` |`-xf-shell-tabbar-title: lightgray;`|
|`-xf-shell-tabbar-unselected`|`Element`|_Farbe_ \|`initial` |`-xf-shell-tabbar-unselected: cyan;`|
|`-xf-shell-title`|`Element`|_Farbe_ \|`initial` |`-xf-shell-title: teal;`|
|`-xf-shell-unselected`|`Element`|_Farbe_ \|`initial` |`-xf-shell-unselected: limegreen;`|

### <a name="color"></a>Color

Die folgenden `color` Werte werden unterstützt:

- `X11`[Farben](https://en.wikipedia.org/wiki/X11_color_names), die CSS-Farben, vordefinierte UWP-Farben und Xamarin.Forms Farben entsprechen. Beachten Sie, dass bei diesen Farbwerten die Groß-/Kleinschreibung
- hexadezimale Farben: `#rgb` , `#argb` , `#rrggbb` ,`#aarrggbb`
- RGB-Farben: `rgb(255,0,0)` , `rgb(100%,0%,0%)` . Die Werte liegen im Bereich von 0-255 oder 0%-100%.
- RGBA-Farben: `rgba(255, 0, 0, 0.8)` , `rgba(100%, 0%, 0%, 0.8)` . Der Wert für die Deckkraft liegt im Bereich 0,0-1.0.
- HSL-Farben: `hsl(120, 100%, 50%)` . Der Wert für "h" liegt im Bereich von 0-360, während "s" und "l" im Bereich von 0%-100% liegen.
- HSLA-Farben: `hsla(120, 100%, 50%, .8)` . Der Wert für die Deckkraft liegt im Bereich 0,0-1.0.

### <a name="thickness"></a>Stärke

Ein, zwei, drei oder vier `thickness` Werte werden unterstützt, die jeweils durch Leerraum voneinander getrennt sind:

- Ein einzelner Wert gibt eine einheitliche Stärke an.
- Zwei Werte geben vertikale und horizontale Dicke an.
- Drei Werte geben Top, dann horizontal (Links und rechts) und dann untere Dicke an.
- Vier Werte geben Top, dann right, then Bottom und dann Left Dicke an.

> [!NOTE]
> CSS- `thickness` Werte unterscheiden sich von XAML- [`Thickness`](xref:Xamarin.Forms.Thickness) Werten. In XAML gibt z. b. ein Wert mit zwei Werten die `Thickness` horizontale und vertikale Dicke an, während ein Wert von vier Werten `Thickness` left, then Top, right, then Bottom Dicke angibt. Außerdem werden XAML- `Thickness` Werte durch Kommas getrennt.

### <a name="namedsize"></a>Namedsize

Die folgenden Werte, die beachtet `namedsize` werden, werden unterstützt:

- `default`
- `micro`
- `small`
- `medium`
- `large`

Die genaue Bedeutung der einzelnen `namedsize` Werte ist plattformabhängig und Ansichts abhängig.

## <a name="css-in-xamarinforms-with-xamarinuniversity"></a>CSS in Xamarin.Forms mit xamarin. University

> [!VIDEO https://youtube.com/embed/va-Vb7vtan8]

**Xamarin.Forms3,0 CSS-Video**

## <a name="related-links"></a>Verwandte Links

- [Monkeyappcss (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-monkeyappcss)
- [Ressourcen Wörterbücher](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Formatieren von Xamarin.Forms-Apps mithilfe von XAML-Formatvorlagen](~/xamarin-forms/user-interface/styles/xaml/index.md)
