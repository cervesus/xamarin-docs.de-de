---
title: Themen zu einer Xamarin.Forms Anwendung
description: Xamarin.FormsAnwendungen unterstützen das Design, indem Sie für jedes Design ein ResourceDictionary erstellen und anschließend die Ressourcen mit der DynamicResource-Markup Erweiterung laden.
ms.prod: xamarin
ms.assetId: BF92AEDD-EF23-4D08-A972-B089066E75F9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/22/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 80660ae7d3af0fe5948a5ae4ffdb35d2f9c2a40f
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136135"
---
# <a name="theming-a-xamarinforms-application"></a>Themen zu einer Xamarin.Forms Anwendung

## <a name="theme-an-application"></a>[Design einer Anwendung](theming.md)

Design kann in Anwendungen implementiert werden Xamarin.Forms , indem für jedes Design ein erstellt [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) und dann die Ressourcen mit der `DynamicResource` Markup Erweiterung geladen werden.

## <a name="respond-to-system-theme-changes"></a>[Reagieren auf Systemdesignänderungen](system-theme-changes.md)

Geräte enthalten in der Regel helle und dunkle Designs, die jeweils auf eine Vielzahl von Darstellungs Einstellungen verweisen, die auf Betriebssystemebene festgelegt werden können. Anwendungen sollten diese Systemdesigns berücksichtigen und sofort reagieren, wenn das Systemdesign geändert wird.
