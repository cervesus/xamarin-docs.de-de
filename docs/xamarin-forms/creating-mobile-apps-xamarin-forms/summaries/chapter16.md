---
title: Zusammenfassung der Kapitel 16. Datenbindung
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: ED997DB0-C229-4868-A5FB-928703B377D6
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 0f200e0c482402813ac7051255dd7c27da93d6dc
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-16-data-binding"></a>Zusammenfassung der Kapitel 16. Datenbindung

Programmierer häufig suchen selbst schreiben Ereignishandler, die erkennen, wenn eine Eigenschaft eines Objekts geändert wurde, und verwenden, um den Wert einer Eigenschaft in ein anderes Objekt zu ändern. Dieser Prozess kann automatisiert werden, mit der Technik der *Datenbindung*. Datenbindungen werden normalerweise in XAML definiert und sind Teil der Definition der Benutzeroberfläche.

Sehr häufig verbinden diese datenbindungen Benutzeroberflächenobjekte, mit der zugrunde liegenden Daten. Dies ist eine Technik, die in mehr untersucht wird [ **Kapitel 18. MVVM**](chapter18.md). Datenbindungen können jedoch auch zwei oder mehr Elemente der Benutzeroberfläche verbinden. Die meisten frühen Beispiele der Datenbindung in diesem Kapitel wird diese Technik veranschaulicht.

## <a name="binding-basics"></a>Grundlagen der Bindung

Binden von Daten sind verschiedene Eigenschaften, Methoden und Klassen beteiligt:

- Die [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Binding/) Klasse abgeleitet [ `BindingBase` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingBase/) und kapselt viele Merkmale einer Bindung
- Die [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) Eigenschaft wird definiert, indem die [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) Klasse
- Die [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) Methode wird auch definiert die [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) Klasse
- Die [ `BindableObjectExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObjectExtensions/) Klasse definiert drei zusätzliche `SetBinding` Methoden

Die folgenden zwei Klassen unterstützen die XAML-Markuperweiterungen für Bindungen:

- [`BindingExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.BindingExtension/) unterstützt die `Binding` Markuperweiterung
- [`ReferenceExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ReferenceExtension/) unterstützt die `x:Reference` Markuperweiterung

Zwei Schnittstellen werden an datenbindungen beteiligt:

- [`INotifyPropertyChanged`](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/) in der `System.ComponentModel` -Namespace ist für die Implementierung der Benachrichtigung, wenn eine Eigenschaft ändert
- [`IValueConverter`](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) wird verwendet, um kleine Klassen definieren, die Werte von einem Typ in einen anderen datenbindungen zu konvertieren.

Eine Datenbindung verbindet zwei Eigenschaften des gleichen Objekts bzw. (häufiger) zwei unterschiedliche Objekte. Diese beiden Eigenschaften werden als bezeichnet den *Quelle* und *Ziel*. Im Allgemeinen eine Änderung in der Source-Eigenschaft führt dazu, dass eine Änderung in der Zieleigenschaft auftreten, aber in einigen Fällen wird die Richtung umgekehrt. Unabhängig vom Typ:

- die *Ziel* Eigenschaft muss gesichert werden, indem ein [`BindableProperty`](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)
- die *Quelle* -Eigenschaft in der Regel ist ein Member einer Klasse, die implementiert [`INotifyPropertyChanged`](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/)

Eine Klasse, die implementiert `INotifyPropertyChanged` ausgelöst wird eine [ `PropertyChanged` ](https://developer.xamarin.com/api/event/System.ComponentModel.INotifyPropertyChanged.PropertyChanged/) Ereignis aus, wenn der Wert eine Eigenschaft geändert wird. `BindableObject` implementiert `INotifyPropertyChanged` und wird automatisch ausgelöst, eine `PropertyChanged` Ereignis aus, wenn eine Eigenschaft durch gesichert eine `BindableProperty` Werte ändern, aber Sie können Ihren eigenen Klassen implementiert, schreiben `INotifyPropertyChanged` ohne eine Ableitung von `BindableObject`.

## <a name="code-and-xaml"></a>Code und XAML

Die [ **OpacityBindingCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/OpacityBindingCode) Beispiel veranschaulicht, wie eine Bindung im Code festgelegt werden:

- Die Quelle ist die `Value` Eigenschaft von einem `Slider`
- Das Ziel ist die `Opacity` Eigenschaft von einem `Label`

Die beiden Objekte verbunden sind, durch Festlegen der `BindingContext` von der `Label` -Objekt an die `Slider` Objekt. Die beiden Eigenschaften verbunden sind, durch den Aufruf eine [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObjectExtensions.SetBinding/p/Xamarin.Forms.BindableObject/Xamarin.Forms.BindableProperty/System.String/) Erweiterungsmethode für die `Label` verweisen auf die `OpacityProperty` bindbare Eigenschaft und die `Value` Eigenschaft der `Slider` ausgedrückt als ein Zeichenfolge.

Bearbeiten der `Slider` löst dann den `Label` zum ein-und auszublenden.

Die [ **OpacityBindingXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/OpacityBindingXaml) desselben Programms mit dem Datenbindung in XAML festgelegt ist. Die `BindingContext` von der `Label` auf festgelegt ist ein `x:Reference` Markup Extension verweisen auf die `Slider`, und die `Opacity` Eigenschaft der `Label` auf festgelegt ist die `Binding` Markuperweiterung mit seine [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) Eigenschaft verweist die `Value` Eigenschaft von der `Slider`.

## <a name="source-and-bindingcontext"></a>Quell- und BindingContext

Die [ **BindingSourceCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingSourceCode) Beispiel wird einen alternativen Ansatz in Code veranschaulicht. Ein `Binding` Objekt wird erstellt, indem die [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Source/) Eigenschaft, um die `Slider` Objekt und die [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) Eigenschaft auf "Value". Die [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) Methode `BindableObject` wird dann aufgerufen werden, auf die `Label` Objekt.

Die [ `Binding` Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Binding.Binding/p/System.String/Xamarin.Forms.BindingMode/Xamarin.Forms.IValueConverter/System.Object/System.String/System.Object/) hätten auch verwendet werden können, definieren die `Binding` Objekt.

Die [ **BindingSourceXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingSourceXaml) Beispiel zeigt das Verfahren vergleichbare, in XAML. Die `Opacity` Eigenschaft von der `Label` auf festgelegt ist eine `Binding` Markuperweiterung mit [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) legen Sie auf der `Value` Eigenschaft und [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Source/) legen Sie auf ein eingebettete `x:Reference` Markuperweiterung.

Es gibt zwei Möglichkeiten, um das Bindungsquellobjekt verweisen, zusammengefasst:

- Über die `BindingContext` Eigenschaft des Ziels
- Über die `Source` Eigenschaft von der `Binding` Objekt selbst

Wenn beide angegeben sind, hat das zweite Vorrang vor. Der Vorteil der `BindingContext` ist, dass sie über die visuelle Struktur weitergegeben wird. Dies ist *sehr* praktisch, wenn mehrere Zieleigenschaften auf dasselbe Objekt für die Datenquelle gebunden sind.

Die [ **WebViewDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/WebViewDemo) Programm veranschaulicht dieses Verfahren mit der [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) Element. Zwei `Button` Elemente für die Navigation in rückwärts und Vorwärts erben eine `BindingContext` von ihrer übergeordneten Gruppe, die verweist auf die `WebView`. Die `IsEnabled` Eigenschaften der zwei Schaltflächen verfügen dann einfache `Binding` Markuperweiterungen, die auf die Schaltfläche mit den abzielen `IsEnabled` Eigenschaften auf Grundlage der Einstellungen der [ `CanGoBack` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.CanGoBack/) und [ `CanGoForward` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.CanGoForward/) schreibgeschützte Eigenschaften von der `WebView`.

## <a name="the-binding-mode"></a>Den Bindungsmodus

Legen Sie die [ `Mode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingBase.Mode/) Eigenschaft `Binding` an ein Mitglied der [ `BindingMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingMode/) Enumeration:

- [`OneWay`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.OneWay/) So, dass Änderungen in der Source-Eigenschaft das Ziel
- [`OneWayToSource`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.OneWayToSource/) So, dass Änderungen in der Zieleigenschaft die Quelle
- [`TwoWay`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.TwoWay/) So, dass Änderungen in den Quell- und Zielservern miteinander
- [`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.Default/) Verwenden der [ `DefaultBindingMode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableProperty.DefaultBindingMode/) angegeben, wenn das Ziel `BindableProperty` erstellt wurde. Falls keiner angegeben wurde, ist die Standardeinstellung `OneWay` für normale bindbare Eigenschaften und `OneWayToSource` für schreibgeschützte bindbare Eigenschaften.

Eigenschaften, die wahrscheinlich die Ziele der datenbindungen in MVVM Szenarien in der Regel sind ein `DefaultBindingMode` von `TwoWay`. Diese lauten wie folgt:

- `Value` Eigenschaft des `Slider` und `Stepper`
- `IsToggled` Eigenschaft `Switch`
- `Text` Eigenschaft des `Entry`, `Editor`, und `SearchBar`
- `Date` Eigenschaft `DatePicker`
- `Time` Eigenschaft `TimePicker`

Die [ **BindingModes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingModes) Beispiel veranschaulicht die von vier Bindungsmodi mit einer Bindung, in dem das Ziel ist, die `FontSize` Eigenschaft eine `Label` und die Quelle der `Value` die Eigenschaft von einem `Slider`. Dadurch kann jede `Slider` steuern Sie den Schriftgrad des entsprechenden `Label`. Aber die `Slider` Elemente werden nicht initialisiert werden, da die `DefaultBindingMode` von der `FontSize` Eigenschaft ist `OneWay`.

Die [ **ReverseBinding** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ReverseBinding) Beispiel legt die Bindungen auf der `Value` Eigenschaft von der `Slider` verweisen auf die `FontSize` -Eigenschaft aller `Label`. Dies scheint der Abwärtskompatibilität vorhanden sein, aber funktioniert besser Ausdruckssuche die `Slider` Elemente da die `Value` Eigenschaft von der `Slider` hat eine `DefaultBindingMode` von `TwoWay`.

[![Dreifacher Screenshot der Reverse-Bindung](images/ch16fg06-small.png "umkehren binden")](images/ch16fg06-large.png#lightbox "Reverse-Bindung")

Dies ist analog zu wie Bindungen in MVVM definiert sind, und verwenden Sie diese Art von häufig binden.

## <a name="string-formatting"></a>Formatierung von Zeichenfolgen

Wenn die Zieleigenschaft den Wert des Typs `string`, können Sie die [ `StringFormat` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingBase.StringFormat/) von definierte Eigenschaft `BindingBase` konvertiert die Quelle eine `string`. Legen Sie die `StringFormat` Eigenschaft, um eine Formatierungszeichenfolge, die Sie mit der statischen verwenden würden .NET [ `String.Format` ](https://developer.xamarin.com/api/member/System.String.Format/p/System.String/System.Object/) Format, um die Anzeige des Objekts. Wenn diese Formatzeichenfolge innerhalb einer Markuperweiterung zu verwenden, setzen Sie sie durch einfache Anführungszeichen, damit die geschweiften Klammern für eine eingebettete Markuperweiterung gehalten werden, wird nicht.

Die [ **ShowViewValues** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ShowViewValues) Beispiel veranschaulicht, wie `StringFormat` in XAML.

Die [ **WhatSizeBindings** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/WhatSizeBindings) Beispiel veranschaulicht, wie die Größe der Seite mit Bindungen in der `Width` und `Height` Eigenschaften der `ContentPage`.

## <a name="why-is-it-called-path"></a>Warum ist es "Path" aufgerufen?

Die [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) Eigenschaft `Binding` wird daher aufgerufen werden, da es eine Reihe von Eigenschaften und Indexer, die durch Punkte getrennt sein kann. Die [ **BindingPathDemos** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingPathDemos) Beispiel zeigt mehrere Beispiele.

## <a name="binding-value-converters"></a>Binden Wertkonverter

Wenn die Quell- und die Eigenschaften einer Bindung unterschiedliche Typen handelt, können Sie zwischen den Typen, die unter Verwendung einer Bindung Konverters konvertieren. Dies ist eine Klasse, implementiert die [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) Schnittstelle, und enthält zwei Methoden: [ `Convert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.IValueConverter.Convert/p/System.Object/System.Type/System.Object/System.Globalization.CultureInfo/) die Quelle zum Ziel, konvertieren und [ `ConvertBack` ](https://developer.xamarin.com/api/member/Xamarin.Forms.IValueConverter.ConvertBack/p/System.Object/System.Type/System.Object/System.Globalization.CultureInfo/) Konvertieren Sie das Ziel in der Quelle.

Der [ `IntToBoolConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IntToBoolConverter.cs) -Klasse in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Bibliothek ist ein Beispiel für das Konvertieren von einer `int` auf eine `bool`. Es wird veranschaulicht, durch die [ **ButtonEnabler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ButtonEnabler) Beispiel nur dadurch die `Button` wurde mindestens ein Zeichen in typisiert ein `Entry`.

Die [ `BoolToStringConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToStringConverter.cs) -Klasse konvertiert einen `bool` auf eine `string` definiert zwei Eigenschaften, um anzugeben, welcher Text zurückgegeben werden soll, und `false` und `true` Werte.
Die [ `BoolToColorConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToColorConverter.cs) ähnelt. Die [ **SwitchText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/SwitchText) Beispiel zeigt die Verwendung dieser beiden Konverter anzuzeigenden unterschiedliche Texte in unterschiedlichen Farben basierend auf einer `Switch` Einstellung.

Die generische [ `BoolToObjectConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToObjectConverter.cs) können ersetzen die `BoolToStringConverter` und `BoolToColorConverter` und dienen als eine generalisierte `bool`-zu-Objekt Konverter eines anderen Typs.

## <a name="bindings-and-custom-views"></a>Bindungen und benutzerdefinierte Ansichten

Sie können benutzerdefinierte Steuerelemente mithilfe der datenbindungen vereinfachen. Die [ `NewCheckBox.cs` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NewCheckBox.xaml.cs) Codedatei definiert `Text`, `TextColor`, `FontSize`, `FontAttributes`, und `IsChecked` Eigenschaften, aber verfügt über keine Logik für die visuellen Elemente des Steuerelements.
Stattdessen die [ `NewCheckBox.cs.xaml` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NewCheckBox.xaml) -Datei enthält alle Markup für das Steuerelement visuelle Elemente über datenbindungen, auf die `Label` Elemente auf Grundlage der Eigenschaften in der CodeBehind-Datei definiert.

Die [ **NewCheckBoxDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/NewCheckBoxDemo) demonstriert die `NewCheckBox` benutzerdefiniertes Steuerelement.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 16 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch16-Apr2016.pdf)
- [Kapitel 16-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16)
