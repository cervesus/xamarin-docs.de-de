---
title: Experimentelle xamarin. Forms-Flags
description: Durch die experimentellen Flags von xamarin. Forms kann das Engineering-Team neue Features schneller an die Benutzer senden, während Sie weiterhin Funktions-APIs ändern können, bevor Sie zu einer stabilen Version wechseln.
ms.prod: xamarin
ms.assetid: AF4BDD27-89F6-48AE-A8CD-D7E4DDA2CCA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/20/2020
ms.openlocfilehash: cebb996da992058616f9cf96ef3212c9ce27022a
ms.sourcegitcommit: 6c60914b380ff679bbffd7790edd4d5e18005d0a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2020
ms.locfileid: "80112600"
---
# <a name="xamarinforms-experimental-flags"></a>Experimentelle xamarin. Forms-Flags

Wenn eine neue xamarin. Forms-Funktion implementiert ist, wird Sie manchmal hinter einem experimentellen Flag abgelegt. Dies ermöglicht es dem Engineering-Team, neue Features schneller bereitzustellen, während Sie weiterhin Funktions-APIs ändern können, bevor Sie zu einer stabilen Version wechseln. Das experimentelle Flag wird dann entfernt, sobald das Feature in eine stabile Version wechselt.

Xamarin. Forms enthält die folgenden experimentellen Flags:

- `CarouselView_Experimental`
- `IndicatorView_Experimental`
- `Markup_Experimental`
- `MediaElement_Experimental`
- `Shell_UWP_Experimental`
- `StateTriggers_Experimental`
- `SwipeView_Experimental`

Wenn Sie Funktionen verwenden, die sich hinter einem experimentellen Flag befinden, müssen Sie das Flag bzw. die Flags in der Anwendung aktivieren. Es gibt zwei Ansätze zum Aktivieren von experimentellen Flags:

- Aktivieren Sie das experimentelle Flag (Flags) in ihren Platt Form Projekten.
- Aktivieren Sie das experimentelle Flag oder Flags in der `App`-Klasse.

> [!WARNING]
> Wenn Sie Funktionen verwenden, die sich hinter einem experimentellen Flag befinden, ohne das-Flag zu aktivieren, löst die Anwendung eine Ausnahme aus, die angibt, welches Flag aktiviert werden muss.

## <a name="enable-flags-in-platform-projects"></a>Aktivieren von Flags in Platt Form Projekten

Die `Xamarin.Forms.Forms.SetFlags`-Methode kann verwendet werden, um ein experimentelles Flag in ihren Platt Form Projekten zu aktivieren:

```csharp
Xamarin.Forms.Forms.SetFlags("CarouselView_Experimental");
```

Die `SetFlags`-Methode sollte in ihrer `AppDelegate`-Klasse unter IOS, in ihrer `MainActivity`-Klasse unter Android und in ihrer `App`-Klasse auf UWP aufgerufen werden.

> [!IMPORTANT]
> Das Aktivieren eines experimentellen Flags in ihren Platt Form Projekten muss eintreten, bevor die `Forms.Init`-Methode aufgerufen wird.

Die `Xamarin.Forms.Forms.SetFlags`-Methode akzeptiert ein `string` Array-Argument, das es ermöglicht, mehrere experimentelle Flags in einem einzelnen Methoden aufzurufen zu aktivieren:

```csharp
Xamarin.Forms.Forms.SetFlags(new string[] { "CarouselView_Experimental", "IndicatorView_Experimental", "SwipeView_Experimental" });
```

> [!WARNING]
> Rufen Sie die `SetFlags`-Methode niemals mehrmals auf, da nachfolgende Aufrufe das Ergebnis vorheriger Aufrufe überschreiben.

## <a name="enable-flags-in-your-app-class"></a>Aktivieren von Flags in ihrer App-Klasse

Die `Device.SetFlags`-Methode kann verwendet werden, um ein experimentelles Flag in der `App`-Klasse im freigegebenen Code Projekt zu aktivieren:

```csharp
Device.SetFlags(new string[]{ "MediaElement_Experimental" });
```

Die `Device.SetFlags`-Methode akzeptiert ein `IReadOnlyList<string>` Argument, das es ermöglicht, mehrere experimentelle Flags in einem einzelnen Methoden aufzurufen zu aktivieren:

```csharp
Device.SetFlags(new string[]{ "CarouselView_Experimental", "MediaElement_Experimental", "SwipeView_Experimental" });
```

> [!WARNING]
> Rufen Sie die `SetFlags`-Methode niemals mehrmals auf, da nachfolgende Aufrufe das Ergebnis vorheriger Aufrufe überschreiben.
