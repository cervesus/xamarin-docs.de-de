---
title: Xamarin.Forms-Gesten
description: Dieser Leitfaden erläutert, wie Xamarin.Forms Geste Erkennungen verwendet werden können, um Benutzerinteraktionen mit Ansichten in einer Xamarin.Forms-Anwendung zu erkennen.
ms.prod: xamarin
ms.assetid: 0E197A51-2304-4C09-A710-C7FF24A89F15
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/25/2018
ms.openlocfilehash: 33968fb935e8b69736ac338bfa0479e4f278e64a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50106181"
---
# <a name="xamarinforms-gestures"></a>Xamarin.Forms-Gesten

_Geste Erkennungen können verwendet werden, um Benutzerinteraktionen mit Ansichten in einer Xamarin.Forms-Anwendung zu erkennen._

Die Xamarin.Forms [ `GestureRecognizer` ](xref:Xamarin.Forms.GestureRecognizer) Klasse unterstützt tippen, verkleinern, schwenken und wischbewegungen auf [ `View` ](xref:Xamarin.Forms.View) Instanzen.

## <a name="adding-a-tap-gesture-recognizertapmd"></a>[Hinzufügen einer Tap-stiftbewegungs-Erkennung](tap.md)

Eine tippbewegung, die für die Tap-Erkennung verwendet wird, und wird erkannt, mit der [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) Klasse.

## <a name="adding-a-pinch-gesture-recognizerpinchmd"></a>[Hinzufügen einer Pinch-stiftbewegungs-Erkennung](pinch.md)

Eine zusammendrückbewegung wird zum Ausführen von interaktiven Zoom verwendet und wird erkannt, mit der [ `PinchGestureRecognizer` ](xref:Xamarin.Forms.PinchGestureRecognizer) Klasse.

## <a name="adding-a-pan-gesture-recognizerpanmd"></a>[Hinzufügen einer Pan stiftbewegungs-Erkennung](pan.md)

Eine Geste für ein Schwenken wird zum Erkennen der Bewegung der Finger auf dem Bildschirm, und diese Verschiebung auf Inhalt angewendet, und wird erkannt, mit der [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer) Klasse.

## <a name="adding-a-swipe-gesture-recognizerswipemd"></a>[Hinzufügen einer streifbewegung stiftbewegungs-Erkennung](swipe.md)

Eine streifbewegung tritt auf, wenn ein Finger auf dem Bildschirm in eine Richtung horizontal oder vertikal verschoben wird, und häufig verwendet, um die Navigation durch Inhalte zu initiieren. Wischbewegungen werden erkannt, mit der [ `SwipeGestureRecognizer` ](xref:Xamarin.Forms.SwipeGestureRecognizer) Klasse.
