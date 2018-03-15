---
title: Zusammenfassung der in Kapitel 15. Die interaktive-Schnittstelle
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F54E86F4-1CDA-474E-9B09-242060C2C13D
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 09b999771ec500409e40dc2aef671045bf9f5565
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2018
---
# <a name="summary-of-chapter-15-the-interactive-interface"></a>Zusammenfassung der in Kapitel 15. Die interaktive-Schnittstelle

In diesem Kapitel werden acht `View` ableitungen, die Interaktion mit dem Benutzer zu ermöglichen.

## <a name="view-overview"></a>View-Übersicht

Xamarin.Forms enthält 20 instanziierbare abgeleitete Klassen `View` , aber nicht `Layout`. Diese sechs haben in den vorherigen Kapiteln behandelt wurde:

- `Label`: [ **Chapter 2. Aufbau einer App**](chapter02.md)
- `BoxView`: [ **Kapitel 3. Durchführen eines Bildlaufs im Stapel**](chapter03.md)
- `Button`: [ **Kapitel 6. Auf eine Schaltfläche**](chapter06.md)
- `Image`: [ **Kapitel 13. Bitmaps**](chapter13.md)
- `ActivityIndicator`: [ **Kapitel 13. Bitmaps**](chapter13.md)
- `ProgressBar`: [ **Kapitel 14. AbsoluteLayout**](chapter14.md)

Die acht-Ansichten in diesem Kapitel können effektiv den Benutzer für die Interaktion mit .NET Grunddatentypen:

|Datentyp|Ansichten|
|--- |--- |
|`Double`|[`Slider`](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/), [`Stepper`](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/)|
|`Boolean`|[`Switch`](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/)|
|`String`|[`Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/), [`Editor`](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/), [`SearchBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/)|
|`DateTime`|[`DatePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/), [`TimePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/)|

Sie können diesen Ansichten als visuelle interaktive Darstellungen der zugrunde liegenden Datentypen vorstellen. Dieses Konzept wird im nächsten Kapitel mehr untersucht [ **Kapitel 16. Die Datenbindung**](chapter16.md).

Die verbleibenden sechs Ansichten werden in den folgenden Kapiteln behandelt:

- `WebView`: [ **Kapitel 16. Datenbindung**](chapter16.md)
- `Picker`: [ **Kapitel 19. Auflistungsansichten**](chapter19.md)
- `ListView`: [ **Kapitel 19. Auflistungsansichten**](chapter19.md)
- `TableView`: [ **Kapitel 19. Auflistungsansichten**](chapter19.md)
- `Map`: [ **Kapitel 28. Speicherort und Karten**](chapter28.md)
- `OpenGLView`: Nicht abgedeckt werden in diesem Buch (und keine Unterstützung für Windows-Plattformen)

## <a name="slider-and-stepper"></a>Schieberegler und zugeordnetem

Beide [ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) und [ `Stepper` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/) ermöglicht dem Benutzer einen numerischen Wert aus einem Bereich auswählen. Die `Slider` ist eine Breite Palette während der `Stepper` diskrete Werte umfasst.

### <a name="slider-basics"></a>Schieberegler-Grundlagen

Die [ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) ist ein horizontales Balkendiagramm, das einen Bereich von Werten von einem minimalen auf der linken Seite auf ein Maximum auf der rechten Seite darstellt. Drei öffentliche Eigenschaften definiert:

- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Value/) Der Typ `double`, Standardwert 0
- [`Minimum`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Minimum/) Der Typ `double`, Standardwert 0
- [`Maximum`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Maximum/) Der Typ `double`, default-Wert von 1

Die bindungsfähigen Eigenschaften, die diese Eigenschaften zu sichern, stellen Sie sicher, dass beide konsistent sind:

- Für alle drei Eigenschaften die [ `coerceValue` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty+CoerceValueDelegate/) Methode angegeben, für die bindbare Eigenschaft wird, dass sichergestellt `Value` zwischen `Minimum` und `Maximum`.
- Die [ `validateValue` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty+ValidateValueDelegate/) Methode `MinimumProperty` gibt `false` Wenn `Minimum` ist auf einen Wert festgelegt wird, größer als oder gleich `Maximum`, und Ähnliches gilt für `MaximumProperty`. Zurückgeben von `false` aus der `validateValue` -Methode nimmt eine `ArgumentException` ausgelöst werden soll.

`Slider` wird ausgelöst der [ `ValueChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Slider.ValueChanged/) Ereignis "mit" eine [ `ValueChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ValueChangedEventArgs/) Argument bei der `Value` eigenschaftsänderungen, entweder programmgesteuert oder wenn der Benutzer bearbeitet die `Slider`.

Die [ **SliderDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SliderDemo) Beispiel veranschaulicht die einfache Verwendung von der `Slider`.

### <a name="common-pitfalls"></a>Häufige Probleme

Sowohl im Code als auch in XAML wird die `Minimum` und `Maximum` in den von Ihnen angegebenen Reihenfolge festgelegt sind. Achten Sie darauf, um diese Eigenschaften zu initialisieren, damit `Maximum` ist immer größer als `Minimum`. Andernfalls wird eine Ausnahme ausgelöst.

Initialisieren der `Slider` Eigenschaften können dazu führen, dass die `Value` zu ändernden Eigenschaft und die `ValueChanged` Ereignis ausgelöst werden. Sie sollten sicherstellen, dass die `Slider` Ereignishandler keine Sichten, die während der seiteninitialisierung noch erstellt wurden, noch nicht zugreifen.

Die `ValueChanged` Ereignis wird nicht ausgelöst, während der `Slider` Initialisierung, wenn die `Value` -Eigenschaft ändert. Sie erreichen die `ValueChanged` Handler direkt aus dem Code.

### <a name="slider-color-selection"></a>Schieberegler-Farbauswahl

Die [ **RgbSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/RgbSliders) Programm enthält drei `Slider` Elemente, die Sie interaktiv eine Farbe auswählen, durch die RGB-Werte angeben können:

[![Dreifacher Screenshot des R G B Schieberegler](images/ch15fg03-small.png "RGB-Schieberegler")](images/ch15fg03-large.png#lightbox "RGB-Schieberegler")

Die [ **TextFade** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/TextFade) Beispiel verwendet zwei `Slider` Elemente verschieben zwei `Label` Elemente über eine `AbsoluteLayout` und in den anderen ausblenden.

### <a name="the-stepper-difference"></a>Der Unterschied zugeordnetem

Die [ `Stepper` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/) definiert die gleichen Eigenschaften und Ereignisse wie `Slider` aber die `Maximum` -Eigenschaft wird mit 100 initialisiert und `Stepper` definiert eine vierte Eigenschaft:

- [`Increment`](https://developer.xamarin.com/api/property/Xamarin.Forms.Stepper.Increment/) Der Typ `double`, initialisiert auf 1

Optisch können die `Stepper` besteht aus zwei Schaltflächen, die mit der Bezeichnung  **&ndash;**  und  **+** . Drücken  **&ndash;**  verringert `Value` von `Increment` auf ein Minimum von `Minimum`. Drücken  **+**  erhöht `Value` von `Increment` auf ein Maximum von `Maximum`.

Dies wird veranschaulicht, durch die [ **StepperDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/StepperDemo) Beispiel.

## <a name="switch-and-checkbox"></a>Switch und das Kontrollkästchen

Die [ `Switch` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/) ermöglicht dem Benutzer, der einen booleschen Wert angeben.

### <a name="switch-basics"></a>Switch-Grundlagen

Optisch können die `Switch` besteht aus einer Umschaltfläche, die deaktivieren und aktivieren aktiviert werden können. Die Klasse definiert eine Eigenschaft:

- [`IsToggled`](https://developer.xamarin.com/api/property/Xamarin.Forms.Switch.IsToggled/) vom Typ `bool`

`Switch` definiert ein Ereignis:

- [`Toggled`](https://developer.xamarin.com/api/event/Xamarin.Forms.Switch.Toggled/) begleitet durch eine [ `ToggledEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ToggledEventArgs/) Objekt, das ausgelöst wird, wenn die `IsToggled` -Eigenschaft ändert.

Die [ **SwitchDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SwitchDemo) Programm zeigt die `Switch`.

### <a name="a-traditional-checkbox"></a>Eine herkömmliche Kontrollkästchen

Einige Entwickler bevorzugen möglicherweise einen herkömmlicheren `CheckBox` auf die `Switch`. Die [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Bibliothek enthält eine `CheckBox` von abgeleitete Klasse `ContentView`. `CheckBox` wird implementiert, indem die [CheckBox.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml) und [CheckBox.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml.cs) Dateien. `CheckBox` definiert drei Eigenschaften (`Text`, `FontSize`, und `IsChecked`) und ein `CheckedChanged` Ereignis.

Die [ **CheckBoxDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/CheckBoxDemo) Beispiel veranschaulicht dies `CheckBox`.

## <a name="typing-text"></a>Eingeben von text

Xamarin.Forms definiert drei Ansichten, mit die die Benutzer Text eingeben und bearbeiten können:

- [`Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) für eine einzelne Textzeile
- [`Editor`](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) für mehrere Textzeilen
- [`SearchBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) für eine einzelne Textzeile zu suchen.

`Entry` und `Editor` abgeleitet [ `InputView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.InputView/), die sich daraus ableitet `View`. `SearchBar` direkt abgeleitet `View`.

### <a name="keyboard-and-focus"></a>Tastatur und den Fokus

Auf Telefonen und Tablets ohne physischen Tastaturen den `Entry`, `Editor`, und `SearchBar` bewirken, dass alle Elemente eine virtuelle Tastatur zum Öffnen. Das Vorhandensein dieser Tastatur auf dem Bildschirm bezieht sich auf den Eingabefokus. Eine Sicht benötigen sowohl der [ `IsVisible` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsVisible/) und [ `IsEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsEnabled/) Eigenschaften festlegen, um `true` Eingabefokus abgerufen.

Zwei Methoden, eine schreibgeschützte Eigenschaft und zwei Ereignisse sind mit den Eingabefokus beteiligt. Diese sind alle definierten von `VisualElement`:

- Die [ `Focus` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Focus()/) Methode versucht, den Eingabefokus auf ein Element festgelegt und gibt `true` bei Erfolg
- Die [ `Unfocus` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Unfocus()/) -Methode entfernt den Eingabefokus von einem Element
- Die [ `IsFocused` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsFocused/) nur-Lesen Eigenschaft gibt an, wenn das Element den Eingabefokus besitzt
- Die [ `Focused` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.Focused/) Ereignis zeigt an, wenn ein Element den Eingabefokus erhält
- Die [ `Unfocused` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.Unfocused/) Ereignis zeigt an, wenn ein Element den Eingabefokus verliert

### <a name="choosing-the-keyboard"></a>Auswählen der Tastatur

Die [ `InputView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.InputView/) Klasse, von der `Entry` und `Editor` leiten Sie nur eine Eigenschaft definiert:

- [`Keyboard`](https://developer.xamarin.com/api/property/Xamarin.Forms.InputView.Keyboard/) vom Typ [`Keyboard`](https://developer.xamarin.com/api/type/Xamarin.Forms.Keyboard/)

Hiermit wird die Art der Tastatur, die angezeigt wird. Manche Tastaturen sind für URIs oder Zahlen optimiert.

Die `Keyboard` Klasse ermöglicht das Festlegen einer Tastatur mit einem statischen [ `Keyboard.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Keyboard.Create/p/Xamarin.Forms.KeyboardFlags/) Methode mit einem Argument des Typs [ `KeyboardFlags` ](https://developer.xamarin.com/api/type/Xamarin.Forms.KeyboardFlags/), eine Enumeration mit folgenden Bitflags:

- `None` auf 0 festgelegt
- [`CapitalizeSentence`](https://developer.xamarin.com/api/field/Xamarin.Forms.KeyboardFlags.CapitalizeSentence/) auf 1 festgelegt
- [`Spellcheck`](https://developer.xamarin.com/api/field/Xamarin.Forms.KeyboardFlags.Spellcheck/) auf 2 festgelegt
- [`Suggestions`](https://developer.xamarin.com/api/field/Xamarin.Forms.KeyboardFlags.Suggestions/) Legen Sie nach 4
- [`All`](https://developer.xamarin.com/api/field/Xamarin.Forms.KeyboardFlags.All/) Legen Sie auf \xFFFFFFFF

Bei Verwendung der mehrzeiligen [ `Editor` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) aufrufen, wenn mindestens eines Absatzes der Text erwartet wird, `Keyboard.Create` ist ein guter Ansatz zum Auswählen einer Tastatur. Für das einzeilige [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/), die folgenden statischen schreibgeschützten Eigenschaften der `Keyboard` eignen sich:

- [`Default`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Default/)
- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Text/)
- [`Chat`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Chat/)
- [`Url`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Url/)
- [`Email`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Email/)
- [`Telephone`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Telephone/)
- [`Numeric`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Numeric/) für positive Zahlen mit oder ohne ein Dezimaltrennzeichen.

Die [ `KeyboardTypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.KeyboardTypeConverter/) ermöglicht die Angabe dieser Eigenschaften in XAML, wie durch die [ **EntryKeyboards** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/EntryKeyboards) Programm.

### <a name="entry-properties-and-events"></a>Eigenschaften und Ereignisse

Die einzeilige [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) definiert die folgenden Eigenschaften:

- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Text/) Der Typ `string`, den angezeigten Text in der `Entry`
- [`TextColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.TextColor/) vom Typ `Color`
- [`FontFamily`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.FontFamily/) vom Typ `string`
- [`FontSize`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.FontSize/) vom Typ `double`
- [`FontAttributes`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.FontAttributes/) vom Typ `FontAttributes`
- [`IsPassword`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.IsPassword/) Der Typ `bool`, wodurch Zeichen, das maskiert werden
- [`Placeholder`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Placeholder/) vom Typ `string`, für schwach angezeigten Text in der `Entry` vor nichts eingegeben wurde
- [`PlaceholderColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.PlaceholderColor/) vom Typ `Color`

Die `Entry` definiert außerdem zwei Ereignisse:

- [`TextChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) mit einem [ `TextChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextChangedEventArgs/) Objekt immer ausgelöst, wenn die `Text` eigenschaftenänderungen
- [`Completed`](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.Completed/), ausgelöst wird, wenn der Benutzer abgeschlossen ist und die Tastatur geschlossen wird. Der Benutzer gibt Abschluss in einer Weise plattformspezifischen

Die [ **QuadraticEquations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/QuadaticEquations) Beispiel veranschaulicht diese beiden Ereignisse.

### <a name="the-editor-difference"></a>Der Editor Unterschied

Der mehrzeilige [ `Editor` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) definiert die gleiche `Text` und `Font` Eigenschaften als `Entry` jedoch nicht die anderen Eigenschaften. `Editor` definiert auch die gleichen zwei Eigenschaften wie `Entry`.

[**JustNotes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/JustNotes) ist ein Freiform-Hinweise zur Aufnahme-Programm, das speichert und wiederherstellt den Inhalt der `Editor`.

### <a name="the-searchbar"></a>Die SearchBar

Die [ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) nicht ableiten `InputView`, sodass sie keine `Keyboard` Eigenschaft. Es muss jedoch alle die `Text`, `Font`, und `Placeholder` Eigenschaften, die `Entry` definiert. Darüber hinaus `SearchBar` drei zusätzliche Eigenschaften definiert:

- [`CancelButtonColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.CancelButtonColor/) vom Typ `Color`
- [`SearchCommand`](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.SearchCommand/) Der Typ [ `ICommand` ](https://developer.xamarin.com/api/type/System.Windows.Input.ICommand/) für die Verwendung mit Daten von Bindungen und MVVM
- [`SearchCommandParameter`](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.SearchCommandParameter/) Der Typ `Object`, für die Verwendung mit `SearchCommand`

Die plattformspezifischen "Abbrechen" Schaltfläche löscht den Text. Die `SearchBar` verfügt auch über eine plattformspezifische Suchschaltfläche. Drücken entweder über diese Schaltflächen löst eine der beiden Ereignisse, die `SearchBar` definiert:

- [`TextChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.SearchBar.TextChanged/) zusammen mit einem [ `TextChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextChangedEventArgs/) Objekt
- [`SearchButtonPressed`](https://developer.xamarin.com/api/event/Xamarin.Forms.SearchBar.SearchButtonPressed/)

Die [ **SearchBarDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SearchBarDemo) demonstriert die `SearchBar`.

## <a name="date-and-time-selection"></a>Datum und Uhrzeit-Auswahl

Die [ `DatePicker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) und [ `TimePicker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/) Ansichten implementieren plattformspezifischen-Steuerelemente, mit denen den Benutzer ein Datum oder eine Uhrzeit angeben.

### <a name="the-datepicker"></a>Der Datumsauswahl

[`DatePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) definiert vier Eigenschaften:

- [`MinimumDate`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MinimumDate/) Der Typ `DateTime`, initialisiert auf 1. Januar 1900
- [`MaximumDate`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MaximumDate/) Der Typ `DateTime`, initialisiert auf 31 Dezember 2100
- [`Date`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Date/) Der Typ `DateTime`, initialisiert auf `DateTime.Today`
- [`Format`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Format/) Der Typ `string`, was zu einer Datumsanzeige wie "7/20/1969" in den USA Formatierung der Zeichenfolge "d", die kurzes Datumsmuster initialisiert .NET.

Sie können festlegen, die `DateTime` Eigenschaften in XAML durch auszudrücken, die Eigenschaften wie Eigenschaftenelemente und verwenden das kurze Datum Kultur zu formatieren ("7/20/1969").   

Die [ **DaysBetweenDates** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/DaysBetweenDates) Beispiel berechnet die Anzahl der Tage zwischen zwei Datumsangaben, die vom Benutzer ausgewählt wurden.

### <a name="the-timepicker-or-is-it-a-timespanpicker"></a>Die TimePicker (oder eine TimeSpanPicker?)

[`TimePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/) definiert zwei Eigenschaften und keine Ereignisse:

- [`Time`](https://developer.xamarin.com/api/property/Xamarin.Forms.TimePicker.Time/) ist vom Typ `TimeSpan` statt `DateTime`, der angibt, der Zeitlimits seit Mitternacht vergangenen
- [`Format`](https://developer.xamarin.com/api/property/Xamarin.Forms.TimePicker.Format/) Der Typ `string`, formatieren die Zeichenfolge, die mit "t", das Muster für kurze Zeit eine Uhrzeitanzeige z. B. "1:45 PM" Was initialisiert in den USA .NET.

Die [ **SetTimer** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SetTimer) Programm veranschaulicht, wie die `TimePicker` , eine Zeit für einen Timer anzugeben. Das Programm funktioniert nur, wenn Sie es im Vordergrund beibehalten.

**SetTimer** auch veranschaulicht die Verwendung der [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/) Methode `Page` um eine Warnung anzuzeigen.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 15 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch15-Apr2016.pdf)
- [Chapter 15-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15)
- [Eingabe](~/xamarin-forms/user-interface/text/entry.md)
- [Editor](~/xamarin-forms/user-interface/text/editor.md)
