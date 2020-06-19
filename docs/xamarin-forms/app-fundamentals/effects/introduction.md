---
title: Einführung in Effekte
description: Durch Effekte können native Steuerelemente auf jeder Plattform angepasst werden. Sie werden normalerweise für kleine Formatierungsänderungen verwendet. In diesem Artikel werden Effekte vorgestellt und die Unterschiede zwischen Effekten und benutzerdefinierten Renderern verdeutlicht. Darüber hinaus wird die PlatformEffect-Klasse beschrieben.
ms.prod: xamarin
ms.assetid: 30CB8615-8F39-4762-BDB7-333D2B57D112
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a891ec70f6f83984ed463fe914442758bdf57a2e
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84139307"
---
# <a name="introduction-to-effects"></a>Einführung in Effekte

_Durch Effekte können native Steuerelemente auf jeder Plattform angepasst werden. Sie werden normalerweise für kleine Formatierungsänderungen verwendet. In diesem Artikel werden Effekte vorgestellt und die Unterschiede zwischen Effekten und benutzerdefinierten Renderern verdeutlicht. Darüber hinaus wird die PlatformEffect-Klasse beschrieben._

Die [Seiten, Layouts und Steuerelemente](~/xamarin-forms/user-interface/controls/index.md) von Xamarin.Forms verfügen über eine gemeinsame API, mit der plattformübergreifende mobile Benutzeroberflächen beschrieben werden können. Die einzelnen Seiten, Layouts und Steuerelemente werden je nach Plattform unterschiedlich mithilfe einer `Renderer`-Klasse gerendert. Diese erstellt ein natives Steuerelement (entsprechend der Xamarin.Forms-Darstellung), ordnet dieses auf dem Bildschirm an und fügt das im gemeinsamen Code angegebene Verhalten hinzu.

Entwickler können ihre eigenen benutzerdefinierten `Renderer`-Klassen implementieren, um die Darstellung und/oder das Verhalten eines Steuerelements anzupassen. Das Implementieren einer benutzerdefinierten Rendererklasse zum Ausführen einer einfachen Anpassung eines Steuerelements ist jedoch oft eine komplizierte Antwort. Durch Effekte wird dieser Prozess vereinfacht. Sie ermöglichen, dass die nativen Steuerelemente auf den einzelnen Plattformen einfacher angepasst werden können.

Effekte werden in plattformspezifischen Projekten erstellt, indem Unterklassen für das Steuerelement `PlatformEffect` erstellt werden. Anschließend werden sie verarbeitet, indem sie in einer .NET Standard-Bibliothek oder einem freigegebenen Bibliotheksprojekt in Xamarin.Forms an das entsprechende Steuerelement angefügt werden.

## <a name="why-use-an-effect-over-a-custom-renderer"></a>Warum soll ein Effekt eher als ein benutzerdefinierter Renderer verwendet werden?

Durch Effekte wird die Anpassung eines Steuerelements vereinfacht. Zudem sind sie wiederverwendbar und können parametrisiert werden, um die Wiederverwendung weiter zu erhöhen.

Alles, was mit einem Effekt erzielt werden kann, kann auch mit einem benutzerdefinierten Renderer erzielt werden. Benutzerdefinierte Renderer bieten jedoch mehr Flexibilität und Anpassungsfähigkeit als Effekte. In den folgenden Richtlinien werden die Umstände aufgeführt, unter denen bevorzugt ein Effekt ausgewählt werden sollte:

- Die Verwendung eines Effekts wird empfohlen, wenn das gewünschte Ergebnis durch das Ändern der Eigenschaften eines plattformspezifischen Steuerelements erzielt wird.
- Ein benutzerdefinierter Renderer ist erforderlich, wenn Methoden eines plattformspezifischen Steuerelements überschrieben werden müssen.
- Zudem ist ein benutzerdefinierter Renderer erforderlich, wenn das plattformspezifische Steuerelement, das ein Xamarin.Forms-Steuerelement implementiert, ersetzt werden muss.

## <a name="subclassing-the-platformeffect-class"></a>Erstellen von Unterklassen für die PlatformEffect-Klasse

Die folgende Tabelle enthält den Namespace der `PlatformEffect`-Klasse auf den jeweiligen Plattformen und die Typen der zugehörigen Eigenschaften:

|Plattform|Namespace|Container|Steuerelement|
|--- |--- |--- |--- |
|iOS|Xamarin.Forms.Platform.iOS|UIView|UIView|
|Android|Xamarin.Forms.Platform.Android|ViewGroup|Ansicht|
|Universelle Windows-Plattform (UWP)|Xamarin.Forms.Platform.UWP|FrameworkElement|FrameworkElement|

Die einzelnen plattformspezifischen `PlatformEffect`-Klassen machen folgende Eigenschaften verfügbar:

- `Container`: Verweist auf das plattformspezifische Steuerelement, das für die Implementierung des Layouts verwendet wird.
- `Control`: verweist auf das plattformspezifische Steuerelement, das für die Implementierung des Xamarin.Forms-Steuerelements verwendet wird
- `Element`: verweist auf das Xamarin.Forms-Steuerelement, das gerendert wird

Effekte weisen keine Typinformationen zu dem Container, Steuerelement oder Element auf, an das sie angefügt sind, da sie an ein beliebiges Element angefügt werden können. Wenn ein Effekt an ein Element angefügt wird, das nicht unterstützt wird, sollte dieses Problem daher diskret behoben oder eine Ausnahme ausgelöst werden. Die `Container`-, `Control`- und `Element`-Eigenschaften können jedoch in ihren Implementierungstyp umgewandelt werden. Weitere Informationen zu diesen Typen finden Sie unter [Renderer-Basisklassen und native Steuerelemente](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Jede plattformspezifische `PlatformEffect`-Klasse macht die folgenden Methoden verfügbar, die zur Implementierung eines Effekts überschrieben werden müssen:

- [`OnAttached`](xref:Xamarin.Forms.Effect.OnAttached): Diese Methode wird aufgerufen, wenn ein Effekt an ein Xamarin.Forms-Steuerelement angefügt wird. In jeder plattformspezifischen Effektklasse ist eine überschriebene Version dieser Methode verfügbar, mit der das Steuerelement angepasst werden kann. In dieser Methode wird auch die Ausnahmebehandlung durchgeführt, falls der Effekt nicht auf das angegebene Xamarin.Forms-Steuerelement angewendet werden kann.
- [`OnDetached`](xref:Xamarin.Forms.Effect.OnDetached): Diese Methode wird aufgerufen, wenn ein Effekt von einem Xamarin.Forms-Steuerelement gelöst wird. In einer überschriebenen Version dieser Methode werden, in den einzelnen plattformspezifischen Effect-Klassen, sämtliche Effekte bereinigt, z. B. durch die Aufhebung der Registrierung eines Ereignishandlers.

Darüber hinaus macht die `PlatformEffect`-Klasse die [`OnElementPropertyChanged`](xref:Xamarin.Forms.PlatformEffect`2.OnElementPropertyChanged(System.ComponentModel.PropertyChangedEventArgs))-Methode verfügbar, die auch überschrieben werden kann. Diese Methode wird aufgerufen, wenn eine Eigenschaft des Elements geändert wurde. In jeder plattformspezifischen Effektklasse ist eine überschriebene Version dieser Methode verfügbar, mit der auf Änderungen der bindbaren Eigenschaften am Xamarin.Forms-Steuerelement reagiert werden kann. Eine geänderte Eigenschaft sollte immer überprüft werden, da die Möglichkeit besteht, dass diese Überschreibung häufig aufgerufen wird.

## <a name="related-links"></a>Verwandte Links

- [Benutzerdefinierte Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
