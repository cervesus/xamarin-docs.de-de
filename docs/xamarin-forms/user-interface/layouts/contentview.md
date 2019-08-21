---
title: Xamarin. Forms-contentview
description: In diesem Artikel wird erläutert, wie Sie mit der contentview-Klasse ein benutzerdefiniertes Steuerelement erstellen, wie z. b. CardView.
ms.prod: xamarin
ms.assetid: 638402E7-CA44-456B-863B-791F6B6B561D
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 08/14/2019
ms.openlocfilehash: e340b45148c7528eff1aa511ee9902a4ac2658c0
ms.sourcegitcommit: 9178e2e689f027212ea3e623b556b312985d79fe
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2019
ms.locfileid: "69658157"
---
# <a name="xamarinforms-contentview"></a>Xamarin. Forms-contentview

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/en-us/samples/xamarin/xamarin-forms-samples/userinterface-cardview/)

Die xamarin. Forms [`ContentView`](xref:Xamarin.Forms.ContentView) -Klasse ist ein Typ `Layout` von, der ein einzelnes untergeordnetes Element enthält und normalerweise verwendet wird, um benutzerdefinierte, wiederverwendbare Steuerelemente zu erstellen. Die `ContentView` Klasse erbt von [`TemplatedView`](xref:Xamarin.Forms.TemplatedView). In diesem Artikel und dem zugehörigen Beispiel wird erläutert, wie ein Benutzer `CardView` definiertes Steuerelement `ContentView` auf Grundlage der-Klasse erstellt wird.

Der folgende Screenshot zeigt ein `CardView` Steuerelement, das von `ContentView` der-Klasse abgeleitet ist:

[![Bildschirm Abbildung der CardView-Beispielanwendung](contentview-images/cardview-list-cropped.png)](contentview-images/cardview-list.png#lightbox)

Die `ContentView` -Klasse definiert eine einzelne Eigenschaft:

* [`Content`](xref:Xamarin.Forms.ContentView.Content)ist ein `View` -Objekt. Diese Eigenschaft wird von einem [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekt unterstützt, sodass Sie das Ziel von Daten Bindungen sein kann.

Die `ContentView` erbt auch eine-Eigenschaft von `TemplatedView` der-Klasse:

* [`ControlTemplate`](xref:Xamarin.Forms.TemplatedView.ControlTemplate)ist ein `ControlTemplate` , mit dem die Darstellung des Steuer Elements definiert oder überschrieben werden kann.

Weitere Informationen zur-Eigenschaft `ControlTemplate` finden Sie unter [Anpassen der Darstellung mit einer ControlTemplate](#customize-appearance-with-a-controltemplate).

## <a name="create-a-custom-control"></a>Erstellen eines benutzerdefinierten Steuer Elements

Die `ContentView` -Klasse bietet nur wenige Funktionen, kann jedoch verwendet werden, um ein benutzerdefiniertes Steuerelement zu erstellen. Das Beispiel Projekt definiert ein `CardView` Steuerelement-ein UI-Element, das ein Bild, einen Titel und eine Beschreibung in einem Karten ähnlichen Layout anzeigt.

Der Prozess zum Erstellen eines benutzerdefinierten Steuer Elements lautet:

1. Erstellen Sie eine neue Klasse mithilfe `ContentView` der Vorlage in Visual Studio 2019.
1. Definieren Sie eindeutige Eigenschaften oder Ereignisse in der Code-Behind-Datei für das neue benutzerdefinierte Steuerelement.
1. Erstellen Sie die Benutzeroberfläche für das benutzerdefinierte Steuerelement.

> [!NOTE]
> Es ist möglich, ein benutzerdefiniertes Steuerelement zu erstellen, dessen Layout im Code anstelle von XAML definiert ist. Der Einfachheit halber definiert die Beispielanwendung nur eine einzelne `CardView` Klasse mit einem XAML-Layout. Die Beispielanwendung enthält jedoch eine **cardviewcodepage** -Klasse, die den Prozess der Verwendung des benutzerdefinierten Steuer Elements im Code anzeigt.

### <a name="create-code-behind-properties"></a>Code Behind-Eigenschaften erstellen

Das `CardView` benutzerdefinierte Steuerelement definiert die folgenden Eigenschaften:

* `CardTitle`: ein `string` -Objekt, das den auf der Karte angezeigten Titel darstellt.
* `CardDescription`: ein `string` -Objekt, das die auf der Karte angezeigte Beschreibung darstellt.
* `IconImageSource`: ein `ImageSource` Objekt, das das Bild darstellt, das auf der Karte angezeigt wird.
* `IconBackgroundColor`: ein `Color` -Objekt, das die Hintergrundfarbe für das Bild darstellt, das auf der Karte angezeigt wird.
* `BorderColor`: ein `Color` -Objekt, das die Farbe des Karten Rahmens, des Bildrands und der Trennlinie darstellt.
* `CardColor`: ein `Color` -Objekt, das die Hintergrundfarbe der Karte darstellt.

> [!NOTE]
> Die `BorderColor` -Eigenschaft wirkt sich auf mehrere-Elemente aus, um Sie zu demonstrieren. Diese Eigenschaft könnte bei Bedarf in drei Eigenschaften aufgeteilt werden.

Jede Eigenschaft wird von einer `BindableProperty` -Instanz unterstützt. Durch die `BindableProperty` Unterstützung kann jede Eigenschaft mithilfe des MVVM-Musters formatiert und gebunden werden. Weitere Informationen finden Sie unter [Binden von Daten mit MVVM](#bind-data-with-mvvm).

Im folgenden Beispiel wird gezeigt, wie Sie eine `BindableProperty`Sicherung erstellen:

```csharp
public static readonly BindableProperty CardTitleProperty = BindableProperty.Create(
    "CardTitle",        // the name of the bindable property
    typeof(string),     // the bindable property type
    typeof(CardView),   // the parent object type
    string.Empty);      // the default value for the property
```

Die benutzerdefinierte Eigenschaft verwendet `GetValue` die `SetValue` -und-Methoden, um `BindableProperty` die Objektwerte zu erhalten und festzulegen:

```csharp
public string CardTitle
{
    get => (string)GetValue(CardView.CardTitleProperty);
    set => SetValue(CardView.CardTitleProperty, value);
}
```

Weitere Informationen zu `BindableProperty` Objekten finden Sie unter [bindbare Eigenschaften](~/xamarin-forms/xaml/bindable-properties.md).

### <a name="define-ui"></a>Definieren der Benutzeroberfläche

Die benutzerdefinierte Steuerelement- `ContentView` Benutzeroberfläche verwendet einen als Stamm `CardView` Element für das-Steuerelement. Das folgende Beispiel zeigt das `CardView` XAML:

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
            <Frame BorderColor="{Binding BorderColor}"
                   BackgroundColor="{Binding IconBackgroundColor}"
                   ...>
                <Image Source="{Binding IconImageSource}"
                       .. />
            </Frame>
            <Label Text="{Binding CardTitle}"
                   ... />
            <BoxView BackgroundColor="{Binding BorderColor}"
                     ... />
            <Label Text="{Binding CardDescription}"
                   ... />
        </Grid>
    </Frame>
</ContentView>
```

Das `ContentView` -Element legt `x:Name` die-Eigenschaft auf **diese**fest, die verwendet werden kann, um auf das `CardView` Objekt zuzugreifen, das an die Instanz gebunden ist. Elemente im Layout legen Bindungen an ihren Eigenschaften auf Werte fest, die für das gebundene Objekt definiert sind.

Weitere Informationen zur Datenbindung finden Sie unter [Xamarin.Forms-Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md).

## <a name="instantiate-a-custom-control"></a>Instanziieren eines benutzerdefinierten Steuer Elements

Ein Verweis auf den Namespace des benutzerdefinierten Steuer Elements muss einer Seite hinzugefügt werden, die das benutzerdefinierte Steuerelement instanziiert. Das folgende Beispiel zeigt einen Namespace Verweis namens Steuer **Elemente** , die `ContentPage` einer-Instanz in XAML hinzugefügt werden:

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:CardViewDemo.Controls" >
```

Nachdem der Verweis hinzugefügt `CardView` wurde, kann in XAML instanziiert und seine Eigenschaften definiert werden:

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

### <a name="bind-data-with-mvvm"></a>Binden von Daten mit MVVM

Die `BindableProperty` -Objekte in `CardView` der-Klasse lassen Model-View-ViewModel (MVVM)-Stil Bindungen zu. Die Beispielanwendung enthält eine `PersonCollectionViewModel` -Klasse, die eine einzelne Auflistungs Eigenschaft definiert:

```csharp
public class PersonCollectionViewModel : INotifyPropertyChanged
{
    ...
    public List<PersonViewModel> Items
    {
        get
        {
            return items;
        }
        set
        {
            items = value;
            NotifyPropertyChanged();
        }
    }
    ...
}
```

Die `PersonViewModel` -Klasse stellt ein persönliches Profil dar:

```csharp
public class PersonViewModel : INotifyPropertyChanged
{
    ...

    string photo;
    public string Photo
    {
        get
        {
            return photo;
        }
        set
        {
            photo = value;
            NotifyPropertyChanged();
        }
    }

    string name;
    public string Name
    {
        get
        {
            return name;
        }
        set
        {
            name = value;
            NotifyPropertyChanged();
        }
    }

    string bio;
    public string Bio
    {
        get
        {
            return bio;
        }
        set
        {
            bio = value;
            NotifyPropertyChanged();
        }
    }
    ...
}
```

Kann verwendet werden, um die Auflistung von `PersonViewModel` -Objekten als eine Liste von Karten zu Rendering. `CardView` Im folgenden Beispiel wird gezeigt, wie eine `PersonViewCollection` -Instanz an `StackLayout` eine-Instanz in XAML gebunden wird:

```xaml
<StackLayout HorizontalOptions="Fill"
             VerticalOptions="Fill"
             BindableLayout.ItemsSource="{Binding Items}">
    <BindableLayout.ItemTemplate>
        <DataTemplate>
            <controls:CardView Margin="4"
                               BorderColor="DarkGray"
                               IconBackgroundColor="SlateGray"
                               BindingContext="{Binding .}"
                               CardTitle="{Binding Name}"
                               CardDescription="{Binding Bio}"
                               IconImageSource="{Binding Photo}"/>
        </DataTemplate>
    </BindableLayout.ItemTemplate>
</StackLayout>
```

Die `Items` -Eigenschaft für `PersonViewCollection` eine- `StackLayout` Instanz wird mithilfe eines bindbaren Layouts an gebunden. Definiert die Darstellung der einzelnen `CardView` -Objekte, und die Daten werden an Eigenschaften in `PersonViewModel`einer gebunden. `DataTemplate` Wenn festgelegt `CardView` `PersonView` `Items` ist, wird für jedes Objekt in der Auflistung ein-Objekt erstellt. `BindingContext` Der `BindingContext` wird wie im folgenden Beispiel gezeigt festgelegt:

```csharp
public partial class CardViewMvvmPage : ContentPage
{
    public CardViewMvvmPage()
    {
        InitializeComponent();
        BindingContext = DataService.GetPersonCollection();
    }
}
```

Weitere Informationen zur Datenbindung finden Sie unter [xamarin. Forms-Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md). Weitere Informationen zu `BindableProperty` Objekten finden Sie unter [bindbare Eigenschaften](~/xamarin-forms/xaml/bindable-properties.md).

## <a name="customize-appearance-with-a-controltemplate"></a>Anpassen des Erscheinungs Bilds mit einer ControlTemplate

Ein benutzerdefiniertes Steuerelement, `ContentView` das von der-Klasse abgeleitet wird, kann die Darstellung mithilfe von XAML oder Code definieren, oder es kann überhaupt keine Darstellung definieren. Unabhängig davon, wie das Erscheinungsbild definiert `ControlTemplate` ist, kann ein-Objekt die Darstellung mit einem benutzerdefinierten Layout überschreiben.

Das `CardView` Layout kann für einige Anwendungsfälle zu viel Speicherplatz belegen. Ein `ControlTemplate` kann das Layout `CardView` überschreiben, um eine kompaktere Ansicht bereitzustellen, die für eine komprimierte Liste geeignet ist:

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

Die Datenbindung in `ControlTemplate` einem verwendet `TemplateBinding` die Markup Erweiterung zum Angeben von Bindungen. Die `ControlTemplate` -Eigenschaft kann dann mithilfe Ihres `x:Key` Werts auf das definierte ControlTemplate-Objekt festgelegt werden. Im folgenden Beispiel wird die `ControlTemplate` -Eigenschaft auf einer `CardView` -Instanz festgelegt:

```xaml
<controls:CardView ControlTemplate="{StaticResource CardViewCompressed}"/>
```

Die folgenden Screenshots zeigen eine Standard `CardView` `CardView` Instanz, deren `ControlTemplate` überschrieben wurde:

[![Bildschirm Abbildung von CardView ControlTemplate](contentview-images/cardview-controltemplates-cropped.png)](contentview-images/cardview-controltemplates.png#lightbox)

Weitere Informationen zu Steuerelement Vorlagen finden Sie unter [xamarin. Forms-Steuerelement Vorlagen](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md).

## <a name="related-links"></a>Verwandte Links

* [CardView-Beispielanwendung](https://docs.microsoft.com/en-us/samples/xamarin/xamarin-forms-samples/userinterface-cardview/)
* [Xamarin. Forms-Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md)
* [Bindbare Eigenschaften](~/xamarin-forms/xaml/bindable-properties.md).
* [Xamarin. Forms-Steuerelement Vorlagen](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md)
