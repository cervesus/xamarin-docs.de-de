---
title: Store und den Zugriff auf Daten im Azure-Speicher von Xamarin.Forms
description: Azure Storage ist ein skalierbarer Cloud-speicherlösung, die zum Speichern von unstrukturierter und strukturierter Daten verwendet werden kann. In diesem Artikel wird erläutert, wie Xamarin.Forms zum Speichern von Text- und Binärdaten im Azure-Speicher zu verwenden, und die Daten zugreifen.
ms.prod: xamarin
ms.assetid: 5B10D37B-839B-4CD0-9C65-91014A93F3EB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/28/2018
ms.openlocfilehash: 044ff7448cc302da4d0efdf88325c40b9db0315c
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2019
ms.locfileid: "67658627"
---
# <a name="store-and-access-data-in-azure-storage-from-xamarinforms"></a>Store und den Zugriff auf Daten im Azure-Speicher von Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureStorage/)

_Azure Storage ist ein skalierbarer Cloud-speicherlösung, die zum Speichern von unstrukturierter und strukturierter Daten verwendet werden kann. In diesem Artikel wird veranschaulicht, wie Xamarin.Forms zum Speichern von Text- und Binärdaten im Azure-Speicher zu verwenden, und die Daten zugreifen._

Azure Storage bietet vier Storage-Dienste:

- BLOB-Speicher. Ein Blob kann Daten von Text- oder Binärdaten, z. B. Sicherungen, virtuelle Computer, Dokumente oder Mediendateien sein.
- Table Storage ist ein NoSQL-Schlüsselattribute-Speicher.
- Queue Storage ist eine messaging-Dienst für die workflowverarbeitung und Kommunikation zwischen Cloud-Diensten.
- File Storage bietet freigegebenen Speicher, die über das SMB-Protokoll.

Es gibt zwei Arten von Speicherkonten:

- Ein allgemeines Speicherkonto bietet Zugriff auf Azure Storage-Dienste, von einem einzelnen Konto.
- Blob Storage-Konto ist eine spezielle Speicherkonten zum Speichern von Blobs. Dieser Kontotyp wird empfohlen, wenn Sie nur zum Speichern von Blob-Daten benötigen.

In diesem Artikel und die zugehörige beispielanwendung veranschaulicht, Hochladen von Bild- und Dateien in Blob Storage, und sie herunterzuladen. Darüber hinaus wird auch eine Liste von Dateien aus Blob Storage abrufen und Löschen von Dateien.

Weitere Informationen zu Azure Storage finden Sie unter [Einführung in Storage](https://azure.microsoft.com/documentation/articles/storage-introduction/).

## <a name="introduction-to-blob-storage"></a>Einführung in Blob Storage

BLOB-Speicher besteht aus drei Komponenten, die in der folgenden Abbildung dargestellt sind:

![](azure-storage-images/blob-storage.png "Konzepte des BLOB-Speicher")

Alle Zugriffe auf Azure Storage erfolgt über ein Speicherkonto. Ein Speicherkonto kann eine unbegrenzte Anzahl von Containern enthalten, und einem Container kann eine unbegrenzte Anzahl von Blobs, die bis zur speicherkapazitätsgrenze des Speicherkontos gespeichert.

Ein Blob ist eine Datei von beliebiger Art und Größe. Azure Storage unterstützt drei verschiedene blobtypen:

- Block-Blobs sind für das Streamen und Speichern von cloudobjekten optimiert und eignen sich gut zum Speichern von Sicherungen, Mediendateien, Dokumente usw. Block-Blobs können bis zu 195 Gb groß sein.
- Fügen Sie Blobs ähneln blockblobs sind aber für Anfügevorgänge, z. B. die Protokollierung. Append-Blobs können bis zu 195Gb groß sein.
- Seitenblobs sind für häufige Lese/Schreibvorgänge optimiert und in der Regel zum Speichern von virtuellen Computern und deren Datenträger verwendet werden. Seiten-Blobs können bis zu 1 Tb groß sein.

> [!NOTE]
> Beachten Sie, dass die Blob Storage-Konten blockieren und unterstützt append Blobs, jedoch keine Seiten-Blobs.

Ein Blob ist in Azure Storage hochgeladen und aus Azure Storage heruntergeladen werden, als Datenstrom von Bytes. Aus diesem Grund müssen die Dateien in einen Datenstrom von Bytes vor dem Hochladen und konvertierte zurück in ihre ursprüngliche Darstellung nach dem Download konvertiert werden.

Jedes Objekt, das in Azure Storage gespeichert ist, hat eine eindeutige URL-Adresse. Der Name des Speicherkontos bildet die Unterdomäne dieser Adresse und die Kombination aus Unterdomäne und Domänenname Namen Formulare ein *Endpunkt* für das Speicherkonto an. Wenn Ihr Speicherkonto mit dem Namen wird z. B. *Mystorageaccount*, der standardmäßigen Blob-Endpunkt für das Speicherkonto ist `https://mystorageaccount.blob.core.windows.net`.

Die URL für den Zugriff auf ein Objekt in einem Speicherkonto wird durch Anhängen des objektstandorts im Speicherkonto, das an den Endpunkt erstellt. Beispielsweise wird eine blobadresse haben das Format `https://mystorageaccount.blob.core.windows.net/mycontainer/myblob`.

## <a name="setup"></a>Setup

Der Prozess zur Integration von Azure Storage-Konto in einer Xamarin.Forms-Anwendung lautet wie folgt aus:

1. Erstellen Sie ein Speicherkonto. Weitere Informationen finden Sie unter [Erstellen eines Speicherkontos](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/#create-a-storage-account).
1. Hinzufügen der [Azure Storage Client Library](https://www.nuget.org/packages/WindowsAzure.Storage/) der Xamarin.Forms-Anwendung.
1. Konfigurieren Sie die speicherverbindungszeichenfolge. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit Azure Storage](#connecting).
1. Hinzufügen `using` Direktiven für die `Microsoft.WindowsAzure.Storage` und `Microsoft.WindowsAzure.Storage.Blob` Namespaces, Klassen, die Azure-Speicher zugegriffen werden.

<a name="connecting" />

## <a name="connecting-to-azure-storage"></a>Herstellen einer Verbindung mit Azure-Speicher

Jede Anforderung für Speicherkonto-Ressourcen muss authentifiziert werden. Während es sich bei Blobs um die Unterstützung anonymer Authentifizierungen konfiguriert werden können, gibt es zwei Hauptansätze, mit denen eine Anwendung mit einem Speicherkonto zu authentifizieren:

- Gemeinsam verwendeten Schlüssel. Dieser Ansatz verwendet den Azure Storage-Konto und der Schlüssel zum Zugreifen auf Storage-Dienste. Ein Speicherkonto wird mit zwei private Schlüssel bei der Erstellung zugewiesen, die für die Authentifizierung mit gemeinsam verwendetem Schlüssel verwendet werden kann.
- Shared Access Signatures. Dies ist ein Token, das an eine URL angefügt werden kann, die delegierten Zugriff auf eine Speicherressource ermöglicht, mit den Berechtigungen er gibt an, für die Zeitspanne, die es gültig ist.

Verbindungszeichenfolgen können angegeben werden, die die Authentifizierungsinformationen, die auf Azure Storage-Ressourcen aus einer Anwendung erforderlich sind. Darüber hinaus kann eine Verbindungszeichenfolge konfiguriert werden, um die Verbindung mit Azure Storage-Emulator in Visual Studio.

> [!NOTE]
> Azure Storage unterstützt HTTP- und HTTPS in einer Verbindungszeichenfolge an. Es wird jedoch empfohlen, diese mithilfe von HTTPS.

### <a name="connecting-to-the-azure-storage-emulator"></a>Herstellen einer Verbindung mit dem Azure-Speicheremulator

Der Azure-Speicheremulator bietet eine lokale Umgebung, die die Azure-Blob, Warteschlange und Tabelle-Tabellendienste für Entwicklungszwecke emuliert.

Die folgende Verbindungszeichenfolge sollte verwendet werden, für die Verbindung mit dem Azure-Speicheremulator:

```csharp
UseDevelopmentStorage=true
```

Weitere Informationen zu den Azure-Speicheremulator, finden Sie unter [Verwenden des Azure-Speicheremulators für Entwicklung und Tests](https://azure.microsoft.com/documentation/articles/storage-use-emulator/).

### <a name="connecting-to-azure-storage-using-a-shared-key"></a>Herstellen einer Verbindung mit Azure-Speicher mit einem gemeinsam verwendeten Schlüssel

Das folgende Format der Verbindungszeichenfolge sollte mit einem gemeinsam verwendeten Schlüssel eine Verbindung mit Azure Storage verwendet werden:

```csharp
DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey
```

`myAccountName` mit dem Namen Ihres Speicherkontos ersetzt werden sollte und `myAccountKey` mit einer der Zugriffsschlüssel Ihres zwei ersetzt werden soll.

> [!NOTE]
> Wenn Schlüsselauthentifizierung, Ihren Kontonamen und kontoschlüssel gemeinsam genutzte wird an jede Person verteilt, die Ihre Anwendung verwendet, die vollständige Lese-/Schreibzugriff auf das Speicherkonto erhalten. Aus diesem Grund für die Authentifizierung mit gemeinsam verwendetem Schlüssel nur für Testzwecke verwenden, und Schlüssel für andere Benutzer niemals zu verteilen.

### <a name="connecting-to-azure-storage-using-a-shared-access-signature"></a>Herstellen einer Verbindung mit Azure Storage mithilfe einer Shared Access Signature

Verbindung mit Azure Storage mit einer SAS, sollte das folgende Format der Verbindungszeichenfolge verwendet werden:

`BlobEndpoint=myBlobEndpoint;SharedAccessSignature=mySharedAccessSignature`

`myBlobEndpoint` mit der Endpunkt Ihres Blob-URL ersetzt werden sollte und `mySharedAccessSignature` mit Ihre SAS ersetzt werden soll. Die SAS enthält das Protokoll, den Dienstendpunkt und die Anmeldeinformationen für die Ressource zuzugreifen.

> [!NOTE]
> SAS-Authentifizierung wird für Produktionsanwendungen empfohlen. In einer produktionsanwendung muss die SAS jedoch aus einer Back-End-Dienst bei Bedarf, anstatt wird zusammen mit der Anwendung abgerufen werden.

Weitere Informationen zu Shared Access Signatures finden Sie unter [mithilfe von Shared Access Signatures (SAS)](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/).

## <a name="creating-a-container"></a>Erstellen eines Containers

Die `GetContainer` Methode wird verwendet, um einen Verweis auf ein benannter Container, abrufen, die dann zum Abrufen von Blobs aus dem Container oder Hinzufügen von Blobs in den Container verwendet werden kann. Das folgende Codebeispiel zeigt die `GetContainer` Methode:

```csharp
static CloudBlobContainer GetContainer(ContainerType containerType)
{
  var account = CloudStorageAccount.Parse(Constants.StorageConnection);
  var client = account.CreateCloudBlobClient();
  return client.GetContainerReference(containerType.ToString().ToLower());
}
```

Die `CloudStorageAccount.Parse` -Methode analysiert eine Verbindungszeichenfolge und gibt eine `CloudStorageAccount` -Instanz, die Storage-Konto darstellt. Ein `CloudBlobClient` -Instanz, die zum Abrufen von Containern und Blobs verwendet wird, wird dann erstellt, durch die `CreateCloudBlobClient` Methode. Die `GetContainerReference` -Methode ruft ab, dem angegebenen Container ein `CloudBlobContainer` -Instanz, bevor er an die aufrufende Methode zurückgegeben wird. In diesem Beispiel wird der Name des Containers die `ContainerType` Enumerationswert, der in eine Zeichenfolge aus Kleinbuchstaben konvertiert.

> [!NOTE]
> Containernamen müssen Kleinbuchstaben sein und müssen mit einem Buchstaben oder einer Ziffer beginnen. Darüber hinaus, sie darf nur Buchstaben, Zahlen und Bindestriche enthalten und müssen zwischen 3 und 63 Zeichen lang sein.

Die `GetContainer` Methode wird aufgerufen, wie folgt:

```csharp
var container = GetContainer(containerType);
```

Die `CloudBlobContainer` Instanz kann dann verwendet, um einen Container zu erstellen, wenn er nicht bereits vorhanden:

```csharp
await container.CreateIfNotExistsAsync();
```

Standardmäßig ist ein neu erstellter Container privat. Dies bedeutet ein speicherzugriffsschlüssel muss angegeben werden, um Blobs aus dem Container abzurufen. Informationen über die Blobs in einem Container öffentlich machen, finden Sie unter [erstellen Sie einen Container](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#create-a-container).

## <a name="uploading-data-to-a-container"></a>Hochladen von Daten in einem Container

Die `UploadFileAsync` Methode wird verwendet, um das Hochladen eines Datenstroms von Bytedaten, blob-Speicher und wird im folgenden Codebeispiel dargestellt:

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

Nach dem Abrufen einer Referenz zur Container, erstellt die Methode den Container aus, wenn er nicht bereits vorhanden. Ein neues `Guid` erstellt, um als einen eindeutigen blobnamen zu fungieren und ein Block-Blob-Verweis abgerufen wird, als ein `CloudBlockBlob` Instanz. Der Datenstrom wird dann mit dem Blob hochgeladen der `UploadFromStreamAsync` Methode, die das Blob erstellt, falls es noch nicht vorhanden ist, oder überschreibt ihn, wenn er vorhanden ist.

Bevor eine Datei in BLOB Storage, die mit dieser Methode hochgeladen werden kann, müssen sie zuerst in einen Bytestream konvertiert werden. Dies wird im folgenden Codebeispiel veranschaulicht:

```csharp
var byteData = Encoding.UTF8.GetBytes(text);
uploadedFilename = await AzureStorage.UploadFileAsync(ContainerType.Text, new MemoryStream(byteData));
```

Die `text` Daten werden in ein Bytearray, das dann als Datenstrom umschlossen wird, die übergeben werden, konvertiert die `UploadFileAsync` Methode.

## <a name="downloading-data-from-a-container"></a>Herunterladen von Daten aus einem Container

Die `GetFileAsync` Methode wird verwendet, um das Herunterladen von Blob-Daten aus Azure Storage und wird im folgenden Codebeispiel dargestellt:

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

Nach dem Abrufen einer Referenz zur Container, ruft die Methode einen Blob-Verweis für die gespeicherten Daten ab. Wenn das Blob vorhanden ist, werden die Eigenschaften abgerufen, indem die `FetchAttributesAsync` Methode. Ein Bytearray der richtigen Größe wird erstellt, und das Blob wird heruntergeladen, als ein Array von Bytes, die an die aufrufende Methode zurückgegeben wird.

Nach dem Herunterladen der Byte-Blob-Daten muss es in die ursprüngliche Darstellung konvertiert werden. Dies wird im folgenden Codebeispiel veranschaulicht:

```csharp
var byteData = await AzureStorage.GetFileAsync(ContainerType.Text, uploadedFilename);
string text = Encoding.UTF8.GetString(byteData);
```

Das Array von Bytes wird abgerufen, aus dem Azure-Speicher von der `GetFileAsync` -Methode, vor der Konvertierung wieder in einer UTF8-codierte Zeichenfolge.

## <a name="listing-data-in-a-container"></a>Auflisten von Daten in einem Container

Die `GetFilesListAsync` Methode dient zum Abrufen einer Liste von Blobs in einem Container gespeichert und wird im folgenden Codebeispiel dargestellt:

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

Nach dem Abrufen einer Referenz zur Container, verwendet die Methode des Containers `ListBlobsSegmentedAsync` Methode, um Verweise auf die Blobs im Container abzurufen. Die Ergebnisse der `ListBlobsSegmentedAsync` Methode aufgezählt werden während der `BlobContinuationToken` Instanz ist nicht `null`. Jedes Blob wird umgewandelt von dem zurückgegebenen `IListBlobItem` auf eine `CloudBlockBlob` Reihenfolge Zugriff die `Name` Eigenschaft des BLOBs vor dem Wert hinzugefügt wird die `allBlobsList` Auflistung. Nach der `BlobContinuationToken` Instanz `null`, der Nachnamen des Blob zurückgegeben wurde und Ausführung wird die Schleife beendet.

## <a name="deleting-data-from-a-container"></a>Löschen von Daten aus einem Container

Die `DeleteFileAsync` Methode wird verwendet, um das Löschen eines BLOBs aus einem Container und wird im folgenden Codebeispiel dargestellt:

```csharp
public static async Task<bool> DeleteFileAsync(ContainerType containerType, string name)
{
  var container = GetContainer(containerType);
  var blob = container.GetBlobReference(name);
  return await blob.DeleteIfExistsAsync();
}
```

Nach dem Abrufen einer Referenz zur Container, ruft die Methode einen Blob-Verweis für das angegebene Blob ab. Das Blob wird dann gelöscht, mit der `DeleteIfExistsAsync` Methode.

## <a name="related-links"></a>Verwandte Links

- [Azure-Speicher (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureStorage/)
- [Einführung in Storage](https://azure.microsoft.com/documentation/articles/storage-introduction/)
- [Gewusst wie: Verwenden des Blobspeichers mit Xamarin](https://azure.microsoft.com/documentation/articles/storage-xamarin-blob-storage/)
- [Verwenden von Shared Access Signatures (SAS)](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)
- [Windows Azure-Speicher (NuGet)](https://www.nuget.org/packages/WindowsAzure.Storage/)
