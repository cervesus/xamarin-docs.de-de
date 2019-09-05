---
title: Erwerben von nutzbaren Produkten in xamarin. IOS
description: In diesem Dokument werden nutzbare Produkte in xamarin. IOS beschrieben. Nutzbare Produkte sind einzelne Verwendungs Elemente, wie z. b. in-Game-Währungen.
ms.prod: xamarin
ms.assetid: E0CB4A0F-C3FA-3933-58A7-13246971D677
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/18/2017
ms.openlocfilehash: c23515c7fc7a3fef836cba76ec30279c94150da2
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70281534"
---
# <a name="purchasing-consumable-products-in-xamarinios"></a>Erwerben von nutzbaren Produkten in xamarin. IOS

Nutzbare Produkte sind am einfachsten zu implementieren, da keine "Restore"-Anforderung vorhanden ist. Sie sind nützlich für Produkte wie z. b. in-Game-Währungen oder eine einzelne nutzbare Funktion. Benutzer können über-und-wiederverwendbare Produkte erneut erwerben.

## <a name="built-in-product-delivery"></a>Integrierte Produkt Bereitstellung

Der Beispielcode für dieses Dokument veranschaulicht integrierte Produkte – die Produkt-IDs sind in der Anwendung hart codiert, da Sie eng mit dem Code verknüpft sind, der das Feature nach der Zahlung entsperrt. Der Kaufvorgang kann wie folgt visualisiert werden:   
   
[![Die Visualisierung des Kaufprozesses](purchasing-consumable-products-images/image26.png)](purchasing-consumable-products-images/image26.png#lightbox)     
   
 Der grundlegende Workflow lautet:   
   
 1. Die APP fügt der `SKPayment` Warteschlange ein hinzu. Falls erforderlich, wird der Benutzer zur Eingabe der Apple-ID aufgefordert und aufgefordert, die Zahlung zu bestätigen.   
   
 2. Storekit sendet die Anforderung zur Verarbeitung an den Server.   
   
 3. Wenn die Transaktion abgeschlossen ist, antwortet der Server mit einer Transaktionsbestätigung.   
   
 4. Die `SKPaymentTransactionObserver` Unterklasse empfängt die Bestätigung und verarbeitet sie.   
   
 5. Die Anwendung aktiviert das Produkt (durch Aktualisieren `NSUserDefaults` oder einen anderen Mechanismus) und ruft dann `FinishTransaction`storekit auf.

Es gibt eine andere Art von Workflow – vom *Server übermittelte Produkte* –, der weiter unten in diesem Dokument erläutert wird (Weitere Informationen finden Sie im Abschnitt *Beleg Überprüfung und vom Server übermittelte Produkte*).

## <a name="consumable-products-example"></a>Beispiel für consumier Bare Produkte

Der [inapppurchasesample-Code](https://docs.microsoft.com/samples/xamarin/ios-samples/storekit) enthält ein Projekt mit dem Namen " *Verbrauchsgüter* ", das eine einfache "in-Game"-Währung (als "affgutguthaben" bezeichnet) implementiert. Das Beispiel zeigt, wie zwei in-App-Produkte implementiert werden, um dem Benutzer zu ermöglichen, so viele "affengutschriften" wie gewünscht zu erwerben – in einer realen Anwendung wäre es auch möglich, diese zu verbringen.   
   
   
   
 Die Anwendung wird in diesen Screenshots angezeigt – jeder Kauf fügt dem Benutzer Saldo weitere "affengutschriften" hinzu:   
   
   
   
 [![Durch jeden Kauf werden dem Benutzer Guthaben weitere affengutschriften hinzugefügt.](purchasing-consumable-products-images/image27.png)](purchasing-consumable-products-images/image27.png#lightbox)   
   
   
   
 Die Interaktionen zwischen benutzerdefinierten Klassen, storekit und App Store sehen wie folgt aus:   
   
   
   
 [![Interaktionen zwischen benutzerdefinierten Klassen, storekit und App Store](purchasing-consumable-products-images/image28.png)](purchasing-consumable-products-images/image28.png#lightbox)

&nbsp;

### <a name="viewcontroller-methods"></a>ViewController-Methoden

Zusätzlich zu den Eigenschaften und Methoden, die zum Abrufen von Produktinformationen erforderlich sind, benötigt der Ansichts Controller zusätzliche Benachrichtigungs Beobachter, um auf Kauf bezogene Benachrichtigungen zu lauschen. Diese werden nur `NSObjects` registriert und `ViewWillDisappear` in `ViewWillAppear` bzw. entfernt.

```csharp
NSObject succeededObserver, failedObserver;
```

Der Konstruktor erstellt `SKProductsRequestDelegate` auch die Unterklasse ( `InAppPurchaseManager`), die wiederum den `SKPaymentTransactionObserver` ( `CustomPaymentObserver`) erstellt und registriert.   
   
   
   
 Der erste Teil der Verarbeitung einer in-App-Kauftransaktion besteht darin, den Schaltflächen Druck zu verarbeiten, wenn der Benutzer etwas kaufen möchte, wie im folgenden Code aus der Beispielanwendung gezeigt:

```csharp
buy5Button.TouchUpInside += (sender, e) => {
   iap.PurchaseProduct (Buy5ProductId);
};
buy10Button.TouchUpInside += (sender, e) => {
   iap.PurchaseProduct (Buy10ProductId);
};
```

   
   
 Der zweite Teil der Benutzeroberfläche ist die Verarbeitung der Benachrichtigung, dass die Transaktion erfolgreich war, in diesem Fall durch Aktualisieren des angezeigten Ausgleichs:

```csharp
priceObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerTransactionSucceededNotification,
(notification) => {
   balanceLabel.Text = CreditManager.Balance() + " monkey credits";
});
```

Der letzte Teil der Benutzeroberfläche zeigt eine Meldung an, wenn eine Transaktion aus irgendeinem Grund abgebrochen wird. Im Beispielcode wird eine Nachricht einfach in das Ausgabefenster geschrieben:

```csharp
failedObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerTransactionFailedNotification,
(notification) => {
   Console.WriteLine ("Transaction Failed");
});
```

Zusätzlich zu diesen Methoden auf dem Ansichts Controller erfordert eine nutzbare Produktkauf Transaktion auch Code in `SKProductsRequestDelegate` `SKPaymentTransactionObserver`und.

### <a name="inapppurchasemanager-methods"></a>Inapppurchasemanager-Methoden

Der Beispielcode implementiert eine Reihe von Kauf bezogenen Methoden in der inapppurchasemanager-Klasse, einschließlich `PurchaseProduct` der-Methode, `SKPayment` die eine-Instanz erstellt und zur Verarbeitung der Warteschlange hinzufügt:

```csharp
public void PurchaseProduct(string appStoreProductId)
{
   SKPayment payment = SKPayment.PaymentWithProduct (appStoreProductId);
   SKPaymentQueue.DefaultQueue.AddPayment (payment);
}
```

Das Hinzufügen der Zahlung zur Warteschlange ist ein asynchroner Vorgang. Die Anwendung erhält die Kontrolle, während storekit die Transaktion verarbeitet und an die Server von Apple sendet. An dieser Stelle prüft IOS, ob der Benutzer am App Store angemeldet ist, und fordert ihn ggf. zur Eingabe einer Apple-ID und eines Kennworts auf.   
   
   
   
 Wenn sich der Benutzer erfolgreich mit dem App Store authentifiziert und der Transaktion zustimmt, empfängt `SKPaymentTransactionObserver` die storekit-Antwort und ruft die folgende Methode auf, um die Transaktion zu erfüllen und Sie abzuschließen.

```csharp
public void CompleteTransaction (SKPaymentTransaction transaction)
{
   var productId = transaction.Payment.ProductIdentifier;
   // Register the purchase, so it is remembered for next time
   PhotoFilterManager.Purchase(productId);
   FinishTransaction(transaction, true);
}
```

Im letzten Schritt müssen Sie sicherstellen, dass Sie storekit Benachrichtigen, dass die Transaktion erfolgreich ausgeführt wurde, `FinishTransaction`indem Sie Folgendes aufrufen:

```csharp
public void FinishTransaction(SKPaymentTransaction transaction, bool wasSuccessful)
{
   // remove the transaction from the payment queue.
   SKPaymentQueue.DefaultQueue.FinishTransaction(transaction);  // THIS IS IMPORTANT - LET'S APPLE KNOW WE'RE DONE !!!!
   using (var pool = new NSAutoreleasePool()) {
       NSDictionary userInfo = NSDictionary.FromObjectsAndKeys(new NSObject[] {transaction},new NSObject[] {new NSString("transaction")});
       if (wasSuccessful) {
           // send out a notification that we've finished the transaction
           NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerTransactionSucceededNotification, this, userInfo);
       } else {
           // send out a notification for the failed transaction
           NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerTransactionFailedNotification, this, userInfo);
       }
   }
}
```

Nachdem das Produkt übermittelt wurde `SKPaymentQueue.DefaultQueue.FinishTransaction` , muss aufgerufen werden, um die Transaktion aus der Zahlungs Warteschlange zu entfernen.

### <a name="skpaymenttransactionobserver-custompaymentobserver-methods"></a>Skpaymenttransaktionobserver-Methoden (custompaymentobserver)

Storekit Ruft die `UpdatedTransactions` -Methode auf, wenn Sie eine Antwort von den Servern von Apple empfängt, und `SKPaymentTransaction` übergibt ein Array von-Objekten, die der Code überprüfen kann. Die-Methode durchläuft jede Transaktion und führt basierend auf dem Transaktionsstatus eine andere Funktion aus (wie hier gezeigt):

```csharp
public override void UpdatedTransactions (SKPaymentQueue queue, SKPaymentTransaction[] transactions)
{
   foreach (SKPaymentTransaction transaction in transactions)
   {
       switch (transaction.TransactionState)
       {
           case SKPaymentTransactionState.Purchased:
              theManager.CompleteTransaction(transaction);
               break;
           case SKPaymentTransactionState.Failed:
              theManager.FailedTransaction(transaction);
               break;
           default:
               break;
       }
   }
}
```

Die `CompleteTransaction` -Methode wurde weiter oben in diesem Abschnitt behandelt – Sie speichert die Kauf `NSUserDefaults`Details in, schließt die Transaktion mit storekit ab und benachrichtigt die Benutzeroberfläche schließlich für die Aktualisierung.

### <a name="purchasing-multiple-products"></a>Erwerb mehrerer Produkte

Wenn es in Ihrer Anwendung sinnvoll ist, mehrere Produkte zu kaufen, verwenden `SKMutablePayment` Sie die-Klasse, und legen Sie das Feld Menge fest:

```csharp
public void PurchaseProduct(string appStoreProductId)
{
   SKMutablePayment payment = SKMutablePayment.PaymentWithProduct (appStoreProductId);
   payment.Quantity = 4; // hardcoded as an example
   SKPaymentQueue.DefaultQueue.AddPayment (payment);
}
```

Der Code, der die abgeschlossene Transaktion behandelt, muss auch die Eigenschaft "Menge" Abfragen, um den Kauf ordnungsgemäß abzuschließen:

```csharp
public void CompleteTransaction (SKPaymentTransaction transaction)
{
   var productId = transaction.Payment.ProductIdentifier;
   var qty = transaction.Payment.Quantity;
   if (productId == ConsumableViewController.Buy5ProductId)
       CreditManager.Add(5 * qty);
   else if (productId == ConsumableViewController.Buy10ProductId)
       CreditManager.Add(10 * qty);
   else
       Console.WriteLine ("Shouldn't happen, there are only two products");
   FinishTransaction(transaction, true);
}
```

Wenn der Benutzer mehrere Mengen erwirbt, spiegelt die storekit-Bestätigungs Warnung die Menge, den Einzelpreis und den Gesamtpreis wider, die Ihnen in Rechnung gestellt werden, wie im folgenden Screenshot zu sehen:

[![Bestätigen eines Kaufs](purchasing-consumable-products-images/image30.png)](purchasing-consumable-products-images/image30.png#lightbox)

## <a name="handling-network-outages"></a>Behandeln von Netzwerkausfällen

In-App-Käufe erfordern eine funktionierende Netzwerkverbindung für storekit, um mit den Apple-Servern zu kommunizieren. Wenn keine Netzwerkverbindung verfügbar ist, ist der in-App-Einkauf nicht verfügbar.

### <a name="product-requests"></a>Produktanforderungen

Wenn das `SKProductRequest`Netzwerk beim Erstellen einer nicht verfügbar ist, wird die `RequestFailed` - `SKProductsRequestDelegate` Methode der unter `InAppPurchaseManager`Klasse () aufgerufen, wie unten dargestellt:

```csharp
public override void RequestFailed (SKRequest request, NSError error)
{
   using (var pool = new NSAutoreleasePool()) {
       NSDictionary userInfo = NSDictionary.FromObjectsAndKeys(new NSObject[] {error},new NSObject[] {new NSString("error")});
       // send out a notification for the failed transaction
       NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerRequestFailedNotification, this, userInfo);
   }
}
```

Der ViewController lauscht dann auf die Benachrichtigung und zeigt eine Meldung in den Kauf Schaltflächen an:

```csharp
requestObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerRequestFailedNotification,
(notification) => {
   Console.WriteLine ("Request Failed");
   buy5Button.SetTitle ("Network down?", UIControlState.Disabled);
   buy10Button.SetTitle ("Network down?", UIControlState.Disabled);
});
```

Da eine Netzwerkverbindung auf mobilen Geräten flüchtig sein kann, möchten Anwendungen möglicherweise den Netzwerkstatus mithilfe des SystemConfiguration-Frameworks überwachen und erneut versuchen, wenn eine Netzwerkverbindung verfügbar ist. Weitere Informationen finden Sie unter Apple oder der, der die Anwendung verwendet.

### <a name="purchase-transactions"></a>Transaktionen kaufen

Die storekit-Zahlungs Warteschlange speichert und weiterleiten Kauf Anforderungen, wenn dies möglich ist, sodass die Auswirkung eines Netzwerkausfalls in Abhängigkeit davon variiert, wann das Netzwerk während des Kaufvorgangs fehlgeschlagen ist.   
   
   
   
 Wenn während einer Transaktion ein Fehler auftritt, wird für `SKPaymentTransactionObserver` die Unterklasse `CustomPaymentObserver`() die `UpdatedTransactions` -Methode aufgerufen, und `SKPaymentTransaction` die-Klasse befindet sich im fehlerhaften Zustand.

```csharp
public override void UpdatedTransactions (SKPaymentQueue queue, SKPaymentTransaction[] transactions)
{
   foreach (SKPaymentTransaction transaction in transactions)
   {
       switch (transaction.TransactionState)
       {
           case SKPaymentTransactionState.Purchased:
               theManager.CompleteTransaction(transaction);
               break;
           case SKPaymentTransactionState.Failed:
               theManager.FailedTransaction(transaction);
               break;
           default:
               break;
       }
   }
}
```

Die `FailedTransaction` -Methode erkennt, ob der Fehler durch einen Benutzer Abbruch verursacht wurde, wie hier gezeigt:

```csharp
public void FailedTransaction (SKPaymentTransaction transaction)
{
   //SKErrorPaymentCancelled == 2
   if (transaction.Error.Code == 2) // user cancelled
       Console.WriteLine("User CANCELLED FailedTransaction Code=" + transaction.Error.Code + " " + transaction.Error.LocalizedDescription);
   else // error!
       Console.WriteLine("FailedTransaction Code=" + transaction.Error.Code + " " + transaction.Error.LocalizedDescription);
   FinishTransaction(transaction,false);
}
```

Auch wenn eine Transaktion fehlschlägt, `FinishTransaction` muss die-Methode aufgerufen werden, um die Transaktion aus der Zahlungs Warteschlange zu entfernen:

```csharp
SKPaymentQueue.DefaultQueue.FinishTransaction(transaction);
```

Der Beispielcode sendet dann eine Benachrichtigung, sodass der ViewController eine Meldung anzeigen kann. Anwendungen sollten keine zusätzliche Nachricht anzeigen, wenn der Benutzer die Transaktion abgebrochen hat. Weitere Fehlercodes, die auftreten können, sind:

```csharp
FailedTransaction Code=0 Cannot connect to iTunes Store
FailedTransaction Code=5002 An unknown error has occurred
FailedTransaction Code=5020 Forget Your Password?
Applications may detect and respond to specific error codes, or handle them in the same way.
```

## <a name="handling-restrictions"></a>Behandeln von Einschränkungen

Mit den **Einstellungen > Feature "allgemeine > Einschränkungen** " von IOS können Benutzer bestimmte Features Ihres Geräts sperren.   
   
   
   
 Sie können Abfragen, ob der Benutzer über die `SKPaymentQueue.CanMakePayments` -Methode in-App-Käufe tätigen darf. Wenn false zurückgegeben wird, kann der Benutzer nicht auf den in-App-Einkauf zugreifen. Storekit zeigt dem Benutzer automatisch eine Fehlermeldung an, wenn versucht wird, einen Kauf auszuführen. Durch die Überprüfung dieses Werts kann die Anwendung stattdessen die Kauf Schaltflächen ausblenden oder andere Aktionen ausführen, um den Benutzer zu unterstützen.   
   
   
   
 In der `InAppPurchaseManager.cs` Datei umschließt die `CanMakePayments` Methode die storekit-Funktion wie folgt:

```csharp
public bool CanMakePayments()
{
   return SKPaymentQueue.CanMakePayments;
}
```

Um diese Methode zu testen, verwenden Sie das Feature " **Einschränkungen** " von IOS, um **in-App-Käufe**zu deaktivieren:   
   
   
   
 [![Verwenden des Features "Einschränkungen" von IOS zum Deaktivieren von in-App-Käufen](purchasing-consumable-products-images/image31.png)](purchasing-consumable-products-images/image31.png#lightbox)   
   
   
   
 Dieser Beispielcode von `ConsumableViewController` reagiert auf `CanMakePayments` die Rückgabe von false, indem der **Deaktivierte AppStore** -Text auf den Schaltflächen deaktiviert angezeigt wird

```csharp
// only if we can make payments, request the prices
if (iap.CanMakePayments()) {
   // now go get prices, if we don't have them already
   if (!pricesLoaded)
       iap.RequestProductData(products); // async request via StoreKit -> App Store
} else {
   // can't make payments (purchases turned off in Settings?)
   // the buttons are disabled by default, and only enabled when prices are retrieved
   buy5Button.SetTitle ("AppStore disabled", UIControlState.Disabled);
   buy10Button.SetTitle ("AppStore disabled", UIControlState.Disabled);
}
```

Die Anwendung sieht wie folgt aus, wenn die Funktion **in-App-Käufe** eingeschränkt ist – die Schaltflächen für den Einkauf sind deaktiviert.   
   
   
   
 [![Die Anwendung sieht wie folgt aus, wenn das Feature in-App-Käufe eingeschränkt ist und die Schaltflächen "kaufen" deaktiviert sind.](purchasing-consumable-products-images/image32.png)](purchasing-consumable-products-images/image32.png#lightbox)   
   
   
   

Produktinformationen können weiterhin angefordert werden, `CanMakePayments` Wenn false ist, sodass die APP weiterhin Preise abrufen und anzeigen kann. Das heißt, wenn wir die `CanMakePayments` Überprüfung aus dem Code entfernt haben, sind die Kauf Schaltflächen weiterhin aktiv. Wenn jedoch ein Kauf Versuch unternommen wird, wird dem Benutzer eine Meldung angezeigt, dass **in-App-Einkäufe nicht zulässig sind** (von storekit generiert, wenn die Zahlungs Warteschlange Zugriff):   
   
   
   
 [![In-App-Käufe sind nicht zulässig.](purchasing-consumable-products-images/image33.png)](purchasing-consumable-products-images/image33.png#lightbox)   
   
   
   
 Reale Anwendungen können einen anderen Ansatz für die Handhabung der Einschränkung treffen, z. b. das Ausblenden der Schaltflächen ganz und möglicherweise eine ausführlichere Meldung als die Warnung, die storekit automatisch anzeigt.

