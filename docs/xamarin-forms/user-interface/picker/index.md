---
title: Xamarin. Forms-Auswahl
description: Die xamarin. Forms-Auswahl zeigt eine kurze Liste von Elementen an, aus der der Benutzer ein Element auswählen kann. In diesem Artikel wird erläutert, wie Sie die Auswahl Klasse verwenden, um ein Textelement aus einer Liste von Daten auszuwählen.
ms.prod: xamarin
ms.assetid: D4815A4B-104B-4294-951B-BD8F2EC33C86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/26/2019
ms.openlocfilehash: c8246f8316a2a579bc890935dc4f9beecccb8dcc
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72696000"
---
# <a name="xamarinforms-picker"></a>Xamarin. Forms-Auswahl

_Die Auswahl Ansicht ist ein Steuerelement zum Auswählen eines Text Elements aus einer Datenliste._

Das xamarin. Forms- [`Picker`](xref:Xamarin.Forms.Picker) zeigt eine kurze Liste von Elementen an, aus der der Benutzer ein Element auswählen kann. `Picker` definiert die folgenden Eigenschaften:

- [`Title`](xref:Xamarin.Forms.Picker.Title) vom Typ `string`, dessen standardmäßig `null` ist.
- `TitleColor` vom Typ [`Color`](xref:Xamarin.Forms.Color), die Farbe, die verwendet wird, um den `Title` Text anzuzeigen.
- [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) vom Typ `IList`, die Quell Liste der anzuzeigenden Elemente, deren Standard ist `null`.
- [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) vom Typ `int`, der Index des ausgewählten Elements, das standardmäßig auf-1 festgelegt ist.
- [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) vom Typ `object`, das ausgewählte Element, das standardmäßig auf `null` festgelegt ist.
- [`TextColor`](xref:Xamarin.Forms.Picker.TextColor) vom Typ [`Color`](xref:Xamarin.Forms.Color), die Farbe, in der der Text angezeigt wird. der Standardwert ist [`Color.Default`](xref:Xamarin.Forms.Color.Default).
- [`FontAttributes`](xref:Xamarin.Forms.Picker.FontAttributes) vom Typ [`FontAttributes`](xref:Xamarin.Forms.FontAttributes), dessen standardmäßig [`FontAtributes.None`](xref:Xamarin.Forms.FontAttributes.None)ist.
- [`FontFamily`](xref:Xamarin.Forms.Picker.FontFamily) vom Typ `string`, dessen standardmäßig `null` ist.
- [`FontSize`](xref:Xamarin.Forms.Picker.FontSize) vom Typ `double`, der standardmäßig auf-1,0 festgelegt ist.
- `CharacterSpacing` vom Typ `double` ist der Abstand zwischen den Zeichen des Elements, das vom `Picker` angezeigt wird.

Alle Eigenschaften werden [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekten unterstützt. Dies bedeutet, dass Sie formatiert werden können, und die Eigenschaften können Ziele von Daten Bindungen sein. Die Eigenschaften [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) und [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) haben einen Standard Bindungs Modus von [`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay). Dies bedeutet, dass Sie Ziele von Daten Bindungen in einer Anwendung sein können, die die [Model-View-ViewModel (MVVM)-](~/xamarin-forms/enterprise-application-patterns/mvvm.md) Architektur verwendet. Weitere Informationen zum Festlegen von Schriftart Eigenschaften finden Sie unter [Schriftarten](~/xamarin-forms/user-interface/text/fonts.md).

Eine [`Picker`](xref:Xamarin.Forms.Picker) zeigt keine Daten an, wenn Sie zum ersten Mal angezeigt wird. Stattdessen wird der Wert der [`Title`](xref:Xamarin.Forms.Picker.Title) -Eigenschaft als Platzhalter auf den IOS-und Android-Plattformen angezeigt:

[![](images/picker-initial.png "Initial Picker Display")](images/picker-initial-large.png#lightbox "Initial Picker Display")

Wenn das [`Picker`](xref:Xamarin.Forms.Picker) den Fokus erhält, werden seine Daten angezeigt, und der Benutzer kann ein Element auswählen:

[![](images/picker-selection.png "Picker Selecting an Item")](images/picker-selection-large.png#lightbox "Picker Selecting an Item")

Der [`Picker`](xref:Xamarin.Forms.Picker) löst ein [`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged) Ereignis aus, wenn der Benutzer ein Element auswählt. Nach der Auswahl wird das ausgewählte Element vom `Picker` angezeigt:

![](images/picker-after-selection.png "Picker after Selection")

Es gibt zwei Verfahren zum Auffüllen einer [`Picker`](xref:Xamarin.Forms.Picker) mit Daten:

- Festlegen der [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) -Eigenschaft auf die anzuzeigenden Daten. Dies ist das empfohlene Verfahren. Weitere Informationen finden Sie unter [Festlegen der ItemsSource-Eigenschaft einer](populating-itemssource.md)Auswahl.
- Hinzufügen der anzuzeigenden Daten zur [`Items`](xref:Xamarin.Forms.Picker.Items) Auflistung. Diese Technik war der ursprüngliche Prozess zum Auffüllen einer [`Picker`](xref:Xamarin.Forms.Picker) mit Daten. Weitere Informationen finden Sie unter [Hinzufügen von Daten zur Element Auflistung einer Auswahl](populating-items.md).

## <a name="related-links"></a>Verwandte Links

- [Auswahl](xref:Xamarin.Forms.Picker)
