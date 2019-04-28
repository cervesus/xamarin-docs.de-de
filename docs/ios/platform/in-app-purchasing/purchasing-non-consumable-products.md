---
title: Erwerb von nicht-Verbrauchsartikeln in Xamarin.iOS
description: Dieses Dokument beschreibt die nicht-verbrauchsartikeln in Xamarin.iOS, Funktionen, die von einem Benutzer erworben haben, die weiterhin auf unbestimmte Zeit, unabhängig vom Gerät zur Verfügung.
ms.prod: xamarin
ms.assetid: 635D9CA2-6BCA-53E1-7B10-968029AA3493
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 060403baf8ac28b9b160632a01471b9828735069
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61403203"
---
# <a name="purchasing-non-consumable-products-in-xamarinios"></a>Erwerb von nicht-Verbrauchsartikeln in Xamarin.iOS

Nicht-verbrauchsartikeln sind, "Besitzer" des Kunden. Es wird erwartet, dass sie immer auf diese zugreifen können, selbst wenn ihr Gerät verloren geht/gestohlen oder sie einen neuen kaufen. Sie sind hilfreich für Bücher, magazine-Ausgaben, Level in Spielen, Foto-Filter, "Pro-Features" usw. Sobald ein Benutzer nicht-anwendbares Produkt gekauft hat, müssen sie nie wieder dafür bezahlen. Wenn Ihr Code versehentlich versuchen kann, wird die StoreKit eine Meldung angezeigt, die sie bereits erworben wurden.

## <a name="non-consumable-products-sample"></a>Beispiel für nicht-Verbrauchsartikeln

Die [InAppPurchaseSample Code](https://developer.xamarin.com/samples/monotouch/StoreKit/) enthält ein Projekt namens *NonConsumables*. Im Codebeispiel wird veranschaulicht, wie zum Implementieren von nicht-verbrauchsartikeln Foto-Filter beispielsweise verwenden wird. Nachdem Sie einen Filter erworben haben können Sie es immer wieder zu dem Foto anwenden. Sie müssen nie erwerben Sie es erneut.   
   
   
   
 Der Kaufvorgang wird angezeigt, in dieser Reihe von Screenshots – die **kaufen** Schaltfläche wird die Schaltfläche mit den Feature-Aktivierung:   
   
   
   
 [![](purchasing-non-consumable-products-images/image34.png "Der Kaufvorgang wird in dieser Reihe von Screenshots angezeigt.")](purchasing-non-consumable-products-images/image34.png#lightbox)   
   
   
   
 Erwerb des ist identisch mit einem Produkt genutzt werden. Der Hauptunterschied besteht im wie der Kauf im Anwendungscode nachverfolgt wird. In diesem Beispiel wird die Schaltfläche "kaufen" nur ist verfügbar, wenn das Produkt noch nicht erworben wurde, andernfalls wird die Schaltfläche aktiviert das Feature selbst.   
   
   
   

Das folgende Diagramm zeigt die Interaktionen zwischen Klassen und dem App Store-Server zum Ausführen von einer nicht-anwendbares Produktkauf:   
   
   
   
 [![](purchasing-non-consumable-products-images/image35.png "Erwerben Sie die Interaktionen zwischen Klassen und dem App Store-Server zum Ausführen eines Produkts nicht verstanden werden können")](purchasing-non-consumable-products-images/image35.png#lightbox)   
   
   
   
 Der wesentliche Unterschied aus dem Beispiel nutzbar ist, dass nach der Erwerb die Benutzeroberfläche so aktualisiert wird, um zu verhindern, dass erneut erwerben. In diesem Beispiel wird die Benachrichtigung über eine erfolgreiche Transaktion die Benutzeroberfläche aktualisiert, damit die **kaufen** Schaltfläche wird konvertiert in eine Schaltfläche, die das Feature selbst aktiviert wird.

## <a name="re-purchasing-non-consumable-products"></a>Neu kaufen von nicht-Verbrauchsartikeln

Ihr Code sollte normalerweise ausblenden oder verwenden Sie nach das Produkt erfolgreich käuflich erworben wurde, um zu verhindern, dass den Benutzer versucht, den Kauf des Produkts erneut, eine Schaltfläche "kaufen". Die beispielanwendung wird durch Ändern der **kaufen** Schaltfläche in der Schaltfläche, die die Beispiel-Fotofilter arbeiten kann.   
   
   
   
 Es gibt Situationen, in denen eine Anwendung feststellen kann, ob ein nicht-anwendbares Produkt bereits gekauft wurde:

-  Wenn eine Anwendung gelöscht und erneut auf einem Gerät installiert wird, werden alle Kaufdatensätze nicht mehr vorhanden, (es sei denn, /, bis der Benutzer eine Sicherung / Wiederherstellung ist). 
-  Wenn der Benutzer hat die Anwendung auf zwei (oder mehr) Geräten installiert, und sorgt für einen Kauf auf eines der Geräte. Die anderen Geräte werden weiterhin das Produkt, das zum Kauf zur Verfügung angezeigt. 
-  Wenn ein Kunde versucht, die ein Produkt nicht verstanden werden können, in diesen Fällen neu erwerben, wird die App-Store das Produkt erneut ohne Kosten erfüllen. Die Benutzeroberfläche erscheint zunächst einen Kauf durchführen (z. B. eine Bestätigung Warnung wird angezeigt, und die Apple-ID wird erforderlich sein) jedoch dem Benutzer dann angezeigt wird eine Meldung, die ihm mitgeteilt wird, dass das Produkt bereits gekauft wurde.  
   
   
   
 Der Pfad für den in diesem Szenario ist identisch mit einer regulären erwerben, sind die einzigen Unterschiede:

-  Der Benutzer wird nicht erneut für das Produkt in Rechnung gestellt.
-  Die `SKPaymentTransaction` an die Anwendung übergebene Objekt weist ein `OriginalTransaction` Eigenschaft, die für die Transaktion verweist, die generiert wurde, wenn das Produkt ursprünglich erworben wurde. 
-  Anwendungen, die nicht-verbrauchsartikeln verkaufen müssen auch StoreKits implementieren **wiederherstellen** Feature, mit Benutzern, die vorhandenen Käufe abrufen können. 
