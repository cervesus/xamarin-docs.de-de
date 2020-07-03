---
title: Xamarin.Formsexperimentelle Flags
description: Xamarin.Formsdurch experimentelle Flags kann das Engineering-Team neue Features schneller an die Benutzer senden, während Sie weiterhin Funktions-APIs ändern können, bevor Sie zu einer stabilen Version wechseln.
ms.prod: xamarin
ms.assetid: AF4BDD27-89F6-48AE-A8CD-D7E4DDA2CCA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/15/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: bd35722c76cce245701b7a548514d402cd978d38
ms.sourcegitcommit: a3f13a216fab4fc20a9adf343895b9d6a54634a5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85853074"
---
# <a name="xamarinforms-experimental-flags"></a>Xamarin.Formsexperimentelle Flags

Wenn eine neue Xamarin.Forms Funktion implementiert ist, wird Sie manchmal hinter einem experimentellen Flag abgelegt. Dies ermöglicht es dem Engineering-Team, neue Features schneller bereitzustellen, während Sie weiterhin Funktions-APIs ändern können, bevor Sie zu einer stabilen Version wechseln. Das experimentelle Flag wird dann entfernt, sobald das Feature in eine stabile Version wechselt.

Xamarin.Formsenthält die folgenden experimentellen Flags:

- `AppTheme_Experimental`
- `CarouselView_Experimental`
- `Expander_Experimental`
- `Markup_Experimental`
- `MediaElement_Experimental`
- `RadioButton_Experimental`
- `Shapes_Experimental`
- `Shell_UWP_Experimental`
- `SwipeView_Experimental`

Wenn Sie Funktionen verwenden, die sich hinter einem experimentellen Flag befinden, müssen Sie das Flag bzw. die Flags in der Anwendung aktivieren. Es gibt zwei Ansätze zum Aktivieren von experimentellen Flags:

- Aktivieren Sie das experimentelle Flag (Flags) in ihren Platt Form Projekten.
- Aktivieren Sie das experimentelle Flag oder Flags in der `App` Klasse.

> [!WARNING]
> Wenn Sie Funktionen verwenden, die sich hinter einem experimentellen Flag befinden, ohne das-Flag zu aktivieren, löst die Anwendung eine Ausnahme aus, die angibt, welches Flag aktiviert werden muss.

## <a name="enable-flags-in-platform-projects"></a>Aktivieren von Flags in Platt Form Projekten

Die- `Xamarin.Forms.Forms.SetFlags` Methode kann verwendet werden, um ein experimentelles Flag in ihren Platt Form Projekten zu aktivieren:

```csharp
Xamarin.Forms.Forms.SetFlags("CarouselView_Experimental");
```

Die `SetFlags` -Methode sollte in Ihrer `AppDelegate` Klasse unter IOS, in Ihrer `MainActivity` Klasse unter Android und in Ihrer- `App` Klasse auf UWP aufgerufen werden.

> [!IMPORTANT]
> Das Aktivieren eines experimentellen Flags in ihren Platt Form Projekten muss eintreten, bevor die- `Forms.Init` Methode aufgerufen wird.

Die- `Xamarin.Forms.Forms.SetFlags` Methode akzeptiert ein `string` Array Argument, das es ermöglicht, mehrere experimentelle Flags in einem einzelnen Methoden aufzurufen zu aktivieren:

```csharp
Xamarin.Forms.Forms.SetFlags(new string[] { "CarouselView_Experimental", "MediaElement_Experimental", "SwipeView_Experimental" });
```

> [!WARNING]
> Rufen Sie die Methode niemals mehrmals auf `SetFlags` , da nachfolgende Aufrufe das Ergebnis vorheriger Aufrufe überschreiben.

## <a name="enable-flags-in-your-app-class"></a>Aktivieren von Flags in ihrer App-Klasse

Die- `Device.SetFlags` Methode kann verwendet werden, um ein experimentelles Flag in der- `App` Klasse in Ihrem freigegebenen Code Projekt zu aktivieren:

```csharp
Device.SetFlags(new string[]{ "MediaElement_Experimental" });
```

Die- `Device.SetFlags` Methode akzeptiert ein `IReadOnlyList<string>` Argument, das es ermöglicht, mehrere experimentelle Flags in einem einzelnen Methoden aufzurufen zu aktivieren:

```csharp
Device.SetFlags(new string[]{ "CarouselView_Experimental", "MediaElement_Experimental", "SwipeView_Experimental" });
```

> [!WARNING]
> Rufen Sie die Methode niemals mehrmals auf `SetFlags` , da nachfolgende Aufrufe das Ergebnis vorheriger Aufrufe überschreiben.
