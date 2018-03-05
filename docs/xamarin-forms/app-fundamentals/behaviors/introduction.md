---
title: "Einführung in die Verhaltensweisen"
description: "Verhaltensweisen können Sie Benutzeroberflächen-Steuerelemente ohne Unterklasse diese Funktionalität hinzufügen. Stattdessen wird die Funktionalität in einer Verhaltensklasse implementiert und an das Steuerelement verbunden, als wäre er Teil des Steuerelements selbst. Dieser Artikel bietet eine Einführung in die Verhaltensweisen."
ms.topic: article
ms.prod: xamarin
ms.assetid: 0DF1EF8C-A212-4142-A3C6-DF760A82A757
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 9a92a80bae49a20b276e0d985845fbf08fe92bec
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-behaviors"></a>Einführung in die Verhaltensweisen

_Verhaltensweisen können Sie Benutzeroberflächen-Steuerelemente ohne Unterklasse diese Funktionalität hinzufügen. Stattdessen wird die Funktionalität in einer Verhaltensklasse implementiert und an das Steuerelement verbunden, als wäre er Teil des Steuerelements selbst. Dieser Artikel bietet eine Einführung in die Verhaltensweisen._

Verhalten ermöglichen es Ihnen, Code zu implementieren, die normalerweise als CodeBehind, schreiben, da sie direkt mit der API des Steuerelements in einer Weise interagiert, dass er deutlicher an das Steuerelement verbunden und für die Wiederverwendung über mehr als eine Anwendung verpackt werden kann. Sie können verwendet werden, um eine vollständige Palette von Funktionen für Steuerelemente, z. B. bereitzustellen:

- Hinzufügen einer e-Mail-Überprüfung einer [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/).
- Erstellen eine Bewertung-Steuerelements, das mit einer Tap-Geste-Erkennung.
- Steuern der Animation.
- Hinzufügen eines Effekts an ein Steuerelement an.

Verhalten ermöglichen auch komplexere Szenarien. Im Kontext des *Befehle*, Verhalten sind hilfreich bei der zum Herstellen einer Verbindung eines Steuerelements an einen Befehl. Darüber hinaus können sie Befehle mit Steuerelementen zuordnen, die nicht entwickelt wurden, für die Interaktion mit Befehlen verwendet werden. Sie können beispielsweise verwendet werden, zum Aufrufen eines Befehls als Antwort auf ein Ereignis auslösen.

Xamarin.Forms unterstützt zwei verschiedene Arten von Verhalten:

- **Xamarin.Forms Verhalten** – abgeleitete Klassen die [ `Behavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/) oder [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) -Klasse, in denen `T` ist der Typ des Steuerelements, das Verhalten muss angewendet werden. Weitere Informationen zu Xamarin.Forms Verhalten, finden Sie unter [Xamarin.Forms Verhalten](~/xamarin-forms/app-fundamentals/behaviors/creating.md) und [Wiederverwendbaren Verhalten](~/xamarin-forms/app-fundamentals/behaviors/reusable/index.md).
- **Angefügt von Verhaltensweisen** – `static` Klassen mit einer oder mehreren angefügte Eigenschaften. Weitere Informationen zu angefügten Verhalten, finden Sie unter [angefügten Verhalten](~/xamarin-forms/app-fundamentals/behaviors/attached.md).

Dieses Handbuch konzentriert sich Xamarin.Forms Verhalten, da es sich um den optimalen Ansatz Verhalten Konstruktion gegenüber sind.



## <a name="related-links"></a>Verwandte Links

- [Verhalten](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/)
- [Verhalten<T>](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)
