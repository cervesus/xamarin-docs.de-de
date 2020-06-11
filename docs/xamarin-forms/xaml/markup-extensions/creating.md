---
Title: "Erstellen von XAML-Markup Erweiterungen" Beschreibung: "in diesem Artikel wird erläutert, wie Sie eigene benutzerdefinierte Xamarin.Forms XAML-Markup Erweiterungen definieren. Eine XAML-Markup Erweiterung ist eine Klasse, die die Schnittstelle "imarkupextension" oder "imarkupextension <T> " implementiert.
ms. Prod: xamarin ms. assetid: 797c1ef9-1c8e-4208-8610-9b79ccar17d46 ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 01/05/2018 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="creating-xaml-markup-extensions"></a>Erstellen von XAML-Markuperweiterungen

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)

Auf der programmatischen Ebene ist eine XAML-Markup Erweiterung eine Klasse, die die- [`IMarkupExtension`](xref:Xamarin.Forms.Xaml.IMarkupExtension) oder- [`IMarkupExtension<T>`](xref:Xamarin.Forms.Xaml.IMarkupExtension`1) Schnittstelle implementiert. Sie können den Quellcode der Standard Markup Erweiterungen, die unten im [Verzeichnis **MarkupExtensions** ](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Xaml/MarkupExtensions) des GitHub-Repository beschrieben werden, untersuchen Xamarin.Forms .

Es ist auch möglich, eigene benutzerdefinierte XAML-Markup Erweiterungen zu definieren, indem Sie von `IMarkupExtension` oder ableiten `IMarkupExtension<T>` . Verwenden Sie das generische Formular, wenn die Markup Erweiterung einen Wert eines bestimmten Typs erhält. Dies ist bei einigen der Xamarin.Forms Markup Erweiterungen der Fall:

- `TypeExtension` wird von `IMarkupExtension<Type>` abgeleitet
- `ArrayExtension` wird von `IMarkupExtension<Array>` abgeleitet
- `DynamicResourceExtension` wird von `IMarkupExtension<DynamicResource>` abgeleitet
- `BindingExtension` wird von `IMarkupExtension<BindingBase>` abgeleitet
- `ConstraintExpression` wird von `IMarkupExtension<Constraint>` abgeleitet

Die beiden `IMarkupExtension` Schnittstellen definieren jeweils nur eine Methode mit dem Namen `ProvideValue` :

```csharp
public interface IMarkupExtension
{
    object ProvideValue(IServiceProvider serviceProvider);
}

public interface IMarkupExtension<out T> : IMarkupExtension
{
    new T ProvideValue(IServiceProvider serviceProvider);
}
```

Da `IMarkupExtension<T>` von abgeleitet `IMarkupExtension` ist und das- `new` Schlüsselwort enthält `ProvideValue` , enthält es beide `ProvideValue` Methoden.

Häufig definieren XAML-Markup Erweiterungen Eigenschaften, die zum Rückgabewert beitragen. (Die offensichtliche Ausnahme ist `NullExtension` , in der `ProvideValue` einfach zurückgibt `null` .) Die- `ProvideValue` Methode verfügt über ein einzelnes Argument vom Typ `IServiceProvider` , das weiter unten in diesem Artikel erläutert wird.

## <a name="a-markup-extension-for-specifying-color"></a>Eine Markup Erweiterung zum Angeben von Farbe

Die folgende XAML-Markup Erweiterung ermöglicht es Ihnen, `Color` mit Hue-, Sättigungs-und Helligkeits Komponenten einen Wert zu erstellen. Es definiert vier Eigenschaften für die vier Komponenten der Farbe, einschließlich einer Alpha Komponente, die mit 1 initialisiert wird. Die-Klasse wird von abgeleitet `IMarkupExtension<Color>` , um einen `Color` Rückgabewert anzugeben:

```csharp
public class HslColorExtension : IMarkupExtension<Color>
{
    public double H { set; get; }

    public double S { set; get; }

    public double L { set; get; }

    public double A { set; get; } = 1.0;

    public Color ProvideValue(IServiceProvider serviceProvider)
    {
        return Color.FromHsla(H, S, L, A);
    }

    object IMarkupExtension.ProvideValue(IServiceProvider serviceProvider)
    {
        return (this as IMarkupExtension<Color>).ProvideValue(serviceProvider);
    }
}
```

Da `IMarkupExtension<T>` von abgeleitet `IMarkupExtension` ist, muss die-Klasse zwei `ProvideValue` Methoden enthalten, eine, die zurückgibt, `Color` und eine andere, die zurückgibt `object` , aber die zweite Methode kann einfach die erste Methode aufruft.

Auf der Seite mit der **HSL-Farb Demo** werden verschiedene Möglichkeiten angezeigt, die `HslColorExtension` in einer XAML-Datei angezeigt werden können, um die Farbe für einen festzulegen `BoxView` :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:MarkupExtensions"
             x:Class="MarkupExtensions.HslColorDemoPage"
             Title="HSL Color Demo">

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="BoxView">
                <Setter Property="WidthRequest" Value="80" />
                <Setter Property="HeightRequest" Value="80" />
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout>
        <BoxView>
            <BoxView.Color>
                <local:HslColorExtension H="0" S="1" L="0.5" A="1" />
            </BoxView.Color>
        </BoxView>

        <BoxView>
            <BoxView.Color>
                <local:HslColor H="0.33" S="1" L="0.5" />
            </BoxView.Color>
        </BoxView>

        <BoxView Color="{local:HslColorExtension H=0.67, S=1, L=0.5}" />

        <BoxView Color="{local:HslColor H=0, S=0, L=0.5}" />

        <BoxView Color="{local:HslColor A=0.5}" />
    </StackLayout>
</ContentPage>
```

Beachten Sie, dass bei `HslColorExtension` einem XML-Tag die vier Eigenschaften als Attribute festgelegt werden. Wenn Sie jedoch zwischen geschweiften Klammern angezeigt werden, werden die vier Eigenschaften durch Kommas ohne Anführungszeichen voneinander getrennt. Die Standardwerte für `H` , `S` , und `L` sind 0, und der Standardwert von `A` ist 1. Daher können diese Eigenschaften ausgelassen werden, wenn Sie auf die Standardwerte festgelegt werden sollen. Im letzten Beispiel wird ein Beispiel gezeigt, bei dem die Helligkeit 0 ist, was normalerweise schwarz ergibt, aber der Alphakanal 0,5 ist, d. h., er ist halbtransparent und wird grau mit dem weißen Hintergrund der Seite angezeigt:

[![Demo zu HSL-Farben](creating-images/hslcolordemo-small.png "Demo zu HSL-Farben")](creating-images/hslcolordemo-large.png#lightbox "Demo zu HSL-Farben")

## <a name="a-markup-extension-for-accessing-bitmaps"></a>Eine Markup Erweiterung für den Zugriff auf Bitmaps

Das-Argument für `ProvideValue` ist ein Objekt, das die- [`IServiceProvider`](xref:System.IServiceProvider) Schnittstelle implementiert, die im .net- `System` Namespace definiert ist. Diese Schnittstelle verfügt über einen Member, eine Methode `GetService` mit dem Namen mit einem- `Type` Argument.

Die `ImageResourceExtension` unten gezeigte-Klasse zeigt eine mögliche Verwendung von `IServiceProvider` und `GetService` zum Abrufen eines `IXmlLineInfoProvider` Objekts, das Zeilen-und Zeichen Informationen bereitstellen kann, die angeben, wo ein bestimmter Fehler erkannt wurde. In diesem Fall wird eine Ausnahme ausgelöst, wenn die- `Source` Eigenschaft nicht festgelegt wurde:

```csharp
[ContentProperty("Source")]
class ImageResourceExtension : IMarkupExtension<ImageSource>
{
    public string Source { set; get; }

    public ImageSource ProvideValue(IServiceProvider serviceProvider)
    {
        if (String.IsNullOrEmpty(Source))
        {
            IXmlLineInfoProvider lineInfoProvider = serviceProvider.GetService(typeof(IXmlLineInfoProvider)) as IXmlLineInfoProvider;
            IXmlLineInfo lineInfo = (lineInfoProvider != null) ? lineInfoProvider.XmlLineInfo : new XmlLineInfo();
            throw new XamlParseException("ImageResourceExtension requires Source property to be set", lineInfo);
        }

        string assemblyName = GetType().GetTypeInfo().Assembly.GetName().Name;
        return ImageSource.FromResource(assemblyName + "." + Source, typeof(ImageResourceExtension).GetTypeInfo().Assembly);
    }

    object IMarkupExtension.ProvideValue(IServiceProvider serviceProvider)
    {
        return (this as IMarkupExtension<ImageSource>).ProvideValue(serviceProvider);
    }
}
```

`ImageResourceExtension`ist hilfreich, wenn eine XAML-Datei auf eine Bilddatei zugreifen muss, die im .NET Standard Bibliotheksprojekt als eingebettete Ressource gespeichert ist. Die-Eigenschaft wird verwendet `Source` , um die statische-Methode aufzurufen `ImageSource.FromResource` . Für diese Methode ist ein voll qualifizierter Ressourcen Name erforderlich, der aus dem Assemblynamen, dem Ordnernamen und dem Dateinamen besteht, die durch Punkte getrennt sind. Das zweite Argument für die- `ImageSource.FromResource` Methode stellt den Assemblynamen bereit und ist nur für Releasebuilds auf UWP erforderlich. Unabhängig davon `ImageSource.FromResource` muss von der Assembly aufgerufen werden, die die Bitmap enthält, d. h., diese XAML-Ressourcen Erweiterung kann nicht Teil einer externen Bibliothek sein, es sei denn, die Bilder sind ebenfalls in dieser Bibliothek enthalten. (Weitere Informationen zum Zugriff auf Bitmaps, die als eingebettete Ressourcen gespeichert sind, finden Sie im Artikel [**eingebettete Bilder**](~/xamarin-forms/user-interface/images.md#embedded-images) ).

Obwohl `ImageResourceExtension` erfordert `Source` , dass die-Eigenschaft festgelegt wird, wird die- `Source` Eigenschaft in einem-Attribut als Content-Eigenschaft der-Klasse angegeben. Dies bedeutet, dass der `Source=` Teil des Ausdrucks in geschweiften Klammern ausgelassen werden kann. Auf der **Demo Seite Bildressource** rufen die `Image` Elemente zwei Bilder ab, wobei der Ordnername und der Dateiname durch Punkte voneinander getrennt sind:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:MarkupExtensions"
             x:Class="MarkupExtensions.ImageResourceDemoPage"
             Title="Image Resource Demo">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Image Source="{local:ImageResource Images.SeatedMonkey.jpg}"
               Grid.Row="0" />

        <Image Source="{local:ImageResource Images.FacePalm.jpg}"
               Grid.Row="1" />

    </Grid>
</ContentPage>
```

Dies ist das Programm, das ausgeführt wird:

[![Demo zu Image Ressourcen](creating-images/imageresourcedemo-small.png "Demo zu Image Ressourcen")](creating-images/imageresourcedemo-large.png#lightbox "Demo zu Image Ressourcen")

## <a name="service-providers"></a>Dienstanbieter

Durch die Verwendung des- `IServiceProvider` Arguments für `ProvideValue` können XAML-Markup Erweiterungen Zugriff auf hilfreiche Informationen über die XAML-Datei erhalten, in der Sie verwendet werden. Um das `IServiceProvider` Argument jedoch erfolgreich zu verwenden, müssen Sie wissen, welche Art von Diensten in bestimmten Kontexten verfügbar ist. Die beste Möglichkeit, um dieses Feature zu verstehen, besteht darin, dass Sie den Quellcode vorhandener XAML-Markup Erweiterungen im [Ordner **MarkupExtensions** ](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Xaml/MarkupExtensions) im Xamarin.Forms Repository auf GitHub untersuchen. Beachten Sie, dass es sich bei einigen Dienst Typen um interne Dienste handelt Xamarin.Forms .

In einigen XAML-Markup Erweiterungen kann dieser Dienst nützlich sein:

```csharp
 IProvideValueTarget provideValueTarget = serviceProvider.GetService(typeof(IProvideValueTarget)) as IProvideValueTarget;
```

Die `IProvideValueTarget` -Schnittstelle definiert zwei Eigenschaften, `TargetObject` und `TargetProperty` . Wenn diese Informationen in der `ImageResourceExtension` -Klasse abgerufen werden, `TargetObject` ist `Image` und `TargetProperty` ein- `BindableProperty` Objekt für die- `Source` Eigenschaft von `Image` . Dies ist die Eigenschaft, für die die XAML-Markup Erweiterung festgelegt wurde.

Der-Befehl mit dem- `GetService` Argument `typeof(IProvideValueTarget)` gibt tatsächlich ein Objekt vom Typ zurück `SimpleValueTargetProvider` , das im- `Xamarin.Forms.Xaml.Internals` Namespace definiert ist. Wenn Sie den Rückgabewert von `GetService` in diesen Typ umwandeln, können Sie auch auf eine Eigenschaft zugreifen, bei der es sich um `ParentObjects` ein Array handelt, das das `Image` Element, das `Grid` übergeordnete Element und das `ImageResourceDemoPage` übergeordnete Element von enthält `Grid` .

## <a name="conclusion"></a>Zusammenfassung

XAML-Markup Erweiterungen spielen eine wichtige Rolle in XAML, indem Sie die Möglichkeit erweitern, Attribute aus einer Vielzahl von Quellen festzulegen. Wenn die vorhandenen XAML-Markup Erweiterungen nicht genau die benötigten Elemente enthalten, können Sie auch eigene schreiben.

## <a name="related-links"></a>Verwandte Links

- [Markup Erweiterungen (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)
- [Kapitel zu XAML-Markup Erweiterungen aus Xamarin.Forms Buch](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
