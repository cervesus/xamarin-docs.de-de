---
title: Hinzufügen von Daten zur Elementsammlung einer Auswahl
description: Die Auswahl-Ansicht ist, ein Steuerelement für ein Element mit Text aus einer Liste von Daten ausgewählt wird. In diesem Artikel wird erläutert, wie eine Auswahl mit Daten aufgefüllt wird, indem sie Sie der Auflistung Elemente hinzufügen und zum Reagieren auf Auswahl durch den Benutzer.
ms.prod: xamarin
ms.assetid: 3C840F64-A430-457D-A4B2-3D7AF46F9DBE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/26/2019
ms.openlocfilehash: 3bbea036efef44077ccbd28a16af06c97cd7026b
ms.sourcegitcommit: 00744f754527e5b55154365f89691caaf1c9d929
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2019
ms.locfileid: "57557229"
---
# <a name="adding-data-to-a-pickers-items-collection"></a>Hinzufügen von Daten zur Elementsammlung einer Auswahl

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PickerDemo/)

_Die Auswahl-Ansicht ist, ein Steuerelement für ein Element mit Text aus einer Liste von Daten ausgewählt wird. In diesem Artikel wird erläutert, wie eine Auswahl mit Daten aufgefüllt wird, indem sie Sie der Auflistung Elemente hinzufügen und zum Reagieren auf Auswahl durch den Benutzer._

## <a name="populating-a-picker-with-data"></a>Eine Auswahl mit Daten auffüllen

Vor dem Xamarin.Forms 2.3.4, den Prozess für das Auffüllen einer [ `Picker` ](xref:Xamarin.Forms.Picker) mit Daten wurde zum Hinzufügen von den Daten, die schreibgeschützt angezeigt wird [ `Items` ](xref:Xamarin.Forms.Picker.Items) -Auflistung, die vom Typ `IList<string>`. Jedes Element in der Auflistung muss vom Typ `string`. Elemente können in XAML hinzugefügt werden, durch die Initialisierung der `Items` Eigenschaft mit einer Liste von `x:String` Elemente:

```xaml
<Picker Title="Select a monkey"
        TitleColor="Red">
  <Picker.Items>
    <x:String>Baboon</x:String>
    <x:String>Capuchin Monkey</x:String>
    <x:String>Blue Monkey</x:String>
    <x:String>Squirrel Monkey</x:String>
    <x:String>Golden Lion Tamarin</x:String>
    <x:String>Howler Monkey</x:String>
    <x:String>Japanese Macaque</x:String>
  </Picker.Items>
</Picker>
```

Der entsprechende C#-Code wird unten gezeigt:

```csharp
var picker = new Picker { Title = "Select a monkey", TitleColor = Color.Red };
picker.Items.Add("Baboon");
picker.Items.Add("Capuchin Monkey");
picker.Items.Add("Blue Monkey");
picker.Items.Add("Squirrel Monkey");
picker.Items.Add("Golden Lion Tamarin");
picker.Items.Add("Howler Monkey");
picker.Items.Add("Japanese Macaque");
```

Zusätzlich zum Hinzufügen von Daten mithilfe der `Items.Add` -Methode, Daten können auch in die Auflistung eingefügt werden, mithilfe der `Items.Insert` Methode.

## <a name="responding-to-item-selection"></a>Reagieren auf die Auswahl von Listenelementen

Ein [ `Picker` ](xref:Xamarin.Forms.Picker) unterstützt die Auswahl eines Elements zu einem Zeitpunkt. Wenn ein Benutzer ein Element auswählt der [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) Ereignis wird ausgelöst, und die [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) Eigenschaft eine ganze Zahl fest, der den Index des ausgewählten Elements in der Liste aktualisiert wird. Die `SelectedIndex` -Eigenschaft ist eine nullbasierte Nummer, der angibt, des Elements, das der Benutzer ausgewählt. Wenn kein Element ausgewählt ist, was der Fall ist bei der `Picker` zuerst erstellt und initialisiert, `SelectedIndex` beträgt-1.

> [!NOTE]
> Das Auswahlverhalten im Element eine [ `Picker` ](xref:Xamarin.Forms.Picker) kann auf iOS mit einer plattformspezifischen angepasst werden. Weitere Informationen finden Sie unter [Auswahl Elementauswahl steuern](~/xamarin-forms/platform/ios/picker-selection.md).

Das folgende Codebeispiel zeigt die `OnPickerSelectedIndexChanged` -Ereignishandlermethode handelt es sich ausgeführt, wenn die [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) -Ereignis ausgelöst wird:

```csharp
void OnPickerSelectedIndexChanged(object sender, EventArgs e)
{
  var picker = (Picker)sender;
  int selectedIndex = picker.SelectedIndex;

  if (selectedIndex != -1)
  {
    monkeyNameLabel.Text = picker.Items[selectedIndex];
  }
}
```

Diese Methode ruft die [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) Eigenschaftswert, und verwendet den Wert zum Abrufen des ausgewählten Elements aus der [ `Items` ](xref:Xamarin.Forms.Picker.Items) Auflistung. Da jedes Element der `Items` Auflistung ist eine `string`, sie können angezeigt werden, indem eine [ `Label` ](xref:Xamarin.Forms.Label) ohne eine Umwandlung.

> [!NOTE]
> Ein [ `Picker` ](xref:Xamarin.Forms.Picker) können initialisiert werden, um ein bestimmtes Element anzeigen, durch Festlegen der [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) Eigenschaft. Allerdings die `SelectedIndex` Eigenschaft muss festgelegt werden, nach der Initialisierung der [ `Items` ](xref:Xamarin.Forms.Picker.Items) Auflistung.

## <a name="related-links"></a>Verwandte Links

- [Auswahl-Demo (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PickerDemo/)
- [Auswahl](xref:Xamarin.Forms.Picker)
