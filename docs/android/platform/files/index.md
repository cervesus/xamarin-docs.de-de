---
title: Dateizugriff mit xamarin. Android
description: In dieser Anleitung wird der Dateizugriff in xamarin. Android erläutert.
ms.prod: xamarin
ms.assetid: FC1CFC58-B799-4DD6-8ED1-DE36B0E56856
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 07/23/2018
ms.openlocfilehash: 1bb0fae73a1e3647cdc0e3266c7b44ac04fcc1ee
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73020415"
---
# <a name="file-storage-and-access-with-xamarinandroid"></a>File Storage und Zugriff mit xamarin. Android

Eine häufige Voraussetzung für Android-Apps ist das Bearbeiten von Dateien &ndash; das Speichern von Bildern, das Herunterladen von Dokumenten oder das Exportieren von Daten für die Freigabe mit anderen Programmen Android (basiert auf Linux) unterstützt dies durch die Bereitstellung von Speicherplatz für den Dateispeicher. Android gruppiert das Dateisystem in zwei verschiedene Speichertypen:

* **Interner Speicher** &ndash; Dies ist ein Teil des Dateisystems, auf den nur von der Anwendung oder dem Betriebssystem zugegriffen werden kann.
* **Externer Speicher** &ndash; Dies ist eine Partition für die Speicherung von Dateien, auf die von allen apps, dem Benutzer und möglicherweise anderen Geräten zugegriffen werden kann. Auf einigen Geräten kann der externe Speicher wechselseitig erfolgen (z. b. eine SD-Karte).

Diese Gruppierungen sind nur konzeptionell und verweisen nicht notwendigerweise auf eine einzelne Partition oder ein einzelnes Verzeichnis auf dem Gerät. Ein Android-Gerät stellt immer eine Partition für den internen und externen Speicher bereit. Möglicherweise haben bestimmte Geräte mehrere Partitionen, die als externer Speicher betrachtet werden. Unabhängig von der Partition sind die APIs zum Lesen, schreiben oder Erstellen von Dateien identisch. Es gibt zwei Sätze von APIs, die eine xamarin. Android-Anwendung für den Dateizugriff verwenden kann:

1. **Die .NET-APIs (von Mono bereitgestellt und von xamarin. Android umschließt)** &ndash; diese enthalten die von [xamarin. Essentials](~/essentials/index.md?context=xamarin/android)bereitgestellten [Dateisystem](~/essentials/file-system-helpers.md?context=xamarin/android) Hilfsprogramme. Die .NET-APIs bieten die beste plattformübergreifende Kompatibilität, und der Schwerpunkt dieses Handbuchs liegt auf diesen APIs.
1. **Die nativen Java-Datei Zugriffs-APIs (von Java bereitgestellt und von xamarin. Android umgeschrieben)** &ndash; Java bietet eigene APIs zum Lesen und Schreiben von Dateien. Dabei handelt es sich um eine vollständig akzeptable Alternative zu den .NET-APIs, die für Android spezifisch sind und für apps, die plattformübergreifend sein sollen, nicht geeignet sind.

Das Lesen und Schreiben in Dateien ist nahezu identisch mit xamarin. Android, wie es bei jeder anderen .NET-Anwendung der Fall ist. Die xamarin. Android-App bestimmt den Pfad zu der Datei, die bearbeitet wird, und verwendet dann die standardmäßige .net-Ausdrücke für den Dateizugriff. Da sich die tatsächlichen Pfade zum internen und externen Speicher von Gerät zu Gerät oder von Android-Version zu Android-Version unterscheiden können, empfiehlt es sich nicht, den Pfad zu den Dateien hart zu codieren. Verwenden Sie stattdessen die xamarin. Android-APIs, um den Pfad zu den Dateien zu ermitteln. Auf diese Weise werden in den .NET-APIs zum Lesen und Schreiben von Dateien die systemeigenen Android-APIs zur Verfügung gestellt, die bei der Ermittlung des Pfads zu Dateien im internen und externen Speicher helfen.

Vor der Erörterung der mit dem Dateizugriff verbundenen APIs ist es wichtig, einige Details zu dem internen und externen Speicher zu verstehen. Dies wird im nächsten Abschnitt erläutert.

## <a name="internal-vs-external-storage"></a>Interner und externer Speicher

Konzeptionell sind interner Speicher und externer Speicher sehr ähnlich &ndash; Sie sind beide Orte, an denen eine xamarin. Android-App möglicherweise Dateien speichert. Diese Ähnlichkeit kann für Entwickler verwirrend sein, die nicht mit Android vertraut sind, da es nicht klar ist, wann eine APP internen Speicher und externen Speicher verwenden soll.

Der interne Speicher bezieht sich auf den nicht flüchtigen Speicher, der von Android dem Betriebssystem, den API und den einzelnen apps zugewiesen wird. Auf diesen Speicherplatz kann außer dem Betriebssystem oder den apps nicht zugegriffen werden. Android weist der internen Speicher Partition für jede APP ein Verzeichnis zu. Wenn die APP deinstalliert wird, werden alle Dateien, die im internen Speicher in diesem Verzeichnis aufbewahrt werden, ebenfalls gelöscht. Interner Speicher eignet sich am besten für Dateien, die nur für die APP zugänglich sind und nicht für andere apps freigegeben werden oder nur sehr wenig Wert haben, wenn die APP deinstalliert wird. Unter Android 6,0 oder höher werden Dateien im internen Speicher möglicherweise automatisch von Google mithilfe des Features " [Automatische Sicherung" in Android 6,0](https://developer.android.com/guide/topics/data/autobackup)gesichert. Interner Speicher hat folgende Nachteile:

* Dateien können nicht freigegeben werden.
* Dateien werden gelöscht, wenn die APP deinstalliert wird.
* Der verfügbare Speicherplatz auf dem internen Speicher ist möglicherweise eingeschränkt.

Externer Speicher bezieht sich auf Dateispeicher, bei dem es sich nicht um einen internen Speicher handelt, und auf den nicht exklusiv für eine APP Der primäre Zweck von externem Speicher ist die Bereitstellung von Dateien, die für die gemeinsame Nutzung von apps verwendet werden sollen oder die zu groß für den internen Speicher sind. Der Vorteil externer Speicherung besteht darin, dass Sie in der Regel viel mehr Speicherplatz für Dateien als für den internen Speicher hat. Es ist jedoch nicht immer gewährleistet, dass externer Speicher auf einem Gerät vorhanden ist, und erfordert möglicherweise spezielle Berechtigungen für den Benutzer, um darauf zuzugreifen.

> [!NOTE]
> Bei Geräten, die mehrere Benutzer unterstützen, stellt Android jedem Benutzer ein eigenes Verzeichnis im internen und externen Speicher zur Verfügung. Auf dieses Verzeichnis kann von anderen Benutzern auf dem Gerät nicht zugegriffen werden. Diese Trennung ist für apps unsichtbar, solange Sie keine Pfade zu Dateien im internen oder externen Speicher hart codieren.

Als Faustregel gilt, dass xamarin. Android-Apps Ihre Dateien im internen Speicher speichern sollten, wenn dies angemessen ist, und sich auf externen Speicher verlassen müssen, wenn Dateien für andere apps freigegeben werden müssen, sehr groß sind oder beibehalten werden sollten, auch wenn die APP deinstalliert wird. Eine Konfigurationsdatei eignet sich z. b. am besten für einen internen Speicher, da Sie keine Wichtigkeit hat, außer der APP, die Sie erstellt.  Im Gegensatz dazu sind Fotos ein guter Kandidat für die externe Speicherung. Sie können sehr groß sein, und in vielen Fällen kann der Benutzer Sie freigeben oder darauf zugreifen, auch wenn die APP deinstalliert wird.

Dieser Leitfaden konzentriert sich auf den internen Speicher. Ausführliche Informationen zur Verwendung von externem Speicher in einer xamarin. Android-Anwendung finden Sie im Handbuch für [externe Speicherung](~/android/platform/files/external-storage.md) .

## <a name="working-with-internal-storage"></a>Arbeiten mit internem Speicher

Das interne Speicher Verzeichnis für eine Anwendung wird vom Betriebssystem bestimmt und für Android-Apps durch die `Android.Content.Context.FilesDir`-Eigenschaft verfügbar gemacht. Dadurch wird ein `Java.IO.File` Objekt zurückgegeben, das das Verzeichnis darstellt, das Android ausschließlich für die APP dediziert hat.  Beispielsweise kann eine APP mit dem Paketnamen **com. CompanyName** das interne Speicher Verzeichnis sein:

```bash
/data/user/0/com.companyname/files
```

In diesem Dokument wird das interne Speicher Verzeichnis als _interner\_Speicher_bezeichnet.

> [!IMPORTANT]
> Der genaue Pfad zum internen Speicher Verzeichnis kann von Gerät zu Gerät und zwischen Android-Versionen variieren. Aus diesem Grund dürfen apps den Pfad zum Speicher Verzeichnis der internen Dateien nicht hart codieren und stattdessen die xamarin. Android-APIs verwenden, wie z. b. `System.Environment.GetFolderPath()`.

Um die Code Freigabe zu maximieren, sollten xamarin. Android-Apps (oder xamarin. Forms-Apps für xamarin. Android) die [`System.Environment.GetFolderPath()`](xref:System.Environment.GetFolderPath*) -Methode verwenden. In xamarin. Android gibt diese Methode eine Zeichenfolge für ein Verzeichnis zurück, das den gleichen Speicherort wie `Android.Content.Context.FilesDir`hat. Diese Methode verwendet eine Enumeration, `System.Environment.SpecialFolder`, die verwendet wird, um einen Satz von enumerierten Konstanten zu identifizieren, die die Pfade spezieller Ordner darstellen, die vom Betriebssystem verwendet werden. Nicht alle `System.Environment.SpecialFolder` Werte werden einem gültigen Verzeichnis in xamarin. Android zugeordnet. In der folgenden Tabelle wird beschrieben, welchen Pfad Sie für einen bestimmten Wert von `System.Environment.SpecialFolder`erwarten können:

| System. Environment. SpecialFolder | Pfad  |
|----------------------|---|
| `ApplicationData` | **_Interner\_Speicher_/.config** |
| `Desktop` | **_Interner\_Speicher_/Desktop** |
| `LocalApplicationData` | **_Interner\_Speicher_/.local/share** |
| `MyDocuments` | **_Interner\_Speicher_** |
| `MyMusic` | **_Interner\_Speicher_/Music** |
| `MyPictures` | **_Interner\_Speicher_/Pictures** |
| `MyVideos` | **_Interner\_Speicher_/Videos** |
| `Personal` | **_Interner\_Speicher_** |

### <a name="reading-or-writing-to-files-on-internal-storage"></a>Lesen oder schreiben in Dateien im internen Speicher

Jede der [ C# APIs zum Schreiben](https://docs.microsoft.com/dotnet/csharp/programming-guide/file-system/how-to-write-to-a-text-file) in eine Datei ist ausreichend. der Pfad zu der Datei, die sich in dem Verzeichnis befindet, das der Anwendung zugeordnet ist, muss lediglich angezeigt werden. Es wird dringend empfohlen, dass die asynchronen Versionen der .NET-APIs verwendet werden, um Probleme zu minimieren, die möglicherweise mit dem Dateizugriff verknüpft sind, der den Haupt Thread blockiert.

Dieser Code Ausschnitt ist ein Beispiel für das Schreiben einer Ganzzahl in eine UTF-8-Textdatei in das interne Speicher Verzeichnis einer Anwendung:

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

Der nächste Code Ausschnitt bietet eine Möglichkeit, einen ganzzahligen Wert zu lesen, der in einer Textdatei gespeichert wurde:

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

## <a name="using--xamarinessentials-ndash-file-system-helpers"></a>Verwenden von xamarin. Essentials &ndash; File System-Hilfsprogramme

[Xamarin. Essentials](~/essentials/file-system-helpers.md?context=xamarin/android) ist ein Satz von APIs zum Schreiben von plattformübergreifendes kompatiblem Code. Die [Datei System](~/essentials/file-system-helpers.md?context=xamarin/android) -Hilfsprogramme sind eine-Klasse, die eine Reihe von Hilfsprogrammen enthält, um die Suche nach dem Cache und den Daten Verzeichnissen der Anwendung zu vereinfachen. Dieser Code Ausschnitt enthält ein Beispiel für die Suche nach dem internen Speicher Verzeichnis und dem Cache Verzeichnis für eine APP:

```csharp
// Get the path to a file on internal storage
var backingFile = Path.Combine(Xamarin.Essentials.FileSystem.AppDataDirectory, "count.txt");

// Get the path to a file in the cache directory
var cacheFile = Path.Combine(Xamarin.Essentials.FileSystem.CacheDirectory, "count.txt");
```

## <a name="hiding-files-from-the-mediastore"></a>Ausblenden von Dateien aus der `MediaStore`

Der `MediaStore` ist eine Android-Komponente, die Metadaten zu Mediendateien (Videos, Musik, Bilder) auf einem Android-Gerät sammelt. Der Zweck besteht darin, die Freigabe dieser Dateien für alle Android-Apps auf dem Gerät zu vereinfachen.

Private Dateien werden nicht als Share Bare Medien angezeigt. Wenn eine APP beispielsweise ein Bild in Ihrem privaten externen Speicher speichert, wird diese Datei nicht vom Medien Scanner (`MediaStore`) übernommen.

Öffentliche Dateien werden von `MediaStore`abgerufen. Verzeichnisse mit einem Dateinamen, der 0 (null) ist **. Nomedia** wird nicht von `MediaStore`gescannt.

## <a name="related-links"></a>Verwandte Links

* [Externer Speicher](~/android/platform/files/external-storage.md)
* [Speichern von Dateien auf dem Gerätespeicher](https://developer.android.com/training/data-storage/files)
* [Xamarin. Essentials-Datei System Hilfsprogramme](~/essentials/file-system-helpers.md?context=xamarin/android)
* [Sichern von Benutzerdaten mit automatischer Sicherung](https://developer.android.com/guide/topics/data/autobackup)
* [Adoptable-Speicher](https://source.android.com/devices/storage/adoptable)
