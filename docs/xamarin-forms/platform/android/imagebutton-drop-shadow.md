---
title: ImageButton-Dropdown Schatten unter Android
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie Sie die plattformspezifische Android-Funktion verwenden, die einen Schlag Schatten für eine ImageButton-Funktion aktiviert.
ms.prod: xamarin
ms.assetid: D3604D87-9F9F-4FE2-8B10-DF3B143C0734
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 567216171dd289e849ee0164452e4b876953f2a3
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68653586"
---
# <a name="imagebutton-drop-shadows-on-android"></a>ImageButton-Dropdown Schatten unter Android

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese Android-plattformspezifische dient zum Aktivieren eines Schlag Schattens `ImageButton`für einen. Es ist in XAML verwendet, durch Festlegen der `ImageButton.IsShadowEnabled` bindbare Eigenschaft `true`, sowie eine Reihe von zusätzlichen optional bindbare Eigenschaften, die das Rendern des Schlagschattens steuern:

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

Alternativ können sie aus C# mithilfe der fluent-API verwendet werden:

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
> Ein Drop-Schatten wird gezeichnet, als Teil der `ImageButton` Hintergrund, und der Hintergrund wird nur gezeichnet, wenn die `BackgroundColor` festgelegt wird. Aus diesem Grund werden ein Schlagschatten wird nicht gezeichnet, wenn die `ImageButton.BackgroundColor` Eigenschaft nicht festgelegt.

Die `ImageButton.On<Android>` Methode gibt an, dass diese plattformspezifischen nur unter Android ausgeführt wird. Die `ImageButton.SetIsShadowEnabled` Methode in der [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) -Namespace wird verwendet, um Kontrolle, ob ein Schlagschatten aktiviert ist, auf die `ImageButton`. Darüber hinaus können die folgenden Methoden aufgerufen werden, zum Rendern des Schlagschattens steuern:

- `SetShadowColor` – Legt die Farbe des Schlagschattens fest. Die Standardfarbe ist [ `Color.Default` ](xref:Xamarin.Forms.Color.Default*).
- `SetShadowOffset` – Legt den Offset des Schlagschattens fest. Der Offset ändert die Richtung der Schattenkopie umgewandelt wird und angegeben wird, als eine [ `Size` ](xref:Xamarin.Forms.Size) Wert. Die `Size` Strukturwerte werden in geräteunabhängigen Einheiten ausgedrückt, mit der erste Wert wird die Entfernung nach links (negativer Wert) oder rechts (positiver Wert) und der zweite Wert wird der Abstand (negativer Wert) oder unterhalb (positiver Wert) . Der Standardwert dieser Eigenschaft ist (0,0, 0,0), was dazu führt, wird der Schatten auf jeder Seite um Umwandeln der `ImageButton`.
- `SetShadowRadius`– Festlegen des Weichzeichnerradius zum Rendern des Schlagschattens verwendet. Der Radius-Standardwert ist 10,0.

> [!NOTE]
> Der Status der einen Schlagschatten abgefragt werden kann, durch den Aufruf der `GetIsShadowEnabled`, `GetShadowColor`, `GetShadowOffset`, und `GetShadowRadius` Methoden.

Das Ergebnis ist, die ein Schlagschatten aktiviert werden kann, auf eine `ImageButton`:

![](imagebutton-drop-shadow-images/imagebutton-drop-shadow.png "ImageButton mit Schlagschatten")

## <a name="related-links"></a>Verwandte Links

- [PlatformSpecifics (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Androidspecific-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [Androidspecific. AppCompat-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
