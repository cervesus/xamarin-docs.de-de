---
title: Gleichzeitige Pan Gestenerkennung unter iOS
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel erläutert das iOS-Plattform-spezifische nutzen, die gleichzeitig Pan gestenerkennung in einer Anwendung verwendet werden können.
ms.prod: xamarin
ms.assetid: 883D89DA-F8FF-4B97-9C3F-2DD05C96A495
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 4587bb89ddfe43873e666a07514075f1a952e985
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2019
ms.locfileid: "65926779"
---
# <a name="simultaneous-pan-gesture-recognition-on-ios"></a>Gleichzeitige Pan Gestenerkennung unter iOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)

Wenn eine [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer) angefügt ist, auf eine Ansicht innerhalb einer Bildlaufansicht, alle der Pfanne Gesten erfasst werden, die `PanGestureRecognizer` und werden nicht an, dass der Bildlauf anzeigen. Aus diesem Grund wird ein Bildlauf Ansicht nicht mehr ausgeführt.

Dieses iOS-Plattform-spezifische ermöglicht eine `PanGestureRecognizer` in einer fortlaufenden zu erfassen und freigeben die Bewegung Schwenken mit der Bildlauf angezeigt. Es ist in XAML verwendet, durch Festlegen der [ `Application.PanGestureRecognizerShouldRecognizeSimultaneously` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Application.PanGestureRecognizerShouldRecognizeSimultaneouslyProperty) angefügte Eigenschaft zu `true`:

```xaml
<Application ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Application.PanGestureRecognizerShouldRecognizeSimultaneously="true">
    ...
</Application>
```

Alternativ können sie aus C# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Xamarin.Forms.Application.Current.On<iOS>().SetPanGestureRecognizerShouldRecognizeSimultaneously(true);
```

Die `Application.On<iOS>` Methode gibt an, dass diese plattformspezifischen nur unter iOS ausgeführt wird. Die [ `Application.SetPanGestureRecognizerShouldRecognizeSimultaneously` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Application.SetPanGestureRecognizerShouldRecognizeSimultaneously(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Application},System.Boolean)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) -Namespace wird verwendet, um Kontrolle, ob eine stiftbewegungs-Erkennung Schwenken in einer fortlaufenden wird erfassen die Pan-Geste oder erfassen und freigeben die Pfanne die Geste mit der Bildlauf angezeigt. Darüber hinaus die [ `Application.GetPanGestureRecognizerShouldRecognizeSimultaneously` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Application.GetPanGestureRecognizerShouldRecognizeSimultaneously(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Application})) Methode kann verwendet werden, um zurück, ob die Geste Schwenken mit der Bildlauf Ansicht gemeinsam verwendet wird, die enthält die [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer).

Aus diesem Grund mit diesem plattformspezifische aktiviert, wenn eine [ `ListView` ](xref:Xamarin.Forms.ListView) enthält eine [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer)sowohl die `ListView` und die `PanGestureRecognizer` erhalten Sie die Pan-Bewegung und Verarbeiten Sie sie. Allerdings mit dieser plattformspezifischen deaktiviert, wenn eine `ListView` enthält eine `PanGestureRecognizer`, die `PanGestureRecognizer` Schwenken Bewegung zu erfassen und verarbeiten, wird und die `ListView` Schwenken Bewegung erhalten keine.

## <a name="related-links"></a>Verwandte Links

- [PlatformSpecifics (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
