---
Title: " Xamarin.Forms Einführung in die Webdienste" Beschreibung: "dieses Handbuch enthält eine exemplarische Vorgehensweise für die Xamarin.Forms Beispielanwendung, die die Kommunikation mit verschiedenen Webdiensten veranschaulicht. Obwohl jeder Webdienst eine separate Beispielanwendung verwendet, sind Sie funktionell ähnlich und weisen gemeinsame Klassen auf. "
ms. Prod: xamarin ms. assetid: A3FEB262-0D79-42E6-8F8B-A565618C490B ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 02/28/2017 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-web-services-introduction"></a>Xamarin.FormsEinführung in Webdienste

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todorest)

_Dieses Thema enthält eine exemplarische Vorgehensweise für die Xamarin.Forms Beispielanwendung, die die Kommunikation mit verschiedenen Webdiensten veranschaulicht. Obwohl jeder Webdienst eine separate Beispielanwendung verwendet, sind Sie funktionell ähnlich und teilen allgemeine Klassen._

Mit der unten beschriebenen Beispiel-Aufgabenlisten Anwendung wird veranschaulicht, wie auf verschiedene Typen von Webdienst-Back-Ends mit zugegriffen werden kann Xamarin.Forms . Es bietet folgende Funktionen:

- Anzeigen einer Liste von Tasks.
- Aufgaben hinzufügen, bearbeiten und löschen.
- Legen Sie den Status einer Aufgabe auf "Done" fest.
- Sprechen Sie die Felder Name und Notes der Aufgabe an.

In allen Fällen werden die Aufgaben in einem Back-End gespeichert, auf das über einen Webdienst zugegriffen wird.

Wenn die Anwendung gestartet wird, wird eine Seite mit allen vom Webdienst abgerufenen Tasks angezeigt, und der Benutzer kann eine neue Aufgabe erstellen. Wenn Sie auf eine Aufgabe klicken, wird die Anwendung auf eine zweite Seite navigiert, auf der die Aufgabe bearbeitet, gespeichert, gelöscht und gesprochen werden kann. Die fertige Anwendung wird unten gezeigt:

![](introduction-images/app-example-1.png "Todo application - first page")
![](introduction-images/app-example-2.png "Todo application - second page")

Jedes Thema in dieser Anleitung enthält einen Download Link zu einer *anderen* Version der Anwendung, die einen bestimmten Typ von Webdienst-Back-End veranschaulicht. Herunterladen des relevanten Beispielcodes auf der Seite, die sich auf die einzelnen Webdienst Stile bezieht.

## <a name="understand-the-application-anatomy"></a>Grundlegendes zur Anwendungs Anatomie

Das freigegebene Code Projekt für jede Beispielanwendung besteht aus drei Haupt Ordnern:

|Ordner|Zweck|
|--- |--- |
|Daten|Enthält die Klassen und Schnittstellen, mit denen Datenelemente verwaltet und mit dem Webdienst kommuniziert werden. Dies schließt mindestens die-Klasse ein `TodoItemManager` , die durch eine Eigenschaft in der-Klasse verfügbar gemacht wird, `App` um Webdienst Vorgänge aufzurufen.|
|Modelle|Enthält die Datenmodell Klassen für die Anwendung. Dies schließt mindestens die-Klasse ein `TodoItem` , die ein einzelnes Datenelement modelliert, das von der Anwendung verwendet wird. Der Ordner kann auch alle zusätzlichen Klassen enthalten, die zum Modellieren von Benutzerdaten verwendet werden.|
|Sichten|Enthält die Seiten für die Anwendung. Dies umfasst normalerweise die `TodoListPage` `TodoItemPage` Klassen und sowie alle zusätzlichen Klassen, die zu Authentifizierungs Zwecken verwendet werden.|

Das freigegebene Code Projekt für jede Anwendung besteht auch aus einer Reihe wichtiger Dateien:

|Datei|Zweck|
|--- |--- |
|Constants.cs|Die- `Constants` Klasse, die alle Konstanten angibt, die von der Anwendung für die Kommunikation mit dem Webdienst verwendet werden. Diese Konstanten müssen aktualisiert werden, um auf Ihren persönlichen Back-End-Dienst zuzugreifen, der auf einem Anbieter erstellt|
|ITextToSpeech.cs|Die- `ITextToSpeech` Schnittstelle, die angibt, dass die `Speak` Methode von jeder implementierenden Klasse bereitgestellt werden muss.|
|Todo.cs|Die `App` Klasse, die für die Instanziierung der ersten Seite verantwortlich ist, die von der Anwendung auf jeder Plattform angezeigt wird, und der Klasse, die `TodoItemManager` zum Aufrufen von Webdienst Vorgängen verwendet wird.|

### <a name="view-pages"></a>Seiten anzeigen

Die Mehrzahl der Beispielanwendungen enthalten mindestens zwei Seiten:

- " **Tdolistpage** " – auf dieser Seite wird eine Liste der `TodoItem` -Instanzen und ein Tick-Symbol angezeigt, wenn die- `TodoItem.Done` Eigenschaft ist `true` . Wenn Sie auf ein Element klicken, wird zu der navigiert `TodoItemPage` . Außerdem können neue Elemente erstellt werden, indem Sie auf das *+* Symbol klicken.
- " **Tdoitempage** " – auf dieser Seite werden die Details für die ausgewählte angezeigt `TodoItem` , und Sie können bearbeitet, gespeichert, gelöscht und gesprochen werden.

Außerdem enthalten einige Beispielanwendungen zusätzliche Seiten, die zum Verwalten des Benutzer Authentifizierungs Vorgangs verwendet werden.

### <a name="model-the-data"></a>Modellieren der Daten

Jede Beispielanwendung verwendet die `TodoItem` -Klasse, um die Daten zu modellieren, die angezeigt und für den Speicher an den Webdienst gesendet werden. Das folgende Codebeispiel zeigt die `TodoItem`-Klasse:

```csharp
public class TodoItem
{
    public string ID { get; set; }
    public string Name { get; set; }
    public string Notes { get; set; }
    public bool Done { get; set; }
}
```

Die `ID` -Eigenschaft wird zur eindeutigen Identifizierung der einzelnen `TodoItem` Instanzen verwendet und wird von jedem Webdienst verwendet, um die zu aktualisierenden oder zu löschenden Daten zu identifizieren.

### <a name="invoke-web-service-operations"></a>Aufrufen von Webdienst Vorgängen

Auf Webdienst Vorgänge wird über die `TodoItemManager` -Klasse zugegriffen, und über die-Eigenschaft kann auf eine Instanz der-Klasse zugegriffen werden `App.TodoManager` . Die- `TodoItemManager` Klasse stellt die folgenden Methoden zum Aufrufen von Webdienst Vorgängen bereit:

- **Gettasksasync** – diese Methode wird verwendet, um das `ListView` Steuerelement auf dem `TodoListPage` mit den `TodoItem` aus dem Webdienst abgerufenen Instanzen aufzufüllen.
- **Savetaskasync** – diese Methode wird verwendet, um eine- `TodoItem` Instanz im Webdienst zu erstellen oder zu aktualisieren.
- **Deletetaskasync** – diese Methode wird verwendet, um eine- `TodoItem` Instanz im Webdienst zu löschen.

Außerdem enthalten einige Beispielanwendungen zusätzliche Methoden in der- `TodoItemManager` Klasse, die verwendet werden, um den Benutzer Authentifizierungsprozess zu verwalten.

Anstatt die Webdienst Vorgänge direkt aufzurufen, `TodoItemManager` rufen die Methoden Methoden für eine abhängige Klasse auf, die in den `TodoItemManager` Konstruktor eingefügt wird. Beispielsweise fügt eine Beispielanwendung die- `RestService` Klasse in den `TodoItemManager` Konstruktor ein, um die-Implementierung bereitzustellen, die Rest-APIs für den Zugriff auf Daten verwendet.

## <a name="related-links"></a>Verwandte Links

- [ASMX (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todoasmx)
- [WCF (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todowcf)
- [Rest (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todorest)
