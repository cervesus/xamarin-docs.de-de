---
title: Bedingungen bezüglich der 32-/64-Bit-Plattform
description: Dieses Dokument beschreibt die verschiedenen Aspekte zu bedenken, Geschäftsgruppen mit 32 Bit und 64-Bit-Architekturen für eine Anwendung Xamarin.iOS oder Xamarin.Mac.
ms.prod: xamarin
ms.assetid: F7126340-04B2-4A10-B14D-394E23527C1A
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: c722efc0bc6e8a4ea29af603f88c0e0644c2ed8c
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781548"
---
# <a name="3264-bit-platform-considerations"></a>Bedingungen bezüglich der 32-/64-Bit-Plattform

IOS- und Mac OS in der Vergangenheit 32- und 64-Bit-apps unterstützt haben, aber Apple wurde allmählich auf 32-Bit-Unterstützung als veraltet.

Seit iOS 11, 32-Bit-apps nicht mehr gestartet, und [alle Übermittlungen auf den App Store müssen 64-Bit-Unterstützung](https://developer.apple.com/news/?id=06282017b).

Ab Januar-2018 [neue apps auf dem Mac App Store übermittelt müssen 64-Bit-Unterstützung](https://developer.apple.com/news/?id=06282017a), und vorhandene apps müssen durch Juni 2018 aktualisiert werden.

Die Xamarin klassische API (`XamMac.dll` und `monotouch.dll`) unterstützt nur 32-Bit-Anwendungen. Neue Xamarin.iOS und Xamarin.Mac Anwendungen verwenden jedoch den [einheitliche API](~/cross-platform/macios/unified/index.md) (`Xamarin.iOS` und `Xamarin.Mac`) standardmäßig und können daher Ziel 32- und 64-Bit, nach Bedarf.

## <a name="ios"></a>iOS

<a name="enable-64" />

### <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Aktivieren die 64-Bit-builds von Xamarin.iOS-apps

> [!WARNING]
> Vergangene Gründen und zur älteren Xamarin.iOS-Projekte, die einheitliche API verschieben und 64-Bit-Unterstützung ist in diesem Abschnitt enthalten. Alle neuen Xamarin.iOS Projekte werden die einheitliche API und das Ziel 64-Bit wird standardmäßig verwendet.

Für Xamarin.iOS mobile Anwendungen, die an die API Unified konvertiert wurden, müssen Entwickler manuell die Buildeinstellungen zum abzielen auf 64-Bit aktualisieren:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. In der **Lösung Pad**, doppelklicken Sie auf die app-Projekt öffnen die **Projektoptionen** Fenster.
2. Wählen Sie **iOS-Build**.
3. Für das iPhone-Simulator in die **unterstützt Architekturen** Dropdownliste, wählen Sie entweder **X86\_64** oder **i386 + X86\_64**:

   [![Unterstützte Architekturen auf X86 festlegen\_64 oder i386 + X86\_64](Images/Image01.png "Setting Supported architectures to x86\_64 or i386 + x86\_64")](Images/Image01-large.png#lightbox) 

4. Wählen Sie für physische Geräte eine der verfügbaren **ARM64** Kombinationen aus:

   [![Unterstützte Architekturen auf eine der ARM64 Kombinationen festlegen](Images/Image02.png "Einstellung unterstützt Architekturen auf einen der ARM64 Kombinationen")](Images/Image02-large.png#lightbox)

5. Klicken Sie auf **OK**.
6. Einen bereinigten Build auszuführen.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. In der **Projektmappen-Explorer**mit der rechten Maustaste auf den app-Projekt, und wählen Sie **Eigenschaften**.
2. Wählen Sie **iOS-Build**.
3. Legen Sie für das iPhone-Simulator, **unterstützt Architekturen** entweder **X86\_64** oder **i386 + X86\_64**: 

   [![Unterstützte Architekturen x86_64 oder i386 + X86 festlegen\_64](Images/VS02.png "Setting Supported architectures to x86_64 or i386 + x86\_64")](Images/VS02-large.png#lightbox)

4. Wählen Sie für physische Geräte eine der verfügbaren **ARM64** Kombinationen aus:
    
   [![Unterstützte Architekturen auf eine der ARM64 Kombinationen festlegen](Images/VS01.png "Einstellung unterstützt Architekturen auf einen der ARM64 Kombinationen")](Images/VS01-large.png#lightbox)

5. Speichern Sie die Änderungen.
6. Einen bereinigten Build auszuführen.

-----

ARMv7s wird nur von der A6-Prozessor enthalten in der iPhone 5 (oder höher) unterstützt. ARMv7-Code ist schneller und kleiner als die ARMv6 nur funktioniert mit dem iPhone 3GS- und höher und muss von Apple beim Abzielen auf dem iPad oder eine niedrigste zulässige iOS-Version 5.0. ARMv6 funktioniert auf allen Geräten, aber es wird nicht mehr unterstützt, vom Compiler mit Xcode 4.5 und höher ausgeliefert. 

ARM64 iOS 8 auf iPhone 6 oder anderen Geräten 64-Bit-Unterstützung ist erforderlich und muss von Apple beim Senden von neuen oder zum Beenden von Anwendungen im iTunes App Store aktualisieren.

Um umfassende Informationen zu den Funktionen verschiedener iOS-Geräte, konsultieren Sie Apple [Gerätekompatibilität](https://developer.apple.com/library/content/documentation/DeviceInformation/Reference/iOSDeviceCompatibility/DeviceCompatibilityMatrix/DeviceCompatibilityMatrix.html) Dokument.

### <a name="64-bit-and-binary-size-increases"></a>64-Bit- und binäre vergrößert

Apps müssen während der Apple Übergang von 32-Bit zu 64-Bit-iOS auf 32-Bit und 64-Bit-Hardware ausgeführt werden. Aus diesem Grund können die Xamarin einheitliche API Entwickler sowohl als Ziel.

32-Bit und 64-Bit-Architekturen abzielen, wird die Größe einer Anwendung erheblich erhöht. Dadurch können jedoch so neuere Geräte optimierten Code ausgeführt wird, während ältere Geräte weiterhin unterstützt.

> [!IMPORTANT]
> Wenn Sie die folgende Meldung angezeigt, wenn Sie eine iOS-Anwendung zum iTunes App Store übermitteln _"Warnung ITMS-9000: fehlende 64-Bit-Unterstützung. Ab dem 1. Februar 2015 neue iOS hochgeladen, auf den App Store-apps müssen umfassen auch die 64-Bit-Unterstützung und mit iOS 8-SDK in Xcode 6 oder höher erstellt werden. So aktivieren Sie 64-Bit-im Projekt sollten unter Verwendung des standardmäßigen Xcode Buildeinstellung "Standard-Architekturen" auf einem einzelnen Binärwert mit 32-Bit und 64-Bit-Code zu erstellen. "_ Sie unterstützten Architekturen auf eine der verfügbaren wechseln müssen **ARM64** Kombination (siehe oben), Recompile und erneutes Senden.

## <a name="mac"></a>Mac

> [!IMPORTANT]
> Ab Januar 2018, müssen alle neuen Mac-apps, die auf dem Mac App Store übermittelt 64-Bit-unterstützen. Vorhandene Mac App Store-apps und ihre Updates müssen 64-Bit ab Juni 2018 unterstützen. Finden Sie unter [Apple Announcment](https://developer.apple.com/news/?id=06282017a) und [eine Anleitung, die beschreibt, wie Ihre apps Xamarin.Mac auf 64-Bit aktualisiert](~/cross-platform/macios/32-and-64/mac-64-bit.md).

Die meisten modernen Macintosh-Computer unterstützen die 32-Bit und 64-Bit-Anwendungen.   Mac OS 10.6 (Schneefall Leopard) wurde das letzte Betriebssystem auf 32-Bit-Systemen ausgeführt werden.   Die meisten Macs seit 2010 beiden Systemen unterstützt.

Im Gegensatz zu iOS werden viele der neuen Frameworks eingeführt, die in den neuesten Versionen der MacOS nur in 64-Bit-Modus (CloudKit, EventKit, GameController, LocalAuthentication, MediaLibrary, MultipeerConnectivity, NotificationCenter, GLKit, SpriteKit, Social, unterstützt und MapKit, u. a.).

Die einheitliche API ermöglichen Entwicklern das auswählen, welche Art von Anwendungen, die sie erzeugen möchten: 32-Bit oder 64-Bit.

**32-Bit-Anwendungen** wird auf 32-Bit und 64-Bit-Macintosh-Computern ausführen, haben Sie einen Adressraum auf 32 Bits beschränkt und erfordern, dass alle Bibliotheken 32 Bits sind.

Normalerweise verwenden Sie diesen Modus haben 32-Bit-Abhängigkeiten, die nicht im 64-Bit-Modus ausgeführt werden, wenn Sie einen kleineren Download haben möchten, oder wenn es gibt keine Leistungsvorteile bei der Umstellung auf 64-Bit.

Dieser Modus ist eingeschränkt, Sie nicht mehr auf die viele-Frameworks in MacOS auf dem Mavericks und MacOS Yosemite verwendet werden.

**64-Bit-Anwendungen** nur auf 64-Bit-Mac-Geräten ausgeführt wird.

Für Mac ist dies der bevorzugte Modus des Vorgangs, wie die meisten Macs verwendet heute, 64-Bit-Modus unterstützen, und Sie Zugriff auf den vollständigen Satz von Apple bereitgestellten Frameworks haben.

### <a name="enabling-64-bit-builds-of-xamarinmac-apps"></a>Aktivieren die 64-Bit-builds von Xamarin.Mac-apps

Informationen zum Erstellen einer 64-Bit-app mit den Xamarin.Mac finden Sie unter der [Anwendungen auf 64-Bit aktualisieren Xamarin.Mac Unified](~/cross-platform/macios/32-and-64/mac-64-bit.md) Handbuch.

## <a name="related-links"></a>Verwandte Links

- [Klassische Vs einheitliche API-Unterschiede](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
