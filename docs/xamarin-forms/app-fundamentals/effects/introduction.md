---
title: Einführung in die Effekte
description: Können durch Effekte native Steuerelemente auf den einzelnen Plattformen angepasst werden, und für kleine Formatierungsänderungen in der Regel verwendet werden. Dieser Artikel bietet eine Einführung in die Auswirkungen, wird beschrieben, die Grenze zwischen Effekte und benutzerdefinierten Renderern und beschreibt die PlatformEffect-Klasse.
ms.prod: xamarin
ms.assetid: 30CB8615-8F39-4762-BDB7-333D2B57D112
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: d3fa958e999a10832d5fa15e4190077955b0e6df
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997378"
---
# <a name="introduction-to-effects"></a>Einführung in die Effekte

_Können durch Effekte native Steuerelemente auf den einzelnen Plattformen angepasst werden, und für kleine Formatierungsänderungen in der Regel verwendet werden. Dieser Artikel bietet eine Einführung in die Auswirkungen, wird beschrieben, die Grenze zwischen Effekte und benutzerdefinierten Renderern und beschreibt die PlatformEffect-Klasse._

Xamarin.Forms [Seiten, Layouts und Steuerelemente](~/xamarin-forms/user-interface/controls/index.md) stellt eine allgemeine API, um plattformübergreifende mobile-Benutzeroberflächen zu beschreiben. Jede Seite, Layout und Steuerelement unterschiedlich auf jeder Plattform mit gerendert wird eine `Renderer` -Klasse, die wiederum ein natives Steuerelement (entsprechend der Darstellung in Xamarin.Forms), erstellt angeordnet, auf dem Bildschirm und fügt Sie in der gemeinsam verwendeten angegebene Verhalten hinzu Code.

Entwickler können ihre eigenen benutzerdefinierten `Renderer`-Klassen implementieren, um die Darstellung und/oder das Verhalten eines Steuerelements anzupassen. Implementieren einer benutzerdefinierten Renderer-Klasse, um ein einfaches Steuerelement anpassen ist jedoch oft eine Heavyweight-Antwort. Effekte vereinfachen dieses Vorgangs können native Steuerelemente auf den einzelnen Plattformen einfacher angepasst werden.

Effekte werden in plattformspezifischen Projekten erstellt, indem Unterklassen der `PlatformEffect` -Steuerelement, und klicken Sie dann die Auswirkungen durch das Anfügen an ein geeignetes Steuerelement in einem Xamarin.Forms .NET Standard-Bibliothek oder eine freigegebene Bibliothek-Projekt verwendet werden.

## <a name="why-use-an-effect-over-a-custom-renderer"></a>Warum verwenden Sie einen Effekt über einen benutzerdefinierten Renderer?

Auswirkungen die Anpassung eines Steuerelements vereinfachen, können wiederverwendet werden und parametrisiert werden können, um die Wiederverwendung weiter zu erhöhen.

Alle Elemente, die mit einem Effekt erzielt werden kann kann auch mit einem benutzerdefinierten Renderer erreicht werden. Benutzerdefinierte Renderer bieten jedoch größere Flexibilität und Anpassungsfähigkeit als bei Auswirkungen. Die folgenden Richtlinien Listen Sie die Umstände, wählen Sie einen Effekt auf ein benutzerdefinierter Renderer auf:

- Ein Effekt wird empfohlen, beim Ändern der Eigenschaften eines Steuerelements plattformspezifische das gewünschte Ergebnis erzielt wird.
- Ein benutzerdefinierter Renderer ist erforderlich, wenn besteht die Notwendigkeit eines Steuerelements plattformspezifische Überschreibungsmethoden.
- Ein benutzerdefinierter Renderer ist erforderlich, wenn es ist das plattformspezifische-Steuerelement zu ersetzen, das eine Xamarin.Forms-Steuerelement implementiert werden muss.

## <a name="subclassing-the-platformeffect-class"></a>Erstellen von Unterklassen für die PlatformEffect-Klasse

Die folgende Tabelle enthält den Namespace für die `PlatformEffect` -Klasse auf jeder Plattform, und die Typen seiner Eigenschaften:

|Plattform|Namespace|Container|Steuerelement|
|--- |--- |--- |--- |
|iOS|Xamarin.Forms.Platform.iOS|UIView|UIView|
|Android|Xamarin.Forms.Platform.Android|ViewGroup|Ansicht|
|Universelle Windows-Plattform (UWP)|Xamarin.Forms.Platform.UWP|FrameworkElement|FrameworkElement|

Jedes plattformspezifische `PlatformEffect` Klasse stellt die folgenden Eigenschaften:

- `Container` – verweist auf das plattformspezifische-Steuerelement wird verwendet, um das Layout zu implementieren.
- `Control` – verweist auf das plattformspezifische-Steuerelement, das zum Implementieren des Xamarin.Forms-Steuerelements verwendet wird.
- `Element` – verweist auf das Xamarin.Forms-Steuerelement, das gerendert wird.

Auswirkungen müssen keine Typinformationen über den Container, -Steuerelement oder Element, das sie angefügt werden, da sie auf ein Element angefügt werden können. Aus diesem Grund sollten sie Wenn ein Effekt auf ein Element angefügt wird, die sie nicht unterstützt teilausfall oder eine Ausnahme auslösen. Allerdings die `Container`, `Control`, und `Element` Eigenschaften, die ihrem implementierenden Typ umgewandelt werden können. Weitere Informationen zu diesen finden Sie unter Datentypen [Renderer-Basisklassen und Native Steuerelemente](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Jedes plattformspezifische `PlatformEffect` Klasse stellt die folgenden Methoden, die überschrieben werden muss, um einen Effekt zu implementieren:

- [`OnAttached`](xref:Xamarin.Forms.Effect.OnAttached) : wird aufgerufen, wenn Sie ein Effekt auf ein Xamarin.Forms-Steuerelement angefügt ist. Diese Methode in jeder Klasse Platfom-spezifische Auswirkungen eine überschriebene Version ist der Ort, an die Anpassung des Steuerelements, zusammen mit der Ausnahmebehandlung für den Fall, dass die Auswirkungen auf das angegebene Xamarin.Forms-Steuerelement angewendet werden kann.
- [`OnDetached`](xref:Xamarin.Forms.Effect.OnDetached) : wird aufgerufen, wenn Sie ein Effekt aus einer Xamarin.Forms-Steuerelements getrennt wird. Eine außer Kraft gesetzte Version von dieser Methode in jeder Klasse plattformspezifische wirksam ist der Ort, um Bereinigungen Effekt, wie z. B. das Aufheben der Registrierung eines ereignishandlers ausführen.

Darüber hinaus die `PlatformEffect` macht die [ `OnElementPropertyChanged` ](xref:Xamarin.Forms.PlatformEffect`2.OnElementPropertyChanged(System.ComponentModel.PropertyChangedEventArgs)) -Methode, die auch überschrieben werden kann. Diese Methode wird aufgerufen, wenn eine Eigenschaft des Elements geändert wurde. Eine außer Kraft gesetzte Version von dieser Methode in jeder Klasse plattformspezifische wirksam ist der Ort, an die Reaktion auf Änderungen der bindbare Eigenschaft in der Xamarin.Forms-Steuerelements. Überprüft, ob die geänderte Eigenschaft sollten immer vorgenommen werden, wie diese Außerkraftsetzung oft aufgerufen werden kann.


## <a name="related-links"></a>Verwandte Links

- [Benutzerdefinierte Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
