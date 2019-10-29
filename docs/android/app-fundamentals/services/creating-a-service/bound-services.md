---
title: Gebundene Dienste in xamarin. Android
description: Gebundene Dienste sind Android-Dienste, die eine Client-Server-Schnittstelle bereitstellen, die ein Client (z. b. eine Android-Aktivität) mit interagieren kann. In diesem Leitfaden werden die wichtigsten Komponenten erläutert, die beim Erstellen eines gebundenen Dienstanbieter und der Verwendung in einer xamarin. Android-Anwendung beteiligt sind.
ms.prod: xamarin
ms.assetid: 809ECE88-EF08-4E9A-B389-A2DC08C51A6E
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/04/2018
ms.openlocfilehash: a3b0e8499d208f209de481163a236e5241c83ee6
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73024995"
---
# <a name="bound-services-in-xamarinandroid"></a>Gebundene Dienste in xamarin. Android

_Gebundene Dienste sind Android-Dienste, die eine Client-Server-Schnittstelle bereitstellen, die ein Client (z. b. eine Android-Aktivität) mit interagieren kann. In diesem Leitfaden werden die wichtigsten Komponenten erläutert, die beim Erstellen eines gebundenen Dienstanbieter und der Verwendung in einer xamarin. Android-Anwendung beteiligt sind._

## <a name="bound-services-overview"></a>Übersicht über gebundene Dienste

Dienste, die eine Client-/Serverschnittstelle bereitstellen, mit der Clients direkt mit dem Dienst interagieren können, werden als _gebundene Dienste_bezeichnet.  Es können mehrere Clients gleichzeitig mit einer einzelnen Instanz eines Dienstanbieter verbunden sein. Der gebundene Dienst und der Client sind voneinander isoliert. Stattdessen stellt Android eine Reihe von zwischen Objekten bereit, die den Status der Verbindung zwischen den beiden verwalten. Dieser Zustand wird von einem Objekt verwaltet, das die [`Android.Content.IServiceConnection`](xref:Android.Content.IServiceConnection) -Schnittstelle implementiert.  Dieses Objekt wird vom Client erstellt und als Parameter an die [`BindService`](xref:Android.Content.Context.BindService*) -Methode übergeben. Der `BindService` ist auf jedem [`Android.Content.Context`](xref:Android.Content.Context) Objekt (z. b. einer Aktivität) verfügbar. Es ist eine Anforderung an das Android-Betriebssystem, den Dienst zu starten und einen Client an ihn zu binden. Es gibt drei Möglichkeiten, wie ein Client mithilfe der `BindService`-Methode eine Bindung an einen Dienst herstellen kann:

- **Ein Dienst Binder** &ndash; ein Dienst Binder ist eine Klasse, die die [`Android.OS.IBinder`](xref:Android.OS.IBinder) -Schnittstelle implementiert. Die meisten Anwendungen implementieren diese Schnittstelle nicht direkt, sondern erweitern die [`Android.OS.Binder`](xref:Android.OS.Binder) -Klasse. Dies ist der häufigste Ansatz und eignet sich für den Fall, dass der Dienst und der Client im selben Prozess vorhanden sind.
- Die **Verwendung eines Messenger** -&ndash; dieses Verfahren eignet sich für den Fall, dass der Dienst in einem separaten Prozess vorhanden sein kann. Stattdessen werden Dienst Anforderungen über einen [`Android.OS.Messenger`](xref:Android.OS.Messenger)zwischen Client und Dienst gemarshallt. Im Dienst wird eine [`Android.OS.Handler`](xref:Android.OS.Handler) erstellt, die die `Messenger` Anforderungen behandelt. Dies wird in einem anderen Handbuch behandelt.
- Die **Verwendung von aidl (Android Interface Definition Language)** -&ndash; [aidl](https://developer.android.com/guide/components/aidl) ist eine erweiterte Technik, die in diesem Handbuch nicht behandelt wird.

Nachdem ein Client an einen Dienst gebunden wurde, erfolgt die Kommunikation zwischen den beiden über `Android.OS.IBinder`-Objekt.  Dieses Objekt ist verantwortlich für die-Schnittstelle, die es dem Client ermöglicht, mit dem Dienst zu interagieren. Es ist nicht erforderlich, dass jede xamarin. Android-Anwendung diese Schnittstelle von Grund auf neu implementiert. die Android SDK stellt die [`Android.OS.Binder`](xref:Android.OS.Binder) Klasse bereit, die den größten Teil des Codes übernimmt, der beim Marshalling des Objekts zwischen dem Client und dem Dienst erforderlich ist.

Wenn ein Client mit dem Dienst ausgeführt wird, muss die Bindung an ihn durch Aufrufen der `UnbindService`-Methode aufgehoben werden. Nachdem die Bindung des letzten Clients an einen Dienst aufgehoben wurde, wird der gebundene Dienst von Android beendet und verworfen.

Dieses Diagramm veranschaulicht, wie die Aktivität, die Dienst Verbindung, der Binder und der Dienst alle miteinander verknüpft sind:

![Ein Diagramm, das zeigt, wie die Dienst Komponenten zueinander zueinander stehen](bound-services-images/bound-services-02.png "Ein Diagramm, das zeigt, wie die Dienst Komponenten miteinander in Beziehung stehen.")

In dieser Anleitung wird erläutert, wie Sie die `Service`-Klasse erweitern, um einen gebundenen Dienst zu implementieren. Außerdem wird die Implementierung von `IServiceConnection` und die Erweiterung `Binder` behandelt, damit ein Client mit dem Dienst kommunizieren kann. Eine Beispiel-App begleitet dieses Handbuch, das eine Projekt Mappe mit einem einzelnen xamarin. Android-Projekt namens **[boundservicedemo](https://github.com/xamarin/monodroid-samples/tree/master/ApplicationFundamentals/ServiceSamples/BoundServiceDemo)** enthält. Dabei handelt es sich um eine sehr einfache Anwendung, die veranschaulicht, wie ein Dienst implementiert wird und wie eine Aktivität an ihn gebunden wird. Der gebundene Dienst verfügt über eine sehr einfache API mit nur einer Methode, `GetFormattedTimestamp`, die eine Zeichenfolge zurückgibt, die den Benutzer darüber informiert, wann der Dienst gestartet wurde und wie lange er ausgeführt wurde. Mit der APP kann der Benutzer die Bindung und Bindung an den Dienst auch manuell aufheben.

[![Screenshot der Anwendung, die auf einem Android-Telefon ausgeführt wird](bound-services-images/bound-services-03-sml.png)](bound-services-images/bound-services-03.png#lightbox)

## <a name="implementing-and-consuming-a-bound-service"></a>Implementieren und Nutzen eines gebundenen Dienstanbieter

Es gibt drei Komponenten, die implementiert werden müssen, damit eine Android-Anwendung einen gebundenen Dienst nutzt:

1. **Erweitern Sie die `Service`-Klasse, und implementieren Sie die-Lebenszyklus Rückruf Methoden** &ndash; diese Klasse enthält den Code, mit dem die Arbeit ausgeführt wird, die vom Dienst angefordert wird. Dies wird im folgenden ausführlicher beschrieben.
2. **Erstellen Sie eine Klasse, die `IServiceConnection` implementiert** &ndash; diese Schnittstelle stellt Rückruf Methoden bereit, die von Android aufgerufen werden, um den Client zu benachrichtigen, wenn sich die Verbindung mit dem Dienst geändert hat, d. h., der Client hat eine Verbindung mit dem Dienst hergestellt oder Die Dienst Verbindung bietet auch einen Verweis auf ein Objekt, das der Client verwenden kann, um direkt mit dem Dienst zu interagieren. Dieser Verweis wird als _Binder_bezeichnet.
3. **Erstellen Sie eine Klasse, die `IBinder` implementiert,** &ndash; eine _Binder_ Implementierung die API bereitstellt, die von einem Client für die Kommunikation mit dem Dienst verwendet wird. Der Binder kann entweder einen Verweis auf den gebundenen Dienst bereitstellen, sodass Methoden direkt aufgerufen werden können, oder der Binder kann eine Client-API bereitstellen, die den gebundenen Dienst aus der Anwendung kapselt und verbirgt. Ein `IBinder` muss den erforderlichen Code für Remote Prozedur Aufrufe bereitstellen. Es ist nicht erforderlich (oder empfohlen), die `IBinder`-Schnittstelle direkt zu implementieren. Stattdessen sollten Anwendungen den `Binder` Typ erweitern, der den größten Teil der für eine `IBinder` erforderlichen Basisfunktionalität bereitstellt.
4. **Starten und Binden an einen Dienst** &ndash; nachdem die Dienst Verbindung, der Binder und der Dienst erstellt wurden, ist die Android-Anwendung dafür verantwortlich, den Dienst zu starten und zu binden.

Jeder dieser Schritte wird in den folgenden Abschnitten ausführlicher erläutert.

### <a name="extend-the-service-class"></a>Erweitern der `Service`-Klasse

Um einen Dienst mithilfe von xamarin. Android zu erstellen, ist es erforderlich, `Service` zu Unterklassen und die Klasse mit dem [`ServiceAttribute`](xref:Android.App.ServiceAttribute)zu schmücken. Das-Attribut wird von den xamarin. Android-Buildtools verwendet, um den Dienst in der Datei " **androidmanifest. XML** " der APP ordnungsgemäß zu registrieren, ähnlich wie bei einer Aktivität. ein gebundener Dienst hat seine eigenen Lebenszyklus-und Rückruf Methoden, die den bedeutenden Ereignissen in zugeordnet sind IT-Lebenszyklus. Die folgende Liste ist ein Beispiel für einige der gängigeren Rückruf Methoden, die von einem Dienst implementiert werden:

- `OnCreate` &ndash; diese Methode von Android aufgerufen wird, während Sie den Dienst instanziiert. Sie wird verwendet, um alle Variablen oder Objekte zu initialisieren, die während ihrer Lebensdauer vom Dienst benötigt werden. Diese Methode ist optional.
- `OnBind` &ndash; diese Methode von allen gebundenen Diensten implementiert werden muss. Sie wird aufgerufen, wenn der erste Client versucht, eine Verbindung mit dem Dienst herzustellen. Es wird eine Instanz von `IBinder` zurückgegeben, damit der Client mit dem Dienst interagieren kann. Solange der Dienst ausgeführt wird, wird das `IBinder` Objekt verwendet, um zukünftige Client Anforderungen für die Bindung an den Dienst zu erfüllen.
- `OnUnbind` &ndash; diese Methode aufgerufen wird, wenn alle gebundenen Clients nicht gebunden wurden. Wenn Sie `true` von dieser Methode zurückgeben, ruft der Dienst später `OnRebind` mit der Absicht auf, die an `OnUnbind` weitergeleitet wird, wenn neue Clients an ihn binden. Dies würden Sie tun, wenn ein Dienst weiterhin ausgeführt wird, nachdem die Bindung aufgehoben wurde. Dies wäre der Fall, wenn der vor kurzem nicht gebundene Dienst auch ein gestarteter Dienst wäre und `StopService` oder `StopSelf` nicht aufgerufen wurde. In einem solchen Szenario ermöglicht `OnRebind` das Abrufen der Absicht. Der Standardwert gibt `false` zurück, der nichts bewirkt. Dies ist optional.
- `OnDestroy` &ndash; diese Methode aufgerufen wird, wenn Android den Dienst zerstört. Alle erforderlichen Bereinigungs Vorgänge, z. b. das Freigeben von Ressourcen, sollten in dieser Methode durchgeführt werden. Dies ist optional.

Die wichtigsten Lebenszyklus Ereignisse eines gebundenen Dienstanbieter werden in diesem Diagramm dargestellt:

![Ein Diagramm, das die Reihenfolge anzeigt, in der die Lebenszyklus Methoden aufgerufen werden](bound-services-images/bound-services-01.png "Ein Diagramm, das die Reihenfolge anzeigt, in der die Lebenszyklus Methoden aufgerufen werden.")

Der folgende Code Ausschnitt aus der begleitenden Anwendung, die diese Anleitung begleitet, zeigt, wie ein gebundener Dienst in xamarin. Android implementiert wird:

```csharp
using Android.App;
using Android.Util;
using Android.Content;
using Android.OS;

namespace BoundServiceDemo
{
    [Service(Name="com.xamarin.ServicesDemo1")]
    public class TimestampService : Service, IGetTimestamp
    {
        static readonly string TAG = typeof(TimestampService).FullName;
        IGetTimestamp timestamper;

        public IBinder Binder { get; private set; }

        public override void OnCreate()
        {
            // This method is optional to implement
            base.OnCreate();
            Log.Debug(TAG, "OnCreate");
            timestamper = new UtcTimestamper();
        }

        public override IBinder OnBind(Intent intent)
        {
            // This method must always be implemented
            Log.Debug(TAG, "OnBind");
            this.Binder = new TimestampBinder(this);
            return this.Binder;
        }

        public override bool OnUnbind(Intent intent)
        {
            // This method is optional to implement
            Log.Debug(TAG, "OnUnbind");
            return base.OnUnbind(intent);
        }

        public override void OnDestroy()
        {
            // This method is optional to implement
            Log.Debug(TAG, "OnDestroy");
            Binder = null;
            timestamper = null;
            base.OnDestroy();
        }

        /// <summary>
        /// This method will return a formatted timestamp to the client.
        /// </summary>
        /// <returns>A string that details what time the service started and how long it has been running.</returns>
        public string GetFormattedTimestamp()
        {
            return timestamper?.GetFormattedTimestamp();
        }
    }
}
```

Im Beispiel Initialisiert die `OnCreate`-Methode ein-Objekt, das die Logik zum Abrufen und Formatieren eines Zeitstempels enthält, der von einem Client angefordert wird. Wenn der erste Client versucht, eine Bindung an den Dienst herzustellen, ruft Android die `OnBind` Methode auf. Dieser Dienst instanziiert ein `TimestampBinder` Objekt, das den Clients den Zugriff auf diese Instanz des laufenden Dienstanbieter gestattet. Die `TimestampBinder`-Klasse wird im nächsten Abschnitt erläutert.

### <a name="implementing-ibinder"></a>Implementieren von IBinder

Wie bereits erwähnt, stellt ein `IBinder` Objekt den Kommunikationskanal zwischen einem Client und einem Dienst bereit. Android-Anwendungen sollten die `IBinder`-Schnittstelle nicht implementieren. die [`Android.OS.Binder`](xref:Android.OS.Binder) sollten erweitert werden. Die `Binder`-Klasse stellt einen Großteil der erforderlichen-Infrastruktur bereit, die notwendig ist, um das Binder Objekt aus dem Dienst (der möglicherweise in einem separaten Prozess ausgeführt wird) an den Client zu Mars Hallen. In den meisten Fällen sind die `Binder`-Unterklasse nur wenige Codezeilen und umschließt einen Verweis auf den Dienst. In diesem Beispiel verfügt `TimestampBinder` über eine-Eigenschaft, die die `TimestampService` für den Client verfügbar macht:

```csharp
public class TimestampBinder : Binder
{
    public TimestampBinder(TimestampService service)
    {
        this.Service = service;
    }

    public TimestampService Service { get; private set; }
}
```

Diese `Binder` ermöglicht das Aufrufen der öffentlichen Methoden für den Dienst. Zum Beispiel:

```csharp
string currentTimestamp = serviceConnection.Binder.Service.GetFormattedTimestamp()
```

Nachdem `Binder` erweitert wurde, müssen `IServiceConnection` implementiert werden, um alles miteinander zu verbinden.

### <a name="creating-the-service-connection"></a>Erstellen der Dienst Verbindung

Der `IServiceConnection` wird angezeigt | Einführung | verfügbar | verbinden Sie das `Binder` Objekt mit dem Client. Zusätzlich zum Implementieren der `IServiceConnection`-Schnittstelle muss die-Klasse `Java.Lang.Object` erweitern. Die Dienst Verbindung sollte auch eine bestimmte Art und Weise bereitstellen, dass der Client auf den `Binder` zugreifen kann (und somit mit dem gebundenen Dienst kommunizieren kann).

Dieser Code aus dem zugehörigen Beispiel Projekt ist eine Möglichkeit zum Implementieren von `IServiceConnection`:

```csharp
using Android.Util;
using Android.OS;
using Android.Content;

namespace BoundServiceDemo
{
    public class TimestampServiceConnection : Java.Lang.Object, IServiceConnection, IGetTimestamp
    {
        static readonly string TAG = typeof(TimestampServiceConnection).FullName;

        MainActivity mainActivity;
        public TimestampServiceConnection(MainActivity activity)
        {
            IsConnected = false;
            Binder = null;
            mainActivity = activity;
        }

        public bool IsConnected { get; private set; }
        public TimestampBinder Binder { get; private set; }

        public void OnServiceConnected(ComponentName name, IBinder service)
        {
            Binder = service as TimestampBinder;
            IsConnected = this.Binder != null;

            string message = "onServiceConnected - ";
            Log.Debug(TAG, $"OnServiceConnected {name.ClassName}");

            if (IsConnected)
            {
                message = message + " bound to service " + name.ClassName;
                mainActivity.UpdateUiForBoundService();
            }
            else
            {
                message = message + " not bound to service " + name.ClassName;
                mainActivity.UpdateUiForUnboundService();
            }

            Log.Info(TAG, message);
            mainActivity.timestampMessageTextView.Text = message;

        }

        public void OnServiceDisconnected(ComponentName name)
        {
            Log.Debug(TAG, $"OnServiceDisconnected {name.ClassName}");
            IsConnected = false;
            Binder = null;
            mainActivity.UpdateUiForUnboundService();
        }

        public string GetFormattedTimestamp()
        {
            if (!IsConnected)
            {
                return null;
            }

            return Binder?.GetFormattedTimestamp();
        }
    }
}

```

Im Rahmen des Bindungs Vorgangs ruft Android die `OnServiceConnected`-Methode auf, die die `name` des gebundenen diensdienstaners und die `binder` bereitstellt, die einen Verweis auf den Dienst selbst enthält. In diesem Beispiel verfügt die Dienst Verbindung über zwei Eigenschaften: eine, die einen Verweis auf den Binder enthält, und ein boolesches Flag für, wenn der Client mit dem Dienst verbunden ist oder nicht.

Die `OnServiceDisconnected`-Methode wird nur aufgerufen, wenn die Verbindung zwischen einem Client und einem Dienst unerwartet verloren geht oder unterbrochen wird. Diese Methode ermöglicht es dem Client, auf die Dienst Unterbrechung zu reagieren.  

## <a name="starting-and-binding-to-a-service-with-an-explicit-intent"></a>Starten und Binden an einen Dienst mit einer expliziten Absicht

Um einen gebundenen Dienst zu verwenden, muss ein-Client (z. b. eine-Aktivität) ein Objekt instanziieren, das `Android.Content.IServiceConnection` implementiert, und die `BindService`-Methode aufrufen. `BindService` gibt `true` zurück, wenn der Dienst an gebunden ist, `false`, wenn dies nicht der Fall ist. Die Methode `BindService` akzeptiert drei Parameter:

- **Eine `Intent`** &ndash; der Absicht sollte explizit ermitteln, mit welchem Dienst eine Verbindung hergestellt werden soll.
- **Ein `IServiceConnection` Objekt** &ndash; dieses-Objekt ist ein Vermittler, der Rückruf Methoden bereitstellt, um den Client zu benachrichtigen, wenn der gebundene Dienst gestartet und beendet wird.
- **[`Android.Content.Bind`](xref:Android.Content.Bind)** Enumeration &ndash; dieser Parameter ist ein Satz von Flags, die vom System verwendet werden, wenn das Objekt gebunden wird. Der am häufigsten verwendete Wert ist [`Bind.AutoCreate`](xref:Android.Content.Bind.AutoCreate), der den Dienst automatisch startet, wenn er nicht bereits ausgeführt wird.

Der folgende Code Ausschnitt ist ein Beispiel dafür, wie ein gebundener Dienst in einer Aktivität mithilfe einer expliziten Absicht gestartet wird:

```csharp
protected override void OnStart ()
{
    base.OnStart ();

    if (serviceConnection == null)
    {
        this.serviceConnection = new TimestampServiceConnection(this);
    }

    Intent serviceToStart = new Intent(this, typeof(TimestampService));
    BindService(serviceToStart, this.serviceConnection, Bind.AutoCreate);

}
```

> [!IMPORTANT]
> Ab Android 5,0 (API-Ebene 21) ist es nur möglich, eine Bindung an einen Dienst mit einer expliziten Absicht herzustellen.

## <a name="architectural-notes-about-the-service-connection-and-the-binder"></a>Architektur Hinweise zur Dienst Verbindung und zum Binder.

Einige OOP-Löschvorgänge können die vorherige Implementierung der `TimestampBinder`-Klasse ablehnen, da dies ein Verstoß gegen das [Gesetz von Demeter](https://en.wikipedia.org/wiki/Law_of_Demeter) ist, das in der einfachsten Form besagt, dass Sie nicht mit fremden reden. sprechen Sie nur mit Ihren Freunden. " Diese spezielle Implementierung macht die konkrete `TimestampService` Klasse für alle Clients verfügbar.

Streng genommen ist es nicht notwendig, dass der Client über die `TimestampService` informiert und diese konkrete Klasse für Clients verfügbar macht, sodass eine Anwendung für die Lebensdauer der Anwendung etwas komplizierter und schwieriger zu verwalten ist. Ein alternativer Ansatz besteht darin, eine Schnittstelle zu verwenden, die die `GetFormattedTimestamp()`-Methode verfügbar macht, und Proxy Aufrufe an den Dienst über die `Binder` (oder möglicherweise die Dienst Verbindungs Klasse):  

```csharp
public class TimestampBinder : Binder, IGetTimestamp
{
    TimestampService service;
    public TimestampBinder(TimestampService service)
    {
        this.service = service;
    }

    public string GetFormattedTimestamp()
    {
        return service?.GetFormattedTimestamp();
    }
}
```

In diesem speziellen Beispiel kann eine Aktivität Methoden für den Dienst selbst aufrufen:

```csharp
// In this example the Activity is only talking to a friend, i.e. the IGetTimestamp interface provided by the Binder.
string currentTimestamp = serviceConnection.Binder.GetFormattedTimestamp()
```

## <a name="related-links"></a>Verwandte Links

- [Android. app. Service](xref:Android.App.Service)
- [Android. Content. Bind](xref:Android.Content.Bind)
- [Android. Content. Context](xref:Android.Content.Context)
- [Android. Content. iserviceconnetction](xref:Android.Content.IServiceConnection)
- [Android. OS. Binder](xref:Android.OS.Binder)
- [Android. OS. IBinder](xref:Android.OS.IBinder)
- [Boundservicedemo (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-servicesamples-boundservicedemo)
