---
title: Gelee Bean-Funktionen
description: "Dieses Dokument bietet eine grobe Übersicht der neuen Funktionen für Entwickler, die in Android 4.1 eingeführt wurden. Zu diesen Funktionen gehören: Erweiterte Benachrichtigungen, Updates für Android Balken zum Freigeben von großer Dateien, Updates multimedia, Peer-zu-Peer-Netzwerkermittlung, Animationen, neue Berechtigungen."
ms.topic: article
ms.prod: xamarin
ms.assetid: 23F57634-2EF9-5C15-C710-B3E19A5AF7E1
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/14/2017
ms.openlocfilehash: 2e54bfc4bea3955dc80a747c4ecce485b78ada1d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="jelly-bean-features"></a>Gelee Bean-Funktionen

_Dieses Dokument bietet eine grobe Übersicht der neuen Funktionen für Entwickler, die in Android 4.1 eingeführt wurden. Zu diesen Funktionen gehören: Erweiterte Benachrichtigungen, Updates für Android Balken zum Freigeben von großer Dateien, Updates multimedia, Peer-zu-Peer-Netzwerkermittlung, Animationen, neue Berechtigungen._

<a name="Overview" />


## <a name="overview"></a>Übersicht

Android 4.1 (API Level 16), auch bekannt als "Gelee Bean", wurde die Veröffentlichung am 9. Juli 2012. Dieser Artikel bietet eine Einführung in Übersichtsform auf einige der neuen Funktionen in Android 4.1 für Entwickler, die mit Xamarin.Android. Einige dieser neuen Funktionen eingeführt werden Erweiterungen für Animationen für das Starten einer Aktivität, neue Sounds einer Kamera und verbesserte Unterstützung für die Navigation in der Anwendung Stapel. Es ist jetzt möglich, Ausschneiden und Einfügen mit Intents.

Die Stabilität des Android-Anwendungen wird verbessert, die Möglichkeit, die Abhängigkeit von instabil Inhaltsanbieter zu isolieren. Dienste können auch isoliert werden, damit sie nur von der Aktivität zugegriffen werden kann, die sie gestartet.

Unterstützung wurde für die Verwendung von Network Service Discovery, dass je nach Bonjour, UPnP oder multicast DNS-Dienste hinzugefügt. Es ist jetzt möglich, dass umfangreichere Benachrichtigungen, die Text, Aktionsschaltflächen und große Bilder formatiert sind.

Schließlich wurden mehrere neue Berechtigungen in Android 4.1 hinzugefügt.

 <a name="Requirements" />


## <a name="requirements"></a>Anforderungen

Xamarin.Android Anwendungsentwicklung mit Gelee Bean Xamarin.Android erfordert 4.2.6 oder höher sowie Android 4.1 (API Level 16) installiert sein, über den Android SDK Manager wie im folgenden Screenshot gezeigt:

[![Wählen im Android SDK Manager Android 4.1](jelly-bean-images/image1.png)](jelly-bean-images/image1.png)

 <a name="What's_New" />


## <a name="whats-new"></a>Neues

 <a name="Animations" />


### <a name="animations"></a>Animationen

Aktivitäten können gestartet werden, mithilfe von Zoom Animationen oder benutzerdefinierten Animationen mithilfe der `ActivityOptions` Klasse. Die folgenden neuen Methoden werden bereitgestellt, um diese Animationen zu unterstützen:

-   `MakeScaleUpAnimation` – Hiermit wird eine Animation erstellt, die sich aus einer Anfangsposition und die Größe auf dem Bildschirm ein Aktivitätsfenster skaliert.
-   `MakeThumbnailScaleUpAnimation` – Erstellt eine Animation, die adapterleistung aus ein Miniaturbild aus der angegebenen Position auf dem Bildschirm.
-   `MakeCustomAnimation` – Erstellt eine Animation über Ressourcen in der Anwendung. Es gibt eine Animation für die Aktivität beim Öffnen und eine andere, wenn die Aktivität beendet.


Die neue `TimeAnimator` Klasse stellt eine Schnittstelle `TimeAnimator.ITimeListener` benachrichtigen, die einer Anwendung, jedes Mal ein Frame in einer Animation ändert. Betrachten Sie beispielsweise die folgende Implementierung des `TimeAnimator.ITimeListener`:

```csharp
class MyTimeListener : Java.Lang.Object,  TimeAnimator.ITimeListener
{
    public void OnTimeUpdate(TimeAnimator animation, long totalTime, long deltaTime)
    {
        Log.Debug("Activity1", "totalTime={0}, deltaTime={1}", totalTime, deltaTime);
    }
}
```

Und nun die Klasse, eine Instanz von verwendet `TimeAnimator` erstellt wurde, und der Listener festgelegt ist:

```csharp
var animator = new TimeAnimator();
animator.SetTimeListener(new MyTimeListener());
animator.Start();
```

Als die `TimeAnimator` wird ausgeführt, es wird aufgerufen `ITimeAnimator.ITimeListener`, melden die dann wie lange der Animator wurde ausgeführt wird und wie lange es als seit der letzten Ausführung die Methode wurde aufgerufen wurde.

 <a name="Application_Stack_Navigation" />


### <a name="application-stack-navigation"></a>Navigation in der Anwendung Stapel

Android 4.1 verbessert die Navigation in der Anwendung Stapel, die in Android 3.0 eingeführt wurde. Durch Angabe der `ParentName` Eigenschaft von der `ActivityAttribute`, Android kann die richtige übergeordnete Aktivität öffnen, wenn der Benutzer drückt die [Schaltfläche](http://developer.android.com/design/patterns/navigation.html#up-vs-back) auf der Aktionsleiste - Android die Aktivität, die gemäß der instanziiertdann`ParentName`Eigenschaft. Dadurch können Anwendungen Hierarchie von Aktivitäten beibehalten, die einen bestimmten Task ausführen zu können.

Für die meisten Anwendungen, die Einstellung der `ParentName` für die Aktivität ist ausreichend Informationen für Android aus, um das richtige Verhalten bereitzustellen, für die Navigation in den Anwendungsstapel; Android werden die erforderlichen rückwärts-zählerstil formatiert ist. durch eine Reihe von Intents für jede übergeordnete Aktivität erstellen. Da dies eine künstliche Anwendungsstapel ist, müssen jede synthetische Aktivität nicht den gespeicherten Zustand Sie allerdings, den eine natürliche Aktivität hätte. Eine Aktivität kann um gespeicherten Zustand für eine synthetische übergeordneten Aktivität bereitzustellen, überschreiben die `OnPrepareNavigationUpTaskStack` Methode. Diese Methode empfängt einen `TaskStackBuilder` Instanz, für die eine Auflistung von Absicht-Objekten, die zum Erstellen von die rückwärts-Android verwenden. Die Aktivität kann diese Intents ändern, sodass, wie die synthetische Aktivität erstellt wurde, Informationen über den richtigen Zustand empfangen wird.

Für komplexere Szenarien gibt es neue Methoden für die Activity-Klasse, die verwendet werden kann, die das Verhalten der Sie die Navigation zu behandeln, und erstellen die rückwärts-:

-   `OnNavigateUp` – Durch das Überschreiben dieser Methode ist es möglich, führen Sie eine benutzerdefinierte Aktion bei der <span class="ui">einrichten</span> gedrückt wird.
-   `NavigateUpTo` – Beim Aufrufen dieser Methode bewirkt, dass die Anwendung aus der aktuellen Aktivität navigieren, an die Aktivität, die durch einen bestimmten Zweck angegeben wird.
-   `ParentActivityIntent` – Dies wird verwendet, um Priorität erhalten, die die übergeordnete Aktivität der aktuellen Aktivität gestartet wird.
-   `ShouldUpRecreateTask` – Diese Methode wird verwendet, um Abfragen, wenn synthetische rückwärts-erstellt werden muss, um bis zu einer übergeordneten Aktivität zu navigieren. Gibt `true` Wenn synthetische Stapel erstellt werden muss. 
-   `FinishAffinity` – Das Aufrufen dieser Methode wird abgeschlossen, die aktuelle Aktivität und alle Aktivitäten darunter in der aktuellen Aufgabe, die die gleiche Aufgabe Affinität besitzen.
-   `OnCreateNavigateUpTaskStack` – Diese Methode wird überschrieben, wenn es ist notwendig, haben vollständige Kontrolle über wie synthetische Stapel erstellt wird.


 <a name="Camera" />


### <a name="camera"></a>Kamera

Es wird eine neue Schnittstelle `Camera.IAutoFocusMoveCallback`, die verwendet werden können, zu erkennen, wenn der Fokus automatisch gestartet oder beendet wird, verschieben. Ein Beispiel für die neue Oberfläche kann in der folgende Codeausschnitt angezeigt werden:

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

Die neue Klasse `MediaActionSound` stellt einen Satz von APIs für die Erzeugung von Sounds für verschiedene Medien-Aktionen. Es gibt mehrere Aktionen, die mit einer Kamera auftreten können, werden diese durch die Enumeration definiert `Android.Media.MediaActionSoundType`:

-   `MediaActionSoundType.FocusComplete` – Mit diesem Sound, der wiedergegeben wird, wenn Fokussierung abgeschlossen wurde.
-   `MediaActionSoundType.ShutterClick` – Mit diesem Sound wird wiedergegeben, wenn ein noch Image Bild aufgenommen wird.
-   `MediaActionSoundType.StartVideoRecording` – Mit diesem Sound wird verwendet, stehen für den Beginn der videoaufzeichnung.
-   `MediaActionSoundType.StopVideoRecording` – Mit diesem Sound wird abgespielt, um das Ende des videoaufzeichnung anzugeben.


Ein Beispiel zum Verwenden der `MediaActionSound` Klasse im folgenden Ausschnitt angezeigt werden kann:

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

 <a name="Connectivity" />


### <a name="connectivity"></a>Konnektivität

 <a name="Android_Beam" />


#### <a name="android-beam"></a>Android Beam

Android Balken wird eine Basis NFC-Technologie, die zwei Android-Geräte, die miteinander kommunizieren kann. Android 4.1 bietet eine bessere Unterstützung für die Übertragung großer Dateien. Bei Verwendung der neuen Methode `NfcAdapter.SetBeamPushUris()` Android wird zwischen alternativen Transportmechanismen (z. B. Bluetooth) wechseln, um eine schnelle Übertragungsrate zu erreichen.

 <a name="Network_Services_Discovery" />


#### <a name="network-services-discovery"></a>Dienste-Netzwerkermittlung

Android 4.1 enthält neue API für die multicast-DNS-basierter Dienstermittlung.
Dabei kann es sich um eine Anwendung zu erkennen und andere Geräte wie Drucker, Kameras und Mediengeräte über Wi-Fi-Verbindung. Diese neuen APIs befinden sich in der `Android.Net.Nsd` Paket.

Zum Erstellen eines Diensts, die von anderen Diensten genutzt werden, kann die `NsdServiceInfo` Klasse wird verwendet, um ein Objekt zu erstellen, die die Eigenschaften eines Diensts definieren. Dieses Objekt wird dann bereitgestellt, um `NsdManager.RegisterService()` zusammen mit einer Implementierung von `NsdManager.ResolveListener`. Implementierungen von `NsdManager.ResolveListener` werden verwendet, um über eine erfolgreiche Registrierung zu benachrichtigen und zum Aufheben der dienstregistrierung.

Ermittlung für das Netzwerk und die Implementierung der Dienste `Nsd.DiscoveryListener` übergeben `NsdManager.discoverServices()`.

 <a name="Network_Usage" />


#### <a name="network-usage"></a>Analysieren der Netzwerkverwendung

Eine neue Methode `ConnectivityManager.IsActiveNetworkMetered` ermöglicht einem Gerät überprüft, ob er mit einer getakteten Netzwerken verbunden ist. Diese Methode kann zum Verwalten von Datennutzung durch Benutzer genau darüber informiert, dass teure Gebühren für Data-Vorgänge können verwendet werden.

 <a name="WiFi_Direct_Service_Discovery" />


#### <a name="wifi-direct-service-discovery"></a>WiFi Direct-Dienstermittlung

Die `WifiP2pManager` Klasse seit Android 4.0 unterstützt *Zeroconf*. Zeroconf (0 Netzwerke Configuration) ist eine Reihe von Techniken, die Geräte (Computer, Drucker, Mobiltelefone) automatisch, eine Verbindung mit Netzwerken herstellen kann mit Eingreifen menschlichen Netzwerkbetreiber oder spezielle Konfigurationsserver.

In Gelee-Bean `WifiP2pManager` ermitteln können, Geräte, die entweder in der Nähe *Bonjour* oder *Upnp*. Bonjour handelt es sich um Apple Implementierung des Zeroconf. UPnP ist festgelegt, der Netzwerkprotokolle, die auch Zeroconf unterstützt. Die folgenden Methoden hinzugefügt, die `WiFiP2pManager` WLAN-Dienst-Ermittlung unterstützt:

-   `AddLocalService()` – Diese Methode wird verwendet, können Sie eine Anwendung als Dienst über Wi-Fi für die Ermittlung von Peers.
-   `AddServiceRequest(` ) – Diese Methode ist das Framework eine Dienst-Ermittlungsanfrage an. Es wird verwendet, die Wi-Fi-Dienstermittlung initialisiert werden.
-   `SetDnsSdResponseListeners()` – Diese Methode wird verwendet, um Rückrufe aufgerufen werden, auf den Erhalt einer Antwort auf Anforderungen zur Ermittlung von Bonjour zu registrieren.
-   `SetUpnpServiceResponseListener()` – Diese Methode wird zum Registrieren von Rückrufen, aufgerufen werden soll, auf den Erhalt einer Antwort auf Anforderungen zur Ermittlung Upnp verwendet.


 <a name="Content_Providers" />


### <a name="content-providers"></a>Inhaltsanbieter

Die `ContentResolver` Klasse hat eine neue Methode empfangen `AcquireUnstableContentProvider`. Diese Methode ermöglicht einer Anwendung, eine "instabil" Inhaltsanbieter abzurufen. Normalerweise stürzt ab, wenn eine Anwendung eine Inhaltsanbieter und diese Inhaltsanbieter bereitstellt, daher wird die Anwendung. Mit dieser Methodenaufruf wird eine Anwendung nicht stürzt ab, wenn Inhaltsanbieter stürzt ab. Stattdessen `Android.OS.DeadObjectionException` aus auf den Inhaltsanbieter aufrufen, um eine Anwendung zu informieren, die der Inhaltsanbieter verschwunden ist ausgelöst. Ein "instabil" Inhaltsanbieter ist nützlich, bei der Interaktion mit Inhaltsanbietern aus anderen Anwendungen – ist es weniger wahrscheinlich, dass dienstupgrade fehlerhaften Code aus einer anderen Anwendung einer anderen Anwendung auswirkt.

 <a name="Copy_and_Paste_With_Intents" />


### <a name="copy-and-paste-with-intents"></a>Kopieren und Einfügen mit Intents

Die `Intent` Klasse haben jetzt eine `ClipData` zugeordnetes Statusobjekt über die `Intent.ClipData` Eigenschaft. Diese Methode ermöglicht zusätzliche Daten aus der Zwischenablage mit der Absicht übermittelt werden sollen. Eine Instanz von `ClipData` darf eine oder mehrere `ClipData.Item`. `ClipData.Item`die Elemente der folgenden Typen sind:

-   **Text** – Dies ist eine beliebige Zeichenfolge entweder HTML-Text oder eine beliebige Zeichenfolge, deren Format, die Formatvorlage für die integrierte Android unterstützt wird, umfasst.
-  **Beabsichtigte** – beliebig `Intent` Objekt.
-   **URI** – Dies kann einen beliebigen URI, z. B. ein HTTP-Lesezeichen vorliegt oder der URI, der eine Inhaltsanbieter sein.


 <a name="Isolated_Services" />


### <a name="isolated-services"></a>Isolierte Services

Ein isolierter Dienst ist ein Dienst, der unter seinem eigenen speziellen Prozess ausgeführt wird und verfügt über keine Berechtigungen selbst. Der nur Kommunikation mit dem Dienst ist, wenn der Dienst gestartet, und Binden an ihn über die Dienst-API. Es ist möglich, einen Dienst als isolierte deklarieren, indem die Eigenschaft `IsolatedProcess="true"` in der `ServiceAttribute` , die eine Dienstklasse erweitert.

 <a name="Media" />


### <a name="media"></a>Medien

Die neue `Android.Media.MediaCodec` -Klasse stellt eine API zur Low-Level-Media-Codecs. Anwendungen können Abfragen, das System, um herauszufinden, welche low-Level Codecs auf dem Gerät verfügbar sind.

Die neue `Android.Media.Audiofx.AudioEffect` Unterklassen wurden hinzugefügt, um zusätzliche vorverarbeitung auf Audioaufnahme Audio unterstützen:

-   `Android.Media.Audiofx.AcousticEchoCanceler` – Diese Klasse wird für die vorverarbeitung Audio So entfernen das Signal aus einem Remoteanbieter aus einem Signal mit erfassten audio verwendet. Entfernen z. B. das Echo über eine Stimme Kommunikation Anwendung.
-   `Android.Media.Audiofx.AutomaticGainControl` – Diese Klasse wird verwendet, um zu normalisieren der erfasste Signal durch boosting oder ein Eingangssignal herabsetzen, sodass Ausgangssignal konstant ist.
-   `Android.Media.Audiofx.NoiseSuppressor` – Diese Klasse wird aus dem aufgezeichneten Signal Hintergrundgeräuschen entfernt.


Nicht alle Geräte unterstützen diese Effekte. Die Methode `AudioEffect.IsAvailable` aufgerufen werden, von einer Anwendung, um festzustellen, ob die betreffende audio Auswirkungen auf dem Gerät, das Ausführen der Anwendung unterstützt wird.

Die `MediaPlayer` -Klasse unterstützt nun die lückenloses Wiedergabe mit der `SetNextMediaPlayer()` Methode. Dieser neuen Methode gibt die nächste MediaPlayer zu starten, wenn das aktuelle Media-Player die Wiedergabe abgeschlossen ist.

Die folgenden neuen Klassen enthalten Standardmechanismen und die Benutzeroberfläche für die Auswahl, auf dem Medium wiedergegeben werden:

-   `MediaRouter` – Diese Klasse ermöglicht es Anwendungen, um das routing von Medien Kanäle über ein Gerät auf externe Lautsprecher oder anderen Geräten zu steuern.
-   `MediaRouterActionProvider` und `MediaRouteButton` – diese Klassen bieten eine konsistente Benutzeroberfläche für die Auswahl und Wiedergeben von Medien.


 <a name="Notifications" />


### <a name="notifications"></a>Benachrichtigungen

Android 4.1 ermöglicht Anwendungen, mehr Flexibilität und Kontrolle Anzeige von Benachrichtigungen. Anwendungen können nun größer und bessere Benachrichtigungen an Benutzer anzeigen. Eine neue Methode `NotificationBuilder.SetStyle()` ermöglicht, dass eine neue drei neue Formatvorlage für Benachrichtigungen festgelegt werden:

-   `Notification.BigPictureStyle` – Dies ist eine Hilfsklasse, die Benachrichtigungen generiert, die ein Bild enthalten wird. Die folgende Abbildung zeigt ein Beispiel für eine Benachrichtigung über ein großes Bild:


 [ ![Beispiel-Screenshot einer BigPictureStyle-Benachrichtigung](jelly-bean-images/image2.png)](jelly-bean-images/image2.png)

-   `Notification.BigTextStyle` – Dies ist eine Hilfsklasse, die Benachrichtigungen generiert, die mehrere Textzeilen, z. B. E-mail hat. Ein Beispiel für dieses neue Benachrichtigung-Format kann im folgenden Screenshot angezeigt werden:


 [ ![Beispiel-Screenshot einer BigTextStyle-Benachrichtigung](jelly-bean-images/image3.png)](jelly-bean-images/image3.png)

-   `Notification.InboxStyle` – Dies ist eine Hilfsklasse, die Benachrichtigungen, die eine Liste von Zeichenfolgen, z. B. Ausschnitte aus einer e-Mail-Nachricht enthalten generieren, wie in diesem Screenshot gezeigt:


 [ ![Beispiel-Screenshot einer Notification.InboxStyle-Benachrichtigung](jelly-bean-images/image4.png)](jelly-bean-images/image4.png)

Es kann bis zu zwei Aktionsschaltflächen am unteren Rand eine Meldung hinzugefügt werden, wenn die Benachrichtigung normalen oder größere-Stil verwendet wird.
Ein Beispiel hierfür werden im folgenden Screenshot sehen, in denen die Aktionsschaltflächen am unteren Rand der Benachrichtigung angezeigt werden:

 [ ![Beispiel-Screenshot des Aktionsschaltflächen unterhalb eine Meldung angezeigt wird](jelly-bean-images/image5.png)](jelly-bean-images/image5.png)

Die `Notification` Klasse neue Konstanten, die Ihnen Entwickler an einen der fünf Prioritätsstufen für eine Benachrichtigung können empfangen hat. Diese können festgelegt werden, auf eine Benachrichtigung mit der `Priority` Eigenschaft.

 <a name="Permissions" />


### <a name="permissions"></a>Berechtigungen

Die folgenden neuen Berechtigungen wurden hinzugefügt:

-   `READ_EXTERNAL_STORAGE` – Die Anwendung erfordert schreibgeschützten Zugriff auf ein externes Speichergerät. Alle Anwendungen weisen derzeit Lesezugriff standardmäßig, jedoch zukünftige Versionen von Android erfordern, dass Anwendungen Lesezugriff explizit anfordern.
-   `READ_USER_DICTIONARY` – Ermöglicht es einer Lesezugriff auf die Benutzer-Wörterbuch.
-   `READ_CALL_LOG` – Ermöglicht es eine Anwendung zum Abrufen von Informationen zu eingehenden und ausgehenden Aufrufen durch Lesen des Protokolls Aufruf.
-   `WRITE_CALL_LOG` – Ermöglicht es eine Anwendung beim Schreiben in den Aufrufen auf dem Telefon.
-   `WRITE_USER_DICTIONARY` – Ermöglicht es eine Anwendung zum Schreiben in Word-Wörterbuch des Benutzers.


Eine wichtige Änderung beachten `READ_EXTERNAL_STORAGE` – derzeit diese Berechtigung von Android automatisch erteilt wird. Zukünftige Versionen von Android benötigen eine Anwendung diese Berechtigung vor angefordert werden, die Berechtigung erteilt.

 <a name="Summary" />


## <a name="summary"></a>Zusammenfassung

In diesem Artikel eingeführt wurden einige der neuen APIs, die in Android 4.1 (API Level 16) verfügbar sind. Er einige der Änderungen für Animationen und Animieren des Start einer Aktivität markiert und das neue APIs für die Netzwerkermittlung der anderen über Protokolle wie Bonjour oder UPnP-Geräte eingeführt. Andere Änderungen an der API wurden ebenfalls hervorgehoben, z. B. die Fähigkeit zum Ausschneiden und Einfügen von Daten über Intents, die Fähigkeit, isolierte Dienste oder "instabil" Inhaltsanbieter verwenden.

In diesem Artikel dann anonymousMethod auf Benachrichtigungen die Updates vorgestellt und erläutert einige der neuen Berechtigungen, die mit Android 4.1 eingeführt wurden


## <a name="related-links"></a>Verwandte Links

- [Animation Uhrzeitbeispiel (Beispiel)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/TimeAnimatorExample/)
- [Android 4.1 APIs](http://developer.android.com/about/versions/android-4.1.html)
- [Aufgaben und Back-Stapel](http://developer.android.com/guide/components/tasks-and-back-stack.html)
- [Navigation mit zurück und nach-oben](http://developer.android.com/design/patterns/navigation.html)
