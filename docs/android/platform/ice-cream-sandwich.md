---
title: ICE Cream Sandwich-Funktionen
description: Dieser Artikel beschreibt einige der neuen Features für Entwickler mit Android 4-API - Ice Cream Sandwich verfügbar. Es behandelt verschiedene neue Technologien für die Benutzer-Schnittstelle und überprüft dann eine Reihe neuer Funktionen, die Android 4 bietet für die Freigabe von Daten zwischen Anwendungen und zwischen Geräten.
ms.prod: xamarin
ms.assetid: 78E18A62-C12F-A699-37FA-44B9F6B44273
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: adb53af9d2e6707cb1fca3c59af63db76e5346d5
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50102619"
---
# <a name="ice-cream-sandwich-features"></a>ICE Cream Sandwich-Funktionen

_Dieser Artikel beschreibt einige der neuen Features für Entwickler mit Android 4-API - Ice Cream Sandwich verfügbar. Es behandelt verschiedene neue Technologien für die Benutzer-Schnittstelle und überprüft dann eine Reihe neuer Funktionen, die Android 4 bietet für die Freigabe von Daten zwischen Anwendungen und zwischen Geräten._

## <a name="overview"></a>Übersicht

Android OS, Version 4.0 (API-Ebene 14) stellt einen wichtigen Überarbeiten des Android-Betriebssysteme und umfasst eine Reihe von wichtigen Änderungen und Aktualisierungen, einschließlich:

-   **Aktualisiert die Benutzeroberfläche** – mehrere neue Funktionen der Benutzeroberfläche erhalten Entwickler, die mehr Leistung und Flexibilität beim Erstellen von Anwendungsbenutzer-Schnittstellen. Diese neuen Features gehören: `GridLayout` , `PopupMenu` , `Switch` Widget, und `TextureView` . 
-   **Bessere Hardwarebeschleunigung** – 2D, der rendering jetzt erfolgt auf der GPU für alle Android-Steuerelemente. Darüber hinaus ist die Hardwarebeschleunigung aktiviert, wird standardmäßig in allen Anwendungen, die für Android 4.0 entwickelt wurden. 
-   **APIs für neue Daten** – es gibt neuer Zugriff auf Daten, die nicht zuvor offiziell zugegriffen werden kann, z. B. Kalenderdaten und des Benutzerprofils des der Eigentümer des Geräts. 
-   **App-Datenfreigabe** – gemeinsame Datennutzung zwischen Anwendungen und Geräten ist jetzt einfacher denn je über Technologien wie z. B. die `ShareActionProvider` , sodass sie mühelos auf eine Aktion zur Freigabe in eine Aktionsleiste erstellen und *Android Balken* für *Near Field Communications (NFC)* , das ist es ein Kinderspiel zum Freigeben von Daten auf Geräten in unmittelbarer Nähe zueinander. 


In diesem Artikel wir werden untersuchen diese Funktionen und andere Änderungen, die an die Android 4.0-API vorgenommen wurden, und wir erläutern, wie jede Funktion, die mit Xamarin.Android verwendet.

## <a name="user-interface-features"></a>Features der Benutzeroberfläche

Eine Vielzahl von neuen Benutzer Schnittstelle Technologien stehen mit Android 4, einschließlich:

-   **[GridLayout](~/android/user-interface/layouts/grid-layout.md)**  – 2D Grid-Layout der Steuerelemente unterstützt. 
-   **[Wechseln Sie Widget](~/android/user-interface/controls/switch.md)**  – ermöglicht das Umschalten zwischen ON oder OFF. 
-   **[TextureView](~/android/user-interface/controls/texture-view.md)**  – können Sie Video- und OpenGL-Inhalte in einer Ansicht. 
-   **[Navigationsleiste](~/android/user-interface/controls/navigation-bar.md)**  – virtuelle Schaltflächen für die Sicherung, Heim- und Multitasking enthält. 


Darüber hinaus andere UI-Elemente wurden verbessert, wie z. B. die `<a href"/guides/android/user_interface/popup_menus">PopupMenu</a>`, das ist jetzt einfacher zu verwenden und für Registerkarten, die ein besseres aussehen.

## <a name="sharing-features"></a>Features zur gemeinsamen Nutzung

Android 4 umfasst verschiedene neue Technologien, mit die wir die Daten auf Geräten und Anwendungen gemeinsam nutzen können. Darüber hinaus den Zugriff auf verschiedene Arten von Daten, die nicht bereits verfügbar ist, z. B. Kalenderinformationen und der Eigentümer des Geräts des Benutzerprofils waren. In diesem Abschnitt untersuchen wir werden eine Reihe von Funktionen von Angeboten Android 4 dieser Adresse diesen Bereichen, einschließlich:

-  **[Android Balken](~/android/platform/android-beam.md)**  – ermöglicht die Datenfreigabe über NFC.
-   **[ShareActionProvider](~/android/user-interface/controls/action-bar.md)**  – erstellt einen Anbieter, die Entwickler Freigabe Aktionen in der Aktionsleiste angeben kann. 
-   **[Benutzerprofil](~/android/user-interface/user-profile.md)**  – ermöglicht den Zugriff auf die Profildaten der Eigentümer des Geräts. 
-   **[Kalender-API-](~/android/user-interface/controls/calendar.md)**  – ermöglicht den Zugriff auf die Kalenderdaten des Kalender-Anbieters. 

## <a name="x86-emulators"></a>X86 Emulatoren

ICS unterstützt noch keine-Entwicklung mit x X86 Emulator. X86 Emulatoren sind nur mit Android 2.3.3, API-Ebene 10 unterstützt. Finden Sie unter [konfigurieren die X86 Emulator](~/android/get-started/installation/android-emulator/index.md) für Weitere Informationen.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel behandelt eine Vielzahl von neuen Technologien, die mit Android 4 verfügbar sind. Wir überprüfen neuer Features der Benutzeroberfläche wie z. B. die *GridLayout*, *PopupMenu*, und *Switch* Widget. Wir haben auch einige der neuen Unterstützung für das System-Benutzeroberfläche sowie Informationen zum Arbeiten mit steuern die *TextureView*. Dann besprochen haben wir eine Vielzahl von Technologien für neue Freigabe. Wir erläutert, wie *Android Balken* lassen Sie uns Freigeben von Informationen auf Geräten, mit denen *NFC*, erläutert die neue *Kalender-API*, und außerdem gezeigt, wie die integrierten in verwenden *ShareActionProvider*.
Abschließend wir untersucht, wie Sie mit der *ContactsContract* Anbieter für den Datenzugriff für Benutzer-Profil.



## <a name="related-links"></a>Verwandte Links

- [ICE Cream Sandwich-Beispiele](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ICS_Samples/)
- [TextureViewDemo (Beispiel)](https://developer.xamarin.com/samples/monodroid/TextureViewDemo/)
- [CalendarDemo (Beispiel)](https://developer.xamarin.com/samples/monodroid/CalendarDemo/)
- [Registerkarte Layout-Tutorial](~/android/user-interface/layouts/tab-layout/index.md)
- [ICE Cream Sandwich](http://developer.android.com/about/versions/android-4.0-highlights.html)
- [Android 4.0-Plattform](http://developer.android.com/about/versions/android-4.0.html)
