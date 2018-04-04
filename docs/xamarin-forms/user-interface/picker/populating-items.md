---
title: Hinzufügen von Daten an eine Datumsauswahl Items-Auflistung
description: Die Auswahl einer Ansicht ist ein Steuerelement für die Auswahl eines Text aus einer Liste von Daten. In diesem Artikel wird erläutert, wie Sie eine Auswahl mit Daten auffüllen, indem Sie der Items-Auflistung hinzugefügt, und reagieren auf die Auswahl in einem vom Benutzer.
ms.prod: xamarin
ms.assetid: 3C840F64-A430-457D-A4B2-3D7AF46F9DBE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/11/2017
ms.openlocfilehash: 63a72861895f79d2d0154297f88610ddb8bb8beb
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="adding-data-to-a-pickers-items-collection"></a>Hinzufügen von Daten an eine Datumsauswahl Items-Auflistung

_Die Auswahl einer Ansicht ist ein Steuerelement für die Auswahl eines Text aus einer Liste von Daten. In diesem Artikel wird erläutert, wie Sie eine Auswahl mit Daten auffüllen, indem Sie der Items-Auflistung hinzugefügt, und reagieren auf die Auswahl in einem vom Benutzer._

## <a name="populating-a-picker-with-data"></a>Eine Auswahl mit Daten auffüllen

Vor dem Xamarin.Forms 2.3.4, den Prozess zum Auffüllen einer [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) mit Daten wurde zum Hinzufügen von den Daten, die schreibgeschützt angezeigt wird [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) Collection, die vom Typ ist`IList<string>`. Jedes Element in der Auflistung muss vom Typ `string`. Elemente können in XAML hinzugefügt werden, durch die Initialisierung der `Items` Eigenschaft mit einer Liste von `x:String` Elemente:

```xaml
<Picker Title="Select a monkey">
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

Die entsprechende C#-Code wird unten gezeigt:

```csharp
var picker = new Picker { Title = "Select a monkey" };
picker.Items.Add("Baboon");
picker.Items.Add("Capuchin Monkey");
picker.Items.Add("Blue Monkey");
picker.Items.Add("Squirrel Monkey");
picker.Items.Add("Golden Lion Tamarin");
picker.Items.Add("Howler Monkey");
picker.Items.Add("Japanese Macaque");
```

Zusätzlich zum Hinzufügen von Daten mithilfe der `Items.Add` -Methode, Daten können auch in die Auflistung eingefügt werden, mithilfe der `Items.Insert` Methode.

## <a name="responding-to-item-selection"></a>Reagieren auf Elementauswahl

Ein [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) Auswahl eines Elements zu einem Zeitpunkt unterstützt. Wenn ein Benutzer ein Element auswählt der [ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/) Ereignis wird ausgelöst, und die [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) Eigenschaft wird aktualisiert, um eine ganze Zahl, die den Index des ausgewählten Elements in der Liste darstellt. Die `SelectedIndex` Eigenschaft ist eine nullbasierte Nummer, der angibt, des Elements, das der Benutzer ausgewählt. Wenn kein Element ausgewählt ist, dies ist der Fall bei der `Picker` erstellt und initialisiert, `SelectedIndex` beträgt-1.

> [!NOTE]
> Das Verhalten der Funktionsauswahl im Element ein [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) für iOS mit einem plattformspezifischen angepasst werden kann. Weitere Informationen finden Sie unter [Steuern der Auswahl einer Elementauswahl](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#picker_update_mode).

Das folgende Codebeispiel zeigt die `OnPickerSelectedIndexChanged` Ereignishandlermethode, die ausgeführt wird, wenn die [ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/) -Ereignis ausgelöst:

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

Diese Methode ruft die [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) Eigenschaftswert, und verwendet den Wert zum Abrufen des ausgewählten Elements aus der [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) Auflistung. Da jedes Element in der `Items` Auflistung ist eine `string`, sie können angezeigt werden, indem eine [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) ohne eine Umwandlung.

> [!NOTE]
> Ein [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) können initialisiert werden, um ein bestimmtes Element angezeigt, durch Festlegen der [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) Eigenschaft. Allerdings die `SelectedIndex` Eigenschaft muss festgelegt werden, wenn nach der Initialisierung der [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) Auflistung.

## <a name="summary"></a>Zusammenfassung

Die [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) Ansicht ist ein Steuerelement für die Auswahl eines Text aus einer Liste von Daten. In diesem Artikel wird erläutert, wie zum Auffüllen einer `Picker` mit Daten, indem sie zum Hinzufügen der [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) Auflistung und reagieren auf die Auswahl in einem vom Benutzer. Dadurch wurde der Prozess für die Verwendung einer `Picker` vor Xamarin.Forms 2.3.4.


## <a name="related-links"></a>Verwandte Links

- [Auswahl einer Demo (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PickerDemo/)
- [Auswahl](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)
