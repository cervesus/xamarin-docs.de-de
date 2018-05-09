---
title: Dynamische Formate
description: Stile nicht reagieren auf eigenschaftenänderungen, und für die Dauer einer Anwendung unverändert bleiben. Beispielsweise wird nicht nach dem Zuweisen einer Formatvorlage auf ein visuelles Element, wenn eine der Setter Instanzen entfernt, geändert wird, oder eine neue Setter-Instanz, die hinzugefügt, die Änderungen auf das visuelle Element angewendet werden. Allerdings können Anwendungen auf Änderungen an Formatvorlagen zur Laufzeit dynamisch zu reagieren, dynamische Ressourcen nutzen.
ms.prod: xamarin
ms.assetid: 13D4FA4B-DF10-42BF-B001-2C49367FC216
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: c484bdc90ec039a8d70209deabbe283cf7100610
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="dynamic-styles"></a>Dynamische Formate

_Stile nicht reagieren auf eigenschaftenänderungen, und für die Dauer einer Anwendung unverändert bleiben. Beispielsweise wird nicht nach dem Zuweisen einer Formatvorlage auf ein visuelles Element, wenn eine der Setter Instanzen entfernt, geändert wird, oder eine neue Setter-Instanz, die hinzugefügt, die Änderungen auf das visuelle Element angewendet werden. Allerdings können Anwendungen auf Änderungen an Formatvorlagen zur Laufzeit dynamisch zu reagieren, dynamische Ressourcen nutzen._

Die `DynamicResource` Markuperweiterung ähnelt der `StaticResource` Markuperweiterung, die beide eine Wörterbuchschlüssel verwenden, um einen Wert aus abzurufen eine [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/). Allerdings dagegen die `StaticResource` führt eine Suche einzelne Wörterbuch der `DynamicResource` verwaltet einen Link zu der Wörterbuchschlüssel. Aus diesem Grund wird die Wörterbucheintrag mit dem Schlüssel zugeordnete ersetzt wird, die Änderung auf das visuelle Element übernommen. Dadurch können Änderungen zur Laufzeit Stil in einer Anwendung vorgenommen werden.

Im folgenden Codebeispiel wird veranschaulicht, *dynamische* Formatvorlagen auf eine XAML-Seite:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.DynamicStylesPage" Title="Dynamic" Icon="xaml.png">
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

Die [ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) Instanzen verwenden die `DynamicResource` Markuperweiterung zum Verweisen auf eine [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) mit dem Namen `searchBarStyle`, die nicht in XAML definiert wird. Jedoch da die [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) Eigenschaften der `SearchBar` Instanzen festgelegt werden, werden eine `DynamicResource`, fehlende Wörterbuchschlüssel nicht dazu führen, eine Ausnahme ausgelöst.

Stattdessen in den Code-Behind-Datei der Konstruktor erstellt ein [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) Eintrag mit dem Schlüssel `searchBarStyle`, wie im folgenden Codebeispiel gezeigt:

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

Wenn die `OnButtonClicked` -Ereignishandler ausgeführt wird, `searchBarStyle` wechselt zwischen `blueSearchBarStyle` und `greenSearchBarStyle`. Daraus ergibt sich die Darstellung in den folgenden Screenshots dargestellt:

[![](dynamic-images/dynamic-style-blue.png "Dynamic Style Beispiel Blau")](dynamic-images/dynamic-style-blue-large.png#lightbox "Blau Dynamic Style Beispiel")
[![](dynamic-images/dynamic-style-green.png "Grün Dynamic Style Beispiel") ] (dynamic-images/dynamic-style-green-large.png#lightbox "Grün Dynamic Style-Beispiel")

Im folgenden Codebeispiel wird veranschaulicht, die entsprechende Seite in c#:

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
            Children = { searchBar1, searchBar2, searchBar3, searchBar4,    button  }
        };
    }
    ...
}
```

In c# ist die [ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) Instanzen verwenden den [ `SetDynamicResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Element.SetDynamicResource/) aufzurufende Methode verweisen `searchBarStyle`. Die `OnButtonClicked` Ereignishandlercode ist identisch mit dem XAML-Beispiel, und bei der Ausführung `searchBarStyle` wechselt zwischen `blueSearchBarStyle` und `greenSearchBarStyle`.

<a name="dynamic-style-inheritance">

## <a name="dynamic-style-inheritance"></a>Dynamic Style-Vererbung

Ableiten einer Formatvorlage aus einem dynamischen Stil nicht erreicht werden, mithilfe der [ `Style.BasedOn` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BasedOn/) Eigenschaft. Stattdessen die [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) Klasse enthält die [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) -Eigenschaft, die auf einem Wörterbuchschlüssel, dessen Wert festgelegt werden kann, kann dynamisch ändern.

Im folgenden Codebeispiel wird veranschaulicht, *dynamische* formatieren Vererbung in einer XAML-Seite:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.DynamicStylesInheritancePage" Title="Dynamic Inheritance" Icon="xaml.png">
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

Die [ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) Instanzen verwenden den `StaticResource` Markuperweiterung zum Verweisen auf eine [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) mit dem Namen `tealSearchBarStyle`. Dies `Style` einige zusätzlichen Eigenschaften festgelegt und verwendet die [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) -Eigenschaft zum Verweisen auf `searchBarStyle`. Die `DynamicResource` Markuperweiterung ist nicht erforderlich, da `tealSearchBarStyle` wird nicht geändert werden, mit Ausnahme der `Style` abgeleitet ist. Aus diesem Grund `tealSearchBarStyle` verwaltet einen Link zur `searchBarStyle` und wird geändert, wenn das Format des Basis ändert.

In der Code-Behind-Datei der Konstruktor erstellt ein [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) Eintrag mit dem Schlüssel `searchBarStyle`gemäß dem vorherigen Beispiel, das dynamische Formate veranschaulicht. Wenn die `OnButtonClicked` -Ereignishandler ausgeführt wird, `searchBarStyle` wechselt zwischen `blueSearchBarStyle` und `greenSearchBarStyle`. Daraus ergibt sich die Darstellung in den folgenden Screenshots dargestellt:

[![](dynamic-images/dynamic-style-inheritance-blue.png "Blau Dynamic Style-Vererbungsbeispiel")](dynamic-images/dynamic-style-inheritance-blue-large.png#lightbox "Blau Dynamic Style-Vererbungsbeispiel")
[![](dynamic-images/dynamic-style-inheritance-green.png "Dynamic Style Grün -Vererbungsbeispiel")](dynamic-images/dynamic-style-inheritance-green-large.png#lightbox "Grün Dynamic Style Vererbung-Beispiel")

Im folgenden Codebeispiel wird veranschaulicht, die entsprechende Seite in c#:

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

Die `tealSearchBarStyle` direkt zugewiesen ist die [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) Eigenschaft von der [ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) Instanzen. Dies `Style` einige zusätzlichen Eigenschaften festgelegt, und verwendet die [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) -Eigenschaft zum Verweisen auf `searchBarStyle`. Die [ `SetDynamicResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Element.SetDynamicResource/) Methode ist hier erforderlich, da `tealSearchBarStyle` wird nicht geändert werden, mit Ausnahme der `Style` abgeleitet ist. Aus diesem Grund `tealSearchBarStyle` verwaltet einen Link zur `searchBarStyle` und wird geändert, wenn das Format des Basis ändert.

## <a name="summary"></a>Zusammenfassung

Stile nicht reagieren auf eigenschaftenänderungen, und für die Dauer einer Anwendung unverändert bleiben. Allerdings können Anwendungen auf Änderungen an Formatvorlagen zur Laufzeit dynamisch zu reagieren, dynamische Ressourcen nutzen. Darüber hinaus *dynamische* Formate können von abgeleitet werden, mit der [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) Eigenschaft.



## <a name="related-links"></a>Verwandte Links

- [XAML-Markuperweiterungen](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Dynamische Formate (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/DynamicStyles/)
- [Arbeiten mit Formatvorlagen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [Stil](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Setter-Methode](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
