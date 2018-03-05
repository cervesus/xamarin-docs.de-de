---
title: "Hinzufügen von Intelligenz mit Cognitive-Diensten"
description: "Microsoft-Cognitive sind ein Satz von APIs, SDKs und Dienste, die für Entwickler ihre Anwendungen intelligenter machen, indem Sie zusätzliche Funktionen wie gesichtserkennung, Spracherkennung und Sprache Verständnis verfügbar. Dieser Artikel bietet eine Einführung in die beispielanwendung, die zeigt, wie einige der Microsoft Cognitive-Dienst-APIs aufgerufen werden."
ms.topic: article
ms.prod: xamarin
ms.assetid: 74121ADB-1322-4C1E-A103-F37257BC7CB0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 650f8dceebb088b3601c21c1f5373fc4ae8c76dc
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="adding-intelligence-with-cognitive-services"></a>Hinzufügen von Intelligenz mit Cognitive-Diensten

_Microsoft-Cognitive sind ein Satz von APIs, SDKs und Dienste, die für Entwickler ihre Anwendungen intelligenter machen, indem Sie zusätzliche Funktionen wie gesichtserkennung, Spracherkennung und Sprache Verständnis verfügbar. Dieser Artikel bietet eine Einführung in die beispielanwendung, die zeigt, wie einige der Microsoft Cognitive-Dienst-APIs aufgerufen werden._

## <a name="overview"></a>Übersicht

Das begleitende Beispiel ist eine Todo List-Anwendung, die für Funktionen bereitstellt:

- Eine Liste der Aufgaben anzuzeigen.
- Hinzufügen und Bearbeiten von Aufgaben über die Bildschirmtastatur oder Spracherkennung mit der Bing-Speech-API ausführen. Weitere Informationen zum Ausführen von der Spracherkennung finden Sie unter [Spracherkennung mithilfe der Bing-Speech-API](speech-recognition.md).
- Rechtschreibprüfungs-Tasks zum Überprüfen der mithilfe der Bing Rechtschreibprüfung aktivieren-API. Weitere Informationen finden Sie unter [Rechtschreibprüfung mithilfe der Bing Rechtschreibprüfung aktivieren-API](spell-check.md).
- Übersetzen Sie Aufgaben aus dem englischen auf Deutsch mithilfe der Konvertierer-API. Weitere Informationen finden Sie unter [Textübersetzung mithilfe der API-Konvertierungsprogramm](text-translation.md).
- Löschen von Aufgaben.
- Aufgabe mit dem Status auf "Fertig" festgelegt.
- Bewerten Sie die Anwendung mit Emotionen Recognition, mithilfe der Emotionen-API. Weitere Informationen finden Sie unter [Emotionen Recognition mithilfe der API Emotionen](emotion-recognition.md).

Aufgaben werden in einer lokalen SQLite-Datenbank gespeichert. Weitere Informationen zur Verwendung einer lokalen SQLite-Datenbank finden Sie unter [arbeiten mit einer lokalen Datenbank](~/xamarin-forms/app-fundamentals/databases.md).

Die `TodoListPage` wird angezeigt, wenn die Anwendung gestartet wird. Diese Seite zeigt eine Liste aller Aufgaben, die in der lokalen Datenbank gespeichert, und ermöglicht es den Benutzer, einen neuen Task erstellen oder um die Anwendung zu bewerten:

![](images/sample-application-1.png "TodoListPage")

Neue Elemente können erstellt werden, indem Sie auf die  *+*  Schaltfläche navigiert zu der `TodoItemPage`. Auf dieser Seite kann auch zu der navigiert werden durch eine Aufgabe auswählen:

![](images/sample-application-2.png "TodoItemPage")

Die `TodoItemPage` Aufgaben erstellt, bearbeitet, wird die Rechtschreibprüfung geprüft, ermöglicht, übersetzt, gespeichert und gelöscht. Spracherkennung kann zum Erstellen oder Bearbeiten einer Aufgabe verwendet werden. Dies wird durch Drücken der Taste Mikrofon zum Starten der Aufzeichnung und durch Drücken der Schaltfläche "Angleichen" ein zweites Mal beim Beenden der Aufzeichnung, erreicht der sendet der Aufzeichnung der Bing-Sprache Recognition-API.

Klicken auf die Schaltfläche "Smileys" die `TodoListPage` navigiert zu der `RateAppPage`, der verwendet wird, Emotionen Recognition in einem Bild eines Gesichtsausdrucks ausführen:

![](images/sample-application-3.png "RateAppPage")

Die `RateAppPage` ermöglicht es dem Benutzer ein Foto des ihre Smiley, werden die an die Emotionen-API mit den zurückgegebenen Emotionen angezeigt wird gesendet wird.

## <a name="understanding-the-application-anatomy"></a>Grundlegendes zu den Aufbau der Anwendung

Das Projekt "Portable Klassenbibliothek (PCL)" für die beispielanwendung besteht aus fünf Hauptordner:

<table>
    <thead>
        <tr><td><strong>Ordner</strong></td><td><strong>Purpose</strong></td></tr>
    </thead>
    <tbody>
        <tr>
            <td><strong>Modelle</strong></td>
            <td>Enthält die Klassen des Daten-Modell für die Anwendung. Dies schließt die <code>TodoItem</code> -Klasse, die ein einzelnes Element der Daten, die von der Anwendung verwendeten modelliert. Der Ordner enthält auch Klassen, mit denen Modell JSON-Antworten von anderen Microsoft Cognitive-Dienst-APIs zurückgegeben.</td>
        </tr>
        <tr>
            <td><strong>Repositorys</strong></td>
                        <td>Enthält die <code>ITodoItemRepository</code> Schnittstelle und <code>TodoItemRepository</code> Klasse, die verwendet werden, um Datenbankvorgänge durchzuführen.</td>
        </tr>
        <tr>
            <td><strong>Dienste</strong></td>
                        <td>Enthält die Schnittstellen und Klassen, mit denen verschiedene Microsoft Cognitive Dienst-APIs, mit Schnittstellen zuzugreifen, mit denen, die <code>DependencyService</code> Klasse, um die Klassen zu suchen, die in Projekten für die Plattform die Schnittstellen implementieren.</td>
        </tr>
        <tr>
            <td><strong>odatautils</strong></td>
            <td>Enthält die <code>Timer</code> -Klasse, die von verwendet wird, die <code>AuthenticationService</code> Klasse, um ein JWT-Zugriffstoken Minuten 9 erneuern.</td>
        </tr>
        <tr>
            <td><strong>Ansichten</strong></td>
            <td>Enthält die Seiten für die Anwendung.</td>
        </tr>
    </tbody>
</table>

Die PCL-Projekt enthält außerdem einige wichtigen Dateien:

<table>
    <thead>
      <tr><td><strong>Datei</strong></td><td><strong>Purpose</strong></td></tr>
    <thead>
    <tbody>
        <tr>
            <td><strong>Constants.cs</strong></td>
            <td>Die <code>Constants</code> -Klasse, die die API-Schlüssel und die Endpunkte für die Microsoft Cognitive-Dienst-APIs gibt an, die aufgerufen werden. Die API-Schlüssel Konstanten müssen für den Zugriff auf die verschiedenen Cognitive-APIs aktualisiert werden.
        </tr>
        <tr>
          <td><strong>App.xaml.cs</strong></td>
          <td>Die <code>App</code> Klasse dient zum Instanziieren der ersten Seite, die von der Anwendung auf jeder Plattform angezeigt werden und die <code>TodoManager</code> -Klasse, die zum Aufrufen von Datenbankvorgängen verwendet wird.</td>
        </tr>
    </tbody>
</table>

### <a name="nuget-packages"></a>NuGet-Pakete

Die beispielanwendung verwendet die folgenden NuGet-Pakete:

- `Microsoft.Net.Http` – Stellt die `HttpClient` Klasse zum Senden von Anforderungen über HTTP.
- `Newtonsoft.Json` – Stellt ein JSON-Framework für .NET bereit.
- `Microsoft.ProjectOxford.Emotion` – eine Clientbibliothek für den Zugriff auf die Emotionen-API.
- `PCLStorage` – bietet eine Reihe von plattformübergreifenden lokale Datei-e/a-APIs.
- `sqlite-net-pcl` – ermöglicht die Speicherung der SQLite-Datenbank.
- `Xam.Plugin.Media` – bietet plattformübergreifende Foto verwendet wird und APIs entnehmen.

Darüber hinaus installieren diese NuGet-Pakete auch ihre eigenen Abhängigkeiten.

### <a name="modeling-the-data"></a>Modellieren von Daten

Die beispielanwendung verwendet die `TodoItem` Klasse, um die Daten zu modellieren, die angezeigt und in der lokalen SQLite-Datenbank gespeichert. Das folgende Codebeispiel zeigt die `TodoItem`-Klasse:

```csharp
public class TodoItem
{
  [PrimaryKey, AutoIncrement]
  public int ID { get; set; }
  public string Name { get; set; }
  public bool Done { get; set; }
}
```

Die `ID` Eigenschaft dient zur eindeutigen Identifizierung der einzelnen `TodoItem` -Instanz und mit SQLite-Attribute, die der Eigenschaft einen automatisch erhöhenden Primärschlüssel in der Datenbank vornehmen ergänzt wird.

### <a name="invoking-database-operations"></a>Aufrufen von Datenbankvorgängen

Die `TodoItemRepository` Klasse implementiert die Datenbankvorgänge aus, und durch eine Instanz der Klasse zugegriffen werden kann die `App.TodoManager` Eigenschaft. Die `TodoItemRepository` Klasse bietet die folgenden Methoden zum Aufrufen von Datenbankvorgängen:

- **GetAllItemsAsync** – alle Elemente aus der lokalen SQLite-Datenbank abgerufen.
- **GetItemAsync** – ein angegebenes Element aus der lokalen SQLite-Datenbank abgerufen.
- **SaveItemAsync** – erstellt oder aktualisiert ein Element in der lokalen SQLite-Datenbank.
- **DeleteItemAsync** – löscht das angegebene Element aus der lokalen SQLite-Datenbank.

### <a name="platform-project-implementations"></a>Implementierungen der Plattform-Projekt

Die `Services` "in der PCL-Projekt enthält die `IFileHelper` und `IAudioRecorderService` Schnittstellen, mit denen, die `DependencyService` Klasse, um die Klassen zu suchen, die in Projekten für die Plattform die Schnittstellen implementieren.

Die `IFileHelper` Schnittstelle wird implementiert, indem die `FileHelper` Klasse in jede plattformprojekt. Diese Klasse besteht aus einer einzelnen Methode, `GetLocalFilePath`, womit auf einen lokalen Pfad zum Speichern von SQLite-Datenbank.

Die `IAudioRecorderService` Schnittstelle wird implementiert, indem die `AudioRecorderService` Klasse in jede plattformprojekt. Diese Klasse besteht aus `StartRecording`, `StopRecording`, und unterstützen von Methoden, die mit der Audio Aufzeichnen von dem Gerät Mikrofon Plattform-APIs und als Wav-Datei zu speichern. Bei iOS kann die `AudioRecorderService` verwendet die `AVFoundation` -API, um Audio aufzeichnen. Auf Android-Geräten die `AudioRecordService` verwendet die `AudioRecord` -API, um Audio aufzeichnen. Auf die universelle Windows-Plattform (UWP), die `AudioRecorderService` verwendet die `AudioGraph` -API, um Audio aufzeichnen.

### <a name="invoking-cognitive-services"></a>Aufrufen von Webdiensten Cognitive

Die beispielanwendung Ruft die folgenden Cognitive Microsoft-Dienste:

- Bing-Speech-API. Weitere Informationen finden Sie unter [Spracherkennung mithilfe der Bing-Speech-API](speech-recognition.md).
- Bing-Rechtschreibprüfung API. Weitere Informationen finden Sie unter [Rechtschreibprüfung mithilfe der Bing Rechtschreibprüfung aktivieren-API](spell-check.md).
- API zu übersetzen. Weitere Informationen finden Sie unter [Textübersetzung mithilfe der API-Konvertierungsprogramm](text-translation.md).
- Emotionen-API. Weitere Informationen finden Sie unter [Emotionen Recognition mithilfe der API Emotionen](emotion-recognition.md).


## <a name="related-links"></a>Verwandte Links

- [Microsoft Cognitive Services-Dokumentation](https://www.microsoft.com/cognitive-services/documentation)
- [TODO-Cognitive Services (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
