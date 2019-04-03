---
title: Dateizugriff auf externen speichern, die mit Xamarin.Android
description: Diesem Leitfaden wird Zugriff auf Dateien auf externen speichern, die in Xamarin.Android erläutert werden.
ms.prod: xamarin
ms.assetid: 40da10b2-a207-4f9c-a2dd-165d9b662f33
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/23/2018
ms.openlocfilehash: 78051fce44239eea86948988a4d19ac37c5ea0d5
ms.sourcegitcommit: c4be32ef914465e808d89767c4d5ee72afe93cc6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2019
ms.locfileid: "58854898"
---
# <a name="external-storage"></a>Externen Speicher

Externer Speicher bezieht sich auf Dateispeicher, der nicht auf den internen Speicher und nicht ausschließlich für die app, die verantwortlich für die Datei ist zugänglich. Der primäre Zweck von externen Speicher ist, geben Sie einen Ort zum Speichern von Dateien, die von den apps gemeinsam verwendet werden sollen, oder sind zu groß für den internen Speicher.

In der Vergangenheit gesehen, externe Speicher bezeichnet wird, auf einer Datenträgerpartition auf Wechselmedien wie SD-Karten (auch bekannt als wurde _tragbares Speichergerät_). Diese Unterscheidung ist nicht mehr so relevant wie Android-Geräte entwickelt haben, und viele Android-Geräte nicht mehr Speicherplatz auf Wechselmedien unterstützen. Stattdessen werden einige Geräte einige ihrer internen nicht flüchtigen Speicher welche Android zum Ausführen der gleichen Funktion Wechselmedien zugeordnet. Dies bezeichnet man als _emulierten_ Speicher und ist weiterhin externen Speicher sein. Alternativ können Sie möglicherweise einige Android-Geräte über mehrere externen Speicher-Partitionen verfügen. Z. B. Android Tablet (zusätzlich zu seinen internen Speicher) möglicherweise emulierte Speicher haben und eine oder mehrere Slots für SD-Karten. Alle diese Partitionen werden von Android als externen Speicher behandelt.

Auf Geräten, die mehrere Benutzer haben, wird jeder Benutzer über ein dediziertes Verzeichnis für die Partition primären externen Speicher für ihren externen Speicher verfügen. Apps, die als ein Benutzer haben keinen Zugriff auf Dateien von einem anderen Benutzer auf dem Gerät. Die Dateien für alle Benutzer sind weiterhin Welt lesbar und Welt beschreibbare; Allerdings Android wird Sandbox jedes Benutzerprofil von den anderen.

Lesen und Schreiben in Dateien ist fast identisch in Xamarin.Android, wie an eine beliebige andere .NET-Anwendung. Die Xamarin.Android-app bestimmt den Pfad zur Datei, die bearbeitet wird, klicken Sie dann verwendet standard .NET Ausdrücke für den Dateizugriff. Da die eigentlichen Pfade in den internen und externen Speicher nach Gerät variieren und von Android-Version auf Android-Version wird nicht durch hartcodierung der Pfad zu den Dateien empfohlen. Stattdessen stellt Xamarin.Android die native Android-APIs, mit deren Hilfe wird, um zu bestimmen, den Pfad zu Dateien im internen und externen Speicher.

Dieses Handbuch wird erläutert, die Konzepte und -APIs in Android, die in einem externen Speicher spezifisch sind.

## <a name="public-and-private-files-on-external-storage"></a>Dateien mit öffentlichen und privaten auf externen Speichern

Es gibt zwei verschiedene Arten von Dateien, die eine app auf dem externen Speicher beibehalten kann:

* **Private** Dateien &ndash; Private Dateien sind Dateien, die für Ihre Anwendung (sind aber immer noch Welt lesbar und Schreibberechtigung) spezifisch sind. Android erwartet, dass private Dateien in einem bestimmten Verzeichnis auf dem externen Speicher gespeichert werden. Auch wenn die Dateien "private" aufgerufen werden, sind immer noch sichtbar und von anderen apps auf dem Gerät zugegriffen werden kann, sie sind keine speziellen Schutz Android verfügbaren.

* **Öffentliche** Dateien &ndash; Hierbei handelt es sich um Dateien, die nicht spezifisch für die Anwendung angesehen, und ohne Bedenken freigegeben werden sollen.

Die Unterschiede zwischen diesen Dateien dient in erster Linie. Private Dateien sind privat, in dem Sinne, die sie gelten als Teil der Anwendung während der Öffentliche Dateien andere Dateien sind, die im externen Speicher vorhanden. Android bietet zwei verschiedene APIs für die Auflösung die Pfade für private und Öffentliche Dateien, aber andernfalls werden die gleichen .NET APIs verwendet, um Lese- und Schreibberechtigungen für diese Dateien. Dies sind die gleichen APIs, die im Abschnitt erläutert werden, auf [lesen und Schreiben von](~/android/platform/files/index.md#reading-or-writing-to-files-on-internal-storage).

### <a name="private-external-files"></a>Private externen Dateien

Private externe Dateien gelten spezifisch für eine Anwendung (ähnlich wie interne Dateien) werden jedoch im externen Speicher für eine beliebige Anzahl von Gründen (z. B. zu groß für die interne Speicherung) aufbewahrt werden. Ähnlich wie interne Dateien, werden diese Dateien gelöscht, wenn die app vom Benutzer deinstalliert wird.

Der primäre Speicherort für private externe Dateien durch Aufrufen der Methode gefunden wird `Android.Content.Context.GetExternalFilesDir(string type)`. Diese Methode gibt eine `Java.IO.File` -Objekt, das private externen Speicher-Verzeichnis für die app darstellt. Übergeben von `null` an diese Methode gibt den Pfad zum Speicher-Verzeichnis des Benutzers für die Anwendung zurück. Als Beispiel für eine Anwendung durch den Namen des Pakets `com.companyname.app`, das Verzeichnis "Root" der privaten externe Dateien wäre:

```bash
/storage/emulated/0/Android/data/com.companyname.app/files/
```

In diesem Dokument wird das Storage-Verzeichnis für Dateien mit privaten auf externen speichern wie finden Sie unter _PRIVATE\_externe\_STORAGE_.


Der Parameter für `GetExternalFilesDir()` ist eine Zeichenfolge, der angibt, ein _Anwendungsverzeichnis_. Dies ist ein Verzeichnis, das einen standardmäßigen Speicherort für die logische Anordnung von Dateien bereitstellen. Die Zeichenfolgenwerte sind verfügbar, über die Konstanten für die `Android.OS.Environment` Klasse:

| `Android.OS.Environment` | Verzeichnis |
|-|-|
| DirectoryAlarms | **_PRIVATE\_externe\_STORAGE_  /Alarme** |
| DirectoryDcim | **_PRIVATE\_EXTERNE\_STORAGE_/DCIM** |
| DirectoryDownloads | **_PRIVATE\_externe\_STORAGE_  /herunterladen** |
| DirectoryDocuments | **_PRIVATE\_externe\_STORAGE_  /Dokumente** |
| DirectoryMovies | **_PRIVATE\_externe\_STORAGE_/Movies** |
| DirectoryMusic | **_PRIVATE\_externe\_STORAGE_/Music** |
| DirectoryNotifications | **_PRIVATE\_externe\_STORAGE_  /Notifications** |
| DirectoryPodcasts | **_PRIVATE\_externe\_STORAGE_/Podcasts** |
| DirectoryRingtones | **_PRIVATE\_externe\_STORAGE_/Ringtones** |
| DirectoryPictures | **_PRIVATE\_externe\_STORAGE_  /Pictures** |

Für Geräte, die über mehrere Partitionen von externen Speicher verfügen, wird jeder Partition ein Verzeichnis sein, die für die private Dateien vorgesehen ist. Die Methode `Android.Content.Context.GetExternalFilesDirs(string type)` gibt ein Array von `Java.IO.Files`. Jedes Objekt wird ein privates anwendungsspezifischen Verzeichnis darstellen auf alle freigegebenen/externen Speichergeräten, in dem die Anwendung die Dateien platzieren kann, die es besitzt.

> [!IMPORTANT]
> Der genaue Pfad für die private Exteral Speicherverzeichnis kann vom Gerät zu Gerät und zwischen Versionen von Android variieren. Aus diesem Grund apps müssen nicht hart codieren Sie den Pfad zu diesem Verzeichnis, und verwenden Sie stattdessen die Xamarin.Android-APIs, wie z. B. `Android.Content.Context.GetExternalFilesDir()`.

### <a name="public-external-files"></a>Öffentliche externe Dateien

Öffentliche Dateien sind Dateien, die auf externen speichern, vorhanden sind, die nicht im Verzeichnis gespeichert werden, die Android für Dateien mit privaten zuordnet. Öffentliche Dateien werden nicht gelöscht werden, wenn die app deinstalliert wird. Android-apps müssen Berechtigung erteilt werden, bevor sie Lese- oder Schreibzugriff auf alle öffentlichen Dateien. Es ist möglich, dass Öffentliche Dateien an einer beliebigen Stelle in externen Speicher vorhanden ist, aber gemäß der Konvention erwartet Android Öffentliche Dateien in das Verzeichnis identifiziert, die durch die Eigenschaft vorhanden sein `Android.OS.Environment.ExternalStorageDirectory`. Diese Eigenschaft gibt eine `Java.IO.File` -Objekt, das Verzeichnis der primären externen Speicher darstellt. Als Beispiel `Android.OS.Environment.ExternalStorageDirectory` bezieht sich möglicherweise auf das folgende Verzeichnis:

```bash
/storage/emulated/0/
```

In diesem Dokument wird das Storage-Verzeichnis für Öffentliche Dateien, die auf externen speichern wie finden Sie unter _öffentliche\_externe\_STORAGE_.


Android unterstützt auch das Konzept von Verzeichnissen auf _öffentliche\_externe\_STORAGE_. Diese Verzeichnisse sind identisch mit den Verzeichnissen für `_PRIVATE\_EXTERNAL\_STORAGE_` und werden in der Tabelle im vorherigen Abschnitt beschrieben. Die Methode `Android.OS.Environment.GetExternalStoragePublicDirectory(string directoryType)` gibt eine `Java.IO.File` -Objekt, das in ein Verzeichnis für die public-Anwendung zu entsprechen. Die `directoryType` Parameter ist ein obligatorischer Parameter und kann nicht `null`.

Zum Beispiel der Aufruf `Environment.GetExternalStoragePublicDirectory(Environment.DirectoryDocuments).AbsolutePath` gibt eine Zeichenfolge, die ähneln wird:

```bash
/storage/emulated/0/Documents
```

> [!IMPORTANT]
> Der genaue Pfad zum Verzeichnis öffentlichen externen Speicher kann vom Gerät zu Gerät und zwischen den Versionen von Android variieren. Aus diesem Grund apps müssen nicht hart codieren Sie den Pfad zu diesem Verzeichnis, und verwenden Sie stattdessen die Xamarin.Android-APIs, wie z. B. `Android.OS.Environment.ExternalStorageDirectory`.

## <a name="working-with-external-storage"></a>Arbeiten mit externem Speicher

Sobald eine Xamarin.Android-app den vollständigen Pfad zu einer Datei abgerufen wurde, sollte von den standardmäßigen .NET APIs für das Erstellen, lesen, schreiben oder Löschen von Dateien nutzen. Dies maximiert die Menge der cross-Platform kompatibel Code für eine app. Jedoch vor dem Versuch, den Zugriff auf eine Datei muss eine Xamarin.Android-app sicherstellen, dass es möglich, den Zugriff auf diese Datei ist.

1. **Überprüfen Sie die externen Speicher** &ndash; je nach Art der externe Speicher, es ist möglich, dass es nicht bereitgestellt werden kann und von der app verwendet werden kann. Alle apps sollten den Status des dem externen Speicher überprüfen, bevor Sie versuchen ihn anzuwenden.
2. **Führen Sie eine Berechtigung laufzeitüberprüfung** &ndash; ein Android-app muss Berechtigung vom Benutzer anfordern, um externen Speicher zuzugreifen. Dies bedeutet, dass eine Laufzeit, die berechtigungsanforderung vor dem Zugriff auf Dateien ausgeführt werden soll. Das Handbuch [Berechtigungen In Xamarin.Android](~/android/app-fundamentals/permissions.md) enthält weitere Informationen zu Android-Berechtigungen.

Diese beiden Aufgaben werden unten erläutert.

### <a name="verifying-that-external-storage-is-available"></a>Überprüfen, ob externer Speicher verfügbar ist.

Der erste Schritt vor dem Schreiben in einen externen Speicher ist, überprüfen Sie, dass es sich um gelesen oder geschrieben wird. Die `Android.OS.Environment.ExternalStorageState` Eigenschaft enthält eine Zeichenfolge, die den Zustand der dem externen Speicher bezeichnet. Diese Eigenschaft gibt eine Zeichenfolge zurück, die den Zustand darstellt. Diese Tabelle ist eine Liste mit den `ExternalStorageState` Werte, die von zurückgegeben werden können `Environment.ExternalStorageState`:

| ExternalStorageState | Beschreibung  |
|----------------------|---|
| MediaBadRemoval      | Das Medium wurde plötzlich entfernt, ohne ordnungsgemäß aufgehoben wird. |
| MediaChecking        | Das Medium vorhanden ist, aber gerade von einem Datenträger überprüfen.  |
| MediaEjecting        | Medium wird zurzeit aufgehoben und ausgeworfen wird.  |
| MediaMounted         | Medien kann wird bereitgestellt und gelesen oder geschrieben werden.  |
| MediaMountedReadOnly | Medium wird bereitgestellt, jedoch kann nur aus gelesen werden. |
| MediaNofs            | Medien vorhanden ist, aber enthält kein Dateisystem für Android geeignet ist. |
| MediaRemoved         | Kein Medium vorhanden ist. |
| MediaShared          | Medien ist vorhanden, aber es ist nicht bereitgestellt. Es wird über USB mit einem anderen Gerät freigegeben wird.|
| MediaUnknown         | Der Status der Medien wird von Android nicht erkannt. |
| MediaUnmountable     | Das Medium vorhanden ist, aber es kann nicht von Android bereitgestellt werden. |
| MediaUnmounted       | Das Medium vorhanden ist, aber es ist nicht bereitgestellt. |


Die meisten Android-apps müssen nur zum Überprüfen, ob die externer Speicher bereitgestellt wird. Der folgende Codeausschnitt zeigt, wie stellen Sie sicher, dass externe Speicher für Lesezugriff oder Lese-/ Schreibzugriff eingebunden wird:

```csharp
bool isReadonly = Environment.MediaMountedReadOnly.Equals(Environment.ExternalStorageState);
bool isWriteable = Environment.MediaMounted.Equals(Environment.ExternalStorageState);
```

## <a name="external-storage-permissions"></a>Externen Speicher-Berechtigungen

Android berücksichtigt beim Zugriff auf externe Speicher werden eine _gefährliche Berechtigungen_, die in der Regel muss der Benutzer die Zustimmung zum Zugriff auf die Ressource zu gewähren. Der Benutzer kann diese Berechtigung jederzeit widerrufen.  Dies bedeutet, dass eine Laufzeit, die berechtigungsanforderung vor dem Zugriff auf Dateien ausgeführt werden soll. Apps werden automatisch gewährt die Berechtigungen zum Lesen und Schreiben ihren eigenen privaten Dateien. Es ist möglich, dass apps zum Lesen und Schreiben von privaten Dateien, die an andere apps nach Ihrer gehören [Berechtigung](~/android/app-fundamentals/permissions.md) durch den Benutzer.

Alle Android-apps müssen deklarieren Sie eine der beiden Berechtigungen für den externen Speicher in den **"androidmanifest.xml"** . Identifizieren Sie die Berechtigungen, eine der beiden folgenden `uses-permission` Elemente hinzugefügt werden müssen **"androidmanifest.xml"**:

```xml
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

> [!NOTE]
> Wenn der Benutzer erteilt `WRITE_EXTERNAL_STORAGE`, klicken Sie dann `READ_EXTERNAL_STORAGE` ist auch implizit erteilt. Es ist nicht erforderlich, um beide Berechtigungen in **"androidmanifest.xml"**.

# [<a name="visual-studio"></a>Visual Studio](#tab/windows)

Die Berechtigungen können auch hinzugefügt werden, mithilfe der **Android-Manifest** Registerkarte die **Projektmappeneigenschaften**:

![Projektmappen-Explorer - erforderlichen Berechtigungen für Visual Studio](./images/required-permissions.w157.png)

# [<a name="visual-studio-for-mac"></a>Visual Studio für Mac](#tab/macos)

Die Berechtigungen können auch hinzugefügt werden, mithilfe der **Android-Manifest** auf der Registerkarte die **Lösung Pad "Eigenschaften"**:

[![SErforderliche Berechtigungen für Visual Studio für Mac rojektmappe Pad](./images/required-permissions.m752-sml.png)](./images/required-permissions.m752.png#lightbox)

-----

Im Allgemeinen müssen alle problematischen Berechtigungen vom Benutzer genehmigt werden. Die Berechtigungen für den externen Speicher sind eine Anomalie, es gibt jedoch Ausnahmen von dieser Regel, abhängig von der Version von Android, die die app ausgeführt wird:

![Flussdiagramm der externen Speicher berechtigungsüberprüfungen](./images/external-permission-check-flowchart.png)

Weitere Informationen zu berechtigungsanforderungen für die Common Language Runtime ausführen, finden Sie in der Anleitung [Berechtigungen In Xamarin.Android](~/android/app-fundamentals/permissions.md). Die **Monodroid-Sample** [LocalFiles](https://github.com/xamarin/monodroid-samples/tree/master/LocalFiles) auch zeigt eine Möglichkeit der Common Language Runtime-berechtigungsüberprüfungen durchführen.

#### <a name="granting-and-revoking-permissions-with-adb"></a>Erteilen und Entziehen von Berechtigungen mit ADB

Beim Entwickeln einer Android-Apps, ist es möglicherweise notwendig, erteilen und Widerrufen von Berechtigungen für die verschiedenen berechtigungsüberprüfungen für die Common Language Runtime bei Workflows zu testen. Es ist möglich, klicken Sie dazu an der Eingabeaufforderung, die ADB verwenden. Die folgenden Befehlszeile Codeausschnitte veranschaulichen, wie zum gewähren oder widerrufen Berechtigungen mithilfe von ADB für eine Android-app, deren Paketname ist **com.companyname.app**:

```bash
$ adb shell pm grant com.companyname.app android.permission.WRITE_EXTERNAL_STORAGE

$ adb shell pm revoke com.companyname.app android.permission.WRITE_EXTERNAL_STORAGE
```

## <a name="deleting-files"></a>Löschen von Dateien

Standardmäßige c#-APIs werden, zum Löschen einer Datei aus dem externen Speicher, z. B. verwendet kann [ `System.IO.File.Delete` ](xref:System.IO.File.Delete*). Es ist auch möglich, die Java-APIs auf Kosten der Codeportabilität verwenden. Zum Beispiel:

```csharp
System.IO.File.Delete("/storage/emulated/0/Android/data/com.companyname.app/files/count.txt");
```

## <a name="related-links"></a>Verwandte Links

* [Lokale Dateien von Xamarin.Android-Beispiel auf **Monodroid-Beispiele**](https://github.com/xamarin/monodroid-samples/tree/master/LocalFiles)
* [Permissions In Xamarin.Android](~/android/app-fundamentals/permissions.md)
