---
title: iOS-Features für Sicherheit und Datenschutz
description: In diesem Dokument wird beschrieben, Sicherheits- und Datenschutzfeatures der e/a und erläutert, wie sie mit Xamarin.iOS. Es dauert einen Blick auf Updates, die in iOS 10 und wie Sie private Benutzerdaten zugreifen.
ms.prod: xamarin
ms.assetid: 718C8721-C359-4650-878A-D68E159A3F53
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 119fd478727a002622861c94462b93b75720a992
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61400582"
---
# <a name="ios-security-and-privacy-features"></a>iOS-Features für Sicherheit und Datenschutz

_Dieser Artikel behandelt die Arbeit mit Sicherheit und Datenschutz für iOS und deren Auswirkung auf einer Xamarin.iOS-app._

Apple hat verschiedene Verbesserungen sowohl Sicherheit und Datenschutz in iOS 10 (und höher) vorgenommen, die den Entwickler verbessern Sie die Sicherheit ihrer Apps und des Endbenutzers Datenschutz gewährleisten können. Dieser Artikel behandelt diese Features in einer Xamarin.iOS-app implementieren.
    
<a name="General-Enhancements" />

## <a name="general-enhancements"></a>Allgemeine Verbesserungen

Die folgenden allgemeinen Änderungen, die Sicherheit und Datenschutz in iOS 10 wurden:

- Die allgemeine Data Security Architecture (CDSA) API ist veraltet und sollte ersetzt werden, mit der SecKey-API, um asymmetrischen Schlüssel zu generieren.
- Die neue `NSAllowsArbitraryLoadsInWebContent` Schlüssel hinzugefügt werden kann die einer app **"Info.plist"** Datei und ermöglicht die Webseiten ordnungsgemäß geladen werden, während die Apple Transport Security (ATS) Schutz für den Rest der app immer noch aktiviert ist. Weitere Informationen finden Sie unserem [App Transport Security](~/ios/app-fundamentals/ats.md) Dokumentation.
- Da der neue Zwischenablage in iOS 10 und MacOS Sierra ermöglicht es dem Benutzer zum Kopieren und Einfügen zwischen Geräten, wurde die API erweitert, um ermöglichen eine Zwischenablage, um auf ein bestimmtes Gerät beschränkt, und mit einem Zeitstempel, zu einem bestimmten Zeitpunkt automatisch gelöscht werden sollen. Darüber hinaus werden die benannten Montageflächen werden nicht mehr beibehalten und mit den freigegebenen Montagefläche Containern ersetzt werden soll.
- Für alle SSL/TLS-Verbindungen ist das symmetrische RC4-Verschlüsselungsverfahren jetzt standardmäßig deaktiviert. Darüber hinaus den sicheren Transport-API unterstützt nicht mehr SSLv3, und es wird empfohlen, dass der Entwickler nicht mehr so bald wie möglich, SHA-1 und 3DES Kryptografie zu verwenden.

<a name="Accessing-Private-User-Data" />

## <a name="accessing-private-user-data"></a>Zugreifen auf Private Benutzerdaten

Apps, die unter iOS 10 (oder höher) müssen statisch deklariert werden ihre Absicht, bestimmte Features oder Benutzerinformationen zugreifen, indem Sie die Eingabe eine oder mehrere Datenschutzschlüssel in ihre **"Info.plist"** Dateien, die dem Benutzer erklären, warum die app zu erhalten möchte, Zugriff.

> [!IMPORTANT]
> Apps, die nicht angeben, die erforderlichen Schlüssel werden automatisch beendet werden durch das System beim Versuch, eine eingeschränkte Features oder Benutzerinformationen zugreifen _ohne Fehler_! Wenn eine app gestartet wird, unter iOS 10 unerwartet misslingt, sicher, dass alle erforderlichen **"Info.plist"** angegeben wurden.

Die folgende Datenschutz im Zusammenhang, dass der Schlüssel zur Verfügung stehen:

- **Datenschutz – Nutzung der Apple Music Beschreibung** (`NSAppleMusicUsageDescription`): ermöglicht dem Entwickler, die beschreiben, warum die app des Benutzers Medienbibliothek zugreifen möchte.
- **Datenschutz – Nutzungsbeschreibung für Bluetooth-Peripheriegeräte** (`NSBluetoothPeripheralUsageDescription`): ermöglicht dem Entwickler, die beschreiben, warum die app die Bluetooth auf dem Gerät des Benutzers zugreifen möchte.
- **Datenschutz – Nutzungsbeschreibung für Kalender** (`NSCalendarsUsageDescription`): ermöglicht dem Entwickler, die beschreiben, warum die app, die auf den Kalender des Benutzers zugreifen möchte.
- **Datenschutz – Nutzungsbeschreibung für Kamera** (`NSCameraUsageDescription`): ermöglicht dem Entwickler, die beschreiben, warum die app das Gerät die Kamera zugreifen möchte.
- **Datenschutz – Nutzungsbeschreibung für Kontakte** (`NSContactsUsageDescription`): ermöglicht dem Entwickler, die beschreiben, warum die app Kontakte des Benutzers zugreifen möchte.
- **Datenschutz – Integritätsfreigabe** (`NSHealthShareUsageDescription`): ermöglicht dem Entwickler, die beschreiben, warum die app Health-Daten des Benutzers zugreifen möchte. Weitere Informationen finden Sie unter Apple [HKHealthStore Class Reference](https://developer.apple.com/reference/healthkit/hkhealthstore).
- **Datenschutz – Integritätsaktualisierung** (`NSHealthUpdateUsageDescription`): ermöglicht dem Entwickler, die beschreiben, warum die app des Benutzers integritätsdaten bearbeiten möchte. Weitere Informationen finden Sie unter Apple [HKHealthStore Class Reference](https://developer.apple.com/reference/healthkit/hkhealthstore).
- **Datenschutz – HomeKit-Nutzungsbeschreibung** (`NSHomeKitUsageDescription`): ermöglicht dem Entwickler, die beschreiben, warum die app, die auf die Daten des Benutzers HomeKit-Konfiguration zugreifen möchte.
- **Datenschutz – Location Always-Nutzungsbeschreibung** (`NSLocationAlwaysUsageDescription`): ermöglicht dem Entwickler, die beschreiben, warum die app immer auf den Standort des Benutzers zugreifen möchte.
- [Veraltet] **Datenschutz – Nutzungsbeschreibung für Ortungsdienste** (`NSLocationUsageDescription`): ermöglicht dem Entwickler, die beschreiben, warum die app, die auf den Speicherort der Benutzer zugreifen möchte. *HINWEIS: Dieser Schlüssel wurde in iOS 8 (und höher) als veraltet markiert. Verwendung `NSLocationAlwaysUsageDescription` oder `NSLocationWhenInUseUsageDescription` stattdessen.*
- **Datenschutz – Location beim In Use Usage Description** (`NSLocationWhenInUseUsageDescription`): ermöglicht dem Entwickler, die beschreiben, warum die app Zugriff auf den Standort des Benutzers, während der Ausführung möchte.
- [Veraltet] **Datenschutz – Nutzungsbeschreibung für Medienbibliothek** -ermöglicht dem Entwickler, die beschreiben, warum die app auf die Medienbibliothek zugreifen möchte. *HINWEIS: Dieser Schlüssel wurde in iOS 8 (und höher) als veraltet markiert. Verwendung `NSAppleMusicUsageDescription` stattdessen.*
- **Datenschutz – Nutzungsbeschreibung für Mikrofon** (`NSMicrophoneUsageDescription`): ermöglicht dem Entwickler, die beschreiben, warum die app das Mikrofon Geräte zugreifen möchte.
- **Datenschutz – Nutzungsbeschreibung für während der Übertragung** (`NSMotionUsageDescription`): ermöglicht dem Entwickler, die beschreiben, warum die app des Geräts Beschleunigungsmesser zugreifen möchte.
- **Datenschutz – Nutzungsbeschreibung für Foto** (`NSPhotoLibraryUsageDescription`): ermöglicht dem Entwickler, die beschreiben, warum die app Fotobibliothek des Benutzers zugreifen möchte.
- **Datenschutz – Nutzungsbeschreibung für Erinnerungen** (`NSRemindersUsageDescription`): ermöglicht dem Entwickler, die beschreiben, warum die app des Benutzers Erinnerungen zugreifen möchte.
- **Datenschutz – Nutzungsbeschreibung für Siri** (`NSSiriUsageDescription`): ermöglicht dem Entwickler, die beschreiben, warum die app die Daten des Benutzers siri senden möchte.
- **Datenschutz – Nutzungsbeschreibung für Spracherkennung** (`NSSpeechRecognitionUsageDescription`): ermöglicht dem Entwickler, die beschreiben, warum die app Benutzerdaten an Apple Speech Recognition Server senden möchte.
- **Datenschutz – Nutzungsbeschreibung für TV-Anbieter** (`NSVideoSubscriberAccountUsageDescription`): ermöglicht dem Entwickler, die beschreiben, warum die app TV-Provider-Konto des Benutzers zugreifen möchte.

Weitere Informationen zum Arbeiten mit **"Info.plist"** Schlüssel finden Sie die Apple [Informationen Eigenschaftsverweis Liste Schlüssel](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009248-SW1).

<a name="Setting-Privacy-Keys" />

## <a name="setting-privacy-keys"></a>Datenschutzschlüssel festlegen

Im folgenden Beispiel für den Zugriff auf HomeKit unter iOS 10 (und höher), die Entwickler müssen hinzufügen, die `NSHomeKitUsageDescription` -Schlüssel in der app **"Info.plist"** Datei, und geben Sie eine Zeichenfolge deklarieren warum die app des Benutzers HomeKit zugreifen möchte die Datenbank. Diese Zeichenfolge wird der Benutzer bei der ersten Zeit angezeigt, die Benutzer die app ausführen:

![Ein Beispiel für Warnung NSHomeKitUsageDescription](security-privacy-images/info01.png "NSHomeKitUsageDescription ein Beispiel für Warnung")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Xamarin.iOS für Visual Studio derzeit nicht unterstützen die Bearbeitung der **"Info.plist"** datenschutzschlüssel über den Standard-iOS-manifest-Editor. Stattdessen müssen Sie mit dem generischen PList-Editor, also gehen Sie folgendermaßen vor:

1. Mit der rechten Maustaste auf die **"Info.plist"** Datei die **Projektmappen-Explorer** , und wählen Sie **Öffnen mit...** .
2. Wählen Sie die **generische PList-Editor** aus der Liste der Programme für die Datei zu öffnen, klicken Sie dann auf **OK**.

    ![Wählen Sie den generischen PList-Editor](security-privacy-images/InfoEditorSelectionVs.png "wählen Sie den generischen PList-Editor")
3. Klicken Sie auf die **+** Schaltfläche in der letzten Zeile in den Editor aus, um einen neuen Eintrag zur Liste hinzuzufügen. Dies wird aufgerufen, "Benutzerdefinierte Eigenschaft", mit der `String` und ein leerer Wert.
4. Klicken Sie auf den Eigenschaftennamen, und eine Dropdownliste wird angezeigt.
5. Wählen Sie aus der Dropdownliste aus, die einen Schlüssel (z. B. **Datenschutz – HomeKit-Nutzungsbeschreibung**): 

    ![Wählen Sie einen Schlüssel](security-privacy-images/InfoPListEditorSelectKey.png "datenschutzschlüssel auswählen")
6. Geben Sie eine Beschreibung in der Spalte Wert die app Warum möchte, auf die angegebene Funktion oder einen Benutzer Informationen zugreifen: 

    ![Geben Sie eine Beschreibung](security-privacy-images/InfoPListSetValue.png "Geben Sie eine Beschreibung")
7. Speichern Sie die Änderungen in der Datei.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Um die Datenschutz-Schlüssel festlegen, führen Sie folgende Schritte aus:

1. Doppelklicken Sie auf die **info.plist**-Datei im **Projektmappen-Explorer**, um sie zu öffnen und zu bearbeiten.
2. Wechseln Sie am unteren Rand des Bildschirms, um die **Quelle** anzeigen.
3. Fügen Sie einen neuen **Eintrag** der Liste.
4. Wählen Sie aus der Dropdownliste aus, die einen Schlüssel (z. B. **Datenschutz – HomeKit-Nutzungsbeschreibung**): 

    ![Wählen Sie einen Schlüssel](security-privacy-images/info02.png "datenschutzschlüssel auswählen")
5. Geben Sie eine Beschreibung für warum die app, die auf die angegebene Informationen zur Funktion oder Benutzer zugreifen möchte: 

    ![Geben Sie eine Beschreibung](security-privacy-images/info03.png "Geben Sie eine Beschreibung")
6. Speichern Sie die Änderungen in der Datei.

-----

> [!IMPORTANT]
> Erhalten Sie im Beispiel oben "," Fehler beim Festlegen der `NSHomeKitUsageDescription` -Schlüssel in der **"Info.plist"** Datei ergibt sich in der app _im Hintergrund fehlerhaften_ (wird vom System zur Laufzeit geschlossen) ohne Fehler bei der Ausführung im iOS 10 ( oder höher).

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden behandelt, die Sicherheit und Datenschutz Änderungen, die Apple iOS 10 und deren Auswirkung auf einer Xamarin.iOS-app gestellt hat.

## <a name="related-links"></a>Verwandte Links

- [iOS 10-Beispiele](https://developer.xamarin.com/samples/ios/iOS10/)
