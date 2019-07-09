---
title: 'Plattformübergreifende App-Fallstudie: Tasky'
description: In diesem Dokument wird beschrieben, wie die Tasky Portable-beispielanwendung entworfen und als eine plattformübergreifende mobile-Anwendung erstellt wurde. Es wird erläutert, der app Anforderungen, Schnittstelle, Datenmodell, Kernfunktionen, Implementierung und mehr.
ms.prod: xamarin
ms.assetid: B581B2D0-9890-C383-C654-0B0E12DAD5A6
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 15b4154ad6e95aabb5e88784660a93bb53c0b252
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2019
ms.locfileid: "67650208"
---
# <a name="cross-platform-app-case-study-tasky"></a>Plattformübergreifende App-Fallstudie: Tasky

*Tasky* *Portable* ist eine einfache aufgabenlistenanwendung. In diesem Dokument wird erläutert, wie es entworfen und erstellt werden, gemäß Anleitung von wurde die [Building Cross-Platform Applications](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) Dokument. Die folgenden Bereiche behandelt:

<a name="Design_Process" />

## <a name="design-process"></a>Entwurfsprozess

Ist es ratsam, erstellen Sie eine der Wegweiser für das, was Sie erreichen, bevor Sie mit dem Programmieren beginnen möchten. Dies gilt insbesondere für die plattformübergreifende Entwicklung, in dem Sie Funktionen erstellen, die auf verschiedene Weise verfügbar gemacht werden. Beginnend mit der eine klare Vorstellung davon, was Sie spart Zeit und Mühe später im Entwicklungszyklus erstellen.

 <a name="Requirements" />

### <a name="requirements"></a>Anforderungen

Der erste Schritt beim Entwerfen einer Anwendung ist auf die gewünschte Funktionen zu identifizieren. Dabei kann es sich um allgemeine Ziele oder Anwendungsfälle. Tasky hat einfache funktionsbezogenen Anforderungen:

 -  Anzeigen einer Liste von Aufgaben
 -  Hinzufügen, bearbeiten und Löschen von Aufgaben
 -  Festlegen Sie Status einer Aufgabe, auf "Fertig"

Sie sollten die Verwendung von plattformspezifischen Features berücksichtigen.  Kann Tasky Geofencing für iOS oder Windows Phone-Live-Kacheln nutzen? Auch wenn Sie in der ersten Version nicht plattformspezifischen Features verwenden, sollten Sie voraus planen, um sicherzustellen, dass Ihr Unternehmen & Datenebenen aufnehmen können.

 <a name="User_Interface_Design" />

### <a name="user-interface-design"></a>Benutzeroberflächendesign

Beginnen Sie mit der ein allgemeiner Entwurf, der über die Zielplattformen implementiert werden kann. Achten Sie darauf, Hinweis Plattform-spezifische UI-Einschränkungen. Z. B. eine `TabBarController` in iOS können zeigt mehr als fünf Schaltflächen an, während die Windows Phone-Entsprechung bis zu vier anzeigen kann.
Zeichnen Sie den Bildschirm-Fluss, der mit dem Tool Ihrer Wahl (Dokument funktioniert).

 [![](case-study-tasky-images/taskydesign.png "Festlegen des Bildschirm-Datenflusses mit dem Tool Ihrer Wahl Papier funktioniert")](case-study-tasky-images/taskydesign.png#lightbox)

 <a name="Data_Model" />

### <a name="data-model"></a>Datenmodell

Zu wissen, welche Daten müssen gespeichert werden können Sie bestimmen die persistenzmechanismus verwendet. Finden Sie unter [plattformübergreifenden Datenzugriff](~/cross-platform/app-fundamentals/index.md) für Informationen zu den verfügbaren Speichermechanismen und Informationen dazu, zwischen ihnen. Für dieses Projekt wird von SQLite.NET verwendet werden.

Tasky benötigt, um drei Eigenschaften für jede "TaskItem" zu speichern:

 -  **Namen** – Zeichenfolge
 -  **Anmerkungen zu dieser** – Zeichenfolge
 -  **Fertig** – Boolesch

 <a name="Core_Functionality" />

### <a name="core-functionality"></a>Kernfunktionalität

Erwägen Sie die API, die die Benutzeroberfläche verwenden, um die Anforderungen erfüllen muss. Eine to-Do-Liste muss die folgenden Funktionen:

 -   **Auflisten aller Aufgaben** : Anzeige die Hauptbildschirm-Liste aller verfügbaren Tasks
 -  **Erhalten Sie eine Aufgabe** – Wenn eine Aufgabenzeile livekomponententests wird
 -  **Speichern Sie eine Aufgabe** – Wenn eine Aufgabe bearbeitet wird
 -  **Löschen Sie eine Aufgabe** – Wenn eine Aufgabe gelöscht wird
 -  **Erstellen von leeren Task** : Wenn Sie eine neue Aufgabe erstellt wird

Um die Wiederverwendung von Code zu erreichen, diese API sollte ein Mal implementiert werden der *Portable Class Library*.

 <a name="Implementation" />

### <a name="implementation"></a>Implementierung

Nachdem Sie der Anwendungsentwurf bei geeinigt haben, erwägen Sie, wie sie als Cross-Platform-Anwendung implementiert werden kann. Dadurch werden die Architektur der Anwendung. Anhand der Anweisungen in der [Building Cross-Platform Applications](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) Dokument der Anwendungscode muss unterbrochen werden nach unten in den folgenden Teilen:

 -   **Gemeinsamer Code** – ein gemeinsames Projekt, die enthält aktionsentwicklung Code zum Speichern der Aufgabendaten verfügbar machen, eine Modellklasse und eine API zum Verwalten von speichern und Laden von Daten.
 -   **Plattformspezifischen Code** – plattformspezifischen Projekte, die eine native Benutzeroberfläche für jedes Betriebssystem, mit der allgemeine Code als "Back-End" implementieren.

 [![](case-study-tasky-images/taskypro-architecture.png "Plattformspezifischen Projekten implementiert, eine native Benutzeroberfläche für jedes Betriebssystem, mit der allgemeine Code als Back-end")](case-study-tasky-images/taskypro-architecture.png#lightbox)

Diese beiden Teile werden in den folgenden Abschnitten beschrieben.

 <a name="Common_(PCL)_Code" />

## <a name="common-pcl-code"></a>Allgemeine (PCL) Code

Tasky Portable verwendet die Portable Class Library-Strategie für die Freigabe von gemeinsamen Codes. Finden Sie unter den [Sharing Code Options](~/cross-platform/app-fundamentals/code-sharing.md) Dokument für eine Beschreibung der Optionen für die gemeinsame Verwendung von Code.

Alle allgemeinen Code ab, einschließlich der Datenzugriffsebene, die Datenbankcode und die Verträge, wird in das Bibliotheksprojekt platziert.

Das vollständige PCL-Projekt wird im folgenden dargestellt. Der gesamte Code in der portablen Bibliothek ist kompatibel mit jede Zielplattform. Bei der Bereitstellung wird jede systemeigene app diese Bibliothek verwenden.

![](case-study-tasky-images/portable-project.png "Bei der Bereitstellung wird jede systemeigene app diese Bibliothek verwenden.")

Das folgende Klassendiagramm zeigt die Klassen, gruppiert nach Ebene. Die `SQLiteConnection` Klasse ist ein Codebaustein, aus dem Sqlite-NET-Paket. Die übrigen Klassen sind die benutzerdefinierten Code für Tasky. Die `TaskItemManager` und `TaskItem` Klassen stellen dar, die API, die für die plattformspezifische Anwendungen verfügbar gemacht wird.

 [![](case-study-tasky-images/classdiagram-core.png "Die TaskItemManager und TaskItem-Klassen darstellen, die API, die für die plattformspezifische Anwendungen verfügbar gemacht wird")](case-study-tasky-images/classdiagram-core.png#lightbox)

Verwenden von Namespaces auf die Ebenen isolieren kann, zum Verwalten von Verweisen zwischen jeder Ebene. Die plattformspezifischen Projekten müssen nur umfassen eine `using` -Anweisung für die Geschäftsschicht. Die Daten von Zugriffsschicht und der Datenschicht sollte von der API, die verfügbar gemacht werden gekapselt werden `TaskItemManager` in der Geschäftsschicht.

 <a name="References" />

### <a name="references"></a>Verweise

Portable Klassenbibliotheken müssen auf mehreren Plattformen, die jeweils verschiedene Ebenen der Unterstützung für die Plattform und Framework-Features verwendet werden kann. Aus diesem Grund gelten einige Einschränkungen, die für die Pakete und Framework-Bibliotheken verwendet werden können. Beispielsweise Xamarin.iOS unterstützt nicht die c#- `dynamic` -Schlüsselwort, daher eine portablen Klassenbibliothek kann keine Pakete, die von dynamischem Code abhängig ist, obwohl Sie diesen Code unter Android funktionieren würde. Visual Studio für Mac wird verhindert, dass Sie aus der Addition inkompatible Pakete und Verweise, aber Sie sollten zu Einschränkungen berücksichtigen, um spätere überraschungen zu vermeiden.

Hinweis: Sie sehen, dass Ihre Projekte zu Framework-Bibliotheken verweisen, die Sie nicht verwendet haben. Diese Verweise werden als Teil der Xamarin-Projektvorlagen enthalten. Wenn apps kompiliert werden, der Verknüpfungsvorgang entfernt Unreferenzierte Code, also auch wenn `System.Xml` wurde auf die verwiesen wird, wird nicht aufgenommen in der endgültigen Anwendung, da wir keine XML-Funktionen verwenden.

 <a name="Data_Layer_(DL)" />

### <a name="data-layer-dl"></a>Datenschicht (DL)

Die Datenschicht enthält den Code, der die physische Speicherung von Daten zu einer Datenbank, Flatfile-Dateien oder anderen Mechanismus. Die Tasky Datenschicht besteht aus zwei Teilen: der SQLite-NET-Bibliothek und den benutzerdefinierten Code hinzugefügt, um zu verknüpfen.

Tasky basiert auf der Sqlite-Net-Nuget-Paket (veröffentlicht von Frank Kreuger) zum Einbetten von SQLite-NET-Code, der eine Datenbankschnittstelle der Object-Relational Mapping (ORM)-bereitstellt. Die `TaskItemDatabase` Klasse erbt von `SQLiteConnection` und fügt die erforderlichen Erstellungs-, Lese-, Update, Delete (CRUD) Methoden zum Lesen und Schreiben von Daten in SQLite. Es ist eine einfache Textbausteine-Implementierung der generischen CRUD-Methoden, die in anderen Projekten wiederverwendet werden kann.

Die `TaskItemDatabase` ist ein Singleton, sicherzustellen, dass alle Zugriffe für dieselbe Instanz auftritt. Eine Sperre wird verwendet, um gleichzeitigen Zugriff von mehreren Threads zu verhindern.

 <a name="SQLite_on_WIndows_Phone" />

#### <a name="sqlite-on-windows-phone"></a>SQLite on Windows Phone

Bei iOS und Android sowohl liefern mit SQLite als Teil des Betriebssystems, umfasst Windows Phone keine kompatiblen Datenbank-Engine. Zum Teilen von Code auf allen drei Plattformen ist eine Windows Phone-Native-Version von SQLite erforderlich. Finden Sie unter [arbeiten mit einer lokalen Datenbank](~/xamarin-forms/data-cloud/data/databases.md) für Weitere Informationen zum Einrichten von Ihrem Windows Phone-Projekt für Sqlite.

 <a name="Using_an_Interface_to_Generalize_Data_Access" />

#### <a name="using-an-interface-to-generalize-data-access"></a>Generalisieren Sie den Datenzugriff mithilfe einer Schnittstelle

Die Datenschicht verwendet eine Abhängigkeit `BL.Contracts.IBusinessIdentity` , damit sie abstrakte Methoden für den Datenzugriff implementieren kann, die einen primären Schlüssel erfordern. Jede Geschäftsschicht-Klasse, die die Schnittstelle implementiert, kann dann in der Datenschicht beibehalten werden.

Die Schnittstelle gibt nur eine Ganzzahleigenschaft, die als Primärschlüssel fungiert:

```csharp
public interface IBusinessEntity {
    int ID { get; set; }
}
```

Die Basisklasse der Klasse implementiert die Schnittstelle und fügt die SQLite-NET-Attribute, um diese als ein automatisch erhöhenden Primärschlüssel kennzeichnen. Jede Klasse in der Geschäftsschicht, die diese Basisklasse implementiert, kann dann in der Datenschicht dauerhaft gespeichert werden:

```csharp
public abstract class BusinessEntityBase : IBusinessEntity {
    public BusinessEntityBase () {}
    [PrimaryKey, AutoIncrement]
    public int ID { get; set; }
}
```

Ein Beispiel für den generischen Methoden in der Datenschicht, die die Schnittstelle verwenden, ist dies `GetItem<T>` Methode:

```csharp
public T GetItem<T> (int id) where T : BL.Contracts.IBusinessEntity, new ()
{
    lock (locker) {
        return Table<T>().FirstOrDefault(x => x.ID == id);
    }
}
```

 <a name="Locking_to_prevent_Concurrent_Access" />

#### <a name="locking-to-prevent-concurrent-access"></a>Sperren, um den gleichzeitigen Zugriff zu verhindern

Ein [Sperre](https://msdn.microsoft.com/library/c5kehkcz(v=vs.100).aspx) wird implementiert, in der `TaskItemDatabase` Klasse, um den gleichzeitigen Zugriff auf die Datenbank zu verhindern. Dadurch wird sichergestellt, gleichzeitigen Zugriff aus verschiedenen Threads serialisiert wird (andernfalls eine UI-Komponente könnte versuchen, die Datenbank zur gleichen Zeit Lesen von ein Hintergrundthread wird aktualisiert wird). Ein Beispiel für die Implementierung der Sperre wird hier gezeigt:

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

Großteil des Codes für die Datenschicht kann in anderen Projekten wiederverwendet werden. Der nur die anwendungsspezifischen Code in der Ebene ist die `CreateTable<TaskItem>` rufen Sie in der `TaskItemDatabase` Konstruktor.

 <a name="Data_Access_Layer_(DAL)" />

### <a name="data-access-layer-dal"></a>Die Datenzugriffsebene (DAL)

Die `TaskItemRepository` -Klasse kapselt den Mechanismus zur datenspeicherung, mit einer stark typisierten API, die ermöglicht `TaskItem` Objekte erstellt, gelöscht, abgerufen und aktualisiert.

 <a name="Using_Conditional_Compilation" />

#### <a name="using-conditional-compilation"></a>Verwenden für die bedingte Kompilierung

Die Klasse verwendet für die bedingte Kompilierung, zum Speicherort der Datei festlegen – Dies ist ein Beispiel der Implementierung der Plattform Abweichungen. Die Eigenschaft, die den Pfad gibt, die in unterschiedlichen Code auf jeder Plattform kompiliert werden. Der Code und plattformspezifischen Compiler-Direktiven sind hier dargestellt:

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

Abhängig von der Plattform, die Ausgabe ist "<app
path>/Library/TaskDB.db3" für iOS "<app
path>/Documents/TaskDB.db3" für Android oder einfach "TaskDB.db3" für Windows Phone.

### <a name="business-layer-bl"></a>Die Geschäftsebene (BL)

Die Geschäftsschicht der ViewModel-Klassen und eine Fassade für deren Verwaltung implementiert werden.
Das Modell in Tasky ist die `TaskItem` Klasse und `TaskItemManager` implementiert das Muster "Fassade" zum Bereitstellen einer API zum Verwalten von `TaskItems`.

 <a name="Façade" />

#### <a name="faade"></a>Fassade

 `TaskItemManager` Dient als Wrapper für die `DAL.TaskItemRepository` um GET-Anforderung zu gewährleisten, speichern und Delete-Methoden, die von der Anwendung und die UI-Ebenen verwiesen werden.

Geschäftsregeln und-Logik würden hier platziert werden, wenn erforderlich – z. B. alle Validierungsregeln, die erfüllt sein müssen, bevor ein Objekt gespeichert wird.

 <a name="API_for_Platform-Specific_Code" />

### <a name="api-for-platform-specific-code"></a>API für die plattformspezifischen Code

Nach der allgemeine Code geschrieben wurde, muss die Benutzeroberfläche erstellt werden, zum Sammeln und Anzeigen der Daten verfügbar gemacht werden. Die `TaskItemManager` -Klasse implementiert das Muster Fassade um eine einfache API den Zugriff auf den Anwendungscode bereitzustellen.

In den einzelnen plattformspezifischen Projekten geschriebene Code ist in der Regel eng mit dem systemeigenen SDK des Geräts verbunden, und nur auf der allgemeine Code mithilfe der API, die von definiert die `TaskItemManager`. Dies umfasst auch die Methoden und Business-Klassen sie verfügbar macht, wie z. B. `TaskItem`.

Images sind nicht plattformübergreifend genutzt, sondern jedes Projekt separat hinzugefügt. Dies ist wichtig, da jede Plattform mit unterschiedlichen Dateinamen, Verzeichnisse und Lösungen für Bilder anders behandelt werden.

Die verbleibenden Abschnitte erläutern die Clientplattform-spezifische Implementierungsdetails der Tasky Benutzeroberfläche.

 <a name="iOS_App" />

## <a name="ios-app"></a>iOS-App

Es gibt nur eine Handvoll von Klassen, die zur Implementierung der iOS-Tasky-Anwendung mit das gemeinsame PCL-Projekt zu speichern und Abrufen von Daten erforderlich sind. Das komplette iOS Xamarin.iOS-Projekt wird unten gezeigt:

 ![](case-study-tasky-images/taskyios-solution.png "iOS-Projekt wird hier angezeigt.")

Die Klassen werden in diesem Diagramm in Ebenen gruppiert angezeigt.

 [![](case-study-tasky-images/classdiagram-android.png "Die Klassen werden in diesem Diagramm in Ebenen gruppiert angezeigt.")](case-study-tasky-images/classdiagram-android.png#lightbox)

 <a name="References" />

### <a name="references"></a>Verweise

Die iOS-app verweist die Clientplattform-spezifische SDK-Bibliotheken – z. B. auf. Xamarin.iOS- und MonoTouch.Dialog-1.

Sie müssen auch verweisen die `TaskyPortableLibrary` PCL-Projekt.
Liste der Verweise ist hier dargestellt:

 ![](case-study-tasky-images/taskyios-references.png "Liste der Verweise wird hier angezeigt.")

In diesem Projekt verwenden diese Verweise werden die Anwendungsschicht sowie die Benutzeroberflächenebene implementiert.

 <a name="Application_Layer_(AL)" />

### <a name="application-layer-al"></a>Anwendungsschicht (AL)

Die Anwendungsschicht enthält plattformspezifischen Klassen erforderlich, um "binden Sie die Objekte, die von der PCL, an der Benutzeroberfläche verfügbar gemacht werden". Die iOS-spezifische Anwendung besteht aus zwei Klassen, um Tasks anzuzeigen:

 -   **EditingSource** – diese Klasse wird verwendet, um Listen mit Aufgaben an die Benutzeroberfläche zu binden. Da `MonoTouch.Dialog` wurde verwendet, für die Aufgabenliste, müssen wir implementieren dieses Hilfsprogramm zum Aktivieren von Wischen-zu-Delete-Funktionen in der `UITableView` . Wischen-zu-Delete ist häufig bei iOS, jedoch keine Android- oder Windows Phone, also das bestimmte iOS-Projekt die einzige, die sie implementiert.
 -   **' TaskDialog ' muss** – diese Klasse wird verwendet, um eine einzelne Aufgabe an der Benutzeroberfläche zu binden. Er verwendet den `MonoTouch.Dialog` Reflektions-API "umschlossen" die `TaskItem` Objekt mit einer Klasse mit den richtigen Attributen damit dem Eingabebildschirm ordnungsgemäß formatiert werden kann.

Die `TaskDialog` -Klasse `MonoTouch.Dialog` Attribute zum Erstellen eines Bildschirms basierend auf einer Klasse Eigenschaften. Die Klasse sieht folgendermaßen aus:

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

Beachten Sie, dass die `OnTap` erfordern einen Methodennamen – diese Methoden müssen in der Klasse vorhanden sein, in denen die `MonoTouch.Dialog.BindingContext` erstellt wird (in diesem Fall die `HomeScreen` Klasse, die im nächsten Abschnitt erläutert).

 <a name="User_Interface_Layer_(UI)" />

### <a name="user-interface-layer-ui"></a>UI-Schicht (UI)

Die UI-Schicht besteht aus den folgenden Klassen:

1.   **AppDelegate** – enthält Aufrufe der API für die Darstellung zum Formatieren von Schriftarten und Farben, die in der Anwendung verwendet. Tasky ist eine eine einfache Anwendung, daher gibt es keine andere unter Initialisierungsaufgaben `FinishedLaunching` .
2.   **Bildschirme** – Unterklassen von `UIViewController` , die jeder Bildschirm und sein Verhalten definieren. Bildschirme miteinander verknüpfen, die Benutzeroberfläche mit Anwendungsschicht-Klassen und die allgemeine API ( `TaskItemManager` ). In diesem Beispiel werden die Bildschirme im Code erstellt, aber sie können wurden entworfen mit Interface Builder von Xcode oder dem Storyboard-Designer.
3.   **Images** – visuelle Elemente sind ein wichtiger Bestandteil der einzelnen Anwendungen. Tasky hat Splash Bildschirm- und Symbol dargestellt, die für iOS im regulären und Retina-Auflösung angegeben werden müssen.

 <a name="Home_Screen" />

#### <a name="home-screen"></a>Home-Bildschirm

Der Home-Bildschirm ist ein `MonoTouch.Dialog` Bildschirm, der eine Liste von Aufgaben, mit der SQLite-Datenbank angezeigt. Sie erbt von `DialogViewController` und implementiert Code zum Festlegen der `Root` enthält eine Auflistung von `TaskItem` Objekte für die Anzeige.

 [![](case-study-tasky-images/ios-taskylist.png "Es erbt von DialogViewController und implementiert Code aus, um das Stammverzeichnis enthält eine Auflistung von TaskItem-Objekten, für die Anzeige festlegen")](case-study-tasky-images/ios-taskylist.png#lightbox)

Die beiden Hauptmethoden, die im Zusammenhang mit der Anzeige und Interaktion mit der Aufgabenliste sind:

1.   **PopulateTable** – wird verwendet, die Geschäftsschicht `TaskManager.GetTasks` Methode zum Abrufen einer Auflistung von `TaskItem` Objekte angezeigt.
2.   **Ausgewählte** – Wenn eine Zeile verwendet wird, wird die Aufgabe in einem neuen Bildschirm angezeigt.

 <a name="Task_Details_Screen" />

#### <a name="task-details-screen"></a>Aufgabe der Bildschirm "Details"

Aufgabendetails ist ein Eingabebildschirm, die Aufgaben, bearbeitet oder gelöscht werden kann.

Tasky verwendet `MonoTouch.Dialog`Reflektions-API auf den Bildschirm angezeigt. Es gibt also keine `UIViewController` Implementierung. Stattdessen die `HomeScreen` -Klasse instanziiert und zeigt eine `DialogViewController` mithilfe der `TaskDialog` Klasse von der Anwendungsebene.

Dieser Screenshot zeigt einen leeren Bildschirm, das veranschaulicht, die `Entry` Attribut festlegen des Wasserzeichentexts in die **Namen** und **Anmerkungen zu dieser** Felder:

 [![](case-study-tasky-images/ios-taskydetail.png "Dieser Screenshot zeigt einen leeren Bildschirm, der zeigt, das Festlegen des Wasserzeichentexts in die Felder \"Name\" und \"Anmerkungen zu dieser Eintrag-Attribut")](case-study-tasky-images/ios-taskydetail.png#lightbox)

Die Funktionalität von der **Aufgabendetails** Bildschirm (z. B. speichern oder Löschen eines Tasks) muss implementiert werden, der `HomeScreen` Klasse, da dies ist, die `MonoTouch.Dialog.BindingContext` wird erstellt. Die folgenden `HomeScreen` Methoden unterstützen die Aufgabendetails Bildschirm:

1.   **ShowTaskDetails** – erstellt eine `MonoTouch.Dialog.BindingContext` um einen Bildschirm zu rendern. Erstellt die Eingabebildschirm mithilfe von Reflektion zum Abrufen von Eigenschaftennamen und Typen aus den `TaskDialog` Klasse. Zusätzliche Informationen wie der Wasserzeichentext für Eingabefelder, wird durch Attribute für die Eigenschaften implementiert.
2.   **SaveTask** – diese Methode wird verwiesen, der `TaskDialog` Klasse über eine `OnTap` Attribut. Wird aufgerufen, wenn **speichern** gedrückt wird, und verwendet eine `MonoTouch.Dialog.BindingContext` die eingegebenen Daten abgerufen werden, vor dem Speichern der Änderungen, die mithilfe von `TaskItemManager` .
3.   **DeleteTask** – diese Methode wird verwiesen, der `TaskDialog` Klasse über eine `OnTap` Attribut. Er verwendet `TaskItemManager` So löschen Sie die Daten, die über den primären Schlüssel (ID-Eigenschaft).

 <a name="Android_App" />

## <a name="android-app"></a>Android App

Das vollständige Xamarin.Android-Projekt ist unten abgebildet:

 ![](case-study-tasky-images/taskyandroid-solution.png "Android-Projekt ist hier abgebildet.")

Im Klassendiagramm mit Klassen, die gruppiert nach Ebene:

 [![](case-study-tasky-images/classdiagram-android.png "Im Klassendiagramm mit Klassen, die gruppiert nach Ebene")](case-study-tasky-images/classdiagram-android.png#lightbox)

 <a name="References" />

### <a name="references"></a>Verweise

Das Android-app-Projekt muss die plattformspezifische Xamarin.Android-Assembly auf Klassen aus dem Android SDK verweisen.

Es muss auch das PCL-Projekt (z.B.) verweisen TaskyPortableLibrary) auf der allgemeine Code für Daten und Business-Ebene.

 ![](case-study-tasky-images/taskyandroid-references.png "TaskyPortableLibrary auf der allgemeine Code für Daten und Business-Ebene")

 <a name="Application_Layer_(AL)" />

### <a name="application-layer-al"></a>Anwendungsschicht (AL)

Ähnlich wie die iOS-Version, die, der wir uns angesehen haben, enthält zuvor der Anwendungsebene in der Android-Version plattformspezifischen Klassen erforderlich, um "binden Sie die Objekte, die von der Kern der Benutzeroberfläche verfügbar gemacht werden".

 **TaskListAdapter** – zum Anzeigen einer Liste<T> von Objekten, die wir implementieren einen Adapter, um die benutzerdefinierten Objekte in anzeigen müssen eine `ListView`. Der Adapter wird gesteuert, welches Layout für jedes Element in der Liste verwendet wird – in diesem Fall verwendet des Codes ein integrierte Android-Layout `SimpleListItemChecked`.

 <a name="User_Interface_(UI)" />

### <a name="user-interface-ui"></a>Benutzeroberfläche (UI)

Der Android-app UI-Schicht ist eine Kombination aus Code und XML-Markup.

 -   **Ressourcen/Layout** – Bildschirmlayouts und die Zeile der Zelle Entwurf als AXML-Dateien implementiert. Die AXML kann manuell geschrieben oder visuell mit dem Xamarin-UI-Designer für Android gelayoutet sein.
 -   **Ressourcen/Drawable** – Bilder (Symbole) und benutzerdefinierte Schaltfläche.
 -   **Bildschirme** – Aktivität-Unterklassen, die jeder Bildschirm und sein Verhalten definieren. Verbindet die Benutzeroberfläche mit Anwendungsschicht-Klassen und die allgemeine API (`TaskItemManager`).

 <a name="Home_Screen" />

#### <a name="home-screen"></a>Home-Bildschirm

Der Startbildschirm besteht aus einer Aktivität-Unterklasse `HomeScreen` und `HomeScreen.axml` Datei definiert das Layout (Position der Schaltfläche und Tasks-Liste). Der Bildschirm sieht folgendermaßen aus:

 [![](case-study-tasky-images/android-taskylist.png "Der Bildschirm sieht wie folgt aus.")](case-study-tasky-images/android-taskylist.png#lightbox)

Der Startbildschirm-Code definiert die Handler für das Klicken auf die Schaltfläche und durch Klicken auf Elemente in der Liste von Elementen sowie das Auffüllen der Liste der in der `OnResume` Methode (also, dass die It in der Aufgabe Detailbildschirm vorgenommene Änderungen enthält). Daten werden mithilfe der Geschäftsschicht geladen `TaskItemManager` und `TaskListAdapter` von der Anwendungsebene.

 <a name="Task_Details_Screen" />

#### <a name="task-details-screen"></a>Aufgabe der Bildschirm "Details"

Der Detailbildschirm für die Aufgabe auch besteht aus einem `Activity` Unterklasse und eine AXML-Layoutdatei. Das Layout bestimmt den Speicherort der Benutzereingabe-Steuerelemente und die C# -Klasse definiert das Verhalten zum Laden und speichern `TaskItem` Objekte.

 [![](case-study-tasky-images/android-taskydetail.png "Die Klasse definiert das Verhalten zum Laden und speichern TaskItem-Objekte")](case-study-tasky-images/android-taskydetail.png#lightbox)

Alle Verweise auf die PCL-Bibliothek werden über die `TaskItemManager` Klasse.

 <a name="Windows_Phone_App" />

## <a name="windows-phone-app"></a>Windows Phone App
Das vollständige Windows Phone-Projekt:

 ![](case-study-tasky-images/taskywp7-solution.png "Windows Phone-App das vollständige Windows Phone-Projekt")

Das folgende Diagramm zeigt die Klassen, die in Ebenen unterteilt:

 [![](case-study-tasky-images/classdiagram-wp7.png "Dieses Diagramm zeigt die Klassen, die in Ebenen gruppiert")](case-study-tasky-images/classdiagram-wp7.png#lightbox)

 <a name="References" />

### <a name="references"></a>Verweise

Das plattformspezifische Projekt muss die erforderlichen plattformspezifische Bibliotheken verweisen (z. B. `Microsoft.Phone` und `System.Windows`) um eine gültige Windows Phone-Anwendung zu erstellen.

Es muss auch das PCL-Projekt (z.B.) verweisen `TaskyPortableLibrary`), nutzen die `TaskItem` -Klasse und der Datenbank.

 ![](case-study-tasky-images/taskywp7-references.png "TaskyPortableLibrary die TaskItem-Klasse und die Datenbank nutzen.")

 <a name="Application_Layer_(AL)" />

### <a name="application-layer-al"></a>Anwendungsschicht (AL)

In diesem Fall besteht aus wie bei IOS- und Android-Versionen, die Anwendungsschicht nicht visuelle Elemente, mit denen Sie um Daten an die Benutzeroberfläche zu binden.

 <a name="ViewModels" />

#### <a name="viewmodels"></a>ViewModels

ViewModels umschließen, Daten aus der PCL ( `TaskItemManager`) und zeigt sie in der Weise, die von Silverlight/XAML-Datenbindung genutzt werden kann. Dies ist ein Beispiel für plattformspezifisches Verhalten (wie im Dokument plattformübergreifende Anwendungen erläutert wird).

 <a name="User_Interface_(UI)" />

### <a name="user-interface-ui"></a>Benutzeroberfläche (UI)

XAML ist eine einzigartige Datenbindung-Funktion, die im Markup deklariert werden und reduzieren den Umfang des Codes erforderlich, um die Objekte anzuzeigen:

1.   **Seiten** – XAML-Dateien und ihre Codebehind die Benutzeroberfläche zu definieren und verweisen auf die ViewModels und PCL-Projekt, um anzuzeigen, und Sammeln von Daten.
2.   **Images** – Splash-Bildschirm, Hintergrund und Symbol-Images sind ein wesentlicher Teil der Benutzeroberfläche.

 <a name="MainPage" />

#### <a name="mainpage"></a>MainPage

Die MainPage-Klasse verwendet die `TaskListViewModel` zum Anzeigen von Daten mithilfe XAMLs-Datenbindung Funktionen. Die Seite `DataContext` festgelegt ist, an das Ansichtsmodell, die asynchron aufgefüllt wird. Die `{Binding}` bestimmt die XAML-Syntax, wie die Daten angezeigt werden.

 <a name="TaskDetailsPage" />

#### <a name="taskdetailspage"></a>TaskDetailsPage

Jede Aufgabe wird angezeigt, durch die Bindung der `TaskViewModel` , die XAML in die TaskDetailsPage.xaml definiert. Die taskdaten werden abgerufen, über die `TaskItemManager` in der Geschäftsschicht.

 <a name="Results" />

## <a name="results"></a>Ergebnisse

Die entstehenden Anwendungen werden auf jeder Plattform wie folgt aussehen:

 <a name="iOS" />

#### <a name="ios"></a>iOS

Die Anwendung verwendet die iOS-Standard-Entwurf der Benutzeroberfläche, z. B. die Schaltfläche "hinzufügen" in der Navigationsleiste positioniert wird, und mithilfe der integrierten **plus (+)** Symbol. Darüber hinaus verwendet er die Standardeinstellung `UINavigationController` Schaltfläche 'zurück', Verhalten und unterstützt "Streifen löschen" in der Tabelle.

 [![](case-study-tasky-images/ios-taskylist.png "Außerdem verwendet das Standardverhalten für UINavigationController Schaltfläche \"zurück\" und unterstützt Streifen zu löschen, in der Tabelle")](case-study-tasky-images/ios-taskylist.png#lightbox) [ ![](case-study-tasky-images/ios-taskylist.png "darüber hinaus verwendet die standardmäßige UINavigationController Sichern Sie die Schaltfläche Verhalten und unterstützt Streifen zu löschen, in der Tabelle")](case-study-tasky-images/ios-taskylist.png#lightbox)

 <a name="Android" />

#### <a name="android"></a>Android

Die Android-app verwendet die integrierte Steuerelemente, einschließlich des integrierten Layouts für Zeilen, die erfordern einen "Tick" angezeigt. Das Hardwaresystem /-Back-Verhalten wird zusätzlich eine Schaltfläche auf dem Bildschirm zurück zu unterstützt.

 [![](case-study-tasky-images/android-taskylist.png "Das Hardwaresystem /-Back-Verhalten wird zusätzlich eine Schaltfläche auf dem Bildschirm zurück zu unterstützt")](case-study-tasky-images/android-taskylist.png#lightbox)[![](case-study-tasky-images/android-taskylist.png "der Hardware-System-Back-Verhalten wird unterstützt, zusätzlich zu einer auf dem Bildschirm Schaltfläche \"zurück\"")](case-study-tasky-images/android-taskylist.png#lightbox)

 <a name="Windows_Phone" />

#### <a name="windows-phone"></a>Windows Phone

Die Windows Phone-app verwendet das Standardlayout, füllen die app-Leiste am unteren Rand des Bildschirms, anstatt eine Navigationsleiste oben.

 [![](case-study-tasky-images/wp-taskylist.png "Die Windows Phone-app verwendet das Standardlayout, Auffüllen der app-Leiste am unteren Rand des Bildschirms, anstatt eine Navigationsleiste am oberen")](case-study-tasky-images/wp-taskylist.png#lightbox) [![](case-study-tasky-images/wp-taskylist.png "verwendet den Standard, der Windows Phone-app Layout die app-Leiste am unteren Rand des Bildschirms, anstatt eine Navigationsleiste am oberen Auffüllen")](case-study-tasky-images/wp-taskylist.png#lightbox)

 <a name="Summary" />

## <a name="summary"></a>Zusammenfassung

Dieses Dokument stellt eine ausführliche Erläuterung der, wie die Prinzipien des mehrstufigen Anwendungsdesign, eine einfache Anwendung zum Wiederverwendungsrate für Code, die auf drei mobilen Plattformen zu ermöglichen angewendet wurden: iOS, Android und Windows Phone.

Weist die Vorgehensweise zum Entwerfen Sie der Anwendungsschichten beschrieben und erläutert, welchen Code &amp; Funktionen in jeder Ebene implementiert wurde.

Der Code kann heruntergeladen werden [Github](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable).

## <a name="related-links"></a>Verwandte Links

- [Building Cross-Platform Applications (wichtigsten Dokument)](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [Tasky Portable-Beispiel-App (Github)](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)
