---
title: Demo zum Anwendungslebenszyklus für Xamarin.iOS
description: Dieses Dokument erläutert die verschiedenen Lebenszyklusereignissen behandelt, die von der app-Delegat in einer iOS-Anwendung veranschaulicht, wann und wie diese Ereignisse verarbeitet werden.
ms.prod: xamarin
ms.assetid: 5C8AACA6-49F8-4C6D-99C3-5F443C01B230
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/17/2018
ms.openlocfilehash: 3beb511c03b328ecea824bf89355d056df003f3e
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50102697"
---
# <a name="application-lifecycle-demo-for-xamarinios"></a>Demo zum Anwendungslebenszyklus für Xamarin.iOS

In diesem Artikel und [Beispielcode](https://developer.xamarin.com/samples/monotouch/LifecycleDemo/) veranschaulicht vier fragmentteils in iOS und die Rolle der `AppDelegate` Methoden in der Benachrichtigung der Anwendung von Wenn Zustände geändert. Die Anwendung wird Updates in der Konsole ausgegeben, wenn sich die app Zustand ändert:

[![](application-lifecycle-demo-images/image3-sml.png "Die Beispiel-app")](application-lifecycle-demo-images/image3.png#lightbox)

[![](application-lifecycle-demo-images/image4.png "Die app wird Updates in der Konsole ausgegeben, wenn die app-statusänderungen")](application-lifecycle-demo-images/image4.png#lightbox)

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

1. Öffnen der **Lebenszyklus** -Projekt in der **LifecycleDemo** Lösung.
1. Öffnen Sie die `AppDelegate` Klasse. Protokollierung wurde hinzugefügt, um die Lebenszyklusmethoden, um anzugeben, wenn der Zustand die Anwendung geändert wurde:

    ```csharp
    public override void OnActivated(UIApplication application)
    {
        Console.WriteLine("OnActivated called, App is active.");
    }
    public override void WillEnterForeground(UIApplication application)
    {
        Console.WriteLine("App will enter foreground");
    }
    public override void OnResignActivation(UIApplication application)
    {
        Console.WriteLine("OnResignActivation called, App moving to inactive state.");
    }
    public override void DidEnterBackground(UIApplication application)
    {
        Console.WriteLine("App entering background state.");
    }
    // not guaranteed that this will run
    public override void WillTerminate(UIApplication application)
    {
        Console.WriteLine("App is terminating.");
    }
    ```

1. Starten Sie die Anwendung im Simulator oder auf dem Gerät. `OnActivated` wird aufgerufen, wenn die app gestartet wird. Die Anwendung ist jetzt in der _Active_ Zustand.
1. Drücken Sie die Start-Schaltfläche auf dem Simulator oder ein Gerät, um die Anwendung in den Hintergrund bringen. `OnResignActivation` und `DidEnterBackground` wird aufgerufen werden, wie die app-Übergänge von `Active` zu `Inactive` und in der `Backgrounded` Zustand. Da keine Anwendung Code festgelegt, im Hintergrund auszuführen ist, gilt die Anwendung _angehalten_ im Arbeitsspeicher.
1. Navigieren Sie zurück an die app aus, um es wieder in den Vordergrund zu versetzen. `WillEnterForeground` und `OnActivated` sowohl aufgerufen werden:

    ![](application-lifecycle-demo-images/image4.png "Statusänderungen in der Konsole ausgegeben.")

    Die folgende Codezeile in der ansichtscontroller wird ausgeführt, wenn die Anwendung im Vordergrund aus dem Hintergrund eingegeben hat, und ändert sich der Text, der auf dem Bildschirm angezeigt:

    ```csharp
    UIApplication.Notifications.ObserveWillEnterForeground ((sender, args) => {
        label.Text = "Welcome back!";
    });
    ```

1. Drücken Sie die **Startseite** Schaltfläche, um die Anwendung in den Hintergrund. Klicken Sie dann, Doppeltippen Sie auf die **Startseite** Schaltfläche, um die Anwendung wechseln. Auf dem iPhone X, Wischen Sie von den unteren Rand des Bildschirms:

    [![Die-anwendungsschalters](application-lifecycle-demo-images/app-switcher-sml.png "des-anwendungsschalters")](application-lifecycle-demo-images/app-switcher.png#lightbox)
  
1. Suchen Sie die Anwendung in der App-Switcher, und navigieren Sie einrichten, um zu entfernen (auf iOS 11, langes drücken, bis die roten Symbole in der Ecke angezeigt werden):

    [![Wischen Sie bis zum Entfernen einer ausgeführten app](application-lifecycle-demo-images/app-switcher-swipe-sml.png "Streifen zu entfernen, eine ausgeführte app")](application-lifecycle-demo-images/app-switcher-swipe.png#lightbox)

iOS wird die Anwendung zu beenden. Beachten Sie, dass `WillTerminate` wird nicht aufgerufen werden, da die Anwendung bereits ist _angehalten_ im Hintergrund.

## <a name="related-links"></a>Verwandte Links

- [LifecycleDemo (Beispiel)](https://developer.xamarin.com/samples/monotouch/LifecycleDemo/)
