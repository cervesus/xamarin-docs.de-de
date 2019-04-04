---
title: Xamarin.Forms-Auswahl
description: Die Xamarin.Forms-Auswahl zeigt eine kurze Liste der Elemente, von denen der Benutzer ein Element auswählen kann. In diesem Artikel wird erläutert, wie Sie die Auswahl-Klasse verwenden, wählen Sie ein Element mit Text aus einer Liste von Daten.
ms.prod: xamarin
ms.assetid: D4815A4B-104B-4294-951B-BD8F2EC33C86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/26/2019
ms.openlocfilehash: dc39fd9c129fb63fa4a3a73b15aea4204a38cdbd
ms.sourcegitcommit: 00744f754527e5b55154365f89691caaf1c9d929
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2019
ms.locfileid: "57557268"
---
# <a name="xamarinforms-picker"></a>Xamarin.Forms-Auswahl

_Die Auswahl-Ansicht ist, ein Steuerelement für ein Element mit Text aus einer Liste von Daten ausgewählt wird._

Die Xamarin.Forms [ `Picker` ](xref:Xamarin.Forms.Picker) zeigt eine kurze Liste der Elemente, von dem der Benutzer ein Element auswählen kann. `Picker` definiert die folgenden Eigenschaften an:

- [`Title`](xref:Xamarin.Forms.Picker.Title) Der Typ `string`, die standardmäßig auf `null`.
- `TitleColor` Der Typ [ `Color` ](xref:Xamarin.Forms.Color), die Farbe zum Anzeigen der `Title` Text.
- [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) Der Typ `IList`, der Quellliste der Elemente angezeigt werden, deren Standard `null`.
- [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) Der Typ `int`, den Index des ausgewählten Elements, dem standardmäßig auf-1 fest.
- [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) Der Typ `object`, das ausgewählte Element, dessen Standard `null`.
- [`TextColor`](xref:Xamarin.Forms.Picker.TextColor) Der Typ [ `Color` ](xref:Xamarin.Forms.Color), die Farbe, mit der Text angezeigt, deren Standard [ `Color.Default` ](xref:Xamarin.Forms.Color.Default).
- [`FontAttributes`](xref:Xamarin.Forms.Picker.FontAttributes) Der Typ [ `FontAttributes` ](xref:Xamarin.Forms.FontAttributes), die standardmäßig auf [ `FontAtributes.None` ](xref:Xamarin.Forms.FontAttributes.None).
- [`FontFamily`](xref:Xamarin.Forms.Picker.FontFamily) Der Typ `string`, die standardmäßig auf `null`.
- [`FontSize`](xref:Xamarin.Forms.Picker.FontSize) Der Typ `double`, die standardmäßig auf den Bereich von -1,0.

Alle Eigenschaften verfügen über [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) -Objekte, die bedeutet, dass sie die formatiert werden können, und die Eigenschaften können Ziele von datenbindungen. Die [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) und [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) Eigenschaften haben einen Standardmodus für die Bindung der [ `BindingMode.TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay), was bedeutet, dass sie Ziele von datenbindungen werden können in einer Anwendung, verwendet der [Model-View-ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) Architektur. Informationen zum Festlegen von Schriftart-Eigenschaften finden Sie unter [Schriftarten](~/xamarin-forms/user-interface/text/fonts.md).

Ein [ `Picker` ](xref:Xamarin.Forms.Picker) keine Daten angezeigt, wenn er zuerst angezeigt wird. Stattdessen der Wert des der [ `Title` ](xref:Xamarin.Forms.Picker.Title) Eigenschaft dient als Platzhalter für die IOS- und Android-Plattformen:

[![](images/picker-initial.png "Ursprüngliche Auswahl anzeigen")](images/picker-initial-large.png#lightbox "erste Auswahl anzeigen")

Wenn die [ `Picker` ](xref:Xamarin.Forms.Picker) erhält den Fokus besitzt, die Daten angezeigt wird, und der Benutzer kann ein Element auswählen:

[![](images/picker-selection.png "Auswählen eines Elements Auswahl")](images/picker-selection-large.png#lightbox "Auswahl, die Auswahl eines Elements")

Die [ `Picker` ](xref:Xamarin.Forms.Picker) löst eine [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) Ereignis aus, wenn der Benutzer ein Element auswählt. Nach der Auswahl wird das ausgewählte Element angezeigt, die `Picker`:

![](images/picker-after-selection.png "Auswahl nach Markierung")

Es gibt zwei Verfahren zum Auffüllen einer [ `Picker` ](xref:Xamarin.Forms.Picker) mit Daten:

- Festlegen der [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) Eigenschaft, um die Daten angezeigt werden. Dies ist das empfohlene Verfahren. Weitere Informationen finden Sie unter [ItemsSource-Eigenschaft einer Auswahl](populating-itemssource.md).
- Die Daten angezeigt werden, Hinzufügen der [ `Items` ](xref:Xamarin.Forms.Picker.Items) Auflistung. Diese Technik wurde der ursprüngliche Prozess für das Auffüllen einer [ `Picker` ](xref:Xamarin.Forms.Picker) mit Daten. Weitere Informationen finden Sie unter [Hinzufügen von Daten mit einer Auswahl Items-Auflistung](populating-items.md).

## <a name="related-links"></a>Verwandte Links

- [Auswahl](xref:Xamarin.Forms.Picker)
