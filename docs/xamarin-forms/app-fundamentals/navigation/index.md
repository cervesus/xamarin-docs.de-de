---
title: Xamarin.Forms-Navigation
description: In diesem Leitfaden wird beschrieben, wie die Navigation in Xamarin.Forms-Apps durchgeführt wird. Xamarin.Forms stellt abhängig vom verwendeten Seitentyp eine Reihe unterschiedlicher Seitennavigationen bereit.
ms.prod: xamarin
ms.assetid: BC5D0C6C-D5A9-4B12-A492-ED1F570CEC87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 65aa0f060e4d48834017a334d69b2f21645825f3
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937188"
---
# <a name="xamarinforms-navigation"></a>Xamarin.Forms-Navigation

_Xamarin.Forms stellt abhängig vom verwendeten Seitentyp eine Reihe unterschiedlicher Seitennavigationsfunktionen bereit._

![Xamarin.Forms-Seitentypen](images/page-types.png)

Alternativ dazu verwenden Xamarin.Forms-Shell-Anwendungen eine URI-basierte Navigationsoberfläche, die keine festgelegte Navigationshierarchie erzwingt. Weitere Informationen finden Sie unter [Navigation in der Xamarin.Forms-Shell](~/xamarin-forms/app-fundamentals/shell/navigation.md).

## <a name="hierarchical-navigation"></a>[Hierarchische Navigation](hierarchical.md)

Die [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)-Klasse stellt eine hierarchische Navigation bereit, bei welcher der Benutzer wie gewünscht in der Vorwärts- und in der Rückwärtsrichtung durch Seiten navigieren kann. Die Klasse implementiert die Navigation als LIFO-Stapel (Last-In-First-out) von [`Page`](xref:Xamarin.Forms.Page)-Objekten.

## <a name="tabbedpage"></a>[TabbedPage](tabbed-page.md)

Die [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)-Klasse von Xamarin.Forms besteht aus einer Liste mit Registerkarten sowie einem größeren Detailbereich. Jede Registerkarte lädt Inhalt in den Detailbereich.

## <a name="carouselpage"></a>[CarouselPage](carousel-page.md)

[`CarouselPage`](xref:Xamarin.Forms.CarouselPage) ist eine Xamarin.Forms-Seite, die Benutzer hin und her wischen können, um durch Inhaltsseiten wie durch einen Katalog zu navigieren.

## <a name="masterdetailpage"></a>[MasterDetailPage](master-detail-page.md)

[`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) ist eine Xamarin.Forms-Seite, die zwei verwandte Seiten mit Informationen verwaltet. Eine davon ist die Masterseite, die Elemente darstellt, und die andere ist eine Detailseite, die Informationen zu Elementen auf der Masterseite darstellt.

## <a name="modal-pages"></a>[Modale Seiten](modal.md)

Xamarin.Forms verfügt auch über Unterstützung für modale Seiten. Eine modale Seite ermutigt Benutzer, eine eigenständige Aufgabe auszuführen. Dabei kann erst dann die Ansicht gewechselt werden, wenn die Aufgabe abgeschlossen oder abgebrochen wurde.
