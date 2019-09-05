---
title: Überprüfen von Live-Anwendungen
description: In diesem Dokument wird beschrieben, wie Sie die Xamarin Inspector zum Überprüfen von Anwendungen verwenden. Außerdem werden die Einschränkungen des Xamarin Inspector Tools erläutert.
ms.prod: xamarin
ms.assetid: 91B3206E-B2A5-4660-A6E5-B924B8FE69A7
author: conceptdev
ms.author: crdun
ms.date: 06/19/2018
ms.openlocfilehash: 2ce4b0366e85580b6d9d816bd91f9ced93997b63
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291489"
---
# <a name="inspecting-live-applications"></a>Überprüfen von Live-Anwendungen

Die Überprüfung der Live-App steht für Enterprise-Kundne zur Verfügung.

1. Öffnen Sie ein [unterstütztes App-Projekt](~/tools/inspector/install.md#supported-platforms) in Visual Studio für Mac oder Visual Studio.
1. Führen Sie die Anwendung im Debugmodus aus.
1. Klicken Sie auf der Symbolleiste der IDE auf die Schaltfläche **Inspect** (Überprüfen) (in Visual Studio steht ebenfalls das Menüelement zum **Überprüfen der aktuellen App** über das Menü**Extras** oder **Debuggen** zur Verfügung).

[![](inspect-images/mac-heres-the-button.png "Klicken Sie in der IDE-Symbolleiste auf die Schaltfläche")](inspect-images/mac-heres-the-button.png#lightbox)

Ein neues Xamarin Inspector Client Fenster wird mit einer neuen repl-Eingabeaufforderung geöffnet.

[![](inspect-images/inspector-0.7.0-map-inspect-small.png "Ein neues Xamarin Inspector Client Fenster wird mit einer neuen repl-Eingabeaufforderung geöffnet.")](inspect-images/inspector-0.7.0-map-inspect.png#lightbox)

Sobald dieses Fenster angezeigt wird, können Sie eine interaktive C#-Eingabeaufforderung verwenden, um C#-Anweisungen und Ausdrücke auszuwerten. Das Besondere dabei ist, dass der Code im Kontext des Zielprozesses ausgewertet wird. In diesem Fall zeigen wir den Code, der für die angezeigte iOS-Anwendung ausgeführt wird.

Änderungen am Zustand der Anwendung werden tatsächlich im Zielprozess durchgeführt, Sie können also C# verwenden, um die Anwendung in Echtzeit zu ändern oder zu überprüfen.

Beispielsweise möchten wir unter IOS möglicherweise die UIApplication-Delegatklasse suchen, bei der es sich um den Haupttreiber handelt (in dem wir einen Großteil des Anwendungs Zustands speichern):

```csharp
var del = (MyApp.AppDelegate) UIApplication.SharedApplication.Delegate
del.Database.GetAllCustomers ()
...
del.Database.AddCustomer (...)
```

(Beachten Sie, dass jede Übermittlung in einem mehrzeiligen Editor erfolgt. `Shift + Enter`erstellt eine neue Zeile, und `Cmd + Enter` (`Ctrl + Enter` unter Windows) sendet den Code für die Auswertung. `Enter`wird automatisch übermittelt, wenn Sie sicher ist.)

Eine bequemere Methode zum Erreichen der visuellen Elemente Ihrer Anwendung ist die Verwendung der Schaltfläche "überprüfen". Nachdem Sie dieses Element gedrückt haben, können Sie ein Benutzeroberflächen Element auswählen, indem Sie auf Ihre Anwendung klicken. Die Variable `selectedView` wird zugewiesen, um auf das tatsächliche Element auf dem Bildschirm zu zeigen. Im obigen Screenshot sehen Sie, wie wir auf das `selectedView.BarTintColor` `UISearchBar` , was wir ausgewählt haben, aufgerufen und dann bearbeitet haben.

Die visuelle echt Zeitstruktur ist ebenfalls sehr nützlich. Stellt die aktuelle Momentaufnahme der Ansichts Hierarchie dar. Sie können Zeilen auswählen, die `selectedView` in der repl festgelegt werden sollen, und die Eigenschaftswerte der Sicht anzeigen. Unter Mac können Sie mit einer 3D-explodierten Visualisierung der geschichteten Ansichten interagieren. Unter Windows können Sie die Eigenschaftswerte einer Ansicht visuell bearbeiten.

## <a name="known-limitations"></a>Bekannte Einschränkungen

- Die Anzeigeauswahl wird nur auf der Haupt Anzeige unterstützt.
- Die Eigenschaften Raster Bearbeitung ist für Mac nicht verfügbar, und unter Windows ist auf einige Datentypen beschränkt. Verwenden Sie die repl für eine leistungsfähigere Bearbeitung.
- Solange das Inspektor-Add-in und das Inspector in Ihrer IDE installiert und aktiviert ist, wird bei jedem Start im Debugmodus Code in Ihre APP eingefügt. Wenn Sie ein seltsames Verhalten in Ihrer APP feststellen, versuchen Sie, das inspektoradd-in-Add-in zu deaktivieren oder zu deinstallieren, den Neustart der IDE zu starten und den Vorgang erneut auszuführen. Und bitte [melden Sie uns, um](~/tools/inspector/install.md#reporting-bugs) uns zu informieren.
- Wenn das Überprüfen eines UI-Elements bewirkt, dass es sich auf trotzdem ändert, [informieren Sie uns](~/tools/inspector/install.md#reporting-bugs), da dies möglicherweise auf einen Fehler hinweist.

