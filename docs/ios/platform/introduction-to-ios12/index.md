---
title: Einführung in iOS 12
description: Dieses Dokument enthält eine allgemeine Beschreibung einiger IOS 12-APIs, für die die Vorschauversion von xamarin Bindungen C# bereitstellt.
ms.prod: xamarin
ms.assetid: 99EA7090-315D-493C-87D3-26AB73D9E1A9
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 07/08/2018
ms.openlocfilehash: 39d954626bc9e789446e7f1deac67e2e0fca51c8
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032005"
---
# <a name="introduction-to-ios-12"></a>Einführung in iOS 12

Dieses Dokument enthält eine allgemeine Beschreibung einiger IOS 12-APIs, für die die Vorschauversion von xamarin Bindungen C# bereitstellt.

Informationen zu den ersten Schritten beim Entwickeln von IOS 12-apps mit xamarin finden Sie im [Leitfaden](get-started.md) zu den ersten Schritten

## <a name="arkit-2arkit2md"></a>[ARKit 2](arkit2.md)

Arkit ist das in ios enthaltene erweiterte Reality-Framework. Arkit 2 ermöglicht es, dass mehrere Benutzer in einer erweiterten Reality-Szene miteinander interagieren können, um Objekte im Raum beizubehalten und zu einem späteren Zeitpunkt zu diesen zu wechseln und die 2D-Bild Erkennung und-Nachverfolgung sowie 3D-Objekterkennung bereitzustellen. IOS 12 bietet auch eine einfache Schnellsuche, eine Möglichkeit zum Rendering von in ihren apps bereitgestellten in ihren apps.

## <a name="siri-shortcutssiri-shortcutsmd"></a>[Siri-Tastenkombinationen](siri-shortcuts.md)

Mithilfe von Siri-Verknüpfungen können Entwickler ihre Anwendungen besser in Siri integrieren. Mit Siri-Verknüpfungen können Benutzer Sprachbefehle verwenden, um Inhalte zu öffnen oder Hintergrundaufgaben zu initiieren, oder Sie können diese Aufgaben über Verknüpfungen initiieren, die Siri auf dem Sperrbildschirm vorschlägt.

## <a name="core-ml-2coremlmd"></a>[Core ML 2](coreml.md)

Core ml 2 reduziert die Anwendungs Größe durch Modell Quantisierung und flexible Modelle, verbessert die Anwendungsleistung mit einer neuen Batch-Vorhersage-API und verwendet benutzerdefinierte Modelle, um den Fortschritt beim maschinellen lernen zu unterstützen.

## <a name="notification-improvementsnotificationsindexmd"></a>[Benachrichtigungs Verbesserungen](notifications/index.md)

In IOS 12 ermöglichen gruppierte Benachrichtigungen das präsentieren von Benutzer Benachrichtigungen in App-oder Thread bezogenen Gruppierungen. Zusammenfassender Text enthält weitere Informationen zu einer Benachrichtigungs Gruppe.

Erweiterungen für Benachrichtigungs Inhalte in IOS 12 erlauben benutzerdefinierte Benutzeroberflächen und Schaltflächen für dynamische Aktionen.

## <a name="natural-language-frameworknatural-languagemd"></a>[Framework für natürliche Sprache](natural-language.md)

Das Framework für natürliche Sprache ermöglicht Anwendungen das Ausführen verschiedener Arten von Sprachanalysen. Sie kann z. b. Teile der Sprache identifizieren und die durch einen TextBlock dargestellte Sprache bestimmen.

## <a name="vision-frameworkiosplatformintroduction-to-ios11visionmd"></a>[Vision Framework](~/ios/platform/introduction-to-ios11/vision.md)

Das Vision-Framework enthält ein verbessertes Gesichts Erkennungs Modul, mit dem Gesichter in verschiedenen Ausrichtungen erkannt werden können. Außerdem können Anforderungs Revisionen eine bestimmte Vision Framework-Algorithmusrevision auswählen.

## <a name="photo-and-video-apis"></a>Foto-und Video-APIs

In IOS 12 gibt die Hochformat Segmentierung-API eine Matte für Hochformat Effekte zurück – eine lineare Maske, die den Vordergrund vom Hintergrund eines Hochformat Bilds abgleicht und zum Erstellen verschiedener Bildeffekte nützlich ist. IOS 12 macht es auch möglich, Tiefe Daten von der truetiefen Kamera für echt Zeit Video Effekte zu verwenden.

## <a name="passwords"></a>Kennwortangriffe

IOS 12 erleichtert Benutzern und Entwicklern die Arbeit mit Kenn Wörtern:

- Kenn Wort AutoFill und automatische sichere Kenn Wörter ermöglichen das automatische generieren, speichern und Verwenden von sicheren Kenn Wörtern in ios-Anwendungen, wenn Sie sich für anmelden und sich bei einer Anwendung anmelden.
- Mithilfe der AutoFill-Methode für den Sicherheits Code können Sie SMS-basierte Authentifizierungscodes verwenden, ohne manuelle Ausschnitten und einfügevorgehensweisen bzw.
- Die `ASWebAuthenticationSession`-Klasse optimiert den Prozess der Arbeit mit Verbund Authentifizierungsdiensten.
- Autofill-Anmelde Informationsanbieter-Erweiterungen ermöglichen es, dass Kenn Wort Anwendungen von Drittanbietern Benutzernamen und Kenn Wörter für Anmelde Felder bereitstellen.

## <a name="healthkit-updates"></a>Healthkit-Updates

IOS 11,3 hat Integritäts [Datensätze](https://www.apple.com/healthcare/health-records/)eingeführt, die es Benutzern ermöglichen, ihre Integritäts Datensatzinformationen aus verschiedenen Gesundheitseinrichtungen herunterzuladen und auf Ihren IOS-Geräten anzuzeigen. IOS 12 fügt APIs hinzu, mit denen Anwendungen von Drittanbietern sicher auf diese Daten zugreifen können.

## <a name="imessage-app-presentation-contexts"></a>IMESS Age-App-Präsentations Kontexte

In IOS 12 unterstützen IMESS Age-apps Präsentations Kontexte, mit denen die apps als normale IMESS-APP oder im Kontext eines Foto-oder Video Effekts ausgeführt werden können.

## <a name="network-framework"></a>Netzwerk Framework

Netzwerk Framework, der Netzwerk Stapel, der den `URLSession` APIs zugrunde liegt, die häufig in ios-Anwendungen verwendet werden, ist nun als eigenständiges Framework verfügbar und vereinfacht die Arbeit mit TCP, UDP, TLS, IPv4/IPv6 usw.

## <a name="carplay"></a>Carplay

In IOS 12 können Drittanbieter-apps Zuordnungen und Turn-by-Turn-Navigationsanweisungen in carplay mithilfe des neuen carplay-Frameworks bereitzustellen.

## <a name="deprecations"></a>Veraltete Funktionen

Bei IOS 12 hat Apple Folgendes als veraltet markiert:

- OpenGL es ist, [Entwickler](https://developer.apple.com/ios/whats-new/) bei der Übernahme von Metal zu unterstützen.
- [`UIWebView`](xref:UIKit.UIWebView), [zugunsten `WKWebView`](https://developer.apple.com/documentation/webkit/wkwebview?language=objc).
