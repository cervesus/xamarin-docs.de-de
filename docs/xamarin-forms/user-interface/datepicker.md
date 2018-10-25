---
title: Xamarin.Forms "DatePicker"
description: "\"DatePicker\" handelt es sich um eine Xamarin.Forms-Sicht, die dem Benutzer ermöglicht, ein Datum auswählen. In diesem Artikel wird erläutert, wie ein \"DatePicker\" in einer Xamarin.Forms-Anwendung genutzt wird."
ms.prod: xamarin
ms.assetid: 68E8EF8A-42E7-4939-8ABE-64D060E609D9
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/04/2018
ms.openlocfilehash: 7917510e910223fc6ca276bf47b1878c19557a38
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2018
ms.locfileid: "38994925"
---
# <a name="xamarinforms-datepicker"></a>Xamarin.Forms "DatePicker"

_Eine Xamarin.Forms-Sicht, die dem Benutzer ermöglicht, ein Datum auswählen._

Die Xamarin.Forms [ `DatePicker` ](xref:Xamarin.Forms.DatePicker) ruft Datumsauswahl-Steuerelement von der Plattform und ermöglicht dem Benutzer ein Datum auswählen. `DatePicker` werden acht Eigenschaften definiert:

- [`MinimumDate`](xref:Xamarin.Forms.DatePicker.MinimumDate) Der Typ [ `DateTime` ](xref:System.DateTime), die standardmäßig auf den ersten Tag des Jahres 1900.
- [`MaximumDate`](xref:Xamarin.Forms.DatePicker.MaximumDate) Der Typ `DateTime`, die den Standardwert bis zum letzten Tag des Jahres 2100.
- [`Date`](xref:Xamarin.Forms.DatePicker.Date) Der Typ `DateTime`, die dem ausgewählten Datum ein, der Wert standardmäßig [ `DateTime.Today` ](xref:System.DateTime.Today).
- [`Format`](xref:Xamarin.Forms.DatePicker.Format) Der Typ `string`, eine [standard](/dotnet/standard/base-types/standard-date-and-time-format-strings/) oder [benutzerdefinierte](/dotnet/standard/base-types/custom-date-and-time-format-strings/) .NET Formatierung der Zeichenfolge, die von "D Standardeinstellungen" das lange Datumsmuster.
- [`TextColor`](xref:Xamarin.Forms.DatePicker.TextColor) Der Typ [ `Color` ](xref:Xamarin.Forms.Color), die Farbe, mit das ausgewählten Datum wird angezeigt, deren Standard [ `Color.Default` ](xref:Xamarin.Forms.Color.Default).
- [`FontAttributes`](xref:Xamarin.Forms.DatePicker.FontAttributes) Der Typ [ `FontAttributes` ](xref:Xamarin.Forms.FontAttributes), die standardmäßig auf [ `FontAtributes.None` ](xref:Xamarin.Forms.FontAttributes.None).
- [`FontFamily`](xref:Xamarin.Forms.DatePicker.FontFamily) Der Typ `string`, die standardmäßig auf `null`.
- [`FontSize`](xref:Xamarin.Forms.DatePicker.FontSize) Der Typ `double`, die standardmäßig auf den Bereich von -1,0.

Die `DatePicker` löst eine [ `DateSelected` ](xref:Xamarin.Forms.DatePicker.DateSelected) Ereignis aus, wenn der Benutzer ein Datum auswählt.

> [!WARNING]
> Beim Festlegen `MinimumDate` und `MaximumDate`, stellen Sie sicher, dass `MinimumDate` ist immer kleiner als oder gleich `MaximumDate`. Andernfalls `DatePicker` wird eine Ausnahme ausgelöst.

Intern wird die `DatePicker` wird sichergestellt, dass `Date` zwischen `MinimumDate` und `MaximumDate`(inklusiv) enthalten. Wenn `MinimumDate` oder `MaximumDate` festgelegt ist, damit `Date` ist nicht zwischen den beiden `DatePicker` passen den Wert der `Date`.

Alle acht Eigenschaften verfügen über [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) -Objekte, die bedeutet, dass sie die formatiert werden können, und die Eigenschaften können Ziele von datenbindungen. Die `Date` Eigenschaft verfügt über einen Standardmodus für die Bindung der [ `BindingMode.TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay), was bedeutet, dass es ein Ziel einer Bindung in einer Anwendung sein kann, verwendet der [Model-View-ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) Architektur.

## <a name="initializing-the-datetime-properties"></a>Initialisieren der DateTime-Eigenschaften

Sie können im Code Initialisieren der `MinimumDate`, `MaximumDate`, und `Date` Eigenschaften, die Werte des Typs `DateTime`:

```csharp
DatePicker datePicker = new DatePicker
{
    MinimumDate = new DateTime(2018, 1, 1),
    MaximumDate = new DateTime(2018, 12, 31),
    Date = new DateTime(2018, 6, 21)
};
```

Bei der eine `DateTime` in XAML, Wert angegeben ist, der XAML-Parser verwendet die `DateTime.Parse` -Methode mit einer `CultureInfo.InvariantCulture` Argument, um die Zeichenfolge zu konvertieren eine `DateTime` Wert. Die Datumsangaben in einem präzisen Format angegeben werden: zweistellige Monate, Tage mit zwei Ziffern und vierstellige Jahresangaben durch Schrägstriche getrennte:

```xaml
<DatePicker MinimumDate="01/01/2018"
            MaximumDate="12/31/2018"
            Date="06/21/2018" />
```

Wenn die `BindingContext` Eigenschaft `DatePicker` festgelegt ist, mit einer Instanz eines "ViewModels" mit Eigenschaften vom Typ `DateTime` mit dem Namen `MinDate`, `MaxDate`, und `SelectedDate` (z. B.), können Sie instanziieren die `DatePicker` wie folgt :

```xaml
<DatePicker MinimumDate="{Binding MinDate}"
            MaximumDate="{Binding MaxDate}"
            Date="{Binding SelectedDate}" />
```

In diesem Beispiel werden alle drei Eigenschaften mit den entsprechenden Eigenschaften in "ViewModel" initialisiert. Da die `Date` Eigenschaft verfügt über eine Bindungsmodus der `TwoWay`, jedes neue Datum, das der Benutzer wählt automatisch in "ViewModel" wiedergegeben wird.

Wenn die `DatePicker` enthält keine Bindung auf seine `Date` -Eigenschaft muss eine Anwendung einen Handler zum Anfügen der `DateSelected` Ereignis darüber informiert, wenn der Benutzer ein neues Datum auswählt.

Informationen zum Festlegen von Schriftart-Eigenschaften finden Sie unter [Schriftarten](~/xamarin-forms/user-interface/text/fonts.md).

## <a name="datepicker-and-layout"></a>DatePicker und layout

Es ist möglich, eine uneingeschränkte horizontales Layout-Option verwenden, z. B. `Center`, `Start`, oder `End` mit `DatePicker`:

```xaml
<DatePicker ···
            HorizontalOptions="Center"
            ··· />
```

Dies wird jedoch nicht empfohlen. Entsprechend der Einstellung von der `Format` Eigenschaft, die ausgewählten Datumsangaben müssen möglicherweise andere breiten. Die Formatzeichenfolge "D" beispielsweise bewirkt, dass `DateTime` zum Anzeigen von Datumsangaben in einem Langformat und "Mittwoch, 12. September 2018" erfordert eine größere Anzeigebreite als "Freitag, 4. Mai 2018". Abhängig von der Plattform, dieser Unterschied kann dazu führen, dass die `DateTime` Ansicht so ändern Sie die Breite in Layout oder für die Anzeige abgeschnitten werden.

> [!TIP]
> Es wird empfohlen, die Standardeinstellung verwenden `HorizontalOptions` -Einstellung `Fill` mit `DatePicker`, und nicht mit einer Breite von `Auto` beim `DatePicker` in einer `Grid` Zelle.

## <a name="datepicker-in-an-application"></a>"DatePicker" in einer Anwendung

Die [ **DaysBetweenDates** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/DatePicker) Beispiel enthält zwei `DatePicker` Ansichten auf der Seite. Diese können auf zwei Datumsangaben, die verwendet werden, und die Anwendung berechnet die Anzahl der Tage zwischen diesen Daten. Das Programm nicht die Einstellungen des Ändern der `MinimumDate` und `MaximumDate` Eigenschaften, sodass zwei Datumsangaben zwischen 1900 und 2100 sein müssen.

Hier ist die XAML-Datei ein:

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

Jede `DatePicker` erhält eine `Format` Eigenschaft von "D" für einen langen Datumsformat. Beachten Sie auch, dass die `endDatePicker` Objekt besitzt eine Bindung, dessen Ziel die `MinimumDate` Eigenschaft. Die Bindungsquelle ist auf dem ausgewählten `Date` Eigenschaft der `startDatePicker` Objekt. Dadurch wird sichergestellt, dass das Enddatum immer zu einem späteren Zeitpunkt als oder gleich dem Startdatum ist. Zusätzlich zu den beiden `DatePicker` Objekte, eine `Switch` ist mit der Bezeichnung "Include beiden Tagen insgesamt".

Die beiden `DatePicker` Ansichten verfügen über Handler angefügt der `DateSelected` Ereignis und die `Switch` wurde ein Handler angefügt dessen `Toggled` Ereignis. Diese Ereignishandler sind in der CodeBehind-Datei und eine neue Berechnung der Tage zwischen den beiden Datumsangaben auslösen:

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

Wenn das Beispiel zuerst ausgeführt wird, sowohl `DatePicker` Ansichten werden initialisiert, das heutige Datum. Der folgende Screenshot zeigt das Programm unter iOS, Android und die universelle Windows-Plattform ausgeführt wird:

[![Starten Sie die Tage zwischen Datumsangaben](datepicker-images/DaysBetweenDatesStart.png "Tage zwischen Datumsangaben starten")](datepicker-images/DaysBetweenDatesStart-Large.png#lightbox "Tage zwischen Datumsangaben starten")

Tippen Sie auf eines der `DatePicker` zeigt Ruft die Datumsauswahl Plattform. Drei Plattformen Implementieren dieser Datumsauswahl sehr unterschiedlich, aber jeder Ansatz ist für Benutzer von dieser Plattform vertraut sind:

[![Wählen Sie Tage zwischen Datumsangaben](datepicker-images/DaysBetweenDatesSelect.png "Tage zwischen Datumsangaben auswählen")](datepicker-images/DaysBetweenDatesSelect-Large.png#lightbox "Tage zwischen Datumsangaben auswählen")

> [!TIP]
> Unter Android die `DatePicker` Dialogfeld kann angepasst werden, durch Überschreiben der `CreateDatePickerDialog` -Methode in der ein benutzerdefinierter Renderer. Dies kann beispielsweise zusätzliche Schaltflächen im Dialogfeld hinzugefügt werden.

Nachdem zwei Datumsangaben, die ausgewählt werden, zeigt die Anwendung die Anzahl der Tage zwischen diesen Datumswerten:

[![Tage zwischen Datumsangaben Ergebnis](datepicker-images/DaysBetweenDatesResult.png "Tage zwischen Datumsangaben Ergebnis")](datepicker-images/DaysBetweenDatesResult-Large.png#lightbox "Tage zwischen dem Ergebnis von Datumsangaben")

## <a name="related-links"></a>Verwandte Links

- [DaysBetweenDates-Beispiel](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/DatePicker)
- [DatePicker-API](xref:Xamarin.Forms.DatePicker)
