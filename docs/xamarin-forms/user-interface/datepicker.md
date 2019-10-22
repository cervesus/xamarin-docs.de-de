---
title: Xamarin. Forms DatePicker
description: DatePicker ist eine xamarin. Forms-Ansicht, mit der der Benutzer ein Datum auswählen kann. In diesem Artikel wird erläutert, wie ein DatePicker in einer xamarin. Forms-Anwendung verwendet wird.
ms.prod: xamarin
ms.assetid: 68E8EF8A-42E7-4939-8ABE-64D060E609D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/04/2018
ms.openlocfilehash: 15d4159d89a463c0335d9c91b24b55151c91de8c
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72695919"
---
# <a name="xamarinforms-datepicker"></a>Xamarin. Forms DatePicker

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-datepicker)

_Eine xamarin. Forms-Ansicht, mit der der Benutzer ein Datum auswählen kann._

Der xamarin. Forms- [`DatePicker`](xref:Xamarin.Forms.DatePicker) Ruft das Steuerelement für die Datumsauswahl der Plattform auf und ermöglicht dem Benutzer die Auswahl eines Datums. in `DatePicker` werden acht Eigenschaften definiert:

- [`MinimumDate`](xref:Xamarin.Forms.DatePicker.MinimumDate) vom Typ [`DateTime`](xref:System.DateTime), der standardmäßig auf den ersten Tag des Jahres 1900 festgelegt ist.
- [`MaximumDate`](xref:Xamarin.Forms.DatePicker.MaximumDate) vom Typ `DateTime`, der standardmäßig auf den letzten Tag des Jahres 2100 festgelegt ist.
- [`Date`](xref:Xamarin.Forms.DatePicker.Date) vom Typ `DateTime`, das ausgewählte Datum, das standardmäßig auf den Wert [`DateTime.Today`](xref:System.DateTime.Today)festgelegt ist.
- [`Format`](xref:Xamarin.Forms.DatePicker.Format) vom Typ `string`, eine [standardmäßige](/dotnet/standard/base-types/standard-date-and-time-format-strings/) oder [benutzerdefinierte](/dotnet/standard/base-types/custom-date-and-time-format-strings/) .net-Formatierungs Zeichenfolge, deren Standardwert "D" ist, das lange Datums Muster.
- [`TextColor`](xref:Xamarin.Forms.DatePicker.TextColor) vom Typ [`Color`](xref:Xamarin.Forms.Color), die Farbe, die zum Anzeigen des ausgewählten Datums verwendet wird, dessen Standardwert [`Color.Default`](xref:Xamarin.Forms.Color.Default)ist.
- [`FontAttributes`](xref:Xamarin.Forms.DatePicker.FontAttributes) vom Typ [`FontAttributes`](xref:Xamarin.Forms.FontAttributes), dessen standardmäßig [`FontAtributes.None`](xref:Xamarin.Forms.FontAttributes.None)ist.
- [`FontFamily`](xref:Xamarin.Forms.DatePicker.FontFamily) vom Typ `string`, dessen standardmäßig `null` ist.
- [`FontSize`](xref:Xamarin.Forms.DatePicker.FontSize) vom Typ `double`, der standardmäßig auf-1,0 festgelegt ist.
- `CharacterSpacing` vom Typ `double` ist der Abstand zwischen den Zeichen des `DatePicker` Texts.

Der `DatePicker` löst ein [`DateSelected`](xref:Xamarin.Forms.DatePicker.DateSelected) Ereignis aus, wenn der Benutzer ein Datum auswählt.

> [!WARNING]
> Wenn Sie `MinimumDate` und `MaximumDate` festlegen, stellen Sie sicher, dass `MinimumDate` immer kleiner oder gleich `MaximumDate` ist. Andernfalls wird von `DatePicker` eine Ausnahme ausgelöst.

Intern stellt die `DatePicker` sicher, dass `Date` zwischen `MinimumDate` und `MaximumDate` (einschließlich) liegt. Wenn `MinimumDate` oder `MaximumDate` festgelegt ist, sodass `Date` nicht dazwischen liegt, wird der Wert `Date` von `DatePicker` angepasst.

Alle acht Eigenschaften werden von [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Objekten gestützt, was bedeutet, dass Sie formatiert werden können, und die Eigenschaften können Ziele von Daten Bindungen sein. Die `Date`-Eigenschaft verfügt über einen Standard Bindungs Modus von [`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay). Dies bedeutet, dass Sie ein Ziel einer Datenbindung in einer Anwendung sein kann, die die [Model-View-ViewModel (MVVM)-](~/xamarin-forms/enterprise-application-patterns/mvvm.md) Architektur verwendet.

## <a name="initializing-the-datetime-properties"></a>Initialisieren der DateTime-Eigenschaften

Im Code können Sie die Eigenschaften `MinimumDate`, `MaximumDate` und `Date` mit Werten des Typs `DateTime` initialisieren:

```csharp
DatePicker datePicker = new DatePicker
{
    MinimumDate = new DateTime(2018, 1, 1),
    MaximumDate = new DateTime(2018, 12, 31),
    Date = new DateTime(2018, 6, 21)
};
```

Wenn ein `DateTime` Wert in XAML angegeben wird, verwendet der XAML-Parser die `DateTime.Parse`-Methode mit einem `CultureInfo.InvariantCulture`-Argument, um die Zeichenfolge in einen `DateTime` Wert zu konvertieren. Die Datumsangaben müssen in einem präzisen Format angegeben werden: zweistellige Monate, zweistellige Tage und vierstellige Jahre, die durch Schrägstriche voneinander getrennt sind:

```xaml
<DatePicker MinimumDate="01/01/2018"
            MaximumDate="12/31/2018"
            Date="06/21/2018" />
```

Wenn die `BindingContext`-Eigenschaft von `DatePicker` auf eine Instanz von ViewModel festgelegt ist, die Eigenschaften des Typs `DateTime` namens `MinDate`, `MaxDate` und `SelectedDate` enthält (z. b.), können Sie die `DatePicker` wie folgt instanziieren. :

```xaml
<DatePicker MinimumDate="{Binding MinDate}"
            MaximumDate="{Binding MaxDate}"
            Date="{Binding SelectedDate}" />
```

In diesem Beispiel werden alle drei Eigenschaften mit den entsprechenden Eigenschaften in "ViewModel" initialisiert. Da die `Date`-Eigenschaft über den Bindungs Modus `TwoWay` verfügt, werden alle neuen Datumsangaben, die der Benutzer auswählt, automatisch in ViewModel widergespiegelt.

Wenn die `DatePicker` keine Bindung für die `Date`-Eigenschaft enthält, sollte eine Anwendung einen Handler an das `DateSelected` Ereignis anfügen, um informiert zu werden, wenn der Benutzer ein neues Datum auswählt.

Weitere Informationen zum Festlegen von Schriftart Eigenschaften finden Sie unter [Schriftarten](~/xamarin-forms/user-interface/text/fonts.md).

## <a name="datepicker-and-layout"></a>DatePicker und Layout

Es ist möglich, eine nicht eingeschränkte horizontale Layoutoption wie z. b. `Center`, `Start` oder `End` mit `DatePicker` zu verwenden:

```xaml
<DatePicker ···
            HorizontalOptions="Center"
            ··· />
```

Dies wird jedoch nicht empfohlen. Abhängig von der Einstellung der `Format` Eigenschaft werden für ausgewählte Datumsangaben möglicherweise andere Anzeigebreite benötigt. Die Format Zeichenfolge "D" bewirkt z. b., dass `DateTime` Datumsangaben in einem langen Format anzeigt, und "Mittwoch, der 12. September 2018" erfordert eine höhere Anzeigebreite als "Friday, 4. Mai 2018". Abhängig von der Plattform kann dieser Unterschied dazu führen, dass die `DateTime` Ansicht die Breite im Layout ändert oder die Anzeige abgeschnitten wird.

> [!TIP]
> Es empfiehlt sich, die Standardeinstellung für die `HorizontalOptions` `Fill` mit `DatePicker` zu verwenden und keine Breite von `Auto` zu verwenden, wenn `DatePicker` in eine `Grid` Zelle versetzt wird.

## <a name="datepicker-in-an-application"></a>DatePicker in einer Anwendung

Das [**daysbetwetendates**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-datepicker) -Beispiel enthält zwei `DatePicker` Ansichten auf der Seite. Diese können verwendet werden, um zwei Datumsangaben auszuwählen, und das Programm berechnet die Anzahl der Tage zwischen diesen Datumsangaben. Das Programm ändert nicht die Einstellungen der Eigenschaften "`MinimumDate`" und "`MaximumDate`", sodass die beiden Datumsangaben zwischen 1900 und 2100 liegen müssen.

Hier ist die XAML-Datei:

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

Jedem `DatePicker` wird eine `Format` Eigenschaft "D" für ein langes Datumsformat zugewiesen. Beachten Sie auch, dass das `endDatePicker` Objekt über eine Bindung verfügt, die seine `MinimumDate`-Eigenschaft als Ziel hat. Die Bindungs Quelle ist die ausgewählte `Date` Eigenschaft des `startDatePicker` Objekts. Dadurch wird sichergestellt, dass das Enddatum immer später oder gleich dem Startdatum ist. Zusätzlich zu den beiden `DatePicker` Objekten wird ein `Switch` mit der Bezeichnung "include both days in Total" bezeichnet.

Die beiden `DatePicker` Ansichten verfügen über Handler, die an das `DateSelected`-Ereignis angefügt sind, und der `Switch` verfügt über einen Handler, der mit seinem `Toggled` Ereignis verbunden ist. Diese Ereignishandler befinden sich in der Code Behind-Datei und initiieren eine neue Berechnung der Tage zwischen den beiden Datumsangaben:

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

Wenn das Beispiel zum ersten Mal ausgeführt wird, werden beide `DatePicker` Sichten mit dem heutigen Datum initialisiert. Der folgende Screenshot zeigt das Programm, das unter IOS, Android und der universelle Windows-Plattform ausgeführt wird:

[![Tage zwischen Datumsangaben](datepicker-images/DaysBetweenDatesStart.png "Tage zwischen Datumsangaben")](datepicker-images/DaysBetweenDatesStart-Large.png#lightbox "Tage zwischen Datumsangaben")

Wenn Sie auf einen der `DatePicker` anzeigen tippen, wird die Platt Form Datumsauswahl aufgerufen. Diese Datumsauswahl wird von den Plattformen auf sehr unterschiedliche Weise implementiert, aber die einzelnen Ansätze sind den Benutzern dieser Plattform vertraut:

[![Tage zwischen Datumsangaben auswählen](datepicker-images/DaysBetweenDatesSelect.png "Tage zwischen Datumsangaben auswählen")](datepicker-images/DaysBetweenDatesSelect-Large.png#lightbox "Tage zwischen Datumsangaben auswählen")

> [!TIP]
> Unter Android kann das Dialogfeld "`DatePicker`" angepasst werden, indem die `CreateDatePickerDialog`-Methode in einem benutzerdefinierten Renderer überschrieben wird. Dies ermöglicht z. b. das Hinzufügen zusätzlicher Schaltflächen zum Dialogfeld.

Nachdem zwei Datumsangaben ausgewählt wurden, zeigt die Anwendung die Anzahl der Tage zwischen diesen Datumsangaben an:

[![Tage zwischen Datumsangaben](datepicker-images/DaysBetweenDatesResult.png "Tage zwischen Datumsangaben")](datepicker-images/DaysBetweenDatesResult-Large.png#lightbox "Tage zwischen Datumsangaben")

## <a name="related-links"></a>Verwandte Links

- [Daysbetweumdates-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-datepicker)
- [DatePicker-API](xref:Xamarin.Forms.DatePicker)
