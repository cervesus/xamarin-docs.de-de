---
title: Xamarin.Forms-ListView
description: 'Dieser Leitfaden beschreibt die Xamarin.Forms-ListView, die Daten in ansprechender, interaktiver Listen dargestellt verwendet werden kann.'
ms.prod: xamarin
ms.assetid: FEFDF7E0-720F-4BD1-863F-4477226AA695
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/14/2015
---

# <a name="xamarinforms-listview"></a>Xamarin.Forms-ListView

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://developer.xamarin.com/samples/WorkingWithListview)

[`ListView`](xref:Xamarin.Forms.ListView) ist eine Ansicht für die Darstellung von Listen mit Daten, besonders lange Listen, die einen Bildlauf erfordern.

> [!IMPORTANT]
> `CollectionView` ist eine Ansicht für die Darstellung von Listen mit Daten anderes Layout-Spezifikationen verwenden. Es zielt darauf ab, die eine flexiblere, bereitstellen und leistungsfähige Alternative zu [ `ListView` ](xref:Xamarin.Forms.ListView). Weitere Informationen finden Sie unter [Xamarin.Forms CollectionView](~/xamarin-forms/user-interface/collectionview/index.md).

## <a name="use-cases"></a>Anwendungsfälle

Stellen Sie sicher, dass die ListView das richtige Steuerelement für Ihre Anforderungen ist. ListView kann in allen Fällen verwendet werden, in dem Sie einen bildlauffähige Listen mit Daten anzeigen möchten. ListViews unterstützt kontextaktionen und Datenbindung.

ListView dürfen nicht verwechselt werden, mit [TableView](~/xamarin-forms/user-interface/tableview.md). Das Steuerelement TableView ist eine bessere Option, wenn Sie eine nicht gebundenen Liste von Optionen oder Daten haben. Beispielsweise eignet sich die iOS-app-Einstellungen, die mit einer größtenteils vordefinierten Satzes von Optionen, besser TableView als ListView verwenden.

Beachten Sie, dass eine ListView am besten geeignet ist ebenfalls geeignet, für homogene Daten &ndash; , also sollte alle Daten vom selben Typ sein. Dies ist, da nur ein Typ der Zelle für jede Zeile in der Liste verwendet werden kann. TableViews kann mehrere Zellentypen unterstützen, damit sie eine bessere Option sind, wenn Sie zum Mischen von Ansichten müssen.

## <a name="components"></a>Komponenten
ListView verfügt über eine Reihe von Komponenten, führen Sie die systemeigene Funktionalität für jede Plattform zur Verfügung. Jede dieser Komponenten wird im folgenden beschrieben:

- **[Kopf- und Fußzeilen](customizing-list-appearance.md#Headers_and_Footers)**  &ndash; Text oder Sicht, für die anzuzeigenden am Anfang und Ende der Liste, zu trennen, aus der Liste. Kopf- und Fußzeilen können an eine Datenquelle unabhängig von der ListView auf die Datenquelle gebunden werden.
- **[Gruppen](customizing-list-appearance.md#Grouping)**  &ndash; Daten in einer ListView können zur leichteren Navigation gruppiert werden. Gruppen sind in der Regel Daten gebunden werden:

![](images/grouping-depth.png "ListView mit gruppierten Daten")

- **[Zellen](customizing-cell-appearance.md)**  &ndash; Daten in einer ListView werden in Zellen angezeigt. Jede Zelle entspricht eine Zeile mit Daten. Integrierte Zellen zur Auswahl vorhanden sind, oder Sie können eigene benutzerdefinierte Zelle definieren. Integrierte und benutzerdefinierte Zellen können in XAML oder Code verwendet oder definiert werden.
  - **[Integrierte](customizing-cell-appearance.md#Built_in_Cells)**  &ndash; erstellt in Zellen, insbesondere TextCell und ImageCell, kann sich hervorragend für die Leistung zu erzielen, sein, da diese in native Steuerelemente für jede Plattform entsprechen.
       - **[TextCell](customizing-cell-appearance.md#TextCell)**  &ndash; zeigt eine Zeichenfolge mit Text, optional mit zusätzlichen Text. Detail-Text wird als eine zweite Zeile in einer kleineren Schriftart mit einer Farbe für Akzente gerendert.
       - **[ImageCell](customizing-cell-appearance.md#ImageCell)**  &ndash; zeigt ein Bild mit Text. Wird als eine TextCell mit einem Bild auf der linken Seite angezeigt.
  - **[Benutzerdefinierte Zellen](customizing-cell-appearance.md#customcells)**  &ndash; benutzerdefinierte Zellen sind großartig, wenn Sie komplexe Daten präsentieren möchten. Beispielsweise könnte eine benutzerdefinierte Ansicht verwendet werden, um eine Liste von Songs, einschließlich Alben und Interpreteninformationen darzustellen:

![](images/image-cell-default.png "ListView mit ImageCells")

Weitere Informationen zum Anpassen von Zellen in einem ListView, finden Sie unter [Anpassen von ListView-Zellendarstellung](customizing-cell-appearance.md).

## <a name="functionality"></a>Funktionalität
ListView unterstützt eine Anzahl von Interaktion Formatvorlagen, einschließlich:

- **[Zum Aktualisieren ziehen](interactivity.md#Pull_to_Refresh)**  &ndash; ListView unterstützt Pull zum Aktualisieren auf jeder Plattform.
- **[Kontextaktionen](interactivity.md#Context_Actions)**  &ndash; ListView unterstützt Aktionen für einzelne Elemente in einer Liste. Beispielsweise können Sie implementieren Wischen-to-Action unter iOS oder lang Sie Aktionen für Android auf.
- **[Auswahl](interactivity.md#selectiontaps)**  &ndash; Sie können für die Auswahl und Deselections Maßnahmen zu ergreifen, wenn eine Zeile angetippt wird lauscht.

![](images/context-default.png "ListView mit Kontextaktionen")

Weitere Informationen zu den Interaktivitätsfunktionen von ListView finden Sie unter [Aktionen auf & Interaktivität mit ListView](interactivity.md).

## <a name="related-links"></a>Verwandte Links

- [Mit ListView arbeiten (Beispiel)](https://developer.xamarin.com/samples/WorkingWithListview)
- [Zwei Wege-Bindung (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/SwitchEntryTwoBinding)
- [In Zellen erstellt (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/BuiltInCells)
- [Benutzerdefinierte Zellen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/CustomCells)
- [Gruppierung (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/Grouping)
- [Benutzerdefinierte Renderer-Ansicht (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/WorkingWithListviewNative)
- [ListView-Interaktivität (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/interactivity)
