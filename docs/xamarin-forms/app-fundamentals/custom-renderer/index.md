---
title: Xamarin.Forms Benutzerdefinierte Renderer
description: Mithilfe benutzerdefinierter Renderer können Entwickler das Standardrendering der nativen Steuerelemente auf jeder Plattform außer Kraft setzen, um so die Darstellung und das Verhalten von Xamarin.Forms-Steuerelementen anzupassen.
ms.prod: xamarin
ms.assetid: BF1CF23A-3BC9-4226-92E6-DAEEB91422F1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/03/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: be281f9b7987a8d23ba6ac93f0771e432f277d45
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84138917"
---
# <a name="xamarinforms-custom-renderers"></a>Xamarin.Forms Benutzerdefinierte Renderer

_Xamarin.Forms-Benutzeroberflächen werden mit den nativen Steuerelementen der Zielplattform gerendert. Dadurch können Xamarin.Forms-Anwendungen das Erscheinungsbild der einzelnen Plattformen beibehalten. Durch benutzerdefinierte Renderer können Entwickler diesen Prozess außer Kraft setzen, um die Darstellung und das Verhalten von Xamarin.Forms-Steuerelementen auf jeder Plattform anzupassen._

## <a name="introduction-to-custom-renderers"></a>[Einführung in benutzerdefinierte Renderer](introduction.md)

Benutzerdefinierte Renderer sind eine praktische Methode zum Anpassen der Darstellung und des Verhaltens von Xamarin.Forms-Steuerelementen. Sie können für geringfügige Formatierungsänderungen oder für umfangreiche, plattformspezifische Anpassungen des Layouts und Verhaltens verwendet werden. In diesem Artikel werden benutzerdefinierte Renderer vorgestellt, und Sie erfahren, wie Sie einen benutzerdefinierten Renderer erstellen können.

## <a name="renderer-base-classes-and-native-controls"></a>[Renderer-Basisklassen und native Steuerelemente](renderers.md)

Jedes Xamarin.Forms-Steuerelement verfügt über einen entsprechenden Renderer für jede Plattform, die eine Instanz eines nativen Steuerelements erstellt. In diesem Artikel werden die Klassen für Renderer und native Steuerelemente aufgelistet, die eine Xamarin.Forms-Seite, ein Xamarin.Forms-Layout, eine Xamarin.Forms-Ansicht und eine Xamarin.Forms-Zelle implementieren.

## <a name="customizing-an-entry"></a>[Anpassen eines Eintrags](entry.md)

Mit dem Xamarin.Forms-Steuerelement [`Entry`](xref:Xamarin.Forms.Entry) kann eine einzelne Textzeile bearbeitet werden. In diesem Artikel wird veranschaulicht, wie Sie einen benutzerdefinierten Renderer für das `Entry`-Steuerelement erstellen, sodass Entwickler das native Standardrendering mit ihrem eigenen plattformspezifischen Rendering überschreiben können.

## <a name="customizing-a-contentpage"></a>[Anpassen einer ContentPage](contentpage.md)

Ein [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Element ist ein visuelles Element, das eine Ansicht anzeigt, die den Großteil des Bildschirms einnimmt. In diesem Artikel wird veranschaulicht, wie Sie einen benutzerdefinierten Renderer für die `ContentPage`-Seite erstellen, sodass Entwickler das native Standardrendering mit ihrem eigenen plattformspezifischen Rendering überschreiben können.

## <a name="customizing-a-map-pin"></a>[Anpassen einer Kartennadel](map-pin.md)

Xamarin.Forms.Maps bietet eine plattformübergreifende Abstraktion zum Anzeigen von Karten, die die native Karten-API auf jeder Plattform verwenden, sodass Benutzer Karten schnell und auf bekannte Weise nutzen können. In diesem Artikel wird veranschaulicht, wie Sie benutzerdefinierte Renderer für das `Map`-Steuerelement erstellen, sodass Entwickler das native Standardrendering mit ihrem eigenen plattformspezifischen Rendering überschreiben können.

## <a name="customizing-a-listview"></a>[Anpassen einer ListView](listview.md)

Beim Xamarin.Forms-Element [`ListView`](xref:Xamarin.Forms.ListView) handelt es sich um eine Ansicht, in der eine Sammlung von Daten als vertikale Liste angezeigt wird. In diesem Artikel wird veranschaulicht, wie Sie einen benutzerdefinierten Renderer erstellen, der plattformspezifische Listensteuerelemente und natives Zellenlayout kapselt, sodass die Kontrolle über die Leistung nativer Listensteuerelemente erhöht wird.

## <a name="customizing-a-viewcell"></a>[Anpassen einer ViewCell](viewcell.md)

Das Xamarin.Forms-Element [`ViewCell`](xref:Xamarin.Forms.ViewCell) ist eine Zelle, die [`ListView`](xref:Xamarin.Forms.ListView) oder [`TableView`](xref:Xamarin.Forms.TableView) hinzugefügt werden kann und die eine vom Entwickler definierte Ansicht enthält. In diesem Artikel wird veranschaulicht, wie Sie einen benutzerdefinierten Renderer für ein `ViewCell`-Element erstellen, das im Xamarin.Forms-Steuerelement `ListView` gehostet wird. Das verhindert, dass die Xamarin.Forms-Layoutberechnungen wiederholt beim Scrollen in `ListView` aufgerufen werden.

## <a name="customizing-a-webview"></a>[Anpassen eines WebView-Elements](hybridwebview.md)

Das Xamarin.Forms-Element [`WebView`](xref:Xamarin.Forms.WebView) ist eine Ansicht, die Web- und HTML-Inhalte in Ihrer App anzeigt. In diesem Artikel wird erläutert, wie Sie einen benutzerdefinierten Renderer erstellen, der `WebView` erweitert, um das Aufrufen von C#-Code über JavaScript zuzulassen.

## <a name="implementing-a-view"></a>[Implementieren einer Ansicht](view.md)

Benutzerdefinierte Xamarin.Forms-Benutzeroberflächen-Steuerelemente sollten von der [`View`](xref:Xamarin.Forms.View)-Klasse abgeleitet werden, die verwendet wird, um einem Bildschirm Layout und Steuerelemente hinzuzufügen. In diesem Artikel wird veranschaulicht, wie Sie einen benutzerdefinierten Renderer für ein benutzerdefiniertes Xamarin.Forms-Steuerelement erstellen, mit dem Sie eine Vorschau eines Videostreams von der Kamera des Geräts anzeigen können.

## <a name="implementing-a-video-player"></a>[Implementieren eines Videoplayers](video-player/index.md)

In diesem Artikel wird veranschaulicht, wie Sie Renderer schreiben können, die ein benutzerdefiniertes `VideoPlayer`-Steuerelement implementieren, das Webvideos, als Anwendungsressourcen eingebettete Videos oder Videos aus der Videogalerie des Geräts abspielen können. Es werden mehrere Vorgehensweisen demonstriert, u.a. das Implementieren von Methoden und schreibgeschützten, bindbaren Eigenschaften.
