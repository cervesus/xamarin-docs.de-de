---
title: Formatieren von Xamarin.Forms-Apps, die mithilfe von Cascading Stylesheets (CSS)
description: Xamarin.Forms unterstützt visuelle Stile-Elemente mithilfe von Cascading Stylesheets (CSS).
ms.prod: xamarin
ms.assetid: C89D57A6-DAB9-4C42-963F-26D67627DDC2
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 12/13/2018
ms.openlocfilehash: f84a6dac64300eb17a45576ae83f9b94208f5732
ms.sourcegitcommit: 93c9fe61eb2cdfa530960b4253eb85161894c882
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55831923"
---
# <a name="styling-xamarinforms-apps-using-cascading-style-sheets-css"></a>Formatieren von Xamarin.Forms-apps, die mithilfe von Cascading Stylesheets (CSS)

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/MonkeyAppCSS/)

_Xamarin.Forms unterstützt visuelle Stile-Elemente mithilfe von Cascading Stylesheets (CSS)._

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
> Derzeit können nicht alle Teil des Designs, die mit XAML-Formatierung kann mit CSS ausgeführt werden. Allerdings können der XAML-Stile verwendet werden, CSS für Eigenschaften zu ergänzen, die derzeit von Xamarin.Forms nicht unterstützt werden. Weitere Informationen zu XAML-Stile, finden Sie unter [Formatieren von Xamarin.Forms-Apps mithilfe von XAML-Stile](~/xamarin-forms/user-interface/styles/xaml/index.md).

Die [MonkeyAppCSS](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/MonkeyAppCSS/) Beispiel veranschaulicht die Verwendung von CSS zum Formatieren der einer einfachen app, und wird in den folgenden Screenshots dargestellt:

[![MonkeyApp-Hauptseite mit CSS-Stile](css-images/MonkeyAppMainPage.png "MonkeyApp-Hauptseite mit CSS-Stile")](css-images/MonkeyAppMainPage-Large.png#lightbox "MonkeyApp-Hauptseite mit CSS-Stile")

[![MonkeyApp Detailseite mit CSS-Stile](css-images/MonkeyAppDetailPage.png "MonkeyApp Detailseite mit CSS-Stile")](css-images/MonkeyAppDetailPage-Large.png#lightbox "MonkeyApp Detailseite mit CSS-Stile")

## <a name="consuming-a-style-sheet"></a>Nutzen ein Stylesheet

Der Prozess zum Hinzufügen eines Stylesheets zu einer Lösung lautet wie folgt aus:

1. Fügen Sie eine leere CSS-Datei zu Ihrem Projekt für .NET Standard-Bibliothek hinzu.
1. Legen Sie die Buildaktion der CSS-Datei, die **EmbeddedResource**.

### <a name="loading-a-style-sheet"></a>Beim Laden eines Stylesheets

Es gibt eine Reihe von Ansätzen, die zum Laden eines Stylesheets verwendet werden kann.

### <a name="xaml"></a>XAML

Ein Stylesheet geladen und mit analysiert werden kann, die [ `StyleSheet` ](xref:Xamarin.Forms.StyleSheets.StyleSheet) Klasse hinzugefügt werden, bevor Sie eine [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary):

```xaml
<Application ...>
    <Application.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </Application.Resources>
</Application>
```

Die [ `StyleSheet.Source` ](xref:Xamarin.Forms.Xaml.StyleSheetExtension.Source) Eigenschaft gibt das Stylesheet als URI relativ zum Speicherort der einschließenden XAML-Datei oder Bezug auf das Stammverzeichnis des Projekts an, wenn der URI mit beginnt eine `/`.

> [!WARNING]
> Die CSS-Datei kann nicht geladen werden, wenn der Buildvorgang nicht, um festgelegt ist **EmbeddedResource**.

Alternativ ein Stylesheet geladen und mit analysiert werden kann die [ `StyleSheet` ](xref:Xamarin.Forms.StyleSheets.StyleSheet) -Klasse, bevor Sie hinzugefügt eine [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), inlining in einer `CDATA` Abschnitt:

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

Weitere Informationen zu Ressourcenwörterbüchern, finden Sie unter [Ressourcenverzeichnisse](~/xamarin-forms/xaml/resource-dictionaries.md).

### <a name="c"></a>C#

In C#, ein Stylesheet als eingebettete Ressource geladen und hinzugefügt werden kann, eine [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary):

```csharp
public partial class MyPage : ContentPage
{
    public MyPage()
    {
        InitializeComponent();

        this.Resources.Add(StyleSheet.FromAssemblyResource(
            IntrospectionExtensions.GetTypeInfo(typeof(MyPage)).Assembly,
            "MyProject.Assets.styles.css"));
    }
}
```

Das erste Argument für die `StyleSheet.FromAssemblyResource` Methode ist die Assembly mit dem Stylesheet, während das zweite Argument ist ein `string` , die den Ressourcenbezeichner darstellt. Der Ressourcenbezeichner abgerufen werden kann, aus der **Eigenschaften** Wartungszeitfensters, in die CSS-Datei ausgewählt ist.

Alternativ kann ein Stylesheet geladen werden, von einem `StringReader` hinzugefügt eine [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary):

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

Das Argument für die `StyleSheet.FromReader` Methode ist die `TextReader` hat, das das Stylesheet gelesen.

## <a name="selecting-elements-and-applying-properties"></a>Auswählen von Elementen und Eigenschaften anwenden

CSS wird Selektoren verwendet, um zu bestimmen, welche Elemente als Ziel. Stile mit übereinstimmenden Selektoren werden nacheinander, in der Definition Reihenfolge angewendet. Für ein bestimmtes Element definierten Stile werden immer zuletzt angewendet. Weitere Informationen zu unterstützten Selektoren, finden Sie unter [Selektor Verweis](#selector-reference).

CSS verwendet zum Formatieren eines ausgewählten Elements. Jede Eigenschaft verfügt über einen Satz möglicher Werte ein, und einige Eigenschaften können jede Art von Element wirken sich auf, während andere auf Gruppen von Elementen anwenden. Weitere Informationen zu unterstützten Eigenschaften finden Sie unter [Eigenschaftsverweis](#property-reference).

### <a name="selecting-elements-by-type"></a>Auswählen von Elementen nach Typ

Elemente in der visuellen Struktur ausgewählt werden können, nach Typ mit der Groß-/Kleinschreibung `element` Selektor:

```css
stacklayout {
    margin: 20;
}
```

Diese Auswahl identifiziert alle [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) Elementen auf Seiten, die das Stylesheet nutzen und ihre Ränder festgelegt, um eine einheitliche Breite von 20.

> [!NOTE]
> Die `element` Auswahl gibt nicht an untergeordnete Klassen vom angegebenen Typ.

### <a name="selecting-elements-by-base-class"></a>Auswählen von Elementen durch Basisklasse

Elemente in der visuellen Struktur ausgewählt werden können, von der Basisklasse mit der Groß-/Kleinschreibung `^base` Selektor:

```css
^contentpage {
    background-color: lightgray;
}
```

Diese Auswahl identifiziert alle [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) Elemente, die das Stylesheet nutzen, und werden ihren Hintergrund Farbe `lightgray`.

> [!NOTE]
> Die `^base` Selektor bezieht sich auf Xamarin.Forms und ist nicht Teil der CSS-Spezifikation.

### <a name="selecting-an-element-by-name"></a>Auswählen eines Elements anhand des Namens

Einzelne Elemente in der visuellen Struktur ausgewählt werden können, mit der Groß-/ Kleinschreibung `#id` Selektor:

```css
#listView {
    background-color: lightgray;
}
```

Diese Auswahl gibt das Element, dessen [ `StyleId` ](xref:Xamarin.Forms.Element.StyleId) -Eigenschaftensatz auf `listView`. Aber wenn die `StyleId` Eigenschaft nicht festgelegt ist, die Auswahl wird ein Fallback auf mit der `x:Name` des Elements. Daher in den folgenden XAML-Beispiel das `#listView` Selektor erkennt die [ `ListView` ](xref:Xamarin.Forms.ListView) , deren `x:Name` -Attributsatz auf `listView`, und legt die Hintergrundfarbe auf `lightgray`.

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

Elemente mit einer bestimmten Klasse-Attribut ausgewählt werden können, mit der Groß-/ Kleinschreibung `.class` Selektor:

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

Eine CSS-Klasse kann an ein XAML-Element zugewiesen werden, durch Festlegen der [ `StyleClass` ](xref:Xamarin.Forms.VisualElement.StyleClass) Eigenschaft des Elements, das den Namen der CSS-Klasse. Aus diesem Grund in den folgenden XAML-Beispiel, die Stile definiert durch die `.detailPageTitle` Klasse zugewiesen werden, mit dem ersten [ `Label` ](xref:Xamarin.Forms.Label), zwar von definierten Stile der `.detailPageSubtitle` Klasse zugewiesen werden, mit dem zweiten `Label`.

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

Untergeordnete Elemente in der visuellen Struktur ausgewählt werden können, mit der Groß-/Kleinschreibung `element element` Selektor:

```css
listview image {
    height: 60;
    width: 60;
}
```

Diese Auswahl identifiziert alle [ `Image` ](xref:Xamarin.Forms.Image) Elemente, die untergeordneten Elemente des [ `ListView` ](xref:Xamarin.Forms.ListView) Elemente und ihrer Höhe und Breite auf 60 festgelegt. Daher in den folgenden XAML-Beispiel die `listview image` Selektor erkennt die [ `Image` ](xref:Xamarin.Forms.Image) ist ein untergeordnetes Element des der [ `ListView` ](xref:Xamarin.Forms.ListView), und seine Höhe und Breite auf 60 festgelegt.

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
> Die `element element` Auswahl erfordert das untergeordnete Element, werden keine _direkte_ untergeordnetes Element des übergeordneten Elements – das untergeordnete Element ist möglicherweise ein anderes übergeordnetes Element. Auswahl vorgenommen wird, vorausgesetzt, dass ein Vorgänger das erste angegebene Element ist.

### <a name="selecting-direct-child-elements"></a>Auswählen von direkt untergeordneten Elementen

Direkte untergeordnete Elemente in der visuellen Struktur ausgewählt werden können, mit der Groß-/Kleinschreibung `element>element` Selektor:

```css
stacklayout>image {
    height: 200;
    width: 200;
}
```

Diese Auswahl identifiziert alle [ `Image` ](xref:Xamarin.Forms.Image) Elemente, die direkt untergeordnete Elemente eines [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) Elemente und legt die Höhe und Breite auf 200 fest. Daher in den folgenden XAML-Beispiel das `stacklayout>image` Selektor erkennt der [ `Image` ](xref:Xamarin.Forms.Image) direkt untergeordnet ist der [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), und seine Höhe und Breite auf 200 festgelegt.

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
> Die `element>element` Auswahl erfordert, dass das untergeordnete Element ist ein _direkte_ untergeordnetes Element des übergeordneten Elements.

## <a name="selector-reference"></a>Selector-Referenz

Die folgenden CSS-Selektoren werden von Xamarin.Forms unterstützt:

|Auswahl|Beispiel|Beschreibung|
|---|---|---|
|`.class`|`.header`|Wählt alle Elemente mit den `StyleClass` Eigenschaft, die "Header" enthält. Beachten Sie, dass diese Auswahl Groß-/Kleinschreibung beachtet wird.|
|`#id`|`#email`|Wählt alle Elemente mit `StyleId` festgelegt `email`. Wenn `StyleId` nicht festgelegt ist, um alternative `x:Name`. Bei Verwendung von XAML, `x:Name` vorzuziehen ist `StyleId`. Beachten Sie, dass diese Auswahl Groß-/Kleinschreibung beachtet wird.|
|`*`|`*`|Wählt alle Elemente aus.|
|`element`|`label`|Wählt alle Elemente des Typs `Label`, aber nicht von untergeordneten Klassen. Beachten Sie, dass diese Auswahl Groß-/Kleinschreibung.|
|`^base`|`^contentpage`|Wählt alle Elemente mit `ContentPage` als Basisklasse, einschließlich `ContentPage` selbst. Beachten Sie, dass diese Auswahl wird die Groß-/Kleinschreibung und ist nicht Teil der CSS-Spezifikation.|
|`element,element`|`label,button`|Wählt alle `Button` Elemente und alle `Label` Elemente. Beachten Sie, dass diese Auswahl Groß-/Kleinschreibung.|
|`element element`|`stacklayout label`|Wählt alle `Label` Elemente innerhalb einer `StackLayout`. Beachten Sie, dass diese Auswahl Groß-/Kleinschreibung.|
|`element>element`|`stacklayout>label`|Wählt alle `Label` Elemente mit `StackLayout` als direkt übergeordnet. Beachten Sie, dass diese Auswahl Groß-/Kleinschreibung.|
|`element+element`|`label+entry`|Wählt alle `Entry` Elemente direkt nach einem `Label`. Beachten Sie, dass diese Auswahl Groß-/Kleinschreibung.|
|`element~element`|`label~entry`|Wählt alle `Entry` Elementen vorangestellt wird, wird eine `Label`. Beachten Sie, dass diese Auswahl Groß-/Kleinschreibung.|

Stile mit übereinstimmenden Selektoren werden nacheinander, in der Definition Reihenfolge angewendet. Für ein bestimmtes Element definierten Stile werden immer zuletzt angewendet.

> [!TIP]
> Selektoren können kombiniert werden, ohne Einschränkung, wie z. B. `StackLayout>ContentView>label.email`.

Folgenden Auswahlmöglichkeiten werden zurzeit nicht unterstützt:

- `[attribute]`
- `@media` und `@supports`
- `:` und `::`

> [!NOTE]
> Spezifität und Spezifität Außerkraftsetzungen werden nicht unterstützt.

## <a name="property-reference"></a>Referenz zur Eigenschaft

Die folgenden CSS-Eigenschaften werden von Xamarin.Forms unterstützt (in der **Werte** Spalte Typen sind _Kursiv_, während Zeichenfolgenliterale sind `gray`):

|Eigenschaft|Betrifft|Werte|Beispiel|
|---|---|---|---|
|`align-content`|`FlexLayout`| `stretch` \| `center` \| `start` \| `end` \| `spacebetween` \| `spacearound` \| `spaceevenly` \| `flex-start` \| `flex-end` \| `space-between` \| `space-around` \| `initial` |`align-content: space-between;`|
|`align-items`|`FlexLayout`| `stretch` \| `center` \| `start` \| `end` \| `flex-start` \| `flex-end` \| `initial` |`align-items: flex-start;`|
|`align-self`|`VisualElement`| `auto` \| `stretch` \| `center` \| `start` \| `end` \| `flex-start` \| `flex-end` \| `initial`|`align-self: flex-end;`|
|`background-color`|`VisualElement`|_Farbe_ \| `initial` |`background-color: springgreen;`|
|`background-image`|`Page`|_Zeichenfolge_ \| `initial` |`background-image: bg.png;`|
|`border-color`|`Button`, `Frame`, `ImageButton`|_Farbe_ \| `initial`|`border-color: #9acd32;`|
|`border-radius`|`BoxView`, `Button`, `Frame`, `ImageButton`|_Double-Wert_ \| `initial` |`border-radius: 10;`|
|`border-width`|`Button`, `ImageButton`|_Double-Wert_ \| `initial` |`border-width: .5;`|
|`color`|`ActivityIndicator`, `BoxView`, `Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `ProgressBar`, `SearchBar`, `Switch`, `TimePicker`|_Farbe_ \| `initial` |`color: rgba(255, 0, 0, 0.3);`|
|`column-gap`|`Grid`|_Double-Wert_ \| `initial`|`column-gap: 9;`|
|`direction`|`VisualElement`|`ltr` \| `rtl` \| `inherit` \| `initial` |`direction: rtl;`|
|`flex-direction`|`FlexLayout`| `column` \| `columnreverse` \| `row` \| `rowreverse` \| `row-reverse` \| `column-reverse` \| `initial`|`flex-direction: column-reverse;`|
|`flex-basis`|`VisualElement`|_"float"_ \| `auto` \| `initial`. Darüber hinaus kann ein Prozentsatz in den Bereich 0 bis 100 % angegeben werden, mit der `%` anmelden.|`flex-basis: 25%;`|
|`flex-grow`|`VisualElement`|_"Float"_ \| `initial`|`flex-grow: 1.5;`|
|`flex-shrink`|`VisualElement`|_"Float"_ \| `initial`|`flex-shrink: 1;`|
|`flex-wrap`|`VisualElement`| `nowrap` \| `wrap` \| `reverse` \| `wrap-reverse` \| `initial`|`flex-wrap: wrap-reverse;`|
|`font-family`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_Zeichenfolge_ \| `initial` |`font-family: Consolas;`|
|`font-size`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_doppelte_ \| _Namedsize_ \| `initial` |`font-size: 12;`|
|`font-style`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|`bold` \| `italic` \| `initial` |`font-style: bold;`|
|`height`|`VisualElement`|_Double-Wert_ \| `initial` |`min-height: 250;`|
|`justify-content`|`FlexLayout`| `start` \| `center` \| `end` \| `spacebetween` \| `spacearound` \| `spaceevenly` \| `flex-start` \| `flex-end` \| `space-between` \| `space-around` \| `initial`|`justify-content: flex-end;`|
|`line-height`|`Label`, `Span`|_Double-Wert_ \| `initial` |`line-height: 1.8;`|
|`margin`|`View`|_Stärke_ \| `initial` |`margin: 6 12;`|
|`margin-left`|`View`|_Stärke_ \| `initial` |`margin-left: 3;`|
|`margin-top`|`View`|_Stärke_ \| `initial` |`margin-top: 2;`|
|`margin-right`|`View`|_Stärke_ \| `initial` |`margin-right: 1;`|
|`margin-bottom`|`View`|_Stärke_ \| `initial` |`margin-bottom: 6;`|
|`max-lines`|`Label`|_Int_ \| `initial`|`max-lines: 2;`|
|`min-height`|`VisualElement`|_Double-Wert_ \| `initial` |`min-height: 50;`|
|`min-width`|`VisualElement`|_Double-Wert_ \| `initial` |`min-width: 112;`|
|`opacity`|`VisualElement`|_Double-Wert_ \| `initial` |`opacity: .3;`|
|`order`|`VisualElement`|_Int_ \| `initial`|`order: -1;`|
|`padding`|`Button`, `ImageButton`, `Layout`, `Page`|_Stärke_ \| `initial` |`padding: 6 12 12;`|
|`padding-left`|`Button`, `ImageButton`, `Layout`, `Page`|_Double-Wert_ \| `initial`|`padding-left: 3;`|
|`padding-top`|`Button`, `ImageButton`, `Layout`, `Page`| _Double-Wert_ \| `initial` |`padding-top: 4;`|
|`padding-right`|`Button`, `ImageButton`, `Layout`, `Page`| _Double-Wert_ \| `initial` |`padding-right: 2;`|
|`padding-bottom`|`Button`, `ImageButton`, `Layout`, `Page`| _Double-Wert_ \| `initial` |`padding-bottom: 6;`|
|`position`|`FlexLayout`| `relative` \| `absolute` \| `initial`|`position: absolute;`|
|`row-gap`|`Grid`| _Double-Wert_ \| `initial`|`row-gap: 12;`|
|`text-align`| `Entry`, `EntryCell`, `Label`, `SearchBar`|`left` \| `top` \| `right` \| `bottom` \| `start` \| `center` \| `middle` \| `end` \| `initial`. `left` und `right` sollte in Umgebungen mit rechts-nach-links-vermieden werden.| `text-align: right;`|
|`text-decoration`|`Label`, `Span`|`none` \| `underline` \| `strikethrough` \| `line-through` \| `initial`|`text-decoration: underline, line-through;`|
|`transform`|`VisualElement`| `none`, `rotate`, `rotateX`, `rotateY`, `scale`, `scaleX`, `scaleY`, `translate`, `translateX`, `translateY`, `initial` |`transform: rotate(180), scaleX(2.5);`|
|`transform-origin`|`VisualElement`| _doppelte_, _double_ \| `initial` |`transform-origin: 7.5, 12.5;`|
|`vertical-align`|`Label`|`left` \| `top` \| `right` \| `bottom` \| `start` \| `center` \| `middle` \| `end` \| `initial`|`vertical-align: bottom;`|
|`visibility`|`VisualElement`|`true` \| `visible` \| `false` \| `hidden` \| `collapse` \| `initial `|`visibility: hidden;`|
|`width`|`VisualElement`|_Double-Wert_ \| `initial`|`min-width: 320;`|

Die folgenden Xamarin.Forms bestimmten CSS-Eigenschaften werden ebenfalls unterstützt. (in der **Werte** Spalte Typen sind _Kursiv_, während Zeichenfolgenliterale sind `gray`):

|Eigenschaft|Betrifft|Werte|Beispiel|
|---|---|---|---|
|`-xf-placeholder`|`Entry`, `Editor`, `SearchBar`|_Text in Anführungszeichen_ \| `initial` |`-xf-placeholder: Enter name;`|
|`-xf-placeholder-color`|`Entry`, `Editor`, `SearchBar`|_Farbe_ \| `initial` |`-xf-placeholder-color: green;`|
|`-xf-max-length`|`Entry`, `Editor`|_Int_ \| `initial` |`-xf-max-length: 20;`|
|`-xf-bar-background-color`|`NavigationPage`, `TabbedPage`|_Farbe_ \| `initial` |`-xf-bar-background-color: teal;`|
|`-xf-bar-text-color`|`NavigationPage`, `TabbedPage`|_Farbe_ \| `initial` |`-xf-bar-text-color: gray`|
|`-xf-orientation`|`ScrollView`, `StackLayout`| `horizontal` \| `vertical` \| `both` \| `initial`. `both` wird nur unterstützt, auf eine `ScrollView`. |`-xf-orientation: horizontal;`|
|`-xf-horizontal-scroll-bar-visibility`|`ScrollView`| `default` \| `always` \| `never` \| `initial` |`-xf-horizontal-scroll-bar-visibility: never;`|
|`-xf-vertical-scroll-bar-visibility`|`ScrollView`| `default` \| `always` \| `never` \| `initial` |`-xf-vertical-scroll-bar-visibility: always;`|
|`-xf-min-track-color`|`Slider`|_Farbe_ \| `initial` |`-xf-min-track-color: yellow;`|
|`-xf-max-track-color`|`Slider`|_Farbe_ \| `initial` |`-xf-max-track-color: red;`|
|`-xf-thumb-color`|`Slider`|_Farbe_ \| `initial` |`-xf-thumb-color: limegreen;`|
|`-xf-spacing`|`StackLayout`|_Double-Wert_ \| `initial` |`-xf-spacing: 8;`|

> [!NOTE]
> `initial` ist ein gültiger Wert für alle Eigenschaften. Es löscht den Wert (den Standardwert zurückgesetzt), der von einem anderen Stil festgelegt wurde.

Die folgenden Eigenschaften werden derzeit nicht unterstützt:

- `all: initial`.
- Layout-Eigenschaften (Feld oder Raster).
- Kompakteigenschaften, z. B. `font`, und `border`.

Darüber hinaus besteht keine `inherit` Wert und daher Vererbung wird nicht unterstützt. Legen Sie daher nicht möglich ist, z. B. die `font-size` Eigenschaft für ein Layout und erwarten, dass alle der [ `Label` ](xref:Xamarin.Forms.Label) -Instanzen in das Layout, das den Wert erben. Die einzige Ausnahme ist die `direction` -Eigenschaft, die einen Standardwert besitzt der `inherit`.

### <a name="color"></a>Farbe

Die folgenden `color` Werte werden unterstützt:

- `X11` [Farben](https://en.wikipedia.org/wiki/X11_color_names/), die mit CSS-Farben, UWP vordefinierte Farben und Farben von Xamarin.Forms übereinstimmen. Beachten Sie, dass diese Farbwerte Groß-/Kleinschreibung.
- Hex-Farben: `#rgb`, `#argb`, `#rrggbb`, `#aarrggbb`
- RGB-Farben: `rgb(255,0,0)`, `rgb(100%,0%,0%)`. Werte liegen im Bereich 0 – 100 % oder 0-255.
- RGBA-Farben: `rgba(255, 0, 0, 0.8)`, `rgba(100%, 0%, 0%, 0.8)`. Der Durchlässigkeitswert ist im Bereich von 0,0 bis 1,0.
- HSL-Farben: `hsl(120, 100%, 50%)`. Der h-Wert wird im Bereich zwischen 0 und 360, s und l sind in den Bereich 0 – 100 %.
- HSLA Farben: `hsla(120, 100%, 50%, .8)`. Der Durchlässigkeitswert ist im Bereich von 0,0 bis 1,0.

### <a name="thickness"></a>Stärke

Einem, zwei, drei oder vier `thickness` Werte werden unterstützt, die jeweils durch ein Leerzeichen voneinander getrennt sind:

- Ein einzelner Wert gibt die einheitliche Stärke an.
- Zwei Werte geben die vertikale und horizontale Breite an.
- Drei Werte geben die Breite des unteren und oberen dann Horizontal (links und rechts).
- Vier Werte geben an, oben, und klicken Sie dann rechts, unten und dann linken Stärke.

> [!NOTE]
> CSS `thickness` Werte unterscheiden sich von XAML [ `Thickness` ](/api/type/Xamarin.Forms.Thickness/) Werte. Z. B. in XAML ein zweiwertiger `Thickness` gibt die horizontale und vertikale Stärke, während ein vier-Wert an `Thickness` gibt an, nach rechts und Links, dann oben nach unten Stärke. Darüber hinaus XAML `Thickness` Werte sind durch Kommas getrennt.

### <a name="namedsize"></a>NamedSize

Die folgenden Groß-/Kleinschreibung `namedsize` Werte werden unterstützt:

- `default`
- `micro`
- `small`
- `medium`
- `large`

Die genaue Bedeutung der einzelnen `namedsize` Wert ist plattformabhängig und abhängige anzeigen.

## <a name="css-in-xamarinforms-with-xamarinuniversity"></a>CSS-Code in Xamarin.Forms mit Xamarin.University

> [!VIDEO https://youtube.com/embed/va-Vb7vtan8]

**Xamarin.Forms 3.0 CSS, [Xamarin University](https://university.xamarin.com/)**

## <a name="related-links"></a>Verwandte Links

- [MonkeyAppCSS (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/MonkeyAppCSS/)
- [Ressourcenverzeichnisse](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Formatieren von Xamarin.Forms-Apps mithilfe von XAML-Formatvorlagen](~/xamarin-forms/user-interface/styles/xaml/index.md)
