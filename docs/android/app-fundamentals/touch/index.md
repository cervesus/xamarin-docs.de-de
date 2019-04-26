---
title: Berührung und Gesten in Xamarin.Android
description: Touchscreens auf viele der heutigen Geräte können Benutzer schnell und effizient mit Geräten auf eine natürliche und intuitive Weise zu interagieren. Diese Interaktion ist nicht beschränkt, nur um einfache Touch-Erkennung – Es ist möglich, Gesten und zu verwenden. Die Pinch-Finger-Zoom-Bewegung wird z. B. ein sehr gängiges Beispiel hierfür von möglichen einen Teil des Bildschirms mit zwei Fingern, die, denen der Benutzer vergrößern oder verkleinern kann. Dieses Handbuch Touch untersucht und anwendungsstiftbewegungen, die unter Android.
ms.prod: xamarin
ms.assetid: 61874769-978A-4562-9B2A-7FFD45F58B38
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 7f957c9ff5a0e7c3a0821978703860ed2f723a92
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61013124"
---
# <a name="touch-and-gestures-in-xamarinandroid"></a>Berührung und Gesten in Xamarin.Android

_Touchscreens auf viele der heutigen Geräte können Benutzer schnell und effizient mit Geräten auf eine natürliche und intuitive Weise zu interagieren. Diese Interaktion ist nicht beschränkt, nur um einfache Touch-Erkennung – Es ist möglich, Gesten und zu verwenden. Die Pinch-Finger-Zoom-Bewegung wird z. B. ein sehr gängiges Beispiel hierfür von möglichen einen Teil des Bildschirms mit zwei Fingern, die, denen der Benutzer vergrößern oder verkleinern kann. Dieses Handbuch Touch untersucht und anwendungsstiftbewegungen, die unter Android._

## <a name="touch-overview"></a>Übersicht über die Fingereingabe

iOS und Android sind ähnlich wie, die Sie Touch behandeln. Beide können Multi-Touch - viele Datenpunkte wenden Sie sich an, auf dem Bildschirm – und komplexe Gesten unterstützen. Dieses Handbuch führt einige der die ähnlichkeiten-Konzepte sowie den Besonderheiten der Implementierung von Touch und anwendungsstiftbewegungen, die auf beiden Plattformen.

Android verwendet einen `MotionEvent` Objekt, das Touch-Daten und Methoden für das Objekt zum Lauschen auf berührungen zu kapseln.

IOS und Android bieten bedeutet, dass neben dem Erfassen von Touch-Daten, für das Interpretieren der Muster der Workflows in die Eingabeaktionen. Diese Geste Erkennungen können wiederum verwendet werden, um anwendungsspezifische-Befehle, wie eine Drehung um ein Bild oder eine Reihe von einer Seite zu interpretieren. Android bietet eine Reihe von unterstützten Gesten sowie Ressourcen für Hinzufügen von komplexen benutzerdefinierten Gesten einfach zur Verfügung.

Ob Sie auf Android oder iOS arbeiten, kann die Wahl zwischen Workflows und Bewegung Erkennungen eine verwirrend sein. Es wird empfohlen, im Allgemeinen Präferenz zu Geste Erkennungen gewährt werden soll. Geste Erkennungen werden als diskrete Klassen implementiert, die stärkere Trennung von Belangen und eine bessere Kapselung bereitstellen. Dies erleichtert Ihnen die Logik zwischen verschiedenen Ansichten, minimieren die Menge an geschriebenen Code freigeben.

Dieses Handbuch folgt ein ähnlichen Formats wie für jedes Betriebssystem: "First", der Plattform Touch APIs eingeführt und beschrieben, wie sie die Grundlage sind, auf welche Touch Interaktionen erstellt werden. Klicken Sie dann befassen wir uns in die Welt der Bewegung Erkennungen: zuerst durch einige allgemeine Gesten untersuchen und mit dem Erstellen von benutzerdefinierter stiftbewegungen für Anwendungen wird abgeschlossen. Zum Schluss sehen Sie einzelner Finger, die mithilfe der nachverfolgung von Low-Level-Touch, erstellen Sie ein Programm Finger-paint nachverfolgen.

## <a name="sections"></a>Abschnitte

-  [Toucheingabe in Android](~/android/app-fundamentals/touch/android-touch-walkthrough.md)
-  [Exemplarische Vorgehensweise: Verwenden von Toucheingaben in Android](~/android/app-fundamentals/touch/android-touch-walkthrough.md)
-  [Multitouch-nachverfolgung](touch-tracking.md)

## <a name="summary"></a>Zusammenfassung

In diesem Handbuch untersucht es von toucheingaben in Android. Für beide Betriebssysteme haben Sie erfahren, wie Toucheingabe zu aktivieren und auf die touchereignisse zu reagieren. Als Nächstes haben wir Gesten und einige der Bewegung Erkennungsprogramme, Android und iOS bereitstellen, um einige der häufigsten Szenarien behandeln. Untersuchten wir das Erstellen von benutzerdefinierter stiftbewegungen, und sie in Anwendungen implementieren. Eine exemplarische Vorgehensweise veranschaulicht die Konzepte und APIs für jedes Betriebssystem in Aktion, und Sie haben auch erfahren, wie einzelner Finger nachverfolgen.



## <a name="related-links"></a>Verwandte Links

- [Android Touch starten (Beispiel)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_start)
- [Android Touch endgültige (Beispiel)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_final)
- [FingerPaint (Beispiel)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint)
