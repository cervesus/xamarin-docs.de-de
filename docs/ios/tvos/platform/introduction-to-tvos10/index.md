---
title: Einführung in tvOS 10
description: In diesem Artikel werden alle neuen und geänderten APIs und Features vorgestellt, die in tvos 10 für xamarin. tvos-Entwickler zur Verfügung stehen.
ms.prod: xamarin
ms.assetid: CB9C1EC8-6008-43AD-977E-976AE7C73DD8
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/16/2017
ms.openlocfilehash: 114d00b0d79b497201b3185a1443b8c8f9699c31
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70283545"
---
# <a name="introduction-to-tvos-10"></a>Einführung in tvOS 10

_In diesem Artikel werden alle neuen und geänderten APIs und Features vorgestellt, die in tvos 10 für xamarin. tvos-Entwickler zur Verfügung stehen._

Mit dem neuen tvos 10 SDK enthält Apple neue APIs und Dienste, mit denen Entwickler neue Kategorien von apps und Features erstellen können. 

Weitere Informationen zu tvos 10 finden Sie in der Dokumentation zu [tvos und apps](https://developer.apple.com/tvos/) von Apple.

## <a name="whats-new-in-tvos-10"></a>Neues in tvos 10

Apple hat in tvos 10 mehrere neue APIs und Dienste hinzugefügt sowie viele Verbesserungen an vorhandenen Features, einschließlich:

## <a name="new-user-interface-styles"></a>Neue Benutzeroberflächen Stile

tvos 10 unterstützt jetzt sowohl ein dunkles als auch ein helles Design der Benutzeroberfläche, mit dem sich alle Build-in-UIKit-Steuerelemente basierend auf den Benutzereinstellungen automatisch an anpassen.

Wenn Sie neue benutzerdefinierte UI-Steuerelemente erstellen und implementieren, sollte der Entwickler die [uitraitcollection](https://developer.apple.com/reference/uikit/uitraitcollection) -Klasse verwenden, um das ausgewählte Design des Benutzers anzupassen.

Weitere Informationen finden Sie in der Dokumentation zur [neuen Benutzeroberfläche](~/ios/tvos/platform/user-interface-styles.md) .

## <a name="security-and-privacy-enhancements"></a>Erweiterungen für Sicherheit und Datenschutz

Apple hat eine Reihe von Verbesserungen an Sicherheit und Datenschutz in tvos 10 vorgenommen, die dem Entwickler dabei helfen, die Sicherheit seiner apps zu verbessern und den Datenschutz für den Endbenutzer zu gewährleisten.

Folglich müssen apps, die auf watchos 3 (oder höher) ausgeführt werden, ihre Absicht, auf bestimmte Features oder Benutzerinformationen zuzugreifen, statisch deklarieren, indem Sie einen oder mehrere Daten `Info.plist` Schutz spezifische Schlüssel in Ihren Dateien eingeben, die dem Benutzer erklären, warum die APP Zugriff erhalten möchte.

Da tvos 10 diese Änderungen mit IOS 10 gemeinsam nutzt, finden Sie weitere Informationen im Leitfaden zu den [Sicherheits-und Datenschutz Erweiterungen](~/ios/app-fundamentals/security-privacy.md) für IOS 10.

## <a name="video-subscriber-account"></a>Video Abonnenten Konto

Neu in tvos 10, das Video Abonnenten-Konto Framework ermöglicht es apps, die authentifizierte Streaming-oder Video on-Demand-Funktionen unterstützen, bei Ihrem Kabel-oder Satelliten-TV-Anbieter zu authentifizieren, indem ein einmaliger Anmeldevorgang für den Endbenutzer verwendet wird.

<!--To find out more, please see our [Video Subscriber Account](~/ios/platform-features/introduction-to-ios10/video-subscriber-account/) guide.-->

## <a name="wide-color"></a>Breite Farbskala

tvos 10 erweitert die Unterstützung für erweiterte Pixel Formate und große Farbbereiche im gesamten System, einschließlich Frameworks wie Kern Grafiken, Core-Bild, Metal und AVFoundation. Die Unterstützung für Geräte mit breit Farbanzeige wird weiter vereinfacht, indem dieses Verhalten im gesamten Grafik Stapel bereitgestellt wird.

Außerdem wurde geändert, um in den neuen erweiterten sRGB-colorspace arbeiten zu können. Dadurch wird es einfacher, Farben in breit Farbgamuts zu kombinieren, ohne dass ein erheblicher Leistungsverlust auftritt. `UIKit`

Apple bietet bei der Arbeit mit breiten Farben die folgenden bewährten Methoden:

- `UIColor`verwendet nun den sRGB-Farbraum und gibt keine Werte mehr in den `0.0` Bereich `1.0` bis an. Wenn die APP auf dem vorherigen Klammer Verhalten basiert, muss Sie für tvos 10 geändert werden.
- Wenn die APP ein benutzerdefiniertes `UIImages`Rendering von ausführt, verwenden Sie die neue [uigraphicsimagerender](https://developer.apple.com/reference/uikit/uigraphicsimagerenderer) -Klasse, um die Verwendung der Formate für erweiterte Bereiche oder Standard Bereiche anzugeben.
- Wenn Sie eine API auf niedriger Ebene, wie z. b. Kern Grafiken oder Metal, zum Bereitstellen der Bildverarbeitung verwenden, sollte die APP einen erweiterten Bereichs Farbraum und ein Pixel Format verwenden, das 16-Bit-Gleit Komma Werte unterstützt. Bei Bedarf muss die APP Farbkomponenten Werte manuell einspannen.
- Haupt Grafiken, Kern Bild-und Metal-leistungshader bieten neue Methoden zum Wechseln zwischen den beiden Farbräumen.

Weitere Informationen finden Sie im Leitfaden [Einführung in Wide Color](~/ios/platform/wide-color.md) .

## <a name="newly-available-existing-frameworks"></a>Neu verfügbare vorhandene Frameworks

Mehrere Frameworks, die unter IOS (und nicht in tvos) verfügbar waren, wurden für tvos 10 zur Verfügung gestellt, wie z. b.:

- Externalzubehör
- HomeKit
- MultipeerConnectivity
- Fotos
- ReplayKit
- UserNotification

## <a name="additional-framework-changes"></a>Weitere frameworkänderungen

Zusätzlich zu den oben aufgeführten wichtigen frameworkänderungen und Ergänzungen hat Apple in tvos 10 viele zusätzliche Änderungen am geringfügigen Framework vorgenommen.

Weitere Informationen finden Sie in unserem Handbuch zu [zusätzlichen Framework-Änderungen](~/ios/tvos/platform/introduction-to-tvos10/additional-framework-changes.md) .

## <a name="deprecated-apis"></a>Nicht mehr unterstützte APIs

Keine APIs oder Frameworks wurden von tvos 10 als veraltet markiert. Eine umfassende Liste mit API-Änderungen finden Sie in der Dokumentation zu den [tvos 10 API-unterschieden](https://developer.apple.com/library/prerelease/content/releasenotes/General/tvOS10APIDiffs/index.html) von Apple.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [Neues in tvos 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
