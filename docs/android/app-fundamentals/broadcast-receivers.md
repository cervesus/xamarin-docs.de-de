---
title: Broadcast Receiver in Xamarin.Android
description: In diesem Abschnitt wird erläutert, wie ein Broadcast Receivers verwendet wird.
ms.prod: xamarin
ms.assetid: B2727160-12F2-43EE-84B5-0B15C8FCF4BD
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/20/2018
ms.openlocfilehash: 51bd3dd4c27dce19344f7660c31a0d4e741e1ad4
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121138"
---
# <a name="broadcast-receivers-in-xamarinandroid"></a>Broadcast Receiver in Xamarin.Android

_In diesem Abschnitt wird erläutert, wie ein Broadcast Receivers verwendet wird._

## <a name="broadcast-receiver-overview"></a>Übersicht über die Übertragungsempfänger

Ein _übertragungsempfänger_ ist eine Android-Komponente, die eine Anwendung auf Meldungen reagieren kann (Android [ `Intent` ](https://developer.xamarin.com/api/type/Android.Content.Intent/)), die von der Android-Betriebssystem oder durch eine Anwendung übertragen werden. Broadcasts führen Sie eine _Veröffentlichen / Abonnieren-_ Modell &ndash; ein Ereignis bewirkt, dass einen Broadcast veröffentlichen und Empfangen von diesen Komponenten, die das Ereignis interessiert sind.

Android gibt zwei Arten von Broadcasts:

* **Explizite Broadcast** &ndash; diese Arten von Broadcasts als Ziel eine bestimmte Anwendung. Die häufigste Verwendung von einer expliziten Übertragung wird eine Aktivität gestartet. Ein Beispiel für eine explizite übertragen, wenn eine app benötigt, um eine Telefonnummer zu wählen; Es wird ein Intent-Objekt, dessen Ziel die Phone-app unter Android und Bahn entlang der Telefonnummer zu wählende zu senden. Android leitet dann die Absicht an der Phone-app weiter.
* **Implizite Broadcast** &ndash; Broadcasts werden für alle apps auf dem Gerät verteilt. Ein Beispiel für eine implizite Broadcastnachricht ist die `ACTION_POWER_CONNECTED` Absicht. Diese Absicht wird jedes Mal veröffentlicht, die Android erkennt, dass der Akku auf dem Gerät geladen wird. Android leitet diese Absicht für alle apps, die für dieses Ereignis registriert haben.

Übertragungsempfänger ist eine Unterklasse von der `BroadcastReceiver` Typ, und es müssen überschreiben die [ `OnReceive` ](https://developer.xamarin.com/api/member/Android.Content.BroadcastReceiver.OnReceive/p/Android.Content.Context/Android.Content.Intent/) Methode. Android führt `OnReceive` im Hauptthread, also diese Methode sollte entworfen werden schnell ausgeführt. Vorsicht geboten, wenn das Erzeugen von Threads im `OnReceive` da Android den Prozess beenden kann, wenn die Methode abgeschlossen ist. Wenn ein broadcast Receiver muss lang ausgeführte Arbeit ausführen, es wird empfohlen, Planen Sie eine _Auftrag_ mithilfe der `JobScheduler` oder _Firebase Auftrag Verteiler_. Planung von Arbeiten mit einem Auftrag wird in ein separates Handbuch besprochen.

Ein _Zielfilter_ wird verwendet, um ein broadcast Receiver registrieren, sodass Nachrichten von Android ordnungsgemäß weiterleiten kann. Der Zielfilter kann zur Laufzeit angegeben werden (Dies wird manchmal als bezeichnet ein _Kontext registrierten Empfänger_ oder als _dynamische Registrierung_), oder er kann statisch im Android-Manifest (eine definiertwerden_Manifest registrierten Empfänger_). Xamarin.Android bietet eine C# Attribut `IntentFilterAttribute`, registrieren, die statisch Zielfilter (Dies wird werden ausführlicher in diesem Handbuch). Ab Android 8.0, ist es nicht möglich, eine Anwendung für eine implizite Broadcastnachricht statisch zu registrieren.

Der Hauptunterschied zwischen der Manifest-registrierten Empfänger und der Empfänger Kontext registriert ist, dass ein Empfänger Kontext registriert nur Broadcasts reagiert, während eine Anwendung ausgeführt wird, während ein Manifest registrierten Empfänger reagieren kann, überträgt, auch wenn die app nicht ausgeführt werden kann.  

Es gibt zwei Sätze an APIs zum Verwalten von ein broadcast Receiver und Senden von Broadcasts:

1. **`Context`** &ndash; Die `Android.Content.Context` Klasse kann verwendet werden, um ein broadcast Receiver zu registrieren, die auf eine systemweite Ereignisse reagiert. Die `Context` wird auch verwendet, um eine systemweite Broadcasts zu veröffentlichen.
2. **[`LocalBroadcastManager`](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))** &ndash; Dies ist eine API, die über die [NuGet-Paket für Xamarin-Support-Bibliothek v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/). Diese Klasse wird verwendet, Übertragungen und übertragungsempfänger isoliert, die im Kontext der Anwendung, die sie verwendet wird. Diese Klasse kann nützlich für verhindert, dass andere Anwendungen reagieren auf nur-Anwendung-Übertragungen oder Senden von Nachrichten an die private Empfänger sein.

Dialogfelder möglicherweise wird ein broadcast Receiver nicht angezeigt, und es wird dringend davon abgeraten, um eine Aktivität aus, in ein broadcast Receiver zu starten. Wenn ein übertragungsempfänger den Benutzer informieren muss, sollten sie eine Benachrichtigung veröffentlichen.

Es ist nicht möglich, zum Binden an, oder starten einen Dienst in ein broadcast Receiver. 

Dieses Handbuch befasst sich wie einen broadcast Receiver erstellen und registrieren, damit es Broadcasts empfangen kann.

## <a name="creating-a-broadcast-receiver"></a>Erstellen einen Broadcast Receiver

Um ein broadcast Receiver in Xamarin.Android zu erstellen, sollte eine Anwendung die Unterklasse der `BroadcastReceiver` Klasse, die mit Verzieren der `BroadcastReceiverAttribute`, und überschreiben die `OnReceive` Methode:

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

Wenn Xamarin.Android Klasse kompiliert wird, werden sie auch die AndroidManifest mit den erforderlichen Metadaten zum Registrieren des Empfängers aktualisiert. Für ein statisch registrierte broadcast Receiver der `Enabled` muss ordnungsgemäß festgelegt werden, um `true`, andernfalls Android wird möglicherweise nicht zum Erstellen einer Instanz des Empfängers.
 
Die `Exported` Eigenschaft steuert, ob der übertragungsempfänger Nachrichten von außerhalb der Anwendung empfangen kann. Wenn die Eigenschaft nicht explizit festgelegt ist, wird der Standardwert der Eigenschaft bestimmt, von Android, die basierend auf, wenn Absicht-Filter übertragungsempfänger zugeordnet. Wenn mindestens ein Intent-Filter für die übertragungsempfänger Android geht davon aus, die die `Exported` Eigenschaft `true`. Wenn keine Absicht-Filter übertragungsempfänger zugeordnet, Android geht davon aus, dass der Wert `false`. 

Die `OnReceive` -Methode empfängt einen Verweis auf die `Intent` , die an übertragungsempfänger gesendet wurde. Dies ermöglicht ist möglich, dass der Absender der Absicht, die Werte an die übertragungsempfänger übergeben.

### <a name="statically-registering-a-broadcast-receiver-with-an-intent-filter"></a>Registrieren einen Broadcast Receivers statisch mit einem Filter für beabsichtigte Lesevorgänge

Wenn eine `BroadcastReceiver` ergänzt wird, mit der [ `IntentFilterAttribute` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/), Xamarin.Android fügen die erforderlichen `<intent-filter>` Element für die Android manifest zum Zeitpunkt der Kompilierung. Der folgende Ausschnitt ist ein Beispiel für ein broadcast Receiver, der ausgeführt werden, wenn ein Gerät abgeschlossen ist, starten (wenn der Benutzer die erforderlichen Android-Berechtigungen erteilt wurden):

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

Es ist auch möglich, einen intent Filter zu erstellen, der auf benutzerdefinierten Intents reagieren. Betrachten Sie das folgende Beispiel: 

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

Apps für Android 8.0 (API-Ebene 26) oder höher kann nicht statisch registrieren für eine implizite Übertragung. Apps können für eine explizite Broadcastnachricht weiter statisch registrieren. Es ist eine kleine Liste implizite Broadcasts, die von dieser Einschränkung ausgenommen sind. Diese Ausnahmen werden weiter in die [implizite Broadcast Ausnahmen](https://developer.android.com/guide/components/broadcast-exceptions.html) Leitfaden in der Android-Dokumentation. Apps, die implizite Broadcasts interessieren müssen dynamisch mithilfe der `RegisterReceiver` Methode. Dies wird nachfolgend beschrieben.

### <a name="context-registering-a-broadcast-receiver"></a>Kontext Registrierung-einen Broadcast Receiver

Kontext-Registrierung (auch bezeichnet als dynamische Registrierung) von einem Empfänger erfolgt durch Aufruf der `RegisterReceiver` -Methode, und der broadcast Receiver muss aufgehoben werden, durch einen Aufruf der `UnregisterReceiver` Methode. Zum Verlust von Ressourcen verhindert, ist es wichtig, den Empfänger aufgehoben werden, wenn es nicht mehr für den Kontext (der Aktivität oder Dienst) relevant sind. Beispielsweise kann ein Dienst zu senden Zweck, eine Aktivität zu informieren, die Updates, die dem Benutzer anzuzeigende verfügbar sind. Wenn die Aktivität gestartet wird, würde es für die Intents registrieren. Wenn die Aktivität wird in den Hintergrund verschoben und für den Benutzer nicht mehr sichtbar, es sollte Aufheben der Registrierung des Empfängers, da die Benutzeroberfläche zum Anzeigen von Updates nicht mehr sichtbar ist. Der folgende Codeausschnitt ist ein Beispiel für das Registrieren und Aufheben der Registrierung eines broadcast Receivers im Kontext einer Aktivität:

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

Im vorherigen Beispiel, wenn die Aktivität in den Vordergrund geholt geht wird registriert einen broadcast Receiver, die eine benutzerdefinierte Absicht mit überwacht die `OnResume` Lebenszyklus-Methode. Wie die Aktivität in den Hintergrund, verschiebt der `OnPause()` Methode wird den Empfänger nicht aufheben.

## <a name="publishing-a-broadcast"></a>Veröffentlichen Sie eine Übertragung

Eine Übertragung aller auf dem Gerät ein Intent-Objekt erstellen und verteilen es mit installierten apps veröffentlicht werden die `SendBroadcast` oder `SendOrderedBroadcast` Methode.  

1. **Context.SendBroadcast Methoden** &ndash; es gibt mehrere Implementierungen dieser Methode.
   Diese Methoden, die das Ziel für das gesamte System übertragen werden. Die übertragungsempfänger, die das Ziel in einer unbestimmten Reihenfolge empfängt. Dies bietet ein hohes Maß an Flexibilität, aber bedeutet, dass es möglich, dass andere Anwendungen registrieren und die Absicht zu erhalten. Dies kann ein potenzielles Sicherheitsrisiko darstellen. Anwendungen müssen möglicherweise so implementieren Sie Sicherheit hinzufügen, um nicht autorisierte Zugriffe zu verhindern. Eine mögliche Lösung ist die Verwendung der `LocalBroadcastManager` die werden nur Nachrichten innerhalb des privaten Adressraums der app zu senden. Dieser Codeausschnitt ist ein Beispiel dafür, wie ein Intent-Objekt mit einer der verteilt die `SendBroadcast` Methoden:

   ```csharp
   Intent message = new Intent("com.xamarin.example.TEST");
   // If desired, pass some values to the broadcast receiver.
   message.PutExtra("key", "value");
   SendBroadcast(message);
   ```

    Dieser Codeausschnitt ist ein weiteres Beispiel für ein Broadcast gesendet, mit der `Intent.SetAction` Methode, um die Aktion zu ermitteln:

    ```csharp
    Intent intent = new Intent();
    intent.SetAction("com.xamarin.example.TEST");
    intent.PutExtra("key", "value");
    SendBroadcast(intent);
    ```

2. **Context.SendOrderedBroadcast** &ndash; Dies ist die Methode ist sehr ähnlich `Context.SendBroadcast`, mit dem Unterschied, dass die Absicht veröffentlichten zum Zeitpunkt, an die Empfänger, in der Reihenfolge werden, dass die Empfänger registriert wurden.

### <a name="localbroadcastmanager"></a>LocalBroadcastManager

Die [Xamarin-Support-Bibliothek v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) stellt eine Hilfsklasse namens [ `LocalBroadcastManager` ](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html). Die `LocalBroadcastManager` richtet sich an für apps, die nicht zum Senden oder Übertragungen von anderen apps auf dem Gerät empfangen soll. Die `LocalBroadcastManager` veröffentlichen Nachrichten innerhalb des Kontexts der Anwendung und gilt nur für diese übertragungsempfänger, die bei registriert werden nur die `LocalBroadcastManager`. Dieser Codeausschnitt ist ein Beispiel für ein broadcast Receiver mit Registrierung `LocalBroadcastManager`:

```csharp
Android.Support.V4.Content.LocalBroadcastManager.GetInstance(this). RegisterReceiver(receiver, new IntentFilter("com.xamarin.example.TEST"));
```

Andere apps auf dem Gerät keine der Nachrichten, die mit veröffentlicht werden empfangen die `LocalBroadcastManager`. Mit diesem Codeausschnitt wird gezeigt, wie eine beabsichtigte mit verteilt die `LocalBroadcastManager`:

```csharp
Intent message = new Intent("com.xamarin.example.TEST");
// If desired, pass some values to the broadcast receiver.
message.PutExtra("key", "value");
Android.Support.V4.Content.LocalBroadcastManager.GetInstance(this).SendBroadcast(message);
```

## <a name="related-links"></a>Verwandte Links

- [Broadcastreceivers API](https://developer.xamarin.com/api/type/Android.Content.BroadcastReceiver/)
- [Context.RegisterReceiver API](https://developer.xamarin.com/api/member/Android.Content.Context.RegisterReceiver/p/Android.Content.BroadcastReceiver/Android.Content.IntentFilter/System.String/Android.OS.Handler/)
- [Context.SendBroadcast API](https://developer.xamarin.com/api/member/Android.Content.Context.SendBroadcast/p/Android.Content.Intent/)
- [Context.UnregisterReceiver API](https://developer.xamarin.com/api/member/Android.Content.Context.UnregisterReceiver/p/Android.Content.BroadcastReceiver/)
- [Intent-API](https://developer.xamarin.com/api/type/Android.Content.Intent/)
- [IntentFilter API](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)
- [LocalBroadcastManager (Android-Dokumentation)](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))
- [Lokale Benachrichtigungen unter Android](~/android/app-fundamentals/notifications/local-notifications.md) Handbuch
- [Android-Unterstützung Bibliothek v4 (NuGet)](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
