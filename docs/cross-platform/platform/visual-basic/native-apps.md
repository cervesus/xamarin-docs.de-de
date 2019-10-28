---
title: Visual Basic in xamarin. Android und xamarin. IOS
description: In dieser exemplarischen Vorgehensweise wird veranschaulicht, wie Native xamarin. IOS-und xamarin. Android-Apps erstellt werden, die in Visual Basic.NET geschriebene Geschäftslogik verwenden.
ms.prod: xamarin
ms.assetid: 455fda67-3879-4299-8036-b12840e6a498
author: conceptdev
ms.author: crdun
ms.date: 04/24/2019
ms.openlocfilehash: ea4dc91b262c2ae153088f6e1a8416cc01cb0fa9
ms.sourcegitcommit: f8583585c501607fdfa061b95e9a9f385ed1d591
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2019
ms.locfileid: "72959124"
---
# <a name="visual-basic-in-xamarin-android-and-ios"></a>Visual Basic in xamarin Android und IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/mobile-samples/visualbasic-taskyvb/)

Die [taskyvb](https://docs.microsoft.com/samples/xamarin/mobile-samples/visualbasic-taskyvb/) -Beispielanwendung veranschaulicht, wie Visual Basic Code, der in eine .NET Standard Bibliothek kompiliert wurde, mit xamarin verwendet werden kann. Im folgenden finden Sie einige Screenshots der sich ergebenden apps, die unter Android und IOS ausgeführt werden:

 [![Android und IOS, die eine mit Visual Basic erstellten app ausführen](native-apps-images/simulators-sml.png)](native-apps-images/simulators.png#lightbox)

Die Android-und IOS-Projekte im Beispiel werden in C#geschrieben. Die Benutzeroberfläche für jede Anwendung wird mit nativen Technologien erstellt, während die `TodoItem` Verwaltung von der Visual Basic .NET Standard Bibliothek mithilfe einer XML-Datei bereitgestellt wird (zu Demonstrationszwecken, nicht als vollständige Datenbank).

## <a name="sample-walkthrough"></a>Exemplarische Vorgehensweise

In dieser Anleitung wird erläutert, wie Visual Basic im [taskyvb](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyVB) xamarin-Beispiel für IOS und Android implementiert wurde.

> [!NOTE]
> Lesen Sie die Anweisungen auf [Visual Basic und .NET Standard](index.md) , bevor Sie mit diesem Handbuch fortfahren.
>
> Weitere Informationen zum Erstellen einer APP mit frei gegebener Benutzeroberfläche Visual Basic Code finden Sie unter [xamarin. Forms Visual Basic](xamarin-forms.md) Anweisungen.

## <a name="visualbasicnetstandard"></a>Visualbasicnetstandard

Visual Basic .NET Standard Bibliotheken können nur in Visual Studio unter Windows erstellt werden.
Die Beispiel Bibliothek enthält die Grundlagen der Anwendung in diesen Visual Basic Dateien:

- "Element. vb"
- "" "" "" "". Vb "
- TodoItemRepositoryXML. vb
- Xmlstorage. vb

### <a name="todoitemvb"></a>"Element. vb"

Diese Klasse enthält das Geschäftsobjekt, das in der gesamten Anwendung verwendet werden soll. Sie wird in Visual Basic definiert und für Android-und IOS-Projekte freigegeben, die in C#geschrieben werden.

Die Klassendefinition wird hier angezeigt:

```vb
Public Class TodoItem
    Property ID() As Integer
    Property Name() As String
    Property Notes() As String
    Property Done() As Boolean
End Class
```

Im Beispiel werden die XML-Serialisierung und die Deserialisierung verwendet, um die todoitem-Objekte zu laden und zu speichern.

### <a name="todoitemmanagervb"></a>"" "" "" "". Vb "

Die Manager-Klasse stellt die "API" für den portablen Code dar. Sie stellt grundlegende CRUD-Vorgänge für die `TodoItem`-Klasse bereit, aber keine Implementierung dieser Vorgänge.

```vb
Public Class TodoItemManager
    Private _repository As TodoItemRepositoryXML
    Public Sub New(filename As String)
        _repository = New TodoItemRepositoryXML(filename, storage)
    End Sub
    Public Function GetTask(id As Integer) As TodoItem
        Return _repository.GetTask(id)
    End Function
    Public Function GetTasks() As List(Of TodoItem)
        Return New List(Of TodoItem)(_repository.GetTasks())
    End Function
    Public Function SaveTask(item As TodoItem) As Integer
        Return _repository.SaveTask(item)
    End Function
    Public Function DeleteTask(item As TodoItem) As Integer
        Return _repository.DeleteTask(item.ID)
    End Function
End Class
```

Der-Konstruktor nimmt eine Instanz von ixmlstorage als Parameter an. Dadurch kann jede Plattform eine eigene funktionierende Implementierung bereitstellen, während der Portable Code andere Funktionen beschreiben kann, die freigegeben werden können.

### <a name="todoitemrepositoryvb"></a>"" "" "" "" ". Vb"

Die Repository-Klasse enthält die Logik zum Verwalten der Liste der Objekt Objekt Objekte. Der gesamte Code ist unten dargestellt – die Logik besteht hauptsächlich darin, einen eindeutigen ID-Wert in den todoitems zu verwalten, wenn Sie hinzugefügt und aus der Auflistung entfernt werden.

```vb
Public Class TodoItemRepositoryXML
    Private _filename As String
    Private _storage As IXmlStorage
    Private _tasks As List(Of TodoItem)

    ''' <summary>Constructor</summary>
    Public Sub New(filename As String)
        _filename = filename
        _storage = New XmlStorage
        _tasks = _storage.ReadXml(filename)
    End Sub
    ''' <summary>Inefficient search for a Task by ID</summary>
    Public Function GetTask(id As Integer) As TodoItem
        For t As Integer = 0 To _tasks.Count - 1
            If _tasks(t).ID = id Then
                Return _tasks(t)
            End If
        Next
        Return New TodoItem() With {.ID = id}
    End Function
    ''' <summary>List all the Tasks</summary>
    Public Function GetTasks() As IEnumerable(Of TodoItem)
        Return _tasks
    End Function
    ''' <summary>Save a Task to the Xml file
    ''' Calculates the ID as the max of existing IDs</summary>
    Public Function SaveTask(item As TodoItem) As Integer
        Dim max As Integer = 0
        If _tasks.Count > 0 Then
            max = _tasks.Max(Function(t As TodoItem) t.ID)
        End If
        If item.ID = 0 Then
            item.ID = ++max
            _tasks.Add(item)
        Else
            Dim j = _tasks.Where(Function(t) t.ID = item.ID).First()
            j = item
        End If
        _storage.WriteXml(_tasks, _filename)
        Return max
    End Function
    ''' <summary>Removes the task from the XMl file</summary>
    Public Function DeleteTask(id As Integer) As Integer
        For t As Integer = 0 To _tasks.Count - 1
            If _tasks(t).ID = id Then
                _tasks.RemoveAt(t)
                _storage.WriteXml(_tasks, _filename)
                Return 1
            End If
        Next
        Return -1
    End Function
End Class
```

> [!NOTE]
> Dieser Code ist ein Beispiel für einen sehr einfachen Daten Speicherungs Mechanismus.
> Er wird bereitgestellt, um zu veranschaulichen, wie eine .NET Standard Bibliothek eine Schnittstelle für den Zugriff auf plattformspezifische Funktionen (in diesem Fall das Laden und Speichern einer XML-Datei) codieren kann. Es sollte nicht als Daten Bank Alternative für die Produktionsqualität dienen.

## <a name="android-and-ios-application-projects"></a>Android-und IOS-Anwendungsprojekte

### <a name="ios"></a>iOS

In der IOS-Anwendung werden die `TodoItemManager` und die `XmlStorageImplementation` in der Datei **AppDelegate.cs** erstellt, wie im folgenden Code Ausschnitt gezeigt. In den ersten vier Zeilen wird lediglich der Pfad zu der Datei erstellt, in der die Daten gespeichert werden. die letzten zwei Zeilen zeigen die beiden Klassen, die instanziiert werden.

```csharp
var xmlFilename = "TodoList.xml";
string documentsPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); // Documents folder
string libraryPath = Path.Combine(documentsPath, "..", "Library"); // Library folder
var path = Path.Combine(libraryPath, xmlFilename);

TaskMgr = new TodoItemManager(path);
```

### <a name="android"></a>Android

In der Android-Anwendung werden die `TodoItemManager` und die `XmlStorageImplementation` in der Datei **Application.cs** erstellt, wie im folgenden Code Ausschnitt gezeigt. In den ersten drei Zeilen wird lediglich der Pfad zu der Datei erstellt, in der die Daten gespeichert werden. die letzten zwei Zeilen zeigen die beiden Klassen, die instanziiert werden.

```csharp
var xmlFilename = "TodoList.xml";
string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal);
var path = Path.Combine(libraryPath, xmlFilename);

TaskMgr = new TodoItemManager(path);
```

Der Rest des Anwendungs Codes betrifft hauptsächlich die Benutzeroberfläche und die Verwendung der `TaskMgr`-Klasse, um `TodoItem` Klassen zu laden und zu speichern.

## <a name="visual-studio-2019-for-mac"></a>Visual Studio 2019 für Mac

> [!WARNING]
> Visual Studio für Mac bietet keine Unterstützung für das Bearbeiten der Visual Basic Sprache – es sind keine Menü Elemente zum Erstellen Visual Basic Projekte oder Dateien vorhanden. Wenn Sie a **. vb** öffnen, gibt es keine Hervorhebung, AutoComplete oder IntelliSense der Sprachsyntax.

Visual Studio 2019 für Mac _kann_ Visual Studio-.NET Standard Projekte kompilieren, die unter Windows erstellt werden, sodass IOS-apps auf diese Projekte verweisen können.

Visual Studio 2017 _kann überhaupt keine_ Visual Basic-Projekte erstellen.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde gezeigt, wie Visual Basic Code in xamarin-Anwendungen mit Visual Studio und .NET Standard Bibliotheken genutzt werden. Obwohl xamarin Visual Basic nicht direkt unterstützt, ermöglicht die Kompilierung Visual Basic in eine .NET Standard Bibliothek das Einschließen von mit Visual Basic geschriebenen Code in IOS-und Android-Apps.

## <a name="related-links"></a>Verwandte Links

- [Taskyvb (.NET Standard Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyVB)
- [Neues in .NET Standard](https://docs.microsoft.com/dotnet/standard/whats-new/whats-new-in-dotnet-standard?tabs=csharp)
