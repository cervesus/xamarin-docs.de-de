---
title: Abonnements und Berichterstellung in Xamarin.iOS
description: Dieses Dokument beschreibt keine monatlichen Abonnements, Abonnements mit einer kostenlosen, automatische erneuerbar Abonnements und iTunes Connect verwenden, um diese Elemente zu melden.
ms.prod: xamarin
ms.assetid: 27EE4234-07F5-D2CD-DC1C-86E27C20141E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 4e63894cb862db3b5b5a1e7a2bebd79160c311a9
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121225"
---
# <a name="subscriptions-and-reporting-in-xamarinios"></a>Abonnements und Berichterstellung in Xamarin.iOS

## <a name="about-non-renewing-subscriptions"></a>Informationen zu nicht-verlängern von Abonnements

Abonnements nicht erneuert werden für Produkte vorgesehen, die die Verkaufszahlen für einen Dienst mit einem zeitbeschränkungen, z. B. (eine Woche des Zugriffs auf eine navigationsanwendung) oder zeitlich begrenzten Zugriff an ein Datenarchiv darstellen.   
   
Die Hauptunterschiede zwischen Abonnements nicht erneuern und andere Produkttypen:

-  Die Produktdefinition in iTunes Connect umfasst den Begriff nicht. Der Anwendungscode muss Ableiten von der Gültigkeitsdauer aus der Produkt-ID sein. 
-  Sie können mehrere Male (z. B. ein nutzbar Produkt) erworben werden. Anwendungen müssen zum Verwalten der Abonnements Begriff/Ablauf und Verlängerung, und verhindern, dass des Benutzers sich überschneidenden Abonnements erwerben. 
-  Die Käufe werden von der Funktion StoreKit Wiederherstellen nicht unterstützt. Wenn das Abonnement über Geräte eines Benutzers verfügbar sein soll, müssen die Anwendung entwerfen und Implementieren dieses Feature in Verbindung mit einem Remoteserver. Anwendungen sind auch zuständig für die Sicherung des Abonnementstatus für Fälle, wenn ein Gerät gesichert ist. Klicken Sie dann wiederhergestellt-aus-Backup. 
-  Übersicht über die Implementierung
-  Abonnements nicht erneuern sollte normalerweise implementiert werden, mithilfe der Workflow-Server bereitgestellt und verwaltet wie verbrauchsartikeln. 


## <a name="about-free-subscriptions"></a>Informationen zu kostenlosen Abonnements

Kostenlose Abonnements ermöglichen Entwicklern, kostenlose Inhalte im Zeitungskiosk-apps (sie können nicht in nicht aus dem Zeitungskiosk-apps verwendet werden). Sobald ein kostenloses Abonnement gestartet wird, wird es auf Geräte des Benutzers verfügbar sein. Kostenlose Abonnements laufen nie ab; Sie enden nur, wenn die Anwendung deinstalliert wird.

### <a name="implementation-overview"></a>Übersicht über die Implementierung

Kostenlose Abonnements Verhalten sich viel wie automatische erneuerbar Abonnements verwendet wird. Die Anwendung benötigen in iTunes Connect ein kostenloses Abonnement Produkt für "Purchase" verfügbar. Wenn vom Benutzer erworben werden, sollte der Kauf kostenloses Abonnement z. B. eine automatische erneuerbar Subscription-Produkts überprüft werden. Kostenloses Abonnement Transaktionen können wiederhergestellt werden.


## <a name="about-auto-renewable-subscriptions"></a>Informationen zu automatischen erneuerbar-Abonnements

Automatische erneuerbar Abonnements werden hauptsächlich in Zeitungskiosk-Anwendungen verwendet werden. Sie stellen dar, ein Produkt, das den Benutzerzugriff auf dynamische Inhalte für einen bestimmten Zeitraum, gewährt der in iTunes Connect (festgelegt werden, reichen von 7 Tagen bis zu 1 Jahr) konfiguriert wird. Abonnements wird automatisch verlängert, die Apple-ID-Benutzer am Ende jeden Abonnementzeitraum Gebühren, es sei denn, der Benutzer "OPTS"-Out. Diese Produkttyp eignet sich gut für Magazin oder News-Abonnements, in denen erhält der Benutzer den Zugriff auf jedes Problem, das veröffentlicht wird, während ihr Abonnement gültig ist.

### <a name="implementation-overview"></a>Übersicht über die Implementierung

Automatische erneuerbar Abonnements sollte mit dem Workflow Server-Delivered Produkte implementiert werden (finden Sie in der *Empfang, Überprüfung und Server-Delivered Produkte* Abschnitt).

#### <a name="shared-secret"></a>Gemeinsamer geheimer Schlüssel

Die In-App-Kauf gemeinsamer geheimer Schlüssel muss in der JSON-Anforderung verwendet werden, bei der Überprüfung der automatischen erneuerbar Abonnements auf dem Server. Der gemeinsame geheime Schlüssel ist über iTunes Connect erstellt/zugegriffen.

Aus dem iTunes Connect-Startseite wählen **meine Apps**:   
   
 [![](subscriptions-and-reporting-images/image2.png "„My Apps“ (Meine Apps) auswählen")](subscriptions-and-reporting-images/image2.png#lightbox)  
 
Wählen Sie eine Anwendung, und klicken Sie auf die **In-App-Käufe** Registerkarte:

[![](subscriptions-and-reporting-images/image6.png "Klicken Sie auf der Registerkarte \"In-App-Käufe\"")](subscriptions-and-reporting-images/image6.png#lightbox)

Wählen Sie in den unteren Rand der Seite, **anzeigen oder einen gemeinsamen geheimen Schlüssel generieren**:
   
 [![](subscriptions-and-reporting-images/image40.png "Wählen Sie die Ansicht oder generieren einen gemeinsamen geheimen Schlüssel")](subscriptions-and-reporting-images/image40.png#lightbox)

 [![](subscriptions-and-reporting-images/image41.png "Generieren Sie einen gemeinsamen geheimen Schlüssel")](subscriptions-and-reporting-images/image41.png#lightbox)   
   
   
   
 Um den gemeinsamen geheimen Schlüssel verwenden zu können, schließen Sie sie in der JSON-Nutzlast, die Apple Server gesendet wird, wenn Sie einen in-app-Käufe Beleg für ein Auto erneuerbar-Abonnement wie folgt überprüfen:

```csharp
{
   "receipt-data" : "(receipt bytes here)",
   "password"     : "(shared secret bytes here)"
}
```

Feld "Status der Antwort" werden 0 (null), ist der Kauf gültig ist, als bei anderen Produktarten.

#### <a name="downloading-items-after-the-initial-subscription-term"></a>Herunterladen von Elementen nach der ersten Abonnementlaufzeit

Im Rahmen der Bereitstellung von Abonnementprodukten sollten der Code häufig überprüfen, ob die letzte bekannte Empfangsbestätigung für Apple Server. Wenn ein Abonnement automatisch seit der letzten Überprüfung verlängert wurde, enthält die JSON-Antwort weitere Felder, die die Anwendung der Transaktion zu benachrichtigen, die aufgetreten ist (die sollten die Gültigkeit des Abonnements erweitern). Enthält die JSON-Antwort:

```csharp
{
   "status" : 0,
   "receipt" : { (receipt here) },
   "latest_receipt" : "(base-64 encoded receipt here)",
   "latest_receipt_info" : { (latest receipt info here) }
}
```

Wenn der Status 0 (null) ist, klicken Sie dann das Abonnement noch gültig ist und gültige Daten enthält, die anderen Felder. Wenn der Status 21006 ist, und klicken Sie dann das Abonnement abgelaufen ist. Finden Sie unter den [überprüfen einen automatischen erneuerbares Abonnement Beleg](https://developer.apple.com/library/ios/releasenotes/General/ValidateAppStoreReceipt/Chapters/ValidateRemotely.html) Dokumentation für andere Fehlercodes.

#### <a name="restoring-auto-renewable-subscriptions"></a>Automatische erneuerbar Abonnements wiederherstellen

Sie erhalten wieder mehrere Transaktionen – die ursprüngliche Kauftransaktion und eine separate Transaktion für jeden Zeitraum, die das Abonnement verlängert wurde. Sie müssen nachverfolgen, die Startdaten und Begriffe, um zu verstehen, was die Gültigkeitsdauer ist.   
   
   
   
 Das Objekt SKPaymentTransaction enthält keine Abonnements – verwenden Sie eine andere Produkt-ID für jeden Begriff und Code schreiben, der den Abonnementzeitraum ab dem Kaufdatum der Transaktion extrapolieren kann.

#### <a name="testing-auto-renewal"></a>Testen die automatische Verlängerung

Um Abonnements zu testen zu vereinfachen, werden deren Dauer komprimiert, beim Testen in der Sandbox. 1 Woche verlängert die Abonnements alle 3 Minuten, 1 Jahr Verlängerung von Abonnements pro Stunde. Abonnements werden automatisch bis zu 6-Mal während des Testens in der Sandbox verlängert.

## <a name="reporting"></a>Berichterstellung

iTunes Connect ( [itunesconnect.apple.com](http://itunesconnect.apple.com)) bietet:   
   
 **Vertrieb und Trends** – zeigt die Details der app-downloads, Updates und in-app-Käufe.   
   
 **Zahlungen und Finanzberichte** – erläutert, das Einkommen, die durch Ihre apps sowie die Auflistung von Zahlungen, die vorgenommen wurden und wie viel Sie Zahlen sind.

Ein Beispiel für Vertrieb und Trends Bericht ist unten dargestellt:   

 [![](subscriptions-and-reporting-images/image42.png "Ein Beispiel für Vertrieb und Trends-Bericht")](subscriptions-and-reporting-images/image42.png#lightbox)   
   
 Es gibt auch eine [ **ITC Verbinden mobiler**iOS-app (iTunes-Link)](http://itunes.apple.com/us/app/itunes-connect-mobile/id376771144?mt=8).
iPhone-Screenshot für einige der den dazu verfügbaren Statistiken werden hier angezeigt:   
   
 [![](subscriptions-and-reporting-images/image43.png "iPhone-Screenshot für einige der den dazu verfügbaren Statistiken")](subscriptions-and-reporting-images/image43.png#lightbox)
