---
title: Elementauswahl für timePicker unter IOS
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie Sie die plattformspezifische IOS-Anwendung nutzen, die steuert, wann die Elementauswahl in einer timePicker auftritt.
ms.prod: xamarin
ms.assetid: 554AC877-8698-4B8C-A676-80DD64305A06
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/15/2020
ms.openlocfilehash: 818f368da8ebb375fbacd97d3d48185ba60470d4
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2020
ms.locfileid: "77646677"
---
# <a name="timepicker-item-selection-on-ios"></a>Elementauswahl für timePicker unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese IOS-plattformspezifischen Steuerelemente, wenn die Elementauswahl in einer [`TimePicker`](xref:Xamarin.Forms.TimePicker)erfolgt, sodass der Benutzer angeben kann, dass die Elementauswahl beim Durchsuchen von Elementen im Steuerelement oder erst nach dem Drücken der Schaltfläche **done** erfolgt. Sie wird in XAML verwendet, indem die `TimePicker.UpdateMode` angefügte-Eigenschaft auf einen Wert der `UpdateMode`-Enumeration festgelegt wird:

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

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

timePicker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
```

Die `TimePicker.On<iOS>`-Methode gibt an, dass diese plattformspezifische nur unter IOS ausgeführt wird. Die `TimePicker.SetUpdateMode`-Methode im [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) -Namespace wird verwendet, um zu steuern, wann die Elementauswahl erfolgt, wobei die `UpdateMode`-Enumeration zwei mögliche Werte bereitstellt:

- `Immediately` – Elementauswahl tritt auf, wenn der Benutzer Elemente im [`TimePicker`](xref:Xamarin.Forms.TimePicker)durchsucht. Dies ist das Standardverhalten in Xamarin.Forms.
- `WhenFinished` – Elementauswahl findet nur statt, wenn der Benutzer auf die Schaltfläche **done** in der [`TimePicker`](xref:Xamarin.Forms.TimePicker)geklickt hat.

Außerdem kann die `SetUpdateMode`-Methode verwendet werden, um die Enumerationswerte durch Aufrufen der `UpdateMode`-Methode, die die aktuelle `UpdateMode`zurückgibt, zum Umschalten zu wechseln:

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

Das Ergebnis ist, dass ein angegebener `UpdateMode` auf den [`TimePicker`](xref:Xamarin.Forms.TimePicker)angewendet wird, der steuert, wann die Elementauswahl erfolgt:

[![Screenshot der timePicker-Aktualisierungs Modi](timepicker-selection-images/timepicker-updatemode.png "TimePicker UpdateMode-plattformspezifisch")](timepicker-selection-images/timepicker-updatemode-large.png#lightbox "TimePicker UpdateMode-plattformspezifisch")

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
