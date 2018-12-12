---
title: 'Xamarin.Essentials: Zwischenablage'
description: In diesem Dokument wird die Klasse „Clipboard“ in Xamarin.Essentials beschrieben, mit der Sie Text in die Zwischenablage kopieren und anwendungsübergreifend einfügen können.
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: 90ede9d0d0fbee9efabcce25c0ae7c3c439d9e69
ms.sourcegitcommit: 01f93a34b466f8d4043cef68fab9b35cd8decee6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2018
ms.locfileid: "52898702"
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
