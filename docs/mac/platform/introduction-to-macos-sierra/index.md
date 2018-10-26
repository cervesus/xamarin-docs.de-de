---
title: Einführung in MacOS Sierra
description: Dieser Artikel enthält alle neuen und geänderten APIs und Features in MacOS Sierra verfügbar für Xamarin.Mac-Entwickler.
ms.prod: xamarin
ms.assetid: 71A8A737-F310-4320-BD23-743AA1E9033C
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 5a944fd8f7dcfdcbb3f025c92b4afac35673416f
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50111011"
---
# <a name="introduction-to-macos-sierra"></a>Einführung in MacOS Sierra

Mit der neuen MacOS Sierra kann Entwickler neue APIs nutzen, mit denen der Endbenutzer mit ihren apps und Websites auf zuvor nicht verfügbar Weise interagieren können. Beispielsweise kann Apple jetzt Websites Kunden die Möglichkeit, bezahlen sich sicher über Apple Pay und Verbesserungen an der Boost-Metal-Framework der app-Grafiken und computing bieten mögliche. 

Weitere Informationen unter MacOS Sierra finden Sie unter Apple [MacOS + Apps](https://developer.apple.com/macos/) Dokumentation.

<a name="Whats-New-in-macOS-Sierra" />

## <a name="whats-new-in-macos-sierra"></a>Was ist neu unter MacOS Sierra

Apple hat verschiedene neue APIs und Dienste unter MacOS Sierra sowie zahlreiche Verbesserungen an vorhandenen Funktionen, einschließlich hinzugefügt:

<a name="Apple-File-System" />

### <a name="apple-file-system"></a>Apple-Dateisystem

Mit MacOS Sierra hat Apple das neue Apple File System als moderne Dateisystem für iOS, MacOS, TvOS und WatchOS veröffentlicht. Das Apple-Dateisystem wurde für Flash- und SSD-Speicher optimiert und bietet die folgenden Features: starke Verschlüsselung, Kopie-bei-Schreibvorgang-Metadaten, Speicherplatz freizugeben und das Klonen für Dateien und Verzeichnisse, Momentaufnahmen, schnelle Directory größenanpassung und primitive für atomare sicher speichern.

Weitere Informationen finden Sie unter Apple [Apple File System Guide](https://developer.apple.com/library/prerelease/content/documentation/FileManagement/Conceptual/APFS_Guide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40016999).

<a name="Apple-Pay-Enhancements" />

### <a name="apple-pay-enhancements"></a>Apple Pay-Verbesserungen

Apple hat mehrere Erweiterungen zu Apple Pay in MacOS Sierra, die der Benutzer auf sichere Zahlungen von Websites haben kann.

Mit MacOS Sierra wurden mehrere neue APIs mit MacOS Sierra, iOS und WatchOS zur Unterstützung von dynamischen Zahlung Netzwerke und eine neue Sandkasten-Test-Umgebung hinzugefügt.

MacOS Sierra enthält das neue ApplePay Javascript-Framework, das dem Entwickler erlaubt, Apple Pay direkt in iOS und MacOS Safari-basierte Websites zu integrieren. Für Websites, die Apple Pay unterstützen, kann der Benutzer Zahlungen, die mit ihrer iPhone oder Apple Watch autorisieren.

Weitere Informationen finden Sie unter Apple [ApplePay JS-Framework](https://developer.apple.com/reference/applepayjs) Verweis.

<a name="Building-Modern-macOS-Apps" />

### <a name="building-modern-macos-apps"></a>Erstellen moderner MacOS-Apps

Moderner MacOS-apps wie z. B. Apple Safari-Webbrowser, Seiten-Textverarbeitung und Zahlen Spread-Tabelle verwenden, die viele neue Technologien, die eine einheitliche, kontextabhängige-Benutzeroberfläche anzeigen, die sofort mit herkömmlichen UI-Elementen, z. B. unverankerte Bereiche und mehrere öffnen Windows.

[![Ein Beispiel für ein Mac-Fenster im Registerkartenformat](images/content08.png)](images/content08.png#lightbox)

Unsere [erstellen moderner MacOS-Apps](~/mac/platform/introduction-to-macos-sierra/modern-cocoa-apps.md) Leitfaden wird beschrieben, einige Tipps, Funktionen und Techniken ein Entwickler Sie eine moderner MacOS-app in Xamarin.Mac zu erstellen können.

<a name="CloudKit-Data-Sharing" />

### <a name="cloudkit-data-sharing"></a>Gemeinsame Nutzung von CloudKit-Daten

Das Framework CloudKit wurde erweitert, unter MacOS Sierra, damit Benutzer schnell und einfach Datensätze oder Ressourceneintragssätze aus ihren privaten iCloud-Datenbanken gemeinsam nutzen kann.

CloudKit bietet eine umfassende Benutzeroberfläche zum Senden und annehmen von freigegebenen Datensatz Einladungen und der Benutzer hat die vollständige Lese-/Schreibzugriff-Kontrolle über die Personen, die Zugriff auf die Einträge haben.

Weitere Informationen finden Sie unter Apple [CloudKit Frameworkverweis](https://developer.apple.com/reference/clockkit) und [CloudKit JS-Frameworkverweis](https://developer.apple.com/reference/cloudkitjs).

> [!IMPORTANT]
> Apple [stellt Tools zur Verfügung](https://developer.apple.com/support/allowing-users-to-manage-data/), die Entwickler dabei unterstützen, die Datenschutz-Grundverordnung (DSGVO) der Europäischen Union umzusetzen.

<a name="Safari-App-Extensions-Support" />

### <a name="safari-app-extensions-support"></a>Unterstützung für die Erweiterungen des Safari-App

Safari-App-Erweiterungen ermöglichen die app, die das Verhalten des Safari-Webbrowser erweitern und gleichzeitig die eng mit MacOS Sierra integriert wird. Da Safari-App-Erweiterungen für MacOS ähnlich wie iOS Safari-App-Erweiterungen zu arbeiten, sind sie leicht an Port von einem System in einen anderen.

Weitere Informationen finden Sie unter Apple [Safari-App-Erweiterung Programming Guide](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternetWeb/Conceptual/SafariAppExtension_PG/index.html#//apple_ref/doc/uid/TP40017319).

<a name="Security-and-Privacy-Enhancements" />

### <a name="security-and-privacy-enhancements"></a>Sicherheit und Datenschutz-Verbesserungen

Apple hat verschiedene Verbesserungen vorgenommen, sowohl Sicherheit und Datenschutz in MacOS Sierra, mit deren Hilfe wird die app verbessern Sie die Sicherheit der app und stellen Sie sicher End die Privatsphäre des Benutzers einschließlich der folgenden:

- Die neue `NSAllowsArbitraryLoadsInWebContent` Schlüssel können in der app hinzugefügt werden `Info.plist` Datei und ermöglicht die Webseiten ordnungsgemäß geladen werden, während die Apple Transport Security (ATS) Schutz für den Rest der app immer noch aktiviert ist.
- Die allgemeine Data Security Architecture (CDSA) API ist veraltet und sollte ersetzt werden, mit der SecKey-API, um asymmetrischen Schlüssel zu generieren.
- Für alle SSL/TLS-Verbindungen ist das symmetrische RC4-Verschlüsselungsverfahren jetzt standardmäßig deaktiviert. Darüber hinaus den sicheren Transport-API unterstützt nicht mehr SSLv3, und es wird empfohlen, dass die app mithilfe von SHA-1 und 3DES Kryptografie so bald wie möglich zu beenden.
- Da der neue Zwischenablage in iOS 10 und MacOS Sierra ermöglicht es dem Benutzer zum Kopieren und Einfügen zwischen Geräten, wurde die API erweitert, um ermöglichen eine Zwischenablage, um auf ein bestimmtes Gerät beschränkt, und mit einem Zeitstempel, zu einem bestimmten Zeitpunkt automatisch gelöscht werden sollen. Darüber hinaus werden die benannten Montageflächen werden nicht mehr beibehalten und mit den freigegebenen Montagefläche Containern ersetzt werden soll.
- Wenn die app die geschützten Daten (z. B. den Kalender des Benutzers), greift auf sie _muss_ diese Absicht zu deklarieren, mit dem richtigen Zweck Zeichenfolge-Wert-Schlüssel in der `Info.plist` Datei (`NSCalendarUsageDescription` im Fall von dem Kalender).
- Entwickler Signed-apps, die nicht über den Mac App Store übermittelt werden können jetzt CloudKit, iCloud Keychain, iCloud Drive, remote-Push-Benachrichtigungen, MapKit und VPN-Berechtigungen nutzen.
- MacOS Sierra wird nicht mehr unterstützt, externen Code oder Daten zusammen mit der app-Code-Signaturgeber der Zip-Archiv oder ohne Vorzeichen Datenträger-Image bereitstellen, wie die Runtime-Verzeichnis vor der Laufzeit nicht bekannt ist.

Darüber hinaus die apps, die unter MacOS Sierra (oder höher) muss statisch deklarieren, die Absicht, bestimmte Features oder Benutzerinformationen zugreifen, indem Sie die Eingabe eine oder mehrere bestimmte Datenschutzschlüssel in ihre `Info.plist` Dateien, die dem Benutzer erklären, warum die app zu erhalten möchte, Zugriff.

Da diese Änderungen mit iOS 10 MacOS Sierra freigegeben hat, finden Sie unserem iOS 10 [Verbesserungen Sicherheit und Datenschutz](~/ios/app-fundamentals/security-privacy.md) Anleitung finden Sie weitere Informationen.

<a name="Smart-Card-Driver-Extension-Support" />

### <a name="smart-card-driver-extension-support"></a>Unterstützung von Smartcard-Treiber

Erstellen der app kann mit MacOS Sierra `NSExtension` basierend der Smartcard-Treiber, die nur-Lese Zugriff auf den Inhalt vor bestimmten Arten von Smartcards ermöglicht. Diese Informationen werden dann in der System-Schlüsselbund (ersetzen Sie die veraltete gemeinsame Datenarchitektur Security-Methode) angezeigt.

Weitere Informationen Pleas finden Sie Apple [CryptoTokenKit Frameworkverweis](https://developer.apple.com/reference/cryptotokenkit).

<a name="Unified-Logging" />

### <a name="unified-logging"></a>Vereinheitlichter Protokollierungsebene

Vereinheitlichter Protokollierungsebene bietet die app mit nur einer API für die effiziente Nachrichten auf allen Ebenen des Systems. Mit Unified-Protokollierung hat die app eine präzisere Kontrolle über mehrere Ebenen der Protokollierung, die Steuerung des Datenschutzes und aktivitätsüberwachung zum einfacheren Debuggen enthalten. 

Protokollierung erhalten Sie automatische Nachrichtenkorrelation bei der Aktivität, die nachverfolgung und Protokollierung zusammen verwendet werden.

MacOS Sierra umfasst eine neue Konsolen-App (in Anwendungen/Hilfsprogramme), die zum Anzeigen von Protokolldaten aus mehreren Quellen, einschließlich der verbundenen Geräte kann. Außerdem unterstützt mit Token versehene und gespeicherte Suchvorgänge und Verbindungen zwischen verknüpften Nachrichten über mehrere Prozesse angezeigt.

Darüber hinaus werden die Meldungen angezeigt werden können und verwaltet wird, mithilfe der Befehlszeilentools.

Weitere Informationen finden Sie unter Apple [Protokollierung Verweis](https://developer.apple.com/reference/os/1891852-logging).

<a name="Wide-Color" />

### <a name="wide-color"></a>Breite Farbskala

MacOS Sierra erweitert die Unterstützung für erweiterte-Range-Pixelformate "und" Wide-Farbskala Farbräume im gesamten System einschließlich Frameworks wie Core Graphics "," Core-Image ","-Metal-Computern "und" AVFoundation. Unterstützung für Geräte mit Breite Farbskala zeigt wird weiter durch die Bereitstellung dieses Verhalten in der gesamten Grafikstapel geringer.

Darüber hinaus `AppKit` wurde geändert, arbeiten Sie in der neuen erweiterten **sRGB** Farbraum, erleichtert Ihnen die zum Mischen von Farben in der Breite Farbskala Farbumfänge ohne signifikante Leistungseinbußen zur Folge.

Apple bietet die folgenden bewährten Methoden bei der Arbeit mit breiten Farben:

- `NSColor` nun verwendet der sRGB-Farbraum und nicht mehr-Werte Clamp, die `0.0` zu `1.0` Bereich. Wenn die app vom vorherigen clamps-Verhalten basiert, müssen sie für MacOS Sierra geändert werden.
- Eine Low-Level-API, z. B. die wichtigste Grafik oder Metall mit bildverarbeitung bereitstellen, sollte die app eine größere Anzahl Farbe Speicherplatz und Pixel-Format verwenden, 16-Bit-Gleitkommawerte unterstützt, werden kann. Bei Bedarf müssen die app manuell Farbkomponentenwerte clamp.
- Wichtigste Grafik "," Core-Image "und"-Metal-Leistung Shader neue Methoden bieten für die Konvertierung zwischen den zwei Farbräume ".

Wenn Sie mehr erfahren möchten, informieren Sie sich unsere [Einführung in die Breite Farbskala](~/ios/platform/wide-color.md) Guide.

<a name="Additional-Framework-Changes" />

## <a name="additional-framework-changes"></a>Zusätzliche-Frameworkänderungen

Zusätzlich zu den wichtigsten Framework-Änderungen und Ergänzungen, die oben aufgeführten hat Apple viele weitere kleinere frameworkänderungen unter MacOS Sierra vorgenommen.

Um mehr zu erfahren, informieren Sie sich unsere [zusätzliche-Frameworkänderungen](~/mac/platform/introduction-to-macos-sierra/additional-framework-changes.md) Guide.

<a name="Deprecated-APIs" />

## <a name="deprecated-apis"></a>Nicht mehr unterstützte APIs

Die folgenden APIs wurden in MacOS Sierra veraltet:

- Das HFS Standard-Dateisystem wird nicht mehr unterstützt.

Finden Sie unter Apple [v10.12 für OS X-API-Unterschiede](https://developer.apple.com/library/prerelease/content/releasenotes/Miscellaneous/APIDiffsMacOS10_12/index.html) Dokumentation für eine vollständige Liste der veralteten und Änderungen.

## <a name="related-links"></a>Verwandte Links

- [Mac-Beispiele](https://developer.xamarin.com/samples/mac/)
- [Neuerungen in OS X 10.12](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
