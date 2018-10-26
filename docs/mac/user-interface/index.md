---
title: Steuerelemente der Benutzeroberfläche in Xamarin.Mac macOS
description: Dieses Dokument enthält Links zu Leitfäden, die verschiedene Steuerelemente der Benutzeroberfläche für Xamarin.Mac-Entwickler zu beschreiben. Verknüpfter Inhalt untersucht, mit Windows, Dialogfelder, Warnungen, Menüs, Symbolleisten, Tabellenansichten, Gliederungsansichten und vieles mehr.
ms.prod: xamarin
ms.assetid: 876B6EC2-E158-43F2-B9C9-03F54F3D2A49
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/27/2018
ms.openlocfilehash: a12553cf0b7b9584bb8ff7bc04ed326ad4a7ad2a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50107832"
---
# <a name="macos-user-interface-controls-in-xamarinmac"></a>Steuerelemente der Benutzeroberfläche in Xamarin.Mac macOS

_Dieser Artikel enthält Links in den Anleitungen, die verschiedenen MacOS-UI-Steuerelemente zu beschreiben._

Bei der Arbeit mit c# und .NET in einer Xamarin.Mac-Anwendung haben Sie Zugriff auf den gleichen Benutzer Schnittstelle steuert, die ein Entwickler *Objective-C-* und *Xcode* ist. Da Xamarin.Mac direkt in Xcode integriert ist, können Sie von Xcode _Interface Builder_ erstellen und verwalten Ihren Benutzeroberflächen (oder erstellen sie optional direkt in c#-Code).

In folgenden Leitfäden erhalten Sie ausführliche Informationen zum Arbeiten mit MacOS-UI-Elemente in einer Xamarin.Mac-Anwendung. Es wird dringend empfohlen, dass Sie über arbeiten die [Hallo, Mac](~/mac/get-started/hello-mac.md) Artikel zuerst, insbesondere die [Einführung in Xcode und Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) und [Outlets und Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) Abschnitte, wie sie behandelt wichtige Konzepte und Techniken, die wir in jedem Artikel verwenden.

Empfiehlt es sich um einen Blick auf die [Verfügbarmachen von c#-Klassen / Methoden mit Objective-C](~/mac/internals/how-it-works.md#exposing-c-classes--methods-to-objective-c) Teil der [Xamarin.Mac-Interna](~/mac/internals/how-it-works.md) dokumentiert, es wird erläutert, die `Register` und `Export` Attribute, die, mit denen Ihre Klassen in c# für Objective-C-Objekte und Elemente der Benutzeroberfläche anschließen.

## <a name="windowsmacuser-interfacewindowmd"></a>[Windows](~/mac/user-interface/window.md)

In diesem Artikel wird das Arbeiten mit Fenstern und Bereichen in einer Xamarin.Mac-Anwendung behandelt. Hierin sind erstellen und Verwalten von Windows und Bereiche in Xcode und Interface Builder, laden Windows bzw. Bereiche aus Storyboard oder XIB-Dateien, unter Verwendung von Windows und Windows in c#-Code reagieren.

## <a name="dialogsmacuser-interfacedialogmd"></a>[Dialogfelder](~/mac/user-interface/dialog.md)

Dieser Artikel behandelt die Arbeit mit Dialogfeldern und modalen Fenstern in einer Xamarin.Mac-Anwendung. Hierin sind erstellen und Verwalten von modalen Fenstern in Xcode und Interface Builder, mit Standarddialogfeldern, arbeiten und Anzeigen von und reagieren auf Windows in C#-Code.

## <a name="alertsmacuser-interfacealertmd"></a>[Benachrichtigungen](~/mac/user-interface/alert.md)

Dieser Artikel behandelt die Arbeit mit Warnungen in einer Xamarin.Mac-Anwendung. Hierin sind erstellen und Anzeigen von Warnungen aus c#-Code aus, und reagieren auf Warnungen.

## <a name="menusmacuser-interfacemenumd"></a>[Menüs](~/mac/user-interface/menu.md)

Menüs sind in verschiedenen Teilen der Benutzeroberfläche einer Mac-Anwendung verwendet. Hauptmenü der Anwendung am oberen Bildschirmrand, um Popup-Menüs und Kontextmenüs, die an einer beliebigen Stelle in einem Fenster angezeigt werden können. Menüs sind ein wesentlicher Bestandteil der Benutzeroberfläche einer Mac-Anwendung. Dieser Artikel behandelt die Arbeit mit Cocoa-Menüs in einer Xamarin.Mac-Anwendung.

## <a name="standard-controlsmacuser-interfacestandard-controlsmd"></a>[Standardsteuerelemente](~/mac/user-interface/standard-controls.md)

Arbeiten mit den standardmäßigen AppKit-Steuerelementen wie Schaltflächen, Bezeichnungen, Textfelder, Kontrollkästchen und segmentierte Steuerelemente in einer Xamarin.Mac-Anwendung. Dieser Leitfaden behandelt das Hinzufügen zu einem Entwurf der Benutzeroberfläche in Interface Builder von Xcode für Code mit Outlets und Aktionen verfügbar zu machen, und Verwenden von AppKit-Steuerelementen in C#-Code.

## <a name="toolbarsmacuser-interfacetoolbarmd"></a>[Symbolleisten](~/mac/user-interface/toolbar.md)

Dieser Artikel behandelt die Arbeit mit Symbolleisten in einer Xamarin.Mac-Anwendung. Hierin sind erstellen und Verwalten von Symbolleisten in Xcode und Interface Builder, wie Sie die Symbolleistenelemente für Code verwenden Outlets und Aktionen, aktivieren und deaktivieren die Elemente der Symbolleiste und schließlich auf Symbolleistenelemente reagieren in C#-Code verfügbar zu machen.

## <a name="table-viewsmacuser-interfacetable-viewmd"></a>[Tabellenansichten](~/mac/user-interface/table-view.md)

Dieser Artikel behandelt die Arbeit mit Tabellenansichten in einer Xamarin.Mac-Anwendung. Hierin sind erstellen und verwalten Tabellenansichten in Xcode und Interface Builder, wie Elemente die Tabellenansicht verfügbar zu machen, Code mit Outlets und Aktionen Auffüllen Tabellenansichten und reagieren auf tabellenansichtenelemente in C#-Code.

## <a name="outline-viewsmacuser-interfaceoutline-viewmd"></a>[Gliederungsansichten](~/mac/user-interface/outline-view.md)

Dieser Artikel behandelt die Arbeit mit Gliederungsansichten in einer Xamarin.Mac-Anwendung. Hierin sind erstellen und verwalten Gliederungsansichten in Xcode und Interface Builder, wie Elemente die Gliederungsansicht verfügbar machen für Code mit Outlets und Aktionen Auffüllen Gliederungsansichten und reagieren auf Anforderungen zum Anzeigen von Elementen im c#-Code beschrieben.

## <a name="source-listsmacuser-interfacesource-listmd"></a>[Quelllisten](~/mac/user-interface/source-list.md)

Dieser Artikel behandelt die Arbeit mit Quelllisten in einer Xamarin.Mac-Anwendung. Hierin sind erstellen und verwalten Quelllisten in Xcode und Interface Builder, wie Sie die quelllistenelemente für Code verwenden Outlets und Aktionen, Quelllisten ausfüllen und reagieren auf quelllistenelemente in C#-Code verfügbar zu machen.

## <a name="collection-viewsmacuser-interfacecollection-viewmd"></a>[Auflistungsansichten](~/mac/user-interface/collection-view.md)

In diesem Artikel wird das Arbeiten mit Auflistungsansichten in einer Xamarin.Mac-Anwendung behandelt. Hierin sind erstellen und verwalten Auflistungsansichten in Xcode und Interface Builder, wie die Elemente, die Auflistungsansicht verfügbar zu machen, Code mit Outlets und Aktionen auffüllen, und reagieren auf Auflistungsansichten in C#-Code.

## <a name="creating-custom-controlsmacuser-interfacecustom-controlsmd"></a>[Erstellen von benutzerdefinierten Steuerelementen](~/mac/user-interface/custom-controls.md)

Diesem Artikel wird beschrieben, erstellen benutzerdefinierte Steuerelemente der Benutzeroberfläche (durch Erben von `NSControl`), zeichnen Sie eine benutzerdefinierte Schnittstelle für das Steuerelement und das Erstellen von benutzerdefinierter Aktionen, die mit Interface Builder von Xcode verwendet werden können.

## <a name="mac-samples-gallery"></a>Mac-Beispielkatalog

Darüber hinaus wird empfohlen ein Blick auf die [Mac Samples Gallery](https://developer.xamarin.com/samples/mac/all/). Es enthält eine Fülle von Code zu verwendende bereit, mit denen Sie eine Xamarin.Mac-Projekt schnell anfangen kann.

## <a name="related-links"></a>Verwandte Links

- [MacOS Human Interface Guidelines](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
