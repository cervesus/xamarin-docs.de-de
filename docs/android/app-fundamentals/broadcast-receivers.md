---
title: Broadcast Empfänger in xamarin. Android
description: In diesem Abschnitt wird die Verwendung eines Broadcast Empfängers erläutert.
ms.prod: xamarin
ms.assetid: B2727160-12F2-43EE-84B5-0B15C8FCF4BD
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/20/2018
ms.openlocfilehash: 9266d8c4e1723adfb7e5e55dce7ede6d47f6f116
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70755480"
---
# <a name="broadcast-receivers-in-xamarinandroid"></a>Broadcast Empfänger in xamarin. Android

_In diesem Abschnitt wird die Verwendung eines Broadcast Empfängers erläutert._

## <a name="broadcast-receiver-overview"></a>Übersicht über Broadcast Empfänger

Ein _Broadcast Empfänger_ ist eine Android-Komponente, die es einer Anwendung ermöglicht, auf Nachrichten [`Intent`](xref:Android.Content.Intent)(Android) zu antworten, die vom Android-Betriebssystem oder von einer Anwendung übertragen werden. Übertragungen folgen einem _Veröffentlichen-Abonnieren-_ Modell &ndash; ein Ereignis bewirkt, dass eine Übertragung von den für das Ereignis relevanten Komponenten veröffentlicht und empfangen wird.

Android identifiziert zwei Arten von Übertragungen:

- **Expliziter Broadcast** &ndash; Diese Arten von Übertragungen Zielen auf eine bestimmte Anwendung ab. Eine explizite Übertragung wird am häufigsten verwendet, um eine Aktivität zu starten. Ein Beispiel für einen expliziten Broadcast, wenn eine APP eine Telefonnummer wählen muss. Er leitet eine Absicht weiter, die auf die Phone-app unter Android ausgerichtet ist, und übergibt die Telefonnummer, die gewählt werden soll. Android leitet dann die Absicht an die Phone-APP weiter.
- **Impliziter Broadcast** &ndash; Diese Übertragungen werden an alle apps auf dem Gerät gesendet. Ein Beispiel für eine implizite Übertragung ist `ACTION_POWER_CONNECTED` die Absicht. Diese Absicht wird jedes Mal veröffentlicht, wenn Android erkennt, dass der Akku auf dem Gerät berechnet wird. Android leitet diese Absicht an alle apps weiter, die für dieses Ereignis registriert wurden.

Der Broadcast Empfänger ist eine Unterklasse des `BroadcastReceiver` Typs und muss die [`OnReceive`](xref:Android.Content.BroadcastReceiver.OnReceive*) -Methode überschreiben. Android wird im `OnReceive` Haupt Thread ausgeführt. Daher sollte diese Methode für eine schnelle Ausführung entworfen werden. Beim Erzeugen von Threads in `OnReceive` sollten Sie sorgfältig vorgehen, da Android den Prozess beenden kann, wenn die-Methode abgeschlossen ist. Wenn ein Broadcast Empfänger eine lange laufende Arbeit ausführen muss, empfiehlt es sich, einen _Auftrag_ mithilfe `JobScheduler` von oder des _Firebase-Auftrags_Verteilers zu planen. Die Planung der Arbeit mit einem Auftrag wird in einem separaten Handbuch erläutert.

Ein beabsichtigter _Filter_ wird verwendet, um einen Broadcast Empfänger zu registrieren, sodass Android ordnungsgemäß Nachrichten weiterleiten kann. Der Intent-Filter kann zur Laufzeit festgelegt werden (Dies wird manchmal als _Kontext registrierter Empfänger_ oder als _dynamische Registrierung_bezeichnet), oder er kann statisch im Android-Manifest (ein _Manifest-registrierter Empfänger_) definiert werden. Xamarin. Android bietet ein C# -Attribut `IntentFilterAttribute`,, das den Intent-Filter statisch registriert (Dies wird später in diesem Handbuch ausführlicher erläutert). Ab Android 8,0 ist es nicht möglich, dass eine Anwendung für eine implizite Übertragung statisch registriert wird.

Der Hauptunterschied zwischen dem vom Manifest registrierten Empfänger und dem Kontexts registrierten Empfänger besteht darin, dass ein Kontext registrierter Empfänger nur auf Übertragungen antwortet, während eine Anwendung ausgeführt wird, während ein Manifest-registrierter Empfänger auf überträgt, auch wenn die APP möglicherweise nicht ausgeführt wird.  

Es gibt zwei Sätze von APIs zum Verwalten eines Broadcast Empfängers und Senden von Übertragungen:

1. **`Context`** Die-Klasse kann verwendet werden, um einen Broadcast Empfänger zu registrieren, der `Android.Content.Context` auf systemweite Ereignisse antwortet. &ndash; Der `Context` wird auch zum Veröffentlichen von systemweiten Übertragungen verwendet.
2. **[`LocalBroadcastManager`](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))** Dies ist eine API, die über das [xamarin-Unterstützungs Bibliothek V4-nuget-Paket](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)verfügbar ist. &ndash; Diese Klasse wird verwendet, um Übertragungen und Broadcast Empfänger im Kontext der Anwendung, die Sie verwendet, isoliert zu halten. Diese Klasse kann nützlich sein, um zu verhindern, dass andere Anwendungen auf nur Anwendungen reagieren, die nur Anwendungen übertragen, oder Nachrichten an private Empfänger senden.

Ein Broadcast Empfänger zeigt möglicherweise keine Dialoge an, und es wird dringend davon abgeraten, eine Aktivität innerhalb eines Broadcast Empfängers zu starten. Wenn ein Broadcast Empfänger den Benutzer benachrichtigen muss, sollte er eine Benachrichtigung veröffentlichen.

Es ist nicht möglich, einen Dienst innerhalb eines Broadcast Empfängers zu binden oder zu starten. 

In diesem Leitfaden wird beschrieben, wie ein Broadcast Empfänger erstellt und registriert wird, damit er Übertragungen empfangen kann.

## <a name="creating-a-broadcast-receiver"></a>Erstellen eines Broadcast Empfängers

Um einen Broadcast Empfänger in xamarin. Android zu erstellen, sollte eine Anwendung die `BroadcastReceiver` -Klasse Unterklassen, Sie mit dem `BroadcastReceiverAttribute`schmücken und die `OnReceive` -Methode überschreiben:

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

Wenn xamarin. Android die Klasse kompiliert, wird auch das androidmanifest mit den erforderlichen Metadaten aktualisiert, um den Empfänger zu registrieren. Bei einem statisch registrierten Broadcast Empfänger muss das `Enabled` ordnungsgemäß auf `true`festgelegt werden. andernfalls kann Android keine Instanz des Empfängers erstellen.

Die `Exported` -Eigenschaft steuert, ob der Broadcast Empfänger Nachrichten von außerhalb der Anwendung empfangen kann. Wenn die Eigenschaft nicht explizit festgelegt wird, wird der Standardwert der Eigenschaft von Android bestimmt, basierend auf, wenn dem Broadcast Empfänger beabsichtigte Filter zugeordnet sind. Wenn mindestens ein Intent-Filter für den Broadcast Empfänger vorhanden ist, geht `Exported` `true`Android davon aus, dass die-Eigenschaft den Wert hat. Wenn dem Broadcast Empfänger keine Intent-Filter zugeordnet sind, wird von Android angenommen, dass der Wert ist `false`. 

Die `OnReceive` -Methode empfängt einen Verweis auf `Intent` den, der an den Broadcast Empfänger gesendet wurde. Dies ermöglicht es dem Absender der Absicht, Werte an den Broadcast Empfänger zu übergeben.

### <a name="statically-registering-a-broadcast-receiver-with-an-intent-filter"></a>Statischer Registrierung eines Broadcast Empfängers mit einem Intent-Filter

Wenn ein `BroadcastReceiver` mit dem [`IntentFilterAttribute`](xref:Android.App.IntentFilterAttribute)versehen wird, fügt xamarin. Android dem Android- `<intent-filter>` Manifest zum Zeitpunkt der Kompilierung das erforderliche-Element hinzu. Der folgende Code Ausschnitt ist ein Beispiel für einen Broadcast Empfänger, der ausgeführt wird, wenn das Starten eines Geräts abgeschlossen ist (sofern die entsprechenden Android-Berechtigungen vom Benutzer erteilt wurden):

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

Es ist auch möglich, einen beabsichtigten Filter zu erstellen, der auf benutzerdefinierte Intents antwortet. Betrachten Sie das folgende Beispiel: 

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

Apps, die auf Android 8,0 (API-Ebene 26) oder höher ausgerichtet sind, können für eine implizite Übertragung nicht statisch registriert werden. Apps können sich für einen expliziten Broadcast weiterhin statisch registrieren. Es gibt eine kleine Liste impliziter Übertragungen, die von dieser Einschränkung ausgenommen sind. Diese Ausnahmen werden im Leitfaden für [implizite Broadcast Ausnahmen](https://developer.android.com/guide/components/broadcast-exceptions.html) in der Android-Dokumentation beschrieben. Apps, die an impliziten Übertragungen interessiert sind, müssen dies dynamisch `RegisterReceiver` mithilfe der-Methode durchführen. Dies wird im nächsten Schritt beschrieben.

### <a name="context-registering-a-broadcast-receiver"></a>Kontext-Registrieren eines Broadcast Empfängers

Die Kontext Registrierung (auch als dynamische Registrierung bezeichnet) eines Empfängers wird durch Aufrufen der `RegisterReceiver` -Methode durchgeführt, und der Broadcast Empfänger muss bei einem Aufruf der `UnregisterReceiver` -Methode die Registrierung aufheben. Um das Verlust von Ressourcen zu verhindern, ist es wichtig, die Registrierung des Empfängers aufzuheben, wenn er für den Kontext (die Aktivität oder den Dienst) nicht mehr relevant ist. Ein Dienst kann z. b. eine Absicht übermitteln, eine Aktivität zu informieren, dass Updates für den Benutzer verfügbar sind. Wenn die Aktivität gestartet wird, würde Sie für diese Intents registriert werden. Wenn die Aktivität in den Hintergrund verschoben und für den Benutzer nicht mehr sichtbar ist, sollte die Registrierung des Empfängers aufgehoben werden, da die Benutzeroberfläche zum Anzeigen der Updates nicht mehr sichtbar ist. Der folgende Code Ausschnitt ist ein Beispiel für die Registrierung und Aufhebung der Registrierung eines Broadcast Empfängers im Kontext einer Aktivität:

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

Wenn im vorherigen Beispiel die Aktivität in den Vordergrund rückt, registriert Sie einen Broadcast Empfänger, der mithilfe der `OnResume` Lifecycle-Methode auf eine benutzerdefinierte Absicht lauscht. Wenn die Aktivität in den Hintergrund wechselt, wird `OnPause()` die Registrierung des Empfängers von der Methode aufgehoben.

## <a name="publishing-a-broadcast"></a>Veröffentlichen einer Übertragung

Eine Übertragung kann für alle auf dem Gerät installierten apps veröffentlicht werden, wodurch ein Intent-Objekt erstellt und mit `SendBroadcast` der- `SendOrderedBroadcast` Methode oder der-Methode versendet wird.  

1. **Context. sendbroadcast-Methoden** &ndash; Es gibt mehrere Implementierungen dieser Methode.
   Diese Methoden übertragen die Absicht an das gesamte System. Broadcast Empfänger, die die Absicht in einer unbestimmten Reihenfolge empfangen. Dies bietet ein hohes Maß an Flexibilität, bedeutet aber, dass es möglich ist, dass andere Anwendungen die Absicht registrieren und erhalten. Dies kann ein potenzielles Sicherheitsrisiko darstellen. Anwendungen müssen möglicherweise zusätzliche Sicherheit implementieren, um nicht autorisierten Zugriff zu verhindern. Eine mögliche Lösung besteht darin, die `LocalBroadcastManager` zu verwenden, die nur Nachrichten innerhalb des privaten Raums der APP verteilt. Dieser Code Ausschnitt ist ein Beispiel dafür, wie Sie eine Absicht mithilfe einer der `SendBroadcast` -Methoden verteilen:

   ```csharp
   Intent message = new Intent("com.xamarin.example.TEST");
   // If desired, pass some values to the broadcast receiver.
   message.PutExtra("key", "value");
   SendBroadcast(message);
   ```

    Dieser Code Ausschnitt ist ein weiteres Beispiel für das Senden einer Broadcast- `Intent.SetAction` Methode mithilfe der-Methode, um die Aktion zu identifizieren:

    ```csharp
    Intent intent = new Intent();
    intent.SetAction("com.xamarin.example.TEST");
    intent.PutExtra("key", "value");
    SendBroadcast(intent);
    ```

2. **Context. sendorderedbroadcast** &ndash; diese Methode `Context.SendBroadcast`ist sehr ähnlich, wobei der Unterschied darin besteht, dass die Absicht bei Empfängern nacheinander in der Reihenfolge veröffentlicht wird, in der die Empfänger registriert wurden.

### <a name="localbroadcastmanager"></a>LocalBroadcastManager

Die [xamarin-Unterstützungs Bibliothek V4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) bietet eine Hilfsprogrammklasse mit dem Namen [`LocalBroadcastManager`](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html). Der `LocalBroadcastManager` ist für apps bestimmt, die keine Übertragungen von anderen apps auf dem Gerät senden oder empfangen möchten. Der `LocalBroadcastManager` veröffentlicht nur Meldungen im Kontext der Anwendung und nur für die Broadcast Empfänger, die `LocalBroadcastManager`beim registriert sind. Dieser Code Ausschnitt ist ein Beispiel für die Registrierung eines Broadcast Empfängers bei `LocalBroadcastManager`:

```csharp
Android.Support.V4.Content.LocalBroadcastManager.GetInstance(this). RegisterReceiver(receiver, new IntentFilter("com.xamarin.example.TEST"));
```

Andere apps auf dem Gerät können keine Nachrichten empfangen, die mit `LocalBroadcastManager`veröffentlicht werden. In diesem Code Ausschnitt wird gezeigt, wie eine Absicht mithilfe `LocalBroadcastManager`von versendet wird:

```csharp
Intent message = new Intent("com.xamarin.example.TEST");
// If desired, pass some values to the broadcast receiver.
message.PutExtra("key", "value");
Android.Support.V4.Content.LocalBroadcastManager.GetInstance(this).SendBroadcast(message);
```

## <a name="related-links"></a>Verwandte Links

- [Broadcastreceiver-API](xref:Android.Content.BroadcastReceiver)
- [Context. registerreceiver-API](xref:Android.Content.Context.RegisterReceiver*)
- [Context. sendbroadcast-API](xref:Android.Content.Context.SendBroadcast*)
- [Context. unregisterreceiver-API](xref:Android.Content.Context.UnregisterReceiver*)
- [Intent-API](xref:Android.Content.Intent)
- [Intentfilter-API](xref:Android.App.IntentFilterAttribute)
- [Localbroadcastmanager (Android-Dokumentation)](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))
- [Lokale Benachrichtigungen in der Android](~/android/app-fundamentals/notifications/local-notifications.md) -Anleitung
- [Android-Unterstützungs Bibliothek V4 (nuget)](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
