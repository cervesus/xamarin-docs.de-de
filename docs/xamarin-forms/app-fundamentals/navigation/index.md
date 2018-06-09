---
title: Xamarin.Forms-Navigation
description: Dieses Handbuch erläutert die Navigation in Xamarin.Forms-apps auszuführen. Xamarin.Forms bietet eine Reihe von anderen Seite Navigation Erfahrungen, abhängig von den verwendeten Seitentyp.
ms.prod: xamarin
ms.assetid: BC5D0C6C-D5A9-4B12-A492-ED1F570CEC87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 90aedee42af7ed1788110e832fb3b435d870ee77
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241955"
---
# <a name="xamarinforms-navigation"></a>Xamarin.Forms-Navigation

_Xamarin.Forms bietet eine Reihe von anderen Seite Navigation Erfahrungen, abhängig von den verwendeten Seitentyp._

![](images/page-types.png "Xamarin.Forms Seitentypen")

## <a name="hierarchical-navigationhierarchicalmd"></a>[Hierarchische Navigation](hierarchical.md)

Die [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)-Klasse stellt eine hierarchische Navigation bereit, bei welcher der Benutzer wie gewünscht in der Vorwärts- und in der Rückwärtsrichtung durch Seiten navigieren kann. Die Klasse implementiert die Navigation als LIFO-Stapel (Last-In-First-out) von [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)-Objekten.

## <a name="tabbedpagetabbed-pagemd"></a>[TabbedPage](tabbed-page.md)

Der Xamarin.Forms [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) besteht aus einer Liste von Registerkarten und ein größerer Detailbereich verfügbar, und jede Registerkarte Laden des Inhalts in den Bereich "Details".

## <a name="carouselpagecarousel-pagemd"></a>[CarouselPage](carousel-page.md)

Der Xamarin.Forms [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) wird eine Seite, die Benutzer nebeneinander navigieren Sie können zum Navigieren durch die Seiten des Inhalts, wie einem Katalog.

## <a name="masterdetailpagemaster-detail-pagemd"></a>[MasterDetailPage](master-detail-page.md)

Der Xamarin.Forms [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) eine Seite, die verwaltet zwei Seiten verwandter Informationen – einer Masterseite, in dem Elemente dargestellt, und eine Detailseite, in dem Informationen zu Elementen auf der Seite "master" dargestellt wird.

## <a name="modal-pagesmodalmd"></a>[Modale Seiten](modal.md)

Xamarin.Forms bietet auch Unterstützung für modale Seiten. Eine modale Seite ermutigt Benutzer, eine eigenständige Aufgabe auszuführen. Dabei kann erst dann die Ansicht gewechselt werden, wenn die Aufgabe abgeschlossen oder abgebrochen wurde.

## <a name="displaying-pop-upspop-upsmd"></a>[Anzeigen von Popups](pop-ups.md)

Xamarin.Forms stellt zwei pop-Einrichtung-ähnliche Elemente der Benutzeroberfläche: eine Warnung und einer Aktion-Tabelle. Diese Elemente der Benutzeroberfläche können Benutzer einfache Fragen stellen und als Anleitung für die Benutzer über die Aufgaben verwendet werden.
