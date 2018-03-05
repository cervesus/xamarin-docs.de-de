---
title: Benutzerdefinierte Renderer
description: "Xamarin.Forms Benutzeroberflächen werden mit den systemeigenen Steuerelementen der Zielplattform, behalten die entsprechenden Aussehen und Verhalten für jede Plattform von Xamarin.Forms Anwendungen gestatten gerendert. Benutzerdefinierte Renderer können Entwickler, die diesen Prozess, um das Aussehen und Verhalten von Xamarin.Forms-Steuerelementen auf jeder Plattform anpassen außer Kraft setzen."
ms.topic: article
ms.prod: xamarin
ms.assetid: BF1CF23A-3BC9-4226-92E6-DAEEB91422F1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: a92da0320addf1569c25ed05873aa11a198b1daa
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="custom-renderers"></a>Benutzerdefinierte Renderer

_Xamarin.Forms Benutzeroberflächen werden mit den systemeigenen Steuerelementen der Zielplattform, behalten die entsprechenden Aussehen und Verhalten für jede Plattform von Xamarin.Forms Anwendungen gestatten gerendert. Benutzerdefinierte Renderer können Entwickler, die diesen Prozess, um das Aussehen und Verhalten von Xamarin.Forms-Steuerelementen auf jeder Plattform anpassen außer Kraft setzen._

## <a name="introduction-to-custom-renderersintroductionmd"></a>[Einführung in benutzerdefinierte Renderer](introduction.md)

Benutzerdefinierter Renderer bieten einen sehr effizienter Ansatz zum Anpassen der Darstellung und Verhalten von Steuerelementen mit Xamarin.Forms. Sie können für kleine Styling Änderungen oder anspruchsvolle plattformspezifischen Layout und Verhalten Anpassung verwendet werden. Dieser Artikel bietet eine Einführung in benutzerdefinierten Renderern und erläutert das Verfahren zum Erstellen eines benutzerdefinierten Renderers.

## <a name="renderer-base-classes-and-native-controlsrenderersmd"></a>[Renderer-Basisklassen und systemeigenen Steuerelementen](renderers.md)

Jedes Xamarin.Forms-Steuerelement verfügt über eine begleitende Renderer für jede Plattform, die eine Instanz eines systemeigenen Steuerelements erstellt. Dieser Artikel führt des Renderers und systemeigene Steuerelementklassen, die jede Xamarin.Forms-Seite, Layout, Ansicht und Zelle zu implementieren.

## <a name="customizing-an-entryentrymd"></a>[Anpassen eines Eintrags](entry.md)

Der Xamarin.Forms [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) Steuerelement ermöglicht es, eine einzelne Textzeile bearbeitet werden. In diesem Artikel wird veranschaulicht, wie zum Erstellen eines benutzerdefinierten Renderers für das `Entry` -Steuerelement, und Entwickler können das Standardrendering zurückgreifen systemeigene mit ihren eigenen plattformspezifische Anpassungen zu überschreiben.

## <a name="customizing-a-contentpagecontentpagemd"></a>[Anpassen einer ContentPage](contentpage.md)

Ein [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) ist ein visuelles Element, das eine einzelne Ansicht und nimmt den größten des Bildschirms. In diesem Artikel wird veranschaulicht, wie zum Erstellen eines benutzerdefinierten Renderers für das `ContentPage` Seite, und Entwickler können das Standardrendering zurückgreifen systemeigene mit ihren eigenen plattformspezifische Anpassungen zu überschreiben.

## <a name="customizing-a-mapmapindexmd"></a>[Anpassen einer Karte](map/index.md)

Xamarin.Forms.Maps bietet eine plattformübergreifende Abstraktion für die Anzeige von Zuordnungen, die systemeigene Zuordnung auf jeder Plattform-APIs zum bieten einer schnelle und vertrauten Zuordnung Erfahrung für Benutzer verwenden. In diesem Thema veranschaulicht das Erstellen von benutzerdefinierten Renderer für die `Map` -Steuerelement, und Entwickler können das Standardrendering zurückgreifen systemeigene mit ihren eigenen plattformspezifische Anpassungen zu überschreiben.

## <a name="customizing-a-listviewlistviewmd"></a>[Anpassen einer ListView](listview.md)

Ein Xamarin.Forms [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) ist eine Sicht, die eine Auflistung von Daten als einer vertikalen Liste anzeigt. In diesem Artikel veranschaulicht, wie einen benutzerdefinierten Renderer, der plattformspezifischen List-Steuerelemente und systemeigene Zelle Layouts kapselt mehr Kontrolle über die einheitlichen Liste Steuerelement Leistung ermöglicht.

## <a name="customizing-a-viewcellviewcellmd"></a>[Anpassen einer ViewCell](viewcell.md)

Eine Xamarin.Forms [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) ist eine Zelle, die hinzugefügt werden kann eine [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) oder [ `TableView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/), eine Sicht entwicklerdefinierter enthält. In diesem Artikel wird veranschaulicht, wie zum Erstellen eines benutzerdefinierten Renderers für eine `ViewCell` gehostet wird, innerhalb einer Xamarin.Forms `ListView` Steuerelement. Dies beendet die Xamarin.Forms Layout Berechnungen wiederholt aufgerufen wird, während der `ListView` Durchführen eines Bildlaufs.

## <a name="implementing-a-viewviewmd"></a>[Implementieren einer Ansicht](view.md)

Xamarin.Forms benutzerdefinierte Schnittstellen Steuerelemente ableiten sollte die [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) Klasse, die verwendet wird, um Layouts und Steuerelemente auf dem Bildschirm zu platzieren. In diesem Artikel wird veranschaulicht, wie einen benutzerdefinierten Renderer für ein benutzerdefiniertes Steuerelement mit Xamarin.Forms erstellt, die zum Anzeigen der Vorschau Videostreams von Kamera des Geräts verwendet wird.

## <a name="implementing-a-hybridwebviewhybridwebviewmd"></a>[Implementieren einer HybridWebView](hybridwebview.md)

In diesem Artikel wird veranschaulicht, wie zum Erstellen eines benutzerdefinierten Renderers für eine `HybridWebView` benutzerdefiniertes Steuerelement, das zur Verbesserung der plattformspezifischen Websteuerelementen um C#-Code, der aufgerufen wird, werden von JavaScript zuzulassen, veranschaulicht.

## <a name="implementing-a-video-playervideo-playerindexmd"></a>[Implementieren einen video player](video-player/index.md)

In diesem Artikel wird gezeigt, wie zum Schreiben von Renderern zur Implementierung einer benutzerdefinierten `VideoPlayer` Steuerelement, die Videos aus dem Web, Videos, die als Anwendungsressourcen eingebettet oder Videos, die in der video-Bibliothek auf dem Gerät des Benutzers gespeicherten spielen können. Verschiedene Techniken werden veranschaulicht, einschließlich der Methoden und schreibgeschützte bindbare Eigenschaften implementieren. 


## <a name="related-links"></a>Verwandte Links

- [Effekte](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Benutzerdefinierte Renderer (Xamarin-University-Video)](https://developer.xamarin.com/videos/cross-platform/xamarinforms-custom-renderers/)
- [Beispiel für benutzerdefinierten Renderer (Xamarin-University-Video)](http://bit.ly/xf-customrenderer)
