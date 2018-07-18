---
title: Einführung in benutzerdefinierte Renderer
description: Dieser Artikel bietet eine Einführung in benutzerdefinierten Renderern und erläutert den Prozess zum Erstellen eines benutzerdefinierten Renderers.
ms.prod: xamarin
ms.assetid: 264314BE-1C5C-4727-A14E-F6F98151CDBD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/19/2016
ms.openlocfilehash: 180a196e0b95854815c8a74ef1d2df63407dd04f
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998003"
---
# <a name="introduction-to-custom-renderers"></a>Einführung in benutzerdefinierte Renderer

_Benutzerdefinierte Renderer bieten einen sehr effizienter Ansatz zum Anpassen der Darstellung und das Verhalten von Xamarin.Forms-Steuerelementen. Sie können für kleine Formatierungsänderungen oder komplexe plattformspezifische Layout und verhaltensanpassung verwendet werden. Dieser Artikel bietet eine Einführung in benutzerdefinierten Renderern und erläutert den Prozess zum Erstellen eines benutzerdefinierten Renderers._

Xamarin.Forms [Seiten, Layouts und Steuerelemente](~/xamarin-forms/user-interface/controls/index.md) stellen eine allgemeine API, um plattformübergreifende mobile-Benutzeroberflächen zu beschreiben. Jede Seite, Layout und Steuerelement unterschiedlich auf jeder Plattform gerendert wird mit einer `Renderer` Klasse, die wiederum ein natives Steuerelement (entsprechend der Darstellung in Xamarin.Forms), erstellt angeordnet, auf dem Bildschirm und fügt das Verhalten der freigegebener Code.

Entwickler können ihre eigenen benutzerdefinierten `Renderer`-Klassen implementieren, um die Darstellung und/oder das Verhalten eines Steuerelements anzupassen. Ein-Anwendungsprojekt, um das Steuerelement an einem Ort anpassen, während gleichzeitig das Standardverhalten auf anderen Plattformen können benutzerdefinierte Renderer für einen bestimmten Typ hinzugefügt werden; oder andere benutzerdefinierte Renderer jedes Anwendungsprojekt zum Erstellen von einem anderen Aussehen und Verhalten auf iOS, Android und die universelle Windows-Plattform (UWP) hinzugefügt werden können. Implementieren einer benutzerdefinierten Renderer-Klasse, um ein einfaches Steuerelement anpassen ist jedoch oft eine Heavyweight-Antwort. Auswirkungen dieser Prozess vereinfacht und in der Regel für kleine Formatierungsänderungen verwendet werden. Weitere Informationen finden Sie unter [Effekte](~/xamarin-forms/app-fundamentals/effects/index.md).

## <a name="examining-why-custom-renderers-are-necessary"></a>Untersuchen warum benutzerdefinierte Renderer sind erforderlich

Ändern der Darstellung eines Xamarin.Forms-Steuerelements, ist ohne Verwendung eines benutzerdefinierten Renderers, ein zweistufiger Prozess, der umfasst das Erstellen eines benutzerdefinierten Steuerelements durch Erstellen von Unterklassen und die anschließende Verwendung des benutzerdefinierten Steuerelements anstelle der ursprünglichen Steuerelements. Das folgende Codebeispiel zeigt ein Beispiel für Unterklassen der `Entry` Steuerelement:

```csharp
public class MyEntry : Entry
{
  public MyEntry ()
  {
    BackgroundColor = Color.Gray;
  }
}
```

Die `MyEntry` Steuerelement ist ein `Entry` steuern, wo die `BackgroundColor` nastaven NA hodnotu gray und können nicht durch deklarieren einen Namespace für den Speicherort und verwenden das Namespacepräfix für das Steuerelement, das in Xaml referenziert werden. Das folgende Codebeispiel zeigt wie die `MyEntry` benutzerdefiniertes Steuerelement kann genutzt werden, indem eine `ContentPage`:

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

Die `local` Namespacepräfix kann alles sein. Allerdings die `namespace` und `assembly` -Werte müssen die Details des benutzerdefinierten Steuerelements entsprechen. Sobald der Namespace deklariert wird, wird das Präfix verwendet, auf das benutzerdefinierte Steuerelement verweisen.

> [!NOTE]
> Definieren der `xmlns` ist viel einfacher in Projekten für .NET Standard-Bibliothek als freigegebene Projekte. .NET Standard-Bibliothek in eine Assembly kompiliert wird, daher ist es einfach, welche die `assembly=CustomRenderer` Wert sein soll. Wenn Sie freigegebene Projekte verwenden, werden alle freigegebenen Assets (einschließlich der XAML) in jede der verweisende Projekte, die bedeutet, dass bei iOS-, Android- und UWP kompiliert Projekte verfügen über eigene *Assemblynamen* ist es unmöglich, schreiben die `xmlns` Deklaration, da der Wert für jede Anwendung unterschiedlich sein muss. Benutzerdefinierte Steuerelemente in XAML für freigegebene Projekte müssen jedes Anwendungsprojekt mit dem gleichen Assemblynamen konfiguriert werden.

Die `MyEntry` benutzerdefiniertes Steuerelement wird dann auf jeder Plattform mit einem grauen Hintergrund gerendert, wie in den folgenden Screenshots gezeigt:

![](introduction-images/screenshots.png "\"Myentry\" benutzerdefiniertes Steuerelement auf jeder Plattform")

Ändern der Hintergrundfarbe des Steuerelements auf jeder Plattform wurde ausschließlich über das Erstellen von Unterklassen für das Steuerelement erreicht. Dieses Verfahren ist jedoch beschränkt, in was sie erreichen kann, da es nicht möglich, zu der Clientplattform-spezifische Erweiterungen und Anpassungen nutzen. Wenn sie benötigt werden, müssen der benutzerdefinierte Renderer implementiert werden.

## <a name="creating-a-custom-renderer-class"></a>Erstellen einer benutzerdefinierten Renderer-Klasse

Der Prozess zum Erstellen einer benutzerdefinierten Renderer-Klasse sieht folgendermaßen aus:

1. Erstellen Sie eine Unterklasse der Rendererklasse, die das native Steuerelement rendert.
1. Überschreiben Sie die Methode, die das native Steuerelement rendert und Schreiben Sie Logik, um das Steuerelement anpassen. Häufig die `OnElementChanged` Methode wird verwendet, um das native Steuerelement zu rendern, das aufgerufen wird, wenn das entsprechende Xamarin.Forms-Steuerelement erstellt wird.
1. Hinzufügen einer `ExportRenderer` -Attribut der benutzerdefinierten Renderer-Klasse, um anzugeben, dass es zum Rendern des Xamarin.Forms-Steuerelements verwendet werden soll. Dieses Attribut wird verwendet, um den benutzerdefinierten Renderer mit Xamarin.Forms zu registrieren.

> [!NOTE]
> Für die meisten Xamarin.Forms-Elemente ist optional, um einen benutzerdefinierten Renderer in jedem plattformprojekt bereitzustellen. Wenn ein benutzerdefinierter Renderer nicht registriert ist, wird der Standard-Renderer für die Basisklasse des Steuerelements verwendet werden. Benutzerdefinierte Renderer sind jedoch erforderlich, in dem jedes plattformprojekt beim Rendern einer [Ansicht](xref:Xamarin.Forms.View) oder [ViewCell](xref:Xamarin.Forms.ViewCell) Element.

Die Themen in dieser Serie bieten Demos und Erklärungen dieses Prozesses für die verschiedenen Elemente von Xamarin.Forms.

## <a name="troubleshooting"></a>Problembehandlung

Wenn ein benutzerdefiniertes Steuerelement in enthalten ist ein .NET Standard Library-Projekt, die mit der Lösung (z. B. nicht .NET Standardbibliothek von Visual Studio für Mac/Visual Studio-Xamarin.Forms-App-Projektvorlage erstellt), die eine Ausnahme hinzugefügt wurde in iOS auftreten wenn Es wird versucht, auf das benutzerdefinierte Steuerelement. Wenn dieses Problem auftritt, können Sie durch Erstellen eines Verweises auf das benutzerdefinierte Steuerelement aus aufgelöst werden die `AppDelegate` Klasse:

```csharp
var temp = new ClassInPCL(); // in AppDelegate, but temp not used anywhere
```

Dies erzwingt, dass den Compiler erkennt die `ClassInPCL` Typ aufgelöst werden kann. Sie können auch die `Preserve` Attribut hinzugefügt werden kann der `AppDelegate` Klasse, um das gleiche Ergebnis erzielen:

```csharp
[assembly: Preserve (typeof (ClassInPCL))]
```

Dies erstellt einen Verweis auf die `ClassInPCL` Typ, der angibt, dass es zur Laufzeit benötigt wird. Weitere Informationen finden Sie unter [beibehalten Code](~/ios/deploy-test/linker.md).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde eine Einführung in benutzerdefinierte Renderer bereitgestellt und ist den Prozess zum Erstellen eines benutzerdefinierten Renderers beschrieben. Benutzerdefinierte Renderer bieten einen sehr effizienter Ansatz zum Anpassen der Darstellung und das Verhalten von Xamarin.Forms-Steuerelementen. Sie können für kleine Formatierungsänderungen oder komplexe plattformspezifische Layout und verhaltensanpassung verwendet werden.


## <a name="related-links"></a>Verwandte Links

- [Effekte](~/xamarin-forms/app-fundamentals/effects/index.md)
