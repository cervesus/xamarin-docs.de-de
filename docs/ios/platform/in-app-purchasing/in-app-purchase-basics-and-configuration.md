---
title: Grundlagen von in-App-Käufe und-Konfiguration in Xamarin.iOS
description: Dieses Dokument beschreibt die in-app-Käufen in Xamarin.iOS, erläutern relevante Informationen zu Regeln, Konfiguration und iTunes Connect.
ms.prod: xamarin
ms.assetid: 11FB7F02-41B3-2B34-5A4F-69F12897FE10
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 4d79988fc2900f1fe58774657344f19fab90f3e4
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105089"
---
# <a name="in-app-purchase-basics-and-configuration-in-xamarinios"></a>Grundlagen von in-App-Käufe und-Konfiguration in Xamarin.iOS

Implementieren von in-app-Käufe, muss die Anwendung auf dem Gerät StoreKit API nutzen. StoreKit verwaltet die gesamte Kommunikation mit Apple iTunes-Servern zum Abrufen von Produktinformationen und Ausführen von Transaktionen. Das Bereitstellungsprofil muss konfiguriert werden, für die in-app-Käufe und Produktinformationen in iTunes Connect eingegeben werden muss.

 [![](in-app-purchase-basics-and-configuration-images/image1.png "StoreKit verwaltet die gesamte Kommunikation von Apple, wie in diesem Diagramm dargestellt.")](in-app-purchase-basics-and-configuration-images/image1.png#lightbox)

Verwenden die App-Store zum Bereitstellen von in-app-Käufe erfordert die folgenden Setup und Konfiguration:

-  **iTunes Connect** – konfigurieren Sie die Produkte verkaufen und Sandbox-Benutzerkonten einrichten, um Kauf zu testen. Sie müssen auch bereitgestellt haben Ihre Bank- und Steuer-Informationen an Apple, damit sie Geldmittel gesammelt, die in Ihrem Namen führen können.
-   **iOS-Bereitstellungsportal** – erstellen Sie eine Bündel-ID und App-Store-Zugriff für Ihre app wird aktiviert.
-  **Store Kit** – Hinzufügen von Code zu Ihrer app für Produkte anzeigen, Erwerb von Produkten und Wiederherstellen von Transaktionen.
-  **Benutzerdefinierter Code** um Einkäufe von Kunden nachverfolgen, und geben Sie die Produkte oder Dienste, die sie erworben haben. Sie können auch implementieren müssen einen serverseitigen Prozess um Empfangsbestätigungen zu überprüfen, wenn Ihre Produkte Inhalt heruntergeladen, die von einem Server (z. B. Bücher und magazine-Ausgaben) enthalten.


Es gibt zwei Store Kit "Server-Umgebungen":

-  **Produktion** – Transaktionen mit Menge Geld gespart. Zugriff nur über Anwendungen, die übermittelt und von Apple genehmigt wurden. In-app-Käufe Produkte müssen auch überprüft und genehmigt werden, bevor sie in der produktionsumgebung verfügbar sind.
-  **Sandbox** hier geschieht, Ihre Tests. Produkte sind hier direkt nach dem erstellen (der Genehmigungsprozess gilt nur für die produktionsumgebung) verfügbar. Transaktionen in der Sandbox erfordern Testbenutzer (nicht um echte Apple-IDs) zum Durchführen von Transaktionen.

## <a name="in-app-purchase-rules"></a>Regeln für in-App-Käufe

Sie können nicht akzeptieren von anderen Formen der Zahlung für digitale Produkte oder Dienste in Ihre app noch erwähnen sie oder finden Sie unter Ihren Benutzern für sie aus einer app. Dies bedeutet, dass Sie Kreditkarteninformationen oder PayPal akzeptieren können, wenn in-app-Käufe der optimale Zahlungsverkehrsmechanismus ist. Es ist ein Sonderfall für den Kauf von digitaler Produkten außerhalb der app, aber verwenden Sie für die app, wie der Kauf von Büchern auf einer Website, die mit einem bestimmten "Login" verknüpft sind, und verwenden, "Login" in der app den Benutzer den Zugriff ermöglicht die erworbenen Büchern.
Anwendungen, die auf diese Weise ausgeführt werden dürfen nicht ganz zu schweigen oder Verknüpfen mit der Funktion für den Erwerb externer – Entwickler müssen diese Funktion für ihre Benutzer auf andere Weise (z. B. über e-Mail-Marketing oder einige andere direkten Kanal) kommunizieren.

Aber da Sie nicht erwirbt in-app-bezahlen physischer Güter Fall, die Sie berechtigt sind, verwenden einen anderen Zahlungsmechanismus (z. b. Kreditkarte, PayPal) von innerhalb der app.

Apple muss jedes Produkt genehmigen, bevor er auf-Verkauf – der Name, Beschreibung geht und ein Screenshot der 'Product' erforderlich, zur Überprüfung ist. Produkt überprüfen Sie, wie oft werden genauso wie bei den benutzerbewertungen.

Sie können keine Preis für Ihr Produkt auswählen – Sie können nur einen "Tarif", die einen bestimmten Wert in jedes Land oder die Währung, die von Apple unterstützt auswählen. Sie können keinen anderen Tarif in verschiedenen Märkten verfügen.

## <a name="configuration"></a>Konfiguration

Vor dem Erwerb in-app-Code schreiben müssen Sie Setup Arbeit in iTunes Connect ( [itunesconnect.apple.com](http://itunesconnect.apple.com)) und den iOS-Bereitstellungsportal ( [developer.apple.com/iOS](http://developer.apple.com/iOS)).

Diese drei Schritte sollten ausgeführt werden, bevor Sie Code schreiben:

-  **Apple-Entwicklerkonto** – übermitteln Sie Ihre Informationen auszahlungstechnische und steuerliche Banking an Apple.
-  **iOS-Bereitstellungsportal** – stellen Sie sicher, Ihre app verfügt über eine gültige App-ID (keine Platzhalter mit einem Sternchen * darin) und In-App-Käufe aktiviert hat.
-  **iTunes Connect-Anwendungsverwaltung** – Hinzufügen von Produkten zu Ihrer Anwendung.


### <a name="apple-developer-account"></a>Apple-Entwicklerkonto

Erstellen und verteilen kostenlose apps erfordert nur geringfügige Konfigurationsschritte im [iTunes Connect](https://itunesconnect.apple.com)jedoch Verkauf der kostenpflichtigen apps oder in-app-Käufe erfordert, dass Sie Apple auszahlungstechnische und steuerliche Banking-Informationen bereitzustellen. Klicken Sie auf **Vereinbarungen, steuern und Bankverbindungen** über das Hauptmenü hier gezeigt:

 [![](in-app-purchase-basics-and-configuration-images/image2.png "Klicken Sie auf Vereinbarungen, steuern und Bankverbindungen über das Hauptmenü")](in-app-purchase-basics-and-configuration-images/image2.png#lightbox)

Ihr Entwicklerkonto müssen eine **kostenpflichtige iOS-Anwendungen** Vertrag aktiviert ist, wie im folgenden Screenshot gezeigt:

 [![](in-app-purchase-basics-and-configuration-images/image3.png "Ihr Entwicklerkonto sollte ein iOS haben, die kostenpflichtigen Anwendungen faktisch Vertrag")](in-app-purchase-basics-and-configuration-images/image3.png#lightbox)

Sie werden keine StoreKit-Funktionalität zu testen, bevor Sie haben eine **kostenpflichtige iOS-Anwendungen** Vertrag: StoreKit Aufrufe in Ihrem Code schlägt fehl, bis Apple verarbeitet hat Ihre **Verträge, steuern und Bankverbindungen** Informationen.

### <a name="ios-provisioning-portal"></a>iOS-Bereitstellungsportal

In neue Anwendungen eingerichtet sind die **App-IDs** Teil der **iOS-Bereitstellungsportal**. Um eine neue App-ID zu erstellen, wechseln Sie zu der [Member Center von der iOS-Bereitstellungsportal](https://developer.apple.com/membercenter/index.action), navigieren Sie zu **Zertifikate, Bezeichner und Profile** Abschnitt des Portals aus, und klicken Sie auf **Bezeichner** unter *iOS-Apps*. Klicken Sie dann oben auf "+" rechts um eine neue App-ID zu generieren.


Das Formular zum Erstellen neuer **App-IDs**

 sieht folgendermaßen aus:

 [![](in-app-purchase-basics-and-configuration-images/image4.png "Das Formular zum Erstellen neuer App-IDs")](in-app-purchase-basics-and-configuration-images/image4.png#lightbox)

Geben Sie einen passenden Namen für die *Beschreibung*, sodass Sie diese App-ID in einer Liste problemlos erkennen können. Für die *App-ID-Präfix*, wählen Sie die Team-ID

#### <a name="bundle-identifierapp-id-suffix-format"></a>Bundle-ID/App-ID-Suffix-Format

Können Sie eine beliebige Zeichenfolge, die Sie wie für Ihre **Bündel-ID** (solange sie in Ihrem Konto eindeutig ist), jedoch Apple, empfiehlt Sie führen Sie die Reverse-DNS-Format, anstatt eine beliebige Zeichenfolge verwenden. Die beispielanwendung, die diesen Artikel begleitet verwendet com.xamarin.storekit.testing für die Paket-ID, wäre es jedoch gleichermaßen gültig ist, einen Bezeichner wie My_store_example verwenden (obwohl Apple es wird empfohlen, nicht).

> [!IMPORTANT]
> Apple kann auch Platzhalter Sternchen am Ende hinzugefügt werden eine **Bündel-ID** , damit eine einzelne App-ID jedoch für mehrere Anwendungen verwendet werden können _Platzhalter App-IDs kann nicht verwendet werden, für die AppPurchase_. Ein Beispiel, dass die Platzhalter Bündel-ID com.xamarin.* möglicherweise

#### <a name="enabling-app-services"></a>Aktivieren von App-Dienste

Beachten Sie, dass **In App-Käufe** wird automatisch in der Liste der Dienste aktiviert:

 [![](in-app-purchase-basics-and-configuration-images/image5.png "In-App-Käufe wird werden in der Liste der Dienste automatisch aktiviert.")](in-app-purchase-basics-and-configuration-images/image5.png#lightbox)

#### <a name="provisioning-profiles"></a>Bereitstellungsprofile

Erstellen Sie Entwicklungs- und Produktionsumgebungen Bereitstellungsprofile wie normalerweise würde, Auswahl der App-ID, die Sie für den Kauf von In-App eingerichtet haben. Finden Sie in der [iOS Device Provisioning](~/ios/get-started/installation/device-provisioning/index.md) und [in den App Store veröffentlichen](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) Anleitungen für die Weitere Informationen.

## <a name="itunes-connect"></a>iTunes Connect

Klicken Sie auf **meine Apps** in iTunes Connect erstellen oder bearbeiten den Eintrag einer iOS-Anwendung. Übersicht über die Anwendungsseite wird hier gezeigt:

 [![](in-app-purchase-basics-and-configuration-images/image6.png "Die Anwendung-Seite \"Übersicht\"")](in-app-purchase-basics-and-configuration-images/image6.png#lightbox)

Klicken Sie auf **In-App-Käufe** erstellen oder bearbeiten Ihre Produkte zum Verkauf. Dieser Screenshot zeigt die Beispiel-app mit verschiedenen Produkten, die bereits hinzugefügt:

 [![](in-app-purchase-basics-and-configuration-images/image7.png "Die Beispiel-app mit verschiedenen Produkten, die bereits hinzugefügt.")](in-app-purchase-basics-and-configuration-images/image7.png#lightbox)

Der Prozess zum Hinzufügen neuer Produkte umfasst zwei Schritte:

1.   Wählen Sie den Product: [![](in-app-purchase-basics-and-configuration-images/image8.png "wählen Sie die Product-Typs")](in-app-purchase-basics-and-configuration-images/image8.png#lightbox) 
2.   Geben Sie das Produkt Attribute, einschließlich der Produkt-Id, Tarif und lokalisierten Beschreibungen: [![](in-app-purchase-basics-and-configuration-images/image9.png "Eingabe der Products-Attribute")](in-app-purchase-basics-and-configuration-images/image9.png#lightbox)

Die Felder für jedes Produkt in app-Käufe erforderlich werden nachfolgend beschrieben:


### <a name="reference-name"></a>Verweisname

Der Verweisname für Ihre Benutzer nicht angezeigt wird; Dabei handelt es sich für die interne Verwendung wird nur in iTunes Connect angezeigt.

### <a name="product-id-format"></a>Produkt-ID-Format

Eine Produkt-ID darf nur alphanumerische (A-Z, a – Z, 0-9), Unterstrich (_), und der Punkt (.). Obwohl Sie eine beliebige Zeichenfolge für die IDs verwenden können, empfiehlt es sich bei Apple um das Reverse-DNS-Format. Die beispielanwendung verwendet beispielsweise diese Bündel-ID:

 `com.xamarin.storekit.testing`

Daher wäre die Konvention zum Identifizieren von in-app-Käufe Produkte wie folgt:

```csharp
com.xamarin.storekit.testing.consume5credits
com.xamarin.storekit.testing.consume10credits
com.xamarin.storekit.testing.sepia
com.xamarin.storekit.testing.greyscale
```

Diese Namenskonvention die Zuordnung wird nicht erzwungen, einfach eine Empfehlung helfen Ihnen beim Verwalten von Ihren Produkten. Darüber hinaus trotz befolgen die gleichen Reverse-DNS-Konvention, die Produkt-IDs sind *nicht verknüpft* nicht auf die Bündel-ID und sind erforderlich, um mit der gleichen Zeichenfolge beginnen. Es wäre immer noch gültige Bezeichner wie z. B. Photo_product_greyscale verwenden (obwohl Apple es wird empfohlen, nicht).

Produkt-ID für Ihre Benutzer nicht angezeigt, aber es wird verwendet, um das Produkt in Ihrem Anwendungscode zu verweisen.

### <a name="product-type"></a>Produkttyp

Es gibt fünf Arten von in-app-Käufe-Produkt, die Sie anbieten können:

1.  **Nutzbar** – Dinge, die "verwendeten einrichten", z. B. in-Game-Währung, die der Spieler investieren können. Wenn der Benutzer eine Sicherung/Wiederherstellung wird, oder andernfalls, ihr Gerät aktualisiert wurde, ist eine nutzbar Transaktion nicht ebenfalls wiederhergestellt abrufen (der effektiv dem Player gleichen bieten den Vorteil wieder ganz von vorn würde). Anwendungscode muss unbedingt nutzbar Element bereit, sobald die Transaktion abgeschlossen ist.
1.  **Nicht verwendbar** – Produkte, die der Benutzer "besitzt", die einmal erworben haben, z. B. ein digital magazine Problem oder eine Spiele-Ebene.
1.  **Automatische erneuerbar Abonnements** – so wie einem echten magazine-Abonnement am Ende des Abonnementzeitraums Apple automatisch der Kunde erneut berechnet und erweitert die Abonnementlaufzeit endlos oder bis der Kunde explizit abgebrochen wird. Dies ist die bevorzugte Zahlungsmethode für Zeitungskiosk-apps (in der Tat apps müssen diese Zahlungsmethode für die Verteilung der aus dem Zeitungskiosk genehmigt werden unterstützt).
1.  **Kostenloses Abonnement** – können nur aus dem Zeitungskiosk-fähige apps angeboten werden, und ermöglicht es den Kunden Zugriff auf Inhalte, Abonnement auf allen ihren Geräten. Kostenlose Abonnements laufen nie ab.
1.  **Abonnement nicht erneuern** – sollte verwendet werden, um zeitlich begrenzten Zugriff auf statische Inhalte wie einen Monat Zugriff auf ein Foto-Archiv zu verkaufen.


 *Dieses Dokument behandelt derzeit nur die ersten beiden Produktfunktionen Typen gibt (verwendet werden können und nicht verwendbar).*

 <a name="Price_Tiers" />

### <a name="price-tiers"></a>Preis Ebenen

Die App-Store ist nicht möglich, die wählen Sie eines beliebigen Preis für Ihre Produkte – Apple bietet Festpreis-Tarifen, denen Sie auswählen können. Die Preise sind in jeder Währung fest, und Apple behält sich das Recht, passen Sie die relative Preise (z. B. nach eine dauerhafte Änderung den relativen Fremd Wechselkurs zwischen einer bestimmten Währung und dem US-Dollar).

Apple bietet eine Matrix Preis, mit denen Sie die geeignete Stufe, die für die Währung/Preis für die gewünschten auswählen. Ein Auszug der Preis Matrix (August 2012) ist hier dargestellt:

 [![](in-app-purchase-basics-and-configuration-images/image10.png "Ein Auszug der Preis Matrix August 2012")](in-app-purchase-basics-and-configuration-images/image10.png#lightbox)

Zum Zeitpunkt der Schriftlegung (Juni 2013) stehen 87 Ebenen von USD 0,99 auf USD bis 999,99. Die Preise Matrix zeigt die Kosten, dass Ihre Kunden auch die Menge, die Sie von Apple – empfängt diese weniger ihre Gebühren 30 % ist und auch lokalen Abgaben erforderlich sind (wie Sie sehen, in dem Beispiel, dass die USA und Kanada Verkäufer 70 ° c für 99 c p erhalten sammeln Product, während der australischen Verkäufer 63 c aufgrund von erhalten "waren &amp; Services Tax" auf der Verkaufspreis erhobene).

Ihres Produkts – Preise kann sich jederzeit, einschließlich der geplanten preisänderungen, die auf ein zukünftiges Datum wirksam aktualisiert werden. Dieser Screenshot zeigt, wie die Zukunft mit einem Stichtag Preisänderung hinzugefügt wird – der Preis geändert wird, vorübergehend von Ebene 1 auf Ebene 3 für den Monat September nur aus:

 [![](in-app-purchase-basics-and-configuration-images/image11.png "Eine Zukunft mit einem Stichtag Preisänderung, in dem der Preis vorübergehend von Ebene 1 auf Ebene 3 für den Monat September nur geändert wird")](in-app-purchase-basics-and-configuration-images/image11.png#lightbox)

### <a name="free-products-not-supported"></a>Kostenlose Produkte, die nicht unterstützt

Obwohl Apple eine spezielle Abonnementoption "Free" für Zeitungskiosk-apps bereitgestellt wurde, ist es nicht möglich, einen 0 (null) (kostenlosen) Preis für alle anderen Typen von in-app-Käufe festzulegen. Während Sie bearbeiten können (d. h. niedrigere) Preise für verkaufswerbungen, Sie können nicht als in-app-Käufe "free" über iTunes Connect.

### <a name="localization"></a>Lokalisierung

In iTunes Connect können Sie verschiedene Namen und Beschreibung den Text für eine beliebige Anzahl von unterstützten Sprachen eingeben. Jede Sprache hinzugefügt/Bearbeitung in über ein Popup sind möglich:

 [![](in-app-purchase-basics-and-configuration-images/image12.png "Jede Sprache möglich hinzugefügt/Bearbeitung in über ein popup")](in-app-purchase-basics-and-configuration-images/image12.png#lightbox)   
   
   
   
 Wenn Sie über Produktinformationen in Ihrer app anzeigen, steht der lokalisierte Text für die Sie über StoreKit anzeigen. Die Währungsanzeige muss ebenfalls lokalisiert werden, um die richtige Symbol und die Dezimalstellen formatieren – zeigen diese Formatierung später in diesem Dokument behandelt wird.

### <a name="app-store-review"></a>Überprüfen Sie die App Store

Identisch mit apps – wird jedes Produkt von Apple überprüft bevor er auf Verkauf zu wechseln. Produkte möglicherweise für anstößige Inhalte in den Namen oder Beschreibung abgelehnt oder Apple beschließen, dass Sie den falsche Produkt-Typ (z.B.) ausgewählt haben Sie haben erstellt Bücher oder magazine Problem, jedoch verständlichen Produkttyp verwendet). Produktbesprechungen dauert so lange wie eine app-Überprüfung.

Das erste Mal, das eine app gesendet wird, mit dem in-app-Erwerb aktiviert (ob es eine neue app ist, oder die Funktion mit einem vorhandenen Arbeitselement wurde) müssen Sie auch einige Produkte mit übermitteln auswählen. ITunes Connect Portal fordert Sie dazu, wie im folgenden Screenshot gezeigt:

 [![](in-app-purchase-basics-and-configuration-images/image13.png "ITunes Connect Portal fordert Sie zum Übermitteln von sowie einige Produkte")](in-app-purchase-basics-and-configuration-images/image13.png#lightbox)   
   
   
   
 Die Anwendung und die in-app-Käufe werden zusammen überprüft werden, damit sie alle auf einmal genehmigt (also die app in den Speicher, ohne alle genehmigten Produkte nicht!).

Nachdem die erste Version mit in-app-Käufe Funktion genehmigt wurde, können Sie weitere Produkte hinzufügen und diese zur Überprüfung zu einem beliebigen Zeitpunkt zu senden. Sie können auch auswählen, senden eine neue Version zusammen mit bestimmten in-app-Käufe-Produkten, mit der **Versionsdetails** Seite wie die Eingabeaufforderung bereits vermuten lässt.

Finden Sie in der [App Store Review Guidelines](https://developer.apple.com/appstore/guidelines.html) für Weitere Informationen.

 [Teil 2 – Store-Verwaltungskit (Übersicht) und Abrufen von Produktinformationen](~/ios/platform/in-app-purchasing/store-kit-overview-and-retreiving-product-information.md)
