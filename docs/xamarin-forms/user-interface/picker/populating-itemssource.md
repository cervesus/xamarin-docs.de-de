---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8c4fc732082a77a2e471465af448a487862b513c
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136291"
---
# <a name="setting-a-pickers-itemssource-property"></a>Festlegen der ItemsSource-Eigenschaft einer Auswahl

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-monkeyapppicker)

_Die Auswahl Ansicht ist ein Steuerelement zum Auswählen eines Text Elements aus einer Datenliste. In diesem Artikel wird erläutert, wie eine Auswahl mit Daten aufgefüllt wird, indem die ItemsSource-Eigenschaft festgelegt wird und wie auf die Elementauswahl durch den Benutzer reagiert wird._

Xamarin.Forms2.3.4 hat die [`Picker`](xref:Xamarin.Forms.Picker) Ansicht verbessert, indem die Möglichkeit hinzugefügt wurde, Sie mit Daten aufzufüllen, indem die zugehörige [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) -Eigenschaft festgelegt und das ausgewählte Element aus der-Eigenschaft abgerufen wird [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) . Außerdem kann die Farbe des Texts für das ausgewählte Element geändert werden, indem die- [`TextColor`](xref:Xamarin.Forms.Picker.TextColor) Eigenschaft auf festgelegt wird [`Color`](xref:Xamarin.Forms.Color) .

## <a name="populating-a-picker-with-data"></a>Auffüllen einer Auswahl mit Daten

Ein [`Picker`](xref:Xamarin.Forms.Picker) kann mit Daten aufgefüllt werden, indem seine- [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) Eigenschaft auf eine-Auflistung festgelegt wird `IList` . Jedes Element in der Auflistung muss vom Typ sein oder davon abgeleitet sein `object` . Elemente können in XAML hinzugefügt werden, indem die- `ItemsSource` Eigenschaft aus einem Array von Elementen initialisiert wird:

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
> Bedenken Sie, dass Sie für das Element `x:Array` ein `Type`-Attribute benötigen, das die Elementtypen im Array angibt.

Der entsprechende c#-Code wird unten dargestellt:

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

Ein [`Picker`](xref:Xamarin.Forms.Picker) unterstützt jeweils nur ein Element. Wenn ein Benutzer ein Element auswählt, wird das-Ereignis ausgelöst. [`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged) die- [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) Eigenschaft wird auf eine ganze Zahl aktualisiert, die den Index des ausgewählten Elements in der Liste darstellt, und die- [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) Eigenschaft wird auf den aktualisiert, der `object` das ausgewählte Element darstellt. Die [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) -Eigenschaft ist eine Null basierte Zahl, die das Element angibt, das der Benutzer ausgewählt hat. Wenn kein Element ausgewählt ist (Dies ist der Fall, wenn das [`Picker`](xref:Xamarin.Forms.Picker) erstmalig erstellt und initialisiert wird), hat den Wert `SelectedIndex` -1.

> [!NOTE]
> Das Elementauswahl Verhalten in einem [`Picker`](xref:Xamarin.Forms.Picker) kann unter IOS mit einem plattformspezifischen angepasst werden. Weitere Informationen finden Sie unter [Steuern der Auswahl des Auswahl Elements](~/xamarin-forms/platform/ios/picker-selection.md).

Im folgenden Codebeispiel wird gezeigt, wie der- [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) Eigenschafts Wert aus [`Picker`](xref:Xamarin.Forms.Picker) in XAML abgerufen wird:

```xaml
<Label Text="{Binding Source={x:Reference picker}, Path=SelectedItem}" />
```

Der entsprechende c#-Code wird unten dargestellt:

```csharp
var monkeyNameLabel = new Label();
monkeyNameLabel.SetBinding(Label.TextProperty, new Binding("SelectedItem", source: picker));
```

Außerdem kann ein Ereignishandler ausgeführt werden, wenn das Ereignis ausgelöst wird [`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged) :

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

Diese Methode ruft den [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) -Eigenschafts Wert ab und verwendet den-Wert, um das ausgewählte Element aus der Auflistung abzurufen [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) . Dies ist funktional äquivalent zum Abrufen des ausgewählten Elements aus der- [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) Eigenschaft. Beachten Sie, dass jedes Element in der Auflistung `ItemsSource` vom Typ ist `object` und daher in einen für die Anzeige umgewandelt werden muss `string` .

> [!NOTE]
> Ein [`Picker`](xref:Xamarin.Forms.Picker) kann initialisiert werden, um ein bestimmtes Element durch Festlegen der-Eigenschaft oder der-Eigenschaft anzuzeigen [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) . Diese Eigenschaften müssen jedoch nach dem Initialisieren der Auflistung festgelegt werden [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) .

## <a name="populating-a-picker-with-data-using-data-binding"></a>Auffüllen einer Auswahl mit Daten mithilfe der Datenbindung

Ein [`Picker`](xref:Xamarin.Forms.Picker) kann auch mit Daten aufgefüllt werden, indem die-Eigenschaft mithilfe der Datenbindung an eine-Auflistung gebunden wird [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) `IList` . In XAML wird dies mit der [`Binding`](xref:Xamarin.Forms.Xaml.BindingExtension) Markup Erweiterung erreicht:

```xaml
<Picker Title="Select a monkey"
        TitleColor="Red"
        ItemsSource="{Binding Monkeys}"
        ItemDisplayBinding="{Binding Name}" />
```

Der entsprechende c#-Code wird unten dargestellt:

```csharp
var picker = new Picker { Title = "Select a monkey", TitleColor = Color.Red };
picker.SetBinding(Picker.ItemsSourceProperty, "Monkeys");
picker.ItemDisplayBinding = new Binding("Name");
```

Die [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) Eigenschaften Daten werden an die- `Monkeys` Eigenschaft des verbundenen Ansichts Modells gebunden, das eine-Auflistung zurückgibt `IList<Monkey>` . Das folgende Codebeispiel zeigt die- `Monkey` Klasse, die vier Eigenschaften enthält:

```csharp
public class Monkey
{
  public string Name { get; set; }
  public string Location { get; set; }
  public string Details { get; set; }
  public string ImageUrl { get; set; }
}
```

Beim Binden an eine Liste von Objekten muss der [`Picker`](xref:Xamarin.Forms.Picker) mitgeteilt werden, welche Eigenschaft aus den einzelnen Objekten angezeigt werden soll. Dies wird erreicht, indem die- [`ItemDisplayBinding`](xref:Xamarin.Forms.Picker.ItemDisplayBinding) Eigenschaft für jedes-Objekt auf die erforderliche Eigenschaft festgelegt wird. In den obigen Codebeispielen `Picker` wird festgelegt, dass jeder `Monkey.Name` Eigenschafts Wert angezeigt wird.

### <a name="responding-to-item-selection"></a>Antworten auf die Elementauswahl

Die Datenbindung kann verwendet werden, um ein Objekt auf den [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) Eigenschafts Wert festzulegen, wenn es geändert wird:

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

Der entsprechende c#-Code wird unten dargestellt:

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

Die [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) Eigenschaften Daten werden an die- `SelectedMonkey` Eigenschaft des verbundenen Ansichts Modells gebunden, das vom Typ ist `Monkey` . Daher [`Picker`](xref:Xamarin.Forms.Picker) wird die- `SelectedMonkey` Eigenschaft auf das ausgewählte Objekt festgelegt, wenn der Benutzer ein Element im auswählt `Monkey` . Die `SelectedMonkey` Objektdaten werden in der Benutzeroberfläche durch die [`Label`](xref:Xamarin.Forms.Label) Ansichten und angezeigt [`Image`](xref:Xamarin.Forms.Image) :

![](populating-itemssource-images/monkeys.png "Picker Item Selection")

> [!NOTE]
> Beachten Sie, dass die [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) Eigenschaften und standardmäßig bidirektionale [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) Bindungen unterstützen.

## <a name="related-links"></a>Verwandte Links

- [Auswahl Demo (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-pickerdemo)
- [Affenapp (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-monkeyapppicker)
- [Bindbare Auswahl (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablepicker)
- [Auswahl-API](xref:Xamarin.Forms.Picker)
