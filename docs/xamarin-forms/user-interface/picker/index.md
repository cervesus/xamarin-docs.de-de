---
title: Datumsauswahl
description: Die Auswahl einer Ansicht ist ein Steuerelement für die Auswahl eines Text aus einer Liste von Daten.
ms.prod: xamarin
ms.assetid: D4815A4B-104B-4294-951B-BD8F2EC33C86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/04/2018
ms.openlocfilehash: 7f0050351ca28d7f8afeb82a85e82e51d399824b
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847497"
---
# <a name="picker"></a>Datumsauswahl

_Die Auswahl einer Ansicht ist ein Steuerelement für die Auswahl eines Text aus einer Liste von Daten._

Der Xamarin.Forms [ `Picker` ](xref:Xamarin.Forms.Picker) zeigt eine kurze Liste mit Elementen, in dem der Benutzer ein Element auswählen kann. `Picker` werden acht Eigenschaften definiert:

- [`Title`](xref:Xamarin.Forms.Picker.Title) Der Typ `string`, die standardmäßig die `null`.
- [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) Der Typ `IList`, der Quellliste Elemente angezeigt, deren Standard `null`.
- [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) Der Typ `int`, den Index des ausgewählten Elements, standardmäßig auf-1 festgelegt ist.
- [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) Der Typ `object`, das ausgewählte Element, dessen Standard `null`.
- [`TextColor`](xref:Xamarin.Forms.Picker.TextColor) Der Typ [ `Color` ](xref:Xamarin.Forms.Color), die Farbe, mit der Text angezeigt, deren Standard [ `Color.Default` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Default/).
- [`FontAttributes`](xref:Xamarin.Forms.Picker.FontAttributes) Der Typ [ `FontAttributes` ](xref:Xamarin.Forms.FontAttributes), die standardmäßig die [ `FontAtributes.None` ](xref:Xamarin.Forms.FontAttributes.None).
- [`FontFamily`](xref:Xamarin.Forms.Picker.FontFamily) Der Typ `string`, die standardmäßig die `null`.
- [`FontSize`](xref:Xamarin.Forms.Picker.FontSize) Der Typ `double`, die standardmäßig die -1.0.

Alle acht Eigenschaften werden durch gestützt [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) Objekte, was bedeutet, dass sie die formatiert werden können, und die Eigenschaften können Ziele von datenbindungen. Die [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) und [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) Eigenschaften verfügen über eine Bindung Standardmodus [ `BindingMode.TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay), was bedeutet, dass sie die Ziele der datenbindungen werden können in einer Anwendung, verwendet der [Model-View-ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) Architektur. Informationen zum Festlegen von Schriftarteigenschaften finden Sie unter [Schriftarten](~/xamarin-forms/user-interface/text/fonts.md).

Ein [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) keine Daten angezeigt, wenn er erstmals angezeigt wird. Stattdessen den Wert des seine [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Title/) Eigenschaft wird als Platzhalter für IOS- und Android-Plattformen dargestellt:

[![](images/picker-initial.png "Erste Auswahl einer Anzeige")](images/picker-initial-large.png#lightbox "erste Auswahl anzeigen")

Wenn die [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) Gewinne Fokus, seine Daten angezeigt wird und der Benutzer ein Element auswählen kann:

[![](images/picker-selection.png "Auswählen eines Elements Datumsauswahl")](images/picker-selection-large.png#lightbox "Datumsauswahl Auswählen eines Elements")

Die [ `Picker` ](xref:Xamarin.Forms.Picker) ausgelöst wird eine [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) Ereignis aus, wenn der Benutzer wählt ein Element aus. Nach Auswahl des ausgewählten Elements wird angezeigt, die `Picker`:

![](images/picker-after-selection.png "Auswahl nach der Auswahl")

Es gibt zwei Verfahren zum Auffüllen einer [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) mit Daten:

- Festlegen der [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) Eigenschaft, um die Daten angezeigt werden. Dies ist das empfohlene Verfahren, die in Xamarin.Forms 2.3.4 eingeführt wurde. Weitere Informationen finden Sie unter [Festlegen einer Auswahl ItemsSource Eigenschaft](populating-itemssource.md).
- Hinzufügen von Daten angezeigt werden die [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) Auflistung. Diese Technik wurde der ursprüngliche Prozess für das Auffüllen einer [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) mit Daten. Weitere Informationen finden Sie unter [Hinzufügen von Daten an eine Datumsauswahl Items-Auflistung](populating-items.md).

## <a name="related-links"></a>Verwandte Links

- [Auswahl](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)
