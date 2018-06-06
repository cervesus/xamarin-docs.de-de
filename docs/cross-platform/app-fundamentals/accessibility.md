---
title: Barrierefreiheit in Xamarin-Apps
description: Dieses Dokument enthält verschiedene Tipps für die Erstellung von apps zugegriffen werden kann. Er enthält beispielsweise Empfehlungen zu groß, hoher Kontrast und selbstbeschreibender Schnittstellen.
ms.prod: xamarin
ms.assetid: E587F0CF-7C1D-41F8-B5A8-DA3E738EDA81
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 97cd3655ac47a017d9590e1890b93d74f10a9c34
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780303"
---
# <a name="accessibility-in-xamarin-apps"></a>Barrierefreiheit in Xamarin-Apps

_Stellen Sie sicher, dass Ihre apps durch die breitesten Zielgruppe nutzbar sind_

Eingabehilfen bezieht sich auf das Konzept der Entwerfen von app-Benutzeroberflächen, die Anzeige und Eingabe-Unterstützung für Betriebssystemfunktionen gut funktionieren, wie z. B. großer Typ, hoher Kontrast vergrößern, Bildschirm lesen (Text), visual oder haptic Feedback-Hinweise, und alternativen Eingabemethoden.

Desktop und mobile Plattformen wie iOS, Android und Windows bieten integrierte APIs, mit denen Entwickler zugänglich apps erstellen, wie z. B. [Google TalkBack](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback) und [Apple VoiceOver](http://www.apple.com/accessibility/ios/voiceover/).

## <a name="platform-specific-apis"></a>Plattformspezifische APIs

Um die Richtlinien in diesem Dokument zu implementieren, verwenden Sie die APIs, die von jeder Plattform bereitgestellt werden:

- [**Android-Eingabehilfen**](~/android/app-fundamentals/accessibility.md)
- [**iOS-Eingabehilfen**](~/ios/app-fundamentals/accessibility.md)
- [**OS X-Eingabehilfen**](~/mac/app-fundamentals/accessibility.md)
- [**Xamarin.Forms**](~/xamarin-forms/app-fundamentals/accessibility/index.md)

<a name="checklist" />

## <a name="accessibility-checklist"></a>Prüfliste für Eingabehilfen

Befolgen Sie diese Tipps, um sicherzustellen, dass Ihre apps für die längsten möglichen Zielgruppe zugänglich sind. Sehen Sie sich die [Android Eingabehilfen testen Prüfliste](http://developer.android.com/training/accessibility/testing.html) und [Apple Seite für Eingabehilfen](http://www.apple.com/accessibility/) zusätzliche Informationen.

### <a name="support-large-fonts-and-high-contrast"></a>Unterstützung großer Schriftarten und hoher Kontrast

Keine hartcodierten Steuerelement Dimensionen und stattdessen lieber Layouts, die die Größe können, um größere Schriftgrade Ihren Bedürfnissen gerecht zu werden.
Testen Sie Farbschemas im Modus für hohe Kontraste um sicherzustellen, dass sie gelesen werden.

### <a name="make-the-user-interface-self-describing"></a>Stellen Sie die Benutzer Schnittstelle selbstbeschreibend

Markieren Sie alle Elemente der Benutzeroberfläche mit beschreibenden Text und Hinweise, die mit dem Bildschirm, lesen APIs für jede Plattform kompatibel sind.

### <a name="ensure-that-images-and-icons-have-an-alternate-text-description"></a>Stellen Sie sicher, Bildern und Symbolen eine alternative Beschreibung aufweisen

Bildern und Symbolen, die Teil der Benutzeroberfläche der Anwendung (z. B. Schaltflächen oder Anzeichen für Status, z. B.) sollte mit einem barrierefreie Beschreibung gekennzeichnet werden.

### <a name="design-the-visual-tree-with-accessible-navigation-in-mind"></a>Entwurf der visuellen Struktur mit zugänglich Navigation Bedenken

Verwenden die entsprechenden Layout-Steuerelemente oder APIs, damit Navigieren zwischen Steuerelementen mithilfe von alternativen Eingabemethoden folgt den gleichen logischen Datenfluss als mit dem Touchscreen.

Schließen Sie unnötige Elemente aus Sprachausgaben (dekorativen Bilder oder für Felder, die bereits zugreifen können, z. B. sind Bezeichnungen).

### <a name="dont-rely-on-audio-or-color-cues-alone"></a>Verlassen Sie sich nicht auf die Audio- oder Farbe Hinweise allein

Vermeiden Sie Situationen, in denen die alleinige Angabe des Status, Beendigung oder einem anderen Status eines Sounds oder Farbe geändert wird. Entweder die Benutzeroberfläche gehören deutlichen visuellen Hinweise (mit Sound und Farbe für nur Verstärkung), oder fügen Zugriff auf bestimmte Indikatoren entwerfen.

Wenn Sie Farben auswählen möchten, versuchen Sie, eine Palette zu vermeiden, die schwer zu farbenblind für Benutzer zu unterscheiden ist.

### <a name="captioning-for-video-text-for-audio"></a>Untertitel für video, Audio text

Geben Sie Beschriftungen für Videoinhalten, und ein lesbares Skript für Audioinhalte. Es ist auch hilfreich, geben Sie Steuerelemente, die von der Geschwindigkeit des Audio-und Videoinhalte angepasst, und stellen Sie sicher, Volume und für Wiedergabe und Pause Schaltflächen sind jedoch leicht zu finden und verwenden.

### <a name="localize"></a>Localize

Eingabehilfen Beschreibungen können (und sollten) lokalisiert werden, in dem die Anwendung mehrere Sprachen unterstützt.



## <a name="related-links"></a>Verwandte Links

- [Android-Eingabehilfen](~/android/app-fundamentals/accessibility.md)
- [iOS-Eingabehilfen](~/ios/app-fundamentals/accessibility.md)
- [OS X-Eingabehilfen](~/mac/app-fundamentals/accessibility.md)
- [Xamarin.Forms-Eingabehilfen](~/xamarin-forms/app-fundamentals/accessibility/index.md)
