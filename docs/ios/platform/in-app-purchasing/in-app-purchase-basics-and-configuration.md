---
title: "In App-Käufe Grundlagen und Konfiguration"
ms.topic: article
ms.prod: xamarin
ms.assetid: 11FB7F02-41B3-2B34-5A4F-69F12897FE10
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 302bb1225067ad401f97ee6bad88b4cd16c6dc95
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="in-app-purchase-basics-and-configuration"></a>In App-Käufe Grundlagen und Konfiguration

In app-Einkäufe implementieren, muss die Anwendung die StoreKit-API auf dem Gerät nutzen können. StoreKit wird die gesamte Kommunikation mit Apple iTunes-Servern zum Abrufen von Produktinformationen und Ausführen von Transaktionen verwaltet. Die provisioning-Profil muss konfiguriert werden, für den Erwerb von app- und Produktinformationen in iTunes Connect eingegeben werden muss.

 [![](in-app-purchase-basics-and-configuration-images/image1.png "StoreKit verwaltet gesamte Kommunikation mit der Apple-, wie in diesem Diagramm dargestellt.")](in-app-purchase-basics-and-configuration-images/image1.png#lightbox)

Mit dem App Store bereitgestellt Erwerb von in-app benötigen Sie die folgenden Setup und Konfiguration Folgendes:

-  **iTunes Connect** – konfigurieren Sie die Produkte verkaufen möchten, und die Einrichtung der Sandbox-Benutzerkonten zum Erwerb von testen. Sie müssen auch bereitgestellt haben Ihre Bankgeschäfte und Steuer-Informationen an Apple damit Betrag in Ihrem Auftrag erfasst überwiesen werden können.
-   **iOS-Bereitstellungsportal** – erstellen Sie ein Paket-ID und den App Store Zugriff für Ihre app aktivieren.
-  **Speichern von Kit** – Hinzufügen von Code für Ihre Anwendung auf das Anzeigen von Produkten, Produkte kaufen und Wiederherstellung von Transaktionen.
-  **Benutzerdefinierter Code** – zum Nachverfolgen der Einkäufe von Kunden, und geben Sie die Produkte oder Dienste, die sie gekauft haben. Sie können auch zu implementieren, müssen einen serverseitigen Prozess um Empfangsbestätigungen zu überprüfen, ob Ihre Produkte des Inhalts heruntergeladen, die von einem Server (z. B. Bücher und magazine Probleme) bestehen.


Es gibt zwei Store Kit "serverumgebungen":

-  **Produktion** – Transaktionen mit echten Money. Zugriff nur über Anwendungen, die übermittelt und von Apple genehmigt wurden. In app-Käufe Produkte müssen ebenfalls überprüft und genehmigt werden, damit sie in der produktionsumgebung verfügbar sind.
-  **Sandkasten** – hierbei der Tests erfolgt. Produkte sind hier direkt nach dem erstellen (der Genehmigungsprozess gilt nur für die produktionsumgebung) verfügbar. Transaktionen in den Sandkasten benötigen Testbenutzer (keine echten Apple-IDs) zum Durchführen von Transaktionen.

## <a name="in-app-purchase-rules"></a>Regeln in App-Käufe

Nicht andere Formen der Zahlung für digitale Produkte oder Dienste in Ihrer app zu akzeptieren noch erwähnen sie oder Ihre Benutzer Ihnen Verweiswerte innerhalb einer app verweisen. Dies bedeutet, dass Sie Kreditkarten oder PayPal akzeptieren können, wenn in der app zu erwerben der am besten geeigneten Zahlungsverkehrsmechanismus ist. Es ist ein besonderer Fall für den Erwerb von digitalen Produkte außerhalb der app, aber verwenden Sie für die app, wie z. B. Kauf von Büchern auf einer Website, die mit einem bestimmten "Login" verknüpft sind, und Verwenden von "Login" in der app den Zugriff des Benutzers können erworbenen Bücher.
Anwendungen, die auf diese Weise ausgeführt werden dürfen keine erwähnen oder Verknüpfen mit externen Kaufverhalten-Feature-Entwickler müssen diese Funktion für ihre Benutzer auf andere Weise (z. B. über e-Mail-Marketing oder einige andere direkten Kanal) kommunizieren.

Jedoch, da Sie nicht kauft in-app für physischen waren Fall werden dürfen, verwenden Sie einen alternativen Payment-Mechanismus (z. b. Kreditkarte, PayPal) von innerhalb der app.

Apple muss jedes Produkt genehmigen, bevor Sie darin auf-Verkauf – der Name, Beschreibung und ein Screenshot der "Product" ist erforderlich zur Überprüfung. Produkt überprüfen Sie, wie oft werden genauso wie bei der Anwendung überprüft.

Sie können jedem Preis nicht für ein bestimmtes Produkt auswählen – Auswahl kann nur "Preis Tier", die einen bestimmten Wert in jeder Land/Währung enthält, die von Apple unterstützt. Sie können keine anderen Ebene in verschiedenen Märkten verfügen.

## <a name="configuration"></a>Konfiguration

Vor dem Erwerb in app-Code schreiben müssen Sie portierungsziel Einrichtung in iTunes Connect ( [itunesconnect.apple.com](http://itunesconnect.apple.com)) und den iOS-Bereitstellungsportal ( [developer.apple.com/iOS](http://developer.apple.com/iOS)).

Diese drei Schritte sollten abgeschlossen sein, bevor Sie Code schreiben zu müssen:

-  **Apple-Entwicklerkonto** – Ihre Bankgeschäfte und auf einen Rechnungsposten Informationen an Apple senden.
-  **iOS-Bereitstellungsportal** – stellen Sie sicher, Ihre app ist eine gültige App-ID (keinen Platzhalter mit einem Sternchen * darin) und In der App Erwerb aktiviert hat.
-  **iTunes Connect Anwendungsverwaltung** – Produkte für Ihre Anwendung hinzufügen.


### <a name="apple-developer-account"></a>Apple-Entwicklerkonto

Erstellen und Verteilen von kostenlose apps erfordert nur geringfügige Konfigurationsschritte in [iTunes Connect](https://itunesconnect.apple.com)allerdings kostenpflichtigen verkaufen möchten, apps oder in app-Einkäufe erfordert, dass Sie Apple Bankgeschäfte und auf einen Rechnungsposten Informationen bereitzustellen. Klicken Sie auf **Vereinbarungen, Steuer und Bankgeschäfte** über das Hauptmenü hier dargestellt:

 [![](in-app-purchase-basics-and-configuration-images/image2.png "Klicken Sie auf Vereinbarungen, Steuer und Bankgeschäfte über das Hauptmenü")](in-app-purchase-basics-and-configuration-images/image2.png#lightbox)

Sollte Ihr Entwicklerkonto haben eine **bezahlt iOS-Anwendungen** Vertrag aktiviert ist, wie in diesem Screenshot gezeigt:

 [![](in-app-purchase-basics-and-configuration-images/image3.png "Ihrem Entwicklerkonto sollte ein iOS verfügen, die Anwendungen bezahlt faktisch Vertrag")](in-app-purchase-basics-and-configuration-images/image3.png#lightbox)

Sie werden keine StoreKit-Funktionalität zu testen, bis Sie haben eine **bezahlt iOS-Anwendungen** -Vertrag: StoreKit Aufrufe in Ihrem Code schlägt fehl, bis Apple verarbeitet hat Ihre **Verträge, Steuer und Bankgeschäfte** Informationen.

### <a name="ios-provisioning-portal"></a>iOS-Bereitstellungsportal

Neue Anwendungen richten Sie Sie in der **App-IDs** Teil der **iOS-Bereitstellungsportal**. Um eine neue App-ID zu erstellen, wechseln Sie zu der [Member Center des iOS-Bereitstellungsportal](https://developer.apple.com/membercenter/index.action), navigieren Sie zu **Zertifikate, Bezeichner und Profile** Abschnitt des Portals, und klicken Sie auf **Bezeichner** unter *iOS-Apps*. Klicken Sie auf das "+" oben rechts, um eine neue App-ID zu generieren


Das Formular zum Erstellen neuer **App-IDs**

 sieht wie folgt aus:

 [![](in-app-purchase-basics-and-configuration-images/image4.png "Das Formular zum Erstellen von neuen App-IDs")](in-app-purchase-basics-and-configuration-images/image4.png#lightbox)

Geben Sie einen passenden Namen für die *Beschreibung*, sodass Sie diese App-ID in einer Liste problemlos erkennen können. Für die *App ID-Präfix*, wählen Sie die Team-ID.

#### <a name="bundle-identifierapp-id-suffix-format"></a>Paket-ID-App-ID-Suffix-Format

Können Sie eine beliebige Zeichenfolge, die Ihnen, für gefällt die **Paket-ID** (solange sie in Ihrem Konto eindeutig ist), jedoch Apple empfiehlt, Sie führen Sie den Reverse-DNS-Format, anstatt jede beliebige Zeichenfolge verwenden. Die beispielanwendung, die in diesem Artikel mit verwendet com.xamarin.storekit.testing für die Paket-ID, es würden jedoch gleichermaßen gültig, bis einen Bezeichner, wie z. B. My_store_example verwenden (obwohl Apple Es empfiehlt sich, keine).

> [!IMPORTANT]
> **Hinweis**: Apple ermöglicht auch das Sternchen Platzhalter am Ende hinzugefügt werden eine **Paket-ID** , damit eine einzelne App-ID jedoch für mehrere Anwendungen verwendet werden können _Platzhalter App-IDs nicht verwendet werden In AppPurchase_. Ein Beispiel, dass Platzhalter Paket-ID com.xamarin.* möglicherweise

#### <a name="enabling-app-services"></a>Aktivieren des App-Dienste

Beachten Sie, dass **In App-Käufe** automatisch in der Liste der Dienste aktiviert werden:

 [![](in-app-purchase-basics-and-configuration-images/image5.png "In App-Käufe wird automatisch in der Liste der Dienste aktiviert werden")](in-app-purchase-basics-and-configuration-images/image5.png#lightbox)

#### <a name="provisioning-profiles"></a>Bereitstellungsprofile

Erstellen Sie die App-ID, die Sie für den Erwerb von In-App eingerichtet haben, haben normalerweise der Fall wäre dann Entwicklung und Produktion Provisioning Profile. Finden Sie in der [iOS Gerätebereitstellung](~/ios/get-started/installation/device-provisioning/index.md) und [veröffentlichen auf dem App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) Leitfäden an, um weitere Informationen.

## <a name="itunes-connect"></a>iTunes Connect

Klicken Sie auf **meine Apps** in iTunes Connect zum Erstellen oder bearbeiten den Eintrag einer iOS-Anwendung. Die Übersicht über die Anwendungsseite wird im folgenden dargestellt:

 [![](in-app-purchase-basics-and-configuration-images/image6.png "Die Anwendung-Seite "Übersicht"")](in-app-purchase-basics-and-configuration-images/image6.png#lightbox)

Klicken Sie auf **In App-Einkäufe** erstellen oder bearbeiten Ihre Produkte für den Verkauf. Diese bildschirmabbildung zeigt die Beispiel-app mit verschiedenen Produkten, die bereits hinzugefügt:

 [![](in-app-purchase-basics-and-configuration-images/image7.png "Die Beispiel-app mit mehreren Produkten, die bereits hinzugefügt.")](in-app-purchase-basics-and-configuration-images/image7.png#lightbox)

Der Prozess zum Hinzufügen neuer Produkte umfasst zwei Schritte:

1.   Wählen Sie den Product: [ ![ ] (in-app-purchase-basics-and-configuration-images/image8.png "wählen Sie die Product-Typs")](in-app-purchase-basics-and-configuration-images/image8.png#lightbox) 
2.   Geben Sie das Produkt Attribute, einschließlich der Produkt-Id, Tarif und lokalisierten Beschreibungen: [ ![ ] (in-app-purchase-basics-and-configuration-images/image9.png "Eingabe der Products-Attribute")](in-app-purchase-basics-and-configuration-images/image9.png#lightbox)

Die Felder für jedes Produkt in app-Käufe erforderlich sind, sind unten beschrieben:


### <a name="reference-name"></a>Verweisname

Der Verweisname ist für Ihre Benutzer nicht angezeigt. Es dient zur internen Verwendung und erscheint nur in iTunes Connect.

### <a name="product-id-format"></a>Produkt-ID-Format

Eine Produkt-ID darf nur alphanumerische (A-Z, a-Z, 0-9), Unterstrich (_) und Punkt (.) Zeichen. Obwohl Sie eine beliebige Zeichenfolge für Ihre Bezeichner verwenden können, empfiehlt es sich bei Apple um Reverse-DNS-Format. Die beispielanwendung verwendet z. B. diese Paket-ID:

 `com.xamarin.storekit.testing`

Daher würde die Aufrufkonvention an in-app-Käufe Produkte zu ermitteln, wie folgt lauten:

```csharp
com.xamarin.storekit.testing.consume5credits
com.xamarin.storekit.testing.consume10credits
com.xamarin.storekit.testing.sepia
com.xamarin.storekit.testing.greyscale
```

Diese Namenskonvention wird nicht erzwungen, einfach eine Empfehlung helfen Ihnen beim Verwalten Ihrer Produkte. Darüber hinaus trotz befolgen die gleichen Reverse-DNS-Konvention, die Produkt-IDs sind *nicht verknüpft* nicht mit dem Paket-ID und sind erforderlich, um mit derselben Zeichenfolge beginnen. Es wäre weiterhin gültige Bezeichner wie Photo_product_greyscale verwenden (obwohl Apple Es empfiehlt sich, nicht).

Produkt-ID ist nicht für Ihre Benutzer angezeigt, aber es wird verwendet, um das Produkt in Ihrem Anwendungscode zu verweisen.

### <a name="product-type"></a>Produkttyp

Es gibt fünf Arten von in-app-Käufe Produkt, die Sie bieten können:

1.  **Nutzbar** – Dinge, die "verwendete einrichten", z. B. Währung Spiels, die der Spieler ausgeben kann. Wenn der Benutzer ein Sichern/Wiederherstellen besitzt, oder andernfalls Geräts aktualisiert wurde, ist eine Transaktion kann nicht als auch wiederhergestellt (wodurch effektiv der Spieler den gleichen Vorteil erneut erhalten würde). Anwendungscode muss unbedingt nutzbar Element bereitstellen, sobald die Transaktion abgeschlossen ist.
1.  **Nicht verwendbar** – Produkte, die der Benutzer "besitzt", ein Mal gekauft werden, wie eine digitale Magazin Problem oder ein Spiel.
1.  **Automatische erneuerbar Abonnements** – so wie eine Zeitschrift realen-Abonnement am Ende des Abonnementzeitraums Apple automatisch erneut den Kunden die Kosten und erweitert den Begriff Abonnement endlos oder bis der Kunde explizit abgebrochen wird. Dies ist die bevorzugte Zahlungsmethode für Newsstand-apps (in der Tat apps müssen diese Zahlungsmethode für Newsstand Verteilung genehmigt werden unterstützt).
1.  **Abonnement frei** – kann nur in Newsstand-fähige apps zur Verfügung gestellt werden, und lässt den Kunden Abonnements für den Zugriff auf alle ihre Geräte. Kostenlose Abonnements laufen nie ab.
1.  **Abonnement nicht erneuern** – sollte verwendet werden, um zeitlich begrenzten Zugriff auf statische Inhalte, z. B. einen Monat Zugriff auf ein Foto-Archiv zu verkaufen.


 *Dieses Dokument behandelt derzeit nur die ersten beiden Produktfunktionen Typen gibt (umgewandelt und nicht verwendbar).*

 <a name="Price_Tiers" />

### <a name="price-tiers"></a>Preis Ebenen

App-Store können Sie weder eine beliebige Preis für Ihre Produkte auswählen – Apple bietet Festpreis Ebenen, von denen Sie wählen können. Preise sind in jeder Währung fest, und Apple behält die relative Preise (z. B. nach einer längeren Änderung der relativen Wechselkurs zwischen einer bestimmten Währung und dem US-Dollar) anpassen.

Apple bietet eine Matrix Preis, mit denen Sie die geeignete Stufe, die für die Währung/Price auszuwählen, sein sollen. Ein Auszug aus der Matrix Preis (August 2012) wird im folgenden dargestellt:

 [![](in-app-purchase-basics-and-configuration-images/image10.png "Ein Auszug aus der Preis Matrix August 2012")](in-app-purchase-basics-and-configuration-images/image10.png#lightbox)

Auf die Zeit der Schriftlegung (Juni 2013) befinden sich 87 Ebenen von USD zu USD 999,99 0,99. Die Preise Matrix zeigt den Preis, dass Ihre Kunden bezahlt und auch die Menge, die ausgehändigt von Apple – dies weniger die Ladung 30 % ist und auch alle lokalen steuern erforderlich sind (wie Sie sehen, in dem Beispiel, dass die USA und Kanada Verkäufer 70 ° c für eine 99 c-p empfangen sammeln Roduct, dagegen australischen Verkäufer nur 63 c aufgrund erhalten "waren &amp; Services Tax" auf der Verkaufspreis erhoben).

Preise Ihres Produkts kann jederzeit, auch geplante preisänderungen, die an einem zukünftigen Datum wirksam aktualisiert werden. Diese bildschirmabbildung zeigt, wie mit einem Stichtag Zukunft Preisänderung hinzugefügt wird: der Preis geändert wird, vorübergehend aus der Ebene 1 auf Ebene 3 für den Monat September nur:

 [![](in-app-purchase-basics-and-configuration-images/image11.png "Mit einem Stichtag Zukunft Preisänderung, in denen der Preis vorübergehend aus der Ebene 1, Ebene 3 für den Monat September nur geändert wird")](in-app-purchase-basics-and-configuration-images/image11.png#lightbox)

### <a name="free-products-not-supported"></a>Kostenlose Produkte nicht unterstützt.

Obwohl Apple eine spezielle Option von kostenloses Abonnement für Newsstand apps bereitgestellt wird wurde, ist es nicht möglich, einen 0 (null) (freien) Preis für alle anderen Typen von in-app-Käufe festzulegen. Während Sie bearbeiten können (d. h. niedrigere) Preise für verkaufswerbungen, Sie können keine in-app-Einkäufe über iTunes Connect "free" sind.

### <a name="localization"></a>Lokalisierung

In iTunes Connect können Sie unterschiedliche Namen und eine Beschreibung für eine beliebige Anzahl von unterstützten Sprachen Texteingaben. Jede Sprache hinzugefügt/bearbeitet über ein Popup sind möglich:

 [![](in-app-purchase-basics-and-configuration-images/image12.png "Jede Sprache möglich hinzugefügt/bearbeitet über ein popup")](in-app-purchase-basics-and-configuration-images/image12.png#lightbox)   
   
   
   
 Wenn Sie in Ihrer app Produktinformationen anzeigen, ist der lokalisierte Text für über StoreKit Anzeige verfügbar. Die Währungsanzeige muss auch lokalisiert werden, um die richtige Symbol und die Formatierung mit Dezimalstellen – zeigen diese Formatierung später in diesem Dokument behandelt wird.

### <a name="app-store-review"></a>Überprüfen Sie die App Store

-Apps – wie wird jedes Produkt von Apple überprüft, bevor Sie Berechtigung zum Wechseln auf Verkauf. Produkte wird möglicherweise zurückgewiesen, für ungeeignete Inhalte in den Namen oder die Beschreibung oder Apple beschließen, dass die falsche Produkttyp (z. b ausgewählte Abbild. Sie haben erstellt ein Buch oder Magazin vorhanden, aber verwendet die nutzbar Produkttyp). Produktprüfungen dauert genauso lang wie die einer app überprüfen.

Eine app gesendet wird, mit dem in der app Erwerb aktiviert (ob es eine neue app ist, oder die Funktionalität wurde mit einer vorhandenen Arbeitsaufgabe hinzugefügt) zum ersten Mal müssen Sie auch alle Produkte mit übermitteln auswählen. Das iTunes Connect-Portal wird dazu aufgefordert werden, wie in diesem Screenshot gezeigt:

 [![](in-app-purchase-basics-and-configuration-images/image13.png "Das iTunes Connect Portal fordert Sie zum Übermitteln von einigen anderen Produkten auch")](in-app-purchase-basics-and-configuration-images/image13.png#lightbox)   
   
   
   
 Die Anwendung und die in-app-Einkäufe werden zusammen, überprüft werden, damit sie alle auf einmal genehmigt (damit die app in den Speicher, ohne alle genehmigten Produkte aus, wechseln Sie nicht!).

Nachdem die erste Version mit in-app-Käufe Funktion genehmigt wurde, können weitere Produkte hinzufügen und sie zur Überprüfung zu einem beliebigen Zeitpunkt zu senden. Sie könne auch senden Sie eine neue Version zusammen mit bestimmten in-app-Käufe-Produkten mithilfe der **Versionsdetails** Seite wie die Aufforderung bereits vermuten lässt.

Finden Sie in der [App Store Review Guidelines](https://developer.apple.com/appstore/guidelines.html) für Weitere Informationen.

 [Teil 2 – Store-Verwaltungskit (Übersicht) "und" beim Abrufen von Produktinformationen](~/ios/platform/in-app-purchasing/store-kit-overview-and-retreiving-product-information.md)
