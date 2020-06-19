---
title: Auswahl Elementauswahl unter IOS
description: Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie die plattformspezifische IOS-Anwendung nutzen, die steuert, wann die Elementauswahl in einer Auswahl erfolgt.
ms.prod: xamarin
ms.assetid: 26B0604A-BD30-49FD-83A6-F0EDFBB0524B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 75ef118b642a8c6a66205c6f7e3bc03089c6593c
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84127893"
---
# <a name="picker-item-selection-on-ios"></a>Auswahl Elementauswahl unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese IOS-plattformspezifischen Steuerelemente, wenn die Elementauswahl in einem Auftritt [`Picker`](xref:Xamarin.Forms.Picker) , sodass der Benutzer angeben kann, dass die Elementauswahl beim Durchsuchen von Elementen im Steuerelement oder erst nach dem Drücken der Schaltfläche **done** erfolgt. Sie wird in XAML verwendet, indem die `Picker.UpdateMode` angefügte-Eigenschaft auf einen Wert der-Enumeration festgelegt wird `UpdateMode` :

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <Picker ... Title="Select a monkey" ios:Picker.UpdateMode="WhenFinished">
          ...
        </Picker>
        ...
    </StackLayout>
</ContentPage>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

picker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
```

Die- `Picker.On<iOS>` Methode gibt an, dass diese plattformspezifische nur unter IOS ausgeführt wird. Die- `Picker.SetUpdateMode` Methode im- [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) Namespace wird verwendet, um zu steuern, wann die Elementauswahl stattfindet, wobei die- `UpdateMode` Enumeration zwei mögliche Werte bereitstellt:

- `Immediately`die – Item-Auswahl tritt auf, wenn der Benutzer Elemente in der durchsucht [`Picker`](xref:Xamarin.Forms.Picker) . Dies ist das Standardverhalten in Xamarin.Forms .
- `WhenFinished`die Auswahl von – Items erfolgt nur, wenn der Benutzer die Schaltfläche **done** in der gedrückt hat [`Picker`](xref:Xamarin.Forms.Picker) .

Außerdem kann die- `SetUpdateMode` Methode zum Umschalten der Enumerationswerte verwendet werden, indem die- `UpdateMode` Methode aufgerufen wird, die den aktuellen zurückgibt `UpdateMode` :

```csharp
switch (picker.On<iOS>().UpdateMode())
{
    case UpdateMode.Immediately:
        picker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
        break;
    case UpdateMode.WhenFinished:
        picker.On<iOS>().SetUpdateMode(UpdateMode.Immediately);
        break;
}
```

Das Ergebnis ist, dass eine angegebene `UpdateMode` auf den angewendet wird [`Picker`](xref:Xamarin.Forms.Picker) , der steuert, wann die Elementauswahl erfolgt:

[![](picker-selection-images/picker-updatemode.png "Picker UpdateMode Platform-Specific")](picker-selection-images/picker-updatemode-large.png#lightbox "Picker UpdateMode Platform-Specific")

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
