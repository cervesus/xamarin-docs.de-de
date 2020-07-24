---
title: Speichern und Zugreifen auf Daten in Azure Storage vonXamarin.Forms
description: Azure Storage ist eine skalierbare cloudspeicherlösung, die zum Speichern von unstrukturierten und strukturierten Daten verwendet werden kann. In diesem Artikel wird erläutert, wie Xamarin.Forms Sie zum Speichern von Text-und Binärdaten in Azure Storage und zum Zugreifen auf die Daten verwenden.
ms.prod: xamarin
ms.assetid: 5B10D37B-839B-4CD0-9C65-91014A93F3EB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/28/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: f67543a6c678e2c3a1395f816e020d69af4bf873
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86936655"
---
# <a name="store-and-access-data-in-azure-storage-from-xamarinforms"></a>Speichern und Zugreifen auf Daten in Azure Storage vonXamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-azurestorage)

_Azure Storage ist eine skalierbare cloudspeicherlösung, die zum Speichern von unstrukturierten und strukturierten Daten verwendet werden kann. In diesem Artikel wird veranschaulicht, wie Xamarin.Forms Sie zum Speichern von Text-und Binärdaten in Azure Storage und zum Zugreifen auf die Daten verwenden._

Azure Storage bietet vier Speicherdienste:

- Blob Storage. Ein BLOB kann Text-oder Binärdaten sein, z. b. Sicherungen, virtuelle Computer, Mediendateien oder Dokumente.
- Table Storage ist ein nosql-Schlüssel Attribut Speicher.
- Queue Storage ist ein Messaging Dienst für die Workflow Verarbeitung und die Kommunikation zwischen Cloud-Diensten.
- File Storage stellt gemeinsam genutzten Speicher über das SMB-Protokoll bereit.

Zwei Typen von Speicherkonten stehen zur Verfügung:

- Ein allgemeines Speicherkonto ermöglicht den Zugriff auf Azure Storage Dienste von einem einzelnen Konto aus.
- Ein BLOB Storage-Konto ist ein spezielles Speicherkonto zum Speichern von BLOBs. Dieser Kontotyp wird empfohlen, wenn Sie nur BLOB-Daten speichern müssen.

Dieser Artikel und die begleitende Beispielanwendung veranschaulichen das Hochladen von Bild-und Textdateien in den BLOB-Speicher und das herunterladen. Außerdem wird das Abrufen einer Liste von Dateien aus dem BLOB-Speicher und das Löschen von Dateien veranschaulicht.

Weitere Informationen zu Azure Storage finden Sie unter [Einführung in Storage](https://azure.microsoft.com/documentation/articles/storage-introduction/).

> [!NOTE]
> Wenn Sie kein [Azure-Abonnement](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing) besitzen, erstellen Sie ein [kostenloses Konto](https://aka.ms/azfree-docs-mobileapps), bevor Sie beginnen.

## <a name="introduction-to-blob-storage"></a>Einführung in BLOB Storage

BLOB Storage besteht aus drei Komponenten, die im folgenden Diagramm dargestellt sind:

![BLOB Storage Konzepte](azure-storage-images/blob-storage.png)

Der gesamte Zugriff auf Azure Storage erfolgt über ein Speicherkonto. Ein Speicherkonto kann eine unbegrenzte Anzahl von Containern enthalten, und ein Container kann eine unbegrenzte Anzahl von BLOB-Speicher speichern, bis die Kapazitätsgrenze des Speicher Kontos erreicht ist.

Ein BLOB ist eine Datei von beliebiger Art und Größe. Azure Storage unterstützt drei verschiedene BLOB-Typen:

- Blockblobs sind für das Streamen und Speichern von cloudressourcen optimiert und sind eine gute Wahl für das Speichern von Sicherungen, Mediendateien, Dokumenten usw. Block-BLOBs können bis zu 1 bis 7 GB groß sein.
- Anfügeblobs ähneln blockblobs, sind aber für Anfüge Vorgänge optimiert, wie z. b. die Protokollierung. Anfügeblobzeichen können bis zu 1 bis 7 GB groß sein.
- Seitenblobs sind für Häufige Lese-/Schreibvorgänge optimiert und werden in der Regel zum Speichern virtueller Maschinen und ihrer Datenträger verwendet. Seitenblobs können eine Größe von bis zu 1 TB aufweisen.

> [!NOTE]
> Beachten Sie, dass BLOB Storage-Konten Block-und anfügeblobs unterstützen, aber keine seitenblobs.

Ein BLOB wird in Azure Storage hochgeladen und von Azure Storage als Bytestream heruntergeladen. Daher müssen Dateien vor dem Hochladen in einen Bytestream konvertiert und nach dem Download wieder in ihre ursprüngliche Darstellung konvertiert werden.

Jedes Objekt, das in Azure Storage gespeichert ist, verfügt über eine eindeutige URL-Adresse. Der Name des Speicher Kontos bildet die Unterdomäne dieser Adresse, und die Kombination aus Unterdomäne und Domänen Name bildet einen *Endpunkt* für das Speicherkonto. Wenn Ihr Speicherkonto z. b. den Namen *mystorageaccount*hat, lautet der standardmäßige BLOB-Endpunkt für das Speicherkonto `https://mystorageaccount.blob.core.windows.net` .

Die URL für den Zugriff auf ein Objekt in einem Speicherkonto wird durch Anhängen des Objektstandorts im Speicherkonto an den Endpunkt generiert. Beispielsweise hat eine BLOB-Adresse das Format `https://mystorageaccount.blob.core.windows.net/mycontainer/myblob` .

## <a name="setup"></a>Setup

Der Prozess für die Integration eines Azure Storage Kontos in eine- Xamarin.Forms Anwendung sieht wie folgt aus:

1. Erstellen Sie ein Speicherkonto. Weitere Informationen finden Sie unter [Erstellen eines Speicherkontos](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/#create-a-storage-account).
1. Fügen Sie der Anwendung die [Azure Storage Client Bibliothek](https://www.nuget.org/packages/WindowsAzure.Storage/) hinzu Xamarin.Forms .
1. Konfigurieren Sie die Speicher Verbindungs Zeichenfolge. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit Azure Storage](#connecting-to-azure-storage).
1. Fügen Sie `using` `Microsoft.WindowsAzure.Storage` `Microsoft.WindowsAzure.Storage.Blob` Klassen, die auf Azure Storage zugreifen, Direktiven für die Namespaces und hinzu.

## <a name="connecting-to-azure-storage"></a>Herstellen einer Verbindung mit Azure Storage

Jede Anforderung, die an Speicherkonto Ressourcen gerichtet ist, muss authentifiziert werden. Während BLOB für die Unterstützung der anonymen Authentifizierung konfiguriert werden können, gibt es zwei Hauptansätze, die eine Anwendung verwenden kann, um sich mit einem Speicherkonto zu authentifizieren:

- Gemeinsam verwendeter Schlüssel. Bei diesem Ansatz werden der Name des Azure Storage Kontos und der Kontoschlüssel verwendet, um auf Speicherdienste zuzugreifen. Einem Speicherkonto werden bei der Erstellung zwei private Schlüssel zugewiesen, die für die Authentifizierung mit gemeinsam verwendetem Schlüssel verwendet werden können.
- Shared Access Signature. Hierbei handelt es sich um ein Token, das an eine URL angefügt werden kann, die den Delegierten Zugriff auf eine Speicher Ressource mit den von ihr festgelegten Berechtigungen für den Gültigkeits Zeitraum ermöglicht.

Es können Verbindungs Zeichenfolgen angegeben werden, die die für den Zugriff auf Azure Storage Ressourcen von einer Anwendung erforderlichen Authentifizierungsinformationen enthalten. Außerdem kann eine Verbindungs Zeichenfolge konfiguriert werden, um von Visual Studio aus eine Verbindung mit dem Azure Storage-Emulator herzustellen.

> [!NOTE]
> Azure Storage unterstützt HTTP und HTTPS in einer Verbindungs Zeichenfolge. Die Verwendung von HTTPS wird jedoch empfohlen.

### <a name="connecting-to-the-azure-storage-emulator"></a>Herstellen einer Verbindung mit dem Azure Storage-Emulator

Der Azure Storage Emulator bietet eine lokale Umgebung, die die Azure-BLOB-, Warteschlangen-und Tabellen Dienste zu Entwicklungszwecken emuliert.

Die folgende Verbindungs Zeichenfolge sollte verwendet werden, um eine Verbindung mit dem Azure Storage Emulator herzustellen:

```csharp
UseDevelopmentStorage=true
```

Weitere Informationen zum Azure Storage Emulator finden Sie unter [Verwenden des Azure Storage Emulators für Entwicklung und Tests](https://azure.microsoft.com/documentation/articles/storage-use-emulator/).

### <a name="connecting-to-azure-storage-using-a-shared-key"></a>Herstellen einer Verbindung mit Azure Storage mithilfe eines gemeinsam genutzten Schlüssels

Das folgende Format der Verbindungs Zeichenfolge sollte zum Herstellen einer Verbindung mit Azure Storage mit einem gemeinsam genutzten Schlüssel verwendet werden:

```csharp
DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey
```

`myAccountName`muss durch den Namen Ihres Speicher Kontos ersetzt werden und sollte durch `myAccountKey` einen ihrer beiden Konto Zugriffsschlüssel ersetzt werden.

> [!NOTE]
> Wenn Sie die Authentifizierung mit gemeinsam verwendetem Schlüssel verwenden, werden Ihr Kontoname und der Kontoschlüssel an alle Personen verteilt, die Ihre Anwendung verwenden. Dadurch erhält der vollständige Lese-/Schreibzugriff auf das Speicherkonto. Verwenden Sie daher die Authentifizierung mit gemeinsam verwendetem Schlüssel nur für Testzwecke, und verteilen Sie Schlüssel niemals an andere Benutzer.

### <a name="connecting-to-azure-storage-using-a-shared-access-signature"></a>Herstellen einer Verbindung mit Azure Storage mithilfe einer Shared Access Signature

Das folgende Format der Verbindungs Zeichenfolge sollte verwendet werden, um eine Verbindung mit Azure Storage mit einer SAS herzustellen:

`BlobEndpoint=myBlobEndpoint;SharedAccessSignature=mySharedAccessSignature`

`myBlobEndpoint`sollte durch die URL Ihres BLOB-Endpunkts ersetzt werden und `mySharedAccessSignature` muss durch ihre SAS ersetzt werden. Die SAS stellt das Protokoll, den Dienst Endpunkt und die Anmelde Informationen für den Zugriff auf die Ressource bereit.

> [!NOTE]
> Die SAS-Authentifizierung wird für Produktionsanwendungen empfohlen. In einer Produktionsanwendung sollte die SAS jedoch bei Bedarf von einem Back-End-Dienst abgerufen werden, anstatt mit der Anwendung gebündelt zu werden.

Weitere Informationen zu Shared Access Signature finden Sie unter [Verwenden von Shared Access Signature (SAS)](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/).

## <a name="creating-a-container"></a>Erstellen eines Containers

Die- `GetContainer` Methode wird zum Abrufen eines Verweises auf einen benannten Container verwendet, der dann zum Abrufen von blobcontainern aus dem Container oder zum Hinzufügen von blobcontainern zum Container verwendet werden kann. Die `GetContainer`-Methode wird in folgendem Codebeispiel veranschaulicht:

```csharp
static CloudBlobContainer GetContainer(ContainerType containerType)
{
  var account = CloudStorageAccount.Parse(Constants.StorageConnection);
  var client = account.CreateCloudBlobClient();
  return client.GetContainerReference(containerType.ToString().ToLower());
}
```

Die `CloudStorageAccount.Parse` -Methode analysiert eine Verbindungs Zeichenfolge und gibt eine- `CloudStorageAccount` Instanz zurück, die das Speicherkonto darstellt. Eine- `CloudBlobClient` Instanz, die zum Abrufen von Containern und blobdaten verwendet wird, wird von der- `CreateCloudBlobClient` Methode erstellt. Die `GetContainerReference` -Methode ruft den angegebenen Container als- `CloudBlobContainer` Instanz ab, bevor er an die aufrufende-Methode zurückgegeben wird. In diesem Beispiel ist der Container Name der- `ContainerType` Enumerationswert, der in eine Zeichenfolge in Kleinbuchstaben konvertiert wird.

> [!NOTE]
> Container Namen müssen Kleinbuchstaben sein und müssen mit einem Buchstaben oder einer Zahl beginnen. Außerdem können Sie nur Buchstaben, Ziffern und Bindestriche enthalten und müssen zwischen 3 und 63 Zeichen lang sein.

Die- `GetContainer` Methode wird wie folgt aufgerufen:

```csharp
var container = GetContainer(containerType);
```

Die `CloudBlobContainer` Instanz kann dann verwendet werden, um einen Container zu erstellen, wenn er nicht bereits vorhanden ist:

```csharp
await container.CreateIfNotExistsAsync();
```

Standardmäßig ist ein neu erstellter Container privat. Dies bedeutet, dass ein Speicherzugriffs Schlüssel angegeben werden muss, um BLOB aus dem Container abzurufen. Weitere Informationen zum Erstellen von blobcontainern in einem Container finden Sie unter [Erstellen eines Containers](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#create-a-container).

## <a name="uploading-data-to-a-container"></a>Hochladen von Daten in einen Container

Die `UploadFileAsync` -Methode wird verwendet, um einen Stream von Bytedaten in den BLOB-Speicher hochzuladen, und wird im folgenden Codebeispiel gezeigt:

```csharp
public static async Task<string> UploadFileAsync(ContainerType containerType, Stream stream)
{
  var container = GetContainer(containerType);
  await container.CreateIfNotExistsAsync();

  var name = Guid.NewGuid().ToString();
  var fileBlob = container.GetBlockBlobReference(name);
  await fileBlob.UploadFromStreamAsync(stream);

  return name;
}
```

Nachdem Sie einen Container Verweis abgerufen haben, erstellt die-Methode den Container, wenn er nicht bereits vorhanden ist. `Guid`Anschließend wird eine neue erstellt, die als eindeutiger BLOB-Name fungiert, und ein BLOB-Block Verweis wird als- `CloudBlockBlob` Instanz abgerufen. Der Datenstrom wird dann mithilfe der-Methode in das BLOB hochgeladen `UploadFromStreamAsync` , wodurch das BLOB erstellt wird, wenn es nicht bereits vorhanden ist, oder überschreibt es, falls es vorhanden ist.

Bevor eine Datei mithilfe dieser Methode in den BLOB-Speicher hochgeladen werden kann, muss Sie zuerst in einen Bytestream konvertiert werden. Dies wird im folgenden Codebeispiel veranschaulicht:

```csharp
var byteData = Encoding.UTF8.GetBytes(text);
uploadedFilename = await AzureStorage.UploadFileAsync(ContainerType.Text, new MemoryStream(byteData));
```

Die `text` Daten werden in ein Bytearray konvertiert, das dann als Datenstrom umgeschrieben wird, der an die-Methode weitergegeben wird `UploadFileAsync` .

## <a name="downloading-data-from-a-container"></a>Herunterladen von Daten aus einem Container

Die `GetFileAsync` -Methode wird verwendet, um BLOB-Daten aus Azure Storage herunterzuladen, und wird im folgenden Codebeispiel gezeigt:

```csharp
public static async Task<byte[]> GetFileAsync(ContainerType containerType, string name)
{
  var container = GetContainer(containerType);

  var blob = container.GetBlobReference(name);
  if (await blob.ExistsAsync())
  {
    await blob.FetchAttributesAsync();
    byte[] blobBytes = new byte[blob.Properties.Length];

    await blob.DownloadToByteArrayAsync(blobBytes, 0);
    return blobBytes;
  }
  return null;
}
```

Nachdem Sie einen Container Verweis abgerufen haben, ruft die-Methode einen BLOB-Verweis für die gespeicherten Daten ab. Wenn das BLOB vorhanden ist, werden seine Eigenschaften von der- `FetchAttributesAsync` Methode abgerufen. Ein Bytearray mit der richtigen Größe wird erstellt, und das BLOB wird als Bytearray heruntergeladen, das an die aufrufende Methode zurückgegeben wird.

Nachdem die BLOB-Byte Daten heruntergeladen wurden, müssen Sie in ihre ursprüngliche Darstellung konvertiert werden. Dies wird im folgenden Codebeispiel veranschaulicht:

```csharp
var byteData = await AzureStorage.GetFileAsync(ContainerType.Text, uploadedFilename);
string text = Encoding.UTF8.GetString(byteData);
```

Das Bytearray wird von der-Methode von Azure Storage abgerufen `GetFileAsync` , bevor es in eine UTF8-codierte Zeichenfolge zurück konvertiert wird.

## <a name="listing-data-in-a-container"></a>Auflisten von Daten in einem Container

Die `GetFilesListAsync` -Methode wird zum Abrufen einer Liste von BLOB-Daten verwendet, die in einem Container gespeichert sind, und wird im folgenden Codebeispiel gezeigt:

```csharp
public static async Task<IList<string>> GetFilesListAsync(ContainerType containerType)
{
  var container = GetContainer(containerType);

  var allBlobsList = new List<string>();
  BlobContinuationToken token = null;

  do
  {
    var result = await container.ListBlobsSegmentedAsync(token);
    if (result.Results.Count() > 0)
    {
      var blobs = result.Results.Cast<CloudBlockBlob>().Select(b => b.Name);
      allBlobsList.AddRange(blobs);
    }
    token = result.ContinuationToken;
  } while (token != null);

  return allBlobsList;
}
```

Nach dem Abrufen eines Container Verweises verwendet die-Methode die-Methode des Containers, `ListBlobsSegmentedAsync` um Verweise auf die blobwerte innerhalb des Containers abzurufen. Die von der-Methode zurückgegebenen Ergebnisse `ListBlobsSegmentedAsync` werden aufgelistet, wenn die- `BlobContinuationToken` Instanz nicht ist `null` . Jedes BLOB wird vom zurückgegebenen `IListBlobItem` in eine umgewandelt `CloudBlockBlob` , um auf die `Name` -Eigenschaft des BLOBs zuzugreifen, bevor der Wert der-Auflistung hinzugefügt wird `allBlobsList` . Sobald die `BlobContinuationToken` Instanz ist `null` , wurde der letzte BLOB-Name zurückgegeben, und die Ausführung wird durch die Ausführung beendet.

## <a name="deleting-data-from-a-container"></a>Löschen von Daten aus einem Container

Die `DeleteFileAsync` -Methode wird verwendet, um ein BLOB aus einem Container zu löschen, und wird im folgenden Codebeispiel gezeigt:

```csharp
public static async Task<bool> DeleteFileAsync(ContainerType containerType, string name)
{
  var container = GetContainer(containerType);
  var blob = container.GetBlobReference(name);
  return await blob.DeleteIfExistsAsync();
}
```

Nachdem Sie einen Container Verweis abgerufen haben, ruft die-Methode einen BLOB-Verweis für das angegebene BLOB ab. Das BLOB wird dann mit der- `DeleteIfExistsAsync` Methode gelöscht.

## <a name="related-links"></a>Verwandte Links

- [Azure Storage (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-azurestorage)
- [Einführung in den Speicher](https://azure.microsoft.com/documentation/articles/storage-introduction/)
- [Verwenden des Blobspeichers mit Xamarin](https://azure.microsoft.com/documentation/articles/storage-xamarin-blob-storage/)
- [Verwenden von Shared Access Signatures (SAS)](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)
- [Windows-Azure Storage (nuget)](https://www.nuget.org/packages/WindowsAzure.Storage/)
