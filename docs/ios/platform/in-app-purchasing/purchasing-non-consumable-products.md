---
title: Erwerb von Produkten nicht verwendbar
ms.topic: article
ms.prod: xamarin
ms.assetid: 635D9CA2-6BCA-53E1-7B10-968029AA3493
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: f5fbd2282dc2f1d5e9a3fa6d29e7b74fde89fc42
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="purchasing-non-consumable-products"></a>Erwerb von Produkten nicht verwendbar

Produkte nicht verwendbar sind, "Besitzer" des Kunden. Es wird erwartet, dass sie stets Zugriff auf sie haben werden, auch wenn ihr Gerät verloren geht/gestohlen oder erwerben Sie ein neues Konto. Sie sind hilfreich für Bücher, Magazin Probleme, Spiel Ebenen, Foto-Filter, "Pro-Features" usw. Sobald ein Benutzer nicht die Verwendbarkeit Produkt gekauft hat, müssen sie niemals dafür Zahlen erneut. Wenn Ihr Code versehentlich Sie ihn ausprobieren kann, wird StoreKit eine Meldung angezeigt, die sie bereits erworben wurden.

## <a name="non-consumable-products-sample"></a>Die Verwendbarkeit nicht Produkte-Beispiel

Die [InAppPurchaseSample Code](https://developer.xamarin.com/samples/monotouch/StoreKit/) enthält ein Projekt mit der Bezeichnung *NonConsumables*. Im Codebeispiel wird veranschaulicht, wie nicht verwendbar Produkte verwenden Sie als Beispiel die Foto-Filter implementiert wird. Sobald Sie einen Filter erworben haben können Sie sie immer wieder auf das Foto anwenden. Sie müssen nie neu zu erwerben.   
   
   
   
 Einkaufsvorgangs wird dargestellt, in dieser Serie Screenshots – die **kaufen** Schaltfläche "" steht die Schaltfläche "Aktivierung der Funktion":   
   
   
   
 [ ![](purchasing-non-consumable-products-images/image34.png "Einkaufsvorgangs wird in dieser Serie Screenshots dargestellt.")](purchasing-non-consumable-products-images/image34.png)   
   
   
   
 Erwerb des entspricht einem nutzbar Produkt; die wichtigsten Unterschied besteht darin, wie die Bestellung im Anwendungscode nachverfolgt wird. In diesem Beispiel wird die Schaltfläche "kaufen" nur ist verfügbar, wenn das Produkt noch nicht erworben wurde, andernfalls wird die Schaltfläche aktiviert die Funktion selbst.   
   
   
   

Das folgende Diagramm zeigt die Interaktionen zwischen Klassen und dem App Store-Server einen Produktkauf nicht verwendbar ausführen:   
   
   
   
 [ ![](purchasing-non-consumable-products-images/image35.png "Erwerben die Interaktionen zwischen Klassen und dem App Store-Server zum Ausführen eines Produkts nicht verwendbar")](purchasing-non-consumable-products-images/image35.png)   
   
   
   
 Der wesentliche Unterschied aus dem Beispiel nutzbar ist, dass nach Abschluss der Kauf die Benutzeroberfläche so aktualisiert wird, um zu verhindern, dass erneut erwerben. In diesem Beispiel wird die Benachrichtigung von einer erfolgreichen Transaktion die Benutzeroberfläche aktualisiert, damit die **kaufen** Schaltfläche wird konvertiert in eine Schaltfläche, die die Funktion selbst wird aktiviert.

## <a name="re-purchasing-non-consumable-products"></a>Erneut kaufen Verwendbarkeit nicht Produkte

Ihr Code sollte normalerweise ausblenden oder eine Schaltfläche Einkauf wiederverwenden, nachdem das Produkt erfolgreich käuflich erworben wurde, um zu verhindern, dass den Benutzer versucht, erneut das Produkt kaufen. Die beispielanwendung wird dies durch Ändern der **kaufen** Schaltfläche in der Schaltfläche, die durch den Beispiel Foto-Filter verwendet werden.   
   
   
   
 Es gibt Situationen, in denen eine Anwendung erkennen können, ob ein Produkt Verwendbarkeit nicht bereits erworben wurden:

-  Wenn eine Anwendung gelöscht und erneut auf einem Gerät installiert wird, werden alle Kaufdatensätze verschwunden, (es sei denn, /, bis der Benutzer eine Sicherung wiederherstellen besitzt). 
-  Verfügt der Benutzer die Anwendung auf zwei (oder mehr) Geräten installiert, und macht einen Kauf auf eines der Geräte. Die anderen Geräte werden weiterhin das Produkt erworben werden angezeigt. 
-  Wenn ein Kunde versucht, ein Produkt Verwendbarkeit nicht in diesen Situationen erneut zu erwerben, werden im App Store das Produkt erneut ohne dafür Gebühren zu erfüllen. Zum Ausführen einer Bestellung wird zunächst der Benutzeroberfläche angezeigt (z. B. eine Bestätigung Warnung wird angezeigt und werden die Apple-ID) jedoch dem Benutzer dann angezeigt wird eine Meldung, in der sie darüber informiert werden, dass das Produkt bereits erworben wurde.  
   
   
   
 Der Pfad für den in diesem Szenario ist identisch mit einem regulären Kauf, sind die einzigen Unterschiede:

-  Der Benutzer wird nicht erneut für das Produkt in Rechnung gestellt.
-  Die `SKPaymentTransaction` an die Anwendung übergebene Objekt weist eine `OriginalTransaction` -Eigenschaft, um die Transaktion verweist, die generiert wurde, wenn das Produkt ursprünglich erworben wurde. 
-  Anwendungen, die nicht verwendbar Produkte verkaufen müssen auch StoreKits implementieren **wiederherstellen** Funktion, damit Benutzer vorhandene Käufe abrufen können. 
