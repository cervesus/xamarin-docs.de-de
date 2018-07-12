---
title: Einführung in TvOS 12
description: Dieses Dokument enthält eine allgemeine Übersicht der neuen und aktualisierten Funktionen in TvOS 12 für die Xamarin Preview-Version aktuell c#-Bindungen enthält.
ms.prod: xamarin
ms.assetid: 037F7FFF-2155-4017-B99A-839CE7EC5C9C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: 03841306ba54e511dbf2f2b86a7c17e9f4669bcd
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38847560"
---
# <a name="introduction-to-tvos-12"></a>Einführung in TvOS 12

![Vorschau](~/media/shared/preview.png)

> [!WARNING]
> Xamarin TvOS-12-Unterstützung ist gegenwärtig im vorschaustadium, d. h., er möglicherweise Fehler enthalten ist nicht die Features, und kann sich ändern. Verwenden sie nur für Experimente.

> [!NOTE]
> - Überprüfen Sie die [Einstieg](~/ios/platform/introduction-to-ios12/get-started.md) Guide finden Sie Anweisungen zum Einstieg zur Erstellung von iOS-12 und 12-TvOS-apps mit Xamarin.
> - Weitere Informationen finden Sie in der [Anmerkungen zu dieser Version](https://releases.xamarin.com/preview-release-xcode-10-beta/) für die Xamarin-Vorschauversion.

Dieses Dokument enthält eine allgemeine Übersicht über neue und aktualisierte TvOS 12 Features, die für die Xamarin, die Preview Release aktuell c#-Bindungen bietet.

## <a name="password-autofill"></a>Das Kennwort automatisch ausfüllen

Mit TvOS 12 können Benutzer ihre iOS-Geräte verwenden, für die Anmeldung zu einer TvOS-app mit einem einzigen fingertipp. Dies erfolgt durch eine Kombination von `UITextContentType` zugehörige Domänen, die zum Erstellen einer Beziehung zwischen einer iOS-app und einer TvOS-app und bevorzugte Fokus Umgebungen auf ein Element, das Fokus erhalten, nachdem ein Benutzer mit Benutzername und ein Kennwort angeben Feldern Stellt einen Benutzernamen und ein Kennwort an.

## <a name="focus-engine-enhancements"></a>Erweiterungen der Fokus-Engine

TvOS-12 kann alle apps, unabhängig davon, wie sie für die Interaktion mit der Engine Fokus gerendert werden, an. Über die Interaktionen des Benutzers mit dem Remoterepository Siri kann der Fokus-Engine verwendet werden mit einer anderen app wählen Sie ein Element, Hinweise zur möglichen fokusänderungen und den Fokus auf natürliche Weise zu aktualisieren. Dieses Verhalten wird aktiviert, in benutzerdefinierten Anwendungen über die UIKit `IUIFocusItemContainer` -Schnittstelle, die `UIFocusMovementHint` -Klasse, die `IUIFocusItemScrollableContainer` -Schnittstelle, und anderen verwandten Klassen und Methoden.

## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [TvOS-Apple-Entwickler (Apple)](https://developer.apple.com/tvos/)
- [Neuerungen in TvOS-12 (Apple) (video)](https://developer.apple.com/videos/play/wwdc2018/208/)
- [TV (Apple)](https://www.apple.com/tv/)
- Xamarin-Vorschauversion [Anmerkungen zu dieser Version](https://releases.xamarin.com/preview-release-xcode-10-beta/)
