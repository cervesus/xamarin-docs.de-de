---
title: ListView-Zeilen Animationen unter IOS
description: Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie die plattformspezifische IOS-Anwendung nutzen können, um zu steuern, ob Zeilen Animationen beim Aktualisieren der ListView Items-Auflistung deaktiviert werden.
ms.prod: xamarin
ms.assetid: E8F5103F-4D8E-4A5A-A16C-7FA14EE786AC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/21/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 594e436c9db7c123fea4f9aa262c9d27af765b07
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
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
