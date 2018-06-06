---
title: Abonnements und Berichterstattung in Xamarin.iOS
description: Dieses Dokument beschreibt die Abonnements nicht erneuern, kostenlose Abonnements automatisch erneuerbar Abonnements und iTunes Connect für Berichte an diese Elemente verwenden.
ms.prod: xamarin
ms.assetid: 27EE4234-07F5-D2CD-DC1C-86E27C20141E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 7e0873107a60b48e5ebfd8e159f3bf3b85d02867
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787028"
---
# <a name="subscriptions-and-reporting-in-xamarinios"></a>Abonnements und Berichterstattung in Xamarin.iOS

## <a name="about-non-renewing-subscriptions"></a>Informationen zu Abonnements nicht zu erneuern

Nicht erneuern des Abonnements sind für Produkte vorgesehen, die die Verkaufszahlen für einen Dienst mit einer zeitbeschränkung z. B. (eine Woche Zugriff auf eine navigationsanwendung) oder zeitlich begrenzten Zugriff auf ein Datenarchiv darstellen.   
   
Die Hauptunterschiede zwischen Abonnements nicht erneuern und anderen Typen:

-  Die Produktdefinition in iTunes Connect umfasst nicht den Begriff. Der Anwendungscode muss ableiten die Gültigkeitsdauer von der Produkt-ID sein. 
-  Sie können mehrere Male (z. B. ein nutzbar Produkt) erworben werden. Anwendungen müssen zum Verwalten der Begriff/Ablauf von Abonnements und Erneuerung und verhindern, dass der Benutzer überlappende Abonnements erwerben. 
-  Die Verkaufszahlen werden von der StoreKit wiederherstellen-Funktion nicht unterstützt. Wenn das Abonnement über Geräte eines Benutzers verfügbar sein soll, müssen die Anwendung entwerfen und Implementieren dieses Feature in Verbindung mit einem Remoteserver. Anwendungen sind zuständig für das Sichern des Abonnementstatus für Fälle, wenn ein Gerät gesichert ist dann wiederhergestellt-aus-Sicherung. 
-  Übersicht über die Implementierung
-  Nicht erneuern des Abonnements sollten normalerweise implementiert werden, mithilfe des Workflows für den Server übermittelt und verwaltet wie nutzbar Produkte. 


## <a name="about-free-subscriptions"></a>Über kostenlose Abonnements

Kostenlose Abonnements ermöglichen Entwicklern das kostenlose Inhalte in Newsstand apps speichern (sie können in nicht-Newsstand-apps verwendet werden). Sobald ein kostenloses Abonnement gestartet wird, wird er auf Geräte des Benutzers verfügbar sein. Kostenlose Abonnements laufen nie ab; Sie beenden nur auf, wenn die Anwendung deinstalliert wird.

### <a name="implementation-overview"></a>Übersicht über die Implementierung

Kostenlose Abonnements Verhalten sich ähnlich wie automatische erneuerbar Abonnements. Die Anwendung muss in iTunes Connect ein kostenloses Abonnement Produkt käuflich "' aufweisen. Wenn vom Benutzer erworben haben, sollte der Kauf kostenloses Abonnement z. B. ein Abonnementversionsprodukt automatisch erneuerbar überprüft werden. Kostenloses Abonnement Transaktionen können wiederhergestellt werden.


## <a name="about-auto-renewable-subscriptions"></a>Informationen zu automatischen erneuerbar Abonnements

Automatische erneuerbar Abonnements werden hauptsächlich in Newsstand Anwendungen eingesetzt werden. Sie stellen ein Produkt, das den Benutzerzugriff auf dynamischen Inhalt für einen bestimmten Zeitraum, gewährt der App im iTunes Connect (durch Festlegen von 7 Tagen auf 1 Jahr zoomzustand Zeiträume) konfiguriert wird. Abonnements erneuern automatisch, die Benutzer Apple-ID am Ende der jeden Abonnementzeitraum nähern, wenn der Benutzer "OPTS"-Out. Diese Produkttyp eignet sich für Magazin oder News-Abonnements, erhält der Benutzer Zugriff auf jedes Problem, das veröffentlicht wird, während ihr Abonnement gültig ist.

### <a name="implementation-overview"></a>Übersicht über die Implementierung

Automatische erneuerbar Abonnements mithilfe der Server-Delivered Produkte Workflow implementiert werden sollte (finden Sie in der *Receipt-Überprüfung und Server-Delivered Produkte* Abschnitt).

#### <a name="shared-secret"></a>Gemeinsamer geheimer Schlüssel

Die In-App Kauf gemeinsamer geheimer Schlüssel muss in der JSON-Anforderung verwendet werden, bei der Überprüfung der automatischen erneuerbar Abonnements auf dem Server. Der gemeinsame geheime Schlüssel wird über iTunes Connect erstellt/zugegriffen.

Aus dem iTunes Connect-Startseite auswählen **meine Apps**:   
   
 [![](subscriptions-and-reporting-images/image2.png "„My Apps“ (Meine Apps) auswählen")](subscriptions-and-reporting-images/image2.png#lightbox)  
 
Wählen Sie eine Anwendung, und klicken Sie auf die **In App-Einkäufe** Registerkarte:

[![](subscriptions-and-reporting-images/image6.png "Klicken Sie auf der Registerkarte \"In-App-Einkäufe\"")](subscriptions-and-reporting-images/image6.png#lightbox)

Wählen Sie in den unteren Rand der Seite, **anzeigen oder erstellen Sie einen gemeinsamen geheimen Schlüssel**:
   
 [![](subscriptions-and-reporting-images/image40.png "Wählen Sie die Sicht oder generieren Sie einen gemeinsamen geheimen Schlüssel")](subscriptions-and-reporting-images/image40.png#lightbox)

 [![](subscriptions-and-reporting-images/image41.png "Generieren Sie einen gemeinsamen geheimen Schlüssel")](subscriptions-and-reporting-images/image41.png#lightbox)   
   
   
   
 Um den gemeinsamen geheimen Schlüssel zu verwenden, fügen Sie ihn in der JSON-Nutzlast, die Apple Server gesendet wird, wenn eine Bestätigung in app-Käufe für ein Abonnement automatisch erneuerbar wie folgt überprüfen:

```csharp
{
   "receipt-data" : "(receipt bytes here)",
   "password"     : "(shared secret bytes here)"
}
```

Die Antwort Statusfeld ist 0 (null), wenn die Bestellung gültig ist, als mit anderen Typen.

#### <a name="downloading-items-after-the-initial-subscription-term"></a>Herunterladen von Elementen nach dem ersten Abonnements.

Im Rahmen des Abonnements Produkte bereitstellen sollten den Code häufig zu den neuesten bekannten Empfang für Apple Server überprüfen. Wenn ein Abonnement Auto-seit der letzten Überprüfung erneuert wurde, enthält die JSON-Antwort weitere Felder, die die Anwendung die Transaktion zu benachrichtigen, die aufgetreten ist (die sollten die Gültigkeit des Abonnements erweitern). Die JSON-Antwort enthält:

```csharp
{
   "status" : 0,
   "receipt" : { (receipt here) },
   "latest_receipt" : "(base-64 encoded receipt here)",
   "latest_receipt_info" : { (latest receipt info here) }
}
```

Wenn der Status 0 (null) ist das Abonnement noch gültig ist, und die anderen Felder enthalten gültige Daten. Wenn der Status 21006 ist, und klicken Sie dann das Abonnement abgelaufen ist. Finden Sie unter der [überprüfen eine automatische erneuerbar Abonnement Bestätigung](https://developer.apple.com/library/ios/releasenotes/General/ValidateAppStoreReceipt/Chapters/ValidateRemotely.html) Dokumentation für andere Fehlercodes.

#### <a name="restoring-auto-renewable-subscriptions"></a>Automatische erneuerbar Abonnements wiederherstellen

Sie erhalten wieder mehrere Transaktionen – die ursprüngliche Einkaufstransaktion plus eine separate Transaktion für jeden Zeitraum, der das Abonnement erneuert wurde. In diesem Fall müssen Sie eine nachverfolgen, die Start- und Begriffe, um zu verstehen, was die Gültigkeitsdauer ist.   
   
   
   
 Das Objekt SKPaymentTransaction enthält keine Abonnements. – verwenden Sie eine andere Produkt-ID für jeden Begriff und Code schreiben, der die Abonnementzeitraum ab dem Kaufdatum der Transaktion zu extrapolieren, kann.

#### <a name="testing-auto-renewal"></a>Testen die automatische Erneuerung

Um Abonnements testen zu vereinfachen, werden deren Dauer komprimiert, wenn Sie in der Sandbox testen. 1 Woche Abonnements erneuern alle 3 Minuten, 1 Jahr Abonnements pro Stunde zu verlängern. Abonnements werden automatisch maximal 6 Mal beim Testen in der Sandbox verlängern.

## <a name="reporting"></a>Berichterstellung

iTunes Connect ( [itunesconnect.apple.com](http://itunesconnect.apple.com)) bietet:   
   
 **Vertriebs- und Trends** – downloads, Updates und in-app-Einkäufe zeigt Details der app.   
   
 **Zahlungen und Finanzberichte** – Details das Einkommen verdienten durch Ihre apps sowie die Liste Zahlungen, die für Sie und welchen Sie geschuldet werden vorgenommen wurden.

Ein Beispiel für Vertrieb und Trends Bericht wird unten gezeigt:   

 [![](subscriptions-and-reporting-images/image42.png "Ein Beispiel für Vertrieb und Trends Bericht")](subscriptions-and-reporting-images/image42.png#lightbox)   
   
 Es gibt auch eine [ **ITC verbinden Mobile**iOS-app (iTunes Link)](http://itunes.apple.com/us/app/itunes-connect-mobile/id376771144?mt=8).
iPhone-Screenshots für einige der verfügbaren Statistiken sind hier aufgeführt:   
   
 [![](subscriptions-and-reporting-images/image43.png "iPhone-Screenshots für einige der verfügbaren Statistiken")](subscriptions-and-reporting-images/image43.png#lightbox)
