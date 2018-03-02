---
title: ListView-Datenquellen
description: "Erfahren Sie, wie Ihre ListView mit Daten aufzufüllen."
ms.topic: article
ms.prod: xamarin
ms.assetid: B5571660-1E82-4379-95C3-0725288CF5D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 78659aff0b6b4e6401bec96a55935c141d1199f1
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="listview-data-sources"></a>ListView-Datenquellen

ListView dient zum Anzeigen von Listen mit Daten. Wir erfahren zum Auffüllen einer ListView mit Daten und wie es in das ausgewählte Element binden können.

- **[Festlegen von ItemsSource](#ItemsSource)**  &ndash; verwendet eine einfache Liste oder ein Array.
- **[Die Datenbindung](#Data_Binding)**  &ndash; legt eine Beziehung zwischen einem Modell und die ListView. Bindung ist ideal für das MVVM-Muster.

## <a name="itemssource"></a>ItemsSource
ListView mit Daten aufgefüllt ist die `ItemsSource` -Eigenschaft, die eine beliebige Auflistung implementieren akzeptieren kann `IEnumerable`. Die einfachste Methode zum Auffüllen einer `ListView` , müssen Sie ein Array von Zeichenfolgen mit:

```csharp
var listView = new ListView();
listView.ItemsSource = new string[]{
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

Der oben beschriebenen Herangehensweise füllt die `ListView` mit einer Liste von Zeichenfolgen. Standardmäßig `ListView` angerufen `ToString` und Anzeigen des Ergebnisses in einer `TextCell` für jede Zeile. Um anzupassen, wie Daten angezeigt werden, finden Sie unter [Darstellung Zelle](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md).

Da `ItemsSource` gesendet wurde in ein Array, der Inhalt werden nicht aktualisiert, wenn sich die zugrunde liegende Liste oder Array ändert. Wenn die ListView automatisch zu aktualisieren, wenn Elemente hinzugefügt, entfernt und in der zugrunde liegenden Liste geändert werden soll, müssen Sie verwenden eine `ObservableCollection`. [`ObservableCollection`](https://developer.xamarin.com/api/type/System.Collections.ObjectModel.ObservableCollection%3CT%3E/) wird definiert, `System.Collections.ObjectModel` und ist ebenso wie `List`, außer dass sie benachrichtigen kann `ListView` über Änderungen vorgenommen:

```csharp
ObservableCollection<Employees> employeeList = new ObservableCollection<Employess>();
listView.ItemsSource = employeeList;

//Mr. Mono will be added to the ListView because it uses an ObservableCollection
employeeList.Add(new Employee(){ DisplayName="Mr. Mono"});
```

<a name="Data_Binding" />

## <a name="data-binding"></a>Datenbindung
Datenbindung ist der "Kleben", die die Eigenschaften eines Benutzerobjekts-Schnittstelle auf die Eigenschaften eines ein CLR-Objekt, z. B. eine Klasse in Ihrem ViewModel gebunden. Datenbindung ist hilfreich, da die Entwicklung von Benutzeroberflächen vereinfacht durch viele langweilig Standardcode zu ersetzen.

Die Datenbindung funktioniert, indem Synchronisieren von Objekten wie deren gebundenen Werte zu ändern. Anstatt Sie Ereignishandler für schreiben, jedes Mal, wenn der Wert eines Steuerelements ändert, die Bindung herstellen und Bindung in Ihre ViewModel aktivieren.

Weitere Informationen über die Datenbindung finden Sie unter [Data Binding-Grundlagen](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md) also Teil 4 von der [Grundlagen der Verwendung von XAML-Xamarin.Forms Artikel Reihe](~/xamarin-forms/xaml/xaml-basics/index.md).

### <a name="binding-cells"></a>Binden von Zellen
Eigenschaften von Zellen (und untergeordnete Elemente von Zellen) gebunden werden können, um Eigenschaften von Objekten in der `ItemsSource`. Beispielsweise konnte eine ListView verwendet werden, um eine Liste der Mitarbeiter mit Bildern zu präsentieren.

Die Klasse für Mitarbeiter:

```csharp
public class Employee{
    public string DisplayName {get; set;}
}
```

`ObservableCollection<Employee>` wird erstellt, und legen Sie als die `ListView`des `ItemsSource`:

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

Beachten Sie, dass die Bindung im Code der Einfachheit halber eingerichtet wurde, obwohl in XAML gebunden wurden konnte.

Das vorherige Bit von XAML definiert eine `ContentPage` , enthält eine `ListView`. Die Datenquelle der `ListView` festgelegt ist, über die `ItemsSource` Attribut. Das Layout der jede Zeile in der `ItemsSource`wird definiert, in der `ListView.ItemTemplate` Element.

Dies ist das Ergebnis:

![](data-and-databinding-images/bound-data.png "ListView mit einer Datenbindung")

### <a name="binding-selecteditem"></a>Bindung "SelectedItem"

Häufig sollten zum Binden an das ausgewählte Element von einem `ListView`, sondern als einen Ereignishandler zu verwenden, um auf Änderungen reagieren. Zu diesem Zweck in XAML binden die `SelectedItem` Eigenschaft:

```xaml
<ListView x:Name="listView"
 SelectedItem="{Binding Source={x:Reference SomeLabel},
 Path=Text}">
 …
</ListView>
```

Vorausgesetzt `listView`des `ItemsSource` ist eine Liste von Zeichenfolgen, `SomeLabel` müssen Sie seine Texteigenschaft gebunden werden, um die `SelectedItem`.



## <a name="related-links"></a>Verwandte Links

- [Zwei Weise binden (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/SwitchEntryTwoBinding)
- [Anmerkungen zu dieser Version 1.4](http://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [Anmerkungen zu dieser Version 1.3](http://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
