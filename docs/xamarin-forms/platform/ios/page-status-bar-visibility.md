---
title: Sichtbarkeit von Seiten Status Leiste unter IOS
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie die plattformspezifische IOS-Anwendung verwendet wird, die die Sichtbarkeit der Statusleiste auf einer Seite festlegt.
ms.prod: xamarin
ms.assetid: D8BB7C24-A27F-4758-8557-6A81F909ABD9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: a187efa9310fa150ddc884d8b42da5ccb9ecee11
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68655843"
---
# <a name="page-status-bar-visibility-on-ios"></a>Sichtbarkeit von Seiten Status Leiste unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese plattformspezifische IOS-Datei wird verwendet, um die Sichtbarkeit der Statusleiste [`Page`](xref:Xamarin.Forms.Page)eines festzulegen, und bietet die Möglichkeit, zu steuern, wie die Statusleiste `Page`in das-Element gelangt oder Sie verlässt. Er wird genutzt, in XAML durch Festlegen der `Page.PrefersStatusBarHidden` angefügte Eigenschaft auf einen Wert von der `StatusBarHiddenMode` -Enumeration und optional die `Page.PreferredStatusBarUpdateAnimation` angefügte Eigenschaft auf einen Wert von der `UIStatusBarAnimation` Enumeration:

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

- [PlatformSpecifics (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
