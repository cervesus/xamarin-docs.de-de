---
title: Transaktionen und Überprüfung in Xamarin.iOS
description: Dieses Dokument beschreibt, wie Sie die Wiederherstellung von vorherigen Produktkäufen in einer Xamarin.iOS-app zu ermöglichen. Darüber hinaus werden die Methoden zum Sichern der Einkäufe und Server bereitgestellte Produkte erläutert.
ms.prod: xamarin
ms.assetid: 84EDD2B9-3FAA-B3C7-F5E8-C1E5645B7C77
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 83f5fd233c004271169a4d00d0a65e70aa925b95
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50117654"
---
# <a name="transactions-and-verification-in-xamarinios"></a>Transaktionen und Überprüfung in Xamarin.iOS

## <a name="restoring-past-transactions"></a>Wiederherstellen nach Transaktionen

Wenn Ihre Anwendung Produkttypen, die wiederhergestellt werden unterstützt, müssen Sie einige Benutzeroberflächenelemente, damit Benutzer diese Einkäufe wiederherstellen einschließen.
Diese Funktionalität ermöglicht es, ein Kunde das Produkt für zusätzliche Geräte hinzufügen oder das Produkt auf dasselbe Gerät wiederherstellen, nach dem Bereinigen zurücksetzen oder entfernen und die app neu zu installieren. Die folgenden Produkttypen werden wiederhergestellt:

-  Nicht-verbrauchsartikeln
-  Automatische erneuerbar Abonnements
-  Kostenlose Abonnements

Der Wiederherstellungsvorgang sollte die Datensätze zu aktualisieren Sie bleiben auf dem Gerät, um Ihre Produkte zu erfüllen. Kunden kann auch jederzeit auf allen Geräten wiederherstellen. Der Wiederherstellungsvorgang sendet erneut alle vorherige Transaktionen für diesen Benutzer. der Anwendungscode muss auszuführende Aktion an, mit diesen Informationen (z. B. überprüfen, wenn bereits eine Aufzeichnung der diesen Erwerb auf dem Gerät vorhanden ist, und wenn Sie keinen, einen Datensatz mit den Kauf erstellen und aktivieren das Produkt für den Benutzer), klicken Sie dann bestimmen.

### <a name="implementing-restore"></a>Implementieren die Wiederherstellung

Die Benutzeroberfläche **wiederherstellen** Schaltfläche ruft die folgende Methode, die RestoreCompletedTransactions wird ausgelöst, auf die `SKPaymentQueue`.

```csharp
public void Restore()
{
   // theObserver will be notified of when the restored transactions start arriving <- AppStore
   SKPaymentQueue.DefaultQueue.RestoreCompletedTransactions();
}
```

StoreKit wird asynchron die Anforderung zum Wiederherstellen an Apple Server senden.   
   
Da die `CustomPaymentObserver` registriert ist als eine Transaktion Observer, erhält er Nachrichten bei der Apple Server reagieren. Die Antwort enthält alle Transaktionen, die dieser Benutzer hat bisher in dieser Anwendung (auf allen Geräten) durchgeführt wurde. Der Code durchläuft jede Transaktion erkennt den Status des wiederhergestellt und die Aufrufe der `UpdatedTransactions` Methode, um es zu verarbeiten, wie unten dargestellt:

```csharp
// called when the transaction status is updated
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
       case SKPaymentTransactionState.Restored:
           theManager.RestoreTransaction(transaction);
           break;
default:
           break;
       }
   }
}
```

Wenn es keine wiederherstellbaren Produkte für den Benutzer gibt `UpdatedTransactions` wird nicht aufgerufen.   
   
Mögliche ganz einfachen Code zum Wiederherstellen einer bestimmten Transaktions in diesem Beispiel wird die gleichen Aktionen wie beim Kauf, außer dass erfolgt die `OriginalTransaction` Eigenschaft wird verwendet, um die Produkt-ID zugreifen:

```csharp
public void RestoreTransaction (SKPaymentTransaction transaction)
{
   // Restored Transactions always have an 'original transaction' attached
   var productId = transaction.OriginalTransaction.Payment.ProductIdentifier;
   // Register the purchase, so it is remembered for next time
   PhotoFilterManager.Purchase(productId); // it's as though it was purchased again
   FinishTransaction(transaction, true);
}
```

Eine komplexere Implementierung kann überprüfen, andere `transaction.OriginalTransaction` Eigenschaften, z. B. die ursprüngliche Anzahl von Datum und die Bestätigung. Diese Informationen werden für einige Produkttypen (z. B. Abonnements) hilfreich sein.

#### <a name="restore-completion"></a>Wiederherstellen der Vervollständigung

Die `CustomPaymentObserver` verfügt über zwei weitere Methoden, die von StoreKit aufgerufen werden, wenn der Wiederherstellungsvorgang abgeschlossen ist, (erfolgreich oder mit einem Fehler), unten:

```csharp
public override void PaymentQueueRestoreCompletedTransactionsFinished (SKPaymentQueue queue)
{
   Console.WriteLine(" ** RESTORE Finished ");
}
public override void RestoreCompletedTransactionsFailedWithError (SKPaymentQueue queue, NSError error)
{
   Console.WriteLine(" ** RESTORE FailedWithError " + error.LocalizedDescription);
}
```

Im Beispiel nichts Unternehmen diese Methoden, aber eine echte Anwendung entscheiden, um eine Nachricht an den Benutzer oder andere Funktionalität zu implementieren.

## <a name="securing-purchases"></a>Sichern von Käufe

Die beiden Beispiele in diesem Dokument `NSUserDefaults` Einkäufe nachverfolgt:   
   
 **Verbrauchsgüter** – das Gleichgewicht der Gutschrift Einkäufe ist eine einfache `NSUserDefaults` ganzzahliger Wert, der mit jeder Kauf erhöht wird.   
   
 **Nicht-Verbrauchsmaterialien** – befindet sich jeder Kauf der Foto-Filter als Schlüssel-Wert-Paar im `NSUserDefaults`.

Mithilfe von `NSUserDefaults` beibehalten des Beispiels den Code einfach, aber die bietet keiner sehr sicheren Lösung, da es möglicherweise technisch orientierte Benutzer zum Aktualisieren der Einstellungen (den Zahlungsverkehrsmechanismus wird dadurch umgangen).   
   
Hinweis: Der realen Anwendungen sollten Übernehmen eines sicheren Mechanismus zum Speichern von erworbenen Inhalt, der kein Benutzer leichter manipuliert ist. Dies kann die Verschlüsselung bzw. andere Techniken, einschließlich Remote-Server-Authentifizierung umfassen.   
   
 Der Mechanismus sollten auch die integrierten Features Sicherung und Wiederherstellung von iOS-, iTunes und iCloud nutzen entworfen werden. Dadurch wird sichergestellt, nachdem ein Benutzer eine Sicherung wiederhergestellt wird ihre vorherige Einkäufe sofort zur Verfügung stehen.   
   
Finden Sie in Apple Secure Coding Guide Weitere iOS-spezifische Richtlinien.

## <a name="receipt-verification-and-server-delivered-products"></a>Receipt-Überprüfung und bereitgestellte Server-Produkte

In die Beispielen in diesem Dokument bisher haben umfasste, ausschließlich der Anwendung direkt mit den App Store-Servern kommuniziert, kaufvorgängen, führen Sie, die Entsperren von Features oder Funktionen, die in die app bereits codiert.   
   
Apple bietet für eine zusätzliche Sicherheitsstufe Kauf-Konfigurationsrichtlinien Einkaufsbelege unabhängig von einem anderen Server überprüft werden, die für eine Anforderung zu überprüfen, bevor Sie zum Bereitstellen von digitalen Inhalten als Teil des einen Kauf hilfreich sein können (z. B. ein Buch digitale oder Magazine).   
   
 **Integrierte Produkte** – wie in den Beispielen in diesem Dokument, an, die für das Produkt erworben werden als Funktionen, die im Lieferumfang der Anwendungs vorhanden ist. Ein in-app-Einkauf kann der Benutzer Zugriff auf die Funktionen an.
Produkt-IDs sind hartcodiert.   
   
 **Server bereitgestellte Produkte** : das Produkt besteht aus herunterladbare Inhalte, die auf einem Remoteserver gespeichert wird, bis eine erfolgreiche Transaktion führt dazu, dass den Inhalt heruntergeladen werden.
Bücher oder magazine-Ausgaben Beispiele. Produkt-IDs werden in der Regel von einem externen Server stammen (, wo der Produktinhalt auch gehostet wird). Anwendungen müssen eine robuste Möglichkeit, wenn eine Transaktion abgeschlossen ist, aufzeichnen, sodass Wenn der download von Inhalt ein Fehler auftritt er erneut versucht werden kann, ohne den Benutzer verwirrend implementieren.

### <a name="server-delivered-products"></a>Bereitgestellte Server-Produkte

Einige Produkte ist Inhalt, z. B. Büchern und Zeitschriften (oder sogar eine Spiele-Ebene) von einem Remoteserver während des Kaufvorgangs heruntergeladen werden muss. Dies bedeutet, dass ein zusätzlicher Server erforderlich ist, speichern und die Produkt-Inhalte übermittelt werden, nachdem es erworben wird.

#### <a name="getting-prices-for-server-delivered-products"></a>Abrufen der Preise für bereitgestellte Server-Produkte

Da die Produkte Remote geliefert werden, ist es auch möglich, weitere Produkte im Lauf der Zeit hinzuzufügen (ohne aktualisieren den app-Code), z. B. das Hinzufügen weitere Bücher oder neue Probleme der ein Magazin. Damit die Anwendung kann diese News-Produkte zu entdecken und sie für den Benutzer anzeigen, die zusätzliche Server speichert und diese Informationen liefern.   
   
[![](transactions-and-verification-images/image38.png "Abrufen der Preise für bereitgestellte Server-Produkte")](transactions-and-verification-images/image38.png#lightbox)   
   
1. Produktinformationen muss an mehreren Orten gespeichert werden: auf dem Server und in iTunes Connect. Darüber hinaus wird jedes Produkt Inhaltsdateien zugeordnet haben. Diese Dateien werden nach einem Kauf erfolgreich übermittelt.   
   
2. Wenn der Benutzer ein Produkt kaufen möchte, muss die Anwendung ermitteln, welche Produkte verfügbar sind. Diese Informationen kann zwischengespeichert werden, aber von einem Remoteserver, in dem die Masterliste der Produkte gespeichert wurde, gesendet werden soll.   
   
3. Der Server gibt eine Liste der Produkt-IDs für die Anwendung zu analysieren.   
   
4. Klicken Sie dann die Anwendung bestimmt, welche diesen Produkt-IDs an StoreKit zum Abrufen von Preisen und Beschreibungen.   
   
5. StoreKit sendet die Liste der Produkt-IDs an Apple Server.   
   
6. Die iTunes-Server reagiert mit gültigen Produktinformationen (Beschreibung und aktueller Preis).   
   
7. Der Anwendung `SKProductsRequestDelegate` die Produktinformationen für die Anzeige für dem Benutzer übergeben wird.

#### <a name="purchasing-server-delivered-products"></a>Erwerben bereitgestellte Server-Produkte

Weil der Remoteserver einige erfordert, überprüfen, dass eine inhaltsanforderung gültig ist (d. h. für bezahlt wurde), die Receipt-Informationen für die Authentifizierung übergeben. Der remote-Server leitet die Daten in iTunes für die Überprüfung und bei erfolgreicher Ausführung enthält den Produktinhalt in der Antwort an die Anwendung.   
   
 [![](transactions-and-verification-images/image39.png "Erwerben bereitgestellte Server-Produkte")](transactions-and-verification-images/image39.png#lightbox)   
   
1. Die app Fügt eine `SKPayment` an die Warteschlange. Bei Bedarf der Benutzer wird aufgefordert, ihre Apple-ID, und aufgefordert, die Zahlung zu bestätigen.   
   
2. StoreKit sendet die Anforderung zur Verarbeitung an den Server an.   
   
3. Wenn die Transaktion abgeschlossen ist, antwortet der Server, mit der eine Bestätigung der Transaktion.   
   
4. Die `SKPaymentTransactionObserver` Unterklasse die Bestätigung empfängt und verarbeitet es. Da das Produkt von einem Server heruntergeladen werden muss, leitet die Anwendung die Anforderung für eine Netzwerksicherheitsgruppe mit dem Remoteserver.   
   
5. Die downloadanforderung wird durch die Receipt-Daten begleitet, damit der Remoteserver überprüfen kann, dass er Zugriff auf den Inhalt berechtigt ist. Client der Anwendung Netzwerk wartet auf eine Antwort auf diese Anforderung.   
   
6. Wenn der Server eine Anforderung für den Inhalt erhält, die Receipt-Daten analysiert und sendet eine Anforderung direkt an die iTunes-Server, um zu überprüfen, ob die Bestätigung für eine Transaktion gültig ist. Der Server sollte eine Logik verwenden, um zu bestimmen, ob die Anforderung an die Produktion oder Sandbox-URL senden. Apple empfiehlt, immer mithilfe der URL für die Produktion und für das Sandboxing wechseln, wenn Ihr Status 21007 (Sandbox-Bestätigung gesendet wird, auf dem Produktionsserver) erhalten. Finden Sie in Apple [Receipt-Programmierhandbuch für die Validierung](https://developer.apple.com/library/archive/releasenotes/General/ValidateAppStoreReceipt/Chapters/ValidateRemotely.html) Weitere Details.
   
7. iTunes überprüft den Empfang und Status von 0 (null) zurück, wenn er gültig ist.   
   
8. Der Server wartet auf Antwort von iTunes. Wenn sie eine gültige Antwort empfängt, sollte der Code die zugehörigen Produkt-Inhaltsdatei sollen in der Antwort an die Anwendung navigieren.   
  
9. Die Anwendung empfängt und analysiert die Antwort, die Produktinhalt des Geräts Dateisystem zu speichern.   
   
10. Die Anwendung können Sie das Produkt, und ruft dann die StoreKit `FinishTransaction`. Die Anwendung kann dann optional den erworbenen Inhalt (z. B. Anzeigen der ersten Seite des erworbenen Bücher oder magazine Problem) anzeigen.

Eine alternative Implementierung für sehr große Produkt Inhaltsdateien kann vorsehen, dass die Bestätigung für die Transaktion einfach in Schritt #9 zu speichern, damit, dass die Transaktion schnell abgeschlossen werden kann und Bereitstellen einer Benutzeroberfläche für den Benutzer zum Herunterladen des Inhalts der tatsächlichen product zu einem späteren Zeitpunkt. Die nachfolgenden downloadanforderung kann gespeicherte Kaufbelegs an den Zugriff auf die erforderliche Product-Content-Datei erneut zu senden.

### <a name="writing-server-side-receipt-verification-code"></a>Schreiben von Code für serverseitige Receipt-Überprüfung

Überprüfen eine Bestätigung im serverseitigen Code kann mit einer einfachen HTTP-POST-Anforderung/Antwort durchgeführt werden, die Schritte #5 bis 8, # im Workflowdiagramm umfasst.   
   
Extrahieren Sie die `SKPaymentTansaction.TransactionReceipt` Eigenschaft in der app. Dies ist die Daten, die in iTunes für die Überprüfung (Schritt #5) gesendet werden müssen.

Base64-codiert die Receipt-Transaktionsdaten (entweder in Schritt #5 oder #6).

Erstellen Sie eine einfache JSON-Nutzlast wie folgt:

```csharp
{
   "receipt-data" : "(base-64 encoded receipt here)"
}
```

HTTP POST JSON in [ https://buy.itunes.apple.com/verifyReceipt ](https://buy.itunes.apple.com/verifyReceipt) für die Produktion oder [ https://sandbox.itunes.apple.com/verifyReceipt ](https://sandbox.itunes.apple.com/verifyReceipt) zum Testen.   
   
 Die JSON-Antwort enthält die folgenden Schlüssel:

```csharp
{
   "status" : 0,
   "receipt" : { (receipt repeated here) }
}
```

Status von 0 (null) gibt eine gültige Bestätigung an. Ihr Server kann mit Inhalt für das erworbene Produkt erfüllen fortfahren. Der Schlüssel für die Bestätigung enthält ein JSON-Wörterbuch mit den gleichen Eigenschaften wie die `SKPaymentTransaction` -Objekt, das von der Anwendung empfangen wurde, damit der Servercode dieses Wörterbuch zum Abrufen von Informationen wie z. B. die "product_id" und die Menge der Bestellung abgefragt werden kann.

Finden Sie unter Apple [Receipt-Programmierhandbuch für die Validierung](https://developer.apple.com/library/archive/releasenotes/General/ValidateAppStoreReceipt/Introduction.html) Dokumentation für zusätzliche Informationen.
