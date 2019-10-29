---
title: Dateizugriff auf externen Speicher mit xamarin. Android
description: In diesem Leitfaden wird der Dateizugriff auf externen Speicher in xamarin. Android erörtert.
ms.prod: xamarin
ms.assetid: 40da10b2-a207-4f9c-a2dd-165d9b662f33
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 07/23/2018
ms.openlocfilehash: 96b0d6a00c7825939b1f89ed63e3e5559ca4ef59
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73020479"
---
# <a name="external-storage"></a>Externer Speicher

Externer Speicher bezieht sich auf Dateispeicher, der sich nicht im internen Speicher befindet und nicht ausschließlich für die APP zugänglich ist, die für die Datei verantwortlich ist. Der primäre Zweck von externem Speicher ist die Bereitstellung von Dateien, die für die gemeinsame Nutzung von apps verwendet werden sollen oder die zu groß für den internen Speicher sind.

In der Vergangenheit hat externer Speicher auf Wechselmedien wie z. b. eine SD-Karte (auch als _portabler Speicher_bezeichnet) eine Datenträger Partition bezeichnet. Dieser Unterschied ist nicht mehr so relevant, wie sich Android-Geräte entwickelt haben, und viele Android-Geräte unterstützen keine Wechselmedien mehr. Stattdessen weisen einige Geräte Ihren internen, nicht flüchtigen Speicher zu, der Android die gleichen Funktions Wechselmedien ausführt. Dies wird als _emulierten_ Speicher bezeichnet und wird weiterhin als externer Speicher betrachtet. Alternativ können einige Android-Geräte über mehrere externe Speicher Partitionen verfügen. Beispielsweise verfügt ein Android-Tablet (zusätzlich zum internen Speicher) möglicherweise über emulierten Speicher und einen oder mehrere Slots für eine SD-Karte. Alle diese Partitionen werden von Android als externer Speicher behandelt.

Auf Geräten mit mehreren Benutzern erhält jeder Benutzer ein dediziertes Verzeichnis auf der primären externen Speicher Partition für den externen Speicher. Apps, die als ein Benutzer ausgeführt werden, haben keinen Zugriff auf Dateien von einem anderen Benutzer auf dem Gerät. Die Dateien für alle Benutzer sind nach wie vor weltweit lesbar und weltweit beschreibbar. Allerdings werden von Android alle Benutzerprofile von den anderen Benutzern in der Sandbox angezeigt.

Das Lesen und Schreiben in Dateien ist nahezu identisch mit xamarin. Android, wie es bei jeder anderen .NET-Anwendung der Fall ist. Die xamarin. Android-App bestimmt den Pfad zu der Datei, die bearbeitet wird, und verwendet dann die standardmäßige .net-Ausdrücke für den Dateizugriff. Da sich die tatsächlichen Pfade zum internen und externen Speicher von Gerät zu Gerät oder von Android-Version zu Android-Version unterscheiden können, empfiehlt es sich nicht, den Pfad zu den Dateien hart zu codieren. Stattdessen stellt xamarin. Android die nativen Android-APIs zur Verfügung, die bei der Ermittlung des Pfads zu Dateien im internen und externen Speicher helfen.

In diesem Handbuch werden die Konzepte und APIs in Android erläutert, die für den externen Speicher spezifisch sind.

## <a name="public-and-private-files-on-external-storage"></a>Öffentliche und private Dateien im externen Speicher

Es gibt zwei verschiedene Typen von Dateien, die eine APP im externen Speicher aufbewahren kann:

* **Private** Dateien &ndash; privaten Dateien sind Dateien, die für Ihre Anwendung spezifisch sind (aber immer noch weltweit lesbar und weltweit beschreibbar sind). Android erwartet, dass private Dateien in einem bestimmten Verzeichnis im externen Speicher gespeichert werden. Obwohl die Dateien als "Privat" bezeichnet werden, sind Sie weiterhin sichtbar und können von anderen apps auf dem Gerät zugänglich gemacht werden. Sie bieten keinen besonderen Schutz durch Android.

* **Öffentliche** Dateien &ndash; Dies sind Dateien, die nicht als spezifisch für die Anwendung angesehen werden und die freigegeben werden sollen.

Die Unterschiede zwischen diesen Dateien sind primär konzeptionell. Private Dateien sind in dem Sinne privat, dass Sie als Teil der Anwendung angesehen werden, und öffentliche Dateien sind andere Dateien, die im externen Speicher vorhanden sind. Android bietet zwei verschiedene APIs zum Auflösen der Pfade zu privaten und öffentlichen Dateien. andernfalls werden dieselben .NET-APIs zum Lesen und Schreiben in diese Dateien verwendet. Dabei handelt es sich um dieselben APIs, die im Abschnitt zum [Lesen und schreiben](~/android/platform/files/index.md#reading-or-writing-to-files-on-internal-storage)erläutert werden.

### <a name="private-external-files"></a>Private externe Dateien

Private externe Dateien gelten als spezifisch für eine Anwendung (ähnlich wie bei internen Dateien), werden jedoch aus verschiedenen Gründen in externem Speicher aufbewahrt (z. b. zu groß für den internen Speicher). Ähnlich wie bei internen Dateien werden diese Dateien gelöscht, wenn die APP vom Benutzer deinstalliert wird.

Der primäre Speicherort für private externe Dateien wird durch Aufrufen der-Methode `Android.Content.Context.GetExternalFilesDir(string type)`gefunden. Diese Methode gibt ein `Java.IO.File` Objekt zurück, das das private externe Speicher Verzeichnis für die APP darstellt. Wenn Sie `null` an diese Methode übergeben, wird der Pfad zum Speicher Verzeichnis des Benutzers für die Anwendung zurückgegeben. Beispiel: für eine Anwendung mit dem Paketnamen `com.companyname.app`lautet das Verzeichnis "root" der privaten externen Dateien wie folgt:

```bash
/storage/emulated/0/Android/data/com.companyname.app/files/
```

In diesem Dokument wird das Speicher Verzeichnis für private Dateien im externen Speicher als _private\_externe\_Speicher_bezeichnet.

Der-Parameter für `GetExternalFilesDir()` ist eine Zeichenfolge, die ein _Anwendungsverzeichnis_angibt. Hierbei handelt es sich um ein Verzeichnis, das einen Standard Speicherort für eine logische Organisation von Dateien bereitstellen soll. Die Zeichen folgen Werte sind über Konstanten in der `Android.OS.Environment`-Klasse verfügbar:

| `Android.OS.Environment` | Verzeichnis |
|-|-|
| Directoren Alarme | **_Private\_externer\_Storage_/Alarms** |
| Directoriydcim | **_Private\_externer\_Storage_/DCIM** |
| Directoren herunterladen | **_Private\_externer\_Storage_/Download** |
| Directoriydocuments | **_Private\_externer\_Storage_/Documents** |
| Directoren | **_Private\_externer\_Storage_/Movies** |
| Directoren | **_Private\_externer\_Storage_/Music** |
| Directoren | **_Private\_externer\_Storage_/Notifications** |
| Directoren | **_Private\_externer\_Storage_/Podcasts** |
| Directoriyringtones | **_Private\_externer\_Storage_/Ringtones** |
| Directoren | **_Private\_externer\_Storage_/Pictures** |

Bei Geräten, die über mehrere externe Speicher Partitionen verfügen, verfügt jede Partition über ein Verzeichnis, das für private Dateien vorgesehen ist. Die-Methode `Android.Content.Context.GetExternalFilesDirs(string type)` gibt ein Array von `Java.IO.Files`zurück. Jedes-Objekt stellt ein privates anwendungsspezifisches Verzeichnis auf allen freigegebenen/externen Speichergeräten dar, auf denen die Anwendung die Dateien platzieren kann, die es besitzt.

> [!IMPORTANT]
> Der exakte Pfad zum privaten externen Speicher Verzeichnis kann von Gerät zu Gerät und zwischen Android-Versionen variieren. Aus diesem Grund dürfen apps den Pfad zu diesem Verzeichnis nicht hart codieren und stattdessen die xamarin. Android-APIs verwenden, wie z. b. `Android.Content.Context.GetExternalFilesDir()`.

### <a name="public-external-files"></a>Öffentliche externe Dateien

Öffentliche Dateien sind Dateien, die in externem Speicher vorhanden sind und nicht in dem Verzeichnis gespeichert sind, das von Android für private Dateien zugewiesen wird. Öffentliche Dateien werden nicht gelöscht, wenn die APP deinstalliert wird. Android-Apps muss eine Berechtigung erteilt werden, bevor öffentliche Dateien gelesen oder geschrieben werden können. Öffentliche Dateien können an beliebiger Stelle im externen Speicher vorhanden sein, aber gemäß der Konvention erwartet Android, dass öffentliche Dateien im Verzeichnis vorhanden sind, das durch die-Eigenschaft `Android.OS.Environment.ExternalStorageDirectory`identifiziert wird. Diese Eigenschaft gibt ein `Java.IO.File` Objekt zurück, das das primäre externe Speicher Verzeichnis darstellt. Beispielsweise können `Android.OS.Environment.ExternalStorageDirectory` auf das folgende Verzeichnis verweisen:

```bash
/storage/emulated/0/
```

In diesem Dokument wird das Speicher Verzeichnis für öffentliche Dateien in externem Speicher als _Public\_externer\_Storage_bezeichnet.

Android unterstützt auch das Konzept von Anwendungs Verzeichnissen in _öffentlichen\_externen\_Storage_. Diese Verzeichnisse sind exakt mit den Anwendungs Verzeichnissen für `PRIVATE_EXTERNAL_STORAGE` identisch und werden in der Tabelle im vorherigen Abschnitt beschrieben. Die-Methode `Android.OS.Environment.GetExternalStoragePublicDirectory(string directoryType)` gibt ein `Java.IO.File`-Objekt zurück, das einem öffentlichen Anwendungsverzeichnis entspricht. Der `directoryType`-Parameter ist ein erforderlicher Parameter und kann nicht `null`werden.

Wenn Sie beispielsweise `Environment.GetExternalStoragePublicDirectory(Environment.DirectoryDocuments).AbsolutePath` aufrufen, wird eine Zeichenfolge zurückgegeben, die folgendem ähnelt:

```bash
/storage/emulated/0/Documents
```

> [!IMPORTANT]
> Der exakte Pfad zum öffentlichen externen Speicher Verzeichnis kann von Gerät zu Gerät und zwischen Android-Versionen variieren. Aus diesem Grund dürfen apps den Pfad zu diesem Verzeichnis nicht hart codieren und stattdessen die xamarin. Android-APIs verwenden, wie z. b. `Android.OS.Environment.ExternalStorageDirectory`.

## <a name="working-with-external-storage"></a>Arbeiten mit externem Speicher

Nachdem eine xamarin. Android-App den vollständigen Pfad zu einer Datei abgerufen hat, sollte Sie eine der standardmäßigen .NET-APIs zum Erstellen, lesen, schreiben oder Löschen von Dateien nutzen. Dadurch wird die Menge des plattformübergreifenden kompatiblen Codes für eine APP maximiert. Bevor Sie jedoch versuchen, auf eine Datei zuzugreifen, muss eine xamarin. Android-App sicherstellen, dass es möglich ist, auf diese Datei zuzugreifen.

1. **Überprüfen Sie den externen Speicher** &ndash; abhängig von der Art des externen Speichers ist es möglich, dass er von der APP nicht bereitgestellt und verwendet werden kann. Alle apps sollten den Status des externen Speichers überprüfen, bevor Sie versuchen, ihn zu verwenden.
2. **Ausführen einer Lauf Zeit Berechtigungsüberprüfung** &ndash; eine Android-App muss eine Berechtigung vom Benutzer anfordern, um auf externen Speicher zuzugreifen. Dies bedeutet, dass eine Laufzeit-Berechtigungs Anforderung vor jedem Dateizugriff ausgeführt werden soll. Die Berechtigungen des Handbuchs [in xamarin. Android](~/android/app-fundamentals/permissions.md) enthalten weitere Details zu den Android-Berechtigungen.

Diese beiden Aufgaben werden im folgenden erläutert.

### <a name="verifying-that-external-storage-is-available"></a>Es wird überprüft, ob externer Speicher verfügbar ist.

Der erste Schritt vor dem Schreiben in den externen Speicher besteht darin, zu überprüfen, ob er lesbar oder schreibbar ist. Die `Android.OS.Environment.ExternalStorageState`-Eigenschaft enthält eine Zeichenfolge, die den Zustand des externen Speichers identifiziert. Diese Eigenschaft gibt eine Zeichenfolge zurück, die den Zustand darstellt. Diese Tabelle ist eine Liste der `ExternalStorageState` Werte, die von `Environment.ExternalStorageState`zurückgegeben werden können:

| Externalstoragestate | Beschreibung  |
|----------------------|---|
| "-Adremoval"      | Das Medium wurde plötzlich entfernt, ohne ordnungsgemäß bereitgestellt zu werden. |
| Mediachecking        | Das Medium ist vorhanden, wird jedoch auf einem Datenträger überprüft.  |
| Mediaeinjektion        | Die Bereitstellung des Mediums wird aufgehoben, und der Vorgang wird aufgehoben.  |
| Medilag         | Medien werden eingebunden und können gelesen oder geschrieben werden.  |
| Medider tedreadonly | Medien werden bereitgestellt, Sie können jedoch nur aus gelesen werden. |
| Medianofs            | Es ist ein Medium vorhanden, aber es enthält kein Dateisystem, das für Android geeignet ist. |
| Mediarebewegt         | Es ist kein Medium vorhanden. |
| Mediased          | Medien sind vorhanden, jedoch nicht eingebunden. Sie wird über USB mit einem anderen Gerät freigegeben.|
| Mediaunknown         | Der Status des Mediums wird von Android nicht erkannt. |
| Mediauntierbar     | Das Medium ist vorhanden, kann aber nicht von Android bereitgestellt werden. |
| Mediauneingebunden       | Das Medium ist vorhanden, aber nicht eingebunden. |

Bei den meisten Android-Apps muss nur überprüft werden, ob externer Speicher bereitgestellt wird. Der folgende Code Ausschnitt zeigt, wie Sie überprüfen, ob externer Speicher für schreibgeschützten Zugriff oder Lese-/Schreibzugriff bereitgestellt wird:

```csharp
bool isReadonly = Environment.MediaMountedReadOnly.Equals(Environment.ExternalStorageState);
bool isWriteable = Environment.MediaMounted.Equals(Environment.ExternalStorageState);
```

## <a name="external-storage-permissions"></a>Externe Speicher Berechtigungen

Android betrachtet den Zugriff auf externen Speicher als _gefährliche Berechtigung_, was normalerweise erfordert, dass der Benutzer seine Berechtigung zum Zugriff auf die Ressource gewährt. Der Benutzer kann diese Berechtigung jederzeit widerrufen.  Dies bedeutet, dass eine Laufzeit-Berechtigungs Anforderung vor jedem Dateizugriff ausgeführt werden soll. Apps werden automatisch Berechtigungen zum Lesen und Schreiben Ihrer eigenen privaten Dateien erteilt. Apps können die privaten Dateien, die anderen apps angehören, lesen und schreiben, nachdem Ihnen die Berechtigung vom Benutzer [erteilt](~/android/app-fundamentals/permissions.md) wurde.

Alle Android-Apps müssen eine der beiden Berechtigungen für den externen Speicher in der Datei " **androidmanifest. XML** " deklarieren. Zum Identifizieren der Berechtigungen muss eines der beiden folgenden `uses-permission` Elemente zu " **androidmanifest. XML**" hinzugefügt werden:

```xml
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

> [!NOTE]
> Wenn der Benutzer `WRITE_EXTERNAL_STORAGE`zuweist, wird `READ_EXTERNAL_STORAGE` ebenfalls implizit gewährt. Es ist nicht erforderlich, beide Berechtigungen in " **androidmanifest. XML**" anzufordern.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Die Berechtigungen können auch über die Registerkarte **Android-Manifest** der Projektmappeneigenschaften hinzugefügt werden:

![Projektmappen-Explorer erforderliche Berechtigungen für Visual Studio](./images/required-permissions.w157.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Die Berechtigungen können auch über die Registerkarte **Android-Manifest** im projektmappeneigenschaftenpad hinzugefügt werden:

[![Lösungspad erforderlichen Berechtigungen für Visual Studio für Mac](./images/required-permissions.m752-sml.png)](./images/required-permissions.m752.png#lightbox)

-----

Im Allgemeinen müssen alle gefährlichen Berechtigungen vom Benutzer genehmigt werden. Die Berechtigungen für den externen Speicher sind in Abhängigkeit von der Android-Version, auf der die app ausgeführt wird, mit Ausnahmen zu dieser Regel zu tun:

![Flussdiagramm der externen Speicher Berechtigungs Überprüfungen](./images/external-permission-check-flowchart.png)

Weitere Informationen zum Ausführen von Lauf Zeit Berechtigungsanforderungen finden Sie in der Anleitung unter " [Berechtigungen" in xamarin. Android](~/android/app-fundamentals/permissions.md). Das **monodroid-Sample** [LocalFiles](https://github.com/xamarin/monodroid-samples/tree/master/LocalFiles) zeigt auch eine Möglichkeit, Lauf Zeit Berechtigungs Überprüfungen durchzuführen.

#### <a name="granting-and-revoking-permissions-with-adb"></a>Erteilen und widerrufen von Berechtigungen mit ADB

Im Verlauf der Entwicklung einer Android-App ist es möglicherweise erforderlich, Berechtigungen zum Testen der verschiedenen mit Lauf Zeit Berechtigungs Überprüfungen beteiligten Arbeitsabläufe zu erteilen und aufzuheben. Dies ist in der Eingabeaufforderung mit ADB möglich. Die folgenden Befehlszeilen Ausschnitte veranschaulichen das erteilen oder widerrufen von Berechtigungen mit ADB für eine Android-App, deren Paketname **com. CompanyName. app**lautet:

```bash
$ adb shell pm grant com.companyname.app android.permission.WRITE_EXTERNAL_STORAGE

$ adb shell pm revoke com.companyname.app android.permission.WRITE_EXTERNAL_STORAGE
```

## <a name="deleting-files"></a>Löschen von Dateien

Jede der Standard C# -APIs kann verwendet werden, um eine Datei aus einem externen Speicher zu löschen, z. b. [`System.IO.File.Delete`](xref:System.IO.File.Delete*). Es ist auch möglich, die Java-APIs auf Kosten der Codeportabilität zu verwenden. Beispiel:

```csharp
System.IO.File.Delete("/storage/emulated/0/Android/data/com.companyname.app/files/count.txt");
```

## <a name="related-links"></a>Verwandte Links

* [Beispiel für lokale xamarin. Android-Dateien unter **monodroid-Samples**](https://github.com/xamarin/monodroid-samples/tree/master/LocalFiles)
* [Berechtigungen in xamarin. Android](~/android/app-fundamentals/permissions.md)
