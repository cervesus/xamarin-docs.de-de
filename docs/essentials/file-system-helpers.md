---
title: 'Xamarin.Essentials: Dateisystemhilfsprogramme'
description: Die FileSystem-Klasse in Xamarin.Essentials enthält einige Hilfsprogramme, um den Cache und die Datenverzeichnisse der Anwendung zu suchen und Dateien im App-Paket zu öffnen.
ms.assetid: B3EC2DE0-EFC0-410C-AF71-7410AE84CF84
author: jamesmontemagno
ms.custom: video
ms.author: jamont
ms.date: 11/04/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: eb35750372c5ccb878c7b38f9d25898b09fd7f1e
ms.sourcegitcommit: e412858ce431b3280c88241e324fcab33066eb58
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/21/2020
ms.locfileid: "86865896"
---
# <a name="xamarinessentials-file-system-helpers"></a>Xamarin.Essentials: Dateisystemhilfsprogramme

Die Klasse **FileSystem** enthält eine Reihe von Hilfsprogrammen, um den Cache und die Datenverzeichnisse der Anwendung zu finden und Dateien im App-Paket zu öffnen.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-file-system-helpers"></a>Verwenden der Dateisystemhilfsprogramme

Fügen Sie in Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

So rufen Sie das Anwendungsverzeichnis zum Speichern von **Cachedaten** ab. Cachedaten können für alle Daten verwendet werden, die länger als temporäre Daten aufbewahrt werden müssen. Dabei sollte es sich aber nicht um Daten handeln, die für den ordnungsgemäßen Betrieb erforderlich sind, da das Betriebssystem vorgibt, wann dieser Speicher gelöscht wird.

```csharp
var cacheDir = FileSystem.CacheDirectory;
```

So rufen Sie das oberste Anwendungsverzeichnis für alle Dateien ab, die keine Benutzerdatendateien sind. Diese Dateien werden über das Synchronisierungsframework des Betriebssystems gesichert. Die Besonderheiten bei der plattformspezifischen Implementierung sind weiter unten beschrieben.

```csharp
var mainDir = FileSystem.AppDataDirectory;
```

So öffnen Sie eine Datei, die im Anwendungspaket gebündelt ist:

```csharp
 using (var stream = await FileSystem.OpenAppPackageFileAsync(templateFileName))
 {
    using (var reader = new StreamReader(stream))
    {
        var fileContents = await reader.ReadToEndAsync();
    }
 }
```

## <a name="platform-implementation-specifics"></a>Besonderheiten bei der plattformspezifischen Implementierung

# <a name="android"></a>[Android](#tab/android)

- **CacheDirectory**: Gibt das Verzeichnis [CacheDir](https://developer.android.com/reference/android/content/Context.html#getCacheDir) des aktuellen Kontexts zurück.
- **AppDataDirectory**: Gibt das Verzeichnis [FilesDir](https://developer.android.com/reference/android/content/Context.html#getFilesDir) des aktuellen Kontexts zurück. Die Sicherung erfolgt mit der [automatischen Sicherung](https://developer.android.com/guide/topics/data/autobackup.html) ab API-Ebene 23 und höher.

Fügen Sie eine beliebige Datei zum Ordner **Assets** im Android Projekt hinzu, und markieren Sie den Buildvorgang als **AndroidAsset**, um ihn mit `OpenAppPackageFileAsync` zu verwenden.

# <a name="ios"></a>[iOS](#tab/ios)

- **CacheDirectory**: Gibt das Verzeichnis [Library/Caches](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html) zurück.
- **AppDataDirectory**: Gibt das Verzeichnis [Library](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html) zurück, das von iTunes und iCloud gesichert wird.

> [!IMPORTANT]
> Im iOS-Simulator wird die Anwendungs-ID (die Teil des Verzeichnisnamens ist) mit jedem Build geändert. Daher müssen Sie die jedes Mal die richtige ID abrufen, wenn Sie Ihre Anwendung für den Simulator erstellen.

Fügen Sie eine beliebige Datei zum Ordner **Resources** im iOS-Projekt hinzu, und markieren Sie den Buildvorgang als **BundledResource**, um ihn mit `OpenAppPackageFileAsync` zu verwenden.

# <a name="uwp"></a>[UWP](#tab/uwp)

- **CacheDirectory**: Gibt das Verzeichnis [LocalCacheFolder](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localcachefolder#Windows_Storage_ApplicationData_LocalCacheFolder) zurück.
- **AppDataDirectory**: Gibt das Verzeichnis [LocalFolder](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localfolder#Windows_Storage_ApplicationData_LocalFolder) zurück, das in der Cloud gesichert wird.

Fügen Sie eine beliebige Datei zum Stammverzeichnis des UWP-Projekts hinzu, und markieren Sie den Buildvorgang als **Content**, um ihn mit `OpenAppPackageFileAsync` zu verwenden.

--------------

## <a name="api"></a>API

- [FileSystem-Hilfsprogramme – Quellcode](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/FileSystem)
- [FileSystem-API-Dokumentation](xref:Xamarin.Essentials.FileSystem)

## <a name="related-video"></a>Zugehörige Videos

> [!Video https://channel9.msdn.com/Shows/XamarinShow/File-System-Helpers-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
