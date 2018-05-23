---
title: Ressourcenverzeichnis
description: Verwendung von XAML-Ressourcen sind Objekte, die freigegeben und wieder in der gesamten einer Xamarin.Forms-Anwendung verwendet werden können.
ms.prod: xamarin
ms.assetid: DF103686-4A92-40FA-9CF1-A9376293B13C
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/21/2018
ms.openlocfilehash: b9c15357895bae64176ef34a848b968917035f3d
ms.sourcegitcommit: 9f8e7393019791bbd6af4fefaa24a1602adabb4e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/23/2018
---
# <a name="resource-dictionaries"></a>Ressourcenverzeichnis

_Verwendung von XAML-Ressourcen sind Definitionen der Objekte, die freigegeben und wieder in der gesamten einer Xamarin.Forms-Anwendung verwendet werden können._

Diese Ressourcenobjekte werden in einem Ressourcenwörterbuch gespeichert. Dieser Artikel beschreibt das Erstellen und Nutzen eines Ressourcenverzeichnisses und Ressourcenwörterbücher zusammenführen.

## <a name="overview"></a>Übersicht

Ein [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) ist ein Repository für Ressourcen, die von einer Xamarin.Forms-Anwendung verwendet werden. Typische Ressourcen, die im rowsetcache eine `ResourceDictionary` enthalten [Stile](~/xamarin-forms/user-interface/styles/index.md), [Steuerelementvorlagen](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md), [Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md), Farben und Konverter.

In XAML wird in gespeicherten Ressourcen eine `ResourceDictionary` abgerufen und auf Elemente mit angewendet werden können die `StaticResource` Markuperweiterung. In c# können Ressourcen auch definiert werden eine `ResourceDictionary` abgerufen und auf Elemente mithilfe eines Indexers zeichenfolgenbasierte angewendet. Es ist jedoch kaum einen Vorteil mithilfe einer `ResourceDictionary` in C# geschrieben, da freigegebene Objekte einfach werden als Felder oder Eigenschaften gespeichert und können direkt ohne zugegriffen müssen zuerst diese abrufen aus einem Wörterbuch.

## <a name="creating-and-consuming-a-resourcedictionary"></a>Erstellen und nutzen ein: "ResourceDictionary"

Ressourcen werden definiert, einem [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) , legen Sie anschließend auf einen der folgenden `Resources` Eigenschaften:

- Die [ `Resources` ](xref:Xamarin.Forms.Application.Resources) Eigenschaft einer Klasse, die abgeleitet [`Application`](xref:Xamarin.Forms.Application)
- Die [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) Eigenschaft einer Klasse, die abgeleitet [`VisualElement`](xref:Xamarin.Forms.Application)

Ein Programm Xamarin.Forms enthält nur eine Klasse, abgeleitet wird `Application` jedoch häufig nutzt viele abgeleitete Klassen `VisualElement`, einschließlich Seiten, Layouts und Steuerelemente. Eines dieser Objekte aufweisen kann seine `Resources` -Eigenschaftensatz auf eine `ResourceDictionary`. Auswählen eines Speicherorts für eine bestimmte `ResourceDictionary` wirkt sich darauf aus, in dem die Ressourcen verwendet werden können:

- Ressourcen in einem `ResourceDictionary` , z. B. eine Sicht zugeordnet ist `Button` oder `Label` kann nur auf dieses bestimmte Objekt angewendet werden, damit diese nicht sehr hilfreich ist.
- Ressourcen in einem `ResourceDictionary` zu einem Layout angefügt sind, z. B. `StackLayout` oder `Grid` auf das Layout und die untergeordneten Elemente des Layouts angewendet werden können.
- Ressourcen in einem `ResourceDictionary` definiert auf der Seite Ebene auf der Seite und aller diesem untergeordneten Elemente angewendet werden kann.
- Ressourcen in einem `ResourceDictionary` definierten Anwendungs-Ebene in der gesamten Anwendung angewendet werden kann.

Der folgende XAML-Code zeigt die Ressourcen, die in der Anwendungsebene definierten `ResourceDictionary` in der **App.xaml** Datei, die als Teil der Xamarin.Forms Standardprogramm erstellt:

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

Dies `ResourceDictionary` definiert drei [ `Color` ](xref:Xamarin.Forms.Color) Ressourcen und ein [ `Style` ](xref:Xamarin.Forms.Style) Ressource. Weitere Informationen zu den `App` Klasse, finden Sie unter [App-Klasse](~/xamarin-forms/app-fundamentals/application-class.md).

Ab Xamarin.Forms 3.0 wird das explizite `ResourceDictionary` Tags sind nicht erforderlich. Die `ResourceDictionary` Objekt wird automatisch erstellt, und Sie können die Ressourcen direkt zwischen Einfügen der `Resources` Property-Element Tags:

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

Jede Ressource weist einen Schlüssel, der dem angegebenen unter Verwendung der `x:Key` -Attribut, das Wörterbuchschlüssel in wird die `ResourceDictionary`. Der Schlüssel wird zum Abrufen einer Ressource aus der `ResourceDictionary` durch die [ `StaticResource` ](xref:Xamarin.Forms.Xaml.StaticResourceExtension) Markuperweiterung, wie im folgenden Beispiel der Verwendung von XAML-Code veranschaulicht, die zusätzliche Ressourcen, die in definierten zeigt die `StackLayout`:

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

Die erste [ `Label` ](xref:Xamarin.Forms.Label) Instanz abgerufen und verarbeitet die `LabelPageHeadingStyle` Ressource auf Anwendungsebene definierten `ResourceDictionary`, mit der zweiten `Label` -Instanz abrufen und Verarbeiten der `LabelNormalStyle`Ressource in der Steuerelementebene definierten `ResourceDictionary`. Auf ähnliche Weise die [ `Button` ](xref:Xamarin.Forms.Button) Instanz abgerufen und verarbeitet die `NormalTextColor` Ressource auf Anwendungsebene definierten `ResourceDictionary`, und die `MediumBoldText` Ressource in der Steuerelementebene definierten `ResourceDictionary`. Daraus ergibt sich die Darstellung in den folgenden Screenshots dargestellt:

[![](resource-dictionaries-images/screenshots-sml.png ": \"ResourceDictionary\" Ressourcen verbrauchen")](resource-dictionaries-images/screenshots.png#lightbox ": \"ResourceDictionary\" Ressourcen zu verbrauchen.")

> [!NOTE]
> Ressourcen, die auf einer einzelnen Seite beziehen darf nicht in einer Anwendung Ebene Ressourcenwörterbuch, daher enthalten sein, die Ressourcen dann beim Anwendungsstart anstelle von analysiert werden, wenn eine Seite erforderlich. Weitere Informationen finden Sie unter [verringern die Größe der Anwendung Ressource Wörterbuch](~/xamarin-forms/deploy-test/performance.md).

## <a name="overriding-resources"></a>Ressourcen überschreiben

Wenn `ResourceDictionary` Freigeben von Ressourcen `x:Key` Attributwerte, Ressourcen, die niedriger ist definiert in der Hierarchie anzeigen Vorrang vor den definierten höher einrichten. Z. B. die `PageBackgroundColor` Ressource `Blue` Ebene wird an die Anwendung von einer Seitenebene überschrieben werden `PageBackgroundColor` Ressource festgelegt werden, um `Yellow`. Auf ähnliche Weise eine Seitenebene `PageBackgroundColor` Ressource wird durch eine Steuerelementebene überschrieben werden `PageBackgroundColor` Ressource. Diese Priorität wird im folgenden Beispiel der Verwendung von XAML-Code veranschaulicht:

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

[![](resource-dictionaries-images/overridding-screenshots-sml.png "Überschreiben: \"ResourceDictionary\" Ressourcen")](resource-dictionaries-images/overridding-screenshots.png#lightbox ": \"ResourceDictionary\" Ressourcen überschreiben")

Beachten Sie jedoch, dass der Hintergrund der Statusleiste des der [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) weiterhin Gelb ist, da die [ `BarBackgroundColor` ](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor) auf den Wert der Eigenschaft festgelegt ist die `PageBackgroundColor` Ressource in der Anwendung definiert Ebene `ResourceDictionary`.

Dies ist eine andere Methode im Wesentlichen `ResourceDictionary` Rangfolge: bei der Verwendung von XAML-Parser erkennt eine `StaticResource`, es nach einem übereinstimmenden Schlüssel gesucht, indem Wegstrecke einrichten, über die visuelle Struktur, mit der ersten Übereinstimmung findet. Wenn diese Suche auf der Seite beendet, und der Schlüssel können weiterhin wurde nicht gefunden wurde, sucht der XAML-Parser die `ResourceDictionary` angefügt, um die `App` Objekt. Wenn der Schlüssel nicht gefunden wird, wird eine Ausnahme ausgelöst.

## <a name="stand-alone-resource-dictionaries"></a>Eigenständige Ressourcenverzeichnis

Eine abgeleitete Klasse `ResourceDictionary` kann auch in einer separaten eigenständige Datei sein. (Genauer gesagt, einer abgeleiteten Klasse `ResourceDictionary` in der Regel erfordert eine _Paar_ von Dateien, da die Ressourcen in einer XAML-Datei jedoch einen Code-Behind-Datei mit definiert sind ein `InitializeComponent` Aufruf ist auch erforderlich.) Die resultierende Datei kann dann zwischen Anwendungen freigegeben werden.

Um eine solche Datei zu erstellen, fügen Sie einen neuen **Inhaltsansicht** oder **Inhaltsseite** Element aus, um das Projekt (aber keine **Inhaltsansicht** oder **Inhaltsseite** mit nur eine C#-Datei). Ändern Sie im XAML-Datei und in C#-Datei den Namen der Basisklasse aus `ContentView` oder `ContentPage` auf `ResourceDictionary`. In der XAML-Datei ist der Name der Basisklasse Element der obersten Ebene.

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

Instanziieren Sie `MyResourceDictionary` verlegen zwischen einem Knotenpaar `Resources` Property-Element tags, z. B. einem `ContentPage`:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <local:MyResourceDictionary />
    </ContentPage.Resources>
    ...
</ContentPage>
```

Eine Instanz von `MyResourceDictionary` festgelegt ist, um die `Resources` Eigenschaft der `ContentPage` Objekt.

Dieser Ansatz hat jedoch einige Einschränkungen: die `Resources` Eigenschaft von der `ContentPage` verweist auf nur diese eine `ResourceDictionary`. In den meisten Fällen möchten Sie die Möglichkeit, einschließlich anderer `ResourceDictionary` Instanzen und vielleicht andere Ressourcen ebenfalls.

Diese Aufgabe erfordert zusammengeführter Ressourcenwörterbücher.

## <a name="merged-resource-dictionaries"></a>Zusammengeführte Ressourcenwörterbücher

Zusammengeführter Ressourcenwörterbücher kombinieren, eine oder mehrere `ResourceDictionary` Instanzen in eine andere `ResourceDictionary`. Hierzu können Sie in einer XAML-Datei durch Festlegen der [ `MergedDictionaries` ](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries) Eigenschaft, um eine oder mehrere Ressourcenverzeichnis, die in der Anwendung, die Seite oder die Steuerungsebene zusammengeführt werden `ResourceDictionary`.

> [!IMPORTANT]
> `ResourceDictionary` definiert auch einen [ `MergedWith` ](xref:Xamarin.Forms.ResourceDictionary.MergedWith) Eigenschaft. Verwenden Sie diese Eigenschaft nicht. Es ist zum Zeitpunkt der Xamarin.Forms 3.0 veraltet.

Und die Instanz des `MyResourceDictionary` können in jeder Anwendung, die Seite oder die Steuerungsebene zusammengeführt werden `ResourceDictionary`. Im folgenden Codebeispiel wird für XAML wird eine Seitenebene zusammengeführt wird `ResourceDictionary` mithilfe der `MergedDictionaries` Eigenschaft:

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

Das Markup zeigt nur eine Instanz von `MyResourceDictionary` hinzugefügte der `ResourceDictionary` , aber Sie können auch andere verweisen `ResourceDictionary` Instanzen innerhalb der `MergedDictionary` Property-Element Tags und andere Ressourcen außerhalb dieser Tags:

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

Es kann nur eine `MergedDictionaries` im Abschnitt eine `ResourceDictionary`, aber Sie können beliebig viele ablegen `ResourceDictionary` Instanzen vorhanden, wie gewünscht.

Beim Zusammenführen der [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) Freigeben von Ressourcen identisch `x:Key` Attributwerte Xamarin.Forms verwendet die folgenden Ressourcen Vorrang vor:

1. Die Ressourcen, die lokal auf das Ressourcenwörterbuch.
1. Die Ressourcen in das Ressourcenwörterbuch, die zusammengeführt wurde über das veraltete [ `MergedWith` ](xref:Xamarin.Forms.ResourceDictionary.MergedWith) Eigenschaft.
1. Die Ressourcen in die Ressourcenwörterbücher, die über zusammengeführt wurden die `MergedDictionaries` Auflistung, in der sie, in aufgelistet sind Reihenfolge der `MergedDictionaries` Eigenschaft.

> [!NOTE]
> Ressourcenverzeichnis suchen kann einen rechenintensiven Vorgang sein, wenn eine Anwendung mehrere enthält große Ressourcenwörterbücher. Vermeiden Sie unnötige Suchläufe, sollten Sie daher sicherstellen, dass jede Seite in einer Anwendung nur Ressourcenverzeichnis verwendet, die auf der Seite "geeignet sind.

## <a name="merging-dictionaries-in-xamarinforms-30"></a>Das Zusammenführen von Wörterbüchern in Xamarin.Forms 3.0

Beginnend mit Xamarin.Forms 3.0, den Prozess der Zusammenführung von [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) Instanzen geworden ist etwas einfacher und flexibler. Die `MergedDictionaries` Property-Element Tags sind nicht mehr erforderlich. Stattdessen Sie auf das Ressourcenwörterbuch Weitere hinzufügen `ResourceDictionary` Tag mit dem neuen [ `Source` ](xref:Xamarin.Forms.ResourceDictionary.Source) -Eigenschaft auf den Dateinamen der Verwendung von XAML-Datei mit den Ressourcen festgelegt:

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

Da Xamarin.Forms 3.0 automatisch instanziiert die `ResourceDictionary`, diese beiden äußeren `ResourceDictionary` Tags nicht mehr erforderlich sind:

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

In diesem Artikel wird erläutert, wie erstellen und nutzen einen [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), und wie Ressourcenverzeichnis zusammengeführt. Ein `ResourceDictionary` können Ressourcen in einem zentralen Ort definiert und wieder in der gesamten einer Xamarin.Forms-Anwendung verwendet werden.

## <a name="related-links"></a>Verwandte Links

- [Ressourcenverzeichnis (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/xaml/resourcedictionaries/)
- [Stile](~/xamarin-forms/user-interface/styles/index.md)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
