---
title: 32/64-Bit-Plattform-Überlegungen
description: Dieses Dokument beschreibt verschiedene Aspekte zu bedenken, wenn es sich bei 32-Bit und 64-Bit-Architekturen für eine Xamarin.iOS oder Xamarin.Mac-Anwendung als Ziel.
ms.prod: xamarin
ms.assetid: F7126340-04B2-4A10-B14D-394E23527C1A
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: 31eb0bfae58ecdca40548e46d1d9d95828be67b4
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61347838"
---
# <a name="3264-bit-platform-considerations"></a>32/64-Bit-Plattform-Überlegungen

IOS- und MacOS in der Vergangenheit 32- und 64-Bit-apps unterstützt haben, aber Apple wurde allmählich auf 32-Bit-Unterstützung als veraltet.

Ab iOS 11, 32-Bit-apps nicht mehr gestartet, und [alle Übermittlungen an den Store-App müssen 64-Bit-Unterstützung](https://developer.apple.com/news/?id=06282017b).

Ab Januar 2018 [neue für den Mac App Store eingereichte apps müssen 64-Bit-Unterstützung](https://developer.apple.com/news/?id=06282017a), und vorhandene apps müssen vom Juni 2018 aktualisiert werden.

Xamarin Classic-API (`XamMac.dll` und `monotouch.dll`) unterstützt nur 32-Bit-Anwendungen. Neue Xamarin.iOS- und Xamarin.Mac-Anwendungen verwenden jedoch die [Unified API](~/cross-platform/macios/unified/index.md) (`Xamarin.iOS` und `Xamarin.Mac`) in der Standardeinstellung und kann daher Ziel 32- und 64-Bit, nach Bedarf.

## <a name="ios"></a>iOS

<a name="enable-64" />

### <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Aktivieren die 64-Bit-builds von Xamarin.iOS-apps

> [!WARNING]
> In diesem Abschnitt ist enthalten, noch aus und Verschieben von älteren Xamarin.iOS-Projekte in der Unified API und 64-Bit-Unterstützung. Alle neuen Xamarin.iOS-Projekte werden standardmäßig der Unified API und des Ziels 64-Bit-verwendet werden.

Für mobile Xamarin.iOS-Anwendungen, die an die Unified API konvertiert wurden, müssen Entwickler die Buildeinstellungen manuell zum abzielen auf 64-Bit aktualisieren:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. In der **Lösungspad**, doppelklicken Sie auf der app-Projekt, öffnen Sie die **Projektoptionen** Fenster.
2. Wählen Sie **iOS-Build**.
3. Für das iPhone-Simulator in der **unterstützte Architekturen** Dropdownliste, wählen Sie entweder **X86\_64** oder **i386 + X86\_64**:

   [![Festlegen von unterstützten Architekturen auf X86\_64 oder i386 + X86\_64](Images/Image01.png "Setting Supported architectures to x86\_64 or i386 + x86\_64")](Images/Image01-large.png#lightbox) 

4. Wählen Sie für physische Geräte eine der verfügbaren **ARM64** Kombinationen:

   [![Festlegen von unterstützten Architekturen auf eine der Kombinationen ARM64](Images/Image02.png "Einstellung unterstützt Architekturen, die eine der Kombinationen ARM64")](Images/Image02-large.png#lightbox)

5. Klicken Sie auf **OK**.
6. Einen bereinigten Build auszuführen.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. In der **Projektmappen-Explorer**mit der rechten Maustaste auf die app Projekt, und wählen Sie **Eigenschaften**.
2. Wählen Sie **iOS-Build**.
3. Legen Sie für das iPhone-Simulator, **unterstützte Architekturen** entweder **X86\_64** oder **i386 + X86\_64**: 

   [![Festlegen von unterstützten Architekturen auf x86_64 "oder" i386 "+" X86\_64](Images/VS02.png "Setting Supported architectures to x86_64 or i386 + x86\_64")](Images/VS02-large.png#lightbox)

4. Wählen Sie für physische Geräte eine der verfügbaren **ARM64** Kombinationen:
    
   [![Festlegen von unterstützten Architekturen auf eine der Kombinationen ARM64](Images/VS01.png "Einstellung unterstützt Architekturen, die eine der Kombinationen ARM64")](Images/VS01-large.png#lightbox)

5. Speichern Sie die Änderungen.
6. Einen bereinigten Build auszuführen.

-----

ARMv7s wird nur von den A6-Prozessor enthalten in der iPhone-5 (oder höher) unterstützt. ARMv7-Code ist schneller und kleiner als die ARMv6 nur funktioniert mit dem iPhone 3GS- und höher, und ist von Apple erforderlich, wenn das iPad oder eine niedrigste zulässige iOS-Version 5.0 ist. ARMv6 funktioniert auf allen Geräten, aber es wird vom Compiler geliefert, mit Xcode 4.5 und höher nicht mehr unterstützt. 

ARM64 ist erforderlich, um die iOS 8 auf dem iPhone 6 oder andere Geräte für die 64-Bit-Unterstützung und wird von Apple erforderlich sein, wenn neuer oder zum Aktualisieren Beenden von Anwendungen im iTunes App Store zu übermitteln.

Einen umfassenden Überblick über die Funktionen der verschiedenen iOS-Geräte werden soll, finden Sie in Apple [Gerätekompatibilität](https://developer.apple.com/library/content/documentation/DeviceInformation/Reference/iOSDeviceCompatibility/DeviceCompatibilityMatrix/DeviceCompatibilityMatrix.html) Dokument.

### <a name="64-bit-and-binary-size-increases"></a>64-Bit- und Binärdateien Größe zunimmt

Bei Apple-Umstellung von 32-Bit zu 64-Bit-iOS werden apps auf 32-Bit- und 64-Bit-Hardware ausgeführt werden müssen. Aus diesem Grund können die Xamarin Unified API Entwickler sowohl als Ziel.

Auf 32-Bit- und 64-Bit-Architekturen erhöht sich erheblich auf die Größe einer Anwendung aus. Dadurch können jedoch so neuere Geräte optimierten Code ausgeführt wird, unterstützt aber weiterhin die ältere Geräte.

> [!IMPORTANT]
> Wenn Sie die folgende Meldung angezeigt, wenn Sie eine iOS-Anwendung zu iTunes App Store, übermitteln _"Warnung ITMS-9000: Fehlende 64-Bit-Unterstützung. Ab dem 1. Februar 2015, neue iOS-apps in der App-Store hochgeladen müssen 64-Bit-Unterstützung einschließen und mit iOS 8-SDK in Xcode 6 oder höher erstellt werden. So aktivieren Sie in Ihrem Projekt die 64-Bit es wird empfohlen, Xcode Buildeinstellung "Standard-Architekturen" standardmäßig eine einzelne Binärdatei mit 32-Bit- und 64-Bit-Code zu erstellen. "_ Sie müssen die unterstützten Architekturen ein Wechsel zu einer der verfügbaren **ARM64** Kombination aus (siehe oben), Neukompilierung und erneute senden.

## <a name="mac"></a>Mac

> [!IMPORTANT]
> Ab Januar 2018, müssen alle neuen Mac-apps, die an den Mac App Store übermittelt die 64-Bit-unterstützen. Vorhandenen Mac App Store-apps und ihre Updates müssen 64-Bit-ab Juni 2018 unterstützen. Finden Sie unter [Apple Announcment](https://developer.apple.com/news/?id=06282017a) und [eine Anleitung, die beschreibt, wie Sie Ihre Xamarin.Mac-apps auf 64-Bit aktualisieren](~/cross-platform/macios/32-and-64/mac-64-bit.md).

Die meisten modernen Macintosh-Computer unterstützt sowohl 32-Bit-als auch 64-Bit-Anwendungen.   Mac OS 10.6 (Schnee Leopard) wurde das letzte Betriebssystem auf 32-Bit-Systemen ausgeführt werden.   Die meisten Macintosh-Computer, die seit 2010 Unterstützung beider Systeme veröffentlicht.

Im Gegensatz zu iOS werden viele der neuen Frameworks eingeführt, die in früheren Versionen von MacOS nur in 64-Bit-Modus ausgeführt wird (CloudKit, EventKit, GameController, LocalAuthentication, MediaLibrary, MultipeerConnectivity, NotificationCenter, GLKit, SpriteKit, Social, unterstützt und MapKit, u. a.).

Die Unified API können Entwickler wählen, welche Art von Anwendungen, die sie erstellen möchten: 32-Bit oder 64-Bit.

**32-Bit-Anwendungen** wird auf 32-Bit- und 64-Bit-Macintosh-Computern ausführen, müssen Sie einen Adressraum, der auf 32 Bits beschränkt und erfordern, dass alle Bibliotheken 32 Bits sind.

Sie können diesen Modus werden in der Regel verwenden, wenn man 32-Bit-Abhängigkeiten, die nicht im 64-Bit-Modus ausgeführt werden, wenn Sie möchten einen kleineren Download oder es gibt keine Leistungsvorteile bei der Umstellung auf 64-Bit.

In diesem Modus beschränkt, da Sie nicht viele Frameworks in MacOS Mavericks und MacOS Yosemite verwenden können.

**64-Bit-Anwendungen** wird nur auf 64-Bit-Mac-Geräten ausgeführt werden.

Für Mac ist dies der bevorzugte Modus zum des Vorgangs, wie die meisten Macs finden Sie unter verwenden heute, 64-Bit-Modus unterstützen, und Zugriff auf den vollständigen Satz von Frameworks, die von Apple bereitgestellten haben.

### <a name="enabling-64-bit-builds-of-xamarinmac-apps"></a>Aktivieren die 64-Bit-builds von Xamarin.Mac-apps

Informationen zu eine 64-Bit-app mit Xamarin.Mac zu erstellen, finden Sie unter den [64-Bit-Anwendungen Aktualisieren von Xamarin.Mac Unified](~/cross-platform/macios/32-and-64/mac-64-bit.md) Guide.

## <a name="related-links"></a>Verwandte Links

- [Klassischen Vs Unified API-Unterschiede](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
