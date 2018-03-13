---
title: Layoutkomprimierung
description: Layout-Komprimierung entfernt angegebenen Layouts aus der visuellen Struktur, in dem Versuch zur Verbesserung der Leistung beim Rendern der Seite. Dieser Artikel beschreibt die Aktivierung der Komprimierung Layout und die Vorteile, die sie wiederherstellen kann.
ms.topic: article
ms.prod: xamarin
ms.assetid: da9e1b26-9d31-4762-94c3-4039f306b7f2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/13/2017
ms.openlocfilehash: e03acbbac737bffd21ee3b592ab017d227f822ad
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="layout-compression"></a>Layoutkomprimierung

_Layout-Komprimierung entfernt angegebenen Layouts aus der visuellen Struktur, in dem Versuch zur Verbesserung der Leistung beim Rendern der Seite. Dieser Artikel beschreibt die Aktivierung der Komprimierung Layout und die Vorteile, die sie wiederherstellen kann._

## <a name="overview"></a>Übersicht

Xamarin.Forms führt Layout mit zwei Reihen von rekursiven-Methodenaufrufen:

- Layout beginnt am oberen Rand der visuellen Struktur mit einer Seite, und es durchläuft alle Zweige der visuellen Struktur jedes visuelle Element auf einer Seite einnimmt. Elemente, die übergeordneten Objekte auf andere Elemente sind sind verantwortlich für die größenanpassung und Positionierung von ihren untergeordneten Elementen relativ zum selbst.
- Invalidierung wird mit dem eine Änderung in einem Element auf einer Seite eine neue Layoutzyklus auslöst. Elemente werden als ungültig betrachtet, wenn sie nicht mehr die richtige Größe oder Position aufweisen. Jedes Element in der visuellen Struktur, die über untergeordnete Elemente verfügt, wird gewarnt, bei jeder Änderung eines seiner untergeordneten Elemente Größen. Aus diesem Grund kann eine Änderung der Größe eines Elements in der visuellen Struktur Änderungen verursachen, die die Struktur ripple.

Weitere Informationen dazu, wie Xamarin.Forms Layout führt, finden Sie unter [Erstellen eines benutzerdefinierten Layouts](~/xamarin-forms/user-interface/layouts/custom.md).

Das Ergebnis im Layoutprozess ist eine Hierarchie von systemeigenen Steuerelementen. Diese Hierarchie enthält jedoch zusätzliche Container Renderern und Wrapper Plattform bei Renderern mit weiteren überhöhte der Hierarchie anzeigen schachteln. Je tiefer sich der Ebene der Schachtelung, desto größer der verbleibende Arbeitsaufwand, der Xamarin.Forms ausführen, damit eine Seite anzeigen muss. Für komplexe Layouts kann die Hierarchie anzeigen Tiefe und Breite, mit mehreren Schachtelungsebenen dargestellt werden.

Betrachten Sie beispielsweise die folgende Schaltfläche aus der beispielanwendung für die Anmeldung bei Facebook aus:

![](layout-compression-images/facebook-button.png "Facebook-Schaltfläche")

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

Die resultierende Hierarchie für eine geschachtelte Ansicht mit untersucht werden kann [Xamarin Inspektor](~/tools/inspector/index.md). Auf Android-Geräten enthält die Hierarchie, eine geschachtelte Ansicht 17 Ansichten:

![](layout-compression-images/no-compression.png "Ansichtshierarchie für Facebook-Schaltfläche")

Layout-Komprimierung für Xamarin.Forms-Anwendungen auf IOS- und Android-Plattform verfügbar ist, zielt darauf ab, so vereinfachen Sie die Ansicht durch das Entfernen der angegebenen Layouts aus der visuellen Struktur, die Seitenrendering verbessert schachteln. Die Leistungsvorteile, die übermittelt wird, variiert abhängig von der Komplexität einer Seite, die Version des Betriebssystems verwendet wird und das Gerät, auf dem die Anwendung ausgeführt wird. Die größten Leistungssteigerungen werden jedoch bei älteren Geräten zu verzeichnen sein.

> [!NOTE]
> Während auf die Ergebnisse des Anwendens von Layout-Komprimierung auf Android dieser Artikel konzentriert sich, ist es gleichermaßen anwendbar für iOS.

## <a name="layout-compression"></a>Layoutkomprimierung

In XAML kann Layout Komprimierung aktiviert werden durch Festlegen der `CompressedLayout.IsHeadless` angefügten Eigenschaft, um `true` für eine Layoutklasse:

```xaml
<StackLayout CompressedLayout.IsHeadless="true">
  ...
</StackLayout>   
```

Alternativ können sie in c# aktiviert werden, durch Angeben der Layout-Instanz als erstes Argument für die `CompressedLayout.SetIsHeadless` Methode:

```csharp
CompressedLayout.SetIsHeadless(stackLayout, true);
```

> [!IMPORTANT]
> Da Layout Komprimierung ein Layout aus der visuellen Struktur entfernt werden, ist es nicht für die Layouts, die eine visuelle Darstellung aufweisen oder erhalten, Fingereingabe, geeignet ist. Aus diesem Grund Layouts festgelegt [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) Eigenschaften (z. B. [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/), [ `IsVisible` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsVisible/), [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/), [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/), [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/) und [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/)) bzw. an deren Gesten akzeptieren, sind keine Kandidaten für Layout Komprimierung. Allerdings führt Aktivierung der Layout-Komprimierung für ein Layout, die visuelle Darstellungseigenschaften festlegt oder Gesten, nimmt, nicht zu einem Build oder Common Language Runtime-Fehler. Stattdessen Layout-Komprimierung wird angewendet, und visual Darstellungseigenschaften und Gestenhandler Deaktivierung werden ohne Meldung fehl.

Layout Komprimierung kann für die Facebook-Schaltfläche die drei Layout Klassen aktiviert werden:

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

Auf Android-führt dies in einer Hierarchie eine geschachtelte Ansicht 14 Ansichten:

![](layout-compression-images/layout-compression.png "Ansichtshierarchie für Facebook-Schaltfläche mit Layout-Komprimierung")

Im Vergleich zu der ursprünglichen eine geschachtelte Ansichtshierarchie 17 Ansichten, stellt dies eine Reduzierung der Anzahl von Ansichten von 17 % dar. Und durch diese Reduzierung der nicht signifikante erscheinen, kann die Ansicht Verringerung über eine gesamte Seite mehr erheblich sein.

### <a name="fast-renderers"></a>Schnelle Renderer

Schnelle Renderer reduzieren die wiederverwendungen und Rendering Kosten Xamarin.Forms Steuerelemente unter Android, indem Sie die resultierende Ansicht mit systemeigenen-Hierarchie vereinfachen. Dies verbessert die Leistung weiter durch das Erstellen von weniger Objekte, wodurch wiederum in einer weniger komplexen visuellen Struktur und weniger Arbeitsspeicher verwenden. Weitere Informationen zu schnellen Renderern finden Sie unter [schnelle Renderer](~/xamarin-forms/internals/fast-renderers.md).

Für die Facebook-Schaltfläche in der beispielanwendung erzeugt das Kombinieren von Layout Komprimierung und schnellen Renderer eine Hierarchie eine geschachtelte Ansicht 8 Ansichten:

![](layout-compression-images/layout-compression-with-fast-renderers.png "Hierarchie für Facebook-Schaltfläche mit Layout-Komprimierung und schnellen Renderer anzeigen")

Im Vergleich zu der ursprünglichen eine geschachtelte Ansichtshierarchie 17 Ansichten, stellt dies eine Verringerung der 52 % dar.

Die beispielanwendung enthält eine Seite aus einer realen Anwendung extrahiert. Ohne Komprimierung Layout und schnellen Renderer erzeugt die Seite eine geschachtelte Ansicht-Hierarchie von 130 Ansichten auf Android-Geräten. Schnelle Renderern und Layout-Komprimierung für die entsprechenden Layout Klassen aktivieren wird die Hierarchie eine geschachtelte Ansicht mit 70 Ansichten, eine Reduzierung der 46 % reduziert.

## <a name="summary"></a>Zusammenfassung

Layout-Komprimierung entfernt angegebenen Layouts aus der visuellen Struktur, in dem Versuch zur Verbesserung der Leistung beim Rendern der Seite. Der daraus resultierende Leistungsvorteil variiert je nach Komplexität einer Seite, der Version des verwendeten Betriebssystems und des Geräts, auf dem die Anwendung ausgeführt wird. Die größten Leistungssteigerungen werden jedoch bei älteren Geräten zu verzeichnen sein.


## <a name="related-links"></a>Verwandte Links

- [Erstellen eines benutzerdefinierten Layouts](~/xamarin-forms/user-interface/layouts/custom.md)
- [Schnelle Renderer](~/xamarin-forms/internals/fast-renderers.md)
- [LayoutCompression (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/layoutcompression/)
