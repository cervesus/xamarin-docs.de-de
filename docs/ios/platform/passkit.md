---
title: PassKit in Xamarin.iOS
description: Die Wallet-app kann iOS-Benutzer digitale übergibt auf ihren Geräten zu speichern. Das PassKit-Framework kann Entwickler zum programmgesteuerten interagieren mit übergibt.
ms.prod: xamarin
ms.assetid: 74B9973B-C1E8-B727-3F6D-59C1F98BAB3A
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/13/2018
ms.openlocfilehash: d1c640bef41e875b3bb427d657c9c239e4c3e16d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121398"
---
# <a name="passkit-in-xamarinios"></a>PassKit in Xamarin.iOS

Die Wallet-app für iOS ermöglicht Benutzern das Speichern von digitalen übergibt auf ihren Geräten.
Diese Pässe werden von Händlern generiert und an den Kunden per e-Mail, URLs, oder über die die Händler-iOS-app gesendet. Diese Pässe können verschiedene Aspekte von Film-Tickets auf Treuekarten, Bordkarten darstellen. Das PassKit-Framework kann Entwickler zum programmgesteuerten interagieren mit übergibt.

In diesem Dokument werden die Wallet und verwenden die PassKit-API mit Xamarin.iOS.

 [![](passkit-images/image1.png "Die Wallet speichert und organisiert werden alle übergibt, die sich auf einem Smartphone")](passkit-images/image1.png#lightbox)

## <a name="requirements"></a>Anforderungen

Die in diesem Dokument behandelten PassKit-Features erfordern Ios6 und Xcode 4.5 zusammen mit Xamarin.iOS 6.0.

## <a name="introduction"></a>Einführung

Das Hauptproblem, das PassKit gelöst wird, ist der Verteilung und Verwaltung von Barcodes. Gehören einige praktische Beispiele wie Barcodes derzeit verwendet werden:

-   **Film Tickets online erwerben** : Kunden werden in der Regel per e-Mail einen Barcode, die ihre Tickets darstellt. Dieser Barcode wird ausgegeben, und klicken Sie auf die Kino umgeleitet wird, zum Eintrag durchsucht werden soll.
-   **Treuekarten** – Kunden eine Anzahl von anderen speicherspezifischen Karten in ihre Brieftasche oder Brieftasche tun und für das Anzeigen und überprüfen beim Erwerb waren.
-   **Coupons** – Coupons druckbaren Webseiten über Letterbox und als Barcodes in Zeitungen und Zeitschriften per e-Mail verteilt werden. Kunden werden in einem Speicher für die Überprüfung, um im Gegenzug waren und Dienstleistungen oder einen Rabatt zu erhalten.
-   **Bordkarten** – ähnlich wie ein Movie-Ticket zu kaufen.

PassKit bietet eine Alternative für jedes dieser Szenarien:

-   **Film Tickets** – nach dem Kauf, fügt der Kunde ein Ereignis Ticket übergeben (per e-Mail oder ein Link zur Website). Mit der Zeit die Movie-Ansätze der Durchlauf wird automatisch auf dem Sperrbildschirm als Erinnerung angezeigt, und bei ihrer Ankunft der Kino im Durchlauf problemlos abgerufen und in Wallet angezeigt wird, für die Überprüfung.
-   **Treuekarten** – statt (oder zusätzlich zu) Sie physischen Karten bei sich haben, speichert können ausgeben (per e-Mail oder nach einer Website-Anmeldung) übergeben Sie eine Store-Karte. Im Store bieten zusätzliche Funktionen wie z. B. den Kontostand des Kontos im Durchlauf mithilfe von Pushbenachrichtigungen zu aktualisieren, und mit Geolocation-Dienste im Durchlauf konnte automatisch angezeigt, auf dem Sperrbildschirm Wenn der Kunde in der Nähe ein Speicherort ist.
-   **Coupons** – Coupon übergibt können problemlos generiert mit eindeutige Merkmale, die bei der nachverfolgung zu unterstützen, und über e-Mail-Adresse oder Website Links verteilt. Heruntergeladene Coupons können automatisch auf dem Sperrbildschirm angezeigt, wenn der Benutzer ist in der Nähe von einem bestimmten Speicherort und/oder an einem bestimmten Datum (z. B. wenn das Ablaufdatum nähert). Da Coupons auf das Telefon des Benutzers gespeichert sind, werden sie sind immer nützlich und werden nicht verlegt. Gutscheine können Kunden empfohlen, Begleit-Apps heruntergeladen werden, da App-Store-Links in der Durchlauf, erhöhen Engagement mit dem Kunden integriert werden können.
-   **Bordkarten** – nach einer online-Check-in-Prozess würde der Kunde ihre bordkarte per e-Mail oder über einen Link empfangen. Eine Begleit-App, die vom Transport Provider bereitgestellt konnte den Eincheckvorgang Prozess, und auch zulassen, dass der Kunde zusätzliche Funktionen wie ihren Arbeitsplatz oder Gericht auswählen. Der Transportanbieter kann Push-Benachrichtigungen verwenden, aktualisieren Sie den Durchlauf Transport verzögert oder abgebrochen wird. Als Einstiegshilfe Zeit Ansätze werden im Durchlauf als Erinnerung und schnellen Zugriff auf den Durchlauf bereitstellen auf dem Sperrbildschirm angezeigt.

Im Grunde stellt PassKit eine einfache und praktische Möglichkeit zum Speichern und Anzeigen von Barcodes auf Ihrem iOS-Gerät bereit. Die zusätzliche Zeit und Ort Sperrbildschirm Integration integrieren Push-Benachrichtigungen und unterstützende Anwendung bietet eine Grundlage für sehr anspruchsvolle Vertrieb, Tickets und Abrechnung Dienste.

## <a name="passkit-ecosystem"></a>PassKit-Ökosystem

PassKit nicht nur eine API in CocoaTouch, sondern ist Teil einer größeren Ökosystem von apps, Daten und Dienste, die sichere Freigabe erleichtern, und Verwaltung von Barcodes und anderen Daten. Diese allgemeine Diagramm zeigt die unterschiedliche Entitäten, die einbezogen werden können in erstellen und Verwenden von übergibt:

 [![](passkit-images/image2.png "Dieses auf hoher Ebene Diagramm zeigt die Entitäten im erstellen und Verwenden von übergibt beteiligten")](passkit-images/image2.png#lightbox)

Jedes Datenelement des Ökosystems verfügt über eine klar definierte Rolle:

-   **Wallet** – Apple integrierte iOS-app, die gespeichert und übergibt angezeigt. Dies ist der einzige Ort, die für die Verwendung in der realen Welt übergibt gerendert werden (d. h. der Barcode, zusammen mit den lokalisierten Daten im Durchlauf angezeigt wird).
-   **Begleit-Apps** – iOS 6-apps, die vom erstellten Anbieter zum Erweitern der Funktionalität von der übergibt, diese ausgeben, z. B. das Hinzufügen von Wert in eine Store-Karte, und ändern den Platz für eine bordkarte oder andere unternehmensspezifische-Funktion, zu übergeben. Begleit-Apps sind nicht erforderlich, für einen Durchlauf nützlich ist.
-   **Ihr Server** – einen sicheren Server, in denen übergibt generiert und für die Verteilung signiert werden können. Ihre Companion-App können sich mit Ihrem Server zum Generieren von neuen übergibt, oder fordern Updates für vorhandene übergibt verbinden. Sie können optional im Web implementieren, Service-API, die Wallet aufgerufen werden musste, um zu aktualisieren übergibt.
-   **APNS-Servern** – Ihrer Server bietet die Möglichkeit, Wallet von Updates für einen Durchlauf für ein bestimmtes Gerät, das mit APNS zu benachrichtigen. Eine Pushbenachrichtigung zu Wallet die klicken Sie dann den Server für die Details der Änderung hergestellt werden. Begleit-apps müssen sich nicht um APNS für dieses Feature zu implementieren (sie können zum Überwachen der `PKPassLibraryDidChangeNotification` ).
-   **Kanal-Apps** – Anwendungen, die Elemente nicht direkt bearbeiten übergibt (wie Begleit-apps), aber die können ihre Nützlichkeit verbessern, indem übergibt erkennen und ermöglicht ihnen zu Wallet hinzugefügt werden. E-Mail-Clients, soziales Netzwerk Browsern und anderen Daten-Aggregation-apps möglicherweise alle Anlagen oder Links zu übergeben.

Das gesamte Ökosystem sieht komplex aus, damit es ist erwähnenswert, dass einige Komponenten optional sind und viel einfachere PassKit-Implementierungen möglich sind.

## <a name="what-is-a-pass"></a>Was ist eine Pass?

Ein Pass ist eine Sammlung von Daten, die ein Ticket, Coupon oder Karte darstellt. Es kann durch eine Einzelperson für eine einmalige Verwendung vorgesehen sein (und daher Details dazu, wie z. B. eine Zuordnung aktiv, Anzahl und der sitznummer enthalten) oder so kann es sich um ein mehrere verwenden-Token, das von einer beliebigen Anzahl von Benutzern (z. B. einen Rabatt von Coupon) gemeinsam genutzt werden kann. Eine ausführliche Beschreibung finden Sie im Apple [Dateien übergeben](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Bundle/Chapters/Introduction.html) Dokument.

### <a name="types"></a>Typen

Derzeit unterstützt fünf Typen, die in der Wallet-app von dem Layout und die oberen Rand des Passes unterschieden werden können:

-  **Ereignis-Ticket** – kleine halbkreisförmige Ausschnitt.
-   **Übergeben Sie Onboarding** – vertikalen Linien in der Seite, je nach Transport Symbol können angegeben werden (z. b. Bus, trainieren, Flugzeug).
-   **Karte Store** – abgerundeten oben, wie eine Karte Kredit- oder Debitkarte.
-  **Coupon** – entlang des oberen durchbrochenen.
-  **Generische** – Store-Karte Gerundetes Top identisch.


Die fünf Pass-Typen werden in diesem Screenshot gezeigt (in Reihenfolge: Coupon, generisch ist, speichern, Karte, bordkarte und Ereignis-Ticket):

 [![](passkit-images/image3.png "Die fünf Pass-Typen werden in diesem Screenshot gezeigt.")](passkit-images/image3.png#lightbox)

### <a name="file-structure"></a>Dateistruktur

Eine Pass-Datei ist eigentlich ein ZIP-Archiv mit einem **.pkpass** , enthält einige JSON-Dateien (erforderlich), die eine Vielzahl von Dateien mit der Erweiterung (optional) sowie die lokalisierten Zeichenfolgen (ebenfalls optional).

-   **Pass.JSON** : erforderlich. Enthält alle Informationen für den Lauf.
-   **"Manifest.JSON"** : erforderlich. Enthält die SHA1-Hashwerte für jede Datei in den Durchlauf mit Ausnahme der Signaturdatei und diese Datei ("Manifest.JSON").
-   **Signatur** : erforderlich. Durch Signieren erstellt die `manifest.json` Datei mit dem Zertifikat, das in der iOS-Bereitstellungsportal generiert.
-  **"Logo.png"** – optional.
-  **Background.PNG** – optional.
-  **Icon.PNG** – optional.
-  **Dateien von lokalisierbaren Zeichenfolgen** – optional.

Verzeichnisstruktur einer Pass-Datei wird unten angezeigt (Dies ist der Inhalt der ZIP-Archiv):

 [![](passkit-images/image4.png "Verzeichnisstruktur einer Pass-Datei wird hier angezeigt.")](passkit-images/image4.png#lightbox)

### <a name="passjson"></a>Pass.JSON

JSON ist das Format aus, da übergibt, in der Regel auf einem Server erstellt werden – dies bedeutet, dass der Code für die Cloudplattform agnostisch ist auf dem Server. Die drei wichtigen Bereichen der Informationen in jedem Durchlauf werden:

-   **TeamIdentifier** – auf diese Weise alle übergibt, die Sie, mit Ihrem App Store-Konto generieren verknüpft. Dieser Wert wird in der iOS-Bereitstellungsportal angezeigt.
-   **PassTypeIdentifier** – registrieren Sie sich in der Gruppe Bereitstellungsportal übergibt zusammen (falls Sie mehr als einen Typ erstellen). Z. B. eine kaffeehaus möglicherweise einen Store übergeben Sie Typ, um ihren Benutzern ermöglichen, die Loyalität Gutschriften verdienen erstellen, aber auch ein separater Coupon zu erstellen und Verteilen von Rabattcoupons Typ übergeben. Diese gleichen kaffeehaus möglicherweise sogar live Musik-Ereignisse enthalten und Ereignis Ticket übergibt für diejenigen ausstellen.
-   **SerialNumber** – eine eindeutige Zeichenfolge in dieser `passTypeidentifier` . Der Wert ist für die Wallet nicht transparent, aber es ist wichtig, dass bestimmte Durchläufe nachverfolgen, bei der Kommunikation mit dem Server.

Es ist eine große Anzahl von anderen JSON-Schlüssel in jedem Durchlauf, nachfolgend ein Beispiel dafür gezeigt:

``` 
{
   "passTypeIdentifier":"com.xamarin.passkitdoc.banana",  //Type Identifier (iOS Provisioning Portal)
   "formatVersion":1,                                     //Always 1 (for now)
   "organizationName":"Xamarin",                          //The name which appears on push notifications
   "serialNumber":"12345436XYZ",                          //A number for you to identify this pass
   "teamIdentifier":"XXXAAA1234",                         //Your Team ID
   "description":"Xamarin Demo",                          //
   "foregroundColor":"rgb(54,80,255)",                    //color of the data text (note the syntax)
   "backgroundColor":"rgb(209,255,247)",                  //color of the background
   "labelColor":"rgb(255,15,15)",                         //color of label text and icons
   "logoText":"Banana ",                                  //Text that appears next to logo on top
   "barcode":{                                            //Specification of the barcode (optional)
      "format":"PKBarcodeFormatQR",                       //Format can be QR, Text, Aztec, PDF417
      "message":"FREE-BANANA",                            //What to encode in barcode
      "messageEncoding":"iso-8859-1"                      //Encoding of the message
   },
   "relevantDate":"2012-09-15T15:15Z",                    //When to show pass on screen. ISO8601 formatted.
  /* The following fields are specific to which type of pass. The name of this object specifies the type, e.g., boardingPass below implies this is a boarding pass. Other options include storeCard, generic, coupon, and eventTicket */
   "boardingPass":{
/*headerFields, primaryFields, secondaryFields, and auxiliaryFields are arrays of field object. Each field has a key, label, and value*/
      "headerFields":[          //Header fields appear next to logoText
         {
            "key":"h1-label",   //Must be unique. Used by iOS apps to get the data.
            "label":"H1-label", //Label of the field
            "value":"H1"        //The actual data in the field
         },
         {
            "key":"h2-label",
            "label":"H2-label",
            "value":"H2"
         }
      ],
      "primaryFields":[       //Appearance differs based on pass type
         {
            "key":"p1-label",
            "label":"P1-label",
            "value":"P1"
         }
      ],
      "secondaryFields":[     //Typically appear below primaryFields
         {
            "key":"s1-label",
            "label":"S1-label",
            "value":"S1"
         }
      ],
      "auxiliaryFields":[    //Appear below secondary fields
         {
            "key":"a1-label",
            "label":"A1-label",
            "value":"A1"
         }
      ],
      "transitType":"PKTransitTypeAir"  //Only present in boradingPass type. Value can
                                        //Air, Bus, Boat, or Train. Impacts the picture
                                        //that shows in the middle of the pass.
   }
}
```

### <a name="barcodes"></a>Barcodes

Nur 2D-Formate werden unterstützt: PDF417, Aztec, QR. Apple behauptet, dass 1D Barcodes Scannen auf einen smartphonebildschirm beleuchtet auferlegte sind.

Alternativer Text unterhalb des Barcodes angezeigt wird, ist optional: einige Ladenbesitzer Lese-/manuell Typ möglich sein soll.

ISO-8859-1-Codierung ist die am häufigsten verwendete und überprüfen Sie die Codierung von der Überprüfungen Systemen verwendet wird, die Ihre übergibt gelesen wird.

### <a name="relevancy-lock-screen"></a>Relevanz (Sperrbildschirm)

Es gibt zwei Arten von Daten, die dazu ein übergeben auf dem Sperrbildschirm angezeigt werden:

 **Position**

Bis zu 10 Standorte kann in einem Durchlauf, z. B. Speicher, die ein Kunde häufig besucht, oder der Speicherort eines Kino oder Flughafen angegeben werden. Ein Kunde konnte diese Standorte über eine Begleit-App festgelegt werden, oder der Anbieter konnte nicht bestimmen sie anhand von Nutzungsdaten (wenn mit der Berechtigung für den Kunden aufgelistet).

Wenn der Pass auf dem Sperrbildschirm angezeigt wird, ist eine Umgrenzung berechnet wird, können Passes über den Sperrbildschirm ausgeblendet ist, wenn der Benutzer den Bereich verlässt. Der Radius ist gebunden, zum Übergeben der Stil, um Missbrauch zu verhindern.

 **Datum und Uhrzeit**

Nur ein Datum/Uhrzeit kann in einem Durchlauf angegeben werden. Datum und Uhrzeit eignet sich für das Auslösen von Sperrbildschirm Erinnerungen für Bordkarten und Ereignis-Tickets.

Kann mithilfe von Push oder über PassKit-API aktualisiert werden, damit die Datum/Uhrzeit im Falle eines Tickets mehrfach verwendet (z. B. ein einem Bereich Theater oder Sportereignisse komplexe Saison-Ticket) aktualisiert werden konnte.

### <a name="localization"></a>Lokalisierung

Einen Durchlauf in mehrere Sprachen übersetzt ist vergleichbar mit eine iOS-Anwendung zu lokalisieren: Erstellen einer Sprache bestimmte Verzeichnisse mit dem `.lproj` Erweiterung, und platzieren Sie die lokalisierte Elemente innerhalb. Übersetzen von Text eingegeben werden in einer `pass.strings` -Datei, während lokalisierter Bilder den gleichen Namen wie das Image müssen sie im Stammverzeichnis Pass ersetzen.

## <a name="security"></a>Sicherheit

Übergibt werden mit einem privaten Zertifikat signiert, die Sie in der iOS-Bereitstellungsportal zu generieren. Die Schritte zum Signieren der übergeben werden:

1.  Berechnen Sie einen SHA1-Hash für jede Datei im Verzeichnis übergeben (Fügen Sie keine der `manifest.json` oder `signature` -Datei, aber keine der vorhanden in dieser Phase trotzdem sein sollte).
1.  Schreiben von `manifest.json` als eine JSON-Schlüssel/Wert-Liste von jeder Dateiname mit dem Hash.
1.  Verwenden Sie das Zertifikat zum Signieren der `manifest.json` Datei, und das Ergebnis in eine Datei mit dem Schreiben `signature` .
1.  ZIPPEN Sie alles, und geben Sie die resultierende Datei eine `.pkpass` Dateierweiterung.


Da Ihren private Schlüssel zum Signieren der Pass erforderlich ist, sollte dieser Vorgang nur auf einem sicheren Server ausgeführt werden, die Sie steuern. Verteilen Sie Ihre Schlüssel zum Testen und zum Generieren von übergibt in einer Anwendung nicht.

 
## <a name="configuration-and-setup"></a>Konfiguration und Einrichtung

Dieser Abschnitt enthält Anweisungen zur Bereitstellung Ihrer Details zur Konfiguration und einem ersten Schritt erstellen.

### <a name="provisioning-passkit"></a>PassKit-Bereitstellung

In der Reihenfolge für einen Durchlauf der App-Store eingeben müssen sie ein Developer-Konto verknüpft werden. Dies sind zwei Schritte erforderlich:

1.  Der Pass muss mit einem eindeutigen Bezeichner, wird aufgerufen, die übergeben Typ-ID registriert werden
1.  Ein gültiges Zertifikat muss generiert werden, um den Lauf mit der Entwickler der digitalen Signatur signieren.

Um die folgenden Passtyp-ID nicht zu erstellen.

#### <a name="create-a-pass-type-id"></a>Erstellen Sie eine Pass-Typ-ID

Der erste Schritt ist das Einrichten einer Passtyp-ID für jedes einzelne _Typ_ Pass unterstützt werden müssen. Die Pass-ID (oder Typbezeichner übergeben) wird einen eindeutigen Bezeichner für den Lauf erstellt. Wir werden diese ID verwenden, um den Durchlauf mit Ihrem Developer-Konto mit einem Zertifikat zu verknüpfen.

1. In der [Zertifikate, Bezeichner und Profile im Abschnitt der iOS-Bereitstellungsportal](https://developer.apple.com/account/overview.action), navigieren Sie zu **Bezeichner** , und wählen Sie **Typ-IDs übergeben,** . Wählen Sie dann die **+** klicken, um einen neuen Pass-Typ zu erstellen: [ ![](passkit-images/passid.png "erstellen Sie einen neuen Pass-Typ")](passkit-images/passid.png#lightbox)

2.   Geben Sie einen **Beschreibung** (Name) und **Bezeichner** (eindeutige Zeichenfolge) für den Lauf. Beachten Sie, die alle übergeben Typ-IDs, mit der Zeichenfolge beginnen muss `pass.` In diesem Beispiel wir verwenden `pass.com.xamarin.coupon.banana` : [ ![](passkit-images/register.png "Geben Sie eine Beschreibung und einen Bezeichner")](passkit-images/register.png#lightbox)


3.   Bestätigen Sie die ID übergeben, indem er die **registrieren** Schaltfläche.

#### <a name="generate-a-certificate"></a>Generieren eines Zertifikats

Führen Sie folgende Schritte aus, um ein neues Zertifikat für diese Passtyp-ID zu erstellen:

1.  Wählen Sie die neu erstellte übergeben-ID aus der Liste aus, und klicken Sie auf **bearbeiten** : [ ![](passkit-images/pass-done.png "wählen Sie die ID des neuen übergeben, aus der Liste")](passkit-images/pass-done.png#lightbox)

    Wählen Sie dann **Create Certificate...** :

    [![](passkit-images/cert-dist.png "Wählen Sie Zertifikat erstellen")](passkit-images/cert-dist.png#lightbox)


2.  Führen Sie die Schritte zum Erstellen von einem Zertifikat Zertifikatsignieranforderungsdatei (CSR) ein.
  
3. Drücken Sie die **Weiter** Schaltfläche im Entwicklerportal und das Hochladen der CSR-Datei um Ihr Zertifikat zu generieren.

4. Laden Sie das Zertifikat, und doppelklicken Sie darauf, um es in Ihrer Keychain zu installieren.


Nun, dass wir ein Zertifikat für diese Passtyp-ID erstellt haben, wird im nächste Abschnitt beschrieben, wie einen Durchlauf manuell zu erstellen.

Weitere Informationen zur Bereitstellung für die Wallet finden Sie in der [arbeiten mit Funktionen](~/ios/deploy-test/provisioning/capabilities/wallet-capabilities.md) Guide.

### <a name="create-a-pass-manually"></a>Erstellen Sie manuell einen Durchlauf

Nun, wir den Typ übergeben erstellt haben wir einen Durchlauf zum Testen der Simulator oder ein Gerät manuell erstellen können. Sind die Schritte, um einen Pass zu erstellen:

-  Erstellen Sie ein Verzeichnis aus, um die Pass-Dateien enthalten.
-  Erstellen Sie eine pass.json-Datei, die alle erforderlichen Daten enthält.
-  Schließen Sie Bilder im Ordner ein (falls erforderlich).
-  Berechnen Sie der SHA1-Hashwerte für jede Datei im Ordner "" und "Manifest.JSON" schreiben.
-  Melden Sie mit der heruntergeladenen Zertifikats P12-Datei "Manifest.JSON".
-  Der Inhalt des Verzeichnisses ZIP, und benennen Sie .pkpass-Erweiterung.


Es gibt einige Quelldateien in der [Beispielcode](https://developer.xamarin.com/samples/monotouch/PassKit/) für diesen Artikel, die verwendet werden kann, um einen Pass zu generieren. Verwenden Sie die Dateien in die `CouponBanana.raw` Verzeichnis des Verzeichnisses CreateAPassManually. Die folgenden Dateien sind vorhanden:

 [![](passkit-images/image18.png "Diese Dateien sind vorhanden.")](passkit-images/image18.png#lightbox)

Öffnen Sie pass.json und bearbeiten Sie den JSON-Code. Sie müssen mindestens aktualisieren die `passTypeIdentifier` und `teamIdentifer` entsprechend Ihrem Apple-Entwicklerkonto.

```csharp
"passTypeIdentifier" : "pass.com.xamarin.coupon.banana",
"teamIdentifier" : "?????????",
```

Müssen Sie Sie dann die Hashes für jede Datei zu berechnen und die `manifest.json` Datei. Es sieht etwa wie folgt, wenn Sie fertig sind:

```csharp
{
  "icon@2x.png" : "30806547dcc6ee084a90210e2dc042d5d7d92a41",
  "icon.png" : "87e9ffb203beb2cce5de76113f8e9503aeab6ecc",
  "pass.json" : "c83cd1441c17ecc6c5911bae530d54500f57d9eb",
  "logo.png" : "b3cd8a488b0674ef4e7d941d5edbb4b5b0e6823f",
  "logo@2x.png" : "3ccd214765507f9eab7244acc54cc4ac733baf87"
}
```

Als Nächstes muss eine Signatur generiert werden, für diese Datei mit dem Zertifikat (P12-Datei), das für diese Pass-Typ-ID generiert wurde

#### <a name="signing-on-a-mac"></a>Anmeldung mit einem Mac

Herunterladen der **Wallet Ausgangswert Unterstützung Materialien** aus der [Apple Downloads](https://developer.apple.com/downloads/index.action?name=Passbook) Standort. Verwenden der `signpass` Tool, um den Ordner in einem Durchlauf aktivieren (dies auch berechnet den SHA1-Wert erstellt einen Hashwert und die Ausgabe in eine Datei .pkpass ZIP).

#### <a name="testing"></a>Test

Wenn Sie die Ausgabe dieser Tools überprüfen (durch Festlegen der Dateiname in .zip, und klicken Sie dann öffnen), sehen Sie die folgenden Dateien (Beachten Sie das Hinzufügen der `manifest.json` und `signature` Dateien):

 [![](passkit-images/image19.png "Untersuchen der Ausgabe dieser Tools")](passkit-images/image19.png#lightbox)

Sobald Sie angemeldet, ZIP und die Datei (z. b. um `BananaCoupon.pkpass`) können Sie ziehen Sie es in den Simulator zum Testen oder e-Mail an sich selbst auf einem echten Gerät abgerufen. Daraufhin sollte einen Bildschirm, um **hinzufügen** im Durchlauf wie folgt:

 [![](passkit-images/image20.png "Fügen Sie die Pass-Bildschirm")](passkit-images/image20.png#lightbox)

Normalerweise würde dieser Prozess automatisiert werden, auf einem Server, jedoch manuelle Pass-Erstellung eine Option für kleine Unternehmen, die nur Coupons erstellen, die nicht über die Unterstützung von Back-End-Server erfordern möglicherweise.

## <a name="wallet"></a>Wallet

Wallet ist das Herzstück des PassKit-Ökosystems. Dieser Screenshot zeigt die leere Wallet, und wie der Pass-Liste und die einzelnen Durchläufe aussehen:

 [![](passkit-images/image21.png "Dieser Screenshot zeigt die Wallet leer, und suchen Sie wie der Pass-Liste und die einzelnen Durchläufe")](passkit-images/image21.png#lightbox)

Der Wallet weist folgende Merkmale auf:

-  Es ist der einzige Ort, die mit ihrer Barcode Scannen übergibt gerendert werden.
-  Benutzer kann die Einstellungen für Updates ändern. Wenn aktiviert, können Pushbenachrichtigungen Updates der Daten im Durchlauf auslösen.
-  Benutzer kann aktivieren oder Deaktivieren von Sperrbildschirm Integration. Wenn aktiviert, können im Durchlauf automatisch auf dem Sperrbildschirm anzeigen, die basierend auf relevanten Zeit und Ort Daten, die in den Pass eingebettet angezeigt werden.
-  Der Rückseite des Passes unterstützt zum Aktualisieren ziehen, wenn eine Web-Server-URL in den Durchlauf von JSON angegeben wird.
-  Begleit-Apps geöffnet (oder heruntergeladen werden kann), wenn die ID der app im Durchlauf JSON angegeben wird.
-  Übergibt können (mit ein niedliches aufteilen Animation) gelöscht werden.

## <a name="adding-passes-into-wallet"></a>Bahnen zu Wallet hinzugefügt

Übergibt können auf folgende Weise zu Wallet hinzugefügt werden:

* **Kanal-Apps** – diese werden übergibt nicht direkt bearbeiten, sie einfach Pass-Dateien laden und Anzeigen der mit der Option Wallet hinzuzufügen. 

* **Begleit-Apps** – diese wurden von Anbietern zum Verteilen von übergibt, und bieten zusätzliche Funktionen zum Durchsuchen oder bearbeitet werden. Xamarin.iOS-Anwendungen haben vollständigen Zugriff auf die PassKit-API zur Erstellung und Bearbeitung von übergibt. Übergibt dann hinzugefügt werden können mit Wallet der `PKAddPassesViewController`. Dieser Prozess wird ausführlicher beschrieben. die **Begleitanwendungen** Abschnitt dieses Dokuments.

### <a name="conduit-applications"></a>Kanal-Anwendungen

Kanal-Anwendungen sind intermediate-apps, die möglicherweise übergibt im Namen eines Benutzers und programmiert werden sollten, auf deren Inhaltstyp zu erkennen, und geben Sie die Funktionalität aus, um die Wallet hinzugefügt. Beispiele für Kanal-apps sind:

-   **Mail** – erkennt die Anlage als bestanden.
-   **Safari** – der Content-Type-Pass erkennt, wenn ein Pass-URL-Link geklickt wird.
-   **Andere benutzerdefinierte apps** – jede app, die Anlagen empfangen, oder Öffnen von Links (social Media-Clients, e-Mail-Leser, usw.).


Dieser Screenshot zeigt, wie **-e-Mails** in iOS 6 eine Pass-Anlage erkennt und (sofern verwendet) bietet **hinzufügen** ihn Wallet.

 [![](passkit-images/image22.png "Dieser Screenshot zeigt, wie E-Mails in iOS 6 eine Pass-Anlage erkannt")](passkit-images/image22.png#lightbox)

 [![](passkit-images/image23.png "Dieser Screenshot zeigt, wie Mail bietet eine Pass-Anlage zu Wallet hinzugefügt")](passkit-images/image23.png#lightbox)

Wenn Sie eine app erstellen, die als datenleitung für übergeben werden konnten, können sie vom erkannt werden:

-  **Dateierweiterung** -.pkpass
-  **MIME-Typ** -application/vnd.apple.pkpass
-  **UTI** – com.apple.pkpass


Der grundlegende Vorgang einer Anwendung zuständig ist, rufen Sie die Pass-Datei, und rufen Sie Passkits `PKAddPassesViewController` , Benutzern die Möglichkeit, die im Durchlauf ihrer Wallet hinzuzufügen. Die Implementierung des View Controller wird im nächsten Abschnitt finden Sie unter **Begleitanwendungen**.

Kanal-Anwendungen müssen nicht für einen bestimmten Durchlauf Typ-ID auf die gleiche Weise bereitgestellt werden, die Companion Anwendungen ausführen.

## <a name="companion-applications"></a>Companion-Anwendungen

Eine unterstützende Anwendung bietet zusätzliche Funktionen für die Arbeit mit übergibt, einschließlich erstellen einen Pass, aktualisieren einen Durchlauf zugeordneten Informationen aus, und Verwalten von andernfalls übergibt, die der Anwendung zugeordnet.

Companion-Anwendungen sollten nicht versuchen, die Funktionen von Wallet zu duplizieren. Sie sind nicht vorgesehen, Pass zur Überprüfung angezeigt.

Im restlichen in diesem Abschnitt wird beschrieben, wie zum Erstellen einer einfachen Companion-App, mit der interagiert mit PassKit wird.

### <a name="provisioning"></a>Bereitstellung

Da Wallet eine Store-Technologie ist, wird die Anwendung muss separat bereitgestellt werden und kann keine Teambereitstellungsprofil oder Wildcard-App-ID. Finden Sie in der [arbeiten mit Funktionen](~/ios/deploy-test/provisioning/capabilities/wallet-capabilities.md) Handbuch, um einen eindeutigen App-ID und das Bereitstellungsprofil für die Wallet-Anwendung zu erstellen.

### <a name="entitlements"></a>Berechtigungen

Die **"Entitlements.plist"** Datei in aller letzten Xamarin.iOS-Projekt eingeschlossen werden soll. Um eine neue Entitlements.plist-Datei hinzuzufügen, führen Sie die Schritte in der [arbeiten mit Berechtigungen](~/ios/deploy-test/provisioning/entitlements.md) Guide.

Festzulegende Berechtigungen gehen Sie folgendermaßen vor:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Doppelklicken Sie auf die **"Entitlements.plist"** Datei im Projektmappenpad den Entitlements.plist-Editor geöffnet:

![](passkit-images/image31.png "Entitlements.plst-editor")

Wählen Sie im Abschnitt Wallet der **Wallet aktivieren** Option

![](passkit-images/image32.png "Wallet-Berechtigung aktivieren")


Die Standardoption ist für Ihre app zu ermöglichen, dass alle Typen zurückgegeben werden. Allerdings ist es möglich, Ihre app einzuschränken und nur eine Teilmenge von teampasstypen zulassen. Zum Aktivieren dieser Option die **zulassen Teilmenge des Teams passtypen** , und geben Sie die Pass-Typ-ID für die Teilmenge, die Sie zulassen möchten.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Doppelklicken Sie auf die **"Entitlements.plist"** Datei, um die XML-Quelldatei zu öffnen.

Um die Berechtigung für die Wallet hinzuzufügen, legen Sie die **Eigenschaft** zu `Passbook Identifiers` die wird automatisch festgelegt, in der Dropdownliste die **Typ** `Array`. Legen Sie dann die Zeichenfolge **Wert** zu `$(TeamIdentifierPrefix)*`:

![](passkit-images/image33.png "Wallet-Berechtigung aktivieren")

So akzeptiert Ihre App alle Passtypen. Um Ihre app einzuschränken und nur eine Teilmenge von teampasstypen zulassen, legen Sie den String-Wert auf:

`$(TeamIdentifierPrefix)pass.$(CFBundleIdentifier)`

Wo `pass.$(CFBundleIdentifier)` ist die Pass-ID, die erstellt wurde [oben](~/ios/platform/passkit.md)

-----

### <a name="debugging"></a>Debugging

Bereitstellen der Anwendung Probleme auftreten, überprüfen Sie, ob die richtige Verwendung von **Bereitstellungsprofil** und die `Entitlements.plist` ausgewählt ist, als die **benutzerdefinierte Berechtigungen** -Datei in die **iPhone Bundle-Signierung** Optionen.

Wenn Sie diese Fehler auftreten, bei der Bereitstellung:

```csharp
Installation failed: Your code signing/provisioning profiles are not correctly configured (error: 0xe8008016)
```

die `pass-type-identifiers` Berechtigungen Array ist falsch (oder entspricht nicht der **Bereitstellungsprofil**). Überprüfen Sie, ob der Typ-IDs, übergeben Sie und Ihre Team-ID richtig sind.

## <a name="classes"></a>Klassen

Die folgenden PassKit-Klassen sind für apps übergibt den Zugriff auf verfügbar:

-  **PKPass** – eine Instanz von einem Durchlauf.
-  **PKPassLibrary** – stellt die API für den Zugriff auf die leitet auf dem Gerät bereit.
-  **PKAddPassesViewController** – verwendet, um einen Pass für den Benutzer zum Speichern in ihre Brieftasche anzuzeigen.
-  **PKAddPassesViewControllerDelegate** – Xamarin.iOS-Entwickler

## <a name="example"></a>Beispiel

Finden Sie in das Projekt PassLibrary in die [Beispielcode](https://developer.xamarin.com/samples/monotouch/PassKit/) für diesen Artikel. Die folgenden allgemeinen Funktionen, die in einer Begleit-Anwendung Brieftasche erforderlich ist, werden veranschaulicht:

### <a name="check-that-wallet-is-available"></a>Überprüfen Sie, dass Wallet verfügbar ist.

Wallet ist nicht verfügbar, auf dem iPad, damit Anwendungen überprüfen soll, bevor Sie versuchen, PassKit-Funktionen zugreifen.

```csharp
if (PKPassLibrary.IsAvailable) {
    // create an instance and do stuff...
}
```

### <a name="creating-a-pass-library-instance"></a>Erstellen einer Instanz der Pass-Bibliothek

Die PassKit-Bibliothek ist kein Singleton, Anwendungen erstellen und zu speichern und für den Zugriff auf die PassKit-API-Instanz.

```csharp
if (PKPassLibrary.IsAvailable) {
    library = new PKPassLibrary ();
    // do stuff...
}
```

### <a name="get-a-list-of-passes"></a>Abrufen einer Liste von Durchläufen

Anwendungen können eine Liste von Durchläufen aus der Bibliothek anfordern. Diese Liste automatisch von PassKit, gefiltert, sodass Sie nur übergibt sehen, die mit Ihrem Team-ID erstellt wurden und die aufgeführt sind, in Ihre Berechtigungen.

```csharp
var passes = library.GetPasses ();  // returns PKPass[]
```

Beachten Sie, dass der Simulator die Liste der übergibt, die zurückgegeben wird, nicht filtern, damit diese Methode immer auf echten Geräten getestet werden soll. Diese Liste kann in einem UITableView angezeigt werden. Die [Beispiel-app](https://developer.xamarin.com/samples/monotouch/PassKit/) sieht wie folgt nach zwei Coupons hinzugefügt wurden:

 [![](passkit-images/image29.png "Das Aussehen der Beispiel-app wie folgt zwei Coupons hinzugefügt worden sind")](passkit-images/image29.png#lightbox)

### <a name="displaying-passes"></a>Anzeigen von übergibt

Eine begrenzte Anzahl von Informationen ist für das Rendering von Durchläufen in Begleit-apps verfügbar.

Wählen Sie aus diesem Satz von Standardeigenschaften zeigen Listen mit übergibt, wie der Beispielcode der Fall ist.

```csharp
string passInfo =
                "Desc:" + pass.LocalizedDescription
                + "\nOrg:" + pass.OrganizationName
                + "\nID:" + pass.PassTypeIdentifier
                + "\nDate:" + pass.RelevantDate
                + "\nWSUrl:" + pass.WebServiceUrl
                + "\n#" + pass.SerialNumber
                + "\nPassUrl:" + pass.PassUrl;
```

Diese Zeichenfolge wird angezeigt, wie eine Warnung in der [Beispiel](https://developer.xamarin.com/samples/monotouch/PassKit/):

 [![](passkit-images/image30.png "Die Warnung Coupon ausgewählt, in dem Beispiel")](passkit-images/image30.png#lightbox)

Sie können auch die `LocalizedValueForFieldKey()` Methode zum Abrufen von Daten aus Feldern in der übergibt, die Sie entwickelt haben (da Sie wissen, welche Felder sollten vorhanden sein). Im Beispielcode wird diese nicht angezeigt.

### <a name="loading-a-pass-from-a-file"></a>Laden einen Durchlauf aus einer Datei

Da ein Durchlauf Wallet mit Zustimmung des Benutzers nur hinzugefügt werden kann, muss ein ansichtscontroller angezeigt, und um entscheiden zu können. Dieser Code wird verwendet, der **hinzufügen** -Schaltfläche im Beispiel verwenden, um einen vordefinierten Durchlauf zu laden, die in der app eingebettet ist (Sie sollten dies durch eine ersetzen, die Sie sich angemeldet haben):

```csharp
NSData nsdata;
using ( FileStream oStream = File.Open (newFilePath, FileMode.Open ) ) {
        nsdata = NSData.FromStream ( oStream );
}
var err = new NSError(new NSString("42"), -42);
var newPass = new PKPass(nsdata,out err);
var pkapvc = new PKAddPassesViewController(newPass);
NavigationController.PresentModalViewController (pkapvc, true);
```

Der Pass erhält **hinzufügen** und **Abbrechen** Optionen:

 [![](passkit-images/image20.png "Der Lauf mit Optionen hinzufügen und \"Abbrechen\" angezeigt")](passkit-images/image20.png#lightbox)

### <a name="replace-an-existing-pass"></a>Übergeben Sie eine vorhandene zu ersetzen

Ersetzen einer vorhandenen Pass erfordert nicht die Berechtigung des Benutzers, jedoch fehl, wenn der Pass nicht bereits vorhanden ist.

```csharp
if (library.Contains (newPass)) {
     library.Replace (newPass);
}
```

### <a name="editing-a-pass"></a>Bearbeiten einen Durchlauf

PKPass nicht änderbar ist, ist, damit Sie Pass-Objekte in Ihrem Code nicht aktualisieren können. Zum Ändern der Daten in einem Durchlauf muss eine Anwendung auf einem Webserver zugreifen, die nachzuhalten, übergeben können, und generieren eine neue Pass-Datei mit aktualisierten Werten, die die Anwendung herunterladen können.

Pass-dateierstellung muss auf einem Server ausgeführt werden, da übergibt mit einem Zertifikat signiert werden müssen, die privat und sicher gehalten werden müssen.

Nachdem eine aktualisierte Pass-Datei generiert wurde, verwenden Sie die `Replace` Methode zum Überschreiben der alten Daten auf dem Gerät.

### <a name="display-a-pass-for-scanning"></a>Anzeigen von einem Durchlauf für die Überprüfung

Wie bereits erwähnt, nur einen Durchlauf für die Überprüfung von Wallet angezeigt werden kann. Ein Durchlauf kann angezeigt werden, mit der `OpenUrl` Methode wie gezeigt:

 `UIApplication.SharedApplication.OpenUrl (p.PassUrl);`

### <a name="receiving-notifications-of-changes"></a>Empfangen von Benachrichtigungen über Änderungen

Anwendungen können Änderungen an der Bibliothek übergeben Lauschen mithilfe der `PKPassLibraryDidChangeNotification`. Durch die Benachrichtigungen Auslösen von Updates im Hintergrund, daher ist es empfehlenswert, diese in Ihrer app zum Lauschen auf werden Änderungen verursacht.

```csharp
noteCenter = NSNotificationCenter.DefaultCenter.AddObserver (PKPassLibrary.DidChangeNotification, (not) => {
    BeginInvokeOnMainThread (() => {
        new UIAlertView("Pass Library Changed", "Notification Received", null, "OK", null).Show();
        // refresh the list
        var passlist = library.GetPasses ();
        table.Source = new TableSource (passlist, library);
        table.ReloadData ();
    });
}, library);  // IMPORTANT: must pass the library in
```

Es ist wichtig, übergeben Sie eine Bibliothek-Instanz, wenn Sie für die Benachrichtigung registrieren, da PKPassLibrary kein Singleton ist.

## <a name="server-processing"></a>Serververarbeitung

Eine ausführliche Erörterung der beim Erstellen einer Serveranwendung, die zur Unterstützung von PassKit ist Gegenstand dieser einführenden Artikel.

Finden Sie unter [Dotnet-Passbook](https://github.com/tomasmcguinness/dotnet-passbook) open-Source C# serverseitigen Code.

## <a name="push-notifications"></a>Pushbenachrichtigungen

Eine ausführliche Erörterung der mithilfe von Push-Benachrichtigungen zum Aktualisieren von Pass ist Gegenstand dieser einführenden Artikel.

Sie ist erforderlich, implementieren Sie die REST-ähnliche-API, die definiert, die von Apple auf webanforderungen von Wallet reagieren, wenn Updates erforderlich sind.

Finden Sie unter Apple [aktualisiert einen Durchlauf](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/PassKit_PG/Updating.html#//apple_ref/doc/uid/TP40012195-CH5-SW1) Anleitung finden Sie weitere Informationen.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel PassKit eingeführt, einige der Gründe, warum es sinnvoll ist, und beschriebenen die verschiedenen Teile, die für eine vollständige PassKit-Lösung implementiert werden müssen. Es wurde beschrieben, die erforderlichen Schritte zum Konfigurieren Ihrer Apple-Entwicklerkonto, um übergibt, den Prozess zu einem Durchlauf manuell, und auch Gewusst wie: Zugriff auf die PassKit-APIs aus einer Xamarin.iOS-Anwendung zu erstellen.

## <a name="related-links"></a>Verwandte Links

- [Wallet für Entwickler](https://developer.apple.com/wallet/)
- [PassKit-Beispiel](https://developer.xamarin.com/samples/monotouch/PassKit/)
- [Wallet-Entwicklerhandbuch](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/PassKit_PG/index.html#//apple_ref/doc/uid/TP40012195-CH1-SW1)
- [Frameworks: Apple Pay und Wallet (WWDC Videos)](https://developer.apple.com/videos/frameworks/apple-pay-and-wallet)
- [Referenz zu PassKit-Framework](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Framework/_index.html)
- [Passbook-Webdienstreferenz](https://developer.apple.com/library/prerelease/ios/#documentation/PassKit/Reference/PassKit_WebService/WebService.html)
- [Informationen zu Pass-Dateien](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Bundle/Chapters/Introduction.html)
- [Dotnet-Passbook](https://github.com/tomasmcguinness/dotnet-passbook), ein Open Source-Bibliothek für das Generieren von iOS Wallet-Pakete
- [Einführung in iOS 6](~/ios/platform/introduction-to-ios6/index.md)
