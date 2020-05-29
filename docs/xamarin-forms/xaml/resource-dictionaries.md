---
title: Xamarin.FormsRessourcen Wörterbücher
description: Xamarin.FormsXAML-Ressourcen sind Objekte, die in einer-Anwendung freigegeben und wieder verwendet werden können Xamarin.Forms .
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.custom: ''
ms.openlocfilehash: a1c7cfd4a0f3549b11ac51dc13b40da552f6b758
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139411"
---
# <a name="xamarinforms-resource-dictionaries"></a>Xamarin.FormsRessourcen Wörterbücher

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-resourcedictionaries)

Ein [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) ist ein Repository für Ressourcen, die von einer- Xamarin.Forms Anwendung verwendet werden. Typische Ressourcen, die in einem `ResourceDictionary` enthalten sind, sind beispielsweise [Stile](~/xamarin-forms/user-interface/styles/index.md), [Steuerelement Vorlagen](~/xamarin-forms/app-fundamentals/templates/control-template.md), [Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md), Farben und Konverter.

In XAML können auf Ressourcen, die in einem gespeichert sind, [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) verwiesen und auf Elemente angewendet werden, indem die- `StaticResource` oder- `DynamicResource` Markup Erweiterung verwendet wird. In c# können Ressourcen auch in einer definiert `ResourceDictionary` und dann mithilfe eines zeichenfolgenbasierten Indexers auf Elemente verwiesen werden. Die Verwendung `ResourceDictionary` von in c# hat jedoch nur einen geringen Vorteil, da freigegebene Objekte als Felder oder Eigenschaften gespeichert werden können und direkt auf Sie zugegriffen werden kann, ohne Sie zuerst von einem Wörterbuch abrufen zu müssen.

## <a name="create-resources-in-xaml"></a>Erstellen von Ressourcen in XAML

Jedes [`VisualElement`](xref:Xamarin.Forms.VisualElement) abgeleitete Objekt verfügt über eine- [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) Eigenschaft, die ein-Objekt ist, [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) das Ressourcen enthalten kann. Entsprechend verfügt ein [`Application`](xref:Xamarin.Forms.Application) abgeleitetes Objekt über eine- [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) Eigenschaft, die ein-Objekt ist, [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) das Ressourcen enthalten kann.

Eine- Xamarin.Forms Anwendung enthält nur eine Klasse, die von abgeleitet [`Application`](xref:Xamarin.Forms.Application) ist, aber häufig viele Klassen verwendet, die von abgeleitet [`VisualElement`](xref:Xamarin.Forms.VisualElement) werden, einschließlich Seiten, Layouts und Steuerelemente. Bei allen diesen Objekten kann die- `Resources` Eigenschaft auf eine [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) enthaltende Ressourcen festgelegt werden. Auswählen des Orts, an dem `ResourceDictionary` die Ressourcen verwendet werden können:

- Ressourcen in einer `ResourceDictionary` , die an eine Ansicht wie oder angefügt ist, [`Button`](xref:Xamarin.Forms.Button) [`Label`](xref:Xamarin.Forms.Label) können nur auf dieses bestimmte Objekt angewendet werden.
- Ressourcen, `ResourceDictionary` die an ein Layout wie oder angefügt [`StackLayout`](xref:Xamarin.Forms.StackLayout) sind, [`Grid`](xref:Xamarin.Forms.Grid) können auf das Layout und alle untergeordneten Elemente dieses Layouts angewendet werden.
- Ressourcen in einer, `ResourceDictionary` die auf der Seitenebene definiert ist, können auf die Seite und alle untergeordneten Elemente angewendet werden.
- Ressourcen in einer, `ResourceDictionary` die auf Anwendungsebene definiert ist, können in der gesamten Anwendung angewendet werden.

Mit Ausnahme impliziter Stile muss jede Ressource im Ressourcen Wörterbuch über einen eindeutigen Zeichen folgen Schlüssel verfügen, der mit dem-Attribut definiert ist `x:Key` .

Der folgende XAML-Code zeigt Ressourcen, die auf Anwendungsebene `ResourceDictionary` in der **app. XAML** -Datei definiert sind:

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

In diesem Beispiel definiert das Ressourcen Wörterbuch eine [`Thickness`](xref:Xamarin.Forms.Thickness) Ressource, mehrere [`Color`](xref:Xamarin.Forms.Color) Ressourcen und zwei implizite [`Style`](xref:Xamarin.Forms.Style) Ressourcen. Weitere Informationen zur- `App` Klasse finden Sie unter [ Xamarin.Forms App-Klasse](~/xamarin-forms/app-fundamentals/application-class.md).

> [!NOTE]
> Es ist auch zulässig, alle Ressourcen zwischen expliziten `ResourceDictionary` Tags zu platzieren. Seit Xamarin.Forms 3,0 `ResourceDictionary` sind die Tags jedoch nicht erforderlich. Stattdessen wird das `ResourceDictionary` Objekt automatisch erstellt, und Sie können die Ressourcen direkt zwischen den `Resources` Eigenschaften-Element-Tags einfügen.

## <a name="consume-resources-in-xaml"></a>Ressourcen in XAML verbrauchen

Jede Ressource verfügt über einen Schlüssel, der mit dem-Attribut angegeben wird `x:Key` , dessen Wörterbuch Schlüssel in der verwendet wird `ResourceDictionary` . Der Schlüssel wird verwendet, um auf eine Ressource aus der [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) mit der- [`StaticResource`](xref:Xamarin.Forms.Xaml.StaticResourceExtension) oder- [`DynamicResource`](xref:Xamarin.Forms.Xaml.DynamicResourceExtension) Markup Erweiterung zu verweisen.

Die `StaticResource` Markup Erweiterung ähnelt der `DynamicResource` Markup Erweiterung darin, dass beide einen Wörterbuch Schlüssel verwenden, um auf einen Wert aus einem Ressourcen Wörterbuch zu verweisen. Obwohl die `StaticResource` Markup Erweiterung eine einzelne Wörterbuchsuche ausführt, `DynamicResource` behält die Markup Erweiterung einen Link zum Wörterbuch Schlüssel bei. Wenn der Wörterbucheintrag, der dem Schlüssel zugeordnet ist, ersetzt wird, wird die Änderung daher auf das visuelle Element angewendet. Dadurch können Lauf Zeit Ressourcen Änderungen in einer Anwendung vorgenommen werden. Weitere Informationen zu Markup Erweiterungen finden Sie unter [XAML-Markup Erweiterungen](~/xamarin-forms/xaml/markup-extensions/index.md).

Im folgenden XAML-Beispiel wird gezeigt, wie Ressourcen genutzt werden können. Außerdem werden zusätzliche Ressourcen in einem-Beispiel definiert [`StackLayout`](xref:Xamarin.Forms.StackLayout) :

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

In diesem Beispiel verwendet das- [`ContentPage`](xref:Xamarin.Forms.ContentPage) Objekt den impliziten Stil, der im Ressourcen Wörterbuch auf Anwendungsebene definiert ist. Das- [`StackLayout`](xref:Xamarin.Forms.StackLayout) Objekt verwendet die `PageMargin` Ressource, die im Ressourcen Wörterbuch auf Anwendungsebene definiert ist, während das- [`Button`](xref:Xamarin.Forms.Button) Objekt den im [`StackLayout`](xref:Xamarin.Forms.StackLayout) Ressourcen Wörterbuch definierten impliziten Stil verwendet. Dies ergibt die in den folgenden Screenshots gezeigte Darstellung:

[![Verbrauchen von ResourceDictionary-Ressourcen](resource-dictionaries-images/consuming.png "Verbrauchen von ResourceDictionary-Ressourcen")](resource-dictionaries-images/consuming-large.png#lightbox "Verbrauchen von ResourceDictionary-Ressourcen")

> [!IMPORTANT]
> Ressourcen, die für eine einzelne Seite spezifisch sind, sollten nicht in ein Ressourcen Wörterbuch auf Anwendungsebene eingeschlossen werden, da diese Ressourcen beim Anwendungsstart und nicht bei der Verwendung durch eine Seite analysiert werden. Weitere Informationen finden Sie unter [reduzieren der Größe des Anwendungs Ressourcen Wörterbuchs](~/xamarin-forms/deploy-test/performance.md).

## <a name="resource-lookup-behavior"></a>Ressourcen Suchverhalten

Der folgende Suchvorgang tritt auf, wenn auf eine Ressource mit der- [`StaticResource`](xref:Xamarin.Forms.Xaml.StaticResourceExtension) oder- [`DynamicResource`](xref:Xamarin.Forms.Xaml.DynamicResourceExtension) Markup Erweiterung verwiesen wird:

- Der angeforderte Schlüssel wird für das-Element, das die-Eigenschaft festlegt, auf im Ressourcen Wörterbuch geprüft, sofern vorhanden. Wenn der angeforderte Schlüssel gefunden wird, wird der Wert zurückgegeben, und der Suchvorgang wird beendet.
- Wenn keine Entsprechung gefunden wird, durchsucht der Suchprozess die visuelle Struktur nach oben und überprüft das Ressourcen Wörterbuch jedes übergeordneten Elements. Wenn der angeforderte Schlüssel gefunden wird, wird der Wert zurückgegeben, und der Suchvorgang wird beendet. Andernfalls wird der Prozess fortgesetzt, bis das Stamm Element erreicht ist.
- Wenn keine Entsprechung im Root-Element gefunden wird, wird das Ressourcen Wörterbuch auf Anwendungsebene untersucht.
- Wenn keine Entsprechung gefunden wird, wird eine ausgelöst `XamlParseException` .

Wenn der XAML-Parser auf eine- `StaticResource` oder- `DynamicResource` Markup Erweiterung stößt, sucht er daher nach einem übereinstimmenden Schlüssel, indem er die visuelle Struktur durchsucht, wobei die erste gefundene Übereinstimmung verwendet wird. Wenn diese Suche auf der Seite endet und der Schlüssel noch immer nicht gefunden wurde, durchsucht der XAML-Parser das `ResourceDictionary` angefügte-Objekt an das- `App` Objekt. Wenn der Schlüssel immer noch nicht gefunden wird, wird eine Ausnahme ausgelöst.

## <a name="override-resources"></a>Ressourcen überschreiben

Wenn Ressourcen Schlüssel gemeinsam nutzen, haben Ressourcen, die in der visuellen Struktur niedriger definiert sind, Vorrang vor den höheren Einstellungen. Wenn Sie z. b `AppBackgroundColor` . eine Ressource auf `AliceBlue` der Anwendungsebene auf festlegen, wird Sie von einer auf Seitenebene auf `AppBackgroundColor` festgelegten Ressource überschrieben `Teal` . Auf ähnliche Weise wird eine `AppBackgroundColor` Ressource auf Seitenebene von einer Ressource auf Steuerungsebene überschrieben `AppBackgroundColor` .

## <a name="stand-alone-resource-dictionaries"></a>Eigenständige Ressourcen Wörterbücher

Eine von abgeleitete Klasse [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) kann sich auch in einer separaten eigenständigen Datei befinden. Die resultierende Datei kann dann von den Anwendungen gemeinsam genutzt werden.

Um eine solche Datei zu erstellen, fügen Sie dem Projekt eine neue **Inhaltsansicht** oder ein **Inhaltsseiten** Element hinzu (jedoch keine **Inhaltsansicht** oder **Inhaltsseite** mit einer c#-Datei). Löschen Sie die Code Behind-Datei, und ändern Sie in der XAML-Datei den Namen der Basisklasse von [`ContentView`](xref:Xamarin.Forms.ContentView) oder in [`ContentPage`](xref:Xamarin.Forms.ContentPage) [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) . Entfernen Sie außerdem das- `x:Class` Attribut aus dem Stammtag der Datei.

Das folgende XAML-Beispiel zeigt einen mit dem [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) Namen **myresourcedictionary. XAML**:

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

In diesem Beispiel [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) enthält eine einzelne Ressource, bei der es sich um ein Objekt vom Typ handelt [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) . **Myresourcedictionary. XAML** kann durch Zusammenführen in ein anderes Ressourcen Wörterbuch verwendet werden.

## <a name="merged-resource-dictionaries"></a>Zusammengeführte Ressourcenverzeichnisse

Zusammengeführte Ressourcen Wörterbücher kombinieren ein oder mehrere [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) Objekte zu einem anderen `ResourceDictionary` .

### <a name="merge-local-resource-dictionaries"></a>Lokale Ressourcen Wörterbücher zusammenführen

Eine lokale [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) Datei kann mit einem anderen zusammengeführt werden `ResourceDictionary` , indem ein- `ResourceDictionary` Objekt erstellt [`Source`](xref:Xamarin.Forms.ResourceDictionary.Source) wird, dessen-Eigenschaft auf den Dateinamen der XAML-Datei mit den Ressourcen festgelegt ist:

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

Diese Syntax instanziiert die- `MyResourceDictionary` Klasse nicht. Stattdessen verweist Sie auf die XAML-Datei. Aus diesem Grund ist beim Festlegen der [`Source`](xref:Xamarin.Forms.ResourceDictionary.Source) -Eigenschaft keine Code Behind-Datei erforderlich, und das- `x:Class` Attribut kann aus dem Stammtag der **myresourcedictionary. XAML** -Datei entfernt werden.

> [!IMPORTANT]
> Die- [`Source`](xref:Xamarin.Forms.ResourceDictionary.Source) Eigenschaft kann nur aus XAML festgelegt werden.

### <a name="merge-resource-dictionaries-from-other-assemblies"></a>Ressourcen Wörterbücher aus anderen Assemblys

Ein [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) kann auch mit einem anderen zusammengeführt werden, `ResourceDictionary` indem es der-Eigenschaft des hinzugefügt wird [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) `ResourceDictionary` . Diese Technik ermöglicht das Zusammenführen von Ressourcen Wörterbüchern, unabhängig von der Assembly, in der Sie sich befinden. Das Zusammenführen von Ressourcen Wörterbüchern aus externen Assemblys erfordert, dass [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) eine Buildaktion auf **EmbeddedResource**festgelegt ist, eine Code-Behind-Datei haben und das `x:Class` Attribut im Stammtag der Datei definieren.

> [!WARNING]
> Die- [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) Klasse definiert auch eine- [`MergedWith`](xref:Xamarin.Forms.ResourceDictionary.MergedWith) Eigenschaft. Diese Eigenschaft ist jedoch veraltet und sollte nicht mehr verwendet werden.

Im folgenden Codebeispiel werden zwei Ressourcen Wörterbücher zur Auflistung [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) einer Seitenebene hinzugefügt [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) :

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

In diesem Beispiel werden ein Ressourcen Wörterbuch aus derselben Assembly und ein Ressourcen Wörterbuch aus einer externen Assembly in das Ressourcen Wörterbuch auf Seitenebene zusammengeführt. Darüber hinaus können Sie auch andere `ResourceDictionary` Objekte innerhalb der [`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) Eigenschaften-Element-Tags und andere Ressourcen außerhalb dieser Tags hinzufügen.

> [!IMPORTANT]
> Es kann nur ein `MergedDictionaries` Eigenschaften Element-Tag in einer geben [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) , aber Sie können dort beliebig viele `ResourceDictionary` Objekte einfügen.

Wenn zusammengeführte [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) Ressourcen identische `x:Key` Attributwerte gemeinsam Xamarin.Forms verwenden, verwendet die folgende Ressourcen Rangfolge:

1. Die lokalen Ressourcen des Ressourcen Wörterbuchs.
1. Die Ressourcen in den Ressourcen Wörterbüchern, die über die-Auflistung zusammengeführt wurden `MergedDictionaries` , werden in umgekehrter Reihenfolge in der- `MergedDictionaries` Eigenschaft aufgelistet.

> [!NOTE]
> Das Durchsuchen von Ressourcen Wörterbüchern kann eine rechenintensive Aufgabe sein, wenn eine Anwendung mehrere große Ressourcen Wörterbücher enthält. Daher sollten Sie sicherstellen, dass jede Seite in einer Anwendung nur für die Seite geeignete Ressourcen Wörterbücher verwendet, um unnötige suchen zu vermeiden.

## <a name="related-links"></a>Verwandte Links

- [Ressourcen Wörterbücher (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-resourcedictionaries)
- [XAML-Markuperweiterungen](~/xamarin-forms/xaml/markup-extensions/index.md)
- [Xamarin.FormsTanz](~/xamarin-forms/user-interface/styles/index.md)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)

## <a name="related-video"></a>Zugehörige Videos

> [!Video https://channel9.msdn.com/Shows/XamarinShow/XamarinForms-101-Application-Resources/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
