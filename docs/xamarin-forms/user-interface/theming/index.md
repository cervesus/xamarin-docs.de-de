---
title: Entwerfen einer Xamarin.Forms-Anwendung
description: Xamarin. Forms-Anwendungen unterstützen Themen, indem Sie für jedes Design ein ResourceDictionary erstellen und dann die Ressourcen mit der DynamicResource-Markup Erweiterung laden.
ms.prod: xamarin
ms.assetId: BF92AEDD-EF23-4D08-A972-B089066E75F9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/22/2020
ms.openlocfilehash: 5988437b40ac875b8b59f9af0f25d4b5c60ded97
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517130"
---
# <a name="theming-a-xamarinforms-application"></a>Design einer xamarin. Forms-Anwendung

## <a name="theme-an-application"></a>[Design einer Anwendung](theming.md)

Design kann in xamarin. Forms-Anwendungen implementiert werden, indem für [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) jedes Design ein erstellt und dann die Ressourcen mit der `DynamicResource` Markup Erweiterung geladen werden.

## <a name="respond-to-system-theme-changes"></a>[Reagieren auf Systemdesign Änderungen](system-theme-changes.md)

Geräte enthalten in der Regel helle und dunkle Designs, die jeweils auf eine Vielzahl von Darstellungs Einstellungen verweisen, die auf Betriebssystemebene festgelegt werden können. Anwendungen sollten diese Systemdesigns berücksichtigen und sofort reagieren, wenn das Systemdesign geändert wird.
