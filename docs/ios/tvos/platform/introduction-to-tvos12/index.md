---
title: Einführung in tvOS 12
description: Dieses Dokument enthält eine Übersicht über neue und aktualisierte Features in tvos 12, für die das xamarin-Vorschau Release derzeit Bindungen bereitstellt C# .
ms.prod: xamarin
ms.assetid: 037F7FFF-2155-4017-B99A-839CE7EC5C9C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 10/05/2018
ms.openlocfilehash: 94fea1786497d04602ea6cf06d875206cf69eb3e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030552"
---
# <a name="introduction-to-tvos-12"></a>Einführung in tvOS 12

Dieses Dokument enthält eine allgemeine Übersicht über neue und aktualisierte tvos 12.

Informationen zu den ersten Schritten beim Erstellen von tvos 12-apps mit xamarin finden Sie im [Leitfaden](~/ios/platform/introduction-to-ios12/get-started.md)für die ersten Schritte.

## <a name="tvuikit"></a>Tvuikit

tvos 12 umfasst tvuikit, eine Reihe von APIs, die es tvos-Entwicklern ermöglichen, gängige tvos-Steuerelemente wie z. b. Poster-Ansichten, Beschriftungs Schaltflächen, Kartenansichten und Monogram-Sichten zu verwenden. tvos 12 führt außerdem eine Eigenschaft ein, mit der Bezeichnungen einen Bildlauf durchführen können, der für eine vollständige Sichtbarkeit zu lang ist.

## <a name="password-autofill"></a>Kenn Wort AutoFill

Mit tvos 12 können sich Benutzer mit ihren IOS-Geräten bei einer tvos-App mit einer einzigen Tap anmelden. Dies wird durch eine Kombination aus `UITextContentType` Verwendung zum Angeben von Benutzername-und Kenn Wort Feldern, zugeordneten Domänen, um eine Beziehung zwischen einer IOS-APP und einer tvos-App herzustellen, und bevorzugten Fokus Umgebungen, um ein Element auszuwählen, das nach einem Benutzer den Fokus erhält. Gibt einen Benutzernamen und ein Kennwort an.

## <a name="focus-engine-enhancements"></a>Erweiterungen der Fokus-Engine

tvos 12 ermöglicht die Interaktion mit der Fokus-Engine für alle apps, unabhängig davon, wie Sie gerendert werden. Durch die Interaktion eines Benutzers mit der Siri-Remote-Engine kann die Fokus-Engine mit jeder beliebigen App verwendet werden, um ein Element auszuwählen, mögliche Fokus Änderungen zu berücksichtigen und den Fokus natürlich zu aktualisieren. Dies ist in benutzerdefinierten Anwendungen über die `IUIFocusItemContainer`-Schnittstelle von UIKit, die `UIFocusMovementHint`-Klasse, die `IUIFocusItemScrollableContainer`-Schnittstelle und andere verwandte Klassen und Methoden aktiviert.

## <a name="vision-framework"></a>Vision Framework

Das Vision-Framework enthält ein verbessertes Gesichts Erkennungs Modul, mit dem Gesichter in verschiedenen Ausrichtungen erkannt werden können. Außerdem können Anforderungs Revisionen nun verwendet werden, um eine bestimmte Vision Framework-Algorithmusrevision auszuwählen.

## <a name="natural-language-framework"></a>Framework für natürliche Sprache

Das Framework für natürliche Sprache ermöglicht Anwendungen das Ausführen verschiedener Arten von Sprachanalysen. Beispielsweise kann Sie verwendet werden, um Teile der Sprache zu identifizieren und die durch einen TextBlock dargestellte Sprache zu bestimmen.

## <a name="deprecations"></a>Veraltete Funktionen

Mit tvos 12 hat Apple veraltete OpenGL es als veraltet eingestuft und [Entwickler dazu ermutigt](https://developer.apple.com/tvos/whats-new/) , Metal zu übernehmen.

## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvos – Apple Developer (Apple)](https://developer.apple.com/tvos/)
- [Neues in tvos 12 (Apple) (Video)](https://developer.apple.com/videos/play/wwdc2018/208/)
- [TV (Apple)](https://www.apple.com/tv/)
