---
Title: "Beschreibung der Hauptthread-Steuerelemente unter IOS" Description: "Platform-Besonderheiten ermöglicht Ihnen die Nutzung von Funktionen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie die plattformspezifische IOS-Anwendung nutzen, mit der Steuerelement Layout und Renderingupdates im Haupt Thread ausgeführt werden können. "
ms. Prod: xamarin ms. assetid: 945e711d-9bd2-4BF 9-9bb3-cbe0d5b25a49 ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 10/24/2018 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="main-thread-control-updates-on-ios"></a>Haupt-Thread Steuerungs Updates unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese IOS-plattformspezifische ermöglicht die Ausführung von Steuerelement Layout und Renderingupdates im Haupt Thread, anstatt in einem Hintergrund Thread ausgeführt zu werden. Er sollte nur selten benötigt werden, aber in einigen Fällen können Abstürze verhindert werden. Die in XAML verbraucht ist, indem die `Application.HandleControlUpdatesOnMainThread` bindbare Eigenschaft auf festgelegt wird `true` :

```xaml
<Application ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Application.HandleControlUpdatesOnMainThread="true">
    ...
</Application>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Xamarin.Forms.Application.Current.On<iOS>().SetHandleControlUpdatesOnMainThread(true);
```

Die- `Application.On<iOS>` Methode gibt an, dass diese plattformspezifische nur unter IOS ausgeführt wird. Die `Application.SetHandleControlUpdatesOnMainThread` -Methode im- [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) Namespace wird verwendet, um zu steuern, ob Steuerelement Layout-und renderingaktualisierungen im Haupt Thread ausgeführt werden, anstatt in einem Hintergrund Thread ausgeführt zu werden. Außerdem `Application.GetHandleControlUpdatesOnMainThread` kann die-Methode verwendet werden, um zurückzugeben, ob Steuerelement Layout-und renderingaktualisierungen im Haupt Thread ausgeführt werden.

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
