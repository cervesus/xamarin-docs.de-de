---
title: CallKit in Xamarin.iOS
description: Dieser Artikel behandelt die neue CallKit-API, die Apple veröffentlicht in iOS 10 und wie Sie es in Xamarin.iOS VOIP-apps zu implementieren.
ms.prod: xamarin
ms.assetid: 738A142D-FFD2-4738-B3ED-57C273179848
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/15/2017
ms.openlocfilehash: bb70dac34847cf46bd06cc20b87df8ea5f72105a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50115184"
---
# <a name="callkit-in-xamarinios"></a>CallKit in Xamarin.iOS

Die neue CallKit-API unter iOS 10 bietet eine Möglichkeit für VOIP-apps Sie mit der iPhone-Benutzeroberfläche und integrieren und bieten eine vertraute Schnittstelle für den Endbenutzer auftreten. Mit dieser API Benutzer können und VOIP-Anrufe über des iOS-Geräts der Sperrbildschirm interagieren Kontakte anzeigen und Verwalten der Phone-app mit **Favoriten** und **zuletzt** Ansichten.

## <a name="about-callkit"></a>Informationen zu CallKit

Gemäß den Apple ist CallKit ein neues Framework, das 3. Voice über IP (VOIP) apps von Drittanbietern eine 1. unter iOS 10 Parteien erhöhen wird. Die CallKit-API ermöglicht die VOIP-apps Sie mit der iPhone-Benutzeroberfläche und integrieren und bieten eine vertraute Schnittstelle für den Endbenutzer auftreten. Genau wie die integrierte Phone-app ein Benutzers kann und VOIP-Anrufe über des iOS-Geräts der Sperrbildschirm interagieren Kontakte anzeigen und Verwalten der Phone-app mit **Favoriten** und **zuletzt** Ansichten.

Darüber hinaus bietet der CallKit-API die Fähigkeit zum Erstellen von App-Erweiterungen, die eine Telefonnummer einen Namen (Anrufer-ID) zuordnen kann, und erkennen Sie, dass das System, wenn eine Zahl sein muss (aufgerufen durch blockieren) blockiert.

### <a name="the-existing-voip-app-experience"></a>Die vorhandenen VOIP-app-Funktionalität

Ehe wir näher auf die neue CallKit-API und die Möglichkeiten, ein Blick auf die vom aktuellen Benutzer wahrnehmbare a 3rd Partei VOIP-app unter iOS 9 (und weniger) mit einer fiktiven VOIP-app, die MonkeyCall aufgerufen. MonkeyCall handelt es sich um eine einfache app, mit dem Benutzer das Senden und Empfangen von VOIP-Anrufe, die mit dem vorhandenen iOS-APIs.

Wenn der Benutzer einen eingehenden Anruf auf MonkeyCall erhält und eigene iPhones gesperrt ist, die Benachrichtigung empfangen wird, auf dem Sperrbildschirm ist derzeit von irgendeinem anderen Typ der Benachrichtigung (wie diejenigen aus den Nachrichten oder z. B. Mail-apps).

Wenn der Benutzer den Anruf zu beantworten, müssten sie schieben Sie die Benachrichtigung MonkeyCall öffnen Sie die app, und geben ihre Kennung (oder die Benutzer Touch ID) zum Entsperren des Telefons, bevor sie den Anruf entgegennehmen und die Konversation starten konnte.

Die benutzererfahrung ist genauso mühselig, wenn das Telefon entsperrt wird. In diesem Fall wird der eingehenden MonkeyCall-Aufruf als ein standard benachrichtigungsbanner angezeigt, das im vom oberen Rand des Bildschirms verschoben. Da die Benachrichtigung temporär ist, können sie leicht vergessen werden, indem der Benutzer gezwungen, entweder die Mitteilungszentrale zu öffnen, und suchen die bestimmte Benachrichtigung beantworten und dann aufrufen, oder suchen und starten Sie die MonkeyCall-app manuell.

### <a name="the-callkit-voip-app-experience"></a>Die CallKit VOIP-app-Funktionalität

Durch die Implementierung der neuen CallKit-APIs in der app MonkeyCall, kann die benutzererfahrung mit einer eingehenden VOIP-Anruf unter iOS 10 erheblich verbessert werden. Betrachten Sie beispielsweise den Benutzer, die einen VOIP-Anruf empfangen, wenn ihre Rufnummer oben gesperrt ist. Durch die Implementierung CallKit, wird der Aufruf auf die iPhone-Sperrbildschirm, angezeigt, genau wie wenn der Aufruf aus der integrierten Phone-app, Vollbildmodus, native Benutzeroberfläche und Standardfunktionen Wischen-zu-Antwort empfangen wurde.

In diesem Fall, wenn das iPhone entsperrt ist, wenn eine MonkeyCall VOIP-Anruf empfangen wird, der gleichen Vollbild-, native Benutzeroberfläche und Wischen-zu-Antwort und Tap-zu-ablehnen Standardfunktionen von den integrierten Phone-app angezeigt wird und MonkeyCall verfügt die Möglichkeit, einen benutzerdefinierten Klingelton Wiedergabe .

CallKit bietet zusätzlicher Funktionalität für MonkeyCall, sodass die VOIP-Aufrufe zur Interaktion mit anderen Arten von aufrufen, in den integrierten zuletzt verwendete Inhalte angezeigt werden und die Favoriten sind aufgeführt, um die integrierten Funktionen für nicht stören "und"-Block zu verwenden, starten Sie MonkeyCall Aufrufe von Siri und bietet die Möglichkeit für Benutzer MonkeyCall Aufrufe an Personen in die Kontakte-app zuweisen.

In den folgenden Abschnitten werden die Architektur CallKit behandelt, die eingehenden und ausgehenden Flows und Aufrufen der CallKit-API im Detail.

## <a name="the-callkit-architecture"></a>Die CallKit-Architektur

In iOS 10 hat Apple CallKit in allen Systemdiensten bedeuten übernommen, sodass Aufrufe, die auf CarPlay, z. B. den System-Benutzeroberfläche über CallKit bekannt sind. In im Beispiel unten angegeben werden, da MonkeyCall CallKit übernimmt, es ist bekannt, auf das System in die gleiche Weise wie diese integrierte Systemdienste und ruft alle die gleichen Funktionen:

[![](callkit-images/callkit01.png "CallKit Dienststapel")](callkit-images/callkit01.png#lightbox)

Nehmen Sie einen genaueren Blick auf die MonkeyCall-App aus dem Diagramm oben. Die app enthält ihr gesamter Code für die Kommunikation mit einem eigenen Netzwerk sowie eigene Benutzeroberflächen. In CallKit für die Kommunikation mit dem System verknüpft:

[![](callkit-images/callkit02.png "MonkeyCall App-Architektur")](callkit-images/callkit02.png#lightbox)

Es gibt zwei Hauptschnittstellen in CallKit, die die app verwendet:

- `CXProvider` -Dies ermöglicht die MonkeyCall-app, um das System des Out-of-Band-Benachrichtigungen zu informieren, die auftreten können.
- `CXCallController` – Ermöglicht die MonkeyCall-app für das System von lokalen Benutzeraktionen zu informieren.

### <a name="the-cxprovider"></a>Die CXProvider

Wie bereits erwähnt, `CXProvider` ermöglicht einer app aus, um das System des Out-of-Band-Benachrichtigungen zu informieren, die auftreten können. Hierbei handelt es sich um Benachrichtigung, die nicht aufgrund eines lokalen Benutzeraktionen auftreten, aber externe Ereignisse wie z. B. eingehende Anrufe zurückzuführen.

Eine app verwenden, sollten die `CXProvider` für Folgendes:

- Melden Sie einen eingehenden Anruf an das System an.
- Melden Sie ein ausgehender Aufruf mit dem System verbunden ist.
- Melden Sie den Remotebenutzer mit der Anruf an das System beendet.

Wenn möchte, dass die app für die Kommunikation mit dem System, verwendet es die `CXCallUpdate` Klasse, und wenn das System mit der app kommunizieren muss, verwendet es die `CXAction` Klasse:

[![](callkit-images/callkit03.png "Kommunikation mit dem System über ein CXProvider")](callkit-images/callkit03.png#lightbox)

### <a name="the-cxcallcontroller"></a>Die CXCallController

Die `CXCallController` ermöglicht einer app informiert das System des lokalen Benutzeraktionen wie z. B. der Benutzer einen VOIP-Anruf ab. Durch die Implementierung einer `CXCallController` die app Ruft bei anderen Arten von Aufrufe in das System aufgrund ab. Wenn es ist bereits ein aktive Telefonie-Aufruf ausgeführt wird, z. B. `CXCallController` können die VOIP-app zum Platzieren von diesem Aufruf anhalten und starten oder einen VOIP-Anruf zu beantworten.

Eine app verwenden, sollten die `CXCallController` für Folgendes:

- Bericht, wenn der Benutzer einen ausgehenden Anruf an das System gestartet wurde.
- Bericht, wenn der Benutzer einen eingehenden Anruf an das System antwortet.
- Bericht, wenn der Benutzer einen Aufruf an das System beendet.

Wenn die app lokaler Benutzeraktionen auf das System kommunizieren möchte, verwendet der `CXTransaction` Klasse:

[![](callkit-images/callkit04.png "Das System über ein CXCallController berichterstellung")](callkit-images/callkit04.png#lightbox)

## <a name="implementing-callkit"></a>Implementieren von CallKit

In den folgenden Abschnitten zeige, wie Sie CallKit in einer Xamarin.iOS-VOIP-app zu implementieren. Als Beispiel werden in diesem Dokument Code aus der fiktiven MonkeyCall VOIP-app verwenden. Der hier dargestellte Code stellt mehrere unterstützende Klassen dar, die CallKit bestimmte Teilen werden in den folgenden Abschnitten ausführlich behandelt.

### <a name="the-activecall-class"></a>Die ActiveCall-Klasse

Die `ActiveCall` Klasse wird von der app MonkeyCall verwendet, um alle Informationen über einen VOIP-Anruf zu speichern, die derzeit aktiv ist, wie folgt:

```csharp
using System;
using CoreFoundation;
using Foundation;

namespace MonkeyCall
{
    public class ActiveCall
    {
        #region Private Variables
        private bool isConnecting;
        private bool isConnected;
        private bool isOnhold;
        #endregion

        #region Computed Properties
        public NSUuid UUID { get; set; }
        public bool isOutgoing { get; set; }
        public string Handle { get; set; }
        public DateTime StartedConnectingOn { get; set;}
        public DateTime ConnectedOn { get; set;}
        public DateTime EndedOn { get; set; }

        public bool IsConnecting {
            get { return isConnecting; }
            set {
                isConnecting = value;
                if (isConnecting) StartedConnectingOn = DateTime.Now;
                RaiseStartingConnectionChanged ();
            }
        }

        public bool IsConnected {
            get { return isConnected; }
            set {
                isConnected = value;
                if (isConnected) {
                    ConnectedOn = DateTime.Now;
                } else {
                    EndedOn = DateTime.Now;
                }
                RaiseConnectedChanged ();
            }
        }

        public bool IsOnHold {
            get { return isOnhold; }
            set {
                isOnhold = value;
            }
        }
        #endregion

        #region Constructors
        public ActiveCall ()
        {
        }

        public ActiveCall (NSUuid uuid, string handle, bool outgoing)
        {
            // Initialize
            this.UUID = uuid;
            this.Handle = handle;
            this.isOutgoing = outgoing;
        }
        #endregion

        #region Public Methods
        public void StartCall (ActiveCallbackDelegate completionHandler)
        {
            // Simulate the call starting successfully
            completionHandler (true);

            // Simulate making a starting and completing a connection
            DispatchQueue.MainQueue.DispatchAfter (new DispatchTime(DispatchTime.Now, 3000), () => {
                // Note that the call is starting
                IsConnecting = true;

                // Simulate pause before connecting
                DispatchQueue.MainQueue.DispatchAfter (new DispatchTime (DispatchTime.Now, 1500), () => {
                    // Note that the call has connected
                    IsConnecting = false;
                    IsConnected = true;
                });
            });
        }

        public void AnswerCall (ActiveCallbackDelegate completionHandler)
        {
            // Simulate the call being answered
            IsConnected = true;
            completionHandler (true);
        }

        public void EndCall (ActiveCallbackDelegate completionHandler)
        {
            // Simulate the call ending
            IsConnected = false;
            completionHandler (true);
        }
        #endregion

        #region Events
        public delegate void ActiveCallbackDelegate (bool successful);
        public delegate void ActiveCallStateChangedDelegate (ActiveCall call);

        public event ActiveCallStateChangedDelegate StartingConnectionChanged;
        internal void RaiseStartingConnectionChanged ()
        {
            if (this.StartingConnectionChanged != null) this.StartingConnectionChanged (this);
        }

        public event ActiveCallStateChangedDelegate ConnectedChanged;
        internal void RaiseConnectedChanged ()
        {
            if (this.ConnectedChanged != null) this.ConnectedChanged (this);
        }
        #endregion
    }
}
```

`ActiveCall` enthält verschiedene Eigenschaften, die definieren, den Zustand eines Aufrufs sowie zwei Ereignisse, die bei der Aufruf der statusänderungen ausgelöst werden können. Da dies nur ein Beispiel ist, gibt es drei Methoden, die verwendet, um simulierte starten, beantworten, und Beenden einen Aufruf an.

### <a name="the-startcallrequest-class"></a>Die StartCallRequest-Klasse

Die `StartCallRequest` statische Klasse, stellt einige Hilfsmethoden, die beim Arbeiten mit einem ausgehenden Aufrufe verwendet werden:

```csharp
using System;
using Foundation;
using Intents;

namespace MonkeyCall
{
    public static class StartCallRequest
    {
        public static string URLScheme {
            get { return "monkeycall"; }
        }

        public static string ActivityType {
            get { return INIntentIdentifier.StartAudioCall.GetConstant ().ToString (); }
        }

        public static string CallHandleFromURL (NSUrl url)
        {
            // Is this a MonkeyCall handle?
            if (url.Scheme == URLScheme) {
                // Yes, return host
                return url.Host;
            } else {
                // Not handled
                return null;
            }
        }

        public static string CallHandleFromActivity (NSUserActivity activity)
        {
            // Is this a start call activity?
            if (activity.ActivityType == ActivityType) {
                // Yes, trap any errors
                try {
                    // Get first contact
                    var interaction = activity.GetInteraction ();
                    var startAudioCallIntent = interaction.Intent as INStartAudioCallIntent;
                    var contact = startAudioCallIntent.Contacts [0];

                    // Get the person handle
                    return contact.PersonHandle.Value;
                } catch {
                    // Error, report null
                    return null;
                }
            } else {
                // Not handled
                return null;
            }
        }
    }
}
```

Die `CallHandleFromURL` und `CallHandleFromActivity` Klassen im appdelegate-Element verwendet, um das Handle wenden Sie sich an die Person, die aufgerufen wird, in einen ausgehenden Anruf erhalten. Weitere Informationen finden Sie unter den [ausgehende Anrufe verarbeiten](#Handling-Outgoing-Calls) Abschnitt weiter unten.

### <a name="the-activecallmanager-class"></a>Die ActiveCallManager-Klasse

Die `ActiveCallManager` Klasse alle geöffneten Aufrufe in der app MonkeyCall behandelt.

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using CallKit;

namespace MonkeyCall
{
    public class ActiveCallManager
    {
        #region Private Variables
        private CXCallController CallController = new CXCallController ();
        #endregion

        #region Computed Properties
        public List<ActiveCall> Calls { get; set; }
        #endregion

        #region Constructors
        public ActiveCallManager ()
        {
            // Initialize
            this.Calls = new List<ActiveCall> ();
        }
        #endregion

        #region Private Methods
        private void SendTransactionRequest (CXTransaction transaction)
        {
            // Send request to call controller
            CallController.RequestTransaction (transaction, (error) => {
                // Was there an error?
                if (error == null) {
                    // No, report success
                    Console.WriteLine ("Transaction request sent successfully.");
                } else {
                    // Yes, report error
                    Console.WriteLine ("Error requesting transaction: {0}", error);
                }
            });
        }
        #endregion

        #region Public Methods
        public ActiveCall FindCall (NSUuid uuid)
        {
            // Scan for requested call
            foreach (ActiveCall call in Calls) {
                if (call.UUID.Equals(uuid)) return call;
            }

            // Not found
            return null;
        }

        public void StartCall (string contact)
        {
            // Build call action
            var handle = new CXHandle (CXHandleType.Generic, contact);
            var startCallAction = new CXStartCallAction (new NSUuid (), handle);

            // Create transaction
            var transaction = new CXTransaction (startCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }

        public void EndCall (ActiveCall call)
        {
            // Build action
            var endCallAction = new CXEndCallAction (call.UUID);

            // Create transaction
            var transaction = new CXTransaction (endCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }

        public void PlaceCallOnHold (ActiveCall call)
        {
            // Build action
            var holdCallAction = new CXSetHeldCallAction (call.UUID, true);

            // Create transaction
            var transaction = new CXTransaction (holdCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }

        public void RemoveCallFromOnHold (ActiveCall call)
        {
            // Build action
            var holdCallAction = new CXSetHeldCallAction (call.UUID, false);

            // Create transaction
            var transaction = new CXTransaction (holdCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }
        #endregion
    }
}
```

Erneut, da dies nur eine Simulation der `ActiveCallManager` nur verwaltet eine Auflistung von `ActiveCall` Objekte und verfügt über eine Routine für die Suche nach einem bestimmten Aufruf durch die `UUID` Eigenschaft. Darüber hinaus Methoden zum Starten, beenden, und Ändern des Zustands gesperrte einen ausgehenden Anruf. Weitere Informationen finden Sie unter den [ausgehende Anrufe verarbeiten](#Handling-Outgoing-Calls) Abschnitt weiter unten.

### <a name="the-providerdelegate-class"></a>Die ProviderDelegate-Klasse

Wie oben beschrieben ein `CXProvider` bietet bidirektionalen Kommunikation zwischen der app und das System für die Out-of-Band-Benachrichtigungen. Geben Sie einen benutzerdefinierten muss der Entwickler `CXProviderDelegate` und fügen Sie es auf die `CXProvider` für die app zum Behandeln von Ereignissen von Out-of-Band-CallKit. MonkeyCall verwendet die folgenden `CXProviderDelegate`:

```csharp
using System;
using Foundation;
using CallKit;
using UIKit;

namespace MonkeyCall
{
    public class ProviderDelegate : CXProviderDelegate
    {
        #region Computed Properties
        public ActiveCallManager CallManager { get; set;}
        public CXProviderConfiguration Configuration { get; set; }
        public CXProvider Provider { get; set; }
        #endregion

        #region Constructors
        public ProviderDelegate (ActiveCallManager callManager)
        {
            // Save connection to call manager
            CallManager = callManager;

            // Define handle types
            var handleTypes = new [] { (NSNumber)(int)CXHandleType.PhoneNumber };

            // Get Image Mask
            var maskImage = UIImage.FromFile ("telephone_receiver.png");

            // Setup the initial configurations
            Configuration = new CXProviderConfiguration ("MonkeyCall") {
                MaximumCallsPerCallGroup = 1,
                SupportedHandleTypes = new NSSet<NSNumber> (handleTypes),
                IconMaskImageData = maskImage.AsPNG(),
                RingtoneSound = "musicloop01.wav"
            };

            // Create a new provider
            Provider = new CXProvider (Configuration);

            // Attach this delegate
            Provider.SetDelegate (this, null);

        }
        #endregion

        #region Override Methods
        public override void DidReset (CXProvider provider)
        {
            // Remove all calls
            CallManager.Calls.Clear ();
        }

        public override void PerformStartCallAction (CXProvider provider, CXStartCallAction action)
        {
            // Create new call record
            var activeCall = new ActiveCall (action.CallUuid, action.CallHandle.Value, true);

            // Monitor state changes
            activeCall.StartingConnectionChanged += (call) => {
                if (call.isConnecting) {
                    // Inform system that the call is starting
                    Provider.ReportConnectingOutgoingCall (call.UUID, call.StartedConnectingOn.ToNsDate());
                }
            };

            activeCall.ConnectedChanged += (call) => {
                if (call.isConnected) {
                    // Inform system that the call has connected
                    provider.ReportConnectedOutgoingCall (call.UUID, call.ConnectedOn.ToNsDate ());
                }
            };

            // Start call
            activeCall.StartCall ((successful) => {
                // Was the call able to be started?
                if (successful) {
                    // Yes, inform the system
                    action.Fulfill ();

                    // Add call to manager
                    CallManager.Calls.Add (activeCall);
                } else {
                    // No, inform system
                    action.Fail ();
                }
            });
        }

        public override void PerformAnswerCallAction (CXProvider provider, CXAnswerCallAction action)
        {
            // Find requested call
            var call = CallManager.FindCall (action.CallUuid);

            // Found?
            if (call == null) {
                // No, inform system and exit
                action.Fail ();
                return;
            }

            // Attempt to answer call
            call.AnswerCall ((successful) => {
                // Was the call successfully answered?
                if (successful) {
                    // Yes, inform system
                    action.Fulfill ();
                } else {
                    // No, inform system
                    action.Fail ();
                }
            });
        }

        public override void PerformEndCallAction (CXProvider provider, CXEndCallAction action)
        {
            // Find requested call
            var call = CallManager.FindCall (action.CallUuid);

            // Found?
            if (call == null) {
                // No, inform system and exit
                action.Fail ();
                return;
            }

            // Attempt to answer call
            call.EndCall ((successful) => {
                // Was the call successfully answered?
                if (successful) {
                    // Remove call from manager's queue
                    CallManager.Calls.Remove (call);

                    // Yes, inform system
                    action.Fulfill ();
                } else {
                    // No, inform system
                    action.Fail ();
                }
            });
        }

        public override void PerformSetHeldCallAction (CXProvider provider, CXSetHeldCallAction action)
        {
            // Find requested call
            var call = CallManager.FindCall (action.CallUuid);

            // Found?
            if (call == null) {
                // No, inform system and exit
                action.Fail ();
                return;
            }

            // Update hold status
            call.isOnHold = action.OnHold;

            // Inform system of success
            action.Fulfill ();
        }

        public override void TimedOutPerformingAction (CXProvider provider, CXAction action)
        {
            // Inform user that the action has timed out
        }

        public override void DidActivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
        {
            // Start the calls audio session here
        }

        public override void DidDeactivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
        {
            // End the calls audio session and restart any non-call
            // related audio
        }
        #endregion

        #region Public Methods
        public void ReportIncomingCall (NSUuid uuid, string handle)
        {
            // Create update to describe the incoming call and caller
            var update = new CXCallUpdate ();
            update.RemoteHandle = new CXHandle (CXHandleType.Generic, handle);

            // Report incoming call to system
            Provider.ReportNewIncomingCall (uuid, update, (error) => {
                // Was the call accepted
                if (error == null) {
                    // Yes, report to call manager
                    CallManager.Calls.Add (new ActiveCall (uuid, handle, false));
                } else {
                    // Report error to user here
                    Console.WriteLine ("Error: {0}", error);
                }
            });
        }
        #endregion
    }
}
```

Wenn eine Instanz dieses Delegaten erstellt wurde, wird sie übergeben die `ActiveCallManager` , dazu verwendet werden, die eine beliebige Aktivität behandelt. Als Nächstes definiert die Typen von Handles (`CXHandleType`), die die `CXProvider` beantwortet:

```csharp
// Define handle types
var handleTypes = new [] { (NSNumber)(int)CXHandleType.PhoneNumber };
```

Und es Ruft die Maske, die auf das Symbol der app angewendet wird, wenn ein Aufruf ausgeführt wird:

```csharp
// Get Image Mask
var maskImage = UIImage.FromFile ("telephone_receiver.png");
```

Diese Werte erhalten gebündelt eine `CXProviderConfiguration` wird, wird zum Konfigurieren der `CXProvider`:

```csharp
// Setup the initial configurations
Configuration = new CXProviderConfiguration ("MonkeyCall") {
    MaximumCallsPerCallGroup = 1,
    SupportedHandleTypes = new NSSet<NSNumber> (handleTypes),
    IconMaskImageData = maskImage.AsPNG(),
    RingtoneSound = "musicloop01.wav"
};
```

Der Delegat erstellt dann eine neue `CXProvider` mit diesen Konfigurationen und fügt sich selbst zuzuweisen:

```csharp
// Create a new provider
Provider = new CXProvider (Configuration);

// Attach this delegate
Provider.SetDelegate (this, null);
```

Wenn CallKit verwenden zu können, die app nicht mehr erstellen und verarbeiten Sie eine eigene Audiodateien Sitzungen, stattdessen müssen sie zum Konfigurieren und verwenden eine audio-Sitzung, die das System zu erstellen und dafür zu behandeln. 

Würde dies eine echte app die `DidActivateAudioSession` Methode wird verwendet, um den Aufruf mit einem bereits konfigurierten starten `AVAudioSession` , die das System bereitgestellt:

```csharp
public override void DidActivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
{
    // Start the call's audio session here...
}
```

Zudem die `DidDeactivateAudioSession` Methode abzuschließen und die Verbindung mit dem System bereitgestellt, audio-Sitzung:

```csharp
public override void DidDeactivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
{
    // End the calls audio session and restart any non-call
    // releated audio
}
```

Der Rest des Codes wird in den Abschnitten ausführlich behandelt, die folgen.

### <a name="the-appdelegate-class"></a>Die AppDelegate-Klasse

MonkeyCall appdelegate-Element verwendet, um Instanzen von aufzunehmen der `ActiveCallManager` und `CXProviderDelegate` , die in der gesamten app verwendet werden:

```csharp
using Foundation;
using UIKit;
using Intents;
using System;

namespace MonkeyCall
{
    [Register ("AppDelegate")]
    public class AppDelegate : UIApplicationDelegate
    {
        #region Constructors
        public override UIWindow Window { get; set; }
        public ActiveCallManager CallManager { get; set; }
        public ProviderDelegate CallProviderDelegate { get; set; }
        #endregion

        #region Override Methods
        public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
        {
            // Initialize the call handlers
            CallManager = new ActiveCallManager ();
            CallProviderDelegate = new ProviderDelegate (CallManager);

            return true;
        }

        public override bool OpenUrl (UIApplication app, NSUrl url, NSDictionary options)
        {
            // Get handle from url
            var handle = StartCallRequest.CallHandleFromURL (url);

            // Found?
            if (handle == null) {
                // No, report to system
                Console.WriteLine ("Unable to get call handle from URL: {0}", url); 
                return false;
            } else {
                // Yes, start call and inform system
                CallManager.StartCall (handle);
                return true;
            }
        }

        public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
        {
            var handle = StartCallRequest.CallHandleFromActivity (userActivity);

            // Found?
            if (handle == null) {
                // No, report to system
                Console.WriteLine ("Unable to get call handle from User Activity: {0}", userActivity);
                return false;
            } else {
                // Yes, start call and inform system
                CallManager.StartCall (handle);
                return true;
            }
        }

        ...
        #endregion
    }
}
```

Die `OpenUrl` und `ContinueUserActivity` außer Kraft setzen, die Methoden werden verwendet, wenn die app einen ausgehenden Aufruf verarbeitet wird. Weitere Informationen finden Sie unter den [ausgehende Anrufe verarbeiten](#Handling-Outgoing-Calls) Abschnitt weiter unten.

## <a name="handling-incoming-calls"></a>Verarbeiten eingehende Anrufe

Es gibt mehrere Zustände und Prozesse, die ein eingehender VOIP-Anruf bei einem typischen Workflow mit eingehenden Aufruf wie z. B. durchlaufen kann:

- Informieren der Benutzer (und das System), ein eingehender Anruf vorhanden ist.
- Empfangen von Benachrichtigungen, wenn der Benutzer den Anruf zu beantworten möchte, und initialisieren den Aufruf mit der andere Benutzer.
- Informieren Sie das System und das Kommunikationsnetzwerk aus, wenn der Benutzer zum Beenden des aktuellen Anrufs möchte.

In den folgenden Abschnitten dauert eine genaue Betrachtung darüber, wie eine app CallKit verwenden kann, um den eingehenden Aufruf Workflow erneut mit der MonkeyCall VOIP-app als Beispiel Behandlung.

### <a name="informing-user-of-incoming-call"></a>Informieren Sie Benutzer der eingehenden Anruf

Wenn ein Remotebenutzer eine VOIP-Konversation mit dem lokalen Benutzer gestartet wurde, geschieht Folgendes:

[![](callkit-images/callkit05.png "Ein Remotebenutzer wurde eine VOIP-Unterhaltung gestartet werden.")](callkit-images/callkit05.png#lightbox)

1. Die app ruft einer Benachrichtigung aus dem Netzwerk für die Kommunikation ab, dass ein eingehender VOIP-Anruf vorhanden ist.
2. Die app verwendet die `CXProvider` zum Senden einer `CXCallUpdate` an das System des Aufrufs zu informieren.
3. Das System wird den Aufruf der System-Benutzeroberfläche, Systemdienste und alle anderen mit CallKit VOIP-apps veröffentlicht.

Z. B. in der `CXProviderDelegate`:

```csharp
public void ReportIncomingCall (NSUuid uuid, string handle)
{
    // Create update to describe the incoming call and caller
    var update = new CXCallUpdate ();
    update.RemoteHandle = new CXHandle (CXHandleType.Generic, handle);

    // Report incoming call to system
    Provider.ReportNewIncomingCall (uuid, update, (error) => {
        // Was the call accepted
        if (error == null) {
            // Yes, report to call manager
            CallManager.Calls.Add (new ActiveCall (uuid, handle, false));
        } else {
            // Report error to user here
            Console.WriteLine ("Error: {0}", error);
        }
    });
}
```

Dieser Code erstellt ein neues `CXCallUpdate` -Instanz und fügt Sie ein Handle an, die den Aufrufer identifiziert wird. Als Nächstes verwendet er die `ReportNewIncomingCall` Methode der `CXProvider` Klasse, um das System des Aufrufs zu informieren. Wenn sie erfolgreich ist, wird der Aufruf hinzugefügt, der app-Sammlung von aktiven rufe, falls nicht, der Fehler, die dem Benutzer gemeldet werden muss.

### <a name="user-answering-incoming-call"></a>Beantwortung eingehender Aufruf für Benutzer

Wenn der Benutzer den eingehenden VOIP-Anruf zu beantworten möchte, geschieht Folgendes:

[![](callkit-images/callkit06.png "Der Benutzer annimmt, den eingehenden VOIP-Anruf")](callkit-images/callkit06.png#lightbox)

1. Die System-Benutzeroberfläche informiert das System, dass der Benutzer möchte die VOIP-Anruf zu beantworten.
2. Das System sendet eine `CXAnswerCallAction` auf die `CXProvider` informiert sie über den Zweck der Antwort.
3. Die app informiert die Kommunikationsnetzwerk, dass der Benutzer ein Anruf beantworten wird und die VOIP-Anruf wird wie gewohnt fortgesetzt.

Z. B. in der `CXProviderDelegate`:

```csharp
public override void PerformAnswerCallAction (CXProvider provider, CXAnswerCallAction action)
{
    // Find requested call
    var call = CallManager.FindCall (action.CallUuid);

    // Found?
    if (call == null) {
        // No, inform system and exit
        action.Fail ();
        return;
    }

    // Attempt to answer call
    call.AnswerCall ((successful) => {
        // Was the call successfully answered?
        if (successful) {
            // Yes, inform system
            action.Fulfill ();
        } else {
            // No, inform system
            action.Fail ();
        }
    });
}
```

Dieser Code sucht zuerst nach den angegebenen Aufruf in die Liste der aktiven Aufrufe verwendet werden. Wenn der Aufruf kann nicht gefunden werden, wird das System benachrichtigt, und die Methode beendet. Wenn es gefunden wird, die `AnswerCall` Methode der `ActiveCall` Klasse wird aufgerufen, um den Aufruf starten, und das System wird Informationen aus, wenn er erfolgreich war oder ein Fehler auftritt.

### <a name="user-ending-incoming-call"></a>Benutzer, die eingehenden Anruf beenden

Wenn der Benutzer den Aufruf aus, in der Benutzeroberfläche der Anwendung zu beenden möchte, hat dies folgende Konsequenzen:

[![](callkit-images/callkit07.png "Der Benutzer beendet den Aufruf von innerhalb der app Benutzeroberfläche")](callkit-images/callkit07.png#lightbox)

1. Die app erstellt `CXEndCallAction` Ruft ab, die gebündelt eine `CXTransaction` , wird an das System gesendet, um sie darüber zu informieren, dass der Aufruf beendet wird.
2. Das System überprüft das Ziel für die End-aufrufen und sendet die `CXEndCallAction` zurück an die app über die `CXProvider`.
3. Die app informiert dann die Kommunikationsnetzwerk, dass der Aufruf beendet wird.

Z. B. in der `CXProviderDelegate`:

```csharp
public override void PerformEndCallAction (CXProvider provider, CXEndCallAction action)
{
    // Find requested call
    var call = CallManager.FindCall (action.CallUuid);

    // Found?
    if (call == null) {
        // No, inform system and exit
        action.Fail ();
        return;
    }

    // Attempt to answer call
    call.EndCall ((successful) => {
        // Was the call successfully answered?
        if (successful) {
            // Remove call from manager's queue
            CallManager.Calls.Remove (call);

            // Yes, inform system
            action.Fulfill ();
        } else {
            // No, inform system
            action.Fail ();
        }
    });
}
```

Dieser Code sucht zuerst nach den angegebenen Aufruf in die Liste der aktiven Aufrufe verwendet werden. Wenn der Aufruf kann nicht gefunden werden, wird das System benachrichtigt, und die Methode beendet. Wenn es gefunden wird, die `EndCall` Methode der `ActiveCall` Klasse wird aufgerufen, um den Anruf zu beenden und das System ist Informationen aus, wenn er erfolgreich war oder ein Fehler auftritt. Im Erfolgsfall wird der Aufruf aus der Auflistung der aktiven rufe entfernt.

## <a name="managing-multiple-calls"></a>Verwalten von mehreren Aufrufen

Die meisten VOIP-apps können mehrere Aufrufe gleichzeitig verarbeiten. Z. B. wenn derzeit eine aktive VOIP-Anruf ist, und kann die app ruft-Benachrichtigung, dass es einen neuen eingehenden Aufruf, der Benutzer gibt eine anhalten oder Auflegen beim ersten Aufruf der zweite Parameter zu beantworten.

In der oben genannten Situation-bieten, das System sendet eine `CXTransaction` an die app, die eine Liste von mehreren Aktionen enthält. (z. B. die `CXEndCallAction` und `CXAnswerCallAction`). Alle diese Aktionen müssen einzeln erfüllt werden, damit das System die Benutzeroberfläche entsprechend aktualisieren kann.

## <a name="handling-outgoing-calls"></a>Verarbeiten ausgehende Aufrufe

Wenn der Benutzer einen Eintrag aus der Liste "zuletzt verwendet" (in der Phone-app) tippt, z. B. das ist bei einem Aufruf an die app gehört er erhält eine _aufrufen Absicht starten_ vom System:

[![](callkit-images/callkit08.png "Empfangen eine Start-Aufruf Absicht")](callkit-images/callkit08.png#lightbox)

1. Die app erstellt ein _Aktion zum Aufrufen eines starten_ basierend auf der starten aufrufen Absicht, die aus dem System empfangen wurde. 
2. Die app verwendet die `CXCallController` aufrufen Startaktion aus dem System anfordern.
3. Wenn das System die Aktion akzeptiert, es zurückgegeben, für die app über die `XCProvider` delegieren.
4. Die app wird den ausgehenden Aufruf mit dem Netzwerk für die Kommunikation gestartet.

Weitere Informationen zu Intents, informieren Sie sich unsere [Intents und Intents UI-Erweiterungen](~/ios/platform/sirikit/understanding-sirikit.md) Dokumentation. 

### <a name="the-outgoing-call-lifecycle"></a>Der ausgehende Aufruf-Lebenszyklus

Bei der Arbeit mit CallKit und einen ausgehenden Anruf müssen die app das System die folgenden Ereignisse des Lebenszyklus zu informieren:

1. **Ab** -informiert das System, das ein ausgehender Anruf wird gerade gestartet.
2. **Schritte** -informiert das System, das ein ausgehender Anruf gestartet wurde.
3. **Herstellen einer Verbindung** -informiert das System, das der ausgehende Aufruf verbunden ist.
4. **Verbundene** -darüber zu informieren den ausgehenden Aufruf verbunden ist und beide Parteien jetzt kommunizieren können.

Der folgende Code kann z. B. einen ausgehenden Anruf gestartet werden:

```csharp
private CXCallController CallController = new CXCallController ();
...

private void SendTransactionRequest (CXTransaction transaction)
{
    // Send request to call controller
    CallController.RequestTransaction (transaction, (error) => {
        // Was there an error?
        if (error == null) {
            // No, report success
            Console.WriteLine ("Transaction request sent successfully.");
        } else {
            // Yes, report error
            Console.WriteLine ("Error requesting transaction: {0}", error);
        }
    });
}

public void StartCall (string contact)
{
    // Build call action
    var handle = new CXHandle (CXHandleType.Generic, contact);
    var startCallAction = new CXStartCallAction (new NSUuid (), handle);

    // Create transaction
    var transaction = new CXTransaction (startCallAction);

    // Inform system of call request
    SendTransactionRequest (transaction);
}
```

Erstellt eine `CXHandle` und verwendet sie zum Konfigurieren einer `CXStartCallAction` der gebündelt wird eine `CXTransaction` , gesendet, um das System mit der `RequestTransaction` -Methode der der `CXCallController` Klasse. Durch Aufrufen der `RequestTransaction` -Methode, das System kann vorhandenen Aufrufe auf-halten, unabhängig von der Quelle einfügen (Phone-app, FaceTime, VOIP, usw.), bevor der neue Aufruf startet.

Die Anforderung an einen ausgehenden VOIP-Anruf zu starten, kann aus verschiedenen Quellen, wie Siri, einen Eintrag auf einer Karte wenden Sie sich an (in der Kontakte-app) oder aus der Liste "zuletzt verwendet" (in der Phone-app) stammen. In diesen Fällen die app erhält eine starten aufrufen Absicht innerhalb einer `NSUserActivity` und appdelegate-Element verarbeiten müssen:

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{
    var handle = StartCallRequest.CallHandleFromActivity (userActivity);

    // Found?
    if (handle == null) {
        // No, report to system
        Console.WriteLine ("Unable to get call handle from User Activity: {0}", userActivity);
        return false;
    } else {
        // Yes, start call and inform system
        CallManager.StartCall (handle);
        return true;
    }
}
```

Hier die `CallHandleFromActivity` Methode der Hilfsklasse `StartCallRequest` wird verwendet, um das Handle abrufen, an der Person, die aufgerufen wird (finden Sie unter [die StartCallRequest Klasse](#The-StartCallRequest-Class) oben). 

Die `PerformStartCallAction` Methode der [ProviderDelegate Klasse](#The-ProviderDelegate-Class) wird verwendet, um schließlich den tatsächlichen ausgehenden Aufruf starten, und das System des Lebenszyklus informieren:

```csharp
public override void PerformStartCallAction (CXProvider provider, CXStartCallAction action)
{
    // Create new call record
    var activeCall = new ActiveCall (action.CallUuid, action.CallHandle.Value, true);

    // Monitor state changes
    activeCall.StartingConnectionChanged += (call) => {
        if (call.IsConnecting) {
            // Inform system that the call is starting
            Provider.ReportConnectingOutgoingCall (call.UUID, call.StartedConnectingOn.ToNsDate());
        }
    };

    activeCall.ConnectedChanged += (call) => {
        if (call.IsConnected) {
            // Inform system that the call has connected
            Provider.ReportConnectedOutgoingCall (call.UUID, call.ConnectedOn.ToNsDate ());
        }
    };

    // Start call
    activeCall.StartCall ((successful) => {
        // Was the call able to be started?
        if (successful) {
            // Yes, inform the system
            action.Fulfill ();

            // Add call to manager
            CallManager.Calls.Add (activeCall);
        } else {
            // No, inform system
            action.Fail ();
        }
    });
}
```

Es erstellt eine Instanz der `ActiveCall` -Klasse (zum Speichern von Informationen über den Aufruf in Bearbeitung), und füllt Sie mit der Person, die aufgerufen wird. Die `StartingConnectionChanged` und `ConnectedChanged` Ereignisse werden zum Überwachen und melden den Lebenszyklus der ausgehende Aufruf verwendet. Der Aufruf gestartet wird, und das System informiert, dass die Aktion verarbeitet wurde.

### <a name="ending-an-outgoing-call"></a>Beenden einen ausgehenden Anruf

Wenn der Benutzer einen ausgehenden Anruf mit möchte er enden abgeschlossen ist, kann der folgende Code verwendet werden:

```csharp
private CXCallController CallController = new CXCallController ();
...

private void SendTransactionRequest (CXTransaction transaction)
{
    // Send request to call controller
    CallController.RequestTransaction (transaction, (error) => {
        // Was there an error?
        if (error == null) {
            // No, report success
            Console.WriteLine ("Transaction request sent successfully.");
        } else {
            // Yes, report error
            Console.WriteLine ("Error requesting transaction: {0}", error);
        }
    });
}

public void EndCall (ActiveCall call)
{
    // Build action
    var endCallAction = new CXEndCallAction (call.UUID);

    // Create transaction
    var transaction = new CXTransaction (endCallAction);

    // Inform system of call request
    SendTransactionRequest (transaction);
}
```

Wenn erstellt einen `CXEndCallAction` mit der UUID des Aufrufs an den Enden, bündelt sie in einer `CXTransaction` , gesendet, um das System mit der `RequestTransaction` -Methode der der `CXCallController` Klasse. 

## <a name="additional-callkit-details"></a>Zusätzliche CallKit-details

In diesem Abschnitt werden einige zusätzliche Details behandelt, die Entwickler berücksichtigt werden, wenn Sie z. B. mit CallKit zu arbeiten muss:

- Konfiguration des Anbieters
- Aktionsfehler
- Systemeinschränkungen
- VOIP-Audio

### <a name="provider-configuration"></a>Konfiguration des Anbieters

Ermöglicht das iOS 10 VOIP-Apps auf die benutzererfahrung (innerhalb der native Benutzeroberfläche für In-Aufruf) beim Anpassen die Anbieterkonfiguration arbeiten mit CallKit.

Eine app kann die folgenden Arten von Anpassungen vornehmen:

- Eine lokalisierte Anzeigename.
- Aktivieren Sie Videokonferenz-Unterstützung.
- Passen Sie die Schaltflächen auf der Benutzeroberfläche im Aufruf, indem Sie eigene maskierte Bildsymbol darstellen. Benutzerinteraktion mit benutzerdefinierten Schaltflächen wird direkt an die app gesendet, verarbeitet werden. 

### <a name="action-errors"></a>Aktionsfehler

iOS 10 VOIP-apps, die mit der CallKit müssen Aktionen, die Fehler ordnungsgemäß behandeln, und den Benutzer über den Zustand Aktion immer beibehalten. 

Berücksichtigen Sie das folgende Beispiel:

1. Die app hat eine Aktion zum Starten aufrufen, und initialisieren Sie einen neuen VOIP-Anruf mit der Kommunikationsnetzwerk begonnen hat.
2. Schlägt fehl wegen eingeschränkte oder keine Netzwerk-Communication-Funktion diese Verbindung aus.
3. Die app *müssen* senden die **fehlschlagen** zurück an die Aktion zum Starten aufrufen (`Action.Fail()`) auf das System für den Fehler zu informieren.
4. Dies kann das System den Benutzer, der den Status des Aufrufs zu informieren. Geben Sie beispielsweise Folgendes ein, um die Benutzeroberfläche für aufrufen, Fehler angezeigt.

Darüber hinaus müssen eine iOS 10-VOIP-app auf reagieren _Timeoutfehler_ können auftreten, wenn eine erwartete Aktion kann nicht in einem bestimmten Zeitraum verarbeitet werden. Jeder Aktionstyp CallKit gebotenen kann es sich um einen maximalen Timeoutwert zugeordnet. Dieser Timeoutwerte stellen Sie sicher, dass jede CallKit-Aktion, die vom Benutzer angeforderte reaktionsfähig wiederverwendbar behandelt wird, sodass die Sicherheit des Betriebssystems fließende und reaktionsfreudige auch.

Es gibt verschiedene Methoden für den Delegaten-Anbieter (`CXProviderDelegate`), sollte dieses Timeout Situationen ebenfalls sauber überschrieben werden.

### <a name="system-restrictions"></a>Systemeinschränkungen

Basierend auf den aktuellen Status des iOS-Geräts, die die VOIP-app für iOS 10 ausführen, können bestimmte systemeinschränkungen erzwungen werden.

Beispielsweise kann ein VOIP-Anruf wenn vom System eingeschränkt werden:

1. Die Person, die aufgerufen wird in der Liste des Benutzers blockiert Aufrufer.
2. IOS-Gerät des Benutzers ist in der-Not-stören-Modus.

Ein VOIP-Anruf von einer dieser Situationen beschränkt ist, verwenden Sie den folgenden Code, um es zu verarbeiten:

```csharp
public class ProviderDelegate : CXProviderDelegate
{
...

    public void ReportIncomingCall (NSUuid uuid, string handle)
    {
        // Create update to describe the incoming call and caller
        var update = new CXCallUpdate ();
        update.RemoteHandle = new CXHandle (CXHandleType.Generic, handle);
    
        // Report incoming call to system
        Provider.ReportNewIncomingCall (uuid, update, (error) => {
            // Was the call accepted
            if (error == null) {
                // Yes, report to call manager
                CallManager.Calls.Add (new ActiveCall (uuid, handle, false));
            } else {
                // Report error to user here
                if (error.Code == (int)CXErrorCodeIncomingCallError.CallUuidAlreadyExists) {
                    // Handle duplicate call ID
                } else if (error.Code == (int)CXErrorCodeIncomingCallError.FilteredByBlockList) {
                    // Handle call from blocked user
                } else if (error.Code == (int)CXErrorCodeIncomingCallError.FilteredByDoNotDisturb) {
                    // Handle call while in do-not-disturb mode
                } else {
                    // Handle unknown error
                }
            }
        });
    }

}
```

### <a name="voip-audio"></a>VOIP-audio

CallKit bietet mehrere Vorteile für die Behandlung von audio Ressourcen, die eine iOS 10-VOIP-app während eines live VOIP-Aufrufs benötigt. Einer der größten Vorteile ist der app audio-Sitzung verfügen über erweiterte Prioritäten bei der Ausführung unter iOS 10. Dies ist die gleiche Prioritätsstufe als den integrierten Phone und FaceTime-apps und dieser erweiterten Prioritätsebene werden verhindert, andere das Ausführen von apps unterbrechen die VOIP-app-audio-Sitzung.

Darüber hinaus verfügt CallKit Zugriff auf andere audio routing Hinweise, die die Leistung verbessert und auf intelligente Weise Weiterleiten von VOIP-Audio für die spezifische Ausgabegeräte während eines live-Aufrufs basierend auf benutzereinstellungen und Gerätestatus. Beispielsweise basiert auf angeschlossene Geräte wie z. B. Bluetooth Kopfhörer, eine CarPlay liveverbindung oder Einstellungen für die Barrierefreiheit.

Während des Lebenszyklus einer typischen VOIP rufen mit CallKit aus, die app muss auf dem Audio Stream zu konfigurieren, die CallKit es bereitstellt. Sehen Sie sich im folgenden Beispiel an:

[![](callkit-images/callkit09.png "Die Aufruffolge Aktion starten")](callkit-images/callkit09.png#lightbox)

1. Eine Aktion zum Starten aufrufen, die von der app, um einen eingehenden Anruf zu beantworten empfangen wird.
2. Bevor diese Aktion von der app erfüllt wird, bietet die Konfiguration, müssen für die `AVAudioSession`.
3. Die app teilt dem System mit, dass die Aktion abgeschlossen wurde.
4. Bevor die Verbindung hergestellt ist, bietet CallKit hoher Priorität `AVAudioSession` übereinstimmende die Konfiguration, die die app angefordert hat. Benachrichtigt die app über die `DidActivateAudioSession` -Methode der seine `CXProviderDelegate`.

## <a name="working-with-call-directory-extensions"></a>Arbeiten mit dem Aufruf von verzeichniserweiterungen

Bei der Arbeit mit CallKit, _Verzeichniserweiterungen Aufrufen_ bieten eine Möglichkeit zum Hinzufügen von blockierten Aufruf Zahlen und Identifizieren von Zahlen, die für eine bestimmte VOIP-app auf Kontakte in der app wenden Sie sich an, auf dem iOS-Gerät spezifisch sind.

### <a name="implementing-a-call-directory-extension"></a>Implementieren eine Aufruf Directory-Erweiterung

Um ein Anrufverzeichnis in einer Xamarin.iOS-app zu implementieren, führen Sie folgende Schritte aus:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Öffnen der Projektmappe der app in Visual Studio für Mac.
2. Mit der rechten Maustaste auf den Namen der Projektmappe in die **Projektmappen-Explorer** , und wählen Sie **hinzufügen** > **neues Projekt hinzufügen**.
3. Wählen Sie **iOS** > **Erweiterungen** > **Verzeichniserweiterungen Aufrufen** , und klicken Sie auf die **Weiter** Schaltfläche: 

    [![](callkit-images/calldir01.png "Erstellen eine neue Anrufverzeichnis")](callkit-images/calldir01.png#lightbox)
4. Geben Sie einen **Namen** für die Erweiterung und auf die **Weiter** Schaltfläche: 

    [![](callkit-images/calldir02.png "Eingeben eines Namens für die Erweiterung")](callkit-images/calldir02.png#lightbox)
5. Anpassen der **Projektname** und/oder **Projektmappenname** Wenn erforderlich, und klicken Sie auf die **erstellen** Schaltfläche: 

    [![](callkit-images/calldir03.png "Erstellen des Projekts")](callkit-images/calldir03.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Öffnen der app-Projektmappe in Visual Studio.
2. Mit der rechten Maustaste auf den Namen der Projektmappe in die **Projektmappen-Explorer** , und wählen Sie **hinzufügen** > **neues Projekt hinzufügen**.
3. Wählen Sie **iOS** > **Erweiterungen** > **Verzeichniserweiterungen Aufrufen** , und klicken Sie auf die **Weiter** Schaltfläche: 

    [![](callkit-images/calldir01w.png "Erstellen eine neue Anrufverzeichnis")](callkit-images/calldir01.png#lightbox)
4. Geben Sie einen **Namen** für die Erweiterung und auf die **OK** Schaltfläche

-----

Hiermit wird eine `CallDirectoryHandler.cs` Klasse, um das Projekt, das wie folgt aussieht:

```csharp
using System;

using Foundation;
using CallKit;

namespace MonkeyCallDirExtension
{
    [Register ("CallDirectoryHandler")]
    public class CallDirectoryHandler : CXCallDirectoryProvider, ICXCallDirectoryExtensionContextDelegate
    {
        #region Constructors
        protected CallDirectoryHandler (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void BeginRequest (CXCallDirectoryExtensionContext context)
        {
            context.Delegate = this;

            if (!AddBlockingPhoneNumbers (context)) {
                Console.WriteLine ("Unable to add blocking phone numbers");
                var error = new NSError (new NSString ("CallDirectoryHandler"), 1, null);
                context.CancelRequest (error);
                return;
            }

            if (!AddIdentificationPhoneNumbers (context)) {
                Console.WriteLine ("Unable to add identification phone numbers");
                var error = new NSError (new NSString ("CallDirectoryHandler"), 2, null);
                context.CancelRequest (error);
                return;
            }

            context.CompleteRequest (null);
        }
        #endregion

        #region Private Methods
        private bool AddBlockingPhoneNumbers (CXCallDirectoryExtensionContext context)
        {
            // Retrieve phone numbers to block from data store. For optimal performance and memory usage when there are many phone numbers,
            // consider only loading a subset of numbers at a given time and using autorelease pool(s) to release objects allocated during each batch of numbers which are loaded.
            //
            // Numbers must be provided in numerically ascending order.

            long [] phoneNumbers = { 14085555555, 18005555555 };

            foreach (var phoneNumber in phoneNumbers)
                context.AddBlockingEntry (phoneNumber);

            return true;
        }

        private bool AddIdentificationPhoneNumbers (CXCallDirectoryExtensionContext context)
        {
            // Retrieve phone numbers to identify and their identification labels from data store. For optimal performance and memory usage when there are many phone numbers,
            // consider only loading a subset of numbers at a given time and using autorelease pool(s) to release objects allocated during each batch of numbers which are loaded.
            //
            // Numbers must be provided in numerically ascending order.

            long [] phoneNumbers = { 18775555555, 18885555555 };
            string [] labels = { "Telemarketer", "Local business" };

            for (var i = 0; i < phoneNumbers.Length; i++) {
                long phoneNumber = phoneNumbers [i];
                string label = labels [i];
                context.AddIdentificationEntry (phoneNumber, label);
            }

            return true;
        }
        #endregion

        #region Public Methods
        public void RequestFailed (CXCallDirectoryExtensionContext extensionContext, NSError error)
        {
            // An error occurred while adding blocking or identification entries, check the NSError for details.
            // For Call Directory error codes, see the CXErrorCodeCallDirectoryManagerError enum.
            //
            // This may be used to store the error details in a location accessible by the extension's containing app, so that the
            // app may be notified about errors which occurred while loading data even if the request to load data was initiated by
            // the user in Settings instead of via the app itself.
        }
        #endregion
    }
}
```

Die `BeginRequest` -Methode in der Directory-Handler aufrufen müssen geändert werden, um die erforderliche Funktionalität bereitzustellen. Im Fall von im obigen Beispiel wird versucht, die die Liste der blockierten und verfügbaren Zahlen in die VOIP-app-Kontakte-Datenbank festlegen. Wenn entweder Anforderungen aus irgendeinem Grund ein Fehler auftritt, erstellen Sie eine `NSError` beschreiben, und übergeben die `CancelRequest` Methode der `CXCallDirectoryExtensionContext` Klasse.

Die Anzahl der blockierten Verwendung Festlegen der `AddBlockingEntry` -Methode der der `CXCallDirectoryExtensionContext` Klasse. Die Zahlen für die Methode bereitgestellten _müssen_ numerisch aufsteigend sortiert. Berücksichtigen Sie für optimale Leistung und speicherauslastung vorhanden sind, dass viele Zahlen phone, nur eine Teilmenge von Zahlen zu einem bestimmten Zeitpunkt laden und Verwenden von Autorelease Empfangsfunktionen freizugebende Objekte zugeordnet werden, während der einzelnen Batches von Zahlen, die geladen werden.

Verwenden, um darüber zu informieren, wenden Sie sich an der App wenden Sie sich an Zahlen bekannt, dass die VOIP-app, die `AddIdentificationEntry` Methode der `CXCallDirectoryExtensionContext` Klasse, und geben Sie die Anzahl und einer identifizierende Bezeichnung. In diesem Fall die Zahlen, die für die Methode bereitgestellten _müssen_ numerisch aufsteigend sortiert. Berücksichtigen Sie für optimale Leistung und speicherauslastung vorhanden sind, dass viele Zahlen phone, nur eine Teilmenge von Zahlen zu einem bestimmten Zeitpunkt laden und Verwenden von Autorelease Empfangsfunktionen freizugebende Objekte zugeordnet werden, während der einzelnen Batches von Zahlen, die geladen werden.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die neue CallKit-API, die in iOS 10 und wie Sie es in Xamarin.iOS VOIP-apps implementieren veröffentlicht Apple behandelt. Es wurde gezeigt, wie CallKit eine app im IOS-System integrieren kann, wie sie integrierte apps (z. B. Telefon) Featureparität bereitstellt und wie es der app-Sichtbarkeit in der gesamten iOS an Speicherorten wie z. B. die lock- und Home-Bildschirm an, über die Interaktionen von Siri und erhöht die Die Contacts-apps.

## <a name="related-links"></a>Verwandte Links

- [iOS 10-Beispiele](https://developer.xamarin.com/samples/ios/iOS10/)
