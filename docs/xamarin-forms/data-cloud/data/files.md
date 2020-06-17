---
Title: "Datei Behandlung in Xamarin.Forms " Description: "die Datei Behandlung mit Xamarin.Forms kann mithilfe von Code in einer .NET Standard Bibliothek oder mithilfe von eingebetteten Ressourcen erreicht werden."
ms. Prod: xamarin ms. assetid: 9987c3f6-5f04-403b-bbb4-ecb024ea6cc8 ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 06/21/2018 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="file-handling-in-xamarinforms"></a>Dateiverarbeitung inXamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfiles)

_Die Verarbeitung von Dateien mit Xamarin.Forms kann mithilfe von Code in einer .NET Standard Bibliothek oder mithilfe von eingebetteten Ressourcen erreicht werden._

## <a name="overview"></a>Übersicht

Xamarin.Formsder Code wird auf mehreren Plattformen ausgeführt, von denen jede über ein eigenes Dateisystem verfügt. Früher wurde das Lesen und Schreiben von Dateien einfach mithilfe der nativen Datei-APIs auf der entsprechenden Plattform durchgeführt. Alternativ stellen eingebettete Ressourcen eine einfachere Lösung für das Verteilen von Datendateien mit einer App dar. Mit .NET Standard 2.0 ist es jedoch möglich, Dateizugriffscode in .NET Standard-Bibliotheken freizugeben.

Weitere Informationen zum Verarbeiten von Bilddateien finden Sie unter [Working with Images (Arbeiten mit Bildern)](~/xamarin-forms/user-interface/images.md).

## <a name="saving-and-loading-files"></a>Speichern und Laden von Dateien

Die `System.IO`-Klassen können verwendet werden, um auf das Dateisystem jeder Plattform zuzugreifen. Mithilfe der `File`-Klasse können Sie Dateien erstellen, löschen und lesen, und mit der `Directory`-Klasse können Sie den Inhalt der Verzeichnisse erstellen, löschen oder auflisten. Sie können ebenfalls die `Stream`-Unterklassen verwenden, mit denen Sie Dateivorgänge (z.B. die Komprimierung oder die Positionssuche innerhalb einer Datei) besser steuern können.

Eine Textdatei kann mithilfe der `File.WriteAllText`-Methode geschrieben werden:

```csharp
File.WriteAllText(fileName, text);
```

Eine Textdatei kann mithilfe der `File.ReadAllText`-Methode gelesen werden:

```csharp
string text = File.ReadAllText(fileName);
```

Darüber hinaus bestimmt die `File.Exists`-Methode, ob die angegebene Datei vorhanden ist:

```csharp
bool doesExist = File.Exists(fileName);
```

Der Pfad der Datei auf jeder Plattform kann aus einer .NET Standard Bibliothek ermittelt werden, indem ein Wert der- [`Environment.SpecialFolder`](xref:System.Environment.SpecialFolder) Enumeration als erstes Argument für die-Methode verwendet wird `Environment.GetFolderPath` . Das Ergebnis kann dann mithilfe der `Path.Combine`-Methode mit einem Dateinamen kombiniert werden:

```csharp
string fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "temp.txt");
```

Diese Vorgänge werden in der Beispiel-App veranschaulicht, die eine Seite enthält, die Text speichert und lädt:

[![Speichern und Laden von Text](files-images/saveandload-sml.png "Speichern und Laden von Dateien in der APP")](files-images/saveandload.png#lightbox "Speichern und Laden von Dateien in der APP")

## <a name="loading-files-embedded-as-resources"></a>Laden von als Ressourcen eingebetteten Dateien

Wenn Sie eine Datei in eine **.NET Standard**-Assembly einbetten möchten, müssen Sie eine Datei erstellen oder hinzufügen und sicherstellen, dass **Buildaktion** auf „EmbeddedResource“ festgelegt ist.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Konfigurieren von eingebetteten ressourcenbuild-Aktionen](files-images/vs-embeddedresource-sml.png "Festlegen von EmbeddedResource BuildAction")](files-images/vs-embeddedresource.png#lightbox "Festlegen von EmbeddedResource BuildAction")

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

[![Textdatei eingebettet in die .NET-Standardbibliothek, Konfigurieren der eingebetteten ressourcenbuild-Aktion](files-images/xs-embeddedresource-sml.png "Festlegen von EmbeddedResource BuildAction")](files-images/xs-embeddedresource.png#lightbox "Festlegen von EmbeddedResource BuildAction")

-----

`GetManifestResourceStream` wird verwendet, um über die **Ressourcen-ID** auf die eingebettete Datei zuzugreifen. Standardmäßig ist die Ressourcen-ID der Dateiname, dem der Standard Namespace für das Projekt vorangestellt ist, in das Sie eingebettet ist. in diesem Fall ist die Assembly **workingwithfiles** , und der Dateiname ist **LibTextResource.txt**, sodass die Ressourcen-ID lautet `WorkingWithFiles.LibTextResource.txt` .

```csharp
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(LoadResourceText)).Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.LibTextResource.txt");
string text = "";
using (var reader = new System.IO.StreamReader (stream))
{  
    text = reader.ReadToEnd ();
}
```

Die `text`-Variable kann anschließend verwendet werden, um den Text darzustellen. Sie können Sie jedoch auch im Code verwenden. Dieser Screenshot der [Beispiel-App](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfiles) veranschaulicht den Text, der in einem `Label`-Steuerelement gerendert wird.

 [![In der .NET-Standardbibliothek eingebettete Textdatei](files-images/pcltext-sml.png "Eingebettete Textdatei in .NET Standard in der APP angezeigter Bibliothek")](files-images/pcltext.png#lightbox "Eingebettete Textdatei in .NET Standard in der APP angezeigter Bibliothek")

Das Laden und Deserialisieren einer XML-Datei ist genauso einfach. Der folgende Code veranschaulicht, wie eine XML-Datei aus einer Ressource geladen und deserialisiert wird und anschließend an ein `ListView`-Element gebunden wird, um angezeigt zu werden. Die XML-Datei enthält ein Array von `Monkey`-Objekten (die Klasse wird im Beispielcode definiert).

```csharp
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(LoadResourceText)).Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.LibXmlResource.xml");
List<Monkey> monkeys;
using (var reader = new System.IO.StreamReader (stream)) {
    var serializer = new XmlSerializer(typeof(List<Monkey>));
    monkeys = (List<Monkey>)serializer.Deserialize(reader);
}
var listView = new ListView ();
listView.ItemsSource = monkeys;
```

 [![In der .NET-Standardbibliothek eingebettete XML-Datei, in ListView angezeigt](files-images/pclxml-sml.png "In ListView angezeigte eingebettete XML-Datei in der .NET-Standardbibliothek")](files-images/pclxml.png#lightbox "In ListView angezeigte eingebettete XML-Datei in der .NET-Standardbibliothek")

## <a name="embedding-in-shared-projects"></a>Einbetten in freigegebene Projekte

Freigegebene Projekte können ebenfalls Dateien als eingebettete Ressourcen enthalten. Da der Inhalt eines freigegebenen Projekts jedoch im verweisenden Projekt kompiliert wird, kann das Präfix variieren, das für die Ressourcen-IDs von eingebetteten Dateien verwendet wird. Das bedeutet, dass die Ressourcen-ID für eingebettete Dateien sich je nach Plattform unterscheiden kann.

Für dieses Problem bei freigegebenen Projekten gibt es zwei Lösungsansätze:

- **Synchronisieren des Projekts:** Bearbeiten Sie die Projekteigenschaften für jede Plattform, damit diese den **gleichen** Assemblynamen und den Standardnamespace verwenden. Dieser Wert kann dann als Präfix für die Ressourcen-IDs von eingebetteten Dateien im freigegebenen Projekt hartcodiert werden.
- **#if-Compilerdirektiven:** Verwenden Sie Compilerdirektiven, um das richtige Präfix für die Ressourcen-ID festzulegen, und verwenden Sie diesen Wert, um die richtige Ressourcen-ID dynamisch zu erstellen.

Im Folgenden finden Sie den Code für den zweiten Lösungsansatz. Compilerdirektiven werden verwendet, um das hartcodierte Ressourcenpräfix auszuwählen (das normalerweise dem Standardnamespace für das verweisende Projekt entspricht). Anschließend wird die `resourcePrefix`-Variable verwendet, um eine gültige Ressourcen-ID zu erstellen, indem diese mit dem Dateinamen der eingebetteten Ressource verkettet wird.

```csharp
#if __IOS__
var resourcePrefix = "WorkingWithFiles.iOS.";
#endif
#if __ANDROID__
var resourcePrefix = "WorkingWithFiles.Droid.";
#endif

Debug.WriteLine("Using this resource prefix: " + resourcePrefix);
// note that the prefix includes the trailing period '.' that is required
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(SharedPage)).Assembly;
Stream stream = assembly.GetManifestResourceStream
    (resourcePrefix + "SharedTextResource.txt");
```

### <a name="organizing-resources"></a>Organisieren von Ressourcen

In den oben genannten Beispielen wird davon ausgegangen, dass die Datei in das Stammverzeichnis des Projekts für die .NET Standard-Bibliothek eingebettet ist. In diesem Fall weist die Ressourcen-ID die Form **Namespace.Dateiname.Erweiterung** auf, z.B. `WorkingWithFiles.LibTextResource.txt` oder `WorkingWithFiles.iOS.SharedTextResource.txt`.

Es ist möglich, eingebettete Ressourcen in Ordnern zu organisieren. Wenn eine eingebettete Ressource in einem Ordner gespeichert wird, wird der Ordnername Teil der Ressourcen-ID (durch Punkte getrennt). Das Format der Ressourcen-ID entspricht dann **Namespace.Ordner.Dateiname.Erweiterung**. Wenn Sie die Dateien, die in der Beispiel-App verwendet werden, im Ordner **MyFolder** speichern würden, wären `WorkingWithFiles.MyFolder.LibTextResource.txt` und `WorkingWithFiles.iOS.MyFolder.SharedTextResource.txt` die entsprechenden Ressourcen-IDs.

### <a name="debugging-embedded-resources"></a>Debuggen von eingebetteten Ressourcen

Manchmal kann nur schwer nachvollzogen werden, warum eine bestimmte Ressource nicht geladen wird. Folgender Code für das Debuggen kann temporär zu einer Anwendung hinzugefügt werden, um zu bestätigen, dass die Ressourcen richtig konfiguriert sind. Dadurch werden alle bekannten Ressourcen, die in die bestimmte Assembly eingebettet sind, im Pad **Fehler** ausgegeben, um Probleme beim Laden von Ressourcen zu debuggen.

```csharp
using System.Reflection;
// ...
// use for debugging, not in released app code!
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(SharedPage)).Assembly;
foreach (var res in assembly.GetManifestResourceNames()) {
    System.Diagnostics.Debug.WriteLine("found resource: " + res);
}
```

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden einfache Dateivorgänge für das Speichern und Laden von Text auf einem Gerät und für das Laden von eingebetteten Ressourcen erläutert. Mit .NET Standard 2.0 ist es möglich, Dateizugriffscode in .NET Standard-Bibliotheken freizugeben.

## <a name="related-links"></a>Verwandte Links

- [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfiles)
- [Xamarin.Forms-Beispiele](https://github.com/xamarin/xamarin-forms-samples)
- [Working with the File System in Xamarin.iOS (Arbeiten mit dem Dateisystem in Xamarin.iOS)](~/ios/app-fundamentals/file-system.md)
