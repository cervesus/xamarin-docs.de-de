---
title: Xamarin.FormsSchnelle Renderer
description: In diesem Artikel werden schnelle Renderer vorgestellt, die die Inflations-und renderingkosten eines Xamarin.Forms Steuer Elements auf Android verringern, indem die resultierende Native Steuerelement Hierarchie vereinfacht wird.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 3f25f4c2da5b2a426673b49045b5d2d05b0c6ac4
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139073"
---
# <a name="xamarinforms-fast-renderers"></a>Xamarin.FormsSchnelle Renderer

Üblicherweise bestehen die meisten der ursprünglichen steuerungrenderer unter Android aus zwei Ansichten:

- Ein natives Steuerelement, z `Button` . b `TextView` . oder.
- Ein Container, der `ViewGroup` einige der Layoutarbeit, Gesten Behandlung und andere Aufgaben behandelt.

Diese Vorgehensweise wirkt sich jedoch darauf aus, dass für jedes logische Steuerelement zwei Sichten erstellt werden. Dies führt zu einer komplexeren visuellen Struktur, die mehr Arbeitsspeicher erfordert, und mehr Verarbeitungsmöglichkeiten, die auf dem Bildschirm dargestellt werden.

Schnelle Renderer verringern die Inflations-und renderingkosten eines Xamarin.Forms Steuer Elements in einer einzigen Ansicht. Anstatt zwei Ansichten zu erstellen und Sie der Ansichts Struktur hinzuzufügen, wird daher nur eine erstellt. Dies verbessert die Leistung, indem weniger Objekte erstellt werden, was wiederum eine weniger komplexe Ansichts Struktur bedeutet, und weniger Arbeitsspeicher Nutzung (was zu weniger Garbage Collection Pausen führt).

Schnelle Renderer sind für die folgenden Steuerelemente in Xamarin.Forms unter Android verfügbar:

- [`Button`](xref:Xamarin.Forms.Button)
- [`Image`](xref:Xamarin.Forms.Image)
- [`Label`](xref:Xamarin.Forms.Label)
- [`Frame`](xref:Xamarin.Forms.Frame)

Diese schnellen Renderer unterscheiden sich nicht von den Legacy-Renderer. Ab Xamarin.Forms 4,0 `FormsAppCompatActivity` werden diese schnellen Renderer von allen Zielanwendungen standardmäßig verwendet. Renderer für alle neuen Steuerelemente, einschließlich [`ImageButton`](xref:Xamarin.Forms.ImageButton) und [`CollectionView`](xref:Xamarin.Forms.CollectionView) , verwenden den schnellen rendereransatz.

Leistungsverbesserungen bei der Verwendung von schnellen Renderern variieren je nach Komplexität des Layouts für jede Anwendung. So können z. b. Leistungsverbesserungen von x2 durch eine durchlaufen werden [`ListView`](xref:Xamarin.Forms.ListView) , die Tausende von Daten Zeilen enthält, wobei die Zellen in den einzelnen Zeilen aus Steuerelementen bestehen, die schnelle Renderer verwenden, was zu einem offensichtlich reibungsloseren Scrollen führt.

> [!NOTE]
> Benutzerdefinierte Renderer können für schnelle Renderer mit dem gleichen Ansatz wie für ältere Renderer erstellt werden. Weitere Informationen finden Sie unter [Custom Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) (Benutzerdefinierte Renderer).

## <a name="backwards-compatibility"></a>Abwärtskompatibilität

Schnelle Renderer können mit den folgenden Ansätzen überschrieben werden:

1. Aktivieren Sie die Legacy-Renderer, indem Sie der-Klasse die folgende Codezeile hinzufügen, `MainActivity` bevor Sie aufrufen `Forms.Init` :

    ```csharp
    Forms.SetFlags("UseLegacyRenderers");
    ```

1. Verwenden von benutzerdefinierten renderatoren, die auf die Legacy-Renderer abzielen. Alle vorhandenen benutzerdefinierten Renderer funktionieren weiterhin mit den Legacy-Renderer.
1. Angeben eines anderen `View.Visual` , z `Material` . b., der unterschiedliche Renderer verwendet. Weitere Informationen zum visuellen Material finden Sie unter [ Xamarin.Forms Material Visual](~/xamarin-forms/user-interface/visual/material-visual.md).

## <a name="related-links"></a>Verwandte Links

- [Benutzerdefinierte Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
