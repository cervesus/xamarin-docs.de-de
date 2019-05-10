---
title: Benutzerdefinierte Xamarin.Forms-Renderer
description: Mithilfe benutzerdefinierter Renderer können Entwickler das Standardrendering der nativen Steuerelemente auf jeder Plattform „überschreiben“, um so die Darstellung und das Verhalten von Xamarin.Forms-Steuerelementen anzupassen.
ms.prod: xamarin
ms.assetid: BF1CF23A-3BC9-4226-92E6-DAEEB91422F1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: cc188abaece54a4df139918582e57d4116f894d0
ms.sourcegitcommit: bf18425f97b48661ab6b775195eac76b356eeba0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/01/2019
ms.locfileid: "64978127"
---
# <a name="xamarinforms-custom-renderers"></a>Benutzerdefinierte Xamarin.Forms-Renderer

_Xamarin.Forms-Benutzeroberflächen werden mit den nativen Steuerelementen der Zielplattform gerendert. Dadurch können Anwendungen von Xamarin.Forms das Erscheinungsbild der einzelnen Plattformen beibehalten. Durch benutzerdefinierte Renderer können Entwickler diesen Prozess außer Kraft setzen, um die Darstellung und das Verhalten von Xamarin.Forms-Steuerelementen auf jeder Plattform anzupassen._

## <a name="introduction-to-custom-renderersintroductionmd"></a>[Einführung in benutzerdefinierte Renderer](introduction.md)

Benutzerdefinierte Renderer sind eine praktische Methode zum Anpassen der Darstellung und des Verhaltens von Xamarin.Forms-Steuerelementen. Sie können für geringfügige stilistische Änderungen oder für umfangreiche, plattformspezifische Anpassungen des Layouts und Verhaltens verwendet werden. In diesem Artikel werden benutzerdefinierte Renderer vorgestellt, und Sie erfahren, wie Sie einen benutzerdefinierten Renderer erstellen können.

## <a name="renderer-base-classes-and-native-controlsrenderersmd"></a>[Renderer-Basisklassen und native Steuerelemente](renderers.md)

Jedes Xamarin.Forms-Steuerelement verfügt über einen entsprechenden Renderer für jede Plattform, der eine Instanz eines nativen Steuerelements erstellt. In diesem Artikel werden die Klassen für Renderer und native Steuerelemente aufgelistet, die eine Xamarin.Forms-Seite, ein Xamarin.Forms-Layout, eine Xamarin.Forms-Ansicht und eine Xamarin.Forms-Zelle implementieren.

## <a name="customizing-an-entryentrymd"></a>[Anpassen eines Eintrags](entry.md)

Durch das Xamarin.Forms-Steuerelement [`Entry`](xref:Xamarin.Forms.Entry) kann eine einzelne Textzeile bearbeitet werden. In diesem Artikel wird veranschaulicht, wie Sie einen benutzerdefinierten Renderer für das `Entry`-Steuerelement erstellen, sodass Entwickler das native Standardrendering mit ihrem eigenen plattformspezifischen Rendering überschreiben können.

## <a name="customizing-a-contentpagecontentpagemd"></a>[Anpassen einer ContentPage](contentpage.md)

Ein [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Element ist ein visuelles Element, das eine Ansicht anzeigt, die den Großteil des Bildschirms einnimmt. In diesem Artikel wird veranschaulicht, wie Sie einen benutzerdefinierten Renderer für die `ContentPage`-Seite erstellen, sodass Entwickler das native Standardrendering mit ihrem eigenen plattformspezifischen Rendering überschreiben können.

## <a name="customizing-a-mapmapindexmd"></a>[Anpassen einer Karte](map/index.md)

Xamarin.Forms-Karten bieten eine plattformübergreifende Abstraktion zum Anzeigen von Karten, die die native Karten-API auf jeder Plattform verwenden, sodass Benutzer Karten schnell und auf bekannte Weise nutzen können. In diesem Artikel wird veranschaulicht, wie Sie benutzerdefinierte Renderer für das `Map`-Steuerelement erstellen, sodass Entwickler das native Standardrendering mit ihrem eigenen plattformspezifischen Rendering überschreiben können.

## <a name="customizing-a-listviewlistviewmd"></a>[Anpassen einer ListView](listview.md)

Eine Xamarin.Forms-[`ListView`](xref:Xamarin.Forms.ListView) ist eine Ansicht, die eine Sammlung von Daten als vertikale Liste anzeigt. In diesem Artikel wird veranschaulicht, wie Sie einen benutzerdefinierten Renderer erstellen, der plattformspezifische Listensteuerelemente und natives Zellenlayout kapselt, sodass die Kontrolle über die Leistung nativer Listensteuerelemente erhöht wird.

## <a name="customizing-a-viewcellviewcellmd"></a>[Anpassen einer ViewCell](viewcell.md)

Eine Xamarin.Forms-[`ViewCell`](xref:Xamarin.Forms.ViewCell) ist eine Zelle, die einer [`ListView`](xref:Xamarin.Forms.ListView) oder [`TableView`](xref:Xamarin.Forms.TableView) hinzugefügt werden kann und die eine vom Entwickler definierte Ansicht enthält. In diesem Artikel wird veranschaulicht, wie Sie einen benutzerdefinierten Renderer für eine `ViewCell` erstellen können, die in einem Xamarin.Forms-`ListView`-Steuerelement gehostet wird. Das verhindert, dass die Xamarin.Forms-Layoutberechnungen wiederholt beim Scrollen in `ListView` aufgerufen werden.

## <a name="implementing-a-viewviewmd"></a>[Implementieren einer Ansicht](view.md)

Benutzerdefinierte Xamarin.Forms-Benutzeroberflächen-Steuerelemente sollten von der [`View`](xref:Xamarin.Forms.View)-Klasse abgeleitet werden, die verwendet wird, um Layout und Steuerelemente einer Anzeige hinzuzufügen. In diesem Artikel wird veranschaulicht, wie Sie einen benutzerdefinierten Renderer für ein benutzerdefiniertes Xamarin.Forms-Steuerelement erstellen können, mit dem Sie eine Vorschau eines Videostreams von der Kamera des Geräts anzeigen können.

## <a name="implementing-a-hybridwebviewhybridwebviewmd"></a>[Implementieren einer HybridWebView](hybridwebview.md)

In diesem Artikel wird veranschaulicht, wie Sie einen benutzerdefinierten Renderer für ein benutzerdefiniertes `HybridWebView`-Steuerelement erstellen können. Dadurch wird veranschaulicht, wie Sie plattformspezifische Websteuerelemente erweitern können, sodass diese zulassen, dass C#-Code über JavaScript aufgerufen werden kann.

## <a name="implementing-a-video-playervideo-playerindexmd"></a>[Implementieren eines Videoplayers](video-player/index.md)

In diesem Artikel wird veranschaulicht, wie Sie Renderer schreiben können, die ein benutzerdefiniertes `VideoPlayer`-Steuerelement implementieren, das Webvideos, als Anwendungsressourcen eingebettete Videos oder Videos aus der Videogalerie des Geräts abspielen können. Es werden mehrere Vorgehensweisen demonstriert, u.a. das Implementieren von Methoden und schreibgeschützten, bindbaren Eigenschaften.

## <a name="related-links"></a>Verwandte Links

- [Effekte](~/xamarin-forms/app-fundamentals/effects/index.md)
