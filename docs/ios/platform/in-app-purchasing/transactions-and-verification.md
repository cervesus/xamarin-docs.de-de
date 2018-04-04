---
title: Transaktionen und Überprüfung
ms.prod: xamarin
ms.assetid: 84EDD2B9-3FAA-B3C7-F5E8-C1E5645B7C77
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: c8d86d0ce3119b3e104a65a170ab141484af44a7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="transactions-and-verification"></a>Transaktionen und Überprüfung

## <a name="restoring-past-transactions"></a>Wiederherstellen nach Transaktionen

Wenn Ihre Anwendung Produkttypen, die wiederhergestellt werden unterstützt, müssen Sie einige Benutzeroberflächenelemente, damit Benutzer diese Einkäufe wiederherstellen können einschließen.
Diese Funktionalität ermöglicht es einen Kunden das Produkt für zusätzliche Geräte hinzufügen oder das Produkt auf einem Gerät wiederherstellen, nach dem Bereinigen zurückgesetzt wird oder entfernen und die app neu zu installieren. Die folgenden Produkttypen werden wiederhergestellt:

-  Produkte nicht verwendbar
-  Automatische erneuerbar Abonnements
-  Kostenlose Abonnements


Der Wiederherstellungsprozess sollten die Datensätze aktualisieren Sie bleiben auf dem Gerät, um Ihre Produkte zu erfüllen. Kunden können jederzeit auf ihre Geräte wiederherstellen. Der Wiederherstellungsvorgang sendet erneut alle vorherige Transaktionen für diesen Benutzer. der Anwendungscode muss dann bestimmen, welche Aktion mit diesen Informationen (z. B. überprüfen, wenn bereits ein Datensatz dar, auf dem Gerät vorhanden ist, und wenn einen Datensatz mit den Kauf nicht der Fall, erstellen und aktivieren das Produkt für den Benutzer).

### <a name="implementing-restore"></a>Implementieren die Wiederherstellung

Die Benutzeroberfläche **wiederherstellen** Schaltfläche ruft die folgende Methode, die RestoreCompletedTransactions wird ausgelöst, auf die `SKPaymentQueue`.

```csharp
public void Restore()
{
   // theObserver will be notified of when the restored transactions start arriving <- AppStore
   SKPaymentQueue.DefaultQueue.RestoreCompletedTransactions();
}
```

StoreKit sendet die Anforderung zum Wiederherstellen asynchron an Apple Server.   
   
   
   
 Da die `CustomPaymentObserver` registriert wird als eine Transaktion "Beobachter", erhält er Nachrichten bei der Apple Server reagieren. Die Antwort enthält alle Transaktionen, die dieser Benutzer in dieser Anwendung jemals (auf allen ihren Geräten) ausgeführt hat. Der Code durchläuft jede Transaktion erkennt den Status wiederhergestellt und ruft die `UpdatedTransactions` Methode, um es zu verarbeiten, wie unten dargestellt:

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

Wenn es keine wiederherstellbaren Produkte für den Benutzer sind `UpdatedTransactions` wird nicht aufgerufen.   
   
   
   
 Die einfachste mögliche Code zum Wiederherstellen einer bestimmten Transaktions im Beispiel werden die gleichen Aktionen wie bei ein Kauf, außer dass stattfindet der `OriginalTransaction` Eigenschaft wird verwendet, um die Produkt-ID zugreifen:

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

Eine komplexere Implementierung möglicherweise andere überprüfen `transaction.OriginalTransaction` Eigenschaften, z. B. die ursprüngliche Nummer für Datum und die Bestätigung. Diese Informationen werden für bestimmte Produkte (z. B. Abonnements) hilfreich.

#### <a name="restore-completion"></a>Stellen Sie die Beendigung wieder her.

Die `CustomPaymentObserver` verfügt über zwei weitere Methoden, die von StoreKit aufgerufen werden, wenn der Wiederherstellungsvorgang abgeschlossen ist, (erfolgreich oder mit einem Fehler), im folgenden gezeigt:

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

Im Beispiel werden diese Methoden keine Aktionen ausgeführt werden, jedoch eine realen Anwendung auswählen kann, um eine Nachricht an den Benutzer oder andere Funktionalität zu implementieren.

## <a name="securing-purchases"></a>Sichern von Käufe

Die beiden Beispiele in diesem Dokument verwenden `NSUserDefaults` um Einkäufe zu verfolgen:   
   
 **Verbrauchsgüter** – das Gleichgewicht der Kreditkarten-Käufe zeigt eine einfache `NSUserDefaults` Integer-Wert, der mit jeder Bestellung erhöht wird.   
   
 **Nicht-Verbrauchsgüter** – jedes Foto Filter Einkauf wird gespeichert als Schlüssel-Wert-Paar in `NSUserDefaults`.

Mithilfe von `NSUserDefaults` behält Sie den Beispielcode einfache, jedoch bietet keine äußerst sichere Lösung, da es möglicherweise technisch-orientierte Benutzer zum Aktualisieren der Einstellungen (die Zahlungsverkehrsmechanismus umgehen).   
   
Hinweis: Echten Anwendungen einen sicheren Mechanismus zum Speichern von Inhalt, der kein Benutzer leichter manipuliert ist erworben übernehmen soll. Dies kann die Verschlüsselung und/oder anderer Techniken, einschließlich Remote-Server-Authentifizierung umfassen.   
   
 Der Mechanismus sollten auch die integrierten Funktionen für Sicherung und Wiederherstellung von iOS, iTunes- und iCloud nutzen entworfen werden. Dadurch wird sichergestellt, dass nachdem ein Benutzer eine Sicherung wiederhergestellt wird ihre vorherigen Einkäufe sofort zur Verfügung stehen.   
   
   
 Finden von Apple Secure Codierung Handbuch Weitere iOS-spezifische Richtlinien.

## <a name="receipt-verification-and-server-delivered-products"></a>Überprüfung der Eingang und Produkte Server übermittelt

In den Beispielen in diesem Dokument bisher haben umfasste, ausschließlich der Anwendung kommunizieren direkt mit dem App Store-Servern zum Kauf Transaktionen durchzuführen, für die Features oder Funktionen, die bereits in der Anwendung hartcodiert entsperren.   
   
   
   
 Apple bietet für eine zusätzliche Sicherheitsstufe Kauf Kauf Empfangsbestätigungen unabhängig von einem anderen Server überprüft werden, kann eine Anforderung zu überprüfen, bevor die Bereitstellung digitale Inhalte als Teil einer Bestellung hilfreich sein können (z. B. ein Buch digitale oder Magazine).   
   
   
   
 **Integrierte Produkte** – wie in den Beispielen in diesem Dokument, das Produkt Funktionalität, die im Lieferumfang der Anwendung vorhanden ist. Ein in-app-Käufe ermöglicht dem Benutzer auf die Funktion zugreifen.
Produkt-IDs sind hartcodiert.   
   
 **Server bereitgestellte Produkte** – das Produkt besteht zum Herunterladen von Inhalt, die auf einem Remoteserver gespeichert ist, bis eine erfolgreiche Transaktion bewirkt, dass den Inhalt heruntergeladen werden.
Beispiele für möglicherweise Bücher oder magazine Probleme sind. Produkt-IDs werden in der Regel von einem externen Server definiert (wobei der Produktinhalt auch gehostet wird). Anwendungen müssen eine robuste Weise aufzeichnen nachdem eine Transaktion abgeschlossen wurde, sodass Wenn download fehlschlägt er erneut versucht werden kann, ohne den Benutzer verwirrend implementieren.

### <a name="server-delivered-products"></a>Server bereitgestellte Produkte

Einige Produkte den Inhalt, z. B. Bücher und Zeitschriften (oder sogar eine spielebene) von einem Remoteserver während des Kaufvorgangs heruntergeladen werden muss. Dies bedeutet, dass ein zusätzlicher Server erforderlich ist, speichern und die Produkt-Inhalte bereitstellen, nachdem es erworben wird.

#### <a name="getting-prices-for-server-delivered-products"></a>Preise abrufen für Server bereitgestellte Produkte

Da die Produkte Remote übermittelt werden, ist es auch möglich, weitere Produkte Zeit hinzufügen (ohne Aktualisierung der app-Code), z. B. das Hinzufügen weitere Bücher oder neue Probleme einem Magazin. Damit die Anwendung kann diese News-Produkte zu ermitteln und dem Benutzer werden können angezeigt, sollten die zusätzliche Server speichern und diese Informationen liefern.   
   
   
   
 [![](transactions-and-verification-images/image38.png "Preise abrufen für Server bereitgestellte Produkte")](transactions-and-verification-images/image38.png#lightbox)   
   
   
   
 1. Produktinformationen muss an mehreren Orten gespeichert werden: auf dem Server und in iTunes Connect. Darüber hinaus wird jedes Produkt Inhaltsdateien zugeordnet haben. Diese Dateien werden nach einem Kauf erfolgreich übermittelt.   
   
 2. Wenn der Benutzer ein Produkt kaufen möchte, muss die Anwendung bestimmen, welche Produkte verfügbar sind. Diese Informationen kann zwischengespeichert werden, sondern übermittelt werden soll, von einem Remoteserver, auf dem die Masterliste der Produkte gespeichert ist.   
   
 3. Der Server gibt eine Liste von Produkt-IDs für die Anwendung zu analysieren.   
   
 4. Klicken Sie dann die Anwendung bestimmt, welche diese Produkt-IDs StoreKit an, um Preise und Beschreibungen abzurufen.   
   
 5. StoreKit sendet die Liste der Produkt-IDs an Apple Server.   
   
 6. Die iTunes-Server antwortet mit gültigen Produktinformationen (Beschreibung und den aktuellen Preis).   
   
 7. Der Anwendungsverzeichnis `SKProductsRequestDelegate` die Produktinformationen für die Anzeige für dem Benutzer übergeben wird.

#### <a name="purchasing-server-delivered-products"></a>Server bereitgestellte Produkte kaufen

Da der Remoteserver Weise überprüfen erfordert, ob eine inhaltsanforderung gültig ist (ie. für bezahlt wurde), die Empfangsinformationen zum entlang für die Authentifizierung übergeben. Der Remoteserver leitet diese Daten in iTunes für die Überprüfung und bei erfolgreicher Ausführung enthält den Produktinhalt in der Antwort an die Anwendung.   
   
 [![](transactions-and-verification-images/image39.png "Server bereitgestellte Produkte kaufen")](transactions-and-verification-images/image39.png#lightbox)   
   
 1. Fügt die app ein `SKPayment` an die Warteschlange. Bei Bedarf der Benutzer wird aufgefordert, ihre Apple-ID und aufgefordert, die Zahlung zu bestätigen.   
   
 2. StoreKit sendet die Anforderung zur Verarbeitung an den Server.   
   
 3. Wenn die Transaktion abgeschlossen ist, antwortet der Server mit der eine Bestätigung der Transaktion.   
   
 4. Die `SKPaymentTransactionObserver` Unterklasse die Bestätigung empfängt und verarbeitet es. Da das Produkt von einem Server heruntergeladen werden muss, initiiert die Anwendung eine netzwerkanforderung an den Remoteserver.   
   
 5. Die downloadanforderung wird durch die Receipt-Daten ausgegeben, damit der Remoteserver überprüfen kann, dass er autorisiert ist, auf den Inhalt zugreifen. Die Anwendung Netzwerkclient wartet auf eine Antwort auf diese Anforderung.   
   
 6. Wenn der Server eine Anforderung für den Inhalt empfängt, analysiert der Receipt-Daten und sendet eine Anforderung direkt an die iTunes-Server, überprüfen Sie, ob die Bestätigung für eine Transaktion gültig ist. Der Server sollte Logik verwenden, um festzustellen, ob die Anforderung an die Produktion oder Sandkasten URL senden. Apple empfiehlt immer mithilfe der Produktions-URL und der Wechsel zur Sandkasten, wenn Status: 21007 (Sandkasten-Bestätigung gesendet, um den Produktionsserver) – finden Sie unter [technischen Hinweis 2259 FAQ Nr. 16](https://developer.apple.com/library/ios/#technotes/tn2259/_index.html).   
   
 7. iTunes überprüft den Empfang und Status von 0 (null) zurück, wenn es gültig ist.   
   
 8. Der Server wartet iTunes-Antwort ab. Wenn es sich um eine gültige Antwort empfängt, sollte der Code die zugehörigen Produkt Inhaltsdatei einschließt, die als Antwort auf die Anwendung suchen.   
   
 9. Die Anwendung empfängt und analysiert die Antwort, die den Produktinhalt an das Gerät Dateisystem speichern.   
   
 10. Die Anwendung ermöglicht das Produkt und ruft dann die StoreKit `FinishTransaction`. Die Anwendung kann dann optional den erworbenen Inhalt (z. B. Anzeigen der ersten Seite eines erworbenen Buch oder Magazin Problem) anzeigen.

Eine alternative Implementierung für sehr große Produkt Inhaltsdateien kann vorsehen, dass die Bestätigung der Transaktion einfach in Schritt &#9; zu speichern, damit, dass die Transaktion schnell abgeschlossen werden kann und Bereitstellen einer Benutzeroberfläche für den Benutzer zum Herunterladen des Inhalts der tatsächlichen product zu einem späteren Zeitpunkt. Die nachfolgenden downloadanforderung kann die Inhaltsdatei erforderlichen Zugriff auf den gespeicherten Empfang erneut senden.

### <a name="writing-server-side-receipt-verification-code"></a>Schreiben von Code für serverseitige Receipt-Überprüfung

Überprüfen eine Bestätigung in serverseitigen Code kann mit einer einfachen HTTP-POST-Anforderung/Antwort erfolgen, die Schritte #5 bis #8 in das Workflowdiagramm umfasst.   
   
   
   
 Extrahieren Sie die `SKPaymentTansaction.TransactionReceipt` Eigenschaft in der app. Dies sind die Daten, die an iTunes für die Überprüfung (Schritt 5 von #) gesendet werden müssen.

Base64-codiert den Receipt-Transaktionsdaten (entweder in Schritt &#5; "oder" #6).

Erstellen Sie eine einfache JSON-Nutzlast wie folgt:

```csharp
{
   "receipt-data" : "(base-64 encoded receipt here)"
}
```

HTTP POST JSON in [ https://buy.itunes.apple.com/verifyReceipt ](https://buy.itunes.apple.com/verifyReceipt) für die Produktion oder [ https://sandbox.itunes.apple.com/verifyReceipt ](https://sandbox.itunes.apple.com/verifyReceipt) zu Testzwecken.   
   
 Die JSON-Antwort enthält die folgenden Schlüssel:

```csharp
{
   "status" : 0,
   "receipt" : { (receipt repeated here) }
}
```

Status von 0 (null) gibt eine gültige Bestätigung an. Inhalt des erworbenen Produkts erfüllen kann Ihren Server beginnen. Der Receipt-Schlüssel enthält ein JSON-Wörterbuch mit den gleichen Eigenschaften wie die `SKPaymentTransaction` -Objekt, das von der Anwendung empfangen wurde, damit dieses Wörterbuch aufweist und zum Abrufen von Informationen, z. B. die Product_id und die Menge der Bestellung von der Servercode abgefragt werden kann.

Finden Sie in der Apple- [überprüfen Store Empfangsbestätigungen](https://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/StoreKitGuide/VerifyingStoreReceipts/VerifyingStoreReceipts.html#//apple_ref/doc/uid/TP40008267-CH104-SW1) Dokumentation weitere Informationen.

#### <a name="using-aspnet"></a>Mithilfe von ASP.NET

Für C#-Entwickler besteht eine nützliche Open Source-Projekt mit der Bezeichnung [APNS-Sharp-](https://github.com/Redth/APNS-Sharp) inklusive der Receipt-Überprüfungscode, der funktioniert in ASP.NET. Insbesondere die `Receipt.cs` und `ReceiptVerification.cs` Dateien in der [ `Jdsoft.Apple.AppStore` ](https://github.com/Redth/APNS-Sharp/tree/master/JdSoft.Apple.AppStore) Verzeichnis kann an eine Website .NET Schritte #6 bis #8 aus dem Workflow-Diagramm Server-Delivered Produkte einfach implementieren hinzugefügt werden.   
   
   
   
 Kommunikation mit den iTunes-Servern verwendet, JSON, einfach in c# zu verarbeiten.   
   
   
   
 Finden Sie in der Apple-In-App-Käufe [Receipt Überprüfung](https://developer.apple.com/library/ios/#releasenotes/StoreKit/IAP_ReceiptValidation/_index.html) Dokumentation für ausführliche Informationen zum Überprüfen, dass eine Bestätigung gültig ist.

### <a name="3rd-party-receipt-verification"></a>3rd Party Receipt-Überprüfung

Es gibt Unternehmen, die Plattformen für Receipt-Überprüfung (und anderem), die Sie verwenden können bieten, anstatt einen eigenen Server zu erstellen. Xamarin ist nicht dieser Dienste bewerben; Sie sind einfach zu Referenzzwecken hier erwähnt.

#### <a name="urban-airship"></a>Besiedelter Airship

Besiedelter Airship bietet eine Reihe von unterschiedlichen Back-End-Diensten für iOS-apps, einschließlich Receipt-Überprüfung und Push-Benachrichtigungen.   
   
 [http://urbanairship.com/products/in-app-purchase/](http://urbanairship.com/products/in-app-purchase/)



