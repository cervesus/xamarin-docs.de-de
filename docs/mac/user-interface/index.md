---
title: macOS-Benutzeroberflächen-Steuerelemente in xamarin. Mac
description: Dieses Dokument ist mit Anleitungen verknüpft, die verschiedene Benutzeroberflächen-Steuerelemente beschreiben, die für xamarin. Mac-Entwickler verfügbar sind. Verknüpfte Inhalte sehen sich Windows, Dialogfelder, Warnungen, Menüs, Symbolleisten, Tabellen Sichten, Gliederungs Ansichten und vieles mehr an.
ms.prod: xamarin
ms.assetid: 876B6EC2-E158-43F2-B9C9-03F54F3D2A49
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/27/2018
ms.openlocfilehash: 7f5303cd63c6ff1433b56b3f47b67d3925b1d1e1
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032781"
---
# <a name="macos-user-interface-controls-in-xamarinmac"></a>macOS-Benutzeroberflächen-Steuerelemente in xamarin. Mac

_Dieser Artikel ist mit Anleitungen verknüpft, die verschiedene macOS-UI-Steuerelemente beschreiben._

Wenn Sie mit C# und .net in einer xamarin. Mac-Anwendung arbeiten, haben Sie Zugriff auf die gleichen Steuerelemente der Benutzeroberfläche, die von einem Entwickler in *Ziel-C* und *Xcode* verwendet werden. Da xamarin. Mac direkt in Xcode integriert ist, können Sie die _Interface Builder_ von Xcode verwenden, um die Benutzeroberflächen zu erstellen und zu verwalten (oder C# Sie optional direkt im Code zu erstellen).

Die unten aufgeführten Anleitungen zeigen ausführliche Informationen zum Arbeiten mit macOS-Benutzeroberflächen Elementen in einer xamarin. Mac-Anwendung. Es wird dringend empfohlen, dass Sie zunächst den Artikel [Hello, Mac](~/mac/get-started/hello-mac.md) , insbesondere die [Einführung in Xcode und die Abschnitte zu Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) und Outlets und [Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) , verwenden, da er wichtige Konzepte und Techniken behandelt, die wir in verwenden werden. Alle Artikel.

Sie können sich auch den Abschnitt verfügbar machen von [ C# Klassen/Methoden zu "Ziel-C](~/mac/internals/how-it-works.md#exposing-c-classes--methods-to-objective-c) " im Dokument " [xamarin. Mac](~/mac/internals/how-it-works.md) " ansehen, da hier die`Register`-und`Export`Attribute erläutert werden, mit denen die C# Klassen an gesendet werden. Ziel-C-Objekte und UI-Elemente.

## <a name="windowsmacuser-interfacewindowmd"></a>[Windows](~/mac/user-interface/window.md)

In diesem Artikel wird das Arbeiten mit Fenstern und Bereichen in einer xamarin. Mac-Anwendung behandelt. Es umfasst das Erstellen und Verwalten von Fenstern und Bereichen in Xcode und Interface Builder, das Laden von Fenstern und Panels aus Storyboard-oder XIb-Dateien, die Verwendung von Windows C# und das reagieren auf Windows im Code.

## <a name="dialogsmacuser-interfacedialogmd"></a>[Dialogfelder](~/mac/user-interface/dialog.md)

In diesem Artikel wird das Arbeiten mit Dialogfeldern und modalen Fenstern in einer xamarin. Mac-Anwendung behandelt. Es behandelt das Erstellen und Verwalten von modalen Fenstern in Xcode und Interface Builder, das Arbeiten mit Standard Dialogfeldern sowie das C# anzeigen und reagieren auf Fenster in Code.

## <a name="alertsmacuser-interfacealertmd"></a>[Benachrichtigungen](~/mac/user-interface/alert.md)

In diesem Artikel wird das Arbeiten mit Warnungen in einer xamarin. Mac-Anwendung behandelt. Es wird das Erstellen und Anzeigen von C# Warnungen aus dem Code und das reagieren auf Warnungen behandelt.

## <a name="menusmacuser-interfacemenumd"></a>[Menüs](~/mac/user-interface/menu.md)

Menüs werden in verschiedenen Teilen der Benutzeroberfläche einer Mac-Anwendung verwendet. im Hauptmenü der Anwendung oben auf dem Bildschirm, um Popup Menüs und Kontextmenüs anzuzeigen, die an beliebiger Stelle in einem Fenster angezeigt werden können. Menüs sind ein integraler Bestandteil einer Mac-Anwendungsumgebung. In diesem Artikel wird das Arbeiten mit Cocoa-Menüs in einer xamarin. Mac-Anwendung behandelt.

## <a name="standard-controlsmacuser-interfacestandard-controlsmd"></a>[Standard Steuerelemente](~/mac/user-interface/standard-controls.md)

Arbeiten mit den standardmäßigen AppKit-Steuerelementen wie Schaltflächen, Bezeichnungen, Textfelder, Kontrollkästchen und segmentierten Steuerelementen in einer xamarin. Mac-Anwendung. In dieser Anleitung wird erläutert, wie Sie Sie zu einem Design der Benutzeroberfläche in Xcode-Interface Builder hinzufügen, Sie für den Code über Outlets und Aktionen verfügbar C# machen und mit AppKit-Steuerelementen im Code arbeiten.

## <a name="toolbarsmacuser-interfacetoolbarmd"></a>[Symbolleisten](~/mac/user-interface/toolbar.md)

In diesem Artikel wird das Arbeiten mit Symbolleisten in einer xamarin. Mac-Anwendung behandelt. Es behandelt das Erstellen und Verwalten von Symbolleisten in Xcode und Interface Builder, das verfügbar machen der Symbolleisten Elemente für Code mithilfe von Outlets und Aktionen, das Aktivieren und Deaktivieren von Symbolleisten Elementen und schließlich C# das reagieren auf Symbolleisten Elemente im Code.

## <a name="table-viewsmacuser-interfacetable-viewmd"></a>[Tabellen Sichten](~/mac/user-interface/table-view.md)

In diesem Artikel wird das Arbeiten mit Tabellen Sichten in einer xamarin. Mac-Anwendung behandelt. Es umfasst das Erstellen und Verwalten von Tabellen Sichten in Xcode und Interface Builder, das verfügbar machen der Tabellen Ansichts Elemente für Code mithilfe von Outlets und Aktionen, das Auffüllen von Tabellen Sichten und das reagieren C# auf Tabellen Ansichts Elemente im Code.

## <a name="outline-viewsmacuser-interfaceoutline-viewmd"></a>[Gliederungs Ansichten](~/mac/user-interface/outline-view.md)

In diesem Artikel wird das Arbeiten mit Gliederungs Ansichten in einer xamarin. Mac-Anwendung behandelt. Es umfasst das Erstellen und Verwalten von Gliederungs Ansichten in Xcode und Interface Builder, das verfügbar machen der Gliederungs Ansichts Elemente für Code mithilfe von Outlets und Aktionen, das Auffüllen von Gliederungs Ansichten und das reagieren auf Gliederungs Ansichts Elemente im C# Code

## <a name="source-listsmacuser-interfacesource-listmd"></a>[Quell Listen](~/mac/user-interface/source-list.md)

In diesem Artikel wird die Arbeit mit Quell Listen in einer xamarin. Mac-Anwendung behandelt. Es umfasst das Erstellen und Verwalten von Quell Listen in Xcode und Interface Builder, das verfügbar machen von Quell Listenelementen für Code mithilfe von Outlets und Aktionen, das Auffüllen von Quell Listen und das reagieren C# auf Quell Listenelemente im Code.

## <a name="collection-viewsmacuser-interfacecollection-viewmd"></a>[Sammlungs Ansichten](~/mac/user-interface/collection-view.md)

In diesem Artikel wird die Arbeit mit Auflistungs Ansichten in einer xamarin. Mac-Anwendung behandelt. Es behandelt das Erstellen und Verwalten von Auflistungs Ansichten in Xcode und Interface Builder, das verfügbar machen der Auflistungs Ansichts Elemente für Code mit Outlets und Aktionen, das Auffüllen von Auflistungs Ansichten und das reagieren auf Auflistungs Ansichten im C# Code.

## <a name="creating-custom-controlsmacuser-interfacecustom-controlsmd"></a>[Erstellen benutzerdefinierter Steuerelemente](~/mac/user-interface/custom-controls.md)

In diesem Artikel wird beschrieben, wie Sie benutzerdefinierte Steuerelemente der Benutzeroberfläche erstellen (indem Sie von `NSControl`erben), eine benutzerdefinierte Schnittstelle für das Steuerelement zeichnen und benutzerdefinierte Aktionen erstellen, die mit Xcode-Interface Builder verwendet werden können.

## <a name="mac-samples-gallery"></a>Mac Samples Gallery

Außerdem wird empfohlen, die [Mac Samples Gallery](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Mac)zu betrachten. Sie enthält eine Fülle von Einsatz bereitem Code, mit dem Sie schnell ein xamarin. Mac-Projekt erhalten können.

## <a name="related-links"></a>Verwandte Links

- [macOS-Eingaberichtlinien](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
