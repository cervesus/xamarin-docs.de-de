---
title: Einführung in xamarin. Forms und Azure Cognitive Services
description: Dieser Artikel enthält eine Einführung in eine beispielanwendung, die zeigt, wie Sie einige der Microsoft Cognitive Services-APIs aufrufen.
ms.prod: xamarin
ms.assetid: 74121ADB-1322-4C1E-A103-F37257BC7CB0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 12802abe7b027f4e6d59abd62d2ae0611d71f438
ms.sourcegitcommit: ba83c107c87b015dbcc9db13964fe111a0573dca
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2020
ms.locfileid: "76265190"
---
# <a name="xamarinforms-and-azure-cognitive-services-introduction"></a>Einführung in xamarin. Forms und Azure Cognitive Services

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)

_Microsoft Cognitive Services sind ein Satz von APIs, sdert und Diensten, die Entwicklern zur Verfügung stehen, um Ihre Anwendungen intelligenter zu gestalten, indem Sie Features wie Gesichtserkennung, Spracherkennung und Sprachverständnis hinzufügen. Dieser Artikel bietet eine Einführung in die-Beispielanwendung, die veranschaulicht, wie einige der Microsoft Cognitive Service-APIs aufgerufen werden._

## <a name="overview"></a>Übersicht über

Das zugehörige Beispiel ist eine Todolist-Anwendung, die für Funktionen bereitstellt:

- Anzeigen einer Liste von Aufgaben.
- Hinzufügen und Bearbeiten von Aufgaben über die Bildschirmtastatur oder Spracherkennung mit der Microsoft Speech-API ausführt.
- Schreiben Sie Check-Aufgaben, die mit der Bing-Rechtschreibprüfungs-API aus. Weitere Informationen finden Sie unter [Rechtschreibprüfung mithilfe der Bing-Rechtschreibprüfungs-API](spell-check.md).
- Übersetzen Sie Aufgaben aus dem englischen auf Deutsch die Translator-API verwenden. Weitere Informationen finden Sie unter [Übersetzung von Text mithilfe der Translator-API](text-translation.md).
- Löschen von Aufgaben.
- Festlegen des Aufgabenstatus auf "Fertig".
- Bewerten Sie die Anwendung durch die emotionserkennung, mit der Gesichtserkennungs-API. Weitere Informationen finden Sie unter [mithilfe der Gesichtserkennungs-API zur Erkennung von Emotionen](emotion-recognition.md).

> [!WARNING]
> Der Bing-Spracheingabe-API ist zugunsten des Azure-sprach Dienstanbieter veraltet. Ein Beispiel für den Azure Speech Service finden Sie unter [Spracherkennung mit der sprach Erkennungsdienst-API](~/xamarin-forms/data-cloud/azure-cognitive-services/speech-recognition.md).

Aufgaben werden in einer lokalen SQLite-Datenbank gespeichert. Weitere Informationen zur Verwendung einer lokalen SQLite-Datenbank finden Sie unter [arbeiten mit einer lokalen Datenbank](~/xamarin-forms/data-cloud/data/databases.md).

Die `TodoListPage` wird angezeigt, wenn die Anwendung gestartet wird. Diese Seite zeigt eine Liste aller Aufgaben, die in der lokalen Datenbank gespeichert, und ermöglicht es dem Benutzer um einen neuen Task erstellen oder bewerten Sie die Anwendung:

![](introduction-images/sample-application-1.png "TodoListPage")

Neue Elemente erstellt werden, indem Sie durch Klicken auf die *+* Schaltfläche, die Navigieren zu den `TodoItemPage`. Auf dieser Seite kann auch zu dem navigiert werden durch eine Aufgabe auswählen:

![](introduction-images/sample-application-2.png "TodoItemPage")

Die `TodoItemPage` können Sie Aufgaben erstellt, bearbeitet, Rechtschreibung überprüft, übersetzt, gespeichert und gelöscht. Spracherkennung kann zum Erstellen oder Bearbeiten einer Aufgabe verwendet werden. Dies wird durch Drücken der Schaltfläche "Mikrofon" zum Starten der Aufzeichnung, und drücken Sie die gleiche Schaltfläche ein zweites Mal Beenden der Aufzeichnung, erreicht, der die Aufzeichnung der Bing-Spracheingabe-Erkennung-API sendet.

Klicken auf die Schaltfläche mit den Smileys der `TodoListPage` navigiert zu der `RateAppPage`, dient zur Erkennung von Emotionen auf ein Bild der Gesichtsausdrücke ausführen:

![](introduction-images/sample-application-3.png "RateAppPage")

Die `RateAppPage` ermöglicht dem Benutzer ein Foto ihrer Fläche, die an der Gesichtserkennungs-API gesendet wird, mit der zurückgegebenen Emotionen, die angezeigt wird.

## <a name="understand-the-application-anatomy"></a>Grundlegendes zur Anwendungs Anatomie

Das Projekt mit dem freigegebenen Code für die Beispielanwendung besteht aus fünf Haupt Ordnern:

|Folder|Zweck|
|--- |--- |
|Models|Enthält die datenmodellklassen für die Anwendung an. Dies schließt die `TodoItem` -Klasse, die ein einzelnes Element der von der Anwendung verwendeten Daten modelliert. Der Ordner enthält auch Klassen, mit denen Modell JSON-Antworten von anderen Microsoft Cognitive Services-APIs zurückgegeben.|
|Repositorys|Enthält die `ITodoItemRepository` Schnittstelle und `TodoItemRepository` -Klasse, die verwendet werden, um Datenbankvorgänge durchzuführen.|
|Dienste|Enthält die Schnittstellen und Klassen, die für den Zugriff auf andere Microsoft Cognitive Services-APIs, mit Schnittstellen, mit denen, die `DependencyService` Klasse, um die Klassen zu suchen, die die Schnittstellen in Plattform-Projekten.|
|"Utils"|Enthält die `Timer` -Klasse, die von verwendet wird, die `AuthenticationService` Klasse, um ein JWT-Zugriffstoken 9 Minuten erneuern.|
|Ansichten|Enthält die Seiten für die Anwendung.|

Das Projekt mit frei gegebenem Code enthält auch einige wichtige Dateien:

|File|Zweck|
|--- |--- |
|Constants.cs|Die `Constants` -Klasse, die die API-Schlüssel und Endpunkten für das Microsoft Cognitive Services-APIs gibt an, die aufgerufen werden. Die API-Schlüssel Konstanten für den Zugriff auf die anderen Cognitive Services-APIs aktualisiert werden müssen.|
|App.xaml.cs|Die `App` -Klasse ist verantwortlich für das Instanziieren beide der ersten Seite, die von der Anwendung auf jeder Plattform angezeigt wird und die `TodoManager` -Klasse, die zum Aufrufen von Datenbankvorgängen verwendet wird.|

### <a name="nuget-packages"></a>NuGet-Pakete

Die beispielanwendung verwendet die folgenden NuGet-Pakete:

- `Newtonsoft.Json` – Stellt ein JSON-Framework für .NET.
- `PCLStorage` – bietet eine Reihe von plattformübergreifenden lokale Datei-e/a-APIs.
- `sqlite-net-pcl` – ermöglicht die Speicherung der SQLite-Datenbank.
- `Xam.Plugin.Media` – bietet plattformübergreifende Foto aufnehmen und Auswählen von APIs.

Diese NuGet-Pakete wird darüber hinaus auch ihre eigenen Abhängigkeiten installieren.

### <a name="model-the-data"></a>Modellieren der Daten

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

Die `ID` Eigenschaft dient zur eindeutigen Identifizierung der einzelnen `TodoItem` -Instanz, und durch SQLite-Attribute, die der Eigenschaft einen automatisch inkrementierten Primärschlüssel in der Datenbank vorzunehmen.

### <a name="invoke-database-operations"></a>Daten Bank Vorgänge aufrufen

Die `TodoItemRepository` -Klasse implementiert die Datenbankvorgänge aus, und eine Instanz der Klasse zugegriffen werden kann, über die `App.TodoManager` Eigenschaft. Die `TodoItemRepository` Klasse stellt die folgenden Methoden zum Aufrufen von Datenbankvorgängen:

- **GetAllItemsAsync** : Ruft alle Elemente aus der lokalen SQLite-Datenbank ab.
- **GetItemAsync** – Ruft ein angegebenes Element aus der lokalen SQLite-Datenbank ab.
- **SaveItemAsync** – erstellt oder aktualisiert ein Element in der lokalen SQLite-Datenbank.
- **DeleteItemAsync** – löscht das angegebene Element aus der lokalen SQLite-Datenbank.

### <a name="platform-project-implementations"></a>Platt Form Projekt Implementierungen

Der Ordner "`Services`" im Projekt mit frei gegebenem Code enthält die `IFileHelper` und `IAudioRecorderService` Schnittstellen, die von der `DependencyService` Klasse verwendet werden, um die Klassen zu finden, die die Schnittstellen in Platt Form Projekten implementieren.

Die `IFileHelper` Schnittstelle wird implementiert, indem die `FileHelper` Klasse in jede plattformprojekt. Diese Klasse besteht aus einer einzelnen Methode, `GetLocalFilePath`, die einen lokalen Pfad zum Speichern der SQLite-Datenbank zurückgibt.

Die `IAudioRecorderService` Schnittstelle wird implementiert, indem die `AudioRecorderService` Klasse in jede plattformprojekt. Diese Klasse besteht aus `StartRecording`, `StopRecording`, und unterstützen von Methoden, die das Gerätemikrofon mit Plattform-APIs können Sie Audio aufzeichnen und speichern es als eine Wav-Datei. Unter iOS die `AudioRecorderService` verwendet die `AVFoundation` -API, um Audio aufzeichnen. Unter Android die `AudioRecordService` verwendet die `AudioRecord` -API, um Audio aufzeichnen. Auf der universellen Windows-Plattform (UWP), die `AudioRecorderService` verwendet die `AudioGraph` -API, um Audio aufzeichnen.

### <a name="invoke-cognitive-services"></a>Aufrufen von Cognitive Services

Die beispielanwendung ruft folgende Microsoft Cognitive Services:

- Microsoft-Spracheingabe-API. Weitere Informationen finden Sie unter [Spracherkennung, die mithilfe der Microsoft Speech API](speech-recognition.md).
- Bing-Rechtschreibprüfungs-API. Weitere Informationen finden Sie unter [Rechtschreibprüfung mithilfe der Bing-Rechtschreibprüfungs-API](spell-check.md).
- Übersetzungs-API. Weitere Informationen finden Sie unter [Übersetzung von Text mithilfe der Translator-API](text-translation.md).
- Gesichtserkennungs-API. Weitere Informationen finden Sie unter [mithilfe der Gesichtserkennungs-API zur Erkennung von Emotionen](emotion-recognition.md).

## <a name="related-links"></a>Verwandte Links

- [Spracherkennung mit der Sprachdienst-API](~/xamarin-forms/data-cloud/azure-cognitive-services/speech-recognition.md)
- [Microsoft Cognitive Services-Dokumentation](https://www.microsoft.com/cognitive-services/documentation)
- [TODO-Cognitive-Services (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)
