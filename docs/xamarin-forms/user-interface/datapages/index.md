---
title: Xamarin.FormsDataPages
description: In diesem Artikel Xamarin.Forms werden DataPages vorgestellt, die eine API bereitstellen, mit der eine Datenquelle schnell und einfach an vorgefertigte Sichten gebunden werden kann.
ms.prod: xamarin
ms.assetid: DF16EAEE-DB78-42CA-9C59-51D9D6CB6B95
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5e1ea94e42e98609b3f77f0198e125b94e2b437d
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86928834"
---
# <a name="xamarinforms-datapages"></a>Xamarin.FormsDataPages

![Diese API befindet sich derzeit in der Vorschau Phase.](~/media/shared/preview.png)

> [!IMPORTANT]
> DataPages erfordert einen Design Xamarin.Forms Verweis zum Rendering. Dies umfasst die Installation von [ Xamarin.Forms . Design. basieren](https://www.nuget.org/packages/Xamarin.Forms.Theme.Base/) Sie auf das nuget-Paket in Ihrem Projekt, gefolgt von dem [ Xamarin.Forms . Design. Light](https://www.nuget.org/packages/Xamarin.Forms.Theme.Light/) oder [ Xamarin.Forms . Design. Dark](https://www.nuget.org/packages/Xamarin.Forms.Theme.Dark/) nuget-Pakete.

Xamarin.FormsDataPages wurden bei Weiterentwicklung 2016 angekündigt und stehen als Vorschau für Kunden zur Verfügung, um uns Feedback zu senden.

DataPages bieten eine API, mit der eine Datenquelle schnell und einfach an vorgefertigte Sichten gebunden werden können. Listenelemente und Detailseiten werden die Daten automatisch Rendering und können mithilfe von Designs angepasst werden.

Sehen Sie sich die [Anleitung](get-started.md)für die ersten Schritte an.

[![DataPages-Beispielanwendung](images/demo-sml.png)](images/demo.png#lightbox "DataPages-Beispielanwendung")

## <a name="introduction"></a>Einführung

Mithilfe von Datenquellen und den zugehörigen Datenseiten können Entwickler schnell und einfach eine unterstützte Datenquelle nutzen und diese mithilfe des integrierten UI-Gerüsts, das mit Designs angepasst werden kann, wiedergeben.

DataPages werden zu einer-Anwendung hinzugefügt, indem das hinzugefügt wird Xamarin.Forms ** Xamarin.Forms . **Das nuget-Paket für Seiten.

### <a name="data-sources"></a>Projektmappen-Explorer

Die Vorschau bietet einige vorgefertigte Datenquellen, die für die Verwendung zur Verfügung stehen:

* **Jsondatasource**
* **Azuredatasource** (separates nuget)
* **Azureeasytabledatasource** (separates nuget)

Ein Beispiel für die Verwendung von finden Sie im [Leitfaden zu](get-started.md) den ersten Schritten `JsonDataSource` .

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

Eine Xamarin.Forms Datenquelle entspricht der- `IDataSource` Schnittstelle.

Die Xamarin.Forms Infrastruktur interagiert mit einer Datenquelle über die folgenden Eigenschaften:

* `Data`– eine schreibgeschützte Liste von Datenelementen, die angezeigt werden können.
* `IsLoading`– ein boolescher Wert, der angibt, ob die Daten geladen und zum Rendern verfügbar sind.
* `[key]`– ein Indexer zum Abrufen von Elementen.

Es gibt zwei Methoden `MaskKey` `UnmaskKey` , die verwendet werden können, um Datenelement Eigenschaften auszublenden (oder anzuzeigen). verhindern, dass Sie gerendert werden.)
Der Schlüssel entspricht der benannten Eigenschaft für das Datenelement Objekt.
