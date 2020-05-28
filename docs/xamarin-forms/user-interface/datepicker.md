---
title: Xamarin.FormsDatePicker
description: Der DatePicker ist eine Xamarin.Forms Ansicht, mit der der Benutzer ein Datum auswählen kann. In diesem Artikel wird erläutert, wie ein DatePicker in einer-Anwendung verwendet wird Xamarin.Forms .
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5c1de52e2a173e7d9a366d8fd7cbd63998b3a6d1
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137357"
---
# <a name="xamarinforms-datepicker"></a>Xamarin.FormsDatePicker

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-datepicker)

_Eine Xamarin.Forms Ansicht, mit der der Benutzer ein Datum auswählen kann._

Xamarin.Forms [`DatePicker`](xref:Xamarin.Forms.DatePicker) Ruft das Steuerelement für die Datumsauswahl der Plattform auf und ermöglicht es dem Benutzer, ein Datum auszuwählen. `DatePicker`definiert acht Eigenschaften:

- [`MinimumDate`](xref:Xamarin.Forms.DatePicker.MinimumDate)vom Typ [`DateTime`](xref:System.DateTime) , der standardmäßig auf den ersten Tag des Jahres 1900 festgelegt ist.
- [`MaximumDate`](xref:Xamarin.Forms.DatePicker.MaximumDate)vom Typ `DateTime` , der standardmäßig auf den letzten Tag des Jahres 2100 festgelegt ist.
- [`Date`](xref:Xamarin.Forms.DatePicker.Date)vom Typ `DateTime` , das ausgewählte Datum, das standardmäßig auf den Wert festgelegt ist [`DateTime.Today`](xref:System.DateTime.Today) .
- [`Format`](xref:Xamarin.Forms.DatePicker.Format)vom Typ `string` , einer [standardmäßigen](/dotnet/standard/base-types/standard-date-and-time-format-strings/) oder [benutzerdefinierten](/dotnet/standard/base-types/custom-date-and-time-format-strings/) .net-Formatierungs Zeichenfolge, deren Standardwert "D" ist, dem langen Datums Muster.
- [`TextColor`](xref:Xamarin.Forms.DatePicker.TextColor)der Typ [`Color`](xref:Xamarin.Forms.Color) , in dem das ausgewählte Datum angezeigt wird, dessen Standard ist [`Color.Default`](xref:Xamarin.Forms.Color.Default) .
- [`FontAttributes`](xref:Xamarin.Forms.DatePicker.FontAttributes)vom Typ [`FontAttributes`](xref:Xamarin.Forms.FontAttributes) , der standardmäßig auf festgelegt ist [`FontAtributes.None`](xref:Xamarin.Forms.FontAttributes.None) .
- [`FontFamily`](xref:Xamarin.Forms.DatePicker.FontFamily)vom Typ `string` , der standardmäßig auf festgelegt ist `null` .
- [`FontSize`](xref:Xamarin.Forms.DatePicker.FontSize)vom Typ `double` , der standardmäßig auf-1,0 festgelegt ist.
- `CharacterSpacing` vom Typ `double`: Abstand zwischen den Zeichen des `DatePicker`-Texts.

Das löst ein-Ereignis aus, `DatePicker` [`DateSelected`](xref:Xamarin.Forms.DatePicker.DateSelected) Wenn der Benutzer ein Datum auswählt.

> [!WARNING]
> Wenn `MinimumDate` Sie und festlegen `MaximumDate` , stellen Sie sicher, dass `MinimumDate` immer kleiner oder gleich ist `MaximumDate` . Andernfalls `DatePicker` wird von eine Ausnahme ausgelöst.

Intern `DatePicker` stellt sicher, dass `Date` zwischen und (einschließlich) liegt `MinimumDate` `MaximumDate` . Wenn `MinimumDate` oder `MaximumDate` festgelegt ist, sodass `Date` nicht dazwischen liegt, `DatePicker` wird der Wert von angepasst `Date` .

Alle acht Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie formatiert werden können, und die Eigenschaften können Ziele von Daten Bindungen sein. Die- `Date` Eigenschaft verfügt über einen Standard Bindungs Modus von [`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) . Dies bedeutet, dass Sie ein Ziel einer Datenbindung in einer Anwendung sein kann, die die [Model-View-ViewModel (MVVM)-](~/xamarin-forms/enterprise-application-patterns/mvvm.md) Architektur verwendet.

## <a name="initializing-the-datetime-properties"></a>Initialisieren der DateTime-Eigenschaften

Im Code können Sie die `MinimumDate` `MaximumDate` Eigenschaften, und `Date` mit Werten des Typs initialisieren `DateTime` :

```csharp
DatePicker datePicker = new DatePicker
{
    MinimumDate = new DateTime(2018, 1, 1),
    MaximumDate = new DateTime(2018, 12, 31),
    Date = new DateTime(2018, 6, 21)
};
```

Wenn ein `DateTime` Wert in XAML angegeben wird, verwendet der XAML-Parser die- `DateTime.Parse` Methode mit einem- `CultureInfo.InvariantCulture` Argument, um die Zeichenfolge in einen-Wert zu konvertieren `DateTime` . Die Datumsangaben müssen in einem präzisen Format angegeben werden: zweistellige Monate, zweistellige Tage und vierstellige Jahre, die durch Schrägstriche voneinander getrennt sind:

```xaml
<DatePicker MinimumDate="01/01/2018"
            MaximumDate="12/31/2018"
            Date="06/21/2018" />
```

Wenn die- `BindingContext` Eigenschaft von `DatePicker` auf eine Instanz von ViewModel festgelegt wird, die Eigenschaften des Typs mit dem `DateTime` Namen, und enthält (z. b. `MinDate` `MaxDate` `SelectedDate` ), können Sie das wie folgt instanziieren `DatePicker` :

```xaml
<DatePicker MinimumDate="{Binding MinDate}"
            MaximumDate="{Binding MaxDate}"
            Date="{Binding SelectedDate}" />
```

In diesem Beispiel werden alle drei Eigenschaften mit den entsprechenden Eigenschaften in "ViewModel" initialisiert. Da die- `Date` Eigenschaft den Bindungs Modus hat `TwoWay` , werden alle neuen Datumsangaben, die der Benutzer auswählt, automatisch im ViewModel reflektiert.

Wenn das `DatePicker` keine Bindung für seine-Eigenschaft enthält `Date` , sollte eine Anwendung einen Handler an das-Ereignis anfügen, `DateSelected` um informiert zu werden, wenn der Benutzer ein neues Datum auswählt.

Weitere Informationen zum Festlegen von Schriftart Eigenschaften finden Sie unter [Schriftarten](~/xamarin-forms/user-interface/text/fonts.md).

## <a name="datepicker-and-layout"></a>DatePicker und Layout

Es ist möglich, eine nicht eingeschränkte horizontale Layoutoption wie `Center` , `Start` oder `End` mit zu verwenden `DatePicker` :

```xaml
<DatePicker ···
            HorizontalOptions="Center"
            ··· />
```

Dies wird jedoch nicht empfohlen. Abhängig von der Einstellung der `Format` Eigenschaft werden für ausgewählte Datumsangaben möglicherweise unterschiedliche Anzeige breiten benötigt. Die Format Zeichenfolge "D" bewirkt z `DateTime` . b., dass Datumsangaben in einem langen Format angezeigt werden und "Mittwoch, der 12. September 2018" eine höhere Anzeigebreite als "Freitag, 4. Mai 2018" erfordert. Abhängig von der Plattform kann dieser Unterschied bewirken, `DateTime` dass die Ansicht die Breite im Layout ändert oder die Anzeige abgeschnitten wird.

> [!TIP]
> Es empfiehlt sich, die Standard `HorizontalOptions` Einstellung von mit zu verwenden `Fill` `DatePicker` und keine Breite von `Auto` beim Einfügen `DatePicker` in eine Zelle zu verwenden `Grid` .

## <a name="datepicker-in-an-application"></a>DatePicker in einer Anwendung

Das [**daysbetwetendates**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-datepicker) -Beispiel enthält zwei `DatePicker` Ansichten auf der Seite. Diese können verwendet werden, um zwei Datumsangaben auszuwählen, und das Programm berechnet die Anzahl der Tage zwischen diesen Datumsangaben. Das Programm ändert nicht die Einstellungen der `MinimumDate` -Eigenschaft und der-Eigenschaft `MaximumDate` , sodass die beiden Datumsangaben zwischen 1900 und 2100 liegen müssen.

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

Jedem `DatePicker` wird die `Format` Eigenschaft "D" für ein langes Datumsformat zugewiesen. Beachten Sie auch, dass das- `endDatePicker` Objekt über eine Bindung verfügt, die auf seine- `MinimumDate` Eigenschaft abzielt Die Bindungs Quelle ist die ausgewählte `Date` Eigenschaft des `startDatePicker` Objekts. Dadurch wird sichergestellt, dass das Enddatum immer später oder gleich dem Startdatum ist. Zusätzlich zu den beiden- `DatePicker` Objekten wird eine mit der `Switch` Bezeichnung "include both days in Total" bezeichnet.

Die beiden `DatePicker` Ansichten verfügen über Handler, die an das Ereignis angefügt sind `DateSelected` `Switch` `Toggled` Diese Ereignishandler befinden sich in der Code Behind-Datei und initiieren eine neue Berechnung der Tage zwischen den beiden Datumsangaben:

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

Wenn das Beispiel zum ersten Mal ausgeführt wird, `DatePicker` werden beide Sichten mit dem heutigen Datum initialisiert. Der folgende Screenshot zeigt das Programm, das unter IOS und Android ausgeführt wird:

[![Tage zwischen Datumsangaben](datepicker-images/DaysBetweenDatesStart.png "Tage zwischen Datumsangaben")](datepicker-images/DaysBetweenDatesStart-Large.png#lightbox "Tage zwischen Datumsangaben")

Wenn Sie auf eines der angezeigten anzeigen tippen, wird `DatePicker` die Platt Form Datumsauswahl aufgerufen Diese Datumsauswahl wird von den Plattformen auf sehr unterschiedliche Weise implementiert, aber die einzelnen Ansätze sind den Benutzern dieser Plattform vertraut:

[![Tage zwischen Datumsangaben auswählen](datepicker-images/DaysBetweenDatesSelect.png "Tage zwischen Datumsangaben auswählen")](datepicker-images/DaysBetweenDatesSelect-Large.png#lightbox "Tage zwischen Datumsangaben auswählen")

> [!TIP]
> Unter Android kann das `DatePicker` Dialogfeld angepasst werden, indem die- `CreateDatePickerDialog` Methode in einem benutzerdefinierten Renderer überschrieben wird. Dies ermöglicht z. b. das Hinzufügen zusätzlicher Schaltflächen zum Dialogfeld.

Nachdem zwei Datumsangaben ausgewählt wurden, zeigt die Anwendung die Anzahl der Tage zwischen diesen Datumsangaben an:

[![Tage zwischen Datumsangaben](datepicker-images/DaysBetweenDatesResult.png "Tage zwischen Datumsangaben")](datepicker-images/DaysBetweenDatesResult-Large.png#lightbox "Tage zwischen Datumsangaben")

## <a name="related-links"></a>Verwandte Links

- [Daysbetweumdates-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-datepicker)
- [DatePicker-API](xref:Xamarin.Forms.DatePicker)
