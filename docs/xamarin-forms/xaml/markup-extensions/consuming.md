---
title: Verwenden von XAML-Markuperweiterungen
description: In diesem Artikel wird erläutert, wie xamarin. Forms-XAML-Markup Erweiterungen verwendet werden, um die Leistungsfähigkeit und Flexibilität von XAML zu verbessern, indem Element Attribute aus einer Vielzahl von Quellen festgelegt werden können.
ms.prod: xamarin
ms.assetid: CE686893-609C-4EC3-9225-6C68D2A9F79C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/21/2020
ms.openlocfilehash: 7f46bc84543a1838241923ab91809bd01c706f44
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517090"
---
# <a name="consuming-xaml-markup-extensions"></a>Verwenden von XAML-Markuperweiterungen

[![Beispiel](~/media/shared/download.png) herunterladen herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)

XAML-Markup Erweiterungen helfen, die Leistungsfähigkeit und Flexibilität von XAML zu verbessern, indem die Festlegung von Element Attributen aus einer Vielzahl von Quellen ermöglicht wird. Mehrere XAML-Markup Erweiterungen sind Teil der XAML 2009-Spezifikation. Diese werden in XAML-Dateien mit dem `x` entsprechenden Namespace Präfix angezeigt und werden häufig mit diesem Präfix bezeichnet. In diesem Artikel werden die folgenden Markup Erweiterungen erläutert:

- [`x:Static`](#static)– verweisen auf statische Eigenschaften, Felder oder Enumerationsmember.
- [`x:Reference`](#reference)– verweist auf benannte Elemente auf der Seite.
- [`x:Type`](#type)– Legen Sie ein Attribut auf `System.Type` ein-Objekt fest.
- [`x:Array`](#array)– Erstellen Sie ein Array von Objekten eines bestimmten Typs.
- [`x:Null`](#null)– Legen Sie ein Attribut auf `null` einen Wert fest.
- [`OnPlatform`](#onplatform)– passen Sie die Darstellung der Benutzeroberfläche plattformspezifisch an.
- [`OnIdiom`](#onidiom)– Anpassen der Darstellung der Benutzeroberfläche basierend auf dem Erscheinungsbild des Geräts, auf dem die Anwendung ausgeführt wird.
- [`DataTemplate`](#datatemplate-markup-extension)– Konvertiert einen-Typ in [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)einen-Typ.
- [`FontImage`](#fontimage-markup-extension)– zeigt ein Schriftart Symbol in einer beliebigen Ansicht an, in `ImageSource`der ein angezeigt werden kann.
- [`OnAppTheme`](#onapptheme-markup-extension)– verwendet eine Ressource basierend auf dem aktuellen Systemdesign.

Weitere XAML-Markup Erweiterungen wurden in der Vergangenheit von anderen XAML-Implementierungen unterstützt und werden auch von xamarin. Forms unterstützt. Diese werden in anderen Artikeln ausführlicher beschrieben:

- `StaticResource`-Verweis Objekte aus einem Ressourcen Wörterbuch, wie im Artikel [**Ressourcen Wörterbücher**](~/xamarin-forms/xaml/resource-dictionaries.md)beschrieben.
- `DynamicResource`-reagieren Sie auf Änderungen in Objekten in einem Ressourcen Wörterbuch, wie im Artikel [**dynamische Stile**](~/xamarin-forms/user-interface/styles/dynamic.md)beschrieben.
- `Binding`-Erstellen Sie eine Verknüpfung zwischen Eigenschaften von zwei-Objekten, wie im Artikel [**Datenbindung**](~/xamarin-forms/app-fundamentals/data-binding/index.md)beschrieben.
- `TemplateBinding`: führt die Datenbindung aus einer Steuerelement Vorlage aus, wie im Artikel [**xamarin. Forms-Steuerelement Vorlagen**](~/xamarin-forms/app-fundamentals/templates/control-template.md)erläutert.
- `RelativeSource`: legt die Bindungs Quelle relativ zur Position des Bindungs Ziels fest, wie im Artikel [relative Bindungen](~/xamarin-forms/app-fundamentals/data-binding/relative-bindings.md)erläutert.

Das [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) Layout verwendet die benutzerdefinierte Markup Erweiterung [`ConstraintExpression`](xref:Xamarin.Forms.ConstraintExpression). Diese Markup Erweiterung wird im Artikel [**relativelayout**](~/xamarin-forms/user-interface/layouts/relative-layout.md)beschrieben.

<a name="static" />

## <a name="xstatic-markup-extension"></a>x:Static-Markuperweiterung

Die `x:Static` Markup Erweiterung wird von der [`StaticExtension`](xref:Xamarin.Forms.Xaml.StaticExtension) -Klasse unterstützt. Die-Klasse verfügt über eine einzelne [`Member`](xref:Xamarin.Forms.Xaml.StaticExtension.Member) Eigenschaft namens `string` vom Typ, die Sie auf den Namen einer öffentlichen Konstante, einer statischen Eigenschaft, eines statischen Felds oder eines Enumerationsmembers festlegen.

Eine gängige Methode zur Verwendung `x:Static` von besteht darin, zuerst eine Klasse mit einigen Konstanten oder statischen Variablen zu definieren, z `AppConstants` . b. diese kleine Klasse im [**MarkupExtensions**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions) -Programm:

```csharp
static class AppConstants
{
    public static double NormalFontSize = 18;
}
```

Die **x:static-Demoseite** zeigt verschiedene Möglichkeiten zur Verwendung `x:Static` der Markup Erweiterung. Der ausführlichste Ansatz instanziiert die `StaticExtension` -Klasse zwischen `Label.FontSize` Eigenschaften-Element-Tags:

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

Der XAML-Parser ermöglicht außerdem `StaticExtension` die Abkürzung der-Klasse `x:Static`:

```xaml
<Label Text="Label No. 2">
    <Label.FontSize>
        <x:Static Member="local:AppConstants.NormalFontSize" />
    </Label.FontSize>
</Label>
```

Dies kann noch weiter vereinfacht werden, aber die Änderung führt zu einer neuen Syntax: Sie besteht aus dem `StaticExtension` platzieren der Klasse und der Member-Einstellung in geschweiften Klammern. Der resultierende Ausdruck wird direkt auf das `FontSize` -Attribut festgelegt:

```xaml
<Label Text="Label No. 3"
       FontSize="{x:StaticExtension Member=local:AppConstants.NormalFontSize}" />
```

Beachten Sie, dass es *keine* Anführungszeichen innerhalb der geschweiften Klammern gibt. Die `Member` -Eigenschaft `StaticExtension` von ist kein XML-Attribut mehr. Er ist stattdessen ein Teil des Ausdrucks für die Markup Erweiterung.

Ebenso wie Sie die abkürzen `x:StaticExtension` können `x:Static` , wenn Sie Sie als Objekt Element verwenden, können Sie Sie auch im Ausdruck in geschweiften Klammern abkürzen:

```xaml
<Label Text="Label No. 4"
       FontSize="{x:Static Member=local:AppConstants.NormalFontSize}" />
```

Die `StaticExtension` -Klasse verfügt `ContentProperty` über ein-Attribut, `Member`das auf die-Eigenschaft verweist, die diese Eigenschaft als Standard Inhalts Eigenschaft der Klasse kennzeichnet. Bei XAML-Markup Erweiterungen, die mit geschweiften Klammern ausgedrückt werden, `Member=` können Sie den Teil des Ausdrucks entfernen:

```xaml
<Label Text="Label No. 5"
       FontSize="{x:Static local:AppConstants.NormalFontSize}" />
```

Dies ist die häufigste Form der `x:Static` Markup Erweiterung.

Die **statische Demoseite** enthält zwei weitere Beispiele. Das Stammtag der XAML-Datei enthält eine XML-Namespace Deklaration für `System` den .NET-Namespace:

```xaml
xmlns:sys="clr-namespace:System;assembly=netstandard"
```

Dadurch kann die `Label` Schriftgröße auf das statische Feld `Math.PI`festgelegt werden. Das führt zu einem eher kleinen Text, sodass `Scale` die-Eigenschaft auf `Math.E`festgelegt ist:

```xaml
<Label Text="&#x03C0; &#x00D7; E sized text"
       FontSize="{x:Static sys:Math.PI}"
       Scale="{x:Static sys:Math.E}"
       HorizontalOptions="Center" />
```

Im letzten Beispiel wird der `Device.RuntimePlatform` -Wert angezeigt. Die `Environment.NewLine` statische-Eigenschaft wird verwendet, um ein neue-Zeile-Zeichen zwischen `Span` den beiden-Objekten einzufügen:

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

Die `x:Reference` Markup Erweiterung wird von der [`ReferenceExtension`](xref:Xamarin.Forms.Xaml.ReferenceExtension) -Klasse unterstützt. Die-Klasse verfügt über eine einzelne [`Name`](xref:Xamarin.Forms.Xaml.ReferenceExtension.Name) -Eigenschaft `string` mit dem Namen vom Typ, die Sie auf den Namen eines Elements auf der Seite festlegen, dem `x:Name`ein Name zugewiesen wurde. Diese `Name` Eigenschaft ist die Content-Eigenschaft `ReferenceExtension`von und `Name=` ist daher nicht erforderlich `x:Reference` , wenn in geschweiften Klammern angezeigt wird.

Die `x:Reference` Markup Erweiterung wird ausschließlich mit Daten Bindungen verwendet, die im Artikel [**Datenbindung**](~/xamarin-forms/app-fundamentals/data-binding/index.md)ausführlicher beschrieben werden.

Die **Demo Seite "x:Reference** " zeigt zwei `x:Reference` Verwendungen von mit Daten Bindungen, der erste, in dem er `Source` verwendet wird, `Binding` um die-Eigenschaft des-Objekts festzulegen, und der `BindingContext` zweite, in dem er verwendet wird, um die-Eigenschaft für zwei Daten Bindungen festzulegen:

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

Beide `x:Reference` Ausdrücke verwenden die abgekürzte Version `ReferenceExtension` des Klassen namens und entfernen `Name=` den Teil des Ausdrucks. Im ersten Beispiel ist die `x:Reference` Markup Erweiterung in der `Binding` Markup Erweiterung eingebettet. Beachten Sie, `Source` dass `StringFormat` die Einstellungen und durch Kommas getrennt sind. Dies ist das Programm, das ausgeführt wird:

[![x:verweisdemo](consuming-images/referencedemo-small.png "x:verweisdemo")](consuming-images/referencedemo-large.png#lightbox "x:verweisdemo")

<a name="type" />

## <a name="xtype-markup-extension"></a>x:Type-Markuperweiterung

Die `x:Type` Markup Erweiterung ist die XAML-Entsprechung des c# [`typeof`](/dotnet/csharp/language-reference/keywords/typeof/) -Schlüssel Worts. Sie wird von der [`TypeExtension`](xref:Xamarin.Forms.Xaml.TypeExtension) -Klasse unterstützt, die eine Eigenschaft [`TypeName`](xref:Xamarin.Forms.Xaml.TypeExtension.TypeName) mit dem `string` Namen des Typs definiert, die auf einen Klassen-oder Struktur Namen festgelegt ist. Die `x:Type` Markup Erweiterung gibt das [`System.Type`](xref:System.Type) -Objekt dieser Klasse oder Struktur zurück. `TypeName`ist die Content-Eigenschaft `TypeExtension`von. `TypeName=` Dies ist daher nicht `x:Type` erforderlich, wenn mit geschweiften Klammern angezeigt wird.

In xamarin. Forms gibt es mehrere Eigenschaften, die über Argumente des Typs `Type`verfügen. Beispiele hierfür sind [`TargetType`](xref:Xamarin.Forms.Style.TargetType) die- `Style`Eigenschaft von und das [x:TypeArguments](~/xamarin-forms/xaml/passing-arguments.md#generic_type_arguments) -Attribut, mit dem Argumente in generischen Klassen angegeben werden. Der XAML-Parser führt den `typeof` Vorgang jedoch automatisch aus, und `x:Type` die Markup Erweiterung wird in diesen Fällen nicht verwendet.

Eine Stelle `x:Type` , an *der erforderlich ist, ist die* `x:Array` Markup Erweiterung, die im [nächsten Abschnitt](#array)beschrieben wird.

Die `x:Type` Markup Erweiterung ist auch nützlich, wenn ein Menü erstellt wird, in dem jedes Menü Element einem Objekt eines bestimmten Typs entspricht. Sie können jedem Menü `Type` Element ein-Objekt zuordnen und dann das-Objekt instanziieren, wenn das Menü Element ausgewählt wird.

Auf diese Weise funktioniert das Navigationsmenü `MainPage` in im **Markup Extensions** -Programm. Die Datei " **MainPage. XAML** " `TableView` enthält einen `TextCell` , der jeweils einer bestimmten Seite im Programm entspricht:

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

Jede `CommandParameter` Eigenschaft wird auf eine `x:Type` Markup Erweiterung festgelegt, die auf eine der anderen Seiten verweist. Die `Command` -Eigenschaft ist an eine Eigenschaft mit `NavigateCommand`dem Namen gebunden. Diese Eigenschaft wird in der `MainPage` Code Behind-Datei definiert:

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

Bei `NavigateCommand` der-Eigenschaft `Command` handelt es sich um ein-Objekt, das einen Execute `Type` &mdash; -Befehl mit `CommandParameter`einem Argument vom Typ den Wert von implementiert. Die-Methode `Activator.CreateInstance` verwendet, um die Seite zu instanziieren und dann zu dieser zu navigieren. Der Konstruktor wird beendet, indem `BindingContext` der der Seite auf sich selbst festgelegt wird `Binding` , `Command` sodass das on funktioniert. Weitere Informationen zu dieser Art von Code finden Sie im Artikel [**Datenbindung**](~/xamarin-forms/app-fundamentals/data-binding/index.md) und insbesondere im Artikel zur [**Befehls**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md) Zeile.

Die **x:Type-Demoseite** verwendet eine ähnliche Technik, um xamarin. Forms-Elemente zu instanziieren und Sie `StackLayout`einem hinzuzufügen. Die XAML `Binding` -Datei besteht anfänglich `Button` aus drei Elementen `Command` , deren Eigenschaften auf festgelegt `CommandParameter` sind, und die Eigenschaften werden auf Typen von drei xamarin. Forms-Ansichten festgelegt:

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

Mit der Code-Behind-Datei wird die `CreateCommand` -Eigenschaft definiert und initialisiert:

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

Die Methode, die ausgeführt wird, `Button` wenn ein gedrückt wird, erstellt eine neue Instanz des Arguments, `VerticalOptions` legt die zugehörige-Eigenschaft fest `StackLayout`und fügt Sie hinzu. Die drei `Button` Elemente geben dann die Seite mit dynamisch erstellten Ansichten frei:

[![x:TypDEMO](consuming-images/typedemo-small.png "x:TypDEMO")](consuming-images/typedemo-large.png#lightbox "x:TypDEMO")

<a name="array" />

## <a name="xarray-markup-extension"></a>x:Array-Markuperweiterung

Die `x:Array` Markup Erweiterung ermöglicht es Ihnen, ein Array im Markup zu definieren. Sie wird von der [`ArrayExtension`](xref:Xamarin.Forms.Xaml.ArrayExtension) -Klasse unterstützt, die zwei Eigenschaften definiert:

- `Type`vom Typ `Type`, der den Typ der Elemente im Array angibt.
- `Items`vom Typ `IList`, bei dem es sich um eine Auflistung der Elemente selbst handelt. Dabei handelt es sich um die `ArrayExtension`Content-Eigenschaft von.

Die `x:Array` Markup Erweiterung selbst wird nie in geschweiften Klammern angezeigt. Stattdessen wird `x:Array` die Liste der Elemente von Start-und Endtags getrennt. Legen Sie `Type` die-Eigenschaft `x:Type` auf eine Markup Erweiterung fest.

Die **x:Array-Demo** Seite zeigt, wie `x:Array` Sie mit Elemente zu einem `ListView` hinzufügen, `ItemsSource` indem Sie die-Eigenschaft auf ein Array festlegen:

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

Der `ViewCell` erstellt einen einfachen `BoxView` für jeden Farb Eintrag:

[![x:Array-Demo](consuming-images/arraydemo-small.png "x:Array-Demo")](consuming-images/arraydemo-large.png#lightbox "x:Array-Demo")

Es gibt mehrere Möglichkeiten, die einzelnen `Color` Elemente in diesem Array anzugeben. Sie können eine `x:Static` Markup Erweiterung verwenden:

```xaml
<x:Static Member="Color.Blue" />
```

Sie können auch verwenden `StaticResource` , um eine Farbe aus einem Ressourcen Wörterbuch abzurufen:

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

Die `x:Null` Markup Erweiterung wird von der [`NullExtension`](xref:Xamarin.Forms.Xaml.NullExtension) -Klasse unterstützt. Sie verfügt über keine Eigenschaften und ist einfach die XAML-Entsprechung des [`null`](/dotnet/csharp/language-reference/keywords/null/) c#-Schlüssel Worts.

Die `x:Null` Markup Erweiterung ist nur selten erforderlich und wird selten verwendet. Wenn Sie jedoch einen Bedarf dafür feststellen, sind Sie froh, dass Sie vorhanden ist.

Die **Demo Seite x:NULL** veranschaulicht ein Szenario, `x:Null` in dem Sie möglicherweise bequem ist. Angenommen, Sie definieren eine implizite `Style` für `Label` , die einen `Setter` enthält, der `FontFamily` die-Eigenschaft auf einen Platt Form abhängigen Familiennamen festlegt:

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

Dann stellen Sie fest, dass Sie für `Label` eines der-Elemente alle Eigenschafts Einstellungen im impliziten `Style` `FontFamily`, mit Ausnahme von, benötigen, was der Standardwert sein soll. Sie können für diesen `Style` Zweck einen anderen definieren, aber ein einfacherer Ansatz besteht darin `FontFamily` , einfach die- `Label` Eigenschaft `x:Null`des jeweiligen auf festzulegen, `Label`wie in der Mitte veranschaulicht.

Dies ist das Programm, das ausgeführt wird:

[![x:NULL-Demo](consuming-images/nulldemo-small.png "x:NULL-Demo")](consuming-images/nulldemo-large.png#lightbox "x:NULL-Demo")

Beachten Sie, dass vier `Label` der-Elemente über eine "Serif"- `Label` Schriftart verfügen, das Center jedoch die standardmäßige Sans-Serif-Schriftart hat.

<a name="onplatform" />

## <a name="onplatform-markup-extension"></a>Onplatform-Markup Erweiterung

Die Markuperweiterung `OnPlatform` ermöglicht Ihnen das plattformspezifische Anpassen der Benutzeroberfläche. Sie bietet die gleiche Funktionalität wie die [`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) - [`On`](xref:Xamarin.Forms.On) Klasse und die-Klasse, jedoch mit einer präziseren Darstellung.

Die `OnPlatform` Markup Erweiterung wird von der [`OnPlatformExtension`](xref:Xamarin.Forms.Xaml.OnPlatformExtension) -Klasse unterstützt, die die folgenden Eigenschaften definiert:

- `Default`vom Typ `object`, die auf einen Standardwert festgelegt wird, der auf die Eigenschaften angewendet werden soll, die Plattformen darstellen.
- `Android`vom Typ `object`, den Sie auf einen Wert festlegen, der auf Android angewendet werden soll.
- `GTK`vom Typ `object`, den Sie auf einen Wert festlegen, der auf GTK-Plattformen angewendet werden soll.
- `iOS`vom Typ `object`, den Sie auf einen Wert festlegen, der auf IOS angewendet werden soll.
- `macOS`vom Typ `object`, den Sie auf einen Wert festlegen, der auf macOS angewendet werden soll.
- `Tizen`vom Typ `object`, den Sie auf einen Wert festlegen, der auf die tizen-Plattform angewendet werden soll.
- `UWP`vom Typ `object`, der auf einen Wert festgelegt wird, der auf die universelle Windows-Plattform angewendet werden soll.
- `WPF`vom Typ `object`, der auf einen Wert festgelegt wird, der auf die Windows Presentation Foundation Plattform angewendet werden soll.
- `Converter`vom Typ `IValueConverter`, die auf eine `IValueConverter` Implementierung festgelegt werden kann.
- `ConverterParameter`vom Typ `object`, die auf einen Wert festgelegt werden kann, der an `IValueConverter` die-Implementierung übergeben werden soll.

> [!NOTE]
> Der XAML-Parser ermöglicht [`OnPlatformExtension`](xref:Xamarin.Forms.Xaml.OnPlatformExtension) , dass die-Klasse `OnPlatform`als abgekürzt wird.

Die `Default` -Eigenschaft ist die Content- `OnPlatformExtension`Eigenschaft von. Daher können Sie für XAML-Markup Ausdrücke, die mit geschweiften Klammern ausgedrückt werden `Default=` , den Teil des Ausdrucks entfernen, vorausgesetzt, dass es sich um das erste Argument handelt. Wenn die `Default` -Eigenschaft nicht festgelegt ist, wird Standard [`BindableProperty.DefaultValue`](xref:Xamarin.Forms.BindableProperty.DefaultValue) mäßig der-Eigenschafts Wert verwendet, vorausgesetzt, [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)dass die Markup Erweiterung auf abzielt.

> [!IMPORTANT]
> Der XAML-Parser erwartet, dass die Werte des richtigen Typs für Eigenschaften bereitgestellt werden `OnPlatform` , die die Markup Erweiterung nutzen. Wenn eine Typkonvertierung erforderlich ist, `OnPlatform` versucht die Markup Erweiterung, Sie mit den von xamarin. Forms bereitgestellten Standard Konvertern auszuführen. Es gibt jedoch einige Typkonvertierungen, die von den Standard Konvertern nicht ausgeführt werden können. in `Converter` diesen Fällen sollte die-Eigenschaft `IValueConverter` auf eine-Implementierung festgelegt werden.

Die **onplatform-Demo** Seite zeigt, wie die `OnPlatform` Markup Erweiterung verwendet wird:

```xaml
<BoxView Color="{OnPlatform Yellow, iOS=Red, Android=Green, UWP=Blue}"
         WidthRequest="{OnPlatform 250, iOS=200, Android=300, UWP=400}"  
         HeightRequest="{OnPlatform 250, iOS=200, Android=300, UWP=400}"
         HorizontalOptions="Center" />
```

In diesem Beispiel verwenden alle drei `OnPlatform` Ausdrücke die abgekürzte Version des `OnPlatformExtension` Klassen namens. Die drei `OnPlatform` Markup Erweiterungen legen die [`Color`](xref:Xamarin.Forms.BoxView.Color)Eigenschaften [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest), und [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) von [`BoxView`](xref:Xamarin.Forms.BoxView) auf unterschiedliche Werte unter IOS, Android und UWP fest. Die Markup Erweiterungen stellen außerdem Standardwerte für diese Eigenschaften auf den Plattformen bereit, die nicht angegeben sind `Default=` , während der Teil des Ausdrucks eliminiert wird. Beachten Sie, dass die festgelegten Markup Erweiterungs Eigenschaften durch Kommas voneinander getrennt sind.

Dies ist das Programm, das ausgeführt wird:

[![Onplatform-Demo](consuming-images/onplatformdemo-small.png "Onplatform-Demo")](consuming-images/onplatformdemo-large.png#lightbox "Onplatform-Demo")

<a name="onidiom" />

## <a name="onidiom-markup-extension"></a>Onidiom-Markup Erweiterung

Die `OnIdiom` Markup Erweiterung ermöglicht es Ihnen, die Darstellung der Benutzeroberfläche basierend auf dem Erscheinungsbild des Geräts anzupassen, auf dem die Anwendung ausgeführt wird. Sie wird von der [`OnIdiomExtension`](xref:Xamarin.Forms.Xaml.OnIdiomExtension) -Klasse unterstützt, die die folgenden Eigenschaften definiert:

- `Default`vom Typ `object`: Sie legen auf einen Standardwert fest, der auf die Eigenschaften angewendet werden soll, die die Geräte Idiome darstellen.
- `Phone`vom Typ `object`, den Sie auf einen auf Smartphones anzuwendenden Wert festlegen.
- `Tablet`vom Typ `object`, den Sie auf einen auf Tablets anzuwendenden Wert festlegen.
- `Desktop`vom Typ `object`, den Sie auf einen auf Desktop Plattformen anzuwendenden Wert festlegen.
- `TV`vom Typ `object`, den Sie auf einen Wert festlegen, der auf TV-Plattformen angewendet werden soll.
- `Watch`vom Typ `object`, den Sie auf einen Wert festlegen, der auf Überwachungs Plattformen angewendet werden soll.
- `Converter`vom Typ `IValueConverter`, die auf eine `IValueConverter` Implementierung festgelegt werden kann.
- `ConverterParameter`vom Typ `object`, die auf einen Wert festgelegt werden kann, der an `IValueConverter` die-Implementierung übergeben werden soll.

> [!NOTE]
> Der XAML-Parser ermöglicht [`OnIdiomExtension`](xref:Xamarin.Forms.Xaml.OnIdiomExtension) , dass die-Klasse `OnIdiom`als abgekürzt wird.

Die `Default` -Eigenschaft ist die Content- `OnIdiomExtension`Eigenschaft von. Daher können Sie für XAML-Markup Ausdrücke, die mit geschweiften Klammern ausgedrückt werden `Default=` , den Teil des Ausdrucks entfernen, vorausgesetzt, dass es sich um das erste Argument handelt.

> [!IMPORTANT]
> Der XAML-Parser erwartet, dass die Werte des richtigen Typs für Eigenschaften bereitgestellt werden `OnIdiom` , die die Markup Erweiterung nutzen. Wenn eine Typkonvertierung erforderlich ist, `OnIdiom` versucht die Markup Erweiterung, Sie mit den von xamarin. Forms bereitgestellten Standard Konvertern auszuführen. Es gibt jedoch einige Typkonvertierungen, die von den Standard Konvertern nicht ausgeführt werden können. in `Converter` diesen Fällen sollte die-Eigenschaft `IValueConverter` auf eine-Implementierung festgelegt werden.

Die Seite " **onidiom Demo** " zeigt die Verwendung `OnIdiom` der Markup Erweiterung:

```xaml
<BoxView Color="{OnIdiom Yellow, Phone=Red, Tablet=Green, Desktop=Blue}"
         WidthRequest="{OnIdiom 100, Phone=200, Tablet=300, Desktop=400}"
         HeightRequest="{OnIdiom 100, Phone=200, Tablet=300, Desktop=400}"
         HorizontalOptions="Center" />
```

In diesem Beispiel verwenden alle drei `OnIdiom` Ausdrücke die abgekürzte Version des `OnIdiomExtension` Klassen namens. Die drei `OnIdiom` Markup Erweiterungen legen die [`Color`](xref:Xamarin.Forms.BoxView.Color)- [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)Eigenschaft, [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) die-Eigenschaft [`BoxView`](xref:Xamarin.Forms.BoxView) und die-Eigenschaft des auf die unterschiedlichen Werte auf der Telefon-, Tablet-und Desktop Idioms fest. Die Markup Erweiterungen stellen außerdem Standardwerte für diese Eigenschaften für die Idiome bereit, die nicht angegeben sind, `Default=` während der Teil des Ausdrucks eliminiert wird. Beachten Sie, dass die festgelegten Markup Erweiterungs Eigenschaften durch Kommas voneinander getrennt sind.

Dies ist das Programm, das ausgeführt wird:

[![Onidiom-Demo](consuming-images/onidiomdemo-small.png "Onidiom-Demo")](consuming-images/onidiomdemo-large.png#lightbox "Onidiom-Demo")

## <a name="datatemplate-markup-extension"></a>DataTemplate-Markup Erweiterung

Die `DataTemplate` Markup Erweiterung ermöglicht es Ihnen, einen Typ in einen [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)zu konvertieren. Sie wird `DataTemplateExtension` von der-Klasse unterstützt, die `TypeName` eine Eigenschaft des Typs `string`definiert, die auf den Namen des Typs festgelegt ist, der in eine `DataTemplate`konvertiert werden soll. Die `TypeName` -Eigenschaft ist die Content- `DataTemplateExtension`Eigenschaft von. Daher können Sie bei XAML-Markupausdrücken (die mit geschweiften Klammern ausgedrückt werden) den `TypeName=`-Teil des Ausdrucks entfernen.

> [!NOTE]
> Im XAML-Parser kann die Klasse `DataTemplateExtension` zu `DataTemplate` abgekürzt werden.

Eine typische Verwendung dieser Markup Erweiterung ist in einer Shellanwendung, wie im folgenden Beispiel gezeigt:

```xaml
<ShellContent Title="Monkeys"
              Icon="monkey.png"
              ContentTemplate="{DataTemplate views:MonkeysPage}" />
```

In diesem `MonkeysPage` Beispiel wird von [`ContentPage`](xref:Xamarin.Forms.ContentPage) einem- [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) `ShellContent.ContentTemplate` Objekt in ein-Objekt konvertiert, das als Wert der-Eigenschaft festgelegt wird. Dadurch wird sicher `MonkeysPage` gestellt, dass nur bei einem Anwendungsstart erstellt wird, wenn die Navigation zur Seite stattfindet.

Weitere Informationen zu Shellanwendungen finden Sie unter [xamarin. Forms Shell](~/xamarin-forms/app-fundamentals/shell/index.md).

## <a name="fontimage-markup-extension"></a>FontImage-Markuperweiterung

Die `FontImage` Markup Erweiterung ermöglicht es Ihnen, ein Schriftart Symbol in jeder Ansicht anzuzeigen, die ein `ImageSource`anzeigen kann. Sie bietet die gleiche Funktionalität wie die `FontImageSource` -Klasse, bietet aber eine präzisere Darstellung.

Die Markuperweiterung `FontImage` wird von der Klasse `FontImageExtension` unterstützt, in der die folgenden Eigenschaften definiert werden:

- `FontFamily`vom Typ `string`, die Schriftfamilie, zu der das Schriftart Symbol gehört.
- `Glyph`des Typs `string`, der Unicode-Zeichen Wert des Schriftart Symbols.
- `Color`der Typ [`Color`](xref:Xamarin.Forms.Color), die Farbe, die beim Anzeigen des Schriftart Symbols verwendet werden soll.
- `Size`vom Typ `double`, die Größe des gerenderten Schriftart Symbols in geräteunabhängigen Einheiten. Der Standardwert ist 30. Außerdem kann diese Eigenschaft auf eine benannte Schriftgröße festgelegt werden.

> [!NOTE]
> Im XAML-Parser kann die Klasse `FontImageExtension` zu `FontImage` abgekürzt werden.

Die `Glyph` -Eigenschaft ist die Content- `FontImageExtension`Eigenschaft von. Daher können Sie für XAML-Markup Ausdrücke, die mit geschweiften Klammern ausgedrückt werden `Glyph=` , den Teil des Ausdrucks entfernen, vorausgesetzt, dass es sich um das erste Argument handelt.

Die Seite " **fontimage Demo** " zeigt die Verwendung `FontImage` der Markup Erweiterung:

```xaml
<Image BackgroundColor="#D1D1D1"
       Source="{FontImage &#xf30c;, FontFamily={OnPlatform iOS=Ionicons, Android=ionicons.ttf#}, Size=44}" />
```

In diesem Beispiel wird die abgekürzte Version `FontImageExtension` des Klassen namens verwendet, um ein Xbox-Symbol aus der Schriftfamilie "ionicons" in [`Image`](xref:Xamarin.Forms.Image)einer anzuzeigen. Der Ausdruck verwendet auch die `OnPlatform` Markup Erweiterung, um unter `FontFamily` schiedliche Eigenschaftswerte unter IOS und Android anzugeben. Außerdem wird der `Glyph=` Teil des Ausdrucks gelöscht, und die festgelegten Markup Erweiterungs Eigenschaften werden durch Kommas getrennt. Beachten Sie, dass das Unicode-Zeichen für das `\uf30c`Symbol in XAML mit Escapezeichen versehen werden muss und `&#xf30c;`daher zu wird.

Dies ist das Programm, das ausgeführt wird:

[![Screenshot der fontimage-Markup Erweiterung](consuming-images/fontimagedemo.png "Fontimage-Demo")](consuming-images/fontimagedemo-large.png#lightbox "Fontimage-Demo")

Informationen zum Anzeigen von Schriftart Symbolen durch Angeben der Schriftart Symbol Daten in einem `FontImageSource` -Objekt finden Sie unter [Anzeigen von Schriftart Symbolen](~/xamarin-forms/user-interface/text/fonts.md#display-font-icons).

## <a name="onapptheme-markup-extension"></a>Onapptheme-Markup Erweiterung

Die `OnAppTheme` Markup Erweiterung ermöglicht es Ihnen, eine zu verwendende Ressource anzugeben, z. b. ein Bild oder eine Farbe, basierend auf dem aktuellen Systemdesign. Sie bietet die gleiche Funktionalität wie die `OnAppTheme<T>` -Klasse, bietet aber eine präzisere Darstellung.

> [!IMPORTANT]
> Die `OnAppTheme` Markup Erweiterung verfügt über die Mindestanforderungen an das Betriebssystem. Weitere Informationen finden Sie unter [reagieren auf Systemdesign Änderungen in xamarin. Forms-Anwendungen](~/xamarin-forms/user-interface/theming/system-theme-changes.md).

Die Markuperweiterung `OnAppTheme` wird von der Klasse `OnAppThemeExtension` unterstützt, in der die folgenden Eigenschaften definiert werden:

- `Default`vom Typ `object`, den Sie auf die Ressource festlegen, die standardmäßig verwendet werden soll.
- `Light`vom Typ `object`, den Sie auf die Ressource festlegen, die verwendet werden soll, wenn das Gerät das helle Design verwendet.
- `Dark`vom Typ `object`, den Sie auf die Ressource festlegen, die verwendet werden soll, wenn das Gerät das dunkle Design verwendet.
- `Value`vom Typ `object`, der die Ressource zurückgibt, die zurzeit von der Markup Erweiterung verwendet wird.
- `Converter`vom Typ `IValueConverter`, die auf eine `IValueConverter` Implementierung festgelegt werden kann.
- `ConverterParameter`vom Typ `object`, die auf einen Wert festgelegt werden kann, der an `IValueConverter` die-Implementierung übergeben werden soll.

> [!NOTE]
> Im XAML-Parser kann die Klasse `OnAppThemeExtension` zu `OnAppTheme` abgekürzt werden.

Die `Default` -Eigenschaft ist die Content- `OnAppThemeExtension`Eigenschaft von. Daher können Sie für XAML-Markup Ausdrücke, die mit geschweiften Klammern ausgedrückt werden `Default=` , den Teil des Ausdrucks entfernen, vorausgesetzt, dass es sich um das erste Argument handelt.

Die **onapptheme-Demoseite** zeigt die Verwendung der `OnAppTheme` Markup Erweiterung:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.OnAppThemeDemoPage"
             Title="OnAppTheme Demo">
    <ContentPage.Resources>

        <Style x:Key="labelStyle"
               TargetType="Label">
            <Setter Property="TextColor"
                    Value="{OnAppTheme Black, Light=Blue, Dark=Teal}" />
        </Style>

    </ContentPage.Resources>
    <StackLayout Margin="20">
        <Label Text="This text is green in light mode, and red in dark mode."
               TextColor="{OnAppTheme Light=Green, Dark=Red}" />
        <Label Text="This text is black by default, blue in light mode, and teal in dark mode."
               Style="{DynamicResource labelStyle}" />
    </StackLayout>
</ContentPage>
```

In diesem Beispiel wird die Textfarbe der ersten [`Label`](xref:Xamarin.Forms.Label) auf grün festgelegt, wenn das Gerät das helle Design verwendet, und wird auf Rot festgelegt, wenn das Gerät das dunkle Design verwendet. In der `Label` zweiten ist [`TextColor`](xref:Xamarin.Forms.Label.TextColor) die-Eigenschaft durch [`Style`](xref:Xamarin.Forms.Style)einen festgelegt. Dadurch `Style` wird die Textfarbe `Label` der standardmäßig auf schwarz festgelegt, auf blau, wenn das Gerät das helle Design verwendet, und auf den Wert, wenn das Gerät das dunkle Design verwendet.

> [!NOTE]
> Ein [`Style`](xref:Xamarin.Forms.Style) , das die `OnAppTheme` Markup Erweiterung verwendet, sollte auf ein Steuerelement mit `DynamicResource` der Markup Erweiterung angewendet werden, damit die Benutzeroberfläche der APP aktualisiert wird, wenn das Systemdesign geändert wird.

Dies ist das Programm, das ausgeführt wird:

![Onapptheme-Demo](consuming-images/onappthemedemo.png "Onapptheme-Demo")

## <a name="define-markup-extensions"></a>Definieren von Markup Erweiterungen

Wenn Sie eine XAML-Markup Erweiterung gefunden haben, die in xamarin. Forms nicht verfügbar ist, können Sie eine [eigene erstellen](creating.md).

## <a name="related-links"></a>Verwandte Links

- [Markup Erweiterungen (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)
- [XAML-Markup Erweiterungen-Kapitel aus dem xamarin. Forms-Buch](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [Ressourcen Wörterbücher](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Dynamische Stile](~/xamarin-forms/user-interface/styles/dynamic.md)
- [Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Xamarin.Forms-Shell](~/xamarin-forms/app-fundamentals/shell/index.md)
- [Reagieren auf Systemdesign Änderungen in xamarin. Forms-Anwendungen](~/xamarin-forms/user-interface/theming/system-theme-changes.md)
