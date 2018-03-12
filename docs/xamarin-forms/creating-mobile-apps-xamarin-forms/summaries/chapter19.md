---
title: Zusammenfassung der Kapitel 19. Auflistungsansichten
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 37afa3a54fd20745a65312fb5a24d958c8ec405f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-19-collection-views"></a>Zusammenfassung der Kapitel 19. Auflistungsansichten

Xamarin.Forms definiert drei Ansichten werden die Auflistungen verwalten und ihre Elemente anzeigen:

- [`Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) ist eine relativ kurze Liste von Zeichenfolgen, die dem Benutzer ermöglicht, eine auswählen
- [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) wird häufig eine lange Liste von Elementen in der Regel vom selben Typ und Formatierung auch dadurch kann der Benutzer eine auswählen
- [`TableView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) ist eine Sammlung von *Zellen* (in der Regel von verschiedenen Typen und eindeutigkeitsmetrik) zum Anzeigen von Daten oder Verwalten von Benutzereingaben

Es ist üblich für MVVM Anwendungen für die Verwendung der `ListView` eine auswählbare Auflistung von Objekten angezeigt.

## <a name="program-options-with-picker"></a>Optionen für das Programm mit der Auswahl

Die [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) ist eine gute Wahl, wenn Sie eine Option aus einer relativ kurzen Übersicht der Benutzer kann müssen `string` Elemente.

### <a name="the-picker-and-event-handling"></a>Die Auswahl und die Ereignisbehandlung

Die [ **PickerDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerDemo) Beispiel veranschaulicht die XAML verwenden, um festzulegen der `Picker` [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Title/) Eigenschaft und fügen `string` Elemente, die die [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) Auflistung. Wenn der Benutzer wählt die `Picker`, es zeigt die Elemente in der `Items` Sammlung in eine plattformabhängig.

Die [ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/) Ereignis zeigt an, wenn der Benutzer ein Element ausgewählt wurde. Der nullbasierte [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) Eigenschaft gibt dann an das ausgewählte Element. Wenn kein Element ausgewählt ist, `SelectedIndex` gleich &#x2013;1.

Sie können auch `SelectedIndex` initialisieren Sie das ausgewählte Element, sondern muss festgelegt werden, nachdem die `Items` Auflistung gefüllt wird. In XAML, bedeutet dies, dass Sie wahrscheinlich ein Eigenschaftenelement festzulegende verwenden `SelectedIndex`.

### <a name="data-binding-the-picker"></a>Binden die Auswahl von Daten

Die `SelectedIndex` Eigenschaft durch eine bindbare Eigenschaft unterstützt wird, aber `Items` nicht der Fall ist, also mit der Datenbindung mit einem `Picker` ist schwierig. Eine Lösung ist die Verwendung der `Picker` in Kombination mit einem [ `ObjectToIndexConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ObjectToIndexConverter.cs) wie in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Bibliothek. Die [ **PickerBinding** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerBinding) wird veranschaulicht, wie dies funktioniert.

## <a name="rendering-data-with-listview"></a>Rendern von Daten mit ListView

Die [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) ist die einzige Klasse, die abgeleitet [ `ItemsView<TVisual>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) aus dem es Daten erbt die [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) und [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) Eigenschaften.

`ItemsSource` ist vom Typ `IEnumerable` ist jedoch `null` in der Standardeinstellung und muss explizit initialisiert oder (häufiger) über eine Datenbindung einer Sammlung festgelegt werden. Die Elemente in dieser Auflistung können eines beliebigen Typs sein.

`ListView` definiert eine [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SelectedItem/) , die entweder-Eigenschaftensatz auf eines der Elemente in der `ItemsSource` Auflistung oder `null` , wenn kein Element ausgewählt ist. `ListView` mit der [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) Ereignis aus, wenn ein neues Element ausgewählt ist.

### <a name="collections-and-selections"></a>Sammlungen und Auswahl

Die [ **ListViewList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewList) Beispiel füllt ein `ListView` mit 17 `Color` Werte in einer `List<Color>` Auflistung. Die Elemente können ausgewählt werden, jedoch in der Standardeinstellung angezeigt mit ihren uninteressant `ToString` Darstellungen. Mehrere Beispiele in diesem Kapitel zeigen, wie zur Anzeige von beheben und machen wie attraktiv wie gewünscht wird.

### <a name="the-row-separator"></a>Das Zeilentrennzeichen

Auf IOS- und Android zeigt trennt eine dünne Linie Zeilen. Sie können steuern, mit der [ `SeparatorVisibiliy` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SeparatorVisibility/) und [ `SeparatorColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SeparatorColor/) Eigenschaften. `SeparatorVisibility` -Eigenschaft ist vom Typ [ `SeparatorVisbility` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SeparatorVisibility/), eine Enumeration mit zwei Membern:

- [`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.SeparatorVisibility.Default/), die Standardeinstellung
- [`None`](https://developer.xamarin.com/api/field/Xamarin.Forms.SeparatorVisibility.None/)

### <a name="data-binding-the-selected-item"></a>Das ausgewählte Element binden von Daten

Die `SelectedItem` Eigenschaft durch eine bindbare Eigenschaft unterstützt wird, damit es die Quelle oder das Ziel einer Bindung sein kann. Der Standardwert `BindingMode` ist `OneWayToSource`, aber es ist im Allgemeinen das Ziel eine bidirektionale Datenbindung, insbesondere in Szenarien mit MVVM. Die [ **ListViewArray** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewArray) Beispiel veranschaulicht diesen Typ der Bindung.

### <a name="the-observablecollection-difference"></a>Der Unterschied ObservableCollection

Die [ **ListViewLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewLogger) Beispiel legt die `ItemsSource` Eigenschaft eine `ListView` auf eine `List<DateTime>` Auflistung und progressiv fügt dann ein neues `DateTime` Objekt der Auflistung jede Sekunde mithilfe eines Zeitgebers.

Allerdings die `ListView` nicht automatisch aktualisiert, da die `List<T>` Auflistung verfügt nicht über einen Benachrichtigungsmechanismus, um anzugeben, wenn Elemente hinzugefügt oder aus der Auflistung entfernt werden.

Eine viel bessere Klasse für die Verwendung in einem solchen Szenario ist [ `ObservableCollection<T>` ](https://developer.xamarin.com/api/type/System.Collections.ObjectModel.ObservableCollection%3CT%3E/) definiert, der `System.Collections.ObjectModel` Namespace. Diese Klasse implementiert die [ `INotifyCollectionChanged` ](https://developer.xamarin.com/api/type/System.Collections.Specialized.INotifyCollectionChanged/) -Schnittstelle, und infolgedessen wird auch ausgelöst wird eine [ `CollectionChanged` ](https://developer.xamarin.com/api/event/System.Collections.ObjectModel.ObservableCollection%3CT%3E.CollectionChanged/) Ereignis aus, wenn Elemente hinzugefügt oder entfernt aus der Auflistung oder bei ersetzt oder in verschoben die Auflistung. Wenn die `ListView` intern erkennt, die eine Klasse implementieren `INotifyCollectionChanged` vorsieht seine `ItemsSource` -Eigenschaft, fügt Sie einen Handler, der `CollectionChanged` Ereignis und seine Anzeige aktualisiert, wenn die Auflistung ändert.

Die [ **ObservableLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ObservableLogger) Beispiel veranschaulicht die Verwendung von `ObservableCollection`.

### <a name="templates-and-cells"></a>Vorlagen und Zellen

Wird standardmäßig ein `ListView` Zeigt Elemente in der Auflistung mithilfe des Elements `ToString` Methode. Ein besserer Ansatz umfasst das Definieren einer Vorlage zum Anzeigen von Elementen.

Um dieses Feature zu experimentieren, können Sie die [ `NamedColor` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs) -Klasse in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Bibliothek. Diese Klasse definiert eine statische `All` Eigenschaft vom Typ `IList<NamedColor>` 141 enthält `NamedColor` öffentlichen Felder des entsprechende Objekte die `Color` Struktur.

Die [ **NaiveNamedColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/NaiveNamedColorList) Beispiel legt die `ItemsSource` von einer `ListView` dieser `NamedColor.All` Eigenschaft, sondern nur die den vollqualifizierten Klassennamen der der `NamedColor` Objekte sind angezeigt.

`ListView` benötigt eine Vorlage aus, um diese Elemente anzuzeigen. Im Code können Sie festlegen der [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) von definierte Eigenschaft `ItemsView<TVisual>` auf eine [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) -Objekt unter Verwendung der [ `DataTemplate` Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.DataTemplate.DataTemplate/p/System.Type/) , verweist auf eine Ableitung von der [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) Klasse. `Cell` verfügt über fünf ableitungen aus:

- [`TextCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/) &#x2014; enthält zwei `Label` Ansichten (prinzipiell sprechen)
- [`ImageCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/) &#x2014; Fügt eine `Image` , anzeigen `TextCell`
- [`EntryCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/) &#x2014; enthält eine `Entry` anzeigen, die mit einer `Label`
- [`SwitchCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/) &#x2014; enthält eine `Switch` mit einer `Label`
- [`ViewCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) &#x2014; kann eine `View` (wahrscheinlich mit untergeordneten Elementen)

Rufen Sie anschließend [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.DataTemplate.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) und [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.DataTemplate.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) auf die `DataTemplate` Objekt, das Zuordnen von Werten mit der `Cell` Eigenschaften oder datenbindungen für festgelegt die `Cell` Eigenschaften verweisen auf Eigenschaften der Elemente in der `ItemsSource` Auflistung. Dies wird dargestellt, der [ **TextCellListCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListCode) Beispiel.

Jedes Element dargestellte ist die `ListView`, eine kleine visuelle Struktur aus der Vorlage erstellt wird und datenbindungen zwischen dem Element und die Eigenschaften der Elemente in dieser visuellen Struktur hergestellt werden. Sie erhalten einen Überblick über diesen Prozess durch Installieren der Handler für das [ `ItemAppearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemAppearing/) und [ `ItemDisappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemDisappearing/) Ereignisse der `ListView`, oder indem Sie eine Alternative [ `DataTemplate`Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.DataTemplate.DataTemplate/p/System.Func%7BSystem.Object%7D/) , verwendet eine Funktion, die jedes Mal aufgerufen wird, ein Element der visuellen Struktur erstellt werden muss.

Die [ **TextCellListXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListXaml) zeigt eine funktionell Programm vollständig in XAML. Ein `DataTemplate` Tag auf festgelegt ist die `ItemTemplate` Eigenschaft von der `ListView`, und klicken Sie dann die `TextCell` auf festgelegt ist die `DataTemplate`. Bindungen zu den Eigenschaften der Elemente in der Auflistung werden festgelegt, direkt auf die [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TextCell.Text/) und [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TextCell.Detail/) Eigenschaften der `TextCell`.

### <a name="custom-cells"></a>Benutzerdefinierte Zellen

In XAML ist es möglich, festzulegen eine [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) auf die `DataTemplate` und definieren Sie eine benutzerdefinierte visuelle Struktur als der [ `View` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ViewCell.View/) Eigenschaft `ViewCell`. (`View` ist die Inhaltseigenschaft des `ViewCell` also die `ViewCell.View` Tags sind nicht erforderlich.) Die [ **CustomNamedColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CustomNamedColorList) Beispiel wird diese Technik veranschaulicht:

[![Dreifacher Screenshot der benutzerdefinierte benannte Farbenliste](images/ch19fg11-small.png "benutzerdefinierte benannte Farbenliste")](images/ch19fg11-large.png "benutzerdefinierte benannte Farbenliste")

Abrufen der Größe für alle Plattformen kann schwierig sein. Die [ `RowHeight` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.RowHeight/) Eigenschaft ist nützlich, aber in einigen Fällen sollten Sie zum Verwenden der [ `HasUnevenRows` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.HasUnevenRows/) -Eigenschaft, die weniger effizient ist, aber erzwingt, dass die `ListView` Größe der Zeilen. Für iOS und Android müssen Sie einen dieser beiden Eigenschaften verwenden, um ordnungsgemäße Zeilengröße abzurufen.

### <a name="grouping-the-listview-items"></a>Die ListView-Elemente zu gruppieren

`ListView` die Gruppierung von Elementen und das Navigieren zwischen diesen Gruppen unterstützt. Die `ItemsSource` Eigenschaft muss festgelegt werden, um eine Auflistung von Auflistungen: das Objekt, `ItemsSource` wird festgelegt, muss implementieren `IEnumerable`, und jedes Element in der Auflistung muss auch implementieren `IEnumerable`. Jede Gruppe sollte zwei Eigenschaften enthalten: eine textbeschreibung der Gruppe und eine dreibuchstabige Abkürzung.

Die [ `NamedColorGroup` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColorGroup.cs) -Klasse in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Bibliothek erstellt sieben Gruppen von `NamedColor` Objekte. Die [ **ColorGroupList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorGroupList) Beispiel wird gezeigt, wie Sie diese Gruppen mit der [ `IsGroupingEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.IsGroupingEnabled/) Eigenschaft `ListView` festgelegt `true`, und die [ `GroupDisplayBinding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupDisplayBinding/) und [ `GroupShortNameBinding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupShortNameBinding/) Eigenschaften gebunden an Eigenschaften in jeder Gruppe.

### <a name="custom-group-headers"></a>Benutzerdefinierte Kopfzeilen

Es ist möglich, Erstellen von benutzerdefinierten Header für die `ListView` Gruppen durch Ersetzen der `GroupDisplayBinding` Eigenschaft mit dem [ `GroupHeaderTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupHeaderTemplate/) definieren eine Vorlage für die Header.

### <a name="listview-and-interactivity"></a>ListView-Steuerelement und Interaktivität

Im Allgemeinen erhält eine Anwendung eine Benutzerinteraktion mit einer `ListView` durch Anfügen von einem Handler, der `ItemSelected` oder [ `ItemTapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemTapped/) Ereignis, oder indem Sie eine Datenbindung für die `SelectedItem` Eigenschaft. Aber einige Zellentypen von (`EntryCell` und `SwitchCell`) ermöglichen die Interaktion des Benutzers, und es ist auch möglich, benutzerdefinierte Zellen, die selbst erstellen, die mit dem Benutzer interagieren. Die [ **InteractiveListView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/InteractiveListView) erstellt 100 Instanzen von [ `ColorViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs) und ermöglicht dem Benutzer so ändern Sie jede Farbe, die mit einem Trio von `Slider` Elemente. Das Programm auch nutzt die [ `ColorToContrastColorConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorToContrastColorConverter.cs) in der [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit).

## <a name="listview-and-mvvm"></a>ListView-Steuerelement und MVVM

`ListView` spielt eine wichtige Rolle in MVVM-Szenarien. Wenn ein `IEnumerable` Auflistung in ein ViewModel vorhanden ist, wird es häufig zu gebunden ist eine `ListView`. Darüber hinaus die Elemente in der Auflistung häufig implementieren `INotifyPropertyChanged` an Eigenschaften in einer Vorlage gebunden werden soll.

### <a name="a-collection-of-viewmodels"></a>Eine Auflistung von ViewModels

Um dies zu untersuchen der [ **SchoolOfFineArts** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) Bibliothek erstellt mehrere Klassen, die auf der Grundlage einer [XML-Datendatei](http://xamarin.github.io/xamarin-forms-book-samples/SchoolOfFineArt/students.xml) und Bilder der fiktiven Schüler dieses fiktive Schule.

Die [ `Student` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/Student.cs) Klasse abgeleitet [ `ViewModelBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/ViewModelBase.cs). Die [ `StudentBody` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/StudentBody.cs) Klasse ist eine Sammlung von `Student` Objekte und leitet auch von `ViewModelBase`. Die [ `SchoolViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/SchoolViewModel.cs) downloads die XML-Datei und alle Objekte assembliert.

Die [ **StudentList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/StudentList) Programm verwendet eine `ImageCell` zum Anzeigen der Studenten und Bilder in einem `ListView`:

[![Dreifacher Screenshot der Student Liste](images/ch19fg18-small.png "Student Liste")](images/ch19fg18-large.png "Student-Liste")

Die [ **ListViewHeader** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewHeader) Beispiel fügt eine [ `Header` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.Header/) Eigenschaft aber nur angezeigt auf Android-Geräten.

### <a name="selection-and-the-binding-context"></a>Auswahl und Bindungskontext

Die [ **SelectedStudentDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/SelectedStudentDetail) Programmieren bindet die `BindingContext` von eine `StackLayout` auf die `SelectedItem` Eigenschaft der `ListView`. Dadurch wird das Programm, um ausführliche Informationen zum ausgewählten Student anzuzeigen.

### <a name="context-menus"></a>Kontextmenüs

Eine Zelle kann ein Kontextmenü definieren, die auf einen plattformspezifischen Weise implementiert wird. Fügen Sie zum Erstellen dieses Menüs [ `MenuItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MenuItem/) Datenbankobjekte in der [ `ContextActions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Cell.ContextActions/) Eigenschaft von der `Cell`.

`MenuItem` werden fünf Eigenschaften definiert:

- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Text/) vom Typ `string`
- [`Icon`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Icon/) vom Typ `FileImageSource`
- [`IsDestructive`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.IsDestructive/) vom Typ `bool`
- [`Command`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Command/) vom Typ `ICommand`
- [`CommandParameter`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.CommandParameter/) vom Typ `object`

Die `Command` und `CommandParameter` Eigenschaften impliziert, dass das ViewModel für jedes Element, Methoden enthält, um die gewünschten Menübefehle durchzuführen. In Szenarien nicht MVVM `MenuItem` definiert außerdem eine [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.MenuItem.Clicked/) Ereignis.

Die [ **CellContextMenu** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CellContextMenu) wird diese Technik veranschaulicht. Die `Command` -Eigenschaft jedes `MenuItem` gebunden ist, eine Eigenschaft vom Typ `ICommand` in die `Student` Klasse. Legen Sie die `IsDestructive` Eigenschaft `true` für eine `MenuItem` , entfernt oder löscht das ausgewählte Objekt.

### <a name="varying-the-visuals"></a>Die visuellen Elemente VARYING

In einigen Fällen benötigen Sie geringfügige abweichungen bei der die visuellen Elemente der Elemente in der `ListView` anhand einer Eigenschaft. Z. B. wenn ein Student Grade Point-Durchschnitt unterschreitet 2.0 die [ **ColorCodedStudents** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorCodedStudents) Beispiel werden diese Student Namen in Rot angezeigt.
Dies erfolgt mithilfe eines Wertkonverters Bindung [ `ThresholdToObjectConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ThresholdToObjectConverter.cs)in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Bibliothek.

### <a name="refreshing-the-content"></a>Der Inhalt wird aktualisiert.

Die `ListView` unterstützt eine Geste Pulldownliste seine Daten zu aktualisieren. Legen Sie das Programm muss die [ `IsPullToRefresh` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.IsPullToRefreshEnabled/) Eigenschaft `true` aktivieren. Die `ListView` reagiert auf Pulldownliste Bewegung durch Festlegen seiner [ `IsRefreshing` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.IsRefreshing/) Eigenschaft `true`, und durch das Auslösen der [ `Refreshing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.Refreshing/) Ereignis und (für Szenarien mit MVVM) aufrufen die `Execute` Methode seine [ `RefreshCommand` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.RefreshCommand/) Eigenschaft.

Code behandeln die `Refresh` Ereignis oder die `RefreshCommand` möglicherweise aktualisiert dann die angezeigten Daten durch die `ListView` und legt `IsRefreshing` an `false`.

Die [ **RssFeed** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/RssFeed) Beispiel veranschaulicht die Verwendung einer [ `RssFeedViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/RssFeed/RssFeed/RssFeed/RssFeedViewModel.cs) implementiert `RefreshCommand` und `IsRefreshing` Eigenschaften für die Datenbindung.

## <a name="the-tableview-and-its-intents"></a>Die TableView und seine intents

Während der `ListView` zeigt in der Regel mehrere Instanzen desselben Typs, der [ `TableView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) ist im Allgemeinen konzentriert sich auf einer Benutzeroberfläche für mehrere Eigenschaften der verschiedenen Typen. Jedes Element bezieht sich auf einem eigenen [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) Ableitung für die Eigenschaft anzeigen oder Bereitstellen einer Benutzeroberfläche zu.

### <a name="properties-and-hierarchies"></a>Eigenschaften und Hierarchien

`TableView` definiert vier Eigenschaften:

- [`Intent`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.Intent/) Der Typ [ `TableIntent` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableIntent/), eine Enumeration
- [`Root`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.Root/) Der Typ [ `TableRoot` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableRoot/), die Content-Eigenschaft des `TableView`
- [`RowHeight`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.RowHeight/) vom Typ `int`
- [`HasUnevenRows`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.HasUnevenRows/) vom Typ `bool`

Die `TableIntent` Enumeration gibt an, wie Sie verwenden möchten die `TableView`:

- [`Data`](https://developer.xamarin.com/api/field/Xamarin.Forms.TableIntent.Data/)
- [`Form`](https://developer.xamarin.com/api/field/Xamarin.Forms.TableIntent.Form/)
- [`Settings`](https://developer.xamarin.com/api/field/Xamarin.Forms.TableIntent.Settings/)
- [`Menu`](https://developer.xamarin.com/api/field/Xamarin.Forms.TableIntent.Menu/)

Diese Elemente vorschlagen auch einige Verwendungsmöglichkeiten für die `TableView`.

Mehrere andere Klassen beteiligt sind eine Tabelle definiert:

- [`TableSectionBase`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSectionBase/) ist eine abstrakte Klasse, die abgeleitet `BindableObject` und definiert einen [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TableSectionBase.Title/) Eigenschaft

- [`TableSectionBase<T>`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSectionBase%3CT%3E/) ist eine abstrakte Klasse, die abgeleitet `TableSectionBase` und implementiert `IList<T>` und `INotifyCollectionChanged`

- [`TableSection`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSection/) leitet sich von `TableSectionBase<Cell>`

- [`TableRoot`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableRoot/) leitet sich von `TableSectionBase<TableSection>`

Kurz gesagt, `TableView` verfügt über eine `Root` -Eigenschaft, die Sie, um festlegen eine `TableRoot` -Objekt, das eine Auflistung von `TableSection` Objekte, von denen jedes eine Sammlung von ist `Cell` Objekte. Eine Tabelle hat wiederum mehrere Abschnitte umfasst, und jeder Abschnitt enthält mehrere Zellen. Die Tabelle selbst kann einen Titel, und jeder Abschnitt kann einen Titel. Obwohl `TableView` nutzt `Cell` ableitungen, es nimmt mithilfe des `DataTemplate`.

### <a name="a-prosaic-form"></a>Ein prosaischen Formular

Die [ **EntryForm** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/EntryForm) Beispiel definiert eine [ `PersonalInformation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs) Ansichtsmodell, eine Instanz, die wird die `BindingContext` von der `TableView`. Jede `Cell` Ableitung in seiner `TableSection` Bindungen Eigenschaften verfügen können die `PersonalInformation` Klasse.

### <a name="custom-cells"></a>Benutzerdefinierte Zellen

Die [ **ConditionalCells** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalCells) Beispiel erweitert **EntryForm**. Die [ `ProgrammerInformation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs) Klasse enthält eine boolesche Eigenschaft, die die Anwendbarkeit der zwei weitere Eigenschaften bestimmt. Für diese zwei zusätzlichen Eigenschaften, die Anwendung verwendet ein benutzerdefiniertes `PickerCell` basierend auf einer [PickerCell.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml) und [PickerCell.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml.cs) in der [  **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Bibliothek.

Obwohl die `IsEnabled` Eigenschaften der beiden `PickerCell` Elemente gebunden sind, um die boolesche Eigenschaft in `ProgrammerInformation`, diese Technik scheint nicht funktionsfähig sind, im nächste Beispiel aufgefordert.

### <a name="conditional-sections"></a>Bedingte Abschnitte

Die [ **ConditionalSection** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalSection) Beispiel fügt die beiden Elemente, die auf der Auswahl des Elements in einem separaten booleschen bedingte sind `TableSection`. Die Code-Behind-Datei entfernt diese aus dem Abschnitt der `TableView` oder fügt es wieder basierend auf die boolesche Eigenschaft.

### <a name="a-tableview-menu"></a>Ein Menü TableView

Eine weitere Verwendungsmöglichkeit von einem `TableView` wird ein Menü. Die [ **MenuCommands** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/MenuCommands) Beispiel zeigt ein Menü, in dem Sie ein wenig verschieben kann `BoxView` auf dem Bildschirm.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 19 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch19-Apr2016.pdf)
- [Kapitel 19-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19)
- [ListView](~/xamarin-forms/user-interface/listview/index.md)
- [TableView](~/xamarin-forms/user-interface/tableview.md)
