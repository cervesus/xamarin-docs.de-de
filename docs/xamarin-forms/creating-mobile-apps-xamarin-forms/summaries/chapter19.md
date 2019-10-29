---
title: Zusammenfassung des Kapitels 19. Sammlungs Ansichten
description: 'Erstellen von Mobile Apps mit xamarin. Forms: Zusammenfassung von Kapitel 19. Sammlungs Ansichten'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 0AEC3A5C-586E-4D0F-9895-67E99A053A79
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
ms.openlocfilehash: bffbd2dec4a8494723597ba6e0f0af69e57f3718
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032864"
---
# <a name="summary-of-chapter-19-collection-views"></a>Zusammenfassung des Kapitels 19. Sammlungs Ansichten

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19)

> [!NOTE] 
> Hinweise auf dieser Seite geben Bereiche an, in denen xamarin. Forms von dem im Buch dargestellten Material abweickte.

Xamarin. Forms definiert drei Sichten, die Auflistungen verwalten und ihre Elemente anzeigen:

- [`Picker`](xref:Xamarin.Forms.Picker) ist eine relativ kurze Liste von Zeichen folgen Elementen, die es dem Benutzer ermöglichen, einen auszuwählen.
- [`ListView`](xref:Xamarin.Forms.ListView) ist häufig eine lange Liste von Elementen, die in der Regel denselben Typ und dieselbe Formatierung aufweisen, sodass der Benutzer auch eine Auswahl treffen kann.
- [`TableView`](xref:Xamarin.Forms.TableView) ist eine Auflistung von *Zellen* (in der Regel verschiedene Typen und Erscheinungen) zum Anzeigen von Daten oder zum Verwalten von Benutzereingaben.

Es kommt häufig vor, dass MVVM-Anwendungen den `ListView` verwenden, um eine auswählbare Auflistung von Objekten anzuzeigen.

## <a name="program-options-with-picker"></a>Programmoptionen mit Auswahl

Das [`Picker`](xref:Xamarin.Forms.Picker) ist eine gute Wahl, wenn Sie es dem Benutzer ermöglichen möchten, eine Option aus einer relativ kurzen Liste von `string` Elementen auszuwählen.

### <a name="the-picker-and-event-handling"></a>Auswahl-und Ereignis Behandlung

Das [**pickerdemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerDemo) -Beispiel veranschaulicht, wie Sie mit XAML die `Picker` [`Title`](xref:Xamarin.Forms.Picker.Title) -Eigenschaft festlegen und der [`Items`](xref:Xamarin.Forms.Picker.Items) Auflistung `string` Elemente hinzufügen. Wenn der Benutzer den `Picker`auswählt, werden die Elemente in der `Items` Auflistung plattformabhängig angezeigt.

Das Ereignis [`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged) gibt an, wann der Benutzer ein Element ausgewählt hat. Die null basierte [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) Eigenschaft gibt dann das ausgewählte Element an. Wenn kein Element ausgewählt ist, entspricht `SelectedIndex` &ndash;1.

Sie können auch `SelectedIndex` verwenden, um das ausgewählte Element zu initialisieren, es muss jedoch nach dem Ausfüllen der `Items` Auflistung festgelegt werden. In XAML bedeutet dies, dass Sie wahrscheinlich ein Property-Element verwenden, um `SelectedIndex`festzulegen.

### <a name="data-binding-the-picker"></a>Datenbindung der Auswahl

Die `SelectedIndex`-Eigenschaft wird durch eine bindbare Eigenschaft gestützt, aber `Items` ist nicht. Daher ist die Verwendung der Datenbindung mit einem `Picker` schwierig. Eine Lösung besteht darin, die `Picker` in Kombination mit einer [`ObjectToIndexConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ObjectToIndexConverter.cs) zu verwenden, z. b. in der [**xamarin. formsbook. Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) -Bibliothek. Die [**pickerbinding**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerBinding) veranschaulicht, wie dies funktioniert.

> [!NOTE] 
> Die xamarin. Forms-`Picker` enthält nun `ItemsSource` und `SelectedItem` Eigenschaften, die die Datenbindung unterstützen. Siehe [Auswahl](~/xamarin-forms/user-interface/picker/index.md).

## <a name="rendering-data-with-listview"></a>Rendern von Daten mit ListView

Der- [`ListView`](xref:Xamarin.Forms.ListView) ist die einzige Klasse, die von [`ItemsView<TVisual>`](xref:Xamarin.Forms.ItemsView`1) abgeleitet wird, von dem Sie die Eigenschaften [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) und [`ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) erbt.

`ItemsSource` ist vom Typ `IEnumerable` ist jedoch standardmäßig `null` und muss durch eine Datenbindung explizit initialisiert oder (häufiger) auf eine Auflistung festgelegt werden. Die Elemente in dieser Sammlung können von einem beliebigen Typ sein.

`ListView` eine [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem) Eigenschaft definiert, die entweder auf eines der Elemente in der `ItemsSource` Auflistung festgelegt ist, oder `null`, wenn kein Element ausgewählt ist. `ListView` löst das [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) -Ereignis aus, wenn ein neues Element ausgewählt wird.

### <a name="collections-and-selections"></a>Sammlungen und Auswahlmöglichkeiten

Das [**listviewlist**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewList) -Beispiel füllt eine `ListView` mit 17 `Color` Werten in einer `List<Color>` Auflistung. Die Elemente sind wählbar, werden jedoch standardmäßig mit ihren unattraktiven `ToString` Darstellungen angezeigt. In den folgenden Beispielen wird gezeigt, wie Sie die Anzeige beheben und Sie so attraktiv machen, wie Sie möchten.

### <a name="the-row-separator"></a>Das Zeilen Trennzeichen

Bei IOS-und Android-anzeigen werden die Zeilen von einer dünnen Linie getrennt. Sie können dies mit den Eigenschaften [`SeparatorVisibility`](xref:Xamarin.Forms.ListView.SeparatorVisibility) und [`SeparatorColor`](xref:Xamarin.Forms.ListView.SeparatorColor) steuern. `SeparatorVisibility`-Eigenschaft ist vom Typ [`SeparatorVisibility`](xref:Xamarin.Forms.SeparatorVisibility), eine Enumeration mit zwei Membern:

- [`Default`](xref:Xamarin.Forms.SeparatorVisibility.Default), die Standardeinstellung
- [`None`](xref:Xamarin.Forms.SeparatorVisibility.None)

### <a name="data-binding-the-selected-item"></a>Datenbindung für das ausgewählte Element

Die `SelectedItem`-Eigenschaft wird durch eine bindbare Eigenschaft gestützt, d. h., Sie kann entweder die Quelle oder das Ziel einer Datenbindung sein. Der Standard `BindingMode` ist `OneWayToSource`, aber im Allgemeinen ist es das Ziel einer bidirektionalen Datenbindung, insbesondere in MVVM-Szenarios. Das [**ListViewArray**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewArray) -Beispiel veranschaulicht diesen Bindungstyp.

### <a name="the-observablecollection-difference"></a>Der Unterschied von ObservableCollection

Mit dem [**listviewlogger**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewLogger) -Beispiel wird die `ItemsSource`-Eigenschaft eines `ListView` auf eine `List<DateTime>` Auflistung festgelegt, und anschließend wird mithilfe eines Zeit Gebers jede Sekunde ein neues `DateTime` Objekt der Auflistung hinzugefügt.

Die `ListView` wird jedoch nicht automatisch aktualisiert, da die `List<T>` Auflistung keinen Benachrichtigungs Mechanismus hat, der angibt, wann Elemente der Auflistung hinzugefügt oder daraus entfernt werden.

Eine viel bessere Klasse, die in solchen Szenarien verwendet werden kann, ist [`ObservableCollection<T>`](xref:System.Collections.ObjectModel.ObservableCollection`1) im `System.Collections.ObjectModel`-Namespace definiert. Diese Klasse implementiert die [`INotifyCollectionChanged`](xref:System.Collections.Specialized.INotifyCollectionChanged) -Schnittstelle und löst folglich ein [`CollectionChanged`](xref:System.Collections.ObjectModel.ObservableCollection`1.CollectionChanged) -Ereignis aus, wenn Elemente der Auflistung hinzugefügt oder daraus entfernt werden oder wenn Sie in der Auflistung ersetzt oder verschoben werden. Wenn der `ListView` intern erkennt, dass eine Klasse, die `INotifyCollectionChanged` implementiert, auf seine `ItemsSource`-Eigenschaft festgelegt wurde, fügt er dem `CollectionChanged` Ereignis einen Handler an und aktualisiert seine Anzeige, wenn sich die Auflistung ändert.

Das [**observablelogger**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ObservableLogger) -Beispiel veranschaulicht die Verwendung von `ObservableCollection`.

### <a name="templates-and-cells"></a>Vorlagen und Zellen

Standardmäßig zeigt ein `ListView` Elemente in der Auflistung mithilfe der `ToString` Methode jedes Elements an. Ein besserer Ansatz besteht darin, eine Vorlage zum Anzeigen der Elemente zu definieren.

Um mit diesem Feature zu experimentieren, können Sie die [`NamedColor`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs) -Klasse in der [**xamarin. formsbook. Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) -Bibliothek verwenden. Diese Klasse definiert eine statische `All`-Eigenschaft vom Typ `IList<NamedColor>`, die 141 `NamedColor`-Objekte enthält, die den öffentlichen Feldern der `Color` Struktur entsprechen.

Das Beispiel " [**naivenamedcolorlist**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/NaiveNamedColorList) " legt die `ItemsSource` eines `ListView` auf diese `NamedColor.All` Eigenschaft fest, aber nur die voll qualifizierten Klassennamen der `NamedColor` Objekte werden angezeigt.

`ListView` benötigt eine Vorlage, um diese Elemente anzuzeigen. Im Code können Sie die [`ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) -Eigenschaft, die durch `ItemsView<TVisual>` definiert wird, mithilfe des [`DataTemplate`-Konstruktors](xref:Xamarin.Forms.DataTemplate.%23ctor(System.Type)) , der auf eine Ableitung der [`Cell`](xref:Xamarin.Forms.Cell) -Klasse verweist, auf ein [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) -Objekt festlegen. `Cell` hat fünf Ableitungen:

- [`TextCell`](xref:Xamarin.Forms.TextCell) &mdash; enthält zwei `Label` Ansichten (konzeptionell).
- [`ImageCell`](xref:Xamarin.Forms.ImageCell) &mdash; fügt eine `Image` Sicht hinzu `TextCell`
- [`EntryCell`](xref:Xamarin.Forms.EntryCell) &mdash; enthält eine `Entry` Sicht mit einer `Label`
- [`SwitchCell`](xref:Xamarin.Forms.SwitchCell) &mdash; enthält eine `Switch` mit einer `Label`
- [`ViewCell`](xref:Xamarin.Forms.ViewCell) &mdash; kann eine beliebige `View` sein (wahrscheinlich mit untergeordneten Elementen).

Anschließend [`SetValue`](xref:Xamarin.Forms.DataTemplate.SetValue(Xamarin.Forms.BindableProperty,System.Object)) und [`SetBinding`](xref:Xamarin.Forms.DataTemplate.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) für das `DataTemplate` Objekt, um den `Cell` Eigenschaften Werte zuzuordnen, oder um Daten Bindungen in den `Cell` Eigenschaften festzulegen, die auf Eigenschaften der Elemente in der `ItemsSource` Auflistung verweisen. Dies wird im [**textcelllistcode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListCode) -Beispiel veranschaulicht.

Wenn jedes Element vom `ListView`angezeigt wird, wird eine kleine visuelle Struktur aus der Vorlage erstellt, und Daten Bindungen werden zwischen dem Element und den Eigenschaften der Elemente in dieser visuellen Struktur erstellt. Sie können sich einen Überblick über diesen Prozess verschaffen, indem Sie Handler für die [`ItemAppearing`](xref:Xamarin.Forms.ListView.ItemAppearing) und [`ItemDisappearing`](xref:Xamarin.Forms.ListView.ItemDisappearing) Ereignisse der `ListView`und einen alternativen [`DataTemplate` Konstruktor](xref:Xamarin.Forms.DataTemplate.%23ctor(System.Func{System.Object})) verwenden, der eine Funktion verwendet, die jedes Mal aufgerufen wird, wenn die visuelle Struktur eines Elements erstellt werden.

[**Textcelllistxaml**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListXaml) zeigt ein funktional identisches Programm vollständig in XAML. Ein `DataTemplate`-Tag wird auf die `ItemTemplate`-Eigenschaft der `ListView`festgelegt, und dann wird der `TextCell` auf den `DataTemplate`festgelegt. Bindungen an Eigenschaften der Elemente in der Auflistung werden direkt in den Eigenschaften [`Text`](xref:Xamarin.Forms.TextCell.Text) und [`Detail`](xref:Xamarin.Forms.TextCell.Detail) der `TextCell`festgelegt.

### <a name="custom-cells"></a>Benutzerdefinierte Zellen

In XAML ist es möglich, eine [`ViewCell`](xref:Xamarin.Forms.ViewCell) auf den `DataTemplate` festzulegen und dann eine benutzerdefinierte visuelle Struktur als [`View`](xref:Xamarin.Forms.ViewCell.View) -Eigenschaft der `ViewCell`zu definieren. (`View` ist die Content-Eigenschaft `ViewCell`, sodass die `ViewCell.View` Tags nicht erforderlich sind.) Das [**customnamedcolorlist**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CustomNamedColorList) -Beispiel veranschaulicht dieses Verfahren:

[![Dreifacher Screenshot der Liste benutzerdefinierter benannter Farben](images/ch19fg11-small.png "Benutzerdefinierte benannte Farbliste")](images/ch19fg11-large.png#lightbox "Benutzerdefinierte benannte Farbliste")

Das erzielen der Größenanpassung für alle Plattformen kann knifflig sein. Die [`RowHeight`](xref:Xamarin.Forms.ListView.RowHeight) -Eigenschaft ist nützlich, aber in einigen Fällen sollten Sie auf die [`HasUnevenRows`](xref:Xamarin.Forms.ListView.HasUnevenRows) -Eigenschaft zurückgreifen, die weniger effizient ist, aber die `ListView` die Größe der Zeilen erzwingt. Für IOS und Android müssen Sie eine dieser beiden Eigenschaften verwenden, um die richtige Zeilengröße zu erhalten.

### <a name="grouping-the-listview-items"></a>Gruppieren der ListView-Elemente

`ListView` unterstützt das Gruppieren von Elementen und Navigieren zwischen diesen Gruppen. Die `ItemsSource`-Eigenschaft muss auf eine Auflistung von Auflistungen festgelegt werden: das-Objekt, auf das `ItemsSource` festgelegt ist, muss `IEnumerable`implementieren, und jedes Element in der Auflistung muss ebenfalls `IEnumerable`implementieren. Jede Gruppe sollte zwei Eigenschaften enthalten: eine Textbeschreibung der Gruppe und eine aus drei Buchstaben bestehende Abkürzung.

Die [`NamedColorGroup`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColorGroup.cs) -Klasse in der [**xamarin. formsbook. Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) -Bibliothek erstellt sieben Gruppen von `NamedColor` Objekten. Das [**colorgrouplist**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorGroupList) -Beispiel zeigt, wie diese Gruppen verwendet werden, wenn die [`IsGroupingEnabled`](xref:Xamarin.Forms.ListView.IsGroupingEnabled) -Eigenschaft von `ListView` auf `true`festgelegt ist und die Eigenschaften [`GroupDisplayBinding`](xref:Xamarin.Forms.ListView.GroupDisplayBinding) und [`GroupShortNameBinding`](xref:Xamarin.Forms.ListView.GroupShortNameBinding) an die Eigenschaften in jeder Gruppe gebunden sind.

### <a name="custom-group-headers"></a>Benutzerdefinierte Gruppen Kopfzeilen

Es ist möglich, benutzerdefinierte Header für die `ListView` Gruppen zu erstellen, indem Sie die `GroupDisplayBinding`-Eigenschaft durch die [`GroupHeaderTemplate`](xref:Xamarin.Forms.ListView.GroupHeaderTemplate) ersetzen, die eine Vorlage für die Header definiert.

### <a name="listview-and-interactivity"></a>ListView und Interaktivität

Im allgemeinen erhält eine Anwendung eine Benutzerinteraktion mit einer `ListView`, indem Sie einen Handler an das `ItemSelected` oder [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) Ereignis anfügt oder indem Sie eine Datenbindung für die `SelectedItem`-Eigenschaft festlegt. Einige Zelltypen (`EntryCell` und `SwitchCell`) ermöglichen eine Benutzerinteraktion, und es ist auch möglich, benutzerdefinierte Zellen zu erstellen, die selbst mit dem Benutzer interagieren. Die [**interactivelistview**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/InteractiveListView) erstellt 100 Instanzen von [`ColorViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs) und ermöglicht es dem Benutzer, jede Farbe mithilfe eines Trios von `Slider` Elementen zu ändern. Das Programm nutzt auch die [`ColorToContrastColorConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorToContrastColorConverter.cs) im [**xamarin. formsbook. Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit).

## <a name="listview-and-mvvm"></a>ListView und MVVM

`ListView` spielt in MVVM-Szenarien eine große Rolle. Wenn eine `IEnumerable` Auflistung in einem ViewModel vorhanden ist, ist Sie häufig an eine `ListView`gebunden. Außerdem implementieren die Elemente in der Auflistung häufig `INotifyPropertyChanged`, um eine Bindung mit Eigenschaften in einer Vorlage herzustellen.

### <a name="a-collection-of-viewmodels"></a>Eine Auflistung von ViewModels.

Um dies zu untersuchen, erstellt die [**schooloffinearts**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) -Bibliothek mehrere Klassen basierend auf einer [XML-Datendatei](https://xamarin.github.io/xamarin-forms-book-samples/SchoolOfFineArt/students.xml) und Images von fiktiven Schülern in dieser fiktiven Schule.

Die [`Student`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/Student.cs) -Klasse wird von [`ViewModelBase`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/ViewModelBase.cs)abgeleitet. Die [`StudentBody`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/StudentBody.cs) -Klasse ist eine Auflistung von `Student` Objekten und wird auch von `ViewModelBase`abgeleitet. Der [`SchoolViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/SchoolViewModel.cs) lädt die XML-Datei herunter und assembliert alle-Objekte.

Das [**studentList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/StudentList) -Programm verwendet eine `ImageCell`, um die Studenten und Ihre Images in einem `ListView`anzuzeigen:

[![Dreifacher Screenshot der Liste "Student"](images/ch19fg18-small.png "Liste der Studenten")](images/ch19fg18-large.png#lightbox "Liste der Studenten")

Das [**ListviewHeader**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewHeader) -Beispiel fügt eine [`Header`](xref:Xamarin.Forms.ListView.Header) -Eigenschaft hinzu, wird jedoch nur unter Android angezeigt.

### <a name="selection-and-the-binding-context"></a>Auswahl und Bindungs Kontext

Das [**selectedstudentdetail**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/SelectedStudentDetail) -Programm bindet die `BindingContext` eines `StackLayout` an die `SelectedItem`-Eigenschaft der `ListView`. Dies ermöglicht es dem Programm, ausführliche Informationen über den ausgewählten Studenten anzuzeigen.

### <a name="context-menus"></a>Kontextmenüs

In einer Zelle kann ein Kontextmenü definiert werden, das plattformspezifisch implementiert wird. Um dieses Menü zu erstellen, fügen Sie der [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) -Eigenschaft der `Cell`[`MenuItem`](xref:Xamarin.Forms.MenuItem) Objekte hinzu.

in `MenuItem` werden fünf Eigenschaften definiert:

- [`Text`](xref:Xamarin.Forms.MenuItem.Text) vom Typ `string`
- [`Icon`](xref:Xamarin.Forms.MenuItem.Icon) vom Typ `FileImageSource`
- [`IsDestructive`](xref:Xamarin.Forms.MenuItem.IsDestructive) vom Typ `bool`
- [`Command`](xref:Xamarin.Forms.MenuItem.Command) vom Typ `ICommand`
- [`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter) vom Typ `object`

Die Eigenschaften `Command` und `CommandParameter` implizieren, dass das ViewModel-Objekt für jedes Element Methoden zum Ausführen der gewünschten Menübefehle enthält. In Szenarios ohne MVVM definiert `MenuItem` auch ein [`Clicked`](xref:Xamarin.Forms.MenuItem.Clicked) Ereignis.

Das [**cellcontextmenu**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CellContextMenu) veranschaulicht dieses Verfahren. Die `Command`-Eigenschaft der einzelnen `MenuItem` ist an eine Eigenschaft vom Typ `ICommand` in der `Student`-Klasse gebunden. Legen Sie die `IsDestructive`-Eigenschaft auf `true` für einen `MenuItem` fest, der das ausgewählte Objekt entfernt oder löscht.

### <a name="varying-the-visuals"></a>Variieren der visuellen Elemente

Manchmal benötigen Sie geringfügige Variationen in den visuellen Elementen der Elemente in der `ListView` basierend auf einer Eigenschaft. Wenn beispielsweise der Wert für den Mittelpunkt eines Studenten unter 2,0 fällt, zeigt das [**colorcodedstudents**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorCodedStudents) -Beispiel den Namen des Studenten rot an.
Dies wird durch die Verwendung eines Bindungs Wert Konverters ( [`ThresholdToObjectConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ThresholdToObjectConverter.cs)) in der [**xamarin. formsbook. Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) -Bibliothek erreicht.

### <a name="refreshing-the-content"></a>Aktualisieren des Inhalts

Der `ListView` unterstützt eine pulldownbewegung zum Aktualisieren seiner Daten. Das Programm muss die [`IsPullToRefresh`](xref:Xamarin.Forms.ListView.IsPullToRefreshEnabled) -Eigenschaft auf `true` festlegen, um dies zu aktivieren. Der `ListView` antwortet auf die pulldownbewegung, indem seine [`IsRefreshing`](xref:Xamarin.Forms.ListView.IsRefreshing) -Eigenschaft auf `true`festgelegt wird, und indem das [`Refreshing`](xref:Xamarin.Forms.ListView.Refreshing) -Ereignis und (für MVVM-Szenarien) aufgerufen wird, um die `Execute`-Methode der [`RefreshCommand`](xref:Xamarin.Forms.ListView.RefreshCommand) -Eigenschaft aufzurufenden.

Code, der das `Refresh` Ereignis oder das `RefreshCommand` behandelt, aktualisiert möglicherweise die Daten, die vom `ListView` angezeigt werden, und legt `IsRefreshing` wieder auf `false`fest.

Das [**RSSFeed**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/RssFeed) -Beispiel veranschaulicht die Verwendung einer [`RssFeedViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/RssFeed/RssFeed/RssFeed/RssFeedViewModel.cs) , die `RefreshCommand` und `IsRefreshing` Eigenschaften für die Datenbindung implementiert.

## <a name="the-tableview-and-its-intents"></a>Die TableView und deren Intents

Während der `ListView` in der Regel mehrere Instanzen desselben Typs anzeigt, konzentriert sich der [`TableView`](xref:Xamarin.Forms.TableView) im Allgemeinen auf die Bereitstellung einer Benutzeroberfläche für mehrere Eigenschaften verschiedener Typen. Jedes Element ist mit einer eigenen [`Cell`](xref:Xamarin.Forms.Cell) Ableitung verknüpft, um die Eigenschaft anzuzeigen oder eine Benutzeroberfläche bereitzustellen.

### <a name="properties-and-hierarchies"></a>Eigenschaften und Hierarchien

`TableView` definiert nur vier Eigenschaften:

- [`Intent`](xref:Xamarin.Forms.TableView.Intent) vom Typ [`TableIntent`](xref:Xamarin.Forms.TableIntent), eine Enumeration
- [`Root`](xref:Xamarin.Forms.TableView.Root) vom Typ [`TableRoot`](xref:Xamarin.Forms.TableRoot), die Content-Eigenschaft von `TableView`
- [`RowHeight`](xref:Xamarin.Forms.TableView.RowHeight) vom Typ `int`
- [`HasUnevenRows`](xref:Xamarin.Forms.TableView.HasUnevenRows) vom Typ `bool`

Die `TableIntent`-Enumeration gibt an, wie Sie die `TableView`verwenden möchten:

- [`Data`](xref:Xamarin.Forms.TableIntent.Data)
- [`Form`](xref:Xamarin.Forms.TableIntent.Form)
- [`Settings`](xref:Xamarin.Forms.TableIntent.Settings)
- [`Menu`](xref:Xamarin.Forms.TableIntent.Menu)

Diese Member empfehlen außerdem einige Verwendungsmöglichkeiten für die `TableView`.

Mehrere andere Klassen sind an der Definition einer Tabelle beteiligt:

- [`TableSectionBase`](xref:Xamarin.Forms.TableSectionBase) ist eine abstrakte Klasse, die von `BindableObject` abgeleitet ist und eine [`Title`](xref:Xamarin.Forms.TableSectionBase.Title) Eigenschaft definiert.

- [`TableSectionBase<T>`](xref:Xamarin.Forms.TableSectionBase`1) ist eine abstrakte Klasse, die von `TableSectionBase` abgeleitet ist und `IList<T>` und implementiert `INotifyCollectionChanged`

- [`TableSection`](xref:Xamarin.Forms.TableSection) wird von `TableSectionBase<Cell>` abgeleitet

- [`TableRoot`](xref:Xamarin.Forms.TableRoot) wird von `TableSectionBase<TableSection>` abgeleitet

Kurz gesagt verfügt `TableView` über eine `Root`-Eigenschaft, die Sie auf ein `TableRoot` Objekt festlegen, das eine Auflistung von `TableSection` Objekten ist, von denen jede eine Auflistung von `Cell` Objekten ist. Eine Tabelle verfügt über mehrere Abschnitte, und jeder Abschnitt enthält mehrere Zellen. Die Tabelle selbst kann über einen Titel verfügen, und jeder Abschnitt kann einen Titel haben. Obwohl `TableView` `Cell` Ableitungen verwendet, wird `DataTemplate`nicht verwendet.

### <a name="a-prosaic-form"></a>Ein Prosaisches Formular

Das [**entryform**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/EntryForm) -Beispiel definiert ein [`PersonalInformation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs) Ansichts Modell, eine Instanz von, die zum `BindingContext` des `TableView`wird. Jede `Cell` Ableitung in Ihrem `TableSection` kann dann über Bindungen zu Eigenschaften der `PersonalInformation`-Klasse verfügen.

### <a name="custom-cells"></a>Benutzerdefinierte Zellen

Das [**conditionalcells**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalCells) -Beispiel wird auf **entryform**erweitert. Die [`ProgrammerInformation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs) -Klasse enthält eine boolesche Eigenschaft, die die Anwendbarkeit von zwei zusätzlichen Eigenschaften steuert. Für diese beiden zusätzlichen Eigenschaften verwendet das Programm eine benutzerdefinierte `PickerCell`, die auf einer " [pickercell. XAML](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml) " und " [PickerCell.XAML.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml.cs) " in der Bibliothek " [**xamarin. formsbook. Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) " basiert.

Obwohl die `IsEnabled` Eigenschaften der beiden `PickerCell` Elemente an die boolesche Eigenschaft in `ProgrammerInformation`gebunden sind, scheint dieses Verfahren nicht zu funktionieren, wodurch das nächste Beispiel angefordert wird.

### <a name="conditional-sections"></a>Bedingte Abschnitte

Im [**conditionalsection**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalSection) -Beispiel werden die beiden Elemente, die für die Auswahl des booleschen Elements bedingt sind, in einer separaten `TableSection`platziert. Durch die Code-Behind-Datei wird dieser Abschnitt aus dem `TableView` entfernt oder basierend auf der booleschen Eigenschaft wieder hinzugefügt.

### <a name="a-tableview-menu"></a>Ein TableView-Menü

Eine weitere Verwendung eines `TableView` ist ein Menü. Das [**MenuCommands**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/MenuCommands) -Beispiel veranschaulicht ein Menü, in dem Sie eine kleine `BoxView` um den Bildschirm verschieben können.

## <a name="related-links"></a>Verwandte Links

- [Kapitel 19 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch19-Apr2016.pdf)
- [Kapitel 19 Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19)
- [Auswahl](~/xamarin-forms/user-interface/picker/index.md)
- [ListView](~/xamarin-forms/user-interface/listview/index.md)
- [TableView](~/xamarin-forms/user-interface/tableview.md)
