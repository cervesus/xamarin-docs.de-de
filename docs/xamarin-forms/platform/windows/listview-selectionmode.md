---
title: ListView SelectionMode unter Windows
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie Sie die Windows-plattformspezifische verwenden, die steuert, ob Elemente in einer ListView auf Tap-Gesten reagieren können.
ms.prod: xamarin
ms.assetid: 57EF3A7F-1407-4B31-AE21-D149293D4228
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: f6a90a8a0397db99a245f706450e7dc83097a45e
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656898"
---
# <a name="listview-selectionmode-on-windows"></a>ListView SelectionMode unter Windows

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Für die universelle Windows-Plattform, standardmäßig die Xamarin.Forms [ `ListView` ](xref:Xamarin.Forms.ListView) verwendet die systemeigenen `ItemClick` Ereignis, um auf die Interaktion, anstatt das systemeigene reagieren `Tapped` Ereignis. Dadurch wird die Barrierefreiheitsfunktionen, sodass die Windows-Sprachausgabe und Tastatur interagieren können die `ListView`. Allerdings rendert es auch alle Bewegungen Tippen Sie in der `ListView` nicht mehr funktionsfähig.

Dies universelle Windows-Plattform plattformspezifische Steuerelemente, ob Elemente [`ListView`](xref:Xamarin.Forms.ListView) in einer auf Tap-Gesten reagieren können und ob das `ListView` Native- `ItemClick` Ereignis `Tapped` das-Ereignis oder das-Ereignis auslöst. Es ist in XAML verwendet, durch Festlegen der [ `ListView.SelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.SelectionModeProperty) angefügte Eigenschaft auf den Wert der [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) Enumeration:

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

Alternativ können sie aus C# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

listView.On<Windows>().SetSelectionMode(ListViewSelectionMode.Inaccessible);
```

Die `ListView.On<Windows>` Methode gibt an, dass diese plattformspezifischen nur für die universelle Windows-Plattform ausgeführt wird. Die [ `ListView.SetSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.SetSelectionMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.ListView},Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) -Namespace wird verwendet, um Steuerelement gibt an, ob Elemente in einer [ `ListView` ](xref:Xamarin.Forms.ListView) tippen von Bewegungen mit reagieren können die [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) Enumeration, die Bereitstellung von zwei möglicher Werten:

- [`Accessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Accessible) – Gibt an, dass die `ListView` wird ausgelöst, die Native `ItemClick` Ereignis behandeln Interaktion, und geben Sie daher die Barrierefreiheitsfunktionen. Aus diesem Grund die Windows-Sprachausgabe und Tastatur interagieren die `ListView`. Jedoch Elemente in der `ListView` Gesten tippen kann nicht reagieren. Dies ist das Standardverhalten für `ListView` Instanzen für die universelle Windows-Plattform.
- [`Inaccessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Inaccessible) – Gibt an, dass die `ListView` wird ausgelöst, die Native `Tapped` zu Interaktion zu behandelnden Ereignisses. Aus diesem Grund Elemente in der `ListView` Gesten tippen reagieren können. Allerdings sind keine Barrierefreiheitsfunktionen vorhanden und somit die Windows-Sprachausgabe und die Tastatur können nicht interagieren mit der `ListView`.

> [!NOTE]
> Die `Accessible` und `Inaccessible` die Modi schließen sich gegenseitig aus, und Sie müssen eine zugängliche Wahlmöglichkeiten [ `ListView` ](xref:Xamarin.Forms.ListView) oder `ListView` , die auf Gesten tippen reagieren können.

Darüber hinaus die [ `GetSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.GetSelectionMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.ListView})) Methode kann verwendet werden, um das aktuelle [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode).

Das Ergebnis ist, die einem angegebenen [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) gilt, an die [ `ListView` ](xref:Xamarin.Forms.ListView), steuert, ob Elemente in der `ListView` Gesten, tippen Sie auf reagieren können und somit, ob das systemeigene `ListView` löst die `ItemClick` oder `Tapped` Ereignis.

## <a name="related-links"></a>Verwandte Links

- [PlatformSpecifics (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Windowsspecific-API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
