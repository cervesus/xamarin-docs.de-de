---
title: Xamarin. Forms-timePicker
description: Die timePicker ist eine xamarin. Forms-Sicht, die dem Benutzer ermöglicht, eine Uhrzeit auszuwählen. In diesem Artikel wird erläutert, wie Sie eine timePicker in einer xamarin. Forms-Anwendung verwenden.
ms.prod: xamarin
ms.assetid: 2E99FB23-B82D-4EB4-AFB3-5002E736E7B2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/16/2018
ms.openlocfilehash: aae0791199b0e3042a3c619fcb11e7b877f52012
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72695925"
---
# <a name="xamarinforms-timepicker"></a>Xamarin. Forms-timePicker

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-timepicker)

_Eine xamarin. Forms-Ansicht, die es dem Benutzer ermöglicht, eine Uhrzeit auszuwählen._

Das xamarin. Forms- [`TimePicker`](xref:Xamarin.Forms.TimePicker) Ruft das Steuerelement für die Zeitauswahl der Plattform auf und ermöglicht es dem Benutzer, eine Uhrzeit auszuwählen. `TimePicker` definiert die folgenden Eigenschaften:

- [`Time`](xref:Xamarin.Forms.TimePicker.Time) vom Typ `TimeSpan`, der ausgewählte Zeitpunkt, der standardmäßig auf einen `TimeSpan` von 0 festgelegt ist. Der `TimeSpan` Typ gibt einen Zeitraum seit Mitternacht an.
- [`Format`](xref:Xamarin.Forms.TimePicker.Format) vom Typ `string`, eine [standardmäßige](/dotnet/standard/base-types/standard-date-and-time-format-strings/) oder [benutzerdefinierte](/dotnet/standard/base-types/custom-date-and-time-format-strings/) .net-Formatierungs Zeichenfolge, die standardmäßig "t" ist, das Kurze Zeitmuster.
- [`TextColor`](xref:Xamarin.Forms.TimePicker.TextColor) vom Typ [`Color`](xref:Xamarin.Forms.Color), die Farbe, die zum Anzeigen der ausgewählten Zeit verwendet wird, wobei der Standardwert [`Color.Default`](xref:Xamarin.Forms.Color.Default)ist.
- [`FontAttributes`](xref:Xamarin.Forms.TimePicker.FontAttributes) vom Typ [`FontAttributes`](xref:Xamarin.Forms.FontAttributes), dessen standardmäßig [`FontAtributes.None`](xref:Xamarin.Forms.FontAttributes.None)ist.
- [`FontFamily`](xref:Xamarin.Forms.TimePicker.FontFamily) vom Typ `string`, dessen standardmäßig `null` ist.
- [`FontSize`](xref:Xamarin.Forms.TimePicker.FontSize) vom Typ `double`, der standardmäßig auf-1,0 festgelegt ist.
- `CharacterSpacing` vom Typ `double` ist der Abstand zwischen den Zeichen des `TimePicker` Texts.

Alle diese Eigenschaften werden durch [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekte gestützt. Dies bedeutet, dass Sie formatiert werden können, und die Eigenschaften können Ziele von Daten Bindungen sein. Die [`Time`](xref:Xamarin.Forms.TimePicker.Time) -Eigenschaft verfügt über einen Standard Bindungs Modus von [`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay). Dies bedeutet, dass Sie ein Ziel einer Datenbindung in einer Anwendung sein kann, die die [Model-View-ViewModel (MVVM)-](~/xamarin-forms/enterprise-application-patterns/mvvm.md) Architektur verwendet.

Der [`TimePicker`](xref:Xamarin.Forms.TimePicker) enthält kein Ereignis, um einen neuen ausgewählten [`Time`](xref:Xamarin.Forms.TimePicker.Time) Wert anzugeben. Wenn Sie über dieses benachrichtigt werden müssen, können Sie einen Handler für das [`PropertyChanged`](xref:Xamarin.Forms.BindableObject.PropertyChanged) -Ereignis hinzufügen.

## <a name="initializing-the-time-property"></a>Initialisieren der Time-Eigenschaft

Im Code können Sie die [`Time`](xref:Xamarin.Forms.TimePicker.Time) -Eigenschaft mit einem Wert vom Typ `TimeSpan` initialisieren:

```csharp
TimePicker timePicker = new TimePicker
{
  Time = new TimeSpan(4, 15, 26) // Time set to "04:15:26"
};
```

Wenn die [`Time`](xref:Xamarin.Forms.TimePicker.Time) -Eigenschaft in XAML angegeben ist, wird der Wert in einen `TimeSpan` konvertiert und überprüft, um sicherzustellen, dass die Anzahl der Millisekunden größer oder gleich 0 (null) ist und die Anzahl der Stunden kleiner als 24 ist. Die Zeit Komponenten sollten durch Doppelpunkte getrennt werden:

```xaml
<TimePicker Time="4:15:26" />
```

Wenn die [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) -Eigenschaft von [`TimePicker`](xref:Xamarin.Forms.TimePicker) auf eine Instanz von ViewModel festgelegt ist, die eine Eigenschaft des Typs `TimeSpan` benannte `SelectedTime` enthält (z. b.), können Sie die `TimePicker` wie folgt instanziieren:

```xaml
<TimePicker Time="{Binding SelectedTime}" />
```

In diesem Beispiel wird die [`Time`](xref:Xamarin.Forms.TimePicker.Time) -Eigenschaft mit der `SelectedTime`-Eigenschaft in "ViewModel" initialisiert. Da die `Time`-Eigenschaft über den Bindungs Modus [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay)verfügt, wird jede neue Zeit, die der Benutzer auswählt, automatisch an das ViewModel weitergegeben.

Wenn die [`TimePicker`](xref:Xamarin.Forms.TimePicker) keine Bindung für die [`Time`](xref:Xamarin.Forms.TimePicker.Time) -Eigenschaft enthält, sollte eine Anwendung einen Handler an das [`PropertyChanged`](xref:Xamarin.Forms.BindableObject.PropertyChanged) Ereignis anfügen, um informiert zu werden, wenn der Benutzer eine neue Uhrzeit auswählt.

Weitere Informationen zum Festlegen von Schriftart Eigenschaften finden Sie unter [Schriftarten](~/xamarin-forms/user-interface/text/fonts.md).

## <a name="timepicker-and-layout"></a>TimePicker und Layout

Es ist möglich, eine nicht eingeschränkte horizontale Layoutoption wie z. b. `Center`, `Start` oder `End` mit [`TimePicker`](xref:Xamarin.Forms.TimePicker)zu verwenden:

```xaml
<TimePicker ···
            HorizontalOptions="Center"
            ··· />
```

Dies wird jedoch nicht empfohlen. Abhängig von der Einstellung der [`Format`](xref:Xamarin.Forms.TimePicker.Format) Eigenschaft werden für ausgewählte Zeiten möglicherweise andere Anzeigebreite benötigt. Beispielsweise bewirkt die "t"-Format Zeichenfolge, dass in der [`TimePicker`](xref:Xamarin.Forms.TimePicker) Ansicht Zeiten in einem langen Format angezeigt werden und "4:15:26 am" eine höhere Anzeigebreite als das kurze Uhrzeit Format ("t") von "4:15 am" erfordert. Abhängig von der Plattform kann dieser Unterschied dazu führen, dass die `TimePicker` Ansicht die Breite im Layout ändert oder die Anzeige abgeschnitten wird.

> [!TIP]
> Es empfiehlt sich, die Standardeinstellung für die `HorizontalOptions` `Fill` mit [`TimePicker`](xref:Xamarin.Forms.TimePicker)zu verwenden und keine Breite von `Auto` zu verwenden, wenn `TimePicker` in eine `Grid` Zelle versetzt wird.

## <a name="timepicker-in-an-application"></a>TimePicker in einer Anwendung

Das Beispiel "*" enthält die Ansichten [`TimePicker`](xref:Xamarin.Forms.TimePicker), [`Entry`](xref:Xamarin.Forms.Entry)und [`Switch`](xref:Xamarin.Forms.Switch) [**auf der entsprechenden**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-timepicker) Seite. Der `TimePicker` kann verwendet werden, um eine Uhrzeit auszuwählen. wenn diese Zeit eintritt, wird ein Warnungs Dialogfeld angezeigt, in dem der Benutzer über den Text im `Entry` benachrichtigt wird, sofern die `Switch` aktiviert ist. Hier ist die XAML-Datei:

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

Mit der [`Entry`](xref:Xamarin.Forms.Entry) können Sie Erinnerungs Text eingeben, der angezeigt wird, wenn der ausgewählte Zeitpunkt auftritt. Dem [`TimePicker`](xref:Xamarin.Forms.TimePicker) wird eine [`Format`](xref:Xamarin.Forms.TimePicker.Format) Eigenschaft "t" für das lange Zeitformat zugewiesen. Es ist ein Ereignishandler an das [`PropertyChanged`](xref:Xamarin.Forms.BindableObject.PropertyChanged) -Ereignis angefügt, und der [`Switch`](xref:Xamarin.Forms.Switch) verfügt über einen Handler, der mit seinem [`Toggled`](xref:Xamarin.Forms.Switch.Toggled) Ereignis verbunden ist. Diese Ereignishandler befinden sich in der Code-Behind-Datei und wenden die `SetTriggerTime`-Methode an:

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

Die `SetTriggerTime`-Methode berechnet eine Zeit Geber Zeit basierend auf dem `DateTime.Today`-Eigenschafts Wert und dem `TimeSpan` Wert, der vom [`TimePicker`](xref:Xamarin.Forms.TimePicker)zurückgegeben wurde. Dies ist erforderlich, da die `DateTime.Today`-Eigenschaft eine `DateTime` zurückgibt, die das aktuelle Datum angibt, jedoch mit einer Uhrzeit von Mitternacht. Wenn die Zeit Geber Zeit bereits verstrichen ist, wird angenommen, dass Sie morgen ist.

Der Timer führt jede Sekunde einen Ticks aus und führt die `OnTimerTick` Methode aus, die überprüft, ob die [`Switch`](xref:Xamarin.Forms.Switch) eingeschaltet ist und ob die aktuelle Zeit größer oder gleich der Zeit Geber Zeit ist. Wenn die Zeit Geber Zeit auftritt, stellt die [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) -Methode dem Benutzer als Erinnerung ein Warn Dialogfeld zur Erinnerung.

Wenn das Beispiel zum ersten Mal ausgeführt wird, wird die [`TimePicker`](xref:Xamarin.Forms.TimePicker) Ansicht mit 11Uhr initialisiert. Wenn Sie auf die `TimePicker` tippen, wird die Platt Form Zeitauswahl aufgerufen Die Plattformen implementieren die Zeitauswahl auf sehr unterschiedliche Weise, aber jeder Ansatz ist den Benutzern dieser Plattform vertraut:

[![Zeit auswählen](timepicker-images/timepicker-open.png "Zeit auswählen")](timepicker-images/timepicker-open-large.png#lightbox "Zeit auswählen")

> [!TIP]
> Unter Android kann das Dialogfeld " [`TimePicker`](xref:Xamarin.Forms.TimePicker) " angepasst werden, indem die `CreateTimePickerDialog`-Methode in einem benutzerdefinierten Renderer überschrieben wird. Dies ermöglicht z. b. das Hinzufügen zusätzlicher Schaltflächen zum Dialogfeld.

Nachdem Sie eine Uhrzeit ausgewählt haben, wird der ausgewählte Zeitpunkt im [`TimePicker`](xref:Xamarin.Forms.TimePicker)angezeigt:

[![Ausgewählte Zeit](timepicker-images/timepicker-selected.png "Ausgewählte Zeit")](timepicker-images/timepicker-selected-large.png#lightbox "Ausgewählte Zeit")

Vorausgesetzt, dass die [`Switch`](xref:Xamarin.Forms.Switch) an der Position ein-/ausgeschaltet wird, zeigt die Anwendung ein Warnungs Dialogfeld an, das den Benutzer über den Text in der [`Entry`](xref:Xamarin.Forms.Entry) benachrichtigt, wenn der ausgewählte Zeitpunkt auftritt:

[![Timer-Popup](timepicker-images/timer-test.png "Timer-Popup")](timepicker-images/timer-test-large.png#lightbox "Timer-Popup")

Sobald das Dialogfeld "Warnung" angezeigt wird, wird der [`Switch`](xref:Xamarin.Forms.Switch) an die Position "aus" gewechselt.

## <a name="related-links"></a>Verwandte Links

- [Abtast Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-timepicker)
- [TimePicker-API](xref:Xamarin.Forms.TimePicker)
