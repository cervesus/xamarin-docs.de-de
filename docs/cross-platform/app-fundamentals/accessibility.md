---
title: Barrierefreiheit in Xamarin-Apps
description: Dieses Dokument enthält verschiedene Tipps für die Erstellung von apps mit zugegriffen werden kann. Er enthält beispielsweise Empfehlungen zu großen Schriftarten, hoher Kontrast, selbstbeschreibende Schnittstellen und mehr.
ms.prod: xamarin
ms.assetid: E587F0CF-7C1D-41F8-B5A8-DA3E738EDA81
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 0ec264e0f3d381fdac46c79dd479da2bc768954f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61282188"
---
# <a name="accessibility-in-xamarin-apps"></a>Barrierefreiheit in Xamarin-Apps

_Stellen Sie sicher, dass Ihre apps durch die größtmögliche Zielgruppe verwendbar sind._

Barrierefreiheit bezieht sich auf das Konzept der Entwerfen von app-Benutzeroberflächen, die Anzeige und Eingabe-Unterstützung von Betriebssystemfunktionen gut funktionieren, wie z. B. vergrößern, großen Schriftgraden, hoher Kontrast, Sprachausgabe (Speech), visual oder Übermitteln von haptischem Feedback-Hinweise, und Alternative Eingabemethoden.

Desktop- und mobile Plattformen wie iOS, Android und Windows bieten integrierte APIs, mit denen Entwickler, die apps zugegriffen werden kann, wie z. B. erstellen [Google TalkBack](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback) und [Apple VoiceOver](http://www.apple.com/accessibility/ios/voiceover/).

## <a name="platform-specific-apis"></a>Plattformspezifische APIs

Um die Richtlinien in diesem Dokument zu implementieren, verwenden Sie die APIs, die von jeder Plattform bereitgestellt werden:

- [**Android-Barrierefreiheit**](~/android/app-fundamentals/accessibility.md)
- [**iOS-Barrierefreiheit**](~/ios/app-fundamentals/accessibility.md)
- [**Zugriff auf OS X**](~/mac/app-fundamentals/accessibility.md)
- [**Xamarin.Forms**](~/xamarin-forms/app-fundamentals/accessibility/index.md)

<a name="checklist" />

## <a name="accessibility-checklist"></a>Checkliste für die Barrierefreiheit

Führen Sie diese Tipps, um sicherzustellen, dass Ihre apps zugegriffen werden kann, um die größtmögliche Zielgruppe möglich sind. Sehen Sie sich die [Android Barrierefreiheit testen Prüfliste](https://developer.android.com/training/accessibility/testing.html) und [Apple Seite für Eingabehilfen](http://www.apple.com/accessibility/) für zusätzliche Informationen.

### <a name="support-large-fonts-and-high-contrast"></a>Unterstützung von großen Schriftarten und einen hohen Kontrast

Hart-Steuerelement Dimensionen und stattdessen lieber Layouts, die zur Aufnahme von größeren Schriftgraden ändern können.
Testen Sie Farbschemas im Modus mit hohem Kontrast, stellen Sie sicher, dass sie lesbar sind.

### <a name="make-the-user-interface-self-describing"></a>Stellen Sie die Benutzer Schnittstelle selbstbeschreibend

Markieren Sie alle Elemente der Benutzeroberfläche mit beschreibenden Text und Hinweise, die mit dem Bildschirm, lesen APIs auf jeder Plattform kompatibel sind.

### <a name="ensure-that-images-and-icons-have-an-alternate-text-description"></a>Sicherstellen Sie, dass die Bilder und Symbole eine Beschreibung der alternative Text haben

Bilder und Symbole, die Teil der Benutzeroberfläche der Anwendung (z. B. Schaltflächen oder Anzeichen für Status, z. B.) sollten mit einer Beschreibung verfügbar gekennzeichnet werden.

### <a name="design-the-visual-tree-with-accessible-navigation-in-mind"></a>Entwerfen der visuellen Struktur mit zugänglich Navigation Bedenken

Verwenden die entsprechenden Layout-Steuerelemente oder APIs, sodass Navigieren zwischen den Steuerelementen mithilfe von alternativen Eingabemethoden folgt dem gleichen logischen Ablauf als über den Touchscreen.

Schließen Sie keine unnötigen Elemente, von der Sprachausgabe (Ausgestaltungsbilder oder Bezeichnungen, Felder, die bereits zugegriffen werden kann, z. B.).

### <a name="dont-rely-on-audio-or-color-cues-alone"></a>Verlassen Sie sich nicht auf Audio- oder Farbe Hinweise allein

Vermeiden Sie Situationen, in denen die alleinige Angabe der Bearbeitung, Vervollständigung oder einem anderen Zustand eines Sounds oder Farbe-Änderung. Entweder Entwerfen der Benutzeroberfläche deutlichen visuellen Hinweise (mit Sounds und Farbe für nur vertiefendes) umfassen oder Zugriff auf bestimmte Indikatoren hinzufügen.

Wenn Sie Farben auswählen möchten, versuchen Sie, eine Palette zu vermeiden, die schwer für Benutzer mit Farbenblindheit zu unterscheiden ist.

### <a name="captioning-for-video-text-for-audio"></a>Untertitel für Videoanrufe, SMS für audio

Geben Sie Beschriftungen für Videoinhalte und ein lesbares Skript für Audioinhalte. Es ist auch hilfreich, geben Sie Steuerelemente, die die Geschwindigkeit von Audio-und Videoinhalte anpassen, und stellen Sie sicher, Volume und abspielen/Pause-Schaltflächen sind leicht zu finden und verwenden.

### <a name="localize"></a>Localize

Barrierefreiheit-Beschreibungen können (und sollten) lokalisiert werden, in dem die Anwendung für mehrere Sprachen unterstützt.



## <a name="related-links"></a>Verwandte Links

- [Android-Barrierefreiheit](~/android/app-fundamentals/accessibility.md)
- [iOS-Barrierefreiheit](~/ios/app-fundamentals/accessibility.md)
- [Zugriff auf OS X](~/mac/app-fundamentals/accessibility.md)
- [Xamarin.Forms-Barrierefreiheit](~/xamarin-forms/app-fundamentals/accessibility/index.md)
