---
Title: "visualelement First Beantworter on IOS" Description: "Platform-Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne benutzerdefinierte Renderer oder Effekte implementieren zu müssen. In diesem Artikel wird erläutert, wie die plattformspezifische IOS-Anwendung verwendet wird, die es ermöglicht, dass ein visualelement-Objekt zum ersten Response-Ereignis wird. "
ms. Prod: xamarin ms. assetid: 3a77ba02-b87a-44ec-AC51-9d3130ef314 c ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 01/15/2020 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="visualelement-first-responder-on-ios"></a>Visualelement First Response on IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Mit diesem IOS-plattformspezifischen ist es möglich, dass ein- [`VisualElement`](xref:Xamarin.Forms.VisualElement) Objekt zum ersten Response-Ereignis wird, anstatt die Seite, die das Element enthält. Sie wird in XAML verwendet, indem die `VisualElement.CanBecomeFirstResponder` bindbare Eigenschaft auf festgelegt wird `true` :

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <Entry Placeholder="Enter text" />
        <Button ios:VisualElement.CanBecomeFirstResponder="True"
                Text="OK" />
    </StackLayout>
</ContentPage>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Entry entry = new Entry { Placeholder = "Enter text" };
Button button = new Button { Text = "OK" };
button.On<iOS>().SetCanBecomeFirstResponder(true);
```

Die- `VisualElement.On<iOS>` Methode gibt an, dass diese plattformspezifische nur unter IOS ausgeführt wird. Die- `VisualElement.SetCanBecomeFirstResponder` Methode im- [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) Namespace wird verwendet, um den so festzulegen, `VisualElement` dass er der erste Beantworter für Berührungs Ereignisse wird. Außerdem kann die- `VisualElement.CanBecomeFirstResponder` Methode verwendet werden, um zurückzugeben, ob das `VisualElement` der erste Antwort für das Berühren von Ereignissen ist.

Das Ergebnis ist, dass ein als [`VisualElement`](xref:Xamarin.Forms.VisualElement) erster Beantworter für Berührungs Ereignisse und nicht für die Seite, die das Element enthält, werden kann. Dadurch können Szenarien wie Chat Anwendungen eine Tastatur nicht verwerfen, wenn ein [`Button`](xref:Xamarin.Forms.Button) getippt wird.

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
