---
title: ListView-Group-Headerformat unter iOS
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie der iOS-Plattform-spezifische zu nutzen, die steuert, ob die ListView Headerzellen während des Bildlaufs "float" werden.
ms.prod: xamarin
ms.assetid: 099B2C7F-727F-4BCF-903B-87E728108C24
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 58b3787b71cff9b78f1c6b577be6c320367f1cee
ms.sourcegitcommit: 00744f754527e5b55154365f89691caaf1c9d929
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2019
ms.locfileid: "57557452"
---
# <a name="listview-group-header-style-on-ios"></a>ListView-Group-Headerformat unter iOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

Diese iOS-Plattform-spezifische steuert, ob [ `ListView` ](xref:Xamarin.Forms.ListView) Headerzellen "float" während des Bildlaufs. Es ist in XAML verwendet, durch Festlegen der `ListView.GroupHeaderStyle` bindbare Eigenschaft auf den Wert der `GroupHeaderStyle` Enumeration:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ... ios:ListView.GroupHeaderStyle="Grouped">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

listView.On<iOS>().SetGroupHeaderStyle(GroupHeaderStyle.Grouped);
```

Die `ListView.On<iOS>` Methode gibt an, dass diese plattformspezifischen nur unter iOS ausgeführt wird. Die `ListView.SetGroupHeaderStyle` Methode in der [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) -Namespace wird verwendet, um Steuerelement gibt an, ob [ `ListView` ](xref:Xamarin.Forms.ListView) Headerzellen "float" während des Bildlaufs. Die `GroupHeaderStyle` Enumeration bietet zwei mögliche Werte:

- `Plain` – Gibt an, dass die Headerzellen Verankerung aufheben, wenn die [ `ListView` ](xref:Xamarin.Forms.ListView) ist ein Bildlauf durchgeführt (Standard).
- `Grouped` – Gibt an, dass die Headerzellen nicht wenn "float" werden die [ `ListView` ](xref:Xamarin.Forms.ListView) ein Bildlauf durchgeführt wird.

Darüber hinaus die `ListView.GetGroupHeaderStyle` Methode kann verwendet werden, um das Zurückgeben der `GroupHeaderStyle` angewendeten der [ `ListView` ](xref:Xamarin.Forms.ListView).

Das Ergebnis ist, die einem angegebenen `GroupHeaderStyle` Wert gilt für die [ `ListView` ](xref:Xamarin.Forms.ListView), die steuert, ob die Headerzellen während des Bildlaufs "float":

[![Screenshot der Gleitkomma- und unverankerte ListView Headerzellen, die unter iOS](listview-group-header-style-images/group-header-styles.png "ListView mit Gleitkomma- und unverankerte Headerzellen")](listview-group-header-style-images/group-header-styles-large.png#lightbox "ListView mit Gleitkomma- und unverankerte Headerzellen")

## <a name="related-links"></a>Verwandte Links

- [PlatformSpecifics (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
