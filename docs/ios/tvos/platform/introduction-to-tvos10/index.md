---
title: "Einführung in die tvos. außerdem wurden 10"
description: "In diesem Artikel werden alle neuen und geänderten-APIs und in tvos. außerdem wurden 10 verfügbaren Funktionen für Entwickler Xamarin.tvOS eingeführt."
ms.topic: article
ms.prod: xamarin
ms.assetid: CB9C1EC8-6008-43AD-977E-976AE7C73DD8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 8bc379e507287cd609dca8440b1210b7f1514114
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-tvos-10"></a>Einführung in die tvos. außerdem wurden 10

_In diesem Artikel werden alle neuen und geänderten-APIs und in tvos. außerdem wurden 10 verfügbaren Funktionen für Entwickler Xamarin.tvOS eingeführt._

Mit der neuen tvos. außerdem wurden hat 10 SDK Apple enthalten neue APIs und Dienste, die den Entwickler so erstellen Sie neue Kategorien von apps und Features zu aktivieren. 

Weitere Informationen zu tvos. außerdem wurden 10, finden Sie im Apple [tvos. außerdem wurden + Apps](https://developer.apple.com/tvos/) Dokumentation.

## <a name="whats-new-in-tvos-10"></a>Was ist neu in tvos. außerdem wurden 10

Apple hat mehrere neue APIs und Dienste in tvos. außerdem wurden 10 sowie zahlreiche Verbesserungen an vorhandenen Funktionen, einschließlich hinzugefügt:

## <a name="new-user-interface-styles"></a>Neuer Benutzer Schnittstelle Stile

tvos. außerdem wurden 10 nun unterstützt einen dunklen und die Benutzeroberfläche Licht Design, dass alle von der Build in UIKit Steuerelemente wird automatisch angepasst, basierend auf die Einstellungen des Benutzers.

Beim Erstellen und neue benutzerdefinierter Benutzeroberflächen-Steuerelemente implementieren, sollten Entwickler verwenden die [UITraitCollection](https://developer.apple.com/reference/uikit/uitraitcollection) Klasse vom Benutzer ausgewählten Design angepasst werden kann.

Weitere Informationen finden Sie unter unsere [neuer Benutzer Schnittstelle Stile](~/ios/tvos/platform/user-interface-styles.md) Dokumentation.

## <a name="security-and-privacy-enhancements"></a>Sicherheit und Datenschutz-Erweiterungen

Apple hat mehrere Erweiterungen auf Sicherheit und Datenschutz für tvos. außerdem wurden 10, mit deren Hilfe werden den Entwickler verbessern Sie die Sicherheit ihrer Apps und des Endbenutzers Datenschutz gewährleisten.

Apps, die unter WatchOS 3 (oder höher) müssen daher statisch deklarieren ihre Absicht auf bestimmte Funktionen oder Benutzerinformationen zugreifen, indem die Eingabe eine oder mehrere bestimmte Datenschutz-Schlüssel in ihre `Info.plist` Dateien, die dem Benutzer erklären, warum die app zugreifen möchte.

Da diese Änderungen mit iOS 10 tvos. außerdem wurden 10 freigegeben hat, finden Sie unter unserer iOS 10 [Sicherheit und Datenschutz Erweiterungen](~/ios/app-fundamentals/security-privacy.md) Weitere Informationen.

## <a name="video-subscriber-account"></a>Video Abonnentenkonto

Neue kann für tvos. außerdem wurden 10, das Video Abonnentenkonto Framework-apps dieser Unterstützung authentifiziert streaming oder Video On Demand, um mit ihren Kabel- oder Satelliten TV-Anbieter, die mit einer einzelnen-anmeldeerfahrung für den Endbenutzer zu authentifizieren.

<!--To find out more, please see our [Video Subscriber Account](~/ios/platform-features/introduction-to-ios10/video-subscriber-account/) guide.-->

## <a name="wide-color"></a>Breite Farbskala

tvos. außerdem wurden 10 erweitert die Unterstützung für erweiterte Schlüsselbereich Pixelformate und Breite Farbskala Farbräumen im gesamten System einschließlich Frameworks wie z. B. Grafiken Core und Core-Image, Metall AVFoundation. Unterstützung für Geräte mit Breite Farbskala zeigt wird weiter Dienstverweise durch dieses Verhalten im ganzen Grafikstapel gesamte bereitstellen.

Darüber hinaus `UIKit` wurde so geändert, arbeiten Sie in der neuen erweiterten **sRGB** Farbraum, die zum Mischen von Farben in wide Farbumfänge ohne signifikante Leistungseinbußen zu erleichtern.

Apple bietet die folgenden bewährten Methoden bei der Arbeit mit breiten Farben:

 - `UIColor` nun verwendet der sRGB Farbspektrum und wird nicht mehr Werte clamp der `0.0` zu `1.0` Bereich. Wenn die app vom vorherigen Clamp Verhalten basiert, müssen sie für tvos. außerdem wurden 10 nicht geändert werden.
 - Wenn die app benutzerdefinierte Rendering führt `UIImages`, verwenden Sie die neue [UIGraphicsImageRender](https://developer.apple.com/reference/uikit/uigraphicsimagerenderer) Klasse, um die Verwendung der erweiterten Bereich oder Standard-Bereich Formate anzugeben.
 - Beim Verwenden einer Low-Level-API z. B. Core Grafiken oder Metall, das eine bildverarbeitung bereitzustellen, sollten die app einen erweiterten Bereich Farbe Speicherplatz und Pixel-Format verwenden, 16-Bit-Gleitkommawerte unterstützt, werden kann. Gegebenenfalls müssen die app manuell Komponente Farbwerte clamp.
 - Core-Grafiken, Core-Image und Metall Leistung Shader alle neue Methoden für die Konvertierung zwischen den zwei Farbräumen bereitstellen.

Wenn Sie mehr erfahren möchten, finden Sie unter unsere [Einführung in die Breite Farbskala](~/ios/platform/wide-color.md) Handbuch.

## <a name="newly-available-existing-frameworks"></a>Neu verfügbare vorhandene Frameworks

Einige Frameworks, die unter iOS (und nicht tvos. außerdem wurden) zur Verfügung standen wurden für tvos. außerdem wurden 10 z. B.:

 - ExternalAccessory
 - HomeKit
 - MultipeerConnectivity
 - Fotos
 - ReplayKit
 - UserNotification

## <a name="additional-framework-changes"></a>Zusätzliche Framework-Änderungen

Zusätzlich zu den wichtigsten Framework Änderungen und Ergänzungen, die oben aufgeführten verfügt über Apple viele zusätzliche kleinere Framework tvos. außerdem wurden 10 geändert.

Wenn Sie mehr erfahren möchten, finden Sie unter unsere [zusätzliche Framework Änderungen](~/ios/tvos/platform/introduction-to-tvos10/additional-framework-changes.md) Handbuch.

## <a name="deprecated-apis"></a>Nicht mehr unterstützte APIs

Keine APIs oder Frameworks wurden durch tvos. außerdem wurden 10 als veraltet markiert. Finden Sie in der Apple- [tvos. außerdem wurden 10 API-Unterschiede](https://developer.apple.com/library/prerelease/content/releasenotes/General/tvOS10APIDiffs/index.html) Dokumentation für eine vollständige Liste der API-Änderungen.



## <a name="related-links"></a>Verwandte Links

- [Beispiele für tvos. außerdem wurden](https://developer.xamarin.com/samples/tvos/all/)
- [Was ist neu in tvos. außerdem wurden 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
