---
title: System-Hilfsprogrammen Xamarin.Essentials-Datei
description: Der FileSystem-Klasse enthält eine Reihe von Hilfsmethoden zum Suchen von Cache der Anwendung und die Datenverzeichnisse und öffnen die Dateien in das app-Paket.
ms.assetid: B3EC2DE0-EFC0-410C-AF71-7410AE84CF84
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 871d164df982d1d170e8ba5bffd3bd6600a4cdda
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-file-system-helpers"></a>System-Hilfsprogrammen Xamarin.Essentials-Datei

![Vorabversion NuGet](~/media/shared/pre-release.png)

Die **FileSystem** Klasse enthält eine Reihe von Hilfsprogrammen, Verzeichnisse für die Anwendung-Caches und Daten suchen und Öffnen von Dateien in das app-Paket.

## <a name="using-file-system-helpers"></a>Verwenden von Datei-System-Hilfsprogrammen

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Im Verzeichnis der Anwendung zum Speichern von abzurufenden **Zwischenspeichern von Daten**. Zwischenspeichern von Daten können für alle Daten verwendet werden, die länger als temporäre Daten beibehalten werden muss, jedoch muss sich nicht auf Daten, die den ordnungsgemäßen Betrieb erforderlich sind.

```csharp
var cacheDir = FileSystem.CacheDirectory;
```

Um die Anwendung der obersten Ebene Diredctory für alle Dateien zu erhalten, die nicht Benutzerdatendateien sind. Diese Dateien werden mit dem Betriebssystem synchronisieren Framework gesichert. Finden Sie unter Plattform Implementierungsspezifika ist unten.

```csharp
var mainDir = FileSystem.AppDataDirectory;
```

Öffnen eine Datei, die in das Anwendungspaket gebündelt ist:

```csharp
 using (var stream = await FileSystem.OpenAppPackageFileAsync(templateFileName))
 {
    using (var reader = new StreamReader(stream))
    {
        var fileContents = await reader.ReadToEndAsync();
    }
 }
```

## <a name="platform-implementation-specifics"></a>Plattform Implementierungsspezifika

# <a name="androidtabandroid"></a>[Android](#tab/android)

- **CacheDirectory** – gibt die [CacheDir](https://developer.android.com/reference/android/content/Context.html#getCacheDir) des aktuellen Kontexts.
- **AppDataDirectory** – gibt die [FilesDir](https://developer.android.com/reference/android/content/Context.html#getFilesDir) den aktuellen Kontext und sind mit gesichert [Autu Sicherung](https://developer.android.com/guide/topics/data/autobackup.html) auf API-23 und höher ab.

Fügen Sie alle Dateien in der **Bestand** Ordner in der Android Projekt, und markieren Sie den Buildvorgang als **AndroidAsset** mit `OpenAppPackageFileAsync`.

# <a name="iostabios"></a>[iOS](#tab/ios)

- **CacheDirectory** – gibt die [Library-Caches](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html) Verzeichnis.
- **AppDataDirectory** – gibt die [Bibliothek](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html) Verzeichnis, das von iTunes- und iCloud gesichert wird.

Fügen Sie alle Dateien in der **Ressourcen** Ordner in der iOS-Projekt, und markieren Sie den Buildvorgang als **BundledResource** mit `OpenAppPackageFileAsync`.

# <a name="uwptabuwp"></a>[UNIVERSELLE WINDOWS-PLATTFORM](#tab/uwp)

- **CacheDirectory** – gibt die [LocalCacheFolder](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdata.localcachefolder#Windows_Storage_ApplicationData_LocalCacheFolder) Directory...
- **AppDataDirectory** – gibt die [LocalFolder](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdata.localfolder#Windows_Storage_ApplicationData_LocalFolder) Verzeichnis, das in der Cloud wurden gesichert.

Fügen Sie alle Dateien in den Stamm in uwp-Projekt hinzu, und markieren Sie den Buildvorgang als **Content** mit `OpenAppPackageFileAsync`.

--------------

## <a name="api"></a>API

- [Datei System Hilfsprogramme-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/FileSystem)
- [Datei System-API-Dokumentation](xref:Xamarin.Essentials.FileSystem)
