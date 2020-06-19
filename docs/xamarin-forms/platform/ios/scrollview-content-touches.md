---
title: Inhalt von ScrollView unter IOS
description: Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie die plattformspezifische IOS-Anwendung nutzen können, um zu steuern, ob eine ScrollView eine touchbewegung behandelt oder an ihren Inhalt übergibt.
ms.prod: xamarin
ms.assetid: 99F823DB-B379-40F0-A343-A9783C341120
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9b8f743b2c3d7f4b38feb4cfc5015b1113620562
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84137097"
---
# <a name="scrollview-content-touches-on-ios"></a>Inhalt von ScrollView unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Ein impliziter Zeitgeber wird ausgelöst, wenn eine touchbewegung in einer [`ScrollView`](xref:Xamarin.Forms.ScrollView) unter IOS beginnt und der `ScrollView` auf der Grundlage der Benutzeraktion innerhalb der Zeit Geber Spanne entscheidet, ob er die Bewegung verarbeiten oder an seinen Inhalt übergeben soll. Standardmäßig verzögert die IOS- `ScrollView` Inhalte Inhalte, dies kann jedoch in einigen Fällen zu Problemen führen, `ScrollView` Wenn der Inhalt die Geste nicht gewinnt. Diese plattformspezifische Steuerung steuert, ob eine `ScrollView` touchbewegung behandelt oder an ihren Inhalt übergibt. Sie wird in XAML verwendet, indem die `ScrollView.ShouldDelayContentTouches` angefügte-Eigenschaft auf einen Wert festgelegt wird `boolean` :

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

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

scrollView.On<iOS>().SetShouldDelayContentTouches(false);
```

Die- `ScrollView.On<iOS>` Methode gibt an, dass diese plattformspezifische nur unter IOS ausgeführt wird. Die- `ScrollView.SetShouldDelayContentTouches` Methode im- [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) Namespace wird verwendet, um zu steuern, ob ein eine [`ScrollView`](xref:Xamarin.Forms.ScrollView) touchbewegung behandelt oder an seinen Inhalt übergibt. Außerdem kann die- `SetShouldDelayContentTouches` Methode verwendet werden, um das Verzögern von Inhalts Berührungen zu umschalten, indem die-Methode aufgerufen wird `ShouldDelayContentTouches` , um zurückzugeben, ob Inhalts Berührungen verzögert werden

```csharp
scrollView.On<iOS>().SetShouldDelayContentTouches(!scrollView.On<iOS>().ShouldDelayContentTouches());
```

Das Ergebnis ist, dass ein das [`ScrollView`](xref:Xamarin.Forms.ScrollView) verzögern des empfangenden Inhalts berührt deaktivieren kann, sodass in diesem Szenario die [`Slider`](xref:Xamarin.Forms.Slider) Bewegung anstelle der [`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail) Seite von empfängt [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) :

[![](scrollview-content-touches-images/scrollview-delay-content-touches.png "ScrollView Delay Content Touches Platform-Specific")](scrollview-content-touches-images/scrollview-delay-content-touches-large.png#lightbox "ScrollView Delay Content Touches Platform-Specific")

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
