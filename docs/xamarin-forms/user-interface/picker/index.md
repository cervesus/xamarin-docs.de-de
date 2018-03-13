---
title: Datumsauswahl
description: "Die Auswahl einer Ansicht ist ein Steuerelement für die Auswahl eines Text aus einer Liste von Daten."
ms.topic: article
ms.prod: xamarin
ms.assetid: D4815A4B-104B-4294-951B-BD8F2EC33C86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/11/2017
ms.openlocfilehash: fc1aaffe4e31b596d57b5de30c87217ffba3772e
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="picker"></a>Datumsauswahl

_Die Auswahl einer Ansicht ist ein Steuerelement für die Auswahl eines Text aus einer Liste von Daten._

Ein [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) zeigt eine kurze Liste mit Elementen, in dem der Benutzer auswählen kann. Allerdings eine [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) keine Daten angezeigt, wenn er erstmals angezeigt wird. Stattdessen den Wert des seine [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Title/) Eigenschaft wird als Platzhalter für IOS- und Android-Plattformen dargestellt:

[![](images/picker-initial.png "Erste Auswahl einer Anzeige")](images/picker-initial-large.png#lightbox "erste Auswahl anzeigen")

Wenn die [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) Gewinne Fokus, seine Daten angezeigt wird und der Benutzer ein Element auswählen kann:

[![](images/picker-selection.png "Auswählen eines Elements Datumsauswahl")](images/picker-selection-large.png#lightbox "Datumsauswahl Auswählen eines Elements")

Nach Auswahl des ausgewählten Elements wird angezeigt, die [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/):

![](images/picker-after-selection.png "Auswahl nach der Auswahl")

Es gibt zwei Verfahren zum Auffüllen einer [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) mit Daten:

- Festlegen der [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) Eigenschaft, um die Daten angezeigt werden. Dies ist das empfohlene Verfahren, die in Xamarin.Forms 2.3.4 eingeführt wurde. Weitere Informationen finden Sie unter [Festlegen einer Auswahl ItemsSource Eigenschaft](populating-itemssource.md).
- Hinzufügen von Daten angezeigt werden die [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) Auflistung. Diese Technik wurde der ursprüngliche Prozess für das Auffüllen einer [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) mit Daten. Weitere Informationen finden Sie unter [Hinzufügen von Daten an eine Datumsauswahl Items-Auflistung](populating-items.md).


## <a name="related-links"></a>Verwandte Links

- [Auswahl](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)
