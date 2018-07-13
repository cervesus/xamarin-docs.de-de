---
title: Xamarin.Forms-Navigation
description: Dieses Handbuch erklärt, wie Navigation in Xamarin.Forms-apps ausgeführt wird. Xamarin.Forms bietet es sich um eine Reihe von der anderen seitennavigationen, abhängig von den Seitentyp, der verwendet wird.
ms.prod: xamarin
ms.assetid: BC5D0C6C-D5A9-4B12-A492-ED1F570CEC87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 202f044ebd7dd5b110b94d2aa60eeb7151150607
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994727"
---
# <a name="xamarinforms-navigation"></a>Xamarin.Forms-Navigation

_Xamarin.Forms bietet es sich um eine Reihe von der anderen seitennavigationen, abhängig von den Seitentyp, der verwendet wird._

![](images/page-types.png "Typen von Xamarin.Forms-Startseite")

## <a name="hierarchical-navigationhierarchicalmd"></a>[Hierarchische Navigation](hierarchical.md)

Die [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)-Klasse stellt eine hierarchische Navigation bereit, bei welcher der Benutzer wie gewünscht in der Vorwärts- und in der Rückwärtsrichtung durch Seiten navigieren kann. Die Klasse implementiert die Navigation als LIFO-Stapel (Last-In-First-out) von [`Page`](xref:Xamarin.Forms.Page)-Objekten.

## <a name="tabbedpagetabbed-pagemd"></a>[TabbedPage](tabbed-page.md)

Die Xamarin.Forms [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) besteht aus einer Liste der Registerkarten und einen größeren Detailbereich, und jede Registerkarte Inhalt wird in den Bereich "Details" geladen.

## <a name="carouselpagecarousel-pagemd"></a>[CarouselPage](carousel-page.md)

Die Xamarin.Forms [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) ist eine Seite, die Benutzer von Seite zu Seite navigieren können, für die Navigation durch Seiten, Inhalte, z. B. einen Katalog.

## <a name="masterdetailpagemaster-detail-pagemd"></a>[MasterDetailPage](master-detail-page.md)

Die Xamarin.Forms [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) ist eine Seite, die zwei Seiten verwandter Informationen – eine Masterseite, die Elemente darstellt, und eine Detailseite, die Details zu Elementen auf der Masterseite präsentiert verwaltet.

## <a name="modal-pagesmodalmd"></a>[Modale Seiten](modal.md)

Xamarin.Forms bietet auch Unterstützung für modale Seiten. Eine modale Seite ermutigt Benutzer, eine eigenständige Aufgabe auszuführen. Dabei kann erst dann die Ansicht gewechselt werden, wenn die Aufgabe abgeschlossen oder abgebrochen wurde.

## <a name="displaying-pop-upspop-upsmd"></a>[Anzeigen von Popups](pop-ups.md)

Xamarin.Forms stellt zwei pop-Registrierung-ähnliche Elemente der Benutzeroberfläche: eine Warnung und ein aktionsblatt. Diese Elemente der Benutzeroberfläche können Benutzer einfache Fragen stellen und Benutzerhandbuch durch Aufgaben verwendet werden.
