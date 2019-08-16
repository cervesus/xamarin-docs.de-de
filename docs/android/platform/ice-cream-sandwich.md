---
title: Ice Cream Sandwich-Features
description: In diesem Artikel werden einige der neuen Features beschrieben, die Anwendungsentwicklern mit dem Android 4-API-Ice-Sandwich zur Verfügung stehen. Es umfasst mehrere neue Technologien für die Benutzeroberfläche und prüft dann eine Reihe neuer Funktionen, die Android 4 für die gemeinsame Nutzung von Daten zwischen Anwendungen und Geräten bietet.
ms.prod: xamarin
ms.assetid: 78E18A62-C12F-A699-37FA-44B9F6B44273
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: f8d9841deff485a67919aea9fede75044541ba5f
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69524196"
---
# <a name="ice-cream-sandwich-features"></a>Ice Cream Sandwich-Features

_In diesem Artikel werden einige der neuen Features beschrieben, die Anwendungsentwicklern mit dem Android 4-API-Ice-Sandwich zur Verfügung stehen. Es umfasst mehrere neue Technologien für die Benutzeroberfläche und prüft dann eine Reihe neuer Funktionen, die Android 4 für die gemeinsame Nutzung von Daten zwischen Anwendungen und Geräten bietet._

## <a name="overview"></a>Übersicht

Die Android-Betriebs System Version 4,0 (API-Ebene 14) stellt eine größere Funktionsweise des Android-Betriebssystems dar und umfasst eine Reihe wichtiger Änderungen und Upgrades, einschließlich der folgenden:

- **Aktualisierte Benutzeroberfläche** – mit mehreren neuen Benutzeroberflächen Features können Entwickler beim Erstellen von Anwendungs Benutzeroberflächen mehr Leistung und Flexibilität erzielen. Zu diesen neuen Features gehören `GridLayout` : `PopupMenu` , `Switch` , Widget und `TextureView` . 
- **Bessere Hardware Beschleunigung** – das 2D-Rendering erfolgt nun auf der GPU für alle Android-Steuerelemente. Außerdem ist die Hardwarebeschleunigung standardmäßig in allen Anwendungen aktiviert, die für Android 4,0 entwickelt wurden. 
- **Neue Daten-APIs** – es gibt neuen Zugriff auf Daten, die zuvor nicht offiziell zugänglich waren, wie z. b. Kalenderdaten und das Benutzerprofil des Geräte Besitzers. 
- **App-Datenfreigabe** – das Freigeben von Daten zwischen Anwendungen und Geräten ist jetzt einfacher als je zuvor über `ShareActionProvider` Technologien wie das, was das Erstellen einer Freigabe Aktion aus einer Aktionsleiste und *Android-Beam* für *Near Field erleichtert. Kommunikation (NFC)* , wodurch es zu einem Snap-in ist, Daten auf mehreren Geräten in unmittelbarer Nähe zueinander zu teilen. 


In diesem Artikel untersuchen wir diese Features und andere Änderungen, die an der Android 4,0-API vorgenommen wurden. Außerdem wird erläutert, wie Sie die einzelnen Features mit xamarin. Android verwenden.

## <a name="user-interface-features"></a>Benutzeroberflächen Funktionen

Eine Reihe von neuen Technologien für die Benutzeroberfläche ist mit Android 4 verfügbar, einschließlich:

- **[GridLayout](~/android/user-interface/layouts/grid-layout.md)** – unterstützt das 2D-Raster Layout von Steuerelementen. 
- **[Switchwidget](~/android/user-interface/controls/switch.md)** – ermöglicht das Umschalten zwischen ein-oder ausschalten. 
- **[Textureview](~/android/user-interface/controls/texture-view.md)** – aktiviert Video-und OpenGL-Inhalte in einer Ansicht. 
- **[Navigationsleiste](~/android/user-interface/controls/navigation-bar.md)** – enthält virtuelle Schaltflächen für "Back", "Home" und "Multitasking". 


Außerdem wurden andere Elemente der Benutzeroberfläche verbessert, wie z. `<a href"/guides/android/user_interface/popup_menus">PopupMenu</a>`b. die, die nun einfacher zu bearbeiten ist, und Registerkarten, die eine präziere Darstellung aufweisen.

## <a name="sharing-features"></a>Freigabe Features

Android 4 umfasst mehrere neue Technologien, mit denen Sie Daten Geräte übergreifend und Anwendungs übergreifend freigeben können. Außerdem wird der Zugriff auf verschiedene Datentypen ermöglicht, die zuvor nicht verfügbar waren, wie z. b. Kalenderinformationen und das Benutzerprofil des Geräte Besitzers. In diesem Abschnitt untersuchen wir eine Vielzahl von Features, die von Android 4 angeboten werden, die die folgenden Bereiche erfüllen:

- **[Android-Beam](~/android/platform/android-beam.md)** – ermöglicht die gemeinsame Nutzung von Daten über NFC.
- **[Shareaktionprovider](~/android/user-interface/controls/action-bar.md)** – erstellt einen Anbieter, der es Entwicklern ermöglicht, Freigabe Aktionen aus dem Aktionsleiste anzugeben. 
- **[Benutzerprofil](~/android/user-interface/user-profile.md)** – bietet Zugriff auf Profildaten des Geräte Besitzers. 
- **[Kalender-API](~/android/user-interface/controls/calendar.md)** – bietet Zugriff auf Kalenderdaten vom Kalender Anbieter. 

## <a name="x86-emulators"></a>x86-Emulatoren

ICS unterstützt die Entwicklung mit einem x86-Emulator noch nicht. x86-Emulatoren werden nur mit Android 2.3.3, API-Ebene 10 unterstützt. Weitere Informationen finden Sie [unter Konfigurieren des x86-Emulators](~/android/get-started/installation/android-emulator/index.md) .

## <a name="summary"></a>Zusammenfassung

Dieser Artikel behandelt eine Reihe von neuen Technologien, die nun mit Android 4 verfügbar sind. Wir haben neue Features für die Benutzeroberfläche, wie z. b. *GridLayout*, *PopupMenu*und *Switch* -Widget, geprüft. Wir haben auch einige der neuen Unterstützung für die Steuerung der Systembenutzer Oberfläche und die Verwendung von *textureview*behandelt. Anschließend haben wir eine Reihe von neuen Technologien für die gemeinsame Nutzung erläutert. Wir haben erläutert, wie Sie mit *Android-Beam* Informationen auf allen Geräten freigeben können, die *NFC*verwenden, die neue *Kalender-API*besprochen und die Verwendung des integrierten *shareaktionsanbieters*gezeigt haben.
Schließlich wurde untersucht, wie der *contactscontract* -Anbieter verwendet wird, um auf Benutzerprofil Daten zuzugreifen.



## <a name="related-links"></a>Verwandte Links

- [Ice Cream Sandwich-Beispiele](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-ics-samples)
- [Textureviewdemo (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/textureviewdemo)
- [Calendardemo (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/calendardemo)
- [Tutorial zur Registerkarten Layout](~/android/user-interface/layouts/tab-layout/index.md)
- [Ice Cream Sandwich](https://developer.android.com/about/versions/android-4.0-highlights.html)
- [Android 4,0-Plattform](https://developer.android.com/about/versions/android-4.0.html)
