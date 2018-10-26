---
title: Jelly Bean Features
description: 'Dieses Dokument bietet einen allgemeinen Überblick über die neuen Features für Entwickler, die in Android 4.1 eingeführt wurden. Zu diesen Funktionen gehören: verbesserte Benachrichtigungen, Updates für Android-Funktionen zum Freigeben von großer Dateien, Updates für multimedia, Peer-zu-Peer die Netzwerkermittlung, Animationen, neue Berechtigungen.'
ms.prod: xamarin
ms.assetid: 23F57634-2EF9-5C15-C710-B3E19A5AF7E1
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 9e83c9a8c1e2740596a981598cafbbfb65e2caf2
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50119240"
---
# <a name="jelly-bean-features"></a>Jelly Bean Features

_Dieses Dokument bietet einen allgemeinen Überblick über die neuen Features für Entwickler, die in Android 4.1 eingeführt wurden. Zu diesen Funktionen gehören: verbesserte Benachrichtigungen, Updates für Android-Funktionen zum Freigeben von großer Dateien, Updates für multimedia, Peer-zu-Peer die Netzwerkermittlung, Animationen, neue Berechtigungen._



## <a name="overview"></a>Übersicht

Android 4.1 (API Level 16), auch bekannt als "Jelly Bean", wurde der Veröffentlichung am 9. Juli 2012. Dieser Artikel bietet eine allgemeine Einführung in einige der neuen Features in Android 4.1 für Entwickler, die mit Xamarin.Android. Einige dieser neuen Funktionen sind Verbesserungen bei der Animationen für den Start von einer Aktivität, die neue Sounds für eine Kamera und verbesserte Unterstützung für die Navigation in der Anwendung Stapel. Es ist jetzt möglich, Ausschneiden und Einfügen mit Intent-Elemente.

Die Stabilität des Android-Anwendungen wird verbessert, mit der Möglichkeit, um die Abhängigkeit von instabil Inhaltsanbieter zu isolieren. Dienste können auch isoliert werden, damit sie nur von der Aktivität zugegriffen werden kann, die sie gestartet hat.

Unterstützung wurde für die Verwendung von Network Service Discovery hinzugefügt, dass Bonjour, UPnP oder multicast-DNS-Dienste basierend. Es ist jetzt möglich, für umfangreichere Benachrichtigungen, die Text, Aktionsschaltflächen und große Bilder formatiert wurde.

Schließlich wurden mehrere neue Berechtigungen im Android 4.1 hinzugefügt.



## <a name="requirements"></a>Anforderungen

Zum Entwickeln von Xamarin.Android-Anwendungen mithilfe von Jelly Bean erfordert Xamarin.Android 4.2.6 oder höher sowie Android 4.1 (API Level 16) installiert werden über den Android SDK-Manager wie im folgenden Screenshot gezeigt:

[![Wählen im Android SDK Manager Android 4.1](jelly-bean-images/image1.png)](jelly-bean-images/image1.png#lightbox)



## <a name="whats-new"></a>Neues



### <a name="animations"></a>Animations

Aktivitäten können gestartet werden, mithilfe von Zoom-Animationen oder benutzerdefinierte Animationen mithilfe der `ActivityOptions` Klasse. Die folgenden neuen Methoden werden bereitgestellt, um diese Animationen zu unterstützen:

-   `MakeScaleUpAnimation` – Dadurch wird eine Animation erstellt, die Sie ein Aktivitätsfenster aus eine Anfangsposition und die Größe auf dem Bildschirm skaliert werden kann.
-   `MakeThumbnailScaleUpAnimation` – Erstellt eine Animation, die skaliert werden, um kann von einer Miniaturansicht aus der angegebenen Position auf dem Bildschirm.
-   `MakeCustomAnimation` – Erstellt eine Animation von Ressourcen in der Anwendung. Es gibt eine Animation für, wenn die Aktivität wird geöffnet und eine für die Beendigung der Aktivitäts.


Die neue `TimeAnimator` Klasse stellt eine Schnittstelle `TimeAnimator.ITimeListener` benachrichtigen, die einer Anwendung jedes Mal, wenn ein Frame einer Animation ändert. Betrachten Sie beispielsweise die folgende Implementierung des `TimeAnimator.ITimeListener`:

```csharp
class MyTimeListener : Java.Lang.Object,  TimeAnimator.ITimeListener
{
    public void OnTimeUpdate(TimeAnimator animation, long totalTime, long deltaTime)
    {
        Log.Debug("Activity1", "totalTime={0}, deltaTime={1}", totalTime, deltaTime);
    }
}
```

Und jetzt mit der Klasse eine Instanz von `TimeAnimator` erstellt wird, und legen Sie der Listener ist:

```csharp
var animator = new TimeAnimator();
animator.SetTimeListener(new MyTimeListener());
animator.Start();
```

Als die `TimeAnimator` wird ausgeführt, es wird aufgerufen `ITimeAnimator.ITimeListener`, die protokolliert dann wie lange der Animator wurde ausgeführt wird und wie lange es da seit der letzten Ausführung die Methode wurde aufgerufen wurde.



### <a name="application-stack-navigation"></a>Stack Anwendungsnavigation

Android 4.1 profitiert von der Anwendung Stack Navigation, die in Android 3.0 eingeführt wurde. Durch Angabe der `ParentName` Eigenschaft der `ActivityAttribute`, Android kann die richtige übergeordnete Aktivität öffnen, wenn der Benutzer drückt die [Schaltfläche](http://developer.android.com/design/patterns/navigation.html#up-vs-back) in der Aktionsleiste - Android instanziiert die Aktivität, die gemäß der `ParentName`Eigenschaft. Dadurch können Anwendungen für die Hierarchie von Aktivitäten zu erhalten, die eine bestimmte Aufgabe zu machen.

Für die meisten Anwendungen, die Einstellung der `ParentName` für die Aktivität ist genügend Informationen für Android, um das richtige Verhalten bereitzustellen, für die Navigation in den Anwendungsstapel Android werden die erforderlichen BackStack künstlich herzustellen, erstellen Sie eine Reihe von Intents für jede übergeordnete Aktivität. Da es sich um ein künstliches Anwendungsstapel handelt, wird aktivitätsspezifischen synthetische müssen jedoch nicht den gespeicherten Zustand, den eine natürliche Aktivität hätte. Eine Aktivität kann zum gespeicherten Zustand, um eine synthetische übergeordnete Aktivität bereitstellen möchten, überschreiben die `OnPrepareNavigationUpTaskStack` Methode. Diese Methode empfängt einen `TaskStackBuilder` -Instanz, die eine Auflistung von Absicht hat-Objekten, die zum Erstellen von des BackStack Android verwenden. Die Aktivität kann diese Intent-Elemente ändern, damit, wie die synthetische Aktivität erstellt wurde, Informationen über den richtigen Zustand empfangen wird.

Für komplexere Szenarien sind es neue Methoden in der Activity-Klasse, die verwendet werden kann, um das Verhalten der Sie die Navigation verarbeiten, und erstellen den Backstack:

-   `OnNavigateUp` – Durch das Überschreiben dieser Methode ist es möglich, eine benutzerdefinierte Aktion ausführen bei der <span class="ui">einrichten</span> gedrückt wird.
-   `NavigateUpTo` – Aufrufen dieser Methode bewirkt, dass die Anwendung, von der aktuellen Aktivität zu navigieren, auf die Aktivität durch eine bestimmte Absicht angegeben wird.
-   `ParentActivityIntent` – Dies wird verwendet, um ein Intent-Objekt zu erhalten, die die übergeordnete Aktivität der aktuellen Aktivität gestartet wird.
-   `ShouldUpRecreateTask` – Diese Methode wird verwendet, um Abfragen, wenn synthetische BackStack erstellt werden muss, um bis zu einer übergeordneten Aktivität zu navigieren. Gibt `true` Wenn synthetische Stapel erstellt werden muss. 
-   `FinishAffinity` – Aufrufen dieser Methode wird abgeschlossen, der aktuellen Aktivität und alle Aktivitäten darunter in der aktuellen Aufgabe, die die gleiche Aufgabe Affinität.
-   `OnCreateNavigateUpTaskStack` – Diese Methode wird überschrieben, wenn es ist erforderlich, Sie haben vollständige Kontrolle über die wie der synthetische Stapel erstellt wird.




### <a name="camera"></a>Kamera

Es gibt eine neue Schnittstelle, `Camera.IAutoFocusMoveCallback`, die können verwendet werden, zu erkennen, wann die Autofokus wurde gestartet oder beendet wird, verschieben. Ein Beispiel für diese neue Schnittstelle kann in den folgenden Codeausschnitt angezeigt werden:

```csharp
public class AutoFocusCallbackActivity : Activity, Camera.IAutoFocusCallback
{
    public void OnAutoFocus(bool success, Camera camera)
    {
        // camera is an instance of the camera service object.

        if (success)
        {
            // Auto focus was successful - do something here.
        }
        else
        {
            // Auto focus didn't happen for some reason - react to that here.
        }
    }
}
```

Die neue Klasse `MediaActionSound` bietet eine Reihe von APIs für die Erzeugung von Sounds für die verschiedene Medien. Es sind mehrere Aktionen, die mit einer Kamera, auftreten können, diese werden durch die Enumeration definiert `Android.Media.MediaActionSoundType`:

-   `MediaActionSoundType.FocusComplete` – Diese Sound, der wiedergegeben wird, wenn konzentrieren abgeschlossen wurde.
-   `MediaActionSoundType.ShutterClick` : Der Klang wird abgespielt, wenn vergrößern Image weiterhin ausgeführt wird.
-   `MediaActionSoundType.StartVideoRecording` – Der Klang wird verwendet, stehen für den Beginn der videoaufzeichnung.
-   `MediaActionSoundType.StopVideoRecording` – Der Klang wird abgespielt, um das Ende der videoaufzeichnung anzugeben.


Ein Beispiel zur Verwendung der `MediaActionSound` Klasse finden Sie in den folgenden Codeausschnitt:

```csharp
var mediaActionPlayer = new MediaActionSound();

// Preload the sound for a shutter click.
mediaActionPlayer.Load(MediaActionSoundType.ShutterClick);
var button = FindViewById<Button>(Resource.Id.MyButton);

// Play the sound on a button click.
button.Click += (sender, args) => mediaActionPlayer.Play(MediaActionSoundType.ShutterClick);

// This releases the preloaded resources. Don’t make any calls on
// mediaActionPlayer after this.
mediaActionPlayer.Release();
```



### <a name="connectivity"></a>Konnektivität



#### <a name="android-beam"></a>Android-Funktionen

Android-Funktionen ist eine Grundlage NFC-Technologie, die zwei Android-Geräte, die miteinander kommunizieren kann. Android 4.1 bietet bessere Unterstützung für die Übertragung großer Dateien. Bei Verwendung der neuen Methode `NfcAdapter.SetBeamPushUris()` Android wird zwischen alternativen Transportmechanismen (z. B. Bluetooth) wechseln, um eine schnelle Übertragung Geschwindigkeit zu erreichen.



#### <a name="network-services-discovery"></a>Dienste-Netzwerkermittlung

Android 4.1 enthält neue API für die multicast-Basis von DNS-Dienst-Ermittlung.
Dadurch kann eine Anwendung zu erkennen und andere Geräte wie Drucker, Kameras und Mediengeräte über Wi-Fi-Verbindung. Diese neuen APIs befinden sich in der `Android.Net.Nsd` Paket.

Um einen Dienst zu erstellen, die von anderen Diensten genutzt werden kann die `NsdServiceInfo` Klasse wird verwendet, um ein Objekt zu erstellen, die die Eigenschaften eines Diensts definieren. Dieses Objekt wird dann bereitgestellt, um `NsdManager.RegisterService()` sowie eine Implementierung von `NsdManager.ResolveListener`. Implementierungen von `NsdManager.ResolveListener` werden verwendet, das über eine erfolgreiche Registrierung benachrichtigt und Aufheben der dienstregistrierung.

Zum Ermitteln der Dienste im Netzwerk und die Implementierung von `Nsd.DiscoveryListener` übergeben `NsdManager.discoverServices()`.



#### <a name="network-usage"></a>Analysieren der Netzwerkverwendung

Eine neue Methode `ConnectivityManager.IsActiveNetworkMetered` kann ein Gerät zum Überprüfen, ob er mit einer getakteten Netzwerken verbunden ist. Diese Methode kann zum Verwalten der Datennutzung durch Benutzer genau darüber informiert, dass möglicherweise teure Gebühren für Datenvorgänge verwendet werden.



#### <a name="wifi-direct-service-discovery"></a>WiFi Direct-Dienstermittlung

Die `WifiP2pManager` Klasse seit Android 4.0 unterstützen *Zeroconf*. Zeroconf (zero Configuration-Netzwerke) ist eine Reihe von Techniken, die Geräte (Computer, Drucker, Smartphones), zum Verbinden mit Netzwerken automatisch zulässt, mit menschlichen Netzwerkbetreiber oder spezielle Konfigurationsserver eingreifen muss.

In Jelly Bean `WifiP2pManager` erkennen, Geräten, die entweder in der Nähe *Bonjour* oder *Upnp*. Bonjour ist Apple Implementierung Zeroconf. UPnP ist festgelegt, der Netzwerkprotokolle, die auch Zeroconf unterstützt. Die folgenden Methoden hinzugefügt, um die `WiFiP2pManager` zur Unterstützung von WLAN-Dienst-Ermittlung:

-   `AddLocalService()` – Diese Methode wird verwendet, eine Anwendung als Dienst über Wi-Fi ankündigen, für die Ermittlung von Peers.
-   `AddServiceRequest(` ) – Diese Methode ist eine dienstanforderung für die Ermittlung für das Framework senden. Es wird zum Initialisieren der WLAN-Dienst-Ermittlungs.
-   `SetDnsSdResponseListeners()` – Diese Methode wird verwendet, um Rückrufe aufgerufen werden, nach dem Empfang einer Antwort auf suchanforderungen von Bonjour zu registrieren.
-   `SetUpnpServiceResponseListener()` – Diese Methode wird verwendet, um Rückrufe aufgerufen werden, auf den Erhalt einer Antwort auf die Upnp-ermittlungsanforderungen zu registrieren.




### <a name="content-providers"></a>Inhaltsanbieter

Die `ContentResolver` Klasse hat eine neue Methode `AcquireUnstableContentProvider`. Diese Methode kann es sich um eine Anwendung, einen "instabilen" Content-Anbieter abzurufen. Normalerweise, wenn eine Anwendung Inhaltsanbieter erwirbt, und dieser Inhaltsanbieter stürzt ab, daher wird die Anwendung. Mit diesem Methodenaufruf wird eine Anwendung nicht abstürzt, wenn Inhaltsanbieter abstürzt. Stattdessen `Android.OS.DeadObjectionException` ausgelöst von Aufrufen für den Inhaltsanbieter eine Anwendung zu informieren, die der Inhaltsanbieter verschwunden ist. Ein "instabil" Inhaltsanbieter ist nützlich, bei der Interaktion mit Inhaltsanbieter aus anderen Anwendungen – es ist weniger wahrscheinlich, dass fehlerhaften Code aus einer anderen Anwendung einer anderen Anwendung auswirkt.



### <a name="copy-and-paste-with-intents"></a>Kopieren und Einfügen mit Intent-Elemente

Die `Intent` Klasse haben jetzt eine `ClipData` über zugeordnete Objekt der `Intent.ClipData` Eigenschaft. Diese Methode ermöglicht zusätzliche Daten aus der Zwischenablage mit der Absicht übertragen werden. Eine Instanz von `ClipData` kann eine oder mehrere enthalten `ClipData.Item`. `ClipData.Item`die Elemente der folgenden Typen sind:

-   **Text** : Dies ist eine beliebige Zeichenfolge entweder HTML-Text oder eine beliebige Zeichenfolge, deren Format wird von den integrierten Android Stil unterstützt, umfasst.
-  **Beabsichtigte** – beliebig `Intent` Objekt.
-   **URI** – Dies kann einen beliebigen URI ein, z. B. ein HTTP-Lesezeichen oder den URI zum Inhaltsanbieter sein.




### <a name="isolated-services"></a>Isolierte Dienste

Ein isolierter Dienst ist ein Dienst, der unter seinem eigenen speziellen Prozess ausgeführt wird, und verfügt über keine Berechtigungen, über eigene. Der nur Kommunikation mit dem Dienst ist, wenn der Dienst gestartet, und Binden an ihn über die Dienst-API. Es ist möglich, einen Dienst als isolierte durch Festlegen der Eigenschaft deklarieren `IsolatedProcess="true"` in die `ServiceAttribute` , die eine Dienstklasse verziert.


### <a name="media"></a>Medien

Die neue `Android.Media.MediaCodec` -Klasse stellt eine API für die Low-Level-Media-Codecs. Anwendungen können Abfragen, das System, um herauszufinden, welche low-Level-Codecs auf dem Gerät verfügbar sind.

Die neue `Android.Media.Audiofx.AudioEffect` Unterklassen unterstützen zusätzliche vorverarbeitung auf die aufgezeichneten Audiodaten Audio hinzugefügt wurden:

-   `Android.Media.Audiofx.AcousticEchoCanceler` – Diese Klasse wird für Vorverarbeitungsskripts wird verarbeitet Audio verwendet, um das Signal aus einem Remoteanbieter aus einem erfassten Audiosignal zu entfernen. Entfernen z. B. die Echo aus einer Sprach-Communication-Anwendung.
-   `Android.Media.Audiofx.AutomaticGainControl` – Diese Klasse wird zum Normalisieren der erfassten Signals durch boosting oder ein Eingangssignal verringern, sodass Ausgangssignal konstant ist.
-   `Android.Media.Audiofx.NoiseSuppressor` – Diese Klasse wird Hintergrundgeräusche aus dem erfassten Signal entfernt.


Nicht alle Geräte werden dieser Effekte unterstützt. Die Methode `AudioEffect.IsAvailable` aufgerufen werden soll, von einer Anwendung, um festzustellen, ob die betreffende Audioeffekt auf dem Gerät, das Ausführen der Anwendung unterstützt wird.

Die `MediaPlayer` -Klasse unterstützt nun die lückenloses Wiedergabe mit der `SetNextMediaPlayer()` Methode. Diese neue Methode gibt MediaPlayer den nächsten zu starten, wenn der aktuelle MediaPlayer die Wiedergabe abgeschlossen ist.

Die folgenden neuen Klassen bieten die Standardmechanismen und die Benutzeroberfläche für die Auswahl, in denen Medien wiedergegeben wird:

-   `MediaRouter` – Diese Klasse ermöglicht Anwendungen das routing von Media-Kanälen, von einem Gerät, um externe Lautsprecher oder andere Geräte steuern.
-   `MediaRouterActionProvider` und `MediaRouteButton` – diese Klassen bieten eine einheitliche Benutzeroberfläche zum auswählen und Wiedergeben von Medien.




### <a name="notifications"></a>Benachrichtigungen

Android 4.1 ermöglicht Anwendungen, mehr Flexibilität und Kontrolle mit Anzeige von Benachrichtigungen. Anwendungen können jetzt größer und bessere Benachrichtigungen für Benutzer angezeigt. Eine neue Methode `NotificationBuilder.SetStyle()` für eines der neuen drei neue Formatvorlage für Benachrichtigungen festgelegt werden können:

-   `Notification.BigPictureStyle` – Dies ist eine Hilfsklasse, die Benachrichtigungen generieren, die ein Bild enthalten ist. Die folgende Abbildung zeigt ein Beispiel für eine Benachrichtigung mit der ein großes Bild:


 [![Beispielscreenshot, der eine Benachrichtigung BigPictureStyle](jelly-bean-images/image2.png)](jelly-bean-images/image2.png#lightbox)

-   `Notification.BigTextStyle` – Dies ist eine Hilfsklasse, die Benachrichtigungen generieren, die mehrere Zeilen Text, wie z. B. E-mail hat. Ein Beispiel für diesen neuen benachrichtigungs-Stil kann im folgenden Screenshot angezeigt werden:


 [![Beispielscreenshot, der eine Benachrichtigung BigTextStyle](jelly-bean-images/image3.png)](jelly-bean-images/image3.png#lightbox)

-   `Notification.InboxStyle` – Dies ist eine Hilfsklasse, die Benachrichtigungen, die eine Liste von Zeichenfolgen, z. B.-Ausschnitte aus einem e-Mail-Nachricht enthalten generiert wird, wie im folgenden Screenshot gezeigt:


 [![Beispielscreenshot, der eine Benachrichtigung Notification.InboxStyle](jelly-bean-images/image4.png)](jelly-bean-images/image4.png#lightbox)

Es ist möglich, bis zu zwei Aktionsschaltflächen am unteren Rand eine benachrichtigungsmeldung hinzufügen, wenn die Benachrichtigung den Stil für normale oder höher verwendet wird.
Ein Beispiel hierfür kann im folgenden Screenshot angezeigt werden, in dem die Aktion-Schaltflächen am unteren Rand der Benachrichtigung sichtbar sind:

 [![Beispielhafter Screenshot der Aktionsschaltflächen unten eine benachrichtigungsmeldung angezeigt](jelly-bean-images/image5.png)](jelly-bean-images/image5.png#lightbox)

Die `Notification` Klasse neue Konstanten, mit denen Entwickler geben Sie einen der fünf Prioritätsstufen für eine Benachrichtigung empfangen hat. Diese können festgelegt werden, auf eine Benachrichtigung mit der `Priority` Eigenschaft.



### <a name="permissions"></a>Berechtigungen

Die folgenden neuen Berechtigungen wurden hinzugefügt:

-   `READ_EXTERNAL_STORAGE` -Die Anwendung muss es sich um schreibgeschützten Zugriff auf den externen Speicher. Derzeit haben alle Anwendungen Lesezugriff standardmäßig, jedoch zukünftige Versionen von Android erfordert, dass Anwendungen explizit Lesezugriff anfordern.
-   `READ_USER_DICTIONARY` – Ermöglicht Lesezugriff auf die Word-Wörterbuch des Benutzers.
-   `READ_CALL_LOG` – Ermöglicht einer Anwendung zum Abrufen von Informationen zu ein- und ausgehende Aufrufe durch Lesen des Protokolls für Aufruf.
-   `WRITE_CALL_LOG` – Ermöglicht einer Anwendung in die Aufruf-Protokoll auf dem Telefon zu schreiben.
-   `WRITE_USER_DICTIONARY` – Ermöglicht einer Anwendung zum Schreiben in Word-Wörterbuch des Benutzers.


Eine wichtige Änderung beachten `READ_EXTERNAL_STORAGE` – mit dieser Berechtigung wird derzeit automatisch gewährt, von Android. Zukünftige Versionen von Android benötigen diese Berechtigung vor dem anfordern eine Anwendung die Berechtigung erteilt.



## <a name="summary"></a>Zusammenfassung

In diesem Artikel eingeführt, einige der neuen APIs, die in Android 4.1 (API Level 16) verfügbar sind. Sie einige der Änderungen für Animationen und animieren den Start einer Aktivität markiert und der neuen APIs des für die Netzwerkermittlung von anderen Geräten, die über Protokolle wie Bonjour oder UPnP eingeführt. Andere Änderungen an der API wurden ebenfalls hervorgehoben, wie z. B. die Möglichkeit, Ausschneiden und Einfügen von Daten über Intent-Elemente, die Fähigkeit, isolierte Dienste oder "instabil" Content-Anbieter verwenden.

In diesem Artikel dann führen Sie die Updates in Benachrichtigungen passiert und erläutert einige der neuen Berechtigungen, die mit Android 4.1 eingeführt wurden


## <a name="related-links"></a>Verwandte Links

- [Animation Uhrzeitbeispiel (Beispiel)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/TimeAnimatorExample/)
- [Android 4.1 APIs](http://developer.android.com/about/versions/android-4.1.html)
- [Aufgaben und Back-Stapel](http://developer.android.com/guide/components/tasks-and-back-stack.html)
- [Navigation mit "zurück" und nach-oben](http://developer.android.com/design/patterns/navigation.html)
