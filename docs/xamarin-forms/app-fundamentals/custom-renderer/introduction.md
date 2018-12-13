---
title: Einführung in benutzerdefinierte Renderer
description: In diesem Artikel werden benutzerdefinierte Renderer vorgestellt, und Sie erfahren, wie Sie einen benutzerdefinierten Renderer erstellen können.
ms.prod: xamarin
ms.assetid: 264314BE-1C5C-4727-A14E-F6F98151CDBD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/19/2016
ms.openlocfilehash: 2b2b5726f4ca28ae37f027a700abdd688aa0b1d7
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108430"
---
# <a name="introduction-to-custom-renderers"></a>Einführung in benutzerdefinierte Renderer

_Benutzerdefinierte Renderer sind eine praktische Methode zum Anpassen der Darstellung und des Verhaltens von Xamarin.Forms-Steuerelementen. Sie können für geringfügige Formatierungsänderungen oder für umfangreiche, plattformspezifische Anpassungen des Layouts und Verhaltens verwendet werden. In diesem Artikel werden benutzerdefinierte Renderer vorgestellt, und Sie erfahren, wie Sie einen benutzerdefinierten Renderer erstellen können._

Die [Pages, Layouts and Controls (Seiten, Layouts und Steuerelemente)](~/xamarin-forms/user-interface/controls/index.md) von Xamarin.Forms stellen eine allgemeine API dar, mit der plattformübergreifende mobile Benutzeroberflächen beschrieben werden können. Die einzelnen Seiten, Layouts und Steuerelemente werden auf jeder Plattform auf unterschiedliche Weise über eine `Renderer`-Klasse gerendert. Diese erstellt ein natives Steuerelement (entsprechend der Xamarin.Forms-Darstellung), ordnet dieses auf dem Bildschirm an und fügt das im freigegebenen Code angegebene Verhalten hinzu.

Entwickler können ihre eigenen benutzerdefinierten `Renderer`-Klassen implementieren, um die Darstellung und/oder das Verhalten eines Steuerelements anzupassen. Benutzerdefinierte Renderer für einen bestimmten Typ können einem Anwendungsprojekt hinzugefügt werden, um das Steuerelement an einer Stelle anzupassen, während auf anderen Plattformen das Standardverhalten zugelassen wird. Alternativ können jedem Anwendungsprojekt unterschiedliche benutzerdefinierte Renderer hinzugefügt werden, um ein unterschiedliches Erscheinungsbild unter iOS, Android und auf der Universellen Windows-Plattform (UWP) zu schaffen. Das Implementieren einer benutzerdefinierten Rendererklasse zum Ausführen einer einfachen Anpassung eines Steuerelements ist jedoch oft eine komplizierte Antwort. Die Auswirkungen vereinfachen diesen Prozess und werden in der Regel für geringfügige Formatierungsänderungen verwendet. Weitere Informationen finden Sie unter [Effekte](~/xamarin-forms/app-fundamentals/effects/index.md).

## <a name="examining-why-custom-renderers-are-necessary"></a>Untersuchen, warum benutzerdefinierte Renderer erforderlich sind

Das Ändern der Darstellung eines Xamarin.Forms-Steuerelements ist ohne benutzerdefinierten Renderer ein zweistufiger Prozess, bei dem ein benutzerdefiniertes Steuerelement durch Erstellen einer Unterklasse erstellt und dann das benutzerdefinierte Steuerelement anstelle des ursprünglichen Steuerelements verwendet wird. Das folgende Codebeispiel veranschaulicht das Erstellen einer Unterklasse des `Entry`-Steuerelements beispielhaft:

```csharp
public class MyEntry : Entry
{
  public MyEntry ()
  {
    BackgroundColor = Color.Gray;
  }
}
```

Das `MyEntry`-Steuerelement ist ein `Entry`-Steuerelement, bei dem `BackgroundColor` auf Grau festgelegt ist und auf das in XAML verwiesen werden kann, indem Sie einen Namespace für seine Position deklarieren und das Namespacepräfix für das Steuerelement verwenden. Das folgende Codebeispiel veranschaulicht, wie das benutzerdefinierte Steuerelement `MyEntry` von einer `ContentPage` genutzt werden kann:

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

Das `local`-Namespacepräfix kann beliebig gewählt werden. Die Werte `namespace` und `assembly` müssen jedoch mit den Angaben des benutzerdefinierten Steuerelements übereinstimmen. Wenn der Namespace deklariert wurde, wird das Präfix verwendet, um auf das benutzerdefinierte Steuerelement zu verweisen.

> [!NOTE]
> Das Definieren des `xmlns` ist in .NET Standard-Bibliotheksprojekten viel einfacher als in freigegebenen Projekten. Eine .NET Standard-Bibliothek wird in eine Assembly kompiliert, sodass der Wert `assembly=CustomRenderer` einfach bestimmt werden kann. Beim Verwenden freigegebener Projekte werden alle freigegebenen Ressourcen (einschließlich der XAML) in jedes der verweisenden Projekte kompiliert. Wenn die iOS-, Android- und UWP-Projekte über eigene *Assemblynamen* verfügen, kann die `xmlns`-Deklaration nicht geschrieben werden, da der Wert für jede Anwendung unterschiedlich sein muss. Für benutzerdefinierte Steuerelemente in XAML für freigegebene Projekte muss jedes Anwendungsprojekt mit dem gleichen Assemblynamen konfiguriert werden.

Das benutzerdefinierte Steuerelement `MyEntry` wird dann auf jeder Plattform mit einem grauen Hintergrund gerendert, wie in den folgenden Screenshots gezeigt:

![](introduction-images/screenshots.png "Benutzerdefiniertes Steuerelement „MyEntry“ auf jeder Plattform")

Das Ändern der Hintergrundfarbe des Steuerelements auf jeder Plattform wurde ausschließlich über das Erstellen von Unterklassen für das Steuerelement erreicht. Die Leistung dieses Verfahrens ist jedoch eingeschränkt, da plattformspezifische Erweiterungen und Anpassungen nicht genutzt werden können. Wenn sie benötigt werden, müssen benutzerdefinierte Renderer implementiert werden.

## <a name="creating-a-custom-renderer-class"></a>Erstellen einer benutzerdefinierten Renderer-Klasse

Gehen Sie folgendermaßen vor, um eine Klasse für einen benutzerdefinierten Renderer zu erstellen:

1. Erstellen Sie eine Unterklasse der Renderer-Klasse, die das native Steuerelement rendert.
1. Überschreiben Sie die Methode, die das native Steuerelement rendert, und schreiben Sie Logik, um das Steuerelement anzupassen. Häufig wird die `OnElementChanged`-Methode verwendet, um das native Steuerelement zu rendern, das bei der Erstellung des entsprechenden Xamarin.Forms-Steuerelements aufgerufen wird.
1. Fügen Sie der Klasse des benutzerdefinierten Renderers ein `ExportRenderer`-Attribut hinzu, um anzugeben, dass sie zum Rendern des Xamarin.Forms-Steuerelements verwendet werden soll. Dieses Attribut wird verwendet, um den benutzerdefinierten Renderer bei Xamarin.Forms zu registrieren.

> [!NOTE]
> Bei den meisten Xamarin.Forms-Elementen ist das Angeben eines benutzerdefinierten Renderers in jedem Plattformprojekt optional. Wenn kein benutzerdefinierter Renderer registriert wurde, wird der Standardrenderer für die Basisklasse des Steuerelements verwendet. Benutzerdefinierte Renderer sind jedoch in jedem Plattformprojekt erforderlich, wenn ein [View](xref:Xamarin.Forms.View)- oder [ViewCell](xref:Xamarin.Forms.ViewCell)-Element gerendert wird.

Die Themen in dieser Reihe bieten Demos und Erklärungen dieses Prozesses für verschiedene Xamarin.Forms-Elemente.

## <a name="troubleshooting"></a>Problembehandlung

Wenn ein benutzerdefiniertes Steuerelement in einem .NET Standard-Bibliotheksprojekt enthalten ist, das der Projektmappe hinzugefügt wurde (d.h. nicht die .NET Standard-Bibliothek, die von der Visual Studio für Mac/Visual Studio-Xamarin.Forms-App-Projektvorlage erstellt wurde). Eine Ausnahme kann unter iOS auftreten, wenn versucht wird, auf das benutzerdefinierte Steuerelement zuzugreifen. Wenn dieses Problem auftritt, kann es durch Erstellen eines Verweises auf das benutzerdefinierte Steuerelement aus der Klasse `AppDelegate` gelöst werden:

```csharp
var temp = new ClassInPCL(); // in AppDelegate, but temp not used anywhere
```

Dies zwingt den Compiler, den Typ `ClassInPCL` zu erkennen, indem er aufgelöst wird. Alternativ kann das Attribut `Preserve` der Klasse `AppDelegate` hinzugefügt werden, um das gleiche Ergebnis zu erzielen:

```csharp
[assembly: Preserve (typeof (ClassInPCL))]
```

Dadurch wird ein Verweis auf den Typ `ClassInPCL` erstellt, der angibt, dass er zur Laufzeit benötigt wird. Weitere Informationen finden Sie unter [Beibehalten von Code](~/ios/deploy-test/linker.md).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden benutzerdefinierte Renderer vorgestellt, und Sie haben erfahren, wie Sie einen benutzerdefinierten Renderer erstellen können. Benutzerdefinierte Renderer sind eine praktische Methode zum Anpassen der Darstellung und des Verhaltens von Xamarin.Forms-Steuerelementen. Sie können für geringfügige Formatierungsänderungen oder für umfangreiche, plattformspezifische Anpassungen des Layouts und Verhaltens verwendet werden.


## <a name="related-links"></a>Verwandte Links

- [Effekte](~/xamarin-forms/app-fundamentals/effects/index.md)
