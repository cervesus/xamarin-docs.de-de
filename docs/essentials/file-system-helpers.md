---
title: 'Xamarin.Essentials: Dateisystemhilfsprogramme'
description: Die Klasse „FileSystem“ in Xamarin.Essentials enthält eine Reihe von Hilfsprogrammen, um den Cache und die Datenverzeichnisse der Anwendung zu finden und Dateien im App-Paket zu öffnen.
ms.assetid: B3EC2DE0-EFC0-410C-AF71-7410AE84CF84
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: 5b155e4976a67bda36e66d2ca3565c9237fde3c6
ms.sourcegitcommit: c6e56545eafd8ff9e540d56aba32aa6232c5315f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2019
ms.locfileid: "68738866"
---
# <a name="xamarinessentials-file-system-helpers"></a>Xamarin.Essentials: Dateisystemhilfsprogramme

Die Klasse **FileSystem** enthält eine Reihe von Hilfsprogrammen, um den Cache und die Datenverzeichnisse der Anwendung zu finden und Dateien im App-Paket zu öffnen.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-file-system-helpers"></a>Verwenden der Dateisystemhilfsprogramme

Fügen Sie Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

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

# <a name="androidtabandroid"></a>[Android](#tab/android)

- **CacheDirectory**: Gibt das Verzeichnis [CacheDir](https://developer.android.com/reference/android/content/Context.html#getCacheDir) des aktuellen Kontexts zurück.
- **AppDataDirectory**: Gibt das Verzeichnis [FilesDir](https://developer.android.com/reference/android/content/Context.html#getFilesDir) des aktuellen Kontexts zurück. Die Sicherung erfolgt mit der [automatischen Sicherung](https://developer.android.com/guide/topics/data/autobackup.html) ab API-Ebene 23 und höher.

Fügen Sie eine beliebige Datei zum Ordner **Assets** im Android Projekt hinzu, und markieren Sie den Buildvorgang als **AndroidAsset**, um ihn mit `OpenAppPackageFileAsync` zu verwenden.

# <a name="iostabios"></a>[iOS](#tab/ios)

- **CacheDirectory**: Gibt das Verzeichnis [Library/Caches](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html) zurück.
- **AppDataDirectory**: Gibt das Verzeichnis [Library](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html) zurück, das von iTunes und iCloud gesichert wird.

Fügen Sie eine beliebige Datei zum Ordner **Resources** im iOS-Projekt hinzu, und markieren Sie den Buildvorgang als **BundledResource**, um ihn mit `OpenAppPackageFileAsync` zu verwenden.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

- **CacheDirectory**: Gibt das Verzeichnis [LocalCacheFolder](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localcachefolder#Windows_Storage_ApplicationData_LocalCacheFolder) zurück.
- **AppDataDirectory**: Gibt das Verzeichnis [LocalFolder](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localfolder#Windows_Storage_ApplicationData_LocalFolder) zurück, das in der Cloud gesichert wird.

Fügen Sie eine beliebige Datei zum Stammverzeichnis des UWP-Projekts hinzu, und markieren Sie den Buildvorgang als **Content**, um ihn mit `OpenAppPackageFileAsync` zu verwenden.

--------------

## <a name="api"></a>API

- [FileSystem-Hilfsprogramme – Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/FileSystem)
- [FileSystem-API-Dokumentation](xref:Xamarin.Essentials.FileSystem)
