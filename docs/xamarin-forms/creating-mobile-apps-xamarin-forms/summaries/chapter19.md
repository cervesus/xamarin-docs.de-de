---
title: Zusammenfassung der Kapitel 19. Auflistungsansichten
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung der Kapitel 19. Auflistungsansichten'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 0AEC3A5C-586E-4D0F-9895-67E99A053A79
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
ms.openlocfilehash: 30fa9e86a82441052641c44a6c2ff8a682792267
ms.sourcegitcommit: 03dfb4a2c20ad68515875b415e7d84ee9b0a8cb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2018
ms.locfileid: "51563614"
---
# <a name="summary-of-chapter-19-collection-views"></a>Zusammenfassung der Kapitel 19. Auflistungsansichten

> [!NOTE] 
> Anmerkungen zu dieser Version auf dieser Seite Geben Sie Bereiche, in denen Xamarin.Forms aus den Informationen im Buch abweichend hat, an.

Xamarin.Forms definiert drei Ansichten, die Auflistungen verwalten und ihre Elemente anzuzeigen:

- [`Picker`](xref:Xamarin.Forms.Picker) ist eine relativ kurze Liste von Zeichenfolge-Elementen, die dem Benutzer ermöglicht, eine auswählen
- [`ListView`](xref:Xamarin.Forms.ListView) ist häufig eine lange Liste von Elementen in der Regel vom gleichen Typ und die Formatierung, damit auch der Benutzer eine auswählen
- [`TableView`](xref:Xamarin.Forms.TableView) ist eine Sammlung von *Zellen* (in der Regel von unterschiedlichen Typen und Darstellungen) Daten anzeigen oder Verwalten von Benutzereingaben

Es ist üblich für MVVM-Anwendungen verwenden die `ListView` eine auswählbare Auflistung von Objekten angezeigt.

## <a name="program-options-with-picker"></a>Optionen für das Programm mit der Auswahl

Die [ `Picker` ](xref:Xamarin.Forms.Picker) ist eine gute Wahl, wenn Sie dem Benutzer ermöglichen, wählen Sie eine Option aus einer relativ kurzen Liste der müssen `string` Elemente.

### <a name="the-picker-and-event-handling"></a>Die Auswahl und die Behandlung von Ereignissen

Die [ **PickerDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerDemo) Beispiel veranschaulicht, wie von XAML zum Festlegen der `Picker` [ `Title` ](xref:Xamarin.Forms.Picker.Title) Eigenschaft und hinzufügen `string` Elemente, die die [ `Items` ](xref:Xamarin.Forms.Picker.Items) Auflistung. Wenn der Benutzer wählt die `Picker`, zeigt die Elemente in der `Items` Auflistung in einer Weise plattformabhängige.

Die [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) Ereignis gibt an, wenn der Benutzer ein Element ausgewählt wurde. Der nullbasierte [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) Eigenschaft gibt dann das ausgewählte Element an. Wenn kein Element ausgewählt ist, `SelectedIndex` gleich &ndash;1.

Sie können auch `SelectedIndex` initialisieren Sie das ausgewählte Element, aber es muss festgelegt werden, nachdem die `Items` Auflistung gefüllt wird. In XAML, dies bedeutet, dass Sie wahrscheinlich ein Eigenschaftenelement festzulegende verwenden `SelectedIndex`.

### <a name="data-binding-the-picker"></a>Die Auswahl für die Datenbindung

Die `SelectedIndex` Eigenschaft durch eine bindbare Eigenschaft unterstützt wird, aber `Items` nicht der Fall ist, also mit der Datenbindung mit einem `Picker` ist schwierig. Eine Lösung ist die Verwendung der `Picker` in Kombination mit einem [ `ObjectToIndexConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ObjectToIndexConverter.cs) wie in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Bibliothek. Die [ **PickerBinding** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerBinding) wird veranschaulicht, wie dies funktioniert.

> [!NOTE] 
> Die Xamarin.Forms `Picker` enthält jetzt `ItemsSource` und `SelectedItem` Eigenschaften, die die Datenbindung zu unterstützen. Finden Sie unter [Auswahl](~/xamarin-forms/user-interface/picker/index.md).

## <a name="rendering-data-with-listview"></a>Rendern von Daten mit ListView

Die [ `ListView` ](xref:Xamarin.Forms.ListView) ist die einzige abgeleitete Klasse [ `ItemsView<TVisual>` ](xref:Xamarin.Forms.ItemsView`1) von der geerbt der [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) und [ `ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) Eigenschaften.

`ItemsSource` ist vom Typ `IEnumerable` aber `null` in der Standardeinstellung und muss explizit initialisiert oder auf eine Auflistung (häufiger) festgelegt werden, bis eine Datenbindung. Die Elemente in dieser Auflistung können eines beliebigen Typs sein.

`ListView` definiert eine [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem) Eigenschaft, die entweder festgelegt wird, um eines der Elemente in der `ItemsSource` Auflistung oder `null` , wenn kein Element ausgewählt ist. `ListView` löst die [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) Ereignis aus, wenn ein neues Element ausgewählt ist.

### <a name="collections-and-selections"></a>Sammlungen und Auswahl

Die [ **ListViewList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewList) Beispiel füllt ein `ListView` mit 17 `Color` Werte in einer `List<Color>` Auflistung. Die Elemente können ausgewählt werden, sondern standardmäßig mit ihren unschöne angezeigt `ToString` Darstellungen. Beispiele in diesem Kapitel werden die zu beheben, die anzeigen, und legen Sie ihn nach Bedarf so attraktiv veranschaulicht.

### <a name="the-row-separator"></a>Das Zeilentrennzeichen

Unter iOS und Android zeigt werden von eine dünne Linie Zeilen getrennt. Sie können steuern, mit der [ `SeparatorVisibility` ](xref:Xamarin.Forms.ListView.SeparatorVisibility) und [ `SeparatorColor` ](xref:Xamarin.Forms.ListView.SeparatorColor) Eigenschaften. `SeparatorVisibility` Eigenschaft ist vom Typ [ `SeparatorVisibility` ](xref:Xamarin.Forms.SeparatorVisibility), eine Enumeration mit zwei Membern:

- [`Default`](xref:Xamarin.Forms.SeparatorVisibility.Default), die Standardeinstellung
- [`None`](xref:Xamarin.Forms.SeparatorVisibility.None)

### <a name="data-binding-the-selected-item"></a>Das ausgewählte Element binden von Daten

Die `SelectedItem` Eigenschaft wird durch eine bindbare Eigenschaft und gesichert, sodass sie entweder die Quelle oder Ziel einer Bindung werden kann. Die standardmäßige `BindingMode` ist `OneWayToSource`, jedoch ist es im Allgemeinen das Ziel eine bidirektionale Datenbindung, insbesondere in MVVM-Szenarien. Die [ **ListViewArray** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewArray) veranschaulicht diese Art von Bindung.

### <a name="the-observablecollection-difference"></a>Der Unterschied ObservableCollection

Die [ **ListViewLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewLogger) Beispiel legt die `ItemsSource` Eigenschaft eine `ListView` auf eine `List<DateTime>` Auflistung und progressiv fügt dann ein neues `DateTime` Objekt, das der Auflistung jede Sekunde einen Timer verwendet.

Allerdings die `ListView` nicht automatisch aktualisiert werden, selbst da die `List<T>` Sammlung verfügt nicht über einen Benachrichtigungsmechanismus, um anzugeben, wenn Elemente hinzugefügt oder aus der Auflistung entfernt werden.

Eine viel bessere Klasse für die Verwendung in einem solchen Szenario ist [ `ObservableCollection<T>` ](xref:System.Collections.ObjectModel.ObservableCollection`1) definiert, der `System.Collections.ObjectModel` Namespace. Diese Klasse implementiert die [ `INotifyCollectionChanged` ](xref:System.Collections.Specialized.INotifyCollectionChanged) -Schnittstelle, und daher löst eine [ `CollectionChanged` ](xref:System.Collections.ObjectModel.ObservableCollection`1.CollectionChanged) Ereignis aus, wenn Elemente hinzugefügt oder entfernt aus der Auflistung oder ein, wenn sie ersetzt werden, oder innerhalb verschoben werden die Auflistung. Bei der `ListView` erkennt intern, die eine Klasse implementieren `INotifyCollectionChanged` festgelegt wurde die `ItemsSource` -Eigenschaft, fügt es einen Handler, der `CollectionChanged` Ereignis und die Anzeige aktualisiert, wenn die Auflistung geändert.

Die [ **ObservableLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ObservableLogger) Beispiel veranschaulicht die Verwendung von `ObservableCollection`.

### <a name="templates-and-cells"></a>Vorlagen und Zellen

Standardmäßig eine `ListView` Zeigt Elemente in der Auflistung, die mit der jedes Elements `ToString` Methode. Ein besserer Ansatz umfasst eine Vorlage zum Anzeigen von Elementen definieren.

Mit diesem Feature experimentieren möchten, können Sie die [ `NamedColor` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs) -Klasse in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Bibliothek. Diese Klasse definiert eine statische `All` Eigenschaft vom Typ `IList<NamedColor>` , 141 enthält `NamedColor` Objekte, die öffentlichen Felder des für die `Color` Struktur.

Die [ **NaiveNamedColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/NaiveNamedColorList) Beispiel legt die `ItemsSource` von einer `ListView` dieser `NamedColor.All` -Eigenschaft, aber nur die den vollqualifizierten Klassennamen der der `NamedColor` Objekte sind angezeigt.

`ListView` benötigt eine Vorlage zum Anzeigen dieser Elemente. Im Code können Sie festlegen der [ `ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) von definierte Eigenschaft `ItemsView<TVisual>` auf eine [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) -Objekt unter Verwendung der [ `DataTemplate` Konstruktor](xref:Xamarin.Forms.DataTemplate.%23ctor(System.Type)) , verweist auf eine Ableitung von der [ `Cell` ](xref:Xamarin.Forms.Cell) Klasse. `Cell` verfügt über fünf ableitungen aus:

- [`TextCell`](xref:Xamarin.Forms.TextCell) &mdash; enthält zwei `Label` Ansichten (konzeptueller Sicht)
- [`ImageCell`](xref:Xamarin.Forms.ImageCell) &mdash; Fügt eine `Image` Ansicht `TextCell`
- [`EntryCell`](xref:Xamarin.Forms.EntryCell) &mdash; enthält eine `Entry` mit Anzeigen einer `Label`
- [`SwitchCell`](xref:Xamarin.Forms.SwitchCell) &mdash; enthält eine `Switch` mit einer `Label`
- [`ViewCell`](xref:Xamarin.Forms.ViewCell) &mdash; kann `View` (wahrscheinlich mit untergeordneten Elementen)

Rufen Sie anschließend [ `SetValue` ](xref:Xamarin.Forms.DataTemplate.SetValue(Xamarin.Forms.BindableProperty,System.Object)) und [ `SetBinding` ](xref:Xamarin.Forms.DataTemplate.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) auf die `DataTemplate` Objekt, das Zuordnen von Werten mit der `Cell` Eigenschaften oder Festlegen der datenbindungen, die auf die `Cell` Eigenschaften verweisen auf Eigenschaften der Elemente in der `ItemsSource` Auflistung. Dies wird veranschaulicht, der [ **TextCellListCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListCode) Beispiel.

Wie jedes Element, durch angezeigt wird die `ListView`, eine kleine visuelle Struktur aus der Vorlage erstellt wird und datenbindungen, die zwischen dem Element und die Eigenschaften der Elemente in dieser visuellen Struktur hergestellt werden. Sie erhalten einen Überblick über diesen Prozess durch die Installation von Handlern für die [ `ItemAppearing` ](xref:Xamarin.Forms.ListView.ItemAppearing) und [ `ItemDisappearing` ](xref:Xamarin.Forms.ListView.ItemDisappearing) Ereignisse der `ListView`, oder indem Sie eine Alternative [ `DataTemplate`Konstruktor](xref:Xamarin.Forms.DataTemplate.%23ctor(System.Func{System.Object})) , verwendet eine Funktion, die jedes Mal aufgerufen wird, ein Element der visuellen Struktur erstellt werden muss.

Die [ **TextCellListXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListXaml) zeigt ein funktionell Programm vollständig in XAML. Ein `DataTemplate` Tag festgelegt ist, um die `ItemTemplate` Eigenschaft der `ListView`, und klicken Sie dann die `TextCell` nastaven NA hodnotu der `DataTemplate`. Bindung an Eigenschaften der Elemente in der Auflistung festgelegt sind, direkt auf die [ `Text` ](xref:Xamarin.Forms.TextCell.Text) und [ `Detail` ](xref:Xamarin.Forms.TextCell.Detail) Eigenschaften der `TextCell`.

### <a name="custom-cells"></a>Benutzerdefinierte Zellen

In XAML ist es möglich, festzulegen eine [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) auf die `DataTemplate` und definieren Sie dann eine benutzerdefinierte visuelle Struktur als die [ `View` ](xref:Xamarin.Forms.ViewCell.View) Eigenschaft `ViewCell`. (`View` ist die Content-Eigenschaft des `ViewCell` daher `ViewCell.View` Tags sind nicht erforderlich.) Die [ **CustomNamedColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CustomNamedColorList) Beispiel wird diese Technik veranschaulicht:

[![Dreifacher Screenshot der Liste von benutzerdefinierten benannte Farbe](images/ch19fg11-small.png "benutzerdefinierte benannte Farbenliste")](images/ch19fg11-large.png#lightbox "benutzerdefinierte benannte Farbenliste")

Die Bestimmung der Systemgröße für alle Plattformen kann knifflig sein. Die [ `RowHeight` ](xref:Xamarin.Forms.ListView.RowHeight) Eigenschaft ist nützlich, aber in einigen Fällen sollten Sie zum Verwenden der [ `HasUnevenRows` ](xref:Xamarin.Forms.ListView.HasUnevenRows) -Eigenschaft, die weniger effizient ist, aber erzwingt, dass die `ListView` , die Größe der Zeilen. Für iOS und Android müssen Sie eine dieser zwei Eigenschaften verwenden, um größenanpassung von entsprechenden Zeilen abzurufen.

### <a name="grouping-the-listview-items"></a>Die ListView-Elemente zu gruppieren

`ListView` unterstützt die Gruppierung von Elementen und zum Navigieren zwischen diesen Gruppen. Die `ItemsSource` Eigenschaft muss festgelegt werden, um eine Auflistung von Auflistungen: das Objekt, `ItemsSource` nastaven NA hodnotu muss implementieren `IEnumerable`, und jedes Element in der Auflistung muss auch implementieren `IEnumerable`. Jede Gruppe sollte zwei Eigenschaften enthalten: eine textbeschreibung der Gruppe und eine drei Buchstaben bestehende Abkürzung.

Die [ `NamedColorGroup` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColorGroup.cs) -Klasse in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Bibliothek erstellt sieben Gruppen von `NamedColor` Objekte. Die [ **ColorGroupList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorGroupList) Beispiel veranschaulicht, wie diese Gruppen mit den [ `IsGroupingEnabled` ](xref:Xamarin.Forms.ListView.IsGroupingEnabled) Eigenschaft `ListView` festgelegt `true`, und die [ `GroupDisplayBinding` ](xref:Xamarin.Forms.ListView.GroupDisplayBinding) und [ `GroupShortNameBinding` ](xref:Xamarin.Forms.ListView.GroupShortNameBinding) Eigenschaften gebunden, um Eigenschaften in jeder Gruppe.

### <a name="custom-group-headers"></a>Benutzerdefinierte Kopfzeilen

Es ist möglich, Erstellen von benutzerdefinierten Header für die `ListView` gruppiert nach und Ersetzen Sie dabei die `GroupDisplayBinding` Eigenschaft mit dem die [ `GroupHeaderTemplate` ](xref:Xamarin.Forms.ListView.GroupHeaderTemplate) definieren eine Vorlage für die Header.

### <a name="listview-and-interactivity"></a>ListView und Interaktivität

Eine Anwendung erhält in der Regel Interaktionen der Benutzer mit einer `ListView` -inhaltsobjektereignisse einen Handler, der die `ItemSelected` oder [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) Ereignis wird oder indem Sie eine Datenbindung für die `SelectedItem` Eigenschaft. Aber einige Zellentypen (`EntryCell` und `SwitchCell`) Benutzerinteraktion möglich ist, und es ist auch möglich, benutzerdefinierte Zellen, die selbst erstellen möchten, die mit dem Benutzer interagieren. Die [ **InteractiveListView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/InteractiveListView) erstellt 100 Instanzen von [ `ColorViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs) und ermöglicht dem Benutzer so ändern Sie jede Farbe, die mithilfe von drei `Slider` Elemente. Das Programm verwendet auch die [ `ColorToContrastColorConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorToContrastColorConverter.cs) in die [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit).

## <a name="listview-and-mvvm"></a>ListView und MVVM

`ListView` spielt eine wichtige Rolle in MVVM-Szenarien. Wenn ein `IEnumerable` Auflistung in ein "ViewModel" vorhanden ist, häufig eine Datenbindung erfolgt eine `ListView`. Darüber hinaus die Elemente in der Auflistung häufig implementieren `INotifyPropertyChanged` an Eigenschaften in einer Vorlage gebunden werden soll.

### <a name="a-collection-of-viewmodels"></a>Eine Auflistung von ViewModels

Um dies zu untersuchen der [ **SchoolOfFineArts** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) Bibliothek erstellt mehrere Klassen, die basierend auf einer [XML-Datendatei](http://xamarin.github.io/xamarin-forms-book-samples/SchoolOfFineArt/students.xml) und Bilder der fiktiven Schüler/Studenten an dieser fiktiven School.

Die [ `Student` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/Student.cs) Klasse leitet sich von [ `ViewModelBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/ViewModelBase.cs). Die [ `StudentBody` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/StudentBody.cs) Klasse ist eine Sammlung von `Student` Objekte und auch abgeleitet `ViewModelBase`. Die [ `SchoolViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/SchoolViewModel.cs) lädt die XML-Datei herunter, und assembliert Sie alle Objekte.

Die [ **StudentList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/StudentList) Programm verwendet eine `ImageCell` zum Anzeigen der Schüler/Studenten und ihre Images in einem `ListView`:

[![Dreifacher Screenshot der Liste der Studenten](images/ch19fg18-small.png "\"Student\" Liste")](images/ch19fg18-large.png#lightbox "\"Student\"-Liste")

Die [ **ListViewHeader** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewHeader) -Beispiel wird eine [ `Header` ](xref:Xamarin.Forms.ListView.Header) Eigenschaft jedoch nur unter Android erscheint.

### <a name="selection-and-the-binding-context"></a>Auswahl und der Bindungskontext

Die [ **SelectedStudentDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/SelectedStudentDetail) Programmieren bindet die `BindingContext` von eine `StackLayout` auf die `SelectedItem` Eigenschaft der `ListView`. Dadurch wird die ausführliche Informationen zu den ausgewählten Studenten angezeigt.

### <a name="context-menus"></a>Kontextmenüs

Eine Zelle kann ein Kontextmenü definieren, die auf eine plattformspezifische Weise implementiert wird. Fügen Sie zum Erstellen dieses Menüs [ `MenuItem` ](xref:Xamarin.Forms.MenuItem) Objekte die [ `ContextActions` ](xref:Xamarin.Forms.Cell.ContextActions) Eigenschaft der `Cell`.

`MenuItem` werden fünf Eigenschaften definiert:

- [`Text`](xref:Xamarin.Forms.MenuItem.Text) Der Typ `string`
- [`Icon`](xref:Xamarin.Forms.MenuItem.Icon) Der Typ `FileImageSource`
- [`IsDestructive`](xref:Xamarin.Forms.MenuItem.IsDestructive) Der Typ `bool`
- [`Command`](xref:Xamarin.Forms.MenuItem.Command) Der Typ `ICommand`
- [`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter) Der Typ `object`

Die `Command` und `CommandParameter` Eigenschaften impliziert, dass das "ViewModel" für jedes Element Methoden zum Ausführen der gewünschten Menüs-Befehle enthält. In nicht-MVVM-Szenarien `MenuItem` definiert auch eine [ `Clicked` ](xref:Xamarin.Forms.MenuItem.Clicked) Ereignis.

Die [ **CellContextMenu** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CellContextMenu) wird diese Technik veranschaulicht. Die `Command` Eigenschaft der einzelnen `MenuItem` gebunden ist, um eine Eigenschaft des Typs `ICommand` in die `Student` Klasse. Legen Sie die `IsDestructive` Eigenschaft `true` für eine `MenuItem` , entfernt oder löscht das ausgewählte Objekt.

### <a name="varying-the-visuals"></a>Variieren die visuellen Elemente

Manchmal möchten Sie geringfügige abweichungen in den Visualisierungen der Elemente in der `ListView` anhand einer Eigenschaft. Z. B. wenn eine Student-Grade Point Average unterschreitet 2.0 die [ **ColorCodedStudents** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorCodedStudents) Beispiel werden diese Student Name in Rot angezeigt.
Dies erfolgt durch die Verwendung eines Wertkonverters Bindung, [ `ThresholdToObjectConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ThresholdToObjectConverter.cs)in die [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Bibliothek.

### <a name="refreshing-the-content"></a>Den Inhalt wird aktualisiert.

Die `ListView` eine Geste für eine Dropdownmenü für das Aktualisieren von Daten unterstützt. Legen Sie das Programm muss die [ `IsPullToRefresh` ](xref:Xamarin.Forms.ListView.IsPullToRefreshEnabled) Eigenschaft `true` um dies zu ermöglichen. Der `ListView` reagiert auf das Pulldownmenü Gesten durch Festlegen der [ `IsRefreshing` ](xref:Xamarin.Forms.ListView.IsRefreshing) Eigenschaft `true`, und durch Auslösen der [ `Refreshing` ](xref:Xamarin.Forms.ListView.Refreshing) Ereignis und (für MVVM-Szenarien) aufrufen die `Execute` -Methode der seine [ `RefreshCommand` ](xref:Xamarin.Forms.ListView.RefreshCommand) Eigenschaft.

Code behandeln die `Refresh` Ereignis oder die `RefreshCommand` möglicherweise aktualisiert dann die angezeigten Daten durch die `ListView` und legt `IsRefreshing` an `false`.

Die [ **RSS-Feed** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/RssFeed) Beispiel zeigt die Verwendung einer [ `RssFeedViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/RssFeed/RssFeed/RssFeed/RssFeedViewModel.cs) implementiert `RefreshCommand` und `IsRefreshing` Eigenschaften für die Datenbindung.

## <a name="the-tableview-and-its-intents"></a>Die TableView und seine Absichten

Während der `ListView` zeigt in der Regel mehrere Instanzen des gleichen Typs, der [ `TableView` ](xref:Xamarin.Forms.TableView) konzentriert sich in der Regel auf das Bereitstellen einer Benutzeroberfläche für mehrere Eigenschaften verschiedener Typen. Jedes Element bezieht sich auf eine eigene [ `Cell` ](xref:Xamarin.Forms.Cell) Ableitung für die Eigenschaft anzeigen oder Bereitstellen einer Benutzeroberfläche.

### <a name="properties-and-hierarchies"></a>Eigenschaften und Hierarchien

`TableView` definiert nur vier Eigenschaften:

- [`Intent`](xref:Xamarin.Forms.TableView.Intent) Der Typ [ `TableIntent` ](xref:Xamarin.Forms.TableIntent), eine Enumeration
- [`Root`](xref:Xamarin.Forms.TableView.Root) Der Typ [ `TableRoot` ](xref:Xamarin.Forms.TableRoot), die Content-Eigenschaft `TableView`
- [`RowHeight`](xref:Xamarin.Forms.TableView.RowHeight) Der Typ `int`
- [`HasUnevenRows`](xref:Xamarin.Forms.TableView.HasUnevenRows) Der Typ `bool`

Die `TableIntent` Enumeration gibt an, wie Sie verwenden möchten die `TableView`:

- [`Data`](xref:Xamarin.Forms.TableIntent.Data)
- [`Form`](xref:Xamarin.Forms.TableIntent.Form)
- [`Settings`](xref:Xamarin.Forms.TableIntent.Settings)
- [`Menu`](xref:Xamarin.Forms.TableIntent.Menu)

Diese Elemente auch vorschlagen, einige Verwendungsmöglichkeiten für die `TableView`.

Verschiedene weitere Klassen sind beteiligt, bei der Definition einer Tabelle:

- [`TableSectionBase`](xref:Xamarin.Forms.TableSectionBase) ist eine abstrakte Klasse, die von abgeleitet `BindableObject` und definiert eine [ `Title` ](xref:Xamarin.Forms.TableSectionBase.Title) Eigenschaft

- [`TableSectionBase<T>`](xref:Xamarin.Forms.TableSectionBase`1) ist eine abstrakte Klasse, die von abgeleitet `TableSectionBase` und implementiert `IList<T>` und `INotifyCollectionChanged`

- [`TableSection`](xref:Xamarin.Forms.TableSection) leitet sich von `TableSectionBase<Cell>`

- [`TableRoot`](xref:Xamarin.Forms.TableRoot) leitet sich von `TableSectionBase<TableSection>`

Kurz gesagt, `TableView` verfügt über eine `Root` -Eigenschaft, die Sie, um festlegen eine `TableRoot` -Objekt, das eine Auflistung von `TableSection` Objekte, von denen jede eine Sammlung von ist `Cell` Objekte. Eine Tabelle enthält mehrere Abschnitte, und jeder Abschnitt enthält mehrere Zellen. Die Tabelle selbst kann einen Titel, und jeder Abschnitt kann einen Titel haben. Obwohl `TableView` nutzt `Cell` ableitungen handeln, es macht nicht nutzen `DataTemplate`.

### <a name="a-prosaic-form"></a>Ein prosaischen Formular

Die [ **EntryForm** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/EntryForm) Beispiel definiert eine [ `PersonalInformation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs) Ansichtsmodell, eine Instanz, die wird die `BindingContext` von der `TableView`. Jede `Cell` Ableitung in seine `TableSection` haben Bindungen auf Eigenschaften klicken Sie dann die `PersonalInformation` Klasse.

### <a name="custom-cells"></a>Benutzerdefinierte Zellen

Die [ **ConditionalCells** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalCells) Beispiel baut auf **EntryForm**. Die [ `ProgrammerInformation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs) Klasse enthält eine boolesche Eigenschaft, die die Anwendbarkeit der zwei zusätzliche Eigenschaften bestimmt. Für diese zwei zusätzlichen Eigenschaften, die Anwendung verwendet eine benutzerdefinierte `PickerCell` basierend auf einer [PickerCell.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml) und [PickerCell.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml.cs) in die [  **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Bibliothek.

Obwohl die `IsEnabled` Eigenschaften der beiden `PickerCell` Elemente gebunden sind, um die boolesche Eigenschaft im `ProgrammerInformation`, dieses Verfahren scheint nicht funktioniert, das nächste Beispiel aufgefordert.

### <a name="conditional-sections"></a>Bedingungsabschnitte

Die [ **ConditionalSection** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalSection) Beispiel fügt die beiden Elemente, die von der Auswahl des Elements in einer separaten booleschen `TableSection`. Die Code-Behind-Datei wird entfernt, in diesem Abschnitt aus der `TableView` oder hinzugefügt, es wieder auf die boolesche Eigenschaft basiert.

### <a name="a-tableview-menu"></a>Ein Menü TableView

Eine andere Verwendung von einem `TableView` ist ein Menü. Die [ **MenuCommands** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/MenuCommands) Beispiel zeigt ein Menü, das Sie ein wenig wechseln kann `BoxView` auf dem Bildschirm.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 19 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch19-Apr2016.pdf)
- [Kapitel 19-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19)
- [Auswahl](~/xamarin-forms/user-interface/picker/index.md)
- [ListView](~/xamarin-forms/user-interface/listview/index.md)
- [TableView](~/xamarin-forms/user-interface/tableview.md)
