---
title: Dokument-Auswahl
description: Die Auswahl einer Dokument modellansichtcontroller gewährt Benutzerzugriff auf Dateien außerhalb einer Anwendung Sandkasten. Es ist ein einfacher Mechanismus zur Freigabe von Dokumenten zwischen apps. Außerdem können komplexere Workflows, da der Benutzer ein einzelnes Dokument mit mehreren Anwendungen bearbeiten können. Dieser Artikel enthält eine Einführung zur Verwendung der Dokument-Auswahl in einem Xamarin.iOS-Anwendung und die Änderungen in Dokumenten mit iCloud zu ihrer Unterstützung erforderlich.
ms.prod: xamarin
ms.assetid: 89539D79-BC6E-4A3E-AEC6-69D9A6CC6818
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: d9b98611c7d269e590ce6fe2ce0270ef71dacf1e
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="document-picker"></a>Dokument-Auswahl

_Die Auswahl einer Dokument modellansichtcontroller gewährt Benutzerzugriff auf Dateien außerhalb einer Anwendung Sandkasten. Es ist ein einfacher Mechanismus zur Freigabe von Dokumenten zwischen apps. Außerdem können komplexere Workflows, da der Benutzer ein einzelnes Dokument mit mehreren Anwendungen bearbeiten können. Dieser Artikel enthält eine Einführung zur Verwendung der Dokument-Auswahl in einem Xamarin.iOS-Anwendung und die Änderungen in Dokumenten mit iCloud zu ihrer Unterstützung erforderlich._

Die Auswahl einer Dokument können Dokumente apps gemeinsam genutzt werden. Diese Dokumente können in iCloud oder in eine andere app-Verzeichnis gespeichert werden. Dokumente werden über den Satz von freigegebenen [Dokument Anbieter Erweiterungen](~/ios/platform/extensions.md) hat der Benutzer auf seinem Gerät installiert. 

Aufgrund der schwierigkeiten Dokumente, die über apps und der Cloud synchronisiert halten führen sie eine bestimmte Menge an erforderlichen Komplexität.

## <a name="requirements"></a>Anforderungen

Folgendes ist erforderlich, die in diesem Artikel vorgestellten Schritte ausführen:

-  **Xcode 7 und iOS 8 oder neueren** – Apple Xcode 7 und iOS 8 oder neueren-APIs installiert und auf dem Computer des Entwicklers konfiguriert werden müssen.
-  **Visual Studio oder Visual Studio für Mac** – sollten die neueste Version von Visual Studio für Mac installiert werden.
-  **iOS-Gerät** – ein iOS-Gerät mit iOS 8 oder höher.

## <a name="changes-to-icloud"></a>Änderungen in icloud zulassen

Um die neuen Funktionen von der Auswahl einer Dokument zu implementieren, haben die folgenden Apple iCloud Dienst geändert wurde:

-  Die iCloud-Daemon wurde vollständig mit CloudKit umgeschrieben.
-  Die vorhandenen iCloud Funktionen wurden umbenannt iCloud Laufwerk.
-  Unterstützung für Microsoft Windows-Betriebssystems wurde in die icloud hinzugefügt.
-  Ein Ordner "iCloud" wurde im Mac OS Finder hinzugefügt.
-  iOS-Geräte können den Inhalt des Ordners iCloud Mac OS zugreifen.

> [!IMPORTANT]
> Apple [bietet Tools](https://developer.apple.com/support/allowing-users-to-manage-data/) Entwickler der Europäischen Union allgemeine Daten Schutz vor (GDPR) ordnungsgemäß verarbeiten können.

## <a name="what-is-a-document"></a>Was ist ein Dokument?

Beim Verweisen auf ein Dokument in iCloud ist eine einzelne, eigenständige-Entität und sollten daher vom Benutzer wahrgenommen werden. Ein Benutzer möglicherweise ändern möchten das Dokument oder für andere Benutzer freigeben, (per e-Mail, z. B.).

Es gibt mehrere Typen von Dateien, die der Benutzer sofort als Dokumente, wie z. B. Seiten Keynote oder Zahlen Dateien erkannt wird. ICloud ist jedoch nicht auf dieses Konzept beschränkt. Beispielsweise kann der Status eines Spiels (z. B. eine Übereinstimmung Chess) als ein Dokument behandelt und in iCloud gespeichert. Diese Datei kann zwischen Geräten des Benutzers übergeben werden und ermöglicht es ihnen, ein Spiel zu übernehmen, in denen aufgehört haben sie auf ein anderes Gerät.

## <a name="dealing-with-documents"></a>Umgang mit Dokumenten

Vor dem Einstieg in den Code erforderlich, um die Auswahl einer Dokument mit Xamarin verwenden, in diesem Artikel wird die bewährten Methoden zum Arbeiten mit Dokumenten mit iCloud abdecken, und einige der Änderungen an den vorhandenen APIs erforderlich, um die Auswahl einer Dokument unterstützen.

### <a name="using-file-coordination"></a>Verwenden die Datei Koordinierung

Da eine Datei an mehreren verschiedenen Stellen geändert werden kann, muss Koordinierung verwendet werden, um Datenverluste zu vermeiden.

 [![](document-picker-images/image1.png "Verwenden die Datei Koordinierung")](document-picker-images/image1.png#lightbox)

Werfen wir einen Blick auf die obige Abbildung:

1.  Datei Koordination mit einem iOS-Gerät ein neues Dokument erstellt und speichert sie in der iCloud-Ordner.
2.  iCloud speichert die geänderte Datei in die Cloud für die Verteilung auf jedem Gerät.
3.  Ein angefügtes Mac sieht die geänderte Datei in die iCloud Ordner und Koordination der Datei verwendet, kopieren Sie die Änderungen in der Datei.
4.  Ein Gerät nicht über Koordination der Datei werden eine Änderung an der Datei und speichert sie in der iCloud-Ordner. Diese Änderungen werden sofort in den anderen Geräten repliziert.

Angenommen die ursprüngliche iOS-Gerät oder dem Mac wurde die Datei bearbeiten jetzt ihre Änderungen verloren gehen und durch die Version der Datei, aus dem Unkoordinierte Gerät außer Kraft gesetzt werden. Um Datenverluste zu vermeiden, ist der Datei Koordinierung muss beim Arbeiten mit Cloud-basierte Dokumente.

### <a name="using-uidocument"></a>Verwenden von UIDocument

 `UIDocument` Aufgaben auf einfache Weise (oder `NSDocument` auf MacOS) weisen Sie alle von der Arbeit für den Entwickler. Bietet Datei zusammen mit Hintergrund Warteschlangen zum Blockieren der Benutzeroberfläche der Anwendung erstellt.

 `UIDocument` stellt mehrere allgemeine APIs, die die Entwicklung einer Xamarin-Anwendung zu vereinfachen, für alle Entwickler Zweck ist erforderlich.

Der folgende Code erstellt eine Unterklasse von `UIDocument` ein allgemeines textbasierte Dokument zu implementieren, die zum Speichern und Abrufen von Text aus icloud zulassen:

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

Die `GenericTextDocument` oben dargestellten Klasse wird in diesem Artikel verwendet werden, bei der Arbeit mit der Auswahl einer Dokument und externe Dokumente in einem Xamarin.iOS-8-Anwendung.

## <a name="asynchronous-file-coordination"></a>Asynchrone Datei-Koordination

iOS 8 stellt mehrere neue asynchrone Datei-Coordination-Funktionen über die neue Datei Koordinierung APIs bereit. Vor iOS 8 waren alle vorhandenen Datei Koordinierung APIs völlig synchron. Dies bedeutet, dass der Entwickler verantwortlich für die Implementierung ihrer eigenen Hintergrunds queuing, um zu verhindern, dass die Datei Koordination Benutzeroberfläches der Anwendung blockiert wurde.

Die neue `NSFileAccessIntent` Klasse enthält eine URL verweist auf die Datei und verschiedene Optionen zum Steuern der Art der Koordination erforderlich. Das folgende Codebeispiel veranschaulicht das Verschieben einer Datei von einem Speicherort zu einem anderen mithilfe von Intents:

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

Die Methode zum Ermitteln und Auflisten von Dokumenten wird mithilfe der vorhandenen `NSMetadataQuery` APIs. Dieser Abschnitt befasst sich mit neue Funktionen hinzugefügt `NSMetadataQuery` , das Arbeiten mit Dokumenten auch einfacher als zuvor vornehmen.

### <a name="existing-behavior"></a>Bereits bestehendem Verhalten

Bevor Sie iOS 8 `NSMetadataQuery` wurde z. B. pickup lokale Datei ändert sich nur langsam: Löscht, erstellt und umbenannt.

 [![](document-picker-images/image2.png "NSMetadataQuery lokale Datei Änderungen (Übersicht)")](document-picker-images/image2.png#lightbox)

In der obigen Abbildung:

1.  Für Dateien, die bereits in der Anwendungscontainer vorhanden `NSMetadataQuery` vorhanden `NSMetadata` Datensätze vorab erstellt und in die Warteschlange gestellt, damit sie sofort an die Anwendung verfügbar sind.
1.  Die Anwendung erstellt eine neue Datei im Container Anwendung.
1.  Verzögerung vor `NSMetadataQuery` erkennt die Änderung auf den Container für die Anwendung und erstellt die erforderlichen `NSMetadata` Datensatz.


Aufgrund der Verzögerung bei der Erstellung der der `NSMetadata` aufzuzeichnen, musste die Anwendung haben zwei Quellen zu öffnen: eine lokale Datei geändert und eine für Cloud-basierte Änderungen.

### <a name="stitching"></a>Zusammenfügen

In iOS 8 `NSMetadataQuery` ist einfacher, direkt mit der ein neues Feature namens zusammenfügen verwenden:

 [![](document-picker-images/image3.png "NSMetadataQuery durch ein neues Feature namens zusammenfügen")](document-picker-images/image3.png#lightbox)

Verwenden in der obigen Abbildung Zusammenfügen:

1.  Wie zuvor für Dateien, die bereits in der Anwendungscontainer vorhanden `NSMetadataQuery` vorhanden `NSMetadata` Datensätze vorab erstellt und in die Warteschlange gestellt.
1.  Die Anwendung erstellt eine neue Datei im Container Anwendung mithilfe der Datei Koordination.
1.  Die Änderung und die Aufrufe ein Hook in den Container für die Anwendung sieht `NSMetadataQuery` beim Erstellen der erforderlichen `NSMetadata` Datensatz.
1.  Die `NSMetadata` Datensatz wird direkt nach der Datei erstellt und die Anwendung zur Verfügung gestellt wird.


Verwenden zusammenfügen die Anwendung nicht mehr zum Öffnen einer Datenquelle zum lokalen überwachen und Cloud-basierten Änderungen der Datenbankdatei. Nachdem die Anwendung auf verlassen kann `NSMetadataQuery` direkt.

> [!IMPORTANT]
> Zusammenfügen funktioniert nur, wenn die Anwendung Koordination der Datei verwendet wird, wie im vorherigen Abschnitt beschrieben. Wenn Datei Koordinierung nicht verwendet wird, standardmäßig die APIs der vorhandenen vor iOS 8-Verhalten.




### <a name="new-ios-8-metadata-features"></a>Neue iOS 8-Metadaten, Funktionen

Die folgenden neuen Funktionen wurden hinzugefügt, um `NSMetadataQuery` in iOS 8:

-   `NSMetatadataQuery` können jetzt nicht lokaler Dokumente in der Cloud gespeicherte auflisten.
-  Neue APIs wurden hinzugefügt, auf die Metadateninformationen für die Cloud-basierte Dokumente zugreifen. 
-  Es gibt eine neue `NSUrl_PromisedItems` -API, die auf die Dateiattribute der Dateien, die möglicherweise möglicherweise ihre verfügbaren Inhalt nicht lokal verfügbar wird.
-  Verwenden Sie die `GetPromisedItemResourceValue` Methode zum Abrufen von Informationen zu einer bestimmten Datei oder der `GetPromisedItemResourceValues` Methode zum Abrufen von Informationen auf mehrere Dateien gleichzeitig zu einem Zeitpunkt.


Für den Umgang mit Metadaten wurden zwei neue Datei Koordinierung Flags hinzugefügt:

-   `NSFileCoordinatorReadImmediatelyAvailableMetadataOnly` 
-   `NSFileCoordinatorWriteContentIndependentMetadataOnly` 


Mit den oben genannten Flags müssen den Inhalt des Dokuments nicht lokal für möglich ist, werden sie zur Verfügung.

Das folgende Codesegment zeigt, wie `NSMetadataQuery` , das Vorhandensein einer bestimmten Datei abzufragen und die Datei zu erstellen, wenn er nicht vorhanden ist:

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

Apple idealer, dass die beste benutzerfreundlichkeit beim Auflisten von Dokumenten für eine Anwendung ist die Verwendung der Vorschau an. Dies bietet den Kontext für den End-Benutzer, damit sie schnell das Dokument identifiziert werden kann, dem sie verwenden möchten.

Vor dem iOS 8 erforderlich, mit der Vorschau von Dokumenten eine benutzerdefinierte Implementierung. Erfahrung mit iOS 8 werden die Attribute des Dateisystems, mit denen den Entwickler schnell mit Dokument-Miniaturansichten arbeiten können.

#### <a name="retrieving-document-thumbnails"></a>Abrufen von Dokument-Miniaturansichten 

Durch Aufrufen der `GetPromisedItemResourceValue` oder `GetPromisedItemResourceValues` Methoden `NSUrl_PromisedItems` -API, eine `NSUrlThumbnailDictionary`, zurückgegeben. Der einzige Schlüssel derzeit in diesem Wörterbuch ist die `NSThumbnial1024X1024SizeKey` und der entsprechenden `UIImage`.

#### <a name="saving-document-thumbnails"></a>Speichern von Dokument-Miniaturansichten

Die einfachste Möglichkeit zum Speichern einer Miniaturansicht wird mithilfe von `UIDocument`. Durch Aufrufen der `GetFileAttributesToWrite` Methode der `UIDocument` und Festlegen der Miniaturansicht, es wird automatisch gespeichert werden, wenn die Datei Dokument ist. ICloud Daemon diese Änderung sehen und in iCloud übertragen. Unter Mac OS X werden Miniaturansichten für den Entwickler durch das Plug-in schnelle Suchen automatisch generiert.

Mit den Grundlagen der Arbeit mit iCloud-basierte Dokumente eingerichtet ist, zusammen mit den Änderungen an vorhandenen API bereit, das Dokument Picker-View-Controller in einer Xamarin iOS 8 implementiert werden Mobile-Anwendung.


## <a name="enabling-icloud-in-xamarin"></a>Aktivieren der iCloud in Xamarin

Bevor die Auswahl einer Dokument in einem Xamarin.iOS-Anwendung verwendet werden kann, muss iCloud-Unterstützung in der Anwendung sowohl über Apple aktiviert werden. 

Die folgenden Schritte Exemplarische Vorgehensweise den Prozess der Bereitstellung für iCloud.

1. Erstellen Sie ein iCloud-Container.
2. Erstellen Sie eine App-ID, die die iCloud Anwendungsdiensts enthält.
3. Erstellen Sie ein Provisioning-Profil, die diese App ID.

Die [arbeiten mit Funktionen](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md) Leitfaden werden die ersten beiden Schritte. Um eine provisioning-Profil zu erstellen, führen Sie die Schritte in der [Bereitstellungsprofil](~/ios/get-started/installation/device-provisioning/index.md#Provisioning_Profile) Handbuch.



Die folgenden Schritte Exemplarische Vorgehensweise den Konfigurationsprozess für Ihre Anwendung für icloud zulassen:

Führen Sie folgende Schritte aus:

1.  Öffnen Sie das Projekt in Visual Studio für Mac oder Visual Studio.
2.  In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie Optionen aus.
3.  Wählen Sie im Dialogfeld Optionen **iOS-Anwendung**, sicher, dass die **Paket-ID** entspricht, die in definiert wurde **App-ID** oben für die Anwendung erstellt. 
4.  Wählen Sie **iOS Bundle Signing**, wählen die **Developer Identität** und die **Bereitstellungsprofil** oben erstellt.
5.  Klicken Sie auf die **OK** Schaltfläche, um die Änderungen zu speichern und das Dialogfeld zu schließen.
6.  Mit der rechten Maustaste auf `Entitlements.plist` in der **Projektmappen-Explorer** um ihn im Editor zu öffnen.

    > [!IMPORTANT]
    > In Visual Studio müssen Sie möglicherweise zu den Berechtigungen-Editor zu öffnen, indem Sie mit der rechten Maustaste darauf, Auswahl **Öffnen mit...** Klicken und Auswählen der Eigenschaft Testlisten-Editor

7.  Überprüfen Sie **aktivieren iCloud** , **iCloud Dokumente** , **Schlüssel-Wert-Speicher** und **CloudKit** .
8.  Sicherstellen der **Container** für die Anwendung vorhanden ist, (wie oben erstellt haben). Ein Beispiel: `iCloud.com.your-company.AppName`
9.  Speichern Sie die Änderungen in der Datei.

Weitere Informationen zu Berechtigungen finden Sie in der [arbeiten mit Berechtigungen](~/ios/deploy-test/provisioning/entitlements.md) Handbuch.

Mit den oben genannten Setup vorhanden kann die Anwendung jetzt Cloud-basierte Dokumente und das neue Dokument Picker-View-Controller verwenden.

## <a name="common-setup-code"></a>Allgemeine Setupcode

Bevor Sie beginnen mit dem Dokument Picker-View-Controller, es gibt einige standard Setupcode erforderlich. Starten, indem Sie der Anwendungsverzeichnis `AppDelegate.cs` Datei, und stellen sie wie folgt aussehen:

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
> Der obige Code enthält den Code aus dem obigen Abschnitt Ermitteln von und Auflisten von Dokumenten. Er wird hier als Ganzes angezeigt, wie es in einer echten Anwendung angezeigt wird. Zur Vereinfachung dieses Beispiel funktioniert mit einer einzelnen, hartcodierte Datei (`test.txt`) nur.

Der obige Code macht mehrere iCloud Laufwerk Tastenkombinationen, um sie leichter mit dem Rest der Anwendung arbeiten können.

Als Nächstes fügen Sie den folgenden Code, um jede Sicht oder den Container anzeigen, die mithilfe der Auswahl des Dokuments oder Arbeiten mit Cloud-basierte Dokumente:

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

Dadurch wird eine Verknüpfung zum Abrufen der `AppDelegate` und Zugriff auf die oben erstellten Verknüpfungen mit der iCloud.

Mit diesem Code werden werfen wir einen Blick auf das Dokument Picker-View-Controller in einer Xamarin iOS 8-Anwendung implementieren.

## <a name="using-the-document-picker-view-controller"></a>Verwenden das Dokument Picker-View-Controller

Bevor Sie iOS 8 war es sehr schwierig ist, Dokumente aus einer anderen Anwendung zugegriffen werden, da es keine Möglichkeit gab, Dokumente, die außerhalb der Anwendung von innerhalb der app zu ermitteln.

### <a name="existing-behavior"></a>Bereits bestehendem Verhalten

 [![](document-picker-images/image31.png "Übersicht über die vorhandenen Verhalten")](document-picker-images/image31.png#lightbox)

Werfen wir einen Blick auf den Zugriff auf ein externes Dokument vor iOS 8:

1.  Zuerst würde der Benutzer verfügen, um die Anwendung zu öffnen, die das Dokument ursprünglich erstellt hat.
1.  Das Dokument ausgewählt ist und die `UIDocumentInteractionController` wird verwendet, um das Dokument an die neue Anwendung zu senden.
1.  Zum Schluss wird eine Kopie des ursprünglichen Dokuments im Container für die neue Anwendung platziert.


Von dort aus wird das Dokument verfügbar, für die zweite Anwendung öffnen und bearbeiten.

### <a name="discovering-documents-outside-of-an-apps-container"></a>Ermittlung von Dokumenten außerhalb einer App-Container

In iOS 8 ist eine Anwendung auf Dokumente außerhalb der eigenen Anwendungscontainer problemlos zugreifen:

 [![](document-picker-images/image32.png "Ermittlung von Dokumenten außerhalb einer App-Container")](document-picker-images/image32.png#lightbox)

Über die die neue iCloud Dokument Zeitauswahl ( `UIDocumentPickerViewController`), eine iOS-Anwendung direkt ermitteln und außerhalb des Containers für die Anwendung zugreifen kann. Die `UIDocumentPickerViewController` bietet ein Mechanismus für den Benutzer zum Gewähren von Zugriff auf und bearbeiten die Dokumente über Berechtigungen ermittelt.

Eine Anwendung muss opt-in, um haben die Dokumenten in iCloud Dokument Farbauswahl angezeigt und stehen dann für andere Anwendungen zu ermitteln und mit ihnen arbeiten. Haben Sie eine Xamarin iOS 8-Anwendung Containers Anwendung freigeben, bearbeiten sie `Info.plist` Datei in einem standard-Text-Editor, und fügen Sie die folgenden zwei Zeilen an das Ende des Wörterbuchs (zwischen der `<dict>...</dict>` Tags):

```xml
<key>NSUbiquitousContainerIsDocumentScopePublic</key>
<true/>
```

Die `UIDocumentPickerViewController` bietet eine hervorragende neue Benutzeroberfläche, die dem Benutzer ermöglicht, Dokumente zu wählen. Um das Dokument Picker-View-Controller in einer Xamarin iOS 8-Anwendung anzuzeigen, führen Sie folgende Schritte aus:

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
> Der Entwickler muss Aufrufen der `StartAccessingSecurityScopedResource` Methode der `NSUrl` vor ein externes Dokument zugegriffen werden kann. Die `StopAccessingSecurityScopedResource` Methode muss aufgerufen werden, um die Sicherheit-Sperre aufzuheben, sobald das Dokument geladen wurde.

### <a name="sample-output"></a>Beispielausgabe

Hier ist ein Beispiel wie oben stehenden Code eine Dokument-Auswahl bei Ausführung auf einem iPhone-Gerät anzeigen würden:

1.  Der Benutzer startet die Anwendung aus, und die Hauptkomponente der Benutzeroberfläche wird angezeigt:   
 
    [![](document-picker-images/image33.png "Die Hauptkomponente der Benutzeroberfläche wird angezeigt.")](document-picker-images/image33.png#lightbox)
1.  Die Benutzer-Taps der **Aktion** Schaltfläche am oberen Rand des Bildschirms und wird gebeten, wählen Sie eine **Dokument Anbieter** aus der Liste der verfügbaren Anbieter:   
 
    [![](document-picker-images/image34.png "Wählen Sie aus der Liste der verfügbaren Anbieter einen Dokument-Anbieter")](document-picker-images/image34.png#lightbox)
1.  Die **Dokument Picker-View-Controller** wird angezeigt, für den ausgewählten **Dokument Anbieter**:   
 
    [![](document-picker-images/image35.png "Das Dokument Picker-View-Controller wird angezeigt.")](document-picker-images/image35.png#lightbox)
1.  Der Benutzer tippt auf eine **Dokumentordner** um seinen Inhalt anzuzeigen:   
 
    [![](document-picker-images/image36.png "Inhalt des Dokuments")](document-picker-images/image36.png#lightbox)
1.  Der Benutzer wählt ein **Dokument** und **Dokument Auswahl** geschlossen wird.
1.  Die Hauptkomponente der Benutzeroberfläche wird erneut angezeigt, die **Dokument** geladen wird, aus dem externen Container und dessen Inhalt angezeigt.


Die tatsächliche Anzeige des Dokuments Picker-View-Controller richtet sich nach der Dokument-Anbieter, dass der Benutzer auf dem Gerät installiert hat und welche Auswahl Dokumentmodus implementieren wurde. Im obigen Beispiel wird mithilfe der Open-Modus, die anderen Modus Typen werden im folgenden ausführlich besprochen werden.

## <a name="managing-external-documents"></a>Verwalten von externen Dokumenten

Wie oben beschrieben, bevor Sie iOS 8, konnte eine Anwendung nur Dokumente zugreifen, die ein Teil des Containers Anwendung enthalten waren. In iOS 8 kann eine Anwendung, Dokumente aus externen Quellen zugreifen:

 [![](document-picker-images/image37.png "Verwalten von externen Dokumente – Übersicht")](document-picker-images/image37.png#lightbox)

Wenn der Benutzer ein Dokument aus einer externen Quelle auswählt, wird ein Dokument Verweis auf den Container für die Anwendung geschrieben, die auf das Originaldokument verweist.

Um Unterstützung bei der Hinzufügen dieser neuen Funktion in vorhandene Anwendungen, wurden mehrere neue Funktionen hinzugefügt die `NSMetadataQuery` API. In der Regel verwendet eine Anwendung, die weit verbreitete Umfang des Dokuments zum Auflisten von Dokumenten, die innerhalb des Containers Anwendung vorhanden. Verwenden diesen Bereich, werden nur Dokumente innerhalb des Containers für die Anwendung weiterhin angezeigt werden.

Verwenden den neuen weit verbreitete externen Dokument Bereich gibt Dokumente zurück, die außerhalb der Anwendungscontainer befinden, und die Metadaten für diese zurückgeben. Die `NSMetadataItemUrlKey` zeigen auf die URL, in dem das Dokument tatsächlich gefunden wird.

In einigen Fällen keine Anwendung mit den Dokumenten Verweisübergabe ten auf arbeiten möchten. Stattdessen möchte die app direkt mit dem Verweis-Dokument zu arbeiten. Beispielsweise kann die app auf das Dokument in den Ordner der Anwendung in der Benutzeroberfläche anzeigen oder die Verweise in einen Ordner verschieben, um dem Benutzer ermöglichen soll.

In iOS 8 ein neues `NSMetadataItemUrlInLocalContainerKey` wurde bereitgestellt, damit die Reference-Dokuments direkt zugreifen. Dieser Schlüssel verweist auf den tatsächlichen Verweis auf das externe Dokument in einem Container für die Anwendung.

Die `NSMetadataUbiquitousItemIsExternalDocumentKey` wird verwendet, um zu testen, und zwar unabhängig davon, ob ein Dokument in einem Anwendungsverzeichnis Container extern ist. Die `NSMetadataUbiquitousItemContainerDisplayNameKey` wird verwendet, um den Namen des Containers zugreifen, die die ursprüngliche Kopie von einem externen Dokument Gehäuse ist.

### <a name="why-document-references-are-required"></a>Warum Dokumentverweise erforderlich sind.

Die Hauptgründe dafür, dass der Zugriff auf externe Dokumente verwendet, iOS 8 ist die Sicherheit. Keine Anwendung wird Zugriff auf eine andere Anwendung Container gewährt. Nur die Auswahl einer Dokument möglich, dass, da ausgeführten Out-of-Process und System Breiten Zugriff.

Die einzige Möglichkeit, um ein Dokument außerhalb der Anwendungscontainer erhalten wird, mit der Auswahl einer Dokument, und, wenn die URL, durch zurückgegeben die Auswahl ist Sicherheit beschränkt. Die Sicherheit im Bereich einer URL enthält nur genügend Informationen, um das Dokument zusammen mit der bereichsbezogenen Rechte, die erforderlich sind, der Zugriff auf das Dokument eine Anwendung ausgewählt.

Es ist wichtig zu beachten, dass die Sicherheit im Bereich einer URL wurde in eine Zeichenfolge dann serialisiert und deserialisiert, die Sicherheitsinformationen verloren würde und die Datei aus der URL zugegriffen werden. Die Dokumentverweis-Funktion bietet einen Mechanismus, um wieder auf die Dateien auf den folgenden URLs zugreifen.

Wenn die Anwendung erhält eine `NSUrl` aus einem der Referenzdokumente, er verfügt bereits über den Sicherheitsbereich angefügt und kann verwendet werden, um den Zugriff auf die Datei. Aus diesem Grund wird dringend vorgeschlagen, dass der Entwickler `UIDocument` daran, dass sie alle diese Informationen und Prozesse für sie übernimmt.

### <a name="using-bookmarks"></a>Verwenden von Lesezeichen

Es ist nicht immer möglich, eine Anwendung Dokumente zu einem bestimmten Dokument, z. B. bei der Wiederherstellung des Benutzerzustand aufgelistet werden. iOS 8 bietet einen Mechanismus zum Erstellen von Lesezeichen, die direkt ein bestimmten Dokument abzielen.

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

Die vorhandene Lesezeichen-API wird verwendet, um ein Lesezeichen für eine vorhandene erstellen `NSUrl` , gespeichert und zum ermöglichen von direktem Zugriff auf eine externe Datei geladen werden können. Im folgende Code wird ein Lesezeichen wiederhergestellt, die oben erstellt wurde:

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

## <a name="open-vs-import-mode-and-the-document-picker"></a>Öffnen Sie Visual Studio. Modus "Import" und die Auswahl einer Dokument

Das Dokument Picker-View-Controller verfügt über zwei unterschiedliche Verwendungsarten des:

1.  **Öffnen Sie im Modus** – In diesem Modus, wenn der Benutzer auswählt und externen Dokument die Auswahl einer Dokument Sicherheit im Bereich einer Textmarke im Container Anwendung erstellen wird.   
 
    [![](document-picker-images/image37.png "Ein Sicherheitstoken im Bereich einer Textmarke im Anwendungscontainer")](document-picker-images/image37.png#lightbox)
1.  **Importmodus** – In diesem Modus, wenn der Benutzer auswählt und externen Dokument, das Dokument Auswahl kein Lesezeichen erstellt, aber stattdessen kopieren Sie die Datei in einen temporären Speicherort und Bereitstellen den Anwendungszugriff auf das Dokument an diesem Speicherort:   
 
    [![](document-picker-images/image38.png "Die Auswahl einer Dokument wird kopieren Sie die Datei in einen temporären Speicherort und Bereitstellen die Anwendungszugriff auf dem Dokument an dieser Stelle")](document-picker-images/image38.png#lightbox)   
 Sobald die Anwendung aus irgendeinem Grund beendet wird, dem temporären Verzeichnis geleert wird, und die Datei entfernt. Wenn die Anwendung für den Zugriff auf die Datei muss, sollte er eine Kopie und fügen Sie ihn in den Container für die Anwendung.


Die Open-Modus ist nützlich, wenn die Anwendung mit einer anderen Anwendung zusammenarbeiten und alle Änderungen, die dem Dokument mit dieser Anwendung freigeben möchte. Der Modus "Import" wird verwendet, wenn die Anwendung nicht ihre Änderungen zu einem Dokument mit anderen Anwendungen gemeinsam möchte.

## <a name="making-a-document-external"></a>Erstellen ein Dokument externe

Wie oben bereits erwähnt, wird eine iOS 8-Anwendung keinen Zugriff auf Container außerhalb der eigenen Anwendung-Container. Die Anwendung kann lokal oder in ein temporäres Verzeichnis sein eigener Container schreiben, verwenden Sie einen spezielle Dokumentmodus resultierende Dokument außerhalb der Anwendungscontainer für einen Benutzer mit dem ausgewählten Speicherort verschoben.

Um ein Dokument in einem externen Speicherort zu verschieben, führen Sie folgende Schritte aus:

1.  Erstellen Sie zunächst ein neues Dokument in einem lokalen oder temporären Speicherort.
1.  Erstellen einer `NSUrl` , der auf das neue Dokument verweist.
1.  Öffnen Sie ein neues Dokument Picker-View-Controller, und übergeben sie die `NSUrl` mit dem Modus der `MoveToService` . 
1.  Sobald der Benutzer einen neuen Speicherort auswählt, wird das Dokument von seinem aktuellen Speicherort auf den neuen Speicherort verschoben werden.
1.  Eines Referenzdokuments werden in der app-Anwendungscontainer geschrieben, damit die Datei immer noch zugegriffen werden kann, können Sie von der Anwendung erstellen.


Der folgende Code kann verwendet werden, um ein Dokument in einem externen Speicherort zu verschieben: `var picker = new UIDocumentPickerViewController (srcURL, UIDocumentPickerMode.MoveToService);`

Die von den oben aufgeführten Vorgang zurückgegebenen Referenzdokumente ist identisch als Open-Modus von der Auswahl einer Dokument erstellt. Es gibt jedoch Fälle, in denen die Anwendung ein Dokument zu verschieben, ohne einen Verweis darauf beibehalten möchten.

Um ein Dokument ohne beim Generieren eines Verweises zu verschieben, verwenden die `ExportToService` Modus. Ein Beispiel: `var picker = new UIDocumentPickerViewController (srcURL, UIDocumentPickerMode.ExportToService);`

Bei Verwendung der `ExportToService` -Modus wird das Dokument in den externen Container kopiert und die vorhandene Kopie in ihrem ursprünglichen Speicherort bleibt.

## <a name="document-provider-extensions"></a>Dokument-Anbieter-Erweiterungen

Mit iOS 8 möchte Apple der Endbenutzer in der Lage, auf ihre Cloud-basierte Dokumente, unabhängig davon, wo sie tatsächlich vorhanden sind. Um dieses Ziel zu erreichen, stellt iOS 8 einen neuen Dokument-Anbietererweiterung-Mechanismus.

### <a name="what-is-a-document-provider-extension"></a>Was ist eine Erweiterung des Dokuments Anbieter?

Einfach ausgedrückt, ist eine Erweiterung der Dokument-Anbieter eine Möglichkeit für ein Entwickler oder ein Drittanbieter-um eine alternative Dokument Anwendungsspeicher bereitzustellen, die auf genau dieselbe Weise als Speicherort für vorhandene iCloud zugegriffen werden kann.

Der Benutzer kann eine der folgenden alternativen Speicherorte aus der Auswahl einer Dokument auswählen und sie können genau gleichen Zugriffsmodi (öffnen, importieren, verschieben oder Export), zum Arbeiten mit Dateien an diesem Speicherort.

Dies wird mithilfe von zwei verschiedenen Erweiterungen implementiert:

-  **Dokumentieren Sie die Auswahl einer Erweiterung** – bietet eine `UIViewController` Unterklasse, die eine grafische Benutzeroberfläche für den Benutzer zur Auswahl eines Dokuments eines alternativen Speicherort bereitstellt. Diese Unterklasse wird als Teil eines Dokuments Picker-View-Controller angezeigt werden.
-  **Geben Sie den/die Dateierweiterung** – Dies ist eine nicht-UI-Erweiterung, der die Bereitstellung von tatsächlich den Inhalt der Dateien behandelt. Diese Erweiterungen werden durch Datei Koordinierung bereitgestellt ( `NSFileCoordinator` ). Dies ist eine andere wichtige Fall, wenn die Datei Koordinierung erforderlich ist.


Das folgende Diagramm zeigt die typischer Datenfluss bei der Arbeit mit Dokument Anbieter Erweiterungen:

 [![](document-picker-images/image39.png "Dieses Diagramm zeigt typische Datenfluss bei der Arbeit mit Dokument Anbieter Erweiterungen")](document-picker-images/image39.png#lightbox)

Der folgende Prozess tritt:

1.  Die Anwendung zeigt ein Dokument Datumsauswahl Controller, damit der Benutzer eine Datei zur Bearbeitung auswählen kann.
1.  Der Benutzer wählt ein alternativer Speicherort und den benutzerdefinierten `UIViewController` Erweiterung wird aufgerufen, um die Benutzeroberfläche anzuzeigen.
1.  Der Benutzer wählt aus einer Datei von diesem Speicherort aus, und die URL zurück an die Auswahl einer Dokument übergeben wird.
1.  Die Auswahl einer Dokument wählt die Datei-URL und gibt ihn an die Anwendung für den Benutzer arbeiten auf zurück.
1.  Die URL übergeben wird an einen übergeordneten Koordinator der Datei den Inhalt der Dateien an die Anwendung zurückgegeben.
1.  Der Koordinator Datei ruft die benutzerdefinierte Erweiterung der Datei-Anbieter zum Abrufen der das.
1.  Der Inhalt der Datei werden an einen übergeordneten Koordinator der Datei zurückgegeben.
1.  Der Inhalt der Datei werden an die Anwendung zurückgegeben.


### <a name="security-and-bookmarks"></a>Sicherheit und Lesezeichen

In diesem Abschnitt dauert auf einen Blick darauf über Lesezeichen funktioniert mit Dokument Anbieter Erweiterungen wie Sicherheit und persistenten Datei zugreifen. Im Gegensatz zu iCloud Dokument-Anbieter, der Sicherheit und Lesezeichen auf den Container für die Anwendung automatisch speichert, nicht Dokument Anbieter Erweiterungen, da sie keinen Teil des Systems Verweis Dokument sind.

Zum Beispiel: in einer Organisation, die eine eigene sichere unternehmensweiten-Datenspeicher bereitstellt, möchte Administratoren nicht vertrauliche Unternehmensdaten zugegriffen oder von der öffentlichen iCloud Server verarbeitet. Aus diesem Grund kann das integrierte Dokument Reference-System verwendet werden.

Die Lesezeichen-System kann weiterhin verwendet werden, und es liegt in der Verantwortung der Dateierweiterung Anbieter ordnungsgemäß verarbeiten einer mit Lesezeichen versehene URL und den Inhalt des Dokuments verweist es zurück.

Aus Sicherheitsgründen wurde iOS 8 eine Isolationsschicht, die die Informationen werden dauerhaft darüber, die welche Anwendung den Zugriff auf die Bezeichner in welche Dateisystemanbieter verfügt. Beachten Sie, dass der Zugriff auf alle Dateien von diesem Isolationsschicht gesteuert wird.

Das folgende Diagramm zeigt den Datenfluss beim Arbeiten mit Lesezeichen und eine Erweiterung der Dokument-Anbieter:

 [![](document-picker-images/image40.png "Dieses Diagramm zeigt den Datenfluss beim Arbeiten mit Lesezeichen und eine Erweiterung der Dokument-Anbieter")](document-picker-images/image40.png#lightbox)

Der folgende Prozess tritt:

1.  Die Anwendung wird gleich Geben Sie den Hintergrund und muss seinen Zustand beibehält. Sie ruft `NSUrl` ein Lesezeichen in einer Datei in alternative Storage erstellt.
1.  `NSUrl` Ruft die Dateierweiterung Anbieter, um eine permanente URL für das Dokument zu erhalten. 
1.  Die Dateierweiterung der Anbieter die URL zurückgegeben, als Zeichenfolge an die `NSUrl` .
1.  Die `NSUrl` bündelt die URL in einem Lesezeichen und an die Anwendung zurückgegeben.
1.  Wenn die Anwendung wird im Hintergrund awakes und muss Zustand wiederherzustellen, übergibt er das Lesezeichen `NSUrl` .
1.  `NSUrl` Ruft die Anbieter-Dateierweiterung durch die URL der Datei an.
1.  Datei Erweiterungsanbieters greift auf die Datei und gibt den Speicherort der Datei `NSUrl` .
1.  Speicherort der Datei ist mit Sicherheitsinformationen gebündelt und an die Anwendung zurückgegeben.


Von hier aus kann die Anwendung auf die Datei zugreifen und mit ihm gearbeitet wird, wie gewohnt.

### <a name="writing-files"></a>Schreiben von Dateien

In diesem Abschnitt dauert auf einen Blick darauf wie Schreiben an einem alternativen Speicherort mit einem Dokument Anbietererweiterung funktioniert Dateien. IOS-Anwendung wird Datei Koordinierung verwenden, um Informationen auf den Datenträger innerhalb des Containers für die Anwendung zu speichern. Kurz nach dem die Datei erfolgreich geschrieben wurde, werden die Dateierweiterung für den Anbieter der Änderung benachrichtigt.

An diesem Punkt können die Anbieter-Dateierweiterung starten Hochladen der Datei auf den alternativen Speicherort (oder markieren die Datei geändert und erfordern hochladen).

### <a name="creating-new-document-provider-extensions"></a>Erstellen von neuen Dokument Anbieter-Erweiterungen

Erstellen von neuen Dokument Anbieter Erweiterungen ist außerhalb des Bereichs dieser einführende Artikel. Diese Informationen sind bereitgestellten hier um anzugeben, dass, basierend auf der Erweiterungen, die ein Benutzer in seinem iOS-Gerät geladen hat, eine Anwendung Zugriff auf Dokument Speicherorte außerhalb der Apple iCloud Speicherort bereitgestellt haben kann.

Entwickler sollten diese Tatsache beachtet werden, bei der Auswahl einer Dokument mithilfe von und Arbeiten mit externen Dokumente. Sie sollten nicht vorausgesetzt, diese Dokuments in iCloud gehostet werden.

Weitere Informationen zum Erstellen eines Speicheranbieter oder der Auswahl einer Erweiterung Dokuments finden Sie unter der [Einführung in die App-Erweiterungen](~/ios/platform/extensions.md) Dokument.

## <a name="migrating-to-icloud-drive"></a>Migrieren zu iCloud Laufwerk

IOS 8 können Benutzer wählen, um den Vorgang fortzusetzen, über die vorhandenen iCloud Dokumente System, die in iOS 7 (und vorherige Systeme) verwendet oder können sie zum Migrieren von vorhandenen Dokumente zu den neuen iCloud Laufwerkmechanismus auswählen.

Für Mac OS X Yosemite Apple bietet die Abwärtskompatibilität Kompatibilität daher alle Dokumente in iCloud Laufwerk migriert werden müssen oder sie nicht mehr werden aktualisiert werden, auf Geräten verfügbar sind.

Nachdem Sie dem Konto eines Benutzers in die icloud Laufwerk migriert wurde, werden können zum Weitergeben von Änderungen auf Dokumente auf diesen Geräten nur Geräte, die über die iCloud Laufwerk.

> [!IMPORTANT]
> Entwickler sollten bewusst sein, dass die neuen Funktionen, die in diesem Artikel beschriebene sind nur verfügbar, wenn das Konto des Benutzers in die icloud Laufwerk migriert wurde. 

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die Änderungen an vorhandenen iCloud behandelt, die APIs zur Unterstützung der iCloud Laufwerk und das neue Dokument Picker-View-Controller erforderlich. Es wurden Datei Koordination und warum es wichtig ist bei der Arbeit mit Cloud-basierte Dokumente behandelt. Es weist behandelt das Setup erforderlich, um Cloud-basierte Dokumente in einem Xamarin.iOS-Anwendung zu aktivieren und erhält eine einführende Betrachtung der Arbeiten mit Dokumenten außerhalb einer app Anwendungscontainer mit dem Dokument Picker-View-Controller.

Darüber hinaus behandelt in diesem Artikel kurz Dokument Anbieter Erweiterungen und warum der Entwickler berücksichtigen sollten sie beim Schreiben von Anwendungen, die Cloud-basierte Dokumente verarbeiten kann.

## <a name="related-links"></a>Verwandte Links

- [DocPicker (Beispiel)](https://developer.xamarin.com/samples/monotouch/ios8/DocPicker/)
- [Einführung in iOS 8](~/ios/platform/introduction-to-ios8.md)
- [Einführung in die App-Erweiterungen](~/ios/platform/extensions.md)
