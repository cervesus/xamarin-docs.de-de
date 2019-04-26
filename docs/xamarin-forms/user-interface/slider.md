---
title: Xamarin.Forms-Schieberegler
description: Der Schieberegler Xamarin.Forms ist eine horizontale Leiste, die vom Benutzer auf einen double-Wert wählen Sie einen durchgehenden Bereich bearbeitet werden kann. In diesem Artikel wird erläutert, wie Sie die Schieberegler-Klasse verwenden, wählen Sie einen Wert aus einem Bereich fortlaufender Werte.
ms.prod: xamarin
ms.assetid: 36B1C645-26E0-4874-B6B6-BDBF77662878
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/27/2019
ms.openlocfilehash: fa339d9fd404cf74aa603d853abde5f9128e57b5
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61250791"
---
# <a name="xamarinforms-slider"></a>Xamarin.Forms-Schieberegler

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos)

_Verwenden Sie einen Schieberegler für die Auswahl aus einem Bereich fortlaufender Werte._

Die Xamarin.Forms [ `Slider` ](xref:Xamarin.Forms.Slider) ist eine horizontale Leiste, die vom Benutzer auszuwählenden bearbeitet werden kann eine `double` Wert aus einen durchgehenden Bereich.

Die `Slider` definiert drei Eigenschaften des Typs `double`:

- [`Minimum`](xref:Xamarin.Forms.Slider.Minimum) ist der Mindestwert des Bereichs, hat den Standardwert 0.
- [`Maximum`](xref:Xamarin.Forms.Slider.Maximum) ist der Höchstwert des Bereichs, hat den Standardwert 1.
- [`Value`](xref:Xamarin.Forms.Slider.Value) ist der Wert des Schiebereglers, die zwischen liegen kann `Minimum` und `Maximum` und hat den Standardwert 0.

Alle drei Eigenschaften verfügen über `BindableProperty` Objekte. Die `Value` Eigenschaft verfügt über einen Standardmodus für die Bindung der `BindingMode.TwoWay`, was bedeutet, dass es als Bindungsquelle in einer Anwendung geeignet ist, verwendet der [Model-View-ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) Architektur.

> [!WARNING]
> Intern wird die `Slider` wird sichergestellt, dass `Minimum` ist kleiner als `Maximum`. Wenn `Minimum` oder `Maximum` einmal festgelegt werden, damit `Minimum` ist nicht kleiner als `Maximum`, wird eine Ausnahme ausgelöst. Finden Sie unter den [ **Vorsichtsmaßnahmen** ](#precautions) weiter unten im Abschnitt, um mehr über die Einstellung der `Minimum` und `Maximum` Eigenschaften.

Die `Slider` wandelt die `Value` Eigenschaft, damit es zwischen ist `Minimum` und `Maximum`(inklusiv) enthalten. Wenn die `Minimum` -Eigenschaftensatz auf einen Wert größer als die `Value` -Eigenschaft, die `Slider` legt diese fest der `Value` Eigenschaft `Minimum`. Auf ähnliche Weise Wenn `Maximum` auf einen Wert festgelegt ist kleiner als `Value`, klicken Sie dann `Slider` legt die `Value` Eigenschaft `Maximum`.

`Slider` definiert eine [ `ValueChanged` ](xref:Xamarin.Forms.Slider.ValueChanged) Ereignis, wenn ausgelöst wird, die `Value` ändert, entweder durch Bearbeitung der durch den Benutzer der `Slider` oder wenn die Anwendung setzt die `Value` -Eigenschaft direkt. Ein `ValueChanged` Ereignis wird auch ausgelöst, wenn die `Value` Eigenschaft wird erzwungen, wie im vorherigen Absatz beschrieben.

Die [ `ValueChangedEventArgs` ](xref:Xamarin.Forms.ValueChangedEventArgs) Objekt an, die mit der `ValueChanged` Ereignis verfügt über zwei Eigenschaften, die beide vom Typ `double`: [ `OldValue` ](xref:Xamarin.Forms.ValueChangedEventArgs.OldValue) und [ `NewValue` ](xref:Xamarin.Forms.ValueChangedEventArgs.NewValue). Zum Zeitpunkt das Ereignis ausgelöst wird, den Wert der `NewValue` ist identisch mit der `Value` Eigenschaft der `Slider` Objekt.

`Slider` definiert außerdem `DragStarted` und `DragCompleted` Ereignisse, die am Anfang und Ende der Drag-Aktion ausgelöst werden. Im Gegensatz zu den [ `ValueChanged` ](xref:Xamarin.Forms.Slider.ValueChanged) -Ereignis, das `DragStarted` und `DragCompleted` Ereignisse werden nur ausgelöst, durch die Benutzer die Bearbeitung von der `Slider`. Wenn die `DragStarted` Ereignis wird ausgelöst, die `DragStartedCommand`, des Typs `ICommand`, ausgeführt wird. Auf ähnliche Weise, wenn die `DragCompleted` Ereignis wird ausgelöst, die `DragCompletedCommand`, des Typs `ICommand`, ausgeführt wird.

> [!WARNING]
> Verwenden Sie keine Optionen uneingeschränkte horizontales Layout `Center`, `Start`, oder `End` mit `Slider`. Für Android und UWP die `Slider` reduziert, die in ein Balkendiagramm der Länge Null, und klicken Sie unter iOS, die Leiste ist sehr kurz. Behalten Sie den Standardwert `HorizontalOptions` -Einstellung `Fill`, und verwenden Sie keine Breite von `Auto` beim `Slider` in eine `Grid` Layout.

Die `Slider` definiert außerdem mehrere Eigenschaften, die die Darstellung auswirken:

- [`MinimumTrackColor`](xref:Xamarin.Forms.Slider.MinimumTrackColorProperty) ist die Leiste Farbe auf der linken Seite des Ziehpunkts.
- [`MaximumTrackColor`](xref:Xamarin.Forms.Slider.MaximumTrackColorProperty) ist die Leiste Farbe auf der rechten Seite des Ziehpunkts.
- [`ThumbColor`](xref:Xamarin.Forms.Slider.ThumbColorProperty) ist die Farbe des Thumb-Steuerelement.
- [`ThumbImage`](xref:Xamarin.Forms.Slider.ThumbImageProperty) ist das Image für das Thumb-Steuerelement des Typs [ `FileImageSource` ](xref:Xamarin.Forms.FileImageSource).

> [!NOTE]
> Die `ThumbColor` und `ThumbImage` Eigenschaften schließen sich gegenseitig. Wenn beide Eigenschaften festgelegt werden, die `ThumbImage` -Eigenschaft Vorrang.

## <a name="basic-slider-code-and-markup"></a>Grundlegende Schieberegler-Code und markup

Die [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) Beispiel beginnt mit drei Seiten, die funktionell identisch sind, aber auf unterschiedliche Weise implementiert werden. Die erste Seite wird nur C#-Code verwendet. der zweite verwendet XAML mit einem Ereignishandler im Code und das dritte hingegen ist, können Sie den Ereignishandler zu vermeiden, indem Sie mithilfe der Datenbindung in der XAML-Datei.

### <a name="creating-a-slider-in-code"></a>Erstellen einen Schieberegler in code

Die **grundlegenden Schieberegler Code** auf der Seite die [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) Beispiel wird gezeigt, anzeigen, erstellen Sie eine `Slider` und zwei `Label` -Objekte im Code:

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

Die `Slider` initialisiert wird, dass eine `Maximum` Eigenschaft 360. Der `ValueChanged` Handler der `Slider` verwendet der `Value` Eigenschaft der `slider` Objekt zum Festlegen von der `Rotation` Eigenschaft des ersten `Label` und verwendet die `String.Format` -Methode mit der `NewValue` Eigenschaft der Ereignisargumente, um das Festlegen der `Text` -Eigenschaft der zweiten `Label`. Diese beiden Ansätze zum Abrufen des aktuellen Werts von der `Slider` sind austauschbar.

Hier wird das Programm Geräte unter iOS, Android und universelle Windows-Plattform (UWP) ausgeführt wird:

[![Grundlegende Schieberegler Code](slider-images/BasicSliderCode.png "grundlegende Schieberegler-Code")](slider-images/BasicSliderCode-Large.png#lightbox)

Die zweite `Label` zeigt den Text "(nicht initialisierte)", bis die `Slider` bearbeitet wird, der bewirkt, dass der erstes `ValueChanged` Ereignis ausgelöst werden. Beachten Sie, dass die Anzahl der Dezimalstellen, die angezeigt werden, je nach Plattform unterschiedlich ist. Diese Unterschiede beziehen sich auf die plattformimplementierungen aus, der die `Slider` und werden weiter unten in diesem Artikel im Abschnitt [Implementierung Plattformunterschiede](#implementations).

### <a name="creating-a-slider-in-xaml"></a>Erstellen einen Schieberegler in XAML

Die **grundlegende Schieberegler XAML** Seite ist funktionell identisch mit **grundlegenden Schieberegler Code** jedoch meist in XAML implementiert:

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

Die Code-Behind-Datei enthält den Handler für die `ValueChanged` Ereignis:

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

Wenn die `Slider` Objekt die erhaltenen eines Namens in der XAML-Datei mit einer `x:Name` -Attribut (z. B. "Slider"), und klicken Sie dann auf den Ereignishandler kann dieses Objekt direkt verweisen:

```csharp
double value = slider.Value;
```

### <a name="data-binding-the-slider"></a>Den Schieberegler für die Datenbindung

Die **Standardbindungen Schieberegler** Seite zeigt, wie ein nahezu gleichwertig Programm schreiben, bei denen keine das `Value` -Ereignishandler mit [Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md):

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

Die `Rotation` Eigenschaft des ersten `Label` gebunden ist die `Value` Eigenschaft der `Slider`, da die `Text` -Eigenschaft der zweiten `Label` mit einer `StringFormat` Spezifikation. Die **Standardbindungen Schieberegler** Seite Funktionen etwas anders aus den vorherigen zwei Seiten: Wenn die Seite zuerst angezeigt wird, das zweite `Label` zeigt die Zeichenfolge mit dem Wert an. Dies ist ein Vorteil der Verwendung der Datenbindung. Um Text ohne Datenbindung anzuzeigen, müssen Sie explizit Initialisieren der `Text` Eigenschaft der `Label` oder simulieren Sie eine Auslösung des der `ValueChanged` Ereignis durch Aufrufen des ereignishandlers aus dem Klassenkonstruktor.

<a name="precautions" />

## <a name="precautions"></a>Vorsichtsmaßnahmen bei der

Der Wert des der `Minimum` Eigenschaft muss immer kleiner als der Wert, der die `Maximum` Eigenschaft. Im folgenden Codebeispiel Codeausschnitt bewirkt, dass die `Slider` zum Auslösen einer Ausnahme:

```csharp
// Throws an exception!
Slider slider = new Slider
{
    Minimum = 10,
    Maximum = 20
};
```

Der C#-Compiler generiert Code, der diese beiden Eigenschaften in der Sequenz ist, legt diese fest und wann die `Minimum` Eigenschaft ist auf 10 festgelegt, es ist größer als der Standardwert `Maximum` Wert 1. Sie können die Ausnahme in diesem Fall vermeiden, durch Festlegen der `Maximum` Eigenschaft erste:

```csharp
Slider slider = new Slider
{
    Maximum = 20,
    Minimum = 10
};
```

Festlegen von `Maximum` auf 20 ist kein Problem, da sie größer als der Standardwert ist `Minimum` Wert 0. Wenn `Minimum` festgelegt ist, wird der Wert ist kleiner als der `Maximum` Wert von 20.

Das gleiche Problem tritt in XAML. Legen Sie die Eigenschaften in der Reihenfolge, die wird, dass sichergestellt `Maximum` ist immer größer als `Minimum`:

```xaml
<Slider Maximum="20"
        Minimum="10" ... />
```

Sie können festlegen, die `Minimum` und `Maximum` Werte in negative Zahlen, aber nur in der Reihenfolge, in denen `Minimum` ist immer kleiner als `Maximum`:

```xaml
<Slider Minimum="-20"
        Maximum="-10" ... />
```

Die `Value` Eigenschaft ist immer größer als oder gleich der `Minimum` Wert und kleiner als oder gleich `Maximum`. Wenn `Value` festgelegt ist auf einen Wert außerhalb dieses Bereichs liegt, wird der Wert im Bereich liegen zwingend werden, jedoch wird keine Ausnahme ausgelöst. Dieser Code beispielsweise wird *nicht* eine Ausnahme ausgelöst:

```csharp
Slider slider = new Slider
{
    Value = 10
};
```

Stattdessen die `Value` Eigenschaft wird erzwungen, um die `Maximum` Wert 1.

Hier ist eine der oben aufgeführten Codeausschnitt aus:

```csharp
Slider slider = new Slider
{
    Maximum = 20,
    Minimum = 10
};
```

Wenn `Minimum` ist auf 10 festgelegt, dann `Value` wird ebenfalls auf 10 festgelegt.

Wenn eine `ValueChanged` Ereignishandler wurde angefügt, die zum Zeitpunkt, der `Value` Eigenschaft wird in einen anderen Wert als den Standardwert 0 (null) umgewandelt wird eine `ValueChanged` Ereignis wird ausgelöst. Hier ist ein Ausschnitt des XAML aus:

```xaml
<Slider ValueChanged="OnSliderValueChanged"
        Maximum="20"
        Minimum="10" />
```

Wenn `Minimum` ist auf 10 festgelegt, `Value` wird ebenfalls auf 10 festgelegt und die `ValueChanged` Ereignis wird ausgelöst. Dies kann auftreten, bevor der Rest der Seite erstellt wurden wurde und der Handler könnte versuchen, auf andere Elemente auf der Seite verweisen, die noch nicht erstellt wurden. Möglicherweise möchten Sie Code zum Hinzufügen der `ValueChanged` Handler, der prüft, ob `null` Werte der anderen Elemente auf der Seite. Oder Sie können festlegen, die `ValueChanged` Ereignishandler nach der `Slider` Werte initialisiert wurden.

<a name="implementations" />

## <a name="platform-implementation-differences"></a>Implementierung von Plattformunterschiede

Die oben gezeigten Screenshots zeigen den Wert von der `Slider` mit einer unterschiedlichen Anzahl von Dezimalstellen. Dies bezieht sich auf wie die `Slider` wird auf die Plattformen Android und UWP implementiert.

### <a name="the-android-implementation"></a>Die Android-Implementierung

Die Android-Implementierung von `Slider` basiert auf dem Android [ `SeekBar` ](https://developer.xamarin.com/api/type/Android.Widget.SeekBar/) und stellt stets die [ `Max` ](https://developer.xamarin.com/api/property/Android.Widget.ProgressBar.Max/) -Eigenschaft auf 1000. Dies bedeutet, dass die `Slider` unter Android hat nur 1.001 diskrete Werte. Setzen Sie die `Slider` haben eine `Minimum` 0 und einem `Maximum` 5000 aus, und klicken Sie dann als der `Slider` bearbeitet wird, die `Value` Eigenschaft enthält die Werte von 0, 5, 10, 15 und So weiter.

### <a name="the-uwp-implementation"></a>Die UWP-Implementierung

Die UWP-Implementierung von `Slider` basiert auf der UWP [ `Slider` ](/uwp/api/windows.ui.xaml.controls.slider) Steuerelement. Die `StepFrequency` Eigenschaft der UWP `Slider` festgelegt ist, um den Unterschied zwischen der `Maximum` und `Minimum` Eigenschaften unterteilt, 10, aber nicht größer als 1.

Für die Standard-Bereich von 0 bis 1, beispielsweise die `StepFrequency` -Eigenschaftensatz auf 0,1. Als die `Slider` bearbeitet wird, die `Value` -Eigenschaft auf 0, 0.1, 0.2, 0.3, 0,4, 0,5, 0,6, 0,7, 0,8, 0,9 und 1.0 beschränkt ist. (Dies wird in der letzten Seite in der [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) Beispiel.) Wenn der Unterschied zwischen der `Maximum` und `Minimum` Eigenschaften ist 10 oder höher ist, klicken Sie dann `StepFrequency` ist auf 1 festgelegt, und die `Value` Eigenschaft verfügt über ganzzahlige Werte.

### <a name="the-stepslider-solution"></a>Die Lösung StepSlider

Ein flexibler `StepSlider` ausführlicher [Kapitel 27. Benutzerdefinierte Renderer](https://xamarin.azureedge.net/developer/xamarin-forms-book/XamarinFormsBook-Ch27-Apr2016.pdf) des Buchs *Erstellen mobiler Apps mit Xamarin.Forms*. Die `StepSlider` ähnelt `Slider` fügt jedoch ein `Steps` -Eigenschaft an die Anzahl von Werten zwischen `Minimum` und `Maximum`.

## <a name="sliders-for-color-selection"></a>Schieberegler für die Farbauswahl

Die letzten zwei Seiten in der [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) Beispiel verwenden drei `Slider` Instanzen für die Farbauswahl. Die erste Seite behandelt alle Aktivitäten in der CodeBehind-Datei während die zweite Seite wie die Datenbindung mit einem ViewModel zu verwendet wird.

### <a name="handling-sliders-in-the-code-behind-file"></a>Behandeln der Schieberegler in der CodeBehind-Datei

Die **RGB-Farbe Schieberegler** Seite instanziiert ein `BoxView` anzuzeigende eine Farbe an, drei `Slider` Instanzen wählen Sie die Komponenten roten, grünen und blauen, der die Farbe und drei `Label` Elemente für diese Farbe anzeigen Werte:

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

Ein `Style` bietet alle drei `Slider` Elemente einen Bereich von 0 bis 255. Die `Slider` Elemente verwenden dieselbe `ValueChanged` Handler, der in der CodeBehind-Datei implementiert wird:

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

Im ersten Abschnitt wird die `Text` Eigenschaft eines der `Label` Instanzen, um eine kurze Textzeichenfolge, die den Wert, der angibt, die `Slider` im Hexadezimalformat. Klicken Sie dann alle drei `Slider` Instanzen erfolgt zum Erstellen einer `Color` Wert aus den RGB-Komponenten:

[![RGB-Farbe Schieberegler](slider-images/RgbColorSliders.png "RGB-Farbe Schieberegler")](slider-images/RgbColorSliders-Large.png#lightbox)

### <a name="binding-the-slider-to-a-viewmodel"></a>Binden den Schieberegler an ein "ViewModel"

Die **HSL-Farbe Schieberegler** Seite zeigt, wie ein "ViewModel" verwenden, führen Sie die Berechnungen, die zum Erstellen einer `Color` Wert aus der Werte für Farbton, Sättigung und Helligkeit. Wie alle ViewModels, die `HSLColorViewModel` -Klasse implementiert die `INotifyPropertyChanged` -Schnittstelle, und löst eine `PropertyChanged` Ereignis, wenn eine der Eigenschaften geändert:

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

ViewModels und `INotifyPropertyChanged` Schnittstelle werden in diesem Artikel erläuterten [Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md).

Die **HslColorSlidersPage.xaml** Datei instanziiert den `HslColorViewModel` und legt es auf der Seite `BindingContext` Eigenschaft. Dadurch können alle Elemente in der XAML-Datei zum Binden an Eigenschaften in "ViewModel":

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

Als die `Slider` Elemente bearbeitet werden, die `BoxView` und `Label` Elemente aus dem ViewModel aktualisiert werden:

[![HSL-Farbe Schieberegler](slider-images/HslColorSliders.png "HSL-Farbe Schieberegler")](slider-images/HslColorSliders-Large.png#lightbox)

Die `StringFormat` -Komponente von der `Binding` Markuperweiterung für ein Format von "F2" festgelegt ist, zwei Dezimalstellen angezeigt. (Datenbindungen Formatieren von Zeichenfolgen ist in diesem Artikel erläuterten [Zeichenfolgenformatierung](~/xamarin-forms/app-fundamentals/data-binding/string-formatting.md).) Allerdings ist die UWP-Version des Programms auf Werte von 0, 0.1, 0.2... 0,9 und 1.0. Dies ist das direkte Ergebnis der Implementierung der UWP `Slider` wie im Abschnitt oben beschrieben [Implementierung Plattformunterschiede](#implementations).

## <a name="related-links"></a>Verwandte Links

- [Beispiel für Schieberegler-Demos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos)
- [Schieberegler-API](xref:Xamarin.Forms.Slider)
