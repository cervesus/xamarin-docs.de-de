---
title: Dateibehandlung in Xamarin.Forms
description: Dateibearbeitung mit Xamarin.Forms kann mithilfe von Code in eine .NET Standardbibliothek oder mithilfe von eingebetteten Ressourcen erreicht werden.
ms.prod: xamarin
ms.assetid: 9987C3F6-5F04-403B-BBB4-ECB024EA6CC8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/21/2018
ms.openlocfilehash: 0be441a7be9777698212e690aca95fdd75e5050f
ms.sourcegitcommit: eac092f84b603958c761df305f015ff84e0fad44
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2018
ms.locfileid: "36310152"
---
# <a name="file-handling-in-xamarinforms"></a>Dateibehandlung in Xamarin.Forms

_Dateibearbeitung mit Xamarin.Forms kann mithilfe von Code in eine .NET Standardbibliothek oder mithilfe von eingebetteten Ressourcen erreicht werden._

## <a name="overview"></a>Übersicht

Xamarin.Forms-Code wird auf mehreren Plattformen ausgeführt, von denen jede ihr eigenes Dateisystem besitzt. Zuvor musste dies, dass beim Lesen und Schreiben von Dateien am einfachsten wurde mithilfe von systemeigene APIs-Datei auf jeder Plattform ausgeführt. Alternativ sind eingebettete Ressourcen eine einfachere Lösung für Datendateien zu einer app zu verteilen. Mit .NET Standard 2.0 ist es jedoch möglich, die Datei Zugangscode in .NET Standardbibliotheken freigeben.

Informationen zur Behandlung von Bilddateien finden Sie unter der [arbeiten mit Bildern](~/xamarin-forms/user-interface/images.md) Seite.

<a name="Loading_and_Saving_Files" />

## <a name="saving-and-loading-files"></a>Speichern und Laden von Dateien

Die `System.IO` Klassen können verwendet werden, um Zugriff auf das Dateisystem auf jeder Plattform. Die `File` -Klasse können Sie erstellen, löschen und Lesen von Dateien, und die `Directory` -Klasse können Sie zum Erstellen, löschen oder aufführen des Inhalts von Verzeichnissen. Sie können auch die `Stream` Unterklassen, die ein höheres Maß an Kontrolle über Dateioperationen (z. B. Komprimierung oder Position Suche innerhalb einer Datei) bereitstellen können.

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

Der Pfad der Datei auf jeder Plattform kann über eine .NET Standardbibliothek bestimmt werden, mit einem Wert, der die [ `Environment.SpecialFolder` ](xref:System.Environment.SpecialFolder) -Enumeration als erstes Argument für die `Environment.GetFolderPath` Methode. Dies kann dann zusammen mit Dateinamen mit der `Path.Combine` Methode:

```csharp
string fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "temp.txt");
```

Diese Vorgänge werden in der Beispiel-app veranschaulicht, die eine Seite einschließt, die speichert und lädt Text:

[![Speichern und Laden von Text](files-images/saveandload-sml.png "speichern und Laden von Dateien in der App")](files-images/saveandload.png#lightbox "speichern und Laden von Dateien in der App")

<a name="Loading_Files_Embedded_as_Resources" />

## <a name="loading-files-embedded-as-resources"></a>Laden von Dateien eingebettet als Ressourcen

Einbetten eine Datei in eine **.NET Standard** Assembly erstellen, oder fügen Sie eine Datei, und sicherstellen, dass **Buildvorgang: EmbeddedResource**.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Konfigurieren von eingebettete Ressourcenbuildvorgang](files-images/vs-embeddedresource-sml.png "Einstellung EmbeddedResource BuildAction")](files-images/vs-embeddedresource.png#lightbox "Einstellung EmbeddedResource BuildAction")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Textdatei eingebettet, in der PCL, konfigurieren eingebettete Ressourcenbuildvorgang](files-images/xs-embeddedresource-sml.png "Einstellung EmbeddedResource BuildAction")](files-images/xs-embeddedresource.png#lightbox "Einstellung EmbeddedResource BuildAction")

-----

`GetManifestResourceStream` wird verwendet, um Zugriff auf die eingebettete Datei mithilfe der **Ressourcen-ID**. Standardmäßig die Ressourcen-ID der Dateiname, der den Standardnamespace für das eingebettet ist in - Projekt mit dem Präfix in diesem Fall die Assembly wird **WorkingWithFiles** und der Dateiname ist **PCLTextResource.txt**, Daher ist die Ressourcen-ID `WorkingWithFiles.PCLTextResource.txt`.

```csharp
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(LoadResourceText)).Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.PCLTextResource.txt");
string text = "";
using (var reader = new System.IO.StreamReader (stream)) {
    text = reader.ReadToEnd ();
}
```

Die `text` Variable kann dann verwendet werden, um den Text anzuzeigen, oder verwenden sie andernfalls im Code. Diesen Screenshot mit der [Beispiel-app](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/) wird der Text gerendert wird, einem `Label` Steuerelement.

 [![Textdatei eingebettet, in der PCL](files-images/pcltext-sml.png "eingebettete Textdatei in PCL angezeigt, in der App")](files-images/pcltext.png#lightbox "eingebettete Textdatei in PCL in-App angezeigt")

Laden und Deserialisieren von XML ist gleichermaßen einfach. Der folgende Code zeigt eine XML-Datei wird geladen und deserialisiert aus einer Ressource gebunden ein `ListView` für die Anzeige. Die XML-Datei enthält ein Array von `Monkey` Objekte (die Klasse wird im Beispielcode definiert).

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

 [![XML-Datei eingebettet, in der PCL, in der Listenansicht angezeigt](files-images/pclxml-sml.png "eingebettete XML-Datei in der PCL angezeigt, in der Listenansicht")](files-images/pclxml.png#lightbox "eingebettete XML-Datei in der PCL, die in der Listenansicht angezeigt")

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

Debug.WriteLine("Using this resource prefix: " + resourcePrefix);
// note that the prefix includes the trailing period '.' that is required
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(SharedPage)).Assembly;
Stream stream = assembly.GetManifestResourceStream
    (resourcePrefix + "SharedTextResource.txt");
```

<a name="Organizing_Resources" />

### <a name="organizing-resources"></a>Organisieren von Ressourcen

In den Beispielen oben wird davon ausgegangen, dass die Datei im Stammverzeichnis des standardmäßigen .NET Library-Projekts eingebettet ist, in dem die Ressourcen-ID im Format wird **Namespace.Dateiname.Erweiterung**, wie z. B. `WorkingWithFiles.PCLTextResource.txt` und `WorkingWithFiles.iOS.SharedTextResource.txt`.

Es ist möglich, eingebettete Ressourcen in Ordnern zu organisieren. Wenn eine eingebettete Ressource in einem Ordner abgelegt wird, wird der Name des Ordners Teil der Ressourcen-ID (durch Punkte getrennt), so, dass die Ressourcen-ID-Format wird **Namespace.Folder.Filename.Extension**. Platzieren die Dateien in der Beispiel-app verwendet wird, in einen Ordner **MyFolder** würde die entsprechenden Ressourcen-IDs machen `WorkingWithFiles.MyFolder.PCLTextResource.txt` und `WorkingWithFiles.iOS.MyFolder.SharedTextResource.txt`.

<a name="Debugging_Embedded_Resources" />

### <a name="debugging-embedded-resources"></a>Debuggen von eingebetteten Ressourcen

Da in einigen Fällen es schwierig ist zu verstehen, warum eine bestimmte Ressource beansprucht wird nicht, kann die folgenden Debugcode vorübergehend hinzugefügt werden, zu einer Anwendung, um zu bestätigen, dass Ressourcen ordnungsgemäß konfiguriert sind. Es gibt alle bekannte Ressourcen, die in der angegebenen Assembly eingebettet der **Fehler** Pad-Ressource laden Probleme Debuggen.

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

Dieser Artikel hat einige einfache Dateivorgänge speichern und Laden von Text auf dem Gerät und zum Laden von eingebetteten Ressourcen angezeigt. Mit .NET Standard 2.0 ist es möglich, Zugangscode in .NET Standardbibliotheken Datei freigeben.

## <a name="related-links"></a>Verwandte Links

- [FilesSample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/)
- [Xamarin.Forms Samples (Beispiele für Xamarin.Forms)](https://github.com/xamarin/xamarin-forms-samples)
- [Arbeiten mit dem Dateisystem in Xamarin.iOS](~/ios/app-fundamentals/file-system.md)
- [Dateien Arbeitsmappe](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/files/files.workbook)
