---
title: Überprüfen von Live-Anwendungen
description: In diesem Dokument wird beschrieben, wie Sie die Xamarin Inspector zum Überprüfen von Anwendungen verwenden. Außerdem werden die Einschränkungen des Xamarin Inspector Tools erläutert.
ms.prod: xamarin
ms.assetid: 91B3206E-B2A5-4660-A6E5-B924B8FE69A7
author: davidortinau
ms.author: daortin
ms.date: 06/19/2018
ms.openlocfilehash: bbb1e21139b5f073e2cc7e3d4781e8bc38334449
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73006302"
---
# <a name="inspecting-live-applications"></a>Überprüfen von Live-Anwendungen

Die Live-App-Prüfung ist für Unternehmenskunden verfügbar.

1. Öffnen Sie ein beliebiges [unterstütztes App-Projekt](~/tools/inspector/install.md#supported-platforms) in Visual Studio für Mac oder Visual Studio.
1. Führen Sie die Anwendung im Debugmodus aus.
1. Klicken Sie **in der IDE** -Symbolleiste auf die Schaltfläche "über **prüfen** " (in Visual Studio ist das Menü Element **aktuelle APP überprüfen** auch im Menü Extras oder **Debuggen** verfügbar).

[![](inspect-images/mac-heres-the-button.png "Click the Inspect button in the IDE toolbar")](inspect-images/mac-heres-the-button.png#lightbox)

Ein neues Xamarin Inspector Client Fenster wird mit einer neuen repl-Eingabeaufforderung geöffnet.

[![](inspect-images/inspector-0.7.0-map-inspect-small.png "A new Xamarin Inspector client window will open, with a fresh REPL prompt")](inspect-images/inspector-0.7.0-map-inspect.png#lightbox)

Wenn dieses Fenster angezeigt wird, verfügen Sie über C# eine interaktive Eingabeaufforderung, mit der Sie Anweisungen C# und Ausdrücke ausführen und auswerten können. Dies macht dies eindeutig, wenn der Code im Kontext des Ziel Prozesses ausgewertet wird. In diesem Fall zeigen wir den Code an, der für die angezeigte IOS-Anwendung ausgeführt wird.

Alle Änderungen, die Sie am Zustand der Anwendung vornehmen, werden tatsächlich im Ziel Prozess ausgeführt, sodass Sie verwenden C# können, um die Anwendung Live zu ändern, oder Sie können den Status der Anwendung Live überprüfen.

Beispielsweise möchten wir unter IOS möglicherweise die UIApplication-Delegatklasse suchen, bei der es sich um den Haupttreiber handelt (in dem wir einen Großteil des Anwendungs Zustands speichern):

```csharp
var del = (MyApp.AppDelegate) UIApplication.SharedApplication.Delegate
del.Database.GetAllCustomers ()
...
del.Database.AddCustomer (...)
```

(Beachten Sie, dass jede Übermittlung in einem mehrzeiligen Editor erfolgt. `Shift + Enter` wird eine neue Zeile erstellen, und `Cmd + Enter` (`Ctrl + Enter` unter Windows) sendet den Code für die Auswertung. `Enter` automatisch übermittelt, wenn er sicher ist.)

Eine bequemere Methode zum Erreichen der visuellen Elemente Ihrer Anwendung ist die Verwendung der Schaltfläche "überprüfen". Nachdem Sie dieses Element gedrückt haben, können Sie ein Benutzeroberflächen Element auswählen, indem Sie auf Ihre Anwendung klicken. Die Variable `selectedView` wird zugewiesen, um auf das tatsächliche Element auf dem Bildschirm zu zeigen. Im obigen Screenshot sehen Sie, wie wir auf den `UISearchBar`, den wir ausgewählt haben, auf die `selectedView.BarTintColor` zugegriffen und diese anschließend bearbeitet haben.

Die visuelle echt Zeitstruktur ist ebenfalls sehr nützlich. Stellt die aktuelle Momentaufnahme der Ansichts Hierarchie dar. Sie können Zeilen auswählen, um `selectedView` in der repl festzulegen und die Eigenschaftswerte der Sicht anzuzeigen. Unter Mac können Sie mit einer 3D-explodierten Visualisierung der geschichteten Ansichten interagieren. Unter Windows können Sie die Eigenschaftswerte einer Ansicht visuell bearbeiten.

## <a name="known-limitations"></a>Bekannte Einschränkungen

- Die Anzeigeauswahl wird nur auf der Haupt Anzeige unterstützt.
- Die Eigenschaften Raster Bearbeitung ist für Mac nicht verfügbar, und unter Windows ist auf einige Datentypen beschränkt. Verwenden Sie die repl für eine leistungsfähigere Bearbeitung.
- Solange das Inspektor-Add-in und das Inspector in Ihrer IDE installiert und aktiviert ist, wird bei jedem Start im Debugmodus Code in Ihre APP eingefügt. Wenn Sie ein seltsames Verhalten in Ihrer APP feststellen, versuchen Sie, das inspektoradd-in-Add-in zu deaktivieren oder zu deinstallieren, den Neustart der IDE zu starten und den Vorgang erneut auszuführen. Und bitte [melden Sie uns, um](~/tools/inspector/install.md#reporting-bugs) uns zu informieren.
- Wenn das Überprüfen eines UI-Elements bewirkt, dass es sich auf trotzdem ändert, [informieren Sie uns](~/tools/inspector/install.md#reporting-bugs), da dies möglicherweise auf einen Fehler hinweist.
