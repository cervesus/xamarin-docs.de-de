---
title: Erwerb von nutzbar Produkte
ms.prod: xamarin
ms.assetid: E0CB4A0F-C3FA-3933-58A7-13246971D677
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 5c2c84c044ff41cced2c97e414502faff45341ec
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="purchasing-consumable-products"></a>Erwerb von nutzbar Produkte

Nutzbar Produkte sind die am einfachsten zu implementieren, da keine Notwendigkeit 'Wiederherstellen besteht'. Sie sind hilfreich für Produkte wie Währung Spiels oder einen einmalcode Teil der Funktionalität. Nutzbar Produkte über-und-Failover von Benutzer können erneut erneut erwerben.

## <a name="built-in-product-delivery"></a>Integrierte Produkt Übermittlung

Im Beispielcode zu diesem Dokument wird veranschaulicht, integrierte Produkte – Produkt-IDs sind in der Anwendung hartcodiert, da sie eng an den Code, die die Funktion nach Zahlung verbunden sind "entsperrt". Erwerb des kann wie folgt visuell dargestellt werden:   
   
[![Das Kaufverhalten Visualisierung verarbeiten](purchasing-consumable-products-images/image26.png)](purchasing-consumable-products-images/image26.png#lightbox)     
   
 Der grundlegende Workflow wird:   
   
   
   
 1. Fügt die app ein `SKPayment` an die Warteschlange. Bei Bedarf der Benutzer wird aufgefordert, ihre Apple-ID und aufgefordert, die Zahlung zu bestätigen.   
   
 2. StoreKit sendet die Anforderung zur Verarbeitung an den Server.   
   
 3. Wenn die Transaktion abgeschlossen ist, antwortet der Server mit der eine Bestätigung der Transaktion.   
   
 4. Die `SKPaymentTransactionObserver` Unterklasse die Bestätigung empfängt und verarbeitet es.   
   
 5. Ermöglicht die Anwendung des Produkts (durch die Aktualisierung `NSUserDefaults` oder eines anderen Mechanismus), und ruft dann die StoreKit `FinishTransaction`.

Es ist ein weiterer Typ von Workflow – *Server-Delivered Produkte* – d. h. weiter unten in das Dokument (finden Sie im Abschnitt *Receipt-Überprüfung und Server-Delivered Produkte*).

## <a name="consumable-products-example"></a>Nutzbar Produkte-Beispiel

Die [InAppPurchaseSample Code](https://developer.xamarin.com/samples/monotouch/StoreKit/) enthält ein Projekt mit der Bezeichnung *Verbrauchsgüter* , der einen grundlegenden 'Spiels Currency' (so genannte "Monkey Gutschriften") implementiert. Das Beispiel zeigt, wie in der app-Käufe Produkten, damit die Benutzer kaufen, wie viele "Monkey Gutschriften" wie gewünscht – in einer realen Anwendung auch es eine Möglichkeit gäbe, diese Ausgaben implementiert!   
   
   
   
 Die Anwendung wird in diese Screenshots dargestellt – jeder Bestellung Saldo des Benutzers weitere "Monkey Gutschriften" hinzugefügt:   
   
   
   
 [![Jede Bestellung fügt Weitere Affe Guthaben auf die Balance Benutzer](purchasing-consumable-products-images/image27.png)](purchasing-consumable-products-images/image27.png#lightbox)   
   
   
   
 Die Interaktionen zwischen benutzerdefinierten Klassen StoreKit und dem App Store aussehen wie folgt:   
   
   
   
 [![Die Interaktionen zwischen benutzerdefinierten Klassen StoreKit und dem App Store](purchasing-consumable-products-images/image28.png)](purchasing-consumable-products-images/image28.png#lightbox)

&nbsp;

### <a name="viewcontroller-methods"></a>ViewController-Methoden

Zusätzlich zu den Eigenschaften und Methoden, die erforderlich sind, um Produktinformationen abzurufen erfordert die View-Controller zusätzliche Benachrichtigung Beobachter für Kauf-bezogenen Benachrichtigungen überwacht. Hierbei handelt es sich einfach `NSObjects` , registriert und in entfernt `ViewWillAppear` und `ViewWillDisappear` bzw.

```csharp
NSObject succeededObserver, failedObserver;
```

Der Konstruktor erstellt auch die `SKProductsRequestDelegate` Unterklasse ( `InAppPurchaseManager`), die wiederum erstellt und registriert die `SKPaymentTransactionObserver` ( `CustomPaymentObserver`).   
   
   
   
 Der erste Teil der Verarbeitung einer Transaktions in app-Käufe ist, Drücken der Schaltfläche zu behandeln, wenn der Benutzer etwas kaufen möchte, wie im folgenden Code aus der beispielanwendung dargestellt:

```csharp
buy5Button.TouchUpInside += (sender, e) => {
   iap.PurchaseProduct (Buy5ProductId);
};
buy10Button.TouchUpInside += (sender, e) => {
   iap.PurchaseProduct (Buy10ProductId);
};
```

   
   
 Die zweite Teil der Benutzeroberfläche die Benachrichtigung behandelt wird, dass die Transaktion, in diesem Fall erfolgreich durch den angezeigten Saldo aktualisieren:

```csharp
priceObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerTransactionSucceededNotification,
(notification) => {
   balanceLabel.Text = CreditManager.Balance() + " monkey credits";
});
```

Der letzte Teil der Benutzeroberfläche wird eine Meldung angezeigt, wenn aus irgendeinem Grund eine Transaktion abgebrochen wird. Im Beispielcode wird eine Meldung einfach in das Fenster "Ausgabe" geschrieben:

```csharp
failedObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerTransactionFailedNotification,
(notification) => {
   Console.WriteLine ("Transaction Failed");
});
```

Neben diesen Methoden auf dem Controller anzeigen, erfordert eine Einkaufstransaktion nutzbar Product Code auch auf die `SKProductsRequestDelegate` und `SKPaymentTransactionObserver`.

### <a name="inapppurchasemanager-methods"></a>InAppPurchaseManager-Methoden

Der Code implementiert eine Reihe von erwerben verwandte Methoden für die InAppPurchaseManager-Klasse, einschließlich der `PurchaseProduct` -Methode, erstellt eine `SKPayment` -Instanz und fügt es in die Warteschlange für die Verarbeitung:

```csharp
public void PurchaseProduct(string appStoreProductId)
{
   SKPayment payment = SKPayment.PaymentWithProduct (appStoreProductId);
   SKPaymentQueue.DefaultQueue.AddPayment (payment);
}
```

Hinzufügen von die Zahlung an die Warteschlange ist ein asynchroner Vorgang. Die Anwendung erhält Steuerelement wieder, während StoreKit die Transaktion verarbeitet und diese an Apple Server sendet. Es ist an diesem Punkt, dass der Benutzer auf den App Store angemeldet ist und sie ein Apple-ID und ein Kennwort aufgefordert werden, falls erforderlich, iOS überprüft.   
   
   
   
 Dass der Benutzer erfolgreich authentifiziert, mit dem App Store und für die Transaktion stimmt die `SKPaymentTransactionObserver` StoreKits Antwort empfängt und rufen Sie die folgende Methode, um die Transaktion zu erfüllen und es zu beenden.

```csharp
public void CompleteTransaction (SKPaymentTransaction transaction)
{
   var productId = transaction.Payment.ProductIdentifier;
   // Register the purchase, so it is remembered for next time
   PhotoFilterManager.Purchase(productId);
   FinishTransaction(transaction, true);
}
```

Im letzte Schritt wird sichergestellt, dass StoreKit zu benachrichtigen, dass Sie die Transaktion erfolgreich durch den Aufruf erfüllt haben `FinishTransaction`:

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

Nach der Auslieferung des Produkts `SKPaymentQueue.DefaultQueue.FinishTransaction` aufgerufen werden, um die Transaktion aus der Warteschlange für die Zahlung zu entfernen.

### <a name="skpaymenttransactionobserver-custompaymentobserver-methods"></a>SKPaymentTransactionObserver (CustomPaymentObserver)-Methoden

StoreKit Aufrufe der `UpdatedTransactions` Methode, wenn er eine Antwort vom Apple Server empfängt und ein Array von übergibt `SKPaymentTransaction` Objekte für den Code, um zu überprüfen. Die Methode durchläuft jede Transaktion und führt eine andere Funktion, die basierend auf den Transaktionsstatus, (wie hier gezeigt):

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

Die `CompleteTransaction` Methode wurde zuvor in diesem Abschnitt behandelten – speichert die Kaufdetails zu `NSUserDefaults`, schließt die Transaktion mit StoreKit ab und schließlich informiert die Benutzeroberfläche zu aktualisieren.

### <a name="purchasing-multiple-products"></a>Mehrere Produkte kaufen

Wenn es in Ihrer Anwendung mehrere Produkte zu kaufen Sinn macht, verwenden Sie die `SKMutablePayment` Klasse, und legen Sie das Feld Menge:

```csharp
public void PurchaseProduct(string appStoreProductId)
{
   SKMutablePayment payment = SKMutablePayment.PaymentWithProduct (appStoreProductId);
   payment.Quantity = 4; // hardcoded as an example
   SKPaymentQueue.DefaultQueue.AddPayment (payment);
}
```

Der Code zur Behandlung der abgeschlossenen Transaktions muss auch Abfragen, die Menge-Eigenschaft, um die Bestellung korrekt zu erfüllen:

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

Wenn der Benutzer mehrere Mengen kauft, wird StoreKit Bestätigung Warnung wider, die Menge, den Einzelpreis und der Gesamtpreis, den sie in Rechnung gestellt werden müssen, den wie im folgenden Screenshot gezeigt:

[![Eine Bestellung bestätigen](purchasing-consumable-products-images/image30.png)](purchasing-consumable-products-images/image30.png#lightbox)

## <a name="handling-network-outages"></a>Behandeln von Netzwerkausfällen

In app-Einkäufe benötigen eine funktionierende Netzwerkverbindung für StoreKit mit Apple Server kommunizieren. Wenn eine Netzwerkverbindung nicht verfügbar ist, dann sind Erwerb von in-app nicht verfügbar.

### <a name="product-requests"></a>Produkt-Anforderungen

Wenn das Netzwerk beim Erstellen nicht mehr verfügbar ist ein `SKProductRequest`, die `RequestFailed` Methode der `SKProductsRequestDelegate` Unterklasse ( `InAppPurchaseManager`) aufgerufen, wie unten dargestellt:

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

Die ViewController klicken Sie dann für die Benachrichtigung überwacht und zeigt eine Meldung in der Bestellung Schaltflächen:

```csharp
requestObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerRequestFailedNotification,
(notification) => {
   Console.WriteLine ("Request Failed");
   buy5Button.SetTitle ("Network down?", UIControlState.Disabled);
   buy10Button.SetTitle ("Network down?", UIControlState.Disabled);
});
```

Da eine Netzwerkverbindung auf mobilen Geräten vorübergehenden sein kann, können Anwendungen mithilfe des SystemConfiguration-Frameworks Netzwerkstatus überwachen möchten und versuchen Sie erneut, wenn eine Netzwerkverbindung verfügbar ist. Finden Sie in der Apple- oder die, die es verwendet.

### <a name="purchase-transactions"></a>Erwerben von Transaktionen

Die StoreKit Zahlung Warteschlange speichert und forward Kauf fordert Wenn möglich, damit die Auswirkungen von einem Netzwerkausfall abhängig variieren, wenn das Netzwerk während des Kaufvorgangs fehlgeschlagen ist.   
   
   
   
 Wenn ein Fehler während einer Transaktion auftritt der `SKPaymentTransactionObserver` Unterklasse ( `CustomPaymentObserver`) müssen die `UpdatedTransactions` Methode mit dem Namen und die `SKPaymentTransaction` Klasse wird der Status "fehlerhaft" sein.

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

Die `FailedTransaction` -Methode erkennt, ob der Fehler durch Benutzer – Abbruch, wie hier gezeigt:

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

Auch wenn eine Transaktion fehl, die `FinishTransaction` -Methode muss aufgerufen werden, um die Transaktion aus der Warteschlange für die Zahlung zu entfernen:

```csharp
SKPaymentQueue.DefaultQueue.FinishTransaction(transaction);
```

Der Beispielcode sendet dann eine Benachrichtigung, damit die ViewController eine Meldung anzeigen kann. Anwendungen sollten keinen weiteren anzeigen angezeigt, wenn der Benutzer die Transaktion abgebrochen. Schließen andere Fehlercodes, die auftreten können:

```csharp
FailedTransaction Code=0 Cannot connect to iTunes Store
FailedTransaction Code=5002 An unknown error has occurred
FailedTransaction Code=5020 Forget Your Password?
Applications may detect and respond to specific error codes, or handle them in the same way.
```

## <a name="handling-restrictions"></a>Behandlung von Einschränkungen

Die **Einstellungen > Allgemein > Einschränkungen** Funktion e/as können Benutzer bestimmte Features des Geräts.   
   
   
   
 Sie können Abfragen, ob der Benutzer in der app-Käufen über die `SKPaymentQueue.CanMakePayments` Methode. Wenn dies auf "false" zurückgibt kann Benutzer in der app zu erwerben nicht zugreifen. StoreKit wird automatisch für den Benutzer eine Fehlermeldung angezeigt, wenn, ein Kauf versucht wird. Anhand dieses Werts kann Ihre Anwendung stattdessen Kauf-Schaltflächen ausblenden oder eine andere Aktion, die den Benutzer helfen.   
   
   
   
 In der `InAppPurchaseManager.cs` Datei die `CanMakePayments` Methode bindet StoreKit Samplingwerte wie folgt:

```csharp
public bool CanMakePayments()
{
   return SKPaymentQueue.CanMakePayments;
}
```

Um diese Methode zu testen, verwenden die **Einschränkungen** Feature e/as deaktivieren **In App-Einkäufe**:   
   
   
   
 [![Verwenden Sie die Einschränkungen-Funktion von iOS In App-Einkäufe deaktivieren](purchasing-consumable-products-images/image31.png)](purchasing-consumable-products-images/image31.png#lightbox)   
   
   
   
 Dieser Beispielcode aus `ConsumableViewController` antwortet auf `CanMakePayments` "false" zurückgeben, indem anzeigen **AppStore deaktiviert** Text auf den deaktivierten Schaltflächen.

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

Die Anwendung sieht, wie diese Lösung, wenn die **In App-Einkäufe** Funktion beschränkt ist – die Bestellung Schaltflächen sind deaktiviert.   
   
   
   
 [![Die Anwendung sieht wie folgt, wenn die Funktion ist In-App-Käufe Kauf beschränkt, die Schaltflächen deaktiviert sind](purchasing-consumable-products-images/image32.png)](purchasing-consumable-products-images/image32.png#lightbox)   
   
   
   

Produktinformationen kann immer noch erforderlich wenn `CanMakePayments` ist "false", damit die app immer noch abrufen und Preise anzeigen kann. Dies bedeutet, wenn wir entfernt die `CanMakePayments` Kontrollkästchen aus dem Code, der die Bestellung Schaltflächen würde weiterhin aktiv sein, aber wenn, ein Kauf versucht wird dem Benutzer eine Meldung angezeigt wird, **In app-Einkäufe sind nicht zulässig.** (generiert durch StoreKit Wenn die Zahlung Warteschlange zugegriffen wird):   
   
   
   
 [![In app-Einkäufe sind nicht zulässig.](purchasing-consumable-products-images/image33.png)](purchasing-consumable-products-images/image33.png#lightbox)   
   
   
   
 Praktische Anwendungen können einen anderen Ansatz für die Behandlung der Einschränkung, z. B. die Schaltflächen vollständig ausblenden und vielleicht bietet eine detailliertere Nachricht als die Warnung, die StoreKit automatisch zeigt dauern.

