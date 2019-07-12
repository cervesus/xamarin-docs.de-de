---
title: Überprüfen von Live-Anwendungen
description: Dieses Dokument beschreibt, wie Sie die Xamarin Inspector zu verwenden, um Anwendungen zu überprüfen. Darüber hinaus werden die Einschränkungen des Xamarin Inspector Tools erläutert.
ms.prod: xamarin
ms.assetid: 91B3206E-B2A5-4660-A6E5-B924B8FE69A7
author: lobrien
ms.author: laobri
ms.date: 06/19/2018
ms.openlocfilehash: 2def0a01bdd28af5eefb76afc19a0e49fd1df355
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67831551"
---
# <a name="inspecting-live-applications"></a>Überprüfen von Live-Anwendungen

Die Überprüfung der Live-App steht für Enterprise-Kundne zur Verfügung.

1. Öffnen Sie ein [unterstütztes App-Projekt](~/tools/inspector/install.md#supported-platforms) in Visual Studio für Mac oder Visual Studio.
1. Führen Sie die Anwendung im Debugmodus aus.
1. Klicken Sie auf der Symbolleiste der IDE auf die Schaltfläche **Inspect** (Überprüfen) (in Visual Studio steht ebenfalls das Menüelement zum **Überprüfen der aktuellen App** über das Menü**Extras** oder **Debuggen** zur Verfügung).

[![](inspect-images/mac-heres-the-button.png "Klicken Sie auf die Schaltfläche \"prüfen\", in der IDE-Symbolleiste")](inspect-images/mac-heres-the-button.png#lightbox)

Ein neues Xamarin Inspector-Client-Fenster wird geöffnet, mit einer neuen REPL-Eingabeaufforderung.

[![](inspect-images/inspector-0.7.0-map-inspect-small.png "Ein neues Xamarin Inspector-Client-Fenster wird geöffnet, mit einer neuen REPL-Eingabeaufforderung")](inspect-images/inspector-0.7.0-map-inspect.png#lightbox)

Sobald dieses Fenster angezeigt wird, können Sie eine interaktive C#-Eingabeaufforderung verwenden, um C#-Anweisungen und Ausdrücke auszuwerten. Das Besondere dabei ist, dass der Code im Kontext des Zielprozesses ausgewertet wird. In diesem Fall zeigen wir den Code, der für die angezeigte iOS-Anwendung ausgeführt wird.

Änderungen am Zustand der Anwendung werden tatsächlich im Zielprozess durchgeführt, Sie können also C# verwenden, um die Anwendung in Echtzeit zu ändern oder zu überprüfen.

Beispielsweise unter iOS möchten wir unsere UIApplication-Delegatklasse, zu erhalten, das unsere Haupttreiber ist (wo wir viele Anwendungszustand speichern):

    var del = (MyApp.AppDelegate) UIApplication.SharedApplication.Delegate
    del.Database.GetAllCustomers ()
    ...
    del.Database.AddCustomer (...)

(Beachten Sie, dass bei jeder Übermittlung eines mehrzeiligen-Editors wird. `Shift + Enter` erstellt eine neue Zeile und `Cmd + Enter` (`Ctrl + Enter` für Windows) wird den Code für die Evaluierung absenden. `Enter` automatisch sendet, wenn es sicher ist.)

Angenehmer, um die visuellen Elemente der Anwendung zu erhalten, ist mithilfe der Schaltfläche "Prüfen". Wenn Sie auf diese klicken, können Sie ein Element der Benutzeroberfläche auswählen, durch Klicken auf die Anwendung. Die Variable `selectedView` zugewiesen wird, zeigen Sie auf das aktuelle Element auf dem Bildschirm. In diesem Screenshot können Sie sehen, wie wir zugegriffen, und klicken Sie dann bearbeitet `selectedView.BarTintColor` auf die `UISearchBar` wir ausgewählt haben.

Die aktive visuelle Struktur ist auch sehr nützlich. Es stellt die aktuelle Momentaufnahme für Ihre Hierarchie von Inhaltsansichten dar. Sie können auswählen, dass Zeilen festlegen `selectedView` in der REPL und Eigenschaftswerte der Ansicht anzuzeigen. Unter Mac können Sie mit einer 3D explodierten Visualisierung der überlappenden Ansichten interagieren. Auf Windows können Sie Eigenschaftswerte für eine Sicht visuell bearbeiten.

## <a name="known-limitations"></a>Bekannte Einschränkungen

- Auswahl der Ansicht wird nur auf Ihre wichtigsten Anzeige unterstützt.
- Eigenschaftenbearbeitung Raster ist nicht verfügbar für Mac, und für Windows ist auf einige Datentypen beschränkt. Verwenden Sie die Repl-Umgebung für eine leistungsfähigere Bearbeitung.
- Solange die Inspector-Add-In-/-Erweiterung installiert und in der IDE aktiviert, werden wir injizieren von Code in Ihrer app jedes Mal, wenn es im Debugmodus gestartet. Wenn Sie auf ungewöhnliche Verhaltensweisen in Ihrer app feststellen, wenden Sie Sie deaktivieren oder deinstallieren die Inspector-Add-In/Erweiterung, Neustart der IDE und anderen. Und Sie [senden Sie Fehlerinformationen](~/tools/inspector/install.md#reporting-bugs) auf uns zu informieren!
- Wenn ein Element der Benutzeroberfläche überprüft in trotzdem ändern wird, nehmen Sie [Teilen Sie uns](~/tools/inspector/install.md#reporting-bugs), da dies einen Fehler hinweisen.

