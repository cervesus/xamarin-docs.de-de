---
title: Zusammenfassung der Kapitel 16. Datenbindung
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung der Kapitel 16. Datenbindung'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: ED997DB0-C229-4868-A5FB-928703B377D6
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: c4ad067778203759a54ed8141db0b82602e40f6c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997451"
---
# <a name="summary-of-chapter-16-data-binding"></a>Zusammenfassung der Kapitel 16. Datenbindung

Programmierer oft sind das Schreiben von Ereignishandlern, die erkennen, wenn eine Eigenschaft eines Objekts geändert wurde, und verwenden, um den Wert einer Eigenschaft in ein anderes Objekt ändern. Dieser Prozess kann automatisiert werden, mit der Technik der *Datenbindung*. Datenbindungen werden in der Regel in XAML definiert und sind Teil der Definition der Benutzeroberfläche.

Sehr oft Verbindung datenbindungen, die diese Objekte der Benutzeroberfläche zugrunde liegenden Daten. Dies ist eine Technik, die in mehr erläutert [ **Kapitel 18. MVVM**](chapter18.md). Allerdings können die datenbindungen, die auch mindestens zwei Elemente der Benutzeroberfläche verbinden. Die meisten frühe Beispiele der Datenbindung in diesem Kapitel wird diese Technik veranschaulicht.

## <a name="binding-basics"></a>Grundlagen der Datenbindung

Mehrere Eigenschaften, Methoden und Klassen sind in der Datenbindung beteiligt:

- Die [ `Binding` ](xref:Xamarin.Forms.Binding) Klasse leitet sich von [ `BindingBase` ](xref:Xamarin.Forms.BindingBase) und kapselt viele Merkmale einer Bindung
- Die [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) Eigenschaft wird definiert, durch die [ `BindableObject` ](xref:Xamarin.Forms.BindableObject) Klasse
- Die [ `SetBinding` ](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) Methode wird auch definiert die [ `BindableObject` ](xref:Xamarin.Forms.BindableObject) Klasse
- Die [ `BindableObjectExtensions` ](xref:Xamarin.Forms.BindableObjectExtensions) -Klasse definiert drei zusätzliche `SetBinding` Methoden

Die beiden folgenden Klassen unterstützen die XAML-Markuperweiterungen für Bindungen:

- [`BindingExtension`](xref:Xamarin.Forms.Xaml.BindingExtension) unterstützt die `Binding` -Markuperweiterung
- [`ReferenceExtension`](xref:Xamarin.Forms.Xaml.ReferenceExtension) unterstützt die `x:Reference` -Markuperweiterung

Zwei Schnittstellen sind beteiligt, bei der Datenbindung:

- [`INotifyPropertyChanged`](xref:System.ComponentModel.INotifyPropertyChanged) in der `System.ComponentModel` -Namespace ist für die Implementierung der Benachrichtigung, wenn eine Eigenschaft ändert
- [`IValueConverter`](xref:Xamarin.Forms.IValueConverter) wird verwendet, um kleine Klassen definieren, die Werte von einem Typ in eine andere datenbindungen zu konvertieren.

Eine Datenbindung verbindet zwei Eigenschaften, die das gleiche Objekt oder (häufiger) zwei verschiedene Objekte. Diese beiden Eigenschaften werden als bezeichnet die *Quelle* und *Ziel*. Im Allgemeinen eine Änderung in der Source-Eigenschaft führt dazu, dass eine Änderung in der Zieleigenschaft erfolgen, aber manchmal wird die Richtung umgekehrt. Unabhängig davon, ob:

- die *Ziel* Eigenschaft muss gesichert werden, indem ein [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)
- die *Quelle* -Eigenschaft ist in der Regel ein Member einer Klasse, die implementiert [`INotifyPropertyChanged`](xref:System.ComponentModel.INotifyPropertyChanged)

Eine Klasse, die implementiert `INotifyPropertyChanged` löst eine [ `PropertyChanged` ](xref:System.ComponentModel.INotifyPropertyChanged.PropertyChanged) Ereignis aus, wenn der Wert eine Eigenschaft geändert wird. `BindableObject` implementiert `INotifyPropertyChanged` und löst automatisch eine `PropertyChanged` Ereignis aus, wenn eine Eigenschaft von unterstützt eine `BindableProperty` Werte ändern, aber Sie können Ihre eigenen Klassen implementiert schreiben `INotifyPropertyChanged` ohne eine Ableitung von `BindableObject`.

## <a name="code-and-xaml"></a>Code und XAML

Die [ **OpacityBindingCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/OpacityBindingCode) Beispiel veranschaulicht, wie eine Bindung im Code festlegen:

- Die Quelle ist die `Value` Eigenschaft ein `Slider`
- Das Ziel ist die `Opacity` Eigenschaft ein `Label`

Die beiden Objekte verbunden sind, durch Festlegen der `BindingContext` von der `Label` -Objekt an die `Slider` Objekt. Die beiden Eigenschaften sind durch Aufrufen von verbunden ein [ `SetBinding` ](xref:Xamarin.Forms.BindableObjectExtensions.SetBinding*) -Erweiterungsmethode auf der `Label` verweisen auf die `OpacityProperty` bindbare Eigenschaft und die `Value` Eigenschaft der `Slider` als eine Zeichenfolge.

Bearbeiten der `Slider` löst dann die `Label` auf Sie ein-und auszublenden.

Die [ **OpacityBindingXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/OpacityBindingXaml) ist das gleiche Programm mit der Datenbindung in XAML festlegen. Der `BindingContext` von der `Label` nastaven NA hodnotu ein `x:Reference` Markup Extension verweisen auf die `Slider`, und die `Opacity` Eigenschaft der `Label` nastaven NA hodnotu der `Binding` Markuperweiterung mit der [ `Path` ](xref:Xamarin.Forms.Binding.Path) Eigenschaft verweist die `Value` Eigenschaft der `Slider`.

## <a name="source-and-bindingcontext"></a>Quell- und BindingContext

Die [ **BindingSourceCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingSourceCode) Beispiel zeigt eine alternative Methode im Code. Ein `Binding` objekterstellung durch Festlegen der [ `Source` ](xref:Xamarin.Forms.Binding.Source) Eigenschaft, um die `Slider` Objekt und die [ `Path` ](xref:Xamarin.Forms.Binding.Path) Eigenschaft auf "Value". Die [ `SetBinding` ](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) -Methode der `BindableObject` wird dann aufgerufen werden, auf die `Label` Objekt.

Die [ `Binding` Konstruktor](xref:Xamarin.Forms.Binding.%23ctor(System.String,Xamarin.Forms.BindingMode,Xamarin.Forms.IValueConverter,System.Object,System.String,System.Object)) hätten auch verwendet werden, definieren die `Binding` Objekt.

Die [ **BindingSourceXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingSourceXaml) Beispiel zeigt die vergleichbare Technik in XAML. Die `Opacity` Eigenschaft der `Label` nastaven NA hodnotu eine `Binding` Markuperweiterung mit [ `Path` ](xref:Xamarin.Forms.Binding.Path) legen Sie auf die `Value` Eigenschaft und [ `Source` ](xref:Xamarin.Forms.Binding.Source) legen Sie auf eine eingebettete `x:Reference` Markuperweiterung.

Zusammenfassend lässt sich sagen gibt es zwei Möglichkeiten, das Bindungsquellobjekt verweisen:

- Durch die `BindingContext` Zieleigenschaft
- Durch die `Source` Eigenschaft der `Binding` Objekt selbst

Wenn beide angegeben werden, hat das zweite Vorrang vor. Der Vorteil der `BindingContext` besteht darin, dass sie über die visuelle Struktur weitergegeben wird. Dies ist *sehr* praktisch, wenn mehrere Eigenschaften auf das gleiche Objekt für die Datenquelle gebunden sind.

Die [ **WebViewDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/WebViewDemo) Programm veranschaulicht dieses Verfahren, mit der [ `WebView` ](xref:Xamarin.Forms.WebView) Element. Zwei `Button` -Elemente für die Navigation rückwärts und Vorwärts erben eine `BindingContext` von ihrer übergeordneten Gruppe, die verweist die `WebView`. Die `IsEnabled` Eigenschaften der zwei Schaltflächen verfügen dann einfach `Binding` Markuperweiterungen, die auf die Schaltfläche mit den abzielen `IsEnabled` Eigenschaften auf Grundlage der Einstellungen von der [ `CanGoBack` ](xref:Xamarin.Forms.WebView.CanGoBack) und [ `CanGoForward` ](xref:Xamarin.Forms.WebView.CanGoForward) schreibgeschützten Eigenschaften der `WebView`.

## <a name="the-binding-mode"></a>Den Bindungsmodus

Legen Sie die [ `Mode` ](xref:Xamarin.Forms.BindingBase.Mode) Eigenschaft `Binding` auf einen Member der [ `BindingMode` ](xref:Xamarin.Forms.BindingMode) Enumeration:

- [`OneWay`](xref:Xamarin.Forms.BindingMode.OneWay) So, dass Änderungen in der Source-Eigenschaft das Ziel
- [`OneWayToSource`](xref:Xamarin.Forms.BindingMode.OneWayToSource) So, dass Änderungen in die Eigenschaft die Quelle
- [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) Damit Änderungen an den Quell- und Zielservern jeweils anderen auswirken
- [`Default`](xref:Xamarin.Forms.BindingMode.Default) Verwenden der [ `DefaultBindingMode` ](xref:Xamarin.Forms.BindableProperty.DefaultBindingMode) angegeben, wenn das Ziel `BindableProperty` erstellt wurde. Falls keine angegeben wurde, wird der Standardwert ist `OneWay` für normale bindbare Eigenschaften und `OneWayToSource` für nur-Lese bindbare Eigenschaften.

Eigenschaften, die wahrscheinlich die Ziele von datenbindungen in MVVM-Szenarien in der Regel sind verfügen über eine `DefaultBindingMode` von `TwoWay`. Diese lauten wie folgt:

- `Value` Eigenschaft des `Slider` und `Stepper`
- `IsToggled` Eigenschaft `Switch`
- `Text` Eigenschaft des `Entry`, `Editor`, und `SearchBar`
- `Date` Eigenschaft `DatePicker`
- `Time` Eigenschaft `TimePicker`

Die [ **BindingModes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingModes) Beispiel veranschaulicht die vier Bindungsmodi mit einer Datenbindung, in dem das Ziel ist, die `FontSize` Eigenschaft eine `Label` und die Quelle der `Value` -Eigenschaft einer `Slider`. Dadurch kann jede `Slider` steuern Sie den Schriftgrad des entsprechenden `Label`. Aber die `Slider` Elemente werden nicht initialisiert werden, da die `DefaultBindingMode` von der `FontSize` Eigenschaft `OneWay`.

Die [ **ReverseBinding** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ReverseBinding) Beispiel wird die Bindungen für die `Value` Eigenschaft der `Slider` verweisen auf die `FontSize` Eigenschaft der einzelnen `Label`. Dies scheint rückwärts sein, aber funktioniert besser Ausdruckssuche die `Slider` Elemente da die `Value` Eigenschaft der `Slider` verfügt über eine `DefaultBindingMode` von `TwoWay`.

[![Dreifacher Screenshot der Reverse-Bindung](images/ch16fg06-small.png "Reverse-Bindung")](images/ch16fg06-large.png#lightbox "Reverse-Bindung")

Dies ist vergleichbar mit wie Bindungen in MVVM definiert werden, und verwenden Sie diese Art von Bindung häufig.

## <a name="string-formatting"></a>Formatieren von Zeichenfolgen

Wenn die Zieleigenschaft ist vom Typ `string`, können Sie die [ `StringFormat` ](xref:Xamarin.Forms.BindingBase.StringFormat) von definierte Eigenschaft `BindingBase` konvertieren Sie die Quelle, die eine `string`. Legen Sie die `StringFormat` Eigenschaft, um eine Formatierungszeichenfolge, die Sie mit der statischen verwenden .NET [ `String.Format` ](xref:System.String.Format(System.String,System.Object)) Format, um das Objekt anzuzeigen. Wenn Sie diese Formatierungszeichenfolge innerhalb einer Markuperweiterung verwenden zu können, formatieren Sie ihn durch einfache Anführungszeichen, damit die geschweiften Klammern für eine eingebettete Markuperweiterung gehalten werden, wird nicht.

Die [ **ShowViewValues** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ShowViewValues) Beispiel veranschaulicht, wie `StringFormat` in XAML.

Die [ **WhatSizeBindings** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/WhatSizeBindings) Beispiel zeigt die Größe der Seite angezeigt, mit Bindungen in der `Width` und `Height` Eigenschaften der `ContentPage`.

## <a name="why-is-it-called-path"></a>Warum werden es "Path" aufgerufen?

Die [ `Path` ](xref:Xamarin.Forms.Binding.Path) Eigenschaft `Binding` wird daher aufgerufen werden, da es eine Reihe von Eigenschaften und Indexer, die durch Punkte voneinander getrennt sein kann. Die [ **BindingPathDemos** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingPathDemos) Beispiel zeigt mehrere Beispiele.

## <a name="binding-value-converters"></a>Binden von Wertkonvertern

Wenn die Quell- und die Eigenschaften einer Bindung unterschiedliche Typen handelt, können Sie zwischen den Typen, die über einen bindungskonverter konvertieren. Dies ist eine Klasse, die implementiert die [ `IValueConverter` ](xref:Xamarin.Forms.IValueConverter) Schnittstelle und enthält zwei Methoden: [ `Convert` ](xref:Xamarin.Forms.IValueConverter.Convert(System.Object,System.Type,System.Object,System.Globalization.CultureInfo)) die Quelle zum Ziel, konvertieren und [ `ConvertBack` ](xref:Xamarin.Forms.IValueConverter.ConvertBack(System.Object,System.Type,System.Object,System.Globalization.CultureInfo)) Konvertieren Sie das Ziel in der Quelle.

Die [ `IntToBoolConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IntToBoolConverter.cs) -Klasse in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) -Bibliothek ist ein Beispiel für die Konvertierung ein `int` auf eine `bool`. Es wird veranschaulicht, durch die [ **ButtonEnabler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ButtonEnabler) Beispiel nur kann die `Button` Wenn mindestens ein Zeichen in typisiert wurde ein `Entry`.

Die [ `BoolToStringConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToStringConverter.cs) -Klasse konvertiert einen `bool` auf eine `string` definiert zwei Eigenschaften, um anzugeben, welcher Text zurückgegeben werden soll, und `false` und `true` Werte.
Die [ `BoolToColorConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToColorConverter.cs) ist ähnlich. Die [ **SwitchText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/SwitchText) Beispiel zeigt die Verwendung dieser beiden Konverter anzuzeigende unterschiedliche Texte in unterschiedlichen Farben basierend auf einer `Switch` festlegen.

Die generische [ `BoolToObjectConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToObjectConverter.cs) können ersetzen die `BoolToStringConverter` und `BoolToColorConverter` und dienen als einen generalisierten `bool`-zu-Konverter für Objekte eines beliebigen Typs.

## <a name="bindings-and-custom-views"></a>Bindungen und benutzerdefinierte Ansichten

Sie können benutzerdefinierte Steuerelemente, die über datenbindungen vereinfachen. Die [ `NewCheckBox.cs` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NewCheckBox.xaml.cs) Codedatei definiert `Text`, `TextColor`, `FontSize`, `FontAttributes`, und `IsChecked` Eigenschaften, jedoch verfügt über keine Logik für die visuellen Elemente des Steuerelements an.
Stattdessen die [ `NewCheckBox.cs.xaml` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NewCheckBox.xaml) -Datei enthält alle Markups für die Darstellung des Steuerelements über datenbindungen für das `Label` Elemente auf Grundlage der Eigenschaften in der CodeBehind-Datei definiert.

Die [ **NewCheckBoxDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/NewCheckBoxDemo) Beispiel veranschaulicht die `NewCheckBox` benutzerdefiniertes Steuerelement.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 16 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch16-Apr2016.pdf)
- [Kapitel 16-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16)
