---
title: Übersicht über die StoreKit und Abrufen von Produktinformationen in Xamarin.iOS
description: Dieses Dokument enthält eine Übersicht über StoreKit. Es beschreibt die Klassen mit StoreKit, StoreKit Interaktionen zu testen, Anzeigen von Produkten zum Verkauf, Behandeln von ungültigen Produkte und lokalisierte Preise anzeigen.
ms.prod: xamarin
ms.assetid: FC21192E-6325-4389-C060-E92DBB5EBD87
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 0dcda2e4fd1ca7773668a0a6fdf46e01f2f0841d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61366967"
---
# <a name="storekit-overview-and-retrieving-product-info-in-xamarinios"></a>Übersicht über die StoreKit und Abrufen von Produktinformationen in Xamarin.iOS

Die Benutzeroberfläche für eine in-app-Käufe wird in den folgenden Screenshots dargestellt.
Bevor eine Transaktion durchgeführt wird, muss die Anwendung, Preis und eine Beschreibung für die Anzeige des Produkts abrufen. Klicken Sie dann, wenn der Benutzer drückt **kaufen**, die Anwendung sendet eine Anforderung an StoreKit, der die Bestätigungsdialogfeld und Apple-ID-Anmeldung verwaltet. Vorausgesetzt, dass die Transaktion dann erfolgreich ist, StoreKit benachrichtigt den Anwendungscode, muss die speichern Sie das Transaktionsergebnis und bietet dem Benutzer mit Zugriff auf den Kauf.   

   
 [![](store-kit-overview-and-retreiving-product-information-images/image14.png "StoreKit benachrichtigt den Anwendungscode, die das Transaktionsergebnis zu speichern und bietet dem Benutzer mit Zugriff auf den Kauf müssen")](store-kit-overview-and-retreiving-product-information-images/image14.png#lightbox)

## <a name="classes"></a>Klassen

Implementieren von in-app-Einkäufe sind die folgenden Klassen aus dem Framework StoreKit erforderlich:   
   
 **SKProductsRequest** – eine Anforderung an StoreKit genehmigten Produkte verkaufen (App Store). Kann mit einer Anzahl von der Produkt-Ids konfiguriert werden.

-   **SKProductsRequestDelegate** : deklariert die Methoden, um die Product-Anforderungen und Antworten zu verarbeiten. 
-   **SKProductsResponse** – zurück an den Delegaten von StoreKit (App Store) gesendet. Enthält die SKProducts, die das Produkt entsprechen, die Ids mit der Anforderung gesendet. 
-   **SKProduct** – ein Produkt aus StoreKit (die Sie in iTunes Connect konfiguriert haben) abgerufen. Enthält Informationen über das Produkt wie Produkt-ID, Titel, Beschreibung und Preis. 
-   **SKPayment** – mit der einer Produkt-ID erstellt und an die Warteschlange Zahlung, führen Sie einen Kauf hinzugefügt. 
-   **SKPaymentQueue** – Payment-Anforderungen in der Warteschlange an Apple gesendet werden. Als Ergebnis der einzelnen Zahlung verarbeitet werden Benachrichtigungen ausgelöst werden. 
-   **SKPaymentTransaction** – stellt eine abgeschlossene Transaktion (eine Bestellanforderung, die von der App-Store verarbeitet und an die Anwendung über StoreKit gesendet wurde). Die Transaktion möglicherweise erworbenen, wiederhergestellt oder Fehler. 
-   **SKPaymentTransactionObserver** – benutzerdefinierte Unterklasse, die auf Ereignisse, die von der Warteschlange des StoreKit Zahlung generiert reagiert. 
-   **StoreKit Vorgänge sind asynchron** : Nachdem ein SKProductRequest gestartet wird oder ein SKPayment wird die Steuerung wieder an Ihren Code in der Warteschlange hinzugefügt. StoreKit rufen die Methoden für die Unterklasse SKProductsRequestDelegate oder SKPaymentTransactionObserver, beim Empfang von Daten aus dem Apple Server. 


Das folgende Diagramm zeigt die Beziehungen zwischen den verschiedenen StoreKit-Klassen (abstrakte Klassen müssen in Ihrer Anwendung implementiert werden):   
   
   
   
 [![](store-kit-overview-and-retreiving-product-information-images/image15.png "Die Beziehungen zwischen den verschiedenen StoreKit Klassen abstrakte Klassen müssen implementiert werden, in der app")](store-kit-overview-and-retreiving-product-information-images/image15.png#lightbox)   
   
   
   
 Diese Klassen werden weiter unten in diesem Dokument ausführlicher erläutert.

## <a name="testing"></a>Test

Die meisten StoreKit-Vorgänge erfordern ein echtes Gerät für Tests. Abrufen von Produktinformationen (d. h. Preis &amp; Beschreibung) funktioniert im Simulator, aber erwerben und Wiederherstellungsvorgänge werden ein Fehler zurückgegeben (z. B. FailedTransaction Code 5002 = ein Unbekannter Fehler).

Hinweis: StoreKit kann nicht in der iOS-Simulator ausgeführt werden. Wenn Ihre Anwendung im iOS-Simulator ausgeführt wird, protokolliert StoreKit eine Warnung aus, wenn die Anwendung versucht wird, um die Zahlung Warteschlange abzurufen. Testen den Store muss auf Ihren Geräten ausgeführt werden.   
   
   
   
 Wichtig: Melden Sie sich nicht Ihrem Testkonto an, in der Anwendung von Einstellungen. Sie können die Anwendung Einstellungen Anmelden aus allen vorhandenen Apple-ID-Konten verwenden, und Sie müssen warten, damit Sie aufgefordert werden *innerhalb Ihrer In App-Käufe Sequenz* Anmeldung mit einem Test-Apple-ID   
   
   
   

Wenn Sie versuchen, den tatsächlichen Speicher mit einem Testkonto an anmelden, wird sie automatisch in eine echte Apple-ID konvertiert werden Dieses Konto wird nicht mehr für Tests verwendet werden.

Um StoreKit Code zu testen müssen Sie die Abmeldung von Ihren regulären iTunes-Testkonto und Ihre Anmeldeinformationen mit einem speziellen Testkonto an (in iTunes Connect erstellt), die mit dem Testen von Store verknüpft ist. Abmelden der aktuellen Konto Besuch **Einstellungen > iTunes und App-Store** wie hier gezeigt:

 [![](store-kit-overview-and-retreiving-product-information-images/image16.png "Melden Sie sich das aktuelle Konto finden Sie unter Einstellungen iTunes und App Store")](store-kit-overview-and-retreiving-product-information-images/image16.png#lightbox)
 
Melden Sie sich mit einem Testkonto an *Wenn StoreKit in Ihrer app anfordert*:



Zum Erstellen von Testbenutzern in iTunes Connect, klicken Sie auf **Benutzer und Rollen** auf der Hauptseite.

 [![](store-kit-overview-and-retreiving-product-information-images/image17.png "Zum Erstellen von Testbenutzern in iTunes Connect klicken Sie auf Benutzer und Rollen auf der Hauptseite")](store-kit-overview-and-retreiving-product-information-images/image17.png#lightbox)

Wählen Sie **Sandbox-Tester**

 [![](store-kit-overview-and-retreiving-product-information-images/image18.png "Auswählen von Sandbox-Tester")](store-kit-overview-and-retreiving-product-information-images/image18.png#lightbox)

Die Liste mit den vorhandenen Benutzern wird angezeigt. Sie können einen neuen Benutzer hinzufügen oder löschen ein vorhandenes Datensatzes. Das Portal nicht (derzeit) können Sie anzeigen oder Bearbeiten vorhandener Benutzer zu testen, daher wird empfohlen, dass Sie gut jedes Benutzers Test aufzeichnen, die (vor allem das Kennwort, Sie weisen) erstellt wird. Wenn Sie einen Testbenutzer löschen darf nicht die e-Mail-Adresse für ein anderes Konto für Tests erneut verwendet sein.  
   
 [![](store-kit-overview-and-retreiving-product-information-images/image19.png "Die Liste mit den vorhandenen Benutzern wird angezeigt.")](store-kit-overview-and-retreiving-product-information-images/image19.png#lightbox)   
   
 Neuen Testbenutzer haben ähnliche Attribute auf eine echte Apple-ID (z. B. Name, Kennwort, geheime Frage und Antwort). Dokumentieren Sie alle Details, die hier eingegebenen. Die **wählen iTunes Store** Feld bestimmen die Währung und Sprache, die in-app-Käufe, werden verwenden, wenn als dieser Benutzer angemeldet.

 [![](store-kit-overview-and-retreiving-product-information-images/image20.png "Das Feld Wählen iTunes Store bestimmen Währung und Sprache für ihre Einkäufe in der app des Benutzers")](store-kit-overview-and-retreiving-product-information-images/image20.png#lightbox)

## <a name="retrieving-product-information"></a>Abrufen von Produktinformationen

Der erste Schritt beim Verkauf ein Produkt in app-Käufe es angezeigt wird: Abrufen von den aktuellen Preis und die Beschreibung aus dem App Store für die Anzeige.   
   
   
   
 Unabhängig davon, welche Art von Produkten (verwendet werden können, nicht verwendbar oder einen Typ des Abonnements) eine app verkauft ist der Prozess zum Abrufen von Produktinformationen für die Anzeige identisch. Der InAppPurchaseSample-Code, der diesen Artikel begleitet, enthält ein Projekt namens *Verbrauchsmaterialien* die veranschaulicht, wie zum Abrufen von Produktionsinformationen für die Anzeige. Es zeigt, wie Sie:

-  Erstellen Sie eine Implementierung von `SKProductsRequestDelegate` und Implementieren der `ReceivedResponse` abstrakte Methode. Der Beispielcode ruft dies den `InAppPurchaseManager` Klasse. 
-  Wenden Sie sich an StoreKit, um festzustellen, ob Zahlungen zulässig sind (mit `SKPaymentQueue.CanMakePayments` ). 
-  Instanziieren Sie ein `SKProductsRequest` mit Produkt-Ids, die in iTunes Connect definiert wurden. Dies erfolgt in des Beispiels `InAppPurchaseManager.RequestProductData` Methode. 
-  Rufen Sie die Start-Methode für die `SKProductsRequest` . Dies löst einen asynchronen Aufruf an die Server App-Store. Der Delegat ( `InAppPurchaseManager` ) werden mit den Ergebnissen wird aufgerufen, zurück. 
-  Der Delegat, der ( `InAppPurchaseManager` ) `ReceivedResponse` Methode aktualisiert die Benutzeroberfläche mit den Daten aus dem App Store (Produktpreisen und Beschreibungen oder Meldungen über ungültige Produkte) zurückgegeben. 

Die gesamte Interaktion sieht wie folgt aus ( **StoreKit** ist eine integrierte Funktion für iOS, und die **App Store** Apple Server darstellt):

 [![](store-kit-overview-and-retreiving-product-information-images/image21.png "Abrufen von Produktinformationen-Diagramm")](store-kit-overview-and-retreiving-product-information-images/image21.png#lightbox)

### <a name="displaying-product-information-example"></a>Anzeigen von Informationen, Beispiel Produkt

Die [InAppPurchaseSample](https://developer.xamarin.com/samples/monotouch/StoreKit/) *Verbrauchsmaterialien* -Beispielcode wird veranschaulicht, wie die Produktinformationen abgerufen werden kann. Der Hauptbildschirm des Beispiels zeigt Informationen für zwei Produkte, die aus dem App Store abgerufen:   
   
   
   
 [![](store-kit-overview-and-retreiving-product-information-images/image23.png "Zeigt der Hauptbildschirm Informationen Produkte abgerufen, die aus dem App Store")](store-kit-overview-and-retreiving-product-information-images/image23.png#lightbox)   
   
   
   
 Der Beispielcode zum Abrufen und Anzeigen von Produktinformationen wird im folgenden ausführlicher erläutert.


#### <a name="viewcontroller-methods"></a>ViewController-Methoden

Die `ConsumableViewController` Klasse verwaltet die Anzeige der Preise für zwei Produkte, deren Produkt-IDs in der Klasse hartcodiert werden.

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

Auf die Klasse außerdem sollte ein NSObject deklariert wird, wird verwendet, mit der Einrichtung einer `NSNotificationCenter` Observer:

```csharp
NSObject priceObserver;
```

In der ViewWillAppear-Methode der Beobachter erstellt und mithilfe der standardmäßigen mitteilungszentrale zugewiesen:

```csharp
priceObserver = NSNotificationCenter.DefaultCenter.AddObserver (
  InAppPurchaseManager.InAppPurchaseManagerProductsFetchedNotification,
(notification) => {
   // display code goes here, to handle the response from the App Store
}
```

   
   
 Am Ende der `ViewWillAppear` -Methode, rufen die `RequestProductData` Methode, um die Anforderung StoreKit zu initiieren. Nachdem diese Anforderung vorgenommen wurde, kontaktiert StoreKit asynchron Apple Server zum Abrufen von Informationen, und fügen Sie sie wieder zu Ihrer app. Dies erfolgt durch die `SKProductsRequestDelegate` Unterklasse ( `InAppPurchaseManager`), wird im nächsten Abschnitt erläutert.

```csharp
iap.RequestProductData(products);
```

Der Code zum Anzeigen der Preis und eine Beschreibung einfach Ruft die Informationen aus der SKProduct ab und weist diese UIKit-Steuerelementen (Beachten Sie, die wir Anzeigen der `LocalizedTitle` und `LocalizedDescription` – StoreKit automatisch aufgelöst wird, den richtigen Text und die Preise basierend auf die Einstellungen des Benutzers-Konto). Der folgende Code gehört in der Benachrichtigung, die, der wir zuvor erstellt haben:

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

Zum Schluss die `ViewWillDisappear` Methode soll sicherstellen der Beobachter wird entfernt:

```csharp
NSNotificationCenter.DefaultCenter.RemoveObserver (priceObserver);
```

#### <a name="skproductrequestdelegate-inapppurchasemanager-methods"></a>SKProductRequestDelegate (InAppPurchaseManager)-Methoden

Die `RequestProductData` Methode wird aufgerufen, wenn die Anwendung wünscht, dass zum Abrufen von Produktpreisen und andere Informationen. Er erstellt dann die Auflistung der Produkt-Ids in den richtigen Datentyp analysiert eine `SKProductsRequest` mit diesen Informationen. Die Start-Methode aufrufen, führt dazu, dass eine netzwerkanforderung Apple Server vorgenommen werden. Die Anforderung asynchron ausgeführt wird, und rufen Sie die `ReceivedResponse` Methode des Delegaten, wenn er erfolgreich abgeschlossen wurde.

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

iOS wird die Anforderung automatisch an die "Sandbox" oder "produktionsbranch"-Version von der App-Store, je nachdem welche Bereitstellungsprofil erstellen, die die Anwendung ausgeführt wird, mit – weitergeleitet werden, also beim Entwickeln oder Testen der app die Anforderung den Zugriff auf alle Produkte in iTunes Connect (auch diejenigen, die noch nicht gesendet oder von Apple genehmigt wurden) konfiguriert sind. Wenn Ihre Anwendung in der Produktion ist, zurück StoreKit Anforderungen nur Informationen für **genehmigt** Produkte.   
   
   
   
 Die `ReceivedResponse` überschriebene Methode wird aufgerufen, nachdem Apple Server mit Daten reagiert haben. Da dies im Hintergrund aufgerufen wird, sollte der Code die gültige Daten zu analysieren und verwenden eine Benachrichtigung alle ViewControllers die Produktinformationen an, die "für diese Benachrichtigung überwachen". Der Code zum Erfassen von gültigen Produktinformationen und senden eine Benachrichtigung wird unten gezeigt:

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

Jedoch nicht in der Abbildung dargestellt die `RequestFailed` Methode sollte überschrieben werden, sodass Sie Feedback an dem Benutzer bereitstellen können, falls die App-Store-Server nicht erreichbar sind (oder ein anderer Fehler auftritt). Der Beispielcode ist lediglich in die Konsole geschrieben, aber eine echte Anwendung kann auch für Abfragen `error.Code` -Eigenschaft und implementiert ein benutzerdefiniertes Verhalten (z. B. eine Warnung für den Benutzer).

```csharp
public override void RequestFailed (SKRequest request, NSError error)
{
   Console.WriteLine (" ** InAppPurchaseManager RequestFailed() " + error.LocalizedDescription);
}
```

Dieser Screenshot zeigt die beispielanwendung direkt nach dem Laden (wenn keine Produktinformationen verfügbar sind):

 [![](store-kit-overview-and-retreiving-product-information-images/image24.png "Die Beispiel-app direkt nach dem Laden, wenn keine Produktinformationen verfügbar sind")](store-kit-overview-and-retreiving-product-information-images/image24.png#lightbox)

## <a name="invalid-products"></a>Ungültige Produkte

Ein `SKProductsRequest` möglicherweise auch eine Liste der ungültige Produkt-IDs zurück. Ungültige Produkte werden in der Regel aufgrund einer der folgenden zurückgegeben:   
   
   
   
 **Produkt-ID wurde falsch eingegeben wurde** – nur gültige Produkt-IDs sind zulässig.   
   
 **Produkt wurde nicht genehmigt** : beim Ausführen der Tests aller Produkte, die gelöscht werden, für den Verkauf zurückgegeben werden sollen, indem ein `SKProductsRequest`jedoch in der Produktion nur genehmigten Produkte zurückgegeben werden.   
   
 **App-ID ist keine explizite** – Wildcard App-IDs (durch ein Sternchen) dürfen keine in-app-Käufe.   
   
 **Falsche Bereitstellungsprofil** – Wenn Sie Änderungen an der Anwendungskonfiguration in dem bereitstellungsportal (z. B. das Aktivieren von in-app-Käufe), denken Sie daran, erneut zu generieren und verwenden das korrekte Bereitstellungsprofil aus, wenn es sich bei der Erstellung der app vornehmen.   
   
 **iOS-Anwendungen bezahlt-Vertrag ist nicht an Ort** – StoreKit Features funktionieren nicht alle, es sei denn, es ein gültiger Vertrag für Ihr Apple-Entwicklerkonto gibt.   
   
 **Die Binärdatei ist in den Status "abgelehnt"** – Wenn eine zuvor gesendete Binärdatei in den Status "abgelehnt" (entweder vom App Store-Team, oder vom Entwickler) vorhanden ist, dann StoreKit Funktionen nicht ausgeführt werden.

Die `ReceivedResponse` -Methode in der der Code gibt die ungültige Produkte in der Konsole:

```csharp
public override void ReceivedResponse (SKProductsRequest request, SKProductsResponse response)
{
   // code removed for clarity
   foreach (string invalidProductId in response.InvalidProducts) {
       Console.WriteLine("Invalid product id: " + invalidProductId );
   }
}
```

## <a name="displaying-localized-prices"></a>Lokalisierte Preise anzeigen

Preis Ebenen angeben, die einen bestimmten Preis für jedes Produkt für alle internationalen App Stores. Um sicherzustellen, dass die Preise für jede Währung richtig angezeigt werden, verwenden Sie die folgende Erweiterungsmethode (definiert `SKProductExtension.cs`) anstatt die Preis-Eigenschaft der einzelnen `SKProduct`:

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

Der Code, der den Titel der Schaltfläche legt verwendet die Erweiterungsmethode wie folgt:

```csharp
string Buy = "Buy {0}"; // or a localizable string
buy5Button.SetTitle(String.Format(Buy, product.LocalizedPrice()), UIControlState.Normal);
```

Verwenden von zwei verschiedenen iTunes-Testkonten, dass in den folgenden Screenshots (eine für den Speicher des American) und eine für das im japanischen Store führt:   
   
   
   
 [![](store-kit-overview-and-retreiving-product-information-images/image25.png "Zwei verschiedene iTunes-Testkonten Sprache bestimmte Ergebnisse angezeigt.")](store-kit-overview-and-retreiving-product-information-images/image25.png#lightbox)   
   
   
   
 Beachten Sie, dass der Speicher die Sprache wirkt sich auf, die für die Produkt-Informationen und den Preis Währung, verwendet wird, während des Geräts Spracheinstellung wirkt sich Bezeichnungen und andere lokalisierte Inhalte auf.   
   
   
   
 Denken Sie daran, um einen anderen Speicher verwenden das Konto müssen Sie testen **Abmelden** in die **Einstellungen > iTunes und App-Store** und erneuten Starten der Anwendung mit einem anderen Konto anmelden. So ändern Sie das Gerät die Sprache finden Sie unter **Einstellungen > Allgemein > International > Sprache**.

