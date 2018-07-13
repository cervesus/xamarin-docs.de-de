---
title: Layoutkomprimierung
description: Layoutkomprimierung werden bestimmte Layouts aus der visuellen Struktur Versuch zur Verbesserung der Leistung beim Rendern der Seite entfernt. In diesem Artikel erläutert das Aktivieren der layoutkomprimierung und die Vorteile, die sie bieten kann.
ms.prod: xamarin
ms.assetid: da9e1b26-9d31-4762-94c3-4039f306b7f2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/13/2017
ms.openlocfilehash: ba9be51daa32be1034e2bdfafafe80c45d00d83c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995231"
---
# <a name="layout-compression"></a>Layoutkomprimierung

_Layoutkomprimierung werden bestimmte Layouts aus der visuellen Struktur Versuch zur Verbesserung der Leistung beim Rendern der Seite entfernt. In diesem Artikel erläutert das Aktivieren der layoutkomprimierung und die Vorteile, die sie bieten kann._

## <a name="overview"></a>Übersicht

Xamarin.Forms führt Layout mithilfe von zwei Reihen von rekursiven-Methodenaufrufen:

- Layout beginnt am oberen Rand der visuellen Struktur mit einer Seite, und es durchläuft alle Verzweigungen der visuellen Struktur so, dass jedes visuelle Element auf einer Seite eingeschlossen. Elemente, die übergeordneten Elemente für andere Elemente sind für die größenanpassung und Positionierung von deren untergeordnete Elemente relativ zu sich selbst verantwortlich.
- Aufhebung der Gültigkeit wird mit dem eine Änderung in einem Element auf einer Seite ein neues Layoutzyklus auslöst. Elemente werden als ungültig betrachtet, wenn sie nicht mehr die richtige Größe oder Position verfügen. Jedes Element in der visuellen Struktur, die über untergeordnete Elemente verfügt wird gewarnt, wenn eines der untergeordneten Größe ändert. Aus diesem Grund kann eine Änderung der Größe eines Elements in der visuellen Struktur Änderungen verursachen, die der Struktur aufwärts ripple.

Weitere Informationen dazu, wie Xamarin.Forms Layouts ausführt, finden Sie unter [Erstellen eines benutzerdefinierten Layouts](~/xamarin-forms/user-interface/layouts/custom.md).

Das Ergebnis im Layoutprozess ist eine Hierarchie von systemeigenen Steuerelementen. Diese Hierarchie enthält jedoch zusätzlichen Container-Renderer und der Wrapper für die Plattform Renderern jedoch weiter die Hierarchie von Inhaltsansichten schachteln. Je tiefer die Ebene der Schachtelung, desto größer der Umfang der Arbeit, die Xamarin.Forms ausführen, um eine Seite angezeigt. Für komplexe Layouts kann die Hierarchie von Inhaltsansichten Tiefe und Breite, mit mehreren Schachtelungsebenen sein.

Betrachten Sie beispielsweise die folgende Schaltfläche aus der beispielanwendung für die Anmeldung bei Facebook aus:

![](layout-compression-images/facebook-button.png "Schaltfläche \"Facebook\"")

Diese Schaltfläche wird als ein benutzerdefiniertes Steuerelement mit der folgenden XAML-Ansicht-Hierarchie angegeben:

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

Die resultierende Hierarchie für eine geschachtelte Ansicht mit untersucht werden kann, [Xamarin Inspector](~/tools/inspector/index.md). Unter Android wird enthält die Hierarchie, eine geschachtelte Ansicht 17 Ansichten:

![](layout-compression-images/no-compression.png "Hierarchie von Inhaltsansichten für Facebook-Schaltfläche")

Layoutkomprimierung, die für Xamarin.Forms-Anwendungen auf IOS- und Android-Plattformen verfügbar ist, zielt darauf ab, so vereinfachen Sie die Ansicht schachteln, die durch das Entfernen des angegebenen Layouts aus der visuellen Struktur, die Seite-Rendering-Leistung verbessern kann. Der Leistungsvorteil, der bereitgestellt wird, variiert abhängig von der Komplexität einer Seite, die Version des Betriebssystems verwendet wird und das Gerät, auf dem die Anwendung ausgeführt wird. Die größten Leistungssteigerungen werden jedoch bei älteren Geräten zu verzeichnen sein.

> [!NOTE]
> Während in diesem Artikel auf die Ergebnisse der Anwendung layoutkomprimierung unter Android ausgerichtet ist, ist es gleichermaßen anwendbar für iOS.

## <a name="layout-compression"></a>Layoutkomprimierung

In XAML, layoutkomprimierung aktiviert werden kann, indem die `CompressedLayout.IsHeadless` angefügte Eigenschaft zu `true` für eine Layoutklasse:

```xaml
<StackLayout CompressedLayout.IsHeadless="true">
  ...
</StackLayout>   
```

Alternativ kann diese in C# -Code aktiviert werden, durch die Angabe der Layout-Instanz als das erste Argument für die `CompressedLayout.SetIsHeadless` Methode:

```csharp
CompressedLayout.SetIsHeadless(stackLayout, true);
```

> [!IMPORTANT]
> Da layoutkomprimierung ein Layout aus der visuellen Struktur entfernt, ist es nicht geeignet für Layouts, die eine visuelle Darstellung aufweisen, oder, die Touch-Punkts erhalten. Aus diesem Grund, die Layouts festgelegt [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) Eigenschaften (z. B. [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor), [ `IsVisible` ](xref:Xamarin.Forms.VisualElement.IsVisible), [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation), [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale), [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX) und [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) oder die Gesten akzeptieren, sind keine Kandidaten für Layout Komprimierung. Allerdings führt Aktivieren der layoutkomprimierung auf ein Layout, visuelle Darstellungseigenschaften festlegt, oder, Bewegungen akzeptiert, nicht zu einem Build- oder Fehler. Stattdessen layoutkomprimierung werden angewendet, und visuelle Darstellung von Eigenschaften und gestenerkennung, im Hintergrund fehl.

Für die Facebook-Schaltfläche kann die layoutkomprimierung für die drei Layout Klassen aktiviert werden:

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

Unter Android führt dies in einer Hierarchie eine geschachtelte Ansicht von 14 Ansichten:

![](layout-compression-images/layout-compression.png "Hierarchie von Inhaltsansichten für Facebook-Schaltfläche mit Layoutkomprimierung")

Im Vergleich zu der ursprünglichen eine geschachtelte Ansichtshierarchie 17 Ansichten, stellt dies eine Reduzierung der Anzahl von Ansichten der 17 % dar. Und diese Reduzierung unbedeutend erscheinen, kann die Verringerung der Ansicht über eine gesamte Seite noch folgenreicher sein.

### <a name="fast-renderers"></a>Schnelle Renderer

Schnelle Renderer reduzieren die Inflations- und renderingkosten von Xamarin.Forms-Steuerelementen in Android, indem Sie die resultierende native Hierarchie vereinfachen. Dies verbessert die Leistung weiter durch das Erstellen von weniger Objekte, was wiederum in einer weniger komplexen visuellen Struktur und weniger speicherauslastung führt. Weitere Informationen zu schnellen Renderern finden Sie unter [schnelle Renderer](~/xamarin-forms/internals/fast-renderers.md).

Für die Facebook-Schaltfläche in der beispielanwendung erzeugt das Kombinieren von layoutkomprimierung und schnelle Renderer eine Hierarchie eine geschachtelte Ansicht von 8 wechseln:

![](layout-compression-images/layout-compression-with-fast-renderers.png "Hierarchie von Inhaltsansichten für Facebook-Schaltfläche mit Layoutkomprimierung und schnelle Renderer")

Im Vergleich zu der ursprünglichen eine geschachtelte Ansichtshierarchie 17 Ansichten, stellt dies eine Reduzierung der 52 % dar.

Die beispielanwendung enthält eine Seite aus einer echten Anwendung extrahiert. Ohne layoutkomprimierung und schnelle Renderer erzeugt die Seite eine Hierarchie eine geschachtelte Ansicht von 130 Ansichten unter Android. Aktivieren von schnellen Renderern und layoutkomprimierung auf richtige Layout Klassen wird die geschachtelte Hierarchie zu 70 Ansichten, eine Verringerung der 46 % reduziert.

## <a name="summary"></a>Zusammenfassung

Layoutkomprimierung werden bestimmte Layouts aus der visuellen Struktur Versuch zur Verbesserung der Leistung beim Rendern der Seite entfernt. Der daraus resultierende Leistungsvorteil variiert je nach Komplexität einer Seite, der Version des verwendeten Betriebssystems und des Geräts, auf dem die Anwendung ausgeführt wird. Die größten Leistungssteigerungen werden jedoch bei älteren Geräten zu verzeichnen sein.


## <a name="related-links"></a>Verwandte Links

- [Erstellen eines benutzerdefinierten Layouts](~/xamarin-forms/user-interface/layouts/custom.md)
- [Schnelle Renderer](~/xamarin-forms/internals/fast-renderers.md)
- [LayoutCompression (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/layoutcompression/)
