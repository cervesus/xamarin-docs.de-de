---
title: Visualelement-Legacy Farben Modus unter Android
description: Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie die plattformspezifische Android-Version verwenden, die den Xamarin.Forms Legacy Farb Modus deaktiviert.
ms.prod: xamarin
ms.assetid: 37D95A2D-74AC-488A-B903-2BDD799EAA5C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0e4300354588e5055e58251caa547a56c751fd94
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86936070"
---
# <a name="visualelement-legacy-color-mode-on-android"></a>Visualelement-Legacy Farben Modus unter Android

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Einige der Xamarin.Forms Ansichten verfügen über einen Legacy Farb Modus. In diesem Modus, wenn die- [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled) Eigenschaft der-Sicht auf festgelegt ist, überschreibt `false` die Ansicht die vom Benutzer festgelegten Farben mit den standardmäßigen systemeigenen Farben für den deaktivierten Zustand. Aus Gründen der Abwärtskompatibilität bleibt dieser Legacy Farb Modus das Standardverhalten für unterstützte Ansichten.

Mit dieser Android-plattformspezifischen Option wird dieser Legacy Farb Modus deaktiviert, sodass die für eine Ansicht des Benutzers festgelegten Farben auch dann beibehalten werden, wenn die Ansicht deaktiviert ist. Sie wird in XAML verwendet, indem die [`VisualElement.IsLegacyColorModeEnabled`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.IsLegacyColorModeEnabledProperty) angefügte-Eigenschaft auf festgelegt wird `false` :

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

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

_legacyColorModeDisabledButton.On<Android>().SetIsLegacyColorModeEnabled(false);
```

Die- `VisualElement.On<Android>` Methode gibt an, dass diese plattformspezifische nur unter Android ausgeführt wird. [ `VisualElement.SetIsLegacyColorModeEnabled` ] (Xref: Xamarin.Forms . Platformconfiguration. androidspecific. visualelement. * tislegacycolormodeaktivierte ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. Android, Xamarin.Forms . Visualelement}, System. Boolean))-Methode im- [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) Namespace wird verwendet, um zu steuern, ob der Legacy Farb Modus deaktiviert ist. Außerdem ist [ `VisualElement.GetIsLegacyColorModeEnabled` ] (Xref: Xamarin.Forms . Platformconfiguration. androidspecific. visualelement. getislegacycolormodeaktivierte ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. Android, Xamarin.Forms . Visualelement}))-Methode kann verwendet werden, um zurückzugeben, ob der Legacy Farb Modus deaktiviert ist.

Das Ergebnis ist, dass der Legacy Farb Modus deaktiviert werden kann, sodass die Farben, die für eine Ansicht durch den Benutzer festgelegt sind, auch dann beibehalten werden, wenn die Ansicht deaktiviert ist:

![Legacy Farb Modus deaktiviert](legacy-color-mode-images/legacy-color-mode-disabled.png)

> [!NOTE]
> Wenn Sie [`VisualStateGroup`](xref:Xamarin.Forms.VisualStateGroup) für eine Ansicht festlegen, wird der Legacy Farb Modus vollständig ignoriert. Weitere Informationen zu visuellen Zuständen finden Sie [unter Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Androidspecific-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [Androidspecific. AppCompat-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
