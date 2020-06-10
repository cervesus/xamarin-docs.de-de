---
title: Dienste mit xamarin. Android gestartet
ms.prod: xamarin
ms.assetid: 8CC3A850-4CD2-4F93-98EE-AF3470794000
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 74226aff2ae135144172a06be5e7869c5cd8e408
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84568450"
---
# <a name="started-services-with-xamarinandroid"></a>Dienste mit xamarin. Android gestartet

## <a name="started-services-overview"></a>Übersicht über gestartete Dienste

Gestartete Dienste führen in der Regel eine Arbeitseinheit aus, ohne dem Client direkte Feedback oder Ergebnisse bereitzustellen. Ein Beispiel für eine Arbeitseinheit ist ein Dienst, der eine Datei auf einen Server hochlädt. Der Client führt eine Anforderung an einen Dienst aus, um eine Datei vom Gerät auf eine Website hochzuladen. Der Dienst lädt die Datei in Ruhe, auch wenn die APP keine Aktivitäten im Vordergrund hat, und beendet den Vorgang, wenn der Upload abgeschlossen ist. Es ist wichtig zu wissen, dass ein gestarteter Dienst im UI-Thread einer Anwendung ausgeführt wird. Dies bedeutet, dass bei einem Dienst, der den UI-Thread blockieren wird, ggf. Threads erstellt und gelöscht werden müssen.

Anders als bei einem gebundenen Dienst gibt es keinen Kommunikationskanal zwischen einem reinen, gestarteten Dienst und seinen Clients. Dies bedeutet, dass ein gestarteter Dienst einige andere Lebenszyklus Methoden implementiert als ein gebundener Dienst. In der folgenden Liste werden die allgemeinen Lebenszyklus Methoden in einem gestarteten Dienst hervorgehoben:

- `OnCreate`&ndash;Wird einmal aufgerufen, wenn der Dienst erstmalig gestartet wird. An dieser Stelle muss Initialisierungs Code implementiert werden.
- `OnBind`&ndash;Diese Methode muss von allen Dienst Klassen implementiert werden, aber ein gestarteter Dienst ist in der Regel nicht an ihn gebunden. Aus diesem Grund gibt ein gestarteter Dienst nur zurück `null` . Im Gegensatz dazu muss ein Hybrid Dienst (bei dem es sich um eine Kombination aus einem gebundenen Dienst und einem gestarteten Dienst handelt) implementieren und einen `Binder` für den Client zurückgeben.
- `OnStartCommand`&ndash;Wird für jede Anforderung aufgerufen, um den Dienst zu starten, entweder als Reaktion auf einen Aufruf `StartService` von oder einen Neustart durch das System. An dieser Stelle kann der Dienst eine beliebige Aufgabe mit langer Ausführungszeit starten. Die-Methode gibt einen `StartCommandResult` Wert zurück, der angibt, wie oder ob das System den Neustart des Dienstes nach einem Herunterfahren aufgrund von ungenügendem Arbeitsspeicher behandeln soll. Dieser Befehl wird im Haupt Thread ausgeführt. Diese Methode wird unten ausführlicher beschrieben.
- `OnDestroy`&ndash;Diese Methode wird aufgerufen, wenn der Dienst zerstört wird. Sie wird verwendet, um alle endgültigen Bereinigungs Vorgänge auszuführen.

Die wichtigste Methode für einen gestarteten Dienst ist die- `OnStartCommand` Methode. Sie wird immer dann aufgerufen, wenn der Dienst eine Anforderung zum erledigen von Aufgaben erhält. Der folgende Code Ausschnitt ist ein Beispiel für `OnStartCommand` : 

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

- `StartCommandFlag.Redelivery`&ndash;Dies bedeutet, dass `Intent` eine erneute Übermittlung eines vorherigen ist `Intent` . Dieser Wert wird bereitgestellt, wenn der Dienst zurückgekehrt ist, `StartCommandResult.RedeliverIntent` aber angehalten wurde, bevor er ordnungsgemäß heruntergefahren werden konnte.
-`StartCommandFlag.Retry`&dash;Dieser Wert wird empfangen, wenn beim vorherigen Versuch ein Fehler aufgetreten `OnStartCommand` ist und Android versucht, den Dienst erneut mit der gleichen Absicht wie der vorherige fehlgeschlagene Versuch zu starten.

Der dritte Parameter ist schließlich ein ganzzahliger Wert, der für die Anwendung, die die Anforderung identifiziert, eindeutig ist. Es ist möglich, dass mehrere Aufrufer dasselbe Dienst Objekt aufrufen können. Dieser Wert wird verwendet, um eine Anforderung zu verknüpfen, um einen Dienst mit einer bestimmten Anforderung zum Starten eines dienstandens zu unterbinden. Dies wird im Abschnitt zum [Beenden des Dienstanbieter](#Stopping_the_Service)ausführlicher erläutert. 

Der Wert `StartCommandResult` wird vom Dienst als Vorschlag an Android zurückgegeben, was geschehen soll, wenn der Dienst aufgrund von Ressourceneinschränkungen abgebrochen wird. Es gibt drei mögliche Werte für `StartCommandResult` :

- **[Startcommandresult. notkurzwert](xref:Android.App.StartCommandResult.NotSticky)** &ndash; dieser Wert gibt an, dass der von ihm beendete Dienst nicht neu gestartet werden muss. Betrachten Sie als Beispiel einen Dienst, der Miniaturansichten für einen Katalog in einer APP generiert. Wenn der Dienst abgebrochen wird, ist es nicht entscheidend, die Miniaturansicht sofort neu zu erstellen, wenn die &ndash; Miniaturansicht das nächste Mal ausgeführt wird.
- **[Startcommandresult.](xref:Android.App.StartCommandResult.Sticky)** Kurznotiz: Hiermit &ndash; wird Android aufgefordert, den Dienst neu zu starten, aber nicht die letzte Absicht, die den Dienst gestartet hat. Wenn keine zu behandelnden Intents vorhanden sind, `null` wird für den Intent-Parameter ein bereitgestellt. Ein Beispiel hierfür könnte eine Music Player-App sein. der Dienst wird neu gestartet, um Musik wiederzugeben, aber der letzte Song wird wiedergegeben.
- **[Startcommandresult. redeliverintent](xref:Android.App.StartCommandResult.RedeliverIntent)** &ndash; dieser Wert gibt an, dass Android den Dienst neu starten und die letzte erneut übermitteln muss `Intent` . Ein Beispiel hierfür ist ein Dienst, der eine Datendatei für eine APP herunterlädt. Wenn der Dienst abgebrochen wird, muss die Datendatei noch heruntergeladen werden. `StartCommandResult.RedeliverIntent`Wenn der Dienst von Android zurückgegeben wird, wird er auch als Absicht (mit der URL der herunter zuladenden Datei) für den Dienst bereitgestellt. Dadurch kann der Download entweder neu gestartet oder fortgesetzt werden (abhängig von der exakten Implementierung des Codes).

Der vierte Wert für ist `StartCommandResult` &ndash; `StartCommandResult.ContinuationMask` . Dieser Wert wird von zurückgegeben, `OnStartCommand` und es wird beschrieben, wie Android den von ihm beendeten Dienst fortsetzt. Dieser Wert wird in der Regel nicht verwendet, um einen Dienst zu starten.

Die wichtigsten Lebenszyklus Ereignisse eines gestarteten Dienstanbieter werden in diesem Diagramm dargestellt: 

![Ein Diagramm, das die Reihenfolge anzeigt, in der die Lebenszyklus Methoden aufgerufen werden](started-services-images/started-service-01.png "Ein Diagramm, das die Reihenfolge anzeigt, in der die Lebenszyklus Methoden aufgerufen werden.")

<a name="Stopping_the_Service"></a>

## <a name="stopping-the-service"></a>Beenden des Dienstanbieter

Ein gestarteter Dienst wird weiterhin unbegrenzt ausgeführt. Android behält den Dienst bei, solange ausreichend Systemressourcen vorhanden sind. Entweder muss der Client den Dienst beenden, oder der Dienst kann sich selbst beenden, wenn die Arbeit abgeschlossen ist. Es gibt zwei Möglichkeiten, einen Dienst zu unterbinden: 

- **[Android. Content. Context. StopService ()](xref:Android.Content.Context.StopService*)** &ndash; Ein Client (z. b. eine Aktivität) kann einen Dienst beenden, indem er die- `StopService` Methode aufrufen:

    ```csharp
    StopService(new Intent(this, typeof(DemoService));
    ```

- **[Android. app. Service. stopself ()](xref:Android.App.Service.StopSelf*)** &ndash; Ein Dienst kann durch Aufrufen von heruntergefahren werden `StopSelf` :

    ```csharp
    StopSelf();
    ```

### <a name="using-startid-to-stop-a-service"></a>Verwenden von startID zum Verhindern eines Dienstanbieter

Mehrere Aufrufer können anfordern, dass ein Dienst gestartet wird. Wenn eine ausstehende Start Anforderung vorhanden ist, kann der Dienst die verwenden, die an den-Dienst `startId` übermittelt wird, `OnStartCommand` um zu verhindern, dass der Dienst vorzeitig beendet wird. Der `startId` entspricht dem letzten Aufruf von `StartService` und wird jedes Mal, wenn er aufgerufen wird, inkrementiert. Wenn eine nachfolgende Anforderung von `StartService` noch nicht zu einem Aufruf von geführt hat `OnStartCommand` , kann der Dienst aufrufen `StopSelfResult` und dabei den aktuellen Wert übergeben, den `startId` er empfangen hat (anstatt einfach aufzurufen `StopSelf` ). Wenn ein-Rückruf `StartService` noch nicht zu einem entsprechenden-Rückruf geführt hat `OnStartCommand` , beendet das System den Dienst nicht, da der, der im-Befehl `startId` verwendet `StopSelf` wird, nicht dem aktuellen-Befehl entspricht `StartService` .

## <a name="related-links"></a>Verwandte Links

- [Startedservicesdemo (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-servicesamples-startedservicesdemo)
- [Android. app. Service](xref:Android.App.Service)
- [Android. app. startcommandflags](xref:Android.App.StartCommandFlags)
- [Android. app. startcommandresult](xref:Android.App.StartCommandResult)
- [Android. Content. broadcastreceiver](xref:Android.Content.BroadcastReceiver)
- [Android. Content. Intent](xref:Android.Content.Intent)
- [Android. OS. Handler](xref:Android.OS.Handler)
- [Android. Widget. Toast](xref:Android.Widget.Toast)
- [Symbole der Status Leiste](https://developer.android.com/guide/practices/ui_guidelines/icon_design_status_bar.html)
