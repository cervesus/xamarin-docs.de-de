---
title: SiriKit-Updates in iOS 11
description: In diesem Dokument wird beschrieben, wie in iOS 11 SiriKit eingesetzt wird. Wird untersucht, insbesondere zum Arbeiten mit Aufgaben und Notizen und alternative Namen für eine Anwendung bereitstellen.
ms.prod: xamarin
ms.assetid: 8F75300B-B591-42ED-9D17-001992A5C381
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/07/2017
ms.openlocfilehash: 7e895dc2865880ec2789a40f8cdf047a20f8693b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50111037"
---
# <a name="sirikit-updates-in-ios-11"></a>SiriKit-Updates in iOS 11

SiriKit wurde in iOS 10, mit einer Anzahl von Service-Domänen (einschließlich fitnessaktivitäten, Fahrt Buchung und Aufrufe) eingeführt. Finden Sie in der [SiriKit Abschnitt](~/ios/platform/sirikit/index.md) SiriKit-Konzepten und Implementieren von SiriKit in Ihrer app.

![Siri Aufgabe Liste demo](sirikit-images/sirikit.png)

SiriKit in iOS 11 fügt diese neuen und aktualisierten beabsichtigten Domänen hinzu:

- [**Listet und Anmerkungen zu dieser** ](#listsnotes) – neue! Stellt eine API für apps, um Aufgaben und Notizen zu verarbeiten.
- **Visual Codes** – neue! Siri kann QR-Codes zum Freigeben von Kontaktinformationen oder Payment-Transaktionen teilnehmen angezeigt werden.
- **Zahlungen** – Absicht "Suchen und Übertragung" für die Zahlung Interaktionen hinzugefügt.
- **Zeigt die Buchung** – hinzugefügten Abbrechen Absicht Fahrt "und" Feedback ".

Zu weiteren neuen Features gehören:

- [**Alternative app-Namen** ](#alternativenames) – stellt Aliase, die Kunden weiterhelfen Teilen Sie Siri Ihrer app verwendet wird, indem Sie alternative Namen/Aussprache anbieten.
- **Starten Fitnessaktivitäten** – bietet die Möglichkeit, ein Thema im Hintergrund zu starten.

Einige dieser Features werden im folgenden erläutert. Weitere Informationen zu den anderen, finden Sie unter [Apple SiriKit Dokumentation](https://developer.apple.com/documentation/sirikit).

<a name="listsnotes" />

## <a name="lists-and-notes"></a>Listen und Anmerkungen zu dieser Version

Die neue Domäne für Listen und Anmerkungen zu dieser Version stellt eine API für apps zur Verarbeitung von Aufgaben und Notizen über Siri-Voice-Anforderungen bereit.

**Aufgaben**

- Haben Sie einen Titel und der Abschlussstatus.
- Optional enthalten Sie einen Stichtag und einen Speicherort aus.

**Notizen**

- Haben Sie einen Titel und eine Inhaltsfeld.

Aufgaben und Notizen können in Gruppen organisiert werden. Im restlichen Teil dieses Abschnitts wird beschrieben, wie diese neue Domäne mit SiriKit, implementieren Sie mithilfe der [TasksNotes SiriKit Beispiel](https://developer.xamarin.com/samples/monotouch/ios11/SiriKitSample/).

### <a name="how-to-process-a-sirikit-request"></a>Gewusst wie: Verarbeiten einer Anforderung SiriKit

Verarbeiten einer Anforderung SiriKit mit folgenden Schritten:

1. **Beheben** – Parameter überprüfen und Weitere Informationen vom Benutzer anfordern (falls erforderlich).
2. **Vergewissern Sie sich** – abschließende Überprüfung und Bestätigung, dass die Anforderung verarbeitet werden kann.
3. **Behandeln** – führen Sie den Vorgang (Aktualisieren von Daten oder Durchführen von Netzwerkoperationen).

Die ersten beiden Schritte sind optional (obwohl empfohlen wird), und der letzte Schritt ist erforderlich.
Es gibt eine ausführlichere Anleitung in der [SiriKit Abschnitt](~/ios/platform/sirikit/index.md).

### <a name="resolve-and-confirm-methods"></a>Beheben, und bestätigen Sie die Methoden

Diese optionalen Methoden können Ihren Code, Validierung, auf Standardwerte oder zusätzliche Informationen vom Benutzer ausgeführt werden.

Als Beispiel für die `IINCreateTaskListIntent` -Schnittstelle, die erforderliche Methode ist `HandleCreateTaskList`. Es gibt vier optionale Methoden, die mehr über die Interaktion von Siri Kontrolle:

- `ResolveTitle` : Überprüft den Titel, definiert einen Standardtitel (falls zutreffend) und signalisiert, dass die Daten nicht erforderlich ist.
- `ResolveTaskTitles` – Überprüft die Liste der Aufgaben, die vom Benutzer gesprochen wird.
- `ResolveGroupName` : Überprüft den Gruppennamen, wählt eine Standardgruppe oder signalisiert, dass die Daten nicht erforderlich ist.
- `ConfirmCreateTaskList` – Überprüft, dass Ihr Code kann den angeforderten Vorgang auszuführen, jedoch er keine führt (nur die `Handle*` Methoden sollten Daten ändern).

### <a name="handle-the-intent"></a>Das Ziel behandeln

Es gibt sechs Intent-Elemente in Listen und Anmerkungen zu dieser Domäne, die drei Aufgaben und drei für Anmerkungen zu dieser Version.
Die Methoden, die Sie implementieren müssen, um diese Intent-Elemente zu verarbeiten sind:

- Für Aufgaben:
  - `HandleAddTasks`
  - `HandleCreateTaskList`
  - `HandleSetTaskAttribute`
- Anmerkungen zu dieser:
  - `HandleCreateNote`
  - `HandleAppendToNote`
  - `HandleSearchForNotebookItems`

Jede Methode verfügt über einen bestimmten beabsichtigte Typ übergeben, die alle Informationen enthält Siri aus die Anforderung des Benutzers analysiert wurde (und möglicherweise aktualisiert werden, der `Resolve*` und `Confirm*` Methoden).
Ihre app muss analysiert die Daten bereitgestellt, und klicken Sie dann einige Aktionen, die zum Speichern oder andernfalls Verarbeiten der Daten ausgeführt werden und ein Ergebnis, das Siri spricht, und zeigt dem Benutzer zurückgegeben.

### <a name="response-codes"></a>Antwortcodes

Die erforderlichen `Handle*` und optionalen `Confirm*` Methoden geben einen Antwortcode von auf das Objekt, das sie übergeben einen Wert festlegen, um ihre Abschlusshandler. Antworten stammen aus der `INCreateTaskListIntentResponseCode` Enumeration:

- `Ready` – Kehrt zurück, während der Phase der Bestätigung (ie. aus einer `Confirm*` -Methode, jedoch nicht von einer `Handle*` Methode).
- `InProgress` – Für lang andauernde Vorgänge (z.B. ein Abfragevorgang mit Netzwerk-Server) verwendet.
- `Success` – Antwortet mit den Details der die erfolgreiche Ausführung (nur von einem `Handle*` Methode).
- `Failure` – Bedeutet, dass ein Fehler aufgetreten ist, und der Vorgang nicht abgeschlossen werden konnte.
- `RequiringAppLaunch` – Von der Absicht nicht verarbeitet werden, aber der Vorgang kann in der app ausgeführt.
- `Unspecified` – Verwenden Sie nicht: Fehlermeldung wird dem Benutzer angezeigt werden.

Weitere Informationen zu diesen Methoden und Antworten in Apple [SiriKit aufgelistet und Anmerkungen zu dieser Dokumentation](https://developer.apple.com/documentation/sirikit/lists_and_notes).

### <a name="implementing-lists-and-notes"></a>Implementieren von Listen und Anmerkungen zu dieser Version

Die [TasksNotes SiriKit Beispiel](https://developer.xamarin.com/samples/monotouch/ios11/SiriKitSample/) verwenden die folgenden Schritte zum Hinzufügen von SiriKit-Unterstützung für eine leere iOS-app erstellt wurde.

Zunächst zum Hinzufügen der Unterstützung von SiriKit folgendermaßen Sie für Ihre iOS-app:

1. Teilstriche **SiriKit** in **"Entitlements.plist"**.
2. Hinzufügen der **Datenschutz – Nutzungsbeschreibung für Siri** um **"Info.plist"**, zusammen mit einer Nachricht für Ihre Kunden.
3. Rufen Sie die `INPreferences.RequestSiriAuthorization` Methode in der app, um den Benutzer auffordern, Siri-Interaktionen ermöglichen.
4. Fügen Sie Ihrer App-ID im Entwicklerportal SiriKit und erstellen Sie bereitstellungsprofilen Einbeziehung die neue Berechtigung neu zu.

Fügen Sie ein neues Erweiterungsprojekt dann zu Ihrer app zur Verarbeitung von Siri-Anforderungen:

1. Mit der rechten Maustaste auf Ihre Projektmappe, und wählen Sie **hinzufügen > Neues Projekt hinzufügen...** .
2. Wählen Sie die **iOS > Erweiterung > Intents-Erweiterung** Vorlage.
3. Zwei neue Projekte hinzugefügt werden: Intents und IntentUI. Anpassen der Benutzeroberfläche ist optional, damit das Beispiel nur Code in enthält die **Absicht** Projekt.

Das Erweiterungsprojekt ist, in dem alle SiriKit-Anforderungen verarbeitet werden. Als separate Erweiterung ist automatisch keine Möglichkeit, die mit Ihrer Haupt-app – zu kommunizieren, dies in der Regel aufgelöst wird, durch die Implementierung von freigegebenen File Storage mithilfe von app-Gruppen.

#### <a name="configure-the-intenthandler"></a>Konfigurieren Sie die IntentHandler

Die `IntentHandler` Klasse ist der Einstiegspunkt für Siri-Anforderungen – jedes Ziel, um übergeben wird die `GetHandler` Methode, die ein Objekt zurückgibt, der die Anforderung verarbeiten kann.

Der folgende Code zeigt eine einfache Implementierung:

```csharp
[Register("IntentHandler")]
public partial class IntentHandler : INExtension, IINNotebookDomainHandling
{
  protected IntentHandler(IntPtr handle) : base(handle)
  {}
  public override NSObject GetHandler(INIntent intent)
  {
    // This is the default implementation.  If you want different objects to handle different intents,
    // you can override this and return the handler you want for that particular intent.
    return this;
  }
  // add intent handlers here!
}
```

Die Klasse erben muss `INExtension`, und da im Beispiel wird zur Behandlung von Listen und Anmerkungen dieser Intents, es implementiert auch `IINNotebookDomainHandling`.

> [!NOTE]
> - Ist es eine Konvention in .NET für Schnittstellen im wahrsten Sinne Präfix `I`, die Xamarin beim Binden von Protokollen aus dem iOS-SDK entspricht.
> - Xamarin behält auch Namen von iOS und Apple verwendet die ersten beiden Zeichen im Namen entsprechend das Framework, dem ein Typ gehört.
> - Für die `Intents` Framework Typen werden mit dem Präfix `IN*` (z. b. `INExtension`), aber dies sind _nicht_ Schnittstellen.
> - Folgt auch diese Protokolle (die Schnittstellen werden C#) am Ende mit zwei `I`s, wie z. B. `IINAddTasksIntentHandling`.

#### <a name="handling-intents"></a>Verarbeiten von Intents

Jedes Ziel (Hinzufügen von Tasks, Task-Attribut festgelegt usw.) wird in einer einzelnen Methode, die ähnlich wie unten gezeigt implementiert. Die Methode sollte drei Hauptfunktionen auszuführen:

1. **Verarbeiten Sie die Absicht** – die Daten von Siri analysiert im verfügbar gemacht wurde ein `intent` Objekt spezifisch für den Typ der Absicht. Ihre app möglicherweise wurden überprüft diese Daten mit optionalen `Resolve*` Methoden.
2. **Überprüfen und aktualisieren Sie die Datenspeicher** , speichern Sie Daten in das Dateisystem (mit App-Gruppen an, damit die iOS-Haupt-app auch darauf zugreifen kann) oder über eine netzwerkanforderung.
3. **Geben Sie die Antwort** – verwenden der `completion` Handler, der eine Antwort zurück an Siri anzuzeigender Read/an den Benutzer zu senden:

```csharp
public void HandleCreateTaskList(INCreateTaskListIntent intent, Action<INCreateTaskListIntentResponse> completion)
{
  var list = TaskList.FromIntent(intent);
  // TODO: have to create the list and tasks... in your app data store
  var response = new INCreateTaskListIntentResponse(INCreateTaskListIntentResponseCode.Success, null)
  {
    CreatedTaskList = list
  };
  completion(response);
}
```

Beachten Sie, dass `null` wird übergeben, als zweiten Parameter an die Antwort: ist dies der Benutzerparameter-Aktivität, und wenn er nicht angegeben ist, wird ein Standardwert verwendet werden.
Sie können einen Typ für die benutzerdefinierte Aktivität festlegen, solange Ihre iOS-app, über unterstützt die `NSUserActivityTypes` -Schlüssels im **"Info.plist"**. Sie können dann verarbeiten diesen Fall, wenn Ihre app geöffnet wird, und bestimmte Vorgänge (z. B. öffnen, um einen entsprechenden ansichtscontroller, und Laden der Daten aus den Siri-Vorgang) ausführen.

Das Beispiel auch sind die `Success` Ergebnis, aber in realen Szenarios richtigen Fehlerberichterstattung hinzugefügt werden sollen.

### <a name="test-phrases"></a>Testen von Ausdrücken

Die folgenden Test-Sätze sollte in der Beispiel-app funktionieren:

- "Stellen Sie eine Einkaufsliste mit Äpfeln Bananen und Birnen in TasksNotes"
- "Add Task WWDC in TasksNotes"
- "Add Task WWDC Training-Liste in TasksNotes"
- "Mark WWDC als vollständig in TasksNotes teilnehmen"
- "In TasksNotes erinnern Sie, einem Iphone zu erwerben, wenn Start ich erhalte"
- "Mark kaufen iPhone im TasksNotes als abgeschlossen"
- "Erinnern Sie, 8: 00 Uhr in TasksNotes Start beibehalten"

![Beispiel für eine neue Liste erstellen](sirikit-images/createtasklist-sml.png) ![Set-Vorgang als vollständiges Beispiel](sirikit-images/settaskattribute-sml.png)

> [!NOTE]
> IOS 11 unterstützt die Simulator-Tests mit Siri (im Gegensatz zu früheren Versionen).
>
> Vergessen Sie auf echten Geräten getestet nicht zum Konfigurieren von App-ID und die bereitstellungsprofile SiriKit-Unterstützung.

<a name="alternativenames" />

## <a name="alternative-names"></a>Alternative Namen

Diese neue iOS 11-Funktion bedeutet, dass Sie alternative Namen für Ihre app für Benutzer sie ordnungsgemäß mit Siri auslösen konfigurieren können. Fügen Sie die folgenden Schlüssel, der **"Info.plist"** Datei mit dem iOS-app-Projekt:

![Datei "Info.plist" mit Name-alternative app-Schlüssel und Werte](sirikit-images/alternative-names.png)

Die alternative app-Namen festgelegt, die folgenden Ausdrücke funktionieren auch für die Beispiel-app (mit dem tatsächlich Namen **TasksNotes**):

- "Stellen Sie eine Einkaufsliste mit Äpfeln Bananen und Birnen in _MonkeyNotes_"
- "Fügen WWDC in task _MonkeyTodo_"


## <a name="troubleshooting"></a>Problembehandlung

Einige Fehler, die beim Ausführen des Beispiels oder Hinzufügen von SiriKit zu Ihren eigenen Anwendungen auftreten können:

### <a name="nsinternalinconsistencyexception"></a>NSInternalInconsistencyException

_Objective-C-Ausnahme ausgelöst wird.  Name: NSInternalInconsistencyException Ursache: Verwendung der Klasse < INPreferences: 0x60400082ff00 > von einer app erfordert die Berechtigung com.apple.developer.siri. Aktivieren Sie die Siri-Funktion in Ihrem Xcode-Projekt?_

- SiriKit ist Perform **"Entitlements.plist"**.
- **"Entitlements.plist"** konfiguriert ist, der **Projektoptionen > Erstellen > iOS Bundle-Signierung**.

  [![Projektoptionen mit, dass die Berechtigungen ordnungsgemäß festgelegt.](sirikit-images/set-entitlements-sml.png)](sirikit-images/set-entitlements.png#lightbox)

- (für die gerätebereitstellung) App-ID hat SiriKit aktiviert und Bereitstellungsprofil heruntergeladen.


## <a name="related-links"></a>Verwandte Links

- [SiriKit (Apple)](https://developer.apple.com/documentation/sirikit)
- [TasksNotes SiriKit-Beispiel](https://developer.xamarin.com/samples/monotouch/ios11/SiriKitSample/)
- [Was ist neu in der SiriKit (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/214/)
