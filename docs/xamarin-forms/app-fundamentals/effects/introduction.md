---
title: Einführung in die Effekte
description: Effekte ermöglichen die native Steuerelemente für jede Plattform angepasst werden, und für kleine Styling Änderungen in der Regel verwendet werden. Dieser Artikel bietet eine Einführung in die Auswirkungen, werden die Grenze zwischen Auswirkungen und benutzerdefinierten Renderern und beschreibt die PlatformEffect-Klasse.
ms.prod: xamarin
ms.assetid: 30CB8615-8F39-4762-BDB7-333D2B57D112
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 7b859dd58551675c121c5600ba9c691e4280a03b
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2018
---
# <a name="introduction-to-effects"></a>Einführung in die Effekte

_Effekte ermöglichen die native Steuerelemente für jede Plattform angepasst werden, und für kleine Styling Änderungen in der Regel verwendet werden. Dieser Artikel bietet eine Einführung in die Auswirkungen, werden die Grenze zwischen Auswirkungen und benutzerdefinierten Renderern und beschreibt die PlatformEffect-Klasse._

Xamarin.Forms [Seiten, Layouts und Steuerelemente](~/xamarin-forms/user-interface/controls/index.md) stellt eine gemeinsame-API, um plattformübergreifende mobile-Benutzeroberflächen zu beschreiben. Jede Seite, Layout und Steuerelement unterschiedlich auf jeder Plattform verwenden gerendert wird eine `Renderer` -Klasse, die wiederum ein systemeigenen-Steuerelement (entsprechend der Darstellung Xamarin.Forms), erstellt angeordnet, auf dem Bildschirm, und fügt Sie in der gemeinsam verwendeten angegebene Verhalten Code.

Entwickler können ihre eigenen benutzerdefinierten `Renderer`-Klassen implementieren, um die Darstellung und/oder das Verhalten eines Steuerelements anzupassen. Allerdings ist die Implementierung einer benutzerdefinierten Renderer-Klasse, um ein einfaches Steuerelement durchführen häufig eine Heavyweight-Antwort. Effekte vereinfachen dieses Vorgangs ermöglicht die systemeigene Steuerelemente für jede Plattform einfacher angepasst werden.

Effekte werden in plattformspezifischen Projekte erstellt, durch die Erstellung von Unterklassen von der `PlatformEffect` -Steuerelement, und klicken Sie dann die Auswirkungen von Zuordnen eines entsprechenden Steuerelements in einem Projekt Xamarin.Forms Portable Klassenbibliothek (PCL) oder gemeinsam genutzte Bibliothek genutzt werden.

## <a name="why-use-an-effect-over-a-custom-renderer"></a>Gründe für die Nutzung eines Effekts über eines benutzerdefinierten Renderers

Effekte die Anpassung eines Steuerelements zu vereinfachen, können wiederverwendet werden und können parametrisiert sein, dass die um Wiederverwendung weiter zu steigern.

Elemente, die mit einem Effekt erreicht werden können kann auch einen benutzerdefinierten Renderer erreicht werden. Benutzerdefinierte Renderer bieten jedoch größere Flexibilität und Anpassung als Auswirkungen. Die folgenden Richtlinien Listen Sie die Umstände, in dem einen Effekt über einen benutzerdefinierten Renderer auswählen:

- Ein Effekt wird empfohlen, beim Ändern der Eigenschaften eines Steuerelements plattformspezifischen das gewünschte Ergebnis erzielt wird.
- Ein benutzerdefinierter Renderer ist erforderlich, wenn Methoden eines Steuerelements plattformspezifischen überschreiben Portkonflikt vorliegt.
- Ein benutzerdefinierter Renderer ist erforderlich, wenn das plattformspezifische-Steuerelement zu ersetzen, das ein Steuerelement Xamarin.Forms implementiert Portkonflikt vorliegt.

## <a name="subclassing-the-platformeffect-class"></a>Erstellen von Unterklassen für die PlatformEffect-Klasse

Die folgende Tabelle enthält den Namespace für die `PlatformEffect` Klasse für jede Plattform und den Typen seiner Eigenschaften:

|Plattform|Namespace|Container|Steuerelement|
|--- |--- |--- |--- |
|iOS|Xamarin.Forms.Platform.iOS|UIView|UIView|
|Android|Xamarin.Forms.Platform.Android|' ViewGroup '|Ansicht|
|Universelle Windows-Plattform (UWP)|Xamarin.Forms.Platform.UWP|FrameworkElement|FrameworkElement|

Jede Clientplattform-spezifische `PlatformEffect` Klasse macht die folgenden Eigenschaften:

- `Container` – verweist auf die plattformspezifischen-Steuerelement verwendet wird, um das Layout zu implementieren.
- `Control` – verweist auf die plattformspezifischen-Steuerelement wird verwendet, um die Xamarin.Forms-Steuerelement zu implementieren.
- `Element` – verweist auf das Xamarin.Forms-Steuerelement, das gerendert wird.

Auswirkungen müssen keine Typinformationen über den Container, Steuerelement oder Element, das sie angefügt sind, da sie auf ein Element angefügt werden können. Daher sollte es beim Auswirkungen auf ein Element angefügt wird, die sie unterstützen keine stufenweise Lösung oder löst eine Ausnahme. Allerdings die `Container`, `Control`, und `Element` Eigenschaften auf ihre implementierende Typ umgewandelt werden können. Weitere Informationen zu diesen finden Sie unter Datentypen [Renderer-Basisklassen und systemeigenen Steuerelementen](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Jede Clientplattform-spezifische `PlatformEffect` Klasse macht die folgenden Methoden, die zum Implementieren eines Effekts überschrieben werden müssen:

- [`OnAttached`](https://developer.xamarin.com/api/member/Xamarin.Forms.Effect.OnAttached()/) – wird aufgerufen, wenn eine Auswirkung auf ein Steuerelement Xamarin.Forms angefügt ist. Eine überschriebene Version dieser Methode, die in jeder Klasse Platfom-spezifische Effekt ist der Ort zum Ausführen der Anpassung des Steuerelements, zusammen mit der Ausnahmebehandlung für den Fall, dass die Auswirkungen auf das angegebene Xamarin.Forms-Steuerelement angewendet werden kann.
- [`OnDetached`](https://developer.xamarin.com/api/member/Xamarin.Forms.Effect.OnDetached()/) – wird aufgerufen, wenn Sie ein Effekt aus einem Steuerelement Xamarin.Forms getrennt wird. Eine überschriebene Version dieser Methode, die in jeder Klasse plattformspezifischen wirksam ist die Stelle, um eine Auswirkung Bereinigung ausführen, z. B. Deduplizierung registrieren einen Ereignishandler ausführen.

Darüber hinaus die `PlatformEffect` macht die [ `OnElementPropertyChanged` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformEffect%3CTContainer,TControl%3E.OnElementPropertyChanged/p/System.ComponentModel.PropertyChangedEventArgs/) -Methode, die auch überschrieben werden kann. Diese Methode wird aufgerufen, wenn eine Eigenschaft des Elements geändert wurde. Eine überschriebene Version dieser Methode, die in jeder Klasse plattformspezifischen Effekt ist der Ort mit bindbare eigenschaftenänderungen auf das Steuerelement Xamarin.Forms reagieren. Ein Kontrollkästchen für die Eigenschaft, die geändert werden sollten immer vorgenommen werden, wie diese Außerkraftsetzung oft aufgerufen werden kann.


## <a name="related-links"></a>Verwandte Links

- [Benutzerdefinierte Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
