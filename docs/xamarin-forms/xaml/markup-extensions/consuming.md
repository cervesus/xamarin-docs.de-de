---
title: Verwenden von XAML-Markuperweiterungen
description: In diesem Artikel wird erläutert, wie xamarin. Forms-XAML-Markup Erweiterungen verwendet werden, um die Leistungsfähigkeit und Flexibilität von XAML zu verbessern, indem Element Attribute aus einer Vielzahl von Quellen festgelegt werden können.
ms.prod: xamarin
ms.assetid: CE686893-609C-4EC3-9225-6C68D2A9F79C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/27/2019
ms.openlocfilehash: a8698975d2609599e1404fbb9c87c617a54f23d7
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72696345"
---
# <a name="consuming-xaml-markup-extensions"></a>Verwenden von XAML-Markuperweiterungen

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)

XAML-Markup Erweiterungen helfen, die Leistungsfähigkeit und Flexibilität von XAML zu verbessern, indem die Festlegung von Element Attributen aus einer Vielzahl von Quellen ermöglicht wird. Mehrere XAML-Markup Erweiterungen sind Teil der XAML 2009-Spezifikation. Diese werden in XAML-Dateien mit dem üblichen `x`-Namespace Präfix angezeigt und werden häufig mit diesem Präfix bezeichnet. In diesem Artikel werden die folgenden Markup Erweiterungen erläutert:

- [`x:Static`](#static) – auf statische Eigenschaften, Felder oder Enumerationsmember verweisen.
- [`x:Reference`](#reference) – Verweis benannte Elemente auf der Seite.
- [`x:Type`](#type) – legen Sie ein Attribut auf ein `System.Type` Objekt fest.
- [`x:Array`](#array) – erstellen Sie ein Array von Objekten eines bestimmten Typs.
- [`x:Null`](#null) – legen Sie ein Attribut auf einen `null` Wert fest.
- [`OnPlatform`](#onplatform) – Anpassen der Benutzeroberflächen Darstellung pro Plattform.
- [`OnIdiom`](#onidiom) – Anpassen der Darstellung der Benutzeroberfläche basierend auf dem Erscheinungsbild des Geräts, auf dem die Anwendung ausgeführt wird.
- [`DataTemplate`](#datatemplate-markup-extension) : konvertiert einen Typ in einen [`DataTemplate`](xref:Xamarin.Forms.DataTemplate).
- [`FontImage`](#fontimage-markup-extension) : zeigt ein Schriftart Symbol in einer beliebigen Ansicht an, die eine `ImageSource` anzeigen kann.

Weitere XAML-Markup Erweiterungen wurden in der Vergangenheit von anderen XAML-Implementierungen unterstützt und werden auch von xamarin. Forms unterstützt. Diese werden in anderen Artikeln ausführlicher beschrieben:

- `StaticResource` Verweis Objekte aus einem Ressourcen Wörterbuch, wie im Artikel [**Ressourcen Wörterbücher**](~/xamarin-forms/xaml/resource-dictionaries.md)beschrieben.
- `DynamicResource`: reagieren auf Änderungen in Objekten in einem Ressourcen Wörterbuch, wie im Artikel [**dynamische Stile**](~/xamarin-forms/user-interface/styles/dynamic.md)beschrieben.
- `Binding`: Erstellen Sie eine Verknüpfung zwischen Eigenschaften von zwei-Objekten, wie im Artikel [**Datenbindung**](~/xamarin-forms/app-fundamentals/data-binding/index.md)beschrieben.
- `TemplateBinding`: führt die Datenbindung aus einer Steuerelement Vorlage aus, wie im Artikel [**binden aus einer Steuerelement Vorlage**](~/xamarin-forms/app-fundamentals/templates/control-templates/template-binding.md)erläutert.
- legt `RelativeSource` die Bindungs Quelle relativ zur Position des Bindungs Ziels fest, wie im Artikel [relative Bindungen](~/xamarin-forms/app-fundamentals/data-binding/relative-bindings.md)erläutert.

Das [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) Layout verwendet die benutzerdefinierte Markup Erweiterung [`ConstraintExpression`](xref:Xamarin.Forms.ConstraintExpression). Diese Markup Erweiterung wird im Artikel [**relativelayout**](~/xamarin-forms/user-interface/layouts/relative-layout.md)beschrieben.

<a name="static" />

## <a name="xstatic-markup-extension"></a>x:Static-Markuperweiterung

Die `x:Static` Markup Erweiterung wird von der [`StaticExtension`](xref:Xamarin.Forms.Xaml.StaticExtension) -Klasse unterstützt. Die-Klasse verfügt über eine einzelne Eigenschaft mit dem Namen [`Member`](xref:Xamarin.Forms.Xaml.StaticExtension.Member) vom Typ `string`, die Sie auf den Namen einer öffentlichen Konstante, einer statischen Eigenschaft, eines statischen Felds oder eines Enumerationsmembers festlegen.

Eine gängige Methode, `x:Static` zu verwenden, besteht darin, zuerst eine Klasse mit einigen Konstanten oder statischen Variablen zu definieren, z. b. diese kleine `AppConstants` Klasse im [**MarkupExtensions**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions) -Programm:

```csharp
static class AppConstants
{
    public static double NormalFontSize = 18;
}
```

Die **x:static-Demoseite** zeigt verschiedene Möglichkeiten, die `x:Static` Markup Erweiterung zu verwenden. Der ausführlichste Ansatz instanziiert die `StaticExtension` Klasse zwischen `Label.FontSize` Eigenschaften-Element-Tags:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sys="clr-namespace:System;assembly=mscorlib"
             xmlns:local="clr-namespace:MarkupExtensions"
             x:Class="MarkupExtensions.StaticDemoPage"
             Title="x:Static Demo">
    <StackLayout Margin="10, 0">
        <Label Text="Label No. 1">
            <Label.FontSize>
                <x:StaticExtension Member="local:AppConstants.NormalFontSize" />
            </Label.FontSize>
        </Label>

        ···

    </StackLayout>
</ContentPage>
```

Der XAML-Parser ermöglicht außerdem, dass die `StaticExtension` Klasse als `x:Static` abgekürzt wird:

```xaml
<Label Text="Label No. 2">
    <Label.FontSize>
        <x:Static Member="local:AppConstants.NormalFontSize" />
    </Label.FontSize>
</Label>
```

Dies kann noch weiter vereinfacht werden, aber die Änderung führt zu einer neuen Syntax: Sie besteht aus dem Platzieren der `StaticExtension` Klasse und der Element Einstellung in geschweiften Klammern. Der resultierende Ausdruck wird direkt auf das `FontSize` Attribut festgelegt:

```xaml
<Label Text="Label No. 3"
       FontSize="{x:StaticExtension Member=local:AppConstants.NormalFontSize}" />
```

Beachten Sie, dass es *keine* Anführungszeichen innerhalb der geschweiften Klammern gibt. Die `Member`-Eigenschaft von `StaticExtension` ist nicht mehr ein XML-Attribut. Er ist stattdessen ein Teil des Ausdrucks für die Markup Erweiterung.

Ebenso wie Sie `x:StaticExtension` bei der Verwendung als Objekt Element `x:Static` abkürzen können, können Sie es auch im Ausdruck in geschweiften Klammern abkürzen:

```xaml
<Label Text="Label No. 4"
       FontSize="{x:Static Member=local:AppConstants.NormalFontSize}" />
```

Die `StaticExtension`-Klasse verfügt über ein `ContentProperty` Attribut, das auf die-Eigenschaft `Member` verweist, die diese Eigenschaft als Standard Inhalts Eigenschaft der Klasse kennzeichnet. Bei XAML-Markup Erweiterungen, die mit geschweiften Klammern ausgedrückt werden, können Sie den `Member=` Teil des Ausdrucks entfernen:

```xaml
<Label Text="Label No. 5"
       FontSize="{x:Static local:AppConstants.NormalFontSize}" />
```

Dies ist die gängigste Form der `x:Static` Markup Erweiterung.

Die **statische Demoseite** enthält zwei weitere Beispiele. Das Stammtag der XAML-Datei enthält eine XML-Namespace Deklaration für den .net `System`-Namespace:

```xaml
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

Dadurch kann die `Label` Schriftgröße auf das statische Feld `Math.PI` festgelegt werden. Dies führt zu einem eher kleinen Text, sodass die `Scale`-Eigenschaft auf `Math.E` festgelegt ist:

```xaml
<Label Text="&#x03C0; &#x00D7; E sized text"
       FontSize="{x:Static sys:Math.PI}"
       Scale="{x:Static sys:Math.E}"
       HorizontalOptions="Center" />
```

Im letzten Beispiel wird der `Device.RuntimePlatform` Wert angezeigt. Die `Environment.NewLine` static-Eigenschaft wird verwendet, um ein neue-Zeile-Zeichen zwischen den beiden `Span`-Objekten einzufügen:

```xaml
<Label HorizontalTextAlignment="Center"
       FontSize="{x:Static local:AppConstants.NormalFontSize}">
    <Label.FormattedText>
        <FormattedString>
            <Span Text="Runtime Platform: " />
            <Span Text="{x:Static sys:Environment.NewLine}" />
            <Span Text="{x:Static Device.RuntimePlatform}" />
        </FormattedString>
    </Label.FormattedText>
</Label>
```

Hier ist das Beispiel, das ausgeführt wird:

[![x:statische Demo](consuming-images/staticdemo-small.png "x:statische Demo")](consuming-images/staticdemo-large.png#lightbox "x:statische Demo")

<a name="reference" />

## <a name="xreference-markup-extension"></a>x:Reference-Markuperweiterung

Die `x:Reference` Markup Erweiterung wird von der [`ReferenceExtension`](xref:Xamarin.Forms.Xaml.ReferenceExtension) -Klasse unterstützt. Die-Klasse verfügt über eine einzelne Eigenschaft mit dem Namen [`Name`](xref:Xamarin.Forms.Xaml.ReferenceExtension.Name) vom Typ `string`, die Sie auf den Namen eines Elements auf der Seite festlegen, dem mit `x:Name` ein Name zugewiesen wurde. Diese `Name` Eigenschaft ist die Content-Eigenschaft von `ReferenceExtension`, sodass `Name=` nicht erforderlich ist, wenn `x:Reference` in geschweiften Klammern angezeigt wird.

Die `x:Reference` Markup Erweiterung wird ausschließlich mit Daten Bindungen verwendet, die im Artikel [**Datenbindung**](~/xamarin-forms/app-fundamentals/data-binding/index.md)ausführlicher beschrieben werden.

Die **Demo Seite "x:Reference** " zeigt zwei Verwendungsmöglichkeiten von `x:Reference` mit Daten Bindungen, das erste, wo es verwendet wird, um die `Source`-Eigenschaft des `Binding`-Objekts festzulegen, und das zweite, wo es verwendet wird, um die `BindingContext`-Eigenschaft für zwei Daten Bindungen festzulegen. :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.ReferenceDemoPage"
             x:Name="page"
             Title="x:Reference Demo">

    <StackLayout Margin="10, 0">

        <Label Text="{Binding Source={x:Reference page},
                              StringFormat='The type of this page is {0}'}"
               FontSize="18"
               VerticalOptions="CenterAndExpand"
               HorizontalTextAlignment="Center" />

        <Slider x:Name="slider"
                Maximum="360"
                VerticalOptions="Center" />

        <Label BindingContext="{x:Reference slider}"
               Text="{Binding Value, StringFormat='{0:F0}&#x00B0; rotation'}"
               Rotation="{Binding Value}"
               FontSize="24"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

    </StackLayout>
</ContentPage>
```

Beide `x:Reference` Ausdrücke verwenden die abgekürzte Version des `ReferenceExtension` Klassen namens und entfernen den `Name=` Teil des Ausdrucks. Im ersten Beispiel ist die `x:Reference` Markup Erweiterung in der `Binding` Markup Erweiterung eingebettet. Beachten Sie, dass die `Source`-und `StringFormat` Einstellungen durch Kommas getrennt sind. Dies ist das Programm, das ausgeführt wird:

[![x:verweisdemo](consuming-images/referencedemo-small.png "x:verweisdemo")](consuming-images/referencedemo-large.png#lightbox "x:verweisdemo")

<a name="type" />

## <a name="xtype-markup-extension"></a>x:Type-Markuperweiterung

Die `x:Type` Markup Erweiterung ist die XAML-Entsprechung C# des [`typeof`](/dotnet/csharp/language-reference/keywords/typeof/) -Schlüssel Worts. Sie wird von der [`TypeExtension`](xref:Xamarin.Forms.Xaml.TypeExtension) -Klasse unterstützt, die eine Eigenschaft mit dem Namen [`TypeName`](xref:Xamarin.Forms.Xaml.TypeExtension.TypeName) vom Typ `string` definiert, die auf einen Klassen-oder Struktur Namen festgelegt ist. Die `x:Type` Markup Erweiterung gibt das [`System.Type`](xref:System.Type) -Objekt dieser Klasse oder Struktur zurück. `TypeName` ist die Content-Eigenschaft von `TypeExtension`, sodass `TypeName=` nicht erforderlich ist, wenn `x:Type` mit geschweiften Klammern angezeigt wird.

In xamarin. Forms gibt es mehrere Eigenschaften, die Argumente vom Typ `Type` haben. Beispiele hierfür sind die [`TargetType`](xref:Xamarin.Forms.Style.TargetType) -Eigenschaft `Style` und das [x:TypeArguments](~/xamarin-forms/xaml/passing-arguments.md#generic_type_arguments) -Attribut, mit dem Argumente in generischen Klassen angegeben werden. Der XAML-Parser führt den `typeof` Vorgang jedoch automatisch aus, und die `x:Type` Markup Erweiterung wird in diesen Fällen nicht verwendet.

Ein Ort, an *dem `x:Type` erforderlich ist,* ist die `x:Array` Markup Erweiterung, die im [nächsten Abschnitt](#array)beschrieben wird.

Die `x:Type` Markup Erweiterung ist auch nützlich, wenn ein Menü erstellt wird, in dem jedes Menü Element einem Objekt eines bestimmten Typs entspricht. Sie können jedem Menü Element ein `Type` Objekt zuordnen und dann das Objekt instanziieren, wenn das Menü Element ausgewählt wird.

So funktioniert das Navigationsmenü in `MainPage` im **Markup Extensions** -Programm. Die Datei " **MainPage. XAML** " enthält eine `TableView`, wobei jede `TextCell` einer bestimmten Seite im Programm entspricht:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:MarkupExtensions"
             x:Class="MarkupExtensions.MainPage"
             Title="Markup Extensions"
             Padding="10">
    <TableView Intent="Menu">
        <TableRoot>
            <TableSection>
                <TextCell Text="x:Static Demo"
                          Detail="Access constants or statics"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:StaticDemoPage}" />

                <TextCell Text="x:Reference Demo"
                          Detail="Reference named elements on the page"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:ReferenceDemoPage}" />

                <TextCell Text="x:Type Demo"
                          Detail="Associate a Button with a Type"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:TypeDemoPage}" />

                <TextCell Text="x:Array Demo"
                          Detail="Use an array to fill a ListView"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:ArrayDemoPage}" />

                ···                          

        </TableRoot>
    </TableView>
</ContentPage>
```

Dies ist die öffnende Hauptseite in **Markup Erweiterungen**:

[![Hauptseite](consuming-images/mainpage-small.png "Hauptseite")](consuming-images/mainpage-large.png#lightbox "Hauptseite")

Jede `CommandParameter` Eigenschaft wird auf eine `x:Type` Markup Erweiterung festgelegt, die auf eine der anderen Seiten verweist. Die `Command`-Eigenschaft ist an eine Eigenschaft mit dem Namen `NavigateCommand` gebunden. Diese Eigenschaft wird in der `MainPage` Code Behind-Datei definiert:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();

        NavigateCommand = new Command<Type>(async (Type pageType) =>
        {
            Page page = (Page)Activator.CreateInstance(pageType);
            await Navigation.PushAsync(page);
        });

        BindingContext = this;
    }

    public ICommand NavigateCommand { private set; get; }
}
```

Die `NavigateCommand`-Eigenschaft ist ein `Command` Objekt, das einen Execute-Befehl mit einem Argument vom Typ `Type` &mdash; den Wert `CommandParameter` implementiert. Die-Methode verwendet `Activator.CreateInstance`, um die Seite zu instanziieren und dann zu dieser zu navigieren. Der Konstruktor wird beendet, indem der `BindingContext` der Seite auf sich selbst festgelegt wird, sodass die `Binding` auf `Command` funktionieren kann. Weitere Informationen zu dieser Art von Code finden Sie im Artikel [**Datenbindung**](~/xamarin-forms/app-fundamentals/data-binding/index.md) und insbesondere im Artikel zur [**Befehls**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md) Zeile.

Die **x:Type-Demoseite** verwendet eine ähnliche Technik, um xamarin. Forms-Elemente zu instanziieren und Sie einem `StackLayout` hinzuzufügen. Die XAML-Datei besteht anfänglich aus drei `Button` Elementen, deren `Command` Eigenschaften auf einen `Binding` festgelegt sind, und die `CommandParameter` Eigenschaften auf Typen von drei xamarin. Forms-Ansichten festgelegt sind:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.TypeDemoPage"
             Title="x:Type Demo">

    <StackLayout x:Name="stackLayout"
                 Padding="10, 0">

        <Button Text="Create a Slider"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                Command="{Binding CreateCommand}"
                CommandParameter="{x:Type Slider}" />

        <Button Text="Create a Stepper"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                Command="{Binding CreateCommand}"
                CommandParameter="{x:Type Stepper}" />

        <Button Text="Create a Switch"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                Command="{Binding CreateCommand}"
                CommandParameter="{x:Type Switch}" />
    </StackLayout>
</ContentPage>
```

Mit der Code-Behind-Datei wird die `CreateCommand`-Eigenschaft definiert und initialisiert:

```csharp
public partial class TypeDemoPage : ContentPage
{
    public TypeDemoPage()
    {
        InitializeComponent();

        CreateCommand = new Command<Type>((Type viewType) =>
        {
            View view = (View)Activator.CreateInstance(viewType);
            view.VerticalOptions = LayoutOptions.CenterAndExpand;
            stackLayout.Children.Add(view);
        });

        BindingContext = this;
    }

    public ICommand CreateCommand { private set; get; }
}
```

Die Methode, die ausgeführt wird, wenn ein `Button` gedrückt wird, erstellt eine neue Instanz des Arguments, legt dessen `VerticalOptions`-Eigenschaft fest und fügt Sie der `StackLayout` hinzu. Die drei `Button` Elemente geben dann die Seite mit dynamisch erstellten Ansichten frei:

[![x:TypDEMO](consuming-images/typedemo-small.png "x:TypDEMO")](consuming-images/typedemo-large.png#lightbox "x:TypDEMO")

<a name="array" />

## <a name="xarray-markup-extension"></a>x:Array-Markuperweiterung

Mit der `x:Array` Markup Erweiterung können Sie ein Array im Markup definieren. Sie wird von der [`ArrayExtension`](xref:Xamarin.Forms.Xaml.ArrayExtension) -Klasse unterstützt, die zwei Eigenschaften definiert:

- `Type` vom Typ `Type`, der den Typ der Elemente im Array angibt.
- `Items` vom Typ `IList`, bei dem es sich um eine Auflistung der Elemente selbst handelt. Dies ist die Content-Eigenschaft `ArrayExtension`.

Die `x:Array` Markup Erweiterung selbst wird nie in geschweiften Klammern angezeigt. Mit `x:Array` Start-und Endtags wird die Liste der Elemente getrennt. Legen Sie die `Type`-Eigenschaft auf eine `x:Type` Markup Erweiterung fest.

Die **x:Array-Demo** Seite zeigt, wie Sie mit `x:Array` Elemente zu einem `ListView` hinzufügen, indem Sie die `ItemsSource`-Eigenschaft auf ein Array festlegen:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.ArrayDemoPage"
             Title="x:Array Demo Page">
    <ListView Margin="10">
        <ListView.ItemsSource>
            <x:Array Type="{x:Type Color}">
                <Color>Aqua</Color>
                <Color>Black</Color>
                <Color>Blue</Color>
                <Color>Fuchsia</Color>
                <Color>Gray</Color>
                <Color>Green</Color>
                <Color>Lime</Color>
                <Color>Maroon</Color>
                <Color>Navy</Color>
                <Color>Olive</Color>
                <Color>Pink</Color>
                <Color>Purple</Color>
                <Color>Red</Color>
                <Color>Silver</Color>
                <Color>Teal</Color>
                <Color>White</Color>
                <Color>Yellow</Color>
            </x:Array>
        </ListView.ItemsSource>

        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <BoxView Color="{Binding}"
                             Margin="3" />    
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</ContentPage>        
```

Der `ViewCell` erstellt für jeden Farb Eintrag einen einfachen `BoxView`:

[![x:Array-Demo](consuming-images/arraydemo-small.png "x:Array-Demo")](consuming-images/arraydemo-large.png#lightbox "x:Array-Demo")

Es gibt mehrere Möglichkeiten, die einzelnen `Color` Elemente in diesem Array anzugeben. Sie können eine `x:Static` Markup Erweiterung verwenden:

```xaml
<x:Static Member="Color.Blue" />
```

Alternativ können Sie mit `StaticResource` eine Farbe aus einem Ressourcen Wörterbuch abrufen:

```xaml
<StaticResource Key="myColor" />
```

Am Ende dieses Artikels wird eine benutzerdefinierte XAML-Markup Erweiterung angezeigt, die auch einen neuen Farbwert erstellt:

```xaml
<local:HslColor H="0.5" S="1.0" L="0.5" />
```

Wenn Sie Arrays allgemeiner Typen wie Zeichen folgen oder Zahlen definieren, verwenden Sie die im Artikel [**übergeben von Konstruktorargumenten**](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments) aufgeführten Tags, um die Werte zu begrenzen.

<a name="null" />

## <a name="xnull-markup-extension"></a>x:Null-Markuperweiterung

Die `x:Null` Markup Erweiterung wird von der [`NullExtension`](xref:Xamarin.Forms.Xaml.NullExtension) -Klasse unterstützt. Sie verfügt über keine Eigenschaften und ist einfach die XAML-Entsprechung des C# [`null`](/dotnet/csharp/language-reference/keywords/null/) -Schlüssel Worts.

Die `x:Null` Markup Erweiterung ist nur selten erforderlich und wird selten verwendet. Wenn Sie jedoch einen Bedarf dafür feststellen, sind Sie froh, dass Sie vorhanden ist.

Die **Demo Seite x:NULL** veranschaulicht ein Szenario, in dem `x:Null` möglicherweise praktisch ist. Angenommen, Sie definieren ein implizites `Style` für `Label`, das eine `Setter` enthält, die die `FontFamily`-Eigenschaft auf einen Platt Form abhängigen Familiennamen festlegt:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.NullDemoPage"
             Title="x:Null Demo">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="FontSize" Value="48" />
                <Setter Property="FontFamily">
                    <Setter.Value>
                        <OnPlatform x:TypeArguments="x:String">
                            <On Platform="iOS" Value="Times New Roman" />
                            <On Platform="Android" Value="serif" />
                            <On Platform="UWP" Value="Times New Roman" />
                        </OnPlatform>
                    </Setter.Value>
                </Setter>
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <ContentPage.Content>
        <StackLayout Padding="10, 0">
            <Label Text="Text 1" />
            <Label Text="Text 2" />

            <Label Text="Text 3"
                   FontFamily="{x:Null}" />

            <Label Text="Text 4" />
            <Label Text="Text 5" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>   
```

Dann stellen Sie fest, dass Sie für eines der `Label` Elemente alle Eigenschafts Einstellungen in der impliziten `Style` mit Ausnahme des `FontFamily`, der der Standardwert sein soll, benötigen. Für diesen Zweck können Sie eine andere `Style` definieren, aber ein einfacherer Ansatz besteht darin, die `FontFamily`-Eigenschaft des bestimmten `Label` auf `x:Null` festzulegen, wie in der Center-`Label` veranschaulicht.

Dies ist das Programm, das ausgeführt wird:

[![x:NULL-Demo](consuming-images/nulldemo-small.png "x:NULL-Demo")](consuming-images/nulldemo-large.png#lightbox "x:NULL-Demo")

Beachten Sie, dass vier der `Label` Elemente über eine "Serif"-Schriftart verfügen, der Mittelpunkt `Label` jedoch die standardmäßige Sans-Serif-Schriftart hat.

<a name="onplatform" />

## <a name="onplatform-markup-extension"></a>Onplatform-Markup Erweiterung

Die `OnPlatform` Markup Erweiterung ermöglicht es Ihnen, die Darstellung der Benutzeroberfläche plattformspezifisch anzupassen. Sie bietet die gleiche Funktionalität wie die [`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) -und [`On`](xref:Xamarin.Forms.On) Klassen, bietet jedoch eine präzisere Darstellung.

Die `OnPlatform` Markup Erweiterung wird von der [`OnPlatformExtension`](xref:Xamarin.Forms.Xaml.OnPlatformExtension) -Klasse unterstützt, die die folgenden Eigenschaften definiert:

- `Default` vom Typ `object`, den Sie auf einen Standardwert festlegen, der auf die Eigenschaften angewendet werden soll, die Plattformen darstellen.
- `Android` vom Typ `object`, den Sie auf einen Wert festlegen, der auf Android angewendet werden soll.
- `GTK` vom Typ `object`, den Sie auf einen Wert festlegen, der auf GTK-Plattformen angewendet werden soll.
- `iOS` vom Typ `object`, den Sie auf einen Wert festlegen, der auf IOS angewendet werden soll.
- `macOS` vom Typ `object`, den Sie auf einen Wert festlegen, der auf macOS angewendet werden soll.
- `Tizen` vom Typ `object`, den Sie auf einen Wert festlegen, der auf die tizen-Plattform angewendet werden soll.
- `UWP` vom Typ `object`, den Sie auf einen Wert festgelegt haben, der auf die universelle Windows-Plattform angewendet werden soll.
- `WPF` vom Typ `object`, den Sie auf einen Wert festgelegt haben, der auf die Windows Presentation Foundation Plattform angewendet werden soll.
- `Converter` vom Typ `IValueConverter`, den Sie auf eine `IValueConverter` Implementierung festgelegt haben.
- `ConverterParameter` vom Typ `object`, den Sie auf einen Wert festlegen, der an die `IValueConverter`-Implementierung übergeben werden soll.

> [!NOTE]
> Der XAML-Parser ermöglicht, dass die [`OnPlatformExtension`](xref:Xamarin.Forms.Xaml.OnPlatformExtension) Klasse als `OnPlatform` abgekürzt wird.

Die `Default`-Eigenschaft ist die Content-Eigenschaft von `OnPlatformExtension`. Daher können Sie für XAML-Markup Ausdrücke, die mit geschweiften Klammern ausgedrückt werden, den `Default=` Teil des Ausdrucks entfernen, vorausgesetzt, dass es sich um das erste Argument handelt. Wenn die `Default`-Eigenschaft nicht festgelegt ist, wird standardmäßig der-Eigenschafts Wert [`BindableProperty.DefaultValue`](xref:Xamarin.Forms.BindableProperty.DefaultValue) verwendet, vorausgesetzt, dass die Markup Erweiterung auf eine [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)abzielt.

> [!IMPORTANT]
> Der XAML-Parser erwartet, dass die Werte des richtigen Typs für Eigenschaften bereitgestellt werden, die die `OnPlatform` Markup Erweiterung nutzen. Wenn eine Typkonvertierung erforderlich ist, versucht die `OnPlatform` Markup Erweiterung, Sie mit den von xamarin. Forms bereitgestellten Standard Konvertern auszuführen. Es gibt jedoch einige Typkonvertierungen, die von den Standard Konvertern nicht ausgeführt werden können. in diesen Fällen sollte die `Converter`-Eigenschaft auf eine `IValueConverter` Implementierung festgelegt werden.

Die **onplatform-Demo** Seite zeigt, wie die `OnPlatform` Markup Erweiterung verwendet wird:

```xaml
<BoxView Color="{OnPlatform Yellow, iOS=Red, Android=Green, UWP=Blue}"
         WidthRequest="{OnPlatform 250, iOS=200, Android=300, UWP=400}"  
         HeightRequest="{OnPlatform 250, iOS=200, Android=300, UWP=400}"
         HorizontalOptions="Center" />
```

In diesem Beispiel verwenden alle drei `OnPlatform` Ausdrücke die abgekürzte Version des `OnPlatformExtension` Klassen namens. Die drei `OnPlatform` Markup Erweiterungen legen die Eigenschaften [`Color`](xref:Xamarin.Forms.BoxView.Color), [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)und [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) der [`BoxView`](xref:Xamarin.Forms.BoxView) auf verschiedene Werte unter IOS, Android und UWP fest. Die Markup Erweiterungen stellen außerdem Standardwerte für diese Eigenschaften auf den Plattformen bereit, die nicht angegeben sind, während der `Default=` Teil des Ausdrucks eliminiert wird. Beachten Sie, dass die festgelegten Markup Erweiterungs Eigenschaften durch Kommas voneinander getrennt sind.

Dies ist das Programm, das ausgeführt wird:

[![Onplatform-Demo](consuming-images/onplatformdemo-small.png "Onplatform-Demo")](consuming-images/onplatformdemo-large.png#lightbox "Onplatform-Demo")

<a name="onidiom" />

## <a name="onidiom-markup-extension"></a>Onidiom-Markup Erweiterung

Mit der `OnIdiom` Markup Erweiterung können Sie die Darstellung der Benutzeroberfläche basierend auf dem Erscheinungsbild des Geräts anpassen, auf dem die Anwendung ausgeführt wird. Sie wird von der [`OnIdiomExtension`](xref:Xamarin.Forms.Xaml.OnIdiomExtension) -Klasse unterstützt, die die folgenden Eigenschaften definiert:

- `Default` vom Typ `object`, den Sie auf einen Standardwert festlegen, der auf die Eigenschaften angewendet werden soll, die die Geräte Idiome darstellen.
- `Phone` vom Typ `object`, den Sie auf einen auf Smartphones anzuwendenden Wert festlegen.
- `Tablet` vom Typ `object`, den Sie auf einen auf Tablets anzuwendenden Wert festlegen.
- `Desktop` vom Typ `object`, den Sie auf einen auf Desktop Plattformen anzuwendenden Wert festlegen.
- `TV` vom Typ `object`, den Sie auf einen Wert festlegen, der auf TV-Plattformen angewendet werden soll.
- `Watch` vom Typ `object`, den Sie auf einen auf Überwachungs Plattformen anzuwendenden Wert festlegen.
- `Converter` vom Typ `IValueConverter`, den Sie auf eine `IValueConverter` Implementierung festgelegt haben.
- `ConverterParameter` vom Typ `object`, den Sie auf einen Wert festlegen, der an die `IValueConverter`-Implementierung übergeben werden soll.

> [!NOTE]
> Der XAML-Parser ermöglicht, dass die [`OnIdiomExtension`](xref:Xamarin.Forms.Xaml.OnIdiomExtension) Klasse als `OnIdiom` abgekürzt wird.

Die `Default`-Eigenschaft ist die Content-Eigenschaft von `OnIdiomExtension`. Daher können Sie für XAML-Markup Ausdrücke, die mit geschweiften Klammern ausgedrückt werden, den `Default=` Teil des Ausdrucks entfernen, vorausgesetzt, dass es sich um das erste Argument handelt.

> [!IMPORTANT]
> Der XAML-Parser erwartet, dass die Werte des richtigen Typs für Eigenschaften bereitgestellt werden, die die `OnIdiom` Markup Erweiterung nutzen. Wenn eine Typkonvertierung erforderlich ist, versucht die `OnIdiom` Markup Erweiterung, Sie mit den von xamarin. Forms bereitgestellten Standard Konvertern auszuführen. Es gibt jedoch einige Typkonvertierungen, die von den Standard Konvertern nicht ausgeführt werden können. in diesen Fällen sollte die `Converter`-Eigenschaft auf eine `IValueConverter` Implementierung festgelegt werden.

Die Seite " **onidiom Demo** " zeigt, wie die `OnIdiom` Markup Erweiterung verwendet wird:

```xaml
<BoxView Color="{OnIdiom Yellow, Phone=Red, Tablet=Green, Desktop=Blue}"
         WidthRequest="{OnIdiom 100, Phone=200, Tablet=300, Desktop=400}"
         HeightRequest="{OnIdiom 100, Phone=200, Tablet=300, Desktop=400}"
         HorizontalOptions="Center" />
```

In diesem Beispiel verwenden alle drei `OnIdiom` Ausdrücke die abgekürzte Version des `OnIdiomExtension` Klassen namens. Die drei `OnIdiom` Markup Erweiterungen legen die Eigenschaften [`Color`](xref:Xamarin.Forms.BoxView.Color), [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)und [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) der [`BoxView`](xref:Xamarin.Forms.BoxView) auf verschiedene Werte auf der Telefon-, Tablet-und Desktop Idioms fest. Die Markup Erweiterungen stellen außerdem Standardwerte für diese Eigenschaften für die Idiome bereit, die nicht angegeben sind, während der `Default=` Teil des Ausdrucks eliminiert wird. Beachten Sie, dass die festgelegten Markup Erweiterungs Eigenschaften durch Kommas voneinander getrennt sind.

Dies ist das Programm, das ausgeführt wird:

[![Onidiom-Demo](consuming-images/onidiomdemo-small.png "Onidiom-Demo")](consuming-images/onidiomdemo-large.png#lightbox "Onidiom-Demo")

## <a name="datatemplate-markup-extension"></a>DataTemplate-Markup Erweiterung

Mit der `DataTemplate` Markup Erweiterung können Sie einen Typ in einen [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)konvertieren. Sie wird von der `DataTemplateExtension`-Klasse unterstützt, die eine `TypeName`-Eigenschaft des Typs `string` definiert, die auf den Namen des Typs festgelegt ist, der in eine `DataTemplate` konvertiert werden soll. Die `TypeName`-Eigenschaft ist die Content-Eigenschaft von `DataTemplateExtension`. Daher können Sie für XAML-Markup Ausdrücke, die mit geschweiften Klammern ausgedrückt werden, den `TypeName=` Teil des Ausdrucks entfernen.

> [!NOTE]
> Der XAML-Parser ermöglicht, dass die `DataTemplateExtension` Klasse als `DataTemplate` abgekürzt wird.

Eine typische Verwendung dieser Markup Erweiterung ist in einer Shellanwendung, wie im folgenden Beispiel gezeigt:

```xaml
<ShellContent Title="Monkeys"
              Icon="monkey.png"
              ContentTemplate="{DataTemplate views:MonkeysPage}" />
```

In diesem Beispiel wird `MonkeysPage` von einem [`ContentPage`](xref:Xamarin.Forms.ContentPage) in einen [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)konvertiert, der als Wert der `ShellContent.ContentTemplate`-Eigenschaft festgelegt wird. Dadurch wird sichergestellt, dass `MonkeysPage` nur erstellt wird, wenn die Navigation zur Seite statt beim Starten der Anwendung erfolgt.

Weitere Informationen zu Shellanwendungen finden Sie unter [xamarin. Forms Shell](~/xamarin-forms/app-fundamentals/shell/index.md).

## <a name="fontimage-markup-extension"></a>Fontimage-Markup Erweiterung

Mit der `FontImage` Markup Erweiterung können Sie ein Schriftart Symbol in jeder Ansicht anzeigen, die eine `ImageSource` anzeigen kann. Sie bietet die gleiche Funktionalität wie die `FontImageSource`-Klasse, bietet aber eine präzisere Darstellung.

Die `FontImage` Markup Erweiterung wird von der `FontImageExtension`-Klasse unterstützt, die die folgenden Eigenschaften definiert:

- `FontFamily` vom Typ `string`, die Schriftfamilie, zu der das Schriftart Symbol gehört.
- `Glyph` vom Typ `string`, der Unicode-Zeichen Wert des Schriftart Symbols.
- `Color` vom Typ `Color`, die Farbe, die beim Anzeigen des Schriftart Symbols verwendet werden soll.
- `Size` vom Typ `double` die Größe des gerenderten Schriftart Symbols in geräteunabhängigen Einheiten.

> [!NOTE]
> Der XAML-Parser ermöglicht, dass die `FontImageExtension` Klasse als `FontImage` abgekürzt wird.

Die `Glyph`-Eigenschaft ist die Content-Eigenschaft von `FontImageExtension`. Daher können Sie für XAML-Markup Ausdrücke, die mit geschweiften Klammern ausgedrückt werden, den `Glyph=` Teil des Ausdrucks entfernen, vorausgesetzt, dass es sich um das erste Argument handelt.

Die exemplarische Vorgehensweise " **fontimage** " zeigt, wie die `FontImage` Markup Erweiterung verwendet wird:

```xaml
<Image BackgroundColor="#D1D1D1"
       Source="{FontImage &#xf30c;, FontFamily={OnPlatform iOS=Ionicons, Android=ionicons.ttf#}, Size=44}" />
```

In diesem Beispiel wird die abgekürzte Version des `FontImageExtension` Klassen namens verwendet, um ein Xbox-Symbol aus der Schriftfamilie "ionicons" in einer [`Image`](xref:Xamarin.Forms.Image)anzuzeigen. Der Ausdruck verwendet auch die `OnPlatform` Markup Erweiterung, um unterschiedliche `FontFamily` Eigenschaftswerte unter IOS und Android anzugeben. Außerdem wird der `Glyph=` Teil des Ausdrucks gelöscht, und die festgelegten Markup Erweiterungs Eigenschaften werden durch Kommas getrennt. Beachten Sie, dass das Unicode-Zeichen für das Symbol zwar `\uf30c`, aber in XAML mit Escapezeichen versehen werden muss, sodass es `&#xf30c;` wird.

Dies ist das Programm, das ausgeführt wird:

[![Screenshot der fontimage-Markup Erweiterung](consuming-images/fontimagedemo.png "Fontimage-Demo")](consuming-images/fontimagedemo-large.png#lightbox "Fontimage-Demo")

Informationen zum Anzeigen von Schriftart Symbolen durch Angeben der Schriftart Symbol Daten in einem `FontImageSource` Objekt finden Sie unter [Anzeigen von Schriftart Symbolen](~/xamarin-forms/user-interface/text/fonts.md#display-font-icons).

## <a name="define-your-own-markup-extensions"></a>Definieren eigener Markup Erweiterungen

Wenn Sie eine XAML-Markup Erweiterung gefunden haben, die in xamarin. Forms nicht verfügbar ist, können Sie eine [eigene erstellen](creating.md).

## <a name="related-links"></a>Verwandte Links

- [Markup Erweiterungen (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)
- [XAML-Markup Erweiterungen-Kapitel aus dem xamarin. Forms-Buch](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [Ressourcenverzeichnisse](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Dynamische Stile](~/xamarin-forms/user-interface/styles/dynamic.md)
- [Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Xamarin. Forms-Shell](~/xamarin-forms/app-fundamentals/shell/index.md).
