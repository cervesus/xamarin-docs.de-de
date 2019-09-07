---
title: Xamarin. Forms-DataPages
description: In diesem Artikel werden xamarin. Forms-DataPages vorgestellt, die eine API bereitstellen, mit der eine Datenquelle schnell und einfach an vordefinierte Sichten gebunden werden kann.
ms.prod: xamarin
ms.assetid: DF16EAEE-DB78-42CA-9C59-51D9D6CB6B95
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 1dc62066b71842e1d3b07495912fa35a549c0f1e
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70759685"
---
# <a name="xamarinforms-datapages"></a>Xamarin. Forms-DataPages

![](~/media/shared/preview.png "Diese API ist derzeit als Vorschauversion")

> [!IMPORTANT]
> Für DataPages ist ein xamarin. Forms-Design Verweis zum Rendering erforderlich. Dies umfasst das Installieren des [xamarin. Forms. Theme. Base](https://www.nuget.org/packages/Xamarin.Forms.Theme.Base/) -nuget-Pakets in Ihrem Projekt, gefolgt von den nuget-Paketen [xamarin. Forms. Theme. Light](https://www.nuget.org/packages/Xamarin.Forms.Theme.Light/) oder [xamarin. Forms. Theme. Dark](https://www.nuget.org/packages/Xamarin.Forms.Theme.Dark/) .

Xamarin. Forms-Datentypen wurden bei Weiterentwicklung 2016 angekündigt und stehen als Vorschau für Kunden zur Verfügung, um uns Feedback zu senden.

DataPages Geben Sie eine API, um schnell und einfach zu eine Datenquelle mit vordefinierten Ansichten binden. Listenelemente und Detailseiten werden die Daten automatisch Rendering und können mithilfe von Designs angepasst werden.

Sehen Sie sich die [Anleitung](get-started.md)für die ersten Schritte an.

[![](images/demo-sml.png "Beispielanwendung DataPages")](images/demo.png#lightbox "DataPages-Beispielanwendung")

## <a name="introduction"></a>Einführung

Mithilfe von Datenquellen und den zugehörigen Datenseiten können Entwickler schnell und einfach eine unterstützte Datenquelle nutzen und diese mithilfe des integrierten UI-Gerüsts, das mit Designs angepasst werden kann, wiedergeben.

DataPages werden einer xamarin. Forms-Anwendung hinzugefügt, indem das nuget-Paket **xamarin. Forms. Pages** eingeschlossen wird.

### <a name="data-sources"></a>Datenquellen

Die Vorschau bietet einige vorgefertigte Datenquellen, die für die Verwendung zur Verfügung stehen:

* **JsonDataSource**
* **Azuredatasource** (separates nuget)
* **Azureeasytabledatasource** (separates nuget)

Ein Beispiel für die Verwendung von finden Sie `JsonDataSource`im [Leitfaden zu](get-started.md) den ersten Schritten.

### <a name="pages--controls"></a>Seiten & Steuerelementen

Die folgenden Seiten und Steuerelemente sind enthalten, um eine einfache Bindung an die angegebenen Datenquellen zu ermöglichen:

* **Listdatapage** – siehe das [Beispiel](get-started.md)für die ersten Schritte.
* **Directoriypage** – eine Liste mit aktivierter Gruppierung.
* **Persondetailpage** – eine einzelne Datenelement Sicht, die für einen bestimmten Objekttyp (einen Kontakt Eintrag) angepasst ist.
* **DataView** – eine Sicht, mit der Daten aus der Quelle generisch zugänglich gemacht werden.
* **CardView** – eine formatierte Ansicht, die ein Bild, einen Titeltext und einen Beschreibungstext enthält.
* **Heroimage** – eine Bild Rendering-Ansicht.
* **ListItem** – eine vorgefertigte Ansicht mit einem Layout, das den nativen IOS-und Android-Listenelementen ähnelt.

Beispiele finden Sie in der [Referenz zu DataPages](controls.md) -Steuerelementen.

### <a name="under-the-hood"></a>Im Hintergrund

Eine xamarin. Forms-Datenquelle entspricht der `IDataSource` -Schnittstelle.

Die xamarin. Forms-Infrastruktur interagiert mit einer Datenquelle über die folgenden Eigenschaften:

* `Data`– eine schreibgeschützte Liste von Datenelementen, die angezeigt werden können.
* `IsLoading`– ein boolescher Wert, der angibt, ob die Daten geladen und zum Rendern verfügbar sind.
* `[key]`– ein Indexer zum Abrufen von Elementen.

Es gibt zwei Methoden `MaskKey` `UnmaskKey` , die verwendet werden können, um Datenelement Eigenschaften auszublenden (oder anzuzeigen). verhindern, dass Sie gerendert werden.)
Der Schlüssel entspricht der benannten Eigenschaft für das Datenelement Objekt.
