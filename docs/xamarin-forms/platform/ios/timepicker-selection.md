---
title: Elementauswahl für timePicker unter IOS
description: Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie die plattformspezifische IOS-Anwendung nutzen, die steuert, wann die Elementauswahl in einer timePicker auftritt.
ms.prod: xamarin
ms.assetid: 554AC877-8698-4B8C-A676-80DD64305A06
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/15/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 009482c8f1e90aaa2f592ea04d8fd4f0f31324e8
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84137019"
---
# <a name="timepicker-item-selection-on-ios"></a>Elementauswahl für timePicker unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese IOS-plattformspezifischen Steuerelemente, wenn die Elementauswahl in einem Auftritt [`TimePicker`](xref:Xamarin.Forms.TimePicker) , sodass der Benutzer angeben kann, dass die Elementauswahl beim Durchsuchen von Elementen im Steuerelement oder erst nach dem Drücken der Schaltfläche **done** erfolgt. Sie wird in XAML verwendet, indem die `TimePicker.UpdateMode` angefügte-Eigenschaft auf einen Wert der-Enumeration festgelegt wird `UpdateMode` :

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
       <TimePicker Time="14:00:00"
                   ios:TimePicker.UpdateMode="WhenFinished" />
       ...
    </StackLayout>
</ContentPage>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

timePicker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
```

Die- `TimePicker.On<iOS>` Methode gibt an, dass diese plattformspezifische nur unter IOS ausgeführt wird. Die- `TimePicker.SetUpdateMode` Methode im- [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) Namespace wird verwendet, um zu steuern, wann die Elementauswahl stattfindet, wobei die- `UpdateMode` Enumeration zwei mögliche Werte bereitstellt:

- `Immediately`die – Item-Auswahl tritt auf, wenn der Benutzer Elemente in der durchsucht [`TimePicker`](xref:Xamarin.Forms.TimePicker) . Dies ist das Standardverhalten in Xamarin.Forms .
- `WhenFinished`die Auswahl von – Items erfolgt nur, wenn der Benutzer die Schaltfläche **done** in der gedrückt hat [`TimePicker`](xref:Xamarin.Forms.TimePicker) .

Außerdem kann die- `SetUpdateMode` Methode zum Umschalten der Enumerationswerte verwendet werden, indem die- `UpdateMode` Methode aufgerufen wird, die den aktuellen zurückgibt `UpdateMode` :

```csharp
switch (timePicker.On<iOS>().UpdateMode())
{
    case UpdateMode.Immediately:
        timePicker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
        break;
    case UpdateMode.WhenFinished:
        timePicker.On<iOS>().SetUpdateMode(UpdateMode.Immediately);
        break;
}
```

Das Ergebnis ist, dass eine angegebene `UpdateMode` auf den angewendet wird [`TimePicker`](xref:Xamarin.Forms.TimePicker) , der steuert, wann die Elementauswahl erfolgt:

[![Screenshot der timePicker-Aktualisierungs Modi](timepicker-selection-images/timepicker-updatemode.png "TimePicker UpdateMode-plattformspezifisch")](timepicker-selection-images/timepicker-updatemode-large.png#lightbox "TimePicker UpdateMode-plattformspezifisch")

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
