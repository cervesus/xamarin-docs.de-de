---
title: Symbolleiste
description: 'Die Symbolleiste ist eine Aktionsleisten Komponente, die mehr Flexibilität bietet als die Standard Aktionsleiste: Sie kann an beliebiger Stelle in der APP platziert werden, die Größe kann geändert werden, und Sie kann ein Farbschema verwenden, das sich vom Design der APP unterscheidet. Außerdem kann jeder App-Bildschirm über mehrere Symbolleisten verfügen.'
ms.prod: xamarin
ms.assetid: 22EE5FBD-3240-4308-AF76-EF45D72936DE
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: a0ca1aa42d9173abbc86a38ae26b14bfb4865a58
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70764609"
---
# <a name="toolbar"></a>Symbolleiste

_Die Symbolleiste ist eine Aktionsleisten Komponente, die mehr Flexibilität bietet als die Standard Aktionsleiste: Sie kann an beliebiger Stelle in der APP platziert werden, die Größe kann geändert werden, und Sie kann ein Farbschema verwenden, das sich vom Design der APP unterscheidet. Außerdem kann jeder App-Bildschirm über mehrere Symbolleisten verfügen._

## <a name="overview"></a>Übersicht

Ein wichtiges Entwurfs Element einer Android-Aktivität ist eine *Aktionsleiste*. Die Aktionsleiste ist die Benutzeroberflächen Komponente, die für Navigation, Suche, Menüs und Branding in einer Android-App verwendet wird. In Android-Versionen vor Android 5,0 Lollipop war die Aktionsleiste (auch als *App-Leiste*bezeichnet) die empfohlene Komponente zum Bereitstellen dieser Funktionalität. 

Das `Toolbar` Widget (eingeführt in Android 5,0 Lollipop) kann sich als Generalisierung der Aktionsleisten Schnittstelle &ndash; vorstellen, die die Aktionsleiste ersetzen soll. Der `Toolbar` kann an beliebiger Stelle in einem App-Layout verwendet werden, und er ist viel anpassbarer als eine Aktionsleiste. Der folgende Screenshot veranschaulicht das angepasste `Toolbar` Beispiel, das in diesem Handbuch erstellt wurde: 

[![Beispiel Bildschirm Abbildung einer Symbolleiste mit Menü Elementen "Bearbeiten", "Speichern" und "Überlauf"](images/01-toolbar-sml.png)](images/01-toolbar.png#lightbox)

Es gibt einige wichtige Unterschiede zwischen `Toolbar` der und der Aktionsleiste: 

- Ein `Toolbar` kann an beliebiger Stelle in der Benutzeroberfläche platziert werden.

- Auf demselben Bildschirm können mehrere Symbolleisten angezeigt werden.

- Wenn Fragmente verwendet werden, kann jedes Fragment über ein eigenes `Toolbar`Fragment verfügen. 

- Eine `Toolbar` kann so konfiguriert werden, dass Sie nur eine partielle Breite des Bildschirms umfasst. 

- Da das `Toolbar` nicht an das Farbschema des Fensters der Aktivitäts Fenster gebunden ist, kann es über ein visuell unterschiedliches Farbschema verfügen. 

- Anders als bei der Aktionsleiste `Toolbar` enthält das kein Symbol auf der linken Seite. In den Menüs auf der rechten Seite wird weniger Speicherplatz verwendet. 

- Die `Toolbar` Höhe ist anpassungsfähig. 

- Andere Sichten können in `Toolbar`enthalten sein. 

Eine `Toolbar` kann eines oder mehrere der folgenden Elemente enthalten: 

- Navigations Schaltfläche

- Ein Branding-Logo-Image

- Titel und Untertitel

- Benutzerdefinierte Ansichten

- Menü "Aktion"

- Überlauf Menü

Die [Material Design Guidelines](https://material.google.com/) von Google empfiehlt, diese Elemente zu nutzen, um apps ein unterschiedliches Aussehen zu verschaffen (anstatt sich ausschließlich auf ein Anwendungssymbol und einen Titel zu verlassen). 

In diesem Handbuch werden die am häufigsten verwendeten `Toolbar` Szenarien behandelt:

- Ersetzen der Standard Aktionsleiste einer Aktivität durch eine `Toolbar`. 

- Hinzufügen einer `Toolbar` zweiten zu einer Aktivität.

- Verwenden der Bibliothek für die **Android-Unterstützungs Bibliothek V7 AppCompat** (im weiteren Verlauf dieses Handbuchs als *AppCompat* bezeichnet `Toolbar` ) zur Bereitstellung in früheren Versionen von Android. 

## <a name="requirements"></a>Anforderungen

`Toolbar`ist unter Android 5,0 Lollipop (API 21) und höher verfügbar. Wenn Sie auf Android-Releases vor Android 5,0 abzielen, verwenden Sie die [Android-Unterstützungs Bibliothek V7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/), die `Toolbar` abwärts kompatible Unterstützung in einem nuget-Paket bereitstellt. 
[Symbolleisten Kompatibilität](~/android/user-interface/controls/tool-bar/toolbar-compatibility.md) erläutert, wie diese Bibliothek verwendet wird. 

## <a name="related-links"></a>Verwandte Links

- [Lollipop-Symbolleiste (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-toolbar)
- [AppCompat-Symbolleiste (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/supportv7-appcompat-toolbar)
