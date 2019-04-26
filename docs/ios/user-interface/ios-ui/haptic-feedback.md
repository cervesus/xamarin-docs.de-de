---
title: Übermitteln von Haptischem Feedback in Xamarin.iOS
description: Dieses Dokument beschreibt, wie Übermitteln von haptischem Feedback in einer Xamarin.iOS-app bereitgestellt wird. Es wird erläutert, UIImpactFeedbackGenerator UINotificationFeedbackGenerator und UISelectionFeedbackGenerator.
ms.prod: xamarin
ms.assetid: 888106D1-58F4-453F-BACC-91D51FA39C80
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: b2c381c59ba1574e80babc2c7e68535a3deffe35
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61384618"
---
# <a name="providing-haptic-feedback-in-xamarinios"></a>Übermitteln von Haptischem Feedback in Xamarin.iOS

<a name="Overview" />

## <a name="overview"></a>Übersicht

Auf dem iPhone 7 und das iPhone hat 7 und Apple enthalten neue Übermitteln von haptischem Antworten, die weitere Möglichkeiten, die sich physisch in Verbindung setzen den Benutzer bereitstellen. Übermitteln von haptischem Feedback (oft einfach als Haptics bezeichnet) wird die Bedeutung von Touch (über erzwingen, Vibrationen oder während der Übertragung) im Entwurf der Benutzeroberfläche verwendet. Verwenden Sie diese neuen praktisch Feedbackoptionen, um die Aufmerksamkeit des Benutzers zu erhalten, und vertiefen ihre Aktionen.

Die folgenden Themen werden im Detail behandelt:

- [Zum Übermitteln von Haptischem Feedback](#About-Haptic-Feedback)
- [UIImpactFeedbackGenerator](#UIImpactFeedbackGenerator)
- [UINotificationFeedbackGenerator](#UINotificationFeedbackGenerator)
- [UISelectionFeedbackGenerator](#UISelectionFeedbackGenerator)

<a name="About-Haptic-Feedback" />

## <a name="about-haptic-feedback"></a>Zum Übermitteln von Haptischem Feedback

Einige integrierte Elemente der Benutzeroberfläche bieten bereits Übermitteln von haptischem Feedback wie z. B. Dateiöffnungs-, Switches und Schieberegler. iOS 10 bietet nun die Möglichkeit, programmgesteuert mithilfe einer konkreten Unterklasse von Haptics Auslösen der `UIFeedbackGenerator` Klasse.

Der Entwickler mithilfe einer der folgenden `UIFeedbackGenerator` Unterklassen zu programmgesteuert Trigger Übermitteln von haptischem Feedback:

- `UIImpactFeedbackGenerator` – Verwenden Sie dieses Feedback-Generators, um eine Aktion oder Aufgabe, z. B. eine "Thud" darstellen, wenn eine Ansicht an Stelle Folien oder beiden auf dem Bildschirm Objekte kollidieren zu ergänzen.
- `UINotificationFeedbackGenerator` -Verwenden Sie dieses Feedback-Generators für Benachrichtigungen, wie z. B. eine Aktion abschließen, fehlerhaften oder andere die Art der Warnung.
- `UISelectionFeedbackGenerator` – Verwenden Sie dieses Feedback-Generators für eine Auswahl aktiv ändern, z. B. ein Element aus einer Liste auswählen.

<a name="UIImpactFeedbackGenerator" />

### <a name="uiimpactfeedbackgenerator"></a>UIImpactFeedbackGenerator

Verwenden Sie dieses Feedback-Generators, um eine Aktion oder Aufgabe, z. B. eine "Thud" darstellen, wenn eine Ansicht an Stelle Folien oder beiden auf dem Bildschirm Objekte kollidieren zu ergänzen.

Verwenden Sie den folgenden Code zum Trigger Auswirkungen Feedback:

```csharp
using UIKit;
...

// Initialize feedback
var impact = new UIImpactFeedbackGenerator (UIImpactFeedbackStyle.Heavy);
impact.Prepare ();

// Trigger feedback
impact.ImpactOccurred ();
```

Wenn der Entwickler erstellt eine neue Instanz der der `UIImpactFeedbackGenerator` Klasse, die sie bieten eine `UIImpactFeedbackStyle` die Stärke des Feedbacks als angeben:

- `Heavy`
- `Medium`
- `Light`

Die `Prepare` Methode der `UIImpactFeedbackGenerator` wird aufgerufen, um das System zu informieren, dass das Übermitteln von haptischem Feedback wird durchgeführt, damit sie die Latenz minimieren kann.

Die `ImpactOccurred` Methode löst dann Übermitteln von haptischem Feedback.

<a name="UINotificationFeedbackGenerator" />

### <a name="uinotificationfeedbackgenerator"></a>UINotificationFeedbackGenerator

Verwenden Sie dieses Feedback-Generators für Benachrichtigungen, wie z. B. eine Aktion abschließen, fehlerhaften oder andere die Art der Warnung.

Verwenden Sie den folgenden Code zur Notification-Feedback Trigger:

```csharp
using UIKit;
...

// Initialize feedback
var notification = new UINotificationFeedbackGenerator ();
notification.Prepare ();

// Trigger feedback
notification.NotificationOccurred (UINotificationFeedbackType.Error);
```

Eine neue Instanz der dem `UINotificationFeedbackGenerator` -Klasse erstellt wird und die zugehörige `Prepare` aufgerufen, um das System zu informieren, dass das Übermitteln von haptischem Feedback wird durchgeführt, damit sie die Latenz minimieren kann.

Die `NotificationOccurred` wird aufgerufen, um das Übermitteln von haptischem Feedback mit Auslösen einer bestimmten `UINotificationFeedbackType` von:

- `Success`
- `Warning`
- `Error`

<a name="UISelectionFeedbackGenerator" />

### <a name="uiselectionfeedbackgenerator"></a>UISelectionFeedbackGenerator

Verwenden Sie dieses Feedback-Generator für eine Auswahl aktiv ändern, z. B. ein Element aus einer Liste auswählen.

Verwenden Sie den folgenden Code zur Auswahlfeedback Trigger:

```csharp
using UIKit;
...

// Initialize feedback
var selection = new UISelectionFeedbackGenerator ();
selection.Prepare ();

// Trigger feedback
selection.SelectionChanged ();
```

Eine neue Instanz der dem `UISelectionFeedbackGenerator` -Klasse erstellt wird und die zugehörige `Prepare` aufgerufen, um das System zu informieren, dass das Übermitteln von haptischem Feedback wird durchgeführt, damit sie die Latenz minimieren kann.

Die `SelectionChanged` Methode löst dann Übermitteln von haptischem Feedback.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die neuen Typen für das Übermitteln von haptischem Feedback verfügbar in iOS 10 und deren in Xamarin.iOS Implementierung behandelt.

## <a name="related-links"></a>Verwandte Links

- [iOS 10-Beispiele](https://developer.xamarin.com/samples/ios/iOS10/)
