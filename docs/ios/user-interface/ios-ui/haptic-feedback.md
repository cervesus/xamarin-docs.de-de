---
title: "Haptic Übermitteln von Feedback"
description: "Dieser Artikel behandelt die neuen Typen für haptic Feedback in iOS 10 und deren Implementierung in Xamarin.iOS verfügbar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 888106D1-58F4-453F-BACC-91D51FA39C80
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 9f4eb989fe0c91471c9473c512c4befd36e4ace2
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="providing-haptic-feedback"></a>Haptic Übermitteln von Feedback

_Dieser Artikel behandelt die neuen Typen für haptic Feedback in iOS 10 und deren Implementierung in Xamarin.iOS verfügbar._

<a name="Overview" />

## <a name="overview"></a>Übersicht

Auf dem iPhone 7 und iPhone umfasst 7 Plus Apple neue haptic Antworten, die zusätzliche Möglichkeiten für den Benutzer physisch initiiert bereitstellen. Haptic Feedback (häufig als Haptics bezeichnet) verwendet den Sinn der Fingereingabe (über erzwingen, Vibrationen oder während des Verschiebens) in Benutzeroberfläche entwerfen. Verwenden Sie diese neue praktisch Feedbackoptionen Aufmerksamkeit des Benutzers abrufen und ihre Aktionen zu vertiefen.

Die folgenden Themen werden im Detail behandelt:

- [Zu Haptic Feedback](#About-Haptic-Feedback)
- [UIImpactFeedbackGenerator](#UIImpactFeedbackGenerator)
- [UINotificationFeedbackGenerator](#UINotificationFeedbackGenerator)
- [UISelectionFeedbackGenerator](#UISelectionFeedbackGenerator)

<a name="About-Haptic-Feedback" />

## <a name="about-haptic-feedback"></a>Zu Haptic Feedback

Mehrere integrierte Benutzeroberflächenelemente Ihr Feedback bereits haptic z. B. Bildlaufbereich, Switches und Schieberegler. iOS 10 fügt nun die Möglichkeit, mithilfe einer konkreten Unterklasse von Haptics programmgesteuert Auslösen der `UIFeedbackGenerator` Klasse.

Der Entwickler mithilfe eines der folgenden `UIFeedbackGenerator` Unterklassen programmgesteuert Trigger haptic Feedback:

- `UIImpactFeedbackGenerator` – Verwenden Sie dieses Generators Feedback um zu ergänzen, eine Aktion oder ein Task, z. B. eine "Thud" darstellen, wenn eine Sicht mit einem Folien oder wenn zwei auf dem Bildschirm Objekte in Konflikt stehen.
- `UINotificationFeedbackGenerator` – Verwenden Sie dieses Generators Feedback für Benachrichtigungen, z. B. eine Aktion abschließen, fehlerhaften oder jede andere Art der Warnung.
- `UISelectionFeedbackGenerator` – Verwenden Sie dieses Generators Feedback zur Auswahl aktiv ändern, z. B. ein Element aus einer Liste auswählen.

<a name="UIImpactFeedbackGenerator" />

### <a name="uiimpactfeedbackgenerator"></a>UIImpactFeedbackGenerator

Verwenden Sie dieses Generators Feedback um zu ergänzen, eine Aktion oder ein Task, z. B. eine "Thud" darstellen, wenn eine Sicht mit einem Folien oder wenn zwei auf dem Bildschirm Objekte in Konflikt stehen.

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

Wenn der Entwickler erstellt eine neue Instanz der der `UIImpactFeedbackGenerator` Klasse, die sie bieten eine `UIImpactFeedbackStyle` die Stärke Feedback, das als angeben:

- `Heavy`
- `Medium`
- `Light`

Die `Prepare` Methode der `UIImpactFeedbackGenerator` wird aufgerufen, um dem System zu informieren, dass haptic Feedback wird durchgeführt, sodass-Latenzzeit minimiert werden können.

Die `ImpactOccurred` Methode löst dann haptic Feedback.

<a name="UINotificationFeedbackGenerator" />

### <a name="uinotificationfeedbackgenerator"></a>UINotificationFeedbackGenerator

Verwenden Sie dieses Generators Feedback für Benachrichtigungen, z. B. eine Aktion abschließen, fehlerhaften oder jede andere Art der Warnung an.

Verwenden Sie den folgenden Code zum Trigger Benachrichtigung Feedback:

```csharp
using UIKit;
...

// Initialize feedback
var notification = new UINotificationFeedbackGenerator ();
notification.Prepare ();

// Trigger feedback
notification.NotificationOccurred (UINotificationFeedbackType.Error);
```

Eine neue Instanz der dem `UINotificationFeedbackGenerator` Klasse erstellt wird und die zugehörige `Prepare` Methode wird aufgerufen, um dem System zu informieren, dass haptic Feedback wird durchgeführt, sodass-Latenzzeit minimiert werden können.

Die `NotificationOccurred` wird aufgerufen, um das Auslösen von haptic Feedback mit einer angegebenen `UINotificationFeedbackType` von:

- `Success`
- `Warning`
- `Error`

<a name="UISelectionFeedbackGenerator" />

### <a name="uiselectionfeedbackgenerator"></a>UISelectionFeedbackGenerator

Verwenden Sie diese Feedback-Generator zur Auswahl aktiv ändern, z. B. ein Element aus einer Liste auswählen.

Verwenden Sie den folgenden Code zum Trigger Auswahl Feedback:

```csharp
using UIKit;
...

// Initialize feedback
var selection = new UISelectionFeedbackGenerator ();
selection.Prepare ();

// Trigger feedback
selection.SelectionChanged ();
```

Eine neue Instanz der dem `UISelectionFeedbackGenerator` Klasse erstellt wird und die zugehörige `Prepare` Methode wird aufgerufen, um dem System zu informieren, dass haptic Feedback wird durchgeführt, sodass-Latenzzeit minimiert werden können.

Die `SelectionChanged` Methode löst dann haptic Feedback.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die neuen Typen für haptic Feedback verfügbar im iOS 10 und deren in Xamarin.iOS Implementierung behandelt.

## <a name="related-links"></a>Verwandte Links

- [iOS-10-Beispiele](https://developer.xamarin.com/samples/ios/iOS10/)
