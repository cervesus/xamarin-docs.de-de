---
title: 'Plattformübergreifenden App Case Study: Tasky'
description: Dieses Dokument beschreibt, wie die Tasky Portable-Beispiel-Anwendung entwickelt wurde, und wie eine plattformübergreifende mobile-Anwendung erstellt. Es wird erläutert, des app Anforderungen, Schnittstelle, Datenmodell, Kernfunktionalität, Implementierung und mehr.
ms.prod: xamarin
ms.assetid: B581B2D0-9890-C383-C654-0B0E12DAD5A6
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 48650445d06ad3bc7ca6d4da84c9b8837f8a0f88
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782234"
---
# <a name="cross-platform-app-case-study-tasky"></a>Plattformübergreifenden App Case Study: Tasky

*Tasky* *Portable* ist eine einfache Aufgabe List-Anwendung. Dieses Dokument erläutert, wie es entwickelt und erstellt, nach die Unterstützung von wurde die [Erstellen von plattformübergreifenden Anwendungen](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) Dokument. Die folgende Bereiche behandelt:

<a name="Design_Process" />

## <a name="design-process"></a>Entwurfsprozess

Es ist ratsam, erstellen Sie einen Wegweiser für Sie zu erzielen, bevor Sie die Codierung beginnen möchten. Dies gilt vor allem für die plattformübergreifende Entwicklung, in dem Sie Funktionalität erstellen, die auf verschiedene Weise verfügbar gemacht werden. Beginnend mit eine klare Vorstellung davon, was Sie spart Zeit und Aufwand später im Entwicklungszyklus erstellen.

 <a name="Requirements" />

### <a name="requirements"></a>Anforderungen

Der erste Schritt beim Entwerfen einer Anwendung, ist die gewünschte Funktionen zu identifizieren. Dabei kann es sich um allgemeine Ziele oder detaillierte Anwendungsfälle. Tasky ist unkomplizierte funktionale Anforderungen:

 -  Anzeigen einer Liste von Aufgaben
 -  Hinzufügen, bearbeiten und Löschen von Aufgaben
 -  Festlegen Sie Aufgabe mit dem Status auf "Fertig"

Sie sollten die Verwendung der plattformspezifischen Funktionen.  Kann Tasky Geofencing iOS oder Windows Phone-Live-Kacheln nutzen? Auch wenn Sie in der ersten Version nicht Clientplattform-spezifische Funktionen verwenden, sollten Sie im Voraus planen, um sicherzustellen, dass Ihr Unternehmen & Datenschichten aufnehmen können.

 <a name="User_Interface_Design" />

### <a name="user-interface-design"></a>Benutzeroberflächendesign

Beginnen Sie mit einer allgemeinen Entwurf, der auf den Zielplattformen implementiert werden kann. Achten Sie darauf Hinweis Plattform spezifisch UI Einschränkungen. Z. B. eine `TabBarController` in iOS anzeigen kann mehr als fünf Schaltflächen während der Windows Phone-Entsprechung bis zu vier anzeigen kann.
Zeichnen Sie den Bildschirm-Fluss, der mithilfe des Tools Ihrer Wahl (Papier funktioniert).

 [![](case-study-tasky-images/taskydesign.png "Zeichnen Sie den Bildschirm-Fluss, der mithilfe des Tools Ihrer Wahl Papier funktioniert")](case-study-tasky-images/taskydesign.png#lightbox)

 <a name="Data_Model" />

### <a name="data-model"></a>Datenmodell

Zu wissen, welche Daten gespeichert werden können Sie herausfinden, welche Dauerhaftigkeit zu verwenden. Finden Sie unter [plattformübergreifende Datenzugriff](~/cross-platform/app-fundamentals/index.md) für Informationen zu den verfügbaren Speichermechanismen und Informationen dazu, zwischen ihnen. Für dieses Projekt wird SQLite.NET verwendet werden.

Tasky muss drei Eigenschaften für jede "TaskItem" zu speichern:

 -  **Namen** – Zeichenfolge
 -  **Anmerkungen zu dieser** – Zeichenfolge
 -  **Fertig** – boolescher Wert

 <a name="Core_Functionality" />

### <a name="core-functionality"></a>Kernfunktionalität

Betrachten Sie die API, die die Benutzeroberfläche nutzen, um die Anforderungen erfüllen muss. Eine Aufgabenliste erfordert die folgenden Funktionen:

 -   **Auflisten aller Aufgaben** – richtet sich an den Hauptbildschirm Liste aller verfügbaren Tasks anzuzeigen.
 -  **Abrufen einer Aufgabe** – Wenn eine Aufgabenzeile verwendet wird
 -  **Ein Task "Speichern"** – Wenn eine Aufgabe bearbeitet wird
 -  **Löschen Sie eine Aufgabe** – Wenn eine Aufgabe gelöscht wird
 -  **Erstellen von leeren Task** – Wenn ein neuer Task erstellt wird

Um die Wiederverwendung von Code zu erreichen, diese API sollten implementiert werden einmal der *Portable Class Library*.

 <a name="Implementation" />

### <a name="implementation"></a>Implementierung

Sobald das Anwendungsdesign vereinbart wurden hat, erwägen Sie, wie sie wie eine plattformübergreifende Anwendung implementiert werden kann. Dadurch wird die Architektur der Anwendung sein. Die Anweisungen in der [Erstellen von plattformübergreifenden Anwendungen](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) Dokument den Code der Anwendung unterbrochen werden sollte nach unten in den folgenden Teilen:

 -   **Gemeinsamer Code der** – common-Projekts, das aktionsentwicklung Code zum Speichern von Daten für den Task Quellmaterial und verfügbar zu machen, eine Modellklasse und eine API zum Verwalten von speichern, und Laden von Daten.
 -   **Plattformspezifischen Code** – plattformspezifischen Projekte, die eine einheitliche Benutzeroberfläche für jedes Betriebssystem, unter Verwendung des allgemeinen Codes als "Back-End" implementieren.

 [![](case-study-tasky-images/taskypro-architecture.png "Plattformspezifischen Projekte implementieren eine einheitliche Benutzeroberfläche für jedes Betriebssystem, unter Verwendung des allgemeinen Codes als Back-end")](case-study-tasky-images/taskypro-architecture.png#lightbox)

Diese beiden Teile werden in den folgenden Abschnitten beschrieben.

 <a name="Common_(PCL)_Code" />

## <a name="common-pcl-code"></a>Gemeinsamer Code der (PCL)

Tasky Portable verwendet die Portable Class Library-Strategie für die gemeinsame Nutzung allgemeinen Code. Finden Sie unter der [Sharing Code Options](~/cross-platform/app-fundamentals/code-sharing.md) Dokument für eine Beschreibung der Codefreigabe Optionen.

Alle gemeinsamen Code ab, einschließlich der Datenzugriffsebene, Datenbankcode und Verträge, befindet sich im Klassenbibliotheksprojekt.

Das vollständige PCL-Projekt wird unten veranschaulicht. Der gesamte Code in der portablen Bibliothek ist kompatibel mit jeder Zielplattform. Bei der Bereitstellung wird jede systemeigene Anwendung diese Bibliothek verweisen.

![](case-study-tasky-images/portable-project.png "Bei der Bereitstellung wird jede systemeigene Anwendung diese Bibliothek verweisen.")

Das folgende Klassendiagramm zeigt die Klassen gruppiert nach Ebene. Die `SQLiteConnection` Klasse Code wird häufig aus dem Sqlite-NET-Paket. Die übrigen Klassen sind für Tasky benutzerdefinierten Code. Die `TaskItemManager` und `TaskItem` Klassen stellen die API, die für die Clientplattform-spezifische Anwendungen verfügbar gemacht wird.

 [![](case-study-tasky-images/classdiagram-core.png "Die TaskItemManager und TaskItem-Klassen darstellen, die API, die für die Clientplattform-spezifische Anwendungen bereitgestellt wird")](case-study-tasky-images/classdiagram-core.png#lightbox)

Verwenden von Namespaces, trennen Sie die Ebenen kann zum Verwalten von Verweisen zwischen jeder Ebene. Die plattformspezifischen Projekte müssen Sie nur zum Einschließen einer `using` -Anweisung für die Geschäftsebene. Die Datenzugriffsebene und Datenschicht von der API, die von verfügbar gemacht wird gekapselt werden `TaskItemManager` in der Geschäftsebene.

 <a name="References" />

### <a name="references"></a>Verweise

Portable Klassenbibliotheken müssen über mehrere Plattformen mit unterschiedlichen Ebenen der Unterstützung für die Plattform und Framework-Funktionen verwendet werden. Aus diesem Grund gelten Einschränkungen, auf denen Pakete und Framework-Bibliotheken verwendet werden können. Beispielsweise Xamarin.iOS unterstützt nicht die c#- `dynamic` -Schlüsselwort, daher eine portable Klassenbibliothek kann alle Pakete, die abhängig von dynamischem Code verwendet werden, obwohl diese Art von Code auf Android-Geräten funktionieren würde. Visual Studio für Mac wird verhindert, dass Sie inkompatibler Pakete und Verweise hinzufügen, sondern müssen Einschränkungen beachten, damit Sie später überraschungen vermeiden bleiben sollen.

Hinweis: Sie sehen sich, dass Ihre Projekte Framework-Bibliotheken verweisen, die Sie verwendet haben. Diese Verweise werden als Teil der Xamarin-Projektvorlagen enthalten. Wenn apps kompiliert werden, der Verknüpfungsvorgang entfernt Unreferenzierte Code, auch wenn `System.Xml` wurde auf die verwiesen wird, sie werden nicht eingeschlossen werden in der endgültigen Anwendung da keine XML-Funktionen verwendet wird.

 <a name="Data_Layer_(DL)" />

### <a name="data-layer-dl"></a>Datenschicht (DL)

Die Datenschicht enthält den Code, der die physische Speicherung von Daten – zu einer Datenbank, Flatfile-Dateien oder anderen Mechanismen ausführt. Die Datenschicht Tasky besteht aus zwei Teilen: der SQLite-Netzwerkbibliothek und den benutzerdefinierten Code hinzugefügt, die sie Einrichten von Netzwerkdaten.

Tasky basiert auf der Sqlite-Net-Nuget-Paket (veröffentlicht von Frank Kreuger) zum Einbetten von SQLite-NET-Code, der eine Datenbankschnittstelle objektrelationales Mapping (ORM) bereitstellt. Die `TaskItemDatabase` Klasse erbt von `SQLiteConnection` und fügt die erforderlichen Erstellungs-, Lese-, Update-, löschen (CRUD) Methoden zum Lesen und Schreiben von Daten in SQLite. Es ist eine einfache Textbaustein-Implementierung der generischen CRUD-Methoden, die in anderen Projekten wiederverwendet werden kann.

Die `TaskItemDatabase` ist ein Singleton, sicherstellen, dass alle Zugriffe für dieselbe Instanz auftritt. Eine Sperre wird verwendet, um zu verhindern, dass bei der gleichzeitigen Zugriff von mehreren Threads.

 <a name="SQLite_on_WIndows_Phone" />

#### <a name="sqlite-on-windows-phone"></a>SQLite auf Windows Phone

Bei IOS- und Android sowohl liefern mit SQLite als Teil des Betriebssystems, umfasst Windows Phone nicht kompatible Datenbank-Engine. Zum Freigeben von Code für alle drei Plattformen ist eine Windows Phone-systemeigenen-Version von SQLite erforderlich. Finden Sie unter [arbeiten mit einer lokalen Datenbank](~/xamarin-forms/app-fundamentals/databases.md) für Weitere Informationen zum Einrichten Ihres Windows Phone-Projekts für Sqlite festlegen.

 <a name="Using_an_Interface_to_Generalize_Data_Access" />

#### <a name="using-an-interface-to-generalize-data-access"></a>Verallgemeinern Sie Datenzugriff mithilfe einer Schnittstelle

Die Datenschicht übernimmt eine Abhängigkeit `BL.Contracts.IBusinessIdentity` , damit diese abstrakte Datenzugriffsmethoden implementieren kann, die Primärschlüssel erforderlich. Jede Geschäftsebene-Klasse, die die Schnittstelle implementiert, kann dann in der Datenschicht beibehalten werden.

Die Schnittstelle gibt nur eine Ganzzahleigenschaft, die als Primärschlüssel fungieren:

```csharp
public interface IBusinessEntity {
    int ID { get; set; }
}
```

Die Basisklasse der Klasse implementiert die Schnittstelle und fügt die SQLite-NET-Attribute, um diese als eine automatisch erhöhenden Primärschlüssel kennzeichnen. Jede Klasse in der Geschäftsschicht, die diese Basisklasse implementiert, kann dann in der Datenschicht beibehalten werden:

```csharp
public abstract class BusinessEntityBase : IBusinessEntity {
    public BusinessEntityBase () {}
    [PrimaryKey, AutoIncrement]
    public int ID { get; set; }
}
```

Ein Beispiel für den generischen Methoden in der Datenschicht, mit denen die Schnittstelle ist dies `GetItem<T>` Methode:

```csharp
public T GetItem<T> (int id) where T : BL.Contracts.IBusinessEntity, new ()
{
    lock (locker) {
        return Table<T>().FirstOrDefault(x => x.ID == id);
    }
}
```

 <a name="Locking_to_prevent_Concurrent_Access" />

#### <a name="locking-to-prevent-concurrent-access"></a>Um gleichzeitigen Zugriff zu verhindern, dass Sperren

Ein [Sperre](http://msdn.microsoft.com/library/c5kehkcz(v=vs.100).aspx) wird implementiert, innerhalb der `TaskItemDatabase` Klasse, um den gleichzeitigen Zugriff auf die Datenbank zu verhindern. Dadurch wird sichergestellt, gleichzeitigen Zugriff von anderen Threads serialisiert wird (andernfalls möglicherweise eine UI-Komponente zum Lesen der Datenbank zur gleichen Zeit, ein Hintergrundthread er aktualisiert, versuchen). Ein Beispiel für die Implementierung der Sperre wird hier gezeigt:

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

Großteil des Codes Datenschicht konnte in anderen Projekten wiederverwendet werden. Der nur anwendungsspezifischen Code in der Ebene ist die `CreateTable<TaskItem>` rufen Sie in der `TaskItemDatabase` Konstruktor.

 <a name="Data_Access_Layer_(DAL)" />

### <a name="data-access-layer-dal"></a>Die Datenzugriffsebene (DAL))

Die `TaskItemRepository` -Klasse kapselt Datenspeichermechanismus mit eine stark typisierte-API, ermöglicht `TaskItem` Objekte erstellt, gelöscht, abgerufen und aktualisiert.

 <a name="Using_Conditional_Compilation" />

#### <a name="using-conditional-compilation"></a>Bedingte Kompilierung

Die Klasse verwendet die bedingten Kompilierung, um den Dateispeicherort festzulegen: Dies ist ein Beispiel der Implementierung der Plattform Abweichungen. Die Eigenschaft, die den Pfad zurückgegeben, die in anderen Code, der auf jeder Plattform kompiliert werden. Der Code und die Plattform-spezifischen Compilerdirektiven sind hier aufgeführt:

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

Abhängig von der Plattform werden die Ausgabe "<app
path>/Library/TaskDB.db3" für iOS "<app
path>/Documents/TaskDB.db3" für Android oder einfach "TaskDB.db3" für Windows Phone.

### <a name="business-layer-bl"></a>Geschäftsebene (BL)

Die Geschäftsschicht implementiert der Modellklassen und eine Fassade für deren Verwaltung.
In Tasky das Modell ist die `TaskItem` Klasse und `TaskItemManager` implementiert das Muster Fassade zum Bereitstellen einer API zum Verwalten von `TaskItems`.

 <a name="Façade" />

#### <a name="faade"></a>Fassade

 `TaskItemManager` Dient als Wrapper für die `DAL.TaskItemRepository` um Get zu gewährleisten, speichern und Löschen von Methoden, die von der Anwendung und der UI-Ebenen verwiesen werden.

Geschäftsregeln und Logik wäre hier platziert werden, wenn erforderlich – z. B. alle Validierungsregeln, die erfüllt werden müssen, bevor ein Objekt gespeichert wird.

 <a name="API_for_Platform-Specific_Code" />

### <a name="api-for-platform-specific-code"></a>API für plattformspezifischen Code

Nachdem der allgemeine Code geschrieben wurde, muss die Benutzeroberfläche zum Sammeln und Anzeigen der Daten verfügbar gemacht werden, indem er erstellt werden. Die `TaskItemManager` Klasse implementiert das Fassade-Muster, um eine einfache API für den Code der Anwendung den Zugriff auf bereitzustellen.

In jedem Projekt plattformspezifischen geschriebenen Codes wird in der Regel eng mit dem systemeigenen-SDK von diesem Gerät verknüpft, und nur Zugriff auf der allgemeine Code mithilfe der API, die definiert, indem Sie die `TaskItemManager`. Dazu gehören die Methoden und Business Klassen es macht, z. B. `TaskItem`.

Bilder werden nicht von mehreren Plattformen gemeinsam jedoch unabhängig voneinander jedes Projekt hinzugefügt. Dies ist wichtig, daran, dass jede Plattform Bilder unterschiedlich ist, übernimmt mit unterschiedlichen Dateinamen, Verzeichnisse und Lösungen.

Die verbleibenden Abschnitte erläutern die plattformspezifischen Implementierungsdetails der Tasky Benutzeroberfläche.

 <a name="iOS_App" />

## <a name="ios-app"></a>iOS-App

Es gibt nur eine Handvoll Klassen zum Implementieren der iOS-Tasky Anwendung mithilfe der allgemeinen PCL-Projekt zum Speichern und Abrufen von Daten erforderlich sind. Das vollständige iOS Xamarin.iOS-Projekt wird unten gezeigt:

 ![](case-study-tasky-images/taskyios-solution.png "iOS-Projekt wird hier angezeigt.")

Die Klassen sind in diesem Diagramm, in Schichten gruppiert angezeigt.

 [![](case-study-tasky-images/classdiagram-android.png "Die Klassen sind in diesem Diagramm, in Schichten gruppiert angezeigt.")](case-study-tasky-images/classdiagram-android.png#lightbox)

 <a name="References" />

### <a name="references"></a>Verweise

Die iOS-app verweist die plattformspezifischen SDK-Bibliotheken – z. B. auf. Xamarin.iOS und MonoTouch.Dialog-1.

Sie müssen auch verweisen die `TaskyPortableLibrary` PCL-Projekt.
Liste der Verweise wird hier gezeigt:

 ![](case-study-tasky-images/taskyios-references.png "Liste der Verweise wird hier angezeigt.")

In diesem Projekt verwenden diese Verweise werden der Anwendungsschicht und Benutzeroberflächenebene implementiert.

 <a name="Application_Layer_(AL)" />

### <a name="application-layer-al"></a>Anwendungsebene (AL)

Die Anwendungsebene enthält Clientplattform-spezifische Klassen erforderlich, um "binden Sie die Objekte, die von der PCL an die Benutzeroberfläche verfügbar gemacht werden". Die iOS-Anwendung verfügt über zwei Klassen, um Tasks anzuzeigen:

 -   **EditingSource** – diese Klasse wird verwendet, um Listen mit Aufgaben für die Benutzeroberfläche binden. Da `MonoTouch.Dialog` verwendet wurde für die Aufgabenliste müssen wir implementieren dieses Hilfsprogramm zum Aktivieren Wischen-Delete-Funktionen in der `UITableView` . Wischen zu löschen ist auf iOS, jedoch keine Android oder Windows Phone-üblich, damit bestimmte iOS-Projekt die nur eine Methode, die ihn implementiert.
 -   **' TaskDialog ' muss** – diese Klasse wird verwendet, um eine einzelne Aufgabe an die Benutzeroberfläche binden. Er verwendet die `MonoTouch.Dialog` Reflektions-API "umschlossen" die `TaskItem` Objekt mit einer Klasse enthält die richtigen Attribute, damit die Eingabebildschirm ordnungsgemäß formatiert werden können.

Die `TaskDialog` -Klasse verwendet `MonoTouch.Dialog` Attribute zum Erstellen eines Bildschirms basierend auf einer Klasse Eigenschaften. Die Klasse sieht wie folgt:

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

Beachten Sie, dass die `OnTap` Attribute sind erforderlich, einen Methodennamen – diese Methoden müssen in der Klasse vorhanden sein, in dem die `MonoTouch.Dialog.BindingContext` erstellt wird (in diesem Fall die `HomeScreen` Klasse, die im nächsten Abschnitt erläutert).

 <a name="User_Interface_Layer_(UI)" />

### <a name="user-interface-layer-ui"></a>Die Benutzeroberflächenebene (UI)

Die Benutzeroberflächenebene besteht aus den folgenden Klassen:

1.   **AppDelegate** – enthält Aufrufe an die API für die Darstellung der Schriftarten und Farben, die in der Anwendung verwendeten formatieren. Tasky ist eine eine einfache Anwendung, daher gibt es keine andere Initialisierung ausgeführten Tasks im `FinishedLaunching` .
2.   **Bildschirme** – Unterklassen des `UIViewController` , jeder Bildschirm und sein Verhalten definieren. Bildschirme miteinander verknüpfen, die Benutzeroberfläche mit Anwendungsschicht Klassen und der gemeinsame-API ( `TaskItemManager` ). In diesem Beispiel werden die Bildschirme im Code erstellt, aber sie konnte wurden entwickelt mit Xcodes Benutzeroberflächen-Generator oder des Storyboard-Designers.
3.   **Bilder** – visuelle Elemente sind ein wichtiger Teil jeder Anwendung. Tasky hat Splash Bildschirm- und Symbol für Bilder, die für iOS im regulären und Retina-Lösung angegeben werden müssen.

 <a name="Home_Screen" />

#### <a name="home-screen"></a>Startbildschirm

Der Startbildschirm wird eine `MonoTouch.Dialog` Bildschirm, in dem eine Liste von Aufgaben, mit der SQLite-Datenbank angezeigt. Es erbt von `DialogViewController` und implementiert Code aus, um die `Root` enthält eine Auflistung von `TaskItem` Objekte für die Anzeige.

 [![](case-study-tasky-images/ios-taskylist.png "Es erbt von DialogViewController und implementiert Code zum Stammverzeichnis enthält eine Auflistung von TaskItem-Objekten, für die Anzeige festlegen")](case-study-tasky-images/ios-taskylist.png#lightbox)

Die beiden wichtigsten Methoden im Zusammenhang mit der Anzeige und Interaktion mit der Aufgabenliste werden:

1.   **PopulateTable** – verwendet die Geschäftsebene `TaskManager.GetTasks` Methode zum Abrufen einer Auflistung von `TaskItem` Objekte angezeigt.
2.   **Ausgewählte** – Wenn eine Zeile verwendet wird, wird die Aufgabe in einem neuen Bildschirm angezeigt.

 <a name="Task_Details_Screen" />

#### <a name="task-details-screen"></a>Bildschirm "Details" Task

Aufgabendetails ist ein Eingabebildschirm, die Aufgaben, bearbeitet oder gelöscht werden kann.

Tasky verwendet `MonoTouch.Dialog`Reflektions-API, um den Bildschirm anzuzeigen also, dass sich keine `UIViewController` Implementierung. Stattdessen die `HomeScreen` Klasse instanziiert und zeigt eine `DialogViewController` mithilfe der `TaskDialog` Klasse von der Anwendungsschicht.

Diese bildschirmabbildung zeigt einen leeren Bildschirm, der veranschaulicht die `Entry` -Attribut festlegen den Wasserzeichentext in der **Namen** und **Hinweise** Felder:

 [![](case-study-tasky-images/ios-taskydetail.png "Diese bildschirmabbildung zeigt einen leeren Bildschirm, der veranschaulicht das Festlegen der Wasserzeichentext in den Feldern Name und Anmerkungen zu dieser Eintrag-Attribut")](case-study-tasky-images/ios-taskydetail.png#lightbox)

Die Funktionalität von der **Aufgabendetails** Bildschirm bereitstellt (z.B. speichern oder Löschen eines Tasks) muss implementiert werden, der `HomeScreen` Klasse, da dies ist, die `MonoTouch.Dialog.BindingContext` wird erstellt. Die folgenden `HomeScreen` Methoden unterstützen die Aufgabendetails Bildschirm:

1.   **ShowTaskDetails** – erstellt eine `MonoTouch.Dialog.BindingContext` zum Rendern eines Bildschirms. Erstellt die Eingabebildschirm verwenden Reflektion zum Abrufen von Eigenschaftennamen und Typen aus den `TaskDialog` Klasse. Weitere Informationen, z. B. Wasserzeichentexts für den Eingabefeldern wird mit Attributen auf die Eigenschaften implementiert.
2.   **SaveTask** – diese Methode verwiesen wird, der `TaskDialog` Klasse über eine `OnTap` Attribut. Wird aufgerufen, wenn **speichern** gedrückt wird, und verwendet eine `MonoTouch.Dialog.BindingContext` zum Abrufen von den Benutzer eingegeben Daten vor dem Speichern der Änderungen mit `TaskItemManager` .
3.   **DeleteTask** – diese Methode verwiesen wird, der `TaskDialog` Klasse über eine `OnTap` Attribut. Er verwendet `TaskItemManager` So löschen Sie die Daten mithilfe des Primärschlüssels (ID-Eigenschaft).

 <a name="Android_App" />

## <a name="android-app"></a>Android-App

Das vollständige Xamarin.Android-Projekt wird unten gezeigt:

 ![](case-study-tasky-images/taskyandroid-solution.png "Android-Projekt wird im folgenden abgebildet.")

Das Klassendiagramm mit Klassen gruppiert nach Ebene:

 [![](case-study-tasky-images/classdiagram-android.png "Das Klassendiagramm mit Klassen gruppiert nach Ebene")](case-study-tasky-images/classdiagram-android.png#lightbox)

 <a name="References" />

### <a name="references"></a>Verweise

Das Android-app-Projekt muss die plattformspezifischen Xamarin.Android Assembly auf Klassen aus dem Android-SDK verweisen.

Sie müssen auch die PCL-Projekt (z. b. verweisen TaskyPortableLibrary) zum Zugriff auf gemeinsame Daten und Layer-Code.

 ![](case-study-tasky-images/taskyandroid-references.png "TaskyPortableLibrary zum Zugriff auf gemeinsame Daten und Layer-code")

 <a name="Application_Layer_(AL)" />

### <a name="application-layer-al"></a>Anwendungsebene (AL)

Ähnlich wie in der iOS-Version, der wir besprochen enthält früher-Anwendungsebene in der Android-Version Clientplattform-spezifische Klassen erforderlich, um "binden Sie die Objekte, die von der Kerne und die Benutzeroberfläche verfügbar gemacht werden".

 **TaskListAdapter** – zum Anzeigen einer Liste<T> von Objekten wir implementieren einen Adapter, um die Anzeige der benutzerdefinierten Objekte in müssen einem `ListView`. Der Adapter wird gesteuert, welche Anordnung für jedes Element in der Liste verwendet wird – in diesem Fall verwendet der Code ein Layout für Android integrierte `SimpleListItemChecked`.

 <a name="User_Interface_(UI)" />

### <a name="user-interface-ui"></a>Benutzeroberfläche (UI)

Der Android-app-Benutzeroberflächenebene ist eine Kombination von Code und XML-Markup.

 -   **Ressourcen/Layout** – Bildschirmlayouts und die Zeile der Zelle Entwurf in Dateiform durch AXML implementiert. Die AXML kann manuell geschrieben oder Layout visuell mit dem Xamarin-UI-Designer für Android werden.
 -   **Ressourcen/Drawable** – Bilder (Symbole) und benutzerdefinierte Schaltfläche.
 -   **Bildschirme** – Aktivität Unterklassen, die jeder Bildschirm und sein Verhalten definieren. Verknüpft die Benutzeroberfläche mit Anwendungsschicht Klassen und der gemeinsame-API (`TaskItemManager`).

 <a name="Home_Screen" />

#### <a name="home-screen"></a>Startbildschirm

Die Startseite besteht aus einer Aktivität-Unterklasse `HomeScreen` und die `HomeScreen.axml` Datei definiert das Layout (Position der Schaltfläche und jede Aufgabe Liste). Der Bildschirm sieht wie folgt:

 [![](case-study-tasky-images/android-taskylist.png "Sieht wie folgt aus der Bildschirm")](case-study-tasky-images/android-taskylist.png#lightbox)

Der Startbildschirm-Code definiert die Handler für das Klicken auf die Schaltfläche und durch Klicken auf Elemente in der Liste von Elementen sowie das Auffüllen der Liste der in der `OnResume` Methode (so, dass die It gibt in der Aufgabe Detailbildschirm vorgenommenen Änderungen wieder). Daten werden mithilfe der Geschäftsebene geladen `TaskItemManager` und `TaskListAdapter` von der Anwendungsebene.

 <a name="Task_Details_Screen" />

#### <a name="task-details-screen"></a>Bildschirm "Details" Task

Auch besteht aus den Detailbildschirm Task ein `Activity` -Unterklasse und eine AXML-Layout-Datei. Das Layout bestimmt den Speicherort der Eingabesteuerelemente und die C#-Klasse definiert das Verhalten zum Laden und Speichern von `TaskItem` Objekte.

 [![](case-study-tasky-images/android-taskydetail.png "Die Klasse definiert das Verhalten zum Laden und speichern Sie TaskItem-Objekte")](case-study-tasky-images/android-taskydetail.png#lightbox)

Alle Verweise auf die PCL-Bibliothek werden über die `TaskItemManager` Klasse.

 <a name="Windows_Phone_App" />

## <a name="windows-phone-app"></a>Windows Phone-App
Das vollständige Windows Phone-Projekt:

 ![](case-study-tasky-images/taskywp7-solution.png "Windows Phone-App das vollständige Windows Phone-Projekt")

Das folgende Diagramm zeigt die Klassen, die in Ebenen unterteilt:

 [![](case-study-tasky-images/classdiagram-wp7.png "Dieses Diagramm zeigt die Klassen, die in Ebenen gruppiert")](case-study-tasky-images/classdiagram-wp7.png#lightbox)

 <a name="References" />

### <a name="references"></a>Verweise

Das plattformspezifische Projekt muss die erforderlichen plattformspezifische Bibliotheken verweisen (z. B. `Microsoft.Phone` und `System.Windows`) um eine gültige Windows Phone-Anwendung zu erstellen.

Sie müssen auch die PCL-Projekt (z. b. verweisen `TaskyPortableLibrary`) zum Nutzen der `TaskItem` Klasse und der Datenbank.

 ![](case-study-tasky-images/taskywp7-references.png "TaskyPortableLibrary die TaskItem-Klasse und die Datenbank nutzen können")

 <a name="Application_Layer_(AL)" />

### <a name="application-layer-al"></a>Anwendungsebene (AL)

Erneut besteht aus wie bei IOS- und Android-Versionen, die Anwendungsschicht nicht visuelle Elemente, mit deren Hilfe Sie Daten an die Benutzeroberfläche binden.

 <a name="ViewModels" />

#### <a name="viewmodels"></a>ViewModels

ViewModels umschließen, Daten aus der PCL ( `TaskItemManager`) und zeigt sie in der Weise, die von Silverlight/XAML-Datenbindung verwendet werden kann. Dies ist ein Beispiel Clientplattform-spezifische Verhalten (wie im Dokument plattformübergreifende Anwendungen erläutert wird).

 <a name="User_Interface_(UI)" />

### <a name="user-interface-ui"></a>Benutzeroberfläche (UI)

XAML-hat eine eindeutige Datenbindungsfunktionen-Funktion, die im Markup deklariert werden kann, und verringern Sie die Menge des Codes erforderlich, um die Objekte anzuzeigen:

1.   **Seiten** – Verwendung von XAML-Dateien und deren Codebehind definieren die Benutzeroberfläche und verweisen auf die ViewModels und PCL-Projekt, um anzuzeigen, und Sammeln von Daten.
2.   **Bilder** – Splash-Bildschirm, Hintergrund und Symbol-Images sind ein wesentlicher Bestandteil des der Benutzeroberfläche.

 <a name="MainPage" />

#### <a name="mainpage"></a>MainPage

Der MainPage-Klasse verwendet die `TaskListViewModel` zum Anzeigen von Daten mithilfe des XAML-Datenbindung Funktionen. Der Seite `DataContext` festgelegt ist, um das Modell anzeigen, die asynchron aufgefüllt wird. Die `{Binding}` Syntax in der XAML-Code bestimmt, wie die Daten angezeigt werden.

 <a name="TaskDetailsPage" />

#### <a name="taskdetailspage"></a>TaskDetailsPage

Jede Aufgabe wird angezeigt, durch die Bindung der `TaskViewModel` auf der XAML-Code in der TaskDetailsPage.xaml definiert. Die Aufgabendaten abgerufen werden, über die `TaskItemManager` in der Geschäftsebene.

 <a name="Results" />

## <a name="results"></a>Ergebnisse

Die resultierenden Anwendungen werden auf jeder Plattform wie folgt aussehen:

 <a name="iOS" />

#### <a name="ios"></a>iOS

Die Anwendung verwendet die iOS-Standard-Benutzeroberflächendesign, z. B. die Schaltfläche "hinzufügen" in der Navigationsleiste positioniert wird, und unter Verwendung der integrierten **Pluszeichen (+)** Symbol. Darüber hinaus verwendet die standardmäßige `UINavigationController` Schaltfläche "zurück" Verhalten "und" unterstützt "Streifen löschen" in der Tabelle.

 [![](case-study-tasky-images/ios-taskylist.png "Außerdem verwendet das Standardverhalten für UINavigationController Schaltfläche \"zurück\" und unterstützt Streifen zu löschen, in der Tabelle") ](case-study-tasky-images/ios-taskylist.png#lightbox) [ ![ ] (case-study-tasky-images/ios-taskylist.png "darüber hinaus verwendet die standardmäßige UINavigationController Sichern Sie die Schaltfläche Verhalten und unterstützt Streifen zu löschen, in der Tabelle")](case-study-tasky-images/ios-taskylist.png#lightbox)

 <a name="Android" />

#### <a name="android"></a>Android

Die Android-app verwendet die integrierte Steuerelemente, einschließlich des integrierten Layouts für Zeilen, die erfordern einen Takt"angezeigt. Das Hardwaresystem/Back Verhalten wird zusätzlich zu den auf dem Bildschirm Zurückschaltfläche unterstützt.

 [![](case-study-tasky-images/android-taskylist.png "Das Hardwaresystem/Back Verhalten wird unterstützt, zusätzlich zu den auf dem Bildschirm Zurückschaltfläche")](case-study-tasky-images/android-taskylist.png#lightbox)[![](case-study-tasky-images/android-taskylist.png "das Hardwaresystem/Back Verhalten wird unterstützt, zusätzlich zu einer auf dem Bildschirm Schaltfläche \"zurück\"")](case-study-tasky-images/android-taskylist.png#lightbox)

 <a name="Windows_Phone" />

#### <a name="windows-phone"></a>Windows Phone

Die Windows Phone-app verwendet das Standardlayout, Auffüllen der app-Leiste am unteren Rand des Bildschirms, anstatt eine Navigationsleiste am oberen.

 [![](case-study-tasky-images/wp-taskylist.png "Die Windows Phone-app verwendet das Standardlayout, Auffüllen der app-Leiste am unteren Rand des Bildschirms, anstatt eine Navigationsleiste am oberen") ](case-study-tasky-images/wp-taskylist.png#lightbox) [ ![ ] (case-study-tasky-images/wp-taskylist.png "verwendet den Standard, der Windows Phone-app Layout die app-Leiste am unteren Rand des Bildschirms, anstatt eine Navigationsleiste am oberen Auffüllen")](case-study-tasky-images/wp-taskylist.png#lightbox)

 <a name="Summary" />

## <a name="summary"></a>Zusammenfassung

Dieses Dokument stellt eine ausführliche Erläuterung, wie die Prinzipien der mehrschichtigen Anwendung entwerfen, um Code erneut zu verwenden, die von drei mobile Plattformen zu ermöglichen einer einfachen Anwendung angewendet wurden: iOS, Android und Windows Phone.

Sie hat beschrieben, wie verwendet, um die Anwendungsebenen entwerfen und welcher Code erläutert &amp; Funktionen in jeder Ebene implementiert wurde.

Der Code kann heruntergeladen werden [Github](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable).

## <a name="related-links"></a>Verwandte Links

- [Erstellen von plattformübergreifenden Anwendungen (Hauptspeicherorte für Dokumente)](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [Tasky Portable-Beispiel-App (Github)](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)
