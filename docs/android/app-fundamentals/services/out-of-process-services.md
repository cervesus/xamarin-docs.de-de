---
title: Ausgeführten Android-Dienste in Remote-Prozesse
description: Alle Komponenten in einer Android-Anwendung werden im Allgemeinen in demselben Prozess ausgeführt. Android-Dienste sind eine wichtige Ausnahme, da sie so konfiguriert, dass in ihren eigenen Prozessen ausgeführt werden und für andere Anwendungen, einschließlich der von anderen Android-Entwickler freigegeben werden können. Dieses Handbuch wird beschrieben, wie zum Erstellen und verwenden ein Android-Remotedienst mithilfe von Xamarin.
ms.prod: xamarin
ms.assetid: 27A2E972-A690-480B-B31D-5EF1F74F673C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: db312c4c102feb98791109af19185762bb25856e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61013065"
---
# <a name="running-android-services-in-remote-processes"></a>Ausgeführten Android-Dienste in Remote-Prozesse

_Alle Komponenten in einer Android-Anwendung werden im Allgemeinen in demselben Prozess ausgeführt. Android-Dienste sind eine wichtige Ausnahme, da sie so konfiguriert, dass in ihren eigenen Prozessen ausgeführt werden und für andere Anwendungen, einschließlich der von anderen Android-Entwickler freigegeben werden können. Dieses Handbuch wird beschrieben, wie zum Erstellen und verwenden ein Android-Remotedienst mithilfe von Xamarin._

## <a name="out-of-process-services-overview"></a>Out-of Services Prozessübersicht

Wenn eine Anwendung wird gestartet, erstellt Android einen Prozess, in dem die Anwendung auszuführen. In der Regel wird in alle Komponenten der Anwendung in diese einem Prozess ausgeführt werden. Android-Dienste sind eine wichtige Ausnahme, da sie so konfiguriert, dass in ihren eigenen Prozessen ausgeführt werden und für andere Anwendungen, einschließlich der von anderen Android-Entwickler freigegeben werden können. Diese Arten von Diensten als bezeichnet _Remotedienste_ oder _Out-of-Process-Services_. Der Code für diese Dienste ist das gleiche APK als Hauptdatei der Anwendung enthalten; aber wenn der Dienst gestartet wird Android erstellt einen neuen Prozess für nur diesen Dienst. Im Gegensatz dazu ein Dienst, der im selben Prozess wie der Rest der Anwendung ausgeführt wird manchmal als bezeichnet ein _lokaler Dienst_.

Im Allgemeinen ist es nicht erforderlich, für eine Anwendung auf einen remote-Dienst zu implementieren. Ein lokaler Dienst ist ausreichend (und erwünscht) für eine app Anforderungen in vielen Fällen. Ein Out-of-Process hat seinem eigenen Speicherbereich von Android verwaltet werden muss. Obwohl dies mehr Aufwand für die gesamte Anwendung einleitet, gibt es einige Szenarien, in denen es vorteilhaft sein, ein Dienst in einem eigenen Prozess ausgeführt werden kann:

1. **Freigabe von Funktionen** &ndash; einige Entwickler möglicherweise mehrere apps und Funktionen, die von allen Anwendungen gemeinsam verwendet wird. Packen diese Funktion in einem Android-Dienst mit dem ausgeführt wird, in einem eigenen Prozess Anwendungswartung vereinfachen können. Es ist auch möglich, den Dienst in eine eigene eigenständigen APK-Paket und getrennt vom Rest der Anwendung bereitstellen.
2. **Verbesserung der Benutzerfreundlichkeit** &ndash; es gibt zwei Möglichkeiten, ein Out-of-Process-Dienst die benutzerfreundlichkeit der Anwendung verbessert werden kann. Die erste Möglichkeit befasst sich mit Speicherverwaltung. Wenn eine Garbagecollection (GC) wird Zyklus Android alle Aktivitäten in den Prozess anhalten, bis der Garbage Collector abgeschlossen ist. Der Benutzer kann diese Pause als "Stottern" oder "Jank" wahrnehmen. Wenn ein Dienst ausgeführt wird, im eigenen Prozess, sondern der Dienstprozess, der angehalten wurde, nicht in der der Anwendungsprozess. Diese Pause wird sehr viel weniger für den Benutzer erkennbar, wie der Anwendungsprozess (und daher in der Benutzeroberfläche) nicht angehalten werden.

    Zweitens, falls die arbeitsspeicheranforderungen von einem Prozess ist zu groß, Android, Prozess zum Freigeben von Ressourcen für das Gerät beendet. Wenn ein Dienst verfügt über eine große Menge Arbeitsspeicher belegen, und er in demselben Prozess wie die Benutzeroberfläche ausgeführt wird, wird Klicken Sie dann beim Android erzwingt diese Ressourcen freigegeben, die Benutzeroberfläche, heruntergefahren werden Erzwingen des Benutzers die app zu starten. Wenn ein Dienst, der in einem eigenen Prozess Ausführen von Android heruntergefahren wird, bleibt der UI-Prozess jedoch nicht betroffen. Die Benutzeroberfläche kann binden (den Dienst, für die Benutzer, und das Fortsetzen normal funktionieren transparent und neu starten).

3. **Verbessern der Anwendungsleistung** &ndash; die Benutzeroberfläche des Prozesses beendet oder unabhängig von der Dienstprozess beendet werden kann. Zeitraubendes Aufgaben in einer Out-of-Process-Dienst verschieben, ist es möglich, dass die Startzeit der Benutzeroberfläche möglicherweise verbessert werden, (vorausgesetzt, dass der Dienstprozess zwischen den Zeiten, die UI gestartet wird beibehalten wird).

In vielerlei Hinsicht Bindung an ein Dienst, der in einem anderen Prozess entspricht dem [Binden an einen lokalen Dienst](~/android/app-fundamentals/services/creating-a-service/bound-services.md). Der Client ruft `BindService` binden (und zu starten, falls erforderlich) des Diensts. Ein `Android.OS.IServiceConnection` Objekt erstellt wird, um die Verbindung zwischen dem Client und dem Dienst zu verwalten. Wenn der Client wird erfolgreich an den Dienst bindet, Android, gibt ein Objekt über die `IServiceConnection` , die zum Aufrufen von Methoden für den Dienst verwendet werden kann. Der Client interagiert dann mit dem Dienst mithilfe dieses Objekts. Hier sind die Schritte zum Binden an einen Dienst, um zu überprüfen:

* **Erstellen Sie einen Intent** &ndash; muss ein expliziter Intent-Objekt für die Bindung an den Dienst verwendet werden.
* **Implementieren und Instanziieren einer `IServiceConnection` Objekt** &ndash; der `IServiceConnection` Objekt fungiert als Mittler zwischen dem Client und Dienst.  Er ist verantwortlich für die Verbindung zwischen Client und Server überwachen.
* **Rufen Sie die `BindService` Methode** &ndash; Aufrufen `BindService` verteilt wird, den Zweck und die dienstverbindung erstellt, die in den vorherigen Schritten auf Android, die kümmern wird der Dienst wird gestartet und Herstellen der Kommunikation zwischen Client und Dienst.

Die Notwendigkeit, die Prozessgrenzen überschreiten, führt zusätzlichen Komplexität: die Kommunikation unidirektional (Client zu Server) und der Client nicht direkt aufrufen, Methoden für die Dienstklasse. Denken Sie daran, dass wenn ein Dienst den gleichen Prozess wie der Client ausgeführt wird, Android bietet einer `IBinder` Objekt, das bidirektionale Kommunikation zu ermöglichen kann. Dies ist nicht der Fall in einem eigenen Prozess ausgeführt wird. Ein Client kommuniziert mit einem Remotedienst mithilfe der `Android.OS.Messenger` Klasse.

Android wird aufgerufen, wenn ein Client anfordert, um mit dem Remotedienst zu binden, die `Service.OnBind` lebenszylusmethode, die die interne zurückgibt `IBinder` -Objekt, das von gekapselt wird, die `Messenger`. Die `Messenger` ist ein dünner Wrapper für eine spezielle `IBinder` -Implementierung, die vom Android SDK bereitgestellt wird. Die `Messenger` übernimmt die Kommunikation zwischen den zwei verschiedenen Prozessen. Der Entwickler ist Glücklicherweise mit den Details der Serialisierung einer Nachricht, marshalling der Nachricht über Prozessgrenzen hinweg und anschließend auf dem Client deserialisiert. Diese Arbeit erfolgt durch die `Messenger` Objekt. Dieses Diagramm zeigt die clientseitige-Android-Komponenten, die beteiligt sind, wenn ein Client die Bindung an einen Out-of-Process-Dienst initiiert:

![Diagramm zeigt, die Schritte und Komponenten für einen Client, binden an einen Datendienst](out-of-process-services-images/ipc-01.png "Diagramm zeigt, die Schritte und Komponenten für einen Client, binden an einen Datendienst.")

Die `Service` Klasse in den Remoteprozess geht über den gleichen Lebenszyklus-Rückrufe, die ein gebundener Dienst in einem lokalen Prozess durchlaufen, und viele der beteiligten APIs sind identisch. `Service.OnCreate` Dient zum Initialisieren einer `Handler` , und fügen Sie in `Messenger` Objekt. Ebenso `OnBind` ist außer Kraft gesetzte, aber anstatt einer `IBinder` Objekt auf, die vom Dienst zurückgegeben wird das `Messenger`.  Dieses Diagramm veranschaulicht, was im Dienst geschieht, wenn ein Client an es gebunden wird:

![Diagramm einen Dienst, die Schritte und Komponenten zeigt von einem Remoteclient Bindung an durchläuft](out-of-process-services-images/ipc-02.png "Diagramm einen Dienst, die Schritte und Komponenten zeigt durchläuft, wenn von einem Remoteclient an die gebunden wird.")

Wenn eine `Message` empfangen wird von einem Dienst wird es von verarbeitet, in der Instanz von `Android.OS.Handler`. Der Dienst implementiert einen eigenen `Handler` Unterklasse, die außer Kraft setzen, muss die `HandleMessage` Methode. Diese Methode wird aufgerufen, indem die `Messenger` und empfängt die `Message` als Parameter. Die `Handler` prüft die `Message` Metadaten und mit diesen Informationen zum Aufrufen von Methoden für den Dienst.

Unidirektionale Kommunikation tritt auf, wenn ein Client erstellt eine `Message` Objekt aus, und sendet sie an den Dienst mithilfe der `Messenger.Send` Methode. `Messenger.Send` serialisiert die `Message` und Hand, die Daten aus serialisiert, Android, die die Nachricht über Prozessgrenzen hinweg weiterleitet und an den Dienst.  Die `Messenger` , gehostet wird, indem der Dienst erstellt einen `Message` Objekt aus den eingehenden Daten. Dies `Message` befindet sich in eine Warteschlange, in denen Nachrichten übermittelten einzeln nacheinander, sind die `Handler`. Die `Handler` prüft die Meta-Daten in die `Message` , und rufen Sie die entsprechenden Methoden auf die `Service`. Das folgende Diagramm veranschaulicht diese verschiedenen Konzepte in Aktion:

![Das Diagramm zeigt, wie Nachrichten zwischen Prozessen übergeben werden](out-of-process-services-images/ipc-03.png "das Diagramm zeigt, wie Nachrichten zwischen Prozessen übergeben werden.")

Dieses Handbuch werden die Details der Implementierung einer Out-of-Process-Diensts erläutert. Es wird erläutert, wie Sie einen Dienst implementieren, die in einem eigenen Prozess ausgeführt und wie ein Client mit diesem Dienst kommunizieren kann die `Messenger` Framework. Bidirektionalen Kommunikation wird auch kurz angesprochen: der Client eine Nachricht an einen Dienst und den Dienst Senden einer Nachricht an den Client gesendet. Da Dienste zwischen den verschiedenen Anwendungen genutzt werden können, erläutert dieses Handbuch außerdem ein Verfahren zum Einschränken des Clientzugriffs auf den Dienst mithilfe von Android-Berechtigungen.

> [!IMPORTANT]
> [Bugzilla 51940/GitHub 1950 - Dienste mit isolierten Prozesse und benutzerdefinierte Anwendungsklasse nicht korrekt aufzulösen, Überladungen](https://github.com/xamarin/xamarin-android/issues/1950) Berichte, die ein Xamarin.Android-Dienst nicht ordnungsgemäß gestartet wird bei der `IsolatedProcess` nastaven NA hodnotu `true`. In dieser Anleitung wird eine Referenz bereitgestellt. Eine Xamarin.Android-Anwendung muss immer noch mit einem Out-of-Process-Dienst zu kommunizieren, die in Java geschrieben wird.

## <a name="requirements"></a>Anforderungen

Dieses Handbuch setzt voraus, Kenntnisse im Erstellen von Diensten.

Obwohl es möglich ist, die Verwendung impliziter Intent-Elemente mit apps, die auf älteren Android-APIs, in der vorliegenden ausschließlich für die Verwendung von expliziten Intents konzentriert sich. Apps für Android 5.0 (API Level 21) oder höher muss ein expliziter Intent-Objekt an einen Dienst gebunden werden soll Dies ist das Verfahren, das in dieser Anleitung veranschaulichten sein wird...

## <a name="create-a-service-that-runs-in-a-separate-process"></a>Erstellen Sie einen Dienst, der in einem separaten Prozess ausgeführt wird.

Wie oben beschrieben wird, bedeutet die Tatsache, die ein Dienst in einem eigenen Prozess ausgeführt wird, dass einige andere APIs beteiligt sind. Hier sind die Schritte zum Binden und nutzen einen remote-Dienst, als eine kurze Übersicht zu erhalten:  

* **Erstellen der `Service` Unterklasse** &ndash; Unterklasse der `Service` geben, und implementieren Sie die Lebenszyklusmethoden für einen Dienst gebunden. Es ist auch erforderlich, um Metadaten festzulegen, mit die Android, die den Dienst in einem eigenen Prozess ausgeführt werden mitgeteilt wird soll.
* **Implementieren einer `Handler`**  &ndash; der `Handler` dient zum Analysieren von Clientanforderungen, extrahieren alle Parameter, die vom Client übergeben wurden und die entsprechenden Methoden für den Dienst aufrufen.
* **Instanziieren einer `Messenger`**  &ndash; wie jede oben `Service` muss eine Instanz von "verwalten" die `Messenger` -Klasse, die Clientanforderungen an weitergeleitet werden die `Handler` , die im vorherigen Schritt erstellt wurde.

Ein Dienst, der in einem eigenen Prozess ausgeführt ist, im Grunde immer noch ein gebundener Dienst. Erweitern Sie die Dienstklasse wird die Basis `Service` Klasse und durch die `ServiceAttribute` , die die Metadaten, die Android bündeln im Android-Manifest enthält. Beginnen Sie mit dem Sie die folgenden Eigenschaften der `ServiceAttribute` die für einen Out-of-Process-Dienst wichtig sind:

1. `Exported` &ndash; Diese Eigenschaft muss festgelegt werden, um `true` auf andere Anwendungen mit dem Dienst interagieren können. Der Standardwert dieser Eigenschaft ist `false`.
2. `Process` &ndash; Diese Eigenschaft muss festgelegt werden. Es wird verwendet, den Namen des Prozesses an, die der Dienst ausgeführt wird.
3. `IsolatedProcess` &ndash; Diese Eigenschaft ermöglicht zusätzliche Sicherheit zu gewährleisten, dass Android zum Ausführen des Diensts in einen isolierten Sandkasten mit minimalen Berechtigungen für die Interaktion mit der Rest des Systems. Finden Sie unter [Bugzilla 51940 - Diensten mit isolierten Prozesse und benutzerdefinierte Anwendungsklasse nicht korrekt aufzulösen, Überladungen](https://bugzilla.xamarin.com/show_bug.cgi?id=51940).
4. `Permission` &ndash; Es ist möglich, die Clientzugriff auf den Dienst zu steuern, indem Sie eine Berechtigung, die Clients müssen anfordern (und erteilt werden) angeben.

Ausführung eines Diensts einem eigenen Prozess und die `Process` Eigenschaft für die `ServiceAttribute` muss auf den Namen des Diensts festgelegt werden. Für die Interaktion mit externen Anwendungen, die `Exported` Eigenschaft sollte festgelegt werden, um `true`. Wenn `Exported` ist `false`, und klicken Sie dann nur Clients in der gleichen APK (d. h. die gleiche Anwendung) und die Ausführung in demselben Prozess mit dem Dienst interagieren können.

Der Wert der Art des Prozesses, die der Dienst ausgeführt wird, in hängt die `Process` Eigenschaft. Android geben Aufschluss über drei verschiedene Arten von Prozessen:

-   **Privaten Prozess** &ndash; ein privater Prozess ist ein, die nur für die Anwendung verfügbar ist, die sie gestartet hat. Um einen Prozess als privat zu identifizieren, muss mit der Namen beginnen eine **:** (durch Semikolons). Der Dienst, der im vorherigen Codeausschnitt und Screenshot dargestellt ist, einen privaten Prozess. Der folgende Codeausschnitt ist ein Beispiel für die `ServiceAttribute`:

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process=":timestampservice_process",
             Exported=true)]
    ```

-   **Globale Prozess** &ndash; ein Dienst, der in einem globalen Prozess ausgeführt wird, kann zugegriffen werden, für alle Anwendungen, die auf dem Gerät ausgeführt. Ein globaler Prozess muss es sich um einen vollqualifizierten Klassennamen sein, der mit einem Kleinbuchstaben beginnt.
    (Es sei denn, Sie Maßnahmen ergriffen werden, um den Dienst zu sichern, andere Anwendungen möglicherweise binden und mit ihm interagieren. Den Dienst vor nicht autorisierter Verwendung schützen wird weiter unten in diesem Handbuch erläutert.)

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process="com.xamarin.xample.messengerservice.timestampservice_process",
             Exported=true)]
    ```

-   **Prozess isolierte** &ndash; ein isolierter Prozess ist ein Prozess, der in eine eigene Sandbox isoliert vom Rest des Systems und mit eigenen keine besonderen Berechtigungen ausgeführt wird. Ausführung eines Diensts in einem isolierten Prozess, der `IsolatedProcess` Eigenschaft der `ServiceAttribute` nastaven NA hodnotu `true` wie im folgenden Codeausschnitt gezeigt:
    
    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             IsolatedProcess= true,
             Process="com.xamarin.xample.messengerservice.timestampservice_process",
             Exported=true)]
    ```

> [!IMPORTANT]
> Finden Sie unter [Bugzilla 51940 - Diensten mit isolierten Prozesse und benutzerdefinierte Anwendungsklasse nicht korrekt aufzulösen, Überladungen](https://bugzilla.xamarin.com/show_bug.cgi?id=51940)

Ein isolierter Dienst ist eine einfache Möglichkeit, eine Anwendung und des Geräts vor nicht vertrauenswürdigem Code zu schützen. Beispielsweise kann eine app herunterladen und Ausführen eines Skripts auf einer Website. In diesem Fall das Ausführen dieses Vorgangs in einem isolierten Prozess, wird eine zusätzliche Sicherheitsebene für nicht vertrauenswürdigen Code beeinträchtigen die Android-Gerät enthält.

> [!IMPORTANT]
> Nachdem ein Dienst exportiert wurde, sollten der Namen des Diensts nicht ändern. Ändern des Namens des Diensts kann andere Anwendungen beeinträchtigt werden, die den Dienst verwenden.

Die Wirkung sehen, die die `Process` Eigenschaft verfügt, der folgende Screenshot zeigt einen Dienst, der in einem eigenen privaten Prozess:

![Screenshot mit einem Dienst in einem privaten Prozess](out-of-process-services-images/ipc-04.png "Screenshot mit einem Dienst in einem privaten Prozess ausgeführt.")

Dieser nächste Screenshot zeigt `Process="com.xamarin.xample.messengerservice.timestampservice_process"` und den Dienst in einem globalen Prozess ausgeführt wird:

![Screenshot der ein Dienst, der in einem globalen Prozess](out-of-process-services-images/ipc-05.png "Screenshot eines Diensts in einem globalen Prozess ausgeführt.")

Sobald die `ServiceAttribute` festgelegt wurde, muss der Dienst implementiert einen `Handler`.

### <a name="implementing-a-handler"></a>Implementieren eines Handlers

Um Clientanforderungen zu verarbeiten, muss der Dienst implementieren eine `Handler` und überschreiben die `HandleMessage` MethodThis ist die Methode akzeptiert eine `Message` -Instanz, die den Methodenaufruf vom Client kapselt und übersetzt diesen Aufruf in eine Aktion oder Aufgabe der Dienst ausgeführt. Die `Message` Objekt macht eine Eigenschaft namens `What` Dies ist ein ganzzahliger Wert, deren Bedeutung wird zwischen dem Client und der Dienst gemeinsam genutzt und bezieht sich auf eine Aufgabe, der Dienst für den Client ausführen.

Der folgende Codeausschnitt aus der beispielanwendung zeigt ein Beispiel `HandleMessage`. In diesem Beispiel sind zwei Aktionen, die ein Client anfordern kann, von dem Dienst:

* Die erste Aktion ist ein _Hello, World_ Nachricht, der Client verfügt über eine einfache Nachricht an den Dienst gesendet.
* Die zweite Aktion wird eine Methode für den Dienst aufrufen und eine Zeichenfolge, die Zeichenfolge ist in diesem Fall eine Nachricht, die wann den Dienst gestartet wurde und wie lange zurückgibt, an denen, den er ausgeführt wurde:

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

Es ist auch möglich, Paketparameter für den Dienst in der `Message`. Dies wird weiter unten in diesem Handbuch erläutert werden. Im nächste Thema zu berücksichtigen ist das Erstellen der `Messenger` Objekt zum Verarbeiten des eingehenden `Message`s.

### <a name="instantiating-the-messenger"></a>Instanziieren den Messenger

Wie bereits erläutert wurde, deserialisiert der `Message` Objekt und das Aufrufen von `Handler.HandleMessage` ist dafür verantwortlich, die `Messenger` Objekt. Die `Messenger` Klasse bietet auch eine `IBinder` Objekt, das vom Client zum Senden von Nachrichten an den Dienst verwendet wird.  

Wenn der Dienst gestartet wird, instanziiert er die `Messenger` , und fügen Sie der `Handler`. Ein guter Ausgangspunkt, diese Initialisierung auszuführen ist, auf die `OnCreate` -Methode des Diensts. Dieser Codeausschnitt ist ein Beispiel für einen Dienst, der eine eigene initialisiert `Handler` und `Messenger`:

```csharp
private Messenger messenger; // Instance variable for the Messenger

public override void OnCreate()
{
    base.OnCreate();
    messenger = new Messenger(new TimestampRequestHandler(this));
    Log.Info(TAG, $"TimestampService is running in process id {Android.OS.Process.MyPid()}.");
}
```

An diesem Punkt der letzte Schritt ist für die `Service` überschreiben `OnBind`.

### <a name="implementing-serviceonbind"></a>Implementieren von Service.OnBind

An, ob sie in einem eigenen Prozess oder nicht ausgeführt, müssen alle gebundenen Dienste implementieren den `OnBind` Methode. Der Rückgabewert dieser Methode ist ein Objekt, mit denen der Client mit dem Dienst interagieren. Was dieses Objekt wird hängt ab, ob der Dienst einen lokalen Dienst oder einen Remotedienst ist. Zwar ein lokaler Dienst eine benutzerdefinierte zurückgegeben wird `IBinder` -Implementierung ein Remotediensts gibt die `IBinder` gekapselt, aber die `Messenger` , die im vorherigen Abschnitt erstellt wurde:

```csharp
public override IBinder OnBind(Intent intent)
{
    Log.Debug(TAG, "OnBind");
    return messenger.Binder;
}
```

Sobald diese drei Schritte ausgeführt werden, kann der Remotedienst abgeschlossen betrachtet werden.

## <a name="consuming-the-service"></a>Nutzung des Diensts

Alle Clients müssen Code, um in der Lage zu binden, und nutzen den Remotedienst implementieren. Im Prinzip aus Sicht des Clients gibt es sehr wenige Unterschiede zwischen der Bindung an den lokalen Dienst oder ein Remotedienst. Der Client Ruft die `BindService` -Methode und übergeben einen expliziten Intent zum Identifizieren des Diensts und eine `IServiceConnection` , können die Verbindung zwischen dem Client und dem Dienst zu verwalten.

Dieser Codeausschnitt ist ein Beispiel zum Erstellen einer **expliziter Intent** für die Bindung an einen Remotedienst. Das Ziel muss es sich um das Paket identifizieren, das den Dienst und den Namen des Diensts enthält. Eine Möglichkeit, diese Informationen festzulegen ist die Verwendung einer `Android.Content.ComponentName` Objekts und, die zum Intent bereitzustellen. Dieser Codeausschnitt ist ein Beispiel:  

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

Wenn der Dienst gebunden ist, die `IServiceConnection.OnServiceConnected` Methode wird aufgerufen, und bietet eine `IBinder` an einen Client. Der Client wird verwendet jedoch nicht direkt die `IBinder`. Stattdessen wird es instanziiert einen `Messenger` Objekt aus, die `IBinder`. Dies ist die `Messenger` mit der Client wird für die Interaktion mit dem Remotedienst.

Folgendes ist ein Beispiel für eine sehr einfache `IServiceConnection` -Implementierung, die veranschaulicht, wie ein Client behandeln würde, herstellen und Trennen von einem Dienst. Beachten Sie, dass die `OnServiceConnected` -Methode empfängt und `IBinder`, und der Client erstellt eine `Messenger` , `IBinder`:

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

Nachdem der Dienst-Verbindung und die Absicht erstellt wurden, ist es möglich, dass der Client den Aufruf `BindService` und Initiieren der Bindung:

```csharp
IServiceConnection serviceConnection = new TimestampServiceConnection(this);
BindActivity(serviceToStart, serviceConnection, Bind.AutoCreate);
```

Nachdem der Client erfolgreich an den Dienst gebunden ist und die `Messenger` ist verfügbar, es ist möglich, dass der Client sendet `Messages` an den Dienst.

## <a name="sending-messages-to-the-service"></a>Senden von Nachrichten an den Dienst

Nachdem der Client verbunden ist, und verfügt über eine `Messenger` Objekt, es ist möglich, die Kommunikation mit dem Dienst ausgeführt werden, indem `Message` Objekte über die `Messenger`. Diese Kommunikation unidirektional ist, sendet der Client die Nachricht, aber es gibt keine Antwortnachricht vom Dienst an den Client. In dieser Hinsicht die `Message` ist ein Fire-and-forget-Mechanismus.

Die bevorzugte Methode zum Erstellen einer `Message` Objekt ist die Verwendung der [ `Message.Obtain` ](https://developer.xamarin.com/api/type/Android.OS.Message/#Public%20Methods) Factorymethode. Diese Methode ruft eine `Message` Objekt aus einem globalen Pool, die von Android verwaltet wird. `Message.Obtain` Außerdem verfügt über einige überladene Methoden, mit denen die `Message` Objekt, das mit den Werten und den Dienst erforderlichen Parameter initialisiert werden.  Sobald die `Message` wird instanziiert, Weiterleitung an den Dienst durch den Aufruf `Messenger.Send`. Dieser Codeausschnitt ist ein Beispiel zum Erstellen und Verteilen einer `Message` an den Dienstprozess:

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

Es gibt mehrere verschiedene Formen von der `Message.Obtain` Methode. Im vorherigen Beispiel verwendet die [ `Message.Obtain(Handler h, Int32 what)` ](https://developer.xamarin.com/api/member/Android.OS.Message.Obtain/p/Android.OS.Handler/System.Int32/). Da dies eine asynchrone Anforderung an einen Out-of-Process-Dienst ist. Es werden keine Antwort vom Dienst, also die `Handler` nastaven NA hodnotu `null`. Der zweite Parameter, `Int32 what`, gespeichert werden soll die `.What` Eigenschaft der `Message` Objekt. Die `.What` Eigenschaft wird vom Code in der Dienstprozess zum Aufrufen von Methoden für den Dienst verwendet.

Die `Message` Klasse stellt außerdem zwei zusätzliche Eigenschaften, die an den Empfänger sein können: `Arg1` und `Arg2`. Diese beiden Eigenschaften sind ganzzahlige Werte, die einige spezielle möglicherweise vereinbarten Werte, die Bedeutung zwischen dem Client und dem Dienst haben. Z. B. `Arg1` kann eine Kunden-ID enthalten und `Arg2` möglicherweise eine Bestellnummer für diesen Kunden enthalten. Die [ `Method.Obtain(Handler h, Int32 what, Int32 arg1, Int32 arg2)` ](https://developer.xamarin.com/api/member/Android.OS.Message.Obtain/p/Android.OS.Handler/System.Int32/System.Int32/System.Int32/) können verwendet werden, um die beiden Eigenschaften festgelegt bei der `Message` erstellt wird. Eine weitere Möglichkeit zum Auffüllen dieser beiden Werte festgelegt ist die `.Arg` und `.Arg2` Eigenschaften, die direkt auf die `Message` Objekt, nachdem es erstellt wurde.

### <a name="passing-additional-values-to-the-service"></a>Zusätzliche Werte übergeben, mit dem Dienst

Es ist möglich, komplexere Daten an den Dienst mithilfe von übergibt eine `Bundle`. In diesem Fall können zusätzliche Werte platziert werden, eine `Bundle` und gesendete zusammen mit den `Message` durch Festlegen der [ `.Data` Eigenschaft](https://developer.xamarin.com/api/property/Android.OS.Message.Data/) Eigenschaft vor dem senden.

```csharp
Bundle serviceParameters = new Bundle();
serviceParameters.

var msg = Message.Obtain(null, Constants.SERVICE_TASK_TO_PERFORM);
msg.Data = serviceParameters;

messenger.Send(msg);
```


> [!NOTE]
> Im Allgemeinen eine `Message` dürfen keine Nutzlast, die größer als 1 MB. Die maximale Größe variieren entsprechend der Version von Android und für proprietären Änderungen der Anbieter auf ihre Implementierung von der Android Open Source Projekt (AOSP), das mit dem Gerät komprimiert ist vorgenommen haben.

## <a name="returning-values-from-the-service"></a>Zurückgeben von Werten aus dem Dienst

Die messaging-Architektur, die bis hierhin besprochenen ist unidirektional, der Client sendet eine Nachricht an den Dienst. Wenn für den Dienst einen Wert für einen Client zurückgegeben werden, wird alle Elemente, die bis hierhin besprochenen umgekehrt. Muss beim Erstellen des Diensts eine `Message`verpackte Rückgabewerte, und Verteilen der `Message` über eine `Messenger` an den Client. Der Dienst erstellt jedoch keine eigenen `Messenger`; stattdessen sie beruht auf dem Client instanziiert und dem Paket einen `Messenger` als Teil der ursprünglichen Anforderung. Der Dienst wird `Send` die Nachricht mit diesem vom Client bereitgestellte `Messenger`.  

Die Abfolge der Ereignisse für die bidirektionale Kommunikation ist dies:

1. Der Client bindet an den Dienst. Wenn der Dienst und der Client eine Verbindung herzustellen, die `IServiceConnection` von verwaltet wird der Client muss einen Verweis auf eine `Messenger` -Objekt, das verwendet wird, zum Übertragen von `Message`s, um den Dienst. Um Verwirrung zu vermeiden, dies wird als bezeichnet werden die _Service Messenger_.
2. Laufzeitphase instanziiert ein `Handler` (genannt die _Client Handler_) verwendet, um eine eigene initialisieren `Messenger` (die _Client Messenger_). Beachten Sie, dass der Messenger-Dienst und der Messenger-Client zwei unterschiedliche Objekte, die in zwei verschiedenen Richtungen-Datenverkehr zu bewältigen. Der Messenger-Dienst verarbeitet die Nachrichten vom Client an den Dienst während der Messenger-Client Nachrichten vom Dienst an den Client behandelt.
3. Der Client erstellt eine `Message` Objekt und legt die `ReplyTo` Eigenschaft mit dem Messenger-Client. Die Nachricht wird dann mit der Messenger-Dienst auf den Dienst gesendet.
4. Der Dienst empfängt die Nachricht vom Client, und führt die erforderlichen Aufgaben.
5. Wenn es Zeit für den Dienst zum Senden der Antwort an den Client ist, wird es verwendet `Message.Obtain` zum Erstellen eines neuen `Message` Objekt.
6. Um diese Nachricht an den Client zu senden, wird der Dienst extrahiert der Messenger-Client aus der `.ReplyTo` Eigenschaft des Clients Nachrichten, und verwenden, um `.Send` der `Message` zurück an den Client.
7. Wenn die Antwort vom Client empfangen wird, verfügt es über eine eigene `Handler` verarbeitet, die die `Message` durch Überprüfen der `.What` Eigenschaft (und bei Bedarf, extrahieren Sie alle Parameter enthalten den `Message`).

Dieser Beispielcode wird veranschaulicht, wie der Client instanziiert die `Message` und Verpacken eine `Messenger` , die der Dienst sollte die Antwort verwenden:

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

Der Dienst muss einige Änderungen vornehmen, eine eigene `Handler` zum Extrahieren der `Messenger` und verwenden, um Antworten an den Client zu senden. Dieser Codeausschnitt ist ein Beispiel des Diensts `Handler` würde eine `Message` und zurück an den Client zu senden:  

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

Beachten Sie, dass in den obigen Codebeispielen die `Messenger` Instanz, die vom Client erstellt wird, ist *nicht* das gleiche Objekt, das vom Dienst empfangen wird. Dies sind zwei verschiedene `Messenger` Objekte, die in zwei separate Vorgänge, die den Kommunikationskanal darstellen.

## <a name="securing-the-service-with-android-permissions"></a>Sichern den Dienst mit der Android-Berechtigungen

Ein Dienst, der in einem globalen Prozess ausgeführt wird, kann zugegriffen werden, von allen Anwendungen, die auf dieses Android-Gerät ausgeführt wird. In einigen Fällen diese Flexibilität und die Verfügbarkeit nicht erwünscht ist, und es ist notwendig, den Dienst für den Zugriff von nicht autorisierten Clients zu sichern. Eine Möglichkeit, den Zugriff auf den Remotedienst beschränken ist die Verwendung von Android-Berechtigungen.

Berechtigungen identifiziert werden können, indem die `Permission` Eigenschaft der `ServiceAttribute` ergänzt, die die `Service` Unterklasse. Dadurch wird eine Berechtigung benennen, die der Client beim Binden an den Dienst erteilt werden muss. Android wird ausgelöst, wenn der Client verfügt nicht über die entsprechenden Berechtigungen ein `Java.Lang.SecurityException` Wenn der Client versucht, auf den Dienst zu binden.

Es gibt vier unterschiedliche Berechtigungsstufen, die Android bietet:

* **normale** &ndash; Dies ist die Standard-Berechtigungsebene. Es wird verwendet, um mit geringem Risiko Berechtigungen identifizieren, die automatisch von Android-Clients erteilt werden können, die diesen anfordern. Der Benutzer verfügt nicht über diese Berechtigungen explizit gewähren, aber die Berechtigungen, die in den app-Einstellungen angezeigt werden können.
* **Signatur** &ndash; Dies ist eine besondere Kategorie der Berechtigung, die automatisch von Android für Anwendungen erteilt werden soll, die alle mit demselben Zertifikat signiert sind. Durch diese Berechtigung ist macht es einfach für ein Anwendungsentwickler, Komponenten oder Daten zwischen ihren apps, ohne dass des Benutzers für Konstanten Genehmigungen freizugeben.
* **SignatureOrSystem** &ndash; Dies ähnelt sehr der **Signatur** Berechtigungen, die oben beschriebenen. Automatisch gewährt, apps, die von demselben Zertifikat signiert sind, sondern wird durch diese Berechtigung ebenfalls erteilt für apps, die signiert sind das gleiche Zertifikat, das verwendet wurde, zum Signieren der apps mit dem Android Systemimage installiert. Durch diese Berechtigung ist in der Regel nur von Android ROM-Entwicklern verwendet, ihre Anwendungen mit Drittanbieter-apps genutzt werden. Es wird nicht häufig von apps verwendet, die allgemeine Verteilung für die Öffentlichkeit vorgesehen sind.
* **gefährliche** &ndash; problematischen Berechtigungen sind diejenigen, die Probleme für den Benutzer verursachen. Aus diesem Grund **gefährliche** Berechtigungen müssen explizit vom Benutzer genehmigt werden.

Da `signature` und `normal` Berechtigungen werden automatisch gewährt installierten zum Zeitpunkt von Android, ist es entscheidend, dass APK auf dem der Dienst installiert werden **vor** das APK mit dem Client. Wenn der Client zuerst installiert ist, wird von Android nicht die Berechtigungen erteilt. In diesem Fall ist es erforderlich sein, deinstallieren Sie die APK-Client, den APK-Dienst installieren und dann den APK-Client neu installieren.

Es gibt zwei allgemeine Verfahren zum Sichern eines Diensts mit Android-Berechtigungen:

1.  **Implementieren Sie die Signatur-Sicherheit auf Zeilenebene** &ndash; Signature-Sicherheit auf Zeilenebene bedeutet, dass Berechtigungen automatisch gewährt, die diese Anwendungen, die mit demselben Schlüssel signiert sind, die zum Signieren des APK mit dem Dienst verwendet wurde. Dies ist eine einfache Möglichkeit für Entwickler, schützen ihren Dienst, aber belassen Sie sie in ihren eigenen Anwendungen zugegriffen werden kann. Signatur-Berechtigungen auf Serverebene werden deklariert, durch Festlegen der `Permission` Eigenschaft der `ServiceAttribute` zu `signature`:

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process="com.xamarin.TimestampService.timestampservice_process",
             Permission="signature")]
    public class TimestampService : Service
    {
    }
    ```

2.  **Erstellen Sie eine benutzerdefinierte Berechtigung** &ndash; es ist möglich, dass der Entwickler des Diensts, um eine benutzerdefinierte Berechtigung für den Dienst zu erstellen. Dies ist für, wenn ein Entwickler möchte seinen Dienst mit Anwendungen von anderen Entwicklern gemeinsam am besten geeignet. Eine benutzerdefinierte Berechtigung erfordert etwas mehr Aufwand zum Implementieren und nachfolgend behandelt wird.

Ein vereinfachtes Beispiel zum Erstellen ein benutzerdefinierten `normal` Berechtigung wird im nächsten Abschnitt beschrieben werden. Weitere Informationen zu Android-Berechtigungen finden Sie in Google Dokumentation für [Best Practices und Sicherheit](https://developer.android.com/training/articles/security-tips.html). Weitere Informationen zu Android-Berechtigungen finden Sie unter den [Abschnitt "Berechtigungen"](https://developer.android.com/guide/topics/manifest/manifest-intro.html#perms) der Android-Dokumentation für das Anwendungsmanifest für Weitere Informationen zu Android-Berechtigungen.

> [!NOTE]
> Im allgemeinen [Google wird eine verhindert die Verwendung von benutzerdefinierten Berechtigungen](https://developer.android.com/training/articles/security-tips.html#RequestingPermissions) , wie sie für Benutzer verwirrend sicherlich.

### <a name="creating-a-custom-permission"></a>Erstellen einer benutzerdefinierten Berechtigung

Um eine benutzerdefinierte Berechtigung verwenden, wird er durch den Dienst deklariert, während der Client explizit die Berechtigung anfordert.

Erstellen Sie eine Berechtigung im Dienst APK aus – und eine `permission` Element wird hinzugefügt, um die `manifest` Element im **"androidmanifest.xml"**. Durch diese Berechtigung benötigen die `name`, `protectionLevel`, und `label` Attribute festgelegt. Die `name` -Attribut muss festgelegt werden, um eine Zeichenfolge, die die Berechtigung eindeutig identifiziert. Der Name erscheint in der **App-Info** -Ansicht der **Android-Einstellungen** (wie im nächsten Abschnitt gezeigt).

Die `protectionLevel` Attribut muss festgelegt werden, auf einen der vier Zeichenfolgenwerte, die oben beschrieben wurden.  Die `label` und `description` muss auf Zeichenfolgenressourcen verweisen und werden verwendet, um einen benutzerfreundlichen Namen und eine Beschreibung für dem Benutzer bereitstellen.

Dieser Codeausschnitt ist ein Beispiel des Deklarierens einer benutzerdefiniertes `permission` -Attribut im **"androidmanifest.xml"** die apk-Datei, die den Dienst enthält:

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

Anschließend wird die **"androidmanifest.xml"** des Clients muss APK diese neue Berechtigung explizit anfordern. Dies erfolgt durch Hinzufügen der `users-permission` -Attribut auf die **"androidmanifest.xml"**:

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

### <a name="view-the-permissions-granted-to-an-app"></a>Zeigen Sie die Berechtigungen für eine App

Um die Berechtigungen anzuzeigen, die eine Anwendung erteilt wurde, öffnen Sie die Einstellungen für Android-app, und wählen **Apps**. Suchen Sie, und wählen Sie die Anwendung in der Liste aus. Von der **Anwendungsinformation** tippen **Berechtigungen** die wird eine Sicht mit allen die Berechtigungen für die app angezeigt:

[![Screenshots von Android-Geräten zeigt, wie die Berechtigungen für eine Anwendung zu finden.](out-of-process-services-images/ipc-06-sml.png)](out-of-process-services-images/ipc-06.png#lightbox)

## <a name="summary"></a>Zusammenfassung

Dieses Handbuch wurde eine erweiterte Erläuterung dazu, wie Sie einen Android-Dienst in einem remote-Prozess ausgeführt. Die Unterschiede zwischen einem lokalen und einen Remotedienst wurde erläutert, sowie einige Gründe, warum ein Remotedienst hilfreich sein, Stabilität und Leistung von einer Android-app kann. Nach einer Erläuterung, wie einen Remotedienst implementiert und wie ein Client mit dem Dienst kommunizieren kann, hat sich das Handbuch auf auf bieten eine Möglichkeit, den Zugriff auf den Dienst nur von autorisierten Clients einschränken.


## <a name="related-links"></a>Verwandte Links

- [Handler](https://developer.xamarin.com/api/type/Android.OS.Handler/)
- [Meldung](https://developer.xamarin.com/api/type/Android.OS.Message/)
- [Messenger](https://developer.xamarin.com/api/type/Android.OS.Messenger/)
- [ServiceAttribute](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute)
- [Das Attribut exportiert](https://developer.android.com/guide/topics/manifest/service-element.html#exported)
- [Dienste mit isolierten Prozesse und benutzerdefinierte Anwendungsklasse nicht korrekt aufzulösen, Überladungen](https://bugzilla.xamarin.com/show_bug.cgi?id=51940)
- [Prozesse und Threads](https://developer.android.com/guide/components/processes-and-threads.html)
- [Android-Manifest - Berechtigungen](https://developer.android.com/guide/topics/manifest/manifest-intro.html#perms)
- [Tipps zur Sicherheit](https://developer.android.com/training/articles/security-tips.html)
- [MessengerServiceDemo (Beispiel)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/MessengerServiceDemo/)
