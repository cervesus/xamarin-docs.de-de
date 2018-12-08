---
title: Zusammenfassung der Kapitel 18. MVVM
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung der Kapitel 18. MVVM'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 6A774510-7709-4F60-8EF5-29D478176F8F
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: db837ac8bfa1b7a946ee606e9481f9feb2a8a31f
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2018
ms.locfileid: "53050118"
---
# <a name="summary-of-chapter-18-mvvm"></a>Zusammenfassung der Kapitel 18. MVVM

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18)

Eine der besten Möglichkeiten zum Entwerfen einer Anwendungs durch die Trennung der Benutzeroberfläche aus der zugrunde liegende Code, der manchmal auch aufgerufen wird, wird die *Geschäftslogik*. Verschiedene Methoden vorhanden sind, aber diejenige, die für XAML-basierten Umgebungen zugeschnitten ist, wird als Model-View-ViewModel "oder" MVVM bezeichnet.

## <a name="mvvm-interrelationships"></a>MVVM Wechselbeziehungen

Eine MVVM-Anwendung verfügt über drei Schichten:

- Das Modell bietet die zugrunde liegenden Daten manchmal durch Dateien oder das Web zugreift
- Die Ansicht ist die Schnittstelle oder eine Präsentation Benutzerebene, in der Regel in XAML implementiert.
- Das "ViewModel" stellt eine Verbindung her, das Modell und die Ansicht

Das Modell kennen muss, das "ViewModel" und "ViewModel" Ignorieren der Ansicht ist. Diese drei Ebenen werden in der Regel mit den folgenden Mechanismen miteinander verbinden:

![Anzeigen, ViewModel und Ansicht](images/ch18fg03.png "MVVM")

In viele kleinere Programme (und sogar zu größeren Diensten), häufig das Modell nicht vorhanden ist oder die Funktionalität ist in "ViewModel" integriert.

## <a name="viewmodels-and-data-binding"></a>ViewModels und Datenbindung

Um datenbindungen zu engagieren, muss ein "ViewModel" kann die Ansicht zu benachrichtigen, wenn es sich bei Änderung einer Eigenschaft von "ViewModel". Das "ViewModel" wird durch die Implementierung der [ `INotifyPropertyChanged` ](xref:System.ComponentModel.INotifyPropertyChanged) -Schnittstelle in der `System.ComponentModel` Namespace. Dies ist Teil von .NET anstelle von Xamarin.Forms. (Im Allgemeinen ViewModels versuchen zu Unabhängigkeit von der Plattform zu verwalten.)

Die `INotifyPropertyChanged` Schnittstelle deklariert, ein einzelnes Ereignis mit dem Namen [ `PropertyChanged` ](xref:System.ComponentModel.INotifyPropertyChanged) , der die geänderte Eigenschaft angibt.

### <a name="a-viewmodel-clock"></a>Eine ViewModel-Uhr

Die [ `DateTimeViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DateTimeViewModel.cs) in die [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) Library definiert eine Eigenschaft des Typs `DateTime` , dass Änderungen auf einem Zeitgeber basierende. Die Klasse implementiert `INotifyPropertyChanged` und löst die `PropertyChanged` Ereignis immer die `DateTime` eigenschaftenänderungen.

Die [ **MvvmClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/MvvmClock) Beispiel dieses "ViewModel" instanziiert und verwendet Sie datenbindungen, die "ViewModels", um aktualisierte Datums- und Uhrzeitangaben anzuzeigen.

### <a name="interactive-properties-in-a-viewmodel"></a>Interaktive Eigenschaften in ein "ViewModel"

Eigenschaften in einem ViewModel können mehr Interaktivität sein, wie die [ `SimpleMultiplierViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter18/SimpleMultiplier/SimpleMultiplier/SimpleMultiplier/SimpleMultiplierViewModel.cs) -Klasse, die Teil von der [ **SimpleMultiplier** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/SimpleMultiplier) Beispiel. Datenbindungen bieten Multiplikand und Multiplikator Werte aus zwei `Slider` Elemente und zeigt das Produkt mit einer `Label`. Allerdings können Sie umfangreiche diese Benutzeroberfläche in XAML ohne folgende Änderungen an ViewModel oder Code-Behind-Datei ändern.

### <a name="a-color-viewmodel"></a>Eine Farbe "ViewModel"

Die [ `ColorViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs) in die [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) Bibliothek ist die RGB- und HSL-Farbe-Modelle integriert. Es wird veranschaulicht, der [ **HslSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/HslSliders) Beispiel:

[![Dreifacher Screenshot der TK](images/ch18fg08-small.png "HSL-Farbe Modell")](images/ch18fg08-large.png#lightbox "HSL-Farbe-Modell")

### <a name="streamlining-the-viewmodel"></a>Optimieren das "ViewModel"

Der Code in ViewModels optimiert werden kann, durch die Definition einer `OnPropertyChanged` -Methode der [ `CallerMemberName` ](xref:System.Runtime.CompilerServices.CallerMemberNameAttribute) -Attribut, das den aufrufenden Eigenschaftennamen automatisch abgerufen. Die [ `ViewModelBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ViewModelBase.cs) -Klasse in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) Bibliothek verfügt, und stellt eine Basisklasse für die ViewModels bereit.

## <a name="the-command-interface"></a>Die Befehlsschnittstelle

MVVM funktioniert mit datenbindungen und datenbindungen arbeiten mit Eigenschaften, sodass MVVM fehlerhaft zu sein scheint, bei der die Behandlung einer `Clicked` Ereignis eine `Button` oder `Tapped` Ereignis eine `TapGestureRecognizer`. Um ViewModels so behandeln Sie diese Ereignisse zu ermöglichen, sich Xamarin.Forms unterstützt die *Befehlsschnittstelle*.

Die Befehlsschnittstelle manifestiert sich in der `Button` mit zwei öffentliche Eigenschaften:

- [`Command`](xref:Xamarin.Forms.Button.Command) Der Typ [ `ICommand` ](xref:System.Windows.Input.ICommand) (definiert der `System.Windows.Input` Namespace)
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter) Der Typ `Object`

Um die Befehlsschnittstelle unterstützen zu können, muss ein "ViewModel" eine Eigenschaft des Typs definieren `ICommand` , klicken Sie dann Daten gebunden werden, um die `Command` Eigenschaft der `Button`. Die `ICommand` Schnittstelle deklariert zwei Methoden und ein Ereignis:

- Ein [ `Execute` ](xref:System.Windows.Input.ICommand.Execute(System.Object)) Methode mit einem Argument des Typs `object`
- Ein [ `CanExecute` ](xref:System.Windows.Input.ICommand.CanExecute(System.Object)) Methode mit einem Argument des Typs `object` zurückgibt `bool`
- Ein [ `CanExecuteChanged` ](xref:System.Windows.Input.ICommand.CanExecuteChanged) Ereignis

Ein "ViewModel" legt intern, jede Eigenschaft des Typs `ICommand` mit einer Instanz einer Klasse, die implementiert die `ICommand` Schnittstelle. Durch die Datenbindung, die `Button` zu Beginn aufgerufenen die `CanExecute` -Methode, und deaktiviert Sie sich an, wenn die Methode zurückgibt `false`. Außerdem wird einen Handler für die `CanExecuteChanged` -Ereignis aus und ruft `CanExecute` jedes Mal, wenn dieses Ereignis ausgelöst wird. Wenn die `Button` ist aktiviert, ruft der `Execute` Methode immer die `Button` geklickt wird.

Sie müssen möglicherweise einige ViewModels, die Xamarin.Forms sind älter als ein, und diese möglicherweise bereits die Befehlsschnittstelle unterstützen. Für neue ViewModels nur mit Xamarin.Forms eingesetzt werden soll, um Xamarin.Forms stellt ein [ `Command` ](xref:Xamarin.Forms.Command) Klasse und ein [ `Command<T>` ](xref:Xamarin.Forms.Command`1) Klasse implementiert die `ICommand` Schnittstelle. Der generische Typ ist der Typ des Arguments für die `Execute` und `CanExecute` Methoden.

### <a name="simple-method-executions"></a>Einfache Methode Ausführungen

Die [ **PowersOfThree** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/PowersOfThree) Beispiel veranschaulicht, wie die Befehlsschnittstelle in ein "ViewModel". Die [ `PowersViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter18/PowersOfThree/PowersOfThree/PowersOfThree/PowersViewModel.cs) -Klasse definiert zwei Eigenschaften vom Typ `ICommand` und definiert außerdem zwei private Eigenschaften, die an den einfachsten übergeben [ `Command` Konstruktor](xref:Xamarin.Forms.Command.%23ctor(System.Action)). Das Programm enthält datenbindungen, die von "ViewModel" an die `Command` Eigenschaften von zwei `Button` Elemente.

Die `Button` Elemente können ganz einfach durch ersetzt werden `TapGestureRecognizer` Objekte in XAML ohne codeänderungen.

### <a name="a-calculator-almost"></a>Ein Rechner, fast

Die [ **AddingMachine** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/AddingMachine) Beispiel wird sowohl die `Execute` und `CanExecute` Methoden `ICommand`. Er verwendet eine [ `AdderViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AdderViewModel.cs) -Klasse in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AdderViewModel.cs) Bibliothek. Das ViewModel enthält sechs Eigenschaften des Typs `ICommand`. Diese werden initialisiert, aus der [ `Command` Konstruktor](xref:Xamarin.Forms.Command.%23ctor(System.Action)) und [ `Command` Konstruktor](xref:Xamarin.Forms.Command.%23ctor(System.Action,System.Func{System.Boolean})) von `Command` und [ `Command<T>` Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command%3CT%3E.Command%3CT%3E/p/System.Action%7BT%7D/System.Func%7BT,System.Boolean%7D/) der `Command<T>`. Die numerischen Tasten, der die Rechenmaschine gebunden sind, die Eigenschaft, die mit initialisiert wird `Command<T>`, und ein `string` Argument `Execute` und `CanExecute` identifiziert den bestimmten Schlüssel.

## <a name="viewmodels-and-the-application-lifecycle"></a>ViewModels und den Lebenszyklus der Anwendung

Die `AdderViewModel` in verwendet die **AddingMachine** Beispiel definiert auch zwei Methoden namens `SaveState` und `RestoreState`. Diese Methoden werden von der Anwendung aufgerufen, wenn er in den Ruhezustand wechselt, und wenn er neu wird gestartet.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 18 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch18-Apr2016.pdf)
- [Kapitel 18-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18)
- [Xamarin.Forms-e-Book mit Mustern von Unternehmensanwendungen](~/xamarin-forms/enterprise-application-patterns/index.md)
