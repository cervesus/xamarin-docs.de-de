---
title: Gestartete Dienste, die mit Xamarin.Android
ms.prod: xamarin
ms.assetid: 8CC3A850-4CD2-4F93-98EE-AF3470794000
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 9e7dabf87314d87ab5ab335c220c0e292e56073b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50110887"
---
# <a name="started-services-with-xamarinandroid"></a>Gestartete Dienste, die mit Xamarin.Android

## <a name="started-services-overview"></a>Übersicht über die gestartete Dienste

Gestartete Dienste führen eine Arbeitseinheit in der Regel ohne direkten Feedback oder Ergebnisse an dem Client. Ein Beispiel für eine Arbeitseinheit ist ein Dienst, der eine Datei auf einen Server hochgeladen. Der Client wird eine Anforderung an einen Dienst zum Hochladen einer Datei vom Gerät an eine Website erforderlich. Der Dienst wird automatisch die Datei hochladen, (selbst wenn die app keine Aktivitäten im Vordergrund hat), und sich selbst zu beenden, wenn der Upload abgeschlossen ist. Es ist wichtig zu wissen, dass ein Dienst gestarteter, die im UI-Thread einer Anwendung ausgeführt wird. Dies bedeutet, dass, wenn ein Dienst ist für Arbeit, die im UI-Thread blockiert wird, müssen sie erstellen und von Threads nach Bedarf verwerfen.

Im Gegensatz zu einem gebundenen Dienst besteht keine Kommunikationskanal zwischen einem "rein" gestarteter Dienst und seinen Clients zur Verfügung. Dies bedeutet, dass ein Dienst gestarteter, einige andere Methoden als ein gebundener Dienst implementiert wird. Die folgende Liste nennt die allgemeine Lebenszyklusmethoden in einem Dienst wurde gestartet:

* `OnCreate` &ndash; Wird aufgerufen, einmal beim ersten des Diensts Start. Dies ist, in dem Code zum Initialisieren der implementiert werden soll.
* `OnBind` &ndash; Diese Methode muss durch alle Dienstklassen, implementiert werden, jedoch ein gestarteter Dienst nicht in der Regel einen Client, der an ihn gebunden verfügt. Aus diesem Grund gibt ein gestarteter Dienst nur `null`. Im Gegensatz dazu ein hybriddienst (der Kombination aus einem gebundenen Dienst und ein Dienst gestartet ist) ist, implementieren und Zurückgeben einer `Binder` für den Client.
* `OnStartCommand` &ndash; Wird aufgerufen, für jede Anforderung zum Starten des Diensts, entweder als Reaktion auf einen Aufruf von `StartService` oder einen Neustart durch das System. Dies ist, in dem der Dienst lang andauernden Task beginnen kann. Gibt die Methode eine `StartCommandResult` Wert, der angibt wie oder ob der Neustart des Diensts nach einem Herunterfahren aufgrund von ungenügendem Arbeitsspeicher des Systems behandelt werden sollen. Dieser Aufruf erfolgt auf der Haupt-Thread. Diese Methode wird im folgenden ausführlicher beschrieben.
* `OnDestroy` &ndash; Diese Methode wird aufgerufen, wenn der Dienst zerstört wird. Es wird verwendet, um alle endgültigen führen Bereinigung erforderlich.

Die entscheidende Methode für einen Dienst gestartet ist die `OnStartCommand` Methode. Es wird jedes Mal aufgerufen werden der Dienst empfängt eine Anforderung ausgeführt. Der folgende Codeausschnitt ist ein Beispiel für `OnStartCommand`: 

```csharp
public override StartCommandResult OnStartCommand (Android.Content.Intent intent, StartCommandFlags flags, int startId)
{
    // This method executes on the main thread of the application.
    Log.Debug ("DemoService", "DemoService started");
    ...
    return StartCommandResult.Sticky;
}
```

Der erste Parameter ist ein `Intent` -Objekt, enthält die Metadaten zu den Aufgaben ausführen. Der zweite Parameter enthält einen `StartCommandFlags` -Wert, der einige Informationen über den Methodenaufruf bereitstellt. Dieser Parameter hat einen von zwei möglichen Werten:

* `StartCommandFlag.Redelivery` &ndash; Dies bedeutet, dass die `Intent` ein Re-Bereitstellung von einem vorherigen `Intent`. Dieser Wert wird bereitgestellt, wenn der Dienst zurückgegeben hätte `StartCommandResult.RedeliverIntent` aber beendet wurde, bevor er ordnungsgemäß heruntergefahren werden konnte.
* `StartCommandFlag.Retry` &dash; Dieser Wert wird empfangen, wenn eine vorherige `OnStartCommand` Fehler bei Aufruf und Android versucht, den Dienst erneut mit den gleichen Zweck wie die vorherigen fehlerhaften Versuch zu starten.
 
Der dritte Parameter ist schließlich ein ganzzahliger Wert, der für die Anwendung eindeutig ist, der Anforderung identifiziert. Es ist möglich, dass mehrere Aufrufer des gleichen Objekts aufgerufen werden können. Dieser Wert wird verwendet, um eine Anforderung zum Beenden eines Diensts mit einer angegebenen Anforderung zum Starten eines Diensts zuzuordnen. Es wird ausführlicher im Abschnitt erläutert werden [Beenden des Diensts](#Stopping_the_Service). 

Der Wert `StartCommandResult` wird vom Dienst zurückgegebenen als Vorschlag für Android dazu, was Sie tun, wenn der Dienst aufgrund von ressourcenbeschränkungen beendet wird. Es gibt drei mögliche Werte für `StartCommandResult`:

* **[StartCommandResult.NotSticky](https://developer.xamarin.com/api/field/Android.App.StartCommandResult.NotSticky/)**  &ndash; dieser Wert teilt Android, die es nicht erforderlich ist, den Dienst neu starten, die er beendet wurde. Ein Beispiel dafür sollten Sie einen Dienst, der Miniaturansichten für einen Katalog in einer app generiert. Wenn der Dienst beendet wird, es ist nicht entscheidend, um die Miniaturansicht sofort neu erstellen &ndash; die Miniaturansicht erstellt werden kann, das nächste Mal die app ausgeführt wird.
* **[StartCommandResult.Sticky](https://developer.xamarin.com/api/field/Android.App.StartCommandResult.Sticky/)**  &ndash; dadurch Android den Dienst neu starten, jedoch nicht die letzte Absicht zu liefern, die der Dienst gestartet wird. Wenn es keine ausstehende Intents sind verarbeiten, wird eine `null` wird für den Intent-Parameter angegeben werden. Ein Beispiel hierfür kann eine Musik-Player-app sein. der Dienst wird neu gestartet bereit zum Wiedergeben von Musik, aber der letzte Song abgespielt wird. 
* **[StartCommandResult.RedeliverIntent](https://developer.xamarin.com/api/field/Android.App.StartCommandResult.RedeliverIntent/)** &ndash; This value is will tell Android to restart the service and re-deliver the last `Intent`. Ein Beispiel hierfür ist ein Dienst, der eine Datei für eine app heruntergeladen. Wenn der Dienst beendet wird, muss die Datei weiterhin heruntergeladen werden. Durch Rückgabe `StartCommandResult.RedeliverIntent`, wenn Android des Diensts, die sie erhalten auch die Absicht Neustarten (die die URL der herunterzuladenden Datei enthält) an den Dienst. Dadurch wird ermöglicht, den Download zu starten oder fortsetzen (abhängig von der genauen Implementierung des Codes).

Es gibt eine vierte für Wert `StartCommandResult` &ndash; `StartCommandResult.ContinuationMask`. Dieser Wert wird zurückgegeben, indem `OnStartCommand` und es wird beschrieben, wie Android für den Dienst weiter, wird er beendet wurde. Dieser Wert ist nicht in der Regel verwendet, um einen Dienst zu starten.

Die Ereignisse der Lebenszyklus eines Diensts gestartet sind in diesem Diagramm dargestellt: 

![Das Diagramm zeigt die Reihenfolge, in dem die Lebenszyklusmethoden aufgerufen werden](started-services-images/started-service-01.png "das Diagramm zeigt die Reihenfolge, in dem die Lebenszyklusmethoden aufgerufen werden.")


<a name="Stopping_the_Service" />

## <a name="stopping-the-service"></a>Beenden des Diensts

Ein Dienst gestarteter wird weiterhin auf unbestimmte Zeit ausgeführt; Android bleibt der Dienst ausgeführt, solange es genügend Systemressourcen. Der Client muss den Dienst beenden oder den Dienst selbst beenden kann, wenn er seine Arbeit abgeschlossen ist. Es gibt zwei Möglichkeiten, um einen Dienst zu beenden: 
 
* **[Android.Content.Context.StopService()](https://developer.xamarin.com/api/member/Android.Content.Context.StopService/p/Android.Content.Intent/)** &ndash; A client (such as an Activity) can request a service stop by calling the `StopService` method: 

    ```csharp
    StopService(new Intent(this, typeof(DemoService));
    ```

* **[Android.App.Service.StopSelf()](https://developer.xamarin.com/api/member/Android.App.Service.StopSelf()/)**  &ndash; ein Diensts kann sich selbst zu beenden durch Aufrufen der `StopSelf`:

    ```csharp
    StopSelf();
    ```
    
### <a name="using-startid-to-stop-a-service"></a>Verwenden von StartId zum Beenden eines Diensts

Ein Dienst gestartet werden, können mehrere Aufrufer anfordern. Der Dienst kann verwenden, liegt eine ausstehende startanforderung, der `startId` übergebene in `OnStartCommand` verhindert, dass der Dienst wurde vorzeitig abgebrochen wird. Die `startId` entspricht dem letzten Aufruf von `StartService`, und wird bei jedem Aufruf erhöht. Aus diesem Grund, wenn eine nachfolgende Anforderung an `StartService` hat noch nicht in einem Aufruf von geführt `OnStartCommand`, den Dienst aufrufen kann `StopSelfResult`, und übergeben sie den aktuellen Wert des `startId` empfangen wurde (anstelle von Aufrufen `StopSelf`). Wenn ein Aufruf von `StartService` hat noch nicht in einem entsprechenden Aufruf geführt `OnStartCommand`, das System wird der Dienst nicht angehalten, da die `startId` in verwendet die `StopSelf` Aufruf nicht auf die neueste entsprechen `StartService` aufrufen.


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
