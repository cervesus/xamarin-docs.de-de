---
title: Xamarin.Essentials Zwischenablage
description: Das Zwischenablage-Klasse können Sie kopieren und Einfügen von Text in die Zwischenablage zwischen Anwendungen.
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 67a0218325918b57e5ed2618b57d52d3fe3ee820
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2018
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

- [Zwischenablage-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Clipboard)
- [Zwischenablage-API-Dokumentation](xref:Xamarin.Essentials.Clipboard)
