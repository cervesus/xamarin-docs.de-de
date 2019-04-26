---
title: Animation in Xamarin.Forms
description: Xamarin.Forms umfasst eine eigenen Animation-Infrastruktur, die für das einfache Animationen und gleichzeitig ein vielseitig genug ist, zum Erstellen komplexer Animationen erstellen einfach ist.
ms.prod: xamarin
ms.assetid: AC0B4127-ECA3-44DA-8A24-A2B10A275083
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: bebb3e32f298a2079069787dba3453e1817cf64f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61158592"
---
# <a name="animation-in-xamarinforms"></a>Animation in Xamarin.Forms

_Xamarin.Forms umfasst eine eigenen Animation-Infrastruktur, die für das einfache Animationen und gleichzeitig ein vielseitig genug ist, zum Erstellen komplexer Animationen erstellen einfach ist._

Die Xamarin.Forms-Animationsklassen Ziel unterschiedliche Eigenschaften visueller Elemente mit einer typischen Animation ändern progressiv einer Eigenschaft von einem Wert in eine andere über einen Zeitraum an. Beachten Sie, dass keine XAML-Schnittstelle für die Xamarin.Forms-Animation-Klassen. Allerdings können Animationen in gekapselt [Verhaltensweisen](~/xamarin-forms/app-fundamentals/behaviors/index.md) , und klicken Sie dann auf die verwiesen wird aus XAML.

## <a name="simple-animationssimplemd"></a>[Einfache Animationen](simple.md)

Die [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) Klasse enthält Erweiterungsmethoden, die verwendet werden können, um einfache Animationen erstellen, die drehen, skalieren, übersetzen und fade [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) Instanzen. In diesem Artikel veranschaulicht das Erstellen und Abbrechen von Animationen mit der `ViewExtensions` Klasse.

## <a name="easing-functionseasingmd"></a>[Beschleunigungsfunktionen](easing.md)

Xamarin.Forms umfasst einen [ `Easing` ](xref:Xamarin.Forms.Easing) -Klasse, die Ihnen die Möglichkeit zum Angeben einer Übertragungsfunktion, die steuert, wie Animationen beschleunigen oder verlangsamen, wie sie ausgeführt werden. In diesem Artikel wird veranschaulicht, wie die vordefinierten Beschleunigungsfunktionen genutzt, und wie Sie benutzerdefinierte Beschleunigungsfunktionen erstellen.

## <a name="custom-animationscustommd"></a>[Benutzerdefinierte Animationen](custom.md)

Die [ `Animation` ](xref:Xamarin.Forms.Animation) Klasse ist der Baustein von alle Xamarin.Forms-Animationen, mit den Erweiterungsmethoden in den [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) Klasse erstellen eine oder mehrere `Animation` Objekte. In diesem Artikel wird veranschaulicht, wie Sie mit der `Animation` Klasse erstellen und Abbrechen von Animationen, mehrere Animationen zu synchronisieren, und erstellen benutzerdefinierte Animationen, das Animieren von Eigenschaften, die durch die vorhandenen Methoden für die Animation animiert werden nicht.
