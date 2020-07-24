---
title: Xamarin.Formsund Azure Cognitive Services Introduction
description: Dieser Artikel bietet eine Einführung in eine Beispielanwendung, die veranschaulicht, wie einige der Microsoft Cognitive Service-APIs aufgerufen werden.
ms.prod: xamarin
ms.assetid: 74121ADB-1322-4C1E-A103-F37257BC7CB0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: cce5b0fc9c3d1d04c20b1be242197e3bc9e4f901
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86929336"
---
# <a name="xamarinforms-and-azure-cognitive-services-introduction"></a>Xamarin.Formsund Azure Cognitive Services Introduction

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)

_Microsoft Cognitive Services sind ein Satz von APIs, sdert und Diensten, die Entwicklern zur Verfügung stehen, um Ihre Anwendungen intelligenter zu gestalten, indem Sie Features wie Gesichtserkennung, Spracherkennung und Sprachverständnis hinzufügen. Dieser Artikel bietet eine Einführung in die-Beispielanwendung, die veranschaulicht, wie einige der Microsoft Cognitive Service-APIs aufgerufen werden._

## <a name="overview"></a>Übersicht

Das zugehörige Beispiel ist eine ToDo-Listen Anwendung, die Funktionen für Folgendes bereitstellt:

- Anzeigen einer Liste von Tasks.
- Fügen Sie Aufgaben über die weiche Tastatur hinzu, oder bearbeiten Sie Sie, oder führen Sie die Spracherkennung mit der Microsoft Speech-API durch.
- Rechtschreibprüfung für Aufgaben mit der Bing-Rechtschreibprüfung-API. Weitere Informationen finden Sie unter [Rechtschreibprüfung mit der Bing-Rechtschreibprüfung-API](spell-check.md).
- Übersetzen Sie mithilfe der Translator-API Aufgaben aus Englisch in Deutsch. Weitere Informationen finden Sie unter [Text Übersetzung mithilfe der Translator-API](text-translation.md).
- Löschen von Tasks.
- Legen Sie den Status einer Aufgabe auf "Done" fest.
- Bewerten Sie die Anwendung mit der Emotions Erkennung, indem Sie die Gesichtserkennungs-API verwenden. Weitere Informationen finden Sie unter [Emotions Erkennung mithilfe der Gesichtserkennungs-API](emotion-recognition.md).

> [!WARNING]
> Der Bing-Spracheingabe-API ist zugunsten des Azure-sprach Dienstanbieter veraltet. Ein Beispiel für den Azure Speech Service finden Sie unter [Spracherkennung mit der sprach Erkennungsdienst-API](~/xamarin-forms/data-cloud/azure-cognitive-services/speech-recognition.md).

Tasks werden in einer lokalen SQLite-Datenbank gespeichert. Weitere Informationen zum Verwenden einer lokalen SQLite-Datenbank finden Sie unter [Arbeiten mit einer lokalen Datenbank](~/xamarin-forms/data-cloud/data/databases.md).

`TodoListPage`Wird angezeigt, wenn die Anwendung gestartet wird. Auf dieser Seite wird eine Liste aller Aufgaben angezeigt, die in der lokalen Datenbank gespeichert sind, und der Benutzer kann eine neue Aufgabe erstellen oder die Anwendung bewerten:

!["-Listpage"](introduction-images/sample-application-1.png)

Neue Elemente können erstellt werden, indem Sie auf die *+* Schaltfläche klicken, die zu der navigiert `TodoItemPage` . Auf diese Seite kann auch durch Auswählen einer Aufgabe navigiert werden:

!["" Zu ""](introduction-images/sample-application-2.png)

`TodoItemPage`Ermöglicht das Erstellen, bearbeiten, überschreiben, übersetzen, speichern und Löschen von Tasks. Die Spracherkennung kann verwendet werden, um eine Aufgabe zu erstellen oder zu bearbeiten. Dies wird erreicht, indem Sie auf die Mikrofon Schaltfläche klicken, um die Aufzeichnung zu starten, und indem Sie die gleiche Schaltfläche ein zweites Mal drücken, um Bing-Spracheingabe die Aufzeichnung zu unterbinden

Durch Klicken auf die Schaltfläche Smilies auf der `TodoListPage` navigiert zum `RateAppPage` , das zum Durchführen der Emotions Erkennung für ein Bild eines Gesichtsausdrucks verwendet wird:

![Rateapppage](introduction-images/sample-application-3.png)

`RateAppPage`Ermöglicht es dem Benutzer, ein Foto seines Gesichts zu erfassen, das an die Gesichtserkennungs-API übermittelt wird, wenn die zurückgegebene Emotion angezeigt wird.

## <a name="understand-the-application-anatomy"></a>Grundlegendes zur Anwendungs Anatomie

Das Projekt mit dem freigegebenen Code für die Beispielanwendung besteht aus fünf Haupt Ordnern:

|Ordner|Zweck|
|--- |--- |
|Modelle|Enthält die Datenmodell Klassen für die Anwendung. Dies schließt die- `TodoItem` Klasse ein, die ein einzelnes Datenelement modelliert, das von der Anwendung verwendet wird. Der Ordner enthält auch Klassen, mit denen JSON-Antworten modelliert werden, die von verschiedenen Microsoft Cognitive Service-APIs zurückgegeben werden.|
|Repositorys|Enthält die `ITodoItemRepository` Schnittstelle und die `TodoItemRepository` Klasse, die zum Ausführen von Daten Bank Vorgängen verwendet werden.|
|Dienste|Enthält die Schnittstellen und Klassen, die für den Zugriff auf verschiedene Microsoft Cognitive Service-APIs verwendet werden, sowie Schnittstellen, die von der-Klasse verwendet werden, `DependencyService` um die Klassen zu finden, die die Schnittstellen in Platt Form Projekten implementieren.|
|Utils|Enthält die- `Timer` Klasse, die von der-Klasse verwendet wird, `AuthenticationService` um ein JWT-Zugriffs Token alle 9 Minuten zu erneuern.|
|Sichten|Enthält die Seiten für die Anwendung.|

Das Projekt mit frei gegebenem Code enthält auch einige wichtige Dateien:

|Datei|Zweck|
|--- |--- |
|Constants.cs|Die- `Constants` Klasse, die die API-Schlüssel und-Endpunkte für die aufgerufenen Microsoft Cognitive Service-APIs angibt. Die API-Schlüssel Konstanten erfordern das Aktualisieren, um auf die verschiedenen Cognitive Service-APIs zuzugreifen.|
|App.xaml.cs|Die `App` -Klasse ist für das Instanziieren sowohl der ersten Seite, die von der Anwendung auf jeder Plattform angezeigt wird, als auch der-Klasse, die `TodoManager` zum Aufrufen von Daten Bank Vorgängen verwendet wird, verantwortlich.|

### <a name="nuget-packages"></a>NuGet-Pakete

Die Beispielanwendung verwendet die folgenden nuget-Pakete:

- `Newtonsoft.Json`– bietet ein JSON-Framework für .net.
- `PCLStorage`– bietet eine Reihe von plattformübergreifenden lokalen Datei-e/a-APIs.
- `sqlite-net-pcl`– bietet SQLite-Daten Bank Speicher.
- `Xam.Plugin.Media`– bietet plattformübergreifende Foto-und Auswahl-APIs.

Außerdem installieren diese nuget-Pakete auch Ihre eigenen Abhängigkeiten.

### <a name="model-the-data"></a>Modellieren der Daten

Die Beispielanwendung verwendet die `TodoItem` -Klasse, um die Daten zu modellieren, die in der lokalen SQLite-Datenbank angezeigt und gespeichert werden. Das folgende Codebeispiel zeigt die `TodoItem`-Klasse:

```csharp
public class TodoItem
{
  [PrimaryKey, AutoIncrement]
  public int ID { get; set; }
  public string Name { get; set; }
  public bool Done { get; set; }
}
```

Die `ID` -Eigenschaft wird verwendet, um jede Instanz eindeutig zu identifizieren `TodoItem` , und wird mit SQLite-Attributen versehen, die die Eigenschaft in der Datenbank zu einem automatisch inkrementierenden Primärschlüssel machen.

### <a name="invoke-database-operations"></a>Daten Bank Vorgänge aufrufen

Die `TodoItemRepository` -Klasse implementiert Daten Bank Vorgänge, und über die-Eigenschaft kann auf eine Instanz der-Klasse zugegriffen werden `App.TodoManager` . Die- `TodoItemRepository` Klasse stellt die folgenden Methoden zum Aufrufen von Daten Bank Vorgängen bereit:

- **Getallitemsasync** – Ruft alle Elemente aus der lokalen SQLite-Datenbank ab.
- **Getitemasync – Ruft ein angegebenes** Element aus der lokalen SQLite-Datenbank ab.
- **Saveitemasync** – erstellt oder aktualisiert ein Element in der lokalen SQLite-Datenbank.
- **Deleteitemasync** – löscht das angegebene Element aus der lokalen SQLite-Datenbank.

### <a name="platform-project-implementations"></a>Platt Form Projekt Implementierungen

Der `Services` Ordner im freigegebenen Code Projekt enthält die `IFileHelper` -und- `IAudioRecorderService` Schnittstellen, die von der-Klasse verwendet werden, `DependencyService` um die Klassen zu suchen, die die Schnittstellen in Platt Form Projekten implementieren.

Die- `IFileHelper` Schnittstelle wird von der- `FileHelper` Klasse in jedem Platt Form Projekt implementiert. Diese Klasse besteht aus einer einzelnen Methode, `GetLocalFilePath` , die einen lokalen Dateipfad zum Speichern der SQLite-Datenbank zurückgibt.

Die- `IAudioRecorderService` Schnittstelle wird von der- `AudioRecorderService` Klasse in jedem Platt Form Projekt implementiert. Diese Klasse besteht aus `StartRecording` den `StopRecording` Methoden, und, die Platt Form-APIs verwenden, um Audiodaten vom Mikrofon des Geräts aufzuzeichnen und als WAV-Datei zu speichern. Unter IOS verwendet die `AudioRecorderService` API, `AVFoundation` um Audiodaten aufzuzeichnen. Unter Android verwendet die `AudioRecordService` API, `AudioRecord` um Audiodaten aufzuzeichnen. Beim universelle Windows-Plattform (UWP) wird die `AudioRecorderService` `AudioGraph` -API verwendet, um Audiodaten aufzuzeichnen.

### <a name="invoke-cognitive-services"></a>Aufrufen von Cognitive Services

Die Beispielanwendung Ruft die folgenden Microsoft Cognitive Services auf:

- Microsoft Speech-API. Weitere Informationen finden Sie unter [Spracherkennung mithilfe der Microsoft Speech-API](speech-recognition.md).
- Bing-Rechtschreibprüfung-API. Weitere Informationen finden Sie unter [Rechtschreibprüfung mit der Bing-Rechtschreibprüfung-API](spell-check.md).
- API übersetzen. Weitere Informationen finden Sie unter [Text Übersetzung mithilfe der Translator-API](text-translation.md).
- Gesichtserkennungs-API. Weitere Informationen finden Sie unter [Emotions Erkennung mithilfe der Gesichtserkennungs-API](emotion-recognition.md).

## <a name="related-links"></a>Verwandte Links

- [Spracherkennung mit der Sprachdienst-API](~/xamarin-forms/data-cloud/azure-cognitive-services/speech-recognition.md)
- [Dokumentation zu Microsoft Cognitive Services](https://www.microsoft.com/cognitive-services/documentation)
- [TODO-Cognitive Services (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)
