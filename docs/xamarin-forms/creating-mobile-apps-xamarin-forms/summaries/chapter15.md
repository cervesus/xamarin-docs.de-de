---
title: Zusammenfassung der Kapitel 15. Die interaktive Schnittstelle
description: 'Erstellen von Mobile Apps mit xamarin. Forms: Zusammenfassung der Kapitel 15. Die interaktive Schnittstelle'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F54E86F4-1CDA-474E-9B09-242060C2C13D
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 1c30f87b9173d2ca4de0b2d91ad13145031e9b0a
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70760763"
---
# <a name="summary-of-chapter-15-the-interactive-interface"></a>Zusammenfassung der Kapitel 15. Die interaktive Schnittstelle

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15)

In diesem Kapitel werden acht `View` ableitungen, die Interaktion mit dem Benutzer ermöglichen.

## <a name="view-overview"></a>Übersicht anzeigen

Xamarin.Forms enthält 20 instanziiert werden von abgeleiteten Klassen `View` , nicht jedoch `Layout`. Diese sechs haben in den vorangegangenen Kapiteln beschrieben wurde:

- `Label`: [**Kapitel 2. Aufbau einer app**](chapter02.md)
- `BoxView`: [**Kapitel 3. Scrollen im Stapel**](chapter03.md)
- `Button`: [**Kapitel 6: Schaltflächenklicks**](chapter06.md)
- `Image`: [**Kapitel 13. Bitmaps**](chapter13.md)
- `ActivityIndicator`: [**Kapitel 13. Bitmaps**](chapter13.md)
- `ProgressBar`: [**Kapitel 14. Von "AbsoluteLayout"** ](chapter14.md)

Die acht Ansichten in diesem Kapitel ermöglichen effektiv den Benutzer für die Interaktion mit .NET Grunddatentypen:

|Datentyp|Ansichten|
|--- |--- |
|`Double`|[`Slider`](xref:Xamarin.Forms.Slider), [`Stepper`](xref:Xamarin.Forms.Stepper)|
|`Boolean`|[`Switch`](xref:Xamarin.Forms.Switch)|
|`String`|[`Entry`](xref:Xamarin.Forms.Entry), [`Editor`](xref:Xamarin.Forms.Editor), [`SearchBar`](xref:Xamarin.Forms.SearchBar)|
|`DateTime`|[`DatePicker`](xref:Xamarin.Forms.DatePicker), [`TimePicker`](xref:Xamarin.Forms.TimePicker)|

Sie können diese Sichten als visuelle, interaktive Darstellungen der zugrunde liegenden Datentypen vorstellen. Dieses Konzept wird im nächsten Kapitel mehr untersucht [ **Kapitel 16. Die Datenbindung**](chapter16.md).

Die verbleibenden sechs Ansichten werden in den folgenden Kapiteln erläutert:

- `WebView`: [**Kapitel 16. Datenbindung**](chapter16.md)
- `Picker`: [**Kapitel 19. Auflistungsansichten**](chapter19.md)
- `ListView`: [**Kapitel 19. Auflistungsansichten**](chapter19.md)
- `TableView`: [**Kapitel 19. Auflistungsansichten**](chapter19.md)
- `Map`: [**Kapitel 28. Position und Karten**](chapter28.md)
- `OpenGLView`: Nicht abgedeckt in diesem Buch (und keine Unterstützung für Windows-Plattformen)

## <a name="slider-and-stepper"></a>Schieberegler und zugeordnetem

Beide [ `Slider` ](xref:Xamarin.Forms.Slider) und [ `Stepper` ](xref:Xamarin.Forms.Stepper) ermöglicht dem Benutzer, die einen numerischen Wert aus einer Palette auswählen. Die `Slider` ist ein durchgehenden Bereich während der `Stepper` diskrete Werte umfasst.

### <a name="slider-basics"></a>Schieberegler-Grundlagen

Die [ `Slider` ](xref:Xamarin.Forms.Slider) ein horizontalen Balken, der einen Bereich von Werten aus mindestens auf der linken Seite darstellt, auf ein Maximum auf der rechten Seite ist. Drei öffentliche Eigenschaften definiert:

- [`Value`](xref:Xamarin.Forms.Slider.Value) Der Typ `double`, Standardwert 0
- [`Minimum`](xref:Xamarin.Forms.Slider.Minimum) Der Typ `double`, Standardwert 0
- [`Maximum`](xref:Xamarin.Forms.Slider.Maximum) Der Typ `double`, Standardwert 1

Die bindbaren Eigenschaften, die diese Eigenschaften zurück. Stellen Sie sicher, dass sie konsistent sind:

- Für alle drei Eigenschaften die [ `coerceValue` ](xref:Xamarin.Forms.BindableProperty.CoerceValueDelegate) Methode, die für die bindbare Eigenschaft sichergestellt, dass wird angegebene `Value` zwischen `Minimum` und `Maximum`.
- Die [ `validateValue` ](xref:Xamarin.Forms.BindableProperty.ValidateValueDelegate) Methode `MinimumProperty` gibt `false` Wenn `Minimum` wird festgelegt auf einen Wert größer als oder gleich `Maximum`, und Ähnliches gilt für `MaximumProperty`. Zurückgeben von `false` aus der `validateValue` Methode bewirkt, dass ein `ArgumentException` ausgelöst werden soll.

`Slider` wird ausgelöst der [ `ValueChanged` ](xref:Xamarin.Forms.Slider.ValueChanged) Ereignis mit der eine [ `ValueChangedEventArgs` ](xref:Xamarin.Forms.ValueChangedEventArgs) Argument bei der `Value` eigenschaftsänderungen, entweder programmgesteuert oder wenn der Benutzer bearbeitet der `Slider`.

Die [ **SliderDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SliderDemo) Beispiel veranschaulicht die einfache Verwendung von der `Slider`.

### <a name="common-pitfalls"></a>Häufige Probleme

Sowohl im Code als auch in XAML die `Minimum` und `Maximum` Eigenschaften werden festgelegt, in der Reihenfolge, die Sie angeben. Achten Sie darauf, um diese Eigenschaften zu initialisieren, damit `Maximum` ist immer größer als `Minimum`. Andernfalls wird eine Ausnahme ausgelöst.

Initialisieren der `Slider` Eigenschaften können dazu führen, dass die `Value` zu ändernden Eigenschaft und die `ValueChanged` Ereignis ausgelöst werden. Sie sollten sicherstellen, dass die `Slider` Ereignishandler nicht auf Sichten, die noch während der seiteninitialisierung erstellt wurden, noch nicht zugreifen.

Die `ValueChanged` Ereignis nicht ausgelöst, während der `Slider` Initialisierung, wenn die `Value` eigenschaftenänderungen. Rufen Sie die `ValueChanged` Handler direkt aus dem Code.

### <a name="slider-color-selection"></a>Schieberegler-Farbauswahl

Die [ **RgbSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/RgbSliders) Programm enthält drei `Slider` Elemente, die Sie interaktiv eine Farbe auszuwählen, indem Sie die RGB-Werte angeben können:

[![Dreifacher Screenshot von R, G B Schieberegler](images/ch15fg03-small.png "RGB-Schiebereglern")](images/ch15fg03-large.png#lightbox "RGB-Schiebereglern")

Der [ **TextFade** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/TextFade) Beispiel verwendet zwei `Slider` Elementen, die zwei verschieben `Label` Elemente über eine `AbsoluteLayout` und Ausblenden eines in die andere.

### <a name="the-stepper-difference"></a>Der Unterschied zugeordnetem

Die [ `Stepper` ](xref:Xamarin.Forms.Stepper) definiert die gleichen Eigenschaften und Ereignisse als `Slider` jedoch `Maximum` Eigenschaft ist auf 100 initialisiert und `Stepper` definiert eine vierte-Eigenschaft:

- [`Increment`](xref:Xamarin.Forms.Stepper.Increment) Der Typ `double`, initialisiert auf 1

Visuell die `Stepper` besteht aus zwei Schaltflächen, die mit der Bezeichnung **&ndash;** und **+** . Drücken Sie **&ndash;** verringert `Value` von `Increment` auf ein Minimum von `Minimum`. Drücken Sie **+** erhöht `Value` von `Increment` auf ein Maximum von `Maximum`.

Dies wird veranschaulicht, durch die [ **StepperDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/StepperDemo) Beispiel.

## <a name="switch-and-checkbox"></a>Switch und das Kontrollkästchen

Die [ `Switch` ](xref:Xamarin.Forms.Switch) ermöglicht dem Benutzer, die einen booleschen Wert angeben.

### <a name="switch-basics"></a>Switch-Grundlagen

Visuell die `Switch` besteht aus einer Umschaltfläche, die und deaktiviert werden kann. Die Klasse definiert eine Eigenschaft:

- [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) Der Typ `bool`

`Switch` definiert ein Ereignis:

- [`Toggled`](xref:Xamarin.Forms.Switch.Toggled) zusammen mit einem [ `ToggledEventArgs` ](xref:Xamarin.Forms.ToggledEventArgs) Objekt wird immer dann ausgelöst, wenn die `IsToggled` eigenschaftenänderungen.

Die [ **SwitchDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SwitchDemo) Programm zeigt die `Switch`.

### <a name="a-traditional-checkbox"></a>Eine herkömmliche Kontrollkästchen

Einige Entwickler bevorzugen möglicherweise einen eher traditionellen `CheckBox` auf die `Switch`. Die [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) -Bibliothek enthält eine `CheckBox` abgeleitete Klasse `ContentView`. `CheckBox` wird implementiert, indem die [CheckBox.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml) und [CheckBox.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml.cs) Dateien. `CheckBox` definiert drei Eigenschaften (`Text`, `FontSize`, und `IsChecked`) und ein `CheckedChanged` Ereignis.

Die [ **CheckBoxDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/CheckBoxDemo) veranschaulicht dies `CheckBox`.

## <a name="typing-text"></a>Eingeben von text

Xamarin.Forms definiert drei Ansichten, mit die den Benutzer Text eingeben und bearbeiten können:

- [`Entry`](xref:Xamarin.Forms.Entry) für eine einzelne Zeile des Texts
- [`Editor`](xref:Xamarin.Forms.Editor) für mehrere Textzeilen
- [`SearchBar`](xref:Xamarin.Forms.SearchBar) für eine einzelne Zeile des Texts zu suchen.

`Entry` und `Editor` abgeleitet [ `InputView` ](xref:Xamarin.Forms.InputView), die sich daraus ableitet `View`. `SearchBar` wird direkt von abgeleitet `View`.

### <a name="keyboard-and-focus"></a>Tastenkombinationen und den Fokus

Auf Smartphones und Tablets, ohne physischen Tastaturen die `Entry`, `Editor`, und `SearchBar` Elemente, die alle dazu führen, dass eine virtuelle Tastatur anmeldeinformationsfenster aufgerufen. Das Vorhandensein dieser Tastatur auf dem Bildschirm bezieht sich auf den Eingabefokus. Eine Ansicht benötigen Sie Folgendes der [ `IsVisible` ](xref:Xamarin.Forms.VisualElement.IsVisible) und [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) Eigenschaften festgelegt, um `true` um Eingabefokus erhalten.

Mit dem Eingabefokus sind zwei Methoden, eine schreibgeschützte Eigenschaft und zwei Ereignisse beteiligt. Diese werden alle durch definiert `VisualElement`:

- Die [ `Focus` ](xref:Xamarin.Forms.VisualElement.Focus) Methode versucht, den Eingabefokus auf ein Element festgelegt, und gibt `true` bei Erfolg
- Die [ `Unfocus` ](xref:Xamarin.Forms.VisualElement.Unfocus) Methode entfernt Eingabefokus aus einem Element
- Die [ `IsFocused` ](xref:Xamarin.Forms.VisualElement.IsFocused) schreibgeschützte Eigenschaft gibt an, wenn das Element den Eingabefokus besitzt
- Die [ `Focused` ](xref:Xamarin.Forms.VisualElement.Focused) Ereignis gibt an, wenn ein Element den Eingabefokus erhält
- Die [ `Unfocused` ](xref:Xamarin.Forms.VisualElement.Unfocused) Ereignis gibt an, wenn ein Element den Eingabefokus verliert

### <a name="choosing-the-keyboard"></a>Auswählen der Tastaturfokus

Die [ `InputView` ](xref:Xamarin.Forms.InputView) Klasse, von der `Entry` und `Editor` abgeleitet werden nur eine Eigenschaft definiert:

- [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard) Der Typ [`Keyboard`](xref:Xamarin.Forms.Keyboard)

Hiermit wird die Art der Tastatur, die angezeigt wird. Einige Tastaturen sind für URIs oder Zahlen optimiert.

Die `Keyboard` Klasse ermöglicht das Festlegen einer Tastatur mit einem statischen [ `Keyboard.Create` ](xref:Xamarin.Forms.Keyboard.Create(Xamarin.Forms.KeyboardFlags)) Methode mit einem Argument des Typs [ `KeyboardFlags` ](xref:Xamarin.Forms.KeyboardFlags), eine Enumeration mit den folgenden Bitflags:

- `None` auf 0 festgelegt
- [`CapitalizeSentence`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeSentence) auf 1 festgelegt
- [`Spellcheck`](xref:Xamarin.Forms.KeyboardFlags.Spellcheck) auf 2 festgelegt
- [`Suggestions`](xref:Xamarin.Forms.KeyboardFlags.Suggestions) auf 4 festgelegt
- [`All`](xref:Xamarin.Forms.KeyboardFlags.All) Legen Sie auf \xFFFFFFFF

Bei Verwendung der mehrzeiligen [ `Editor` ](xref:Xamarin.Forms.Editor) aufrufen, wenn ein Absatz oder mehrere der Text erwartet wird, `Keyboard.Create` ist ein guter Ansatz zur Auswahl einer Tastatur. Für die einzeilige [ `Entry` ](xref:Xamarin.Forms.Entry), die folgenden statischen schreibgeschützten Eigenschaften der `Keyboard` eignen:

- [`Default`](xref:Xamarin.Forms.Keyboard.Default)
- [`Text`](xref:Xamarin.Forms.Keyboard.Text)
- [`Chat`](xref:Xamarin.Forms.Keyboard.Chat)
- [`Url`](xref:Xamarin.Forms.Keyboard.Url)
- [`Email`](xref:Xamarin.Forms.Keyboard.Email)
- [`Telephone`](xref:Xamarin.Forms.Keyboard.Telephone)
- [`Numeric`](xref:Xamarin.Forms.Keyboard.Numeric) für positive Zahlen mit oder ohne Dezimalstellen.

Die [ `KeyboardTypeConverter` ](xref:Xamarin.Forms.KeyboardTypeConverter) ermöglicht es Ihnen, diese Eigenschaften in XAML, wie die [ **EntryKeyboards** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/EntryKeyboards) Programm.

### <a name="entry-properties-and-events"></a>Eigenschaften und Ereignisse

Der einzeiligen [ `Entry` ](xref:Xamarin.Forms.Entry) definiert die folgenden Eigenschaften:

- [`Text`](xref:Xamarin.Forms.Entry.Text) Der Typ `string`, der Text erscheint in der `Entry`
- [`TextColor`](xref:Xamarin.Forms.Entry.TextColor) Der Typ `Color`
- [`FontFamily`](xref:Xamarin.Forms.Entry.FontFamily) Der Typ `string`
- [`FontSize`](xref:Xamarin.Forms.Entry.FontSize) Der Typ `double`
- [`FontAttributes`](xref:Xamarin.Forms.Entry.FontAttributes) Der Typ `FontAttributes`
- [`IsPassword`](xref:Xamarin.Forms.Entry.IsPassword) Der Typ `bool`, wodurch Zeichen zu maskierenden
- [`Placeholder`](xref:Xamarin.Forms.Entry.Placeholder) Der Typ `string`, für schwach Text erscheint in der `Entry` vor nichts eingegeben wird
- [`PlaceholderColor`](xref:Xamarin.Forms.Entry.PlaceholderColor) Der Typ `Color`

Die `Entry` definiert außerdem zwei Ereignisse:

- [`TextChanged`](xref:Xamarin.Forms.Entry.TextChanged) mit einem [ `TextChangedEventArgs` ](xref:Xamarin.Forms.TextChangedEventArgs) Objekt ausgelöst, wenn die `Text` eigenschaftsänderungen
- [`Completed`](xref:Xamarin.Forms.Entry.Completed), wird ausgelöst, wenn der Benutzer abgeschlossen ist, und die Tastatur geschlossen wird. Der Benutzer wird der Abschluss auf eine plattformspezifische Weise

Die [ **QuadraticEquations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/QuadaticEquations) veranschaulicht diese beiden Ereignisse.

### <a name="the-editor-difference"></a>Die Editor-Differenz

Das mehrzeilige [ `Editor` ](xref:Xamarin.Forms.Editor) definiert die gleiche `Text` und `Font` Eigenschaften wie `Entry` jedoch nicht die anderen Eigenschaften. `Editor` definiert auch die gleichen zwei Eigenschaften wie `Entry`.

[**JustNotes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/JustNotes) ist ein Freiform-Anfertigung von Anmerkungen zu dieser-Programm, das speichert und wiederherstellt den Inhalt der `Editor`.

### <a name="the-searchbar"></a>Die SearchBar

Die [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) nicht von abgeleitet `InputView`, sodass Sie keine `Keyboard` Eigenschaft. Es muss jedoch alle die `Text`, `Font`, und `Placeholder` Eigenschaften, die `Entry` definiert. Darüber hinaus `SearchBar` drei zusätzliche Eigenschaften definiert:

- [`CancelButtonColor`](xref:Xamarin.Forms.SearchBar.CancelButtonColor) Der Typ `Color`
- [`SearchCommand`](xref:Xamarin.Forms.SearchBar.SearchCommand) Der Typ [ `ICommand` ](xref:System.Windows.Input.ICommand) für die Verwendung mit Daten von Bindungen und MVVM
- [`SearchCommandParameter`](xref:Xamarin.Forms.SearchBar.SearchCommandParameter) Der Typ `Object`, für die Verwendung mit `SearchCommand`

Die plattformspezifischen Abbrechen Löschvorgängen von Schaltfläche, den Text. Die `SearchBar` verfügt auch über eine plattformspezifische Durchsuchen-Schaltfläche. Drücken Sie eine dieser Schaltflächen, löst Sie eine der beiden Ereignisse, die `SearchBar` definiert:

- [`TextChanged`](xref:Xamarin.Forms.SearchBar.TextChanged) zusammen mit einem [ `TextChangedEventArgs` ](xref:Xamarin.Forms.TextChangedEventArgs) Objekt
- [`SearchButtonPressed`](xref:Xamarin.Forms.SearchBar.SearchButtonPressed)

Die [ **SearchBarDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SearchBarDemo) Beispiel veranschaulicht die `SearchBar`.

## <a name="date-and-time-selection"></a>Datum und Uhrzeit-Auswahl

Die [ `DatePicker` ](xref:Xamarin.Forms.DatePicker) und [ `TimePicker` ](xref:Xamarin.Forms.TimePicker) Ansichten implementieren plattformspezifisches-Steuerelemente, die dem Benutzer ermöglichen, geben Sie ein Datums- oder Zeitwert.

### <a name="the-datepicker"></a>"DatePicker"

[`DatePicker`](xref:Xamarin.Forms.DatePicker) definiert vier Eigenschaften:

- [`MinimumDate`](xref:Xamarin.Forms.DatePicker.MinimumDate) Der Typ `DateTime`, mit dem 1. Januar 1900 initialisiert
- [`MaximumDate`](xref:Xamarin.Forms.DatePicker.MaximumDate) Der Typ `DateTime`, mit dem 31 Dezember 2100 initialisiert
- [`Date`](xref:Xamarin.Forms.DatePicker.Date) Der Typ `DateTime`, initialisiert auf `DateTime.Today`
- [`Format`](xref:Xamarin.Forms.DatePicker.Format) Der Typ `string`, die .NET Formatzeichenfolge "d", das kurze Datumsmuster, initialisiert, was zu einer Datumsanzeige wie "7/20/1969" in den USA.

Sie können festlegen, die `DateTime` Eigenschaften in XAML, indem Sie die Eigenschaften als Eigenschaftenelemente Ausdrücken und das kurze Datum invarianter Kultur zu formatieren ("7/20/1969").   

Die [ **DaysBetweenDates** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/DaysBetweenDates) Beispiel berechnet die Anzahl der Tage zwischen zwei Datumsangaben, die vom Benutzer ausgewählt wurde.

### <a name="the-timepicker-or-is-it-a-timespanpicker"></a>TimePicker (oder eine TimeSpanPicker?)

[`TimePicker`](xref:Xamarin.Forms.TimePicker) definiert zwei Eigenschaften und keine Ereignisse:

- [`Time`](xref:Xamarin.Forms.TimePicker.Time) ist vom Typ `TimeSpan` statt `DateTime`, der angibt, der Zeitintervalls seit Mitternacht vergangenen
- [`Format`](xref:Xamarin.Forms.TimePicker.Format) Der Typ `string`, formatieren die Zeichenfolge, die mit "t", die kurzes Uhrzeitmuster, was zu einer Zeitanzeige, z. B. "1:45 PM" initialisiert, in den USA .NET.

Die [ **SetTimer** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SetTimer) Programm veranschaulicht, wie die `TimePicker` eine Zeit für einen Timer angeben. Das Programm funktioniert nur, wenn Sie es im Vordergrund beibehalten.

**SetTimer** auch veranschaulicht, wie mit der [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) -Methode der `Page` um ein Feld für die Warnmeldung anzuzeigen.

## <a name="related-links"></a>Verwandte Links

- [Kapitel 15 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch15-Apr2016.pdf)
- [Kapitel 15-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15)
- [Schieberegler](~/xamarin-forms/user-interface/slider.md)
- [Eingabe](~/xamarin-forms/user-interface/text/entry.md)
- [Editor](~/xamarin-forms/user-interface/text/editor.md)
- [DatePicker](~/xamarin-forms/user-interface/datepicker.md)
