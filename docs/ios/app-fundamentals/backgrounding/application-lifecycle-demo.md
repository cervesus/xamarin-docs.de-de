---
title: Application Lifecycle Demo für Xamarin.iOS
description: Dieses Dokument erläutert verschiedene Lebenszyklusereignisse behandelt, durch den Delegaten app in einer iOS-Anwendung veranschaulicht, wann und wie diese Ereignisse behandelt werden.
ms.prod: xamarin
ms.assetid: 5C8AACA6-49F8-4C6D-99C3-5F443C01B230
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 64c695065012e4bf796c219c260324d9b6278ca5
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783583"
---
# <a name="application-lifecycle-demo-for-xamarinios"></a>Application Lifecycle Demo für Xamarin.iOS

In diesem Abschnitt werden wir eine Anwendung zu untersuchen, die die vier Statuszustände für die Anwendung und die Rolle des veranschaulicht die `AppDelegate` Methoden in benachrichtigen der Anwendung der Zustände Änderungszeitpunkt abrufen. Die Anwendung wird bei jeder Änderung die app Status Updates an die Konsole drucken:

 [![](application-lifecycle-demo-images/image3.png "Die Beispiel-app")](application-lifecycle-demo-images/image3.png#lightbox)

 [![](application-lifecycle-demo-images/image4.png "Die app werden Updates auf der Konsole gedruckt, bei jeder Änderung die app Status")](application-lifecycle-demo-images/image4.png#lightbox)

## <a name="walkthrough"></a>Exemplarische Vorgehensweise


  1. Öffnen der _Lebenszyklus_ -Projekt in der _LifecycleDemo_ Lösung.
  1. Öffnen Sie die `AppDelegate` Klasse. Beachten Sie, dass wir Protokollierung, können die Lebenszyklusmethoden hinzugefügt haben um uns mitzuteilen, wann die Anwendung Zustandsänderung aus:

            ```chsarp
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

  1. Starten Sie die Anwendung im Simulator oder auf dem Gerät. `OnActivated` wird aufgerufen, wenn die Anwendung gestartet wird. Die Anwendung wird nun die _Active_ Zustand.
  1. Drücken Sie die POS1-Taste auf Simulator oder im Gerät auf die Anwendung auf den Hintergrund bringen. `OnResignActivation` und `DidEnterBackground` wird aufgerufen, als die app Übergänge von `Active` auf `Inactive` und in der `Backgrounded` Zustand. Da wir keine Anwendung keinen Code für die im Hintergrund ausgeführt gewährt haben, wird die Anwendung als _Suspended_ im Arbeitsspeicher.
  1. Navigieren Sie zurück an die app aus, um es wieder in den Vordergrund übereinstimmt. `WillEnterForeground` und `OnActivated` sowohl aufgerufen werden:

        ![](application-lifecycle-demo-images/image4.png "Statusänderungen auf der Konsole ausgegeben.")

    Beachten Sie, dass wir unsere modellansichtcontroller uns darüber informiert, dass die Anwendung im Vordergrund aus Hintergrund eingegeben hat die folgende Codezeile hinzugefügt:

        ```csharp
            UIApplication.Notifications.ObserveWillEnterForeground ((sender, args) => {
                    label.Text = "Welcome back!";
                });
        ```

1. Drücken Sie die **Home** Schaltfläche, um die Anwendung in den Hintergrund versetzen. Klicken Sie dann Doppeltippen der **Startseite** Schaltfläche, um die Anwendung Switcher anzuzeigen:
    
    ![](application-lifecycle-demo-images/app-switcher-.png "Der Switcher Anwendung")
  
1. Suchen Sie nach der Anwendung in der App-Switcher und Streifen Sie oben, um ihn zu entfernen:
    
    ![](application-lifecycle-demo-images/app-switcher-swipe-.png "Wischen batteriemodulbügel eine ausgeführte app") 
    
iOS wird die Anwendung beendet. Beachten Sie, dass `WillTerminate` wird nicht aufgerufen werden, weil es eine Anwendung beendet werden, die _Suspended_ im Hintergrund.

Nun, dass iOS-Anwendungszustände und Übergänge verstehen, führen wir einen Blick auf die verschiedenen Optionen für die im iOS backgrounding verfügbar.



## <a name="related-links"></a>Verwandte Links

- [LifecycleDemo(Part2) (Beispiel)](https://developer.xamarin.com/samples/monotouch/LifecycleDemo/)
