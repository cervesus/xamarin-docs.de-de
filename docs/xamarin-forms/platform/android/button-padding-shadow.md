---
title: Schaltflächen Auffüll Flächen und Schatten unter Android
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie Sie das plattformspezifische Android-Gerät verwenden, das die Standard Auffüll-und Schatten Werte von Android-Schaltflächen verwendet.
ms.prod: xamarin
ms.assetid: BD2B60D1-DE6E-4691-A777-8EC5F560A4E9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 07b3ff630154b7a59ab7f3395ad9b813da688c74
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656348"
---
# <a name="button-padding-and-shadows-on-android"></a>Schaltflächen Auffüll Flächen und Schatten unter Android

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese Android-plattformspezifischen steuern, ob xamarin. Forms-Schaltflächen die Standard Auffüll-und Schatten Werte von Android-Schaltflächen verwenden. Es ist in XAML verwendet, durch Festlegen der [ `Button.UseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultPaddingProperty) und [ `Button.UseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultShadowProperty) angefügte Eigenschaften zu `boolean` Werte:

```xaml
<ContentPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Button ...
                android:Button.UseDefaultPadding="true"
                android:Button.UseDefaultShadow="true" />         
    </StackLayout>
</ContentPage>
```

Alternativ können sie aus C# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

button.On<Android>().SetUseDefaultPadding(true).SetUseDefaultShadow(true);
```

Die `Button.On<Android>` Methode gibt an, dass diese plattformspezifischen nur unter Android ausgeführt wird. Mit [`Button.SetUseDefaultPadding`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.SetUseDefaultPadding(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button},System.Boolean)) der [`Button.SetUseDefaultShadow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.SetUseDefaultShadow(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button},System.Boolean)) -Methode und der [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) -Methode im-Namespace wird gesteuert, ob xamarin. Forms-Schaltflächen die Standard Auffüll-und Schatten Werte von Android-Schaltflächen verwenden. Darüber hinaus die [ `Button.UseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultPadding(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button})) und [ `Button.UseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultShadow(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button})) Methoden können verwendet werden, um zurück, ob eine Schaltfläche bzw. padding-Wert und Standardwert Schattenkopien zugewiesen wurde, verwendet.

Das Ergebnis ist, dass Xamarin.Forms-Schaltflächen die standardauffüllung und den Volumeschattenkopie-Werte, der Android-Schaltflächen verwenden können:

![](button-padding-shadow-images/button-padding-and-shadow.png "Standard Auffüll-und Schatten Werte für Android-Schaltflächen")

Beachten Sie, dass im Screenshot oben jedes [ `Button` ](xref:Xamarin.Forms.Button) verfügt über identische Definitionen, außer dass die rechten `Button` verwendet die standardauffüllung und den Volumeschattenkopie-Werte, der Android-Schaltflächen.

## <a name="related-links"></a>Verwandte Links

- [PlatformSpecifics (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Androidspecific-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [Androidspecific. AppCompat-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
