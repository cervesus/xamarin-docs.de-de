---
title: ListView-Trennzeichen Stil unter IOS
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie Sie die plattformspezifische IOS-Anwendung nutzen, die steuert, ob das Trennzeichen zwischen Zellen in einer ListView die volle Breite von ListView verwendet.
ms.prod: xamarin
ms.assetid: A4CB45CE-9FB7-47ED-8C3D-93E39BF282E4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 921ef905eed17b6bfe5cb2ea401f7da92ff744af
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68648114"
---
# <a name="listview-separator-style-on-ios"></a>ListView-Trennzeichen Stil unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Mit diesem IOS-plattformspezifischen Steuerelement wird gesteuert, ob [`ListView`](xref:Xamarin.Forms.ListView) das Trennzeichen zwischen den Zellen in einer die vollständige Breite des verwendet. `ListView` Es ist in XAML verwendet, durch Festlegen der [ `ListView.SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.ListView.SeparatorStyleProperty) angefügte Eigenschaft auf den Wert der [ `SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) Enumeration:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ... ios:ListView.SeparatorStyle="FullWidth">
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

listView.On<iOS>().SetSeparatorStyle(SeparatorStyle.FullWidth);
```

Die `ListView.On<iOS>` Methode gibt an, dass diese plattformspezifischen nur unter iOS ausgeführt wird. Die [ `ListView.SetSeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.ListView.SetSeparatorStyle(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.ListView},Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) -Namespace wird verwendet, um Kontrolle, ob in das Trennzeichen zwischen den Zellen der [ `ListView` ](xref:Xamarin.Forms.ListView) verwendet die vollständige Breite der `ListView`, mit der [ `SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) Enumeration, die Bereitstellung von zwei möglicher Werten:

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle.Default) – Gibt das Standardverhalten für iOS-Trennzeichen. Dies ist das Standardverhalten in Xamarin.Forms.
- [`FullWidth`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle.FullWidth) – Gibt an, dass Trennzeichen von einer Kante gezeichnet werden soll, werden die `ListView` zum anderen.

Das Ergebnis ist, die einem angegebenen [ `SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) Wert gilt für die [ `ListView` ](xref:Xamarin.Forms.ListView), die die Breite des Trennzeichens zwischen den Zellen steuert:

![](listview-separator-style-images/listview-separatorstyle.png "ListView SeparatorStyle plattformspezifische")

> [!NOTE]
> Nachdem Sie der Trennzeichenstil festgelegt wurde, `FullWidth`, es kann nicht geändert werden, an `Default` zur Laufzeit.

## <a name="related-links"></a>Verwandte Links

- [PlatformSpecifics (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
