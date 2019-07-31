---
title: Demo zum Anwendungslebenszyklus für xamarin. IOS
description: In diesem Dokument werden verschiedene Lebenszyklus Ereignisse erläutert, die vom APP-Delegaten in einer IOS-Anwendung verarbeitet werden. Dies veranschaulicht, wann und wie diese Ereignisse behandelt werden
ms.prod: xamarin
ms.assetid: 5C8AACA6-49F8-4C6D-99C3-5F443C01B230
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/17/2018
ms.openlocfilehash: 4eefbd63a91c6fd9eeed7a6e5043db5a2ee9105b
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68649374"
---
# <a name="application-lifecycle-demo-for-xamarinios"></a>Demo zum Anwendungslebenszyklus für xamarin. IOS

In diesem Artikel und [Beispielcode](https://docs.microsoft.com/samples/xamarin/ios-samples/lifecycledemo) werden die vier Anwendungs Zustände in IOS und die Rolle `AppDelegate` der Methoden zum Benachrichtigen der Anwendung von, wenn sich die Zustände ändern, veranschaulicht. Die Anwendung druckt Updates an der Konsole, wenn sich der Status der app ändert:

[![](application-lifecycle-demo-images/image3-sml.png "Die Beispiel-App")](application-lifecycle-demo-images/image3.png#lightbox)

[![](application-lifecycle-demo-images/image4.png "Die APP druckt Updates an der Konsole, wenn der Status der APP geändert wird.")](application-lifecycle-demo-images/image4.png#lightbox)

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

1. Öffnen Sie das **Lebenszyklus** Projekt in der Projekt Mappe " **lifecycledemo** ".
1. Öffnen Sie die `AppDelegate` -Klasse. Die Protokollierung wurde den Lifecycle-Methoden hinzugefügt, um anzugeben, wann die Anwendung den Zustand geändert hat:

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

1. Starten Sie die Anwendung im Simulator oder auf dem Gerät. `OnActivated`wird aufgerufen, wenn die APP gestartet wird. Die Anwendung befindet sich nun im _aktiven_ Zustand.
1. Drücken Sie auf dem Simulator oder auf dem Gerät die Start Schaltfläche, um die Anwendung in den Hintergrund zu bringen. `OnResignActivation`und `DidEnterBackground` werden aufgerufen, wenn die APP von `Active` in `Inactive` den und in den `Backgrounded` -Zustand übergeht. Da kein Anwendungscode für die Ausführung im Hintergrund festgelegt ist, wird die Anwendung im Arbeitsspeicher als angeh _alten betrachtet._
1. Navigieren Sie zurück zur APP, um Sie wieder in den Vordergrund zu bringen. `WillEnterForeground`und `OnActivated` werden beide aufgerufen:

    ![](application-lifecycle-demo-images/image4.png "In der Konsole gedruckte Zustandsänderungen")

    Die folgende Codezeile im Ansichts Controller wird ausgeführt, wenn die Anwendung den Vordergrund aus dem Hintergrund eingegeben hat und den auf dem Bildschirm angezeigten Text ändert:

    ```csharp
    UIApplication.Notifications.ObserveWillEnterForeground ((sender, args) => {
        label.Text = "Welcome back!";
    });
    ```

1. Klicken Sie auf die **Start** Schaltfläche, um die Anwendung in den Hintergrund zu versetzen. Doppel Tippen Sie dann auf die **Start** Schaltfläche, um den Anwendungs Umschalter zu aktivieren. Wischen Sie auf dem iPhone X vom unteren Bildschirmrand nach oben:

    [![Anwendungs] Umschaltung (application-lifecycle-demo-images/app-switcher-sml.png "Anwendungs") Umschaltung](application-lifecycle-demo-images/app-switcher.png#lightbox)
  
1. Suchen Sie die Anwendung im App-Switcher, und ziehen Sie Sie nach oben, um Sie zu entfernen (unter IOS 11, Long drücken Sie, bis die roten Symbole in der Ecke angezeigt werden):

    [![Zum Entfernen einer laufenden app schwenken](application-lifecycle-demo-images/app-switcher-swipe-sml.png "Zum Entfernen einer laufenden app schwenken")](application-lifecycle-demo-images/app-switcher-swipe.png#lightbox)

IOS beendet die Anwendung. Beachten Sie `WillTerminate` , dass nicht aufgerufen wird, da die _Anwendung bereits im_ Hintergrund angehalten wurde.

## <a name="related-links"></a>Verwandte Links

- [Lifecycledemo (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/lifecycledemo)
