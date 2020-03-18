---
title: Zusammenfassung von Kapitel 15. Die interaktive Benutzeroberfläche
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung von Kapitel 15. Die interaktive Benutzeroberfläche'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F54E86F4-1CDA-474E-9B09-242060C2C13D
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 5f96d2f4b619bbb10bb58e9b1b5dc7007c1ce888
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "77131100"
---
# <a name="summary-of-chapter-15-the-interactive-interface"></a>Zusammenfassung von Kapitel 15. Die interaktive Benutzeroberfläche

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15)

In diesem Kapitel werden acht `View`-Ableitungen besprochen, die eine Interaktion mit dem Benutzer ermöglichen.

## <a name="view-overview"></a>„View“ (Ansicht) – Übersicht

Xamarin.Forms enthält 20 instanziierbare Klassen, die von `View`, aber nicht von `Layout` abgeleitet werden. Sechs davon wurden in den vorherigen Kapiteln behandelt:

- `Label`: [**Kapitel 2, „Aufbau einer App“** ](chapter02.md)
- `BoxView`: [**Kapitel 3, „Scrollen im Stapel“** ](chapter03.md)
- `Button`: [**Kapitel 6, „Schaltflächenklicks“** ](chapter06.md)
- `Image`: [**Kapitel 13, „Bitmaps“** ](chapter13.md)
- `ActivityIndicator`: [**Kapitel 13, „Bitmaps“** ](chapter13.md)
- `ProgressBar`: [**Kapitel 14, „AbsoluteLayout“** ](chapter14.md)

Die acht Ansichten in diesem Kapitel gestatten dem Benutzer effektiv die Interaktion mit grundlegenden .NET-Datentypen:

|Datentyp|Ansichten|
|--- |--- |
|`Double`|[`Slider`](xref:Xamarin.Forms.Slider), [`Stepper`](xref:Xamarin.Forms.Stepper)|
|`Boolean`|[`Switch`](xref:Xamarin.Forms.Switch)|
|`String`|[`Entry`](xref:Xamarin.Forms.Entry), [`Editor`](xref:Xamarin.Forms.Editor), [`SearchBar`](xref:Xamarin.Forms.SearchBar)|
|`DateTime`|[`DatePicker`](xref:Xamarin.Forms.DatePicker), [`TimePicker`](xref:Xamarin.Forms.TimePicker)|

Sie können sich diese Ansichten als visuell interaktive Darstellungen der zugrunde liegenden Datentypen vorstellen. Dieses Konzept wird im nächsten Kapitel, [**Kapitel 16, „Datenbindung“** ](chapter16.md), näher untersucht.

Die verbleibenden sechs Ansichten werden in den folgenden Kapiteln behandelt:

- `WebView`: [**Kapitel 16, „Datenbindung“** ](chapter16.md)
- `Picker`: [**Kapitel 19, „Sammlungsansichten“** ](chapter19.md)
- `ListView`: [**Kapitel 19, „Sammlungsansichten“** ](chapter19.md)
- `TableView`: [**Kapitel 19, „Sammlungsansichten“** ](chapter19.md)
- `Map`: [**Kapitel 28, „Position und Karten“** ](chapter28.md)
- `OpenGLView`: wird in diesem Buch nicht behandelt (und keine Unterstützung für Windows-Plattformen).

## <a name="slider-and-stepper"></a>„Slider“ (Schieberegler) und „Stepper“ (Schritte)

Sowohl [`Slider`](xref:Xamarin.Forms.Slider) als auch [`Stepper`](xref:Xamarin.Forms.Stepper) gestatten Benutzern die Auswahl eines numerischen Werts aus einem Bereich. `Slider` ist ein fortlaufender Bereich, während `Stepper` diskrete Werte umfasst.

### <a name="slider-basics"></a>„Slider“-Grundlagen

[`Slider`](xref:Xamarin.Forms.Slider) ist eine horizontale Leiste, die einen Wertebereich von einem Mindestwert (links) bis zu einem Höchstwert (rechts) darstellt. Er definiert drei öffentliche Eigenschaften:

- [`Value`](xref:Xamarin.Forms.Slider.Value) vom Typ `double`, der Standardwert ist 0.
- [`Minimum`](xref:Xamarin.Forms.Slider.Minimum) vom Typ `double`, der Standardwert ist 0.
- [`Maximum`](xref:Xamarin.Forms.Slider.Maximum) vom Typ `double`, der Standardwert ist 1.

Die bindbaren Eigenschaften, die diese Eigenschaften unterstützen, stellen sicher dass sie konsistent sind:

- Für alle drei Eigenschaften stellt die für die bindbare Eigenschaft angegebene [`coerceValue`](xref:Xamarin.Forms.BindableProperty.CoerceValueDelegate)-Methode sicher, dass `Value` zwischen `Minimum` und `Maximum` liegt.
- Die [`validateValue`](xref:Xamarin.Forms.BindableProperty.ValidateValueDelegate)-Methode mit `MinimumProperty` gibt `false` zurück, wenn `Minimum` auf einen Wert größer als oder gleich `Maximum` festgelegt wird, und ähnliches gilt für `MaximumProperty`. Die Rückgabe von `false` von der `validateValue`-Methode führt dazu, dass eine `ArgumentException` ausgelöst wird.

`Slider` löst das [`ValueChanged`](xref:Xamarin.Forms.Slider.ValueChanged)-Ereignis mit einem [`ValueChangedEventArgs`](xref:Xamarin.Forms.ValueChangedEventArgs)-Argument aus, wenn sich die `Value`-Eigenschaft programmgesteuert oder durch Manipulation von `Slider` durch den Benutzer ändert.

Im [**SliderDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SliderDemo)-Beispiel wird die einfache Verwendung von `Slider` veranschaulicht.

### <a name="common-pitfalls"></a>Häufige Probleme

Sowohl in Code als auch in XAML werden die Eigenschaften `Minimum` und `Maximum` in der von Ihnen angegebenen Reihenfolge festgelegt. Stellen Sie sicher, dass Sie diese Eigenschaften initialisieren, sodass `Maximum` immer größer als `Minimum` ist. Andernfalls wird eine Ausnahme ausgelöst.

Wenn Sie die `Slider`-Eigenschaften initialisieren, kann dies dazu führen, dass sich die `Value`-Eigenschaft ändert und dass das `ValueChanged`-Ereignis ausgelöst wird. Sie sollten sicherstellen, dass der `Slider`-Ereignishandler nicht auf Ansichten zugreift, die während der Seiteninitialisierung noch nicht erstellt wurden.

Das `ValueChanged`-Ereignis wird während der `Slider`-Initialisierung nur dann ausgelöst, wenn sich die `Value`-Eigenschaft ändert. Sie können den `ValueChanged`-Handler direkt im Code aufrufen.

### <a name="slider-color-selection"></a>Farbauswahl für „Slider“

Das [**RgbSliders**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/RgbSliders)-Programm enthält drei `Slider`-Elemente, mit denen Sie interaktiv eine Farbe auswählen können, indem Sie ihre RGB-Werte angeben:

[![Dreifacher Screenshot des R-G-B-Schiebereglers (Slider)](images/ch15fg03-small.png "RGB-Schieberegler")](images/ch15fg03-large.png#lightbox "RGB-Schieberegler")

Im [**TextFade**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/TextFade)-Beispiel werden zwei `Slider`-Elemente verwendet, um zwei `Label`-Elemente in einem `AbsoluteLayout` zu verschieben und dabei das eine in das andere überzublenden.

### <a name="the-stepper-difference"></a>Der „Stepper“-Unterschied

Der [`Stepper`](xref:Xamarin.Forms.Stepper) definiert dieselben Eigenschaften und Ereignisse wie der `Slider`, aber die `Maximum`-Eigenschaft wird mit 100 initialisiert, und `Stepper` definiert eine vierte Eigenschaft:

- [`Increment`](xref:Xamarin.Forms.Stepper.Increment) vom Typ `double`, initialisiert mit 1.

Visuell besteht der `Stepper` aus zwei Schaltflächen, die mit **&ndash;** und **+** bezeichnet sind. Drücken auf **&ndash;** verringert `Value` um `Increment` bis auf ein Minimum von `Minimum`. Drücken auf **+** erhöht `Value` um `Increment` bis auf ein Maximum von `Maximum`.

Dieser Vorgang wird im [**StepperDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/StepperDemo)-Beispiel veranschaulicht.

## <a name="switch-and-checkbox"></a>„Switch“ (Schalter) und „CheckBox“ (Kontrollkästchen)

[`Switch`](xref:Xamarin.Forms.Switch) ermöglicht dem Benutzer die Angabe eines booleschen Werts.

### <a name="switch-basics"></a>„Switch“-Grundlagen

Visuell besteht der `Switch` aus einer Umschaltfläche, die ein- und ausgeschaltet werden kann. Die Klasse definiert eine Eigenschaft:

- [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) vom Typ `bool`

`Switch` definiert ein Ereignis:

- [`Toggled`](xref:Xamarin.Forms.Switch.Toggled), begleitet von einem [`ToggledEventArgs`](xref:Xamarin.Forms.ToggledEventArgs)-Objekt, das ausgelöst wird, wenn sich die `IsToggled`-Eigenschaft ändert.

Das [**SwitchDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SwitchDemo)-Programm veranschaulicht den `Switch`.

### <a name="a-traditional-checkbox"></a>Ein traditionelles Kontrollkästchen (CheckBox)

Manche Entwickler ziehen vielleicht ein traditionelleres `CheckBox` dem `Switch` vor. Die Bibliothek [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) enthält eine `CheckBox`-Klasse, die von `ContentView` abgeleitet wird. `CheckBox` wird mit den Dateien [CheckBox.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml) und [CheckBox.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml.cs) implementiert. `CheckBox` definiert drei Eigenschaften (`Text`, `FontSize` und `IsChecked`) und ein `CheckedChanged`-Ereignis.

Dieses `CheckBox` wird im [**CheckBoxDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/CheckBoxDemo)-Beispiel veranschaulicht.

## <a name="typing-text"></a>Eingabe von Text

Xamarin.Forms definiert drei Ansichten, in denen der Benutzer Text eingeben und bearbeiten kann:

- [`Entry`](xref:Xamarin.Forms.Entry) für eine einzelne Textzeile.
- [`Editor`](xref:Xamarin.Forms.Editor) für mehrere Textzeilen.
- [`SearchBar`](xref:Xamarin.Forms.SearchBar) für eine einzelne Textzeile zu Suchzwecken.

`Entry` und `Editor` werden von [`InputView`](xref:Xamarin.Forms.InputView) abgeleitet, die wiederum von `View` abgeleitet wird. `SearchBar` wird direkt von `View` abgeleitet.

### <a name="keyboard-and-focus"></a>Tastatur und Fokus

Bei Telefonen und Tablets ohne physische Tastaturen sorgen die Elemente `Entry`, `Editor` und `SearchBar` dafür, dass eine virtuelle Tastatur angezeigt wird. Das Vorhandensein dieser Tastatur am Bildschirm steht im Zusammenhang mit dem Eingabefokus. Für eine Ansicht müssen ihre beiden Eigenschaften [`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible) und [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled) auf `true` festgelegt sein, um den Eingabefokus zu erhalten.

Zwei Methoden, eine schreibgeschützte Eigenschaft und zwei Ereignisse sind am Eingabefokus beteiligt. Diese werden alle von `VisualElement` definiert.

- Die [`Focus`](xref:Xamarin.Forms.VisualElement.Focus)-Methode versucht, den Eingabefokus auf ein Element festzulegen, und gibt bei Erfolg `true` zurück
- Die [`Unfocus`](xref:Xamarin.Forms.VisualElement.Unfocus)-Methode entfernt den Eingabefokus von einem Element.
- Die schreibgeschützte [`IsFocused`](xref:Xamarin.Forms.VisualElement.IsFocused)-Eigenschaft zeigt an, ob das Element den Eingabefokus hat.
- Das [`Focused`](xref:Xamarin.Forms.VisualElement.Focused)-Ereignis zeigt an, wenn ein Element den Eingabefokus erhält.
- Das [`Unfocused`](xref:Xamarin.Forms.VisualElement.Unfocused)-Ereignis zeigt an, wenn ein Element den Eingabefokus verliert.

### <a name="choosing-the-keyboard"></a>Auswählen der Tastatur

Die [`InputView`](xref:Xamarin.Forms.InputView)-Klasse, von der `Entry` und `Editor` abgeleitet werden, definiert nur eine Eigenschaft:

- [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard) vom Typ [`Keyboard`](xref:Xamarin.Forms.Keyboard)

Diese zeigt den Typ der Tastatur an, die angezeigt wird. Manche Tastaturen sind für URIs oder Zahlen optimiert.

Mit der `Keyboard`-Klasse können Sie eine Tastatur mit einer statischen [`Keyboard.Create`](xref:Xamarin.Forms.Keyboard.Create(Xamarin.Forms.KeyboardFlags))-Methode mit einem Argument vom Typ [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags) definieren, einer Enumeration mit den folgenden Bitflags:

- `None`, auf 0 festgelegt.
- [`CapitalizeSentence`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeSentence), auf 1 festgelegt.
- [`Spellcheck`](xref:Xamarin.Forms.KeyboardFlags.Spellcheck), auf 2 festgelegt.
- [`Suggestions`](xref:Xamarin.Forms.KeyboardFlags.Suggestions), auf 4 festgelegt.
- [`All`](xref:Xamarin.Forms.KeyboardFlags.All), auf „\xFFFFFFFF“ festgelegt.

Bei Verwendung des mehrzeiligen [`Editor`](xref:Xamarin.Forms.Editor), wenn ein Absatz oder mehr Text erwartet wird, stellt das Aufrufen von `Keyboard.Create` einen guten Ansatz zur Auswahl einer Tastatur dar. Für den einzeiligen [`Entry`](xref:Xamarin.Forms.Entry) sind die folgenden statischen, schreibgeschützten Eigenschaften von `Keyboard` nützlich:

- [`Default`](xref:Xamarin.Forms.Keyboard.Default)
- [`Text`](xref:Xamarin.Forms.Keyboard.Text)
- [`Chat`](xref:Xamarin.Forms.Keyboard.Chat)
- [`Url`](xref:Xamarin.Forms.Keyboard.Url)
- [`Email`](xref:Xamarin.Forms.Keyboard.Email)
- [`Telephone`](xref:Xamarin.Forms.Keyboard.Telephone)
- [`Numeric`](xref:Xamarin.Forms.Keyboard.Numeric) für positive Zahlen mit oder ohne Dezimaltrennzeichen.

Der [`KeyboardTypeConverter`](xref:Xamarin.Forms.KeyboardTypeConverter) gestattet das Angeben dieser Eigenschaften in XAML, wie vom [**EntryKeyboards**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/EntryKeyboards)-Programm veranschaulicht.

### <a name="entry-properties-and-events"></a>„Entry“-Eigenschaften und -Ereignisse

Der einzeilige [`Entry`](xref:Xamarin.Forms.Entry) definiert die folgenden Eigenschaften:

- [`Text`](xref:Xamarin.Forms.InputView.Text) vom Typ `string`; der im `Entry` angezeigte Text.
- [`TextColor`](xref:Xamarin.Forms.InputView.TextColor) vom Typ `Color`
- [`FontFamily`](xref:Xamarin.Forms.Entry.FontFamily) vom Typ `string`
- [`FontSize`](xref:Xamarin.Forms.Entry.FontSize) vom Typ `double`
- [`FontAttributes`](xref:Xamarin.Forms.Entry.FontAttributes) vom Typ `FontAttributes`
- [`IsPassword`](xref:Xamarin.Forms.Entry.IsPassword) vom Typ `bool`; bewirkt die Maskierung von Zeichen.
- [`Placeholder`](xref:Xamarin.Forms.InputView.Placeholder) vom Typ `string` für blass eingefärbten Text, der in `Entry` vor jeglicher Eingabe angezeigt wird.
- [`PlaceholderColor`](xref:Xamarin.Forms.InputView.PlaceholderColor) vom Typ `Color`

`Entry` definiert außerdem zwei Ereignisse:

- [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) mit einem [`TextChangedEventArgs`](xref:Xamarin.Forms.TextChangedEventArgs)-Objekt, das immer ausgelöst wird, wenn sich die `Text`-Eigenschaft ändert.
- [`Completed`](xref:Xamarin.Forms.Entry.Completed), das ausgelöst wird, wenn der Benutzer fertig ist und die Tastatur geschlossen wird. Der Benutzer zeigt die Fertigstellung auf plattformspezifische Weise an.

Im [**QuadraticEquations**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/QuadaticEquations)-Beispiel werden diese beiden Ereignisse veranschaulicht.

### <a name="the-editor-difference"></a>Der „Editor“-Unterschied

Der mehrzeilige [`Editor`](xref:Xamarin.Forms.Editor) definiert dieselben Eigenschaften `Text` und `Font` wie `Entry`, aber nicht die anderen Eigenschaften. `Editor` definiert ferner dieselben zwei Eigenschaften wie `Entry`.

[**JustNotes**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/JustNotes) ist ein Programm zum Erstellen von Freihandnotizen, das den Inhalt von `Editor` speichert und wiederherstellt.

### <a name="the-searchbar"></a>Die „SearchBar“ (Suchleiste)

[`SearchBar`](xref:Xamarin.Forms.SearchBar) wird nicht von `InputView` abgeleitet, weshalb sie keine `Keyboard`-Eigenschaft besitzt. Sie besitzt aber all die Eigenschaften `Text`, `Font` und `Placeholder`, die `Entry` definiert. Darüber hinaus definiert `SearchBar` drei zusätzliche Eigenschaften:

- [`CancelButtonColor`](xref:Xamarin.Forms.SearchBar.CancelButtonColor) vom Typ `Color`
- [`SearchCommand`](xref:Xamarin.Forms.SearchBar.SearchCommand) vom Typ [`ICommand`](xref:System.Windows.Input.ICommand) zur Verwendung mit Datenbindungen und MVVM.
- [`SearchCommandParameter`](xref:Xamarin.Forms.SearchBar.SearchCommandParameter) vom Typ `Object` zur Verwendung mit `SearchCommand`.

Die plattformspezifische Schaltfläche „Abbrechen“ löscht den Text. Die `SearchBar` besitzt auch eine plattformspezifische Schaltfläche „Suchen“. Durch das Drücken einer der beiden Schaltflächen wird eins der beiden Ereignisse ausgelöst, die `SearchBar` definiert:

- [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged), begleitet von einem [`TextChangedEventArgs`](xref:Xamarin.Forms.TextChangedEventArgs)-Objekt.
- [`SearchButtonPressed`](xref:Xamarin.Forms.SearchBar.SearchButtonPressed)

Die `SearchBar` wird im [**SearchBarDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SearchBarDemo)-Beispiel veranschaulicht.

## <a name="date-and-time-selection"></a>Datums- und Zeitauswahl

Die Ansichten [`DatePicker`](xref:Xamarin.Forms.DatePicker) und [`TimePicker`](xref:Xamarin.Forms.TimePicker) implementieren plattformspezifische Steuerelemente, die Benutzern das Angeben eines Datums oder einer Uhrzeit gestatten.

### <a name="the-datepicker"></a>Die Datumsauswahl (DatePicker)

[`DatePicker`](xref:Xamarin.Forms.DatePicker) definiert vier Eigenschaften:

- [`MinimumDate`](xref:Xamarin.Forms.DatePicker.MinimumDate) vom Typ `DateTime`, initialisiert mit 1. Januar 1900.
- [`MaximumDate`](xref:Xamarin.Forms.DatePicker.MaximumDate) vom Typ `DateTime`, initialisiert mit 31. Dezember 2100.
- [`Date`](xref:Xamarin.Forms.DatePicker.Date) vom Typ `DateTime`, initialisiert mit `DateTime.Today`.
- [`Format`](xref:Xamarin.Forms.DatePicker.Format) vom Typ `string`, mit der .NET-Formatierungszeichenfolge initialisiert mit „d“, dem kurzen Datumsmuster, was zu einer Datumsanzeige wie „20.7.1969“ in Deutschland führt.

Sie können die `DateTime`-Eigenschaften in XAML festlegen, in dem Sie die Eigenschaften als Eigenschaftselemente ausdrücken und das kulturinvariante kurze Datumsformat verwenden („20.7.1969“).   

Im [**DaysBetweenDates**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/DaysBetweenDates)-Beispiel wird die Anzahl der Tage zwischen zwei vom Benutzer ausgewählten Daten berechnet.

### <a name="the-timepicker-or-is-it-a-timespanpicker"></a>Die Uhrzeitauswahl (TimePicker) (oder ist es eine Zeitraumauswahl (TimeSpanPicker)?)

[`TimePicker`](xref:Xamarin.Forms.TimePicker) definiert zwei Eigenschaften und keine Ereignisse:

- [`Time`](xref:Xamarin.Forms.TimePicker.Time) ist eher vom Typ `TimeSpan` als `DateTime`, was die seit Mitternacht verstrichene Zeit angibt.
- [`Format`](xref:Xamarin.Forms.TimePicker.Format) vom Typ `string`, mit der .NET-Formatierungszeichenfolge initialisiert mit „t“, dem kurzen Uhrzeitmuster, was zu einer Uhrzeitanzeige wie „13:45“ in Deutschland führt.

Das [**SetTimer**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SetTimer)-Programm veranschaulicht, wie Sie `TimePicker` verwenden, um eine Uhrzeit für einen Timer anzugeben. Das Programm funktioniert nur, wenn Sie es im Vordergrund halten.

**SetTimer** veranschaulicht auch die Verwendung der [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String))-Methode von `Page` zur Anzeige eines Warnfelds.

## <a name="related-links"></a>Verwandte Links

- [Kapitel 15 – vollständiger Text (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch15-Apr2016.pdf)
- [Kapitel 15 – Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15)
- [Schieberegler](~/xamarin-forms/user-interface/slider.md)
- [Eingabe](~/xamarin-forms/user-interface/text/entry.md)
- [Editor](~/xamarin-forms/user-interface/text/editor.md)
- [DatePicker](~/xamarin-forms/user-interface/datepicker.md)
