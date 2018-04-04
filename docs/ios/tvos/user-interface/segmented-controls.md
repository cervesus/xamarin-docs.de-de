---
title: Arbeiten mit segmentierte Steuerelemente
description: Dieser Artikel umfasst das Entwerfen von und Arbeiten mit segmentierte Steuerelemente innerhalb einer Xamarin.tvOS-app.
ms.prod: xamarin
ms.assetid: 23AD94CC-E93A-40B1-8E2B-ECD21FA355BE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: d4eac932c7fad628a0a65127bceb641f34ea5d79
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-segmented-controls"></a>Arbeiten mit segmentierte Steuerelemente

_Dieser Artikel umfasst das Entwerfen von und Arbeiten mit segmentierte Steuerelemente innerhalb einer Xamarin.tvOS-app._


Ein segmentiertes Steuerelement bietet es sich um eine Reihe von linearen Elemente, von die jedes ein Symbol oder Text enthalten kann, und wird verwendet, um eine Gruppe von verwandten Auswahlmöglichkeiten für dem Benutzer bereitstellen.

[![](segmented-controls-images/segment01.png "Segment beispielsteuerelementen")](segmented-controls-images/segment01.png#lightbox)

Apple hat die folgenden Vorschläge für die Arbeit mit segmentierte Steuerelemente:

- **Geben Sie ausreichend Speicherplatz** -Care muss dauert, bis ausreichend Platz zwischen anderen [den Fokus erhalten kann Elemente](~/ios/tvos/app-fundamentals/navigation-focus.md) und einem segmentierten Steuerelement. Ein einzelnes Segment wird ausgewählt, wenn es in den Fokus hat (nicht, wenn darauf geklickt wird) und der Benutzer versehentlich Segmente ändern kann, wenn sie tatsächlich den Fokus erhalten kann ein anderes-Element auf das aktuelle Segment auswählen möchten.
- **Split-Ansichten für das Filtern von Inhalten verwenden** -segmentierte Steuerelemente machen Sie keine gute Wahl für Inhalte, die Filterung, während die Teilung Ansichten für einfache Navigation zwischen dem Inhalt und die Filter entwickelt wurden.
- **Limit in sieben Segmente Max** -sollten Sie versuchen, die maximale Anzahl von Segmenten unten acht (8) als belassen Dies ist einfacher, Analysieren von über den Raum auf der Couch und einfacher zu navigieren.
- **Verwenden Sie einheitliche Segmentgröße Inhalt** – alle Segmente haben die gleiche Breite und, falls möglich, sollten Sie versuchen, dem Inhalt in jedem Segment die gleiche Größe behalten. Nicht nur die Segment-Steuerelemente visuell Bannerbild macht dies erleichtert es, auf einen Blick zu lesen.
- **Vermeiden Sie das Mischen von Symbole und Text** -jedes einzelne Segment kann entweder ein Symbol oder Text, jedoch nicht beides enthalten. Es ist, zwar möglich, Symbole und Text in der gleichen segmentierte Steuerelement zu mischen sollte dies vermieden werden.

<a name="About-Segment-Icons" />

## <a name="about-segment-icons"></a>Zu Segment Symbolen

Apple schlägt vor, Ihnen die Verwendung von einfachen, erkennbare Images für Segment Symbole, z. B. ein Vergrößerungsglas für die Suche. Übermäßig komplexe Symbole sind schwer zu erkennen auf einem Bildschirm TV über den Raum, daher ist es am besten, die Symbole in einfachen Darstellungen zu beschränken.

Können nicht gemischt werden Text und Symbole in einem bestimmten Segment, und vermeiden Sie das Mischen von Symbole und Text in einem einzelnen segmentierte Steuerelement. Es sollten entweder alle Symbole oder alle Text sein.

<a name="Summary" />

## <a name="segment-text"></a>Segment-Text

Apple macht die folgenden Vorschläge für die Arbeit mit der Segment-Text:

- **Verwenden Sie kurze, sinnvolle Nomen** -Segment der Titel sollte den Typ des Inhalts, der die Benutzer erwarten, bei der Auswahl von eines bestimmten Segments speziell darauf hingewiesen. Zum Beispiel: Musik oder Videos.
- **Verwenden Sie die Anfangsbuchstaben Groß-/Kleinschreibung** -jedes Wort des Titels Segmente sollte groß geschrieben werden, mit Ausnahme von Artikeln, Konjunktionen und Präpositionen von weniger als vier (4) Zeichen.
- **Verwenden Sie kurze, mit Fokus Titel** -behalten Sie den Titel, kurze und konzentriert sich auf den Typ des Inhalts zu erwarten, wenn das Segment ausgewählt ist.

Erneut, können nicht gemischt werden Text und Symbole für ein gegebenes Segment, und vermeiden Sie das Mischen von Symbole und Text in einem einzelnen segmentierte Steuerelement.

<a name="Segment-Controls-and-Storyboards" />

## <a name="segment-controls-and-storyboards"></a>Segment-Steuerelemente und Storyboards

Die einfachste Möglichkeit zum Arbeiten mit der Segment-Steuerelemente in einer app Xamarin.tvOS werden diese Benutzeroberfläche der Anwendung, die mit der iOS-Designer hinzufügen.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. In der **Lösung Pad**, doppelklicken Sie auf die `Main.storyboard` Datei und öffnet ihn zur Bearbeitung.
1. Ziehen Sie eine **Segment Steuerelement** aus der **Toolbox** und legen Sie sie in der Sicht: 

    [![](segmented-controls-images/segment02.png "Ein Segment-Steuerelement")](segmented-controls-images/segment02.png#lightbox)
1. In der **Registerkarte "Widget"** von der **Eigenschaft Pad**, können Sie verschiedene Eigenschaften des Steuerelements Segment wie z. B. anpassen seine **Stil** und **Zustand**: 

    [![](segmented-controls-images/segment03.png "Die Registerkarte "Widget"")](segmented-controls-images/segment03.png#lightbox)
1. Verwenden der **Segmente** Feld zum Steuern der Anzahl der Segmente im Controller.
1. Wählen Sie ein gegebenes Segment aus der **Segment Dropdownliste** die einzelnen Eigenschaften anpassen, wie z. B. **Titel** oder **Image** und steuern, ob ein gegebenes Segment ist  **Aktiviert** oder **ausgewählte** Wenn das Steuerelement angezeigt wird.
1. Weisen Sie schließlich **Namen** auf die Steuerelemente, damit Sie in C#-Code auf sie reagieren können. Zum Beispiel: 

    [![](segmented-controls-images/segment04.png "Weisen Sie einen Namen")](segmented-controls-images/segment04.png#lightbox)
1. Speichern Sie die Änderungen.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. In der **Projektmappen-Explorer**, doppelklicken Sie auf die `Main.storyboard` Datei und öffnet ihn zur Bearbeitung.
1. Ziehen Sie eine **Segment Steuerelement** aus der **Toolbox** und legen Sie sie in der Sicht: 

    [![](segmented-controls-images/segment02-vs.png "Ein Segment-Steuerelement")](segmented-controls-images/segment02-vs.png#lightbox)
1. In der **Registerkarte "Widget"** von der **Property Explorer**, können Sie verschiedene Eigenschaften des Steuerelements Segment wie z. B. Anpassen der **Stil** und **Zustand**: 

    [![](segmented-controls-images/segment03-vs.png "Die Registerkarte "Widget"")](segmented-controls-images/segment03-vs.png#lightbox)
1. Verwenden der **Segmente** Feld zum Steuern der Anzahl der Segmente im Controller.
1. Wählen Sie ein gegebenes Segment aus der **Segment Dropdownliste** die einzelnen Eigenschaften anpassen, wie z. B. **Titel** oder **Image** und steuern, ob ein gegebenes Segment ist  **Aktiviert** oder **ausgewählte** Wenn das Steuerelement angezeigt wird.
1. Weisen Sie schließlich **Namen** auf die Steuerelemente, damit Sie in C#-Code auf sie reagieren können. Zum Beispiel: 

    [![](segmented-controls-images/segment04-vs.png "Weisen Sie einen Namen")](segmented-controls-images/segment04-vs.png#lightbox)
1. Speichern Sie die Änderungen.
    
-----

Weitere Informationen zum Arbeiten mit Storyboards finden Sie unter unsere [Hello tvos. außerdem wurden Quick Start Guide](~/ios/tvos/get-started/hello-tvos.md). 

<a name="Working-with-Segmented-Controls" />

## <a name="working-with-segmented-controls"></a>Arbeiten mit segmentierte Steuerelemente

Wie oben s angegeben, segmentierte Steuerelement stellt einen linearen Elementsatz bereit, von denen jeder kann ein Symbol oder Text enthalten und wird verwendet, um eine Gruppe von verwandten Auswahlmöglichkeiten für dem Benutzer bereitzustellen.

Es gibt mehrere Möglichkeiten, die Sie in Ihrer app Xamarin.tvOS segmentierte Steuerelemente verwenden können.

<a name="Exposed-as-Outlets-and-Actions" />

## <a name="exposed-as-names-and-events"></a>Namen und Ereignisse verfügbar gemacht

Wenn Sie das Segment-Steuerelement in der Benutzeroberflächen-Designer erstellt und es als ein Steuerelement mit dem Namen und ein Ereignis verfügbar gemacht können Sie den folgenden Code verwenden, so reagieren Sie auf das Segment ändern:

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

Bei dem obigen Beispiel die Segment-Steuerelement verfügbar gemacht wurde als eine `PlayerCount` Namen und eine `PlayerCountChanged` Ereignisaktion. Weitere Informationen zum Arbeiten mit Aktionen und Ausgänge finden Sie unter der [Schreiben von Code mit Steckdosen und Aktionen](~/ios/tvos/get-started/hello-tvos.md#Writing-the-Code) Teil unserer [Hello tvos. außerdem wurden Quick Start Guide](~/ios/tvos/get-started/hello-tvos.md).

Die `SelectedSegment` Eigenschaft ruft ab oder legt den Index der aktuell ausgewählten Segment als eine Null (0) basiert. Wenn also Segmente fünf (5) ist das erste Segment einen Index von 0 (null) und der letzte Index vier (4).

<a name="Modifying-Segments" />

## <a name="modifying-segments"></a>Ändern von Segmenten

Zu einem beliebigen Zeitpunkt können Sie die Anzahl und den Inhalt der segmentierte Steuerelemente ändern. Verwenden Sie den folgenden Code, um ein neues Symbol Segment einzufügen:

```csharp
// Icon Segment
SegmentedControl.InsertSegment(UIImage.FromFile("icon.png"), 0, true);

// Text Segment
SegmentedControl.InsertSegment("New Segment", 0, true);
```

Der zweite Parameter definiert, in denen das Segment sind, mit einer NULL (0) je Index eingefügt. Wenn der letzte Parameter ist `true` einfügen animiert wird.

Um ein gegebenes Segment zu entfernen, verwenden Sie folgende Schritte aus:

```csharp
SegmentedControl.RemoveSegmentAtIndex(0, true);
```

Oder die folgenden Optionen, um alle Segmente zu entfernen:

```csharp
SegmentedControl.RemoveAllSegments();
```

Wenn der letzte Parameter ist `true`, die Entfernung animiert wird. Verwenden der `NumberOfSegments` Eigenschaft, um die aktuelle Anzahl der Segmente zurückzugeben.

Zum Abrufen der **Titel** oder **Symbol** für ein gegebenes Segment, verwenden Sie Folgendes:

```csharp
// Get title
var title = SegmentedControl.TitleAt(0);

// Get icon
var icon = SegmentedControl.ImageAt(0);
```

Und zum Ändern der **Titel** oder **Symbol**, verwenden Sie Folgendes:

```csharp
// Set title
SegmentedControl.SetTitle("New Title", 0);

// Set icon
SegmentedControl.SetImage(UIImage.FromFile("icon.png"), 0);
```

Um festzustellen, ob ein gegebenes Segment dieses **aktiviert**, verwenden Sie Folgendes:

```csharp
if (SegmentedControl.IsEnabled(0)) {
    // Do something
    ...
}
```

Und **aktivieren/deaktivieren** einem angegebenen Segment, verwenden Sie die folgenden:

```csharp
SegmentedControl.SetEnabled(false, 0);
```

<a name="Modifying-the-Segmented-Controls-Appearance" />

## <a name="modifying-the-segmented-controls-appearance"></a>Ändern der Darstellung des Steuerelements segmentierte

Sie können den folgenden Code verwenden, so ändern Sie den Hintergrund eines bestimmten Segments zu einem Bild:

```csharp
SegmentedControl.SetBackgroundImage (UIImage.FromFile("background.png"), UIControlState.Normal, UIBarMetrics.Default);
```

Wobei `UIControlState` gibt den Status des Steuerelements, das Sie das Bild für als festlegen:

- Normal
- Highlighted
- Deaktiviert
- Ausgewählt
- Focused

Und `UIBarMetrics` gibt die Metriken, die als verwendet:

- Standard
- Komprimieren
- DefaultPrompt
- CompactPrompt

Darüber hinaus können Sie den Unterteiler zwischen den Segmenten mit festlegen:

```csharp
SegmentedControl.SetDividerImage (UIImage.FromFile("divider.png"), UIControlState.Normal, UIControlState.Normal, UIBarMetrics.Default);
```

In dem die erste `UIControlState` gibt den Status des Segments links neben den Unterteiler und das zweite `UIControlState` gibt den Status des Segments auf der rechten Seite.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden behandelt, Entwerfen und Arbeiten mit segmentierte Steuerelement innerhalb einer Xamarin.tvOS-app.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos. außerdem wurden Handbücher für interaktive Workflowdienste-Schnittstelle](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos. außerdem wurden](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
