---
title: Core NFC in Xamarin.iOS
description: Dieses Dokument beschreibt, wie in der Nähe von Feld-Communication-Tags in Xamarin.iOS unter Verwendung der APIs in iOS 11 eingeführte gelesen wird.
ms.prod: xamarin
ms.technology: xamarin-ios
ms.assetid: 846B59D3-F66A-48F3-A78C-84217697194E
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/25/2017
ms.openlocfilehash: 1381a4564f93fd091f181949454df3f06b31ae6b
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350832"
---
# <a name="core-nfc-in-xamarinios"></a>Core NFC in Xamarin.iOS

_Lesen Near Field Communication (NFC)-Tags, die mithilfe von iOS 11_

CoreNFC ist ein neues Framework in iOS 11, den Zugriff auf die _Near Field Communication_ (NFC) Radio Tags aus, in apps zu lesen. Es funktioniert auf einem iPhone 7 7 Plus 8, 8, Plus und X.

Der NFC-Tag-Reader in iOS-Geräten unterstützt alle NFC Tag 1 bis 5, die enthalten _NFC-Datenaustauschformat_ (NDEF) Informationen.

Es gibt einige Einschränkungen zu beachten:

- CoreNFC unterstützt nur Transponder lesen (nicht das Schreiben oder Formatierung).
- Überprüfungen der Tag muss vom Benutzer initiierte, und Timeout nach 60 Sekunden.
- Apps müssen im Vordergrund für die Überprüfung angezeigt werden.
- CoreNFC kann nur auf echten Geräten (nicht auf dem Simulator) getestet werden.

Diese Seite beschreibt die Konfiguration erforderlich, um CoreNFC verwenden, und zeigt, wie die API mit dem ["TFCTagReader" Beispielcode](https://developer.xamarin.com/samples/monotouch/ios11/NFCTagReader/).

## <a name="configuration"></a>Konfiguration

Um CoreNFC zu aktivieren, müssen Sie drei Elemente in Ihrem Projekt konfigurieren:

- Ein **"Info.plist"** datenschutzschlüssel.
- Ein **"Entitlements.plist"** Eintrag.
- Ein Bereitstellungsprofil mit **Lesen von NFC-Tags** Funktion.

### <a name="infoplist"></a>Info.plist

Hinzufügen der **NFCReaderUsageDescription** datenschutzschlüssel und Text, der dem Benutzer angezeigt wird, während die Überprüfung ausgeführt wird. Verwenden Sie eine Nachricht, die für Ihre Anwendung geeignet (z. B. erläutert den Zweck des Scans):

```xml
<key>NFCReaderUsageDescription</key>
<string>NFC tag to read NDEF messages into the application</string>
```

### <a name="entitlementsplist"></a>Entitlements.plist

Muss Ihre app Anfordern der **in der Nähe von Feld Kommunikation lesen Tags** Funktion, die mit den folgenden Schlüssel/Wert-Paar in Ihre **"Entitlements.plist"**:

```xml
<key>com.apple.developer.nfc.readersession.formats</key>
<array>
  <string>NDEF</string>
</array>
```

### <a name="provisioning-profile"></a>Bereitstellungsprofil

Erstellen Sie ein neues **App-ID** und sicherstellen, dass die **Lesen von NFC-Tags** Dienst ausgewählt ist:

[![Auf der Developer-Portal neue App-ID mit dem Lesen von NFC-Tags ausgewählt](corenfc-images/app-services-nfc-sml.png)](corenfc-images/app-services-nfc.png#lightbox)

Sie sollten klicken Sie dann erstellen ein neues Bereitstellungsprofil für diese App-ID, und klicken Sie dann herunterladen, und installieren Sie es auf der Mac-Entwicklung

## <a name="reading-a-tag"></a>Ein Tag gelesen

Sobald Ihr Projekt konfiguriert ist, fügen `using CoreNFC;` am Anfang der Datei und führen Sie diese drei Schritte zum Implementieren von NFC tag-lesen-Funktionen:

### <a name="1-implement-infcndefreadersessiondelegate"></a>1. Implementieren `INFCNdefReaderSessionDelegate`

Die Schnittstelle verfügt über zwei Methoden implementiert werden:

- `DidDetect` : Wird aufgerufen, wenn ein Transponder erfolgreich gelesen wird.
- `DidInvalidate` : Wird aufgerufen, wenn ein Fehler auftritt oder das Timeout von 60 Sekunden erreicht ist.

#### <a name="diddetect"></a>DidDetect

Im Beispielcode wird die einzelnen gescannte Nachrichten zu einer Tabellenansicht hinzugefügt:

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

Diese Methode kann mehrmals aufgerufen werden (und möglicherweise ein Array von Nachrichten übergeben werden), wenn die Sitzung mehrere RFID-Transponderlesevorgänge ermöglicht. Dadurch wird festgelegt, mit dem dritten Parameter von der `Start` Methode (beschrieben [Schritt 2](#step2)).

#### <a name="didinvalidate"></a>DidInvalidate

Invalidierung kann aus verschiedenen Gründen auftreten:

- Fehler beim Überprüfen.
- Die app nicht mehr im Vordergrund werden.
- Der Benutzer möchte die Überprüfung abbrechen.
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

Sobald eine Sitzung ungültig geworden ist, muss ein neues Sitzungsobjekt erstellt werden, um erneut zu überprüfen.

<a name="step2" />

### <a name="2-start-an-nfcndefreadersession"></a>2. Starten Sie eine `NFCNdefReaderSession`

Überprüfung sollten mit einer benutzeranforderung, wie z. B. Klicken einer Schaltfläche beginnen.
Der folgende Code erstellt und startet eine Scan-Sitzung:

```csharp
Session = new NFCNdefReaderSession(this, null, true);
Session?.BeginSession();
```

Die Parameter für die `NFCNdefReaderSession` Konstruktor lauten wie folgt:

- `delegate` – Eine Implementierung von `INFCNdefReaderSessionDelegate`. Im Beispielcode wird der Delegat wird implementiert in der Tabelle-View-Controller daher `this` als die Delegate-Parameter verwendet wird.
- `queue` – Die Warteschlange, der Rückrufe für behandelt werden. Es kann sein `null`, werden in diesem Fall verwenden Sie die `DispatchQueue.MainQueue` beim Aktualisieren von Benutzeroberflächen-Steuerelemente (wie im Beispiel gezeigt).
- `invalidateAfterFirstRead` – Wenn `true`, die Überprüfung beendet wird, nach der ersten erfolgreichen Überprüfung; Wenn `false` Analyse fortgesetzt wird und mehrere Ergebnisse zurückgegeben werden soll, bis die Überprüfung wird abgebrochen, oder das Timeout von 60 Sekunden erreicht wird.


### <a name="3-cancel-the-scanning-session"></a>3. Die Scan-Sitzung abbrechen

Der Benutzer kann über eine vom System bereitgestellte Schaltfläche in der Benutzeroberfläche der Scan-Sitzung abbrechen:

![Schaltfläche "Abbrechen", bei der Überprüfung](corenfc-images/scan-cancel-sml.png)

Die app kann programmgesteuert die Überprüfung abbrechen, durch den Aufruf der `InvalidateSession` Methode:

```csharp
Session.InvalidateSession();
```

In beiden Fällen den Delegaten der `DidInvalidate` aufgerufene Methode.

## <a name="summary"></a>Zusammenfassung

CoreNFC ermöglicht Ihrer app zum Lesen von Daten von NFC-Tags. Es unterstützt eine Vielzahl von Tag-Formate (NDEF Arten von 1 bis 5) lesen und schreiben oder die Formatierung nicht unterstützt.


## <a name="related-links"></a>Verwandte Links

- [NFCTagReader (Beispiel)](https://developer.xamarin.com/samples/monotouch/ios11/NFCTagReader/)
- [Einführung in Core NFC (WWDC) (Video)](https://developer.apple.com/videos/play/wwdc2017/718/)
