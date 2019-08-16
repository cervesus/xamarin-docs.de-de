---
title: Funktionen der Gelee
description: 'Dieses Dokument bietet einen Überblick über die neuen Features für Entwickler, die in Android 4,1 eingeführt wurden. Zu diesen Funktionen gehören: Erweiterte Benachrichtigungen, Updates für Android-Beam für die gemeinsame Nutzung großer Dateien, Aktualisierungen von Multimedia, Peer-to-Peer-Netzwerk Ermittlung, Animationen, neue Berechtigungen.'
ms.prod: xamarin
ms.assetid: 23F57634-2EF9-5C15-C710-B3E19A5AF7E1
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 614a0e3952db42d2587930b66bf71ce4c703d035
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69524075"
---
# <a name="jelly-bean-features"></a>Funktionen der Gelee

_Dieses Dokument bietet einen Überblick über die neuen Features für Entwickler, die in Android 4,1 eingeführt wurden. Zu diesen Funktionen gehören: Erweiterte Benachrichtigungen, Updates für Android-Beam für die gemeinsame Nutzung großer Dateien, Aktualisierungen von Multimedia, Peer-to-Peer-Netzwerk Ermittlung, Animationen, neue Berechtigungen._



## <a name="overview"></a>Übersicht

Android 4,1 (API-Ebene 16), auch bekannt als "Gelee Bean", war am 9. Juli 2012. Dieser Artikel bietet eine allgemeine Einführung in einige der neuen Features in Android 4,1 für Entwickler, die xamarin. Android verwenden. Einige dieser neuen Features sind Erweiterungen für Animationen zum Starten einer Aktivität, neue Sounds für eine Kamera und verbesserte Unterstützung für die Anwendungs Stapel Navigation. Es ist jetzt möglich, mit Intents Ausschneiden und einfügen.

Die Stabilität von Android-Anwendungen wird durch die Möglichkeit verbessert, die Abhängigkeit von instabilen Inhaltsanbietern zu isolieren. Dienste können auch isoliert werden, damit Sie nur von der Aktivität zugänglich sind, von der Sie gestartet wurden.

Es wurde Unterstützung für die Netzwerkdienst Ermittlung mithilfe von Bonjour, UPnP oder DNS-basierten Multicast Diensten hinzugefügt. Es ist jetzt möglich, umfangreichere Benachrichtigungen mit formatiertem Text, Aktions Schaltflächen und großen Bildern zu erhalten.

Schließlich wurden in Android 4,1 mehrere neue Berechtigungen hinzugefügt.



## <a name="requirements"></a>Anforderungen

Zum Entwickeln von xamarin. Android-Anwendungen mit Jelly Bean muss xamarin. Android 4.2.6 bis oder höher und Android 4,1 (API-Ebene 16) über den Android SDK Manager installiert werden, wie im folgenden Screenshot zu sehen:

[![Auswählen von Android 4,1 im Android SDK-Manager](jelly-bean-images/image1.png)](jelly-bean-images/image1.png#lightbox)



## <a name="whats-new"></a>Neues



### <a name="animations"></a>Animationen

Aktivitäten können mithilfe von Zoom-Animationen oder benutzerdefinierten Animationen mithilfe der `ActivityOptions` -Klasse gestartet werden. Die folgenden neuen Methoden werden zur Unterstützung dieser Animationen bereitgestellt:

- `MakeScaleUpAnimation`– Hierdurch wird eine Animation erstellt, die ein Aktivitäts Fenster von einer Startposition und-Größe auf dem Bildschirm zentral hochskaliert.
- `MakeThumbnailScaleUpAnimation`– Hierdurch wird eine Animation erstellt, die von einem Miniaturbild von der angegebenen Position auf dem Bildschirm hochskaliert wird.
- `MakeCustomAnimation`– Erstellt eine Animation aus Ressourcen in der Anwendung. Es gibt eine Animation für den Zeitpunkt, zu dem die Aktivität geöffnet wird, und einen weiteren für die Beendigung der Aktivität.


Die neue `TimeAnimator` -Klasse stellt eine `TimeAnimator.ITimeListener` Schnittstelle bereit, die eine Anwendung jedes Mal benachrichtigen kann, wenn ein Frame in einer Animation geändert wird. Beachten Sie beispielsweise die folgende Implementierung von `TimeAnimator.ITimeListener`:

```csharp
class MyTimeListener : Java.Lang.Object,  TimeAnimator.ITimeListener
{
    public void OnTimeUpdate(TimeAnimator animation, long totalTime, long deltaTime)
    {
        Log.Debug("Activity1", "totalTime={0}, deltaTime={1}", totalTime, deltaTime);
    }
}
```

Und nun die-Klasse verwenden, wird eine Instanz `TimeAnimator` von erstellt, und der Listener wird festgelegt:

```csharp
var animator = new TimeAnimator();
animator.SetTimeListener(new MyTimeListener());
animator.Start();
```

Wenn die `TimeAnimator` Instanz ausgeführt wird, `ITimeAnimator.ITimeListener`wird Sie aufgerufen. Anschließend wird die Zeit protokolliert, wie lange die Animation ausgeführt wurde und wie lange Sie war, als die Methode zuletzt aufgerufen wurde.



### <a name="application-stack-navigation"></a>Anwendungs Stapel Navigation

Android 4,1 verbessert die in Android 3,0 eingeführte Anwendungs Stapel Navigation. Wenn Sie die `ParentName` -Eigenschaft `ActivityAttribute`von angeben, kann Android die ordnungsgemäße übergeordnete Aktivität öffnen, wenn der Benutzer die nach- [oben-Taste](https://developer.android.com/design/patterns/navigation.html#up-vs-back) auf der Aktionsleiste drückt: Android instanziiert die Aktivität, die von der `ParentName` -Eigenschaft angegeben wird. Dies ermöglicht es Anwendungen, die Hierarchie von Aktivitäten beizubehalten, die eine bestimmte Aufgabe bilden.

Bei den meisten Anwendungen ist `ParentName` das Festlegen von auf die-Aktivität ausreichend Informationen für Android, um das richtige Verhalten für die Navigation im Anwendungs Stapel bereitzustellen. Android synthetisieren den erforderlichen BackStack, indem er für jede übergeordnete Aktivität eine Reihe von Intents erstellt. Da es sich hierbei jedoch um einen künstlichen Anwendungs Stapel handelt, hat jede synthetische Aktivität keinen gespeicherten Zustand, den eine natürliche Aktivität aufweisen würde. Um einen gespeicherten Zustand für eine synthetische übergeordnete Aktivität bereitzustellen, kann eine `OnPrepareNavigationUpTaskStack` Aktivität die-Methode überschreiben. Diese Methode empfängt eine `TaskStackBuilder` -Instanz, die eine Auflistung von Intent-Objekten enthält, die von Android zum Erstellen des backstacks verwendet werden. Diese Intents können von der-Aktivität geändert werden, damit beim Erstellen der synthetischen Aktivität die richtigen Zustandsinformationen empfangen werden.

Für komplexere Szenarios gibt es neue Methoden für die Activity-Klasse, die verwendet werden können, um das Verhalten der Navigations Navigation zu verarbeiten und den BackStack zu erstellen:

- `OnNavigateUp`– Durch Überschreiben dieser Methode ist es möglich, eine benutzerdefinierte Aktion auszuführen, wenn die nach- **oben** -Taste gedrückt wird.
- `NavigateUpTo`– Wenn diese Methode aufgerufen wird, navigiert die Anwendung von der aktuellen Aktivität zu der Aktivität, die von einer bestimmten Absicht angegeben wird.
- `ParentActivityIntent`– Wird verwendet, um eine Absicht zu erhalten, die die übergeordnete Aktivität der aktuellen Aktivität startet.
- `ShouldUpRecreateTask`– Diese Methode wird verwendet, um abzufragen, ob der synthetische BackStack erstellt werden muss, um zu einer übergeordneten Aktivität zu navigieren. Gibt `true` zurück, wenn der synthetische Stapel erstellt werden muss. 
- `FinishAffinity`– Wenn diese Methode aufgerufen wird, werden die aktuelle Aktivität und alle untergeordneten Aktivitäten in der aktuellen Aufgabe beendet, die dieselbe Aufgaben Affinität aufweisen.
- `OnCreateNavigateUpTaskStack`– Diese Methode wird überschrieben, wenn es erforderlich ist, die umfassende Kontrolle über die Erstellung des synthetischen Stapels zu haben.




### <a name="camera"></a>Camera

Es gibt eine neue Schnittstelle `Camera.IAutoFocusMoveCallback`,, die verwendet werden kann, um zu erkennen, wann der automatische Fokus gestartet oder angehalten wurde. Ein Beispiel für diese neue Schnittstelle finden Sie im folgenden Code Ausschnitt:

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

Die neue Klasse `MediaActionSound` stellt eine Reihe von APIs zum Erstellen von Sounds für die verschiedenen Medienaktionen bereit. Es gibt mehrere Aktionen, die mit einer Kamera auftreten können. Diese werden durch die `Android.Media.MediaActionSoundType`-Aufzählung definiert:

- `MediaActionSoundType.FocusComplete`– Dieser Sound, der wiedergegeben wird, wenn der Fokus abgeschlossen ist.
- `MediaActionSoundType.ShutterClick`– Dieser Sound wird abgespielt, wenn ein Bild Bild immer noch angezeigt wird.
- `MediaActionSoundType.StartVideoRecording`– Dieser Sound wird verwendet, um den Start der Videoaufzeichnung anzugeben.
- `MediaActionSoundType.StopVideoRecording`– Dieser Sound wird wiedergegeben, um das Ende der Videoaufzeichnung anzugeben.


Ein Beispiel für die Verwendung der `MediaActionSound` -Klasse finden Sie im folgenden Code Ausschnitt:

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



#### <a name="android-beam"></a>Android Beam

Android-Beam ist eine NFC-basierte Technologie, die es zwei Android-Geräten ermöglicht, miteinander zu kommunizieren. Android 4,1 bietet eine bessere Unterstützung für die Übertragung großer Dateien. Bei Verwendung der neuen Methode `NfcAdapter.SetBeamPushUris()` wechselt Android zwischen alternativen Transportmechanismen (z. b. Bluetooth), um eine schnelle Übertragungsgeschwindigkeit zu erzielen.



#### <a name="network-services-discovery"></a>Network Services Ermittlung

Android 4,1 enthält neue APIs für die DNS-basierte Multicast Dienst Ermittlung.
Dies ermöglicht es einer Anwendung, über WLAN eine Verbindung mit anderen Geräten wie z. b. Druckern, Kameras und Mediengeräten herzustellen. Diese neuen APIs befinden sich im `Android.Net.Nsd` Paket.

Um einen Dienst zu erstellen, der möglicherweise von anderen Diensten verwendet `NsdServiceInfo` wird, wird die-Klasse verwendet, um ein Objekt zu erstellen, das die Eigenschaften eines Diensts definiert. Dieses Objekt wird dann `NsdManager.RegisterService()` zusammen mit einer Implementierung von `NsdManager.ResolveListener`bereitgestellt. Implementierungen von `NsdManager.ResolveListener` werden verwendet, um Benachrichtigungen über eine erfolgreiche Registrierung und die Aufhebung der Registrierung des Dienstanbieter zu erhalten.

Zum Ermitteln von Diensten im Netzwerk und zur Implementierung von `Nsd.DiscoveryListener` , die `NsdManager.discoverServices()`an weitergegeben wurde.



#### <a name="network-usage"></a>Analysieren der Netzwerkverwendung

Mit einer neuen Methode `ConnectivityManager.IsActiveNetworkMetered` kann ein Gerät überprüfen, ob es mit einem gemessenen Netzwerk verbunden ist. Diese Methode kann verwendet werden, um die Datennutzung zu verwalten, indem Benutzer genau darüber informiert werden, dass es kostenpflichtige Kosten für Daten Vorgänge geben kann.



#### <a name="wifi-direct-service-discovery"></a>Direkte WiFi-Dienst Ermittlung

Die `WifiP2pManager` Klasse wurde in Android 4,0 eingeführt, um *zeroconf*zu unterstützen. Bei Nullen (null) handelt es sich um eine Reihe von Techniken, die es Geräten (Computern, Druckern, Smartphones) ermöglichen, automatisch eine Verbindung mit Netzwerken herzustellen, mit dem Eingreifen menschlicher Netzwerk Bediener oder spezieller Konfigurationsserver.

In "Gelee `WifiP2pManager` Bean" kann Geräte in der Nähe mithilfe von *Bonjour* oder *UPnP*ermitteln. Bonjour ist die Implementierung von zeroconf von Apple. UPnP ist ein Satz von Netzwerkprotokollen, der auch nuloconf unterstützt. Die folgenden Methoden wurden zur unter `WiFiP2pManager` Stützung der Wi-Fi-Dienst Ermittlung hinzugefügt:

- `AddLocalService()`– Diese Methode wird verwendet, um eine Anwendung als Dienst als Wi-Fi für die Ermittlung durch Peers anzukündigen.
- `AddServiceRequest(`) – Diese Methode besteht darin, eine Dienst Ermittlungs Anforderung an das Framework zu senden. Sie wird verwendet, um die Wi-Fi-Dienst Ermittlung zu initialisieren.
- `SetDnsSdResponseListeners()`– Diese Methode wird verwendet, um Rückrufe zu registrieren, die beim Empfangen einer Antwort auf Ermittlungsanforderungen von Bonjour aufgerufen werden.
- `SetUpnpServiceResponseListener()`– Diese Methode wird verwendet, um Rückrufe zu registrieren, die beim Empfangen einer Antwort auf Ermittlungsanforderungen UPnP aufgerufen werden.




### <a name="content-providers"></a>Inhaltsanbieter

Die `ContentResolver` Klasse hat eine neue Methode,, `AcquireUnstableContentProvider`empfangen. Diese Methode ermöglicht es einer Anwendung, einen "instabilen" Inhaltsanbieter zu erhalten. Wenn eine Anwendung einen Inhaltsanbieter übernimmt und der Inhaltsanbieter abstürzt, wird die Anwendung normalerweise von der Anwendung verwendet. Mit diesem Methoden aufrufstürzt eine Anwendung nicht ab, wenn der Inhaltsanbieter abstürzt. `Android.OS.DeadObjectionException` Stattdessen wird von Aufrufen des Inhalts Anbieters ausgelöst, um eine Anwendung darüber zu informieren, dass der Inhaltsanbieter entfernt wurde. Ein "instabiler" Inhaltsanbieter ist nützlich, wenn er mit Inhaltsanbietern von anderen Anwendungen interagiert – es ist weniger wahrscheinlich, dass sich fehlerhafter Code aus einer anderen Anwendung auf eine andere Anwendung auswirkt.



### <a name="copy-and-paste-with-intents"></a>Kopieren und Einfügen mit Intents

Der `Intent` -Klasse kann nun über `ClipData` die `Intent.ClipData` -Eigenschaft ein-Objekt zugeordnet werden. Diese Methode ermöglicht es, dass zusätzliche Daten aus der Zwischenablage mit der Absicht übertragen werden. Eine Instanz von `ClipData` kann eine oder mehrere `ClipData.Item`enthalten. `ClipData.Item`sind Elemente der folgenden Typen:

- **Text** – Dies ist eine beliebige Text Zeichenfolge, entweder HTML oder eine beliebige Zeichenfolge, deren Format von den integrierten Android-Stil spannen unterstützt wird.
- **Intent** – ein `Intent` beliebiges Objekt.
- **URI** – Dies kann ein beliebiger URI sein, z. b. ein HTTP-Lesezeichen oder der URI für einen Inhaltsanbieter.




### <a name="isolated-services"></a>Isolierte Dienste

Bei einem isolierten Dienst handelt es sich um einen Dienst, der unter einem eigenen speziellen Prozess ausgeführt wird und über keine eigenen Berechtigungen verfügt. Die einzige Kommunikation mit dem Dienst besteht darin, dass der Dienst gestartet und über die Dienst-API an ihn gebunden wird. Es ist möglich, einen Dienst als isoliert zu deklarieren, indem `IsolatedProcess="true"` die- `ServiceAttribute` Eigenschaft in der, die eine Dienstklasse ist, festgelegt wird.


### <a name="media"></a>Medien

Die neue `Android.Media.MediaCodec` Klasse stellt eine API für Medien Codecs auf niedriger Ebene bereit. Anwendungen können das System Abfragen, um herauszufinden, welche auf dem Gerät verfügbaren Codecs auf niedriger Ebene verfügbar sind.

Die neuen `Android.Media.Audiofx.AudioEffect` Unterklassen wurden hinzugefügt, um zusätzliche audiovorverarbeitung für erfasste Audiodaten zu unterstützen:

- `Android.Media.Audiofx.AcousticEchoCanceler`– Diese Klasse wird für die Vorverarbeitung von Audiodaten verwendet, um das Signal von einer Remote Partei aus einem erfassten Audiosignal zu entfernen. Entfernen Sie z. b. das Echo aus einer sprachkommunikationsanwendung.
- `Android.Media.Audiofx.AutomaticGainControl`– Diese Klasse wird verwendet, um das erfasste Signal zu normalisieren, indem ein Eingabe Signal erhöht oder gesenkt wird, sodass das Ausgabesignal konstant ist.
- `Android.Media.Audiofx.NoiseSuppressor`– Diese Klasse entfernt das Hintergrundrauschen aus dem erfassten Signal.


Diese Auswirkungen werden nicht von allen Geräten unterstützt. Die- `AudioEffect.IsAvailable` Methode sollte von einer Anwendung aufgerufen werden, um zu überprüfen, ob der betreffende Audioeffekt auf dem Gerät unterstützt wird, auf dem die Anwendung ausgeführt wird.

Die `MediaPlayer` -Klasse unterstützt nun die Wiedergabe lose `SetNextMediaPlayer()` Wiedergabe mit der-Methode. Diese neue Methode gibt den nächsten Media Player an, der gestartet werden soll, wenn die Wiedergabe des aktuellen Media Players abgeschlossen ist.

Die folgenden neuen Klassen stellen Standardmechanismen und die Benutzeroberfläche für die Auswahl der Wiedergabe Medien bereit:

- `MediaRouter`– Diese Klasse ermöglicht es Anwendungen, das Routing von Medienkanälen von einem Gerät an externe Referenten oder andere Geräte zu steuern.
- `MediaRouterActionProvider`und `MediaRouteButton` – diese Klassen helfen Ihnen, eine konsistente Benutzeroberfläche für die Auswahl und Wiedergabe von Medien bereitzustellen.




### <a name="notifications"></a>Benachrichtigungen

Mit Android 4,1 können Anwendungen mehr Flexibilität und Kontrolle beim Anzeigen von Benachrichtigungen erhalten. Anwendungen können Benutzern nun größere und bessere Benachrichtigungen anzeigen. Eine neue Methode `NotificationBuilder.SetStyle()` ermöglicht die Festlegung eines neuen drei neuen Stils für Benachrichtigungen:

- `Notification.BigPictureStyle`– Dies ist eine Hilfsklasse, mit der Benachrichtigungen generiert werden, die ein Bild enthalten. Die folgende Abbildung zeigt ein Beispiel für eine Benachrichtigung mit einem großen Bild:


 [![Beispiel-Screenshot einer bigpicturestyle-Benachrichtigung](jelly-bean-images/image2.png)](jelly-bean-images/image2.png#lightbox)

- `Notification.BigTextStyle`– Dies ist eine Hilfsklasse, mit der Benachrichtigungen generiert werden, die mehrere Textzeilen enthalten, z. b. e-Mail. Ein Beispiel für diesen neuen Benachrichtigungs Stil finden Sie im folgenden Screenshot:


 [![Beispiel-Screenshot einer bigtextstyle-Benachrichtigung](jelly-bean-images/image3.png)](jelly-bean-images/image3.png#lightbox)

- `Notification.InboxStyle`– Dies ist eine Hilfsklasse, mit der Benachrichtigungen generiert werden, die eine Liste von Zeichen folgen wie z. b. Code Ausschnitte aus einer e-Mail-Nachricht enthalten, wie in diesem Screenshot gezeigt:


 [![Beispiel-Screenshot einer Benachrichtigung. inboxstyle-Benachrichtigung](jelly-bean-images/image4.png)](jelly-bean-images/image4.png#lightbox)

Es ist möglich, bis zu zwei Aktions Schaltflächen am Ende einer Benachrichtigungs Meldung hinzuzufügen, wenn die Benachrichtigung den normalen oder größeren Stil verwendet.
Ein Beispiel hierfür finden Sie im folgenden Screenshot, in dem die Aktions Schaltflächen am Ende der Benachrichtigung sichtbar sind:

 [![Screenshot der Aktions Schaltflächen, die unter einer Benachrichtigungs Meldung angezeigt werden](jelly-bean-images/image5.png)](jelly-bean-images/image5.png#lightbox)

Die `Notification` Klasse hat neue Konstanten erhalten, mit denen ein Entwickler eine von fünf Prioritätsstufen für eine Benachrichtigung angeben kann. Diese können mithilfe der `Priority` -Eigenschaft für eine Benachrichtigung festgelegt werden.



### <a name="permissions"></a>Berechtigungen

Die folgenden neuen Berechtigungen wurden hinzugefügt:

- `READ_EXTERNAL_STORAGE`-Die Anwendung benötigt schreibgeschützten Zugriff auf den externen Speicher. Zurzeit verfügen alle Anwendungen standardmäßig über Lesezugriff, aber zukünftige Versionen von Android erfordern, dass Anwendungen explizit Lesezugriff anfordern.
- `READ_USER_DICTIONARY`-Ermöglicht einen Lesezugriff auf das Wörterbuch des Benutzers.
- `READ_CALL_LOG`-Ermöglicht einer Anwendung das Abrufen von Informationen über eingehende und ausgehende Aufrufe durchlesen des Aufruf Protokolls.
- `WRITE_CALL_LOG`: Ermöglicht einer Anwendung das Schreiben in das Anrufprotokoll auf dem Telefon.
- `WRITE_USER_DICTIONARY`-Ermöglicht es einer Anwendung, in das Wörterbuch des Benutzers zu schreiben.


Eine wichtige Änderung `READ_EXTERNAL_STORAGE` – derzeit wird diese Berechtigung von Android automatisch erteilt. In zukünftigen Versionen von Android ist es erforderlich, dass eine Anwendung diese Berechtigung anfordert, bevor die Berechtigung gewährt wird.



## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden einige der neuen APIs vorgestellt, die in Android 4,1 (API-Ebene 16) verfügbar sind. Es wurden einige Änderungen für Animationen hervorgehoben und das Starten einer Aktivität animiert. Außerdem wurden die neuen APIs für die Netzwerk Ermittlung anderer Geräte mithilfe von Protokollen wie Bonjour oder UPnP eingeführt. Es wurden auch andere Änderungen an der API hervorgehoben, wie z. b. die Möglichkeit zum Ausschneiden und Einfügen von Daten über Intents, die Möglichkeit, isolierte Dienste oder "instabile" Inhaltsanbieter zu verwenden.

In diesem Artikel wurden die Aktualisierungen für Benachrichtigungen vorgestellt und einige der neuen Berechtigungen erläutert, die mit Android 4,1 eingeführt wurden.


## <a name="related-links"></a>Verwandte Links

- [Beispiel für Zeit Animation (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-timeanimatorexample)
- [Android 4,1-APIs](https://developer.android.com/about/versions/android-4.1.html)
- [Tasks und backstacks](https://developer.android.com/guide/components/tasks-and-back-stack.html)
- [Navigation mit Sicherung](https://developer.android.com/design/patterns/navigation.html)
