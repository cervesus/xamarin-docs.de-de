---
title: Ice Cream Sandwich-Features
description: In diesem Artikel werden einige der neuen Features beschrieben, die Anwendungsentwicklern mit der Android 4-API – Ice Cream Sandwich zur Verfügung stehen. Er behandelt mehrere neue Technologien für die Benutzeroberfläche und untersucht außerdem eine Reihe neuer Funktionen, die Android 4 für die gemeinsame Nutzung von Daten durch Anwendungen und Geräte bietet.
ms.prod: xamarin
ms.assetid: 78E18A62-C12F-A699-37FA-44B9F6B44273
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: 4fbbe1bec317e66166d5218ef0ed54247aa4f6dd
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "76724375"
---
# <a name="ice-cream-sandwich-features"></a>Ice Cream Sandwich-Features

_In diesem Artikel werden einige der neuen Features beschrieben, die Anwendungsentwicklern mit der Android 4-API – Ice Cream Sandwich zur Verfügung stehen. Er behandelt mehrere neue Technologien für die Benutzeroberfläche und untersucht außerdem eine Reihe neuer Funktionen, die Android 4 für die gemeinsame Nutzung von Daten durch Anwendungen und Geräte bietet._

## <a name="overview"></a>Übersicht

Das Android-Betriebssystem, Version 4.0 (API Level 14), stellt eine größere Überarbeitung des Android-Betriebssystems dar und enthält eine Reihe wichtiger Änderungen und Upgrades, darunter:

- **Aktualisierte Benutzeroberfläche**: Verschiedene neue Features der Benutzeroberfläche bieten Entwicklern mehr Leistung und Flexibilität beim Erstellen von Benutzeroberflächen für Anwendungen. Zu diesen neuen Features gehören: `GridLayout`, `PopupMenu`, `Switch`-Widget und `TextureView`.
- **Bessere Hardwarebeschleunigung**: 2D-Rendering erfolgt jetzt für alle Android-Steuerelemente in der GPU. Darüber hinaus ist die Hardwarebeschleunigung standardmäßig in allen Anwendungen aktiviert, die für Android 4.0 entwickelt wurden.
- **Neue Daten-APIs**: Es gibt neuen Zugang zu Daten, auf die bisher offiziell nicht zugegriffen werden konnte, wie etwa Kalenderdaten und das Benutzerprofil des Gerätebesitzers.
- **Freigabe von App-Daten**: Das Freigeben von Daten zwischen Anwendungen und Geräten ist jetzt einfacher denn je, dank Technologien wie dem `ShareActionProvider`, mit dem eine Freigabeaktion komfortabel von einer Aktionsleiste erstellt werden kann, und *Android Beam* für *NFC (Near Field Communications, Nahbereichskommunikation)* , wodurch das Teilen von Daten zwischen nah beieinander befindlichen Geräten zu einem Kinderspiel wird.

In diesem Artikel untersuchen wir diese Features und weitere Änderungen, die an der Android 4.0-API vorgenommen wurden, und wir erläutern, wie jedes dieser Features mit Xamarin.Android verwendet wird.

## <a name="user-interface-features"></a>Features der Benutzeroberfläche

Mit Android 4 wird eine Vielzahl neuer Technologien für die Benutzeroberfläche eingeführt, darunter:

- **[GridLayout](~/android/user-interface/layouts/grid-layout.md)** : Unterstützt das 2D-Rasterlayout von Steuerelementen.
- **[Switch-Widget](~/android/user-interface/controls/switch.md)** : Ermöglicht das Umschalten zwischen EIN und AUS.
- **[TextureView](~/android/user-interface/controls/texture-view.md)** : Ermöglicht Video- und OpenGL-Inhalte in einer Ansicht.
- **[Navigationsleiste](~/android/user-interface/controls/navigation-bar.md)** : Enthält virtuelle Schaltflächen für Zurück, Home und Multitasking.

Darüber hinaus wurden andere Elemente der Benutzeroberfläche verbessert, wie etwa das `<a href"/guides/android/user_interface/popup_menus">PopupMenu</a>`, mit dem sich jetzt leichter arbeiten lässt, und Registerkarten, die eine geschliffenere Oberfläche aufweisen.

## <a name="sharing-features"></a>Freigabefeatures

Android 4 enthält eine Reihe neuer Technologien, mit denen sich Daten übergreifend zwischen Geräten und Anwendungen teilen lassen. Es bietet außerdem Zugriff auf verschiedene Arten von Daten, die bisher nicht verfügbar waren, wie etwa Kalenderinformationen und das Benutzerprofil des Gerätebesitzers. In diesem Abschnitt untersuchen wir eine Vielzahl von Features, die von Android 4 geboten werden und sich auf diese Bereiche beziehen, darunter:

- **[Android Beam](~/android/platform/android-beam.md)** : Ermöglicht die Freigabe von Daten per NFC.
- **[ShareActionProvider](~/android/user-interface/controls/action-bar.md)** : Erstellt einen Anbieter, der es Entwicklern ermöglicht, auf der Aktionsleiste Freigabeaktionen anzugeben.
- **[Benutzerprofil](~/android/user-interface/user-profile.md)** : Bietet Zugriff auf Profildaten des Gerätebesitzers.
- **[Kalender-API](~/android/user-interface/controls/calendar.md)** : Bietet Zugriff auf Kalenderdaten vom Kalenderanbieter.

## <a name="x86-emulators"></a>x86-Emulatoren

ICS unterstützt bisher die Entwicklung mit einem x86-Emulator noch nicht. x86-Emulators werden nur mit Android 2.3.3, API-Ebene 10, unterstützt. Weitere Informationen finden Sie unter [Konfigurieren des x86-Emulators](~/android/get-started/installation/android-emulator/index.md).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde eine Reihe der neuen Technologien behandelt, die jetzt in Android 4 verfügbar sind. Wir haben neue Features der Benutzeroberfläche besprochen, wie etwa das *GridLayout*, das *PopupMenu* und das *Switch*-Widget. Wir haben uns außerdem die neue Unterstützung für die Steuerung der Systembenutzeroberfläche sowie das Arbeiten mit der *TextureView* angesehen. Anschließend haben wir eine Reihe neuer Freigabetechnologien erörtert. Wir haben den geräteübergreifenden Informationsaustausch mithilfe von *Android Beam* per *NFC* behandelt, die neue *Kalender-API* erörtert und außerdem gezeigt, wie der neue integrierte *ShareActionProvider* verwendet wird.
Schließlich haben wir untersucht, wie der *ContactsContract*-Anbieter für den Zugriff auf Benutzerprofildaten verwendet wird.

## <a name="related-links"></a>Verwandte Links

- [TextureViewDemo (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/textureviewdemo)
- [CalendarDemo (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/calendardemo)
- [Registerkartenlayout-Tutorial](~/android/user-interface/layouts/tab-layout/index.md)
- [Ice Cream Sandwich](https://developer.android.com/about/versions/android-4.0-highlights.html)
- [Android 4.0-Plattform](https://developer.android.com/about/versions/android-4.0.html)
