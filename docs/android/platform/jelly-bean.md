---
title: Jelly Bean-Features
description: 'Dieses Dokument bietet einen Überblick über die neuen Features für Entwickler, die in Android 4.1 eingeführt wurden. Zu diesen Features gehören: erweiterte Benachrichtigungen, Updates für Android Beam für die Freigabe großer Dateien, Updates von Multimedia, Peer-to-Peer-Netzwerkermittlung, Animationen, neue Berechtigungen.'
ms.prod: xamarin
ms.assetid: 23F57634-2EF9-5C15-C710-B3E19A5AF7E1
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: a638ccf7810c737faaeded7fcc98fcf657c85288
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027208"
---
# <a name="jelly-bean-features"></a>Jelly Bean-Features

_Dieses Dokument bietet einen Überblick über die neuen Features für Entwickler, die in Android 4.1 eingeführt wurden. Zu diesen Features gehören: erweiterte Benachrichtigungen, Updates für Android Beam für die Freigabe großer Dateien, Updates von Multimedia, Peer-to-Peer-Netzwerkermittlung, Animationen, neue Berechtigungen._

## <a name="overview"></a>Übersicht

Android 4.1 (API-Ebene 16), auch bekannt als „Jelly Bean“, ist seit dem 9. Juli 2012 verfügbar. Dieser Artikel bietet Entwicklern, die Xamarin.Android verwenden, eine allgemeine Einführung in einige der neuen Features in Android 4.1. Einige dieser neu eingeführten Features sind verbesserte Animationen für den Start einer Aktivität, neue Kameratöne und eine verbesserte Unterstützung für die Navigation im Anwendungsstapel. Ab sofort können Ausschneide- und Einfügevorgänge mit Absichten vorgenommen werden.

Für eine verbesserte Stabilität von Android-Anwendungen ist es nun möglich, eine Trennung von instabilen Inhaltsanbietern vorzunehmen. Dienste können auch isoliert werden, sodass sie nur durch die Aktivität zugänglich sind, durch die sie gestartet wurden.

Zudem wird ab sofort die Erkennung von Netzwerkdiensten mit Bonjour-, UPnP- oder Multicast-DNS-basierten Diensten unterstützt. Es ist jetzt möglich, umfangreichere Benachrichtigungen mit formatiertem Text, Aktionsschaltflächen und großen Bildern zu erstellen.

Schließlich wurden in Android 4.1 mehrere neue Berechtigungen hinzugefügt.

## <a name="requirements"></a>Anforderungen

Um Xamarin.Android-Anwendungen für Jelly Bean zu entwickeln, ist die Installation von Xamarin.Android 4.2.6 oder höher und Android 4.1 (API-Ebene 16) über den Android-SDK-Manager erforderlich, wie im folgenden Screenshot gezeigt:

[![Auswählen von Android 4.1 im Android-SDK-Manager](jelly-bean-images/image1.png)](jelly-bean-images/image1.png#lightbox)

## <a name="whats-new"></a>Neues

### <a name="animations"></a>Animationen

Die Aktivitäten können entweder über Zoomanimationen oder über benutzerdefinierte Animationen unter Verwendung der Klasse `ActivityOptions` gestartet werden. Die folgenden neuen Methoden werden zur Unterstützung dieser Animationen bereitgestellt:

- `MakeScaleUpAnimation` – Hierdurch wird eine Animation erstellt, die ein Aktivitätsfenster von einer Startposition und -größe auf dem Bildschirm zentral hochskaliert.
- `MakeThumbnailScaleUpAnimation` – Hierdurch wird eine Animation erstellt, die von einer Miniaturansicht von der angegebenen Position auf dem Bildschirm zentral hochskaliert.
- `MakeCustomAnimation` – Dies erstellt eine Animation aus Ressourcen in der Anwendung. Es gibt eine Animation für das Öffnen der Aktivität und eine weitere für das Beenden der Aktivität.

Die neue Klasse `TimeAnimator` bietet eine Schnittstelle `TimeAnimator.ITimeListener`, die eine Anwendung bei jeder Änderung eines Frames in einer Animation benachrichtigen kann. Betrachten Sie zum Beispiel die folgende Implementierung von `TimeAnimator.ITimeListener`:

```csharp
class MyTimeListener : Java.Lang.Object,  TimeAnimator.ITimeListener
{
    public void OnTimeUpdate(TimeAnimator animation, long totalTime, long deltaTime)
    {
        Log.Debug("Activity1", "totalTime={0}, deltaTime={1}", totalTime, deltaTime);
    }
}
```

Und jetzt wird zur Verwendung der Klasse eine Instanz von `TimeAnimator` erstellt, und der Listener wird festgelegt:

```csharp
var animator = new TimeAnimator();
animator.SetTimeListener(new MyTimeListener());
animator.Start();
```

Während die `TimeAnimator`-Instanz läuft, ruft sie `ITimeAnimator.ITimeListener` auf. Dadurch wird protokolliert, wie lange die Animation ausgeführt wird und wie viel Zeit seit dem letzten Aufruf der Methode vergangen ist.

### <a name="application-stack-navigation"></a>Navigation im Anwendungsstapel

Android 4.1 verbessert die mit Android 3.0 eingeführte Anwendungsstapelnavigation. Durch die Angabe der `ParentName`-Eigenschaft von `ActivityAttribute` kann Android die richtige übergeordnete Aktivität öffnen, wenn der Benutzer die [Nach-oben-Schaltfläche](https://developer.android.com/design/patterns/navigation.html#up-vs-back) in der Aktionsleiste drückt. Android wird dann die durch die `ParentName`-Eigenschaft angegebene Aktivität instanziieren. Dies ermöglicht es Anwendungen, die Hierarchie von Aktivitäten beizubehalten, die eine bestimmte Task bilden.

Für die meisten Anwendungen reicht die Einstellung des `ParentName`-Elements für die Aktivität aus, damit Android das richtige Verhalten für die Navigation im Anwendungsstapel bereitstellt; Android synthetisiert den notwendigen Back-Stapel, indem eine Reihe von Absichten für jede übergeordnete Aktivität erstellt wird. Da es sich hierbei jedoch um einen künstlichen Anwendungsstapel handelt, hat keine synthetische Aktivität einen gespeicherten Zustand, den eine natürliche Aktivität aufweisen würde. Damit eine synthetische übergeordnete Aktivität einen gespeicherten Zustand erhält, kann eine Aktivität die `OnPrepareNavigationUpTaskStack`-Methode außer Kraft setzen. Diese Methode empfängt eine `TaskStackBuilder`-Instanz, die eine Sammlung von Intent-Objekten enthält, die von Android zum Erstellen des Back-Stapels verwendet werden. Die Aktivität kann diese Absichten modifizieren, sodass sie, wenn die synthetische Aktivität erstellt wird, die richtigen Zustandsinformationen erhält.

Für komplexere Szenarios gibt es neue Methoden in der Activity-Klasse, mit denen das Verhalten der Nach-oben-Navigation und der Aufbau des Back-Stapels gehandhabt werden kann:

- `OnNavigateUp` – Durch Außerkraftsetzung dieser Methode ist es möglich, eine benutzerdefinierte Aktion auszuführen, wenn die **Nach-oben-Schaltfläche** gedrückt wird.
- `NavigateUpTo` – Durch den Aufruf dieser Methode navigiert die Anwendung von der aktuellen Aktivität zu der durch eine bestimmte Absicht festgelegten Aktivität.
- `ParentActivityIntent` – Dadurch wird eine Absicht erhalten, die die übergeordnete Aktivität der aktuellen Aktivität startet.
- `ShouldUpRecreateTask` – Mit dieser Methode wird abgefragt, ob der synthetische Back-Stapel erstellt werden muss, um zu einer übergeordneten Aktivität zu navigieren. Gibt `true` zurück, wenn der synthetische Stapel erstellt werden muss. 
- `FinishAffinity` – Durch den Aufruf dieser Methode werden die aktuelle Aktivität und alle untergeordneten Aktivitäten in der aktuellen Task beendet, die die gleiche Taskaffinität aufweisen.
- `OnCreateNavigateUpTaskStack` – Diese Methode wird außer Kraft gesetzt, wenn die vollständige Kontrolle über die Erstellung des synthetischen Stapels erforderlich ist.

### <a name="camera"></a>Kamera

Es gibt eine neue Schnittstelle, `Camera.IAutoFocusMoveCallback`, mit der erkannt werden kann, wann der Autofokus gestartet oder gestoppt wurde. Ein Beispiel für diese neue Schnittstelle finden Sie im folgenden Codeausschnitt:

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

Die neue Klasse `MediaActionSound` bietet eine Reihe von APIs zur Wiedergabe von Sounds für die verschiedenen Medienaktionen. Es gibt mehrere Aktionen für eine Kamera. Diese werden durch die Enumeration `Android.Media.MediaActionSoundType` definiert:

- `MediaActionSoundType.FocusComplete` – Dieser Sound wird nach Abschluss der Fokussierung abgespielt.
- `MediaActionSoundType.ShutterClick` – Dieser Sound wird bei der Aufnahme eines Standbilds abgespielt.
- `MediaActionSoundType.StartVideoRecording` – Dieser Sound signalisiert den Beginn der Videoaufzeichnung.
- `MediaActionSoundType.StopVideoRecording` – Dieser Sound signalisiert das Ende der Videoaufzeichnung.

Ein Beispiel für die Verwendung der `MediaActionSound`-Klasse finden Sie in folgendem Codeausschnitt:

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

Android Beam ist eine NFC-basierte Technologie, durch die zwei Android-Geräte miteinander kommunizieren können. Android 4.1 bietet eine bessere Unterstützung für die Übertragung großer Dateien. Bei Verwendung der neuen Methode `NfcAdapter.SetBeamPushUris()` schaltet Android zwischen alternativen Transportmechanismen (wie z. B. Bluetooth) um, um eine schnelle Übertragungsgeschwindigkeit zu erreichen.

#### <a name="network-services-discovery"></a>Erkennung von Netzwerkdiensten

Android 4.1 enthält neue APIs für die Erkennung von Multicast-DNS-basierten Diensten.
Dadurch kann eine Anwendung über WLAN eine Verbindung mit anderen Geräten wie Drucker, Kameras und Mediengeräten herstellen. Diese neuen APIs sind im `Android.Net.Nsd`-Paket enthalten.

Um einen Dienst zu erstellen, der möglicherweise von anderen Diensten verwendet wird, wird mithilfe der `NsdServiceInfo`-Klasse ein Objekt erstellt, das die Eigenschaften eines Diensts definiert. Dieses Objekt wird dann für `NsdManager.RegisterService()` zusammen mit einer Implementierung von `NsdManager.ResolveListener` bereitgestellt. Implementierungen von `NsdManager.ResolveListener` werden zur Benachrichtigung über eine erfolgreiche Registrierung und zur Aufhebung der Registrierung des Diensts verwendet.

Um Dienste im Netzwerk zu ermitteln und die Implementierung von `Nsd.DiscoveryListener` an `NsdManager.discoverServices()` zu übergeben.

#### <a name="network-usage"></a>Analysieren der Netzwerkverwendung

Mithilfe der neuen Methode `ConnectivityManager.IsActiveNetworkMetered` kann ein Gerät prüfen, ob es mit einem Netzwerk verbunden ist, für das Verbrauchseinheiten gemessen werden. Diese Methode unterstützt die Steuerung der Datennutzung, indem die Benutzer genau darüber informiert werden, dass für Datenübertragungen hohe Gebühren anfallen können.

#### <a name="wifi-direct-service-discovery"></a>Erkennung des Wi-Fi Direct-Diensts

Die Klasse `WifiP2pManager` wurde in Android 4.0 zur Unterstützung von *zeroconf* eingeführt. Zeroconf (Zero Configuration Networking) umfasst verschiedene Methoden, die es Geräten (Computern, Druckern, Telefonen) ermöglichen, sich automatisch mit Netzwerken zu verbinden, wobei menschliche Netzwerkbetreiber oder spezielle Konfigurationsserver eingreifen.

In Jelly Bean kann `WifiP2pManager` Geräte in der Nähe entweder mit *Bonjour* oder *UPnP* erkennen. Bonjour ist die zeroconf-Implementierung von Apple. UPnP umfasst mehrere Netzwerkprotokolle und unterstützt ebenfalls zeroconf. Die folgenden Methoden wurden `WiFiP2pManager` hinzugefügt, um die Erkennung des WLAN-Diensts zu unterstützen:

- `AddLocalService()` – Diese Methode wird verwendet, um eine Anwendung als Dienst über WLAN für die Entdeckung durch Peers anzukündigen.
- `AddServiceRequest(` – Diese Methode dient dazu, eine Anforderung zur Diensterkennung an das Framework zu senden. Sie wird zur Initialisierung der Erkennung des WLAN-Diensts verwendet.
- `SetDnsSdResponseListeners()` – Diese Methode wird verwendet, um Rückrufe zu registrieren, die beim Empfangen einer Antwort auf Erkennungsanforderungen von Bonjour aufgerufen werden sollen.
- `SetUpnpServiceResponseListener()` – Diese Methode wird verwendet, um Rückrufe zu registrieren, die beim Empfangen einer Antwort auf Erkennungsanforderungen von UPnP aufgerufen werden sollen.

### <a name="content-providers"></a>Inhaltsanbieter

Die Klasse `ContentResolver` hat eine neue Methode empfangen: `AcquireUnstableContentProvider`. Diese Methode ermöglicht es einer Anwendung, einen „instabilen“ Inhaltsanbieter abzurufen. Wenn eine Anwendung einen Inhaltsanbieter abruft und dieser Inhaltsanbieter abstürzt, stürzt in der Regel auch die Anwendung ab. Mit diesem Methodenaufruf stürzt eine Anwendung nicht ab, wenn der Inhaltsanbieter abstürzt. Stattdessen wird `Android.OS.DeadObjectionException` von Aufrufen an den Inhaltsanbieter ausgelöst, um eine Anwendung darüber zu informieren, dass der Inhaltsanbieter entfernt wurde. Ein „instabiler“ Inhaltsanbieter ist nützlich, wenn er mit Inhaltsanbietern von anderen Anwendungen interagiert – es ist weniger wahrscheinlich, dass sich fehlerhafter Code aus einer anderen Anwendung auf eine andere Anwendung auswirkt.

### <a name="copy-and-paste-with-intents"></a>Kopieren und Einfügen mit Absichten

Der `Intent`-Klasse kann jetzt über die `Intent.ClipData`-Eigenschaft ein `ClipData`-Objekt zugeordnet werden. Diese Methode ermöglicht es, zusätzliche Daten aus der Zwischenablage mit der Absicht zu übertragen. Eine Instanz von `ClipData` kann mindestens ein `ClipData.Item`-Element enthalten. `ClipData.Item`-Elemente gehören zu folgenden Typen:

- **Text** – Dabei handelt es sich um einen beliebigen Text, entweder HTML oder eine beliebige Zeichenfolge, deren Format von den integrierten Android-Formatierungselementen unterstützt wird.
- **Intent** – Jedes `Intent`-Objekt.
- **Uri** – Dies kann jeder URI wie ein HTTP-Lesezeichen oder der URI für einen Inhaltsanbieter sein.

### <a name="isolated-services"></a>Isolierte Dienste

Ein isolierter Dienst ist ein Dienst, der unter einem eigenen gesonderten Prozess ausgeführt wird und keine eigenen Berechtigungen hat. Die einzige Kommunikation mit dem Dienst erfolgt beim Starten des Diensts und der Bindung an diesen über die Dienst-API. Es ist möglich, einen Dienst als isoliert zu deklarieren, indem die Eigenschaft `IsolatedProcess="true"` in der `ServiceAttribute`-Klasse, die eine Dienstklasse kennzeichnet, festgelegt wird.

### <a name="media"></a>Medien

Die neue Klasse `Android.Media.MediaCodec` bietet eine API für Low-Level-Mediencodecs. Das System kann von Anwendungen abgefragt werden, um herauszufinden, welche Low-Level-Codecs auf dem Gerät verfügbar sind.

Die neuen `Android.Media.Audiofx.AudioEffect`-Unterklassen wurden hinzugefügt, um eine zusätzliche Audiovorverarbeitung für erfasste Audiodaten zu unterstützen:

- `Android.Media.Audiofx.AcousticEchoCanceler` – Diese Klasse wird für die Vorverarbeitung von Audiodaten verwendet, um das Signal von einer Remotepartei aus einem erfassten Audiosignal zu entfernen. Ein Beispiel ist das Entfernen des Echos aus einer Sprachkommunikationsanwendung.
- `Android.Media.Audiofx.AutomaticGainControl` – Diese Klasse wird verwendet, um das erfasste Signal zu normalisieren, indem ein Eingangssignal so verstärkt oder verringert wird, dass das Ausgangssignal konstant ist.
- `Android.Media.Audiofx.NoiseSuppressor` – Diese Klasse entfernt Hintergrundgeräusche aus dem erfassten Signal.

Diese Effekte werden nicht von allen Geräten unterstützt. Die Methode `AudioEffect.IsAvailable` sollte von einer Anwendung aufgerufen werden, um zu prüfen, ob der betreffende Audioeffekt auf dem Gerät, auf dem die Anwendung läuft, unterstützt wird.

Die Klasse `MediaPlayer` unterstützt jetzt die lückenlose Wiedergabe mit der `SetNextMediaPlayer()`-Methode. Diese neue Methode legt den nächsten zu startenden MediaPlayer fest, wenn der aktuelle MediaPlayer seine Wiedergabe beendet.

Die folgenden neuen Klassen bieten Standardmechanismen und eine Benutzeroberfläche um auszuwählen, wo die Medien abgespielt werden sollen:

- `MediaRouter` – Diese Klasse ermöglicht es Anwendungen, das Routing von Medienkanälen von einem Gerät zu externen Lautsprechern oder anderen Geräten zu steuern.
- `MediaRouterActionProvider` und `MediaRouteButton` – Mithilfe dieser Klassen wird eine einheitliche Benutzeroberfläche für die Auswahl und Wiedergabe von Medien bereitgestellt.

### <a name="notifications"></a>Benachrichtigungen

Android 4.1 bietet Anwendungen mehr Flexibilität und Kontrolle bei der Anzeige von Benachrichtigungen. Anwendungen können jetzt umfangreichere und genauere Benachrichtigungen an die Benutzer anzeigen. Die neue Methode `NotificationBuilder.SetStyle()` bietet die Möglichkeit, einen der drei neuen Formatvorlagen für Benachrichtigungen festzulegen:

- `Notification.BigPictureStyle` – Dies ist eine Hilfsklasse, die Benachrichtigungen generiert, die ein Bild enthalten. Das folgende Bild zeigt ein Beispiel für eine Benachrichtigung mit einem großen Bild:

 [![Screenshot mit Beispiel für eine BigPictureStyle-Benachrichtigung](jelly-bean-images/image2.png)](jelly-bean-images/image2.png#lightbox)

- `Notification.BigTextStyle` – Dies ist eine Hilfsklasse, die Benachrichtigungen generiert, die mehrere Textzeilen haben, wie z. B. E-Mails. Ein Beispiel für diese neue Formatvorlage für Benachrichtigungen ist in der folgenden Abbildung zu sehen:

 [![Screenshot mit Beispiel für eine BigTextStyle-Benachrichtigung](jelly-bean-images/image3.png)](jelly-bean-images/image3.png#lightbox)

- `Notification.InboxStyle` – Dies ist eine Hilfsklasse, die Benachrichtigungen generiert, die eine Liste von Zeichenfolgen enthalten, wie z. B. Ausschnitte aus einer E-Mail-Nachricht, wie in diesem Screenshot gezeigt:

 [![Screenshot mit Beispiel für eine Notification.InboxStyle-Benachrichtigung](jelly-bean-images/image4.png)](jelly-bean-images/image4.png#lightbox)

Es ist möglich, bis zu zwei Aktionsschaltflächen unter einer Benachrichtigungsmeldung hinzuzufügen, wenn für die Benachrichtigung die normale oder größere Formatvorlage verwendet wird.
Ein Beispiel hierfür finden Sie im folgenden Screenshot, in dem die Aktionsschaltflächen am Ende der Benachrichtigung zu sehen sind:

 [![Screenshot mit Beispiel für Aktionsschaltflächen, die unter einer Benachrichtigung angezeigt werden](jelly-bean-images/image5.png)](jelly-bean-images/image5.png#lightbox)

Die Klasse `Notification` verfügt nun über neue Konstanten, die es einem Entwickler ermöglichen, eine von fünf Prioritätsstufen für eine Benachrichtigung festzulegen. Diese können für eine Benachrichtigung mit der `Priority`-Eigenschaft festgelegt werden.

### <a name="permissions"></a>Berechtigungen

Die folgenden neuen Berechtigungen wurden hinzugefügt:

- `READ_EXTERNAL_STORAGE` – Die Anwendung erfordert Lesezugriff auf den externen Speicher. Derzeit haben alle Anwendungen standardmäßig Lesezugriff, aber in zukünftigen Versionen von Android werden Anwendungen den Lesezugriff explizit anfordern müssen.
- `READ_USER_DICTIONARY` – Ermöglicht Lesezugriff auf das Wortwörterbuch des Benutzers.
- `READ_CALL_LOG` – Ermöglicht es einer Anwendung, Informationen über eingehende und ausgehende Anrufe durch Lesen des Anrufprotokolls abzurufen.
- `WRITE_CALL_LOG` – Ermöglicht es einer Anwendung, in das Anrufprotokoll auf dem Telefon zu schreiben.
- `WRITE_USER_DICTIONARY` – Ermöglicht es einer Anwendung, in das Wörterbuch des Benutzers zu schreiben.

Eine wichtige Änderung zu `READ_EXTERNAL_STORAGE`: Derzeit wird diese Berechtigung von Android automatisch erteilt. In zukünftigen Versionen von Android ist es erforderlich, dass eine Anwendung diese Berechtigung anfordert, bevor die Berechtigung gewährt wird.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden einige der neuen APIs vorgestellt, die in Android 4.1 (API-Ebene 16) verfügbar sind. Es wurden einige Änderungen für Animationen und die Animation des Starts einer Aktivität hervorgehoben sowie die neuen APIs für die Netzwerkerkennung anderer Geräte mit Protokollen wie Bonjour oder UPnP beschrieben. Außerdem wurden weitere Änderungen an der API hervorgehoben, wie z. B. die Möglichkeit, Daten mithilfe von Absichten auszuschneiden und einzufügen oder isolierte Dienste bzw. „instabile“ Inhaltsanbieter zu nutzen.

Anschließend wurden in diesem Artikel die Aktualisierungen der Benachrichtigungen erläutert und einige der neuen Berechtigungen, die mit Android 4.1 eingeführt wurden, vorgestellt.

## <a name="related-links"></a>Verwandte Links

- [Time Animation Example (sample)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-timeanimatorexample) (Beispiel für Zeitanimation (Beispiel))
- [Android 4.1-APIs](https://developer.android.com/about/versions/android-4.1.html)
- [Tasks and Back Stacks](https://developer.android.com/guide/components/tasks-and-back-stack.html) (Tasks und Back-Stapel)
- [Navigation with Back and Up](https://developer.android.com/design/patterns/navigation.html) (Navigation mit „Back“ und „Up“)
