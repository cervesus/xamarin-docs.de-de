---
Title: "ListView Separator Style on IOS" Description: "Platform-Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie die plattformspezifische IOS-Version verwenden, die steuert, ob das Trennzeichen zwischen Zellen in einer ListView die volle Breite von ListView verwendet. "
ms. Prod: xamarin ms. assetid: A4CB45CE-9FB7-47ED-8C3D-93E39BF282E4 ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 10/24/2018 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="listview-separator-style-on-ios"></a>ListView-Trennzeichen Stil unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Mit diesem IOS-plattformspezifischen Steuerelement wird gesteuert, ob das Trennzeichen zwischen den Zellen in einer [`ListView`](xref:Xamarin.Forms.ListView) die vollständige Breite des verwendet `ListView` . Sie wird in XAML verwendet, indem die [`ListView.SeparatorStyle`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.ListView.SeparatorStyleProperty) angefügte-Eigenschaft auf einen Wert der-Enumeration festgelegt wird [`SeparatorStyle`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) :

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ... ios:ListView.SeparatorStyle="FullWidth">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

listView.On<iOS>().SetSeparatorStyle(SeparatorStyle.FullWidth);
```

Die- `ListView.On<iOS>` Methode gibt an, dass diese plattformspezifische nur unter IOS ausgeführt wird. [ `ListView.SetSeparatorStyle` ] (Xref: Xamarin.Forms . Platformconfiguration. iosspecific. ListView. setseparatorstyle ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. IOS, Xamarin.Forms . ListView}, Xamarin.Forms . Platformconfiguration. iosspecific. SeparatorStyle))-Methode im- [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) Namespace wird verwendet, um zu steuern, ob das Trennzeichen zwischen den Zellen in die [`ListView`](xref:Xamarin.Forms.ListView) vollständige Breite des verwendet `ListView` , wobei die- [`SeparatorStyle`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) Enumeration zwei mögliche Werte bereitstellt:

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle.Default)– Gibt das Standardverhalten für IOS-Trennzeichen an. Dies ist das Standardverhalten in Xamarin.Forms .
- [`FullWidth`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle.FullWidth)– Gibt an, dass Trennzeichen von einem Rand der `ListView` zum anderen gezeichnet werden.

Das Ergebnis ist, dass ein [`SeparatorStyle`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) angegebener Wert auf den angewendet wird [`ListView`](xref:Xamarin.Forms.ListView) , der die Breite des Trenn Zeichens zwischen Zellen steuert:

![](listview-separator-style-images/listview-separatorstyle.png "ListView SeparatorStyle Platform-Specific")

> [!NOTE]
> Nachdem der Trenn Zeichenstil auf festgelegt wurde `FullWidth` , kann er nicht zur Laufzeit wieder in geändert werden `Default` .

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
