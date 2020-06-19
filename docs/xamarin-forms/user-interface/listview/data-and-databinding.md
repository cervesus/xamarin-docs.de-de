---
title: ListView-Datenquellen
description: In diesem Artikel Xamarin.Forms wird erläutert, wie ListView mit Daten aufgefüllt wird und wie die Datenbindung mit einem ListView-Steuer Punkt verwendet wird.
ms.prod: xamarin
ms.assetid: B5571660-1E82-4379-95C3-0725288CF5D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/23/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 38a895c9064fc012aec35b37eac78bb16ff009a9
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84131507"
---
# <a name="listview-data-sources"></a>ListView-Datenquellen

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-switchentrytwobinding)

Eine Xamarin.Forms [`ListView`](xref:Xamarin.Forms.ListView) wird zum Anzeigen von Listen mit Daten verwendet. In diesem Artikel wird erläutert, wie ein `ListView` mit Daten aufgefüllt wird und wie Daten an das ausgewählte Element gebunden werden.

## <a name="itemssource"></a>ItemsSource

Eine [`ListView`](xref:Xamarin.Forms.ListView) wird mithilfe der-Eigenschaft mit Daten aufgefüllt [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) , die jede Auflistung akzeptieren kann, die implementiert `IEnumerable` . Die einfachste Möglichkeit zum Auffüllen einer `ListView` umfasst das Verwenden eines Arrays von Zeichen folgen:

```xaml
<ListView>
      <ListView.ItemsSource>
          <x:Array Type="{x:Type x:String}">
            <x:String>mono</x:String>
            <x:String>monodroid</x:String>
            <x:String>monotouch</x:String>
            <x:String>monorail</x:String>
            <x:String>monodevelop</x:String>
            <x:String>monotone</x:String>
            <x:String>monopoly</x:String>
            <x:String>monomodal</x:String>
            <x:String>mononucleosis</x:String>
          </x:Array>
      </ListView.ItemsSource>
</ListView>
```

Der entsprechende C#-Code lautet:

```csharp
var listView = new ListView();
listView.ItemsSource = new string[]
{
  "mono",
  "monodroid",
  "monotouch",
  "monorail",
  "monodevelop",
  "monotone",
  "monopoly",
  "monomodal",
  "mononucleosis"
};
```

![](data-and-databinding-images/itemssource-simple.png "ListView Displaying List of Strings")

Diese Vorgehensweise füllt den `ListView` mit einer Liste von Zeichen folgen auf. Standardmäßig `ListView` wird von aufgerufen, `ToString` und das Ergebnis wird in einer `TextCell` für jede Zeile angezeigt. Informationen zum Anpassen der Anzeige von Daten finden Sie unter [Zellen](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)Darstellung.

Da `ItemsSource` an ein Array gesendet wurde, wird der Inhalt nicht aktualisiert, wenn die zugrunde liegende Liste oder das zugrunde liegende Array geändert wird. Wenn die ListView automatisch aktualisiert werden soll, wenn Elemente hinzugefügt, entfernt und in der zugrunde liegenden Liste geändert werden, müssen Sie eine verwenden `ObservableCollection` . [`ObservableCollection`](xref:System.Collections.ObjectModel.ObservableCollection`1)wird in definiert `System.Collections.ObjectModel` und ähnelt `List` , mit der Ausnahme, dass Sie über Änderungen benachrichtigt werden kann `ListView` :

```csharp
ObservableCollection<Employee> employees = new ObservableCollection<Employee>();
listView.ItemsSource = employees;

//Mr. Mono will be added to the ListView because it uses an ObservableCollection
employees.Add(new Employee(){ DisplayName="Mr. Mono"});
```

## <a name="data-binding"></a>Datenbindung

Die Datenbindung ist der "Klebstoff", der die Eigenschaften eines Benutzeroberflächen Objekts an die Eigenschaften eines CLR-Objekts bindet, z. b. eine Klasse in "ViewModel". Die Datenbindung ist nützlich, da Sie die Entwicklung von Benutzeroberflächen vereinfacht, indem Sie viele langweilige Code Bausteine ersetzen.

Die Datenbindung funktioniert, indem Objekte synchronisiert werden, wenn sich die gebundenen Werte ändern. Anstatt Ereignishandler für jedes Mal schreiben zu müssen, wenn sich der Wert eines Steuer Elements ändert, können Sie die Bindung einrichten und die Bindung in "ViewModel" aktivieren.

Weitere Informationen zur Datenbindung finden Sie unter [Grundlagen der Datenbindung](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md) . Dies ist der vierte Teil der [ Xamarin.Forms Artikel Reihe zu XAML-Grundlagen](~/xamarin-forms/xaml/xaml-basics/index.md).

### <a name="binding-cells"></a>Binden von Zellen

Eigenschaften von Zellen (und untergeordnete Elemente von Zellen) können an Eigenschaften von Objekten in der gebunden werden `ItemsSource` . Beispielsweise kann ein `ListView` verwendet werden, um eine Liste von Mitarbeitern darzustellen.

Die Employee-Klasse:

```csharp
public class Employee
{
    public string DisplayName {get; set;}
}
```

Ein `ObservableCollection<Employee>` wird erstellt, als festgelegt `ListView` `ItemsSource` , und die Liste wird mit Daten aufgefüllt:

```csharp
ObservableCollection<Employee> employees = new ObservableCollection<Employee>();
public ObservableCollection<Employee> Employees { get { return employees; }}

public EmployeeListPage()
{
    EmployeeView.ItemsSource = employees;

    // ObservableCollection allows items to be added after ItemsSource
    // is set and the UI will react to changes
    employees.Add(new Employee{ DisplayName="Rob Finnerty"});
    employees.Add(new Employee{ DisplayName="Bill Wrestler"});
    employees.Add(new Employee{ DisplayName="Dr. Geri-Beth Hooper"});
    employees.Add(new Employee{ DisplayName="Dr. Keith Joyce-Purdy"});
    employees.Add(new Employee{ DisplayName="Sheri Spruce"});
    employees.Add(new Employee{ DisplayName="Burt Indybrick"});
}
```

> [!WARNING]
> Während ein als `ListView` Reaktion auf Änderungen im zugrunde liegenden aktualisiert wird `ObservableCollection` , `ListView` wird nicht aktualisiert, wenn `ObservableCollection` dem ursprünglichen Verweis eine andere Instanz zugewiesen ist `ObservableCollection` (z. b. `employees = otherObservableCollection;` ).

Der folgende Code Ausschnitt veranschaulicht eine `ListView` , die an eine Liste von Mitarbeitern gebunden ist:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:constants="clr-namespace:XamarinFormsSample;assembly=XamarinFormsXamlSample"
             x:Class="XamarinFormsXamlSample.Views.EmployeeListPage"
             Title="Employee List">
  <ListView x:Name="EmployeeView"
            ItemsSource="{Binding Employees}">
    <ListView.ItemTemplate>
      <DataTemplate>
        <TextCell Text="{Binding DisplayName}" />
      </DataTemplate>
    </ListView.ItemTemplate>
  </ListView>
</ContentPage>
```

In diesem XAML-Beispiel wird ein definiert `ContentPage` , das eine enthält `ListView` . Die Datenquelle der `ListView` wird über das Attribut `ItemsSource` festgelegt. Das Layout der einzelnen Zeilen in der `ItemsSource` wird im Element `ListView.ItemTemplate` definiert. Dies führt zu den folgenden Screenshots:

![](data-and-databinding-images/bound-data.png "ListView using Data Binding")

> [!WARNING]
> `ObservableCollection` ist nicht threadsicher. Das Ändern einer `ObservableCollection` bewirkt, dass UI-Aktualisierungen in demselben Thread durchgeführt werden, der die Änderungen durchgeführt hat. Wenn der Thread nicht der primäre UI-Thread ist, wird eine Ausnahme ausgelöst.

### <a name="binding-selecteditem"></a>Binden von SelectedItem

Häufig möchten Sie die Bindung an das ausgewählte Element einer `ListView` vornehmen, anstatt einen Ereignishandler zu verwenden, um auf Änderungen zu reagieren. Binden Sie die-Eigenschaft, um dies in XAML zu erreichen `SelectedItem` :

```xaml
<ListView x:Name="listView"
          SelectedItem="{Binding Source={x:Reference SomeLabel},
          Path=Text}">
 …
</ListView>
```

Wenn `listView` es `ItemsSource` sich um eine Liste von Zeichen folgen handelt, `SomeLabel` wird `Text` die-Eigenschaft an die gebunden `SelectedItem` .

## <a name="related-links"></a>Verwandte Links

- [Two-Way-Bindung (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-switchentrytwobinding)
