---
title: 'Xamarin.Essentials: Zwischenablage'
description: Dieses Dokument beschreibt die Klasse der Zwischenablage in Xamarin.Essentials, was Ihnen das Kopieren und Einfügen von Text in die Zwischenablage zwischen Anwendungen.
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 41b15b480fa23bd49667b68e904043e4f1a95732
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782358"
---
# <a name="xamarinessentials-clipboard"></a>Xamarin.Essentials: Zwischenablage

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
Clipboard.SetText("Hello World");
```

Lesen von Text aus der **Zwischenablage**:

```csharp
var text = await Clipboard.GetTextAsync();
```

## <a name="api"></a>API

- [Zwischenablage-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Clipboard)
- [Zwischenablage-API-Dokumentation](xref:Xamarin.Essentials.Clipboard)
