---
title: Arbeiten mit TvOS segmentierte Steuerelemente in Xamarin
description: Dieses Dokument beschreibt das Arbeiten mit TvOS segmentierte Steuerelemente in einer app mit Xamarin erstellt wurde. Es wird erläutert, Segment-Symbole und Text, Ereignisse, ändern die Darstellung des Steuerelements und vieles mehr.
ms.prod: xamarin
ms.assetid: 23AD94CC-E93A-40B1-8E2B-ECD21FA355BE
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 98a770d05014e0498b805ed9ffa0c84314efc765
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50107116"
---
# <a name="working-with-tvos-segmented-controls-in-xamarin"></a>Arbeiten mit TvOS segmentierte Steuerelemente in Xamarin

Ein segmentiertes Steuerelement bietet einen Satz von linearen Elementen, von die jedes ein Symbol oder Text enthalten kann, und wird verwendet, um einen Satz von verwandten Optionen für dem Benutzer bereitstellen.

[![](segmented-controls-images/segment01.png "Beispiel-Segment-Steuerelemente")](segmented-controls-images/segment01.png#lightbox)

Apple hat die folgenden Vorschläge für die Arbeit mit segmentierte Steuerelemente:

- **Geben Sie genügend Speicherplatz** -Sorgfalt muss brauchen, um die bietet somit viel Platz zwischen anderen [Fokussierbare Elemente](~/ios/tvos/app-fundamentals/navigation-focus.md) und ein segmentiertes Steuerelement. Einem einzelnen Segment wird ausgewählt, wenn es in den Fokus hat (nicht, wenn darauf geklickt wird) und der Benutzer die Segmente versehentlich ändern kann, wenn sie tatsächlich ein anderes fokussierbares Element auf das aktuelle Segment auswählen möchten.
- **Verwenden Sie geteilten Ansichten für das Filtern von Inhalt** -segmentierte Steuerelemente nicht die stellen eine gute Wahl für die inhaltsfilterung wie geteilten Ansichten für einfache Navigation zwischen dem Inhalt und die Filter entwickelt wurden.
- **Beschränkung für sieben Segmente Max** -sollten Sie versuchen, behalten Sie die maximalen Anzahl von Segmenten unter acht (8) als dies ist einfacher, Analysieren von durch das Zimmer auf dem Sofa und einfacher zu navigieren.
- **Konsistente Größe des Segments Inhalt verwenden** – alle Segmente verfügen über die gleiche Breite und, falls möglich, sollten Sie versuchen, dem Inhalt in jedem Segment verwenden die gleiche Größe behalten. Dies macht die Segment-Steuerelemente visuell ansprechende klassifizierungsvorgänge erleichtert es, einen Blick zu lesen.
- **Vermeiden Sie das Kombinieren von Symbolen und Text** -jedes einzelnen Segment kann entweder ein Symbol oder Text, jedoch nicht beides enthalten. Es ist, zwar möglich, sowohl Symbole und Text in einem segmentierten Steuerelement zu kombinieren sollte dies vermieden werden.

<a name="About-Segment-Icons" />

## <a name="about-segment-icons"></a>Informationen zu den Segment-Symbole

Apple empfiehlt das Verwenden von einfachen, erkennbaren clientimages für Segment-Symbole, z. B. einer Lupe für die Suche. Kompliziert, als Symbole sind schwer zu erkennen auf einem TV-Bildschirm, über den Raum, daher wird empfohlen, um die Symbole in einfache Darstellungen zu beschränken.

Sowohl Text als auch Symbole auf ein bestimmtes Segment können nicht gemischt werden, und vermeiden Sie das Kombinieren von Symbolen und Text in einem einzelnen segmentierten Steuerelement. Es muss entweder alle Symbole oder Text.

<a name="Summary" />

## <a name="segment-text"></a>Segment-Text

Apple stellt die folgenden Vorschläge für die Arbeit mit der Segment-Text:

- **Verwenden Sie kurze, aussagekräftige Nomen** : das Segment Titel sollte den Typ des Inhalts, die der Benutzer erwarten soll, wenn das angegebene Segment auswählen eindeutig angeben. Zum Beispiel: Musik oder Videos.
- **Verwenden Sie die Groß-/Kleinschreibung große Anfangsbuchstaben** -jedes Wort des Titels Segmente sollten groß geschrieben werden, mit Ausnahme von Artikeln, Konjunktionen und Präpositionen von weniger als vier (4) Zeichen.
- **Verwenden Sie kurze, konzentriert sich Titel** -behalten Sie den Titel, kurze und konzentriert sich auf den Typ des Inhalts zu erwarten, wenn das Segment ausgewählt ist.

In diesem Fall können nicht gemischt werden sowohl Text als auch Symbole auf ein bestimmtes Segment, und vermeiden Sie das Kombinieren von Symbolen und Text in einem einzelnen segmentierten Steuerelement.

<a name="Segment-Controls-and-Storyboards" />

## <a name="segment-controls-and-storyboards"></a>Segment-Steuerelemente und Storyboards

Die einfachste Möglichkeit zum Arbeiten mit der Segment-Steuerelemente in einer Xamarin.tvOS-app ist so fügen sie der app Benutzeroberfläche, die Verwendung des iOS Designers hinzu.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. In der **Lösungspad**, doppelklicken Sie auf die `Main.storyboard` und öffnen Sie sie für die Bearbeitung.
1. Ziehen Sie eine **Segment-Steuerelement** aus der **Toolbox** und legen Sie sie in der Ansicht: 

    [![](segmented-controls-images/segment02.png "Ein Segment-Steuerelement")](segmented-controls-images/segment02.png#lightbox)
1. In der **Widget Registerkarte** von der **Pad "Eigenschaft"**, können Sie verschiedene Eigenschaften des Steuerelements Segment wie z. B. Anpassen der **Stil** und **Zustand**: 

    [![](segmented-controls-images/segment03.png "Die Registerkarte \"Widget\"")](segmented-controls-images/segment03.png#lightbox)
1. Verwenden der **Segmente** Feld zum Steuern der Anzahl der Segmente im Controller.
1. Wählen Sie ein bestimmtes Segment aus der **Segment Dropdownliste** ihre individuellen Eigenschaften anpassen, wie z. B. **Titel** oder **Image** und steuern, ob ein bestimmtes Segment ist  **Aktiviert** oder **ausgewählte** Wenn das Steuerelement angezeigt wird.
1. Weisen Sie schließlich **Namen** auf die Steuerelemente, damit Sie auf diese Dateien im reagieren können C# Code. Zum Beispiel: 

    [![](segmented-controls-images/segment04.png "Weisen Sie einen Namen")](segmented-controls-images/segment04.png#lightbox)
1. Speichern Sie die Änderungen.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)
    
1. In der **Projektmappen-Explorer**, doppelklicken Sie auf die `Main.storyboard` und öffnen Sie sie für die Bearbeitung.
1. Ziehen Sie eine **Segment-Steuerelement** aus der **Toolbox** und legen Sie sie in der Ansicht: 

    [![](segmented-controls-images/segment02-vs.png "Ein Segment-Steuerelement")](segmented-controls-images/segment02-vs.png#lightbox)
1. In der **Widget Registerkarte** von der **Eigenschaften-Explorer**, können Sie verschiedene Eigenschaften des Steuerelements Segment wie z. B. Anpassen der **Stil** und **Zustand**: 

    [![](segmented-controls-images/segment03-vs.png "Die Registerkarte \"Widget\"")](segmented-controls-images/segment03-vs.png#lightbox)
1. Verwenden der **Segmente** Feld zum Steuern der Anzahl der Segmente im Controller.
1. Wählen Sie ein bestimmtes Segment aus der **Segment Dropdownliste** ihre individuellen Eigenschaften anpassen, wie z. B. **Titel** oder **Image** und steuern, ob ein bestimmtes Segment ist  **Aktiviert** oder **ausgewählte** Wenn das Steuerelement angezeigt wird.
1. Weisen Sie schließlich **Namen** auf die Steuerelemente, damit Sie auf diese Dateien im reagieren können C# Code. Zum Beispiel: 

    [![](segmented-controls-images/segment04-vs.png "Weisen Sie einen Namen")](segmented-controls-images/segment04-vs.png#lightbox)
1. Speichern Sie die Änderungen.
    
-----

Weitere Informationen zum Arbeiten mit Storyboards, informieren Sie sich unsere [Hello, TvOS – Kurzanleitung](~/ios/tvos/get-started/hello-tvos.md). 

<a name="Working-with-Segmented-Controls" />

## <a name="working-with-segmented-controls"></a>Arbeiten mit segmentierte Steuerelemente

Wie oben, s angegeben, dass ein segmentiertes Steuerelement über einen Satz von linearen Elemente bereitstellt, von denen jeder kann ein Symbol oder Text enthalten und wird verwendet, um einen Satz von verwandten Optionen für dem Benutzer bereitstellen.

Es gibt mehrere Möglichkeiten, die Sie in Ihrer app Xamarin.tvOS segmentierte Steuerelemente verwenden können.

<a name="Exposed-as-Outlets-and-Actions" />

## <a name="exposed-as-names-and-events"></a>Als Namen und Ereignissen verfügbar gemacht werden

Wenn Sie es als ein Steuerelement mit dem Namen und ein Ereignis verfügbar gemacht und das Segment-Steuerelement im Benutzeroberflächen-Designer erstellt können Sie den folgenden Code, um auf das Segment ändern zu reagieren:

```csharp
partial void PlayerCountChanged (Foundation.NSObject sender) {

    // Take action based on the number of players selected
    switch(PlayerCount.SelectedSegment) {
    case 0:
        // Do something if the segment is selected
        ...
        break;
    case 1:
        // Do something if the segment is selected
        ...
        break;
    case 2:
        // Do something if the segment is selected
        ...
        break;
    }
}
```

Im Fall der obigen Beispiel wurde die Segment-Steuerelement als verfügbar gemacht eine `PlayerCount` Namen und eine `PlayerCountChanged` Ereignisaktion. Weitere Informationen zum Arbeiten mit Aktionen und Ergebnisdaten finden Sie unter den [Schreiben von Code mit Outlets und Aktionen](~/ios/tvos/get-started/hello-tvos.md#Writing-the-Code) Teil unserer [Hello, TvOS – Kurzanleitung](~/ios/tvos/get-started/hello-tvos.md).

Die `SelectedSegment` -Eigenschaft ruft ab oder legt den Index des aktuell ausgewählten Segment als null (0), die basierend. Wenn also Sie fünf (5) Segmente verfügen, das erste Segment einen Index von 0 (null) und der letzte Index vier (4).

<a name="Modifying-Segments" />

## <a name="modifying-segments"></a>Ändern von Segmenten

Zu einem beliebigen Zeitpunkt können Sie die Anzahl und den Inhalt der segmentierte Steuerelemente ändern. Verwenden Sie den folgenden Code, um ein neues Symbol Segment einzufügen:

```csharp
// Icon Segment
SegmentedControl.InsertSegment(UIImage.FromFile("icon.png"), 0, true);

// Text Segment
SegmentedControl.InsertSegment("New Segment", 0, true);
```

Der zweite Parameter definiert, in dem das Segment mit der (eine Null 0)-basierter Index eingefügt. Wenn der letzte Parameter ist `true` Insert animiert wird.

Um ein bestimmtes Segment zu entfernen, verwenden Sie Folgendes ein:

```csharp
SegmentedControl.RemoveSegmentAtIndex(0, true);
```

Oder So entfernen Sie alle Segmente Folgendes:

```csharp
SegmentedControl.RemoveAllSegments();
```

In diesem Fall ist der letzte Parameter `true`, die Entfernung animiert wird. Verwenden der `NumberOfSegments` Eigenschaft, die die aktuelle Anzahl von Segmenten zurückgegeben.

Zum Abrufen der **Titel** oder **Symbol** für ein bestimmtes Segment, verwenden Sie Folgendes:

```csharp
// Get title
var title = SegmentedControl.TitleAt(0);

// Get icon
var icon = SegmentedControl.ImageAt(0);
```

Und so ändern Sie die **Titel** oder **Symbol**, verwenden Sie Folgendes:

```csharp
// Set title
SegmentedControl.SetTitle("New Title", 0);

// Set icon
SegmentedControl.SetImage(UIImage.FromFile("icon.png"), 0);
```

Um festzustellen, ob ein bestimmtes Segment ist **aktiviert**, verwenden Sie Folgendes:

```csharp
if (SegmentedControl.IsEnabled(0)) {
    // Do something
    ...
}
```

Und **Aktivieren/Deaktivieren von** ein Segment wird angegeben, verwenden Sie Folgendes:

```csharp
SegmentedControl.SetEnabled(false, 0);
```

<a name="Modifying-the-Segmented-Controls-Appearance" />

## <a name="modifying-the-segmented-controls-appearance"></a>Ändern der Darstellung der segmentierte Steuerelemente

Sie können den folgenden Code verwenden, auf um den Hintergrund eines bestimmten Segments zu einem Bild zu ändern:

```csharp
SegmentedControl.SetBackgroundImage (UIImage.FromFile("background.png"), UIControlState.Normal, UIBarMetrics.Default);
```

Wo `UIControlState` gibt den Zustand des Steuerelements, das Sie das Image für als festlegen:

- Normal
- hervorgehoben
- Deaktiviert
- Ausgewählt
- Focused (Mit Fokus)

Und `UIBarMetrics` gibt an, die Metriken, die als verwendet:

- Standard
- Komprimieren
- DefaultPrompt
- CompactPrompt

Darüber hinaus können Sie den Unterteiler zwischen den Segmenten mit festlegen:

```csharp
SegmentedControl.SetDividerImage (UIImage.FromFile("divider.png"), UIControlState.Normal, UIControlState.Normal, UIBarMetrics.Default);
```

In dem die erste `UIControlState` gibt den Status des Segments, das links neben dem Unterteiler und das zweite `UIControlState` gibt den Status des Segments, das rechts.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden behandelt, Entwerfen und Arbeiten mit segmentierten Steuerelement in ein Xamarin.tvOS-app.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [TvOS Human Interface-Handbücher](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos verwendet.](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
