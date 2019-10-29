---
title: Dienste mit xamarin. Android gestartet
ms.prod: xamarin
ms.assetid: 8CC3A850-4CD2-4F93-98EE-AF3470794000
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: f5b5f8cf224c18852a0a0e7e4f591b49905ba026
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73024765"
---
# <a name="started-services-with-xamarinandroid"></a>Dienste mit xamarin. Android gestartet

## <a name="started-services-overview"></a>Übersicht über gestartete Dienste

Gestartete Dienste führen in der Regel eine Arbeitseinheit aus, ohne dem Client direkte Feedback oder Ergebnisse bereitzustellen. Ein Beispiel für eine Arbeitseinheit ist ein Dienst, der eine Datei auf einen Server hochlädt. Der Client führt eine Anforderung an einen Dienst aus, um eine Datei vom Gerät auf eine Website hochzuladen. Der Dienst lädt die Datei in Ruhe, auch wenn die APP keine Aktivitäten im Vordergrund hat, und beendet den Vorgang, wenn der Upload abgeschlossen ist. Es ist wichtig zu wissen, dass ein gestarteter Dienst im UI-Thread einer Anwendung ausgeführt wird. Dies bedeutet, dass bei einem Dienst, der den UI-Thread blockieren wird, ggf. Threads erstellt und gelöscht werden müssen.

Anders als bei einem gebundenen Dienst gibt es keinen Kommunikationskanal zwischen einem reinen, gestarteten Dienst und seinen Clients. Dies bedeutet, dass ein gestarteter Dienst einige andere Lebenszyklus Methoden implementiert als ein gebundener Dienst. In der folgenden Liste werden die allgemeinen Lebenszyklus Methoden in einem gestarteten Dienst hervorgehoben:

- `OnCreate` &ndash; beim ersten Starten des Dienstanbieter aufgerufen. An dieser Stelle muss Initialisierungs Code implementiert werden.
- `OnBind` &ndash; diese Methode von allen Dienst Klassen implementiert werden muss, aber ein gestarteter Dienst ist in der Regel nicht an ihn gebunden. Aus diesem Grund gibt ein gestarteter Dienst nur `null` zurück. Im Gegensatz dazu muss ein Hybrid Dienst (bei dem es sich um eine Kombination aus einem gebundenen Dienst und einem gestarteten Dienst handelt) implementieren und eine `Binder` für den Client zurückgeben.
- `OnStartCommand` &ndash; für jede Anforderung aufgerufen, um den Dienst zu starten, entweder als Reaktion auf einen Aufruf von `StartService` oder einen Neustart durch das System. An dieser Stelle kann der Dienst eine beliebige Aufgabe mit langer Ausführungszeit starten. Die Methode gibt einen `StartCommandResult` Wert zurück, der angibt, wie oder ob das System den Neustart des Dienstanbieter nach einem Herunterfahren aufgrund von ungenügendem Arbeitsspeicher behandeln soll. Dieser Befehl wird im Haupt Thread ausgeführt. Diese Methode wird unten ausführlicher beschrieben.
- `OnDestroy` &ndash; diese Methode aufgerufen wird, wenn der Dienst zerstört wird. Sie wird verwendet, um alle endgültigen Bereinigungs Vorgänge auszuführen.

Die wichtigste Methode für einen gestarteten Dienst ist die `OnStartCommand` Methode. Sie wird immer dann aufgerufen, wenn der Dienst eine Anforderung zum erledigen von Aufgaben erhält. Der folgende Code Ausschnitt ist ein Beispiel für `OnStartCommand`: 

```csharp
public override StartCommandResult OnStartCommand (Android.Content.Intent intent, StartCommandFlags flags, int startId)
{
    // This method executes on the main thread of the application.
    Log.Debug ("DemoService", "DemoService started");
    ...
    return StartCommandResult.Sticky;
}
```

Der erste Parameter ist ein `Intent` Objekt, das die Metadaten zu der auszuführenden Arbeit enthält. Der zweite Parameter enthält einen `StartCommandFlags` Wert, der Informationen zum Methoden aufrufen bereitstellt. Dieser Parameter hat einen von zwei möglichen Werten:

- `StartCommandFlag.Redelivery` &ndash; bedeutet dies, dass der `Intent` eine erneute Übermittlung eines vorherigen `Intent` ist. Dieser Wert wird bereitgestellt, wenn der Dienst `StartCommandResult.RedeliverIntent` zurückgegeben hat, aber angehalten wurde, bevor er ordnungsgemäß heruntergefahren werden konnte.
- `StartCommandFlag.Retry` &dash; dieser Wert empfangen, wenn ein vorheriger `OnStartCommand`-Befehl fehlgeschlagen ist und Android versucht, den Dienst erneut mit der gleichen Absicht wie der vorherige fehlgeschlagene Versuch zu starten.

Der dritte Parameter ist schließlich ein ganzzahliger Wert, der für die Anwendung, die die Anforderung identifiziert, eindeutig ist. Es ist möglich, dass mehrere Aufrufer dasselbe Dienst Objekt aufrufen können. Dieser Wert wird verwendet, um eine Anforderung zu verknüpfen, um einen Dienst mit einer bestimmten Anforderung zum Starten eines dienstandens zu unterbinden. Dies wird im Abschnitt zum [Beenden des Dienstanbieter](#Stopping_the_Service)ausführlicher erläutert. 

Der Wert `StartCommandResult` wird vom Dienst als Vorschlag an Android zurückgegeben, was geschehen soll, wenn der Dienst aufgrund von Ressourceneinschränkungen abgebrochen wird. Für `StartCommandResult` gibt es drei mögliche Werte:

- **[Startcommandresult. notinterval](xref:Android.App.StartCommandResult.NotSticky)** &ndash; dieser Wert weist Android an, dass der beendete Dienst nicht neu gestartet werden muss. Betrachten Sie als Beispiel einen Dienst, der Miniaturansichten für einen Katalog in einer APP generiert. Wenn der Dienst beendet wird, ist es nicht entscheidend, die Miniaturansicht sofort neu zu erstellen, &ndash; die Miniaturansicht beim nächsten Ausführen der APP neu erstellt werden kann.
- **[Startcommandresult.](xref:Android.App.StartCommandResult.Sticky)** kurz&ndash; Dies weist Android an, den Dienst neu zu starten, jedoch nicht, um die letzte Absicht zu übermitteln, die den Dienst gestartet hat. Wenn keine zu behandelnden Intents vorhanden sind, wird für den Intent-Parameter eine `null` bereitgestellt. Ein Beispiel hierfür könnte eine Music Player-App sein. der Dienst wird neu gestartet, um Musik wiederzugeben, aber der letzte Song wird wiedergegeben.
- **[Startcommandresult. redeliverintent](xref:Android.App.StartCommandResult.RedeliverIntent)** &ndash; dieser Wert gibt an, dass Android den Dienst neu starten und die letzte `Intent` erneut übermitteln muss. Ein Beispiel hierfür ist ein Dienst, der eine Datendatei für eine APP herunterlädt. Wenn der Dienst abgebrochen wird, muss die Datendatei noch heruntergeladen werden. Wenn der Dienst von Android `StartCommandResult.RedeliverIntent` zurückgegeben wird, wird er auch als Absicht (mit der URL der herunter zuladenden Datei) für den Dienst bereitgestellt. Dadurch kann der Download entweder neu gestartet oder fortgesetzt werden (abhängig von der exakten Implementierung des Codes).

Es gibt einen vierten Wert für `StartCommandResult` &ndash; `StartCommandResult.ContinuationMask`. Dieser Wert wird von `OnStartCommand` zurückgegeben, und es wird beschrieben, wie Android den von ihm beendeten Dienst fortsetzt. Dieser Wert wird in der Regel nicht verwendet, um einen Dienst zu starten.

Die wichtigsten Lebenszyklus Ereignisse eines gestarteten Dienstanbieter werden in diesem Diagramm dargestellt: 

![Ein Diagramm, das die Reihenfolge anzeigt, in der die Lebenszyklus Methoden aufgerufen werden](started-services-images/started-service-01.png "Ein Diagramm, das die Reihenfolge anzeigt, in der die Lebenszyklus Methoden aufgerufen werden.")

<a name="Stopping_the_Service" />

## <a name="stopping-the-service"></a>Beenden des Dienstanbieter

Ein gestarteter Dienst wird weiterhin unbegrenzt ausgeführt. Android behält den Dienst bei, solange ausreichend Systemressourcen vorhanden sind. Entweder muss der Client den Dienst beenden, oder der Dienst kann sich selbst beenden, wenn die Arbeit abgeschlossen ist. Es gibt zwei Möglichkeiten, einen Dienst zu unterbinden: 

- **[Android. Content. Context. StopService ()](xref:Android.Content.Context.StopService*)** &ndash; ein-Client (z. b. eine-Aktivität) einen Dienst Stopp durch Aufrufen der `StopService`-Methode anfordern kann:

    ```csharp
    StopService(new Intent(this, typeof(DemoService));
    ```

- **[Android. app. Service. stopself ()](xref:Android.App.Service.StopSelf*)** &ndash; ein Dienst durch Aufrufen der `StopSelf` heruntergefahren werden kann:

    ```csharp
    StopSelf();
    ```

### <a name="using-startid-to-stop-a-service"></a>Verwenden von startID zum Verhindern eines Dienstanbieter

Mehrere Aufrufer können anfordern, dass ein Dienst gestartet wird. Wenn eine ausstehende Start Anforderung vorliegt, kann der Dienst die an `OnStartCommand` über gebenden `startId` verwenden, um zu verhindern, dass der Dienst vorzeitig beendet wird. Der `startId` entspricht dem letzten Aufruf von `StartService` und wird jedes Mal, wenn er aufgerufen wird, inkrementiert. Wenn eine nachfolgende Anforderung an `StartService` noch nicht zu einem Aufruf von `OnStartCommand` geführt hat, kann der Dienst `StopSelfResult` aufrufen und dabei den neuesten Wert `startId` übergeben, die er empfangen hat (anstatt einfach `StopSelf` aufzurufen). Wenn ein aufzurufende `StartService` noch keinen entsprechenden `OnStartCommand` aufgerufen hat, wird der Dienst vom System nicht angehalten, da die im `StopSelf`-Befehl verwendete `startId` nicht dem aktuellen `StartService`-Befehl entspricht.

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
