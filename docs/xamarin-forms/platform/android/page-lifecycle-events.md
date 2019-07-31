---
title: Seiten Lebenszyklus-Ereignisse unter Android
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie Sie die plattformspezifische Android-Anwendung nutzen, die das Verschwinden und die Anzeige von Seiten Ereignissen für die Anwendungs Unterbrechung bzw. Fortsetzung deaktiviert.
ms.prod: xamarin
ms.assetid: F6E3759C-D347-407A-91A2-CF9B3B7D4CBD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 1745f137f2eeb04c0894c57bb0e45e5c43be7d0b
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68649932"
---
# <a name="page-lifecycle-events-on-android"></a>Seiten Lebenszyklus-Ereignisse unter Android

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese Android-plattformspezifische wird verwendet, um [`Disappearing`](xref:Xamarin.Forms.Page.Appearing) die [`Appearing`](xref:Xamarin.Forms.Page.Appearing) Seiten Ereignisse und für Anwendungen zu deaktivieren, die AppCompat verwenden. Darüber hinaus enthält es die Möglichkeit, um zu steuern, ob die Bildschirmtastatur bei Reaktivierung angezeigt wird, wenn es auf Pause angezeigt wurde, vorausgesetzt, dass der Betriebsmodus, der die Bildschirmtastatur, um festgelegt ist [ `WindowSoftInputModeAdjust.Resize` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize).

> [!NOTE]
> Beachten Sie, dass diese Ereignisse standardmäßig aktiviert sind, vorhandenes Verhalten für Anwendungen beibehalten, die auf den Ereignissen basieren. Deaktivieren diese Ereignisse kann der AppCompat-Ereigniszyklus der Pre-AppCompat-Ereigniszyklus übereinstimmen.

Diese plattformspezifischen kann in XAML verwendet werden, durch Festlegen der [ `Application.SendDisappearingEventOnPause` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendDisappearingEventOnPauseProperty), [ `Application.SendAppearingEventOnResume` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendAppearingEventOnResumeProperty), und [ `Application.ShouldPreserveKeyboardOnResume` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.ShouldPreserveKeyboardOnResumeProperty) angefügte Eigenschaften zu `boolean` Werte:

```xaml
<Application ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"             xmlns:androidAppCompat="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;assembly=Xamarin.Forms.Core"
             android:Application.WindowSoftInputModeAdjust="Resize"
             androidAppCompat:Application.SendDisappearingEventOnPause="false"
             androidAppCompat:Application.SendAppearingEventOnResume="false"
             androidAppCompat:Application.ShouldPreserveKeyboardOnResume="true">
  ...
</Application>
```

Alternativ können sie aus C# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;
...

Xamarin.Forms.Application.Current.On<Android>()
     .UseWindowSoftInputModeAdjust(WindowSoftInputModeAdjust.Resize)
     .SendDisappearingEventOnPause(false)
     .SendAppearingEventOnResume(false)
     .ShouldPreserveKeyboardOnResume(true);
```

Die `Application.Current.On<Android>` Methode gibt an, dass diese plattformspezifischen nur unter Android ausgeführt wird. Die [ `Application.SendDisappearingEventOnPause` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendDisappearingEventOnPause(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application},System.Boolean)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat) -Namespace wird verwendet, aktiviert oder deaktiviert das Auslösen der [ `Disappearing` ](xref:Xamarin.Forms.Page.Appearing) -Ereignis der Seite, wenn die Anwendung Wechselt in den Hintergrund. Die [ `Application.SendAppearingEventOnResume` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendAppearingEventOnResume(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application},System.Boolean)) Methode dient zum Aktivieren oder deaktivieren das Auslösen der [ `Appearing` ](xref:Xamarin.Forms.Page.Appearing) Seitenereignis, wenn die Anwendung im Hintergrund fortgesetzt wird. Die [ `Application.ShouldPreserveKeyboardOnResume` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.ShouldPreserveKeyboardOnResume(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application},System.Boolean)) Methode wird verwendet, Kontrolle, ob die Bildschirmtastatur bei Reaktivierung angezeigt wird, wenn es auf Pause angezeigt wurde bereitgestellt, der Betriebsmodus, der die Bildschirmtastatur eingerichtet wird, um [ `WindowSoftInputModeAdjust.Resize` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize).

Das Ergebnis ist, die [ `Disappearing` ](xref:Xamarin.Forms.Page.Appearing) und [ `Appearing` ](xref:Xamarin.Forms.Page.Appearing) Seitenereignisse wird nicht ausgelöst werden, auf die Anwendung anhalten und fortsetzen bzw. und, auch wenn die Bildschirmtastatur wurde angezeigt wird, wenn die Anwendung wurde angehalten, es wird auch angezeigt, wenn die Anwendung fortgesetzt wird:

[![](page-lifecycle-events-images/keyboard-on-resume.png "Lebenszyklus Ereignisse Clientplattform-spezifische")](page-lifecycle-events-images/keyboard-on-resume-large.png#lightbox "Lebenszyklus Ereignisse Clientplattform-spezifische")

## <a name="related-links"></a>Verwandte Links

- [PlatformSpecifics (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Androidspecific-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [Androidspecific. AppCompat-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
