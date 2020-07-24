---
title: Eingabe Cursor Farbe unter IOS
description: Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie die plattformspezifische IOS-Anwendung verwenden, die die Cursor Farbe eines Eintrags festlegt.
ms.prod: xamarin
ms.assetid: 867D70BA-53F9-4434-8094-85D71DCECC2D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 501210f669c7d62eddaf7ebe3b4ebf19614589a7
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86936343"
---
# <a name="entry-cursor-color-on-ios"></a>Eingabe Cursor Farbe unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Mit diesem IOS-plattformspezifischen wird die Cursor Farbe eines [`Entry`](xref:Xamarin.Forms.Entry) auf eine angegebene Farbe festgelegt. Sie wird in XAML verwendet, indem die [`Entry.CursorColor`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.CursorColorProperty) bindbare Eigenschaft auf festgelegt wird [`Color`](xref:Xamarin.Forms.Color) :

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <Entry ... ios:Entry.CursorColor="LimeGreen" />
    </StackLayout>
</ContentPage>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var entry = new Xamarin.Forms.Entry();
entry.On<iOS>().SetCursorColor(Color.LimeGreen);
```

Die- `Entry.On<iOS>` Methode gibt an, dass diese plattformspezifische nur unter IOS ausgeführt wird. [ `Entry.SetCursorColor` ] (Xref: Xamarin.Forms . Platformconfiguration. iosspecific. entry. setcurrsorcolor ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. IOS, Xamarin.Forms . Eintrag}, Xamarin.Forms . Color))-Methode im- [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) Namespace legt die Cursor Farbe auf eine angegebene fest [`Color`](xref:Xamarin.Forms.Color) . Außerdem ist [ `Entry.GetCursorColor` ] (Xref: Xamarin.Forms . Platformconfiguration. iosspecific. entry. getcurrsorcolor ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. IOS, Xamarin.Forms . Entry}))-Methode kann verwendet werden, um die aktuelle Cursor Farbe abzurufen.

Das Ergebnis ist, dass die Cursor Farbe in einem [`Entry`](xref:Xamarin.Forms.Entry) auf einen bestimmten festgelegt werden kann [`Color`](xref:Xamarin.Forms.Color) :

![Farbe für Eingabecursor](entry-cursor-color-images/entry-cursorcolor.png)

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
