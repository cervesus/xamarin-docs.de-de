---
title: 'Xamarin.Essentials: Zwischenablage'
description: In diesem Dokument wird die Klasse „Clipboard“ in Xamarin.Essentials beschrieben, mit der Sie Text in die Zwischenablage kopieren und anwendungsübergreifend einfügen können.
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
author: jamesmontemagno
ms.author: jamont
ms.date: 01/06/2020
ms.custom: video
ms.openlocfilehash: 0b5eaf3feb608a352f8f9c97bdddac55c89d4f94
ms.sourcegitcommit: fec87846fcb262fc8b79774a395908c8c8fc8f5b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2020
ms.locfileid: "77545179"
---
# <a name="xamarinessentials-clipboard"></a>Xamarin.Essentials: Zwischenablage

Mit der Klasse **Clipboard** können Sie Text in die Zwischenablage kopieren und anwendungsübergreifend einfügen.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-clipboard"></a>Verwenden der Zwischenablage

Fügen Sie Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

So überprüfen Sie, ob die **Zwischenablage** aktuell Text enthält, der eingefügt werden kann

```csharp
var hasText = Clipboard.HasText;
```

So legen Sie Text für die **Zwischenablage** fest

```csharp
await Clipboard.SetTextAsync("Hello World");
```

So lesen Sie Text aus der **Zwischenablage**

```csharp
var text = await Clipboard.GetTextAsync();
```

Jedes Mal, wenn sich der Inhalt der Zwischenablage geändert hat, wird ein Ereignis ausgelöst:

```csharp
public class ClipboardTest
{
    public ClipboardTest()
    {
        // Register for clipboard changes, be sure to unsubscribe when needed
        Clipboard.ClipboardContentChanged += OnClipboardContentChanged;
    }

    void OnClipboardContentChanged(object sender, EventArgs    e)
    {
        Console.WriteLine($"Last clipboard change at {DateTime.UtcNow:T}";);
    }
}
```

> [!TIP]
> Der Zugriff auf die Zwischenablage muss über den Hauptthread der Benutzeroberfläche erfolgen. Informationen zum Aufrufen von Methoden im Hauptthread der Benutzeroberfläche finden Sie in der Dokumentation zur API [MainThread](~/essentials/main-thread.md).

## <a name="api"></a>API

- [Clipboard-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Clipboard)
- [Clipboard-API-Dokumentation](xref:Xamarin.Essentials.Clipboard)

## <a name="related-video"></a>Zugehörige Videos

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Clipboard-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
