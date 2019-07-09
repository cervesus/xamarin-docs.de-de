---
title: Dateiverarbeitung in Xamarin.Forms
description: Die Dateiverarbeitung mit Xamarin.Forms kann mit Code in einer .NET Standard-Bibliothek oder mithilfe eingebetteter Ressourcen erfolgen.
ms.prod: xamarin
ms.assetid: 9987C3F6-5F04-403B-BBB4-ECB024EA6CC8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/21/2018
ms.openlocfilehash: 93218289cefe26c523bf14721202f7ec4743f022
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2019
ms.locfileid: "67659077"
---
# <a name="file-handling-in-xamarinforms"></a>Dateiverarbeitung in Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/)

_Die Dateiverarbeitung mit Xamarin.Forms kann mit Code in einer .NET Standard-Bibliothek oder mithilfe eingebetteter Ressourcen erfolgen._

## <a name="overview"></a>Übersicht

Xamarin.Forms-Code wird auf mehreren Plattformen ausgeführt, von denen jede ihr eigenes Dateisystem besitzt. Früher wurde das Lesen und Schreiben von Dateien einfach mithilfe der nativen Datei-APIs auf der entsprechenden Plattform durchgeführt. Alternativ stellen eingebettete Ressourcen eine einfachere Lösung für das Verteilen von Datendateien mit einer App dar. Mit .NET Standard 2.0 ist es jedoch möglich, Dateizugriffscode in .NET Standard-Bibliotheken freizugeben.

Weitere Informationen zum Verarbeiten von Bilddateien finden Sie unter [Working with Images (Arbeiten mit Bildern)](~/xamarin-forms/user-interface/images.md).

<a name="Loading_and_Saving_Files" />

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

Der Pfad der Datei auf jeder Plattform kann über eine .NET Standard-Bibliothek bestimmt werden, indem ein Wert der [`Environment.SpecialFolder`](xref:System.Environment.SpecialFolder)-Enumeration als erstes Argument der `Environment.GetFolderPath`-Methode verwendet wird. Das Ergebnis kann dann mithilfe der `Path.Combine`-Methode mit einem Dateinamen kombiniert werden:

```csharp
string fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "temp.txt");
```

Diese Vorgänge werden in der Beispiel-App veranschaulicht, die eine Seite enthält, die Text speichert und lädt:

[![Text speichern und Laden](files-images/saveandload-sml.png "Dateien in der App speichern und laden")](files-images/saveandload.png#lightbox "Dateien in der App speichern und laden")

<a name="Loading_Files_Embedded_as_Resources" />

## <a name="loading-files-embedded-as-resources"></a>Laden von als Ressourcen eingebetteten Dateien

Wenn Sie eine Datei in eine **.NET Standard**-Assembly einbetten möchten, müssen Sie eine Datei erstellen oder hinzufügen und sicherstellen, dass für den Buildvorgang **EmbeddedResource** festgelegt ist.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Buildaktion auf „EmbeddedResource“ festlegen](files-images/vs-embeddedresource-sml.png "Buildaktion auf „EmbeddedResource“ festlegen")](files-images/vs-embeddedresource.png#lightbox "Buildaktion auf „EmbeddedResource“ festlegen")

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![In PCL eingebettete Textdatei, Buildaktion auf „EmbeddedResource“ festlegen](files-images/xs-embeddedresource-sml.png "Buildaktion auf „EmbeddedResource“ festlegen")](files-images/xs-embeddedresource.png#lightbox "Buildaktion auf „EmbeddedResource“ festlegen")

-----

`GetManifestResourceStream` wird verwendet, um über die **Ressourcen-ID** auf die eingebettete Datei zuzugreifen. Die Ressourcen-ID entspricht standardmäßig dem Dateinamen mit dem vorangestellten Standardnamespace für das Projekt, in das die Datei eingebettet ist. In diesem Fall ist die Assembly **WorkingWithFiles** und der Dateiname **PCLTextResource.txt**, die Ressourcen-ID entspricht also `WorkingWithFiles.PCLTextResource.txt`.

```csharp
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(LoadResourceText)).Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.PCLTextResource.txt");
string text = "";
using (var reader = new System.IO.StreamReader (stream)) {
    text = reader.ReadToEnd ();
}
```

Die `text`-Variable kann anschließend verwendet werden, um den Text darzustellen. Sie können Sie jedoch auch im Code verwenden. Dieser Screenshot der [Beispiel-App](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/) veranschaulicht den Text, der in einem `Label`-Steuerelement gerendert wird.

 [![In PCL eingebettete Textdatei](files-images/pcltext-sml.png "In PCL eingebettete Textdatei in der App")](files-images/pcltext.png#lightbox "In PCL eingebettete Textdatei in der App")

Das Laden und Deserialisieren einer XML-Datei ist genauso einfach. Der folgende Code veranschaulicht, wie eine XML-Datei aus einer Ressource geladen und deserialisiert wird und anschließend an ein `ListView`-Element gebunden wird, um angezeigt zu werden. Die XML-Datei enthält ein Array von `Monkey`-Objekten (die Klasse wird im Beispielcode definiert).

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

 [![In PCL eingebettete XML-Datei, die in ListView angezeigt wird](files-images/pclxml-sml.png "In PCL eingebettete XML-Datei, die in ListView angezeigt wird")](files-images/pclxml.png#lightbox "In PCL eingebettete XML-Datei, die in ListView angezeigt wird")

<a name="Embedding_in_Shared_Projects" />

## <a name="embedding-in-shared-projects"></a>Einbetten in freigegebene Projekte

Freigegebene Projekte können ebenfalls Dateien als eingebettete Ressourcen enthalten. Da der Inhalt eines freigegebenen Projekts jedoch im verweisenden Projekt kompiliert wird, kann das Präfix variieren, das für die Ressourcen-IDs von eingebetteten Dateien verwendet wird. Das bedeutet, dass die Ressourcen-ID für eingebettete Dateien sich je nach Plattform unterscheiden kann.

Für dieses Problem bei freigegebenen Projekten gibt es zwei Lösungsansätze:

-  **Synchronisieren des Projekts:** Bearbeiten Sie die Projekteigenschaften für jede Plattform, damit diese den **gleichen** Assemblynamen und den Standardnamespace verwenden. Dieser Wert kann dann als Präfix für die Ressourcen-IDs von eingebetteten Dateien im freigegebenen Projekt hartcodiert werden.
-  **#if-Compilerdirektiven:** Verwenden Sie Compilerdirektiven, um das richtige Präfix für die Ressourcen-ID festzulegen, und verwenden Sie diesen Wert, um die richtige Ressourcen-ID dynamisch zu erstellen.


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

<a name="Organizing_Resources" />

### <a name="organizing-resources"></a>Organisieren von Ressourcen

In den oben genannten Beispielen wird davon ausgegangen, dass die Datei in das Stammverzeichnis des Projekts für die .NET Standard-Bibliothek eingebettet ist. In diesem Fall weist die Ressourcen-ID die Form **Namespace.Dateiname.Erweiterung** auf, z.B. `WorkingWithFiles.PCLTextResource.txt` oder `WorkingWithFiles.iOS.SharedTextResource.txt`.

Es ist möglich, eingebettete Ressourcen in Ordnern zu organisieren. Wenn eine eingebettete Ressource in einem Ordner gespeichert wird, wird der Ordnername Teil der Ressourcen-ID (durch Punkte getrennt). Das Format der Ressourcen-ID entspricht dann **Namespace.Ordner.Dateiname.Erweiterung**. Wenn Sie die Dateien, die in der Beispiel-App verwendet werden, im Ordner **MyFolder** speichern würden, wären `WorkingWithFiles.MyFolder.PCLTextResource.txt` und `WorkingWithFiles.iOS.MyFolder.SharedTextResource.txt` die entsprechenden Ressourcen-IDs.

<a name="Debugging_Embedded_Resources" />

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

- [Beispiel](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/)
- [Xamarin.Forms Samples (Beispiele für Xamarin.Forms)](https://github.com/xamarin/xamarin-forms-samples)
- [Working with the File System in Xamarin.iOS (Arbeiten mit dem Dateisystem in Xamarin.iOS)](~/ios/app-fundamentals/file-system.md)

