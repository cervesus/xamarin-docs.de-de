---
title: Animation inXamarin.Forms
description: Xamarin.Formsenthält eine eigene Animations Infrastruktur, die für die Erstellung einfacher Animationen einfach ist, während Sie auch vielseitig genug ist, um komplexe Animationen zu erstellen.
ms.prod: xamarin
ms.assetid: AC0B4127-ECA3-44DA-8A24-A2B10A275083
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 88a671c4d28d62a5f73e90a7b2fa9c45b7dbe8b1
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84128998"
---
# <a name="animation-in-xamarinforms"></a>Animation inXamarin.Forms

_Xamarin. Forms enthält eine eigene Animations Infrastruktur, mit der einfache Animationen problemlos erstellt werden können, während Sie auch vielseitig genug ist, um komplexe Animationen zu erstellen._

Die Xamarin.Forms Animations klassenzielen auf verschiedene Eigenschaften von visuellen Elementen ab, wobei eine typische Animation eine Eigenschaft in einem bestimmten Zeitraum progressiv von einem Wert in einen anderen wechselt. Beachten Sie, dass es keine XAML-Schnittstelle für die Xamarin.Forms Animations Klassen gibt. Animationen können jedoch in [Verhalten](~/xamarin-forms/app-fundamentals/behaviors/index.md) gekapselt und dann von XAML referenziert werden.

## <a name="simple-animations"></a>[Einfache Animationen](simple.md)

Die- [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) Klasse stellt Erweiterungs Methoden bereit, die verwendet werden können, um einfache Animationen zu erstellen, mit denen Instanzen gedreht, skaliert, übersetzt und ausgeblendet werden [`VisualElement`](xref:Xamarin.Forms.VisualElement) . In diesem Artikel wird veranschaulicht, wie Animationen mithilfe der-Klasse erstellt und abgebrochen werden `ViewExtensions` .

## <a name="easing-functions"></a>[Beschleunigungsfunktionen](easing.md)

Xamarin.Formsenthält eine- [`Easing`](xref:Xamarin.Forms.Easing) Klasse, die es Ihnen ermöglicht, eine Übertragungsfunktion anzugeben, mit der gesteuert wird, wie Animationen bei der Ausführung beschleunigt oder verlangsamt werden. In diesem Artikel wird veranschaulicht, wie die vordefinierten Beschleunigungsfunktionen verwendet werden und wie benutzerdefinierte Beschleunigungsfunktionen erstellt werden.

## <a name="custom-animations"></a>[Benutzerdefinierte Animationen](custom.md)

Die- [`Animation`](xref:Xamarin.Forms.Animation) Klasse ist der Baustein aller Xamarin.Forms Animationen mit den Erweiterungs Methoden in der-Klasse, die [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) ein oder mehrere- `Animation` Objekte erstellen. In diesem Artikel wird veranschaulicht, wie Sie mit der- `Animation` Klasse Animationen erstellen und Abbrechen, mehrere Animationen synchronisieren und benutzerdefinierte Animationen erstellen, die Eigenschaften animieren, die nicht von den vorhandenen Animations Methoden animiert werden.
