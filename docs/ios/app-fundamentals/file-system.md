---
title: Arbeiten mit dem Dateisystem
description: "Die gleichen System.IO-Klassen können Xamarin.iOS arbeiten mit Dateien und Verzeichnissen in iOS, die Sie in jeder .NET-Anwendung verwenden würden. Allerdings wird trotz der vertrauten Klassen und Methoden, ein iOS implementiert einige Einschränkungen auf die Dateien, die erstellt oder zugegriffen werden können und außerdem bietet spezielle Funktionen für bestimmte Verzeichnisse. In diesem Artikel werden diese Einschränkungen und Funktionen dargestellt, und veranschaulicht die Funktionsweise der Zugriff auf Dateien in einem Xamarin.iOS-Anwendung."
ms.topic: article
ms.prod: xamarin
ms.assetid: 37DF2F38-901E-8F8E-269A-5EE0CCD28C08
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: a3337264bf04f5ad5495043c7e958276aba9eaee
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
# <a name="working-with-the-file-system"></a>Arbeiten mit dem Dateisystem

_Die gleichen System.IO-Klassen können Xamarin.iOS arbeiten mit Dateien und Verzeichnissen in iOS, die Sie in jeder .NET-Anwendung verwenden würden. Allerdings wird trotz der vertrauten Klassen und Methoden, ein iOS implementiert einige Einschränkungen auf die Dateien, die erstellt oder zugegriffen werden können und außerdem bietet spezielle Funktionen für bestimmte Verzeichnisse. In diesem Artikel werden diese Einschränkungen und Funktionen dargestellt, und veranschaulicht die Funktionsweise der Zugriff auf Dateien in einem Xamarin.iOS-Anwendung._

Können Sie Xamarin.iOS und `System.IO` Klassen in der *.NET Basisklassenbibliothek (Base Class Library, BCL)* auf iOS-Dateisystem zugreifen. Die `File` -Klasse können Sie erstellen, löschen und Lesen von Dateien, und die `Directory` -Klasse können Sie zum Erstellen, löschen oder aufführen des Inhalts von Verzeichnissen. Sie können auch `Stream` Unterklassen, die ein höheres Maß an Kontrolle über Dateioperationen (z. B. Komprimierung oder Position Suche innerhalb einer Datei) bereitstellen können.

iOS erzwingt einige Einschränkungen für eine Anwendung mit dem Dateisystem, um die Sicherheit einer Anwendung Daten beibehalten und Benutzer über bösartige apps schützen Möglichkeiten. Diese Einschränkungen sind Teil der *Anwendung Sandkasten* – ein Satz von Regeln, die eine Anwendung Zugriff auf Dateien, Einstellungen, Netzwerkressourcen, Hardware usw. beschränkt. Eine Anwendung ist auf Lesen und Schreiben von Dateien in sein Basisverzeichnis (Installationsort); Es kann nicht auf eine andere Anwendung Dateien zugreifen.

iOS verfügt auch über einige Funktionen Datei: bestimmte Verzeichnisse erfordern eine besondere Behandlung in Bezug auf die Sicherungen und Updates und Anwendungen können auch Freigeben von Dateien über iTunes.

Dieser Artikel beschreibt die Funktionen und Einschränkungen des iOS-Dateisystem im Detail und eine beispielanwendung, die Xamarin.iOS auszuführende einige einfache Dateisystemvorgänge veranschaulicht:

 [![](file-system-images/05-sampleapp.png "Ein Beispiel für iOS ausführen einige einfache Dateisystemvorgänge")](file-system-images/05-sampleapp.png#lightbox)

 <a name="General_File_Access" />


## <a name="general-file-access"></a>Allgemeine Dateizugriff

Xamarin.iOS können Sie die Verwendung der .NET `System.IO` Klassen, die für Dateisystemvorgänge unter iOS.

Die folgenden Codeausschnitte veranschaulichen einige allgemeinen Dateivorgänge. Finden Sie Sie alle nachfolgend in der `SampleCode.cs` Datei, in der beispielanwendung für diesen Artikel.

 <a name="Working_with_directories" />


### <a name="working-with-directories"></a>Arbeiten mit Verzeichnissen

Dieser Code Listet die Unterverzeichnisse im aktuellen Verzeichnis (gemäß der ". /" Parameters), gibt den Speicherort der ausführbaren Datei für Ihre Anwendung.
Die Ausgabe wird eine Liste aller Dateien und Ordnern, die mit Ihrer Anwendung (angezeigt im Konsolenfenster angezeigt, während des Debuggens) bereitgestellt werden.

```csharp
var directories = Directory.EnumerateDirectories("./");
foreach (var directory in directories) {
      Console.WriteLine(directory);
}
```

 <a name="Reading_files" />


### <a name="reading-files"></a>Lesen von Dateien

Um eine Textdatei lesen zu können, benötigen Sie nur eine einzige Codezeile. In diesem Beispiel wird der Inhalt einer Textdatei im Ausgabefenster Anwendung angezeigt.

```csharp
var text = File.ReadAllText("TestData/ReadMe.txt");
Console.WriteLine(text);
```

 <a name="XML_Serialization" />


### <a name="xml-serialization"></a>XML-Serialisierung

Obwohl arbeiten mit den vollständigen `System.Xml` Namespace ist nicht Gegenstand dieses Artikels, problemlos Deserialisieren von XML-Dokument aus dem Dateisystem kann mit einem StreamReader wie folgt:

```csharp
using (TextReader reader = new StreamReader("./TestData/test.xml")) {
      XmlSerializer serializer = new XmlSerializer(typeof(MyObject));
      var xml = (MyObject)serializer.Deserialize(reader);
}
```

Finden Sie in der MSDN-Dokumentation für die ["System.xml"](http://msdn.microsoft.com/en-us/library/system.xml.aspx) Namespace-URI für Weitere Informationen zu [Serialisierung](http://msdn.microsoft.com/en-us/library/system.xml.serialization.aspx). Überprüfen Sie außerdem die [Xamarin.iOS Dokumentation](~/ios/deploy-test/linker.md) auf vom Linker – in der Regel Sie müssen zum Hinzufügen der `[Preserve]` -Attribut auf Klassen, die Sie serialisieren möchten.

 <a name="Creating_Files_and_Directories" />


### <a name="creating-files-and-directories"></a>Erstellen von Dateien und Verzeichnissen

Dieses Beispiel zeigt, wie die `Environment` Klasse, um den Ordner "Dokumente" zugreifen, in dem wir Dateien und Verzeichnisse erstellen können.

```csharp
var documents =
 Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments); 
var filename = Path.Combine (documents, "Write.txt");
File.WriteAllText(filename, "Write this text into a file");
```

Erstellen ein Verzeichnis ist sehr ähnlich:

```csharp
var documents =
 Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var directoryname = Path.Combine (documents, "NewDirectory");
Directory.CreateDirectory(directoryname);
```

Weitere Informationen zu den System.IO-Namespace, finden Sie unter der [MSDN-Dokumentation](http://msdn.microsoft.com/en-us/library/system.io.aspx).


### <a name="serializing-json"></a>Serialisieren von Json

Arbeiten mit Json Daten in einer Anwendung Xamarin.iOS ist sehr einfach mit der [Json.NET](http://www.newtonsoft.com/json) Hochleistungs-JSON-Framework für .NET NuGet-Paket. Fügen Sie einfach das NuGet-Paket, das Projekt Ihrer Anwendung: 

[![](file-system-images/json01.png "Das NuGet-Paket hinzufügen zum Projekt Anwendungen")](file-system-images/json01.png#lightbox)

Als Nächstes fügen Sie eine Klasse, die als Datenmodell für die Serialisierung/Deserialisierung fungiert (in diesem Fall `Account.cs`):

```csharp
using System;
using System.Collections.Generic;
using Foundation; // for Preserve attribute, which helps serialization with Linking enabled

namespace FileSystem
{
    [Preserve]
    public class Account
    {
        #region Computed Properties
        public string Email { get; set; }
        public bool Active { get; set; }
        public DateTime CreatedDate { get; set; }
        public List<string> Roles { get; set; }
        #endregion

        #region Constructors
        public Account() {

        }
        #endregion
    }
}
```

Schließlich erstellen Sie eine Instanz von der `Account` Klasse und deren Serialisierung in Json-Daten in eine Datei zu schreiben:

```csharp
// Create a new record
var account = new Account(){
    Email = "monkey@xamarin.com",
    Active = true,
    CreatedDate = new DateTime(2015, 5, 27, 0, 0, 0, DateTimeKind.Utc),
    Roles = new List<string> {"User", "Admin"}
};

// Serialize object
var json = JsonConvert.SerializeObject(account, Newtonsoft.Json.Formatting.Indented);

// Save to file
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var filename = Path.Combine (documents, "account.json");
File.WriteAllText(filename, json);
```
Finden Sie in der Json-.NET [Dokumentation](http://www.newtonsoft.com/json/help) für Weitere Informationen zum Arbeiten mit Json-Daten in einer .NET-Anwendung.

<a name="Special_Considerations" />

## <a name="special-considerations"></a>Besonderheiten

Trotz der Ähnlichkeit zwischen Xamarin.iOS und .NET Dateivorgänge, iOS und Xamarin.iOS unterscheiden sich in einigen wichtigen Punkten von .NET.

 <a name="runtimeaccessible" />


### <a name="making-project-files-accessible-at-runtime"></a>Zur Laufzeit von Projektdateien zugänglich

Standardmäßig, wenn Sie Ihrem Projekt eine Datei hinzufügen, es wird nicht in die endgültige Assembly enthalten sein, und daher nicht für Ihre Anwendung verfügbar. Um eine Datei in die Assembly einschließen möchten, müssen Sie es mit dem speziellen Buildvorgang aufgerufen Inhalt markieren.

Um eine Datei für die Aufnahme zu kennzeichnen, mit der rechten Maustaste auf die Dateien, und wählen Sie **Buildvorgang &gt; Content** in Visual Studio für Mac. Sie können auch ändern, die **Buildvorgang** in der Datei **Eigenschaften** Blatt.

 <a name="Case_Sensitivity" />


### <a name="case-sensitivity"></a>Groß- und Kleinschreibung

Es ist wichtig zu verstehen, dass das iOS-Dateisystem ist *Groß-/Kleinschreibung beachtet*. Dies bedeutet, dass die Datei- und Verzeichnisnamen genau übereinstimmen müssen – "Readme.txt" und "Readme.txt" unterschiedliche Dateinamen betrachtet werden.

Dies ist möglicherweise verwirrend für .NET-Entwickler, mit dem Windows-Dateisystem vertrauter sind also *Groß-/Kleinschreibung beachten*– "Dateien", "Dateien" und "Dateien", würden alle in dasselbe Verzeichnis verweisen.

Deshalb Fall zwar iOS-Geräte werden die Groß-/Kleinschreibung beachtet, und Ihren Code mit dem geschrieben werden soll, beachten Sie, die iOS-die Simulator ist nicht vertraulich standardmäßig. Dies bedeutet, wenn die Datei selbst und die Verweise auf ihn im Code die Filename Groß-/Kleinschreibung unterschiedlich, Ihren Code weiterhin im Simulator funktionieren möglicherweise jedoch, dass es auf einem echten Gerät ein Fehler auftritt. Dies ist einer der Gründe, warum es wichtig für die Bereitstellung auf einem echten Gerät frühzeitig und häufig während der iOS-Entwicklung ist.

 <a name="Path_Separator" />


### <a name="path-separator"></a>Pfadtrennzeichen

iOS uses the forward slash ‘/’as the path separator (which is different from Windows, which uses the backslash ‘\’).

Aufgrund dieses Unterschieds verwirrend, die es wird empfohlen, verwenden Sie die `System.IO.Path.Combine` -Methode, die für die aktuelle Plattform, statt eine hartcodierung einer bestimmten Pfadtrennzeichen passt. Dies ist ein einfacher Schritt, der den Code für andere Plattformen besser portierbar macht.

 <a name="Application_Sandbox" />


## <a name="application-sandbox"></a>Anwendung Sandkasten

Ihre Anwendung Zugriff auf das Dateisystem (und anderen Ressourcen wie die Features für Netzwerk- und Hardware) ist aus Sicherheitsgründen beschränkt. Diese Einschränkung wird als bezeichnet den *Anwendung Sandkasten*. Im Hinblick auf das Dateisystem ist Ihre Anwendung auf erstellen und Löschen von Dateien und Verzeichnissen in sein Basisverzeichnis beschränkt.

Das Basisverzeichnis ist ein eindeutiger Speicherort im Dateisystem, in dem die Anwendung und alle ihre Daten gespeichert sind. Sie können den Speicherort des Basisverzeichnisses für Ihre Anwendung wählen Sie (oder ändern); Stellen jedoch IOS- und Xamarin.iOS, Eigenschaften und Methoden zum Verwalten von Dateien und Verzeichnisse in.

 <a name="The_Application_Bundle" />


## <a name="the-application-bundle"></a>Anwendungspaket

Die *Anwendungspaket* ist der Ordner, der die Anwendung enthält.
Es ist in anderen Ordnern unterschieden, werden, wenn das Suffix .app in den Namen des Verzeichnisses. Ihre Anwendung-Paket enthält die ausführbare Datei und der gesamte Inhalt (Dateien, Bilder usw.) für das Projekt erforderlich.

Wenn Sie die Anwendungspaket unter Mac OS durchsuchen, wird ein anderes Symbol als in anderen Verzeichnissen angezeigt werden (und das Suffix .app ausgeblendet ist); Es ist jedoch eine reguläre Verzeichnis, das das Betriebssystem anders angezeigt wird.

Um das Anwendungspaket für den Beispielcode anzuzeigen, mit der rechten Maustaste auf das Projekt in Visual Studio für Mac, und wählen **enthaltenden Ordner öffnen**. Navigieren Sie zu **"bin" /Debug/** sollten finden Sie ein Anwendungssymbol (vergleichbar mit dem folgenden Screenshot).

 [![](file-system-images/40-bundle.png "Navigieren Sie zu "bin" / Debug, um ein Anwendungssymbol ähnlich wie in diesem Screenshot suchen")](file-system-images/40-bundle.png#lightbox)

Mit der rechten Maustaste auf das Symbol, und wählen Sie **Anzeigen des Paketinhalts** den Inhalt des Verzeichnisses Anwendungspaket durchsuchen. Der Inhalt wird wie der Inhalt eines regulären Verzeichnisses angezeigt, wie hier gezeigt:

 [![](file-system-images/45-bundle.png "Der Inhalt des app-Pakets")](file-system-images/45-bundle.png#lightbox)

Anwendungspaket ist, was im Simulator oder auf Ihrem Gerät installiert ist, während der Tests und letztlich ist was für die Aufnahme in den App Store an Apple gesendet wird.

 <a name="Application_Directories" />


## <a name="application-directories"></a>Anwendungsverzeichnisse

Wenn Ihre Anwendung auf einem Gerät installiert ist, wird das Betriebssystem sein Basisverzeichnis erstellt und innerhalb Ihrer Anwendungspaket platziert. Anwendungspaket zum Lesen von Daten auf den Code zugreifen kann, aber nichts sollte in dieses Stammverzeichnis geschrieben werden, wie sie signiert und alle Änderungen für ungültig erklärt Ihrer Anwendung und verhindern, dass er gestartet werden.

Ja, obwohl Sie nichts in das Stammverzeichnis geschrieben werden soll <b>in iOS 7 und früher</b> erstellt eine Reihe von Verzeichnissen im Stammverzeichnis Anwendung, die für die Verwendung verfügbar sind. <b>In iOS 8 sind die Benutzer zugänglichen Verzeichnisse <a href="https://developer.apple.com/library/ios/technotes/tn2406/_index.html" target="_blank">befindet sich nicht</a> innerhalb des Stammverzeichnisses der Anwendung</b>.

Diese Verzeichnisse und ihre Zwecke zu verwenden, sind unten aufgeführt:

&nbsp;

|Verzeichnis|Beschreibung|
|---|---|
|[ApplicationName].app/|**In iOS 7 und früher** Dies ist die `ApplicationBundle` Verzeichnis, in dem die ausführbare Datei der Anwendung gespeichert ist. Die Verzeichnisstruktur, die Sie in Ihrer app zu erstellen, die in diesem Verzeichnis (z. B. Bilder und andere Dateitypen, die Sie als Ressourcen in Ihrer Visual Studio für Mac-Projekt markiert haben) vorhanden ist.<br /><br />Wenn Sie die Inhaltsdateien in Ihre Anwendungspaket zugreifen müssen, steht der Pfad zu diesem Verzeichnis über die `NSBundle.MainBundle.BundlePath` Eigenschaft.|
|Dokumente /|Verwenden Sie dieses Verzeichnis zum Speichern von Benutzerdokumente und Datendateien der Anwendung.<br /><br />Der Inhalt dieses Verzeichnisses können werden zur Verfügung gestellt, die dem Benutzer über iTunes Dateifreigabe (obwohl dies ist standardmäßig deaktiviert). Hinzufügen einer `UIFileSharingEnabled` Boolean-Taste, um die Datei "Info.plist", um Benutzern Zugriff auf diese Dateien ermöglichen.<br /><br />Auch wenn eine Anwendung nicht sofort nicht Dateifreigabe aktivieren, vermeiden Sie Ablegen von Dateien, die Ihre Benutzer in diesem Verzeichnis ausgeblendet werden soll (z. B. Datenbankdateien, es sei denn, Sie beabsichtigen, diese freizugeben). Als vertrauliche Dateien ausgeblendet bleiben, diese Dateien nicht verfügbar gemacht werden (und möglicherweise verschoben, geänderte oder gelöschte von iTunes) Wenn die Freigabe von Dateien in einer zukünftigen Version aktiviert ist.<br /><br /> Sie können die `Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments)` Methode, um den Pfad zu dem Verzeichnis "Dokumente" für Ihre Anwendung abzurufen.<br /><br />Der Inhalt dieses Verzeichnisses werden vom iTunes gesichert.|
|Bibliothek /|Das Verzeichnis der Bibliothek ist eine gute zum Speichern von Dateien, die nicht direkt vom Benutzer, z. B. Datenbanken oder andere Anwendung generierte Dateien erstellt werden. Der Inhalt dieses Verzeichnisses werden nie an den Benutzer über iTunes verfügbar gemacht.<br /><br />Sie können eigene Unterverzeichnisse in der Bibliothek erstellen. Es gibt jedoch bereits einige vom System erstellten Verzeichnisse hier, dass Sie von, z. B. Voreinstellungen und Caches berücksichtigen sollten.<br /><br />Der Inhalt dieses Verzeichnisses (mit Ausnahme der Caches Unterverzeichnis) werden vom iTunes gesichert. Benutzerdefinierte Verzeichnisse, die Sie in der Bibliothek erstellen, werden gesichert.|
|Bibliothek/Voreinstellungen /|Anwendungsspezifische Einstellungsdateien werden in diesem Verzeichnis gespeichert. Diese Dateien nicht direkt erstellt werden. Verwenden Sie stattdessen die `NSUserDefaults` Klasse.<br /><br />Der Inhalt dieses Verzeichnisses werden vom iTunes gesichert.|
|Bibliothek/Caches /|Das Caches-Verzeichnis ist, führen Sie ein guter Ausgangspunkt für Datendateien gespeichert, die Ihrer Anwendung behilflich sein können, aber, die können einfach neu erstellt werden, falls erforderlich. Die Anwendung sollte erstellen und diese Dateien nach Bedarf löschen und diese Dateien bei Bedarf neu erstellt werden. Ios5 kann auch diese Dateien (unter äußerst niedrig Speicher Situationen) löschen, jedoch kein Wert zurückgegeben wird, während die Anwendung ausgeführt wird.<br /><br />Der Inhalt dieses Verzeichnisses nicht vom iTunes, d., sie sind nicht vorhanden h., wenn der Benutzer ein Gerät wiederherstellt, gesichert werden, und sie können möglicherweise nicht vorhanden, nachdem eine aktualisierte Version der Anwendung installiert ist.<br /><br />Für den Fall, dass Ihre Anwendung keine Verbindung mit dem Netzwerk herstellen kann, können Sie das Verzeichnis Caches verwenden, zum Speichern von Daten oder Dateien, um eine gute offline Erfahrung zu bieten. Die Anwendung speichern und diese Daten schnell beim Warten auf Antworten Netzwerk abrufen kann, aber es muss nicht gesichert werden und kann problemlos werden wiederhergestellt oder neu erstellt werden, nachdem eine Wiederherstellung oder der Version aktualisieren.|
|tmp/|Anwendungen können temporäre Dateien speichern, die nur für einen kurzen Zeitraum in diesem Verzeichnis erforderlich sind. Um Speicherplatz zu sparen, sollten die Dateien gelöscht werden, wenn sie nicht mehr benötigt werden. Das Betriebssystem kann auch Dateien von diesem Verzeichnis gelöscht, wenn eine Anwendung nicht ausgeführt wird.<br /><br />Der Inhalt dieses Verzeichnisses werden vom iTunes nicht gesichert.<br /><br />Beispielsweise kann die Tmp-Verzeichnis verwendet, werden zum Speichern temporärer Dateien, die für die Anzeige für den Benutzer (z. B. Twitter Avatare oder e-Mail-Anlagen) heruntergeladen werden, jedoch gelöscht werden konnte, nachdem sie haben wurde angezeigt (und erneut heruntergeladen werden, wenn sie in der Zukunft erforderlich sind ).|

Diese bildschirmabbildung zeigt die Verzeichnisstruktur, geben Sie im Finder:

 [![](file-system-images/08-library-directory.png "Diese bildschirmabbildung zeigt die Verzeichnisstruktur, geben Sie im Finder")](file-system-images/08-library-directory.png#lightbox)

 <a name="Accessing_Other_Directories_Programmatically" />

### <a name="accessing-other-directories-programmatically"></a>Programmgesteuerter Zugriff auf andere Verzeichnisse

Die früheren Verzeichnis- und Dateinamen Beispiele, die Zugriff auf die `Documents` Verzeichnis. In ein anderes Verzeichnis schreiben, müssen Sie erstellen einen Pfad mithilfe der ".." Syntax wie hier gezeigt:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var library = Path.Combine (documents, "..", "Library");
var filename = Path.Combine (library, "WriteToLibrary.txt");
File.WriteAllText(filename, "Write this text into a file in Library");
```

Erstellen eines Verzeichnisses ist sehr ähnlich:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var library = Path.Combine (documents, "..", "Library");
var directoryname = Path.Combine (library, "NewLibraryDirectory");
Directory.CreateDirectory(directoryname);
```

Pfade zu den `Caches` und `tmp` Verzeichnisse können wie folgt erstellt werden:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var cache = Path.Combine (documents, "..", "Library", "Caches");
var tmp = Path.Combine (documents, "..", "tmp");
```

 <a name="Sharing_Files_with_the_User_through_iTunes" />


## <a name="sharing-files-with-the-user-through-itunes"></a>Freigeben von Dateien mit dem Benutzer über iTunes

Benutzer können die Dateien im Verzeichnis "Dokumente" Ihre Anwendung zugreifen, indem Sie die Bearbeitung `Info.plist` und erstellen eine **Anwendung unterstützt iTunes Freigabe** (`UIFileSharingEnabled`) Eintrag in der **Quelle** Ansicht als hier gezeigt:

 [![](file-system-images/09-uifilesharingenabled-plist.png "Hinzufügen der Anwendung unterstützt iTunes-Freigabe-Eigenschaft")](file-system-images/09-uifilesharingenabled-plist.png#lightbox)

Diese Dateien können im iTunes zugegriffen werden, wenn das Gerät verbunden ist und der Benutzer wählt die `Apps` Registerkarte. Das folgende Bildschirmfoto zeigt z. B. die Dateien im ausgewählten freigegebenen über iTunes app:

 [![](file-system-images/10-itunes-file-sharing.png "Diese bildschirmabbildung zeigt die Dateien im ausgewählten freigegebenen über iTunes app")](file-system-images/10-itunes-file-sharing.png#lightbox)

Benutzer können nur die Elemente der obersten Ebene in diesem Verzeichnis über iTunes zugreifen. Sie können nicht den Inhalt der Unterverzeichnisse (obwohl sie können auf ihre Computer kopieren oder löschen) finden Sie unter. Beispielsweise können für GoodReader, PDF und epub-Dateien mit der Anwendung freigegeben werden, damit Benutzer auf ihre iOS-Geräten gelesen werden können.

Benutzer, die den Inhalt von ihren Ordner "Dokumente" nicht ändern können Probleme verursachen, wenn sie nicht sorgfältig sind. Ihre Anwendung sollte dies berücksichtigen und sind unempfindlich für destruktiven Updates der Ordner "Dokumente".

Der Beispielcode für diesen Artikel erstellt eine Datei und einen Ordner im Ordner "Dokumente" (in **SampleCode.cs**) und ermöglicht die Dateifreigabe, die der **"Info.plist"** Datei. Diese bildschirmabbildung zeigt, wie diese in iTunes angezeigt werden:

 [![](file-system-images/15-itunes-file-sharing-example.png "Diese Abbildung zeigt, wie die Dateien im iTunes dargestellt")](file-system-images/15-itunes-file-sharing-example.png#lightbox)

Finden Sie in der [arbeiten mit Bildern](~/ios/app-fundamentals/images-icons/index.md) Artikel finden Sie Informationen über das Festlegen von Symbolen für die Anwendung und für alle benutzerdefinierten Typen, die Sie erstellen.

Wenn die `UIFileSharingEnabled` Schlüssel ist "false" oder nicht vorhanden ist, und klicken Sie dann Dateifreigabe ist, wird standardmäßig deaktiviert und Benutzer werden nicht in der Lage, für die Interaktion mit Ihrem Documentsdirectory.

 <a name="Backup_and_Restore" />

## <a name="backup-and-restore"></a>Sichern und Wiederherstellen

Wenn ein Gerät von iTunes gesichert wird, werden alle Verzeichnisse im Stammverzeichnis Ihrer Anwendung erstellt mit Ausnahme der folgenden gespeichert:

-   **[ApplicationName] .app** – das Anwendungspaket *ist* gesichert, jedoch nur, wenn das Paket geändert wurde (Wenn z. B. ein Anwendungsupdate installiert ist). Dieses Verzeichnis darf nicht trotzdem, geändert werden, da es signiert ist und daher nach der Installation unverändert bleiben müssen. 
-   **Bibliothek/Caches** – richtet sich an das Cacheverzeichnis für ordnungsgemäße Dateien, die nicht gesichert werden müssen. 
-   **TMP** – dieses Verzeichnis wird verwendet, für die temporären Dateien, die erstellt und gelöscht, wenn nicht mehr benötigt werden, oder für Dateien löscht, iOS, wenn Speicherplatz benötigt. 


Eine große Datenmenge sichern, kann sehr lange dauern. Wenn Sie sich entscheiden, müssen Sie bestimmten Dokument oder die Daten sichern, die Anwendung sollte nur verwenden die Dokumente und die Bibliothek für diesen Ordner. Verwenden Sie für vorübergehende Daten oder Dateien, die einfach aus dem Netzwerk abgerufen werden können die Caches oder der Tmp-Verzeichnis.

Denken Sie daran, dass iOS "im Dateisystem" bereinigt wird, wenn ein Gerät nur noch sehr wenig Speicherplatz auf dem Datenträger ausgeführt wird. Dieser Vorgang entfernt alle Dateien aus der Bibliothek-Caches und Tmp Ordner von Anwendungen, die derzeit nicht ausgeführt wird.

 <a name="Complying_with_iOS5_iCloud_Backup_Restrictions" />

## <a name="complying-with-ios5-icloud-backup-restrictions"></a>Einhaltung iOS5 iCloud Sicherung Einschränkungen

Apple eingeführt *iCloud Sicherung* -Funktionalität mit iOS 5. Wenn iCloud-Sicherung aktiviert ist, alle Dateien im Basisverzeichnis der Anwendung (Ausschließen von Verzeichnissen, die normalerweise nicht gestartet wurde, z. B. die app-Bündel gesichert werden `Caches` und `tmp`) sind gesicherte mit iCloud-Servern. Diese Funktion bietet den Benutzer mit einer vollständigen Sicherung Fall, dass ihr Gerät verloren geht, gestohlen oder beschädigt wird.

Da iCloud nur 5Gb Speicherplatz "free" auf jeden Benutzer bietet, und um zu vermeiden, dabei unnötig Bandbreite, Apple-Anwendungen sichern wichtigen vom Benutzer generierte Daten erwartet. Zur Einhaltung der iOS-Speicherrichtlinien Daten sollten Sie die Menge der Daten beschränken, die gesichert Ruft ab, durch die folgenden Elemente einhalten:

-  Speichern Sie nur vom Benutzer generierte Daten oder Daten, die andernfalls neu erstellt, in das Verzeichnis "Dokumente" werden können (das gesicherte ist). 
-  Speichern Sie alle anderen Daten, die einfach neu erstellt oder erneut im heruntergeladen `Library/Caches` oder `tmp` (d. h. nicht gesichert, und konnte "bereinigt"). 
-  Wenn bestimmte Dateien, die möglicherweise für die `Library/Caches` oder `tmp` Ordners, jedoch nicht möchten "bereinigt werden" aus, die an anderer Stelle zu speichern (z. B. `Library/YourData` ) und wenden Sie die "werden nicht gesichert einrichten" Attribut, um zu verhindern, dass die Dateien mit iCloud Sicherung Ba Ndwidth und Speicherplatz. Diese Daten verwendet weiterhin Speicherplatz auf dem Gerät, damit Sie sorgfältig verwalten und löschen Sie sie nach Möglichkeit sollten. 

Die "führen Sie nicht sichern einrichten"-Attribut festgelegt ist, mit der `NSFileManager` Klasse. Sicherstellen, dass die Klasse ist `using Foundation` , und rufen Sie `SetSkipBackupAttribute` wie folgt:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var filename = Path.Combine (documents, "LocalOnly.txt");
File.WriteAllText(filename, "This file will never get backed-up. It would need to be re-created after a restore or re-install");
NSFileManager.SetSkipBackupAttribute (filename, true); // backup will be skipped for this file
```

Wenn `SetSkipBackupAttribute` ist `true` die Datei werden nicht gesichert, unabhängig von dem Verzeichnis sich in befindet (auch das `Documents` Verzeichnis). Sie können Abfragen, die mithilfe des Attributs der `GetSkipBackupAttribute` Methode, und Sie können es zurücksetzen durch Aufrufen der `SetSkipBackupAttribute` Methode mit `false`, wie folgt:

```csharp
NSFileManager.SetSkipBackupAttribute (filename, false); // file will be backed-up
```

## <a name="sharing-data-between-ios-apps-and-app-extensions"></a>Freigeben von Daten zwischen iOS-Apps und App-Erweiterungen

Da App-Erweiterungen als Teil einer hostanwendung (im Gegensatz zu ihren enthaltenden app) ausgeführt werden, nicht die gemeinsame Nutzung von Daten automatisch enthalten, sodass zusätzliche Arbeit erforderlich ist. App-Gruppen sind, dass iOS Mechanismus verwendet, damit andere apps Daten gemeinsam nutzen. Wenn die Anwendungen ordnungsgemäß mit den richtigen Berechtigungen konfiguriert wurden, und bereitstellen, können sie ein freigegebenes Verzeichnis außerhalb ihrer normalen iOS Sandbox zugreifen.

### <a name="configure-an-app-group"></a>Konfigurieren Sie ein App-Gruppe

Mit der freigegebene Speicherort konfiguriert ein [App-Gruppe](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19), die konfiguriert ist, der **Zertifikate "," Bezeichner "und" Profile** Abschnitt [iOS Dev Center](https://developer.apple.com/devcenter/ios/). Dieser Wert muss auch in jedem Projekt verwiesen werden **Entitlements.plist**.

Informationen zum Erstellen und konfigurieren ein App-Gruppe finden Sie unter der [App Gruppenverwaltungsfunktionen](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) Handbuch.

### <a name="files"></a>Dateien

Die iOS-app und die Erweiterung können auch Freigeben von Dateien verwenden einen gemeinsamen Dateipfad (erhält sie wurden ordnungsgemäß mit den richtigen Berechtigungen konfiguriert und Bereitstellung):

```csharp
var FileManager = new NSFileManager ();
var appGroupContainer =FileManager.GetContainerUrl ("group.com.xamarin.WatchSettings");
var appGroupContainerPath = appGroupContainer.Path

Console.WriteLine ("Group Path: " + appGroupContainerPath);

// use the path to create and update files
...
```
> [!IMPORTANT]
> Wenn die Gruppe Pfad zurückgegeben wird `null`, überprüfen Sie die Konfiguration die Berechtigungen und die provisioning-Profil, und stellen Sie sicher, dass diese richtig sind.


<a name="Application_Version_Updates" />

## <a name="application-version-updates"></a>-Anwendung Versionsupdates

Wenn eine neue Version der Anwendung heruntergeladen wird, wird IOS-erstellt ein neues home-Verzeichnis und neue Anwendungspaket darin gespeichert. iOS verschiebt klicken Sie dann die folgenden Ordnern aus der vorherigen Version von Ihrem Anwendungspaket zum neuen Basisverzeichnis:

-  **Dokumente**
-  **Bibliothek**


Andere Verzeichnisse können auch über kopiert und unter Ihrem neuen Basisverzeichnis ablegen, aber sie haben nicht die Sicherheit kopiert werden, damit Ihre Anwendung nicht auf diesem Systemverhalten basieren soll.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde gezeigt, Dateisystemvorgänge mit Xamarin.iOS so einfach wie bei jeder anderen .NET Anwendung sind. Außerdem den Sandkasten für die Anwendung eingeführt und untersucht die Sicherheitsrisiken, die ihren verursacht. Als Nächstes untersucht es das Konzept der ein Anwendungspaket an. Schließlich werden die speziellen Verzeichnisse für die Anwendung verfügbaren aufgelistet und ihre Rollen während Anwendungsupgrades und Sicherungen erläutert.


## <a name="related-links"></a>Verwandte Links

- [FileSystemSampleCode](https://developer.xamarin.com/samples/FileSystemSampleCode/)
- [Programmierhandbuch für Datei-System](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/FileSystemProgrammingGUide/Introduction/Introduction.html)
- [Registrieren die Datei Typen Ihrer App unterstützt](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html#/apple_ref/doc/uid/TP40010411-SW1)
