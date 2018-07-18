---
title: Einführung in die Verhaltensweisen
description: Verhalten können Sie die Steuerelemente der Benutzeroberfläche ohne Unterklasse diese Funktionalität hinzugefügt. Stattdessen wird die Funktionalität in eine Verhaltensklasse implementiert und an das Steuerelement angefügt werden, als wäre es Teil des Steuerelements selbst. Dieser Artikel enthält eine Einführung in die Verhaltensweisen.
ms.prod: xamarin
ms.assetid: 0DF1EF8C-A212-4142-A3C6-DF760A82A757
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 176f41d4b7349af2cf7cc49de8ba0789ad2f8c11
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995813"
---
# <a name="introduction-to-behaviors"></a>Einführung in die Verhaltensweisen

_Verhalten können Sie die Steuerelemente der Benutzeroberfläche ohne Unterklasse diese Funktionalität hinzugefügt. Stattdessen wird die Funktionalität in eine Verhaltensklasse implementiert und an das Steuerelement angefügt werden, als wäre es Teil des Steuerelements selbst. Dieser Artikel enthält eine Einführung in die Verhaltensweisen._

Verhalten ermöglichen es, Code zu implementieren, die Sie normalerweise schreiben müsste, als Code-Behind, da sie direkt mit der API des Steuerelements in einer Weise interagiert, dass es deutlicher an das Steuerelement verbunden und für die Wiederverwendung über mehrere Anwendungen verpackt werden kann. Sie können verwendet werden, um eine Vielzahl von Funktionen für Steuerelemente, z. B. bereitzustellen:

- Hinzufügen zu einem Validierungssteuerelement-e-Mail ein [ `Entry` ](xref:Xamarin.Forms.Entry).
- Erstellen eines bewertungssteuerelements mithilfe einer Tap-stiftbewegungs-Erkennung.
- Zum Steuern einer Animation aus.
- Einen Effekt hinzufügen auf ein Steuerelement.

Verhalten ermöglichen auch komplexere Szenarios. Im Kontext des *Befehle*, Verhalten sind eine nützliche Methode für das Herstellen einer Verbindung eines Steuerelements zu einem Befehl. Darüber hinaus können sie Befehle mit Steuerelementen zuordnen, die nicht entwickelt wurden, für die Interaktion mit Befehlen verwendet werden. Sie können z. B. verwendet werden, zum Aufrufen eines Befehls als Reaktion auf ein Ereignis auslösen.

Xamarin.Forms unterstützt zwei verschiedene Formate Verhaltensweisen:

- **Xamarin.Forms-Verhaltensweisen** – abgeleitete Klassen die [ `Behavior` ](xref:Xamarin.Forms.Behavior) oder [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) -Klasse, in denen `T` ist der Typ des Steuerelements, das Verhalten gelten soll. Weitere Informationen zu Xamarin.Forms-Verhaltensweisen, finden Sie unter [Xamarin.Forms-Verhaltensweisen](~/xamarin-forms/app-fundamentals/behaviors/creating.md) und [Wiederverwendbare Verhaltensweisen](~/xamarin-forms/app-fundamentals/behaviors/reusable/index.md).
- **Angefügte Verhaltensweisen** – `static` Klassen mit einer oder mehreren angefügte Eigenschaften. Weitere Informationen zu angefügten Verhalten, finden Sie unter [angefügte Verhaltensweisen](~/xamarin-forms/app-fundamentals/behaviors/attached.md).

Dieser Leitfaden konzentriert sich auf Xamarin.Forms-Verhaltensweisen, da sie den bevorzugten Ansatz zur Erstellung von Verhalten sind.



## <a name="related-links"></a>Verwandte Links

- [Verhalten](xref:Xamarin.Forms.Behavior)
- [Verhalten&lt;T&gt;](xref:Xamarin.Forms.Behavior`1)
