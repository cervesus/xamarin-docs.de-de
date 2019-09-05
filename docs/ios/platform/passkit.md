---
title: Passkit in xamarin. IOS
description: Die Wallet-App ermöglicht IOS-Benutzern das Speichern digitaler Pass auf Ihren Geräten. Das passkit-Framework ermöglicht Entwicklern die programmgesteuerte Interaktion mit Durchläufen.
ms.prod: xamarin
ms.assetid: 74B9973B-C1E8-B727-3F6D-59C1F98BAB3A
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 06/13/2018
ms.openlocfilehash: 8039482175465a67867f3c70f17518dee8b9500b
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70277868"
---
# <a name="passkit-in-xamarinios"></a>Passkit in xamarin. IOS

Die IOS Wallet-App ermöglicht Benutzern das Speichern digitaler Pass auf Ihren Geräten.
Diese durch Führungen werden von Händlern generiert und per e-Mail, URLs oder über die eigene IOS-App des Händlers an den Kunden gesendet. Diese Durchgänge können verschiedene Dinge darstellen, von Film Tickets bis zu Treuekarten bis zum Boarding von Durchläufen. Das passkit-Framework ermöglicht Entwicklern die programmgesteuerte Interaktion mit Durchläufen.

Dieses Dokument enthält eine Einführung in Wallet und die Verwendung der passkit-API mit xamarin. IOS.

 [![](passkit-images/image1.png "Die Geldbörse speichert und organisiert alle Pass-ins auf einem Telefon.")](passkit-images/image1.png#lightbox)

## <a name="requirements"></a>Anforderungen

Die in diesem Dokument erläuterten passkit-Features erfordern IOS 6 und Xcode 4,5 zusammen mit xamarin. IOS 6,0.

## <a name="introduction"></a>Einführung

Das Hauptproblem, das von passkit gelöst wird, ist die Verteilung und Verwaltung von Barcodes. Im folgenden finden Sie einige Beispiele für die Verwendung von Barcodes in der Praxis:

- **Kaufen von Movie Tickets Online** – Kunden erhalten in der Regel einen Barcode, der Ihre Tickets darstellt. Dieser Barcode wird gedruckt und in das Kino aufgenommen, um auf den Eintrag zu suchen.
- **Treuekarten** – Kunden haben eine Reihe von unterschiedlichen Speicher spezifischen Karten in Ihre Geldbörse oder Tasche, um Sie beim Kauf von waren anzuzeigen und zu überprüfen.
- **Coupons** – Coupons werden per e-Mail, als Druck Bare Webseiten, über Brief Fächer und als Barcodes in Zeitungen und Zeitschriften verteilt. Kunden werden Sie in einen Store zum Scannen bringen, um waren, Dienste oder Rabatte im Gegenzug zu erhalten.
- Das **Boarding geht** – ähnlich wie beim Kauf eines Film Tickets.

Passkit bietet eine Alternative für jedes dieser Szenarien:

- **Movie Tickets** – nach dem Kauf fügt der Kunde einen Ereignis Ticket Pass (per e-Mail oder über einen Link zur Website) hinzu. In der Zeit für den Film wird der Durchlauf automatisch als Erinnerung auf dem Sperrbildschirm angezeigt, und bei der Ankunft im Kino wird der Durchlauf problemlos abgerufen und zum Scannen in Wallet angezeigt.
- **Treuekarten** – anstatt (oder zusätzlich zu), die eine physische Karte bereitstellen, können Filialen (per e-Mail oder nach einer Website Anmeldung) einen Pass Karten Pass ausgeben. Der Speicher kann zusätzliche Features bereitstellen, wie z. b. das Aktualisieren des Saldo des Kontos während des Pass-Through-Pushbenachrichtigungen, und die Verwendung von geolozierungsdiensten der Pass könnte automatisch auf dem Sperrbildschirm angezeigt werden, wenn sich der Kunde in der Nähe eines Speicher Orts befindet
- **Coupons** – Coupon Pässe können problemlos mit eindeutigen Merkmalen generiert werden, um Sie bei der Nachverfolgung und Verteilung per e-Mail oder Website Links zu unterstützen. Heruntergeladene Coupons können automatisch auf dem Sperrbildschirm angezeigt werden, wenn sich der Benutzer an einem bestimmten Speicherort befindet, und/oder an einem bestimmten Datum (z. b. wenn sich das Ablaufdatum nähert). Da die Coupons auf dem Telefon des Benutzers gespeichert sind, sind Sie immer praktisch und werden nicht falsch platziert. Gutscheine können Kunden ermutigen, Begleit-apps herunterzuladen, da App Store-Links in den Durchlauf integriert werden können, wodurch sich die Einbindung mit dem Kunden erhöht.
- Das **Onboarding** – nach einem Online Eincheck Vorgang erhält der Kunde seine Einstiegs Pass per e-Mail oder über einen Link. Eine Begleit-APP, die vom Transportanbieter bereitgestellt wird, kann den Eincheck Vorgang einschließen und es dem Kunden ermöglichen, zusätzliche Funktionen wie die Auswahl seines Arbeitsplatzes oder seiner Mahlzeit auszuführen. Der Transportanbieter kann Pushbenachrichtigungen verwenden, um den Durchlauf zu aktualisieren, wenn der Transport verzögert oder abgebrochen wird. Wenn die Boardingzeit erreicht wird, wird der Durchlauf auf dem Sperrbildschirm als Erinnerung angezeigt und ermöglicht einen schnellen Zugriff auf den Durchlauf.

Im Kern bietet passkit eine einfache und bequeme Möglichkeit zum Speichern und Anzeigen von Barcodes auf Ihrem IOS-Gerät. Dank der zusätzlichen Zeit-und Speicherort-Lock-Screen-Integration bietet Pushbenachrichtigungen und die Integration von Begleit Anwendungen eine Grundlage für sehr ausgereifte Vertriebs-, Ticket-und Abrechnungsdienste.

## <a name="passkit-ecosystem"></a>Passkit-Ökosystem

Passkit ist nicht nur eine API in cocoatouch, sondern gehört zu einem größeren Ökosystem von apps, Daten und Diensten, die das sichere freigeben und Verwalten von Barcodes und anderen Daten erleichtern. Dieses allgemeine Diagramm zeigt die verschiedenen Entitäten, die an der Erstellung und Verwendung von Durchläufen beteiligt sein können:

 [![](passkit-images/image2.png "Dieses allgemeine Diagramm zeigt die Entitäten, die beim Erstellen und Verwenden von Durchläufen beteiligt sind.")](passkit-images/image2.png#lightbox)

Alle Teil des Ökosystems verfügen über eine eindeutig definierte Rolle:

- **Wallet** – die integrierte IOS-APP von Apple, die Pass-Stores speichert und anzeigt. Dies ist die einzige Stelle, an der Pass-Through zur Verwendung in der realen Welt gerendert werden (d. h., der Barcode wird zusammen mit allen lokalisierten Daten im Durchlauf angezeigt).
- **Begleitende apps** – IOS 6-apps, die von den Pass Anbietern erstellt werden, um die Funktionalität der von Ihnen bereitzustellen, z. b. das Hinzufügen eines Werts zu einer Geschäfts Karte, das Ändern des Arbeitsplatzes bei einem boardingdurchlauf oder einer anderen Geschäfts spezifischen Funktion Begleit-apps sind nicht erforderlich, damit ein Pass nützlich ist.
- **Der Server** – ein sicherer Server, auf dem Durchgänge generiert und für die Verteilung signiert werden kann. Ihre begleitende App kann eine Verbindung mit Ihrem Server herstellen, um neue Pass-oder Anforderungs Updates für vorhandene Pass-ins zu generieren. Sie können optional die Webdienst-API implementieren, die von Wallet zum Aktualisieren von Durchläufen aufgerufen wird.
- **APNs-Server** – der Server hat die Möglichkeit, Wallet über Updates an einen Pass auf einem bestimmten Gerät mithilfe von APNs zu benachrichtigen. Pushen Sie eine Benachrichtigung an Wallet, um Details zur Änderung an Ihren Server zu wenden. Begleit Anwendungen müssen keine APNs für dieses Feature implementieren (Sie können auf `PKPassLibraryDidChangeNotification` lauschen).
- **Kanal-apps** – Anwendungen, die keine direkte Bearbeitung (wie begleitende Apps) ausführen, jedoch das Hilfsprogramm verbessern können, indem Sie "Pass" erkennen und zulassen, dass Sie in Wallet eingefügt werden. E-Mail-Clients, Netzwerke für soziale Netzwerke und andere Datenaggregations-Apps können alle Anlagen oder Verknüpfungen zu Durchläufen haben.

Das gesamte Ökosystem sieht Komplex aus. es ist also erwähnenswert, dass einige Komponenten optional sind und viel einfachere passkit-Implementierungen möglich sind.

## <a name="what-is-a-pass"></a>Was ist ein Durchlauf?

Ein Durchlauf ist eine Sammlung von Daten, die ein Ticket, einen Coupon oder eine Karte darstellen. Sie kann für eine einzelne Verwendung durch eine Einzelperson (und somit auch für eine Flugnummer und eine Arbeitsplatz Zuweisung) vorgesehen sein, oder es kann sich um ein Token mit mehreren verwendetem Token handeln, das von einer beliebigen Anzahl von Benutzern (z. b. einem Rabattgutschein) gemeinsam genutzt werden kann. Eine ausführliche Beschreibung finden Sie im Dokument [über Pass Files von](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Bundle/Chapters/Introduction.html) Apple.

### <a name="types"></a>Typen

Derzeit werden fünf unterstützte Typen unterstützt, die in der Wallet-app durch Layout und oberen Rand des Durchlauf unterschieden werden können:

- **Ereignis Ticket** – kleiner semizirkel Ausschnitt.
- Das **Boarding von Pass** – gibt an, dass ein Transport spezifisches Symbol angegeben werden kann (z. b. Bus, Train, Flugzeug).
- **Store Card** – abgerundet, wie z. b. eine Kredit-oder Debitkarte.
- **Coupon** – am oberen Rand.
- **Generisch** – identisch mit der Speicherkarte, gerundet.


Die fünf Pass Typen sind in diesem Screenshot dargestellt (in der folgenden Reihenfolge: Coupon, generisch, Store Card, Boarding Pass und Ereignis Ticket):

 [![](passkit-images/image3.png "Die fünf Pass Typen sind in diesem Screenshot dargestellt.")](passkit-images/image3.png#lightbox)

### <a name="file-structure"></a>Dateistruktur

Eine Pass Datei ist tatsächlich ein ZIP-Archiv mit der Erweiterung **. pkpass** , das einige bestimmte JSON-Dateien (erforderlich), eine Vielzahl von Bilddateien (optional) und lokalisierte Zeichen folgen (auch optional) enthält.

- " **Pass. JSON** " – erforderlich. Enthält alle Informationen für den Durchlauf.
- **Manifest. JSON** – erforderlich. Enthält SHA1-Hashes für jede Datei im Durchlauf außer der Signatur Datei und dieser Datei (Manifest. Json).
- **Signatur** – erforderlich. Wird erstellt, indem `manifest.json` die Datei mit dem Zertifikat signiert wird, das im IOS-Bereitstellungs Portal generiert wurde.
- **Logo. png** – optional.
- **Background. png** – optional.
- **Icon. png** – optional.
- **Lokalisier** bare Zeichen folgen Dateien – optional.

Die Verzeichnisstruktur einer Pass Datei ist unten dargestellt (Dies ist der Inhalt des ZIP-Archivs):

 [![](passkit-images/image4.png "Hier wird die Verzeichnisstruktur einer Pass Datei angezeigt.")](passkit-images/image4.png#lightbox)

### <a name="passjson"></a>"Pass. JSON"

JSON ist das Format, da Pass-in der Regel auf einem Server erstellt werden – dies bedeutet, dass der Generierungs Code auf dem Server plattformunabhängig ist. Die drei wichtigsten Informationen in jedem Durchlauf sind:

- **teamidentifier** – Hiermit werden alle von Ihnen an Ihr App Store-Konto generierten weitergeleitet. Dieser Wert ist im IOS-Bereitstellungs Portal sichtbar.
- **passtypeidentifier** – registrieren Sie sich im Bereitstellungs Portal, um Pass-by-Gruppen zu gruppieren (wenn Sie mehr als einen Typ entwickeln). Beispielsweise könnte ein Coffee Store einen Pass-Card-Passtyp erstellen, damit Ihre Kunden Treue Gutschriften erhalten können, aber auch einen separaten Coupon passentyp zum Erstellen und Verteilen von Rabattcoupons. Der gleiche Kaffeehandel hält sogar Live Musikereignisse und gibt Ereignis Ticket Pass für diese aus.
- **serialNumber** – eine eindeutige Zeichenfolge in `passTypeidentifier` diesem. Der Wert ist für Wallet nicht transparent, aber wichtig für die Nachverfolgung spezifischer Durchgänge bei der Kommunikation mit dem Server.

Es gibt eine große Anzahl von anderen JSON-Schlüsseln in jedem Durchlauf, ein Beispiel, das unten dargestellt wird:

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

Nur 2D-Formate werden unterstützt: PDF417, Aztec, QR. Apple beansprucht, dass 1D-Barcodes nicht für die Überprüfung auf einem Bildschirm mit Hintergrundbeleuchtung geeignet sind.

Alternativer Text, der unterhalb des Barcodes angezeigt wird, ist optional – einige Händler möchten manuell lesen/eingeben können.

ISO-8859-1-Codierung ist am häufigsten. Überprüfen Sie, welche Codierung von den Überprüfungs Systemen verwendet wird, die ihre Durchgänge lesen.

### <a name="relevancy-lock-screen"></a>Relevanz (Sperrbildschirm)

Es gibt zwei Arten von Daten, die dazu führen können, dass ein Durchlauf auf dem Sperrbildschirm angezeigt wird:

 **Speicherort**

Es können bis zu 10 Standorte in einem Durchlauf angegeben werden, z. b. für einen Kunden, der häufig besucht wird, oder für den Standort eines Kinos oder eines Flughafen. Ein Kunde könnte diese Standorte über eine begleitende App festlegen, oder der Anbieter könnte Sie anhand von Verwendungs Daten ermitteln (bei Erfassung mit der Berechtigung des Kunden).

Wenn der Durchlauf auf dem Sperrbildschirm angezeigt wird, wird ein Fence berechnet, sodass der Durchlauf im Sperrbildschirm ausgeblendet wird, wenn der Benutzer den Bereich verlässt. Der RADIUS ist an den Durchlauf Stil gebunden, um Missbrauch zu verhindern.

 **Datum und Uhrzeit**

In einem Durchlauf kann nur ein Datum/eine Uhrzeit angegeben werden. Das Datum und die Uhrzeit sind nützlich, um Sperrbildschirm Erinnerungen für das Boarding von Durchläufen und Ereignis Tickets auszulösen.

Kann über Push oder über die passkit-API aktualisiert werden, damit das Datum/die Uhrzeit bei einem mehrfach Verwendungs Ticket (z. b. ein Saisonticket zu einem Theater oder Sportkomplex) aktualisiert werden kann.

### <a name="localization"></a>Lokalisierung

Das Übersetzen eines Pass-Through in mehrere Sprachen ähnelt dem Lokalisieren einer IOS-Anwendung – erstellen Sie sprach `.lproj` spezifische Verzeichnisse mit der Erweiterung, und platzieren Sie die lokalisierten Elemente in. Text Übersetzungen sollten in eine `pass.strings` Datei eingegeben werden, während lokalisierte Bilder den gleichen Namen wie das Image aufweisen, das Sie im Durchlauf Stamm ersetzen.

## <a name="security"></a>Sicherheit

Pässe werden mit einem privaten Zertifikat signiert, das Sie im IOS-Bereitstellungs Portal generieren. Die Schritte zum Signieren des bestanden lauten wie folgt:

1. Berechnen Sie einen SHA1-Hash für jede Datei im Pass-Verzeichnis (schließen Sie `manifest.json` weder `signature` die-Datei noch die-Datei ein, die in dieser Phase überhaupt nicht vorhanden sein sollte).
1. Schreiben `manifest.json` Sie als JSON-Schlüssel-Wert-Liste jedes Datei namens mit dem Hashwert.
1. Verwenden Sie das Zertifikat, um `manifest.json` die Datei zu signieren, und schreiben Sie `signature` das Ergebnis in eine Datei namens.
1. Packen Sie alles nach oben, und übergeben Sie der `.pkpass` resultierenden Datei eine Dateierweiterung.


Da der private Schlüssel erforderlich ist, um den Durchlauf zu signieren, sollte dieser Vorgang nur auf einem sicheren Server durchgeführt werden, den Sie steuern. Verteilen Sie Ihre Schlüssel nicht zum Testen und Generieren von Durchläufen in einer Anwendung.

 
## <a name="configuration-and-setup"></a>Konfiguration und Setup

Dieser Abschnitt enthält Anweisungen, mit denen Sie Ihre Bereitstellungs Details einrichten und ihren ersten Durchlauf erstellen können.

### <a name="provisioning-passkit"></a>Bereitstellen von passkit

Damit ein Pass in den App Store eingegeben werden kann, muss er mit einem Entwicklerkonto verknüpft werden. Hierfür sind zwei Schritte erforderlich:

1. Der Durchlauf muss mithilfe eines eindeutigen Bezeichners registriert werden, der als Passtyp-ID bezeichnet wird.
1. Ein gültiges Zertifikat muss generiert werden, um den Durchlauf mit der digitalen Signatur des Entwicklers zu signieren.

Gehen Sie folgendermaßen vor, um eine Passtyp-ID zu erstellen.

#### <a name="create-a-pass-type-id"></a>Erstellen einer Passtyp-ID

Der erste Schritt besteht darin, eine Passtyp-ID für jeden unterstützten _Typ_ von Durchlauf einzurichten. Die Pass-ID (oder der verlauftyp Bezeichner) erstellt einen eindeutigen Bezeichner für den Durchlauf. Wir verwenden diese ID, um den Pass mit Ihrem Entwicklerkonto mithilfe eines Zertifikats zu verknüpfen.

1. Navigieren Sie im [IOS-Bereitstellungs Portal im Abschnitt Zertifikate, Bezeichner und Profile](https://developer.apple.com/account/overview.action)zu **Identifizierer** , und wählen Sie **Passtyp-IDs** aus. Wählen Sie dann **+** die Schaltfläche aus, um einen neuen Passtyp zu erstellen: [![](passkit-images/passid.png "Neuen Passtyp erstellen")](passkit-images/passid.png#lightbox)

2. Geben Sie eine **Beschreibung** (Name) und einen **Bezeichner** (eindeutige Zeichenfolge) für den Durchlauf an. Beachten Sie, dass alle Pass-Type-IDs mit `pass.` der Zeichenfolge beginnen müssen `pass.com.xamarin.coupon.banana` , die wir in diesem Beispiel verwenden: [![](passkit-images/register.png "Angeben einer Beschreibung und eines Bezeichners")](passkit-images/register.png#lightbox)


3. Bestätigen Sie die Pass-ID durch Drücken der Schaltfläche **registrieren** .

#### <a name="generate-a-certificate"></a>Generieren eines Zertifikats

Gehen Sie folgendermaßen vor, um ein neues Zertifikat für diese Passtyp-ID zu erstellen:

1. Wählen Sie die neu erstellte Pass-ID aus der Liste aus, und klicken Sie auf **Bearbeiten** : [![](passkit-images/pass-done.png "Wählen Sie die neue Pass-ID aus der Liste aus.")](passkit-images/pass-done.png#lightbox)

    Klicken Sie dann auf **Zertifikat erstellen...** :

    [![](passkit-images/cert-dist.png "Wählen Sie Zertifikat erstellen.")](passkit-images/cert-dist.png#lightbox)


2. Befolgen Sie die Schritte zum Erstellen einer Zertifikat Signier Anforderung (Certificate Signing Request, CSR).
  
3. Klicken Sie im Entwickler Portal auf die Schaltfläche **weiter** , und laden Sie die CSR hoch, um Ihr Zertifikat zu generieren.

4. Laden Sie das Zertifikat herunter, und doppelklicken Sie darauf, um es in ihrer Keychain zu installieren.


Nachdem wir nun ein Zertifikat für diese Passtyp-ID erstellt haben, wird im nächsten Abschnitt beschrieben, wie ein Pass manuell erstellt wird.

Weitere Informationen zur Bereitstellung für Wallet finden Sie im Handbuch [Arbeiten mit Funktionen](~/ios/deploy-test/provisioning/capabilities/wallet-capabilities.md) .

### <a name="create-a-pass-manually"></a>Manuelles Erstellen eines Pass-Through

Nachdem wir den Passtyp erstellt haben, können wir manuell einen Durchlauf zum Testen auf dem Simulator oder auf einem Gerät erstellen. Die Schritte zum Erstellen eines bestanden sind:

- Erstellen Sie ein Verzeichnis, in dem die Pass Dateien enthalten sein sollen.
- Erstellen Sie eine Datei "Pass. JSON", die alle erforderlichen Daten enthält.
- Schließt Bilder in den Ordner ein (falls erforderlich).
- Berechnen Sie SHA1-Hashes für jede Datei im Ordner, und schreiben Sie Sie in "Manifest. JSON".
- Signieren Sie die Datei Manifest. JSON mit der heruntergeladenen Datei Certificate. P12.
- Zippen Sie den Inhalt des Verzeichnisses, und benennen Sie ihn mit der Erweiterung. pkpass um


Im [Beispielcode](https://docs.microsoft.com/samples/xamarin/ios-samples/passkit) für diesen Artikel sind einige Quelldateien vorhanden, die verwendet werden können, um einen Durchlauf zu generieren. Verwenden Sie die Dateien im `CouponBanana.raw` Verzeichnis des Verzeichnisses "kreateapassmanual". Die folgenden Dateien sind vorhanden:

 [![](passkit-images/image18.png "Diese Dateien sind vorhanden.")](passkit-images/image18.png#lightbox)

Öffnen Sie "Pass. JSON", und bearbeiten Sie die JSON. Sie müssen mindestens den `passTypeIdentifier` und entsprechend Ihrem Apple- `teamIdentifer` Entwicklerkonto aktualisieren.

```csharp
"passTypeIdentifier" : "pass.com.xamarin.coupon.banana",
"teamIdentifier" : "?????????",
```

Anschließend müssen Sie die Hashes für jede Datei berechnen und die `manifest.json` Datei erstellen. Dies sieht in etwa wie folgt aus:

```csharp
{
  "icon@2x.png" : "30806547dcc6ee084a90210e2dc042d5d7d92a41",
  "icon.png" : "87e9ffb203beb2cce5de76113f8e9503aeab6ecc",
  "pass.json" : "c83cd1441c17ecc6c5911bae530d54500f57d9eb",
  "logo.png" : "b3cd8a488b0674ef4e7d941d5edbb4b5b0e6823f",
  "logo@2x.png" : "3ccd214765507f9eab7244acc54cc4ac733baf87"
}
```

Als nächstes muss eine Signatur für diese Datei mit dem Zertifikat (P12-Datei) generiert werden, das für diese Passtyp-ID generiert wurde.

#### <a name="signing-on-a-mac"></a>Signieren auf einem Mac

Laden Sie die **Support Materialien für Wallet-Seed** von der Apple- [Download](https://developer.apple.com/downloads/index.action?name=Passbook) Website herunter. Verwenden Sie `signpass` das Tool, um den Ordner in einen Durchlauf umzuwandeln (Dadurch werden auch die SHA1-Hashwerte berechnet und die Ausgabe in eine pkpass-Datei gezippt).

#### <a name="testing"></a>Test

Wenn Sie die Ausgabe dieser Tools untersuchen (indem Sie den Dateinamen auf. zip festlegen und ihn dann öffnen), werden die folgenden Dateien angezeigt (Beachten Sie das Hinzufügen `manifest.json` der Dateien und `signature` ):

 [![](passkit-images/image19.png "Überprüfen der Ausgabe dieser Tools")](passkit-images/image19.png#lightbox)

Nachdem Sie die Datei signiert, gezippt und umbenannt haben (z. b. zu `BananaCoupon.pkpass`) Sie können Sie in den Simulator ziehen, um Sie zu testen, oder per e-Mail an sich selbst, um Sie auf einem echten Gerät abzurufen. Sie sollten einen Bildschirm sehen, um den Durchlauf **hinzuzufügen** , wie folgt:

 [![](passkit-images/image20.png "Hinzufügen des Pass-Bildschirms")](passkit-images/image20.png#lightbox)

Normalerweise ist dieser Prozess auf einem Server automatisiert, aber die manuelle Pass-Erstellung ist möglicherweise eine Option für kleine Unternehmen, die nur Gutscheine erstellen, die nicht die Unterstützung eines Back-End-Servers erfordern.

## <a name="wallet"></a>Wallet

Wallet ist der zentrale Bestandteil des passkit-Ökosystems. Dieser Screenshot zeigt die leere Brieftasche und die Art und Weise, wie die Pass Liste und die einzelnen Durchgänge aussehen:

 [![](passkit-images/image21.png "Dieser Screenshot zeigt die leere Brieftasche und die Art und Weise, wie die Pass Liste und die einzelnen Durchgänge aussehen")](passkit-images/image21.png#lightbox)

Zu den Features von Wallet gehören:

- Es ist die einzige Stelle, an der die Überprüfungen mit Ihrem Barcode zum Scannen gerendert werden.
- Der Benutzer kann die Einstellungen für Updates ändern. Wenn diese Option aktiviert ist, können Pushbenachrichtigungen Updates der Daten im Durchlauf auslöst.
- Der Benutzer kann die Integration per Lock-Screen aktivieren oder deaktivieren. Wenn diese Option aktiviert ist, kann der Durchlauf automatisch auf dem Sperrbildschirm angezeigt werden, basierend auf relevanten Zeit-und Positionsdaten, die in den Durchlauf eingebettet sind.
- Die umgekehrte Seite des Pass-to-refresh-Vorgangs unterstützt Pull-to-refresh, wenn eine Web-Server-URL im Pass-JSON-Code bereitgestellt wird.
- Begleit-Apps können geöffnet (oder heruntergeladen) werden, wenn die APP-ID im Pass-JSON-Code bereitgestellt wird.
- Durchgänge können gelöscht werden (mit einer süßen Animation).

## <a name="adding-passes-into-wallet"></a>Hinzufügen von Pass in Wallet

Pässe können auf folgende Weise zu Wallet hinzugefügt werden:

- **Kanal-apps** – diese werden nicht direkt übermittelt, Sie laden einfach Pass Dateien und stellen dem Benutzer die Möglichkeit, Sie der Wallet hinzuzufügen. 

- **Begleit Anwendungen** – diese werden von Anbietern geschrieben, um Pass-und-Funktionen zu verteilen und zusätzliche Funktionen zum Durchsuchen oder bearbeiten zu bieten. Xamarin. IOS-Anwendungen verfügen über vollständigen Zugriff auf die passkit-API zum Erstellen und Bearbeiten von Durchläufen. Durch Pass können dann mithilfe von der `PKAddPassesViewController`Wallet hinzugefügt werden. Dieser Vorgang wird im Abschnitt " **begleitende Anwendungen** " dieses Dokuments ausführlicher beschrieben.

### <a name="conduit-applications"></a>Kanalanwendungen

Bei Kanal Anwendungen handelt es sich um zwischengeschaltete apps, die im Auftrag eines Benutzers weitergeleitet werden und so programmiert werden müssen, dass Sie Ihren Inhaltstyp erkennen und Funktionen bereitstellen, die der Wallet hinzugefügt werden. Beispiele für Kanal-apps sind:

- **Mail** – erkennt Anlage als Pass.
- **Safari** – erkennt den Pass Content-Type, wenn auf einen Pass-URL-Link geklickt wird.
- **Andere benutzerdefinierte apps** – jede APP, die Anlagen oder offene Links (Social Media-Clients, e-Mail-Reader usw.) empfängt.


Dieser Screenshot zeigt, wie **e-Mail** in ios 6 eine Pass-Anlage erkennt und (wenn Sie berührt) Angebote zum **Hinzufügen zur Wallet hinzufügen** können.

 [![](passkit-images/image22.png "Dieser Screenshot zeigt, wie e-Mail in ios 6 eine Pass Anlage erkennt.")](passkit-images/image22.png#lightbox)

 [![](passkit-images/image23.png "Dieser Screenshot zeigt, wie e-Mail Angebote zum Hinzufügen einer Pass Anlage zu Wallet anbietet.")](passkit-images/image23.png#lightbox)

Wenn Sie eine App entwickeln, die möglicherweise ein Kanal für Durchgänge ist, können Sie wie folgt erkannt werden:

- **Dateierweiterung** -. pkpass
- **MIME-Typ** -application/vnd. Apple. pkpass
- **UTI** – com. Apple. pkpass


Der grundlegende Vorgang einer Kanal Anwendung besteht darin, die Pass- `PKAddPassesViewController` Datei abzurufen und passkit aufzurufen, um dem Benutzer die Möglichkeit zu geben, den Pass der Geldbörse hinzuzufügen. Die Implementierung dieses Ansichts Controllers wird im nächsten Abschnitt zu begleitenden **Anwendungen**behandelt.

Leitungs Anwendungen müssen nicht auf die gleiche Weise für eine bestimmte Passtyp-ID bereitgestellt werden wie begleitende Anwendungen.

## <a name="companion-applications"></a>Begleit Anwendungen

Eine Begleit Anwendung bietet zusätzliche Funktionen zum Arbeiten mit Durchläufen, einschließlich der Erstellung eines Durchlaufs, dem Aktualisieren von Informationen, die mit einem Durchlauf verknüpft sind, und der sonstigen Verwaltung von mit der Anwendung verknüpften

Begleit Anwendungen sollten nicht versuchen, die Features von Wallet zu duplizieren. Sie sind nicht zum Anzeigen von Durchläufen zum Scannen vorgesehen.

In diesem Rest dieses Abschnitts wird beschrieben, wie Sie eine grundlegende Begleit-app erstellen, die mit passkit interagiert.

### <a name="provisioning"></a>Bereitstellung

Da Wallet eine Store-Technologie ist, muss die Anwendung separat bereitgestellt werden, und das Team Bereitstellungs Profil oder die Platzhalter-APP-ID kann nicht verwendet werden. Informationen zum Erstellen einer eindeutigen APP-ID und eines Bereitstellungs Profils für die Wallet-Anwendung finden Sie im Handbuch [Arbeiten mit Funktionen](~/ios/deploy-test/provisioning/capabilities/wallet-capabilities.md) .

### <a name="entitlements"></a>Berechtigten

Die Datei " **Berechtigungen. plist** " sollte in allen aktuellen xamarin. IOS-Projekten enthalten sein. Führen Sie die Schritte im Handbuch [Arbeiten mit Berechtigungen](~/ios/deploy-test/provisioning/entitlements.md) aus, um eine neue Datei "Berechtigungen. plist" hinzuzufügen.

Gehen Sie folgendermaßen vor, um Berechtigungen festzulegen:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Doppelklicken Sie auf die Datei " **Berechtigungen. plist** " im Lösungspad, um den Berechtigungs Listen-Editor zu öffnen:

![](passkit-images/image31.png "Berechtigungen. plst-Editor")

Wählen Sie im Abschnitt Wallet die Option **Wallet aktivieren** aus.

![](passkit-images/image32.png "Wallet-Berechtigung aktivieren")


Die Standardoption ist, dass Ihre APP alle Pass Typen zulässt. Es ist jedoch möglich, Ihre APP einzuschränken und nur eine Teilmenge von Team Pass Typen zuzulassen. Um dies zu aktivieren, wählen Sie die Option **Teilmenge von Team Pass Typen zulassen** aus, und geben Sie den passentyp Bezeichner der Teilmenge ein, die Sie zulassen möchten.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Doppelklicken Sie auf die Datei " **Berechtigungen. plist** ", um die XML-Quelldatei zu öffnen.

Um die Wallet-Berechtigung hinzuzufügen, legen Sie `Passbook Identifiers` die-Eigenschaft in der Dropdown Liste auf fest, wodurch der **Typ** `Array`automatisch festgelegt wird. Legen Sie dann den Zeichen folgen Wert `$(TeamIdentifierPrefix)*`auf fest:

![](passkit-images/image33.png "Wallet-Berechtigung aktivieren")

So akzeptiert Ihre App alle Passtypen. Um Ihre APP einzuschränken und nur eine Teilmenge von Team Pass Typen zuzulassen, legen Sie den Zeichen folgen Wert auf fest:

`$(TeamIdentifierPrefix)pass.$(CFBundleIdentifier)`

Hierbei entspricht `pass.$(CFBundleIdentifier)` der Pass-ID, die [zuvor](~/ios/platform/passkit.md) erstellt wurde.

-----

### <a name="debugging"></a>Debuggen

Wenn bei der Bereitstellung der Anwendung Probleme auftreten, überprüfen Sie, ob Sie das richtige **Bereitstellungs Profil** verwenden und ob in den `Entitlements.plist` **iPhone Bundle-Signierungs** Optionen als **benutzerdefinierte** Berechtigungsdatei ausgewählt ist.

Wenn bei der Bereitstellung dieser Fehler auftritt:

```csharp
Installation failed: Your code signing/provisioning profiles are not correctly configured (error: 0xe8008016)
```

dann ist `pass-type-identifiers` das Berechtigungs Array falsch (oder stimmt nicht mit dem **Bereitstellungs Profil**). Überprüfen Sie, ob die Passtyp-IDs und Ihre Team-ID korrekt sind.

## <a name="classes"></a>Klassen

Die folgenden passkit-Klassen sind für apps verfügbar, die auf Durchläufen zugreifen:

- **Pkpass** – eine Instanz eines bestanden.
- **Pkpasslibrary** – stellt die API für den Zugriff auf die Passungen auf dem Gerät bereit.
- **Pkaddpassesviewcontroller** – wird verwendet, um einen Durchlauf anzuzeigen, den der Benutzer in seinem Wallet speichern soll.
- **Pkaddpassesviewcontrollerdelegat** – xamarin. IOS-Entwickler

## <a name="example"></a>Beispiel

Weitere Informationen finden Sie im passlibrary-Projekt im [Beispielcode](https://docs.microsoft.com/samples/xamarin/ios-samples/passkit) für diesen Artikel. Es werden die folgenden allgemeinen Funktionen veranschaulicht, die in einer Wallet-Begleit Anwendung erforderlich wären:

### <a name="check-that-wallet-is-available"></a>Überprüfen, ob Wallet verfügbar ist

Wallet ist auf dem iPad nicht verfügbar, daher sollten Anwendungen überprüfen, bevor Sie versuchen, auf passkit-Features zuzugreifen.

```csharp
if (PKPassLibrary.IsAvailable) {
    // create an instance and do stuff...
}
```

### <a name="creating-a-pass-library-instance"></a>Erstellen einer Pass Library-Instanz

Die passkit-Bibliothek ist kein Singleton, und Anwendungen sollten für den Zugriff auf die passkit-API erstellt und gespeichert werden.

```csharp
if (PKPassLibrary.IsAvailable) {
    library = new PKPassLibrary ();
    // do stuff...
}
```

### <a name="get-a-list-of-passes"></a>Eine Liste von Durchläufen erhalten

Anwendungen können eine Liste von Durchläufen aus der Bibliothek anfordern. Diese Liste wird automatisch nach passkit gefiltert, sodass Sie nur die mit Ihrer Team-ID erstellten Durchgänge sehen können, die in ihren Berechtigungen aufgeführt sind.

```csharp
var passes = library.GetPasses ();  // returns PKPass[]
```

Beachten Sie, dass der Simulator die Liste der zurückgegebenen Pässe nicht filtert, daher sollte diese Methode immer auf echten Geräten getestet werden. Diese Liste kann in einer uitableview angezeigt werden. Die [Beispiel-App](https://docs.microsoft.com/samples/xamarin/ios-samples/passkit) sieht wie folgt aus, nachdem zwei Coupons hinzugefügt wurden:

 [![](passkit-images/image29.png "Die Beispiel-App sieht wie folgt aus, nachdem zwei Coupons hinzugefügt wurden.")](passkit-images/image29.png#lightbox)

### <a name="displaying-passes"></a>Anzeigen von Durchläufen

Ein eingeschränkter Satz von Informationen ist für das Rendering von Pass-in-Begleit-apps verfügbar.

Wählen Sie aus diesem Satz von Standardeigenschaften aus, um Listen von Durchläufen anzuzeigen, wie im Beispielcode gezeigt.

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

Diese Zeichenfolge wird im [Beispiel](https://docs.microsoft.com/samples/xamarin/ios-samples/passkit)als Warnung angezeigt:

 [![](passkit-images/image30.png "Die von Coupon ausgewählte Warnung im Beispiel")](passkit-images/image30.png#lightbox)

Sie können auch die `LocalizedValueForFieldKey()` -Methode verwenden, um Daten aus Feldern in den von Ihnen entworfenen Vorgängen abzurufen (da Sie wissen, welche Felder vorhanden sein sollten). Der Beispielcode zeigt dies nicht.

### <a name="loading-a-pass-from-a-file"></a>Laden einer Datei aus einer Datei

Da ein Durchlauf nur mit der Berechtigung des Benutzers zu Wallet hinzugefügt werden kann, muss ein Ansichts Controller angezeigt werden, um die Entscheidung zu treffen. Dieser Code wird in der Schaltfläche " **Hinzufügen** " im Beispiel verwendet, um ein vorgefertigter Durchlauf zu laden, das in die APP eingebettet ist (Sie sollten dies durch eine solche ersetzen, die Sie signiert haben):

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

Der Durchlauf wird mit den Optionen **Hinzufügen** und **Abbrechen** angezeigt:

 [![](passkit-images/image20.png "Die mit Add-und Cancel-Optionen vorgestellte Übergabe")](passkit-images/image20.png#lightbox)

### <a name="replace-an-existing-pass"></a>Vorhandenen Durchlauf ersetzen

Wenn ein vorhandener Durchlauf ersetzt wird, ist die Berechtigung des Benutzers nicht erforderlich, es tritt jedoch ein Fehler auf, wenn der Durchlauf nicht bereits vorhanden ist.

```csharp
if (library.Contains (newPass)) {
     library.Replace (newPass);
}
```

### <a name="editing-a-pass"></a>Bearbeiten eines bestanden

Pkpass ist nicht änderbar, sodass Sie keine Pass-Objekte in Ihrem Code aktualisieren können. Um die Daten in einem Durchlauf zu ändern, muss eine Anwendung Zugriff auf einen Webserver haben, der einen Datensatz mit Pass-Through-Vorgängen aufbewahren und eine neue Pass Datei mit aktualisierten Werten generieren kann, die von der Anwendung heruntergeladen werden können.

Die Erstellung der Dateierstellung muss auf einem Server erfolgen, da Pass mit einem Zertifikat signiert werden muss, das Privat und sicher gehalten werden muss.

Nachdem eine aktualisierte Pass Datei generiert wurde, verwenden Sie die `Replace` -Methode, um die alten Daten auf dem Gerät zu überschreiben.

### <a name="display-a-pass-for-scanning"></a>Einen Durchlauf zum Scannen anzeigen

Wie bereits erwähnt, kann nur Wallet einen Durchlauf zum Scannen anzeigen. Ein Durchlauf kann mithilfe der `OpenUrl` -Methode wie folgt angezeigt werden:

 `UIApplication.SharedApplication.OpenUrl (p.PassUrl);`

### <a name="receiving-notifications-of-changes"></a>Empfangen von Benachrichtigungen über Änderungen

Anwendungen können mithilfe `PKPassLibraryDidChangeNotification`von Änderungen an der Pass Bibliothek überwachen. Änderungen können dadurch verursacht werden, dass Benachrichtigungen im Hintergrund aktualisiert werden. Daher empfiehlt es sich, Sie in Ihrer APP zu überwachen.

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

Beim Registrieren für die Benachrichtigung ist es wichtig, eine Bibliotheks Instanz zu übergeben, da pkpasslibrary kein Singleton ist.

## <a name="server-processing"></a>Server Verarbeitung

Eine ausführliche Erläuterung zum Aufbau einer Serveranwendung zur Unterstützung von passkit geht über den Rahmen dieses Einführungs Artikels hinaus.

Weitere Informationen finden [Sie unter dotnet-Passbook](https://github.com/tomasmcguinness/dotnet-passbook) serverseitiger Open Source C# -Code.

## <a name="push-notifications"></a>Pushbenachrichtigungen

Eine ausführliche Erläuterung der Verwendung von Pushbenachrichtigungen zum Aktualisieren von Durchläufen geht über den Rahmen dieses Einführungs Artikels hinaus.

Sie müssen die von Apple definierte Rest-ähnliche API implementieren, um auf Webanforderungen von Wallet zu antworten, wenn Updates erforderlich sind.

Weitere Informationen finden Sie in der Apple-Anleitung zum [Aktualisieren eines Pass](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/PassKit_PG/Updating.html#//apple_ref/doc/uid/TP40012195-CH5-SW1) Handbuchs.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde passkit vorgestellt, einige der Gründe, warum es hilfreich ist, beschrieben und die verschiedenen Teile beschrieben, die für eine vollständige passkit-Lösung implementiert werden müssen. Es wurden die Schritte beschrieben, die erforderlich sind, um Ihr Apple-Entwicklerkonto zum Erstellen von Durchläufen, den Prozess zum manuellen durchführen und den Zugriff auf die passkit-APIs aus einer xamarin. IOS-Anwendung zu konfigurieren.

## <a name="related-links"></a>Verwandte Links

- [Wallet für Entwickler](https://developer.apple.com/wallet/)
- [Passkit-Beispiel](https://docs.microsoft.com/samples/xamarin/ios-samples/passkit)
- [Wallet-Entwicklerhandbuch](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/PassKit_PG/index.html#//apple_ref/doc/uid/TP40012195-CH1-SW1)
- [Frameworks – Apple Pay und Wallet (WWDC-Videos)](https://developer.apple.com/videos/frameworks/apple-pay-and-wallet)
- [Verweis auf das passkit-Framework](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Framework/_index.html)
- [Webdienst Referenz für Passbook](https://developer.apple.com/library/prerelease/ios/#documentation/PassKit/Reference/PassKit_WebService/WebService.html)
- [Informationen zu Pass files](https://developer.apple.com/library/prerelease/ios/#documentation/UserExperience/Reference/PassKit_Bundle/Chapters/Introduction.html)
- [dotnet-Passbook](https://github.com/tomasmcguinness/dotnet-passbook), eine Open-Source-Bibliothek zum Erstellen von IOS-Wallet-Paketen
- [Einführung in iOS 6](~/ios/platform/introduction-to-ios6/index.md)
