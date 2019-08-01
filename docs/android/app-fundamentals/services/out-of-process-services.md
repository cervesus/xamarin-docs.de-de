---
title: Ausführen von Android-Diensten in Remote Prozessen
description: Im Allgemeinen werden alle Komponenten in einer Android-Anwendung im selben Prozess ausgeführt. Android-Dienste sind eine wichtige Ausnahme darin, dass Sie so konfiguriert werden können, dass Sie in ihren eigenen Prozessen ausgeführt werden und gemeinsam mit anderen Anwendungen, einschließlich der von anderen Android-Entwicklern, freigegeben werden. In dieser Anleitung wird erläutert, wie ein Android-Remote Dienst mithilfe von xamarin erstellt und verwendet wird.
ms.prod: xamarin
ms.assetid: 27A2E972-A690-480B-B31D-5EF1F74F673C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 360ea18de0c9d30988d63602ba3c17c3d00ed83a
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68644091"
---
# <a name="running-android-services-in-remote-processes"></a>Ausführen von Android-Diensten in Remote Prozessen

_Im Allgemeinen werden alle Komponenten in einer Android-Anwendung im selben Prozess ausgeführt. Android-Dienste sind eine wichtige Ausnahme darin, dass Sie so konfiguriert werden können, dass Sie in ihren eigenen Prozessen ausgeführt werden und gemeinsam mit anderen Anwendungen, einschließlich der von anderen Android-Entwicklern, freigegeben werden. In dieser Anleitung wird erläutert, wie ein Android-Remote Dienst mithilfe von xamarin erstellt und verwendet wird._

## <a name="out-of-process-services-overview"></a>Übersicht über Out-of-Process Services

Wenn eine Anwendung gestartet wird, erstellt Android einen Prozess, in dem die Anwendung ausgeführt wird. In der Regel werden alle Komponenten, die von der Anwendung in diesem Prozess ausgeführt werden. Android-Dienste sind eine wichtige Ausnahme darin, dass Sie so konfiguriert werden können, dass Sie in ihren eigenen Prozessen ausgeführt werden und gemeinsam mit anderen Anwendungen, einschließlich der von anderen Android-Entwicklern, freigegeben werden. Diese Dienst Typen werden als _Remote Dienste_ oder _out-of-Process-Dienste_bezeichnet. Der Code für diese Dienste wird im gleichen APK wie die Hauptanwendung enthalten sein. Wenn der Dienst gestartet wird, erstellt Android jedoch nur einen neuen Prozess für diesen Dienst. Im Gegensatz dazu wird ein Dienst, der im gleichen Prozess wie der Rest der Anwendung ausgeführt wird, manchmal auch als _lokaler Dienst_bezeichnet.

Im Allgemeinen ist es nicht erforderlich, dass eine Anwendung einen Remote Dienst implementiert. Ein lokaler Dienst ist in vielen Fällen ausreichend (und wünschenswert) für die Anforderungen einer App. Ein Prozess außerhalb des Prozesses hat seinen eigenen Speicherplatz, der von Android verwaltet werden muss. Dies führt zwar zu einem höheren Verwaltungsaufwand für die gesamte Anwendung, es gibt jedoch einige Szenarien, in denen es vorteilhaft sein kann, einen Dienst in einem eigenen Prozess auszuführen:

1. **Freigabe Funktionalität** &ndash; Einige Anwendungsentwickler verfügen möglicherweise über mehrere apps und Funktionen, die von allen Anwendungen gemeinsam genutzt werden. Das Verpacken dieser Funktionalität in einen Android-Dienst, der in einem eigenen Prozess ausgeführt wird, kann die Anwendungs Wartung vereinfachen. Es ist auch möglich, den Dienst in einem eigenen eigenständigen APK zu verpacken und separat vom Rest der Anwendung bereitzustellen.
2. **Verbessern der Benutzer** Leistung &ndash; Es gibt zwei Möglichkeiten, wie ein Prozess außerhalb des Prozesses die Benutzer Leistung der Anwendung verbessern kann. Die erste Möglichkeit ist die Speicherverwaltung. Wenn ein Garbage Collection (GC)-Cycle auftritt, hält Android alle Aktivitäten im Prozess an, bis die GC vollständig ist. Der Benutzer kann diese Pause als "Stutter" oder "Jank" betrachten. Wenn ein Dienst im eigenen Prozess ausgeführt wird, ist es der angehaltene Dienst Prozess, nicht der Anwendungsprozess. Diese Pause ist für den Benutzer deutlich weniger bemerkbar, da der Anwendungsprozess (und damit auch die Benutzeroberfläche) nicht angehalten wird.

    Wenn die Arbeitsspeicher Anforderungen eines Prozesses zu groß werden, kann Android den Prozess beenden, um Ressourcen für das Gerät freizugeben. Wenn ein Dienst über einen hohen Speicherbedarf verfügt und im gleichen Prozess wie die Benutzeroberfläche ausgeführt wird, wird die Benutzeroberfläche beim Erzwingen der Wiedergabe dieser Ressourcen von Android heruntergefahren, und der Benutzer wird gezwungen, die APP zu starten. Wenn jedoch ein Dienst, der in einem eigenen Prozess ausgeführt wird, von Android heruntergefahren wird, bleibt der UI-Prozess unberührt. Die Benutzeroberfläche kann den Dienst binden (und neu starten), transparent für den Benutzer und Wiederherstellen der normalen Funktionsweise.

3. **Verbessern der Anwendungsleistung** &ndash; Der UI-Prozess wird möglicherweise unabhängig vom Dienst Prozess beendet oder heruntergefahren. Wenn Sie lange Start Tasks in einen Prozess außerhalb des Prozesses verschieben, ist es möglich, dass sich die Startzeit der Benutzeroberfläche verbessert hat (vorausgesetzt, dass der Dienst Prozess zwischen dem Start der Benutzeroberfläche aktiv bleibt).

In vielerlei Hinsicht ist das Binden an einen Dienst, der in einem anderen Prozess ausgeführt wird, identisch mit der [Bindung an einen lokalen Dienst](~/android/app-fundamentals/services/creating-a-service/bound-services.md). Der Client wird `BindService` aufgerufen, um den Dienst zu binden (und ggf. zu starten). Ein `Android.OS.IServiceConnection` -Objekt wird erstellt, um die Verbindung zwischen dem Client und dem Dienst zu verwalten. Wenn der Client erfolgreich an den Dienst gebunden wird, gibt Android ein Objekt über das `IServiceConnection` zurück, das zum Aufrufen von Methoden für den Dienst verwendet werden kann. Der Client interagiert dann mit dem Dienst und verwendet dieses Objekt. Um dies zu überprüfen, finden Sie hier die Schritte zum Binden an einen Dienst:

* **Absicht erstellen** &ndash; Zum Binden an den Dienst muss eine explizite Absicht verwendet werden.
* **Implementieren und instanziieren Sie `IServiceConnection` ein Objekt** &ndash; , das das `IServiceConnection` Objekt als Vermittler zwischen dem Client und dem Dienst fungiert.  Er ist für die Überwachung der Verbindung zwischen Client und Server zuständig.
* **Rufen Sie `BindService` die Methode** &ndash; auf, die aufruft `BindService` , die beabsichtigte und die in den vorherigen Schritten erstellte Dienst Verbindung an Android weiterzuleiten, um den Dienst zu starten und die Kommunikation zwischen Client und Dienst.

Die Notwendigkeit, prozessübergreifende Grenzen zu überschreiten, führt zu einer zusätzlichen Komplexität: die Kommunikation erfolgt unidirektional (Client zu Server), und der Client kann Methoden für die Dienstklasse nicht direkt aufrufen. Beachten Sie, dass Android ein `IBinder` Objekt bereitstellt, das möglicherweise eine bidirektionale Kommunikation zulässt, wenn ein Dienst denselben Prozess wie der Client ausgeführt hat. Dies ist nicht der Fall, wenn der Dienst in einem eigenen Prozess ausgeführt wird. Ein Client kommuniziert mit einem Remote Dienst mithilfe `Android.OS.Messenger` der-Klasse.

Wenn ein Client eine Bindung an den Remote Dienst anfordert, ruft Android die `Service.OnBind` Lifecycle-Methode auf, die das interne `IBinder` Objekt zurückgibt, `Messenger`das von gekapselt wird. Der `Messenger` ist ein schlanker Wrapper für eine spezielle `IBinder` Implementierung, die vom Android SDK bereitgestellt wird. Der `Messenger` kümmert sich um die Kommunikation zwischen den beiden unterschiedlichen Prozessen. Der Entwickler hat keine Sorgen um die Details der Serialisierung einer Nachricht, das Marshalling der Nachricht über die Prozess Grenze und die anschließende Deserialisierung auf dem Client. Diese Arbeit wird vom `Messenger` -Objekt behandelt. Dieses Diagramm zeigt die Client seitigen Android-Komponenten, die beteiligt sind, wenn ein Client eine Bindung an einen Out-of-Process-Dienst initiiert:

![Diagramm, das die Schritte und Komponenten für eine Client Bindung an einen Dienst anzeigt](out-of-process-services-images/ipc-01.png "Diagramm, das die Schritte und Komponenten für eine Client Bindung an einen Dienst anzeigt.")

Die `Service` -Klasse im Remote Prozess durchläuft die gleichen Lebenszyklus Rückrufe, die von einem gebundenen Dienst in einem lokalen Prozess durchlaufen werden, und viele der beteiligten APIs sind identisch. `Service.OnCreate`wird verwendet, um eine `Handler` zu initialisieren und in `Messenger` das-Objekt einzufügen. Entsprechend wird außer Kraft gesetzt, aber statt ein `IBinder` Objekt zurückzugeben, gibt der Dienst den `Messenger`zurück. `OnBind`  Dieses Diagramm veranschaulicht, was im Dienst passiert, wenn ein Client an ihn gebunden wird:

![Diagramm, das die Schritte und Komponenten anzeigt, die ein Dienst durchläuft, wenn er von einem Remote Client an gebunden wird](out-of-process-services-images/ipc-02.png "Diagramm, das die Schritte und Komponenten anzeigt, die ein Dienst durchläuft, wenn er von einem Remote Client an gebunden wird.")

Wenn eine `Message` von einem Dienst empfangen wird, wird Sie von in einer Instanz von `Android.OS.Handler`verarbeitet. Der Dienst implementiert eine eigene `Handler` Unterklasse, die die `HandleMessage` -Methode überschreiben muss. Diese Methode wird von `Messenger` aufgerufen und `Message` empfängt als Parameter. Der `Handler` überprüft die `Message` Metadaten und verwendet diese Informationen, um Methoden für den Dienst aufzurufen.

Eine unidirektionale Kommunikation erfolgt, wenn ein `Message` Client ein-Objekt erstellt und mithilfe der `Messenger.Send` -Methode an den Dienst sendet. `Messenger.Send`serialisiert `Message` und übergibt die serialisierten Daten an Android, wodurch die Nachricht über die Prozess Grenze und an den Dienst weitergeleitet wird.  Der `Messenger` , der vom-Dienst gehostet wird, erstellt `Message` ein-Objekt aus den eingehenden Daten. Diese `Message` wird in eine Warteschlange eingereiht, in der Nachrichten einzeln an den `Handler`gesendet werden. Der `Handler` überprüft die in der `Message` enthaltenen Metadaten und ruft `Service`die entsprechenden Methoden für auf. Das folgende Diagramm veranschaulicht die verschiedenen Konzepte in Aktion:

Diagramm zum übertragen ![von Nachrichten zwischen Prozessen](out-of-process-services-images/ipc-03.png "Diagramm, das zeigt, wie Nachrichten zwischen Prozessen übermittelt werden.")

In diesem Leitfaden werden die Details der Implementierung eines Out-of-Process-Dienstanbieter erörtert. Es wird erläutert, wie ein Dienst implementiert wird, der in einem eigenen Prozess ausgeführt werden soll, und wie ein Client mithilfe des `Messenger` Frameworks mit diesem Dienst kommunizieren kann. Außerdem wird die bidirektionale Kommunikation kurz erörtert: der Client sendet eine Nachricht an einen Dienst, und der Dienst sendet eine Nachricht zurück an den Client. Da Dienste von verschiedenen Anwendungen gemeinsam genutzt werden können, wird in diesem Handbuch auch eine Methode zum Einschränken des Client Zugriffs auf den Dienst mithilfe von Android-Berechtigungen erörtert.

> [!IMPORTANT]
> [Bugzilla 51940/GitHub 1950: Dienste mit isolierten Prozessen und benutzerdefinierten Anwendungs Klassen können über Ladungen nicht ordnungsgemäß beheben](https://github.com/xamarin/xamarin-android/issues/1950) , dass ein xamarin. Android-Dienst nicht ordnungsgemäß gestartet `IsolatedProcess` wird, wenn `true`auf festgelegt ist. Dieses Handbuch wird für einen-Verweis bereitgestellt. Eine xamarin. Android-Anwendung sollte weiterhin in der Lage sein, mit einem Out-of-Process-Dienst zu kommunizieren, der in Java geschrieben ist.

## <a name="requirements"></a>Anforderungen

In dieser Anleitung wird das Erstellen von-Diensten vorausgesetzt.

Obwohl es möglich ist, implizite Intents mit apps zu verwenden, die auf ältere Android-APIs abzielen, konzentriert sich dieses Handbuch ausschließlich auf die Verwendung von expliziten Intents. Apps, die auf Android 5,0 (API-Ebene 21) oder höher abzielen, müssen eine explizite Absicht zum Binden an einen Dienst verwenden. Dies ist das Verfahren, das in diesem Handbuch veranschaulicht wird.

## <a name="create-a-service-that-runs-in-a-separate-process"></a>Erstellen eines Dienstanbieter, der in einem separaten Prozess ausgeführt wird

Wie oben beschrieben, bedeutet die Tatsache, dass ein Dienst in einem eigenen Prozess ausgeführt wird, dass einige verschiedene APIs beteiligt sind. Im folgenden finden Sie eine kurze Übersicht über die Schritte zum Binden und Verwenden eines Remote Dienstanbieter:  

* **Erstellen Sie die Unterklasse des Typs, und implementieren Sie die Lebenszyklus Methoden für einen gebundenen Dienst. `Service`**  &ndash; `Service` Es ist auch erforderlich, Metadaten festzulegen, die Android darüber informieren, dass der Dienst in einem eigenen Prozess ausgeführt werden soll.
* **Implementieren Sie `Handler` einen** &ndash; , der`Handler` für die Analyse der Client Anforderungen, für das Extrahieren von Parametern, die vom Client weitergegeben wurden, und für den Aufruf der entsprechenden Methoden für den Dienst zuständig ist.
* **`Messenger` Instanziieren** Sie wie oben `Service` beschrieben eine &ndash; Instanz der `Messenger` -Klasse, die Client Anforderungen an die `Handler` weiterleitet, die im vorherigen Schritt erstellt wurde.

Ein Dienst, der in einem eigenen Prozess ausgeführt werden soll, ist im Grunde noch ein gebundener Dienst. Die Dienstklasse erweitert die Basis `Service` Klasse und wird `ServiceAttribute` mit dem versehen, das die Metadaten enthält, die Android im Android-Manifest bündeln muss. Zunächst sind die folgenden Eigenschaften von `ServiceAttribute` wichtig für einen Out-of-Process-Dienst:

1. `Exported`Diese Eigenschaft muss auf `true` festgelegt werden, damit andere Anwendungen mit dem Dienst interagieren können. &ndash; Der Standardwert dieser Eigenschaft ist `false`.
2. `Process`&ndash; Diese Eigenschaft muss festgelegt werden. Es wird verwendet, um den Namen des Prozesses anzugeben, in dem der Dienst ausgeführt wird.
3. `IsolatedProcess`&ndash; Diese Eigenschaft ermöglicht zusätzliche Sicherheit und weist Android an, den Dienst in einem isolierten Sandkasten mit minimaler Berechtigung zum interagieren mit dem Rest des Systems auszuführen. Weitere Informationen finden [Sie unter Bugzilla 51940-Dienste mit isolierten Prozessen und benutzerdefinierte Anwendungsklasse Fehler beim ordnungsgemäßen Auflösen von über Ladungen](https://bugzilla.xamarin.com/show_bug.cgi?id=51940).
4. `Permission`&ndash; Es ist möglich, den Client Zugriff auf den Dienst zu steuern, indem Sie eine Berechtigung angeben, die Clients anfordern (und erteilt) werden müssen.

Um einen Dienst als eigenen Prozess auszuführen, `Process` `ServiceAttribute` muss die-Eigenschaft für den auf den Namen des Dienstanbieter festgelegt werden. Um mit externen Anwendungen zu interagieren, `Exported` sollte die-Eigenschaft auf `true`festgelegt werden. Wenn `Exported`den Wert hat,könnennurClientsimgleichenAPK(d.h.diegleicheAnwendung),dieindemselbenProzessausgeführtwerden,mitdemDienstinteragieren.`false`

Die Art des Prozesses, in dem der Dienst ausgeführt wird, hängt vom Wert `Process` der-Eigenschaft ab. Android identifiziert drei verschiedene Arten von Prozessen:

-   **Privater Prozess** &ndash; Ein privater Prozess ist ein Dienst, der nur für die Anwendung verfügbar ist, von der er gestartet wurde. Um einen Prozess als privat zu identifizieren, muss der Name **mit einem** Semikolon (Semikolon) beginnen. Der Dienst, der im vorherigen Code Ausschnitt und Screenshot dargestellt wurde, ist ein privater Prozess. Der folgende Code Ausschnitt ist ein Beispiel für `ServiceAttribute`:

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process=":timestampservice_process",
             Exported=true)]
    ```

-   **Globaler Prozess** &ndash; Ein Dienst, der in einem globalen Prozess ausgeführt wird, ist für alle Anwendungen verfügbar, die auf dem Gerät ausgeführt werden. Ein globaler Prozess muss ein voll qualifizierter Klassenname sein, der mit einem Kleinbuchstaben beginnt.
    (Es sei denn, es werden Schritte zum Sichern des diendienstanbieter durchgeführt. andere Anwendungen können diese binden und damit interagieren. Die Sicherung des Dienstanbieter vor nicht autorisierter Verwendung wird weiter unten in diesem Handbuch erläutert.)

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process="com.xamarin.xample.messengerservice.timestampservice_process",
             Exported=true)]
    ```

-   **Isolierter Prozess** &ndash; Ein isolierter Prozess ist ein Prozess, der in einem eigenen Sandkasten ausgeführt wird, isoliert vom Rest des Systems und ohne eigene Berechtigungen. Um einen Dienst in einem isolierten Prozess auszuführen, `IsolatedProcess` `ServiceAttribute` wird die-Eigenschaft von auf `true` festgelegt, wie im folgenden Code Ausschnitt gezeigt:
    
    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             IsolatedProcess= true,
             Process="com.xamarin.xample.messengerservice.timestampservice_process",
             Exported=true)]
    ```

> [!IMPORTANT]
> Weitere Informationen finden [Sie unter Bugzilla 51940-Dienste mit isolierten Prozessen und benutzerdefinierte Anwendungsklasse Fehler beim ordnungsgemäßen Auflösen von über Ladungen](https://bugzilla.xamarin.com/show_bug.cgi?id=51940) .

Ein isolierter Dienst ist eine einfache Möglichkeit, eine Anwendung und das Gerät vor nicht vertrauenswürdigem Code zu schützen. Beispielsweise kann eine APP ein Skript von einer Website herunterladen und ausführen. In diesem Fall stellt die Durchführung dieses Vorgangs in einem isolierten Prozess eine zusätzliche Sicherheitsebene vor nicht vertrauenswürdigem Code dar, der das Android-Gerät kompromittiert.

> [!IMPORTANT]
> Nachdem ein Dienst exportiert wurde, sollte der Name des Dienstanbieter nicht geändert werden. Wenn Sie den Dienstnamen ändern, können auch andere Anwendungen, die den Dienst verwenden, nicht mehr verwendet werden.

Der folgende Screenshot zeigt einen Dienst `Process` , der in einem eigenen privaten Prozess ausgeführt wird, um die Auswirkung der-Eigenschaft anzuzeigen:

![Screenshot, der einen Dienst anzeigt, der in einem privaten Prozess ausgeführt] wird (out-of-process-services-images/ipc-04.png "Screenshot, der einen Dienst anzeigt, der in einem privaten Prozess ausgeführt wird.")

Der nächste Screenshot zeigt `Process="com.xamarin.xample.messengerservice.timestampservice_process"` , und der Dienst wird in einem globalen Prozess ausgeführt:

![Screenshot eines Dienstanbieter, der in einem globalen Prozess ausgeführt] wird (out-of-process-services-images/ipc-05.png "Screenshot eines Dienstanbieter, der in einem globalen Prozess ausgeführt wird.")

Nachdem der `ServiceAttribute` festgelegt wurde, muss der-Dienst einen `Handler`implementieren.

### <a name="implementing-a-handler"></a>Implementieren eines Handlers

Um Client Anforderungen zu verarbeiten, muss der Dienst eine `Handler` implementieren und die `HandleMessage` methodthis-Methode außer Kraft setzen `Message` , wenn die Methode eine-Instanz annimmt, die den Methodenaufruf vom Client kapselt und diesen Aufruf in eine Aktion oder Aufgabe übersetzt. Dies wird vom Dienst durchgeführt. Das `Message` -Objekt macht eine Eigenschaft `What` mit dem Namen verfügbar, bei der es sich um einen ganzzahligen Wert handelt, dessen Bedeutung von Client und Dienst gemeinsam genutzt wird und sich auf eine Aufgabe bezieht, die der Dienst für den Client ausführen soll.

Der folgende Code Ausschnitt aus der Beispielanwendung zeigt ein Beispiel für `HandleMessage`. In diesem Beispiel gibt es zwei Aktionen, die von einem Client für den Dienst angefordert werden können:

* Die erste Aktion ist eine " _Hello, World_ "-Meldung, dass der Client eine einfache Nachricht an den Dienst gesendet hat.
* Die zweite Aktion ruft eine Methode für den Dienst auf und ruft eine Zeichenfolge ab. in diesem Fall handelt es sich bei der Zeichenfolge um eine Meldung, die die Uhrzeit zurückgibt, zu der der Dienst gestartet wurde und wie lange er ausgeführt wurde:

```csharp
public class TimestampRequestHandler : Android.OS.Handler
{
    // other code omitted for clarity

    public override void HandleMessage(Message msg)
    {
        int messageType = msg.What;
        Log.Debug(TAG, $"Message type: {messageType}.");

        switch (messageType)
        {
            case Constants.SAY_HELLO_TO_TIMESTAMP_SERVICE:
                // The client as sent a simple Hello, say in the Android Log.
                break;

            case Constants.GET_UTC_TIMESTAMP:
                // Call methods on the service to retrieve a timestamp message.
                break;
            default:
                Log.Warn(TAG, $"Unknown messageType, ignoring the value {messageType}.");
                base.HandleMessage(msg);
                break;
        }
    }
}
```

Es ist auch möglich, Parameter für den Dienst in `Message`zu verpacken. Dies wird später in diesem Handbuch erläutert. Das nächste zu berücksichtigende Thema ist `Messenger` das Erstellen des-Objekts `Message`für die Verarbeitung der eingehenden s.

### <a name="instantiating-the-messenger"></a>Instanziieren des Messenger

Wie bereits erwähnt, `Message` `Handler.HandleMessage` ist das Deserialisieren des Objekts und das Aufrufen der Verantwortung des- Objekts.`Messenger` Die `Messenger` -Klasse stellt auch `IBinder` ein Objekt bereit, das der Client verwendet, um Nachrichten an den Dienst zu senden.  

Wenn der Dienst gestartet wird, wird der `Messenger` instanziiert und `Handler`eingefügt. Ein guter Ausgangspunkt für diese Initialisierung ist die `OnCreate` -Methode des Diensts. Dieser Code Ausschnitt ist ein Beispiel für einen Dienst, der seine eigenen `Handler` und `Messenger`initialisiert:

```csharp
private Messenger messenger; // Instance variable for the Messenger

public override void OnCreate()
{
    base.OnCreate();
    messenger = new Messenger(new TimestampRequestHandler(this));
    Log.Info(TAG, $"TimestampService is running in process id {Android.OS.Process.MyPid()}.");
}
```

An diesem Punkt ist der letzte Schritt das, das `Service` überschrieben `OnBind`werden soll.

### <a name="implementing-serviceonbind"></a>Implementieren von "Service. onbind"

Alle gebundenen Dienste, unabhängig davon, ob Sie in einem eigenen Prozess ausgeführt werden, müssen `OnBind` die-Methode implementieren. Der Rückgabewert dieser Methode ist ein Objekt, das der Client verwenden kann, um mit dem Dienst zu interagieren. Welches Objekt genau ist, hängt davon ab, ob es sich bei dem Dienst um einen lokalen Dienst oder einen Remote Dienst handelt. Während ein lokaler Dienst eine benutzerdefinierte `IBinder` -Implementierung zurückgibt, gibt ein Remote Dienst den-Wert zurück, der `IBinder` gekapselt ist, aber den, der `Messenger` im vorherigen Abschnitt erstellt wurde:

```csharp
public override IBinder OnBind(Intent intent)
{
    Log.Debug(TAG, "OnBind");
    return messenger.Binder;
}
```

Nachdem diese drei Schritte durchgeführt wurden, kann der Remote Dienst als abgeschlossen angesehen werden.

## <a name="consuming-the-service"></a>Nutzen des Dienstanbieter

Alle Clients müssen Code implementieren, damit der Remote Dienst gebunden und genutzt werden kann. Aus Sicht des Clients gibt es nur wenige Unterschiede zwischen der Bindung an einen lokalen Dienst oder einen Remote Dienst. Der Client ruft die `BindService` -Methode auf und übergibt dabei eine explizite Absicht, den Dienst `IServiceConnection` zu identifizieren, und einen, der die Verbindung zwischen dem Client und dem Dienst verwaltet.

Dieser Code Ausschnitt ist ein Beispiel für das Erstellen einer **expliziten Absicht** für die Bindung an einen Remote Dienst. Die Absicht muss das Paket identifizieren, das den Dienst enthält, und den Namen des Dienstanbieter. Eine Möglichkeit, diese Informationen festzulegen, ist die `Android.Content.ComponentName` Verwendung eines-Objekts und die Bereitstellung der Absicht. Dieser Code Ausschnitt ist ein Beispiel:  

```csharp
// This is the package name of the APK, set in the Android manifest
const string REMOTE_SERVICE_COMPONENT_NAME = "com.xamarin.TimestampService";
// This is the name of the service, according the value of ServiceAttribute.Name
const string REMOTE_SERVICE_PACKAGE_NAME   = "com.xamarin.xample.messengerservice";

// Provide the package name and the name of the service with a ComponentName object.
ComponentName cn = new ComponentName(REMOTE_SERVICE_PACKAGE_NAME, REMOTE_SERVICE_COMPONENT_NAME);
Intent serviceToStart = new Intent();
serviceToStart.SetComponent(cn);
```

Wenn der Dienst gebunden ist, wird `IServiceConnection.OnServiceConnected` die-Methode aufgerufen und `IBinder` stellt einen für einen Client bereit. Der Client verwendet `IBinder`jedoch nicht direkt. Stattdessen wird ein `Messenger` -Objekt aus diesem `IBinder`instanziiert. Dies ist der `Messenger` , der vom Client für die Interaktion mit dem Remote Dienst verwendet wird.

Im folgenden finden Sie ein Beispiel für eine Grund `IServiceConnection` Legende Implementierung, die veranschaulicht, wie ein Client das Herstellen einer Verbindung mit einem Dienst und das Trennen der Verbindung mit einem Dienst übernimmt. Beachten Sie, `OnServiceConnected` dass die- `IBinder`Methode und empfängt, und der `Messenger` `IBinder`Client erstellt einen aus dem folgenden:

```csharp
public class TimestampServiceConnection : Java.Lang.Object, IServiceConnection
{
    static readonly string TAG = typeof(TimestampServiceConnection).FullName;

    MainActivity mainActivity;
    Messenger messenger;

    public TimestampServiceConnection(MainActivity activity)
    {
        IsConnected = false;
        mainActivity = activity;
    }

    public bool IsConnected { get; private set; }
    public Messenger Messenger { get; private set; }

    public void OnServiceConnected(ComponentName name, IBinder service)
    {
        Log.Debug(TAG, $"OnServiceConnected {name.ClassName}");

        IsConnected = service != null
        Messenger = new Messenger(service);

        if (IsConnected)
        {
            // things to do when the connection is successful. perhaps notify the client? enable UI features?
        }
        else
        {
            // things to do when the connection isn't successful.
        }
    }

    public void OnServiceDisconnected(ComponentName name)
    {
        Log.Debug(TAG, $"OnServiceDisconnected {name.ClassName}");
        IsConnected = false;
        Messenger = null;

        // Things to do when the service disconnects. perhaps notify the client? disable UI features?

    }
}
```

Nachdem die Dienst Verbindung und die Absicht erstellt wurden, kann der Client den Bindungsprozess aufzurufen `BindService` und initiieren:

```csharp
IServiceConnection serviceConnection = new TimestampServiceConnection(this);
BindActivity(serviceToStart, serviceConnection, Bind.AutoCreate);
```

Nachdem der Client erfolgreich an den-Dienst gebunden wurde und `Messenger` verfügbar ist, kann der Client an den Dienst senden. `Messages`

## <a name="sending-messages-to-the-service"></a>Senden von Nachrichten an den Dienst

Sobald der Client verbunden ist und über ein `Messenger` -Objekt verfügt, ist es möglich, mit dem Dienst zu kommunizieren `Message` , indem Sie `Messenger`-Objekte über das verteilen. Diese Kommunikation erfolgt unidirektional, der Client sendet die Nachricht, aber es gibt keine Rückgabe Meldung vom Dienst an den Client. In dieser Hinsicht `Message` ist ein Fire-and-Forget-Mechanismus.

Die bevorzugte Methode zum Erstellen eines `Message` -Objekts ist die Verwendung [`Message.Obtain`](xref:Android.OS.Message) der Factorymethode. Mit dieser Methode wird ein `Message` -Objekt aus einem globalen Pool, der von Android verwaltet wird, abgerufen. `Message.Obtain`verfügt auch über überladene Methoden, `Message` die das Initialisieren des Objekts mit den Werten und Parametern ermöglichen, die für den Dienst erforderlich sind.  Nachdem der `Message` instanziiert wurde, wird er durch Aufrufen `Messenger.Send`von an den Dienst gesendet. Dieser Code Ausschnitt ist ein Beispiel für das Erstellen und Verteilen eines `Message` an den Dienst Prozess:

```csharp
Message msg = Message.Obtain(null, Constants.SAY_HELLO_TO_TIMESTAMP_SERVICE);
try
{
    serviceConnection.Messenger.Send(msg);
}
catch (RemoteException ex)
{
    Log.Error(TAG, ex, "There was a error trying to send the message.");
}
```

Es gibt mehrere verschiedene Formen der `Message.Obtain` -Methode. Im vorherigen Beispiel wird verwendet [`Message.Obtain(Handler h, Int32 what)`](xref:Android.OS.Message.Obtain). Da es sich hierbei um eine asynchrone Anforderung an einen Out-of-Process-Dienst handelt, Es gibt keine Antwort vom Dienst, sodass `Handler` auf `null`festgelegt ist. Der zweite Parameter `Int32 what`,, wird in der `.What` -Eigenschaft des `Message` -Objekts gespeichert. Die `.What` -Eigenschaft wird vom Code im Dienst Prozess verwendet, um Methoden für den Dienst aufzurufen.

Die `Message` -Klasse macht auch zwei zusätzliche Eigenschaften verfügbar, die möglicherweise für den Empfänger `Arg1` verwendet `Arg2`werden: und. Diese beiden Eigenschaften sind ganzzahlige Werte, die möglicherweise einige besondere Werte aufweisen, die zwischen dem Client und dem Dienst Bedeutung haben. Beispielsweise `Arg1` kann eine Kunden-ID enthalten und `Arg2` eine Bestellnummer für diesen Kunden enthalten. Kann verwendet werden, um die beiden Eigenschaften festzulegen, `Message` wenn der erstellt wird. [`Method.Obtain(Handler h, Int32 what, Int32 arg1, Int32 arg2)`](xref:Android.OS.Message.Obtain) Eine andere Möglichkeit zum Auffüllen dieser beiden Werte besteht darin, die `.Arg` - `.Arg2` Eigenschaft und die- `Message` Eigenschaft direkt auf das-Objekt festzulegen, nachdem es erstellt wurde.

### <a name="passing-additional-values-to-the-service"></a>Übergeben zusätzlicher Werte an den Dienst

Es ist möglich, mithilfe eines `Bundle`komplexere Daten an den Dienst zu übergeben. In diesem Fall können zusätzliche Werte in einem `Bundle` abgelegt und zusammen mit dem `Message` gesendet werden, indem die [ `.Data` Eigenschaft Eigenschaft](xref:Android.OS.Message.Data) vor dem senden festgelegt wird.

```csharp
Bundle serviceParameters = new Bundle();
serviceParameters.

var msg = Message.Obtain(null, Constants.SERVICE_TASK_TO_PERFORM);
msg.Data = serviceParameters;

messenger.Send(msg);
```


> [!NOTE]
> Im Allgemeinen sollte ein `Message` keine Nutzlast aufweisen, die größer als 1 MB ist. Die Größenbeschränkung kann je nach Version von Android und auf allen proprietären Änderungen, die der Hersteller möglicherweise an der Implementierung des Android Open Source-Projekts (aosp) vorgenommen hat, das mit dem Gerät gebündelt ist, variieren.

## <a name="returning-values-from-the-service"></a>Zurückgeben von Werten aus dem Dienst

Die Messaging Architektur, die zu diesem Zeitpunkt diskutiert wurde, ist unidirektional, der Client sendet eine Nachricht an den Dienst. Wenn es erforderlich ist, dass der Dienst einen Wert an einen Client zurückgibt, wird alles, was zu diesem Zeitpunkt besprochen wurde, umgekehrt. Der Dienst muss eine `Message`erstellen, beliebige Rückgabewerte Verpacken und `Message` über einen `Messenger` an den Client verteilen. Der Dienst erstellt jedoch keinen eigenen `Messenger`, sondern verwendet die Client Instanziierung und das Verpacken von a `Messenger` als Teil der ursprünglichen Anforderung. Der Dienst verwendet `Send` die Meldung, die von diesem Client `Messenger`bereitgestellt wird.  

Die Abfolge der Ereignisse für die bidirektionale Kommunikation lautet wie folgt:

1. Der Client bindet an den Dienst. Wenn der Dienst und der Client eine Verbindung herstellen `IServiceConnection` , hat der, der vom Client verwaltet wird, einen Verweis `Messenger` auf ein-Objekt, das `Message`zum Übertragen von s an den Dienst verwendet wird. Um Verwechslungen zu vermeiden, wird dies als _Dienst Messenger_bezeichnet.
2. Der Client instanziiert `Handler` einen (als _Client Handler_bezeichnet) und verwendet diesen, um seinen eigenen `Messenger` (den _Client Messenger_) zu initialisieren. Beachten Sie, dass es sich bei dem Dienst Messenger und dem Client Messenger um zwei verschiedene Objekte handelt, die Datenverkehr in zwei verschiedenen Richtungen verarbeiten. Der Dienst Messenger verarbeitet Nachrichten vom Client an den Dienst, während der Client Messenger Nachrichten vom Dienst an den Client verarbeitet.
3. Vom Client wird ein `Message` -Objekt erstellt, und `ReplyTo` die-Eigenschaft wird mit dem Client Messenger festgelegt. Die Nachricht wird dann über den Dienst Messenger an den Dienst gesendet.
4. Der Dienst empfängt die Nachricht vom Client und führt die angeforderte Arbeit aus.
5. Wenn der Dienst die Antwort an den Client sendet, wird zum Erstellen eines neuen `Message.Obtain` `Message` -Objekts verwendet.
6. Um diese Nachricht an den Client zu senden, extrahiert der Dienst den Client Messenger aus der `.ReplyTo` -Eigenschaft der Client Nachricht und verwendet diese für `.Send` die `Message` zurück zum Client.
7. Wenn die Antwort vom Client empfangen wird, hat `Handler` Sie einen eigenen, der `Message` verarbeitet, indem Sie die `.What` -Eigenschaft prüft (und ggf. alle Parameter extrahiert, die `Message`in enthalten sind).

In diesem Beispielcode wird veranschaulicht, wie der Client das `Message` und das Paket a `Messenger` instanziiert, das der Dienst für seine Antwort verwenden soll:

```csharp
Handler clientHandler = new ActivityHandler();
Messenger clientMessenger = new Messenger(activityHandler);

Message msg = Message.Obtain(null, Constants.GET_UTC_TIMESTAMP);
msg.ReplyTo = clientMessenger;

try
{
    serviceConnection.Messenger.Send(msg);
}
catch (RemoteException ex)
{
    Log.Error(TAG, ex, "There was a problem sending the message.");
}
```

Der Dienst muss eigene `Handler` Änderungen vornehmen, um den `Messenger` zu extrahieren und diesen zum Senden von Antworten an den Client zu verwenden. Dieser Code Ausschnitt ist ein Beispiel dafür, wie der Dienst `Handler` ein `Message` erstellt und an den Client zurücksendet:  

```csharp
// This is the message that the service will send to the client.
Message responseMessage = Message.Obtain(null, Constants.RESPONSE_TO_SERVICE);
Bundle dataToReturn = new Bundle();
dataToReturn.PutString(Constants.RESPONSE_MESSAGE_KEY, "This is the result from the service.");
responseMessage.Data = dataToReturn;

// The msg object here is the message that was received by the service. The service will not instantiate a client,
// It will use the client that is encapsulated by the message from the client.
Messenger clientMessenger = msg.ReplyTo;
if (clientMessenger!= null)
{
    try
    {
        clientMessenger.Send(responseMessage);
    }
    catch (Exception ex)
    {
        Log.Error(TAG, ex, "There was a problem sending the message.");
    }
}
```

Beachten Sie, dass die `Messenger` vom Client erstellte-Instanz in den obigen Codebeispielen *nicht* das gleiche Objekt ist, das vom Dienst empfangen wird. Dabei handelt es sich `Messenger` um zwei verschiedene Objekte, die in zwei separaten Prozessen ausgeführt werden, die den Kommunikationskanal darstellen.

## <a name="securing-the-service-with-android-permissions"></a>Sichern des Dienstanbieter mit Android-Berechtigungen

Ein Dienst, der in einem globalen Prozess ausgeführt wird, ist für alle Anwendungen verfügbar, die auf diesem Android-Gerät ausgeführt werden. In einigen Situationen ist diese Offenheit und Verfügbarkeit nicht erwünscht, und es ist erforderlich, den Dienst vor Zugriff von nicht autorisierten Clients zu schützen. Eine Möglichkeit, den Zugriff auf den Remote Dienst einzuschränken, ist die Verwendung von Android-Berechtigungen.

Berechtigungen können durch die `Permission` -Eigenschaft `ServiceAttribute` der identifiziert werden, die die `Service` Unterklasse ergänzt. Dadurch wird eine Berechtigung erstellt, die dem Client bei der Bindung an den Dienst erteilt werden muss. Wenn der Client nicht über die entsprechenden Berechtigungen verfügt, löst Android eine aus, `Java.Lang.SecurityException` wenn der Client versucht, eine Bindung an den Dienst herzustellen.

Android bietet vier verschiedene Berechtigungsstufen:

* **Normal** &ndash; Dies ist die Standard Berechtigungsebene. Sie wird verwendet, um Berechtigungen mit geringem Risiko zu identifizieren, die von Android automatisch für Clients erteilt werden können, von denen Sie angefordert werden. Der Benutzer muss diese Berechtigungen nicht explizit erteilen, aber die Berechtigungen können in den App-Einstellungen angezeigt werden.
* **Signatur** &ndash; Dies ist eine spezielle Kategorie der Berechtigung, die von Android automatisch für Anwendungen gewährt wird, die alle mit demselben Zertifikat signiert sind. Diese Berechtigung dient dazu, einem Anwendungsentwickler das Freigeben von Komponenten oder Daten zwischen ihren apps zu erleichtern, ohne den Benutzer für konstante Genehmigungen zu stören.
* **signatureorsystem** Dies ist sehr ähnlich wie die oben beschriebenen Signatur Berechtigungen. &ndash; Zusätzlich zur automatischen Gewährung von apps, die mit demselben Zertifikat signiert sind, wird diese Berechtigung auch apps erteilt, die mit demselben Zertifikat signiert sind, das zum Signieren der mit dem Android-System Abbild installierten Apps verwendet wurde. Diese Berechtigung wird in der Regel nur von Android Rom-Entwicklern verwendet, damit Ihre Anwendungen mit Drittanbieter-apps arbeiten können. Sie wird häufig nicht von apps verwendet, die für die öffentliche Öffentlichkeit allgemein verteilt sind.
* **gefährlich** &ndash; Gefährliche Berechtigungen sind solche, die Probleme für den Benutzer verursachen könnten. Aus diesem Grund müssen die **gefährlichen** Berechtigungen explizit vom Benutzer genehmigt werden.

Da `signature` - `normal` und-Berechtigungen zur installierten Zeit von Android automatisch erteilt werden, ist es wichtig, dass das APK, das den Dienst gehostet, **vor** dem APK installiert wird, das den-Client enthält. Wenn der Client zuerst installiert wird, werden die Berechtigungen von Android nicht erteilt. In diesem Fall ist es erforderlich, das Client-APK zu deinstallieren, das Dienst-APK zu installieren und dann das Client-APK erneut zu installieren.

Es gibt zwei gängige Möglichkeiten, um einen Dienst mit Android-Berechtigungen zu sichern:

1.  **Implementieren der Sicherheit auf Signatur Ebene** &ndash; Sicherheit auf Signatur Ebene bedeutet, dass den Anwendungen, die mit demselben Schlüssel signiert wurden, der zum Signieren des APK mit dem Dienst verwendet wurde, automatisch Berechtigungen erteilt werden. Dies ist eine einfache Möglichkeit für Entwickler, ihren Dienst zu schützen, aber Sie können Sie über ihre eigenen Anwendungen zugänglich halten. Berechtigungen `ServiceAttribute` auf Signatur Ebene werden deklariert, indem `Permission` die-Eigenschaft des `signature`auf festgelegt wird:

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process="com.xamarin.TimestampService.timestampservice_process",
             Permission="signature")]
    public class TimestampService : Service
    {
    }
    ```

2.  **Erstellen einer benutzerdefinierten Berechtigung** &ndash; Der Entwickler des Dienstanbieter kann eine benutzerdefinierte Berechtigung für den Dienst erstellen. Dies eignet sich am besten für den Fall, dass ein Entwickler seinen Dienst für Anwendungen anderer Entwickler freigeben möchte. Eine benutzerdefinierte Berechtigung erfordert etwas mehr Aufwand für die Implementierung von und wird unten behandelt.

Ein vereinfachtes Beispiel für das Erstellen `normal` einer benutzerdefinierten Berechtigung wird im nächsten Abschnitt beschrieben. Weitere Informationen zu Android-Berechtigungen finden Sie in der Google-Dokumentation, um [bewährte Methoden & Sicherheit](https://developer.android.com/training/articles/security-tips.html)zu erhalten. Weitere Informationen zu Android-Berechtigungen finden Sie im [Abschnitt "Berechtigungen](https://developer.android.com/guide/topics/manifest/manifest-intro.html#perms) " in der Android-Dokumentation für das Anwendungs Manifest, um weitere Informationen zu Android-Berechtigungen zu erhalten.

> [!NOTE]
> Im allgemeinen [lehnt Google die Verwendung benutzerdefinierter Berechtigungen](https://developer.android.com/training/articles/security-tips.html#RequestingPermissions) ab, da Sie für Benutzer verwirrend sein könnten.

### <a name="creating-a-custom-permission"></a>Erstellen einer benutzerdefinierten Berechtigung

Um eine benutzerdefinierte Berechtigung zu verwenden, wird Sie vom Dienst deklariert, während der Client diese Berechtigung explizit anfordert.

Zum Erstellen einer Berechtigung im Dienst-APK wird dem `permission` `manifest` -Element in " **androidmanifest. XML**" ein-Element hinzugefügt. Für diese Berechtigung müssen die `name`Attribute `protectionLevel`, und `label` festgelegt sein. Das `name` -Attribut muss auf eine Zeichenfolge festgelegt werden, die die Berechtigung eindeutig identifiziert. Der Name wird in der APP- **Info** -Ansicht der **Android-Einstellungen** angezeigt (wie im nächsten Abschnitt gezeigt).

Das `protectionLevel` -Attribut muss auf einen der vier Zeichen folgen Werte festgelegt werden, die oben beschrieben wurden.  `label` Und`description` müssen auf Zeichen folgen Ressourcen verweisen und zum Bereitstellen eines benutzerfreundlichen namens und einer Beschreibung für den Benutzer verwendet werden.

Dieser Code Ausschnitt ist ein Beispiel für das deklarieren `permission` eines benutzerdefinierten Attributs in " **androidmanifest. XML** " des APK, das den Dienst enthält:

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          android:versionCode="1"
          android:versionName="1.0"
          package="com.xamarin.xample.messengerservice">

    <uses-sdk android:minSdkVersion="21" />

    <permission android:name="com.xamarin.xample.messengerservice.REQUEST_TIMESTAMP"
                android:protectionLevel="signature"
                android:label="@string/permission_label"
                android:description="@string/permission_description"
                />

    <application android:allowBackup="true"
            android:icon="@mipmap/icon"
            android:label="@string/app_name"
            android:theme="@style/AppTheme">

    </application>
</manifest>
```

Anschließend muss die Datei " **androidmanifest. XML** " des Client-APK diese neue Berechtigung explizit anfordern. Dies erfolgt durch Hinzufügen des `users-permission` Attributs zur Datei " **androidmanifest. XML**":

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          android:versionCode="1"
          android:versionName="1.0"
          package="com.xamarin.xample.messengerclient">

    <uses-sdk android:minSdkVersion="21" />

    <uses-permission android:name="com.xamarin.xample.messengerservice.REQUEST_TIMESTAMP" />

    <application
            android:allowBackup="true"
            android:icon="@mipmap/icon"
            android:label="@string/app_name"
            android:theme="@style/AppTheme">
    </application>
    </manifest>
```

### <a name="view-the-permissions-granted-to-an-app"></a>Anzeigen der Berechtigungen, die einer APP erteilt wurden

Öffnen Sie die Android-Einstellungs-APP, und wählen Sie **apps**aus, um die Berechtigungen anzuzeigen, die einer Anwendung erteilt wurden. Suchen Sie die Anwendung, und wählen Sie Sie in der Liste aus. Tippen Sie auf dem Bildschirm " **App-Info** " auf **Berechtigungen** , mit denen eine Ansicht angezeigt wird, die alle der APP gewährten Berechtigungen anzeigt:

[![Screenshots von einem Android-Gerät, das zeigt, wie die einer Anwendung gewährten Berechtigungen gefunden werden](out-of-process-services-images/ipc-06-sml.png)](out-of-process-services-images/ipc-06.png#lightbox)

## <a name="summary"></a>Zusammenfassung

In diesem Leitfaden wurde erläutert, wie Sie einen Android-Dienst in einem Remote Prozess ausführen. Die Unterschiede zwischen einem lokalen und einem Remote Dienst wurden zusammen mit einigen Gründen erläutert, warum ein Remote Dienst für die Stabilität und Leistung einer Android-App hilfreich sein kann. Nachdem erläutert wurde, wie ein Remote Dienst implementiert wird und wie ein Client mit dem Dienst kommunizieren kann, wurde in diesem Handbuch eine Möglichkeit bereitgestellt, den Zugriff auf den Dienst nur von autorisierten Clients aus einzuschränken.


## <a name="related-links"></a>Verwandte Links

- [Lers](xref:Android.OS.Handler)
- [Meldung](xref:Android.OS.Message)
- [Messenger](xref:Android.OS.Messenger)
- [Service Attribute](xref:Android.App.ServiceAttribute)
- [Das exportierte Attribut.](https://developer.android.com/guide/topics/manifest/service-element.html#exported)
- [Dienste mit isolierten Prozessen und benutzerdefinierten Anwendungs Klassen können über Ladungen nicht ordnungsgemäß auflösen.](https://bugzilla.xamarin.com/show_bug.cgi?id=51940)
- [Prozesse und Threads](https://developer.android.com/guide/components/processes-and-threads.html)
- [Android-Manifest: Berechtigungen](https://developer.android.com/guide/topics/manifest/manifest-intro.html#perms)
- [Tipps zur Sicherheit](https://developer.android.com/training/articles/security-tips.html)
- [Messengerservicedemo (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-servicesamples-messengerservicedemo)
