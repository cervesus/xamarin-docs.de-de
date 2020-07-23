---
title: Grundlagen und Konfiguration von in-App-Käufen in xamarin. IOS
description: In diesem Dokument werden in-App-Käufe in xamarin. IOS beschrieben und relevante Informationen zu Regeln, Konfiguration und iTunes Connect erörtert.
ms.prod: xamarin
ms.assetid: 11FB7F02-41B3-2B34-5A4F-69F12897FE10
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: fb63568adee9e89d08a0fc64168c865eeb271f10
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86928886"
---
# <a name="in-app-purchase-basics-and-configuration-in-xamarinios"></a>Grundlagen und Konfiguration von in-App-Käufen in xamarin. IOS

Zum Implementieren von in-App-Käufen muss die Anwendung die storekit-API auf dem Gerät verwenden. Storekit verwaltet die gesamte Kommunikation mit den iTunes-Servern von Apple, um Produktinformationen zu erhalten und Transaktionen auszuführen. Das Bereitstellungs Profil muss für den in-App-Einkauf konfiguriert werden, und Produktinformationen müssen in iTunes Connect eingegeben werden.

 [![Storekit verwaltet die gesamte Kommunikation mit Apple, wie in diesem Diagramm gezeigt.](in-app-purchase-basics-and-configuration-images/image1.png)](in-app-purchase-basics-and-configuration-images/image1.png#lightbox)

Zum Bereitstellen von in-App-Käufen mithilfe des App Store sind folgende Einrichtung und Konfiguration erforderlich:

- **iTunes Connect** – Konfigurieren der zu verkaufenden Produkte und Einrichten von Sandbox-Benutzerkonten zum Testen des Kaufs. Sie müssen auch Ihre Bank-und Steuerinformationen an Apple weitergegeben haben, damit Sie in Ihrem Namen erfasste Gelder übermitteln können.
- **IOS-Bereitstellungs Portal** – Erstellen eines Bündel Bezeichners und Aktivieren des App Store-Zugriffs für Ihre APP.
- **Store-Kit** – Hinzufügen von Code zu Ihrer APP, um Produkte anzuzeigen, Produkte zu kaufen und Transaktionen wiederherzustellen.
- **Benutzerdefinierter Code** – um Käufe von Kunden zu verfolgen und die Produkte oder Dienste bereitzustellen, die Sie gekauft haben. Möglicherweise müssen Sie auch einen serverseitigen Prozess implementieren, um Bestätigungen zu überprüfen, wenn Ihre Produkte aus Inhalten bestehen, die von einem Server heruntergeladen wurden (z. b. Bücher und Magazine

Es gibt zwei Store-Kit "Serverumgebungen":

- **Produktion** – Transaktionen mit echtem Geld. Der Zugriff ist nur über Anwendungen möglich, die von Apple übermittelt und genehmigt wurden. In-App-Käufe müssen auch überprüft und genehmigt werden, bevor Sie in der Produktionsumgebung verfügbar sind.
- **Sandbox** – wo die Tests durchgeführt werden. Produkte sind hier sofort nach der Erstellung verfügbar (der Genehmigungs Vorgang gilt nur für die Produktionsumgebung). Transaktionen in der Sandbox erfordern Test Benutzer (keine echten Apple-IDs) zum Ausführen von Transaktionen.

## <a name="in-app-purchase-rules"></a>In-App-Kauf Regeln

Sie können keine anderen Zahlungsformen für digitale Produkte oder Dienste in Ihrer APP akzeptieren, und Sie können Sie auch nicht erwähnen oder Ihre Benutzer aus einer APP heraus darauf verweisen. Dies bedeutet, dass Sie Kreditkarten oder PayPal nicht akzeptieren können, wenn der in-App-Einkauf der geeignetste Zahlungsmechanismus ist. Es gibt einen Sonderfall für den Erwerb digitaler Produkte außerhalb der APP, aber für die Verwendung in der APP, z. b. das kaufen von Büchern auf einer Website, die einer bestimmten "Anmeldung" zugeordnet ist, und die Verwendung dieser "Login" in der App ermöglicht dem Benutzer den Zugriff auf die erworbenen Bücher.
Anwendungen, die auf diese Weise agieren, dürfen die externe Purchasing-Funktion nicht erwähnen oder mit ihr verknüpft werden – Entwickler müssen diese Funktion ihren Benutzern auf andere Weise mitteilen (z. u. über e-Mail-Marketing oder einen anderen direkten Channel).

Da Sie jedoch keine in-App-Käufe für physische waren verwenden können, ist in diesem Fall die Verwendung eines alternativen Zahlungsmechanismus (z. b. Kreditkarte, PayPal) in der app.

Apple muss alle Produkte genehmigen, bevor es in den Verkauf wechselt – der Name, die Beschreibung und ein Screenshot des Produkts "Product" sind zur Überprüfung erforderlich. Die Produkt Prüfungszeiten sind identisch mit denen für Anwendungs Überprüfungen.

Sie können keinen Preis für Ihr Produkt auswählen – Sie können nur einen "Tarif" auswählen, der einen bestimmten Wert in jedem Land bzw. jeder Währung hat, den Apple unterstützt. Es ist nicht möglich, eine andere Preisstufe in verschiedenen Märkten zu haben.

## <a name="configuration"></a>Konfiguration

Vor dem Schreiben eines in-App-Kauf Codes müssen Sie einige Setup Aufgaben in iTunes Connect ( [iTunesConnect.Apple.com](https://itunesconnect.apple.com)) und im IOS-Bereitstellungs Portal ( [Developer.Apple.com/IOS](https://developer.apple.com/iOS)) durchführen.

Diese drei Schritte sollten ausgeführt werden, bevor Sie Code schreiben:

- **Apple-Entwicklerkonto** – übermitteln Sie Ihre Bank-und Steuerinformationen an Apple.
- **IOS-Bereitstellungs Portal** – stellen Sie sicher, dass Ihre APP über eine gültige APP-ID (kein Platzhalter mit einem Sternchen *) verfügt und dass der APP-Einkauf aktiviert ist.
- **iTunes Connect-Anwendungs Verwaltung** – fügen Sie Produkte zu Ihrer Anwendung hinzu.

### <a name="apple-developer-account"></a>Apple-Entwicklerkonto

Zum Erstellen und Verteilen von kostenlosen Apps ist in [iTunes Connect](https://itunesconnect.apple.com)sehr wenig Konfiguration erforderlich. Wenn Sie jedoch kostenpflichtige Apps oder in-App-Käufe verkaufen möchten, müssen Sie Apple Informationen zu Bank-und Steuerinformationen bereitstellen. Klicken Sie im hier gezeigten Hauptmenü auf **Verträge, Steuern und Banken** :

 [![Klicken Sie im Hauptmenü auf Verträge, Steuern und Banken.](in-app-purchase-basics-and-configuration-images/image2.png)](in-app-purchase-basics-and-configuration-images/image2.png#lightbox)

Für Ihr Entwicklerkonto sollte ein Vertrag mit IOS-Zahlungs **pflichtigen Anwendungen** wirksam sein, wie in diesem Screenshot gezeigt:

 [![Für Ihr Entwicklerkonto sollte ein kostenpflichtiger IOS-Anwendungs Vertrag vorhanden sein.](in-app-purchase-basics-and-configuration-images/image3.png)](in-app-purchase-basics-and-configuration-images/image3.png#lightbox)

Sie sind nicht in der Lage, storekit-Funktionen zu testen, bis Sie einen Vertrag mit IOS-Zahlungs **pflichtigen Anwendungen** haben – storekit-Aufrufe im Code schlagen fehl, bis Apple Ihre **Verträge, Steuern und Bank** Informationen verarbeitet hat.

### <a name="ios-provisioning-portal"></a>iOS-Bereitstellungsportal

Neue Anwendungen werden im **IOS-Bereitstellungs Portal**im Abschnitt **App-IDs** eingerichtet. Um eine neue APP-ID zu erstellen, wechseln Sie zum [Member Center im IOS-Bereitstellungs Portal](https://developer.apple.com/membercenter/index.action), navigieren Sie im Portal zum Abschnitt **Zertifikate, Bezeichner und Profile** , und klicken **Sie unter** IOS- *apps*auf Bezeichner. Klicken Sie dann oben rechts auf "+", um eine neue APP-ID zu generieren.

Das Formular zum Erstellen neuer **App-IDs** .

 sieht wie folgt aus:

 [![Das Formular zum Erstellen neuer App-IDs.](in-app-purchase-basics-and-configuration-images/image4.png)](in-app-purchase-basics-and-configuration-images/image4.png#lightbox)

Geben Sie einen geeigneten Namen für die *Beschreibung*ein, damit Sie diese APP-ID leicht in einer Liste identifizieren können. Wählen Sie für das *App-ID-Präfix*die Team-ID aus.

#### <a name="bundle-identifierapp-id-suffix-format"></a>Bundle Identifier/APP-ID-Suffix-Format

Sie können eine beliebige Zeichenfolge für die **Bündel** -ID verwenden (sofern Sie in Ihrem Konto eindeutig ist), aber Apple empfiehlt, dass Sie das Reverse-DNS-Format befolgen, anstatt eine beliebige Zeichenfolge zu verwenden. Die Beispielanwendung, die diesen Artikel begleitet, verwendet com. xamarin. storekit. testing für die Bündel-ID, es wäre aber gleichermaßen zulässig, einen Bezeichner wie my_store_example zu verwenden (obwohl dies von Apple nicht empfohlen wird).

> [!IMPORTANT]
> Außerdem ermöglicht Apple das Hinzufügen eines Platzhalter Sternchens zum Ende eines **Bündel Bezeichners** , damit eine einzelne APP-ID für mehrere Anwendungen verwendet werden kann, jedoch keine Platzhalter _-App-IDs für in-apppurchase verwendet werden_können. Ein Beispiel für eine Platzhalter-Bundle-ID ist com. xamarin. *

#### <a name="enabling-app-services"></a>Aktivieren von App Services

Beachten Sie, dass **in-App-Käufe** automatisch in der Liste der Dienste aktiviert werden:

 [![In-App-Käufe werden automatisch in der Liste "Dienste" aktiviert.](in-app-purchase-basics-and-configuration-images/image5.png)](in-app-purchase-basics-and-configuration-images/image5.png#lightbox)

#### <a name="provisioning-profiles"></a>Bereitstellungsprofile

Erstellen Sie Entwicklungs-und Produktions Bereitstellungs Profile wie gewohnt, und wählen Sie die APP-ID aus, die Sie für den in-App-Einkauf eingerichtet haben. Weitere Informationen finden Sie in den Handbüchern zur Bereitstellung und Veröffentlichung von [IOS-Geräten](~/ios/get-started/installation/device-provisioning/index.md) [im App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) .

## <a name="itunes-connect"></a>iTunes Connect

Klicken Sie in iTunes Connect auf **meine apps** , um einen IOS-Anwendungs Eintrag zu erstellen oder zu bearbeiten. Die Seite "Anwendungs Übersicht" wird hier angezeigt:

 [![Die Seite "Anwendungs Übersicht"](in-app-purchase-basics-and-configuration-images/image6.png)](in-app-purchase-basics-and-configuration-images/image6.png#lightbox)

Klicken Sie auf **in-App-Käufe** , um Ihre Produkte zum Verkauf zu erstellen oder zu bearbeiten. Dieser Screenshot zeigt die Beispiel-App mit mehreren bereits hinzugefügten Produkten:

 [![Die Beispiel-App mit mehreren bereits hinzugefügten Produkten](in-app-purchase-basics-and-configuration-images/image7.png)](in-app-purchase-basics-and-configuration-images/image7.png#lightbox)

Der Vorgang zum Hinzufügen neuer Produkte umfasst zwei Schritte:

1. Produkttyp auswählen: [ ![ Wählen Sie den](in-app-purchase-basics-and-configuration-images/image8.png)](in-app-purchase-basics-and-configuration-images/image8.png#lightbox) Produkttyp aus. 
2. Geben Sie die Attribute des Produkts ein, einschließlich Produkt-ID, Tarif und lokalisierten Beschreibungen: [ ![ eingeben der Products-Attribute](in-app-purchase-basics-and-configuration-images/image9.png)](in-app-purchase-basics-and-configuration-images/image9.png#lightbox) .

Die Felder, die für die einzelnen in-App-Produkte erforderlich sind, werden im folgenden beschrieben:

### <a name="reference-name"></a>Verweisname

Der Verweis Name wird den Benutzern nicht angezeigt. Sie ist für die interne Verwendung vorgesehen und wird nur in iTunes Connect angezeigt.

### <a name="product-id-format"></a>Produkt-ID-Format

Ein Produkt Bezeichner darf nur alphanumerische Zeichen (a-z, a-z, 0-9), Unterstriche (_) und Zeit (.) enthalten. Obwohl Sie eine beliebige Zeichenfolge für Ihre Bezeichner verwenden können, empfiehlt Apple das Reverse-DNS-Format. Beispielsweise verwendet die Beispielanwendung diese Bündel-ID:

 `com.xamarin.storekit.testing`

Daher lautet die Konvention zur Identifizierung von in-App-Kauf Produkten wie folgt:

```csharp
com.xamarin.storekit.testing.consume5credits
com.xamarin.storekit.testing.consume10credits
com.xamarin.storekit.testing.sepia
com.xamarin.storekit.testing.greyscale
```

Diese Benennungs Konvention wird nicht erzwungen, sondern eine Empfehlung zur Verwaltung Ihrer Produkte. Außerdem sind die Produkt Bezeichner trotz der gleichen Reverse-DNS-Konvention nicht mit der Bündel-ID *verknüpft* und müssen nicht mit derselben Zeichenfolge beginnen. Es ist weiterhin gültig, Bezeichner wie photo_product_greyscale zu verwenden (obwohl Apple dies nicht empfiehlt).

Die Produkt-ID wird Ihren Benutzern nicht angezeigt, aber Sie wird verwendet, um in Ihrem Anwendungscode auf das Produkt zu verweisen.

### <a name="product-type"></a>Produkttyp

Es gibt fünf Arten von in-App-Kauf Produkten, die Sie anbieten können:

1. **Consudbar** – Dinge, die verwendet werden, wie z. b. in-Game-Währungen, die der Spieler aufwenden kann. Wenn der Benutzer eine Sicherung/Wiederherstellung durchführt oder das Gerät auf andere Weise aktualisiert wird, wird eine nutzbare Transaktion ebenfalls nicht wieder hergestellt (was dem Player den gleichen Vorteil bietet). Der Anwendungscode muss das "nutzbare Element" bereitstellen, sobald die Transaktion abgeschlossen ist.
1. **Nicht nutzbare** –-Produkte, die der Benutzer als Besitzer besitzt, z. b. ein digitales Magazine-Problem oder eine Spielebene.
1. **Abonnements mit automatischer** Wiederveröffentlichung – genau wie ein echtes Magazin-Abonnement wird der Kunde am Ende des Abonnementzeitraums automatisch in Rechnung gestellt, und die Abonnement Laufzeit wird so lange verlängert, bis der Kunde Sie explizit abbricht. Dies ist die bevorzugte Zahlungsmethode für NewsStand-Apps (Apps müssen diese Zahlungsmethode unterstützen, um für die News-Stand Verteilung genehmigt zu werden).
1. **Kostenloses Abonnement** – kann nur in newsstandfähigen apps angeboten werden und ermöglicht dem Kunden den Zugriff auf Abonnement Inhalte auf allen Geräten. Kostenlose Abonnements laufen nie ab.
1. **Nicht erneuerndes Abonnement** – sollte verwendet werden, um zeitlich begrenzten Zugriff auf statische Inhalte, z. b. den Zugriff eines Monats auf ein Foto Archiv, zu verkaufen.

 *In diesem Dokument werden zurzeit nur die ersten beiden Produkttypen (verwendbar und nicht verwendbar) behandelt.*

 <a name="Price_Tiers"></a>

### <a name="price-tiers"></a>Preisstufen

Mit dem App Store können Sie keinen beliebigen Preis für Ihre Produkte auswählen – Apple bietet festgelegte Tarife, aus denen Sie auswählen können. Die Preise sind in jeder Währung korrigiert, und Apple behält sich das Recht vor, die relativen Preise anzupassen (z. b. nach einer dauerhaften Änderung des relativen fremd Wechselkurses zwischen einer bestimmten Währung und dem US-Dollar).

Apple stellt eine Preis Matrix bereit, mit der Sie den richtigen Tarif für die gewünschte Währung bzw. den gewünschten Preis auswählen können. Ein Auszug aus der Preis Matrix (August 2012) wird hier angezeigt:

 [![Ein Auszug aus der Preis Matrix August 2012](in-app-purchase-basics-and-configuration-images/image10.png)](in-app-purchase-basics-and-configuration-images/image10.png#lightbox)

Zum Zeitpunkt des Schreibens (Juni 2013) gibt es 87 Tarife von USD 0,99 bis USD 999,99. Die Preis Matrix zeigt den Preis, den Ihre Kunden bezahlen werden, sowie die Menge, die Sie von Apple erhalten werden – dies ist weniger als 30-prozentige Kosten und auch alle lokalen Steuern, die für die Erfassung erforderlich sind (Beachten Sie, dass in dem Beispiel US-und kanadische Verkäufer 70c für ein 99-c-Produkt erhalten, während australische Verkäufer nur 63c erhalten, weil &amp; der Verkaufspreis

Die Preise Ihres Produkts können jederzeit aktualisiert werden, einschließlich geplanter Preisänderungen, die für ein zukünftiges Datum wirksam werden. In diesem Screenshot wird gezeigt, wie eine in der Zukunft abzurufende Preisänderung hinzugefügt wird – der Preis wird vorübergehend für den Monat September von Ebene 1 in Ebene 3 geändert:

 [![Eine Zukunfts basierte Preisänderung, bei der der Preis für den Monat September vorübergehend von Ebene 1 in Ebene 3 geändert wird.](in-app-purchase-basics-and-configuration-images/image11.png)](in-app-purchase-basics-and-configuration-images/image11.png#lightbox)

### <a name="free-products-not-supported"></a>Kostenlose Produkte werden nicht unterstützt

Apple hat zwar eine spezielle kostenlose Abonnement Option für NewsStand-apps bereitgestellt, aber es ist nicht möglich, für andere in-App-Käufe einen Preis für NULL (Free) festzulegen. Wenngleich Sie die Preise für Sales Promotionangebote (d. a. niedriger) bearbeiten können, können Sie in-App-Käufe "Free" nicht über iTunes Connect durchführen.

### <a name="localization"></a>Lokalisierung

In iTunes Connect können Sie einen anderen Namen und Beschreibungstext für eine beliebige Anzahl unterstützter Sprachen eingeben. Jede Sprache kann über ein Popup in hinzugefügt/bearbeitet werden:

 [![Jede Sprache kann über ein Popup in hinzugefügt/bearbeitet werden.](in-app-purchase-basics-and-configuration-images/image12.png)](in-app-purchase-basics-and-configuration-images/image12.png#lightbox)   

Wenn Sie in Ihrer APP Produktinformationen anzeigen, steht Ihnen der lokalisierte Text zur Verfügung, der über storekit angezeigt werden kann. Die Währungs Anzeige muss ebenfalls lokalisiert werden, um die korrekte Symbol-und dezimal Formatierung anzuzeigen – diese Formatierung wird später im Dokument behandelt.

### <a name="app-store-review"></a>App Store-Review

Identisch mit apps – alle Produkte werden von Apple überprüft, bevor Sie in den Verkauf einsteigen können. Produkte werden möglicherweise für ungeeignete Inhalte im Namen oder in der Beschreibung abgelehnt, oder Apple entscheidet sich für den falschen Produkttyp (z. b. Sie haben ein Buch-oder Magazin-Problem erstellt, aber den verwendbaren Produkttyp verwendet.) Produkt Reviews können so lange dauern wie eine APP-Überprüfung.

Beim ersten übermitteln einer APP mit aktiviertem in-App-Einkauf (unabhängig davon, ob es sich um eine neue App handelt oder die Funktionalität zu einer vorhandenen hinzugefügt wurde) müssen Sie auch einige Produkte auswählen, die Sie mit der APP übermitteln möchten. Im iTunes Connect-Portal werden Sie dazu aufgefordert, dies zu tun, wie in diesem Screenshot gezeigt:

 [![Im iTunes Connect-Portal werden Sie aufgefordert, einige Produkte ebenfalls zu übermitteln.](in-app-purchase-basics-and-configuration-images/image13.png)](in-app-purchase-basics-and-configuration-images/image13.png#lightbox)   

Die Anwendung und die in-App-Käufe werden gleichzeitig überprüft, sodass Sie alle gleichzeitig genehmigt werden (sodass die APP ohne genehmigte Produkte nicht in den Store wechselt!).

Nachdem Ihre erste Version mit in-App-Kauf Funktion genehmigt wurde, können Sie weitere Produkte hinzufügen und Sie jederzeit zur Überprüfung einreichen. Sie können auch eine neue Version mit bestimmten in-App-Kauf Produkten übermitteln, indem Sie die Seite mit den **Versions Details** verwenden, die von der Eingabeaufforderung vorgeschlagen wird.

Weitere Informationen finden Sie in den [Richtlinien für den App Store-Review](https://developer.apple.com/appstore/guidelines.html) .

 [Teil 2: Übersicht über das Store-Kit und Abrufen von Produktinformationen](~/ios/platform/in-app-purchasing/store-kit-overview-and-retreiving-product-information.md)
