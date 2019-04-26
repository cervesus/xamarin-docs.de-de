---
title: Einführung in Portable Klassenbibliotheken (PCL)
description: Dieser Artikel führt Portable Klassenbibliothek (PCL) Projekte und führt durch die Erstellung und Nutzung von PCL-Projekte in Visual Studio für Mac und Visual Studio.
ms.prod: xamarin
ms.assetid: 76ba8f7a-9b6e-40f5-9a29-ff1274ece4f2
author: conceptdev
ms.author: crdun
ms.date: 07/18/2018
ms.openlocfilehash: 221ee49e282b3b038d03f659d238336710283a66
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61230035"
---
# <a name="portable-class-libraries-pcl"></a>Portable Klassenbibliotheken

> [!TIP]
> Portable Klassenbibliotheken (PCLs) gelten, in die neuesten Versionen von Visual Studio als veraltet markiert.
> Während Sie können immer noch öffnen, bearbeiten und Kompilieren von PCLs, für neue Projekte es wird empfohlen, verwenden Sie [.NET Standard-Bibliotheken](~/cross-platform/app-fundamentals/net-standard.md) auf eine größere API-Oberfläche zugreifen.

Ein wichtiger Bestandteil beim Erstellen von plattformübergreifenden Anwendungen wird in der Lage, Code in verschiedenen plattformspezifischen Projekten gemeinsam nutzen. Dies ist jedoch durch die Tatsache komplizierter, unterschiedliche Plattformen verwenden häufig eine andere untergeordnete Gruppe von der .NET Basisklassenbibliothek (BCL), und aus diesem Grund werden tatsächlich auf ein anderes .NET Core-Bibliothek-Profil erstellt. Dies bedeutet, dass jede Plattform nur Klassenbibliotheken verwenden kann, die für das gleiche Profil vorgesehen sind, damit sie angezeigt werden sollen, um separate Klassenbibliothekprojekte für jede Plattform erfordern.

Es gibt drei wesentliche Ansätze zur gemeinsamen Nutzung von Code, der dieses Problem zu beheben: **.NET Standard-Projekte**, **Projekten mit freigegebenen Asset**, und **Portable Klassenbibliothek (PCL) Projekte**.

- **.NET standard-Projekte** sind der bevorzugte Ansatz für die gemeinsame Nutzung von .NET Code und erfahren Sie mehr über [.NET Standard-Projekte und Xamarin](~/cross-platform/app-fundamentals/net-standard.md).
- **Freigegebene Asset Projekte** verwendet einen einzelnen Satz von Dateien und bietet eine schnelle und einfache Art, wie zum Freigeben von Code in einer Projektmappe und im Allgemeinen werden Bedingte Kompilierungsdirektiven Codepfade für die verschiedenen Plattformen angeben, die sie (Weitere Informationen verwenden Informationen finden Sie unter den [freigegebene Projekte Artikel](~/cross-platform/app-fundamentals/shared-projects.md)).
- **PCL** -Projekte bestimmte Profile, die einen bekannten Satz von BCL-Klassen und-Funktionen unterstützen. SaveSetting zu PCL ist jedoch, dass sie oft zusätzliche architektonische Aufwand zum Aufteilen von Profile für bestimmten Code in ihre eigenen Bibliotheken erfordern.

Auf dieser Seite wird erläutert, wie zum Erstellen einer **PCL** Projekt, ein bestimmtes Profil aus, die dann von mehreren plattformspezifischen Projekten verwiesen werden kann.

## <a name="what-is-a-portable-class-library"></a>Was ist eine Portable Klassenbibliothek?

Wenn Sie ein Projekt oder ein Steuerelementbibliothek-Projekt erstellen, ist die resultierende DLL beschränkt, an die jeweilige Plattform, die für die Erstellung zu arbeiten. Dies verhindert, dass Sie von einer Assembly für eine Windows-app schreiben, und es anschließend für Xamarin.iOS und Xamarin.Android erneut verwenden.

Wenn Sie eine Portable Klassenbibliothek erstellen, können Sie jedoch eine Kombination aus Plattformen auswählen, die Ihren Code ausgeführt werden sollen. Die Anwendungskompatibilitäts-Optionen, die Sie vornehmen, wenn Sie eine Portable Klassenbibliothek erstellen werden in einem Bezeichner "Profile" übersetzt, die beschreibt, welche Plattformen die Bibliothek unterstützt.

Die folgende Tabelle zeigt einige der Features, die von .NET Plattform unterschiedlich sind. Um eine PCL-Assembly zu schreiben, die garantiert auf bestimmten Geräten/Plattformen ausgeführt wird, wählen Sie einfach die Unterstützung bei der Erstellung des Projekts erforderlich ist.

|Feature|.NET Framework|UWP-Apps|Silverlight|Windows Phone|Xamarin|
|---|---|---|---|---|---|
|Kernspeicher|J|J|J|J|J|
|LINQ|J|J|J|J|J|
|IQueryable|J|J|J|7.5 +|J|
|Serialisierung|J|J|J|J|J|
|Datenanmerkungen|4.0.3 +|J|J||J|

Die Xamarin-Spalte gibt die Tatsache, dass Xamarin.iOS und Xamarin.Android unterstützt die Profile, die für die im Lieferumfang von Visual Studio und die Verfügbarkeit von Funktionen in Bibliotheken, die Sie erstellen werden nur von anderen Plattformen, die Sie unterstützen auch beschränkt werden.

Dies umfasst Profile, die Kombinationen aus:

- .NET 4 oder .NET 4.5
- Silverlight 5
- Windows Phone 8
- UWP-Apps

Erfahren Sie mehr über die verschiedenen Profile-Funktionen auf [Microsoft Website](https://msdn.microsoft.com/library/gg597391(v=vs.110).aspx) und finden Sie unter einem anderen Communitymitglied [PCL-Profil Zusammenfassung](http://embed.plnkr.co/03ck2dCtnJogBKHJ9EjY) darunter Framework Informationen und andere Hinweise unterstützt.

**Vorteile**

1. Zentralisierte Codefreigabe – schreiben und Testen von Code in einem einzelnen Projekt, das von anderen Bibliotheken oder Anwendungen genutzt werden kann.
2. Vorgänge werden umgestaltet wirkt sich der gesamte Code in der Projektmappe (der portablen Klassenbibliothek und den plattformspezifischen Projekten) geladen.
3. PCL-Projekt kann leicht von anderen Projekten in einer Projektmappe verwiesen werden, oder die Ausgabeassembly kann gemeinsam genutzt werden, damit andere Sie verweisen in ihren Lösungen.

**Nachteile**

1. Da der gleiche portablen Klassenbibliothek zwischen mehreren Anwendungen gemeinsam genutzt wird, können nicht plattformspezifische Bibliotheken (z. b. verwiesen werden Community.CsharpSqlite.WP7).
2. Die Portable Class Library-Teilmenge kann keine Klassen enthalten, die andernfalls in MonoTouch und Mono für Android (z. B. "DllImport" oder "System.IO.File") verfügbar.

> [!NOTE]
> Portable Klassenbibliotheken sind in der neuesten Version von Visual Studio veraltet und [.NET Standard-Bibliotheken](net-standard.md) stattdessen empfohlen werden.

Zu einem gewissen Grad können beide Nachteile umgangen werden mithilfe der anbietermuster oder Dependency Injection, um die tatsächliche Implementierung in den Plattform-Projekten für eine Schnittstelle oder Base-Klasse zu codieren, die in der portablen Klassenbibliothek definiert ist.

Dieses Diagramm zeigt die Architektur einer plattformübergreifenden Anwendung mithilfe einer portablen Klassenbibliothek gemeinsamen Code nutzen, aber auch mithilfe der Abhängigkeitsinjektion plattformabhängige Funktionen übergeben:

[![](pcl-images/image1.png "Dieses Diagramm zeigt die Architektur einer plattformübergreifenden Anwendung mithilfe einer portablen Klassenbibliothek gemeinsamen Code nutzen, aber auch mithilfe der Abhängigkeitsinjektion plattformabhängige Funktionen übergeben")](pcl-images/image1.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

## <a name="visual-studio-for-mac-walkthrough"></a>Visual Studio für Mac walkthrough

In diesem Abschnitt führt Sie durch die Schritte zum Erstellen und Verwenden einer portablen Klassenbibliothek mit Visual Studio für Mac. Finden Sie unter dem Abschnitt von PCL-Beispiel für eine vollständige Implementierung.

### <a name="creating-a-pcl"></a>Erstellen eine PCL

Der Projektmappe eine Portable Klassenbibliothek hinzufügen ähnelt dem Hinzufügen eines regulären Library-Projekts.

1. In der **neues Projekt** Dialogfeld die **Multi-Plattform > Bibliothek > Portable Library** Option:

    ![Erstellen Sie ein neues PCL-Projekt](pcl-images/image2.png)

1. Beim Erstellen eine PCL in Visual Studio für Mac wird er automatisch mit einem Profil konfiguriert, die für Xamarin.iOS und Xamarin.Android funktioniert. PCL-Projekt wird angezeigt, wie im folgenden Screenshot gezeigt:

    ![PCL-Projekt im projektmappenpad](pcl-images/image3.png)

Die PCL ist nun bereit für Code hinzugefügt werden. Es kann auch von anderen Projekten (Anwendungsprojekte, Bibliotheksprojekte und auch andere PCL-Projekte) verwiesen werden.

### <a name="editing-pcl-settings"></a>PCL-Einstellungen bearbeiten

Klicken Sie zum Anzeigen und ändern Sie die PCL-Einstellungen für dieses Projekt, mit der rechten Maustaste in des Projekts, und wählen Sie **Optionen > Erstellen > Allgemein** , der hier gezeigten Bildschirm angezeigt:

[![PCL-Projekt-Optionen, um das Profil festzulegen](pcl-images/image4-sml.png)](pcl-images/image4.png#lightbox)

Klicken Sie auf **ändern...**  , das Zielprofil für diese portable Klassenbibliothek zu ändern.

Wenn das Profil geändert wird, nachdem der Code bereits zu der PCL hinzugefügt wurde, ist es möglich, dass die Bibliothek nicht mehr kompiliert wird, wenn der Code verweist auf Funktionen, die nicht Teil des neu ausgewählten Profils sind.

## <a name="working-with-a-pcl"></a>Arbeiten mit einer PCL

Wenn Code in einer PCL-Bibliothek geschrieben wird, die Visual Studio für Mac-Editor erkennt die Einschränkungen des ausgewählten Profils und Optionen für die automatische Vervollständigung entsprechend anpassen. In diesem Screenshot zeigt beispielsweise die AutoVervollständigen-Optionen für die System.IO mithilfe des Standard-Profils (Profile136) in Visual Studio für Mac verwendet – Beachten Sie, dass die Bildlaufleiste, womit ungefähr die Hälfte der verfügbaren Klassen angezeigt werden (es sind sogar nur 14 Klassen zur Verfügung).

[![IntelliSense-Liste von 14 Klassen in der System.IO-Klasse ein PCL](pcl-images/image6.png)](pcl-images/image6.png#lightbox)

Vergleichen mit der automatischen Vervollständigung in einem Xamarin.iOS oder Xamarin.Android-Projekt – System.IO stehen 40 Klassen zur Verfügung einschließlich häufig Klassen wie verwendeten `File` und `Directory` die befinden sich nicht in einem PCL-Profil.

[![IntelliSense-Liste von 40 Klassen in .NET Framework System.IO-namespace](pcl-images/image7.png)](pcl-images/image7.png#lightbox)

Dies entspricht der zugrunde liegenden Nachteil der Verwendung von PCL – die Möglichkeit zum Teilen von Code nahtlos auf vielen Plattformen bedeutet, dass bestimmte APIs nicht für Sie verfügbar sind, da sie nicht vergleichbare Implementierungen für alle möglichen Plattformen.

### <a name="using-pcl"></a>Mithilfe von PCL

Nachdem ein PCL-Projekt erstellt wurde, können Sie einen Verweis darauf aus einem beliebigen kompatible Anwendung oder Bibliothek-Projekt auf die gleiche Weise hinzufügen, die Sie normalerweise Verweise hinzufügen. Klicken Sie in Visual Studio für Mac mit der rechten Maustaste auf den Knoten "Verweise", und wählen Sie **Verweise bearbeiten...**  wechseln Sie zu der **Projekte** Registerkarte wie gezeigt:

[![Hinzufügen eines Verweises auf eine PCL über die Option "Verweise bearbeiten"](pcl-images/image8.png)](pcl-images/image8.png#lightbox)

Der folgende Screenshot zeigt Pad "Projektmappe" für die TaskyPortable Beispiel-app mit der PCL-Bibliothek unter dem unteren und einen Verweis auf die PCL-Bibliothek im Xamarin.iOS-Projekt.

[![TaskyPortable Beispiel Lösung mit PCL-Projekt](pcl-images/image9.png)](pcl-images/image9.png#lightbox)

Die Ausgabe aus einer PCL-(d. h. die resultierende Assembly-DLL) kann auch als Verweis auf den meisten Projekten hinzugefügt werden. Dadurch PCL eine ideale Möglichkeit für den Versand von Cross-Platform-Komponenten und Bibliotheken.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="visual-studio-walkthrough"></a>Visual Studio: Exemplarische Vorgehensweise

In diesem Abschnitt erläutert, wie zum Erstellen und Verwenden einer portablen Klassenbibliothek mit Visual Studio. Finden Sie unter dem Abschnitt von PCL-Beispiel für eine vollständige Implementierung.

### <a name="creating-a-pcl"></a>Erstellen eine PCL

Hinzufügen einer PCL für Ihre Lösung in Visual Studio ist ein Unterschied zu ein regulären-Projekt hinzufügen:

1. In der **neues Projekt hinzufügen** auf die **Class-Bibliothek (Legacy-portabel)** Option. Beachten Sie, dass die Beschreibung auf der rechten Seite weist an, der dieser Projekttyp wurde als veraltet markiert.

    [![Fenster für neues Projekt zum Erstellen einer portablen Klassenbibliothek](pcl-images/image10-sml.png "Portable Class Library")](pcl-images/image10.png#lightbox)

2. Visual Studio fordert das folgende Dialogfeld sofort auf, damit das Profil konfiguriert werden kann.
 Aktivieren Sie die Plattformen, die Sie unterstützen möchten und klicken auf OK.

    [![Auswählen der Zielplattformen für die Bibliothek](pcl-images/image11-sml.png "aktivieren Sie die Plattformen, die Sie unterstützen möchten und klicken auf OK")](pcl-images/image11.png#lightbox)

3. PCL-Projekt wird angezeigt, wie im Projektmappen-Explorer dargestellt &ndash; Text **(portabel)** neben den Projektnamen, um anzugeben, dass es eine PCL angezeigt:

    ![.NET Framework definiert wird, indem Sie dem PCL-Profil](pcl-images/image12.png ".NET Framework definiert wird, indem Sie dem PCL-Profil")

Die PCL ist nun bereit für Code hinzugefügt werden. Es kann auch von anderen Projekten (Anwendungsprojekte Bibliotheksprojekte und auch andere PCL-Projekte) verwiesen werden.

### <a name="editing-pcl-settings"></a>PCL-Einstellungen bearbeiten

Die PCL-Einstellungen angezeigt und geändert werden, indem mit der rechten Maustaste auf das Projekt und können **Eigenschaften > Bibliothek** , wie im folgenden Screenshot gezeigt:

[![Bearbeiten Sie die Zielplattform](pcl-images/image13-sml.png)](pcl-images/image13.png#lightbox)

Wenn das Profil geändert wird, nachdem der Code bereits zu der PCL hinzugefügt wurde, ist es möglich, dass die Bibliothek nicht mehr kompiliert wird, wenn der Code verweist auf Funktionen, die nicht Teil des neu ausgewählten Profils sind.

> [!TIP]
> Es gibt auch eine Nachricht, die darüber informiert **. NETStandard ist die empfohlene Methode zur Freigabe von Code**. Dies ist ein Hinweis darauf, die beim PCLs weiterhin unterstützt werden, es wird empfohlen, die auf .NET Standard zu aktualisieren.

### <a name="working-with-a-pcl"></a>Arbeiten mit einer PCL

Wenn Code in einer PCL-Bibliothek geschrieben wird, Visual Studio erkennt die Einschränkungen des ausgewählten Profils und Intellisense-Optionen entsprechend anpassen. Angenommen, dieser Screenshot zeigt die AutoVervollständigen-Optionen für System.IO mithilfe des Standardprofils (Profile136) – Beachten Sie, dass die Bildlaufleiste, womit ungefähr die Hälfte der verfügbaren Klassen angezeigt werden (in der Tat gibt es nur 14 Klassen verfügbar sind).

[![Reduzierte Anzahl von e/a-Klassen, die in einer PCL verfügbar sind.](pcl-images/image14.png)](pcl-images/image14.png#lightbox)

Vergleichen mit AutoVervollständigen in einem regulären-Projekt – System.IO stehen 40 Klassen zur Verfügung einschließlich häufig Klassen wie verwendeten `File` und `Directory` die befinden sich nicht in einem PCL-Profil.

[![Viele weitere e/a-Klassen in .NET Framework verfügbar.](pcl-images/image15.png)](pcl-images/image15.png#lightbox)

Dies entspricht der zugrunde liegenden Nachteil der Verwendung von PCL – die Möglichkeit zum Teilen von Code nahtlos auf vielen Plattformen bedeutet, dass bestimmte APIs nicht für Sie verfügbar sind, da sie nicht vergleichbare Implementierungen für alle möglichen Plattformen.

> [!TIP]
> .NET standard 2.0 stellt eine viel größere API-Oberfläche als portable Klassenbibliotheken, einschließlich des System.IO-Namespaces dar. Bei neuen Projekten wird .NET Standard über PCL empfohlen.

### <a name="using-pcl"></a>Mithilfe von PCL

Nachdem ein PCL-Projekt erstellt wurde, können Sie einen Verweis darauf aus einem beliebigen kompatible Anwendung oder Bibliothek-Projekt auf die gleiche Weise hinzufügen, die Sie normalerweise Verweise hinzufügen. Klicken Sie in Visual Studio mit der rechten Maustaste auf den Knoten "Verweise", und wählen Sie `Add Reference...` wechseln Sie zu der **Lösung > Projekte** Registerkarte wie gezeigt:

[![Hinzufügen eines Verweises auf eine PCL über die Registerkarte "Verweis-Projekte hinzufügen"](pcl-images/image16.png)](pcl-images/image16.png#lightbox)

Der folgende Screenshot zeigt die im Bereich der Lösung für die TaskyPortable Beispiel-app mit der PCL-Bibliothek unter dem unteren und einen Verweis auf die PCL-Bibliothek im Xamarin.iOS-Projekt.

[![TaskyPortable-beispiellösung mit einer PCL-Bibliothek](pcl-images/image17.png)](pcl-images/image17.png#lightbox)

Die Ausgabe aus einer PCL-(d. h. die resultierende Assembly-DLL) kann auch als Verweis auf den meisten Projekten hinzugefügt werden.
Dadurch PCL eine ideale Möglichkeit für den Versand von Cross-Platform-Komponenten und Bibliotheken.

-----

## <a name="pcl-example"></a>PCL-Beispiel

Die [TaskyPortable](https://developer.xamarin.com/samples/mobile/TaskyPortable/) beispielanwendung für veranschaulicht, wie eine Portable Klassenbibliothek mit Xamarin verwendet werden können.
Hier sind einige Screenshots der resultierenden unter iOS und Android-apps:

[![](pcl-images/image18.png "Hier sind einige Screenshots der resultierenden unter iOS, Android und Windows Phone-apps")](pcl-images/image18.png#lightbox)

Er teilt sich eine Reihe von Daten und Logik Klassen, die ausschließlich übertragbarem Code sind, und außerdem wird veranschaulicht, wie bestimmte Anforderungen, die mithilfe der Abhängigkeitsinjektion für die Implementierung der SQLite-Datenbank zu integrieren.

Die Lösungsstruktur wird unten gezeigt (in Visual Studio für Mac und Visual Studio entsprechend):

[![](pcl-images/image19.png "Die Projektmappenstruktur in Visual Studio für Mac und Visual Studio bzw. sehen Sie hier")](pcl-images/image19.png#lightbox)

Da der SQLite-NET-Code plattformspezifische Teilen (um die SQLite-Implementierungen für jedes Betriebssystem funktionieren) verfügt, für die Demo wurde es in einer abstrakten umgestaltet Zwecke Klasse, die in einer portablen Klassenbibliothek kompiliert werden kann und der eigentliche Code, der als Unterklassen in den IOS- und Android-Projekte implementiert.

### <a name="taskyportablelibrary"></a>TaskyPortableLibrary

Die Features von .NET ist der portablen Klassenbibliothek beschränkt, die dies unterstützen. Da sie für die Ausführung auf mehreren Plattformen kompiliert wird, können nicht dafür nutzen `[DllImport]` Funktionen, die in SQLite-NET-verwendet wird. SQLite-NET ist stattdessen als eine abstrakte Klasse implementiert, und klicken Sie dann die restlichen den freigegebenen Code verwiesen wird. Ein Auszug aus der abstrakten-API ist unten dargestellt:

```csharp
public abstract class SQLiteConnection : IDisposable {

    public string DatabasePath { get; private set; }
    public bool TimeExecution { get; set; }
    public bool Trace { get; set; }
    public SQLiteConnection(string databasePath) {
         DatabasePath = databasePath;
    }
    public abstract int CreateTable<T>();
    public abstract SQLiteCommand CreateCommand(string cmdText, params object[] ps);
    public abstract int Execute(string query, params object[] args);
    public abstract List<T> Query<T>(string query, params object[] args) where T : new();
    public abstract TableQuery<T> Table<T>() where T : new();
    public abstract T Get<T>(object pk) where T : new();
    public bool IsInTransaction { get; protected set; }
    public abstract void BeginTransaction();
    public abstract void Rollback();
    public abstract void Commit();
    public abstract void RunInTransaction(Action action);
    public abstract int Insert(object obj);
    public abstract int Update(object obj);
    public abstract int Delete<T>(T obj);

    public void Dispose()
    {
        Close();
    }
    public abstract void Close();

}
```

Der Rest der freigegebene Code verwendet die abstrakte Klasse, um "Speichern" und "abrufen" Objekte aus der Datenbank. In Anwendungen, die dieser abstrakten Klasse verwenden müssen wir eine vollständige Implementierung übergeben, die die aktuelle Datenbank-Funktionen bereitstellt.

### <a name="taskyandroid-and-taskyios"></a>TaskyAndroid und TaskyiOS

Die IOS- und Android-Anwendungsprojekten enthalten der Benutzeroberfläche und anderen plattformspezifischen Code verwendet, um den freigegebenen Code in der PCL anschließen.

Diese Projekte enthalten auch eine Implementierung der abstrakten Datenbank-API, die auf dieser Plattform funktioniert. Unter iOS und Android die Sqlite-Datenbank-Engine ist in das Betriebssystem integriert, damit die Implementierung zu verwenden, kann `[DllImport]` wie gezeigt, um die konkrete Implementierung der Verbindung mit der Datenbank bereitzustellen. Ein Auszug des Codes plattformspezifische Implementierung ist hier dargestellt:

```csharp
[DllImport("sqlite3", EntryPoint = "sqlite3_open")]
public static extern Result Open(string filename, out IntPtr db);

[DllImport("sqlite3", EntryPoint = "sqlite3_close")]
public static extern Result Close(IntPtr db);
```

Die vollständige Implementierung kann im Code angezeigt werden.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel kurz erläutert die vor- und Nachteile des Portable Class Libraries, veranschaulicht, wie das Erstellen und Nutzen von PCLs von innerhalb von Visual Studio für Mac und Visual Studio an; und schließlich eingeführt, eine vollständige beispielanwendung – TaskyPortable –, die eine PCL in Aktion anzeigt.

## <a name="related-links"></a>Verwandte Links

- [TaskyPortable (Beispiel)](https://developer.xamarin.com/samples/mobile/TaskyPortable/)
- [Erstellen von plattformübergreifenden Anwendungen](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [Portable Visual Basic](~/cross-platform/platform/visual-basic/index.md)
- [Freigegebene Projekte](~/cross-platform/app-fundamentals/shared-projects.md)
- [Optionen für die Codefreigabe](~/cross-platform/app-fundamentals/code-sharing.md)
- [Plattformübergreifende Entwicklung mit .NET Framework (Microsoft)](https://docs.microsoft.com/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library)
