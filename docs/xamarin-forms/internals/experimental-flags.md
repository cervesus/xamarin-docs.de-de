---
title: Experimentelle xamarin. Forms-Flags
description: Durch die experimentellen Flags von xamarin. Forms kann das Engineering-Team neue Features schneller an die Benutzer senden, während Sie weiterhin Funktions-APIs ändern können, bevor Sie zu einer stabilen Version wechseln.
ms.prod: xamarin
ms.assetid: AF4BDD27-89F6-48AE-A8CD-D7E4DDA2CCA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/14/2020
ms.openlocfilehash: cca377a7a88599bc34fd66695ad303162e6be200
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82516552"
---
# <a name="xamarinforms-experimental-flags"></a>Experimentelle xamarin. Forms-Flags

Wenn eine neue xamarin. Forms-Funktion implementiert ist, wird Sie manchmal hinter einem experimentellen Flag abgelegt. Dies ermöglicht es dem Engineering-Team, neue Features schneller bereitzustellen, während Sie weiterhin Funktions-APIs ändern können, bevor Sie zu einer stabilen Version wechseln. Das experimentelle Flag wird dann entfernt, sobald das Feature in eine stabile Version wechselt.

Xamarin. Forms enthält die folgenden experimentellen Flags:

- `AppTheme_Experimental`
- `CarouselView_Experimental`
- `Expander_Experimental`
- `IndicatorView_Experimental`
- `Markup_Experimental`
- `MediaElement_Experimental`
- `RadioButton_Experimental`
- `Shell_UWP_Experimental`
- `StateTriggers_Experimental`
- `SwipeView_Experimental`

Wenn Sie Funktionen verwenden, die sich hinter einem experimentellen Flag befinden, müssen Sie das Flag bzw. die Flags in der Anwendung aktivieren. Es gibt zwei Ansätze zum Aktivieren von experimentellen Flags:

- Aktivieren Sie das experimentelle Flag (Flags) in ihren Platt Form Projekten.
- Aktivieren Sie `App` das experimentelle Flag oder Flags in der Klasse.

> [!WARNING]
> Wenn Sie Funktionen verwenden, die sich hinter einem experimentellen Flag befinden, ohne das-Flag zu aktivieren, löst die Anwendung eine Ausnahme aus, die angibt, welches Flag aktiviert werden muss.

## <a name="enable-flags-in-platform-projects"></a>Aktivieren von Flags in Platt Form Projekten

Die `Xamarin.Forms.Forms.SetFlags` -Methode kann verwendet werden, um ein experimentelles Flag in ihren Platt Form Projekten zu aktivieren:

```csharp
Xamarin.Forms.Forms.SetFlags("CarouselView_Experimental");
```

Die `SetFlags` -Methode sollte in Ihrer `AppDelegate` Klasse unter IOS, in Ihrer `MainActivity` Klasse unter Android und in Ihrer `App` -Klasse auf UWP aufgerufen werden.

> [!IMPORTANT]
> Das Aktivieren eines experimentellen Flags in ihren Platt Form Projekten muss eintreten `Forms.Init` , bevor die-Methode aufgerufen wird.

Die `Xamarin.Forms.Forms.SetFlags` -Methode akzeptiert `string` ein Array Argument, das es ermöglicht, mehrere experimentelle Flags in einem einzelnen Methoden aufzurufen zu aktivieren:

```csharp
Xamarin.Forms.Forms.SetFlags(new string[] { "CarouselView_Experimental", "IndicatorView_Experimental", "SwipeView_Experimental" });
```

> [!WARNING]
> Rufen Sie die `SetFlags` Methode niemals mehrmals auf, da nachfolgende Aufrufe das Ergebnis vorheriger Aufrufe überschreiben.

## <a name="enable-flags-in-your-app-class"></a>Aktivieren von Flags in ihrer App-Klasse

Die `Device.SetFlags` -Methode kann verwendet werden, um ein experimentelles Flag `App` in der-Klasse in Ihrem freigegebenen Code Projekt zu aktivieren:

```csharp
Device.SetFlags(new string[]{ "MediaElement_Experimental" });
```

Die `Device.SetFlags` -Methode akzeptiert `IReadOnlyList<string>` ein Argument, das es ermöglicht, mehrere experimentelle Flags in einem einzelnen Methoden aufzurufen zu aktivieren:

```csharp
Device.SetFlags(new string[]{ "CarouselView_Experimental", "MediaElement_Experimental", "SwipeView_Experimental" });
```

> [!WARNING]
> Rufen Sie die `SetFlags` Methode niemals mehrmals auf, da nachfolgende Aufrufe das Ergebnis vorheriger Aufrufe überschreiben.
