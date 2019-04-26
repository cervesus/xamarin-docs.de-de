---
title: Einführung in tvOS 12
description: Dieses Dokument enthält eine allgemeine Übersicht der neuen und aktualisierten Funktionen in TvOS 12 für die Xamarin Preview-Version aktuell c#-Bindungen enthält.
ms.prod: xamarin
ms.assetid: 037F7FFF-2155-4017-B99A-839CE7EC5C9C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 10/05/2018
ms.openlocfilehash: f7fb8cc379a070b848c5154c9c1d4fbfc8186266
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "60932540"
---
# <a name="introduction-to-tvos-12"></a>Einführung in tvOS 12

Dieses Dokument enthält eine allgemeine Übersicht über neue und aktualisierte TvOS 12.

Informationen zum Einstieg TvOS-12-apps mit Xamarin erstellen, sehen Sie sich die [Handbuch mit ersten Schritten](~/ios/platform/introduction-to-ios12/get-started.md).

## <a name="tvuikit"></a>TVUIKit

TvOS 12 enthält TVUIKit, einen Satz von APIs, die für TvOS-Entwicklern, TvOS-Standardsteuerelemente, z. B. Poster Ansichten, Titelleistenschaltflächen, Smartcard-Ansichten und Monogramm Ansichten verwenden können. TvOS 12 führt außerdem eine Eigenschaft, die können Beschriftungen, Text zu blättern, die vollständig sichtbar ist zu lang ist.

## <a name="password-autofill"></a>Das Kennwort automatisch ausfüllen

Mit TvOS 12 können Benutzer ihre iOS-Geräte verwenden, für die Anmeldung zu einer TvOS-app mit einem einzigen fingertipp. Dies erfolgt durch eine Kombination von `UITextContentType` zugehörige Domänen, die zum Erstellen einer Beziehung zwischen einer iOS-app und einer TvOS-app und bevorzugte Fokus Umgebungen auf ein Element, das Fokus erhalten, nachdem ein Benutzer mit Benutzername und ein Kennwort angeben Feldern Stellt einen Benutzernamen und ein Kennwort an.

## <a name="focus-engine-enhancements"></a>Erweiterungen der Fokus-Engine

TvOS-12 kann alle apps, unabhängig davon, wie sie für die Interaktion mit der Engine Fokus gerendert werden, an. Über die Interaktionen des Benutzers mit dem Remoterepository Siri kann der Fokus-Engine verwendet werden mit einer anderen app wählen Sie ein Element, Hinweise zur möglichen fokusänderungen und den Fokus auf natürliche Weise zu aktualisieren. Dieses Verhalten wird aktiviert, in benutzerdefinierten Anwendungen über die UIKit `IUIFocusItemContainer` -Schnittstelle, die `UIFocusMovementHint` -Klasse, die `IUIFocusItemScrollableContainer` -Schnittstelle, und anderen verwandten Klassen und Methoden.

## <a name="vision-framework"></a>Maschinelles sehen-framework

Das Vision Framework enthält eine verbesserte gesichtserkennung, die in verschiedenen Ausrichtungen Gesichter erkennen kann. Darüber hinaus können Anforderung Revisionen jetzt verwendet werden, um eine bestimmte Vision Framework Algorithmusrevision auszuwählen.

## <a name="natural-language-framework"></a>Natürlicher Sprachen-framework

Natürlicher Sprachen-Framework ermöglicht Anwendungen das Ausführen von verschiedenen Arten von sprachanalyse. Beispielsweise kann verwendet werden, zum Identifizieren von Wortarten, und ermitteln die Sprache, die durch einen Textblock dargestellt wird.

## <a name="deprecations"></a>Veralteten

Mit TvOS 12 veraltet Apple OpenGL-ES, [und Entwickler werden angeregt](https://developer.apple.com/tvos/whats-new/) -Metal-Computern zu übernehmen.

## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [TvOS-Apple-Entwickler (Apple)](https://developer.apple.com/tvos/)
- [Neuerungen in TvOS-12 (Apple) (video)](https://developer.apple.com/videos/play/wwdc2018/208/)
- [TV (Apple)](https://www.apple.com/tv/)