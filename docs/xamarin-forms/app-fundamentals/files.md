---
title: Dateibehandlung in Xamarin.Forms
description: Dateiverarbeitung mit Xamarin.Forms kann mithilfe von Code in einer .NET Standard-Bibliothek oder mithilfe von eingebetteten Ressourcen erreicht werden.
ms.prod: xamarin
ms.assetid: 9987C3F6-5F04-403B-BBB4-ECB024EA6CC8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/21/2018
ms.openlocfilehash: 87084a0ccc2970f56e7ef7a6d2f4c59c49032aa0
ms.sourcegitcommit: 7eed80186e23e6aff3ddbbf7ce5cd1fa20af1365
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2018
ms.locfileid: "51527364"
---
# <a name="file-handling-in-xamarinforms"></a>Dateibehandlung in Xamarin.Forms

_Dateiverarbeitung mit Xamarin.Forms kann mithilfe von Code in einer .NET Standard-Bibliothek oder mithilfe von eingebetteten Ressourcen erreicht werden._

## <a name="overview"></a>Übersicht

Xamarin.Forms-Code wird auf mehreren Plattformen ausgeführt, von denen jede ihr eigenes Dateisystem besitzt. Früher bedeutete dies, dass mit der nativen Datei-APIs auf jeder Plattform lesen und Schreiben von Dateien am einfachsten durchgeführt wurde. Eingebettete Ressourcen sind hingegen eine einfachere Lösung Datendateien mit einer app zu verteilen. Mit .NET Standard 2.0 ist es jedoch möglich, die Datei Zugriffscode in .NET Standard-Bibliotheken gemeinsam nutzen.

Informationen zur Behandlung von Bilddateien, finden Sie in der [arbeiten mit Bildern](~/xamarin-forms/user-interface/images.md) Seite.

<a name="Loading_and_Saving_Files" />

## <a name="saving-and-loading-files"></a>Speichern und Laden von Dateien

Die `System.IO` Klassen können verwendet werden, um Zugriff auf das Dateisystem auf jeder Plattform. Die `File` Klasse können Sie die zu erstellen, löschen und Lesen von Dateien, und die `Directory` Klasse können Sie erstellen, löschen oder den Inhalt der Verzeichnisse auflisten. Sie können auch die `Stream` Unterklassen, die ein höheres Maß an Kontrolle über Dateioperationen (z. B. Komprimierung oder Position Suchen innerhalb einer Datei) bereitstellen können.

Eine Textdatei mit geschrieben werden die `File.WriteAllText` Methode:

```csharp
File.WriteAllText(fileName, text);
```

Eine Textdatei kann gelesen werden, mithilfe der `File.ReadAllText` Methode:

```csharp
string text = File.ReadAllText(fileName);
```

Darüber hinaus die `File.Exists` Methode bestimmt, ob die angegebene Datei vorhanden ist:

```csharp
bool doesExist = File.Exists(fileName);
```

Der Pfad der Datei auf jeder Plattform lassen sich aus einer .NET Standard-Bibliothek mit dem Wert der [ `Environment.SpecialFolder` ](xref:System.Environment.SpecialFolder) Enumeration als das erste Argument für die `Environment.GetFolderPath` Methode. Dies kann dann mit der ein Dateiname mit kombiniert werden die `Path.Combine` Methode:

```csharp
string fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "temp.txt");
```

In der Beispiel-app enthält eine Seite, die speichert und lädt Text, werden diese Vorgänge veranschaulicht:

[![Speichern und Laden von Text](files-images/saveandload-sml.png "speichern und Laden von Dateien in-App")](files-images/saveandload.png#lightbox "speichern und Laden von Dateien in-App")

<a name="Loading_Files_Embedded_as_Resources" />

## <a name="loading-files-embedded-as-resources"></a>Laden von Dateien, die als Ressourcen eingebettet

Einbetten eine Datei in eine **.NET Standard** Assembly erstellen, oder fügen Sie eine Datei, und stellen sicher, dass **Buildvorgang: EmbeddedResource**.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Konfigurieren der eingebetteten Ressource-Buildvorgang](files-images/vs-embeddedresource-sml.png "Einstellung EmbeddedResource BuildAction")](files-images/vs-embeddedresource.png#lightbox "Einstellung EmbeddedResource BuildAction")

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![Text-Datei eingebettet, in der PCL, konfigurieren die eingebettete Ressource-Buildvorgang](files-images/xs-embeddedresource-sml.png "Einstellung EmbeddedResource BuildAction")](files-images/xs-embeddedresource.png#lightbox "Einstellung EmbeddedResource BuildAction")

-----

`GetManifestResourceStream` wird verwendet, um den Zugriff auf die eingebettete Datei mithilfe der **Ressourcen-ID**. Die Ressourcen-ID ist der Dateiname, der den Standardnamespace für den es eingebettet - Projekt mit dem Präfix in diesem Fall die Assembly wird standardmäßig **WorkingWithFiles** und der Dateiname ist **PCLTextResource.txt**, Daher ist die Ressourcen-ID `WorkingWithFiles.PCLTextResource.txt`.

```csharp
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(LoadResourceText)).Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.PCLTextResource.txt");
string text = "";
using (var reader = new System.IO.StreamReader (stream)) {
    text = reader.ReadToEnd ();
}
```

Die `text` Variable kann dann verwendet werden, um den Text anzuzeigen, oder verwenden sie andernfalls im Code. Diesen Screenshot mit der [Beispiel-app](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/) wird der Text gerendert wird, einem `Label` Steuerelement.

 [![Text-Datei eingebettet, in der PCL](files-images/pcltext-sml.png "eingebettete Text-Datei in die PCL, die in-App angezeigt")](files-images/pcltext.png#lightbox "eingebettete Text-Datei in die PCL in-App angezeigt.")

Laden und Deserialisieren eines XML-ist genauso einfach. Der folgende Code zeigt eine XML-Datei wird, geladen und deserialisiert von einer Ressource, und an gebunden eine `ListView` für die Anzeige. Die XML-Datei enthält ein Array von `Monkey` Objekte (die Klasse wird im Beispielcode definiert).

```csharp
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(LoadResourceText)).Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.PCLXmlResource.xml");
List<Monkey> monkeys;
using (var reader = new System.IO.StreamReader (stream)) {
    var serializer = new XmlSerializer(typeof(List<Monkey>));
    monkeys = (List<Monkey>)serializer.Deserialize(reader);
}
var listView = new ListView ();
listView.ItemsSource = monkeys;
```

 [![XML-Datei eingebettet, in der PCL, in der ListView angezeigten](files-images/pclxml-sml.png "eingebetteten XML-Datei in die PCL in ListView angezeigten")](files-images/pclxml.png#lightbox "eingebetteten XML-Datei in die PCL in ListView angezeigt")

<a name="Embedding_in_Shared_Projects" />

## <a name="embedding-in-shared-projects"></a>Einbetten in freigegebene Projekte

Freigegebene Projekte können außerdem Dateien als eingebettete Ressourcen enthalten, aber da der Inhalt eines freigegebenen Projekts in die verweisende Projekte kompiliert werden, mit das Präfix für Ressource "File" eingebettet wird, die IDs ändern können. Dies bedeutet, dass die Ressourcen-ID für jede eingebettete Datei je nach Plattform unterschiedlich sein kann.

Es gibt zwei Lösungen für dieses Problem mit freigegebenen Projekten:

-  **Synchronisieren Sie die Projekte** -bearbeiten Sie die Projekteigenschaften für jede Plattform mit der **gleichen** Assembly-Namen und Namespace. Dieser Wert kann "hartcodiert" als Präfix für embedded-Ressourcen-IDs im freigegebenen Projekt sein.
-  **#if-Compiler-Direktiven** -Compiler-Direktiven verwenden, legen Sie die richtigen Ressourcen-ID-Präfix, und verwenden diesen Wert auf dynamische Weise erstellen, die richtigen Ressourcen-ID.


Code zur Veranschaulichung der zweiten Option ist unten dargestellt. Compiler-Direktiven werden verwendet, um das ressourcenpräfix hartcodiert auszuwählen (die normalerweise den Standardnamespace für das verweisende Projekt identisch ist). Die `resourcePrefix` Variable wird dann verwendet, um eine gültige Ressourcen-ID durch Verketten es mit dem Dateinamen der eingebetteten Ressource zu erstellen.

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

<a name="Organizing_Resources" />

### <a name="organizing-resources"></a>Organisieren von Ressourcen

Die oben genannten Beispielen wird davon ausgegangen, dass die Datei im Stammverzeichnis des Projekts für .NET Standard-Bibliothek eingebettet ist, in dem Fall die Ressourcen-ID das Format weist **Namespace.Dateiname.Erweiterung**, z. B. `WorkingWithFiles.PCLTextResource.txt` und `WorkingWithFiles.iOS.SharedTextResource.txt`.

Es ist möglich, eingebettete Ressourcen in Ordnern zu organisieren. Wenn eine eingebettete Ressource in einem Ordner gespeichert ist, den Namen des Ordners wird die Ressourcen-ID (die durch Punkte getrennt werden), damit die Ressourcen-ID-Format wird **Namespace.Folder.Filename.Extension**. Platzieren die Dateien in der Beispiel-app in einem Ordner **MyFolder** würde die entsprechenden Ressourcen-IDs machen `WorkingWithFiles.MyFolder.PCLTextResource.txt` und `WorkingWithFiles.iOS.MyFolder.SharedTextResource.txt`.

<a name="Debugging_Embedded_Resources" />

### <a name="debugging-embedded-resources"></a>Debuggen von eingebetteten Ressourcen

Da es in einigen Fällen schwer ist zu verstehen, warum eine bestimmte Ressource nicht geladen werden ist, kann die folgenden Debuggen von Code vorübergehend hinzugefügt werden, zu einer Anwendung, um zu bestätigen, dass die Ressourcen korrekt konfiguriert sind. Alle bekannte Ressourcen, die in der angegebenen Assembly eingebettet erfolgt die Ausgabe der **Fehler** Pad zum Debuggen von Problemen Laden der Ressource.

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

Dieser Artikel hat einige einfache Vorgänge für das Speichern und Laden von Text auf dem Gerät und zum Laden von eingebetteten Ressourcen angezeigt. Mit .NET Standard 2.0 ist es möglich, die Zugriff den Code in .NET Standard-Bibliotheken gemeinsam nutzen.

## <a name="related-links"></a>Verwandte Links

- [FilesSample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/)
- [Xamarin.Forms Samples (Beispiele für Xamarin.Forms)](https://github.com/xamarin/xamarin-forms-samples)
- [Arbeiten mit dem Dateisystem in Xamarin.iOS](~/ios/app-fundamentals/file-system.md)

