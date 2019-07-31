---
title: ListView-Zeilen Animationen unter IOS
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie Sie die plattformspezifische IOS-Anwendung nutzen können, um zu steuern, ob Zeilen Animationen beim Aktualisieren der ListView Items-Auflistung deaktiviert werden.
ms.prod: xamarin
ms.assetid: E8F5103F-4D8E-4A5A-A16C-7FA14EE786AC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/21/2019
ms.openlocfilehash: 9e44c22e670847102cf0bc0f79a860abcac30760
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68652833"
---
# <a name="listview-row-animations-on-ios"></a>ListView-Zeilen Animationen unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Mit dieser IOS-plattformspezifischen Steuerung wird gesteuert, ob Zeilen [`ListView`](xref:Xamarin.Forms.ListView) Animationen beim Aktualisieren der Element Sammlung deaktiviert werden. Es ist in XAML verwendet, durch Festlegen der `ListView.RowAnimationsEnabled` bindbare Eigenschaft `false`:

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

Alternativ können sie aus C# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

listView.On<iOS>().SetRowAnimationsEnabled(false);
```

Die `ListView.On<iOS>` Methode gibt an, dass diese plattformspezifischen nur unter iOS ausgeführt wird. Die `ListView.SetRowAnimationsEnabled` -Methode [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) im-Namespace wird verwendet, um zu steuern, ob Zeilen Animationen beim Aktualisieren [`ListView`](xref:Xamarin.Forms.ListView) der Items-Sammlung deaktiviert werden. Außerdem kann die `ListView.GetRowAnimationsEnabled` -Methode verwendet werden, um zurückzugeben, ob Zeilen Animationen in der `ListView`deaktiviert werden.

> [!NOTE]
> [`ListView`](xref:Xamarin.Forms.ListView)Zeilen Animationen sind standardmäßig aktiviert. Daher wird eine Animation ausgeführt, wenn eine neue Zeile in einen `ListView`eingefügt wird.

## <a name="related-links"></a>Verwandte Links

- [PlatformSpecifics (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
