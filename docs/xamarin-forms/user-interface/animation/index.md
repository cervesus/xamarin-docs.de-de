---
title: Animation in Xamarin.Forms
description: Xamarin.Forms enthält einen eigenen Animation-Infrastruktur, die zum Erstellen von einfacher Animationen, während Ihnen zugleich vielseitige zum Erstellen komplexer Animationen einfach ist.
ms.prod: xamarin
ms.assetid: AC0B4127-ECA3-44DA-8A24-A2B10A275083
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: 5bc04f638168a10266c20e278481fc0c513afe48
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245025"
---
# <a name="animation-in-xamarinforms"></a>Animation in Xamarin.Forms

_Xamarin.Forms enthält einen eigenen Animation-Infrastruktur, die zum Erstellen von einfacher Animationen, während Ihnen zugleich vielseitige zum Erstellen komplexer Animationen einfach ist._

Die Xamarin.Forms-Animation-Klassen Ziel unterschiedliche Eigenschaften visueller Elemente, die mit einer typischen Animation ändern progressiv einer Eigenschaft von einem Wert in eine andere über einen Zeitraum. Beachten Sie, dass keine Verwendung von XAML-Schnittstelle für die Xamarin.Forms-Animation-Klassen. Allerdings können Animationen in gekapselt [Verhalten](~/xamarin-forms/app-fundamentals/behaviors/index.md) , und klicken Sie dann auf die verwiesen wird aus XAML.

## <a name="simple-animationssimplemd"></a>[Einfache Animationen](simple.md)

Die [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) Klasse enthält Erweiterungsmethoden, die verwendet werden können, einfache Animationen erstellen, die drehen, zu skalieren, zu übersetzen und abgeblendet [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) Instanzen. Dieser Artikel veranschaulicht das Erstellen und Abbrechen von Animationen mithilfe der `ViewExtensions` Klasse.

## <a name="easing-functionseasingmd"></a>[Beschleunigungsfunktionen](easing.md)

Xamarin.Forms enthält ein [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) -Klasse, die Ihnen die Möglichkeit geben Sie eine Übertragungsfunktion, die steuert, wie Animationen beschleunigen oder verlangsamen Sie, wie sie ausgeführt werden. In diesem Artikel wird veranschaulicht, wie die vordefinierten Beschleunigungsfunktionen verwendet und wie benutzerdefinierte Beschleunigungsfunktionen erstellen.

## <a name="custom-animationscustommd"></a>[Benutzerdefinierte Animationen](custom.md)

Die [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) Klasse ist der Baustein von allen Xamarin.Forms Animationen mit den Erweiterungsmethoden in den [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) Klasse erstellen eine oder mehrere `Animation` Objekte. Dieser Artikel veranschaulicht, wie die `Animation` Klasse zum Erstellen und "Abbrechen" Animationen, mehrere Animationen zu synchronisieren, und Erstellen von benutzerdefinierten Animationen, die Eigenschaften zu animieren, die durch die vorhandenen Methoden für die Animation animiert werden nicht.
