---
title: Grundlegendes zum Beispiel
description: Dieses Thema enthält eine exemplarische Vorgehensweise für die Xamarin.Forms-beispielanwendung, die zeigt, wie Sie mit anderen Webdiensten kommunizieren. Während jeder Webdienst eine separate Anwendung verwendet wird, sind mit ähnlichen Funktionen und Klassen für allgemeine freigeben.
ms.prod: xamarin
ms.assetid: A3FEB262-0D79-42E6-8F8B-A565618C490B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2017
ms.openlocfilehash: bfd7330fa1eee5f80a9043341d9760058d99d48b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61321749"
---
# <a name="understanding-the-sample"></a>Grundlegendes zum Beispiel

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST)

_Dieses Thema enthält eine exemplarische Vorgehensweise für die Xamarin.Forms-beispielanwendung, die zeigt, wie Sie mit anderen Webdiensten kommunizieren. Während jeder Webdienst eine separate Anwendung verwendet wird, sind mit ähnlichen Funktionen und Klassen für allgemeine freigeben._

Die beispielanwendung für Aufgabenlisten unten wird veranschaulicht, wie Zugriff auf verschiedene Typen von Web-Dienst-Back-Ends mit Xamarin.Forms verwendet. Es stellt Funktionen bereit:

- Anzeigen einer Liste von Aufgaben.
- Hinzufügen, bearbeiten und Löschen von Aufgaben.
- Festlegen des Aufgabenstatus auf "Fertig".
- Sprechen Sie die Task Name "und" Hinweise ".

In allen Fällen werden die Aufgaben in einem Back-End gespeichert, die über einen Webdienst zugegriffen wird.

Wenn die Anwendung gestartet wird, wird eine Seite angezeigt, die führt alle Aufgaben, die vom Webdienst abgerufen, und ermöglicht dem Benutzer, einen neuen Task erstellen. Durch Klicken auf eine Aufgabe, navigiert die Anwendung zu einer zweiten Seite, in dem die Aufgabe bearbeitet, gespeichert, gelöscht, gesprochen und werden kann. Die fertige Anwendung wird unten gezeigt:

![](walkthrough-images/app-example-1.png "TODO-Anwendung – die erste Seite")
![](walkthrough-images/app-example-2.png "Todo-Anwendung: zweiten Seite")

Jedes Thema in diesem Handbuch enthält einen Link zum Herunterladen einer *verschiedene* Version der Anwendung, die einen bestimmten Typ von Web-Dienst-Back-End veranschaulicht. Herunterladen des relevante Codes auf der Seite, die im Zusammenhang mit jeder Webdienst-Stil.

## <a name="understanding-the-application-anatomy"></a>Grundlegendes zu den Aufbau der Anwendung

PCL-Projekt für jede beispielanwendung besteht aus drei Hauptordner:

|Ordner|Zweck|
|--- |--- |
|Daten|Enthält die Klassen und Schnittstellen, die zum Verwalten von Datenelementen und mit dem Webdienst kommunizieren. Mindestens dazu gehören die `TodoItemManager` -Klasse, die über eine Eigenschaft in verfügbar gemacht wird die `App` Klasse, um Webdienstvorgänge aufzurufen.|
|Modelle|Enthält die datenmodellklassen für die Anwendung an. Mindestens dazu gehören die `TodoItem` -Klasse, die ein einzelnes Element der von der Anwendung verwendeten Daten modelliert. Der Ordner kann auch verwendeten, um Benutzer Modelldaten zusätzlichen Klassen enthalten.|
|Ansichten|Enthält die Seiten für die Anwendung. Dies umfasst in der Regel die `TodoListPage` und `TodoItemPage` Klassen und zusätzlichen Klassen zum Zweck der Authentifizierung verwendet.|

PCL-Projekt für jede Anwendung umfasst auch eine Reihe wichtiger Dateien:

|Datei|Zweck|
|--- |--- |
|Constants.cs|Die `Constants` -Klasse, die alle Konstanten, die von der Anwendung für die Kommunikation mit dem Webdienst verwendeten angibt. Diese Konstanten erfordern, dass für einen Anbieter aktualisiert, um den Zugriff auf Ihre persönlichen Back-End-Dienst erstellt werden.|
|ITextToSpeech.cs|Die `ITextToSpeech` Schnittstelle, die angibt, dass die `Speak` Methode jeder beliebigen implementierenden Klasse bereitgestellt werden muss.|
|TODO.cs|Die `App` Klasse, die verantwortlich für das Instanziieren beide der ersten Seite, die von der Anwendung auf jeder Plattform angezeigt wird und die `TodoItemManager` -Klasse, die verwendet wird, um Webdienstvorgänge aufzurufen.|

### <a name="viewing-pages"></a>Anzeigen von Seiten

Die meisten der Beispielanwendungen enthalten mindestens zwei Seiten:

- **TodoListPage** – auf dieser Seite zeigt eine Liste der `TodoItem` Instanzen und einem Symbol "Tick" Wenn die `TodoItem.Done` Eigenschaft `true`. Durch Klicken auf ein Element navigiert zu der `TodoItemPage`. Darüber hinaus können neue Elemente erstellt werden, durch Klicken auf die *+* Symbol.
- **TodoItemPage** – auf dieser Seite zeigt die Details für den ausgewählten `TodoItem`, und bearbeitet, gespeichert, gelöscht, gesprochen und werden kann.

Darüber hinaus enthalten einige Beispielanwendungen zusätzliche Seiten, die verwendet werden, um den Benutzer-Authentifizierung zu verwalten.

### <a name="modeling-the-data"></a>Modellieren der Daten

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

Die `ID` Eigenschaft dient zur eindeutigen Identifizierung der einzelnen `TodoItem` -Instanz und wird durch jeden Webdienst verwendet, zum Identifizieren der Daten aktualisiert oder gelöscht werden sollen.

### <a name="invoking-web-service-operations"></a>Aufrufen von Webdienstvorgängen

Webdienstvorgänge erfolgt über die `TodoItemManager` -Klasse, und eine Instanz der Klasse möglich über die `App.TodoManager` Eigenschaft. Die `TodoItemManager` -Klasse stellt die folgenden Methoden, um Webdienstvorgänge aufzurufen:

- **GetTasksAsync** – diese Methode wird zum Auffüllen der `ListView` control für die `TodoListPage` mit der `TodoItem` Instanzen, die vom Webdienst abgerufen.
- **SaveTaskAsync** – diese Methode wird verwendet, erstellt oder aktualisiert eine `TodoItem` -Instanz auf den Webdienst.
- **DeleteTaskAsync** – diese Methode wird verwendet, um das Löschen einer `TodoItem` -Instanz auf den Webdienst.

Darüber hinaus enthalten einige Beispielanwendungen zusätzliche Methoden in der die `TodoItemManager` -Klasse, die zum Verwalten der Benutzerauthentifizierung verwendet werden.

Anstatt die Webdienstvorgänge direkt aufrufen der `TodoItemManager` Methoden Aufrufen von Methoden auf einer abhängigen Klasse, die in eingefügt wird die `TodoItemManager` Konstruktor. Z. B. eine beispielanwendung fügt die `RestService` Klasse in der `TodoItemManager` Konstruktor, um die Implementierung bereitzustellen, die REST-APIs für den Datenzugriff verwendet.

### <a name="translating-text-to-speech"></a>Übersetzen von Text in Sprache

Die meisten der Beispielanwendungen enthalten Speech (TTS) Funktionen, die Werte der sprechen die `TodoItem.Name` und `TodoItem.Notes` Eigenschaften. Dies erfolgt durch die `OnSpeakActivated` -Ereignishandler in der `TodoItemPage` Klasse, wie im folgenden Codebeispiel gezeigt:

```csharp
void OnSpeakActivated (object sender, EventArgs e)
{
    var todoItem = (TodoItem)BindingContext;
    App.Speech.Speak(todoItem.Name + " " + todoItem.Notes);
}
```

Diese Methode ruft einfach die `Speak` -Methode, die von einer plattformspezifischen implementiert wird `Speech` Klasse. Jede `Speech` -Klasse implementiert die `ITextToSpeech` -Schnittstelle, und plattformspezifische Startcode erstellt eine Instanz des der `Speech` -Klasse, die über zugegriffen werden kann die `App.Speech` Eigenschaft.

## <a name="summary"></a>Zusammenfassung

In diesem Thema bereitgestellten eine exemplarische Vorgehensweise für die Xamarin.Forms-beispielanwendung, die verwendet wird, veranschaulicht, wie Sie mit anderen Webdiensten kommunizieren. Während jeder Webdienst eine separate Anwendung verwendet, sie basieren alle auf die gleiche Logik der Benutzeroberfläche und Unternehmen wie oben beschrieben – nur der Web Service Mechanismus zur datenspeicherung unterscheidet.


## <a name="related-links"></a>Verwandte Links

- [ASMX-Version (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoASMX)
- [WCF-Version (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoWCF)
- [REST-Version (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST)
- [Azure-Version (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzure)
