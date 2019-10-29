---
title: Sicherheits-und Datenschutzfunktionen für IOS
description: In diesem Dokument werden die Sicherheits-und Datenschutzfunktionen von IOS beschrieben und erläutert, wie Sie mit xamarin. IOS verwendet werden. Hier finden Sie Informationen zu den in ios 10 vorgenommenen Updates und zum Zugriff auf private Benutzerdaten.
ms.prod: xamarin
ms.assetid: 718C8721-C359-4650-878A-D68E159A3F53
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: cdc59c4eb3863435482acf5eb0dcd1368beea693
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73009660"
---
# <a name="ios-security-and-privacy-features"></a>Sicherheits-und Datenschutzfunktionen für IOS

_In diesem Artikel wird das Arbeiten mit Sicherheit und Datenschutz in IOS und deren Auswirkungen auf eine xamarin. IOS-App behandelt._

Apple hat mehrere Verbesserungen hinsichtlich Sicherheit und Datenschutz in ios 10 (und höher) vorgenommen, die dem Entwickler dabei helfen, die Sicherheit Ihrer apps zu verbessern und den Datenschutz für den Endbenutzer zu gewährleisten. In diesem Artikel wird die Implementierung dieser Features in einer xamarin. IOS-App behandelt.

<a name="General-Enhancements" />

## <a name="general-enhancements"></a>Allgemeine Erweiterungen

Die folgenden allgemeinen Änderungen wurden an Sicherheit und Datenschutz in ios 10 vorgenommen:

- Die API der Common Data Security Architecture (CDSA) wurde als veraltet markiert und sollte durch die API "-API" ersetzt werden, um asymmetrische Schlüssel zu generieren.
- Der neue `NSAllowsArbitraryLoadsInWebContent` Schlüssel kann der Datei " **Info. plist** " einer app hinzugefügt werden und ermöglicht das ordnungsgemäße Laden von Webseiten, während der Apple Transport Security (ATS)-Schutz weiterhin für den Rest der App aktiviert ist. Weitere Informationen finden Sie in der Dokumentation zur [App-Transport Sicherheit](~/ios/app-fundamentals/ats.md) .
- Da die neue Zwischenablage in ios 10 und macOS Sierra dem Benutzer das Kopieren und Einfügen zwischen Geräten ermöglicht, wurde die API erweitert, damit eine Zwischenablage auf ein bestimmtes Gerät beschränkt werden kann, und Sie muss mit einem Zeitstempel versehen werden, um an einem bestimmten Punkt automatisch gelöscht zu werden. Außerdem sind benannte pasteboards nicht mehr persistent und sollten durch die freigegebenen Zwischenablage-Container ersetzt werden.
- Für alle SSL/TLS-Verbindungen ist das symmetrische RC4-Chiffre jetzt standardmäßig deaktiviert. Darüber hinaus unterstützt die Secure-Transport-API SSLv3 nicht mehr. es wird empfohlen, dass der Entwickler so bald wie möglich die SHA-1-und 3DES-Kryptografie nicht mehr verwendet.

<a name="Accessing-Private-User-Data" />

## <a name="accessing-private-user-data"></a>Zugreifen auf private Benutzerdaten

Apps, die unter IOS 10 (oder höher) ausgeführt werden, müssen ihre Absicht, auf bestimmte Features oder Benutzerinformationen zuzugreifen, statisch deklarieren, indem Sie einen oder mehrere Datenschutz Schlüssel in Ihren **Info. plist** -Dateien eingeben, die dem Benutzer erklären, warum die APP Zugriff erhalten möchte.

> [!IMPORTANT]
> Apps, die die erforderlichen Schlüssel nicht bereitstellen, werden vom System automatisch beendet, wenn Sie versuchen, auf eine der eingeschränkten Features oder Benutzerinformationen _ohne Fehler_zuzugreifen. Wenn eine APP auf IOS 10 unerwartet ausfällt, stellen Sie sicher, dass die erforderliche Datei " **Info. plist** " angegeben wurde.

Die folgenden Datenschutz bezogenen Schlüssel sind verfügbar:

- **Datenschutz-Apple Music Verwendungs Beschreibung** (`NSAppleMusicUsageDescription`): ermöglicht es dem Entwickler zu beschreiben, warum die APP auf die Medienbibliothek des Benutzers zugreifen möchte.
- **Datenschutz: Nutzungs Beschreibung für Bluetooth-Peripherie** Geräte (`NSBluetoothPeripheralUsageDescription`): ermöglicht es dem Entwickler zu beschreiben, warum die APP auf Bluetooth auf dem Gerät des Benutzers zugreifen möchte.
- **Datenschutz: Nutzungs Beschreibung für Kalender** (`NSCalendarsUsageDescription`): ermöglicht es dem Entwickler zu beschreiben, warum die APP auf den Kalender des Benutzers zugreifen möchte.
- **Datenschutz: Nutzungs Beschreibung für Kamera** (`NSCameraUsageDescription`): ermöglicht es dem Entwickler zu beschreiben, warum die APP auf die Kamera des Geräts zugreifen möchte.
- **Datenschutz: Nutzungs Beschreibung für Kontakte** (`NSContactsUsageDescription`): ermöglicht es dem Entwickler zu beschreiben, warum die APP auf die Kontakte des Benutzers zugreifen möchte.
- **Datenschutz: Nutzungs Beschreibung** für Integritäts Freigabe (`NSHealthShareUsageDescription`): ermöglicht es dem Entwickler zu beschreiben, warum die APP auf die Integritäts Daten des Benutzers zugreifen möchte. Weitere Informationen finden Sie in der [hkhealthstore-Klassenreferenz](https://developer.apple.com/reference/healthkit/hkhealthstore)von Apple.
- **Datenschutz: Nutzungs Beschreibung** für Integritäts Aktualisierung (`NSHealthUpdateUsageDescription`): ermöglicht es dem Entwickler zu beschreiben, warum die APP die Integritäts Daten des Benutzers bearbeiten möchte. Weitere Informationen finden Sie in der [hkhealthstore-Klassenreferenz](https://developer.apple.com/reference/healthkit/hkhealthstore)von Apple.
- **Datenschutz: Nutzungs Beschreibung für homekit** (`NSHomeKitUsageDescription`): ermöglicht es dem Entwickler zu beschreiben, warum die APP auf die homekit-Konfigurationsdaten des Benutzers zugreifen möchte.
- **Datenschutz-Beschreibung der Speicherorte Always Usage** (`NSLocationAlwaysUsageDescription`): ermöglicht es dem Entwickler zu beschreiben, warum die APP stets Zugriff auf den Speicherort des Benutzers haben möchte.
- Veraltet **Datenschutz-Beschreibung der Orts Nutzung** (`NSLocationUsageDescription`): ermöglicht es dem Entwickler zu beschreiben, warum die APP auf den Benutzer Speicherort zugreifen möchte. *Hinweis: dieser Schlüssel wurde in ios 8 (und höher) als veraltet markiert. Verwenden Sie stattdessen `NSLocationAlwaysUsageDescription` oder `NSLocationWhenInUseUsageDescription`.*
- **Datenschutz-Beschreibung der Verwendung von "Speicherort in Verwendung** " (`NSLocationWhenInUseUsageDescription`): ermöglicht es dem Entwickler zu beschreiben, warum die APP während der Ausführung auf den Speicherort des Benutzers zugreifen möchte.
- Veraltet **Datenschutz: Nutzungs Beschreibung für Medienbibliothek** : ermöglicht es dem Entwickler zu beschreiben, warum die APP auf die Medienbibliothek des Benutzers zugreifen möchte. *Hinweis: dieser Schlüssel wurde in ios 8 (und höher) als veraltet markiert. Verwenden Sie stattdessen `NSAppleMusicUsageDescription`.*
- **Datenschutz-Beschreibung der Mikrofon Verwendung** (`NSMicrophoneUsageDescription`): ermöglicht es dem Entwickler zu beschreiben, warum die APP auf das Mikrofon zugreifen möchte.
- **Datenschutz-Beschreibung der Nutzungs Beschreibung** (`NSMotionUsageDescription`): ermöglicht es dem Entwickler zu beschreiben, warum die APP auf den Beschleunigungsmesser des Geräts zugreifen möchte.
- **Datenschutz: Verwendungs Beschreibung der Fotobibliothek** (`NSPhotoLibraryUsageDescription`): ermöglicht es dem Entwickler zu beschreiben, warum die APP auf die Fotobibliothek des Benutzers zugreifen möchte.
- **Datenschutz: Nutzungs Beschreibung für Erinnerungen** (`NSRemindersUsageDescription`): ermöglicht es dem Entwickler zu beschreiben, warum die APP auf die Erinnerungen des Benutzers zugreifen möchte.
- **Datenschutz: Nutzungs Beschreibung für Siri** (`NSSiriUsageDescription`): ermöglicht es dem Entwickler zu beschreiben, warum die APP Benutzerdaten an Siri senden möchte.
- **Datenschutz: Nutzungs Beschreibung für Spracherkennung** (`NSSpeechRecognitionUsageDescription`): ermöglicht es dem Entwickler zu beschreiben, warum die APP Benutzerdaten an die sprach Erkennungs Server von Apple senden möchte.
- **Datenschutz: Nutzungs Beschreibung für den TV-Anbieter** (`NSVideoSubscriberAccountUsageDescription`): ermöglicht es dem Entwickler zu beschreiben, warum die APP auf das TV-Anbieter Konto des Benutzers zugreifen möchte.

Weitere Informationen zum Arbeiten mit " **Info. plist** "-Schlüsseln finden Sie in der Apple-Referenz für die [Informations Eigenschaften Liste](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009248-SW1).

<a name="Setting-Privacy-Keys" />

## <a name="setting-privacy-keys"></a>Festlegen von Datenschutz Schlüsseln

Im folgenden Beispiel für den Zugriff auf homekit unter IOS 10 (und höher) muss der Entwickler der Datei " **Info. plist** " der APP den `NSHomeKitUsageDescription` Schlüssel hinzufügen und eine Zeichenfolge angeben, die erklärt, warum die APP auf die homekit-Datenbank des Benutzers zugreifen möchte. Diese Zeichenfolge wird dem Benutzer beim ersten Ausführen der App angezeigt:

![Ein Beispiel für eine nshomekitusagedescription-Warnung](security-privacy-images/info01.png "Ein Beispiel für eine nshomekitusagedescription-Warnung")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Xamarin. IOS für Visual Studio unterstützt derzeit nicht die Bearbeitung der Datenschutz Schlüssel " **Info. plist** " aus dem standardmäßigen IOS-Manifest-Editor. Stattdessen müssen Sie den generischen plist-Editor verwenden. führen Sie daher die folgenden Schritte aus:

1. Klicken Sie mit der rechten Maustaste auf die Datei **Info. plist** im **Projektmappen-Explorer** , und wählen Sie **Öffnen mit...** aus.
2. Wählen Sie in der Liste der Programme den **generischen plist-Editor** aus, um die Datei zu öffnen, und klicken Sie auf **OK**.

    ![Auswählen des generischen plist-Editors](security-privacy-images/InfoEditorSelectionVs.png "Auswählen des generischen plist-Editors")
3. Klicken Sie in der letzten Zeile des Editors auf die Schaltfläche **+** , um der Liste einen neuen Eintrag hinzuzufügen. Dies wird als "benutzerdefinierte Eigenschaft" bezeichnet, wobei der Typ auf "`String`" und einen leeren Wert festgelegt ist.
4. Klicken Sie auf den Eigenschaften Namen, und es wird eine Dropdown Liste angezeigt.
5. Wählen Sie in der Dropdown Liste einen Datenschutz Schlüssel aus (z. b. **Datenschutz: Nutzungs Beschreibung für homekit**): 

    ![Datenschutz Schlüssel auswählen](security-privacy-images/InfoPListEditorSelectKey.png "Datenschutz Schlüssel auswählen")
6. Geben Sie eine Beschreibung in die Spalte Wert ein, um den Grund für den Zugriff der APP auf die angegebene Funktion oder die Benutzerinformationen zu erhalten: 

    ![Beschreibung eingeben](security-privacy-images/InfoPListSetValue.png "Geben Sie eine Beschreibung ein")
7. Speichern Sie die Änderungen in der Datei.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Gehen Sie folgendermaßen vor, um die Datenschutz Schlüssel festzulegen:

1. Doppelklicken Sie auf die **info.plist**-Datei im **Projektmappen-Explorer**, um sie zu öffnen und zu bearbeiten.
2. Wechseln Sie am unteren Bildschirmrand zur **Quell** Ansicht.
3. Fügen Sie der Liste einen neuen **Eintrag** hinzu.
4. Wählen Sie in der Dropdown Liste einen Datenschutz Schlüssel aus (z. b. **Datenschutz: Nutzungs Beschreibung für homekit**): 

    ![Datenschutz Schlüssel auswählen](security-privacy-images/info02.png "Datenschutz Schlüssel auswählen")
5. Geben Sie eine Beschreibung für den Grund für den Zugriff der APP auf die angegebene Funktion oder die Benutzerinformationen an: 

    ![Beschreibung eingeben](security-privacy-images/info03.png "Geben Sie eine Beschreibung ein")
6. Speichern Sie die Änderungen in der Datei.

-----

> [!IMPORTANT]
> Im obigen Beispiel führt das Fehlschlagen des `NSHomeKitUsageDescription` Schlüssels in der **Info. plist** -Datei dazu, dass die APP beim Ausführen in ios _10 (oder_ höher) ohne Fehler in den Hintergrund wechselt (durch das System zur Laufzeit geschlossen).

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die Sicherheits-und Datenschutzänderungen erläutert, die Apple in ios 10 vorgenommen hat, sowie deren Auswirkung auf eine xamarin. IOS-app.

## <a name="related-links"></a>Verwandte Links

- [IOS 10-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
