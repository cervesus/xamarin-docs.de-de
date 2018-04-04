---
title: Speichern Sie, Abrufen von Informationen und Verwaltungskit (Übersicht)
ms.prod: xamarin
ms.assetid: FC21192E-6325-4389-C060-E92DBB5EBD87
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: f4ecd2942a99f80854fd340be454f9d8fefa5a36
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="store-kit-overview-and-retrieving-product-information"></a>Speichern Sie, Abrufen von Informationen und Verwaltungskit (Übersicht)

Die Benutzeroberfläche für eine in-app-Käufe wird in den nachstehenden Screenshots dargestellt.
Bevor jede Transaktion stattfindet, muss die Anwendung abrufen, Preis und eine Beschreibung für die Anzeige des Produkts. Klicken Sie dann, wenn der Benutzer drückt **kaufen**, die Anwendung sendet eine Anfrage an StoreKit, die das Bestätigungsdialogfeld und Apple-ID-Anmeldung verwaltet. Vorausgesetzt, dass die Transaktion dann erfolgreich ist, StoreKit benachrichtigt den Anwendungscode, muss die speichern Sie das Transaktionsergebnis und geben Sie die Benutzer mit Zugriff auf ihren Kauf.   

   
 [![](store-kit-overview-and-retreiving-product-information-images/image14.png "StoreKit benachrichtigt den Anwendungscode, der speichern Sie das Transaktionsergebnis und geben Sie die Benutzer mit Zugriff auf ihren Kauf")](store-kit-overview-and-retreiving-product-information-images/image14.png#lightbox)

## <a name="classes"></a>Klassen

Implementierung von in-app-Einkäufe benötigt die folgenden Klassen aus dem Framework StoreKit:   
   
 **SKProductsRequest** – eine Anforderung zum StoreKit für genehmigte Produkte verkaufen (App-Speicher). Kann mit einer Anzahl von Produkt-Ids konfiguriert werden.

-   **SKProductsRequestDelegate** – deklariert Methoden, die Produkt-Anforderungen und Antworten zu behandeln. 
-   **SKProductsResponse** – zurück an den Delegaten von StoreKit (App-Store) gesendet. Enthält die SKProducts, die das Produkt entsprechen, die Ids für die Anforderung gesendet. 
-   **SKProduct** – ein Produkt aus StoreKit (die Sie in iTunes Connect konfiguriert haben) abgerufen. Enthält Informationen über das Produkt wie Produkt-ID, Titel, Beschreibung und Preis. 
-   **SKPayment** – mit einer Produkt-ID erstellt und an die Warteschlange "Payment" zum Ausführen einer Bestellung hinzugefügt. 
-   **SKPaymentQueue** – Payment-Anforderungen in der Warteschlange an Apple gesendet werden. Benachrichtigungen sind aufgrund der jeder verarbeiteten Zahlung ausgelöst. 
-   **SKPaymentTransaction** – stellt eine abgeschlossene Transaktion (eine Bestellanforderung, die durch den App Store verarbeitet und zurück an Ihre Anwendung über StoreKit gesendet wurden). Die Transaktion möglicherweise erworben, wiederhergestellt oder Fehler. 
-   **SKPaymentTransactionObserver** – benutzerdefinierte Unterklasse, die auf von der Warteschlange des StoreKit Zahlung generierte Ereignisse reagiert. 
-   **StoreKit Vorgänge sind asynchrone** – nachdem eine SKProductRequest gestartet wird oder eine SKPayment wird Code die Kontrolle zurück an die Warteschlange hinzugefügt. StoreKit rufen Methoden auf die Unterklasse SKProductsRequestDelegate oder SKPaymentTransactionObserver, beim Empfang von Daten aus dem Apple Server. 


Das folgende Diagramm zeigt die Beziehungen zwischen den verschiedenen StoreKit-Klassen (abstrakte Klassen müssen in der Anwendung implementiert werden):   
   
   
   
 [![](store-kit-overview-and-retreiving-product-information-images/image15.png "In der app müssen die Beziehungen zwischen den verschiedenen StoreKit Klassen abstrakte Klassen implementiert werden")](store-kit-overview-and-retreiving-product-information-images/image15.png#lightbox)   
   
   
   
 Diese Klassen werden weiter unten in diesem Dokument ausführlicher erläutert.

## <a name="testing"></a>Test

Die meisten StoreKit-Vorgänge erfordern ein echtes Gerät zu Testzwecken. Abrufen von Produktinformationen (d. h. Preis &amp; Beschreibung) funktioniert in der Simulator jedoch den Kauf und Wiederherstellungsvorgänge einen Fehler zurück (z. B. FailedTransaction Code = 5002 Unbekannter Fehler).

Hinweis: StoreKit kann nicht in der iOS-Simulator ausgeführt werden. Wenn Ihre Anwendung in der iOS-Simulator ausgeführt werden, protokolliert StoreKit eine Warnung aus, wenn Ihre Anwendung versucht, die Zahlung Warteschlange abrufen. Testen den Speicher muss auf die eigentlichen Geräte durchgeführt werden.   
   
   
   
 Wichtig: Melden Sie sich nicht mit Ihrem Test in der Anwendung-Einstellungen. Können Sie die Anwendung Einstellungen für die Registrierung aus jeder vorhandenen Apple-ID-Konto, und Sie müssen warten, zur Eingabe aufgefordert, *innerhalb der Sequenz In App-Käufe* bei der Anmeldung mit einem Test Apple-ID.   
   
   
   

Wenn Sie versuchen, den realen Speicher mit einem Testkonto anmelden, wird es automatisch in einer echten Apple-ID konvertiert werden Dieses Konto wird nicht mehr zum Testen verwendet werden.

Um StoreKit Code zu testen müssen Sie melden Sie sich von Ihren regulären iTunes-Testkonto und Ihre Anmeldeinformationen mit einem speziellen Testkonto (in iTunes Connect erstellt), die mit dem Test-Store verknüpft ist. Zum Abmelden des aktuellen Konto Besuchs **Einstellungen > iTunes App Store, und** wie hier gezeigt:

 [![](store-kit-overview-and-retreiving-product-information-images/image16.png "Zum Signieren von aus dem aktuellen Konto Besuch iTunes Einstellungen und App Store")](store-kit-overview-and-retreiving-product-information-images/image16.png#lightbox)
 
Melden Sie sich mit einem Testkonto *durch StoreKit innerhalb Ihrer app angefordert*:



Zum Erstellen von Testbenutzern in iTunes Connect, klicken Sie auf **Benutzer und Rollen** auf der Hauptseite.

 [![](store-kit-overview-and-retreiving-product-information-images/image17.png "Zum Erstellen von Testbenutzern in iTunes Connect klicken Sie auf Benutzer und Rollen auf der Hauptseite")](store-kit-overview-and-retreiving-product-information-images/image17.png#lightbox)

Wählen Sie **Sandkasten-Tester**

 [![](store-kit-overview-and-retreiving-product-information-images/image18.png "Auswählen der Sandbox-Tester")](store-kit-overview-and-retreiving-product-information-images/image18.png#lightbox)

Die Liste der vorhandenen Benutzer wird angezeigt. Sie können einen neuen Benutzer hinzufügen oder Löschen einen vorhandenen Datensatz. Das Portal nicht (derzeit) können Sie anzeigen oder Bearbeiten vorhandener Benutzer zu testen, daher wird empfohlen, dass Sie gut jedes Benutzers Test, die erstellt wird (insbesondere das Kennwort aufzeichnen, weisen Sie Sie). Nachdem Sie einen Testbenutzer löschen darf nicht die e-Mail-Adresse erneut verwendet, für einen anderen Testkonto sein.  
   
 [![](store-kit-overview-and-retreiving-product-information-images/image19.png "Die Liste der vorhandenen Benutzer wird angezeigt.")](store-kit-overview-and-retreiving-product-information-images/image19.png#lightbox)   
   
 Neue Testbenutzer haben ähnliche Attribute auf eine echte Apple-ID (z. B. Name, Kennwort, geheime Frage und Antwort). Notieren Sie sich die Details, die bei dem hier eingegebenen. Die **wählen iTunes Store** Feld bestimmt, welche Währung und Sprache, die die app-Käufe wird verwendet, wenn angemeldeten Namen dieses Benutzers.

 [![](store-kit-overview-and-retreiving-product-information-images/image20.png "Das Feld Wählen iTunes Store wird Währung und Sprache für ihre app-Einkäufe des Benutzers bestimmt.")](store-kit-overview-and-retreiving-product-information-images/image20.png#lightbox)

## <a name="retrieving-product-information"></a>Abrufen von Produktinformationen

Der erste Schritt beim Verkauf ein Produkt in app-Käufe angezeigt wird: das Abrufen der aktuellen Preis und eine Beschreibung aus dem App Store für die Anzeige.   
   
   
   
 Unabhängig davon, welche Art von Produkten eine app (umgewandelt, nicht verwendbar oder ein Typ des Abonnements) verkauft ist der Prozess zum Abrufen von Produktinformationen für die Anzeige identisch. Der InAppPurchaseSample-Code, die mit dieser Artikel enthält ein Projekt namens *Verbrauchsgüter* , die veranschaulicht, wie Produktionsinformationen für die Anzeige abzurufen. Es wird gezeigt, wie Sie:

-  Erstellen Sie eine Implementierung von `SKProductsRequestDelegate` und Implementieren der `ReceivedResponse` abstrakte Methode. Der Beispielcode ruft diese die `InAppPurchaseManager` Klasse. 
-  Erkundigen Sie sich StoreKit, um festzustellen, ob Zahlungen zulässig sind (mit `SKPaymentQueue.CanMakePayments` ). 
-  Instanziieren einer `SKProductsRequest` mit Produkt-Ids, die in iTunes Connect definiert wurden. Dies erfolgt in des Beispiels `InAppPurchaseManager.RequestProductData` Methode. 
-  Rufen Sie die Start-Methode für die `SKProductsRequest` . Dies löst einen asynchronen Aufruf an den App Store-Servern. Der Delegat ( `InAppPurchaseManager` ) werden mit den Ergebnissen Namens zurück. 
-  Der Delegat ( `InAppPurchaseManager` ) `ReceivedResponse` Methode aktualisiert die Benutzeroberfläche mit den Daten aus dem App Store (Produktpreise & Beschreibungen oder Meldungen über ungültige Produkte) zurückgegeben. 

Die gesamte Interaktion sieht wie folgt ( **StoreKit** ist eine integrierte Funktion für iOS, und die **App Store** Apple Server darstellt):

 [![](store-kit-overview-and-retreiving-product-information-images/image21.png "Abrufen von Produktinformationen-Diagramm")](store-kit-overview-and-retreiving-product-information-images/image21.png#lightbox)

### <a name="displaying-product-information-example"></a>Anzeigen von Produkt Informationen, Beispiel

Die [InAppPurchaseSample](https://developer.xamarin.com/samples/monotouch/StoreKit/) *Verbrauchsgüter* -Beispielcode wird veranschaulicht, wie die Produktinformationen abgerufen werden kann. Zeigt der Hauptbildschirm des Beispiels-Informationen für zwei Produkte, die aus dem App Store abgerufen werden:   
   
   
   
 [![](store-kit-overview-and-retreiving-product-information-images/image23.png "Zeigt der Hauptbildschirm Informationen Produkte abgerufen, die aus dem App Store")](store-kit-overview-and-retreiving-product-information-images/image23.png#lightbox)   
   
   
   
 Der Beispielcode zum Abrufen und Anzeigen von Produktinformationen wird im folgenden ausführlicher erläutert.


#### <a name="viewcontroller-methods"></a>ViewController-Methoden

Die `ConsumableViewController` Klasse verwaltet die Anzeige von Preisen für zwei Produkte, deren Produkt-IDs in der Klasse sind.

```csharp
public static string Buy5ProductId = "com.xamarin.storekit.testing.consume5credits",
   Buy10ProductId = "com.xamarin.storekit.testing.consume10credits";
List<string> products;
InAppPurchaseManager iap;
public ConsumableViewController () : base()
{
   // two products for sale on this page
   products = new List<string>() {Buy5ProductId, Buy10ProductId};
   iap = new InAppPurchaseManager();
}
```

Die Klasse es auch darf eine NSObject deklariert, die Ebene wird verwendet, um Setup eine `NSNotificationCenter` "Beobachter":

```csharp
NSObject priceObserver;
```

In der ViewWillAppear-Methode der "Beobachter" erstellt und mithilfe der Standard-mitteilungszentrale zugewiesen:

```csharp
priceObserver = NSNotificationCenter.DefaultCenter.AddObserver (
  InAppPurchaseManager.InAppPurchaseManagerProductsFetchedNotification,
(notification) => {
   // display code goes here, to handle the response from the App Store
}
```

   
   
 Am Ende der `ViewWillAppear` -Methode, rufen die `RequestProductData` Methode, um die StoreKit-Anforderung initiiert werden. Sobald diese Anforderung gestellt wurde, wird StoreKit asynchron wenden Sie sich an Apple Server zum Abrufen von Informationen und zum feed es wieder an die Anwendung. Dies erfolgt durch die `SKProductsRequestDelegate` Unterklasse ( `InAppPurchaseManager`), wird im nächsten Abschnitt erläutert.

```csharp
iap.RequestProductData(products);
```

Der Code zum Anzeigen der Preis und eine Beschreibung einfach die Informationen aus der SKProduct abgerufen und UIKit Steuerelemente zugewiesen (Beachten Sie, die wir Anzeigen der `LocalizedTitle` und `LocalizedDescription` – StoreKit automatisch aufgelöst, den richtigen Text und die Preise basierend auf der Benutzer kontoeinstellungen). Der folgende Code gehört in der Benachrichtigung, der wir zuvor erstellt haben:

```csharp
priceObserver = NSNotificationCenter.DefaultCenter.AddObserver (
  InAppPurchaseManager.InAppPurchaseManagerProductsFetchedNotification,
(notification) => {
   // display code goes here, to handle the response from the App Store
   var info = notification.UserInfo;
   if (info.ContainsKey(NSBuy5ProductId)) {
       var product = (SKProduct) info.ObjectForKey(NSBuy5ProductId);
       buy5Button.Enabled = true;
       buy5Title.Text = product.LocalizedTitle;
       buy5Description.Text = product.LocalizedDescription;
       buy5Button.SetTitle("Buy " + product.Price, UIControlState.Normal); // price display should be localized
   }
}
```

Schließlich die `ViewWillDisappear` Methode soll sicherstellen der Beobachter wird entfernt:

```csharp
NSNotificationCenter.DefaultCenter.RemoveObserver (priceObserver);
```

#### <a name="skproductrequestdelegate-inapppurchasemanager-methods"></a>SKProductRequestDelegate (InAppPurchaseManager)-Methoden

Die `RequestProductData` Methode wird aufgerufen, wenn die Anwendung Produktpreisen und andere Informationen abrufen möchte. Es wird die Auflistung der Produkt-Ids in den richtigen Datentyp analysiert dann erstellt eine `SKProductsRequest` mit diesen Informationen. Aufrufen der Start-Methode bewirkt, dass eine netzwerkanforderung an Apple Server vorgenommen werden müssen. Die Anforderung asynchron ausgeführt wird, und rufen Sie die `ReceivedResponse` -Methode des Delegaten, wenn er erfolgreich abgeschlossen wurde.

```csharp
public void RequestProductData (List<string> productIds)
{
   var array = new NSString[productIds.Count];
   for (var i = 0; i < productIds.Count; i++) {
       array[i] = new NSString(productIds[i]);
   }
   NSSet productIdentifiers = NSSet.MakeNSObjectSet<NSString>(array);
   productsRequest = new SKProductsRequest(productIdentifiers);
   productsRequest.Delegate = this; // for SKProductsRequestDelegate.ReceivedResponse
   productsRequest.Start();
}
```

iOS wird die Anforderung automatisch an die "Sandkasten" oder "Produktion"-Version des App-Stores, je nachdem welche Bereitstellungsprofil – die Anwendung ausgeführt wird weitergeleitet werden, sodass beim Entwickeln oder Testen Ihre app wird mit die Anforderung Zugriff auf jedes Produkt haben konfiguriert in iTunes Connect (auch diejenigen, die noch nicht gesendet oder von Apple genehmigt wurden). Wenn Ihre Anwendung in Produktion ist, StoreKit Anforderungen nur Informationen für zurückgeben **genehmigt** Produkte.   
   
   
   
 Die `ReceivedResponse` außer Kraft gesetzte Methode wird aufgerufen, nachdem der Apple-Servern mit Daten geantwortet haben. Da dies im Hintergrund aufgerufen wird, sollte der Code die gültige Daten analysieren und verwenden eine Benachrichtigung "für diese Benachrichtigung Zuhören" alle ViewControllers die Produktinformationen an. Der Code zum Erfassen von gültigen Produktinformationen und senden eine Benachrichtigung wird unten gezeigt:

```csharp
public override void ReceivedResponse (SKProductsRequest request, SKProductsResponse response)
{
   SKProduct[] products = response.Products;
   NSDictionary userInfo = null;
   if (products.Length > 0) {
       NSObject[] productIdsArray = new NSObject[response.Products.Length];
       NSObject[] productsArray = new NSObject[response.Products.Length];
       for (int i = 0; i < response.Products.Length; i++) {
           productIdsArray[i] = new NSString(response.Products[i].ProductIdentifier);
           productsArray[i] = response.Products[i];
       }
       userInfo = NSDictionary.FromObjectsAndKeys (productsArray, productIdsArray);
   }
   NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerProductsFetchedNotification, this, userInfo);
}
```

Obwohl nicht in der Abbildung gezeigt den `RequestFailed` Methode muss überschrieben werden, damit, dass Sie uns Ihr Feedback senden an dem Benutzer bereitstellen können, falls die App Store-Server nicht erreichbar sind (oder ein anderer Fehler auftritt). Der Beispielcode ist lediglich in die Konsole geschrieben, aber eine realen Anwendung kann sich entscheiden, die in Abfragen `error.Code` Eigenschaft und implementieren benutzerdefinierte Verhalten (z. B. eine Warnung für den Benutzer).

```csharp
public override void RequestFailed (SKRequest request, NSError error)
{
   Console.WriteLine (" ** InAppPurchaseManager RequestFailed() " + error.LocalizedDescription);
}
```

Diese bildschirmabbildung zeigt die beispielanwendung unmittelbar nach dem Laden (wenn keine Produktinformationen verfügbar ist):

 [![](store-kit-overview-and-retreiving-product-information-images/image24.png "Die Beispiel-app unmittelbar nach dem Laden, wenn keine Produktinformationen verfügbar ist.")](store-kit-overview-and-retreiving-product-information-images/image24.png#lightbox)

## <a name="invalid-products"></a>Ungültige Produkte

Ein `SKProductsRequest` möglicherweise auch eine Liste der ungültigen Produkt-IDs zurück. Ungültige Produkte werden in der Regel aufgrund einer der folgenden zurückgegeben:   
   
   
   
 **Produkt-ID wurde falsch geschrieben wurde** – nur gültige Produkt-IDs werden akzeptiert.   
   
 **Produkt wurde nicht genehmigt** – beim Ausführen der Tests alle Produkte, die für den Verkauf gelöscht werden zurückgegeben werden soll, indem ein `SKProductsRequest`; jedoch in der Produktion nur genehmigte Produkte zurückgegeben werden.   
   
 **App-ID ist keine explizite** – Wildcard App-IDs (mit einem Sternchen) lassen sich nicht darauf aus, in der app zu erwerben.   
   
 **Falsche Bereitstellungsprofil** – Wenn Sie Änderungen, um der Anwendungskonfiguration in bereitstellungsportal vornehmen (z. B. das Aktivieren von in-app-Einkäufe), denken Sie daran, erneut zu generieren und verwenden das richtige Bereitstellungsprofil beim Erstellen der app ein.   
   
 **iOS-Anwendungen bezahlt-Vertrag ist noch nicht gültig** – StoreKit Funktionen funktionieren nicht überhaupt, es sei denn, es ein gültiger Vertrag für Ihr Apple Developer-Konto gibt.   
   
 **Die Binärdatei wird im Status "abgelehnt"** – Wenn eine zuvor übermittelte Binärdatei im Status "abgelehnt" (entweder vom Team App Store oder vom Entwickler) vorhanden ist, StoreKit-Funktionen nicht verwendet werden.

Die `ReceivedResponse` Methode im Beispielcode gibt die ungültige Produkte in der Konsole:

```csharp
public override void ReceivedResponse (SKProductsRequest request, SKProductsResponse response)
{
   // code removed for clarity
   foreach (string invalidProductId in response.InvalidProducts) {
       Console.WriteLine("Invalid product id: " + invalidProductId );
   }
}
```

## <a name="displaying-localized-prices"></a>Anzeigen von lokalisierten Preise

Preis Ebenen angeben, die einen bestimmten Preis für jedes Produkt für alle internationale App-Stores. Um sicherzustellen, dass Preise für jede Währung ordnungsgemäß angezeigt werden, verwenden Sie die folgende Erweiterungsmethode (definiert `SKProductExtension.cs`) anstelle der Eigenschaft Preis jedes `SKProduct`:

```csharp
public static class SKProductExtension {
   public static string LocalizedPrice (this SKProduct product)
   {
       var formatter = new NSNumberFormatter ();
       formatter.FormatterBehavior = NSNumberFormatterBehavior.Version_10_4;  
       formatter.NumberStyle = NSNumberFormatterStyle.Currency;
       formatter.Locale = product.PriceLocale;
       var formattedString = formatter.StringFromNumber(product.Price);
       return formattedString;
   }
}
```

Der Code, der Schaltfläche Titel festlegt, verwendet die Erweiterungsmethode wie folgt:

```csharp
string Buy = "Buy {0}"; // or a localizable string
buy5Button.SetTitle(String.Format(Buy, product.LocalizedPrice()), UIControlState.Normal);
```

Verwenden von zwei verschiedenen iTunes Testkonten (eine für den American Speicher) und eine für die japanische Store in den folgenden Screenshots Ergebnis:   
   
   
   
 [![](store-kit-overview-and-retreiving-product-information-images/image25.png "Zwei verschiedene iTunes Testkonten Sprache bestimmte Ergebnisse angezeigt.")](store-kit-overview-and-retreiving-product-information-images/image25.png#lightbox)   
   
   
   
 Beachten Sie, dass der Speicher der Sprache wirkt sich auf, die für Produkt Informationen und Preis Währung, verwendet wird, während das Gerät Language-Einstellung wirkt sich Bezeichnungen und andere lokalisierte Inhalte auf.   
   
   
   
 Denken Sie daran, die einen anderen Speicher verwenden Konto müssen Sie testen **Abmelden** in der **Einstellungen > iTunes App Store, und** und erneutes Starten der Anwendung mit einem anderen Konto anmelden. So ändern Sie das Gerät Sprache wechseln Sie zu **Einstellungen > Allgemein > International > Sprache**.

