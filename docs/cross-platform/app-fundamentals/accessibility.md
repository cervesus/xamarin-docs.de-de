---
title: Barrierefreiheit in xamarin-apps
description: Dieses Dokument enthält verschiedene Tipps für die Erstellung von zugänglichen apps. Sie enthält beispielsweise Empfehlungen zu großen Schriftarten, hohem Kontrast, selbst beschreibenden Schnittstellen und mehr.
ms.prod: xamarin
ms.assetid: E587F0CF-7C1D-41F8-B5A8-DA3E738EDA81
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: df042521d4e9852d6e23c2bbdf24484f9068250d
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571258"
---
# <a name="accessibility-in-xamarin-apps"></a>Barrierefreiheit in xamarin-apps

_Stellen Sie sicher, dass Ihre apps von der größtmöglichen Zielgruppe aus verwendbar sind_

Barrierefreiheit bezieht sich auf das Konzept des Entwurfs von App-Benutzeroberflächen, die eine gute Funktionsweise von Betriebssystem Anzeige-und-Eingabe Unterstützung aufweisen, wie z. b. große Typen, hohe Kontraste, Zoom, Bildschirm Lesevorgänge (Text-zu-Sprache), visuelle oder willkürliche Feedback Hinweise und Alternative Eingabemethoden.

Desktop-und Mobile Plattformen wie IOS, Android und Windows bieten integrierte APIs, mit denen Entwickler barrierefreie Apps erstellen können, wie z. b. [Google Talkback](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback) und [den VoiceOver von Apple](https://www.apple.com/accessibility/ios/voiceover/).

## <a name="platform-specific-apis"></a>Plattformspezifische APIs

Um die Richtlinien in diesem Dokument zu implementieren, verwenden Sie die von den einzelnen Plattformen bereitgestellten APIs:

- [**Android-Barrierefreiheit**](~/android/app-fundamentals/accessibility.md)
- [**IOS-Barrierefreiheit**](~/ios/app-fundamentals/accessibility.md)
- [**OS X-Barrierefreiheit**](~/mac/app-fundamentals/accessibility.md)
- [**Xamarin.Forms**](~/xamarin-forms/app-fundamentals/accessibility/index.md)

<a name="checklist"></a>

## <a name="accessibility-checklist"></a>Zugriffs Prüfliste

Befolgen Sie diese Tipps, um sicherzustellen, dass Ihre apps für die größtmögliche Zielgruppe zugänglich sind. Weitere Informationen finden Sie in der [Checkliste für Android-Barrierefreiheits Tests](https://developer.android.com/training/accessibility/testing.html) und [der Seite "Barrierefreiheit](https://www.apple.com/accessibility/) " von Apple.

### <a name="support-large-fonts-and-high-contrast"></a>Unterstützen von großen Schriftarten und hohem Kontrast

Vermeiden Sie hardcodierungssteuerungsdimensionen, und bevorzugen Sie stattdessen Layouts, die die Größe ändern können, um größere Schriftgrößen zu bieten.
Testen Sie Farbschemas im Modus mit hohem Kontrast, um sicherzustellen, dass Sie lesbar sind.

### <a name="make-the-user-interface-self-describing"></a>Benutzeroberfläche selbst beschreiben

Markieren Sie alle Elemente der Benutzeroberfläche durch beschreibenden Text und Hinweise, die mit den Bildschirm Lese-APIs auf den einzelnen Plattformen kompatibel sind.

### <a name="ensure-that-images-and-icons-have-an-alternate-text-description"></a>Stellen Sie sicher, dass Bilder und Symbole eine alternative Textbeschreibung aufweisen.

Bilder und Symbole, die Teil der Anwendungs Benutzeroberfläche sind (z. b. Schaltflächen oder Status Indikatoren), sollten mit einer zugänglichen Beschreibung gekennzeichnet werden.

### <a name="design-the-visual-tree-with-accessible-navigation-in-mind"></a>Entwerfen der visuellen Struktur mit Barrierefreiheits Navigation

Verwenden Sie geeignete Layoutsteuerelemente oder APIs, damit die Navigation zwischen Steuerelementen mit alternativen Eingabemethoden dem gleichen logischen Ablauf wie die Verwendung des Touchscreens folgt.

Ausschließen nicht benötigter Elemente aus Bildschirmlesern (z. b. für Felder, die bereits zugänglich sind).

### <a name="dont-rely-on-audio-or-color-cues-alone"></a>Verlassen Sie sich nicht allein auf Audio-oder Farb Hinweise.

Vermeiden Sie Situationen, in denen der einzige Hinweis auf den Fortschritt, den Abschluss oder einen anderen Status eine Ton-oder Farbänderung ist. Entwerfen Sie entweder die Benutzeroberfläche so, dass Sie klare visuelle Hinweise enthält (nur mit Sound und Farbe zur Verstärkung), oder fügen Sie bestimmte Barrierefreiheits Indikatoren hinzu.

Versuchen Sie bei der Auswahl von Farben, eine Palette zu vermeiden, die für Benutzer mit Farb Blindheit schwer zu unterscheiden ist.

### <a name="captioning-for-video-text-for-audio"></a>Beschriftungen für Video, Text für Audiodaten

Stellen Sie Beschriftungen für Videoinhalte und ein lesbares Skript für Audioinhalte bereit. Es ist auch hilfreich, Steuerelemente bereitzustellen, die die Geschwindigkeit von Audioinhalten oder Videoinhalten anpassen, und sicherstellen, dass die Schaltflächen "Volume" und "Wiedergabe" leicht zu finden und zu verwenden sind

### <a name="localize"></a>Localize

Barrierefreiheits Beschreibungen können (und sollten) lokalisiert werden, wenn die Anwendung mehrere Sprachen unterstützt.

## <a name="related-links"></a>Verwandte Links

- [Android-Barrierefreiheit](~/android/app-fundamentals/accessibility.md)
- [IOS-Barrierefreiheit](~/ios/app-fundamentals/accessibility.md)
- [OS X-Barrierefreiheit](~/mac/app-fundamentals/accessibility.md)
- [Xamarin.Forms-Barrierefreiheit](~/xamarin-forms/app-fundamentals/accessibility/index.md)
