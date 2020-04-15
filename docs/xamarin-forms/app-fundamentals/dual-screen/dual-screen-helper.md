---
title: Xamarin.Forms-Plattformhilfsprogramme für Dual-Screen-Geräte
description: In diesem Artikel wird erläutert, wie Sie mit der DualScreenHelper-Klasse in Xamarin.Forms die Benutzeroberfläche Ihrer App für Dual-Screen-Geräte wie Surface Duo und Surface Neo optimieren.
ms.prod: xamarin
ms.assetid: 5aa184c2-5611-427d-85c7-1c56486c3e1b
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
ms.openlocfilehash: 9ab6106b8b92660d416e8a22cc3b1bdf1b41c5cf
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "80628267"
---
# <a name="xamarinforms-dual-screen-platform-helpers"></a>Xamarin.Forms-Plattformhilfsprogramme für Dual-Screen-Geräte

![](~/media/shared/preview.png "This API is currently pre-release")

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-dualscreendemos/)

Mit der `DualScreenHelper`-Klasse lässt sich erkennen, ob ein Gerät den Bild-im-Bild-Modus unterstützt, und anschließend eine `ContentPage`-Klasse als Bild-im-Bild-Fenster öffnen. Wenn Sie ein Neo-Gerät verwenden, wird das Fenster in der WonderBar-Leiste geöffnet, wenn sich das Gerät im Modus „Verfassen“ befindet.

## <a name="properties"></a>Eigenschaften

- Mit `HasCompactModeSupport` wird überprüft, ob die Plattform das Öffnen einer `ContentPage`-Klasse im CompactMode unterstützt.
- `OpenCompactMode` öffnet eine `ContentPage`-Klasse im CompactMode, wenn dies von der Plattform unterstützt wird.

## <a name="example"></a>Beispiel

```csharp
async void OpenCompactWindowClicked(object sender, EventArgs e)
{
    if (!DualScreenHelper.HasCompactModeSupport())
    {
        await DisplayAlert("Unsupported", "This platform doesn't support this feature", "Ok");
        return;
    }

    ContentPage page = new ContentPage() { BackgroundColor = Color.Purple };

    var button = new Button()
    {
        Text = "Close this Window"
    };

    var layout = new StackLayout()
    {
        Children =
        {
            new Label(){ Text = "Welcome to your Compact Mode Window" },
            button
        },
        BackgroundColor = Color.Yellow,
        HorizontalOptions = LayoutOptions.Fill,
        VerticalOptions = LayoutOptions.Fill
    };

    page.Content = new ScrollView() { Content = layout };
    var args = await DualScreenHelper.OpenCompactMode(page);

    button.Command = new Command(async () =>
    {
        await args.CloseAsync();
    });
}
```
