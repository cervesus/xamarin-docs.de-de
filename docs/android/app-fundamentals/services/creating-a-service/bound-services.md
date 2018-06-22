---
title: Gebunden Sie Dienste werden im Xamarin.Android.
description: Gebundene Dienste sind Android Dienste, die eine Client / Server-Schnittstelle bereitstellen, der mit ein Client (z. B. eine Android-Aktivität) interagieren kann. Dieses Handbuch werden die wichtigsten Komponenten der mit dem Erstellen eines gebundenen Diensts und wie für die Verwendung in einer Anwendung Xamarin.Android erläutert.
ms.prod: xamarin
ms.assetid: 809ECE88-EF08-4E9A-B389-A2DC08C51A6E
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 05/04/2018
ms.openlocfilehash: f4fe1bd753260f05dedb452655572d290c0781d0
ms.sourcegitcommit: daa089d41cfe1ed0456d6de2f8134cf96ae072b1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
ms.locfileid: "33850573"
---
# <a name="bound-services-in-xamarinandroid"></a>Gebunden Sie Dienste werden im Xamarin.Android.

_Gebundene Dienste sind Android Dienste, die eine Client / Server-Schnittstelle bereitstellen, der mit ein Client (z. B. eine Android-Aktivität) interagieren kann. Dieses Handbuch werden die wichtigsten Komponenten der mit dem Erstellen eines gebundenen Diensts und wie für die Verwendung in einer Anwendung Xamarin.Android erläutert._

## <a name="bound-services-overview"></a>Gebunden (Übersicht)

Dienste, die eine Client / Server-Schnittstelle für Clients die direkte Interaktion mit dem Dienst bereitstellen, werden als bezeichnet _gebunden Services_.  Es können mehrere Clients, die auf eine einzelne Instanz eines Diensts gleichzeitig verbunden sein. Der gebundene-Dienst und der Client sind voneinander isoliert. Stattdessen bietet Android einer Reihe von Zwischenobjekten, die den Zustand der Verbindung zwischen den beiden zu verwalten. Dieser Status wird durch ein Objekt, das implementiert verwaltet die [ `Android.Content.IServiceConnection` ](https://developer.xamarin.com/api/type/Android.Content.IServiceConnection/) Schnittstelle.  Dieses Objekt ist vom Client erstellt und übergeben Sie als Parameter an die [ `BindService` ](https://developer.xamarin.com/api/member/Android.Content.Context.BindService/) Methode. Die `BindService` steht auf jedem [ `Android.Content.Context` ](https://developer.xamarin.com/api/type/Android.Content.Context) Objekt (z. B. eine Aktivität). Es ist eine Anforderung an den Dienst starten, und binden einen Client, der Android-Betriebssystem. Es gibt drei Möglichkeiten, ein Client möglicherweise an einen Dienst binden die `BindService` Methode:

* **Ein Dienst Binder** &ndash; ein Binder Service ist eine Klasse, implementiert die [ `Android.OS.IBinder` ](https://developer.xamarin.com/api/type/Android.OS.IBinder) Schnittstelle. Die meisten Anwendungen implementieren diese Schnittstelle nicht direkt, erweitern sie stattdessen die [ `Android.OS.Binder` ](https://developer.xamarin.com/api/type/Android.OS.Binder) Klasse. Dies ist die am häufigsten verwendete Ansatz und eignet sich für, wenn der Dienst und der Client im selben Prozess vorhanden ist.
* **Mithilfe ein Messenger** &ndash; dieses Verfahren eignet sich für, wenn der Dienst in einem separaten Prozess vorhanden sein kann. Stattdessen Serviceanfragen gemarshallt werden, zwischen dem Client und Dienst über eine [ `Android.OS.Messenger` ](https://developer.xamarin.com/api/type/Android.OS.Messenger). Ein [ `Android.OS.Handler` ](https://developer.xamarin.com/api/type/Android.OS.Handler) wird erstellt, im Dienst behandelt die `Messenger` Anforderungen. Dies wird in einem anderen Handbuch behandelt werden.
* **Verwenden AIDL** &ndash; dies eintritt Terroraktivitäten im Zentrum der meisten Android-Entwickler und werden in diesem Handbuch nicht behandelt.

Sobald ein Client an einen Dienst gebunden wurde, wird die Kommunikation zwischen den beiden erfolgt über `Android.OS.IBinder` Objekt.  Dieses Objekt ist verantwortlich für die Schnittstelle, mit dem den Client mit dem Dienst interagieren kann. Es ist nicht erforderlich für jede Anwendung Xamarin.Android Implementierung dieser Schnittstelle von Grund auf neu, das Android SDK bietet die [ `Android.OS.Binder` ](https://developer.xamarin.com/api/type/Android.OS.Binder) Klasse die übernimmt. der größte Teil des Codes, mit das Objekt zwischen marshalling erforderlich der Client und dem Dienst.

Wenn ein Client mit dem Dienst erfolgt, es muss die Bindung aufheben daraus durch Aufrufen der `UnbindService` Methode. Nachdem der letzte Client von einem Dienst aufgehoben wurde, wird Android beenden und löschen Sie den Dienst gebunden.

Dieses Diagramm veranschaulicht, wie die Aktivität, Dienst-Verbindung, Binder und Dienst miteinander verknüpft:

![Ein Diagramm wie die Dienstkomponenten miteinander in Beziehung stehen](bound-services-images/bound-services-02.png "ein Diagramm wie die Dienstkomponenten miteinander in Beziehung stehen.")

Diese Anleitung wird beschrieben, wie Erweitern der `Service` Klasse zum Implementieren eines gebundenen Diensts. Es befasst sich auch implementieren `IServiceConnection` und Erweitern von `Binder` damit einen Client mit dem Dienst kommunizieren kann. Eine Beispiel-app begleitet dieses Handbuch, die eine Lösung mit einer einzelnen Xamarin.Android Projekt mit der Bezeichnung enthält **[BoundServiceDemo](https://github.com/xamarin/monodroid-samples/tree/master/ApplicationFundamentals/ServiceSamples/BoundServiceDemo)** . Dies ist eine sehr einfache Anwendung die veranschaulicht, wie einen Dienst implementiert und wie Sie eine Aktivität zu binden. Der gebundene-Dienst hat eine sehr einfache API mit nur eine Methode, `GetFormattedTimestamp`, womit eine Zeichenfolge, die dem Benutzer mitteilt, wenn der Dienst gestartet wurde und wie lange er ausgeführt wurde. Die app kann auch der Benutzer manuell Aufheben der Bindung und mit dem Dienst verbunden.

[![Screenshot der Anwendung auf einem Android-Telefon](bound-services-images/bound-services-03-sml.png)](bound-services-images/bound-services-03.png#lightbox)

## <a name="implementing-and-consuming-a-bound-service"></a>Implementieren und Verarbeiten einer gebundenen-Diensts

Es gibt drei Komponenten, die in der Reihenfolge für eine Android-Anwendung zum Verarbeiten eines gebundenen Diensts implementiert werden müssen:

1. **Erweitern der `Service` Klasse und Implementieren von Rückrufmethoden Lebenszyklus** &ndash; diese Klasse enthält den Code, der die Arbeit ausgeführt, die des Diensts angefordert werden. Dies wird im folgenden ausführlicher erläutert.
2. **Erstellen Sie eine Klasse, implementiert `IServiceConnection`**  &ndash; diese Schnittstelle bietet Rückrufmethoden werden aufgerufen, von Android auf den Client zu benachrichtigen, wenn die Verbindung mit dem Dienst geändert wurde, d. h. der Client verbunden oder getrennt hat das -Dienst. Die Dienst-Verbindung stellt auch einen Verweis auf ein Objekt bereit, die vom Client verwendet werden kann, um direkt mit dem Dienst interagieren. Dieser Verweis wird als bezeichnet den _Binder_.
3. **Erstellen Sie eine Klasse, implementiert `IBinder`**  &ndash; ein _Binder_ Implementierung stellt die API, die ein Client verwendet, um die Kommunikation mit dem Dienst bereit. Binder kann entweder bereitstellen einen Verweis auf die gebundene Dienst, sodass Methoden, die direkt aufgerufen werden, oder der Binder kann eine Client-API, die kapselt und blendet Sie aus den gebundenen Dienst aus der Anwendung bereitstellen. Ein `IBinder` müssen den erforderlichen Code für Remoteprozeduraufrufe bereitstellen. Es ist nicht erforderlich (oder empfohlen) zum Implementieren der `IBinder` -Schnittstelle direkt. Stattdessen sollten Anwendungen erweitern die `Binder` Typ, der Großteil die grundlegenden Funktionen bietet eine `IBinder`.
4. **Starten und Binden an einen Datendienst** &ndash; der Android-Anwendung ist verantwortlich für den Dienst zu starten und zu binden, nachdem der Dienst-Verbindung, der Binder und der Dienst erstellt wurden.

Jeder dieser Schritte wird in den folgenden Abschnitten im Detail besprochen.

### <a name="extend-the-service-class"></a>Erweitern der `Service` Klasse

Um einen Dienst mithilfe von Xamarin.Android zu erstellen, es ist notwendig, Unterklasse `Service` und hinzugefügt werden sollen die Klasse mit dem [ `ServiceAttribute` ](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute). Das Attribut wird von den Buildtools Xamarin.Android verwendet, um den Dienst ordnungsgemäß in der app zu registrieren **AndroidManifest.xml** Datei ähnlich wie eine Aktivität, ein Dienst gebundener hat seinem eigenen Lebenszyklus und Rückruf zugeordnete Methoden der wichtige Ereignisse in dessen Lebenszyklus. In der folgenden Liste ist ein Beispiel für einige der häufigeren Rückrufmethoden, die ein Dienst implementiert wird:

* `OnCreate` &ndash; Diese Methode wird von Android aufgerufen, wie sie den Dienst instanziiert wird. Es wird zum Initialisieren von Variablen oder Objekte, die während der Lebensdauer des Diensts erforderlich sind. Diese Methode ist optional.
* `OnBind` &ndash; Diese Methode muss von allen gebundenen Diensten implementiert werden. Wird aufgerufen, wenn der erste Client versucht, mit dem Dienst herzustellen. Es wird eine Instanz des zurückgeben `IBinder` , damit der Client mit dem Dienst interagieren kann. Solange der Dienst ausgeführt wird, die `IBinder` Objekt wird verwendet, um alle zukünftigen Clientanforderungen zum Binden an den Dienst zu erfüllen.
* `OnUnbind` &ndash; Diese Methode wird aufgerufen, wenn alle gebundenen Clients aufgehoben haben. Wird durch zurückgeben `true` von dieser Methode aufrufen des Diensts später `OnRebind` mit der Absicht an übergeben `OnUnbind` beim Binden neue Clients zu. Dies würden Sie tun, wenn ein Dienst ausgeführt wird weiterhin, nachdem sie aufgehoben wurde. Dies ist der Fall wäre der vor kurzem ungebundene Dienst auch einem gestarteten Dienst und `StopService` oder `StopSelf` hadn't aufgerufen wurde. In solch einem Szenario `OnRebind` die Absicht, die abgerufen werden können. Der Standardwert gibt `false` , dem wird keine Aktion ausgeführt. Dies ist optional.
* `OnDestroy` &ndash; Diese Methode wird aufgerufen, wenn Android des Diensts gelöscht wird. Bei dieser Methode sollten alle erforderlichen Cleanup aus, wie z. B. das Freigeben von Ressourcen, ausgeführt werden. Dies ist optional.

In diesem Diagramm werden die wichtigsten Lebenszyklusereignisse eines gebundenen Diensts angezeigt:

![Diagramm mit der Reihenfolge, in dem die Lebenszyklusmethoden heißen](bound-services-images/bound-services-01.png "Diagramm mit der Reihenfolge, in der Lebenszyklusmethoden aufgerufen werden.")

Der folgende Codeausschnitt aus der Begleit-Anwendung, die mit diesem Handbuch wird gezeigt, wie einen gebundenen Dienst in Xamarin.Android implementiert wird:

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

Im Beispiel wird die `OnCreate` -Methode initialisiert ein Objekt, enthält die Logik zum Abrufen und formatieren einen Zeitstempel, der von einem Client angefordert werden würde. Android wird aufgerufen, wenn der erste Client versucht, die an den Dienst gebunden, die `OnBind` Methode. Dieser Dienst instanziiert dann eine `TimestampServiceBinder` Objekt, mit dem die Clients auf diese Instanz des ausgeführten Diensts zugreifen kann. Die `TimestampServiceBinder` Klasse wird im nächsten Abschnitt erläutert.

### <a name="implementing-ibinder"></a>Implementieren von IBinder

Wie bereits erwähnt, ein `IBinder` Objekt stellt den Kommunikationskanal zwischen einem Client und einem Dienst. Android-Anwendungen sollten nicht implementieren, die `IBinder` -Schnittstelle, die [ `Android.OS.Binder` ](https://developer.xamarin.com/api/type/Android.OS.Binder/) erweitert werden soll. Die `Binder` Klasse bietet eine ähnliche die notwendige Infrastruktur die notwendigen Marshallen ist der Binder-Objekt an den Client vom Dienst (der in einem separaten Prozess ausgeführt werden kann). In den meisten Fällen die `Binder` -Unterklasse ist nur ein paar Codezeilen und einen Verweis auf den Dienst umschließt. In diesem Beispiel `TimestampBinder` verfügt über eine Eigenschaft, die verfügbar macht die `TimestampService` an den Client:

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

Dies `Binder` ermöglicht den öffentlichen Methoden auf den Dienst aufrufen, zum Beispiel:

```csharp
string currentTimestamp = serviceConnection.Binder.Service.GetFormattedTimestamp()
```

Einmal `Binder` wurde erweitert, ist es notwendig, implementieren `IServiceConnection` alles miteinander zu verbinden.

### <a name="creating-the-service-connection"></a>Erstellen die Dienst-Verbindung

Die `IServiceConnection` bietet | einführen | verfügbar zu machen | Verbinden der `Binder` Objekt an den Client. Zusätzlich zur Implementierung der `IServiceConnection` -Schnittstelle, die die Klasse erweitern muss `Java.Lang.Object`. Die Dienst-Verbindung muss außerdem eine Möglichkeit, die den Client zugänglichen enthalten die `Binder` (und daher mit der gebundenen Dienst kommunizieren).

Dieser Code stammt aus dem zugehörigen Beispielprojekt ist eine Möglichkeit zum Implementieren `IServiceConnection`:

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

Als Teil des Bindungsvorgangs Android aufrufen wird die `OnServiceConnected` -Methode, Bereitstellen der `name` des Diensts, der gebunden wird und die `binder` , enthält einen Verweis auf den Dienst selbst. In diesem Beispiel hat die Dienst-Verbindung für zwei Eigenschaften, die einen Verweis auf den Binder und ein boolesches Flag für enthält, wenn der Client mit dem Dienst verbunden ist.

Die `OnServiceDisconnected` Methode wird nur aufgerufen, wenn die Verbindung zwischen einem Client und einem Dienst unerwartet verloren geht oder beschädigt ist. Diese Methode ermöglicht dem Client die Möglichkeit, die auf die dienstunterbrechung zu reagieren.  

## <a name="starting-and-binding-to-a-service-with-an-explicit-intent"></a>Starten und Binden an einen Dienst mit der expliziten Priorität

Um einen Dienst gebundene verwenden zu können, muss ein Client (z. B. eine Aktivität) ein Objekt, das implementiert instanziieren `Android.Content.IServiceConnection` und rufen die `BindService` Methode.` BindService` Gibt zurück, `true` , wenn der Dienst gebunden ist `false` wird jedoch nicht. Die Methode `BindService` akzeptiert drei Parameter:

* **Ein `Intent`**  &ndash; die Absicht sollte explizit welcher Dienst für die Verbindung identifiziert.
* **Ein `IServiceConnection` Objekt** &ndash; dieses Objekt ist Zwischenelement, das stellt Rückrufmethoden, um den Client zu benachrichtigen, wenn der gebundene-Dienst gestartet oder beendet wird.
* **[`Android.Content.Bind`](https://developer.xamarin.com/api/type/Android.Content.Bind/) Enum** &ndash; dieser Parameter ist ein Satz von Flags werden vom System verwendet, wenn das Objekt zu binden. Ist der am häufigsten verwendete Wert [ `Bind.AutoCreate` ](https://developer.xamarin.com/api/field/Android.Content.Bind.AutoCreate/), dem wird den Dienst automatisch gestartet, wenn er nicht bereits ausgeführt wird.

Der folgende Codeausschnitt ist ein Beispiel zum Starten eines gebundenen Diensts in einer Aktivität mithilfe von expliziten Priorität:

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
> Beginnend mit Android 5.0.x (API-Ebene 21) Es ist nur möglich, binden an einen Dienst mit der expliziten Priorität.

## <a name="architectural-notes-about-the-service-connection-and-the-binder"></a>Architektur Hinweise zum Herstellen einer Verbindung des und den Binder.

Einige OOP-Puristen möglicherweise von der früheren Implementierungen von Projektmanagern die `TimestampBinder` -Klasse als es einen Verstoß gegen die [Gesetz von Demeter](https://en.wikipedia.org/wiki/Law_of_Demeter) , die in der einfachsten Form Status "nicht fremden; wenden Sie sich nur wenden Sie sich an Ihren Freunden". In dieser Implementierung macht die konkrete `TimestampService` Klasse für alle Clients.

Es ist streng genommen ist nicht erforderlich, für den Client kennen die `TimestampService` und Verfügbarmachen dieses konkrete Klasse für Clients kann eine Anwendung jedoch fehleranfällig und schwieriger zu über dessen Lebensdauer zu verwalten. Eine alternative Methode besteht darin, eine Schnittstelle verwenden, die verfügbar macht, ist die `GetFormattedTimestamp()` -Methode und Proxy-Aufrufe an den Dienst über die `Binder` (oder eine mögliche Service Connection-Klasse):  

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

In diesem Beispiel kann von einer Aktivität zum Aufrufen von Methoden für den Dienst selbst:

```csharp
// In this example the Activity is only talking to a friend, i.e. the IGetTimestamp interface provided by the Binder.
string currentTimestamp = serviceConnection.Binder.GetFormattedTimestamp()
```


## <a name="related-links"></a>Verwandte Links

- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service/)
- [Android.Content.Bind](https://developer.xamarin.com/api/type/Android.Content.Bind/)
- [Android.Content.Context](https://developer.xamarin.com/api/type/Android.Content.Context/)
- [Android.Content.IServiceConnection](https://developer.xamarin.com/api/type/Android.Content.IServiceConnection/)
- [Android.OS.Binder](https://developer.xamarin.com/api/type/Android.OS.Binder/)
- [Android.OS.IBinder](https://developer.xamarin.com/api/type/Android.OS.IBinder)
- [BoundServiceDemo (Beispiel)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/BoundServiceDemo/)
