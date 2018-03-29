---
title: Ressourcenverzeichnis
description: 'Verwendung von XAML-Ressourcen sind Definitionen der Objekte, die mehr als einmal verwendet werden können. Ein: "ResourceDictionary" kann Ressourcen in einem zentralen Ort definiert und wieder in der gesamten einer Xamarin.Forms-Anwendung verwendet werden. In diesem Artikel wird erläutert, wie erstellen und nutzen ein: "ResourceDictionary", und wie Ressourcenverzeichnis zusammengeführt.'
ms.topic: article
ms.prod: xamarin
ms.assetid: DF103686-4A92-40FA-9CF1-A9376293B13C
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/17/2017
ms.openlocfilehash: aa3ae9fed67b6cd7521e5c59edcb54f05cc6b7c5
ms.sourcegitcommit: 17a9cf246a4d33cfa232016992b308df540c8e4f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2018
---
# <a name="resource-dictionaries"></a>Ressourcenverzeichnis

_Verwendung von XAML-Ressourcen sind Definitionen der Objekte, die mehr als einmal verwendet werden können. Ein: "ResourceDictionary" kann Ressourcen in einem zentralen Ort definiert und wieder in der gesamten einer Xamarin.Forms-Anwendung verwendet werden. In diesem Artikel wird erläutert, wie erstellen und nutzen ein: "ResourceDictionary", und wie Ressourcenverzeichnis zusammengeführt._

## <a name="overview"></a>Übersicht

Ein [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) ist ein Repository für Ressourcen, die von einer Xamarin.Forms-Anwendung verwendet werden. Typische Ressourcen, die im rowsetcache eine `ResourceDictionary` enthalten [Stile](~/xamarin-forms/user-interface/styles/index.md), [Steuerelementvorlagen](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md), [Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md), Farben und Konverter.

In XAML werden Ressourcen in definiert eine [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) abgerufen und auf Elemente angewendet wird, mithilfe der `StaticResource` Markuperweiterung. In C#-Ressourcen definiert sind, einem `ResourceDictionary` abgerufen und auf Elemente mithilfe eines Indexers zeichenfolgenbasierte angewendet. Besteht jedoch kaum einen Vorteil mithilfe einer `ResourceDictionary` in c# als Ressourcen können einfach direkt zugewiesen werden Eigenschaften visueller Elemente ohne zuerst daraus Abrufen einer `ResourceDictionary`.

## <a name="creating-and-consuming-a-resourcedictionary"></a>Erstellen und nutzen ein: "ResourceDictionary"

Ressourcen können definiert werden, einer [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) angefügte der [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) Sammlung von einer Seite oder eines Steuerelements, oder die [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Resources/) Auflistung der Anwendung. Auswählen des Installationsorts für das Definieren einer [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) wirkt sich darauf aus, wo sie verwendet werden kann:

- Ressourcen in einem [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) definiert das Steuerelement Ebene kann nur angewendet werden das Steuerelement und seine untergeordneten Elemente.
- Ressourcen in einem [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) definiert auf der Seite Ebene kann nur angewendet werden auf der Seite und zu seinen untergeordneten Elementen.
- Ressourcen in einem [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) definierten Anwendungs-Ebene in der gesamten Anwendung angewendet werden kann.

Das folgende XAML-Code-Beispiel zeigt, Ressourcen, die in der Anwendungsebene definierten [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/):

```xaml
<Application ...>
    <Application.Resources>
        <ResourceDictionary>
            <Color x:Key="PageBackgroundColor">Yellow</Color>
            <Color x:Key="HeadingTextColor">Black</Color>
            <Color x:Key="NormalTextColor">Blue</Color>
            <Style x:Key="LabelPageHeadingStyle" TargetType="Label">
                <Setter Property="FontAttributes" Value="Bold" />
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="TextColor" Value="{StaticResource HeadingTextColor}" />
            </Style>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

Dies [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) definiert drei [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) Ressourcen und ein [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) Ressource. Weitere Informationen zum Erstellen einer XAML `App` Klasse, finden Sie unter [App-Klasse](~/xamarin-forms/app-fundamentals/application-class.md).

Jede Ressource weist einen Schlüssel, der dem angegebenen unter Verwendung der `x:Key` -Attribut, das sie einen beschreibenden Schlüssel, in bietet der `ResourceDictionary`. Der Schlüssel wird zum Abrufen einer Ressource aus der [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) durch die `StaticResource` Markuperweiterung, wie im folgenden Beispiel der Verwendung von XAML-Code veranschaulicht, die zusätzliche Ressourcen, die in einem Steuerelement definiert Ebene zeigt `ResourceDictionary`:

```xaml
<StackLayout Margin="0,20,0,0">
  <StackLayout.Resources>
    <ResourceDictionary>
      <Style x:Key="LabelNormalStyle" TargetType="Label">
        <Setter Property="TextColor" Value="{StaticResource NormalTextColor}" />
      </Style>
      <Style x:Key="MediumBoldText" TargetType="Button">
        <Setter Property="FontSize" Value="Medium" />
        <Setter Property="FontAttributes" Value="Bold" />
      </Style>
    </ResourceDictionary>
  </StackLayout.Resources>
  <Label Text="ResourceDictionary Demo" Style="{StaticResource LabelPageHeadingStyle}" />
    <Label Text="This app demonstrates consuming resources that have been defined in resource dictionaries."
           Margin="10,20,10,0"
           Style="{StaticResource LabelNormalStyle}" />
    <Button Text="Navigate"
            Clicked="OnNavigateButtonClicked"
            TextColor="{StaticResource NormalTextColor}"
            Margin="0,20,0,0"
            HorizontalOptions="Center"
            Style="{StaticResource MediumBoldText}" />
</StackLayout>
```

Die erste [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) Instanz abgerufen und verarbeitet die `LabelPageHeadingStyle` Ressource auf Anwendungsebene definierten [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), mit der zweiten `Label` Instanz Abrufen und Verarbeiten der `LabelNormalStyle` Ressource in der Steuerelementebene definierten `ResourceDictionary`. Auf ähnliche Weise die [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) Instanz abgerufen und verarbeitet die `NormalTextColor` Ressource auf Anwendungsebene definierten `ResourceDictionary`, und die `MediumBoldText` Ressource in der Steuerelementebene definierten `ResourceDictionary`. Daraus ergibt sich die Darstellung in den folgenden Screenshots dargestellt:

[![](resource-dictionaries-images/screenshots-sml.png ": "ResourceDictionary" Ressourcen verbrauchen")](resource-dictionaries-images/screenshots.png#lightbox ": "ResourceDictionary" Ressourcen zu verbrauchen.")

> [!NOTE]
> Ressourcen, die auf einer einzelnen Seite beziehen darf nicht in einer Anwendung Ebene Ressourcenwörterbuch, daher enthalten sein, die Ressourcen dann beim Anwendungsstart anstelle von analysiert werden, wenn eine Seite erforderlich. Weitere Informationen finden Sie unter [verringern die Größe der Anwendung Ressource Wörterbuch](~/xamarin-forms/deploy-test/performance.md).

## <a name="overriding-resources"></a>Ressourcen überschreiben

Wenn [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) Freigeben von Ressourcen `x:Key` Attributwerte, Ressourcen, die niedriger ist definiert in der Hierarchie anzeigen Vorrang vor den definierten höher einrichten. Z. B. die `PageBackgroundColor` Ressource `Blue` Ebene wird an die Anwendung von einer Seitenebene überschrieben werden `PageBackgroundColor` Ressource festgelegt werden, um `Yellow`. Auf ähnliche Weise eine Seitenebene `PageBackgroundColor` Ressource wird durch eine Steuerelementebene überschrieben werden `PageBackgroundColor` Ressource. Diese Priorität wird im folgenden Beispiel der Verwendung von XAML-Code veranschaulicht:

```xaml
<ContentPage ... BackgroundColor="{StaticResource PageBackgroundColor}">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Color x:Key="PageBackgroundColor">Blue</Color>
            <Color x:Key="NormalTextColor">Yellow</Color>
        </ResourceDictionary>
    </ContentPage.Resources>
    <StackLayout Margin="0,20,0,0">
        ...
        <Label Text="ResourceDictionary Demo" Style="{StaticResource LabelPageHeadingStyle}" />
        <Label Text="This app demonstrates consuming resources that have been defined in resource dictionaries."
               Margin="10,20,10,0"
               Style="{StaticResource LabelNormalStyle}" />
        <Button Text="Navigate"
                Clicked="OnNavigateButtonClicked"
                TextColor="{StaticResource NormalTextColor}"
                Margin="0,20,0,0"
                HorizontalOptions="Center"
                Style="{StaticResource MediumBoldText}" />
    </StackLayout>
</ContentPage>
```

Die ursprüngliche `PageBackgroundColor` und `NormalTextColor` Instanzen, die auf der Anwendungsebene definierten codierungsangaben durch die `PageBackgroundColor` und `NormalTextColor` Instanzen auf Seitenebene definiert. Aus diesem Grund die Hintergrundfarbe wird Blau, und der Text auf der Seite wird Gelb, wie in den folgenden Screenshots veranschaulicht:

[![](resource-dictionaries-images/overridding-screenshots-sml.png "Überschreiben: "ResourceDictionary" Ressourcen")](resource-dictionaries-images/overridding-screenshots.png#lightbox ": "ResourceDictionary" Ressourcen überschreiben")

Beachten Sie jedoch, dass der Hintergrund der Statusleiste des der [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) weiterhin Gelb ist, da die [ `BarBackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.BarBackgroundColor/) auf den Wert der Eigenschaft festgelegt ist die `PageBackgroundColor` Ressource in der Anwendung definiert Ebene [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/).

## <a name="merged-resource-dictionaries"></a>Zusammengeführte Ressourcenwörterbücher

Zusammengeführter Ressourcenwörterbücher kombinieren, eine oder mehrere [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) Instanzen in eine andere. Dies geschieht durch Festlegen der `ResourceDictionary.MergedDictionaries` Eigenschaft, um eine oder mehrere Ressourcenverzeichnis, die in der Anwendung, die Seite oder die Steuerungsebene zusammengeführt werden `ResourceDictionary`.

> [!IMPORTANT]
> Die [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) -Typ weist außerdem eine [ `MergedWith` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ResourceDictionary.MergedWith/) -Eigenschaft, die verwendet werden kann, um eine einzelne merge `ResourceDictionary` in eine andere, mit der `ResourceDictionary` als Wert für die angegeben`MergedWith`zusammengeführt, in der aktuellen Eigenschaft `ResourceDictionary` Instanz. Beim Zusammenführen der über die `MergedWith` -Eigenschaft, die Ressourcen in der aktuellen `ResourceDictionary` dieser Freigabe `x:Key` Attributwerte mit Ressourcen in der `ResourceDictionary` zusammengeführt werden, ersetzt werden. Allerdings die `MergedWith` Eigenschaft wird in einer zukünftigen Version von Xamarin.Forms veraltet sein. Aus diesem Grund wurde empfohlen, verwenden Sie die `MergedDictionaries` Eigenschaft zusammenführen `ResourceDictionary` Instanzen.

Das folgende Beispiel zeigt für die Verwendung von XAML-Code eine [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) mit dem Namen `MyResourceDictionary` , die eine einzelne Ressource enthält:

```xaml
<ResourceDictionary xmlns="http://xamarin.com/schemas/2014/forms"
                    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                    x:Class="ResourceDictionaryDemo.MyResourceDictionary">
    <DataTemplate x:Key="PersonDataTemplate">
        <ViewCell>
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="0.5*" />
                    <ColumnDefinition Width="0.2*" />
                    <ColumnDefinition Width="0.3*" />
                </Grid.ColumnDefinitions>
                <Label Text="{Binding Name}" TextColor="{StaticResource NormalTextColor}" FontAttributes="Bold" />
                <Label Grid.Column="1" Text="{Binding Age}" TextColor="{StaticResource NormalTextColor}" />
                <Label Grid.Column="2" Text="{Binding Location}" TextColor="{StaticResource NormalTextColor}" HorizontalTextAlignment="End" />
            </Grid>
        </ViewCell>
    </DataTemplate>
</ResourceDictionary>
```

`MyResourceDictionary` kann in jeder Anwendung, die Seite oder die Steuerungsebene zusammengeführt werden `ResourceDictionary`. Im folgenden Codebeispiel wird für XAML wird eine Seitenebene zusammengeführt wird `ResourceDictionary` mithilfe der `MergedDictionaries` Eigenschaft:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <local:MyResourceDictionary />
                <!-- Add more resource dictionaries here -->
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Beim Zusammenführen der [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) Freigeben von Ressourcen identisch `x:Key` Attributwerte Xamarin.Forms verwendet die folgenden Ressourcen Vorrang vor:

1. Die Ressourcen, die lokal auf das Ressourcenwörterbuch.
1. Die Ressourcen in das Ressourcenwörterbuch, die über zusammengeführt wurde die [ `MergedWith` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ResourceDictionary.MergedWith/) Eigenschaft.
1. Die Ressourcen in die Ressourcenwörterbücher, die über zusammengeführt wurden die `MergedDictionaries` Auflistung, in der sie, in aufgelistet sind Reihenfolge der `MergedDictionaries` Eigenschaft.

> [!NOTE]
> Ressourcenverzeichnis suchen kann einen rechenintensiven Vorgang sein, wenn eine Anwendung mehrere enthält große Ressourcenwörterbücher. Daher stellen Sie sicher, dass jede Seite in einer Anwendung nur Ressourcenverzeichnis, die auf der Seite verwendet, um zu vermeiden unnötige Suchläufe geeignet sind.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird erläutert, wie erstellen und nutzen einen [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), und wie Ressourcenverzeichnis zusammengeführt. Ein `ResourceDictionary` können Ressourcen in einem zentralen Ort definiert und wieder in der gesamten einer Xamarin.Forms-Anwendung verwendet werden.


## <a name="related-links"></a>Verwandte Links

- [Ressourcenverzeichnis (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/xaml/resourcedictionaries/)
- [Stile](~/xamarin-forms/user-interface/styles/index.md)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
