---
title: Dateien
description: "Mit Xamarin.Forms dateibearbeitung kann mithilfe eingebetteter Ressourcen oder Schreiben für das systemeigene Dateisystem-APIs erfolgen."
ms.topic: article
ms.prod: xamarin
ms.assetid: 9987C3F6-5F04-403B-BBB4-ECB024EA6CC8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/22/2017
ms.openlocfilehash: 605374c0f2bfe656e564e48d14ffe18ce5b7dfe5
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="files"></a>Dateien

_Mit Xamarin.Forms dateibearbeitung kann mithilfe eingebetteter Ressourcen oder Schreiben für das systemeigene Dateisystem-APIs erfolgen._

## <a name="overview"></a>Übersicht

Xamarin.Forms-Code wird auf mehreren Plattformen ausgeführt, von denen jede ihr eigenes Dateisystem besitzt. Dies bedeutet, dass beim Lesen und Schreiben von Dateien in den meisten ist einfach "Fertig" ändert die systemeigene Datei-APIs auf jeder Plattform verwenden. Alternativ sind eingebettete Ressourcen eine einfachere Lösung für Datendateien zu einer app zu verteilen.

Dieses Dokument behandelt die folgende Datei mit Allgemeine Behandlung von Szenarien:

-  [ **Dateien eingebettet als Ressourcen** ](#Loading_Files_Embedded_as_Resources) -Dateien, die als Teil einer Anwendung und geladenen mit der Reflektions-API bereitgestellt werden können.
-  [ **Speichern und Laden von Dateien** ](#Loading_and_Saving_Files) -Benutzer beschreibbaren Speicher kann es sich systemintern implementiert und Zugriff auf die dann mit der `DependencyService` .


Ein Drittanbieter-komponentenaufrufs **PCLStorage** zum Lesen und Schreiben von Dateien in den Benutzer zugänglich Speicher aus PCL Code eingesetzt werden können.

Informationen zur Behandlung von Bilddateien finden Sie unter der [arbeiten mit Bildern](~/xamarin-forms/user-interface/images.md) Seite.

<a name="Loading_Files_Embedded_as_Resources" />

## <a name="loading-files-embedded-as-resources"></a>Laden von Dateien eingebettet als Ressourcen

Einbetten eine Datei in eine **PCL** Assembly erstellen, oder fügen Sie eine Datei, und sicherstellen, dass **Buildvorgang: EmbeddedResource**.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![Konfigurieren von eingebettete Ressourcenbuildvorgang](files-images/vs-embeddedresource-sml.png "Einstellung EmbeddedResource BuildAction")](files-images/vs-embeddedresource.png "Einstellung EmbeddedResource BuildAction")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[ ![Textdatei eingebettet, in der PCL, konfigurieren eingebettete Ressourcenbuildvorgang](files-images/xs-embeddedresource-sml.png "Einstellung EmbeddedResource BuildAction")](files-images/xs-embeddedresource.png "Einstellung EmbeddedResource BuildAction")

-----

`GetManifestResourceStream` wird verwendet, um Zugriff auf die eingebettete Datei mithilfe der **Ressourcen-ID**. Standardmäßig die Ressourcen-ID der Dateiname, der den Standardnamespace für das eingebettet ist in - Projekt mit dem Präfix in diesem Fall die Assembly wird **WorkingWithFiles** und der Dateiname ist **PCLTextResource.txt**, Daher ist die Ressourcen-ID `WorkingWithFiles.PCLTextResource.txt`.

```csharp
var assembly = typeof(LoadResourceText).GetTypeInfo().Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.PCLTextResource.txt");
string text = "";
using (var reader = new System.IO.StreamReader (stream)) {
    text = reader.ReadToEnd ();
}
```

Die `text` Variable kann dann verwendet werden, um den Text anzuzeigen, oder verwenden sie andernfalls im Code. Diesen Screenshot mit der [Beispiel-app](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/) wird der Text gerendert wird, einem `Label` Steuerelement.

 [ ![Textdatei eingebettet, in der PCL](files-images/pcltext-sml.png "eingebettete Textdatei in PCL angezeigt, in der App")](files-images/pcltext.png "eingebettete Textdatei in PCL in-App angezeigt")

Laden und Deserialisieren von XML ist gleichermaßen einfach. Der folgende Code zeigt eine XML-Datei wird geladen und deserialisiert aus einer Ressource gebunden ein `ListView` für die Anzeige. Die XML-Datei enthält ein Array von `Monkey` Objekte (die Klasse wird im Beispielcode definiert).

```csharp
var assembly = typeof(LoadResourceText).GetTypeInfo().Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.PCLXmlResource.xml");
List<Monkey> monkeys;
using (var reader = new System.IO.StreamReader (stream)) {
    var serializer = new XmlSerializer(typeof(List<Monkey>));
    monkeys = (List<Monkey>)serializer.Deserialize(reader);
}
var listView = new ListView ();
listView.ItemsSource = monkeys;
```

 [ ![XML-Datei eingebettet, in der PCL, in der Listenansicht angezeigt](files-images/pclxml-sml.png "eingebettete XML-Datei in der PCL angezeigt, in der Listenansicht")](files-images/pclxml.png "eingebettete XML-Datei in der PCL, die in der Listenansicht angezeigt")

<a name="Embedding_in_Shared_Projects" />

### <a name="embedding-in-shared-projects"></a>Einbetten in gemeinsam genutzte Projekte

Shared-Projekte können auch Dateien als eingebettete Ressourcen enthalten, aber, da der Inhalt eines freigegebenen Projekts in der verweisenden Projekte kompiliert werden, das Präfix für Ressource "File" eingebetteten IDs nicht geändert werden können. Dies bedeutet, dass die Ressourcen-ID für jede eingebettete Datei für jede Plattform unterschiedlich sein kann.

Es gibt zwei Lösungen für dieses Problem mit freigegebenen Projekten:

-  **Synchronisieren Sie die Projekte** -bearbeiten Sie die Projekteigenschaften für jede Plattform, die **gleichen** Namen und Standardwerten Assembly-Namespace. Dieser Wert kann dann "hartcodiert" als Präfix für eingebettete Ressourcen-IDs im freigegebenen Projekt sein.
-  **Compiler-Direktiven #if** -Compiler-Direktiven verwenden, legen Sie das richtige Ressourcen-ID-Präfix und diesen Wert verwenden, um die richtige Ressourcen-ID dynamisch zu erstellen


Code, der mit einem Beispiel für die zweite Option wird unten gezeigt. Compiler-Direktiven werden verwendet, um das Präfix des hartcodiert Ressource auswählen (die normalerweise den Standardnamespace für das verweisende Projekt identisch ist). Die `resourcePrefix` Variable wird dann verwendet, um eine gültige Ressourcen-ID zu erstellen, indem Sie verketten es mit dem Dateinamen der eingebetteten Ressource.

```csharp
#if __IOS__
var resourcePrefix = "WorkingWithFiles.iOS.";
#endif
#if __ANDROID__
var resourcePrefix = "WorkingWithFiles.Droid.";
#endif
#if WINDOWS_PHONE
var resourcePrefix = "WorkingWithFiles.WinPhone.";
#endif

Debug.WriteLine("Using this resource prefix: " + resourcePrefix);
// note that the prefix includes the trailing period '.' that is required
var assembly = typeof(SharedPage).GetTypeInfo().Assembly;
Stream stream = assembly.GetManifestResourceStream
    (resourcePrefix + "SharedTextResource.txt");
```

<a name="Organizing_Resources" />

### <a name="organizing-resources"></a>Organisieren von Ressourcen

In den Beispielen oben wird davon ausgegangen, dass die Datei im Stammverzeichnis des PCL-Projekt eingebettet ist, in dem die Ressourcen-ID im Format wird **Namespace.Dateiname.Erweiterung**, wie z. B. `WorkingWithFiles.PCLTextResource.txt` und `WorkingWithFiles.iOS.SharedTextResource.txt`.

Es ist möglich, eingebettete Ressourcen in Ordnern zu organisieren. Wenn eine eingebettete Ressource in einem Ordner abgelegt wird, wird der Name des Ordners Teil der Ressourcen-ID (durch Punkte getrennt), so, dass die Ressourcen-ID-Format wird **Namespace.Folder.Filename.Extension**. Platzieren die Dateien in der Beispiel-app verwendet wird, in einen Ordner **MyFolder** würde die entsprechenden Ressourcen-IDs machen `WorkingWithFiles.MyFolder.PCLTextResource.txt` und `WorkingWithFiles.iOS.MyFolder.SharedTextResource.txt`.

<a name="Debugging_Embedded_Resources" />

### <a name="debugging-embedded-resources"></a>Debuggen von eingebetteten Ressourcen

Da in einigen Fällen es schwierig ist zu verstehen, warum eine bestimmte Ressource beansprucht wird nicht, kann die folgenden Debugcode vorübergehend hinzugefügt werden, zu einer Anwendung, um zu bestätigen, dass Ressourcen ordnungsgemäß konfiguriert sind. Es gibt alle bekannte Ressourcen, die in der angegebenen Assembly eingebettet der **Fehler** Pad-Ressource laden Probleme Debuggen.

```csharp
using System.Reflection;
// ...
// use for debugging, not in released app code!
var assembly = typeof(SharedPage).GetTypeInfo().Assembly;
foreach (var res in assembly.GetManifestResourceNames()) {
    System.Diagnostics.Debug.WriteLine("found resource: " + res);
}
```

<a name="Loading_and_Saving_Files" />

## <a name="saving-and-loading-files"></a>Speichern und Laden von Dateien

Da Xamarin.Forms auf mehrere Plattformen mit einem eigenen Dateisystem ausgeführt besteht keine einen Ansatz für das Laden und Speichern von Dateien, die vom Benutzer erstellt. Um zu veranschaulichen, wie das Speichern und laden die Beispiel-app einen Bildschirm enthält, der speichert und lädt einer Benutzereingabe - Textdateien wird nicht mehr benötigen Bildschirm unten gezeigt:

 [ ![Speichern und Laden von Text](files-images/saveandload-sml.png "speichern und Laden von Dateien in der App")](files-images/saveandload.png "speichern und Laden von Dateien in der App")

Jede Plattform verfügt über eine leicht abweichende Verzeichnisstruktur und anderen Filesystem-Funktionen – z. B. Xamarin.iOS und Xamarin.Android unterstützen die meisten `System.IO` Funktionalität jedoch Windows Phone unterstützt nur `IsolatedStorage` und [ `Windows.Storage` ](http://msdn.microsoft.com/library/windowsphone/develop/jj681698(v=vs.105).aspx) APIs.

Um dieses Problem zu umgehen, definiert die Beispiel-app eine Schnittstelle in der PCL Xamarin.Forms zum Laden und Speichern von Dateien. Es bietet eine einfache API zum Laden und Speichern von Textdateien, die auf dem Gerät gespeichert werden soll.

```csharp
public interface ISaveAndLoad {
    void SaveText (string filename, string text);
    string LoadText (string filename);
}
```

Die PCL code dann mithilfe der [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md) zum Abrufen eines Verweises auf eine systemeigene Implementierung der Schnittstelle. Dadurch wird die portablen Code, das Laden und Speichern von Dateien in jedes der plattformspezifischen Projekte geschriebene Klasse zu delegieren. Im Beispiel wird die **speichern** und **laden** Schaltflächen werden geschrieben, wie dargestellt:

```csharp
var saveButton = new Button {Text = "Save"};
saveButton.Clicked += (sender, e) => {
    DependencyService.Get<ISaveAndLoad>().SaveText("temp.txt", input.Text);
};
var loadButton = new Button {Text = "Load"};
loadButton.Clicked += (sender, e) => {
    output.Text = DependencyService.Get<ISaveAndLoad>().LoadText("temp.txt");
};
```

Eine Implementierung und muss an jede der plattformspezifischen Projekten hinzugefügt werden, bevor Dateien tatsächlich gespeichert und geladen werden können.

### <a name="ios-and-android"></a>iOS und Android

Die Implementierung der Schnittstelle für Xamarin.iOS und Xamarin.Android-Projekte kann identisch sein. Der Code unten angezeigt, einschließlich der `[assembly: Dependency (typeof (SaveAndLoad))]` Attribut ist erforderlich für die `DependencyService` arbeiten.

```csharp
[assembly: Dependency (typeof (SaveAndLoad))]
namespace WorkingWithFiles {
    public class SaveAndLoad : ISaveAndLoad {
        public void SaveText (string filename, string text) {
            var documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal);
            var filePath = Path.Combine (documentsPath, filename);
            System.IO.File.WriteAllText (filePath, text);
        }
        public string LoadText (string filename) {
            var documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal);
            var filePath = Path.Combine (documentsPath, filename);
            return System.IO.File.ReadAllText (filePath);
        }
    }
}
```

### <a name="universal-windows-platform-uwp-windows-81-and-windows-phone-81"></a>Universelle Windows-Plattform (UWP), Windows 8.1 und Windows Phone 8.1

Diese Plattformen haben Sie ein anderes Dateisystem-API – [ `Windows.Storage` ](/windows/uwp/files/quickstart-reading-and-writing-files/) – d. h. zum Speichern und Laden von Dateien verwendet.
Die `ISaveAndLoad` Schnittstelle implementiert werden kann, wie unten dargestellt:

```csharp
using System;
using System.Threading.Tasks;
using Windows.Storage;
using WinApp;
using WorkingWithFiles;
using Xamarin.Forms;

[assembly: Dependency(typeof(SaveAndLoad_WinApp))]

namespace WindowsApp
{
    // https://msdn.microsoft.com/library/windows/apps/xaml/hh758325.aspx
    public class SaveAndLoad_WinApp : ISaveAndLoad
    {
        public async Task SaveTextAsync(string filename, string text)
        {
            StorageFolder localFolder = ApplicationData.Current.LocalFolder;
            StorageFile sampleFile = await localFolder.CreateFileAsync(filename, CreationCollisionOption.ReplaceExisting);
            await FileIO.WriteTextAsync(sampleFile, text);
        }
        public async Task<string> LoadTextAsync(string filename)
        {
            StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
            StorageFile sampleFile = await storageFolder.GetFileAsync(filename);
            string text = await Windows.Storage.FileIO.ReadTextAsync(sampleFile);
            return text;
        }
    }
}
```


<a name="Saving_and_Loading_in_Shared_Projects" />

### <a name="saving-and-loading-in-shared-projects"></a>Speichern und Laden freigegebener Projekte

Da freigegebene Projekte Compilerdirektiven unterstützen ist es möglich, alle plattformspezifischen Code in eine einzelne Klassendatei im freigegebenen Projekt einzuschließen (ohne Verwendung der `DependencyService`).
Ein einzelnes `SaveAndLoad` Klasse konnten geschrieben werden, die beide Implementierungen, die oben genannten enthält in der verweisenden Projekte mithilfe von Compiler-Anweisungen, wie selektiv kompiliert `#if WINDOWS_PHONE`, `#if __IOS__`, und `#if __ANDROID__`.

## <a name="additional-information"></a>Zusätzliche Informationen

PCL-basierte Xamarin.Forms Projekte können auch nutzen die [PCLStorage NuGet](http://www.nuget.org/packages/pclstorage) ([Code &amp; Dokumentation](https://pclstorage.codeplex.com/)) auf eine Weise plattformübergreifende Dateivorgänge implementieren können.


## <a name="summary"></a>Zusammenfassung

Dieses Dokument hat einige einfache Dateivorgänge für eingebettete Ressourcen laden und speichern und Laden von Text auf dem Gerät angezeigt. Entwickler können implementieren ihre eigenen systemeigene Datei-APIs verwenden das `DependencyService`, somit so komplex wie erforderlich, um ihre dateibearbeitung Anforderungen zu verarbeiten.


## <a name="related-links"></a>Verwandte Links

- [FilesSample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/)
- [Erstellen, schreiben und Lesen einer Datei (UWP)](/windows/uwp/files/quickstart-reading-and-writing-files/)
- [Xamarin.Forms-Beispiele](https://github.com/xamarin/xamarin-forms-samples)
- [Dateien Arbeitsmappe](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/files/files.workbook)
