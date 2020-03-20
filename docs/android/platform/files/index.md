---
title: Dateizugriff mit Xamarin.Android
description: Der vorliegende Leitfaden erläutert den Dateizugriff in Xamarin.Android.
ms.prod: xamarin
ms.assetid: FC1CFC58-B799-4DD6-8ED1-DE36B0E56856
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 07/23/2018
ms.openlocfilehash: 1bb0fae73a1e3647cdc0e3266c7b44ac04fcc1ee
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2020
ms.locfileid: "79303666"
---
# <a name="file-storage-and-access-with-xamarinandroid"></a>Dateispeicherung und Dateizugriff mit Xamarin.Android

Eine häufige Anforderung an Android-Apps ist die Bearbeitung von Dateien – das Speichern von Bildern, das Herunterladen von Dokumenten oder das Exportieren von Daten zur gemeinsamen Nutzung mit anderen Programmen. Das (auf Linux basierende) Android unterstützt diese Funktionalität durch die Bereitstellung von Speicherplatz für den Dateispeicher. Android unterteilt das Dateisystem in zwei Arten von Speicher:

* **Interner Speicher** – ein Bestandteil des Dateisystems, auf den nur die Anwendung oder das Betriebssystem zugreifen kann.
* **Externer Speicher** – eine Partition für die Speicherung von Dateien, auf die sämtliche Apps, der Benutzer und möglicherweise auch andere Geräte zugreifen können. Auf einigen Geräten kann es sich bei dem externen Speicher um einen Wechseldatenträger handeln (z. B. eine SD-Karte).

Dies ist eine konzeptionelle Unterteilung, sie bezieht sich nicht unbedingt auf eine einzelne Partition oder ein einzelnes Verzeichnis auf dem Gerät. Ein Android-Gerät stellt immer eine Partition für den internen und den externen Speicher bereit. Es ist möglich, dass bestimmte Geräte über mehrere Partitionen verfügen, die als externer Speicher betrachtet werden. Unabhängig von der Partition sind die APIs zum Lesen, Schreiben oder Erstellen von Dateien dieselben. Es gibt zwei API-Sätze, die eine Xamarin.Android-Anwendung für den Dateizugriff nutzen kann:

1. **.NET-APIs (bereitgestellt von Mono und eingeschlossen in Xamarin.Android)** –  dies schließt die von [Xamarin.Essentials](~/essentials/index.md?context=xamarin/android) bereitgestellten [Dateisystemhilfsprogramme](~/essentials/file-system-helpers.md?context=xamarin/android) ein. Die .NET-APIs bieten die beste plattformübergreifende Kompatibilität und stehen in diesem Leitfaden deshalb im Vordergrund.
1. **Native Java-APIs für den Dateizugriff (bereitgestellt von Java und eingeschlossen in Xamarin.Android)** – Java stellt eigene APIs zum Lesen und Schreiben von Dateien bereit. Diese APIs sind eine akzeptable Alternative zu den .NET-APIs, jedoch spezifisch für Android und nicht für Anwendungen geeignet, die plattformübergreifend eingesetzt werden sollen.

Lese- und Schreibvorgänge für Dateien erfolgen in Xamarin.Android in nahezu gleicher Weise wie in jeder anderen .NET-Anwendung. Die Xamarin.Android-App bestimmt den Pfad zur Datei, die bearbeitet werden soll, und verwendet anschließend .NET-Standardausdrücke für den Dateizugriff. Da sich die tatsächlichen Pfade zum externen und internen Speicher je nach Gerät oder Android-Version unterscheiden können, wird von einer Hartcodierung der Dateipfade abgeraten. Verwenden Sie stattdessen die Xamarin.Android-APIs, um den Pfad zu den Dateien zu ermitteln. Hierbei machen die .NET-APIs zum Lesen und Schreiben von Dateien die nativen Android-APIs verfügbar, um den Pfad zu Dateien im internen oder externen Speicher zu bestimmen.

Vor einer Erläuterung der APIs für den Dateizugriff ist es wichtig, einige Details im Zusammenhang mit dem internen und externen Speicher zu verstehen. Diese werden im nächsten Abschnitt erläutert.

## <a name="internal-vs-external-storage"></a>Interner und externer Speicher im Vergleich

Vom Konzept her sind sich interner Speicher und externer Speicher sehr ähnlich. In beiden Fällen handelt es sich um Orte, an denen eine Xamarin.Android-App Dateien speichern kann. Diese Ähnlichkeit kann für Entwickler, die mit Android nicht vertraut sind, verwirrend sein, da möglicherweise unklar ist, wann eine Anwendung internen und wann externen Speicher verwenden sollte.

Als interner Speicher wird der permanente Speicher bezeichnet, den Android dem Betriebssystem, APKs und einzelnen Apps zuweist. Auf diesen Speicher kann nur durch das Betriebssystem oder Apps zugegriffen werden. Android weist jeder App ein Verzeichnis in der Partition des internen Speichers zu. Wird die App deinstalliert, werden alle im internen Speicher enthaltenen Dateien in diesem Verzeichnis ebenfalls gelöscht. Der interne Speicher eignet sich besonders für Dateien, auf die nur von der App zugegriffen werden kann und die nicht für andere Apps freigegeben werden oder nach dem Deinstallieren der App nicht mehr relevant sind. Unter Android 6.0 oder höher können Dateien im internen Speicher von Google mithilfe der [Funktion zur automatischen Sicherung in Android 6.0](https://developer.android.com/guide/topics/data/autobackup) automatisch gesichert werden. Der interne Speicher hat folgende Nachteile:

* Dateien können nicht freigegeben werden.
* Dateien werden gelöscht, wenn die App deinstalliert wird.
* Der im internen Speicher verfügbare Speicherplatz ist möglicherweise begrenzt.

Als externer Speicher wird der Dateispeicher bezeichnet, bei dem es sich nicht um internen Speicher handelt und der nicht ausschließlich für eine App zugänglich ist. Der Hauptzweck von externem Speicher besteht darin, einen Aufbewahrungsort für Dateien bereitzustellen, die gemeinsam von verschiedenen Apps verwendet werden sollen oder zu groß für den internen Speicher sind. Der Vorteil des externen Speichers besteht darin, dass er üblicherweise sehr viel mehr Platz für Dateien bietet als der interne Speicher. Es ist jedoch nicht immer garantiert, dass ein externer Speicher auf einem Gerät vorhanden ist, und möglicherweise werden für den Zugriff auf den externen Speicher spezielle Benutzerberechtigungen benötigt.

> [!NOTE]
> Bei Geräten, die mehrere Benutzer unterstützen, stellt Android jedem Benutzer ein eigenes Verzeichnis im internen und im externen Speicher zur Verfügung. Dieses Verzeichnis ist für andere Benutzer auf dem Gerät nicht zugänglich. Diese Trennung ist für Apps nicht sichtbar, solange keine hartcodierten Pfade zu Dateien im internen oder externen Speicher verwendet werden.

Als Faustregel gilt: Dateien von Xamarin.Android-Anwendungen sollten vorzugsweise im internen Speicher abgelegt werden, wenn dies sinnvoll ist. Auf den externen Speicher sollte zurückgegriffen werden, wenn Dateien für andere Apps freigegeben werden müssen, die Dateien sehr groß sind oder auch dann beibehalten werden sollten, wenn die App deinstalliert wird. Beispielsweise ist eine Konfigurationsdatei am besten für einen internen Speicher geeignet, da sie nur für die App wichtig ist, über die sie erstellt wird.  Fotos dagegen sind ein guter Kandidat für einen externen Speicher. Sie können sehr groß sein, und in vielen Fällen möchte der Benutzer sie teilen oder weiter darauf zugreifen, auch wenn die App deinstalliert wurde.

Dieser Leitfaden konzentriert sich auf den internen Speicher. Ausführliche Informationen zur Verwendung von externem Speicher in einer Xamarin.Android-Anwendung finden Sie im Leitfaden [Externer Speicher](~/android/platform/files/external-storage.md).

## <a name="working-with-internal-storage"></a>Arbeiten mit internem Speicher

Das interner Speicherverzeichnis für eine Anwendung wird durch das Betriebssystem bestimmt und durch die `Android.Content.Context.FilesDir`-Eigenschaft für Android-Apps verfügbar gemacht. So wird ein `Java.IO.File`-Objekt zurückgegeben, das das Verzeichnis repräsentiert, das Android exklusiv für die App reserviert hat.  Beispielsweise kann das interne Speicherverzeichnis für eine App mit dem Paketnamen **com.companyname** so lauten:

```bash
/data/user/0/com.companyname/files
```

Im vorliegenden Dokument wird auf das interne Speicherverzeichnis als _INTERNER\_SPEICHER_ verwiesen.

> [!IMPORTANT]
> Der genaue Pfad zum internen Speicherverzeichnis kann je nach Gerät und Android-Version abweichen. Aus diesem Grund dürfen Apps den Pfad zu diesem Verzeichnis nicht hartcodieren und müssen stattdessen die Xamarin.Android-APIs verwenden, wie z. B. `System.Environment.GetFolderPath()`.

Um die gemeinsame Nutzung von Code zu maximieren, sollten Xamarin.Android-Apps (oder auf Xamarin.Android ausgerichtete Xamarin.Forms-Apps) die [`System.Environment.GetFolderPath()`](xref:System.Environment.GetFolderPath*)-Methode verwenden. In Xamarin.Android gibt diese Methode eine Zeichenfolge für ein Verzeichnis zurück, das sich am selben Speicherort befindet wie `Android.Content.Context.FilesDir`. Diese Methode erwartet eine Enumeration (`System.Environment.SpecialFolder`). Mit dieser wird ein Satz von Enumerationskonstanten identifiziert, die die Pfade zu speziellen, vom Betriebssystem verwendeten Ordnern darstellen. Nicht alle `System.Environment.SpecialFolder`-Werte werden einem gültigen Verzeichnis in Xamarin.Android zugeordnet. Die folgende Tabelle beschreibt, welcher Pfad für einen vorhandenen Wert von `System.Environment.SpecialFolder` erwartet werden kann:

| System.Environment.SpecialFolder | Pfad  |
|----------------------|---|
| `ApplicationData` | **_INTERNER\_SPEICHER_/.config** |
| `Desktop` | **_INTERNER\_SPEICHER_/Desktop** |
| `LocalApplicationData` | **_INTERNER\_SPEICHER_/.local/share** |
| `MyDocuments` | **_INTERNER\_SPEICHER_** |
| `MyMusic` | **_INTERNER\_SPEICHER_/Music** |
| `MyPictures` | **_INTERNER\_SPEICHER_/Pictures** |
| `MyVideos` | **_INTERNER\_SPEICHER_/Videos** |
| `Personal` | **_INTERNER\_SPEICHER_** |

### <a name="reading-or-writing-to-files-on-internal-storage"></a>Lesen oder Schreiben von Dateien in den internen Speicher

Jede der [C#-APIs für Schreibvorgänge](https://docs.microsoft.com/dotnet/csharp/programming-guide/file-system/how-to-write-to-a-text-file) in eine Datei ist ausreichend. Es muss lediglich der Pfad zur Datei ermittelt werden, die sich in dem der Anwendung zugewiesenen Verzeichnis befindet. Es wird dringend empfohlen, die asynchronen Versionen der .NET-APIs zu verwenden, um Probleme zu minimieren, die durch eine Blockierung des Hauptthreads durch den Dateizugriff entstehen könnten.

Dieser Codeausschnitt ist ein Beispiel zum Schreiben einer ganzen Zahl in eine UTF-8-Textdatei im internen Speicherverzeichnis einer Anwendung:

```csharp
public async Task SaveCountAsync(int count)
{
    var backingFile = Path.Combine(System.Environment.GetFolderPath(System.Environment.SpecialFolder.Personal), "count.txt");
    using (var writer = File.CreateText(backingFile))
    {
        await writer.WriteLineAsync(count.ToString());
    }
}
```

Der nächste Codeausschnitt bietet eine Möglichkeit, einen ganzzahligen Wert zu lesen, der in einer Textdatei gespeichert wurde:

```csharp
public async Task<int> ReadCountAsync()
{
    var backingFile = Path.Combine(System.Environment.GetFolderPath(System.Environment.SpecialFolder.Personal), "count.txt");

    if (backingFile == null || !File.Exists(backingFile))
    {
        return 0;
    }

    var count = 0;
    using (var reader = new StreamReader(backingFile, true))
    {
        string line;
        while ((line = await reader.ReadLineAsync()) != null)
        {
            if (int.TryParse(line, out var newcount))
            {
                count = newcount;
            }
        }
    }

    return count;
}
```

## <a name="using--xamarinessentials-ndash-file-system-helpers"></a>Verwenden von Xamarin.Essentials: Dateisystemhilfsprogramme

[Xamarin.Essentials](~/essentials/file-system-helpers.md?context=xamarin/android) ist ein Satz mit APIs zum Schreiben von plattformübergreifendem, kompatiblem Code. Bei den [Dateisystemhilfsprogrammen](~/essentials/file-system-helpers.md?context=xamarin/android) handelt es sich um eine Klasse mit verschiedenen Hilfsprogrammen zum einfacheren Auffinden der Cache- und Datenverzeichnisse einer Anwendung. Dieser Codeausschnitt bietet ein Beispiel für die Ermittlung des internen Speicherverzeichnisses und des Cacheverzeichnisses für eine App:

```csharp
// Get the path to a file on internal storage
var backingFile = Path.Combine(Xamarin.Essentials.FileSystem.AppDataDirectory, "count.txt");

// Get the path to a file in the cache directory
var cacheFile = Path.Combine(Xamarin.Essentials.FileSystem.CacheDirectory, "count.txt");
```

## <a name="hiding-files-from-the-mediastore"></a>Ausblenden von Dateien im `MediaStore`

Der `MediaStore` ist eine Android-Komponente, in der Metadaten zu den Mediendateien (Videos, Musik, Bilder) auf einem Android-Gerät erfasst werden. Der Zweck dieses Speichers besteht darin, die Freigabe dieser Dateien für alle Android-Apps auf dem Gerät zu vereinfachen.

Private Dateien werden nicht als Medien angezeigt, die freigegeben werden können. Wenn eine App beispielsweise ein Bild im privaten externen Speicher der App ablegt, wird diese Datei nicht von der Medienerkennung erfasst (`MediaStore`).

Öffentliche Dateien werden vom `MediaStore` erfasst. Verzeichnisse, die eine 0-Byte-Datei namens **.NOMEDIA** aufweisen, werden von `MediaStore` nicht erfasst.

## <a name="related-links"></a>Verwandte Links

* [Externer Speicher](~/android/platform/files/external-storage.md)
* [Speichern von Dateien im Gerätespeicher](https://developer.android.com/training/data-storage/files)
* [Xamarin.Essentials: Dateisystemhilfsprogramme](~/essentials/file-system-helpers.md?context=xamarin/android)
* [Sichern von Benutzerdaten mit der automatischen Sicherung](https://developer.android.com/guide/topics/data/autobackup)
* [Anpassbarer Speicher](https://source.android.com/devices/storage/adoptable)
