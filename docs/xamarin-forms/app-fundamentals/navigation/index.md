---
title: Xamarin.Forms-Navigation
description: In diesem Leitfaden wird erklärt, wie die Navigation in Xamarin.Forms-Apps durchgeführt wird. Xamarin.Forms stellt abhängig von dem verwendeten Seitentyp eine Reihe unterschiedlicher Seitennavigationen bereit.
ms.prod: xamarin
ms.assetid: BC5D0C6C-D5A9-4B12-A492-ED1F570CEC87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 202f044ebd7dd5b110b94d2aa60eeb7151150607
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994727"
---
# <a name="xamarinforms-navigation"></a>Xamarin.Forms-Navigation

_Xamarin.Forms stellt abhängig von dem verwendeten Seitentyp eine Reihe unterschiedlicher Seitennavigationen bereit._

![](images/page-types.png "Xamarin.Forms-Seitentypen")

## <a name="hierarchical-navigationhierarchicalmd"></a>[Hierarchische Navigation](hierarchical.md)

Die [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)-Klasse stellt eine hierarchische Navigation bereit, bei welcher der Benutzer wie gewünscht in der Vorwärts- und in der Rückwärtsrichtung durch Seiten navigieren kann. Die Klasse implementiert die Navigation als LIFO-Stapel (Last-In-First-out) von [`Page`](xref:Xamarin.Forms.Page)-Objekten.

## <a name="tabbedpagetabbed-pagemd"></a>[TabbedPage](tabbed-page.md)

Die [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)-Klasse von Xamarin.Forms besteht aus einer Liste von Registerkarten sowie einem größeren Detailbereich. Jede Registerkarte lädt Inhalt in den Detailbereich.

## <a name="carouselpagecarousel-pagemd"></a>[CarouselPage](carousel-page.md)

Die [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) von Xamarin.Forms ist eine Seite, die Benutzer hin und her wischen können, um auf Inhaltsseiten wie durch einen Katalog zu navigieren.

## <a name="masterdetailpagemaster-detail-pagemd"></a>[MasterDetailPage](master-detail-page.md)

[`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) ist eine Xamarin.Forms-Seite, die zwei Seiten verwandter Informationen verwaltet. Eine davon ist die Masterseite, die Elemente darstellt, und die andere eine Detailseite, die Informationen zu Elementen auf der Masterseite darstellt.

## <a name="modal-pagesmodalmd"></a>[Modale Seiten](modal.md)

Xamarin.Forms bietet auch Unterstützung für modale Seiten. Eine modale Seite ermutigt Benutzer, eine eigenständige Aufgabe auszuführen. Dabei kann erst dann die Ansicht gewechselt werden, wenn die Aufgabe abgeschlossen oder abgebrochen wurde.

## <a name="displaying-pop-upspop-upsmd"></a>[Anzeigen von Popups](pop-ups.md)

Xamarin.Forms bietet zwei Benutzeroberflächenelemente, die Popupelementen ähneln: eine Warnung und ein Aktionsblatt. Diese Schnittstellenelemente können verwendet werden, um Benutzern einfache Fragen zu stellen und sie durch Aufgaben zu leiten.
