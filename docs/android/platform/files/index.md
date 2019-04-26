---
title: Den Zugriff auf Dateien, die mit Xamarin.Android
description: Dieses Handbuch wird erläutert, die Zugriff auf Dateien in Xamarin.Android
ms.prod: xamarin
ms.assetid: FC1CFC58-B799-4DD6-8ED1-DE36B0E56856
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/23/2018
ms.openlocfilehash: 2978f0b2bcbdd463876784a9addd7dec055b8af9
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "60949416"
---
# <a name="file-storage-and-access-with-xamarinandroid"></a>File Storage sowie den Zugriff, die mit Xamarin.Android

Eine häufige Anforderung für Android-apps ist zum Bearbeiten von Dateien &ndash; Speichern von Bildern, Dokumente werden heruntergeladen und Exportieren von Daten für andere Programme freigeben. Android (die auf Linux basiert) unterstützt durch die Bereitstellung von Speicherplatz für File Storage. Android gruppiert das Dateisystem in zwei verschiedene Arten von Speicher:

* **Interner Speicher** &ndash; Dies ist ein Teil des Dateisystems, die nur durch die Anwendung oder das Betriebssystem zugegriffen werden kann.
* **Externen Speicher** &ndash; Dies ist eine Partition für die Speicherung von Dateien, die über alle apps, die Benutzer und möglicherweise auf anderen Geräten zugegriffen werden kann. Auf einigen Geräten möglicherweise externer Speicher Wechselmedien (z. B. einer SD-Karte).

Diese Gruppierungen sind nur konzeptionellen und nicht unbedingt finden Sie in einer einzelnen Partition oder eines Verzeichnisses auf dem Gerät. Ein Android-Gerät wird immer Partition für interne und externe Speicher bereit. Es ist möglich, dass bestimmte Geräte über mehrere Partitionen verfügen können, die berücksichtigt werden, um externen Speicher zu werden. Unabhängig von der Partition die APIs zum Lesen ist das Schreiben und Erstellen von Dateien identisch. Es gibt zwei Sätze an APIs, die für den Zugriff auf eine Xamarin.Android-Anwendung verwenden können:

1. **Die .NET APIs (von Mono bereitgestellt und von Xamarin.Android eingeschlossen)** &ndash; dazu gehören die [dateisystemhilfsprogramme](~/essentials/file-system-helpers.md?context=xamarin/android) gebotenen [Xamarin.Essentials](~/essentials/index.md?context=xamarin/android). Die .NET APIs bieten die beste plattformübergreifende Kompatibilität und daher der Schwerpunkt dieses Leitfadens wird auf diese APIs.
1. **Die systemeigenen Java-Dateizugriff-APIs (bereitgestellt von Java und umschlossen von Xamarin.Android)** &ndash; Java stellt eine eigene APIs zum Lesen und Schreiben von Dateien bereit. Diese sind eine Alternative vollkommen akzeptabel, auf die .NET APIs, jedoch gelten für Android und eignen sich nicht für apps, die plattformübergreifend sein sollen.

Lesen und Schreiben in Dateien ist fast identisch in Xamarin.Android, wie an eine beliebige andere .NET-Anwendung. Die Xamarin.Android-app bestimmt den Pfad zur Datei, die bearbeitet wird, klicken Sie dann verwendet standard .NET Ausdrücke für den Dateizugriff. Da die eigentlichen Pfade in den internen und externen Speicher nach Gerät variieren und von Android-Version auf Android-Version wird nicht durch hartcodierung der Pfad zu den Dateien empfohlen. Verwenden Sie stattdessen die Xamarin.Android-APIs, um den Pfad zu Dateien zu ermitteln. Auf diese Weise stellt die .NET APIs zum Lesen und Schreiben von Dateien der systemeigenen Android-APIs, mit deren Hilfe wird, um zu bestimmen, den Pfad zu Dateien im internen und externen Speicher.

Ehe wir näher auf die APIs, die mit den Zugriff auf Dateien, ist es wichtig, einige Details Zusammenhang mit internen und externen Speicher zu verstehen. Dies wird im nächsten Abschnitt erläutert werden.

## <a name="internal-vs-external-storage"></a>Interner und externer Speicher

Im Prinzip internen Speicher und externen Speicher sind sehr ähnlich &ndash; werden die beiden Orten, an dem eine Xamarin.Android-app möglicherweise Dateien speichern. Diese Ähnlichkeit kann für Entwickler verwirrend sein, die nicht mit Android vertraut sind, wie nicht ersichtlich ist, wenn eine app internen Speicher und externen Speicher verwenden soll.

Interner Speicher bezieht sich auf den permanenten Speicher, den Android APKs, das Betriebssystem und für einzelne apps zuordnet. Dieser Speicherplatz ist nicht nur durch das Betriebssystem oder die apps zugegriffen werden kann. Android wird ein Verzeichnis in der Partition für die interne Speicherung für jede app zugewiesen. Wenn die app deinstalliert wird, werden alle Dateien, die auf den internen Speicher in diesem Verzeichnis gespeichert werden ebenfalls gelöscht. Interner Speicher eignet sich optimal für Dateien, die nur für die app zugänglich sind, und wird nicht mit anderen apps freigegeben werden oder nur sehr wenig Wert vorhanden, sobald die app deinstalliert wird. Auf Android 6.0 oder höher verwenden, Dateien auf den internen Speicher möglicherweise automatisch gesichert werden mithilfe von Google die [automatische Sicherung-Funktion in Android 6.0](https://developer.android.com/guide/topics/data/autobackup). Interner Speicher hat folgende Nachteile:

* Dateien können nicht freigegeben werden.
* Dateien werden gelöscht werden, wenn die app deinstalliert wird.
* Der verfügbare Speicherplatz auf internen Speicher möglicherweise eingeschränkt.

Externer Speicher bezieht sich auf die Speicherung von Dateien, die nicht die interne Speicherung ist nicht ausschließlich auf eine app zugreifen kann. Der primäre Zweck von externen Speicher ist, geben Sie einen Ort zum Speichern von Dateien, die von den apps gemeinsam verwendet werden sollen, oder sind zu groß für den internen Speicher. Der Vorteil des externen Speicher ist, dass sie in der Regel viel mehr Speicherplatz für Dateien als interner Speicher hat. Allerdings werden die externer Speicher ist nicht immer unbedingt auf einem Gerät vorhanden und erfordern spezielle Berechtigung vom Benutzer darauf zugreifen.

> [!NOTE]
> Für Geräte, die mehrere Benutzer zu unterstützen, bieten Android jeder Benutzer ihre eigenen Verzeichnis für interne und externe Speicher. Dieses Verzeichnis ist nicht möglich, anderen Benutzern auf dem Gerät. Aufgrund dieser Trennung ist für apps unsichtbar, solange sie nicht hartcodieren Pfade zu Dateien im internen oder externen Speicher ausführen.

Als Faustregel gilt sollten Xamarin.Android-apps bevorzugen, speichern ihre Dateien auf den internen Speicher, wenn es sinnvoll ist, und basieren auf externen speichern, wenn Dateien mit anderen apps freigegeben werden müssen, sind sehr große oder beibehalten werden soll, auch wenn die app deinstalliert wird. Beispielsweise ist eine Konfigurationsdatei für einen internen Speicher am besten geeignet, wie hat keine Bedeutung außer der App, die es erstellt.  Im Gegensatz dazu sind Fotos ein guter Kandidat für die externe Speicherung. Sie können sehr groß sein und in vielen Fällen der Benutzer möchte eventuell freigeben bzw. darauf zugreifen, selbst wenn die app deinstalliert wird.

Dieses Handbuch konzentriert sich auf den internen Speicher. Sie finden Sie im Handbuch [externen Speicher](~/android/platform/files/external-storage.md) für Informationen zur Verwendung von externen Speichers in einer Xamarin.Android-Anwendung.

## <a name="working-with-internal-storage"></a>Arbeiten mit internen Speicher

Das Verzeichnis der internen Speicher für eine Anwendung richtet sich nach dem Betriebssystem und für Android-apps verfügbar gemacht wird die `Android.Content.Context.FilesDir` Eigenschaft. Dies ergibt eine `Java.IO.File` Objekt, das das Verzeichnis, das Android ausschließlich für die app reserviert hat darstellt.  Z. B. einer app durch den Namen des Pakets **com.companyname** ist möglicherweise das Verzeichnis für die interne Speicherung:

```bash
/data/user/0/com.companyname/files
```

In diesem Dokument im internen Speicherverzeichnis wie bezeichnen _intern\_STORAGE_.

> [!IMPORTANT]
> Der genaue Pfad in das Verzeichnis für die interne Speicherung kann vom Gerät zu Gerät und zwischen Versionen von Android variieren. Aus diesem Grund apps müssen nicht hart codieren Sie den Pfad zum Verzeichnis mit internen Speicher, und verwenden Sie stattdessen die Xamarin.Android-APIs, wie z. B. `System.Environment.GetFolderPath()`.

Zur Maximierung der Freigabe von Code Xamarin.Android-apps (oder eine Xamarin.Forms-apps, die für Xamarin.Android) verwenden sollte die [ `System.Environment.GetFolderPath()` ](xref:System.Environment.GetFolderPath*) Methode. In Xamarin.Android gibt diese Methode eine Zeichenfolge für ein Verzeichnis am gleichen Speicherort wie `Android.Content.Context.FilesDir`. Diese Methode akzeptiert eine Enumeration, `System.Environment.SpecialFolder`, dient zur Kennzeichnung einer Gruppe von Enumerationskonstanten, die die Pfade der besondere Ordner, die vom Betriebssystem verwendeten darstellen. Nicht alle der `System.Environment.SpecialFolder` Werte zugeordnet, ein gültiges Verzeichnis auf Xamarin.Android. Die folgende Tabelle beschreibt, welcher Pfad für einen bestimmten Wert von ausfallen kann `System.Environment.SpecialFolder`:

| System.Environment.SpecialFolder | Pfad  |
|----------------------|---|
| `ApplicationData` | **_INTERNAL\_STORAGE_/.config** |
| `Desktop` | **_INTERNE\_STORAGE_  /Desktop** |
| `LocalApplicationData` | **_INTERNE\_STORAGE_/.local/share** |
| `MyDocuments` | **_INTERNE\_SPEICHER_** |
| `MyMusic` | **_INTERNAL\_STORAGE_/Music** |
| `MyPictures` | **_INTERNE\_STORAGE_  /Pictures** |
| `MyVideos` | **_INTERNAL\_STORAGE_/Videos** |
| `Personal` | **_INTERNE\_SPEICHER_** |


### <a name="reading-or-writing-to-files-on-internal-storage"></a>Lesen oder Schreiben in Dateien auf den internen Speicher

Keines der [ C# APIs für das Schreiben von](https://docs.microsoft.com/dotnet/csharp/programming-guide/file-system/how-to-write-to-a-text-file) in eine Datei sind ausreichend; erforderlich ist lediglich um den Pfad zu der Datei abzurufen, die in das Verzeichnis der Anwendung zugeordnet ist. Es wird dringend empfohlen, dass der Hauptthread blockiert den Zugriff auf Dateien der asynchronen Versionen der .NET APIs verwendet werden, um Probleme zu minimieren, die möglicherweise zugeordnet.

Dieser Codeausschnitt ist ein Beispiel für eine ganze Zahl in eine UTF-8-Textdatei in das Verzeichnis der internen Speicher einer Anwendung zu schreiben:

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

## <a name="using--xamarinessentials-ndash-file-system-helpers"></a>Mithilfe von Xamarin.Essentials &ndash; Dateisystemhilfsprogramme

[Xamarin.Essentials](~/essentials/file-system-helpers.md?context=xamarin/android) ein Satz von APIs für das Schreiben von plattformübergreifend kompatibel ist. Die [Datei System Hilfsprogramme](~/essentials/file-system-helpers.md?context=xamarin/android) ist eine Klasse, die eine Reihe von Hilfsmethoden zur Vereinfachung der Suche nach der Anwendung Cache und Datenverzeichnisse enthält. Dieser Codeausschnitt bietet ein Beispiel für das Verzeichnis für die interne Speicherung und das Cacheverzeichnis für eine app zu suchen:

```csharp
// Get the path to a file on internal storage
var backingFile = Path.Combine(Xamarin.Essentials.FileSystem.AppDataDirectory, "count.txt");

// Get the path to a file in the cache directory
var cacheFile = Path.Combine(Xamarin.Essentials.FileSystem.CacheDirectory, "count.txt");
```

## <a name="hiding-files-from-the-mediastore"></a>Ausblenden von Dateien aus dem `MediaStore`

Die `MediaStore` ist eine Android-Komponente, die sammelt Metadaten über die Mediendateien (Videos, Musik, Bilder) auf einem Android-Gerät. Ihr Zweck ist die gemeinsame Nutzung dieser Dateien für alle Android-apps auf dem Gerät vereinfachen.

Private Dateien werden nicht als freigegebenen Medium angezeigt. Z. B. wenn eine app ein Bild für den privaten externen Speicher gespeichert wird, klicken Sie dann die Datei nicht übernommen durch die Überprüfung von Medien (`MediaStore`).

Öffentliche Dateien werden von abgeholt `MediaStore`. Verzeichnisse, die einen NULL Byte-Dateinamen haben **. NOMEDIA** werden nicht überprüft werden, von `MediaStore`.

## <a name="related-links"></a>Verwandte Links

* [Externer Speicher](~/android/platform/files/external-storage.md)
* [Speichern von Dateien auf den Speicher des Geräts](https://developer.android.com/training/data-storage/files)
* [System-Hilfsprogramme Xamarin.Essentials-Datei](~/essentials/file-system-helpers.md?context=xamarin/android)
* [Sicherung von Benutzerdaten mit automatische Sicherung](https://developer.android.com/guide/topics/data/autobackup)
* [Überwindenden Speicher](https://source.android.com/devices/storage/adoptable)
