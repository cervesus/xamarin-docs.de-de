---
title: Erstellen von XAML-Markuperweiterungen
description: Definieren Sie Ihre eigenen benutzerdefinierten XAML-Markuperweiterungen
ms.prod: xamarin
ms.assetid: 797C1EF9-1C8E-4208-8610-9B79CCF17D46
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/05/2018
ms.openlocfilehash: d4ae3b42c5c926749310da6e36b6f4e9754d398c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="creating-xaml-markup-extensions"></a>Erstellen von XAML-Markuperweiterungen

Auf der Ebene des programmgesteuerten eine XAML-Markuperweiterung ist eine Klasse, implementiert die [ `IMarkupExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.IMarkupExtension/) oder [ `IMarkupExtension<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.IMarkupExtension%3CT%3E/) Schnittstelle. Sie können den Quellcode der standardmäßigen Markuperweiterungen finden Sie unten im Untersuchen der [ **MarkupExtensions** Directory](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Xaml/MarkupExtensions) des Xamarin.Forms GitHub-Repositorys. 

Es ist auch möglich, Ihre eigenen benutzerdefinierten XAML-Markuperweiterungen definieren, durch Ableiten von `IMarkupExtension` oder `IMarkupExtension<T>`. Verwenden Sie das generische Formular aus, wenn die Markuperweiterung einen Wert eines bestimmten Typs erhält. Dies ist der Fall bei einiger der Markuperweiterungen Xamarin.Forms:

- `TypeExtension` leitet sich von `IMarkupExtension<Type>`
- `ArrayExtension` leitet sich von `IMarkupExtension<Array>`
- `DynamicResourceExtension` leitet sich von `IMarkupExtension<DynamicResource>`
- `BindingExtension` leitet sich von `IMarkupExtension<BindingBase>`
- `ConstraintExpression` leitet sich von `IMarkupExtension<Constraint>`

Die beiden `IMarkupExtension` Schnittstellen definieren nur eine Methode mit dem Namen `ProvideValue`: 

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

Da `IMarkupExtension<T>` leitet sich von `IMarkupExtension` und enthält die `new` -Schlüsselwort in `ProvideValue`, er enthält sowohl `ProvideValue` Methoden.

Sehr häufig definieren Markuperweiterungen für XAML-Eigenschaften, die beitragen, auf den Rückgabewert. (Eine offensichtliche Ausnahme ist `NullExtension`, in dem `ProvideValue` gibt einfach auftragsantwortnachrichten zurück `null`.) Die `ProvideValue` Methode verfügt über ein einzelnes Argument vom Typ `IServiceProvider` , wird weiter unten in diesem Artikel diskutiert werden.

## <a name="a-markup-extension-for-specifying-color"></a>Eine Markuperweiterung für die Angabe von Farbe

Der folgende XAML-Markuperweiterung bietet die Möglichkeit zum Erstellen einer `Color` -Wert mit den Farbton, Sättigung und Helligkeit-Komponenten. Er definiert vier Eigenschaften für die vier Komponenten der Farbe, einschließlich einer alpha-Komponente, die mit 1 initialisiert wird. Die Klasse leitet sich von `IMarkupExtension<Color>` an, dass eine `Color` Rückgabewert:

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

Da `IMarkupExtension<T>` leitet sich von `IMarkupExtension`, muss die Klasse enthalten zwei `ProvideValue` Methoden, die gibt `Color` und ein anderes, das zurückgegeben `object`, aber die zweite Methode kann einfach die erste Methode aufrufen.

Die **HSL-Farbe Demo** Seite zeigt einer Vielzahl von Möglichkeiten, die `HslColorExtension` stehen in einer XAML-Datei an, die Farbe für eine `BoxView`:

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

Beachten Sie, dass bei `HslColorExtension` ist ein XML-Tag, die vier Eigenschaften werden als Attribute festgelegt, wenn es zwischen den geschweiften Klammern angezeigt wird, die vier Eigenschaften sind jedoch getrennt durch Semikolons ohne Anführungszeichen ein. Die Standardwerte für `H`, `S`, und `L` sind 0 und der Standardwert des `A` beträgt 1, sodass diese Eigenschaften ausgelassen werden, können Wenn Sie auf die Standardwerte festgelegt werden sollen. Im letzte Beispiel zeigt ein Beispiel, bei denen die Brillanz 0, d. h. normalerweise Schwarz ergibt, aber der alpha-Kanal ist 0,5, damit halb transparent und wird, anhand der weiße Hintergrund der Seite grauen:

[![HSL-Farbe Demo](creating-images/hslcolordemo-small.png "HSL-Farbe Demo")](creating-images/hslcolordemo-large.png#lightbox "HSL-Farbe-Demo")

## <a name="a-markup-extension-for-accessing-bitmaps"></a>Eine Markuperweiterung für den Zugriff auf Bitmaps

Das Argument für `ProvideValue` ist ein Objekt, implementiert die [ `IServiceProvider` ](https://developer.xamarin.com/api/type/System.IServiceProvider/) -Schnittstelle, die in .NET definiert ist `System` Namespace. Diese Schnittstelle verfügt über ein Element, eine Methode namens `GetService` mit einem `Type` Argument. 

Die `ImageResourceExtension` Klasse, die unten zeigt eine mögliche Verwendung der `IServiceProvider` und `GetService` zum Abrufen einer `IXmlLineInfoProvider` -Objekt, das Aufschluss kann Zeile und das Zeichen, der angibt, in denen ein bestimmter Fehler aufgetreten ist. In diesem Fall eine Ausnahme ausgelöst wird bei der `Source` Eigenschaft nicht festgelegt wurde:

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

        return ImageSource.FromResource(assemblyName + "." + Source);
    }

    object IMarkupExtension.ProvideValue(IServiceProvider serviceProvider)
    {
        return (this as IMarkupExtension<ImageSource>).ProvideValue(serviceProvider);
    }
}
```

`ImageResourceExtension` ist hilfreich, wenn eine XAML-Datei auf eine Bilddatei, die als eingebettete Ressource in die Portable Class Library-Projekt gespeichert muss. Er verwendet die `Source` zum Aufrufen der statischen Eigenschaft `ImageSource.FromResource` Methode. Diese Methode muss es sich um ein vollständig qualifizierter Ressourcenname, besteht aus den Assemblynamen, den Namen des Ordners und der Dateiname, die durch Punkte getrennt. Die `ImageResourceExtension` nicht müssen der Assemblyname Teil daran, dass er erhält der Name der Assembly mit Reflektion und voran, damit die `Source` Eigenschaft. Unabhängig davon, `ImageSource.FromResource` muss aufgerufen werden, aus der Assembly, die die Bitmap, was bedeutet enthält, dass diese Erweiterung der Verwendung von XAML-Ressource von einer externen Bibliothek kann nicht verwendet werden, wenn die Images auch in diese Bibliothek sind. (Siehe die [ **eingebettete Bilder** ](~/xamarin-forms/user-interface/images.md#embedded_images) Artikel finden Sie weitere Informationen zum Zugreifen auf Bitmaps, die als eingebettete Ressourcen gespeichert.) 

Obwohl `ImageResourceExtension` erfordert die `Source` festzulegende Eigenschaft, die `Source` Eigenschaft in einem Attribut angegeben ist, als die Content-Eigenschaft der Klasse. Dies bedeutet, dass die `Source=` Teil des Ausdrucks in geschweiften Klammern kann ausgelassen werden. In der **Image Ressource Demo** Seite der `Image` zwei Images, die mit den Namen des Ordners und der Dateiname, die durch Punkte getrennt Elemente abgerufen werden:

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

Hier wird das Programm auf allen drei Plattformen ausgeführt:

[![Bild-Ressource Demo](creating-images/imageresourcedemo-small.png "Image Ressource Demo")](creating-images/imageresourcedemo-large.png#lightbox "Image Ressource Demo")

## <a name="service-providers"></a>Dienstanbieter

Mithilfe der `IServiceProvider` Argument `ProvideValue`, XAML-Markuperweiterungen erhalten Zugriff auf nützliche Informationen zur Verwendung von XAML-Datei in dem sie verwendet werden können. Sondern verwenden die `IServiceProvider` Argument erfolgreich ist, müssen Sie wissen, welche Art von Services in bestimmten Kontexten verfügbar sind. Die beste Möglichkeit, Sie erhalten einen Überblick über diese Funktion wird durch den Quellcode der vorhandenen XAML-Markuperweiterungen in der [ **MarkupExtensions** Ordner](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Xaml/MarkupExtensions) im Xamarin.Forms-Repository auf GitHub. Denken Sie daran, dass einige Arten von Diensten für Xamarin.Forms intern sind.

In einigen XAML-Markuperweiterungen kann diesen Dienst nützlich sein:

```csharp
 IProvideValueTarget provideValueTarget = serviceProvider.GetService(typeof(IProvideValueTarget)) as IProvideValueTarget;
```

Die `IProvideValueTarget` Schnittstelle definiert zwei Eigenschaften `TargetObject` und `TargetProperty`. Wenn diese Informationen abgerufen wird, der `ImageResourceExtension` -Klasse, `TargetObject` ist die `Image` und `TargetProperty` ist eine `BindableProperty` -Objekt für die `Source` Eigenschaft des `Image`. Dies ist die Eigenschaft, auf der die Verwendung von XAML-Markuperweiterung festgelegt wurde.

Die `GetService` Aufruf mit dem Argument `typeof(IProvideValueTarget)` tatsächlich gibt ein Objekt vom Typ `SimpleValueTargetProvider`, definiert die `Xamarin.Forms.Xaml.Internals` Namespace. Wenn Sie den Rückgabewert der umgewandelt `GetService` auf diesen Typ können Sie auch zugreifen eine `ParentObjects` -Eigenschaft, die ein Array ist, enthält der `Image` -Element, die `Grid` übergeordneten, und die `ImageResourceDemoPage` übergeordnet der `Grid`.

## <a name="conclusion"></a>Schlussbemerkung

Verwendung von XAML-Markuperweiterungen spielen eine wichtige Rolle in XAML durch die Erweiterung der Möglichkeit, Attribute aus einer Vielzahl von Quellen festzulegen. Darüber hinaus, wenn die vorhandenen XAML-Markuperweiterungen nicht bereitstellen, was genau Sie benötigen, können Sie auch eigene schreiben. 


## <a name="related-links"></a>Verwandte Links

- [Markuperweiterungen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)
- [Verwendung von XAML-Markup Extensions Kapitel Xamarin.Forms Buchs](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
