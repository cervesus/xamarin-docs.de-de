---
title: Ressourcenverzeichnisse
description: XAML-Ressourcen sind Objekte, die freigegeben und erneut in einer Xamarin.Forms-Anwendung verwendet werden können.
ms.prod: xamarin
ms.assetid: DF103686-4A92-40FA-9CF1-A9376293B13C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/15/2019
ms.custom: video
ms.openlocfilehash: a9b9b2d12193161e0cb4514600381c3a7a38495a
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69529319"
---
# <a name="resource-dictionaries"></a>Ressourcenverzeichnisse

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-resourcedictionaries)

_XAML-Ressourcen sind Definitionen von Objekten, die in einer xamarin. Forms-Anwendung freigegeben und wieder verwendet werden können. Diese Ressourcen Objekte werden in einem Ressourcen Wörterbuch gespeichert._

Ein [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) ist ein Repository für Ressourcen, die von einer Xamarin.Forms-Anwendung verwendet werden. Typische Ressourcen, die in gespeichert werden eine `ResourceDictionary` enthalten [Stile](~/xamarin-forms/user-interface/styles/index.md), [Steuerelementvorlagen](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md), [Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md), Farben und Typkonverter.

In XAML, Ressourcen, die im rowsetcache eine `ResourceDictionary` abgerufen und mit Elementen angewendet werden können die `StaticResource` Markuperweiterung. In c# können Ressourcen auch definiert werden eine `ResourceDictionary` abgerufen und mithilfe eines Indexers zeichenfolgenbasierten auf Elemente angewendet. Es ist jedoch wenig Vorteil der Verwendung einer `ResourceDictionary` in c# wie freigegebene Objekte einfach als Felder oder Eigenschaften gespeichert, und aufgerufen werden können direkt ohne zuerst müssen sie abrufen aus einem Wörterbuch.

## <a name="create-and-consume-a-resourcedictionary"></a>Erstellen und Verwenden eines ResourceDictionary

Ressourcen werden definiert, einem [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) , legen Sie dann auf einen der folgenden `Resources` Eigenschaften:

- Die [ `Resources` ](xref:Xamarin.Forms.Application.Resources) Eigenschaft jeder Klasse, die von abgeleitet ist. [`Application`](xref:Xamarin.Forms.Application)
- Die [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) Eigenschaft jeder Klasse, die von abgeleitet ist. [`VisualElement`](xref:Xamarin.Forms.Application)

Eine Xamarin.Forms-Anwendung enthält nur eine abgeleitete Klasse `Application` aber häufig viele abgeleitete Klassen `VisualElement`, einschließlich Seiten, Layouts und Steuerelemente. Eines dieser Objekte haben die `Resources` -Eigenschaftensatz auf eine `ResourceDictionary`. Auswählen eines Speicherorts für eine bestimmte `ResourceDictionary` Auswirkungen, wo die Ressourcen verwendet werden können:

- Ressourcen in einem `ResourceDictionary` , z. B. eine Ansicht angefügt ist `Button` oder `Label` können nur auf dieses bestimmte Objekt angewendet werden, damit dies nicht sehr nützlich ist.
- Ressourcen in einem `ResourceDictionary` zu einem Layout angefügt sind, z. B. `StackLayout` oder `Grid` auf das Layout und alle untergeordneten Elemente dieses Layout angewendet werden können.
- Ressourcen in einem `ResourceDictionary` definiert auf der Seite auf der Seite und alle untergeordneten angewendet werden kann.
- Ressourcen in einem `ResourceDictionary` definiert die Anwendung kann auf in der gesamten Anwendung angewendet werden.

Der folgende XAML zeigt die Ressourcen, die in der Anwendungsebene definiert `ResourceDictionary` in die **"App.xaml"** Datei, die als Teil der standardmäßigen Xamarin.Forms-Anwendung erstellt:

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

Dies `ResourceDictionary` definiert drei [ `Color` ](xref:Xamarin.Forms.Color) Ressourcen und ein [ `Style` ](xref:Xamarin.Forms.Style) Ressource. Weitere Informationen zu den `App` Klasse, finden Sie unter [Anwendungsklasse](~/xamarin-forms/app-fundamentals/application-class.md).

Ab den expliziten Xamarin.Forms 3.0 `ResourceDictionary` Tags sind nicht erforderlich. Die `ResourceDictionary` Objekt wird automatisch erstellt, und Sie können die Ressourcen direkt zwischen Einfügen der `Resources` Eigenschaftenelement Tags:

```xaml
<Application ...>
    <Application.Resources>
        <Color x:Key="PageBackgroundColor">Yellow</Color>
        <Color x:Key="HeadingTextColor">Black</Color>
        <Color x:Key="NormalTextColor">Blue</Color>
        <Style x:Key="LabelPageHeadingStyle" TargetType="Label">
            <Setter Property="FontAttributes" Value="Bold" />
            <Setter Property="HorizontalOptions" Value="Center" />
            <Setter Property="TextColor" Value="{StaticResource HeadingTextColor}" />
        </Style>
    </Application.Resources>
</Application>
```

Jede Ressource verfügt über einen Schlüssel, der angegebenen unter Verwendung der `x:Key` -Attribut, das es Wörterbuchschlüssel in wird die `ResourceDictionary`. Der Schlüssel dient zum Abrufen einer Ressource aus der `ResourceDictionary` durch die [ `StaticResource` ](xref:Xamarin.Forms.Xaml.StaticResourceExtension) Markuperweiterung, wie in den folgenden XAML-Codebeispiel wird veranschaulicht, die zusätzliche Ressourcen, die in definierten zeigt die `StackLayout`:

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

Die erste [ `Label` ](xref:Xamarin.Forms.Label) Instanz abruft und verarbeitet die `LabelPageHeadingStyle` Ressource auf Anwendungsebene definiert `ResourceDictionary`, mit dem zweiten `Label` Instanz abrufen und Verarbeiten der `LabelNormalStyle`in die Steuerungsebene definierte Ressource `ResourceDictionary`. Auf ähnliche Weise die [ `Button` ](xref:Xamarin.Forms.Button) Instanz abruft und verarbeitet die `NormalTextColor` Ressource auf Anwendungsebene definiert `ResourceDictionary`, und die `MediumBoldText` in die Steuerungsebene definierte Ressource `ResourceDictionary`. Dadurch wird die Darstellung, die in den folgenden Screenshots gezeigt:

[![](resource-dictionaries-images/screenshots-sml.png "Ressourcenverbrauch ResourceDictionary")](resource-dictionaries-images/screenshots.png#lightbox "ResourceDictionary-Ressourcen zu verbrauchen.")

> [!NOTE]
> Ressourcen, die auf eine einzelne Seite spezifisch sind, darf nicht in einer Anwendung auf Ressourcenverzeichnis, daher enthalten sein, die Ressourcen dann beim Start der Anwendung nicht analysiert werden soll, bei einer Seite erforderlich. Weitere Informationen finden Sie unter [Reduzieren der Größe des Ressourcenverzeichnisses der Anwendung](~/xamarin-forms/deploy-test/performance.md).

## <a name="override-resources"></a>Ressourcen überschreiben

Wenn `ResourceDictionary` Ressourcen freigeben `x:Key` Attributwerte niedriger in der Hierarchie von Inhaltsansichten definierte Ressourcen Vorrang vor den definierten höher einrichten. Beispiel: Wenn die `PageBackgroundColor` Ressource `Blue` Ebene wird in der Anwendung von einer Seitenebene überschrieben werden `PageBackgroundColor` Ressourcensatz, zu `Yellow`. Auf ähnliche Weise eine Seitenebene `PageBackgroundColor` Ressource wird von der Steuerungsebene überschrieben `PageBackgroundColor` Ressource. Diese Priorität wird im folgenden XAML-Codebeispiel veranschaulicht:

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

Die ursprüngliche `PageBackgroundColor` und `NormalTextColor` Instanzen, die auf der Anwendungsebene definiert werden überschrieben, indem die `PageBackgroundColor` und `NormalTextColor` Instanzen auf Seitenebene definiert. Aus diesem Grund die Hintergrundfarbe ist Blau, und der Text auf der Seite wird Gelb, wie in den folgenden Screenshots veranschaulicht:

[![](resource-dictionaries-images/overridding-screenshots-sml.png "Überschreiben ResourceDictionary Ressourcen")](resource-dictionaries-images/overridding-screenshots.png#lightbox "ResourceDictionary Ressourcen überschreiben")

Beachten Sie jedoch, dass der Hintergrund-Leiste des der [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) weiterhin Gelb ist, da die [ `BarBackgroundColor` ](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor) -Eigenschaftensatz auf den Wert des der `PageBackgroundColor` Ressource, die in der Anwendung definiert Ebene `ResourceDictionary`.

Dies ist eine weitere Möglichkeit, die `ResourceDictionary` Rangfolge zu betrachten: Wenn der XAML-Parser auf `StaticResource`einen stößt, sucht er nach einem übereinstimmenden Schlüssel, indem er die visuelle Struktur durchsucht, wobei die erste gefundene Übereinstimmung verwendet wird. Wenn es sich bei dieser Suche wird beendet, auf der Seite, und der Schlüssel können weiterhin wurde nicht gefunden wurde, sucht der XAML-Parser die `ResourceDictionary` angefügt, um die `App` Objekt. Wenn der Schlüssel nicht gefunden wird, wird eine Ausnahme ausgelöst.

## <a name="stand-alone-resource-dictionaries"></a>Eigenständige Ressourcen Wörterbücher

Eine abgeleitete Klasse `ResourceDictionary` kann auch in einer separaten eigenständigen Datei sein. (Genauer gesagt, eine Klasse, die von abgeleiteten `ResourceDictionary` erfordert normalerweise, dass eine _Paar_ von Dateien, da die Ressourcen in einer XAML-Datei, aber mit einer CodeBehind-Datei definiert sind ein `InitializeComponent` Aufruf ist ebenfalls erforderlich.) Die resultierende Datei kann für Anwendungen genutzt werden.

Um eine solche Datei zu erstellen, fügen Sie ein neues **Ansicht "Inhalt"** oder **Inhaltsseite** Elements zum Projekt (aber keine **Ansicht "Inhalt"** oder **Inhaltsseite** mit nur eine C#-Datei). Ändern Sie in der XAML-Datei und c#-Datei, den Namen der Basisklasse aus `ContentView` oder `ContentPage` zu `ResourceDictionary`. In der XAML-Datei ist der Name der Basisklasse Element der obersten Ebene.

Das folgende XAML-Beispiel zeigt eine [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) mit dem Namen `MyResourceDictionary`:

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

Dies `ResourceDictionary` enthält eine einzelne Ressource, die ein Objekt des Typs ist `DataTemplate`.

Instanziieren Sie `MyResourceDictionary` Wenn man diese Anwendungen zwischen einem Paar von `Resources` Eigenschaftenelement tags, z. B. einem `ContentPage`:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <local:MyResourceDictionary />
    </ContentPage.Resources>
    ...
</ContentPage>
```

Eine Instanz von `MyResourceDictionary` nastaven NA hodnotu der `Resources` Eigenschaft der `ContentPage` Objekt.

Bei diesem Ansatz gelten jedoch einige Einschränkungen: Die `Resources` -Eigenschaft `ContentPage` des-Objekts verweist nur `ResourceDictionary`auf dieses. In den meisten Fällen soll die Möglichkeit, einschließlich anderer `ResourceDictionary` Instanzen und möglicherweise andere Ressourcen als auch.

Diese Aufgabe erfordert zusammengeführte Ressourcenwörterbücher.

## <a name="merged-resource-dictionaries"></a>Zusammengeführte Ressourcen Verzeichnisse

Zusammengeführte Ressourcen Wörterbücher Kombi [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) Nieren ein oder `ResourceDictionary`mehrere Objekte zu einem anderen.

### <a name="merge-local-resource-dictionaries"></a>Lokale Ressourcen Wörterbücher zusammenführen

Eine lokale [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) kann in eine andere `ResourceDictionary` zusammengeführt werden, [`Source`](xref:Xamarin.Forms.ResourceDictionary.Source) indem die-Eigenschaft auf den Dateinamen der XAML-Datei mit den Ressourcen festgelegt wird:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <!-- Add more resources here -->
        <ResourceDictionary Source="MyResourceDictionary.xaml" />
        <!-- Add more resources here -->
    </ContentPage.Resources>
    ...
</ContentPage>
```

Diese Syntax instanziiert die `MyResourceDictionary` -Klasse nicht. Stattdessen verweist es auf die XAML-Datei. Aus diesem Grund ist beim Festlegen der [`Source`](xref:Xamarin.Forms.ResourceDictionary.Source) -Eigenschaft die Code Behind-Datei (**MyResourceDictionary.XAML.cs**) nicht erforderlich, und das `x:Class` -Attribut kann aus dem Stammtag der **myresourcedictionary. XAML** -Datei entfernt werden. Beim Zusammenführen von Ressourcen Wörterbüchern mithilfe dieses Ansatzes instanziiert xamarin. Forms das [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)automatisch, sodass die äußeren `ResourceDictionary` Tags nicht benötigt werden.

> [!IMPORTANT]
> Die [`Source`](xref:Xamarin.Forms.ResourceDictionary.Source) -Eigenschaft kann nur aus XAML festgelegt werden.

### <a name="merge-resource-dictionaries-from-other-assemblies"></a>Ressourcen Wörterbücher aus anderen Assemblys

Ein [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) kann auch `ResourceDictionary` miteinem`ResourceDictionary`anderen zusammengeführt werden, indem es der- [Eigenschaftdeshinzugefügtwird.`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) Diese Technik ermöglicht das Zusammenführen von Ressourcen Wörterbüchern, unabhängig von der Assembly, in der Sie sich befinden.

> [!IMPORTANT]
> Die [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) -Klasse definiert auch [`MergedWith`](xref:Xamarin.Forms.ResourceDictionary.MergedWith) eine-Eigenschaft. Diese Eigenschaft ist jedoch veraltet und sollte nicht mehr verwendet werden.

Das folgende Codebeispiel zeigt `MyResourceDictionary` , wie der [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) Auflistung einer Seitenebene [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)hinzugefügt wird:

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:ResourceDictionaryDemo">
    <ContentPage.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <local:MyResourceDictionary />
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Dieses Beispiel zeigt eine Instanz von `MyResourceDictionary`, die sich in derselben Assembly befindet und dem [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)hinzugefügt wird. Außerdem können Sie Ressourcen Wörterbücher aus anderen Assemblys, anderen `ResourceDictionary` Objekten innerhalb der [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) Eigenschaften-Element Tags und anderen Ressourcen außerhalb der folgenden Tags hinzufügen:

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:ResourceDictionaryDemo"
             xmlns:theme="clr-namespace:MyThemes;assembly=MyThemes">
    <ContentPage.Resources>
        <ResourceDictionary>
            <!-- Add more resources here -->
            <ResourceDictionary.MergedDictionaries>
                <!-- Add more resource dictionaries here -->
                <local:MyResourceDictionary />
                <theme:LightTheme />
                <!-- Add more resource dictionaries here -->
            </ResourceDictionary.MergedDictionaries>
            <!-- Add more resources here -->
        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

> [!IMPORTANT]
> Es kann nur `MergedDictionaries` ein Eigenschaften Element-Tag in einer [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)geben, aber Sie können dort beliebig viele `ResourceDictionary` Objekte einfügen.

Beim Zusammenführen der [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) Freigeben von Ressourcen identisch `x:Key` Attributwerte, Xamarin.Forms verwendet die folgende Ressource Rangfolge:

1. Die Ressourcen, die lokal auf das Ressourcenverzeichnis.
1. Die Ressourcen in die Ressourcenverzeichnisse, die über zusammengeführt wurden die `MergedDictionaries` Auflistung, in der umgekehrten Reihenfolge sie in finden der `MergedDictionaries` Eigenschaft.

> [!NOTE]
> Ressourcenverzeichnisse suchen kann eine rechenintensive Aufgabe sein, wenn eine Anwendung mehrere enthält große Ressourcenwörterbücher. Um zu vermeiden, unnötige Suchläufe, sollten Sie daher sicherstellen, dass jede Seite in einer Anwendung nur Ressourcenverzeichnisse verwendet, die auf der Seite geeignet sind.

## <a name="related-links"></a>Verwandte Links

- [Ressourcenverzeichnisse (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-resourcedictionaries)
- [Stile](~/xamarin-forms/user-interface/styles/index.md)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)

## <a name="related-video"></a>Verwandte Videos

> [!Video https://channel9.msdn.com/Shows/XamarinShow/XamarinForms-101-Application-Resources/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
