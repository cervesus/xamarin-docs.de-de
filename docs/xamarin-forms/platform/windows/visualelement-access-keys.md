---
title: Visualelement-Zugriffsschlüssel unter Windows
description: Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie die Windows-plattformspezifische verwenden, die einen Zugriffsschlüssel für ein visualelement angibt.
ms.prod: xamarin
ms.assetid: 771AF785-76B8-4372-89F5-E4D521D21E0C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 1bfd61e79a2b4697e884afb45e4b9080ee939b87
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136525"
---
# <a name="visualelement-access-keys-on-windows"></a>Visualelement-Zugriffsschlüssel unter Windows

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Zugriffstasten sind Tastenkombinationen, die die Benutzerfreundlichkeit und Barrierefreiheit von apps auf dem universelle Windows-Plattform (UWP) verbessern, indem Sie Benutzern eine intuitive Möglichkeit bieten, schnell zu navigieren und mit der sichtbaren Benutzeroberfläche der APP über eine Tastatur anstelle von Touchscreen oder Maus zu interagieren. Dabei handelt es sich um Kombinationen aus der Alt-Taste und einem oder mehreren alphanumerischen Schlüsseln, die in der Regel nacheinander gedrückt werden. Tastenkombinationen werden automatisch für Zugriffsschlüssel unterstützt, die ein einzelnes alphanumerisches Zeichen verwenden.

Zugriffsschlüssel Tipps sind Gleit Komma, die neben Steuerelementen angezeigt werden, die Zugriffstasten enthalten. Jeder Zugriffsschlüssel Tipp enthält die alphanumerischen Schlüssel zum Aktivieren des zugeordneten Steuer Elements. Wenn ein Benutzer die Alt-Taste drückt, werden die Zugriffstasten Tipps angezeigt.

Diese UWP-plattformspezifische wird verwendet, um einen Zugriffsschlüssel für einen anzugeben [`VisualElement`](xref:Xamarin.Forms.VisualElement) . Sie wird in XAML verwendet, indem die [`VisualElement.AccessKey`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyProperty) angefügte-Eigenschaft auf einen alphanumerischen Wert festgelegt wird, und indem optional die [`VisualElement.AccessKeyPlacement`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyPlacementProperty) angefügte-Eigenschaft auf einen Wert der- [`AccessKeyPlacement`](xref:Xamarin.Forms.AccessKeyPlacement) Enumeration, die [`VisualElement.AccessKeyHorizontalOffset`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyHorizontalOffsetProperty) angefügte-Eigenschaft auf einen `double` und die [`VisualElement.AccessKeyVerticalOffset`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyVerticalOffsetProperty) angefügte-Eigenschaft `double` auf festgelegt wird:

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <ContentPage Title="Page 1"
                 windows:VisualElement.AccessKey="1">
        <StackLayout Margin="20">
            ...
            <Switch windows:VisualElement.AccessKey="A" />
            <Entry Placeholder="Enter text here"
                   windows:VisualElement.AccessKey="B" />
            ...
            <Button Text="Access key F, placement top with offsets"
                    Margin="20"
                    Clicked="OnButtonClicked"
                    windows:VisualElement.AccessKey="F"
                    windows:VisualElement.AccessKeyPlacement="Top"
                    windows:VisualElement.AccessKeyHorizontalOffset="20"
                    windows:VisualElement.AccessKeyVerticalOffset="20" />
            ...
        </StackLayout>
    </ContentPage>
    ...
</TabbedPage>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

var page = new ContentPage { Title = "Page 1" };
page.On<Windows>().SetAccessKey("1");

var switchView = new Switch();
switchView.On<Windows>().SetAccessKey("A");
var entry = new Entry { Placeholder = "Enter text here" };
entry.On<Windows>().SetAccessKey("B");
...

var button4 = new Button { Text = "Access key F, placement top with offsets", Margin = new Thickness(20) };
button4.Clicked += OnButtonClicked;
button4.On<Windows>()
    .SetAccessKey("F")
    .SetAccessKeyPlacement(AccessKeyPlacement.Top)
    .SetAccessKeyHorizontalOffset(20)
    .SetAccessKeyVerticalOffset(20);
...
```

Die- `VisualElement.On<Windows>` Methode gibt an, dass diese plattformspezifische nur auf der universelle Windows-Plattform ausgeführt wird. [ `VisualElement.SetAccessKey` ] (Xref: Xamarin.Forms . Platformconfiguration. windowsspecific. visualelement. SetAccessKey ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. Windows, Xamarin.Forms . Die visualelement}, System. String))-Methode im- [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) Namespace wird verwendet, um den Zugriffsschlüssel Wert für das festzulegen `VisualElement` . [ `VisualElement.SetAccessKeyPlacement` ] (Xref: Xamarin.Forms . Platformconfiguration. windowsspecific. visualelement. setaccesskeyplacement ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. Windows, Xamarin.Forms . Visualelement}, Xamarin.Forms . Accesskeyplacement))-Methode, und gibt optional die Position an, die zum Anzeigen des Zugriffsschlüssel Tipps verwendet werden soll, wobei die- [`AccessKeyPlacement`](xref:Xamarin.Forms.AccessKeyPlacement) Enumeration die folgenden möglichen Werte bereitstellt:

- [`Auto`](xref:Xamarin.Forms.AccessKeyPlacement.Auto)– Gibt an, dass die Zugriffsschlüssel-Tip-Platzierung vom Betriebssystem bestimmt wird.
- [`Top`](xref:Xamarin.Forms.AccessKeyPlacement.Top)– Gibt an, dass der Zugriffsschlüssel Tipp oberhalb des oberen Rands von angezeigt wird `VisualElement` .
- [`Bottom`](xref:Xamarin.Forms.AccessKeyPlacement.Bottom)– Gibt an, dass der Zugriffsschlüssel Tipp unter dem unteren Rand von angezeigt wird `VisualElement` .
- [`Right`](xref:Xamarin.Forms.AccessKeyPlacement.Right)– Gibt an, dass der Zugriffsschlüssel Tipp rechts neben dem rechten Rand von angezeigt wird `VisualElement` .
- [`Left`](xref:Xamarin.Forms.AccessKeyPlacement.Left)– Gibt an, dass der Zugriffsschlüssel Tipp links neben dem linken Rand von angezeigt wird `VisualElement` .
- [`Center`](xref:Xamarin.Forms.AccessKeyPlacement.Center)– Gibt an, dass der Zugriffsschlüssel Tipp in der Mitte der angezeigt wird `VisualElement` .

> [!NOTE]
> In der Regel [`Auto`](xref:Xamarin.Forms.AccessKeyPlacement.Auto) reicht die Platzierung des Schlüssel Tipps aus, einschließlich der Unterstützung adaptiver Benutzeroberflächen.

[ `VisualElement.SetAccessKeyHorizontalOffset` ] (Xref: Xamarin.Forms . Platformconfiguration. windowsspecific. visualelement. setaccesskeyhorizontaloffset ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. Windows, Xamarin.Forms . Visualelement}, System. Double)) und [ `VisualElement.SetAccessKeyVerticalOffset` ] (Xref: Xamarin.Forms . Platformconfiguration. windowsspecific. visualelement. setaccesskeyverticaloffset ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. Windows, Xamarin.Forms . Die visualelement}-, System. Double)-Methoden können für eine genauere Steuerung des Trink Geld Speicher Orts des Zugriffsschlüssels verwendet werden. Das-Argument für die- `SetAccessKeyHorizontalOffset` Methode gibt an, wie weit der Zugriffsschlüssel Tipp nach links oder rechts verschoben werden soll, und das-Argument der- `SetAccessKeyVerticalOffset` Methode gibt an, wie weit der Zugriffsschlüssel Trinkgeld nach oben oder unten verschoben werden soll.

>[!NOTE]
> Zugriffsschlüssel-Tip Offsets können nicht festgelegt werden, wenn die Zugriffsschlüssel Platzierung festgelegt ist `Auto` .

Außerdem ist [ `GetAccessKey` ] (Xref: Xamarin.Forms . Platformconfiguration. windowsspecific. visualelement. GetAccessKey ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. Windows, Xamarin.Forms . Visualelement})), [ `GetAccessKeyPlacement` ] (Xref: Xamarin.Forms . Platformconfiguration. windowsspecific. visualelement. getaccesskeyplacement ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. Windows, Xamarin.Forms . Visualelement})), [ `GetAccessKeyHorizontalOffset` ] (Xref: Xamarin.Forms . Platformconfiguration. windowsspecific. visualelement. getaccesskeyhorizontaloffset ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. Windows, Xamarin.Forms . Visualelement})) und [ `GetAccessKeyVerticalOffset` ] (Xref: Xamarin.Forms . Platformconfiguration. windowsspecific. visualelement. getaccesskeyverticaloffset ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. Windows, Xamarin.Forms . Visualelement}))-Methoden können verwendet werden, um einen Zugriffsschlüssel Wert und seinen Speicherort abzurufen.

Das Ergebnis ist, dass Zugriffsschlüssel Tipps neben allen Instanzen, die [`VisualElement`](xref:Xamarin.Forms.VisualElement) Zugriffsschlüssel definieren, durch Drücken der Alt-Taste angezeigt werden können:

![Visualelement-Zugriffsschlüssel plattformspezifisch](visualelement-access-keys-images/visualelement-accesskeys.png "Visualelement-Zugriffsschlüssel plattformspezifisch")

Wenn ein Benutzer eine Zugriffstaste durch Drücken der Alt-Taste, gefolgt von der Zugriffstaste aktiviert, wird die Standardaktion für `VisualElement` ausgeführt. Wenn ein Benutzer beispielsweise den Zugriffsschlüssel auf einem aktiviert [`Switch`](xref:Xamarin.Forms.Switch) , wird das ein-und ausgeschaltet `Switch` . Wenn ein Benutzer die Zugriffstaste auf einem aktiviert [`Entry`](xref:Xamarin.Forms.Entry) , `Entry` erhält der Fokus. Wenn ein Benutzer den Zugriffsschlüssel auf einem aktiviert [`Button`](xref:Xamarin.Forms.Button) , wird der Ereignishandler für das [`Clicked`](xref:Xamarin.Forms.Button.Clicked) Ereignis ausgeführt.

Weitere Informationen zu Zugriffs Schlüsseln finden Sie unter [Zugriffsschlüssel](/windows/uwp/design/input/access-keys#key-tip-positioning).

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Windowsspecific-API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
