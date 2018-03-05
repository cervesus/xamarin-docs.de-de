---
title: Eis Rustikal Sandwich-Funktionen
description: "Dieser Artikel beschreibt einige der neuen Funktionen, die mit der Android 4-API - Eis Rustikal Sandwich Anwendungsentwicklern zur Verfügung stehen. Es umfasst mehrere neue Technologien für die Benutzer-Schnittstelle und dann untersucht eine Vielzahl von neuen Funktionen, die Android 4 für die Freigabe von Daten zwischen Anwendungen und Geräte bietet."
ms.topic: article
ms.prod: xamarin
ms.assetid: 78E18A62-C12F-A699-37FA-44B9F6B44273
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.openlocfilehash: 9726ec83cd38dd2f237152f0f8e7373f509a3e01
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="ice-cream-sandwich-features"></a>Eis Rustikal Sandwich-Funktionen

_Dieser Artikel beschreibt einige der neuen Funktionen, die mit der Android 4-API - Eis Rustikal Sandwich Anwendungsentwicklern zur Verfügung stehen. Es umfasst mehrere neue Technologien für die Benutzer-Schnittstelle und dann untersucht eine Vielzahl von neuen Funktionen, die Android 4 für die Freigabe von Daten zwischen Anwendungen und Geräte bietet._

## <a name="overview"></a>Übersicht

Android OS Version 4.0 (API-Ebene 14) stellt einen wichtigen reagieren von Android-Betriebssysteme und umfasst eine Reihe von wichtige Änderungen und Updates, einschließlich:

-   **Aktualisiert die Benutzeroberfläche** – mehrere neue Features der Benutzeroberfläche können Entwickler, die mehr Leistung und Flexibilität beim Erstellen von Anwendungsbenutzer-Schnittstellen. Diese neuen Funktionen zählen: `GridLayout` , `PopupMenu` , `Switch` Widget "", und `TextureView` . 
-   **Hardwarebeschleunigung zu einer besseren** – 2D Rendern jetzt auf dem GPU für alle Android Steuerelemente erfolgt. Darüber hinaus ist die Hardwarebeschleunigung, wird standardmäßig in allen Anwendungen für Android 4.0 entwickelt. 
-   **Neue Data-APIs** – es gibt neue Zugriff auf Daten, die nicht zuvor offiziell zugegriffen werden, z. B. Kalenderdaten und das Benutzerprofil der Besitzer des Geräts konnte. 
-   **Freigabe von App** – Freigeben von Daten zwischen Anwendungen und Geräten ist jetzt einfacher als je über Technologien wie z. B. die `ShareActionProvider` , die ganz einfach zum Erstellen einer Freigabe Aktion aus einem Aktionsleiste und *Android Balken* für *Near Field Communications (NFC)* , ist daher ein Kinderspiel Daten auf Geräten in unmittelbarer Nähe zueinander gemeinsam nutzen. 


In diesem Artikel wir werden diese Funktionen und andere Änderungen, die an die Android 4.0-API vorgenommenen durchsuchen, und erläutern wir, wie jede Funktion mit Xamarin.Android verwenden.

## <a name="user-interface-features"></a>Benutzeroberflächen-Features

Eine Vielzahl neuer Benutzer Schnittstelle Technologien stehen mit Android 4, einschließlich:

-   **[GridLayout](~/android/user-interface/layouts/grid-layout.md)**  – unterstützt 2D Rasterlayout von Steuerelementen. 
-   **[Wechseln Sie Widget](~/android/user-interface/controls/switch.md)**  – ermöglicht das Umschalten zwischen ON oder OFF. 
-   **[TextureView](~/android/user-interface/controls/texture-view.md)**  – Video und OpenGL-Inhalte in einer Ansicht ermöglicht. 
-   **[Navigationsleiste](~/android/user-interface/controls/navigation-bar.md)**  – virtuelle Schaltflächen für die Sicherung, Heim und Multitasking enthält. 


Darüber hinaus andere Elemente der Benutzeroberfläche wurden verbessert, z. B. die `<a href"/guides/android/user_interface/popup_menus">PopupMenu</a>`, d. h. jetzt einfacher zu verwenden, und Registerkarten, wofür ein professionelles Aussehen.

## <a name="sharing-features"></a>Freigeben von Funktionen

Android 4 enthält mehrere neue Technologien, mit die wir die Daten auf Geräten und anwendungsübergreifend freigeben können. Darüber hinaus den Zugriff auf verschiedene Arten von Daten, die nicht zuvor verfügbar, z. B. Kalenderinformationen und Besitzer des Geräts Benutzerprofil waren. In diesem Abschnitt untersuchen wir werden eine Reihe von Features von Angeboten Android 4 dieser Adresse dieser Bereiche, einschließlich:

-  **[Android Balken](~/android/platform/android-beam.md)**  – ermöglicht die Datenfreigabe über NFC.
-   **[ShareActionProvider](~/android/user-interface/controls/action-bar.md)**  – erstellt einen Anbieter, ermöglicht Entwicklern das Freigeben von Aktionen in der Menüleiste Aktion angeben. 
-   **[Benutzerprofil](~/android/user-interface/user-profile.md)**  – ermöglicht den Zugriff auf die Profildaten der Besitzer des Geräts. 
-   **[API "Kalender"](~/android/user-interface/controls/calendar.md)**  – ermöglicht den Zugriff auf Kalenderdaten aus der Calendar-Anbieter. 

## <a name="x86-emulators"></a>X86 Emulatoren

ICS unterstützt noch keine Entwicklung mit dem x X86 Emulator. X86 Emulatoren werden nur mit Android 2.3.3, API-Ebene 10 unterstützt. Finden Sie unter [konfigurieren die X86 Emulator](~/android/get-started/installation/android-emulator/index.md) für Weitere Informationen.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel behandelt eine Vielzahl von neuen Technologien, die jetzt mit Android 4 verfügbar sind. Wir gesehen neue Benutzeroberflächen-Features wie z. B. die *GridLayout*, *PopupMenu*, und *Switch* Widget. Außerdem erläutert einige der neuen Unterstützung für die System-Benutzeroberfläche sowie Informationen zum Arbeiten mit steuern die *TextureView*. Klicken Sie dann wurde eine Vielzahl neuer Freigabe Technologien erläutert. Wir behandelt wie *Android Balken* wir Sie Informationen über Geräte, die gemeinsamen *NFC*, erläutert die neuen *Kalender-API*, und auch wurde gezeigt, wie die integrierte verwenden *ShareActionProvider*.
Zum Schluss wie untersucht die *ContactsContract* Zugriff Benutzerprofildaten-Anbieter.



## <a name="related-links"></a>Verwandte Links

- [Eis Rustikal Sandwich-Beispiele](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ICS_Samples/)
- [TextureViewDemo (Beispiel)](https://developer.xamarin.com/samples/monodroid/TextureViewDemo/)
- [CalendarDemo (Beispiel)](https://developer.xamarin.com/samples/monodroid/CalendarDemo/)
- [Registerkarte Layout-Lernprogramm](~/android/user-interface/layouts/tab-layout/index.md)
- [Eis Rustikal Sandwich](http://developer.android.com/about/versions/android-4.0-highlights.html)
- [Android 4.0 Platform](http://developer.android.com/about/versions/android-4.0.html)
