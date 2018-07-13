---
title: Binden von Wertkonvertern Xamarin.Forms
description: In diesem Artikel wird erläutert, wie umwandeln oder konvertieren Werte in einer Xamarin.Forms-Datenbindung, durch die Implementierung eines Wertkonverters (die auch als ein Binding-Konverter oder Wertkonverter für die Bindung bezeichnet) wird.
ms.prod: xamarin
ms.assetid: 02B1BBE6-D804-490D-BDD4-8ACED8B70C92
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 28892692133020de1fa5a6eb007bb3f9bcf2612b
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997479"
---
# <a name="xamarinforms-binding-value-converters"></a>Binden von Wertkonvertern Xamarin.Forms

Datenbindungen übertragen von Daten in der Regel aus einer Quelleigenschaft auf eine Zieleigenschaft, und klicken Sie in einigen Fällen von der Zieleigenschaft die Source-Eigenschaft. Diese Übertragung ist einfach, wenn die Quelle und Ziel-Eigenschaften vom gleichen Typ sind, oder wenn ein Typ in den anderen Typ über eine implizite Konvertierung konvertiert werden kann. Wenn dies nicht der Fall ist, muss eine typkonvertierung durchgeführt werden.

In der [ **Zeichenfolgenformatierung** ](string-formatting.md) Artikel haben Sie gesehen, wie Sie verwenden können die `StringFormat` Eigenschaft eine Datenbindung an einen beliebigen Typ in eine Zeichenfolge zu konvertieren. Für andere Typen von Konvertierungen, müssen Sie einige spezielle Code in einer Klasse zu schreiben, implementiert die [ `IValueConverter` ](xref:Xamarin.Forms.IValueConverter) Schnittstelle. (Die universelle Windows-Plattform enthält eine ähnliche Klasse, die mit dem Namen [ `IValueConverter` ](/uwp/api/Windows.UI.Xaml.Data.IValueConverter/) in die `Windows.UI.Xaml.Data` Namespaces, sondern dies `IValueConverter` befindet sich in der `Xamarin.Forms` Namespace.) Klassen, in denen `IValueConverter` heißen *Wert Konverter*, aber sie sind auch bezeichnet als *binding-Konverter* oder *Binden von Wertkonvertern*.

## <a name="the-ivalueconverter-interface"></a>Die IValueConverter-Schnittstelle

Angenommen, Sie möchten, um eine Datenbindung zu definieren, in denen die Quelleigenschaft vom Typ ist `int` die Zieleigenschaft ist jedoch ein `bool`. Soll dieser Datenbindung zum Erzeugen einer `false` Wert, wenn die Quelle für die ganze Zahl gleich 0 (null) ist und `true` andernfalls.  

Hierzu können Sie mit einer Klasse, die implementiert die `IValueConverter` Schnittstelle:

```csharp
public class IntToBoolConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (int)value != 0;
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (bool)value ? 1 : 0;
    }
}
```

Festlegen, dass eine Instanz dieser Klasse, die [ `Converter` ](xref:Xamarin.Forms.Binding.Converter) Eigenschaft der `Binding` Klasse oder die [ `Converter` ](xref:Xamarin.Forms.Xaml.BindingExtension.Converter) Eigenschaft der `Binding` Markuperweiterung. Diese Klasse wird die Datenbindung.

Die `Convert` Methode wird aufgerufen, wenn Daten aus der Quelle in das Ziel in Verschiebt `OneWay` oder `TwoWay` Bindungen. Die `value` -Parameter ist das Objekt oder der Wert aus der Quelle für die Datenbindung. Die Methode muss einen Wert vom Typ des Ziels Datenbindung zurückgeben. Die Methode, die hier Umwandlungen der `value` Parameter, um eine `int` und vergleicht sie anschließend mit 0 für eine `bool` Wert zurückgeben.

Die `ConvertBack` Methode wird aufgerufen, wenn Daten vom Ziel zur Quelle in Verschiebt `TwoWay` oder `OneWayToSource` Bindungen. `ConvertBack` führt die umgekehrte Konvertierung: Es wird davon ausgegangen der `value` -Parameter ist ein `bool` aus dem Ziel, und konvertiert sie in einer `int` -Wert für die Quelle zurückgegeben.

Wenn die Datenbindung auch enthält eine `StringFormat` festlegen, der Wertkonverter wird aufgerufen, bevor das Ergebnis als Zeichenfolge formatiert wird.

Die **aktivieren Schaltflächen** auf der Seite die [ **Datenbindung Demos** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) Beispiel veranschaulicht, wie diese Wertkonverter in einer Datenbindung zu verwenden. Die `IntToBoolConverter` im Ressourcenverzeichnis der Seite instanziiert wird. Darauf verwiesen wird dann mit einem `StaticResource` -Markuperweiterung festgelegt die `Converter` -Eigenschaft in zwei datenbindungen. Es ist üblich, Datenkonverter zwischen mehreren datenbindungen, die auf der Seite gemeinsam zu nutzen:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.EnableButtonsPage"
             Title="Enable Buttons">
    <ContentPage.Resources>
        <ResourceDictionary>
            <local:IntToBoolConverter x:Key="intToBool" />
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Padding="10, 0">
        <Entry x:Name="entry1"
               Text=""
               Placeholder="enter search term"
               VerticalOptions="CenterAndExpand" />

        <Button Text="Search"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                IsEnabled="{Binding Source={x:Reference entry1},
                                    Path=Text.Length,
                                    Converter={StaticResource intToBool}}" />

        <Entry x:Name="entry2"
               Text=""
               Placeholder="enter destination"
               VerticalOptions="CenterAndExpand" />

        <Button Text="Submit"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                IsEnabled="{Binding Source={x:Reference entry2},
                                    Path=Text.Length,
                                    Converter={StaticResource intToBool}}" />
    </StackLayout>
</ContentPage>
```

Wenn Sie ein Wertkonverter auf mehreren Seiten der Anwendung verwendet wird, können Sie Sie instanziieren im Ressourcenverzeichnis in den **"App.xaml"** Datei.

Die **Schaltflächen aktivieren** Seite zeigt eine allgemeine erforderlich, wenn eine `Button` führt einen Vorgang, der basierend auf den Text, der der Benutzer, in eingibt eine `Entry` anzeigen. Wenn "nothing" in typisiert wurde die `Entry`, `Button` sollte deaktiviert werden. Jede `Button` enthält eine Datenbindung für die `IsEnabled` Eigenschaft. Die Quelle für die Datenbindung ist der `Length` Eigenschaft der `Text` -Eigenschaft des entsprechenden `Entry`. Wenn das `Length` Eigenschaft ist nicht 0 (null) Gibt zurück, der Wertkonverter `true` und `Button` aktiviert ist:

[![Aktivieren Sie die Schaltflächen](converters-images/enablebuttons-small.png "Schaltflächen aktivieren")](converters-images/enablebuttons-large.png#lightbox "Schaltflächen aktivieren")

Beachten Sie, dass die `Text` Eigenschaft in den einzelnen `Entry` auf eine leere Zeichenfolge initialisiert wird. Die `Text` Eigenschaft `null` standardmäßig, und die Daten Bindung funktioniert nicht in diesem Fall.

Einige Wertkonverter werden speziell für bestimmte Anwendungen geschrieben, während andere verallgemeinert werden. Wenn Sie wissen, dass ein Wertkonverter in verwendet werden, `OneWay` Bindungen, und klicken Sie dann die `ConvertBack` Methode einfach zurückgeben `null`.

Die `Convert` implizit oben gezeigten Methode setzt voraus, dass die `value` Argument ist vom Typ `int` und der Rückgabewert muss vom Typ `bool`. Auf ähnliche Weise die `ConvertBack` Methode setzt voraus, dass die `value` Argument ist vom Typ `bool` und der Rückgabewert ist `int`. Wenn dies nicht der Fall ist, wird eine Laufzeitausnahme ausgelöst.

Sie können den Wertkonverter mehr generalisiert werden und akzeptiert verschiedene Arten von Daten schreiben. Die `Convert` und `ConvertBack` Methoden können die `as` oder `is` Operatoren mit den `value` -Parameter oder durch Aufrufen `GetType` für diesen Parameter, um dessen Typ ein, und führen Sie etwas zu bestimmen, auf die entsprechende. Der erwartete Typ des Rückgabewerts der Methode, angegeben durch die `targetType` Parameter. In einigen Fällen werden die Wertkonverter mit datenbindungen, die von unterschiedlichen Zieltypen verwendet. der Wertkonverter können die `targetType` Argument beim Ausführen einer Konvertierung für den richtigen Typ.

Wenn die Konvertierung ausgeführt wird für die verschiedenen Kulturen unterschiedlich ist, verwenden die `culture` Parameter für diesen Zweck. Die `parameter` Argument `Convert` und `ConvertBack` wird weiter unten in diesem Artikel.

## <a name="binding-converter-properties"></a>Binding-Konverter-Eigenschaften

Wertkonverterklassen können es sich um Eigenschaften und generische Parameter aufweisen. Dieser bestimmte Wertkonverter konvertiert eine `bool` aus der Quelle in ein Objekt des Typs `T` für das Ziel:

```csharp
public class BoolToObjectConverter<T> : IValueConverter
{
    public T TrueObject { set; get; }

    public T FalseObject { set; get; }

    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (bool)value ? TrueObject : FalseObject;
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return ((T)value).Equals(TrueObject);
    }
}
```

Die **Switch Indikatoren** Seite erläutert, wie sie verwendet werden kann, um den Wert der an eine `Switch` anzeigen. Obwohl es üblich, Value Converter als Ressourcen in einem Ressourcenverzeichnis instanziiert ist, wird auf dieser Seite zeigt eine Alternative: jeder Wertkonverter wird instanziiert, zwischen `Binding.Converter` Eigenschaftenelement Tags. Die `x:TypeArguments` das generische Argument, und `TrueObject` und `FalseObject` auf Objekte dieses Typs festlegen:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.SwitchIndicatorsPage"
             Title="Switch Indicators">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="FontSize" Value="18" />
                <Setter Property="VerticalOptions" Value="Center" />
            </Style>

            <Style TargetType="Switch">
                <Setter Property="VerticalOptions" Value="Center" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Padding="10, 0">
        <StackLayout Orientation="Horizontal"
                     VerticalOptions="CenterAndExpand">
            <Label Text="Subscribe?" />
            <Switch x:Name="switch1" />
            <Label>
                <Label.Text>
                    <Binding Source="{x:Reference switch1}"
                             Path="IsToggled">
                        <Binding.Converter>
                            <local:BoolToObjectConverter x:TypeArguments="x:String"
                                                         TrueObject="Of course!"
                                                         FalseObject="No way!" />
                        </Binding.Converter>
                    </Binding>
                </Label.Text>
            </Label>
        </StackLayout>

        <StackLayout Orientation="Horizontal"
                     VerticalOptions="CenterAndExpand">
            <Label Text="Allow popups?" />
            <Switch x:Name="switch2" />
            <Label>
                <Label.Text>
                    <Binding Source="{x:Reference switch2}"
                             Path="IsToggled">
                        <Binding.Converter>
                            <local:BoolToObjectConverter x:TypeArguments="x:String"
                                                         TrueObject="Yes"
                                                         FalseObject="No" />
                        </Binding.Converter>
                    </Binding>
                </Label.Text>
                <Label.TextColor>
                    <Binding Source="{x:Reference switch2}"
                             Path="IsToggled">
                        <Binding.Converter>
                            <local:BoolToObjectConverter x:TypeArguments="Color"
                                                         TrueObject="Green"
                                                         FalseObject="Red" />
                        </Binding.Converter>
                    </Binding>
                </Label.TextColor>
            </Label>
        </StackLayout>

        <StackLayout Orientation="Horizontal"
                     VerticalOptions="CenterAndExpand">
            <Label Text="Learn more?" />
            <Switch x:Name="switch3" />
            <Label FontSize="18"
                   VerticalOptions="Center">
                <Label.Style>
                    <Binding Source="{x:Reference switch3}"
                             Path="IsToggled">
                        <Binding.Converter>
                            <local:BoolToObjectConverter x:TypeArguments="Style">
                                <local:BoolToObjectConverter.TrueObject>
                                    <Style TargetType="Label">
                                        <Setter Property="Text" Value="Indubitably!" />
                                        <Setter Property="FontAttributes" Value="Italic, Bold" />
                                        <Setter Property="TextColor" Value="Green" />
                                    </Style>                                    
                                </local:BoolToObjectConverter.TrueObject>

                                <local:BoolToObjectConverter.FalseObject>
                                    <Style TargetType="Label">
                                        <Setter Property="Text" Value="Maybe later" />
                                        <Setter Property="FontAttributes" Value="None" />
                                        <Setter Property="TextColor" Value="Red" />
                                    </Style>
                                </local:BoolToObjectConverter.FalseObject>
                            </local:BoolToObjectConverter>
                        </Binding.Converter>
                    </Binding>
                </Label.Style>
            </Label>
        </StackLayout>
    </StackLayout>
</ContentPage>
```

In den letzten drei `Switch` und `Label` -Paaren, das generische Argument nastaven NA hodnotu `Style`, und der gesamte `Style` Objekte werden bereitgestellt, für die Werte der `TrueObject` und `FalseObject`. Diese außer Kraft setzen, die impliziten Stil für `Label` im Ressourcenverzeichnis festgelegt, also die Eigenschaften in dieser Art werden explizit zugewiesen die `Label`. Umschalten der `Switch` bewirkt, dass die entsprechenden `Label` entsprechend die Änderung:

[![Wechseln Sie Indikatoren](converters-images/switchindicators-small.png "wechseln Indikatoren")](converters-images/switchindicators-large.png#lightbox "Indikatoren wechseln")

Es ist auch möglich mit [ `Triggers` ](~/xamarin-forms/app-fundamentals/triggers.md) ähnliche Änderungen in der Benutzeroberfläche, basierend auf den anderen Ansichten zu implementieren.

## <a name="binding-converter-parameters"></a>Konverter Bindungsparameter

Die `Binding` -Klasse definiert eine [ `ConverterParameter` ](xref:Xamarin.Forms.Binding.ConverterParameter) -Eigenschaft, und die `Binding` Markuperweiterung definiert auch eine [ `ConverterParameter` ](xref:Xamarin.Forms.Xaml.BindingExtension.ConverterParameter) Eigenschaft. Wenn diese Eigenschaft wird festgelegt, wird der Wert, um übergeben wird die `Convert` und `ConvertBack` Methoden wie die `parameter` Argument. Auch wenn die Instanz für einen Wertkonverter, wenn mehrere datenbindungen aufgeteilt werden, die `ConverterParameter` können etwas andere Konvertierungen unterschiedlich sein.

Die Verwendung von `ConverterParameter` wird mit einem Programm Farbauswahl veranschaulicht. In diesem Fall die `RgbColorViewModel` verfügt über drei Eigenschaften des Typs `double` mit dem Namen `Red`, `Green`, und `Blue` , die zum Erstellen verwendet eine `Color` Wert:

```csharp
public class RgbColorViewModel : INotifyPropertyChanged
{
    Color color;
    string name;

    public event PropertyChangedEventHandler PropertyChanged;

    public double Red
    {
        set
        {
            if (color.R != value)
            {
                Color = new Color(value, color.G, color.B);
            }
        }
        get
        {
            return color.R;
        }
    }

    public double Green
    {
        set
        {
            if (color.G != value)
            {
                Color = new Color(color.R, value, color.B);
            }
        }
        get
        {
            return color.G;
        }
    }

    public double Blue
    {
        set
        {
            if (color.B != value)
            {
                Color = new Color(color.R, color.G, value);
            }
        }
        get
        {
            return color.B;
        }
    }

    public Color Color
    {
        set
        {
            if (color != value)
            {
                color = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Red"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Green"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Blue"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Color"));

                Name = NamedColor.GetNearestColorName(color);
            }
        }
        get
        {
            return color;
        }
    }

    public string Name
    {
        private set
        {
            if (name != value)
            {
                name = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Name"));
            }
        }
        get
        {
            return name;
        }
    }
}
```

Die `Red`, `Green`, und `Blue` Eigenschaften Bereich zwischen 0 und 1. Allerdings empfiehlt es sich, dass die Komponenten werden als hexadezimale Werte zwei Ziffern angezeigt.

Um diese als hexadezimale Werte in XAML anzuzeigen, sie müssen 255 multipliziert, in eine ganze Zahl konvertiert und dann mit einer Spezifikation eines "X2" im Format der `StringFormat` Eigenschaft. Die ersten beiden Aufgaben (Multiplikation mit 255 und Konvertieren in eine ganze Zahl) können von den Wertkonverter behandelt werden. Um den Wertkonverter als generalisiert wie möglich zu gestalten, kann mit der Multiplikationsfaktor angegeben werden die `ConverterParameter` -Eigenschaft, d. h. er gibt die `Convert` und `ConvertBack` Methoden wie die `parameter` Argument:

```csharp
public class DoubleToIntConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (int)Math.Round((double)value * GetParameter(parameter));
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (int)value / GetParameter(parameter);
    }

    double GetParameter(object parameter)
    {
        if (parameter is double)
            return (double)parameter;

        else if (parameter is int)
            return (int)parameter;

        else if (parameter is string)
            return double.Parse((string)parameter);

        return 1;
    }
}
```

Die `Convert` konvertiert eine `double` zu `int` beim Multiplizieren mit der `parameter` Wert; die `ConvertBack` teilt die ganze Zahl `value` Argument nach `parameter` und gibt eine `double` Ergebnis. (Im unten gezeigten-Programm der Wertkonverter dient nur in Verbindung mit der Formatierung von Zeichenfolgen, also `ConvertBack` wird nicht verwendet.)

Der Typ des der `parameter` Argument ist wahrscheinlich variieren, je nachdem, ob die Datenbindung im Code oder in XAML definiert ist. Wenn die `ConverterParameter` Eigenschaft `Binding` festgelegt ist im Code, es ist wahrscheinlich auf einen numerischen Wert festgelegt werden:

```csharp
binding.ConverterParameter = 255;
```

Die `ConverterParameter` Eigenschaft ist vom Typ `Object`, sodass der C#-Compiler das literal 255 als ganze Zahl interpretiert und legt die Eigenschaft auf diesen Wert fest.

In XAML, jedoch die `ConverterParameter` ist wahrscheinlich, wie folgt festgelegt werden:

```xaml
<Label Text="{Binding Red,
                      Converter={StaticResource doubleToInt},
                      ConverterParameter=255,
                      StringFormat='Red = {0:X2}'}" />
```

Die 255 sieht wie eine Zahl, aber aus, da `ConverterParameter` ist vom Typ `Object`, der XAML-Parser behandelt die 255 als Zeichenfolge.

Aus diesem Grund enthält der oben gezeigte Wertkonverter ein separates `GetParameter` Methode, die Fälle, in denen behandelt `parameter` Typ `double`, `int`, oder `string`.  

Die **RGB-Farbauswahl** Seite instanziiert `DoubleToIntConverter` in dessen Ressourcenverzeichnis, befolgen die Definition von zwei implizite Stile:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.RgbColorSelectorPage"
             Title="RGB Color Selector">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Slider">
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>

            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>

            <local:DoubleToIntConverter x:Key="doubleToInt" />
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout>
        <StackLayout.BindingContext>
            <local:RgbColorViewModel Color="Gray" />
        </StackLayout.BindingContext>

        <BoxView Color="{Binding Color}"
                 VerticalOptions="FillAndExpand" />

        <StackLayout Margin="10, 0">
            <Label Text="{Binding Name}" />

            <Slider Value="{Binding Red}" />
            <Label Text="{Binding Red,
                                  Converter={StaticResource doubleToInt},
                                  ConverterParameter=255,
                                  StringFormat='Red = {0:X2}'}" />

            <Slider Value="{Binding Green}" />
            <Label Text="{Binding Green,
                                  Converter={StaticResource doubleToInt},
                                  ConverterParameter=255,
                                  StringFormat='Green = {0:X2}'}" />

            <Slider Value="{Binding Blue}" />
            <Label>
                <Label.Text>
                    <Binding Path="Blue"
                             StringFormat="Blue = {0:X2}"
                             Converter="{StaticResource doubleToInt}">
                        <Binding.ConverterParameter>
                            <x:Double>255</x:Double>
                        </Binding.ConverterParameter>
                    </Binding>
                </Label.Text>
            </Label>
        </StackLayout>
    </StackLayout>
</ContentPage>    
```

Die Werte der `Red` und `Green` Eigenschaften werden angezeigt, mit einem `Binding` Markuperweiterung. Die `Blue` Eigenschaft instanziiert jedoch die `Binding` Klasse wie eine explizite veranschaulicht `double` Wert kann festgelegt werden, um `ConverterParameter` Eigenschaft.

Hier ist das Ergebnis:

[![RGB-Farbauswahl](converters-images/rgbcolorselector-small.png "RGB-Farbauswahl")](converters-images/rgbcolorselector-large.png#lightbox "RGB-Farbauswahl")


## <a name="related-links"></a>Verwandte Links

- [Binden von Daten-Demos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Im Kapitel Daten-Bindung von Xamarin.Forms-Buch](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
