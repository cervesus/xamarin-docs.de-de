---
title: Touch- und Gesten in Xamarin.Android
description: Touchscreens auf viele der heutigen Geräte ermöglichen Benutzern Geräten auf eine natürliche und intuitive Weise schnell und effizient interagieren. Diese Aktivität ist nicht nur auf einfache Touch Erkennung beschränkt – es ist möglich, auch Gesten verwenden. Zwei-Finger-Zoom Bewegung ist z. B. ein sehr gängiges Beispiel dieser durch einen Teil des Bildschirms mit zwei Fingern fest, denen der Benutzer vergrößern oder verkleinern kann pinching an. Dieses Handbuch Touch untersucht und Aktionen in Android.
ms.prod: xamarin
ms.assetid: 61874769-978A-4562-9B2A-7FFD45F58B38
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 3700f9c73fb58fefcdba7987c9931e385cd52d38
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/03/2018
ms.locfileid: "32436289"
---
# <a name="touch-and-gestures-in-xamarinandroid"></a>Touch- und Gesten in Xamarin.Android

_Touchscreens auf viele der heutigen Geräte ermöglichen Benutzern Geräten auf eine natürliche und intuitive Weise schnell und effizient interagieren. Diese Aktivität ist nicht nur auf einfache Touch Erkennung beschränkt – es ist möglich, auch Gesten verwenden. Zwei-Finger-Zoom Bewegung ist z. B. ein sehr gängiges Beispiel dieser durch einen Teil des Bildschirms mit zwei Fingern fest, denen der Benutzer vergrößern oder verkleinern kann pinching an. Dieses Handbuch Touch untersucht und Aktionen in Android._

## <a name="touch-overview"></a>Touch-Übersicht

iOS und Android werden ähnlich wie die Methoden, die sie Touch behandeln. Beide können Multitouch - viele Punkte des Kontakts auf dem Bildschirm- und komplexe Gesten unterstützen. Dieser Leitfaden führt einige der ähnlichkeiten Konzepte sowie den Besonderheiten des Implementierens Touch und Aktionen auf beiden Plattformen.

Android verwendet einen `MotionEvent` Objekt zum Kapseln von Touch-Daten und Methoden für das Objekt anzeigen, um den letzten Schliff zu lauschen.

IOS und Android bereitstellen bedeutet, dass zusätzlich zum Erfassen der Touch-Daten, für das Muster der Fingereingaben in Gesten interpretieren. Diese Bewegung Prüfer können wiederum zum Interpretieren von anwendungsspezifischen Befehle, wie z. B. eine Drehung ein Bild oder eine Deaktivieren einer Seite verwendet werden. Android bietet eine Handvoll unterstützten Gesten sowie Ressourcen zum Hinzufügen von benutzerdefinierten Gesten einfach komplexe vornehmen.

Ob Sie auf Android oder iOS ausgeführt werden, kann die Wahl zwischen Fingereingaben sowie Geste Prüfer eine verwirrend sein. Es wird empfohlen, im Allgemeinen Voreinstellung für die Geste Prüfer gewährt werden soll. Prüfer Geste werden als diskrete Klassen implementiert, die wodurch die stärkere Trennung von Anliegen und eine bessere Kapselung bereitzustellen. Dies vereinfacht die Logik zwischen verschiedenen Ansichten, minimieren die Menge an Code geschrieben freigeben.

Dieses Handbuch folgt ein ähnlichen Formats wie für jedes Betriebssystem: zuerst wird die Plattform-Touch-APIs sind eingeführt und erläutert, wie sie die Grundlage bilden, auf welche Touch Aktivitäten erstellt werden. Klicken Sie dann, befassen wir uns mit der Welt der Gestenhandler Prüfer – zuerst durch einige allgemeine Gesten durchsuchen und mit dem Erstellen von benutzerdefinierter Gesten für Anwendungen wird abgeschlossen. Abschließend sehen Sie, wie einzelne Finger mithilfe der Low-Level-Touch-nachverfolgung So erstellen Sie ein Programm Finger-paint nachverfolgt.

## <a name="sections"></a>Abschnitte

-  [Toucheingabe in Android](~/android/app-fundamentals/touch/android-touch-walkthrough.md)
-  [Exemplarische Vorgehensweise: Verwenden von Touch in Android](~/android/app-fundamentals/touch/android-touch-walkthrough.md)
-  [Multi-Touch-Überwachung](touch-tracking.md)

## <a name="summary"></a>Zusammenfassung

In diesem Handbuch untersucht wir Touch Android. Für beide Betriebssysteme haben wir wie durch Toucheingabe zu aktivieren und So reagieren Sie auf die Berührungsereignisse. Als Nächstes kennen gelernt Gesten und einige der Gestenhandler Prüfer, Android und iOS bereitstellen, um einige der häufigen Szenarien behandelt. Untersucht das Erstellen von benutzerdefinierter Gesten und in Anwendungen zu implementieren. Eine exemplarische Vorgehensweise veranschaulicht die Konzepte und APIs für jedes Betriebssystem in Aktion, und Sie wurde auch erläutert, wie einzelne Finger nachverfolgen.



## <a name="related-links"></a>Verwandte Links

- [Android berühren starten (Beispiel)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_start)
- [Android berühren endgültigen (Beispiel)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_final)
- [FingerPaint (Beispiel)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint)
