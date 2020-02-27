---
title: Visualelement First Response on IOS
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie Sie die plattformspezifische IOS-Anwendung nutzen können, mit der ein visualelement-Objekt zum ersten Antwort Ereignis wird.
ms.prod: xamarin
ms.assetid: 3A77BA02-B87A-44EC-AC51-9D3130EF314C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/15/2020
ms.openlocfilehash: be6c233b63d172d2fcacb1cea7f5e9aeeb7faed1
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2020
ms.locfileid: "77646713"
---
# <a name="visualelement-first-responder-on-ios"></a>Visualelement First Response on IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese IOS-plattformspezifische ermöglicht, dass ein [`VisualElement`](xref:Xamarin.Forms.VisualElement) Objekt zum ersten Response-Ereignis wird, anstatt die Seite, die das Element enthält. Sie wird in XAML verwendet, indem die `VisualElement.CanBecomeFirstResponder` bindbare Eigenschaft auf `true`festgelegt wird:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <Entry Placeholder="Enter text" />
        <Button ios:VisualElement.CanBecomeFirstResponder="True"
                Text="OK" />
    </StackLayout>
</ContentPage>
```

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Entry entry = new Entry { Placeholder = "Enter text" };
Button button = new Button { Text = "OK" };
button.On<iOS>().SetCanBecomeFirstResponder(true);
```

Die `VisualElement.On<iOS>`-Methode gibt an, dass diese plattformspezifische nur unter IOS ausgeführt wird. Die `VisualElement.SetCanBecomeFirstResponder`-Methode im [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) -Namespace wird verwendet, um die `VisualElement` so festzulegen, dass Sie der erste Beantworter für Berührungs Ereignisse wird. Außerdem kann die `VisualElement.CanBecomeFirstResponder`-Methode verwendet werden, um zurückzugeben, ob die `VisualElement` der erste Antwort Dienst für die Berührungs Ereignisse ist.

Das Ergebnis ist, dass ein [`VisualElement`](xref:Xamarin.Forms.VisualElement) zum ersten Antwort Dienst für Berührungs Ereignisse werden kann, anstatt der Seite, die das Element enthält. Dadurch können Szenarien wie Chat Anwendungen eine Tastatur nicht verwerfen, wenn ein [`Button`](xref:Xamarin.Forms.Button) getippt wird.

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
