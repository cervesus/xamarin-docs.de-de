---
title: Xamarin.Forms schnelle Renderer
description: In diesem Artikel werden schnelle Renderer vorgestellt, welche die Inflations- und Renderingkosten von Xamarin.Forms-Steuerelementen für Android reduzieren, indem Sie die resultierende native Steuerelementhierarchie vereinfachen.
ms.prod: xamarin
ms.assetid: 097f87f2-d891-4f3c-be02-fb7d195a481a
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: 9a40644df6abcbbcc327b1b0c2dcb26c2dbc4db5
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61293593"
---
# <a name="xamarinforms-fast-renderers"></a>Xamarin.Forms schnelle Renderer

![Vorschau](~/media/shared/preview.png)

_Dieser Artikel enthält die schnelle Renderer (hinzugefügt in Xamarin.Forms 2.4), die die Inflations- und renderingkosten von einer Xamarin.Forms-Steuerelement für Android zu reduzieren, indem Sie die resultierende native Steuerelementhierarchie vereinfachen._

In der Vergangenheit bestehen die meisten der ursprünglichen steuerelementrenderer für Android von zwei Ansichten:

- Ein natives Steuerelement, z. B. eine `Button` oder `TextView`.
- Ein Container `ViewGroup` , die Teil der Arbeit Layout, Verarbeitung und andere Aufgaben verarbeitet.

Dieser Ansatz hat jedoch eine Leistung Implikation, zwei Ansichten für jedes logische Steuerelement erstellt werden. Dies führt eine komplexere visuelle Struktur, die erfordert mehr Arbeitsspeicher und vieles mehr Verarbeitung auf dem Bildschirm gerendert werden soll.

Schnelle Renderer reduzieren die Inflations- und renderingkosten von Xamarin.Forms-Steuerelements in einer einzigen Ansicht an. Aus diesem Grund wird anstelle von zwei Ansichten erstellen, und der Struktur hinzugefügt, die nur eine erstellt. Dies verbessert die Leistung, indem Sie weniger Objekte, was wiederum bedeutet eine Struktur für weniger komplexe, erstellen und weniger arbeitsspeichernutzung (wodurch auch weniger Pausen der Garbage Collections).

Schnelle Renderer sind für die folgenden Steuerelemente in Xamarin.Forms 2.4 unter Android verfügbar:

- [`Button`](xref:Xamarin.Forms.Button)
- [`Image`](xref:Xamarin.Forms.Image)
- [`Label`](xref:Xamarin.Forms.Label)
- [`Frame`](xref:Xamarin.Forms.Frame)

Funktionell unterscheiden sich diese schnelle Renderer nicht auf dem ursprünglichen Renderer. Allerdings sie derzeit experimentell und kann nur verwendet werden, durch die folgende Zeile des Codes zum Hinzufügen Ihrer `MainActivity` Klasse vor dem Aufruf `Forms.Init`:

```csharp
Forms.SetFlags("FastRenderers_Experimental");
```

> [!NOTE]
> Schnelle Renderer gelten nur für die app Compat Android Back-End, damit diese Einstellung auf Pre-app Compat Aktivitäten ignoriert wird.

Leistungsverbesserungen variiert für jede Anwendung, die je nach Komplexität des Layouts. Leistungsverbesserungen X2 sind z. B. möglich, beim Durchführen eines Bildlaufs durch einen [ `ListView` ](xref:Xamarin.Forms.ListView) mit Tausenden von Zeilen mit Daten, die, in denen die Zellen in jeder Zeile sind Steuerelemente, die schnellere Renderern verwendet werden, was dazu führt sichtbar reibungsloseren scrollen.


## <a name="related-links"></a>Verwandte Links

- [Benutzerdefinierte Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
