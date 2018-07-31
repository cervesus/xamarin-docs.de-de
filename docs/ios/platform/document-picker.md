---
title: Dokumentauswahl in Xamarin.iOS
description: In diesem Dokument wird beschrieben, die iOS-Dokumentauswahl und erläutert, wie in Xamarin.iOS verwenden. Es dauert einen Blick auf iCloud, Dokumente, allgemeine Setupcode, Dokument-Anbietererweiterungen und vieles mehr.
ms.prod: xamarin
ms.assetid: 89539D79-BC6E-4A3E-AEC6-69D9A6CC6818
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/05/2017
ms.openlocfilehash: ca0c7a6e655fdc44aa673a59be71bc83044d3085
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353333"
---
# <a name="document-picker-in-xamarinios"></a>Dokumentauswahl in Xamarin.iOS

Die Dokumentauswahl können Dokumente apps gemeinsam genutzt werden. Diese Dokumente können in iCloud oder in eine andere app-Verzeichnis gespeichert werden. Dokumente werden gemeinsam genutzt, über den Satz von [Dokument-Anbietererweiterungen](~/ios/platform/extensions.md) der Benutzer hat auf ihrem Gerät installiert. 

Aufgrund der Schwierigkeit, Dokumente, die in Ihre apps und der Cloud synchronisiert zu halten bringen jedoch eine gewisse erforderlichen Komplexität.

## <a name="requirements"></a>Anforderungen

Folgendes ist erforderlich, um die in diesem Artikel vorgestellten Schritte ausführen:

-  **Xcode 7 und iOS 8 oder neueren** – Apple Xcode 7 und iOS 8 oder neueren APIs müssen installiert und konfiguriert werden, auf dem Computer des Entwicklers.
-  **Visual Studio oder Visual Studio für Mac** – die neueste Version von Visual Studio für Mac installiert werden soll.
-  **iOS-Gerät** – ein iOS-Gerät unter iOS 8 oder höher.

## <a name="changes-to-icloud"></a>Änderungen in iCloud

Um die neuen Funktionen von der Dokumentauswahl implementieren zu können, wurden in iCloud für Apple Dienst die folgenden Änderungen vorgenommen:

-  Die iCloud-Daemon wurde vollständig umgeschrieben wurde, verwenden von CloudKit.
-  Vorhandene iCloud, in denen Funktionen waren umbenannt iCloud Drive.
-  Unterstützung für Microsoft Windows-Betriebssystem wurde in iCloud hinzugefügt.
-  Ein Ordner "iCloud" wurde in Mac OS Finder hinzugefügt.
-  iOS-Geräte können den Inhalt des Ordners iCloud Mac OS zugreifen.

> [!IMPORTANT]
> Apple [stellt Tools zur Verfügung](https://developer.apple.com/support/allowing-users-to-manage-data/), die Entwickler dabei unterstützen, die Datenschutz-Grundverordnung (DSGVO) der Europäischen Union umzusetzen.

## <a name="what-is-a-document"></a>Was ist ein Dokument?

In Bezug auf ein Dokument in iCloud ist eine einzelne, eigenständige Entität und sollte daher vom Benutzer wahrgenommen werden. Ein Benutzer kann beim Ändern Sie das Dokument oder (per e-Mail-Adresse, z. B.) für andere Benutzer freigeben möchten.

Es gibt mehrere Typen von Dateien, die der Benutzer sofort als Dokumenten, z. B. Seiten Keynote oder Zahlen Dateien erkannt werden. ICloud ist jedoch nicht auf dieses Konzept beschränkt. Beispielsweise kann der Status eines Spiels (z. B. Chess Übereinstimmung) als ein Dokument behandelt und in iCloud gespeichert werden. Diese Datei kann zwischen Geräten des Benutzers übergeben werden und diese, um ein Spiel übernehmen Stelle sie auf einem anderen Gerät zulassen.

## <a name="dealing-with-documents"></a>Umgang mit Dokumenten

Bevor näher auf den erforderlichen Code für die Dokumentauswahl mit Xamarin verwenden, in diesem Artikel wird die bewährten Methoden für die Arbeit mit iCloud-Dokumente behandeln, und einige Änderungen an vorhandenen APIs erforderlich, um die Dokumentauswahl unterstützen.

### <a name="using-file-coordination"></a>Verwenden die Datei Koordination

Da eine Datei an verschiedenen Stellen geändert werden kann, muss Koordination verwendet werden, um Datenverluste zu vermeiden.

 [![](document-picker-images/image1.png "Verwenden die Datei Koordination")](document-picker-images/image1.png#lightbox)

Werfen Sie einen Blick auf die obige Abbildung an:

1.  Ein iOS-Gerät mithilfe der Datei Coordination erstellt ein neues Dokument und speichert sie in der iCloud-Ordner.
2.  iCloud speichert die geänderte Datei in die Cloud für die Verteilung auf jedem Gerät.
3.  Eine angefügte Mac sieht die geänderte Datei in die iCloud-Ordner und Koordinierung der Datei verwendet, um die Änderungen in die Datei kopieren.
4.  Ein Geräts nicht mithilfe des Datei-Koordination nimmt eine Änderung an der Datei und speichert sie in der iCloud-Ordner. Diese Änderungen werden sofort in den anderen Geräten repliziert.

Angenommen die ursprüngliche iOS-Gerät oder dem Mac wurde die Datei bearbeiten nun ihre Änderungen verloren gehen und durch die Version der Datei, aus dem Unkoordinierte Gerät außer Kraft gesetzt werden. Um Datenverluste zu vermeiden, ist Koordination der Datei beim Arbeiten mit Cloud-basierten Dokumenten.

### <a name="using-uidocument"></a>Verwenden von UIDocument

 `UIDocument` macht die Dinge einfach (oder `NSDocument` unter MacOS) wie all die Schwerarbeit für Entwickler. Es bietet Datei zusammen mit Hintergrund-Warteschlangen zum Blockieren der Benutzeroberfläche der Anwendung erstellt.

 `UIDocument` stellt mehrere APIs auf höherer Ebene, die den Entwicklungsaufwand der Xamarin-Anwendung zu erleichtern, für alle Entwickler dessen Zweck ist erforderlich.

Der folgende Code erstellt eine Unterklasse von `UIDocument` eine generische Textdokument zu implementieren, die zum Speichern und Abrufen von Text aus der iCloud-verwendet werden kann:

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

Die `GenericTextDocument` oben dargestellten Klasse wird in diesem Artikel verwendet werden, bei der Arbeit mit den Dokumentauswahl und externe Dokumente in einer Xamarin.iOS-8-Anwendung.

## <a name="asynchronous-file-coordination"></a>Asynchrone Datei-Koordination

iOS 8 bietet mehrere neue asynchrone Datei-Coordination-Funktionen über die neue Datei Coordination-APIs. Vor iOS 8 waren alle vorhandenen Datei Koordination APIs völlig synchron. Dies bedeutete, dass der Entwickler verantwortlich für die Implementierung eigener Hintergrunds queuing, um zu verhindern, dass die Datei Koordination der Benutzeroberfläche der Anwendung blockiert wurde.

Die neue `NSFileAccessIntent` -Klasse enthält eine URL verweist auf die Datei und mehrere Optionen zum Steuern der Art der Koordination erforderlich. Der folgende Code zeigt, verschieben eine Datei von einem Speicherort in einen anderen mithilfe von Intents:

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

## <a name="discovering-and-listing-documents"></a>Ermitteln und Auflisten von Dokumenten

Die Möglichkeit, um zu ermitteln und die Liste von Dokumenten wird mithilfe der vorhandenen `NSMetadataQuery` APIs. In diesem Abschnitt behandelt die neuen Features in `NSMetadataQuery` , die, die Arbeit mit Dokumenten sogar einfacher als zuvor.

### <a name="existing-behavior"></a>Vorhandene Verhalten zur

Bevor Sie iOS 8 `NSMetadataQuery` war auf dem lokalen dateiänderungen wie z. B.: Löscht, erstellt und benennt.

 [![](document-picker-images/image2.png "Übersicht über lokale Datei NSMetadataQuery")](document-picker-images/image2.png#lightbox)

In der obigen Abbildung:

1.  Für Dateien, die bereits im Container-Anwendung `NSMetadataQuery` vorhanden `NSMetadata` Datensätze vorab erstellt und in die Warteschlange gestellt, damit sie die Anwendung sofort zur Verfügung stehen.
1.  Die Anwendung erstellt eine neue Datei im Container-Anwendung.
1.  Es gibt eine Verzögerung vor dem `NSMetadataQuery` erkennt die Änderung in den Container-Anwendung und erstellt die erforderlichen `NSMetadata` Datensatz.


Aufgrund der Verzögerung bei der Erstellung der `NSMetadata` aufzeichnen, musste die Anwendung haben zwei Quellen zu öffnen: eine lokale Datei Änderungen und eine für Cloud-basierten Änderungen.

### <a name="stitching"></a>Zusammenfügen von

In iOS 8 `NSMetadataQuery` ist einfacher, direkt in ein neues Feature namens zusammenfügen verwenden:

 [![](document-picker-images/image3.png "NSMetadataQuery als neues Feature namens zusammenfügen")](document-picker-images/image3.png#lightbox)

Verwenden in der obigen Abbildung Zusammenfügen:

1.  Wie zuvor für Dateien, die bereits im Container-Anwendung `NSMetadataQuery` vorhanden `NSMetadata` Datensätze vorab erstellt und in die Warteschlange gestellt.
1.  Die Anwendung erstellt eine neue Datei im Container-Anwendung mithilfe von Koordination der Datei.
1.  Ein Hook in den Container für die Anwendung sieht die Änderungen und ruft `NSMetadataQuery` beim Erstellen der erforderlichen `NSMetadata` Datensatz.
1.  Die `NSMetadata` Datensatz wird direkt nach der Datei erstellt und an die Anwendung zur Verfügung gestellt wird.


Zusammenfügen mit die Anwendung nicht mehr muss Öffnen einer Datenquelle zum Überwachen von lokalen und cloudbasierten dateiänderungen. Nachdem die Anwendung auf zurückgreifen kann `NSMetadataQuery` direkt.

> [!IMPORTANT]
> Zusammenfügen von funktioniert nur, wenn die Anwendung Koordination der Datei verwendet, wie im vorherigen Abschnitt angezeigt. Wenn die Datei Koordination nicht verwendet wird, standardmäßig die APIs das vorhandene Verhalten zur vor iOS 8.




### <a name="new-ios-8-metadata-features"></a>Neue Features von iOS 8-Metadaten

Die folgenden neuen Funktionen wurden hinzugefügt `NSMetadataQuery` IOS 8:

-   `NSMetatadataQuery` können jetzt nicht lokalen-Dokumente, die in der Cloud gespeicherten auflisten.
-  Neue APIs wurden hinzugefügt, den Zugriff auf Metadateninformationen in den Cloud-basierten Dokumenten. 
-  Es gibt eine neue `NSUrl_PromisedItems` API zur Verfügung, auf die Dateiattribute der Dateien, die möglicherweise nicht möglicherweise ihre verfügbaren Inhalte lokal.
-  Verwenden Sie die `GetPromisedItemResourceValue` Methode zum Abrufen von Informationen zu einer bestimmten Datei oder verwenden Sie die `GetPromisedItemResourceValues` -Methode zum Abrufen von Informationen auf mehr als eine Datei zu einem Zeitpunkt.


Für den Umgang mit Metadaten wurden zwei neue Optionen für die Datei-Koordination hinzugefügt:

-   `NSFileCoordinatorReadImmediatelyAvailableMetadataOnly` 
-   `NSFileCoordinatorWriteContentIndependentMetadataOnly` 


Mit den oben genannten Flags verwenden müssen den Inhalt der Datei Dokument nicht lokal für verwendet werden.

Das folgende Codesegment zeigt, wie `NSMetadataQuery` Abfragen, die das Vorhandensein einer bestimmten Datei aus, und erstellen Sie die Datei aus, wenn er nicht vorhanden:

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
            new Selector("queryDidFinishGathering:"),           NSMetadataQuery.DidFinishGatheringNotification,
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

### <a name="document-thumbnails"></a>Dokument-Miniaturansichten

Apple nun das Gefühl, die bestmögliche benutzererfahrung beim Auflisten von Dokumenten für eine Anwendung ist die Verwendung der Vorschau. Dies bietet den Kontext für den End-Benutzer, damit sie schnell das Dokument identifiziert werden kann, dem sie verwenden möchten.

Vor iOS 8 erforderlich, mit der Vorschau von Dokumenten eine benutzerdefinierte Implementierung. Noch nicht mit iOS 8 sind die Attribute des Dateisystems, die die Entwickler schnell mit Dokument-Miniaturansichten arbeiten können.

#### <a name="retrieving-document-thumbnails"></a>Abrufen von Dokument-Miniaturansichten 

Durch Aufrufen der `GetPromisedItemResourceValue` oder `GetPromisedItemResourceValues` Methoden `NSUrl_PromisedItems` -API, eine `NSUrlThumbnailDictionary`, zurückgegeben wird. Der einzige Schlüssel derzeit in dieses Wörterbuch ist die `NSThumbnial1024X1024SizeKey` und der entsprechenden `UIImage`.

#### <a name="saving-document-thumbnails"></a>Speichern von Dokument-Miniaturansichten

Die einfachste Möglichkeit zum Speichern einer Miniaturansicht ist die Verwendung `UIDocument`. Durch Aufrufen der `GetFileAttributesToWrite` Methode der `UIDocument` und Festlegen der Miniaturansicht, es werden automatisch gespeichert, wenn die Dokument-Datei ist. ICloud Daemon diese Änderung sehen und in iCloud weitergeben. Unter Mac OS X sind Miniaturansichten für den Entwickler durch die schnelle Suchen-Plug-in automatisch generiert.

Mit den Grundlagen der Arbeit mit basierte iCloud-Dokumente in Ort zusammen und die Änderungen an vorhandenen API, wir können die Dokument-Auswahl-View-Controller in einer Xamarin.IOS 8 implementieren Mobile-Anwendung.


## <a name="enabling-icloud-in-xamarin"></a>Aktivieren von iCloud in Xamarin

Bevor die Dokumentauswahl in einer Xamarin.iOS-Anwendung verwendet werden kann, muss iCloud-Support in Ihrer Anwendung sowohl über Apple aktiviert werden. 

Die folgenden Schritte Exemplarische Vorgehensweise der Prozess der Bereitstellung für die iCloud.

1. Erstellen Sie ein iCloud-Container.
2. Erstellen Sie eine App-ID, die die iCloud-App Service enthält.
3. Erstellen Sie ein Bereitstellungsprofil, das diese App ID. enthält.

Die [arbeiten mit Funktionen](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md) Leitfaden führt Sie durch die ersten zwei Schritte. Um ein Bereitstellungsprofil erstellen, führen Sie die Schritte in der [Bereitstellungsprofil](~/ios/get-started/installation/device-provisioning/index.md#provisioning-your-device) Guide.



Die folgenden Schritte Exemplarische Vorgehensweise den Prozess der Konfiguration Ihrer Anwendung für die iCloud:

Führen Sie folgende Schritte aus:

1.  Öffnen Sie das Projekt in Visual Studio für Mac oder Visual Studio.
2.  In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie Optionen aus.
3.  Wählen Sie im Dialogfeld Optionen **iOS-Anwendung**, sicher, dass die **Bündel-ID** entspricht, die in definiert wurde **App-ID** oben für die Anwendung erstellt. 
4.  Wählen Sie **iOS Bundle-Signierung**, wählen die **Entwickleridentität** und **Bereitstellungsprofil** oben erstellt haben.
5.  Klicken Sie auf die **OK** Schaltfläche, um die Änderungen zu speichern und schließen Sie das Dialogfeld.
6.  Mit der rechten Maustaste auf `Entitlements.plist` in die **Projektmappen-Explorer** um ihn im Editor zu öffnen.

    > [!IMPORTANT]
    > In Visual Studio müssen Sie möglicherweise auf die Berechtigungs-Editor zu öffnen, indem Sie mit der rechten Maustaste darauf, auswählen von **Öffnen mit...** und plist auswählen

7.  Überprüfen Sie **iCloud aktivieren** , **iCloud-Dokumente** , **Schlüssel-Wert-Speicher** und **CloudKit** .
8.  Stellen Sie sicher die **Container** für die Anwendung vorhanden ist, (wie oben erstellt haben). Ein Beispiel: `iCloud.com.your-company.AppName`
9.  Speichern Sie die Änderungen in der Datei.

Weitere Informationen zu Berechtigungen finden Sie in der [arbeiten mit Berechtigungen](~/ios/deploy-test/provisioning/entitlements.md) Guide.

Mit der oben beschriebenen Einrichtung vorhanden kann die Anwendung jetzt Cloud-basierten Dokumenten und das neue Dokument Picker-View-Controller verwenden.

## <a name="common-setup-code"></a>Allgemeine Setupcode

Bevor Sie beginnen mit dem Dokument Picker-View-Controller, es gibt standardeinrichtung Code erforderlich. Ändern die Anwendung zunächst `AppDelegate.cs` Datei, und stellen sie wie folgt aussehen:

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
                    // Yes, inform caller and save location the the Application Container
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
> Der obige Code enthält den Code aus dem obigen Abschnitt Ermitteln von und Auflisten von Dokumenten. Es wird hier in seiner Gesamtheit und angezeigt werden, wie es in einer echten Anwendung angezeigt wird. Der Einfachheit halber wird in diesem Beispiel wird mit einer einzelnen, hart kodierte Datei funktioniert (`test.txt`) nur.

Der obige Code macht mehrere Arbeitserleichterungen vor iCloud Drive und so einfacher, mit der Rest der Anwendung arbeiten, verfügbar.

Als Nächstes fügen Sie den folgenden Code, um jede Sicht oder den Container anzeigen, die mithilfe der Dokumentauswahl oder Arbeiten mit Cloud-basierten Dokumenten werden:

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

Dadurch wird eine Verknüpfung zum Abrufen der `AppDelegate` und Zugriff auf die oben erstellten iCloud-Verknüpfungen.

Mit diesem Code werden werfen wir einen Blick auf die Implementierung der Dokument-Auswahl-View-Controller in einer Xamarin iOS 8-Anwendung.

## <a name="using-the-document-picker-view-controller"></a>Mithilfe der Dokument-Auswahl-View-Controller

Bevor Sie iOS 8 war es sehr schwierig, Dokumente aus einer anderen Anwendung zugreifen, da gab es keine Möglichkeit, Dokumente, die außerhalb der Anwendung von innerhalb der app zu ermitteln.

### <a name="existing-behavior"></a>Vorhandene Verhalten zur

 [![](document-picker-images/image31.png "Übersicht über die vorhandenen Verhalten")](document-picker-images/image31.png#lightbox)

Werfen wir einen Blick auf den Zugriff auf ein externes Dokument vor iOS 8:

1.  Zuerst wird der Benutzer verfügen, um die Anwendung zu öffnen, die das Dokument ursprünglich erstellt.
1.  Das Dokument ausgewählt ist und die `UIDocumentInteractionController` wird verwendet, um das Dokument an die neue Anwendung zu senden.
1.  Zum Schluss wird eine Kopie des ursprünglichen Dokuments in der neuen Anwendung Container platziert.


Dort ist das Dokument zur Verfügung, für die zweite Anwendung öffnen und bearbeiten.

### <a name="discovering-documents-outside-of-an-apps-container"></a>Ermitteln von Dokumenten außerhalb der App-Containers

Unter iOS 8 ist eine Anwendung Zugriff auf Dokumente außerhalb ihres eigenen Containers Anwendung problemlos möglich:

 [![](document-picker-images/image32.png "Ermitteln von Dokumenten außerhalb der App-Containers")](document-picker-images/image32.png#lightbox)

Über Dokumentauswahl neue iCloud ( `UIDocumentPickerViewController`), eine iOS-Anwendung direkt erkennen und außerhalb ihres Containers für die Anwendung zugreifen kann. Die `UIDocumentPickerViewController` bietet ein Mechanismus für den Benutzer zum Gewähren des Zugriffs auf und bearbeiten die ermittelten Dokumente über die Berechtigungen.

Eine Anwendung muss teilnehmen für seine Dokumente in iCloud Dokumentauswahl angezeigt und stehen für andere Anwendungen, um zu ermitteln und mit ihnen arbeiten können. Haben Sie eine Xamarin iOS 8-Anwendung die Anwendungscontainer freigeben, Sie bearbeiten, `Info.plist` -Datei in einem standard-Text-Editor, und fügen Sie die folgenden zwei Zeilen hinzu, an das Ende des Wörterbuchs (zwischen den `<dict>...</dict>` Tags):

```xml
<key>NSUbiquitousContainerIsDocumentScopePublic</key>
<true/>
```

Die `UIDocumentPickerViewController` bietet eine hervorragende neue Benutzeroberfläche, die dem Benutzer ermöglicht, Dokumente zu wählen. Um den Dokument-Auswahl-View-Controller in einer Xamarin iOS 8-Anwendung anzuzeigen, führen Sie folgende Schritte aus:

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
> Der Entwickler muss Aufrufen der `StartAccessingSecurityScopedResource` Methode der `NSUrl` damit ein externes Dokument zugegriffen werden kann. Die `StopAccessingSecurityScopedResource` Methode aufgerufen werden, um die Sicherheit-Sperre freizugeben, sobald das Dokument geladen wurde.

### <a name="sample-output"></a>Beispielausgabe

Hier wird verdeutlicht, wie der obige Code eine Dokumentauswahl, die bei Ausführung auf einem iPhone-Gerät angezeigt wird:

1.  Der Benutzer die Anwendung gestartet wird, und die Hauptkomponente der Benutzeroberfläche angezeigt wird:   
 
    [![](document-picker-images/image33.png "Die Hauptkomponente der Benutzeroberfläche wird angezeigt.")](document-picker-images/image33.png#lightbox)
1.  Die Benutzer-Klicks die **Aktion** Schaltfläche am oberen Rand des Bildschirms und wird gebeten, wählen Sie eine **Dokument Anbieter** aus der Liste der verfügbaren Anbieter:   
 
    [![](document-picker-images/image34.png "Wählen Sie aus der Liste der verfügbaren Anbieter einen Dokument-Anbieter")](document-picker-images/image34.png#lightbox)
1.  Die **Dokument Picker-View-Controller** wird angezeigt, für den ausgewählten **Dokument Anbieter**:   
 
    [![](document-picker-images/image35.png "Der Dokument-Auswahl-View-Controller wird angezeigt.")](document-picker-images/image35.png#lightbox)
1.  Der Benutzer tippt auf eine **Ordner "Dokumente"** um seinen Inhalt anzuzeigen:   
 
    [![](document-picker-images/image36.png "Der Inhalt der Ordner \"Dokumente\"")](document-picker-images/image36.png#lightbox)
1.  Der Benutzer wählt eine **Dokument** und **Dokumentauswahl** geschlossen wird.
1.  Die Hauptkomponente der Benutzeroberfläche wird erneut angezeigt, die **Dokument** geladen wird, aus dem externen Container und dessen Inhalt angezeigt.


Die tatsächliche Anzeige des Ansichtscontrollers Dokument Auswahl hängt von der Dokument-Anbieter, dass der Benutzer auf dem Gerät installiert hat und welche Auswahl Dokumentmodus implementieren wurde ab. Im obige Beispiel ist der Modus "Open" verwenden, der andere Modus Typen werden im Anschluss näher erläutert werden.

## <a name="managing-external-documents"></a>Verwalten von externen Dokumenten

Wie oben beschrieben, bevor Sie iOS 8 konnte eine Anwendung nur Dokumente zugreifen, die Teil des Containers Anwendung waren. IOS 8 kann eine Anwendung, Dokumente aus externen Quellen zugreifen:

 [![](document-picker-images/image37.png "Übersicht über die externe Dokumente verwalten")](document-picker-images/image37.png#lightbox)

Wenn der Benutzer ein Dokument aus einer externen Quelle auswählt, richtet ein Referenzdokument in den Container-Anwendung, die auf das ursprüngliche Dokument verweist.

Um beim Hinzufügen dieses neue Feature in vorhandene Anwendungen zu unterstützen, mehrere neue Features hinzugefügt wurden die `NSMetadataQuery` API. In der Regel verwendet eine Anwendung den universellen Dokument Bereich zum Auflisten von Dokumenten, die sich innerhalb des Containers der Anwendung befinden. Verwenden diesen Bereich, werden nur die Dokumente innerhalb des Containers für die Anwendung weiterhin angezeigt werden soll.

Mit den neuen universellen externen Dokument Bereich gibt Dokumente zurück, die außerhalb des Containers für die Anwendung live und die Metadaten für diese zurückgeben. Die `NSMetadataItemUrlKey` verweist auf die URL, wo befindet sich das Dokument tatsächlich.

Manchmal möchten eine Anwendung nicht mit den Dokumenten, die auf das gezeigt wird als th-Verweis zu arbeiten. Stattdessen möchte die app direkt mit dem Verweis-Dokument zu arbeiten. Beispielsweise sollten die app das Dokument in den Ordner der Anwendung in der Benutzeroberfläche angezeigt oder dem Benutzer ermöglichen, die Verweise in einem Ordner zu verschieben.

In iOS 8 ein neues `NSMetadataItemUrlInLocalContainerKey` wurde bereitgestellt, damit das Referenzdokument direkt zugreifen. Dieser Schlüssel verweist auf den tatsächlichen Verweis auf das externe Dokument in einem Container-Anwendung.

Die `NSMetadataUbiquitousItemIsExternalDocumentKey` wird verwendet, um zu testen, ob ein Dokument außerhalb der Anwendung Containers ist oder nicht. Die `NSMetadataUbiquitousItemContainerDisplayNameKey` wird verwendet, um den Namen des Containers zugreifen, die die ursprüngliche Kopie ein externes Dokument untergebracht wird.

### <a name="why-document-references-are-required"></a>Warum Dokumentverweise erforderlich sind.

Die Hauptgründe dafür, dass der Zugriff auf externe Dokumente verwendet, iOS 8 ist die Sicherheit. Keine Anwendung erhält Zugriff auf eine andere Anwendung Container. Nur die Dokumentauswahl möglich, dass da ausgeführten Out-of-Process und System weitreichenden Zugriff hat.

Die einzige Möglichkeit, um ein Dokument außerhalb des Containers für die Anwendung zu erhalten ist mit die Dokumentauswahl, und die URL zurückgegeben, durch die Auswahl ist Sicherheit beschränkt. Die Sicherheit im Bereich einer URL enthält nur genügend Informationen, um das Dokument sowie die bereichsbezogenen Berechtigungen sind erforderlich zum Erteilen von einer Anwendung den Zugriffs auf das Dokument ausgewählt.

Es ist wichtig zu beachten, dass wenn die Sicherheit im Bereich einer URL wurde in eine Zeichenfolge dann serialisiert und deserialisiert, die Sicherheitsinformationen verloren, und die Datei aus der URL zugegriffen werden. Die Dokumentverweis-Funktion bietet einen Mechanismus, um wieder auf die Dateien, verweist diese URLs zu erhalten.

Wenn die Anwendung erhält eine `NSUrl` von eines der Dokumente, Referenz, er bereits hat den Sicherheitsbereich angefügt und kann verwendet werden, um Zugriff auf die Datei. Aus diesem Grund wird dringend empfohlen, dass der Entwickler `UIDocument` , da er alle diese Daten und Prozesse für sie verarbeitet.

### <a name="using-bookmarks"></a>Verwenden von Lesezeichen

Es ist nicht immer möglich, Dokumente von einer Anwendung zu einem bestimmten Dokument, z. B. bei der Wiederherstellung des Benutzerzustand aufgelistet werden. iOS 8 bietet einen Mechanismus, um Lesezeichen zu erstellen, die ein bestimmtes Dokument direkt als Ziel.

Der folgende Code erstellt ein Lesezeichen aus einem `UIDocument`des `FileUrl` Eigenschaft:

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

Die vorhandene Lesezeichen-API wird verwendet, um ein Lesezeichen für eine vorhandene erstellen `NSUrl` , gespeichert und zur direkten Zugriff auf eine externe Datei geladen werden können. Der folgende Code wird ein Lesezeichen wiederherstellen, die weiter oben erstellt wurde:

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

## <a name="open-vs-import-mode-and-the-document-picker"></a>Öffnen Sie Visual Studio. Importmodus und die Dokumentauswahl

Der Dokument-Auswahl-View-Controller bietet zwei unterschiedliche Betriebsmodi:

1.  **Öffnen Sie im Modus** – In diesem Modus, wenn der Benutzer auswählt und externen Dokument die Dokumentauswahl ein Lesezeichen für die Sicherheit-Bereich im Container-Anwendung erstellt.   
 
    [![](document-picker-images/image37.png "Ein im Bereich einer Textmarke in den Anwendungscontainer")](document-picker-images/image37.png#lightbox)
1.  **Importmodus** – In diesem Modus, wenn der Benutzer auswählt und externen Dokument, das die Dokumentauswahl nicht erstellen Sie ein Lesezeichen, aber stattdessen kopieren Sie die Datei in einen temporären Speicherort und Bereitstellen den Anwendungszugriff auf das Dokument an diesem Speicherort:   
 
    [![](document-picker-images/image38.png "Die Dokumentauswahl wird kopieren Sie die Datei in einen temporären Speicherort und Bereitstellen die Anwendungszugriff auf dem Dokument an dieser Stelle")](document-picker-images/image38.png#lightbox)   
 Sobald die Anwendung aus irgendeinem Grund beendet wird, wird der temporäre Speicherort geleert, und die Datei entfernt. Wenn die Anwendung für den Zugriff auf die Datei muss, sollten sie eine Kopie und platzieren Sie es in einem Container-Anwendung.


Der Modus "Open" ist nützlich, wenn die Anwendung mit einer anderen Anwendung zusammenzuarbeiten, und alle Änderungen an das Dokument mit der jeweiligen Anwendung freigeben möchte. Importmodus wird verwendet, wenn die Anwendung nicht die Änderungen an einem Dokument mit anderen Anwendungen gemeinsam nutzen möchten.

## <a name="making-a-document-external"></a>Machen ein Dokument externe

Wie oben erwähnt, ist eine iOS 8-Anwendung in Container außerhalb ihres eigenen Containers Anwendung keinen Zugriff. Die Anwendung kann lokal oder in einem temporären Speicherort auf den eigenen Container zu schreiben und dann einen spezielles Dokument-Modus verwenden, um das resultierende Dokument außerhalb des Containers für die Anwendung zu einem Benutzer ausgewählten Speicherort zu verschieben.

Um ein Dokument an einen externen Speicherort zu verschieben, führen Sie folgende Schritte aus:

1.  Erstellen Sie zunächst ein neues Dokument in einem lokalen oder temporären Speicherort.
1.  Erstellen Sie eine `NSUrl` , der auf das neue Dokument verweist.
1.  Öffnen Sie ein neues Dokument Auswahl-View-Controller, und übergeben sie die `NSUrl` mit dem Modus der `MoveToService` . 
1.  Sobald der Benutzer einen neuen Speicherort auswählt, wird das Dokument an ihrem aktuellen Speicherort auf den neuen Speicherort verschoben werden.
1.  Ein Referenzdokument wird auf der app-Anwendungscontainer geschrieben, damit die Datei immer noch zugegriffen werden kann, können Sie von der Anwendung erstellen.


Der folgende Code kann verwendet werden, um ein Dokument an einen externen Speicherort zu verschieben: `var picker = new UIDocumentPickerViewController (srcURL, UIDocumentPickerMode.MoveToService);`

Ist das Referenzdokument, die von den oben angegebenen Vorgang zurückgegebenen entspricht genau erstellt haben, indem Sie den Modus "Open", der die Dokumentauswahl. Es gibt jedoch Fälle, in denen die Anwendung ein Dokument zu verschieben, ohne einen Verweis darauf behalten möchten.

Verwenden Sie zum Verschieben eines Dokuments, ohne einen Verweis zu erstellen, die `ExportToService` Modus. Ein Beispiel: `var picker = new UIDocumentPickerViewController (srcURL, UIDocumentPickerMode.ExportToService);`

Bei Verwendung der `ExportToService` Modus, wird es in den externen Container kopiert und die vorhandene Kopie am ursprünglichen Speicherort bleibt.

## <a name="document-provider-extensions"></a>Dokument-Anbietererweiterungen

Mit iOS 8 will Apple die Endbenutzer in der Lage, auf ihre Cloud-basierten Dokumenten, unabhängig davon, wo sie tatsächlich vorhanden sind. Um dieses Ziel zu erreichen, bietet iOS 8 einen neuen Mechanismus für die Anbieter-Dokumenterweiterung.

### <a name="what-is-a-document-provider-extension"></a>Was ist eine Erweiterung der Dokument-Anbieter?

Einfach ausgedrückt ist eine Erweiterung der Dokument-Anbieter eine Möglichkeit für Entwickler oder von Drittanbietern, um eine alternative Dokument Anwendungsspeicher bereitzustellen, die auf genau die gleiche Weise als den vorhandenen Speicherort der iCloud-Speicher zugegriffen werden kann.

Der Benutzer kann eine der folgenden alternativen Speicherorte aus der Dokument-Auswahl auswählen und können genau gleichen Zugriffsmodi für das (Open, Import, verschieben oder Export), zum Arbeiten mit Dateien, die an diesem Speicherort.

Dies wird mithilfe von zwei verschiedenen Erweiterungen implementiert:

-  **Dokumentauswahl-Erweiterung zu dokumentieren** – bietet eine `UIViewController` Unterklasse, die eine grafische Benutzeroberfläche für den Benutzer zur Auswahl eines Dokuments eines alternativen Speicherort bereitstellt. Diese Unterklasse wird als Teil des Ansichtscontrollers Dokument Auswahl angezeigt.
-  **Geben Sie den/die Dateierweiterung** – Dies ist eine nicht-UI-Erweiterung, die mit der Bereitstellung tatsächlich den Inhalt der Dateien behandelt. Diese Erweiterungen werden durch die Koordination der Datei bereitgestellt ( `NSFileCoordinator` ). Dies ist ein weiterer wichtiger Fall, in dem Datei-Koordinierung erforderlich ist.


Das folgende Diagramm zeigt den typischen Data Flow aus, bei der Arbeit mit Dokument-Anbietererweiterungen:

 [![](document-picker-images/image39.png "Dieses Diagramm zeigt den typischen Data Flow aus, bei der Arbeit mit Dokument-Anbietererweiterungen")](document-picker-images/image39.png#lightbox)

Der folgende Prozess durchgeführt:

1.  Die Anwendung stellt einen Dokument-Auswahl-Controller, damit der Benutzer, wählen Sie eine Datei mit arbeiten kann.
1.  Der Benutzer auswählt, einen alternativen Speicherort und die benutzerdefinierten `UIViewController` Erweiterung wird aufgerufen, um die Benutzeroberfläche anzuzeigen.
1.  Der Benutzer wählt eine Datei von diesem Speicherort aus, und die URL an die die Dokumentauswahl.
1.  Die Dokumentauswahl wählt der Datei-URL und gibt sie zurück an die Anwendung für den Benutzer auf.
1.  Datei Koordinators fest, um den Inhalt der Dateien an die Anwendung zurückgegeben wird die URL übergeben.
1.  Der Koordinator Datei ruft die benutzerdefinierte Erweiterung der Datei-Anbieter zum Abrufen der Datei.
1.  Der Inhalt der Datei werden mit dem Datei-Coordinator zurückgegeben.
1.  Der Inhalt der Datei werden an die Anwendung zurückgegeben.


### <a name="security-and-bookmarks"></a>Sicherheit und Lesezeichen

In diesem Abschnitt dauert einen kurzen Blick auf die über Lesezeichen funktioniert mit Dokument-Anbietererweiterungen wie Sicherheit und persistenten Datei zugreifen. Im Gegensatz zu den iCloud-Dokument-Anbieter, der Sicherheits- und Lesezeichen in den Container für die Anwendung automatisch speichert, nicht Dokument-Anbietererweiterungen, da sie nicht Teil des Verweissystems Dokument sind.

Zum Beispiel: in einer unternehmensumgebung, die eine eigene sichere unternehmensweiten-Datenspeicher bereitstellt, Administratoren nicht vertrauliche Unternehmensdaten zugegriffen oder von der öffentlichen iCloud Servern verarbeitet werden soll. Aus diesem Grund kann das integrierte Dokument Reference-System verwendet werden.

Die Lesezeichen-System kann weiterhin verwendet werden, und es ist Aufgabe der Anbieter Dateierweiterung zu eine Lesezeichen-URL ordnungsgemäß verarbeiten, und geben Sie den Inhalt des Dokuments verweist es zurück.

Aus Sicherheitsgründen hat iOS 8 eine Isolation-Ebene, die die Informationen persistieren über die Anwendung Zugriff auf die Bezeichner in der Datei-Anbieter hat. Anzumerken ist, dass den Zugriff auf alle Dateien, die von dieser Ebene der Isolation gesteuert wird.

Das folgende Diagramm zeigt den Datenfluss bei der Verwendung von Lesezeichen und eine Erweiterung der Dokument-Anbieter:

 [![](document-picker-images/image40.png "Dieses Diagramm zeigt den Datenfluss bei der Verwendung von Lesezeichen und eine Dokument-Anbietererweiterung")](document-picker-images/image40.png#lightbox)

Der folgende Prozess durchgeführt:

1.  Die Anwendung in den Hintergrund wechselt und seinen Zustand beibehalten werden soll. Ruft `NSUrl` ein Lesezeichen in einer Datei in alternative Speicher zu erstellen.
1.  `NSUrl` Ruft die Dateierweiterung-Anbieter, um eine permanente URL für das Dokument abzurufen. 
1.  Die Dateierweiterung der Anbieter gibt die URL zurück, als eine Zeichenfolge, die die `NSUrl` .
1.  Die `NSUrl` bündelt Sie die URL in einem Lesezeichen, und gibt ihn an die Anwendung zurück.
1.  Wenn die Anwendung wird im Hintergrund awakes und benötigt, um den Status wiederherzustellen, übergibt er das Lesezeichen `NSUrl` .
1.  `NSUrl` Ruft die Dateierweiterung für die Anbieter mit der URL der Datei an.
1.  Dem Dateianbieter-Erweiterung greift auf die Datei und gibt den Speicherort der Datei, die `NSUrl` .
1.  Speicherort der Datei wird zusammen mit Informationen zur Sicherheit und an die Anwendung zurückgegeben.


Von hier aus kann die Anwendung Zugriff auf die Datei und damit wie gewohnt arbeiten.

### <a name="writing-files"></a>Schreiben von Dateien

In diesem Abschnitt dauert einen kurzen Blick auf das Schreiben von an einem alternativen Speicherort durch eine Dokument-Erweiterung funktioniert wie-Dateien. Die iOS-Anwendung wird Koordination der Datei verwenden, um Informationen auf dem Datenträger innerhalb des Containers für die Anwendung zu speichern. Kurz nach dem die Datei erfolgreich geschrieben wurde, werden die Dateierweiterung für den Anbieter der Änderung benachrichtigt.

An diesem Punkt können die Anbieter-Dateierweiterung starten Sie die Datei in den alternativen Speicherort Hochladen (oder markieren die Datei geändert und erfordern hochladen).

### <a name="creating-new-document-provider-extensions"></a>Erstellen neue Dokument-Anbietererweiterungen

Erstellen neue Dokument-Anbietererweiterungen ist außerhalb des Bereichs dieser einführenden Artikel. Diese Informationen dient hier um, die basierend auf den Erweiterungen anzuzeigen, die ein Benutzer in seinem iOS-Gerät geladen hat, eine Anwendung auf Dokumentspeicherorte außerhalb von Apple bereitgestellten iCloud Speicherort zugreifen kann.

Der Entwickler muss diese Tatsache bewusst sein, wenn die Dokumentauswahl mit und Verwenden von externen Dokumenten. Sie sollte nicht voraussetzen, diese Dokument in iCloud gehostet werden.

Weitere Informationen zum Erstellen eines Storage-Anbieter oder die Dokumentauswahl-Erweiterung finden Sie unter den [Einführung in die App-Erweiterungen](~/ios/platform/extensions.md) Dokument.

## <a name="migrating-to-icloud-drive"></a>Migrieren zu iCloud Drive

Unter iOS 8 können Benutzer wählen, um den Vorgang fortzusetzen, mithilfe der vorhandenen iCloud-Dokumente-System verwendet in iOS 7 (und früheren Systems) oder sie können vorhandene Dokumente zu der neuen iCloud Drive Mechanismus zu migrieren.

Unter Mac OS X Yosemite, Apple bietet keine die Abwärtskompatibilität Kompatibilität daher alle Dokumente in iCloud Drive migriert werden müssen oder sie nicht mehr werden aktualisiert werden, auf Geräten.

Nach dem Konto eines Benutzers in iCloud Drive migriert wurde, werden nur Geräte, die über die iCloud Drive können Änderungen an Dokumenten auf diesen Geräten weitergegeben werden.

> [!IMPORTANT]
> Entwickler sollten bedenken, dass die neuen Features, die in diesem Artikel beschriebenen sind nur verfügbar, wenn das Konto des Benutzers in iCloud Drive migriert wurde. 

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die Änderungen an vorhandenen iCloud behandelt, APIs, die zur Unterstützung von iCloud Laufwerk und das neue Dokument Picker-View-Controller erforderlich sind. Es wurden die Koordinierung der Datei, und warum es wichtig ist bei der Arbeit mit Cloud-basierten Dokumenten behandelt. Behandelt das Setup erforderlich, um Cloud-basierten Dokumenten in einer Xamarin.iOS-Anwendung zu aktivieren und erhält einen einführenden Einblick in die Arbeit mit Dokumenten außerhalb der app-Container der Anwendung, die mithilfe der Dokument-Auswahl-View-Controller.

Darüber hinaus beschrieben in diesem Artikel kurz, Dokument-Anbietererweiterungen und warum der Entwickler informiert sein sollten sie beim Schreiben von Anwendungen, die Cloud-basierte Dokumente verarbeiten kann.

## <a name="related-links"></a>Verwandte Links

- [DocPicker (Beispiel)](https://developer.xamarin.com/samples/monotouch/ios8/DocPicker/)
- [Einführung in iOS 8](~/ios/platform/introduction-to-ios8.md)
- [Einführung in die App-Erweiterungen](~/ios/platform/extensions.md)
