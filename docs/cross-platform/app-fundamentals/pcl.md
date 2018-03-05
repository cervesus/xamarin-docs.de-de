---
title: "Einführung in portablen Klassenbibliotheken"
description: "Dieser Artikel führt Projekte für Portable Klassenbibliothek (PCL) und führt Sie durch Erstellen und Nutzen von PCL-Projekte in Visual Studio für Mac und Visual Studio."
ms.topic: article
ms.prod: xamarin
ms.assetid: 76ba8f7a-9b6e-40f5-9a29-ff1274ece4f2
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: e3701960f246a8f627d991edf244656b5fd8958e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-portable-class-libraries"></a>Einführung in portablen Klassenbibliotheken

_Dieser Artikel führt Projekte für Portable Klassenbibliothek (PCL) und führt Sie durch Erstellen und Nutzen von PCL-Projekte in Visual Studio für Mac und Visual Studio._


Ein wichtiger Bestandteil beim Erstellen von plattformübergreifenden Anwendungen wird Code in verschiedenen plattformspezifischen Projekte gemeinsam nutzen können. Dies wird durch die Tatsache komplizierter, unterschiedliche Plattformen häufig eine andere Teilmenge von der .NET Basisklassenbibliothek (BCL), und aus diesem Grund werden tatsächlich auf ein anderes .NET Core Library-Profil erstellt. Dies bedeutet, dass jede Plattform nur Klassenbibliotheken verwenden kann, die auf das gleiche Profil gerichtet sind, damit sie angezeigt werden, um separate Klassenbibliothekprojekte für jede Plattform anzufordern.

Stehen drei wichtige Möglichkeiten zur gemeinsamen Nutzung von Code, der dieses Problem zu beheben: **.NET Standardprojekten**, **Projekte für Portable Klassenbibliothek (PCL)**, und **freigegebene Asset Projekte**.

- **.NET Standardprojekten** [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md).
-  **PCL** -Projekte bestimmter Profile, die einen bekannten Satz von BCL Klassen und-Funktionen unterstützen. Die Seite nach unten PCL ist jedoch, dass sie häufig sehr architektonische Aufwand zum Aufteilen von Profil bestimmten Code in ihrer eigenen Bibliotheken erfordern. Eine ausführlichere Erläuterung für diese beiden Ansätze, finden Sie unter der [Sharing Code Options geführt](~/cross-platform/app-fundamentals/code-sharing.md) .
-  **Freigegebener Projekte Asset** verwenden einen einzelnen Satz von Dateien und Angebote, die eine schnelle und einfache ermöglichen das Freigeben von Code in einer Projektmappe und im Allgemeinen beschäftigt bedingten Kompilierungsdirektiven Codepfade für verschiedene Plattformen angeben, die sie (Weitere Informationen verwenden Informationen finden Sie unter der [freigegebene Projekte Artikel](~/cross-platform/app-fundamentals/shared-projects.md) und [Einrichten einer Xamarin plattformübergreifende Lösungshandbuch](~/cross-platform/app-fundamentals/code-sharing.md) ).


Auf dieser Seite erläutert, wie eine **PCL** das für ein bestimmtes Profil vorgesehen ist, das dann von mehreren plattformspezifischen Projekte verwiesen werden kann.

## <a name="requirements"></a>Anforderungen

Portable Library-Projekte werden automatisch in Visual Studio für Mac in aktiviert MacOS und auf Visual Studio 2013 und höher integriert sind.


## <a name="what-is-a-portable-class-library"></a>Was ist eine Portable Klassenbibliothek?

Wenn Sie ein Projekt für die Anwendung oder ein Klassenbibliotheksprojekt erstellen, ist die resultierende DLL für die Arbeit auf die jeweilige Plattform für die Erstellung beschränkt. Dies verhindert, dass Sie von einer Assembly für eine Windows-app schreiben und dann erneut auf Xamarin.iOS und Xamarin.Android verwenden.

Wenn Sie eine Portable Klassenbibliothek erstellen, können Sie jedoch eine Kombination von Plattformen auswählen, die Ihr Code auch auf ausgeführt werden soll. Kompatibilität Optionen, die Sie beim Erstellen einer portablen Klassenbibliothek werden in einen Bezeichner "Profil" übersetzt, die beschreibt, welche Plattformen unterstützt die Bibliothek.

Die folgende Tabelle zeigt einige der Funktionen, die von der .NET Plattform variieren. Um eine Assembly PCL schreiben, die garantiert auf bestimmten Geräten-Plattformen ausgeführt wird, wählen Sie einfach die Unterstützung erforderlich ist, wenn Sie das Projekt erstellen.

<table border="1" cellpadding="1" cellspacing="1">
  <tbody>
    <tr>
      <td>
Feature </td>
      <td>
.NET Framework </td>
      <td>
UWP-Apps </td>
      <td>
Silverlight </td>
      <td>
Windows Phone </td>
      <td>
Xamarin </td>
    </tr>
    <tr>
      <td>
Kernspeicher </td>
      <td>
J </td>
      <td>
J </td>
      <td>
J </td>
      <td>
J </td>
      <td>
J </td>
    </tr>
    <tr>
      <td>
LINQ </td>
      <td>
J </td>
      <td>
J </td>
      <td>
J </td>
      <td>
J </td>
      <td>
J </td>
    </tr>
    <tr>
      <td>
IQueryable </td>
       <td>
J </td>
      <td>
J </td>
      <td>
J </td>
      <td>
7.5 + </td>
      <td>
J </td>
    </tr>
    <tr>
      <td>
Serialisierung </td>
      <td>
J </td>
      <td>
J </td>
      <td>
J </td>
      <td>
J </td>
      <td>
J </td>
    </tr>
    <tr>
      <td>
Datenanmerkungen </td>
      <td>
4.0.3 + </td>
      <td>
J </td>
      <td>
J </td>
      <td>
      </td>
      <td>
J </td>
    </tr>
  </tbody>
</table>

Die Xamarin-Spalte gibt die Tatsache, Xamarin.iOS und Xamarin.Android unterstützt die Profile, die mit Visual Studio 2013 und höher ausgeliefert und die Verfügbarkeit von Funktionen in Bibliotheken, die Sie erstellen werden nur beschränkt werden, die andere Plattformen, mit denen Sie nach Wunsch wieder. unterstützt.

Dies umfasst Profile, die Kombinationen aus:

-  .NET 4 oder .NET 4.5
-  Silverlight 5
-  Windows Phone 8
-  UWP-Apps

Erfahren Sie mehr über die verschiedenen Profile Funktionen auf [Microsoft Website](http://msdn.microsoft.com/en-us/library/gg597391(v=vs.110).aspx) und finden Sie unter anderen Community Elements [PCL profilzusammenfassung](http://embed.plnkr.co/03ck2dCtnJogBKHJ9EjY) inklusive der Framework-Informationen und andere Hinweise unterstützt.



Erstellen einer PCL zum Freigeben der Code hat eine Reihe von vor- und Nachteile im Vergleich zur Alternative Datei verknüpfen:


**Vorteile**

1. Zentralisierte Codefreigabe – schreiben und Testen von Code in einem einzelnen Projekt, das von anderen Bibliotheken oder Anwendungen genutzt werden können.
1. Umgestaltungsvorgänge wirkt sich der gesamte Code in der Projektmappe (der portablen Klassenbibliothek und die plattformspezifischen Projekte) geladen.
1. PCL-Projekt kann leicht von anderen Projekten in einer Projektmappe verwiesen werden, oder die Ausgabeassembly für andere Benutzer in ihre Lösungen verweisen gemeinsam genutzt werden kann.


**Nachteile**

1. Da die gleichen Portable Class Library mehrere Anwendungen gemeinsam verwendet werden, können nicht plattformspezifische Bibliotheken (z. b. verwiesen werden Community.CsharpSqlite.WP7).
1. Die Portable Class Library-Teilmenge dürfen keine Klassen enthalten, die sonst in MonoTouch und Mono für Android (z. B. DllImport oder System.IO.File) verfügbar.



In gewissem Maß können beide Nachteile umgangen werden, mit dem Muster Datenanbieters oder die Abhängigkeitsinjektion code von der tatsächlichen Implementierung in Projekten für die Plattform für eine Schnittstelle oder Base-Klasse, die in der portablen Klassenbibliothek definiert wird.



Dieses Diagramm zeigt die Architektur einer plattformübergreifenden-Anwendung mithilfe einer portablen Klassenbibliothek Freigeben von Code, aber auch mithilfe der Abhängigkeitsinjektion plattformabhängigen Funktionen übergeben:



[![](pcl-images/image1.png "Dieses Diagramm zeigt die Architektur einer plattformübergreifenden-Anwendung mithilfe einer portablen Klassenbibliothek Freigeben von Code, aber auch mithilfe der Abhängigkeitsinjektion plattformabhängigen Funktionen übergeben")](pcl-images/image1.png)



# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)



## <a name="visual-studio-for-mac-walkthrough"></a>Exemplarische Vorgehensweise für Visual Studio für Mac


Dieser Abschnitt führt Sie durch die Schritte zum Erstellen und Verwenden einer portablen Klassenbibliothek mit Visual Studio für Mac. Finden Sie unter dem Abschnitt eine vollständige Implementierung PCL-Beispiel.



### <a name="creating-a-pcl"></a>Erstellen einer PCL


Das Hinzufügen einer portablen Klassenbibliothek zur Projektmappe ist vergleichbar mit einem regulären-Bibliotheksprojekt hinzufügen.


1. Wählen Sie in das Dialogfeld "Neues Projekt" die `Multiplatform > Library > Portable Library` Option


    ![](pcl-images/image2.png "Mehrere Plattformen > Bibliothek > Portable Library")


1. Beim Erstellen eine PCL in Visual Studio für Mac wird er automatisch mit einem Profil konfiguriert, der für Xamarin.iOS und Xamarin.Android geeignet. PCL-Projekt wird angezeigt, wie in diesem Screenshot gezeigt:

    ![](pcl-images/image3.png "PCL-Projekt wird angezeigt, wie in diesem Screenshot dargestellt.")

Die PCL ist nun bereit für Code hinzugefügt werden. Es kann auch von anderen Projekten (Anwendungsprojekte, Bibliotheksprojekte und auch andere Projekte PCL) verwiesen werden.



### <a name="editing-pcl-settings"></a>Bearbeiten der Einstellungen für PCL


Klicken Sie zum Anzeigen und ändern Sie die PCL-Einstellungen für dieses Projekt, mit der rechten Maustaste des Projekts, und wählen Sie **Optionen > Erstellen > Allgemein** , der hier gezeigte Bildschirm finden Sie unter:



[![](pcl-images/image4.png "Zum Anzeigen und ändern Sie die PCL-Einstellungen für dieses Projekt, mit der rechten Maustaste des Projekts, und wählen Optionen erstellen Allgemein auf die hier gezeigte Bildschirm finden Sie unter")](pcl-images/image4.png)



Die Einstellungen auf diesem Bildschirm steuern, welche Plattformen, die diesen bestimmten PCL mit kompatibel ist. Eine dieser Optionen ändern, ändert das Profil dieses PCL, die wiederum steuert, welche Funktionen werden können, die in der portablen Code verwendet.



Ändern eines der `Target Framework` Optionen automatisch aktualisiert, die `Current Profile`; der Bildschirm wird außerdem eine Warnmeldung angezeigt, wenn inkompatible Optionen ausgewählt sind.



[![](pcl-images/image5.png "Die Zielframework-Optionen automatisch ändern, aktualisiert das aktuelle Profil der Bildschirm wird außerdem eine Warnmeldung angezeigt, wenn inkompatible Optionen ausgewählt werden")](pcl-images/image5.png)



Wenn das Profil geändert wird, nachdem der PCL Code bereits hinzugefügt wurde, ist es möglich, dass die Bibliothek nicht mehr kompiliert wird, wenn der Code verweist auf Funktionen, die nicht Teil des Profils neu ausgewählt sind.


## <a name="working-with-a-pcl"></a>Arbeiten mit einer PCL


Wenn Code in einer PCL-Bibliothek geschrieben wird, wird der Visual Studio für Mac-Editor erkennt die Einschränkungen des ausgewählten Profils und AutoVervollständigen-Optionen entsprechend anpassen. Beispielsweise diese Screenshot zeigt die AutoVervollständigen-Optionen für System.IO mithilfe des Standardprofils (Profile136) in Visual Studio für Mac verwendet – Beachten Sie die Bildlaufleiste gibt an, etwa eine Hälfte der verfügbaren Klassen angezeigt werden (in der Tat stehen nur 14 Klassen zur Verfügung).



[![](pcl-images/image6.png "E/a über das Standardprofil Profile136 in Visual Studio für Mac Beachten Sie die Bildlaufleiste gibt an, etwa eine Hälfte der verfügbaren Klassen tatsächlich angezeigten sind nur 14 verfügbaren Klassen verwendet wird")](pcl-images/image6.png)



Vergleichen Sie, dass die automatische Vervollständigung in einem Xamarin.iOS oder Xamarin.Android – System.IO sind 40 Klassen verfügbaren häufig Klassen wie verwendete `File` und `Directory` der sind nicht in jedem beliebigen PCL-Profil.



[![](pcl-images/image7.png "Es sind 40 Klassen, die Klassen wie Datei- und Verzeichnisdaten, die nicht in jedem beliebigen PCL Profil sind, verfügbare einschließlich häufig verwendet werden.")](pcl-images/image7.png)



Dies ist der zugrunde liegenden Nachteil der Verwendung der PCL, widerspiegelt – die Möglichkeit, Code nahtlos in einer Vielzahl von Plattformen gemeinsam nutzen, dass bestimmte APIs nicht für Sie verfügbar sind, da sie nicht alle möglichen plattformübergreifend vergleichbare Implementierungen verfügen.



### <a name="using-pcl"></a>Verwenden von PCL


Sobald eine PCL-Projekt erstellt wurde, können Sie einen Verweis darauf aus einem kompatiblen Anwendung oder Library-Projekt auf die gleiche Weise hinzufügen, Sie normalerweise fügen Sie Verweise hinzu. Klicken Sie in Visual Studio für Mac mit der rechten Maustaste auf den Knoten "Verweise", und wählen Sie `Edit References…` klicken Sie dann auf der Registerkarte "Projekte" wechseln, wie dargestellt:



[![](pcl-images/image8.png "In Visual Studio für Mac mit der rechten Maustaste auf den Knoten "Verweise", und wählen Sie Verweise bearbeiten und dann wechseln Sie zur Registerkarte "Projekte" gezeigten")](pcl-images/image8.png)



Das folgende Bildschirmfoto zeigt das Auffüllzeichen Lösung für die TaskyPortable Beispiel-app mit der PCL-Bibliothek unter dem unteren und einen Verweis auf die PCL-Bibliothek im Projekt Xamarin.iOS.



[![](pcl-images/image9.png "Das Auffüllzeichen Lösung für die TaskyPortable-Beispiel-app")](pcl-images/image9.png)



Die Ausgabe einer PCL (d. h. die resultierende Assembly-DLL) kann auch als Verweis auf die meisten Projekte hinzugefügt werden. Auf diese Weise PCL eine ideale Möglichkeit, um plattformübergreifende-Komponenten und -Bibliotheken ausgeliefert.




# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)




## <a name="visual-studio-walkthrough"></a>Visual Studio: Exemplarische Vorgehensweise


Dieser Abschnitt führt Sie durch die Schritte zum Erstellen und Verwenden einer portablen Klassenbibliothek mit Visual Studio. Finden Sie unter dem Abschnitt eine vollständige Implementierung PCL-Beispiel.



### <a name="creating-a-pcl"></a>Erstellen einer PCL


Das Hinzufügen einer PCL der Projektmappe in Visual Studio ist etwas anders als ein reguläres Projekt hinzufügen.



1. Wählen Sie auf dem Bildschirm "Neues Projekt hinzufügen" die `Portable Class Library` Option


    ![](pcl-images/image10.png "Portable Klassenbibliothek")


1. Visual Studio wird sofort das folgende Dialogfeld aufgefordert, damit das Profil konfiguriert werden kann.
 Aktivieren Sie die Plattformen, die Sie benötigen, zu unterstützen, und klicken auf OK.


    ![](pcl-images/image11.png "Aktivieren Sie die Plattformen, die Sie benötigen, zu unterstützen, und klicken auf OK")


1. PCL-Projekt wird angezeigt, wie im Projektmappen-Explorer angezeigt. Der Knoten "Verweise" wird angegeben, dass die Bibliothek eine Teilmenge von .NET Framework (definiert durch das Profil PCL) verwendet.

    ![](pcl-images/image12.png ".NET Framework definiert, indem Sie das PCL-Profil")

Die PCL ist nun bereit für Code hinzugefügt werden. Es kann auch von anderen Projekten (Anwendungsprojekte, Bibliotheksprojekte und auch andere Projekte PCL) verwiesen werden.



### <a name="editing-pcl-settings"></a>Bearbeiten der Einstellungen für PCL


Die PCL-Einstellungen angezeigt und geändert werden, indem Sie mit der rechten Maustaste auf das Projekt und können **Eigenschaften > Bibliothek** , wie in diesem Screenshot dargestellt:



[![](pcl-images/image13.png "Die PCL-Einstellungen können angezeigt und geändert, indem Sie mit der rechten Maustaste auf das Projekt, und wählen die Bibliothek für Eigenschaften, wie in diesem Screenshot dargestellt werden")](pcl-images/image13.png)



Wenn das Profil geändert wird, nachdem der PCL Code bereits hinzugefügt wurde, ist es möglich, dass die Bibliothek nicht mehr kompiliert wird, wenn der Code verweist auf Funktionen, die nicht Teil des Profils neu ausgewählt sind.



### <a name="working-with-a-pcl"></a>Arbeiten mit einer PCL


Wenn Code in einer PCL-Bibliothek geschrieben wird, wird Visual Studio erkennt die Einschränkungen des ausgewählten Profils und Intellisense-Optionen entsprechend anpassen. Beispielsweise diese Screenshot zeigt die AutoVervollständigen-Optionen für System.IO mithilfe des Standardprofils (Profile136) – Beachten Sie die Bildlaufleiste gibt an, etwa eine Hälfte der verfügbaren Klassen angezeigt werden (in der Tat stehen nur 14 Klassen zur Verfügung).



[![](pcl-images/image14.png "E/a über das Standardprofil Profile136")](pcl-images/image14.png)



Vergleichen Sie, dass die automatische Vervollständigung in einem regulären Projekt – System.IO sind 40 Klassen verfügbaren häufig Klassen wie verwendete `File` und `Directory` der sind nicht in jedem beliebigen PCL-Profil.



[![](pcl-images/image15.png "Automatische Vervollständigung in einem regulären-Projekt")](pcl-images/image15.png)



Dies ist der zugrunde liegenden Nachteil der Verwendung der PCL, widerspiegelt – die Möglichkeit, Code nahtlos in einer Vielzahl von Plattformen gemeinsam nutzen, dass bestimmte APIs nicht für Sie verfügbar sind, da sie nicht alle möglichen plattformübergreifend vergleichbare Implementierungen verfügen.



### <a name="using-pcl"></a>Verwenden von PCL


Sobald eine PCL-Projekt erstellt wurde, können Sie einen Verweis darauf aus einem kompatiblen Anwendung oder Library-Projekt auf die gleiche Weise hinzufügen, Sie normalerweise fügen Sie Verweise hinzu. Klicken Sie in Visual Studio mit der rechten Maustaste auf den Knoten "Verweise", und wählen Sie `Add Reference...` wechseln Sie zu der **Lösung: Projekte** Registerkarte wie gezeigt:



[![](pcl-images/image16.png ""Projekte" gezeigten")](pcl-images/image16.png)



Der folgende Screenshot zeigt den Projektmappen-Bereich für die TaskyPortable Beispiel-app mit der PCL-Bibliothek unter dem unteren und einen Verweis auf die PCL-Bibliothek im Projekt Xamarin.iOS.



[![](pcl-images/image17.png "Im Bereich der Lösung für die TaskyPortable-Beispiel-app")](pcl-images/image17.png)



Die Ausgabe einer PCL (d. h. die resultierende Assembly-DLL) kann auch als Verweis auf die meisten Projekte hinzugefügt werden.
Auf diese Weise PCL eine ideale Möglichkeit, um plattformübergreifende-Komponenten und -Bibliotheken ausgeliefert.




-----



## <a name="pcl-example"></a>PCL-Beispiel


Die [TaskyPortable](https://developer.xamarin.com/samples/mobile/TaskyPortable/) beispielanwendung für veranschaulicht, wie eine Portable Klassenbibliothek mit Xamarin verwendet werden können.
Hier sind einige Screenshots der resultierenden apps unter iOS, Android und Windows Phone:



[![](pcl-images/image18.png "Hier sind einige Screenshots der resultierenden apps unter iOS, Android und Windows Phone")](pcl-images/image18.png)



Die eine Reihe von Daten und Logik Klassen, die ausschließlich portablen Code sind geteilt, und darüber hinaus veranschaulicht Clientplattform-spezifische Anforderungen, die mithilfe der Abhängigkeitsinjektion für die Implementierung der SQLite-Datenbank zu integrieren.




Die Lösungsstruktur wird unten gezeigt (in Visual Studio für Mac und Visual Studio bzw.):



[![](pcl-images/image19.png "Die Projektmappenstruktur in Visual Studio für Mac und Visual Studio bzw. lautet")](pcl-images/image19.png)



Da der SQLite-NET-Code plattformspezifische Schritte (mit SQLite Implementierungen auf allen anderen Betriebssystemen arbeiten) aufweist, für die Demo Zwecke wurde in eine abstrakte umgestaltet Klasse, die in einer portablen Klassenbibliothek kompiliert werden kann und der tatsächliche Code, der als Unterklassen IOS- und Android-Projekte implementiert.



### <a name="taskyportablelibrary"></a>TaskyPortableLibrary

Die Features von .NET ist der portablen Klassenbibliothek beschränkt, die sie unterstützen kann. Da sie für die Ausführung auf mehreren Plattformen kompiliert wird, kann nicht erleichtern nutzen `[DllImport]` Funktionen, die in SQLite-NET verwendet wird. Stattdessen ist SQLite-NET als eine abstrakte Klasse implementiert, und klicken Sie dann auf die verwiesen wird durch den Rest des freigegebenen Codes. Ein Auszug aus der abstrakten-API wird unten gezeigt:


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


Die restliche den gemeinsam verwendeten Code verwendet die abstrakte Klasse, um "Speichern" und "abrufen" Objekte aus der Datenbank. In jeder Anwendung, die diese abstrakte Klasse wird verwendet, müssen in eine vollständige Implementierung übergeben, die die aktuelle Datenbankfunktionalität bereitstellt.



### <a name="taskyandroid-and-taskyios"></a>TaskyAndroid and TaskyiOS


IOS- und Android-Anwendungsprojekten enthalten die Benutzeroberfläche und andere plattformspezifischen Code verwendet, um den gemeinsam verwendeten Code in der PCL anschließen.



Diese Projekte enthalten auch eine Implementierung der abstrakten Datenbank-API, die für diese Plattform verwendet werden kann. Auf IOS- und Android die Sqlite-Datenbankmoduls ist für das Betriebssystem integriert, damit die Implementierung verwenden, kann `[DllImport]` wie gezeigt, um die konkrete Implementierung der Verbindung mit der Datenbank bereitzustellen. Ein Auszug aus der plattformspezifischen Implementierungscode wird hier gezeigt:


```csharp
[DllImport("sqlite3", EntryPoint = "sqlite3_open")]
public static extern Result Open(string filename, out IntPtr db);

[DllImport("sqlite3", EntryPoint = "sqlite3_close")]
public static extern Result Close(IntPtr db);
```


Die vollständige Implementierung kann im Beispielcode angezeigt werden.

### <a name="taskywinphone"></a>TaskyWinPhone


Die Windows Phone-Anwendung hat ihre Benutzeroberfläche mit XAML erstellt und enthält andere plattformspezifischen Code, um die freigegebenen Objekte mit der Benutzeroberfläche eine Verbindung herzustellen.



Im Gegensatz zu von der Implementierung für iOS und Android, Windows Phone-app erstellen und verwenden eine Instanz von muss die `Community.Sqlite.dll` als Teil seiner abstrakte Datenbank-API. Anstelle von `DllImport`, Methoden wie z. B. Open werden implementiert, für die Community.Sqlite-Assembly, auf das verweist, die `TaskWinPhone` Projekt. Ein Auszug wird für den Vergleich mit der IOS- und Android-Version als hier angezeigt.


```csharp
public static Result Open(string filename, out Sqlite3.sqlite3 db)
{
    db = new Sqlite3.sqlite3();
    return (Result)Sqlite3.sqlite3_open(filename, ref db);
}

public static Result Close(Sqlite3.sqlite3 db)
{
    return (Result)Sqlite3.sqlite3_close(db);
}
```


## <a name="summary"></a>Zusammenfassung


In diesem Artikel kurz erläutert, die vor- und Nachteile von portablen Klassenbibliotheken, veranschaulicht, wie erstellen und Nutzen von PCLs innerhalb von Visual Studio für Mac und Visual Studio; und schließlich eingeführt eine vollständige beispielanwendung – TaskyPortable –, die eine PCL in Aktion anzeigt.


## <a name="related-links"></a>Verwandte Links

- [TaskyPortable (Beispiel)](https://developer.xamarin.com/samples/mobile/TaskyPortable/)
- [Erstellen von plattformübergreifenden Anwendungen](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [Portable Visual Basic](~/cross-platform/platform/visual-basic/index.md)
- [Freigegebene Projekte](~/cross-platform/app-fundamentals/shared-projects.md)
- [Optionen für die Codefreigabe](~/cross-platform/app-fundamentals/code-sharing.md)
- [Plattformübergreifende Entwicklung mit .NET Framework (Microsoft)](http://msdn.microsoft.com/en-us/library/gg597391(v=vs.110).aspx)
