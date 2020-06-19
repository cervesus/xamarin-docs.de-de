---
title: 'title: "Xamarin.Forms Datenvorlagen"description: "Ein DataTemplate wird verwendet, um die Darstellung von Daten auf unterstützten Steuerelementen anzugeben. In der Regel wird DataTemplate an die anzuzeigenden Daten gebunden."'
description: 'ms.prod: xamarin ms.assetid: 838F4BDB-B719-457F-8633-27E9B267A2A0 ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date: 09/11/2017 no-loc: [Xamarin.Forms, Xamarin.Essentials]'
ms.prod: xamarin
ms.assetid: 838F4BDB-B719-457F-8633-27E9B267A2A0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e0961fad18ccd961a5b84b2a5535bca70781dd8d
ms.sourcegitcommit: ea9269b5d9e3d68b61bb428560a10034117ee457
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2020
ms.locfileid: "84136122"
---
# <a name="xamarinforms-data-templates"></a>Xamarin.Forms-Datenvorlagen

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/templates-datatemplates)

_Ein DataTemplate wird verwendet, um die Darstellung von Daten auf unterstützten Steuerelementen anzugeben. In der Regel wird DataTemplate an die anzuzeigenden Daten gebunden._

## <a name="introduction"></a>[Introduction (Einführung)](introduction.md)

Xamarin.Forms-Datenvorlagen bieten die Möglichkeit, die Darstellung von Daten für unterstützte Steuerelemente zu definieren. In diesem Artikel werden Datenvorlagen grundlegend vorgestellt, und es wird erläutert, warum sie nötig sind.

## <a name="creating-a-datatemplate"></a>[Erstellen einer DataTemplate-Klasse](creating.md)

Datenvorlagen können inline erstellt werden, z. B. in einem [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)-Element oder über einen benutzerdefinierten Typ oder entsprechenden Xamarin.Forms-Zellentyp. Eine Inlinevorlage sollte verwendet werden, wenn keine Notwendigkeit besteht, die Datenvorlage woanders erneut zu verwenden. Alternativ kann eine Datenvorlage erneut verwendet werden, indem sie als benutzerdefinierter Typ oder als Ressource auf Steuerelementebene, Seitenebene oder Anwendungsebene definiert wird.

## <a name="creating-a-datatemplateselector"></a>[Erstellen einer DataTemplateSelector-Klasse](selector.md)

Eine [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) kann verwendet werden, um eine [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) zur Laufzeit basierend auf dem Wert der datengebundenen Eigenschaft auszuwählen. Dadurch können mehrere `DataTemplate`-Instanzen auf den gleichen Objekttyp angewendet werden, um die Darstellung bestimmter Objekte anzupassen. Dieser Artikel demonstriert, wie eine `DataTemplateSelector`-Klasse erstellt und genutzt wird.

## <a name="related-links"></a>Verwandte Links

- [Data Templates (Datenvorlagen (Beispiel))](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/templates-datatemplates)
