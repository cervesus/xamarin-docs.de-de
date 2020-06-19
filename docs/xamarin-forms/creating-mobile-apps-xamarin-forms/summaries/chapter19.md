---
title: 'title: "Zusammenfassung von Kapitel 19. Sammlungsansichten"description: "Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung von Kapitel 19.'
description: 'Sammlungsansichten" ms.prod: xamarin ms.technology: xamarin-forms ms.assetid: 0AEC3A5C-586E-4D0F-9895-67E99A053A79 author: davidbritch ms.author: dabritch ms.date: 07/18/2018 no-loc: [Xamarin.Forms, Xamarin.Essentials] Zusammenfassung von Kapitel 19.'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 0AEC3A5C-586E-4D0F-9895-67E99A053A79
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0eafdeffb6783a0ed54fdf23e6d10de24e2b4c6f
ms.sourcegitcommit: ea9269b5d9e3d68b61bb428560a10034117ee457
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2020
ms.locfileid: "84136694"
---
# <a name="summary-of-chapter-19-collection-views"></a>Auflistungsansichten [![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19)

In den Anmerkungen auf dieser Seite wird beschrieben, inwiefern die Angaben innerhalb des Buchs heute nicht mehr für Xamarin.Forms gelten.

> [!NOTE] 
> In Xamarin.Forms sind drei Ansichten definiert, mit denen Auflistungen verwaltet und die zugehörigen Elemente angezeigt werden:

[`Picker`](xref:Xamarin.Forms.Picker) ist eine relativ kurze Liste mit Zeichenfolgenelementen, aus denen der Benutzer ein Element auswählen kann

- [`ListView`](xref:Xamarin.Forms.ListView) ist häufig eine lange Liste mit Elementen, die normalerweise denselben Typ und dieselbe Formatierung aufweisen. Aus dieser Liste können Benutzer ebenfalls ein Element auswählen
- [`TableView`](xref:Xamarin.Forms.TableView) ist eine Auflistung von *Zellen* (in der Regel verschiedene Typen und Erscheinungen), mit denen Daten angezeigt und Benutzereingaben verwaltet werden können
- MVVM-Anwendungen verwenden häufig `ListView`, um eine auswählbare Auflistung von Objekten anzuzeigen.

Programmoptionen bei Picker

## <a name="program-options-with-picker"></a>[`Picker`](xref:Xamarin.Forms.Picker) ist gut geeignet, wenn Sie es Benutzern ermöglichen möchten, eine Option aus einer relativ kurzen Liste von `string`-Elementen auszuwählen.

Die Picker-Ansicht und die Ereignisbehandlung

### <a name="the-picker-and-event-handling"></a>Das Beispiel [**PickerDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerDemo) zeigt, wie Sie mithilfe von XAML die `Picker`-Eigenschaft [`Title`](xref:Xamarin.Forms.Picker.Title) festlegen und `string`-Elemente zur [`Items`](xref:Xamarin.Forms.Picker.Items)-Auflistung hinzufügen.

Wenn der Benutzer `Picker` auswählt, werden die Elemente in der `Items`-Auflistung plattformabhängig angezeigt. Das [`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged)-Ereignis zeigt an, wenn der Benutzer ein Element ausgewählt hat.

Der nullbasierte [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex)-Eigenschaft zeigt dann das ausgewählte Element an. Wenn kein Element ausgewählt wird, ist das Ergebnis von `SelectedIndex` der Wert &ndash;1. Sie können auch die Eigenschaft `SelectedIndex` verwenden, um das ausgewählte Element zu initialisieren. Diese Eigenschaft muss jedoch nach dem Füllen der `Items`-Auflistung festgelegt werden.

In XAML bedeutet dies, dass Sie `SelectedIndex` wahrscheinlich mit einem Eigenschaftselement festlegen. Datenbindung und Picker

### <a name="data-binding-the-picker"></a>Die `SelectedIndex`-Eigenschaft wird durch eine bindbare Eigenschaft gestützt, `Items` jedoch nicht. Daher ist die Verwendung der Datenbindung mit `Picker` schwierig.

Eine Lösung besteht darin, `Picker` in Kombination mit einem [`ObjectToIndexConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ObjectToIndexConverter.cs) zu verwenden, z.B. mit dem in der [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)-Bibliothek enthaltenen Konverter. Das Beispiel [**PickerBinding**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerBinding) zeigt die Funktionsweise. Die Xamarin.Forms-Ansicht `Picker` umfasst nun die Eigenschaften `ItemsSource` und `SelectedItem`, die eine Datenbindung unterstützen.

> [!NOTE] 
> Siehe [Picker](~/xamarin-forms/user-interface/picker/index.md). Rendern von Daten mit ListView

## <a name="rendering-data-with-listview"></a>[`ListView`](xref:Xamarin.Forms.ListView) ist die einzige Klasse, die von der Klasse [`ItemsView<TVisual>`](xref:Xamarin.Forms.ItemsView`1) abgeleitet wird, von der sie die Eigenschaften [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) und [`ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) erbt.

`ItemsSource` weist den Typ `IEnumerable`, standardmäßig jedoch den Wert `null` auf und muss explizit initialisiert werden bzw. (die häufigere Vorgehensweise) mithilfe einer Datenbindung auf eine Auflistung festgelegt werden.

Die Elemente in dieser Auflistung können einen beliebigen Typ aufweisen. `ListView` definiert eine [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem)-Eigenschaft, die entweder auf eins der Elemente in der `ItemsSource`-Auflistung oder auf `null` festgelegt ist, wenn kein Element ausgewählt wird.

Wenn ein neues Element ausgewählt wird, löst `ListView` das Ereignis [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) aus. Auflistungen und Auswahlmöglichkeiten

### <a name="collections-and-selections"></a>Im Beispiel [**ListViewList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewList) wird eine `ListView` mit 17 `Color`-Werten in einer `List<Color>`-Auflistung gefüllt.

Die Elemente können ausgewählt werden, werden standardmäßig jedoch in ihren weniger schönen `ToString`-Darstellungen angezeigt. In verschiedenen Beispielen innerhalb dieses Kapitels wird gezeigt, wie sich diese Darstellung ändern lässt, um die Elemente in der gewünschten Form anzuzeigen. Das Zeilentrennzeichen

### <a name="the-row-separator"></a>Bei iOS und Android werden die Zeilen durch eine dünne Linie getrennt.

Sie können dieses Verhalten über die Eigenschaften [`SeparatorVisibility`](xref:Xamarin.Forms.ListView.SeparatorVisibility) und [`SeparatorColor`](xref:Xamarin.Forms.ListView.SeparatorColor) steuern. Die Eigenschaft `SeparatorVisibility` weist den Typ [`SeparatorVisibility`](xref:Xamarin.Forms.SeparatorVisibility) auf, eine Enumeration, die über zwei Member verfügt: [`Default`](xref:Xamarin.Forms.SeparatorVisibility.Default), die Standardeinstellung

- Datenbindung für das ausgewählte Element
- [`None`](xref:Xamarin.Forms.SeparatorVisibility.None)

### <a name="data-binding-the-selected-item"></a>Die Eigenschaft `SelectedItem` wird durch eine bindbare Eigenschaft gestützt und kann folglich die Quelle oder das Ziel einer Datenbindung sein.

Für `BindingMode` ist standardmäßig `OneWayToSource` festgelegt, im Allgemeinen ist die Eigenschaft jedoch das Ziel einer bidirektionalen Datenbindung (insbesondere in einem MVVM-Szenario). Diese Art von Bindung wird im Beispiel [**ListViewArray**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewArray) veranschaulicht. Die Klasse „ObservableCollection“

### <a name="the-observablecollection-difference"></a>Im Beispiel [**ListViewLogger**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewLogger) wird die Eigenschaft `ItemsSource` einer `ListView` auf eine `List<DateTime>`-Auflistung festgelegt. Anschließend wird fortwährend mithilfe eines Timers jede Sekunde ein neues `DateTime`-Objekt hinzugefügt.

`ListView` aktualisiert sich jedoch nicht automatisch selbst, da die `List<T>`-Auflistung über keinen Benachrichtigungsmechanismus verfügt, der anzeigt, wenn Elemente zur Auflistung hinzugefügt oder aus der Auflistung entfernt werden.

In einem solchen Szenario sollte stattdessen die Klasse [`ObservableCollection<T>`](xref:System.Collections.ObjectModel.ObservableCollection`1) verwendet werden, die im `System.Collections.ObjectModel`-Namespace definiert ist.

Diese Klasse implementiert die [`INotifyCollectionChanged`](xref:System.Collections.Specialized.INotifyCollectionChanged)-Schnittstelle und löst daher ein [`CollectionChanged`](xref:System.Collections.ObjectModel.ObservableCollection`1.CollectionChanged)-Ereignis aus, wenn Elemente zur Auflistung hinzugefügt oder aus der Auflistung entfernt werden. Auch wenn Elemente ersetzt oder innerhalb der Auflistung verschoben werden, wird ein Ereignis ausgelöst. Wenn `ListView` intern erkennt, dass eine Klasse, die `INotifyCollectionChanged` implementiert, auf ihre Eigenschaft `ItemsSource` festgelegt wurde, fügt ListView einen Handler zum `CollectionChanged`-Ereignis hinzu und aktualisiert die Anzeige, sobald die Auflistung sich ändert. Die Verwendung von `ObservableCollection` wird im Beispiel [**ObservableLogger**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ObservableLogger) veranschaulicht.

Vorlagen und Zellen

### <a name="templates-and-cells"></a>Eine `ListView`-Klasse zeigt die Elemente in ihrer Auflistung standardmäßig mithilfe der `ToString`-Methode der einzelnen Elemente an.

Ein besserer Ansatz ist die Definition einer Vorlage, um die Elemente anzuzeigen. Um diese Funktion auszuprobieren, können Sie die Klasse [`NamedColor`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs) in der [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)-Bibliothek verwenden.

Diese Klasse definiert eine statische `All`-Eigenschaft vom Typ `IList<NamedColor>` mit 141 `NamedColor`-Objekten, die den öffentlichen Feldern der `Color`-Struktur entsprechen. Im Beispiel [**NaiveNamedColorList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/NaiveNamedColorList) wird `ItemsSource` einer `ListView`-Klasse auf diese `NamedColor.All`-Eigenschaft festgelegt. Es werden jedoch nur die vollqualifizierten Klassennamen der `NamedColor`-Objekte angezeigt.

`ListView` benötigt eine Vorlage, um diese Elemente anzuzeigen.

Im Code können Sie die von `ItemsView<TVisual>` definierte Eigenschaft [`ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) auf ein [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)-Objekt festlegen. Verwenden Sie dazu den [`DataTemplate`-Konstruktor](xref:Xamarin.Forms.DataTemplate.%23ctor(System.Type)), der auf eine Ableitung der [`Cell`](xref:Xamarin.Forms.Cell)-Klasse verweist. `Cell` verfügt über fünf Ableitungen: [`TextCell`](xref:Xamarin.Forms.TextCell) &mdash; umfasst (konzeptionell) zwei `Label`-Ansichten

- [`ImageCell`](xref:Xamarin.Forms.ImageCell) &mdash; fügt eine `Image`-Ansicht zu `TextCell` hinzu
- [`EntryCell`](xref:Xamarin.Forms.EntryCell) &mdash; umfasst eine `Entry`-Ansicht mit einem `Label`
- [`SwitchCell`](xref:Xamarin.Forms.SwitchCell) &mdash; umfasst einen `Switch` mit einem `Label`
- [`ViewCell`](xref:Xamarin.Forms.ViewCell) &mdash; kann eine beliebige `View` sein (wahrscheinlich mit untergeordneten Elementen)
- Rufen Sie anschließend [`SetValue`](xref:Xamarin.Forms.DataTemplate.SetValue(Xamarin.Forms.BindableProperty,System.Object)) und [`SetBinding`](xref:Xamarin.Forms.DataTemplate.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) für das Objekt `DataTemplate` auf, um Werte den `Cell`-Eigenschaften zuzuordnen oder Datenbindungen für die `Cell`-Eigenschaften festzulegen, indem auf Eigenschaften der Elemente in der Auflistung `ItemsSource` verwiesen wird.

Dies wird im Beispiel [**TextCellListCode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListCode) veranschaulicht. Wenn `ListView` alle Elemente anzeigt, wird anhand der Vorlage eine kleine visuelle Struktur erstellt. Außerdem werden zwischen dem Element und den Eigenschaften der Elemente in dieser visuellen Struktur Datenbindungen erstellt.

Um sich mit diesem Vorgang vertraut zu machen, können Sie Handler für die [`ItemAppearing`](xref:Xamarin.Forms.ListView.ItemAppearing)- und [`ItemDisappearing`](xref:Xamarin.Forms.ListView.ItemDisappearing)-Ereignisse von `ListView` installieren. Eine weitere Möglichkeit besteht darin, einen alternativen [`DataTemplate`-Konstruktor](xref:Xamarin.Forms.DataTemplate.%23ctor(System.Func{System.Object})) zu einzusetzen, der eine Funktion verwendet, die jedes Mal aufgerufen wird, wenn die visuelle Struktur eines Elements erstellt werden muss. Im Beispiel [**TextCellListXaml**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListXaml) wird ein funktional identisches Programm in XAML gezeigt.

Ein `DataTemplate`-Tag wird auf die `ItemTemplate`-Eigenschaft von `ListView` festgelegt, anschließend wird `TextCell` auf `DataTemplate` festgelegt. Bindungen mit Eigenschaften der Elemente in der Auflistung werden direkt für die [`Text`](xref:Xamarin.Forms.TextCell.Text)- und [`Detail`](xref:Xamarin.Forms.TextCell.Detail)-Eigenschaften von `TextCell` festgelegt. Benutzerdefinierte Zellen

### <a name="custom-cells"></a>In XAML ist es möglich, [`ViewCell`](xref:Xamarin.Forms.ViewCell) auf `DataTemplate` festzulegen und dann eine benutzerdefinierte visuelle Struktur als [`View`](xref:Xamarin.Forms.ViewCell.View)-Eigenschaft von `ViewCell` zu definieren.

(Da `View` die Content-Eigenschaft von `ViewCell` ist, sind die `ViewCell.View`-Tags nicht erforderlich.) Diese Vorgehensweise wird im Beispiel [**CustomNamedColorList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CustomNamedColorList) gezeigt: [![Screenshot von CustomNamedColorList](images/ch19fg11-small.png "CustomNamedColorList")](images/ch19fg11-large.png#lightbox "CustomNamedColorList")

Das Ermitteln der richtigen Größe für alle Plattformen kann schwierig sein.

Die [`RowHeight`](xref:Xamarin.Forms.ListView.RowHeight)-Eigenschaft ist nützlich, in einigen Fällen sollten Sie jedoch auf die [`HasUnevenRows`](xref:Xamarin.Forms.ListView.HasUnevenRows)-Eigenschaft zurückgreifen. Diese Eigenschaft ist weniger effizient, erzwingt jedoch das Anpassen der Zeilengröße durch `ListView`. Bei iOS und Android müssen Sie eine dieser beiden Eigenschaften verwenden, um die richtige Zeilengröße zu erhalten. Gruppieren der ListView-Elemente

### <a name="grouping-the-listview-items"></a>`ListView` unterstützt das Gruppieren von Elementen sowie das Navigieren zwischen diesen Gruppen.

Die `ItemsSource`-Eigenschaft muss auf eine Auflistung aus Auflistungen festgelegt werden: Das Objekt, auf das `ItemsSource` festgelegt ist, muss `IEnumerable`implementieren. Außerdem muss jedes Element in der Auflistung auch `IEnumerable`implementieren. Jede Gruppe sollte zwei Eigenschaften enthalten: eine Textbeschreibung der Gruppe und eine aus drei Buchstaben bestehende Abkürzung. Mit der Klasse [`NamedColorGroup`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColorGroup.cs) in der [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)-Bibliothek werden sieben Gruppen aus `NamedColor`-Objekten erstellt.

Das Beispiel [**ColorGroupList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorGroupList) zeigt, wie diese Gruppen verwendet werden. Dabei ist die [`IsGroupingEnabled`](xref:Xamarin.Forms.ListView.IsGroupingEnabled)-Eigenschaft von `ListView` auf `true` festgelegt, und die Eigenschaften [`GroupDisplayBinding`](xref:Xamarin.Forms.ListView.GroupDisplayBinding) und [`GroupShortNameBinding`](xref:Xamarin.Forms.ListView.GroupShortNameBinding) sind an Eigenschaften in den einzelnen Gruppen gebunden. Benutzerdefinierte Gruppenheader

### <a name="custom-group-headers"></a>Sie können benutzerdefinierte Header für die `ListView`-Gruppen erstellen, indem Sie die `GroupDisplayBinding`-Eigenschaft durch [`GroupHeaderTemplate`](xref:Xamarin.Forms.ListView.GroupHeaderTemplate) ersetzen. Auf diese Weise wird eine Vorlage für die Header definiert.

ListView und Interaktivität

### <a name="listview-and-interactivity"></a>Für die Benutzerinteraktion mit `ListView` wird üblicherweise ein Handler an das `ItemSelected`- oder [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped)-Ereignis angefügt oder eine Datenbindung für die `SelectedItem`-Eigenschaft festlegt.

Es gibt jedoch auch Zellentypen (`EntryCell` und `SwitchCell`), die eine Benutzerinteraktion ermöglichen. Darüber hinaus ist es möglich, benutzerdefinierte Zellen zu erstellen, die selbst mit dem Benutzer interagieren. Mit [**InteractiveListView**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/InteractiveListView) werden 100 Instanzen von [`ColorViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs) erstellt. Außerdem kann der Benutzer mithilfe von drei `Slider`-Elementen die einzelnen Farben ändern. Das Programm nutzt zudem den [`ColorToContrastColorConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorToContrastColorConverter.cs) im [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit). ListView und MVVM

## <a name="listview-and-mvvm"></a>`ListView` spielt in MVVM-Szenarien eine große Rolle.

Wenn in einem ViewModel eine `IEnumerable`-Auflistung vorhanden ist, ist sie oft an ein `ListView` gebunden. Außerdem implementieren die Elemente in der Auflistung häufig `INotifyPropertyChanged`, um eine Bindung mit Eigenschaften in einer Vorlage herzustellen. Eine ViewModels-Auflistung

### <a name="a-collection-of-viewmodels"></a>Um diese Funktion zu veranschaulichen, erstellt die [**SchoolOfFineArts**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt)-Bibliothek mehrere Klassen basierend auf einer [XML-Datendatei](https://xamarin.github.io/xamarin-forms-book-samples/SchoolOfFineArt/students.xml) sowie Bilder von fiktiven Studenten an dieser fiktiven Schule.

Die Klasse [`Student`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/Student.cs) wird von [`ViewModelBase`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/ViewModelBase.cs) abgeleitet.

Die Klasse [`StudentBody`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/StudentBody.cs) ist eine Auflistung aus `Student`-Objekten und wird ebenfalls von `ViewModelBase` abgeleitet. [`SchoolViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/SchoolViewModel.cs) lädt die XML-Datei herunter und setzt alle Objekte zusammen. Das [**StudentList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/StudentList)-Programm verwendet eine `ImageCell`, um die Studenten und ihre Bilder in einer `ListView` anzuzeigen:

[![Screenshot der Studentenliste](images/ch19fg18-small.png "StudentList")](images/ch19fg18-large.png#lightbox "StudentList")

Im Beispiel [**ListViewHeader**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewHeader) wird eine [`Header`](xref:Xamarin.Forms.ListView.Header)-Eigenschaft hinzugefügt, die jedoch nur unter Android angezeigt wird.

Auswahl und der Bindungskontext

### <a name="selection-and-the-binding-context"></a>Das Programm [**SelectedStudentDetail**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/SelectedStudentDetail) bindet den `BindingContext` eines `StackLayout` an die `SelectedItem`-Eigenschaft von `ListView`.

Dadurch kann das Programm detaillierte Informationen zum ausgewählten Studenten anzeigen. Kontextmenüs

### <a name="context-menus"></a>In einer Zelle kann ein Kontextmenü definiert werden, das plattformspezifisch implementiert wird.

Um dieses Menü zu erstellen, fügen Sie [`MenuItem`](xref:Xamarin.Forms.MenuItem)-Objekte zur [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions)-Eigenschaft der `Cell` hinzu. `MenuItem` definiert fünf Eigenschaften:

[`Text`](xref:Xamarin.Forms.MenuItem.Text) vom Typ `string`

- [`Icon`](xref:Xamarin.Forms.MenuItem.Icon) vom Typ `FileImageSource`
- [`IsDestructive`](xref:Xamarin.Forms.MenuItem.IsDestructive) vom Typ `bool`
- [`Command`](xref:Xamarin.Forms.MenuItem.Command) vom Typ `ICommand`
- [`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter) vom Typ `object`
- Die Eigenschaften `Command` und `CommandParameter` implizieren, dass das ViewModel-Objekt für jedes Element Methoden zum Ausführen der gewünschten Menübefehle enthält.

In Nicht-MVVM-Szenarien definiert `MenuItem` zudem ein [`Clicked`](xref:Xamarin.Forms.MenuItem.Clicked)-Ereignis. Dieses Verfahren wird in [**CellContextMenu**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CellContextMenu) veranschaulicht.

Die `Command`-Eigenschaft jedes `MenuItem` ist an eine Eigenschaft vom Typ `ICommand` in der `Student`-Klasse gebunden. Legen Sie die `IsDestructive`-Eigenschaft für ein `MenuItem` auf `true` fest, wenn das Menüelement das ausgewählte Objekt entfernt oder löscht. Variierende visuelle Elemente

### <a name="varying-the-visuals"></a>In einigen Fällen sollen die Elemente der `ListView` basierend auf einer Eigenschaft mit geringfügigen Variationen angezeigt werden.

Im Beispiel [**ColorCodedStudents**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorCodedStudents) wird der Name eines Studenten z.B. rot angezeigt, wenn sein Notendurchschnitt unter 2.0 fällt. Zu diesem Zweck wird der Bindungsdatenkonverter [`ThresholdToObjectConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ThresholdToObjectConverter.cs) aus der [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)-Bibliothek verwendet.
Aktualisieren des Inhalts

### <a name="refreshing-the-content"></a>`ListView` unterstützt das Herunterziehen der Inhalte, um die Daten zu aktualisieren.

Um diese Funktion zu aktivieren, muss das Programm die [`IsPullToRefresh`](xref:Xamarin.Forms.ListView.IsPullToRefreshEnabled)-Eigenschaft auf `true` festlegen. `ListView` reagiert auf das Herunterziehen, indem ihre [`IsRefreshing`](xref:Xamarin.Forms.ListView.IsRefreshing)-Eigenschaft auf `true` festgelegt wird. Außerdem wird das [`Refreshing`](xref:Xamarin.Forms.ListView.Refreshing)-Ereignis ausgelöst und (für MVVM-Szenarien) die `Execute`-Methode ihrer [`RefreshCommand`](xref:Xamarin.Forms.ListView.RefreshCommand)-Eigenschaft aufgerufen. Code, der das `Refresh`-Ereignis oder den `RefreshCommand` verarbeitet, aktualisiert dann möglicherweise die Daten, die von `ListView` angezeigt werden, und legt `IsRefreshing` wieder auf `false` fest.

Das Beispiel [**RssFeed**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/RssFeed) veranschaulicht die Verwendung eines [`RssFeedViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/RssFeed/RssFeed/RssFeed/RssFeedViewModel.cs), das die Eigenschaften `RefreshCommand` und `IsRefreshing` für Datenbindungen implementiert.

TableView und die zugehörigen Intents

## <a name="the-tableview-and-its-intents"></a>Während `ListView` in der Regel mehrere Instanzen desselben Typs anzeigt, stellt [`TableView`](xref:Xamarin.Forms.TableView) im Allgemeinen eine Benutzeroberfläche für mehrere Eigenschaften unterschiedlicher Typen bereit.

Jedes Element ist einer eigenen [`Cell`](xref:Xamarin.Forms.Cell)-Ableitung zum Anzeigen der Eigenschaft oder Bereitstellen einer Benutzeroberfläche zugeordnet. Eigenschaften und Hierarchien

### <a name="properties-and-hierarchies"></a>`TableView` definiert nur vier Eigenschaften:

[`Intent`](xref:Xamarin.Forms.TableView.Intent) vom Typ [`TableIntent`](xref:Xamarin.Forms.TableIntent), eine Enumeration

- [`Root`](xref:Xamarin.Forms.TableView.Root) vom Typ [`TableRoot`](xref:Xamarin.Forms.TableRoot), die Content-Eigenschaft von `TableView`
- [`RowHeight`](xref:Xamarin.Forms.TableView.RowHeight) vom Typ `int`
- [`HasUnevenRows`](xref:Xamarin.Forms.TableView.HasUnevenRows) vom Typ `bool`
- Die `TableIntent`-Enumeration gibt an, wie Sie `TableView`verwenden möchten:

Diese Member empfehlen außerdem einige Verwendungsmöglichkeiten für `TableView`.

- [`Data`](xref:Xamarin.Forms.TableIntent.Data)
- [`Form`](xref:Xamarin.Forms.TableIntent.Form)
- [`Settings`](xref:Xamarin.Forms.TableIntent.Settings)
- [`Menu`](xref:Xamarin.Forms.TableIntent.Menu)

Für die Definition einer Tabelle werden eine Reihe weiterer Klassen verwendet:

[`TableSectionBase`](xref:Xamarin.Forms.TableSectionBase) ist eine abstrakte Klasse, die von `BindableObject` abgeleitet wird und eine [`Title`](xref:Xamarin.Forms.TableSectionBase.Title)-Eigenschaft definiert

- [`TableSectionBase<T>`](xref:Xamarin.Forms.TableSectionBase`1) ist eine abstrakte Klasse, die von `TableSectionBase` abgeleitet wird und `IList<T>` und `INotifyCollectionChanged` implementiert

- [`TableSection`](xref:Xamarin.Forms.TableSection) wird von `TableSectionBase<Cell>` abgeleitet

- [`TableRoot`](xref:Xamarin.Forms.TableRoot) wird von `TableSectionBase<TableSection>` abgeleitet

- Kurz gesagt verfügt `TableView` über eine `Root`-Eigenschaft, die Sie auf ein `TableRoot`-Objekt festlegen, bei dem es sich um eine Auflistung von `TableSection`-Objekten handelt. Jedes dieser Objekte ist wiederum ebenfalls eine Auflistung von `Cell`-Objekten.

Eine Tabelle verfügt über mehrere Abschnitte, und jeder Abschnitt enthält mehrere Zellen. Die Tabelle selbst und jeder Abschnitt kann über einen Titel verfügen. Wenngleich `TableView` mit `Cell`-Ableitungen arbeitet, wird `DataTemplate` nicht verwendet. Ein einfaches Formular

### <a name="a-prosaic-form"></a>Das [**EntryForm**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/EntryForm)-Beispiel definiert ein [`PersonalInformation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs)-Ansichtsmodell. Eine Instanz dieses Modells wird zum `BindingContext` von `TableView`.

Jede `Cell`-Ableitung in der zugehörigen `TableSection` kann anschließend über Bindungen mit Eigenschaften der `PersonalInformation`-Klasse verfügen. Benutzerdefinierte Zellen

### <a name="custom-cells"></a>Im Beispiel [**ConditionalCells**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalCells) wird **EntryForm** erweitert.

Die [`ProgrammerInformation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs)-Klasse enthält eine boolesche Eigenschaft, die die Anwendbarkeit von zwei zusätzlichen Eigenschaften regelt. Für diese beiden zusätzlichen Eigenschaften verwendet das Programm eine benutzerdefinierte `PickerCell`, die auf einer [PickerCell.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml)- und [PickerCell.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml.cs)-Klasse in der [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)-Bibliothek basiert. Obwohl die `IsEnabled`-Eigenschaften der beiden `PickerCell`-Elemente an die boolesche Eigenschaft in `ProgrammerInformation`gebunden sind, scheint dieses Verfahren nicht zu funktionieren. Aus diesem Grund wird das nächste Beispiel veranschaulicht.

Bedingungsabschnitte

### <a name="conditional-sections"></a>Im Beispiel [**ConditionalSection**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalSection) werden die beiden bedingten Elemente der Auswahl des booleschen Elements in einer separaten `TableSection` platziert.

Die CodeBehind-Datei entfernt diesen Abschnitt aus der `TableView` oder fügt ihn basierend auf der booleschen Eigenschaft erneut hinzu. Ein TableView-Menü

### <a name="a-tableview-menu"></a>Eine weitere Verwendungsmöglichkeit von `TableView` ist ein Menü.

Im Beispiel [**MenuCommands**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/MenuCommands) wird ein Menü gezeigt, in dem Sie eine kleine `BoxView` auf dem Bildschirm verschieben können. Verwandte Links

## <a name="related-links"></a>[Kapitel 19 – vollständiger Text (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch19-Apr2016.pdf)

- [Kapitel 19 – Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19)
- [Auswahl](~/xamarin-forms/user-interface/picker/index.md)
- [ListView](~/xamarin-forms/user-interface/listview/index.md)
- [TableView](~/xamarin-forms/user-interface/tableview.md)
- CustomNamedColorList
