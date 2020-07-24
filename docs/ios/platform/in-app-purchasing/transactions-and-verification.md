---
title: Transaktionen und Überprüfung in xamarin. IOS
description: In diesem Dokument wird beschrieben, wie Sie die Wiederherstellung früherer Käufe in einer xamarin. IOS-App zulassen. Außerdem werden Möglichkeiten zum Sichern von Käufen und vom Server gelieferten Produkten erläutert.
ms.prod: xamarin
ms.assetid: 84EDD2B9-3FAA-B3C7-F5E8-C1E5645B7C77
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 605f82c90f98bb4b50e5b630a53721d186ff35a1
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86935758"
---
# <a name="transactions-and-verification-in-xamarinios"></a>Transaktionen und Überprüfung in xamarin. IOS

## <a name="restoring-past-transactions"></a>Wiederherstellen früherer Transaktionen

Wenn Ihre Anwendung Produkttypen unterstützt, die wiederherstellbar sind, müssen Sie einige Benutzeroberflächen Elemente einschließen, damit Benutzer diese Käufe wiederherstellen können.
Diese Funktion ermöglicht es einem Kunden, das Produkt zusätzlichen Geräten hinzuzufügen oder das Produkt auf demselben Gerät wiederherzustellen, nachdem es bereinigt oder entfernt und neu installiert wurde. Die folgenden Produkttypen sind wiederherstellbar:

- Nicht nutzbare Produkte
- Automatisch-Abonnements
- Kostenlose Abonnements

Der Wiederherstellungs Vorgang sollte die Datensätze aktualisieren, die Sie auf dem Gerät aufbewahren, um Ihre Produkte zu erfüllen. Der Kunde kann jederzeit auf allen Geräten wiederherstellen. Beim Wiederherstellungs Vorgang werden alle vorherigen Transaktionen für diesen Benutzer erneut gesendet. der Anwendungscode muss dann bestimmen, welche Aktion mit diesen Informationen ausgeführt werden soll (z. b. überprüfen, ob bereits ein Datensatz für diesen Kauf auf dem Gerät vorhanden ist, und wenn dies nicht der Fall ist, einen Datensatz des Kaufs erstellen und das Produkt für den Benutzer aktivieren).

### <a name="implementing-restore"></a>Implementieren der Wiederherstellung

Mit der Schaltfläche **Wiederherstellen** der Benutzeroberfläche wird die folgende Methode aufgerufen, die restorecompletedtransactions im auslöst `SKPaymentQueue` .

```csharp
public void Restore()
{
   // theObserver will be notified of when the restored transactions start arriving <- AppStore
   SKPaymentQueue.DefaultQueue.RestoreCompletedTransactions();
}
```

Storekit sendet die Restore-Anforderung asynchron an die Server von Apple.   
   
Da der `CustomPaymentObserver` als Transaktions Beobachter registriert ist, empfängt er Nachrichten, wenn die Server von Apple Antworten. Die Antwort enthält alle Transaktionen, die dieser Benutzer jemals in dieser Anwendung ausgeführt hat (auf allen Geräten). Der Code durchläuft jede Transaktion, erkennt den wiederhergestellten Zustand und ruft die- `UpdatedTransactions` Methode auf, um Sie zu verarbeiten, wie unten dargestellt:

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

Wenn für den Benutzer keine wiederherstellbaren Produkte vorhanden sind, `UpdatedTransactions` wird nicht aufgerufen.   
   
Der einfachste mögliche Code für die Wiederherstellung einer bestimmten Transaktion im Beispiel führt dieselben Aktionen aus, wie bei einem Kauf, mit der Ausnahme, dass die- `OriginalTransaction` Eigenschaft für den Zugriff auf die Produkt-ID verwendet wird:

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

Eine anspruchsvollere Implementierung kann andere Eigenschaften überprüfen `transaction.OriginalTransaction` , z. b. das ursprüngliche Datum und die ursprüngliche empfangsnummer. Diese Informationen sind nützlich für einige Produkttypen (z. b. Abonnements).

#### <a name="restore-completion"></a>Wiederherstellungs Abschluss

`CustomPaymentObserver`Verfügt über zwei zusätzliche Methoden, die von storekit aufgerufen werden, wenn der Wiederherstellungsprozess abgeschlossen wurde (entweder erfolgreich oder mit einem Fehler), wie unten gezeigt:

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

In diesem Beispiel führen diese Methoden keine Aktionen aus. eine echte Anwendung kann jedoch eine Meldung für den Benutzer oder eine andere Funktionalität implementieren.

## <a name="securing-purchases"></a>Sichern von Käufen

In den beiden Beispielen in diesem Dokument wird `NSUserDefaults` zum Nachverfolgen von Käufen verwendet:   
   
 **Verbrauchsgüter** – die "Balance" von Guthaben Käufen ist ein einfacher `NSUserDefaults` ganzzahliger Wert, der bei jedem Einkauf inkrementiert wird.   
   
 **Nicht-Verbrauchs** Werte – jeder Fotofilter Einkauf wird als Schlüssel-Wert-Paar in gespeichert `NSUserDefaults` .

Durch `NSUserDefaults` die Verwendung von bleibt der Beispielcode einfach, aber es ist keine sehr sichere Lösung verfügbar, da es für technisch denkbare Benutzer möglicherweise möglich ist, die Einstellungen zu aktualisieren (Umgehung des Zahlungsmechanismus).   
   
Hinweis: reale Anwendungen sollten einen sicheren Mechanismus zum Speichern erworbener Inhalte übernehmen, die nicht von Benutzer Manipulationen abhängig sind. Dabei kann es sich um Verschlüsselung und/oder andere Verfahren handeln, einschließlich der Remote Server Authentifizierung.   
   
 Der Mechanismus sollte auch entwickelt werden, um die integrierten Sicherungs-und Wiederherstellungs Funktionen von IOS, iTunes und icloud nutzen zu können. Dadurch wird sichergestellt, dass die vorherigen Käufe sofort verfügbar sind, nachdem ein Benutzer eine Sicherung wieder hergestellt hat.   
   
Weitere IOS-spezifische Richtlinien finden Sie im sicheren Codierungs Handbuch von Apple.

## <a name="receipt-verification-and-server-delivered-products"></a>Empfangs Überprüfung und vom Server übermittelte Produkte

Die Beispiele in diesem Dokument haben bisher ausschließlich aus der Anwendung besteht, die direkt mit den App Store-Servern kommuniziert, um Kauf Transaktionen durchzuführen, die Features oder Funktionen entsperren, die bereits in der App codiert sind.   
   
Apple bietet ein zusätzliches Maß an kaufsicherheit, da Kauf Bestätigungen von einem anderen Server unabhängig überprüft werden können. Dies kann nützlich sein, um eine Anforderung zu überprüfen, bevor digitale Inhalte als Teil eines Kaufs (z. b. ein digitales Buch oder Magazin) bereitgestellt werden.   
   
 **Integrierte Produkte** – wie die Beispiele in diesem Dokument, ist das erworbene Produkt als in der Anwendung enthaltene Funktionalität vorhanden. Ein in-App-Einkauf ermöglicht dem Benutzer den Zugriff auf die Funktionalität.
Produkt-IDs sind hart codiert.   
   
 Von einem **Server übermittelte Produkte** – das Produkt besteht aus herunter ladbarem Inhalt, der auf einem Remote Server gespeichert wird, bis eine erfolgreiche Transaktion den Download des Inhalts bewirkt.
Beispiele können Bücher oder Magazine-Probleme enthalten. Produkt-IDs stammen in der Regel von einem externen Server (auf dem der Produktinhalt ebenfalls gehostet wird). Anwendungen müssen eine robuste Aufzeichnungsmethode implementieren, wenn eine Transaktion abgeschlossen ist, sodass beim Herunterladen des Inhalts ein erneuter Versuch unternommen werden kann, ohne den Benutzer zu verwirren.

### <a name="server-delivered-products"></a>Vom Server übermittelte Produkte

Der Inhalt einiger Produkte, z. b. Bücher und Magazine (oder sogar eine Spielebene), muss während des Kaufvorgangs von einem Remote Server heruntergeladen werden. Dies bedeutet, dass ein zusätzlicher Server erforderlich ist, um den Produktinhalt nach dem Erwerb zu speichern und zu übermitteln.

#### <a name="getting-prices-for-server-delivered-products"></a>Preise für vom Server gelieferte Produkte erhalten

Da die Produkte Remote geliefert werden, ist es auch möglich, mehr Produkte im Zeitverlauf hinzuzufügen (ohne den App-Code zu aktualisieren), wie z. b. das Hinzufügen von weiteren Büchern oder neuen Problemen eines Magazins. Damit die Anwendung diese Nachrichten Produkte ermitteln und für den Benutzer anzeigen kann, sollte der zusätzliche Server diese Informationen speichern und bereitstellen.   
   
[![Preise für vom Server gelieferte Produkte erhalten](transactions-and-verification-images/image38.png)](transactions-and-verification-images/image38.png#lightbox)   
   
1. Produktinformationen müssen an mehreren Stellen gespeichert werden: auf dem Server und in iTunes Connect. Außerdem werden jedem Produkt Inhalts Dateien zugeordnet. Diese Dateien werden nach einem erfolgreichen Kauf zugestellt.   
   
2. Wenn der Benutzer ein Produkt erwerben möchte, muss die Anwendung bestimmen, welche Produkte verfügbar sind. Diese Informationen können zwischengespeichert werden, sollten aber von einem Remote Server übermittelt werden, auf dem die Masterliste der Produkte gespeichert ist.   
   
3. Der Server gibt eine Liste mit Produkt-IDs zurück, die die Anwendung analysieren soll.   
   
4. Die Anwendung bestimmt dann, welche dieser Produkt-IDs an storekit gesendet werden, um Preise und Beschreibungen abzurufen.   
   
5. Storekit sendet die Liste der Produkt-IDs an die Apple-Server.   
   
6. Die iTunes-Server Antworten mit gültigen Produktinformationen (Beschreibung und aktueller Preis).   
   
7. Der Anwendung werden `SKProductsRequestDelegate` die Produktinformationen für die Anzeige an den Benutzer übermittelt.

#### <a name="purchasing-server-delivered-products"></a>Erwerb von vom Server gelieferten Produkten

Da der Remote Server eine Möglichkeit zum Überprüfen erfordert, dass eine Inhalts Anforderung gültig ist (d. h. wurde bezahlt), werden die Empfangs Informationen zur Authentifizierung weitergeleitet. Der Remote Server leitet diese Daten zur Überprüfung an iTunes weiter und enthält, wenn erfolgreich, den Produktinhalt in der Antwort an die Anwendung.   
   
 [![Erwerb von vom Server gelieferten Produkten](transactions-and-verification-images/image39.png)](transactions-and-verification-images/image39.png#lightbox)   
   
1. Die APP fügt `SKPayment` der Warteschlange ein hinzu. Falls erforderlich, wird der Benutzer zur Eingabe der Apple-ID aufgefordert und aufgefordert, die Zahlung zu bestätigen.   
   
2. Storekit sendet die Anforderung zur Verarbeitung an den Server.   
   
3. Wenn die Transaktion abgeschlossen ist, antwortet der Server mit einer Transaktionsbestätigung.   
   
4. Die `SKPaymentTransactionObserver` Unterklasse empfängt die Bestätigung und verarbeitet sie. Da das Produkt von einem Server heruntergeladen werden muss, initiiert die Anwendung eine Netzwerk Anforderung an den Remote Server.   
   
5. Die Download Anforderung wird von den Bestätigungsdaten begleitet, damit der Remote Server überprüfen kann, ob er für den Zugriff auf den Inhalt autorisiert ist. Der Netzwerkclient der Anwendung wartet auf eine Antwort auf diese Anforderung.   
   
6. Wenn der Server eine Inhalts Anforderung empfängt, analysiert er die Empfangsdaten und sendet eine Anforderung direkt an die iTunes-Server, um zu überprüfen, ob die Bestätigung für eine gültige Transaktion gilt. Der Server sollte eine Logik verwenden, um zu bestimmen, ob die Anforderung an die Produktions-oder Sandbox-URL gesendet werden soll. Apple schlägt vor, die Produktions-URL immer zu verwenden und zum Sandkasten zu wechseln, wenn Ihr Empfangsstatus 21007 (Sandkasten Bestätigung an Produktionsserver gesendet). Weitere Informationen finden Sie im [Programmier Handbuch zur Empfangs Validierung](https://developer.apple.com/library/archive/releasenotes/General/ValidateAppStoreReceipt/Chapters/ValidateRemotely.html) von Apple.
   
7. iTunes überprüft die Bestätigung und gibt den Status 0 (null) zurück, wenn Sie gültig ist.   
   
8. Der Server wartet auf iTunes ' Response. Wenn eine gültige Antwort empfangen wird, sollte der Code die zugehörige Produkt Inhalts Datei suchen, die in die Antwort an die Anwendung aufgenommen werden soll.   
  
9. Die Anwendung empfängt und analysiert die Antwort und speichert den Produktinhalt im Dateisystem des Geräts.   
   
10. Die Anwendung aktiviert das Produkt und ruft dann storekit auf `FinishTransaction` . Die Anwendung kann dann optional den erworbenen Inhalt anzeigen (z. b. die erste Seite eines erworbenen Buchs oder Magazine-Problems anzeigen).

Eine alternative Implementierung für sehr große Produkt Inhalts Dateien könnte das einfache Speichern der Transaktionsbestätigung in Schritt #9 beinhalten, sodass die Transaktion schnell abgeschlossen werden kann, und das Bereitstellen einer Benutzeroberfläche für den Benutzer, um den eigentlichen Produktinhalt zu einem späteren Zeitpunkt herunterzuladen. Die nachfolgende Download Anforderung kann die gespeicherte Bestätigung erneut senden, um auf die erforderliche Produkt Inhalts Datei zuzugreifen.

### <a name="writing-server-side-receipt-verification-code"></a>Der Server seitige Empfangs Überprüfungs Code wird geschrieben.

Das Überprüfen einer Bestätigung in Server seitigem Code kann mit einer einfachen HTTP POST-Anforderung/-Antwort erfolgen, die Schritte umfasst, die durch #8 im Workflow Diagramm #5 werden.   
   
Extrahieren Sie die- `SKPaymentTansaction.TransactionReceipt` Eigenschaft in der app. Dies sind die Daten, die zur Überprüfung an iTunes gesendet werden müssen (Schritt #5).

Base64-Codieren der Transaktions Beleg Daten (entweder in Schritt #5 oder #6).

Erstellen Sie eine einfache JSON-Nutzlast wie folgt:

```csharp
{
   "receipt-data" : "(base-64 encoded receipt here)"
}
```

HTTP Posten Sie den JSON-Code [https://buy.itunes.apple.com/verifyReceipt](https://buy.itunes.apple.com/verifyReceipt) für die Produktion oder [https://sandbox.itunes.apple.com/verifyReceipt](https://sandbox.itunes.apple.com/verifyReceipt) für Tests.   
   
 Die JSON-Antwort enthält die folgenden Schlüssel:

```csharp
{
   "status" : 0,
   "receipt" : { (receipt repeated here) }
}
```

Der Status 0 (null) gibt eine gültige Bestätigung an. Der Server kann fortfahren, um den Inhalt des erworbenen Produkts zu erfüllen. Der Empfangs Schlüssel enthält ein JSON-Wörterbuch mit denselben Eigenschaften wie das `SKPaymentTransaction` Objekt, das von der APP empfangen wurde. Daher kann der Servercode dieses Wörterbuch Abfragen, um Informationen wie die product_id und die Menge des Kaufs abzurufen.

Weitere Informationen finden Sie in der Dokumentation zur [Receipt Validation-Programmier](https://developer.apple.com/library/archive/releasenotes/General/ValidateAppStoreReceipt/Introduction.html) Dokumentation von Apple.
