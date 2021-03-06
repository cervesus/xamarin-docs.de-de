---
title: Weicher Tastatureingabe Modus unter Android
description: Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie die plattformspezifische Android-Anwendung verwenden, die den Betriebsmodus für einen Soft Tastatureingabe Bereich festlegt.
ms.prod: xamarin
ms.assetid: AFCDC92F-F61E-42F6-904B-50B5C4949970
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0c7a379fa0128f73af471509974a043dbf2475d3
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86933314"
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

[![Soft-Tastatur-Betriebsmodus plattformspezifisch](soft-keyboard-input-mode-images/pan-resize.png)](soft-keyboard-input-mode-images/pan-resize-large.png#lightbox "Soft-Tastatur-Betriebsmodus plattformspezifisch")

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Androidspecific-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [Androidspecific. AppCompat-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
