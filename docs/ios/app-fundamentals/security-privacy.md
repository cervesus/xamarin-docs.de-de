---
title: iOS Sicherheits- und Datenschutzfunktionen
description: "Dieser Artikel behandelt die Arbeit mit Sicherheit und Datenschutz für IOS- und wie sie eine app Xamarin.iOS auswirken."
ms.topic: article
ms.prod: xamarin
ms.assetid: 718C8721-C359-4650-878A-D68E159A3F53
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 5e4bbc22403c6c0bfa5c8dc7ac4e3a39545051d4
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
# <a name="ios-security-and-privacy-features"></a>iOS Sicherheits- und Datenschutzfunktionen

_Dieser Artikel behandelt die Arbeit mit Sicherheit und Datenschutz für IOS- und wie sie eine app Xamarin.iOS auswirken._

Apple hat mehrere Erweiterungen für Sicherheit und Datenschutz für iOS 10 (und höher), mit deren Hilfe wird den Entwickler verbessern Sie die Sicherheit ihrer Apps und des Endbenutzers Datenschutz gewährleisten. In diesem Artikel befasst sich diese Funktionen in einer app Xamarin.iOS implementieren.

Die folgenden Themen werden im Detail behandelt:

- [Allgemeine Verbesserungen](#General-Enhancements)
- [Zugreifen auf Private Benutzerdaten](#Accessing-Private-User-Data)
    - [Datenschutz Einstellungsschlüssel](#Setting-Privacy-Keys)
    
<a name="General-Enhancements" />

## <a name="general-enhancements"></a>Allgemeine Verbesserungen

Die folgenden allgemeinen Sicherheit und Datenschutz in iOS 10 vorgenommenen Änderungen wurden:

- Der allgemeine Data Security Architecture (CDSA) API ist veraltet und sollten ersetzt werden, mit der SecKey-API, um asymmetrische Schlüssel zu generieren.
- Die neue `NSAllowsArbitraryLoadsInWebContent` Schlüssel können hinzugefügt werden die einer app `Info.plist` Datei und lässt das Webseiten ordnungsgemäß geladen werden, während der Apple-Transport-Sicherheit (ATS)-Schutz für den Rest der app weiterhin aktiviert ist. Weitere Informationen finden Sie unter unsere [App Transportsicherheit](~/ios/app-fundamentals/ats.md) Dokumentation.
- Da die neue Zwischenablage in iOS 10 und MacOS Sierra ermöglicht es dem Benutzer kopieren und Einfügen zwischen den Geräten, wurde die API erweitert, um eine Zwischenablage auf ein bestimmtes Gerät beschränkt und werden mit einem Zeitstempel versehen, um zu einem bestimmten Zeitpunkt automatisch aufgelöst werden können. Darüber hinaus benannte Montageflächen werden nicht mehr beibehalten und mit den freigegebenen Montagefläche Containern ersetzt werden sollte.
- Für alle SSL/TLS-Verbindungen wird der symmetrische RC4-Verschlüsselungsverfahren jetzt standardmäßig deaktiviert. Darüber hinaus die sicheren Transport-API unterstützt nicht mehr SSLv3, und es wird empfohlen, dass der Entwickler nicht mehr so bald wie möglich, SHA-1 und 3DES Kryptografie zu verwenden.

<a name="Accessing-Private-User-Data" />

## <a name="accessing-private-user-data"></a>Zugreifen auf Private Benutzerdaten

Apps unter iOS 10 (oder höher) müssen ihre Absicht auf bestimmte Funktionen oder Benutzerinformationen zugreifen, indem Sie in einen oder mehrere Datenschutz-Schlüssel eingeben statisch deklarieren ihre `Info.plist` Dateien, die dem Benutzer erklären, warum die app zugreifen möchte.

> [!IMPORTANT]
> Apps, die nicht bereitstellen, die benötigten Schlüssel werden im Hintergrund beendet das System beim Versuch, eine der eingeschränkten Funktionen oder Benutzerinformationen zugreifen _ohne Fehler_! Start eine app auf iOS 10 unerwartet fehlschlagen müssen sicherstellen, dass alle erforderlichen `Info.plist` angegeben wurden.

Im Zusammenhang mit des folgenden Datenschutzes Schlüssel verfügbar sind:

- **Datenschutz - Apple-Musik Nutzung Beschreibung** (`NSAppleMusicUsageDescription`)-ermöglicht dem Entwickler, die beschreiben, warum die app des Benutzers Medienbibliothek zugreifen möchte.
- **Datenschutz – Bluetooth Peripheriegeräte Beschreibung der Verwendung** (`NSBluetoothPeripheralUsageDescription`)-ermöglicht dem Entwickler, die beschreiben, warum die app Bluetooth auf dem Gerät des Benutzers zugreifen möchte.
- **Datenschutz - Beschreibung der Verwendung Kalender** (`NSCalendarsUsageDescription`)-ermöglicht dem Entwickler, die beschreiben, warum die app, die auf den Kalender des Benutzers zugreifen möchte.
- **Datenschutz - Beschreibung der Kamera Verwendung** (`NSCameraUsageDescription`)-ermöglicht dem Entwickler, die beschreiben, warum die app die Kamera des Geräts zugreifen möchte.
- **Datenschutz - Beschreibung der Kontakte Verwendung** (`NSContactsUsageDescription`)-ermöglicht dem Entwickler, die beschreiben, warum die app dem Benutzer Kontakte zugreifen möchte.
- **Datenschutz - Beschreibung der Integrität Freigabe Verwendung** (`NSHealthShareUsageDescription`)-ermöglicht dem Entwickler, die beschreiben, warum die app des Benutzers Zustandsdaten zugreifen möchte. Weitere Informationen finden Sie in der Apple- [HKHealthStore Class Reference](https://developer.apple.com/reference/healthkit/hkhealthstore).
- **Datenschutz - Beschreibung bei der Verwendung von Integrität** (`NSHealthUpdateUsageDescription`)-ermöglicht dem Entwickler, die beschreiben, warum die app des Benutzers Zustandsdaten bearbeiten möchte. Weitere Informationen finden Sie in der Apple- [HKHealthStore Class Reference](https://developer.apple.com/reference/healthkit/hkhealthstore).
- **Datenschutz - Beschreibung der Verwendung HomeKit** (`NSHomeKitUsageDescription`)-ermöglicht dem Entwickler, die beschreiben, warum die app des Benutzers HomeKit Konfigurationsdaten zugreifen möchte.
- **Datenschutz - Speicherort immer Beschreibung der Verwendung** (`NSLocationAlwaysUsageDescription`)-ermöglicht dem Entwickler, die beschreiben, warum die app immer auf den Standort des Benutzers zugreifen möchte.
- [Veraltet] **Datenschutz - Beschreibung des Standorts Nutzung** (`NSLocationUsageDescription`)-ermöglicht dem Entwickler, die beschreiben, warum die app Zugriff auf den Speicherort der Benutzer möchte. *Hinweis: Dieser Schlüssel wurde in iOS 8 (und höher) als veraltet markiert. Verwendung `NSLocationAlwaysUsageDescription` oder `NSLocationWhenInUseUsageDescription` stattdessen.*
- **Datenschutz - Speicherort bei In Verwendung Verwendungsbeschreibung** (`NSLocationWhenInUseUsageDescription`)-ermöglicht dem Entwickler, die beschreiben, warum das vom Standort des Benutzers zugreifen, während der Ausführung die app möchte.
- [Veraltet] **Datenschutz - Beschreibung der Media Library Verwendung** -ermöglicht dem Entwickler, die beschreiben, warum die app den Zugriff auf die Medienbibliothek der Benutzer möchte. *Hinweis: Dieser Schlüssel wurde in iOS 8 (und höher) als veraltet markiert. Verwendung `NSAppleMusicUsageDescription` stattdessen.*
- **Datenschutz - Beschreibung der Verwendung Mikrofon** (`NSMicrophoneUsageDescription`)-ermöglicht dem Entwickler, die beschreiben, warum die app das Mikrofon Geräte zugreifen möchte.
- **Datenschutz - Beschreibung der Verwendung während des Verschiebens** (`NSMotionUsageDescription`)-ermöglicht dem Entwickler, die beschreiben, warum die app, die auf dem Gerät Beschleunigungsmesser zugreifen möchte.
- **Datenschutz - Beschreibung der Foto Bibliothek Verwendung** (`NSPhotoLibraryUsageDescription`)-ermöglicht dem Entwickler, die beschreiben, warum die app des Benutzers Fotobibliothek zugreifen möchte.
- **Datenschutz - Beschreibung der Verwendung Erinnerungen** (`NSRemindersUsageDescription`)-ermöglicht dem Entwickler, die beschreiben, warum die app dem Benutzer Erinnerungen zugreifen möchte.
- **Datenschutz - Beschreibung der Verwendung Siri** (`NSSiriUsageDescription`)-ermöglicht dem Entwickler, die beschreiben, warum die app die Benutzerdaten zu Siri senden möchte.
- **Datenschutz - Beschreibung der Spracherkennung Recognition Verwendung** (`NSSpeechRecognitionUsageDescription`)-ermöglicht dem Entwickler, die beschreiben, warum die app Benutzerdaten an Apple Speech Recognition Server senden möchte.
- **Datenschutz - TV-Nutzung Anbieterbeschreibung** (`NSVideoSubscriberAccountUsageDescription`)-ermöglicht dem Entwickler, die beschreiben, warum die app, die auf das Konto des Benutzers TV-Anbieter zugreifen möchte.

Weitere Informationen zum Arbeiten mit `Info.plist` Schlüssel, finden Sie in der Apple- [Informationen Eigenschaftsverweis Liste Schlüssel](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009248-SW1).

<a name="Setting-Privacy-Keys" />

## <a name="setting-privacy-keys"></a>Datenschutz Einstellungsschlüssel

Nehmen Sie im folgende Beispiel für den Zugriff auf HomeKit auf iOS 10 (und höher), muss die Entwickler hinzufügen, die `NSHomeKitUsageDescription` Schlüssel für der app `Info.plist` Datei, und geben Sie eine Zeichenfolge deklarieren warum die app des Benutzers HomeKit Datenbank zugreifen möchte. Diese Zeichenfolge auf die Benutzer bei der ersten Zeit erhält Ausführen der app zu erstellen:

[![](security-privacy-images/info01.png "Eine Beispiel-NSHomeKitUsageDescription-Warnung")](security-privacy-images/info01.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Xamarin.iOS for Visual Studio aktuelle unterstützt keine Bearbeitung der sicherheitserweiterung `Info.plist` Einstellungen aus der IDE, damit die folgende problemumgehung erforderlich werden:

1. Öffnen der `Info.plist` Datei in einem externen Texteditor.
2. Vor dem letzten `</dict>` Knoten, fügen Sie den folgenden Knoten: `<key>NSHomeKitUsageDescription</key>`
3. Fügen Sie den folgenden Knoten aus, um die erforderlichen Beschreibung hinzu: `<string>Allows the app to control HomeKit enabled devices.</string>`
4. Die `Info.plist` Datei sollte folgendermaßen aussehen: 

    [![](security-privacy-images/info02vs.png "Die Datei "Info.plist" sollte wie folgt aussehen.")](security-privacy-images/info02vs.png#lightbox)
4. Speichern Sie die Änderungen in der Datei.
5. Zurück zu Visual Studio und zu den Recompile der app.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Um einer der Datenschutz-Schlüssel festzulegen, führen Sie folgende Schritte aus:

1. Doppelklicken Sie auf die `Info.plist` in der Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen.
2. Am unteren Rand des Bildschirms, wechseln Sie zu der **Quelle** anzeigen.
3. Fügen Sie einen neuen **Eintrag** der Liste.
4. Wählen Sie in der Dropdownliste einen Schlüssel (z. B. **Datenschutz - Beschreibung der Verwendung HomeKit**): 

    [![](security-privacy-images/info02.png "Wählen Sie einen datenschutzschlüssel")](security-privacy-images/info02.png#lightbox)
5. Geben Sie eine Beschreibung für die warum die app, die auf die angegebene Informationen zur Funktion oder Benutzer zugreifen möchte ein: 

    [![](security-privacy-images/info03.png "Geben Sie eine Beschreibung")](security-privacy-images/info03.png#lightbox)
6. Speichern Sie die Änderungen in der Datei.

-----

> [!IMPORTANT]
> Im Beispiel wird angegeben, über Fehler beim Festlegen der `NSHomeKitUsageDescription` -Schlüssel in der `Info.plist` Datei würde die app _im Hintergrund fehlerhaften_ (wird vom System zur Laufzeit geschlossen) ohne Fehler bei der Ausführung im iOS 10 (oder höher).

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden behandelt, die Sicherheit und Datenschutz Änderungen, die Apple iOS 10 und wie sie eine Xamarin.iOS-app betreffen gestellt hat.



## <a name="related-links"></a>Verwandte Links

- [iOS-10-Beispiele](https://developer.xamarin.com/samples/ios/iOS10/)
