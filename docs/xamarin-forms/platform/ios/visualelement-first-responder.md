---
title: Visualelement First Response on IOS
description: Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie die plattformspezifische IOS-Anwendung nutzen können, mit der ein visualelement-Objekt zum ersten Antwort Ereignis wird.
ms.prod: xamarin
ms.assetid: 3A77BA02-B87A-44EC-AC51-9D3130EF314C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/15/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d8bd539c2bb0e8963afae3392b6f8e99d79af9af
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136968"
---
# <a name="visualelement-first-responder-on-ios"></a>Visualelement First Response on IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Mit diesem IOS-plattformspezifischen ist es möglich, dass ein- [`VisualElement`](xref:Xamarin.Forms.VisualElement) Objekt zum ersten Response-Ereignis wird, anstatt die Seite, die das Element enthält. Sie wird in XAML verwendet, indem die `VisualElement.CanBecomeFirstResponder` bindbare Eigenschaft auf festgelegt wird `true` :

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

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Entry entry = new Entry { Placeholder = "Enter text" };
Button button = new Button { Text = "OK" };
button.On<iOS>().SetCanBecomeFirstResponder(true);
```

Die- `VisualElement.On<iOS>` Methode gibt an, dass diese plattformspezifische nur unter IOS ausgeführt wird. Die- `VisualElement.SetCanBecomeFirstResponder` Methode im- [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) Namespace wird verwendet, um den so festzulegen, `VisualElement` dass er der erste Beantworter für Berührungs Ereignisse wird. Außerdem kann die- `VisualElement.CanBecomeFirstResponder` Methode verwendet werden, um zurückzugeben, ob das `VisualElement` der erste Antwort für das Berühren von Ereignissen ist.

Das Ergebnis ist, dass ein als [`VisualElement`](xref:Xamarin.Forms.VisualElement) erster Beantworter für Berührungs Ereignisse und nicht für die Seite, die das Element enthält, werden kann. Dadurch können Szenarien wie Chat Anwendungen eine Tastatur nicht verwerfen, wenn ein [`Button`](xref:Xamarin.Forms.Button) getippt wird.

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
