---
title: 'Xamarin.Forms: DualScreenHelper'
description: In diesem Artikel wird erläutert, wie Sie mit DualScreenHelper in Xamarin.Forms die Benutzeroberfläche Ihrer App für Dual-Screen-Geräte wie Surface Duo und Surface Neo optimieren.
ms.prod: xamarin
ms.assetid: 5aa184c2-5611-427d-85c7-1c56486c3e1b
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
ms.openlocfilehash: b06e105560de635a21b42e8366bb47842372cf6a
ms.sourcegitcommit: 07941cf9704ff88cf4087de5ebdea623ff54edb1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2020
ms.locfileid: "77145544"
---
# <a name="xamarinforms-dualscreenhelper"></a>Xamarin.Forms: DualScreenHelper
[![Beispiel herunterladen](~/media/shared/download.png)Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-samples/UserInterface/DualScreenDemos)
_Mit DualScreenHelper lässt sich erkennen, ob ein Gerät den Bild-im-Bild-Modus unterstützt, und anschließend eine ContentPage als Bild-im-Bild-Fenster öffnen. Wenn Sie ein Neo-Gerät verwenden, wird das Fenster in der WonderBar-Leiste geöffnet, wenn sich das Gerät im Modus „Verfassen“ befindet._
## <a name="properties"></a>Eigenschaften
- Mit `HasCompactModeSupport` wird überprüft, ob die Plattform das Öffnen einer ContentPage im CompactMode unterstützt.
- Mit `OpenCompactMode` wird eine ContentPage im CompactMode geöffnet, wenn die Plattform dies unterstützt.
## <a name="example-usage"></a>Verwendungsbeispiel
```c#
async void OpenCompactWindowClicked(object sender, EventArgs e)
{
    if(!DualScreenHelper.HasCompactModeSupport())
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