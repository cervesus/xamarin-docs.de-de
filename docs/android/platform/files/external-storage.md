---
title: Dateizugriff auf externem Speicher mit Xamarin.Android
description: In diesem Leitfaden wird der Dateizugriff auf externem Speicher in Xamarin.Android erörtert.
ms.prod: xamarin
ms.assetid: 40da10b2-a207-4f9c-a2dd-165d9b662f33
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 07/23/2018
ms.openlocfilehash: 96b0d6a00c7825939b1f89ed63e3e5559ca4ef59
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "73020479"
---
# <a name="external-storage"></a>Externer Speicher

Externer Speicher bezieht sich auf Dateispeicher, der sich nicht im internen Speicher befindet und nicht ausschließlich für die App zugänglich ist, die für die Datei zuständig ist. Der Hauptzweck von externem Speicher besteht darin, einen Aufbewahrungsort für Dateien bereitzustellen, die gemeinsam von verschiedenen Apps verwendet werden sollen oder zu groß für den internen Speicher sind.

In der Vergangenheit bezog sich „externer Speicher“ auf eine Datenträgerpartition auf Wechselmedien, wie etwa eine SD-Karte (auch als _tragbarer Speicher_ bezeichnet). Diese Unterscheidung ist nicht mehr in gleichem Maß relevant, da Android-Geräte sich weiterentwickelt haben und viele Android-Geräte keine Wechselmedien mehr unterstützen. Stattdessen weisen einige Geräte einen Teil ihres nicht flüchtigen internen Speichers zu, mit dem Android die gleichen Funkionen wie auf Wechselmedien ausführt. Dies wird als _emulierter_ Speicher bezeichnet und trotzdem als externer Speicher betrachtet. Alternativ können einige Android-Geräte mehrere externe Speicherpartitionen aufweisen. Beispielsweise kann ein Android-Tablet (zusätzlich zum internen Speicher) über emulierten Speicher und einen oder mehrere Slots für eine SD-Karte verfügen. Alle diese Partitionen werden von Android als externer Speicher behandelt.

Auf Geräten mit mehreren Benutzern erhält jeder Benutzer ein dediziertes Verzeichnis auf der primären externen Speicherpartition für den eigenen externen Speicher. Apps, die mit einem Benutzerkonto ausgeführt werden, haben keinen Zugriff auf Dateien eines anderen Benutzers auf dem Gerät. Dateien für alle Benutzer sind trotzdem global lesbar und schreibbar, jedoch schirmt Android jedes Benutzerprofil mithilfe einer Sandbox von den anderen ab.

Das Lesen aus und Schreiben in Dateien erfolgt in Xamarin.Android nahezu identisch wie in jeder anderen .NET-Anwendung. Die Xamarin.Android-App bestimmt den Pfad zu der zu ändernden Datei und verwendet anschließend .NET-Standardausdrücke für den Dateizugriff. Da sich die tatsächlichen Pfade zu externem und internem Speicher von Gerät zu Gerät oder von Android-Version zu Android-Version unterscheiden können, empfiehlt es sich nicht, die Dateipfade hart zu codieren. Stattdessen macht Xamarin.Android die nativen Android-APIs verfügbar, die die Bestimmung des Pfads zu Dateien in internem und externem Speicher unterstützen.

In diesem Leitfaden werden die Konzepte und APIs in Android erläutert, die für externen Speicher spezifisch sind.

## <a name="public-and-private-files-on-external-storage"></a>Öffentliche und private Dateien in externem Speicher

Es gibt zwei verschiedene Typen von Dateien, die eine App im externen Speicher aufbewahren kann:

* **Private** Dateien &ndash; Private Dateien sind Dateien, die für Ihre Anwendung spezifisch sind (aber trotzdem global lesbar und global schreibbar sind). Android erwartet, dass private Dateien im externen Speicher in einem bestimmten Verzeichnis gespeichert werden. Obwohl die Dateien als „privat“ bezeichnet werden, sind sie trotzdem sichtbar, und andere Apps auf dem Gerät können auf sie zugreifen, sie unterliegen keinem besonderen Schutz durch Android.

* **Öffentliche** Dateien &ndash; Dies sind Dateien, die nicht als spezifisch für die Anwendung angesehen werden und für uneingeschränktes Teilen vorgesehen sind.

Die Unterschiede zwischen diesen Dateien sind in erster Linie konzeptionell. Private Dateien sind insofern privat, als sie als Teil der Anwendung betrachtet werden, während öffentliche Dateien beliebige andere Dateien sind, die in externem Speicher vorhanden sind. Android bietet zwei verschiedene APIs zum Auflösen der Pfade von privaten und öffentlichen Dateien, davon abgesehen werden aber die gleichen .NET-APIs zum Lesen und Schreiben der Dateien verwendet. Dies sind die gleichen APIs, die im Abschnitt zum [Lesen und Schreiben](~/android/platform/files/index.md#reading-or-writing-to-files-on-internal-storage) erörtert werden.

### <a name="private-external-files"></a>Private externe Dateien

Private externe Dateien werden als anwendungsspezifisch angesehen (ähnlich wie interne Dateien), werden aber aus beliebigen Gründen in externem Speicher aufbewahrt (etwa, weil sie für den internen Speicher zu groß sind). Ähnlich wie interne Dateien werden diese Dateien gelöscht, wenn die App vom Benutzer deinstalliert wird.

Der primäre Speicherort für private externe Dateien lässt sich durch Aufrufen der Methode `Android.Content.Context.GetExternalFilesDir(string type)` ermitteln. Diese Methode gibt ein `Java.IO.File`-Objekt zurück, das das private externe Speicherverzeichnis für die App darstellt. Durch Übergeben von `null` an diese Methode wird der Pfad zum Speicherverzeichnis des Benutzers für die Anwendung zurückgegeben. Beispielsweise wäre für eine Anwendung mit dem Paketnamen `com.companyname.app` das „Stamm“-Verzeichnis für private externe Dateien:

```bash
/storage/emulated/0/Android/data/com.companyname.app/files/
```

Im Rahmen dieses Dokuments wird das Speicherverzeichnis für private Dateien in externem Speicher als _PRIVATER\_EXTERNER\_SPEICHER_ bezeichnet.

Der Parameter für `GetExternalFilesDir()` ist eine Zeichenfolge, die ein _Anwendungsverzeichnis_ angibt. Dies ist ein Verzeichnis, das dazu bestimmt ist, einen Standardspeicherort für eine logische Dateistruktur bereitzustellen. Die Zeichenfolgenwerte sind in Form von Konstanten in der `Android.OS.Environment`-Klasse verfügbar:

| `Android.OS.Environment` | Verzeichnis |
|-|-|
| DirectoryAlarms | **_PRIVATER\_EXTERNER\_SPEICHER_/Alarms** |
| DirectoryDcim | **_PRIVATER\_EXTERNER\_SPEICHER_/DCIM** |
| DirectoryDownloads | **_PRIVATER\_EXTERNER\_SPEICHER_/Download** |
| DirectoryDocuments | **_PRIVATER\_EXTERNER\_SPEICHER_/Documents** |
| DirectoryMovies | **_PRIVATER\_EXTERNER\_SPEICHER_/Movies** |
| DirectoryMusic | **_PRIVATER\_EXTERNER\_SPEICHER_/Music** |
| DirectoryNotifications | **_PRIVATER\_EXTERNER\_SPEICHER_/Notifications** |
| DirectoryPodcasts | **_PRIVATER\_EXTERNER\_SPEICHER_/Podcasts** |
| DirectoryRingtones | **_PRIVATER\_EXTERNER\_SPEICHER_/Ringtones** |
| DirectoryPictures | **_PRIVATER\_EXTERNER\_SPEICHER_/Pictures** |

Bei Geräten mit mehreren externen Speicherpartitionen weist jede Partition ein Verzeichnis auf, das für private Dateien vorgesehen ist. Die Methode `Android.Content.Context.GetExternalFilesDirs(string type)` gibt ein Array von `Java.IO.Files` zurück. Jedes-Objekt stellt ein privates anwendungsspezifisches Verzeichnis auf allen freigegebenen/externen Speichergeräten dar, auf denen die Anwendung die Dateien ablegen kann, die sie besitzt.

> [!IMPORTANT]
> Der genaue Pfad zum privaten externen Speicherverzeichnis kann von Gerät zu Gerät und zwischen Android-Versionen abweichen. Aus diesem Grund dürfen Apps den Pfad zu diesem Verzeichnis nicht hart codieren und müssen stattdessen die Xamarin.Android-APIs verwenden, wie etwa `Android.Content.Context.GetExternalFilesDir()`.

### <a name="public-external-files"></a>Öffentliche externe Dateien

Öffentliche Dateien sind Dateien, die in externem Speicher vorhanden sind und nicht in dem Verzeichnis gespeichert sind, das von Android für private Dateien zugewiesen wird. Öffentliche Dateien werden nicht gelöscht, wenn die App deinstalliert wird. Android-Apps muss zum Lesen oder Schreiben jeglicher öffentlicher Dateien eine Berechtigung erteilt werden. Öffentliche Dateien können an beliebiger Stelle im externen Speicher vorhanden sein, aber gemäß Konvention erwartet Android, dass öffentliche Dateien in dem durch die-Eigenschaft `Android.OS.Environment.ExternalStorageDirectory`bezeichneten Verzeichnis vorliegen. Diese Eigenschaft gibt ein `Java.IO.File`-Objekt zurück, das das primäre externe Speicherverzeichnis darstellt. Beispielsweise kann `Android.OS.Environment.ExternalStorageDirectory` auf das folgende Verzeichnis verweisen:

```bash
/storage/emulated/0/
```

Im Rahmen dieses Dokuments wird das Speicherverzeichnis für öffentliche Dateien in externem Speicher als _ÖFFENTLICHER\_EXTERNER\_SPEICHER_ bezeichnet.

Android unterstützt ebenfalls das Konzept von Anwendungsverzeichnissen im _ÖFFENTLICHEN\_EXTERNEN\_SPEICHER_. Diese Verzeichnisse stimmen exakt mit den Anwendungsverzeichnissen für `PRIVATE_EXTERNAL_STORAGE` überein und werden in der Tabelle im vorherigen Abschnitt beschrieben. Die-Methode `Android.OS.Environment.GetExternalStoragePublicDirectory(string directoryType)` gibt ein `Java.IO.File`-Objekt zurück, das einem öffentlichen Anwendungsverzeichnis entspricht. Der Parameter `directoryType` ist erforderlich und kann nicht `null` sein.

Wenn Sie beispielsweise `Environment.GetExternalStoragePublicDirectory(Environment.DirectoryDocuments).AbsolutePath` aufrufen, wird eine Zeichenfolge ähnlich der folgenden zurückgegeben:

```bash
/storage/emulated/0/Documents
```

> [!IMPORTANT]
> Der genaue Pfad zum öffentlichen externen Speicherverzeichnis kann von Gerät zu Gerät und zwischen Android-Versionen abweichen. Aus diesem Grund dürfen Apps den Pfad zu diesem Verzeichnis nicht hart codieren und müssen stattdessen die Xamarin.Android-APIs verwenden, wie etwa `Android.OS.Environment.ExternalStorageDirectory`.

## <a name="working-with-external-storage"></a>Arbeiten mit externem Speicher

Nachdem eine Xamarin.Android-App den vollständigen Pfad einer Datei abgerufen hat, sollte sie eine der standardmäßigen .NET-APIs zum Erstellen, Lesen, Schreiben oder Löschen von Dateien nutzen. Dadurch wird die Menge des plattformübergreifend kompatiblen Codes für eine App maximiert. Vor dem Zugriffsversuch auf eine Datei muss eine Xamarin.Android-App jedoch sicherstellen, dass der Zugriff auf die betreffende Datei möglich ist.

1. **Überprüfen von externem Speicher** &ndash; Je nach Art des externen Speichers ist es möglich, dass der Speicher nicht eingebunden und nicht von der App verwendbar ist. Alle Apps sollten den Status des externen Speichers überprüfen, bevor sie versuchen, ihn zu verwenden.
2. **Durchführen einer Berechtigungsprüfung zur Laufzeit** &ndash; Eine Android-App muss die Erlaubnis des Benutzers anfordern, um auf externen Speicher zuzugreifen. Das bedeutet, dass vor jeglichem Dateizugriff eine Berechtigungsanforderung zur Laufzeit erfolgen sollte. Der Leitfaden [Berechtigungen in Xamarin.Android](~/android/app-fundamentals/permissions.md) bietet weitere Details zu Android-Berechtigungen.

Beide Aufgaben werden unten erörtert.

### <a name="verifying-that-external-storage-is-available"></a>Überprüfung, ob der externe Speicher verfügbar ist

Der erste Schritt vor dem Schreiben in externen Speicher besteht darin, zu prüfen, ob er lesbar oder beschreibbar ist. Die `Android.OS.Environment.ExternalStorageState`-Eigenschaft enthält eine Zeichenfolge, die den Status des externen Speichers kennzeichnet. Diese Eigenschaft gibt eine Zeichenfolge zurück, die den Status darstellt. Diese Tabelle stellt eine Liste der Werte von `ExternalStorageState` dar, die von `Environment.ExternalStorageState` zurückgegeben werden können:

| ExternalStorageState | Beschreibung  |
|----------------------|---|
| MediaBadRemoval      | Das Medium wurde abrupt entfernt, ohne ordnungsgemäße Aufhebung seiner Einbindung. |
| MediaChecking        | Das Medium ist vorhanden, wird aber einer Datenträgerprüfung unterzogen.  |
| MediaEjecting        | Das Medium befindet sich in der Aufhebung der Einbindung und wird dann ausgeworfen.  |
| MediaMounted         | Das Medium ist eingebunden und kann gelesen oder beschrieben werden.  |
| MediaMountedReadOnly | Das Medium ist eingebunden, kann aber nur gelesen werden. |
| MediaNofs            | Das Medium ist vorhanden, enthält aber kein für Android geeignetes Dateisystem. |
| MediaRemoved         | Es ist kein Medium vorhanden. |
| MediaShared          | Das Medium ist vorhanden, aber nicht eingebunden. Es wird per USB mit einem anderen Gerät geteilt.|
| MediaUnknown         | Der Status des Mediums wird von Android nicht erkannt. |
| MediaUnmountable     | Das Medium ist vorhanden, kann aber von Android nicht eingebunden werden. |
| MediaUnmounted       | Das Medium ist vorhanden, aber nicht eingebunden. |

Die meisten Android-Apps brauchen lediglich zu überprüfen, ob externer Speicher eingebunden ist. Der folgende Codeausschnitt zeigt, wie überprüft werden kann, ob der externe Speicher nur für Lesezugriff oder für Schreib-Lesezugriff eingebunden ist:

```csharp
bool isReadonly = Environment.MediaMountedReadOnly.Equals(Environment.ExternalStorageState);
bool isWriteable = Environment.MediaMounted.Equals(Environment.ExternalStorageState);
```

## <a name="external-storage-permissions"></a>Berechtigungen für externen Speicher

Android betrachtet den Zugriff auf externen Speicher als _gefährliche Berechtigung_, was es normalerweise erforderlich macht, dass der Benutzer die Berechtigung zum Zugriff auf die Ressource erteilt. Der Benutzer kann diese Berechtigung jederzeit widerrufen.  Das bedeutet, dass vor jeglichem Dateizugriff eine Berechtigungsanforderung zur Laufzeit erfolgen sollte. Apps werden automatisch Berechtigungen zum Lesen und Schreiben ihrer eigenen privaten Dateien erteilt. Apps können die privaten Dateien anderer Apps lesen und schreiben, nachdem ihnen vom Benutzer die [Berechtigung erteilt](~/android/app-fundamentals/permissions.md) wurde.

Alle Android-Apps müssen eine der zwei Berechtigungen für externen Speicher im **AndroidManifest.xml** deklarieren. Zum Bestimmen der Berechtigungen muss eines der folgenden zwei `uses-permission`-Elemente zu **AndroidManifest.xml** hinzugefügt werden:

```xml
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

> [!NOTE]
> Wenn der Benutzer `WRITE_EXTERNAL_STORAGE` erteilt, ist `READ_EXTERNAL_STORAGE` implizit ebenfalls erteilt. Es ist nicht erforderlich, in **AndroidManifest.xml** beide Berechtigungen anzufordern.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Die Berechtigungen können ebenfalls mithilfe der Registerkarte **Android Manifest** der **Projektmappeneigenschaften** hinzugefügt werden:

![Projektmappen-Explorer: Erforderliche Berechtigungen für Visual Studio](./images/required-permissions.w157.png)

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

Die Berechtigungen können ebenfalls mithilfe der Registerkarte **Android Manifest** des **Projektmappeneigenschaften-Pads** hinzugefügt werden:

[![Lösungspad: Erforderliche Berechtigungen für Visual Studio für Mac](./images/required-permissions.m752-sml.png)](./images/required-permissions.m752.png#lightbox)

-----

Generell gilt, dass alle gefährlichen Berechtigungen vom Benutzer genehmigt werden müssen. Die Berechtigungen für externen Speicher stellen insofern eine Anomalie dar, als dass es Ausnahmen von dieser Regel gibt, je nach der Android-Version, unter der die App ausgeführt wird:

![Flussdiagramm der Berechtigungsprüfungen für externen Speicher](./images/external-permission-check-flowchart.png)

Weitere Informationen zum Ausführen von Berechtigungsanforderungen zur Laufzeit finden Sie im Leitfaden [Berechtigungen in Xamarin.Android](~/android/app-fundamentals/permissions.md). Das **monodroid-Beispiel** [LocalFiles](https://github.com/xamarin/monodroid-samples/tree/master/LocalFiles) veranschaulicht eine weitere Methode zum Durchführen von Berechtigungsprüfungen zur Laufzeit.

#### <a name="granting-and-revoking-permissions-with-adb"></a>Erteilen und Widerrufen von Berechtigungen mithilfe von ADB

Im Lauf der Entwicklung einer Android-App kann es erforderlich sein, Berechtigungen zu erteilen und zu widerrufen, um die verschiedenen Workflows zu testen, die mit Berechtigungsprüfungen zur Laufzeit einhergehen. Dies kann an der Eingabeaufforderung mithilfe von ADB erfolgen. Die folgenden Befehlszeilen-Codeausschnitte veranschaulichen das Erteilen oder Widerrufen von Berechtigungen mithilfe von ADB für eine Android-App, deren Paketname **com.companyname.app** lautet:

```bash
$ adb shell pm grant com.companyname.app android.permission.WRITE_EXTERNAL_STORAGE

$ adb shell pm revoke com.companyname.app android.permission.WRITE_EXTERNAL_STORAGE
```

## <a name="deleting-files"></a>Löschen von Dateien

Zum Löschen einer Datei aus dem externen Speicher kann jede der standardmäßigen C#-APIs verwendet werden, beispielsweise [`System.IO.File.Delete`](xref:System.IO.File.Delete*). Es ist ferner möglich, die Java-APIs zu verwenden, um den Preis der Portierbarkeit des Codes. Zum Beispiel:

```csharp
System.IO.File.Delete("/storage/emulated/0/Android/data/com.companyname.app/files/count.txt");
```

## <a name="related-links"></a>Verwandte Links

* [Xamarin.Android Local Files-Beispiel auf **monodroid-samples**](https://github.com/xamarin/monodroid-samples/tree/master/LocalFiles)
* [Berechtigungen in Xamarin.Android](~/android/app-fundamentals/permissions.md)
