---
title: Dateisystem Zugriff in xamarin. IOS
description: In diesem Dokument wird beschrieben, wie Sie in xamarin. IOS mit dem Dateisystem arbeiten. Es werden Verzeichnisse, das Lesen von Dateien, die XML-und JSON-Serialisierung, die Anwendungs Sandbox, das Freigeben von Dateien über iTunes und mehr erläutert.
ms.prod: xamarin
ms.assetid: 37DF2F38-901E-8F8E-269A-5EE0CCD28C08
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 11/12/2018
ms.openlocfilehash: e52f9abb31090f3acc361eb5a3f9ae2e12600b36
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68653519"
---
# <a name="file-system-access-in-xamarinios"></a>Dateisystem Zugriff in xamarin. IOS

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/ios-samples/filesystemsamplecode)

Sie können xamarin. IOS und die `System.IO` Klassen in der .net- *Basisklassen Bibliothek (BCL)* verwenden, um auf das IOS-Dateisystem zuzugreifen. Mithilfe der `File`-Klasse können Sie Dateien erstellen, löschen und lesen, und mit der `Directory`-Klasse können Sie den Inhalt der Verzeichnisse erstellen, löschen oder auflisten. Sie können auch unter `Stream` Klassen verwenden, die ein höheres Maß an Kontrolle über Datei Vorgänge bereitstellen können (z. b. Komprimierung oder Positions Suche innerhalb einer Datei).

IOS erzwingt einige Einschränkungen, die eine Anwendung mit dem Dateisystem ausführen kann, um die Sicherheit der Anwendungsdaten zu erhalten und Benutzer vor bösartigen apps zu schützen. Diese Einschränkungen sind Teil der *Anwendungs Sandbox* – ein Satz von Regeln, die den Zugriff einer Anwendung auf Dateien, Einstellungen, Netzwerkressourcen, Hardware usw. einschränken. Eine Anwendung ist auf das Lesen und Schreiben von Dateien innerhalb Ihres Basisverzeichnisses (installierter Speicherort) beschränkt. der Zugriff auf die Dateien einer anderen Anwendung ist nicht möglich.

IOS verfügt auch über einige Dateisystem spezifische Features: bestimmte Verzeichnisse erfordern eine besondere Behandlung in Bezug auf Sicherungen und Upgrades, und Anwendungen können auch Dateien untereinander **und die Datei** -app (seit IOS 11) und über iTunes gemeinsam nutzen.

In diesem Artikel werden die Features und Einschränkungen des IOS-Dateisystems beschrieben. es enthält eine Beispielanwendung, die veranschaulicht, wie mit xamarin. IOS einige einfache Dateisystem Vorgänge ausgeführt werden:

[![Ein Beispiel für IOS, das einige einfache Dateisystem Vorgänge ausführt](file-system-images/01-sampleapp-sml.png)](file-system-images/01-sampleapp.png#lightbox)

## <a name="general-file-access"></a>Allgemeiner Dateizugriff

Xamarin. IOS ermöglicht die Verwendung der .net `System.IO` -Klassen für Dateisystem Vorgänge unter IOS.

Die folgenden Code Ausschnitte veranschaulichen einige gängige Datei Vorgänge. Sie finden Sie unten in der **SampleCode.cs** -Datei in der Beispielanwendung für diesen Artikel.

### <a name="working-with-directories"></a>Arbeiten mit Verzeichnissen

Dieser Code listet die Unterverzeichnisse im aktuellen Verzeichnis auf (angegeben durch den Parameter "./"), wobei es sich um den Speicherort der ausführbaren Datei der Anwendung handelt.
Bei der Ausgabe handelt es sich um eine Liste aller Dateien und Ordner, die mit Ihrer Anwendung bereitgestellt werden (im Konsolenfenster angezeigt, während Sie Debuggen).

```csharp
var directories = Directory.EnumerateDirectories("./");
foreach (var directory in directories) {
      Console.WriteLine(directory);
}
```

### <a name="reading-files"></a>Lesen von Dateien

Um eine Textdatei zu lesen, benötigen Sie nur eine einzige Codezeile. In diesem Beispiel wird der Inhalt einer Textdatei im Fenster "Anwendungs Ausgabe" angezeigt.

```csharp
var text = File.ReadAllText("TestData/ReadMe.txt");
Console.WriteLine(text);
```

### <a name="xml-serialization"></a>XML-Serialisierung

Obwohl die Arbeit mit dem `System.Xml` gesamten Namespace über den Rahmen dieses Artikels hinausgeht, können Sie ein XML-Dokument problemlos aus dem Dateisystem deserialisieren, indem Sie einen StreamReader wie diesen Code Ausschnitt verwenden:

```csharp
using (TextReader reader = new StreamReader("./TestData/test.xml")) {
      XmlSerializer serializer = new XmlSerializer(typeof(MyObject));
      var xml = (MyObject)serializer.Deserialize(reader);
}
```

Weitere Informationen finden Sie in der Dokumentation zu [System. XML](xref:System.Xml) und [Serialisierung](xref:System.Xml.Serialization). Weitere Informationen finden Sie in der [xamarin. IOS-Dokumentation](~/ios/deploy-test/linker.md) auf dem Linker – oft müssen `[Preserve]` Sie das-Attribut zu Klassen hinzufügen, die Sie serialisieren möchten.

### <a name="creating-files-and-directories"></a>Erstellen von Dateien und Verzeichnissen

Dieses Beispiel zeigt, wie Sie die `Environment` -Klasse verwenden, um auf den Ordner "Dokumente" zuzugreifen, in dem Dateien und Verzeichnisse erstellt werden können.

```csharp
var documents =
 Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments); 
var filename = Path.Combine (documents, "Write.txt");
File.WriteAllText(filename, "Write this text into a file");
```

Das Erstellen eines Verzeichnisses ist ein ähnlicher Prozess:

```csharp
var documents =
 Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var directoryname = Path.Combine (documents, "NewDirectory");
Directory.CreateDirectory(directoryname);
```

Weitere Informationen finden Sie in der [System.IO-API-Referenz](xref:System.IO).

### <a name="serializing-json"></a>Serialisieren von JSON

[JSON.net](http://www.newtonsoft.com/json) ist ein leistungsfähiges JSON-Framework, das mit xamarin. IOS funktioniert und für nuget verfügbar ist. Fügen Sie dem Anwendungsprojekt das nuget-Paket hinzu, indem **Sie nuget in Visual Studio für Mac hinzufügen** :

[![](file-system-images/json01.png "Hinzufügen des nuget-Pakets zum Anwendungsprojekt")](file-system-images/json01.png#lightbox)

Fügen Sie als nächstes eine Klasse hinzu, die als Datenmodell für die Serialisierung/Deserialisierung fungiert ( `Account.cs`in diesem Fall):

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

Erstellen Sie abschließend eine Instanz der `Account` -Klasse, serialisieren Sie Sie in JSON-Daten, und schreiben Sie Sie in eine Datei:

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

Weitere Informationen zum Arbeiten mit JSON-Daten in einer .NET-Anwendung finden Sie in der [Dokumentation](http://www.newtonsoft.com/json/help)zu JSON. net.

## <a name="special-considerations"></a>Besonderheiten

Trotz der Ähnlichkeiten zwischen xamarin. IOS-und .NET-Datei Vorgängen unterscheiden sich IOS und xamarin. IOS in einigen wichtigen Punkten von .net.

### <a name="making-project-files-accessible-at-runtime"></a>Zugreifen auf Projektdateien zur Laufzeit

Wenn Sie dem Projekt eine Datei hinzufügen, wird diese standardmäßig nicht in die endgültige Assembly eingeschlossen und ist daher für Ihre Anwendung nicht verfügbar. Wenn Sie eine Datei in die Assembly einschließen möchten, müssen Sie Sie mit einer speziellen Buildaktion markieren, die als Inhalt bezeichnet wird.

Um eine Datei für die Einbindung zu markieren, klicken Sie mit der rechten Maustaste auf die Datei **&gt;** (en), und wählen Sie in Visual Studio für Mac buildaktionsinhalt. Sie können auch die Buildaktion auf der **Eigenschaften** Seite der Datei ändern.

### <a name="case-sensitivity"></a>Groß-/Kleinschreibung

Es ist wichtig zu wissen, dass beim IOS-Dateisystem die *groß-* /Kleinschreibung beachtet wird. Berücksichtigung der Groß-/Kleinschreibung bedeutet, dass die Datei-und Verzeichnisnamen exakt übereinstimmen müssen – "Infodatei **. txt** " und "Infodatei **. txt** " werden als andere Dateinamen angesehen

Dies könnte für .NET-Entwickler verwirrend sein, die mit dem Windows-Dateisystem vertraut sind, *bei* dem die Groß-/Kleinschreibung nicht beachtet wird – **Dateien**, **Dateien**und **Dateien** würden alle auf dasselbe Verzeichnis verweisen.

> [!WARNING]
> Beim IOS-Simulator wird die Groß-/Kleinschreibung nicht beachtet.
> Wenn die Schreibweise des Datei namens zwischen der Datei selbst und den verweisen darauf im Code abweicht, kann Ihr Code im Simulator weiterhin verwendet werden, schlägt jedoch auf einem echten Gerät fehl. Dies ist einer der Gründe, warum es wichtig ist, früh und häufig während der IOS-Entwicklung auf einem tatsächlichen Gerät bereitzustellen und zu testen.

### <a name="path-separator"></a>Pfad Trennzeichen

IOS verwendet den Schrägstrich "/" als Pfad Trennzeichen (unterscheidet sich von Windows, der den umgekehrten Schrägstrich "\" verwendet).

Aufgrund dieses verwirrenden Unterschieds empfiehlt es sich, die `System.IO.Path.Combine` -Methode zu verwenden, die sich für die aktuelle Plattform anpasst, anstatt ein bestimmtes Pfad Trennzeichen hart zu codieren. Dies ist ein einfacher Schritt, der Ihren Code besser auf andere Plattformen portierbar macht.

## <a name="application-sandbox"></a>Anwendungs Sandbox

Der Zugriff auf das Dateisystem (und andere Ressourcen, z. b. die Netzwerk-und Hardware Features) Ihrer Anwendung ist aus Sicherheitsgründen eingeschränkt. Diese Einschränkung wird als *Anwendungs Sandbox*bezeichnet. Im Hinblick auf das Dateisystem ist die Anwendung auf das Erstellen und Löschen von Dateien und Verzeichnissen in Ihrem Basisverzeichnis beschränkt.

Das Basisverzeichnis ist ein eindeutiger Speicherort im Dateisystem, in dem Ihre Anwendung und alle Ihre Daten gespeichert werden. Der Speicherort des Basisverzeichnisses für Ihre Anwendung kann nicht ausgewählt (oder geändert) werden. IOS und xamarin. IOS stellen jedoch Eigenschaften und Methoden bereit, um die Dateien und Verzeichnisse in zu verwalten.

## <a name="the-application-bundle"></a>Das Anwendungspaket

Das *Anwendungspaket* ist der Ordner, der Ihre Anwendung enthält.
Sie wird von anderen Ordnern unterschieden, indem dem Verzeichnisnamen das Suffix. app hinzugefügt wird. Ihr Anwendungspaket enthält die ausführbare Datei und alle Inhalte (Dateien, Bilder usw.), die für Ihr Projekt erforderlich sind.

Wenn Sie in Mac OS zu Ihrem Anwendungspaket navigieren, wird es mit einem anderen Symbol angezeigt als in anderen Verzeichnissen (und das Suffix " **. app** " ist ausgeblendet). Es handelt sich jedoch lediglich um ein reguläres Verzeichnis, das vom Betriebssystem unterschiedlich angezeigt wird.

Um das Anwendungspaket für den Beispielcode anzuzeigen, klicken Sie in **Visual Studio für Mac** mit der rechten Maustaste auf das Projekt, und wählen Sie **in Finder**anzeigen aus. Navigieren Sie dann zum Verzeichnis " **bin/** Verzeichnis", in dem Sie ein Anwendungssymbol finden sollten (ähnlich wie im nachfolgenden Screenshot).

![Navigieren Sie durch das bin-Verzeichnis, um ein Anwendungssymbol zu finden, das diesem Screenshot ähnelt.](file-system-images/40-bundle.png)

Klicken Sie mit der rechten Maustaste auf dieses Symbol, und wählen Sie **Paket Inhalt anzeigen** aus, um den Inhalt des Anwendungspaket Verzeichnisses zu durchsuchen. Der Inhalt wird genau wie der Inhalt eines regulären Verzeichnisses angezeigt, wie hier gezeigt:

[![Der Inhalt des App Bundle](file-system-images/45-bundle-sml.png)](file-system-images/45-bundle.png#lightbox)

Das Anwendungspaket wird während des Tests auf dem Simulator oder auf Ihrem Gerät installiert, und letztendlich wird es an Apple zur Einbindung in den App Store übermittelt.

## <a name="application-directories"></a>Anwendungs Verzeichnisse

Wenn die Anwendung auf einem Gerät installiert ist, erstellt das Betriebssystem ein Basisverzeichnis für Ihre Anwendung und erstellt eine Reihe von Verzeichnissen im Stammverzeichnis der Anwendung, die zur Verwendung verfügbar sind. Seit IOS 8 befinden sich die vom Benutzer zugänglichen Verzeichnisse [nicht](https://developer.apple.com/library/ios/technotes/tn2406/_index.html) innerhalb des Anwendungs Stamms, sodass Sie die Pfade für das Anwendungspaket nicht aus den Benutzerverzeichnissen ableiten können (oder umgekehrt).

Diese Verzeichnisse, wie Sie Ihren Pfad ermitteln, und ihre Zwecke sind unten aufgeführt:

&nbsp;

|Verzeichnis|Beschreibung|
|---|---|
|[ApplicationName]. app/|**In ios 7 und früheren**Versionen ist dies das `ApplicationBundle` Verzeichnis, in dem die ausführbare Datei der Anwendung gespeichert ist. Die Verzeichnisstruktur, die Sie in Ihrer APP erstellen, ist in diesem Verzeichnis vorhanden (z. b. Bilder und andere Dateitypen, die Sie als Ressourcen in Ihrem Visual Studio für Mac Projekt gekennzeichnet haben).<br /><br />Wenn Sie auf die Inhalts Dateien innerhalb des Anwendungspakets zugreifen müssen, ist der Pfad zu diesem Verzeichnis über die `NSBundle.MainBundle.BundlePath` -Eigenschaft verfügbar.|
|Dokumenten|Verwenden Sie dieses Verzeichnis zum Speichern von Benutzer Dokumenten und Anwendungs Datendateien.<br /><br />Der Inhalt dieses Verzeichnisses kann dem Benutzer über die iTunes-Dateifreigabe zur Verfügung gestellt werden (obwohl dies standardmäßig deaktiviert ist). Fügen Sie `UIFileSharingEnabled` der Datei "Info. plist" einen booleschen Schlüssel hinzu, damit Benutzer auf diese Dateien zugreifen können.<br /><br />Auch wenn eine Anwendung die Dateifreigabe nicht sofort aktiviert, sollten Sie verhindern, dass Dateien, die für die Benutzer in diesem Verzeichnis ausgeblendet werden sollen (z. b. Datenbankdateien), nicht mehr zur Verfügung stehen. Solange sensible Dateien ausgeblendet bleiben, werden diese Dateien nicht verfügbar gemacht (und möglicherweise von iTunes verschoben, geändert oder gelöscht), wenn die Dateifreigabe in einer zukünftigen Version aktiviert ist.<br /><br /> Sie können die `Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments)` -Methode verwenden, um den Pfad zum Verzeichnis "Dokumente" für Ihre Anwendung zu erhalten.<br /><br />Der Inhalt dieses Verzeichnisses wird von iTunes gesichert.|
|Fern|Das Bibliotheksverzeichnis ist ein guter Ort, um Dateien zu speichern, die nicht direkt vom Benutzer erstellt werden, z. b. Datenbanken oder andere von der Anwendung generierte Dateien. Der Inhalt dieses Verzeichnisses wird dem Benutzer nie über iTunes zugänglich gemacht.<br /><br />Sie können eigene Unterverzeichnisse in der Bibliothek erstellen. Es gibt jedoch bereits einige vom System erstellte Verzeichnisse, die Sie kennen sollten, einschließlich Einstellungen und Caches.<br /><br />Der Inhalt dieses Verzeichnisses (mit Ausnahme des Unterverzeichnisses Caches) wird von iTunes gesichert. Benutzerdefinierte Verzeichnisse, die Sie in der Bibliothek erstellen, werden gesichert.|
|Bibliothek/Einstellungen/|Anwendungsspezifische Einstellungsdateien werden in diesem Verzeichnis gespeichert. Erstellen Sie diese Dateien nicht direkt. Verwenden Sie stattdessen die `NSUserDefaults` -Klasse.<br /><br />Der Inhalt dieses Verzeichnisses wird von iTunes gesichert.|
|Bibliothek/Caches/|Das Caches-Verzeichnis ist ein guter Ort zum Speichern von Datendateien, die Ihre Anwendung unterstützen, aber problemlos neu erstellt werden können. Die Anwendung sollte diese Dateien nach Bedarf erstellen und löschen und diese Dateien bei Bedarf neu erstellen können. IOS 5 kann diese Dateien auch löschen (in Situationen mit geringem Speicherplatz), dies ist jedoch nicht der Fall, während die Anwendung ausgeführt wird.<br /><br />Der Inhalt dieses Verzeichnisses wird nicht von iTunes gesichert. Dies bedeutet, dass Sie nicht vorhanden sind, wenn der Benutzer ein Gerät wiederherstellt, und Sie sind möglicherweise nicht vorhanden, nachdem eine aktualisierte Version der Anwendung installiert wurde.<br /><br />Wenn Ihre Anwendung beispielsweise keine Verbindung mit dem Netzwerk herstellen kann, können Sie das Caches-Verzeichnis verwenden, um Daten oder Dateien zu speichern, um ein gutes Offline Verhalten zu gewährleisten. Diese Daten können von der Anwendung beim Warten auf Netzwerk Antworten schnell gespeichert und abgerufen werden. Sie müssen jedoch nicht gesichert werden und können nach einem Wiederherstellungs-oder Versions Update problemlos wieder hergestellt oder neu erstellt werden.|
|tmp|Anwendungen können temporäre Dateien speichern, die nur für einen kurzen Zeitraum in diesem Verzeichnis benötigt werden. Um Speicherplatz zu sparen, sollten Dateien gelöscht werden, wenn Sie nicht mehr benötigt werden. Das Betriebssystem kann auch Dateien aus diesem Verzeichnis löschen, wenn eine Anwendung nicht ausgeführt wird.<br /><br />Der Inhalt dieses Verzeichnisses wird nicht von iTunes gesichert.<br /><br />Beispielsweise kann das tmp-Verzeichnis zum Speichern temporärer Dateien verwendet werden, die für die Anzeige für den Benutzer heruntergeladen werden (z. b. Twitter-Avatare oder e-Mail-Anlagen). Diese können jedoch gelöscht werden, sobald Sie angezeigt werden (und erneut heruntergeladen werden, wenn Sie in Zukunft benötigt werden). .|

Dieser Screenshot zeigt die Verzeichnisstruktur in einem Finder-Fenster:

[![](file-system-images/08-library-directory.png "Dieser Screenshot zeigt die Verzeichnisstruktur in einem Finder-Fenster")](file-system-images/08-library-directory.png#lightbox)

### <a name="accessing-other-directories-programmatically"></a>Programm gesteuertes zugreifen auf andere Verzeichnisse

In den vorherigen Verzeichnis-und Datei Beispielen `Documents` wurde auf das Verzeichnis zugegriffen. Um in ein anderes Verzeichnis zu schreiben, müssen Sie einen Pfad mit der Syntax ".." erstellen, wie hier gezeigt:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var library = Path.Combine (documents, "..", "Library");
var filename = Path.Combine (library, "WriteToLibrary.txt");
File.WriteAllText(filename, "Write this text into a file in Library");
```

Das Erstellen eines Verzeichnisses ist ähnlich:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var library = Path.Combine (documents, "..", "Library");
var directoryname = Path.Combine (library, "NewLibraryDirectory");
Directory.CreateDirectory(directoryname);
```

Pfade zu den `Caches` Verzeichnissen `tmp` und können wie folgt erstellt werden:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var cache = Path.Combine (documents, "..", "Library", "Caches");
var tmp = Path.Combine (documents, "..", "tmp");
```

## <a name="sharing-with-the-files-app"></a>Freigabe mit der App "Dateien"

IOS 11 hat die Datei **-App eingeführt** : einen Dateibrowser für IOS, mit dem Benutzer Ihre Dateien in icloud anzeigen und mit ihr interagieren und auch von einer beliebigen Anwendung, die Sie unterstützt, gespeichert werden können. Damit der Benutzer direkt auf Dateien in der App zugreifen kann, erstellen Sie einen neuen booleschen Schlüssel in der **Info. plist** -Datei `LSSupportsOpeningDocumentsInPlace` , und legen Sie ihn wie folgt auf `true`fest:

![Lssupportsopeningdocumentsinplace in "Info. plist" festlegen](file-system-images/51-supports-opening.png)

Das Verzeichnis " **Dokumente** " der APP steht nun in der **Datei** -App zum Durchsuchen zur Verfügung. Navigieren Sie in der **Datei** -APP **auf meinem iPhone** zu, und jede APP mit freigegebenen Dateien wird angezeigt. Die folgenden Screenshots zeigen, wie die [File System-Beispiel-App](https://docs.microsoft.com/samples/xamarin/ios-samples/filesystemsamplecode) aussieht:

![IOS 11-Dateien-App](file-system-images/50-files-app-1-sml.png) ![Meine iPhone-Dateien durchsuchen](file-system-images/50-files-app-2-sml.png) ![Beispiel-APP-Dateien](file-system-images/50-files-app-3-sml.png)

## <a name="sharing-files-with-the-user-through-itunes"></a>Freigeben von Dateien für den Benutzer über iTunes

Benutzer können auf die Dateien im Verzeichnis "Dokumente" Ihrer Anwendung zugreifen `Info.plist` , indem Sie eine Anwendung, die den iTunes`UIFileSharingEnabled`Sharing ()-Eintrag **unterstützt** , in der **Quell** Ansicht bearbeiten und erstellen, wie hier gezeigt:

[![Das Hinzufügen der Anwendung unterstützt die iTunes-Freigabe Eigenschaft](file-system-images/09-uifilesharingenabled-plist-sml.png)](file-system-images/09-uifilesharingenabled-plist.png#lightbox)

Diese Dateien können in iTunes aufgerufen werden, wenn das Gerät verbunden ist und der Benutzer die `Apps` Registerkarte auswählt. Der folgende Screenshot zeigt z. b. die Dateien in der ausgewählten APP, die über iTunes freigegeben wurde:

[![Dieser Screenshot zeigt die Dateien in ausgewählter APP, die über iTunes freigegeben wurde.](file-system-images/10-itunes-file-sharing-sml.png)](file-system-images/10-itunes-file-sharing.png#lightbox)

Benutzer können nur über iTunes auf die Elemente der obersten Ebene in diesem Verzeichnis zugreifen. Die Inhalte von Unterverzeichnissen können nicht angezeigt werden (Sie können Sie jedoch auf Ihren Computer kopieren oder löschen). Beispielsweise können mit goodreader PDF-und EPUB-Dateien für die Anwendung freigegeben werden, damit Benutzer Sie auf Ihren IOS-Geräten lesen können.

Benutzer, die den Inhalt Ihres Ordners "Dokumente" ändern, können Probleme verursachen, wenn Sie nicht vorsichtig sind. Die Anwendung sollte dies berücksichtigen und sich gegen zerstörerische Updates des Ordners "Dokumente" widerstandsfähig machen.

Der Beispielcode für diesen Artikel erstellt sowohl eine Datei als auch einen Ordner im Ordner "Documents" (in **SampleCode.cs**) und aktiviert die Dateifreigabe in der Datei " **Info. plist** ". Dieser Screenshot zeigt, wie diese in iTunes angezeigt werden:

[![Dieser Screenshot zeigt, wie die Dateien in iTunes angezeigt werden.](file-system-images/15-itunes-file-sharing-example-sml.png)](file-system-images/15-itunes-file-sharing-example.png#lightbox)

Informationen dazu, wie Sie Symbole für die Anwendung und benutzerdefinierte Dokumenttypen festlegen, die Sie erstellen, finden Sie im Artikel [Working with Images (Arbeiten mit Images](~/ios/app-fundamentals/images-icons/index.md) ).

Wenn der `UIFileSharingEnabled` Schlüssel falsch oder nicht vorhanden ist, wird die Dateifreigabe standardmäßig deaktiviert, und die Benutzer können nicht mit Ihrem Verzeichnis "Dokumente" interagieren.

## <a name="backup-and-restore"></a>Sichern und Wiederherstellen

Wenn ein Gerät von iTunes gesichert wird, werden alle Verzeichnisse, die im Basisverzeichnis Ihrer Anwendung erstellt werden, mit Ausnahme der folgenden Verzeichnisse gespeichert:

- **[ApplicationName]. app** – schreiben Sie in dieses Verzeichnis nicht, da es signiert ist und daher nach der Installation unverändert bleiben muss. Sie enthält möglicherweise Ressourcen, auf die Sie aus Ihrem Code zugreifen, aber Sie benötigen keine Sicherung, da Sie durch erneutes herunterladen der APP wieder hergestellt werden.
- **Bibliothek/Caches** – das Cache Verzeichnis ist für Arbeitsdateien vorgesehen, die nicht gesichert werden müssen.
- **tmp** – dieses Verzeichnis wird für temporäre Dateien verwendet, die erstellt und gelöscht werden, wenn Sie nicht mehr benötigt werden, oder für Dateien, die von IOS gelöscht werden, wenn Speicherplatz benötigt wird.

Das Sichern einer großen Datenmenge kann einige Zeit in Anspruch nehmen. Wenn Sie festlegen, dass Sie ein bestimmtes Dokument oder Daten sichern möchten, sollte Ihre Anwendung entweder die Ordner "Dokumente" und "Bibliothek" verwenden. Bei vorübergehenden Daten oder Dateien, die problemlos aus dem Netzwerk abgerufen werden können, verwenden Sie entweder die Caches oder das tmp-Verzeichnis.

> [!NOTE]
> IOS wird das Dateisystem bereinigen, wenn der Speicherplatz auf einem Gerät deutlich niedrig ist.
> Bei diesem Vorgang werden alle Dateien aus den Bibliotheken/Caches und dem tmp-Ordner von Anwendungen entfernt, die derzeit nicht ausgeführt werden.

## <a name="complying-with-ios-5-icloud-backup-restrictions"></a>Einhaltung der Einschränkungen für IOS 5-icloud-Sicherungen

> [!NOTE]
> Obwohl diese Richtlinie erstmals mit IOS 5 eingeführt wurde (was vor längerer Zeit scheint), ist die Anleitung für apps noch heute relevant.

Apple hat eine *icloud-Sicherungs* Funktion mit IOS 5 eingeführt. Wenn die icloud-Sicherung aktiviert ist, werden alle Dateien im Stammverzeichnis Ihrer Anwendung (ausgenommen Verzeichnisse, die normalerweise nicht gesichert werden, z. b. `Caches`die APP Bundle `tmp`, und) auf icloud-Servern gesichert. Diese Funktion bietet dem Benutzer eine komplette Sicherung für den Fall, dass das Gerät verloren geht, gestohlen wird oder beschädigt ist.

Da icloud nur 5 GB freien Speicherplatz für jeden Benutzer bereitstellt und die Bandbreite unnötig vermeiden kann, erwartet Apple, dass Anwendungen nur wichtige benutzergenerierte Daten sichern. Zur Einhaltung der IOS-Daten Speicherungs Richtlinien sollten Sie die Menge der zu sichernden Daten einschränken, indem Sie die folgenden Elemente einhalten:

- Speichern Sie nur benutzergenerierte Daten oder Daten, die anderweitig nicht neu erstellt werden können, im Verzeichnis "Dokumente" (das gesichert ist).
- Speichern Sie alle anderen Daten, die einfach neu erstellt oder heruntergeladen `Library/Caches` werden können, oder `tmp` (die nicht gesichert und möglicherweise "bereinigt" werden).
- Wenn Sie über Dateien verfügen, die möglicherweise für `Library/Caches` den `tmp` Ordner oder geeignet sind, aber nicht "bereinigt" werden sollen, speichern `Library/YourData`Sie Sie an einer anderen Stelle (z. b.), und wenden Sie das Attribut "nicht sichern" an, um zu verhindern, dass die Dateien die icloud-Sicherung verwenden. Bandbreite und Speicherplatz. Diese Daten verbraucht weiterhin Speicherplatz auf dem Gerät, sodass Sie Sie nach Möglichkeit sorgfältig verwalten und löschen sollten.

Das ' do not Backup '-Attribut wird mithilfe der `NSFileManager` -Klasse festgelegt. Stellen Sie sicher, `using Foundation` dass Ihre `SetSkipBackupAttribute` Klasse ist und wie folgt aufruft:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var filename = Path.Combine (documents, "LocalOnly.txt");
File.WriteAllText(filename, "This file will never get backed-up. It would need to be re-created after a restore or re-install");
NSFileManager.SetSkipBackupAttribute (filename, true); // backup will be skipped for this file
```

Wenn `SetSkipBackupAttribute` `Documents` ist `true` , wird die Datei unabhängig von dem Verzeichnis, in dem Sie gespeichert ist, nicht gesichert (auch das Verzeichnis). Sie können das-Attribut mithilfe der `GetSkipBackupAttribute` -Methode Abfragen, und Sie können es zurücksetzen `SetSkipBackupAttribute` , indem `false`Sie die-Methode mit wie folgt aufrufen:

```csharp
NSFileManager.SetSkipBackupAttribute (filename, false); // file will be backed-up
```

## <a name="sharing-data-between-ios-apps-and-app-extensions"></a>Freigeben von Daten zwischen IOS-apps und App-Erweiterungen

Da App-Erweiterungen als Teil einer Host Anwendung ausgeführt werden (im Gegensatz zu ihrer enthaltenden APP), ist die Datenfreigabe nicht automatisch eingeschlossen, sodass zusätzliche Arbeit erforderlich ist. App-Gruppen sind der Mechanismus, den IOS verwendet, um unterschiedlichen apps das Freigeben von Daten zu gestatten. Wenn die Anwendungen ordnungsgemäß mit den richtigen Berechtigungen und der Bereitstellung konfiguriert wurden, können Sie auf ein frei gegebenes Verzeichnis außerhalb ihrer normalen IOS-Sandbox zugreifen.

### <a name="configure-an-app-group"></a>Konfigurieren einer APP-Gruppe

Der freigegebene Speicherort wird mit einer [App-Gruppe](https://developer.apple.com/library/archive/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19)konfiguriert, die im Abschnitt Zertifikate, Bezeichner **& profile** im [IOS dev Center](https://developer.apple.com/devcenter/ios/)konfiguriert ist. Auf diesen Wert muss in den **Berechtigungen. plist**jedes Projekts verwiesen werden.

Informationen zum Erstellen und Konfigurieren einer APP-Gruppe finden Sie im Handbuch zu [App-Gruppenfunktionen](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) .

### <a name="files"></a>Dateien

Die IOS-APP und die Erweiterung können auch Dateien mit einem gemeinsamen Dateipfad freigeben (wenn Sie ordnungsgemäß mit den richtigen Berechtigungen und der Bereitstellung konfiguriert wurden):

```csharp
var FileManager = new NSFileManager ();
var appGroupContainer =FileManager.GetContainerUrl ("group.com.xamarin.WatchSettings");
var appGroupContainerPath = appGroupContainer.Path

Console.WriteLine ("Group Path: " + appGroupContainerPath);

// use the path to create and update files
...
```

> [!IMPORTANT]
> Wenn der zurückgegebene Gruppen Pfad `null`lautet, überprüfen Sie die Konfiguration der Berechtigungen und des Bereitstellungs Profils, und stellen Sie sicher, dass Sie korrekt sind.

## <a name="application-version-updates"></a>Updates der Anwendungs Version

Wenn eine neue Version der Anwendung heruntergeladen wird, erstellt IOS ein neues Basisverzeichnis und speichert das neue Anwendungs Bündel darin. anschließend verschiebt IOS die folgenden Ordner aus der vorherigen Version Ihres Anwendungspakets in das neue Basisverzeichnis:

- **Dokumente**
- **Bibliothek**

Andere Verzeichnisse können auch über das neue Basisverzeichnis kopiert und abgelegt werden, aber nicht unbedingt kopiert werden, sodass die Anwendung nicht auf dieses Systemverhalten angewiesen ist.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde gezeigt, dass Dateisystem Vorgänge mit xamarin. IOS allen anderen .NET-Anwendungen ähneln. Außerdem wurde die Anwendungs Sandbox eingeführt und die Auswirkungen auf die Sicherheit untersucht. Als nächstes wurde das Konzept eines Anwendungspakets untersucht. Schließlich wurden die spezialisierten Verzeichnisse aufgelistet, die für Ihre Anwendung verfügbar sind, und ihre Rollen wurden während Anwendungs Upgrades und-Sicherungen erläutert.

## <a name="related-links"></a>Verwandte Links

- [File System-Beispielcode](https://docs.microsoft.com/samples/xamarin/ios-samples/filesystemsamplecode)
- [Programmier Handbuch für Dateisysteme](https://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/FileSystemProgrammingGUide/Introduction/Introduction.html)
- [Registrieren der von Ihrer APP unterstützten Dateitypen](https://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html#/apple_ref/doc/uid/TP40010411-SW1)
