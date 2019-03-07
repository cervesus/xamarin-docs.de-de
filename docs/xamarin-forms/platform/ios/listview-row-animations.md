---
title: ListView-Zeile-Animationen für iOS
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel erläutert das iOS-Plattform-spezifische zu nutzen, die steuert, ob die Zeile Animationen deaktiviert sind, wenn die Auflistung der ListView-Elemente aktualisiert wird.
ms.prod: xamarin
ms.assetid: E8F5103F-4D8E-4A5A-A16C-7FA14EE786AC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/21/2019
ms.openlocfilehash: 50480f5b21c6f0c855ff6f9aa22b6126c6a6787c
ms.sourcegitcommit: 00744f754527e5b55154365f89691caaf1c9d929
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2019
ms.locfileid: "57557442"
---
# <a name="listview-row-animations-on-ios"></a>ListView-Zeile-Animationen für iOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

Diese iOS-Plattform-spezifische steuert Zeile gibt an, ob Animationen sind deaktiviert, wenn die [ `ListView` ](xref:Xamarin.Forms.ListView) Items-Auflistung aktualisiert wird. Es ist in XAML verwendet, durch Festlegen der `ListView.RowAnimationsEnabled` bindbare Eigenschaft `false`:

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

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

listView.On<iOS>().SetRowAnimationsEnabled(false);
```

Die `ListView.On<iOS>` Methode gibt an, dass diese plattformspezifischen nur unter iOS ausgeführt wird. Die `ListView.SetRowAnimationsEnabled` Methode in der [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) -Namespace wird verwendet, um Kontrolle, ob die Zeile Animationen sind deaktiviert, wenn die [ `ListView` ](xref:Xamarin.Forms.ListView) Items-Auflistung aktualisiert wird. Darüber hinaus die `ListView.GetRowAnimationsEnabled` Methode kann verwendet werden, um zurück, ob die Zeile Animationen deaktiviert sind, auf die `ListView`.

> [!NOTE]
> [`ListView`](xref:Xamarin.Forms.ListView) Zeile Animationen sind standardmäßig aktiviert. Aus diesem Grund tritt eine Animation auf, wenn eine neue Zeile eingefügt wird eine `ListView`.

## <a name="related-links"></a>Verwandte Links

- [PlatformSpecifics (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
