---
title: Xamarin.Forms Custom Renderer
description: Benutzerdefinierte Renderer können Entwickler, die das Rendern von native Steuerelemente auf jeder Plattform zum Anpassen der Darstellung und Verhalten von Xamarin.Forms-Steuerelementen zu überschreiben.
ms.prod: xamarin
ms.assetid: BF1CF23A-3BC9-4226-92E6-DAEEB91422F1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: c7ae25688b2f8635a9a89318e0b307e58add7a5a
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998742"
---
# <a name="xamarinforms-custom-renderers"></a>Xamarin.Forms Custom Renderer

_Xamarin.Forms-Benutzeroberflächen werden gerendert mit den nativen Steuerelementen der Zielplattform, sodass Xamarin.Forms-Anwendungen das entsprechende Aussehen und Verhalten für die einzelnen Plattformen beibehalten werden sollen. Benutzerdefinierte Renderer können Entwickler, die diesen Vorgang, um das Aussehen und Verhalten von Xamarin.Forms-Steuerelementen auf jeder Plattform anpassen außer Kraft zu setzen._

## <a name="introduction-to-custom-renderersintroductionmd"></a>[Einführung in benutzerdefinierte Renderer](introduction.md)

Benutzerdefinierte Renderer bieten einen sehr effizienter Ansatz zum Anpassen der Darstellung und das Verhalten von Xamarin.Forms-Steuerelementen. Sie können für kleine Formatierungsänderungen oder komplexe plattformspezifische Layout und verhaltensanpassung verwendet werden. Dieser Artikel bietet eine Einführung in benutzerdefinierten Renderern und erläutert den Prozess zum Erstellen eines benutzerdefinierten Renderers.

## <a name="renderer-base-classes-and-native-controlsrenderersmd"></a>[Renderer-Basisklassen und native Steuerelemente](renderers.md)

Alle Xamarin.Forms-Steuerelements verfügt über eine zugehörige Renderer für jede Plattform, die eine Instanz eines systemeigenen Steuerelements erstellt. Dieser Artikel listet die Renderer und dem nativen Steuerelements-Klassen, die jede Xamarin.Forms-Seite, Layout, Ansicht und Zelle zu implementieren.

## <a name="customizing-an-entryentrymd"></a>[Anpassen eines Eintrags](entry.md)

Die Xamarin.Forms [ `Entry` ](xref:Xamarin.Forms.Entry) Steuerelement ermöglicht es, eine einzelne Textzeile bearbeitet werden. In diesem Artikel wird veranschaulicht, wie zum Erstellen eines benutzerdefinierten Renderers für das `Entry` -Steuerelement ermöglicht es Entwicklern, native standarddarstellung mit ihren eigenen plattformspezifische Anpassungen zu überschreiben.

## <a name="customizing-a-contentpagecontentpagemd"></a>[Anpassen einer ContentPage](contentpage.md)

Ein [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) ist ein visuelles Element, das zeigt eine einzelnen Ansicht und der Großteil des Bildschirms einnimmt. In diesem Artikel wird veranschaulicht, wie zum Erstellen eines benutzerdefinierten Renderers für das `ContentPage` Seite ermöglichen Entwicklern, native standarddarstellung mit ihren eigenen plattformspezifische Anpassungen zu überschreiben.

## <a name="customizing-a-mapmapindexmd"></a>[Anpassen einer Karte](map/index.md)

Xamarin.Forms.Maps bietet eine plattformübergreifende Abstraktion für die Anzeige von Karten, die die native Zuordnung APIs auf jeder Plattform zu einer Karte schnell und vertraute Erfahrung für Benutzer verwenden. In diesem Thema veranschaulicht, wie benutzerdefinierte Renderer für die `Map` -Steuerelement ermöglicht es Entwicklern, native standarddarstellung mit ihren eigenen plattformspezifische Anpassungen zu überschreiben.

## <a name="customizing-a-listviewlistviewmd"></a>[Anpassen einer ListView](listview.md)

Eine Xamarin.Forms [ `ListView` ](xref:Xamarin.Forms.ListView) ist eine Ansicht, die eine Sammlung von Daten als vertikale Liste angezeigt. In diesem Artikel veranschaulicht, wie einen benutzerdefinierten Renderer, der plattformspezifische Listensteuerelementen und native Zelle Layouts, kapselt ermöglicht mehr Kontrolle über die Steuerung der Leistung des systemeigenen Liste.

## <a name="customizing-a-viewcellviewcellmd"></a>[Anpassen einer ViewCell](viewcell.md)

Eine Xamarin.Forms [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) ist eine Zelle, die hinzugefügt werden kann, eine [ `ListView` ](xref:Xamarin.Forms.ListView) oder [ `TableView` ](xref:Xamarin.Forms.TableView), die eine Entwickler benutzerdefinierte Sicht enthält. In diesem Artikel wird veranschaulicht, wie zum Erstellen eines benutzerdefinierten Renderers für eine `ViewCell` , gehostet in einer Xamarin.Forms `ListView` Steuerelement. Dadurch wird wiederholt aufgerufen wird, während die Xamarin.Forms-layoutberechnungen beendet `ListView` scrollen.

## <a name="implementing-a-viewviewmd"></a>[Implementieren einer Ansicht](view.md)

Xamarin.Forms benutzerdefinierte Schnittstellen Benutzersteuerelemente abgeleitet sollte die [ `View` ](xref:Xamarin.Forms.View) -Klasse, die Layouts und Steuerelemente auf dem Bildschirm platziert. In diesem Artikel wird veranschaulicht, wie einen benutzerdefinierten Renderer für ein benutzerdefiniertes Steuerelement mit Xamarin.Forms erstellt, mit dem das Gerät die Kamera einen Vorschau-Videodatenstrom anzuzeigen.

## <a name="implementing-a-hybridwebviewhybridwebviewmd"></a>[Implementieren einer HybridWebView](hybridwebview.md)

In diesem Artikel wird veranschaulicht, wie zum Erstellen eines benutzerdefinierten Renderers für eine `HybridWebView` benutzerdefiniertes Steuerelement, das zeigt, wie zur Verbesserung der plattformspezifischen Websteuerelemente um C#-Code aufgerufen werden von JavaScript zu ermöglichen.

## <a name="implementing-a-video-playervideo-playerindexmd"></a>[Implementieren einen Videoplayer](video-player/index.md)

In diesem Artikel zeigt, wie zum Schreiben von Renderern, die zum Implementieren eines benutzerdefinierten `VideoPlayer` -Steuerelement, das Videos von der Web-, Videos, die als Anwendungsressourcen eingebettet oder in der Videobibliothek auf dem Gerät des Benutzers gespeicherten Videos wiedergeben kann. Verschiedene Techniken werden veranschaulicht, einschließlich der Implementierung von Methoden und schreibgeschützte bindbare Eigenschaften.


## <a name="related-links"></a>Verwandte Links

- [Effekte](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Benutzerdefinierte Renderer (Xamarin University-Video)](https://developer.xamarin.com/videos/cross-platform/xamarinforms-custom-renderers/)
- [Beispiel für benutzerdefinierte Renderer (Xamarin University-Video)](http://bit.ly/xf-customrenderer)
