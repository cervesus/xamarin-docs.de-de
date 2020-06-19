---
title: Xamarin.FormsFormen
description: Xamarin.FormsFormen sind Typen von Sichten, mit denen Sie Formen auf dem Bildschirm zeichnen können.
ms.prod: xamarin
ms.assetid: 4E749FE8-852C-46DA-BB1E-652936106357
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a2ee7e6afc46c9d7b6e2f3d7710bdf758bafe1b9
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84947338"
---
# <a name="xamarinforms-shapes"></a>Xamarin.FormsFormen

![](~/media/shared/preview.png "This API is currently pre-release")

Ein `Shape` ist ein Typ von [`View`](xref:Xamarin.Forms.View) , mit dem Sie eine Form auf dem Bildschirm zeichnen können. `Shape`-Objekte können in layoutklassen und den meisten Steuerelementen verwendet werden, da die- `Shape` Klasse von der-Klasse abgeleitet wird `View` .

Xamarin.FormsFormen sind im `Xamarin.Forms.Shapes` -Namespace unter IOS, Android, macOS, den universelle Windows-Plattform (UWP) und Windows Presentation Foundation (WPF) verfügbar.

> [!IMPORTANT]
> Xamarin.FormsFormen sind zurzeit experimentell und können nur verwendet werden, indem das-Flag festgelegt wird `Shapes_Experimental` . Weitere Informationen finden Sie unter [experimentelle Flags](~/xamarin-forms/internals/experimental-flags.md).

`Shape` definiert die folgenden Eigenschaften:

- `Aspect`Gibt an, `Stretch` wie die Form den zugeordneten Bereich füllt. Der Standardwert dieser Eigenschaft ist `Stretch.None`.
- `Fill`Gibt die Farbe an, mit der `Color` das Innere der Form gezeichnet wird.
- `Stroke``Color`gibt die Farbe an, die zum Zeichnen der Kontur der Form verwendet wird.
- `StrokeDashArray`vom Typ `DoubleCollection` , der eine Auflistung von Werten darstellt, die `double` das Muster von Bindestrichen und Lücken angeben, die zum Gliedern einer Form verwendet werden.
- `StrokeDashOffset``double`gibt den Abstand innerhalb des Strich Musters an, in dem ein Bindestrich beginnt. Der Standardwert dieser Eigenschaft ist 0,0.
- `StrokeLineCap`, vom Typ `PenLineCap` , beschreibt die Form am Anfang und am Ende einer Linie oder eines Segments. Der Standardwert dieser Eigenschaft ist `PenLineCap.Flat`.
- `StrokeLineJoin`Gibt den Typ des Joins an, der in den Scheitel Punkten `PenLineJoin` einer Form verwendet wird. Der Standardwert dieser Eigenschaft ist `PenLineJoin.Miter`.
- `StrokeThickness``double`gibt die Breite der Form Gliederung des Typs an. Der Standardwert dieser Eigenschaft ist 1,0.

Diese Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

Xamarin.Formsdefiniert eine Reihe von-Objekten, die von der-Klasse abgeleitet werden `Shape` . Dazu zählen `Ellipse`, `Line`, `Path`, `Polygon`, `Polyline` und `Rectangle`.

## <a name="related-links"></a>Verwandte Links

- [Shapedemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapedemos/)
