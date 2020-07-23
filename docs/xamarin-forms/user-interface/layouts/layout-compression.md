---
title: Layoutkomprimierung
description: Bei der layoutkomprimierung werden angegebene Layouts aus der visuellen Struktur entfernt, um die Seiten Rendering-Leistung zu verbessern. In diesem Artikel wird erläutert, wie Sie die layoutkomprimierung und die damit verbundenen Vorteile aktivieren können.
ms.prod: xamarin
ms.assetid: da9e1b26-9d31-4762-94c3-4039f306b7f2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/13/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5fa9c7592ecd2cb314ce12d7e303677447a5e104
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86931169"
---
# <a name="layout-compression"></a>Layoutkomprimierung

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layoutcompression)

_Bei der layoutkomprimierung werden angegebene Layouts aus der visuellen Struktur entfernt, um die Seiten Rendering-Leistung zu verbessern. In diesem Artikel wird erläutert, wie Sie die layoutkomprimierung und die damit verbundenen Vorteile aktivieren können._

## <a name="overview"></a>Übersicht

Xamarin.Formsführt das Layout mit zwei Reihen von rekursiven Methoden aufrufen aus:

- Layout beginnt am oberen Rand der visuellen Struktur mit einer Seite und durchläuft alle Verzweigungen der visuellen Struktur, um alle visuellen Elemente auf einer Seite zu umfassen. Elemente, die übergeordnete Elemente anderer Elemente sind, sind für die Größenanpassung und Positionierung ihrer untergeordneten Elemente in Bezug auf sich verantwortlich.
- Die Invalidierung ist der Prozess, durch den eine Änderung in einem Element auf einer Seite einen neuen layoutcycle auslöst. Elemente werden als ungültig betrachtet, wenn Sie nicht mehr über die richtige Größe oder Position verfügen. Jedes Element in der visuellen Struktur mit untergeordneten Elementen wird immer dann benachrichtigt, wenn eines der untergeordneten Elemente seine Größe ändert. Daher kann eine Änderung der Größe eines Elements in der visuellen Strukturänderungen verursachen, die die Struktur aufschlagen.

Weitere Informationen zur Funktionsweise von Xamarin.Forms Layout finden Sie unter [Erstellen eines benutzerdefinierten Layouts](~/xamarin-forms/user-interface/layouts/custom.md).

Das Ergebnis des Layoutprozesses ist eine Hierarchie von systemeigenen Steuerelementen. Diese Hierarchie enthält jedoch zusätzliche Container-Renderer und Wrapper für Platt Form Renderer, die das Schachteln der Ansichts Hierarchie weiter erhöhen. Umso tiefer die Schachtelungs Ebene, desto größer ist der Arbeitsaufwand, der Xamarin.Forms zum Anzeigen einer Seite ausgeführt werden muss. Bei komplexen Layouts kann die Ansichts Hierarchie tief und breit sein und über mehrere Schachtelungs Ebenen verfügen.

Sehen Sie sich beispielsweise die folgende Schaltfläche aus der Beispielanwendung für die Anmeldung bei Facebook an:

![Facebook-Schaltfläche](layout-compression-images/facebook-button.png)

Diese Schaltfläche wird als benutzerdefiniertes Steuerelement mit der folgenden XAML-Ansichts Hierarchie angegeben:

```xaml
<ContentView ...>
    <StackLayout>
        <StackLayout ...>
            <AbsoluteLayout ...>
                <Button ... />    
                <Image ... />
                <Image ... />
                <BoxView ... />
                <Label ... />
                <Button ... />
            </AbsoluteLayout>
        </StackLayout>
        <Label ... />
    </StackLayout>    
</ContentView>
```

Die resultierende Struktur der Hierarchie-Sicht kann mit [Xamarin Inspector](~/tools/inspector/index.md)untersucht werden. Unter Android enthält die Hierarchie der Hierarchie der Hierarchie 17 Ansichten:

![Hierarchie für Facebook-Schaltfläche anzeigen](layout-compression-images/no-compression.png)

Die layoutkomprimierung, die für Xamarin.Forms Anwendungen auf den IOS-und Android-Plattformen verfügbar ist, zielt darauf ab, die Schachtelung der Ansicht zu vereinfachen, indem angegebene Layouts aus der visuellen Struktur entfernt werden, wodurch die Seiten Rendering verbessert werden kann Der Leistungsvorteil hängt von der Komplexität einer Seite, der Version des verwendeten Betriebssystems und dem Gerät ab, auf dem die Anwendung ausgeführt wird. Die größten Leistungssteigerungen werden jedoch bei älteren Geräten zu verzeichnen sein.

> [!NOTE]
> Obwohl sich dieser Artikel auf die Ergebnisse der Anwendung der layoutkomprimierung unter Android konzentriert, ist er gleichermaßen auf IOS anwendbar.

## <a name="layout-compression"></a>Layoutkomprimierung

In XAML kann die layoutkomprimierung aktiviert werden, indem die `CompressedLayout.IsHeadless` angefügte-Eigenschaft für `true` eine Layoutklasse auf festgelegt wird:

```xaml
<StackLayout CompressedLayout.IsHeadless="true">
  ...
</StackLayout>   
```

Alternativ kann Sie in c# aktiviert werden, indem die layoutanstanz als erstes Argument der-Methode angegeben wird `CompressedLayout.SetIsHeadless` :

```csharp
CompressedLayout.SetIsHeadless(stackLayout, true);
```

> [!IMPORTANT]
> Da bei der layoutkomprimierung ein Layout aus der visuellen Struktur entfernt wird, eignet es sich nicht für Layouts, die eine visuelle Darstellung haben oder die Berührungs Eingaben erhalten. Daher sind Layouts, die [`VisualElement`](xref:Xamarin.Forms.VisualElement) Eigenschaften festlegen (z [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) . b., [`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible) , [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) , [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) , [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) und [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) oder, die Gesten akzeptieren, keine Kandidaten für die layoutkomprimierung. Das Aktivieren der layoutkomprimierung für ein Layout, das Eigenschaften visueller Darstellungen festlegt, oder die Gesten akzeptiert, führt jedoch nicht zu einem Build-oder Laufzeitfehler. Stattdessen wird die layoutkomprimierung angewendet, und die Eigenschaften der visuellen Darstellung und die Gestenerkennung schlagen im Hintergrund fehl.

Für die Facebook-Schaltfläche kann die layoutkomprimierung in den drei layoutklassen aktiviert werden:

```xaml
<StackLayout CompressedLayout.IsHeadless="true">
    <StackLayout CompressedLayout.IsHeadless="true" ...>
        <AbsoluteLayout CompressedLayout.IsHeadless="true" ...>
            ...
        </AbsoluteLayout>
    </StackLayout>
    ...
</StackLayout>  
```

Unter Android führt dies zu einer Hierarchie der Hierarchie mit einer Hierarchie von 14 Ansichten:

![Hierarchie für Facebook-Schaltfläche mit layoutkomprimierung](layout-compression-images/layout-compression.png)

Im Vergleich zur ursprünglichen Hierarchie der Hierarchie mit der ursprünglichen Struktur von 17 Sichten stellt dies eine Reduzierung der Anzahl von Sichten von 17% dar. Diese Reduzierung mag unbedeutend erscheinen, aber die Sicht Reduzierung auf eine gesamte Seite kann signifikanter sein.

### <a name="fast-renderers"></a>Schnelle Renderer

Schnelle Renderer verringern die Inflations-und renderingkosten von Steuer Xamarin.Forms Elementen auf Android, indem die resultierende Native Ansichts Hierarchie vereinfacht wird. Dies verbessert die Leistung, indem weniger Objekte erstellt werden, was wiederum zu einer weniger komplexen visuellen Struktur und weniger Arbeitsspeicher Auslastung führt. Weitere Informationen zu schnellen rendererarbeitern finden Sie unter [schnelle Renderer](~/xamarin-forms/internals/fast-renderers.md).

Für die Facebook-Schaltfläche in der Beispielanwendung erzeugt die Kombination der layoutkomprimierung und der schnellen Renderer eine Schaltfläche für die Hierarchie von 8 Ansichten:

![Hierarchie für Facebook-Schaltfläche mit layoutkomprimierung und schnellen renderatoren anzeigen](layout-compression-images/layout-compression-with-fast-renderers.png)

Im Vergleich zur ursprünglichen Hierarchie der Hierarchie mit der ursprünglichen Struktur von 17 Sichten stellt dies eine Reduzierung von 52% dar.

Die Beispielanwendung enthält eine Seite, die aus einer realen Anwendung extrahiert wurde. Ohne layoutkomprimierung und schnelle Renderer erzeugt die Seite eine Hierarchie der Hierarchie mit einer Hierarchie von 130 in Android. Durch die Aktivierung schneller Renderer und layoutkomprimierung für geeignete layoutklassen wird die Hierarchie der geschachtelte View auf 70 Sichten reduziert, was zu einer Verringerung von 46% führt.

## <a name="summary"></a>Zusammenfassung

Bei der layoutkomprimierung werden angegebene Layouts aus der visuellen Struktur entfernt, um die Seiten Rendering-Leistung zu verbessern. Der daraus resultierende Leistungsvorteil variiert je nach Komplexität einer Seite, der Version des verwendeten Betriebssystems und des Geräts, auf dem die Anwendung ausgeführt wird. Die größten Leistungssteigerungen werden jedoch bei älteren Geräten zu verzeichnen sein.

## <a name="related-links"></a>Ähnliche Themen

- [Erstellen eines benutzerdefinierten Layouts](~/xamarin-forms/user-interface/layouts/custom.md)
- [Schnelle Renderer](~/xamarin-forms/internals/fast-renderers.md)
- [Layoutcompression (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layoutcompression)
