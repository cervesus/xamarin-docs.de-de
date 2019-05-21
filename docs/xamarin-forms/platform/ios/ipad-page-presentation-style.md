---
title: 'iPad: Präsentationsformat für modale Seiten'
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie die iOS-Plattform-spezifische legt den Presentation-Stil, der eine modale Seite auf einem iPad genutzt wird.
ms.prod: xamarin
ms.assetid: C791F7CF-330A-44BA-987A-4CFCCBB9278B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: fa851ddc753d1fb9cb39f4c08dcfde518a123b62
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2019
ms.locfileid: "65926740"
---
# <a name="ipad-modal-page-presentation-style"></a>iPad: Präsentationsformat für modale Seiten

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)

Dieses iOS-Plattform-spezifische wird verwendet, die Präsentationsstil, der eine modale Seite auf einem iPad festlegen. Es ist in XAML verwendet, durch Festlegen der `Page.ModalPresentationStyle` bindbare Eigenschaft eine `UIModalPresentationStyle` Enumerationswert:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.ModalPresentationStyle="FormSheet">
    ...
</ContentPage>
```

Alternativ können sie aus C# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

public class iOSModalFormSheetPageCS : ContentPage
{
    public iOSModalFormSheetPageCS()
    {
        On<iOS>().SetModalPresentationStyle(UIModalPresentationStyle.FormSheet);
        ...
    }
}
```

Die `Page.On<iOS>` Methode gibt an, dass diese plattformspezifischen nur unter iOS ausgeführt wird. Die `Page.SetModalPresentationStyle` Methode in der [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) -Namespace wird verwendet, um modale Präsentation an Festlegen einer [ `Page` ](xref:Xamarin.Forms.Page) durch Angabe eines der folgenden `UIModalPresentationStyle` Enumeration Werte:

- `FullScreen`, wodurch die modale Presentation-Formatvorlage, die den gesamten Bildschirm einnimmt. Standardmäßig werden modale Seiten mit dieser Präsentationsstil angezeigt.
- `FormSheet`, wodurch den modalen Presentation-Stil auf Zentriert und kleiner als der Bildschirm ist festgelegt.

Darüber hinaus die `GetModalPresentationStyle` Methode kann verwendet werden, um das Abrufen des aktuellen Werts der `UIModalPresentationStyle` Enumeration, die auf die [ `Page` ](xref:Xamarin.Forms.Page).

Das Ergebnis ist, die das Format der modale Präsentation auf eine [ `Page` ](xref:Xamarin.Forms.Page) kann festgelegt werden:

[![](page-presentation-style-images/modal-presentation-style-small.png "Modale Präsentationsformate auf einem iPad")](page-presentation-style-images/modal-presentation-style-large.png#lightbox "modale Präsentationsformate auf einem iPad")

> [!NOTE]
> Seiten, die diese plattformspezifischen zu verwenden, um den modalen Präsentationsstil festzulegen, müssen modalen Navigation verwenden. Weitere Informationen finden Sie unter [Xamarin.Forms modale Seiten](~/xamarin-forms/app-fundamentals/navigation/modal.md).

## <a name="related-links"></a>Verwandte Links

- [PlatformSpecifics (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
