---
title: Standardressourcen
ms.topic: article
ms.prod: xamarin
ms.assetid: 762572F0-173A-D994-0510-8F36BEF3D487
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 3d9c747cdf8e43f33b9310ac1156550066b400eb
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="default-resources"></a>Standardressourcen

Standardressourcen sind Elemente, die nicht auf bestimmte Geräte oder Formfaktor spezifisch sind und daher die Standardauswahl von Android-Betriebssysteme Wenn kein spezifischere Ressourcen gefunden werden. Daher sind die am häufigsten verwendete Typ der Ressource zu erstellen. Sie sind in Unterverzeichnissen von organisiert die **Ressourcen** Verzeichnis gemäß ihren Ressourcentyp:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Standardressourcendateien](default-resources-images/01-resource-files-vs.png)

In der obigen Abbildung wurde das Projekt Standardwerte für zeichenbaren Ressourcen, Layouts und Werten (XML-Dateien, die einfache Werte enthalten).

Eine vollständige Liste der Ressourcentypen finden Sie weiter unten:

-  **Animator** &ndash; XML-Dateien, die Eigenschaftenanimationen beschreiben.
   Eigenschaftenanimationen in API-Ebene 11 (Android 3.0) eingeführt wurden und für die Animation von Eigenschaften für ein Objekt enthält. Eigenschaftenanimationen sind eine Möglichkeit flexibler und leistungsfähiger Animationen auf jede Art von Objekt zu beschreiben.

-  **Anim** &ndash; XML-Dateien, die beschreiben, *Tween* Animationen. Tween Animationen handelt es sich um eine Reihe von Animation Anweisungen zum Ausführen von Transformationen auf dem Inhalt einer Drehung anzeigen, Objekt oder beispielsweise ein Bild oder wächst die Größe des Texts. Tween Animationen sind beschränkt, nur die Objekte anzuzeigen.

-  **Farbe** &ndash; XML-Dateien, die eine Liste der Status von Farben zu beschreiben. Um Farbe Zustand Listen zu verstehen, sollten Sie ein UI-Widget, z. B. eine Schaltfläche aus.
   Erleichtern können verschiedenen Zustände, wie z. B. gedrückt wird oder deaktiviert haben, und die Schaltfläche ändern die Farbe bei jeder Änderung im Zustand. Die Liste wird in eine Status-Liste angegeben.

-  **zeichenbaren** &ndash; zeichenbare Ressourcen sind eine allgemeine Konzept für die Grafiken, die können werden in die Anwendung kompiliert und dann auf die API-Aufrufe zugreift oder verweist auf andere XML-Ressourcen.
   Einige Beispiele für Drawables sind Bitmap-Dateien (PNG, GIF, JPG), spezielle in der Größe veränderbaren Bitmaps genannt [neun Patches](https://developer.android.com/guide/topics/graphics/2d-graphics.html#nine-patch), Status Listet allgemeine Formen, die in XML usw. definiert.
 
-  **Layout** &ndash; XML-Dateien, die ein Layout für die Benutzeroberfläche, z. B. eine Aktivität oder eine Zeile in einer Liste zu beschreiben.

-  **Menü** &ndash; XML-Dateien, die beschreiben, wie z. B. Anwendungsmenüs *Optionen Menüs*, *Kontextmenüs*, und *Untermenüs*. Ein Beispiel für Menüs, finden Sie unter der [Popup-Menü Demo](https://developer.xamarin.com/samples/monodroid/PopupMenuDemo/) oder [Standardsteuerelemente](https://developer.xamarin.com/samples/mobile/StandardControls/) Beispiel.

-  **unformatierte** &ndash; beliebige Dateien, die in ihrer Roh, binäre Form gespeichert werden. Diese Dateien werden in einer Android-Anwendung in einem binären Format kompiliert.

-  **Werte** &ndash; XML-Dateien, die einfache Werte enthalten. Eine XML-Datei im Verzeichnis Werte eine einzelne Ressource ist nicht definiert, aber Sie können mehrere Ressourcen stattdessen definieren. Beispielsweise kann eine XML-Datei eine Liste von Zeichenfolgenwerten, halten, während eine andere XML-Datei eine Liste von Farbwerten enthalten kann.

-  **XML** &ndash; XML-Dateien, die gleiche Funktion wie die Konfigurationsdateien .NET sind. Hierbei handelt es sich um beliebigen XML-Code, der zur Laufzeit von der Anwendung gelesen werden kann


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

![Standardressourcendateien](default-resources-images/01-resource-files-xs.png)

In der obigen Abbildung wurde das Projekt Standardwerte für zeichenbaren Ressourcen, Layouts und Werten (XML-Dateien, die einfache Werte enthalten).

Eine vollständige Liste der Ressourcentypen finden Sie weiter unten:

-  **Animator** &ndash; XML-Dateien, die Eigenschaftenanimationen beschreiben.
   Eigenschaftenanimationen in API-Ebene 11 (Android 3.0) eingeführt wurden und für die Animation von Eigenschaften für ein Objekt enthält. Eigenschaftenanimationen sind eine Möglichkeit flexibler und leistungsfähiger Animationen auf jede Art von Objekt zu beschreiben.

-  **Anim** &ndash; XML-Dateien, die beschreiben, *Tween* Animationen. Tween Animationen handelt es sich um eine Reihe von Animation Anweisungen zum Ausführen von Transformationen auf dem Inhalt einer Drehung anzeigen, Objekt oder beispielsweise ein Bild oder wächst die Größe des Texts. Tween Animationen sind beschränkt, nur die Objekte anzuzeigen.

-  **Farbe** &ndash; XML-Dateien, die eine Liste der Status von Farben zu beschreiben. Um Farbe Zustand Listen zu verstehen, sollten Sie ein UI-Widget, z. B. eine Schaltfläche aus.
   Erleichtern können verschiedenen Zustände, wie z. B. gedrückt wird oder deaktiviert haben, und die Schaltfläche ändern die Farbe bei jeder Änderung im Zustand. Die Liste wird in eine Status-Liste angegeben.

-  **Schriftart** &ndash; ab API-Ebene 26, es ist möglich, Einbetten von Schriftarten als Ressource in einer Android-Anwendung. Der Support-Bibliothek 26 wird Backport Schriftarten zum API-Ebene 14. Einbetten von Schriftarten kann Anwendungen

-  **MipMap** &ndash; zeichenbare Ressourcen sind eine allgemeine Konzept für die Grafiken, die können werden in die Anwendung kompiliert und dann auf die API-Aufrufe zugreift oder verweist auf andere XML-Ressourcen.
   Einige Beispiele für Drawables sind Bitmap-Dateien (PNG, GIF, JPG), spezielle in der Größe veränderbaren Bitmaps genannt [neun Patches](https://developer.android.com/guide/topics/graphics/2d-graphics.html#nine-patch), Status Listet allgemeine Formen, die in XML usw. definiert.

-  **Layout** &ndash; XML-Dateien, die ein Layout für die Benutzeroberfläche, z. B. eine Aktivität oder eine Zeile in einer Liste zu beschreiben.

-  **Menü** &ndash; XML-Dateien, die beschreiben, wie z. B. Anwendungsmenüs *Optionen Menüs*, *Kontextmenüs*, und *Untermenüs*. Ein Beispiel für Menüs, finden Sie unter der [Popup-Menü Demo](https://developer.xamarin.com/samples/monodroid/PopupMenuDemo/) oder [Standardsteuerelemente](https://developer.xamarin.com/samples/mobile/StandardControls/) Beispiel.

-  **unformatierte** &ndash; beliebige Dateien, die in ihrer Roh, binäre Form gespeichert werden. Diese Dateien werden in einer Android-Anwendung in einem binären Format kompiliert.

-  **Werte** &ndash; XML-Dateien, die einfache Werte enthalten. Eine XML-Datei im Verzeichnis Werte eine einzelne Ressource ist nicht definiert, aber Sie können mehrere Ressourcen stattdessen definieren. Beispielsweise kann eine XML-Datei eine Liste von Zeichenfolgenwerten, halten, während eine andere XML-Datei eine Liste von Farbwerten enthalten kann.

-  **XML** &ndash; XML-Dateien, die gleiche Funktion wie die Konfigurationsdateien .NET sind. Hierbei handelt es sich um beliebigen XML-Code, der zur Laufzeit von der Anwendung gelesen werden kann

-----
