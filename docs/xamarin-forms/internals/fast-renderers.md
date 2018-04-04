---
title: Schnelle Renderer
description: Dieser Artikel führt schnelle Renderern, die der wiederverwendungen und Rendering Kosten eines Steuerelements Xamarin.Forms auf Android-Geräten zu reduzieren, indem Sie die resultierende systemeigene Steuerelement-Hierarchie vereinfachen.
ms.prod: xamarin
ms.assetid: 097f87f2-d891-4f3c-be02-fb7d195a481a
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: 16807d890f12810ccc1d50cb8e506e104ec8e6a3
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="fast-renderers"></a>Schnelle Renderer

_Dieser Artikel führt schnelle Renderern, die der wiederverwendungen und Rendering Kosten eines Steuerelements Xamarin.Forms auf Android-Geräten zu reduzieren, indem Sie die resultierende systemeigene Steuerelement-Hierarchie vereinfachen._

Traditionell, bestehen die meisten der ursprünglichen Steuerelement-Renderer unter Android zwei Ansichten:

- Ein systemeigenes zu steuern, wie z. B. eine `Button` oder `TextView`.
- Ein Container `ViewGroup` , die Teil der Arbeit Layout, Geste Behandlung und andere Aufgaben verarbeitet.

Dieser Ansatz hat jedoch eine Leistung Implikation insofern, dass zwei Ansichten für jedes logische Steuerelement erstellt werden Vortäuschen einer komplexeren visuellen Struktur, die erfordert mehr Arbeitsspeicher und vieles mehr Verarbeitung aus, um auf dem Bildschirm gerendert.

Schnelle Renderer Reduzieren der wiederverwendungen und Rendering Kosten eines Xamarin.Forms-Steuerelements in einer einzigen Ansicht an. Aus diesem Grund wird statt zwei Ansichten erstellen, und die Struktur der Sicht hinzugefügt, die nur eine erstellt. Dies verbessert die Leistung, indem Sie weniger Objekte, womit wiederum eine weniger komplexe Ansicht-Struktur, erstellen und weniger Arbeitsspeicher verwenden (was auch weniger Pausen der Garbage Collections zur Folge).

Schnelle Renderer stehen für die folgenden Steuerelemente in Xamarin.Forms 2.4 auf Android-Geräten zur Verfügung:

- [`Button`](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/)
- [`Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)
- [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)

Funktionell unterscheiden sich diese schnelle Renderer nicht auf dem ursprünglichen Renderer. Allerdings sie sind derzeit experimentellen und kann nur verwendet werden, indem Sie die folgende Codezeile, Hinzufügen der `MainActivity` Klasse vor dem Aufruf `Forms.Init`:

```csharp
Forms.SetFlags("FastRenderers_Experimental");
```

> [!NOTE]
> Schnelle Renderer gelten nur für app Compat Android Back-End, daher wird diese Einstellung auf Pre-app/Compat Aktivitäten ignoriert.

Verbesserte Leistung beim variiert für jede Anwendung, die je nach Komplexität des Layouts. Leistungsverbesserungen der X2 sind z. B. möglich, beim Durchführen eines Bildlaufs durch einen [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) enthält Tausende von Zeilen mit Daten, die, in denen die Zellen in jeder Zeile werden Steuerelemente, die schnelle Renderern verwendet werden, was dazu führt das glatter Durchführen eines Bildlaufs.


## <a name="related-links"></a>Verwandte Links

- [Benutzerdefinierte Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
