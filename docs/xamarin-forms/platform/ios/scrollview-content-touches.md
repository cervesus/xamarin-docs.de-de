---
title: Inhalt von ScrollView unter IOS
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie Sie die plattformspezifische IOS-Anwendung nutzen können, um zu steuern, ob eine ScrollView eine touchbewegung behandelt oder an ihren Inhalt übergibt.
ms.prod: xamarin
ms.assetid: 99F823DB-B379-40F0-A343-A9783C341120
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 154666cce4ad6c53949952fa93f5ad7dc89824ab
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68651762"
---
# <a name="scrollview-content-touches-on-ios"></a>Inhalt von ScrollView unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Ein impliziter Timer wird immer dann ausgelöst, wenn eine Touch-Geste in beginnt eine [ `ScrollView` ](xref:Xamarin.Forms.ScrollView) unter iOS und die `ScrollView` entscheidet, basierend auf die Aktion innerhalb der Spanne Timer, sollte die Aktion verarbeiten oder übergeben Sie es an seinen Inhalt. Standardmäßig wird der iOS `ScrollView` Verzögerungen bei der Content-Workflows, aber dadurch können Probleme auftreten, in einigen Fällen kann es bei der `ScrollView` Inhalt ausschlaggebende nicht die Aktion aus, wenn dies erforderlich ist. Aus diesem Grund diese plattformspezifischen steuert, ob eine `ScrollView` behandelt eine Touch-Geste oder übergibt es an seinen Inhalt. Es ist in XAML verwendet, durch Festlegen der `ScrollView.ShouldDelayContentTouches` angefügten Eigenschaft, um eine `boolean` Wert:

```xaml
<MasterDetailPage ...
                  xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <MasterDetailPage.Master>
        <ContentPage Title="Menu" BackgroundColor="Blue" />
    </MasterDetailPage.Master>
    <MasterDetailPage.Detail>
        <ContentPage>
            <ScrollView x:Name="scrollView" ios:ScrollView.ShouldDelayContentTouches="false">
                <StackLayout Margin="0,20">
                    <Slider />
                    <Button Text="Toggle ScrollView DelayContentTouches" Clicked="OnButtonClicked" />
                </StackLayout>
            </ScrollView>
        </ContentPage>
    </MasterDetailPage.Detail>
</MasterDetailPage>
```

Alternativ können sie aus C# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

scrollView.On<iOS>().SetShouldDelayContentTouches(false);
```

Die `ScrollView.On<iOS>` Methode gibt an, dass diese plattformspezifischen nur unter iOS ausgeführt wird. Die `ScrollView.SetShouldDelayContentTouches` Methode in der [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) -Namespace wird verwendet, um Steuerelement gibt an, ob eine [ `ScrollView` ](xref:Xamarin.Forms.ScrollView) behandelt eine Touch-Geste oder übergibt es an seinen Inhalt. Darüber hinaus die `SetShouldDelayContentTouches` Methode kann verwendet werden, um umzuschalten, verzögern Content berührungen durch Aufrufen der `ShouldDelayContentTouches` Methode zurückgeben soll, ob Inhalt Workflows verzögert werden:

```csharp
scrollView.On<iOS>().SetShouldDelayContentTouches(!scrollView.On<iOS>().ShouldDelayContentTouches());
```

Das Ergebnis ist, eine [ `ScrollView` ](xref:Xamarin.Forms.ScrollView) können deaktivieren verzögern Content berührungen, also zu empfangen, die in diesem Szenario die [ `Slider` ](xref:Xamarin.Forms.Slider) empfängt die Bewegung anstelle der [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) auf der Seite die [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage):

[![](scrollview-content-touches-images/scrollview-delay-content-touches.png "ScrollView-Verzögerung Inhalt betrifft plattformspezifische")](scrollview-content-touches-images/scrollview-delay-content-touches-large.png#lightbox "ScrollView-Verzögerung Inhalt betrifft plattformspezifische")

## <a name="related-links"></a>Verwandte Links

- [PlatformSpecifics (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
