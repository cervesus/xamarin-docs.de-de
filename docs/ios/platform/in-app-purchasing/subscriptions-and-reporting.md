---
title: Abonnements und Berichterstellung in xamarin. IOS
description: In diesem Dokument werden Abonnements ohne Erneuerung, Kostenlose Abonnements, Abonnements für die automatische Wiederherstellung und die Verwendung von iTunes Connect zum Melden dieser Elemente beschrieben.
ms.prod: xamarin
ms.assetid: 27EE4234-07F5-D2CD-DC1C-86E27C20141E
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 534ecb6a2f779875a6934306c7f9956450d880ed
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86938501"
---
# <a name="subscriptions-and-reporting-in-xamarinios"></a>Abonnements und Berichterstellung in xamarin. IOS

## <a name="about-non-renewing-subscriptions"></a>Informationen zu nicht erneuerten Abonnements

Abonnements, die nicht erneuert werden, sind für Produkte bestimmt, die den Verkauf eines diengs mit einer Zeitbeschränkung darstellen (z. b. der Zugriff einer Woche auf eine Navigations Anwendung oder der zeitlich eingeschränkte Zugriff auf ein Datenarchiv).   

Wichtige Unterschiede zwischen nicht erneuerten Abonnements und anderen Produkttypen:

- Die Produktdefinition in iTunes Connect enthält nicht den Begriff. Der Anwendungscode muss die Gültigkeitsdauer von der Produkt-ID ableiten können.
- Sie können mehrmals gekauft werden (z. b. ein nutzbares Produkt). Anwendungen sind erforderlich, um die Abonnement Laufzeit bzw. den Ablauf und die Erneuerung zu verwalten, und verhindern, dass der Benutzer überlappende Abonnements kauft.
- Die Einkäufe werden von der storekit-Wiederherstellungs Funktion nicht unterstützt. Wenn das Abonnement auf allen Geräten eines Benutzers verfügbar sein soll, muss diese Funktion in Verbindung mit einem Remote Server entworfen und implementiert werden. Anwendungen sind auch dafür verantwortlich, den Abonnement Status zu sichern, wenn ein Gerät gesichert und dann wieder hergestellt wird.
- Implementierungs Übersicht
- Abonnements, die nicht erneuert werden, sollten normalerweise mit dem vom Server übermittelten Workflow implementiert und wie z. b. nutzbare Produkte verwaltet werden

## <a name="about-free-subscriptions"></a>Informationen zu kostenlosen Abonnements

Mit kostenlosen Abonnements können Entwickler kostenlose Inhalte in NewsStand-apps einfügen (Sie können nicht in nicht-News-Kiosk-Apps verwendet werden). Nachdem ein kostenloses Abonnement gestartet wurde, ist es auf allen Geräten des Benutzers verfügbar. Kostenlose Abonnements laufen nie ab. Sie enden nur, wenn die Anwendung deinstalliert wird.

### <a name="implementation-overview"></a>Implementierungs Übersicht

Kostenlose Abonnements Verhalten sich ähnlich wie Abonnements mit automatischer verfügbaren Abonnements. Die Anwendung muss über ein kostenloses Abonnement Produkt verfügen, das für "Purchase" in iTunes Connect verfügbar ist. Beim Erwerb durch den Benutzer sollte der Erwerb eines kostenlosen Abonnements wie ein Produkt mit automatischer Produktnutzung überprüft werden. Kostenlose Abonnement Transaktionen können wieder hergestellt werden.

## <a name="about-auto-renewable-subscriptions"></a>Informationen zu automatischen Abonnements

Automatisch-Abonnements werden in NewsStand-Anwendungen verwendet. Sie stellen ein Produkt dar, das dem Benutzer Zugriff auf dynamischen Inhalt für einen bestimmten Zeitraum gewährt, der in iTunes Connect konfiguriert ist (Zeiträume zwischen 7 Tagen und 1 Jahr festlegen). Abonnements werden automatisch erneuert, wobei die Apple-ID des Benutzers am Ende jedes Abonnementzeitraums berechnet wird, es sei denn, der Benutzer gibt den Vorgang aus. Dieser Produkttyp eignet sich gut für Magazine-oder News-Abonnements, bei denen der Benutzer Zugriff auf jedes Problem erhält, das veröffentlicht wird, während das Abonnement gültig ist.

### <a name="implementation-overview"></a>Implementierungs Übersicht

Abonnements mit automatischer Wiederverwendung sollten mithilfe des Workflows für vom Server übermittelte Produkte implementiert werden (Weitere Informationen finden Sie im Abschnitt über *Prüfung der Empfangs-und Server Produkte* ).

#### <a name="shared-secret"></a>Gemeinsamer geheimer Schlüssel

Der gemeinsame geheime Schlüssel in-App-Käufe muss in der JSON-Anforderung verwendet werden, wenn die auf dem Server automatisch zu überprüfenden Abonnements überprüft werden. Der gemeinsame geheime Schlüssel wird über iTunes Connect erstellt/aufgerufen.

Wählen Sie auf der iTunes Connect-Startseite **meine apps**aus:   

 [![Meine apps auswählen](subscriptions-and-reporting-images/image2.png)](subscriptions-and-reporting-images/image2.png#lightbox)  

Wählen Sie eine Anwendung aus, und klicken Sie auf die Registerkarte **in-App-Käufe** :

[![Klicken Sie auf die Registerkarte in-App-Käufe.](subscriptions-and-reporting-images/image6.png)](subscriptions-and-reporting-images/image6.png#lightbox)

Wählen Sie am unteren Rand der Seite die Option **zum Anzeigen oder Generieren eines gemeinsamen geheimen**Schlüssels aus:

 [![Auswählen eines gemeinsamen geheimen Schlüssels anzeigen oder generieren](subscriptions-and-reporting-images/image40.png)](subscriptions-and-reporting-images/image40.png#lightbox)

 [![Generieren eines gemeinsamen geheimen Schlüssels](subscriptions-and-reporting-images/image41.png)](subscriptions-and-reporting-images/image41.png#lightbox)   

Um den gemeinsamen geheimen Schlüssel zu verwenden, fügen Sie ihn in die JSON-Nutzlast ein, die an die Apple-Server gesendet wird, wenn Sie eine in-App-Kaufbestätigung für ein automatisch verlängertes Abonnement überprüfen, wie hier:

```csharp
{
   "receipt-data" : "(receipt bytes here)",
   "password"     : "(shared secret bytes here)"
}
```

Das Status Feld der Antwort ist 0 (null), wenn der Kauf gültig ist, wie bei anderen Produkttypen.

#### <a name="downloading-items-after-the-initial-subscription-term"></a>Herunterladen von Elementen nach der anfänglichen Abonnement Laufzeit

Im Rahmen der Bereitstellung von Abonnement Produkten sollte der Code häufig den letzten bekannten Empfang der Apple-Server überprüfen. Wenn ein Abonnement seit der letzten Überprüfung automatisch erneuert wurde, enthält die JSON-Antwort zusätzliche Felder, die die Anwendung über die aufgetretene Transaktion Benachrichtigen (wodurch die Gültigkeit der Abonnements verlängert werden sollte). Die JSON-Antwort enthält Folgendes:

```csharp
{
   "status" : 0,
   "receipt" : { (receipt here) },
   "latest_receipt" : "(base-64 encoded receipt here)",
   "latest_receipt_info" : { (latest receipt info here) }
}
```

Wenn der Status 0 (null) lautet, ist das Abonnement weiterhin gültig, und die anderen Felder enthalten gültige Daten. Wenn der Status 21006 lautet, ist das Abonnement abgelaufen. Weitere Fehlercodes finden Sie in der Dokumentation zum über [Prüfen der Bestätigung eines automatischen, nachwachsenden Abonnements](https://developer.apple.com/library/ios/releasenotes/General/ValidateAppStoreReceipt/Chapters/ValidateRemotely.html) .

#### <a name="restoring-auto-renewable-subscriptions"></a>Wiederherstellen automatisch verlänger Abonnements

Sie erhalten mehrere Transaktionen – die ursprüngliche Kauftransaktion und eine separate Transaktion für jeden Zeitraum, für den das Abonnement erneuert wurde. Sie müssen die Startdatums Angaben und-Bedingungen nachverfolgen, um die Gültigkeitsdauer zu verstehen.   

Das skpaymenttransaction-Objekt schließt den Abonnement Begriff nicht ein – Sie sollten für jeden Begriff eine andere Produkt-ID verwenden und Code schreiben, der den Abonnementzeitraum vom Kauf Datum der Transaktion extrapolieren kann.

#### <a name="testing-auto-renewal"></a>Testen der automatischen Verlängerung

Um das Testen von Abonnements zu vereinfachen, werden deren Dauer beim Testen in der Sandbox komprimiert. 1-Wochen-Abonnements werden alle drei Minuten erneuert, 1 Jahre Abonnements werden stündlich verlängert. Abonnements werden beim Testen in der Sandbox automatisch maximal 6 Mal verlängert.

## <a name="reporting"></a>Berichterstellung

iTunes Connect ( [iTunesConnect.Apple.com](https://itunesconnect.apple.com)) bietet Folgendes:   

 **Vertrieb und Trends** – hier werden Details zu app-Downloads, Updates und in-App-Käufen angezeigt.   

 **Zahlungen und Finanzberichte** – erläutert das von ihren apps verdiente Einkommen und listet Zahlungen auf, die an Sie übermittelt wurden, und wie viel Sie Ihnen geschuldet sind.

Im folgenden finden Sie einen Beispiel Bericht "Sales and Trends":   

 [![Ein Beispiel für einen Umsatz-und Trendbericht](subscriptions-and-reporting-images/image42.png)](subscriptions-and-reporting-images/image42.png#lightbox)   

 Es gibt auch eine **ITC Connect Mobile** IOS-app. die iPhone-Screenshots für einige der verfügbaren Statistiken werden hier angezeigt:   

 [![iPhone-Screenshots für einige der verfügbaren Statistiken](subscriptions-and-reporting-images/image43.png)](subscriptions-and-reporting-images/image43.png#lightbox)
