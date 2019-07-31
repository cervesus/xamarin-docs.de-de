---
title: Weicher Tastatureingabe Modus unter Android
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie Sie die plattformspezifische Android-Anwendung verwenden, die den Betriebsmodus für einen Soft Tastatureingabe Bereich festlegt.
ms.prod: xamarin
ms.assetid: AFCDC92F-F61E-42F6-904B-50B5C4949970
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 835f75b473fe966b5e92e7cb599f7675a7a459dd
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68655672"
---
# <a name="soft-keyboard-input-mode-on-android"></a>Weicher Tastatureingabe Modus unter Android

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese Android-plattformspezifische wird verwendet, um den Betriebsmodus für einen Soft Tastatureingabe Bereich festzulegen, und wird in XAML verwendet [`Application.WindowSoftInputModeAdjust`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Application.WindowSoftInputModeAdjustProperty) , indem die angefügte-Eigenschaft [`WindowSoftInputModeAdjust`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust) auf einen Wert der-Enumeration festgelegt wird:

```xaml
<Application ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
             android:Application.WindowSoftInputModeAdjust="Resize">
  ...
</Application>
```

Alternativ können sie aus C# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

App.Current.On<Android>().UseWindowSoftInputModeAdjust(WindowSoftInputModeAdjust.Resize);
```

Die `Application.On<Android>` Methode gibt an, dass diese plattformspezifischen nur unter Android ausgeführt wird. Die [ `Application.UseWindowSoftInputModeAdjust` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Application.UseWindowSoftInputModeAdjust(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) -Namespace wird verwendet, um die Bildschirmtastatur Eingabebereich Betriebsmodus mit Festlegen der [ `WindowSoftInputModeAdjust` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust) Enumeration, die Bereitstellung von zwei Werten: [ `Pan` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Pan) und [ `Resize` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize). Die `Pan` Wert verwendet die [ `AdjustPan` ](xref:Android.Views.SoftInput.AdjustPan) Anpassung-Option, die Größe des Fensters ändern nicht, wenn ein Steuerelement den Fokus besitzt. Stattdessen werden der Inhalt des Fensters verschoben, damit an, dass der aktuelle Fokus die Bildschirmtastatur verdeckt. Die `Resize` Wert verwendet die [ `AdjustResize` ](xref:Android.Views.SoftInput.AdjustResize) Anpassung-Option, mit der die Fenstergröße wird, wenn ein Eingabesteuerelements, um Platz für die Bildschirmtastatur fokussierte.

Das Ergebnis ist, dass die Bildschirmtastatur Bereich Betriebsmodus kann festgelegt werden Eingabe, wenn ein Steuerelement den Fokus besitzt:

[![](soft-keyboard-input-mode-images/pan-resize.png "Vorläufig Tastatur Betriebssystem Modus plattformspezifische")](soft-keyboard-input-mode-images/pan-resize-large.png#lightbox "Betrieb-Modus plattformspezifische Bildschirmtastatur")

## <a name="related-links"></a>Verwandte Links

- [PlatformSpecifics (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Androidspecific-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [Androidspecific. AppCompat-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
