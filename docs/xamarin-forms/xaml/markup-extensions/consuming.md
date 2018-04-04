---
title: Nutzen die Verwendung von XAML-Markuperweiterungen
description: Verwenden Sie die Verwendung von XAML-Markuperweiterungen in Xamarin.Forms verfügbar
ms.prod: xamarin
ms.assetid: CE686893-609C-4EC3-9225-6C68D2A9F79C
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/05/2018
ms.openlocfilehash: 25eada483e8bd2ce95cb3101dfe873ea38b283ab
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="consuming-xaml-markup-extensions"></a>Nutzen die Verwendung von XAML-Markuperweiterungen

Verwendung von XAML-Markuperweiterungen verbessern die Leistungsfähigkeit und Flexibilität von XAML-Elementattribute aus einer Vielzahl von Datenquellen festgelegt werden können. Einige XAML-Markuperweiterungen sind Teil der Spezifikation von XAML 2009. Diese werden im XAML-Dateien mit der üblichen `x` Namespacepräfix und sind im Allgemeinen bezeichnet mit diesem Präfix. Diese werden in den folgenden Abschnitten beschrieben:

- [`x:Static`](#static) &ndash; verweisen Sie statische Eigenschaften, Felder oder Enumerationsmember.
- [`x:Reference`](#reference) &ndash; Verweis mit dem Namen Elemente auf der Seite.
- [`x:Type`](#type) &ndash; Legen Sie ein Attribut auf eine `System.Type` Objekt.
- [`x:Array`](#array) &ndash; ein Array von Objekten eines bestimmten Typs zu erstellen.
- [`x:Null`](#null) &ndash; Legen Sie ein Attribut auf eine `null` Wert.

Drei andere XAML-Markuperweiterungen in der Vergangenheit durch andere XAML-Implementierungen unterstützt und werden ebenfalls unterstützt, indem Sie Xamarin.Forms. Diese werden in anderen Artikeln ausführlicher beschrieben:

- `StaticResource` &ndash; Verweisen auf Objekte aus einem Ressourcenwörterbuch, wie im Artikel beschrieben [ **Ressourcenverzeichnis**](~/xamarin-forms/xaml/resource-dictionaries.md).
- `DynamicResource` &ndash; Reagieren auf Änderungen in Objekten in einem Ressourcenwörterbuch, wie im Artikel beschrieben [ **dynamische Formate**](~/xamarin-forms/user-interface/styles/dynamic.md).
- `Binding` &ndash; Richten Sie eine Verknüpfung zwischen Eigenschaften von zwei Objekten, wie im Artikel beschrieben [ **Datenbindung**](~/xamarin-forms/app-fundamentals/data-binding/index.md).
- `TemplateBinding` &ndash; Binden von Daten aus einer Steuerelementvorlage ausführt, wie im Artikel erläutert [**binden aus einer Steuerelementvorlage**] / Führungslinien/Xamarin-Forms/Anwendung-Grundlagen/Vorlagen /-Vorlagen/Vorlage-steuerelementbindung /)

Die [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/) Layout verwendet die benutzerdefinierte Markuperweiterung [ `ConstraintExpression` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ConstraintExpression/). Diese Markuperweiterung ist im Artikel beschriebenen [ **RelativeLayout**](~/xamarin-forms/user-interface/layouts/relative-layout.md).

<a name="static" />

## <a name="xstatic-markup-extension"></a>x:Statische Markuperweiterung

Die `x:Static` Markuperweiterung wird unterstützt, indem Sie die [ `StaticExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticExtension/) Klasse. Die Klasse verfügt über eine einzelne Eigenschaft mit dem Namen [ `Member` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.StaticExtension.Member/) des Typs `string` , dass Sie auf den Namen einer öffentlichen Konstante, statische Eigenschaft, statisches Feld oder Enumerationsmember festgelegt.

Eine gebräuchliche Möglichkeit zum Verwenden `x:Static` besteht darin, zuerst eine Klasse mit Konstanten oder statische Variablen, wie z. B. definieren diesem kleinen `AppConstants` -Klasse in der [ **MarkupExtensions** ](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/) Programm:

```csharp
static class AppConstants
{
    public static double NormalFontSize = 18;
}
```

Die **X: Static-Demo** Seite veranschaulicht mehrere Möglichkeiten zum Verwenden der `x:Static` Markuperweiterung. Die ausführlichste Ansatz instanziiert die `StaticExtension` zwischen Klasse `Label.FontSize` Property-Element Tags:

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

XAML-Parser kann auch die `StaticExtension` Klasse als abgekürzt werden `x:Static`:

```xaml
<Label Text="Label No. 2">
    <Label.FontSize>
        <x:Static Member="local:AppConstants.NormalFontSize" />
    </Label.FontSize>
</Label>
```

Dies noch weiter vereinfacht werden, aber die Änderung führt einige neue Syntax: Einfügen besteht die `StaticExtension` Klasse und das Element in der geschweiften Klammern festlegen. Der resultierende Ausdruck festgelegt ist, direkt an die `FontSize` Attribut:

```xaml
<Label Text="Label No. 3"
       FontSize="{x:StaticExtension Member=local:AppConstants.NormalFontSize}" />
```

Beachten Sie, dass es *keine* Anführungszeichen in der geschweiften Klammern. Die `Member` Eigenschaft `StaticExtension` ist nicht mehr ein XML-Attribut. Stattdessen ist er Teil des Ausdrucks für die Markuperweiterung.

So fest, wie Sie abkürzen können `x:StaticExtension` zu `x:Static` , wenn Sie es als Element eines Objektelements verwenden, Sie können auch abkürzen es im Ausdruck in geschweiften Klammern:

```xaml
<Label Text="Label No. 4"
       FontSize="{x:Static Member=local:AppConstants.NormalFontSize}" />
```

Die `StaticExtension` -Klasse verfügt über eine `ContentProperty` Attribut verweisen auf die Eigenschaft `Member`, die diese Eigenschaft als Inhalt Standardeigenschaft der Klasse kennzeichnet. Für die Verwendung von XAML-Markuperweiterungen, ausgedrückt in geschweifte Klammern setzen, vermeiden Sie die `Member=` Teil des Ausdrucks:

```xaml
<Label Text="Label No. 5"
       FontSize="{x:Static local:AppConstants.NormalFontSize}" />
```

Dies ist die am häufigsten verwendete Form der `x:Static` Markuperweiterung.

Die **statische Demo** Seite enthält zwei weitere Beispiele. Das Stamm-Tag, der XAML-Datei enthält eine XML-Namespacedeklaration für das .NET `System` Namespace:

```xaml
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

Dies ermöglicht die `Label` Schriftgrad auf das statische Feld festgelegt werden `Math.PI`. Das ergibt eine relativ kleine Text, sodass der `Scale` -Eigenschaftensatz auf `Math.E`:

```xaml
<Label Text="&#x03C0; &#x00D7; E sized text"
       FontSize="{x:Static sys:Math.PI}"
       Scale="{x:Static sys:Math.E}"
       HorizontalOptions="Center" />
```

Im abschließenden Beispiel zeigt die `Device.RuntimePlatform` Wert. Die `Environment.NewLine` statische Eigenschaft wird verwendet, um ein neue-Zeile-Zeichen zwischen den beiden einfügen `Span` Objekte:

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

So sieht das Beispiel auf allen drei Plattformen ausgeführt:

[![X: Static-Demo](consuming-images/staticdemo-small.png "X: Static-Demo")](consuming-images/staticdemo-large.png#lightbox "X: Static-Demo")

<a name="reference" />

## <a name="xreference-markup-extension"></a>x:Reference-Markuperweiterung

Die `x:Reference` Markuperweiterung wird unterstützt, indem Sie die [ `ReferenceExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ReferenceExtension/) Klasse. Die Klasse verfügt über eine einzelne Eigenschaft mit dem Namen [ `Name` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.ReferenceExtension.Name/) des Typs `string` , dass Sie auf den Namen eines Elements auf der Seite festgelegt, die einen Namen mit erhalten hat `x:Name`. Dies `Name` Eigenschaft ist für die Inhaltseigenschaft des `ReferenceExtension`, sodass `Name=` ist nicht erforderlich, wenn `x:Reference` wird in der geschweiften Klammern angezeigt.

Die `x:Reference` Markuperweiterung dient ausschließlich mit datenbindungen, die in den Artikel ausführlicher beschrieben werden [ **Datenbindung**](~/xamarin-forms/app-fundamentals/data-binding/index.md).

Die **X: Reference-Demo** Seite zeigt zwei Verwendungen von `x:Reference` mit datenbindungen, die erste, wo sie verwendet wird, legen Sie, die `Source` Eigenschaft der `Binding` Objekt und das zweite, wo sie verwendet wird, legen Sie, die `BindingContext` die Eigenschaft für zwei Bindungen:

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

Sowohl `x:Reference` Ausdrücke verwenden, die Kurzform der `ReferenceExtension` Klassennamen und Beseitigen der `Name=` Teil des Ausdrucks. Im ersten Beispiel das `x:Reference` Markuperweiterung eingebettet ist, der `Binding` Markuperweiterung. Beachten Sie, dass die `Source` und `StringFormat` Einstellungen werden durch Kommas getrennt. Hier wird das Programm auf allen drei Plattformen ausgeführt:

[![X: Reference-Demo](consuming-images/referencedemo-small.png "X: Reference-Demo")](consuming-images/referencedemo-large.png#lightbox "X: Reference-Demo")

<a name="type" />

## <a name="xtype-markup-extension"></a>x:Type-Markuperweiterung

Die `x:Type` Markuperweiterung ist die Verwendung von XAML-Entsprechung der C#- [ `typeof` ](/dotnet/csharp/language-reference/keywords/typeof/) Schlüsselwort. Entsprechender Unterstützung durch die [ `TypeExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TypeExtension/) Klasse, die eine Eigenschaft mit dem Namen definiert [ `TypeName` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.TypeExtension.TypeName/) des Typs `string` , die auf eine Klasse oder Struktur Name festgelegt ist. Die `x:Type` Markup Extension gibt die [ `System.Type` ](https://developer.xamarin.com/api/type/System.Type/) Objekt dieser Klasse oder Struktur. `TypeName` ist die Inhaltseigenschaft des `TypeExtension`, sodass `TypeName=` ist nicht erforderlich, wenn `x:Type` mit geschweiften Klammern angezeigt.

In Xamarin.Forms, es gibt mehrere Eigenschaften, die Argumente des Typs haben `Type`. Beispiele hierfür sind die [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) Eigenschaft `Style`, und die [X: TypeArguments-](~/xamarin-forms/xaml/passing-arguments.md#generic_type_arguments) Attribut zur Angabe von Argumenten in generischen Klassen verwendet. XAML-Parser führt jedoch die `typeof` Vorgang automatisch, und die `x:Type` Markuperweiterung wird in diesen Fällen nicht verwendet.

Zentral, in denen `x:Type` *ist* erforderlich ist, mit der `x:Array` Markuperweiterung, die in beschrieben ist die [nächsten Abschnitt](#array).

Die `x:Type` Markuperweiterung ist auch nützlich, wenn ein Menü zu erstellen, wobei ein Objekt eines bestimmten Typs jedes Elements entspricht. Die Zuordnung kann eine `Type` Objekts, für jedes Menüelement und instanziieren Sie das Objekt an, wenn das Menüelement aktiviert ist.

Dies ist wie das Navigationsmenü im `MainPage` in der **Markuperweiterungen** Programm funktioniert. Die **"MainPage.xaml"** -Datei enthält eine `TableView` mit jedem `TextCell` zu einer bestimmten Seite in der Anwendung entspricht:

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

Hier ist die Hauptseite der öffnenden in **Markuperweiterungen**:

[![Main Seite](consuming-images/mainpage-small.png "Main Seite")](consuming-images/mainpage-large.png#lightbox "Main Seite")

Jede `CommandParameter` -Eigenschaftensatz auf eine `x:Type` Markuperweiterung, die auf eine der anderen Seiten verweist. Die `Command` Eigenschaft gebunden ist, auf eine Eigenschaft mit dem Namen `NavigateCommand`. Diese Eigenschaft wird definiert, der `MainPage` Code-Behind-Datei:

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

Die `NavigateCommand` Eigenschaft ist ein `Command` Objekt, das einen Execute-Befehl mit einem Argument vom Typ implementiert `Type` &mdash; den Wert des `CommandParameter`. Die Methode verwendet `Activator.CreateInstance` auf die Seite zu instanziieren und wechselt anschließend darauf. Der Konstruktor endet, durch Festlegen der `BindingContext` der Seite auf sich selbst, wodurch die `Binding` auf `Command` arbeiten. Finden Sie unter der [ **Datenbindung** ](~/xamarin-forms/app-fundamentals/data-binding/index.md) Artikel und insbesondere die [ **Commanding** ](~/xamarin-forms/app-fundamentals/data-binding/commanding.md) Artikel finden Sie weitere Details über diese Art von Code.

Die **X: Type-Demo** Seite verwendet eine ähnliche Technik, Xamarin.Forms Elemente zu instanziieren und sie zum Hinzufügen einer `StackLayout`. Die XAML-Datei zunächst besteht aus drei `Button` Elemente mit ihren `Command` Eigenschaften festlegen, um eine `Binding` und `CommandParameter` Eigenschaften, die auf drei Xamarin.Forms Ansichtsarten festgelegt:

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

Die Code-Behind-Datei definiert und initialisiert die `CreateCommand` Eigenschaft:

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

Die Methode, ausgeführt wird, wenn eine `Button` gedrückt erstellt eine neue Instanz des Arguments wird, wird seine `VerticalOptions` -Eigenschaft, und fügt es der `StackLayout`. Die drei `Button` Elemente anschließend freigegeben wird, die Seite dynamisch erstellte Sichten:

[![X: Type-Demo](consuming-images/typedemo-small.png "X: Type-Demo")](consuming-images/typedemo-large.png#lightbox "X: Type-Demo")

<a name="array" />

## <a name="xarray-markup-extension"></a>x:Array-Markuperweiterung

Die `x:Array` Markuperweiterung können Sie ein Array im Markup zu definieren. Entsprechender Unterstützung durch die [ `ArrayExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ArrayExtension/) Klasse, die zwei Eigenschaften definiert:

- `Type` Der Typ `Type`, gibt den Typ der Elemente im Array an.
- `Items` Der Typ `IList`, dies ist eine Auflistung der Elemente selbst. Dies ist die Inhaltseigenschaft des `ArrayExtension`.

Die `x:Array` Markuperweiterung selbst nie in der geschweiften Klammern angezeigt wird. Stattdessen `x:Array` Start- und Endtags begrenzen Sie die Liste der Elemente. Legen Sie die `Type` Eigenschaft, um eine `x:Type` Markuperweiterung.

Die **X: Array-Demo** Seite zeigt, wie `x:Array` Elemente hinzufügen einer `ListView` durch Festlegen der `ItemsSource` Eigenschaft in ein Array:

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

Die `ViewCell` erstellt eine einfache `BoxView` für jeden Eintrag Farbe:

[![X: Array-Demo](consuming-images/arraydemo-small.png "X: Array-Demo")](consuming-images/arraydemo-large.png#lightbox "X: Array-Demo")

Es gibt mehrere Möglichkeiten zum Angeben der einzelnen `Color` Elemente in diesem Array. Sie können eine `x:Static` Markuperweiterung:

```xaml
<x:Static Member="Color.Blue" />
```

Alternativ können Sie `StaticResource` um eine Farbe aus einem Ressourcenwörterbuch abzurufen:

```xaml
<StaticResource Key="myColor" />
```

Gegen Ende dieses Artikels sehen Sie eine benutzerdefinierte XAML-Markuperweiterung, die auch einen neuen Farbwert erstellt:

```xaml
<local:HslColor H="0.5" S="1.0" L="0.5" />
```

Wenn Arrays von gemeinsamen Typen wie Zeichenfolgen oder Zahlen zu definieren, verwenden Sie die Tags aufgeführt, die der [ **Konstruktorargumente übergeben** ](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments) Artikel, um die Werte begrenzen.

<a name="null" />

## <a name="xnull-markup-extension"></a>x:Null-Markuperweiterung

Die `x:Null` Markuperweiterung wird unterstützt, indem Sie die [ `NullExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.NullExtension/) Klasse. Er verfügt über keine Eigenschaften und wird einfach die Verwendung von XAML-Entsprechung der C#- [ `null` ](/dotnet/csharp/language-reference/keywords/null/) Schlüsselwort.

Die `x:Null` Markuperweiterung selten erforderlich ist, und nur selten verwendet, aber Sie müssen dafür zu finden, müssen Sie froh, die vorhanden sein.

Die **X: Null-Demo** Seite veranschaulicht ein Szenario bei `x:Null` kann zweckmäßig sein. Nehmen wir an, dass Sie einen impliziten definieren `Style` für `Label` , umfasst eine `Setter` festlegt, die die `FontFamily` Eigenschaft, um einen Familiennamen plattformabhängige:

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

Dann Sie, die für eine der ermittelt der `Label` Elemente, Sie möchten die eigenschafteneinstellungen in den impliziten `Style` mit Ausnahme von der `FontFamily`, als Standardwert verwendet werden soll. Können Sie einen anderen definieren `Style` für diesen Zweck, sondern ein einfacher Ansatz besteht darin, legen Sie die `FontFamily` Eigenschaft des entsprechenden `Label` auf `x:Null`, wie gezeigt, in der Mitte `Label`.

Hier wird das Programm auf drei Plattformen ausgeführt wird:

[![X: Null-Demo](consuming-images/nulldemo-small.png "X: Null-Demo")](consuming-images/nulldemo-large.png#lightbox "X: Null-Demo")

Beachten Sie, dass diese vier von der `Label` Elemente haben eine Serifenschriftart jedoch im Center `Label` Schriftart sans-Serif "Standard" hat.

## <a name="define-your-own-markup-extensions"></a>Ihre eigenen Markuperweiterungen definieren

Wenn Sie eine Notwendigkeit für eine XAML-Markuperweiterung, die nicht in Xamarin.Forms verfügbar ist gefunden haben, können Sie [erstellen Sie einen eigenen](creating.md).


## <a name="related-links"></a>Verwandte Links

- [Markuperweiterungen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)
- [Verwendung von XAML-Markup Extensions Kapitel Xamarin.Forms Buchs](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [Ressourcenverzeichnisse](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Dynamische Stile](~/xamarin-forms/user-interface/styles/dynamic.md)
- [Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md)
