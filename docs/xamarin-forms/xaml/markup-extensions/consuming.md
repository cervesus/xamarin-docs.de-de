---
Title: "Verwenden von XAML-Markup Erweiterungen" Beschreibung: "in diesem Artikel wird erläutert, wie XAML Xamarin.Forms -Markup Erweiterungen verwendet werden, um die Leistungsfähigkeit und Flexibilität von XAML zu verbessern, indem Element Attribute aus einer Vielzahl von Quellen festgelegt werden können."
ms. Prod: xamarin ms. assetid: CE686893-609C-4EC3-9225-6C68D2A9F79C ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 04/21/2020 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="consuming-xaml-markup-extensions"></a>Verwenden von XAML-Markuperweiterungen

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)

XAML-Markup Erweiterungen helfen, die Leistungsfähigkeit und Flexibilität von XAML zu verbessern, indem die Festlegung von Element Attributen aus einer Vielzahl von Quellen ermöglicht wird. Mehrere XAML-Markup Erweiterungen sind Teil der XAML 2009-Spezifikation. Diese werden in XAML-Dateien mit dem entsprechenden `x` Namespace Präfix angezeigt und werden häufig mit diesem Präfix bezeichnet. In diesem Artikel werden die folgenden Markup Erweiterungen erläutert:

- [`x:Static`](#xstatic-markup-extension)– verweisen auf statische Eigenschaften, Felder oder Enumerationsmember.
- [`x:Reference`](#xreference-markup-extension)– verweist auf benannte Elemente auf der Seite.
- [`x:Type`](#xtype-markup-extension)– Legen Sie ein Attribut auf ein- `System.Type` Objekt fest.
- [`x:Array`](#xarray-markup-extension)– Erstellen Sie ein Array von Objekten eines bestimmten Typs.
- [`x:Null`](#xnull-markup-extension)– Legen Sie ein Attribut auf einen `null` Wert fest.
- [`OnPlatform`](#onplatform-markup-extension)– passen Sie die Darstellung der Benutzeroberfläche plattformspezifisch an.
- [`OnIdiom`](#onidiom-markup-extension)– Anpassen der Darstellung der Benutzeroberfläche basierend auf dem Erscheinungsbild des Geräts, auf dem die Anwendung ausgeführt wird.
- [`DataTemplate`](#datatemplate-markup-extension)– Konvertiert einen-Typ in einen-Typ [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) .
- [`FontImage`](#fontimage-markup-extension)– zeigt ein Schriftart Symbol in einer beliebigen Ansicht an, in der ein angezeigt werden kann `ImageSource` .
- [`OnAppTheme`](#onapptheme-markup-extension)– verwendet eine Ressource basierend auf dem aktuellen Systemdesign.

Weitere XAML-Markup Erweiterungen wurden in der Vergangenheit von anderen XAML-Implementierungen unterstützt und werden auch von unterstützt Xamarin.Forms . Diese werden in anderen Artikeln ausführlicher beschrieben:

- `StaticResource`-Verweis Objekte aus einem Ressourcen Wörterbuch, wie im Artikel [**Ressourcen Wörterbücher**](~/xamarin-forms/xaml/resource-dictionaries.md)beschrieben.
- `DynamicResource`-reagieren Sie auf Änderungen in Objekten in einem Ressourcen Wörterbuch, wie im Artikel [**dynamische Stile**](~/xamarin-forms/user-interface/styles/dynamic.md)beschrieben.
- `Binding`-Erstellen Sie eine Verknüpfung zwischen Eigenschaften von zwei-Objekten, wie im Artikel [**Datenbindung**](~/xamarin-forms/app-fundamentals/data-binding/index.md)beschrieben.
- `TemplateBinding`: führt Daten Bindungen aus einer Steuerelement Vorlage aus, wie im Artikel [** Xamarin.Forms Steuerelement Vorlagen**](~/xamarin-forms/app-fundamentals/templates/control-template.md)erläutert.
- `RelativeSource`: legt die Bindungs Quelle relativ zur Position des Bindungs Ziels fest, wie im Artikel [relative Bindungen](~/xamarin-forms/app-fundamentals/data-binding/relative-bindings.md)erläutert.

Das [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) Layout verwendet die benutzerdefinierte Markup Erweiterung [`ConstraintExpression`](xref:Xamarin.Forms.ConstraintExpression) . Diese Markup Erweiterung wird im Artikel [**relativelayout**](~/xamarin-forms/user-interface/layouts/relative-layout.md)beschrieben.

## <a name="xstatic-markup-extension"></a>x:Static-Markuperweiterung

Die `x:Static` Markup Erweiterung wird von der- [`StaticExtension`](xref:Xamarin.Forms.Xaml.StaticExtension) Klasse unterstützt. Die-Klasse verfügt über eine einzelne Eigenschaft namens [`Member`](xref:Xamarin.Forms.Xaml.StaticExtension.Member) vom Typ, die `string` Sie auf den Namen einer öffentlichen Konstante, einer statischen Eigenschaft, eines statischen Felds oder eines Enumerationsmembers festlegen.

Eine gängige Methode zur Verwendung von besteht darin, `x:Static` zuerst eine Klasse mit einigen Konstanten oder statischen Variablen zu definieren, z. b. diese kleine `AppConstants` Klasse im [**MarkupExtensions**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions) -Programm:

```csharp
static class AppConstants
{
    public static double NormalFontSize = 18;
}
```

Die **x:static-Demoseite** zeigt verschiedene Möglichkeiten zur Verwendung der `x:Static` Markup Erweiterung. Der ausführlichste Ansatz instanziiert die `StaticExtension` -Klasse zwischen `Label.FontSize` Eigenschaften-Element-Tags:

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

Der XAML-Parser ermöglicht außerdem die Abkürzung der- `StaticExtension` Klasse `x:Static` :

```xaml
<Label Text="Label No. 2">
    <Label.FontSize>
        <x:Static Member="local:AppConstants.NormalFontSize" />
    </Label.FontSize>
</Label>
```

Dies kann noch weiter vereinfacht werden, aber die Änderung führt zu einer neuen Syntax: Sie besteht aus dem Platzieren der `StaticExtension` Klasse und der Member-Einstellung in geschweiften Klammern. Der resultierende Ausdruck wird direkt auf das- `FontSize` Attribut festgelegt:

```xaml
<Label Text="Label No. 3"
       FontSize="{x:StaticExtension Member=local:AppConstants.NormalFontSize}" />
```

Beachten Sie, dass es *keine* Anführungszeichen innerhalb der geschweiften Klammern gibt. Die- `Member` Eigenschaft von `StaticExtension` ist kein XML-Attribut mehr. Er ist stattdessen ein Teil des Ausdrucks für die Markup Erweiterung.

Ebenso wie Sie die abkürzen `x:StaticExtension` können `x:Static` , wenn Sie Sie als Objekt Element verwenden, können Sie Sie auch im Ausdruck in geschweiften Klammern abkürzen:

```xaml
<Label Text="Label No. 4"
       FontSize="{x:Static Member=local:AppConstants.NormalFontSize}" />
```

Die- `StaticExtension` Klasse verfügt über ein- `ContentProperty` Attribut, das auf die-Eigenschaft verweist `Member` , die diese Eigenschaft als Standard Inhalts Eigenschaft der Klasse kennzeichnet. Bei XAML-Markup Erweiterungen, die mit geschweiften Klammern ausgedrückt werden, können Sie den `Member=` Teil des Ausdrucks entfernen:

```xaml
<Label Text="Label No. 5"
       FontSize="{x:Static local:AppConstants.NormalFontSize}" />
```

Dies ist die häufigste Form der `x:Static` Markup Erweiterung.

Die **statische Demoseite** enthält zwei weitere Beispiele. Das Stammtag der XAML-Datei enthält eine XML-Namespace Deklaration für den .net- `System` Namespace:

```xaml
xmlns:sys="clr-namespace:System;assembly=netstandard"
```

Dadurch kann die `Label` Schriftgröße auf das statische Feld festgelegt werden `Math.PI` . Das führt zu einem eher kleinen Text, sodass die- `Scale` Eigenschaft auf festgelegt ist `Math.E` :

```xaml
<Label Text="&#x03C0; &#x00D7; E sized text"
       FontSize="{x:Static sys:Math.PI}"
       Scale="{x:Static sys:Math.E}"
       HorizontalOptions="Center" />
```

Im letzten Beispiel wird der- `Device.RuntimePlatform` Wert angezeigt. Die `Environment.NewLine` statische-Eigenschaft wird verwendet, um ein neue-Zeile-Zeichen zwischen den beiden- `Span` Objekten einzufügen:

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

## <a name="xreference-markup-extension"></a>x:Reference-Markuperweiterung

Die `x:Reference` Markup Erweiterung wird von der- [`ReferenceExtension`](xref:Xamarin.Forms.Xaml.ReferenceExtension) Klasse unterstützt. Die-Klasse verfügt über eine einzelne-Eigenschaft mit dem Namen [`Name`](xref:Xamarin.Forms.Xaml.ReferenceExtension.Name) vom Typ `string` , die Sie auf den Namen eines Elements auf der Seite festlegen, dem ein Name zugewiesen wurde `x:Name` . Diese `Name` Eigenschaft ist die Content-Eigenschaft von `ReferenceExtension` und `Name=` ist daher nicht erforderlich, wenn `x:Reference` in geschweiften Klammern angezeigt wird.

Die `x:Reference` Markup Erweiterung wird ausschließlich mit Daten Bindungen verwendet, die im Artikel [**Datenbindung**](~/xamarin-forms/app-fundamentals/data-binding/index.md)ausführlicher beschrieben werden.

Die **Demo Seite "x:Reference** " zeigt zwei Verwendungen von `x:Reference` mit Daten Bindungen, der erste, in dem er verwendet wird, um die `Source` -Eigenschaft des `Binding` -Objekts festzulegen, und der zweite, in dem er verwendet wird, um die- `BindingContext` Eigenschaft für zwei Daten Bindungen festzulegen:

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

Beide `x:Reference` Ausdrücke verwenden die abgekürzte Version des `ReferenceExtension` Klassen namens und entfernen den `Name=` Teil des Ausdrucks. Im ersten Beispiel `x:Reference` ist die Markup Erweiterung in der `Binding` Markup Erweiterung eingebettet. Beachten Sie, dass die `Source` `StringFormat` Einstellungen und durch Kommas getrennt sind. Dies ist das Programm, das ausgeführt wird:

[![x:verweisdemo](consuming-images/referencedemo-small.png "x:verweisdemo")](consuming-images/referencedemo-large.png#lightbox "x:verweisdemo")

## <a name="xtype-markup-extension"></a>x:Type-Markuperweiterung

Die `x:Type` Markup Erweiterung ist die XAML-Entsprechung des c#- [`typeof`](/dotnet/csharp/language-reference/keywords/typeof/) Schlüssel Worts. Sie wird von der- [`TypeExtension`](xref:Xamarin.Forms.Xaml.TypeExtension) Klasse unterstützt, die eine Eigenschaft [`TypeName`](xref:Xamarin.Forms.Xaml.TypeExtension.TypeName) mit dem Namen des Typs definiert, `string` die auf einen Klassen-oder Struktur Namen festgelegt ist. Die `x:Type` Markup Erweiterung gibt das- [`System.Type`](xref:System.Type) Objekt dieser Klasse oder Struktur zurück. `TypeName`ist die Content-Eigenschaft von `TypeExtension` `TypeName=` . Dies ist daher nicht erforderlich, wenn `x:Type` mit geschweiften Klammern angezeigt wird.

In Xamarin.Forms gibt es mehrere Eigenschaften, die über Argumente des Typs verfügen `Type` . Beispiele hierfür sind die [`TargetType`](xref:Xamarin.Forms.Style.TargetType) -Eigenschaft von `Style` und das [x:TypeArguments](~/xamarin-forms/xaml/passing-arguments.md#specifying-a-generic-type-argument) -Attribut, mit dem Argumente in generischen Klassen angegeben werden. Der XAML-Parser führt den `typeof` Vorgang jedoch automatisch aus, und die `x:Type` Markup Erweiterung wird in diesen Fällen nicht verwendet.

Eine Stelle, an der `x:Type` *is* erforderlich ist, ist die `x:Array` Markup Erweiterung, die im [nächsten Abschnitt](#xarray-markup-extension)beschrieben wird.

Die `x:Type` Markup Erweiterung ist auch nützlich, wenn ein Menü erstellt wird, in dem jedes Menü Element einem Objekt eines bestimmten Typs entspricht. Sie können `Type` jedem Menü Element ein-Objekt zuordnen und dann das-Objekt instanziieren, wenn das Menü Element ausgewählt wird.

Auf diese Weise funktioniert das Navigationsmenü in `MainPage` im **Markup Extensions** -Programm. Die Datei " **MainPage. XAML** " enthält einen `TableView` , der jeweils `TextCell` einer bestimmten Seite im Programm entspricht:

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

Jede `CommandParameter` Eigenschaft wird auf eine `x:Type` Markup Erweiterung festgelegt, die auf eine der anderen Seiten verweist. Die- `Command` Eigenschaft ist an eine Eigenschaft mit dem Namen gebunden `NavigateCommand` . Diese Eigenschaft wird in der `MainPage` Code Behind-Datei definiert:

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

Bei der- `NavigateCommand` Eigenschaft handelt es sich um ein- `Command` Objekt, das einen Execute-Befehl mit einem Argument vom Typ `Type` &mdash; den Wert von implementiert `CommandParameter` . Die-Methode verwendet `Activator.CreateInstance` , um die Seite zu instanziieren und dann zu dieser zu navigieren. Der Konstruktor wird beendet, indem der `BindingContext` der Seite auf sich selbst festgelegt wird, sodass das `Binding` on `Command` funktioniert. Weitere Informationen zu dieser Art von Code finden Sie im Artikel [**Datenbindung**](~/xamarin-forms/app-fundamentals/data-binding/index.md) und insbesondere im Artikel zur [**Befehls**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md) Zeile.

Die **x:Type-Demoseite** verwendet eine ähnliche Technik, um Elemente zu instanziieren Xamarin.Forms und Sie einem hinzuzufügen `StackLayout` . Die XAML-Datei besteht anfänglich aus drei `Button` Elementen `Command` , deren Eigenschaften auf festgelegt sind, `Binding` und den `CommandParameter` Eigenschaften, die auf drei Ansichten festgelegt sind Xamarin.Forms :

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

Mit der Code-Behind-Datei wird die-Eigenschaft definiert und initialisiert `CreateCommand` :

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

Die Methode, die ausgeführt wird, wenn ein `Button` gedrückt wird, erstellt eine neue Instanz des Arguments, legt die zugehörige `VerticalOptions` -Eigenschaft fest und fügt Sie hinzu `StackLayout` . Die drei `Button` Elemente geben dann die Seite mit dynamisch erstellten Ansichten frei:

[![x:TypDEMO](consuming-images/typedemo-small.png "x:TypDEMO")](consuming-images/typedemo-large.png#lightbox "x:TypDEMO")

## <a name="xarray-markup-extension"></a>x:Array-Markuperweiterung

Die `x:Array` Markup Erweiterung ermöglicht es Ihnen, ein Array im Markup zu definieren. Sie wird von der- [`ArrayExtension`](xref:Xamarin.Forms.Xaml.ArrayExtension) Klasse unterstützt, die zwei Eigenschaften definiert:

- `Type`vom Typ `Type` , der den Typ der Elemente im Array angibt.
- `Items`vom Typ `IList` , bei dem es sich um eine Auflistung der Elemente selbst handelt. Dabei handelt es sich um die Content-Eigenschaft von `ArrayExtension` .

Die `x:Array` Markup Erweiterung selbst wird nie in geschweiften Klammern angezeigt. Stattdessen wird `x:Array` die Liste der Elemente von Start-und Endtags getrennt. Legen Sie die- `Type` Eigenschaft auf eine `x:Type` Markup Erweiterung fest.

Die **x:Array-Demo** Seite zeigt, wie Sie mit `x:Array` Elemente zu einem hinzufügen, `ListView` indem Sie die- `ItemsSource` Eigenschaft auf ein Array festlegen:

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

Sie können auch verwenden, `StaticResource` um eine Farbe aus einem Ressourcen Wörterbuch abzurufen:

```xaml
<StaticResource Key="myColor" />
```

Am Ende dieses Artikels wird eine benutzerdefinierte XAML-Markup Erweiterung angezeigt, die auch einen neuen Farbwert erstellt:

```xaml
<local:HslColor H="0.5" S="1.0" L="0.5" />
```

Wenn Sie Arrays allgemeiner Typen wie Zeichen folgen oder Zahlen definieren, verwenden Sie die im Artikel [**übergeben von Konstruktorargumenten**](~/xamarin-forms/xaml/passing-arguments.md#passing-constructor-arguments) aufgeführten Tags, um die Werte zu begrenzen.

## <a name="xnull-markup-extension"></a>x:Null-Markuperweiterung

Die `x:Null` Markup Erweiterung wird von der- [`NullExtension`](xref:Xamarin.Forms.Xaml.NullExtension) Klasse unterstützt. Sie verfügt über keine Eigenschaften und ist einfach die XAML-Entsprechung des c#- [`null`](/dotnet/csharp/language-reference/keywords/null/) Schlüssel Worts.

Die `x:Null` Markup Erweiterung ist nur selten erforderlich und wird selten verwendet. Wenn Sie jedoch einen Bedarf dafür feststellen, sind Sie froh, dass Sie vorhanden ist.

Die **Demo Seite x:NULL** veranschaulicht ein Szenario, in dem Sie `x:Null` möglicherweise bequem ist. Angenommen, Sie definieren eine implizite `Style` für `Label` , die einen enthält `Setter` , der die- `FontFamily` Eigenschaft auf einen Platt Form abhängigen Familiennamen festlegt:

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

Dann stellen Sie fest, dass Sie für eines der- `Label` Elemente alle Eigenschafts Einstellungen im impliziten `Style` , mit Ausnahme von `FontFamily` , benötigen, was der Standardwert sein soll. Sie können `Style` für diesen Zweck einen anderen definieren, aber ein einfacherer Ansatz besteht darin, einfach die- `FontFamily` Eigenschaft des jeweiligen auf festzulegen `Label` `x:Null` , wie in der Mitte veranschaulicht `Label` .

Dies ist das Programm, das ausgeführt wird:

[![x:NULL-Demo](consuming-images/nulldemo-small.png "x:NULL-Demo")](consuming-images/nulldemo-large.png#lightbox "x:NULL-Demo")

Beachten Sie, dass vier der- `Label` Elemente über eine "Serif"-Schriftart verfügen, das Center jedoch `Label` die standardmäßige Sans-Serif-Schriftart hat.

## <a name="onplatform-markup-extension"></a>Onplatform-Markup Erweiterung

Die Markuperweiterung `OnPlatform` ermöglicht Ihnen das plattformspezifische Anpassen der Benutzeroberfläche. Sie bietet die gleiche Funktionalität wie die [`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) -Klasse und die- [`On`](xref:Xamarin.Forms.On) Klasse, jedoch mit einer präziseren Darstellung.

Die `OnPlatform` Markup Erweiterung wird von der- [`OnPlatformExtension`](xref:Xamarin.Forms.Xaml.OnPlatformExtension) Klasse unterstützt, die die folgenden Eigenschaften definiert:

- `Default`vom Typ `object` , die auf einen Standardwert festgelegt wird, der auf die Eigenschaften angewendet werden soll, die Plattformen darstellen.
- `Android`vom Typ `object` , den Sie auf einen Wert festlegen, der auf Android angewendet werden soll.
- `GTK`vom Typ `object` , den Sie auf einen Wert festlegen, der auf GTK-Plattformen angewendet werden soll.
- `iOS`vom Typ `object` , den Sie auf einen Wert festlegen, der auf IOS angewendet werden soll.
- `macOS`vom Typ `object` , den Sie auf einen Wert festlegen, der auf macOS angewendet werden soll.
- `Tizen`vom Typ `object` , den Sie auf einen Wert festlegen, der auf die tizen-Plattform angewendet werden soll.
- `UWP`vom Typ `object` , der auf einen Wert festgelegt wird, der auf die universelle Windows-Plattform angewendet werden soll.
- `WPF`vom Typ `object` , der auf einen Wert festgelegt wird, der auf die Windows Presentation Foundation Plattform angewendet werden soll.
- `Converter`vom Typ `IValueConverter` , die auf eine Implementierung festgelegt werden kann `IValueConverter` .
- `ConverterParameter`vom Typ `object` , die auf einen Wert festgelegt werden kann, der an die-Implementierung übergeben werden soll `IValueConverter` .

> [!NOTE]
> Der XAML-Parser ermöglicht [`OnPlatformExtension`](xref:Xamarin.Forms.Xaml.OnPlatformExtension) , dass die-Klasse als abgekürzt wird `OnPlatform` .

Die- `Default` Eigenschaft ist die Content-Eigenschaft von `OnPlatformExtension` . Daher können Sie für XAML-Markup Ausdrücke, die mit geschweiften Klammern ausgedrückt werden, den `Default=` Teil des Ausdrucks entfernen, vorausgesetzt, dass es sich um das erste Argument handelt. Wenn die- `Default` Eigenschaft nicht festgelegt ist, wird standardmäßig der- [`BindableProperty.DefaultValue`](xref:Xamarin.Forms.BindableProperty.DefaultValue) Eigenschafts Wert verwendet, vorausgesetzt, dass die Markup Erweiterung auf abzielt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) .

> [!IMPORTANT]
> Der XAML-Parser erwartet, dass die Werte des richtigen Typs für Eigenschaften bereitgestellt werden, die die `OnPlatform` Markup Erweiterung nutzen. Wenn eine Typkonvertierung erforderlich ist, `OnPlatform` versucht die Markup Erweiterung, Sie mit den von bereitgestellten Standard Konvertern auszuführen Xamarin.Forms . Es gibt jedoch einige Typkonvertierungen, die von den Standard Konvertern nicht ausgeführt werden können. in diesen Fällen sollte die- `Converter` Eigenschaft auf eine-Implementierung festgelegt werden `IValueConverter` .

Die **onplatform-Demo** Seite zeigt, wie die `OnPlatform` Markup Erweiterung verwendet wird:

```xaml
<BoxView Color="{OnPlatform Yellow, iOS=Red, Android=Green, UWP=Blue}"
         WidthRequest="{OnPlatform 250, iOS=200, Android=300, UWP=400}"  
         HeightRequest="{OnPlatform 250, iOS=200, Android=300, UWP=400}"
         HorizontalOptions="Center" />
```

In diesem Beispiel verwenden alle drei `OnPlatform` Ausdrücke die abgekürzte Version des `OnPlatformExtension` Klassen namens. Die drei `OnPlatform` Markup Erweiterungen legen die [`Color`](xref:Xamarin.Forms.BoxView.Color) [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) Eigenschaften, und [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) von [`BoxView`](xref:Xamarin.Forms.BoxView) auf unterschiedliche Werte unter IOS, Android und UWP fest. Die Markup Erweiterungen stellen außerdem Standardwerte für diese Eigenschaften auf den Plattformen bereit, die nicht angegeben sind, während der `Default=` Teil des Ausdrucks eliminiert wird. Beachten Sie, dass die festgelegten Markup Erweiterungs Eigenschaften durch Kommas voneinander getrennt sind.

Dies ist das Programm, das ausgeführt wird:

[![Onplatform-Demo](consuming-images/onplatformdemo-small.png "Onplatform-Demo")](consuming-images/onplatformdemo-large.png#lightbox "Onplatform-Demo")

## <a name="onidiom-markup-extension"></a>Onidiom-Markup Erweiterung

Die `OnIdiom` Markup Erweiterung ermöglicht es Ihnen, die Darstellung der Benutzeroberfläche basierend auf dem Erscheinungsbild des Geräts anzupassen, auf dem die Anwendung ausgeführt wird. Sie wird von der- [`OnIdiomExtension`](xref:Xamarin.Forms.Xaml.OnIdiomExtension) Klasse unterstützt, die die folgenden Eigenschaften definiert:

- `Default`vom Typ `object` : Sie legen auf einen Standardwert fest, der auf die Eigenschaften angewendet werden soll, die die Geräte Idiome darstellen.
- `Phone`vom Typ `object` , den Sie auf einen auf Smartphones anzuwendenden Wert festlegen.
- `Tablet`vom Typ `object` , den Sie auf einen auf Tablets anzuwendenden Wert festlegen.
- `Desktop`vom Typ `object` , den Sie auf einen auf Desktop Plattformen anzuwendenden Wert festlegen.
- `TV`vom Typ `object` , den Sie auf einen Wert festlegen, der auf TV-Plattformen angewendet werden soll.
- `Watch`vom Typ `object` , den Sie auf einen Wert festlegen, der auf Überwachungs Plattformen angewendet werden soll.
- `Converter`vom Typ `IValueConverter` , die auf eine Implementierung festgelegt werden kann `IValueConverter` .
- `ConverterParameter`vom Typ `object` , die auf einen Wert festgelegt werden kann, der an die-Implementierung übergeben werden soll `IValueConverter` .

> [!NOTE]
> Der XAML-Parser ermöglicht [`OnIdiomExtension`](xref:Xamarin.Forms.Xaml.OnIdiomExtension) , dass die-Klasse als abgekürzt wird `OnIdiom` .

Die- `Default` Eigenschaft ist die Content-Eigenschaft von `OnIdiomExtension` . Daher können Sie für XAML-Markup Ausdrücke, die mit geschweiften Klammern ausgedrückt werden, den `Default=` Teil des Ausdrucks entfernen, vorausgesetzt, dass es sich um das erste Argument handelt.

> [!IMPORTANT]
> Der XAML-Parser erwartet, dass die Werte des richtigen Typs für Eigenschaften bereitgestellt werden, die die `OnIdiom` Markup Erweiterung nutzen. Wenn eine Typkonvertierung erforderlich ist, `OnIdiom` versucht die Markup Erweiterung, Sie mit den von bereitgestellten Standard Konvertern auszuführen Xamarin.Forms . Es gibt jedoch einige Typkonvertierungen, die von den Standard Konvertern nicht ausgeführt werden können. in diesen Fällen sollte die- `Converter` Eigenschaft auf eine-Implementierung festgelegt werden `IValueConverter` .

Die Seite " **onidiom Demo** " zeigt die Verwendung der `OnIdiom` Markup Erweiterung:

```xaml
<BoxView Color="{OnIdiom Yellow, Phone=Red, Tablet=Green, Desktop=Blue}"
         WidthRequest="{OnIdiom 100, Phone=200, Tablet=300, Desktop=400}"
         HeightRequest="{OnIdiom 100, Phone=200, Tablet=300, Desktop=400}"
         HorizontalOptions="Center" />
```

In diesem Beispiel verwenden alle drei `OnIdiom` Ausdrücke die abgekürzte Version des `OnIdiomExtension` Klassen namens. Die drei `OnIdiom` Markup Erweiterungen legen die-Eigenschaft, die [`Color`](xref:Xamarin.Forms.BoxView.Color) [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) -Eigenschaft und [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) die-Eigenschaft des [`BoxView`](xref:Xamarin.Forms.BoxView) auf die unterschiedlichen Werte auf der Telefon-, Tablet-und Desktop Idioms fest. Die Markup Erweiterungen stellen außerdem Standardwerte für diese Eigenschaften für die Idiome bereit, die nicht angegeben sind, während der `Default=` Teil des Ausdrucks eliminiert wird. Beachten Sie, dass die festgelegten Markup Erweiterungs Eigenschaften durch Kommas voneinander getrennt sind.

Dies ist das Programm, das ausgeführt wird:

[![Onidiom-Demo](consuming-images/onidiomdemo-small.png "Onidiom-Demo")](consuming-images/onidiomdemo-large.png#lightbox "Onidiom-Demo")

## <a name="datatemplate-markup-extension"></a>DataTemplate-Markup Erweiterung

Die `DataTemplate` Markup Erweiterung ermöglicht es Ihnen, einen Typ in einen zu konvertieren [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) . Sie wird von der-Klasse unterstützt, `DataTemplateExtension` die eine `TypeName` Eigenschaft des Typs definiert, die `string` auf den Namen des Typs festgelegt ist, der in eine konvertiert werden soll `DataTemplate` . Die- `TypeName` Eigenschaft ist die Content-Eigenschaft von `DataTemplateExtension` . Daher können Sie bei XAML-Markupausdrücken (die mit geschweiften Klammern ausgedrückt werden) den `TypeName=`-Teil des Ausdrucks entfernen.

> [!NOTE]
> Im XAML-Parser kann die Klasse `DataTemplateExtension` zu `DataTemplate` abgekürzt werden.

Eine typische Verwendung dieser Markup Erweiterung ist in einer Shellanwendung, wie im folgenden Beispiel gezeigt:

```xaml
<ShellContent Title="Monkeys"
              Icon="monkey.png"
              ContentTemplate="{DataTemplate views:MonkeysPage}" />
```

In diesem Beispiel `MonkeysPage` wird von einem- [`ContentPage`](xref:Xamarin.Forms.ContentPage) Objekt in ein- [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) Objekt konvertiert, das als Wert der-Eigenschaft festgelegt wird `ShellContent.ContentTemplate` . Dadurch wird sichergestellt, dass `MonkeysPage` nur bei einem Anwendungsstart erstellt wird, wenn die Navigation zur Seite stattfindet.

Weitere Informationen zu Shellanwendungen finden Sie unter [ Xamarin.Forms Shell](~/xamarin-forms/app-fundamentals/shell/index.md).

## <a name="fontimage-markup-extension"></a>FontImage-Markuperweiterung

Die `FontImage` Markup Erweiterung ermöglicht es Ihnen, ein Schriftart Symbol in jeder Ansicht anzuzeigen, die ein anzeigen kann `ImageSource` . Sie bietet die gleiche Funktionalität wie die- `FontImageSource` Klasse, bietet aber eine präzisere Darstellung.

Die Markuperweiterung `FontImage` wird von der Klasse `FontImageExtension` unterstützt, in der die folgenden Eigenschaften definiert werden:

- `FontFamily`vom Typ `string` , die Schriftfamilie, zu der das Schriftart Symbol gehört.
- `Glyph`des Typs `string` , der Unicode-Zeichen Wert des Schriftart Symbols.
- `Color`der Typ [`Color`](xref:Xamarin.Forms.Color) , die Farbe, die beim Anzeigen des Schriftart Symbols verwendet werden soll.
- `Size`vom Typ `double` , die Größe des gerenderten Schriftart Symbols in geräteunabhängigen Einheiten. Der Standardwert ist 30. Außerdem kann diese Eigenschaft auf eine benannte Schriftgröße festgelegt werden.

> [!NOTE]
> Im XAML-Parser kann die Klasse `FontImageExtension` zu `FontImage` abgekürzt werden.

Die- `Glyph` Eigenschaft ist die Content-Eigenschaft von `FontImageExtension` . Daher können Sie für XAML-Markup Ausdrücke, die mit geschweiften Klammern ausgedrückt werden, den `Glyph=` Teil des Ausdrucks entfernen, vorausgesetzt, dass es sich um das erste Argument handelt.

Die Seite " **fontimage Demo** " zeigt die Verwendung der `FontImage` Markup Erweiterung:

```xaml
<Image BackgroundColor="#D1D1D1"
       Source="{FontImage &#xf30c;, FontFamily={OnPlatform iOS=Ionicons, Android=ionicons.ttf#}, Size=44}" />
```

In diesem Beispiel wird die abgekürzte Version des `FontImageExtension` Klassen namens verwendet, um ein Xbox-Symbol aus der Schriftfamilie "ionicons" in einer anzuzeigen [`Image`](xref:Xamarin.Forms.Image) . Der Ausdruck verwendet auch die `OnPlatform` Markup Erweiterung, um unterschiedliche `FontFamily` Eigenschaftswerte unter IOS und Android anzugeben. Außerdem `Glyph=` wird der Teil des Ausdrucks gelöscht, und die festgelegten Markup Erweiterungs Eigenschaften werden durch Kommas getrennt. Beachten Sie, dass das Unicode-Zeichen für das Symbol in XAML mit Escapezeichen versehen `\uf30c` werden muss und daher zu wird `&#xf30c;` .

Dies ist das Programm, das ausgeführt wird:

[![Screenshot der fontimage-Markup Erweiterung](consuming-images/fontimagedemo.png "Fontimage-Demo")](consuming-images/fontimagedemo-large.png#lightbox "Fontimage-Demo")

Informationen zum Anzeigen von Schriftart Symbolen durch Angeben der Schriftart Symbol Daten in einem- `FontImageSource` Objekt finden Sie unter [Anzeigen von Schriftart Symbolen](~/xamarin-forms/user-interface/text/fonts.md#display-font-icons).

## <a name="onapptheme-markup-extension"></a>OnAppTheme-Markuperweiterung

Die `OnAppTheme` Markup Erweiterung ermöglicht es Ihnen, eine zu verwendende Ressource anzugeben, z. b. ein Bild oder eine Farbe, basierend auf dem aktuellen Systemdesign. Sie bietet die gleiche Funktionalität wie die- `OnAppTheme<T>` Klasse, bietet aber eine präzisere Darstellung.

> [!IMPORTANT]
> Die `OnAppTheme` Markup Erweiterung verfügt über die Mindestanforderungen an das Betriebssystem. Weitere Informationen finden Sie unter [reagieren auf Änderungen des Systemdesigns in Xamarin.Forms Anwendungen](~/xamarin-forms/user-interface/theming/system-theme-changes.md).

Die Markuperweiterung `OnAppTheme` wird von der Klasse `OnAppThemeExtension` unterstützt, in der die folgenden Eigenschaften definiert werden:

- `Default`vom Typ `object` , den Sie auf die Ressource festlegen, die standardmäßig verwendet werden soll.
- `Light`vom Typ `object` , den Sie auf die Ressource festlegen, die verwendet werden soll, wenn das Gerät das helle Design verwendet.
- `Dark`vom Typ `object` , den Sie auf die Ressource festlegen, die verwendet werden soll, wenn das Gerät das dunkle Design verwendet.
- `Value`vom Typ `object` , der die Ressource zurückgibt, die zurzeit von der Markup Erweiterung verwendet wird.
- `Converter`vom Typ `IValueConverter` , die auf eine Implementierung festgelegt werden kann `IValueConverter` .
- `ConverterParameter`vom Typ `object` , die auf einen Wert festgelegt werden kann, der an die-Implementierung übergeben werden soll `IValueConverter` .

> [!NOTE]
> Im XAML-Parser kann die Klasse `OnAppThemeExtension` zu `OnAppTheme` abgekürzt werden.

Die- `Default` Eigenschaft ist die Content-Eigenschaft von `OnAppThemeExtension` . Daher können Sie für XAML-Markup Ausdrücke, die mit geschweiften Klammern ausgedrückt werden, den `Default=` Teil des Ausdrucks entfernen, vorausgesetzt, dass es sich um das erste Argument handelt.

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

In diesem Beispiel wird die Textfarbe der ersten [`Label`](xref:Xamarin.Forms.Label) auf grün festgelegt, wenn das Gerät das helle Design verwendet, und wird auf Rot festgelegt, wenn das Gerät das dunkle Design verwendet. In der zweiten ist die- `Label` [`TextColor`](xref:Xamarin.Forms.Label.TextColor) Eigenschaft durch einen festgelegt [`Style`](xref:Xamarin.Forms.Style) . Dadurch wird `Style` die Textfarbe der `Label` standardmäßig auf schwarz festgelegt, auf blau, wenn das Gerät das helle Design verwendet, und auf den Wert, wenn das Gerät das dunkle Design verwendet.

> [!NOTE]
> Ein [`Style`](xref:Xamarin.Forms.Style) , das die `OnAppTheme` Markup Erweiterung verwendet, sollte auf ein Steuerelement mit der `DynamicResource` Markup Erweiterung angewendet werden, damit die Benutzeroberfläche der APP aktualisiert wird, wenn das Systemdesign geändert wird.

Dies ist das Programm, das ausgeführt wird:

![Onapptheme-Demo](consuming-images/onappthemedemo.png "Onapptheme-Demo")

## <a name="define-markup-extensions"></a>Definieren von Markup Erweiterungen

Wenn Sie eine XAML-Markup Erweiterung gefunden haben, die in nicht verfügbar ist Xamarin.Forms , können Sie eine [eigene erstellen](creating.md).

## <a name="related-links"></a>Verwandte Links

- [Markup Erweiterungen (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)
- [Kapitel zu XAML-Markup Erweiterungen aus Xamarin.Forms Buch](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [Ressourcen Wörterbücher](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Dynamische Stile](~/xamarin-forms/user-interface/styles/dynamic.md)
- [Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Xamarin.Forms-Shell](~/xamarin-forms/app-fundamentals/shell/index.md)
- [Reagieren auf Änderungen des Systemdesigns in Xamarin.Forms Anwendungen](~/xamarin-forms/user-interface/theming/system-theme-changes.md)
