---
title: Gebundene Dienste in Xamarin.Android
description: Gebundene Dienste sind Android-Dienste, die eine Client / Server-Schnittstelle bereitstellen, mit der ein Client (z. B. ein Android-Aktivität) interagieren kann. Dieses Handbuch werden die wichtigsten Komponenten bei der Erstellung einer gebundenen Dienst und für die Verwendung in einer Xamarin.Android-Anwendung erläutert.
ms.prod: xamarin
ms.assetid: 809ECE88-EF08-4E9A-B389-A2DC08C51A6E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/04/2018
ms.openlocfilehash: c0adee0dae1135bdfd076082e85a471db1cd1ecf
ms.sourcegitcommit: 849bf6d1c67df943482ebf3c80c456a48eda1e21
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2018
ms.locfileid: "51528558"
---
# <a name="bound-services-in-xamarinandroid"></a>Gebundene Dienste in Xamarin.Android

_Gebundene Dienste sind Android-Dienste, die eine Client / Server-Schnittstelle bereitstellen, mit der ein Client (z. B. ein Android-Aktivität) interagieren kann. Dieses Handbuch werden die wichtigsten Komponenten bei der Erstellung einer gebundenen Dienst und für die Verwendung in einer Xamarin.Android-Anwendung erläutert._

## <a name="bound-services-overview"></a>Übersicht über die gebundene Dienste

Dienste, die eine Client / Server-Schnittstelle für Clients, die direkte Interaktion mit dem Dienst bereitstellen, werden als bezeichnet _gebundene Dienste_.  Es können mehrere Clients, die auf eine einzelne Instanz eines Diensts gleichzeitig verbunden sein. Der gebundene Dienst und Client sind voneinander isoliert. Stattdessen bietet Android eine Reihe von Zwischenobjekten, die den Zustand der Verbindung zwischen den beiden zu verwalten. Dieser Status wird durch ein Objekt, das implementiert verwaltet die [ `Android.Content.IServiceConnection` ](https://developer.xamarin.com/api/type/Android.Content.IServiceConnection/) Schnittstelle.  Dieses Objekt ist vom Client erstellt und übergeben Sie als Parameter an die [ `BindService` ](https://developer.xamarin.com/api/member/Android.Content.Context.BindService/) Methode. Die `BindService` steht auf jedem [ `Android.Content.Context` ](https://developer.xamarin.com/api/type/Android.Content.Context) Objekt (z. B. eine Aktivität). Es ist eine Anforderung an den Dienst starten, und binden einen Client, der Android-Betriebssystem. Es gibt drei Möglichkeiten, einem Client können eine Bindung an einen Dienst der `BindService` Methode:

* **Ein Dienst Binder** &ndash; ein Binder Service ist eine Klasse, die implementiert die [ `Android.OS.IBinder` ](https://developer.xamarin.com/api/type/Android.OS.IBinder) Schnittstelle. Die meisten Anwendungen implementieren diese Schnittstelle nicht direkt, erweitern sie stattdessen die [ `Android.OS.Binder` ](https://developer.xamarin.com/api/type/Android.OS.Binder) Klasse. Dies ist der am häufigsten verwendete Ansatz und eignet sich für, wenn der Dienst und der Client im selben Prozess vorhanden ist.
* **Verwenden eine Messenger** &ndash; dieses Verfahren eignet sich für die bei der Dienst in einem separaten Prozess vorhanden sein kann. Stattdessen dienstanforderungen gemarshallt werden, zwischen dem Client und Dienst über eine [ `Android.OS.Messenger` ](https://developer.xamarin.com/api/type/Android.OS.Messenger). Ein [ `Android.OS.Handler` ](https://developer.xamarin.com/api/type/Android.OS.Handler) wird erstellt, im Dienst übernimmt die `Messenger` Anforderungen. Dies wird in einem anderen Handbuch erläutert.
* **Mithilfe der Android Interface Definition Language (AIDL)** &ndash; [AIDL](https://developer.android.com/guide/components/aidl) ist ein erweitertes Verfahren, die in diesem Handbuch nicht behandelt werden.

Sobald ein Client an einen Dienst gebunden wurde, wird die Kommunikation zwischen den beiden erfolgt über `Android.OS.IBinder` Objekt.  Dieses Objekt ist verantwortlich für die Schnittstelle, mit dem den Client mit dem Dienst interagieren kann. Es ist nicht erforderlich, für jede Xamarin.Android-Anwendung von Grund auf neu implementieren, das Android SDK bietet die [ `Android.OS.Binder` ](https://developer.xamarin.com/api/type/Android.OS.Binder) -Klasse die kümmert sich der Großteil des Codes, der das Objekt zwischen marshalling erforderlich der Client und dem Dienst.

Wenn ein Client mit dem Dienst abgeschlossen ist, sie müssen die Bindung aufheben von ihm durch Aufrufen der `UnbindService` Methode. Nachdem der letzte Client von einem Dienst aufgehoben wurde, wird die Android stoppen und Verwerfen der gebundene Dienst.

Dieses Diagramm veranschaulicht, wie die Aktivität, Dienst-Verbindung, Binder und Dienst miteinander verknüpft:

![Das Diagramm zeigt, wie die Dienstkomponenten miteinander in Beziehung stehen](bound-services-images/bound-services-02.png "das Diagramm zeigt, wie die Dienstkomponenten miteinander in Beziehung stehen.")

Dieser Anleitung erfahren Sie, wie zum Erweitern der `Service` Klasse, um ein gebundener Dienst implementiert wird. Es werden auch behandelt, implementieren `IServiceConnection` und Erweitern von `Binder` damit einen Client mit dem Dienst kommunizieren kann. Eine Beispiel-app begleitet diesem Leitfaden enthalten eine Lösung mit einem einzelnen Xamarin.Android-Projekt namens **[BoundServiceDemo](https://github.com/xamarin/monodroid-samples/tree/master/ApplicationFundamentals/ServiceSamples/BoundServiceDemo)** . Dies ist eine sehr einfache Anwendung, die veranschaulicht, wie ein Dienst implementiert wird und wie eine Aktivität an ihn binden. Der gebundene Dienst hat eine sehr einfache API mit nur eine Methode, `GetFormattedTimestamp`, womit eine Zeichenfolge, die dem Benutzer mitteilt, wenn der Dienst gestartet wurde und wie lange er ausgeführt wurde. Der app können auch den Benutzer manuell Aufheben der Bindung und Binden an den Dienst.

[![Screenshot der Anwendung auf einem Android-Telefon ausgeführt wird](bound-services-images/bound-services-03-sml.png)](bound-services-images/bound-services-03.png#lightbox)

## <a name="implementing-and-consuming-a-bound-service"></a>Implementieren und Verwenden eines gebundenen-Diensts

Es gibt drei Komponenten, die in der Reihenfolge für eine Android-Anwendung einen gebundenen Webdiensts implementiert werden müssen:

1. **Erweitern Sie die `Service` Klasse und implementieren Sie die Lebenszyklus-Rückrufmethoden** &ndash; diese Klasse enthält den Code, der die Arbeit ausgeführt werden, die von dem Dienst angefordert werden. Dies wird im folgenden ausführlicher behandelt.
2. **Erstellen Sie eine Klasse, implementiert `IServiceConnection`**  &ndash; diese Schnittstelle bietet Rückrufmethoden werden aufgerufen, von Android auf den Client zu benachrichtigen, wenn die Verbindung mit dem Dienst geändert wurde, d. h. der Client verbunden oder getrennt hat die -Dienst. Herstellen einer Verbindung des stellt auch einen Verweis auf ein Objekt bereit, mit denen der Client direkt mit dem Dienst interagieren. Dieser Verweis wird als bezeichnet die _Binder_.
3. **Erstellen Sie eine Klasse, implementiert `IBinder`**  &ndash; ein _Binder_ -Implementierung bietet die API, die ein Client für die Kommunikation mit dem Dienst verwendet. Der Binder kann entweder einen Verweis auf den gebundenen Dienst, Methoden, die direkt aufgerufen werden können oder der Binder kann eine Client-API, kapselt und blendet Sie aus den gebundenen Dienst aus der Anwendung, bereitstellen. Ein `IBinder` müssen den erforderlichen Code für Remoteprozeduraufrufe angeben. Es ist nicht erforderlich (oder empfohlen) zum Implementieren der `IBinder` -Schnittstelle direkt. Stattdessen sollten Anwendungen erweitern die `Binder` Typ bietet die grundlegenden Funktionen, die erforderlich sind, indem Sie die meisten ein `IBinder`.
4. **Starten und Binden an einen Datendienst** &ndash; die Android-Anwendung ist verantwortlich für den Dienst starten und zuzuordnen, nachdem der Dienst-Verbindung, Binder und Dienst erstellt wurden.

Jeder dieser Schritte wird in den folgenden Abschnitten ausführlicher erläutert.

### <a name="extend-the-service-class"></a>Erweitern Sie die `Service` Klasse

Zum Erstellen eines Diensts mithilfe von Xamarin.Android, es ist erforderlich, um eine Unterklasse `Service` und zum Verzieren von der Klasse mit dem [ `ServiceAttribute` ](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute). Das Attribut wird von den Xamarin.Android-Buildtools verwendet, um den Dienst ordnungsgemäß in der app zu registrieren **"androidmanifest.xml"** Datei ähnlich wie eine Aktivität, seinem eigenen Lebenszyklus und Rückruf zugeordnete Methoden über einen gebundener Dienst verfügt die wichtige Ereignisse des Lebenszyklus. In der folgende Liste ist ein Beispiel für einige der häufigeren Rückrufmethoden, die ein Dienst implementiert wird:

* `OnCreate` &ndash; Diese Methode wird von Android aufgerufen, wie sie den Dienst instanziiert wird. Es wird verwendet, um alle Variablen oder die Objekte, die vom Dienst, während dessen Lebensdauer erforderlich sind zu initialisieren. Diese Methode ist optional.
* `OnBind` &ndash; Diese Methode muss von allen gebundenen Diensten implementiert werden. Wird aufgerufen, wenn der erste Client versucht, mit dem Dienst herstellen. Es gibt eine Instanz von `IBinder` , damit der Client mit dem Dienst interagieren kann. Solange der Dienst ausgeführt wird, die `IBinder` Objekt wird verwendet, um alle zukünftigen Clientanforderungen für die Bindung an den Dienst zu erfüllen.
* `OnUnbind` &ndash; Diese Methode wird aufgerufen, wenn alle gebundenen Clients aufgehoben haben. Durch Rückgabe `true` von dieser Methode aufrufen des Diensts höher `OnRebind` mit der Absicht, die an `OnUnbind` neue Clients, wenn zu binden. Dies würden Sie tun, wenn ein Dienst ausgeführt wird weiterhin, nachdem er aufgehoben wurde. Dies geschieht, wenn der vor kurzem ungebundene Dienst auch einen Dienst gestartet wurden und `StopService` oder `StopSelf` noch nicht aufgerufen wurde. In solch einem Szenario `OnRebind` die Absicht, die abgerufen werden können. Der Standardwert gibt `false` , die nichts. Dies ist optional.
* `OnDestroy` &ndash; Diese Methode wird aufgerufen, wenn Android den Dienst zerstört wird. Eventuell Erforderlicher Bereinigungen, z.B. das Freigeben von Ressourcen sollte in dieser Methode durchgeführt werden. Dies ist optional.

Die Lebenszyklus-Ereignisse von einem gebundenen Dienst sind in diesem Diagramm dargestellt:

![Das Diagramm zeigt die Reihenfolge, in dem die Lebenszyklusmethoden aufgerufen werden](bound-services-images/bound-services-01.png "das Diagramm zeigt die Reihenfolge, in dem die Lebenszyklusmethoden aufgerufen werden.")

Der folgende Codeausschnitt aus der Begleit-Anwendung, die mit diesem Leitfaden zeigt, wie einen gebundenen Dienst in Xamarin.Android implementiert wird:

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

Im Beispiel wird die `OnCreate` Methode initialisiert ein Objekt, enthält die Logik zum Abrufen und formatieren einen Zeitstempel, die von einem Client angefordert werden sollen. Android wird aufgerufen, wenn der erste Client versucht, die an den Dienst zu binden, die `OnBind` Methode. Dieser Dienst instanziiert ein `TimestampBinder` -Objekt, das die Clients auf diese Instanz des ausgeführten Diensts zugreifen kann. Die `TimestampBinder` Klasse wird im nächsten Abschnitt erläutert.

### <a name="implementing-ibinder"></a>Implementieren von IBinder

Wie bereits erwähnt, ein `IBinder` Objekt stellt den Kommunikationskanal zwischen einem Client und einen Dienst bereit. Android-Anwendungen sollten nicht implementieren, die `IBinder` -Schnittstelle, die [ `Android.OS.Binder` ](https://developer.xamarin.com/api/type/Android.OS.Binder/) erweitert werden soll. Die `Binder` Klasse bietet im Wesentlichen die notwendige Infrastruktur die notwendigen Marshallen ist das Binder-Objekt an den Client aus dem Dienst (die in einem separaten Prozess ausgeführt werden kann). In den meisten Fällen die `Binder` Unterklasse ist nur ein paar Codezeilen und einen Verweis auf den Dienst umschließt. In diesem Beispiel `TimestampBinder` verfügt über eine Eigenschaft, die verfügbar macht die `TimestampService` an den Client:

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

Dies `Binder` ermöglicht das Aufrufen der öffentlichen Methoden, auf dem Dienst z. B.:

```csharp
string currentTimestamp = serviceConnection.Binder.Service.GetFormattedTimestamp()
```

Einmal `Binder` wurde erweitert, ist es erforderlich, implementieren `IServiceConnection` alles miteinander zu verbinden.

### <a name="creating-the-service-connection"></a>Erstellen die Dienst-Verbindung

Die `IServiceConnection` wird | Einführung | verfügbar zu machen | Verbinden der `Binder` Objekt an den Client. Zusätzlich zur Implementierung der `IServiceConnection` -Schnittstelle, die die Klasse erweitern muss `Java.Lang.Object`. Herstellen einer Verbindung des sollte auch eine Möglichkeit, die der Client zugreifen kann Bereitstellen der `Binder` (und daher mit dem gebundenen Dienst kommunizieren).

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

Als Teil des Bindungsvorgangs, Android Ruft die `OnServiceConnected` -Methode bereitstellen der `name` des Diensts, der gebunden wird und die `binder` , enthält einen Verweis auf den Dienst selbst. In diesem Beispiel hat die Dienst-Verbindung zwei Eigenschaften, die einen Verweis auf die Bindung und ein boolesches Flag für enthält, wenn der Client mit dem Dienst oder nicht verbunden ist.

Die `OnServiceDisconnected` Methode wird nur aufgerufen, wenn die Verbindung zwischen einem Client und einem Dienst unerwartet verloren geht oder beschädigt ist. Diese Methode ermöglicht dem Client die Möglichkeit, auf die dienstunterbrechung zu reagieren.  

## <a name="starting-and-binding-to-a-service-with-an-explicit-intent"></a>Starten und Binden an einen Datendienst mit expliziter Intent

Um einen gebundenen Dienst verwenden, muss einen Client (z. B. eine Aktivität) zu instanziieren ein Objekt, das implementiert `Android.Content.IServiceConnection` und Aufrufen der `BindService` Methode.` BindService` Gibt `true` , wenn der Dienst gebunden ist, `false` ist dies nicht. Die Methode `BindService` akzeptiert drei Parameter:

* **Ein `Intent`**  &ndash; die Absicht sollten welcher Dienst für die Verbindung explizit identifizieren.
* **Ein `IServiceConnection` Objekt** &ndash; dieses Objekt ist ein Vermittler, der stellt Rückrufmethoden bereit, um den Client zu benachrichtigen, wenn der gebundene Dienst gestartet oder beendet wird.
* **[`Android.Content.Bind`](https://developer.xamarin.com/api/type/Android.Content.Bind/) Enum** &ndash; dieser Parameter ist ein Satz von Flags wird vom System, um Wenn das Objekt zu binden. Der am häufigsten verwendete Wert ist [ `Bind.AutoCreate` ](https://developer.xamarin.com/api/field/Android.Content.Bind.AutoCreate/), dem wird den Dienst automatisch gestartet, wenn es nicht bereits ausgeführt wird.

Der folgende Codeausschnitt ist ein Beispiel für einen gebundenen Dienst in einer Aktivität mit Intent expliziter zu starten:

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
> Ab Android 5.0 (API Level 21) Es ist nur möglich, eine Bindung an einen Dienst mit Intent expliziter.

## <a name="architectural-notes-about-the-service-connection-and-the-binder"></a>Architektur zum Herstellen einer Verbindung des und den Binder Anmerkungen zu dieser.

Einige OOP-Puristen können der vorherigen Implementierung von Projektmanagern den `TimestampBinder` Klasse, da es sich um einen Verstoß gegen ist die [Gesetz von Demeter](https://en.wikipedia.org/wiki/Law_of_Demeter) , die in der einfachsten Form gibt "nicht mit fremden; sprechen nur mit Ihren Freunden kommunizieren Sie". In dieser Implementierung macht die konkreten `TimestampService` Klasse, um alle Clients.

Es ist genau genommen nicht erforderlich, für den Client, Informationen zu den `TimestampService` und konkrete Klasse an Clients bereitstellen kann eine Anwendung vornehmen, fehleranfällig und schwieriger zu über dessen Lebensdauer zu verwalten. Ein alternativer Ansatz ist die Verwendung eine Schnittstelle der verfügbar macht die `GetFormattedTimestamp()` -Methode und Proxy-Aufrufe an den Dienst über die `Binder` (oder möglich Service Connection-Klasse):  

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

Dieses Beispiel kann von einer Aktivität zum Aufrufen von Methoden für den Dienst selbst:

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
