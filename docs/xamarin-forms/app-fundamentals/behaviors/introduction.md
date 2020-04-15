---
title: Einführung in Verhalten
description: Durch Verhalten können Sie Steuerelementen für Benutzeroberflächen Funktionen hinzufügen, ohne Unterklassen erstellen zu müssen. Stattdessen wird die Funktion in einer Verhaltensklasse implementiert und an das Steuerelement angefügt, als wäre sie ein Teil des Steuerelements selbst. In diesem Artikel werden Verhalten grundlegend vorgestellt.
ms.prod: xamarin
ms.assetid: 0DF1EF8C-A212-4142-A3C6-DF760A82A757
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: d62ba6b025b2fe9865df8279a5e98eba254bb5a2
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "70772048"
---
# <a name="introduction-to-behaviors"></a>Einführung in Verhalten

_Durch Verhalten können Sie Steuerelementen für Benutzeroberflächen Funktionen hinzufügen, ohne Unterklassen erstellen zu müssen. Stattdessen wird die Funktion in einer Verhaltensklasse implementiert und an das Steuerelement angefügt, als wäre sie ein Teil des Steuerelements selbst. In diesem Artikel werden Verhalten grundlegend vorgestellt._

Durch Verhalten können Sie Code implementieren, den Sie normalerweise in eine CodeBehind-Datei schreiben würden, da dieser so direkt mit der API des Steuerelements interagiert, dass er präzise an das Steuerelement angefügt werden und zur Wiederverwendung in mehr als eine App gepackt werden kann. Sie können verwendet werden, um vollständige Funktionen der Steuerelemente bereitzustellen, wie:

- Hinzufügen eines Validierungssteuerelement für E-Mails an [`Entry`](xref:Xamarin.Forms.Entry)
- Erstellen eines Bewertungssteuerelements mithilfe einer Gestenerkennung für Tippbewegungen
- Steuern einer Animation
- Hinzufügen eines Effekts zu einem Steuerelement

Verhalten aktivieren auch fortgeschrittenere Szenarios. Bei *Befehlen* sind Verhalten nützlich, um ein Steuerelement mit einem Befehl zu verbinden. Sie können auch zum Zuordnen von Befehlen zu Steuerelementen verwendet werden, die nicht zum Interagieren mit Befehlen konzipiert sind. Beispielsweise können diese als Reaktion auf ein auslösendes Ereignis zum Aufrufen eines Befehls verwendet werden.

Xamarin.Forms unterstützen zwei verschiedene Verhalten:

- **Xamarin.Forms-Verhalten:** Klassen, die von der Klasse [`Behavior`](xref:Xamarin.Forms.Behavior) oder [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1) abgeleitet werden, in der `T` der Steuerelementtyp ist, für den Verhalten angewendet werden sollte. Weitere Informationen über Xamarin.Forms-Verhalten finden Sie unter [Xamarin.Forms Behaviors (Xamarin.Forms-Verhalten)](~/xamarin-forms/app-fundamentals/behaviors/creating.md) und [Reusable Behaviors (Wiederverwendbare Verhalten)](~/xamarin-forms/app-fundamentals/behaviors/reusable/index.md).
- **Angefügte Verhalten:** `static`-Klassen mit einer oder mehreren angefügten Eigenschaften. Weitere Informationen zu angefügten Verhalten finden Sie unter [Attached Behaviors (Angefügte Verhalten)](~/xamarin-forms/app-fundamentals/behaviors/attached.md).

In diesem Leitfaden werden vor allem Xamarin.Forms-Verhalten beschrieben, da sie die bevorzugte Herangehensweise für die Verhaltenskonstruktion darstellen.

## <a name="related-links"></a>Verwandte Links

- [Behavior class (Behavior-Klasse)](xref:Xamarin.Forms.Behavior)
- [Behavior&lt;T&gt; class (Behavior<T>-Klasse)](xref:Xamarin.Forms.Behavior`1)
