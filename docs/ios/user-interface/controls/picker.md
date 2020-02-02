---
title: Auswahl Steuerelement in xamarin. IOS
description: In diesem Dokument wird beschrieben, wie Sie Auswahl Steuerelemente in einer xamarin. IOS-App entwerfen und mit diesen arbeiten. Darin wird erläutert, wie eine Auswahl im Code und im IOS-Designer implementiert wird.
ms.prod: xamarin
ms.assetid: A2369EFC-285A-44DD-9E80-EC65BC3DF041
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/14/2018
ms.openlocfilehash: 43dbafe16d7cbabdb3b7902dd3d46d845f213fcd
ms.sourcegitcommit: 52fb214c0e0243587d4e9ad9306b75e92a8cc8b7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2020
ms.locfileid: "76940942"
---
# <a name="picker-control-in-xamarinios"></a>Auswahl Steuerelement in xamarin. IOS

Mit einem [`UIPickerView`](xref:UIKit.UIPickerView) können Sie einen Wert aus einer Liste auswählen, indem Sie die einzelnen Komponenten einer Rad ähnlichen Schnittstelle scrollen.

Picker werden häufig verwendet, um ein Datum und eine Uhrzeit auszuwählen. Apple stellt die [`UIDatePicker`](xref:UIKit.UIDatePicker)
-Klasse zu diesem Zweck.

In diesem Artikel wird beschrieben, wie Sie die `UIPickerView`-und `UIDatePicker`-Steuerelemente implementieren und verwenden.

## <a name="uipickerview"></a>Uipickerview

### <a name="implementing-a-picker"></a>Implementieren einer Auswahl

Implementieren Sie eine Auswahl, indem Sie eine neue `UIPickerView`instanziieren:

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

### <a name="pickers-and-storyboards"></a>Picker und Storyboards

Um eine Auswahl im IOS- **Designer**zu erstellen, ziehen Sie eine Auswahl **Ansicht** aus der **Toolbox** auf die Entwurfs Oberfläche.

![Ziehen Sie eine Auswahl Ansicht auf die Entwurfs Oberfläche.](picker-images/image1.png "Ziehen Sie eine Auswahl Ansicht auf die Entwurfs Oberfläche.")

### <a name="working-with-a-picker-control"></a>Arbeiten mit einem Auswahl Steuerelement

Eine Auswahl verwendet ein _Modell_ für die Interaktion mit Daten:

```csharp
public override void ViewDidLoad()
{
    base.ViewDidLoad();
    var pickerModel = new PeopleModel(personLabel);
    personPicker.Model = pickerModel;
}
```

Die [`UIPickerViewModel`](xref:UIKit.UIPickerViewModel) -Basisklasse implementiert zwei Schnittstellen [`IUIPickerDataSource`](xref:UIKit.IUIPickerViewDataSource)
und [`IUIPickerViewDelegate`](xref:UIKit.IUIPickerViewDelegate), die verschiedene Methoden deklarieren, mit denen die Daten einer Auswahl angegeben werden, und wie die Interaktion behandelt wird:

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

Eine Auswahl kann über mehrere Spalten oder _Komponenten_verfügen. Komponenten Partitionieren eine Auswahl in mehrere Abschnitte und ermöglichen so eine einfachere und spezifischere Datenauswahl:

![Auswahl mit zwei Komponenten](picker-images/image3.png "Auswahl mit zwei Komponenten")

Um die Anzahl der Komponenten in einer Auswahl anzugeben, verwenden Sie die [`GetComponentCount`](xref:UIKit.UIPickerViewModel.GetComponentCount(UIKit.UIPickerView)) 
-Methode.

### <a name="customizing-a-pickers-appearance"></a>Anpassen der Darstellung einer Auswahl

Verwenden Sie zum Anpassen der Darstellung einer Auswahl die [`UIPickerView.UIPickerViewAppearance`](xref:UIKit.UIPickerView.UIPickerViewAppearance)
-Klasse oder überschreiben Sie die Methoden [`GetView`](xref:UIKit.UIPickerViewModel.GetView(UIKit.UIPickerView,System.nint,System.nint,UIKit.UIView)) und [`GetRowHeight`](xref:UIKit.UIPickerViewModel.GetRowHeight(UIKit.UIPickerView,System.nint)) in der `UIPickerViewModel`.

## <a name="uidatepicker"></a>UIDatePicker

### <a name="implementing-a-date-picker"></a>Implementieren einer Datumsauswahl

Implementieren Sie eine Datumsauswahl, indem Sie eine `UIDatePicker`instanziieren:

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

### <a name="date-pickers-and-storyboards"></a>Datums-und Storyboards

Zum Erstellen einer Datumsauswahl im **IOS-Designer**ziehen Sie eine **Datums** Auswahl aus der **Toolbox** auf die Entwurfs Oberfläche.

![Ziehen Sie eine Datumsauswahl auf die Entwurfs Oberfläche.](picker-images/image2.png "Ziehen Sie eine Datumsauswahl auf die Entwurfs Oberfläche.")

### <a name="date-picker-properties"></a>Datumsauswahl Eigenschaften

#### <a name="minimum-and-maximum-date"></a>Mindest-und höchst Datum

[`MinimumDate`](xref:UIKit.UIDatePicker.MinimumDate) und [`MaximumDate`](xref:UIKit.UIDatePicker.MaximumDate) beschränken Sie den Datumsbereich, der in der Datumsauswahl verfügbar ist. Der folgende Code schränkt z. b. eine Datumsauswahl auf die 60 Jahre ein, die bis zum aktuellen Zeitpunkt liegen:

```csharp
var calendar = new NSCalendar(NSCalendarType.Gregorian);
var currentDate = NSDate.Now;
var components = new NSDateComponents();
components.Year = -60;
NSDate minDate = calendar.DateByAddingComponents(components, currentDate, NSCalendarOptions.None);
datePickerView.MinimumDate = minDate;
datePickerView.MaximumDate = currentDate;
```

> [!TIP]
> Es ist möglich, eine `DateTime` explizit in eine `NSDate`umzuwandeln:
>
> ```csharp
> DatePicker.MinimumDate = (NSDate)DateTime.Today.AddDays (-7);
> DatePicker.MaximumDate = (NSDate)DateTime.Today.AddDays (7);
> ```

#### <a name="minute-interval"></a>Minuten Intervall

Mit der [`MinuteInterval`](xref:UIKit.UIDatePicker.MinuteInterval) -Eigenschaft wird das Intervall festgelegt, in dem die Auswahl Minuten anzeigt:

```csharp
datePickerView.MinuteInterval = 10;
```

#### <a name="mode"></a>Modus

Date-Picker unterstützen vier [Modi](xref:UIKit.UIDatePickerMode), die unten beschrieben werden:

##### <a name="uidatepickermodetime"></a>UIDatePickerMode.Time

in `UIDatePickerMode.Time` wird die Uhrzeit mit einer Auswahl von Stunde und Minute und einer optionalen am-oder PM-Bezeichnung angezeigt:

```csharp
datePickerView.Mode = UIDatePickerMode.Time;
```

![Uidatepickermode. Time](picker-images/image8.png "UIDatePickerMode.Time")

##### <a name="uidatepickermodedate"></a>UIDatePickerMode.Date

`UIDatePickerMode.Date` zeigt das Datum mit einer Auswahl für Monat, Tag und Jahr an:

```csharp
datePickerView.Mode = UIDatePickerMode.Date;
```

![Uidatepickermode. Date](picker-images/image7.png "UIDatePickerMode.Date")

Die Reihenfolge der Selektoren hängt vom Gebiets Schema der Datumsauswahl ab, das standardmäßig das Gebiets Schema des Systems verwendet. In der Abbildung oben wird das Layout der Selektoren im `en_US` Gebiets Schema dargestellt. im folgenden wird die Reihenfolge jedoch in "Day" geändert. Monat | Jährigen

```csharp
datePickerView.Locale = NSLocale.FromLocaleIdentifier("en_GB");
```

![Tag | Monat | Jährigen](picker-images/image9.png "Tag | Monat | Jährigen")

##### <a name="uidatepickermodedateandtime"></a>UIDatePickerMode.DateAndTime

`UIDatePickerMode.DateAndTime` zeigt eine verkürzte Ansicht des Datums, die Uhrzeit in Stunden und Minuten sowie eine optionale am-oder PM-Bezeichnung an (abhängig davon, ob ein 12-oder 24-Stunden-Takt verwendet wird):

```csharp
datePickerView.Mode = UIDatePickerMode.DateAndTime;
```

![Uidatepickermode. DateAndTime](picker-images/image6.png "UIDatePickerMode.DateAndTime")

Wie bei [`UIDatePickerMode.Date`](#uidatepickermodedate)hängt die Reihenfolge der Selektoren und die Verwendung einer 12-oder 24-Stunden-Uhrzeit vom Gebiets Schema der Datumsauswahl ab.

> [!TIP]
> Verwenden Sie die `Date`-Eigenschaft, um den Wert einer Datumsauswahl im Modus `UIDatePickerMode.Time`, `UIDatePickerMode.Date`oder `UIDatePickerMode.DateAndTime`zu erfassen. Dieser Wert wird als `NSDate`gespeichert.

##### <a name="uidatepickermodecountdowntimer"></a>UIDatePickerMode.CountDownTimer

`UIDatePickerMode.CountDownTimer` zeigt die Stunden-und Minuten Werte an:

```csharp
datePickerView.Mode = UIDatePickerMode.CountDownTimer;
```

!["Uidatepickermode. Countdowntimer"](picker-images/image5.png "UIDatePickerMode.CountDownTimer")

Die `CountDownDuration`-Eigenschaft erfasst den Wert einer Datumsauswahl im `UIDatePickerMode.CountDownTimer` Modus. Fügen Sie beispielsweise dem aktuellen Datum den countdownwert hinzu:

```csharp
var currentTime = NSDate.Now;
var countDownTimerTime = datePickerView.CountDownDuration;
var finishCountdown = currentTime.AddSeconds(countDownTimerTime);

dateLabel.Text = "Alarm set for:" + coundownTimeformat.ToString(finishCountdown);
```

#### <a name="nsdateformatter"></a>NSDateFormatter

Um eine `NSDate`zu formatieren, verwenden Sie eine [`NSDateFormatter`](xref:Foundation.NSDateFormatter).

Um einen `NSDateFormatter`zu verwenden, nennen Sie dessen [`ToString`](xref:Foundation.NSDateFormatter.ToString(Foundation.NSDate)) -Methode. Beispiel:

```csharp
var date = NSDate.Now;
var formatter = new NSDateFormatter();
formatter.DateStyle = NSDateFormatterStyle.Full;
formatter.TimeStyle = NSDateFormatterStyle.Full;
var formattedDate = formatter.ToString(d);
// Tuesday, August 14, 2018 at 11:20:42 PM Mountain Daylight Time
```

##### <a name="dateformat"></a>DateFormat

Die [`DateFormat`](xref:Foundation.NSDateFormatter.DateFormat) -Eigenschaft (eine Zeichenfolge) einer `NSDateFormatter` ermöglicht eine anpassbare Datumsformat Spezifikation:

```csharp
NSDateFormatter dateFormat = new NSDateFormatter();
dateFormat.DateFormat = "yyyy-MM-dd";
```

##### <a name="timestyle"></a>Timestyle

Die [`TimeStyle`](xref:Foundation.NSDateFormatter.TimeStyle) -Eigenschaft (ein [`NSDateFormatterStyle`](xref:Foundation.NSDateFormatterStyle) einer `NSDateFormatter` die die Zeit Formatierung basierend auf vordefinierten Stilen angibt:

```csharp
NSDateFormatter timeFormat = new NSDateFormatter();
timeFormat.TimeStyle = NSDateFormatterStyle.Short;
```

Verschiedene `NSDateFormatterStyle` Werte zeigen die Zeiten wie folgt an:

- `NSDateFormatterStyle.Full`: 7:46:00 Uhr Ost-Sommerzeit
- `NSDateFormatterStyle.Long`: 7:47:00 Uhr EDT
- `NSDateFormatterStyle.Medium`: 7:47:00 Uhr
- `NSDateFormatterSytle.Short`: 7:47 Uhr

##### <a name="datestyle"></a>DateStyle

Die [`DateStyle`](xref:Foundation.NSDateFormatter.DateStyle) -Eigenschaft (ein `NSDateFormatterStyle`) eines `NSDateFormatter` gibt die Datums Formatierung basierend auf vordefinierten Stilen an:

```csharp
NSDateFormatter dateTimeformat = new NSDateFormatter();
dateTimeformat.DateStyle = NSDateFormatterStyle.Long;
```

Verschiedene `NSDateFormatterStyle` Werte zeigen Datumsangaben wie folgt an:

- `NSDateFormatterStyle.Full`: Mittwoch, 2. August, 2017 Uhr 7:48 Uhr
- `NSDateFormatterStyle.Long`: 2. August, 2017 Uhr 7:49 Uhr
- `NSDateFormatterStyle.Medium`: 2. Aug, 2017, 7:49 Uhr
- `NSDateFormatterStyle.Short`: 8/2/17, 7:50 Uhr

> [!NOTE]
> `DateFormat` und `DateStyle`/`TimeStyle` bieten verschiedene Möglichkeiten zum Angeben der Datums-und Uhrzeit Formatierung. Die zuletzt festgelegten Eigenschaften bestimmen die Ausgabe des dateiformatierers.

## <a name="related-links"></a>Verwandte Links

- [Pickercontrol (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/pickercontrol)
