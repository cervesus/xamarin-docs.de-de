---
title: Updates für Sirikit in ios 11
description: In diesem Dokument wird beschrieben, wie Sie mit dem Sirikit in ios 11 arbeiten. Insbesondere wird untersucht, wie mit Aufgaben und Notizen gearbeitet wird und wie alternative Namen für eine Anwendung bereitgestellt werden.
ms.prod: xamarin
ms.assetid: 8F75300B-B591-42ED-9D17-001992A5C381
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/07/2017
ms.openlocfilehash: ce4514059b2d0713cdf1e0a4a9956ab38aae7604
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032147"
---
# <a name="sirikit-updates-in-ios-11"></a>Updates für Sirikit in ios 11

Das Sirikit wurde in ios 10 mit einer Reihe von Dienst Domänen (einschließlich Workouts, Fahrten Reservierung und Anrufe) eingeführt. Im Abschnitt " [Sirikit](~/ios/platform/sirikit/index.md) " finden Sie Informationen zu "Sirikit" und zur Implementierung von "Sirikit" in Ihrer APP.

![Demo zur Siri-Aufgabenliste](sirikit-images/sirikit.png)

Mit dem Sirikit in ios 11 werden diese neuen und aktualisierten beabsichtigten Domänen hinzugefügt:

- [**Listen und Notizen**](#listsnotes) – neu! Bietet eine API für apps, um Aufgaben und Notizen zu verarbeiten.
- **Visual Codes** – neu! Siri kann QR-Codes anzeigen, um Kontaktinformationen auszutauschen oder an Zahlungs Transaktionen teilzunehmen.
- **Zahlungen** – hinzugefügte Such-und Übertragungs Absichten für Zahlungs Interaktionen.
- **Fahrt Reservierung** – hinzugefügte Abbruch Fahrt und Feedback Intents.

Zu weiteren neuen Features gehören:

- [**Alternative APP-Namen**](#alternativenames) – bietet Aliase, die Kunden helfen, Siri für Ihre APP zu informieren, indem alternative Namen/Ausdrücke angeboten werden.
- **Starten von Workouts** – bietet die Möglichkeit, ein Training im Hintergrund zu starten.

Einige dieser Features werden weiter unten erläutert. Weitere Informationen zu den anderen finden Sie in [der Dokumentation zu Sirikit von Apple](https://developer.apple.com/documentation/sirikit).

<a name="listsnotes" />

## <a name="lists-and-notes"></a>Listen und Notizen

Die neue Domäne "Listen und Notizen" stellt eine API für Apps zur Verarbeitung von Aufgaben und Notizen über Siri-Sprachanforderungen bereit.

**Tasks (Aufgaben)**

- Einen Titel und einen Abschluss Status aufweisen.
- Fügen Sie optional einen Stichtag und einen Speicherort ein.

**Notizen**

- Sie haben einen Titel und ein Inhaltsfeld.

Sowohl Aufgaben als auch Notizen können in Gruppen organisiert werden. Im restlichen Teil dieses Abschnitts wird beschrieben, wie diese neue Domäne mit "Sirikit" mit dem [tasksnotes-Beispiel "Sirikit](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-sirikitsample)" implementiert wird.

### <a name="how-to-process-a-sirikit-request"></a>Verarbeiten einer Sirikit-Anforderung

Verarbeiten Sie eine Sirikit-Anforderung, indem Sie die folgenden Schritte ausführen:

1. **Lösen** Sie – validieren Sie die Parameter, und fordern Sie weitere Informationen vom Benutzer an (falls erforderlich).
2. **Bestätigen** – abschließende Validierung und Überprüfung, ob die Anforderung verarbeitet werden kann.
3. **Handle** – führen Sie den Vorgang aus (Aktualisieren von Daten oder Durchführen von Netzwerk Vorgängen).

Die ersten beiden Schritte sind optional (obwohl empfohlen), und der letzte Schritt ist erforderlich.
Eine ausführlichere Anleitung finden Sie im [Abschnitt "Sirikit](~/ios/platform/sirikit/index.md)".

### <a name="resolve-and-confirm-methods"></a>Auflösen und bestätigen von Methoden

Mit diesen optionalen Methoden können Sie Ihren Code validieren, Standardwerte auswählen oder zusätzliche Informationen vom Benutzer anfordern.

Beispielsweise ist für die `IINCreateTaskListIntent`-Schnittstelle die erforderliche Methode `HandleCreateTaskList`. Es gibt vier optionale Methoden, die eine bessere Kontrolle über die Siri-Interaktion bieten:

- `ResolveTitle` – überprüft den Titel, legt einen Standard Titel (falls zutreffend) fest oder signalisiert, dass die Daten nicht erforderlich sind.
- `ResolveTaskTitles` – überprüft die Liste der Aufgaben, die vom Benutzer gesprochen werden.
- `ResolveGroupName` – überprüft den Gruppennamen, wählt eine Standardgruppe aus oder signalisiert, dass die Daten nicht erforderlich sind.
- `ConfirmCreateTaskList` – überprüft, ob Ihr Code den angeforderten Vorgang ausführen kann, führt ihn aber nicht aus (nur die `Handle*` Methoden sollten Daten ändern).

### <a name="handle-the-intent"></a>Verarbeiten der Absicht

Die Domäne "Listen und Notizen" enthält sechs Absichten, drei für Aufgaben und drei für Notizen.
Die Methoden, die Sie implementieren müssen, um diese Intents zu verarbeiten, lauten wie folgt:

- Für Aufgaben:
  - `HandleAddTasks`
  - `HandleCreateTaskList`
  - `HandleSetTaskAttribute`
- Hinweise:
  - `HandleCreateNote`
  - `HandleAppendToNote`
  - `HandleSearchForNotebookItems`

Jede Methode verfügt über einen bestimmten Intent-Typ, der alle Informationen enthält, die Siri aus der Anforderung des Benutzers analysiert hat (und möglicherweise in den `Resolve*`-und `Confirm*` Methoden aktualisiert).
Ihre APP muss die bereitgestellten Daten analysieren, dann einige Aktionen ausführen, um die Daten zu speichern oder anderweitig zu verarbeiten, und ein Ergebnis zurückgeben, das Siri meldet und dem Benutzer anzeigt.

### <a name="response-codes"></a>Antwortcodes

Die erforderlichen `Handle*` und optionalen `Confirm*` Methoden geben einen Antwort Code an, indem Sie einen Wert für das Objekt festlegen, das Sie an den Abschluss Handler übergeben. Antworten stammen aus der `INCreateTaskListIntentResponseCode`-Enumeration:

- `Ready` – wird während der Bestätigungsphase (d. von einer `Confirm*` Methode, jedoch nicht von einer `Handle*` Methode) zurückgegeben.
- `InProgress` – für Aufgaben mit langer Ausführungszeit (z. b. ein Netzwerk-/Server-Vorgang).
- `Success` – antwortet mit den Details des erfolgreichen Vorgangs (nur aus einer `Handle*`-Methode).
- `Failure` – bedeutet, dass ein Fehler aufgetreten ist, und der Vorgang konnte nicht abgeschlossen werden.
- `RequiringAppLaunch` – kann nicht von der Absicht verarbeitet werden, aber der Vorgang ist in der APP möglich.
- `Unspecified` – nicht verwenden: die Fehlermeldung wird dem Benutzer angezeigt.

Weitere Informationen zu diesen Methoden und Antworten finden Sie in der Dokumentation zu den [Sirikit-Listen und Notizen](https://developer.apple.com/documentation/sirikit/lists_and_notes)von Apple.

### <a name="implementing-lists-and-notes"></a>Implementieren von Listen und Notizen

Das [Beispiel tasksnotes Sirikit](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-sirikitsample) wurde mit den folgenden Schritten erstellt, um eine Unterstützung für eine leere IOS-APP zu unterstützen.

Führen Sie zum Hinzufügen der Unterstützung von Sirikit zunächst die folgenden Schritte für Ihre IOS-App aus:

1. Tick **Sirikit** in "Berechtigungs **. plist**".
2. Fügen Sie " **Info. plist**" den Schlüssel " **Privacy – Siri Usage Description** " und eine Nachricht für Ihre Kunden hinzu.
3. Wenden Sie die `INPreferences.RequestSiriAuthorization`-Methode in der APP an, um den Benutzer aufzufordern, Siri-Interaktionen zuzulassen.
4. Fügen Sie Ihrer APP-ID im Entwickler Portal "Sirikit" hinzu, und erstellen Sie Ihre Bereitstellungs Profile neu, damit Sie die neue Berechtigung enthalten.

Fügen Sie Ihrer APP dann ein neues Erweiterungsprojekt hinzu, um Siri-Anforderungen verarbeiten zu können:

1. Klicken Sie mit der rechten Maustaste auf Ihre Projekt Mappe, und wählen Sie **Hinzufügen > Neues Projekt hinzufügen**aus.
2. Wählen Sie die **IOS-> Erweiterung > Intents-Erweiterungs** Vorlage aus.
3. Es werden zwei neue Projekte hinzugefügt: Intent und intentui. Das Anpassen der Benutzeroberfläche ist optional, sodass das Beispiel nur Code im **Intent** -Projekt enthält.

Das Erweiterungsprojekt ist der Ort, an dem alle Sirikit-Anforderungen verarbeitet werden. Als separate Erweiterung hat Sie nicht automatisch eine Möglichkeit, mit Ihrer Haupt-APP zu kommunizieren – Dies wird in der Regel durch die Implementierung von frei gegebenem Dateispeicher mithilfe von App-Gruppen gelöst.

#### <a name="configure-the-intenthandler"></a>Konfigurieren des intenthandler

Die `IntentHandler`-Klasse ist der Einstiegspunkt für Siri-Anforderungen – jede Absicht wird an die `GetHandler`-Methode übermittelt, die ein Objekt zurückgibt, das die Anforderung verarbeiten kann.

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

Die Klasse muss von `INExtension`erben, und da das Beispiel die Verarbeitung von Listen und Notizen behandelt, implementiert es auch `IINNotebookDomainHandling`.

> [!NOTE]
>
> - Es gibt eine Konvention in .net für Schnittstellen, denen ein `I`vorangestellten werden soll, die xamarin bei der Bindung von Protokollen aus dem IOS SDK befolgt.
> - Xamarin behält auch Typnamen von IOS bei, und Apple verwendet die ersten beiden Zeichen in Typnamen, um das Framework widerzuspiegeln, zu dem ein Typ gehört.
> - Für das `Intents` Framework verfügen Typen über die Präfix `IN*` (z. b. `INExtension`), aber dies sind _keine_ Schnittstellen.
> - Außerdem folgt die Verwendung von Protokollen (die zu Schnitt C#stellen in werden) zwei`I`s, z. b.`IINAddTasksIntentHandling`.

#### <a name="handling-intents"></a>Umgang mit Intents

Jede Absicht (Aufgabe hinzufügen, Set Task Attribute usw.) wird in einer einzelnen Methode implementiert, die der unten gezeigten ähnelt. Die-Methode sollte drei Hauptfunktionen ausführen:

1. **Verarbeiten der Absicht** – die von Siri analysierten Daten werden in einem `intent` Objekt zur Verfügung gestellt, das für den Typ der Absicht spezifisch ist. Ihre APP hat diese Daten möglicherweise mithilfe optionaler `Resolve*` Methoden überprüft.
2. **Datenspeicher überprüfen und aktualisieren** – speichern Sie Daten im Dateisystem (mithilfe von App-Gruppen, sodass die IOS-Haupt-APP auch darauf zugreifen kann) oder über eine Netzwerk Anforderung.
3. **Antwort angeben** – verwenden Sie den `completion` Handler, um eine Antwort zurück an Siri zu senden, um den Benutzer zu lesen/anzuzeigen:

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

Beachten Sie, dass `null` als zweiter Parameter an die Antwort übergeben wird – dies ist der Benutzer Aktivitäts Parameter. wenn er nicht angegeben wird, wird ein Standardwert verwendet.
Sie können einen benutzerdefinierten Aktivitätstyp festlegen, solange Ihre IOS-App ihn über den `NSUserActivityTypes`-Schlüssel in der Datei " **Info. plist**" unterstützt. Sie können diesen Fall dann verarbeiten, wenn Ihre APP geöffnet wird, und bestimmte Vorgänge ausführen (z. b. das Öffnen eines relevanten Ansichts Controllers und das Laden der Daten aus dem Siri-Vorgang).

Im Beispiel wird auch das `Success` Ergebnis hart codiert, aber in realen Szenarien sollte die richtige Fehlerberichterstattung hinzugefügt werden.

### <a name="test-phrases"></a>Test Ausdrücke

Die folgenden Test Ausdrücke sollten in der Beispiel-App funktionieren:

- "Erstellen einer lebensmittelliste mit Äpfeln, Bananen und Birnen in tasksnotes"
- "Task WWDC in tasksnotes hinzufügen"
- "Task WWDC zu Trainings Liste in tasksnotes hinzufügen"
- "Mark attend WWDC as Complete in tasksnotes"
- "In tasksnotes erinnern Sie sich an den Kauf eines iPhones, wenn ich zu Hause komme."
- "Mark iPhone als abgeschlossen in tasksnotes markieren"
- "Erinnern Sie mich daran, in tasksnotes an der Startseite zu bleiben."

![Beispiel für eine neue Liste erstellen](sirikit-images/createtasklist-sml.png) ![Aufgabe als vervollständigen festlegen (Beispiel)](sirikit-images/settaskattribute-sml.png)

> [!NOTE]
> Der IOS 11-Simulator unterstützt Tests mit Siri (im Gegensatz zu früheren Versionen).
>
> Wenn Sie auf echten Geräten testen, vergessen Sie nicht, Ihre APP-ID und Bereitstellungs Profile für die Unterstützung von Sirikit zu konfigurieren.

<a name="alternativenames" />

## <a name="alternative-names"></a>Alternative Namen

Dieses neue IOS 11-Feature bedeutet, dass Sie alternative Namen für Ihre APP konfigurieren können, damit Benutzer Sie mithilfe von Siri ordnungsgemäß auslöst. Fügen Sie der Datei " **Info. plist** " des IOS-App-Projekts die folgenden Schlüssel hinzu:

![Info. plist mit alternativen App-namens Schlüsseln und-Werten](sirikit-images/alternative-names.png)

Wenn die alternativen APP-Namen festgelegt sind, funktionieren die folgenden Ausdrücke auch für die Beispiel-app (die tatsächlich **tasksnotes**heißt):

- "Erstellen einer lebensmittelliste mit Äpfeln, Bananen und Birnen in _monkeynotes_"
- "Task WWDC in _monkeytodo_hinzufügen"

## <a name="troubleshooting"></a>Problembehandlung

Einige Fehler, die bei der Ausführung des Beispiels auftreten können, oder zum Hinzufügen von Sirikit zu ihren eigenen Anwendungen:

### <a name="nsinternalinconsistencyexception"></a>NSInternalInconsistencyException

_Die ausgelöste Ziel-C-Ausnahme.  Name: NSInternalInconsistencyException Reason: die Verwendung der Klasse < inpreferences: 0x60400082ff00 > einer APP erfordert die Berechtigung com. Apple. Developer. Siri. Haben Sie die Siri-Funktion in Ihrem Xcode-Projekt aktiviert?_

- "Sirikit" wird in "Berechtigungs **. plist" angezeigt**.
- " **Berechtigungen. plist** " wird in den **Projektoptionen > Build > IOS-Bündel Signierung**konfiguriert.

  [![Projektoptionen mit ordnungsgemäßer Festlegung von Berechtigungen](sirikit-images/set-entitlements-sml.png)](sirikit-images/set-entitlements.png#lightbox)

- (für die Geräte Bereitstellung) Für die APP-ID ist das Sirikit aktiviert und das Bereitstellungs Profil heruntergeladen.

## <a name="related-links"></a>Verwandte Links

- [Sirikit (Apple)](https://developer.apple.com/documentation/sirikit)
- [Tasksnotes-Beispiel für Sirikit](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-sirikitsample)
- [Neuerungen in "Sirikit" (WWDC) (Video)](https://developer.apple.com/videos/play/wwdc2017/214/)
