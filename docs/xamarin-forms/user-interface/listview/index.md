---
title: ListView
description: "Präsentieren von Daten in ansprechende, interaktive Listen."
ms.topic: article
ms.prod: xamarin
ms.assetid: FEFDF7E0-720F-4BD1-863F-4477226AA695
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/14/2015
ms.openlocfilehash: 5ad52fb3a3bb28511ab5a80b93304f3ff5e1b6fc
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="listview"></a>ListView

ListView ist eine Ansicht zum Darstellen von Listen mit Daten, besonders lange Listen, die einen Bildlauf erfordern. Dieses Handbuch erfahren Sie, wie Sie ListView verwenden:

1. **[Datenquellen](data-and-databinding.md)**  &ndash; eine Listenansicht mit Daten, mit oder ohne die Datenbindung zu füllen.
2. **[Darstellung der Zelle](customizing-cell-appearance.md)**  &ndash; Anpassen der Darstellung der integrierten Zellen oder eine eigene benutzerdefinierte Zelle erstellen.
3. **[Liste der Darstellung](customizing-list-appearance.md)**  &ndash; Anpassen der Darstellung der ListView. Festlegen von Kopf- und Fußzeilen, Gruppen zu aktivieren, und ändern Sie die Höhe der Zeilen.
4. **[Interaktivität](interactivity.md)**  &ndash; Taps und Auswahl verarbeiten, implementieren Sie zum Aktualisieren ziehen und kontextbezogene Aktionen hinzufügen.
5. **[Leistung](performance.md)**  &ndash; Leistungsprobleme zu vermeiden.

## <a name="use-cases"></a>Anwendungsfälle
Stellen Sie sicher, dass es sich bei ListView das richtige Steuerelement für Ihre Anforderungen ist. ListView kann in allen Situationen verwendet werden, in dem Sie bildlauffähige Listen der Daten angezeigt werden. Listenansichten Kontext Aktionen und die Datenbindung zu unterstützen.

ListView dürfen nicht mit verwechselt werden [TableView](~/xamarin-forms/user-interface/tableview.md). Das Steuerelement TableView ist eine bessere Option, wenn Sie eine nicht gebundene Liste von Optionen oder Daten haben. Beispielsweise eignet sich die iOS-app-Einstellungen, die größtenteils vordefinierte Optionen aufweist, besser als ListView TableView verwenden.

Beachten Sie, dass eine Listenansicht am besten geeignet ist außerdem geeignet, für homogene Daten &ndash; das heißt, alle Daten vom selben Typ sein sollte. Ursache hierfür ist nur eine Art der Zelle für jede Zeile in der Liste verwendet werden kann. TableViews kann mehrere Zellentypen unterstützen, damit sie eine bessere Option sind, wenn Sie zum Mischen von Ansichten müssen.


## <a name="components"></a>Komponenten
ListView wurde eine Reihe von Komponenten, die für die systemeigene Funktionalität für jede Plattform Übung verfügbar. Jede dieser Komponenten wird im folgenden beschrieben:

- **[Kopf- und Fußzeilen](customizing-list-appearance.md#Headers_and_Footers)**  &ndash; Text oder Ansicht am Anfang und Ende einer Liste anzeigen aus Daten in der Liste zu trennen. Kopf- und Fußzeilen können mit einer Datenquelle unabhängig von der ListView-Datenquelle gebunden werden.
- **[Gruppen](customizing-list-appearance.md#Grouping)**  &ndash; Daten in einem ListView können zur leichteren Navigation gruppiert werden. Gruppen sind in der Regel Daten gebunden:

![](images/grouping-depth.png "ListView mit gruppierten Daten")

- **[Zellen](customizing-cell-appearance.md)**  &ndash; Daten in einem ListView werden in Zellen angezeigt. Jede Zelle entspricht einer Datenzeile. Es Arebuilt in Zellen auswählen, oder Sie können eigene benutzerdefinierte Zelle definieren. Integrierte und benutzerdefinierte Zellen können in XAML oder Code verwendet oder definiert werden.
  - **[Integrierte](customizing-cell-appearance.md#Built_in_Cells)**  &ndash; aus Zellen, insbesondere TextCell und ImageCell erstellt, können sich hervorragend für die Leistung zu erzielen, sein, da diese systemeigenen Steuerelementen für jede Plattform entsprechen.
    - **[TextCell](customizing-cell-appearance.md#TextCell)**  &ndash; eine Textzeichenfolge, optional mit Details Text angezeigt. Detail-Text wird als eine zweite Zeile in einer kleineren Schriftart mit einem Akzentfarbe gerendert.
    - **[ImageCell](customizing-cell-appearance.md#ImageCell)**  &ndash; zeigt ein Bild mit Text. Wird als eine TextCell mit einem Bild auf der linken Seite angezeigt.
  - **[Benutzerdefinierte Zellen](customizing-cell-appearance.md#customcells)**  &ndash; benutzerdefinierte Zellen sind hervorragend, wenn Sie komplexe Daten bereitstellen müssen. Beispielsweise konnte eine benutzerdefinierte Ansicht verwendet werden, um eine Liste von Songs ermöglicht, einschließlich Album und Interpreten darzustellen:

![](images/image-cell-default.png "ListView mit ImageCells")

Weitere Informationen zum Anpassen von Zellen in einem ListView finden Sie unter [Anpassen der Darstellung der ListView Zelle](customizing-cell-appearance.md).

## <a name="functionality"></a>Funktionalität
ListView unterstützt eine Reihe von Interaktionen Formatvorlagen, einschließlich:

- **[Zum Aktualisieren ziehen](interactivity.md#Pull_to_Refresh)**  &ndash; ListView Pull zum Aktualisieren auf jeder Plattform unterstützt.
- **[Kontext Aktionen](interactivity.md#Context_Actions)**  &ndash; ListView unterstützt Maßnahmen für einzelne Elemente in einer Liste. Angenommen, können Sie implementieren Wischen-Action-auf iOS oder Long-Tap Aktionen unter Android und Windows Phone.
- **[Auswahl](interactivity.md#selectiontaps)**  &ndash; können Sie überwachen, für die Auswahl und Deselections Maßnahmen zu ergreifen, wenn eine Zeile abgerufen werden.

![](images/context-default.png "ListView mit Kontext Aktionen")

Weitere Informationen zu den Interaktivitätsfunktionen des ListView finden Sie unter [Aktionen & Interaktivität ListView](interactivity.md).


## <a name="related-links"></a>Verwandte Links

- [Arbeiten mit ListView (Beispiel)](https://developer.xamarin.com/samples/WorkingWithListview)
- [Zwei Weise binden (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/SwitchEntryTwoBinding)
- [In Zellen erstellt (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/BuiltInCells)
- [Benutzerdefinierte Zellen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/CustomCells)
- [Gruppierung (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/Grouping)
- [Benutzerdefinierte Ansicht der Renderer (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/WorkingWithListviewNative)
- [ListView-Interaktivität (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/interactivity)
- [iOS-Arbeitsmappe](https://developer.xamarin.com/workbooks/xamarin-forms/user-interface/listview/ListView-ios.workbook)
- [Android Workbook](https://developer.xamarin.com/workbooks/xamarin-forms/user-interface/listview/ListView-android.workbook)
