---
title: Dynamische Stile inXamarin.Forms
description: In diesem Artikel wird erläutert, wie eine-Anwendung auf Format Xamarin.Forms Änderungen dynamisch zur Laufzeit mithilfe dynamischer Ressourcen reagieren kann.
ms.prod: xamarin
ms.assetid: 13D4FA4B-DF10-42BF-B001-2C49367FC216
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/28/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.custom: video
ms.openlocfilehash: d40ca3423cca68757cf458faf5cca1138aec5461
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84140087"
---
# <a name="dynamic-styles-in-xamarinforms"></a>Dynamische Stile inXamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-dynamicstyles)

_Stile reagieren nicht auf Eigenschafts Änderungen und bleiben für die Dauer einer Anwendung unverändert. Wenn Sie z. b. einem visuellen Element einen Stil zuweisen und eine der Setter-Instanzen geändert, entfernt oder eine neue Setter-Instanz hinzugefügt wurde, werden die Änderungen nicht auf das visuelle Element angewendet. Anwendungen können jedoch zur Laufzeit dynamisch auf Formatänderungen reagieren, indem Sie dynamische Ressourcen verwenden._

Die `DynamicResource` Markup Erweiterung ähnelt der `StaticResource` Markup Erweiterung darin, dass beide eine Wörterbuch Taste zum Abrufen eines Werts aus einem verwenden [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) . Wenn jedoch `StaticResource` eine einzelne Wörterbuchsuche ausführt, `DynamicResource` behält einen Link zum Wörterbuch Schlüssel bei. Wenn der Wörterbucheintrag, der dem Schlüssel zugeordnet ist, ersetzt wird, wird die Änderung daher auf das visuelle Element angewendet. Auf diese Weise können Änderungen im Lauf Zeitformat in einer Anwendung vorgenommen werden.

Im folgenden Codebeispiel werden *dynamische* Stile in einer XAML-Seite veranschaulicht:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.DynamicStylesPage" Title="Dynamic" IconImageSource="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
              ...
            </Style>
            <Style x:Key="blueSearchBarStyle"
                   TargetType="SearchBar"
                   BasedOn="{StaticResource baseStyle}">
              ...
            </Style>
            <Style x:Key="greenSearchBarStyle"
                   TargetType="SearchBar">
              ...
            </Style>
            ...
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <SearchBar Placeholder="These SearchBar controls"
                       Style="{DynamicResource searchBarStyle}" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Die- [`SearchBar`](xref:Xamarin.Forms.SearchBar) Instanzen verwenden die `DynamicResource` Markup Erweiterung, um auf einen mit dem Namen zu verweisen [`Style`](xref:Xamarin.Forms.Style) `searchBarStyle` , der nicht in XAML definiert ist. Da jedoch die [`Style`](xref:Xamarin.Forms.NavigableElement.Style) Eigenschaften der- `SearchBar` Instanzen mit einem festgelegt werden `DynamicResource` , führt der fehlende Wörterbuch Schlüssel nicht dazu, dass eine Ausnahme ausgelöst wird.

Stattdessen erstellt der Konstruktor in der Code Behind-Datei einen [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) Eintrag mit dem Schlüssel `searchBarStyle` , wie im folgenden Codebeispiel gezeigt:

```csharp
public partial class DynamicStylesPage : ContentPage
{
    bool originalStyle = true;

    public DynamicStylesPage ()
    {
        InitializeComponent ();
        Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];
    }

    void OnButtonClicked (object sender, EventArgs e)
    {
        if (originalStyle) {
            Resources ["searchBarStyle"] = Resources ["greenSearchBarStyle"];
            originalStyle = false;
        } else {
            Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];
            originalStyle = true;
        }
    }
}
```

Wenn der `OnButtonClicked` Ereignishandler ausgeführt wird, `searchBarStyle` wechselt zwischen `blueSearchBarStyle` und `greenSearchBarStyle` . Dies ergibt die in den folgenden Screenshots gezeigte Darstellung:

Beispiel für ein [ ![ blaues dynamisches Stil Beispiel](dynamic-images/dynamic-style-blue.png)](dynamic-images/dynamic-style-blue-large.png#lightbox)für ein 
 [ ![ grünes Beispiel](dynamic-images/dynamic-style-green.png)](dynamic-images/dynamic-style-green-large.png#lightbox)

Im folgenden Codebeispiel wird die entsprechende Seite in c# veranschaulicht:

```csharp
public class DynamicStylesPageCS : ContentPage
{
    bool originalStyle = true;

    public DynamicStylesPageCS ()
    {
        ...
        var baseStyle = new Style (typeof(View)) {
            ...
        };
        var blueSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        var greenSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        ...
        var searchBar1 = new SearchBar { Placeholder = "These SearchBar controls" };
        searchBar1.SetDynamicResource (VisualElement.StyleProperty, "searchBarStyle");
        ...
        Resources = new ResourceDictionary ();
        Resources.Add ("blueSearchBarStyle", blueSearchBarStyle);
        Resources.Add ("greenSearchBarStyle", greenSearchBarStyle);
        Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];

        Content = new StackLayout {
            Children = { searchBar1, searchBar2, searchBar3, searchBar4,    button    }
        };
    }
    ...
}
```

In c# verwenden die- [`SearchBar`](xref:Xamarin.Forms.SearchBar) Instanzen die- [`SetDynamicResource`](xref:Xamarin.Forms.Element.SetDynamicResource*) Methode, um auf zu verweisen `searchBarStyle` . Der `OnButtonClicked` Ereignishandlercode ist mit dem XAML-Beispiel identisch und wechselt bei der Ausführung `searchBarStyle` zwischen `blueSearchBarStyle` und `greenSearchBarStyle` .

## <a name="dynamic-style-inheritance"></a>Vererbung dynamischer Stile

Das Ableiten eines Stils von einem dynamischen Stil kann nicht mithilfe der-Eigenschaft erreicht werden [`Style.BasedOn`](xref:Xamarin.Forms.Style.BasedOn) . Stattdessen enthält die- [`Style`](xref:Xamarin.Forms.Style) Klasse die- [`BaseResourceKey`](xref:Xamarin.Forms.Style.BaseResourceKey) Eigenschaft, die auf einen Wörterbuch Schlüssel festgelegt werden kann, dessen Wert dynamisch geändert werden kann.

Das folgende Codebeispiel veranschaulicht die *dynamische* Stil Vererbung in einer XAML-Seite:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.DynamicStylesInheritancePage" Title="Dynamic Inheritance" IconImageSource="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
              ...
            </Style>
            <Style x:Key="blueSearchBarStyle" TargetType="SearchBar" BasedOn="{StaticResource baseStyle}">
              ...
            </Style>
            <Style x:Key="greenSearchBarStyle" TargetType="SearchBar">
              ...
            </Style>
            <Style x:Key="tealSearchBarStyle" TargetType="SearchBar" BaseResourceKey="searchBarStyle">
              ...
            </Style>
            ...
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <SearchBar Text="These SearchBar controls" Style="{StaticResource tealSearchBarStyle}" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Die- [`SearchBar`](xref:Xamarin.Forms.SearchBar) Instanzen verwenden die `StaticResource` Markup Erweiterung, um auf eine benannte zu verweisen [`Style`](xref:Xamarin.Forms.Style) `tealSearchBarStyle` . Dadurch werden `Style` einige zusätzliche Eigenschaften festgelegt, und die- [`BaseResourceKey`](xref:Xamarin.Forms.Style.BaseResourceKey) Eigenschaft wird für den Verweis verwendet `searchBarStyle` . Die `DynamicResource` Markup Erweiterung ist nicht erforderlich, da sich `tealSearchBarStyle` nicht ändert, es sei denn, `Style` Sie wird von abgeleitet. Daher `tealSearchBarStyle` verwaltet einen Link zu `searchBarStyle` und wird geändert, wenn sich der Basisstil ändert.

In der Code-Behind-Datei erstellt der-Konstruktor einen [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) Eintrag mit dem Schlüssel gemäß `searchBarStyle` dem vorherigen Beispiel, in dem dynamische Stile demonstriert wurden. Wenn der `OnButtonClicked` Ereignishandler ausgeführt wird, `searchBarStyle` wechselt zwischen `blueSearchBarStyle` und `greenSearchBarStyle` . Dies ergibt die in den folgenden Screenshots gezeigte Darstellung:

[ ![ Blaue dynamische Stil Vererbung Beispiel](dynamic-images/dynamic-style-inheritance-blue.png)](dynamic-images/dynamic-style-inheritance-blue-large.png#lightbox)Beispiel für 
 [ ![ grünes dynamisches Format Vererbung](dynamic-images/dynamic-style-inheritance-green.png)](dynamic-images/dynamic-style-inheritance-green-large.png#lightbox)

Im folgenden Codebeispiel wird die entsprechende Seite in c# veranschaulicht:

```csharp
public class DynamicStylesInheritancePageCS : ContentPage
{
    bool originalStyle = true;

    public DynamicStylesInheritancePageCS ()
    {
        ...
        var baseStyle = new Style (typeof(View)) {
            ...
        };
        var blueSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        var greenSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        var tealSearchBarStyle = new Style (typeof(SearchBar)) {
            BaseResourceKey = "searchBarStyle",
            ...
        };
        ...
        Resources = new ResourceDictionary ();
        Resources.Add ("blueSearchBarStyle", blueSearchBarStyle);
        Resources.Add ("greenSearchBarStyle", greenSearchBarStyle);
        Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];

        Content = new StackLayout {
            Children = {
                new SearchBar { Text = "These SearchBar controls", Style = tealSearchBarStyle },
                ...
            }
        };
    }
    ...
}
```

Der `tealSearchBarStyle` wird der- [`Style`](xref:Xamarin.Forms.NavigableElement.Style) Eigenschaft der-Instanzen direkt zugewiesen [`SearchBar`](xref:Xamarin.Forms.SearchBar) . Dadurch `Style` werden einige zusätzliche Eigenschaften festgelegt und die-Eigenschaft verwendet, [`BaseResourceKey`](xref:Xamarin.Forms.Style.BaseResourceKey) um auf zu verweisen `searchBarStyle` . Die- [`SetDynamicResource`](xref:Xamarin.Forms.Element.SetDynamicResource*) Methode ist hier nicht erforderlich `tealSearchBarStyle` , da sich nicht ändert, es sei denn, Sie wird `Style` von abgeleitet. Daher `tealSearchBarStyle` verwaltet einen Link zu `searchBarStyle` und wird geändert, wenn sich der Basisstil ändert.

## <a name="related-links"></a>Verwandte Links

- [XAML-Markuperweiterungen](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Dynamische Stile (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-dynamicstyles)
- [Arbeiten mit Stilen (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [style](xref:Xamarin.Forms.Style)
- [Trend](xref:Xamarin.Forms.Setter)

## <a name="related-video"></a>Zugehörige Videos

> [!Video https://channel9.msdn.com/Shows/XamarinShow/XamarinForms-101-Dynamic-Resources/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
