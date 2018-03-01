---
title: "Überprüfen des Live-Anwendungen"
description: Visualisieren und Debuggen der live-app
ms.topic: article
ms.prod: xamarin
ms.assetid: 91B3206E-B2A5-4660-A6E5-B924B8FE69A7
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/29/2017
ms.openlocfilehash: bd6d47f98435cc68ecf4156423526c31dbac09da
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="inspecting-live-applications"></a>Überprüfen des Live-Anwendungen

Aktive app Inspektion ist für Enterprise-Kunden zur Verfügung.


1. [Installieren Sie Xamarin Arbeitsmappen & Inspektor.](~/tools/inspector/install.md)

1. Öffnen Sie einen [app-Projekt unterstützt](~/tools/inspector/install.md#supported-platforms) in Visual Studio für Mac oder im Visual Studio.
1. Führen Sie die Anwendung im Debugmodus aus.
1. Klicken Sie auf die **Inspect** auf der Symbolleiste der IDE (in Visual Studio die **aktuelle app überprüfen...**  Menüelement steht auch auf die **Tools** oder **Debuggen** Menü).



[ ![](inspect-images/mac-heres-the-button.png "Klicken Sie auf die Schaltfläche "prüfen", in der IDE-Symbolleiste")](inspect-images/mac-heres-the-button.png)

Ein neue Xamarin-Inspektor-Client-Fenster wird mit einer neuen REPL-Eingabeaufforderung geöffnet.

[ ![](inspect-images/inspector-0.7.0-map-inspect-small.png "Ein neue Xamarin-Inspektor-Client-Fenster wird geöffnet, mit einer neuen REPL-Eingabeaufforderung")](inspect-images/inspector-0.7.0-map-inspect.png)

Nachdem dieses Fenster angezeigt wird, müssen Sie eine interaktive C#-Eingabeaufforderung, die Sie verwenden können, um auszuführen und C#-Anweisungen und Ausdrücke auswerten. Was ist dies eindeutig, dass der Code im Kontext des Zielprozesses ausgewertet wird. In diesem Fall zeigen wir den Code für iOS-Anwendung angezeigt.

Änderungen, die Sie in den Zustand der Anwendung werden auf den Zielprozess tatsächlich geschieht, damit Sie verwenden können live c# so ändern Sie die Anwendung oder können Sie den Status der Anwendung live überprüfen.

Beispielsweise IOS möchten wir unsere UIApplication Delegate-Klasse zu ermitteln, die unsere Haupt-Treiber ist (, in dem wir einen Großteil der Anwendungsstatus gespeichert):

    var del = (MyApp.AppDelegate) UIApplication.SharedApplication.Delegate
    del.Database.GetAllCustomers ()
    ...
    del.Database.AddCustomer (...)

(Beachten Sie, dass jede Übermittlung in einem mehrzeiligen-Editor auftritt. `Shift + Enter` erstellt eine neue Zeile und `Cmd + Enter` (`Ctrl + Enter` unter Windows), sendet den Code für die Evaluierung. `Enter` automatisch übermittelt, sobald es sicher ist.)

Ein Bequemerer Weg, um die visuellen Elemente der Anwendung zu erhalten, wird mithilfe der Schaltfläche "Prüfen". Nachdem Sie diese gedrückt haben, können Sie ein Element der Benutzeroberfläche durch Klicken auf die Anwendung auswählen. Die Variable `selectedView` zugewiesen wird, zeigen Sie auf das aktuelle Element auf dem Bildschirm. In der oben dargestellten Screenshot können Sie sehen, wie wir zugegriffen, und klicken Sie dann bearbeitet `selectedView.BarTintColor` auf die `UISearchBar` wir ausgewählt wurde.

Visuelle echtzeitstruktur ist ebenfalls sehr nützlich. Es stellt die aktuelle Momentaufnahme Ihrer Hierarchie anzeigen. Sie können Zeilen festzulegende auswählen `selectedView` in die Textgröße für Replikation und Eigenschaftswerte für die Ansicht angezeigt. Auf einem Mac können Sie mit einer 3D explodierten Visualisierung der geschichteten Sichten interagieren. Unter Windows können Sie Eigenschaftswerte für eine Sicht visuell bearbeiten.

## <a name="known-limitations"></a>Bekannte Einschränkungen

 - Auswahl anzeigen, wird nur auf Ihre wichtigsten Anzeige unterstützt.
 - Raster eigenschaftenbearbeitung ist nicht verfügbar für Mac und unter Windows ist auf einige Datentypen beschränkt. Verwenden Sie die Textgröße für Replikation für eine leistungsfähigere Bearbeitung.
 - Solange die Inspektor-Add-In/Erweiterung installiert und in die IDE aktiviert ist, werden wir Code in Ihrer app Räumen, jedes Mal, wenn sie im Debugmodus gestartet wird. Wenn Sie in Ihrer app jede ungewöhnliches Verhalten bemerken, bitte versuchen Sie es deaktivieren oder Deinstallieren der Inspektor-Add-In/Erweiterung, neu zu starten der IDE und erneute. Und Sie [Problemen](~/tools/inspector/install.md#reporting-bugs) an uns das mitzuteilen.
 - Wenn ein Benutzeroberflächenelement überprüfen jedoch trotzdem in ändern ein auftritt, wenden [informieren Sie uns](~/tools/inspector/install.md#reporting-bugs), wie dies einen Fehler hinweisen.

