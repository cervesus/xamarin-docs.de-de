---
title: Zusammenfassung der Kapitel 15. Die interaktive Schnittstelle
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung der Kapitel 15. Die interaktive Schnittstelle'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F54E86F4-1CDA-474E-9B09-242060C2C13D
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 5f96d2f4b619bbb10bb58e9b1b5dc7007c1ce888
ms.sourcegitcommit: ccbf914615c0ce6b3f308d930f7a77418aeb4dbc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2020
ms.locfileid: "77131100"
---
# <a name="summary-of-chapter-15-the-interactive-interface"></a>Zusammenfassung der Kapitel 15. Die interaktive Schnittstelle

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15)

In diesem Kapitel werden acht `View` Ableitungen erläutert, die eine Interaktion mit dem Benutzer ermöglichen.

## <a name="view-overview"></a>Übersicht anzeigen

Xamarin. Forms enthält 20 instanziier Bare Klassen, die von `View`, aber nicht `Layout`abgeleitet werden. Diese sechs haben in den vorangegangenen Kapiteln beschrieben wurde:

- `Label`: [ **Kapitel 2. Anatomie einer APP**](chapter02.md)
- `BoxView`: [ **Kapitel 3. Scrollen des Stapels**](chapter03.md)
- `Button`: [ **Kapitel 6. Schaltflächen Klicks**](chapter06.md)
- `Image`: [ **Kapitel 13. Bitmaps**](chapter13.md)
- `ActivityIndicator`: [ **Kapitel 13. Bitmaps**](chapter13.md)
- `ProgressBar`: [ **Kapitel 14. AbsoluteLayout**](chapter14.md)

Die acht Ansichten in diesem Kapitel ermöglichen effektiv den Benutzer für die Interaktion mit .NET Grunddatentypen:

|Datentyp|Sichten|
|--- |--- |
|`Double`|[`Slider`](xref:Xamarin.Forms.Slider), [`Stepper`](xref:Xamarin.Forms.Stepper)|
|`Boolean`|[`Switch`](xref:Xamarin.Forms.Switch)|
|`String`|[`Entry`](xref:Xamarin.Forms.Entry), [`Editor`](xref:Xamarin.Forms.Editor) [`SearchBar`](xref:Xamarin.Forms.SearchBar)|
|`DateTime`|[`DatePicker`](xref:Xamarin.Forms.DatePicker), [`TimePicker`](xref:Xamarin.Forms.TimePicker)|

Sie können diese Sichten als visuelle, interaktive Darstellungen der zugrunde liegenden Datentypen vorstellen. Dieses Konzept wird in den nächsten Kapiteln, Kapitel 16, näher untersucht [ **. Datenbindung**](chapter16.md).

Die verbleibenden sechs Ansichten werden in den folgenden Kapiteln erläutert:

- `WebView`: [ **Kapitel 16. Datenbindung**](chapter16.md)
- `Picker`: [ **Kapitel 19. Sammlungs Ansichten**](chapter19.md)
- `ListView`: [ **Kapitel 19. Sammlungs Ansichten**](chapter19.md)
- `TableView`: [ **Kapitel 19. Sammlungs Ansichten**](chapter19.md)
- `Map`: [ **Kapitel 28. Speicherort und** Zuordnungen](chapter28.md)
- `OpenGLView`: in diesem Buch nicht abgedeckt (und keine Unterstützung für Windows-Plattformen)

## <a name="slider-and-stepper"></a>Schieberegler und zugeordnetem

Sowohl [`Slider`](xref:Xamarin.Forms.Slider) als auch [`Stepper`](xref:Xamarin.Forms.Stepper) ermöglichen es dem Benutzer, einen numerischen Wert aus einem Bereich auszuwählen. Der `Slider` ist ein kontinuierlicher Bereich, während der `Stepper` diskrete Werte umfasst.

### <a name="slider-basics"></a>Schieberegler-Grundlagen

Der [`Slider`](xref:Xamarin.Forms.Slider) ist ein horizontaler Balken, der einen Wertebereich von einem minimalen Links bis zu einem Maximum auf der rechten Seite darstellt. Drei öffentliche Eigenschaften definiert:

- [`Value`](xref:Xamarin.Forms.Slider.Value) vom Typ `double`, Standardwert 0
- [`Minimum`](xref:Xamarin.Forms.Slider.Minimum) vom Typ `double`, Standardwert 0
- [`Maximum`](xref:Xamarin.Forms.Slider.Maximum) vom Typ `double`, Standardwert 1

Die bindbaren Eigenschaften, die diese Eigenschaften zurück. Stellen Sie sicher, dass sie konsistent sind:

- Für alle drei Eigenschaften stellt die [`coerceValue`](xref:Xamarin.Forms.BindableProperty.CoerceValueDelegate) Methode, die für die bindbare Eigenschaft angegeben wurde, sicher, dass `Value` zwischen `Minimum` und `Maximum`ist.
- Die [`validateValue`](xref:Xamarin.Forms.BindableProperty.ValidateValueDelegate) -Methode auf `MinimumProperty` gibt `false` zurück, wenn `Minimum` auf einen Wert größer als oder gleich `Maximum`festgelegt wird, und ähnlich für `MaximumProperty`. Das Zurückgeben von `false` aus der `validateValue`-Methode bewirkt, dass eine `ArgumentException` ausgelöst wird.

`Slider` löst das [`ValueChanged`](xref:Xamarin.Forms.Slider.ValueChanged) Ereignis mit einem [`ValueChangedEventArgs`](xref:Xamarin.Forms.ValueChangedEventArgs) Argument aus, wenn sich die `Value` Eigenschaft entweder Programm gesteuert oder wenn der Benutzer die `Slider`bearbeitet.

Das [**sliderdemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SliderDemo) -Beispiel veranschaulicht die einfache Verwendung der `Slider`.

### <a name="common-pitfalls"></a>Häufige Probleme

Sowohl im Code als auch in XAML werden die Eigenschaften `Minimum` und `Maximum` in der von Ihnen angegebenen Reihenfolge festgelegt. Stellen Sie sicher, dass diese Eigenschaften initialisiert werden, damit `Maximum` immer größer als `Minimum`ist. Andernfalls wird eine Ausnahme ausgelöst.

Wenn Sie die `Slider` Eigenschaften initialisieren, kann die `Value`-Eigenschaft geändert werden, und das `ValueChanged` Ereignis wird ausgelöst. Stellen Sie sicher, dass der `Slider` Ereignishandler nicht auf Sichten zugreift, die während der Seiten Initialisierung noch nicht erstellt wurden.

Das `ValueChanged` Ereignis wird während `Slider` Initialisierung nicht ausgelöst, es sei denn, die `Value`-Eigenschaft wird geändert. Sie können den `ValueChanged`-Handler direkt aus dem Code abrufen.

### <a name="slider-color-selection"></a>Schieberegler-Farbauswahl

Das [**rgbslider**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/RgbSliders) -Programm enthält drei `Slider` Elemente, mit deren Hilfe Sie eine Farbe interaktiv auswählen können, indem Sie Ihre RGB-Werte angeben:

[![Dreifacher Screenshot von R G B-Schiebereglern](images/ch15fg03-small.png "RGB-Schieberegler")](images/ch15fg03-large.png#lightbox "RGB-Schieberegler")

Das [**textfade**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/TextFade) -Beispiel verwendet zwei `Slider` Elemente, um zwei `Label` Elemente in eine `AbsoluteLayout` zu verschieben und eine in die andere zu blenden.

### <a name="the-stepper-difference"></a>Der Unterschied zugeordnetem

In der [`Stepper`](xref:Xamarin.Forms.Stepper) werden dieselben Eigenschaften und Ereignisse wie `Slider` definiert, aber die `Maximum`-Eigenschaft wird mit 100 initialisiert, und `Stepper` definiert eine vierte Eigenschaft:

- [`Increment`](xref:Xamarin.Forms.Stepper.Increment) vom Typ `double`, initialisiert mit 1

Der `Stepper` besteht aus zwei Schaltflächen **&ndash;** und **+** . Durch das Drücken von **&ndash;** wird `Value` verringert, indem mindestens `Minimum``Increment` wird. Durch das Drücken von **+** `Increment` wird `Value` um maximal `Maximum`vergrößert.

Dies wird durch das Beispiel " [**stepperdemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/StepperDemo) " veranschaulicht.

## <a name="switch-and-checkbox"></a>Switch und das Kontrollkästchen

Der [`Switch`](xref:Xamarin.Forms.Switch) ermöglicht dem Benutzer, einen booleschen Wert anzugeben.

### <a name="switch-basics"></a>Switch-Grundlagen

Der `Switch` besteht aus einer UMSCHALT Fläche, die ausgeschaltet und aktiviert werden kann. Die Klasse definiert eine Eigenschaft:

- [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) vom Typ `bool`

`Switch` definiert ein Ereignis:

- [`Toggled`](xref:Xamarin.Forms.Switch.Toggled) begleitet von einem [`ToggledEventArgs`](xref:Xamarin.Forms.ToggledEventArgs) -Objekt, das ausgelöst wird, wenn sich die `IsToggled`-Eigenschaft ändert.

Das [**switchdemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SwitchDemo) -Programm veranschaulicht die `Switch`.

### <a name="a-traditional-checkbox"></a>Eine herkömmliche Kontrollkästchen

Einige Entwickler bevorzugen möglicherweise eine herkömmlichere `CheckBox` für die `Switch`. Die [**xamarin. formsbook. Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) -Bibliothek enthält eine `CheckBox` Klasse, die von `ContentView`abgeleitet ist. `CheckBox` wird von den Dateien [CheckBox. XAML](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml) und [CheckBox.XAML.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml.cs) implementiert. in `CheckBox` werden drei Eigenschaften (`Text`, `FontSize`und `IsChecked`) und ein `CheckedChanged` Ereignis definiert.

Das [**checkboxdemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/CheckBoxDemo) -Beispiel veranschaulicht diese `CheckBox`.

## <a name="typing-text"></a>Eingeben von text

Xamarin.Forms definiert drei Ansichten, mit die den Benutzer Text eingeben und bearbeiten können:

- [`Entry`](xref:Xamarin.Forms.Entry) für eine einzelne Textzeile
- [`Editor`](xref:Xamarin.Forms.Editor) für mehrere Textzeilen
- [`SearchBar`](xref:Xamarin.Forms.SearchBar) für eine einzelne Textzeile zu Such Zwecken.

`Entry` und `Editor` von [`InputView`](xref:Xamarin.Forms.InputView)abgeleitet, das von `View`abgeleitet ist. `SearchBar` direkt von `View`abgeleitet.

### <a name="keyboard-and-focus"></a>Tastenkombinationen und den Fokus

Auf Smartphones und Tablets ohne physische Tastaturen bewirken die `Entry`-, `Editor`-und `SearchBar` Elemente, dass eine virtuelle Tastatur angezeigt wird. Das Vorhandensein dieser Tastatur auf dem Bildschirm bezieht sich auf den Eingabefokus. Für eine Sicht muss sowohl der [`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible) als auch [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled) Eigenschaften auf `true` festgelegt sein, um den Eingabefokus zu erhalten.

Mit dem Eingabefokus sind zwei Methoden, eine schreibgeschützte Eigenschaft und zwei Ereignisse beteiligt. Diese werden alle durch `VisualElement`definiert:

- Die [`Focus`](xref:Xamarin.Forms.VisualElement.Focus) -Methode versucht, den Eingabefokus auf ein Element festzulegen, und gibt bei erfolgreicher Ausführung `true` zurück.
- Die [`Unfocus`](xref:Xamarin.Forms.VisualElement.Unfocus) -Methode entfernt den Eingabefokus aus einem Element.
- Die [`IsFocused`](xref:Xamarin.Forms.VisualElement.IsFocused) schreibgeschützte Eigenschaft gibt an, ob das Element den Eingabefokus besitzt.
- Das [`Focused`](xref:Xamarin.Forms.VisualElement.Focused) Ereignis gibt an, wann ein Element den Eingabefokus erhält.
- Das Ereignis [`Unfocused`](xref:Xamarin.Forms.VisualElement.Unfocused) gibt an, wenn ein Element den Eingabefokus verliert.

### <a name="choosing-the-keyboard"></a>Auswählen der Tastaturfokus

Die [`InputView`](xref:Xamarin.Forms.InputView) Klasse, von der `Entry` und `Editor` abgeleitet werden, definiert nur eine Eigenschaft:

- [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard) vom Typ [`Keyboard`](xref:Xamarin.Forms.Keyboard)

Hiermit wird die Art der Tastatur, die angezeigt wird. Einige Tastaturen sind für URIs oder Zahlen optimiert.

Die `Keyboard`-Klasse ermöglicht das Definieren einer Tastatur mit einer statischen [`Keyboard.Create`](xref:Xamarin.Forms.Keyboard.Create(Xamarin.Forms.KeyboardFlags)) -Methode mit einem Argument vom Typ [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags), einer Enumeration mit den folgenden Bitflags:

- `None` auf 0 festgelegt.
- [`CapitalizeSentence`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeSentence) auf 1 festgelegt.
- [`Spellcheck`](xref:Xamarin.Forms.KeyboardFlags.Spellcheck) auf 2 festgelegt.
- [`Suggestions`](xref:Xamarin.Forms.KeyboardFlags.Suggestions) auf 4 festgelegt.
- [`All`](xref:Xamarin.Forms.KeyboardFlags.All) auf \xffffffff festgelegt.

Wenn Sie die mehrzeilige [`Editor`](xref:Xamarin.Forms.Editor) verwenden, wenn ein Absatz oder mehr Text erwartet wird, ist das Aufrufen von `Keyboard.Create` ein guter Ansatz für die Auswahl einer Tastatur. Für das einzeilige [`Entry`](xref:Xamarin.Forms.Entry)sind die folgenden statischen schreibgeschützten Eigenschaften von `Keyboard` nützlich:

- [`Default`](xref:Xamarin.Forms.Keyboard.Default)
- [`Text`](xref:Xamarin.Forms.Keyboard.Text)
- [`Chat`](xref:Xamarin.Forms.Keyboard.Chat)
- [`Url`](xref:Xamarin.Forms.Keyboard.Url)
- [`Email`](xref:Xamarin.Forms.Keyboard.Email)
- [`Telephone`](xref:Xamarin.Forms.Keyboard.Telephone)
- [`Numeric`](xref:Xamarin.Forms.Keyboard.Numeric) für positive Zahlen mit oder ohne Dezimaltrennzeichen.

Der [`KeyboardTypeConverter`](xref:Xamarin.Forms.KeyboardTypeConverter) ermöglicht das Angeben dieser Eigenschaften in XAML, wie vom [**entrytastaturen**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/EntryKeyboards) -Programm veranschaulicht.

### <a name="entry-properties-and-events"></a>Eigenschaften und Ereignisse

Der einzeilige [`Entry`](xref:Xamarin.Forms.Entry) definiert die folgenden Eigenschaften:

- [`Text`](xref:Xamarin.Forms.InputView.Text) vom Typ `string`, wird der Text, der im `Entry`
- [`TextColor`](xref:Xamarin.Forms.InputView.TextColor) vom Typ `Color`
- [`FontFamily`](xref:Xamarin.Forms.Entry.FontFamily) vom Typ `string`
- [`FontSize`](xref:Xamarin.Forms.Entry.FontSize) vom Typ `double`
- [`FontAttributes`](xref:Xamarin.Forms.Entry.FontAttributes) vom Typ `FontAttributes`
- [`IsPassword`](xref:Xamarin.Forms.Entry.IsPassword) vom Typ `bool`, der bewirkt, dass Zeichen maskiert werden.
- [`Placeholder`](xref:Xamarin.Forms.InputView.Placeholder) vom Typ `string`, für dimly farbigen Text, der in der `Entry` angezeigt wird, bevor etwas eingegeben wird
- [`PlaceholderColor`](xref:Xamarin.Forms.InputView.PlaceholderColor) vom Typ `Color`

Der `Entry` definiert auch zwei Ereignisse:

- [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) mit einem [`TextChangedEventArgs`](xref:Xamarin.Forms.TextChangedEventArgs) Objekt, das ausgelöst wird, wenn sich die `Text`-Eigenschaft ändert.
- [`Completed`](xref:Xamarin.Forms.Entry.Completed), wird ausgelöst, wenn der Benutzer fertig ist und die Tastatur verworfen wurde. Der Benutzer wird der Abschluss auf eine plattformspezifische Weise

Im Beispiel [**quadraticequations**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/QuadaticEquations) werden diese beiden Ereignisse veranschaulicht.

### <a name="the-editor-difference"></a>Die Editor-Differenz

Das mehrzeilige [`Editor`](xref:Xamarin.Forms.Editor) definiert die gleichen `Text` und `Font` Eigenschaften wie `Entry`, aber nicht die anderen Eigenschaften. `Editor` definiert auch die gleichen zwei Eigenschaften wie `Entry`.

[**Justnotes**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/JustNotes) ist ein kostenloses Programm, das die Inhalte der `Editor`speichert und wiederherstellt.

### <a name="the-searchbar"></a>Die SearchBar

Der [`SearchBar`](xref:Xamarin.Forms.SearchBar) wird nicht von `InputView`abgeleitet und verfügt daher nicht über eine `Keyboard`-Eigenschaft. Es verfügt jedoch über alle Eigenschaften von `Text`, `Font`und `Placeholder`, die von `Entry` definiert werden. Außerdem werden in `SearchBar` drei zusätzliche Eigenschaften definiert:

- [`CancelButtonColor`](xref:Xamarin.Forms.SearchBar.CancelButtonColor) vom Typ `Color`
- [`SearchCommand`](xref:Xamarin.Forms.SearchBar.SearchCommand) vom Typ [`ICommand`](xref:System.Windows.Input.ICommand) für die Verwendung mit Daten Bindungen und MVVM
- [`SearchCommandParameter`](xref:Xamarin.Forms.SearchBar.SearchCommandParameter) vom Typ "`Object`" für die Verwendung mit `SearchCommand`

Die plattformspezifischen Abbrechen Löschvorgängen von Schaltfläche, den Text. Der `SearchBar` verfügt auch über eine plattformspezifische Such Schaltfläche. Wenn Sie eine der beiden Schaltflächen drücken, wird eines der beiden Ereignisse ausgelöst, die `SearchBar` definiert:

- [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) begleitet von einem [`TextChangedEventArgs`](xref:Xamarin.Forms.TextChangedEventArgs) Objekt
- [`SearchButtonPressed`](xref:Xamarin.Forms.SearchBar.SearchButtonPressed)

Das Beispiel [**searchbardemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SearchBarDemo) veranschaulicht die `SearchBar`.

## <a name="date-and-time-selection"></a>Datum und Uhrzeit-Auswahl

In den [`DatePicker`](xref:Xamarin.Forms.DatePicker) -und [`TimePicker`](xref:Xamarin.Forms.TimePicker) Sichten werden plattformspezifische Steuerelemente implementiert, mit denen der Benutzer ein Datum oder eine Uhrzeit angeben kann.

### <a name="the-datepicker"></a>"DatePicker"

[`DatePicker`](xref:Xamarin.Forms.DatePicker) definiert vier Eigenschaften:

- [`MinimumDate`](xref:Xamarin.Forms.DatePicker.MinimumDate) vom Typ `DateTime`, initialisiert mit dem 1. Januar 1900
- [`MaximumDate`](xref:Xamarin.Forms.DatePicker.MaximumDate) vom Typ `DateTime`, initialisiert am 31. Dezember 2100
- [`Date`](xref:Xamarin.Forms.DatePicker.Date) vom Typ `DateTime`, initialisiert mit `DateTime.Today`
- [`Format`](xref:Xamarin.Forms.DatePicker.Format) vom Typ `string`die .net-Formatierungs Zeichenfolge, die mit "d" initialisiert wurde, das kurze Datums Muster, was in den USA zu einer Datumsanzeige wie "7/20/1969" führt.

Sie können die `DateTime` Eigenschaften in XAML festlegen, indem Sie die Eigenschaften als Eigenschafts Elemente Ausdrücken und das Format der Kultur invarianten Kurzform ("7/20/1969") verwenden.   

Das [**daysbetweenumdates**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/DaysBetweenDates) -Beispiel berechnet die Anzahl der Tage zwischen zwei vom Benutzer ausgewählten Datumsangaben.

### <a name="the-timepicker-or-is-it-a-timespanpicker"></a>TimePicker (oder eine TimeSpanPicker?)

[`TimePicker`](xref:Xamarin.Forms.TimePicker) definiert zwei Eigenschaften und keine Ereignisse:

- [`Time`](xref:Xamarin.Forms.TimePicker.Time) vom Typ `TimeSpan` anstelle von `DateTime`ist und die seit Mitternacht verstrichene Zeit angibt.
- [`Format`](xref:Xamarin.Forms.TimePicker.Format) vom Typ `string`, die .net-Formatierungs Zeichenfolge, die mit "t" initialisiert wurde, das Kurze Zeitmuster, was in den USA zu einer Zeitanzeige wie "1:45 pm" führt.

Das [**Programm "** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SetTimer) " ist ein Beispiel für die Verwendung der `TimePicker`, um eine Zeit für einen Timer anzugeben. Das Programm funktioniert nur, wenn Sie es im Vordergrund beibehalten.

Mit der [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) -Methode von `Page` können **Sie auch ein** Warnungs Feld anzeigen.

## <a name="related-links"></a>Verwandte Links

- [Kapitel 15 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch15-Apr2016.pdf)
- [Kapitel 15 Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15)
- [Slider](~/xamarin-forms/user-interface/slider.md)
- [Eingabe](~/xamarin-forms/user-interface/text/entry.md)
- [Editor](~/xamarin-forms/user-interface/text/editor.md)
- [DatePicker](~/xamarin-forms/user-interface/datepicker.md)
