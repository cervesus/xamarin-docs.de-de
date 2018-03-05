---
title: Speichern und Zugreifen auf Daten in Azure Storage
description: "Azure-Speicher ist eine skalierbare Cloud-speicherlösung, die verwendet werden kann, um unstrukturierte und strukturierte Daten zu speichern. In diesem Artikel wird veranschaulicht, wie Sie Xamarin.Forms verwenden, um Text und Binärdaten im Azure-Speicher zu speichern und zum Zugreifen auf die Daten."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5B10D37B-839B-4CD0-9C65-91014A93F3EB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
ms.openlocfilehash: d2d85840a0c698bfd3aa01dbacb204072ecca119
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="storing-and-accessing-data-in-azure-storage"></a>Speichern und Zugreifen auf Daten in Azure Storage

_Azure-Speicher ist eine skalierbare Cloud-speicherlösung, die verwendet werden kann, um unstrukturierte und strukturierte Daten zu speichern. In diesem Artikel wird veranschaulicht, wie Sie Xamarin.Forms verwenden, um Text und Binärdaten im Azure-Speicher zu speichern und zum Zugreifen auf die Daten._

## <a name="overview"></a>Übersicht

Azure-Speicher stellt vier Speicherdienste bereit:

- BLOB-Speicher. Ein Blob kann Daten von Text- oder Binärdaten, z. B. Sicherungen, virtuellen Maschinen, Mediendateien oder Dokumente sein.
- Tabellenspeicher ist ein NoSQL-Schlüsselattribut-Datenspeicher.
- Warteschlangenspeicher ist ein messaging-Dienst für die workflowverarbeitung und Kommunikation zwischen Cloud-Diensten.
- Speichern von Dateien bietet freigegebenen Speicher mithilfe des SMB-Protokolls.

Es gibt zwei Arten von Speicherkonten:

- Eine allgemeine Speicherkonten bietet Zugriff auf Azure-Speicherdienste, von einem einzelnen Konto.
- Ein Blob-Speicherkonto ist eine spezielle Speicherkonto zum Speichern von Blobs. Für diesen Kontotyp wird empfohlen, wenn Sie nur zum Speichern von Blob-Daten benötigen.

In diesem Artikel und zugehörigen beispielanwendung veranschaulicht hochladen Bild und Text-Dateien in blobspeicher herunterladen. Darüber hinaus wird auch eine Liste von Dateien aus Blob-Speicher abrufen und Löschen von Dateien.

Weitere Informationen zum Azure-Speicher finden Sie unter [Introduction to Storage](https://azure.microsoft.com/documentation/articles/storage-introduction/).

## <a name="introduction-to-blob-storage"></a>Einführung in den Blobspeicher

BLOB-Speicher besteht aus drei Komponenten, die in der folgenden Abbildung dargestellt werden:

![](azure-storage-images/blob-storage.png "Konzepte des BLOB-Speichers")

Der gesamte Zugriff auf Azure-Speicher erfolgt über ein Speicherkonto. Ein Speicherkonto kann eine unbegrenzte Anzahl von Containern enthalten, und ein Container kann eine unbegrenzte Anzahl von Blobs, bis die Kapazitätsgrenze des Speicherkontos zu speichern.

Ein Blob ist eine Datei eines beliebigen Typs und beliebiger Größe. Azure-Speicher unterstützt drei verschiedene Blob-Typen:

- Block-Blobs für streaming und Speichern von cloudobjekten optimiert sind und eine gute Wahl zum Speichern von Sicherungen, Mediendateien, Dokumente usw. sind. Block-Blobs können bis zu 195 Gb groß sein.
- Anfügen Blobs sind mit Block-Blobs vergleichbar, allerdings sind optimiert für Anfügevorgänge, wie z. B. Protokollierung. Anfügen Blobs können bis zu 195Gb groß sein.
- Seiten-Blobs für häufige Lese-/Schreibvorgänge optimiert sind und in der Regel zum Speichern von virtuellen Computern und Datenträgern verwendet werden. Seiten-Blobs können bis zu 1 Tb groß sein.

> [!NOTE]
> Beachten Sie, dass die Blob-Speicherkonten unterstützen Block und Anfügen von Blobs, jedoch keine Seiten-Blobs.

Ein Blob ist in den Azure-Speicher hochgeladen und heruntergeladen aus dem Azure-Speicher als Datenstrom von Bytes. Daher müssen Dateien in einen Datenstrom von Bytes vor dem Hochladen und konvertierte wieder an ihre ursprüngliche Darstellung nach dem Download konvertiert werden.

Alle Objekte, die im Azure-Speicher gespeichert sind verfügt über eine eindeutige URL-Adresse. Der Name des Speicherkontos bildet die Unterdomäne von dieser Adresse und die Kombination der Unterdomäne und Domain Name Formen ein *Endpunkt* für das Speicherkonto an. Wenn Ihr Speicherkonto heißt beispielsweise *Mystorageaccount*, der standardmäßigen Blob-Endpunkt für das Speicherkonto ist `https://mystorageaccount.blob.core.windows.net`.

Die URL für den Zugriff auf ein Objekt in einem Speicherkonto wird erstellt, durch Anfügen der Speicherort des Objekts im Speicherkonto, das an den Endpunkt. Beispielsweise wird eine Blob-Adresse haben das Format `https://mystorageaccount.blob.core.windows.net/mycontainer/myblob`.

## <a name="setup"></a>Setup

Der Prozess für die Integration von Azure Storage-Konto in einer Xamarin.Forms-Anwendung lautet wie folgt:

1. Erstellen Sie ein Speicherkonto. Weitere Informationen finden Sie unter [Erstellen eines Speicherkontos](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/#create-a-storage-account).
1. Hinzufügen der [Azure Storage Client Library](https://www.nuget.org/packages/WindowsAzure.Storage/) an die Xamarin.Forms-Anwendung.
1. Konfigurieren Sie die Speicher-Verbindungszeichenfolge. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit Azure-Speicher](#connecting).
1. Hinzufügen `using` Direktiven für die `Microsoft.WindowsAzure.Storage` und `Microsoft.WindowsAzure.Storage.Blob` Namespaces, Klassen, die Azure-Speicher zugreifen.

> [!NOTE]
> Während dieses Beispiel eine Shared Access-Projekt verwendet wird, unterstützt der Azure-Speicherclientbibliothek jetzt auch aus einem Projekt der portablen Klassenbibliothek (PCL) genutzt wird.

<a name="connecting" />

## <a name="connecting-to-azure-storage"></a>Herstellen einer Verbindung mit Azure-Speicher

Jede speicherkontoressourcen gerichtete Anforderung muss authentifiziert werden. Während Blobs konfiguriert werden können, um die anonyme Authentifizierung zu unterstützen, gibt es zwei Hauptansätze, die eine Anwendung verwenden kann, um ein Speicherkonto zu authentifizieren:

- Gemeinsam verwendeten Schlüssel. Dieser Ansatz verwendet den Azure Storage-Konto und der Schlüssel Storage-Dienste zugreifen. Ein Speicherkonto wird zwei private Schlüssel bei der Erstellung zugewiesen, die für die Authentifizierung mit gemeinsam verwendetem Schlüssel verwendet werden kann.
- Shared Access Signature. Dies ist ein Token, das an eine URL angefügt werden kann, die delegierten Zugriff auf eine Speicherressource ermöglicht, mit den Berechtigungen gibt, für die Zeitspanne, die es gültig ist.

Verbindungszeichenfolgen können angegeben werden, die die Authentifizierungsinformationen, die zum Zugreifen auf Azure-Speicherressourcen aus einer Anwendung erforderlich sind. Darüber hinaus kann eine Verbindungszeichenfolge zur Verbindung mit des Azure-Speicheremulators in Visual Studio konfiguriert werden.

> [!NOTE]
> Azure-Speicher unterstützt HTTP- und HTTPS in einer Verbindungszeichenfolge an. Es wird jedoch empfohlen, diese mithilfe von HTTPS.

### <a name="connecting-to-the-azure-storage-emulator"></a>Herstellen einer Verbindung mit der Azure-Speicheremulator

Der Azure-Speicher-Emulator ermöglicht eine lokale Umgebung, die die Azure-Blob, Warteschlangen- und Tabellendienste zu Entwicklungszwecken emuliert.

Die folgende Verbindungszeichenfolge sollte verwendet werden, für die Verbindung mit dem Azure-Speicher-Emulator:

```csharp
UseDevelopmentStorage=true
```

Weitere Informationen zu den Azure-Speicheremulator, finden Sie unter [Verwenden des Azure-Speicheremulators für Entwicklung und Tests](https://azure.microsoft.com/documentation/articles/storage-use-emulator/).

### <a name="connecting-to-azure-storage-using-a-shared-key"></a>Herstellen einer Verbindung mit Azure-Speicher mithilfe eines freigegebenen Schlüssels

Das folgende Verbindungszeichenfolgenformat sollte mit einem gemeinsamen Schlüssel eine Verbindung mit Azure-Speicher verwendet werden:

```csharp
DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey
```

`myAccountName` mit dem Namen Ihres Speicherkontos ersetzt werden sollte und `myAccountKey` mit einem der zwei Zugriffsschlüssel ersetzt werden sollte.

> [!NOTE]
> Wann verwenden Schlüsselauthentifizierung, Ihren Kontonamen und kontoschlüssel freigegeben werden, die jeder Person verteilt, die Ihre Anwendung verwendet wird, die vollständigen Lese-/Schreibzugriff auf das Speicherkonto zu erhalten. Aus diesem Grund für die Authentifizierung mit gemeinsam verwendetem Schlüssel nur für Testzwecke verwenden und nie Schlüssel an andere Benutzer verteilen.

### <a name="connecting-to-azure-storage-using-a-shared-access-signature"></a>Herstellen einer Verbindung mit Azure-Speicher mit einer Shared Access Signature

Das folgende Verbindungszeichenfolgenformat sollte für die Verbindung zum Azure-Speicher mit einer SAS verwendet werden:

`BlobEndpoint=myBlobEndpoint;SharedAccessSignature=mySharedAccessSignature`

`myBlobEndpoint` durch die URL der die Blob-Dienstendpunkt ersetzt werden sollte und `mySharedAccessSignature` mit Ihres SAS-Tokens ersetzt werden sollte. Die SAS enthält das Protokoll, den Dienstendpunkt und die Anmeldeinformationen für die Ressource zuzugreifen.

> [!NOTE]
> SAS-Authentifizierung wird für Produktionsanwendungen empfohlen. In einer produktionsanwendung sollte der SAS eines Back-End-Dienst bei Bedarf anstelle wird zusammen mit der Anwendung abgerufen werden.

Weitere Informationen zu Shared Access Signatures finden Sie unter [mithilfe von Shared Access Signatures (SAS)](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/).

## <a name="creating-a-container"></a>Erstellen eines Containers

Die `GetContainer` Methode wird verwendet, um einen Verweis auf ein benannter Container abrufen, die dann zum Abrufen von Blobs aus dem Container oder Blobs in den Container hinzufügen verwendet werden können. Das folgende Codebeispiel zeigt die `GetContainer` Methode:

```csharp
static CloudBlobContainer GetContainer(ContainerType containerType)
{
  var account = CloudStorageAccount.Parse(Constants.StorageConnection);
  var client = account.CreateCloudBlobClient();
  return client.GetContainerReference(containerType.ToString().ToLower());
}
```

Die `CloudStorageAccount.Parse` Methode analysiert eine Verbindungszeichenfolge und gibt eine `CloudStorageAccount` Instanz, die das Speicherkonto darstellt. Ein `CloudBlobClient` -Instanz, die zum Abrufen von Container und Blobs verwendet wird, wird erstellt, indem die `CreateCloudBlobClient` Methode. Die `GetContainerReference` Methode ruft ab, der angegebene Container als ein `CloudBlobContainer` -Instanz erfolgt, bevor es an die aufrufende Methode zurückgegeben wird. In diesem Beispiel wird der Containername wird die `ContainerType` Enumerationswert, der in eine Zeichenfolge in Kleinbuchstaben konvertiert.

> [!NOTE]
> Containernamen müssen Kleinbuchstaben sein und müssen mit einem Buchstaben oder einer Ziffer beginnen. Darüber hinaus, sie darf nur Buchstaben, Zahlen und Bindestriche enthalten und müssen zwischen 3 und 63 Zeichen lang sein.

Die `GetContainer` Methode aufgerufen wird, wie folgt:

```csharp
var container = GetContainer(containerType);
```

Die `CloudBlobContainer` Instanz kann dann verwendet, um einen Container zu erstellen, wenn er nicht bereits vorhanden:

```csharp
await container.CreateIfNotExistsAsync();
```

Standardmäßig ist der neu erstellter Container privat. Dies bedeutet, dass ein speicherzugriffsschlüssel werden, zum Abrufen von Blobs aus dem Container angegeben muss. Informationen zum vornehmen von Blobs in einem öffentlichen Container, finden Sie unter [erstellen Sie einen Container](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#create-a-container).

## <a name="uploading-data-to-a-container"></a>Hochladen von Daten in einem Container

Die `UploadFileAsync` Methode wird verwendet, um einen Datenstrom von Bytedaten, blob-Speicher hochladen und wird im folgenden Codebeispiel gezeigt:

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

Nach dem Abrufen einer Referenz zur Container, erstellt die Methode den Container aus, wenn er nicht bereits vorhanden. Ein neues `Guid` erstellt, um fungieren als eindeutige blobnamen und ein BLOB-Block Verweis abgerufen wird, als ein `CloudBlockBlob` Instanz. Der Datenstrom der wird dann mit dem Blob hochgeladen der `UploadFromStreamAsync` -Methode, die das Blob erstellt wird, wenn er nicht bereits vorhanden ist, oder überschrieben, wenn sie vorhanden ist.

Bevor Sie eine Datei hochgeladen werden kann, um blob-Speicher, die mit dieser Methode müssen sie zuerst in einen Bytestream konvertiert werden. Dies wird im folgenden Codebeispiel veranschaulicht:

```csharp
var byteData = Encoding.UTF8.GetBytes(text);
uploadedFilename = await AzureStorage.UploadFileAsync(ContainerType.Text, new MemoryStream(byteData));
```

Die `text` Daten werden in ein Bytearray, das dann als Stream umschlossen wird, der zum übergeben wird, konvertiert der `UploadFileAsync` Methode.

## <a name="downloading-data-from-a-container"></a>Herunterladen von Daten aus einem Container

Die `GetFileAsync` Methode wird verwendet, um Blob-Daten aus dem Azure-Speicher herunterladen, und wird im folgenden Codebeispiel gezeigt:

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

Nach dem Abrufen einer Referenz zur Container, ruft die Methode einen Blob-Verweis für die gespeicherten Daten ab. Wenn das Blob vorhanden ist, werden dessen Eigenschaften abgerufen, indem die `FetchAttributesAsync` Methode. Ein Bytearray mit der richtigen Größe wird erstellt, und das Blob wird heruntergeladen, als ein Array von Bytes, die an die aufrufende Methode zurückgegeben wird.

Nach dem Herunterladen der Byte-Blob-Daten, muss es in seiner ursprünglichen Darstellung konvertiert werden. Dies wird im folgenden Codebeispiel veranschaulicht:

```csharp
var byteData = await AzureStorage.GetFileAsync(ContainerType.Text, uploadedFilename);
string text = Encoding.UTF8.GetString(byteData);
```

Das Array von Bytes aus dem Azure-Speicher von abgerufen wird die `GetFileAsync` Methode, die vor der Konvertierung zurück in einen UTF8-codierte Zeichenfolge.

## <a name="listing-data-in-a-container"></a>Auflisten von Daten in einem Container

Die `GetFilesListAsync` Methode dient zum Abrufen einer Liste von Blobs in einem Container gespeichert, und wird im folgenden Codebeispiel gezeigt:

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

Nach dem Abrufen einer Referenz zur Container an, die Methode verwendet des Containers `ListBlobsSegmentedAsync` Methode, um Verweise auf die Blobs im Container abzurufen. Von der zurückgegebenen Ergebnisse der `ListBlobsSegmentedAsync` -Methode aufgezählt werden während der `BlobContinuationToken` Instanz ist keine `null`. Jedes Blob umgewandelt wird von der zurückgegebenen `IListBlobItem` auf eine `CloudBlockBlob` in Reihenfolge Zugriff der `Name` -Eigenschaft des Blob, bevor er Wert wird hinzugefügt der `allBlobsList` Auflistung. Einmal die `BlobContinuationToken` Instanz `null`der letzten Blob-Name zurückgegeben wurde und Ausführung wird die Schleife beendet.

## <a name="deleting-data-from-a-container"></a>Löschen von Daten aus einem Container

Die `DeleteFileAsync` Methode wird verwendet, um ein Blob aus einem Container zu löschen, und wird im folgenden Codebeispiel gezeigt:

```csharp
public static async Task<bool> DeleteFileAsync(ContainerType containerType, string name)
{
  var container = GetContainer(containerType);
  var blob = container.GetBlobReference(name);
  return await blob.DeleteIfExistsAsync();
}
```

Nach dem Abrufen einer Referenz zur Container, ruft die Methode einen Blob-Verweis für das angegebene Blob ab. Das Blob wird dann gelöscht, mit der `DeleteIfExistsAsync` Methode.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird veranschaulicht, wie Sie Xamarin.Forms verwenden, um Text und Binärdaten im Azure-Speicher zu speichern und zum Zugreifen auf die Daten. Azure-Speicher ist eine skalierbare Cloud-speicherlösung, die zum Speichern von unstrukturierte und strukturierte Daten verwendet werden kann.


## <a name="related-links"></a>Verwandte Links

- [Azure-Speicher (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureStorage/)
- [Einführung in den Speicher](https://azure.microsoft.com/documentation/articles/storage-introduction/)
- [Gewusst wie: Verwenden von Blob-Speicher von Xamarin](https://azure.microsoft.com/documentation/articles/storage-xamarin-blob-storage/)
- [Verwenden von Shared Access Signatures (SAS)](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)
- [Windows Azure Storage](https://www.nuget.org/packages/WindowsAzure.Storage/)
