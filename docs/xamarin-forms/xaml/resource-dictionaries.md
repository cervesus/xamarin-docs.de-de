---
title: Ressourcenverzeichnisse
description: XAML-Ressourcen sind Objekte, die freigegeben und erneut in einer Xamarin.Forms-Anwendung verwendet werden können.
ms.prod: xamarin
ms.assetid: DF103686-4A92-40FA-9CF1-A9376293B13C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/26/2020
ms.custom: video
ms.openlocfilehash: 496251ec9596b9ef76fb34149acca184b5934c37
ms.sourcegitcommit: 6c60914b380ff679bbffd7790edd4d5e18005d0a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2020
ms.locfileid: "80070393"
---
# <a name="resource-dictionaries"></a>Ressourcenverzeichnisse

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-resourcedictionaries)

_XAML-Ressourcen sind Definitionen von Objekten, die in einer xamarin. Forms-Anwendung freigegeben und wieder verwendet werden können. Diese Ressourcen Objekte werden in einem Ressourcen Wörterbuch gespeichert._

Bei einem [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) handelt es sich um ein Repository für Ressourcen, die von einer xamarin. Forms-Anwendung verwendet werden. Typische Ressourcen, die in einem `ResourceDictionary` gespeichert sind, umfassen [Stile](~/xamarin-forms/user-interface/styles/index.md), [Steuerelement Vorlagen](~/xamarin-forms/app-fundamentals/templates/control-template.md), [Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md), Farben und Konverter.

In XAML können Ressourcen, die in einem `ResourceDictionary` gespeichert werden, mithilfe der `StaticResource` Markup Erweiterung abgerufen und auf Elemente angewendet werden. In C#können Ressourcen auch in einem `ResourceDictionary` definiert und dann mithilfe eines zeichenfolgenbasierten Indexers abgerufen und auf Elemente angewendet werden. Allerdings gibt es kaum Vorteile bei der Verwendung einer `ResourceDictionary` C#in, da freigegebene Objekte einfach als Felder oder Eigenschaften gespeichert werden können und direkt auf Sie zugegriffen werden kann, ohne Sie zuerst von einem Wörterbuch abrufen zu müssen.

## <a name="create-and-consume-a-resourcedictionary"></a>Erstellen und Verwenden eines ResourceDictionary

Ressourcen werden in einem [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) definiert, der anschließend auf eine der folgenden `Resources` Eigenschaften festgelegt wird:

- Die [`Resources`](xref:Xamarin.Forms.Application.Resources) -Eigenschaft einer Klasse, die von abgeleitet wird [`Application`](xref:Xamarin.Forms.Application)
- Die [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) -Eigenschaft einer Klasse, die von abgeleitet wird [`VisualElement`](xref:Xamarin.Forms.Application)

Ein xamarin. Forms-Programm enthält nur eine Klasse, die von `Application` abgeleitet ist, aber häufig viele Klassen verwendet, die von `VisualElement`abgeleitet sind, einschließlich Seiten, Layouts und Steuerelemente. Für jedes dieser Objekte kann die `Resources`-Eigenschaft auf einen `ResourceDictionary`festgelegt werden. Die Entscheidung, wo ein bestimmtes `ResourceDictionary` platziert werden soll, wirkt sich auf die Verwendung der Ressourcen aus:

- Ressourcen in einer `ResourceDictionary`, die an eine Ansicht wie `Button` oder `Label` angefügt ist, können nur auf dieses bestimmte Objekt angewendet werden. Dies ist daher nicht sehr nützlich.
- Ressourcen in einem `ResourceDictionary`, die an ein Layout wie `StackLayout` oder `Grid` angefügt sind, können auf das Layout und alle untergeordneten Elemente dieses Layouts angewendet werden.
- Ressourcen in einer `ResourceDictionary`, die auf der Seitenebene definiert sind, können auf die Seite und alle untergeordneten Elemente angewendet werden.
- Ressourcen in einer `ResourceDictionary`, die auf Anwendungsebene definiert sind, können in der gesamten Anwendung angewendet werden.

Der folgende XAML-Code zeigt Ressourcen, die auf Anwendungsebene definiert sind `ResourceDictionary` in der Datei **app. XAML** , die als Teil des xamarin. Forms-Standardprogramms erstellt wurde:

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

In dieser `ResourceDictionary` werden drei [`Color`](xref:Xamarin.Forms.Color) Ressourcen und eine [`Style`](xref:Xamarin.Forms.Style) Ressource definiert. Weitere Informationen zum `App`-Klasse finden Sie unter [App-Klasse](~/xamarin-forms/app-fundamentals/application-class.md).

Ab xamarin. Forms 3,0 sind die expliziten `ResourceDictionary` Tags nicht erforderlich. Das `ResourceDictionary` Objekt wird automatisch erstellt, und Sie können die Ressourcen direkt zwischen den `Resources` Eigenschaften-Element-Tags einfügen:

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

Jede Ressource verfügt über einen Schlüssel, der mit dem `x:Key`-Attribut angegeben wird, dessen Wörterbuch Schlüssel in der `ResourceDictionary`wird. Der Schlüssel wird verwendet, um eine Ressource aus dem `ResourceDictionary` von der [`StaticResource`](xref:Xamarin.Forms.Xaml.StaticResourceExtension) Markup Erweiterung abzurufen, wie im folgenden XAML-Codebeispiel veranschaulicht, das zusätzliche Ressourcen zeigt, die innerhalb der `StackLayout`definiert sind:

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

Die erste [`Label`](xref:Xamarin.Forms.Label) Instanz Ruft die `LabelPageHeadingStyle` Ressource ab, die auf der `ResourceDictionary`auf Anwendungsebene definiert ist, und nutzt Sie, wobei die zweite `Label` Instanz die `LabelNormalStyle` Ressource abruft und nutzt, die auf der Steuerungsebene `ResourceDictionary`definiert ist. Ebenso Ruft die [`Button`](xref:Xamarin.Forms.Button) Instanz die `NormalTextColor` Ressource ab, die auf der `ResourceDictionary`Ebene der Anwendungsebene definiert ist, und nutzt Sie, und die `MediumBoldText` Ressource, die auf der Steuerelement Ebene `ResourceDictionary`definiert ist. Dies ergibt die in den folgenden Screenshots gezeigte Darstellung:

[![verbrauchen von ResourceDictionary-Ressourcen](resource-dictionaries-images/screenshots-sml.png)](resource-dictionaries-images/screenshots.png#lightbox)

> [!NOTE]
> Ressourcen, die auf eine einzelne Seite spezifisch sind, darf nicht in einer Anwendung auf Ressourcenverzeichnis, daher enthalten sein, die Ressourcen dann beim Start der Anwendung nicht analysiert werden soll, bei einer Seite erforderlich. Weitere Informationen finden Sie unter [reduzieren der Größe des Anwendungs Ressourcen Wörterbuchs](~/xamarin-forms/deploy-test/performance.md).

## <a name="override-resources"></a>Ressourcen überschreiben

Wenn `ResourceDictionary` Ressourcen `x:Key` Attributwerte gemeinsam nutzen, haben Ressourcen, die in der Sicht Hierarchie niedriger definiert sind, Vorrang vor den höheren Werten. Beispielsweise wird das Festlegen der `PageBackgroundColor` Ressource auf die `Blue` auf Anwendungsebene von einer `PageBackgroundColor` Ressource auf Seitenebene überschrieben, die auf `Yellow`festgelegt ist. Ebenso wird eine `PageBackgroundColor` Ressource auf Seitenebene durch eine Steuerelement Ebene `PageBackgroundColor` Ressource überschrieben. Diese Priorität wird im folgenden XAML-Codebeispiel veranschaulicht:

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

Die ursprünglichen `PageBackgroundColor` und `NormalTextColor` Instanzen, die auf Anwendungsebene definiert sind, werden durch die `PageBackgroundColor` und `NormalTextColor` Instanzen überschrieben, die auf der Seitenebene definiert sind. Aus diesem Grund die Hintergrundfarbe ist Blau, und der Text auf der Seite wird Gelb, wie in den folgenden Screenshots veranschaulicht:

[![Überschreiben von ResourceDictionary-Ressourcen](resource-dictionaries-images/overridding-screenshots-sml.png)](resource-dictionaries-images/overridding-screenshots.png#lightbox)

Beachten Sie jedoch, dass die Hintergrund Leiste des [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) immer noch gelb ist, da die Eigenschaft [`BarBackgroundColor`](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor) auf den Wert der `PageBackgroundColor` Ressource festgelegt ist, die auf der Ebene der Anwendungs `ResourceDictionary`Ebene definiert ist.

Dies ist eine weitere Möglichkeit, um `ResourceDictionary` Rangfolge zu betrachten: Wenn der XAML-Parser auf eine `StaticResource`stößt, sucht er nach einem passenden Schlüssel, indem er die visuelle Struktur durchsucht, wobei die erste gefundene Übereinstimmung verwendet wird. Wenn diese Suche auf der Seite endet und der Schlüssel noch immer nicht gefunden wurde, durchsucht der XAML-Parser die `ResourceDictionary`, die an das `App` Objekt angefügt ist. Wenn der Schlüssel nicht gefunden wird, wird eine Ausnahme ausgelöst.

## <a name="stand-alone-resource-dictionaries"></a>Eigenständige Ressourcen Wörterbücher

Eine von `ResourceDictionary` abgeleitete Klasse kann sich auch in einer separaten eigenständigen Datei befinden. Die resultierende Datei kann für Anwendungen genutzt werden.

Um eine solche Datei zu erstellen, fügen Sie dem Projekt eine neue **Inhaltsansicht** oder ein **Inhaltsseiten** Element hinzu (aber keine **Inhaltsansicht** oder **Inhaltsseite** mit C# nur einer Datei). Ändern Sie in der XAML- C# Datei und-Datei den Namen der Basisklasse von `ContentView` oder `ContentPage` in `ResourceDictionary`. In der XAML-Datei ist der Name der Basisklasse Element der obersten Ebene.

Das folgende XAML-Beispiel zeigt eine [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) mit dem Namen `MyResourceDictionary`:

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

Diese `ResourceDictionary` enthält eine einzelne Ressource, bei der es sich um ein Objekt vom Typ `DataTemplate`handelt.

Sie können `MyResourceDictionary` instanziieren, indem Sie Sie zwischen einem Paar `Resources` Eigenschaften Element Tags, z. b. in einem `ContentPage`, einfügen:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <local:MyResourceDictionary />
    </ContentPage.Resources>
    ...
</ContentPage>
```

Eine Instanz von `MyResourceDictionary` ist auf die `Resources`-Eigenschaft des `ContentPage`-Objekts festgelegt.

Bei diesem Ansatz gelten jedoch einige Einschränkungen: die `Resources`-Eigenschaft des `ContentPage` verweist nur auf diese `ResourceDictionary`. In den meisten Fällen möchten Sie auch andere `ResourceDictionary` Instanzen und möglicherweise andere Ressourcen einschließen.

Diese Aufgabe erfordert zusammengeführte Ressourcenwörterbücher.

## <a name="merged-resource-dictionaries"></a>Zusammengeführte Ressourcen Verzeichnisse

Zusammengeführte Ressourcen Wörterbücher kombinieren ein oder mehrere [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) Objekte in einem anderen `ResourceDictionary`.

### <a name="merge-local-resource-dictionaries"></a>Lokale Ressourcen Wörterbücher zusammenführen

Eine lokale [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) kann in einer anderen `ResourceDictionary` zusammengeführt werden, indem die Eigenschaft [`Source`](xref:Xamarin.Forms.ResourceDictionary.Source) auf den Dateinamen der XAML-Datei mit den Ressourcen festgelegt wird:

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

Mit dieser Syntax wird die `MyResourceDictionary`-Klasse nicht instanziiert. Stattdessen verweist es auf die XAML-Datei. Wenn die [`Source`](xref:Xamarin.Forms.ResourceDictionary.Source) -Eigenschaft festgelegt wird, ist aus diesem Grund keine Code Behind-Datei erforderlich, und das `x:Class`-Attribut kann aus dem Stammtag der **myresourcedictionary. XAML** -Datei entfernt werden. Beim Zusammenführen von Ressourcen Wörterbüchern mithilfe dieses Ansatzes instanziiert xamarin. Forms die [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary), sodass die äußeren `ResourceDictionary` Tags nicht erforderlich sind.

> [!IMPORTANT]
> Die [`Source`](xref:Xamarin.Forms.ResourceDictionary.Source) -Eigenschaft kann nur aus XAML festgelegt werden.

### <a name="merge-resource-dictionaries-from-other-assemblies"></a>Ressourcen Wörterbücher aus anderen Assemblys

Eine [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) kann auch in einem anderen `ResourceDictionary` zusammengeführt werden, indem Sie Sie der [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) -Eigenschaft der `ResourceDictionary`hinzufügen. Diese Technik ermöglicht das Zusammenführen von Ressourcen Wörterbüchern, unabhängig von der Assembly, in der Sie sich befinden.

> [!IMPORTANT]
> Die [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) -Klasse definiert auch eine [`MergedWith`](xref:Xamarin.Forms.ResourceDictionary.MergedWith) -Eigenschaft. Diese Eigenschaft ist jedoch veraltet und sollte nicht mehr verwendet werden.

Im folgenden Codebeispiel wird gezeigt, `MyResourceDictionary` der [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) Auflistung eines [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)auf Seitenebene hinzugefügt wird:

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

Dieses Beispiel zeigt eine Instanz von `MyResourceDictionary`, die sich in derselben Assembly befindet und der [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)hinzugefügt wird. Außerdem können Sie Ressourcen Wörterbücher aus anderen Assemblys, anderen `ResourceDictionary` Objekten innerhalb der [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) Eigenschaften-Element-Tags und anderen Ressourcen außerhalb der folgenden Tags hinzufügen:

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

Wenn Sie eine [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) in einer externen Assembly platzieren, stellen Sie sicher, dass die Buildaktion auf **EmbeddedResource**festgelegt ist. Außerdem müssen Sie sicherstellen, dass Sie über eine Code-Behind-Datei verfügt.

> [!IMPORTANT]
> Es darf nur ein `MergedDictionaries` Property-Element-Tag in einer [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)vorhanden sein, aber Sie können dort beliebig viele `ResourceDictionary` Objekte ablegen.

Wenn zusammengeführte [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) Ressourcen identische `x:Key` Attributwerte gemeinsam verwenden, verwendet xamarin. Forms die folgende Ressourcen Rangfolge:

1. Die Ressourcen, die lokal auf das Ressourcenverzeichnis.
1. Die Ressourcen in den Ressourcen Wörterbüchern, die über die `MergedDictionaries` Auflistung zusammengeführt wurden, werden in umgekehrter Reihenfolge in der `MergedDictionaries`-Eigenschaft aufgelistet.

> [!NOTE]
> Ressourcenverzeichnisse suchen kann eine rechenintensive Aufgabe sein, wenn eine Anwendung mehrere enthält große Ressourcenwörterbücher. Um zu vermeiden, unnötige Suchläufe, sollten Sie daher sicherstellen, dass jede Seite in einer Anwendung nur Ressourcenverzeichnisse verwendet, die auf der Seite geeignet sind.

## <a name="related-links"></a>Verwandte Links

- [Ressourcen Wörterbücher (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-resourcedictionaries)
- [Stile](~/xamarin-forms/user-interface/styles/index.md)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)

## <a name="related-video"></a>Zugehörige Videos

> [!Video https://channel9.msdn.com/Shows/XamarinShow/XamarinForms-101-Application-Resources/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
