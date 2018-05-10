---
title: Xamarin.Essentials Zwischenablage
description: Das Zwischenablage-Klasse können Sie kopieren und Einfügen von Text in die Zwischenablage zwischen Anwendungen.
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 44ba8090851b65327682b3d290d58fb23bb8aae4
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
---
# <a name="xamarinessentials-clipboard"></a>Xamarin.Essentials Zwischenablage

![Vorabversion NuGet](~/media/shared/pre-release.png)

Die **Zwischenablage** -Klasse können Sie kopieren und Einfügen von Text in die Zwischenablage zwischen Anwendungen.

## <a name="using-clipboard"></a>Verwenden der Zwischenablage

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Beim Überprüfen der **Zwischenablage** enthält Text, die derzeit kann nun eingefügt werden:

```csharp
var hasText = Clipboard.HasText;
```

Text festgelegt, um die **Zwischenablage**:

```csharp
ClipBoard.SetText("Hello World");
```

Lesen von Text aus der **Zwischenablage**:

```csharp
var text = await Clipboard.GetTextAsync();
```

## <a name="api"></a>API

- [Zwischenablage-Quellcode](https://github.com/xamarin/Essentials/tree/master/Essentials/Clipboard)
- [Zwischenablage-API-Dokumentation](xref:Xamarin.Essentials.Clipboard)
