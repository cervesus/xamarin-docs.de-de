---
title: Über die iCloud mit Xamarin.iOS
description: Dieses Dokument beschreibt die iCloud und deren Verwendung bei Xamarin.iOS-Anwendungen. Es wird erläutert, Schlüssel-Wert-Speicher, Speicherung von Dokumenten und iCloud-Sicherung.
ms.prod: xamarin
ms.assetid: C6F3B87C-C195-4434-EF14-D66E63894F09
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/09/2016
ms.openlocfilehash: 009e061726f655999c08192b5839a5c962d35e24
ms.sourcegitcommit: c4be32ef914465e808d89767c4d5ee72afe93cc6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2019
ms.locfileid: "58855093"
---
# <a name="using-icloud-with-xamarinios"></a>Über die iCloud mit Xamarin.iOS

Die iCloud-Speicher-API unter iOS 5 kann Benutzerdokumente und anwendungsspezifische Daten an einem zentralen Ort speichern und diese Elemente über Geräte des Benutzers zugreifen.

Es gibt vier Arten von Speicher zur Verfügung:

- **Schlüssel-Wert-Speicher** – Sie kleine Mengen von Daten mit Ihrer Anwendung auf der anderen Geräte eines Benutzers.

- **UIDocument-Speicher** – Dokumente und andere Daten in die iCloud-Konto des Benutzers mit der eine Unterklasse von UIDocument zu speichern.

- **CoreData** -SQLite-Datenbank-Speicher.

- **Einzelne Dateien und Verzeichnisse** – für die Verwaltung von vielen verschiedenen Dateien direkt in das Dateisystem.

Dieses Dokument erläutert die ersten beiden Typen – Schlüssel-Wert-Paare und UIDocument Unterklassen – und wie diese Features in Xamarin.iOS verwendet.

> [!IMPORTANT]
> Apple [stellt Tools zur Verfügung](https://developer.apple.com/support/allowing-users-to-manage-data/), die Entwickler dabei unterstützen, die Datenschutz-Grundverordnung (DSGVO) der Europäischen Union umzusetzen.

## <a name="requirements"></a>Anforderungen

- Die neueste stabile Version von Xamarin.iOS
- Xcode-10
- Visual Studio für Mac oder Visual Studio-2019.

## <a name="preparing-for-icloud-development"></a>Vorbereitung für die iCloud-Entwicklung

Anwendungen müssen konfiguriert werden, zur Verwendung von iCloud in die [Apple-Bereitstellungsportal](https://developer.apple.com/account/ios/overview.action) und das Projekt selbst. Vor dem Entwickeln für iCloud (oder sich die Beispiele) die folgenden Schritte ausführen.

Um eine Anwendung auf iCloud ordnungsgemäß zu konfigurieren:

-   **Suchen Ihrer TeamID** -melden Sie sich [developer.apple.com](https://developer.apple.com) , und besuchen Sie die **Member Center > Your Account > Developer Account Summary** wurde Ihr Team-ID (oder einzelne-ID für einzelne Entwickler ). Sie werden eine 10-stellige Zeichenfolge ( **A93A5CM278** z. B.) – Dies ist Teil der Bezeichner"Container".

-   **Erstellen Sie eine neue App-ID** : Informationen zum Erstellen einer App-ID, befolgen die Schritte der [Bereitstellung für Store-Technologien-Abschnitt des Handbuchs Gerätebereitstellung](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md), und sehen Sie sich **iCloud** als ein zulässige-Dienst:

 [![](introduction-to-icloud-images/icloud-sml.png "Überprüfen von iCloud als zulässige-Dienst")](introduction-to-icloud-images/icloud.png#lightbox)

- **Erstellen eines neuen Bereitstellungsprofils** : Informationen zum Erstellen eines Bereitstellungsprofils, befolgen die Schritte der [Device Provisioning-Handbuch](~/ios/get-started/installation/device-provisioning/index.md#provisioning-your-device) .

- **Fügen Sie den Bezeichner des Containers zu "Entitlements.plist"** -Format für die Container-ID ist `TeamID.BundleID`. Weitere Informationen finden Sie in der [arbeiten mit Berechtigungen](~/ios/deploy-test/provisioning/entitlements.md) Guide.

- **Konfigurieren Sie die Projekteigenschaften** – In der Datei "Info.plist" Datei stellen Sie sicher die **Bündel-ID** entspricht der **Bündel-ID** festgelegt, wenn [erstellen eine App-ID ](~/ios/deploy-test/provisioning/capabilities/index.md); IOS Bundle-Signierung verwendet eine **Bereitstellungsprofil** , die eine App-ID mit der iCloud-App Service enthalten und die **benutzerdefinierte Berechtigungen** Datei ausgewählt. Dies kann alle in Visual Studio unter dem Bereich der Projekt-Eigenschaften erfolgen.

- **Aktivieren von iCloud auf Ihrem Gerät** : Wechseln Sie zu **Einstellungen > iCloud** und stellen Sie sicher, dass das Gerät angemeldet ist.
Wählen Sie aus, und aktivieren Sie die **Dokumente und Daten** Option.

- **Sie müssen ein Gerät verwenden, zum Testen von iCloud** – es funktioniert nicht auf dem Simulator.
In der Tat benötigen Sie eigentlich zwei oder mehr Geräte alle dieselbe Apple-ID angemeldet sind, um die iCloud in Aktion finden Sie unter.


## <a name="key-value-storage"></a>Schlüssel-Wert-Speicher

Schlüssel-Wert-Speicher ist für kleine Mengen von Daten vorgesehen, die ein Benutzer hinweg beibehaltene Geräten – z. B. die letzte Seite interessant sein könnten, die sie in einem Buch oder Magazine angezeigt. Schlüssel-Wert-Speicher sollte nicht für die Sicherung von Daten verwendet werden.

Es gibt einige Einschränkungen bei Verwendung von Schlüssel-Wert-Speicher berücksichtigen:

- **Größte gültige Schlüsselgröße** -Schlüsselnamen darf nicht länger als 64 Bytes sein.

- **Maximale Größe** -mehr als 64 KB in einem einzelnen Wert kann nicht gespeichert werden können.

- **Schlüssel-Wert-Speicher, die maximale Größe für eine app** -Anwendungen können nur bis zu 64 KB von Schlüssel-Wert-Daten insgesamt speichern. Zum Festlegen der Schlüssel über diesen Grenzwert hinaus nicht möglich, und der vorherige Wert beibehalten wird.

- **Datentypen** – nur einfache Typen wie Zeichenfolgen, Zahlen und boolesche Werte können gespeichert werden.

Die **iCloudKeyValue** wird veranschaulicht, wie es funktioniert. Der Beispielcode erstellt einen Schlüssel mit dem Namen für jedes Gerät: Sie legen Sie diesen Schlüssel auf einem Gerät und sehen Sie sich den Wert für andere Personen weitergegeben werden können. Erstellt auch einen Schlüssel namens "Freigegeben" die auf jedem Gerät - bearbeitet werden kann, wenn Sie für viele Geräte gleichzeitig bearbeiten, entscheidet sich auf iCloud aus dem Wert "gewinnt" (mit einem Zeitstempel bei der Änderung) und ruft weitergegeben.

Dieser Screenshot zeigt das Beispiel verwendet. Sie sind in die Ansicht für den Bildlauf am unteren Rand des Bildschirms gedruckt und in die Eingabefelder aktualisiert, wenn änderungsbenachrichtigungen, aus der iCloud empfangen werden.



 [![](introduction-to-icloud-images/icloud-kv-arrows.png "Den Fluss der Nachrichten zwischen Geräten")](introduction-to-icloud-images/icloud-kv-arrows.png#lightbox)

### <a name="setting-and-retrieving-data"></a>Festlegen und Abrufen von Daten

Dieser Code zeigt, wie Sie einen Zeichenfolgenwert festgelegt wird.

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
store.SetString("testkey", "VALUE IN THE CLOUD");  // key and value
store.Synchronize();
```

Aufrufen von synchronisieren wird sichergestellt, dass der Wert in nur lokalen Datenträgerspeicher beibehalten wird. Die Synchronisierung mit iCloud erfolgt im Hintergrund und kann nicht "erzwungen" durch den Anwendungscode. Mit der Netzwerkverbindung einwandfrei erfolgt die Synchronisierung häufig innerhalb von 5 Sekunden, aber wenn das Netzwerk eine schlechte (oder nicht verbundenen) ist ein Update wesentlich länger dauern.

Sie können einen Wert mit dem folgenden Code abrufen:

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
display.Text = store.GetString("testkey");
```

Der Wert wird aus dem lokalen Datenspeicher abgerufen: Diese Methode versucht nicht, wenden Sie sich an der iCloud-Server, um den Wert "latest" abzurufen. iCloud wird den lokale Datenspeicher einen eigenen Zeitplan entsprechend aktualisiert.

### <a name="deleting-data"></a>Löschen von Daten

Um Schlüssel-Wert-Paar vollständig zu entfernen, verwenden Sie die Remove-Methode wie folgt:

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
store.Remove("testkey");
store.Synchronize();
```

### <a name="observing-changes"></a>Prüfen von Änderungen

Eine Anwendung kann auch die Benachrichtigungen erhalten, wenn die Werte von iCloud geändert werden, indem Sie einen Beobachter für Hinzufügen der `NSNotificationCenter.DefaultCenter`.
Der folgende code aus **KeyValueViewController.cs** `ViewWillAppear` Methode zeigt, wie diese Benachrichtigungen überwachen, und erstellen Sie eine Liste der Schlüssel geändert wurden:

```csharp
keyValueNotification =
NSNotificationCenter.DefaultCenter.AddObserver (
    NSUbiquitousKeyValueStore.DidChangeExternallyNotification, notification => {
    Console.WriteLine ("Cloud notification received");
    NSDictionary userInfo = notification.UserInfo;

    var reasonNumber = (NSNumber)userInfo.ObjectForKey (NSUbiquitousKeyValueStore.ChangeReasonKey);
    nint reason = reasonNumber.NIntValue;

    var changedKeys = (NSArray)userInfo.ObjectForKey (NSUbiquitousKeyValueStore.ChangedKeysKey);
    var changedKeysList = new List<string> ();
    for (uint i = 0; i < changedKeys.Count; i++) {
        var key = changedKeys.GetItem<NSString> (i); // resolve key to a string
        changedKeysList.Add (key);
    }
    // now do something with the list...
});
```

Ihr Code kann dann eine Aktion mit der Liste der geänderten Schlüssel, wie z. B. eine lokale Kopie der Dateien aktualisiert, oder Aktualisieren der Benutzeroberfläche mit den neuen Werten vornehmen.

Mögliche Gründe sind: ServerChange (0), InitialSyncChange (1) oder QuotaViolationChange (2). Sie können Zugriff auf den Grund und andere Verarbeitung nach Bedarf ausführen (z. B. Sie möglicherweise einige Schlüssel aufgrund entfernen möchten eine *QuotaViolationChange*).

## <a name="document-storage"></a>Speichern von Dokumenten

iCloud-Dokumente dient zum Verwalten von Daten, die wichtig für Ihre app (und für den Benutzer). Es kann zum Verwalten von Dateien und andere Daten, die Ihre app werden, während gleichzeitig die iCloud-Sicherung auf Bereitstellung und Freigabe von Funktionen für Geräte des Benutzers ausgeführt muss verwendet werden.

Dieses Diagramm zeigt, wie alles zusammenpasst. Jedes Gerät hat die Daten auf lokalen Speicher (die UbiquityContainer) und des Betriebssystems iCloud, die Daemon kümmert sich senden und Empfangen von Daten in der Cloud gespeichert. Alle Dateizugriff auf die UbiquityContainer muss über FilePresenter/FileCoordinator, um den gleichzeitigen Zugriff zu verhindern, dass durchgeführt werden. Die `UIDocument` -Klasse implementiert, die für Sie; in diesem Beispiel wird gezeigt, wie UIDocument verwenden.

 [![](introduction-to-icloud-images/icloud-overview.png "Übersicht über die Dokument-Speicher")](introduction-to-icloud-images/icloud-overview.png#lightbox)

Im Beispiel iCloudUIDoc implementiert einen einfachen `UIDocument` Unterklasse, die ein einzelnen Textfeld enthält. Der Text gerendert wird, einem `UITextView` und Bearbeitungen von iCloud mit anderen Geräten weitergeleitet werden, mit der eine Meldung in Rot dargestellt. Der Code behandelt nicht mehr erweiterte iCloud-Features wie die Auflösung des Konflikts zwischen.

Dieser Screenshot zeigt die beispielanwendung – nach dem Ändern des Texts, und drücken Sie **UpdateChangeCount** das Dokument über die iCloud mit anderen Geräten synchronisiert wird.

 [![](introduction-to-icloud-images/iclouduidoc.png "Dieser Screenshot zeigt die beispielanwendung nach dem Ändern des Texts, und drücken UpdateChangeCount")](introduction-to-icloud-images/iclouduidoc.png#lightbox)

Es gibt fünf Teile zum iCloudUIDoc-Beispiel:

1. **Zugreifen auf die UbiquityContainer** -ermitteln, ob iCloud aktiviert ist, und wenn dies der Fall ist der Pfad zu Ihrer Anwendung iCloud-Speicherbereich.

1. **Erstellen einer Unterklasse UIDocument** – erstellen Sie eine Klasse Zwischenglied zwischen iCloud-Speicher und die Modellobjekte.

1. **Suchen und öffnen die iCloud-Dokumente** -verwenden Sie `NSFileManager` und `NSPredicate` iCloud-Dokumente suchen und öffnen Sie sie.

1. **Anzeigen von Dokumenten mit iCloud** -Verfügbarmachen der Eigenschaften aus Ihrem `UIDocument` , damit Sie mit UI-Steuerelemente interagieren können.

1. **Speichern von Dokumenten mit iCloud** -stellen Sie sicher, dass Änderungen in der Benutzeroberfläche auf Datenträger und iCloud beibehalten werden.

Alle iCloud-Vorgänge ausführen (oder ausgeführt werden soll) asynchron, damit sie nicht blockieren, während Sie warten, dass etwas passiert. Sehen Sie drei verschiedene Möglichkeiten zum erledigen dies im Beispiel ein:

 **Threads** – klicken Sie im `AppDelegate.FinishedLaunching` dem ersten Aufruf `GetUrlForUbiquityContainer` erfolgt in einem anderen Thread aus, um zu verhindern, dass der Hauptthread blockiert.

 **NotificationCenter** : Registrieren für Benachrichtigungen, wenn asynchrone Vorgänge, wie z.B. `NSMetadataQuery.StartQuery` abgeschlossen.

 **Abschluss-Handler** : übergeben Sie Methoden zur Ausführung auf den Abschluss asynchroner Operationen wie `UIDocument.Open`.

### <a name="accessing-the-ubiquitycontainer"></a>Zugreifen auf die UbiquityContainer

Der erste Schritt beim über die iCloud-Dokumente um zu bestimmen, ob iCloud aktiviert ist, und wenn dies der Fall ist der Speicherort des "ortsunabhängigen Containers" (das Verzeichnis, in dem iCloud-fähige Dateien auf dem Gerät gespeichert sind).

Dieser Code befindet sich in der `AppDelegate.FinishedLaunching` Methode des Beispiels.

```csharp
// GetUrlForUbiquityContainer is blocking, Apple recommends background thread or your UI will freeze
ThreadPool.QueueUserWorkItem (_ => {
    CheckingForiCloud = true;
    Console.WriteLine ("Checking for iCloud");
    var uburl = NSFileManager.DefaultManager.GetUrlForUbiquityContainer (null);
    // OR instead of null you can specify "TEAMID.com.your-company.ApplicationName"

    if (uburl == null) {
        HasiCloud = false;
        Console.WriteLine ("Can't find iCloud container, check your provisioning profile and entitlements");

        InvokeOnMainThread (() => {
            var alertController = UIAlertController.Create ("No \uE049 available",
            "Check your Entitlements.plist, BundleId, TeamId and Provisioning Profile!", UIAlertControllerStyle.Alert);
            alertController.AddAction (UIAlertAction.Create ("OK", UIAlertActionStyle.Destructive, null));
            viewController.PresentViewController (alertController, false, null);
        });
    } else { // iCloud enabled, store the NSURL for later use
        HasiCloud = true;
        iCloudUrl = uburl;
        Console.WriteLine ("yyy Yes iCloud! {0}", uburl.AbsoluteUrl);
    }
    CheckingForiCloud = false;
});
```

Obwohl das Beispiel nicht gezeigt wird, empfiehlt Apple GetUrlForUbiquityContainer aufrufen, sobald eine app in den Vordergrund gesetzt wird.

### <a name="creating-a-uidocument-subclass"></a>Erstellen einer UIDocument-Unterklasse

Alle iCloud-Dateien und Verzeichnisse (d. h. nichts im Verzeichnis UbiquityContainer gespeichert) mithilfe von NSFileManager-Methoden, die NSFilePresenter-Protokoll implementiert und über eine NSFileCoordinator schreiben verwaltet werden müssen.
Die einfachste Möglichkeit, all dies zu tun ist, nicht, um sie zu schreiben, selbst, aber die Unterklasse UIDocument was geschieht alles für Sie.

Es gibt nur zwei Methoden, die Sie implementieren müssen, in einer Unterklasse UIDocument mit iCloud arbeiten:

- **LoadFromContents** -übergibt die NSData, der den Inhalt der Datei für die Sie in Ihrem Modell Klasse/es zu entpacken.

- **ContentsForType** -Anforderung aufgefordert, Ihr Modell Klasse/es die NSData Darstellung, die auf Datenträger (und der Cloud) zu speichern.

Dieser Beispielcode aus **iCloudUIDoc\MonkeyDocument.cs** UIDocument Implementierung veranschaulicht.

```csharp
public class MonkeyDocument : UIDocument
{
    // the 'model', just a chunk of text in this case; must easily convert to NSData
    NSString dataModel;
    // model is wrapped in a nice .NET-friendly property
    public string DocumentString {
        get {
            return dataModel.ToString ();
        }
        set {
            dataModel = new NSString (value);
        }
    }
    public MonkeyDocument (NSUrl url) : base (url)
    {
        DocumentString = "(default text)";
    }
    // contents supplied by iCloud to display, update local model and display (via notification)
    public override bool LoadFromContents (NSObject contents, string typeName, out NSError outError)
    {
        outError = null;

        Console.WriteLine ("LoadFromContents({0})", typeName);

        if (contents != null)
            dataModel = NSString.FromData ((NSData)contents, NSStringEncoding.UTF8);

        // LoadFromContents called when an update occurs
        NSNotificationCenter.DefaultCenter.PostNotificationName ("monkeyDocumentModified", this);
        return true;
    }
    // return contents for iCloud to save (from the local model)
    public override NSObject ContentsForType (string typeName, out NSError outError)
    {
        outError = null;

        Console.WriteLine ("ContentsForType({0})", typeName);
        Console.WriteLine ("DocumentText:{0}",dataModel);

        NSData docData = dataModel.Encode (NSStringEncoding.UTF8);
        return docData;
    }
}
```

Das Datenmodell ist in diesem Fall sehr einfach: ein einzelnes Textfeld. Das Datenmodell kann so komplex wie erforderlich, z. B. eine XML-Dokument oder Binärdaten sein. Die primäre Rolle der UIDocument-Implementierung ist übersetzt zwischen Ihren Modellklassen und eine NSData-Darstellung, die auf dem Datenträger gespeichert/geladen werden können.

### <a name="finding-and-opening-icloud-documents"></a>Suchen und iCloud-Dokumente öffnen

Die Beispiel-app befasst sich nur mit einer einzelnen Datei: "Test.txt" - also den Code in **Datei "appdelegate.cs"** erstellt eine `NSPredicate` und `NSMetadataQuery` speziell für diesen Dateinamen suchen. Die `NSMetadataQuery` wird asynchron ausgeführt und sendet eine Benachrichtigung, wenn er abgeschlossen ist. `DidFinishGathering` Ruft die von der Beobachter Benachrichtigungen aufgerufen werden, beendet die Abfrage aus, und ruft Sie LoadDocument, die verwendet die `UIDocument.Open` -Methode mit einem Abschlusshandler, laden Sie die Datei und zeigt es im eine `MonkeyDocumentViewController`.

```csharp
string monkeyDocFilename = "test.txt";
void FindDocument ()
{
    Console.WriteLine ("FindDocument");
    query = new NSMetadataQuery {
        SearchScopes = new NSObject [] { NSMetadataQuery.UbiquitousDocumentsScope }
    };

    var pred = NSPredicate.FromFormat ("%K == %@", new NSObject[] {
        NSMetadataQuery.ItemFSNameKey, new NSString (MonkeyDocFilename)
    });

    Console.WriteLine ("Predicate:{0}", pred.PredicateFormat);
    query.Predicate = pred;

    NSNotificationCenter.DefaultCenter.AddObserver (
        this,
        new Selector ("queryDidFinishGathering:"),
        NSMetadataQuery.DidFinishGatheringNotification,
        query
    );

    query.StartQuery ();
}

[Export ("queryDidFinishGathering:")]
void DidFinishGathering (NSNotification notification)
{
    Console.WriteLine ("DidFinishGathering");
    var metadataQuery = (NSMetadataQuery)notification.Object;
    metadataQuery.DisableUpdates ();
    metadataQuery.StopQuery ();

    NSNotificationCenter.DefaultCenter.RemoveObserver (this, NSMetadataQuery.DidFinishGatheringNotification, metadataQuery);
    LoadDocument (metadataQuery);
}

void LoadDocument (NSMetadataQuery metadataQuery)
{
    Console.WriteLine ("LoadDocument");

    if (metadataQuery.ResultCount == 1) {
        var item = (NSMetadataItem)metadataQuery.ResultAtIndex (0);
        var url = (NSUrl)item.ValueForAttribute (NSMetadataQuery.ItemURLKey);
        doc = new MonkeyDocument (url);

        doc.Open (success => {
            if (success) {
                Console.WriteLine ("iCloud document opened");
                Console.WriteLine (" -- {0}", doc.DocumentString);
                viewController.DisplayDocument (doc);
            } else {
                Console.WriteLine ("failed to open iCloud document");
            }
        });
    } // TODO: if no document, we need to create one
}
```

### <a name="displaying-icloud-documents"></a>Anzeigen von iCloud-Dokumente

Anzeigen einer UIDocument darf nicht auf jede beliebige andere Modellklasse anders sein
- Eigenschaften werden im UI-Steuerelemente, die möglicherweise vom Benutzer bearbeitet, und klicken Sie dann auf das Modell zurückgeschrieben angezeigt.

Im Beispiel **iCloudUIDoc\MonkeyDocumentViewController.cs** zeigt den MonkeyDocument Text in einem `UITextView`. `ViewDidLoad` überwacht die Benachrichtigung gesendet, der `MonkeyDocument.LoadFromContents` Methode. `LoadFromContents` wird aufgerufen, wenn iCloud neue Daten für die Datei enthält, damit Benachrichtigung gibt an, dass das Dokument aktualisiert wurde.

```csharp
NSNotificationCenter.DefaultCenter.AddObserver (this,
    new Selector ("dataReloaded:"),
    new NSString ("monkeyDocumentModified"),
    null
);
```

Die Beispiel-Code-Benachrichtigungshandler Ruft eine Methode, um die Benutzeroberfläche – in diesem Fall ohne jede konflikterkennung oder-Lösung zu aktualisieren.

```csharp
[Export ("dataReloaded:")]
void DataReloaded (NSNotification notification)
{
    doc = (MonkeyDocument)notification.Object;
    // we just overwrite whatever was being typed, no conflict resolution for now
    docText.Text = doc.DocumentString;
}
```

### <a name="saving-icloud-documents"></a>ICloud-Dokumente gespeichert.

Eine UIDocument in iCloud hinzufügen, können Sie aufrufen `UIDocument.Save` direkt (für neue Dokumente) oder verschieben Sie eine vorhandene Datei mit `NSFileManager.DefaultManager.SetUbiquitious`. Der Beispielcode erstellt ein neues Dokument direkt in den ortsunabhängigen-Container mit dem folgenden Code (es gibt zwei Abschluss Handler, eine für die `Save` Vorgang und einen anderen für das Öffnen):

```csharp
var docsFolder = Path.Combine (iCloudUrl.Path, "Documents"); // NOTE: Documents folder is user-accessible in Settings
var docPath = Path.Combine (docsFolder, MonkeyDocFilename);
var ubiq = new NSUrl (docPath, false);
var monkeyDoc = new MonkeyDocument (ubiq);
monkeyDoc.Save (monkeyDoc.FileUrl, UIDocumentSaveOperation.ForCreating, saveSuccess => {
Console.WriteLine ("Save completion:" + saveSuccess);
if (saveSuccess) {
    monkeyDoc.Open (openSuccess => {
        Console.WriteLine ("Open completion:" + openSuccess);
        if (openSuccess) {
            Console.WriteLine ("new document for iCloud");
            Console.WriteLine (" == " + monkeyDoc.DocumentString);
            viewController.DisplayDocument (monkeyDoc);
        } else {
            Console.WriteLine ("couldn't open");
        }
    });
} else {
    Console.WriteLine ("couldn't save");
}
```

Nachfolgende Änderungen am Dokument sind nicht "gespeichert" direkt, stattdessen geben wir die `UIDocument` , die sie geändert wurde, mit `UpdateChangeCount`, und es wird automatisch in den Betrieb des Datenträgers ein Speichern planen:

```csharp
doc.UpdateChangeCount (UIDocumentChangeKind.Done);
```

### <a name="managing-icloud-documents"></a>Verwalten von iCloud-Dokumente

Benutzer verwalten können, iCloud-Dokumente in der **Dokumente** Verzeichnis des "ortsunabhängigen Containers" außerhalb Ihrer Anwendung über die Einstellungen können sie anzeigen, die Liste der Dateien und Wischen, um zu löschen. Anwendungscode sollte in der Lage, die Situation behandeln, in Dokumenten, die vom Benutzer gelöscht werden. Speichern Sie keine internen Anwendungsdaten in die **Dokumente** Verzeichnis.

 [![](introduction-to-icloud-images/icloudstorage.png "ICloud-Dokumente workflowverwaltung")](introduction-to-icloud-images/icloudstorage.png#lightbox)



Benutzer erhalten auch andere Warnungen, wenn sie versuchen, eine iCloud-fähigen Anwendung auf ihrem Gerät, um über den Status der iCloud-Dokumente, die im Zusammenhang mit dieser Anwendung informieren zu entfernen.

 [![](introduction-to-icloud-images/icloud-delete1.png "Beispiel-Dialogfeld, wenn der Benutzer versucht, eine iCloud-fähige Anwendung auf ihrem Gerät entfernt werden soll.")](introduction-to-icloud-images/icloud-delete1.png#lightbox)

 [![](introduction-to-icloud-images/icloud-delete2.png "Beispiel-Dialogfeld, wenn der Benutzer versucht, eine iCloud-fähige Anwendung auf ihrem Gerät entfernt werden soll.")](introduction-to-icloud-images/icloud-delete2.png#lightbox)

## <a name="icloud-backup"></a>iCloud-Sicherung

Während der Sicherung in iCloud ein Feature nicht, die direkt von Entwicklern zugegriffen wird, kann die Möglichkeit, die Sie Ihre Anwendung entwerfen, die benutzerfreundlichkeit auswirken.
Apple bietet [iOS Daten Speicherrichtlinien](https://developer.apple.com/icloud/documentation/data-storage/) für Entwickler, damit Sie in ihre iOS-Anwendungen ausführen.

Der wichtigste Aspekt ist, gibt an, ob Ihre app umfangreiche Dateien gespeichert werden, die nicht benutzergeneriertem (z. B. einer magazine Reader-Anwendung, die Inhalt pro Problem hundred-plus MB speichert) sind. Apple bevorzugt, dass Sie nicht speichern, sollten diese Art von Daten, wo sie wird werden Sicherung in iCloud und füllen Sie das Kontingent des Benutzers iCloud unnötig.

Anwendungen, die große Mengen von Daten wie folgt speichern sollten entweder speichern es in eines der Benutzerverzeichnisse, die nicht gesichert wurden (z. b. Caches oder Tmp) oder `NSFileManager.SetSkipBackupAttribute` auf ein Flag mit diesen Dateien anwenden, sodass sie bei Sicherungsvorgängen von iCloud ignoriert.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde eingeführt, die neue iCloud-Funktion, die unter iOS 5 enthalten. Er untersucht die Schritte erforderlich, um das Projekt zur Verwendung von iCloud zu konfigurieren und anschließend Beispiele für die iCloud-Funktionen zu implementieren.

Das Schlüssel-Wert-Speicher-Beispiel wird veranschaulicht, wie iCloud verwendet werden kann, um eine kleine Menge von Daten, die ähnlich wie die zu speichern, die NSUserPreferences gespeichert sind. Die UIDocument-Beispiel wurde gezeigt, wie komplexen Daten gespeichert und auf mehreren Geräten über die iCloud synchronisiert werden können.

Schließlich enthalten sie eine kurze Erläuterung zur wie das Hinzufügen von iCloud-Sicherung Entwurf der Anwendung beeinflussen sollte.



## <a name="related-links"></a>Verwandte Links

- [Einführung zu iCloud (Beispiel)](https://developer.xamarin.com/samples/monotouch/IntroductionToiCloud)
- [iCloud-Seminar-Beispielcode](https://github.com/xamarin/Seminars/tree/master/2012-03-22-iCloud)
- [iCloud-Seminar Folien](https://www.slideshare.net/Xamarin/using-icloud-with-monotouch)
- [iCloud NSUbiquitousKeyValueStore](https://developer.apple.com/library/prerelease/ios/)
- [iCloud-Speicher](https://support.apple.com/kb/HT4847)
