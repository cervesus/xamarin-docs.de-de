---
title: Dateisystemzugriff in Xamarin.iOS
description: Dieses Dokument beschreibt, wie Sie mit dem Dateisystem in Xamarin.iOS arbeiten. Es wird erläutert, Verzeichnisse, die beim Lesen der Dateien, XML und JSON-Serialisierung, der Sandbox der Anwendung, Freigeben von Dateien über iTunes und vieles mehr.
ms.prod: xamarin
ms.assetid: 37DF2F38-901E-8F8E-269A-5EE0CCD28C08
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 11/12/2018
ms.openlocfilehash: 09e05fcfe10a994e14aa605b203ea67efae80d62
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61393614"
---
# <a name="file-system-access-in-xamarinios"></a>Dateisystemzugriff in Xamarin.iOS

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/FileSystemSampleCode/)

Können Sie Xamarin.iOS und `System.IO` Klassen in der *.NET Basisklassenbibliothek (BCL)* auf das iOS-Dateisystem zugreifen. Mithilfe der `File`-Klasse können Sie Dateien erstellen, löschen und lesen, und mit der `Directory`-Klasse können Sie den Inhalt der Verzeichnisse erstellen, löschen oder auflisten. Sie können auch `Stream` Unterklassen, die ein höheres Maß an Kontrolle über Dateioperationen (z. B. Komprimierung oder Position Suchen innerhalb einer Datei) bereitstellen können.

iOS gelten einige Einschränkungen, in denen Sie eine Anwendung mit dem Dateisystem, um die Sicherheit der Daten einer Anwendung zu erhalten, und Benutzer vor bösartige apps schützen kann. Diese Einschränkungen sind Teil der *Sandbox der Anwendung* – eine Reihe von Regeln, die einer Anwendung den Zugriff auf Dateien, Einstellungen, Netzwerkressourcen, Hardware usw. beschränkt. Eine Anwendung ist beschränkt auf das Lesen und Schreiben von Dateien in ihrem Basisverzeichnis (Installationsort); Es kann nicht auf eine andere Anwendung Dateien zugreifen.

iOS enthält auch einige spezifische dateifeatures: bestimmte Verzeichnisse erfordern eine besondere Behandlung in Bezug auf Sicherungen und Upgrades und Anwendungen können auch Dateien mit jeweils anderen freigeben und die **Dateien** app (ab iOS 11), und über iTunes.

Dieser Artikel behandelt die Features und Einschränkungen des iOS-Dateisystem und eine beispielanwendung, die zeigt, wie Sie Xamarin.iOS zu verwenden, um einige einfache Dateisystemvorgänge auszuführen:

[![Ein Beispiel für iOS, die einige einfache Dateisystemvorgänge ausführen](file-system-images/01-sampleapp-sml.png)](file-system-images/01-sampleapp.png#lightbox)

## <a name="general-file-access"></a>Allgemeine Dateizugriff

Xamarin.iOS ermöglicht die Verwendung der .NET `System.IO` Klassen, die für Dateisystemvorgänge unter iOS.

Die folgenden Codeausschnitte veranschaulichen einige häufig verwendete Dateivorgänge. Finden Sie alle unter Ihnen, in der **SampleCode.cs** -Datei in der beispielanwendung für diesen Artikel.

### <a name="working-with-directories"></a>Arbeiten mit Verzeichnissen

Dieser Code Listet die Unterverzeichnisse im aktuellen Verzeichnis (angegeben durch die ". /" Parameter), gibt den Speicherort der ausführbaren Datei für Ihre Anwendung.
Die Ausgabe wird eine Liste aller Dateien und Ordnern, die mit Ihrer Anwendung (angezeigt im Konsolenfenster angezeigt, während des Debuggens) bereitgestellt werden.

```csharp
var directories = Directory.EnumerateDirectories("./");
foreach (var directory in directories) {
      Console.WriteLine(directory);
}
```

### <a name="reading-files"></a>Lesen von Dateien

Um eine Textdatei zu lesen, benötigen Sie nur eine einzige Zeile Code. In diesem Beispiel wird der Inhalt einer Textdatei im Ausgabefenster der Anwendung angezeigt.

```csharp
var text = File.ReadAllText("TestData/ReadMe.txt");
Console.WriteLine(text);
```

### <a name="xml-serialization"></a>XML-Serialisierung

Obwohl die vollständige Verwendung `System.Xml` Namespace sprengt den Rahmen dieses Artikels sprengen, Sie Deserialisieren können einfach ein XML-Dokument aus dem Dateisystem mithilfe ein StreamReader, wie in diesem Codeausschnitt:

```csharp
using (TextReader reader = new StreamReader("./TestData/test.xml")) {
      XmlSerializer serializer = new XmlSerializer(typeof(MyObject));
      var xml = (MyObject)serializer.Deserialize(reader);
}
```

Weitere Informationen finden Sie in der Dokumentation für ["System.xml"](xref:System.Xml) und [Serialisierung](xref:System.Xml.Serialization). Finden Sie unter den [Xamarin.iOS-Dokumentation](~/ios/deploy-test/linker.md) zum Linker – häufig Sie benötigen zum Hinzufügen der `[Preserve]` -Attribut auf Klassen, die serialisiert werden soll.

### <a name="creating-files-and-directories"></a>Erstellen von Dateien und Verzeichnissen

Dieses Beispiel zeigt, wie Sie mit der `Environment` Klasse auf den Ordner "Dokumente", in dem wir Dateien und Verzeichnisse erstellen können.

```csharp
var documents =
 Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments); 
var filename = Path.Combine (documents, "Write.txt");
File.WriteAllText(filename, "Write this text into a file");
```

Erstellen ein Verzeichnis ist ein ähnlicher Prozess:

```csharp
var documents =
 Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var directoryname = Path.Combine (documents, "NewDirectory");
Directory.CreateDirectory(directoryname);
```

Weitere Informationen finden Sie unter den [System.IO-API-Referenz](xref:System.IO).

### <a name="serializing-json"></a>Serialisieren von JSON

[Json.NET](http://www.newtonsoft.com/json) ist ein Hochleistungs-JSON-Framework, die mit Xamarin.iOS funktioniert und ist unter NuGet verfügbar. Fügen Sie das NuGet-Paket für Ihre Anwendung-Projekt aus mithilfe **NuGet hinzufügen** in Visual Studio für Mac:

[![](file-system-images/json01.png "Die Applikationen-Projekt hinzugefügt das NuGet-Paket")](file-system-images/json01.png#lightbox)

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
        public string Email { get; set; }
        public bool Active { get; set; }
        public DateTime CreatedDate { get; set; }
        public List<string> Roles { get; set; }

        public Account() {
        }
    }
}
```

Abschließend erstellen Sie eine Instanz von der `Account` Klasse, auf den JSON-Daten serialisieren und in eine Datei zu schreiben:

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

Weitere Informationen zum Arbeiten mit JSON-Daten in einer .NET-Anwendung finden Sie unter Json .NET [Dokumentation](http://www.newtonsoft.com/json/help).

## <a name="special-considerations"></a>Besonderheiten

Trotz der Ähnlichkeit zwischen Xamarin.iOS und .NET-Dateivorgänge, iOS und Xamarin.iOS unterscheiden sich in einigen wichtigen Aspekten von .NET.

### <a name="making-project-files-accessible-at-runtime"></a>Zur Laufzeit für Projektdateien jedermann

In der Standardeinstellung, wenn Sie eine Datei zu Ihrem Projekt hinzufügen es nicht in die endgültige Assembly einbezogen, und daher nicht mehr für Ihre Anwendung verfügbar. Um eine Datei in die Assembly einschließen möchten, müssen Sie sie mit einer speziellen Buildaktion, Inhalt namens kennzeichnen.

Um eine Datei für die Aufnahme zu markieren, mit der rechten Maustaste auf die Dateien, und wählen Sie **Buildvorgang &gt; Content** in Visual Studio für Mac. Sie können auch ändern, die **Buildvorgang** in der Datei **Eigenschaften** Blatt.

### <a name="case-sensitivity"></a>Groß-/Kleinschreibung

Es ist wichtig zu verstehen, dass das iOS-Dateisystem ist *Groß-/Kleinschreibung*. Instanzenmethode bedeutet, dass die Datei- und Verzeichnisnamen genau – übereinstimmen müssen **"Readme.txt"** und **"Readme.txt"** unterschiedliche Dateinamen betrachtet werden.

Dies ist möglicherweise für .NET Entwickler, die immer vertrauter mit dem Windows-Dateisystem, handelt es sich verwirrend *Groß-/Kleinschreibung* – **Dateien**, **Dateien**, und  **Dateien** würde in dasselbe Verzeichnis verweisen.

> [!WARNING]
> Die iOS-Simulator kein ist Groß-/Kleinschreibung beachtet.
> Wenn sich Ihre Filename Groß-/Kleinschreibung zwischen der Datei selbst und die Verweise auf ihn im Code unterscheidet, tritt immer noch der Code im Simulator funktionieren möglicherweise jedoch ein auf einem echten Gerät. Dies ist einer der Gründe, warum es wichtig ist, bereitstellen und Testen auf einem echten Gerät frühzeitig und häufig während der iOS-Entwicklung.

### <a name="path-separator"></a>Pfadtrennzeichen

iOS verwendet einen Schrägstrich "/" als Pfadtrennzeichen (unterscheidet sich vom Windows, die verwendet den umgekehrten Schrägstrich ' \').

Aufgrund dieses Unterschieds verwirrend, die es wird empfohlen, mit der `System.IO.Path.Combine` -Methode, die für die aktuelle Plattform statt hartcodierung einer bestimmten Pfadtrennzeichen angepasst wird. Dies ist eine einfache Schritt, der Ihren Code für andere Plattformen einfacher macht.

## <a name="application-sandbox"></a>Sandbox der Anwendung

Ihre Anwendung auf das Dateisystem (und andere Ressourcen wie die Features für Netzwerk- und Hardwareproblemen) ist aus Sicherheitsgründen beschränkt. Diese Einschränkung wird als bezeichnet die *Sandbox der Anwendung*. Im Hinblick auf das Dateisystem kann Ihre Anwendung zu erstellen und Löschen von Dateien und Verzeichnissen in ihrem Basisverzeichnis beschränkt.

Das Basisverzeichnis ist ein eindeutiger Speicherort im Dateisystem, in denen Ihre Anwendung und alle darin enthaltenen Daten gespeichert werden. Sie können nicht den Speicherort des Basisverzeichnisses für Ihre Anwendung auswählen (oder ändern); iOS- und Xamarin.iOS Eigenschaften und Methoden zum Verwalten von Dateien und Verzeichnisse innerhalb jedoch ermöglichen.

## <a name="the-application-bundle"></a>Anwendungspaket

Die *Anwendungsbündel* ist der Ordner, der Ihre Anwendung enthält.
Es ist von anderen Ordnern, dass das Suffix ". app" in den Namen des Verzeichnisses unterschieden. Ihr Anwendungspaket enthält Ihrer ausführbaren Datei und den gesamten Inhalt (Dateien, Bilder usw.) für Ihr Projekt erforderlich sind.

Wenn Sie zu Ihrem Anwendungspaket unter Mac OS navigieren, wird Sie durch ein anderes Symbol, als Sie in anderen Verzeichnissen finden Sie unter (und die **.app** Suffix wird ausgeblendet,), aber es ist nur ein regulärer Verzeichnis, das das Betriebssystem anzeigt unterschiedlich.

Zum Anzeigen der Anwendungspaket für den Beispielcode mit der Maustaste, auf das Projekt im **Visual Studio für Mac** , und wählen Sie **im Finder zeigen**. Navigieren Sie dann auf die **"bin" /** Verzeichnis, in dem Sie ein Anwendungssymbols feststellen werden (ähnlich wie im folgenden Screenshot).

![Navigieren Sie durch das Bin-Verzeichnis ein Anwendungssymbol ähnlich wie in diesem Screenshot gefunden.](file-system-images/40-bundle.png)

Mit der rechten Maustaste auf das Symbol, und wählen Sie **Paketinhalt anzeigen** um den Inhalt des Verzeichnisses Anwendungspaket zu suchen. Der Inhalt wird genau wie der Inhalt von einem regulären Verzeichnis angezeigt, wie hier gezeigt:

[![Der Inhalt der app-Bundle](file-system-images/45-bundle-sml.png)](file-system-images/45-bundle.png#lightbox)

Das Anwendungspaket ist, was auf dem Simulator oder auf Ihrem Gerät installiert ist, während der Tests, und letztlich liegt es in was für die Aufnahme in die App-Store an Apple gesendet wird.

## <a name="application-directories"></a>Anwendungsverzeichnisse

Wenn Ihre Anwendung auf einem Gerät installiert ist, wird das Betriebssystem ein Basisverzeichnis für Ihre Anwendung erstellt, und erstellt eine Reihe von Verzeichnissen im Stammverzeichnis Anwendung, die für die Verwendung verfügbar sind. Seit iOS 8, den Benutzer zugänglichen Verzeichnisse sind [nicht gefunden](https://developer.apple.com/library/ios/technotes/tn2406/_index.html) innerhalb des Anwendungsstammverzeichnisses, sodass Sie können nicht abgeleitet werden die Pfade für das anwendungsbündel aus den Verzeichnissen nach Benutzer (oder umgekehrt).

Diese Verzeichnisse, wie Sie ihren Pfad und ihren Zweck zu ermitteln, sind nachfolgend aufgeführt:

&nbsp;

|Verzeichnis|Beschreibung|
|---|---|
|[ApplicationName].app/|**In iOS 7 und früher**, dies ist die `ApplicationBundle` Verzeichnis, in dem die ausführbare Datei Ihrer Anwendung gespeichert ist. Die Verzeichnisstruktur, die Sie in Ihrer app zu erstellen, die in diesem Verzeichnis (z. B. Bilder und andere Dateitypen, die Sie als Ressourcen in Ihrer Visual Studio für Mac-Projekt gekennzeichnet haben) vorhanden ist.<br /><br />Wenn Sie die Inhaltsdateien in Ihr Anwendungspaket zugreifen möchten, steht der Pfad zu diesem Verzeichnis über die `NSBundle.MainBundle.BundlePath` Eigenschaft.|
|Dokumente /|Verwenden Sie dieses Verzeichnis, um Benutzerdokumente und Anwendungsdaten zu speichern.<br /><br />Der Inhalt dieses Verzeichnisses können zur Verfügung gestellt, die dem Benutzer über iTunes Dateifreigabe (auch wenn diese Option standardmäßig deaktiviert ist). Hinzufügen einer `UIFileSharingEnabled` Boolean-Taste, um die Datei "Info.plist", um Benutzern Zugriff auf diese Dateien zu ermöglichen.<br /><br />Selbst, wenn eine Anwendung sofort Dateifreigabe kann nicht, sollten Sie vermeiden, Ablegen von Dateien, die von Ihren Benutzern in diesem Verzeichnis ausgeblendet werden soll (z. B. Datenbankdateien, es sei denn, Sie sie freigeben möchten). Als vertrauliche Dateien ausgeblendet bleiben, diese Dateien nicht verfügbar gemacht werden (und möglicherweise verschoben, geänderte oder gelöschte von iTunes) Wenn die Dateifreigabe in einer zukünftigen Version aktiviert ist.<br /><br /> Sie können die `Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments)` Methode, um den Pfad zu dem Verzeichnis "Dateien" für Ihre Anwendung abzurufen.<br /><br />Der Inhalt dieses Verzeichnisses werden von iTunes gesichert.|
|Bibliothek /|Das Verzeichnis für die Bibliothek ist eine gute Möglichkeit zum Speichern von Dateien, die nicht direkt vom Benutzer, z. B. Datenbanken oder anderen Anwendung generierte Dateien erstellt werden. Der Inhalt dieses Verzeichnisses werden nie an den Benutzer über iTunes verfügbar gemacht.<br /><br />Sie können Ihre eigenen Unterverzeichnisse in Bibliothek erstellen. Es gibt jedoch noch einige vom System erstellten Verzeichnisse hier, dass Sie von, z. B. Einstellungen und Caches berücksichtigen sollten.<br /><br />Der Inhalt dieses Verzeichnisses (mit Ausnahme der Caches Unterverzeichnis) werden von iTunes gesichert. Benutzerdefinierte Verzeichnisse, die Sie in der Bibliothek erstellen werden gesichert werden.|
|Library/Preferences /|Anwendungsspezifische Einstellungsdateien werden in diesem Verzeichnis gespeichert. Erstellen Sie diese Dateien nicht direkt auf. Verwenden Sie stattdessen die `NSUserDefaults` Klasse.<br /><br />Der Inhalt dieses Verzeichnisses werden von iTunes gesichert.|
|Library/Caches/|Das Verzeichnis des Caches ist ein guter Ausgangspunkt zum Speichern von Datendateien, mit die Ihre Anwendung können ausgeführt, aber, die einfach neu erstellt werden kann. Die Anwendung erstellen und löschen Sie diese Dateien nach Bedarf und in der Lage, diese Dateien bei Bedarf neu erstellen. iOS 5 kann auch diese Dateien (Situationen Speicher), löschen, jedoch kein Wert zurückgegeben wird, während die Anwendung ausgeführt wird.<br /><br />Der Inhalt dieses Verzeichnisses nicht vom iTunes, das bedeutet, dass sie nicht vorhanden, wenn der Benutzer ein Gerät wiederherstellt, gesichert werden, und sie möglicherweise nicht vorhanden, nachdem eine aktualisierte Version der Anwendung installiert ist.<br /><br />Für den Fall, dass Ihre Anwendung keine Verbindung mit dem Netzwerk herstellen kann, können Sie beispielsweise das Verzeichnis des Caches verwenden, zum Speichern von Daten oder Dateien, um ein gutes offline zu ermöglichen. Kann die Anwendung zu speichern und diese Daten schnell abgerufen, beim Warten auf Netzwerk-Antworten, jedoch nicht gesichert werden müssen und können leicht wiederhergestellt oder werden neu erstellt werden, nachdem eine Wiederherstellung oder eine Version aktualisiert.|
|tmp/|Anwendungen können das Speichern temporärer Dateien, die nur für einen kurzen Zeitraum in diesem Verzeichnis erforderlich sind. Um Speicherplatz zu sparen, sollten die Dateien gelöscht werden, wenn sie nicht mehr benötigt werden. Das Betriebssystem kann auch Dateien aus diesem Verzeichnis löschen, wenn eine Anwendung nicht ausgeführt wird.<br /><br />Der Inhalt dieses Verzeichnisses werden von iTunes nicht gesichert.<br /><br />Beispielsweise kann das Tmp Verzeichnis verwendet zum Speichern von temporärer Dateien, die für die Anzeige für Benutzer (z. B. Twitter Avatare oder e-Mail-Anlagen) heruntergeladen werden, jedoch konnte, die gelöscht werden, sobald sie haben angezeigt (und wurden erneut heruntergeladen werden, wenn sie in der Zukunft erforderlich sind) .|

Dieser Screenshot zeigt die Verzeichnisstruktur, in einem finderfenster:

[![](file-system-images/08-library-directory.png "Dieser Screenshot zeigt die Verzeichnisstruktur, in einem finderfenster")](file-system-images/08-library-directory.png#lightbox)

### <a name="accessing-other-directories-programmatically"></a>Programmgesteuerter Zugriff auf andere Verzeichnisse

Die früheren Verzeichnis- und Beispiele, die Zugriff auf die `Documents` Verzeichnis. Um zu einem anderen Verzeichnis zu schreiben, müssen Sie erstellen, einen Pfad mithilfe der ".." Syntax wie hier gezeigt:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var library = Path.Combine (documents, "..", "Library");
var filename = Path.Combine (library, "WriteToLibrary.txt");
File.WriteAllText(filename, "Write this text into a file in Library");
```

Erstellen eines Verzeichnisses entspricht:

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

## <a name="sharing-with-the-files-app"></a>Die app Dateien freigeben

iOS 11 eingeführte der **Dateien** app – ein Datei-Browser für iOS, kann der Benutzer anzeigen und ihre zugehörigen Dateien in iCloud interagieren, die von jeder Anwendung, die es unterstützt auch gespeichert. Damit um die Benutzer auf Dateien in Ihrer app direkt zugreifen zu können, erstellen Sie einen neuen booleschen Schlüssel in der **"Info.plist"** Datei `LSSupportsOpeningDocumentsInPlace` und legen ihn auf `true`, wie hier:

![LSSupportsOpeningDocumentsInPlace in "Info.plist" festlegen](file-system-images/51-supports-opening.png)

Der app **Dokumente** Verzeichnis zur Verfügung, zum Durchsuchen in die **Dateien** app. In der **Dateien** -app, navigieren Sie zu **auf Mein iPhone** und jede app mit freigegebenen Dateien werden angezeigt. Die folgenden Screenshots zeigen, was die [FileSystem-Beispiel-app](https://developer.xamarin.com/samples/monotouch/FileSystemSampleCode/) wie folgt aussieht:

![app für iOS 11-Dateien](file-system-images/50-files-app-1-sml.png) ![Mein iPhone Dateien durchsuchen](file-system-images/50-files-app-2-sml.png) ![Beispiel-app-Dateien](file-system-images/50-files-app-3-sml.png)

## <a name="sharing-files-with-the-user-through-itunes"></a>Freigeben von Dateien mit dem Benutzer über iTunes

Benutzer können die Dateien im Verzeichnis "Dateien" Ihrer Anwendung zugreifen, indem Sie die Bearbeitung `Info.plist` und das Erstellen einer **Anwendung, die iTunes-Freigaben unterstützt** (`UIFileSharingEnabled`) Eintrag in der **Quelle** Zeigen Sie an, wie hier gezeigt:

[![Hinzufügen der Anwendung unterstützt iTunes-Freigabe-Eigenschaft](file-system-images/09-uifilesharingenabled-plist-sml.png)](file-system-images/09-uifilesharingenabled-plist.png#lightbox)

Diese Dateien können in iTunes zugegriffen werden, wenn das Gerät verbunden ist und des Benutzers die `Apps` Registerkarte. Der folgende Screenshot zeigt beispielsweise die Dateien in der ausgewählten app, die über iTunes freigegeben:

[![Dieser Screenshot zeigt die Dateien in der ausgewählten app, die über iTunes freigegeben](file-system-images/10-itunes-file-sharing-sml.png)](file-system-images/10-itunes-file-sharing.png#lightbox)

Benutzer können nur die Elemente der obersten Ebene in diesem Verzeichnis über iTunes zugreifen. Sie können nicht den Inhalt der alle Unterverzeichnisse (obwohl sie können auf ihren Computer kopieren oder löschen) finden Sie unter. Beispielsweise können GoodReader, PDF- und EPUB-Dateien mit der Anwendung freigegeben werden, damit Benutzer diese auf ihren iOS-Geräten lesen können.

Benutzer, die den Inhalt von ihren Ordnern "Dokumente" ändern, können Probleme verursachen, wenn sie nicht vorsichtig sind. Ihre Anwendung sollte dies berücksichtigen und sind unempfindlich für destruktive Updates der Ordner "Dokumente".

Der Beispielcode für diesen Artikel erstellt eine Datei und einen Ordner im Ordner "Dokumente" (in **SampleCode.cs**) und die Dateifreigabe ermöglicht die **"Info.plist"** Datei. Dieser Screenshot zeigt, wie diese in iTunes angezeigt werden:

[![Dieser Screenshot zeigt, wie die Dateien in iTunes angezeigt werden](file-system-images/15-itunes-file-sharing-example-sml.png)](file-system-images/15-itunes-file-sharing-example.png#lightbox)

Finden Sie in der [arbeiten mit Bildern](~/ios/app-fundamentals/images-icons/index.md) Weitere Informationen über das Festlegen von Symbolen für die Anwendung und für alle benutzerdefinierten Dokumenttypen, die Sie erstellen.

Wenn die `UIFileSharingEnabled` Schlüssel ist falsch oder nicht vorhanden ist, dann teilen der Datei ist, wird standardmäßig deaktiviert, und Benutzer werden nicht in der Lage, für die Interaktion mit Ihrem Verzeichnis "Dateien".

## <a name="backup-and-restore"></a>Sichern und Wiederherstellen

Wenn ein Gerät von iTunes gesichert wird, ausgenommen alles, was die Verzeichnisse im Stammverzeichnis Ihrer Anwendung erstellten gespeichert werden, die folgenden Verzeichnisse:

- **[ApplicationName] .app** – nicht zu diesem Verzeichnis schreiben, da es signiert ist und daher nach der Installation unverändert bleiben muss. Sie enthält eventuell Ressourcen, die Sie vom Code aus zugreifen, aber sie erfordern keine Sicherung, da sie zum Herunterladen der app erneut wiederhergestellt werden soll.
- **Bibliothek/Caches** – richtet sich an das Cacheverzeichnis für Arbeitsdateien, die nicht gesichert werden müssen.
- **TMP** : Dieses Verzeichnis wird verwendet, für die temporären Dateien, die erstellt und gelöscht werden, wenn nicht mehr benötigt werden, oder für Dateien löscht, iOS, wenn Speicherplatz benötigt.

Eine große Datenmenge sichern, kann sehr lange dauern. Wenn Sie sich entscheiden, müssen Sie keine bestimmten Dokument oder die Daten sichern, sollte Ihre Anwendung entweder mit den Dokumenten verwenden und Bibliothek-Ordner. Verwenden Sie für vorübergehende Daten oder Dateien, die einfach über das Netzwerk abgerufen werden können die Caches oder der Tmp-Verzeichnis.

> [!NOTE]
> iOS wird "im Dateisystem clean" Wenn ein Gerät nur noch wenig Speicherplatz auf dem Datenträger ausgeführt wird.
> Dieser Vorgang entfernt alle Dateien aus der Bibliothek/Caches und Tmp Ordner von Anwendungen, die derzeit nicht ausgeführt wird.

## <a name="complying-with-ios-5-icloud-backup-restrictions"></a>Einhaltung der iOS 5 iCloud-backup-Einschränkungen

> [!NOTE]
> Diese Richtlinie wurde zwar mit iOS 5 (die vor langer Zeit scheint) eingeführt wurden die Anweisungen immer noch relevant für apps noch heute.

Apple eingeführt *iCloud-Sicherung* -Funktionalität mit iOS 5. Wenn der iCloud-Sicherung aktiviert ist, alle Dateien im Stammverzeichnis Ihrer Anwendung (ausgenommen Verzeichnisse, die nicht normal, z. B. das app-Bundle, gesichert werden `Caches`, und `tmp`) sind Sicherung auf iCloud-Server. Dieses Feature bietet dem Benutzer eine vollständige Sicherung aus, falls ihr Gerät verloren geht, gestohlen oder beschädigt ist.

Da iCloud nur 5 Gb "Speicherplatz" für jeden Benutzer und vermeiden unnötig Bandbreite bereitstellt, wird von Apple Anwendungen nur wichtige Benutzern generierte Daten erwartet. Um mit dem iOS-Speicherrichtlinien für Daten zu erfüllen, sollten Sie die Menge der Daten einschränken, die gesichert werden, durch Nutzung der folgenden Elemente:

- Speichern Sie nur von Benutzern generierte Daten oder Daten, die sonst neu erstellt, in das Verzeichnis "Dateien" können nicht (die gesichert wird).
- Alle anderen Daten, die einfach neu erstellt oder erneut heruntergeladen werden können Store `Library/Caches` oder `tmp` (das ist nicht gesichert und kann "bereinigt").
- Wenn Sie über Dateien verfügen, die möglicherweise für die `Library/Caches` oder `tmp` Ordner, sondern Sie möchten nicht "bereinigt werden" und speichert Sie sie an anderer Stelle (wie z. B. `Library/YourData`) und wenden Sie die "werden nicht zurück einrichten" Attribut, um zu verhindern, dass die Dateien regelmäßig iCloud-Sicherung Bandbreite und Speicherplatz. Diese Daten verwendet weiterhin Speicherplatz auf dem Gerät, damit Sie ihn sorgfältig verwalten und löschen Sie sie nach Möglichkeit sollte.

Der "werden nicht zurück um"-Attribut festgelegt ist, mit der `NSFileManager` Klasse. Sicherstellen, dass die Klasse ist `using Foundation` , und rufen Sie `SetSkipBackupAttribute` wie folgt aus:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var filename = Path.Combine (documents, "LocalOnly.txt");
File.WriteAllText(filename, "This file will never get backed-up. It would need to be re-created after a restore or re-install");
NSFileManager.SetSkipBackupAttribute (filename, true); // backup will be skipped for this file
```

Wenn `SetSkipBackupAttribute` ist `true` die Datei werden nicht gesichert wurden, unabhängig davon, sich im befindet, Verzeichnis (auch auf dem `Documents` Verzeichnis). Können Sie Abfragen, bei der Verwendung des Attributs der `GetSkipBackupAttribute` Methode, und Sie können es zurücksetzen durch Aufrufen der `SetSkipBackupAttribute` -Methode mit `false`, wie folgt aus:

```csharp
NSFileManager.SetSkipBackupAttribute (filename, false); // file will be backed-up
```

## <a name="sharing-data-between-ios-apps-and-app-extensions"></a>Freigeben von Daten zwischen iOS-apps und app-Erweiterungen

Da App-Erweiterungen als Teil einer hostanwendung (im Gegensatz zu ihren enthaltenden app) ausgeführt werden, erfolgt nicht die Freigabe von Daten automatisch enthalten, sodass zusätzliche Arbeit erforderlich ist. App-Gruppen sind, dass der Mechanismus iOS verwendet, um verschiedene apps zum Freigeben von Daten zu ermöglichen. Wenn die Anwendungen ordnungsgemäß konfiguriert, die für die richtigen Berechtigungen und Bereitstellung erfüllt sind, können sie ein freigegebenes Verzeichnis außerhalb ihrer normalen iOS-Sandbox zugreifen.

### <a name="configure-an-app-group"></a>Konfigurieren Sie eine App-Gruppe

Der freigegebene Speicherort erfolgt über eine [App-Gruppe](https://developer.apple.com/library/archive/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19), die konfiguriert wird, der **Zertifikate, Bezeichner & Profile** im Abschnitt [iOS Developer Center](https://developer.apple.com/devcenter/ios/). Dieser Wert muss außerdem in jedem Projekt auf die verwiesen werden **"Entitlements.plist"**.

Informationen zum Erstellen und konfigurieren eine App-Gruppe finden Sie in der [App-Gruppen-Funktionen](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) Guide.

### <a name="files"></a>Dateien

Die iOS-app und die Erweiterung können auch Freigeben von Dateien, die über einen gemeinsamen Dateipfad (erhalten sie wurden ordnungsgemäß konfiguriert, die für die richtigen Berechtigungen und Bereitstellung):

```csharp
var FileManager = new NSFileManager ();
var appGroupContainer =FileManager.GetContainerUrl ("group.com.xamarin.WatchSettings");
var appGroupContainerPath = appGroupContainer.Path

Console.WriteLine ("Group Path: " + appGroupContainerPath);

// use the path to create and update files
...
```

> [!IMPORTANT]
> Wenn der Pfad für die Gruppe zurückgegeben wird `null`, überprüfen Sie die Konfiguration von den Berechtigungen und das Bereitstellungsprofil und stellen Sie sicher, dass diese richtig sind.

## <a name="application-version-updates"></a>Version von Anwendungsupdates

Wenn eine neue Version der Anwendung heruntergeladen wird, wird iOS erstellt ein neues home-Verzeichnis und das neue Anwendungspaket darin gespeichert. iOS verschiebt klicken Sie dann die folgenden Ordnern aus der vorherigen Version Ihres Application Bundles an Ihrem neuen home-Verzeichnis:

- **Dokumente**
- **Bibliothek**

Andere Verzeichnisse können auch über kopiert und unter Ihrem neuen home-Verzeichnis gestellt werden, aber sie sind nicht notwendigerweise kopiert werden, damit Ihre Anwendung nicht auf diesem Systemverhalten verlassen sollten.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben, dass Dateisystemvorgänge mit Xamarin.iOS an eine beliebige andere .NET-Anwendung ähnlich sind. Außerdem der Sandbox der Anwendung eingeführt und untersucht die negative Auswirkungen auf die Sicherheit die fehlschlägt. Als Nächstes untersucht es das Konzept der ein Anwendungspaket. Schließlich werden die speziellen Verzeichnisse, die für Ihre Anwendung aufgelistet und ihre Rollen während des Anwendungsupgrades und Sicherungen erläutert.

## <a name="related-links"></a>Verwandte Links

- [Dateisystem-Beispielcode](https://developer.xamarin.com/samples/FileSystemSampleCode/)
- [Programmierhandbuch für Datei-System](https://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/FileSystemProgrammingGUide/Introduction/Introduction.html)
- [Registrieren die Datei Typen Ihrer App unterstützt](https://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html#/apple_ref/doc/uid/TP40010411-SW1)
