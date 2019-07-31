---
title: Auswahl Elementauswahl unter IOS
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie Sie die plattformspezifische IOS-Anwendung nutzen, die steuert, wann die Elementauswahl in einer Auswahl erfolgt.
ms.prod: xamarin
ms.assetid: 26B0604A-BD30-49FD-83A6-F0EDFBB0524B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 57420921100c99db1e2c3a5259ece30cfda719f2
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68651842"
---
# <a name="picker-item-selection-on-ios"></a>Auswahl Elementauswahl unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese IOS-plattformspezifischen Steuerelemente, wenn die Element [`Picker`](xref:Xamarin.Forms.Picker)Auswahl in einem auftritt, sodass der Benutzer angeben kann, dass die Elementauswahl beim Durchsuchen von Elementen im Steuerelement oder erst nach dem Drücken der Schaltfläche **done** erfolgt. Es ist in XAML verwendet, durch Festlegen der `Picker.UpdateMode` angefügte Eigenschaft auf den Wert der `UpdateMode` Enumeration:

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

Alternativ können sie aus C# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

picker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
```

Die `Picker.On<iOS>` Methode gibt an, dass diese plattformspezifischen nur unter iOS ausgeführt wird. Die `Picker.SetUpdateMode` Methode in der [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) -Namespace wird verwendet, um zu steuern Elementauswahl tritt auf, mit der `UpdateMode` Enumeration, die Bereitstellung von zwei möglicher Werten:

- `Immediately` – Auswahl von Listenelementen tritt auf, wie der Benutzer Elemente in durchsucht die [ `Picker` ](xref:Xamarin.Forms.Picker). Dies ist das Standardverhalten in Xamarin.Forms.
- `WhenFinished` – Elementauswahl tritt nur auf, nachdem der Benutzer geklickt hat, hat die **Fertig** Schaltfläche der [ `Picker` ](xref:Xamarin.Forms.Picker).

Darüber hinaus die `SetUpdateMode` Methode kann verwendet werden, zum Umschalten der Enumerationswerte durch Aufrufen der `UpdateMode` Methode, die die aktuelle zurückgibt `UpdateMode`:

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

Das Ergebnis ist, die einem angegebenen `UpdateMode` gilt, an die [ `Picker` ](xref:Xamarin.Forms.Picker), die steuert, wann Elementauswahl auftritt:

[![](picker-selection-images/picker-updatemode.png "Auswahl UpdateMode plattformspezifische")](picker-selection-images/picker-updatemode-large.png#lightbox "Auswahl UpdateMode plattformspezifische")

## <a name="related-links"></a>Verwandte Links

- [PlatformSpecifics (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
