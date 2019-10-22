---
title: 'Plattformübergreifende APP-Fallstudie: Tasky'
description: In diesem Dokument wird beschrieben, wie die Portable Beispielanwendung Tasky als plattformübergreifende Mobile Anwendung entworfen und erstellt wurde. Es werden die Anforderungen, die Schnittstelle, das Datenmodell, die Kernfunktionen, die Implementierung und vieles mehr der APP erläutert.
ms.prod: xamarin
ms.assetid: B581B2D0-9890-C383-C654-0B0E12DAD5A6
author: conceptdev
ms.author: crdun
ms.date: 03/23/2017
ms.openlocfilehash: 246ee002404fdf6fe1120c19701aceb3c2dee7db
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "71249786"
---
# <a name="cross-platform-app-case-study-tasky"></a>Plattformübergreifende APP-Fallstudie: Tasky

*Tasky* *Portable* ist eine einfache Aufgabenlisten Anwendung. In diesem Dokument wird erläutert, wie es entworfen und erstellt wurde. Befolgen Sie hierzu die Anweisungen im Dokument erstellen von Platt [Form übergreifenden Anwendungen](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) . In der Erörterung werden die folgenden Bereiche behandelt:

<a name="Design_Process" />

## <a name="design-process"></a>Entwurfsprozess

Es empfiehlt sich, eine von Road Map für das zu erstellen, was Sie tun möchten, bevor Sie mit dem Programmieren beginnen. Dies gilt insbesondere für die plattformübergreifende Entwicklung, bei der Sie Funktionen entwickeln, die auf verschiedene Weise verfügbar gemacht werden. Eine klare Vorstellung davon, was Sie aufbauen, spart Zeit und Aufwand später im Entwicklungs-und Zeitaufwand.

 <a name="Requirements" />

### <a name="requirements"></a>Anforderungen

Der erste Schritt beim Entwerfen einer Anwendung ist die Identifizierung gewünschter Features. Hierbei kann es sich um allgemeine Ziele oder um detaillierte Anwendungsfälle handeln. Tasky hat unkomplizierte funktionale Anforderungen:

- Anzeigen einer Liste von Aufgaben
- Aufgaben hinzufügen, bearbeiten und löschen
- Legen Sie den Status einer Aufgabe auf "Done" fest.

Sie sollten die Verwendung von plattformspezifischen Features in Erwägung gezogen.  Können die Vorteile der IOS-Geofencing oder Windows phone Live-Kacheln durch Tasky genutzt werden? Auch wenn Sie keine plattformspezifischen Features in der ersten Version verwenden, sollten Sie voraus planen, um sicherzustellen, dass Ihre Geschäfts & Datenschichten Sie aufnehmen können.

 <a name="User_Interface_Design" />

### <a name="user-interface-design"></a>Benutzeroberflächendesign

Beginnen Sie mit einem Design auf hoher Ebene, das auf den Zielplattformen implementiert werden kann. Achten Sie darauf, plattformspezifische Einschränkungen für die Benutzeroberfläche zu beachten. Beispielsweise kann ein `TabBarController` in ios mehr als fünf Schaltflächen anzeigen, während die Windows Phone äquivalente bis zu vier Schaltflächen anzeigen kann.
Zeichnen Sie den bildschirmfluss mit dem Tool Ihrer Wahl (Paper Works).

 [![](case-study-tasky-images/taskydesign.png "Draw the screen-flow using the tool of your choice paper works")](case-study-tasky-images/taskydesign.png#lightbox)

 <a name="Data_Model" />

### <a name="data-model"></a>Datenmodell

Wenn Sie wissen, welche Daten gespeichert werden müssen, können Sie bestimmen, welcher Persistenzmechanismus verwendet werden soll. Informationen zu den verfügbaren Speicher Mechanismen finden Sie unter [plattformübergreifender Datenzugriff](~/cross-platform/app-fundamentals/index.md) und Unterstützung bei der Entscheidung zwischen Ihnen. Für dieses Projekt verwenden wir sqlite.net.

Tasky muss drei Eigenschaften für jedes "TaskItem" speichern:

- **Name** – Zeichenfolge
- **Hinweise** – Zeichenfolge
- **Done** – boolescher Wert

 <a name="Core_Functionality" />

### <a name="core-functionality"></a>Kernfunktionalität

Beachten Sie die API, die die Benutzeroberfläche zum erfüllen der Anforderungen benötigt. Eine Aufgabenliste erfordert die folgenden Funktionen:

- **Auflisten aller Aufgaben** – zum Anzeigen der Hauptbildschirm Liste aller verfügbaren Tasks
- **Eine Aufgabe erhalten** – wenn eine Aufgaben Zeile berührt wird
- **Eine Aufgabe speichern** – bei Bearbeitung einer Aufgabe
- **Löschen einer Aufgabe** – wenn ein Task gelöscht wird
- **Leere Aufgabe erstellen** – wenn eine neue Aufgabe erstellt wird

Um die Wiederverwendung von Code zu erreichen, sollte diese API einmal in der *portablen Klassenbibliothek*implementiert werden.

 <a name="Implementation" />

### <a name="implementation"></a>Implementierung

Nachdem der Anwendungs Entwurf zugestimmt wurde, sollten Sie die Implementierung als plattformübergreifende Anwendung in Erwägung gezogen. Dies wird zur Architektur der Anwendung. Gemäß der Anleitung im Dokument zum Entwickeln von Platt [Form übergreifenden Anwendungen](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) muss der Anwendungscode in folgende Teile aufgeteilt werden:

- Allgemeiner **Code** – ein gängiges Projekt, das wiederverwendbaren Code zum Speichern der Aufgaben Daten enthält. machen Sie eine Modell Klasse und eine API verfügbar, um das Speichern und Laden von Daten zu verwalten.
- **Plattformspezifischer Code** – plattformspezifische Projekte, die eine native Benutzeroberfläche für jedes Betriebssystem implementieren, wobei der gemeinsame Code als "Back-End" genutzt wird.

[![](case-study-tasky-images/taskypro-architecture.png "Platform-specific projects implement a native UI for each operating system, utilizing the common code as the back end")](case-study-tasky-images/taskypro-architecture.png#lightbox)

Diese beiden Komponenten werden in den folgenden Abschnitten beschrieben.

 <a name="Common_(PCL)_Code" />

## <a name="common-pcl-code"></a>Allgemeiner Code (PCL)

Tasky Portable verwendet die Portable Klassen Bibliotheks Strategie für die gemeinsame Nutzung von gemeinsamem Code. Im Dokument [Freigabe Code Optionen](~/cross-platform/app-fundamentals/code-sharing.md) finden Sie eine Beschreibung der Optionen für die Code Freigabe.

Der gesamte allgemeine Code, einschließlich der Datenzugriffs Ebene, Datenbankcode und Verträge, wird im Bibliotheksprojekt abgelegt.

Das komplette PCL-Projekt ist unten dargestellt. Der gesamte Code in der portablen Bibliothek ist mit jeder Zielplattform kompatibel. Bei der Bereitstellung verweist jede Native App auf diese Bibliothek.

![](case-study-tasky-images/portable-project.png "When deployed, each native app will reference that library")

Das Klassendiagramm unten zeigt die Klassen, die nach Ebene gruppiert sind. Bei der `SQLiteConnection`-Klasse handelt es sich um Code Bausteine aus dem SQLite-net-Paket. Die übrigen Klassen stellen benutzerdefinierten Code für Tasky dar. Die Klassen `TaskItemManager` und `TaskItem` stellen die API dar, die für die plattformspezifischen Anwendungen verfügbar gemacht wird.

 [![](case-study-tasky-images/classdiagram-core.png "The TaskItemManager and TaskItem classes represent the API that is exposed to the platform-specific applications")](case-study-tasky-images/classdiagram-core.png#lightbox)

Die Verwendung von Namespaces zum Trennen der Ebenen erleichtert die Verwaltung von Verweisen zwischen den einzelnen Ebenen. Die plattformspezifischen Projekte sollten nur eine `using`-Anweisung für die Geschäfts Schicht enthalten. Die Datenzugriffs Schicht und die Datenschicht sollten von der API gekapselt werden, die von `TaskItemManager` in der Geschäfts Schicht verfügbar gemacht wird.

 <a name="References" />

### <a name="references"></a>Verweise

Portable Klassenbibliotheken müssen auf mehreren Plattformen verwendet werden können, die jeweils über unterschiedliche Ebenen der Unterstützung für Plattform-und Frameworkfunktionen verfügen. Aus diesem Grund gibt es Einschränkungen hinsichtlich der Verwendung von Paketen und Framework-Bibliotheken. Xamarin. IOS unterstützt z. b. das c#-`dynamic` Schlüsselwort nicht, daher kann eine portable Klassenbibliothek kein Paket verwenden, das von dynamischem Code abhängt, auch wenn dieser Code auf Android funktioniert. Visual Studio für Mac verhindert, dass Sie inkompatible Pakete und Verweise hinzufügen, aber Sie sollten die Einschränkungen beachten, um später unerwartete Überraschungen zu vermeiden.

Hinweis: Sie sehen, dass Ihre Projekte auf Framework-Bibliotheken verweisen, die Sie nicht verwendet haben. Diese Verweise sind als Teil der xamarin-Projektvorlagen enthalten. Wenn apps kompiliert werden, entfernt der Verknüpfungs Prozess nicht referenzierten Code, obwohl auf `System.Xml` verwiesen wird, dass er nicht in der endgültigen Anwendung enthalten ist, da keine XML-Funktionen verwendet werden.

 <a name="Data_Layer_(DL)" />

### <a name="data-layer-dl"></a>Datenschicht (DL)

Die Datenschicht enthält den Code, der die physische Speicherung von Daten durchführt – ob eine Datenbank, Flatfiles oder ein anderer Mechanismus. Die Tasky-Datenschicht besteht aus zwei Teilen: der SQLite-NET-Bibliothek und dem benutzerdefinierten Code, der hinzugefügt wird.

Tasky stützt sich auf das SQLite-net-nuget-Paket (veröffentlicht von Frank Kreuger) zum Einbetten von SQLite-NET-Code, der eine ORM-Datenbankschnittstelle (Object-Relational Mapping) bereitstellt. Die `TaskItemDatabase`-Klasse erbt von `SQLiteConnection` und fügt die erforderlichen Create-, Read-, Update-, DELETE-und DELETE (CRUD)-Methoden zum Lesen und Schreiben von Daten in SQLite hinzu. Dabei handelt es sich um eine einfache Implementierung von generischen CRUD-Methoden, die in anderen Projekten wieder verwendet werden können.

Der `TaskItemDatabase` ist ein Singleton, mit dem sichergestellt wird, dass der gesamte Zugriff auf dieselbe Instanz erfolgt. Eine Sperre wird verwendet, um den gleichzeitigen Zugriff von mehreren Threads zu verhindern.

 <a name="SQLite_on_WIndows_Phone" />

#### <a name="sqlite-on-windows-phone"></a>SQLite auf Windows Phone

Während IOS und Android beide als Teil des Betriebssystems mit SQLite ausgeliefert werden, enthält Windows Phone keine kompatible Datenbank-Engine. Um Code für alle drei Plattformen freizugeben, ist eine Windows Phone-native Version von SQLite erforderlich. Weitere Informationen zum Einrichten des Windows Phone Projekts für SQLite finden Sie [unter Arbeiten mit einer lokalen Datenbank](~/xamarin-forms/data-cloud/data/databases.md) .

 <a name="Using_an_Interface_to_Generalize_Data_Access" />

#### <a name="using-an-interface-to-generalize-data-access"></a>Verwenden einer Schnittstelle zum generalisieren des Datenzugriffs

Die Datenschicht hat eine Abhängigkeit von `BL.Contracts.IBusinessIdentity`, sodass Sie abstrakte Datenzugriffs Methoden implementieren kann, die einen Primärschlüssel erfordern. Alle Geschäfts Schicht Klassen, die die-Schnittstelle implementieren, können dann in der Datenschicht persistent gespeichert werden.

Die-Schnittstelle gibt nur eine ganzzahlige Eigenschaft an, die als Primärschlüssel fungiert:

```csharp
public interface IBusinessEntity {
    int ID { get; set; }
}
```

Die-Basisklasse implementiert die-Schnittstelle und fügt die SQLite-NET-Attribute hinzu, um Sie als automatischen inkrementellen Primärschlüssel zu kennzeichnen. Jede Klasse in der Geschäfts Schicht, die diese Basisklasse implementiert, kann dann in der Datenschicht persistent gespeichert werden:

```csharp
public abstract class BusinessEntityBase : IBusinessEntity {
    public BusinessEntityBase () {}
    [PrimaryKey, AutoIncrement]
    public int ID { get; set; }
}
```

Ein Beispiel für die generischen Methoden in der Datenebene, die die-Schnittstelle verwenden, ist diese `GetItem<T>` Methode:

```csharp
public T GetItem<T> (int id) where T : BL.Contracts.IBusinessEntity, new ()
{
    lock (locker) {
        return Table<T>().FirstOrDefault(x => x.ID == id);
    }
}
```

 <a name="Locking_to_prevent_Concurrent_Access" />

#### <a name="locking-to-prevent-concurrent-access"></a>Sperren, um gleichzeitigen Zugriff zu verhindern

Eine [Sperre](https://msdn.microsoft.com/library/c5kehkcz(v=vs.100).aspx) wird in der `TaskItemDatabase`-Klasse implementiert, um den gleichzeitigen Zugriff auf die Datenbank zu verhindern. Dadurch wird sichergestellt, dass der gleichzeitige Zugriff von verschiedenen Threads serialisiert wird (Andernfalls versucht eine Benutzeroberflächen Komponente möglicherweise, die Datenbank zu lesen, wenn Sie von einem Hintergrund Thread aktualisiert wird). Ein Beispiel für die Implementierung der Sperre finden Sie hier:

```csharp
static object locker = new object ();
public IEnumerable<T> GetItems<T> () where T : BL.Contracts.IBusinessEntity, new ()
{
    lock (locker) {
        return (from i in Table<T> () select i).ToList ();
    }
}
public T GetItem<T> (int id) where T : BL.Contracts.IBusinessEntity, new ()
{
    lock (locker) {
        return Table<T>().FirstOrDefault(x => x.ID == id);
    }
}
```

Der größte Teil des Datenschicht Codes kann in anderen Projekten wieder verwendet werden. Der einzige anwendungsspezifische Code in der Ebene ist der `CreateTable<TaskItem>`-Aufrufe im `TaskItemDatabase`-Konstruktor.

 <a name="Data_Access_Layer_(DAL)" />

### <a name="data-access-layer-dal"></a>Datenzugriffs Ebene (DAL)

Die `TaskItemRepository`-Klasse kapselt den Datenspeichermechanismus mit einer stark typisierten API, die das Erstellen, löschen, abrufen und Aktualisieren von `TaskItem` Objekten ermöglicht.

 <a name="Using_Conditional_Compilation" />

#### <a name="using-conditional-compilation"></a>Verwenden der bedingten Kompilierung

Die Klasse verwendet die bedingte Kompilierung, um den Speicherort der Datei festzulegen. Dies ist ein Beispiel für das Implementieren von Platt Form Abweichungen Die Eigenschaft, die den Pfad zurückgibt, wird auf jeder Plattform in anderen Code kompiliert. Die Code-und plattformspezifischen Compilerdirektiven werden hier angezeigt:

```csharp
public static string DatabaseFilePath {
    get {
        var sqliteFilename = "TaskDB.db3";
#if SILVERLIGHT
        // Windows Phone expects a local path, not absolute
        var path = sqliteFilename;
#else
#if __ANDROID__
        // Just use whatever directory SpecialFolder.Personal returns
        string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); ;
#else
        // we need to put in /Library/ on iOS5.1+ to meet Apple's iCloud terms
        // (they don't want non-user-generated data in Documents)
        string documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal); // Documents folder
        string libraryPath = Path.Combine (documentsPath, "..", "Library"); // Library folder
#endif
        var path = Path.Combine (libraryPath, sqliteFilename);
                #endif
                return path;
    }
}
```

Abhängig von der Plattform lautet die Ausgabe "<app
path>/Library/taskdb.db3" für IOS, "<app
path>/Documents/taskdb.db3" für Android oder "taskdb. db3" für Windows phone.

### <a name="business-layer-bl"></a>Geschäfts Schicht (BL)

Die Geschäfts Schicht implementiert die Modellklassen und eine Fassade, um Sie zu verwalten.
In Tasky ist das Modell die `TaskItem` Klasse und `TaskItemManager` das Fassaden Muster implementiert, um eine API zum Verwalten von `TaskItems` bereitzustellen.

 <a name="Façade" />

#### <a name="faade"></a>Hoff

 `TaskItemManager` umschließt die `DAL.TaskItemRepository`, um die Get-, Save-und Delete-Methoden bereitzustellen, auf die von der Anwendungs-und UI-Schicht verwiesen wird.

Geschäftsregeln und Logik werden hier bei Bedarf eingefügt – beispielsweise alle Validierungsregeln, die erfüllt sein müssen, bevor ein Objekt gespeichert wird.

 <a name="API_for_Platform-Specific_Code" />

### <a name="api-for-platform-specific-code"></a>API für plattformspezifischen Code

Nachdem der gemeinsame Code geschrieben wurde, muss die Benutzeroberfläche zum Erfassen und Anzeigen der von ihr bereitgestellten Daten erstellt werden. Die `TaskItemManager`-Klasse implementiert das Fassaden Muster, um eine einfache API für den Anwendungscode bereitzustellen.

Der in den einzelnen plattformspezifischen Projekten geschriebene Code ist in der Regel eng mit dem systemeigenen SDK dieses Geräts verknüpft, und der Zugriff auf den gemeinsamen Code erfolgt nur über die vom `TaskItemManager` definierte API. Dies schließt die von ihm verfügbar gemachten Methoden und Geschäftsklassen ein, z. b. `TaskItem`.

Bilder werden nicht plattformübergreifend freigegeben, sondern den einzelnen Projekten unabhängig hinzugefügt. Dies ist wichtig, da jede Plattform Bilder unterschiedlich behandelt und dabei unterschiedliche Dateinamen, Verzeichnisse und Auflösungen verwendet.

In den restlichen Abschnitten werden die plattformspezifischen Implementierungsdetails der Tasky-Benutzeroberfläche erörtert.

 <a name="iOS_App" />

## <a name="ios-app"></a>IOS-App

Zum Implementieren der IOS-Tasky-Anwendung mit dem allgemeinen PCL-Projekt zum Speichern und Abrufen von Daten sind nur wenige Klassen erforderlich. Das vollständige IOS xamarin. IOS-Projekt ist unten dargestellt:

 ![](case-study-tasky-images/taskyios-solution.png "iOS project is shown here")

Die Klassen werden in diesem Diagramm dargestellt und in Ebenen gruppiert.

 [![](case-study-tasky-images/classdiagram-android.png "The classes are shown in this diagram, grouped into layers")](case-study-tasky-images/classdiagram-android.png#lightbox)

 <a name="References" />

### <a name="references"></a>Verweise

Die IOS-App verweist auf die plattformspezifischen SDK-Bibliotheken – z. b. Xamarin. IOS und MonoTouch. Dialog-1.

Es muss auch auf das `TaskyPortableLibrary` PCL-Projekt verweisen.
Die Verweis Liste wird hier angezeigt:

 ![](case-study-tasky-images/taskyios-references.png "The references list is shown here")

Die Anwendungsschicht und die Benutzeroberflächen Ebene werden in diesem Projekt mithilfe dieser Verweise implementiert.

 <a name="Application_Layer_(AL)" />

### <a name="application-layer-al"></a>Anwendungsschicht (Al)

Die Anwendungsschicht enthält plattformspezifische Klassen, die erforderlich sind, um die Objekte, die von der PCL verfügbar gemacht werden, an die Benutzeroberfläche zu binden. Die IOS-spezifische Anwendung verfügt über zwei Klassen, die Ihnen helfen sollen, Aufgaben anzuzeigen:

- **Editingsource** – diese Klasse wird verwendet, um Listen von Aufgaben an die Benutzeroberfläche zu binden. Da `MonoTouch.Dialog` für die Aufgabenliste verwendet wurde, muss dieses Hilfsprogramm implementiert werden, um die Funktion zum Löschen im `UITableView` zu ermöglichen. Swipe-to-Delete ist in ios üblich, aber nicht in Android oder Windows Phone, sodass das IOS-spezifische Projekt das einzige ist, das es implementiert.
- **Taskdialog** – diese Klasse wird verwendet, um eine einzelne Aufgabe an die Benutzeroberfläche zu binden. Er verwendet die `MonoTouch.Dialog` reflektionsapi, um das `TaskItem`-Objekt mit einer Klasse zu "wrappen", die die richtigen Attribute enthält, damit der Eingabe Bildschirm ordnungsgemäß formatiert werden kann.

Die `TaskDialog`-Klasse verwendet `MonoTouch.Dialog` Attribute, um einen Bildschirm auf der Grundlage der Eigenschaften einer Klasse zu erstellen. Die-Klasse sieht wie folgt aus:

```csharp
public class TaskDialog {
    public TaskDialog (TaskItem task)
    {
        Name = task.Name;
        Notes = task.Notes;
        Done = task.Done;
    }
    [Entry("task name")]
    public string Name { get; set; }
    [Entry("other task info")]
    public string Notes { get; set; }
    [Entry("Done")]
    public bool Done { get; set; }
    [Section ("")]
    [OnTap ("SaveTask")]    // method in HomeScreen
    [Alignment (UITextAlignment.Center)]
    public string Save;
    [Section ("")]
    [OnTap ("DeleteTask")]  // method in HomeScreen
    [Alignment (UITextAlignment.Center)]
    public string Delete;
}
```

Beachten Sie, dass für die `OnTap` Attribute ein Methodenname erforderlich ist – diese Methoden müssen in der Klasse vorhanden sein, in der die `MonoTouch.Dialog.BindingContext` erstellt wird (in diesem Fall die im nächsten Abschnitt beschriebene `HomeScreen` Klasse).

 <a name="User_Interface_Layer_(UI)" />

### <a name="user-interface-layer-ui"></a>Benutzeroberflächen Ebene (UI)

Die Benutzeroberflächen Ebene besteht aus den folgenden Klassen:

1. **Appdelegat** – enthält Aufrufe der Darstellungs-API, um die in der Anwendung verwendeten Schriftarten und Farben zu formatieren. Tasky ist eine einfache Anwendung, sodass keine anderen Initialisierungs Tasks in `FinishedLaunching` ausgeführt werden.
2. **Bildschirme** – Unterklassen von `UIViewController`, die die einzelnen Bildschirme und deren Verhalten definieren. Bildschirme verbinden die Benutzeroberfläche mit den Klassen der Anwendungsschicht und der allgemeinen API (`TaskItemManager`). In diesem Beispiel werden die Bildschirme im Code erstellt, aber Sie können mit der Interface Builder von Xcode oder dem Storyboard-Designer entworfen worden sein.
3. **Bilder** – visuelle Elemente sind ein wichtiger Bestandteil jeder Anwendung. Tasky verfügt über Begrüßungsbildschirm-und Symbolbilder, die für IOS in regulärer und Retina-Auflösung bereitgestellt werden müssen.

 <a name="Home_Screen" />

#### <a name="home-screen"></a>Startbildschirm

Der Startbildschirm ist ein `MonoTouch.Dialog` Bildschirm, auf dem eine Liste der Aufgaben aus der SQLite-Datenbank angezeigt wird. Sie erbt von `DialogViewController` und implementiert Code, um die `Root` so festzulegen, dass Sie eine Auflistung von `TaskItem` Objekten für die Anzeige enthält.

 [![](case-study-tasky-images/ios-taskylist.png "It inherits from DialogViewController and implements code to set the Root to contain a collection of TaskItem objects for display")](case-study-tasky-images/ios-taskylist.png#lightbox)

Die zwei Hauptmethoden im Zusammenhang mit der Anzeige und Interaktion mit der Aufgabenliste sind:

1. **Populatetable** – verwendet die `TaskManager.GetTasks` Methode der Geschäfts Schicht, um eine Auflistung von `TaskItem` Objekten abzurufen, die angezeigt werden sollen.
2. **Ausgewählt** – wenn eine Zeile berührt wird, wird die Aufgabe in einem neuen Bildschirm angezeigt.

 <a name="Task_Details_Screen" />

#### <a name="task-details-screen"></a>Task Details-Bildschirm

Task Details ist ein Eingabe Bildschirm, mit dem Aufgaben bearbeitet oder gelöscht werden können.

Tasky verwendet `MonoTouch.Dialog` Reflection-API, um den Bildschirm anzuzeigen, sodass keine `UIViewController` Implementierung vorhanden ist. Stattdessen instanziiert und zeigt die `HomeScreen`-Klasse eine `DialogViewController` mithilfe der `TaskDialog`-Klasse aus der Anwendungsschicht an.

Dieser Screenshot zeigt einen leeren Bildschirm, der die `Entry` Attribute veranschaulicht, die den Wasserzeichen Text in den Feldern " **Name** " und " **Notizen** " festlegen:

 [![](case-study-tasky-images/ios-taskydetail.png "This screenshot shows an empty screen that demonstrates the Entry attribute setting the watermark text in the Name and Notes fields")](case-study-tasky-images/ios-taskydetail.png#lightbox)

Die Funktionalität des Bildschirms " **Aufgaben Details** " (z. b. das Speichern oder Löschen einer Aufgabe) muss in der `HomeScreen` Klasse implementiert werden, da hier die `MonoTouch.Dialog.BindingContext` erstellt wird. Die folgenden `HomeScreen` Methoden unterstützen den Bildschirm "Aufgaben Details":

1. **Showtaskdetails** – erstellt eine `MonoTouch.Dialog.BindingContext` zum Rendering eines Bildschirms. Der Eingabe Bildschirm wird mithilfe von Reflektion erstellt, um Eigenschaftsnamen und-Typen aus der `TaskDialog` Klasse abzurufen. Zusätzliche Informationen, wie z. b. der Wasserzeichen Text für die Eingabefelder, werden mit Attributen der Eigenschaften implementiert.
2. **Savetask** – auf diese Methode wird in der `TaskDialog`-Klasse über ein `OnTap`-Attribut verwiesen. Sie wird aufgerufen, wenn **Save** gedrückt wird, und verwendet eine `MonoTouch.Dialog.BindingContext`, um die vom Benutzer eingegebenen Daten abzurufen, bevor die Änderungen mithilfe `TaskItemManager` gespeichert werden.
3. **DeleteTask** – auf diese Methode wird in der `TaskDialog`-Klasse über ein `OnTap`-Attribut verwiesen. Es verwendet `TaskItemManager`, um die Daten mit dem Primärschlüssel (ID-Eigenschaft) zu löschen.

 <a name="Android_App" />

## <a name="android-app"></a>Android-App

Das vollständige xamarin. Android-Projekt wird unten dargestellt:

 ![](case-study-tasky-images/taskyandroid-solution.png "Android project is pictured here")

Das Klassendiagramm mit Klassen, die nach Ebene gruppiert sind:

 [![](case-study-tasky-images/classdiagram-android.png "The class diagram, with classes grouped by layer")](case-study-tasky-images/classdiagram-android.png#lightbox)

 <a name="References" />

### <a name="references"></a>Verweise

Das Android-App-Projekt muss auf die plattformspezifische xamarin. Android-Assembly verweisen, um auf Klassen aus dem Android SDK zuzugreifen.

Er muss auch auf das PCL-Projekt verweisen (z. b. Taskyportablelibrary) für den Zugriff auf den allgemeinen Code der Daten-und Geschäfts Schicht.

 ![](case-study-tasky-images/taskyandroid-references.png "TaskyPortableLibrary to access the common data and business layer code")

 <a name="Application_Layer_(AL)" />

### <a name="application-layer-al"></a>Anwendungsschicht (Al)

Ähnlich wie bei der bereits erwähnten IOS-Version enthält die Anwendungsschicht in der Android-Version plattformspezifische Klassen, die erforderlich sind, um die Objekte zu binden, die vom Kern an die Benutzeroberfläche verfügbar gemacht werden.

 **Tasklistadapter** – um eine Liste \<T > von Objekten anzuzeigen, müssen wir einen Adapter implementieren, um benutzerdefinierte Objekte in einem `ListView` anzuzeigen. Der Adapter steuert, welches Layout für jedes Element in der Liste verwendet wird – in diesem Fall verwendet der Code ein integriertes Android-Layout `SimpleListItemChecked`.

 <a name="User_Interface_(UI)" />

### <a name="user-interface-ui"></a>Benutzeroberfläche (UI)

Die Benutzeroberflächen Ebene der Android-App ist eine Kombination aus Code und XML-Markup.

- **Ressourcen/Layout** – Bildschirmlayouts und der Zeilen Zell Entwurf, der als axml-Dateien implementiert ist. Der axml-Code kann von Hand geschrieben oder visuell mithilfe des xamarin UI-Designers für Android angelegt werden.
- **Ressourcen/drawable** – Bilder (Symbole) und benutzerdefinierte Schaltfläche.
- **Bildschirme** – Aktivitäts Unterklassen, die die einzelnen Bildschirme und deren Verhalten definieren. Verbindet die Benutzeroberfläche mit den Klassen der Anwendungsschicht und der allgemeinen API (`TaskItemManager`).

 <a name="Home_Screen" />

#### <a name="home-screen"></a>Startbildschirm

Der Startbildschirm besteht aus einer Aktivitäts Unterklasse `HomeScreen` und der `HomeScreen.axml` Datei, die das Layout definiert (Position der Schaltfläche und Aufgabenliste). Der Bildschirm sieht wie folgt aus:

 [![](case-study-tasky-images/android-taskylist.png "The screen looks like this")](case-study-tasky-images/android-taskylist.png#lightbox)

Der Startbildschirm Code definiert die Handler für das Klicken auf die Schaltfläche und das Klicken auf Elemente in der Liste sowie das Auffüllen der Liste in der `OnResume`-Methode (damit die im Bildschirm "Aufgaben Details" vorgenommenen Änderungen widergespiegelt werden). Daten werden mithilfe der `TaskItemManager` der Geschäfts Schicht und der `TaskListAdapter` von der Anwendungsschicht geladen.

 <a name="Task_Details_Screen" />

#### <a name="task-details-screen"></a>Task Details-Bildschirm

Der Bildschirm Task Details besteht auch aus einer `Activity`-Unterklasse und einer axml-Layoutdatei. Das Layout bestimmt die Position der Eingabe Steuerelemente, und C# die Klasse definiert das Verhalten, das `TaskItem` Objekten geladen und gespeichert werden soll.

 [![](case-study-tasky-images/android-taskydetail.png "The class defines the behavior to load and save TaskItem objects")](case-study-tasky-images/android-taskydetail.png#lightbox)

Alle Verweise auf die PCL-Bibliothek befinden sich über die `TaskItemManager`-Klasse.

 <a name="Windows_Phone_App" />

## <a name="windows-phone-app"></a>Windows Phone-App
Das komplette Windows Phone Projekt:

 ![](case-study-tasky-images/taskywp7-solution.png "Windows Phone App The complete Windows Phone project")

Das folgende Diagramm zeigt die Klassen, die in Ebenen gruppiert sind:

 [![](case-study-tasky-images/classdiagram-wp7.png "This diagram presents the classes grouped into layers")](case-study-tasky-images/classdiagram-wp7.png#lightbox)

 <a name="References" />

### <a name="references"></a>Verweise

Das plattformspezifische Projekt muss auf die erforderlichen plattformspezifischen Bibliotheken (z. b. `Microsoft.Phone` und `System.Windows`) verweisen, um eine gültige Windows Phone Anwendung zu erstellen.

Er muss auch auf das PCL-Projekt verweisen (z. b. `TaskyPortableLibrary`), um die `TaskItem`-Klasse und die-Datenbank zu verwenden.

 ![](case-study-tasky-images/taskywp7-references.png "TaskyPortableLibrary to utilize the TaskItem class and database")

 <a name="Application_Layer_(AL)" />

### <a name="application-layer-al"></a>Anwendungsschicht (Al)

Ebenso wie bei den IOS-und Android-Versionen besteht die Anwendungsschicht aus den nicht visuellen Elementen, die das Binden von Daten an die Benutzeroberfläche erleichtern.

 <a name="ViewModels" />

#### <a name="viewmodels"></a>ViewModels

ViewModels wrappen Daten aus der PCL (`TaskItemManager`) und zeigt Sie so an, dass Sie von der Silverlight/XAML-Datenbindung verwendet werden kann. Dies ist ein Beispiel für ein plattformspezifisches Verhalten (wie im Dokument für plattformübergreifende Anwendungen erläutert).

 <a name="User_Interface_(UI)" />

### <a name="user-interface-ui"></a>Benutzeroberfläche (UI)

XAML verfügt über eine eindeutige Daten Bindungs Funktion, die im Markup deklariert werden kann und die erforderliche Menge an Code zum Anzeigen von Objekten reduziert:

1. **Pages** – XAML-Dateien und Ihr Code Behind definieren die Benutzeroberfläche und verweisen auf ViewModels und das PCL-Projekt, um Daten anzuzeigen und zu erfassen.
2. **Bilder** – Begrüßungsbildschirm, Hintergrund-und Symbolbilder sind ein wichtiger Bestandteil der Benutzeroberfläche.

 <a name="MainPage" />

#### <a name="mainpage"></a>MainPage

Die MainPage-Klasse verwendet die `TaskListViewModel`, um Daten mithilfe der Daten Bindungsfunktionen von XAML anzuzeigen. Die `DataContext` der Seite wird auf das Ansichts Modell festgelegt, das asynchron aufgefüllt wird. Die `{Binding}`-Syntax im XAML-Code bestimmt, wie die Daten angezeigt werden.

 <a name="TaskDetailsPage" />

#### <a name="taskdetailspage"></a>Taskdetailspage

Jede Aufgabe wird angezeigt, indem die `TaskViewModel` an das in der Datei "taskdetailspage. XAML" definierte XAML-Code gebunden wird. Die Aufgaben Daten werden über die `TaskItemManager` in der Geschäfts Schicht abgerufen.

 <a name="Results" />

## <a name="results"></a>Ergebnisse

Die resultierenden Anwendungen sehen auf jeder Plattform wie folgt aus:

 <a name="iOS" />

### <a name="ios"></a>iOS

Die Anwendung verwendet das Design der IOS-Standard-Benutzeroberfläche, z. b. die Schaltfläche "hinzufügen", die auf der Navigationsleiste positioniert ist, und das integrierte **Pluszeichen (+)** . Außerdem wird das standardmäßige `UINavigationController` Verhalten der Schaltfläche "zurück" verwendet, und in der Tabelle wird "Swipe-to-Delete" unterstützt.

 [![](case-study-tasky-images/ios-taskylist.png "Außerdem wird das Standardverhalten der UINavigationController zurück-Schaltfläche verwendet und die Verwendung von Swipe in der Tabelle unterstützt.")](case-study-tasky-images/ios-taskylist.png#lightbox)[![](case-study-tasky-images/ios-taskylist.png "Außerdem wird das Standardverhalten der UINavigationController zurück-Schaltfläche verwendet und die Verwendung von Swipe in der Tabelle unterstützt.")](case-study-tasky-images/ios-taskylist.png#lightbox)

 <a name="Android" />

### <a name="android"></a>Android

Die Android-App verwendet integrierte Steuerelemente, einschließlich des integrierten Layouts für Zeilen, für die ein "Tick" angezeigt wird. Das Hardware-/systembackverhalten wird zusätzlich zu einer auf dem Bildschirm zurück basierenden Schaltfläche unterstützt.

 [![](case-study-tasky-images/android-taskylist.png "The hardware/system back behavior is supported in addition to an on-screen back button")](case-study-tasky-images/android-taskylist.png#lightbox)[![](case-study-tasky-images/android-taskylist.png "The hardware/system back behavior is supported in addition to an on-screen back button")](case-study-tasky-images/android-taskylist.png#lightbox)

 <a name="Windows_Phone" />

### <a name="windows-phone"></a>Windows Phone

Die Windows Phone-App verwendet das Standardlayout und füllt die APP-Leiste am unteren Bildschirmrand anstelle einer Navigationsleiste im oberen Bereich auf.

 [![](case-study-tasky-images/wp-taskylist.png "Die Windows Phone-App verwendet das Standardlayout und füllt die APP-Leiste am unteren Bildschirmrand anstelle einer Navigationsleiste am oberen Rand.")](case-study-tasky-images/wp-taskylist.png#lightbox)[![](case-study-tasky-images/wp-taskylist.png "Die Windows Phone-App verwendet das Standardlayout und füllt die APP-Leiste am unteren Bildschirmrand anstelle einer Navigationsleiste am oberen Rand.")](case-study-tasky-images/wp-taskylist.png#lightbox)

 <a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Dokument wurde ausführlich erläutert, wie die Prinzipien des geschichteten Anwendungs Entwurfs auf eine einfache Anwendung angewendet wurden, um die Wiederverwendung von Code auf drei mobilen Plattformen zu vereinfachen: IOS, Android und Windows phone.

Er hat den Prozess zum Entwerfen der Anwendungsschichten beschrieben und erläutert, welche Code &amp; Funktionalität in jeder Schicht implementiert wurde.

Der Code kann von [GitHub](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)heruntergeladen werden.

## <a name="related-links"></a>Verwandte Links

- [Entwickeln plattformübergreifender Anwendungen (Hauptdokument)](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [Tasky Portable-Beispiel-app (GitHub)](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)
