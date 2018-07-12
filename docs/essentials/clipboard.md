---
title: 'Xamarin.Essentials: Zwischenablage'
description: Dieses Dokument beschreibt die Zwischenablage-Klasse in Xamarin.Essentials, was Ihnen das Kopieren und Einfügen von Text in die Systemzwischenablage zwischen Anwendungen.
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 41b15b480fa23bd49667b68e904043e4f1a95732
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38842614"
---
# <a name="xamarinessentials-clipboard"></a>Xamarin.Essentials: Zwischenablage

![Vorabversionen von NuGet](~/media/shared/pre-release.png)

Die **Zwischenablage** Klasse können Sie Text in die Systemzwischenablage zwischen Anwendungen kopieren und einfügen.

## <a name="using-clipboard"></a>Verwenden der Zwischenablage

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Überprüft, ob die **Zwischenablage** enthält Text, die derzeit kann nun eingefügt werden:

```csharp
var hasText = Clipboard.HasText;
```

Text festlegen, um die **Zwischenablage**:

```csharp
Clipboard.SetText("Hello World");
```

Zum Lesen von Text aus der **Zwischenablage**:

```csharp
var text = await Clipboard.GetTextAsync();
```

## <a name="api"></a>API

- [Quellcode für die Zwischenablage](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Clipboard)
- [Zwischenablage-API-Dokumentation](xref:Xamarin.Essentials.Clipboard)
