---
title: Verwenden von Schieberegler
description: Verwenden Sie einen Schieberegler für die Auswahl aus einem Bereich fortlaufender Werte ein.
ms.prod: xamarin
ms.assetid: 36B1C645-26E0-4874-B6B6-BDBF77662878
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/16/2018
ms.openlocfilehash: 99109f6377037ffb9f622b7ddb237b42d241e505
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="using-slider"></a>Verwenden von Schieberegler

_Verwenden Sie einen Schieberegler für die Auswahl aus einem Bereich fortlaufender Werte ein._

Der Xamarin.Forms [ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) wird ein horizontaler Balken an, die durch den Benutzer zur Auswahl bearbeitet werden kann eine `double` Wert aus einem kontinuierlichen Bereich. 

Die `Slider` definiert drei Eigenschaften des Typs `double`:

- [`Minimum`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Minimum/) ist der minimale Wert des Bereichs liegt, hat den Standardwert 0.
- [`Maximum`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Maximum/) ist das Maximum des Bereichs, hat den Standardwert 1.
- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Value/) ist der Schieberegler-Wert, der im Bereich zwischen kann `Minimum` und `Maximum` und hat den Standardwert 0.

Alle drei Eigenschaften werden durch gestützt `BindableProperty` Objekte. Die `Value` Eigenschaft verfügt über eine Bindung Standardmodus `BindingMode.TwoWay`, was bedeutet, dass sie als Bindungsquelle in einer Anwendung geeignet ist, verwendet der [Model-View-ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) Architektur. 

> [!WARNING]
> Intern können die `Slider` wird sichergestellt, dass `Minimum` ist kleiner als `Maximum`. Wenn `Minimum` oder `Maximum` jemals festgelegt sind, damit `Minimum` ist nicht kleiner als `Maximum`, wird eine Ausnahme ausgelöst. Finden Sie unter der [ **Vorsichtsmaßnahmen** ](#precautions) weiter unten im Abschnitt Weitere Informationen zum Einrichten der `Minimum` und `Maximum` Eigenschaften.

Die `Slider` wandelt die `Value` Eigenschaft, sodass diese zwischen ist `Minimum` und `Maximum`(einschließlich). Wenn die `Minimum` ist-Eigenschaftensatz auf einen Wert größer als die `Value` -Eigenschaft, die `Slider` legt die `Value` Eigenschaft, um `Minimum`. Auf ähnliche Weise, wenn `Maximum` auf einen Wert festgelegt ist kleiner als `Value`, klicken Sie dann `Slider` legt die `Value` Eigenschaft, um `Maximum`. 

`Slider` definiert eine [ `ValueChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Slider.ValueChanged/) Ereignis, wenn ausgelöst wird, die `Value` geändert wird, entweder durch Bearbeitung der Benutzer von der `Slider` oder wenn die Anwendung setzt die `Value` -Eigenschaft direkt. Ein `ValueChanged` Ereignis wird auch ausgelöst, wenn die `Value` Eigenschaft umgewandelt wird, wie im vorherigen Absatz beschrieben. 

Die [ `ValueChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ValueChangedEventArgs/) -Objekt, das mit der `ValueChanged` Ereignis verfügt über zwei Eigenschaften, die sowohl vom Typ `double`: [ `OldValue` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ValueChangedEventArgs.OldValue/) und [ `NewValue` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ValueChangedEventArgs.NewValue/). Zum Zeitpunkt das Ereignis ausgelöst wird, den Wert der `NewValue` ist identisch mit der `Value` Eigenschaft von der `Slider` Objekt.

> [!WARNING]
> Verwenden Sie nicht die uneingeschränkte horizontale Layoutoptionen von `Center`, `Start`, oder `End` mit `Slider`. Auf Android- und das universelle Windows-Plattform die `Slider` reduziert, um einen Balken der Länge Null, und klicken Sie auf der Leiste iOS ist sehr kurz. Behalten Sie den Standardwert `HorizontalOptions` Festlegen von `Fill`, und verwenden Sie keine Breite von `Auto` beim `Slider` in einer `Grid` Layout.

## <a name="basic-slider-code-and-markup"></a>Grundlegende Schieberegler Code- und Markupdateien

Die [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) Beispiel beginnt mit drei Seiten, die funktionell identisch sind, jedoch auf unterschiedliche Weise implementiert werden. Die erste Seite wird nur C#-Code verwendet, beim zweiten wird XAML verwendet, mit einem Ereignishandler im Code und der dritte ist, können Sie den Ereignishandler zu vermeiden, indem Sie mit der Datenbindung in der XAML-Datei.

### <a name="creating-a-slider-in-code"></a>Erstellen einen Schieberegler in code

Die **grundlegende Schieberegler Code** auf der Seite der [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) Beispiel wird gezeigt, anzeigen, erstellen Sie eine `Slider` und zwei `Label` -Objekte im Code:

```csharp
public class BasicSliderCodePage : ContentPage
{
    public BasicSliderCodePage()
    {
        Label rotationLabel = new Label
        {
            Text = "ROTATING TEXT",
            FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)),
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        Label displayLabel = new Label
        {
            Text = "(uninitialized)",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        Slider slider = new Slider
        {
            Maximum = 360
        };
        slider.ValueChanged += (sender, args) =>
        {
            rotationLabel.Rotation = slider.Value;
            displayLabel.Text = String.Format("The Slider value is {0}", args.NewValue);
        };

        Title = "Basic Slider Code";
        Padding = new Thickness(10, 0);
        Content = new StackLayout
        {
            Children =
            {
                rotationLabel,
                slider,
                displayLabel
            }
        };
    }
}
```

Die `Slider` wird initialisiert, damit eine `Maximum` Eigenschaft 360. Der `ValueChanged` Handler der `Slider` verwendet die `Value` Eigenschaft der `slider` Objekt festgelegt der `Rotation` Eigenschaft des ersten `Label` und verwendet die `String.Format` Methode mit der `NewValue` Eigenschaft des der Ereignisargumente, um das Festlegen der `Text` Eigenschaft des zweiten `Label`. Diese beiden Ansätze zum Abrufen des aktuellen Werts der die `Slider` sind austauschbar. 

Hier wird das Programm Geräte unter iOS, Android und universelle Windows-Plattform (UWP) ausgeführt wird:

[![Grundlegende Schieberegler Code](slider-images/BasicSliderCode.png "grundlegende Schieberegler-Code")](slider-images/BasicSliderCode-Large.png#lightbox)

Die zweite `Label` zeigt den Text "(nicht initialisierte)", bis die `Slider` bearbeitet wird, die Fälle der ersten `ValueChanged` Ereignis ausgelöst werden. Beachten Sie, dass die Anzahl der Dezimalstellen, die angezeigt werden, für die drei Plattformen anders ist. Diese Unterschiede beziehen sich auf die Plattform Implementierungen der `Slider` und werden weiter unten in diesem Artikel im Abschnitt [Unterschieden bei der Implementierung Plattform](#implementations).

### <a name="creating-a-slider-in-xaml"></a>Erstellen einen Schieberegler in XAML

Die **grundlegende Schieberegler XAML** Seite ist funktionell identisch **grundlegende Schieberegler Code** jedoch meist in XAML implementiert:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SliderDemos.BasicSliderXamlPage"
             Title="Basic Slider XAML"
             Padding="10, 0">
    <StackLayout>
        <Label x:Name="rotatingLabel" 
               Text="ROTATING TEXT"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider Maximum="360"
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="displayLabel"
               Text="(uninitialized)"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Die Code-Behind-Datei enthält den Handler für das `ValueChanged` Ereignis:

```csharp
public partial class BasicSliderXamlPage : ContentPage
{
    public BasicSliderXamlPage()
    {
        InitializeComponent();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        double value = args.NewValue;
        rotatingLabel.Rotation = value;
        displayLabel.Text = String.Format("The Slider value is {0}", value);
    }
}
```

Es ist auch möglich, für den Ereignishandler zum Abrufen der `Slider` ist, die das Ereignis durch Auslösen der `sender` Argument. Die `Value` Eigenschaft enthält den aktuellen Wert:

```csharp
double value = ((Slider)sender).Value;
```

Wenn die `Slider` Objekt ausgehändigten einen Namen in der XAML-Datei mit einer `x:Name` Attribut (z. B. "Slider"), und klicken Sie dann auf der Ereignishandler kann dieses Objekt verweisen Sie direkt auf:

```csharp
double value = slider.Value;
```

### <a name="data-binding-the-slider"></a>Den Schieberegler für die Datenbindung

Die **grundlegende Schieberegler Bindungen** Seite wird gezeigt, wie ein nahezu äquivalente Programm zu schreiben, die eliminiert die `Value` Ereignishandler mithilfe [zum Binden von Daten](~/xamarin-forms/app-fundamentals/data-binding/index.md):

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SliderDemos.BasicSliderBindingsPage"
             Title="Basic Slider Bindings"
             Padding="10, 0">
    <StackLayout>
        <Label Text="ROTATING TEXT"
               Rotation="{Binding Source={x:Reference slider}, 
                                  Path=Value}"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                Maximum="360" />

        <Label x:Name="displayLabel"
               Text="{Binding Source={x:Reference slider}, 
                              Path=Value, 
                              StringFormat='The Slider value is {0:F0}'}"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Die `Rotation` Eigenschaft des ersten `Label` gebunden ist der `Value` Eigenschaft der `Slider`, da der `Text` Eigenschaft des zweiten `Label` mit einer `StringFormat` Spezifikation. Die **grundlegende Schieberegler Bindungen** Seite Funktionen etwas anders aus den beiden vorherigen Seiten: Wenn die Seite zuerst angezeigt wird, wobei der zweite `Label` zeigt die Zeichenfolge mit dem Wert. Hierin liegt der Vorteil der Verwendung der Datenbindung. Um Text ohne Datenbindung anzuzeigen, müssen Sie explizit Initialisieren der `Text` Eigenschaft von der `Label` oder simulieren Sie eine Auslösen von der `ValueChanged` Ereignis durch Aufrufen des ereignishandlers über den Klassenkonstruktor.

<a name="precautions" />

## <a name="precautions"></a>Vorsichtsmaßnahmen

Der Wert des der `Minimum` Eigenschaft muss immer kleiner als der Wert, der die `Maximum` Eigenschaft. Der folgende code Snippet Ursachen der `Slider` zum Auslösen einer Ausnahme:

```csharp
// Throws an exception!
Slider slider = new Slider
{
    Minimum = 10,
    Maximum = 20
};
```

Der C#-Compiler generiert Code, der diese beiden Eigenschaften in der Sequenz ist, legt diese fest und wann die `Minimum` -Eigenschaft auf 10 festgelegt ist, ist größer als der Standardwert `Maximum` Wert 1. Sie können die Ausnahme in diesem Fall vermeiden, indem die `Maximum` Eigenschaft erste:

```csharp
Slider slider = new Slider
{
    Maximum = 20,
    Minimum = 10
};
```

Festlegen von `Maximum` 20 ist kein Problem, da sie größer als der Standardwert ist `Minimum` 0 festlegen. Wenn `Minimum` festgelegt ist, wird der Wert ist kleiner als das `Maximum` Wert von 20.

Das gleiche Problem tritt in XAML. Legen Sie die Eigenschaften in der Reihenfolge, die garantiert, dass `Maximum` ist immer größer als `Minimum`:

```xaml
<Slider Maximum="20"
        Minimum="10" ... />
```

Sie können festlegen, die `Minimum` und `Maximum` Werte an eine negative Zahl, jedoch nur in einer Reihenfolge, in dem `Minimum` ist immer kleiner als `Maximum`:

```xaml
<Slider Minimum="-20"
        Maximum="-10" ... />
```

Die `Value` Eigenschaft ist immer größer als oder gleich der `Minimum` Wert und kleiner als oder gleich `Maximum`. Wenn `Value` festgelegt ist auf einen Wert außerhalb dieses Bereichs liegt, wird der Wert im Bereich liegen zwingend werden, jedoch wird keine Ausnahme ausgelöst. Dieser Code z. B. wird *nicht* eine Ausnahme ausgelöst:

```csharp
Slider slider = new Slider
{
    Value = 10
};
```

Stattdessen die `Value` Eigenschaft umgewandelt, in dem `Maximum` Wert 1.

So sieht ein oben gezeigten Codeausschnitt aus: 

```csharp
Slider slider = new Slider
{
    Maximum = 20,
    Minimum = 10
};
```

Wenn `Minimum` klicken Sie dann auf 10 festgelegt ist `Value` wird ebenfalls auf 10 festgelegt. 

Wenn eine `ValueChanged` Ereignishandler zum Zeitpunkt angefügt wurde, die `Value` Eigenschaft wird in einen anderen Wert als den Standardwert 0 (null) umgewandelt und dann eine `ValueChanged` Ereignis ausgelöst wird. So sieht ein XAML-Ausschnitt aus: 

```xaml
<Slider ValueChanged="OnSliderValueChanged"
        Maximum="20"
        Minimum="10" />
```

Wenn `Minimum` wird auf 10 festgelegt `Value` wird ebenfalls auf 10 festgelegt und die `ValueChanged` Ereignis ausgelöst wird. Dies kann auftreten, bevor der Rest der Seite erstellt wurden wurde und der Handler versuchen, andere Elemente auf der Seite zu verweisen, die noch nicht erstellt wurde. Möglicherweise möchten Sie Code zum Hinzufügen der `ValueChanged` Handler, die prüft, ob `null` Werte aus anderen Elementen auf der Seite. Oder Sie können festlegen, die `ValueChanged` -Ereignishandler nach der `Slider` Werte initialisiert wurden.

<a name="implementations" />

## <a name="platform-implementation-differences"></a>Plattform Unterschieden bei der Implementierung

Die weiter oben dargestellten Screenshots zeigt den Wert der die `Slider` mit einer unterschiedlichen Anzahl von Dezimalstellen. Dies bezieht sich auf wie die `Slider` auf die Plattformen Android und universelle Windows-Plattform implementiert wird.

### <a name="the-android-implementation"></a>Die Android-Implementierung 

Die Android-Implementierung von `Slider` basiert auf dem Android [ `SeekBar` ](https://developer.xamarin.com/api/type/Android.Widget.SeekBar/) und legt die [ `Max` ](https://developer.xamarin.com/api/property/Android.Widget.ProgressBar.Max/) Eigenschaft bis 1000. Dies bedeutet, dass die `Slider` für Android verfügt nur 1.001 diskrete Werte. Wenn Sie festlegen der `Slider` haben eine `Minimum` 0 und eine `Maximum` von 5000 aus, und klicken Sie dann als der `Slider` bearbeitet wird, die `Value` Eigenschaft enthält die Werte von 0, 5, 10, 15 und So weiter. 

### <a name="the-uwp-implementation"></a>Die uwp-Implementierung

Die uwp-Implementierung von `Slider` basiert auf UWP [ `Slider` ](/uwp/api/windows.ui.xaml.controls.slider) Steuerelement. Die `StepFrequency` Eigenschaft UWP `Slider` festgelegt ist, um die Differenz zwischen der `Maximum` und `Minimum` Eigenschaften unterteilt, 10, aber nicht größer als 1. 

Für den Standardbereich von 0 bis 1, beispielsweise die `StepFrequency` Eigenschaft auf 0,1 festgelegt ist. Als die `Slider` bearbeitet wird, die `Value` Eigenschaft auf 0, 0,1, 0,2, 0,3, 0,4, 0,5, 0,6, 0,7, 0,8, 0,9 und 1.0 beschränkt ist. (Dies liegt in der letzten Seite in der [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) Beispiel.) Wenn der Unterschied zwischen der `Maximum` und `Minimum` Eigenschaften ist 10 oder höher, klicken Sie dann `StepFrequency` auf 1 festgelegt ist und die `Value` Eigenschaft hat ganzzahlige Werte. 

### <a name="the-stepslider-solution"></a>Die Lösung StepSlider

Ein flexibler `StepSlider` finden [Kapitel 27. Benutzerdefinierte Renderer](https://xamarin.azureedge.net/developer/xamarin-forms-book/XamarinFormsBook-Ch27-Apr2016.pdf) des Buchs *Erstellen mobiler Apps mit Xamarin.Forms*. Die `StepSlider` ähnelt `Slider` fügt jedoch ein `Steps` Eigenschaft, um die Anzahl der Werte zwischen anzugeben `Minimum` und `Maximum`.

## <a name="sliders-for-color-selection"></a>Schieberegler für die Farbauswahl

Der endgültige zwei Seiten der [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) Sample beide verwenden drei `Slider` Instanzen für die Farbauswahl. Die erste Seite behandelt alle Aktivitäten in der CodeBehind-Datei während die zweite Seite wie die Datenbindung mit einem ViewModel verwendet wird. 

### <a name="handling-sliders-in-the-code-behind-file"></a>Behandlung von Schieberegler in der CodeBehind-Datei 

Die **RGB-Farbe Schieberegler** Seite instanziiert eine `BoxView` eine Farbe anzuzeigenden drei `Slider` Instanzen auswählen der Rot-, Grün- und blauen-Komponenten, die Farbe und drei `Label` Elemente für diese Farbe anzeigen Werte:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SliderDemos.RgbColorSlidersPage"
             Title="RGB Color Sliders">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Slider">
                <Setter Property="Maximum" Value="255" />
            </Style>
            
            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Margin="10">
        <BoxView x:Name="boxView"
                 Color="Black"
                 VerticalOptions="FillAndExpand" />

        <Slider x:Name="redSlider"
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="redLabel" />

        <Slider x:Name="greenSlider" 
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="greenLabel" />

        <Slider x:Name="blueSlider" 
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="blueLabel" />
    </StackLayout>
</ContentPage>
```

Ein `Style` bietet alle drei `Slider` Elemente einen Bereich von 0 bis 255. Die `Slider` Elemente verwenden dieselbe `ValueChanged` Handler auf, die in der Code-Behind-Datei implementiert wird:

```csharp
public partial class RgbColorSlidersPage : ContentPage
{
    public RgbColorSlidersPage()
    {
        InitializeComponent();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        if (sender == redSlider)
        {
            redLabel.Text = String.Format("Red = {0:X2}", (int)args.NewValue);
        }
        else if (sender == greenSlider)
        {
            greenLabel.Text = String.Format("Green = {0:X2}", (int)args.NewValue);
        }
        else if (sender == blueSlider)
        {
            blueLabel.Text = String.Format("Blue = {0:X2}", (int)args.NewValue);
        }

        boxView.Color = Color.FromRgb((int)redSlider.Value,
                                      (int)greenSlider.Value,
                                      (int)blueSlider.Value);
    }
}
```

Im ersten Abschnitt wird die `Text` Eigenschaft eines der `Label` -Instanzen, die eine kurze Textzeichenfolge, die den Wert, der angibt, die `Slider` im Hexadezimalformat. Klicken Sie dann alle drei `Slider` -Instanzen erfolgt zum Erstellen einer `Color` Wert aus den RGB-Komponenten:

[![RGB-Farbe Schieberegler](slider-images/RgbColorSliders.png "RGB-Farbe Schieberegler")](slider-images/RgbColorSliders-Large.png#lightbox)

### <a name="binding-the-slider-to-a-viewmodel"></a>Binden den Schieberegler an einem ViewModel

Die **HSL-Farbe Schieberegler** Seite zeigt, wie ein ViewModel für die Berechnungen zum Erstellen einer `Color` Wert aus der Werte für Farbton, Sättigung und Helligkeit. Wie alle ViewModels die `HSLColorViewModel` -Klasse implementiert die `INotifyPropertyChanged` -Schnittstelle und löst eine `PropertyChanged` Ereignis aus, wenn eine der Eigenschaften geändert wird:

```csharp
public class HslColorViewModel : INotifyPropertyChanged
{
    Color color;

    public event PropertyChangedEventHandler PropertyChanged;

    public double Hue
    {
        set
        {
            if (color.Hue != value)
            {
                Color = Color.FromHsla(value, color.Saturation, color.Luminosity);
            }
        }
        get 
        {
            return color.Hue;
        }
    }

    public double Saturation
    {
        set
        {
            if (color.Saturation != value)
            {
                Color = Color.FromHsla(color.Hue, value, color.Luminosity);
            }
        }
        get
        {
            return color.Saturation;
        }
    }

    public double Luminosity
    {
        set
        {
            if (color.Luminosity != value)
            {
                Color = Color.FromHsla(color.Hue, color.Saturation, value);
            }
        }
        get
        {
            return color.Luminosity;
        }
    }

    public Color Color
    {
        set
        {
            if (color != value)
            {
                color = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Hue"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Saturation"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Luminosity"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Color"));
            }
        }
        get
        {
            return color;
        }
    }
}
```

ViewModels und `INotifyPropertyChanged` Schnittstelle werden in diesem Artikel erläutert [Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md).

Die **HslColorSlidersPage.xaml** Datei instanziiert den `HslColorViewModel` und legt es auf der Seite `BindingContext` Eigenschaft. Dadurch können alle Elemente in der XAML-Datei zum Binden an Eigenschaften in das ViewModel:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:SliderDemos"
             x:Class="SliderDemos.HslColorSlidersPage"
             Title="HSL Color Sliders">

    <ContentPage.BindingContext>
        <local:HslColorViewModel Color="Chocolate" />
    </ContentPage.BindingContext>

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Margin="10">
        <BoxView Color="{Binding Color}"
                 VerticalOptions="FillAndExpand" />

        <Slider Value="{Binding Hue}" />
        <Label Text="{Binding Hue, StringFormat='Hue = {0:F2}'}" />

        <Slider Value="{Binding Saturation}" />
        <Label Text="{Binding Saturation, StringFormat='Saturation = {0:F2}'}" />

        <Slider Value="{Binding Luminosity}" />
        <Label Text="{Binding Luminosity, StringFormat='Luminosity = {0:F2}'}" />
    </StackLayout>
</ContentPage> 
```

Als die `Slider` Elemente bearbeitet werden, die `BoxView` und `Label` Elemente aus der ViewModel aktualisiert werden:

[![HSL-Farbe Schieberegler](slider-images/HslColorSliders.png "HSL-Farbe-Schieberegler")](slider-images/HslColorSliders-Large.png#lightbox)

Die `StringFormat` -Komponente von der `Binding` Markuperweiterung für Format "F2" festgelegt ist, zwei Dezimalstellen angezeigt. (Zeichenfolgenformatierung datenbindungen wird erläutert, in dem Artikel [Zeichenfolgenformatierung](~/xamarin-forms/app-fundamentals/data-binding/string-formatting.md).) Allerdings ist die uwp-Version des Programms auf den Werten 0, 0,1, 0,2 beschränkt... 0.9 und 1.0. Dies ist das direkte Ergebnis der Implementierung des UWP `Slider` wie im Abschnitt oben beschrieben [Unterschieden bei der Implementierung Plattform](#implementations).

## <a name="related-links"></a>Verwandte Links

- [Schieberegler Demos-Beispiel](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos)
- [Schieberegler-API](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/)
