---
title: Dienste mit xamarin. Android gestartet
ms.prod: xamarin
ms.assetid: 8CC3A850-4CD2-4F93-98EE-AF3470794000
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 9f3ac33df34f5046fad6d392a6b7edf8a9a7f23f
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68644137"
---
# <a name="started-services-with-xamarinandroid"></a>Dienste mit xamarin. Android gestartet

## <a name="started-services-overview"></a>Übersicht über gestartete Dienste

Gestartete Dienste führen in der Regel eine Arbeitseinheit aus, ohne dem Client direkte Feedback oder Ergebnisse bereitzustellen. Ein Beispiel für eine Arbeitseinheit ist ein Dienst, der eine Datei auf einen Server hochlädt. Der Client führt eine Anforderung an einen Dienst aus, um eine Datei vom Gerät auf eine Website hochzuladen. Der Dienst lädt die Datei in Ruhe, auch wenn die APP keine Aktivitäten im Vordergrund hat, und beendet den Vorgang, wenn der Upload abgeschlossen ist. Es ist wichtig zu wissen, dass ein gestarteter Dienst im UI-Thread einer Anwendung ausgeführt wird. Dies bedeutet, dass bei einem Dienst, der den UI-Thread blockieren wird, ggf. Threads erstellt und gelöscht werden müssen.

Anders als bei einem gebundenen Dienst gibt es keinen Kommunikationskanal zwischen einem reinen, gestarteten Dienst und seinen Clients. Dies bedeutet, dass ein gestarteter Dienst einige andere Lebenszyklus Methoden implementiert als ein gebundener Dienst. In der folgenden Liste werden die allgemeinen Lebenszyklus Methoden in einem gestarteten Dienst hervorgehoben:

- `OnCreate`&ndash; Wird einmal aufgerufen, wenn der Dienst erstmalig gestartet wird. An dieser Stelle muss Initialisierungs Code implementiert werden.
- `OnBind`&ndash; Diese Methode muss von allen Dienst Klassen implementiert werden, aber ein gestarteter Dienst ist in der Regel nicht an ihn gebunden. Aus diesem Grund gibt ein gestarteter Dienst `null`nur zurück. Im Gegensatz dazu muss ein Hybrid Dienst (bei dem es sich um eine Kombination aus einem gebundenen Dienst und einem gestarteten Dienst handelt) `Binder` implementieren und einen für den Client zurückgeben.
- `OnStartCommand`Wird für jede Anforderung aufgerufen, um den Dienst zu starten, entweder als Reaktion auf `StartService` einen Aufruf von oder einen Neustart durch das System. &ndash; An dieser Stelle kann der Dienst eine beliebige Aufgabe mit langer Ausführungszeit starten. Die-Methode gibt `StartCommandResult` einen Wert zurück, der angibt, wie oder ob das System den Neustart des Dienstes nach einem Herunterfahren aufgrund von ungenügendem Arbeitsspeicher behandeln soll. Dieser Befehl wird im Haupt Thread ausgeführt. Diese Methode wird unten ausführlicher beschrieben.
- `OnDestroy`&ndash; Diese Methode wird aufgerufen, wenn der Dienst zerstört wird. Sie wird verwendet, um alle endgültigen Bereinigungs Vorgänge auszuführen.

Die wichtigste Methode für einen gestarteten Dienst ist die `OnStartCommand` -Methode. Sie wird immer dann aufgerufen, wenn der Dienst eine Anforderung zum erledigen von Aufgaben erhält. Der folgende Code Ausschnitt ist ein Beispiel für `OnStartCommand`: 

```csharp
public override StartCommandResult OnStartCommand (Android.Content.Intent intent, StartCommandFlags flags, int startId)
{
    // This method executes on the main thread of the application.
    Log.Debug ("DemoService", "DemoService started");
    ...
    return StartCommandResult.Sticky;
}
```

Der erste Parameter ist ein `Intent` Objekt, das die Metadaten zu der auszuführenden Arbeit enthält. Der zweite Parameter enthält einen `StartCommandFlags` Wert, der einige Informationen über den Methoden Aufrufwert bereitstellt. Dieser Parameter hat einen von zwei möglichen Werten:

- `StartCommandFlag.Redelivery`Dies bedeutet, dass eine erneute Übermittlung eines vorherigen `Intent` ist.`Intent` &ndash; Dieser Wert wird bereitgestellt, wenn der Dienst `StartCommandResult.RedeliverIntent` zurückgekehrt ist, aber angehalten wurde, bevor er ordnungsgemäß heruntergefahren werden konnte.
-`StartCommandFlag.Retry`Dieser Wert wird empfangen, wenn beim `OnStartCommand` vorherigen Versuch ein Fehler aufgetreten ist und Android versucht, den Dienst erneut mit der gleichen Absicht wie der vorherige fehlgeschlagene Versuch zu starten. &dash;
 
Der dritte Parameter ist schließlich ein ganzzahliger Wert, der für die Anwendung, die die Anforderung identifiziert, eindeutig ist. Es ist möglich, dass mehrere Aufrufer dasselbe Dienst Objekt aufrufen können. Dieser Wert wird verwendet, um eine Anforderung zu verknüpfen, um einen Dienst mit einer bestimmten Anforderung zum Starten eines dienstandens zu unterbinden. Dies wird im Abschnitt zum [Beenden des Dienstanbieter](#Stopping_the_Service)ausführlicher erläutert. 

Der Wert `StartCommandResult` wird vom Dienst als Vorschlag an Android zurückgegeben, was geschehen soll, wenn der Dienst aufgrund von Ressourceneinschränkungen abgebrochen wird. Es gibt drei mögliche Werte für `StartCommandResult`:

- **[Startcommandresult.](xref:Android.App.StartCommandResult.NotSticky)** &ndash; notkurzwert dieser Wert gibt an, dass der von ihm beendete Dienst nicht neu gestartet werden muss. Betrachten Sie als Beispiel einen Dienst, der Miniaturansichten für einen Katalog in einer APP generiert. Wenn der Dienst abgebrochen wird, ist es nicht entscheidend, die Miniatur &ndash; Ansicht sofort neu zu erstellen, wenn die Miniaturansicht das nächste Mal ausgeführt wird.
- **[Startcommandresult.](xref:Android.App.StartCommandResult.Sticky)** &ndash; Kurznotiz: Hiermit wird Android aufgefordert, den Dienst neu zu starten, aber nicht die letzte Absicht, die den Dienst gestartet hat. Wenn keine zu behandelnden Intents vorhanden sind, wird für den `null` Intent-Parameter ein bereitgestellt. Ein Beispiel hierfür könnte eine Music Player-App sein. der Dienst wird neu gestartet, um Musik wiederzugeben, aber der letzte Song wird wiedergegeben.
- **[StartCommandResult.RedeliverIntent](xref:Android.App.StartCommandResult.RedeliverIntent)** &ndash; This value is will tell Android to restart the service and re-deliver the last `Intent`. Ein Beispiel hierfür ist ein Dienst, der eine Datendatei für eine APP herunterlädt. Wenn der Dienst abgebrochen wird, muss die Datendatei noch heruntergeladen werden. Wenn der `StartCommandResult.RedeliverIntent`Dienst von Android zurückgegeben wird, wird er auch als Absicht (mit der URL der herunter zuladenden Datei) für den Dienst bereitgestellt. Dadurch kann der Download entweder neu gestartet oder fortgesetzt werden (abhängig von der exakten Implementierung des Codes).

Der vierte Wert für `StartCommandResult` &ndash; `StartCommandResult.ContinuationMask`ist. Dieser Wert wird von zurück `OnStartCommand` gegeben, und es wird beschrieben, wie Android den von ihm beendeten Dienst fortsetzt. Dieser Wert wird in der Regel nicht verwendet, um einen Dienst zu starten.

Die wichtigsten Lebenszyklus Ereignisse eines gestarteten Dienstanbieter werden in diesem Diagramm dargestellt: 

![Ein Diagramm, das die Reihenfolge anzeigt, in der die Lebenszyklus Methoden aufgerufen werden](started-services-images/started-service-01.png "Ein Diagramm, das die Reihenfolge anzeigt, in der die Lebenszyklus Methoden aufgerufen werden.")

<a name="Stopping_the_Service" />

## <a name="stopping-the-service"></a>Beenden des Dienstanbieter

Ein gestarteter Dienst wird weiterhin unbegrenzt ausgeführt. Android behält den Dienst bei, solange ausreichend Systemressourcen vorhanden sind. Entweder muss der Client den Dienst beenden, oder der Dienst kann sich selbst beenden, wenn die Arbeit abgeschlossen ist. Es gibt zwei Möglichkeiten, einen Dienst zu unterbinden: 

- **[Android.Content.Context.StopService()](xref:Android.Content.Context.StopService*)** &ndash; A client (such as an Activity) can request a service stop by calling the `StopService` method:

    ```csharp
    StopService(new Intent(this, typeof(DemoService));
    ```

- **[Android. app. Service. stopself ()](xref:Android.App.Service.StopSelf*)** Ein Dienst kann durch `StopSelf`Aufrufen von heruntergefahren werden: &ndash;

    ```csharp
    StopSelf();
    ```

### <a name="using-startid-to-stop-a-service"></a>Verwenden von startID zum Verhindern eines Dienstanbieter

Mehrere Aufrufer können anfordern, dass ein Dienst gestartet wird. Wenn eine ausstehende Start Anforderung vorhanden ist, kann der Dienst die `startId` verwenden, die `OnStartCommand` an den-Dienst übermittelt wird, um zu verhindern, dass der Dienst vorzeitig beendet wird. Der `startId` entspricht dem letzten Aufruf von `StartService`und wird jedes Mal, wenn er aufgerufen wird, inkrementiert. Wenn eine nachfolgende `StartService` Anforderung von noch nicht zu einem Aufruf von `OnStartCommand`geführt hat, kann der Dienst aufrufen `StopSelfResult`und dabei den aktuellen Wert `startId` übergeben, den er empfangen hat (anstatt einfach aufzurufen `StopSelf`). Wenn ein-Rückruf `StartService` noch nicht zu einem entsprechenden- `OnStartCommand`Rückruf geführt hat, beendet das System den Dienst nicht, `StopSelf` da der `startId` , der im-Befehl verwendet wird, nicht dem aktuellen `StartService` -Befehl entspricht.

## <a name="related-links"></a>Verwandte Links

- [Startedservicesdemo (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-servicesamples-startedservicesdemo)
- [Android.App.Service](xref:Android.App.Service)
- [Android.App.StartCommandFlags](xref:Android.App.StartCommandFlags)
- [Android.App.StartCommandResult](xref:Android.App.StartCommandResult)
- [Android.Content.BroadcastReceiver](xref:Android.Content.BroadcastReceiver)
- [Android.Content.Intent](xref:Android.Content.Intent)
- [Android.OS.Handler](xref:Android.OS.Handler)
- [Android.Widget.Toast](xref:Android.Widget.Toast)
- [Symbole der Status Leiste](https://developer.android.com/guide/practices/ui_guidelines/icon_design_status_bar.html)
