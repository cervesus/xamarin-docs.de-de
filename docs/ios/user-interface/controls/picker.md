---
title: Datumsauswahl
description: Dieser Leitfaden behandelt entwerfen und Arbeiten mit Bildlaufbereich in einem Xamarin.iOS-app.
ms.topic: article
ms.prod: xamarin
ms.assetid: A2369EFC-285A-44DD-9E80-EC65BC3DF041
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/02/2017
ms.openlocfilehash: 402b17ddbb28fb8896ad0f158fe8dbcd17689f40
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="picker"></a>Datumsauswahl

Das Kontoauswahl-Steuerelement zeigt 'Wheel Like'-Steuerelements, das eine bildlauffähige Liste von Werten mit den ausgewählten Wert wird hervorgehoben enthält. Benutzer drehen Sie das Mausrad, um die gewünschte Option.

Ein bestimmter Benutzer Fall werden für die Datumsauswahl das Datum und / oder festlegen. Diese Apple bereit hat eine benutzerdefinierte Unterklasse von der UIPickerView Klasse mit dem Namen UIDatePicker erstellt.

Der Artikel umfasst, implementieren und Verwenden der [Datumsauswahl](#picker) und [Datumsauswahl](#datepicker) Steuerelemente.

<a name="picker">

## <a name="picker"></a>Datumsauswahl

### <a name="implementing-a-picker"></a>Implementieren eine Auswahl

Eine Auswahl wird dadurch implementiert, instanziieren ein neues [`UIPickerView`](https://developer.xamarin.com/api/type/UIKit.UIPickerView/):

```csharp
UIPickerView pickerView = new UIPickerView(
                            new CGRect(
                                UIScreen.MainScreen.Bounds.X-UIScreen.MainScreen.Bounds.Width, UIScreen.MainScreen.Bounds.Height -230, 
                                UIScreen.MainScreen.Bounds.Width, 
                                180));
```


### <a name="pickers-and-storyboards"></a>Farbauswahl und Storyboards

Wenn Sie den iOS-Designer verwenden, um die Benutzeroberfläche zu erstellen, kann die Auswahl auf das Layout aus der Toolbox hinzugefügt werden:

![](picker-images/image1.png)


### <a name="working-with-picker"></a>Arbeiten mit der Auswahl

Nachdem Sie eine Auswahl, ob im Code erstellt haben oder über Storyboards, Sie zum zuweisen müssen einer _Modell_ darauf, damit Sie übergeben und mit Daten interagieren können

```csharp
public override void ViewDidLoad()
{
    base.ViewDidLoad();

    var pickerModel = new PeopleModel(personLabel);

    personPicker.Model = pickerModel;
}
```

Der folgende Code zeigt ein Beispiel für ein Modell:

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

Zuerst müssen Sie einige Daten für die verschiedene Optionen für die Benutzer auswählen Bereitstellung übergeben. Wenn möglich versuchen, diese Liste so kurz wie möglich zu halten oder ggf. versuchen, mehr als eine verwenden "Nummer wählen" (aufgerufen *Komponenten*):

![Farbauswahl mit zwei Komponenten](picker-images/image3.png)

Um die Anzahl der Komponenten festzulegen, überschreiben die `GetComponentCount` Methode: 

```csharp
public override nint GetComponentCount(UIPickerView pickerView)
{
    return 2;
}
```

Der Rückgabewert gibt die Anzahl der wählt Ihrer Auswahl haben.

### <a name="customizing-appearance"></a>Anpassen der Darstellung
 
Die Darstellung von der `UIPickerView` angepasst werden, mithilfe der `UIPickerView.UIPickerViewAppearance` -Klasse oder durch Überschreiben der `UIPickerViewModel.GetView` und `UIPickerViewModel.GetRowHeight` Methoden in der `UIPickerViewModel`.


<a name="datepicker">

## <a name="date-picker"></a>Datumsauswahl

### <a name="implementing-a-date-picker"></a>Implementieren einer Datumsauswahl

Eine Datumsauswahl wird dadurch implementiert, instanziieren ein neues [ `UIDatePickerView` ](https://developer.xamarin.com/api/type/UIKit.UIDatePicker/):

```csharp
UIPickerView pickerView = new UIPickerView(
                            new CGRect(
                                UIScreen.MainScreen.Bounds.X-UIScreen.MainScreen.Bounds.Width, UIScreen.MainScreen.Bounds.Height -230, 
                                UIScreen.MainScreen.Bounds.Width, 
                                180));
```


### <a name="date-pickers-and-storyboards"></a>Datumsauswahl und Storyboards

Wenn Sie die iOS-Designer zum Erstellen der Benutzeroberflächenautomatisierungs verwenden die **Datumsauswahl** können das Layout aus der Toolbox hinzugefügt werden. Die folgenden Eigenschaften können über das Auffüllzeichen Eigenschaften angepasst werden:

![](picker-images/image2.png)

* **Modus** – das Datum und Uhrzeit-Modus. Dies kann Datum, Uhrzeit, Datum und Uhrzeit oder einen Countdownzeitgeber sein. 
* **Gebietsschema** – das Gebietsschema der Datumsauswahl. Wählen Sie **Standard** auf dem als Standard festgelegt oder auf einem anderen bestimmten Gebietsschema festgelegt.
* **Intervall** – zeigt den inkrementellen Wert in der die Uhr Optionen angezeigt werden.
* **Datum, Datum minimale, Maximaldatum** – legt die Anfangsdatum der Auswahl angezeigt wird und die Einschränkungen für die auswählbaren Datumsangaben.

### <a name="configuring-the-datepicker"></a>Konfigurieren der Datumsauswahl

Sie können einschränken, den Datumsbereich, die ein Benutzer, mithilfe auswählen kann der `MinimumDate` und `MaximumDate` Eigenschaften. Der folgende Codeausschnitt zeigt ein Beispiel zum Festlegen des Bereichs zwischen 60 Jahre vor und heute:

```csharp
var calendar = new NSCalendar(NSCalendarType.Gregorian);
var currentDate = NSDate.Now;
var components = new NSDateComponents();

components.Year = -60;

NSDate minDate = calendar.DateByAddingComponents(components, NSDate.Now, NSCalendarOptions.None);

datePickerView.MinimumDate = minDate;
datePickerView.MaximumDate = NSDate.Now;
```

Sie alternativ, können auch .NET Steuerelemente verwenden den minimalen und maximalen Datumsbereich festlegen. Zum Beispiel:

```csharp
DatePicker.MinimumDate = (NSDate)DateTime.Today.AddDays (-7);
DatePicker.MaximumDate = (NSDate)DateTime.Today.AddDays (7);
```

Sie können auch Festlegen der `MinuteInterval` Eigenschaft, um das Intervall festzulegen, an dem die Auswahl einer Minuten angezeigt wird. Der folgende Codeausschnitt kann verwendet werden, festlegen den Selektor Minuten, in Intervallen von 10 festgelegt werden.

```csharp
datePickerView.MinuteInterval = 10;
```

Es gibt vier Modi, die der Datumsauswahl mithilfe können, um festgelegt werden die [ `UIDatePicker.Mode` ](https://developer.xamarin.com/api/property/UIKit.UIDatePicker.Mode/) Eigenschaft. Die folgende Liste zeigt ein Beispiel der einzelnen Aktionen und ihre Implementierung:

#### <a name="time"></a>zeit

Der Time-Modus zeigt die Zeit mit einer Stunde und Minute-Auswahl und eine optionale Bezeichnung von AM oder PM. Es wird festgelegt, mit der `UIDatePickerMode.Time` Eigenschaft. Zum Beispiel:

```csharp
datePickerView.Mode = UIDatePickerMode.Time;
```

Die folgende Abbildung zeigt ein Beispiel für dieses DatePicker-Modus:

![](picker-images/image8.png)



#### <a name="date"></a>Datum

Der Date-Modus zeigt das Datum mit einem Monat, Tag und Jahr Selektor. Es wird festgelegt, mit der `UIDatePickerMode.Date` Eigenschaft. Zum Beispiel:

```csharp
datePickerView.Mode = UIDatePickerMode.Date;
```

Die folgende Abbildung zeigt ein Beispiel für diese DatePicker:

![](picker-images/image7.png)

Die Reihenfolge der Selektoren, die auf dem Gebietsschema des hängt die der `UIDatePicker`. Standardmäßig wird dies auf dem als Standard festgelegt werden. Die Abbildung oben zeigt das Layout der Selektoren in der `en_US` Gebietsschema angegeben, aber, ändern Sie ihn in einem Tag | Monat | Jahr-Layout können Sie folgenden Code verwenden, zum Festlegen des Gebietsschemas:

```csharp
datePickerView.Locale = NSLocale.FromLocaleIdentifier("en_GB");
```

![](picker-images/image9.png)


#### <a name="date-and-time"></a>Datum und Uhrzeit

Das Datum und Uhrzeit-Modus enthält einen Überblick über Shortend das Datum, die Zeit in Stunden und Minuten sowie eine optionale Bezeichnung Dependings von AM oder PM auf, wenn ein 12- oder 24-Stunden-Format verwendet wird. Es wird festgelegt, mit der `UIDatePickerMode.DateAndTime` Eigenschaft. Zum Beispiel:

```csharp
datePickerView.Mode = UIDatePickerMode.DateAndTime;
```

Die folgende Abbildung zeigt ein Beispiel für diese DatePicker:

![](picker-images/image6.png)

Wie bei [Datum](#Date), die Reihenfolge der Auswahlzeiger und die Verwendung von 12 oder 24-Stunden-Format hängt von dem Gebietsschema des der der `UIDatePicker`.

#### <a name="countdown-timer"></a>Countdownzeitgeber

Der Countdown Timer-Modus zeigt die Stunde und Minute Werte. Es wird festgelegt, mit der `UIDatePickerMode.CountDownTimer` Eigenschaft. Zum Beispiel:

```csharp
datePickerView.Mode = UIDatePickerMode.CountDownTimer;
```

Die folgende Abbildung zeigt ein Beispiel für diese DatePicker:

![](picker-images/image5.png)

Sie können die `CountDownDuration` Eigenschaft den Wert Dispayed durch die Datumsauswahl Countdown erfassen. Um das aktuelle Datum der countdownwert hinzugefügt haben, verwenden Sie beispielsweise den folgenden Code:

```csharp
var currentTime = NSDate.Now;
var countDownTimerTime = datePickerView.CountDownDuration;
var finishCountdown = currentTime.AddSeconds(countDownTimerTime);

dateLabel.Text = "Alarm set for:" + coundownTimeformat.ToString(finishCountdown);
```

#### <a name="formatting"></a>Formatierung 

Die Werte der Uhrzeit-, Datums- und DateAndTime Modi können aufgezeichnet werden, mithilfe der `Date` Eigenschaft auf die UIDatePicker (z. B.: `datePickerView.Date`), vom Typ NSDate. Verwenden Sie zum Formatieren dieses Datums in etwas mehr lesbare [ `NSDateFormatter` ](https://developer.xamarin.com/api/type/Foundation.NSDateFormatter/). Die folgenden Beispiele zeigen, wie einige der verfügbaren Eigenschaften für diese Klasse verwendet werden.

Die `DateFormat` als Zeichenfolge festgelegt ist, um darzustellen, wie das Datum angezeigt werden soll:

```csharp
NSDateFormatter dateFormat = new NSDateFormatter();
dateFormat.DateFormat = "yyyy-MM-dd";
```

Die `TimeStyle` Eigenschaftensätze eine `NSDateFormatterStyle`:

```csharp
NSDateFormatter timeFormat = new NSDateFormatter();
timeFormat.TimeStyle = NSDateFormatterStyle.Short;
```

Die Felder für `NSDateFormatterStyle` wie folgt angezeigt:

![](picker-images/timestyle.png)

Die `DateStyle` Eigenschaftensätze eine `NSDateFormatterStyle`:

```csharp
NSDateFormatter dateTimeformat = new NSDateFormatter();
dateTimeformat.DateStyle = NSDateFormatterStyle.Long;
```

Die Felder für `NSDateFormatterStyle` wie folgt angezeigt:

![](picker-images/datestyle.png)

Sie können dann die formatierte NSDate in eine Zeichenfolge ausgeben, mit dem folgenden Code:

```csharp
dateLabel.Text = dateTimeformat.ToString(datePickerView.Date);
```

## <a name="related-links"></a>Verwandte Links

- [PickerControl (Beispiel)](https://developer.xamarin.com/samples/monotouch/PickerControl/)
