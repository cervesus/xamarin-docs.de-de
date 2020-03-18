---
title: Zusammenfassung von Kapitel 18. MVVM
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung von Kapitel 18. MVVM'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 6A774510-7709-4F60-8EF5-29D478176F8F
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 32c16409f30d6b6d502b7cc074eafb182898594a
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "70771072"
---
# <a name="summary-of-chapter-18-mvvm"></a>Zusammenfassung von Kapitel 18. MVVM

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18)

Eine der besten Möglichkeiten zum Entwerfen einer Anwendung besteht darin, die Benutzeroberfläche vom zugrunde liegenden Code zu trennen, der manchmal als *Geschäftslogik* bezeichnet wird. Es gibt mehrere Methoden, doch die auf XAML-basierte Umgebungen zugeschnittene ist als „Model View ViewModel“ oder „MVVM“ bekannt.

## <a name="mvvm-interrelationships"></a>MVVM-Wechselbeziehungen

Eine MVVM-Anwendung weist drei Ebenen auf:

- Das Modell (Model) stellt zugrunde liegende Daten bereit, manchmal durch Dateien oder Webzugriffe.
- Die Ansicht (View) ist die Benutzeroberfläche oder Präsentationsebene, die generell in XAML implementiert wird.
- Das ViewModel verbindet das Modell und die Ansicht.

Das Modell hat keinen Kontakt zum ViewModel, und das ViewModel hat keinen Kontakt zur Ansicht. Diese drei Ebenen stellen generell mithilfe der folgenden Mechanismen eine Verbindung untereinander her:

![Ansicht (View), ViewModel und Ansicht (View)](images/ch18fg03.png "MVVM")

In vielen kleineren Programmen (und sogar größeren) fehlt häufig das Modell, oder seine Funktionalität ist in das ViewModel integriert.

## <a name="viewmodels-and-data-binding"></a>ViewModels und Datenbindung

Zur Verwendung von Datenbindungen muss ein ViewModel in der Lage sein, die Ansicht zu benachrichtigen, wenn sich eine Eigenschaft des ViewModels geändert hat. Das ViewModel erreicht dies durch Implementierung der [`INotifyPropertyChanged`](xref:System.ComponentModel.INotifyPropertyChanged)-Schnittstelle in den `System.ComponentModel`-Namespace. Dieser ist eher Bestandteil von .NET als von Xamarin.Forms. (In der Regel versuchen ViewModels, die Plattformunabhängigkeit aufrechtzuerhalten.)

Die `INotifyPropertyChanged`-Schnittstelle deklariert ein einzelnes Ereignis namens [`PropertyChanged`](xref:System.ComponentModel.INotifyPropertyChanged), das die geänderte Eigenschaft angibt.

### <a name="a-viewmodel-clock"></a>Eine ViewModel-Uhr

Das [`DateTimeViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DateTimeViewModel.cs) in der Bibliothek [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) definiert eine Eigenschaft vom Typ `DateTime`, die sich auf Grundlage eines Timers ändert. Die Klasse implementiert `INotifyPropertyChanged` und löst immer das `PropertyChanged`-Ereignis aus, wenn sich die `DateTime`-Eigenschaft ändert.

Das [**MvvmClock**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/MvvmClock)-Beispiel instanziiert dieses ViewModel und verwendet Datenbindungen an das ViewModel, um aktualisierte Datums- und Uhrzeitinformationen anzuzeigen.

### <a name="interactive-properties-in-a-viewmodel"></a>Interaktive Eigenschaften in einem ViewModel

Eigenschaften in einem ViewModel können stärker interaktiv sein, wie von der [`SimpleMultiplierViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter18/SimpleMultiplier/SimpleMultiplier/SimpleMultiplier/SimpleMultiplierViewModel.cs)-Klasse veranschaulicht, die Teil des [**SimpleMultiplier**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/SimpleMultiplier)-Beispiels ist. Die Datenbindungen stellen Multiplikand- und Multiplikatorwerte aus zwei `Slider`-Elementen bereit und zeigen das Produkt mit einem `Label` an. Sie können jedoch umfassende Änderungen an dieser Benutzeroberfläche in XAML vornehmen, ohne dass sich daraus Änderungen am ViewModel oder der CodeBehind-Datei ergeben.

### <a name="a-color-viewmodel"></a>Ein Farb-ViewModel

Das [`ColorViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs) in der Bibliothek [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) integriert die RGB- und HSL-Farbmodelle. Dies wird im [**HslSliders**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/HslSliders)-Beispiel veranschaulicht:

[![Dreifacher Screenshot von TK](images/ch18fg08-small.png "HSL-Farbmodell")](images/ch18fg08-large.png#lightbox "HSL-Farbmodell")

### <a name="streamlining-the-viewmodel"></a>Optimieren von ViewModel

Der Code in ViewModels kann optimiert werden, indem eine `OnPropertyChanged`-Methode mithilfe des [`CallerMemberName`](xref:System.Runtime.CompilerServices.CallerMemberNameAttribute)-Attributs definiert wird, die den Namen der aufrufenden Eigenschaft automatisch abruft. Die [`ViewModelBase`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ViewModelBase.cs)-Klasse in der Bibliothek [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) leistet dies und stellt eine Basisklasse für ViewModels bereit.

## <a name="the-command-interface"></a>Die Befehlsschnittstelle

MVVM arbeitet mit Datenbindungen, und Datenbindungen arbeiten mit Eigenschaften, sodass MVVM bei der Behandlung eines `Clicked`-Ereignisses eines `Button` oder eines `Tapped`-Ereignisses eines `TapGestureRecognizer` unzureichend erscheint. Damit ViewModels solche Ereignisse verarbeiten können, unterstützt Xamarin.Forms die *Befehlsschnittstelle*.

Die Befehlsschnittstelle manifestiert sich im `Button` mit zwei öffentlichen Eigenschaften:

- [`Command`](xref:Xamarin.Forms.Button.Command) vom Typ [`ICommand`](xref:System.Windows.Input.ICommand) (im `System.Windows.Input`-Namespace definiert).
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter) vom Typ `Object`

Um die Befehlsschnittstelle zu unterstützen, muss ein ViewModel eine Eigenschaft vom Typ `ICommand` definieren, die dann datenmäßig an die `Command`-Eigenschaft des `Button` gebunden ist. Die `ICommand`-Schnittstelle deklariert zwei Methoden und ein Ereignis:

- Eine [`Execute`](xref:System.Windows.Input.ICommand.Execute(System.Object))-Methode mit einem Argument vom Typ `object`.
- Eine [`CanExecute`](xref:System.Windows.Input.ICommand.CanExecute(System.Object))-Methode mit einem Argument vom Typ `object`, die `bool` zurückgibt.
- Ein [`CanExecuteChanged`](xref:System.Windows.Input.ICommand.CanExecuteChanged)-Ereignis.

Intern legt ein ViewModel jede Eigenschaft vom Typ `ICommand` auf eine Instanz einer Klasse fest, die die `ICommand`-Schnittstelle implementiert. Durch die Datenbindung ruft der `Button` anfänglich die `CanExecute`-Methode auf und deaktiviert sich selbst, wenn die Methode `false` zurückgibt. Außerdem wird ein Handler für das `CanExecuteChanged`-Ereignis festgelegt und immer `CanExecute` aufgerufen, wenn dieses Ereignis ausgelöst wird. Wenn `Button` aktiviert ist, ruft er immer die `Execute`-Methode auf, wenn auf `Button` geklickt wird.

Möglicherweise verfügen Sie über ViewModels, die älter als Xamarin.Forms sind und möglicherweise bereits die Befehlsschnittstelle unterstützen. Bei neuen ViewModels, die nur für die Verwendung mit Xamarin.Forms gedacht sind, stellt Xamarin.Forms eine [`Command`](xref:Xamarin.Forms.Command)-Klasse und eine [`Command<T>`](xref:Xamarin.Forms.Command`1)-Klasse bereit, die die `ICommand`-Schnittstelle implementieren. Der generische Typ ist der Typ des Arguments für die Methoden `Execute` und `CanExecute`.

### <a name="simple-method-executions"></a>Einfache Methodenausführungen

Das [**PowersOfThree**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/PowersOfThree)-Beispiel veranschaulicht, wie Sie die Befehlsschnittstelle in einem ViewModel verwenden. Die [`PowersViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter18/PowersOfThree/PowersOfThree/PowersOfThree/PowersViewModel.cs)-Klasse definiert zwei Eigenschaften vom Typ `ICommand` sowie außerdem zwei private Eigenschaften, die an den einfachsten [`Command`-Konstruktor](xref:Xamarin.Forms.Command.%23ctor(System.Action)) übergeben werden. Das Programm enthält Datenbindungen von diesem ViewModel an die `Command`-Eigenschaften von zwei `Button`-Elementen.

Die `Button`-Elemente können ohne Codeänderungen ganz einfach in XAML durch `TapGestureRecognizer`-Objekte ersetzt werden.

### <a name="a-calculator-almost"></a>Fast ein Taschenrechner

Im [**AddingMachine**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/AddingMachine)-Beispiel werden die beiden Methoden `Execute` und `CanExecute` von `ICommand` verwendet. Das Beispiel verwendet eine [`AdderViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AdderViewModel.cs)-Klasse in der Bibliothek [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AdderViewModel.cs). Das ViewModel enthält sechs Eigenschaften vom Typ `ICommand`. Diese werden vom [`Command`-Konstruktor](xref:Xamarin.Forms.Command.%23ctor(System.Action)) und vom [`Command`-Konstruktor](xref:Xamarin.Forms.Command.%23ctor(System.Action,System.Func{System.Boolean})) von `Command` sowie vom [`Command<T>`-Konstruktor](https://docs.microsoft.com/dotnet/api/xamarin.forms.command.-ctor?view=xamarin-forms#Xamarin_Forms_Command__ctor_System_Action_System_Object__System_Func_System_Object_System_Boolean__) von `Command<T>` initialisiert. Die numerischen Schlüssel der Rechenmaschine sind alle an die Eigenschaft gebunden, die mit `Command<T>` initialisiert wird, und ein `string`-Argument an `Execute` und `CanExecute` identifiziert den jeweiligen Schlüssel.

## <a name="viewmodels-and-the-application-lifecycle"></a>ViewModels und der Anwendungslebenszyklus

Das im **AddingMachine**-Beispiel verwendete `AdderViewModel` definiert außerdem zwei Methoden namens `SaveState` und `RestoreState`. Diese Methoden werden von der Anwendung aufgerufen, wenn sie in den Standbymodus wechselt, sowie bei Neustarts.

## <a name="related-links"></a>Verwandte Links

- [Kapitel 18 – vollständiger Text (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch18-Apr2016.pdf)
- [Kapitel 18 – Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18)
- [E-Book zu Mustern von Unternehmensanwendungen mithilfe von Xamarin.Forms](~/xamarin-forms/enterprise-application-patterns/index.md)
