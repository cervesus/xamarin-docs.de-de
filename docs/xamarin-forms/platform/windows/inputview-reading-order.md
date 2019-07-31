---
title: Inputview-Lesefolge unter Windows
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie Sie die Windows-plattformspezifische verwenden können, mit der die Lesefolge von bidirektionalem Text dynamisch erkannt werden kann.
ms.prod: xamarin
ms.assetid: E61BAEE0-C8B7-4F33-8DDC-FA1B9CA8E81D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: c184424a982aa82712685dbc33ad57422f2f8338
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68651426"
---
# <a name="inputview-reading-order-on-windows"></a>Inputview-Lesefolge unter Windows

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese universelle Windows-Plattform plattformspezifisch ermöglicht das dynamische Erkennen der Lesereihenfolge (von links nach rechts oder von rechts nach links) von bidirektionalem [`Label`](xref:Xamarin.Forms.Label) Text in [`Entry`](xref:Xamarin.Forms.Entry)-, [`Editor`](xref:Xamarin.Forms.Editor)-und-Instanzen. Es ist in XAML verwendet, durch Festlegen der [ `InputView.DetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.DetectReadingOrderFromContentProperty) (für `Entry` und `Editor` Instanzen) oder [ `Label.DetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Label.DetectReadingOrderFromContentProperty) angefügten Eigenschaft, um eine `boolean` Wert:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <Editor ... windows:InputView.DetectReadingOrderFromContent="true" />
        ...
    </StackLayout>
</ContentPage>
```

Alternativ können sie aus C# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

editor.On<Windows>().SetDetectReadingOrderFromContent(true);
```

Die `Editor.On<Windows>` Methode gibt an, dass diese plattformspezifischen nur für die universelle Windows-Plattform ausgeführt wird. Die [ `InputView.SetDetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.SetDetectReadingOrderFromContent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.InputView},System.Boolean)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) -Namespace wird verwendet um zu steuern, ob die leserichtung aus dem Inhalt erkannt wird die [ `InputView` ](xref:Xamarin.Forms.InputView). Darüber hinaus die `InputView.SetDetectReadingOrderFromContent` Methode kann verwendet werden, um umzuschalten, ob die leserichtung aus dem Inhalt, durch den Aufruf erkannt wird der [ `InputView.GetDetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.GetDetectReadingOrderFromContent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.InputView})) Methode, um den aktuellen Wert zurückzugeben:

```csharp
editor.On<Windows>().SetDetectReadingOrderFromContent(!editor.On<Windows>().GetDetectReadingOrderFromContent());
```

Das Ergebnis ist, [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), und [ `Label` ](xref:Xamarin.Forms.Label) Instanzen können die leserichtung ihrer Inhalte, die dynamisch ermittelt haben:

[![InputView erkennen die Lesefolge von Inhalten plattformspezifische](inputview-reading-order-images/editor-readingorder.png "wird, die Lesefolge von Inhalten plattformspezifische erkennen")](inputview-reading-order-images/editor-readingorder-large.png#lightbox "InputView erkennen die Lesefolge von Inhalt plattformspezifische")

> [!NOTE]
> Im Gegensatz zur Einstellung der [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) -Eigenschaft, die Logik für Ansichten, die erkennen, die leserichtung von deren Textinhalt wirkt sich nicht auf die Ausrichtung des Texts in der Ansicht. Stattdessen wird die Reihenfolge, in der bidirektionalen Textblöcke angeordnet werden, angepasst.

## <a name="related-links"></a>Verwandte Links

- [PlatformSpecifics (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Windowsspecific-API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
