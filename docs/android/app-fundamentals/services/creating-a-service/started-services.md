---
title: Gestarteten Diensten mit Xamarin.Android
ms.topic: article
ms.prod: xamarin
ms.assetid: 8CC3A850-4CD2-4F93-98EE-AF3470794000
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: b6c0e67e3411aa5b7846a1b7bc0de2473a3546fd
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="started-services-with-xamarinandroid"></a>Gestarteten Diensten mit Xamarin.Android

## <a name="started-services-overview"></a>Gestarteten Diensten (Übersicht)

Normalerweise führen gestartete Diensten eine Arbeitseinheit ohne Angabe von jeder direktes Feedback oder die Ergebnisse an dem Client. Ein Beispiel für eine Arbeitseinheit ist ein Dienst, der eine Datei auf einen Server hochgeladen. Der Client wird eine Anforderung an einen Dienst zum Hochladen einer Datei vom Gerät zu einer Website vornehmen. Der Dienst wird – die Datei hochzuladen, (selbst wenn die app keine Aktivitäten in den Vordergrund gestellt wurde), und beenden Sie selbst, wenn der Upload abgeschlossen ist. Es ist wichtig zu beachten, dass es sich bei ein gestarteten Dienst auf dem UI-Thread einer Anwendung ausgeführt wird. Dies bedeutet, dass wenn ein Dienst ist für die Ausführung der Arbeit, die im UI-Thread blockiert, müssen sie erstellen und löschen Sie nach Bedarf Threads.

Im Gegensatz zu einem gebundenen Dienst besteht keine Kommunikationskanal zwischen einem "rein" gestarteten Dienst und seinen Clients zur Verfügung. Dies bedeutet, dass es sich bei ein gestarteten Dienst einige andere Lebenszyklusmethoden als Hintergrunddienst gebundenen implementiert wird. Die folgende Liste nennt die allgemeine Lebenszyklusmethoden in einem gestarteten Dienst:

* `OnCreate` &ndash; Wird aufgerufen, einmal, wenn der Dienst zuerst gestartet wurde. Dies ist, in dem Initialisierungscode implementiert werden soll.
* `OnBind` &ndash; Diese Methode muss durch alle Dienstklassen implementiert werden, aber ein gestarteten Dienst nicht in der Regel einen Client, der an es gebunden haben. Aus diesem Grund gibt ein gestarteten Dienst nur `null`. Im Gegensatz dazu wurde ein hybriddienst (also die Kombination aus einem gebundenen Dienst und einem gestarteten Dienst) zu implementieren und Zurückgeben einer `Binder` für den Client.
* `OnStartCommand` &ndash; Wird aufgerufen, für jede Anforderung zum Starten des Diensts, entweder als Antwort auf einen Aufruf von `StartService` oder einen Neustart durch das System. Dies ist, in dem der Dienst eine lang dauernde Aufgabe beginnen können. Die Methode gibt ein `StartCommandResult` Wert, der angibt wie oder wenn der Neustart des Diensts nach einem Herunterfahren aufgrund von ungenügendem Arbeitsspeicher des Systems behandelt werden sollen. Dieser Aufruf erfolgt auf der Haupt-Thread. Diese Methode wird im folgenden ausführlicher beschrieben.
* `OnDestroy` &ndash; Diese Methode wird aufgerufen, wenn der Dienst zerstört wird. Es wird verwendet, um alle endgültigen ausführen Cleanup erforderlich.

Die Methode einem gestarteten Dienst wichtige ist die `OnStartCommand` Methode. Es wird jedes Mal aufgerufen werden der Dienst empfängt eine Anforderung Aufgaben ausführen. Der folgende Codeausschnitt ist ein Beispiel für `OnStartCommand`: 

```csharp
public override StartCommandResult OnStartCommand (Android.Content.Intent intent, StartCommandFlags flags, int startId)
{
    // This method executes on the main thread of the application.
    Log.Debug ("DemoService", "DemoService started");
    ...
    return StartCommandResult.Sticky;
}
```

Der erste Parameter ist ein `Intent` Objekt, enthält die Metadaten zu den Aufgaben ausführen. Der zweite Parameter enthält einen `StartCommandFlags` -Wert, der einige Informationen zum Aufruf Methode enthält. Dieser Parameter verfügt über einen von zwei möglichen Werten:

* `StartCommandFlag.Redelivery` &ndash; Dies bedeutet, dass die `Intent` ist eine Re-Übermittlung von einer früheren `Intent`. Dieser Wert wird bereitgestellt, wenn der Dienst zurückgegeben hatte `StartCommandResult.RedeliverIntent` jedoch beendet wurde, bevor er ordnungsgemäß heruntergefahren werden konnte.
* `StartCommandFlag.Retry` &dash; Dieser Wert empfangen wird, wenn eine vorherige `OnStartCommand` Fehler beim Aufruf und Android versucht, den Dienst erneut mit den gleichen Zweck als die vorherigen fehlerhaften Versuch zu starten.
 
Der dritte Parameter ist schließlich, einen ganzzahligen Wert, der für die Anwendung eindeutig ist, der die Anforderung identifiziert. Es ist möglich, dass mehrere Aufrufer das gleiche Dienstobjekt aufrufen können. Dieser Wert wird verwendet, um eine Anforderung zum Beenden eines Diensts mit einer angegebenen Anforderung zum Starten eines Diensts zuzuordnen. Sie wird im Abschnitt ausführlicher besprochen werden [Beenden des Diensts](#Stopping_the_Service). 

Der Wert `StartCommandResult` wird vom Dienst zurückgegebenen als einen Vorschlag zu Android zu tun, wenn der Dienst aufgrund von ressourceneinschränkungen abgebrochen wird. Es gibt drei mögliche Werte für `StartCommandResult`:

* **[StartCommandResult.NotSticky](https://developer.xamarin.com/api/field/Android.App.StartCommandResult.NotSticky/)**  &ndash; dieser Wert wird angewiesen, Android, die es nicht erforderlich ist, den Dienst neu starten, die er abgebrochen wurde. Ein Beispiel dafür sollten Sie einen Dienst, der für einen Katalog Miniaturansichten in einer app generiert. Wenn der Dienst beendet wird, ist es nicht entscheidend, um die Miniaturansicht sofort neu zu erstellen &ndash; die Miniaturansicht kann neu erstellt das nächste Mal die app ausgeführt wird.
* **[StartCommandResult.Sticky](https://developer.xamarin.com/api/field/Android.App.StartCommandResult.Sticky/)**  &ndash; das weist darauf hin, Android, die den Dienst neu starten, aber nicht für die letzten Absicht zuzustellen, die der Dienst gestartet wurde. Wenn keine ausstehende Intents behandelt, es gibt eine `null` für den beabsichtigten Parameter bereitgestellt werden. Ein Beispiel hierfür kann eine Musik-Player-app werden; der Dienst startet bereit Musik wiedergeben, aber es wird den letzten Titel wiedergegeben. 
* **[StartCommandResult.RedeliverIntent](https://developer.xamarin.com/api/field/Android.App.StartCommandResult.RedeliverIntent/)** &ndash; This value is will tell Android to restart the service and re-deliver the last `Intent`. Ein Beispiel hierfür ist ein Dienst, der eine Datendatei für eine app heruntergeladen. Wenn der Dienst beendet wird, muss die Datendatei noch heruntergeladen werden. Durch zurückgeben `StartCommandResult.RedeliverIntent`, wenn Android startet den Dienst auch die Absicht ermöglicht (die die URL der herunterzuladenden Datei enthält) an den Dienst neu. Dadurch kann den Download neu starten oder fortsetzen (je nach genauen Implementierung des Codes).

Es wird eine vierte Wert `StartCommandResult` &ndash; `StartCommandResult.ContinuationMask`. Dieser Wert wird zurückgegeben, indem Sie `OnStartCommand` und es wird beschrieben, wie Android der Dienst wird fortgesetzt, es wurde abgebrochen. Dieser Wert wird nicht in der Regel verwendet, um einen Dienst zu starten.

In diesem Diagramm werden die wichtigsten Lebenszyklusereignisse von einem gestarteten Dienst gezeigt: 

![Diagramm mit der Reihenfolge, in dem die Lebenszyklusmethoden heißen](started-services-images/started-service-01.png "Diagramm mit der Reihenfolge, in der Lebenszyklusmethoden aufgerufen werden.")


## <a name="stopping-the-service"></a>Beenden des Diensts

Ein gestarteten Dienst wird weiterhin auf unbestimmte Zeit ausgeführt; Android muss der Dienst ausgeführt werden, solange genügend Systemressourcen vorhanden sind. Der Client muss den Dienst beenden oder der Dienst möglicherweise nicht mehr selbst wenn ihre Arbeit abgeschlossen ist. Es gibt zwei Möglichkeiten, einen Dienst zu beenden: 
 
* **[Android.Content.Context.StopService()](https://developer.xamarin.com/api/member/Android.Content.Context.StopService/p/Android.Content.Intent/)** &ndash; A client (such as an Activity) can request a service stop by calling the `StopService` method: 

    ```csharp
    StopService(new Intent(this, typeof(DemoService));
    ```

* **[Android.App.Service.StopSelf()](https://developer.xamarin.com/api/member/Android.App.Service.StopSelf()/)** &ndash; A service may shut itself down by invoking the `StopSelf`:

    ```csharp
    StopSelf();
    ```
    
### <a name="using-startid-to-stop-a-service"></a>Verwenden von StartId zum Beenden eines Diensts

Mehrere Aufrufer können anfordern, dass ein Dienst gestartet werden. Ist eine startanforderung für den ausstehenden, der Dienst kann anhand der `startId` in übergebenen `OnStartCommand` um zu verhindern, dass den Dienst wurde vorzeitig beendet wird. Die `startId` entspricht dem letzten Aufruf von `StartService`, und inkrementiert wird jedes Mal aufgerufen wird. Daher, wenn eine nachfolgende Anforderung zum `StartService` hat noch nicht in einem Aufruf geführt `OnStartCommand`, den Dienst aufrufen kann `StopSelfResult`, und übergeben sie den letzten Wert des `startId` empfangen wurde (statt einfach aufzurufen `StopSelf`). Wenn ein Aufruf von `StartService` hat noch nicht in einem entsprechenden Aufruf geführt `OnStartCommand`, das System wird den Dienst nicht beendet, da die `startId` verwendet der `StopSelf` entsprechen Aufruf nicht auf die neueste `StartService` aufrufen.


## <a name="related-links"></a>Verwandte Links

- [StartedServicesDemo (Beispiel)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/StartedServicesDemo/)
- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service)
- [Android.App.StartCommandFlags](https://developer.xamarin.com/api/type/Android.App.StartCommandFlags)
- [Android.App.StartCommandResult](https://developer.xamarin.com/api/type/Android.App.StartCommandResult)
- [Android.Content.BroadcastReceiver](https://developer.xamarin.com/api/type/Android.Content.BroadcastReceiver/)
- [Android.Content.Intent](https://developer.xamarin.com/api/type/Android.Content.Intent)
- [Android.OS.Handler](https://developer.xamarin.com/api/type/Android.OS.Handler/)
- [Android.Widget.Toast](https://developer.xamarin.com/api/type/Android.Widget.Toast/)
- [Statusleiste-Symbole](http://developer.android.com/guide/practices/ui_guidelines/icon_design_status_bar.html)
