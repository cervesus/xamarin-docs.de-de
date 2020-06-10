---
title: Einführung in macOS Sierra
description: In diesem Artikel werden alle neuen und geänderten APIs und Features vorgestellt, die in macOS Sierra für xamarin. Mac-Entwickler zur Verfügung stehen.
ms.prod: xamarin
ms.assetid: 71A8A737-F310-4320-BD23-743AA1E9033C
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 4dde8941b09ac7b235b94e73e86dc60167fb6bd4
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84574456"
---
# <a name="introduction-to-macos-sierra"></a>Einführung in macOS Sierra

Mit dem neuen macOS Sierra kann der Entwickler neue APIs nutzen, die dem Endbenutzer die Interaktion mit ihren apps und Websites auf zuvor nicht verfügbare Weise ermöglichen. Beispielsweise ermöglicht Apple es Websites, Kunden die Möglichkeit zu geben, sicher über Apple Pay und Erweiterungen des Metal-Frameworks zu profitieren, um die Grafik und das computingpotenzial einer APP zu verbessern. 

Weitere Informationen zu macOS Sierra finden Sie in der Dokumentation zu [macOS und apps](https://developer.apple.com/macos/) von Apple.

<a name="Whats-New-in-macOS-Sierra"></a>

## <a name="whats-new-in-macos-sierra"></a>Neues in macOS Sierra

Apple hat mehrere neue APIs und Dienste in macOS Sierra zusammen mit vielen Verbesserungen an vorhandenen Features hinzugefügt, einschließlich:

<a name="Apple-File-System"></a>

### <a name="apple-file-system"></a>Apple-Datei System

Mit macOS Sierra hat Apple das neue Apple-Dateisystem als modernes Dateisystem für IOS, macOS, tvos und watchos veröffentlicht. Das Apple-Datei System wurde für Flash-und SSD-Speicher optimiert und bietet die folgenden Features: starke Verschlüsselung, kopiekopiemetadaten, Freigabe von Speicherplatz, Klonen für Dateien und Verzeichnisse, Momentaufnahmen, schnelle Verzeichnis Größenänderung und atomarische Sicherheits-/sicherungprimitiver.

Weitere Informationen finden Sie im Apple- [Handbuch zum Datei System](https://developer.apple.com/library/prerelease/content/documentation/FileManagement/Conceptual/APFS_Guide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40016999)von Apple.

<a name="Apple-Pay-Enhancements"></a>

### <a name="apple-pay-enhancements"></a>Apple Pay Erweiterungen

Apple hat mehrere Verbesserungen an den Apple Pay in macOS Sierra vorgenommen, die dem Benutzer die Möglichkeit bieten, sichere Zahlungen von Websites zu erstellen.

Mit macOS Sierra wurden mehrere neue APIs hinzugefügt, die mit macOS Sierra, IOS und watchos zusammenarbeiten, um dynamische Zahlungs Netzwerke und eine neue Sandkasten Testumgebung zu unterstützen.

macOS Sierra umfasst das neue Apple Pay JavaScript-Framework, mit dem Entwickler Apple Pay direkt in Safari-basierte IOS-und macOS-Websites integrieren können. Für Websites, die Apple Pay unterstützen, kann der Benutzer die Zahlung entweder mithilfe seines iPhones oder Apple Watch autorisieren.

Weitere Informationen finden Sie in der Apple-Referenz zum Apple [Pay js-Framework](https://developer.apple.com/reference/applepayjs) .

<a name="Building-Modern-macOS-Apps"></a>

### <a name="building-modern-macos-apps"></a>Erstellen moderner macOS-Apps

Moderne macOS-apps wie z. b. Safari Webbrowser von Microsoft, Pages Word Processor and Numbers Spread Sheet verwenden viele neue Technologien, um eine einheitliche, kontextabhängige Benutzeroberfläche zu präsentieren, die herkömmliche Elemente der Benutzeroberfläche, wie z. b. Gleit Komma Bereiche und mehrere geöffnete Fenster, bietet.

[![Ein Beispiel für ein Mac-Fenster im Register Format](images/content08.png)](images/content08.png#lightbox)

Im Leitfaden [Erstellen moderner MacOS-apps](~/mac/platform/introduction-to-macos-sierra/modern-cocoa-apps.md) finden Sie verschiedene Tipps, Features und Techniken, die ein Entwickler zum Erstellen einer modernen macOS-app in xamarin. Mac verwenden kann.

<a name="CloudKit-Data-Sharing"></a>

### <a name="cloudkit-data-sharing"></a>Cloudkit-Datenfreigabe

Das cloudkit-Framework wurde in macOS Sierra erweitert, damit Benutzerdaten Sätze oder Ressourcen Eintrags Sätze schnell und einfach aus Ihren privaten icloud-Datenbanken freigeben können.

Cloudkit bietet eine umfassende Benutzeroberfläche zum Senden und akzeptieren von freigegebenen Daten Satz Einladungen, und der Benutzer verfügt über eine umfassende Lese-/Schreibkontrolle über die Personen, die Zugriff auf die Datensätze haben.

Weitere Informationen finden Sie in der Referenz zum [cloudkit-Framework](https://developer.apple.com/reference/clockkit) von Apple und in der [cloudkit js-frameworkreferenz](https://developer.apple.com/reference/cloudkitjs).

> [!IMPORTANT]
> Apple [stellt Tools zur Verfügung](https://developer.apple.com/support/allowing-users-to-manage-data/), die Entwickler dabei unterstützen, die Datenschutz-Grundverordnung (DSGVO) der Europäischen Union umzusetzen.

<a name="Safari-App-Extensions-Support"></a>

### <a name="safari-app-extensions-support"></a>Unterstützung von Safari App Extensions

Safari-App-Erweiterungen ermöglichen der APP, das Verhalten des Safari-Webbrowsers zu erweitern, während Sie eng in macOS Sierra integriert werden. Da macOS-Safari-App-Erweiterungen ähnlich wie IOS Safari-App-Erweiterungen funktionieren, können Sie problemlos von einem System auf ein anderes portieren.

Weitere Informationen finden Sie im [Programmier Handbuch zur Safari-App-Erweiterung](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternetWeb/Conceptual/SafariAppExtension_PG/index.html#//apple_ref/doc/uid/TP40017319)von Apple.

<a name="Security-and-Privacy-Enhancements"></a>

### <a name="security-and-privacy-enhancements"></a>Erweiterungen für Sicherheit und Datenschutz

Apple hat mehrere Verbesserungen hinsichtlich Sicherheit und Datenschutz in macOS Sierra vorgenommen, die der APP dabei helfen, die Sicherheit der APP zu verbessern und den Datenschutz für den Endbenutzer zu gewährleisten, einschließlich der folgenden:

- Der neue `NSAllowsArbitraryLoadsInWebContent` Schlüssel kann der APP-Datei hinzugefügt werden `Info.plist` und ermöglicht das ordnungsgemäße Laden von Webseiten, während der Apple Transport Security (ATS)-Schutz weiterhin für den Rest der App aktiviert ist.
- Die API der Common Data Security Architecture (CDSA) wurde als veraltet markiert und sollte durch die API "-API" ersetzt werden, um asymmetrische Schlüssel zu generieren.
- Für alle SSL/TLS-Verbindungen ist das symmetrische RC4-Chiffre jetzt standardmäßig deaktiviert. Außerdem unterstützt die Secure-Transport-API SSLv3 nicht mehr. es wird empfohlen, dass die APP so bald wie möglich die SHA-1-und 3DES-Kryptografie nicht mehr verwendet.
- Da die neue Zwischenablage in ios 10 und macOS Sierra dem Benutzer das Kopieren und Einfügen zwischen Geräten ermöglicht, wurde die API erweitert, damit eine Zwischenablage auf ein bestimmtes Gerät beschränkt werden kann, und Sie muss mit einem Zeitstempel versehen werden, um an einem bestimmten Punkt automatisch gelöscht zu werden. Außerdem sind benannte pasteboards nicht mehr persistent und sollten durch die freigegebenen Zwischenablage-Container ersetzt werden.
- Wenn die APP auf geschützte Daten zugreift (z. b. den Kalender des Benutzers), _muss_ diese Absicht mit dem richtigen Schlüsselzeichen-Zeichen folgen Wert in der `Info.plist` Datei ( `NSCalendarUsageDescription` im Fall des Kalenders) deklariert werden.
- Entwickler signierte apps, die nicht über den Mac App Store bereitgestellt werden, können nun cloudkit, icloud keychain, icloud Drive, Remote Push-Benachrichtigungen, MapKit und VPN-Berechtigungen nutzen.
- macOS Sierra unterstützt nicht mehr die Übermittlung von externem Code oder Daten zusammen mit der Code Signatur Geber-App im ZIP-Archiv oder nicht signierten Datenträger Image, da der Laufzeitpfad vor der Laufzeit nicht bekannt ist.

Darüber hinaus müssen apps, die auf macOS Sierra (oder höher) ausgeführt werden, ihre Absicht, auf bestimmte Features oder Benutzerinformationen zuzugreifen, statisch deklarieren, indem Sie einen oder mehrere Datenschutz spezifische Schlüssel in Ihren `Info.plist` Dateien eingeben, die dem Benutzer erklären, warum die APP Zugriff erhalten möchte.

Da macOS Sierra diese Änderungen mit IOS 10 gemeinsam nutzt, finden Sie weitere Informationen im Leitfaden zu den [Sicherheits-und Datenschutz Erweiterungen](~/ios/app-fundamentals/security-privacy.md) für IOS 10.

<a name="Smart-Card-Driver-Extension-Support"></a>

### <a name="smart-card-driver-extension-support"></a>Unterstützung von Smartcard-Treiber Erweiterungen

Mit macOS Sierra kann die APP `NSExtension` auf Basis von smartcardtreibern erstellen, die schreibgeschützten Zugriff auf den Inhalt von bestimmten Arten von Smartcards ermöglicht. Diese Informationen werden dann innerhalb des System Keychain dargestellt (durch Ersetzen der veralteten Common Data Security Architecture-Methode).

Weitere Informationen finden Sie in der Referenz zum [cryptotokenkit-Framework](https://developer.apple.com/reference/cryptotokenkit)von Apple.

<a name="Unified-Logging"></a>

### <a name="unified-logging"></a>Einheitliche Protokollierung

Unified Logging bietet der App eine einzige API für effizientes Messaging auf allen Ebenen des Systems. Mit einheitlicher Protokollierung verfügt die APP über eine differenzierte Steuerung der verschiedenen Protokollierungs Ebenen, einschließlich Datenschutz Kontrollen und Aktivitäts Überwachung für einfacheres Debuggen. 

Die Protokollierung ermöglicht die automatische Nachrichten Korrelation, wenn Aktivitäts Überwachung und-Protokollierung zusammen verwendet werden.

macOS Sierra enthält eine neue Konsolen-app (in Anwendungen/Dienstprogrammen), mit der Protokolldaten aus mehreren Quellen, einschließlich verbundener Geräte, angezeigt werden können. Außerdem werden tokenisierte und gespeicherte Suchvorgänge unterstützt, und es werden Verbindungen zwischen verknüpften Nachrichten über mehrere Prozesse hinweg angezeigt.

Darüber hinaus können Protokollmeldungen mit Befehlszeilen Tools angezeigt und verwaltet werden.

Weitere Informationen finden Sie in der Apple- [Protokollierungs Referenz](https://developer.apple.com/documentation/os/logging).

<a name="Wide-Color"></a>

### <a name="wide-color"></a>Breite Farbskala

macOS Sierra erweitert die Unterstützung für erweiterte Pixel Formate und große Farbbereiche im gesamten System, einschließlich Frameworks wie Kern Grafiken, Core-Bild, Metal und AVFoundation. Die Unterstützung für Geräte mit breit Farbanzeige wird weiter vereinfacht, indem dieses Verhalten im gesamten Grafik Stapel bereitgestellt wird.

Außerdem wurde `AppKit` geändert, um in den neuen erweiterten **sRGB** -colorspace arbeiten zu können. Dadurch wird es einfacher, Farben in breit Farbgamuts zu kombinieren, ohne dass ein erheblicher Leistungsverlust auftritt.

Apple bietet bei der Arbeit mit breiten Farben die folgenden bewährten Methoden:

- `NSColor`verwendet nun den sRGB-Farbraum und gibt keine Werte mehr in den `0.0` Bereich bis an `1.0` . Wenn die APP auf dem vorherigen Klammer Verhalten basiert, muss Sie für macOS Sierra geändert werden.
- Wenn Sie eine API auf niedriger Ebene, wie z. b. Kern Grafiken oder Metal, zum Bereitstellen der Bildverarbeitung verwenden, sollte die APP einen erweiterten Bereichs Farbraum und ein Pixel Format verwenden, das 16-Bit-Gleit Komma Werte unterstützt. Bei Bedarf muss die APP Farbkomponenten Werte manuell einspannen.
- Haupt Grafiken, Kern Bild-und Metal-leistungshader bieten neue Methoden zum Wechseln zwischen den beiden Farbräumen.

Weitere Informationen finden Sie im Leitfaden [Einführung in Wide Color](~/ios/platform/wide-color.md) .

<a name="Additional-Framework-Changes"></a>

## <a name="additional-framework-changes"></a>Weitere frameworkänderungen

Zusätzlich zu den oben aufgeführten wichtigen frameworkänderungen und Ergänzungen hat Apple in macOS Sierra viele zusätzliche Änderungen am geringfügigen Framework vorgenommen.

Weitere Informationen finden Sie in unserem Handbuch zu [zusätzlichen Framework-Änderungen](~/mac/platform/introduction-to-macos-sierra/additional-framework-changes.md) .

<a name="Deprecated-APIs"></a>

## <a name="deprecated-apis"></a>Nicht mehr unterstützte APIs

Die folgenden APIs sind in macOS Sierra veraltet:

- Das HFS-Standard Datei System wird nicht mehr unterstützt.

Eine komplette Liste der veralteten und Änderungen finden Sie in der Dokumentation zu den [macOS v 10.12-API-Distributionen](https://developer.apple.com/library/archive/releasenotes/General/APIDiffsMacOS10_12/index.html) von Apple.

## <a name="related-links"></a>Verwandte Links

- [Mac-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Mac)
- [Neues in macOS 10,12](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
