---
title: Visual Basic.NET in Xamarin.IOS und Android
description: Diese exemplarische Vorgehensweise veranschaulicht, wie Sie native apps für Xamarin.iOS und Xamarin.Android zu erstellen, die in Visual Basic.NET geschriebene Geschäftslogik zu verwenden.
ms.prod: xamarin
ms.assetid: 455fda67-3879-4299-8036-b12840e6a498
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: affebab9bb6b07f204beef24cce2b57444d45e49
ms.sourcegitcommit: 7eed80186e23e6aff3ddbbf7ce5cd1fa20af1365
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2018
ms.locfileid: "51527299"
---
# <a name="visual-basicnet-in-xamarin-ios-and-android"></a>Visual Basic.NET in Xamarin.IOS und Android

Die [TaskyPortable](/samples/mobile/VisualBasic/TaskyPortableVB/) beispielanwendung für veranschaulicht, wie Visual Basic-Code in eine Portable Klassenbibliothek kompiliert mit Xamarin verwendet werden kann. Hier sind einige Screenshots der resultierenden unter iOS, Android und Windows Phone-apps:

 [![](native-apps-images/image5.png "IOS-, Android- und Windows phones Ausführen von Apps mit Visual Basic")](native-apps-images/image5.png#lightbox)

IOS, Android und Windows Phone-Projekte im Beispiel werden alle in C#. Die Benutzeroberfläche für jede Anwendung verfügt über native Technologien (Storyboards, Xml und XAML-bzw.), während die `TodoItem` Management wird von der portablen Klassenbibliothek mit Visual Basic bereitgestellt mithilfe einer `IXmlStorage` -Implementierung von das systemeigene Projekt.

## <a name="sample-walkthrough"></a>Exemplarische Vorgehensweise

Dieser Anleitung wird erläutert, wie in Visual Basic implementiert wurde die [TaskyPortableVB](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyPortableVB) Xamarin-Beispiel für iOS und Android.

> [!NOTE]
> Überprüfen Sie die Anweisungen auf [Visual Basic.NET PCLs](/guides/cross-platform/application_fundamentals/pcl/portable_visual_basic_net/) bevor Sie mit dieser Anleitung fortfahren.

## <a name="visualbasicportablelibrary"></a>VisualBasicPortableLibrary

Portable Klassenbibliotheken in Visual Basic kann nur in Visual Studio erstellt werden.
Die Beispielbibliothek enthält die Grundlagen der Anwendung in vier Visual Basic-Dateien:

-  IXmlStorage.vb
-  TodoItem.vb
-  TodoItemManager.vb
-  TodoItemRepositoryXML.vb


### <a name="ixmlstoragevb"></a>IXmlStorage.vb

Da Datei Zugriff Verhalten sich so deutlich zwischen Plattformen variieren, Portable Class Libraries bieten keine `System.IO` file Storage-APIs in einem Profil. Dies bedeutet, dass wenn Sie direkt mit dem Dateisystem in unserem portablen Code interagieren möchten, wir unsere systemeigene Projekte auf jeder Plattform Rückruf müssen.  Durch das Schreiben von unseren Visual Basic-Code für eine einfache Schnittstelle, die in implementiert werden kann C# auf jeder Plattform haben wir gemeinsam nutzbaren Visual Basic-Code, der immer Zugriff auf das Dateisystem noch.

Der Beispielcode verwendet diese sehr einfache Schnittstelle, die nur zwei Methoden enthält: Lesen und schreiben eine serialisierte XML-Datei.

```vb
Public Interface IXmlStorage
    Function ReadXml(filename As String) As List(Of TodoItem)
    Sub WriteXml(tasks As List(Of TodoItem), filename As String)
End Interface
```

iOS, Android und Windows Phone-Implementierungen für diese Schnittstelle werden weiter unten in der Anleitung angezeigt werden.

### <a name="todoitemvb"></a>TodoItem.vb

Diese Klasse enthält das Geschäftsobjekt, das in der gesamten Anwendung verwendet werden. Es wird in Visual Basic definiert werden, und freigegebene mit dem iOS, Android und Windows Phone-Projekte, die geschrieben werden, in C#.

Die Definition der Klasse ist hier dargestellt:

```vb
Public Class TodoItem
    Property ID() As Integer
    Property Name() As String
    Property Notes() As String
    Property Done() As Boolean
End Class
```

Das Beispiel verwendet XML-Serialisierung und Deserialisierung zu laden und speichern Sie die TodoItem-Objekte.

### <a name="todoitemmanagervb"></a>TodoItemManager.vb

Die Manager-Klasse stellt die API für portablen Code. Es bietet grundlegende CRUD-Vorgänge für die `TodoItem` -Klasse, aber keine Implementierung dieser Vorgänge.

```vb
Public Class TodoItemManager
    Private _repository As TodoItemRepositoryXML
    Public Sub New(filename As String, storage As IXmlStorage)
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

Der Konstruktor akzeptiert eine Instanz von IXmlStorage als Parameter an. Dadurch wird jeder Plattform eine eigene Implementierung arbeiten und ermöglichen es immer noch die übertragbaren Code andere Funktionen zu beschreiben, die gemeinsam genutzt werden können gleichzeitig bereitstellen.

### <a name="todoitemrepositoryvb"></a>TodoItemRepository.vb

Die "Repository"-Klasse enthält die Logik zum Verwalten der Liste der TodoItem-Objekte. Der vollständige Code wird unten – die Logik, die in erster Linie um einen eindeutigen ID-Wert für die TodoItems zu verwalten, wie diese hinzugefügt und aus der Auflistung entfernt vorhanden ist.

```vb
Public Class TodoItemRepositoryXML
    Private _filename As String
    Private _storage As IXmlStorage
    Private _tasks As List(Of TodoItem)

    ''' <summary>Constructor</summary>
    Public Sub New(filename As String, storage As IXmlStorage)
        _filename = filename
        _storage = storage
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
> Dieser Code ist ein Beispiel für einen sehr einfachen Datenspeicher-Mechanismus.
> Er wird bereitgestellt, um zu veranschaulichen, wie eine Portable Klassenbibliothek für eine Schnittstelle für den Zugriff auf plattformspezifische Funktionen (in diesem Fall laden und speichern eine XML-Datei) programmieren können. Es sollte sie keine Alternative mit Produktionsqualität Datenbank sein.

## <a name="ios-android-and-windows-phone-application-projects"></a>iOS, Android und Windows Phone-Anwendungsprojekten

Dieser Abschnitt enthält die plattformspezifischen Implementierungen für die Schnittstelle IXmlStorage und zeigt, wie es in jeder Anwendung verwendet wird. Die Anwendungsprojekte sind in geschrieben C#.

### <a name="ios-and-android-ixmlstorage"></a>iOS und Android IXmlStorage

Xamarin.iOS und Xamarin.Android bieten vollständige `System.IO` Funktionalität, sodass Sie problemlos laden und speichern Sie die XML-Datei mithilfe der folgenden Klasse können:

```csharp
public class XmlStorageImplementation : IXmlStorage
{
    public XmlStorageImplementation(){}
    public List<TodoItem> ReadXml(string filename)
    {
        if (File.Exists(filename))
        {
            var serializer = new XmlSerializer(typeof(List<TodoItem>));
            using (var stream = new FileStream(filename, FileMode.Open))
            {
                return (List<TodoItem>)serializer.Deserialize(stream);
            }
        }
        return new List<TodoItem>();
    }
    public void WriteXml(List<TodoItem> tasks, string filename)
    {
        var serializer = new XmlSerializer(typeof(List<TodoItem>));
        using (var writer = new StreamWriter(filename))
        {
            serializer.Serialize(writer, tasks);
        }
    }
}
```

In der iOS-Anwendung die `TodoItemManager` und `XmlStorageImplementation` werden erstellt, der **Datei "appdelegate.cs"** Datei wie im folgenden Codeausschnitt gezeigt. Die ersten vier Zeilen sind nur den Pfad zu der Datei erstellen, in dem Daten gespeichert werden; die letzten beiden Zeilen zeigen die beiden Klassen instanziiert wird.

```csharp
var xmlFilename = "TodoList.xml";
string documentsPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); // Documents folder
string libraryPath = Path.Combine(documentsPath, "..", "Library"); // Library folder
var path = Path.Combine(libraryPath, xmlFilename);
var xmlStorage = new XmlStorageImplementation();
TaskMgr = new TodoItemManager(path, xmlStorage);
```

In der Android-Anwendung die `TodoItemManager` und `XmlStorageImplementation` werden erstellt, der **Application.cs** Datei wie im folgenden Codeausschnitt gezeigt. Die ersten drei Zeilen werden nur den Pfad zu der Datei erstellen, in dem Daten gespeichert werden; die letzten beiden Zeilen zeigen die beiden Klassen instanziiert wird.

```csharp
var xmlFilename = "TodoList.xml";
string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal);
var path = Path.Combine(libraryPath, xmlFilename);
var xmlStorage = new AndroidTodo.XmlStorageImplementation();
TaskMgr = new TodoItemManager(path, xmlStorage);
```

Der Rest des Anwendungscodes ist in erster Linie mit der Benutzeroberfläche befassen und Verwenden der `TaskMgr` -Klasse zum Laden und speichern Sie `TodoItem` Klassen.

### <a name="windows-phone-ixmlstorage"></a>Windows Phone IXmlStorage

Windows Phone bietet keine vollständigen Zugriff auf das Dateisystem des Geräts, stattdessen der IsolatedStorage-API verfügbar zu machen. Die `IXmlStorage` Implementierung für Windows Phone sieht wie folgt aus:

```csharp
public class XmlStorageImplementation : IXmlStorage
{
    public XmlStorageImplementation(){}
    public List<TodoItem> ReadXml(string filename)
    {
        IsolatedStorageFile fileStorage = IsolatedStorageFile.GetUserStoreForApplication();
        if (fileStorage.FileExists(filename))
        {
            var serializer = new XmlSerializer(typeof(List<TodoItem>));
            using (var stream = new StreamReader(new IsolatedStorageFileStream(filename, FileMode.Open, fileStorage)))
            {
                return (List<TodoItem>)serializer.Deserialize(stream);
            }
        }
        return new List<TodoItem>();
    }
    public void WriteXml(List<TodoItem> tasks, string filename)
    {
        IsolatedStorageFile fileStorage = IsolatedStorageFile.GetUserStoreForApplication();
        var serializer = new XmlSerializer(typeof(List<TodoItem>));
        using (var writer = new StreamWriter(new IsolatedStorageFileStream(filename, FileMode.OpenOrCreate, fileStorage)))
        {
            serializer.Serialize(writer, tasks);
        }
    }
}
```

Die `TodoItemManager` und `XmlStorageImplementation` werden erstellt, der **"App.Xaml.cs"** Datei wie im folgenden Codeausschnitt gezeigt.

```csharp
var filename = "TodoList.xml";
var xmlStorage = new XmlStorageImplementation();
TodoMgr = new TodoItemManager(filename, xmlStorage);
```

Der Rest der Windows Phone-Anwendung besteht aus Xaml und C# die Benutzeroberfläche zu erstellen und Verwenden der `TodoMgr` -Klasse zum Laden und speichern Sie `TodoItem` Objekte.

## <a name="visual-basic-pcl-in-visual-studio-for-mac"></a>Visual Basic Plc in Visual Studio für Mac

Visual Studio für Mac unterstützt nicht die Visual Basic-Sprache – Sie können nicht erstellt oder Kompilieren von Visual Basic-Projekten mit Visual Studio für Mac.

Visual Studio für Mac – Unterstützung für Portable Klassenbibliotheken bedeutet, dass es PCL-Assemblys verweisen kann, die von Visual Basic erstellt wurden.

In diesem Abschnitt wird erläutert, wie zum Kompilieren einer PCL-Assembly in Visual Studio, und stellen Sie sicher, dass es in einem Versionskontrollsystem gespeichert und von anderen Projekten verwiesen wird.

### <a name="keeping-the-pcl-output-from-visual-studio"></a>Halten die PCL-Ausgabe in Visual Studio

In der Standardeinstellung die meisten Versionskontrollsysteme (einschließlich TFS und Git) werden so konfiguriert, ignoriert der **/bin /** Verzeichnis, das bedeutet, die kompilierte Assembly der PCL dass werden nicht gespeichert werden. Dies bedeutet, dass Sie sie manuell auf Computer mit Visual Studio für Mac, um einen Verweis darauf hinzuzufügen kopieren müssen.

Um sicherzustellen, dass Ihr Versionskontrollsystem die PCL-Assemblyausgabe speichern kann, können Sie ein Skript nach der Erstellung erstellen, die sie in das Stammverzeichnis des Projekts kopiert. Dieser Schritt nach der Erstellung trägt dazu bei die Assembly kann ganz einfach zur quellcodeverwaltung hinzugefügt und für andere Projekte freigegeben.

#### <a name="visual-studio-2017"></a>Visual Studio 2017

1. Mit der rechten Maustaste auf das Projekt, und wählen Sie die **Eigenschaften > Buildereignisse** Abschnitt.

2. Hinzufügen einer _Postbuild_ -Skript, das die Ausgabe-DLL aus dem Projekt in das Stammverzeichnis des Projekts kopiert (die befindet sich außerhalb der **/bin /**). Je nach Konfiguration der Zugriffssteuerung für Ihre Version sollte die DLL nun zur quellcodeverwaltung hinzugefügt werden können.

  [![](native-apps-images/image6-vs-sml.png "Buildereignisse Post-Build-Skript zum Kopieren von VB-DLL")](native-apps-images/image6-vs.png#lightbox)

#### <a name="visual-studio-2015"></a>Visual Studio 2015

1.  Mit der rechten Maustaste auf das Projekt, und wählen Sie **Eigenschaften > Kompilieren** , stellen Sie sicher, alle Konfigurationen in der oberen linken Kamm-Feld ausgewählt ist. Klicken Sie auf die **Buildereignisse...**  -Schaltfläche in der unteren rechten Ecke.

    [![](native-apps-images/image6.png "Kompilieren Sie die Projekteigenschaften im Abschnitt")](native-apps-images/image6.png#lightbox)

1.  Hinzufügen eines Postbuild-Skripts, die die Ausgabe-DLL aus dem Projekt in das Stammverzeichnis des Projekts kopiert (Dies ist außerhalb des **/bin /** ). Je nach Konfiguration der Zugriffssteuerung für Ihre Version sollte die DLL nun zur quellcodeverwaltung hinzugefügt werden können.

    [![](native-apps-images/image7.png "Erstellen von Ereignisfenster")](native-apps-images/image7.png#lightbox)

#### <a name="all-versions"></a>Alle Versionen

Beim nächsten Erstellen Sie das Projekt, die Portable Class Library-Assembly kopiert werden, um das Stammverzeichnis des Projekts, und wann Sie Check-in/Commits/pushes Ihre Änderungen die DLL können gespeichert wird (sodass sie auf einem Mac mit Visual Studio für Mac heruntergeladen werden kann).

  [![](native-apps-images/image8-sml.png "Speicherort der Ausgabeassembly Visual Basic")](native-apps-images/image8.png#lightbox)


Diese Assembly kann dann Xamarin-Projekte in Visual Studio für Mac, hinzugefügt werden, obwohl die Visual Basic-Sprache in Xamarin.IOS oder Android-Projekte nicht unterstützt wird.

### <a name="referencing-the-pcl-in-visual-studio-for-mac"></a>Verweisen auf die PCL in Visual Studio für Mac

Da Xamarin nicht mit Visual Basic unterstützt kann nicht es dem PCL-Projekt (noch mit der Windows Phone-app) geladen, wie im folgenden Screenshot gezeigt:

 [![](native-apps-images/image9.png "Visual Studio für Mac-Lösung")](native-apps-images/image9.png#lightbox)

Wir können die Visual Basic-PCL-Assembly-DLL immer noch in die Xamarin.iOS und Xamarin.Android-Projekte einschließen:

1.  Mit der rechten Maustaste auf die **Verweise** Knoten, und wählen **Verweise bearbeiten...**

    [![](native-apps-images/image10.png "Projekt-Menü \"Verweise bearbeiten\"")](native-apps-images/image10.png#lightbox)

1.  Wählen Sie die **.Net Assembly** Registerkarte aus, und navigieren Sie zu die Ausgabe-DLL im Verzeichnis Visual Basic-Projekts. Obwohl Visual Studio für Mac das Projekt nicht öffnen können, sollten alle Dateien aus der quellcodeverwaltung vorhanden sein. Klicken Sie auf **hinzufügen** dann **OK** dieser Assembly für die IOS- und Android-Apps hinzufügen.

    [![](native-apps-images/image11-sml.png "Klicken Sie auf Hinzufügen und dann auf OK, um diese Assembly für die IOS- und Android-Anwendungen hinzuzufügen.")](native-apps-images/image11.png#lightbox)

1.  Die IOS- und Android-Anwendungen können jetzt die Anwendungslogik, die von der portablen Klassenbibliothek mit Visual Basic bereitgestellten enthalten. Dieser Screenshot zeigt eine iOS-Anwendung, die verweist auf die Visual Basic-PCL und enthält Code, der die Funktionen aus dieser Bibliothek verwendet.

    [![](native-apps-images/image12-sml.png "Bearbeiten Sie Verweise hinzufügen .NET Assembly-Fenster")](native-apps-images/image12.png#lightbox)


Wenn Änderungen vorgenommen werden, auf Visual Basic-Projekt in Visual Studio Denken Sie daran, das Projekt erstellen, speichern die resultierende Assembly-DLL, in der quellcodeverwaltung, und klicken Sie dann abrufen, neue DLL aus der quellcodeverwaltung auf Ihrem Mac, damit Visual Studio für Mac erstellt, enthält die neueste Version die Funktionalität.


## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde veranschaulicht, wie Visual Basic-Code in Xamarin-Anwendungen mit Visual Studio und Portable Class Libraries genutzt. Obwohl Xamarin Visual Basic nicht direkt unterstützt, ermöglicht das Kompilieren von Visual Basic in einer PCL Code mit Visual Basic, um in iOS und Android-apps enthalten sein.

## <a name="related-links"></a>Verwandte Links

- [TaskyPortableVB (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyPortableVB)
- [Plattformübergreifende Entwicklung mit .NET Framework (Microsoft)](http://msdn.microsoft.com/library/gg597391(v=vs.110).aspx)
