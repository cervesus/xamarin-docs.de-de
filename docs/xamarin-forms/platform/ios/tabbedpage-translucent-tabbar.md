---
Title: "tabbedpage-Registerkarten Leiste unter IOS" Description: "Platform-Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne benutzerdefinierte Renderer oder Effekte implementieren zu müssen. In diesem Artikel wird erläutert, wie Sie die plattformspezifische IOS-Anwendung nutzen, die den Transaktionsmodus der Registerkarten Leiste auf einer tabbedpage festlegt.
ms. Prod: xamarin ms. assetid: 9581ae81-9557-47ad-8b07-25a1ac5f 055b ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 01/16/2020 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="tabbedpage-translucent-tab-bar-on-ios"></a>Tabbedpage-Schiebe leisten-Registerkarten Leiste unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese IOS-plattformspezifische wird verwendet, um den Transaktionsmodus der Registerkarten Leiste eines festzulegen [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) . Sie wird in XAML verwendet, indem die `TabbedPage.TranslucencyMode` bindbare Eigenschaft auf einen- `TranslucencyMode` Enumerationswert festgelegt wird:

```xaml
<TabbedPage ...
            xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
            ios:TabbedPage.TranslucencyMode="Opaque">
    ...
</TabbedPage>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetTranslucencyMode(TranslucencyMode.Opaque);
```

Die- `TabbedPage.On<iOS>` Methode gibt an, dass diese plattformspezifische nur unter IOS ausgeführt wird. Die- `TabbedPage.SetTranslucencyMode` Methode im- [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) Namespace wird verwendet, um den Transaktionsmodus der Registerkarten Leiste eines festzulegen, [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) indem Sie einen der folgenden `TranslucencyMode` Enumerationswerte angeben:

- `Default`, wodurch die Registerkarten Leiste auf ihren standardmäßigen Transaktionsmodus festgelegt wird. Dies ist der Standardwert der `TabbedPage.TranslucencyMode`-Eigenschaft.
- `Translucent`, bei dem die Registerkarten Leiste als durchscheinend festgelegt wird.
- `Opaque`, wodurch die Registerkarten Leiste als nicht transparent festgelegt wird.

Außerdem kann die- `GetTranslucencyMode` Methode zum Abrufen des aktuellen Werts der-Enumeration verwendet werden, die `TranslucencyMode` auf den angewendet wird [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) .

Das Ergebnis ist, dass der Transaktionsmodus der Registerkarten Leiste eines [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) festgelegt werden kann:

![Screenshot der durchlässigen und transparenten Registerkarten leisten unter IOS](tabbedpage-translucent-tabbar-images/translucencymodes.png "Durchlässiges und undurchsichtiges Tabstopp")

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
