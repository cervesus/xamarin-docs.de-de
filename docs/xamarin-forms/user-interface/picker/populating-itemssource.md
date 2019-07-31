---
title: Festlegen der ItemsSource-Eigenschaft einer Auswahl
description: Die Auswahl-Ansicht ist, ein Steuerelement für ein Element mit Text aus einer Liste von Daten ausgewählt wird. In diesem Artikel wird erläutert, wie eine Auswahl mit Daten aufgefüllt wird, durch Festlegen der ItemsSource-Eigenschaft, und zum Reagieren auf Auswahl durch den Benutzer.
ms.prod: xamarin
ms.assetid: 8ECF390C-9DB2-4441-B9A3-101AE7E5AEC5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/26/2019
ms.openlocfilehash: 803cd1babc32e3a42d957c4bac0cc93c4552fb8c
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656123"
---
# <a name="setting-a-pickers-itemssource-property"></a>Festlegen der ItemsSource-Eigenschaft einer Auswahl

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-monkeyapppicker)

_Die Auswahl-Ansicht ist, ein Steuerelement für ein Element mit Text aus einer Liste von Daten ausgewählt wird. In diesem Artikel wird erläutert, wie eine Auswahl mit Daten aufgefüllt wird, durch Festlegen der ItemsSource-Eigenschaft, und zum Reagieren auf Auswahl durch den Benutzer._

Xamarin.Forms 2.3.4 wurde verbessert, die [ `Picker` ](xref:Xamarin.Forms.Picker) Ansicht durch Hinzufügen der Möglichkeit, die sie mit Daten auffüllen, indem Sie festlegen der [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) -Eigenschaft, und wie Sie das ausgewählte Element aus der Abrufen[ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) Eigenschaft. Darüber hinaus kann die Farbe des Texts für das ausgewählte Element geändert werden, indem die [ `TextColor` ](xref:Xamarin.Forms.Picker.TextColor) Eigenschaft, um eine [ `Color` ](xref:Xamarin.Forms.Color).

## <a name="populating-a-picker-with-data"></a>Auffüllen einer Auswahl mit Daten

Ein [ `Picker` ](xref:Xamarin.Forms.Picker) mit Daten gefüllt werden kann, durch Festlegen seiner [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) Eigenschaft, um eine `IList` Auflistung. Jedes Element in der Auflistung sein muss, oder geben Sie die daraus abgeleitet wurden, `object`. Elemente können in XAML hinzugefügt werden, durch die Initialisierung der `ItemsSource` Eigenschaft aus einem Array von Elementen:

```xaml
<Picker x:Name="picker"
        Title="Select a monkey"
        TitleColor="Red">
  <Picker.ItemsSource>
    <x:Array Type="{x:Type x:String}">
      <x:String>Baboon</x:String>
      <x:String>Capuchin Monkey</x:String>
      <x:String>Blue Monkey</x:String>
      <x:String>Squirrel Monkey</x:String>
      <x:String>Golden Lion Tamarin</x:String>
      <x:String>Howler Monkey</x:String>
      <x:String>Japanese Macaque</x:String>
    </x:Array>
  </Picker.ItemsSource>
</Picker>
```

> [!NOTE]
> Beachten Sie, dass die `x:Array` -Element ist ein `Type` Attribut, der angibt, der des Typs der Elemente im Array.

Der entsprechende C#-Code wird unten gezeigt:

```csharp
var monkeyList = new List<string>();
monkeyList.Add("Baboon");
monkeyList.Add("Capuchin Monkey");
monkeyList.Add("Blue Monkey");
monkeyList.Add("Squirrel Monkey");
monkeyList.Add("Golden Lion Tamarin");
monkeyList.Add("Howler Monkey");
monkeyList.Add("Japanese Macaque");

var picker = new Picker { Title = "Select a monkey", TitleColor = Color.Red };
picker.ItemsSource = monkeyList;
```

## <a name="responding-to-item-selection"></a>Antworten auf die Elementauswahl

Ein [ `Picker` ](xref:Xamarin.Forms.Picker) unterstützt die Auswahl eines Elements zu einem Zeitpunkt. Wenn ein Benutzer ein Element auswählt der [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) Ereignis wird ausgelöst, die [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) Eigenschaft eine ganze Zahl fest, der den Index des ausgewählten Elements in der Liste aktualisiert wird und die [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) Eigenschaft wird aktualisiert, um die `object` , die das ausgewählte Element darstellt. Die [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) -Eigenschaft ist eine nullbasierte Nummer, der angibt, die vom Benutzer ausgewählten Element. Wenn kein Element ausgewählt ist, was der Fall ist bei der [ `Picker` ](xref:Xamarin.Forms.Picker) zuerst erstellt und initialisiert, `SelectedIndex` beträgt-1.

> [!NOTE]
> Das Auswahlverhalten im Element eine [ `Picker` ](xref:Xamarin.Forms.Picker) kann auf iOS mit einer plattformspezifischen angepasst werden. Weitere Informationen finden Sie unter [Auswahl Elementauswahl steuern](~/xamarin-forms/platform/ios/picker-selection.md).

Im folgenden Codebeispiel wird veranschaulicht, wie zum Abrufen der [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) Eigenschaftswert aus dem [ `Picker` ](xref:Xamarin.Forms.Picker) in XAML:

```xaml
<Label Text="{Binding Source={x:Reference picker}, Path=SelectedItem}" />
```

Der entsprechende C#-Code wird unten gezeigt:

```csharp
var monkeyNameLabel = new Label();
monkeyNameLabel.SetBinding(Label.TextProperty, new Binding("SelectedItem", source: picker));
```

Darüber hinaus kann ein Ereignishandler sein ausgeführt, wenn die [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) -Ereignis ausgelöst wird:

```csharp
void OnPickerSelectedIndexChanged(object sender, EventArgs e)
{
  var picker = (Picker)sender;
  int selectedIndex = picker.SelectedIndex;

  if (selectedIndex != -1)
  {
    monkeyNameLabel.Text = (string)picker.ItemsSource[selectedIndex];
  }
}
```

Diese Methode ruft die [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) Eigenschaftswert, und verwendet den Wert zum Abrufen des ausgewählten Elements aus der [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) Auflistung. Dies entspricht funktional dem Abrufen des ausgewählten Elements aus der [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) Eigenschaft. Beachten Sie, dass jedes Element in der `ItemsSource` Auflistung ist vom Typ `object`, und daher in umgewandelt werden, eine `string` für die Anzeige.

> [!NOTE]
> Ein [ `Picker` ](xref:Xamarin.Forms.Picker) können initialisiert werden, um ein bestimmtes Element anzeigen, durch Festlegen der [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) oder [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) Eigenschaften. Allerdings müssen diese Eigenschaften festgelegt werden, nach der Initialisierung der [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) Auflistung.

## <a name="populating-a-picker-with-data-using-data-binding"></a>Auffüllen einer Auswahl mit Daten mithilfe der Datenbindung

Ein [ `Picker` ](xref:Xamarin.Forms.Picker) können auch mit Daten gefüllt werden mithilfe der Datenbindung binden die [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) Eigenschaft, um eine `IList` Auflistung. In XAML erfolgt dies über die [ `Binding` ](xref:Xamarin.Forms.Xaml.BindingExtension) Markuperweiterung:

```xaml
<Picker Title="Select a monkey"
        TitleColor="Red"
        ItemsSource="{Binding Monkeys}"
        ItemDisplayBinding="{Binding Name}" />
```

Der entsprechende C#-Code wird unten gezeigt:

```csharp
var picker = new Picker { Title = "Select a monkey", TitleColor = Color.Red };
picker.SetBinding(Picker.ItemsSourceProperty, "Monkeys");
picker.ItemDisplayBinding = new Binding("Name");
```

Die [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) Eigenschaftendaten bindet an die `Monkeys` Eigenschaft des verbundenen Ansichtsmodell, wodurch ein `IList<Monkey>` Auflistung. Das folgende Codebeispiel zeigt die `Monkey` -Klasse, die vier Eigenschaften enthält:

```csharp
public class Monkey
{
  public string Name { get; set; }
  public string Location { get; set; }
  public string Details { get; set; }
  public string ImageUrl { get; set; }
}
```

Beim Binden an eine Liste von Objekten, die [ `Picker` ](xref:Xamarin.Forms.Picker) muss Ihr mitgeteilt werden, welche Eigenschaft aus jedem Objekt angezeigt. Dies wird erreicht, indem die [ `ItemDisplayBinding` ](xref:Xamarin.Forms.Picker.ItemDisplayBinding) -Eigenschaft auf die erforderliche Eigenschaft aus jedem Objekt. In den obigen Codebeispielen die `Picker` nastaven NA hodnotu jedes `Monkey.Name` -Eigenschaftswert.

### <a name="responding-to-item-selection"></a>Antworten auf die Elementauswahl

Die Datenbindung kann verwendet werden, um ein Objekt festgelegt werden, um die [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) Eigenschaftswert, wenn dieses geändert wird:

```xaml
<Picker Title="Select a monkey"
        TitleColor="Red"
        ItemsSource="{Binding Monkeys}"
        ItemDisplayBinding="{Binding Name}"
        SelectedItem="{Binding SelectedMonkey}" />
<Label Text="{Binding SelectedMonkey.Name}" ... />
<Label Text="{Binding SelectedMonkey.Location}" ... />
<Image Source="{Binding SelectedMonkey.ImageUrl}" ... />
<Label Text="{Binding SelectedMonkey.Details}" ... />
```

Der entsprechende C#-Code wird unten gezeigt:

```csharp
var picker = new Picker { Title = "Select a monkey", TitleColor = Color.Red };
picker.SetBinding(Picker.ItemsSourceProperty, "Monkeys");
picker.SetBinding(Picker.SelectedItemProperty, "SelectedMonkey");
picker.ItemDisplayBinding = new Binding("Name");

var nameLabel = new Label { ... };
nameLabel.SetBinding(Label.TextProperty, "SelectedMonkey.Name");

var locationLabel = new Label { ... };
locationLabel.SetBinding(Label.TextProperty, "SelectedMonkey.Location");

var image = new Image { ... };
image.SetBinding(Image.SourceProperty, "SelectedMonkey.ImageUrl");

var detailsLabel = new Label();
detailsLabel.SetBinding(Label.TextProperty, "SelectedMonkey.Details");
```

Die [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) Eigenschaftendaten bindet an die `SelectedMonkey` Eigenschaft des verbundenen Ansichtsmodell, das vom Typ `Monkey`. Aus diesem Grund, wenn der Benutzer wählt ein Element in der [ `Picker` ](xref:Xamarin.Forms.Picker), `SelectedMonkey` Eigenschaft wird festgelegt, die dem ausgewählten `Monkey` Objekt. Die `SelectedMonkey` Objektdaten werden angezeigt, in der Benutzeroberfläche von [ `Label` ](xref:Xamarin.Forms.Label) und [ `Image` ](xref:Xamarin.Forms.Image) Ansichten:

![](populating-itemssource-images/monkeys.png "Auswahl-Elementauswahl")

> [!NOTE]
> Beachten Sie, dass die [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) und [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) Eigenschaften beide unterstützen bidirektionale Bindungen standardmäßig.

## <a name="related-links"></a>Verwandte Links

- [Auswahl-Demo (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-pickerdemo)
- [Monkey-App (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-monkeyapppicker)
- [Bindbare Auswahl (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablepicker)
- [Auswahl-API](xref:Xamarin.Forms.Picker)
