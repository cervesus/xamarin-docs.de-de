---
title: Toucheingabe und Gesten in xamarin. Android
description: Mit Touchscreens auf vielen der heutigen Geräte können Benutzer auf natürliche und intuitive Weise schnell und effizient mit Geräten interagieren. Diese Interaktion ist nicht nur auf einfache Berührungs Erkennung beschränkt. es ist auch möglich, Gesten zu verwenden. Beispielsweise ist die Stift-zu-Zoom-Geste ein sehr gängiges Beispiel hierfür, indem ein Teil des Bildschirms mit zwei Fingern fixiert wird, die der Benutzer vergrößern oder verkleinern kann. In dieser Anleitung werden toucheingaben und Gesten in Android untersucht.
ms.prod: xamarin
ms.assetid: 61874769-978A-4562-9B2A-7FFD45F58B38
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 43637d8592631b2732e5922544f52d91947dd3bd
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73024280"
---
# <a name="touch-and-gestures-in-xamarinandroid"></a>Toucheingabe und Gesten in xamarin. Android

_Mit Touchscreens auf vielen der heutigen Geräte können Benutzer auf natürliche und intuitive Weise schnell und effizient mit Geräten interagieren. Diese Interaktion ist nicht nur auf einfache Berührungs Erkennung beschränkt. es ist auch möglich, Gesten zu verwenden. Beispielsweise ist die Stift-zu-Zoom-Geste ein sehr gängiges Beispiel hierfür, indem ein Teil des Bildschirms mit zwei Fingern fixiert wird, die der Benutzer vergrößern oder verkleinern kann. In dieser Anleitung werden toucheingaben und Gesten in Android untersucht._

## <a name="touch-overview"></a>Übersicht über Touchscreen

IOS und Android sind in der Art und Weise vergleichbar, wie Sie die Fingereingabe behandeln. Beide können Multitouch-viele Kontaktpunkte auf dem Bildschirm und komplexe Gesten unterstützen. In diesem Handbuch werden einige der Ähnlichkeiten in den Konzepten sowie die Besonderheiten der Implementierung von toucheingaben und Gesten auf beiden Plattformen vorgestellt.

Android verwendet ein `MotionEvent` Objekt, um Touch-Daten zu kapseln, und Methoden für das Ansichts Objekt, um auf Berührungen zu lauschen.

Zusätzlich zur Erfassung von Touch-Daten bieten sowohl IOS als auch Android Mittel für das Interpretieren von Mustern von Berührungen in Gesten. Diese Gesten Erkennungs Tools können wiederum verwendet werden, um anwendungsspezifische Befehle zu interpretieren, z. b. eine Drehung eines Bilds oder eine Seite. Android bietet eine Reihe von unterstützten Gesten sowie Ressourcen, die das Hinzufügen komplexer benutzerdefinierter Gesten vereinfachen.

Unabhängig davon, ob Sie mit Android oder IOS arbeiten, kann die Wahl zwischen den Berührungen und Gesten erkenungen verwirrend sein. In dieser Anleitung wird empfohlen, dass Gesten Erkennungs Tools im allgemeinen bevorzugt werden. Gesten Erkennungs Tools werden als diskrete Klassen implementiert, die eine stärkere Trennung von Anliegen und eine bessere Kapselung bieten. Dies vereinfacht das Freigeben der Logik zwischen unterschiedlichen Ansichten und minimiert die Menge an geschriebenem Code.

Diese Anleitung folgt einem ähnlichen Format für jedes Betriebssystem: zuerst werden die touchapis der Plattform eingeführt und erläutert, da Sie die Grundlage dafür sind, dass touchinteraktionen erstellt werden. Anschließend werden wir uns mit der Welt der Gestenerkennung befassen – zuerst durch untersuchen einiger allgemeiner Gesten und Fertigstellung der Erstellung von benutzerdefinierten Gesten für Anwendungen. Schließlich erfahren Sie, wie Sie mit der Touch-Nachverfolgung auf niedriger Ebene einzelne Finger nachverfolgen, um ein Finger Paint-Programm zu erstellen.

## <a name="sections"></a>Abschnitte

- [Toucheingabe in Android](~/android/app-fundamentals/touch/android-touch-walkthrough.md)
- [Exemplarische Vorgehensweise: Verwenden von Touchscreen in Android](~/android/app-fundamentals/touch/android-touch-walkthrough.md)
- [Multi-Touch-Nachverfolgung](touch-tracking.md)

## <a name="summary"></a>Zusammenfassung

In dieser Anleitung haben wir die Berührungs Unterstützung in Android untersucht. Bei beiden Betriebssystemen haben wir gelernt, wie Sie die Fingereingabe aktivieren und auf die Berührungs Ereignisse reagieren. Als nächstes haben wir die Gesten und einige der Gesten erkentungen kennengelernt, die von Android und IOS bereitgestellt werden, um einige der gängigeren Szenarien zu bewältigen. Wir haben untersucht, wie benutzerdefinierte Gesten erstellt und in Anwendungen implementiert werden. In einer exemplarischen Vorgehensweise wurden die Konzepte und APIs für jedes Betriebssystem in Aktion veranschaulicht, und Sie haben auch gesehen, wie Sie einzelne Finger nachverfolgen können.

## <a name="related-links"></a>Verwandte Links

- [Android-Berührungs Start (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-touch-start)
- [Android-Berührungs Finale (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-touch-final)
- [Fingerpaint (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint)
