---
title: Arbeiten mit tvos segmentierten Steuerelementen in xamarin
description: In diesem Dokument wird beschrieben, wie Sie mit tvos segmentierte Steuerelemente in einer mit xamarin erstellten App arbeiten. Dabei werden Segment Symbole und Text, Ereignisse, das Ändern der Darstellung des Steuer Elements und mehr erläutert.
ms.prod: xamarin
ms.assetid: 23AD94CC-E93A-40B1-8E2B-ECD21FA355BE
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: a31b0bcf3a61b5a1ea7e84f35131e6ceca1eef82
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84569880"
---
# <a name="working-with-tvos-segmented-controls-in-xamarin"></a>Arbeiten mit tvos segmentierten Steuerelementen in xamarin

Ein segmentiertes Steuerelement stellt eine Reihe linearer Elemente bereit, von denen jedes ein Symbol oder einen Text enthalten kann. Sie wird verwendet, um dem Benutzer eine Reihe verwandter Optionen bereitzustellen.

[![](segmented-controls-images/segment01.png "Sample segment controls")](segmented-controls-images/segment01.png#lightbox)

Apple hat die folgenden Vorschläge zum Arbeiten mit segmentierten Steuerelementen:

- **Stellen Sie genügend Speicherplatz** zur Verfügung, um genügend Speicherplatz zwischen anderen [Fokus baren Elementen](~/ios/tvos/app-fundamentals/navigation-focus.md) und einem segmentierten Steuerelement bereitzustellen. Ein einzelnes Segment wird ausgewählt, wenn es sich im Fokus befindet (nicht beim Klicken), und der Benutzer kann versehentlich Segmente ändern, wenn ein anderes Fokus bares Element für das aktuelle Segment ausgewählt werden soll.
- **Verwenden von geteilten Sichten für das Filtern von Inhalten** : segmentierte Steuerelemente treffen keine guten Optionen für das Filtern von Inhalten, da geteilte Ansichten für eine einfache Navigation zwischen dem Inhalt und den Filtern entworfen wurden.
- Maximal **7 Segmente begrenzen** -Sie sollten versuchen, die maximale Anzahl von Segmenten unterhalb von acht (8) beizubehalten, da dies im Raum auf dem Couch einfacher zu analysieren und leichter zu navigieren ist.
- **Konsistente Segment Inhalts Größe verwenden** : alle Segmente weisen die gleiche Breite auf, und wenn möglich, sollten Sie versuchen, den Inhalt in jedem Segment in derselben Größe beizubehalten. Dadurch sind die Segment Steuerelemente nicht mehr visuell ansprechend, aber Sie können auf einen Blick leichter lesbar sein.
- **Vermeiden Sie das Mischen von Symbolen und Text** . jedes einzelne Segment kann entweder ein Symbol oder einen Text enthalten, aber nicht beides. Obwohl es möglich ist, sowohl Symbole als auch Text im gleichen segmentierten Steuerelement zu mischen, sollte dies vermieden werden.

<a name="About-Segment-Icons"></a>

## <a name="about-segment-icons"></a>Informationen zu Segment Symbolen

Apple schlägt vor, einfache, erkennbare Bilder für Segment Symbole zu verwenden, z. b. eine Lupe für die Suche. Übermäßig komplexe Symbole sind auf einem TV-Bildschirm im gesamten Raum schwer zu erkennen. Daher sollten Sie Ihre Symbole am besten auf einfache Darstellungen beschränken.

Sie können nicht sowohl Text als auch Symbole für ein bestimmtes Segment mischen und vermeiden, Symbole und Text in einem einzelnen segmentierten Steuerelement zu mischen. Dabei sollte es sich entweder um alle Symbole oder um den gesamten Text handeln.

<a name="Summary"></a>

## <a name="segment-text"></a>Segment Text

Apple nimmt die folgenden Vorschläge zum Arbeiten mit Segment Text vor:

- **Verwenden Sie kurze, sinnvolle Nomen** . der Segment Titel sollte eindeutig den Inhaltstyp angeben, den der Benutzer bei der Auswahl des angegebenen Segments erwarten sollte. Beispiel: Musik oder Videos.
- Groß-/Kleinschreibung von groß- **/Kleinschreibung verwenden** : jedes Wort des Segment Titels sollte groß geschrieben werden, mit Ausnahme von Artikeln, ankreuzungen und Positionen von weniger als vier (4) Zeichen.
- **Verwenden Sie kurze, fokussierte Titel** : behalten Sie die Titel, kurz, und konzentrieren Sie sich auf den Inhaltstyp, der bei Auswahl des Segments zu erwarten ist.

Auch hier können Sie nicht sowohl Text als auch Symbole für ein bestimmtes Segment mischen und vermeiden, Symbole und Text in einem einzelnen segmentierten Steuerelement zu mischen.

<a name="Segment-Controls-and-Storyboards"></a>

## <a name="segment-controls-and-storyboards"></a>Segment Steuerelemente und Storyboards

Die einfachste Möglichkeit, mit Segment Steuerelementen in einer xamarin. tvos-APP zu arbeiten, besteht darin, Sie mithilfe des IOS-Designers zur Benutzeroberfläche der APP hinzuzufügen.

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

1. Doppelklicken Sie im **Lösungspad**auf die `Main.storyboard` Datei, und öffnen Sie Sie zur Bearbeitung.
1. Ziehen Sie ein **Segment-Steuer** Element aus der **Toolbox** , und legen Sie es in der Ansicht ab: 

    [![](segmented-controls-images/segment02.png "A Segment Control")](segmented-controls-images/segment02.png#lightbox)
1. Auf der **Registerkarte widget** des **eigenschaftenpad**können Sie verschiedene Eigenschaften des Segment Steuer Elements anpassen, z. b. den **Stil** und den **Zustand**: 

    [![](segmented-controls-images/segment03.png "The Widget Tab")](segmented-controls-images/segment03.png#lightbox)
1. Verwenden Sie das Feld **Segmente** , um die Anzahl der Segmente im Controller zu steuern.
1. Wählen Sie ein bestimmtes Segment aus der **Dropdown Liste Segment** aus, um die einzelnen Eigenschaften, z. b. **Titel** oder **Bild** , anzupassen und zu steuern, ob ein bestimmtes Segment **aktiviert** oder **ausgewählt** ist, wenn das Steuerelement angezeigt wird.
1. Weisen Sie den Steuerelementen schließlich **Namen** zu, damit Sie in c#-Code darauf reagieren können. Beispiel: 

    [![](segmented-controls-images/segment04.png "Assign a Name")](segmented-controls-images/segment04.png#lightbox)
1. Speichern Sie die Änderungen.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Doppelklicken Sie im **Projektmappen-Explorer**auf die `Main.storyboard` Datei, und öffnen Sie Sie zur Bearbeitung.
1. Ziehen Sie ein **Segment-Steuer** Element aus der **Toolbox** , und legen Sie es in der Ansicht ab: 

    [![](segmented-controls-images/segment02-vs.png "A Segment Control")](segmented-controls-images/segment02-vs.png#lightbox)
1. Auf der **Registerkarte widget** des **Eigenschaften-Explorers**können Sie verschiedene Eigenschaften des Segment Steuer Elements anpassen, z. b. den **Stil** und den **Zustand**: 

    [![](segmented-controls-images/segment03-vs.png "The Widget Tab")](segmented-controls-images/segment03-vs.png#lightbox)
1. Verwenden Sie das Feld **Segmente** , um die Anzahl der Segmente im Controller zu steuern.
1. Wählen Sie ein bestimmtes Segment aus der **Dropdown Liste Segment** aus, um die einzelnen Eigenschaften, z. b. **Titel** oder **Bild** , anzupassen und zu steuern, ob ein bestimmtes Segment **aktiviert** oder **ausgewählt** ist, wenn das Steuerelement angezeigt wird.
1. Weisen Sie den Steuerelementen schließlich **Namen** zu, damit Sie in c#-Code darauf reagieren können. Beispiel: 

    [![](segmented-controls-images/segment04-vs.png "Assign a Name")](segmented-controls-images/segment04-vs.png#lightbox)
1. Speichern Sie die Änderungen.

-----

Weitere Informationen zum Arbeiten mit Storyboards finden Sie in unserer [Hello-, tvos-Schnellstarthandbuch](~/ios/tvos/get-started/hello-tvos.md). 

<a name="Working-with-Segmented-Controls"></a>

## <a name="working-with-segmented-controls"></a>Arbeiten mit segmentierten Steuerelementen

Wie bereits erwähnt, stellt das segmentierte Steuerelement eine Reihe linearer Elemente bereit, von denen jedes ein Symbol oder einen Text enthalten kann. Sie wird verwendet, um dem Benutzer eine Reihe verwandter Optionen bereitzustellen.

Es gibt verschiedene Möglichkeiten, wie Sie in ihrer xamarin. tvos-App mit segmentierten Steuerelementen arbeiten können.

<a name="Exposed-as-Outlets-and-Actions"></a>

## <a name="exposed-as-names-and-events"></a>Verfügbar gemacht als Namen und Ereignisse

Wenn Sie das Segment Steuerelement im Schnittstellen-Designer erstellt und als benanntes Steuerelement verfügbar gemacht haben, können Sie den folgenden Code verwenden, um auf die Änderung des Segments zu reagieren:

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

Im obigen Beispiel wurde das Segment Steuerelement als `PlayerCount` Name und eine `PlayerCountChanged` Ereignis Aktion verfügbar gemacht. Weitere Informationen zum Arbeiten mit Aktionen und Outlets finden Sie im Abschnitt [Schreiben des Codes mit Outlets und Aktionen](~/ios/tvos/get-started/hello-tvos.md#Writing-the-Code) in unserer [Schnellstarthandbuch Hello, tvos](~/ios/tvos/get-started/hello-tvos.md).

Die- `SelectedSegment` Eigenschaft ruft das aktuell ausgewählte Segment als NULL (0)-basierten Index ab oder legt dieses fest. Wenn Sie also fünf (5) Segmente haben, weist das erste Segment den Index 0 (null) und den letzten Index von vier (4) auf.

<a name="Modifying-Segments"></a>

## <a name="modifying-segments"></a>Ändern von Segmenten

Sie können die Anzahl und den Inhalt der segmentierten Steuerelemente jederzeit ändern. Verwenden Sie den folgenden Code, um ein neues Symbol Segment einzufügen:

```csharp
// Icon Segment
SegmentedControl.InsertSegment(UIImage.FromFile("icon.png"), 0, true);

// Text Segment
SegmentedControl.InsertSegment("New Segment", 0, true);
```

Mit dem zweiten Parameter wird definiert, wo das Segment mit einem NULL-Index (0) eingefügt wird. Wenn der letzte Parameter ist, `true` wird der Einfügevorgang animiert.

Um ein bestimmtes Segment zu entfernen, verwenden Sie Folgendes:

```csharp
SegmentedControl.RemoveSegmentAtIndex(0, true);
```

Sie können auch Folgendes entfernen, um alle Segmente zu entfernen:

```csharp
SegmentedControl.RemoveAllSegments();
```

Wenn der letzte Parameter ist `true` , wird die Entfernung animiert. Verwenden Sie die- `NumberOfSegments` Eigenschaft, um die aktuelle Anzahl von Segmenten zurückzugeben.

Um den **Titel** oder das **Symbol** für ein bestimmtes Segment zu erhalten, verwenden Sie Folgendes:

```csharp
// Get title
var title = SegmentedControl.TitleAt(0);

// Get icon
var icon = SegmentedControl.ImageAt(0);
```

Zum Ändern des **Titels** oder **Symbols**verwenden Sie Folgendes:

```csharp
// Set title
SegmentedControl.SetTitle("New Title", 0);

// Set icon
SegmentedControl.SetImage(UIImage.FromFile("icon.png"), 0);
```

Um festzustellen, ob ein bestimmtes Segment **aktiviert**ist, verwenden Sie Folgendes:

```csharp
if (SegmentedControl.IsEnabled(0)) {
    // Do something
    ...
}
```

Verwenden Sie Folgendes, um ein bestimmtes Segment zu **Aktivieren bzw** . zu deaktivieren:

```csharp
SegmentedControl.SetEnabled(false, 0);
```

<a name="Modifying-the-Segmented-Controls-Appearance"></a>

## <a name="modifying-the-segmented-controls-appearance"></a>Ändern der Darstellung des segmentierten Steuer Elements

Sie können den folgenden Code verwenden, um den Hintergrund eines bestimmten Segments in ein Bild zu ändern:

```csharp
SegmentedControl.SetBackgroundImage (UIImage.FromFile("background.png"), UIControlState.Normal, UIBarMetrics.Default);
```

Where `UIControlState` gibt den Zustand des Steuer Elements an, für das Sie das Bild festlegen:

- Normal
- Highlighted
- Disabled
- Ausgewählt
- Focused

Und `UIBarMetrics` gibt die zu verwendenden Metriken an:

- Standard
- Kompakt
- DefaultPrompt
- Compactprompt

Darüber hinaus können Sie den unter Teiler zwischen den Segmenten mithilfe von festlegen:

```csharp
SegmentedControl.SetDividerImage (UIImage.FromFile("divider.png"), UIControlState.Normal, UIControlState.Normal, UIBarMetrics.Default);
```

, Wobei der erste `UIControlState` den Status des Segments Links vom unter Teiler angibt und der zweite `UIControlState` den Zustand des Segments auf der rechten Seite angibt.

<a name="Summary"></a>

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde das Entwerfen und arbeiten mit segmentiertem Steuerelement in einer xamarin. tvos-App behandelt.

## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos Human Interface Guides](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Leitfaden zur APP-Programmierung für tvos](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
