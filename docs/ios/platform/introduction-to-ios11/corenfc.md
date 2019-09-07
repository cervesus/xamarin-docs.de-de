---
title: Kern-NFC in xamarin. IOS
description: In diesem Dokument wird beschrieben, wie Near-Field-Kommunikations Tags in xamarin. IOS mithilfe der in ios 11 eingeführten APIs gelesen werden.
ms.prod: xamarin
ms.technology: xamarin-ios
ms.assetid: 846B59D3-F66A-48F3-A78C-84217697194E
author: conceptdev
ms.author: crdun
ms.date: 09/25/2017
ms.openlocfilehash: c7a9d359842dde916fc14ffea5ec6e3f453dfee0
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70752429"
---
# <a name="core-nfc-in-xamarinios"></a>Kern-NFC in xamarin. IOS

_Lesen von NFC-Tags (Near Field Communication) mithilfe von IOS 11_

Correfc ist ein neues Framework in ios 11, das den Zugriff auf das NFC-Radio ( _Near Field Communication_ ) zum Lesen von Tags aus Apps ermöglicht. Es funktioniert auf iPhone 7, 7 Plus, 8, 8 Plus und X.

Der NFC-TagReader in IOS-Geräten unterstützt alle NFC-Tagtypen von 1 bis 5, die Informationen zum _NFC-Datenaustausch Format_ (ndef) enthalten.

Beachten Sie die folgenden Einschränkungen:

- Corenfc unterstützt nur Transponder Lesevorgänge (nicht schreiben oder formatieren).
- Tagscans müssen vom Benutzer initiiert werden, und es muss ein Timeout nach 60 Sekunden angegeben werden.
- Apps müssen im Vordergrund für die Überprüfung sichtbar sein.
- Correfc kann nur auf echten Geräten (nicht auf dem Simulator) getestet werden.

Auf dieser Seite wird die Konfiguration beschrieben, die für die Verwendung von corenfc und die Verwendung der API mithilfe des [Beispielcodes "nfctagreader](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-nfctagreader)" erforderlich ist.

## <a name="configuration"></a>Konfiguration

Zum Aktivieren von Cor-FC müssen Sie drei Elemente in Ihrem Projekt konfigurieren:

- Ein Datenschutz Schlüssel für " **Info. plist** ".
- Ein "Berechtigungs **. plist** "-Eintrag.
- Ein Bereitstellungs Profil mit **NFC-taglese** Funktion.

### <a name="infoplist"></a>Info.plist

Fügen Sie den Datenschutz Schlüssel **nfkreaderusagedescription** und den Text hinzu, der dem Benutzer angezeigt wird, während die Überprüfung stattfindet. Verwenden Sie eine für Ihre Anwendung geeignete Nachricht (z. b., um den Zweck der Überprüfung zu erläutern):

```xml
<key>NFCReaderUsageDescription</key>
<string>NFC tag to read NDEF messages into the application</string>
```

### <a name="entitlementsplist"></a>Entitlements.plist

Ihre APP muss die Lesefunktion des **Near Field Communications-Tags** mit dem folgenden Schlüssel-Wert-Paar in der Berechtigung " **Berechtigungen. plist**" anfordern:

```xml
<key>com.apple.developer.nfc.readersession.formats</key>
<array>
  <string>NDEF</string>
</array>
```

### <a name="provisioning-profile"></a>Bereitstellungsprofil

Erstellen Sie eine neue **App-ID** , und stellen Sie sicher, dass der NFC- **taglesedienst** getickt ist

[![Entwickler Portal Seite "neue APP-ID" mit ausgewähltem NFC-Tag](corenfc-images/app-services-nfc-sml.png)](corenfc-images/app-services-nfc.png#lightbox)

Sie sollten dann ein neues Bereitstellungs Profil für diese APP-ID erstellen, es dann herunterladen und auf Ihrem Entwicklungs-Mac installieren.

## <a name="reading-a-tag"></a>Lesen eines Tags

Nachdem das Projekt konfiguriert wurde, fügen `using CoreNFC;` Sie am Anfang der Datei hinzu, und befolgen Sie diese drei Schritte, um NFC-Transponder Lesefunktionen zu implementieren:

### <a name="1-implement-infcndefreadersessiondelegate"></a>1. Umsetzt`INFCNdefReaderSessionDelegate`

Die-Schnittstelle verfügt über zwei Methoden, die implementiert werden müssen:

- `DidDetect`– Wird aufgerufen, wenn ein Transponder erfolgreich gelesen wurde.
- `DidInvalidate`– Wird aufgerufen, wenn ein Fehler auftritt oder das 60-Sekunden-Timeout erreicht wird.

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

Diese Methode kann mehrmals aufgerufen werden (und es kann ein Array von Nachrichten übermittelt werden), wenn die Sitzung mehrere Transponder Lesevorgänge zulässt. Dies wird mit dem dritten Parameter der `Start` -Methode festgelegt (erläutert in [Schritt 2](#step2)).

#### <a name="didinvalidate"></a>DidInvalidate

Die Invalidierung kann aus verschiedenen Gründen erfolgen:

- Fehler beim Scannen.
- Die APP ist nicht mehr im Vordergrund.
- Der Benutzer hat den Scanvorgang abgebrochen.
- Die Überprüfung wurde von der APP abgebrochen.

Der folgende Code zeigt, wie ein Fehler behandelt wird:

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

Nachdem eine Sitzung ungültig gemacht wurde, muss ein neues Sitzungs Objekt erstellt werden, um den Scanvorgang erneut durchsuchen zu können.

<a name="step2" />

### <a name="2-start-an-nfcndefreadersession"></a>2. Starten eines`NFCNdefReaderSession`

Die Überprüfung sollte mit einer Benutzer Anforderung beginnen, z. b. einem Schaltflächen-Press.
Mit dem folgenden Code wird eine Scan Sitzung erstellt und gestartet:

```csharp
Session = new NFCNdefReaderSession(this, null, true);
Session?.BeginSession();
```

Die Parameter für den `NFCNdefReaderSession` Konstruktor lauten wie folgt:

- `delegate`– Eine Implementierung von `INFCNdefReaderSessionDelegate`. Im Beispielcode wird der Delegat im Tabellen Ansichts Controller implementiert und daher `this` als Delegatparameter verwendet.
- `queue`– Die Warteschlange, für die die Rückrufe verarbeitet werden. Dies kann sein `null`. in diesem Fall sollten Sie sicherstellen, `DispatchQueue.MainQueue` dass beim Aktualisieren von Steuerelementen der Benutzeroberfläche verwendet wird (wie im Beispiel gezeigt).
- `invalidateAfterFirstRead`– Gibt an `false` ,dassdieÜberprüfungnachdererstenerfolgreichenÜberprüfungbeendetwird,wenndieÜberprüfungfortgesetztwirdundmehrereErgebnissezurückgegebenwerden,bisdieÜberprüfungabgebrochenoderdasTimeout`true`von 60 Sekunden erreicht

### <a name="3-cancel-the-scanning-session"></a>3. Abbrechen der Scan Sitzung

Der Benutzer kann die Scan Sitzung über eine vom System bereitgestellte Schaltfläche in der Benutzeroberfläche Abbrechen:

![Schaltfläche "Abbrechen" beim Scannen](corenfc-images/scan-cancel-sml.png)

Die APP kann den Scanprogramm gesteuert abbrechen, indem Sie `InvalidateSession` die-Methode aufrufen:

```csharp
Session.InvalidateSession();
```

In beiden Fällen wird die- `DidInvalidate` Methode des Delegaten aufgerufen.

## <a name="summary"></a>Zusammenfassung

Corenfc ermöglicht der APP das Lesen von Daten aus NFC-Tags. Es unterstützt das Lesen einer Vielzahl von tagformaten (ndef-Typen 1 bis 5), unterstützt jedoch weder das Schreiben noch das formatieren.

## <a name="related-links"></a>Verwandte Links

- [NF ctagreader (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-nfctagreader)
- [Einführung in Core NFC (WWDC) (Video)](https://developer.apple.com/videos/play/wwdc2017/718/)
