---
title: Symbolleiste
description: 'Die Symbolleiste ist eine Aktionsleisten Komponente, die mehr Flexibilität bietet als die Standard Aktionsleiste: Sie kann an beliebiger Stelle in der APP platziert werden, die Größe kann geändert werden, und Sie kann ein Farbschema verwenden, das sich vom Design der APP unterscheidet. Außerdem kann jeder App-Bildschirm über mehrere Symbolleisten verfügen.'
ms.prod: xamarin
ms.assetid: 22EE5FBD-3240-4308-AF76-EF45D72936DE
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 3f4a9393d8ef6ef54ba3dba29f1109080f1e321a
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029094"
---
# <a name="toolbar"></a>Symbolleiste

_die Symbolleiste eine Aktionsleisten Komponente ist, die mehr Flexibilität bietet als die Standard Aktionsleiste: Sie kann an beliebiger Stelle in der APP platziert werden, die Größe kann geändert werden, und Sie kann ein Farbschema verwenden, das sich vom Design der APP unterscheidet. Außerdem kann jeder App-Bildschirm über mehrere Symbolleisten verfügen._

## <a name="overview"></a>Übersicht

Ein wichtiges Entwurfs Element einer Android-Aktivität ist eine *Aktionsleiste*. Die Aktionsleiste ist die Benutzeroberflächen Komponente, die für Navigation, Suche, Menüs und Branding in einer Android-App verwendet wird. In Android-Versionen vor Android 5,0 Lollipop war die Aktionsleiste (auch als *App-Leiste*bezeichnet) die empfohlene Komponente zum Bereitstellen dieser Funktionalität. 

Das `Toolbar`-Widget (eingeführt in Android 5,0 Lollipop) kann als Generalisierung der Aktionsleisten Schnittstelle betrachtet werden, &ndash; Sie die Aktionsleiste ersetzen soll. Der `Toolbar` kann an beliebiger Stelle in einem App-Layout verwendet werden, und er ist viel anpassbarer als eine Aktionsleiste. Der folgende Screenshot veranschaulicht das angepasste `Toolbar` Beispiel, das in diesem Handbuch erstellt wurde: 

[![Beispiel-Screenshot einer Symbolleiste mit Menü Elementen "Bearbeiten", "Speichern" und "Überlauf"](images/01-toolbar-sml.png)](images/01-toolbar.png#lightbox)

Es gibt einige wichtige Unterschiede zwischen dem `Toolbar` und der Aktionsleiste: 

- Eine `Toolbar` kann an beliebiger Stelle in der Benutzeroberfläche platziert werden.

- Auf demselben Bildschirm können mehrere Symbolleisten angezeigt werden.

- Wenn Fragmente verwendet werden, kann jedes Fragment über eine eigene `Toolbar`verfügen. 

- Eine `Toolbar` kann so konfiguriert werden, dass Sie nur eine partielle Breite des Bildschirms umfasst. 

- Da der `Toolbar` nicht an das Farbschema des Fensters der Aktivitäts Fenster gebunden ist, kann es ein visuell unterschiedliches Farbschema aufweisen. 

- Anders als bei der Aktionsleiste enthält das `Toolbar` kein Symbol auf der linken Seite. In den Menüs auf der rechten Seite wird weniger Speicherplatz verwendet. 

- Die `Toolbar` Höhe ist anpassungsfähig. 

- Andere Ansichten können in den `Toolbar`eingeschlossen werden. 

Eine `Toolbar` kann eines oder mehrere der folgenden Elemente enthalten: 

- Navigations Schaltfläche

- Ein Branding-Logo-Image

- Titel und Untertitel

- Benutzerdefinierte Ansichten

- Aktionsmenü

- Überlauf Menü

Die [Material Design Guidelines](https://material.google.com/) von Google empfiehlt, diese Elemente zu nutzen, um apps ein unterschiedliches Aussehen zu verschaffen (anstatt sich ausschließlich auf ein Anwendungssymbol und einen Titel zu verlassen). 

In diesem Leitfaden werden die am häufigsten verwendeten `Toolbar` Szenarien behandelt:

- Ersetzen der Standard Aktionsleiste einer Aktivität durch eine `Toolbar`. 

- Hinzufügen eines zweiten `Toolbar` zu einer Aktivität.

- Verwenden der Bibliothek für die **Android-Unterstützungs Bibliothek V7 AppCompat** (im weiteren Verlauf dieses Handbuchs als *AppCompat* bezeichnet) zum Bereitstellen von `Toolbar` in früheren Versionen von Android. 

## <a name="requirements"></a>Voraussetzungen

`Toolbar` ist unter Android 5,0 Lollipop (API 21) und höher verfügbar. Wenn Sie auf Android-Releases vor Android 5,0 abzielen, verwenden Sie die [Android-Unterstützungs Bibliothek V7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/), die abwärts kompatible `Toolbar` Unterstützung in einem nuget-Paket bereitstellt. 
[Symbolleisten Kompatibilität](~/android/user-interface/controls/tool-bar/toolbar-compatibility.md) erläutert, wie diese Bibliothek verwendet wird. 

## <a name="related-links"></a>Verwandte Links

- [Lollipop-Symbolleiste (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-toolbar)
- [AppCompat-Symbolleiste (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/supportv7-appcompat-toolbar)
