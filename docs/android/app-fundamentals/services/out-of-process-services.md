---
title: Ausführen der Android-Dienste in Remote-Prozesse
description: Alle Komponenten in einer Android-Anwendung werden im Allgemeinen in demselben Prozess ausgeführt. Android-Dienste sind eine wichtige Ausnahme, da so konfiguriert, dass in eigene Prozesse ausgeführt und von anderen Anwendungen, einschließlich der von anderen Entwicklern Android genutzt werden können. Diese Anleitung wird erläutert, wie zum Erstellen und verwenden eine Android-Remotedienst mithilfe von Xamarin.
ms.prod: xamarin
ms.assetid: 27A2E972-A690-480B-B31D-5EF1F74F673C
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 02/16/2018
ms.openlocfilehash: 57be8187509ad7607c38ea17233e9ab5d25b41f1
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
ms.locfileid: "30773659"
---
# <a name="running-android-services-in-remote-processes"></a>Ausführen der Android-Dienste in Remote-Prozesse

_Alle Komponenten in einer Android-Anwendung werden im Allgemeinen in demselben Prozess ausgeführt. Android-Dienste sind eine wichtige Ausnahme, da so konfiguriert, dass in eigene Prozesse ausgeführt und von anderen Anwendungen, einschließlich der von anderen Entwicklern Android genutzt werden können. Diese Anleitung wird erläutert, wie zum Erstellen und verwenden eine Android-Remotedienst mithilfe von Xamarin._

## <a name="out-of-process-services-overview"></a>Out-of-Process (Übersicht)

Wenn eine Anwendung wird gestartet, erstellt Android einen Prozess, in dem die Anwendung auszuführen. In der Regel wird alle Komponenten der Anwendung in dieser ein Prozess ausgeführt. Android-Dienste sind eine wichtige Ausnahme, da so konfiguriert, dass in eigene Prozesse ausgeführt und von anderen Anwendungen, einschließlich der von anderen Entwicklern Android genutzt werden können. Diese Arten von Diensten werden als bezeichnet _Remotedienste_ oder _Out-of-Process-Services_. Der Code für diese Dienste werden in der gleichen APK wie die Hauptassembly der Anwendung enthalten sein; jedoch, wenn der Dienst gestartet ist Android erstellt einen neuen Prozess für nur diesen Dienst. Im Gegensatz dazu ein Dienst, der im gleichen Prozess wie der Rest der Anwendung ausgeführt wird manchmal als bezeichnet eine _lokaler Dienst_.

Im Allgemeinen ist es nicht erforderlich, für eine Anwendung auf einen Remotedienst zu implementieren. Ein lokaler Dienst ist ausreichend (und wünschenswert) für eine app-Anforderungen in vielen Fällen. Eine Out-of-Process hat seinem eigenen Speicherbereich der Android verwaltet werden muss. Obwohl dies einen größeren Mehraufwand für die gesamte Anwendung entstehen, gibt es einige Szenarios, in denen es vorteilhaft sein, ein Dienst in einem eigenen Prozess ausgeführt werden kann:

1. **Freigeben von Funktionalität** &ndash; einige Entwickler möglicherweise mehrere apps und Funktionen, die von allen Anwendungen gemeinsam verwendet wird. Verpacken diese Funktion in einem Android-Dienst mit dem ausgeführt wird, in einem eigenen Prozess Anwendungswartung vereinfachen können. Es ist auch möglich, den Dienst in einem eigenen eigenständigen APK-Paket und getrennt vom Rest der Anwendung bereitgestellt.
2. **Verbessern der Benutzererfahrung** &ndash; stehen zwei Möglichkeiten, dass ein Out-of-Process-Dienst erforderlichen Erfahrungsgrad des Benutzers der Anwendung verbessert werden kann. Die erste Möglichkeit behandelt die Verwaltung des Arbeitsspeichers. Wenn eine Garbagecollection (GC) tritt Zyklus auf, Android alle Aktivitäten im Prozess anhalten, bis der globale Katalogserver abgeschlossen ist. Der Benutzer kann diese Pause als "Stottern bereitstellt" oder "Jank" wahrnehmen. Wenn ein Dienst ausgeführt wird, es eigenen Prozess ist, wird der Dienstprozess, der angehalten wurde, nicht den Anwendungsprozess aus. Diese Pause wird wesentlich weniger für den Benutzer erkennbar wie der Anwendungsprozess (und daher die Benutzeroberfläche) angehalten werden.

    Zweitens können kann die arbeitsspeicheranforderungen für einen Prozess ist zu groß, Android dieses Prozesses, um Ressourcen für das Gerät frei kill. Wenn ein Dienst verfügt über eine große Menge Arbeitsspeicher belegen, und es im selben Prozess wie die Benutzeroberfläche ausgeführt wird, wird Klicken Sie dann bei Android wird erzwungen. diese Ressourcen freigibt die Benutzeroberfläche heruntergefahren werden Erzwingen des Benutzers für den Anwendungsstart. Wenn ein Dienst, in einem eigenen Prozess von Android heruntergefahren wird, bleibt der UI-Prozess nicht betroffen. Die Benutzeroberfläche kann binden (der Dienst, für den Benutzer und Resume normale Funktionsweise transparent und neu starten).

3. **Verbessern der Anwendungsleistung** &ndash; die Benutzeroberfläche des Prozesses ist möglicherweise beendet oder unabhängig von den Dienstprozess beenden. Umgezogen langwierige Startaufgaben für einen Out-of-Process-Dienst ist es möglich, dass die Startzeit der Benutzeroberfläche möglicherweise verbessert werden, (vorausgesetzt, dass der Dienstprozess zwischen der Häufigkeit, mit der Benutzeroberfläche gestartet wird beibehalten wird).

In vielerlei Hinsicht Bindung mit einem Dienst in einem anderen Prozess ist identisch mit [Binden an einen lokalen Dienst](~/android/app-fundamentals/services/creating-a-service/bound-services.md). Der Client ruft `BindService` binden (und zu starten, falls erforderlich) den Dienst. Ein `Android.OS.IServiceConnection` Objekt wird erstellt, um die Verbindung zwischen dem Client und dem Dienst zu verwalten. Ein Objekt über Android zurück, wenn der Client bindet erfolgreich an den Dienst die `IServiceConnection` , die zum Aufrufen von Methoden für den Dienst verwendet werden kann. Der Client interagiert mit dem Dienst unter Verwendung dieses Objekts. Hier sind die Schritte zum Binden an einen Dienst, um zu überprüfen:

* **Erstellen Sie eine beabsichtigte** &ndash; muss eine explizite Absicht hinzu, um den Dienst verwendet werden.
* **Implementieren und Instanziieren einer `IServiceConnection` Objekt** &ndash; der `IServiceConnection` Objekt dient als Vermittler zwischen dem Client und dem Dienst.  Es ist für die Überwachung der Verbindung zwischen Client und Server verantwortlich.
* **Aufrufen der `BindService` Methode** &ndash; Aufrufen `BindService` verteilt wird, den Zweck und die Dienst-Verbindung erstellt, die in den vorherigen Schritten, Android, die kümmern wird den Dienst zu starten, und beim Herstellen der Kommunikation zwischen Client und Dienst.

Die Notwendigkeit, die Prozessgrenzen überschreiten zusätzlichen Komplexität eingeführt: die Kommunikation erfolgt unidirektional (Client zu Server) und der Client nicht direkt aufrufen, Methoden für die Dienstklasse. Beachten Sie, dass wenn ein Dienst den gleichen Prozess wie der Client ausgeführt wird, Android bietet einer `IBinder` Objekt, das für die bidirektionale Kommunikation zulassen kann. Dies ist nicht der Fall bei in einem eigenen Prozess ausgeführt wird. Ein Client kommuniziert mit einem Remotedienst mit der Unterstützung der `Android.OS.Messenger` Klasse.

Android wird aufgerufen, wenn ein Client fordert an den Remotedienst gebunden werden soll, die `Service.OnBind` Lifecycle-Methode, die internen zurückliefert `IBinder` -Objekt, das von gekapselt wird, die `Messenger`. Die `Messenger` ist ein schlanker Wrapper über eine spezielle `IBinder` Implementierung, die von der Android-SDK bereitgestellt wird. Die `Messenger` übernimmt die Kommunikation zwischen den beiden anderen Prozessen. Der Entwickler ist Glücklicherweise mit den Details der Serialisierung einer Nachricht, die Nachricht über Prozessgrenzen hinweg gemarshallt, und diese anschließend deserialisiert auf dem Client. Dieses Werk erfolgt durch die `Messenger` Objekt. Dieses Diagramm zeigt die clientseitigen Komponenten Android, die beteiligt sind, wenn ein Client die Bindung an einen Out-of-Process-Dienst initiiert:

![Diagramm zeigt die Schritte und die Komponenten für einen Client, binden an einen Datendienst](out-of-process-services-images/ipc-01.png "Diagramm zeigt die Schritte und die Komponenten für einen Client, binden an einen Datendienst.")

Die `Service` Klasse im Remoteprozess geht über den gleichen Lebenszyklus-Rückrufe, die ein gebundener Dienst in einem lokalen Prozess durchlaufen wird, und viele der APIs beteiligten sind identisch. `Service.OnCreate` Dient zum Initialisieren einer `Handler` und einfügen, die in `Messenger` Objekt. Ebenso `OnBind` ist außer Kraft gesetzte, aber anstatt einer `IBinder` -Objekt, die der Dienst zurückgegeben wird die `Messenger`.  Dieses Diagramm veranschaulicht, was im Dienst geschieht, wenn ein Client gebunden werden:

![Diagramm, das die Schritte und die Komponenten ein Diensts zeigt durchläuft, wenn von einem Remoteclient an gebunden](out-of-process-services-images/ipc-02.png "Diagramm, das die Schritte und die Komponenten ein Diensts zeigt durchläuft, wenn von einem Remoteclient an gebunden wird.")

Wenn eine `Message` empfangen wird von einem Dienst von verarbeitet wird in der Instanz von `Android.OS.Handler`. Der Dienst implementiert einen eigenen `Handler` Unterklasse, die außer Kraft setzen, muss die `HandleMessage` Methode. Diese Methode wird aufgerufen, indem die `Messenger` und empfängt die `Message` als Parameter. Die `Handler` prüft die `Message` -Metadaten und diese Informationen zum Aufrufen von Methoden für den Dienst verwenden.

Unidirektionale Kommunikation stattfindet, wenn ein Client erstellt einen `Message` -Objekt und sendet es an den Dienst mithilfe der `Messenger.Send` Methode. `Messenger.Send` serialisiert die `Message` und Hand, die Daten aus serialisiert, Android, die Weiterleitung der Nachricht über Prozessgrenzen hinweg wird und an den Dienst.  Die `Messenger` gehostet, wird der Dienst erstellt einen `Message` Objekt aus den empfangenen Daten. Dies `Message` befindet in eine Warteschlange, in denen Nachrichten gesendeten einzeln nacheinander, sind die `Handler`. Die `Handler` wird die in enthaltenen Metadaten untersuchen der `Message` und rufen Sie die entsprechende Methoden auf die `Service`. Das folgende Diagramm veranschaulicht diese verschiedenen Konzepte in Aktion:

![Diagramm mit, wie Nachrichten zwischen Prozessen übergeben werden](out-of-process-services-images/ipc-03.png "Diagramm wie Nachrichten zwischen Prozessen übergeben werden.")

Dieses Handbuch werden die Details der Implementierung einer Out-of-Process-Diensts erläutert. Es wird erläutert, wie einen Dienst implementiert, die in einem eigenen Prozess ausgeführt werden soll und wie ein Client mit diesem Dienst mit kommunizieren kann die `Messenger` Framework. Es wird auch kurz erläutert die bidirektionalen Kommunikation: der Client eine Nachricht an einen Dienst und den Dienst Senden einer Nachricht an den Client gesendet. Dienste können zwischen verschiedenen Anwendungen freigegeben werden, werden diesem Handbuch auch eine Technik zum Einschränken der Client beim Zugriff auf den Dienst mit Berechtigungen für Android erläutert.

> [!IMPORTANT]
> [Bugzilla 51940 - Diensten mit isolierten Prozesse und benutzerdefinierte Anwendungsklasse nicht Überladungen ordnungsgemäß aufgelöst](https://bugzilla.xamarin.com/show_bug.cgi?id=51940) Berichte, die ein Xamarin.Android-Dienst nicht ordnungsgemäß gestartet wird bei der `IsolatedProcess` festgelegt ist, um `true`. Dieses Handbuch ist eine Referenz bereitgestellt. Eine Anwendung Xamarin.Android sollte immer noch mit einem Out-of-Process-Dienst zu kommunizieren, die in Java geschrieben wird.

## <a name="requirements"></a>Anforderungen

Dieses Handbuch setzt voraus, Kenntnisse über das Erstellen von Diensten.

Obwohl es möglich ist, verwenden Sie implizite Intents mit apps, die auf älteren konzentriert sich Android-APIs, dieses Handbuchs ausschließlich für die Verwendung von expliziten Intents. Apps für Android 5.0.x (API-Ebene 21) oder höher muss eine explizite Absicht an einen Dienst gebunden werden soll Dies ist das Verfahren, das in diesem Handbuch zu nachzuweisen...

## <a name="create-a-service-that-runs-in-a-separate-process"></a>Erstellen Sie einen Dienst, der in einem separaten Prozess ausgeführt wird.

Wie oben beschrieben wird, bedeutet die Tatsache, die ein Dienst in einem eigenen Prozess ausgeführt wird, dass einige andere APIs beteiligt sind. Es folgen die Schritte mit gebunden und nutzen einen Remotedienst vorhanden, als eine kurze Übersicht über:  

* **Erstellen der `Service` Unterklasse** &ndash; Unterklasse der `Service` geben und für einen Dienst gebundene Lebenszyklusmethoden implementieren. Es ist auch erforderlich, um Metadaten festzulegen, die Android informiert wird, der Dienst in einem eigenen Prozess ausgeführt.
* **Implementieren einer `Handler`**  &ndash; der `Handler` ist verantwortlich für das Analysieren von Clientanforderungen, extrahieren alle Parameter, die vom Client übergeben wurden und die entsprechenden Methoden für den Dienst aufrufen.
* **Instanziieren einer `Messenger`**  &ndash; wie oben, jede beschrieben `Service` verwalten muss eine Instanz von der `Messenger` -Klasse, die Clientanforderungen zum Weiterleiten der `Handler` , die im vorherigen Schritt erstellt wurde.

Ein Dienst, der in einem eigenen Prozess ausgeführt ist, grundsätzlich immer noch einen Dienst gebunden. Erweitern Sie die Dienstklasse Basis `Service` Klasse und ergänzt wird, mit der `ServiceAttribute` , enthält die Metadaten, die in der Android-Manifest bündeln Android muss. Beginnen Sie mit den folgenden Eigenschaften von der `ServiceAttribute` , sind wichtig, eine Out-of-Process-Dienst:

1. `Exported` &ndash; Diese Eigenschaft muss festgelegt werden, um `true` damit andere Anwendungen mit dem Dienst interagieren können. Der Standardwert dieser Eigenschaft ist `false`.
2. `Process` &ndash; Diese Eigenschaft muss festgelegt werden. Es wird verwendet, um den Namen des Prozesses anzugeben, die in der Dienst ausgeführt wird.
3. `IsolatedProcess` &ndash; Diese Eigenschaft ermöglicht zusätzliche Sicherheit, mitteilen, Android, die zum Ausführen des Diensts in einer isolierten Sandkasten mit minimalen Berechtigungen für Iteract mit dem Rest des Systems. Finden Sie unter [Bugzilla 51940 - Diensten mit isolierten Prozesse und benutzerdefinierte Anwendungsklasse nicht Überladungen ordnungsgemäß aufgelöst](https://bugzilla.xamarin.com/show_bug.cgi?id=51940).
4. `Permission` &ndash; Es ist möglich, zum Steuern der Client beim Zugriff auf den Dienst durch Angabe einer Berechtigung, die Clients müssen anfordern (und gewährt werden).

Ausführung eines Diensts als eigener Prozess der `Process` Eigenschaft auf die `ServiceAttribute` muss auf den Namen des Diensts festgelegt werden. Für die Interaktion mit externen Anwendungen die `Exported` Eigenschaft sollte festgelegt werden, um `true`. Wenn `Exported` ist `false`, und klicken Sie dann nur Clients in der gleichen APK (d. h. die gleiche Anwendung) und die Ausführung in demselben Prozess mit dem Dienst interagieren können.

Welche Art des Prozesses, die der Dienst wird ausgeführt, in den Wert der hängt die `Process` Eigenschaft. Android gibt drei verschiedene Typen von Prozessen:

-   **Private-Prozess** &ndash; ein privater Prozess ist eine, die nur für die Anwendung verfügbar ist, die er gestartet. Um einen Prozess als privat zu identifizieren, muss mit der Namen beginnen ein **:** (durch Semikolons). Der Dienst, der im vorherigen Codeausschnitt und Screenshot dargestellt ist eine private Prozesse. Der folgende Codeausschnitt ist ein Beispiel für die `ServiceAttribute`:

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process=":timestampservice_process",
             Exported=true)]
    ```

-   **Globale Prozess** &ndash; ein Dienst, der in einem globalen Prozess ausgeführt wird für alle Anwendungen, die auf dem Gerät ausgeführt wird. Ein globale Prozess muss ein voll qualifizierter Klassenname, der mit einem Kleinbuchstaben Zeichen beginnt.
    (Es sei denn, die Maßnahmen ergriffen werden, um den Dienst zu sichern, andere Anwendungen möglicherweise binden und mit ihr zu interagieren. Sichern den Dienst vor nicht autorisierter Verwendung wird weiter unten in diesem Handbuch besprochen.)

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process="com.xamarin.xample.messengerservice.timestampservice_process",
             Exported=true)]
    ```

-   **Prozess isolierte** &ndash; ein isolierter Prozess ist ein Prozess, der in einem eigenen Sandkasten, isoliert vom Rest des Systems und ohne besondere Berechtigungen selbst ausgeführt wird. Ausführung eines Diensts in einem isolierten Prozess der `IsolatedProcess` Eigenschaft von der `ServiceAttribute` festgelegt ist, um `true` wie in diesem Codeausschnitt gezeigt:
    
    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             IsolatedProcess= true,
             Process="com.xamarin.xample.messengerservice.timestampservice_process",
             Exported=true)]
    ```

> [!IMPORTANT]
> Finden Sie unter [Bugzilla 51940 - Diensten mit isolierten Prozesse und benutzerdefinierte Anwendungsklasse nicht Überladungen ordnungsgemäß aufgelöst](https://bugzilla.xamarin.com/show_bug.cgi?id=51940)

Ein isolierter Dienst ist eine einfache Möglichkeit zum Sichern einer Anwendung und das Gerät mit nicht vertrauenswürdigen Code. Beispielsweise kann eine app herunterladen und Ausführen eines Skripts aus einer Website. In diesem Fall dies in einem isolierten Prozess ausführen, wird eine zusätzliche Sicherheitsebene für nicht vertrauenswürdigen Code Gefährdung der Android-Gerät enthält.

> [!IMPORTANT]
> Nachdem ein Dienst exportiert wurde, sollten der Namen des Diensts nicht ändern. Ändern den Namen des Diensts kann andere Anwendungen umbrochen werden, die vom Dienst verwendet werden.

Um die Auswirkungen anzuzeigen, die die `Process` Eigenschaft verfügt, der folgende Screenshot zeigt einen Dienst, der in einem eigenen privaten Prozess:

![Screenshot, der einen Dienst, der in einem privaten Prozess zeigt](out-of-process-services-images/ipc-04.png "Screenshots, der einen Dienst in einem privaten Prozess ausgeführt wird.")

Dieses nächste Screenshot zeigt `Process="com.xamarin.xample.messengerservice.timestampservice_process"` und dem Dienst in einem globalen Prozess ausgeführt:

![Screenshot der ein Dienst, der in einem globalen Prozess](out-of-process-services-images/ipc-05.png "Screenshot eines Diensts in einem globalen Prozess ausgeführt.")

Einmal die `ServiceAttribute` festgelegt wurde, muss der Dienst implementiert einen `Handler`.

### <a name="implementing-a-handler"></a>Implementieren einen Handler

Um Clientanforderungen zu verarbeiten, muss der Dienst implementieren eine `Handler` und überschreiben die `HandleMessage` MethodThis wird die Methode akzeptiert eine `Message` Instanz fest, welche die kapselt den Aufruf der Methode vom Client und wandelt diesen Aufruf in eine Aktion oder eine Aufgabe, die der Dienst ausgeführt wird. Die `Message` Objekt macht eine Eigenschaft mit dem Namen `What` die handelt es sich um einen ganzzahligen Wert an, deren Bedeutung ist zwischen dem Client und dem Dienst freigegeben und bezieht sich auf eine Aufgabe, die der Dienst für den Client ausführen.

Der folgende Codeausschnitt aus der beispielanwendung zeigt ein Beispiel dafür `HandleMessage`. In diesem Beispiel werden die zwei Aktionen, die ein Client anfordern kann, des Diensts angezeigt:

* Die erste Aktion ist ein _Hello, World_ Nachricht, der Client hat eine einfache Meldung an den Dienst gesendet.
* Die zweite Aktion Aufrufen einer Methode für den Dienst und eine Zeichenfolge abgerufen wird, in diesem Fall wird die Zeichenfolge eine Nachricht, die er ausgeführt wurde den Dienst gestartet wurde und wie lange Uhrzeit zurückgibt:

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
                // Call methods on the service to retrive a timestamp message.
                break;
            default:
                Log.Warn(TAG, $"Unknown messageType, ignoring the value {messageType}.");
                base.HandleMessage(msg);
                break;
        }
    }
}
```

Es ist auch möglich, Paketparameter für den Dienst in der `Message`. Dies wird weiter unten in diesem Handbuch besprochen. Im nächste Thema zu berücksichtigen ist das Erstellen der `Messenger` Objekts, das das eingehende `Message`s.

### <a name="instantiating-the-messenger"></a>Instanziieren der Messenger

Wie bereits erläutert wird, deserialisiert der `Message` Objekt und das Ergebnis `Handler.HandleMessage` ist die Möglichkeit, die Verantwortung für die `Messenger` Objekt. Die `Messenger` Klasse bietet auch eine `IBinder` Objekt, das vom Client zum Senden von Nachrichten an den Dienst verwendet wird.  

Wenn der Dienst startet, instanziiert es dann der `Messenger` und Einfügen der `Handler`. Ein guter Ausgangspunkt für diese Initialisierung auszuführen ist, auf die `OnCreate` -Methode des Diensts. Dieser Codeausschnitt ist ein Beispiel für einen Dienst, der einen eigenen initialisiert `Handler` und `Messenger`:

```csharp
private Messenger messenger; // Instance variable for the Messenger

public override void OnCreate()
{
    base.OnCreate();
    messenger = new Messenger(new TimestampRequestHandler(this));
    Log.Info(TAG, $"TimestampService is running in process id {Android.OS.Process.MyPid()}.");
}
```

An diesem Punkt ist der letzte Schritt für die `Service` überschreiben `OnBind`.

### <a name="implementing-serviceonbind"></a>Implementieren von Service.OnBind

An, ob sie in einem eigenen Prozess oder nicht ausgeführt, müssen alle gebundenen Dienste implementieren die `OnBind` Methode. Der Rückgabewert dieser Methode ist ein Objekt, das vom Client verwendet werden kann, um mit dem Dienst interagieren. Genau wie dieses Objekt ist hängt ab, ob der Dienst einen lokalen Dienst oder ein Remotedienst ist. Zwar ein lokaler Dienst ein benutzerdefiniertes zurückgegeben wird `IBinder` Implementierung ein Remotediensts zurück die `IBinder` , gekapselt ist jedoch die `Messenger` , die im vorherigen Abschnitt erstellt wurde:

```csharp
public override IBinder OnBind(Intent intent)
{
    Log.Debug(TAG, "OnBind");
    return messenger.Binder;
}
```

Sobald diese drei Schritte ausgeführt werden, kann der Remotedienst als abgeschlossen betrachtet werden.

## <a name="consuming-the-service"></a>Verarbeiten des Diensts

Alle Clients müssen Code zum möglich gebunden und nutzen den Remotedienst zu implementieren. Aus Sicht des Clients sind im Prinzip nur sehr wenige Unterschiede zwischen der Bindung, lokaler Dienst oder einen Remotedienst. Der Client Ruft die `BindService` -Methode, und übergeben einer expliziten Absicht zum Identifizieren des Diensts und eine `IServiceConnection` , hilft bei die Verbindung zwischen dem Client und dem Dienst verwalten.

Dieser Codeausschnitt ist ein Beispiel zum Erstellen einer **explizite Absicht** für die Bindung an einen Remotedienst. Die Absicht muss das Paket identifizieren, das den Dienst und den Namen des Diensts enthält. Eine Möglichkeit, diese Informationen festzulegen ist die Verwendung einer `Android.Content.ComponentName` Objekt und die, die den Zweck bereitstellen. Dieser Codeausschnitt ist ein Beispiel:  

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

Wenn der Dienst gebunden ist, die `IServiceConnection.OnServiceConnected` Methode wird aufgerufen, und bietet eine `IBinder` an einen Client. Der Client nicht direkt verwendet jedoch die `IBinder`. Stattdessen, instanziiert es ein `Messenger` -Sitzungsobjekts, `IBinder`. Dies ist die `Messenger` , die vom Client für die Interaktion mit dem Remotedienst verwendet.

Im folgenden ist ein Beispiel für eine sehr grundlegende `IServiceConnection` Implementierung, die veranschaulicht, wie ein Client zum Herstellen oder Trennen von einem Dienst behandelt würden. Beachten Sie, dass die `OnServiceConnected` Methode empfängt und `IBinder`, und der Client erstellt einen `Messenger` aus, die `IBinder`:

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

Sobald die Dienst-Verbindung und den Zweck erstellt werden, es ist möglich, dass der Client Aufrufen `BindService` und des Bindungsvorgangs initiieren:

```csharp
IServiceConnection serviceConnection = new TimestampServiceConnection(this);
BindActivity(serviceToStart, serviceConnection, Bind.AutoCreate);
```

Nachdem der Client erfolgreich an den Dienst gebunden ist und die `Messenger` ist verfügbar, es ist möglich, dass der Client sendet `Messages` an den Dienst.

## <a name="sending-messages-to-the-service"></a>Senden von Nachrichten an den Dienst

Sobald der Client verbunden ist, und verfügt über eine `Messenger` -Objekt, es ist möglich, die Kommunikation mit dem Dienst ausgeführt werden, indem `Message` Objekte über die `Messenger`. Diese Kommunikation unidirektional ist, sendet der Client die Nachricht, aber es gibt keine Antwortnachricht vom Dienst an den Client. In dieser Hinsicht dem `Message` ist ein Mechanismus auslösen und vergessen.

Die bevorzugte Methode zum Erstellen einer `Message` Objekt ist die Verwendung der [ `Message.Obtain` ](https://developer.xamarin.com/api/type/Android.OS.Message/#Public%20Methods) Factorymethode. Diese Methode ruft eine `Message` Objekt aus einem globalen Pool, der von Android verwaltet wird. `Message.Obtain` Außerdem verfügt über einige überladene Methoden, mit denen die `Message` Objekt, das mit den Werten und vom Dienst erforderlichen Parameter initialisiert werden.  Sobald die `Message` wird instanziiert, Weiterleitung an den Dienst durch den Aufruf `Messenger.Send`. Dieser Codeausschnitt ist ein Beispiel zum Erstellen und Verteilen einer `Message` an den Dienstprozess:

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

Es gibt mehrere verschiedene Typen von der `Message.Obtain` Methode. Im vorherigen Beispiel wird die [ `Message.Obtain(Handler h, Int32 what)` ](https://developer.xamarin.com/api/member/Android.OS.Message.Obtain/p/Android.OS.Handler/System.Int32/). Da dies eine asynchrone Anforderung an einen Out-of-Process-Dienst ist; Es werden keine Antwort vom Dienst, sodass der `Handler` auf festgelegt ist `null`. Der zweite Parameter `Int32 what`, gespeichert werden soll die `.What` Eigenschaft von der `Message` Objekt. Die `.What` Eigenschaft wird vom Code in den Dienstprozess zum Aufrufen von Methoden für den Dienst verwendet.

Die `Message` Klasse macht auch zwei weitere Eigenschaften, die mithilfe der Recipent möglicherweise: `Arg1` und `Arg2`. Diese beiden Eigenschaften sind ganzzahlige Werte, die einige spezielle möglicherweise vereinbart Werte, die zwischen dem Client und dem Dienst Bedeutung haben. Beispielsweise `Arg1` hält unter Umständen eine Kunden-ID und `Arg2` kann eine Bestellnummer für diesen Kunden enthalten. Die [ `Method.Obtain(Handler h, Int32 what, Int32 arg1, Int32 arg2)` ](https://developer.xamarin.com/api/member/Android.OS.Message.Obtain/p/Android.OS.Handler/System.Int32/System.Int32/System.Int32/) können verwendet werden, um die beiden Eigenschaften festgelegt bei der `Message` wird erstellt. Eine weitere Möglichkeit zum Auffüllen dieser beiden Werte festgelegt werden die `.Arg` und `.Arg2` Eigenschaften direkt auf die `Message` Objekt, nachdem es erstellt wurde.

### <a name="passing-additional-values-to-the-service"></a>Übergeben von zusätzlichen Werte an den Dienst

Es ist möglich, übergeben komplexere Daten an den Dienst mithilfe einer `Bundle`. In diesem Fall für die zusätzlichen Werte platziert werden können, einem `Bundle` und gesendete zusammen mit der `Message` durch Festlegen der [ `.Data` Eigenschaft](https://developer.xamarin.com/api/property/Android.OS.Message.Data/) Eigenschaft vor dem senden.

```csharp
Bundle serviceParameters = new Bundle();
serviceParameters.

var msg = Message.Obtain(null, Constants.SERVICE_TASK_TO_PERFORM);
msg.Data = serviceParameters;

messenger.Send(msg);
```


> [!NOTE]
> Im Allgemeinen eine `Message` müssen sich nicht auf eine Nutzlast, die größer als 1 MB. Das Größenlimit variieren entsprechend der Version von Android und auf proprietären Änderungen möglicherweise der Hersteller der Implementierung von der Android Open Quelle Projekt (AOSP), die mit dem Gerät gebündelt wird vorgenommen haben.

## <a name="returning-values-from-the-service"></a>Zurückgeben von Werten aus dem Dienst

Der messaging-Architektur, die zum angegebenen Zeitpunkt diskutiert ist unidirektional, der Client sendet eine Nachricht an den Dienst. Bei Bedarf für den Dienst, der einen Wert an einen Client zurückgeben, wird alles, die zum angegebenen Zeitpunkt diskutiert umgekehrt. Erstellen des Diensts muss ein `Message`verpackte Rückgabewerte und die Verteilung der `Message` über eine `Messenger` an den Client. Allerdings der Dienst erstellt keine eigenen `Messenger`; stattdessen es beruht auf dem Client instanziieren und dem Paket einen `Messenger` als Teil der ursprünglichen Anforderung. Der Dienst `Send` die Nachricht mit diesem Client bereitgestellte `Messenger`.  

Die Abfolge von Ereignissen für die bidirektionale Kommunikation wird:

1. Der Client bindet an den Dienst. Wenn der Dienst und den Client verbinden, die `IServiceConnection` , wird beibehalten, indem der Client muss einen Verweis auf eine `Messenger` -Objekt, das verwendet wird, übertragen `Message`s, um den Dienst. Um Verwirrung zu vermeiden, dies wird als bezeichnet werden die _Messenger-Dienst_.
2. Client instanziiert einen `Handler` (genannt der _Client Handler_) dann verwendet, um eine eigene initialisieren `Messenger` (die _Client Messenger_). Beachten Sie, dass der Messenger-Dienst und der Client Messenger zwei unterschiedliche Objekte sind, die in zwei verschiedene Richtungen-Datenverkehr zu bewältigen. Der Messenger-Dienst verarbeitet die Nachrichten vom Client an den Dienst während der Client Messenger Nachrichten vom Dienst an den Client behandelt.
3. Der Client erstellt einen `Message` -Objekt und stellt die `ReplyTo` Eigenschaft mit dem Client Messenger. Die Nachricht wird dann an den Dienst mithilfe der Messenger-Dienst gesendet.
4. Der Dienst empfängt die Nachricht vom Client und führt die erforderlichen Aufgaben.
5. Wenn Zeitpunkt für den Dienst zum Senden der Antwort an den Client erreicht ist, wird es verwendet `Message.Obtain` zum Erstellen eines neuen `Message` Objekt.
6. Um diese Nachricht an den Client zu senden, wird der Dienst der Messenger-Client aus extrahiert der `.ReplyTo` Eigenschaft des Clients-Nachricht und verwenden, um `.Send` der `Message` zurück an den Client.
7. Wenn die Antwort vom Client empfangen wird, sie hat ein eigenes `Handler` verarbeitet, die die `Message` durch Überprüfen der `.What` Eigenschaft (und bei Bedarf, extrahieren Sie alle Parameter enthalten sind die `Message`).

In diesem Beispielcode wird veranschaulicht, wie der Client instanziiert, die `Message` und Packen Sie eine `Messenger` , die der Dienst sollte für die Antwort verwenden:

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

Der Dienst muss einige Änderungen vornehmen, einen eigenen `Handler` zum Extrahieren der `Messenger` und verwenden, die zum Senden von Antworten an den Client. Dieser Codeausschnitt ist ein Beispiel dafür, wie des Diensts `Handler` würde eine `Message` und zurück an den Client zu senden:  

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

Beachten Sie, dass in den obigen Codebeispielen die `Messenger` Instanz, die vom Client erstellt wird, ist *nicht* das gleiche Objekt, das vom Dienst empfangen wird. Dies sind zwei unterschiedliche `Messenger` Objekte, die in zwei separate Vorgänge, die den Kommunikationskanal darstellen.

## <a name="securing-the-service-with-android-permissions"></a>Sichern den Dienst mit der Android-Berechtigungen

Ein Dienst, der in einem globalen Prozess ausgeführt wird, kann zugegriffen werden, von allen Anwendungen, die auf diesem Gerät Android ausgeführt. In einigen Situationen dieser Zugänglichkeit und Verfügbarkeit ist nicht wünschenswert, und es ist notwendig, den Dienst für den Zugriff von nicht autorisierten Clients zu sichern. Eine Möglichkeit, den Zugriff auf den Remotedienst beschränken ist die Verwendung von Android-Berechtigungen.

Berechtigungen können festgestellt werden, indem die `Permission` Eigenschaft von der `ServiceAttribute` ergänzt, die die `Service` Unterklasse. Dadurch wird eine Berechtigung benennen, die der Client beim Binden an den Dienst erteilt werden muss. Wenn der Client verfügt nicht über die entsprechenden Berechtigungen, Android löst eine `Java.Lang.SecurityException` Wenn der Client versucht, an den Dienst zu binden.

Es gibt vier unterschiedliche Berechtigungsstufen Android bietet:

* **normale** &ndash; Dies ist die Standard-Berechtigungsebene. Es wird verwendet, mit niedrigem Risiko Berechtigungen identifizieren, die automatisch von Android Clients erteilt werden können, die sie anfordern. Der Benutzer verfügt nicht über diese Berechtigungen explizit gewähren, aber die Berechtigungen können in den Einstellungen der app angezeigt werden.
* **Signatur** &ndash; Dies ist eine spezielle Kategorie der Berechtigung, die automatisch von Android auf Anwendungen erteilt werden, die alle mit demselben Zertifikat signiert sind. Durch diese Berechtigung ist darauf ausgelegt, problemlos für Anwendungsentwickler, Komponenten oder Daten zwischen apps ohne erfährt des Benutzers für Konstante Genehmigungen freizugeben.
* **SignatureOrSystem** &ndash; Dies ist vergleichbar mit der **Signatur** Berechtigungen, die oben beschriebenen. Abgesehen davon, dass automatisch gewährt, apps, die mit demselben Zertifikat signiert sind, wird diese Berechtigung auch erteilt für apps, die signiert sind die dasselbe Zertifikat zum Signieren der apps verwendet wurde, mit der Android Systemabbild installiert. Diese Berechtigung ist in der Regel nur von ROM Android-Entwickler verwendet damit ihre Anwendungen mit Drittanbieter-apps arbeiten können. Es ist im Allgemeinen nicht von apps verwendet, die allgemeine Verteilung für die Öffentlichkeit vorgesehen sind.
* **gefährliche** &ndash; problematische Berechtigungen sind solche, die für den Benutzer Probleme verursachen kann. Aus diesem Grund **gefährliche** Berechtigungen explizit vom Benutzer genehmigt werden müssen.

Da `signature` und `normal` Berechtigungen werden automatisch gewährt installierten Zeitpunkt von Android, ist es entscheidend, dass APK Hosten des Diensts installiert werden **vor** der APK mit dem Client aus. Wenn der Client zuerst installiert ist, können Sie Android nicht die Berechtigungen gewähren. In diesem Fall wird es erforderlich sein, die APK-Client deinstallieren und installieren Sie den Dienst APK APK Client neu installieren.

Es gibt zwei allgemeine Verfahren zum Sichern eines Diensts mit Android Berechtigungen:

1.  **Implementieren von Signatur-Sicherheit auf Zeilenebene** &ndash; Signatur-Sicherheit auf Zeilenebene bedeutet, dass die Berechtigung automatisch gewährt von der jeweiligen Anwendung, die mit demselben Schlüssel signiert sind, die zum Signieren der APK, halten den Dienst verwendet wurde. Dies ist eine einfache Möglichkeit für Entwickler, sichern ihren Dienst noch bleiben in ihren eigenen Anwendungen zugegriffen werden kann. Berechtigungsstufe Signatur werden deklariert, durch Festlegen der `Permission` Eigenschaft der `ServiceAttribute` auf `signature`:

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process="com.xamarin.TimestampService.timestampservice_process",
             Permission="signature")]
    public class TimestampService : Service
    {
    }
    ```

2.  **Erstellen Sie eine benutzerdefinierte Berechtigung** &ndash; es ist möglich, dass der Entwickler des Diensts, um eine benutzerdefinierte Berechtigung für den Dienst zu erstellen. Dies wird empfohlen, wenn ein Entwickler möchte, ihren Dienst mit Anwendungen von anderen Entwicklern gemeinsam verwenden. Eine benutzerdefinierte Berechtigung erfordert etwas mehr Aufwand zum Implementieren und werden im folgenden ausführlicher.

Ein vereinfachtes Beispiel zum Erstellen einer benutzerdefinierten `normal` Berechtigung wird im nächsten Abschnitt beschrieben werden. Weitere Informationen zu Berechtigungen für Android finden Sie in Google Dokumentation für [Best Practices und Sicherheit](https://developer.android.com/training/articles/security-tips.html). Weitere Informationen zu Berechtigungen für Android finden Sie unter der [Abschnitt "Berechtigungen"](https://developer.android.com/guide/topics/manifest/manifest-intro.html#perms) der Android-Dokumentation für das Anwendungsmanifest für Weitere Informationen zu Android-Berechtigungen.

> [!NOTE]
> Im allgemeinen [Google rät davon ab, zu der Verwendung von benutzerdefinierten Berechtigungen](https://developer.android.com/training/articles/security-tips.html#RequestingPermissions) , wie sie für Benutzer verwirrend erweisen.

### <a name="creating-a-custom-permission"></a>Erstellen eine benutzerdefinierte Berechtigung

Um eine benutzerdefinierte Berechtigung verwenden, wird er vom Dienst deklariert, während der Client fordert die Berechtigung explizit an.

So erstellen Sie eine Berechtigung in den Dienst APK, eine `permission` Element wird hinzugefügt, um die `manifest` Element im **AndroidManifest.xml**. Durch diese Berechtigung benötigen die `name`, `protectionLevel`, und `label` Attribute festgelegt. Die `name` Attribut muss festgelegt werden, um eine Zeichenfolge, die die Berechtigung eindeutig identifiziert. Der Name erscheint der **AppInfo** -Ansicht der **Android-Einstellungen** (wie im nächsten Abschnitt gezeigt).

Die `protectionLevel` Attribut muss festgelegt werden, auf einen der vier Zeichenfolgenwerte, die oben beschrieben wurden.  Die `label` und `description` muss auf Zeichenfolgenressourcen verweisen und werden verwendet, um einen benutzerfreundlichen Namen und eine Beschreibung für den Benutzer bereitstellen.

Dieser Codeausschnitt ist ein Beispiel des Deklarierens einer benutzerdefiniertes `permission` -Attribut im **AndroidManifest.xml** von der APK, die den Dienst enthält:

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

Anschließend wird die **AndroidManifest.xml** des Clients muss APK diese neue Berechtigung explizit anfordern. Dies erfolgt durch Hinzufügen der `users-permission` -Attribut auf die **AndroidManifest.xml**:

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

### <a name="view-the-permissions-granted-to-an-app"></a>Zeigen Sie die Berechtigungen für eine App an.

Klicken Sie zum Anzeigen der Berechtigungen, die eine Anwendung erteilt wurden, öffnen Sie die Einstellungen für Android-app, und wählen **Apps**. Suchen Sie, und wählen Sie die Anwendung in der Liste aus. Aus der **AppInfo** tippen **Berechtigungen** das wird angezeigt, eine Ansicht, die alle Berechtigungen für die app zeigt:

[![Bildschirmfotos Gewusst wie: Suchen Sie die Berechtigungen für eine Anwendung mit einem Android-Gerät](out-of-process-services-images/ipc-06-sml.png)](out-of-process-services-images/ipc-06.png#lightbox)

## <a name="summary"></a>Zusammenfassung

Dieses Handbuch wurde eine erweiterte Diskussion darüber, wie Sie einen Android-Dienst in einem remote-Prozess ausgeführt. Die Unterschiede zwischen einem lokalen und eines Remotediensts wurde erläutert, sowie einige Gründe, warum ein Remotedienst auf Stabilität und Leistung von Android-app hilfreich sein können. Nach erläutern, wie Sie einen Remotedienst zu implementieren und wie ein Client mit dem Dienst kommunizieren kann, ein Beispiel ist im Handbuch auf bieten eine Möglichkeit, den Zugriff auf den Dienst nur von autorisierten Clients einschränken.


## <a name="related-links"></a>Verwandte Links

- [Ereignishandler](https://developer.xamarin.com/api/type/Android.OS.Handler/)
- [Message](https://developer.xamarin.com/api/type/Android.OS.Message/)
- [Messenger](https://developer.xamarin.com/api/type/Android.OS.Messenger/)
- [ServiceAttribute](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute)
- [Das Attribut exportiert](https://developer.android.com/guide/topics/manifest/service-element.html#exported)
- [Nicht-Diensten mit isolierten Prozesse und benutzerdefinierte Anwendungsklasse Überladungen ordnungsgemäß aufgelöst](https://bugzilla.xamarin.com/show_bug.cgi?id=51940)
- [Prozesse und Threads](https://developer.android.com/guide/components/processes-and-threads.html)
- [Android-Manifest - Berechtigungen](https://developer.android.com/guide/topics/manifest/manifest-intro.html#perms)
- [Tipps zur Sicherheit](https://developer.android.com/training/articles/security-tips.html)
- [MessengerServiceDemo (Beispiel)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/MessengerServiceDemo/)
