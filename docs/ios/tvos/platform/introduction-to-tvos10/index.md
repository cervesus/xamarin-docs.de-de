---
title: Einführung in TvOS 10
description: Dieser Artikel enthält alle neuen und geänderten APIs und in TvOS 10 verfügbaren Features für Entwickler mit Xamarin.tvOS.
ms.prod: xamarin
ms.assetid: CB9C1EC8-6008-43AD-977E-976AE7C73DD8
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 260d01d6aa8344dd3cf107f1ffc34167c457a491
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120982"
---
# <a name="introduction-to-tvos-10"></a>Einführung in TvOS 10

_Dieser Artikel enthält alle neuen und geänderten APIs und in TvOS 10 verfügbaren Features für Entwickler mit Xamarin.tvOS._

Neues TvOS hat 10 SDK Apple enthalten neue APIs und Dienste, die den Entwickler zum Erstellen von neuer Kategorien von apps und Features zu ermöglichen. 

Weitere Informationen zu TvOS 10, finden Sie unter Apple [TvOS + Apps](https://developer.apple.com/tvos/) Dokumentation.

## <a name="whats-new-in-tvos-10"></a>Was ist neu in TvOS 10

Apple hat verschiedene neue APIs und Dienste in TvOS 10 sowie zahlreiche Verbesserungen an vorhandenen Funktionen, einschließlich hinzugefügt:

## <a name="new-user-interface-styles"></a>Neue Stile der Benutzeroberfläche

TvOS 10 nun unterstützt sowohl die Light-Benutzeroberfläche ein dunkles Design, dass alle von der integrierten UIKit Steuerelemente wird automatisch angepasst, basierend auf den Einstellungen des Benutzers.

Beim Erstellen und Implementieren neuer benutzerdefinierter Benutzeroberflächen-Steuerelemente, die Entwickler verwenden, sollten die [UITraitCollection](https://developer.apple.com/reference/uikit/uitraitcollection) Klasse zur Anpassung an ausgewählten Designs des Benutzers.

Weitere Informationen finden Sie unserem [neue Stile der Benutzeroberfläche](~/ios/tvos/platform/user-interface-styles.md) Dokumentation.

## <a name="security-and-privacy-enhancements"></a>Sicherheit und Datenschutz-Verbesserungen

Apple hat mehrere Verbesserungen vorgenommen, sowohl Sicherheit und Datenschutz in TvOS 10, die den Entwickler verbessern Sie die Sicherheit ihrer Apps und des Endbenutzers Datenschutz gewährleisten, die helfen.

Apps für WatchOS 3 (oder höher) müssen daher statisch deklarieren ihrer Absicht, bestimmte Features oder Benutzerinformationen zugreifen, indem Sie die Eingabe eine oder mehrere bestimmte Datenschutzschlüssel in ihre `Info.plist` Dateien, die dem Benutzer erklären, warum die app zugreifen möchte.

Da diese Änderungen mit iOS 10 TvOS 10 freigegeben hat, finden Sie unserem iOS 10 [Verbesserungen Sicherheit und Datenschutz](~/ios/app-fundamentals/security-privacy.md) Anleitung finden Sie weitere Informationen.

## <a name="video-subscriber-account"></a>Video-Abonnentenkonto

Neue kann für TvOS 10 und der Video-Abonnentenkonto Framework apps diese Unterstützung authentifiziert streaming oder Video On Demand, um bei ihrem Kabel- oder Satelliten TV-Anbieter, die mit einer einzelnen-Anmeldung für den Endbenutzer zu authentifizieren.

<!--To find out more, please see our [Video Subscriber Account](~/ios/platform-features/introduction-to-ios10/video-subscriber-account/) guide.-->

## <a name="wide-color"></a>Breite Farbskala

TvOS 10 erweitert die Unterstützung für erweiterte-Range-Pixelformate "und" Wide-Farbskala Farbräume im gesamten System einschließlich Frameworks wie Core Graphics "," Core-Image ","-Metal-Computern "und" AVFoundation. Unterstützung für Geräte mit Breite Farbskala zeigt wird weiter durch die Bereitstellung dieses Verhalten in der gesamten Grafikstapel geringer.

Darüber hinaus `UIKit` wurde geändert, arbeiten Sie in der neuen erweiterten **sRGB** Farbraum, erleichtert Ihnen die zum Mischen von Farben in der Breite Farbskala Farbumfänge ohne signifikante Leistungseinbußen zur Folge.

Apple bietet die folgenden bewährten Methoden bei der Arbeit mit breiten Farben:

 - `UIColor` nun verwendet der sRGB-Farbraum und nicht mehr-Werte Clamp, die `0.0` zu `1.0` Bereich. Wenn die app vom vorherigen clamps-Verhalten basiert, müssen sie für TvOS 10 geändert werden.
 - Wenn die Anwendung benutzerdefinierte Rendering von führt `UIImages`, verwenden Sie die neue [UIGraphicsImageRender](https://developer.apple.com/reference/uikit/uigraphicsimagerenderer) Klasse, um die Verwendung der Formate erweiterte-Range- oder Standard-Bereich anzugeben.
 - Eine Low-Level-API, z. B. die wichtigste Grafik oder Metall mit bildverarbeitung bereitstellen, sollte die app eine größere Anzahl Farbe Speicherplatz und Pixel-Format verwenden, 16-Bit-Gleitkommawerte unterstützt, werden kann. Bei Bedarf müssen die app manuell Farbkomponentenwerte clamp.
 - Wichtigste Grafik "," Core-Image "und"-Metal-Leistung Shader neue Methoden bieten für die Konvertierung zwischen den zwei Farbräume ".

Wenn Sie mehr erfahren möchten, informieren Sie sich unsere [Einführung in die Breite Farbskala](~/ios/platform/wide-color.md) Guide.

## <a name="newly-available-existing-frameworks"></a>Neu verfügbare vorhandenen Frameworks

Mehrere Frameworks, die unter iOS (und nicht TvOS) verfügbar waren wurden für TvOS 10 verfügbar gemacht z. B.:

 - ExternalAccessory
 - HomeKit
 - MultipeerConnectivity
 - Fotos
 - ReplayKit
 - UserNotification

## <a name="additional-framework-changes"></a>Zusätzliche-Frameworkänderungen

Zusätzlich zu den wichtigsten Framework-Änderungen und Ergänzungen, die oben aufgeführten hat Apple viele weitere kleinere frameworkänderungen in TvOS 10 vorgenommen.

Um mehr zu erfahren, informieren Sie sich unsere [zusätzliche-Frameworkänderungen](~/ios/tvos/platform/introduction-to-tvos10/additional-framework-changes.md) Guide.

## <a name="deprecated-apis"></a>Nicht mehr unterstützte APIs

Keine APIs oder Frameworks wurden TvOS 10 als veraltet markiert. Finden Sie unter Apple [TvOS 10 API-Unterschiede](https://developer.apple.com/library/prerelease/content/releasenotes/General/tvOS10APIDiffs/index.html) Dokumentation für eine vollständige Liste der API-Änderungen.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [Neuerungen in TvOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
