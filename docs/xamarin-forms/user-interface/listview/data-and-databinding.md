---
title: ListView-Datenquellen
description: In diesem Artikel wird erläutert, wie die Xamarin.Forms-ListView mit Daten aufgefüllt und wie Sie die Datenbindung mit einer ListView verwenden.
ms.prod: xamarin
ms.assetid: B5571660-1E82-4379-95C3-0725288CF5D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/30/2018
ms.openlocfilehash: e53f6dce47dd7db60267d21c8d816ece554dc46c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61319928"
---
# <a name="listview-data-sources"></a>ListView-Datenquellen

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/SwitchEntryTwoBinding)

Ein [ `ListView` ](xref:Xamarin.Forms.ListView) wird für die Anzeige von Listen mit Daten verwendet. Wir lernen, zum Auffüllen einer ListView mit Daten, und wie für das ausgewählte Element gebunden werden kann.

## <a name="itemssource"></a>ItemsSource

Ein [ `ListView` ](xref:Xamarin.Forms.ListView) mit Daten aufgefüllt ist die [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) -Eigenschaft, die Auflistung implementieren akzeptieren kann `IEnumerable`. Die einfachste Methode zum Auffüllen einer `ListView` dazu werden ein Array von Zeichenfolgen:

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

Der entsprechende C#-Code ist:

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

//monochrome will not appear in the list because it was added
//after the list was populated.
listView.ItemsSource.Add("monochrome");
```

![](data-and-databinding-images/itemssource-simple.png "Anzeigen von ListView-Liste von Zeichenfolgen")

Der oben beschriebene Ansatz füllt die `ListView` mit einer Liste von Zeichenfolgen. In der Standardeinstellung `ListView` ruft `ToString` und zeigt das Ergebnis in eine `TextCell` für jede Zeile. Um anzupassen, wie Daten angezeigt werden, finden Sie unter [Zellendarstellung](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md).

Da `ItemsSource` gesendet wurde in ein Array, der Inhalt wird nicht aktualisiert werden, als die zugrunde liegenden Liste oder Array ändern. Wenn die ListView automatisch aktualisiert werden, wie Elemente hinzugefügt, entfernt und in der zugrunde liegenden Liste geändert werden sollen, müssen Sie verwenden eine `ObservableCollection`. [`ObservableCollection`](xref:System.Collections.ObjectModel.ObservableCollection`1) wird definiert, `System.Collections.ObjectModel` und ist wie `List`, außer dass sie benachrichtigt, kann `ListView` von Änderungen:

```csharp
ObservableCollection<Employee> employees = new ObservableCollection<Employee>();
listView.ItemsSource = employees;

//Mr. Mono will be added to the ListView because it uses an ObservableCollection
employees.Add(new Employee(){ DisplayName="Mr. Mono"});
```

<a name="Data_Binding" />

## <a name="data-binding"></a>Datenbindung
Datenbindung ist der "Glue", die die Eigenschaften eines Benutzerobjekts-Schnittstelle an den Eigenschaften des einige CLR-Objekt, z. B. eine Klasse in Ihrem ViewModel gebunden wird. Die Datenbindung ist hilfreich, da die Entwicklung von Benutzeroberflächen vereinfacht durch Ersetzen eine Menge Standardcode langweilig.

Die Datenbindung funktioniert durch das Synchronisieren von Objekten wie deren gebundenen Werte zu ändern. Statt-Ereignishandlern für schreiben, jedes Mal, wenn der Wert eines Steuerelements ändert, müssen Sie die Bindung herzustellen und Bindung in Ihrem ViewModel zu aktivieren.

Weitere Informationen zur Datenbindung finden Sie unter [Grundlagen der Datenbindung](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md) die ist der vierte Teil von der [Xamarin.Forms XAML Basics-Artikelreihe](~/xamarin-forms/xaml/xaml-basics/index.md).

### <a name="binding-cells"></a>Binden von Zellen
Eigenschaften von Zellen (und untergeordnete Elemente von Zellen) gebunden werden können, um Eigenschaften von Objekten in der `ItemsSource`. Beispielsweise kann ein ListView verwendet werden, um eine Liste von Mitarbeitern darzustellen.

Die Employee-Klasse:

```csharp
public class Employee
{
    public string DisplayName {get; set;}
}
```

`ObservableCollection<Employee>` erstellt wird und wie die `ListView`des `ItemsSource`:

```csharp
ObservableCollection<Employee> employees = new ObservableCollection<Employee>();
public EmployeeListPage()
{
  //defined in XAML to follow
  EmployeeView.ItemsSource = employees;
  ...
}
```

Die Liste wird mit Daten aufgefüllt:

```csharp
public EmployeeListPage()
{
  ...
  employees.Add(new Employee{ DisplayName="Rob Finnerty"});
  employees.Add(new Employee{ DisplayName="Bill Wrestler"});
  employees.Add(new Employee{ DisplayName="Dr. Geri-Beth Hooper"});
  employees.Add(new Employee{ DisplayName="Dr. Keith Joyce-Purdy"});
  employees.Add(new Employee{ DisplayName="Sheri Spruce"});
  employees.Add(new Employee{ DisplayName="Burt Indybrick"});
}
```

Der folgende Codeausschnitt zeigt ein `ListView` an eine Liste von Mitarbeitern gebunden:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
xmlns:constants="clr-namespace:XamarinFormsSample;assembly=XamarinFormsXamlSample"
x:Class="XamarinFormsXamlSample.Views.EmployeeListPage"
Title="Employee List">
  <ListView x:Name="EmployeeView">
    <ListView.ItemTemplate>
      <DataTemplate>
        <TextCell Text="{Binding DisplayName}" />
      </DataTemplate>
    </ListView.ItemTemplate>
  </ListView>
</ContentPage>
```

Beachten Sie, dass die Bindung im Code der Einfachheit halber eingerichtet wurde, auch wenn es in XAML gebunden wurden konnte.

Das vorherige Bit des XAML definiert einen `ContentPage` , enthält eine `ListView`. Die Datenquelle die `ListView` festgelegt ist, über die `ItemsSource` Attribut. Das Layout der einzelnen Zeilen in der `ItemsSource`wird definiert, in der `ListView.ItemTemplate` Element.

Dies ist das Ergebnis:

![](data-and-databinding-images/bound-data.png "ListView mit Datenbindung")

### <a name="binding-selecteditem"></a>Bindung SelectedItem

Häufig möchten Sie binden an das ausgewählte Element eine `ListView`, sondern als einen Ereignishandler zu verwenden, um auf Änderungen reagieren. Zu diesem Zweck in XAML zu binden der `SelectedItem` Eigenschaft:

```xaml
<ListView x:Name="listView"
 SelectedItem="{Binding Source={x:Reference SomeLabel},
 Path=Text}">
 …
</ListView>
```

Vorausgesetzt, `listView`des `ItemsSource` ist eine Liste von Zeichenfolgen, `SomeLabel` hat die Texteigenschaft gebunden, die `SelectedItem`.

## <a name="related-links"></a>Verwandte Links

- [Zwei Wege-Bindung (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/SwitchEntryTwoBinding)
