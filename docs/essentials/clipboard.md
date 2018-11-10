---
title: 'Xamarin.Essentials: Zwischenablage'
description: In diesem Dokument wird die Klasse „Clipboard“ in Xamarin.Essentials beschrieben, mit der Sie Text in die Zwischenablage kopieren und anwendungsübergreifend einfügen können.
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 8dd238da678dfb5773801137d313b286590aa463
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675535"
---
# <a name="xamarinessentials-clipboard"></a>Xamarin.Essentials: Zwischenablage

![NuGet-Vorabrelease](~/media/shared/pre-release.png)

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
Clipboard.SetText("Hello World");
```

So lesen Sie Text aus der **Zwischenablage**

```csharp
var text = await Clipboard.GetTextAsync();
```

## <a name="api"></a>API

- [Clipboard-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Clipboard)
- [Clipboard-API-Dokumentation](xref:Xamarin.Essentials.Clipboard)
