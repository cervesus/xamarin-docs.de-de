---
title: Xamarin.Forms-ListView
description: In diesem Leitfaden wird die ListView xamarin. Forms vorgestellt, die zum Darstellen von Daten in interaktiven Listen verwendet werden kann.
ms.prod: xamarin
ms.assetid: FEFDF7E0-720F-4BD1-863F-4477226AA695
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/04/2019
ms.openlocfilehash: 9d7dd98b98c7722d6fcdcb49c10e326732fa9f0b
ms.sourcegitcommit: a5ef4497db04dfa016865bc7454b3de6ff088554
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/15/2019
ms.locfileid: "70997981"
---
# <a name="xamarinforms-listview"></a>Xamarin.Forms-ListView

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistview)

[`ListView`](xref:Xamarin.Forms.ListView)ist eine Ansicht für die Darstellung von Listen mit Daten, insbesondere für lange Listen, die einen Bildlauf erfordern.

> [!IMPORTANT]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView) ist eine Ansicht für die Darstellung von Listen mit Daten mit unterschiedlichen Layoutspezifikationen. Es zielt darauf ab, eine flexiblere und leistungsfähigere Alternative zu [`ListView`](xref:Xamarin.Forms.ListView)bereitzustellen. Weitere Informationen finden Sie unter [Xamarin.Forms: CollectionView](~/xamarin-forms/user-interface/collectionview/index.md).

## <a name="use-cases"></a>Anwendungsfälle

Ein `ListView` -Steuerelement kann in jeder Situation verwendet werden, in der Sie scrollbare Listen mit Daten anzeigen. Die `ListView` -Klasse unterstützt Kontext Aktionen und Datenbindung.

Das `ListView` -Steuerelement sollte nicht mit [`TableView`](~/xamarin-forms/user-interface/tableview.md) dem-Steuerelement verwechselt werden. Das `TableView` -Steuerelement ist eine bessere Option, wenn Sie über eine nicht gebundene Liste von Optionen oder Daten verfügen, da damit vordefinierte Optionen in XAML angegeben werden können. Beispielsweise ist die IOS-Einstellungs-APP, die überwiegend vordefinierten Satz von Optionen verfügt, besser für die Verwendung `TableView` eines `ListView`als geeignet.

Die `ListView` -Klasse unterstützt das Definieren von Listenelementen in XAML nicht. `ItemsSource` Sie müssen die-Eigenschaft oder `ItemTemplate` die Datenbindung mit einem verwenden, um Elemente in der Liste zu definieren.

Eine `ListView` eignet sich am besten für Sammlungen, die aus einem einzelnen Datentyp bestehen. Diese Anforderung liegt daran, dass für jede Zeile in der Liste nur ein Zellentyp verwendet werden kann. Das `TableView` Steuerelement kann mehrere Zelltypen unterstützen, daher ist es eine bessere Option, wenn Sie mehrere Datentypen anzeigen müssen.

Weitere Informationen zum Binden von Daten an eine `ListView` -Instanz finden Sie unter [ListView-Datenquellen](~/xamarin-forms/user-interface/listview/data-and-databinding.md).

## <a name="components"></a>Komponenten

Das `ListView` -Steuerelement verfügt über eine Reihe von Komponenten, die für die systemeigene Funktionalität der einzelnen Plattformen verfügbar sind. Diese Komponenten werden in den folgenden Abschnitten definiert.

### <a name="headers-and-footerscustomizing-list-appearancemdheaders-and-footers"></a>[Kopf-und Fußzeilen](customizing-list-appearance.md#headers-and-footers)

Kopf-und Fußzeilen Komponenten werden am Anfang und am Ende einer Liste getrennt von den Daten der Liste angezeigt. Kopf-und Fußzeilen können aus der Datenquelle von ListView an eine separate Datenquelle gebunden werden.

### <a name="groupscustomizing-list-appearancemdgrouping"></a>[Musik](customizing-list-appearance.md#grouping)

Daten in einer `ListView` können zur einfacheren Navigation gruppiert werden. Gruppen sind in der Regel Daten gebunden. Der folgende Screenshot zeigt eine `ListView` mit gruppierten Daten:

Gruppierte Daten [in einem ListView-gruppierten Daten in einem ListView-Steuer Punkt ![](images/grouping-depth-cropped.png)](images/grouping-depth.png#lightbox "")

### <a name="cellscustomizing-cell-appearancemd"></a>[Zellen](customizing-cell-appearance.md)

Datenelemente in einem `ListView` werden als Zellen bezeichnet. Jede Zelle entspricht eine Zeile mit Daten. Integrierte Zellen zur Auswahl vorhanden sind, oder Sie können eigene benutzerdefinierte Zelle definieren. Integrierte und benutzerdefinierte Zellen können in XAML oder Code verwendet oder definiert werden.

- [Integrierte Zellen](customizing-cell-appearance.md#built-in-cells), wie z `TextCell` . b. und `ImageCell`, entsprechen systemeigenen Steuerelementen und sind besonders leistungsfähig.
  - Eine [`TextCell`](customizing-cell-appearance.md#textcell) zeigt eine Text Zeichenfolge an, optional mit Detail Text. Detail-Text wird als eine zweite Zeile in einer kleineren Schriftart mit einer Farbe für Akzente gerendert.
  - Ein [`ImageCell`](customizing-cell-appearance.md#imagecell) zeigt ein Bild mit Text an. Wird als `TextCell` mit einem Bild auf der linken Seite angezeigt.
- [Benutzerdefinierte Zellen](customizing-cell-appearance.md#custom-cells) werden verwendet, um komplexe Daten darzustellen. Beispielsweise könnte eine benutzerdefinierte Zelle verwendet werden, um eine Liste von Liedern zu präsentieren, die das Album und den Künstler enthält.

Der folgende Screenshot zeigt ein `ListView` -Element mit ImageCell-Elementen:

ImageCell- [Elemente in einem ListView-ImageCell-Element in einem ListView-Element ![](images/image-cell-default-cropped.png)](images/image-cell-default.png#lightbox "")

Weitere Informationen zum Anpassen von Zellen in einem `ListView`finden Sie unter [Anpassen der ListView-Zellen](customizing-cell-appearance.md)Darstellung.

## <a name="functionality"></a>Funktionalität

Die `ListView` -Klasse unterstützt eine Reihe von Interaktions Stilen.

- Mit [Pull-to-refresh](interactivity.md#pull-to-refresh) kann der Benutzer den `ListView` Inhalt nach unten ziehen, um ihn zu aktualisieren.
- [Kontext Aktionen](interactivity.md#context-actions) ermöglichen es dem Entwickler, benutzerdefinierte Aktionen für einzelne Listenelemente anzugeben. Beispielsweise können Sie implementieren Wischen-to-Action unter iOS oder lang Sie Aktionen für Android auf.
- Die [Auswahl](interactivity.md#selection-and-taps) ermöglicht dem Entwickler das Anfügen von Funktionen an Auswahl-und Auswahl Ereignisse für Listenelemente.

Der folgende Screenshot zeigt eine `ListView` mit Kontext Aktionen:

Kontext Aktionen [in einem ListView-Kontext in einer ListView ![](images/context-default-cropped.png)](images/context-default.png#lightbox "")

Weitere Informationen zu den Interaktivitätsfunktionen von `ListView`finden Sie unter [Aktionen & Interaktivität mit ListView](interactivity.md).

## <a name="related-links"></a>Verwandte Links

- [Mit ListView arbeiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistview)
- [Zwei Wege-Bindung (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-switchentrytwobinding)
- [In Zellen erstellt (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-builtincells)
- [Benutzerdefinierte Zellen (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-customcells)
- [Gruppierung (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-grouping)
- [Benutzerdefinierte Renderer-Ansicht (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative/)
- [ListView-Interaktivität (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-interactivity)
