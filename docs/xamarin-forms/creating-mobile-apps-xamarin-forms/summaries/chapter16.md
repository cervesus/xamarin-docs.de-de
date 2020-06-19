---
title: 'title: "Zusammenfassung von Kapitel 16: Datenbindung" description: "Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung von Kapitel 16.'
description: 'Datenbindung" ms.prod: xamarin ms.technology: xamarin-forms ms.assetid: ED997DB0-C229-4868-A5FB-928703B377D6 author: davidbritch ms.author: dabritch ms.date: 07/18/2018 no-loc: [Xamarin.Forms, Xamarin.Essentials] Zusammenfassung von Kapitel 16.'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: ED997DB0-C229-4868-A5FB-928703B377D6
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: ece93730100001e8339a5f50cdb7ac437d96fa62
ms.sourcegitcommit: ea9269b5d9e3d68b61bb428560a10034117ee457
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2020
ms.locfileid: "84136733"
---
# <a name="summary-of-chapter-16-data-binding"></a>Datenbindung [![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16)

In den Anmerkungen auf dieser Seite wird beschrieben, inwiefern die Angaben innerhalb des Buchs heute nicht mehr für Xamarin.Forms gelten.

> [!NOTE] 
> Programmierer müssen oft Ereignishandler schreiben, die erkennen, wenn sich eine Eigenschaft eines Objekts geändert hat, und dies nutzen, um den Wert einer Eigenschaft in einem anderen Objekt zu ändern.

Dieser Prozess kann mithilfe einer *Datenbindung* genannten Technik automatisiert werden. Datenbindungen werden in der Regel in XAML definiert und Teil der Definition der Benutzeroberfläche. Sehr oft verbinden diese Datenbindungen Benutzeroberflächenobjekte mit zugrunde liegenden Daten.

Dies ist ein Verfahren, das in [**Kapitel 18: MVVM**](chapter18.md) näher beschrieben ist. Datenbindungen können jedoch auch zwei oder mehr Benutzeroberflächenelemente verbinden. In den meisten der frühen Beispiele für Datenbindung in diesem Kapitel wird diese Technik veranschaulicht. Grundlagen der Datenbindung

## <a name="binding-basics"></a>An der Datenbindung sind mehrere Eigenschaften, Methoden und Klassen beteiligt:

Die Klasse [`Binding`](xref:Xamarin.Forms.Binding) leitet sich von [`BindingBase`](xref:Xamarin.Forms.BindingBase) ab und kapselt viele Merkmale einer Datenbindung.

- Die Eigenschaft [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) wird von der Klasse [`BindableObject`](xref:Xamarin.Forms.BindableObject) definiert.
- Die [`SetBinding`](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase))-Methode wird auch von der [`BindableObject`](xref:Xamarin.Forms.BindableObject)-Klasse definiert.
- Die Klasse [`BindableObjectExtensions`](xref:Xamarin.Forms.BindableObjectExtensions) definiert drei weitere `SetBinding`-Methoden.
- Die beiden folgenden Klassen unterstützen XAML-Markuperweiterungen für Bindungen:

[`BindingExtension`](xref:Xamarin.Forms.Xaml.BindingExtension) unterstützt die Markuperweiterung `Binding`.

- [`ReferenceExtension`](xref:Xamarin.Forms.Xaml.ReferenceExtension) unterstützt die Markuperweiterung `x:Reference`.
- An der Datenbindung sind zwei Schnittstellen beteiligt:

[`INotifyPropertyChanged`](xref:System.ComponentModel.INotifyPropertyChanged) im Namespace `System.ComponentModel` dient der Implementierung der Benachrichtigung bei Änderung einer Eigenschaft.

- [`IValueConverter`](xref:Xamarin.Forms.IValueConverter) dient zur Definition kleiner Klassen, die in Datenbindungen Werte von einem Typ in einen anderen konvertieren.
- Eine Datenbindung verbindet zwei Eigenschaften desselben Objekts oder (häufiger) zwei verschiedene Objekte.

Diese beiden Eigenschaften werden als *Quelle* und *Ziel* bezeichnet. Im Allgemeinen bewirkt eine Änderung der Quelleigenschaft eine Änderung der Zieleigenschaft, doch mitunter wird die Richtung umgekehrt. Stets gilt jedoch: Die *Zieleigenschaft* muss von [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) unterstützt werden.

- Die *Quelleigenschaft* ist im Allgemeinen ein Member einer Klasse, die [`INotifyPropertyChanged`](xref:System.ComponentModel.INotifyPropertyChanged) implementiert.
- Eine Klasse, die `INotifyPropertyChanged` implementiert, löst das Ereignis [`PropertyChanged`](xref:System.ComponentModel.INotifyPropertyChanged.PropertyChanged) aus, sobald eine Eigenschaft ihren Wert ändert.

`BindableObject` implementiert `INotifyPropertyChanged` und löst automatisch das Ereignis `PropertyChanged` aus, wenn eine durch `BindableProperty` unterstützte Eigenschaft Werte ändert. Sie können jedoch eigene Klassen schreiben, die `INotifyPropertyChanged` ohne Ableitung von `BindableObject` implementieren. Code und XAML

## <a name="code-and-xaml"></a>Das Beispiel [**OpacityBindingCode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/OpacityBindingCode) zeigt, wie eine Datenbindung im Code festgelegt werden kann:

Die Quelle ist die `Value`-Eigenschaft eines `Slider`-Objekts.

- Das Ziel ist die `Opacity`-Eigenschaft eines `Label`-Objekts.
- Die beiden Objekte werden durch Festlegen von `BindingContext` des `Label`-Objekts mit dem `Slider`-Objekt verbunden.

Die beiden Eigenschaften werden durch den Aufruf der Erweiterungsmethode [`SetBinding`](xref:Xamarin.Forms.BindableObjectExtensions.SetBinding*) für das `Label`-Objekt verbunden. Dabei wird auf die bindbare Eigenschaft `OpacityProperty` und die `Value`-Eigenschaft des `Slider`-Objekts, ausgedrückt als Zeichenfolge, verwiesen. Wenn Sie das `Slider`-Objekt bearbeiten, wird das `Label`-Objekt aus der Ansicht ausgeblendet.

[**OpacityBindingXaml**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/OpacityBindingXaml) ist dasselbe Programm, bei dem die Datenbindung in XAML festgelegt ist.

`BindingContext` des `Label`-Objekts wird auf die Markuperweiterung `x:Reference` festgelegt, die auf `Slider` verweist. Die Eigenschaft `Opacity` des `Label`-Objekts wird auf die Markuperweiterung `Binding` festgelegt, wobei die Eigenschaft [`Path`](xref:Xamarin.Forms.Binding.Path) auf die `Value`-Eigenschaft von `Slider` verweist. Quelle und BindingContext

## <a name="source-and-bindingcontext"></a>Das Beispiel [**BindingSourceCode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingSourceCode) zeigt einen alternativen Ansatz im Code.

Ein `Binding`-Objekt wird erstellt, indem die [`Source`](xref:Xamarin.Forms.Binding.Source)-Eigenschaft auf das `Slider`-Objekt und die [`Path`](xref:Xamarin.Forms.Binding.Path)-Eigenschaft auf „Wert“ festgelegt wird. Die [`SetBinding`](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase))-Methode von`BindableObject` wird dann für das `Label`-Objekt aufgerufen. Der [`Binding`-Konstruktor](xref:Xamarin.Forms.Binding.%23ctor(System.String,Xamarin.Forms.BindingMode,Xamarin.Forms.IValueConverter,System.Object,System.String,System.Object)) kann auch zum Definieren des `Binding`-Objekts verwendet werden.

Das Beispiel [**BindingSourceXaml**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingSourceXaml) zeigt die vergleichbare Technik in XAML.

Die `Opacity`-Eigenschaft von `Label` wird auf die Markuperweiterung `Binding` festgelegt, wobei [`Path`](xref:Xamarin.Forms.Binding.Path) auf die `Value`-Eigenschaft und [`Source`](xref:Xamarin.Forms.Binding.Source) auf die eingebettete Markuperweiterung `x:Reference` festgelegt wird. Zusammenfassend gibt es zwei Möglichkeiten, auf das Bindungsquellobjekt zu verweisen:

Über die `BindingContext`-Eigenschaft des Ziels

- Über die `Source`-Eigenschaft des `Binding`-Objekts selbst
- Wenn beides angegeben wird, hat die zweite Vorrang.

Der Vorteil von `BindingContext` besteht darin, dass die Weitergabe durch die visuelle Struktur erfolgt. Dies ist *sehr* praktisch, wenn mehrere Zieleigenschaften an dasselbe Quellobjekt gebunden sind. Das Programm [**WebViewDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/WebViewDemo) demonstriert diese Technik mit dem Element [`WebView`](xref:Xamarin.Forms.WebView).

Zwei `Button`-Elemente zur Vor- und Rückwärtsnavigation erben `BindingContext` von ihrem übergeordneten Element, das auf `WebView` verweist. Die `IsEnabled`-Eigenschaften der beiden Schaltflächen haben dann einfache Markuperweiterungen des Typs `Binding`, die auf die `IsEnabled`-Eigenschaften der Schaltfläche abzielen, und zwar auf der Grundlage der Einstellungen für [`CanGoBack`](xref:Xamarin.Forms.WebView.CanGoBack) und der schreibgeschützten [`CanGoForward`](xref:Xamarin.Forms.WebView.CanGoForward)-Eigenschaften der `WebView`. Der Bindungsmodus

## <a name="the-binding-mode"></a>Legen Sie die [`Mode`](xref:Xamarin.Forms.BindingBase.Mode)-Eigenschaft von `Binding` auf einen Member der [`BindingMode`](xref:Xamarin.Forms.BindingMode)-Enumeration fest:

[`OneWay`](xref:Xamarin.Forms.BindingMode.OneWay), damit sich Änderungen in der Quelleigenschaft auf das Ziel auswirken.

- [`OneWayToSource`](xref:Xamarin.Forms.BindingMode.OneWayToSource), damit sich Änderungen in der Zieleigenschaft auf die Quelle auswirken.
- [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay), damit sich Änderungen in Quelle und Ziel gegenseitig beeinflussen.
- [`Default`](xref:Xamarin.Forms.BindingMode.Default), damit der [`DefaultBindingMode`](xref:Xamarin.Forms.BindableProperty.DefaultBindingMode) verwendet wird, der beim Erstellen der Ziel-`BindableProperty` angegeben wurde.
- Wenn nichts angegeben wurde, ist der Standard `OneWay` für normale bindbare Eigenschaften und `OneWayToSource` für schreibgeschützte bindbare Eigenschaften. Die Enumeration `BindingMode` enthält nun auch `OnTime`, um eine Bindung nur dann anzuwenden, wenn sich der Bindungskontext ändert, und nicht, wenn sich die Quelleigenschaft ändert.

> [!NOTE]
> Eigenschaften, die voraussichtlich das Ziel von Datenbindungen in MVVM-Szenarien sein werden, haben im Allgemeinen die `DefaultBindingMode`-Einstellung `TwoWay`.

Diese lauten wie folgt: die `Value`-Eigenschaft von `Slider` und `Stepper`

- die `IsToggled`-Eigenschaft von `Switch`
- die `Text`-Eigenschaft von `Entry`, `Editor` und `SearchBar`
- die `Date`-Eigenschaft von `DatePicker`
- die `Time`-Eigenschaft von `TimePicker`
- Das Beispiel [**BindingModes**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingModes) veranschaulicht die vier Bindungsmodi mit einer Datenbindung, bei der das Ziel die `FontSize`-Eigenschaft eines `Label`-Elements und die Quelle die `Value`-Eigenschaft eines `Slider`-Elements ist.

Dies ermöglicht es jedem `Slider`-Element, den Schriftgrad des entsprechenden `Label`-Elements zu steuern. Die `Slider`-Elemente werden jedoch nicht initialisiert, weil der `DefaultBindingMode` der `FontSize`-Eigenschaft `OneWay` ist. Das Beispiel [**ReverseBinding**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ReverseBinding) legt die Bindungen für die `Value`-Eigenschaft des `Slider`-Elements fest, die auf die `FontSize`-Eigenschaft jedes `Label`-Elements verweist.

Das scheint verkehrt herum zu sein, aber es funktioniert besser bei der Initialisierung der `Slider`-Elemente, weil die `Value`-Eigenschaft des `Slider`-Elements die `DefaultBindingMode`-Einstellung `TwoWay` hat. [![Dreifacher Screenshot der umgekehrten Bindung](images/ch16fg06-small.png "Umgekehrte Bindung")](images/ch16fg06-large.png#lightbox "Umgekehrte Bindung")

Dies ist analog zur Definition von Bindungen in MVVM. Sie werden diese Art von Bindung häufig verwenden.

Zeichenfolgenformatierung

## <a name="string-formatting"></a>Wenn die Zieleigenschaft den Typ `string` hat, können Sie die von `BindingBase` definierte [`StringFormat`](xref:Xamarin.Forms.BindingBase.StringFormat)-Eigenschaft verwenden, um die Quelle in `string` zu konvertieren.

Legen Sie die `StringFormat`-Eigenschaft auf eine .NET-Formatierungszeichenfolge fest, die Sie mit dem statischen Format [`String.Format`](xref:System.String.Format(System.String,System.Object)) zum Anzeigen des Objekts verwenden. Bei Verwenden dieser Formatierungszeichenfolge innerhalb einer Markuperweiterung umgeben Sie diese mit einfachen Anführungszeichen, damit die geschweiften Klammern nicht mit einer eingebetteten Markuperweiterung verwechselt werden. Das Beispiel [**ShowViewValues**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ShowViewValues) veranschaulicht die Verwendung von `StringFormat` in XAML.

Das Beispiel [**WhatSizeBindings**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/WhatSizeBindings) demonstriert die Darstellung der Größe der Seite mit Bindungen an die Eigenschaften `Width` und `Height` von `ContentPage`.

Warum lautet der Name „Path“?

## <a name="why-is-it-called-path"></a>Die Eigenschaft [`Path`](xref:Xamarin.Forms.Binding.Path) von `Binding` wird so genannt, weil es sich um eine Reihe von Eigenschaften und Indexern handeln kann, die durch Punkte getrennt sind.

Das Beispiel [**BindingPathDemos**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingPathDemos) zeigt mehrere Beispiele. Bindungswertkonverter

## <a name="binding-value-converters"></a>Wenn die Quell- und Zieleigenschaften einer Bindung unterschiedliche Typen sind, können Sie mit einem Bindungskonverter die Konvertierung zwischen den Typen vornehmen.

Dies ist eine Klasse, die die Schnittstelle [`IValueConverter`](xref:Xamarin.Forms.IValueConverter) implementiert und zwei Methoden enthält: [`Convert`](xref:Xamarin.Forms.IValueConverter.Convert(System.Object,System.Type,System.Object,System.Globalization.CultureInfo)) zur Konvertierung der Quelle in das Ziel und [`ConvertBack`](xref:Xamarin.Forms.IValueConverter.ConvertBack(System.Object,System.Type,System.Object,System.Globalization.CultureInfo)) zur Konvertierung des Ziels in die Quelle. Die [`IntToBoolConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IntToBoolConverter.cs)-Klasse in der Bibliothek [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) ist ein Beispiel für das Konvertieren von `int` in `bool`.

Dies wird durch das Beispiel [**ButtonEnabler**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ButtonEnabler) veranschaulicht, das `Button` nur dann aktiviert, wenn mindestens ein Zeichen in `Entry` eingegeben wurde. Die Klasse [`BoolToStringConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToStringConverter.cs) konvertiert `bool` in `string` und definiert zwei Eigenschaften, um anzugeben, welcher Text für die Werte `false` und `true` zurückgegeben werden soll.

[`BoolToColorConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToColorConverter.cs) ist ähnlich.
Das Beispiel [**SwitchText**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/SwitchText) demonstriert die Verwendung dieser beiden Konverter zur Anzeige unterschiedlicher Texte in unterschiedlichen Farben auf Grundlage einer `Switch`-Einstellung. Der generische [`BoolToObjectConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToObjectConverter.cs) kann `BoolToStringConverter` und `BoolToColorConverter` ersetzen und als universeller `bool`-zu-Objekt-Konverter beliebiger Typen dienen.

Bindungen und benutzerdefinierte Ansichten

## <a name="bindings-and-custom-views"></a>Mithilfe von Datenbindungen können Sie benutzerdefinierte Steuerelemente vereinfachen.

Die Codedatei [`NewCheckBox.cs`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NewCheckBox.xaml.cs) definiert die Eigenschaften `Text`, `TextColor`, `FontSize`, `FontAttributes` und `IsChecked`, enthält jedoch keinerlei Logik für die visuelle Darstellung des Steuerelements. Stattdessen enthält die Datei [`NewCheckBox.cs.xaml`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NewCheckBox.xaml) das gesamte Markup für die visuellen Elemente des Steuerelements durch Datenbindungen an die `Label`-Elemente, und zwar auf Grundlage der in der CodeBehind-Datei definierten Eigenschaften.
Das Beispiel [**NewCheckBoxDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/NewCheckBoxDemo) veranschaulicht das benutzerdefinierte Steuerelement `NewCheckBox`.

Verwandte Links

## <a name="related-links"></a>[Kapitel 16 – vollständiger Text (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch16-Apr2016.pdf)

- [Kapitel 16 – Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16)
- [Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- Umgekehrte Bindung
