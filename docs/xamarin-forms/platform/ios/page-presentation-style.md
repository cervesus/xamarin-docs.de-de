---
title: Stil der modalen Seitendarstellung unter IOS
description: Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie die plattformspezifische IOS-Anwendung verwenden, die den Präsentationsstil einer modalen Seite festlegt.
ms.prod: xamarin
ms.assetid: C791F7CF-330A-44BA-987A-4CFCCBB9278B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/02/2020
ms.openlocfilehash: 19175a6d97afb9d2d985d8a1bf8e1ce4c0179242
ms.sourcegitcommit: 87035b654434c22ca1b0d70ee8d2496dcb63b365
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/18/2020
ms.locfileid: "83546505"
---
# <a name="modal-page-presentation-style-on-ios"></a>Stil der modalen Seitendarstellung unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese IOS-plattformspezifische wird verwendet, um den Präsentationsstil einer modalen Seite festzulegen, und kann zusätzlich verwendet werden, um modale Seiten mit transparenten Hintergründen anzuzeigen. Sie wird in XAML verwendet, indem die `Page.ModalPresentationStyle` bindbare Eigenschaft auf einen- `UIModalPresentationStyle` Enumerationswert festgelegt wird:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.ModalPresentationStyle="OverFullScreen">
    ...
</ContentPage>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

public class iOSModalFormSheetPageCS : ContentPage
{
    public iOSModalFormSheetPageCS()
    {
        On<iOS>().SetModalPresentationStyle(UIModalPresentationStyle.OverFullScreen);
        ...
    }
}
```

Die- `Page.On<iOS>` Methode gibt an, dass diese plattformspezifische nur unter IOS ausgeführt wird. Die- `Page.SetModalPresentationStyle` Methode im- [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) Namespace wird verwendet, um den modalen Präsentationsstil für einen festzulegen, [`Page`](xref:Xamarin.Forms.Page) indem Sie einen der folgenden `UIModalPresentationStyle` Enumerationswerte angeben:

- `FullScreen`, wodurch der modale Präsentationsstil auf den gesamten Bildschirm festgelegt wird. Standardmäßig werden modale Seiten mithilfe dieses Präsentations Stils angezeigt.
- `FormSheet`, mit dem der modale Präsentationsstil festgelegt wird, der auf den Bildschirm ausgerichtet und kleiner ist.
- `Automatic`, die den modalen Präsentationsstil auf den vom System gewählten Standardwert festlegt. Bei den meisten Ansichts Controllern wird `UIKit` dies von zugeordnet, `UIModalPresentationStyle.PageSheet` aber einige System Ansichts Controller ordnen Sie möglicherweise einem anderen Format zu.
- `OverFullScreen`, wodurch der modale Präsentationsstil auf den Bildschirm festgelegt wird.
- `PageSheet`, wodurch der modale Präsentationsstil auf den zugrunde liegenden Inhalt festgelegt wird.

Außerdem kann die- `GetModalPresentationStyle` Methode zum Abrufen des aktuellen Werts der-Enumeration verwendet werden, die `UIModalPresentationStyle` auf den angewendet wird [`Page`](xref:Xamarin.Forms.Page) .

Das Ergebnis ist, dass der modale Präsentationsstil in einem [`Page`](xref:Xamarin.Forms.Page) festgelegt werden kann:

[![](page-presentation-style-images/modal-presentation-style-small.png "Modal Presentation Styles")](page-presentation-style-images/modal-presentation-style-large.png#lightbox "Modal Presentation Styles")

> [!NOTE]
> Für Seiten, die diese plattformspezifische zum Festlegen des modalen Präsentations Stils verwenden, muss die modale Navigation verwendet werden. Weitere Informationen finden Sie unter [xamarin. Forms Modal Pages](~/xamarin-forms/app-fundamentals/navigation/modal.md).

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
