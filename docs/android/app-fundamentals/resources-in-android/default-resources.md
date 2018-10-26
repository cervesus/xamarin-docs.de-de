---
title: Standardressourcen
ms.prod: xamarin
ms.assetid: 762572F0-173A-D994-0510-8F36BEF3D487
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/13/2018
ms.openlocfilehash: 20865b71cce16f57b84a1c54986bd84180d3e190
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50107026"
---
# <a name="default-resources"></a>Standardressourcen

Standardressourcen sind Elemente, sind nicht spezifisch für jedes bestimmten Gerät oder den Formfaktor, und aus diesem Grund sind die Standardauswahl von Android-Betriebssystems Wenn kein spezifischer, Ressourcen gefunden werden. Daher sind die am häufigsten verwendete Typ des zu erstellenden Ressource. Diese Unterverzeichnisse unterteilt sind die **Ressourcen** Verzeichnis entsprechend ihren Ressourcentyp:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Standard-Ressourcendateien](default-resources-images/01-resource-files-vs.png)

In der obigen Abbildung weist das Projekt die Standardwerte für zeichenbare Ressourcen, Layouts und Werten (XML-Dateien, die einfache Werte enthalten).

Eine vollständige Liste der Ressourcentypen finden Sie weiter unten:

-  **Animator** &ndash; XML-Dateien, die Eigenschaftenanimationen beschreiben.
   Eigenschaftenanimationen wurden in API-Ebene 11 (Android 3.0) eingeführt und bietet für die Animation von Eigenschaften eines Objekts. Eigenschaftenanimationen sind flexibler und leistungsfähiger können Animationen auf jede Art von Objekt zu beschreiben.

-  **Anim** &ndash; XML-Dateien, die beschreiben, *Tween* Animationen. Tween Animationen sind eine Reihe von Animation-Anweisungen zum Ausführen von Transformationen auf den Inhalt einer Rotation anzeigen, Objekt oder beispielsweise ein Bild oder wächst die Größe des Texts. Tween Animationen sind beschränkt, nur Objekte anzeigen.

-  **Farbe** &ndash; XML-Dateien, die eine Liste der Status von Farben zu beschreiben. Um Farbe Zustand Listen zu verstehen, sollten Sie ein UI-Widget, z. B. eine Schaltfläche aus.
   Möglicherweise unterschiedliche Zustände wie z. B. gedrückt oder deaktiviert sind, und die Schaltfläche ändern die Farbe bei jeder Änderung im Zustand. Die Liste wird in der Liste einen Zustand angegeben.

-  **drawable** &ndash; zeichenbare Ressourcen sind ein generelles Konzept Grafiken, können werden in der Anwendung kompiliert und klicken Sie dann auf die von API-Aufrufe zugegriffen oder verweist auf andere XML-Ressourcen.
   Einige Beispiele für zeichenbarer Ressourcen sind Bitmapdateien (PNG, GIF, JPG), spezielle mit veränderbarer Größe Bitmaps, bekannt als [neun-Patches](https://developer.android.com/guide/topics/graphics/2d-graphics.html#nine-patch), Status listet, generische Formen, die in XML usw. definiert.
 
-  **Layout** &ndash; XML-Dateien, die ein Layout für die Benutzeroberfläche, z. B. eine Aktivität oder eine Zeile in einer Liste zu beschreiben.

-  **Menü** &ndash; XML-Dateien, die beschreiben, wie z. B. Anwendungsmenüs *Optionen Menüs*, *Kontextmenüs*, und *Untermenüs*. Ein Beispiel für Menüs, finden Sie unter den [Popup-Menü-Demo](https://developer.xamarin.com/samples/monodroid/PopupMenuDemo/) oder [Standardsteuerelemente](https://developer.xamarin.com/samples/mobile/StandardControls/) Beispiel.

-  **unformatierte** &ndash; beliebige Dateien, die in unformatierten, binäre Form gespeichert werden. Diese Dateien werden in einer Android-Anwendung in einem binären Format kompiliert.

-  **Werte** &ndash; XML-Dateien, die einfache Werte enthalten. Eine XML-Datei im Verzeichnis Werte definiert eine einzelne Ressource nicht, aber stattdessen kann mehrere Ressourcen definieren. Kann z. B. eine XML-Datei die eine Liste von Zeichenfolgenwerten, halten, während eine andere XML-Datei eine Liste von Farbwerten enthalten kann.

-  **XML** &ndash; XML-Dateien, die gleiche Funktion wie die Dateien für die Konfiguration sind. Hierbei handelt es sich um beliebige XML, die von der Anwendung zur Laufzeit gelesen werden kann.


# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![Standard-Ressourcendateien](default-resources-images/01-resource-files-xs.png)

In der obigen Abbildung weist das Projekt die Standardwerte für zeichenbare Ressourcen, Layouts und Werten (XML-Dateien, die einfache Werte enthalten).

Eine vollständige Liste der Ressourcentypen finden Sie weiter unten:

-  **Animator** &ndash; XML-Dateien, die Eigenschaftenanimationen beschreiben.
   Eigenschaftenanimationen wurden in API-Ebene 11 (Android 3.0) eingeführt und bietet für die Animation von Eigenschaften eines Objekts. Eigenschaftenanimationen sind flexibler und leistungsfähiger können Animationen auf jede Art von Objekt zu beschreiben.

-  **Anim** &ndash; XML-Dateien, die beschreiben, *Tween* Animationen. Tween Animationen sind eine Reihe von Animation-Anweisungen zum Ausführen von Transformationen auf den Inhalt einer Rotation anzeigen, Objekt oder beispielsweise ein Bild oder wächst die Größe des Texts. Tween Animationen sind beschränkt, nur Objekte anzeigen.

-  **Farbe** &ndash; XML-Dateien, die eine Liste der Status von Farben zu beschreiben. Um Farbe Zustand Listen zu verstehen, sollten Sie ein UI-Widget, z. B. eine Schaltfläche aus.
   Es kann verschiedene Zustände wie z. B. gedrückt oder deaktiviert haben, und die Schaltfläche ändern die Farbe bei jeder Änderung im Zustand. Die Liste wird in der Liste einen Zustand angegeben.

-  **Schriftart** &ndash; ab API-Ebene 26, es ist möglich, Einbetten von Schriftarten als Ressource in einer Android-Anwendung. Die Support-Bibliothek-26 werden Backport-Schriftarten zum API-Ebene 14. Einbetten von Schriftarten kann Anwendungen benutzerdefinierte Schriftarten direkt aus dem XML-Layouts zu laden, ohne diese als Ressourcen vor der Verwendung zu importieren.

-  **MipMap** &ndash; zeichenbare Ressourcen sind ein generelles Konzept Grafiken, können werden in der Anwendung kompiliert und klicken Sie dann auf die von API-Aufrufe zugegriffen oder verweist auf andere XML-Ressourcen.
   Einige Beispiele für zeichenbarer Ressourcen sind Bitmapdateien (PNG, GIF, JPG), spezielle mit veränderbarer Größe Bitmaps, bekannt als [neun-Patches](https://developer.android.com/guide/topics/graphics/2d-graphics.html#nine-patch), Status listet, generische Formen, die in XML usw. definiert.

-  **Layout** &ndash; XML-Dateien, die ein Layout für die Benutzeroberfläche, z. B. eine Aktivität oder eine Zeile in einer Liste zu beschreiben.

-  **Menü** &ndash; XML-Dateien, die beschreiben, wie z. B. Anwendungsmenüs *Optionen Menüs*, *Kontextmenüs*, und *Untermenüs*. Ein Beispiel für Menüs, finden Sie unter den [Popup-Menü-Demo](https://developer.xamarin.com/samples/monodroid/PopupMenuDemo/) oder [Standardsteuerelemente](https://developer.xamarin.com/samples/mobile/StandardControls/) Beispiel.

-  **unformatierte** &ndash; beliebige Dateien, die in unformatierten, binäre Form gespeichert werden. Diese Dateien werden in einer Android-Anwendung in einem binären Format kompiliert.

-  **Werte** &ndash; XML-Dateien, die einfache Werte enthalten. Eine XML-Datei im Verzeichnis Werte definiert eine einzelne Ressource nicht, aber stattdessen kann mehrere Ressourcen definieren. Kann z. B. eine XML-Datei die eine Liste von Zeichenfolgenwerten, halten, während eine andere XML-Datei eine Liste von Farbwerten enthalten kann.

-  **XML** &ndash; XML-Dateien, die gleiche Funktion wie die Dateien für die Konfiguration sind. Hierbei handelt es sich um beliebige XML, die von der Anwendung zur Laufzeit gelesen werden können

-----
