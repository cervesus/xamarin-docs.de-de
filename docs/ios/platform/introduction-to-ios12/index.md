---
title: Einführung in iOS 12
description: Dieses Dokument enthält eine allgemeine Beschreibung einiger iOS-12-APIs, die für die Xamarin Preview Release c#-Bindungen bietet.
ms.prod: xamarin
ms.assetid: 99EA7090-315D-493C-87D3-26AB73D9E1A9
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/08/2018
ms.openlocfilehash: 99f2b98614c2b8d558dd8744b31a62b787fc955c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61035965"
---
# <a name="introduction-to-ios-12"></a>Einführung in iOS 12

Dieses Dokument enthält eine allgemeine Beschreibung einiger iOS-12-APIs, die für die Xamarin Preview Release c#-Bindungen bietet.

Informationen zum Einstieg 12-iOS-apps mit Xamarin erstellen, finden Sie unter den [Handbuch mit ersten Schritten](get-started.md)

## <a name="arkit-2arkit2md"></a>[ARKit 2](arkit2.md)

ARKit werden augmented Reality-Framework mit iOS enthalten ist. ARKit 2 ermöglicht mehreren Benutzern, die miteinander interagieren, in einer Szene augmented Reality-Modus ermöglicht es Objekten im Raum beizubehalten und zu einem späteren Zeitpunkt darauf zurückgeben und bietet 2D bilderkennung und nachverfolgung und 3D objekterkennung. iOS 12 bietet auch AR schnelle Suchen, eine Möglichkeit zum Rendern von Usdz AR-Modelle in Ihren apps.

## <a name="siri-shortcutssiri-shortcutsmd"></a>[Siri-Tastenkombinationen](siri-shortcuts.md)

Siri-Tastenkombinationen können Entwickler ihre Anwendungen in Siri besser zu integrieren. Mit Siri-Verknüpfungen können Benutzer die Sprachbefehle Inhalte öffnen oder das Initiieren von Hintergrundaufgaben oder sie können diese Aufgaben über Tastenkombinationen für die vorgeschlagene Siri auf dem Sperrbildschirm initiieren.

## <a name="core-ml-2coremlmd"></a>[Core ML 2](coreml.md)

Core ML 2 reduziert die Größe der Anwendung über das Modell Quantisierung und flexible Modelle, verbessert die Leistung der Anwendung mit einem neuen batchvorhersage-APIs und benutzerdefinierte Modelle verwendet, um Fortschritte in Machine Learning zu unterstützen.

## <a name="notification-improvementsnotificationsindexmd"></a>[Verbesserungen bei Benachrichtigungen](notifications/index.md)

In iOS 12 ermöglichen gruppierte Benachrichtigungen vorhanden benutzerbenachrichtigungen in-app oder in Bezug auf Threads Gruppierungen. Zusammenfassungstext enthält weitere Informationen zu einer Benachrichtigungsgruppe.

Benutzerdefinierte Benutzeroberflächen und dynamische Aktionsschaltflächen ermöglichen Notification-Content-Erweiterungen in iOS 12.

## <a name="natural-language-frameworknatural-languagemd"></a>[Natürlicher Sprachen-framework](natural-language.md)

Natürlicher Sprachen-Framework ermöglicht Anwendungen das Ausführen von verschiedenen Arten von sprachanalyse. Sie können beispielsweise Wortarten zu identifizieren und bestimmt die Sprache, die durch einen Textblock dargestellt.

## <a name="vision-framework"></a>Maschinelles sehen-framework

Das Vision Framework enthält eine verbesserte gesichtserkennung, die in verschiedenen Ausrichtungen Gesichter erkennen kann. Anforderung Revisionen können auch bestimmte Vision Framework Algorithmusrevision auswählen.

## <a name="photo-and-video-apis"></a>Fotos und video-APIs

In iOS 12 gibt die Segmentierung Hochformat API Hochformat Effekte Maske – eine lineare Maske, die im Vordergrund aus dem Hintergrund der ein Bild im Hochformat grenzt und ist nützlich bei der Erstellung verschiedener Image Effekte zurück. iOS-12 ist es auch ist möglich, Tiefe Daten von der Kamera TrueDepth für Echtzeit-video-Effekte zu verwenden.

## <a name="passwords"></a>Kennwortangriffe

iOS 12 erleichtert es Benutzern und Entwicklern die Arbeit mit Kennwörtern:

- Kennwort AutoAusfüllen und automatische sichere Kennwörter können sie automatisch zu generieren, speichern und verwenden Sie sichere Kennwörter in iOS-Anwendungen, die beim Registrieren für und sich bei einer Anwendung anmelden können.
- Security Code AutoAusfüllen ist es möglich, SMS-basierte Authentifizierung-Codes, ohne manuelle Ausschneiden und einfügen oder merken.
- Die `ASWebAuthenticationSession` -Klasse optimiert den Prozess der Arbeit mit Verbundauthentifizierung-Diensten.
- AutoAusfüllen-Anmeldeinformationsanbieter-Erweiterungen ermöglichen es Drittanbieter-Kennwort-Anwendungen, Benutzernamen und Kennwörter Anmeldefelder anzugeben.

## <a name="healthkit-updates"></a>HealthKit-updates

iOS 11.3 eingeführt [Gesundheitsdaten](https://www.apple.com/healthcare/health-records/), wodurch Benutzer ihre Integrität herunterladen Informationen aus verschiedenen Institutionen aus dem Gesundheitswesen aufzeichnen und auf ihren iOS-Geräten anzeigen. iOS 12 hinzugefügt APIs, die von Drittanbietern, sicheren Zugriff auf diese Daten zu ermöglichen.

## <a name="imessage-app-presentation-contexts"></a>iMessage-App-Präsentation-Kontexten

In iOS 12 unterstützen iMessage-apps Presentation-Kontexten, die die apps zum Ausführen als normaler iMessage-app oder im Rahmen eines Fotos oder Videos wirksam zu ermöglichen.

## <a name="network-framework"></a>Netzwerk-framework

Netzwerk-Framework, das Netzwerk stack zugrunde liegende der `URLSession` APIs, die häufig in iOS-Anwendungen verwendet, ist jetzt verfügbar als eigenständiges Framework, erleichtert Ihnen die Arbeit mit TCP, UDP, TLS, IPv4/IPv6 und mehr.

## <a name="carplay"></a>CarPlay

In iOS 12 können Drittanbieter-apps Karten und Turn-von-Turn navigationsanweisungen CarPlay übermitteln mithilfe des neuen CarPlay-Frameworks.

## <a name="deprecations"></a>Veralteten

In iOS 12 hat Apple als veraltet markiert:

- OpenGL-ES, [und Entwickler werden angeregt](https://developer.apple.com/ios/whats-new/) -Metal-Computern zu übernehmen.
- [`UIWebView`](xref:UIKit.UIWebView), [zugunsten des `WKWebView` ](https://developer.apple.com/documentation/webkit/wkwebview?language=objc).

## <a name="related-links"></a>Verwandte Links

- [Machen Sie sich bereit für iOS-12 (Apple)](https://developer.apple.com/ios/)
