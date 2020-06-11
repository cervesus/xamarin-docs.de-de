---
Title: "Eingabe Schriftart Größe unter IOS" Description: "Platform-Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne benutzerdefinierte Renderer oder Effekte implementieren zu müssen. In diesem Artikel wird erläutert, wie Sie die plattformspezifische IOS-Anwendung nutzen, die den Schrift Grad eines Eintrags skaliert.
ms. Prod: xamarin ms. assetid: E8881D4E-902b-4397-A43E-916b2885ec87 ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 10/24/2018 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="entry-font-size-on-ios"></a>Eingabe Schriftgröße unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese plattformspezifische IOS-Datei wird verwendet, um den Schrift Grad eines zu skalieren [`Entry`](xref:Xamarin.Forms.Entry) , um sicherzustellen, dass der eingeputzte Text in das Steuerelement passt. Sie wird in XAML verwendet, indem die [`Entry.AdjustsFontSizeToFitWidth`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.AdjustsFontSizeToFitWidthProperty) angefügte-Eigenschaft auf einen Wert festgelegt wird `boolean` :

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
    <StackLayout Margin="20">
        <Entry x:Name="entry"
               Placeholder="Enter text here to see the font size change"
               FontSize="22"
               ios:Entry.AdjustsFontSizeToFitWidth="true" />
        ...
    </StackLayout>
</ContentPage>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

entry.On<iOS>().EnableAdjustsFontSizeToFitWidth();
```

Die- `Entry.On<iOS>` Methode gibt an, dass diese plattformspezifische nur unter IOS ausgeführt wird. [ `Entry.EnableAdjustsFontSizeToFitWidth` ] (Xref: Xamarin.Forms . Platformconfiguration. iosspecific. entry. enableder sfontsizedeftwidth ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. IOS, Xamarin.Forms . Entry}))-Methode im- [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) Namespace wird verwendet, um den Schrift Grad des eingegebenen Texts zu skalieren, um sicherzustellen, dass er in passt [`Entry`](xref:Xamarin.Forms.Entry) . Außerdem verfügt die- [`Entry`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry) Klasse im- `Xamarin.Forms.PlatformConfiguration.iOSSpecific` Namespace ebenfalls über ein [ `DisableAdjustsFontSizeToFitWidth` ] (Xref:) Xamarin.Forms . Platformconfiguration. iosspecific. entry. disableder sfontsizedeftwidth ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. IOS, Xamarin.Forms . Entry}))-Methode, die diese plattformspezifische und eine [ `SetAdjustsFontSizeToFitWidth` ] (Xref:) deaktiviert Xamarin.Forms . Platformconfiguration. iosspecific. entry. setanpassungen sfontsizedeftwidth ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. IOS, Xamarin.Forms . Entry}, System. Boolean)-Methode, die verwendet werden kann, um die Skalierung der Schriftgröße durch Aufrufen von [ `AdjustsFontSizeToFitWidth` ] (Xref:) zu umschalten Xamarin.Forms . Platformconfiguration. iosspecific. entry. ". Xamarin.Forms ". Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. IOS, Xamarin.Forms . Entry}))-Methode:

```csharp
entry.On<iOS>().SetAdjustsFontSizeToFitWidth(!entry.On<iOS>().AdjustsFontSizeToFitWidth());
```

Das Ergebnis ist, dass der Schrift Grad der [`Entry`](xref:Xamarin.Forms.Entry) skaliert wird, um sicherzustellen, dass der eingeputzte Text in das Steuerelement passt:

![](entry-font-size-images/entry-font-size.png "Adjust Entry Font Size Platform-Specific")

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
