---
title: DataPages
ms.topic: article
ms.prod: xamarin
ms.assetid: DF16EAEE-DB78-42CA-9C59-51D9D6CB6B95
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 60973068b56ea4160c3e5ae53d387b063c601afe
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="datapages"></a>DataPages

![](~/media/shared/preview.png "Diese API ist derzeit als Vorschau verfügbar")

> [!IMPORTANT]
> DataPages erfordert eine [Xamarin.Forms Design](~/xamarin-forms/user-interface/themes/index.md) Verweis auf das Rendern.

Xamarin.Forms DataPages Evolve 2016 vorgestellt wurden und als Vorschau für Kunden, um zu testen und Bereitstellen von Feedback verfügbar sind.

DataPages bieten eine API, um schnell und einfach mit vorgefertigten Ansichten eine Datenquelle binden. Listenelemente Detailseiten die Daten werden automatisch gerendert und Designs mit angepasst werden können.

Um zu sehen, wie die Evolve Keynote Demo funktioniert, sehen Sie sich die [Handbuch mit ersten Schritten](get-started.md).

[![](images/demo-sml.png "DataPages-Beispielanwendung")](images/demo.png#lightbox "DataPages-Beispielanwendung")

## <a name="introduction"></a>Einführung

Datenquellen und die zugehörigen Datenseiten können Entwickler schnell und einfach eine unterstützten Datenquelle nutzen und Rendern Designs mit integrierte Benutzeroberfläche Gerüstbau, die angepasst werden kann.

DataPages werden zu einer Xamarin.Forms-Anwendung hinzugefügt, dazu den **Xamarin.Forms.Pages** NuGet-Paket.

### <a name="data-sources"></a>Datenquellen

Die Vorschau hat einige vordefinierte Datenquellen, die für die Verwendung zur Verfügung:

* **JsonDataSource**
* **AzureDataSource** (trennen Sie Nuget)
* **AzureEasyTableDataSource** (trennen Sie Nuget)

Finden Sie unter der [Handbuch mit ersten Schritten](get-started.md) für ein Beispiel zur Verwendung einer `JsonDataSource`.


### <a name="pages--controls"></a>Seiten und Steuerelemente

Die folgenden Seiten und Steuerelemente sind enthalten, um einfache Bindung mit den angegebenen Datenquellen zu ermöglichen:

* **ListDataPage** – finden Sie unter der [Einstieg Beispiel](get-started.md).
* **DirectoryPage** – eine Liste mit Gruppen aktiviert.
* **PersonDetailPage** – ein einzelnes Datenelement anzeigen, die für einen bestimmten Objekttyp (eine Kontakt-Eintrag) angepasst.
* **"DataView"** – eine Ansicht, um Daten aus der Quelldatenbank auf generische Weise verfügbar zu machen.
* **CardView** – eine Ansicht, ein Bild, Titeltext und Description-Text enthält, formatiert.
* **HeroImage** – eine Image-Rendering-Sicht.
* **"ListItem"** – eine vorkonfigurierte Ansicht mit einem Layout nativer IOS- und Android Listenelemente ähnelt.

Finden Sie unter der [DataPages steuert Verweis](controls.md) Beispiele.



### <a name="under-the-hood"></a>Hinter den Kulissen

Eine Datenquelle Xamarin.Forms unterliegen die `IDataSource` Schnittstelle.

Die Infrastruktur Xamarin.Forms interagiert mit einer Datenquelle über die folgenden Eigenschaften:

* `Data` – eine schreibgeschützte Liste von Datenelementen, die angezeigt werden können.
* `IsLoading` – Ein boolescher Wert, der angibt, ob die Daten geladen und für das Rendering verfügbar sind.
* `[key]` – nach einem Indexer, um die Elemente abgerufen werden sollen.

Es gibt zwei Methoden `MaskKey` und `UnmaskKey` , die verwendet werden kann, und ausblenden () Elementeigenschaften (d. h. verhindern sie gerendert wird).
Der Schlüssel entspricht der eine benannte Eigenschaft für das Element Datenobjekt.

