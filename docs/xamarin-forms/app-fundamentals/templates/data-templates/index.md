---
title: Xamarin.Forms-Datenvorlagen
description: Ein DataTemplate-Element wird verwendet, um die Darstellung der Daten für unterstützte Steuerelemente festzulegen, und in der Regel bindet an die Daten, die angezeigt werden.
ms.prod: xamarin
ms.assetid: 838F4BDB-B719-457F-8633-27E9B267A2A0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: 771ae22c3e28a4fce758bbfd6a3bd63bafb75e53
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994970"
---
# <a name="xamarinforms-data-templates"></a>Xamarin.Forms-Datenvorlagen

_Ein DataTemplate-Element wird verwendet, um die Darstellung der Daten für unterstützte Steuerelemente festzulegen, und in der Regel bindet an die Daten, die angezeigt werden._

## <a name="introductionintroductionmd"></a>[Introduction (Einführung)](introduction.md)

Xamarin.Forms-Datenvorlagen bieten die Möglichkeit, die Darstellung von Daten für unterstützte Steuerelemente zu definieren. Dieser Artikel enthält eine Einführung in die Data-Vorlagen, untersuchen, warum sie erforderlich sind.

## <a name="creating-a-datatemplatecreatingmd"></a>[Erstellen ein DataTemplate-Element](creating.md)

Datenvorlagen können Inline erstellt werden, einem [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), oder aus einem benutzerdefinierten Typ oder die entsprechende Xamarin.Forms-Zellentyp. Eine Inlinevorlage sollte verwendet werden, wenn keine Notwendigkeit besteht, die Datenvorlage an anderer Stelle wiederverwenden. Alternativ kann eine Datenvorlage wiederverwendet werden, definieren sie als einen benutzerdefinierten Typ oder als Steuerungsebene, Seiten- oder Anwendungsebene Ressource.

## <a name="creating-a-datatemplateselectorselectormd"></a>[Erstellen eine DataTemplateSelector](selector.md)

Ein [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector) dienen zum Auswählen einer [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) zur Laufzeit anhand des Werts einer datengebundenen Eigenschaft. Dadurch können mehrere `DataTemplate` -Instanzen, die in den gleichen Typ des Objekts, Anpassen die Darstellung bestimmter Objekte angewendet werden. In diesem Artikel wird veranschaulicht, wie das Erstellen und nutzen einen `DataTemplateSelector`.


## <a name="related-links"></a>Verwandte Links

- [Datenvorlagen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
