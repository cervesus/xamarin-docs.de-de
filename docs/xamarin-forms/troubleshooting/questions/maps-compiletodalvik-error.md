---
title: Warum fehl Meine Xamarin.Forms.Maps Android-Projekt mit COMPILETODALVIK Fehler der obersten Ebene?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C0251EB1-F509-47AD-98D6-846AF46425E5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 9df9e348440b9dd4b18b3859d64cbe47bd05b24c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="why-does-my-xamarinformsmaps-android-project-fail-with-compiletodalvik-unexpected-top-level-error"></a>Warum fehl Meine Xamarin.Forms.Maps Android-Projekt mit COMPILETODALVIK Fehler der obersten Ebene?

Dieser Fehler kann in das Auffüllzeichen Fehler von Visual Studio für Mac oder im Fenster Ausgabe Erstellen von Visual Studio angezeigt werden; in der Android-Projekte mit Xamarin.Forms.Maps.

Dies wird am häufigsten durch Erhöhen der Java-Heapgröße für Ihr Projekt Xamarin.Android aufgelöst. Führen Sie diese Schritte aus, um die Größe des Heaps zu erhöhen:

## <a name="visual-studio"></a>Visual Studio

1. Mit der rechten Maustaste des Android-Projekts, und öffnen Sie die Optionen für das Projekt.
2. Wechseln Sie zu **Android Optionen -> erweiterte**
3. Geben Sie im Textfeld Java Heap Größe 1G aus.
4. Erstellen Sie das Projekt neu.

![Screenshot der Optionen für die Visual Studio-Projekt](maps-compiletodalvik-error-images/vsjavaheap.png "Buildoptionen für Android in Visual Studio")

## <a name="visual-studio-for-mac"></a>Visual Studio für Mac

1.  Mit der rechten Maustaste des Android-Projekts, und öffnen Sie die Optionen für das Projekt.
2.  Wechseln Sie zu **Android Build erstellen -> -> erweiterte**
3.  Geben Sie im Textfeld Java Heap Größe 1G aus.
4.  Erstellen Sie das Projekt neu.  

![Screenshot von Visual Studio für Mac-Projektoptionen](maps-compiletodalvik-error-images/xsjavaheap.png "Android Buildoptionen in Visual Studio für Mac")

