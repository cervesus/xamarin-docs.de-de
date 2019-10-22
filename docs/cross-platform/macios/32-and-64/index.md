---
title: Überlegungen zu 32/64-Bit-Plattformen
description: In diesem Dokument werden verschiedene Überlegungen beschrieben, die Sie berücksichtigen sollten, wenn Sie für eine xamarin. IOS-oder xamarin. Mac-Anwendung auf 32-Bit-und 64-Bit-Architekturen abzielen.
ms.prod: xamarin
ms.assetid: F7126340-04B2-4A10-B14D-394E23527C1A
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: bcee9c7e09a9470cbf80e99c047a7c52f61f888a
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "71249795"
---
# <a name="3264-bit-platform-considerations"></a>Überlegungen zu 32/64-Bit-Plattformen

Obwohl IOS und macOS sowohl 32-als auch 64-Bit-apps in der Vergangenheit unterstützt haben, hat Apple die Unterstützung für die Unterstützung von 32-Bit

Ab IOS 11 werden 32-Bit-apps nicht mehr gestartet, und [alle Übermittlungen an den App Store müssen 64 Bit unterstützen](https://developer.apple.com/news/?id=06282017b).

Ab Januar 2018 [müssen neue apps, die an den Mac App Store übermittelt werden, 64-Bit unterstützen](https://developer.apple.com/news/?id=06282017a), und vorhandene apps müssen bis 2018 aktualisiert werden.

Die Classic API von xamarin (`XamMac.dll` und `monotouch.dll`) unterstützten nur 32-Bit-Anwendungen. Neue xamarin. IOS-und xamarin. Mac-Anwendungen verwenden jedoch standardmäßig die [Unified API](~/cross-platform/macios/unified/index.md) (`Xamarin.iOS` und `Xamarin.Mac`) und können daher bei Bedarf sowohl 32 als auch 64-Bit als Ziel verwenden.

## <a name="ios"></a>iOS

<a name="enable-64" />

### <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Aktivieren von 64-Bit-Builds von xamarin. IOS-apps

> [!WARNING]
> Dieser Abschnitt ist aus historischen Gründen enthalten und soll Sie dabei unterstützen, ältere xamarin. IOS-Projekte in die Unified API zu verschieben und 64-Bit-Unterstützung zu unterstützen. Alle neuen xamarin. IOS-Projekte verwenden standardmäßig die Unified API und das 64-Bit-Ziel.

Für Mobile xamarin. IOS-Anwendungen, die in die Unified API konvertiert wurden, müssen Entwickler die Buildeinstellungen manuell auf das 64-Bit-Ziel aktualisieren:

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Doppelklicken Sie im **Lösungspad**auf das Projekt der APP, um das Fenster **Projektoptionen** zu öffnen.
2. Wählen Sie **IOS-Build**aus.
3. Wählen Sie für den iPhone-Simulator in der Dropdown Liste **unterstützte Architekturen** entweder **x86 \_64** oder **i386 + x86 \_64**aus:

   [![Festlegen unterstützter Architekturen auf x86 \_64 oder i386 + x86 \_64](Images/Image01.png "Setting Supported architectures to x86\_64 or i386 + x86\_64")](Images/Image01-large.png#lightbox) 

4. Wählen Sie für physische Geräte eine der verfügbaren **ARM64** -Kombinationen aus:

   [![Festlegen unterstützter Architekturen auf eine der ARM64-Kombinationen](Images/Image02.png "Festlegen unterstützter Architekturen auf eine der ARM64-Kombinationen")](Images/Image02-large.png#lightbox)

5. Klicken Sie auf **OK**.
6. Führt einen sauberen Build aus.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt der APP, und wählen Sie **Eigenschaften**aus.
2. Wählen Sie **IOS-Build**aus.
3. Legen Sie für den iPhone-Simulator **unterstützte Architekturen** entweder auf **x86 \_64** oder **i386 + x86 \_64**fest: 

   [![Festlegen unterstützter Architekturen auf x86_64 oder i386 + x86 \_64](Images/VS02.png "Setting Supported architectures to x86_64 or i386 + x86\_64")](Images/VS02-large.png#lightbox)

4. Wählen Sie für physische Geräte eine der verfügbaren **ARM64** -Kombinationen aus:
    
   [![Festlegen unterstützter Architekturen auf eine der ARM64-Kombinationen](Images/VS01.png "Festlegen unterstützter Architekturen auf eine der ARM64-Kombinationen")](Images/VS01-large.png#lightbox)

5. Speichern Sie die Änderungen.
6. Führt einen sauberen Build aus.

-----

ARMv7s wird nur vom A6-Prozessor unterstützt, der in iPhone 5 (oder höher) enthalten ist. ARMv7-Code ist schneller und kleiner als der ARMv6, funktioniert nur mit dem iPhone 3GS und höher und ist für Apple erforderlich, wenn das iPad oder eine minimale IOS-Version 5,0 verwendet wird. ARMv6 funktioniert auf allen Geräten, wird jedoch nicht mehr vom Compiler unterstützt, der mit Xcode 4,5 und höher ausgeliefert wurde. 

ARM64 ist erforderlich, um IOS 8 auf iPhone 6 oder anderen 64-Bit-Geräten zu unterstützen, und wird von Apple benötigt, wenn neue oder Aktualisier Ende Anwendungen im iTunes App Store übermittelt werden.

Eine umfassende Betrachtung der Funktionen verschiedener IOS-Geräte finden Sie im Dokument zur [Gerätekompatibilität](https://developer.apple.com/library/content/documentation/DeviceInformation/Reference/iOSDeviceCompatibility/DeviceCompatibilityMatrix/DeviceCompatibilityMatrix.html) von Apple.

### <a name="64-bit-and-binary-size-increases"></a>64-Bit und binäre Größe steigen

Bei der Umstellung von 32-Bit auf 64 Bit müssen IOS-apps sowohl auf der 32-Bit-als auch auf der 64-Bit-Hardware ausgeführt werden. Aus diesem Grund ermöglicht die xamarin-Unified API es Entwicklern, beide Ziele zu erreichen.

Wenn Sie sowohl 32-Bit-als auch 64-Bit-Architekturen als Ziel haben, wird die Größe einer Anwendung erheblich vergrößert. Dies ermöglicht jedoch neueren Geräten das Ausführen von optimiertem Code, während ältere Geräte weiterhin unterstützt werden.

> [!IMPORTANT]
> Wenn beim Senden einer IOS-Anwendung an den iTunes App Store die folgende Meldung angezeigt wird _: "Warnung iTMS-9000: fehlende 64-Bit-Unterstützung. Ab dem 1. Februar 2015 müssen neue IOS-apps, die in den App Store hochgeladen werden, Unterstützung für 64-Bit enthalten und mit dem IOS 8 SDK erstellt werden, das in Xcode 6 oder höher enthalten ist. Um 64-Bit im Projekt zu aktivieren, wird empfohlen, die standardmäßige Xcode-Buildeinstellung "Standard Architekturen" zu verwenden, um eine einzelne Binärdatei mit 32-Bit-und 64-Bit-Code zu erstellen._ Sie müssen die unterstützten Architekturen auf eine der verfügbaren **ARM64** -Kombinationen umstellen (wie oben gezeigt), Sie neu kompilieren und erneut übermitteln.

## <a name="mac"></a>Mac

> [!IMPORTANT]
> Ab Januar 2018 müssen alle neuen Mac-apps, die an den Mac App Store übermittelt werden, 64 Bit unterstützen. Vorhandene Mac App Store-Apps und deren Updates müssen ab dem 2018. Juni 64-Bit-Version unterstützen. Weitere Informationen finden Sie in [der Apple-Ankündigung](https://developer.apple.com/news/?id=06282017a) und [in einem Leitfaden, in dem beschrieben wird, wie Sie Ihre xamarin. Mac-apps auf 64-Bit aktualisieren](~/cross-platform/macios/32-and-64/mac-64-bit.md).

Die meisten modernen Macintosh-Computer unterstützen sowohl 32-Bit-als auch 64-Bit-Anwendungen.   MacOS 10,6 (Schnee Leopard) war das letzte Betriebssystem, das auf 32-Bit-Systemen ausgeführt werden konnte.   Die meisten seit 2010 freigegebenen Macs unterstützen beide Systeme.

Anders als bei IOS werden viele der neuen Frameworks, die in neueren Versionen von macOS eingeführt wurden, nur im 64-Bit-Modus unterstützt (cloudkit, eventkit, Gamecontroller, localauthentication, Medialibrary, multipeer Connectivity, notificationcenter, glkit, spritekit, Social, und MapKit, u.a.).

Mit dem Unified API können Entwickler auswählen, welche Art von Anwendungen Sie entwickeln möchten: 32-Bit oder 64-Bit.

**32-Bit-Anwendungen** können auf 32-Bit-und 64-Bit-Macintosh-Computern ausgeführt werden, haben einen Adressraum, der auf 32 Bits beschränkt ist, und erfordern, dass alle Bibliotheken 32 Bits sind.

In der Regel verwenden Sie diesen Modus, wenn Sie über 32-Bit-Abhängigkeiten verfügen, die nicht im 64-Bit-Modus ausgeführt werden, wenn Sie einen kleineren Download benötigen oder wenn es keine Leistungsvorteile bei der Umstellung auf 64-Bit gibt.

Dieser Modus schränkt ein, da Sie nicht in der Lage sind, viele Frameworks zu verwenden, die in MacOS-Mavericks und MacOS Yosemite verfügbar sind.

**64-Bit-Anwendungen** können nur auf 64-Bit-Macintosh-Geräten ausgeführt werden.

Für Mac ist dies der bevorzugte Betriebsmodus, da die meisten verwendeten Macs heute den 64-Bit-Modus unterstützen, und Sie haben Zugriff auf den kompletten Satz von Frameworks, die von Apple bereitgestellt werden.

### <a name="enabling-64-bit-builds-of-xamarinmac-apps"></a>Aktivieren von 64-Bit-Builds von xamarin. Mac-apps

Weitere Informationen zum Entwickeln einer 64-Bit-App mithilfe von xamarin. Mac finden Sie im Handbuch [Aktualisieren von xamarin. Mac Unified Applications auf 64-Bit](~/cross-platform/macios/32-and-64/mac-64-bit.md) .

## <a name="related-links"></a>Verwandte Links

- [Unterschiede bei klassischem vs Unified API](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
