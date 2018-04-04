---
title: Einführung in MacOS Sierra
description: In diesem Artikel werden alle neuen und geänderten-APIs und in MacOS Sierra verfügbaren Funktionen für Entwickler Xamarin.Mac eingeführt.
ms.prod: xamarin
ms.assetid: 71A8A737-F310-4320-BD23-743AA1E9033C
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 5ab373d708d47ad7c3dbbf4507284be04a1f9934
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-macos-sierra"></a>Einführung in MacOS Sierra

Mit der neuen MacOS Sierra kann Entwickler neue APIs nutzen, die Endbenutzer zum interagieren mit ihren apps und Websites zuvor nicht verfügbar Möglichkeiten zu ermöglichen. Beispielsweise ermöglicht Apple jetzt Websites so, dass Kunden die Möglichkeit, sicher bezahlen über Apple Pay und Erweiterungen, die die Verstärkung-Metal-Framework ein app-Grafiken und computing erhalten potenzielle. 

Weitere Informationen zu MacOS Sierra, finden Sie im Apple [MacOS + Apps](https://developer.apple.com/macos/) Dokumentation.

<a name="Whats-New-in-macOS-Sierra" />

## <a name="whats-new-in-macos-sierra"></a>Was ist neu in MacOS Sierra

Apple hat mehrere neue APIs und Dienste in MacOS Sierra sowie zahlreiche Verbesserungen an vorhandenen Funktionen, einschließlich hinzugefügt:

<a name="Apple-File-System" />

### <a name="apple-file-system"></a>Apple-Dateisystem

Mit MacOS Sierra wurde das neue Dateisystem von Apple Apple als eine moderne Dateisystem für iOS-, Mac OS, tvos. außerdem wurden und WatchOS freigegeben werden. Das Apple-Dateisystem wurde für Flash- und SSD-Speicher optimiert und bietet die folgenden Funktionen: starke Verschlüsselung, Kopie-bei-Schreibvorgang-Metadaten Speicherplatz freigeben, das Klonen für Dateien und Verzeichnissen, Momentaufnahmen, schnelle Directory Sizing und atomic sicher speichern primitive Typen.

Weitere Informationen finden Sie in der Apple- [Apple File System Guide](https://developer.apple.com/library/prerelease/content/documentation/FileManagement/Conceptual/APFS_Guide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40016999).

<a name="Apple-Pay-Enhancements" />

### <a name="apple-pay-enhancements"></a>Apple Pay-Erweiterungen

Apple hat gestellt mehrere Erweiterungen Apple Pay MacOS Sierra, mit denen den Benutzer zum sicheren Zahlungen Websites stellen können.

Mit MacOS Sierra wurden mehrere neue APIs hinzugefügt, arbeiten mit MacOS Sierra, iOS und WatchOS zur Unterstützung von dynamischen Zahlung Netzwerke und eine neue Sandkasten-testumgebung.

MacOS Sierra enthält das neue ApplePay Javascript-Framework, das den Entwickler mit Apple Pay direkt in iOS und Mac OS Safari-basierte Websites integrieren kann. Für Websites, die Apple Pay unterstützen, kann der Benutzer mit ihrer iPhone oder Apple Watch Zahlung autorisieren.

Weitere Informationen finden Sie in der Apple- [ApplePay JS Framework](https://developer.apple.com/reference/applepayjs) Verweis.

<a name="Building-Modern-macOS-Apps" />

### <a name="building-modern-macos-apps"></a>Erstellen von modernen MacOS Apps

Moderne MacOS-apps wie z. B. Apple Safari-Webbrowser Textverarbeitungsprogramm Seiten und Zahlen Weiterverbreitung Stylesheet verwenden, die viele neue Technologien, um eine einheitliche, kontextbezogene Benutzeroberfläche bereit, die sofort mit herkömmlichen Benutzeroberflächenelemente, z. B. unverankerte Bereiche ist und mehrere öffnen Windows.

[![Ein Beispiel für einen Macintosh-Fenster im Registerkartenformat](images/content08.png)](images/content08.png#lightbox)

Unsere [Erstellen von modernen MacOS Apps](~/mac/platform/introduction-to-macos-sierra/modern-cocoa-apps.md) Anleitung enthält einige Tipps, Funktionen und Techniken ein Entwickler eine moderne MacOS-app in Xamarin.Mac zu erstellen kann.

<a name="CloudKit-Data-Sharing" />

### <a name="cloudkit-data-sharing"></a>CloudKit die Datenfreigabe

Das Framework CloudKit wurde in MacOS Sierra, damit Benutzer schnell und einfach Datensätze oder Datensatzgruppen aus ihren Datenbanken privaten iCloud freigeben kann erweitert.

CloudKit bietet eine vollständige Benutzeroberfläche für das Senden und freigegebene Datensatz Einladungen akzeptieren, und der Benutzer über vollständige Lese-/Schreibzugriff Kontrolle über die Personen, die Zugriff auf die Datensätze haben.

Weitere Informationen finden Sie in der Apple- [CloudKit Frameworkverweis](https://developer.apple.com/reference/clockkit) und [CloudKit JS-Frameworkverweis](https://developer.apple.com/reference/cloudkitjs).

<a name="Safari-App-Extensions-Support" />

### <a name="safari-app-extensions-support"></a>Safari-Support für App-Erweiterungen

Safari-App-Erweiterungen ermöglichen die app, die das Verhalten des Safari-Webbrowser zu erweitern, während die MacOS Sierra eng integriert wird. Da MacOS Safari-App-Erweiterungen wie iOS Safari-App-Erweiterungen zu arbeiten, werden sie einfach an Port von einem System in einen anderen.

Weitere Informationen finden Sie in der Apple- [Safari App-Erweiterung-Programmierhandbuch](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternetWeb/Conceptual/SafariAppExtension_PG/index.html#//apple_ref/doc/uid/TP40017319).

<a name="Security-and-Privacy-Enhancements" />

### <a name="security-and-privacy-enhancements"></a>Sicherheit und Datenschutz-Erweiterungen

Apple hat die verschiedene Verbesserungen vorgenommen, um Sicherheit und Datenschutz für MacOS Sierra, mit deren Hilfe wird die app verbessern Sie die Sicherheit der app, und vergewissern Sie sich der Endbenutzer Datenschutz u. a. folgende:

- Die neue `NSAllowsArbitraryLoadsInWebContent` Schlüssel können der app hinzufügen `Info.plist` Datei und lässt das Webseiten ordnungsgemäß geladen werden, während der Apple-Transport-Sicherheit (ATS)-Schutz für den Rest der app weiterhin aktiviert ist.
- Der allgemeine Data Security Architecture (CDSA) API ist veraltet und sollten ersetzt werden, mit der SecKey-API, um asymmetrische Schlüssel zu generieren.
- Für alle SSL/TLS-Verbindungen wird der symmetrische RC4-Verschlüsselungsverfahren jetzt standardmäßig deaktiviert. Darüber hinaus wird der sichere Transport-API unterstützt nicht mehr SSLv3, und es wird empfohlen, dass die Verwendung von SHA-1 und 3DES Kryptografie so bald wie möglich mit der app zu beenden.
- Da die neue Zwischenablage in iOS 10 und MacOS Sierra ermöglicht es dem Benutzer kopieren und Einfügen zwischen den Geräten, wurde die API erweitert, um eine Zwischenablage auf ein bestimmtes Gerät beschränkt und werden mit einem Zeitstempel versehen, um zu einem bestimmten Zeitpunkt automatisch aufgelöst werden können. Darüber hinaus benannte Montageflächen werden nicht mehr beibehalten und mit den freigegebenen Montagefläche Containern ersetzt werden sollte.
- Wenn die Anwendung (z. B. den Kalender des Benutzers), geschützte Daten zugreift es _müssen_ deklarieren diese Absicht mit der richtigen Zweck Wert Zeichenfolgenschlüssel in seiner `Info.plist` Datei (`NSCalendarUsageDescription` im Fall der Kalender).
- Entwickler Signed apps, die nicht über Mac App Store übermittelt werden können jetzt CloudKit, iCloud Schlüsselbund, iCloud Laufwerk, remote Pushbenachrichtigungen, MapKit und VPN-Berechtigungen nutzen.
- MacOS Sierra wird nicht mehr unterstützt, externe Code- oder Datenmenge zusammen mit der app Code Unterzeichner in der Zip-Archiv oder ohne Vorzeichen Datenträger-Image bereitstellen, wie die Common Language Runtime-Pfad vor der Laufzeit unbekannt ist.

Darüber hinaus die apps, die auf MacOS Sierra ausgeführt wird (oder höher) muss statisch deklariert werden ihre Absicht auf bestimmte Funktionen oder Benutzerinformationen zugreifen, indem die Eingabe eine oder mehrere bestimmte Datenschutz-Schlüssel in ihre `Info.plist` Dateien, die dem Benutzer erklären, warum die app zu erhalten möchte, Zugriff.

Da diese Änderungen mit iOS 10 MacOS Sierra freigegeben hat, finden Sie unter unserer iOS 10 [Sicherheit und Datenschutz Erweiterungen](~/ios/app-fundamentals/security-privacy.md) Weitere Informationen.

<a name="Smart-Card-Driver-Extension-Support" />

### <a name="smart-card-driver-extension-support"></a>Smartcard-Erweiterung Treiberunterstützung

Erstellen der app kann mit MacOS Sierra, `NSExtension` basierten Smartcard-Treiber, die nur-Lese-Zugriff auf den Inhalt von bestimmten Typen von Smartcards ermöglicht. Diese Informationen werden dann in der System-Schlüsselbund (ersetzt die veraltete allgemeine Daten-Sicherheitsarchitektur-Methode) präsentiert.

Weitere Informationen Pleas finden Sie Apple des [CryptoTokenKit Frameworkverweis](https://developer.apple.com/reference/cryptotokenkit).

<a name="Unified-Logging" />

### <a name="unified-logging"></a>Einheitliche Protokollierung

Einheitliche Protokollierung bietet die app mit einer einzelnen API für die effiziente messaging über alle Ebenen des Systems an. Mit Unified Logging hat die app eine präzisere Kontrolle über mehrere Ebenen von Protokollierung, die Datenschutz-Steuerelemente und aktivitätsnachverfolgung zum einfacheren Debuggen enthalten. 

Protokollierung bietet automatische Nachrichtenkorrelation, wenn Aktivitäten verfolgt und protokolliert zusammen verwendet werden.

MacOS Sierra umfasst eine neue Konsolenanwendung (in Applications/Utilities), die Protokolldaten aus mehreren Quellen einschließlich verbundene Geräte anzeigen kann. Außerdem unterstützt mit Token versehene und gespeicherte Suchvorgänge und Verbindungen zwischen zusammengehöriger Nachrichten für mehrere Prozesse angezeigt.

Darüber hinaus protokollmeldungen können angezeigt werden und von Befehlszeilentools beibehalten.

Weitere Informationen finden Sie in der Apple- [Logging Reference](https://developer.apple.com/reference/os/1891852-logging).

<a name="Wide-Color" />

### <a name="wide-color"></a>Breite Farbskala

MacOS Sierra erweitert die Unterstützung für erweiterte Schlüsselbereich Pixelformate und Breite Farbskala Farbräumen im gesamten System einschließlich Frameworks wie z. B. Grafiken Core und Core-Image, Metall AVFoundation. Unterstützung für Geräte mit Breite Farbskala zeigt wird weiter Dienstverweise durch dieses Verhalten im ganzen Grafikstapel gesamte bereitstellen.

Darüber hinaus `AppKit` wurde so geändert, arbeiten Sie in der neuen erweiterten **sRGB** Farbraum, die zum Mischen von Farben in wide Farbumfänge ohne signifikante Leistungseinbußen zu erleichtern.

Apple bietet die folgenden bewährten Methoden bei der Arbeit mit breiten Farben:

- `NSColor` nun verwendet der sRGB Farbspektrum und wird nicht mehr Werte clamp der `0.0` zu `1.0` Bereich. Wenn die app vom vorherigen Clamp Verhalten basiert, müssen sie für MacOS Sierra geändert werden.
- Beim Verwenden einer Low-Level-API z. B. Core Grafiken oder Metall, das eine bildverarbeitung bereitzustellen, sollten die app einen erweiterten Bereich Farbe Speicherplatz und Pixel-Format verwenden, 16-Bit-Gleitkommawerte unterstützt, werden kann. Gegebenenfalls müssen die app manuell Komponente Farbwerte clamp.
- Core-Grafiken, Core-Image und Metall Leistung Shader alle neue Methoden für die Konvertierung zwischen den zwei Farbräumen bereitstellen.

Wenn Sie mehr erfahren möchten, finden Sie unter unsere [Einführung in die Breite Farbskala](~/ios/platform/wide-color.md) Handbuch.

<a name="Additional-Framework-Changes" />

## <a name="additional-framework-changes"></a>Zusätzliche Framework-Änderungen

Zusätzlich zu den wichtigsten Framework Änderungen und Ergänzungen, die oben aufgeführten verfügt über Apple viele zusätzliche kleinere Framework MacOS Sierra geändert.

Wenn Sie mehr erfahren möchten, finden Sie unter unsere [zusätzliche Framework Änderungen](~/mac/platform/introduction-to-macos-sierra/additional-framework-changes.md) Handbuch.

<a name="Deprecated-APIs" />

## <a name="deprecated-apis"></a>Nicht mehr unterstützte APIs

Die folgenden APIs sind in MacOS Sierra veraltet:

- Die HFS Standard im Dateisystem wird nicht mehr unterstützt.

Finden Sie in der Apple- [v10.12 für OS X-API Diffs](https://developer.apple.com/library/prerelease/content/releasenotes/Miscellaneous/APIDiffsMacOS10_12/index.html) Dokumentation für eine vollständige Liste der veralterungen und Änderungen.

## <a name="related-links"></a>Verwandte Links

- [Mac-Beispiele](https://developer.xamarin.com/samples/mac/)
- [Was ist neu in OS X 10.12](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
