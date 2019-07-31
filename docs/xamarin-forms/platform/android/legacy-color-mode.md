---
title: Visualelement-Legacy Farben Modus unter Android
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie Sie das plattformspezifische Android-Dokument verwenden, das den xamarin. Forms-Legacy Farb Modus deaktiviert.
ms.prod: xamarin
ms.assetid: 37D95A2D-74AC-488A-B903-2BDD799EAA5C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: ad7b5b7bae131a58e16c77eca73e24834a57bf3a
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68655688"
---
# <a name="visualelement-legacy-color-mode-on-android"></a>Visualelement-Legacy Farben Modus unter Android

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Einige der Xamarin.Forms-Ansichten bieten eines ältere Farbmodus. In diesem Modus bei der [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) -Eigenschaft der Ansicht nastaven NA hodnotu `false`, die Ansicht überschreibt die Farben, die vom Benutzer mit den systemeigenen Standardfarben für den deaktivierten Zustand festgelegt wird. Für die Abwärtskompatibilität dieser älteren Farbmodus-Kompatibilität für das Standardverhalten für die unterstützten Ansichten bleibt.

Mit dieser Android-plattformspezifischen Option wird dieser Legacy Farb Modus deaktiviert, sodass die für eine Ansicht des Benutzers festgelegten Farben auch dann beibehalten werden, wenn die Ansicht deaktiviert ist. Es ist in XAML verwendet, durch Festlegen der [ `VisualElement.IsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.IsLegacyColorModeEnabledProperty) angefügte Eigenschaft zu `false`:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Button Text="Button"
                TextColor="Blue"
                BackgroundColor="Bisque"
                android:VisualElement.IsLegacyColorModeEnabled="False" />
        ...
    </StackLayout>
</ContentPage>
```

Alternativ können sie aus C# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

_legacyColorModeDisabledButton.On<Android>().SetIsLegacyColorModeEnabled(false);
```

Die `VisualElement.On<Android>` Methode gibt an, dass diese plattformspezifischen nur unter Android ausgeführt wird. Die [ `VisualElement.SetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.SetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.VisualElement},System.Boolean)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) -Namespace wird verwendet, um Kontrolle, ob die älteren Farbmodus deaktiviert ist. Darüber hinaus die [ `VisualElement.GetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.GetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.VisualElement})) Methode kann verwendet werden, um zurück, ob es sich bei der älteren Farbmodus deaktiviert ist.

Das Ergebnis ist, dass die ältere Farbmodus deaktiviert werden kann, Farben, die für eine Sicht vom Benutzer festgelegt auch bleiben, wenn die Sicht deaktiviert ist:

![](legacy-color-mode-images/legacy-color-mode-disabled.png "Ältere Farbmodus deaktiviert")

> [!NOTE]
> Beim Festlegen einer [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) für eine Sicht, die ältere Farbmodus vollständig ignoriert wird. Weitere Informationen zu visuellen Status finden Sie unter [die Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="related-links"></a>Verwandte Links

- [PlatformSpecifics (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Androidspecific-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [Androidspecific. AppCompat-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
