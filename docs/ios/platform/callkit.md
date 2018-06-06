---
title: CallKit in Xamarin.iOS
description: Dieser Artikel behandelt die neuen CallKit-API, Apple iOS 10 und wie Sie es in Xamarin.iOS VOIP-apps implementieren freigegeben.
ms.prod: xamarin
ms.assetid: 738A142D-FFD2-4738-B3ED-57C273179848
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: c674802eac9105d60471b6b130615e1b7efc1b28
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787200"
---
# <a name="callkit-in-xamarinios"></a>CallKit in Xamarin.iOS

_Dieser Artikel behandelt die neuen CallKit-API, Apple iOS 10 und wie Sie es in Xamarin.iOS VOIP-apps implementieren freigegeben._

Die neue CallKit-API in iOS 10 bietet eine Möglichkeit für VOIP-apps für iPhone-Benutzeroberfläche integriert und bieten eine vertraute Oberfläche für den Endbenutzer auftreten. Mit dieser API Benutzer anzeigen und interagieren mit VOIP-Aufrufe von des iOS-Geräts Sperrbildschirm und verwalten können Kontakte mit der Phone-app **Favoriten** und **zuletzt geöffnete** Ansichten.

## <a name="about-callkit"></a>Informationen zu CallKit

Gemäß den Apple ist CallKit ein neues Framework, das 3rd apps von Drittanbietern Stimme über IP (VOIP) eine 1. Erfahrung auf iOS 10 Partei erhöhen wird. Die CallKit-API ermöglicht VOIP-apps für iPhone-Benutzeroberfläche integriert und bieten eine vertraute Oberfläche für den Endbenutzer auftreten. Genau wie die integrierte Phone-app ein Benutzer anzeigen und interagieren mit VOIP-Aufrufe von des iOS-Geräts Sperrbildschirm und verwalten kann Kontakte mit der Phone-app **Favoriten** und **zuletzt geöffnete** Ansichten.

Darüber hinaus bietet die CallKit-API die Fähigkeit zum Erstellen von App-Erweiterungen, die einem Namen (Anrufer-ID) eine zehnstellige Nummer zuordnen oder mitteilt, dass das System, wenn eine Zahl sein darf (aufrufen blockiert sind) blockiert können.

### <a name="the-existing-voip-app-experience"></a>Die vorhandenen VOIP-App-Erfahrung

Vor dem im Zusammenhang mit der neuen API zum CallKit und seiner Funktionen an, die vom aktuellen Benutzer wahrnehmbare a 3rd Partei VOIP-app im iOS 9 (und weniger als) mithilfe einer fiktiven VOIP-app MonkeyCall aufgerufen. MonkeyCall ist eine einfache app, die ermöglicht es dem Benutzer zum Senden und Empfangen von VOIP-Aufrufe, die mithilfe der vorhandenen iOS-APIs.

Aktuell, wenn der Benutzer einen eingehenden Anruf auf MonkeyCall erhält und ihrer iPhone ist gesperrt, die Benachrichtigung empfangen, auf dem Sperrbildschirm lässt nicht von irgendeinem anderen Typ der Benachrichtigung (etwa aus den Nachrichten oder z. B. e-Mail-apps).

Wenn der Benutzer den Anruf entgegennehmen, müssten sie schieben Sie die Benachrichtigung MonkeyCall, öffnen Sie die app, und geben ihre Kennung (oder Benutzer Touch ID) zum Entsperren des Telefons, bevor sie den Anruf entgegennehmen und die Konversation starten konnte.

Die Erfahrung ist gleichermaßen umständlich, wenn das Telefon entsperrt wird. Erneut, wird der eingehenden MonkeyCall Aufruf als standard Notification Banner angezeigt, die Folien in vom oberen Rand des Bildschirms. Da die Benachrichtigung temporär ist, kann es leicht vergessen werden durch den Benutzer gezwungen wird, öffnen Sie die Mitteilungszentrale und suchen die jeweiligen Benachrichtigung beantworten dann aufrufen, oder suchen und MonkeyCall-app manuell zu starten.

### <a name="the-callkit-voip-app-experience"></a>Die CallKit VOIP-App-Erfahrung

Durch die Implementierung der neuen CallKit-APIs in der app MonkeyCall, kann die benutzerfreundlichkeit mit einer eingehenden VOIP-Aufruf in iOS 10 deutlich verbessert. Nehmen Sie als Beispiel des Benutzers einen VOIP-Aufruf empfangen, wenn ihre Rufnummer oben gesperrt ist. Der Aufruf wird durch die Implementierung CallKit können auf dem iPhone Sperrbildschirm, angezeigt, ebenso wie dies der Fall wäre, wenn der Aufruf aus der integrierten Phone-app mit dem Vollbildmodus, systemeigene Benutzeroberfläche und Wischen-Antwort-Standardfunktionen empfangen wurde.

Auch wenn die iPhone aufgehoben wird, wenn eine MonkeyCall VOIP-Anruf empfangen wird, hat die gleichen Vollbildmodus, systemeigene Benutzeroberfläche und Wischen-Antwort- und tippen Sie zum Ablehnen Standardfunktionen der integrierten Phone-app angezeigt wird und MonkeyCall die Möglichkeit, einen benutzerdefinierten Klingelton wiedergeben .

CallKit bietet zusätzlicher Funktionalität für MonkeyCall, sodass seine VOIP aufruft, zur Interaktion mit anderen Arten von aufrufen, in der integrierten zuletzt geöffnete angezeigt werden und listet Favoriten, um die integrierten nicht stören und Block-Features verwenden zu können, beginnen Sie MonkeyCall Aufrufe von Siri und bietet die Möglichkeit für Benutzer MonkeyCall Aufrufe an Personen in der Kontakte-app zugewiesen.

In den folgenden Abschnitten werden die Architektur CallKit behandelt, die eingehenden und ausgehenden aufrufen Datenflüssen und die CallKit-API im Detail.


## <a name="the-callkit-architecture"></a>Die CallKit-Architektur

In iOS 10 hat Apple CallKit in allen Systemdienste eingeführt, sodass Aufrufe, die auf CarPlay, z. B. die System-Benutzeroberfläche über CallKit bekannt sind. In im Beispiel unten angegeben werden, da MonkeyCall CallKit einführt, es in die gleiche Weise wie diese integrierte System-Dienste mit dem System bekannt ist und alle die gleichen Funktionen abgerufen:

[![](callkit-images/callkit01.png "CallKit Dienststapel")](callkit-images/callkit01.png#lightbox)

Nehmen Sie eine genauere Betrachtung der MonkeyCall App aus der Abbildung oben. Die app enthält alle des zugehörigen Codes für die Kommunikation mit einem eigenen Netzwerk sowie eigene Benutzeroberflächen. Es enthält links in CallKit für die Kommunikation mit dem System:

[![](callkit-images/callkit02.png "Architektur der MonkeyCall-App")](callkit-images/callkit02.png#lightbox)

Es gibt zwei Schnittstellen in CallKit, die die app verwendet:

- `CXProvider` -Dies ermöglicht die app MonkeyCall, um das System für die Out-of-Band-Benachrichtigungen zu informieren, die auftreten können.
- `CXCallController` – Ermöglicht es die app MonkeyCall, um das System des lokalen Benutzeraktionen zu informieren.

### <a name="the-cxprovider"></a>Die CXProvider

Wie bereits erwähnt, `CXProvider` ermöglicht einer app, um das System für die Out-of-Band-Benachrichtigungen zu informieren, die auftreten können. Hierbei handelt es sich um Benachrichtigung, die nicht aufgrund eines lokalen Benutzeraktionen auftreten, aber kommen, da externe Ereignisse wie z. B. eingehende Aufrufe.

Eine app zu verwendende der `CXProvider` für Folgendes:

- Einen eingehenden Anruf an das System zu melden.
- Berichten ein, der ausgehende Aufruf mit dem System verbunden hat.
- Den Remotebenutzer, beenden den Aufruf an das System zu melden.

Wenn möchte, dass die app mit dem System kommunizieren, verwendet der `CXCallUpdate` Klasse, und wenn das System für die Kommunikation mit der app muss, die Anwendung verwendet die `CXAction` Klasse:

[![](callkit-images/callkit03.png "Bei der Kommunikation mit dem System über eine CXProvider")](callkit-images/callkit03.png#lightbox)

### <a name="the-cxcallcontroller"></a>Die CXCallController

Die `CXCallController` ermöglicht einer app informiert das System des lokalen Benutzeraktionen wie der Benutzer einen VOIP-Anruf ab. Durch die Implementierung einer `CXCallController` Ruft mit anderen Typen von Aufrufen im System Interaktion zwischen die app ab. Wenn es ist bereits ein aktive Telefonie-Aufruf ausgeführt wird, z. B. `CXCallController` können die VOIP-app, setzen Sie diesen Aufruf anhalten und starten oder einen VOIP-Anruf zu beantworten.

Eine app zu verwendende der `CXCallController` für Folgendes:

- Bericht, wenn der Benutzer einen ausgehenden Aufruf an das System gestartet wurde.
- Bericht, wenn der Benutzer auf das System einen eingehenden Anruf annimmt.
- Bericht, wenn der Benutzer einen Aufruf an das System beendet.

Wenn die app lokaler Benutzeraktionen auf dem System kommunizieren möchte, verwendet die `CXTransaction` Klasse:

[![](callkit-images/callkit04.png "Um das System über eine CXCallController Reporting")](callkit-images/callkit04.png#lightbox)

## <a name="implementing-callkit"></a>Implementieren von CallKit

In den folgenden Abschnitten erfahren, wie in einer app Xamarin.iOS VOIP CallKit implementieren. Beispiel wird dieses Dokument Code aus der fiktiven MonkeyCall VOIP-app verwenden. Der Code, der hier vorgestellten stellt mehrere unterstützende Klassen, die CallKit bestimmte Teile werden in den folgenden Abschnitten ausführlich behandelt.

### <a name="the-activecall-class"></a>Die ActiveCall-Klasse

Die `ActiveCall` -Klasse wird von der MonkeyCall-app verwendet, um alle Informationen über einen VOIP-Aufruf aufzunehmen, die derzeit aktiv ist, wie folgt:

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

`ActiveCall` enthält einige Eigenschaften, die definieren den Status des Aufrufs und zwei Ereignisse, die ausgelöst werden können, wenn der Aufruf Zustand ändert. Da dies nur ein Beispiel ist, gibt es drei Methoden, die verwendet, um die simulierte starten, beantworten, und Beenden einen Aufruf an.

### <a name="the-startcallrequest-class"></a>Die StartCallRequest-Klasse

Die `StartCallRequest` statische Klasse bietet einige Hilfsmethoden, die beim Arbeiten mit ausgehende Aufrufe verwendet werden:

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

Die `CallHandleFromURL` und `CallHandleFromActivity` Klassen werden in der AppDelegate verwendet, wenden Sie sich an das Handle für die Person, die in einen ausgehenden Aufruf aufgerufen werden abgerufen. Weitere Informationen finden Sie unter der [ausgehende Aufrufe Behandlung von](#Handling-Outgoing-Calls) Abschnitt weiter unten.

### <a name="the-activecallmanager-class"></a>Die ActiveCallManager-Klasse

Die `ActiveCallManager` Klasse alle geöffneten Aufrufe in der app MonkeyCall verarbeitet.

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
                if (call.UUID == uuid) return call;
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

Erneut aus, da dies nur eine Simulation ist die `ActiveCallManager` nur verwaltet eine Auflistung von `ActiveCall` Objekte und verfügt über eine Routine für die Suche nach einem bestimmten Aufruf von dessen `UUID` Eigenschaft. Darüber hinaus Methoden zum Starten, beenden, und Ändern des Status auf halten einen ausgehenden Aufruf. Weitere Informationen finden Sie unter der [ausgehende Aufrufe Behandlung von](#Handling-Outgoing-Calls) Abschnitt weiter unten.

### <a name="the-providerdelegate-class"></a>Die ProviderDelegate-Klasse

Wie oben beschrieben ein `CXProvider` bietet bidirektionalen Kommunikation zwischen der Anwendung und das System für die Out-of-Band-Benachrichtigungen. Der Entwickler muss eine benutzerdefinierte bereitstellen `CXProviderDelegate` und fügen Sie es auf die `CXProvider` die App auf die Out-of-Band-CallKit Ereignisse zu behandeln. MonkeyCall verwendet die folgenden `CXProviderDelegate`:

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

Wenn eine Instanz dieses Delegaten erstellt wird, wird es übergeben der `ActiveCallManager` , behandeln Sie jede Aktivität verwendet wird. Als Nächstes definiert die Handletypen (`CXHandleType`), die die `CXProvider` antwortet auf:

```csharp
// Define handle types
var handleTypes = new [] { (NSNumber)(int)CXHandleType.PhoneNumber };
```

Und es Ruft die Maske, die auf das Symbol für die app angewendet wird, wenn ein Aufruf ausgeführt wird:

```csharp
// Get Image Mask
var maskImage = UIImage.FromFile ("telephone_receiver.png");
```

Diese Werte abrufen gebündelt eine `CXProviderConfiguration` , die verwendet werden wird, so konfigurieren Sie die `CXProvider`:

```csharp
// Setup the initial configurations
Configuration = new CXProviderConfiguration ("MonkeyCall") {
    MaximumCallsPerCallGroup = 1,
    SupportedHandleTypes = new NSSet<NSNumber> (handleTypes),
    IconMaskImageData = maskImage.AsPNG(),
    RingtoneSound = "musicloop01.wav"
};
```

Der Delegat erstellt dann ein neues `CXProvider` mit diesen Konfigurationen und fügt sich selbst zu:

```csharp
// Create a new provider
Provider = new CXProvider (Configuration);

// Attach this delegate
Provider.SetDelegate (this, null);
```

Wenn CallKit verwenden zu können, die app nicht mehr erstellt und eine eigene audio Sitzungen zu behandeln, müssen sie stattdessen zum Konfigurieren und verwenden eine audio-Sitzung, die das System zu erstellen und behandeln dafür. 

Wenn dies eine wirkliche app wäre die `DidActivateAudioSession` Methode wird verwendet, um den Aufruf mit einem vorkonfigurierten starten `AVAudioSession` , die das System bereitgestellt:

```csharp
public override void DidActivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
{
    // Start the call's audio session here...
}
```

Verwenden Sie auch die `DidDeactivateAudioSession` Methode zu beenden und Freigeben der Verbindungs mit dem System bereitgestellt, audio Sitzung:

```csharp
public override void DidDeactivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
{
    // End the calls audio session and restart any non-call
    // releated audio
}
```

Der Rest des Codes wird in den Abschnitten detailliert beschrieben werden, die folgen.

### <a name="the-appdelegate-class"></a>Die AppDelegate-Klasse

MonkeyCall verwendet die AppDelegate für Instanzen der `ActiveCallManager` und `CXProviderDelegate` , die in der app verwendet werden:

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

Die `OpenUrl` und `ContinueUserActivity` Außerkraftsetzung Methoden werden verwendet, wenn die app einen ausgehenden Aufruf verarbeitet wird. Weitere Informationen finden Sie unter der [ausgehende Aufrufe Behandlung von](#Handling-Outgoing-Calls) Abschnitt weiter unten.

## <a name="handling-incoming-calls"></a>Behandlung von eingehende Anrufe

Es gibt mehrere Zustände und Prozesse, die ein eingehenden VOIP-Aufruf während ein typischer Workflow für eingehenden Aufruf wie z. B. durchlaufen kann:

- Informieren den Benutzer (und das System), dass ein eingehender Anruf vorhanden ist.
- Empfangen von Benachrichtigungen, wenn der Benutzer möchte den Anruf zu beantworten, und den Aufruf mit einem anderen Benutzer initialisieren.
- Das System und Kommunikationsnetzwerk informiert werden, wenn der Benutzer möchte die aktuellen Anruf zu beenden.

In den folgenden Abschnitten dauert eine ausführliche Übersicht über die wie eine app CallKit verwenden kann, um den eingehenden Aufruf Workflow mit erneut als Beispiel die MonkeyCall VOIP-app zu behandeln.

### <a name="informing-user-of-incoming-call"></a>Informieren Sie Benutzer von eingehender Anrufe

Wenn ein Remotebenutzer eine VOIP-Konversation mit dem lokalen Benutzer gestartet wurde, geschieht Folgendes:

[![](callkit-images/callkit05.png "Ein Remotebenutzer wurde eine VOIP-Konversation gestartet werden.")](callkit-images/callkit05.png#lightbox)

1. Die app ruft einer Benachrichtigung aus seiner Kommunikationsnetzwerk, ergibt sich ein eingehender VOIP-Anruf.
2. Der app verwendet die `CXProvider` zum Senden einer `CXCallUpdate` an das System des Aufrufs zu informieren.
3. Das System werden den Aufruf der System-Benutzeroberfläche, Systemdienste und alle anderen VOIP-apps mit CallKit veröffentlicht.

Beispielsweise ist in der `CXProviderDelegate`:

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

Dieser Code erstellt ein neues `CXCallUpdate` Instanz und fügt ein Handle, der den Aufrufer identifiziert werden kann. Als Nächstes wird die `ReportNewIncomingCall` Methode der `CXProvider` Klasse, um das System des Aufrufs zu informieren. Wenn der Vorgang erfolgreich, wird der Aufruf hinzugefügt wird, in der app-Sammlung von aktiven rufe, wenn dies nicht der Fall, der Fehler muss an den Benutzer ausgegeben werden.

### <a name="user-answering-incoming-call"></a>Benutzer Beantwortung eingehender Anrufe

Wenn der Benutzer die VOIP-Anruf zu beantworten möchte, hat dies folgende Konsequenzen:

[![](callkit-images/callkit06.png "Der Benutzer nimmt den eingehenden VOIP-Anruf")](callkit-images/callkit06.png#lightbox)

1. Die System-Benutzeroberfläche informiert das System, dass der Benutzer möchte die VOIP-Anruf zu beantworten.
2. Das System sendet eine `CXAnswerCallAction` an der app `CXProvider` darüber informiert wird, der die Absicht der Antwort.
3. Die app ihre Kommunikationsnetzwerk informiert werden, dass der Benutzer wird durch die Beantwortung des Aufrufs der VOIP-Aufruf wie gewohnt fortgesetzt wird.

Beispielsweise ist in der `CXProviderDelegate`:

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

Dieser Code sucht zuerst nach dem Aufruf in die Liste der aktiven rufe. Wenn der Aufruf nicht gefunden werden, das System eine Benachrichtigung, und die Methode beendet. Wenn es gefunden wird, die `AnswerCall` Methode der `ActiveCall` Klasse aufgerufen, um den Aufruf gestartet, und wird vom System Informationen aus, wenn er erfolgreich ist oder fehlschlägt.

### <a name="user-ending-incoming-call"></a>Benutzer, die eingehenden Anruf zu beenden

Wenn der Benutzer den Aufruf von in der Benutzeroberfläche der Anwendung zu beenden möchte, hat dies folgende Konsequenzen:

[![](callkit-images/callkit07.png "Der Benutzer beendet den Aufruf von in der Benutzeroberfläche der Anwendung")](callkit-images/callkit07.png#lightbox)

1. Die app erstellt `CXEndCallAction` , die gebündelt Ruft eine `CXTransaction` , die an das System gesendet wird, um sie zu informieren, dass der Aufruf beendet wird.
2. Das System überprüft die Absicht für die End-aufrufen und sendet die `CXEndCallAction` zurück an die app über die `CXProvider`.
3. Die app informiert dann seine Kommunikationsnetzwerk, dass der Aufruf beendet wird.

Beispielsweise ist in der `CXProviderDelegate`:

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

Dieser Code sucht zuerst nach dem Aufruf in die Liste der aktiven rufe. Wenn der Aufruf nicht gefunden werden, das System eine Benachrichtigung, und die Methode beendet. Wenn es gefunden wird, die `EndCall` Methode der `ActiveCall` Klasse aufgerufen, um den Anruf zu beenden, und wird vom System Informationen aus, wenn er erfolgreich ist oder fehlschlägt. Im Erfolgsfall wird der Aufruf aus der Auflistung der aktiven rufe entfernt.

## <a name="managing-multiple-calls"></a>Verwalten von mehreren Aufrufen

Die meisten VOIP-apps können mehrere Aufrufe gleichzeitig zu verarbeiten. Wenn z. B. besteht derzeit ein aktiver VOIP-Aufruf und kann die app ruft-Benachrichtigung, dass es gibt eine neue eingehender Anrufe, der Benutzer ist angehalten oder Auflegen beim ersten Aufruf dem zweiten Ausdruck beantworten an.

In der obigen Situation-erteilen, sendet das System eine `CXTransaction` der App, die eine Liste von mehreren Aktionen enthalten (z. B. die `CXEndCallAction` und `CXAnswerCallAction`). Alle diese Aktionen werden einzeln, erfüllt werden müssen, damit das System entsprechend die Benutzeroberfläche aktualisieren kann.

## <a name="handling-outgoing-calls"></a>Behandlung von ausgehende Anrufe

Wenn der Benutzer einen Eintrag aus der Liste zuletzt geöffnete (in der Phone-app) tippt, z. B., die von einem Aufruf an die app, gehören ist es wird gesendet werden eine _aufrufen Absicht starten_ vom System:

[![](callkit-images/callkit08.png "Die Absicht eine Start-Aufruf empfangen")](callkit-images/callkit08.png#lightbox)

1. Die app erstellt eine _aufrufen Startaktion_ basierend auf der starten aufrufen Absicht, die vom System empfangen wurde. 
2. Die app verwendet die `CXCallController` aufrufen Startaktion aus dem System anfordern.
3. Wenn das System die Aktion annimmt, wird diese an die app über zurückgegeben werden die `XCProvider` delegieren.
4. Die app wird mit seiner Kommunikationsnetzwerk des ausgehenden Aufrufs gestartet.

Weitere Informationen zu Intents, finden Sie unter unsere [Intents und Intents UI Erweiterungen](~/ios/platform/sirikit/understanding-sirikit.md) Dokumentation. 

### <a name="the-outgoing-call-lifecycle"></a>Der ausgehende Aufruf-Lebenszyklus

Bei der Arbeit mit CallKit und einen ausgehenden Aufruf müssen die app das System die folgenden Lebenszyklusereignisse zu informieren:

1. **Starten von** -informiert das System, das ein ausgehender Aufruf wird gerade gestartet.
2. **Gestartet** -informiert das System, das ein ausgehender Aufruf gestartet wurde.
3. **Herstellen einer Verbindung** -informiert das System, das der ausgehende Aufruf verbunden ist.
4. **Verbunden** -informiert den ausgehenden Aufruf hat eine Verbindung hergestellt werden, sodass beide Parteien jetzt kommunizieren können.

Der folgende Code kann z. B. einen ausgehenden Aufruf gestartet werden:

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

Erstellt eine `CXHandle` und verwendet ihn so konfigurieren Sie eine `CXStartCallAction` der gebündelt ist ein `CXTransaction` also gesendet, um das System mithilfe der `RequestTransaction` Methode der `CXCallController` Klasse. Durch Aufrufen der `RequestTransaction` -Methode, das System kann vorhandenen Aufrufe auf halten, platzieren, unabhängig von der Quelle (Phone-app, FaceTime, VOIP, usw.), bevor Sie der neue Aufruf gestartet wird.

Die Anforderung zum Starten eines ausgehenden VOIP-Aufrufs kann aus verschiedenen Quellen, wie Siri, einem Eintrag in einer Kontaktkarte (in der Kontakte-app) oder aus der Liste zuletzt geöffnete (in der Phone-app) stammen. In solchen Fällen können die app gesendet eine starten aufrufen Absicht innerhalb einer `NSUserActivity` und die AppDelegate verarbeiten müssen:

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

Hier die `CallHandleFromActivity` Methode von der Hilfsklasse `StartCallRequest` wird verwendet, um das Handle an der Person, die aufgerufen werden abgerufen (finden Sie unter [die StartCallRequest Klasse](#The-StartCallRequest-Class) oben). 

Die `PerformStartCallAction` Methode der [ProviderDelegate Klasse](#The-ProviderDelegate-Class) wird verwendet, um schließlich die tatsächlichen ausgehenden Aufruf gestartet und informiert das System des Lebenszyklus:

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

Er erstellt eine Instanz der `ActiveCall` -Klasse (für Informationen über den Aufruf in Bearbeitung), und füllt Sie mit der Person, die aufgerufen werden. Die `StartingConnectionChanged` und `ConnectedChanged` Ereignisse werden zum Überwachen und einen ausgehenden Aufruf Lebenszyklus Bericht verwendet. Der Aufruf gestartet, und das System darüber informiert, dass die Aktion verarbeitet wurde.

### <a name="ending-an-outgoing-call"></a>Beenden einen ausgehenden Aufruf

Wenn der Benutzer möchte jedoch und einen ausgehenden Aufruf abgeschlossen ist, kann der folgende Code verwendet werden:

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

Wenn erstellt eine `CXEndCallAction` gebündelt, mit der UUID des Aufrufs zum Beenden, ihn in eine `CXTransaction` also gesendet, um das System mithilfe der `RequestTransaction` Methode der `CXCallController` Klasse. 

## <a name="additional-callkit-details"></a>Zusätzliche CallKit-Details

In diesem Abschnitt werden einige zusätzliche Details behandelt, die der Entwickler beim Arbeiten z. B. mit CallKit berücksichtigt werden muss:

- Die Konfiguration des Anbieters
- Aktionsfehler
- System-Einschränkungen
- VOIP-Audio

### <a name="provider-configuration"></a>Die Konfiguration des Anbieters

Die Anbieterkonfiguration ermöglicht einer iOS-10-VOIP-app erforderlichen Erfahrungsgrad des Benutzers (in die systemeigene Benutzeroberfläche des im Aufruf) anpassen, wenn CallKit arbeiten.

Eine app sind die folgenden Arten von Anpassungen möglich:

- Einen lokalisierten Name wird angezeigt.
- Aktivieren Sie Videokonferenz-Unterstützung.
- Anpassen der Schaltflächen auf der Benutzeroberfläche im Aufruf durch eine eigene maskierten Bildsymbol bereitstellen. Benutzerinteraktion mit benutzerdefinierten Schaltflächen wird direkt an die Anwendung gesendet, um verarbeitet werden. 

### <a name="action-errors"></a>Aktionsfehler

iOS 10 VOIP-apps, die mithilfe von CallKit müssen Aktionen Fehler ordnungsgemäß behandeln, und behalten Sie die Benutzer über den Zustand Aktion jederzeit informiert. 

Berücksichtigen Sie das folgende Beispiel:

1. Die app hat eine Aktion zum Starten aufrufen und den Prozess stehen zum Initialisieren eines neuen VOIP-Aufrufs mit seiner Kommunikationsnetzwerk begonnen hat.
2. Ein Fehler aufgrund von wenig oder keine Kommunikation-Funktion des Netzwerks auftritt diese Verbindung.
3. Die app *müssen* Senden der **fehlschlagen** -Nachricht zurück an die Startaktion für aufrufen (`Action.Fail()`) um das System den Fehler zu informieren.
4. Dies kann das System den Benutzer über den Status des Aufrufs zu informieren. Geben Sie beispielsweise Folgendes ein, um die Benutzeroberfläche aufrufen Fehler anzeigen.

Darüber hinaus müssen eine iOS-10-VOIP-app So reagieren Sie auf _Timeoutfehler_ , die auftreten können, wenn eine erwartete Aktion innerhalb einer bestimmten Zeitspanne nicht verarbeitet werden kann. Jeder Aktion CallKit gebotenen hat einen maximale Timeoutwert zugeordnet. Dieser Timeoutwerte stellen Sie sicher, dass alle CallKit-Aktion, die vom Benutzer angefordert wird auf eine Weise reagieren behandelt wird, sodass die Sicherheit des Betriebssystems flüssig und schnell reagierend sowie.

Es gibt mehrere Methoden für den Anbieter Delegaten (`CXProviderDelegate`), sollte dieses Timeout Situationen ebenfalls sauber überschrieben werden.

### <a name="system-restrictions"></a>System-Einschränkungen

Basierend auf den aktuellen Status der iOS-Gerät, die die VOIP-app für iOS 10 ausgeführt, können bestimmte System Einschränkungen erzwungen werden.

Beispielsweise kann ein eingehender Anruf von VOIP Wenn vom System eingeschränkt werden:

1. Die Person, die aufrufen ist auf der Liste des Benutzers blockiert Aufrufer.
2. IOS-Gerät des Benutzers befindet sich im Modus Not stören.

Wenn ein VOIP-Aufruf durch diesen Fällen eingeschränkt ist, verwenden Sie den folgenden Code verarbeiten:

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

### <a name="voip-audio"></a>VOIP-Audio

CallKit bietet verschiedene Vorteile für die Behandlung von Audio-Ressourcen, die eine iOS-10-VOIP-app während eines Aufrufs der live VOIP erfordern. Einer der größten Vorteile ist, dass der app audio-Sitzung wird Prioritäten erhöhten, bei der Ausführung in iOS 10. Dies ist die gleiche Prioritätsstufe als das integrierte Phone und FaceTime apps und diese erweiterte Prioritätsstufe hindert andere ausgeführte apps unterbrechen die VOIP-app-audio-Sitzung.

Darüber hinaus weist CallKit Zugriff auf andere audio routing Hinweise, die die Leistung verbessert und eine intelligente fehlerwiederherstellung VOIP-Audio für die spezifische Ausgabegeräte bei einem live-Aufruf basierend auf den benutzereinstellungen und Gerätestatus weiterleiten können. Zum Beispiel basiert auf angeschlossene Geräte wie z. B. Bluetooth-Kopfhörer, eine liveverbindung CarPlay oder Einstellungen für die Barrierefreiheit.

Während des Lebenszyklus einer typischen VOIP mit CallKit aufrufen, muss die app die Audio-Datenstrom zu konfigurieren, die CallKit es bereitstellt. Sehen Sie sich im folgenden Beispiel an:

[![](callkit-images/callkit09.png "Die Aufruffolge Aktion starten")](callkit-images/callkit09.png#lightbox)

1. Eine Aktion zum Starten aufrufen, die von der app einen eingehenden Anruf zu beantworten empfangen wird.
2. Bevor diese Aktion von der app erfüllt ist, bietet die Konfiguration, die für erforderlich seine `AVAudioSession`.
3. Die app informiert das System, dass die Aktion erfüllt wurde.
4. Bevor die Verbindung hergestellt ist, bietet CallKit hoher Priorität `AVAudioSession` Abgleich der Konfigurations, die die app angefordert hat. Die app benachrichtigt Sie über die `DidActivateAudioSession` Methode seine `CXProviderDelegate`.

## <a name="working-with-call-directory-extensions"></a>Arbeiten mit Aufruf Verzeichniserweiterungen

Bei der Arbeit mit CallKit, _Verzeichniserweiterungen Aufrufen_ bieten eine Möglichkeit zum Hinzufügen von blockierten Aufruf Zahlen und Identifizieren von Zahlen, die auf einer bestimmten VOIP-app, Kontakte in der Contact-app auf iOS-Gerät beziehen.

### <a name="implementing-a-call-directory-extension"></a>Implementieren eine Aufruf Directory-Erweiterung

Führen Sie folgende Schritte aus, um sinnvolles Directory Aufrufen in einem Xamarin.iOS-app zu implementieren:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Öffnen Sie die app-Projektmappe in Visual Studio für Mac.
2. Mit der rechten Maustaste auf den Namen der Projektmappe in der **Projektmappen-Explorer** , und wählen Sie **hinzufügen** > **neues Projekt hinzufügen**.
3. Wählen Sie **iOS** > **Erweiterungen** > **Verzeichniserweiterungen Aufrufen** , und klicken Sie auf die **Weiter** Schaltfläche: 

    [![](callkit-images/calldir01.png "Erstellen einer neuen aufrufen Directory-Erweiterung")](callkit-images/calldir01.png#lightbox)
4. Geben Sie einen **Namen** für die Erweiterung, und klicken Sie auf die **Weiter** Schaltfläche: 

    [![](callkit-images/calldir02.png "Einen Namen für die Erweiterung eingeben")](callkit-images/calldir02.png#lightbox)
5. Anpassen der **Projektname** und/oder **Projektmappenname** Wenn erforderlich, und klicken Sie auf die **erstellen** Schaltfläche: 

    [![](callkit-images/calldir03.png "Erstellen des Projekts")](callkit-images/calldir03.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Öffnen Sie die app-Projektmappe in Visual Studio.
2. Mit der rechten Maustaste auf den Namen der Projektmappe in der **Projektmappen-Explorer** , und wählen Sie **hinzufügen** > **neues Projekt hinzufügen**.
3. Wählen Sie **iOS** > **Erweiterungen** > **Verzeichniserweiterungen Aufrufen** , und klicken Sie auf die **Weiter** Schaltfläche: 

    [![](callkit-images/calldir01w.png "Erstellen einer neuen aufrufen Directory-Erweiterung")](callkit-images/calldir01.png#lightbox)
4. Geben Sie einen **Namen** für die Erweiterung, und klicken Sie auf die **OK** Schaltfläche

-----


Dies fügt eine `CallDirectoryHandler.cs` Klasse, um das Projekt, das wie folgt aussieht:

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

Die `BeginRequest` Methode in der Directory-Handler aufrufen müssen geändert werden, um die erforderliche Funktionalität bereitzustellen. Bei dem obigen Beispiel versucht die Liste der blockierten und verfügbaren Zahlen in die VOIP-app-Kontakte-Datenbank festlegen. Wenn aus irgendeinem Grund-Anforderungen entweder ein Fehler auftritt, erstellen Sie eine `NSError` beschreiben, und übergeben der `CancelRequest` Methode der `CXCallDirectoryExtensionContext` Klasse.

Die Verwendung von blockierten Zahlen Festlegen der `AddBlockingEntry` Methode der `CXCallDirectoryExtensionContext` Klasse. Die Zahlen für die Methode bereitgestellten _müssen_ numerisch sortiert werden. Für optimale Leistung und speicherauslastung bei vorhanden, dass viele Zahlen Telefon sind, erwägen Sie nur laden eine Teilmenge von Zahlen zu einem bestimmten Zeitpunkt und Autorelease Pools zum Freigeben von Objekten zugeordnet, während der einzelnen Batches von Zahlen, die geladen werden.

Verwenden, wenden Sie sich an App wenden Sie sich an Zahlen bekannt, dass die VOIP-app informiert werden, die `AddIdentificationEntry` Methode der `CXCallDirectoryExtensionContext` Klasse, und geben Sie die Anzahl und eine Bezeichnung. Erneut, die Zahlen, die für die Methode bereitgestellten _müssen_ numerisch sortiert werden. Für optimale Leistung und speicherauslastung bei vorhanden, dass viele Zahlen Telefon sind, erwägen Sie nur laden eine Teilmenge von Zahlen zu einem bestimmten Zeitpunkt und Autorelease Pools zum Freigeben von Objekten zugeordnet, während der einzelnen Batches von Zahlen, die geladen werden.


## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden neue CallKit-API, Apple iOS 10 und wie er in Xamarin.iOS VOIP-apps implementiert veröffentlicht behandelt. Es wurde gezeigt, CallKit, wie eine app in der iOS-System integrieren kann, wie sie integrierte apps (z. B. Telefon) Funktionsparität bereitstellt und wie sie eine app Sichtbarkeit in der gesamten iOS an Speicherorten wie z. B. die Sperre und Startseite Bildschirme, über Siri Interaktionen und über erhöhen die Kontakte-apps.



## <a name="related-links"></a>Verwandte Links

- [iOS-10-Beispiele](https://developer.xamarin.com/samples/ios/iOS10/)
