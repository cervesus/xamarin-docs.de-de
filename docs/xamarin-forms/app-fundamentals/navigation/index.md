---
title: Xamarin.Forms-Navigation
description: In diesem Leitfaden wird beschrieben, wie die Navigation in Xamarin.Forms-Apps durchgeführt wird. Xamarin.Forms stellt abhängig vom verwendeten Seitentyp eine Reihe unterschiedlicher Seitennavigationen bereit.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8c907cd8a4a1d14b936dee309610bffc67ef363f
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137838"
---
# <a name="xamarinforms-navigation"></a>Xamarin.Forms-Navigation

_Xamarin.Forms stellt abhängig von dem verwendeten Seitentyp eine Reihe unterschiedlicher Seitennavigationen bereit._

![](images/page-types.png "Xamarin.Forms Page Types")

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
