---
title: "Broadcast Empfänger in Xamarin.Android"
description: "In diesem Abschnitt erläutert, wie ein Empfänger senden."
ms.topic: article
ms.prod: xamarin
ms.assetid: B2727160-12F2-43EE-84B5-0B15C8FCF4BD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 7ea280e11e4051b51da8c20eb9ac5a4e298547f3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="broadcast-receiver-in-xamarinandroid"></a>Broadcast Empfänger in Xamarin.Android

_In diesem Abschnitt erläutert, wie ein Empfänger senden._


## <a name="broadcast-receiver-overview"></a>Übersicht über die Empfänger senden

Ein _broadcast Empfänger_ ist eine Android-Komponente, die eine Anwendung auf Meldungen reagiert werden kann (ein Android [ `Intent` ](https://developer.xamarin.com/api/type/Android.Content.Intent/)), werden von Android-Betriebssystem oder von einer Anwendung übermittelt. Führen Sie die Broadcasts eine _Veröffentlichen / Abonnieren_ Modell &ndash; bewirkt, dass ein Ereignis einen Broadcast veröffentlicht und empfangen, indem Sie die Komponenten, die im Ereignis interessiert sind. 

Android gibt zwei Kategorien von Broadcasts:

* **Normale Übertragung** &ndash; ein normaler Broadcast an alle registrierten broadcast Empfänger in einer unbestimmten Reihenfolge weitergeleitet werden. Jeder Empfänger wird dem Zweck eine nicht definierte Reihenfolge angezeigt. 
* **Broadcast sortiert** &ndash; eine geordnete Übertragung wird jeweils einzeln an die registrierten Empfänger übermittelt. Beim Empfang der Absicht broadcast Empfänger kann die Absicht ändern oder die Übertragung beendet werden kann.

Der Broadcastpakete Empfänger ist eine Unterklasse von der `BroadcastReceiver` -Klasse, und es müssen überschreiben die [ `OnReceive` ](https://developer.xamarin.com/api/member/Android.Content.BroadcastReceiver.OnReceive/p/Android.Content.Context/Android.Content.Intent/) Methode. Android führt `OnReceive` auf den Hauptthread, also diese Methode sollte entworfen werden schnell ausgeführt werden. Vorsicht beim Erstellen von Threads im `OnReceive` da Android den Prozess beenden kann, wenn die Methode abgeschlossen ist. Wenn broadcast Empfänger durchzuführenden lang ausgeführte Arbeit wird empfohlen, zu planen einer _Auftrag_ mithilfe der `JobScheduler` oder _Firebase Auftrag wurde vom Verteiler_. Planen der Arbeit mit einem Auftrag wird in einer separaten Handbuch besprochen.

Ein _beabsichtigte Filter_ wird verwendet, um einen broadcast Empfänger zu registrieren, sodass Android ordnungsgemäß Nachrichten weiterleiten kann. Der beabsichtigte Filter kann zur Laufzeit angegeben werden (Dies wird manchmal als bezeichnet eine _Kontext registrierten Empfänger_ oder als _dynamische Registrierung_) oder können statisch in das Android-Manifest (eine definiert_Manifest registrierten Empfänger_). Xamarin.Android enthält ein C#-Attribut, `IntentFilterAttribute`, registrieren, die statisch den beabsichtigten Filter (Dies wird werden ausführlicher weiter unten in diesem Handbuch). 

Der Hauptunterschied zwischen der Empfänger Manifest registriert und der Empfänger Kontext registriert ist, dass ein Empfänger Kontext registriert nur auf Broadcasts Endbenutzerverhalten, während eine Anwendung ausgeführt wird, während ein Manifest registrierten Empfänger beantwortet werden kann, auch wenn die app nicht ausgeführt werden kann, sendet.  

Es gibt zwei Sätze von APIs zum Verwalten von broadcast Empfänger und Senden von Broadcasts:

1. **`Context`** &ndash; Die `Android.Content.Context` Klasse kann verwendet werden, um einen broadcast Empfänger zu registrieren, die systemweite Ereignisse reagiert. Die `Context` dient auch zum systemweite Broadcasts zu veröffentlichen.
2. **[`LocalBroadcastManager`](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))** &ndash; Dies ist eine API, die über die [Xamarin-Unterstützungsbibliothek v4-NuGet-Paket](https://www.nuget.org/packages/Xamarin.Android.Support.v4/). Diese Klasse wird verwendet, Broadcasts und Broadcastpakete im Kontext der Anwendung, die sie verwenden isolierte Empfänger gehalten werden. Diese Klasse kann zum Verhindern von anderen Anwendungen aus der Anwendung nur Broadcasts beantworten oder Senden von Nachrichten an privaten Empfänger nützlich sein.

Broadcast Empfänger u. u. nicht Dialogfelder angezeigt, und es wird dringend davon abgeraten, um eine Aktivität aus einem broadcast Empfänger zu starten. Wenn broadcast Empfänger den Benutzer informieren muss, sollten sie eine Benachrichtigung veröffentlichen.

Es ist nicht möglich, binden an oder starten einen Dienst in einen broadcast Empfänger. 

Dieses Handbuch befasst, wie auf einen Broadcast-Empfänger erstellt haben und ihn zu registrieren, damit es Broadcasts empfangen kann.

## <a name="creating-a-broadcast-receiver"></a>Erstellen einen Broadcast Empfänger

Um einen broadcast Empfänger in Xamarin.Android erstellt haben, sollte eine Anwendung Unterklasse der `BroadcastReceiver` Klasse, die hinzugefügt wurden, mit der `BroadcastReceiverAttribute`, und überschreiben die `OnReceive` Methode:

```csharp
[BroadcastReceiver(Enabled = true, Exported = false)]
public class SampleReceiver : BroadcastReceiver
{
    public override void OnReceive(Context context, Intent intent)
    {
        // Do stuff here.
        
        String value = intent.GetStringExtra("key");
    }
}
```

Wenn Xamarin.Android Klasse kompiliert wird, aktualisieren sie die AndroidManifest auch mit den erforderlichen Metadaten zum Registrieren des Empfängers. Für Broadcast-Empfänger statisch registriert die `Enabled` muss ordnungsgemäß festgelegt werden, um `true`, andernfalls Android ist nicht möglich, eine Instanz des Empfängers erstellen.
 
Die `Exported` Eigenschaft steuert, ob der Broadcastpakete Empfänger Nachrichten von außerhalb der Anwendung empfangen werden kann. Wenn die Eigenschaft nicht explizit festgelegt ist, wird der Standardwert der Eigenschaft bestimmt, von Android basierend darauf, ob die zugeordneten broadcast Empfängers Absicht-Filter vorhanden sind. Wenn mindestens eine beabsichtigte-Filter als broadcast Empfänger für Android wird vorausgesetzt, dass die `Exported` Eigenschaft ist `true`. Wenn keine Absicht-Filter der Broadcastpakete Empfänger zugeordnet, Android wird davon ausgegangen, dass der Wert `false`. 

Die `OnReceive` Methode empfängt einen Verweis auf die `Intent` , die an den broadcast Empfänger weitergeleitet wurde. Dies ermöglicht die ist möglich, dass der Absender der Absicht, übergeben von Werten an den Empfänger broadcastgrenzen zu erhalten.

### <a name="statically-registering-a-broadcast-receiver-with-an-intent-filter"></a>Registrieren von Broadcast Empfänger statisch mit einer beabsichtigten Filter

Wenn eine `BroadcastReceiver` ergänzt wird, mit der [ `IntentFilterAttribute` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/), Xamarin.Android fügen die erforderlichen `<intent-filter>` Element auf der Android manifest zum Zeitpunkt der Kompilierung. Der folgende Codeausschnitt zeigt ein Beispiel für einen broadcast Empfänger, der ausgeführt werden, wenn ein Gerät abgeschlossen ist, starten (sofern die erforderlichen Berechtigungen für die Android vom Benutzer erteilt wurden):

```csharp
[BroadcastReceiver(Enabled = true)]
[IntentFilter(new[] { Android.Content.Intent.ActionBootCompleted })]
public class MyBootReceiver : BroadcastReceiver
{
    public override void OnReceive(Context context, Intent intent)
    {
        // Work that should be done when the device boots.     
    }
}
```

Es ist auch möglich, einen beabsichtigten Filter zu erstellen, auf die benutzerdefinierte Prioritäten reagiert. Betrachten Sie das folgende Beispiel: 

```csharp
[BroadcastReceiver(Enabled = true)]
[IntentFilter(new[] { "com.xamarin.example.TEST" })]
public class MySampleBroadcastReceiver : BroadcastReceiver
{
    public override void OnReceive(Context context, Intent intent)
    {
        // Do stuff here
    }
}
```

### <a name="context-registering-a-broadcast-receiver"></a>Broadcast Empfänger Registrierung Kontext- 

Kontext-Registrierung, der einem Empfänger erfolgt durch Aufrufen der `RegisterReceiver` -Methode und der Broadcastpakete Empfänger muss aufgehoben werden, durch einen Aufruf der `UnregisterReceiver` Methode. Um auslaufenden Ressourcen zu verhindern, unbedingt sollte die Empfänger aufgehoben wird, wenn es nicht mehr im Kontext irrelevant ist. Ein Dienst kann z. B. ein versucht, eine Aktivität zu informieren, die Updates, die dem Benutzer angezeigt werden, verfügbar sind übermittelt. Wenn die Aktivität gestartet wird, würde es für diese Intents registrieren. Wenn die Aktivität in den Hintergrund verschoben wird, und für den Benutzer nicht mehr sichtbar ist, sollten Aufheben des Empfängers, da die Benutzeroberfläche zum Anzeigen von Updates nicht mehr sichtbar ist. Der folgende Codeausschnitt ist ein Beispiel zum Registrieren und Aufheben der Registrierung eines broadcast Empfängers im Kontext einer Aktivität:

```csharp
[Activity(Label = "MainActivity", MainLauncher = true, Icon = "@mipmap/icon")]
public class MainActivity: Activity 
{
    MySampleBroadcastReceiver receiver;
    
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);
        receiver = new MySampleBroadcastReceiver()
        
        // Code omitted for clarity
    }
    
    protected override OnResume() 
    {
        base.OnResume();
        RegisterReceiver(receiver, new IntentFilter("com.xamarin.example.TEST"));
        // Code omitted for clarity
    }
    
    protected override OnPause() 
    {
        UnregisterReceiver(receiver);
        // Code omitted for clarity
        base.OnPause();
    }
}
```

Im vorherigen Beispiel, bei der die Aktivität in den Vordergrund wird folglich broadcast Empfänger, die eine benutzerdefinierte Absicht mit überwacht die `OnResume` Lifecycle-Methode. Bewegen Sie die Aktivität in den Hintergrund der `OnPause()` Methode wird den Empfänger aufgehoben.

## <a name="publishing-a-broadcast"></a>Veröffentlichen einer Übertragung

Eine Übertragung wird veröffentlicht Kapseln einer _Aktion_ Priorität und verteilen diese mit einem der zwei APIs: 

1. **[`LocalBroadcastManager`](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))** &ndash; Intents, die mit veröffentlicht werden die `LocalBroadcastManager` wird nur von der Anwendung; empfangen wird nicht für andere Anwendungen weitergeleitet werden. Dies sollte bevorzugt, sein, es bietet eine zusätzliche Sicherheitsebene, bleiben die Absichten innerhalb der aktuellen Anwendung, und da alles, was im Prozess ist keine Mehrbedarf in Verbindung mit prozessübergreifende Aufrufe vorhanden ist. Diesem Codeausschnitt wird veranschaulicht, wie eine Aktivität eine beabsichtigte mit dispatch-möglicherweise die `LocalBroadcastManager`:

   ```csharp
   Intent message = new Intent("com.xamarin.example.TEST");
   // If desired, pass some values to the broadcast receiver.
   intent.PutExtra("key", "value");
   Android.Support.V4.Content.LocalBroadcastManager.GetInstance(this).SendBroadcast(message);
   ```

2. **[`Context.SendBroadcast`](https://developer.xamarin.com/api/member/Android.Content.Context.SendBroadcast/p/Android.Content.Intent/) Methoden** &ndash; es gibt mehrere Implementierungen dieser Methode.
   Diese Methoden werden die Absicht, die das gesamte System übermittelt. Dies bietet ein hohes Maß an Flexibilität jedoch bedeutet, dass anderen Anwendungen registriert werden können, um die Anwendung empfängt. Dies kann ein potenzielles Sicherheitsrisiko darstellen. Anwendungen müssen möglicherweise zum Implementieren von Sicherheit hinzufügen, um nicht autorisierten Zugriff auf die Absicht sicherzustellen. Dieser Codeausschnitt ist ein Beispiel dafür, wie mit einer der Priorität verteilt die `SendBroadcast` Methoden:

   ```csharp
   Intent message = new Intent("com.xamarin.example.TEST");
   // If desired, pass some values to the broadcast receiver.
   intent.PutExtra("key", "value");
   SendBroadcast(intent);
   ```
        
> [!NOTE]
> Die LocalBroadcastManager kann über die [Xamarin-Unterstützungsbibliothek v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) NuGet-Paket. 

Dieser Codeausschnitt ist ein weiteres Beispiel eine Übertragung senden, mithilfe der `Intent.SetAction` Methode, um die Aktion zu identifizieren:

```csharp 
Intent intent = new Intent();
intent.SetAction("com.xamarin.example.TEST");
intent.PutExtra("key", "value");
SendBroadcast(intent);
```



## <a name="related-links"></a>Verwandte Links

- [BroadcastReceiver](https://developer.xamarin.com/api/type/Android.Content.BroadcastReceiver/)
- [Context.RegisterReceiver](https://developer.xamarin.com/api/member/Android.Content.Context.RegisterReceiver/p/Android.Content.BroadcastReceiver/Android.Content.IntentFilter/System.String/Android.OS.Handler/)
- [Context.SendBroadcast](https://developer.xamarin.com/api/member/Android.Content.Context.SendBroadcast/p/Android.Content.Intent/)
- [Context.UnregisterReceiver](https://developer.xamarin.com/api/member/Android.Content.Context.UnregisterReceiver/p/Android.Content.BroadcastReceiver/)
- [Zweck](https://developer.xamarin.com/api/type/Android.Content.Intent/)
- [IntentFilter](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)
- [LocalBroadcastManager](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))
- [Lokale Benachrichtigungen in Android](~/android/app-fundamentals/notifications/local-notifications.md)
- [Android unterstützt Bibliothek v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
