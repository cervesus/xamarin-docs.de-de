---
title: PassKit
description: Wallet ist eine System-iOS-app, die gespeichert und angezeigt werden, Barcodes und andere Informationen zum Verknüpfen von Kundentransaktionen auf ihre Mobiltelefon mit der realen Welt.
ms.prod: xamarin
ms.assetid: 74B9973B-C1E8-B727-3F6D-59C1F98BAB3A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: f1c8ac92c5ff7eed5116587ed13755ddee74a877
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="passkit"></a>PassKit

_Wallet ist eine System-iOS-app, die gespeichert und angezeigt werden, Barcodes und andere Informationen zum Verknüpfen von Kundentransaktionen auf ihre Mobiltelefon mit der realen Welt._

Wallet ist eine app für iPhones und iPod mit iOS 6 berührt. Er speichert und zeigt Barcodes und andere Informationen zum Verknüpfen von Kundentransaktionen auf ihre Mobiltelefon mit der realen Welt. Übergibt von Händlern generiert und gesendet, die an den Kunden per e-Mail, URLs oder in eine eines Anbieters iOS-app. Wallet speichert und organisiert alle übergibt, die sich auf einem Telefon und Erinnerungen Pass auf dem Sperrbildschirm je nach Datum/Uhrzeit oder den Speicherort des Geräts angezeigt.

Dieses Dokument führt Wallet, Xamarin.iOS, mit der Kit-API übergeben und erläutert, wie auf dem Server übergibt implementieren.

 [![](passkit-images/image1.png "Die Wallet speichert und organisiert werden allen Durchläufen auf einem Telefon")](passkit-images/image1.png#lightbox)


## <a name="requirements"></a>Anforderungen

Die Store-Kit-Funktionen, die in diesem Dokument beschriebenen erforderlich ist, iOS 6 und Xcode 4.5 zusammen mit Xamarin.iOS 6.0.


## <a name="introduction"></a>Einführung

Das Hauptproblem, das übergeben Kit gelöst ist die Verteilung und Verwaltung von Barcodes. Beispiele für einige praktische wie Barcodes derzeit verwendet werden:

-   **Film Tickets online kaufen** – Kunden werden in der Regel per e-Mail einen Barcode, die ihren Fahrkarten darstellt. Dieser Barcode wird gedruckt und ausgeführt, um die im Kino, die für den Eintrag gescannt werden.
-   **Kundentreue Karten** – Kunden führen eine Anzahl von verschiedenen speicherspezifischen Karten in ihren Wallet Ringwaden, für das Anzeigen und Scannen, wenn sie Waren zu erwerben.
-   **Coupons** – Coupons druckbaren Webseiten über Briefkästen und als Barcodes in Zeitungen und Zeitschriften per e-Mail verteilt werden. Kunden müssen Sie sie in einen Speicher für das Scannen, um im Gegenzug waren und Dienstleistungen oder einen Rabatt zu erhalten.
-   **Übergibt ein-** – ähnlich wie ein Movie-Ticket zu kaufen.


Pass-Kit bietet eine Alternative für jedes dieser Szenarien:

-   **Film Tickets** – nach dem Kauf, fügt der Kunde ein Ereignis Ticket übergeben (per e-Mail oder ein Link zur Website). Als die Zeit für die Film-Ansätze der Durchlauf wird automatisch auf dem Sperrbildschirm Erinnerung angezeigt, und bei ihrer Ankunft der im Kino der Durchlauf wird problemlos abgerufen und in Wallet zum Scannen angezeigt.
-   **Kundentreue Karten** – statt (oder zusätzlich) Sie physischen Karten bei sich haben, speichert können ausstellen (per e-Mail oder nach einer Website-Anmeldung) eine Übergabe der Store-Karte. Der Speicher kann stellen zusätzliche Funktionen wie das Aktualisieren des Saldo des Kontos für die Übergabe über Pushbenachrichtigungen und Geolocation-Dienste mit der Durchlauf konnte automatisch angezeigt, auf dem Sperrbildschirm Wenn der Kunde in der Nähe von einem Speicherort ist.
-   **Coupons** – Coupons übergibt können problemlos generiert mit eindeutige Merkmale, die bei der Überwachung unterstützen, und über e-Mail-Adresse oder Website Links verteilt. Heruntergeladene Coupons können automatisch auf dem Sperrbildschirm angezeigt, wenn der Benutzer ist in der Nähe von einem bestimmten Speicherort und/oder an einem bestimmten Termin (z. B. wenn das Ablaufdatum nähert). Da die Coupons auf der Anschluss des Benutzers gespeichert sind, sind immer praktisch und nicht verlegt. Coupons möglicherweise ermutigen Sie Kunden Begleit-Apps herunterladen, da App Store-Links in den Durchlauf, erhöhen die Zusammenarbeit mit Kunden integriert werden können.
-   **Übergibt ein-** – nach einer online-Check-in-Prozess würde der Kunde erhalten ihre Integration Pass per e-Mail oder ein Link. Eine Begleit-App, die vom Transportanbieter bereitgestellt könnten schließen Sie den Prozess Einchecken und auch ermöglichen es dem Kunden zusätzliche Funktionen wie ihre Arbeitsplätze oder Mahlzeit auswählen. Der Transportanbieter können Pushbenachrichtigungen die erfolgreich aktualisiert wird, wenn der Transport verzögert oder abgebrochen wird. Als Einstiegshilfe Zeit Ansätze der Pass auf dem Sperrbildschirm Erinnerung und schnellen Zugriff auf den Durchlauf bereitstellen erscheint.


Im Kern enthält Kit übergeben eine einfache und praktische Möglichkeit zum Speichern und Anzeigen von Barcodes auf Ihrem iPhone oder iPod Touch-Gerät. Der zusätzliche Zeit und Speicherort Sperrbildschirm Integration integrieren, Pushbenachrichtigungen und Begleit-Anwendung bietet eine Grundlage für sehr anspruchsvolle Sales ticketausstellung und Abrechnung Dienste.


## <a name="pass-kit-ecosystem"></a>Übergeben Sie Kit Ökosystem

Pass-Kit ist nicht einfach eine API innerhalb CocoaTouch, sondern ist Teil einer größeren Ökosystem von apps, Daten und Dienste, die die sichere Freigabe zu ermöglichen und Verwaltung von Barcodes und anderen Daten. Das übersichtlichen Diagramm zeigt verschiedenen Entitäten, die einbezogen werden können, in erstellen und Verwenden von übergibt:

 [![](passkit-images/image2.png "Dieses Diagramm auf hoher Ebene Aufschluss über die Entitäten erstellen und Verwenden von übergibt")](passkit-images/image2.png#lightbox)

Jeder Teil der Umgebung hat eine klar definierte Rolle:

-   **Wallet** – Apple integrierte iOS-app (für iPhone- und iPod Touch), die gespeichert und übergibt angezeigt. Dies ist der einzige Ort, die für die Verwendung in der realen Welt übergibt gerendert werden (ie der Barcode, zusammen mit den lokalisierten Daten im Durchgang angezeigt wird).
-   **Begleit-Apps** – iOS 6-apps, die von Pass-Anbietern zum Erweitern der Funktionalität des Laufs, sie ausgeben, z. B. das Hinzufügen von Wert in eine Store-Karte, ändern den Platz für eine Einstiegshilfe Übergabe oder andere geschäftsspezifische-Funktion, erstellt. Begleit-Apps sind nicht für eine Übergabe sind aussagekräftig sind erforderlich.
-   **Ihr Server** – einem sicheren Server, auf dem übergibt generiert und für die Verteilung signiert werden können. Die Begleit-App kann auf Ihrem Server zum neuen übergibt generieren, oder bitten Sie Updates für vorhandene übergibt verbinden. Sie können optional die Webdienst-API implementieren, die Wallet aufgerufen würde, um übergibt zu aktualisieren.
-   **APNs-Servern** – Ihr Server hat die Möglichkeit, Wallet von Updates für eine Übergabe auf einem bestimmten Gerät mit APNS zu benachrichtigen. Drücken Sie eine Benachrichtigung, um Wallet die klicken Sie dann den Server für die Details der Änderung Kontakt aufgenommen wird. Begleit-apps müssen nicht APNS für dieses Feature zu implementieren (sie können zum Überwachen der `PKPassLibraryDidChangeNotification` ).
-   **Zwischen Apps** – Anwendungen, die nicht direkt bearbeiten, übergibt (z. B. Begleit-apps ausgeführt werden), aber die können ihre Nützlichkeit verbessern, indem übergibt erkennen und ihnen ermöglicht, Wallet hinzugefügt werden. E-Mail-Clients, soziale Netzwerke Browsern und andere Daten Aggregation apps auftreten alle Anlagen oder Links zu übergeben.


Die gesamte Ökosystem sieht komplex ist, ist Folgendes zu beachten, dass einige Komponenten optional sind und viel einfachere übergeben Kit-Implementierungen möglich sind.

## <a name="what-is-a-pass"></a>Was ist eine Übergabe?

Ein Durchlauf ist eine Sammlung von Daten, die ein Ticket, Coupons oder eine Karte darstellt. Es möglicherweise durch eine Einzelperson für eine einmalige Verwendung vorgesehen sein (und daher enthalten Details wie z. B. eine Zuweisung Flight, Anzahl und Arbeitsplätze) oder es kann ein mehrere verwenden-Token, das von einer beliebigen Anzahl von Benutzern (z. B. eines Coupons Rabatt) gemeinsam genutzt werden kann. Eine ausführliche Beschreibung finden Sie in der Apple- [Dateien übergeben](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Bundle/Chapters/Introduction.html) Dokument.


### <a name="types"></a>Typen

Derzeit unterstützt fünf Typen, die in der Wallet-app von dem Layout und die oberen Rand des Durchgangs unterschieden werden können:

-  **Ereignis-Ticket** – kleine halbkreisförmige Ausschnitt.
-   **Übergeben Sie die Pflege** – Stufen im Symbol "-Seite, transportspezifische" können angegeben werden (z. b. Bus trainieren, Flugzeug).
-   **Speichern Sie die Karte** – abgerundeten oben, wie eine Kreditkarte.
-  **Coupondetails** – entlang der Oberkante perforierte.
-  **Generische** – identisch mit Speicher-Karte Gerundetes Top.


Die fünf Pass-Typen sind in diesem Screenshot gezeigt (in der Reihenfolge: Coupons generisch ist, zu speichern, Karten, Einstiegshilfe bestanden wurden und Ereignis-Ticket):

 [![](passkit-images/image3.png "Die fünf Pass-Typen sind in diesem Screenshot dargestellt.")](passkit-images/image3.png#lightbox)

### <a name="file-structure"></a>Dateistruktur

Eine Pass-Datei ist tatsächlich ein ZIP-Archiv mit einem **.pkpass** Erweiterung, mit einigen bestimmten JSON-Dateien (erforderlich), eine Vielzahl von Dateien (optional) sowie lokalisierte Zeichenfolgen (ebenfalls optional).

-   **Pass.JSON** – erforderlich. Enthält alle Informationen für die Übergabe an.
-   **Manifest.JSON** – erforderlich. Enthält die SHA1-Hashes für jede Datei in der Bahn mit Ausnahme der Signaturdatei und diese Datei (manifest.json).
-   **Signatur** – erforderlich. Erstellt durch das Signieren der `manifest.json` Datei mit dem Zertifikat in iOS-Bereitstellungsportal generiert.
-  **logo.png** – optional.
-  **background.png** – optional.
-  **icon.png** – optional.
-  **Lokalisierbaren Zeichenfolgen Dateien** – optional.


Verzeichnisstruktur einer Pass-Datei wird unten angezeigt (Dies ist der Inhalt der ZIP-Archiv):

 [![](passkit-images/image4.png "Verzeichnisstruktur einer Pass-Datei wird hier angezeigt.")](passkit-images/image4.png#lightbox)

### <a name="passjson"></a>pass.json

JSON ist das Format aus, da übergibt in der Regel auf einem Server erstellt werden – dies bedeutet, dass vom Generierungscode plattformunabhängigem ist auf dem Server. Die drei wichtigsten Arten von Informationen in jedem Durchlauf werden:

-   **TeamIdentifier** – auf diese Weise alle übergibt, die Sie mit Ihrem App Store-Konto generieren verknüpft. Dieser Wert ist in der iOS-Bereitstellungsportal sichtbar.
-   **PassTypeIdentifier** – Register im Portal-Bereitstellung, zur Gruppe übergibt zusammen (falls Sie mehrere Typen erzeugen). Beispielsweise kann einem Internetcafé erstellen, eine Karte übergeben Speichertyp können ihre Kunden Kundentreue Gutschriften verdienen, sondern auch ein separater Coupons übergeben Typ zum Erstellen und Verteilen von Rabattcoupons. Diese gleichen Internetcafé möglicherweise sogar live Musik Ereignisse enthalten und geben Ereignis Ticket übergibt für diejenigen aus.
-   **SerialNumber** – eine eindeutige Zeichenfolge in dieser `passTypeidentifier` . Der Wert ist für Wallet nicht transparent, aber es ist wichtig für das Nachverfolgen von bestimmten übergibt bei der Kommunikation mit dem Server.


Es wird eine große Anzahl von anderen JSON-Schlüssel in jedem Durchlauf, von denen das Beispiel unten zeigt ist:

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

Nur 2D Formate werden unterstützt: PDF417, Aztec, QR. Apple vorgibt, dass 1D Barcodes-Scan auf einem beleuchtet Phone Bildschirm richtigen sind.

Alternativer Text unterhalb der Barcode angezeigt wird, ist optional – einige Händlern soll in Lese-/manuell Typ sein.

ISO-8859-1-Codierung ist die am häufigsten verwendete und überprüfen, welche Codierung von den Scannen-Systemen verwendet wird, die Ihre übergibt gelesen wird.

### <a name="relevancy-lock-screen"></a>Relevanz (Bildschirm)

Es gibt zwei Arten von Daten, die dazu eine Übergabe, die auf dem Sperrbildschirm angezeigt werden:

 **Position**

Bis zu 10 Speicherorte kann in einem Durchlauf, z. B. Speicher, die häufig ein Kunde besucht oder den Speicherort einer im Kino oder Flughafen angegeben werden. Ein Kunde kann dort über eine Begleit-App festlegen, oder der Anbieter konnte nicht bestimmen sie anhand von Nutzungsdaten (wenn mit der Berechtigung des Kunden aufgelistet).

Wenn der Pass auf dem Sperrbildschirm angezeigt wird, wird eine Umgrenzung berechnet, damit die erfolgreich aus dem Sperrbildschirm ausgeblendet ist, wenn der Benutzer den Bereich verlässt. Radius ist gebunden, übergeben Sie Stil, um Missbrauch zu verhindern.

 **Datum und Uhrzeit**

In einem Durchlauf kann nur ein Datum/Uhrzeit angegeben werden. Das Datum und die Uhrzeit eignet sich für das Auslösen von Sperrbildschirm Erinnerungen für Einstiegshilfe übergibt und Ereignis-Tickets.

Kann per Push oder per PassKit-API aktualisiert werden, damit die Datum/Uhrzeit im Falle eines Tickets mehrfach verwendet (z. B. ein Saison Ticket zu einer Region oder Sport komplexe) aktualisiert werden konnte.

### <a name="localization"></a>Lokalisierung

Eine Übergabe in mehrere Sprachen übersetzen ähnelt der Lokalisierungsprozess einer iOS-Anwendung – Sprache erstellen, bestimmte Verzeichnisse mit den `.lproj` Erweiterung, und platzieren Sie die lokalisierte Elemente innerhalb. Übersetzungen Text eingegeben werden in einer `pass.strings` Datei, während lokalisierte Bilder den gleichen Namen wie das Bild haben sollten sie im Stammverzeichnis Pass ersetzen.

## <a name="security"></a>Sicherheit

Übergibt werden mit ein privates Zertifikat signiert, die in der iOS-Bereitstellungsportal generiert werden. Die Schritte zum Signieren der übergeben werden:

1.  SHA1-Hash für jede Datei im Verzeichnis Pass berechnen (verwenden Sie keine der `manifest.json` oder `signature` keines der vorhanden, in diesem Stadium trotzdem sein sollten-Datei).
1.  Schreiben von `manifest.json` als ein JSON-Schlüssel/Wert-Liste von jedem Dateinamen mit dem Hash.
1.  Verwenden Sie das Zertifikat zum Signieren der `manifest.json` Datei und das Ergebnis in eine Datei namens schreiben `signature` .
1.  Das alles KOMPRIMIEREN, und geben Sie die resultierende Datei eine `.pkpass` Dateierweiterung.


Da mit Ihrem private Schlüssel zum Signieren der Übergabe erforderlich ist, sollte dieser Vorgang nur auf einem sicheren Server vorgenommen werden, die Sie steuern. Verteilen Sie der Schlüssel, um die Generierung übergibt in einer Anwendung nicht.

 
## <a name="configuration-and-setup"></a>Konfiguration und Installation

Dieser Abschnitt enthält Anweisungen zum Einrichten von Details der Bereitstellung und Erstellen Ihrer ersten Durchlauf.

### <a name="provisioning-passkit"></a>Provisioning PassKit

In der Reihenfolge für eine Übergabe an den App Store eingeben muss er mit einem Entwicklerkonto verknüpft werden. Hier sind zwei Schritte erforderlich:

1.  Die Übergabe muss einen eindeutigen Bezeichner, die übergeben Typ-ID aufgerufen registriert
1.  Ein gültiges Zertifikat muss generiert werden, um den Durchlauf mit der Entwickler digitale Signatur zu signieren.

Ein Typ-ID übergeben möchten folgenden erstellen.


<a name="create-passid"/>

#### <a name="create-a-pass-type-id"></a>Erstellen Sie eine Pass-Typ-ID

Der erste Schritt ist das Einrichten einer übergeben Typ-ID für jedes einzelne _Typ_ erfolgreich unterstützt werden müssen. Die ID übergeben (oder Typbezeichner übergeben) wird einen eindeutigen Bezeichner für die erfolgreich erstellt. Wir verwenden diese ID, die erfolgreich mit Ihrem Entwicklerkonto mithilfe eines Zertifikats zu verknüpfen.

1. In der [Zertifikate, Bezeichner und Profile-Abschnitt des iOS-Bereitstellungsportal](https://developer.apple.com/account/overview.action), navigieren Sie zu **Bezeichner** , und wählen Sie **Typ-IDs übergeben** . Wählen Sie dann die **+** um einen neuen Pass-Typ zu erstellen: [ ![ ] (passkit-images/passid.png "Erstellen eines neuen Pass-Typs")](passkit-images/passid.png#lightbox)

2.   Geben Sie einen **Beschreibung** (Name) und **Bezeichner** (eindeutige Zeichenfolge) für die erfolgreich. Beachten Sie, die alle übergeben Typ-IDs mit der Zeichenfolge beginnen muss `pass.` In diesem Beispiel wir verwenden `pass.com.xamarin.coupon.banana` : [ ![ ] (passkit-images/register.png "Geben Sie eine Beschreibung und einen Bezeichner")](passkit-images/register.png#lightbox)


3.   Bestätigen Sie die ID übergeben, durch Drücken der **registrieren** Schaltfläche.


<a name="generate" />

#### <a name="generate-a-certificate"></a>Generieren eines Zertifikats

Um ein neues Zertifikat für diese ID übergeben zu erstellen, führen Sie folgende Schritte aus:

1.  Wählen Sie die neu erstellte übergeben-ID aus der Liste aus, und klicken Sie auf **bearbeiten** : [ ![ ] (passkit-images/pass-done.png "wählen Sie aus der Liste die ID des neuen übergeben")](passkit-images/pass-done.png#lightbox)

    Aktivieren Sie das Kontrollkästchen **Zertifikat erstellen...** :

    [![](passkit-images/cert-dist.png "Wählen Sie erstellen Zertifikat.")](passkit-images/cert-dist.png#lightbox)


2.  Führen Sie die Schritte zum Erstellen einer Zertifikatsignieranforderung (CSR).
  
3. Drücken Sie die **Fortfahren** Schaltfläche am Entwicklerportal und Hochladen der Zertifikatsignieranforderung um Ihr Zertifikat zu generieren.

4. Herunterladen Sie des Zertifikats, und doppelklicken Sie darauf, es im Schlüsselbund zu installieren.


Nun, da es ein Zertifikat für diese ID übergeben erstellt haben, wird im nächste Abschnitt beschrieben, wie eine Übergabe manuell erstellen.

Weitere Informationen zu einer Bereitstellung für Wallet, finden Sie in der [arbeiten mit Funktionen](~/ios/deploy-test/provisioning/capabilities/wallet-capabilities.md) Handbuch.

 <a name="Create_a_Pass_Manually" />

### <a name="create-a-pass-manually"></a>Erstellen Sie manuell eine Übergabe

Nun, wir den Typ übergeben wir einen Durchlauf erstellt haben so testen Sie den Simulator bzw. ein Gerät manuell erstellen können. Die Schritte zum Erstellen einer übergeben werden:

-  Erstellen Sie ein Verzeichnis aus, um die Pass-Dateien enthalten.
-  Erstellen Sie eine pass.json-Datei, die alle erforderlichen Daten enthält.
-  Schließen Sie Bilder im Ordner "" ein (falls erforderlich).
-  Berechnen Sie die SHA1-Hashwerte für jede Datei im Ordner "", und Schreiben in manifest.json.
-  Melden Sie manifest.json mit der heruntergeladenen Zertifikats P12-Datei.
-  Der Inhalt des Verzeichnisses ZIP, und benennen Sie mit der Erweiterung .pkpass.


Es gibt einige Quelldateien im Beispielcode für diesen Artikel, die verwendet werden kann, um eine Übergabe zu generieren. Verwenden Sie die Dateien in den `CouponBanana.raw` des Verzeichnisses CreateAPassManually Verzeichnis. Die folgenden Dateien sind vorhanden:

 [![](passkit-images/image18.png "Diese Dateien sind vorhanden.")](passkit-images/image18.png#lightbox)

Öffnen Sie pass.json und bearbeiten Sie die JSON-Objekte. Sie müssen mindestens ein update der `passTypeIdentifier` und `teamIdentifer` entsprechend Ihrem Apple Developer-Konto.

```csharp
"passTypeIdentifier" : "pass.com.xamarin.coupon.banana",
"teamIdentifier" : "?????????",
```

Dann müssen Sie die Hashes für jede Datei berechnen und erstellen die `manifest.json` Datei. Es sieht etwa wie folgt, wenn Sie fertig sind:

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

 <a name="Signing_On_a_Mac" />


#### <a name="signing-on-a-mac"></a>Auf einem Mac signieren

Herunterladen der **Wallet Ausgangswert Unterstützung Materialien** aus der [Apple Downloads](https://developer.apple.com/downloads/index.action?name=Passbook) Standort. Verwenden der `signpass` Tool, um eine Übergabe Ordner verwandeln (Dies wird auch Berechnen der SHA1 erstellt einen Hashwert und die Ausgabe in eine Datei .pkpass ZIP).

 <a name="Signing_On_a_PC" />


#### <a name="signing-on-a-pc"></a>Auf einem PC anmelden

Im Beispiel wird Code für diesen Artikel gibt es ein Projekt mit der Bezeichnung `signpassnet` , die in .NET unter Windows ausgeführt wird. Es wird versucht Apple Tool zu imitieren, aber sie viel weniger Code für die Überprüfung enthält.

 <a name="Testing" />


#### <a name="testing"></a>Test

Wenn Sie die Ausgabe dieser Tools zu überprüfen, (durch Festlegen der Dateiname in .zip, und öffnen ihn), sehen Sie die folgenden Dateien (Beachten Sie das Hinzufügen der `manifest.json` und `signature` Dateien):

 [![](passkit-images/image19.png "Untersuchen die Ausgabe dieser Tools")](passkit-images/image19.png#lightbox)

Sobald Sie angemeldet, ZIP und erhält die Datei (z. b. um `BananaCoupon.pkpass`) können Sie ziehen Sie es in der Simulator zum Testen oder per e-Mail an sich selbst auf einem echten Gerät abrufen. Daraufhin sollte einen Bildschirm, um **hinzufügen** übergeben, wie folgt:

 [![](passkit-images/image20.png "Fügen Sie den Bildschirm übergeben")](passkit-images/image20.png#lightbox)

Normalerweise würde diese Prozess auf einem Server, jedoch manuelle Erstellung von Pass-Option ist für kleine Unternehmen, die nur Coupons erstellen, die nicht über die Unterstützung von Back-End-Server erfordern möglicherweise automatisiert werden.

 <a name="Wallet" />

## <a name="wallet"></a>Wallet

Wallet ist die zentrale Information das Ökosystem Kit übergeben. Diese bildschirmabbildung zeigt leere Wallet und wie der Pass-Liste und einzelne übergibt aussehen:

 [![](passkit-images/image21.png "Diese bildschirmabbildung zeigt leere Wallet, und wie der Pass-Liste und einzelne übergibt suchen")](passkit-images/image21.png#lightbox)

Wallet-Features aufgeführt:

-  Es ist der einzige Ort, die mit ihren Barcode Scannen übergibt gerendert werden.
-  Benutzer kann die Einstellungen für Updates ändern. Wenn aktiviert, können Pushbenachrichtigungen Updates der Daten in den Durchlauf auslösen.
-  Benutzer kann aktivieren oder Deaktivieren von Sperrbildschirm Integration. Wenn aktiviert, ermöglicht die Übergabe an automatisch auf ihren Sperrbildschirm angezeigt anhand dies relevanten Zeitpunkt und Ort Daten, die in den Durchlauf eingebettet.
-  Der Rückseite des Durchgangs unterstützt Pull zum Aktualisieren, wenn eine Web-Server-URL, in der JSON-übergeben angegeben wird.
-  Begleit-Apps kann geöffnet (oder heruntergeladen werden), wenn die app-ID, in der JSON-übergeben angegeben wird.
-  Übergibt können (mit einer nette vernichten Animation) gelöscht werden.


 <a name="Getting_Passes_into_Wallet" />

## <a name="adding-passes-into-wallet"></a>Übergibt in Wallet hinzufügen

Übergibt können Wallet auf folgende Weise hinzugefügt werden:

* **Zwischen Apps** – diese nicht übergibt direkt bearbeitet werden, sie einfach Pass-Dateien laden und präsentieren Sie den Benutzer mit der Option Wallet hinzugefügt. 

* **Begleit-Apps** – diese werden von Anbietern zum Verteilen von übergibt und bieten zusätzliche Funktionen zum Durchsuchen oder bearbeitet werden geschrieben. Xamarin.iOS Anwendungen haben vollständigen Zugriff auf die übergeben Kit-API zur Erstellung und Bearbeitung von übergibt. Übergibt können anschließend hinzugefügt werden Wallet mithilfe der `PKAddPassesViewController`. Dieser Prozess wird ausführlich in die **Companion Anwendungen** Abschnitt dieses Dokuments.

### <a name="conduit-applications"></a>Kanal Anwendungen

Kanal Anwendungen sind intermediate apps, die möglicherweise übergibt im Namen eines Benutzers zu sollten ihre Inhaltstyp erkannt werden und Funktionalität der Wallet hinzuzufügende programmiert werden. Beispiele für Kanal-apps:

-   **Mail** – Anlage als eine Übergabe erkennt.
-   **Safari** – übergeben Content-Type erkennt, wenn ein URL übergeben Link geklickt wird.
-   **Andere benutzerdefinierte apps** – jeder app, die Anlagen empfangen, oder Öffnen von Links (social Media-Clients, e-Mail usw.).


Diese bildschirmabbildung zeigt wie **Mail** in iOS 6 eine Pass-Anlage erkennt und (sofern verwendet) bietet **hinzufügen** es Wallet.

 [![](passkit-images/image22.png "Diese bildschirmabbildung zeigt, wie iOS 6-e-Mail eine Pass-Anlage erkennt")](passkit-images/image22.png#lightbox)

 [![](passkit-images/image23.png "Diese bildschirmabbildung zeigt, wie Nachrichten bietet, um einen Pass-Anhang Wallet hinzufügen")](passkit-images/image23.png#lightbox)

Wenn Sie eine app erstellen, die ein Kanal für übergibt sein kann, können sie vom erkannt werden:

-  **Dateierweiterung** -.pkpass
-  **MIME-Typ** -application/vnd.apple.pkpass
-  **UTI** – com.apple.pkpass


Einer Anwendung zwischen der grundlegende Vorgang ist, rufen Sie die Pass-Datei, und rufen Sie übergeben Kit `PKAddPassesViewController` , Benutzern die Möglichkeit, ihre Wallet die erfolgreich hinzugefügt. Die Implementierung dieses View-Controller wird im nächsten Abschnitt finden Sie unter **Companion Anwendungen**.

Kanal Anwendungen müssen nicht für eine bestimmte übergeben Typ-ID auf die gleiche Weise bereitgestellt werden, die Begleit-Anwendungen führen.

## <a name="companion-applications"></a>Begleit-Anwendungen

Eine Begleit-Anwendung ermöglicht, zusätzliche Funktionen für die Arbeit mit übergibt, z. B. eine Übergabe erstellen, aktualisieren eine Übergabe zugeordnete Informationen aus, und andernfalls verwalten übergibt, der Anwendung zugeordnet sind.

Begleit-Anwendungen sollten nicht versuchen, die Funktionen von Wallet zu duplizieren. Sie dürfen nicht zum Scannen übergibt anzuzeigen.

Diese übrige Teil dieses Abschnitts wird beschrieben, wie zum Erstellen einer einfachen Begleit-App, die interagiert Kit übergeben wird.

### <a name="provisioning"></a>Bereitstellung

Da Wallet eine Store-Technologie ist, wird die Anwendung muss separat bereitgestellt werden und kann keine Team Provisioning-Profil "oder" Wildcard App ID. Finden Sie in der [arbeiten mit Funktionen](~/ios/deploy-test/provisioning/capabilities/wallet-capabilities.md) Handbuch, um eine eindeutige ID der App und Bereitstellungsprofil für die Wallet-Anwendung zu erstellen.

### <a name="entitlements"></a>Berechtigungen

Die **Entitlements.plist** Datei in aller letzten Xamarin.iOS Projekt eingeschlossen werden soll. Um eine neue Entitlements.plist Datei hinzuzufügen, führen Sie die Schritte in der [arbeiten mit Berechtigungen](~/ios/deploy-test/provisioning/entitlements.md) Handbuch.

Festzulegende Berechtigungen gehen Sie folgendermaßen vor:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Doppelklicken Sie auf die **Entitlements.plist** -Datei in der Projektmappe aufgefüllt, den Entitlements.plist-Editor zu öffnen:

![](passkit-images/image31.png "Entitlements.plst-editor")

Wählen Sie im Abschnitt Wallet der **aktivieren Wallet** Option

![](passkit-images/image32.png "Aktivieren Sie Wallet-Berechtigung")


Option "Default" ist für Ihre app zu ermöglichen, dass alle Typen übergeben werden. Allerdings ist es möglich, beschränken Ihre app und nur eine Teilmenge der Team-Pass-Typen zulassen. So aktivieren Sie dieses wählen die **zulassen Teilmenge des Teams Typen übergeben** , und geben Sie die Pass-Typ-ID für die Teilmenge, die Sie zulassen möchten.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Doppelklicken Sie auf die **Entitlements.plist** Datei, um die XML-Quelldatei zu öffnen.

Um die Wallet Ansprüche hinzuzufügen, legen die **Eigenschaft** auf `Passbook Identifiers` die wird automatisch festgelegt, in der Dropdownliste die **Typ** `Array`. Schalten Sie dann die Zeichenfolge **Wert** auf `$(TeamIdentifierPrefix)*`:

![](passkit-images/image33.png "Aktivieren Sie Wallet-Berechtigung")

So akzeptiert Ihre App alle Passtypen. Um Ihre app einschränken und nur eine Teilmenge der Team-Pass-Typen zulassen, legen Sie den String-Wert auf:

`$(TeamIdentifierPrefix)pass.$(CFBundleIdentifier)`

Wobei `pass.$(CFBundleIdentifier)` ist die ID mit übergeben, der erstellt wurde [oben](~/ios/platform/passkit.md)

-----

### <a name="debugging"></a>Debuggen

Bereitstellen der Anwendung Probleme auftreten, überprüfen Sie, ob Sie den richtigen verwenden **Bereitstellungsprofil** und der `Entitlements.plist` ausgewählt ist, als die **benutzerdefinierte Berechtigungen** -Datei in die **iPhone Bundle signieren** Optionen.

Wenn Sie diesen Fehler auftreten, bei der Bereitstellung:

```csharp
Installation failed: Your code signing/provisioning profiles are not correctly configured (error: 0xe8008016)
```

die `pass-type-identifiers` Berechtigungen Array ist falsch (oder entspricht nicht der **Bereitstellungsprofil**). Überprüfen Sie, ob die Typ-IDs, übergeben Sie und Ihr Team-ID richtig sind.

 <a name="Classes" />

## <a name="classes"></a>Klassen

Die folgenden übergeben Kit Klassen sind für apps übergibt den Zugriff auf verfügbar:

-  **PKPass** – eine Instanz von einem Durchlauf.
-  **PKPassLibrary** – stellt die API für den Zugriff auf die übergibt auf dem Gerät bereit.
-  **PKAddPassesViewController** – verwendet, um eine Übergabe für den Benutzer, deren Wallet speichern anzuzeigen.
-  **PKAddPassesViewControllerDelegate** – Xamarin.iOS-Entwickler


## <a name="example"></a>Beispiel

Verweisen Sie auf das Projekt PassLibrary im Beispielcode für diesen Artikel. Es veranschaulicht die folgenden allgemeinen Funktionen, die in einer Wallet Begleit-Anwendung erforderlich sind:

### <a name="check-that-wallet-is-available"></a>Überprüfen Sie, dass Wallet verfügbar ist.

Wallet ist nicht verfügbar, auf dem iPad, damit Anwendungen überprüfen soll, bevor Sie versuchen, den Zugriff auf Funktionen Kit übergeben.

```csharp
if (PKPassLibrary.IsAvailable) {
    // create an instance and do stuff...
}
```

### <a name="creating-a-pass-library-instance"></a>Erstellen einer Instanz der Pass-Bibliothek

Die Bibliothek übergeben Kit ist kein Singleton, Anwendungen erstellen und speichern und Instanz für den Zugriff auf die Kit-API übergeben.

```csharp
if (PKPassLibrary.IsAvailable) {
    library = new PKPassLibrary ();
    // do stuff...
}
```

### <a name="get-a-list-of-passes"></a>Abrufen einer Liste von Durchläufen

-Anwendungen können eine Liste von Durchläufen aus der Bibliothek anfordern. Diese Liste automatisch von Kit übergeben gefiltert, sodass Sie nur übergibt, die mit Ihrem Team-ID erstellt wurden und die aufgeführt sind, in Ihrer Berechtigungen anzeigen können.

```csharp
var passes = library.GetPasses ();  // returns PKPass[]
```

Beachten Sie, dass der Simulator die Liste der übergibt, die zurückgegeben wird, nicht filtern, damit diese Methode immer auf echten Geräten getestet werden soll. Diese Liste kann in einer UITableView, die Beispiel-app wie folgt aussehen angezeigt werden, nach zwei Coupons hinzugefügt wurden:

 [![](passkit-images/image29.png "Das Beispiel-app-aussehen wie folgt zwei Coupons hinzugefügt worden sind")](passkit-images/image29.png#lightbox)


### <a name="displaying-passes"></a>Anzeigen von übergibt

Eine begrenzte Anzahl von Informationen ist für das Rendering von Durchläufen in Begleit-apps verfügbar.

Wählen Sie Sie aus dieser Reihe von Standardeigenschaften anzuzeigenden Listen von Durchläufen, wie im Beispielcode an.

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

Diese Zeichenfolge wird als Warnung im Beispiel gezeigt:

 [![](passkit-images/image30.png "Die Warnung Coupons ausgewählt, in der Stichprobe")](passkit-images/image30.png#lightbox)

Sie können auch die `LocalizedValueForFieldKey()` Methode zum Abrufen von Daten aus Feldern in der übergibt, die Sie entwickelt haben (da Sie wissen, welche Felder sollten vorhanden sein). Im Beispielcode wird diese nicht angezeigt.

### <a name="loading-a-pass-from-a-file"></a>Laden eine Übergabe aus einer Datei

Da eine Übergabe nur Wallet mit Zustimmung des Benutzers hinzugefügt werden kann, muss ein modellansichtcontroller angezeigt werden, um entscheiden zu können. Dieser Code wird verwendet, der **hinzufügen** Schaltfläche im Beispiel verwenden, um eine vorgefertigte Durchlauf zu laden, die in der app eingebettet ist (Sie sollten dies durch eine ersetzen, die Sie sich angemeldet haben):

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

Die Übergabe erhält **hinzufügen** und **"Abbrechen"** Optionen:

 [![](passkit-images/image20.png "Die erfolgreich mit den Optionen hinzufügen und "Abbrechen" angezeigt")](passkit-images/image20.png#lightbox)

### <a name="replace-an-existing-pass"></a>Ersetzen Sie einen vorhandenen Durchlauf

Ersetzen eine vorhandene Pass erfordert nicht die Berechtigung des Benutzers, jedoch fehl, wenn die Übergabe nicht bereits vorhanden ist.

```csharp
if (library.Contains (newPass)) {
     library.Replace (newPass);
}
```

### <a name="editing-a-pass"></a>Bearbeiten eine Übergabe

PKPass nicht änderbar ist, damit Sie Pass-Objekte in Ihrem Code nicht aktualisieren können. Um die Daten in einem Durchlauf eine Anwendung zu ändern, muss auf einem Webserver zugreifen, die einen Datensatz von Durchläufen beibehalten und generieren eine neue Pass-Datei mit aktualisierten Werten, das die Anwendung herunterladen können können.

Pass-dateierstellung muss auf einem Server ausgeführt, da übergibt mit einem Zertifikat signiert werden müssen, die vertrauliche und sichere aufbewahrt werden müssen.

Nachdem eine aktualisierte Pass-Datei generiert wurde, verwenden Sie die `Replace` Methode, um die alten Daten auf dem Gerät zu überschreiben.

### <a name="display-a-pass-for-scanning"></a>Zeigt eine Übergabe zum Scannen

Wie bereits erwähnt, nur eine Übergabe Wallet zum Scannen angezeigt werden kann. Eine Übergabe kann angezeigt werden, mithilfe der `OpenUrl` Methode wie gezeigt:

 `UIApplication.SharedApplication.OpenUrl (p.PassUrl);`

### <a name="receiving-notifications-of-changes"></a>Empfangen von Benachrichtigungen von Änderungen

Anwendungen für Änderungen an der Bibliothek übergeben Lauschen können mithilfe der `PKPassLibraryDidChangeNotification`. Änderungen können durch Benachrichtigungen auslösen Updates im Hintergrund, daher ist es empfiehlt sich, die in Ihrer app Abhören verursacht werden.

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

Es ist wichtig, eine Bibliothek Instanz übergeben, wenn Sie für die Benachrichtigung registrieren, da PKPassLibrary kein Singleton ist.

## <a name="server-processing"></a>Verarbeitung

Eine detaillierte Erläuterung der Erstellung einer Server-Anwendung zur Unterstützung von übergeben Kit ist Gegenstand dieses Artikels einführende.

Der .NET Code bereitgestellt, der *Signpassnet* Beispiel könnte als Grundlage für eine serverseitige-Methode, die übergibt generieren kann verwendet werden.

Ansicht [WWDC Video: Einführung in Passbook, Teil 2](https://developer.apple.com/videos/wwdc/2012/?include=309#309) von 27:00 Minuten für Weitere Informationen.

### <a name="other-resources"></a>Weitere Ressourcen

Finden Sie unter [Dotnet Passbook](https://github.com/tomasmcguinness/dotnet-passbook) Quellcode c# serverseitige zu öffnen.

## <a name="push-notifications"></a>Pushbenachrichtigungen

Eine detaillierte Erläuterung der Verwendung von Pushbenachrichtigungen übergibt aktualisieren ist Gegenstand dieses Artikels einführende.

Sie ist erforderlich, implementiert die REST-ähnliche-API, die von Apple zu webanforderungen aus Wallet reagieren, wenn Updates erforderlich sind, definiert wird. Der .NET Code bereitgestellt, der *Signpassnet* Beispiel kann als Grundlage für das Erstellen einer neuen übergibt als Ergebnis dieser Anforderungen verwendet werden.

Ansicht [WWDC Video: Einführung in Passbook, Teil 2](https://developer.apple.com/videos/wwdc/2012/?include=309#309) von 27:00 Minuten für Weitere Informationen.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel eingeführt Kit übergeben, einige der Gründe, warum sie nützlich ist, und beschriebenen die verschiedenen Teile, die für eine vollständige übergeben Kit-Lösung implementiert werden müssen. Es erläutert die erforderlichen Schritte zum Konfigurieren von Apple Developer für Ihr Konto übergibt, der Prozess auf einer Übergabe manuell, und auch die APIs für übergeben Kit aus einem Xamarin.iOS-Anwendung Zugriff auf Benutzer zu erstellen.

## <a name="related-links"></a>Verwandte Links

- [CreateAPassManually (Beispiel)](https://developer.xamarin.com/samples/PassKit/)
- [PassKit-Beispiel](https://developer.xamarin.com/samples/monotouch/PassKit/)
- [Einführung in iOS 6](~/ios/platform/introduction-to-ios6/index.md)
- [Passbook Programmierhandbuch](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Conceptual/PassKit_PG/Chapters/Introduction.html)
- [Passbook für Entwickler](https://developer.apple.com/passbook/)
- [Informationen zu Pass-Dateien](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Bundle/Chapters/Introduction.html)
- [Übergeben Sie Kit Frameworkverweis](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Framework/_index.html)
- [Übergeben Sie Kit Frameworkverweis](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Framework/_index.html)
- [Passbook Webdienstreferenz](https://developer.apple.com/library/prerelease/ios/#documentation/PassKit/Reference/PassKit_WebService/WebService.html)
