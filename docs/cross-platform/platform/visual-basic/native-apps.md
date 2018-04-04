---
title: Visual Basic.NET in Xamarin iOS und Android
description: Xin Exemplarische Vorgehensweise veranschaulicht, wie systemeigene Xamarin.iOS und Xamarin.Android-apps zu erstellen, die in Visual Basic geschriebene Geschäftslogik zu nutzen.
ms.prod: xamarin
ms.assetid: 455fda67-3879-4299-8036-b12840e6a498
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: c922102d20eb103eb265d8a7bcb418e5187296a5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="visual-basicnet-in-xamarin-ios-and-android"></a>Visual Basic.NET in Xamarin iOS und Android

Die [TaskyPortable](/samples/mobile/VisualBasic/TaskyPortableVB/) beispielanwendung für veranschaulicht, wie Visual Basic-Code in eine Portable Klassenbibliothek kompiliert mit Xamarin verwendet werden kann. Hier sind einige Screenshots der resultierenden apps unter iOS, Android und Windows Phone:

 [![](native-apps-images/image5.png "iOS, Android und Windows-Telefone Ausführen von Apps mit Visual Basic")](native-apps-images/image5.png#lightbox)

IOS, Android und Windows Phone-Projekte im Beispiel in c# geschrieben sind. Die Benutzeroberfläche für jede Anwendung mit systemeigenen Technologien integriert ist (Storyboards, Xml und Verwendung von XAML-bzw.), während die `TodoItem` Verwaltung wird bereitgestellt, von der portablen Klassenbibliothek in Visual Basic mithilfe einer `IXmlStorage` Implementierung, die das systemeigene Projekt.

## <a name="sample-walkthrough"></a>Exemplarische Vorgehensweise

Dieses Handbuch erläutert, wie in Visual Basic implementiert wurde die [TaskyPortableVB](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyPortableVB) Xamarin-Beispiel für iOS und Android.

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

Da so deutlich Datei Zugriff Verhalten zwischen den Plattformen unterschiedlich sind, portablen Klassenbibliotheken bieten keine `System.IO` Dateispeicher APIs in jedem beliebigen Profil. Dies bedeutet, dass wenn wir uns direkt interagieren mit dem Dateisystem in unserer portablen Code möchten, wir unsere systemeigene Projekte auf jeder Plattform Rückruf müssen.  Durch das Schreiben von unseren Visual Basic-Code für eine einfache Oberfläche, die in c# auf jeder Plattform implementiert werden kann, können wir freigegeben Visual Basic-Code haben, weiterhin Zugriff auf das Dateisystem verfügt.

Im Beispielcode wird diese sehr einfache Schnittstelle, die nur über zwei Methoden enthält: zum Lesen und Schreiben einer serialisierten XML-Datei.

```vb
Public Interface IXmlStorage
    Function ReadXml(filename As String) As List(Of TodoItem)
    Sub WriteXml(tasks As List(Of TodoItem), filename As String)
End Interface
```

iOS, Android und Windows Phone-Implementierungen für diese Schnittstelle werden weiter unten in der Anleitung angezeigt werden.

### <a name="todoitemvb"></a>TodoItem.vb

Diese Klasse enthält das Geschäftsobjekt, das in der Anwendung verwendet werden soll. Es wird in Visual Basic definiert werden und gemeinsam mit dem iOS, Android und Windows Phone-Projekte, die in c# geschrieben sind.

Die Definition der Klasse wird hier gezeigt:

```vb
Public Class TodoItem
    Property ID() As Integer
    Property Name() As String
    Property Notes() As String
    Property Done() As Boolean
End Class
```

Das Beispiel verwendet die XML-Serialisierung und Deserialisierung zu laden und speichern Sie die TodoItem-Objekte.

### <a name="todoitemmanagervb"></a>TodoItemManager.vb

Die Manager-Klasse stellt die API für portablen Code. Es bietet grundlegende CRUD-Vorgänge für die `TodoItem` Klasse, aber keine Implementierung dieser Vorgänge.

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

Der Konstruktor akzeptiert eine Instanz von IXmlStorage als Parameter an. Dadurch wird jeder Plattform eine eigene Implementierung arbeiten und weiterhin ermöglichen die portablen Code andere Funktionen zu beschreiben, die gemeinsam genutzt werden können gleichzeitig bereitstellen.

### <a name="todoitemrepositoryvb"></a>TodoItemRepository.vb

Die Repository-Klasse enthält die Logik zum Verwalten der Liste der TodoItem-Objekte. Der vollständige Code wird unten – die Logik, die hauptsächlich, dass um einen eindeutige ID-Wert über die TodoItems zu verwalten, hinzugefügt und entfernt aus der Auflistung vorhanden ist.

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
> Dieser Code ist ein Beispiel für ein sehr einfaches Daten-Speichermechanismus.
> Er wird bereitgestellt, um zu veranschaulichen, wie eine Portable Klassenbibliothek für eine Schnittstelle den Zugriff auf plattformspezifische Funktionen (in diesem Fall laden und speichern eine XML-Datei) code können. Sie sollte es keine Alternative professionell Datenbank sein.

## <a name="ios-android-and-windows-phone-application-projects"></a>iOS, Android und Windows Phone-Anwendungsprojekten

Dieser Abschnitt enthält die Clientplattform-spezifische Implementierungen für die IXmlStorage-Schnittstelle und zeigt, wie sie in jeder Anwendung verwendet wird. Die Anwendungsprojekte werden alle in c# geschrieben.

### <a name="ios-and-android-ixmlstorage"></a>IOS- und Android IXmlStorage

Xamarin.iOS und Xamarin.Android bieten vollständige `System.IO` Funktionalität, sodass Sie problemlos laden und speichern Sie die XML-Datei mithilfe der folgenden Klasse:

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

In der iOS-Anwendung die `TodoItemManager` und `XmlStorageImplementation` werden erstellt, der **AppDelegate.cs** Datei wie in diesem Codeausschnitt gezeigt. Die ersten vier Zeilen sind nur den Pfad zur Datei erstellen, wo Daten gespeichert werden sollen; die letzten beiden Zeilen zeigen die beiden Klassen instanziiert wird.

```csharp
var xmlFilename = "TodoList.xml";
string documentsPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); // Documents folder
string libraryPath = Path.Combine(documentsPath, "..", "Library"); // Library folder
var path = Path.Combine(libraryPath, xmlFilename);
var xmlStorage = new XmlStorageImplementation();
TaskMgr = new TodoItemManager(path, xmlStorage);
```

In der Android-Anwendung die `TodoItemManager` und `XmlStorageImplementation` werden erstellt, der **Application.cs** Datei wie in diesem Codeausschnitt gezeigt. Die ersten drei Zeilen sind nur den Pfad zur Datei erstellen, wo Daten gespeichert werden sollen; die letzten beiden Zeilen zeigen die beiden Klassen instanziiert wird.

```csharp
var xmlFilename = "TodoList.xml";
string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal);
var path = Path.Combine(libraryPath, xmlFilename);
var xmlStorage = new AndroidTodo.XmlStorageImplementation();
TaskMgr = new TodoItemManager(path, xmlStorage);
```

Der Rest des Anwendungscodes ist in erster Linie mit der Benutzeroberfläche und mit der `TaskMgr` Klasse zum Laden und speichern `TodoItem` Klassen.

### <a name="windows-phone-ixmlstorage"></a>Windows Phone IXmlStorage

Windows Phone bietet keine vollständige Zugang zum Dateisystem des Geräts, stattdessen die IsolatedStorage-API bereitstellen. Die `IXmlStorage` für Windows Phone-Implementierung sieht wie folgt aus:

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

Die `TodoItemManager` und `XmlStorageImplementation` werden erstellt, der **App.xaml.cs** Datei wie in diesem Codeausschnitt gezeigt.

```csharp
var filename = "TodoList.xml";
var xmlStorage = new XmlStorageImplementation();
TodoMgr = new TodoItemManager(filename, xmlStorage);
```

Der Rest der Windows Phone-Anwendung besteht aus Xaml und c# zum Erstellen der Benutzeroberfläche und Verwenden der `TodoMgr` Klasse zum Laden und speichern `TodoItem` Objekte.

## <a name="visual-basic-pcl-in-visual-studio-for-mac"></a>Visual Basic PCL in Visual Studio für Mac

Visual Studio für Mac unterstützt nicht die Sprache Visual Basic – Sie können nicht erstellt oder Kompilieren der Visual Basic-Projekte mit Visual Studio für Mac.

Visual Studio für Mac Unterstützung für Portable Klassenbibliotheken bedeutet, dass es PCL-Assemblys verweisen kann, die von Visual Basic erstellt wurden.

In diesem Abschnitt erläutert, wie eine PCL-Assembly in Visual Studio zu kompilieren, und stellen Sie sicher, dass es in ein Versionskontrollsystem gespeichert und auf andere Projekte verweist.

### <a name="keeping-the-pcl-output-from-visual-studio"></a>Halten die PCL-Ausgabe von Visual Studio

Standardmäßig werden die meisten Versionskontrollsysteme (einschließlich TFS- und Git) zum ignorieren konfiguriert die **/bin/** Verzeichnis, d. die kompilierte PCL-Assembly h. wird nicht gespeichert werden. Dies bedeutet, dass Sie müssten manuell auf dem Computer mit Visual Studio für Mac einen Verweis darauf hinzufügen zu kopieren.

Um sicherzustellen, dass die Ausgabe der PCL-Assembly mit das Versionskontrollsystem gespeichert werden kann, können Sie ein Skript nach der Erstellung erstellen, die sie in den Projektstamm kopiert. Dieser Schritt nach der Erstellung kann sichergestellt werden, die Assembly einfach zur quellcodeverwaltung hinzugefügt und mit anderen Projekten gemeinsam genutzt.

#### <a name="visual-studio-2017"></a>Visual Studio 2017

1. Mit der rechten Maustaste auf das Projekt, und wählen Sie die **Eigenschaften > Buildereignisse** Abschnitt.

2. Hinzufügen einer _Postbuild_ Skript, das die Ausgabe-DLL in diesem Projekt in das Projektstammverzeichnis kopiert (außerhalb der **/bin/**). Abhängig von Ihrer Version Control-Konfiguration sollte die DLL jetzt Datenquellen-Steuerelement hinzugefügt werden können.

  [![](native-apps-images/image6-vs-sml.png "Buildereignisse Post Buildskript zum Kopieren von VB-DLL")](native-apps-images/image6-vs.png#lightbox)

#### <a name="visual-studio-2015"></a>Visual Studio 2015

1.  Mit der rechten Maustaste auf das Projekt, und wählen Sie **Eigenschaften > Kompilieren** , vergewissern Sie sich dann mit dem alle Konfigurationen in der oberen linken Kamm ausgewählt ist. Klicken Sie auf die **Buildereignisse...**  Schaltfläche in der unteren rechten Ecke.

    [![](native-apps-images/image6.png "Kompilieren Sie die Projekteigenschaften im Abschnitt")](native-apps-images/image6.png#lightbox)

1.  Fügen Sie ein Postbuild-Skript, das die Ausgabe-DLL in diesem Projekt in das Stammverzeichnis des Projekts kopiert (Dies ist außerhalb des **/bin/** ). Abhängig von Ihrer Version Control-Konfiguration sollte die DLL jetzt Datenquellen-Steuerelement hinzugefügt werden können.

    [![](native-apps-images/image7.png "Fenster Protokollereignisse erstellen")](native-apps-images/image7.png#lightbox)

#### <a name="all-versions"></a>Alle Versionen

Beim nächsten Erstellen Sie das Projekt, das Portable Class Library-Assembly kopiert werden, in das Projektstammverzeichnis, und wenn Sie Check-in/Commit/Push sind die Änderungen der DLL gespeichert wird (sodass sie auf einem Mac mit Visual Studio für Mac heruntergeladen werden kann).

  [![](native-apps-images/image8-sml.png "Speicherort der Ausgabeassembly Visual Basic")](native-apps-images/image8.png#lightbox)


Diese Assembly kann dann Xamarin-Projekte in Visual Studio für Mac, hinzugefügt werden, obwohl die Sprache Visual Basic selbst nicht in Xamarin iOS oder Android-Projekte unterstützt wird.

### <a name="referencing-the-pcl-in-visual-studio-for-mac"></a>Verweisen auf die PCL, in Visual Studio für Mac

Da Xamarin Visual Basic nicht unterstützt kann nicht es PCL-Projekt (noch die Windows Phone-app) geladen, wie in diesem Screenshot gezeigt:

 [![](native-apps-images/image9.png "Visual Studio für Mac-Lösung")](native-apps-images/image9.png#lightbox)

Wir können die Visual Basic-PCL-Assembly-DLL immer noch in den Projekten Xamarin.iOS und Xamarin.Android einschließen:

1.  Mit der rechten Maustaste auf die **Verweise** Knoten, und wählen **Verweise bearbeiten...**

    [![](native-apps-images/image10.png "Projektmenü für Verweise bearbeiten")](native-apps-images/image10.png#lightbox)

1.  Wählen Sie die **.Net Assembly** Registerkarte, und navigieren Sie zu der Ausgabe-DLL in das Verzeichnis der Visual Basic-Projekt. Obwohl Visual Studio für Mac das Projekt nicht öffnen können, sollten alle Dateien aus der quellcodeverwaltung vorhanden sein. Klicken Sie auf **hinzufügen** dann **OK** Hinzufügen dieser Assembly auf IOS- und Android-Anwendungen.

    [![](native-apps-images/image11-sml.png "Klicken Sie auf Hinzufügen und dann auf OK, um diese Assembly IOS- und Android-Anwendungen hinzuzufügen")](native-apps-images/image11.png#lightbox)

1.  IOS- und Android-Anwendungen enthalten können nun die Anwendungslogik, die von der portablen Klassenbibliothek in Visual Basic bereitgestellt. Diese bildschirmabbildung zeigt eine iOS-Anwendung, die verweist auf die Visual Basic-PCL und Code, der die Funktionalität aus dieser Bibliothek verwendet wurde.

    [![](native-apps-images/image12-sml.png "Bearbeiten Sie Verweise hinzufügen .NET Assembly-Fenster")](native-apps-images/image12.png#lightbox)


Wenn Änderungen vorgenommen werden, auf die Visual Basic-Projekt in Visual Studio Denken Sie daran, das Projekt erstellen, speichern die resultierende DLL-Assembly in der quellcodeverwaltung, und ziehen Sie dann diese neue DLL aus der quellcodeverwaltung auf Ihrem Mac, damit Visual Studio für Mac erstellt enthalten Sie die neuesten die Funktionalität.


## <a name="summary"></a>Zusammenfassung

Dieser Artikel wurde beschrieben, wie Visual Basic-Code in die Xamarin-Anwendungen, die mithilfe von Visual Studio und portablen Klassenbibliotheken genutzt. Obwohl Xamarin Visual Basic nicht direkt unterstützt wird, ermöglicht das Kompilieren von Visual Basic in einer PCL Code mit Visual Basic in iOS und Android-apps aufgenommen werden.

## <a name="related-links"></a>Verwandte Links

- [TaskyPortableVB (sample)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyPortableVB)
- [Plattformübergreifende Entwicklung mit .NET Framework (Microsoft)](http://msdn.microsoft.com/en-us/library/gg597391(v=vs.110).aspx)
