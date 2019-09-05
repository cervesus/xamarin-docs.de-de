---
title: Übersicht über storekit und Abrufen von Produktinformationen in xamarin. IOS
description: Dieses Dokument enthält eine Übersicht über storekit. Es beschreibt die mit storekit verwendeten Klassen, testet storekit-Interaktionen, zeigt Produkte für den Verkauf an, verarbeitet ungültige Produkte und zeigt lokalisierte Preise an.
ms.prod: xamarin
ms.assetid: FC21192E-6325-4389-C060-E92DBB5EBD87
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/18/2017
ms.openlocfilehash: 4a68526187271c00332548764850783531391c73
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292189"
---
# <a name="storekit-overview-and-retrieving-product-info-in-xamarinios"></a>Übersicht über storekit und Abrufen von Produktinformationen in xamarin. IOS

Die Benutzeroberfläche für einen in-App-Einkauf wird in den nachfolgenden Screenshots angezeigt.
Bevor eine Transaktion durchgeführt wird, muss die Anwendung den Preis und die Beschreibung des Produkts für die Anzeige abrufen. Wenn der Benutzer den **Kauf**drückt, sendet die Anwendung eine Anforderung an storekit, die das Bestätigungs Dialogfeld und die Apple-ID-Anmeldung verwaltet. Wenn die Transaktion dann erfolgreich ausgeführt wird, benachrichtigt storekit den Anwendungscode, der das Transaktions Ergebnis speichern muss und dem Benutzer Zugriff auf den Kauf bietet.   

   
 [![](store-kit-overview-and-retreiving-product-information-images/image14.png "Storekit benachrichtigt den Anwendungscode, der das Transaktions Ergebnis speichern muss und dem Benutzer Zugriff auf seinen Einkauf bietet.")](store-kit-overview-and-retreiving-product-information-images/image14.png#lightbox)

## <a name="classes"></a>Klassen

Die Implementierung von in-App-Käufen erfordert die folgenden Klassen aus dem storekit-Framework:   
   
 **Skproducungrequest** – eine Anforderung an storekit für genehmigte Produkte, die verkauft werden sollen (App Store). Kann mit einer Reihe von Produkt-IDs konfiguriert werden.

- **Skproductrequestdelegat** – deklariert Methoden, um Produktanforderungen und-Antworten zu verarbeiten. 
- **Skproductresponse** – wird zurück an den Delegaten von storekit (App Store) gesendet. Enthält die skproducts, die den mit der Anforderung gesendeten Produkt-IDs entsprechen. 
- **Skproduct** – ein Produkt, das von storekit abgerufen wurde (das Sie in iTunes Connect konfiguriert haben). Enthält Informationen über das Produkt, z. b. Produkt-ID, Titel, Beschreibung und Preis. 
- **Skpayment** – erstellt mit einer Produkt-ID und wird zur Zahlungs Warteschlange hinzugefügt, um einen Kauf auszuführen. 
- **Skpaymentqueue** – Zahlungsanforderungen in der Warteschlange, die an Apple gesendet werden sollen. Benachrichtigungen werden ausgelöst, wenn jede Zahlung verarbeitet wird. 
- **Skpaymenttransaction** – stellt eine abgeschlossene Transaktion dar (eine Bestellung, die vom App Store verarbeitet und über storekit an Ihre Anwendung zurückgesendet wurde). Die Transaktion konnte gekauft, wieder hergestellt oder nicht ausgeführt werden. 
- **Skpaymenttransaktionobserver** – benutzerdefinierte Unterklasse, die auf Ereignisse antwortet, die von der storekit-Zahlungs Warteschlange generiert werden. 
- **Storekit-Vorgänge sind asynchron** – nachdem ein skproductrequest gestartet oder eine skpayment der Warteschlange hinzugefügt wurde, wird die Steuerung an den Code zurückgegeben. Storekit ruft Methoden in der Unterklasse skproduczrequestdelegat oder skpaymenttransaktionobserver auf, wenn Daten von den Servern von Apple empfangen werden. 


Das folgende Diagramm zeigt die Beziehungen zwischen den verschiedenen storekit-Klassen (abstrakte Klassen müssen in der Anwendung implementiert werden):   
   
   
   
 [![](store-kit-overview-and-retreiving-product-information-images/image15.png "Die Beziehungen zwischen den verschiedenen Klassen der storekit-Klassen müssen in der APP implementiert werden.")](store-kit-overview-and-retreiving-product-information-images/image15.png#lightbox)   
   
   
   
 Diese Klassen werden im weiteren Verlauf dieses Dokuments ausführlicher erläutert.

## <a name="testing"></a>Test

Die meisten storekit-Vorgänge erfordern ein reales Gerät zum Testen. Abrufen von Produktinformationen (d.h. die &amp; Preis Beschreibung) funktioniert im Simulator, aber bei den Kauf-und Wiederherstellungs Vorgängen wird ein Fehler zurückgegeben (z. b. failedtransaction Code = 5002 ein unbekannter Fehler ist aufgetreten).

Hinweis: Storekit funktioniert nicht im IOS-Simulator. Wenn Sie die Anwendung im IOS-Simulator ausführen, protokolliert storekit eine Warnung, wenn die Anwendung versucht, die Zahlungs Warteschlange abzurufen. Das Testen des Stores muss auf den eigentlichen Geräten durchgeführt werden.   
   
   
   
 Wichtig: Melden Sie sich nicht mit Ihrem Testkonto in der Anwendung "Einstellungen" an. Sie können die Anwendung "Einstellungen" verwenden, um sich von einem beliebigen vorhandenen Apple ID-Konto anzumelden. Anschließend müssen Sie in der *in-App-Kauf Sequenz* darauf warten, dass Sie sich mit einer Apple-Test-ID anmelden.   
   
   
   

Wenn Sie versuchen, sich mit einem Testkonto beim echten Store anzumelden, wird es automatisch in eine echte Apple-ID konvertiert. Dieses Konto kann nicht mehr für Tests verwendet werden.

Um storekit-Code zu testen, müssen Sie sich von Ihrem regulären iTunes-Testkonto abmelden und sich mit einem speziellen Testkonto (in iTunes Connect erstellt) anmelden, das mit dem Test Speicher verknüpft ist. Wenn Sie sich beim aktuellen Konto abmelden möchten, besuchen Sie die **Einstellungen > iTunes und App Store** , wie hier gezeigt:

 [![](store-kit-overview-and-retreiving-product-information-images/image16.png "Wenn Sie sich beim aktuellen Konto abmelden möchten, besuchen Sie die Einstellungen iTunes und App Store.")](store-kit-overview-and-retreiving-product-information-images/image16.png#lightbox)
 
Melden Sie sich dann mit einem Testkonto an, *Wenn es von storekit in Ihrer APP angefordert*wird:



Klicken Sie zum Erstellen von Test Benutzern in iTunes Connect auf der Hauptseite auf **Benutzer und Rollen** .

 [![](store-kit-overview-and-retreiving-product-information-images/image17.png "Klicken Sie zum Erstellen von Test Benutzern in iTunes Connect auf der Hauptseite auf Benutzer und Rollen.")](store-kit-overview-and-retreiving-product-information-images/image17.png#lightbox)

**Sandkasten Tester** auswählen

 [![](store-kit-overview-and-retreiving-product-information-images/image18.png "Auswählen der Sandkasten Tester")](store-kit-overview-and-retreiving-product-information-images/image18.png#lightbox)

Die Liste vorhandener Benutzer wird angezeigt. Sie können einen neuen Benutzer hinzufügen oder einen vorhandenen Datensatz löschen. Im Portal ist es (derzeit) nicht möglich, vorhandene Test Benutzer anzuzeigen oder zu bearbeiten. es wird daher empfohlen, dass Sie einen guten Datensatz für jeden erstellten Test Benutzer (insbesondere das Kennwort, das Sie zuweisen) beibehalten. Nachdem Sie einen Test Benutzer gelöscht haben, kann die e-Mail-Adresse für ein anderes Testkonto nicht wieder verwendet werden.  
   
 [![](store-kit-overview-and-retreiving-product-information-images/image19.png "Die Liste vorhandener Benutzer wird angezeigt.")](store-kit-overview-and-retreiving-product-information-images/image19.png#lightbox)   
   
 Neue Test Benutzer haben ähnliche Attribute wie eine echte Apple-ID (z. b. Name, Kennwort, Geheimnis Frage und-Antwort). Behalten Sie einen Datensatz aller hier eingegebenen Details bei. Im Feld **iTunes Store auswählen** wird festgelegt, welche Währung und Sprache die in-App-Käufe verwenden, wenn Sie als dieser Benutzer angemeldet werden.

 [![](store-kit-overview-and-retreiving-product-information-images/image20.png "Im Feld iTunes Store auswählen werden die Währung und die Sprache des Benutzers für Ihre in-App-Einkäufe festgelegt.")](store-kit-overview-and-retreiving-product-information-images/image20.png#lightbox)

## <a name="retrieving-product-information"></a>Abrufen von Produktinformationen

Der erste Schritt beim Verkauf eines in-App-Kauf Produkts ist die Anzeige: Abrufen des aktuellen Preises und der Beschreibung aus dem App Store für die Anzeige.   
   
   
   
 Unabhängig davon, welche Art von Produkten von einer APP verkauft wird (für einen nutzbaren, nicht verwendbaren oder einen Abonnementtyp), ist der Prozess zum Abrufen von Produktinformationen für die Anzeige identisch. Der inapppurchasesample-Code, der diesen Artikel begleitet, enthält ein Projekt mit dem Namen *Verbrauchsmaterialien* , das zeigt, wie Produktionsinformationen für die Anzeige abgerufen werden. Es zeigt Folgendes:

- Erstellen Sie eine Implementierung `SKProductsRequestDelegate` von, und `ReceivedResponse` implementieren Sie die abstrakte-Methode. Im Beispielcode wird die `InAppPurchaseManager` -Klasse aufgerufen. 
- Überprüfen Sie mit storekit, ob Zahlungen zulässig sind ( `SKPaymentQueue.CanMakePayments` mithilfe von). 
- Instanziieren Sie `SKProductsRequest` eine mit den Produkt-IDs, die in iTunes Connect definiert wurden. Dies erfolgt in der-Methode des `InAppPurchaseManager.RequestProductData` -Beispiels. 
- Ruft die Start-Methode für `SKProductsRequest` den auf. Dadurch wird ein asynchroner Aufrufe der App Store-Server ausgelöst. Der Delegat `InAppPurchaseManager` () wird mit den Ergebnissen zurückgerufen. 
- Die ( `InAppPurchaseManager` ) `ReceivedResponse` -Methode des Delegaten aktualisiert die Benutzeroberfläche mit den Daten, die aus dem App Store zurückgegeben werden (Produktpreise & Beschreibungen oder Nachrichten zu ungültigen Produkten). 

Die gesamte Interaktion sieht wie folgt aus ( **storekit** ist in ios integriert, und der **App Store** steht für Apple-Server):

 [![](store-kit-overview-and-retreiving-product-information-images/image21.png "Abrufen des Produkt Informations Diagramms")](store-kit-overview-and-retreiving-product-information-images/image21.png#lightbox)

### <a name="displaying-product-information-example"></a>Anzeigen von Produkt Informations Beispielen

Der Beispielcode " [inapppurchasesample](https://docs.microsoft.com/samples/xamarin/ios-samples/storekit) *Verbrauchs* Tests" veranschaulicht, wie Produktinformationen abgerufen werden können. Auf dem Hauptbildschirm des Beispiels werden Informationen für zwei Produkte angezeigt, die aus dem App Store abgerufen werden:   
   
   
   
 [![](store-kit-overview-and-retreiving-product-information-images/image23.png "Im Hauptbildschirm werden Informationsprodukte angezeigt, die aus dem App Store abgerufen wurden.")](store-kit-overview-and-retreiving-product-information-images/image23.png#lightbox)   
   
   
   
 Der Beispielcode zum Abrufen und Anzeigen von Produktinformationen wird unten ausführlicher erläutert.


#### <a name="viewcontroller-methods"></a>ViewController-Methoden

Die `ConsumableViewController` -Klasse verwaltet die Anzeige der Preise für zwei Produkte, deren Produkt-IDs in der-Klasse hart codiert sind.

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

Auf Klassenebene sollte auch ein NSObject deklariert sein, das zum Einrichten eines `NSNotificationCenter` Beobachters verwendet wird:

```csharp
NSObject priceObserver;
```

In der viewwillview-Methode wird der Observer erstellt und mithilfe des Standard-Benachrichtigungs Centers zugewiesen:

```csharp
priceObserver = NSNotificationCenter.DefaultCenter.AddObserver (
  InAppPurchaseManager.InAppPurchaseManagerProductsFetchedNotification,
(notification) => {
   // display code goes here, to handle the response from the App Store
}
```

   
   
 Am Ende der `ViewWillAppear` Methode wird die `RequestProductData` -Methode aufgerufen, um die storekit-Anforderung zu initiieren. Nachdem diese Anforderung erfolgt ist, kontaktiert storekit asynchron die Apple-Server, um die Informationen zu erhalten und Sie wieder in Ihre APP zu übermitteln. Dies wird durch die `SKProductsRequestDelegate` Unterklasse ( `InAppPurchaseManager`) erreicht, die im nächsten Abschnitt erläutert wird.

```csharp
iap.RequestProductData(products);
```

Der Code zum Anzeigen von Preis und Beschreibung ruft einfach die Informationen aus dem skproduct ab und weist Sie UIKit-Steuerelementen zu (Beachten Sie, `LocalizedTitle` dass `LocalizedDescription` die und – storekit automatisch den richtigen Text und die richtigen Preise auf der Grundlage von die Kontoeinstellungen des Benutzers). Der folgende Code gehört zu der Benachrichtigung, die wir oben erstellt haben:

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

Schließlich sollte die `ViewWillDisappear` -Methode sicherstellen, dass der Beobachter entfernt wird:

```csharp
NSNotificationCenter.DefaultCenter.RemoveObserver (priceObserver);
```

#### <a name="skproductrequestdelegate-inapppurchasemanager-methods"></a>Skproductrequestdelegat-Methoden (inapppurchasemanager)

Die `RequestProductData` -Methode wird aufgerufen, wenn die Anwendung Produktpreise und andere Informationen abrufen möchte. Es analysiert die Auflistung von Produkt-IDs in den richtigen Datentyp und erstellt dann `SKProductsRequest` eine mit diesen Informationen. Wenn Sie die Start-Methode aufrufen, wird eine Netzwerk Anforderung an die Server von Apple gerichtet. Die Anforderung wird asynchron ausgeführt und ruft die `ReceivedResponse` -Methode des Delegaten auf, wenn Sie erfolgreich abgeschlossen wurde.

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

IOS leitet die Anforderung abhängig von dem Bereitstellungs Profil, mit dem die Anwendung ausgeführt wird, automatisch an die Sandbox-oder Produktionsversion des App Stores weiter – wenn Sie also Ihre APP entwickeln oder testen, erhält die Anforderung Zugriff auf jedes Produkt. in iTunes Connect konfiguriert (auch solche, die noch nicht von Apple übermittelt oder genehmigt wurden). Wenn sich Ihre Anwendung in der Produktion befindet, werden von storekit-Anforderungen nur Informationen für **genehmigte** Produkte zurückgegeben.   
   
   
   
 Die `ReceivedResponse` überschriebene-Methode wird aufgerufen, nachdem die Server von Apple mit Daten geantwortet haben. Da dies im Hintergrund aufgerufen wird, sollte der Code die gültigen Daten analysieren und eine Benachrichtigung verwenden, um die Produktinformationen an alle ViewController zu senden, die für diese Benachrichtigung "lauschen". Der Code zum Erfassen gültiger Produktinformationen und Senden einer Benachrichtigung wird unten dargestellt:

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

Obwohl die- `RequestFailed` Methode nicht im Diagramm angezeigt wird, sollte Sie auch überschrieben werden, damit Sie dem Benutzer Feedback geben können, wenn die App Store-Server nicht erreichbar sind (oder ein anderer Fehler auftritt). Im Beispielcode wird lediglich in die Konsole geschrieben, aber eine echte Anwendung könnte eine Abfrage `error.Code` der Eigenschaft durchführen und benutzerdefiniertes Verhalten (z. b. eine Warnung für den Benutzer) implementieren.

```csharp
public override void RequestFailed (SKRequest request, NSError error)
{
   Console.WriteLine (" ** InAppPurchaseManager RequestFailed() " + error.LocalizedDescription);
}
```

Dieser Screenshot zeigt die Beispielanwendung unmittelbar nach dem Laden (wenn keine Produktinformationen verfügbar sind):

 [![](store-kit-overview-and-retreiving-product-information-images/image24.png "Die Beispiel-App unmittelbar nach dem Laden, wenn keine Produktinformationen verfügbar sind")](store-kit-overview-and-retreiving-product-information-images/image24.png#lightbox)

## <a name="invalid-products"></a>Ungültige Produkte

Eine `SKProductsRequest` kann auch eine Liste der ungültigen Produkt-IDs zurückgeben. Ungültige Produkte werden in der Regel aufgrund eines der folgenden zurückgegeben:   
   
   
   
 Die **Produkt-ID wurde falsch** geschrieben – nur gültige Produkt-IDs werden akzeptiert.   
   
 Das **Produkt wurde nicht genehmigt** – beim Testen sollten alle Produkte, die für den Verkauf gelöscht werden, von einem `SKProductsRequest`zurückgegeben werden. in der Produktion werden jedoch nur genehmigte Produkte zurückgegeben.   
   
 Die **App-ID ist nicht explizit** – Platzhalter-App-IDs (mit einem Sternchen) erlauben keinen in-App-Einkauf.   
   
 **Falsches Bereitstellungs Profil** – Wenn Sie Änderungen an ihrer Anwendungskonfiguration im Bereitstellungs Portal vornehmen (z. b. das Aktivieren von in-App-Käufen), sollten Sie das richtige Bereitstellungs Profil beim Erstellen der APP erneut generieren und verwenden.   
   
 der **Vertrag für kostenpflichtige IOS-Anwendungen ist nicht vorhanden** – storekit-Features funktionieren überhaupt nicht, es sei denn, es ist ein gültiger Vertrag für Ihr Apple-Entwicklerkonto vorhanden.   
   
 **Die Binärdatei weist den Status "abgelehnt" auf** – wenn eine zuvor gesendete Binärdatei im abgelehnten Zustand vorliegt (entweder vom App Store-Team oder vom Entwickler), funktionieren die storekit-Funktionen nicht.

Die `ReceivedResponse` -Methode im Beispielcode gibt die ungültigen Produkte an die Konsole aus:

```csharp
public override void ReceivedResponse (SKProductsRequest request, SKProductsResponse response)
{
   // code removed for clarity
   foreach (string invalidProductId in response.InvalidProducts) {
       Console.WriteLine("Invalid product id: " + invalidProductId );
   }
}
```

## <a name="displaying-localized-prices"></a>Anzeigen lokalisierter Preise

Preisstufen geben einen bestimmten Preis für jedes Produkt in allen internationalen App Stores an. Um sicherzustellen, dass die Preise für jede Währung ordnungsgemäß angezeigt werden, verwenden Sie die `SKProductExtension.cs`folgende Erweiterungsmethode (definiert in) anstelle `SKProduct`der Preis Eigenschaft der einzelnen:

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

Der Code, mit dem der Titel der Schaltfläche festgelegt wird, verwendet die-Erweiterungsmethode wie folgt:

```csharp
string Buy = "Buy {0}"; // or a localizable string
buy5Button.SetTitle(String.Format(Buy, product.LocalizedPrice()), UIControlState.Normal);
```

Die Verwendung von zwei verschiedenen iTunes-Testkonten (eine für den American Store und eine für den japanischen Store) führt zu den folgenden Screenshots:   
   
   
   
 [![](store-kit-overview-and-retreiving-product-information-images/image25.png "Zwei verschiedene iTunes-Testkonten, die sprachspezifische Ergebnisse zeigen")](store-kit-overview-and-retreiving-product-information-images/image25.png#lightbox)   
   
   
   
 Beachten Sie, dass sich der Speicher auf die Sprache auswirkt, die für Produktinformationen und Preiswährung verwendet wird, während sich die Spracheinstellung des Geräts auf Bezeichnungen und andere lokalisierte Inhalte auswirkt.   
   
   
   
 Wenn Sie ein anderes Store-Testkonto verwenden möchten, müssen Sie sich in den **Einstellungen > iTunes und App Store** **Abmelden** und die Anwendung erneut starten, um sich mit einem anderen Konto anzumelden. Um die Sprache des Geräts zu ändern, wechseln Sie zu **Einstellungen > Allgemein > Internationale > Sprache**.

