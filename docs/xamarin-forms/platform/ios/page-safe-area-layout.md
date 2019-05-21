---
title: Sichere Führungslinie für IOS
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie der iOS-Plattform-spezifische genutzt, die sicherstellt, dass der Inhalt der Seite auf einen Bereich des Bildschirms befindet, die für alle Geräte sicher ist, die iOS 11 und höher verwenden.
ms.prod: xamarin
ms.assetid: 2B6789C1-39B4-4C16-ADE1-3ED3378EAC63
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: f38064027b4eb6dada2becc69b4163d6fa6082fb
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2019
ms.locfileid: "65927021"
---
# <a name="safe-area-layout-guide-on-ios"></a>Sichere Führungslinie für IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)

Dieses iOS-Plattform-spezifische wird verwendet, um sicherzustellen, dass der Inhalt der Seite auf einen Bereich des Bildschirms befindet, die für alle Geräte sicher ist, die iOS 11 und höher verwenden. Insbesondere leichter sicherstellen, dass der Inhalt wird nicht gerundet Gerät Ecken, home Indikators oder der Sensor wohnkosten auf einem iPhone X abgeschnitten. Es ist in XAML verwendet, durch Festlegen der `Page.UseSafeArea` angefügten Eigenschaft, um eine `boolean` Wert:

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

- [PlatformSpecifics (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
