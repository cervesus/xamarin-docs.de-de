---
title: Xamarin.Forms Ressourcenwörterbücher
description: Xamarin.Forms XAML-Ressourcen sind Objekte, die in einer Xamarin.Forms-Anwendung freigegeben und wiederverwendet werden können.
ms.prod: xamarin
ms.assetid: DF103686-4A92-40FA-9CF1-A9376293B13C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/01/2020
ms.custom: video
ms.openlocfilehash: 8dd3c7f36ddd436a812927816a1326dbb7c48341
ms.sourcegitcommit: ee9e48e2ec643915f42a6c1641077970ae20cb17
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2020
ms.locfileid: "80523214"
---
# <a name="xamarinforms-resource-dictionaries"></a>Xamarin.Forms Ressourcenwörterbücher

[![Beispiel](~/media/shared/download.png) herunterladen Download des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-resourcedictionaries)

A [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) ist ein Repository für Ressourcen, die von einer Xamarin.Forms-Anwendung verwendet werden. Typische Ressourcen, die `ResourceDictionary` in [Include-Stilen,](~/xamarin-forms/user-interface/styles/index.md) [Steuerelementvorlagen,](~/xamarin-forms/app-fundamentals/templates/control-template.md) [Datenvorlagen,](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)Farben und Konvertern gespeichert sind.

In XAML können Ressourcen, [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) die in einem gespeichert sind, `StaticResource` mithilfe `DynamicResource` der oder Markuperweiterung referenziert und auf Elemente angewendet werden. Ressourcen können auch in einem definiert `ResourceDictionary` und dann mithilfe eines zeichenfolgenbasierten Indexers auf Elemente verwiesen und angewendet werden. Es hat jedoch wenig Vorteile `ResourceDictionary` bei der Verwendung von a in C, da freigegebene Objekte als Felder oder Eigenschaften gespeichert werden können und direkt darauf zugegriffen werden können, ohne sie zuerst aus einem Wörterbuch abrufen zu müssen.

## <a name="create-resources-in-xaml"></a>Erstellen von Ressourcen in XAML

Jedes [`VisualElement`](xref:Xamarin.Forms.VisualElement) abgeleitete [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) Objekt verfügt über [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) eine Eigenschaft, die Ressourcen enthalten kann. Ebenso verfügt [`Application`](xref:Xamarin.Forms.Application) ein abgeleitetes Objekt über eine [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) Eigenschaft, die [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) Ressourcen enthalten kann.

Eine Xamarin.Forms-Anwendung enthält nur eine [`Application`](xref:Xamarin.Forms.Application)Klasse, die von ableitet, [`VisualElement`](xref:Xamarin.Forms.VisualElement)verwendet jedoch häufig viele Klassen, die von ableiten, einschließlich Seiten, Layouts und Steuerelementen. Jedes dieser Objekte kann `Resources` seine Eigenschaft [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) auf eine enthaltende Ressourcen festlegen. Auswählen, wo eine `ResourceDictionary` bestimmte Auswirkung gesetzt werden soll, wo die Ressourcen verwendet werden können:

- Ressourcen in `ResourceDictionary` einem, die einer [`Button`](xref:Xamarin.Forms.Button) Ansicht [`Label`](xref:Xamarin.Forms.Label) zugeordnet sind, z. B. oder nur auf dieses bestimmte Objekt angewendet werden können.
- Ressourcen in `ResourceDictionary` einem Layout, [`StackLayout`](xref:Xamarin.Forms.StackLayout) z. B. oder [`Grid`](xref:Xamarin.Forms.Grid) können auf das Layout und alle untergeordneten Elemente dieses Layouts angewendet werden.
- Ressourcen in `ResourceDictionary` einer auf Seitenebene definierten Seite können auf die Seite und alle untergeordneten Elemente angewendet werden.
- Ressourcen in `ResourceDictionary` einer auf Anwendungsebene definierten Ressourcen können in der gesamten Anwendung angewendet werden.

Mit Ausnahme impliziter Stile muss jede Ressource im Ressourcenwörterbuch über einen `x:Key` eindeutigen Zeichenfolgenschlüssel verfügen, der mit dem Attribut definiert ist.

Das folgende XAML zeigt Ressourcen, `ResourceDictionary` die in einer Anwendungsebene in der **Datei App.xaml** definiert sind:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ResourceDictionaryDemo.App">
    <Application.Resources>

        <Thickness x:Key="PageMargin">20</Thickness>

        <!-- Colors -->
        <Color x:Key="AppBackgroundColor">AliceBlue</Color>
        <Color x:Key="NavigationBarColor">#1976D2</Color>
        <Color x:Key="NavigationBarTextColor">White</Color>
        <Color x:Key="NormalTextColor">Black</Color>

        <!-- Implicit styles -->
        <Style TargetType="{x:Type NavigationPage}">
            <Setter Property="BarBackgroundColor"
                    Value="{StaticResource NavigationBarColor}" />
            <Setter Property="BarTextColor"
                    Value="{StaticResource NavigationBarTextColor}" />
        </Style>

        <Style TargetType="{x:Type ContentPage}"
               ApplyToDerivedTypes="True">
            <Setter Property="BackgroundColor"
                    Value="{StaticResource AppBackgroundColor}" />
        </Style>

    </Application.Resources>
</Application>
```

In diesem Beispiel definiert das [`Thickness`](xref:Xamarin.Forms.Thickness) Ressourcenwörterbuch [`Color`](xref:Xamarin.Forms.Color) eine Ressource, [`Style`](xref:Xamarin.Forms.Style) mehrere Ressourcen und zwei implizite Ressourcen. Weitere Informationen zur `App` Klasse finden Sie unter [Xamarin.Forms App Class](~/xamarin-forms/app-fundamentals/application-class.md).

> [!NOTE]
> Es ist auch gültig, alle `ResourceDictionary` Ressourcen zwischen expliziten Tags zu platzieren. Seit Xamarin.Forms 3.0 `ResourceDictionary` sind die Tags jedoch nicht erforderlich. Stattdessen wird `ResourceDictionary` das Objekt automatisch erstellt, und Sie `Resources` können die Ressourcen direkt zwischen den Eigenschaftenelement-Tags einfügen.

## <a name="consume-resources-in-xaml"></a>Ressourcenverbrauch in XAML

Jede Ressource verfügt über einen `x:Key` Schlüssel, der mithilfe des `ResourceDictionary`Attributs angegeben wird, der zu seinem Wörterbuchschlüssel im wird. Der Schlüssel wird verwendet, um [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) auf [`StaticResource`](xref:Xamarin.Forms.Xaml.StaticResourceExtension) [`DynamicResource`](xref:Xamarin.Forms.Xaml.DynamicResourceExtension) eine Ressource aus der mit der oder Markuperweiterung zu verweisen.

Die `StaticResource` Markuperweiterung ähnelt `DynamicResource` der Markuperweiterung, da beide einen Wörterbuchschlüssel verwenden, um auf einen Wert aus einem Ressourcenwörterbuch zu verweisen. Während die `StaticResource` Markuperweiterung jedoch eine einzelne Wörterbuchsuche durchführt, behält die `DynamicResource` Markuperweiterung eine Verknüpfung mit dem Wörterbuchschlüssel bei. Wenn daher der dem Schlüssel zugeordnete Wörterbucheintrag ersetzt wird, wird die Änderung auf das visuelle Element angewendet. Dadurch können Laufzeitressourcenänderungen in einer Anwendung vorgenommen werden. Weitere Informationen zu Markuperweiterungen finden Sie unter [XAML Markup Extensions](~/xamarin-forms/xaml/markup-extensions/index.md).

Das folgende XAML-Beispiel zeigt, wie Ressourcen verbraucht werden, und definiert zusätzliche Ressourcen in einem: [`StackLayout`](xref:Xamarin.Forms.StackLayout)

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ResourceDictionaryDemo.HomePage"
             Title="Home Page">
    <StackLayout Margin="{StaticResource PageMargin}">
        <StackLayout.Resources>
            <!-- Implicit style -->
            <Style TargetType="Button">
                <Setter Property="FontSize" Value="Medium" />
                <Setter Property="BackgroundColor" Value="#1976D2" />
                <Setter Property="TextColor" Value="White" />
                <Setter Property="CornerRadius" Value="5" />
            </Style>
        </StackLayout.Resources>

        <Label Text="This app demonstrates consuming resources that have been defined in resource dictionaries." />
        <Button Text="Navigate"
                Clicked="OnNavigateButtonClicked" />
    </StackLayout>
</ContentPage>
```

In diesem Beispiel [`ContentPage`](xref:Xamarin.Forms.ContentPage) verwendet das Objekt den impliziten Stil, der im Ressourcenwörterbuch auf Anwendungsebene definiert ist. Das [`StackLayout`](xref:Xamarin.Forms.StackLayout) Objekt verwendet `PageMargin` die im Ressourcenwörterbuch auf Anwendungsebene definierte Ressource, während [`Button`](xref:Xamarin.Forms.Button) [`StackLayout`](xref:Xamarin.Forms.StackLayout) das Objekt den im Ressourcenwörterbuch definierten impliziten Stil verwendet. Dies ergibt die in den folgenden Screenshots gezeigte Darstellung:

[![Verbrauch von ResourceDictionary-Ressourcen](resource-dictionaries-images/consuming.png "Verwenden von ResourceDictionary-Ressourcen")](resource-dictionaries-images/consuming-large.png#lightbox "Verwenden von ResourceDictionary-Ressourcen")

> [!IMPORTANT]
> Ressourcen, die für eine einzelne Seite spezifisch sind, sollten nicht in ein Ressourcenwörterbuch auf Anwendungsebene aufgenommen werden, da diese Ressourcen dann beim Anwendungsstart analysiert werden, anstatt wenn dies von einer Seite benötigt wird. Weitere Informationen finden Sie unter [Reduzieren der Größe des Anwendungsressourcenwörterbuchs](~/xamarin-forms/deploy-test/performance.md).

## <a name="resource-lookup-behavior"></a>Ressourcensuchverhalten

Der folgende Suchvorgang tritt auf, wenn [`StaticResource`](xref:Xamarin.Forms.Xaml.StaticResourceExtension) auf [`DynamicResource`](xref:Xamarin.Forms.Xaml.DynamicResourceExtension) eine Ressource mit der Oder-Markuperweiterung verwiesen wird:

- Der angeforderte Schlüssel wird im Ressourcenwörterbuch, falls vorhanden, für das Element geprüft, das die Eigenschaft festlegt. Wenn der angeforderte Schlüssel gefunden wird, wird sein Wert zurückgegeben, und der Suchvorgang wird beendet.
- Wenn keine Übereinstimmung gefunden wird, durchsucht der Suchprozess die visuelle Struktur nach oben und überprüft das Ressourcenwörterbuch jedes übergeordneten Elements. Wenn der angeforderte Schlüssel gefunden wird, wird sein Wert zurückgegeben, und der Suchvorgang wird beendet. Andernfalls wird der Prozess nach oben fortgesetzt, bis das Stammelement erreicht ist.
- Wenn eine Übereinstimmung im Stammelement nicht gefunden wird, wird das Ressourcenwörterbuch auf Anwendungsebene untersucht.
- Wenn eine Übereinstimmung immer noch nicht `XamlParseException` gefunden wird, wird eine ausgelöst.

Wenn der XAML-Parser `StaticResource` auf `DynamicResource` eine oder eine Markuperweiterung trifft, sucht er daher nach einem übereinstimmenden Schlüssel, indem er durch die visuelle Struktur nach oben fährt und die erste Übereinstimmung verwendet, die er findet. Wenn diese Suche auf der Seite endet und der Schlüssel immer noch nicht gefunden `ResourceDictionary` wurde, `App` durchsucht der XAML-Parser das an das Objekt angefügte Objekt. Wenn der Schlüssel immer noch nicht gefunden wird, wird eine Ausnahme ausgelöst.

## <a name="override-resources"></a>Überschreiben von Ressourcen

Wenn Ressourcen Schlüssel gemeinsam nutzen, haben Ressourcen, die in der visuellen Struktur definiert sind, Vorrang vor den höher definierten Ressourcen. Beispielsweise wird das `AppBackgroundColor` Festlegen `AliceBlue` einer Ressource auf Anwendungsebene durch `AppBackgroundColor` eine Ressourcenressource auf Seitenebene überschrieben, die auf `Teal`festgelegt ist. Ebenso wird eine `AppBackgroundColor` Ressource auf Seitenebene von `AppBackgroundColor` einer Ressource auf Steuerelementebene überschrieben.

## <a name="stand-alone-resource-dictionaries"></a>Eigenständige Ressourcenwörterbücher

Eine von [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) einer Klasse abgeleitete Klasse kann sich auch in einer separaten eigenständigen Datei befinden. Die resultierende Datei kann dann von Anwendungen gemeinsam genutzt werden.

Um eine solche Datei zu erstellen, fügen Sie dem Projekt ein neues **Inhaltsansichts-** oder **Inhaltsseitenelement** hinzu (jedoch keine **Inhaltsansicht** oder **Inhaltsseite** mit nur einer C-Datei). Löschen Sie die CodeBehind-Datei, und ändern Sie in der [`ContentView`](xref:Xamarin.Forms.ContentView) XAML-Datei den Namen der Basisklasse von oder [`ContentPage`](xref:Xamarin.Forms.ContentPage) in [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary). Entfernen Sie außerdem `x:Class` das Attribut aus dem Root-Tag der Datei.

Das folgende XAML-Beispiel zeigt einen [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) Namen **MyResourceDictionary.xaml**:

```xaml
<ResourceDictionary xmlns="http://xamarin.com/schemas/2014/forms"
                    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml">
    <DataTemplate x:Key="PersonDataTemplate">
        <ViewCell>
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="0.5*" />
                    <ColumnDefinition Width="0.2*" />
                    <ColumnDefinition Width="0.3*" />
                </Grid.ColumnDefinitions>
                <Label Text="{Binding Name}"
                       TextColor="{StaticResource NormalTextColor}"
                       FontAttributes="Bold" />
                <Label Grid.Column="1"
                       Text="{Binding Age}"
                       TextColor="{StaticResource NormalTextColor}" />
                <Label Grid.Column="2"
                       Text="{Binding Location}"
                       TextColor="{StaticResource NormalTextColor}"
                       HorizontalTextAlignment="End" />
            </Grid>
        </ViewCell>
    </DataTemplate>
</ResourceDictionary>
```

In diesem Beispiel [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) enthält der eine einzelne Ressource, [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)die ein Objekt vom Typ ist. **MyResourceDictionary.xaml** kann durch Zusammenführen in ein anderes Ressourcenwörterbuch verbraucht werden.

## <a name="merged-resource-dictionaries"></a>Zusammengeführte Ressourcenverzeichnisse

Zusammengeführte Ressourcenwörterbücher kombinieren [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) ein oder `ResourceDictionary`mehrere Objekte in einem anderen .

### <a name="merge-local-resource-dictionaries"></a>Zusammenführen lokaler Ressourcenwörterbücher

Eine [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) lokale Datei kann `ResourceDictionary` in einer `ResourceDictionary` anderen [`Source`](xref:Xamarin.Forms.ResourceDictionary.Source) zusammengeführt werden, indem ein Objekt erstellt wird, dessen Eigenschaft auf den Dateinamen der XAML-Datei mit den Folgenden festgelegt ist:

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

Diese Syntax instanziiert die `MyResourceDictionary` Klasse nicht. Stattdessen verweist er auf die XAML-Datei. Aus diesem Grund ist [`Source`](xref:Xamarin.Forms.ResourceDictionary.Source) beim Festlegen der Eigenschaft keine CodeBehind-Datei `x:Class` erforderlich, und das Attribut kann aus dem Root-Tag der Datei **MyResourceDictionary.xaml** entfernt werden.

> [!IMPORTANT]
> Die [`Source`](xref:Xamarin.Forms.ResourceDictionary.Source) Eigenschaft kann nur von XAML festgelegt werden.

### <a name="merge-resource-dictionaries-from-other-assemblies"></a>Zusammenführen von Ressourcenwörterbüchern aus anderen Assemblys

A [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) kann auch in `ResourceDictionary` einem anderen [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) zusammengeführt `ResourceDictionary`werden, indem es der Eigenschaft der hinzugefügt wird. Mit dieser Technik können Ressourcenwörterbücher zusammengeführt werden, unabhängig von der Assembly, in der sie sich befinden. Das Zusammenführen von Ressourcenwörterbüchern [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) aus externen Assemblys erfordert, dass eine Buildaktion auf **EmbeddedResource**festgelegt ist, eine CodeBehind-Datei enthält und das `x:Class` Attribut im Stammtag der Datei definiert wird.

> [!WARNING]
> Die [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) Klasse definiert [`MergedWith`](xref:Xamarin.Forms.ResourceDictionary.MergedWith) auch eine Eigenschaft. Diese Eigenschaft ist jedoch veraltet und sollte nicht mehr verwendet werden.

Das folgende Codebeispiel zeigt zwei Ressourcenwörterbücher, die [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) der [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)Auflistung einer Seitenebene hinzugefügt werden:

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

In diesem Beispiel werden ein Ressourcenwörterbuch aus derselben Assembly und ein Ressourcenwörterbuch aus einer externen Assembly mit dem Ressourcenwörterbuch auf Seitenebene zusammengeführt. Darüber hinaus können Sie `ResourceDictionary` auch andere [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) Objekte innerhalb der Eigenschaftselement-Tags und andere Ressourcen außerhalb dieser Tags hinzufügen.

> [!IMPORTANT]
> Es kann nur `MergedDictionaries` ein Eigenschaftselement-Tag in einem [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) `ResourceDictionary` vorhanden sein, aber Sie können beeinete Objekte wie erforderlich einstecken.

Wenn [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) zusammengeführte `x:Key` Ressourcen identische Attributwerte verwenden, verwendet Xamarin.Forms die folgende Ressourcenpriorität:

1. Die Ressourcen, die für das Ressourcenwörterbuch lokal sind.
1. Die Ressourcen, die in den Ressourcenwörterbüchern enthalten sind, die über `MergedDictionaries` `MergedDictionaries` die Auflistung zusammengeführt wurden, in der umgekehrten Reihenfolge, in der sie in der Eigenschaft aufgeführt sind.

> [!NOTE]
> Das Durchsuchen von Ressourcenwörterbüchern kann eine rechenintensive Aufgabe sein, wenn eine Anwendung mehrere, große Ressourcenwörterbücher enthält. Um unnötige Suchen zu vermeiden, sollten Sie daher sicherstellen, dass jede Seite in einer Anwendung nur Ressourcenwörterbücher verwendet, die für die Seite geeignet sind.

## <a name="related-links"></a>Verwandte Links

- [Ressourcenwörterbücher (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-resourcedictionaries)
- [XAML-Markuperweiterungen](~/xamarin-forms/xaml/markup-extensions/index.md)
- [Xamarin.Forms-Formatvorlagen](~/xamarin-forms/user-interface/styles/index.md)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)

## <a name="related-video"></a>Zugehörige Videos

> [!Video https://channel9.msdn.com/Shows/XamarinShow/XamarinForms-101-Application-Resources/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
