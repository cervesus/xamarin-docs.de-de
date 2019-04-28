---
title: Kaufen von Verbrauchsartikeln in Xamarin.iOS
description: Dieses Dokument beschreibt verbrauchsartikeln in Xamarin.iOS. Verbrauchsartikeln sind einmaligen Verwendung von Funktionen, z. B. im Spiel Währung.
ms.prod: xamarin
ms.assetid: E0CB4A0F-C3FA-3933-58A7-13246971D677
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: b55465a700974e0ce5ceb8893d96311d920e04ae
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61366570"
---
# <a name="purchasing-consumable-products-in-xamarinios"></a>Kaufen von Verbrauchsartikeln in Xamarin.iOS

Verbrauchsartikeln sind am einfachsten zu implementieren, da keine Notwendigkeit 'Wiederherstellen besteht'. Sie sind hilfreich für Produkte wie in-Game-Währung oder einen Teil der einmaligen Verwendung der Funktionalität. Verbrauchsartikeln-Over und Failover-von-Benutzer können erneut erneut erwerben.

## <a name="built-in-product-delivery"></a>Integrierte Produktlieferung

Im Beispielcode zu diesem Dokument wird veranschaulicht, integrierte Produkte: die Produkt-IDs sind hartcodiert die Anwendung aus, da sie auf den Code eng zusammenhängen, die "nach der Zahlung das Feature entsperrt". Erwerb des kann wie folgt visuell dargestellt werden:   
   
[![Der Erwerb Prozess-Visualisierung](purchasing-consumable-products-images/image26.png)](purchasing-consumable-products-images/image26.png#lightbox)     
   
 Der grundlegende Workflow ist:   
   
 1. Die app Fügt eine `SKPayment` an die Warteschlange. Bei Bedarf der Benutzer wird aufgefordert, ihre Apple-ID, und aufgefordert, die Zahlung zu bestätigen.   
   
 2. StoreKit sendet die Anforderung zur Verarbeitung an den Server an.   
   
 3. Wenn die Transaktion abgeschlossen ist, antwortet der Server, mit der eine Bestätigung der Transaktion.   
   
 4. Die `SKPaymentTransactionObserver` Unterklasse die Bestätigung empfängt und verarbeitet es.   
   
 5. Die Anwendung kann das Produkt (durch die Aktualisierung `NSUserDefaults` oder eines anderen Mechanismus), und ruft dann die StoreKit `FinishTransaction`.

Es gibt eine andere Art von Workflow – *Server-Delivered Produkte* – d. h. später in diesem Dokument erläutert (finden Sie im Abschnitt *Empfang, Überprüfung und Server-Delivered Produkte*).

## <a name="consumable-products-example"></a>Beispiel für die Produkte genutzt werden

Die [InAppPurchaseSample Code](https://developer.xamarin.com/samples/monotouch/StoreKit/) enthält ein Projekt namens *Verbrauchsmaterialien* , der eine einfache 'in-Game Currency' ("Monkey Gutschriften" genannt) implementiert. Das Beispiel zeigt die Implementierung von in-app-Käufe-Produkten, damit die Benutzer erwerben, wie viele "Monkey Guthaben" wie gewünscht – in einer echten Anwendung eine Möglichkeit wird, diese Ausgaben!   
   
   
   
 Die Anwendung wird in diese Screenshots gezeigt – jeder Kauf des Benutzers Saldo "Monkey Gutschriften" hinzugefügt:   
   
   
   
 [![Die erworbenen fügt Monkey-Gutschriften zum Saldo für Benutzer](purchasing-consumable-products-images/image27.png)](purchasing-consumable-products-images/image27.png#lightbox)   
   
   
   
 Die Interaktionen zwischen benutzerdefinierten Klassen StoreKit und die Store-App wird wie folgt aussehen:   
   
   
   
 [![Die Interaktionen zwischen benutzerdefinierten Klassen StoreKit und den App Store](purchasing-consumable-products-images/image28.png)](purchasing-consumable-products-images/image28.png#lightbox)

&nbsp;

### <a name="viewcontroller-methods"></a>ViewController-Methoden

Zusätzlich zu den Eigenschaften und Methoden, die erforderlich sind, um Produktinformationen abzurufen muss der ansichtscontroller zusätzliche Benachrichtigung Observer-Objekte zum Abhören von Purchase-bezogenen Benachrichtigungen. Hierbei handelt es sich einfach `NSObjects` , registriert und in entfernt `ViewWillAppear` und `ViewWillDisappear` bzw.

```csharp
NSObject succeededObserver, failedObserver;
```

Der Konstruktor erstellt auch die `SKProductsRequestDelegate` Unterklasse ( `InAppPurchaseManager`), die wiederum erstellt und registriert die `SKPaymentTransactionObserver` ( `CustomPaymentObserver`).   
   
   
   
 Der erste Teil der Verarbeitung einer Transaktions in app-Käufe ist das Drücken der Schaltfläche zu behandeln, wenn der Benutzer erwerben, möchte wie im folgenden Code aus der beispielanwendung dargestellt:

```csharp
buy5Button.TouchUpInside += (sender, e) => {
   iap.PurchaseProduct (Buy5ProductId);
};
buy10Button.TouchUpInside += (sender, e) => {
   iap.PurchaseProduct (Buy10ProductId);
};
```

   
   
 Der zweite Teil der Benutzeroberfläche behandelt wird die Benachrichtigung, dass die Transaktion in diesem Fall erfolgreich ausgeführt wurde, indem Sie den angezeigten Kontostand zu aktualisieren:

```csharp
priceObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerTransactionSucceededNotification,
(notification) => {
   balanceLabel.Text = CreditManager.Balance() + " monkey credits";
});
```

Der letzte Teil der Benutzeroberfläche wird eine Meldung angezeigt, wenn eine Transaktion aus irgendeinem Grund abgebrochen wird. Im Beispielcode wird eine Meldung im Ausgabefenster einfach aufgezeichnet:

```csharp
failedObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerTransactionFailedNotification,
(notification) => {
   Console.WriteLine ("Transaction Failed");
});
```

Neben diesen Methoden für den ansichtscontroller, erfordert einen verwendbaren Produkt Kauftransaktion in Kraft auch Code für die `SKProductsRequestDelegate` und `SKPaymentTransactionObserver`.

### <a name="inapppurchasemanager-methods"></a>InAppPurchaseManager-Methoden

Der Code implementiert eine Reihe von erwerben verwandte Methoden für die InAppPurchaseManager-Klasse, einschließlich der `PurchaseProduct` Methode, die erstellt eine `SKPayment` -Instanz und fügt es in die Warteschlange für die Verarbeitung:

```csharp
public void PurchaseProduct(string appStoreProductId)
{
   SKPayment payment = SKPayment.PaymentWithProduct (appStoreProductId);
   SKPaymentQueue.DefaultQueue.AddPayment (payment);
}
```

Hinzufügen der Zahlungsausgangs an die Warteschlange ist ein asynchroner Vorgang. Die Anwendung erhält Steuerelement wieder, während StoreKit die Transaktion verarbeitet und sie an Apple Server sendet. Es ist an diesem Punkt, dass der Benutzer auf den App Store angemeldet ist und sie ein Apple-ID und ein Kennwort aufgefordert werden, falls erforderlich, iOS überprüft.   
   
   
   
 Wenn der Benutzer erfolgreich authentifiziert sich mit den App Store und der Transaktion stimmt die `SKPaymentTransactionObserver` StoreKit Antwort empfangen wird, und rufen Sie die folgende Methode, um die Transaktion zu erfüllen, und beenden sie.

```csharp
public void CompleteTransaction (SKPaymentTransaction transaction)
{
   var productId = transaction.Payment.ProductIdentifier;
   // Register the purchase, so it is remembered for next time
   PhotoFilterManager.Purchase(productId);
   FinishTransaction(transaction, true);
}
```

Der letzte Schritt besteht darin sicherzustellen, dass StoreKit benachrichtigt, dass Sie die Transaktion erfolgreich durch den Aufruf erfüllt haben `FinishTransaction`:

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

Nach der Auslieferung des Produkts, `SKPaymentQueue.DefaultQueue.FinishTransaction` aufgerufen werden, um die Transaktion aus der Warteschlange für die Zahlung zu entfernen.

### <a name="skpaymenttransactionobserver-custompaymentobserver-methods"></a>SKPaymentTransactionObserver (CustomPaymentObserver)-Methoden

StoreKit Aufrufe der `UpdatedTransactions` Methode, wenn sie eine Antwort von Apple Server empfängt, und ein Array von übergibt `SKPaymentTransaction` Objekte für Ihren Code, um zu überprüfen. Die Methode durchläuft jede Transaktion und führt eine andere Funktion, die basierend auf den Transaktionsstatus, (wie hier gezeigt):

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

Die `CompleteTransaction` Methode wurde zuvor in diesem Abschnitt behandelten – speichert die Details der Bestellung, `NSUserDefaults`, schließt die Transaktion mit StoreKit ab, und schließlich benachrichtigt die Benutzeroberfläche zu aktualisieren.

### <a name="purchasing-multiple-products"></a>Mehrere Produkte erwerben

Wenn es in der Anwendung für mehrere Produkte erwerben sinnvoll ist, verwenden Sie die `SKMutablePayment` Klasse, und legen Sie das Feld Menge:

```csharp
public void PurchaseProduct(string appStoreProductId)
{
   SKMutablePayment payment = SKMutablePayment.PaymentWithProduct (appStoreProductId);
   payment.Quantity = 4; // hardcoded as an example
   SKPaymentQueue.DefaultQueue.AddPayment (payment);
}
```

Der Code zur Behandlung der abgeschlossenen Transaktions muss auch Abfragen, die Menge-Eigenschaft, um den Kauf ordnungsgemäß zu erfüllen:

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

Wenn der Benutzer mehrere erwirbt, spiegeln StoreKit Bestätigung Warnung die Menge, den Einzelpreis und der Gesamtpreis, die, den Sie in Rechnung gestellt werden müssen, den wie im folgenden Screenshot gezeigt:

[![Kauf bestätigen](purchasing-consumable-products-images/image30.png)](purchasing-consumable-products-images/image30.png#lightbox)

## <a name="handling-network-outages"></a>Behandlung von Netzwerkausfällen

In-app-Käufe benötigen eine funktionierende Netzwerkverbindung für StoreKit mit Apple Servern kommunizieren. Wenn eine Netzwerkverbindung nicht verfügbar ist, wird dann in-app-Käufe nicht möglich.

### <a name="product-requests"></a>Produkt-Anforderungen

Wenn das Netzwerk nicht verfügbar, bei der Erstellung ist ein `SKProductRequest`, wird die `RequestFailed` -Methode der der `SKProductsRequestDelegate` Unterklasse ( `InAppPurchaseManager`) wird aufgerufen, wie unten dargestellt:

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

Der ViewController klicken Sie dann wartet auf die Benachrichtigung und zeigt eine Meldung in die Schaltflächen erwerben:

```csharp
requestObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerRequestFailedNotification,
(notification) => {
   Console.WriteLine ("Request Failed");
   buy5Button.SetTitle ("Network down?", UIControlState.Disabled);
   buy10Button.SetTitle ("Network down?", UIControlState.Disabled);
});
```

Da eine Netzwerkverbindung auf mobilen Geräten vorübergehend sein kann, können Anwendungen mithilfe des SystemConfiguration-Frameworks Netzwerkstatus überwachen möchten und versuchen Sie erneut, wenn eine Netzwerkverbindung verfügbar ist. Finden Sie im Apple oder den, der es verwendet.

### <a name="purchase-transactions"></a>Erwerben von Transaktionen

Die StoreKit Zahlung Warteschlange speichert und forward Kauf angeforderte Wenn möglich, die Auswirkungen von einem Ausfall des Netzwerks abhängig variieren, wenn das Netzwerk während des Kaufvorgangs fehlgeschlagen ist.   
   
   
   
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

Die `FailedTransaction` Methode erkennt, ob der Fehler durch Benutzer-Abbruch, wie hier gezeigt:

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

Auch wenn eine Transaktion ein Fehler auftritt, die `FinishTransaction` Methode aufgerufen werden, um die Transaktion aus der Warteschlange für die Zahlung zu entfernen:

```csharp
SKPaymentQueue.DefaultQueue.FinishTransaction(transaction);
```

Der Beispielcode sendet dann eine Benachrichtigung, so, dass der ViewController eine Meldung angezeigt werden kann. Anwendungen sollten nicht mehr ein zusätzliches anzeigen Nachricht, wenn der Benutzer die Transaktion abgebrochen. Sonstige Fehlercodes, die auftreten können, gehören:

```csharp
FailedTransaction Code=0 Cannot connect to iTunes Store
FailedTransaction Code=5002 An unknown error has occurred
FailedTransaction Code=5020 Forget Your Password?
Applications may detect and respond to specific error codes, or handle them in the same way.
```

## <a name="handling-restrictions"></a>Behandeln von Einschränkungen

Die **Einstellungen > Allgemein > Einschränkungen** Feature der e/a können Benutzer bestimmte Features von ihrem Gerät zu sperren.   
   
   
   
 Sie können Abfragen, ob der Benutzer darf in-app-Käufen über den `SKPaymentQueue.CanMakePayments` Methode. Wenn der Rückgabewert false kann Benutzer in-app-Käufe nicht zugreifen. StoreKit wird automatisch für den Benutzer eine Fehlermeldung angezeigt, wenn, ein Kauf versucht wird. Anhand dieses Werts kann Ihre Anwendung stattdessen den Kauf Schaltflächen ausblenden oder eine andere Aktion, die den Benutzer helfen.   
   
   
   
 In der `InAppPurchaseManager.cs` Datei die `CanMakePayments` Methode umschließt die StoreKit-Funktion wie folgt:

```csharp
public bool CanMakePayments()
{
   return SKPaymentQueue.CanMakePayments;
}
```

Verwenden Sie zum Testen dieser Methode die **Einschränkungen** Feature der e/a deaktivieren **In-App-Käufe**:   
   
   
   
 [![Verwenden Sie das Feature der Einschränkungen der e/a, um In-App-Käufe deaktivieren](purchasing-consumable-products-images/image31.png)](purchasing-consumable-products-images/image31.png#lightbox)   
   
   
   
 Dieser Beispielcode aus `ConsumableViewController` reagiert auf `CanMakePayments` "false" zurückgeben, indem anzeigen **AppStore deaktiviert** Text auf die deaktivierten Schaltflächen.

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

Die Anwendung sieht aus wie beschrieben vor, wenn die **In-App-Käufe** Funktion beschränkt ist – die Kauf Schaltflächen werden deaktiviert.   
   
   
   
 [![Die Anwendung sieht wie folgt aus, wenn die Funktion ist In-App-Käufe den Einkauf beschränkt, die Schaltflächen deaktiviert sind](purchasing-consumable-products-images/image32.png)](purchasing-consumable-products-images/image32.png#lightbox)   
   
   
   

Produktinformationen kann weiterhin werden angefordert, wenn `CanMakePayments` ist "false", damit die app immer noch abrufen und die Preise anzuzeigen. Dies bedeutet, wenn es entfernt die `CanMakePayments` Kontrollkästchen aus dem Code, die Kauf Schaltflächen würden immer noch, aktiv sein, aber wenn, ein Kauf versucht wird dem Benutzer eine Meldung angezeigt wird, die **In-app-Einkäufe sind nicht zulässig.** (generiert durch StoreKit Wenn die Warteschlange für die Zahlung erfolgt):   
   
   
   
 [![In-app-Einkäufe sind nicht zulässig.](purchasing-consumable-products-images/image33.png)](purchasing-consumable-products-images/image33.png#lightbox)   
   
   
   
 Reale Anwendungen dauert einen anderen Ansatz zur Behandlung der Einschränkung, wie z. B. vollständig durch Ausblenden der Schaltflächen und bietet möglicherweise eine weitere ausführliche Meldung als Warnung, die StoreKit automatisch zeigt.

