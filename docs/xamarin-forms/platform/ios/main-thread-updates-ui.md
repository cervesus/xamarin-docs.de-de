---
title: Main Thread Steuern von Updates auf iOS
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, die iOS-Plattform-spezifische nutzen, die Steuerung des Layouts und Rendern von Updates auf dem Hauptthread ausgeführt werden können.
ms.prod: xamarin
ms.assetid: 945E711D-9BD2-4BF9-9FB3-CBE0D5B25A49
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: b9f39cd33d660999cfa00f2003edab7af731ca7c
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2019
ms.locfileid: "65925715"
---
# <a name="main-thread-control-updates-on-ios"></a>Main Thread Steuern von Updates auf iOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)

Dieses iOS-Plattform-spezifische ermöglicht die Steuerung des Layouts und Rendern von Updates auf dem Hauptthread statt in einem Hintergrundthread ausgeführt wird, ausgeführt werden. Es sollten nur selten erforderlich sein, aber in einigen Fällen kann verhindern, dass abstürzen. Die verwendeten in XAML durch Festlegen der `Application.HandleControlUpdatesOnMainThread` bindbare Eigenschaft `true`:

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

- [PlatformSpecifics (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
