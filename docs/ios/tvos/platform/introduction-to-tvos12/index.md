---
title: Einführung in die tvos. außerdem wurden 12
description: Dieses Dokument bietet einen allgemeinen Überblick über die neuen und aktualisierten Features in tvos. außerdem wurden 12 für Preview-Version der Xamarin derzeit C#-Bindungen bietet.
ms.prod: xamarin
ms.assetid: 037F7FFF-2155-4017-B99A-839CE7EC5C9C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: 03841306ba54e511dbf2f2b86a7c17e9f4669bcd
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2018
ms.locfileid: "37067316"
---
# <a name="introduction-to-tvos-12"></a>Einführung in die tvos. außerdem wurden 12

![Vorschau](~/media/shared/preview.png)

> [!WARNING]
> Die Xamarin tvos. außerdem wurden 12-Unterstützung ist zurzeit als Vorschau verfügbar, d. h., er möglicherweise Fehler enthalten ist nicht die Funktion abgeschlossen, und kann geändert werden. Verwenden Sie diese für nur experimentieren.

> [!NOTE]
> - Überprüfen Sie die [Einstieg](~/ios/platform/introduction-to-ios12/get-started.md) Handbuch eine Anleitung zum Erstellen von iOS-12 und tvos. außerdem wurden 12-apps mit Xamarin beginnen.
> - Weitere Informationen finden Sie unter der [Anmerkungen zu dieser Version](https://releases.xamarin.com/preview-release-xcode-10-beta/) für die Xamarin-Vorschauversion.

Dieses Dokument enthält eine allgemeine Übersicht über neue und aktualisierte tvos. außerdem wurden 12 für die Xamarin-Preview Release derzeit c# Bindungen bereitgestellten Funktionen.

## <a name="password-autofill"></a>Das Kennwort automatisch ausfüllen

Mit tvos. außerdem 12 wurden können Benutzer ihre iOS-Geräte verwenden, zu einer app tvos. außerdem wurden mit einem einfachen tippen anmelden. Dies erfolgt über eine Kombination von `UITextContentType` Felder Nutzung Benutzernamen und ein Kennwort angeben, die zugeordnete Domänen zum Herstellen einer Beziehung zwischen einer iOS-app und einer app tvos. außerdem wurden und wählen Sie ein Element aus, um den Fokus erhalten, nachdem ein Benutzer bevorzugte Fokus-Umgebungen Stellt einen Benutzernamen und ein Kennwort an.

## <a name="focus-engine-enhancements"></a>Datenbankmodul-Verbesserungen konzentrieren

tvos. außerdem wurden 12 können alle apps, unabhängig davon, wie sie für die Interaktion mit dem Fokus Modul gerendert werden, an. Durch einen Benutzer Interaktionen mit der Remoteinstanz Siri kann das Modul den Fokus verwendet werden mit einer anderen app wählen Sie ein Element, auf mögliche fokusänderungen-Hinweis und natürliche Weise aktualisieren Sie den Fokus. Dies erfolgt in benutzerdefinierten Anwendungen über die UIKit `IUIFocusItemContainer` -Schnittstelle, die `UIFocusMovementHint` -Klasse, die `IUIFocusItemScrollableContainer` -Schnittstelle, und anderen verwandten Klassen und Methoden.

## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvos. außerdem wurden – Apple Developer (Apple)](https://developer.apple.com/tvos/)
- [Was ist neu in tvos. außerdem wurden 12 (Apple) (video)](https://developer.apple.com/videos/play/wwdc2018/208/)
- [TV (Apple)](https://www.apple.com/tv/)
- Xamarin-Vorschau [Anmerkungen zu dieser Version](https://releases.xamarin.com/preview-release-xcode-10-beta/)
