---
title: Xamarin.Forms-Datenvorlagen
description: Ein DataTemplate wird verwendet, um die Darstellung von Daten auf unterstützten Steuerelementen anzugeben. In der Regel wird DataTemplate an die anzuzeigenden Daten gebunden.
ms.prod: xamarin
ms.assetid: 838F4BDB-B719-457F-8633-27E9B267A2A0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: 5d130a6644af4e5831263c6de137513c021e0b6a
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70760800"
---
# <a name="xamarinforms-data-templates"></a>Xamarin.Forms-Datenvorlagen

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/templates-datatemplates)

_Ein DataTemplate wird verwendet, um die Darstellung von Daten auf unterstützten Steuerelementen anzugeben. In der Regel wird DataTemplate an die anzuzeigenden Daten gebunden._

## <a name="introductionintroductionmd"></a>[Introduction (Einführung)](introduction.md)

Mit Xamarin.Forms-Datenvorlagen können Sie die Darstellung von Daten in unterstützten Steuerelementen definieren. Dieser Artikel bietet eine Einführung in Datenvorlagen und die Erläuterung, warum diese nötig sind.

## <a name="creating-a-datatemplatecreatingmd"></a>[Erstellen einer DataTemplate-Klasse](creating.md)

Datenvorlagen können inline erstellt werden, z.B. in einem [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)-Element oder über einen benutzerdefinierten Typ oder entsprechenden Xamarin.Forms-Zellentyp. Eine Inlinevorlage sollte verwendet werden, wenn keine Notwendigkeit besteht, die Datenvorlage woanders erneut zu verwenden. Alternativ kann eine Datenvorlage erneut verwendet werden, indem sie als benutzerdefinierter Typ oder als Ressource auf Steuerelementebene, Seitenebene oder Anwendungsebene definiert wird.

## <a name="creating-a-datatemplateselectorselectormd"></a>[Erstellen einer DataTemplateSelector-Klasse](selector.md)

Eine [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) kann verwendet werden, um eine [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) zur Laufzeit basierend auf dem Wert der datengebundenen Eigenschaft auszuwählen. Dadurch können mehrere `DataTemplate`-Instanzen auf den gleichen Objekttyp angewendet werden, um die Darstellung bestimmter Objekte anzupassen. Dieser Artikel demonstriert, wie eine `DataTemplateSelector`-Klasse erstellt und genutzt wird.

## <a name="related-links"></a>Verwandte Links

- [Data Templates (Datenvorlagen (Beispiel))](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/templates-datatemplates)
