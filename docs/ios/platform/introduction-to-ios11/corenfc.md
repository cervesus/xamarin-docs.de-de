---
title: Core NFC
description: Lesen Near Field Communication (NFC)-Tags mit iOS 11
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/25/2016
ms.openlocfilehash: 4975b4008c635ad2355ca2806ba867636dd50201
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="core-nfc"></a>Core NFC

_Lesen Near Field Communication (NFC)-Tags mit iOS 11_

CoreNFC ist ein neues Framework in iOS 11 den Zugriff auf die _Near Field Communication_ (NFC)-Sender, um Tags aus, in apps zu lesen. Es funktioniert auf dem iPhone 7, 7 Plus 8, 8 Plus- und X.

Der NFC-transponderlesegerät in iOS-Geräte unterstützt alle NFC Transpondertypen 1 bis 5, die enthalten _NFC-Datenaustauschformat_ (NDEF) Informationen.

Es gibt einige Einschränkungen zu berücksichtigen:

- CoreNFC unterstützt nur Transponder lesen (nicht schreiben oder Formatierung).
- Tag-Scans muss Benutzer initiiert, und das Timeout nach 60 Sekunden.
- Apps müssen im Vordergrund zum Scannen sichtbar werden.
- CoreNFC kann nur auf echten Geräten (nicht auf die Simulator) getestet werden.

Diese Seite beschreibt die Konfiguration erforderlich, um CoreNFC verwenden und zeigt, wie die API mit den ["TFCTagReader" Beispielcode](https://developer.xamarin.com/samples/monotouch/ios11/NFCTagReader/).

## <a name="configuration"></a>Konfiguration

Um CoreNFC zu aktivieren, müssen Sie drei Elemente in Ihrem Projekt konfigurieren:

- Ein **"Info.plist"** datenschutzschlüssel.
- Ein **Entitlements.plist** Eintrag.
- Ein Bereitstellungsprofil mit **Einlesen von NFC-Tags** Funktion.

### <a name="infoplist"></a>Info.plist

Hinzufügen der **NFCReaderUsageDescription** datenschutzschlüssel und Text, der dem Benutzer angezeigt wird, während die Überprüfung erfolgt. Verwenden Sie eine Nachricht, die für Ihre Anwendung geeignet (z. B. erklärt den Zweck des Scans):

```xml
<key>NFCReaderUsageDescription</key>
<string>NFC tag to read NDEF messages into the application</string>
```

### <a name="entitlementsplist"></a>Entitlements.plist

Ihre app muss Anfordern der **in der Nähe Feld Kommunikation Einlesen von Tags** Funktion, die mit den folgenden Schlüssel/Wert-Schlüsselpaar in Ihre **Entitlements.plist**:

```xml
<key>com.apple.developer.nfc.readersession.formats</key>
<array>
  <string>NDEF</string>
</array>
```

### <a name="provisioning-profile"></a>Bereitstellungsprofil

Erstellen Sie ein neues **App-ID** und sicherstellen, dass die **Einlesen von NFC-Tags** Dienst Konfigurationsdaten:

[ ![Developer Portal neue App-ID-Seite mit NFC Tag lesen ausgewählt](corenfc-images/app-services-nfc-sml.png)](corenfc-images/app-services-nfc.png)

Sie klicken Sie dann erstellen ein neues Bereitstellungsprofil für diese App-ID und dann herunterladen und installieren es auf der Mac-Entwicklung

## <a name="reading-a-tag"></a>Lesen Sie ein Tag

Sobald Ihr Projekt konfiguriert ist, fügen `using CoreNFC;` an den Anfang der Datei und führen Sie diese drei Schritte zur Implementierung von NFC tag lesen-Funktionalität:

### <a name="1-implement-infcndefreadersessiondelegate"></a>1. Implementieren `INFCNdefReaderSessionDelegate`

Die Schnittstelle verfügt über zwei Methoden implementiert werden:

- `DidDetect` – Wird aufgerufen, wenn ein Transponder erfolgreich gelesen wird.
- `DidInvalidate` – Wird aufgerufen, wenn ein Fehler auftritt oder das Timeout von 60 Sekunden erreicht ist.

#### <a name="diddetect"></a>DidDetect

Im Beispielcode wird jede gescannte Nachricht einer Tabellenansicht hinzugefügt:

```csharp
public void DidDetect(NFCNdefReaderSession session, NFCNdefMessage[] messages)
{
    foreach (NFCNdefMessage msg in messages)
    {  // adds the messages to a list view
        DetectedMessages.Add(msg);
    }
    DispatchQueue.MainQueue.DispatchAsync(() =>
    {
        this.TableView.ReloadData();
    });
}
```

Diese Methode kann mehrfach aufgerufen werden (und ein Array von Nachrichten übergeben werden kann), wenn die Sitzung für mehrere RFID-Transponderlesevorgänge lässt. Dadurch wird festgelegt, mit dem dritten Parameter von der `Start` Methode (erläutert [Schritt 2](#step2)).

#### <a name="didinvalidate"></a>DidInvalidate

Ungültigkeit kann aus verschiedenen Gründen auftreten:

- Fehler bei der Suche.
- Die app nicht mehr im Vordergrund werden.
- Der Benutzer hat die Überprüfung abbrechen.
- Die Überprüfung wurde von der app abgebrochen.

Der folgende Code zeigt, wie Fehler behandelt:

```csharp
public void DidInvalidate(NFCNdefReaderSession session, NSError error)
{
    var readerError = (NFCReaderError)(long)error.Code;
    if (readerError != NFCReaderError.ReaderSessionInvalidationErrorFirstNDEFTagRead &&
        readerError != NFCReaderError.ReaderSessionInvalidationErrorUserCanceled)
    {
      // some error handling
    }
}
```

Nachdem eine Sitzung ungültig geworden ist, muss ein neues Sitzungsobjekt erstellt werden, um erneut zu scannen.

<a name="step2" />

### <a name="2-start-an-nfcndefreadersession"></a>2. Starten Sie eine `NFCNdefReaderSession`

Überprüfung sollte mit einer Anforderung des Benutzers, z. B. ein Drücken der Taste beginnen.
Der folgende Code erstellt und startet eine Scan-Sitzung:

```csharp
Session = new NFCNdefReaderSession(this, null, true);
Session?.BeginSession();
```

Die Parameter für die `NFCNdefReaderSession` Konstruktor lauten wie folgt:

- `delegate` – Eine Implementierung von `INFCNdefReaderSessionDelegate`. Im Beispielcode wird der Delegat wird implementiert in der Tabelle View-Controller, daher `this` dient als Delegatparameters.
- `queue` – Die Warteschlange, der Rückrufe für behandelt werden. Es kann sein `null`, werden in diesem Fall sollten Sie anhand der `DispatchQueue.MainQueue` beim Aktualisieren von Benutzeroberflächen-Steuerelemente (wie im Beispiel gezeigt).
- `invalidateAfterFirstRead` – Wenn `true`, die Überprüfung beendet wird, nach der ersten erfolgreichen Scanvorgang; Wenn `false` Scan wird fortgesetzt, und mehrere Ergebnisse zurückgegeben, bis die Überprüfung abgebrochen wird, oder das Timeout von 60 Sekunden erreicht ist.


### <a name="3-cancel-the-scanning-session"></a>3. Die Scan-Sitzung abbrechen

Der Benutzer kann die Scan Sitzung über eine vom System bereitgestellte Schaltfläche in der Benutzeroberfläche Abbrechen:

![Schaltfläche "Abbrechen", bei der Suche nach](corenfc-images/scan-cancel-sml.png)

Die app programmgesteuert die Überprüfung durch Aufrufen von "Abbrechen" kann die `InvalidateSession` Methode:

```csharp
Session.InvalidateSession();
```

In beiden Fällen wird der Delegat des `DidInvalidate` Methode wird aufgerufen.

## <a name="summary"></a>Zusammenfassung

CoreNFC ermöglicht Ihrer app das Lesen von Daten aus der NFC-Tags. Unterstützt eine Vielzahl von Tag-Formate (NDEF Arten von 1 bis 5) lesen, aber schreiben oder die Formatierung nicht unterstützt.


## <a name="related-links"></a>Verwandte Links

- [NFCTagReader (Beispiel)](https://developer.xamarin.com/samples/monotouch/ios11/NFCTagReader/)
- [Einführung in die Core NFC (WWDC) (Video)](https://developer.apple.com/videos/play/wwdc2017/718/)
