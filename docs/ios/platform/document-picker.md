---
title: Dokument Auswahl in xamarin. IOS
description: In diesem Dokument wird die IOS-Dokument Auswahl beschrieben und erläutert, wie Sie Sie in xamarin. IOS verwenden. Sie sehen sich icloud, Dokumente, allgemeinen Setup Code, Dokument Anbieter Erweiterungen und vieles mehr an.
ms.prod: xamarin
ms.assetid: 89539D79-BC6E-4A3E-AEC6-69D9A6CC6818
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/05/2017
ms.openlocfilehash: bbb38acdb3de972cd7f2e2ee04233bf7ed88897a
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032588"
---
# <a name="document-picker-in-xamarinios"></a>Dokument Auswahl in xamarin. IOS

Die Dokument Auswahl ermöglicht die gemeinsame Nutzung von Dokumenten zwischen apps. Diese Dokumente können in icloud oder in einem anderen App-Verzeichnis gespeichert werden. Dokumente werden über den Satz von [Dokument Anbieter Erweiterungen](~/ios/platform/extensions.md) freigegeben, die der Benutzer auf seinem Gerät installiert hat. 

Aufgrund der Schwierigkeit, dass Dokumente über apps und die Cloud hinweg synchronisiert werden, stellen Sie eine gewisse erforderliche Komplexität dar.

## <a name="requirements"></a>Voraussetzungen

Folgendes ist erforderlich, um die in diesem Artikel beschriebenen Schritte auszuführen:

- **Xcode 7 und IOS 8 oder** höher – die APIs Xcode 7 und IOS 8 oder neuere APIs von Apple müssen auf dem Computer des Entwicklers installiert und konfiguriert werden.
- **Visual Studio oder Visual Studio für Mac** – die neueste Version von Visual Studio für Mac muss installiert sein.
- **IOS-Gerät** – ein IOS-Gerät, auf dem IOS 8 oder höher ausgeführt wird.

## <a name="changes-to-icloud"></a>Änderungen an icloud

Zum Implementieren der neuen Funktionen der Dokument Auswahl wurden die folgenden Änderungen am icloud-Dienst von Apple vorgenommen:

- Der icloud-Daemon wurde mithilfe von cloudkit vollständig umgeschrieben.
- Die vorhandenen icloud-Features wurden in icloud Drive umbenannt.
- Die Unterstützung für das Microsoft Windows-Betriebssystem wurde icloud hinzugefügt.
- Im Mac OS Finder wurde ein icloud-Ordner hinzugefügt.
- IOS-Geräte können auf den Inhalt des Ordners Mac OS icloud zugreifen.

> [!IMPORTANT]
> Apple [stellt Tools zur Verfügung](https://developer.apple.com/support/allowing-users-to-manage-data/), die Entwickler dabei unterstützen, die Datenschutz-Grundverordnung (DSGVO) der Europäischen Union umzusetzen.

## <a name="what-is-a-document"></a>Was ist ein Dokument?

Beim Verweisen auf ein Dokument in icloud ist es eine einzelne, eigenständige Entität und sollte vom Benutzer als solche wahrgenommen werden. Ein Benutzer möchte das Dokument möglicherweise ändern oder für andere Benutzer freigeben (z. b. per e-Mail).

Es gibt mehrere Typen von Dateien, die der Benutzer sofort als Dokumente erkennt, z. b. Seiten, Keynote-oder Zahlen Dateien. Icloud ist jedoch nicht auf dieses Konzept beschränkt. Beispielsweise kann der Status eines Spiels (z. b. eine Schach Übereinstimmung) als Dokument behandelt und in icloud gespeichert werden. Diese Datei kann zwischen den Geräten eines Benutzers weitergeleitet werden und es Ihnen ermöglichen, ein Spiel zu übernehmen, wo Sie auf einem anderen Gerät ausgelassen wurden.

## <a name="dealing-with-documents"></a>Umgang mit Dokumenten

Bevor Sie sich mit dem Code befassen, der für die Verwendung der Dokument Auswahl mit xamarin erforderlich ist, werden in diesem Artikel die bewährten Methoden für die Arbeit mit icloud-Dokumenten und einige der Änderungen an vorhandenen APIs behandelt, die zur Unterstützung der Dokument Auswahl erforderlich sind.

### <a name="using-file-coordination"></a>Verwenden der Datei Koordination

Da eine Datei von mehreren verschiedenen Speicherorten geändert werden kann, muss eine Koordinierung verwendet werden, um Datenverluste zu verhindern.

 [![](document-picker-images/image1.png "Using File Coordination")](document-picker-images/image1.png#lightbox)

Werfen wir einen Blick auf die obige Abbildung:

1. Ein IOS-Gerät, das die Datei Koordination verwendet, erstellt ein neues Dokument und speichert es im Ordner "icloud".
2. icloud speichert die geänderte Datei für die Verteilung an jedes Gerät in der Cloud.
3. Ein angefügter Mac sieht die geänderte Datei im Ordner "icloud" und verwendet die Datei Koordination, um die Änderungen in die Datei zu kopieren.
4. Ein Gerät, das die Datei Koordination nicht verwendet, nimmt eine Änderung an der Datei vor und speichert Sie im Ordner "icloud". Diese Änderungen werden sofort auf die anderen Geräte repliziert.

Nehmen Sie an, dass das ursprüngliche IOS-Gerät oder der Mac die Datei bearbeitet hat. Ihre Änderungen gehen nun verloren und werden mit der Version der Datei vom nicht koordinierten Gerät überschrieben. Um Datenverluste zu vermeiden, ist die Datei Koordination bei der Arbeit mit cloudbasierten Dokumenten ein muss.

### <a name="using-uidocument"></a>Verwenden von uidocument

 `UIDocument` macht das alles ganz einfach (oder `NSDocument` unter macOS), indem es die Arbeit für den Entwickler vollständig erschwert. Es bietet integrierte Datei Koordination mit Hintergrund Warteschlangen, um die Blockierung der Benutzeroberfläche der Anwendung zu verhindern.

 `UIDocument` stellt mehrere APIs auf hoher Ebene zur Verfügung, die den Entwicklungsaufwand einer xamarin-Anwendung für beliebige Zwecke des Entwicklers vereinfachen.

Mit dem folgenden Code wird eine Unterklasse von `UIDocument` erstellt, um ein generisches textbasiertes Dokument zu implementieren, das zum Speichern und Abrufen von Text aus icloud verwendet werden kann:

```csharp
using System;
using Foundation;
using UIKit;

namespace DocPicker
{
    public class GenericTextDocument : UIDocument
    {
        #region Private Variable Storage
        private NSString _dataModel;
        #endregion

        #region Computed Properties
        public string Contents {
            get { return _dataModel.ToString (); }
            set { _dataModel = new NSString(value); }
        }
        #endregion

        #region Constructors
        public GenericTextDocument (NSUrl url) : base (url)
        {
            // Set the default document text
            this.Contents = "";
        }

        public GenericTextDocument (NSUrl url, string contents) : base (url)
        {
            // Set the default document text
            this.Contents = contents;
        }
        #endregion

        #region Override Methods
        public override bool LoadFromContents (NSObject contents, string typeName, out NSError outError)
        {
            // Clear the error state
            outError = null;

            // Were any contents passed to the document?
            if (contents != null) {
                _dataModel = NSString.FromData( (NSData)contents, NSStringEncoding.UTF8 );
            }

            // Inform caller that the document has been modified
            RaiseDocumentModified (this);

            // Return success
            return true;
        }

        public override NSObject ContentsForType (string typeName, out NSError outError)
        {
            // Clear the error state
            outError = null;

            // Convert the contents to a NSData object and return it
            NSData docData = _dataModel.Encode(NSStringEncoding.UTF8);
            return docData;
        }
        #endregion

        #region Events
        public delegate void DocumentModifiedDelegate(GenericTextDocument document);
        public event DocumentModifiedDelegate DocumentModified;

        internal void RaiseDocumentModified(GenericTextDocument document) {
            // Inform caller
            if (this.DocumentModified != null) {
                this.DocumentModified (document);
            }
        }
        #endregion
    }
}
```

Die oben dargestellte `GenericTextDocument` Klasse wird in diesem Artikel bei der Arbeit mit der Dokument Auswahl und externen Dokumenten in einer xamarin. IOS 8-Anwendung verwendet.

## <a name="asynchronous-file-coordination"></a>Asynchrone Datei Koordination

IOS 8 bietet über die neuen dateikoordinations-APIs mehrere neue Funktionen für die asynchrone Datei Koordination. Vor IOS 8 waren alle vorhandenen APIs für die Datei Koordination vollständig synchron. Dies bedeutete, dass der Entwickler dafür verantwortlich war, eigene hintergrundschlangen zu implementieren, um zu verhindern, dass die Datei Koordination die Benutzeroberfläche der Anwendung blockiert.

Die neue `NSFileAccessIntent`-Klasse enthält eine URL, die auf die Datei verweist, und mehrere Optionen, um die erforderliche koordinierungsart zu steuern. Der folgende Code veranschaulicht, wie eine Datei mithilfe von Intents von einem Speicherort in einen anderen verschoben wird:

```csharp
// Get source options
var srcURL = NSUrl.FromFilename ("FromFile.txt");
var srcIntent = NSFileAccessIntent.CreateReadingIntent (srcURL, NSFileCoordinatorReadingOptions.ForUploading);

// Get destination options
var dstURL = NSUrl.FromFilename ("ToFile.txt");
var dstIntent = NSFileAccessIntent.CreateReadingIntent (dstURL, NSFileCoordinatorReadingOptions.ForUploading);

// Create an array
var intents = new NSFileAccessIntent[] {
    srcIntent,
    dstIntent
};

// Initialize a file coordination with intents
var queue = new NSOperationQueue ();
var fileCoordinator = new NSFileCoordinator ();
fileCoordinator.CoordinateAccess (intents, queue, (err) => {
    // Was there an error?
    if (err!=null) {
        Console.WriteLine("Error: {0}",err.LocalizedDescription);
    }
});
```

## <a name="discovering-and-listing-documents"></a>Entdecken und Auflisten von Dokumenten

Die Möglichkeit zum Ermitteln und Auflisten von Dokumenten besteht darin, die vorhandenen `NSMetadataQuery`-APIs zu verwenden. In diesem Abschnitt werden neue Features behandelt, die `NSMetadataQuery` hinzugefügt wurden, die das Arbeiten mit Dokumenten noch einfacher machen als bisher.

### <a name="existing-behavior"></a>Vorhandenes Verhalten

Vor IOS 8 waren `NSMetadataQuery` bei der Abholung von lokalen Dateiänderungen langsam, wie z. b. löschen, erstellen und umbenennen.

 [![](document-picker-images/image2.png "NSMetadataQuery local file changes overview")](document-picker-images/image2.png#lightbox)

Im obigen Diagramm:

1. Für Dateien, die bereits im Anwendungs Container vorhanden sind, `NSMetadataQuery` bereits vorhandene `NSMetadata` Datensätze vorab erstellt und spoolbar, sodass Sie für die Anwendung sofort verfügbar sind.
1. Die Anwendung erstellt eine neue Datei im Anwendungs Container.
1. Es gibt eine Verzögerung, bevor `NSMetadataQuery` die Änderung am Anwendungs Container sieht und den erforderlichen `NSMetadata` Datensatz erstellt.

Aufgrund der Verzögerung bei der Erstellung des `NSMetadata` Datensatzes mussten für die Anwendung zwei Datenquellen geöffnet sein: eine für lokale Dateiänderungen und eine für cloudbasierte Änderungen.

### <a name="stitching"></a>Ftungen

In ios 8 ist `NSMetadataQuery` direkt mit einem neuen Feature namens "Nähte" zu verwenden:

 [![](document-picker-images/image3.png "NSMetadataQuery with a new feature called Stitching")](document-picker-images/image3.png#lightbox)

Verwenden der zusammen Fügung im obigen Diagramm:

1. Wie zuvor ist für Dateien, die bereits im Anwendungs Container vorhanden sind, `NSMetadataQuery` vorhandene `NSMetadata` Datensätze vorab erstellt und Spooling.
1. Die Anwendung erstellt eine neue Datei im Anwendungs Container mithilfe der Datei Koordination.
1. Ein Hook im Anwendungs Container sieht die Änderung und ruft `NSMetadataQuery` auf, um den erforderlichen `NSMetadata` Datensatz zu erstellen.
1. Der `NSMetadata` Datensatz wird direkt nach der Datei erstellt und für die Anwendung zur Verfügung gestellt.

Durch die Verwendung von Nähten muss die Anwendung nicht mehr eine Datenquelle öffnen, um lokale und cloudbasierte Dateiänderungen zu überwachen. Nun kann die Anwendung direkt auf `NSMetadataQuery` basieren.

> [!IMPORTANT]
> Das Zusammenfügen funktioniert nur, wenn die Anwendung die Datei Koordination verwendet, wie im obigen Abschnitt gezeigt. Wenn die Datei Koordination nicht verwendet wird, wird für die APIs standardmäßig das vorhandene Pre IOS 8-Verhalten verwendet.

### <a name="new-ios-8-metadata-features"></a>Neue IOS 8-Metadatenfeatures

Die folgenden neuen Features wurden `NSMetadataQuery` in ios 8 hinzugefügt:

- `NSMetatadataQuery` können jetzt nicht lokale Dokumente auflisten, die in der Cloud gespeichert sind.
- Neue APIs wurden hinzugefügt, um auf Metadateninformationen in den cloudbasierten Dokumenten zuzugreifen. 
- Es gibt eine neue `NSUrl_PromisedItems`-API, mit der auf die Dateiattribute von Dateien zugegriffen werden kann, deren Inhalt lokal verfügbar sein kann.
- Verwenden Sie die `GetPromisedItemResourceValue`-Methode, um Informationen über eine bestimmte Datei zu erhalten, oder verwenden Sie die `GetPromisedItemResourceValues`-Methode, um Informationen zu mehr als einer Datei gleichzeitig zu erhalten.

Für den Umgang mit Metadaten wurden zwei neue dateikoordinations-Flags hinzugefügt:

- `NSFileCoordinatorReadImmediatelyAvailableMetadataOnly` 
- `NSFileCoordinatorWriteContentIndependentMetadataOnly` 

Mit den obigen Flags muss der Inhalt der Dokument Datei nicht lokal verfügbar sein, damit Sie verwendet werden kann.

Das folgende Codesegment zeigt, wie `NSMetadataQuery` verwendet wird, um das vorhanden sein einer bestimmten Datei abzufragen und die Datei zu erstellen, wenn Sie nicht vorhanden ist:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading;
using Foundation;
using UIKit;
using ObjCRuntime;
using System.IO;

#region Static Properties
public const string TestFilename = "test.txt"; 
#endregion

#region Computed Properties
public bool HasiCloud { get; set; }
public bool CheckingForiCloud { get; set; }
public NSUrl iCloudUrl { get; set; }

public GenericTextDocument Document { get; set; }
public NSMetadataQuery Query { get; set; }
#endregion

#region Private Methods
private void FindDocument () {
    Console.WriteLine ("Finding Document...");

    // Create a new query and set it's scope
    Query = new NSMetadataQuery();
    Query.SearchScopes = new NSObject [] {
                NSMetadataQuery.UbiquitousDocumentsScope,
                NSMetadataQuery.UbiquitousDataScope,
                NSMetadataQuery.AccessibleUbiquitousExternalDocumentsScope
            };

    // Build a predicate to locate the file by name and attach it to the query
    var pred = NSPredicate.FromFormat ("%K == %@"
        , new NSObject[] {
            NSMetadataQuery.ItemFSNameKey
            , new NSString(TestFilename)});
    Query.Predicate = pred;

    // Register a notification for when the query returns
    NSNotificationCenter.DefaultCenter.AddObserver (this,
            new Selector("queryDidFinishGathering:"),             NSMetadataQuery.DidFinishGatheringNotification,
            Query);

    // Start looking for the file
    Query.StartQuery ();
    Console.WriteLine ("Querying: {0}", Query.IsGathering);
}

[Export("queryDidFinishGathering:")]
public void DidFinishGathering (NSNotification notification) {
    Console.WriteLine ("Finish Gathering Documents.");

    // Access the query and stop it from running
    var query = (NSMetadataQuery)notification.Object;
    query.DisableUpdates();
    query.StopQuery();

    // Release the notification
    NSNotificationCenter.DefaultCenter.RemoveObserver (this
        , NSMetadataQuery.DidFinishGatheringNotification
        , query);

    // Load the document that the query returned
    LoadDocument(query);
}

private void LoadDocument (NSMetadataQuery query) {
    Console.WriteLine ("Loading Document...");    

    // Take action based on the returned record count
    switch (query.ResultCount) {
    case 0:
        // Create a new document
        CreateNewDocument ();
        break;
    case 1:
        // Gain access to the url and create a new document from
        // that instance
        NSMetadataItem item = (NSMetadataItem)query.ResultAtIndex (0);
        var url = (NSUrl)item.ValueForAttribute (NSMetadataQuery.ItemURLKey);

        // Load the document
        OpenDocument (url);
        break;
    default:
        // There has been an issue
        Console.WriteLine ("Issue: More than one document found...");
        break;
    }
}
#endregion

#region Public Methods
public void OpenDocument(NSUrl url) {

    Console.WriteLine ("Attempting to open: {0}", url);
    Document = new GenericTextDocument (url);

    // Open the document
    Document.Open ( (success) => {
        if (success) {
            Console.WriteLine ("Document Opened");
        } else
            Console.WriteLine ("Failed to Open Document");
    });

    // Inform caller
    RaiseDocumentLoaded (Document);
}

public void CreateNewDocument() {
    // Create path to new file
    // var docsFolder = Environment.GetFolderPath (Environment.SpecialFolder.Personal);
    var docsFolder = Path.Combine(iCloudUrl.Path, "Documents");
    var docPath = Path.Combine (docsFolder, TestFilename);
    var ubiq = new NSUrl (docPath, false);

    // Create new document at path 
    Console.WriteLine ("Creating Document at:" + ubiq.AbsoluteString);
    Document = new GenericTextDocument (ubiq);

    // Set the default value
    Document.Contents = "(default value)";

    // Save document to path
    Document.Save (Document.FileUrl, UIDocumentSaveOperation.ForCreating, (saveSuccess) => {
        Console.WriteLine ("Save completion:" + saveSuccess);
        if (saveSuccess) {
            Console.WriteLine ("Document Saved");
        } else {
            Console.WriteLine ("Unable to Save Document");
        }
    });

    // Inform caller
    RaiseDocumentLoaded (Document);
}

public bool SaveDocument() {
    bool successful = false;

    // Save document to path
    Document.Save (Document.FileUrl, UIDocumentSaveOperation.ForOverwriting, (saveSuccess) => {
        Console.WriteLine ("Save completion: " + saveSuccess);
        if (saveSuccess) {
            Console.WriteLine ("Document Saved");
            successful = true;
        } else {
            Console.WriteLine ("Unable to Save Document");
            successful=false;
        }
    });

    // Return results
    return successful;
}
#endregion

#region Events
public delegate void DocumentLoadedDelegate(GenericTextDocument document);
public event DocumentLoadedDelegate DocumentLoaded;

internal void RaiseDocumentLoaded(GenericTextDocument document) {
    // Inform caller
    if (this.DocumentLoaded != null) {
        this.DocumentLoaded (document);
    }
}
#endregion
```

### <a name="document-thumbnails"></a>Dokument Miniaturansichten

Apple ist der Meinung, dass die Verwendung von Vorschauen bei der Auflistung von Dokumenten für eine Anwendung die beste Benutzer Leistung ist. Dies ermöglicht den Endbenutzern den Kontext, damit Sie das Dokument, mit dem Sie arbeiten möchten, schnell identifizieren können.

Vor IOS 8 erforderte die Anzeige von Dokument Vorschauen eine benutzerdefinierte Implementierung. Neu in ios 8 sind Dateisystem Attribute, die es dem Entwickler ermöglichen, schnell mit Dokument Miniaturansichten zu arbeiten.

#### <a name="retrieving-document-thumbnails"></a>Abrufen von Dokument Miniaturansichten 

Durch Aufrufen der Methoden `GetPromisedItemResourceValue` oder `GetPromisedItemResourceValues` wird `NSUrl_PromisedItems` API ein `NSUrlThumbnailDictionary`zurückgegeben. Der einzige Schlüssel, der derzeit in diesem Wörterbuch vorhanden ist, ist der `NSThumbnial1024X1024SizeKey` und der entsprechende `UIImage`.

#### <a name="saving-document-thumbnails"></a>Speichern von Dokument Miniaturansichten

Die einfachste Möglichkeit, eine Miniaturansicht zu speichern, ist die Verwendung `UIDocument`. Wenn Sie die `GetFileAttributesToWrite`-Methode der `UIDocument` aufrufen und die Miniaturansicht festlegen, wird Sie automatisch gespeichert, wenn die Dokument Datei ist. Der icloud-Daemon wird diese Änderung sehen und an icloud weitergeben. Auf Mac OS X werden Miniaturansichten für den Entwickler automatisch durch das Quick Look-Plug-in generiert.

Mit den Grundlagen der Arbeit mit icloud-basierten Dokumenten und den Änderungen an der vorhandenen API können wir den Dokument Auswahl-Ansichts Controller in einer mobilen xamarin IOS 8-Anwendung implementieren.

## <a name="enabling-icloud-in-xamarin"></a>Aktivieren von icloud in xamarin

Bevor die Dokument Auswahl in einer xamarin. IOS-Anwendung verwendet werden kann, muss der icloud-Support sowohl in Ihrer Anwendung als auch über Apple aktiviert werden. 

In den folgenden Schritten wird der Bereitstellungs Prozess für icloud beschrieben.

1. Erstellen Sie einen icloud-Container.
2. Erstellen Sie eine APP-ID, die die icloud-App Service enthält.
3. Erstellen Sie ein Bereitstellungs Profil, das diese APP-ID enthält.

Der Leitfaden [Arbeiten mit Funktionen](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md) führt Sie durch die ersten beiden Schritte. Um ein Bereitstellungs Profil zu erstellen, führen Sie die Schritte im Handbuch für den [Bereitstellungs Profil](~/ios/get-started/installation/device-provisioning/index.md#provisioning-your-device) aus.

In den folgenden Schritten wird beschrieben, wie Sie Ihre Anwendung für icloud konfigurieren:

Führen Sie folgende Schritte aus:

1. Öffnen Sie das Projekt in Visual Studio für Mac oder Visual Studio.
2. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie Optionen aus.
3. Wählen Sie im Dialog Feld Optionen die Option **IOS-Anwendung**aus, und vergewissern Sie sich, dass die **Bündel** -ID mit der ID übereinstimmt, die in der oben erstellten **App-ID** für die 
4. Wählen Sie **IOS-Bündel Signierung**aus, und wählen Sie die **Entwickler Identität** und das oben erstellte **Bereitstellungs Profil** aus.
5. Klicken Sie auf die Schaltfläche **OK** , um die Änderungen zu speichern und das Dialogfeld zu schließen.
6. Klicken Sie mit der rechten Maustaste auf `Entitlements.plist` im **Projektmappen-Explorer** , um Sie im Editor zu öffnen.

    > [!IMPORTANT]
    > In Visual Studio müssen Sie möglicherweise den Berechtigungs-Editor öffnen, indem Sie mit der rechten Maustaste darauf klicken und dann **Öffnen mit..** . auswählen. und auswählen des Eigenschaften Listen-Editors

7. **Aktivieren Sie icloud** , **icloud Documents** , **Key-Value Storage** und **cloudkit** .
8. Stellen Sie sicher, dass der **Container** für die Anwendung vorhanden ist (wie oben erstellt). Ein Beispiel: `iCloud.com.your-company.AppName`
9. Speichern Sie die Änderungen in der Datei.

Weitere Informationen zu Berechtigungen finden Sie im Handbuch [Arbeiten mit Berechtigungen](~/ios/deploy-test/provisioning/entitlements.md) .

Wenn das oben beschriebene Setup vorhanden ist, kann die Anwendung jetzt cloudbasierte Dokumente und den neuen Dokument Auswahl-Ansichts Controller verwenden.

## <a name="common-setup-code"></a>Allgemeiner Setup Code

Bevor Sie mit dem Ansichts Controller für die Dokument Auswahl beginnen, ist ein standardmäßiger Setup Code erforderlich. Ändern Sie zunächst die `AppDelegate.cs` Datei der Anwendung, und führen Sie Sie wie folgt aus:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading;
using Foundation;
using UIKit;
using ObjCRuntime;
using System.IO;

namespace DocPicker
{

    [Register ("AppDelegate")]
    public partial class AppDelegate : UIApplicationDelegate
    {
        #region Static Properties
        public const string TestFilename = "test.txt"; 
        #endregion

        #region Computed Properties
        public override UIWindow Window { get; set; }
        public bool HasiCloud { get; set; }
        public bool CheckingForiCloud { get; set; }
        public NSUrl iCloudUrl { get; set; }

        public GenericTextDocument Document { get; set; }
        public NSMetadataQuery Query { get; set; }
        public NSData Bookmark { get; set; }
        #endregion

        #region Private Methods
        private void FindDocument () {
            Console.WriteLine ("Finding Document...");

            // Create a new query and set it's scope
            Query = new NSMetadataQuery();
            Query.SearchScopes = new NSObject [] {
                NSMetadataQuery.UbiquitousDocumentsScope,
                NSMetadataQuery.UbiquitousDataScope,
                NSMetadataQuery.AccessibleUbiquitousExternalDocumentsScope
            };

            // Build a predicate to locate the file by name and attach it to the query
            var pred = NSPredicate.FromFormat ("%K == %@",
                 new NSObject[] {NSMetadataQuery.ItemFSNameKey
                , new NSString(TestFilename)});
            Query.Predicate = pred;

            // Register a notification for when the query returns
            NSNotificationCenter.DefaultCenter.AddObserver (this
                , new Selector("queryDidFinishGathering:")
                , NSMetadataQuery.DidFinishGatheringNotification
                , Query);

            // Start looking for the file
            Query.StartQuery ();
            Console.WriteLine ("Querying: {0}", Query.IsGathering);
        }

        [Export("queryDidFinishGathering:")]
        public void DidFinishGathering (NSNotification notification) {
            Console.WriteLine ("Finish Gathering Documents.");

            // Access the query and stop it from running
            var query = (NSMetadataQuery)notification.Object;
            query.DisableUpdates();
            query.StopQuery();

            // Release the notification
            NSNotificationCenter.DefaultCenter.RemoveObserver (this
                , NSMetadataQuery.DidFinishGatheringNotification
                , query);

            // Load the document that the query returned
            LoadDocument(query);
        }

        private void LoadDocument (NSMetadataQuery query) {
            Console.WriteLine ("Loading Document...");    

            // Take action based on the returned record count
            switch (query.ResultCount) {
            case 0:
                // Create a new document
                CreateNewDocument ();
                break;
            case 1:
                // Gain access to the url and create a new document from
                // that instance
                NSMetadataItem item = (NSMetadataItem)query.ResultAtIndex (0);
                var url = (NSUrl)item.ValueForAttribute (NSMetadataQuery.ItemURLKey);

                // Load the document
                OpenDocument (url);
                break;
            default:
                // There has been an issue
                Console.WriteLine ("Issue: More than one document found...");
                break;
            }
        }
        #endregion

        #region Public Methods

        public void OpenDocument(NSUrl url) {

            Console.WriteLine ("Attempting to open: {0}", url);
            Document = new GenericTextDocument (url);

            // Open the document
            Document.Open ( (success) => {
                if (success) {
                    Console.WriteLine ("Document Opened");
                } else
                    Console.WriteLine ("Failed to Open Document");
            });

            // Inform caller
            RaiseDocumentLoaded (Document);
        }

        public void CreateNewDocument() {
            // Create path to new file
            // var docsFolder = Environment.GetFolderPath (Environment.SpecialFolder.Personal);
            var docsFolder = Path.Combine(iCloudUrl.Path, "Documents");
            var docPath = Path.Combine (docsFolder, TestFilename);
            var ubiq = new NSUrl (docPath, false);

            // Create new document at path 
            Console.WriteLine ("Creating Document at:" + ubiq.AbsoluteString);
            Document = new GenericTextDocument (ubiq);

            // Set the default value
            Document.Contents = "(default value)";

            // Save document to path
            Document.Save (Document.FileUrl, UIDocumentSaveOperation.ForCreating, (saveSuccess) => {
                Console.WriteLine ("Save completion:" + saveSuccess);
                if (saveSuccess) {
                    Console.WriteLine ("Document Saved");
                } else {
                    Console.WriteLine ("Unable to Save Document");
                }
            });

            // Inform caller
            RaiseDocumentLoaded (Document);
        }

        /// <summary>
        /// Saves the document.
        /// </summary>
        /// <returns><c>true</c>, if document was saved, <c>false</c> otherwise.</returns>
        public bool SaveDocument() {
            bool successful = false;

            // Save document to path
            Document.Save (Document.FileUrl, UIDocumentSaveOperation.ForOverwriting, (saveSuccess) => {
                Console.WriteLine ("Save completion: " + saveSuccess);
                if (saveSuccess) {
                    Console.WriteLine ("Document Saved");
                    successful = true;
                } else {
                    Console.WriteLine ("Unable to Save Document");
                    successful=false;
                }
            });

            // Return results
            return successful;
        }
        #endregion

        #region Override Methods
        public override void FinishedLaunching (UIApplication application)
        {

            // Start a new thread to check and see if the user has iCloud
            // enabled.
            new Thread(new ThreadStart(() => {
                // Inform caller that we are checking for iCloud
                CheckingForiCloud = true;

                // Checks to see if the user of this device has iCloud
                // enabled
                var uburl = NSFileManager.DefaultManager.GetUrlForUbiquityContainer(null);

                // Connected to iCloud?
                if (uburl == null)
                {
                    // No, inform caller
                    HasiCloud = false;
                    iCloudUrl =null;
                    Console.WriteLine("Unable to connect to iCloud");
                    InvokeOnMainThread(()=>{
                        var okAlertController = UIAlertController.Create ("iCloud Not Available", "Developer, please check your Entitlements.plist, Bundle ID and Provisioning Profiles.", UIAlertControllerStyle.Alert);
                        okAlertController.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
                        Window.RootViewController.PresentViewController (okAlertController, true, null);
                    });
                }
                else
                {    
                    // Yes, inform caller and save location the Application Container
                    HasiCloud = true;
                    iCloudUrl = uburl;
                    Console.WriteLine("Connected to iCloud");

                    // If we have made the connection with iCloud, start looking for documents
                    InvokeOnMainThread(()=>{
                        // Search for the default document
                        FindDocument ();
                    });
                }

                // Inform caller that we are no longer looking for iCloud
                CheckingForiCloud = false;

            })).Start();
                
        }
        
        // This method is invoked when the application is about to move from active to inactive state.
        // OpenGL applications should use this method to pause.
        public override void OnResignActivation (UIApplication application)
        {
        }
        
        // This method should be used to release shared resources and it should store the application state.
        // If your application supports background execution this method is called instead of WillTerminate
        // when the user quits.
        public override void DidEnterBackground (UIApplication application)
        {
            // Trap all errors
            try {
                // Values to include in the bookmark packet
                var resources = new string[] {
                    NSUrl.FileSecurityKey,
                    NSUrl.ContentModificationDateKey,
                    NSUrl.FileResourceIdentifierKey,
                    NSUrl.FileResourceTypeKey,
                    NSUrl.LocalizedNameKey
                };

                // Create the bookmark
                NSError err;
                Bookmark = Document.FileUrl.CreateBookmarkData (NSUrlBookmarkCreationOptions.WithSecurityScope, resources, iCloudUrl, out err);

                // Was there an error?
                if (err != null) {
                    // Yes, report it
                    Console.WriteLine ("Error Creating Bookmark: {0}", err.LocalizedDescription);
                }
            }
            catch (Exception e) {
                // Report error
                Console.WriteLine ("Error: {0}", e.Message);
            }
        }
        
        // This method is called as part of the transition from background to active state.
        public override void WillEnterForeground (UIApplication application)
        {
            // Is there any bookmark data?
            if (Bookmark != null) {
                // Trap all errors
                try {
                    // Yes, attempt to restore it
                    bool isBookmarkStale;
                    NSError err;
                    var srcUrl = new NSUrl (Bookmark, NSUrlBookmarkResolutionOptions.WithSecurityScope, iCloudUrl, out isBookmarkStale, out err);

                    // Was there an error?
                    if (err != null) {
                        // Yes, report it
                        Console.WriteLine ("Error Loading Bookmark: {0}", err.LocalizedDescription);
                    } else {
                        // Load document from bookmark
                        OpenDocument (srcUrl);
                    }
                }
                catch (Exception e) {
                    // Report error
                    Console.WriteLine ("Error: {0}", e.Message);
                }
            }

        }
        
        // This method is called when the application is about to terminate. Save data, if needed.
        public override void WillTerminate (UIApplication application)
        {
        }
        #endregion

        #region Events
        public delegate void DocumentLoadedDelegate(GenericTextDocument document);
        public event DocumentLoadedDelegate DocumentLoaded;

        internal void RaiseDocumentLoaded(GenericTextDocument document) {
            // Inform caller
            if (this.DocumentLoaded != null) {
                this.DocumentLoaded (document);
            }
        }
        #endregion
    }
}

```

> [!IMPORTANT]
> Der obige Code enthält den Code aus dem obigen Abschnitt "ermitteln und Auflisten von Dokumenten". Sie wird hier vollständig dargestellt, wie Sie in einer eigentlichen Anwendung angezeigt wird. Der Einfachheit halber funktioniert dieses Beispiel nur mit einer einzelnen, hart codierten Datei (`test.txt`).

Der obige Code macht mehrere icloud-Laufwerk Verknüpfungen verfügbar, damit Sie im Rest der Anwendung einfacher arbeiten können.

Fügen Sie als nächstes den folgenden Code zu einem Sicht-oder Ansichts Container hinzu, der die Dokument Auswahl verwendet oder mit cloudbasierten Dokumenten verwendet werden soll:

```csharp
using CloudKit;
...

#region Computed Properties
/// <summary>
/// Returns the delegate of the current running application
/// </summary>
/// <value>The this app.</value>
public AppDelegate ThisApp {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
#endregion
```

Dadurch wird eine Verknüpfung hinzugefügt, um zum `AppDelegate` zu gelangen und auf die oben erstellten icloud-Verknüpfungen zuzugreifen.

Wenn dieser Code vorhanden ist, sehen wir uns die Implementierung des Ansichts Controllers für die Dokument Auswahl in einer xamarin IOS 8-Anwendung an.

## <a name="using-the-document-picker-view-controller"></a>Verwenden des Ansichts Controllers für die Dokument Auswahl

Vor IOS 8 war es sehr schwierig, auf Dokumente aus einer anderen Anwendung zuzugreifen, da es keine Möglichkeit gab, Dokumente außerhalb der Anwendung innerhalb der APP zu ermitteln.

### <a name="existing-behavior"></a>Vorhandenes Verhalten

 [![](document-picker-images/image31.png "Existing Behavior overview")](document-picker-images/image31.png#lightbox)

Werfen wir einen Blick auf den Zugriff auf ein externes Dokument vor IOS 8:

1. Zuerst muss der Benutzer die Anwendung öffnen, die das Dokument ursprünglich erstellt hat.
1. Das Dokument ist ausgewählt, und der `UIDocumentInteractionController` wird zum Senden des Dokuments an die neue Anwendung verwendet.
1. Schließlich wird eine Kopie des ursprünglichen Dokuments in den Container der neuen Anwendung eingefügt.

Von dort aus ist das Dokument verfügbar, damit die zweite Anwendung geöffnet und bearbeitet werden kann.

### <a name="discovering-documents-outside-of-an-apps-container"></a>Entdecken von Dokumenten außerhalb eines App-Containers

In ios 8 kann eine Anwendung problemlos auf Dokumente außerhalb eines eigenen Anwendungs Containers zugreifen:

 [![](document-picker-images/image32.png "Discovering Documents Outside of an App's Container")](document-picker-images/image32.png#lightbox)

Mithilfe der neuen icloud-Dokument Auswahl (`UIDocumentPickerViewController`) kann eine IOS-Anwendung außerhalb Ihres Anwendungs Containers direkt ermitteln und darauf zugreifen. Der `UIDocumentPickerViewController` stellt einen Mechanismus bereit, mit dem der Benutzer Zugriff auf diese ermittelten Dokumente über Berechtigungen gewähren und bearbeiten können.

Eine Anwendung muss abonnieren, damit Ihre Dokumente in der icloud-Dokument Auswahl angezeigt werden und für andere Anwendungen verfügbar sind, um Sie zu ermitteln und damit zu arbeiten. Wenn eine xamarin IOS 8-Anwendung ihren Anwendungs Container freigeben soll, bearbeiten Sie Sie `Info.plist` Datei in einem Standardtext-Editor, und fügen Sie am Ende des Wörterbuchs (zwischen den `<dict>...</dict>` Tags) die folgenden beiden Zeilen hinzu:

```xml
<key>NSUbiquitousContainerIsDocumentScopePublic</key>
<true/>
```

Der `UIDocumentPickerViewController` bietet eine hervorragend neue Benutzeroberfläche, die es dem Benutzer ermöglicht, Dokumente auszuwählen. Gehen Sie folgendermaßen vor, um den Dokument Auswahl-Ansichts Controller in einer xamarin IOS 8-Anwendung anzuzeigen:

```csharp
using MobileCoreServices;
...

// Allow the Document picker to select a range of document types
        var allowedUTIs = new string[] {
            UTType.UTF8PlainText,
            UTType.PlainText,
            UTType.RTF,
            UTType.PNG,
            UTType.Text,
            UTType.PDF,
            UTType.Image
        };

        // Display the picker
        //var picker = new UIDocumentPickerViewController (allowedUTIs, UIDocumentPickerMode.Open);
        var pickerMenu = new UIDocumentMenuViewController(allowedUTIs, UIDocumentPickerMode.Open);
        pickerMenu.DidPickDocumentPicker += (sender, args) => {

            // Wireup Document Picker
            args.DocumentPicker.DidPickDocument += (sndr, pArgs) => {

                // IMPORTANT! You must lock the security scope before you can
                // access this file
                var securityEnabled = pArgs.Url.StartAccessingSecurityScopedResource();

                // Open the document
                ThisApp.OpenDocument(pArgs.Url);

                // IMPORTANT! You must release the security lock established
                // above.
                pArgs.Url.StopAccessingSecurityScopedResource();
            };

            // Display the document picker
            PresentViewController(args.DocumentPicker,true,null);
        };

pickerMenu.ModalPresentationStyle = UIModalPresentationStyle.Popover;
PresentViewController(pickerMenu,true,null);
UIPopoverPresentationController presentationPopover = pickerMenu.PopoverPresentationController;
if (presentationPopover!=null) {
    presentationPopover.SourceView = this.View;
    presentationPopover.PermittedArrowDirections = UIPopoverArrowDirection.Down;
    presentationPopover.SourceRect = ((UIButton)s).Frame;
}
```

> [!IMPORTANT]
> Der Entwickler muss die `StartAccessingSecurityScopedResource`-Methode des `NSUrl` aufrufen, bevor auf ein externes Dokument zugegriffen werden kann. Die `StopAccessingSecurityScopedResource`-Methode muss aufgerufen werden, um die Sicherheitssperre freizugeben, sobald das Dokument geladen wurde.

### <a name="sample-output"></a>Beispielausgabe

Im folgenden finden Sie ein Beispiel dafür, wie der obige Code eine Dokument Auswahl bei der Verwendung auf einem iPhone-Gerät anzeigt:

1. Der Benutzer startet die Anwendung, und die Hauptschnittstelle wird angezeigt:   

    [![](document-picker-images/image33.png "The main interface is displayed")](document-picker-images/image33.png#lightbox)
1. Der Benutzer tippt oben auf dem Bildschirm auf die Schaltfläche **Aktion** und wird aufgefordert, einen **Dokument Anbieter** aus der Liste der verfügbaren Anbieter auszuwählen:   

    [![](document-picker-images/image34.png "Select a Document Provider from the list of available providers")](document-picker-images/image34.png#lightbox)
1. Der **Ansichts Controller** für die Dokument Auswahl wird für den ausgewählten **Dokument Anbieter**angezeigt:   

    [![](document-picker-images/image35.png "The Document Picker View Controller is displayed")](document-picker-images/image35.png#lightbox)
1. Der Benutzer tippt auf einen **Dokument Ordner** , um seinen Inhalt anzuzeigen:   

    [![](document-picker-images/image36.png "The Document Folder contents")](document-picker-images/image36.png#lightbox)
1. Der Benutzer wählt ein **Dokument** aus, und die **Dokument** Auswahl ist geschlossen.
1. Die Hauptschnittstelle wird erneut angezeigt, das **Dokument** wird aus dem externen Container geladen und der Inhalt wird angezeigt.

Die tatsächliche Anzeige des Ansichts Controllers für die Dokument Auswahl hängt von den Dokument Anbietern ab, die der Benutzer auf dem Gerät installiert hat und welchem Dokument Auswahlmodus implementiert wurde. Im obigen Beispiel wird der Modus "Open" verwendet, die anderen Modustypen werden im folgenden ausführlich erläutert.

## <a name="managing-external-documents"></a>Verwalten externer Dokumente

Wie bereits erwähnt, konnte eine Anwendung vor IOS 8 nur auf Dokumente zugreifen, die Bestandteil des Anwendungs Containers waren. In ios 8 kann eine Anwendung auf Dokumente aus externen Quellen zugreifen:

 [![](document-picker-images/image37.png "Managing External Documents overview")](document-picker-images/image37.png#lightbox)

Wenn der Benutzer ein Dokument aus einer externen Quelle auswählt, wird ein Referenzdokument in den Anwendungs Container geschrieben, das auf das ursprüngliche Dokument verweist.

Zur Unterstützung beim Hinzufügen dieser neuen Funktionalität in vorhandene Anwendungen wurden der `NSMetadataQuery`-API mehrere neue Features hinzugefügt. In der Regel verwendet eine Anwendung den universellen Dokumentbereich, um Dokumente aufzulisten, die in Ihrem Anwendungs Container Leben. Wenn Sie diesen Bereich verwenden, werden nur Dokumente innerhalb des Anwendungs Containers weiterhin angezeigt.

Die Verwendung des neuen universellen externen Dokument Bereichs gibt Dokumente zurück, die sich außerhalb des Anwendungs Containers befinden und die Metadaten für Sie zurückgeben. Die `NSMetadataItemUrlKey` zeigt auf die URL, auf der sich das Dokument tatsächlich befindet.

Manchmal möchte eine Anwendung nicht mit den Dokumenten arbeiten, auf die durch den Verweis verwiesen wird. Stattdessen möchte die APP direkt mit dem Verweis Dokument arbeiten. Beispielsweise kann es sein, dass die APP das Dokument im Anwendungsordner in der Benutzeroberfläche anzeigen kann, oder dass der Benutzer die Verweise innerhalb eines Ordners verschieben kann.

In ios 8 wurde eine neue `NSMetadataItemUrlInLocalContainerKey` bereitgestellt, um direkt auf das Referenzdokument zuzugreifen. Dieser Schlüssel verweist auf den tatsächlichen Verweis auf das externe Dokument in einem Anwendungs Container.

Der `NSMetadataUbiquitousItemIsExternalDocumentKey` wird verwendet, um zu testen, ob ein Dokument außerhalb eines Anwendungs Containers steht. Der-`NSMetadataUbiquitousItemContainerDisplayNameKey` wird verwendet, um auf den Namen des Containers zuzugreifen, der die ursprüngliche Kopie eines externen Dokuments verwendet.

### <a name="why-document-references-are-required"></a>Warum Dokument Verweise erforderlich sind

Der Hauptgrund dafür, dass IOS 8 Verweise für den Zugriff auf externe Dokumente verwendet, ist Sicherheit. Keine Anwendung erhält Zugriff auf den Container einer anderen Anwendung. Dies ist nur für die Dokument Auswahl möglich, da außerhalb des Prozesses ausgeführt wird und über systemweiten Zugriff verfügt.

Die einzige Möglichkeit, auf ein Dokument außerhalb des Anwendungs Containers zu gelangen, ist die Verwendung der Dokument Auswahl und, wenn die von der Auswahl zurückgegebene URL Sicherheits bezogen ist. Die Sicherheitsbereichs bezogene URL enthält genau genug Informationen, um das Dokument zusammen mit den Bereichs bezogenen rechten auszuwählen, die erforderlich sind, um einer Anwendung den Zugriff auf das Dokument zu gewähren.

Beachten Sie Folgendes: Wenn die Sicherheitsbereichs bezogene URL in eine Zeichenfolge serialisiert und anschließend deserialisiert wurde, gehen die Sicherheitsinformationen verloren, und die Datei ist nicht mehr über die URL zugänglich. Das Dokument Verweis Feature bietet einen Mechanismus zum zurückkehren zu den Dateien, auf die von diesen URLs verwiesen wird.

Wenn die Anwendung also eine `NSUrl` von einem der Referenzdokumente erhält, ist Ihr der Sicherheitsbereich bereits angefügt und kann für den Zugriff auf die Datei verwendet werden. Aus diesem Grund wird dringend empfohlen, dass der Entwickler `UIDocument` verwendet, da er alle diese Informationen und Prozesse für ihn verarbeitet.

### <a name="using-bookmarks"></a>Verwenden von Lesezeichen

Es ist nicht immer möglich, die Dokumente einer Anwendung aufzählen, um zu einem bestimmten Dokument zurückzukehren, z. b. bei der Zustands Wiederherstellung. IOS 8 bietet einen Mechanismus zum Erstellen von Lesezeichen, die ein bestimmtes Dokument direkt als Ziel haben.

Mit dem folgenden Code wird ein Lesezeichen aus der `FileUrl`-Eigenschaft eines `UIDocument`erstellt:

```csharp
// Trap all errors
try {
    // Values to include in the bookmark packet
    var resources = new string[] {
        NSUrl.FileSecurityKey,
        NSUrl.ContentModificationDateKey,
        NSUrl.FileResourceIdentifierKey,
        NSUrl.FileResourceTypeKey,
        NSUrl.LocalizedNameKey
    };

    // Create the bookmark
    NSError err;
    Bookmark = Document.FileUrl.CreateBookmarkData (NSUrlBookmarkCreationOptions.WithSecurityScope, resources, iCloudUrl, out err);

    // Was there an error?
    if (err != null) {
        // Yes, report it
        Console.WriteLine ("Error Creating Bookmark: {0}", err.LocalizedDescription);
    }
}
catch (Exception e) {
    // Report error
    Console.WriteLine ("Error: {0}", e.Message);
}
```

Die vorhandene Bookmark-API wird verwendet, um ein Lesezeichen für eine vorhandene `NSUrl` zu erstellen, die gespeichert und geladen werden kann, um den direkten Zugriff auf eine externe Datei zu ermöglichen. Mit dem folgenden Code wird ein Lesezeichen wieder hergestellt, das oben erstellt wurde:

```csharp
if (Bookmark != null) {
    // Trap all errors
    try {
        // Yes, attempt to restore it
        bool isBookmarkStale;
        NSError err;
        var srcUrl = new NSUrl (Bookmark, NSUrlBookmarkResolutionOptions.WithSecurityScope, iCloudUrl, out isBookmarkStale, out err);

        // Was there an error?
        if (err != null) {
            // Yes, report it
            Console.WriteLine ("Error Loading Bookmark: {0}", err.LocalizedDescription);
        } else {
            // Load document from bookmark
            OpenDocument (srcUrl);
        }
    }
    catch (Exception e) {
        // Report error
        Console.WriteLine ("Error: {0}", e.Message);
    }
}
```

## <a name="open-vs-import-mode-and-the-document-picker"></a>Öffnen Sie vs. Import Modus und Dokument Auswahl

Der Ansichts Controller für die Dokument Auswahl verfügt über zwei unterschiedliche Betriebsmodi:

1. **Öffnungs Modus** – in diesem Modus erstellt die Dokument Auswahl im Anwendungs Container ein Lesezeichen mit Sicherheitsbereich, wenn der Benutzer und das externe Dokument auswählt.   

    [![](document-picker-images/image37.png "A Security Scoped Bookmark in the Application Container")](document-picker-images/image37.png#lightbox)
1. **Import Modus** – wenn der Benutzer und das externe Dokument auswählen, erstellt die Dokument Auswahl in diesem Modus kein Lesezeichen. Kopieren Sie die Datei stattdessen an einen temporären Speicherort, und stellen Sie der Anwendung Zugriff auf das Dokument an diesem Speicherort:   

    [![](document-picker-images/image38.png "The Document Picker will copy the file into a Temporary Location and provide the application access to the Document at this location")](document-picker-images/image38.png#lightbox)   
 Wenn die Anwendung aus irgendeinem Grund beendet wird, wird der temporäre Speicherort geleert und die Datei entfernt. Wenn die Anwendung Zugriff auf die Datei behalten muss, sollte Sie eine Kopie erstellen und Sie in Ihrem Anwendungs Container platzieren.

Der Öffnungs Modus ist nützlich, wenn die Anwendung mit einer anderen Anwendung zusammenarbeiten und alle Änderungen, die an diesem Dokument vorgenommen wurden, mit der Anwendung freigeben möchten. Der Import Modus wird verwendet, wenn die Anwendung die Änderungen nicht für ein Dokument mit anderen Anwendungen freigeben möchte.

## <a name="making-a-document-external"></a>Erstellen eines externen Dokuments

Wie bereits erwähnt, hat eine IOS 8-Anwendung keinen Zugriff auf Container außerhalb Ihres eigenen Anwendungs Containers. Die Anwendung kann lokal oder an einem temporären Speicherort in ihren eigenen Container schreiben und dann einen speziellen Dokument Modus verwenden, um das resultierende Dokument außerhalb des Anwendungs Containers an einen vom Benutzer ausgewählten Speicherort zu verschieben.

Gehen Sie folgendermaßen vor, um ein Dokument an einen externen Speicherort zu verschieben:

1. Erstellen Sie zuerst ein neues Dokument an einem lokalen Speicherort oder einem temporären Speicherort.
1. Erstellen Sie eine `NSUrl`, die auf das neue Dokument verweist.
1. Öffnen Sie einen neuen Dokument Auswahl-Ansichts Controller, und übergeben Sie die `NSUrl` mit dem Modus `MoveToService`. 
1. Sobald der Benutzer einen neuen Speicherort auswählt, wird das Dokument von seinem aktuellen Speicherort an den neuen Speicherort verschoben.
1. Ein Referenzdokument wird in den Anwendungs Container der APP geschrieben, damit die erstellte Anwendung weiterhin auf die Datei zugreifen kann.

Der folgende Code kann verwendet werden, um ein Dokument an einen externen Speicherort zu verschieben: `var picker = new UIDocumentPickerViewController (srcURL, UIDocumentPickerMode.MoveToService);`

Das vom oben genannten Prozess zurückgegebene Verweis Dokument ist identisch mit dem vom geöffneten Modus der Dokument Auswahl erstellten Verweis Dokument. Es kann jedoch vorkommen, dass die Anwendung ein Dokument verschieben möchte, ohne einen Verweis darauf zu behalten.

Wenn Sie ein Dokument ohne das Erstellen eines Verweises verschieben möchten, verwenden Sie den `ExportToService`-Modus. Ein Beispiel: `var picker = new UIDocumentPickerViewController (srcURL, UIDocumentPickerMode.ExportToService);`

Wenn Sie den `ExportToService` Modus verwenden, wird das Dokument in den externen Container kopiert, und die vorhandene Kopie wird an Ihrem ursprünglichen Speicherort belassen.

## <a name="document-provider-extensions"></a>Dokument Anbieter Erweiterungen

Mit IOS 8 möchte Apple, dass der Endbenutzer in der Lage ist, auf seine cloudbasierten Dokumente zuzugreifen, unabhängig davon, wo Sie tatsächlich vorhanden sind. Um dieses Ziel zu erreichen, bietet IOS 8 einen neuen Dokument Anbieter-Erweiterungsmechanismus.

### <a name="what-is-a-document-provider-extension"></a>Was ist eine Dokument Anbieter Erweiterung?

Eine Dokument Anbieter Erweiterung ist eine Möglichkeit für einen Entwickler oder einen Drittanbieter, einem Anwendungs Alternativen Dokument Speicher bereitzustellen, auf den genau wie der vorhandene icloud-Speicherort zugegriffen werden kann.

Der Benutzer kann einen dieser alternativen Speicherorte aus der Dokument Auswahl auswählen und genau die gleichen Zugriffs Modi (öffnen, importieren, verschieben oder exportieren) verwenden, um mit Dateien an diesem Speicherort zu arbeiten.

Dies wird mit zwei verschiedenen Erweiterungen implementiert:

- **Dokument Auswahl Erweiterung** – stellt eine `UIViewController` Unterklasse bereit, die eine grafische Benutzeroberfläche für den Benutzer bereitstellt, um ein Dokument aus einem alternativen Speicherort auszuwählen. Diese Unterklasse wird als Teil des Ansichts Controllers für die Dokument Auswahl angezeigt.
- **Dateierweiterung angeben** – Dies ist eine nicht-UI-Erweiterung, die die tatsächliche Bereitstellung der Dateiinhalte behandelt. Diese Erweiterungen werden durch die Datei Koordination (`NSFileCoordinator`) bereitgestellt. Dies ist ein weiterer wichtiger Fall, bei dem die Datei Koordination erforderlich ist.

Das folgende Diagramm zeigt den typischen Datenfluss beim Arbeiten mit Dokument Anbieter Erweiterungen:

 [![](document-picker-images/image39.png "This diagram shows the typical data flow when working with Document Provider Extensions")](document-picker-images/image39.png#lightbox)

Der folgende Prozess tritt auf:

1. Die Anwendung stellt einen Dokument Auswahl Controller dar, mit dem der Benutzer eine Datei auswählen kann, mit der Sie arbeiten möchten.
1. Der Benutzer wählt einen alternativen Datei Speicherort aus, und die benutzerdefinierte `UIViewController` Erweiterung wird aufgerufen, um die Benutzeroberfläche anzuzeigen.
1. Der Benutzer wählt eine Datei von diesem Speicherort aus, und die URL wird an die Dokument Auswahl zurückgegeben.
1. Die Dokument Auswahl wählt die URL der Datei aus und gibt Sie an die Anwendung zurück, an der der Benutzer arbeitet.
1. Die URL wird an den Datei Koordinator übermittelt, um den Inhalt der Dateien an die Anwendung zurückzugeben.
1. Der Datei Koordinator Ruft die Erweiterung des benutzerdefinierten Datei Anbieters auf, um die Datei abzurufen.
1. Der Inhalt der Datei wird an den Datei Koordinator zurückgegeben.
1. Der Inhalt der Datei wird an die Anwendung zurückgegeben.

### <a name="security-and-bookmarks"></a>Sicherheit und Lesezeichen

In diesem Abschnitt wird kurz erläutert, wie die Sicherheit und der permanente Dateizugriff über Lesezeichen mit Dokument Anbieter Erweiterungen funktionieren. Im Gegensatz zum icloud-Dokument Anbieter, der automatisch Sicherheit und Lesezeichen im Anwendungs Container speichert, sind die Dokument Anbieter Erweiterungen nicht, da Sie nicht Teil des Dokument Referenzsystems sind.

Beispiel: in einer Unternehmensumgebung, die einen eigenen unternehmensweiten sicheren Datenspeicher bereitstellt, benötigen Administratoren keine vertraulichen Unternehmensinformationen, auf die die öffentlichen icloud-Server zugreifen oder diese verarbeiten. Daher kann das integrierte Dokument Verweis System nicht verwendet werden.

Das Lesezeichen System kann weiterhin verwendet werden, und es liegt in der Verantwortung der Datei Anbieter Erweiterung, eine mit Lesezeichen markierte URL ordnungsgemäß zu verarbeiten und den Inhalt des Dokuments zurückzugeben, auf das Sie verweist.

Aus Sicherheitsgründen verfügt IOS 8 über eine Isolationsschicht, die die Informationen darüber beibehält, welche Anwendung auf welche ID in welchem Datei Anbieter zugreifen kann. Beachten Sie, dass der gesamte Dateizugriff von dieser Isolationsschicht gesteuert wird.

Das folgende Diagramm zeigt den Datenfluss beim Arbeiten mit Lesezeichen und einer Dokument Anbieter Erweiterung:

 [![](document-picker-images/image40.png "This diagram shows the data flow when working with Bookmarks and a Document Provider Extension")](document-picker-images/image40.png#lightbox)

Der folgende Prozess tritt auf:

1. Die Anwendung ist in der Lage, den Hintergrund einzugeben, und muss ihren Zustand beibehalten. Es wird `NSUrl` aufgerufen, um ein Lesezeichen für eine Datei im alternativen Speicher zu erstellen.
1. `NSUrl` Ruft die Datei Anbieter Erweiterung auf, um eine permanente URL für das Dokument zu erhalten. 
1. Die Datei Anbieter Erweiterung gibt die URL als Zeichenfolge an den `NSUrl` zurück.
1. Der `NSUrl` bündelt die URL in ein Lesezeichen und gibt Sie an die Anwendung zurück.
1. Wenn sich die Anwendung im Hintergrund befindet und den Zustand wiederherstellen muss, übergibt Sie das Lesezeichen an `NSUrl`.
1. `NSUrl` Ruft die Datei Anbieter Erweiterung mit der URL der Datei auf.
1. Der Datei Erweiterungs Anbieter greift auf die Datei zu und gibt den Speicherort der Datei an `NSUrl` zurück.
1. Der Speicherort der Datei wird mit Sicherheitsinformationen gebündelt und an die Anwendung zurückgegeben.

Von hier aus kann die Anwendung auf die Datei zugreifen und Sie wie gewohnt bearbeiten.

### <a name="writing-files"></a>Schreiben von Dateien

In diesem Abschnitt wird kurz erläutert, wie das Schreiben von Dateien an einen alternativen Speicherort mit einer Dokument Anbieter Erweiterung funktioniert. Die IOS-Anwendung verwendet die Datei Koordination, um im Anwendungs Container Informationen auf dem Datenträger zu speichern. Kurz nachdem die Datei erfolgreich geschrieben wurde, wird die Datei Anbieter Erweiterung über die Änderung benachrichtigt.

An diesem Punkt kann die Datei Anbieter Erweiterung mit dem Hochladen der Datei an den alternativen Speicherort beginnen (oder die Datei als geändert markieren und hochladen).

### <a name="creating-new-document-provider-extensions"></a>Erstellen neuer Dokument Anbieter Erweiterungen

Das Erstellen neuer Dokument Anbieter Erweiterungen liegt außerhalb des Umfangs dieses Einführungs Artikels. Diese Informationen werden hier bereitgestellt, um anzuzeigen, dass eine Anwendung basierend auf den Erweiterungen, die ein Benutzer in sein IOS-Gerät geladen hat, möglicherweise Zugriff auf die Speicherorte von Dokumenten außerhalb des von Apple bereitgestellten icloud-Standorts hat.

Der Entwickler sollte diese Tatsache beachten, wenn er die Dokument Auswahl verwendet und mit externen Dokumenten arbeitet. Sie sollten nicht davon ausgehen, dass diese Dokumente in icloud gehostet werden.

Weitere Informationen zum Erstellen eines Speicher Anbieters oder einer Dokument Auswahl Erweiterung finden Sie im Dokument [Introduction to App Extensions](~/ios/platform/extensions.md) .

## <a name="migrating-to-icloud-drive"></a>Migrieren zu icloud-Laufwerk

Unter IOS 8 können Benutzer das vorhandene icloud-Dokumenten System, das in ios 7 (und früheren Systemen) verwendet wird, weiterhin verwenden, oder Sie können vorhandene Dokumente zum neuen icloud Drive-Mechanismus migrieren.

Auf Mac OS X Yosemite bietet Apple keine Abwärtskompatibilität, sodass alle Dokumente zu einem icloud-Laufwerk migriert oder nicht mehr auf Geräten aktualisiert werden müssen.

Nachdem das Konto eines Benutzers zu einem icloud-Laufwerk migriert wurde, können nur Geräte, die das icloud-Laufwerk verwenden, Änderungen an Dokumenten über diese Geräte weitergeben.

> [!IMPORTANT]
> Entwickler sollten sich bewusst sein, dass die neuen Features, die in diesem Artikel behandelt werden, nur verfügbar sind, wenn das Konto des Benutzers zu icloud Drive migriert wurde. 

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die Änderungen an vorhandenen icloud-APIs behandelt, die zur Unterstützung von icloud Drive und des neuen Dokument Auswahl-Ansichts Controllers erforderlich sind. Die Datei Koordination ist abgedeckt, und es ist wichtig, dass Sie bei der Arbeit mit cloudbasierten Dokumenten wichtig ist. Es wurde die Einrichtung erläutert, die zum Aktivieren von cloudbasierten Dokumenten in einer xamarin. IOS-Anwendung erforderlich ist, und eine Einführung in die Arbeit mit Dokumenten außerhalb der Anwendungs Container einer App mithilfe des Dokument Auswahl-Ansichts Controllers.

Außerdem wurden in diesem Artikel die Dokument Anbieter Erweiterungen kurz erläutert und erläutert, warum der Entwickler beim Schreiben von Anwendungen, die cloudbasierte Dokumente verarbeiten können, diese kennen sollten.

## <a name="related-links"></a>Verwandte Links

- [Docpicker (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-docpicker)
- [Einführung in iOS 8](~/ios/platform/introduction-to-ios8.md)
- [Einführung in App-Erweiterungen](~/ios/platform/extensions.md)
