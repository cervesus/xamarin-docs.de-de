---
title: 'Xamarin.Essentials: Zwischenablage'
description: In diesem Dokument wird die Klasse „Clipboard“ in Xamarin.Essentials beschrieben, mit der Sie Text in die Zwischenablage kopieren und anwendungsübergreifend einfügen können.
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
author: jamesmontemagno
ms.author: jamont
ms.date: 02/12/2019
ms.custom: video
ms.openlocfilehash: 3511850391b2be809daf2b70e81fa5b591db8dfa
ms.sourcegitcommit: c6ff24b524d025d7e87b7b9c25f04c740dd93497
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2019
ms.locfileid: "56240343"
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

## <a name="api"></a>API

- [Clipboard-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Clipboard)
- [Clipboard-API-Dokumentation](xref:Xamarin.Essentials.Clipboard)

## <a name="related-video"></a>Zugehörige Videos

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Clipboard-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
