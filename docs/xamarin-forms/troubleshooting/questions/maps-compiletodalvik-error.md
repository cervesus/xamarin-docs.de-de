---
title: Warum schlägt mein Xamarin.Forms.Maps-Android-Projekt mit COMPILETODALVIK Fehler der obersten Ebene fehl?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C0251EB1-F509-47AD-98D6-846AF46425E5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 9df9e348440b9dd4b18b3859d64cbe47bd05b24c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61250457"
---
# <a name="why-does-my-xamarinformsmaps-android-project-fail-with-compiletodalvik-unexpected-top-level-error"></a>Warum schlägt mein Xamarin.Forms.Maps-Android-Projekt mit COMPILETODALVIK Fehler der obersten Ebene fehl?

Dieser Fehler kann in der Fehlerbereich von Visual Studio für Mac oder im Fenster "Buildausgabe" von Visual Studio angezeigt werden; in der Android-Projekte mit Xamarin.Forms.Maps.

Dies wird meist durch das Erhöhen der Java-Heapgröße für Xamarin.Android-Projekt aufgelöst. Um die Größe des Heaps zu erhöhen, gehen Sie wie folgt vor:

## <a name="visual-studio"></a>Visual Studio

1. Mit der rechten Maustaste in des Android-Projekts, und öffnen Sie die Optionen für das Projekt.
2. Wechseln Sie zu **Android-Optionen -> erweiterte**
3. Geben Sie im Textfeld Java Heap Size 1G aus.
4. Erstellen Sie das Projekt neu.

![Screenshot der Optionen für das Visual Studio-Projekt](maps-compiletodalvik-error-images/vsjavaheap.png "Buildoptionen für Android in Visual Studio")

## <a name="visual-studio-for-mac"></a>Visual Studio für Mac

1.  Mit der rechten Maustaste in des Android-Projekts, und öffnen Sie die Optionen für das Projekt.
2.  Wechseln Sie zu **erstellen -> Android-Build -> erweiterte**
3.  Geben Sie im Textfeld Java Heap Size 1G aus.
4.  Erstellen Sie das Projekt neu.  

![Screenshot der Visual Studio für Mac – Projektoptionen](maps-compiletodalvik-error-images/xsjavaheap.png "Android Buildoptionen in Visual Studio für Mac")

