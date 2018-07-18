---
title: Xamarin.Forms DataPages
description: Dieser Artikel enthält die Xamarin.Forms-DataPages, stellen eine API, um schnell und problemlos eine Datenquelle mit vordefinierten Ansichten binden.
ms.prod: xamarin
ms.assetid: DF16EAEE-DB78-42CA-9C59-51D9D6CB6B95
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 2a74b636a41a72b26776157a774f0a33ef45a075
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815886"
---
# <a name="xamarinforms-datapages"></a>Xamarin.Forms DataPages

![](~/media/shared/preview.png "Diese API ist derzeit als Vorschauversion")

> [!IMPORTANT]
> DataPages erfordert eine [Xamarin.Forms Design](~/xamarin-forms/user-interface/themes/index.md) Verweis auf das Rendern.

Xamarin.Forms DataPages auf Evolve 2016 angekündigt wurden und sind als Vorschau für Kunden, testen und Feedback zur Verfügung.

DataPages Geben Sie eine API, um schnell und einfach zu eine Datenquelle mit vordefinierten Ansichten binden. Auflisten von Elementen Detailseiten rendert die Daten automatisch und können unter Verwendung von Designs angepasst werden.

Um anzuzeigen, wie die Evolve-Keynote-Demo funktioniert, sehen Sie sich die [Handbuch mit ersten Schritten](get-started.md).

[![](images/demo-sml.png "Beispielanwendung DataPages")](images/demo.png#lightbox "DataPages-Beispielanwendung")

## <a name="introduction"></a>Einführung

Datenquellen und den zugehörigen Seiten ermöglichen Entwicklern, schnell und einfach eine unterstützte Datenquelle verwenden und es mithilfe des integrierten Gerüstbau, die Benutzeroberfläche angepasst werden kann, mit Designs zu rendern.

DataPages werden zu einer Xamarin.Forms-Anwendung hinzugefügt, durch Einschließen der **Xamarin.Forms.Pages** Nuget-Paket.

### <a name="data-sources"></a>Datenquellen

Die Vorschau hat einige vorgefertigte Datenquellen, die für die Verwendung zur Verfügung:

* **JsonDataSource**
* **AzureDataSource** (trennen Sie Nuget)
* **AzureEasyTableDataSource** (trennen Sie Nuget)

Finden Sie unter den [Handbuch mit ersten Schritten](get-started.md) ein Beispiel mit einem `JsonDataSource`.


### <a name="pages--controls"></a>Seiten und Steuerelemente

Die folgenden Seiten und Steuerelemente werden eingeschlossen, um die einfache Bindung mit den angegebenen Datenquellen zuzulassen:

* **ListDataPage** – finden Sie unter den [Einstieg Beispiel](get-started.md).
* **"Directorypage"** – eine Liste mit aktivierter Gruppierung.
* **PersonDetailPage** – ein einzelnes Datenelement anzeigen, die für einen bestimmten Objekttyp (einen Kontakteintrag) angepasst.
* **DataView** – eine Ansicht, um Daten aus der Quelle in generischer Form verfügbar zu machen.
* **CardView** – im Stil an, die ein Bild, Titeltext und Beschreibungstext enthält.
* **HeroImage** – eine Image-Rendering-Ansicht.
* **ListItem** : eine vorkonfigurierte Ansicht mit einem Layout ähnelt native IOS- und Android-Listenelemente.

Finden Sie unter den [DataPages Steuerelemente Referenz](controls.md) Beispiele.



### <a name="under-the-hood"></a>Im Hintergrund

Eine Xamarin.Forms-Datenquelle entspricht der `IDataSource` Schnittstelle.

Die Xamarin.Forms-Infrastruktur interagiert mit einer Datenquelle über die folgenden Eigenschaften:

* `Data` – eine schreibgeschützte Liste von Datenelementen, die angezeigt werden können.
* `IsLoading` – Ein boolescher Wert, der angibt, ob die Daten geladen und für das verfügbar ist.
* `[key]` – ein Indexer zum Abrufen von Elementen.

Es gibt zwei Methoden `MaskKey` und `UnmaskKey` , die verwendet werden kann, (oder ausblenden) Elementeigenschaften (d. h. verhindern sie gerenderte).
Der Schlüssel entspricht das eine benannte Eigenschaft für das Datenobjekt für das Element.
