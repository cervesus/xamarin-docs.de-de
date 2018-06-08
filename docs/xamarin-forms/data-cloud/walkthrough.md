---
title: Grundlegendes zur im Beispiel
description: Dieses Thema enthält eine exemplarische Vorgehensweise für die Xamarin.Forms-beispielanwendung, die für die Kommunikation mit anderen Webdiensten veranschaulicht. Während jeder Webdienst eine separate beispielanwendung verwendet, sind mit ähnlichen Funktionen und Klassen für allgemeine freigeben.
ms.prod: xamarin
ms.assetid: A3FEB262-0D79-42E6-8F8B-A565618C490B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2017
ms.openlocfilehash: d10bdc605fca199a4d0286ce662a27094b5b98f4
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847565"
---
# <a name="understanding-the-sample"></a>Grundlegendes zur im Beispiel

_Dieses Thema enthält eine exemplarische Vorgehensweise für die Xamarin.Forms-beispielanwendung, die für die Kommunikation mit anderen Webdiensten veranschaulicht. Während jeder Webdienst eine separate beispielanwendung verwendet, sind mit ähnlichen Funktionen und Klassen für allgemeine freigeben._

Die unten beschriebene Beispiel Aufgabe List-Anwendung wird verwendet, zu zeigen, wie verschiedene Typen von Web Service-Back-Ends mit Xamarin.Forms zugreifen. Es stellt die Funktionalität zum bereit:

- Eine Liste der Aufgaben anzuzeigen.
- Hinzufügen, bearbeiten und Löschen von Aufgaben.
- Aufgabe mit dem Status auf "Fertig" festgelegt.
- Der Task Felder "Name" und "Notizen zu sprechen.

In allen Fällen werden die Aufgaben in einer Back-End-gespeichert, die über einen Webdienst zugegriffen wird.

Wenn die Anwendung gestartet wird, wird eine Seite angezeigt, die führt alle Aufgaben, die vom Webdienst abgerufen, und ermöglicht dem Benutzer eine neue Aufgabe zu erstellen. Durch Klicken auf eine Aufgabe, navigiert die Anwendung zu einer zweiten Seite, in dem die Aufgabe bearbeitet, gespeichert, gelöscht, gesprochen und werden kann. Die fertige Anwendung wird unten gezeigt:

![](walkthrough-images/app-example-1.png "TODO-Anwendung – erste Seite")
![](walkthrough-images/app-example-2.png "TODO-Anwendung – zweite Seite")

Jedes Thema in diesem Handbuch enthält einen Link zum Herunterladen einer *unterschiedliche* Version der Anwendung, die einen bestimmten Typ von Web Service-Backend veranschaulicht. Herunterladen Sie den relevanten Beispielcode auf der Seite, die im Zusammenhang mit jeder Webdienst Stil.

## <a name="understanding-the-application-anatomy"></a>Grundlegendes zu den Aufbau der Anwendung

Die PCL-Projekt für jede beispielanwendung besteht aus drei Hauptordner:

|Ordner|Zweck|
|--- |--- |
|Daten|Enthält die Klassen und Schnittstellen, die zum Verwalten von Datenelementen und mit dem Webdienst kommunizieren. Mindestens, dazu gehören die `TodoItemManager` -Klasse, die über eine Eigenschaft in offen gelegt werden die `App` Klasse um Webdienstvorgänge aufzurufen.|
|Modelle|Enthält die Klassen des Daten-Modell für die Anwendung. Mindestens, dazu gehören die `TodoItem` -Klasse, die ein einzelnes Element der Daten, die von der Anwendung verwendeten modelliert. Der Ordner kann auch alle zusätzlichen Klassen verwendet, um Benutzer Modelldaten enthalten.|
|Ansichten|Enthält die Seiten für die Anwendung. Dies umfasst normalerweise die `TodoListPage` und `TodoItemPage` Klassen und alle zusätzlichen Klassen zu Authentifizierungszwecken verwendet.|

Die PCL-Projekt für jede Anwendung umfasst auch eine Reihe wichtiger Dateien:

|Datei|Zweck|
|--- |--- |
|Constants.cs|Die `Constants` -Klasse, der alle Konstanten, die von der Anwendung verwendet werden, um mit dem Webdienst kommunizieren angibt. Diese Konstanten erfordern, dass für einen Anbieter aktualisieren für den Zugriff auf Ihren persönlichen Back-End-Dienst erstellt werden.|
|ITextToSpeech.cs|Die `ITextToSpeech` -Schnittstelle, das angibt, dass die `Speak` Methode muss von jeder beliebigen implementierenden Klasse angegeben werden.|
|TODO.cs|Die `App` Klasse, die verantwortlich für die Instanziierung der ersten Seite, die von der Anwendung auf jeder Plattform angezeigt werden und die `TodoItemManager` -Klasse, die verwendet wird, um Webdienstvorgänge aufzurufen.|

### <a name="viewing-pages"></a>Anzeigen von Seiten

Der Großteil der Beispielanwendungen enthalten mindestens zwei Seiten:

- **TodoListPage** – auf dieser Seite zeigt eine Liste der `TodoItem` Instanzen und ein Symbol "Takt" Wenn die `TodoItem.Done` Eigenschaft ist `true`. Durch Klicken auf ein Element navigiert zu der `TodoItemPage`. Darüber hinaus können neue Elemente erstellt werden, indem Sie auf die *+* Symbol.
- **TodoItemPage** – auf dieser Seite zeigt die Details für den ausgewählten `TodoItem`, und ermöglicht dessen bearbeitet werden gespeichert, gelöscht, gesprochen und sein.

Darüber hinaus enthalten einige Beispielanwendungen zusätzliche Seiten, die zur Verwaltung des Authentifizierungsprozesses Benutzer verwendet werden.

### <a name="modeling-the-data"></a>Modellieren von Daten

Jede beispielanwendung verwendet die `TodoItem` Klasse, um die Daten zu modellieren, die angezeigt und an den Webdienst für den Speicher gesendet wird. Das folgende Codebeispiel zeigt die `TodoItem`-Klasse:

```csharp
public class TodoItem
{
    public string ID { get; set; }
    public string Name { get; set; }
    public string Notes { get; set; }
    public bool Done { get; set; }
}
```

Die `ID` Eigenschaft dient zur eindeutigen Identifizierung der einzelnen `TodoItem` -Instanz und wird durch jeden Webdienst verwendet, zum Identifizieren von Daten, die aktualisiert oder gelöscht werden.

### <a name="invoking-web-service-operations"></a>Aufrufen von Webdienstvorgängen

Webdienstvorgänge erfolgt über die `TodoItemManager` Klasse und einer Instanz der Klasse durch zugegriffen werden kann die `App.TodoManager` Eigenschaft. Die `TodoItemManager` Klasse bietet die folgenden Methoden um Webdienstvorgänge aufzurufen:

- **GetTasksAsync** – diese Methode dient zum Auffüllen der `ListView` control für die `TodoListPage` mit der `TodoItem` Instanzen, die vom Webdienst abgerufen.
- **SaveTaskAsync** – diese Methode dient zum Erstellen oder Aktualisieren einer `TodoItem` -Instanz auf den Webdienst.
- **DeleteTaskAsync** – diese Methode wird zum Löschen einer `TodoItem` -Instanz auf den Webdienst.

Darüber hinaus enthalten einige Beispielanwendungen zusätzliche Methoden in der `TodoItemManager` -Klasse, die zur Verwaltung des Authentifizierungsprozesses Benutzer verwendet werden.

Anstatt die Webdienstvorgänge direkt aufrufen der `TodoItemManager` Methoden Aufrufen von Methoden für eine abhängige-Klasse, die in eingeschleust wird die `TodoItemManager` Konstruktor. Eine beispielanwendung beispielsweise fügt die `RestService` Klasse in der `TodoItemManager` Konstruktor, um die Implementierung bereitstellen, die REST-APIs für den Datenzugriff verwendet.

### <a name="translating-text-to-speech"></a>Übersetzen von Text-zu-Sprache

Der Großteil der Beispielanwendungen enthält (Sprachausgabe)-Sprachausgabefunktionen, die Werte der sprechen die `TodoItem.Name` und `TodoItem.Notes` Eigenschaften. Dies wird erreicht, indem die `OnSpeakActivated` -Ereignishandler in der `TodoItemPage` Klasse, wie im folgenden Codebeispiel gezeigt:

```csharp
void OnSpeakActivated (object sender, EventArgs e)
{
    var todoItem = (TodoItem)BindingContext;
    App.Speech.Speak(todoItem.Name + " " + todoItem.Notes);
}
```

Diese Methode ruft einfach auf die `Speak` durch einen plattformspezifischen implementierte Methode an `Speech` Klasse. Jede `Speech` -Klasse implementiert die `ITextToSpeech` -Schnittstelle, und plattformspezifischen Startcode erstellt eine Instanz von der `Speech` -Klasse, die über zugegriffen werden kann die `App.Speech` Eigenschaft.

## <a name="summary"></a>Zusammenfassung

In diesem Thema bereitgestellten eine exemplarische Vorgehensweise für die Xamarin.Forms-beispielanwendung, die verwendet wird, veranschaulicht, wie Sie mit anderen Webdiensten kommunizieren. Während jeder Webdienst eine separate beispielanwendung verwendet, sie basieren auf die gleiche Benutzeroberfläche und Geschäftslogik wie oben beschrieben – nur Web Service Datenspeichermechanismus unterscheidet.


## <a name="related-links"></a>Verwandte Links

- [ASMX-Version (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoASMX)
- [WCF-Version (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoWCF)
- [REST-Version (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST)
- [Azure-Version (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzure)
