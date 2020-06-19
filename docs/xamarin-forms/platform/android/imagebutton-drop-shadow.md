---
title: ImageButton-Dropdown Schatten unter Android
description: Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie die plattformspezifische Android-Funktion verwenden, die einen Schlag Schatten für eine ImageButton-Funktion aktiviert.
ms.prod: xamarin
ms.assetid: D3604D87-9F9F-4FE2-8B10-DF3B143C0734
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5e2ad97eb5e7db3b832e8fb4340c86904b766b9a
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84139998"
---
# <a name="imagebutton-drop-shadows-on-android"></a>ImageButton-Dropdown Schatten unter Android

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese Android-plattformspezifische dient zum Aktivieren eines Schlag Schattens für einen `ImageButton` . Sie wird in XAML verwendet, indem die `ImageButton.IsShadowEnabled` bindbare Eigenschaft auf festgelegt `true` wird, sowie eine Reihe zusätzlicher optionaler bindbare Eigenschaften, die den Schlag Schatten Steuern:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
       <ImageButton ...
                    Source="XamarinLogo.png"
                    BackgroundColor="GhostWhite"
                    android:ImageButton.IsShadowEnabled="true"
                    android:ImageButton.ShadowColor="Gray"
                    android:ImageButton.ShadowRadius="12">
            <android:ImageButton.ShadowOffset>
                <Size>
                    <x:Arguments>
                        <x:Double>10</x:Double>
                        <x:Double>10</x:Double>
                    </x:Arguments>
                </Size>
            </android:ImageButton.ShadowOffset>
        </ImageButton>
        ...
    </StackLayout>
</ContentPage>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

var imageButton = new Xamarin.Forms.ImageButton { Source = "XamarinLogo.png", BackgroundColor = Color.GhostWhite, ... };
imageButton.On<Android>()
           .SetIsShadowEnabled(true)
           .SetShadowColor(Color.Gray)
           .SetShadowOffset(new Size(10, 10))
           .SetShadowRadius(12);
```

> [!IMPORTANT]
> Ein Schlag Schatten wird als Teil des `ImageButton` Hintergrunds gezeichnet, und der Hintergrund wird nur gezeichnet, wenn die- `BackgroundColor` Eigenschaft festgelegt ist. Daher wird ein Schlag Schatten nicht gezeichnet, wenn die- `ImageButton.BackgroundColor` Eigenschaft nicht festgelegt ist.

Die- `ImageButton.On<Android>` Methode gibt an, dass diese plattformspezifische nur unter Android ausgeführt wird. Die- `ImageButton.SetIsShadowEnabled` Methode im- [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) Namespace wird verwendet, um zu steuern, ob ein Schlag Schatten für aktiviert ist `ImageButton` . Außerdem können die folgenden Methoden aufgerufen werden, um den Schlag Schatten zu steuern:

- `SetShadowColor`– legt die Farbe des Schlag Schattens fest. Die Standardfarbe ist [`Color.Default`](xref:Xamarin.Forms.Color.Default*) .
- `SetShadowOffset`– Legt den Offset des Schlag Schattens fest. Der Offset ändert die Richtung, in die der Schatten umgewandelt wird, und wird als [`Size`](xref:Xamarin.Forms.Size) Wert angegeben. Die `Size` Struktur Werte werden in geräteunabhängigen Einheiten ausgedrückt, wobei der erste Wert die Distanz nach links (negativer Wert) oder rechts (positiver Wert) und der zweite Wert die Distanz oberhalb (negativer Wert) oder niedriger (positiver Wert) ist. Der Standardwert dieser Eigenschaft ist (0,0, 0,0), was dazu führt, dass der Schatten um jede Seite von umbrochen wird `ImageButton` .
- `SetShadowRadius`– Legt den zum Rendering des Schlag Schattens verwendeten Weichzeichnerradius fest. Der Standard RADIUS-Wert ist 10,0.

> [!NOTE]
> Der Zustand eines Ablage Schattens kann durch Aufrufen der `GetIsShadowEnabled` `GetShadowColor` Methoden,, und abgefragt werden `GetShadowOffset` `GetShadowRadius` .

Das Ergebnis ist, dass ein Ablage Schatten für eine aktiviert werden kann `ImageButton` :

![](imagebutton-drop-shadow-images/imagebutton-drop-shadow.png "ImageButton with drop shadow")

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Androidspecific-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [Androidspecific. AppCompat-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
