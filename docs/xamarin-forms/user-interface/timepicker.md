---
title: Xamarin.Forms TimePicker
description: TimePicker handelt es sich um eine Xamarin.Forms-Sicht, die dem Benutzer ermöglicht, eine Uhrzeit auswählen. In diesem Artikel wird erläutert, wie ein TimePicker in einer Xamarin.Forms-Anwendung genutzt wird.
ms.prod: xamarin
ms.assetid: 2E99FB23-B82D-4EB4-AFB3-5002E736E7B2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/16/2018
ms.openlocfilehash: 1b929b507d738cb4000bab20cfab5480b2222ed2
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61024087"
---
# <a name="xamarinforms-timepicker"></a>Xamarin.Forms TimePicker

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TimePicker/)

_Eine Xamarin.Forms-Sicht, die dem Benutzer ermöglicht, eine Uhrzeit auswählen._

Die Xamarin.Forms [ `TimePicker` ](xref:Xamarin.Forms.TimePicker) der Plattform Zeitauswahl Steuerelement ruft auf und ermöglicht dem Benutzer, die eine Uhrzeit auswählen. `TimePicker` definiert die folgenden Eigenschaften an:

- [`Time`](xref:Xamarin.Forms.TimePicker.Time) vom Typ `TimeSpan`, die der ausgewählten Zeit, die standardmäßig einen `TimeSpan` 0. Die `TimeSpan` Typ gibt an, eine bestimmte Zeitdauer seit Mitternacht.
- [`Format`](xref:Xamarin.Forms.TimePicker.Format) vom Typ `string`, [standard](/dotnet/standard/base-types/standard-date-and-time-format-strings/) oder [benutzerdefinierte](/dotnet/standard/base-types/custom-date-and-time-format-strings/) .NET Formatierung der Zeichenfolge, die standardmäßig auf "t", die das Muster für kurze Zeit.
- [`TextColor`](xref:Xamarin.Forms.TimePicker.TextColor) Der Typ [ `Color` ](xref:Xamarin.Forms.Color), die Farbe verwendet, um den ausgewählten Zeitraum anzuzeigen, deren Standard [ `Color.Default` ](xref:Xamarin.Forms.Color.Default).
- [`FontAttributes`](xref:Xamarin.Forms.TimePicker.FontAttributes) Der Typ [ `FontAttributes` ](xref:Xamarin.Forms.FontAttributes), die standardmäßig auf [ `FontAtributes.None` ](xref:Xamarin.Forms.FontAttributes.None).
- [`FontFamily`](xref:Xamarin.Forms.TimePicker.FontFamily) Der Typ `string`, die standardmäßig auf `null`.
- [`FontSize`](xref:Xamarin.Forms.TimePicker.FontSize) Der Typ `double`, die standardmäßig auf den Bereich von -1,0.

Alle diese Eigenschaften verfügen über [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) -Objekte, die bedeutet, dass sie die formatiert werden können, und die Eigenschaften können Ziele von datenbindungen. Die [ `Time` ](xref:Xamarin.Forms.TimePicker.Time) Eigenschaft verfügt über einen Standardmodus für die Bindung der [ `BindingMode.TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay), was bedeutet, dass es ein Ziel einer Bindung in einer Anwendung sein kann, verwendet der [ Model-View-ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) Architektur.

Die [ `TimePicker` ](xref:Xamarin.Forms.TimePicker) eine Ereignis, um anzugeben, eine neue beinhaltet keine [ `Time` ](xref:Xamarin.Forms.TimePicker.Time) Wert. Wenn Sie dieses benachrichtigt werden möchten, können Sie einen Handler für Hinzufügen der [ `PropertyChanged` ](xref:Xamarin.Forms.BindableObject.PropertyChanged) Ereignis.

## <a name="initializing-the-time-property"></a>Initialisieren der Eigenschaft

Sie können im Code Initialisieren der [ `Time` ](xref:Xamarin.Forms.TimePicker.Time) Eigenschaft auf einen Wert vom Typ `TimeSpan`:

```csharp
TimePicker timePicker = new TimePicker
{
  Time = new TimeSpan(4, 15, 26) // Time set to "04:15:26"
};
```

Wenn die [ `Time` ](xref:Xamarin.Forms.TimePicker.Time) in XAML angegeben wird, wird der Wert in konvertiert eine `TimeSpan` und überprüft, um sicherzustellen, dass die Anzahl der Millisekunden größer als oder gleich 0 ist und die Anzahl der Stunden auf weniger als 24 Stunden ist. Die Zeitkomponenten sollten durch Doppelpunkte getrennt werden:

```xaml
<TimePicker Time="4:15:26" />
```

Wenn die [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) Eigenschaft [ `TimePicker` ](xref:Xamarin.Forms.TimePicker) festgelegt ist, um eine Instanz eines "ViewModels" mit einer Eigenschaft vom Typ `TimeSpan` mit dem Namen `SelectedTime` (z. B.), können Sie instanziieren die `TimePicker` wie folgt aus:

```xaml
<TimePicker Time="{Binding SelectedTime}" />
```

In diesem Beispiel die [ `Time` ](xref:Xamarin.Forms.TimePicker.Time) Eigenschaft wird initialisiert, um die `SelectedTime` -Eigenschaft in "ViewModel". Da die `Time` Eigenschaft verfügt über eine Bindungsmodus der [ `TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay), jederzeit neue, die der Benutzer wählt automatisch an das ViewModel weitergegeben wird.

Wenn die [ `TimePicker` ](xref:Xamarin.Forms.TimePicker) enthält keine Bindung auf seine [ `Time` ](xref:Xamarin.Forms.TimePicker.Time) -Eigenschaft muss eine Anwendung einen Handler zum Anfügen der [ `PropertyChanged` ](xref:Xamarin.Forms.BindableObject.PropertyChanged) Ereignis werden Sie informiert, wenn der Benutzer eine neue Zeit auswählt.

Informationen zum Festlegen von Schriftart-Eigenschaften finden Sie unter [Schriftarten](~/xamarin-forms/user-interface/text/fonts.md).

## <a name="timepicker-and-layout"></a>TimePicker und layout

Es ist möglich, eine uneingeschränkte horizontales Layout-Option verwenden, z. B. `Center`, `Start`, oder `End` mit [ `TimePicker` ](xref:Xamarin.Forms.TimePicker):

```xaml
<TimePicker ···
            HorizontalOptions="Center"
            ··· />
```

Dies wird jedoch nicht empfohlen. Entsprechend der Einstellung von der [ `Format` ](xref:Xamarin.Forms.TimePicker.Format) -Eigenschaft ausgewählten Zeiten unter Umständen andere breiten. Die Formatzeichenfolge "T" beispielsweise bewirkt, dass die [ `TimePicker` ](xref:Xamarin.Forms.TimePicker) anzeigen, um in einem langen Format anzuzeigen, und "4:15:26 Uhr" erfordert eine größere Anzeigebreite als das kurze Uhrzeitformat von ("t") von "4:15 Uhr". Abhängig von der Plattform, dieser Unterschied kann dazu führen, dass die `TimePicker` Ansicht so ändern Sie die Breite in Layout oder für die Anzeige abgeschnitten werden.

> [!TIP]
> Es wird empfohlen, die Standardeinstellung verwenden `HorizontalOptions` -Einstellung `Fill` mit [ `TimePicker` ](xref:Xamarin.Forms.TimePicker), und nicht mit einer Breite von `Auto` beim `TimePicker` in einer `Grid` Zelle.

## <a name="timepicker-in-an-application"></a>TimePicker in einer Anwendung

Die [ **SetTimer** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TimePicker/) Beispiel enthält [ `TimePicker` ](xref:Xamarin.Forms.TimePicker), [ `Entry` ](xref:Xamarin.Forms.Entry), und [ `Switch` ](xref:Xamarin.Forms.Switch) Ansichten auf der Seite. Die `TimePicker` können verwendet werden, um eine Uhrzeit auswählen und tritt auf, wenn Zeit, die ein Dialogfeld "Warnung" wird angezeigt, die den Benutzer Text im daran erinnert die `Entry`, vorausgesetzt die `Switch` eingeschaltet ist. Hier ist die XAML-Datei ein:

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

Die [ `Entry` ](xref:Xamarin.Forms.Entry) können Sie die erinnerungstext eingeben, der angezeigt wird, wenn die ausgewählte Zeit erfolgt. Die [ `TimePicker` ](xref:Xamarin.Forms.TimePicker) erhält eine [ `Format` ](xref:Xamarin.Forms.TimePicker.Format) Eigenschaft von "T" für lange Uhrzeitformat. Einen Ereignishandler angefügt werden kann die [ `PropertyChanged` ](xref:Xamarin.Forms.BindableObject.PropertyChanged) Ereignis und die [ `Switch` ](xref:Xamarin.Forms.Switch) wurde ein Handler angefügt dessen [ `Toggled` ](xref:Xamarin.Forms.Switch.Toggled) Ereignis. Diese Ereignishandler werden in der CodeBehind-Datei und rufen die `SetTriggerTime` Methode:

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

Die `SetTriggerTime` Methode berechnet einen Timer Zeit auf Grundlage der `DateTime.Today` Eigenschaftswert und `TimeSpan` Wert, der von der [ `TimePicker` ](xref:Xamarin.Forms.TimePicker). Dies ist erforderlich, da die `DateTime.Today` -Eigenschaft gibt eine `DateTime` , der angibt, des aktuellen Datums, aber mit einer Uhrzeit von Mitternacht. Wenn der Zeitgeber Zeit noch heute bereits verstrichen ist, wird davon ausgegangen, dass es morgen sein.

Den zeitgebertakten pro Sekunde, die Ausführung der `OnTimerTick` Methode, die überprüft, ob die [ `Switch` ](xref:Xamarin.Forms.Switch) ist auf und gibt an, ob die aktuelle Zeit ist größer als oder gleich der Timer-Zeit. Bei der die Timer-Zeit werden die [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert*) Methode zeigt ein Dialogfeld "Warnung" für dem Benutzer als Erinnerung.

Beim ersten des Beispiels ausführen, das [ `TimePicker` ](xref:Xamarin.Forms.TimePicker) Ansicht um 11 Uhr initialisiert. Durch Tippen auf die `TimePicker` Ruft die Uhrzeit der Plattform. Die Plattformen implementiert die Zeitauswahl sehr unterschiedlich, aber jeder Ansatz Benutzern dieser Plattform vertraut sind:

[![Wählen Sie die Zeit](timepicker-images/timepicker-open.png "wählen")](timepicker-images/timepicker-open-large.png#lightbox "auswählen")

> [!TIP]
> Unter Android die [ `TimePicker` ](xref:Xamarin.Forms.TimePicker) Dialogfeld kann angepasst werden, durch Überschreiben der `CreateTimePickerDialog` -Methode in der ein benutzerdefinierter Renderer. Dies kann beispielsweise zusätzliche Schaltflächen im Dialogfeld hinzugefügt werden.

Nach der Auswahl einer Zeit, wird die ausgewählte Zeit angezeigt, der [ `TimePicker` ](xref:Xamarin.Forms.TimePicker):

[![Ausgewählte Zeit](timepicker-images/timepicker-selected.png "ausgewählten Zeitraum")](timepicker-images/timepicker-selected-large.png#lightbox "ausgewählten Zeitraum")

Vorausgesetzt, dass die [ `Switch` ](xref:Xamarin.Forms.Switch) umgeschaltet, in die Position, die Anwendung zeigt ein Dialogfeld "Warnung" erinnert des Benutzers Text in die [ `Entry` ](xref:Xamarin.Forms.Entry) Zeitpunkt des Auftretens der ausgewählten Zeit:

[![Timer-Popup](timepicker-images/timer-test.png "Timer Popup")](timepicker-images/timer-test-large.png#lightbox "Timer-Popup")

Sobald das Dialogfeld "Warnung" angezeigt wird, die [ `Switch` ](xref:Xamarin.Forms.Switch) umgeschaltet, auf die Position aus.

## <a name="related-links"></a>Verwandte Links

- [SetTimer-Beispiel](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TimePicker/)
- [TimePicker-API](xref:Xamarin.Forms.TimePicker)
