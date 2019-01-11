---
title: VisualElement Zugriffstasten für Windows
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie die Windows-Plattform-spezifische genutzt, die eine Zugriffstaste für eine VisualElement angibt.
ms.prod: xamarin
ms.assetid: 771AF785-76B8-4372-89F5-E4D521D21E0C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: f14014b257ee5061b6dd074719c3ca27577c6013
ms.sourcegitcommit: 395774577f7524b57035c5cca3c9034a4b636489
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/10/2019
ms.locfileid: "54209335"
---
# <a name="visualelement-access-keys-on-windows"></a>VisualElement Zugriffstasten für Windows

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

Zugriffsschlüssel werden Tastenkombinationen aufgeführt, die Verbesserung der gebrauchstauglichkeit sowie der Barrierefreiheit von apps für die universelle Windows-Plattform (UWP) durch die Bereitstellung einer intuitiven Methode für Benutzer schnell navigieren und interagieren mit der Benutzeroberfläche der Anwendung sichtbar über eine Tastatur anstelle von per touch oder eine Maus. Sie sind Kombinationen aus die Alt-Taste und einen oder mehrere alphanumerische Schlüssel, die in der Regel nacheinander aufgerufen. Tastenkombinationen in Visual Studio werden automatisch für Zugriffsschlüssel unterstützt, die ein einzelnes alphanumerisches Zeichen verwenden.

Wichtige Tipps für den Zugriff schwimmen Badges, die neben der Steuerelemente, die Zugriffsschlüssel enthalten. Jeder Zugriff Zugriffstasteninfos enthält die alphanumerische Schlüssel, die das zugeordnete Steuerelement zu aktivieren. Wenn ein Benutzer die Alt-Taste drückt, werden die Zugriffstasteninfos Zugriff angezeigt.

Diese plattformspezifischen UWP wird verwendet, um Geben Sie eine Zugriffstaste für eine [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Es ist in XAML verwendet, durch Festlegen der [ `VisualElement.AccessKey` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyProperty) angefügten Eigenschaft, um einen alphanumerischen Wert, und legen optional die [ `VisualElement.AccessKeyPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyPlacementProperty) angefügte Eigenschaft auf den Wert der [ `AccessKeyPlacement` ](xref:Xamarin.Forms.AccessKeyPlacement) Enumeration, die [ `VisualElement.AccessKeyHorizontalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyHorizontalOffsetProperty) angefügten Eigenschaft, um eine `double`, und die [ `VisualElement.AccessKeyVerticalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyVerticalOffsetProperty) angefügte Eigenschaft auf einen `double`:

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

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

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

Die `VisualElement.On<Windows>` Methode gibt an, dass diese plattformspezifischen nur für die universelle Windows-Plattform ausgeführt wird. Die [ `VisualElement.SetAccessKey` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetAccessKey(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.String)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) -Namespace wird verwendet, um den Wert des Zugriffsschlüssels für Festlegen der `VisualElement`. Die [ `VisualElement.SetAccessKeyPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetAccessKeyPlacement(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},Xamarin.Forms.AccessKeyPlacement)) -Methode, gibt optional die Position zum Anzeigen der Zugriffstasteninfos Zugriff, mit der [ `AccessKeyPlacement` ](xref:Xamarin.Forms.AccessKeyPlacement) Enumeration, die Bereitstellung von der folgenden möglichen Werten:

- [`Auto`](xref:Xamarin.Forms.AccessKeyPlacement.Auto) – Gibt an, dass die Platzierung der Zugriffstasteninfos vom Betriebssystem bestimmt wird.
- [`Top`](xref:Xamarin.Forms.AccessKeyPlacement.Top) – Gibt an, dass die Zugriffstasteninfos für den Zugriff über dem oberen Rand des angezeigt werden, wird die `VisualElement`.
- [`Bottom`](xref:Xamarin.Forms.AccessKeyPlacement.Bottom) – Gibt an, dass der Zugriff Zugriffstasteninfos unter am unteren Rand des angezeigt wird der `VisualElement`.
- [`Right`](xref:Xamarin.Forms.AccessKeyPlacement.Right) – Gibt an, dass der Zugriff Zugriffstasteninfos rechts neben dem rechten Rand angezeigt wird der `VisualElement`.
- [`Left`](xref:Xamarin.Forms.AccessKeyPlacement.Left) – Gibt an, dass die Zugriffstasteninfos für den Zugriff auf der linken Seite des linken Rands des erscheint die `VisualElement`.
- [`Center`](xref:Xamarin.Forms.AccessKeyPlacement.Center) – Gibt an, dass die Zugriffstasteninfos für den Zugriff auf den Mittelpunkt der überlagerten angezeigt wird der `VisualElement`.

> [!NOTE]
> In der Regel die [ `Auto` ](xref:Xamarin.Forms.AccessKeyPlacement.Auto) Zugriffstasteninfos Platzierung ist ausreichend, wozu die Unterstützung für adaptive Benutzeroberflächen.

Die [ `VisualElement.SetAccessKeyHorizontalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetAccessKeyHorizontalOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.Double)) und [ `VisualElement.SetAccessKeyVerticalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetAccessKeyVerticalOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.Double)) Methoden können für eine präzisere Steuerung des Speicherorts Zugriffstasteninfos Zugriff verwendet werden. Das Argument für die `SetAccessKeyHorizontalOffset` -Methode gibt an, wie weit nach links Zugriffstasteninfos Zugriff zu verschieben oder rechts, und das Argument für die `SetAccessKeyVerticalOffset` Methode gibt an, wie weit die Zugriffstasteninfos für den Zugriff nach oben oder unten verschieben.

>[!NOTE]
> Zugriff Zugriffstasteninfos Offsets können nicht festgelegt werden, wenn die Platzierung der Key festgelegt ist `Auto`.

Darüber hinaus die [ `GetAccessKey` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetAccessKey(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})), [ `GetAccessKeyPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetAccessKeyPlacement(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})), [ `GetAccessKeyHorizontalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetAccessKeyHorizontalOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})), und [ `GetAccessKeyVerticalOffset` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetAccessKeyVerticalOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})) Methoden können verwendet werden zum Abrufen eines Wert und den Speicherort des Schlüssels.

Das Ergebnis ist, wichtige Tipps für den Zugriff können, neben einer beliebigen angezeigt werden [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) Instanzen, die definieren, Zugriffsschlüssel, durch Drücken der Alt-Taste:

![VisualElement-Zugriffsschlüsseln plattformspezifische](visualelement-access-keys-images/visualelement-accesskeys.png "VisualElement-Zugriffsschlüsseln plattformspezifische")

Wenn ein Benutzer einen Zugriffsschlüssel aktiviert, durch Drücken der Alt-Taste gefolgt von den Zugriff Schlüssel ist, wird die Standardaktion für die `VisualElement` ausgeführt wird. Z. B. wenn ein Benutzer aktiviert den Zugriffsschlüssel für ein [ `Switch` ](xref:Xamarin.Forms.Switch), `Switch` umgeschaltet wird. Wenn ein Benutzer die Zugriffstaste aktiviert, auf eine [ `Entry` ](xref:Xamarin.Forms.Entry), `Entry` den Fokus erhält. Wenn ein Benutzer die Zugriffstaste aktiviert, auf eine [ `Button` ](xref:Xamarin.Forms.Button), den Ereignishandler für die [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) -Ereignis immer ausgeführt wird.

Weitere Informationen zu Zugriffsschlüsseln finden Sie unter [Zugriffsschlüssel](/windows/uwp/design/input/access-keys#key-tip-positioning).

## <a name="related-links"></a>Verwandte Links

- [PlatformSpecifics (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [WindowsSpecific API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
