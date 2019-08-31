---
title: Callkit in xamarin. IOS
description: In diesem Artikel wird die neue callkit-API behandelt, die Apple in ios 10 veröffentlicht hat und wie Sie in xamarin. IOS VoIP-Apps implementiert wird.
ms.prod: xamarin
ms.assetid: 738A142D-FFD2-4738-B3ED-57C273179848
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/15/2017
ms.openlocfilehash: 92e4cd45a7fe49a7a78a8922bf70ac87870db095
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2019
ms.locfileid: "70200402"
---
# <a name="callkit-in-xamarinios"></a>Callkit in xamarin. IOS

Die neue callkit-API in ios 10 bietet eine Möglichkeit für VoIP-Apps, mit der iPhone-Benutzeroberfläche zu integrieren und eine vertraute Schnittstelle und Benutzeroberfläche für den Endbenutzer bereitzustellen. Mit dieser API können Benutzer VoIP-Anrufe über den Sperrbildschirm des IOS-Geräts anzeigen und mit ihnen interagieren und Kontakte mithilfe der **Favoriten** -und Wiederholungs Ansichten der Phone-App verwalten.

## <a name="about-callkit"></a>Informationen über callkit

Laut Apple handelt es sich bei callkit um ein neues Framework, mit dem Sie VoIP-Apps (Voice over IP) von Drittanbietern auf eine erste Seite mit IOS 10 herauf Stufen können. Mit der callkit-API können VoIP-Apps in die iPhone-Benutzeroberfläche integriert werden und eine vertraute Oberfläche für den Endbenutzer bereitstellen. Ebenso wie die integrierte Phone-App kann ein Benutzer VoIP-Anrufe über den Sperrbildschirm des IOS-Geräts anzeigen und mit ihm interagieren und Kontakte mithilfe der **Favoriten** -und Wiederholungs Ansichten der Phone-App verwalten.

Außerdem bietet die callkit-API die Möglichkeit, App-Erweiterungen zu erstellen, die eine Telefonnummer mit einem Namen (Aufrufer-ID) verknüpfen oder dem System mitteilen können, wenn eine Zahl blockiert werden soll (Aufruf Sperre).

### <a name="the-existing-voip-app-experience"></a>Die vorhandene VoIP-App-Funktion

Vor der Erörterung der neuen callkit-API und ihrer Fähigkeiten sollten Sie sich die aktuelle Benutzerumgebung mit einer Drittanbieter-VoIP-app in ios 9 (und weniger) mit einer fiktiven VoIP-App namens monkeycallansehen. Monkeycallist eine einfache APP, die es dem Benutzer ermöglicht, mithilfe der vorhandenen IOS-APIs VoIP-Anrufe zu senden und zu empfangen.

Wenn der Benutzer derzeit einen eingehenden Rückruf für monkeycallempfängt und sein iPhone gesperrt ist, kann die auf dem Sperrbildschirm empfangene Benachrichtigung nicht von anderen Benachrichtigungs Typen (z. b. von den Nachrichten oder Mail-Apps) unterschieden werden.

Wenn der Benutzer den Anruf beantworten wollte, musste er die monkeycallbenachrichtigung verschieben, um die APP zu öffnen, und seine Kennung (oder Fingereingabe-ID) eingeben, um das Telefon zu entsperren, bevor es den Anruf annehmen und die Konversation starten konnte.

Diese Vorgehensweise ist gleichermaßen mühsam, wenn das Smartphone entsperrt ist. Auch hier wird der eingehende monkeycallanrufe als standardmäßiges Benachrichtigungs Banner angezeigt, das vom oberen Bildschirmrand aus bewegt wird. Da es sich um eine temporäre Benachrichtigung handelt, kann der Benutzer leicht übersehen, dass Sie das Benachrichtigungs Center öffnen und die jeweilige Benachrichtigung finden, die Sie beantworten möchte, um die monkeycallapp manuell aufzurufen und zu starten.

### <a name="the-callkit-voip-app-experience"></a>Die callkit VoIP-App-Funktion

Durch Implementieren der neuen callkit-APIs in der monkeycallapp kann die Benutzer Darstellung eines eingehenden VoIP-Aufrufes in ios 10 erheblich verbessert werden. Nehmen Sie beispielsweise an, dass der Benutzer einen VoIP-Anruf erhält, wenn sein Telefon von oben gesperrt ist. Durch die Implementierung von callkit wird der Aufruf auf dem Sperrbildschirm des iPhones angezeigt, ebenso wie bei Empfang des Aufrufs von der integrierten Phone-App mit dem voll Bildschirm, der nativen Benutzeroberfläche und der standardmäßigen Swipe-zu-Antwort-Funktionalität.

Wenn das iPhone beim Empfang eines monkeycallvoip-Aufrufs entsperrt wird, werden die gleichen Vollbild-, nativen Benutzeroberflächen-und standardmäßigen Swipe-zu-Antwort-und Tap-to-ablehnen-Funktionen der integrierten Phone-App angezeigt, und monkeycallhat die Möglichkeit, einen benutzerdefinierten Rington zu spielen. .

Callkit stellt zusätzliche Funktionen für monkeycallbereit und ermöglicht es den VoIP-aufrufen, mit anderen Arten von aufrufen zu interagieren, die in den integrierten und bevorzugten Listen angezeigt werden, um die integrierten Features "nicht stören" und "blockieren" zu verwenden, die integrierten monkeycallaufrufe von Siri und bietet Benutzern die Möglichkeit, monkeycallanrufe an Personen in der Kontakte-App zuzuweisen.

In den folgenden Abschnitten werden die callkit-Architektur, die eingehenden und ausgehenden Aufruf Flüsse und die callkit-API ausführlich behandelt.

## <a name="the-callkit-architecture"></a>Die callkit-Architektur

In ios 10 hat Apple callkit in allen System Diensten übernommen, sodass Aufrufe von carplay beispielsweise der System Benutzeroberfläche über callkit bekannt sind. Da monkeycallcallkit in dem unten angegebenen Beispiel annimmt, ist es dem System auf die gleiche Weise bekannt wie diese integrierten System Dienste und erhält die gleichen Features:

[![](callkit-images/callkit01.png "Der callkit-Dienst Stapel")](callkit-images/callkit01.png#lightbox)

Sehen Sie sich die monkeycallapp aus dem obigen Diagramm genauer an. Die app enthält den gesamten Code für die Kommunikation mit Ihrem eigenen Netzwerk und enthält eigene Benutzeroberflächen. Er verknüpft den callkit, um mit dem System zu kommunizieren:

[![](callkit-images/callkit02.png "Monkeycallapp-Architektur")](callkit-images/callkit02.png#lightbox)

Es gibt zwei Haupt Schnittstellen in callkit, die von der APP verwendet werden:

- `CXProvider`-Dadurch kann die monkeycallapp das System über alle ggf. auftretenden out-of-Band-Benachrichtigungen informieren.
- `CXCallController`-Ermöglicht der monkeycallapp, das System über lokale Benutzeraktionen zu informieren.

### <a name="the-cxprovider"></a>Der cxprovider

Wie bereits erwähnt, `CXProvider` ermöglicht es einer APP, das System über alle möglicherweise auftretenden out-of-Band-Benachrichtigungen zu informieren. Hierbei handelt es sich um Benachrichtigungen, die nicht aufgrund von lokalen Benutzeraktionen auftreten, sondern aufgrund externer Ereignisse wie z. b. eingehender Anrufe auftreten.

Eine APP sollte `CXProvider` für Folgendes verwenden:

- Einen eingehenden System Rückruf melden.
- Melden Sie einen, dass der ausgehende-Rückruf mit dem System verbunden ist.
- Melden Sie den Remote Benutzer, der den System Rückruf beendet.

Wenn die APP mit dem System kommunizieren möchte, verwendet Sie die `CXCallUpdate` -Klasse, und wenn das System mit der APP kommunizieren muss, verwendet es die `CXAction` -Klasse:

[![](callkit-images/callkit03.png "Kommunikation mit dem System über einen cxprovider")](callkit-images/callkit03.png#lightbox)

### <a name="the-cxcallcontroller"></a>Cxcallcontroller

`CXCallController` Ermöglicht einer APP, das System über lokale Benutzeraktionen zu informieren, z. b. den Benutzer, der einen VoIP-Befehl startet. Durch Implementieren `CXCallController` von wird die APP für das Zusammenwirken mit anderen Typen von Aufrufen im System ausgeführt. Wenn z. b. bereits ein aktiver telefonieanruf ausgeführt wird, `CXCallController` kann der VoIP-APP das Platzieren dieses Aufrufes erlauben und einen VoIP-Anruf starten oder beantworten.

Eine APP sollte `CXCallController` für Folgendes verwenden:

- Meldet, wenn der Benutzer einen ausgehenden System Aufrufvorgang gestartet hat.
- Melden Sie sich an, wenn der Benutzer einen eingehenden System Rückruf beantwortet.
- Melden Sie sich an, wenn der Benutzer einen System Rückruf beendet.

Wenn die APP lokale Benutzeraktionen an das System übermitteln möchte, verwendet Sie die `CXTransaction` -Klasse:

[![](callkit-images/callkit04.png "Übermitteln von Berichten an das System mit einem cxcallcontroller")](callkit-images/callkit04.png#lightbox)

## <a name="implementing-callkit"></a>Implementieren von callkit

In den folgenden Abschnitten wird gezeigt, wie Sie callkit in einer xamarin. IOS VoIP-App implementieren. Zum Beispiel wird in diesem Dokument Code aus der fiktiven monkeycallvoip-App verwendet. Der hier dargestellte Code stellt mehrere unterstützende Klassen dar, die in den folgenden Abschnitten ausführlich behandelt werden.

### <a name="the-activecall-class"></a>Die activecall-Klasse

Die `ActiveCall` -Klasse wird von der monkeycallapp verwendet, um alle Informationen über einen VoIP-Befehl aufzunehmen, der derzeit wie folgt aktiv ist:

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

`ActiveCall`enthält mehrere Eigenschaften, die den Status des Aufrufes definieren, und zwei Ereignisse, die ausgelöst werden können, wenn sich der Status des Aufrufes ändert. Da es sich nur um ein Beispiel handelt, gibt es drei Methoden zum Simulieren des Starts, zum beantworten und Beenden eines Aufrufes.

### <a name="the-startcallrequest-class"></a>Die startcallrequest-Klasse

Die `StartCallRequest` statische-Klasse stellt einige Hilfsmethoden bereit, die beim Arbeiten mit ausgehenden Aufrufen verwendet werden:

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

Die `CallHandleFromURL` Klassen `CallHandleFromActivity` und werden im appdelegaten zum Abrufen des Kontakt Handles der Person verwendet, die in einem ausgehenden Aufruf aufgerufen wird. Weitere Informationen finden Sie weiter unten im Abschnitt [Verarbeiten von ausgehenden Aufrufen](#handling-outgoing-calls) .

### <a name="the-activecallmanager-class"></a>Die activecallmanager-Klasse

Die `ActiveCallManager` -Klasse verarbeitet alle geöffneten Aufrufe in der monkeycallapp.

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

Da dies nur eine Simulation ist, verwaltet `ActiveCallManager` nur eine Auflistung von `ActiveCall` -Objekten und verfügt über eine Routine, mit der ein angegebener- `UUID` Befehl über seine-Eigenschaft gefunden werden kann. Sie enthält außerdem Methoden zum Starten, beenden und Ändern des Zustands eines ausgehenden Aufrufes. Weitere Informationen finden Sie weiter unten im Abschnitt [Verarbeiten von ausgehenden Aufrufen](#handling-outgoing-calls) .

### <a name="the-providerdelegate-class"></a>Die providerdelegatklasse

Wie bereits erläutert, bietet `CXProvider` eine bidirektionale Kommunikation zwischen der APP und dem System für Out-of-Band-Benachrichtigungen. Der Entwickler muss ein benutzerdefiniertes `CXProviderDelegate` bereitstellen und an das `CXProvider` anfügen, damit die app out-of-Band-callkit-Ereignisse verarbeiten kann. Monkeycallverwendet Folgendes `CXProviderDelegate`:

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

            // Get Image Template
            var templateImage = UIImage.FromFile ("telephone_receiver.png");

            // Setup the initial configurations
            Configuration = new CXProviderConfiguration ("MonkeyCall") {
                MaximumCallsPerCallGroup = 1,
                SupportedHandleTypes = new NSSet<NSNumber> (handleTypes),
                IconTemplateImageData = templateImage.AsPNG(),
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
                    Provider.ReportConnectingOutgoingCall (call.UUID, call.StartedConnectingOn.ToNSDate());
                }
            };

            activeCall.ConnectedChanged += (call) => {
                if (call.isConnected) {
                    // Inform system that the call has connected
                    provider.ReportConnectedOutgoingCall (call.UUID, call.ConnectedOn.ToNSDate ());
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

Wenn eine Instanz dieses Delegaten erstellt wird, wird die an Sie `ActiveCallManager` übermittelt, die für die Behandlung von Aufruf Aktivitäten verwendet wird. Anschließend werden die Handle-Typen (`CXHandleType`) definiert, auf die der `CXProvider` antwortet:

```csharp
// Define handle types
var handleTypes = new [] { (NSNumber)(int)CXHandleType.PhoneNumber };
```

Außerdem wird das Vorlagen Image abgerufen, das auf das App-Symbol angewendet wird, wenn ein-Vorgang ausgeführt wird:

```csharp
// Get Image Template
var templateImage = UIImage.FromFile ("telephone_receiver.png");
```

Diese Werte werden in einem `CXProviderConfiguration` gebündelt, das zum `CXProvider`Konfigurieren von verwendet wird:

```csharp
// Setup the initial configurations
Configuration = new CXProviderConfiguration ("MonkeyCall") {
    MaximumCallsPerCallGroup = 1,
    SupportedHandleTypes = new NSSet<NSNumber> (handleTypes),
    IconTemplateImageData = templateImage.AsPNG(),
    RingtoneSound = "musicloop01.wav"
};
```

Der Delegat erstellt dann mit `CXProvider` diesen Konfigurationen einen neuen und fügt ihn an ihn an:

```csharp
// Create a new provider
Provider = new CXProvider (Configuration);

// Attach this delegate
Provider.SetDelegate (this, null);
```

Wenn Sie callkit verwenden, erstellt und verarbeitet die APP keine eigenen audiositzungen mehr, sondern muss eine Audiositzung konfigurieren und verwenden, die vom System erstellt und verarbeitet wird. 

Wenn dies eine echte App wäre, wird `DidActivateAudioSession` die-Methode verwendet, um den-Befehl mit einem vorkonfigurierten `AVAudioSession` zu starten, den das System bereitgestellt hat:

```csharp
public override void DidActivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
{
    // Start the call's audio session here...
}
```

Außerdem wird die `DidDeactivateAudioSession` -Methode verwendet, um die Verbindung mit der vom System bereitgestellten Audiositzung abzuschließen und freizugeben:

```csharp
public override void DidDeactivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
{
    // End the calls audio session and restart any non-call
    // releated audio
}
```

Der restliche Code wird in den folgenden Abschnitten ausführlich behandelt.

### <a name="the-appdelegate-class"></a>Die appdelegatklasse

Monkeycallverwendet den appdelegaten zum Speichern von Instanzen `ActiveCallManager` von `CXProviderDelegate` und, die in der gesamten App verwendet werden:

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

Die `OpenUrl` - `ContinueUserActivity` Methode und die-Überschreibungs Methode werden verwendet, wenn die APP einen ausgehenden-Befehl verarbeitet. Weitere Informationen finden Sie weiter unten im Abschnitt [Verarbeiten von ausgehenden Aufrufen](#handling-outgoing-calls) .

## <a name="handling-incoming-calls"></a>Behandeln eingehender Aufrufe

Es gibt mehrere Zustände und Prozesse, die ein eingehender VoIP-Rückruf während eines typischen eingehenden Workflow Workflows durchlaufen kann, wie z. b.:

- Informieren des Benutzers (und des Systems), dass ein eingehender-Rückruf vorhanden ist.
- Empfangen einer Benachrichtigung, wenn der Benutzer den Anruf beantworten und den Anruf mit dem anderen Benutzer initialisieren möchte.
- Informieren Sie das System und das Kommunikationsnetzwerk, wenn der Benutzer den aktuellen-Befehl beenden möchte.

In den folgenden Abschnitten wird ausführlich erläutert, wie eine APP callkit zum Verarbeiten des eingehenden Aufruf Workflows verwenden kann, wobei die monkeycallvoip-App als Beispiel verwendet wird.

### <a name="informing-user-of-incoming-call"></a>Informieren des Benutzers über eingehenden Anrufe

Wenn ein Remote Benutzer eine VoIP-Konversation mit dem lokalen Benutzer gestartet hat, geschieht Folgendes:

[![](callkit-images/callkit05.png "Ein Remote Benutzer hat eine VoIP-Konversation gestartet.")](callkit-images/callkit05.png#lightbox)

1. Die APP erhält eine Benachrichtigung von Ihrem Kommunikationsnetzwerk, dass ein eingehender VoIP-Rückruf vorliegt.
2. Die APP verwendet das `CXProvider` , um eine `CXCallUpdate` an das System zu senden, das Sie über den-Befehl informiert.
3. Das System veröffentlicht den Aufruf der Systembenutzer Oberfläche, der Systemdienste und aller anderen VoIP-Apps mithilfe von callkit.

Beispielsweise in `CXProviderDelegate`:

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

Mit diesem Code wird eine `CXCallUpdate` neue-Instanz erstellt und ein Handle an das Objekt angehängt, das den Aufrufer identifiziert. Anschließend wird die- `ReportNewIncomingCall` Methode `CXProvider` der-Klasse verwendet, um das System über den-Befehl zu informieren. Wenn dies erfolgreich ist, wird der Aufruf der APP-Auflistung aktiver Aufrufe hinzugefügt, wenn dies nicht der Fall ist. der Fehler muss dem Benutzer gemeldet werden.

### <a name="user-answering-incoming-call"></a>Benutzer beantwortete eingehenden Anruf

Wenn der Benutzer den eingehenden VoIP-Anruf beantworten möchte, geschieht Folgendes:

[![](callkit-images/callkit06.png "Der Benutzer antwortet auf den eingehenden VoIP-Rückruf.")](callkit-images/callkit06.png#lightbox)

1. Die Benutzeroberfläche des Systems informiert das System darüber, dass der Benutzer den VoIP-Anruf beantworten möchte.
2. Das System sendet eine `CXAnswerCallAction` an die `CXProvider` APP, die Sie über die Antwort Absicht informiert.
3. Die APP informiert das Kommunikationsnetzwerk, dass der Benutzer den Anruf beantwortet, und der VoIP-Anruf wird wie gewohnt fortgesetzt.

Beispielsweise in `CXProviderDelegate`:

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

Dieser Code sucht zuerst in der Liste der aktiven Aufrufe nach dem angegebenen Aufruf. Wenn der-Befehl nicht gefunden werden kann, wird das System benachrichtigt und die Methode beendet. Wenn Sie gefunden wird, wird `AnswerCall` die-Methode `ActiveCall` der-Klasse aufgerufen, um den Aufruf zu starten, und das System ist, wenn es erfolgreich ist oder fehlschlägt.

### <a name="user-ending-incoming-call"></a>Benutzer Ende eingehender Anrufe

Wenn der Benutzer den-Befehl in der Benutzeroberfläche der APP beenden möchte, geschieht Folgendes:

[![](callkit-images/callkit07.png "Der Benutzer beendet den-Befehl in der Benutzeroberfläche der app.")](callkit-images/callkit07.png#lightbox)

1. Die App erstellt `CXEndCallAction` , die in einem `CXTransaction` gebündelt wird, das an das System gesendet wird, um Sie darüber zu informieren, dass der-Vorgang beendet wird.
2. Das System überprüft die Absicht des endanrufens und `CXEndCallAction` sendet die zurück an die APP `CXProvider`über den.
3. Die APP informiert das Kommunikationsnetzwerk dann darüber, dass der-Endpunkt beendet wird.

Beispielsweise in `CXProviderDelegate`:

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

Dieser Code sucht zuerst in der Liste der aktiven Aufrufe nach dem angegebenen Aufruf. Wenn der-Befehl nicht gefunden werden kann, wird das System benachrichtigt und die Methode beendet. Wenn Sie gefunden wird, wird `EndCall` die-Methode `ActiveCall` der-Klasse aufgerufen, um den Aufruf zu beenden, und das System enthält Informationen, wenn es erfolgreich ist oder fehlschlägt. Bei erfolgreicher Ausführung wird der Aufruf aus der Sammlung aktiver Aufrufe entfernt.

## <a name="managing-multiple-calls"></a>Verwalten mehrerer Aufrufe

Die meisten VoIP-Apps können mehrere Aufrufe gleichzeitig verarbeiten. Wenn z. b. zurzeit ein aktiver VoIP-Anruf vorliegt und die APP benachrichtigt wird, dass ein neuer eingehender Anruf vorliegt, kann der Benutzer beim ersten Anruf anhalten oder darauf warten, um den zweiten Anruf zu beantworten.

In der obigen Situation sendet das System eine `CXTransaction` an die APP, die eine Liste mehrerer Aktionen (z `CXEndCallAction` . b. und `CXAnswerCallAction`) enthält. Alle diese Aktionen müssen einzeln ausgeführt werden, damit das System die Benutzeroberfläche entsprechend aktualisieren kann.

## <a name="handling-outgoing-calls"></a>Behandeln von ausgehenden Aufrufen

Wenn der Benutzer auf einen Eintrag aus der Liste der Empfänger (in der Phone-APP) tippt (z. b. aus einem Anruf, der zur APP gehört), wird er vom System zu einer _Start Anruf Absicht_ gesendet:

[![](callkit-images/callkit08.png "Empfangen einer beabsichtigte Start Absicht")](callkit-images/callkit08.png#lightbox)

1. Die App erstellt eine _Aktion zum Starten des Aufrufes_ auf der Grundlage der Start-callanabsicht, die Sie vom System erhalten hat. 
2. Die APP verwendet das `CXCallController` , um die Aktion zum Starten des Aufrufes vom System anzufordern.
3. Wenn das System die Aktion annimmt, wird es über den `XCProvider` Delegaten an die APP zurückgegeben.
4. Die APP startet den ausgehenden-Rückruf mit dem Kommunikationsnetzwerk.

Weitere Informationen zu Intents finden Sie in unserer Dokumentation zu Intents [und Intents UI Extensions](~/ios/platform/sirikit/understanding-sirikit.md) . 

### <a name="the-outgoing-call-lifecycle"></a>Der Lebenszyklus der ausgehenden Aufrufe

Bei der Arbeit mit callkit und einem ausgehenden Aufruf muss die APP das System über die folgenden Lebenszyklus Ereignisse informieren:

1. Wird **gestartet** : informieren Sie das System, dass ein ausgehender-Rückruf gestartet wird.
2. **Gestartet** : informieren Sie das System, dass ein ausgehender-Befehl gestartet wurde.
3. **Verbindung** wird hergestellt: informieren Sie das System, dass der ausgehende Verbindungsaufbau eine Verbindung herstellt
4. **Verbunden** -informieren Sie, dass der ausgehende-Befehl eine Verbindung hergestellt hat und dass beide Parteien jetzt kommunizieren können.

Der folgende Code startet z. b. einen ausgehenden-Befehl:

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

Es wird ein `CXHandle` erstellt und verwendet, um eine `CXStartCallAction` zu konfigurieren, die in `CXTransaction` einem gebündelt ist, das mithilfe der `RequestTransaction` -Methode der- `CXCallController` Klasse an das System gesendet wird. Durch Aufrufen der `RequestTransaction` -Methode kann das System alle vorhandenen Aufrufe auf dem standhalten, unabhängig von der Quelle (Phone-APP, Facetime, VoIP usw.), bevor der neue Aufruf gestartet wird.

Die Anforderung zum Starten eines ausgehenden VoIP-Aufrufs kann aus verschiedenen Quellen stammen, z. b. Siri, einem Eintrag auf einer Kontaktkarte (in der App "Kontakte") oder aus der Liste der wieder herzuführenden (in der Telefon-APP). In diesen Fällen wird der App eine Start Aufruf Absicht innerhalb `NSUserActivity` von gesendet, und der appdelegat muss Sie behandeln:

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

Hier wird `CallHandleFromActivity` die-Methode der- `StartCallRequest` Hilfsklasse verwendet, um das Handle für die aufgerufene Person zu erhalten (siehe die oben genannte [startcallrequest-Klasse](#the-startcallrequest-class) ).

Die `PerformStartCallAction` -Methode der [providerdelegatklasse](#the-providerdelegate-class) wird verwendet, um den eigentlichen ausgehenden Aufruf zu starten und das System über seinen Lebenszyklus zu informieren:

```csharp
public override void PerformStartCallAction (CXProvider provider, CXStartCallAction action)
{
    // Create new call record
    var activeCall = new ActiveCall (action.CallUuid, action.CallHandle.Value, true);

    // Monitor state changes
    activeCall.StartingConnectionChanged += (call) => {
        if (call.IsConnecting) {
            // Inform system that the call is starting
            Provider.ReportConnectingOutgoingCall (call.UUID, call.StartedConnectingOn.ToNSDate());
        }
    };

    activeCall.ConnectedChanged += (call) => {
        if (call.IsConnected) {
            // Inform system that the call has connected
            Provider.ReportConnectedOutgoingCall (call.UUID, call.ConnectedOn.ToNSDate ());
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

Er erstellt eine Instanz der `ActiveCall` -Klasse (zum Speichern von Informationen über den Aufruf in Bearbeitung) und füllt mit der Person, die aufgerufen wird. Das `StartingConnectionChanged` - `ConnectedChanged` Ereignis und das-Ereignis werden verwendet, um den Lebenszyklus des ausgehenden Aufrufes Der-Befehl wird gestartet, und das System informierte, dass die Aktion erfüllt wurde.

### <a name="ending-an-outgoing-call"></a>Beenden eines ausgehenden Aufrufes

Wenn der Benutzer einen ausgehenden-Befehl beendet hat und ihn beenden möchte, kann der folgende Code verwendet werden:

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

Wenn eine `CXEndCallAction` - `CXCallController` Klasse mit der UUID für den End-End-Befehl erstellt, `CXTransaction` bündeln Sie Sie in einem, der mithilfe `RequestTransaction` der-Methode der-Klasse an das System gesendet wird. 

## <a name="additional-callkit-details"></a>Weitere callkit-Details

In diesem Abschnitt werden einige zusätzliche Details behandelt, die der Entwickler bei der Arbeit mit callkit berücksichtigen muss, wie z. b.:

- Anbieter Konfiguration
- Aktions Fehler
- System Einschränkungen
- VoIP-Audiodatei

### <a name="provider-configuration"></a>Anbieter Konfiguration

Die Anbieter Konfiguration ermöglicht einer IOS 10 VoIP-APP das Anpassen der Benutzeroberfläche (innerhalb der systemeigenen Benutzeroberfläche in der aufrufenden Benutzeroberfläche) bei der Arbeit mit callkit.

Eine APP kann die folgenden Arten von Anpassungen vornehmen:

- Zeigen Sie einen lokalisierten Namen an.
- Unterstützung für Videoanrufe aktivieren.
- Passen Sie die Schaltflächen auf der Benutzeroberfläche in der Benutzeroberfläche an, indem Sie ein eigenes Vorlagen Bildsymbol darstellen. Benutzerinteraktionen mit benutzerdefinierten Schaltflächen werden direkt an die zu verarbeitende App gesendet. 

### <a name="action-errors"></a>Aktions Fehler

IOS 10 VoIP-Apps, die callkit verwenden, müssen Aktionen behandeln, die fehlerhaft sind, und den Benutzer jederzeit über den Aktionszustand informieren. 

Beachten Sie Folgendes Beispiel:

1. Die APP hat eine Aktion zum Starten des Aufrufes empfangen und begonnen, den Prozess der Initialisierung eines neuen VoIP-Aufrufes mit dem Kommunikationsnetzwerk zu starten.
2. Aufgrund der eingeschränkten oder keinen Netzwerk Kommunikationsfunktion tritt bei dieser Verbindung ein Fehler auf.
3. Die APP *muss* die Fehlermeldung zurück an die Aktion zum Starten des Aufrufens ( `Action.Fail()`) senden, um das System über den Fehler zu informieren.
4. Dadurch kann das System den Benutzer über den Status des Aufrufes informieren. Beispielsweise, um die Benutzeroberfläche für den Aufrufen eines Fehlers anzuzeigen.

Darüber hinaus muss eine IOS 10 VoIP-App auf _Timeout Fehler_ reagieren, die auftreten können, wenn eine erwartete Aktion nicht innerhalb eines bestimmten Zeitraums verarbeitet werden kann. Jedem Aktionstyp, der von callkit bereitgestellt wird, ist ein maximaler Timeout Wert zugeordnet. Diese Timeout Werte stellen sicher, dass jede vom Benutzer angeforderte callkit-Aktion reaktionsfähig verarbeitet wird, sodass das Betriebssystem auch flüssig und reaktionsfähig bleibt.

Es gibt mehrere Methoden für den Anbieter Delegaten`CXProviderDelegate`(), die überschrieben werden sollten, um diese Timeout Situationen ordnungsgemäß behandeln zu können.

### <a name="system-restrictions"></a>System Einschränkungen

Basierend auf dem aktuellen Status des IOS-Geräts, auf dem die IOS 10 VoIP-app ausgeführt wird, können bestimmte Systemeinschränkungen erzwungen werden.

Beispielsweise kann ein eingehender VoIP-Rückruf durch das System eingeschränkt werden, wenn Folgendes gilt:

1. Die Person, die aufruft, ist in der Liste der gesperrten Aufrufer des Benutzers
2. Das IOS-Gerät des Benutzers befindet sich im Modus "do-not-stört".

Wenn ein VoIP-Rückruf in einer dieser Situationen eingeschränkt ist, verwenden Sie den folgenden Code, um ihn zu behandeln:

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

### <a name="voip-audio"></a>VoIP-Audiodatei

Callkit bietet verschiedene Vorteile für die Handhabung der Audioressourcen, die eine IOS 10 VoIP-APP während eines Live-VoIP-Aufrufes erfordert. Einer der größten Vorteile ist, dass die Audiositzung der App bei Ausführung in ios 10 höhere Prioritäten hat. Dies ist dieselbe Prioritätsstufe wie die integrierten Phone-und FaceTime-apps, und diese erweiterte Prioritätsstufe verhindert, dass andere laufende Apps die Audiositzung der VoIP-App unterbrechen.

Außerdem verfügt callkit über Zugriff auf andere audioroutinghinweise, die die Leistung verbessern und auf intelligente Weise VoIP-Audiodaten an bestimmte Ausgabegeräte während eines Live-Aufrufes basierend auf Benutzereinstellungen und Geräte Zuständen weiterleiten können. Beispielsweise basierend auf angefügten Geräten, z. b. Bluetooth-Kopfhörern, einer Live-carplay-Verbindung oder Barrierefreiheits Einstellungen.

Während des Lebenszyklus eines typischen VoIP-Aufrufes mit callkit muss die APP den Audiostream konfigurieren, der von callkit bereitgestellt wird. Sehen Sie sich das folgende Beispiel an:

[![](callkit-images/callkit09.png "Die Aktions Sequenz für den Start Vorgang.")](callkit-images/callkit09.png#lightbox)

1. Eine Aktion zum Starten des Aufrufes wird von der APP empfangen, um einen eingehenden Anruf zu beantworten.
2. Bevor diese Aktion von der APP erfüllt wird, wird die Konfiguration bereitstellt, die für `AVAudioSession`die erforderlich ist.
3. Die APP informiert das System darüber, dass die Aktion erfüllt wurde.
4. Bevor der Aufruf eine Verbindung herstellt, bietet callkit eine hohe `AVAudioSession` Priorität, die der von der APP angeforderten Konfiguration entspricht. Die APP wird über die `DidActivateAudioSession` -Methode `CXProviderDelegate`von benachrichtigt.

## <a name="working-with-call-directory-extensions"></a>Arbeiten mit Verzeichnis Erweiterungen für Anrufe

Beim Arbeiten mit callkit bieten _Aufruf Verzeichnis Erweiterungen_ eine Möglichkeit zum Hinzufügen von blockierten Aufruf Nummern und Identifizieren von Zahlen, die für eine bestimmte VoIP-App spezifisch sind, für Kontakte in der Kontakt-APP auf dem IOS-Gerät.

### <a name="implementing-a-call-directory-extension"></a>Implementieren einer Erweiterung für das Erweiterungs Verzeichnis

Gehen Sie folgendermaßen vor, um eine Erweiterung des Callcenter in einer xamarin. IOS-APP zu implementieren:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Öffnen Sie die Projekt Mappe der app in Visual Studio für Mac.
2. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektmappennamen, und wählen Sie **hinzu** > fügen**Neues Projekt**hinzufügen
3. Wählen Sie **IOS** > **Extensions** > **Aufrufe Verzeichnis Erweiterungen** aus, und klicken Sie auf die Schaltfläche **weiter** : 

    [![](callkit-images/calldir01.png "Erstellen einer neuen Erweiterung für das Erweiterungs Verzeichnis")](callkit-images/calldir01.png#lightbox)
4. Geben Sie einen **Namen** für die Erweiterung ein, und klicken Sie auf die Schaltfläche **weiter** : 

    [![](callkit-images/calldir02.png "Eingeben eines Namens für die Erweiterung")](callkit-images/calldir02.png#lightbox)
5. Passen Sie den **Projektnamen** und/ oder Projektmappennamen bei Bedarf an, und klicken Sie auf die Schaltfläche **Erstellen** 

    [![](callkit-images/calldir03.png "Erstellen des Projekts")](callkit-images/calldir03.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Öffnen Sie die Projekt Mappe der app in Visual Studio.
2. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektmappennamen, und wählen Sie **hinzu** > fügen**Neues Projekt**hinzufügen
3. Wählen Sie **IOS** > **Extensions** > **Aufrufe Verzeichnis Erweiterungen** aus, und klicken Sie auf die Schaltfläche **weiter** : 

    [![](callkit-images/calldir01w.png "Erstellen einer neuen Erweiterung für das Erweiterungs Verzeichnis")](callkit-images/calldir01.png#lightbox)
4. Geben Sie einen **Namen** für die Erweiterung ein, und klicken Sie auf **OK** .

-----

Dadurch wird dem Projekt `CallDirectoryHandler.cs` eine Klasse hinzugefügt, die wie folgt aussieht:

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

Die `BeginRequest` -Methode im Handler für den Callcenter muss geändert werden, um die erforderliche Funktionalität bereitzustellen. Im obigen Beispiel wird versucht, die Liste der blockierten und verfügbaren Nummern in der Contact-Datenbank der VoIP-App festzulegen. Wenn beide Anforderungen aus irgendeinem Grund fehlschlagen, erstellen `NSError` Sie einen, um den Fehler zu beschreiben `CancelRequest` , und übergeben `CXCallDirectoryExtensionContext` Sie ihm die-Methode der-Klasse.

Verwenden Sie die `AddBlockingEntry` -Methode `CXCallDirectoryExtensionContext` der-Klasse, um die blockierten Nummern festzulegen. Die für die Methode bereitgestellten Zahlen _müssen_ in numerischer aufsteigender Reihenfolge vorliegen. Für eine optimale Leistung und Arbeitsspeicher Auslastung, wenn viele Telefonnummern vorhanden sind, sollten Sie nur eine Teilmenge der Zahlen zu einem bestimmten Zeitpunkt laden und einen autorelease-Pool (s) zum Freigeben von Objekten verwenden, die während der geladenen Batches von Zahlen zugeordnet werden.

Verwenden Sie die `AddIdentificationEntry` -Methode `CXCallDirectoryExtensionContext` der-Klasse, und geben Sie sowohl die Zahl als auch eine identifizierende Bezeichnung an, um die Kontakt-APP über die der VoIP-App bekannten Kontaktnummern zu informieren. Auch hier _müssen_ die für die Methode bereitgestellten Zahlen in numerischer aufsteigender Reihenfolge angegeben werden. Für eine optimale Leistung und Arbeitsspeicher Auslastung, wenn viele Telefonnummern vorhanden sind, sollten Sie nur eine Teilmenge der Zahlen zu einem bestimmten Zeitpunkt laden und einen autorelease-Pool (s) zum Freigeben von Objekten verwenden, die während der geladenen Batches von Zahlen zugeordnet werden.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde die neue callkit-API behandelt, die Apple in ios 10 veröffentlicht hat und wie Sie in xamarin. IOS VoIP-Apps implementiert wird. Es wurde gezeigt, wie callkit es einer App ermöglicht, in das IOS-System zu integrieren, wie die Funktions Parität mit integrierten Apps (z. b. Telefon) bereitgestellt wird und wie die Sichtbarkeit einer APP über IOS an Standorten wie den Sperr-und Start Bildschirmen, über Siri-Interaktionen und Via ermöglicht wird. die Kontakte-apps.

## <a name="related-links"></a>Verwandte Links

- [IOS 10-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
