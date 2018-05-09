---
title: Formatieren Sie Xamarin.Forms-Apps mithilfe von Cascading Stylesheets
description: Xamarin.Forms unterstützt Styling visuelle Elemente, die mithilfe von Cascading Stylesheets (CSS).
ms.prod: xamarin
ms.assetid: C89D57A6-DAB9-4C42-963F-26D67627DDC2
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/07/2018
ms.openlocfilehash: 811abacff330bf7b6e6240691cb6a15ebbd9d242
ms.sourcegitcommit: daa089d41cfe1ed0456d6de2f8134cf96ae072b1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="styling-xamarinforms-apps-using-cascading-style-sheets"></a>Formatieren Sie Xamarin.Forms-apps mithilfe von Cascading Stylesheets

_Xamarin.Forms unterstützt Styling visuelle Elemente, die mithilfe von Cascading Stylesheets (CSS)._

Xamarin.Forms 3.0 stellt die Möglichkeit, eine app, die Verwendung von CSS-Stil. Ein Stylesheet besteht aus einer Liste von Regeln, mit jeder Regel einen oder mehrere Auswahlzeiger und einen Deklarationsblock besteht. Ein Deklarationsblock besteht aus einer Liste der Deklarationen in der geschweiften Klammern, mit jeder Deklaration besteht aus einer Eigenschaft, einen Doppelpunkt und einem Wert. Wenn mehrere Deklarationen in einem Block vorhanden sind, wird ein Semikolon als Trennzeichen eingefügt. Das folgende Codebeispiel zeigt einige Xamarin.Forms kompatibel CSS:

```css
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

In Xamarin.Forms mit CSS-Stylesheets werden analysiert und zur Laufzeit statt Kompilierzeit ausgewertet, und Stylesheets werden bei der Verwendung erneut analysiert.

> [!NOTE]
> Derzeit kann alle die Formatvorlage, die bei Verwendung von XAML-Format mit CSS ausgeführt werden. Verwendung von XAML-Formate können jedoch verwendet werden, CSS für Eigenschaften zu ergänzen, die von Xamarin.Forms derzeit nicht unterstützt werden. Weitere Informationen zur Verwendung von XAML-Formate finden Sie unter [formatieren Xamarin.Forms-Apps mit Verwendung von XAML-Stile](~/xamarin-forms/user-interface/styles/xaml/index.md).

Die [MonkeyAppCSS](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/MonkeyAppCSS/) Beispiel veranschaulicht die Verwendung von CSS so formatieren Sie eine einfache app, und in den folgenden Screenshots angezeigt wird:

[![Hauptseite mit CSS-Stilen MonkeyApp](css-images/MonkeyAppMainPage.png "MonkeyApp-Hauptseite mit CSS-Stilen")](css-images/MonkeyAppMainPage-Large.png#lightbox "MonkeyApp-Hauptseite mit CSS-Stilen")

[![Detailseite mit CSS-Stilen MonkeyApp](css-images/MonkeyAppDetailPage.png "MonkeyApp Detailseite mit CSS-Stilen")](css-images/MonkeyAppDetailPage-Large.png#lightbox "MonkeyApp Detailseite mit CSS-Stilen")

> [!NOTE]
> Es ist nicht aktuell möglich so formatieren Sie die Farbe des Hintergrunds einer [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) mithilfe eines Stylesheets. Aus diesem Grund in der beispielanwendung der [ `NavigationPage.BarBackgroundColor` ](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor) Eigenschaft im Code festgelegt ist.

## <a name="consuming-a-style-sheet"></a>Verarbeiten eines Stylesheets

Die Vorgehensweise zum Hinzufügen eines Stylesheets zu einer Projektmappe lautet wie folgt:

1. Eine leere CSS-Datei dem standardmäßigen .NET Library-Projekt hinzufügen.
1. Legen Sie den Buildvorgang der CSS-Datei, die **EmbeddedResource**.

### <a name="loading-a-style-sheet"></a>Beim Laden eines Stylesheets

Es gibt mehrere Verfahren, die zum Laden eines Stylesheets verwendet werden kann.

### <a name="xaml"></a>XAML

Ein Stylesheet geladen und mit analysiert werden kann die [ `StyleSheet` ](xref:Xamarin.Forms.StyleSheets.StyleSheet) Klasse, bevor Sie hinzugefügt werden, um die [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) für die Seite:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    ...
</ContentPage>
```

Die [ `StyleSheet.Source` ](xref:Xamarin.Forms.Xaml.StyleSheetExtension.Source) Eigenschaft gibt das Stylesheet als relativ zum Speicherort der einschließenden XAML-Datei oder relativ zum Projektstamm-URI an, ob der URI mit beginnt ein `/`.

> [!WARNING]
> Die CSS-Datei wird nicht geladen werden, ist er Buildvorgang ist nicht festgelegt, um **EmbeddedResource**.

Alternativ kann das Stylesheet geladen und analysiert, mit der [ `StyleSheet` ](xref:Xamarin.Forms.StyleSheets.StyleSheet) -Klasse inlining in einer `CDATA` Abschnitt:

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

### <a name="c"></a>C#

In c# ist ein Stylesheet als eingebettete Ressource geladen und hinzugefügt werden kann die [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) für die Seite:

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

Das erste Argument für die `StyleSheet.FromAssemblyResource` Methode ist die Assembly mit dem Stylesheet, während das zweite Argument ist eine `string` , die den Ressourcenbezeichner darstellt. Der Ressourcenbezeichner abgerufen werden kann, aus der **Eigenschaften** Fenster beim Erstellen die CSS-Datei ausgewählt ist.

Alternativ kann das Stylesheet geladen werden, aus einer `StringReader` und hinzugefügt werden, um die [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) für die Seite:

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

Das Argument für die `StyleSheet.FromReader` Methode ist die `TextReader` , hat das Stylesheet lesen.

## <a name="selecting-elements-and-applying-properties"></a>Auswählen von Elementen und Eigenschaften anwenden.

CSS wird Selektoren verwendet, um zu bestimmen, welche Elemente als Ziel. Stile mit übereinstimmenden Selektoren werden fortlaufend Definition nacheinander angewendet. Für ein bestimmtes Element definierten Stile werden immer zuletzt angewendet. Weitere Informationen zu unterstützten Selektoren, finden Sie unter [Selektor Verweis](#selector-reference).

CSS verwendet Eigenschaften, um ein ausgewähltes Element ein Format zuzuweisen. Jede Eigenschaft verfügt über einen Satz möglicher Werte, und einige Eigenschaften können jeder Typ des Elements, beeinträchtigen, während andere für Gruppen von Elementen gelten. Weitere Informationen zu unterstützten Eigenschaften finden Sie unter [Eigenschaftsverweis](#property-reference).

### <a name="selecting-elements-by-type"></a>Auswählen von Elementen vom Typ

Elemente in der visuellen Struktur ausgewählt werden können, nach Typ mit der Groß-/Kleinschreibung beachten `element` Selektor:

```css
stacklayout {
    margin: 20;
}
```

Diese Auswahl identifiziert alle [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) Elemente auf Seiten, die das Stylesheet, und ihre Ränder um einheitliche Stärke von 20 festgelegt.

> [!NOTE]
> Die `element` Selektor Unterklassen des angegebenen Typs nicht identifiziert.

### <a name="selecting-elements-by-base-class"></a>Auswählen von Elementen von der Basisklasse

Elemente in der visuellen Struktur ausgewählt werden können, von der Basisklasse mit der Groß-/Kleinschreibung beachten `^base` Selektor:

```css
^contentpage {
    background-color: lightgray;
}
```

Diese Auswahl identifiziert alle [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) Elemente, die das Stylesheet, und legt deren Hintergrund Farbe `lightgray`.

> [!NOTE]
> Die `^base` -Selektor gilt nur für Xamarin.Forms und ist nicht Teil der CSS-Spezifikation.

### <a name="selecting-an-element-by-name"></a>Auswählen eines Elements anhand des Namens

Einzelne Elemente in der visuellen Struktur ausgewählt werden können, mit der Groß-/Kleinschreibung beachtet `#id` Selektor:

```css
#listView {
    background-color: lightgray;
}
```

Diese Auswahl identifiziert das Element, dessen [ `StyleId` ](xref:Xamarin.Forms.Element.StyleId) -Eigenschaftensatz auf `listView`. Jedoch, wenn die `StyleId` Eigenschaft nicht festgelegt ist, die Auswahl wird ein Fallback auf mit der `x:Name` des Elements. Daher in den folgenden XAML-Beispiel der `#listView` Selektor identifiziert die [ `ListView` ](xref:Xamarin.Forms.ListView) , deren `x:Name` -Attributsatz zur `listView`, und legen Sie die Hintergrundfarbe wird auf `lightgray`.

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

Elemente mit einer bestimmten Klasse-Attribut ausgewählt werden können, mit der Groß-/Kleinschreibung beachtet `.class` Selektor:

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

Kann eine CSS-Klasse ein XAML-Element zugewiesen werden, durch Festlegen der [ `StyleClass` ](xref:Xamarin.Forms.VisualElement.StyleClass) -Eigenschaft des Elements auf den Namen der CSS-Klasse. Aus diesem Grund in folgenden XAML-Beispiel der Stile von definiert die `.detailPageTitle` der ersten Klasse zugewiesen sind [ `Label` ](xref:Xamarin.Forms.Label), while-Formatvorlagen, definiert durch die `.detailPageSubtitle` die zweite Klasse zugewiesen sind `Label`.

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

Untergeordnete Elemente in der visuellen Struktur ausgewählt werden können, mit der Groß-/Kleinschreibung beachten `element element` Selektor:

```css
listview image {
    height: 60;
    width: 60;
}
```

Diese Auswahl identifiziert alle [ `Image` ](xref:Xamarin.Forms.Image) Elemente, die untergeordnete Elemente des [ `ListView` ](xref:Xamarin.Forms.ListView) Elemente, und die Höhe und Breite auf 60 festgelegt. Daher in den folgenden XAML-Beispiel der `listview image` Selektor identifiziert die [ `Image` ](xref:Xamarin.Forms.Image) , ist ein untergeordnetes Element eines der [ `ListView` ](xref:Xamarin.Forms.ListView), und die Höhe und Breite auf 60 festgelegt.

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
> Die `element element` Selektor erfordert das untergeordnete Element werden keine _direkte_ untergeordnetes Element des übergeordneten Elements – das untergeordnete Element möglicherweise ein anderen übergeordneten Element. Auswahl tritt auf, vorausgesetzt, dass ein Vorgänger das erste angegebene Element ist.

### <a name="selecting-direct-child-elements"></a>Auswählen von direkt untergeordneten Elementen

Direkte untergeordnete Elemente in der visuellen Struktur ausgewählt werden können, mit der Groß-/Kleinschreibung beachten `element>element` Selektor:

```css
stacklayout>image {
    height: 200;
    width: 200;
}
```

Diese Auswahl identifiziert alle [ `Image` ](xref:Xamarin.Forms.Image) Elemente, die direkt untergeordneten [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) Elemente, und die Höhe und Breite auf 200 festgelegt. Daher in den folgenden XAML-Beispiel der `stacklayout>image` Selektor identifiziert die [ `Image` ](xref:Xamarin.Forms.Image) , ist ein direkt untergeordnetes Element des der [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), und die Höhe und Breite auf 200 festgelegt.

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
> Die `element>element` Selektor erfordert, dass das untergeordnete Element ist ein _direkte_ untergeordnetes Element des übergeordneten Elements.

## <a name="selector-reference"></a>Auswahl-Referenz

Die folgenden CSS-Selektoren werden von Xamarin.Forms unterstützt:

|Auswahl|Beispiel|Beschreibung|
|---|---|---|
|`.class`|`.header`|Wählt alle Elemente mit der `StyleClass` Eigenschaft "Header" enthält. Beachten Sie, dass diese Auswahl Groß-/Kleinschreibung beachtet wird.|
|`#id`|`#email`|Wählt alle Elemente mit `StyleId` festgelegt `email`. Wenn `StyleId` nicht festgelegt ist, um alternative `x:Name`. Bei Verwendung von XAML-Code `x:Name` ist die bevorzugte über `StyleId`. Beachten Sie, dass diese Auswahl Groß-/Kleinschreibung beachtet wird.|
|`*`|`*`|Wählt alle Elemente aus.|
|`element`|`label`|Wählt alle Elemente des Typs `Label`, jedoch nicht von untergeordneten Klassen. Beachten Sie, dass diese Auswahl Groß-/Kleinschreibung unterschieden wird.|
|`^base`|`^contentpage`|Wählt alle Elemente mit `ContentPage` als Basisklasse, einschließlich `ContentPage` selbst. Beachten Sie, dass diese Auswahl Groß-/Kleinschreibung nicht beachtet wird, und ist nicht Teil der CSS-Spezifikation.|
|`element,element`|`label,button`|Wählt alle `Button` -Elementen und all `Label` Elemente. Beachten Sie, dass diese Auswahl Groß-/Kleinschreibung unterschieden wird.|
|`element element`|`stacklayout label`|Wählt alle `Label` Elemente innerhalb einer `StackLayout`. Beachten Sie, dass diese Auswahl Groß-/Kleinschreibung unterschieden wird.|
|`element>element`|`stacklayout>label`|Wählt alle `Label` Elemente mit `StackLayout` als direkte übergeordnetes Element. Beachten Sie, dass diese Auswahl Groß-/Kleinschreibung unterschieden wird.|
|`element+element`|`label+entry`|Wählt alle `Entry` Elemente direkt hinter einem `Label`. Beachten Sie, dass diese Auswahl Groß-/Kleinschreibung unterschieden wird.|
|`element~element`|`label~entry`|Wählt alle `Entry` Elemente vorangestellt wird, wird eine `Label`. Beachten Sie, dass diese Auswahl Groß-/Kleinschreibung unterschieden wird.|

Stile mit übereinstimmenden Selektoren werden fortlaufend Definition nacheinander angewendet. Für ein bestimmtes Element definierten Stile werden immer zuletzt angewendet.

> [!TIP]
> Selektoren kombiniert werden können, ohne Einschränkung, z. B. `StackLayout>ContentView>label.email`.

Die folgenden Auswahlen werden derzeit nicht unterstützt:

- `[attribute]`
- `@media` und `@supports`
- `:` und `::`

> [!NOTE]
> Besonderheit und Besonderheit Außerkraftsetzungen werden nicht unterstützt.

## <a name="property-reference"></a>Referenz zu Servereigenschaften

Die folgenden CSS-Eigenschaften werden von Xamarin.Forms unterstützt (in der **Werte** Spalte Typen sind _Kursiv_, während Zeichenfolgenliterale sind `gray`):

|Eigenschaft|Betrifft|Werte|Beispiel|
|---|---|---|---|
|`background-color`|`VisualElement`|_Farbe_ \| `initial` |`background-color: springgreen;`|
|`background-image`|`Page`|_Zeichenfolge_ \| `initial` |`background-image: bg.png;`|
|`border-color`|`Button`, `Frame`|_Farbe_ \| `initial`|`border-color: #9acd32;`|
|`border-width`|`Button`|_Double_ \| `initial` |`border-width: .5;`|
|`color`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`|_Farbe_ \| `initial` |`color: rgba(255, 0, 0, 0.3);`|
|`direction`|`VisualElement`|`ltr` \| `rtl` \| `inherit` \| `initial` |`direction: rtl;`|
|`font-family`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_Zeichenfolge_ \| `initial` |`font-family: Consolas;`|
|`font-size`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_doppelte_ \| _Namedsize_  \| `initial` |`font-size: 12;`|
|`font-style`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|`bold` \| `italic` \| `initial` |`font-style: bold;`|
|`height`|`VisualElement`|_Double_ \| `initial` |`min-height: 250;`|
|`margin`|`View`|_Linienstärke_ \| `initial` |`margin: 6 12;`|
|`margin-left`|`View`|_Linienstärke_ \| `initial` |`margin-left: 3;`|
|`margin-top`|`View`|_Linienstärke_ \| `initial` |`margin-top: 2;`|
|`margin-right`|`View`|_Linienstärke_ \| `initial` |`margin-right: 1;`|
|`margin-bottom`|`View`|_Linienstärke_ \| `initial` |`margin-bottom: 6;`|
|`min-height`|`VisualElement`|_Double_ \| `initial` |`min-height: 50;`|
|`min-width`|`VisualElement`|_Double_ \| `initial` |`min-width: 112;`|
|`opacity`|`VisualElement`|_Double_ \| `initial` |`opacity: .3;`|
|`padding`|`Layout`, `Page`|_Linienstärke_ \| `initial` |`padding: 6 12 12;`|
|`padding-left`|`Layout`, `Page`|_Double_ \| `initial`|`padding-left: 3;`|
|`padding-top`|`Layout`, `Page`| _Double_ \| `initial` |`padding-top: 4;`|
|`padding-right`|`Layout`, `Page`| _Double_ \| `initial` |`padding-right: 2;`|
|`padding-bottom`|`Layout`, `Page`| _Double_ \| `initial` |`padding-bottom: 6;`|
|`text-align`| `Entry`, `EntryCell`, `Label`, `SearchBar`|`left` \| `right` \| `center` \| `start` \| `end` \| `initial`. `left` und `right` sollte in Umgebungen mit rechts-nach-links-vermieden werden.| `text-align: right;`|
|`visibility`|`VisualElement`|`true` \| `visible` \| `false` \| `hidden` \| `collapse` \| `initial `|`visibility: hidden;`|
|`width`|`VisualElement`|_Double_ \| `initial`|`min-width: 320;`|

> [!NOTE]
> `initial` ist ein gültiger Wert für alle Eigenschaften. Löscht den Wert (auf den Standardwert zurückgesetzt), der von einem anderen Stil festgelegt wurde.

Die folgenden Eigenschaften sind derzeit nicht unterstützt:

- `all: initial`
- Layout-Eigenschaften (Feld oder Rasterformat).
- Eigenschaften, z. B. `font`, und `border`.

Darüber hinaus besteht keine `inherit` Wert und daher Vererbung wird nicht unterstützt. Aus diesem Grund nicht möglich ist, z. B. Festlegen der `font-size` Eigenschaft in einem Layout und erwarten, dass alle der [ `Label` ](xref:Xamarin.Forms.Label) Instanzen im Layout auf den Wert zu erben. Die einzige Ausnahme ist die `direction` -Eigenschaft, die einen Standardwert besitzt der `inherit`.

### <a name="color"></a>Farbe

Die folgenden `color` Werte werden unterstützt:

- `X11` [Farben](https://en.wikipedia.org/wiki/X11_color_names/), die CSS-Farben, universelle Windows-Plattform vordefinierte Farben und Xamarin.Forms Farben übereinstimmen. Beachten Sie, dass diese Farbwerte Groß-/Kleinschreibung beachtet werden.
- Hex-Farben: `#rgb`, `#argb`, `#rrggbb`, `#aarrggbb`
- RGB-Farben: `rgb(255,0,0)`, `rgb(100%,0%,0%)`. Werte liegen im Bereich 0-255 oder 0-100 %.
- RGBA Farben: `rgba(255, 0, 0, 0.8)`, `rgba(100%, 0%, 0%, 0.8)`. Der Wert für die Deckkraft liegt im Bereich 0,0 bis 1,0.
- HSL Farben: `hsl(120, 100%, 50%)`. H-Wert wird im Bereich zwischen 0 und 360, s und l in den Bereich 0-100 % sind.
- HSLA Farben: `hsla(120, 100%, 50%, .8)`. Der Wert für die Deckkraft liegt im Bereich 0,0 bis 1,0.

### <a name="thickness"></a>Stärke

Einem, zwei, drei oder vier `thickness` Werte werden unterstützt, die jeweils durch ein Leerzeichen voneinander getrennt sind:

- Ein einzelner Wert gibt die einheitliche Stärke an.
- Zwei Werte deuten vertikale und horizontale Breite auf.
- Drei Werte geben Sie oben auf Horizontal (links und rechts) und dann unten Stärke an.
- Vier Werte geben Sie oben auf rechts auf unten und dann links Stärke an.

> [!NOTE]
> CSS `thickness` Werte unterscheiden sich von c#-XAML- [ `Thickness` ](/api/type/Xamarin.Forms.Thickness/) Werte. Z. B. in XAML eine zweiwertiger `Thickness` gibt die horizontale und vertikale Stärke, während eine vier-Wert an `Thickness` gibt an, nach links auf nach oben und dann rechts unten erfolgt Stärke. Darüber hinaus XAML `Thickness` Werte sind durch Kommas getrennt.

### <a name="namedsize"></a>NamedSize

Die folgenden Groß-/Kleinschreibung beachten `namedsize` Werte werden unterstützt:

- `default`
- `micro`
- `small`
- `medium`
- `large`

Die genaue Bedeutung der einzelnen `namedsize` Wert ist plattformabhängig und abhängige Ansicht.

## <a name="css-in-xamarinforms-with-xamarinuniversity"></a>CSS in Xamarin.Forms mit Xamarin.University

> [!VIDEO https://youtube.com/embed/va-Vb7vtan8]

**Xamarin.Forms 3.0 CSS, von [Xamarin University](https://university.xamarin.com/)**

## <a name="related-links"></a>Verwandte Links

- [MonkeyAppCSS (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/MonkeyAppCSS/)
- [Formatieren Sie Xamarin.Forms-Apps, die mit der Verwendung von XAML-Stile](~/xamarin-forms/user-interface/styles/xaml/index.md)
