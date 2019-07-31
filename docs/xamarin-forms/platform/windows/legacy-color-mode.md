---
title: Visualelement-Legacy Farb Modus unter Windows
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie Sie das Windows-plattformspezifische verwenden, das den xamarin. Forms-Legacy Farb Modus deaktiviert.
ms.prod: xamarin
ms.assetid: B8759309-07C7-4DCA-A18A-C1A198A7951B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 7319b0886476ea502b7b9c450416cb4fe69e01fa
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656909"
---
# <a name="visualelement-legacy-color-mode-on-windows"></a>Visualelement-Legacy Farb Modus unter Windows

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Einige der Xamarin.Forms-Ansichten bieten eines ältere Farbmodus. In diesem Modus bei der [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) -Eigenschaft der Ansicht nastaven NA hodnotu `false`, die Ansicht überschreibt die Farben, die vom Benutzer mit den systemeigenen Standardfarben für den deaktivierten Zustand festgelegt wird. Für die Abwärtskompatibilität dieser älteren Farbmodus-Kompatibilität für das Standardverhalten für die unterstützten Ansichten bleibt.

Diese universelle Windows-Plattform plattformspezifisch deaktiviert diesen Legacy Farb Modus, sodass Farben, die für eine Ansicht durch den Benutzer festgelegt sind, auch dann beibehalten werden, wenn die Ansicht deaktiviert ist. Es ist in XAML verwendet, durch Festlegen der [ `VisualElement.IsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.IsLegacyColorModeEnabledProperty) angefügte Eigenschaft zu `false`:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Editor Text="Enter text here"
                TextColor="Blue"
                BackgroundColor="Bisque"
                windows:VisualElement.IsLegacyColorModeEnabled="False" />
        ...
    </StackLayout>
</ContentPage>
```

Alternativ können sie aus C# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

_legacyColorModeDisabledEditor.On<Windows>().SetIsLegacyColorModeEnabled(false);
```

Die `VisualElement.On<Windows>` Methode angibt, dass diese plattformspezifischen nur auf Windows ausgeführt wird. Die [ `VisualElement.SetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.Boolean)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) -Namespace wird verwendet, um Kontrolle, ob die älteren Farbmodus deaktiviert ist. Darüber hinaus die [ `VisualElement.GetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})) Methode kann verwendet werden, um zurück, ob es sich bei der älteren Farbmodus deaktiviert ist.

Das Ergebnis ist, dass die ältere Farbmodus deaktiviert werden kann, Farben, die für eine Sicht vom Benutzer festgelegt auch bleiben, wenn die Sicht deaktiviert ist:

![](legacy-color-mode-images/legacy-color-mode-disabled.png "Ältere Farbmodus deaktiviert")

> [!NOTE]
> Beim Festlegen einer [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) für eine Sicht, die ältere Farbmodus vollständig ignoriert wird. Weitere Informationen zu visuellen Status finden Sie unter [die Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="related-links"></a>Verwandte Links

- [PlatformSpecifics (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Windowsspecific-API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
