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
ms.openlocfilehash: aaf94536d41da47aec10fc655f9d053b753da5a2
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84135966"
---
# <a name="masterdetailpage-shadow-on-ios"></a>Masterdetailpage Schatten unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese plattformspezifische steuert, ob die Detailseite eines auf die Detailseite [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) angewendet wird, wenn die Master Seite offengelegt wird. Sie wird in XAML verwendet, indem die `MasterDetailPage.ApplyShadow` bindbare Eigenschaft auf festgelegt wird `true` :

```xaml
<MasterDetailPage ...
                  xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                  ios:MasterDetailPage.ApplyShadow="true">
    ...
</MasterDetailPage>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

public class iOSMasterDetailPageCS : MasterDetailPage
{
    public iOSMasterDetailPageCS(ICommand restore)
    {
        On<iOS>().SetApplyShadow(true);
        // ...
    }
}
```

Die- `MasterDetailPage.On<iOS>` Methode gibt an, dass diese plattformspezifische nur unter IOS ausgeführt wird. Die- `MasterDetailPage.SetApplyShadow` Methode im- [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) Namespace wird verwendet, um zu steuern, ob auf die Detailseite eines [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) auf den Schatten angewendet wird, wenn die Master Seite offengelegt wird. Außerdem kann die- `GetApplyShadow` Methode verwendet werden, um zu bestimmen, ob Schatten auf die Detailseite einer angewendet wird `MasterDetailPage` .

Das Ergebnis ist, dass auf der Detailseite eines auf die Seite " [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) Schatten" angewendet werden kann, wenn die Master Seite angezeigt wird:

[![Screenshot einer masterdetailpage mit und ohne Schatten](masterdetailpage-shadow-images/shadow.png "Masterdetailpage mit und ohne Schatten")](masterdetailpage-shadow-images/shadow-large.png#lightbox "Masterdetailpage mit und ohne Schatten")

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
