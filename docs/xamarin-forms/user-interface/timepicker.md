---
Title: " Xamarin.Forms timePicker" Description: "timePicker ist eine Xamarin.Forms Ansicht, die es dem Benutzer ermöglicht, eine Uhrzeit auszuwählen. In diesem Artikel wird erläutert, wie eine timePicker in einer-Anwendung verwendet wird Xamarin.Forms . "
ms. Prod: xamarin ms. assetid: 2e99f B23-b82d-4eb4-afb3-5002e736e7b2 ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 10/16/2018 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-timepicker"></a>Xamarin.FormsTimePicker

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-timepicker)

_Eine Xamarin.Forms Ansicht, die es dem Benutzer ermöglicht, eine Uhrzeit auszuwählen._

Xamarin.Forms [`TimePicker`](xref:Xamarin.Forms.TimePicker) Ruft das Steuerelement für die Zeitauswahl der Plattform auf und ermöglicht es dem Benutzer, eine Uhrzeit auszuwählen. `TimePicker` definiert die folgenden Eigenschaften:

- [`Time`](xref:Xamarin.Forms.TimePicker.Time)vom Typ `TimeSpan` , der ausgewählte Zeitpunkt, der standardmäßig auf 0 (null) festgelegt ist `TimeSpan` . Der- `TimeSpan` Typ gibt einen Zeitraum seit Mitternacht an.
- [`Format`](xref:Xamarin.Forms.TimePicker.Format)vom Typ `string` , eine [standardmäßige](/dotnet/standard/base-types/standard-date-and-time-format-strings/) oder [benutzerdefinierte](/dotnet/standard/base-types/custom-date-and-time-format-strings/) .net-Formatierungs Zeichenfolge, die standardmäßig "t" ist, das Kurze Zeitmuster.
- [`TextColor`](xref:Xamarin.Forms.TimePicker.TextColor)vom Typ [`Color`](xref:Xamarin.Forms.Color) , die Farbe, die zum Anzeigen der ausgewählten Uhrzeit verwendet wird. der Standardwert ist [`Color.Default`](xref:Xamarin.Forms.Color.Default) .
- [`FontAttributes`](xref:Xamarin.Forms.TimePicker.FontAttributes)vom Typ [`FontAttributes`](xref:Xamarin.Forms.FontAttributes) , der standardmäßig auf festgelegt ist [`FontAtributes.None`](xref:Xamarin.Forms.FontAttributes.None) .
- [`FontFamily`](xref:Xamarin.Forms.TimePicker.FontFamily)vom Typ `string` , der standardmäßig auf festgelegt ist `null` .
- [`FontSize`](xref:Xamarin.Forms.TimePicker.FontSize)vom Typ `double` , der standardmäßig auf-1,0 festgelegt ist.
- `CharacterSpacing` vom Typ `double`: Abstand zwischen den Zeichen des `TimePicker`-Texts.

Alle diese Eigenschaften werden von-Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie formatiert werden können, und die Eigenschaften können Ziele von Daten Bindungen sein. Die- [`Time`](xref:Xamarin.Forms.TimePicker.Time) Eigenschaft verfügt über einen Standard Bindungs Modus von [`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) . Dies bedeutet, dass Sie ein Ziel einer Datenbindung in einer Anwendung sein kann, die die [Model-View-ViewModel (MVVM)-](~/xamarin-forms/enterprise-application-patterns/mvvm.md) Architektur verwendet.

Der [`TimePicker`](xref:Xamarin.Forms.TimePicker) enthält kein Ereignis, das einen neuen ausgewählten [`Time`](xref:Xamarin.Forms.TimePicker.Time) Wert angibt. Wenn Sie über dieses benachrichtigt werden müssen, können Sie einen Handler für das-Ereignis hinzufügen [`PropertyChanged`](xref:Xamarin.Forms.BindableObject.PropertyChanged) .

## <a name="initializing-the-time-property"></a>Initialisieren der Time-Eigenschaft

Im Code können Sie die- [`Time`](xref:Xamarin.Forms.TimePicker.Time) Eigenschaft auf einen Wert vom Typ initialisieren `TimeSpan` :

```csharp
TimePicker timePicker = new TimePicker
{
  Time = new TimeSpan(4, 15, 26) // Time set to "04:15:26"
};
```

Wenn die- [`Time`](xref:Xamarin.Forms.TimePicker.Time) Eigenschaft in XAML angegeben wird, wird der Wert in eine konvertiert `TimeSpan` und überprüft, um sicherzustellen, dass die Anzahl der Millisekunden größer oder gleich 0 (null) ist und die Anzahl der Stunden kleiner als 24 ist. Die Zeit Komponenten sollten durch Doppelpunkte getrennt werden:

```xaml
<TimePicker Time="4:15:26" />
```

Wenn die- [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) Eigenschaft von [`TimePicker`](xref:Xamarin.Forms.TimePicker) auf eine Instanz eines ViewModel festgelegt ist, das eine Eigenschaft des Typs mit dem `TimeSpan` Namen (z. b.) enthält `SelectedTime` , können Sie die wie folgt instanziieren `TimePicker` :

```xaml
<TimePicker Time="{Binding SelectedTime}" />
```

In diesem Beispiel wird die- [`Time`](xref:Xamarin.Forms.TimePicker.Time) Eigenschaft mit der- `SelectedTime` Eigenschaft in "ViewModel" initialisiert. Da die- `Time` Eigenschaft den Bindungs Modus hat [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) , wird jede neue Zeit, die der Benutzer auswählt, automatisch an das ViewModel weitergegeben.

Wenn das [`TimePicker`](xref:Xamarin.Forms.TimePicker) keine Bindung für seine-Eigenschaft enthält [`Time`](xref:Xamarin.Forms.TimePicker.Time) , sollte eine Anwendung einen Handler an das-Ereignis anfügen, [`PropertyChanged`](xref:Xamarin.Forms.BindableObject.PropertyChanged) um informiert zu werden, wenn der Benutzer eine neue Uhrzeit auswählt.

Weitere Informationen zum Festlegen von Schriftart Eigenschaften finden Sie unter [Schriftarten](~/xamarin-forms/user-interface/text/fonts.md).

## <a name="timepicker-and-layout"></a>TimePicker und Layout

Es ist möglich, eine nicht eingeschränkte horizontale Layoutoption wie `Center` , `Start` oder `End` mit zu verwenden [`TimePicker`](xref:Xamarin.Forms.TimePicker) :

```xaml
<TimePicker ···
            HorizontalOptions="Center"
            ··· />
```

Dies wird jedoch nicht empfohlen. Abhängig von der Einstellung der [`Format`](xref:Xamarin.Forms.TimePicker.Format) Eigenschaft werden für ausgewählte Zeiten möglicherweise unterschiedliche Anzeige breiten benötigt. Beispielsweise bewirkt die "t"-Format Zeichenfolge, dass die [`TimePicker`](xref:Xamarin.Forms.TimePicker) Ansicht Zeiten in einem langen Format anzeigt, und "4:15:26 am" erfordert eine höhere Anzeigebreite als das kurze Uhrzeit Format ("t") von "4:15 am". Abhängig von der Plattform kann dieser Unterschied bewirken, `TimePicker` dass die Ansicht die Breite im Layout ändert oder die Anzeige abgeschnitten wird.

> [!TIP]
> Es empfiehlt sich, die Standard `HorizontalOptions` Einstellung von mit zu verwenden `Fill` [`TimePicker`](xref:Xamarin.Forms.TimePicker) und keine Breite von `Auto` beim Einfügen `TimePicker` in eine Zelle zu verwenden `Grid` .

## <a name="timepicker-in-an-application"></a>TimePicker in einer Anwendung

Das [**"**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-timepicker) "-Beispiel enthält [`TimePicker`](xref:Xamarin.Forms.TimePicker) die [`Entry`](xref:Xamarin.Forms.Entry) Sichten, und [`Switch`](xref:Xamarin.Forms.Switch) auf der Seite. `TimePicker`Kann verwendet werden, um eine Uhrzeit auszuwählen. wenn diese Zeit eintritt, wird ein Warnungs Dialogfeld angezeigt, in dem der Benutzer an den Text in erinnert wird `Entry` , sofern der aktiviert `Switch` ist. Hier ist die XAML-Datei:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:SetTimer"
             x:Class="SetTimer.MainPage">
    <StackLayout>
        ...
        <Entry x:Name="_entry"
               Placeholder="Enter event to be reminded of" />
        <Label Text="Select the time below to be reminded at." />
        <TimePicker x:Name="_timePicker"
                    Time="11:00:00"
                    Format="T"
                    PropertyChanged="OnTimePickerPropertyChanged" />
        <StackLayout Orientation="Horizontal">
            <Label Text="Enable timer:" />
            <Switch x:Name="_switch"
                    HorizontalOptions="EndAndExpand"
                    Toggled="OnSwitchToggled" />
        </StackLayout>
    </StackLayout>
</ContentPage>
```

[`Entry`](xref:Xamarin.Forms.Entry)Mit dem können Sie Erinnerungs Text eingeben, der angezeigt wird, wenn der ausgewählte Zeitpunkt auftritt. Der [`TimePicker`](xref:Xamarin.Forms.TimePicker) wird dem [`Format`](xref:Xamarin.Forms.TimePicker.Format) langen Zeitformat die Eigenschaft "T" zugewiesen. Es ist ein Ereignishandler an das [`PropertyChanged`](xref:Xamarin.Forms.BindableObject.PropertyChanged) -Ereignis angefügt, und der [`Switch`](xref:Xamarin.Forms.Switch) verfügt über einen Handler, der mit seinem-Ereignis verbunden ist [`Toggled`](xref:Xamarin.Forms.Switch.Toggled) . Diese Ereignishandler befinden sich in der Code-Behind-Datei und nennen die- `SetTriggerTime` Methode:

```csharp
public partial class MainPage : ContentPage
{
    DateTime _triggerTime;

    public MainPage()
    {
        InitializeComponent();

        Device.StartTimer(TimeSpan.FromSeconds(1), OnTimerTick);
    }

    bool OnTimerTick()
    {
        if (_switch.IsToggled && DateTime.Now >= _triggerTime)
        {
            _switch.IsToggled = false;
            DisplayAlert("Timer Alert", "The '" + _entry.Text + "' timer has elapsed", "OK");
        }
        return true;
    }

    void OnTimePickerPropertyChanged(object sender, PropertyChangedEventArgs args)
    {
        if (args.PropertyName == "Time")
        {
            SetTriggerTime();
        }
    }

    void OnSwitchToggled(object sender, ToggledEventArgs args)
    {
        SetTriggerTime();
    }

    void SetTriggerTime()
    {
        if (_switch.IsToggled)
        {
            _triggerTime = DateTime.Today + _timePicker.Time;
            if (_triggerTime < DateTime.Now)
            {
                _triggerTime += TimeSpan.FromDays(1);
            }
        }
    }
}
```

Die `SetTriggerTime` -Methode berechnet eine Zeit Geber Zeit basierend auf dem `DateTime.Today` -Eigenschafts Wert und dem Wert, der `TimeSpan` von zurückgegeben wird [`TimePicker`](xref:Xamarin.Forms.TimePicker) . Dies ist erforderlich, da die- `DateTime.Today` Eigenschaft ein-Objekt zurückgibt `DateTime` , das das aktuelle Datum angibt, jedoch mit einer Uhrzeit von Mitternacht. Wenn die Zeit Geber Zeit bereits verstrichen ist, wird angenommen, dass Sie morgen ist.

Der Timer führt jede Sekunde einen Ticks `OnTimerTick` aus und führt die-Methode aus, die überprüft, ob [`Switch`](xref:Xamarin.Forms.Switch) auf ON fest steht und ob die aktuelle Zeit größer oder gleich der Zeit Geber Zeit ist. Wenn die Zeit Geber Zeit auftritt, [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) stellt die Methode dem Benutzer als Erinnerung ein Warn Dialogfeld zur Erinnerung.

Wenn das Beispiel zum ersten Mal ausgeführt wird, [`TimePicker`](xref:Xamarin.Forms.TimePicker) wird die Ansicht auf 11Uhr initialisiert. Wenn Sie auf das Tippen, wird `TimePicker` die Platt Form Zeitauswahl Die Plattformen implementieren die Zeitauswahl auf sehr unterschiedliche Weise, aber jeder Ansatz ist den Benutzern dieser Plattform vertraut:

[![Zeit auswählen](timepicker-images/timepicker-open.png "Zeit auswählen")](timepicker-images/timepicker-open-large.png#lightbox "Zeit auswählen")

> [!TIP]
> Unter Android kann das [`TimePicker`](xref:Xamarin.Forms.TimePicker) Dialogfeld angepasst werden, indem die- `CreateTimePickerDialog` Methode in einem benutzerdefinierten Renderer überschrieben wird. Dies ermöglicht z. b. das Hinzufügen zusätzlicher Schaltflächen zum Dialogfeld.

Nachdem Sie eine Uhrzeit ausgewählt haben, wird der ausgewählte Zeitpunkt in der folgenden Anzeige angezeigt [`TimePicker`](xref:Xamarin.Forms.TimePicker) :

[![Ausgewählte Zeit](timepicker-images/timepicker-selected.png "Ausgewählte Zeit")](timepicker-images/timepicker-selected-large.png#lightbox "Ausgewählte Zeit")

Wenn das-Objekt [`Switch`](xref:Xamarin.Forms.Switch) an der Position des-Objekts ein-und ausgeschaltet wird, zeigt die Anwendung ein Warnungs Dialogfeld an, das den Benutzer des Texts im anzeigt, [`Entry`](xref:Xamarin.Forms.Entry) Wenn der ausgewählte Zeitpunkt auftritt:

[![Timer-Popup](timepicker-images/timer-test.png "Timer-Popup")](timepicker-images/timer-test-large.png#lightbox "Timer-Popup")

Sobald das Warn Dialogfeld angezeigt wird, wird das-Objekt [`Switch`](xref:Xamarin.Forms.Switch) an die Position aus gewechselt.

## <a name="related-links"></a>Verwandte Links

- [Abtast Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-timepicker)
- [TimePicker-API](xref:Xamarin.Forms.TimePicker)
