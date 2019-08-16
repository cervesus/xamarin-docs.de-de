---
title: Erwerben nicht nutzbarer Produkte in xamarin. IOS
description: In diesem Dokument werden nicht nutzbare Produkte in xamarin. IOS beschrieben. dabei handelt es sich um Funktionen, die von einem Benutzer erworben wurden, der unabhängig vom Gerät unbegrenzt verfügbar bleibt.
ms.prod: xamarin
ms.assetid: 635D9CA2-6BCA-53E1-7B10-968029AA3493
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: e09d8511dc74bdf9368497ff8fe39bd87b62565c
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69528292"
---
# <a name="purchasing-non-consumable-products-in-xamarinios"></a>Erwerben nicht nutzbarer Produkte in xamarin. IOS

Nicht nutzbare Produkte sind vom Kunden "Besitzer". Es wird davon ausgegangen, dass Sie immer darauf zugreifen können, selbst wenn Ihr Gerät verloren geht/gestohlen wird oder Sie ein neues kaufen. Sie sind nützlich für Bücher, Magazine-Probleme, Spielebenen, Fotofilter, "pro-Features" usw. Nachdem ein Benutzer ein nicht nutzbares Produkt gekauft hat, muss er nie mehr dafür bezahlen. Wenn Ihr Code Sie versehentlich ausprobieren kann, zeigt storekit eine Meldung an, dass Sie bereits gekauft wurde.

## <a name="non-consumable-products-sample"></a>Beispiel für nicht nutzbare Produkte

Der [inapppurchasesample-Code](https://docs.microsoft.com/samples/xamarin/ios-samples/storekit) enthält ein Projekt mit dem Namen " *nicht Verbrauchsgüter*". Das Codebeispiel veranschaulicht, wie nicht nutzbare Produkte mithilfe von Foto Filtern als Beispiel implementiert werden. Nachdem Sie einen Filter gekauft haben, können Sie ihn immer wieder auf das Foto anwenden. Sie müssen ihn nie erneut erwerben.   
   
   
   
 Der Kaufvorgang wird in dieser Reihe von Screenshots angezeigt – die Schaltfläche " **kaufen** " wird zur Funktions Aktivierungs Schaltfläche:   
   
   
   
 [![](purchasing-non-consumable-products-images/image34.png "Der Kaufvorgang wird in dieser Reihe von Screenshots gezeigt.")](purchasing-non-consumable-products-images/image34.png#lightbox)   
   
   
   
 Der Kaufvorgang ist identisch mit einem nutzbaren Produkt. der Hauptunterschied besteht darin, wie der Kauf im Anwendungscode nachverfolgt wird. In diesem Beispiel ist die Schaltfläche "kaufen" nur verfügbar, wenn das Produkt nicht bereits gekauft wurde. andernfalls aktiviert die Schaltfläche die Funktion selbst.   
   
   
   

Das folgende Diagramm zeigt die Interaktionen zwischen Klassen und dem App Store-Server, um einen nicht nutzbaren Produktkauf durchzuführen:   
   
   
   
 [![](purchasing-non-consumable-products-images/image35.png "Interaktionen zwischen Klassen und dem App Store-Server, um einen nicht nutzbaren Produkt Einkauf auszuführen")](purchasing-non-consumable-products-images/image35.png#lightbox)   
   
   
   
 Der Hauptunterschied zum nutzbaren Beispiel besteht darin, dass nach Abschluss des Kaufs die Benutzeroberfläche aktualisiert wird, um einen erneuten Einkauf zu verhindern. In diesem Beispiel aktualisiert die Benachrichtigung einer erfolgreichen Transaktion die Benutzeroberfläche, sodass die Schaltfläche " **kaufen** " in eine Schaltfläche konvertiert wird, die das Feature selbst aktiviert.

## <a name="re-purchasing-non-consumable-products"></a>Erneutes kaufen nicht nutzbarer Produkte

Der Code sollte eine Kauf Schaltfläche normalerweise ausblenden oder wiederholen, sobald das Produkt erfolgreich gekauft wurde, um zu verhindern, dass der Benutzer versucht, das Produkt erneut zu kaufen. Die Beispielanwendung bewirkt, dass die Schaltfläche " **kaufen** " in die Schaltfläche geändert wird, mit der der Beispiel Fotofilter funktioniert.   
   
   
   
 Es gibt Situationen, in denen eine Anwendung nicht erkennen kann, ob bereits ein nicht nutzbares Produkt gekauft wurde:

- Wenn eine Anwendung auf einem Gerät gelöscht und erneut installiert wird, werden alle Kauf Datensätze entfernt (es sei denn, der Benutzer führt eine Sicherungs Wiederherstellung durch). 
- , Wenn der Benutzer die Anwendung auf zwei (oder mehr) Geräten installiert hat und einen Einkauf auf einem der Geräte durchführt. Die anderen Geräte zeigen weiterhin das Produkt an, das für den Kauf zur Verfügung steht. 
- Wenn ein Kunde versucht, ein nicht verwendbar bares Produkt in diesen Fällen erneut zu erwerben, wird das Produkt vom App Store ohne Aufpreis erneut untersucht. Die Benutzeroberfläche wird anfänglich angezeigt, um einen Kauf auszuführen (z. b. wird eine Bestätigungs Warnung angezeigt, und die Apple-ID ist erforderlich). dem Benutzer wird jedoch eine Meldung angezeigt, in der er darüber informiert wird, dass das Produkt bereits gekauft wurde.  
   
   
   
 Der Codepfad in diesem Szenario ist genau mit einem regulären Kauf identisch. die einzigen Unterschiede sind:

- Der Benutzer wird für das Produkt nicht erneut in Rechnung gestellt.
- Das `SKPaymentTransaction` an die Anwendung über gegebene-Objekt verfügt `OriginalTransaction` über eine-Eigenschaft, die auf die Transaktion verweist, die beim anfänglichen Erwerb des Produkts generiert wurde. 
- Anwendungen, die nicht nutzbare Produkte verkaufen, müssen auch die **Wiederherstellungs** Funktion von storekit implementieren, damit Benutzer vorhandene Einkäufe abrufen können. 
