---
title: Einführung in benutzerdefinierte Renderer
description: Dieser Artikel bietet eine Einführung in benutzerdefinierten Renderern und erläutert das Verfahren zum Erstellen eines benutzerdefinierten Renderers.
ms.prod: xamarin
ms.assetid: 264314BE-1C5C-4727-A14E-F6F98151CDBD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/19/2016
ms.openlocfilehash: fa22be081433bdd0c59a0d921511d3f3d83d4448
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241763"
---
# <a name="introduction-to-custom-renderers"></a>Einführung in benutzerdefinierte Renderer

_Benutzerdefinierter Renderer bieten einen sehr effizienter Ansatz zum Anpassen der Darstellung und Verhalten von Steuerelementen mit Xamarin.Forms. Sie können für kleine Styling Änderungen oder anspruchsvolle plattformspezifischen Layout und Verhalten Anpassung verwendet werden. Dieser Artikel bietet eine Einführung in benutzerdefinierten Renderern und erläutert das Verfahren zum Erstellen eines benutzerdefinierten Renderers._

Xamarin.Forms [Seiten, Layouts und Steuerelemente](~/xamarin-forms/user-interface/controls/index.md) eine allgemeine API zum Beschreiben von plattformübergreifenden mobilen von Benutzeroberflächen darzustellen. Jede Seite, Layout und Steuerelement unterschiedlich auf jeder Plattform gerendert wird mithilfe einer `Renderer` -Klasse, die wiederum ein systemeigenen-Steuerelement (entsprechend der Darstellung Xamarin.Forms), erstellt angeordnet, auf dem Bildschirm, und fügt das Verhalten der gemeinsam verwendeten Code.

Entwickler können ihre eigenen benutzerdefinierten `Renderer`-Klassen implementieren, um die Darstellung und/oder das Verhalten eines Steuerelements anzupassen. Eine Anwendung-Projekt, um das Steuerelement an einem Ort anpassen, während gleichzeitig das Standardverhalten auf anderen Plattformen können benutzerdefinierten Renderer für einen bestimmten Typ hinzugefügt werden. oder anderen benutzerdefinierten Renderer können jede Anwendungsprojekt zum Erstellen von einem anderen Aussehen und Verhalten auf iOS-, Android- und die universelle Windows-Plattform (UWP) hinzugefügt werden. Allerdings ist die Implementierung einer benutzerdefinierten Renderer-Klasse, um ein einfaches Steuerelement durchführen häufig eine Heavyweight-Antwort. Auswirkungen dieser Prozess vereinfacht, und sind für kleine Styling Änderungen in der Regel verwendet. Weitere Informationen finden Sie unter [Effekte](~/xamarin-forms/app-fundamentals/effects/index.md).

## <a name="examining-why-custom-renderers-are-necessary"></a>Untersuchen Warum benutzerdefinierten Renderern sind erforderlich

Ändern der Darstellung eines Steuerelements Xamarin.Forms, ein zweistufiger Prozess, der umfasst das Erstellen eines benutzerdefinierten Steuerelements durch Erstellen von Unterklassen für, und klicken Sie dann das benutzerdefinierte Steuerelement anstelle der ursprünglichen Steuerelement nutzen, ohne Verwendung eines benutzerdefinierten Renderers ist. Das folgende Codebeispiel zeigt ein Beispiel für die Erstellung von Unterklassen von der `Entry` Steuerelement:

```csharp
public class MyEntry : Entry
{
  public MyEntry ()
  {
    BackgroundColor = Color.Gray;
  }
}
```

Die `MyEntry` -Steuerelement ist ein `Entry` steuern, wo die `BackgroundColor` auf festgelegt ist grau, und kann in Xaml durch deklarieren einen Namespace für den Speicherort der und verwenden das Namespacepräfix für das Steuerelement-Element verwiesen werden. Im folgenden Codebeispiel wird veranschaulicht wie die `MyEntry` benutzerdefiniertes Steuerelement genutzt werden kann, indem Sie eine `ContentPage`:

```xaml
<ContentPage
    ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <local:MyEntry Text="In Shared Code" />
    ...
</ContentPage>
```

Die `local` Namespacepräfix kann alles sein. Allerdings die `namespace` und `assembly` Werte müssen die Details des benutzerdefinierten Steuerelements übereinstimmen. Nach der Namespace deklariert ist, wird das Präfix verwendet, auf das benutzerdefinierte Steuerelement verweisen.

> [!NOTE]
> Definieren der `xmlns` ist deutlich einfacher in standardmäßigen .NET Library-Projekte als freigegebene Projekte. Eine .NET Standardbibliothek wird in eine Assembly kompiliert, daher ist es einfach zu bestimmen, welche die `assembly=CustomRenderer` Wert sein soll. Bei Verwendung von freigegebenen Projekten werden alle freigegebenen Ressourcen (einschließlich des XAML-Codes) kompiliert, in jede der verweisenden-Projekten; Dies bedeutet, dass bei iOS, Android und uwp-Projekte verfügen über eigene *Assemblynamen* unmöglich ist, schreiben die `xmlns` Deklaration, da der Wert für jede Anwendung unterschiedlich sein muss. Benutzerdefinierte Steuerelemente in XAML für freigegebene Projekte erfordern alle Anwendungsprojekt mit demselben Namen konfiguriert werden.

Die `MyEntry` benutzerdefiniertes Steuerelement wird dann für jede Plattform, mit einem grauen Hintergrund gerendert, wie in den folgenden Screenshots dargestellt:

![](introduction-images/screenshots.png "\"Myentry\" benutzerdefiniertes Steuerelement auf jeder Plattform")

Ändern die Hintergrundfarbe des Steuerelements auf jeder Plattform verfügt ausschließlich über das Erstellen von Unterklassen für das Steuerelement erreicht wurde. Allerdings wird diese Technik in was er erreichen kann, da es nicht möglich, zu der Clientplattform-spezifische Erweiterungen und Anpassungen nutzen ist beschränkt. Wenn sie benötigt werden, müssen der benutzerdefinierten Renderer implementiert werden.

## <a name="creating-a-custom-renderer-class"></a>Erstellen einer benutzerdefinierten Renderer-Klasse

Der Prozess zum Erstellen einer benutzerdefinierten Rendererklasse lautet wie folgt:

1. Erstellen Sie eine Unterklasse der Rendererklasse, die das systemeigene Steuerelement rendert.
1. Überschreiben Sie die Methode, die das systemeigene Steuerelement gerendert wird, und erstellen Sie Logik zum Anpassen des Steuerelements. Häufig die `OnElementChanged` Methode wird verwendet, um das Rendern des systemeigenen Steuerelements, die aufgerufen wird, wenn das entsprechende Xamarin.Forms-Steuerelement erstellt wird.
1. Hinzufügen einer `ExportRenderer` -Attribut auf die benutzerdefinierten Renderer-Klasse, um anzugeben, dass es zum Rendern des Steuerelements Xamarin.Forms verwendet werden soll. Dieses Attribut wird verwendet, um den benutzerdefinierten Renderer mit Xamarin.Forms zu registrieren.

> [!NOTE]
> Für die meisten Xamarin.Forms Elemente ist optional, geben Sie einen benutzerdefinierten Renderer in jedem plattformprojekt. Wenn ein benutzerdefinierter Renderer nicht registriert ist, wird der Standardrenderer für die Basisklasse für das Steuerelement verwendet werden. Benutzerdefinierte Renderer sind jedoch erforderlich, in jede plattformprojekt beim Rendern einer [Ansicht](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) oder [ViewCell](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) Element.

Die Themen in dieser Serie bieten Demonstrationen und erläuterungen zu den von diesem Prozess für verschiedene Xamarin.Forms Elemente.

## <a name="troubleshooting"></a>Problembehandlung

Wenn ein benutzerdefiniertes Steuerelement, in enthalten ist ein .NET Standard-Bibliotheksprojekt, das der Projektmappe (d. h. nicht die .NET Standardbibliothek durch den Visual Studio für Mac/Visual Studio Xamarin.Forms-App-Projektvorlage erstellt), eine Ausnahme hinzugefügt wurde in iOS-auftreten bei Es wird versucht, auf das benutzerdefinierte Steuerelement. Wenn dieses Problem tritt auf, es kann aufgelöst werden, erstellen Sie einen Verweis auf das benutzerdefinierte Steuerelement aus der `AppDelegate` Klasse:

```csharp
var temp = new ClassInPCL(); // in AppDelegate, but temp not used anywhere
```

Dies erzwingt den Compiler erkennt die `ClassInPCL` Typ aufgelöst werden kann. Alternativ können Sie die `Preserve` Attribut kann hinzugefügt werden, um die `AppDelegate` Klasse, um das gleiche Ergebnis erzielen:

```csharp
[assembly: Preserve (typeof (ClassInPCL))]
```

Dies erstellt einen Verweis auf die `ClassInPCL` Typ gibt an, dass es zur Laufzeit erforderlich ist. Weitere Informationen finden Sie unter [bleiben Code](~/ios/deploy-test/linker.md).

## <a name="summary"></a>Zusammenfassung

Dieser Artikel stellt eine Einführung in die benutzerdefinierten Renderern und hat den Prozess zum Erstellen eines benutzerdefinierten Renderers beschrieben. Benutzerdefinierter Renderer bieten einen sehr effizienter Ansatz zum Anpassen der Darstellung und Verhalten von Steuerelementen mit Xamarin.Forms. Sie können für kleine Styling Änderungen oder anspruchsvolle plattformspezifischen Layout und Verhalten Anpassung verwendet werden.


## <a name="related-links"></a>Verwandte Links

- [Effekte](~/xamarin-forms/app-fundamentals/effects/index.md)
