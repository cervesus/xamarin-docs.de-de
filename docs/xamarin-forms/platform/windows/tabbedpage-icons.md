---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: f6db5014050ad3f037869120d017e51803a7c48f
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136538"
---
# <a name="tabbedpage-icons-on-windows"></a>Tabbedpage-Symbole unter Windows

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese universelle Windows-Plattform plattformspezifisch ermöglicht, dass Seiten Symbole auf einer [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) Symbolleiste angezeigt werden, und bietet die Möglichkeit, optional die Symbolgröße anzugeben. Sie wird in XAML verwendet, indem die [`TabbedPage.HeaderIconsEnabled`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.HeaderIconsEnabledProperty) angefügte-Eigenschaft auf festgelegt `true` wird, und indem optional die [`TabbedPage.HeaderIconsSize`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.HeaderIconsSizeProperty) angefügte-Eigenschaft auf einen Wert festgelegt wird [`Size`](xref:Xamarin.Forms.Size) :

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
            windows:TabbedPage.HeaderIconsEnabled="true">
    <windows:TabbedPage.HeaderIconsSize>
        <Size>
            <x:Arguments>
                <x:Double>24</x:Double>
                <x:Double>24</x:Double>
            </x:Arguments>
        </Size>
    </windows:TabbedPage.HeaderIconsSize>
    <ContentPage Title="Todo" IconImageSource="todo.png">
        ...
    </ContentPage>
    <ContentPage Title="Reminders" IconImageSource="reminders.png">
        ...
    </ContentPage>
    <ContentPage Title="Contacts" IconImageSource="contacts.png">
        ...
    </ContentPage>
</TabbedPage>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

public class WindowsTabbedPageIconsCS : Xamarin.Forms.TabbedPage
{
  public WindowsTabbedPageIconsCS()
  {
    On<Windows>().SetHeaderIconsEnabled(true);
    On<Windows>().SetHeaderIconsSize(new Size(24, 24));

    Children.Add(new ContentPage { Title = "Todo", IconImageSource = "todo.png" });
    Children.Add(new ContentPage { Title = "Reminders", IconImageSource = "reminders.png" });
    Children.Add(new ContentPage { Title = "Contacts", IconImageSource = "contacts.png" });
  }
}
```

Die- `TabbedPage.On<Windows>` Methode gibt an, dass diese plattformspezifische nur auf der universelle Windows-Plattform ausgeführt wird. [ `TabbedPage.SetHeaderIconsEnabled` ] (Xref: Xamarin.Forms . Platformconfiguration. windowsspecific. tabbedpage. *-adericonsenabled ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. Windows, Xamarin.Forms . Tabbedpage}, System. Boolean))-Methode im- [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) Namespace wird verwendet, um Header Symbole ein-oder auszuschalten. [ `TabbedPage.SetHeaderIconsSize` ] (Xref: Xamarin.Forms . Platformconfiguration. windowsspecific. tabbedpage. Xamarin.Forms ". Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. Windows, Xamarin.Forms . Tabbedpage}, Xamarin.Forms . Size))-Methode gibt optional die Header Symbolgröße mit einem [`Size`](xref:Xamarin.Forms.Size) Wert an.

Außerdem verfügt die- `TabbedPage` Klasse im- `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` Namespace auch über eine- [`EnableHeaderIcons`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.EnableHeaderIcons*) Methode, die Header Symbole aktiviert, eine [`DisableHeaderIcons`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.DisableHeaderIcons*) Methode, die Header Symbole deaktiviert, und eine [`IsHeaderIconsEnabled`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.IsHeaderIconsEnabled*) Methode, die einen Wert zurückgibt, `boolean` der angibt, ob Header Symbole aktiviert sind.

Das Ergebnis ist, dass Seiten Symbole auf einer Symbolleiste angezeigt werden können [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) , wobei die Symbolgröße optional auf eine gewünschte Größe festgelegt wird:

![Tabbedpage-Symbole sind plattformspezifisch aktiviert](tabbedpage-icons-images/tabbedpage-icons.png "Tabbedpage-Symbole sind plattformspezifisch aktiviert")

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Windowsspecific-API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
