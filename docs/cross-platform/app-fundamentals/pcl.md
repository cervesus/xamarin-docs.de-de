---
title: Einführung in Portable Klassenbibliotheken (PCL)
description: In diesem Artikel werden Projekte für Portable Klassenbibliotheken (Portable Class Library, PCL) vorgestellt, und es wird erläutert, wie Sie PCL-Projekte in Visual Studio für Mac und Visual Studio
ms.prod: xamarin
ms.assetid: 76ba8f7a-9b6e-40f5-9a29-ff1274ece4f2
author: conceptdev
ms.author: crdun
ms.date: 07/18/2018
ms.openlocfilehash: a4ee81f7d59c9fb680dfd371a7aaba7660fb3343
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "68681071"
---
# <a name="portable-class-libraries-pcl"></a>Portable Klassenbibliotheken

> [!TIP]
> Portable Klassenbibliotheken (Portable Class Libraries, pcls) werden in den neuesten Versionen von Visual Studio als veraltet eingestuft.
> Obwohl Sie weiterhin pcls öffnen, bearbeiten und kompilieren können, empfiehlt es sich, für neue Projekte .NET Standard- [Bibliotheken](~/cross-platform/app-fundamentals/net-standard.md) für den Zugriff auf eine größere API-Oberfläche zu verwenden.

Eine wichtige Komponente bei der Bildung plattformübergreifender Anwendungen ist die Möglichkeit, Code für verschiedene plattformspezifische Projekte freizugeben. Dies wird jedoch durch die Tatsache erschwert, dass unterschiedliche Plattformen häufig eine andere Teilmenge der .net-Basisklassen Bibliothek (Base Class Library, BCL) verwenden und daher tatsächlich in einem anderen .net Core-Bibliotheks Profil erstellt werden. Dies bedeutet, dass jede Plattform nur Klassenbibliotheken verwenden kann, die auf dasselbe Profil abzielen, sodass Sie für jede Plattform separate Klassen Bibliotheks Projekte erfordern.

Es gibt drei Hauptansätze für die Code Freigabe, die dieses Problem beheben: **.NET Standard-Projekte**, frei **gegebene assetprojekte**und **PCL-Projekte (Portable Class Library)** .

- **.NET Standard Projekte** sind der bevorzugte Ansatz für die gemeinsame Nutzung von .NET-Code. erfahren Sie mehr über [.NET Standard Projekte und xamarin](~/cross-platform/app-fundamentals/net-standard.md).
- Frei **gegebene assetprojekte** verwenden einen einzelnen Satz von Dateien und bieten eine schnelle und einfache Möglichkeit, Code in einer Projekt Mappe gemeinsam zu nutzen und in der Regel bedingte Kompilierungs Direktiven zum Angeben von Codepfade für verschiedene Plattformen zu verwenden, von denen Sie verwendet wird (für weitere Informationen finden Sie im Artikel zu frei [gegebenen Projekten](~/cross-platform/app-fundamentals/shared-projects.md).
- **PCL** -Projekte richten sich an bestimmte Profile, die einen bekannten Satz von BCL-Klassen/-Features unterstützen. Die Seite "nach unten" zu "PCL" besteht jedoch darin, dass Sie häufig einen zusätzlichen architektonischen Aufwand erfordern, um Profil spezifischen Code in seine eigenen Bibliotheken aufzuteilen.

Auf dieser Seite wird erläutert, wie ein **PCL** -Projekt erstellt wird, das auf ein bestimmtes Profil abzielt, auf das von mehreren plattformspezifischen Projekten verwiesen werden kann.

## <a name="what-is-a-portable-class-library"></a>Was ist eine portable Klassenbibliothek?

Wenn Sie ein Anwendungsprojekt oder ein Bibliotheksprojekt erstellen, ist die resultierende DLL auf die jeweilige Plattform beschränkt, für die Sie erstellt wurde. Dadurch wird verhindert, dass Sie eine Assembly für eine Windows-app schreiben und dann in xamarin. IOS und xamarin. Android wieder verwenden.

Wenn Sie jedoch eine portable Klassenbibliothek erstellen, können Sie eine Kombination von Plattformen auswählen, auf denen der Code ausgeführt werden soll. Die Kompatibilitäts Entscheidungen, die Sie beim Erstellen einer portablen Klassenbibliothek treffen, werden in einen "Profil Bezeichner" übersetzt, der beschreibt, welche Plattformen die Bibliothek unterstützt.

In der folgenden Tabelle sind einige der Features aufgeführt, die von der .NET-Plattform abweichen. Zum Schreiben einer PCL-Assembly, die garantiert auf bestimmten Geräten/Plattformen ausgeführt werden kann, wählen Sie einfach aus, welche Unterstützung beim Erstellen des Projekts erforderlich ist.

|Feature|.NET Framework|UWP-Apps|Silverlight|Windows Phone|Xamarin|
|---|---|---|---|---|---|
|Kernspeicher|Y|Y|Y|Y|Y|
|LINQ|Y|Y|Y|Y|Y|
|IQueryable|Y|Y|Y|7,5 und höher|Y|
|Serialisierung|Y|Y|Y|Y|Y|
|Datenanmerkungen|4.0.3 +|Y|Y||Y|

Die xamarin-Spalte spiegelt die Tatsache wider, dass xamarin. IOS und xamarin. Android alle Profile unterstützt, die in Visual Studio enthalten sind. die Verfügbarkeit von Features in den von Ihnen erstellten Bibliotheken wird nur von den anderen Plattformen eingeschränkt, die Sie unterstützen möchten.

Dies schließt Profile ein, die eine Kombination aus:

- .NET 4 oder .NET 4,5
- Silverlight 5
- Windows Phone 8
- UWP-Apps

Weitere Informationen zu den Funktionen der verschiedenen Profile finden Sie auf der [Microsoft-Website](https://msdn.microsoft.com/library/gg597391(v=vs.110).aspx) , und sehen Sie sich die [PCL-Profil Zusammenfassung](http://embed.plnkr.co/03ck2dCtnJogBKHJ9EjY) eines anderen Community-Members an, die unterstützte frameworkinformationen und andere Hinweise enthält

**Vorteile**

1. Zentralisierte Code Freigabe – schreiben und Testen von Code in einem einzelnen Projekt, das von anderen Bibliotheken oder Anwendungen verwendet werden kann.
2. Refactoringvorgänge wirken sich auf den gesamten Code aus, der in der Projekt Mappe geladen wird (die portable Klassenbibliothek und die plattformspezifischen Projekte).
3. Das PCL-Projekt kann problemlos von anderen Projekten in einer Projekt Mappe referenziert werden, oder die Ausgabeassembly kann für andere Benutzer freigegeben werden.

**Nachteile**

1. Da dieselbe portable Klassenbibliothek von mehreren Anwendungen gemeinsam genutzt wird, kann auf plattformspezifische Bibliotheken nicht verwiesen werden (z. b. Community. csharpsqlite. WP7).
2. Die Untermenge der portablen Klassenbibliothek darf keine Klassen enthalten, die andernfalls sowohl in MonoTouch als auch Mono für Android (z. b. DllImport oder System. IO. File) verfügbar wären.

> [!NOTE]
> Portable Klassenbibliotheken sind in der neuesten Version von Visual Studio veraltet, und es werden stattdessen [.NET Standard Bibliotheken](net-standard.md) empfohlen.

In gewissem Maße können beide Nachteile mithilfe des Anbieter Musters oder der Abhängigkeitsinjektion umgangen werden, um die tatsächliche Implementierung in den Platt Form Projekten mit einer Schnittstelle oder Basisklasse zu codieren, die in der portablen Klassenbibliothek definiert ist.

Dieses Diagramm zeigt die Architektur einer plattformübergreifenden Anwendung mithilfe einer portablen Klassenbibliothek für die gemeinsame Nutzung von Code, aber auch die Verwendung von Abhängigkeitsinjektion, um Platt Form abhängige Features zu übergeben:

[![](pcl-images/image1.png "This diagram shows the architecture of a cross-platform application using a Portable Class Library to share code, but also using Dependency Injection to pass in platform-dependent features")](pcl-images/image1.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

## <a name="visual-studio-for-mac-walkthrough"></a>Exemplarische Vorgehensweise Visual Studio für Mac

In diesem Abschnitt wird das Erstellen und Verwenden einer portablen Klassenbibliothek mithilfe von Visual Studio für Mac erläutert. Eine komplette Implementierung finden Sie im Abschnitt zu PCL-Beispiel.

### <a name="creating-a-pcl"></a>Erstellen einer PCL

Das Hinzufügen einer portablen Klassenbibliothek zu ihrer Projekt Mappe ähnelt dem Hinzufügen eines regulären Bibliotheks Projekts.

1. Wählen Sie im Dialogfeld **Neues Projekt** die Option **Multiplattform-> Bibliothek > portablen Bibliothek** aus:

    ![Erstellen eines neuen PCL-Projekts](pcl-images/image2.png)

1. Wenn eine PCL in Visual Studio für Mac erstellt wird, wird Sie automatisch mit einem Profil konfiguriert, das für xamarin. IOS und xamarin. Android funktioniert. Das PCL-Projekt wird wie im folgenden Screenshot gezeigt angezeigt:

    ![PCL-Projekt im lösungspad](pcl-images/image3.png)

Die PCL ist nun zum Hinzufügen von Code bereit. Es kann auch von anderen Projekten (Anwendungsprojekte, Bibliotheks Projekte und sogar anderen PCL-Projekten) auf Sie verwiesen werden.

### <a name="editing-pcl-settings"></a>Bearbeiten von PCL-Einstellungen

Um die PCL-Einstellungen für dieses Projekt anzuzeigen und zu ändern, klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **Optionen > Build > Allgemein** aus, um den folgenden Bildschirm anzuzeigen:

[![PCL Projektoptionen zum Festlegen des Profils](pcl-images/image4-sml.png)](pcl-images/image4.png#lightbox)

Klicken Sie auf **ändern...** , um das Ziel Profil für diese portable Klassenbibliothek zu ändern.

Wenn das Profil geändert wird, nachdem der Code der PCL bereits hinzugefügt wurde, ist es möglich, dass die Bibliothek nicht mehr kompiliert wird, wenn der Code auf Features verweist, die nicht Teil des neu ausgewählten Profils sind.

## <a name="working-with-a-pcl"></a>Arbeiten mit einer PCL

Wenn Code in einer PCL-Bibliothek geschrieben wird, erkennt der Visual Studio für Mac-Editor die Einschränkungen des ausgewählten Profils und passt die Optionen für die automatische Vervollständigung entsprechend an. Dieser Screenshot zeigt z. b. die Optionen für die automatische Vervollständigung für System.IO mithilfe des Standard Profils (Profile136), das in Visual Studio für Mac verwendet wird – Beachten Sie, dass die Scrollleiste angezeigt wird, die angibt, dass die Hälfte der verfügbaren Klassen angezeigt wird (tatsächlich sind nur 14 Verfügbare Klassen).

[![Intellisense Liste von 14 Klassen in der System.IO-Klasse einer PCL](pcl-images/image6.png)](pcl-images/image6.png#lightbox)

Vergleichen Sie dies mit der automatischen Vervollständigung System.IO in einem xamarin. IOS-oder xamarin. Android-Projekt – es gibt 40-Klassen, einschließlich häufig verwendeter Klassen wie `File` und `Directory` die nicht in einem PCL-Profil vorhanden sind.

[![Intellisense Liste der 40-Klassen in .NET Framework System.IO-Namespace](pcl-images/image7.png)](pcl-images/image7.png#lightbox)

Dies spiegelt den zugrunde liegenden Kompromiss der Verwendung von PCL wider – die Möglichkeit, Code nahtlos auf vielen Plattformen freizugeben, bedeutet, dass bestimmte APIs nicht verfügbar sind, da Sie nicht über vergleichbare Implementierungen auf allen möglichen Plattformen verfügen.

### <a name="using-pcl"></a>Verwenden von PCL

Nachdem ein PCL-Projekt erstellt wurde, können Sie von einem beliebigen kompatiblen Anwendungs-oder Bibliotheksprojekt aus einen Verweis darauf hinzufügen, wie Sie normalerweise Verweise hinzufügen. Klicken Sie in Visual Studio für Mac mit der rechten Maustaste auf den Knoten Verweise, und wählen Sie **Verweise bearbeiten...** aus, und wechseln Sie dann zur Registerkarte **Projekte** .

[![Add eines Verweises auf eine PCL mithilfe der Option "Verweise bearbeiten"](pcl-images/image8.png)](pcl-images/image8.png#lightbox)

Der folgende Screenshot zeigt den lösungspad für die taskyportable-Beispiel-APP, die die PCL-Bibliothek unten anzeigt, und einen Verweis auf die PCL-Bibliothek im xamarin. IOS-Projekt.

[![TaskyPortable Beispiel Projekt Mappe, die das PCL-Projekt anzeigt](pcl-images/image9.png)](pcl-images/image9.png#lightbox)

Die Ausgabe einer PCL (d.h. die resultierende Assembly-DLL) kann auch als Verweis auf die meisten Projekte hinzugefügt werden. Dadurch ist PCL eine ideale Möglichkeit, plattformübergreifende Komponenten und Bibliotheken zu liefern.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="visual-studio-walkthrough"></a>Exemplarische Vorgehensweise zu Visual Studio

In diesem Abschnitt wird erläutert, wie Sie eine portable Klassenbibliothek mithilfe von Visual Studio erstellen und verwenden. Eine komplette Implementierung finden Sie im Abschnitt zu PCL-Beispiel.

### <a name="creating-a-pcl"></a>Erstellen einer PCL

Das Hinzufügen einer PCL zu ihrer Projekt Mappe in Visual Studio unterscheidet sich geringfügig vom Hinzufügen eines regulären Projekts:

1. Wählen Sie auf dem Bildschirm **Neues Projekt hinzufügen** die Option **Klassenbibliothek (vorportabel)** aus. Hinweis die Beschreibung auf der rechten Seite weist darauf hin, dass dieser Projekttyp veraltet ist.

    [![Neues Projektfenster zum Erstellen einer portablen Klassenbibliothek](pcl-images/image10-sml.png "Portable Klassenbibliothek")](pcl-images/image10.png#lightbox)

2. Visual Studio wird sofort mit dem folgenden Dialogfeld aufgefordert, damit das Profil konfiguriert werden kann.
 Wählen Sie die Plattformen, die Sie unterstützen müssen, und drücken Sie OK

    [![Wählen Sie die Zielplattformen für die Bibliothek aus.](pcl-images/image11-sml.png "Über die Plattformen, die Sie unterstützen müssen, und OK")](pcl-images/image11.png#lightbox)

3. Das PCL-Projekt wird wie in der Projektmappen-Explorer angezeigt &ndash; der Text **(portabel)** neben dem Projektnamen angezeigt wird, um anzugeben, dass es sich um eine PCL handelt:

    ![Durch das PCL-Profil definiertes .NET Framework](pcl-images/image12.png "Durch das PCL-Profil definiertes .NET Framework")

Die PCL ist nun zum Hinzufügen von Code bereit. Es kann auch von anderen Projekten (Anwendungsprojekte, Bibliotheks Projekte und sogar anderen PCL-Projekten) auf Sie verwiesen werden.

### <a name="editing-pcl-settings"></a>Bearbeiten von PCL-Einstellungen

Die PCL-Einstellungen können angezeigt und geändert werden, indem Sie mit der rechten Maustaste auf das Projekt klicken und **Eigenschaften > Bibliothek** auswählen, wie in diesem Screenshot gezeigt:

[![Edit der Platt Form Ziele](pcl-images/image13-sml.png)](pcl-images/image13.png#lightbox)

Wenn das Profil geändert wird, nachdem der Code der PCL bereits hinzugefügt wurde, ist es möglich, dass die Bibliothek nicht mehr kompiliert wird, wenn der Code auf Features verweist, die nicht Teil des neu ausgewählten Profils sind.

> [!TIP]
> Es wird auch eine Meldung mit der Meldung angezeigt **. Netstandard ist die empfohlene Methode für die gemeinsame Nutzung von Code**. Dies deutet darauf hin, dass während der weiteren Unterstützung von pcls ein Upgrade auf .NET Standard empfohlen wird.

### <a name="working-with-a-pcl"></a>Arbeiten mit einer PCL

Wenn Code in einer PCL-Bibliothek geschrieben wird, erkennt Visual Studio die Einschränkungen des ausgewählten Profils und passt die IntelliSense-Optionen entsprechend an. Dieser Screenshot zeigt z. b. die Optionen für die automatische Vervollständigung für System.IO unter Verwendung des Standard Profils (Profile136) – beachten Sie, dass die Scrollleiste angezeigt wird, die angibt, dass die Hälfte der verfügbaren Klassen angezeigt wird (tatsächlich sind nur 14 Klassen verfügbar).

[![Reduced Anzahl der in einer PCL verfügbaren e/a-Klassen](pcl-images/image14.png)](pcl-images/image14.png#lightbox)

Vergleichen Sie dies mit der automatischen Vervollständigung System.IO in einem regulären Projekt – es gibt 40-Klassen, einschließlich häufig verwendeter Klassen wie `File` und `Directory` die nicht in einem PCL-Profil vorhanden sind.

[![Many weitere IO-Klassen, die im .NET Framework verfügbar sind](pcl-images/image15.png)](pcl-images/image15.png#lightbox)

Dies spiegelt den zugrunde liegenden Kompromiss der Verwendung von PCL wider – die Möglichkeit, Code nahtlos auf vielen Plattformen freizugeben, bedeutet, dass bestimmte APIs nicht verfügbar sind, da Sie nicht über vergleichbare Implementierungen auf allen möglichen Plattformen verfügen.

> [!TIP]
> .NET Standard 2,0 stellt eine viel größere API-Oberfläche dar als pcls, einschließlich des System.IO-Namespace. Bei neuen Projekten wird .NET Standard über PCL empfohlen.

### <a name="using-pcl"></a>Verwenden von PCL

Nachdem ein PCL-Projekt erstellt wurde, können Sie von einem beliebigen kompatiblen Anwendungs-oder Bibliotheksprojekt aus einen Verweis darauf hinzufügen, wie Sie normalerweise Verweise hinzufügen. Klicken Sie in Visual Studio mit der rechten Maustaste auf den Knoten Verweise, und wählen Sie `Add Reference...` klicken Sie dann auf die Registerkarte **Projekt Mappe > Projekte** , wie hier gezeigt:

[![Add eines Verweises auf eine PCL über die Registerkarte "Verweis Projekte hinzufügen"](pcl-images/image16.png)](pcl-images/image16.png#lightbox)

Der folgende Screenshot zeigt den Lösungs Bereich der taskyportable-Beispiel-APP, die die PCL-Bibliothek unten und einen Verweis auf die PCL-Bibliothek im xamarin. IOS-Projekt anzeigt.

[![TaskyPortable Beispiellösung mit einer PCL-Bibliothek](pcl-images/image17.png)](pcl-images/image17.png#lightbox)

Die Ausgabe einer PCL (d.h. die resultierende Assembly-DLL) kann auch als Verweis auf die meisten Projekte hinzugefügt werden.
Dadurch ist PCL eine ideale Möglichkeit, plattformübergreifende Komponenten und Bibliotheken zu liefern.

-----

## <a name="pcl-example"></a>PCL-Beispiel

Die [taskyportable](https://docs.microsoft.com/samples/xamarin/mobile-samples/taskyportable/) -Beispielanwendung veranschaulicht, wie eine portable Klassenbibliothek mit xamarin verwendet werden kann.
Im folgenden finden Sie einige Screenshots der sich ergebenden apps, die unter IOS und Android ausgeführt werden:

[![](pcl-images/image18.png "Here are some screenshots of the resulting apps running on iOS, Android and Windows Phone")](pcl-images/image18.png#lightbox)

Es verwendet eine Reihe von Daten-und Logik Klassen, die rein portablen Code sind, und zeigt außerdem, wie plattformspezifische Anforderungen mithilfe von Abhängigkeitsinjektion für die SQLite-Datenbankimplementierung integriert werden.

Die Lösungs Struktur ist unten dargestellt (in Visual Studio für Mac bzw. in Visual Studio):

[![](pcl-images/image19.png "The solution structure is shown here in Visual Studio for Mac and Visual Studio respectively")](pcl-images/image19.png#lightbox)

Da der SQLite-NET-Code plattformspezifische Teile aufweist (um mit den SQLite-Implementierungen auf jedem anderen Betriebssystem zu arbeiten), wurde er zu Demonstrationszwecken in eine abstrakte Klasse umgestaltet, die in eine portable Klassenbibliothek kompiliert werden kann. der tatsächliche Code, der als Unterklassen in den IOS-und Android-Projekten implementiert ist.

### <a name="taskyportablelibrary"></a>Taskyportablelibrary

Die portable Klassenbibliothek ist in den .NET-Funktionen eingeschränkt, die Sie unterstützen kann. Da es für die Verwendung auf mehreren Plattformen kompiliert ist, kann es keine `[DllImport]` Funktionen verwenden, die in SQLite-NET verwendet werden. Stattdessen wird SQLite-NET als abstrakte Klasse implementiert, auf die dann über den restlichen freigegebenen Code verwiesen wird. Eine Extraktion der abstrakten API wird unten dargestellt:

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

Der Rest des freigegebenen Codes verwendet die abstrakte-Klasse zum Speichern und Abrufen von Objekten aus der Datenbank. In jeder Anwendung, die diese abstrakte Klasse verwendet, muss eine komplette-Implementierung übergeben werden, die die tatsächliche Datenbankfunktionalität bereitstellt.

### <a name="taskyandroid-and-taskyios"></a>Taskyandroid und taskyios

Die IOS-und Android-Anwendungsprojekte enthalten die Benutzeroberfläche und anderen plattformspezifischen Code, der verwendet wird, um den freigegebenen Code in der PCL zu verbinden.

Diese Projekte enthalten auch eine Implementierung der abstrakten Datenbank-API, die auf dieser Plattform funktioniert. Unter IOS und Android ist das SQLite-Datenbankmodul in das Betriebssystem integriert, sodass die Implementierung `[DllImport]` wie gezeigt verwenden kann, um die konkrete Implementierung der Datenbankkonnektivität bereitzustellen. Hier sehen Sie einen Auszug des plattformspezifischen Implementierungs Codes:

```csharp
[DllImport("sqlite3", EntryPoint = "sqlite3_open")]
public static extern Result Open(string filename, out IntPtr db);

[DllImport("sqlite3", EntryPoint = "sqlite3_close")]
public static extern Result Close(IntPtr db);
```

Die vollständige Implementierung finden Sie im Beispielcode.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die Vorteile und Fehlerquellen von portablen Klassenbibliotheken kurz erläutert. es wurde gezeigt, wie Sie pcls in Visual Studio für Mac und Visual Studio erstellen und nutzen können. und schließlich wurde eine komplette Beispielanwendung – taskyportable – eingeführt, die eine PCL in Aktion anzeigt.

## <a name="related-links"></a>Verwandte Links

- [Taskyportable (Beispiel)](https://docs.microsoft.com/samples/xamarin/mobile-samples/taskyportable/)
- [Erstellen von plattformübergreifenden Anwendungen](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [Portable Visual Basic](~/cross-platform/platform/visual-basic/index.md)
- [Freigegebene Projekte](~/cross-platform/app-fundamentals/shared-projects.md)
- [Freigeben von Code Optionen](~/cross-platform/app-fundamentals/code-sharing.md)
- [Plattformübergreifende Entwicklung mit der .NET Framework (Microsoft)](https://docs.microsoft.com/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library)
