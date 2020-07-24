---
title: Überprüfen von Live-Anwendungen
description: In diesem Dokument wird beschrieben, wie Sie die Xamarin Inspector zum Überprüfen von Anwendungen verwenden. Außerdem werden die Einschränkungen des Xamarin Inspector Tools erläutert.
ms.prod: xamarin
ms.assetid: 91B3206E-B2A5-4660-A6E5-B924B8FE69A7
author: davidortinau
ms.author: daortin
ms.date: 06/19/2018
ms.openlocfilehash: 4e4a231b6ae8dda3417cd32f95850a44d6c72260
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86939385"
---
# <a name="inspecting-live-applications"></a>Überprüfen von Live-Anwendungen

Die Live-App-Prüfung ist für Unternehmenskunden verfügbar.

1. Öffnen Sie ein beliebiges [unterstütztes App-Projekt](~/tools/inspector/install.md#supported-platforms) in Visual Studio für Mac oder Visual Studio.
1. Führen Sie die Anwendung im Debugmodus aus.
1. Klicken Sie **in der IDE** -Symbolleiste auf die Schaltfläche "über **prüfen** " (in Visual Studio ist das Menü Element **aktuelle APP überprüfen** auch im Menü Extras oder **Debuggen** verfügbar).

[![Klicken Sie in der IDE-Symbolleiste auf die Schaltfläche](inspect-images/mac-heres-the-button.png)](inspect-images/mac-heres-the-button.png#lightbox)

Ein neues Xamarin Inspector Client Fenster wird mit einer neuen repl-Eingabeaufforderung geöffnet.

[![Ein neues Xamarin Inspector Client Fenster wird mit einer neuen repl-Eingabeaufforderung geöffnet.](inspect-images/inspector-0.7.0-map-inspect-small.png)](inspect-images/inspector-0.7.0-map-inspect.png#lightbox)

Wenn dieses Fenster angezeigt wird, verfügen Sie über eine interaktive c#-Eingabeaufforderung, mit der Sie c#-Anweisungen und-Ausdrücke ausführen und auswerten können. Dies macht dies eindeutig, wenn der Code im Kontext des Ziel Prozesses ausgewertet wird. In diesem Fall zeigen wir den Code an, der für die angezeigte IOS-Anwendung ausgeführt wird.

Alle Änderungen, die Sie am Zustand der Anwendung vornehmen, werden tatsächlich im Ziel Prozess ausgeführt, sodass Sie c# verwenden können, um die Anwendung Live zu ändern, oder Sie können den Status der Anwendung Live überprüfen.

Beispielsweise möchten wir unter IOS möglicherweise die UIApplication-Delegatklasse suchen, bei der es sich um den Haupttreiber handelt (in dem wir einen Großteil des Anwendungs Zustands speichern):

```csharp
var del = (MyApp.AppDelegate) UIApplication.SharedApplication.Delegate
del.Database.GetAllCustomers ()
...
del.Database.AddCustomer (...)
```

(Beachten Sie, dass jede Übermittlung in einem mehrzeiligen Editor erfolgt. `Shift + Enter`erstellt eine neue Zeile, und `Cmd + Enter` ( `Ctrl + Enter` unter Windows) sendet den Code für die Auswertung. `Enter`wird automatisch übermittelt, wenn Sie sicher ist.)

Eine bequemere Methode zum Erreichen der visuellen Elemente Ihrer Anwendung ist die Verwendung der Schaltfläche "überprüfen". Nachdem Sie dieses Element gedrückt haben, können Sie ein Benutzeroberflächen Element auswählen, indem Sie auf Ihre Anwendung klicken. Die Variable `selectedView` wird zugewiesen, um auf das tatsächliche Element auf dem Bildschirm zu zeigen. Im obigen Screenshot sehen Sie, wie wir auf das, was wir ausgewählt haben, aufgerufen und dann bearbeitet haben `selectedView.BarTintColor` `UISearchBar` .

Die visuelle echt Zeitstruktur ist ebenfalls sehr nützlich. Stellt die aktuelle Momentaufnahme der Ansichts Hierarchie dar. Sie können Zeilen auswählen, die `selectedView` in der repl festgelegt werden sollen, und die Eigenschaftswerte der Sicht anzeigen. Unter Mac können Sie mit einer 3D-explodierten Visualisierung der geschichteten Ansichten interagieren. Unter Windows können Sie die Eigenschaftswerte einer Ansicht visuell bearbeiten.

## <a name="known-limitations"></a>Bekannte Einschränkungen

- Die Anzeigeauswahl wird nur auf der Haupt Anzeige unterstützt.
- Die Eigenschaften Raster Bearbeitung ist für Mac nicht verfügbar, und unter Windows ist auf einige Datentypen beschränkt. Verwenden Sie die repl für eine leistungsfähigere Bearbeitung.
- Solange das Inspektor-Add-in und das Inspector in Ihrer IDE installiert und aktiviert ist, wird bei jedem Start im Debugmodus Code in Ihre APP eingefügt. Wenn Sie ein seltsames Verhalten in Ihrer APP feststellen, versuchen Sie, das inspektoradd-in-Add-in zu deaktivieren oder zu deinstallieren, den Neustart der IDE zu starten und den Vorgang erneut auszuführen. Und bitte [melden Sie uns, um](~/tools/inspector/install.md#reporting-bugs) uns zu informieren.
- Wenn das Überprüfen eines UI-Elements bewirkt, dass es sich auf trotzdem ändert, [informieren Sie uns](~/tools/inspector/install.md#reporting-bugs), da dies möglicherweise auf einen Fehler hinweist.
