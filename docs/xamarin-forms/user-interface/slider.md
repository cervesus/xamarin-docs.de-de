---
title: Xamarin.FormsKeits
description: Der Xamarin.Forms Schieberegler ist ein horizontaler Balken, der vom Benutzer bearbeitet werden kann, um einen doppelten Wert aus einem kontinuierlichen Bereich auszuwählen. In diesem Artikel wird erläutert, wie Sie mit der Slider-Klasse einen Wert aus einem Bereich von kontinuierlichen Werten auswählen.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 1cde999e6781f019b6abceee82caf259e1e5a710
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84140152"
---
# <a name="xamarinforms-slider"></a>Xamarin.FormsKeits

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos)

_Verwenden Sie einen Schieberegler, um aus einem Bereich von kontinuierlichen Werten auszuwählen._

Der Xamarin.Forms [`Slider`](xref:Xamarin.Forms.Slider) ist ein horizontaler Balken, der vom Benutzer bearbeitet werden kann, um einen `double` Wert aus einem kontinuierlichen Bereich auszuwählen.

`Slider`Definiert drei Eigenschaften vom Typ `double` :

- [`Minimum`](xref:Xamarin.Forms.Slider.Minimum)der Mindestwert des Bereichs, der Standardwert ist 0 (null).
- [`Maximum`](xref:Xamarin.Forms.Slider.Maximum)der Höchstwert des Bereichs, der Standardwert ist 1.
- [`Value`](xref:Xamarin.Forms.Slider.Value)der Wert des Schiebereglers, der zwischen und liegen kann `Minimum` `Maximum` . der Standardwert ist 0.

Alle drei Eigenschaften werden von- `BindableProperty` Objekten unterstützt. Die- `Value` Eigenschaft verfügt über einen Standard Bindungs Modus von `BindingMode.TwoWay` . Dies bedeutet, dass Sie als Bindungs Quelle in einer Anwendung geeignet ist, die die [Model-View-ViewModel (MVVM)-](~/xamarin-forms/enterprise-application-patterns/mvvm.md) Architektur verwendet.

> [!WARNING]
> Intern `Slider` stellt sicher, dass `Minimum` kleiner als ist `Maximum` . Wenn `Minimum` oder `Maximum` jemals festgelegt sind, sodass `Minimum` nicht kleiner als ist `Maximum` , wird eine Ausnahme ausgelöst. Weitere Informationen zum Festlegen der-Eigenschaft und der-Eigenschaft finden Sie im Abschnitt " [**Vorsichtsmaßnahmen**](#precautions) " `Minimum` `Maximum` .

Der wandelt `Slider` die `Value` -Eigenschaft so um, dass Sie zwischen und (einschließlich) liegt `Minimum` `Maximum` . Wenn die `Minimum` Eigenschaft auf einen Wert festgelegt ist, der größer als die- `Value` Eigenschaft ist, wird `Slider` die- `Value` Eigenschaft von auf festgelegt `Minimum` . Wenn `Maximum` auf einen Wert kleiner als festgelegt wird `Value` , dann wird `Slider` die- `Value` Eigenschaft auf festgelegt `Maximum` .

`Slider`definiert ein- [`ValueChanged`](xref:Xamarin.Forms.Slider.ValueChanged) Ereignis, das ausgelöst wird `Value` , wenn die geändert werden, entweder über die Benutzer Bearbeitung von `Slider` oder, wenn das Programm die- `Value` Eigenschaft direkt festlegt. Ein- `ValueChanged` Ereignis wird auch ausgelöst, wenn die- `Value` Eigenschaft wie im vorherigen Absatz beschrieben erzwungen wird.

Das [`ValueChangedEventArgs`](xref:Xamarin.Forms.ValueChangedEventArgs) Objekt, das das `ValueChanged` Ereignis begleitet, verfügt über zwei Eigenschaften vom Typ `double` : [`OldValue`](xref:Xamarin.Forms.ValueChangedEventArgs.OldValue) und [`NewValue`](xref:Xamarin.Forms.ValueChangedEventArgs.NewValue) . Wenn das-Ereignis ausgelöst wird, ist der Wert von mit `NewValue` der- `Value` Eigenschaft des-Objekts identisch `Slider` .

`Slider`definiert auch `DragStarted` -und- `DragCompleted` Ereignisse, die am Anfang und Ende der Zieh Aktion ausgelöst werden. Im Gegensatz zum [`ValueChanged`](xref:Xamarin.Forms.Slider.ValueChanged) -Ereignis `DragStarted` werden das-Ereignis und das-Ereignis `DragCompleted` nur durch die Benutzer Bearbeitung von ausgelöst `Slider` . Wenn das-Ereignis ausgelöst wird `DragStarted` , `DragStartedCommand` wird der vom Typ `ICommand` ausgeführt. Ebenso wird, wenn das Ereignis ausgelöst wird `DragCompleted` , der `DragCompletedCommand` vom Typ `ICommand` ausgeführt.

> [!WARNING]
> Verwenden Sie keine eingeschränkten horizontalen Layoutoptionen von `Center` , `Start` oder `End` mit `Slider` . Bei Android und UWP reduziert sich der `Slider` auf einen Balken der Länge 0 (null), und bei IOS ist die Leiste sehr kurz. Behalten Sie die Standard `HorizontalOptions` Einstellung `Fill` bei, und verwenden Sie `Auto` beim Einfügen `Slider` eines Layouts keine Breite von `Grid` .

Der `Slider` definiert auch mehrere Eigenschaften, die sich auf seine Darstellung auswirken:

- [`MinimumTrackColor`](xref:Xamarin.Forms.Slider.MinimumTrackColorProperty)die Balken Farbe auf der linken Seite des Zieh Punkts.
- [`MaximumTrackColor`](xref:Xamarin.Forms.Slider.MaximumTrackColorProperty)die Balken Farbe auf der rechten Seite des Zieh Punkts.
- [`ThumbColor`](xref:Xamarin.Forms.Slider.ThumbColorProperty)die Zieh Punktfarbe.
- [`ThumbImageSource`](xref:Xamarin.Forms.Slider.ThumbImageSourceProperty)das Bild, das für den Ziehpunkt des Typs verwendet werden soll [`ImageSource`](xref:Xamarin.Forms.ImageSource) .

> [!NOTE]
> Die `ThumbColor` `ThumbImageSource` Eigenschaften und schließen sich gegenseitig aus. Wenn beide Eigenschaften festgelegt sind, hat die `ThumbImageSource` Eigenschaft Vorrang.

## <a name="basic-slider-code-and-markup"></a>Grundlegender Schieberegler-Code und Markup

Das [**sliderdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos) -Beispiel beginnt mit drei Seiten, die funktionell identisch sind, jedoch auf unterschiedliche Weise implementiert werden. Die erste Seite verwendet nur c#-Code, der zweite verwendet XAML mit einem Ereignishandler im Code, und der dritte kann den Ereignishandler mithilfe der Datenbindung in der XAML-Datei vermeiden.

### <a name="creating-a-slider-in-code"></a>Erstellen eines Schiebereglers im Code

Die **Codepage für den grundlegenden Schieberegler** im [**sliderdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos) -Beispiel zeigt, wie ein `Slider` -Objekt und zwei- `Label` Objekte im Code erstellt werden:

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

Die `Slider` wird mit der- `Maximum` Eigenschaft 360 initialisiert. Der- `ValueChanged` Handler des `Slider` `Value` -Objekts verwendet die-Eigenschaft des `slider` -Objekts, um die `Rotation` -Eigenschaft des ersten festzulegen, `Label` und verwendet die- `String.Format` Methode mit der- `NewValue` Eigenschaft der Ereignis Argumente, um die- `Text` Eigenschaft der zweiten festzulegen `Label` . Diese beiden Ansätze zum Abrufen des aktuellen Werts von `Slider` sind austauschbar.

Hier ist das Programm, das auf IOS-und Android-Geräten ausgeführt wird:

[![Standard Schieberegler-Code](slider-images/BasicSliderCode.png "Standard Schieberegler-Code")](slider-images/BasicSliderCode-Large.png#lightbox)

Die zweite `Label` zeigt den Text "(nicht initialisiert)" `Slider` an, bis der manipuliert wird, was bewirkt, dass das erste `ValueChanged` Ereignis ausgelöst wird. Beachten Sie, dass die Anzahl der angezeigten Dezimalstellen für jede Plattform unterschiedlich ist. Diese Unterschiede beziehen sich auf die Platt Form Implementierungen von `Slider` und werden weiter unten in diesem Artikel im Abschnitt [Unterschiede bei der Platt Form Implementierung](#implementations)erläutert.

### <a name="creating-a-slider-in-xaml"></a>Erstellen eines Schiebereglers in XAML

Die **grundlegende Schieberegler-XAML** -Seite ist funktional identisch mit dem **grundlegenden Schieberegler-Code** , der jedoch größtenteils in XAML implementiert ist

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

Die Code-Behind-Datei enthält den Handler für das- `ValueChanged` Ereignis:

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

Der Ereignishandler kann auch den abrufen `Slider` , der das Ereignis durch das- `sender` Argument auslöst. Die- `Value` Eigenschaft enthält den aktuellen Wert:

```csharp
double value = ((Slider)sender).Value;
```

Wenn dem `Slider` Objekt ein Name in der XAML-Datei mit einem- `x:Name` Attribut (z. b. "Schieberegler") zugewiesen wurde, kann der Ereignishandler direkt auf dieses Objekt verweisen:

```csharp
double value = slider.Value;
```

### <a name="data-binding-the-slider"></a>Datenbindung für den Schieberegler

Die Seite " **grundlegende Schieberegler-Bindungen** " zeigt, wie Sie ein nahezu gleichwertiges Programm schreiben, das den `Value` Ereignishandler mithilfe der [Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md)eliminiert:

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

Die- `Rotation` Eigenschaft der ersten `Label` ist an die- `Value` Eigenschaft von gebunden `Slider` , wie die- `Text` Eigenschaft der zweiten `Label` mit einer- `StringFormat` Spezifikation. Die Seite mit den **grundlegenden Schieberegler-Bindungen** funktioniert etwas anders als die beiden vorherigen Seiten: Wenn die Seite zum ersten Mal angezeigt wird, zeigt die zweite `Label` die Text Zeichenfolge mit dem Wert an. Dies hat den Vorteil, dass die Datenbindung verwendet wird. Um Text ohne Datenbindung anzuzeigen, müssen Sie die- `Text` Eigenschaft des-Objekts explizit initialisieren `Label` oder ein Auslösen des-Ereignisses simulieren, `ValueChanged` indem Sie den-Ereignishandler aus dem-Klassenkonstruktor aufrufen.

<a name="precautions" />

## <a name="precautions"></a>Treffen

Der Wert der- `Minimum` Eigenschaft muss stets kleiner sein als der Wert der- `Maximum` Eigenschaft. Der folgende Code Ausschnitt bewirkt, dass `Slider` eine Ausnahme auslöst:

```csharp
// Throws an exception!
Slider slider = new Slider
{
    Minimum = 10,
    Maximum = 20
};
```

Der c#-Compiler generiert Code, mit dem diese beiden Eigenschaften nacheinander festgelegt werden. wenn die- `Minimum` Eigenschaft auf 10 festgelegt ist, ist sie größer als der Standard `Maximum` Wert 1. In diesem Fall können Sie die Ausnahme vermeiden, indem Sie `Maximum` zuerst die-Eigenschaft festlegen:

```csharp
Slider slider = new Slider
{
    Maximum = 20,
    Minimum = 10
};
```

`Maximum`Die Einstellung auf 20 ist kein Problem, da Sie größer als der Standard `Minimum` Wert 0 ist. Wenn `Minimum` festgelegt ist, ist der Wert kleiner als der `Maximum` Wert von 20.

Das gleiche Problem ist in XAML aufgetreten. Legen Sie die Eigenschaften in einer Reihenfolge fest, die sicherstellt, dass `Maximum` immer größer ist als `Minimum` :

```xaml
<Slider Maximum="20"
        Minimum="10" ... />
```

Sie können den `Minimum` -Wert und den- `Maximum` Wert auf negative Zahlen festlegen, aber nur in einer Reihenfolge, in der `Minimum` immer kleiner ist als `Maximum` :

```xaml
<Slider Minimum="-20"
        Maximum="-10" ... />
```

Die `Value` -Eigenschaft ist immer größer oder gleich dem `Minimum` -Wert und kleiner als oder gleich `Maximum` . Wenn `Value` auf einen Wert außerhalb dieses Bereichs festgelegt wird, wird der Wert in den Bereich umgewandelt, aber es wird keine Ausnahme ausgelöst. Beispielsweise wird mit diesem Code *keine* Ausnahme ausgelöst:

```csharp
Slider slider = new Slider
{
    Value = 10
};
```

Stattdessen wird die- `Value` Eigenschaft in den `Maximum` Wert 1 umgewandelt.

Hier sehen Sie einen oben gezeigten Code Ausschnitt:

```csharp
Slider slider = new Slider
{
    Maximum = 20,
    Minimum = 10
};
```

Wenn `Minimum` auf 10 festgelegt ist, `Value` wird auch auf 10 festgelegt.

Wenn ein- `ValueChanged` Ereignishandler zu dem Zeitpunkt angefügt wurde, an dem die- `Value` Eigenschaft in einen anderen als den Standardwert 0 (null) umgewandelt wird, wird ein- `ValueChanged` Ereignis ausgelöst. Hier ist ein Code Ausschnitt von XAML:

```xaml
<Slider ValueChanged="OnSliderValueChanged"
        Maximum="20"
        Minimum="10" />
```

Wenn `Minimum` auf 10 festgelegt ist, `Value` wird auch auf 10 festgelegt, und das- `ValueChanged` Ereignis wird ausgelöst. Dies kann vorkommen, bevor der Rest der Seite erstellt wurde, und der Handler versucht möglicherweise, auf andere Elemente auf der Seite zu verweisen, die noch nicht erstellt wurden. Möglicherweise möchten Sie dem Handler Code hinzufügen `ValueChanged` , der `null` auf Werte anderer Elemente auf der Seite prüft. Oder Sie können den `ValueChanged` Ereignishandler festlegen, nachdem die `Slider` Werte initialisiert wurden.

<a name="implementations" />

## <a name="platform-implementation-differences"></a>Platt Form Implementierungs Unterschiede

In den zuvor gezeigten Screenshots wird der Wert von `Slider` mit einer unterschiedlichen Anzahl von Dezimalstellen angezeigt. Dies bezieht sich auf `Slider` die Implementierung von auf Android-und UWP-Plattformen.

### <a name="the-android-implementation"></a>Die Android-Implementierung

Die Android-Implementierung von `Slider` basiert auf Android [`SeekBar`](xref:Android.Widget.SeekBar) und legt die- [`Max`](xref:Android.Widget.ProgressBar.Max) Eigenschaft immer auf 1000 fest. Dies bedeutet, dass unter `Slider` Android nur 1.001 diskrete Werte aufweist. Wenn Sie das `Slider` -Objekt auf den `Minimum` Wert 0 und den `Maximum` Wert 5000 festgelegt haben, `Slider` hat die- `Value` Eigenschaft den Wert 0, 5, 10, 15 usw., wenn der manipuliert wird.

### <a name="the-uwp-implementation"></a>Die UWP-Implementierung

Die UWP-Implementierung von `Slider` basiert auf dem UWP- [`Slider`](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.slider) Steuerelement. Die `StepFrequency` -Eigenschaft der UWP `Slider` wird auf den Unterschied der `Maximum` Eigenschaften und `Minimum` dividiert durch 10, aber nicht größer als 1 festgelegt.

Für den Standardbereich von 0 bis 1 wird die-Eigenschaft beispielsweise `StepFrequency` auf 0,1 festgelegt. Da die- `Slider` Eigenschaft bearbeitet wird, ist die- `Value` Eigenschaft auf 0, 0,1, 0,2, 0,3, 0,4, 0,5, 0,6, 0,7, 0,8, 0,9 und 1,0 beschränkt. (Dies ist auf der letzten Seite des [**sliderdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos) -Beispiels ersichtlich.) Wenn der Unterschied zwischen der `Maximum` -Eigenschaft und der- `Minimum` Eigenschaft 10 oder größer ist, `StepFrequency` wird auf 1 festgelegt, und die-Eigenschaft weist ganzzahlige `Value` Werte auf.

### <a name="the-stepslider-solution"></a>Die stepslider-Lösung

Eine ausführlichere Beschreibung `StepSlider` wird in [Kapitel 27 erläutert. Benutzerdefinierte Renderer](https://xamarin.azureedge.net/developer/xamarin-forms-book/XamarinFormsBook-Ch27-Apr2016.pdf) des Buchs, * Xamarin.Forms mit dem Mobile Apps erstellt *wird. Der `StepSlider` ähnelt, `Slider` fügt jedoch eine `Steps` Eigenschaft hinzu, um die Anzahl der Werte zwischen `Minimum` und anzugeben `Maximum` .

## <a name="sliders-for-color-selection"></a>Schieberegler für Farbauswahl

Die letzten zwei Seiten im [**sliderdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos) -Beispiel verwenden jeweils drei `Slider` Instanzen für die Farbauswahl. Die erste Seite verarbeitet alle Interaktionen in der Code Behind-Datei, während die zweite Seite zeigt, wie die Datenbindung mit einem ViewModel verwendet wird.

### <a name="handling-sliders-in-the-code-behind-file"></a>Behandeln von Schiebereglern in der Code-Behind-Datei

Die **RGB-Farb Schieberegler** -Seite instanziiert ein `BoxView` -Element, um eine Farbe, drei `Slider` -Instanzen zum Auswählen der roten, grünen und blauen Komponenten der Farbe und drei `Label` Elemente zum Anzeigen dieser Farbwerte anzuzeigen:

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

Ein-Wert `Style` gibt alle drei `Slider` Elemente mit einem Bereich von 0 bis 255 an. Die `Slider` Elemente verwenden denselben `ValueChanged` Handler, der in der Code-Behind-Datei implementiert ist:

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

Im ersten Abschnitt wird die- `Text` Eigenschaft einer der- `Label` Instanzen auf eine kurze Text Zeichenfolge festgelegt, die den Wert von `Slider` in Hexadezimal angibt. Anschließend wird auf alle drei `Slider` Instanzen zugegriffen, um einen `Color` Wert aus den RGB-Komponenten zu erstellen:

[![RGB-Farb Schieberegler](slider-images/RgbColorSliders.png "RGB-Farb Schieberegler")](slider-images/RgbColorSliders-Large.png#lightbox)

### <a name="binding-the-slider-to-a-viewmodel"></a>Binden des Schiebereglers an ein ViewModel

Die Seite **HSL-Farb Schieberegler** zeigt, wie ein ViewModel verwendet wird, um die Berechnungen auszuführen, mit denen ein `Color` Wert aus Farbton-, Sättigungs-und Helligkeits Werten erstellt wird. Wie alle ViewModels implementiert die- `HSLColorViewModel` Klasse die `INotifyPropertyChanged` -Schnittstelle und löst ein- `PropertyChanged` Ereignis aus, wenn sich eine der Eigenschaften ändert:

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

ViewModels und die- `INotifyPropertyChanged` Schnittstelle werden im Artikel [Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md)erläutert.

Die Datei **hslcolorsliderspage. XAML** instanziiert `HslColorViewModel` und legt Sie auf die-Eigenschaft der Seite fest `BindingContext` . Dadurch können alle Elemente in der XAML-Datei an Eigenschaften im ViewModel gebunden werden:

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

Wenn die `Slider` Elemente manipuliert werden, `BoxView` werden das-Element und das- `Label` Element aus ViewModel aktualisiert:

[![HSL-Farb Schieberegler](slider-images/HslColorSliders.png "HSL-Farb Schieberegler")](slider-images/HslColorSliders-Large.png#lightbox)

Die `StringFormat` Komponente der `Binding` Markup Erweiterung wird für das Format "F2" festgelegt, um zwei Dezimalstellen anzuzeigen. (Die Zeichen folgen Formatierung in Daten Bindungen wird im Artikel [Zeichen folgen Formatierung](~/xamarin-forms/app-fundamentals/data-binding/string-formatting.md)erläutert.) Die UWP-Version des Programms ist jedoch auf die Werte 0, 0,1, 0,2,... 0,9 und 1,0. Dies ist ein direktes Ergebnis der Implementierung der UWP, `Slider` wie oben im Abschnitt [Unterschiede bei der Platt Form Implementierung](#implementations)beschrieben.

## <a name="related-links"></a>Verwandte Links

- [Beispiel für Slider-Demos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos)
- [Schieberegler-API](xref:Xamarin.Forms.Slider)
