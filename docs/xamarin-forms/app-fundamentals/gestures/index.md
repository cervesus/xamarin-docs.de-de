---
title: Xamarin.Forms-Gesten
description: In diesem Leitfaden wird erläutert, wie die Gestenerkennungsfunktionen in Xamarin.Forms verwendet werden können, um Benutzerinteraktionen mit Ansichten in einer Xamarin.Forms-Anwendung zu erkennen.
ms.prod: xamarin
ms.assetid: 0E197A51-2304-4C09-A710-C7FF24A89F15
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/25/2018
ms.openlocfilehash: 33968fb935e8b69736ac338bfa0479e4f278e64a
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "61184570"
---
# <a name="xamarinforms-gestures"></a>Xamarin.Forms-Gesten

_Gestenerkennungsfunktionen können in Xamarin.Forms verwendet werden, um Benutzerinteraktionen mit Ansichten in einer Xamarin.Forms-Anwendung zu erkennen._

Die Xamarin.Forms-Klasse [`GestureRecognizer`](xref:Xamarin.Forms.GestureRecognizer) unterstützt Tipp-, Zusammendrück-, Schwenk- und Wischbewegungen auf [`View`](xref:Xamarin.Forms.View)-Instanzen.

## <a name="adding-a-tap-gesture-recognizer"></a>[Hinzufügen einer Gestenerkennung für Tippbewegungen](tap.md)

Eine Tippbewegung wird für die Tipperkennung verwendet und mit der [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)-Klasse erkannt.

## <a name="adding-a-pinch-gesture-recognizer"></a>[Hinzufügen der Gestenerkennung für Zusammendrückbewegungen](pinch.md)

Eine Zusammendrückbewegung wird zum Ausführen des interaktiven Zooms verwendet und mit der Klasse [`PinchGestureRecognizer`](xref:Xamarin.Forms.PinchGestureRecognizer) erkannt.

## <a name="adding-a-pan-gesture-recognizer"></a>[Hinzufügen einer Gestenerkennung für Schwenkbewegungen](pan.md)

Eine Schwenkbewegung wird verwendet, um die Bewegung von Fingern um den Bildschirm zu erkennen und um diese Bewegung auf den Inhalt zu übertragen. Diese wird mit der [`PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer)-Klasse erkannt.

## <a name="adding-a-swipe-gesture-recognizer"></a>[Hinzufügen einer Gestenerkennung für Wischbewegungen](swipe.md)

Eine Wischbewegung tritt auf, wenn ein Finger in horizontaler oder vertikaler Richtung über den Bildschirm bewegt wird. Diese Bewegung wird oft genutzt, um durch Inhalte zu navigieren. Wischgesten werden mit der [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer)-Klasse erkannt.
