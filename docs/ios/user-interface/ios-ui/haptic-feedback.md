---
title: Bereitstellen von haptischem Feedback in xamarin. IOS
description: In diesem Dokument wird beschrieben, wie Sie ein haptisches Feedback in einer xamarin. IOS-App bereitstellen. Es werden uiimpactfeedbackgenerator, uinotificationfeedbackgenerator und uiselectionfeedbackgenerator erläutert.
ms.prod: xamarin
ms.assetid: 888106D1-58F4-453F-BACC-91D51FA39C80
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: 86c7c726465257d71b6b407686153650dddbcbca
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84574469"
---
# <a name="providing-haptic-feedback-in-xamarinios"></a>Bereitstellen von haptischem Feedback in xamarin. IOS

<a name="Overview"></a>

## <a name="overview"></a>Übersicht

Auf iPhone 7 und iPhone 7 plus enthält Apple neue, willkürliche Antworten, die zusätzliche Möglichkeiten zur physischen Einbindung des Benutzers bereitstellen. Bei einem willkürlichen Feedback (häufig auch als Haptik bezeichnet) wird das Gefühl der Toucheingabe (über "Force", "Vibrations" oder "Motion") beim Entwurf der Benutzeroberfläche verwendet. Verwenden Sie diese neuen Optionen für das taktile Feedback, um die Aufmerksamkeit des Benutzers zu erhalten und seine Aktionen zu verstärken.

Die folgenden Themen werden im Detail behandelt:

- [Informationen zu haptischem Feedback](#About-Haptic-Feedback)
- [Uiimpactfeedbackgenerator](#UIImpactFeedbackGenerator)
- [Uinotificationfeedbackgenerator](#UINotificationFeedbackGenerator)
- [Uiselectionfeedbackgenerator](#UISelectionFeedbackGenerator)

<a name="About-Haptic-Feedback"></a>

## <a name="about-haptic-feedback"></a>Informationen zu haptischem Feedback

Mehrere integrierte Benutzeroberflächen Elemente bieten bereits haptisches Feedback wie z. b. Picker, Switches und Schieberegler. IOS 10 bietet nun die Möglichkeit, die Haptik mithilfe einer konkreten Unterklasse der-Klasse Programm gesteuert zu initiieren `UIFeedbackGenerator` .

Der Entwickler kann eine der folgenden `UIFeedbackGenerator` Unterklassen verwenden, um haptisches Feedback Programm gesteuert zu initiieren:

- `UIImpactFeedbackGenerator`-Verwenden Sie diesen Feedback Generator, um eine Aktion oder Aufgabe zu ergänzen, z. b. eine "thud" darzustellen, wenn eine Ansicht in den Ort bewegt wird oder zwei aufeinander folgende Objekte miteinander kollidieren.
- `UINotificationFeedbackGenerator`-Verwenden Sie diesen Feedback Generator für Benachrichtigungen wie z. b. eine Aktion, die einen Fehler verursacht, oder einen anderen Warnungstyp.
- `UISelectionFeedbackGenerator`-Verwenden Sie diesen Feedback Generator für eine aktive Änderung, z. b. das Auswählen eines Elements aus einer Liste.

<a name="UIImpactFeedbackGenerator"></a>

### <a name="uiimpactfeedbackgenerator"></a>Uiimpactfeedbackgenerator

Verwenden Sie diesen Feedback Generator, um eine Aktion oder Aufgabe zu ergänzen, z. b. eine "thud" darzustellen, wenn eine Ansicht in den Ort bewegt wird oder zwei aufeinander folgende Objekte miteinander kollidieren.

Verwenden Sie den folgenden Code, um das Impact-Feedback zu initiieren:

```csharp
using UIKit;
...

// Initialize feedback
var impact = new UIImpactFeedbackGenerator (UIImpactFeedbackStyle.Heavy);
impact.Prepare ();

// Trigger feedback
impact.ImpactOccurred ();
```

Wenn der Entwickler eine neue Instanz der-Klasse erstellt, `UIImpactFeedbackGenerator` Geben Sie einen `UIImpactFeedbackStyle` an, der die Stärke des Feedbacks wie folgt angibt:

- `Heavy`
- `Medium`
- `Light`

Die- `Prepare` Methode des `UIImpactFeedbackGenerator` wird aufgerufen, um das System zu informieren, dass ein willkürlichem Feedback auftritt, um die Latenz zu minimieren.

Die- `ImpactOccurred` Methode löst dann haptisches Feedback aus.

<a name="UINotificationFeedbackGenerator"></a>

### <a name="uinotificationfeedbackgenerator"></a>Uinotificationfeedbackgenerator

Verwenden Sie diesen Feedback Generator für Benachrichtigungen wie z. b. eine Aktion, die einen Fehler verursacht, oder einen anderen Warnungstyp.

Verwenden Sie den folgenden Code, um das Benachrichtigungs Feedback zu initiieren:

```csharp
using UIKit;
...

// Initialize feedback
var notification = new UINotificationFeedbackGenerator ();
notification.Prepare ();

// Trigger feedback
notification.NotificationOccurred (UINotificationFeedbackType.Error);
```

Es wird eine neue Instanz der `UINotificationFeedbackGenerator` -Klasse erstellt, und die zugehörige- `Prepare` Methode wird aufgerufen, um das System zu informieren, dass ein willkürlichem Feedback auftritt, damit die Latenz minimiert werden kann.

`NotificationOccurred`Wird aufgerufen, um haptisches Feedback mit einem bestimmten von zu initiieren `UINotificationFeedbackType` :

- `Success`
- `Warning`
- `Error`

<a name="UISelectionFeedbackGenerator"></a>

### <a name="uiselectionfeedbackgenerator"></a>Uiselectionfeedbackgenerator

Verwenden Sie diesen Feedback Generator für eine aktive Änderung, z. b. das Auswählen eines Elements aus einer Liste.

Verwenden Sie den folgenden Code, um das Auswahl Feedback zu initiieren:

```csharp
using UIKit;
...

// Initialize feedback
var selection = new UISelectionFeedbackGenerator ();
selection.Prepare ();

// Trigger feedback
selection.SelectionChanged ();
```

Es wird eine neue Instanz der `UISelectionFeedbackGenerator` -Klasse erstellt, und die zugehörige- `Prepare` Methode wird aufgerufen, um das System zu informieren, dass ein willkürlichem Feedback auftritt, damit die Latenz minimiert werden kann.

Die- `SelectionChanged` Methode löst dann haptisches Feedback aus.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die neuen Typen von willkürlich verfügbarem Feedback in ios 10 erläutert und erläutert, wie Sie in xamarin. IOS implementiert werden.

## <a name="related-links"></a>Verwandte Links

- [IOS 10-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
