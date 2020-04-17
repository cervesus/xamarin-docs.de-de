---
title: Xamarin.Forms-Navigation
description: In diesem Leitfaden wird erklärt, wie die Navigation in Xamarin.Forms-Apps durchgeführt wird. Xamarin.Forms stellt abhängig von dem verwendeten Seitentyp eine Reihe unterschiedlicher Seitennavigationen bereit.
ms.prod: xamarin
ms.assetid: BC5D0C6C-D5A9-4B12-A492-ED1F570CEC87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 682e3bd0ac4cdd651203496dd28586db2cef3165
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "66835261"
---
# <a name="xamarinforms-navigation"></a>Xamarin.Forms-Navigation

_Xamarin.Forms stellt abhängig von dem verwendeten Seitentyp eine Reihe unterschiedlicher Seitennavigationen bereit._

![](images/page-types.png "Xamarin.Forms Page Types")

Alternativ dazu verwenden Xamarin.Forms-Shell-Anwendungen eine URI-basierte Navigationsoberfläche, die keine festgelegte Navigationshierarchie erzwingt. Weitere Informationen finden Sie unter [Navigation in der Xamarin.Forms-Shell](~/xamarin-forms/app-fundamentals/shell/navigation.md).

## <a name="hierarchical-navigation"></a>[Hierarchische Navigation](hierarchical.md)

Die [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)-Klasse stellt eine hierarchische Navigation bereit, bei welcher der Benutzer wie gewünscht in der Vorwärts- und in der Rückwärtsrichtung durch Seiten navigieren kann. Die Klasse implementiert die Navigation als LIFO-Stapel (Last-In-First-out) von [`Page`](xref:Xamarin.Forms.Page)-Objekten.

## <a name="tabbedpage"></a>[TabbedPage](tabbed-page.md)

Die [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)-Klasse von Xamarin.Forms besteht aus einer Liste von Registerkarten sowie einem größeren Detailbereich. Jede Registerkarte lädt Inhalt in den Detailbereich.

## <a name="carouselpage"></a>[CarouselPage](carousel-page.md)

Die [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) von Xamarin.Forms ist eine Seite, die Benutzer hin und her wischen können, um auf Inhaltsseiten wie durch einen Katalog zu navigieren.

## <a name="masterdetailpage"></a>[MasterDetailPage](master-detail-page.md)

[`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) ist eine Xamarin.Forms-Seite, die zwei Seiten verwandter Informationen verwaltet. Eine davon ist die Masterseite, die Elemente darstellt, und die andere eine Detailseite, die Informationen zu Elementen auf der Masterseite darstellt.

## <a name="modal-pages"></a>[Modale Seiten](modal.md)

Xamarin.Forms bietet auch Unterstützung für modale Seiten. Eine modale Seite ermutigt Benutzer, eine eigenständige Aufgabe auszuführen. Dabei kann erst dann die Ansicht gewechselt werden, wenn die Aufgabe abgeschlossen oder abgebrochen wurde.
