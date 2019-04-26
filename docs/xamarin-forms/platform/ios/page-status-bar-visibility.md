---
title: Seite Status Sichtbarkeit unter iOS
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie die plattformspezifischen iOS nutzen, die die Sichtbarkeit der Statusleiste auf einer Seite festlegt wird.
ms.prod: xamarin
ms.assetid: D8BB7C24-A27F-4758-8557-6A81F909ABD9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 98eba6dea1fb528aa15a1fb242b0fb0eb7dada56
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61258610"
---
# <a name="page-status-bar-visibility-on-ios"></a>Seite Status Sichtbarkeit unter iOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

Dieses iOS-Plattform-spezifische wird verwendet, um die Sichtbarkeit der Statusleiste für Festlegen einer [ `Page` ](xref:Xamarin.Forms.Page), sowie die Möglichkeit zum Steuern, wie die Statusleiste eintritt oder dieses verlässt die `Page`. Er wird genutzt, in XAML durch Festlegen der `Page.PrefersStatusBarHidden` angefügte Eigenschaft auf einen Wert von der `StatusBarHiddenMode` -Enumeration und optional die `Page.PreferredStatusBarUpdateAnimation` angefügte Eigenschaft auf einen Wert von der `UIStatusBarAnimation` Enumeration:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.PrefersStatusBarHidden="True"
             ios:Page.PreferredStatusBarUpdateAnimation="Fade">
  ...
</ContentPage>
```

Alternativ können sie aus C# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetPrefersStatusBarHidden(StatusBarHiddenMode.True)
         .SetPreferredStatusBarUpdateAnimation(UIStatusBarAnimation.Fade);
```

Die `Page.On<iOS>` Methode gibt an, dass diese plattformspezifischen nur unter iOS ausgeführt wird. Die `Page.SetPrefersStatusBarHidden` Methode in der `Xamarin.Forms.PlatformConfiguration.iOSSpecific` -Namespace wird verwendet, um die Sichtbarkeit der Statusleiste auf festzulegen eine [ `Page` ](xref:Xamarin.Forms.Page) durch Angabe eines der `StatusBarHiddenMode` Enumerationswerte: `Default`, `True` , oder `False`. Die `StatusBarHiddenMode.True` und `StatusBarHiddenMode.False` Werte festzulegen, die Sichtbarkeit der Status unabhängig von der geräteausrichtung, und die `StatusBarHiddenMode.Default` Wert Blendet die Statusleiste in einer vertikal compact-Umgebung.

Das Ergebnis ist, die die Sichtbarkeit der Statusleiste auf eine [ `Page` ](xref:Xamarin.Forms.Page) kann festgelegt werden:

![](page-status-bar-visibility-images/hide-status-bar.png "Statusleiste Sichtbarkeit plattformspezifische")

> [!NOTE]
> Auf einem [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage), dem angegebenen `StatusBarHiddenMode` Enumerationswert aktualisiert auch die Statusleiste auf alle untergeordneten Seiten. Auf allen anderen [ `Page` ](xref:Xamarin.Forms.Page)-abgeleiteten Typen, die den angegebenen `StatusBarHiddenMode` Enumerationswert wird nur die Statusleiste auf die aktuelle Seite aktualisieren.

Die `Page.SetPreferredStatusBarUpdateAnimation` Methode wird verwendet, um festzulegen, wie die Statusleiste eintritt oder dieses verlässt die [ `Page` ](xref:Xamarin.Forms.Page) durch Angabe eines der `UIStatusBarAnimation` Enumerationswerte: `None`, `Fade`, oder `Slide`. Wenn die `Fade` oder `Slide` Enumerationswert angegeben wird, ein 0,25 zweites Animation ausgeführt wird, wie die Statusleiste eintritt oder dieses verlässt die `Page`.

## <a name="related-links"></a>Verwandte Links

- [PlatformSpecifics (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
