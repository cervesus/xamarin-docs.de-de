---
title: Standardressourcen
ms.prod: xamarin
ms.assetid: 762572F0-173A-D994-0510-8F36BEF3D487
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/13/2018
ms.openlocfilehash: 7375121375cce3927e495dba33fe140f936ad0bd
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73025073"
---
# <a name="default-resources"></a>Standardressourcen

Standard Ressourcen sind Elemente, die nicht spezifisch für ein bestimmtes Gerät oder einen bestimmten Formular Faktor sind. Sie sind daher die Standardoption für das Android-Betriebssystem, wenn keine spezifischeren Ressourcen gefunden werden können. Daher handelt es sich um den am häufigsten verwendeten Ressourcentyp, der erstellt werden soll. Sie sind gemäß ihrem Ressourcentyp in Unterverzeichnissen des **Ressourcen** Verzeichnisses organisiert:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Standard Ressourcen Dateien](default-resources-images/01-resource-files-vs.png)

In der obigen Abbildung verfügt das Projekt über Standardwerte für drawable-Ressourcen, Layouts und Werte (XML-Dateien, die einfache Werte enthalten).

Eine komplette Liste der Ressourcentypen finden Sie unten:

- **Animator** &ndash; XML-Dateien, die Eigenschafts Animationen beschreiben.
   Eigenschafts Animationen wurden auf API-Ebene 11 (Android 3,0) eingeführt und stellen die Animation von Eigenschaften für ein Objekt bereit. Eigenschafts Animationen sind eine flexiblere und leistungsfähigere Möglichkeit, Animationen auf beliebigen Objekttyp zu beschreiben.

- **Anim** &ndash; XML-Dateien, die *Tween* -Animationen beschreiben. Tween-Animationen sind eine Reihe von Animations Anweisungen, mit denen Transformationen für den Inhalt eines Ansichts Objekts durchgeführt werden können. Sie können z. b. ein Bild drehen oder die Textgröße vergrößern. Tween-Animationen sind nur auf Objekte anzeigen beschränkt.

- **Farben &ndash; XML** -Dateien, die eine Status Liste von Farben beschreiben. Zum Verständnis der Farb Zustands Listen sollten Sie ein UI-Widget wie eine Schaltfläche in Erwägung gezogen haben.
   Sie kann unterschiedliche Zustände haben, z. b. "gedrückt" oder "deaktiviert", und die Schaltfläche kann die Farbe mit jeder Zustandsänderung ändern. Die Liste wird in einer Status Liste ausgedrückt.

- **drawable** -&ndash; drawable-Ressourcen sind ein allgemeines Konzept für Grafiken, die in die Anwendung kompiliert werden können und auf die dann durch API-Aufrufe zugegriffen wird oder auf die von anderen XML-Ressourcen verwiesen wird.
   Einige Beispiele für drawables sind Bitmapdateien (PNG, GIF, JPG), spezielle Bitmaps, die in der Größe geändert werden, die als [neun-Patches](https://developer.android.com/guide/topics/graphics/2d-graphics.html#nine-patch), Zustands Listen, generische, in XML definierte Formen usw. bezeichnet werden.

- **Layout** &ndash; XML-Dateien, die ein Benutzeroberflächen Layout beschreiben, z. b. eine Aktivität oder eine Zeile in einer Liste.

- **Menü** &ndash; XML-Dateien, die Anwendungs Menüs beschreiben, z. b. *options Menüs*, *Kontextmenüs*und *Untermenüs*. Ein Beispiel für Menüs finden Sie in der [Popup-Menü Demo](https://docs.microsoft.com/samples/xamarin/monodroid-samples/popupmenudemo) oder im Beispiel für [Standard Steuerelemente](https://docs.microsoft.com/samples/xamarin/mobile-samples/standardcontrols/) .

- unformatierte **&ndash; beliebige** Dateien, die in Ihrem unformatierten Binärformat gespeichert werden. Diese Dateien werden in einer Android-Anwendung in einem Binärformat kompiliert.

- **Werte** &ndash; XML-Dateien, die einfache Werte enthalten. Eine XML-Datei im Werte Verzeichnis definiert nicht eine einzelne Ressource, sondern kann stattdessen mehrere Ressourcen definieren. Eine XML-Datei kann z. b. eine Liste von Zeichen folgen Werten enthalten, während eine andere XML-Datei eine Liste der Farbwerte enthalten kann.

- **XML** -&ndash; XML-Dateien, die in der Funktion mit den .NET-Konfigurationsdateien vergleichbar sind. Dabei handelt es sich um beliebige XML-Daten, die zur Laufzeit von der Anwendung gelesen werden können.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![Standard Ressourcen Dateien](default-resources-images/01-resource-files-xs.png)

In der obigen Abbildung verfügt das Projekt über Standardwerte für drawable-Ressourcen, Layouts und Werte (XML-Dateien, die einfache Werte enthalten).

Eine komplette Liste der Ressourcentypen finden Sie unten:

- **Animator** &ndash; XML-Dateien, die Eigenschafts Animationen beschreiben.
   Eigenschafts Animationen wurden auf API-Ebene 11 (Android 3,0) eingeführt und stellen die Animation von Eigenschaften für ein Objekt bereit. Eigenschafts Animationen sind eine flexiblere und leistungsfähigere Möglichkeit, Animationen auf beliebigen Objekttyp zu beschreiben.

- **Anim** &ndash; XML-Dateien, die *Tween* -Animationen beschreiben. Tween-Animationen sind eine Reihe von Animations Anweisungen, mit denen Transformationen für den Inhalt eines Ansichts Objekts durchgeführt werden können. Sie können z. b. ein Bild drehen oder die Textgröße vergrößern. Tween-Animationen sind nur auf Objekte anzeigen beschränkt.

- **Farben &ndash; XML** -Dateien, die eine Status Liste von Farben beschreiben. Zum Verständnis der Farb Zustands Listen sollten Sie ein UI-Widget wie eine Schaltfläche in Erwägung gezogen haben.
   Möglicherweise haben Sie unterschiedliche Zustände, z. b. "gedrückt" oder "deaktiviert", und die Schaltfläche kann die Farbe mit jeder Zustandsänderung ändern. Die Liste wird in einer Status Liste ausgedrückt.

- **Schriftart** &ndash; beginnend mit API-Ebene 26, ist es möglich, Schriftarten als Ressource in eine Android-Anwendung einzubetten. In der Unterstützungs Bibliothek 26 werden Schriftarten auf API-Ebene 14 zurück portieren. Durch das Einbetten von Schriftarten können Anwendungen benutzerdefinierte Schriftarten direkt aus XML-Layouts laden, ohne Sie als Ressourcen vor der Verwendung importieren zu müssen

- **MipMap** -&ndash; drawable-Ressourcen sind ein allgemeines Konzept für Grafiken, die in die Anwendung kompiliert werden können und auf die dann durch API-Aufrufe zugegriffen wird oder auf die von anderen XML-Ressourcen verwiesen wird.
   Einige Beispiele für drawables sind Bitmapdateien (PNG, GIF, JPG), spezielle Bitmaps, die in der Größe geändert werden, die als [neun-Patches](https://developer.android.com/guide/topics/graphics/2d-graphics.html#nine-patch), Zustands Listen, generische, in XML definierte Formen usw. bezeichnet werden.

- **Layout** &ndash; XML-Dateien, die ein Benutzeroberflächen Layout beschreiben, z. b. eine Aktivität oder eine Zeile in einer Liste.

- **Menü** &ndash; XML-Dateien, die Anwendungs Menüs beschreiben, z. b. *options Menüs*, *Kontextmenüs*und *Untermenüs*. Ein Beispiel für Menüs finden Sie in der [Popup-Menü Demo](https://docs.microsoft.com/samples/xamarin/monodroid-samples/popupmenudemo) oder im Beispiel für [Standard Steuerelemente](https://docs.microsoft.com/samples/xamarin/mobile-samples/standardcontrols/) .

- unformatierte **&ndash; beliebige** Dateien, die in Ihrem unformatierten Binärformat gespeichert werden. Diese Dateien werden in einer Android-Anwendung in einem Binärformat kompiliert.

- **Werte** &ndash; XML-Dateien, die einfache Werte enthalten. Eine XML-Datei im Werte Verzeichnis definiert nicht eine einzelne Ressource, sondern kann stattdessen mehrere Ressourcen definieren. Eine XML-Datei kann z. b. eine Liste von Zeichen folgen Werten enthalten, während eine andere XML-Datei eine Liste der Farbwerte enthalten kann.

- **XML** -&ndash; XML-Dateien, die in der Funktion mit den .NET-Konfigurationsdateien vergleichbar sind. Dabei handelt es sich um beliebige XML-Daten, die zur Laufzeit von der Anwendung gelesen werden können.

-----
