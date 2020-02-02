---
title: Verwenden von XAML-Markuperweiterungen
description: In diesem Artikel wird erläutert, wie Xamarin.Forms-XAML-Markuperweiterungen verwenden, um die Leistungsfähigkeit und Flexibilität von XAML zu verbessern, indem Sie Attribute des Elements aus einer Vielzahl von Datenquellen festgelegt werden können.
ms.prod: xamarin
ms.assetid: CE686893-609C-4EC3-9225-6C68D2A9F79C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/04/2019
ms.openlocfilehash: 693dd29e59aee73a206398a8a44cc4f15e385d8d
ms.sourcegitcommit: 52fb214c0e0243587d4e9ad9306b75e92a8cc8b7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2020
ms.locfileid: "76940287"
---
# <a name="consuming-xaml-markup-extensions"></a>Verwenden von XAML-Markuperweiterungen

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)

XAML-Markuperweiterungen können die Leistungsfähigkeit und Flexibilität von XAML zu verbessern, indem Sie Attribute des Elements aus einer Vielzahl von Datenquellen festgelegt werden können. Einige XAML-Markuperweiterungen sind Teil der Spezifikation für die XAML 2009. Diese werden in XAML-Dateien mit der üblichen `x` Namespace-Präfix, und Sie werden häufig bezeichnet mit diesem Präfix. In diesem Artikel werden die folgenden Markuperweiterungen erläutert:

- [`x:Static`](#static) – statische Eigenschaften, Felder oder Enumerationsmember zu verweisen.
- [`x:Reference`](#reference) – Verweis mit dem Namen Elemente auf der Seite.
- [`x:Type`](#type) – Legen Sie ein Attribut auf eine `System.Type` Objekt.
- [`x:Array`](#array) – ein Array von Objekten eines bestimmten Typs zu erstellen.
- [`x:Null`](#null) – Legen Sie ein Attribut auf eine `null` Wert.
- [`OnPlatform`](#onplatform) – Benutzeroberfläche-Darstellung auf einer Basis pro Plattform anpassen.
- [`OnIdiom`](#onidiom) – Anpassen der UI-Darstellung, die abhängig von der Sprache des Geräts auf die Anwendung ausgeführt wird.
- [`DataTemplate`](#datatemplate-markup-extension) : konvertiert einen Typ in einen [`DataTemplate`](xref:Xamarin.Forms.DataTemplate).
- [`FontImage`](#fontimage-markup-extension) : zeigt ein Schriftart Symbol in einer beliebigen Ansicht an, die eine `ImageSource`anzeigen kann.

Zusätzliche XAML-Markuperweiterungen verfügen in der Vergangenheit von anderen XAML-Implementierungen unterstützt wurde, und werden auch von Xamarin.Forms unterstützt. Diese werden in anderen Artikeln ausführlicher beschrieben:

- `StaticResource` Verweis Objekte aus einem Ressourcen Wörterbuch, wie im Artikel [**Ressourcen Wörterbücher**](~/xamarin-forms/xaml/resource-dictionaries.md)beschrieben.
- `DynamicResource`: reagieren auf Änderungen in Objekten in einem Ressourcen Wörterbuch, wie im Artikel [**dynamische Stile**](~/xamarin-forms/user-interface/styles/dynamic.md)beschrieben.
- `Binding`: Erstellen Sie eine Verknüpfung zwischen Eigenschaften von zwei-Objekten, wie im Artikel [**Datenbindung**](~/xamarin-forms/app-fundamentals/data-binding/index.md)beschrieben.
- `TemplateBinding`: führt die Datenbindung aus einer Steuerelement Vorlage aus, wie im Artikel [**xamarin. Forms-Steuerelement Vorlagen**](~/xamarin-forms/app-fundamentals/templates/control-template.md)erläutert.
- legt `RelativeSource` die Bindungs Quelle relativ zur Position des Bindungs Ziels fest, wie im Artikel [relative Bindungen](~/xamarin-forms/app-fundamentals/data-binding/relative-bindings.md)erläutert.

Die [ `RelativeLayout` ](xref:Xamarin.Forms.RelativeLayout) Layout verwendet die benutzerdefinierte Markuperweiterung [ `ConstraintExpression` ](xref:Xamarin.Forms.ConstraintExpression). Diese Markuperweiterung ist in diesem Artikel beschriebenen [ **RelativeLayout**](~/xamarin-forms/user-interface/layouts/relative-layout.md).

<a name="static" />

## <a name="xstatic-markup-extension"></a>x:Static-Markuperweiterung

Die `x:Static` Markuperweiterung wird unterstützt, indem die [ `StaticExtension` ](xref:Xamarin.Forms.Xaml.StaticExtension) Klasse. Die Klasse verfügt über eine einzelne Eigenschaft namens [ `Member` ](xref:Xamarin.Forms.Xaml.StaticExtension.Member) des Typs `string` , dass Sie den Namen der eine öffentliche Konstante, statische Eigenschaft, Feld oder Enumerationsmember festlegen.

Eine gebräuchliche Möglichkeit zum Verwenden `x:Static` besteht darin, zuerst eine Klasse mit einigen Konstanten oder statischen Variablen wie z. B. definieren diesem kleinen `AppConstants` -Klasse in der [ **MarkupExtensions** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions) Programm:

```csharp
static class AppConstants
{
    public static double NormalFontSize = 18;
}
```

Die **X: Static Demo** Seite zeigt verschiedene Möglichkeiten zum Verwenden der `x:Static` Markuperweiterung. Die ausführlichste Ansatz instanziiert die `StaticExtension` Klasse zwischen `Label.FontSize` Eigenschaftenelement Tags:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sys="clr-namespace:System;assembly=netstandard"
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

Der XAML-Parser ermöglicht auch die `StaticExtension` Klasse als abgekürzt werden `x:Static`:

```xaml
<Label Text="Label No. 2">
    <Label.FontSize>
        <x:Static Member="local:AppConstants.NormalFontSize" />
    </Label.FontSize>
</Label>
```

Dies sogar noch weiter vereinfacht werden, aber die Änderung werden einige neue Syntax eingeführt: Es besteht aus zu platzieren, die `StaticExtension` -Klasse und der Member, die in geschweifte Klammern festlegen. Der resultierende Ausdruck festgelegt ist, direkt an die `FontSize` Attribut:

```xaml
<Label Text="Label No. 3"
       FontSize="{x:StaticExtension Member=local:AppConstants.NormalFontSize}" />
```

Beachten Sie, dass es *keine* Anführungszeichen innerhalb der geschweiften Klammern. Die `Member` Eigenschaft `StaticExtension` ist nicht mehr ein XML-Attribut. Stattdessen ist er Teil des Ausdrucks für die Markuperweiterung.

Wie Sie abkürzen können `x:StaticExtension` zu `x:Static` , wenn Sie es als Objektelement verwenden, Sie können auch abkürzen es im Ausdruck in geschweiften Klammern:

```xaml
<Label Text="Label No. 4"
       FontSize="{x:Static Member=local:AppConstants.NormalFontSize}" />
```

Die `StaticExtension` -Klasse verfügt über eine `ContentProperty` Attribut mit Verweis auf die Eigenschaft `Member`, die diese Eigenschaft als Standard Content-Eigenschaft der Klasse kennzeichnet. Für XAML-Markuperweiterungen mit geschweiften Klammern ausgedrückt, Sie können vermeiden, die `Member=` Teil des Ausdrucks:

```xaml
<Label Text="Label No. 5"
       FontSize="{x:Static local:AppConstants.NormalFontSize}" />
```

Dies ist die am häufigsten verwendeten Form der `x:Static` Markuperweiterung.

Die **statische Demo** Seite enthält zwei weitere Beispiele. Stammelement der XAML-Datei enthält eine XML-Namespacedeklaration für .NET `System` Namespace:

```xaml
xmlns:sys="clr-namespace:System;assembly=netstandard"
```

Auf diese Weise können die `Label` Schriftgrad auf das statische Feld festzulegende `Math.PI`. Dies führt zu eher klein Text, sodass die `Scale` -Eigenschaftensatz auf `Math.E`:

```xaml
<Label Text="&#x03C0; &#x00D7; E sized text"
       FontSize="{x:Static sys:Math.PI}"
       Scale="{x:Static sys:Math.E}"
       HorizontalOptions="Center" />
```

Im letzte Beispiel zeigt die `Device.RuntimePlatform` Wert. Die `Environment.NewLine` statische Eigenschaft wird verwendet, um ein neue-Zeile-Zeichen zwischen den beiden einfügen `Span` Objekte:

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

So sieht das Beispiel ausgeführt:

[![x:statische Demo](consuming-images/staticdemo-small.png "x:statische Demo")](consuming-images/staticdemo-large.png#lightbox "x:statische Demo")

<a name="reference" />

## <a name="xreference-markup-extension"></a>x:Reference-Markuperweiterung

Die `x:Reference` Markuperweiterung wird unterstützt, indem die [ `ReferenceExtension` ](xref:Xamarin.Forms.Xaml.ReferenceExtension) Klasse. Die Klasse verfügt über eine einzelne Eigenschaft namens [ `Name` ](xref:Xamarin.Forms.Xaml.ReferenceExtension.Name) des Typs `string` , auf den Namen eines Elements auf der Seite festzulegen, der einen Namen erhalten hat `x:Name`. Dies `Name` -Eigenschaft ist die Content-Eigenschaft des `ReferenceExtension`, sodass `Name=` ist nicht erforderlich, wenn `x:Reference` in geschweiften Klammern angezeigt wird.

Die `x:Reference` Markuperweiterung dient ausschließlich mit datenbindungen, die in diesem Artikel ausführlicher beschrieben werden [ **Datenbindung**](~/xamarin-forms/app-fundamentals/data-binding/index.md).

Die **X: Reference-Demo** Seite zeigt zwei Verwendungen von `x:Reference` mit datenbindungen, die erste, wo sie verwendet wird, legen Sie, die `Source` Eigenschaft der `Binding` Objekt und das zweite, in denen dient zum Festlegen, der `BindingContext` die Eigenschaft für zwei Bindungen:

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

Beide `x:Reference` Ausdrücke verwenden, die gekürzte Version des der `ReferenceExtension` Klassennamen, und Entfernen der `Name=` Teil des Ausdrucks. Im ersten Beispiel das `x:Reference` Markuperweiterung eingebettet ist, der `Binding` Markuperweiterung. Beachten Sie, dass die `Source` und `StringFormat` Einstellungen werden durch Kommas getrennt. Dies ist das Programm, das ausgeführt wird:

[![x:verweisdemo](consuming-images/referencedemo-small.png "x:verweisdemo")](consuming-images/referencedemo-large.png#lightbox "x:verweisdemo")

<a name="type" />

## <a name="xtype-markup-extension"></a>x:Type-Markuperweiterung

Die `x:Type` Markuperweiterung ist die XAML-Entsprechung der C#- [ `typeof` ](/dotnet/csharp/language-reference/keywords/typeof/) Schlüsselwort. Wird von unterstützt die [ `TypeExtension` ](xref:Xamarin.Forms.Xaml.TypeExtension) Klasse, die eine Eigenschaft mit dem Namen definiert [ `TypeName` ](xref:Xamarin.Forms.Xaml.TypeExtension.TypeName) des Typs `string` , die auf eine Klasse oder Struktur Name festgelegt wird. Die `x:Type` Markup Extension gibt die [ `System.Type` ](xref:System.Type) Objekt der Klasse oder Struktur. `TypeName` die Inhaltseigenschaft ist `TypeExtension`, sodass `TypeName=` ist nicht erforderlich, wenn `x:Type` mit geschweiften Klammern angezeigt wird.

In Xamarin.Forms, es gibt mehrere Eigenschaften, die Argumente des Typs haben `Type`. Beispiele hierfür sind die [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) Eigenschaft `Style`, und die [X: TypeArguments](~/xamarin-forms/xaml/passing-arguments.md#generic_type_arguments) Attribut zur Angabe von Argumenten in generischen Klassen verwendet. Der XAML-Parser führt jedoch die `typeof` Vorgang automatisch, und die `x:Type` Markuperweiterung wird in diesen Fällen nicht verwendet.

Einem zentralen Ort, in denen `x:Type` *ist* erforderlich ist, mit der `x:Array` Markuperweiterung, die in beschrieben wird die [nächsten Abschnitt](#array).

Die `x:Type` Markuperweiterung ist auch nützlich, wenn ein Menü zu erstellen, wobei ein Objekt eines bestimmten Typs jedes Elements entspricht. Sie können Zuordnen einer `Type` -Objekt mit jedes Menüelement, und klicken Sie dann das Objekt zu instanziieren, wenn das Menüelement aktiviert ist.

Dies ist wie das Navigationsmenü im `MainPage` in die **Markuperweiterungen** Programm funktioniert. Die **"MainPage.xaml"** -Datei enthält eine `TableView` mit jedem `TextCell` für eine bestimmte Seite in das Programm:

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

Hier ist die öffnendes-Hauptseite in **Markuperweiterungen**:

[![Hauptseite](consuming-images/mainpage-small.png "Hauptseite")](consuming-images/mainpage-large.png#lightbox "Hauptseite")

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

Die `NavigateCommand` -Eigenschaft ist eine `Command` Objekt, das einen Execute-Befehl mit einem Argument des Typs implementiert `Type` &mdash; den Wert der `CommandParameter`. Die Methode verwendet `Activator.CreateInstance` auf die Seite zu instanziieren und anschließend darauf. Der Konstruktor wird abgeschlossen, indem Sie die Einstellung der `BindingContext` Seitenrand, um sich selbst, wodurch die `Binding` auf `Command` funktioniert. Finden Sie unter den [ **Datenbindung** ](~/xamarin-forms/app-fundamentals/data-binding/index.md) Artikel und insbesondere die [ **Commanding** ](~/xamarin-forms/app-fundamentals/data-binding/commanding.md) finden Sie weitere Details über diese Art von Code.

Die **X: Type-Demo** Seite verwendet ein ähnliches Verfahren aus, um die Xamarin.Forms-Elemente zu instanziieren und zum Hinzufügen einer `StackLayout`. Die XAML-Datei zunächst besteht aus drei `Button` Elemente mit ihren `Command` Eigenschaften festgelegt, um eine `Binding` und `CommandParameter` Eigenschaften, die Typen von drei Xamarin.Forms-Ansichten:

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

Die Methode, ausgeführt, wenn eine `Button` gedrückt wird erstellt eine neue Instanz des Arguments, legt die `VerticalOptions` -Eigenschaft und fügt es der `StackLayout`. Die drei `Button` Elemente teilen sich die Seite klicken Sie dann mit dynamisch erstellten Ansichten:

[![x:TypDEMO](consuming-images/typedemo-small.png "x:TypDEMO")](consuming-images/typedemo-large.png#lightbox "x:TypDEMO")

<a name="array" />

## <a name="xarray-markup-extension"></a>x:Array-Markuperweiterung

Die `x:Array` Markuperweiterung können Sie ein Array im Markup zu definieren. Wird von unterstützt die [ `ArrayExtension` ](xref:Xamarin.Forms.Xaml.ArrayExtension) Klasse, die zwei Eigenschaften definiert:

- `Type` Der Typ `Type`, die den Typ der Elemente im Array angibt.
- `Items` Der Typ `IList`, dies ist eine Auflistung der Elemente selbst. Dies ist die Content-Eigenschaft des `ArrayExtension`.

Die `x:Array` Markuperweiterung selbst nie in geschweiften Klammern angezeigt wird. Stattdessen `x:Array` Start- und Endtags trennen Sie die Liste der Elemente. Legen Sie die `Type` Eigenschaft, um eine `x:Type` Markuperweiterung.

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

Die `ViewCell` erstellt ein einfaches `BoxView` für jeden Eintrag Farbe:

[![x:Array-Demo](consuming-images/arraydemo-small.png "x:Array-Demo")](consuming-images/arraydemo-large.png#lightbox "x:Array-Demo")

Es gibt mehrere Möglichkeiten, geben Sie die einzelnen `Color` Elemente in diesem Array. Sie können eine `x:Static` Markuperweiterung:

```xaml
<x:Static Member="Color.Blue" />
```

Sie können auch `StaticResource` eine Farbe aus einem Ressourcenverzeichnis abrufen:

```xaml
<StaticResource Key="myColor" />
```

Am Ende dieses Artikels sehen Sie eine benutzerdefinierte Markuperweiterung in XAML, die auch einen neuen Wert für die Farbe erstellt:

```xaml
<local:HslColor H="0.5" S="1.0" L="0.5" />
```

Wenn Arrays von gemeinsamen Typen wie Zeichenfolgen oder Zahlen zu definieren, verwenden Sie die Tags im der [ **Konstruktorargument übergeben** ](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments) Artikel, um die Werte zu trennen.

<a name="null" />

## <a name="xnull-markup-extension"></a>x:Null-Markuperweiterung

Die `x:Null` Markuperweiterung wird unterstützt, indem die [ `NullExtension` ](xref:Xamarin.Forms.Xaml.NullExtension) Klasse. Er verfügt über keine Eigenschaften und ist einfach die XAML-Entsprechung der C#- [ `null` ](/dotnet/csharp/language-reference/keywords/null/) Schlüsselwort.

Die `x:Null` Markuperweiterung ist nur selten benötigt und selten verwendet, aber wenn Sie die Notwendigkeit von es finden können, werden Sie froh, die es vorhanden ist.

Die **X: Null-Demo** Seite veranschaulicht ein Szenario bei `x:Null` kann zweckmäßig sein. Nehmen wir an, dass Sie ein implizites definieren `Style` für `Label` , enthält eine `Setter` festlegt, die die `FontFamily` Eigenschaft, um einen Familiennamen plattformabhängige:

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

Und Sie, die für eine der ermittelt der `Label` Elemente sollen alle Einstellungen der Eigenschaften in den impliziten `Style` mit Ausnahme von der `FontFamily`, die als Standardwert verwendet werden sollen. Sie definieren eine andere `Style` für diesen Zweck jedoch ein einfacherer Ansatz besteht darin, legen Sie die `FontFamily` Eigenschaft des entsprechenden `Label` zu `x:Null`, wie in der Mitte `Label`.

Dies ist das Programm, das ausgeführt wird:

[![x:NULL-Demo](consuming-images/nulldemo-small.png "x:NULL-Demo")](consuming-images/nulldemo-large.png#lightbox "x:NULL-Demo")

Beachten Sie, dass diese vier von der `Label` Elemente haben eine Serifenschriftart jedoch das Center `Label` hat die Standard-Schriftart sans-Serif.

<a name="onplatform" />

## <a name="onplatform-markup-extension"></a>Onplatform-Markup Erweiterung

Die `OnPlatform` Markuperweiterung können Sie UI-Darstellung auf einer Basis pro Plattform anpassen. Es bietet die gleiche Funktionalität wie die [ `OnPlatform` ](xref:Xamarin.Forms.OnPlatform`1) und [ `On` ](xref:Xamarin.Forms.On) Klassen, jedoch eine präzisere Darstellung.

Die `OnPlatform` Markuperweiterung wird unterstützt, indem die [ `OnPlatformExtension` ](xref:Xamarin.Forms.Xaml.OnPlatformExtension) -Klasse, die die folgenden Eigenschaften definiert:

- `Default` Der Typ `object`, dass Sie auf einen Standardwert festlegen, auf die Eigenschaften angewendet werden, die Plattformen darstellen.
- `Android` Der Typ `object`, die Sie auf einen Wert festlegen, für Android verwendet werden soll.
- `GTK` Der Typ `object`, die Sie auf einen Wert festlegen, für GTK-Plattformen verwendet werden soll.
- `iOS` Der Typ `object`, die Sie auf einen Wert festlegen, für iOS verwendet werden soll.
- `macOS` Der Typ `object`, die Sie auf einen Wert festlegen, unter MacOS angewendet werden.
- `Tizen` Der Typ `object`, die Sie auf einen Wert festlegen, auf der Plattform Tizen angewendet werden.
- `UWP` Der Typ `object`, die Sie auf einen Wert festlegen, für die universelle Windows-Plattform verwendet werden soll.
- `WPF` Der Typ `object`, die Sie auf einen Wert festlegen, für die Windows Presentation Foundation-Plattform verwendet werden soll.
- `Converter` Der Typ `IValueConverter`, die Sie festlegen, um eine `IValueConverter` Implementierung.
- `ConverterParameter` Der Typ `object`, die Sie auf einen Wert festlegen, Übergabe an die `IValueConverter` Implementierung.

> [!NOTE]
> Der XAML-Parser lässt die [ `OnPlatformExtension` ](xref:Xamarin.Forms.Xaml.OnPlatformExtension) Klasse als abgekürzt werden `OnPlatform`.

Die `Default` -Eigenschaft ist die Content-Eigenschaft des `OnPlatformExtension`. Aus diesem Grund für XAML-Markup-Ausdrücke mit geschweiften Klammern ausgedrückt, Sie können vermeiden, die `Default=` Teil des Ausdrucks, vorausgesetzt, dass es sich um das erste Argument ist. Wenn die `Default`-Eigenschaft nicht festgelegt ist, wird standardmäßig der-Eigenschafts Wert [`BindableProperty.DefaultValue`](xref:Xamarin.Forms.BindableProperty.DefaultValue) verwendet, vorausgesetzt, dass die Markup Erweiterung auf eine [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)abzielt.

> [!IMPORTANT]
> Der XAML-Parser wird vorausgesetzt, dass die Werte des richtigen Typs zu Eigenschaften, die Nutzung bereitgestellt werden die `OnPlatform` Markuperweiterung. Wenn typkonvertierung erforderlich ist, ist die `OnPlatform` Markuperweiterung versucht, die sie mit die Standard-Konverter von Xamarin.Forms bereitgestelltes ausführen. Es gibt jedoch einige Typumwandlungen, die durch die Standard-Konverter und in diesen Fällen nicht ausgeführt werden können die `Converter` Eigenschaft sollte festgelegt werden, um eine `IValueConverter` Implementierung.

Die **OnPlatform-Demo** Seite zeigt, wie die `OnPlatform` Markuperweiterung:

```xaml
<BoxView Color="{OnPlatform Yellow, iOS=Red, Android=Green, UWP=Blue}"
         WidthRequest="{OnPlatform 250, iOS=200, Android=300, UWP=400}"  
         HeightRequest="{OnPlatform 250, iOS=200, Android=300, UWP=400}"
         HorizontalOptions="Center" />
```

In diesem Beispiel alle drei `OnPlatform` Ausdrücke verwenden, die Kurzform der `OnPlatformExtension` Klassenname. Die drei `OnPlatform` Markup Extensions Satz der [ `Color` ](xref:Xamarin.Forms.BoxView.Color), [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest), und [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) Eigenschaften der [ `BoxView` ](xref:Xamarin.Forms.BoxView) auf iOS-, Android- und UWP unterschiedliche Werte. Markuperweiterungen geben auch Standardwerte für diese Eigenschaften auf den Plattformen, die angegeben sind, sodass die `Default=` Teil des Ausdrucks. Beachten Sie, dass die Markup-Erweiterungseigenschaften, die festgelegt werden, die durch Kommas getrennt werden.

Dies ist das Programm, das ausgeführt wird:

[![Onplatform-Demo](consuming-images/onplatformdemo-small.png "Onplatform-Demo")](consuming-images/onplatformdemo-large.png#lightbox "Onplatform-Demo")

<a name="onidiom" />

## <a name="onidiom-markup-extension"></a>Onidiom-Markup Erweiterung

Mit der `OnIdiom` Markup Erweiterung können Sie die Darstellung der Benutzeroberfläche basierend auf dem Erscheinungsbild des Geräts anpassen, auf dem die Anwendung ausgeführt wird. Wird von unterstützt die [ `OnIdiomExtension` ](xref:Xamarin.Forms.Xaml.OnIdiomExtension) -Klasse, die die folgenden Eigenschaften definiert:

- `Default` Der Typ `object`, dass Sie auf einen Standardwert festlegen, auf die Eigenschaften angewendet werden, die Geräte-Ausdrücke darstellen.
- `Phone` Der Typ `object`, die Sie auf einen Wert festlegen, auf Telefonen angewendet werden.
- `Tablet` Der Typ `object`, die Sie auf einen Wert festlegen, auf Tablets angewendet werden.
- `Desktop` Der Typ `object`, die Sie auf einen Wert festlegen, für die desktop-Plattformen verwendet werden soll.
- `TV` Der Typ `object`, die Sie auf einen Wert festlegen, für TV-Plattformen verwendet werden soll.
- `Watch` Der Typ `object`, die Sie auf einen Wert festlegen, für die Watch-Plattformen verwendet werden soll.
- `Converter` Der Typ `IValueConverter`, die Sie festlegen, um eine `IValueConverter` Implementierung.
- `ConverterParameter` Der Typ `object`, die Sie auf einen Wert festlegen, Übergabe an die `IValueConverter` Implementierung.

> [!NOTE]
> Der XAML-Parser lässt die [ `OnIdiomExtension` ](xref:Xamarin.Forms.Xaml.OnIdiomExtension) Klasse als abgekürzt werden `OnIdiom`.

Die `Default` -Eigenschaft ist die Content-Eigenschaft des `OnIdiomExtension`. Aus diesem Grund für XAML-Markup-Ausdrücke mit geschweiften Klammern ausgedrückt, Sie können vermeiden, die `Default=` Teil des Ausdrucks, vorausgesetzt, dass es sich um das erste Argument ist.

> [!IMPORTANT]
> Der XAML-Parser wird vorausgesetzt, dass die Werte des richtigen Typs zu Eigenschaften, die Nutzung bereitgestellt werden die `OnIdiom` Markuperweiterung. Wenn typkonvertierung erforderlich ist, ist die `OnIdiom` Markuperweiterung versucht, die sie mit die Standard-Konverter von Xamarin.Forms bereitgestelltes ausführen. Es gibt jedoch einige Typumwandlungen, die durch die Standard-Konverter und in diesen Fällen nicht ausgeführt werden können die `Converter` Eigenschaft sollte festgelegt werden, um eine `IValueConverter` Implementierung.

Die **OnIdiom Demo** Seite zeigt, wie die `OnIdiom` Markuperweiterung:

```xaml
<BoxView Color="{OnIdiom Yellow, Phone=Red, Tablet=Green, Desktop=Blue}"
         WidthRequest="{OnIdiom 100, Phone=200, Tablet=300, Desktop=400}"
         HeightRequest="{OnIdiom 100, Phone=200, Tablet=300, Desktop=400}"
         HorizontalOptions="Center" />
```

In diesem Beispiel alle drei `OnIdiom` Ausdrücke verwenden, die Kurzform der `OnIdiomExtension` Klassenname. Die drei `OnIdiom` Markup Extensions Satz der [ `Color` ](xref:Xamarin.Forms.BoxView.Color), [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest), und [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) Eigenschaften der [ `BoxView` ](xref:Xamarin.Forms.BoxView) auf unterschiedliche Werte auf das Telefon, Tablet und desktop-Ausdrücke. Markuperweiterungen geben auch Standardwerte für diese Eigenschaften für die Ausdrücke, die angegeben sind, sodass die `Default=` Teil des Ausdrucks. Beachten Sie, dass die Markup-Erweiterungseigenschaften, die festgelegt werden, die durch Kommas getrennt werden.

Dies ist das Programm, das ausgeführt wird:

[![Onidiom-Demo](consuming-images/onidiomdemo-small.png "Onidiom-Demo")](consuming-images/onidiomdemo-large.png#lightbox "Onidiom-Demo")

## <a name="datatemplate-markup-extension"></a>DataTemplate-Markup Erweiterung

Mit der `DataTemplate` Markup Erweiterung können Sie einen Typ in einen [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)konvertieren. Sie wird von der `DataTemplateExtension`-Klasse unterstützt, die eine `TypeName`-Eigenschaft des Typs `string`definiert, die auf den Namen des Typs festgelegt ist, der in eine `DataTemplate`konvertiert werden soll. Die `TypeName` -Eigenschaft ist die Content-Eigenschaft des `DataTemplateExtension`. Daher können Sie bei XAML-Markupausdrücken (die mit geschweiften Klammern ausgedrückt werden) den `TypeName=`-Teil des Ausdrucks entfernen.

> [!NOTE]
> Im XAML-Parser kann die Klasse `DataTemplateExtension` zu `DataTemplate` abgekürzt werden.

Eine typische Verwendung dieser Markup Erweiterung ist in einer Shellanwendung, wie im folgenden Beispiel gezeigt:

```xaml
<ShellContent Title="Monkeys"
              Icon="monkey.png"
              ContentTemplate="{DataTemplate views:MonkeysPage}" />
```

In diesem Beispiel wird `MonkeysPage` von einem [`ContentPage`](xref:Xamarin.Forms.ContentPage) in einen [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)konvertiert, der als Wert der `ShellContent.ContentTemplate`-Eigenschaft festgelegt wird. Dadurch wird sichergestellt, dass `MonkeysPage` nur erstellt wird, wenn die Navigation zur Seite statt beim Starten der Anwendung erfolgt.

Weitere Informationen zu Shellanwendungen finden Sie unter [xamarin. Forms Shell](~/xamarin-forms/app-fundamentals/shell/index.md).

## <a name="fontimage-markup-extension"></a>Fontimage-Markup Erweiterung

Mit der `FontImage` Markup Erweiterung können Sie ein Schriftart Symbol in jeder Ansicht anzeigen, die eine `ImageSource`anzeigen kann. Sie bietet die gleiche Funktionalität wie die `FontImageSource`-Klasse, bietet aber eine präzisere Darstellung.

Die Markuperweiterung `FontImage` wird von der Klasse `FontImageExtension` unterstützt, in der die folgenden Eigenschaften definiert werden:

- `FontFamily` vom Typ `string`, die Schriftfamilie, zu der das Schriftart Symbol gehört.
- `Glyph` vom Typ `string`, der Unicode-Zeichen Wert des Schriftart Symbols.
- `Color` vom Typ [`Color`](xref:Xamarin.Forms.Color), die Farbe, die beim Anzeigen des Schriftart Symbols verwendet werden soll.
- `Size` vom Typ `double`die Größe des gerenderten Schriftart Symbols in geräteunabhängigen Einheiten. Der Standardwert ist 30. Außerdem kann diese Eigenschaft auf eine benannte Schriftgröße festgelegt werden.

> [!NOTE]
> Im XAML-Parser kann die Klasse `FontImageExtension` zu `FontImage` abgekürzt werden.

Die `Glyph` -Eigenschaft ist die Content-Eigenschaft des `FontImageExtension`. Aus diesem Grund für XAML-Markup-Ausdrücke mit geschweiften Klammern ausgedrückt, Sie können vermeiden, die `Glyph=` Teil des Ausdrucks, vorausgesetzt, dass es sich um das erste Argument ist.

Die exemplarische Vorgehensweise " **fontimage** " zeigt, wie die `FontImage` Markup Erweiterung verwendet wird:

```xaml
<Image BackgroundColor="#D1D1D1"
       Source="{FontImage &#xf30c;, FontFamily={OnPlatform iOS=Ionicons, Android=ionicons.ttf#}, Size=44}" />
```

In diesem Beispiel wird die abgekürzte Version des `FontImageExtension` Klassen namens verwendet, um ein Xbox-Symbol aus der Schriftfamilie "ionicons" in einer [`Image`](xref:Xamarin.Forms.Image)anzuzeigen. Der Ausdruck verwendet auch die `OnPlatform` Markup Erweiterung, um unterschiedliche `FontFamily` Eigenschaftswerte unter IOS und Android anzugeben. Außerdem wird der `Glyph=` Teil des Ausdrucks gelöscht, und die festgelegten Markup Erweiterungs Eigenschaften werden durch Kommas getrennt. Beachten Sie, dass das Unicode-Zeichen für das Symbol zwar `\uf30c`, aber in XAML mit Escapezeichen versehen werden muss, sodass es `&#xf30c;`wird.

Dies ist das Programm, das ausgeführt wird:

[![Screenshot der fontimage-Markup Erweiterung](consuming-images/fontimagedemo.png "Fontimage-Demo")](consuming-images/fontimagedemo-large.png#lightbox "Fontimage-Demo")

Informationen zum Anzeigen von Schriftart Symbolen durch Angeben der Schriftart Symbol Daten in einem `FontImageSource` Objekt finden Sie unter [Anzeigen von Schriftart Symbolen](~/xamarin-forms/user-interface/text/fonts.md#display-font-icons).

## <a name="define-your-own-markup-extensions"></a>Definieren Sie Ihre eigenen Markuperweiterungen

Wenn Sie müssen für eine XAML-Markuperweiterung, die nicht in Xamarin.Forms verfügbar ist gefunden haben, können Sie [erstellen Sie Ihre eigenen](creating.md).

## <a name="related-links"></a>Verwandte Themen

- [Markuperweiterungen (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)
- [XAML-Markup Extensions Kapitel von Xamarin.Forms-Buch](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [Ressourcenverzeichnisse](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Dynamische Stile](~/xamarin-forms/user-interface/styles/dynamic.md)
- [Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Xamarin. Forms-Shell](~/xamarin-forms/app-fundamentals/shell/index.md).
