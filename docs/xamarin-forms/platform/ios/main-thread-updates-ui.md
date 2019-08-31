---
title: Haupt-Thread Steuerungs Updates unter IOS
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie Sie die plattformspezifische IOS-Anwendung verwenden, die das Ausführen von Steuerelement Layout und Renderingupdates im Haupt Thread ermöglicht.
ms.prod: xamarin
ms.assetid: 945E711D-9BD2-4BF9-9FB3-CBE0D5B25A49
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: d55ef4a97d5d4f320bf152ba05c86aff82eb2f1e
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2019
ms.locfileid: "70200119"
---
# <a name="main-thread-control-updates-on-ios"></a>Haupt-Thread Steuerungs Updates unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese IOS-plattformspezifische ermöglicht die Ausführung von Steuerelement Layout und Renderingupdates im Haupt Thread, anstatt in einem Hintergrund Thread ausgeführt zu werden. Es sollten nur selten erforderlich sein, aber in einigen Fällen kann verhindern, dass abstürzen. Die verwendeten in XAML durch Festlegen der `Application.HandleControlUpdatesOnMainThread` bindbare Eigenschaft `true`:

```xaml
<Application ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Application.HandleControlUpdatesOnMainThread="true">
    ...
</Application>
```

Alternativ können sie aus C# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Xamarin.Forms.Application.Current.On<iOS>().SetHandleControlUpdatesOnMainThread(true);
```

Die `Application.On<iOS>` Methode gibt an, dass diese plattformspezifischen nur unter iOS ausgeführt wird. Die `Application.SetHandleControlUpdatesOnMainThread` Methode in der [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) -Namespace wird verwendet um zu steuern, ob das Steuerelementlayout und Rendern von Updates ausgeführt werden, auf dem Hauptthread statt in einem Hintergrundthread ausgeführt wird. Darüber hinaus die `Application.GetHandleControlUpdatesOnMainThread` Methode kann verwendet werden, um zurück, ob das Steuerelementlayout und Rendern von Updates für den Hauptthread ausgeführt werden.

## <a name="related-links"></a>Verwandte Links

- [PlatformSpecifics (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
