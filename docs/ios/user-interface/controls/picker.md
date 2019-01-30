---
title: Datumsauswahl-Steuerelement in Xamarin.iOS
description: Dieses Dokument beschreibt, wie Sie entwerfen und Arbeiten mit Picker-Steuerelemente in einer Xamarin.iOS-app. Es wird erläutert, wie eine Auswahl im Code und in der iOS-Designer implementieren.
ms.prod: xamarin
ms.assetid: A2369EFC-285A-44DD-9E80-EC65BC3DF041
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/14/2018
ms.openlocfilehash: 525ddf3c8cfc457738099c3afbb162fd3fb9239b
ms.sourcegitcommit: a1a58afea68912c79d16a3f64de9a0c1feb2aeb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55233574"
---
# <a name="picker-control-in-xamarinios"></a>Datumsauswahl-Steuerelement in Xamarin.iOS

Ein [ `UIPickerView` ](xref:UIKit.UIPickerView) ermöglicht es, einen Wert aus einer Liste auszuwählen, indem Sie einen Bildlauf der einzelne Komponenten einer RAD-ähnlichen Schnittstelle.

Datumsauswahl werden häufig verwendet, um ein Datum und eine Uhrzeit auszuwählen; Apple bietet die [`UIDatePicker`](xref:UIKit.UIDatePicker)
die Klasse für diesen Zweck.

Der Artikel beschreibt die Implementierung und Verwendung der `UIPickerView` und `UIDatePicker` Steuerelemente.

## <a name="uipickerview"></a>UIPickerView

### <a name="implementing-a-picker"></a>Implementieren einer Auswahl

Implementieren Sie eine Auswahl durch Instanziierung eines neuen `UIPickerView`:

```csharp
UIPickerView pickerView = new UIPickerView(
    new CGRect(
        UIScreen.MainScreen.Bounds.X - UIScreen.MainScreen.Bounds.Width, 
        UIScreen.MainScreen.Bounds.Height - 230,
        UIScreen.MainScreen.Bounds.Width,
        180
    )
);
```

### <a name="pickers-and-storyboards"></a>Auswahltools und storyboards

Erstellen Sie eine Auswahl in der **iOS-Designer**, ziehen Sie eine **Auswahl anzeigen** aus der **Toolbox** auf die Entwurfsoberfläche.

![Ziehen Sie eine Ansicht für die Auswahl auf die Entwurfsoberfläche](picker-images/image1.png "eine Datumsauswahl-Ansicht auf die Entwurfsoberfläche ziehen.")

### <a name="working-with-a-picker-control"></a>Arbeiten mit einer Datumsauswahl-Steuerelement

Eine Auswahl verwendet eine _Modell_ für die Interaktion mit Daten:

```csharp
public override void ViewDidLoad()
{
    base.ViewDidLoad();
    var pickerModel = new PeopleModel(personLabel);
    personPicker.Model = pickerModel;
}
```

Die [ `UIPickerViewModel` ](xref:UIKit.UIPickerViewModel) Basisklasse implementiert zwei Schnittstellen, [`IUIPickerDataSource`](xref:UIKit.IUIPickerViewDataSource)
und [ `IUIPickerViewDelegate` ](xref:UIKit.IUIPickerViewDelegate), die verschiedenen Methoden zu deklarieren, die Daten mit einer Auswahl angeben, und wie diese Interaktion behandelt:

```csharp
public class PeopleModel : UIPickerViewModel
{
    public string[] names = new string[] {
            "Amy Burns",
            "Kevin Mullins",
            "Craig Dunn",
            "Joel Martinez",
            "Charles Petzold",
            "David Britch",
            "Mark McLemore",
            "Tom Opegenorth",
            "Joseph Hill",
            "Miguel De Icaza"
        };

    private UILabel personLabel;

    public PeopleModel(UILabel personLabel)
    {
        this.personLabel = personLabel;
    }

    public override nint GetComponentCount(UIPickerView pickerView)
    {
        return 2;
    }

    public override nint GetRowsInComponent(UIPickerView pickerView, nint component)
    {
        return names.Length;
    }

    public override string GetTitle(UIPickerView pickerView, nint row, nint component)
    {
        if (component == 0)
            return names[row];
        else
            return row.ToString();
    }

    public override void Selected(UIPickerView pickerView, nint row, nint component)
    {
        personLabel.Text = $"This person is: {names[pickerView.SelectedRowInComponent(0)]},\n they are number {pickerView.SelectedRowInComponent(1)}";
    }

    public override nfloat GetComponentWidth(UIPickerView picker, nint component)
    {
        if (component == 0)
            return 240f;
        else
            return 40f;
    }

    public override nfloat GetRowHeight(UIPickerView picker, nint component)
    {
        return 40f;
    }
```

Eine Auswahl haben mehrere Spalten oder _Komponenten_. Komponenten Partitionieren Sie eine Auswahl in mehrere Abschnitte, um einfachere und genauere Daten ausgewählt werden kann:

![Auswahl mit zwei Komponenten](picker-images/image3.png "Farbauswahl mit zwei Komponenten")

Verwenden Sie zum Angeben der Anzahl der Komponenten in einer Auswahl der [`GetComponentCount`](xref:UIKit.UIPickerViewModel.GetComponentCount(UIKit.UIPickerView)) 
-Methode.

### <a name="customizing-a-pickers-appearance"></a>Anpassen der Darstellung von einer Auswahl

Verwenden Sie zum Anpassen der Darstellung einer Auswahl der [`UIPickerView.UIPickerViewAppearance`](https://developer.xamarin.com/api/type/UIKit.UIPickerView+UIPickerViewAppearance/)
Klasse oder außer Kraft setzen der [ `GetView` ](xref:UIKit.UIPickerViewModel.GetView(UIKit.UIPickerView,System.nint,System.nint,UIKit.UIView)) und [ `GetRowHeight` ](xref:UIKit.UIPickerViewModel.GetRowHeight(UIKit.UIPickerView,System.nint)) Methoden in der `UIPickerViewModel`.

## <a name="uidatepicker"></a>UIDatePicker

### <a name="implementing-a-date-picker"></a>Implementieren einer Datumsauswahl

Implementieren eine Datumsauswahl durch Instanziieren einer `UIDatePicker`:

```csharp
UIPickerView pickerView = new UIPickerView(
    new CGRect(
        UIScreen.MainScreen.Bounds.X - UIScreen.MainScreen.Bounds.Width,
        UIScreen.MainScreen.Bounds.Height - 230,
        UIScreen.MainScreen.Bounds.Width,
        180
     )
);
```

### <a name="date-pickers-and-storyboards"></a>Datumsauswahl und storyboards

Zum Erstellen einer Datumsauswahl in die **iOS-Designer**, ziehen Sie eine **Datumsauswahl** aus der **Toolbox** auf die Entwurfsoberfläche.

![Ziehen Sie eine Datumsauswahl auf der Entwurfsoberfläche](picker-images/image2.png "einer Datumsauswahl auf die Entwurfsoberfläche ziehen")

### <a name="date-picker-properties"></a>Eigenschaften für Datumsauswahl

#### <a name="minimum-and-maximum-date"></a>Minimale und maximale Datum

[`MinimumDate`](xref:UIKit.UIDatePicker.MinimumDate) und [ `MaximumDate` ](xref:UIKit.UIDatePicker.MaximumDate) schränkt den Bereich der Datumsangaben in der Datumsauswahl verfügbar. So schränkt zum Beispiel der folgende Code eine Datumsauswahl 60 Jahre vor dem Zeitpunkt vorhanden:

```csharp
var calendar = new NSCalendar(NSCalendarType.Gregorian);
var currentDate = NSDate.Now;
var components = new NSDateComponents();
components.Year = -60;
NSDate minDate = calendar.DateByAddingComponents(components, NSDate.Now, NSCalendarOptions.None);
datePickerView.MinimumDate = minDate;
datePickerView.MaximumDate = NSDate.Now;
```

> [!TIP]
> Es ist möglich, explizit Umwandeln einer `DateTime` auf eine `NSDate`:
> ```csharp
> DatePicker.MinimumDate = (NSDate)DateTime.Today.AddDays (-7);
> DatePicker.MaximumDate = (NSDate)DateTime.Today.AddDays (7);
> ```

#### <a name="minute-interval"></a>Zeitraum von Minuten

Die [ `MinuteInterval` ](xref:UIKit.UIDatePicker.MinuteInterval) Eigenschaft legt fest, das Intervall, an dem die Auswahl Minuten angezeigt werden:

```csharp
datePickerView.MinuteInterval = 10;
```

#### <a name="mode"></a>Modus

Datumsauswahl unterstützen vier [Modi](xref:UIKit.UIDatePickerMode), im folgenden beschrieben:

##### <a name="uidatepickermodetime"></a>UIDatePickerMode.Time

`UIDatePickerMode.Time` Zeigt an, mit einer Stunde und Minute Selektor und einer optionalen AM oder PM-Bezeichnung:

```csharp
datePickerView.Mode = UIDatePickerMode.Time;
```

![UIDatePickerMode.Time](picker-images/image8.png "UIDatePickerMode.Time")

##### <a name="uidatepickermodedate"></a>UIDatePickerMode.Date

`UIDatePickerMode.Date` Zeigt das Datum mit einem Monat, Tag und Jahr Selektor:

```csharp
datePickerView.Mode = UIDatePickerMode.Date;
```

![UIDatePickerMode.Date](picker-images/image7.png "UIDatePickerMode.Date")

Die Reihenfolge der Selektoren hängt von der Datumsauswahl-Gebietsschema, die standardmäßig das Gebietsschema des Systems verwendet ab. In der Abbildung oben zeigt das Layout der Selektoren in die `en_US` Gebietsschema, aber die folgenden, ändert die Reihenfolge von Tag | Monat | Jahr:

```csharp
datePickerView.Locale = NSLocale.FromLocaleIdentifier("en_GB");
```

![Tag | Monat | Jahr](picker-images/image9.png "Tag | Monat | Jahr")

##### <a name="uidatepickermodedateandtime"></a>UIDatePickerMode.DateAndTime

`UIDatePickerMode.DateAndTime` Zeigt eine verkürzte Ansicht des Datums, die Zeit in Stunden und Minuten sowie eine optionale AM oder PM-Bezeichnung (je nachdem, ob ein 12- oder 24-Stunden-Format verwendet wird):

```csharp
datePickerView.Mode = UIDatePickerMode.DateAndTime;
```

![UIDatePickerMode.DateAndTime](picker-images/image6.png "UIDatePickerMode.DateAndTime")

Wie bei [ `UIDatePickerMode.Date` ](#uidatepickermodedate), die Reihenfolge der Selektoren und die Verwendung von einem 12- oder 24-Stunden-Format, die auf dem Gebietsschema der Datumsauswahl abhängig ist.

> [!TIP]
> Verwenden der `Date` Eigenschaft, um den Wert einer Datumsauswahl im Modus erfassen `UIDatePickerMode.Time`, `UIDatePickerMode.Date`, oder `UIDatePickerMode.DateAndTime`. Dieser Wert wird gespeichert, als ein `NSDate`.

##### <a name="uidatepickermodecountdowntimer"></a>UIDatePickerMode.CountDownTimer

`UIDatePickerMode.CountDownTimer` Zeigt die Werte für Stunden und Minuten:

```csharp
datePickerView.Mode = UIDatePickerMode.CountDownTimer;
```

!["UIDatePickerMode.CountDownTimer"](picker-images/image5.png "UIDatePickerMode.CountDownTimer")

Die `CountDownDuration` Eigenschaft erfasst den Wert einer Datumsauswahl in `UIDatePickerMode.CountDownTimer` Modus. Geben Sie beispielsweise Folgendes ein, um den countdownwert auf das aktuelle Datum hinzufügen:

```csharp
var currentTime = NSDate.Now;
var countDownTimerTime = datePickerView.CountDownDuration;
var finishCountdown = currentTime.AddSeconds(countDownTimerTime);

dateLabel.Text = "Alarm set for:" + coundownTimeformat.ToString(finishCountdown);
```

#### <a name="nsdateformatter"></a>NSDateFormatter

Formatieren einer `NSDate`, verwenden Sie eine [ `NSDateFormatter` ](xref:Foundation.NSDateFormatter).

Verwenden einer `NSDateFormatter`, rufen Sie die [ `ToString` ](xref:Foundation.NSDateFormatter.ToString(Foundation.NSDate)) Methode. Zum Beispiel:

```csharp
var date = NSDate.Now;
var formatter = new NSDateFormatter();
formatter.DateStyle = NSDateFormatterStyle.Full;
formatter.TimeStyle = NSDateFormatterStyle.Full;
var formattedDate = formatter.ToString(d);
// Tuesday, August 14, 2018 at 11:20:42 PM Mountain Daylight Time
```

##### <a name="dateformat"></a>DateFormat

Die [ `DateFormat` ](xref:Foundation.NSDateFormatter.DateFormat) Eigenschaft (eine Zeichenfolge) eine `NSDateFormatter` ermöglicht eine Formatspezifikation anpassbare Datum:

```csharp
NSDateFormatter dateFormat = new NSDateFormatter();
dateFormat.DateFormat = "yyyy-MM-dd";
```

##### <a name="timestyle"></a>TimeStyle

Die [ `TimeStyle` ](xref:Foundation.NSDateFormatter.TimeStyle) Eigenschaft (eine [ `NSDateFormatterStyle` ](xref:Foundation.NSDateFormatterStyle) von einer `NSDateFormatter` gibt die Zeit Formatierung basierend auf vordefinierten Formate an:

```csharp
NSDateFormatter timeFormat = new NSDateFormatter();
timeFormat.TimeStyle = NSDateFormatterStyle.Short;
```

Verschiedene `NSDateFormatterStyle` Werte anzeigen wie folgt:

- `NSDateFormatterStyle.Full`: 19:46:00 Uhr Eastern Daylight Time
- `NSDateFormatterStyle.Long`: 7:47:00 PM EDT
- `NSDateFormatterStyle.Medium`: 7:47:00 PM
- `NSDateFormatterSytle.Short`: 7:47 PM

##### <a name="datestyle"></a>DateStyle

Die [ `DateStyle` ](xref:Foundation.NSDateFormatter.DateStyle) Eigenschaft (eine `NSDateFormatterStyle`) von einer `NSDateFormatter` gibt die Formatierung des Datums basierend auf vordefinierten Formate an:

```csharp
NSDateFormatter dateTimeformat = new NSDateFormatter();
dateTimeformat.DateStyle = NSDateFormatterStyle.Long;
```

Verschiedene `NSDateFormatterStyle` Werte anzeigen von Datumsangaben wie folgt:

- `NSDateFormatterStyle.Full`: Mittwoch, August 2 2017 um 7:48 Uhr
- `NSDateFormatterStyle.Long`: 2. August 2017 um 7:49 Uhr
- `NSDateFormatterStyle.Medium`: 2. August 2017, 19:49 Uhr
- `NSDateFormatterStyle.Short`: 8/2/17, 19:50 UHR

> [!NOTE]
> `DateFormat` und `DateStyle` / `TimeStyle` bieten verschiedene Möglichkeiten zum Angeben von Datums- und uhrzeitformatierungen. Bestimmen die zuletzt Eigenschaften festlegen, dass das Datum Formatierungsprogramm Ausgabe des.

## <a name="related-links"></a>Verwandte Links

- [PickerControl (Beispiel)](https://developer.xamarin.com/samples/monotouch/PickerControl/)
