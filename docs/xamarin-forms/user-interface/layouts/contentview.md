---
title: Xamarin.Forms-ContentView
description: In diesem Artikel wird erläutert, wie Sie mit der contentview-Klasse ein benutzerdefiniertes Steuerelement erstellen, wie z. b. CardView.
ms.prod: xamarin
ms.assetid: 638402E7-CA44-456B-863B-791F6B6B561D
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 08/14/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 46d2abf895ffe31bd1dc1c22caf36440c54b331c
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84130116"
---
# <a name="xamarinforms-contentview"></a>Xamarin.Forms-ContentView

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-contentviewdemos/)

Die Xamarin.Forms [`ContentView`](xref:Xamarin.Forms.ContentView) -Klasse ist ein Typ von `Layout` , der ein einzelnes untergeordnetes Element enthält und in der Regel zum Erstellen benutzerdefinierter, wiederverwendbarer Steuerelemente verwendet wird. Die `ContentView` Klasse erbt von [`TemplatedView`](xref:Xamarin.Forms.TemplatedView) . In diesem Artikel und dem zugehörigen Beispiel wird erläutert, wie ein benutzerdefiniertes Steuerelement auf `CardView` Grundlage der-Klasse erstellt wird `ContentView` .

Der folgende Screenshot zeigt ein `CardView` Steuerelement, das von der-Klasse abgeleitet ist `ContentView` :

[![Bildschirm Abbildung der CardView-Beispielanwendung](contentview-images/cardview-list-cropped.png)](contentview-images/cardview-list.png#lightbox)

Die- `ContentView` Klasse definiert eine einzelne Eigenschaft:

* [`Content`](xref:Xamarin.Forms.ContentView.Content)ist ein- `View` Objekt. Diese Eigenschaft wird von einem-Objekt unterstützt, [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) sodass Sie das Ziel von Daten Bindungen sein kann.

Die `ContentView` erbt auch eine-Eigenschaft von der- `TemplatedView` Klasse:

* [`ControlTemplate`](xref:Xamarin.Forms.TemplatedView.ControlTemplate)ist ein `ControlTemplate` , mit dem die Darstellung des Steuer Elements definiert oder überschrieben werden kann.

Weitere Informationen zur-Eigenschaft finden Sie unter Anpassen der Darstellung `ControlTemplate` [mit einer ControlTemplate](#customize-appearance-with-a-controltemplate).

## <a name="create-a-custom-control"></a>Erstellen eines benutzerdefinierten Steuer Elements

Die- `ContentView` Klasse bietet nur wenige Funktionen, kann jedoch verwendet werden, um ein benutzerdefiniertes Steuerelement zu erstellen. Das Beispiel Projekt definiert ein `CardView` Steuerelement-ein UI-Element, das ein Bild, einen Titel und eine Beschreibung in einem Karten ähnlichen Layout anzeigt.

Der Prozess zum Erstellen eines benutzerdefinierten Steuer Elements lautet:

1. Erstellen Sie eine neue Klasse mithilfe der `ContentView` Vorlage in Visual Studio 2019.
1. Definieren Sie eindeutige Eigenschaften oder Ereignisse in der Code-Behind-Datei für das neue benutzerdefinierte Steuerelement.
1. Erstellen Sie die Benutzeroberfläche für das benutzerdefinierte Steuerelement.

> [!NOTE]
> Es ist möglich, ein benutzerdefiniertes Steuerelement zu erstellen, dessen Layout im Code anstelle von XAML definiert ist. Der Einfachheit halber definiert die Beispielanwendung nur eine einzelne `CardView` Klasse mit einem XAML-Layout. Die Beispielanwendung enthält jedoch eine **cardviewcodepage** -Klasse, die den Prozess der Verwendung des benutzerdefinierten Steuer Elements im Code anzeigt.

### <a name="create-code-behind-properties"></a>Code Behind-Eigenschaften erstellen

Das `CardView` benutzerdefinierte Steuerelement definiert die folgenden Eigenschaften:

* `CardTitle`: ein- `string` Objekt, das den auf der Karte angezeigten Titel darstellt.
* `CardDescription`: ein- `string` Objekt, das die auf der Karte angezeigte Beschreibung darstellt.
* `IconImageSource`: ein `ImageSource` Objekt, das das Bild darstellt, das auf der Karte angezeigt wird.
* `IconBackgroundColor`: ein- `Color` Objekt, das die Hintergrundfarbe für das Bild darstellt, das auf der Karte angezeigt wird.
* `BorderColor`: ein- `Color` Objekt, das die Farbe des Karten Rahmens, des Bildrands und der Trennlinie darstellt.
* `CardColor`: ein- `Color` Objekt, das die Hintergrundfarbe der Karte darstellt.

> [!NOTE]
> Die- `BorderColor` Eigenschaft wirkt sich auf mehrere-Elemente aus, um Sie zu demonstrieren. Diese Eigenschaft könnte bei Bedarf in drei Eigenschaften aufgeteilt werden.

Jede Eigenschaft wird von einer- `BindableProperty` Instanz unterstützt. Durch die Unterstützung `BindableProperty` kann jede Eigenschaft mithilfe des MVVM-Musters formatiert und gebunden werden.

Im folgenden Beispiel wird gezeigt, wie Sie eine Sicherung erstellen `BindableProperty` :

```csharp
public static readonly BindableProperty CardTitleProperty = BindableProperty.Create(
    "CardTitle",        // the name of the bindable property
    typeof(string),     // the bindable property type
    typeof(CardView),   // the parent object type
    string.Empty);      // the default value for the property
```

Die benutzerdefinierte Eigenschaft verwendet die `GetValue` -und- `SetValue` Methoden, um die Objektwerte zu erhalten und festzulegen `BindableProperty` :

```csharp
public string CardTitle
{
    get => (string)GetValue(CardView.CardTitleProperty);
    set => SetValue(CardView.CardTitleProperty, value);
}
```

Weitere Informationen zu `BindableProperty` Objekten finden Sie unter [bindbare Eigenschaften](~/xamarin-forms/xaml/bindable-properties.md).

### <a name="define-ui"></a>Definieren der Benutzeroberfläche

Die benutzerdefinierte Steuerelement-Benutzeroberfläche verwendet einen `ContentView` als Stamm Element für das- `CardView` Steuerelement. Das folgende Beispiel zeigt das `CardView` XAML:

```XAML
<ContentView ...
             x:Name="this"
             x:Class="CardViewDemo.Controls.CardView">
    <Frame BindingContext="{x:Reference this}"
            BackgroundColor="{Binding CardColor}"
            BorderColor="{Binding BorderColor}"
            ...>
        <Grid>
            ...
            <Frame BorderColor="{Binding BorderColor, FallbackValue='Black'}"
                   BackgroundColor="{Binding IconBackgroundColor, FallbackValue='Grey'}"
                   ...>
                <Image Source="{Binding IconImageSource}"
                       .. />
            </Frame>
            <Label Text="{Binding CardTitle, FallbackValue='Card Title'}"
                   ... />
            <BoxView BackgroundColor="{Binding BorderColor, FallbackValue='Black'}"
                     ... />
            <Label Text="{Binding CardDescription, FallbackValue='Card description text.'}"
                   ... />
        </Grid>
    </Frame>
</ContentView>
```

Das- `ContentView` Element legt die- `x:Name` Eigenschaft auf **diese**fest, die verwendet werden kann, um auf das Objekt zuzugreifen, das an die Instanz gebunden ist `CardView` . Elemente im Layout legen Bindungen an ihren Eigenschaften auf Werte fest, die für das gebundene Objekt definiert sind.

Weitere Informationen zur Datenbindung finden Sie unter [Xamarin.Forms-Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md).

> [!NOTE]
> Die- `FallbackValue` Eigenschaft stellt einen Standardwert für den Fall bereit, dass die Bindung ist `null` . Dadurch kann der [XAML-Previewer](~/xamarin-forms/xaml/xaml-previewer/index.md) in Visual Studio das Steuerelement auch Rendering `CardView` .

## <a name="instantiate-a-custom-control"></a>Instanziieren eines benutzerdefinierten Steuer Elements

Ein Verweis auf den Namespace des benutzerdefinierten Steuer Elements muss einer Seite hinzugefügt werden, die das benutzerdefinierte Steuerelement instanziiert. Das folgende Beispiel zeigt einen Namespace Verweis namens Steuer **Elemente** , die einer- `ContentPage` Instanz in XAML hinzugefügt werden:

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:CardViewDemo.Controls" >
```

Nachdem der Verweis hinzugefügt wurde `CardView` , kann in XAML instanziiert und seine Eigenschaften definiert werden:

```xaml
<controls:CardView BorderColor="DarkGray"
                   CardTitle="Slavko Vlasic"
                   CardDescription="Lorem ipsum dolor sit..."
                   IconBackgroundColor="SlateGray"
                   IconImageSource="user.png"/>
```

Eine `CardView` kann auch im Code instanziiert werden:

```csharp
CardView card = new CardView
{
    BorderColor = Color.DarkGray,
    CardTitle = "Slavko Vlasic",
    CardDescription = "Lorem ipsum dolor sit...",
    IconBackgroundColor = Color.SlateGray,
    IconImageSource = ImageSource.FromFile("user.png")
};
```

## <a name="customize-appearance-with-a-controltemplate"></a>Anpassen des Erscheinungs Bilds mit einer ControlTemplate

Ein benutzerdefiniertes Steuerelement, das von der-Klasse abgeleitet wird, kann die Darstellung `ContentView` mithilfe von XAML oder Code definieren, oder es kann überhaupt keine Darstellung definieren. Unabhängig davon, wie das Erscheinungsbild definiert ist, kann ein- `ControlTemplate` Objekt die Darstellung mit einem benutzerdefinierten Layout überschreiben.

Das `CardView` Layout kann für einige Anwendungsfälle zu viel Speicherplatz belegen. Ein `ControlTemplate` kann das `CardView` Layout überschreiben, um eine kompaktere Ansicht bereitzustellen, die für eine komprimierte Liste geeignet ist:

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <ControlTemplate x:Key="CardViewCompressed">
            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="100" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="100" />
                    <ColumnDefinition Width="100*" />
                </Grid.ColumnDefinitions>
                <Image Grid.Row="0"
                       Grid.Column="0"
                       Source="{TemplateBinding IconImageSource}"
                       BackgroundColor="{TemplateBinding IconBackgroundColor}"
                       WidthRequest="100"
                       HeightRequest="100"
                       Aspect="AspectFill"
                       HorizontalOptions="Center"
                       VerticalOptions="Center"/>
                <StackLayout Grid.Row="0"
                             Grid.Column="1">
                    <Label Text="{TemplateBinding CardTitle}"
                           FontAttributes="Bold" />
                    <Label Text="{TemplateBinding CardDescription}" />
                </StackLayout>
            </Grid>
        </ControlTemplate>
    </ResourceDictionary>
</ContentPage.Resources>
```

Die Datenbindung in einem `ControlTemplate` verwendet die `TemplateBinding` Markup Erweiterung zum Angeben von Bindungen. Die- `ControlTemplate` Eigenschaft kann dann mithilfe ihres Werts auf das definierte ControlTemplate-Objekt festgelegt werden `x:Key` . Im folgenden Beispiel wird die- `ControlTemplate` Eigenschaft auf einer-Instanz festgelegt `CardView` :

```xaml
<controls:CardView ControlTemplate="{StaticResource CardViewCompressed}"/>
```

Die folgenden Screenshots zeigen eine Standard `CardView` Instanz, `CardView` deren `ControlTemplate` überschrieben wurde:

[![Bildschirm Abbildung von CardView ControlTemplate](contentview-images/cardview-controltemplates-cropped.png)](contentview-images/cardview-controltemplates.png#lightbox)

Weitere Informationen zu Steuerelementvorlagen finden Sie unter [Xamarin.Forms-Steuerelementvorlagen](~/xamarin-forms/app-fundamentals/templates/control-template.md).

## <a name="related-links"></a>Verwandte Links

* [Contentview-Beispielanwendung](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-contentviewdemos/)
* [Xamarin.Forms-Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md)
* [Bindbare Eigenschaften](~/xamarin-forms/xaml/bindable-properties.md).
* [Xamarin.Forms-Steuerelementvorlagen](~/xamarin-forms/app-fundamentals/templates/control-template.md)
