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
ms.openlocfilehash: 6c73f46d2845be7bb54e24cd02ec22f3c2cd386d
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137245"
---
# <a name="listview-selectionmode-on-windows"></a>ListView SelectionMode unter Windows

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Auf dem universelle Windows-Plattform verwendet standardmäßig Xamarin.Forms [`ListView`](xref:Xamarin.Forms.ListView) das Native- `ItemClick` Ereignis, um auf die Interaktion statt auf das Native Ereignis zu reagieren `Tapped` . Dies bietet Barrierefreiheits Funktionen, sodass die Windows-Sprachausgabe und die Tastatur mit dem interagieren können `ListView` . Allerdings rendert Sie auch beliebige Tap-Gesten innerhalb der nicht `ListView` operable.

Dies universelle Windows-Plattform plattformspezifische Steuerelemente, ob Elemente in einer [`ListView`](xref:Xamarin.Forms.ListView) auf Tap-Gesten reagieren können und ob das Native- `ListView` Ereignis das- `ItemClick` Ereignis oder das-Ereignis auslöst `Tapped` . Sie wird in XAML verwendet, indem die [`ListView.SelectionMode`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.SelectionModeProperty) angefügte-Eigenschaft auf einen Wert der-Enumeration festgelegt wird [`ListViewSelectionMode`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) :

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <ListView ... windows:ListView.SelectionMode="Inaccessible">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

listView.On<Windows>().SetSelectionMode(ListViewSelectionMode.Inaccessible);
```

Die- `ListView.On<Windows>` Methode gibt an, dass diese plattformspezifische nur auf der universelle Windows-Plattform ausgeführt wird. [ `ListView.SetSelectionMode` ] (Xref: Xamarin.Forms . Platformconfiguration. windowsspecific. ListView. setSelectionMode ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. Windows, Xamarin.Forms . ListView}, Xamarin.Forms . Platformconfiguration. windowsspecific. listviewselectionmode))-Methode im- [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) Namespace wird verwendet, um zu steuern, ob Elemente in einer [`ListView`](xref:Xamarin.Forms.ListView) auf Tap-Gesten reagieren können, wobei die- [`ListViewSelectionMode`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) Enumeration zwei mögliche Werte bereitstellt:

- [`Accessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Accessible)– Gibt an, dass das `ListView` native `ItemClick` Ereignis auslöst, um die Interaktion zu verarbeiten, und somit Barrierefreiheits Funktionen bereitstellen. Daher können die Windows-Sprachausgabe und die Tastatur mit interagieren `ListView` . Allerdings können Elemente in der `ListView` nicht auf Tap-Gesten reagieren. Dies ist das Standardverhalten für- `ListView` Instanzen auf dem universelle Windows-Plattform.
- [`Inaccessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Inaccessible)– Gibt an, dass das `ListView` native `Tapped` Ereignis auslöst, um die Interaktion zu verarbeiten. Daher können Elemente in `ListView` auf Tap-Gesten reagieren. Allerdings gibt es keine Barrierefreiheits Funktionen, weshalb die Windows-Sprachausgabe und die Tastatur nicht mit dem interagieren können `ListView` .

> [!NOTE]
> Die `Accessible` `Inaccessible` Auswahl Modi und schließen sich gegenseitig aus, und Sie müssen zwischen einem zugänglichen [`ListView`](xref:Xamarin.Forms.ListView) oder einem wählen, `ListView` das auf Tap-Gesten reagieren kann.

Außerdem ist [ `GetSelectionMode` ] (Xref: Xamarin.Forms . Platformconfiguration. windowsspecific. ListView. getSelectionMode ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. Windows, Xamarin.Forms . ListView}))-Methode kann verwendet werden, um die aktuelle-Methode zurückzugeben [`ListViewSelectionMode`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) .

Das Ergebnis ist, dass ein [`ListViewSelectionMode`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) angegebenes auf den angewendet wird [`ListView`](xref:Xamarin.Forms.ListView) , der steuert, ob Elemente im `ListView` auf Tap-Gesten reagieren können, und ob der Native `ListView` das- `ItemClick` Ereignis oder das-Ereignis auslöst `Tapped` .

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Windowsspecific-API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
