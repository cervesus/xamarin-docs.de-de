---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 594e436c9db7c123fea4f9aa262c9d27af765b07
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136005"
---
# <a name="listview-row-animations-on-ios"></a>ListView-Zeilen Animationen unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Mit dieser IOS-plattformspezifischen Steuerung wird gesteuert, ob Zeilen Animationen beim Aktualisieren der Element Sammlung deaktiviert werden [`ListView`](xref:Xamarin.Forms.ListView) . Sie wird in XAML verwendet, indem die `ListView.RowAnimationsEnabled` bindbare Eigenschaft auf festgelegt wird `false` :

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ... ios:ListView.RowAnimationsEnabled="false">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

listView.On<iOS>().SetRowAnimationsEnabled(false);
```

Die- `ListView.On<iOS>` Methode gibt an, dass diese plattformspezifische nur unter IOS ausgeführt wird. Die- `ListView.SetRowAnimationsEnabled` Methode im- [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) Namespace wird verwendet, um zu steuern, ob Zeilen Animationen beim Aktualisieren der Items-Sammlung deaktiviert werden [`ListView`](xref:Xamarin.Forms.ListView) . Außerdem kann die- `ListView.GetRowAnimationsEnabled` Methode verwendet werden, um zurückzugeben, ob Zeilen Animationen in der deaktiviert werden `ListView` .

> [!NOTE]
> [`ListView`](xref:Xamarin.Forms.ListView)Zeilen Animationen sind standardmäßig aktiviert. Daher wird eine Animation ausgeführt, wenn eine neue Zeile in einen eingefügt wird `ListView` .

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
