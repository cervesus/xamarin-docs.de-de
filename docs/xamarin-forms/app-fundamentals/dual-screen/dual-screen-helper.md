---
title: Xamarin.Forms-Plattformhilfsprogramme für Dual-Screen-Geräte
description: In diesem Artikel wird erläutert, wie Sie mit der DualScreenHelper-Klasse in Xamarin.Forms die Benutzeroberfläche Ihrer App für Dual-Screen-Geräte wie Surface Duo und Surface Neo optimieren.
ms.prod: xamarin
ms.assetid: 5aa184c2-5611-427d-85c7-1c56486c3e1b
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
ms.openlocfilehash: e16e926d9eb3c5f3ca04b7e1359f24624a971617
ms.sourcegitcommit: 5b6d3bddf7148f8bb374de5657bdedc125d72ea7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2020
ms.locfileid: "78160572"
---
# <a name="xamarinforms-dual-screen-platform-helpers"></a>Xamarin.Forms-Plattformhilfsprogramme für Dual-Screen-Geräte

![](~/media/shared/preview.png "This API is currently pre-release")

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-samples/UserInterface/DualScreenDemos)

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
