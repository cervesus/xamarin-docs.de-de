---
title: DatePicker-Elementauswahl unter IOS
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie Sie die plattformspezifische IOS-Anwendung nutzen, die steuert, wann die Elementauswahl in einem DatePicker auftritt.
ms.prod: xamarin
ms.assetid: 847E69D1-6AE0-4E82-B9C8-919E009C2014
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/15/2020
ms.openlocfilehash: df84cf01909cec564edc9c6c8bb55382a2b9dfe3
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2020
ms.locfileid: "77646689"
---
# <a name="datepicker-item-selection-on-ios"></a>DatePicker-Elementauswahl unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese IOS-plattformspezifischen Steuerelemente, wenn die Elementauswahl in einer [`DatePicker`](xref:Xamarin.Forms.DatePicker)erfolgt, sodass der Benutzer angeben kann, dass die Elementauswahl beim Durchsuchen von Elementen im Steuerelement oder erst nach dem Drücken der Schaltfläche **done** erfolgt. Sie wird in XAML verwendet, indem die `DatePicker.UpdateMode` angefügte-Eigenschaft auf einen Wert der `UpdateMode`-Enumeration festgelegt wird:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
       <DatePicker MinimumDate="01/01/2020"
                   MaximumDate="12/31/2020"
                   ios:DatePicker.UpdateMode="WhenFinished" />
       ...
    </StackLayout>
</ContentPage>
```

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

datePicker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
```

Die `DatePicker.On<iOS>`-Methode gibt an, dass diese plattformspezifische nur unter IOS ausgeführt wird. Die `DatePicker.SetUpdateMode`-Methode im [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) -Namespace wird verwendet, um zu steuern, wann die Elementauswahl erfolgt, wobei die `UpdateMode`-Enumeration zwei mögliche Werte bereitstellt:

- `Immediately` – Elementauswahl tritt auf, wenn der Benutzer Elemente im [`DatePicker`](xref:Xamarin.Forms.DatePicker)durchsucht. Dies ist das Standardverhalten in Xamarin.Forms.
- `WhenFinished` – Elementauswahl findet nur statt, wenn der Benutzer auf die Schaltfläche **done** in der [`DatePicker`](xref:Xamarin.Forms.DatePicker)geklickt hat.

Außerdem kann die `SetUpdateMode`-Methode verwendet werden, um die Enumerationswerte durch Aufrufen der `UpdateMode`-Methode, die die aktuelle `UpdateMode`zurückgibt, zum Umschalten zu wechseln:

```csharp
switch (datePicker.On<iOS>().UpdateMode())
{
    case UpdateMode.Immediately:
        datePicker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
        break;
    case UpdateMode.WhenFinished:
        datePicker.On<iOS>().SetUpdateMode(UpdateMode.Immediately);
        break;
}
```

Das Ergebnis ist, dass ein angegebener `UpdateMode` auf den [`DatePicker`](xref:Xamarin.Forms.DatePicker)angewendet wird, der steuert, wann die Elementauswahl erfolgt:

[![Screenshot der DatePicker-Aktualisierungs Modi](datepicker-selection-images/datepicker-updatemode.png "DatePicker UpdateMode plattformspezifisch")](datepicker-selection-images/datepicker-updatemode-large.png#lightbox "DatePicker UpdateMode plattformspezifisch")

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
