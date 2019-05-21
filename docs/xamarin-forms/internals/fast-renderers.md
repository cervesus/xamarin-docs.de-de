---
title: Xamarin.Forms schnelle Renderer
description: In diesem Artikel werden schnelle Renderer vorgestellt, welche die Inflations- und Renderingkosten von Xamarin.Forms-Steuerelementen für Android reduzieren, indem Sie die resultierende native Steuerelementhierarchie vereinfachen.
ms.prod: xamarin
ms.assetid: 097f87f2-d891-4f3c-be02-fb7d195a481a
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/09/2019
ms.openlocfilehash: 861d9e3f898dcd61015d9aca27ae66c3fe72d1a9
ms.sourcegitcommit: 482aef652bdaa440561252b6a1a1c0a40583cd32
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2019
ms.locfileid: "65970720"
---
# <a name="xamarinforms-fast-renderers"></a>Xamarin.Forms schnelle Renderer

In der Vergangenheit bestehen die meisten der ursprünglichen steuerelementrenderer für Android von zwei Ansichten:

- Ein natives Steuerelement, z. B. eine `Button` oder `TextView`.
- Ein Container `ViewGroup` , die Teil der Arbeit Layout, Verarbeitung und andere Aufgaben verarbeitet.

Dieser Ansatz hat jedoch eine Leistung Implikation, zwei Ansichten für jedes logische Steuerelement erstellt werden. Dies führt eine komplexere visuelle Struktur, die erfordert mehr Arbeitsspeicher und vieles mehr Verarbeitung auf dem Bildschirm gerendert werden soll.

Schnelle Renderer reduzieren die Inflations- und renderingkosten von Xamarin.Forms-Steuerelements in einer einzigen Ansicht an. Aus diesem Grund wird anstelle von zwei Ansichten erstellen, und der Struktur hinzugefügt, die nur eine erstellt. Dies verbessert die Leistung, indem Sie weniger Objekte, was wiederum bedeutet eine Struktur für weniger komplexe, erstellen und weniger arbeitsspeichernutzung (wodurch auch weniger Pausen der Garbage Collections).

Schnelle Renderer sind für die folgenden Steuerelemente in Xamarin.Forms unter Android verfügbar:

- [`Button`](xref:Xamarin.Forms.Button)
- [`Image`](xref:Xamarin.Forms.Image)
- [`Label`](xref:Xamarin.Forms.Label)
- [`Frame`](xref:Xamarin.Forms.Frame)

Diese schnelle Renderer sind funktionell nichts davon unterscheidet, der legacy-Renderer. Von oder höher, Xamarin.Forms 4.0 Anwendungen mit dem Ziel `FormsAppCompatActivity` diese schnelle Renderer standardmäßig verwendet wird. Renderer für alle neuen Steuerelemente, einschließlich [ `ImageButton` ](xref:Xamarin.Forms.ImageButton) und [ `CollectionView` ](xref:Xamarin.Forms.CollectionView), verwenden Sie den Ansatz für die schnelle Renderer.

Leistungsverbesserungen beim Verwenden von schneller Renderers variiert für jede Anwendung, die je nach Komplexität des Layouts. Leistungsverbesserungen X2 sind z. B. möglich, beim Durchführen eines Bildlaufs durch einen [ `ListView` ](xref:Xamarin.Forms.ListView) mit Tausenden von Zeilen mit Daten, die, in denen die Zellen in jeder Zeile sind Steuerelemente, die schnellere Renderern verwendet werden, was dazu führt sichtbar reibungsloseren scrollen.

> [!NOTE]
> Benutzerdefinierte Renderer können für die schnelle Renderer, die mit den gleichen Ansatz wie für die legacy-Renderer erstellt werden. Weitere Informationen finden Sie unter [Custom Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) (Benutzerdefinierte Renderer).

## <a name="backwards-compatibility"></a>Abwärtskompatibilität

Schnelle Renderer können mithilfe der folgenden Ansätze überschrieben werden:

1. Aktivieren die legacy-Renderer durch Hinzufügen der folgenden Zeile des Codes zu Ihrem `MainActivity` Klasse vor dem Aufruf `Forms.Init`:

    ```csharp
    Forms.SetFlags("UseLegacyRenderers");
    ```

1. Mithilfe von benutzerdefinierten Renderern, die auf dem legacy-Renderer abzielen. Alle vorhandenen benutzerdefinierten Renderern werden weiterhin mit dem legacy-Renderer.
1. Angeben einer anderen `View.Visual`, z. B. `Material`, verschiedene Renderer verwendet. Weitere Informationen zu Visual Material, finden Sie unter [Xamarin.Forms Material Visual](~/xamarin-forms/user-interface/visual/material-visual.md).

## <a name="related-links"></a>Verwandte Links

- [Benutzerdefinierte Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
