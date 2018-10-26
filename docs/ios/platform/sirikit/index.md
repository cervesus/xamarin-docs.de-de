---
title: SiriKit in Xamarin.iOS
description: In diesem Artikel veranschaulicht SiriKit in einer Xamarin.iOS-app verwenden, um Dienste bereitzustellen, die für den Benutzer mithilfe von Siri auf einem iOS-Gerät zugänglich sind.
ms.prod: xamarin
ms.assetid: 84E5681A-F557-4967-AA99-F831169157AA
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 9f7cbb3f7d9e448947ec8163a8660616910e750f
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50117524"
---
# <a name="sirikit-in-xamarinios"></a>SiriKit in Xamarin.iOS

_In diesem Artikel veranschaulicht SiriKit in einer Xamarin.iOS-app verwenden, um Dienste bereitzustellen, die für den Benutzer mithilfe von Siri auf einem iOS-Gerät zugänglich sind._

Neue iOS 10, SiriKit kann eine iOS-app, um Dienste bereitzustellen, die für den Benutzer mithilfe von Siri und die Karten-app auf einem iOS-Gerät mithilfe von App-Erweiterungen und neuen zugänglich sind **Intents** und **Intents UI** Frameworks.

Siri arbeitet mit dem Konzept der **Domänen**, wissen, Gruppen von Aktionen für verwandte Aufgaben. Jede Interaktion, die eine app mit Siri muss wie folgt in eine Domäne bekannten Diensts liegen:

- Audio- oder Videoanruf.
- Buchung einer Tour an.
- Verwalten von fitnessaktivitäten aus.
- Messaging.
- Durchsuchen von Fotos.
- Senden oder Empfangen von Zahlungen.

Wenn der Benutzer eine Anforderung von Siri im Zusammenhang mit einer der Dienste für eine App-Erweiterung stellt, sendet SiriKit die Erweiterung einer **Absicht** Objekt, das die Anforderung des Benutzers sowie alle unterstützungsdaten beschreibt. Die App-Erweiterung generiert dann das entsprechende **Antwort** -Objekt für den angegebenen **Absicht**, mit Informationen, wie die Erweiterung für die Anforderung verarbeiten kann.

## <a name="understanding-sirikit-conceptsiosplatformsirikitunderstanding-sirikitmd"></a>[Grundlegendes zu SiriKit-Konzepten](~/ios/platform/sirikit/understanding-sirikit.md)

Dieser Artikel behandelt die grundlegenden Konzepte, die für die Arbeit mit SiriKit in einer Xamarin.iOS-app benötigt werden. Hierin sind die neuen Intents und Intents UI-Erweiterungspunkte und wie sie mit der App und das Vokabular der Benutzer zum Öffnen einer app siri funktionieren.

## <a name="implementing-sirikitiosplatformsirikitimplementing-sirikitmd"></a>[Implementieren von SiriKit](~/ios/platform/sirikit/implementing-sirikit.md)

Dieser Artikel behandelt die erforderlichen Schritte zum Implementieren von SiriKit-Unterstützung in einem Xamarin.iOS-apps. Entwickler sollten im Handbuch "Grundlegendes zu SiriKit-Konzepten" oben lesen, bevor Sie versuchen, SiriKit-Unterstützung hinzufügen zu einer app, als Schlüssel, die Konzepte behandelt werden, die für die erfolgreiche Implementierung benötigt werden.





## <a name="related-links"></a>Verwandte Links

- [ElizaChat-Beispiel](https://developer.xamarin.com/samples/monotouch/ios10/ElizaChat/)
- [SiriKit-Programmierhandbuch](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [Frameworkverweis Intents](https://developer.apple.com/reference/intents)
- [Referenz für Intents UI-Framework](https://developer.apple.com/reference/intentsui)
