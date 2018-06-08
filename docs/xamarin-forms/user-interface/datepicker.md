---
title: DatePicker verwenden
description: Eine Xamarin.Forms-Sicht, die dem Benutzer ermöglicht, ein Datum auswählen
ms.prod: xamarin
ms.assetid: 68E8EF8A-42E7-4939-8ABE-64D060E609D9
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/04/2018
ms.openlocfilehash: 09b0bd788d9ac436e0270b447556ad2b0a848f99
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848563"
---
# <a name="using-datepicker"></a>DatePicker verwenden

_Eine Xamarin.Forms-Sicht, die dem Benutzer ermöglicht, ein Datum auswählen_

Der Xamarin.Forms [ `DatePicker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) die Plattform Datumsauswahl Steuerelement ruft auf und ermöglicht es dem Benutzer ein Datum auswählen. `DatePicker` werden acht Eigenschaften definiert:

- [`MinimumDate`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MinimumDate/) Der Typ [ `DateTime` ](https://developer.xamarin.com/api/type/System.DateTime/), die standardmäßig des ersten Tages des Jahres 1900.
- [`MaximumDate`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MaximumDate/) Der Typ `DateTime`, die den Standardwert bis zum letzten Tag des Jahres 2100.
- [`Date`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Date/) Der Typ `DateTime`, die dem ausgewählten Datum wird auf den Wert standardmäßig- [ `DateTime.Today` ](https://developer.xamarin.com/api/property/System.DateTime.Today/).
- [`Format`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Format/) vom Typ `string`, [standard](/dotnet/standard/base-types/standard-date-and-time-format-strings/) oder [benutzerdefinierte](/dotnet/standard/base-types/custom-date-and-time-format-strings/) .NET-Formatzeichenfolge, dessen Standard "D", die lange Datum Muster.
- [`TextColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.TextColor/) Der Typ [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/), die Farbe, mit das ausgewählte Datum angezeigt, deren Standard [ `Color.Default` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Default/).
- [`FontAttributes`](xref:Xamarin.Forms.DatePicker.FontAttributes) Der Typ [ `FontAttributes` ](xref:Xamarin.Forms.FontAttributes), die standardmäßig die [ `FontAtributes.None` ](xref:Xamarin.Forms.FontAttributes.None).
- [`FontFamily`](xref:Xamarin.Forms.DatePicker.FontFamily) Der Typ `string`, die standardmäßig die `null`.
- [`FontSize`](xref:Xamarin.Forms.DatePicker.FontSize) Der Typ `double`, die standardmäßig die -1.0.

Die `DatePicker` ausgelöst wird eine [ `DateSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.DatePicker.DateSelected/) Ereignis aus, wenn der Benutzer wählt ein Datum aus.

> [!WARNING]
> Beim Festlegen von `MinimumDate` und `MaximumDate`, stellen Sie sicher, dass `MinimumDate` ist immer kleiner als oder gleich `MaximumDate`. Andernfalls `DatePicker` wird eine Ausnahme ausgelöst.

Intern können die `DatePicker` wird sichergestellt, dass `Date` zwischen `MinimumDate` und `MaximumDate`(einschließlich). Wenn `MinimumDate` oder `MaximumDate` festgelegt ist, damit `Date` befindet sich nicht zwischen, `DatePicker` passen den Wert des `Date`.

Alle acht Eigenschaften werden durch gestützt [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) Objekte, was bedeutet, dass sie die formatiert werden können, und die Eigenschaften können Ziele von datenbindungen. Die `Date` Eigenschaft hat einen Standardmodus für die Bindung des [ `BindingMode.TwoWay` ](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.TwoWay/), was bedeutet, dass es ein Ziel einer Bindung in einer Anwendung sein kann, verwendet der [Model-View-ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) Architektur.

## <a name="initializing-the-datetime-properties"></a>Initialisieren der Eigenschaften "DateTime"

Sie können im Code Initialisieren der `MinimumDate`, `MaximumDate`, und `Date` Eigenschaften zu Werten vom Typ `DateTime`:

```csharp
DatePicker datePicker = new DatePicker
{
    MinimumDate = new DateTime(2018, 1, 1),
    MaximumDate = new DateTime(2018, 12, 31),
    Date = new DateTime(2018, 6, 21)
};
```

Wenn eine `DateTime` angegeben Wert in XAML wird die Verwendung von XAML-Parser beansprucht der `DateTime.Parse` Methode mit einer `CultureInfo.InvariantCulture` Argument, um die Zeichenfolge zu konvertieren eine `DateTime` Wert. Die Datumsangaben in einem präzisen Format angegeben werden: zweistellige Monate, Tage zweistellige und vierstellige Jahresangaben durch Schrägstriche getrennte:

```xaml
<DatePicker MinimumDate="01/01/2018"
            MaximumDate="12/31/2018"
            Date="06/21/2018" />
```

Wenn die `BindingContext` Eigenschaft `DatePicker` mit einer Instanz von einem ViewModel, enthält die Eigenschaften des Typs festgelegt ist `DateTime` mit dem Namen `MinDate`, `MaxDate`, und `SelectedDate` (z. B.), können Sie instanziieren der `DatePicker` folgendermaßen :

```xaml
<DatePicker MinimumDate="{Binding MinDate}"
            MaximumDate="{Binding MaxDate}"
            Date="{Binding SelectedDate}" />
```

In diesem Beispiel werden alle drei Eigenschaften mit den entsprechenden Eigenschaften in das ViewModel initialisiert. Da die `Date` Eigenschaft verfügt über eine Bindungsmodus der `TwoWay`, alle neuen Datum, die wählt der Benutzer automatisch in das ViewModel wiedergegeben wird.

Wenn die `DatePicker` enthält eine Bindung nicht auf seine `Date` -Eigenschaft, eine Anwendung sollte einen Handler zum Anfügen der `DateSelected` Ereignis darüber informiert, wenn der Benutzer ein neues Datum auswählt.

Informationen zum Festlegen von Schriftarteigenschaften finden Sie unter [Schriftarten](~/xamarin-forms/user-interface/text/fonts.md).

## <a name="datepicker-and-layout"></a>DatePicker und layout

Es ist möglich, eine nicht eingeschränkte horizontales Layout-Option verwenden, z. B. `Center`, `Start`, oder `End` mit `DatePicker`:

```xaml
<DatePicker ···
            HorizontalOptions="Center"
            ··· />
```

Dies ist jedoch nicht empfohlen. Abhängig von der Einstellung von der `Format` Eigenschaft ausgewählt Datumsangaben möglicherweise unterschiedlichen breiten erforderlich. Die Formatzeichenfolge "D" verursacht beispielsweise `DateTime` zum Anzeigen von Datumsangaben in einem Langformat und "Mittwoch, 12 September 2018" erfordert eine Breite größer als "Freitag, 4 Mai 2018". Abhängig von der Plattform dieser Unterschied kann dazu führen, dass die `DateTime` Ansicht so ändern Sie die Breite in Layout oder für die Anzeige abgeschnitten werden.

> [!TIP]
> Es wird empfohlen, die Standardeinstellung verwenden `HorizontalOptions` Einstellung des `Fill` mit `DatePicker`, und nicht mit einer Breite von `Auto` beim `DatePicker` in einer `Grid` Zelle.

## <a name="datepicker-in-an-application"></a>DatePicker in einer Anwendung

Die [ **DaysBetweenDates** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/DatePicker) Beispiel enthält zwei `DatePicker` Ansichten auf der Seite. Diese können verwendet werden, um zwei Datumsangaben auszuwählen, und das Programm berechnet die Anzahl der Tage zwischen diesen Daten. Ändern Sie das Programm nicht die Einstellungen der `MinimumDate` und `MaximumDate` Eigenschaften, damit die beiden Datumsangaben zwischen 1900 und 2100 sein müssen.

Hier wird die Verwendung von XAML-Datei ein:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DaysBetweenDates"
             x:Class="DaysBetweenDates.MainPage">
    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>

    <StackLayout Margin="10">
        <Label Text="Days Between Dates"
               Style="{DynamicResource TitleStyle}"
               Margin="0, 20"
               HorizontalTextAlignment="Center" />

        <Label Text="Start Date:" />

        <DatePicker x:Name="startDatePicker"
                    Format="D"
                    Margin="30, 0, 0, 30"
                    DateSelected="OnDateSelected" />

        <Label Text="End Date:" />

        <DatePicker x:Name="endDatePicker"
                    MinimumDate="{Binding Source={x:Reference startDatePicker},
                                          Path=Date}"
                    Format="D"
                    Margin="30, 0, 0, 30"
                    DateSelected="OnDateSelected" />

        <StackLayout Orientation="Horizontal"
                     Margin="0, 0, 0, 30">
            <Label Text="Include both days in total: "
                   VerticalOptions="Center" />
            <Switch x:Name="includeSwitch"
                    Toggled="OnSwitchToggled" />
        </StackLayout>

        <Label x:Name="resultLabel"
               FontAttributes="Bold"
               HorizontalTextAlignment="Center" />

    </StackLayout>
</ContentPage>
```

Jede `DatePicker` erhält eine `Format` Eigenschaft von "D" für einen langen Datumsformat. Beachten Sie auch, dass die `endDatePicker` Objekt besitzt eine Bindung, die als Ziel verwendet die `MinimumDate` Eigenschaft. Bindungsquelle ist auf dem ausgewählten `Date` Eigenschaft von der `startDatePicker` Objekt. Dadurch wird sichergestellt, dass das Enddatum immer zu einem späteren Zeitpunkt als oder gleich dem Startdatum ist. Zusätzlich zu den beiden `DatePicker` Objekte eine `Switch` ist mit der Bezeichnung "Include beide Tage insgesamt".

Die beiden `DatePicker` Ansichten verfügen über Handler verknüpft die `DateSelected` -Ereignis und die `Switch` an ein Handler angefügt seine `Toggled` Ereignis. Diese Ereignishandler sind in der CodeBehind-Datei und eine neue Berechnung der Tage zwischen den beiden Datumsangaben auslösen:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }

    void OnDateSelected(object sender, DateChangedEventArgs args)
    {
        Recalculate();
    }

    void OnSwitchToggled(object sender, ToggledEventArgs args)
    {
        Recalculate();
    }

    void Recalculate()
    {
        TimeSpan timeSpan = endDatePicker.Date - startDatePicker.Date +
            (includeSwitch.IsToggled ? TimeSpan.FromDays(1) : TimeSpan.Zero);

        resultLabel.Text = String.Format("{0} day{1} between dates",
                                            timeSpan.Days, timeSpan.Days == 1 ? "" : "s");
    }
}
```

Beim Ausführen des Beispiels wird zuerst beide `DatePicker` Ansichten werden initialisiert, das heutige Datum. Das folgende Bildschirmfoto zeigt das Programm auf iOS-, Android- und universellen Windows-Plattform ausgeführt wird:

[![Wochen zwischen Datumswerten anfangen](datepicker-images/DaysBetweenDatesStart.png "Wochen zwischen Datumswerten anfangen")](datepicker-images/DaysBetweenDatesStart-Large.png#lightbox "Tage zwischen Datumsangaben starten")

Tippen Sie auf eines der `DatePicker` aufruft, zeigt die Datumsauswahl Plattform. Die drei Plattformen implementieren diese Datumsauswahl sehr unterschiedlich, aber jeder Ansatz ist für Benutzer dieser Plattform vertraut:

[![Wählen Sie Tage zwischen Datumsangaben](datepicker-images/DaysBetweenDatesSelect.png "Tage zwischen Datumswerten auswählen")](datepicker-images/DaysBetweenDatesSelect-Large.png#lightbox "Tage zwischen Datumswerten auswählen")

Nach zwei Datumsangaben ausgewählt sind, zeigt die Anwendung die Anzahl der Tage zwischen diesen Datumswerten:

[![Tage zwischen Datumsangaben Ergebnis](datepicker-images/DaysBetweenDatesResult.png "Tage zwischen Datumsangaben Ergebnis")](datepicker-images/DaysBetweenDatesResult-Large.png#lightbox "Tage zwischen Datumsangaben Ergebnis")

## <a name="related-links"></a>Verwandte Links

- [DaysBetweenDates-Beispiel](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/DatePicker)
- [DatePicker-API](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/)
