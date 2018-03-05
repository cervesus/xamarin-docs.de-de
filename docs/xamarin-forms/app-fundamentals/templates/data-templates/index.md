---
title: Datenvorlagen
description: "Eine DataTemplate wird verwendet, um die Darstellung der Daten auf unterstützte Steuerelemente angeben und in der Regel bindet an die Daten angezeigt werden."
ms.topic: article
ms.prod: xamarin
ms.assetid: 838F4BDB-B719-457F-8633-27E9B267A2A0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: 99a00c98471ae85af2a8cba2e1e52444370a9332
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="data-templates"></a>Datenvorlagen

_Eine DataTemplate wird verwendet, um die Darstellung der Daten auf unterstützte Steuerelemente angeben und in der Regel bindet an die Daten angezeigt werden._

## <a name="introductionintroductionmd"></a>[Introduction (Einführung)](introduction.md)

Xamarin.Forms Datenvorlagen bieten die Möglichkeit zum Definieren der Darstellung von Daten auf unterstützte Steuerelemente. Dieser Artikel bietet eine Einführung in Data-Vorlagen, untersuchen, warum diese erforderlich sind.

## <a name="creating-a-datatemplatecreatingmd"></a>[Erstellen einer DataTemplate](creating.md)

Datenvorlagen können Inline erstellt werden, einem [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), oder aus einem benutzerdefinierten Typ oder die entsprechenden Xamarin.Forms Zellentyp. Eine Inlinevorlage sollte verwendet werden, wenn keine Notwendigkeit besteht, die Datenvorlage an anderer Stelle wiederverwenden. Alternativ kann eine Datenvorlage definieren ihn als einen benutzerdefinierten Typ oder als Steuerungsebene,-Ressource auf Seitenebene oder auf Anwendungsebene wiederverwendet werden.

## <a name="creating-a-datatemplateselectorselectormd"></a>[Erstellen eine DataTemplateSelector](selector.md)

Ein [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) dienen zum Auswählen einer [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) zur Laufzeit anhand des Werts einer datengebundenen Eigenschaft. Dadurch können mehrere `DataTemplate` Instanzen auf den gleichen Typ des Objekts, zum Anpassen der Darstellung bestimmter Objekte angewendet werden soll. In diesem Artikel wird veranschaulicht, wie erstellen und nutzen einen `DataTemplateSelector`.


## <a name="related-links"></a>Verwandte Links

- [Datenvorlagen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
