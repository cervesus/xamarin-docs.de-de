---
title: ListView-Datenquellen
description: In diesem Artikel wird erläutert, wie die Xamarin.Forms-ListView mit Daten aufgefüllt und wie Sie die Datenbindung mit einer ListView verwenden.
ms.prod: xamarin
ms.assetid: B5571660-1E82-4379-95C3-0725288CF5D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/23/2020
ms.openlocfilehash: e51f0bd011750b030c0a11b9b89a2c2473f2a9ed
ms.sourcegitcommit: d83c6af42ed26947aa7c0ecfce00b9ef60f33319
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/25/2020
ms.locfileid: "80247586"
---
# <a name="listview-data-sources"></a>ListView-Datenquellen

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-switchentrytwobinding)

Ein xamarin. Forms- [`ListView`](xref:Xamarin.Forms.ListView) wird zum Anzeigen von Listen mit Daten verwendet. In diesem Artikel wird erläutert, wie eine `ListView` mit Daten aufgefüllt und Daten an das ausgewählte Element gebunden werden.

## <a name="itemssource"></a>ItemsSource

Ein [`ListView`](xref:Xamarin.Forms.ListView) wird mithilfe der [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) -Eigenschaft mit Daten aufgefüllt, die jede Auflistung akzeptieren kann, die `IEnumerable`implementiert. Die einfachste Möglichkeit zum Auffüllen eines `ListView` besteht darin, ein Array von Zeichen folgen zu verwenden:

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

Diese Vorgehensweise füllt die `ListView` mit einer Liste von Zeichen folgen auf. Standardmäßig ruft `ListView` `ToString` auf und zeigt das Ergebnis für jede Zeile in einem `TextCell` an. Informationen zum Anpassen der Anzeige von Daten finden Sie unter [Zellen](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)Darstellung.

Da `ItemsSource` an ein Array gesendet wurde, wird der Inhalt nicht aktualisiert, wenn die zugrunde liegende Liste oder das zugrunde liegende Array geändert wird. Wenn die ListView automatisch aktualisiert werden soll, wenn Elemente hinzugefügt, entfernt und in der zugrunde liegenden Liste geändert werden, müssen Sie eine `ObservableCollection`verwenden. [`ObservableCollection`](xref:System.Collections.ObjectModel.ObservableCollection`1) ist in `System.Collections.ObjectModel` definiert und ähnelt `List`, mit dem Unterschied, dass `ListView` von Änderungen benachrichtigt werden kann:

```csharp
ObservableCollection<Employee> employees = new ObservableCollection<Employee>();
listView.ItemsSource = employees;

//Mr. Mono will be added to the ListView because it uses an ObservableCollection
employees.Add(new Employee(){ DisplayName="Mr. Mono"});
```

## <a name="data-binding"></a>Datenbindung

Die Datenbindung ist der "Klebstoff", der die Eigenschaften eines Benutzeroberflächen Objekts an die Eigenschaften eines CLR-Objekts bindet, z. b. eine Klasse in "ViewModel". Die Datenbindung ist hilfreich, da die Entwicklung von Benutzeroberflächen vereinfacht durch Ersetzen eine Menge Standardcode langweilig.

Die Datenbindung funktioniert durch das Synchronisieren von Objekten wie deren gebundenen Werte zu ändern. Anstatt Ereignishandler für jedes Mal schreiben zu müssen, wenn sich der Wert eines Steuer Elements ändert, können Sie die Bindung einrichten und die Bindung in "ViewModel" aktivieren.

Weitere Informationen zur Datenbindung finden Sie unter [Grundlagen der Datenbindung](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md) . Dies ist der vierte Teil der [Artikel Reihe zu xamarin. Forms-XAML-Grundlagen](~/xamarin-forms/xaml/xaml-basics/index.md).

### <a name="binding-cells"></a>Binden von Zellen

Eigenschaften von Zellen (und untergeordnete Elemente von Zellen) können an Eigenschaften von Objekten in der `ItemsSource`gebunden werden. Beispielsweise kann ein `ListView` verwendet werden, um eine Liste von Mitarbeitern darzustellen.

Die Employee-Klasse:

```csharp
public class Employee
{
    public string DisplayName {get; set;}
}
```

Ein `ObservableCollection<Employee>` wird erstellt, als `ListView` `ItemsSource`festgelegt, und die Liste wird mit Daten aufgefüllt:

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
> Während eine `ListView` als Reaktion auf Änderungen in der zugrunde liegenden `ObservableCollection`aktualisiert wird, wird ein `ListView` nicht aktualisiert, wenn dem ursprünglichen `ObservableCollection` Verweis (z. b. `employees = otherObservableCollection;`) eine andere `ObservableCollection` Instanz zugewiesen ist.

Der folgende Code Ausschnitt veranschaulicht eine `ListView`, die an eine Liste von Mitarbeitern gebunden ist:

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

Dieses XAML-Beispiel definiert eine `ContentPage`, die eine `ListView`enthält. Die Datenquelle des `ListView` wird über das `ItemsSource`-Attribut festgelegt. Das Layout der einzelnen Zeilen im `ItemsSource` wird innerhalb des `ListView.ItemTemplate`-Elements definiert. Dies führt zu den folgenden Screenshots:

![](data-and-databinding-images/bound-data.png "ListView using Data Binding")

> [!WARNING]
> `ObservableCollection` ist nicht Thread sicher. Das Ändern einer `ObservableCollection` bewirkt, dass UI-Aktualisierungen in demselben Thread durchgeführt werden, der die Änderungen durchgeführt hat. Wenn der Thread nicht der primäre UI-Thread ist, wird eine Ausnahme ausgelöst.

### <a name="binding-selecteditem"></a>Bindung SelectedItem

Häufig ist es ratsam, eine Bindung an das ausgewählte Element einer `ListView`vorzunehmen, anstatt einen Ereignishandler zu verwenden, um auf Änderungen zu reagieren. Binden Sie die `SelectedItem`-Eigenschaft, um dies in XAML zu erreichen:

```xaml
<ListView x:Name="listView"
          SelectedItem="{Binding Source={x:Reference SomeLabel},
          Path=Text}">
 …
</ListView>
```

Wenn `listView``ItemsSource` eine Liste von Zeichen folgen ist, wird `SomeLabel` seine `Text`-Eigenschaft an den `SelectedItem`gebunden.

## <a name="related-links"></a>Verwandte Links

- [Two-Way-Bindung (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-switchentrytwobinding)
