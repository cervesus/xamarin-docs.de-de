---
title: Einführung in xamarin. Forms und Azure Cognitive Services
description: Dieser Artikel bietet eine Einführung in eine Beispielanwendung, die veranschaulicht, wie einige der Microsoft Cognitive Service-APIs aufgerufen werden.
ms.prod: xamarin
ms.assetid: 74121ADB-1322-4C1E-A103-F37257BC7CB0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 52774b387644b14e3d4612dffa6d3c3b28a37f25
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "68652310"
---
# <a name="xamarinforms-and-azure-cognitive-services-introduction"></a>Einführung in xamarin. Forms und Azure Cognitive Services

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)

_Microsoft Cognitive Services sind ein Satz von APIs, sdert und Diensten, die Entwicklern zur Verfügung stehen, um Ihre Anwendungen intelligenter zu gestalten, indem Sie Features wie Gesichtserkennung, Spracherkennung und Sprachverständnis hinzufügen. Dieser Artikel bietet eine Einführung in die-Beispielanwendung, die veranschaulicht, wie einige der Microsoft Cognitive Service-APIs aufgerufen werden._

## <a name="overview"></a>Übersicht

Das zugehörige Beispiel ist eine ToDo-Listen Anwendung, die Funktionen für Folgendes bereitstellt:

- Anzeigen einer Liste von Tasks.
- Fügen Sie Aufgaben über die weiche Tastatur hinzu, oder bearbeiten Sie Sie, oder führen Sie die Spracherkennung mit der Microsoft Speech-API durch. Weitere Informationen zum Durchführen der Spracherkennung finden [Sie unter Spracherkennung mithilfe der Microsoft Speech-API](speech-recognition.md).
- Rechtschreibprüfung für Aufgaben mit der Bing-Rechtschreibprüfung-API. Weitere Informationen finden Sie unter [Rechtschreibprüfung mit der Bing-Rechtschreibprüfung-API](spell-check.md).
- Übersetzen Sie mithilfe der Translator-API Aufgaben aus Englisch in Deutsch. Weitere Informationen finden Sie unter [Text Übersetzung mithilfe der Translator-API](text-translation.md).
- Löschen von Tasks.
- Legen Sie den Status einer Aufgabe auf "Done" fest.
- Bewerten Sie die Anwendung mit der Emotions Erkennung, indem Sie die Gesichtserkennungs-API verwenden. Weitere Informationen finden Sie unter [Emotions Erkennung mithilfe der Gesichtserkennungs-API](emotion-recognition.md).

Tasks werden in einer lokalen SQLite-Datenbank gespeichert. Weitere Informationen zum Verwenden einer lokalen SQLite-Datenbank finden Sie unter [Arbeiten mit einer lokalen Datenbank](~/xamarin-forms/data-cloud/data/databases.md).

Der `TodoListPage` wird angezeigt, wenn die Anwendung gestartet wird. Auf dieser Seite wird eine Liste aller Aufgaben angezeigt, die in der lokalen Datenbank gespeichert sind, und der Benutzer kann eine neue Aufgabe erstellen oder die Anwendung bewerten:

![](introduction-images/sample-application-1.png "TodoListPage")

Neue Elemente können erstellt werden, indem Sie auf die Schaltfläche *+* klicken, die zum `TodoItemPage` navigiert. Auf diese Seite kann auch durch Auswählen einer Aufgabe navigiert werden:

![](introduction-images/sample-application-2.png "TodoItemPage")

Der `TodoItemPage` ermöglicht das Erstellen, bearbeiten, überschreiben, übersetzen, speichern und Löschen von Tasks. Die Spracherkennung kann verwendet werden, um eine Aufgabe zu erstellen oder zu bearbeiten. Dies wird erreicht, indem Sie auf die Mikrofon Schaltfläche klicken, um die Aufzeichnung zu starten, und indem Sie die gleiche Schaltfläche ein zweites Mal drücken, um Bing-Spracheingabe die Aufzeichnung zu unterbinden

Wenn Sie auf die Schaltfläche Smilies im `TodoListPage` klicken, navigieren Sie zum `RateAppPage`, das zum Durchführen der Emotions Erkennung für ein Bild eines Gesichtsausdrucks verwendet wird:

![](introduction-images/sample-application-3.png "RateAppPage")

Der `RateAppPage` ermöglicht dem Benutzer, ein Foto seines Gesichts zu erfassen, das an die Gesichtserkennungs-API übermittelt wird, wenn die zurückgegebene Emotion angezeigt wird.

## <a name="understand-the-application-anatomy"></a>Grundlegendes zur Anwendungs Anatomie

Das Projekt mit dem freigegebenen Code für die Beispielanwendung besteht aus fünf Haupt Ordnern:

|Ordner|Zweck|
|--- |--- |
|Modelle|Enthält die Datenmodell Klassen für die Anwendung. Dies schließt die `TodoItem`-Klasse ein, die ein einzelnes Datenelement modelliert, das von der Anwendung verwendet wird. Der Ordner enthält auch Klassen, mit denen JSON-Antworten modelliert werden, die von verschiedenen Microsoft Cognitive Service-APIs zurückgegeben werden.|
|Stätten|Enthält die `ITodoItemRepository`-Schnittstelle und `TodoItemRepository` Klasse, die zum Ausführen von Daten Bank Vorgängen verwendet werden.|
|Dienste|Enthält die Schnittstellen und Klassen, die für den Zugriff auf verschiedene Microsoft Cognitive Service-APIs verwendet werden, zusammen mit Schnittstellen, die von der `DependencyService` Klasse verwendet werden, um die Klassen zu finden, die die Schnittstellen in Platt Form Projekten implementieren.|
|utils|Enthält die `Timer` Klasse, die von der `AuthenticationService`-Klasse verwendet wird, um ein JWT-Zugriffs Token alle 9 Minuten zu erneuern.|
|Ansichten|Enthält die Seiten für die Anwendung.|

Das Projekt mit frei gegebenem Code enthält auch einige wichtige Dateien:

|Datei|Zweck|
|--- |--- |
|Constants.cs|Die `Constants`-Klasse, die die API-Schlüssel und-Endpunkte für die aufgerufenen Microsoft Cognitive Service-APIs angibt. Die API-Schlüssel Konstanten erfordern das Aktualisieren, um auf die verschiedenen Cognitive Service-APIs zuzugreifen.|
|App.xaml.cs|Die `App`-Klasse ist verantwortlich für die Instanziierung sowohl der ersten Seite, die von der Anwendung auf jeder Plattform angezeigt wird, als auch der `TodoManager` Klasse, die zum Aufrufen von Daten Bank Vorgängen verwendet wird.|

### <a name="nuget-packages"></a>NuGet-Pakete

Die Beispielanwendung verwendet die folgenden nuget-Pakete:

- `Newtonsoft.Json` – bietet ein JSON-Framework für .net.
- `PCLStorage` – bietet eine Reihe von plattformübergreifenden lokalen Datei-e/a-APIs.
- `sqlite-net-pcl` – bietet SQLite-Daten Bank Speicher.
- `Xam.Plugin.Media` – bietet plattformübergreifende Foto-und Auswahl-APIs.

Außerdem installieren diese nuget-Pakete auch Ihre eigenen Abhängigkeiten.

### <a name="model-the-data"></a>Modellieren der Daten

Die Beispielanwendung verwendet die `TodoItem`-Klasse, um die Daten zu modellieren, die in der lokalen SQLite-Datenbank angezeigt und gespeichert werden. Das folgende Codebeispiel zeigt die `TodoItem`-Klasse:

```csharp
public class TodoItem
{
  [PrimaryKey, AutoIncrement]
  public int ID { get; set; }
  public string Name { get; set; }
  public bool Done { get; set; }
}
```

Die `ID`-Eigenschaft wird verwendet, um jede `TodoItem` Instanz eindeutig zu identifizieren, und wird mit SQLite-Attributen versehen, die die Eigenschaft in der Datenbank zu einem automatisch inkrementierenden Primärschlüssel machen.

### <a name="invoke-database-operations"></a>Daten Bank Vorgänge aufrufen

Die `TodoItemRepository`-Klasse implementiert Daten Bank Vorgänge, und über die `App.TodoManager`-Eigenschaft kann auf eine Instanz der-Klasse zugegriffen werden. Die `TodoItemRepository`-Klasse stellt die folgenden Methoden zum Aufrufen von Daten Bank Vorgängen bereit:

- **Getallitemsasync** – Ruft alle Elemente aus der lokalen SQLite-Datenbank ab.
- **Getitemasync – Ruft ein angegebenes** Element aus der lokalen SQLite-Datenbank ab.
- **Saveitemasync** – erstellt oder aktualisiert ein Element in der lokalen SQLite-Datenbank.
- **Deleteitemasync** – löscht das angegebene Element aus der lokalen SQLite-Datenbank.

### <a name="platform-project-implementations"></a>Platt Form Projekt Implementierungen

Der Ordner "`Services`" im Projekt mit frei gegebenem Code enthält die `IFileHelper` und `IAudioRecorderService` Schnittstellen, die von der `DependencyService` Klasse verwendet werden, um die Klassen zu finden, die die Schnittstellen in Platt Form Projekten implementieren.

Die `IFileHelper`-Schnittstelle wird von der `FileHelper`-Klasse in jedem Platt Form Projekt implementiert. Diese Klasse besteht aus einer einzelnen Methode, `GetLocalFilePath`, die einen lokalen Dateipfad zum Speichern der SQLite-Datenbank zurückgibt.

Die `IAudioRecorderService`-Schnittstelle wird von der `AudioRecorderService`-Klasse in jedem Platt Form Projekt implementiert. Diese Klasse besteht aus `StartRecording`-, `StopRecording`-und Unterstützungsmethoden, die Platt Form-APIs verwenden, um Audiodaten aus dem Mikrofon des Geräts aufzuzeichnen und als WAV-Datei zu speichern. Unter IOS verwendet die `AudioRecorderService` die `AVFoundation`-API, um Audiodaten aufzuzeichnen. Unter Android verwendet die `AudioRecordService` die `AudioRecord`-API, um Audiodaten aufzuzeichnen. Auf dem universelle Windows-Plattform (UWP) verwendet die `AudioRecorderService` die `AudioGraph`-API, um Audiodaten aufzuzeichnen.

### <a name="invoke-cognitive-services"></a>Aufrufen von Cognitive Services

Die Beispielanwendung Ruft die folgenden Microsoft Cognitive Services auf:

- Microsoft Speech-API. Weitere Informationen finden Sie unter [Spracherkennung mithilfe der Microsoft Speech-API](speech-recognition.md).
- Bing-Rechtschreibprüfung-API. Weitere Informationen finden Sie unter [Rechtschreibprüfung mit der Bing-Rechtschreibprüfung-API](spell-check.md).
- API übersetzen. Weitere Informationen finden Sie unter [Text Übersetzung mithilfe der Translator-API](text-translation.md).
- Gesichtserkennungs-API. Weitere Informationen finden Sie unter [Emotions Erkennung mithilfe der Gesichtserkennungs-API](emotion-recognition.md).

## <a name="related-links"></a>Verwandte Links

- [Dokumentation zu Microsoft Cognitive Services](https://www.microsoft.com/cognitive-services/documentation)
- [TODO-Cognitive Services (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)
