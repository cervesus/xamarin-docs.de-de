---
title: Zusammenfassung der Kapitel 18. MVVM
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 6A774510-7709-4F60-8EF5-29D478176F8F
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: cadab2432d0b6c29ead9cde1f4220bb64e1e1886
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-18-mvvm"></a>Zusammenfassung der Kapitel 18. MVVM

Eine der besten Möglichkeiten zum Entwickeln einer Anwendung durch die Trennung von der Benutzeroberfläche von der zugrunde liegende Code, der manchmal aufgerufen wird, ist die *Geschäftslogik*. Verschiedene Techniken vorhanden, aber das Projekt, das für die XAML-basierten Umgebungen zugeschnitten ist als Model-View-ViewModel oder MVVM bekannt ist.

## <a name="mvvm-interrelationships"></a>MVVM Wechselbeziehungen

Eine Anwendung MVVM verfügt über drei Ebenen:

- Das Modell bietet zugrunde liegenden Daten in einigen Fällen über die Dateien oder Web zugreift
- Die Sicht ist der Benutzer-Schnittstelle oder eine Präsentation Ebene, in der Regel in XAML implementiert
- Das ViewModel stellt eine Verbindung her, das Modell und die Ansicht

Das Modell ist für das ViewModel persistenzignoranten und ViewModel persistenzignoranten der Sicht ist. Diese drei Ebenen in der Regel verbunden miteinander mit den folgenden Mechanismen:

![Anzeigen, ViewModel und Ansicht](images/ch18fg03.png "MVVM")

Viele kleinere Programme (und auch größere) häufig das Modell nicht vorhanden ist oder seine Funktionalität in das ViewModel integriert ist.

## <a name="viewmodels-and-data-binding"></a>ViewModels und Datenbindung

Um datenbindungen aufzunehmen, muss ein ViewModel kann benachrichtigen die Ansicht aus, wenn eine Eigenschaft des ViewModel geändert hat. Das ViewModel wird dies durch die Implementierung der [ `INotifyPropertyChanged` ](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/) -Schnittstelle in der `System.ComponentModel` Namespace. Dies ist Teil von .NET anstelle von Xamarin.Forms. (Im Allgemeinen ViewModels versucht wird, um plattformunabhängig zu verwalten.)

Die `INotifyPropertyChanged` Schnittstelle deklariert ein Ereignis mit dem Namen [ `PropertyChanged` ](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/) , der die geänderte Eigenschaft angibt.

### <a name="a-viewmodel-clock"></a>Eine ViewModel-Uhr

Die [ `DateTimeViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DateTimeViewModel.cs) in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) Library definiert eine Eigenschaft vom Typ `DateTime` , dass Änderungen auf einen Zeitgeber. Die Klasse implementiert `INotifyPropertyChanged` und löst die `PropertyChanged` Ereignis immer die `DateTime` -Eigenschaft ändert.

Die [ **MvvmClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/MvvmClock) Beispiel diesem ViewModel instanziiert und verwendet Sie datenbindungen auf das ViewModel aktualisierte Datums- und Uhrzeitangaben angezeigt.

### <a name="interactive-properties-in-a-viewmodel"></a>Interaktive Eigenschaften in einem ViewModel

Eigenschaften in einem ViewModel werden überwiegend interaktiv, wie durch die [ `SimpleMultiplierViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter18/SimpleMultiplier/SimpleMultiplier/SimpleMultiplier/SimpleMultiplierViewModel.cs) -Klasse, die Teil von der [ **SimpleMultiplier** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/SimpleMultiplier) Beispiel. Die datenbindungen geben Multiplikand und Multiplikator Werte aus zwei `Slider` Elemente und zeigen Sie das Produkt mit einer `Label`. Allerdings können Sie eine umfangreiche dieser Benutzeroberfläche in XAML, ohne ein Änderungen an der ViewModel oder der Code-Behind-Datei ändern.

### <a name="a-color-viewmodel"></a>Eine Farbe ViewModel

Die [ `ColorViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs) in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) Bibliothek integriert die RGB und HSL-Farbe-Modelle. Es wird veranschaulicht, der [ **HslSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/HslSliders) Beispiel:

[![Dreifacher Screenshot des TK](images/ch18fg08-small.png "HSL-Farbe Modell")](images/ch18fg08-large.png#lightbox "HSL-Farbe-Modell")

### <a name="streamlining-the-viewmodel"></a>Das ViewModel rationalisieren

Der Code in ViewModels kann optimiert werden, durch die Definition einer `OnPropertyChanged` -Methode der [ `CallerMemberName` ](https://developer.xamarin.com/api/type/System.Runtime.CompilerServices.CallerMemberNameAttribute/) -Attribut, das den aufrufenden Eigenschaftennamen automatisch abgerufen. Die [ `ViewModelBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ViewModelBase.cs) -Klasse in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) Bibliothek wird dies ausgeführt und stellt eine Basisklasse für ViewModels.

## <a name="the-command-interface"></a>Die Befehlsschnittstelle

MVVM funktioniert mit datenbindungen und datenbindungen arbeiten mit Eigenschaften, sodass MVVM ungenügend werden scheint bei der Behandlung einer `Clicked` -Ereignis für ein `Button` oder ein `Tapped` -Ereignis für eine `TapGestureRecognizer`. Damit wird ViewModels solche Ereignisse behandeln, Xamarin.Forms unterstützt die *Befehlsschnittstelle*.

Die Befehlsschnittstelle äußert sich in der `Button` mit zwei öffentliche Eigenschaften:

- [`Command`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.Command/) Der Typ [ `ICommand` ](https://developer.xamarin.com/api/type/System.Windows.Input.ICommand/) (definiert der `System.Windows.Input` Namespace)
- [`CommandParameter`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.CommandParameter/) vom Typ `Object`

Um die Befehlsschnittstelle unterstützen zu können, muss ein ViewModel eine Eigenschaft vom Typ definieren `ICommand` , klicken Sie dann gebundenen Daten der `Command` Eigenschaft von der `Button`. Die `ICommand` Schnittstelle deklariert zwei Methoden und ein Ereignis:

- Ein [ `Execute` ](https://developer.xamarin.com/api/member/System.Windows.Input.ICommand.Execute/p/System.Object/) Methode mit einem Argument des Typs `object`
- Ein [ `CanExecute` ](https://developer.xamarin.com/api/member/System.Windows.Input.ICommand.CanExecute/p/System.Object/) Methode mit einem Argument des Typs `object` zurückgibt `bool`
- Ein [ `CanExecuteChanged` ](https://developer.xamarin.com/api/event/System.Windows.Input.ICommand.CanExecuteChanged/) Ereignis

Ein ViewModel legt intern, jede Eigenschaft des Typs `ICommand` mit einer Instanz einer Klasse, die implementiert die `ICommand` Schnittstelle. Durch die Datenbindung der `Button` zu Beginn aufgerufenen der `CanExecute` -Methode, und deaktiviert Sie sich an, wenn die-Methode zurückgibt `false`. Außerdem wird einen Handler für das `CanExecuteChanged` -Ereignis aus und ruft `CanExecute` dieses Ereignis wird ausgelöst, wenn. Wenn die `Button` ist aktiviert, ruft er die `Execute` Methode immer die `Button` geklickt wird.

Sie müssen möglicherweise einige ViewModels, die schon vor Xamarin.Forms verfügbar waren, und diese möglicherweise bereits die Befehlsschnittstelle unterstützen. Für neue ViewModels nur mit Xamarin.Forms verwendet werden soll, um Xamarin.Forms stellt einen [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) Klasse und ein [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command%3CT%3E/) Klasse implementiert, die `ICommand` Schnittstelle. Der generische Typ ist der Typ des Arguments für die `Execute` und `CanExecute` Methoden.

### <a name="simple-method-executions"></a>Einfache Methode Ausführungen

Die [ **PowersOfThree** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/PowersOfThree) Beispiel veranschaulicht, wie die in einem ViewModel-Schnittstelle. Die [ `PowersViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter18/PowersOfThree/PowersOfThree/PowersOfThree/PowersViewModel.cs) Klasse definiert zwei Eigenschaften des Typs `ICommand` und definiert außerdem zwei private Eigenschaften, die an die am einfachsten übergeben [ `Command` Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command.Command/p/System.Action/). Das Programm enthält datenbindungen, die von diesem ViewModel, um die `Command` Eigenschaften von zwei `Button` Elemente.

Die `Button` Elemente können leicht mit ersetzt `TapGestureRecognizer` Objekte in XAML ohne codeänderungen.

### <a name="a-calculator-almost"></a>Ein Rechner fast

Die [ **AddingMachine** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/AddingMachine) Beispiel macht verwenden beide die `Execute` und `CanExecute` Methoden der `ICommand`. Er verwendet ein [ `AdderViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AdderViewModel.cs) -Klasse in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AdderViewModel.cs) Bibliothek. Das ViewModel enthält sechs Eigenschaften vom Typ `ICommand`. Initialisiert diese aus der [ `Command` Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command.Command/p/System.Action/) und [ `Command` Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command.Command/p/System.Action/System.Func%7BSystem.Boolean%7D/) von `Command` und [ `Command<T>` Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command%3CT%3E.Command%3CT%3E/p/System.Action%7BT%7D/System.Func%7BT,System.Boolean%7D/) der `Command<T>`. Die numerischen Tasten des Hinzufügen eines Computers werden für die Eigenschaft, die mit initialisiert wird gebunden `Command<T>`, und ein `string` Argument `Execute` und `CanExecute` bestimmten Schlüssel identifiziert.

## <a name="viewmodels-and-the-application-lifecycle"></a>ViewModels und Anwendungslebenszyklus

Die `AdderViewModel` verwendet der **AddingMachine** Beispiel definiert außerdem zwei Methoden namens `SaveState` und `RestoreState`. Diese Methoden werden von der Anwendung aufgerufen, wenn er in den Ruhezustand wechselt und erneut wird gestartet.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 18 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch18-Apr2016.pdf)
- [Kapitel 18-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18)
