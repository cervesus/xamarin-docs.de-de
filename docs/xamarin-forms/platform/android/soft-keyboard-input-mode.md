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
ms.openlocfilehash: c62d09c7d7848d9f62c018caa1698bb53a2a39a8
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84128712"
---
# <a name="soft-keyboard-input-mode-on-android"></a>Weicher Tastatureingabe Modus unter Android

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese Android-plattformspezifische wird verwendet, um den Betriebsmodus für einen Soft Tastatureingabe Bereich festzulegen, und wird in XAML verwendet, indem die [`Application.WindowSoftInputModeAdjust`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Application.WindowSoftInputModeAdjustProperty) angefügte-Eigenschaft auf einen Wert der-Enumeration festgelegt wird [`WindowSoftInputModeAdjust`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust) :

```xaml
<Application ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
             android:Application.WindowSoftInputModeAdjust="Resize">
  ...
</Application>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

App.Current.On<Android>().UseWindowSoftInputModeAdjust(WindowSoftInputModeAdjust.Resize);
```

Die- `Application.On<Android>` Methode gibt an, dass diese plattformspezifische nur unter Android ausgeführt wird. [ `Application.UseWindowSoftInputModeAdjust` ] (Xref: Xamarin.Forms . Platformconfiguration. androidspecific. Application. usewindowsoft inputmudeadjust ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. Android, Xamarin.Forms . Anwendung}, Xamarin.Forms . Platformconfiguration. androidspecific. windowsoftinputmudeadjust))-Methode im- [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) Namespace wird verwendet, um den Betriebsmodus für den Soft Tastatureingabe Bereich festzulegen, wobei die [`WindowSoftInputModeAdjust`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust) -Enumeration zwei Werte bereitstellt: [`Pan`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Pan) und [`Resize`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize) . Der `Pan` Wert verwendet die [`AdjustPan`](xref:Android.Views.SoftInput.AdjustPan) Anpassungs Option, bei der die Größe des Fensters nicht geändert wird, wenn ein Eingabe Steuerelement den Fokus besitzt. Stattdessen wird der Inhalt des Fensters so fixiert, dass der aktuelle Fokus nicht durch die weiche Tastatur verdeckt wird. Der `Resize` Wert verwendet die [`AdjustResize`](xref:Android.Views.SoftInput.AdjustResize) Anpassungs Option, die die Größe des Fensters ändert, wenn ein Eingabe Steuerelement den Fokus besitzt, um Platz für die weiche Tastatur zu schaffen.

Das Ergebnis ist, dass der Betriebsmodus für den Soft Tastatureingabe Bereich festgelegt werden kann, wenn ein Eingabe Steuerelement den Fokus hat:

[![](soft-keyboard-input-mode-images/pan-resize.png "Soft Keyboard Operating Mode Platform-Specific")](soft-keyboard-input-mode-images/pan-resize-large.png#lightbox "Soft Keyboard Operating Mode Platform-Specific")

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Androidspecific-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [Androidspecific. AppCompat-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
