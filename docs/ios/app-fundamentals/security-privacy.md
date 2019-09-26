---
title: Sicherheits-und Datenschutzfunktionen für IOS
description: In diesem Dokument werden die Sicherheits-und Datenschutzfunktionen von IOS beschrieben und erläutert, wie Sie mit xamarin. IOS verwendet werden. Hier finden Sie Informationen zu den in ios 10 vorgenommenen Updates und zum Zugriff auf private Benutzerdaten.
ms.prod: xamarin
ms.assetid: 718C8721-C359-4650-878A-D68E159A3F53
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/16/2017
ms.openlocfilehash: 7847148551c20dbcf49bcc263bdc50716a6ef14e
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "70283170"
---
# <a name="ios-security-and-privacy-features"></a>Sicherheits-und Datenschutzfunktionen für IOS

_In diesem Artikel wird das Arbeiten mit Sicherheit und Datenschutz in IOS und deren Auswirkungen auf eine xamarin. IOS-App behandelt._

Apple hat mehrere Verbesserungen hinsichtlich Sicherheit und Datenschutz in ios 10 (und höher) vorgenommen, die dem Entwickler dabei helfen, die Sicherheit Ihrer apps zu verbessern und den Datenschutz für den Endbenutzer zu gewährleisten. In diesem Artikel wird die Implementierung dieser Features in einer xamarin. IOS-App behandelt.

<a name="General-Enhancements" />

## <a name="general-enhancements"></a>Allgemeine Erweiterungen

Die folgenden allgemeinen Änderungen wurden an Sicherheit und Datenschutz in ios 10 vorgenommen:

- Die API der Common Data Security Architecture (CDSA) wurde als veraltet markiert und sollte durch die API "-API" ersetzt werden, um asymmetrische Schlüssel zu generieren.
- Der neue `NSAllowsArbitraryLoadsInWebContent` Schlüssel kann der Datei " **Info. plist** " einer app hinzugefügt werden und ermöglicht das ordnungsgemäße Laden von Webseiten, während der Apple Transport Security (ATS)-Schutz für den Rest der APP weiterhin aktiviert ist. Weitere Informationen finden Sie in der Dokumentation zur [App-Transport Sicherheit](~/ios/app-fundamentals/ats.md) .
- Da die neue Zwischenablage in ios 10 und macOS Sierra dem Benutzer das Kopieren und Einfügen zwischen Geräten ermöglicht, wurde die API erweitert, damit eine Zwischenablage auf ein bestimmtes Gerät beschränkt werden kann, und Sie muss mit einem Zeitstempel versehen werden, um an einem bestimmten Punkt automatisch gelöscht zu werden. Außerdem sind benannte pasteboards nicht mehr persistent und sollten durch die freigegebenen Zwischenablage-Container ersetzt werden.
- Für alle SSL/TLS-Verbindungen ist das symmetrische RC4-Chiffre jetzt standardmäßig deaktiviert. Darüber hinaus unterstützt die Secure-Transport-API SSLv3 nicht mehr. es wird empfohlen, dass der Entwickler so bald wie möglich die SHA-1-und 3DES-Kryptografie nicht mehr verwendet.

<a name="Accessing-Private-User-Data" />

## <a name="accessing-private-user-data"></a>Zugreifen auf private Benutzerdaten

Apps, die unter IOS 10 (oder höher) ausgeführt werden, müssen ihre Absicht, auf bestimmte Features oder Benutzerinformationen zuzugreifen, statisch deklarieren, indem Sie einen oder mehrere Datenschutz Schlüssel in Ihren **Info. plist** -Dateien eingeben, die dem Benutzer erklären, warum die APP Zugriff erhalten möchte.

> [!IMPORTANT]
> Apps, die die erforderlichen Schlüssel nicht bereitstellen, werden vom System automatisch beendet, wenn Sie versuchen, auf eine der eingeschränkten Features oder Benutzerinformationen _ohne Fehler_zuzugreifen. Wenn eine APP auf IOS 10 unerwartet ausfällt, stellen Sie sicher, dass die erforderliche Datei " **Info. plist** " angegeben wurde.

Die folgenden Datenschutz bezogenen Schlüssel sind verfügbar:

- **Datenschutz: Nutzungs Beschreibung für Apple Music** (`NSAppleMusicUsageDescription`): Hiermit kann der Entwickler beschreiben, warum die APP auf die Medienbibliothek des Benutzers zugreifen möchte.
- **Datenschutz: Nutzungs Beschreibung für Bluetooth-Peripherie** Geräte (`NSBluetoothPeripheralUsageDescription`): Hiermit kann der Entwickler beschreiben, warum die APP auf Bluetooth auf dem Gerät des Benutzers zugreifen möchte.
- **Datenschutz: Nutzungs Beschreibung für Kalender** (`NSCalendarsUsageDescription`): Hiermit kann der Entwickler beschreiben, warum die APP auf den Kalender des Benutzers zugreifen möchte.
- **Datenschutz: Nutzungs Beschreibung für Kamera** (`NSCameraUsageDescription`): Hiermit kann der Entwickler beschreiben, warum die APP auf die Kamera des Geräts zugreifen möchte.
- **Datenschutz: Nutzungs Beschreibung für Kontakte** (`NSContactsUsageDescription`): Hiermit kann der Entwickler beschreiben, warum die APP auf die Kontakte des Benutzers zugreifen möchte.
- **Datenschutz: Nutzungs Beschreibung** für Integritäts Freigabe (`NSHealthShareUsageDescription`): Hiermit kann der Entwickler beschreiben, warum die APP auf die Integritäts Daten des Benutzers zugreifen möchte. Weitere Informationen finden Sie in der [hkhealthstore-Klassenreferenz](https://developer.apple.com/reference/healthkit/hkhealthstore)von Apple.
- **Datenschutz: Nutzungs Beschreibung** für Integritäts Aktualisierung (`NSHealthUpdateUsageDescription`): Hiermit kann der Entwickler beschreiben, warum die APP die Integritäts Daten des Benutzers bearbeiten möchte. Weitere Informationen finden Sie in der [hkhealthstore-Klassenreferenz](https://developer.apple.com/reference/healthkit/hkhealthstore)von Apple.
- **Datenschutz: Nutzungs Beschreibung für homekit** (`NSHomeKitUsageDescription`): Hiermit kann der Entwickler beschreiben, warum die APP auf die homekit-Konfigurationsdaten des Benutzers zugreifen möchte.
- **Datenschutz-Beschreibung der ortsbasierten Verwendung** (`NSLocationAlwaysUsageDescription`): Hiermit kann der Entwickler beschreiben, warum die APP immer Zugriff auf den Speicherort des Benutzers haben möchte.
- Veraltet **Datenschutz-Beschreibung der Standort Verwendung** (`NSLocationUsageDescription`): Hiermit kann der Entwickler beschreiben, warum die APP auf den Speicherort des Benutzers zugreifen möchte. *HINWEIS: Dieser Schlüssel wurde in ios 8 (und höher) als veraltet markiert. Verwenden `NSLocationAlwaysUsageDescription` Sie `NSLocationWhenInUseUsageDescription` stattdessen oder.*
- **Datenschutz: Nutzungs Beschreibung für "Standort in Verwendung** " (`NSLocationWhenInUseUsageDescription`): Hiermit kann der Entwickler beschreiben, warum die APP während der Ausführung auf den Speicherort des Benutzers zugreifen möchte.
- Veraltet **Datenschutz: Nutzungs Beschreibung für Medienbibliothek** : ermöglicht es dem Entwickler zu beschreiben, warum die APP auf die Medienbibliothek des Benutzers zugreifen möchte. *HINWEIS: Dieser Schlüssel wurde in ios 8 (und höher) als veraltet markiert. Verwenden `NSAppleMusicUsageDescription` Sie stattdessen.*
- **Datenschutz-Beschreibung der Mikrofon Verwendung** (`NSMicrophoneUsageDescription`): Hiermit kann der Entwickler beschreiben, warum die APP auf das Mikrofon zugreifen möchte.
- **Datenschutz: Nutzungs Beschreibung für Motion** (`NSMotionUsageDescription`): Hiermit kann der Entwickler beschreiben, warum die APP auf den Beschleunigungsmesser des Geräts zugreifen möchte.
- **Datenschutz: Nutzungs Beschreibung für Fotobibliothek** (`NSPhotoLibraryUsageDescription`): Hiermit kann der Entwickler beschreiben, warum die APP auf die Fotobibliothek des Benutzers zugreifen möchte.
- **Datenschutz: Nutzungs Beschreibung für Erinnerungen** (`NSRemindersUsageDescription`): Hiermit kann der Entwickler beschreiben, warum die APP auf die Erinnerungen des Benutzers zugreifen möchte.
- **Datenschutz: Nutzungs Beschreibung für Siri** (`NSSiriUsageDescription`): Hiermit kann der Entwickler beschreiben, warum die APP Benutzerdaten an Siri senden möchte.
- **Datenschutz: Nutzungs Beschreibung für Spracherkennung** (`NSSpeechRecognitionUsageDescription`): Hiermit kann der Entwickler beschreiben, warum die APP Benutzerdaten an die sprach Erkennungs Server von Apple senden möchte.
- **Datenschutz: Nutzungs Beschreibung für TV-Anbieter** (`NSVideoSubscriberAccountUsageDescription`): Hiermit kann der Entwickler beschreiben, warum die APP auf das TV-Anbieter Konto des Benutzers zugreifen möchte.

Weitere Informationen zum Arbeiten mit " **Info. plist** "-Schlüsseln finden Sie in der Apple-Referenz für die [Informations Eigenschaften Liste](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009248-SW1).

<a name="Setting-Privacy-Keys" />

## <a name="setting-privacy-keys"></a>Festlegen von Datenschutz Schlüsseln

Betrachten Sie das folgende Beispiel für den Zugriff auf homekit unter IOS 10 (und höher). der Entwickler muss den `NSHomeKitUsageDescription` Schlüssel der Datei " **Info. plist** " der APP hinzufügen und eine Zeichenfolge angeben, die deklariert, warum die APP auf die homekit-Datenbank des Benutzers zugreifen möchte. Diese Zeichenfolge wird dem Benutzer beim ersten Ausführen der App angezeigt:

![Ein Beispiel für eine nshomekitusagedescription-Warnung](security-privacy-images/info01.png "Ein Beispiel für eine nshomekitusagedescription-Warnung")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Xamarin. IOS für Visual Studio unterstützt derzeit nicht die Bearbeitung der Datenschutz Schlüssel " **Info. plist** " aus dem standardmäßigen IOS-Manifest-Editor. Stattdessen müssen Sie den generischen plist-Editor verwenden. führen Sie daher die folgenden Schritte aus:

1. Klicken Sie mit der rechten Maustaste auf die Datei **Info. plist** im **Projektmappen-Explorer** , und wählen Sie **Öffnen mit...** aus.
2. Wählen Sie in der Liste der Programme den **generischen plist-Editor** aus, um die Datei zu öffnen, und klicken Sie auf **OK**.

    ![Auswählen des generischen plist-Editors](security-privacy-images/InfoEditorSelectionVs.png "Auswählen des generischen plist-Editors")
3. Klicken Sie **+** in der letzten Zeile des Editors auf die Schaltfläche, um der Liste einen neuen Eintrag hinzuzufügen. Dies wird als "benutzerdefinierte Eigenschaft" bezeichnet, wobei der Typ auf `String` und ein leerer Wert festgelegt ist.
4. Klicken Sie auf den Eigenschaften Namen, und es wird eine Dropdown Liste angezeigt.
5. Wählen Sie in der Dropdown Liste einen Datenschutz Schlüssel aus (z. b. **Datenschutz: Nutzungs Beschreibung für homekit**): 

    ![Datenschutz Schlüssel auswählen](security-privacy-images/InfoPListEditorSelectKey.png "Datenschutz Schlüssel auswählen")
6. Geben Sie eine Beschreibung in die Spalte Wert ein, um den Grund für den Zugriff der APP auf die angegebene Funktion oder die Benutzerinformationen zu erhalten: 

    ![Beschreibung eingeben](security-privacy-images/InfoPListSetValue.png "Beschreibung eingeben")
7. Speichern Sie die Änderungen in der Datei.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Gehen Sie folgendermaßen vor, um die Datenschutz Schlüssel festzulegen:

1. Doppelklicken Sie auf die **info.plist**-Datei im **Projektmappen-Explorer**, um sie zu öffnen und zu bearbeiten.
2. Wechseln Sie am unteren Bildschirmrand zur **Quell** Ansicht.
3. Fügen Sie der Liste einen neuen **Eintrag** hinzu.
4. Wählen Sie in der Dropdown Liste einen Datenschutz Schlüssel aus (z. b. **Datenschutz: Nutzungs Beschreibung für homekit**): 

    ![Datenschutz Schlüssel auswählen](security-privacy-images/info02.png "Datenschutz Schlüssel auswählen")
5. Geben Sie eine Beschreibung für den Grund für den Zugriff der APP auf die angegebene Funktion oder die Benutzerinformationen an: 

    ![Beschreibung eingeben](security-privacy-images/info03.png "Beschreibung eingeben")
6. Speichern Sie die Änderungen in der Datei.

-----

> [!IMPORTANT]
> Im obigen Beispiel führt das Fehlschlagen `NSHomeKitUsageDescription` des Schlüssels in der **Info. plist** -Datei dazu, dass die APP beim Ausführen in ios 10 (oder höher) ohne Fehler in dem Hintergrund _ausfällt_ (wird vom System zur Laufzeit geschlossen).

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die Sicherheits-und Datenschutzänderungen erläutert, die Apple in ios 10 vorgenommen hat, sowie deren Auswirkung auf eine xamarin. IOS-app.

## <a name="related-links"></a>Verwandte Links

- [IOS 10-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
