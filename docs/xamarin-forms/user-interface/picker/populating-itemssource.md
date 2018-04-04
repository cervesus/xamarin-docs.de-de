---
title: Festlegen einer Auswahl ItemsSource-Eigenschaft
description: Die Auswahl einer Ansicht ist ein Steuerelement für die Auswahl eines Text aus einer Liste von Daten. In diesem Artikel wird erläutert, wie Sie eine Auswahl mit Daten auffüllen, indem Sie die ItemsSource-Eigenschaft festlegen, und reagieren auf die Auswahl in einem vom Benutzer.
ms.prod: xamarin
ms.assetid: 8ECF390C-9DB2-4441-B9A3-101AE7E5AEC5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/11/2017
ms.openlocfilehash: bf3940bc1bc0318bad4d785388f9dc9292af80ca
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="setting-a-pickers-itemssource-property"></a>Festlegen einer Auswahl ItemsSource-Eigenschaft

_Die Auswahl einer Ansicht ist ein Steuerelement für die Auswahl eines Text aus einer Liste von Daten. In diesem Artikel wird erläutert, wie Sie eine Auswahl mit Daten auffüllen, indem Sie die ItemsSource-Eigenschaft festlegen, und reagieren auf die Auswahl in einem vom Benutzer._

Xamarin.Forms 2.3.4 wurde verbessert, sodass die [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) Ansicht durch Hinzufügen der Funktion, um es mit Daten auffüllen, indem Sie festlegen seiner [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) -Eigenschaft, und um das ausgewählte Element aus der abzurufen[ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) Eigenschaft. Darüber hinaus kann die Farbe des Texts für das ausgewählte Element geändert werden, indem die [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.TextColor/) Eigenschaft, um eine [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/).

## <a name="populating-a-picker-with-data"></a>Eine Auswahl mit Daten auffüllen

Ein [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) mit Daten aufgefüllt werden kann, durch Festlegen seiner [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) Eigenschaft, um eine `IList` Auflistung. Jedes Element in der Auflistung sein muss, oder geben Sie die abgeleitet, `object`. Elemente können in XAML hinzugefügt werden, durch die Initialisierung der `ItemsSource` Eigenschaft aus einem Array von Elementen:

```xaml
<Picker x:Name="picker" Title="Select a monkey">
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
> Beachten Sie, dass die `x:Array` Element erfordert eine `Type` Attribut, der angibt, der Typ der Elemente im Array.

Die entsprechende C#-Code wird unten gezeigt:

```csharp
var monkeyList = new List<string>();
monkeyList.Add("Baboon");
monkeyList.Add("Capuchin Monkey");
monkeyList.Add("Blue Monkey");
monkeyList.Add("Squirrel Monkey");
monkeyList.Add("Golden Lion Tamarin");
monkeyList.Add("Howler Monkey");
monkeyList.Add("Japanese Macaque");

var picker = new Picker { Title = "Select a monkey" };
picker.ItemsSource = monkeyList;
```

## <a name="responding-to-item-selection"></a>Reagieren auf Elementauswahl

Ein [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) Auswahl eines Elements zu einem Zeitpunkt unterstützt. Wenn ein Benutzer ein Element auswählt der [ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/) Ereignis wird ausgelöst, die [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) Eigenschaft wird auf eine ganze Zahl, die den Index des ausgewählten Elements in der Liste aktualisiert und die [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) Eigenschaft wird aktualisiert, um die `object` , die das ausgewählte Element darstellt. Die [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) Eigenschaft ist eine nullbasierte Nummer, der angibt, die vom Benutzer ausgewählten Element. Wenn kein Element ausgewählt ist, dies ist der Fall bei der [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) erstellt und initialisiert, `SelectedIndex` beträgt-1.

> [!NOTE]
> Das Verhalten der Funktionsauswahl im Element ein [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) für iOS mit einem plattformspezifischen angepasst werden kann. Weitere Informationen finden Sie unter [Steuern der Auswahl einer Elementauswahl](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#picker_update_mode).

Im folgenden Codebeispiel wird veranschaulicht, wie zum Abrufen der [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) Eigenschaftswert aus dem [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) in XAML:

```xaml
<Label Text="{Binding Source={x:Reference picker}, Path=SelectedItem}" />
```

Die entsprechende C#-Code wird unten gezeigt:

```csharp
var monkeyNameLabel = new Label();
monkeyNameLabel.SetBinding(Label.TextProperty, new Binding("SelectedItem", source: picker));
```

Darüber hinaus kann ein Ereignishandler ausgeführt wird, wenn die [ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/) -Ereignis ausgelöst:

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

Diese Methode ruft die [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) Eigenschaftswert, und verwendet den Wert zum Abrufen des ausgewählten Elements aus der [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) Auflistung. Dies entspricht funktional dem Abrufen des ausgewählten Elements aus der [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) Eigenschaft. Beachten Sie, dass jedes Element in der `ItemsSource` Auflistung weist den Typ `object`, und daher in umgewandelt werden, muss ein `string` für die Anzeige.

> [!NOTE]
> Ein [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) können initialisiert werden, um ein bestimmtes Element angezeigt, durch Festlegen der [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) oder [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) Eigenschaften. Allerdings müssen diese Eigenschaften festgelegt werden, wenn nach der Initialisierung der [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) Auflistung.

## <a name="populating-a-picker-with-data-using-data-binding"></a>Eine Auswahl unter Verwendung der Datenbindung mit Daten auffüllen

Ein [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) können auch mit Daten gefüllt werden mithilfe der Datenbindung binden seine [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) Eigenschaft, um eine `IList` Auflistung. In XAML Dies erfolgt mit der [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.BindingExtension/) Markuperweiterung:

```xaml
<Picker Title="Select a monkey" ItemsSource="{Binding Monkeys}" ItemDisplayBinding="{Binding Name}" />
```

Die entsprechende C#-Code wird unten gezeigt:

```csharp
var picker = new Picker { Title = "Select a monkey" };
picker.SetBinding(Picker.ItemsSourceProperty, "Monkeys");
picker.ItemDisplayBinding = new Binding("Name");
```

Die [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) Eigenschaftendaten bindet an die `Monkeys` Eigenschaft des Modells verbundene Ansicht, die zurückgibt, eine `IList<Monkey>` Auflistung. Das folgende Codebeispiel zeigt die `Monkey` -Klasse, die vier Eigenschaften enthält:

```csharp
public class Monkey
{
  public string Name { get; set; }
  public string Location { get; set; }
  public string Details { get; set; }
  public string ImageUrl { get; set; }
}
```

Beim Binden an eine Liste von Objekten, die [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) muss für die anzuzeigenden aus jedem Objekt, dessen Eigenschaft mitgeteilt werden. Dies erfolgt durch Festlegen der [ `ItemDisplayBinding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemDisplayBinding/) -Eigenschaft auf die erforderliche Eigenschaft aus jedem Objekt. In den obigen Codebeispielen die `Picker` festgelegt ist, um jeden anzuzeigen `Monkey.Name` Eigenschaftswert.

### <a name="responding-to-item-selection"></a>Reagieren auf Elementauswahl

Die Datenbindung verwendet werden kann, auf ein Objekt festgelegt werden soll die [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) Eigenschaftswert ändert:

```xaml
<Picker Title="Select a monkey"
        ItemsSource="{Binding Monkeys}"
        ItemDisplayBinding="{Binding Name}"
        SelectedItem="{Binding SelectedMonkey}" />
<Label Text="{Binding SelectedMonkey.Name}" ... />
<Label Text="{Binding SelectedMonkey.Location}" ... />
<Image Source="{Binding SelectedMonkey.ImageUrl}" ... />
<Label Text="{Binding SelectedMonkey.Details}" ... />
```

Die entsprechende C#-Code wird unten gezeigt:

```csharp
var picker = new Picker { Title = "Select a monkey" };
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

Die [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) Eigenschaftendaten bindet an die `SelectedMonkey` Eigenschaft von der verbundenen Ansichtsmodell, in dem Typ `Monkey`. Daher, wenn der Benutzer wählt ein Element in der [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/), `SelectedMonkey` -Eigenschaft wird festgelegt, die dem ausgewählten `Monkey` Objekt. Die `SelectedMonkey` Objektdaten werden angezeigt, in der Benutzeroberfläche von [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) und [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) Ansichten:

![](populating-itemssource-images/monkeys.png "Auswahl einer Elementauswahl")

> [!NOTE]
> Beachten Sie, dass die [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedItem/) und [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) Eigenschaften beide unterstützen bidirektionale Bindungen standardmäßig.

## <a name="summary"></a>Zusammenfassung

Die [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) Ansicht ist ein Steuerelement für die Auswahl eines Text aus einer Liste von Daten. In diesem Artikel wird erläutert, wie zum Auffüllen einer `Picker` mit Daten durch Festlegen der [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) -Eigenschaft, und reagieren auf die Auswahl in einem vom Benutzer. Dieser Ansatz, der in Xamarin.Forms 2.3.4 eingeführt wurde, ist die empfohlene Vorgehensweise für die Interaktion mit einem `Picker`.


## <a name="related-links"></a>Verwandte Links

- [Auswahl einer Demo (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PickerDemo/)
- [Affe App (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/MonkeyAppPicker/)
- [Bindbare Zeitauswahl (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BindablePicker/)
- [Auswahl](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)
