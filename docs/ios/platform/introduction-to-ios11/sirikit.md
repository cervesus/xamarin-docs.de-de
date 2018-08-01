---
title: SiriKit Updates in iOS 11
description: Dieses Dokument beschreibt, wie mit SiriKit iOS 11 arbeiten. Insbesondere überprüft er zum Arbeiten mit Aufgaben und Notizen und wie Sie alternative Namen für eine Anwendung bereitstellen.
ms.prod: xamarin
ms.assetid: 8F75300B-B591-42ED-9D17-001992A5C381
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/07/2017
ms.openlocfilehash: 28160b40c97b8cc62fae95d3643801f1c4cc5e93
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787584"
---
# <a name="sirikit-updates-in-ios-11"></a>SiriKit Updates in iOS 11

SiriKit wurde in iOS 10, mit einer Anzahl von Domänen (einschließlich fitnessaktivitäten, fuhr buchungs- und Aufrufe) eingeführt. Finden Sie in der [SiriKit Abschnitt](~/ios/platform/sirikit/index.md) SiriKit Konzepte und wie Sie SiriKit in Ihrer app zu implementieren.

![Siri Aufgabe Liste demo](sirikit-images/sirikit.png)

SiriKit in iOS 11 wird dieser neuen und aktualisierten beabsichtigten Domänen hinzugefügt:

- [**Listet und Anmerkungen zu dieser** ](#listsnotes) – neue! Stellt eine API für apps, um Aufgaben und Notizen zu verarbeiten.
- **Visual Codes** – neue! Siri kann QR-Codes zum Freigeben von Kontaktinformationen oder Zahlungstransaktionen teilnehmen angezeigt werden.
- **Zahlungen** – suchen und die Übertragung Intents für Zahlung Interaktionen hinzugefügt.
- **Außer Kraft setzen und buchen** – hinzugefügten fuhr und Feedback Intents Abbrechen.

Andere neue Funktionen:

- [**Alternative app-Namen** ](#alternativenames) – bietet Aliase, die Kunden zu teilen Siri auf Ihre app verwendet wird, indem Sie alternative Namen/Aussprache anbietet.
- **Starten Training** – bietet die Möglichkeit, eine Trainings im Hintergrund zu starten.

Einige dieser Funktionen werden im folgenden erläutert. Weitere Details zu den anderen, finden Sie unter [Apple SiriKit Dokumentation](https://developer.apple.com/documentation/sirikit).

<a name="listsnotes" />

## <a name="lists-and-notes"></a>Listen und Hinweise

Die neue Listen und Anmerkungen zu dieser Domäne enthält eine API für apps, um Aufgaben und Notizen über Siri Voice-Anforderungen zu verarbeiten.

**Aufgaben**

- Haben Sie einen Titel und eine Abschlussstatus angezeigt.
- Optional enthalten Sie einen Stichtag und einen Speicherort aus.

**Notizen**

- Haben Sie einen Titel und eine Inhaltsfelds.

Aufgaben und Notizen können in Gruppen organisiert werden. Im restlichen Teil dieses Abschnitts wird beschrieben, wie diese neue Domäne mit SiriKit, implementieren Sie mithilfe der [TasksNotes SiriKit Beispiel](https://developer.xamarin.com/samples/monotouch/ios11/SiriKitSample/).

### <a name="how-to-process-a-sirikit-request"></a>Gewusst wie: Verarbeiten einer Anforderung SiriKit

Verarbeiten einer Anforderung SiriKit folgende Schritte:

1. **Beheben Sie** – Parameter überprüfen und Weitere Informationen angefordert werden vom Benutzer (falls erforderlich).
2. **Vergewissern Sie sich** – endgültige Validierung und überprüft wird, ob die Anforderung verarbeitet werden kann.
3. **Behandeln** – führen Sie den Vorgang (Aktualisieren von Daten oder Durchführen von Netzwerkoperationen).

Die ersten beiden Schritte sind optional, (obwohl empfohlen wird), und der letzte Schritt ist erforderlich.
Es werden ausführlichere Anweisungen in der [SiriKit Abschnitt](~/ios/platform/sirikit/index.md).

### <a name="resolve-and-confirm-methods"></a>Beheben, und bestätigen Sie die Methoden

Diese optionale Methoden können Codes, Validierung, wählen Sie Standardeinstellungen oder zusätzliche Anforderungsinformationen des Benutzers ausführen.

Als Beispiel für die `IINCreateTaskListIntent` -Schnittstelle, die die erforderliche Methode ist `HandleCreateTaskList`. Es gibt vier optionale Methoden, die mehr Kontrolle über die Interaktion Siri bereitstellen:

- `ResolveTitle` – Überprüft den Titel, legt einen Standardtitel (falls zutreffend) oder signalisiert, dass die Daten nicht erforderlich ist.
- `ResolveTaskTitles` – Überprüft die Liste der Aufgaben, die vom Benutzer gesprochen.
- `ResolveGroupName` – Überprüft den Gruppennamen, wählt eine Standardgruppe oder signalisiert, dass die Daten nicht erforderlich ist.
- `ConfirmCreateTaskList` – Überprüft, dass Ihr Code kann den angeforderten Vorgang auszuführen, jedoch er keine führt (nur die `Handle*` Methoden sollten Daten ändern).

### <a name="handle-the-intent"></a>Handle der Absicht

Es gibt sechs Intents in den Listen und die Anmerkungen zu dieser Domäne, die drei für Aufgaben und drei Anmerkungen zu dieser.
Die Methoden, die Sie implementieren müssen, um diese Intents zu behandeln sind:

- Für Aufgaben:
  - `HandleAddTasks`
  - `HandleCreateTaskList`
  - `HandleSetTaskAttribute`
- Hinweise:
  - `HandleCreateNote`
  - `HandleAppendToNote`
  - `HandleSearchForNotebookItems`

Jede Methode verfügt über einen bestimmten beabsichtigte Typ übergeben wird, die alle Informationen enthält Siri aus die Anforderung des Benutzers analysiert wurde (und möglicherweise aktualisiert werden, der `Resolve*` und `Confirm*` Methoden).
Ihrer app muss analysiert die Daten bereitgestellt, dann führen einige Aktionen zum Speichern oder andernfalls die Daten verarbeiten und ein Ergebnis, das Siri spricht und zeigt dem Benutzer zurückgeben.

### <a name="response-codes"></a>Antwortcodes

Die erforderliche `Handle*` und optionale `Confirm*` Methoden einen Antwortcode anhand der auf das Objekt, das sie übergeben einen Wert festlegen, um ihre Abschlusshandler angeben. Antworten stammen aus den `INCreateTaskListIntentResponseCode` Enumeration:

- `Ready` – Gibt während der Phase der Bestätigung (ie. aus einer `Confirm*` -Methode, jedoch nicht von einer `Handle*` Methode).
- `InProgress` – Für lang andauernden Aufgaben (z. B. einen Netzwerk-Server-Vorgang) verwendet.
- `Success` – Antwortet mit den Details für die erfolgreiche Ausführung (nur aus einer `Handle*` Methode).
- `Failure` – Bedeutet, dass ein Fehler aufgetreten, und der Vorgang nicht abgeschlossen werden konnte.
- `RequiringAppLaunch` – Von der Absicht nicht verarbeitet werden, aber der Vorgang ist in der Anwendung möglich.
- `Unspecified` – Verwenden Sie nicht: Fehlermeldung wird dem Benutzer angezeigt werden.

Weitere Informationen zu diesen Methoden und Antworten in der Apple- [SiriKit aufgelistet und Anmerkungen zu dieser Dokumentation](https://developer.apple.com/documentation/sirikit/lists_and_notes).

### <a name="implementing-lists-and-notes"></a>Implementieren von Listen und Hinweise

Die [TasksNotes SiriKit Beispiel](https://developer.xamarin.com/samples/monotouch/ios11/SiriKitSample/) erstellt wurde, mithilfe der folgenden Schritte SiriKit-Unterstützung für eine leere iOS-app hinzufügen.

Erstens SiriKit-Unterstützung hinzuzufügen, gehen Sie folgendermaßen vor für die iOS-app:

1. Teilstriche **SiriKit** in **Entitlements.plist**.
2. Hinzufügen der **Datenschutz – Beschreibung der Verwendung Siri** um **"Info.plist"**, zusammen mit einer Meldung für Ihre Kunden.
3. Rufen Sie die `INPreferences.RequestSiriAuthorization` Methode in der app, für den Benutzer auffordern, Interaktionen Siri zulassen.
4. Ihre App-ID im Entwicklerportal SiriKit hinzu, und erstellen Sie Ihre Bereitstellung Profile auf Einbeziehung die neue Berechtigung neu.

Klicken Sie dann fügen Sie ein neues Erweiterungsprojekt zur Ihrer app hinzu, Siri-Anforderungen zu verarbeiten:

1. Mit der rechten Maustaste auf die Projektmappe, und wählen Sie **hinzufügen > Neues Projekt hinzufügen...** .
2. Wählen Sie die **iOS > Erweiterung > Intents Erweiterung** Vorlage.
3. Zwei neue Projekte hinzugefügt werden: Absicht und IntentUI. Anpassen der Benutzeroberfläche ist optional, damit das Beispiel nur in Code enthält die **Absicht** Projekt.

Das Erweiterungsprojekt wird, in dem alle SiriKit-Anforderungen verarbeitet werden. Als eine separate Erweiterung ist automatisch keine keinerlei für die Kommunikation mit der Haupt-app – dies in der Regel durch die Implementierung von app-Gruppen mit dateifreigabespeicher aufgelöst wird.

#### <a name="configure-the-intenthandler"></a>Konfigurieren der IntentHandler

Die `IntentHandler` Klasse ist der Einstiegspunkt für Siri fordert – jeder Absicht übergeben wird die `GetHandler` Methode, die ein Objekt zurückgibt, die die Anforderung verarbeiten kann.

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

Die Klasse muss von erben `INExtension`, und da im Beispiel wird für die Behandlung von Listen und Anmerkungen dieser Intents, implementiert er auch `IINNotebookDomainHandling`.

> [!NOTE]
> - Es ist eine Konvention, die in .NET für Schnittstellen als Präfix einen Großbuchstaben `I`, dem Xamarin entspricht, beim Binden von Protokollen im IOS-SDK.
> - Xamarin behält auch Typnamen IOS und Apple verwendet die ersten beiden Zeichen in Typnamen, um das Framework widerspiegeln, dem ein Typ gehört.
> - Für die `Intents` Framework Typen werden mit dem Präfix `IN*` (z. b. `INExtension`), aber dies sind _nicht_ Schnittstellen.
> - Daraus folgt auch, dass Protokolle (die Schnittstellen, die in C# geschrieben sind) mit zwei Enden `I`s, z. B. `IINAddTasksIntentHandling`.

#### <a name="handling-intents"></a>Behandlung von Intents

Jede Absicht (Hinzufügen von Tasks, Task-Attribut festgelegt usw.) wird in einer einzelnen Methode, die ähnlich wie in der Abbildung unten implementiert. Die Methode sollte drei Hauptfunktionen auszuführen:

1. **Die Absicht** – die Daten analysiert werden Siri im zur Verfügung gestellt werden ein `intent` objektspezifischen in den Typ der Absicht. Ihre app kann diese Daten mit optionalen überprüft `Resolve*` Methoden.
2. **Überprüfen und Aktualisieren von Datenspeicher** – speichern Daten in das Dateisystem (verwenden App-Gruppen aus, sodass die wichtigsten iOS-app auch darauf zugreifen kann) oder über ein netzwerkanforderung.
3. **Geben Sie die Antwort** – verwenden der `completion` Handler zum Senden einer Antwort Siri mit Lese-/Anzeige für den Benutzer an:

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

Beachten Sie, dass `null` übergeben wird als zweiten Parameter an die Antwort – ist dies die Benutzer-Aktivitätsparameter, und wenn es nicht angegeben wird, wird ein Standardwert verwendet.
Sie können einen benutzerdefinierten Aktivitätstyp festlegen, solange Ihre iOS-app auch über unterstützt die `NSUserActivityTypes` -Schlüssel in **"Info.plist"**. Sie können diesen Fall zu behandeln, wenn Ihre app geöffnet wird, und bestimmte Vorgänge (z. B. mit einem Controller relevanten Ansicht öffnen und das Laden der Daten aus dem Vorgang Siri) ausführen.

Das Beispiel auch Hardcodes der `Success` Ergebnis, aber in realen Szenarios ordnungsgemäße Fehlerberichterstattung hinzugefügt werden sollen.

### <a name="test-phrases"></a>Testen von Ausdrücken

Die folgenden Test-Zeichenfolgen sollten in der Beispiel-app arbeiten:

- "Stellen Sie eine Liste Lebensmittelgeschäften mit Äpfeln, Bananen und Birnen in TasksNotes"
- "Aufgabe WWDC in TasksNotes hinzufügen"
- "Vorgang WWDC Training Liste in TasksNotes hinzufügen"
- "Markierung studiert WWDC als vollständig in TasksNotes"
- "In TasksNotes erinnern Sie Iphone zu erwerben, wenn ich erhalte die Startseite"
- "Mark Bezugsquellen iPhone als TasksNotes abgeschlossen"
- "Erinnern Sie Startseite 8.00 Uhr in TasksNotes verlassen"

![Ein Beispiel für eine neue Liste erstellen](sirikit-images/createtasklist-sml.png) ![Set-Vorgang als vollständiges Beispiel](sirikit-images/settaskattribute-sml.png)

> [!NOTE]
> IOS 11 unterstützt Simulator Testen mit Siri (im Gegensatz zu früheren Versionen).
>
> Wenn auf echten Geräten zu testen, vergessen Sie nicht so konfigurieren Sie Ihre App-ID und bereitstellungsprofile für SiriKit-Unterstützung.

<a name="alternativenames" />

## <a name="alternative-names"></a>Alternative Namen

Dieses neue Feature für iOS 11 bedeutet, dass Sie alternative Namen für Ihre app, damit sie ordnungsgemäß mit Siri trigger Benutzer konfigurieren können. Die folgenden Schlüssel zum Hinzufügen der **"Info.plist"** -Datei des iOS-app-Projekt:

![Datei "Info.plist" mit Schlüssel der alternativen app-Namen und Werte](sirikit-images/alternative-names.png)

Mit dem Namen alternative app, die folgenden Ausdrücke funktioniert auch für die Beispiel-app (die tatsächlich Bezeichnung **TasksNotes**):

- "Erstellen Sie eine Liste Lebensmittelgeschäften mit Äpfeln, Bananen und Birnen in _MonkeyNotes_"
- "Hinzufügen Aufgabe WWDC in _MonkeyTodo_"


## <a name="troubleshooting"></a>Problembehandlung

Einige Fehler, die beim Ausführen des Beispiels oder Hinzufügen von SiriKit zu Ihren eigenen Anwendungen auftreten können:

### <a name="nsinternalinconsistencyexception"></a>NSInternalInconsistencyException

_Objective-C-Ausnahme ausgelöst wird.  Name: NSInternalInconsistencyException Ursache: Verwenden der Klasse < INPreferences: 0x60400082ff00 > von einer app benötigt die Berechtigung com.apple.developer.siri. Haben Sie die Siri-Funktion in Ihrem Xcode-Projekt aktiviert?_

- SiriKit Konfigurationsdaten **Entitlements.plist**.
- **Entitlements.plist** konfiguriert ist, der **Projektoptionen > Erstellen > iOS Bundle Signing**.

  [![-Projektoptionen mit, dass die Berechtigungen richtig festgelegt.](sirikit-images/set-entitlements-sml.png)](sirikit-images/set-entitlements.png#lightbox)

- (für die Bereitstellung der Geräte) App-ID besitzt SiriKit aktiviert und Bereitstellungsprofil heruntergeladen.


## <a name="related-links"></a>Verwandte Links

- [SiriKit (Apple)](https://developer.apple.com/documentation/sirikit)
- [TasksNotes SiriKit-Beispiel](https://developer.xamarin.com/samples/monotouch/ios11/SiriKitSample/)
- [Neuigkeiten in der SiriKit (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/214/)
