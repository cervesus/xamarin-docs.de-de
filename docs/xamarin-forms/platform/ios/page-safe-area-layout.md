---
title: Handbuch zum Safe Area Layout unter IOS
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie Sie die plattformspezifische IOS-Anwendung nutzen, um sicherzustellen, dass Seiteninhalte in einem Bereich des Bildschirms positioniert sind, der für alle Geräte mit IOS 11 und höher sicher ist.
ms.prod: xamarin
ms.assetid: 2B6789C1-39B4-4C16-ADE1-3ED3378EAC63
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: c6a2ec5a4d1466b7118e6cc7b03cc5518b27e2fb
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68644542"
---
# <a name="safe-area-layout-guide-on-ios"></a>Handbuch zum Safe Area Layout unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese IOS-plattformspezifische wird verwendet, um sicherzustellen, dass der Seiten Inhalt in einem Bereich des Bildschirms positioniert ist, der für alle Geräte, die IOS 11 und höher verwenden, sicher ist. Insbesondere leichter sicherstellen, dass der Inhalt wird nicht gerundet Gerät Ecken, home Indikators oder der Sensor wohnkosten auf einem iPhone X abgeschnitten. Es ist in XAML verwendet, durch Festlegen der `Page.UseSafeArea` angefügten Eigenschaft, um eine `boolean` Wert:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             Title="Safe Area"
             ios:Page.UseSafeArea="true">
    <StackLayout>
        ...
    </StackLayout>
</ContentPage>
```

Alternativ können sie aus C# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetUseSafeArea(true);
```

Die `Page.On<iOS>` Methode gibt an, dass diese plattformspezifischen nur unter iOS ausgeführt wird. Die `Page.SetUseSafeArea` Methode in der [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) Namespace steuert, ob der Bereich für geschützte Layoutführungslinie aktiviert ist.

Das Ergebnis ist, dass der Inhalt der Seite auf einen Bereich des Bildschirms positioniert werden kann, die für alle iPhones sicher ist:

[![](page-safe-area-images/safe-area-layout.png "Sicherheitsbereich Layoutführungslinie")](page-safe-area-images/safe-area-layout-large.png#lightbox "Sicherheitsbereich Layoutführungslinie")

> [!NOTE]
> Der sichere Bereich definiert, die von Apple dient in Xamarin.Forms zum Festlegen der [ `Page.Padding` ](xref:Xamarin.Forms.Page.Padding) -Eigenschaft, und überschreibt alle vorherigen Werte dieser Eigenschaft, die festgelegt wurden.

Der sichere Bereich kann angepasst werden, durch Abrufen der [ `Thickness` ](xref:Xamarin.Forms.Thickness) Wert mit einer der `Page.SafeAreaInsets` Methode aus der [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) Namespace. Können dann als geändert werden erforderlich neu zugewiesen zu, und wählen Sie die `Padding` -Eigenschaft in der Page-Konstruktor oder [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) außer Kraft setzen:

```csharp
protected override void OnAppearing()
{
    base.OnAppearing();

    var safeInsets = On<iOS>().SafeAreaInsets();
    safeInsets.Left = 20;
    Padding = safeInsets;
}
```

## <a name="related-links"></a>Verwandte Links

- [PlatformSpecifics (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
