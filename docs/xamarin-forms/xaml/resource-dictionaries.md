---
title: Ressourcenverzeichnisse
description: XAML-Ressourcen sind Objekte, die freigegeben und erneut in einer Xamarin.Forms-Anwendung verwendet werden können.
ms.prod: xamarin
ms.assetid: DF103686-4A92-40FA-9CF1-A9376293B13C
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/21/2018
ms.openlocfilehash: d305de651f6e770d02ca35f5f8f8ffcf08424e28
ms.sourcegitcommit: bf51592be39b2ae3d63d029be1d7745ee63b0ce1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2018
ms.locfileid: "39573645"
---
# <a name="resource-dictionaries"></a>Ressourcenverzeichnisse

_XAML-Ressourcen sind Definitionen der Objekte, die gemeinsam genutzt und in einer Xamarin.Forms-Anwendung erneut verwendet werden können._

Diese Ressourcenobjekte werden in einem Ressourcenverzeichnis gespeichert. Dieser Artikel beschreibt, wie das Erstellen und nutzen ein Ressourcenverzeichnis und Ressourcenverzeichnisse zusammenführen.

## <a name="overview"></a>Übersicht

Ein [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) ist ein Repository für Ressourcen, die von einer Xamarin.Forms-Anwendung verwendet werden. Typische Ressourcen, die in gespeichert werden eine `ResourceDictionary` enthalten [Stile](~/xamarin-forms/user-interface/styles/index.md), [Steuerelementvorlagen](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md), [Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md), Farben und Typkonverter.

In XAML, Ressourcen, die im rowsetcache eine `ResourceDictionary` abgerufen und mit Elementen angewendet werden können die `StaticResource` Markuperweiterung. In c# können Ressourcen auch definiert werden eine `ResourceDictionary` abgerufen und mithilfe eines Indexers zeichenfolgenbasierten auf Elemente angewendet. Es ist jedoch wenig Vorteil der Verwendung einer `ResourceDictionary` in c# wie freigegebene Objekte einfach als Felder oder Eigenschaften gespeichert, und aufgerufen werden können direkt ohne zuerst müssen sie abrufen aus einem Wörterbuch.

## <a name="creating-and-consuming-a-resourcedictionary"></a>Erstellen und Nutzen von einem ResourceDictionary

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

## <a name="overriding-resources"></a>Ressourcen überschreiben

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

Hier ist eine andere Möglichkeit, sich vorzustellen `ResourceDictionary` Vorrang vor: Wenn die XAML-Parser erkennt eine `StaticResource`, es sucht nach einem übereinstimmenden Schlüssel, indem Sie sich über die visuelle Struktur zurücklegen, die erste Übereinstimmung findet. Wenn es sich bei dieser Suche wird beendet, auf der Seite, und der Schlüssel können weiterhin wurde nicht gefunden wurde, sucht der XAML-Parser die `ResourceDictionary` angefügt, um die `App` Objekt. Wenn der Schlüssel nicht gefunden wird, wird eine Ausnahme ausgelöst.

## <a name="stand-alone-resource-dictionaries"></a>Eigenständige Ressourcenverzeichnisse

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

Dieser Ansatz hat jedoch einige Einschränkungen: der `Resources` Eigenschaft der `ContentPage` verweist auf nur diese eine `ResourceDictionary`. In den meisten Fällen soll die Möglichkeit, einschließlich anderer `ResourceDictionary` Instanzen und möglicherweise andere Ressourcen als auch.

Diese Aufgabe erfordert zusammengeführte Ressourcenwörterbücher.

## <a name="merged-resource-dictionaries"></a>Zusammengeführte Ressourcenwörterbücher

Zusammengeführte Ressourcenwörterbücher kombinieren, eine oder mehrere `ResourceDictionary` Instanzen in eine andere `ResourceDictionary`. Sie können dazu in einer XAML-Datei durch Festlegen der [ `MergedDictionaries` ](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) Eigenschaft, um eine oder mehrere Ressourcenverzeichnisse, die in der Anwendung, die Seite oder die Steuerungsebene zusammengeführt werden `ResourceDictionary`.

> [!IMPORTANT]
> `ResourceDictionary` definiert auch eine [ `MergedWith` ](xref:Xamarin.Forms.ResourceDictionary.MergedWith) Eigenschaft. Verwenden Sie diese Eigenschaft nicht. Es wurde seit Xamarin.Forms 3.0 als veraltet markiert.

Eine Instanz von `MyResourceDictionary` zusammengeführt werden können, in Anwendungen, Seite oder Steuerungsebene `ResourceDictionary`. Im folgende XAML-Codebeispiel in eine auf Seitenebene gemergt wird `ResourceDictionary` mithilfe der `MergedDictionaries` Eigenschaft:

```xaml
<ContentPage ...>
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

Dieses Markup zeigt nur eine Instanz der `MyResourceDictionary` hinzugefügt wird die `ResourceDictionary` , aber Sie können auch andere verweisen `ResourceDictionary` Instanzen innerhalb der `MergedDictionary` Eigenschaftenelement Tags und andere Ressourcen außerhalb dieser Tags:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <ResourceDictionary>

            <!-- Add more resources here -->

            <ResourceDictionary.MergedDictionaries>

                <!-- Add more resource dictionaries here -->

                <local:MyResourceDictionary />

                <!-- Add more resource dictionaries here -->

            </ResourceDictionary.MergedDictionaries>

            <!-- Add more resources here -->

        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Es kann nur eine `MergedDictionaries` im Abschnitt eine `ResourceDictionary`, Sie können jedoch so viele `ResourceDictionary` Instanzen vorhanden, wie Sie möchten.

Beim Zusammenführen der [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) Freigeben von Ressourcen identisch `x:Key` Attributwerte, Xamarin.Forms verwendet die folgende Ressource Rangfolge:

1. Die Ressourcen, die lokal auf das Ressourcenverzeichnis.
1. Die Ressourcen im Ressourcenverzeichnis, die zusammengeführt wurde über die veraltete [ `MergedWith` ](xref:Xamarin.Forms.ResourceDictionary.MergedWith) Eigenschaft.
1. Die Ressourcen in die Ressourcenverzeichnisse, die über zusammengeführt wurden die `MergedDictionaries` Auflistung, in der umgekehrten Reihenfolge sie in finden der `MergedDictionaries` Eigenschaft.

> [!NOTE]
> Ressourcenverzeichnisse suchen kann eine rechenintensive Aufgabe sein, wenn eine Anwendung mehrere enthält große Ressourcenwörterbücher. Um zu vermeiden, unnötige Suchläufe, sollten Sie daher sicherstellen, dass jede Seite in einer Anwendung nur Ressourcenverzeichnisse verwendet, die auf der Seite geeignet sind.

## <a name="merging-dictionaries-in-xamarinforms-30"></a>Das Zusammenführen von Wörterbüchern in Xamarin.Forms 3.0

Beginnend mit Xamarin.Forms 3.0, Zusammenführen von [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) Instanzen ist etwas einfacher und flexibler geworden. Die `MergedDictionaries` Eigenschaftenelement Tags werden nicht mehr benötigt. Stattdessen, Sie fügen Sie in das Ressourcenverzeichnis ein weiteres `ResourceDictionary` Tag mit dem neuen [ `Source` ](xref:Xamarin.Forms.ResourceDictionary.Source) -Eigenschaft auf den Dateinamen der XAML-Datei mit den Ressourcen:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <ResourceDictionary>

            <!-- Add more resources here -->

            <ResourceDictionary Source="MyResourceDictionary.xaml" />

            <!-- Add more resources here -->

        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Da Xamarin.Forms 3.0 automatisch instanziiert die `ResourceDictionary`, diese beiden äußeren `ResourceDictionary` Tags sind nicht mehr erforderlich:

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

Dieser neuen Syntax ist _nicht_ Instanziieren der `MyResourceDictionary` Klasse. Stattdessen verweist es auf die XAML-Datei. Der Grund, dass die Code-Behind-Datei (**MyResourceDictionary.xaml.cs**) ist nicht mehr erforderlich. Sie können auch Entfernen der `x:Class` Attribut aus der Stamm-Tag, der die **MyResourceDictionary.xaml** Datei.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie das Erstellen und nutzen einen [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), und wie Ressourcenverzeichnisse zusammengeführt. Ein `ResourceDictionary` können Ressourcen in einem zentralen Ort definiert und in einer Xamarin.Forms-Anwendung erneut verwendet werden.

## <a name="related-links"></a>Verwandte Links

- [Ressourcenverzeichnisse (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/xaml/resourcedictionaries/)
- [Stile](~/xamarin-forms/user-interface/styles/index.md)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
