---
Title: "visualelement Legacy Color Mode on Android" Description: "Platform-Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne benutzerdefinierte Renderer oder Effekte implementieren zu müssen. In diesem Artikel wird erläutert, wie Sie die plattformspezifische Android-Version verwenden, mit der der Xamarin.Forms Legacy Farb Modus deaktiviert wird. "
ms. Prod: xamarin ms. assetid: 37d95a2d-74AC-488a-B903-2bdd799eaa5c ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 07/10/2018 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
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

![](legacy-color-mode-images/legacy-color-mode-disabled.png "Legacy color mode disabled")

> [!NOTE]
> Wenn Sie [`VisualStateGroup`](xref:Xamarin.Forms.VisualStateGroup) für eine Ansicht festlegen, wird der Legacy Farb Modus vollständig ignoriert. Weitere Informationen zu visuellen Zuständen finden Sie [unter Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Androidspecific-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [Androidspecific. AppCompat-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
