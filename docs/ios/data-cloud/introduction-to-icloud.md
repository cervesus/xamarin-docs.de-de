---
title: iCloud
description: Apple iCloud in iOS 5, die als Dienst Anwendungen zum Speichern von Daten auf der Apple Server, und es auf allen Geräten, die von derselben Person (per ihre Apple-ID) verwendeten synchronisiert eingeführt wird. Sie weist außerdem eine Sicherungskomponente, in dem die Daten auf Ihren Geräten werden an Apple Server gesichert. Dieses Dokument beschreibt, wie einige der iCloud von Apple bereitgestellten APIs zum Speichern und Abrufen von Daten von ihren Servern, mithilfe des C#-Beispiele für das Speichern von Volumedaten mit kleinem Umfang Schlüssel-Wert-Paare und zum Speichern von Dokumenten. Es wird auch erläutert, wie iCloud-Sicherung für den Entwurf der Anwendung beeinflussen kann.
ms.prod: xamarin
ms.assetid: C6F3B87C-C195-4434-EF14-D66E63894F09
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/09/2016
ms.openlocfilehash: a62d4621a8f3ace64401d64e35c806317a591c03
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="icloud"></a>iCloud

_Apple iCloud in iOS 5, die als Dienst Anwendungen zum Speichern von Daten auf der Apple Server, und es auf allen Geräten, die von derselben Person (per ihre Apple-ID) verwendeten synchronisiert eingeführt wird. Sie weist außerdem eine Sicherungskomponente, in dem die Daten auf Ihren Geräten werden an Apple Server gesichert. Dieses Dokument beschreibt, wie einige der iCloud von Apple bereitgestellten APIs zum Speichern und Abrufen von Daten von ihren Servern, mithilfe des C#-Beispiele für das Speichern von Volumedaten mit kleinem Umfang Schlüssel-Wert-Paare und zum Speichern von Dokumenten. Es wird auch erläutert, wie iCloud-Sicherung für den Entwurf der Anwendung beeinflussen kann._

Die iCloud-Speicher-API in Ios5 kann Anwendungen Benutzerdokumente und anwendungsspezifische Daten an einem zentralen Ort speichern und diese Elemente von Geräten des Benutzers zugreifen.

Es gibt vier Arten von Speicher zur Verfügung:

- **Schlüssel-Wert-Speicher** – Informationen zum Freigeben von kleiner Mengen von Daten mit der Anwendung auf eine andere Geräte eines Benutzers.

- **UIDocument Speicher** - Dokumente und andere Daten in iCloud-Konto des Benutzers verwenden eine Unterklasse von UIDocument speichern.

- **CoreData** -Datenbankspeicher SQLite.

- **Einzelne Dateien und Verzeichnisse** – für die Verwaltung von vielen verschiedenen Dateien direkt in das Dateisystem.

Dieses Dokument erläutert die ersten beiden Typen - Schlüssel-Wert-Paare und UIDocument Unterklassen- und wie diese Funktionen in Xamarin.iOS verwendet.

> [!IMPORTANT]
> Apple [bietet Tools](https://developer.apple.com/support/allowing-users-to-manage-data/) Entwickler der Europäischen Union allgemeine Daten Schutz vor (GDPR) ordnungsgemäß verarbeiten können.

## <a name="requirements"></a>Anforderungen

- Die aktuellste stabile Version des Xamarin.iOS
- Xcode 8 oder höher
- Visual Studio für Mac oder Visual Studio 2015 und höher.

## <a name="preparing-for-icloud-development"></a>Vorbereitung für die Entwicklung mit icloud zulassen

Anwendungen must be configured iCloud im Verwenden der [Bereitstellung Apple-Portal](https://developer.apple.com/account/ios/overview.action) und das Projekt selbst. Vor dem Entwickeln für iCloud (oder das Testen der Beispiele) die folgenden Schritte.

So konfigurieren Sie eine Anwendung auf iCloud ordnungsgemäß:

-   **Ihr Team finden** -Anmeldung bei [developer.apple.com](http://developer.apple.com) und besuchen Sie die **Member Center > Ihr Konto > Developer Konto Zusammenfassung** Ihr Team (oder einzelne-ID für die einzelnen Entwickler abrufen ). Es wird eine Zeichenfolge von 10 Zeichen sein ( **A93A5CM278** z. B.) – Dies ist Bestandteil des Bezeichners"Container".

-   **Erstellen einer neuen App-ID** : Informationen zum Erstellen einer App-ID, befolgen die Schritte der [Bereitstellung für Store-Technologien-Abschnitt des Handbuchs Gerätebereitstellung](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md), und prüfen Sie unbedingt **iCloud** wie ein zulässige Dienst:

 [![](introduction-to-icloud-images/icloud-sml.png "Überprüfen Sie die iCloud als zulässige-Dienst")](introduction-to-icloud-images/icloud.png#lightbox)

- **Erstellen Sie ein neues Bereitstellungsprofil** : Informationen zum Erstellen eines Profils Bereitstellung befolgen die Schritte der [Gerätebereitstellung Handbuch](~/ios/get-started/installation/device-provisioning/index.md#Provisioning_Profile) .

- **Fügen Sie den Bezeichner des Containers zu Entitlements.plist** -ist das Bezeichner das Containerformat `TeamID.BundleID`. Weitere Informationen finden Sie unter der [arbeiten mit Berechtigungen](~/ios/deploy-test/provisioning/entitlements.md) Handbuch.

- **Konfigurieren Sie die Projekteigenschaften** – In der Datei "Info.plist" der **Paket-ID** entspricht der **Paket-ID** festgelegt, wenn [erstellen ein App-ID ](~/ios/deploy-test/provisioning/capabilities/index.md); IOS Bundle Signierung verwendet eine **Bereitstellungsprofil** , die eine App-ID mit der iCloud Anwendungsdiensts enthalten und die **benutzerdefinierte Ansprüche** Datei ausgewählt. Dies kann alle in Visual Studio unter dem Projekt Eigenschaftenbereich erfolgen.

- **Aktivieren Sie auf Ihrem Gerät iCloud** - wechseln Sie zu **Einstellungen > iCloud** und stellen Sie sicher, dass das Gerät angemeldet ist.
Wählen Sie aus, und aktivieren Sie die **Dokumente und Daten** Option.

- **Sie müssen ein Gerät verwenden, um die iCloud testen** -funktioniert nicht im Simulator.
In der Tat benötigen Sie tatsächlich zwei oder mehr Geräte alle die gleiche Apple-ID angemeldet sind, um iCloud in Aktion anzuzeigen.


## <a name="key-value-storage"></a>Schlüssel-Wert-Speicher

Schlüssel-Wert-Speicher ist für kleine Mengen von Daten vorgesehen, die ein Benutzer möglicherweise hinweg beibehalten Geräte – z. B. die letzte Seite gefällt, die sie in einem Buch oder Magazin angezeigt. Schlüssel-Wert-Speicher sollte nicht für die Sicherung von Daten verwendet werden.

Es gibt einige Einschränkungen bei Verwendung von Schlüssel-Wert-Speicher geachtet werden:

- **Maximale Schlüsselgröße** -Schlüssel dürfen nicht länger als 64 Bytes sein.

- **Maximale Größe** -Sie können nicht mehr als 64 KB gespeichert, in einen einzelnen Wert.

- **Schlüssel / Wert-Speicher, die maximale Größe für eine app** -Anwendungen können nur bis zu 64 KB von Schlüssel-Wert-Daten insgesamt speichern. Ist nicht möglich, Schlüssel über diesen Grenzwert hinaus festgelegt, und der vorherige Wert beibehalten wird.

- **Datentypen** – nur einfache Typen wie Zeichenfolgen, Zahlen und boolesche Werte können gespeichert werden.

Die **iCloudKeyValue** wird veranschaulicht, wie es funktioniert. Der Beispielcode erstellt einen Schlüssel namens für jedes Gerät: Sie legen Sie diesen Schlüssel auf einem Gerät und sehen Sie sich den Wert an andere Benutzer weitergegeben werden können. Es erstellt außerdem einen Schlüssel namens "Freigegeben", die auf jedem Gerät - bearbeitet werden kann, wenn Sie gleichzeitig auf vielen Geräten bearbeiten, entscheidet sich auf iCloud aus dem Wert "Wins" (mit einem Zeitstempel bei der Änderung) und ruft weitergegeben.

Diese bildschirmabbildung zeigt das Beispiel verwendet. Bei änderungsbenachrichtigungen von iCloud empfangen werden. sind sie in der fortlaufenden Textansicht am unteren Rand des Bildschirms gedruckt und in der Eingabefelder aktualisiert.



 [![](introduction-to-icloud-images/icloud-kv-arrows.png "Der Nachrichtenfluss zwischen den Geräten")](introduction-to-icloud-images/icloud-kv-arrows.png#lightbox)

### <a name="setting-and-retrieving-data"></a>Festlegen und Abrufen von Daten

Dieser Code zeigt, wie einen String-Wert festgelegt wird.

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
store.SetString("testkey", "VALUE IN THE CLOUD");  // key and value
store.Synchronize();
```

Synchronize aufrufen wird sichergestellt, dass der Wert nur in den lokalen Datenträgerspeicher beibehalten wird. Die Synchronisierung mit iCloud geschieht im Hintergrund und kann nicht erzwungen werden,"" durch den Anwendungscode. Mit der gute Netzwerkkonnektivität erfolgt die Synchronisierung häufig innerhalb von 5 Sekunden, aber wenn das Netzwerk eine schlechte (oder getrennt) ist ein Update wesentlich länger dauern.

Sie können einen-Wert mit dem folgenden Code abrufen:

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
display.Text = store.GetString("testkey");
```

Der Wert wird aus dem lokalen Datenspeicher abgerufen: Diese Methode wird nicht versucht, mit iCloud-Servern zum Abrufen des Werts des "letzten". iCloud wird den lokalen Datenspeicher einen eigenen Zeitplan aktualisiert.

### <a name="deleting-data"></a>Löschen von Daten

Um ein Schlüssel-Wert-Paar vollständig zu entfernen, verwenden Sie die Remove-Methode wie folgt:

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
store.Remove("testkey");
store.Synchronize();
```

### <a name="observing-changes"></a>Beobachten von Änderungen

Eine Anwendung kann auch Benachrichtigungen empfangen, wenn Werte von iCloud ändern werden, indem Sie einen Beobachter für die `NSNotificationCenter.DefaultCenter`.
Der folgende code in **KeyValueViewController.cs** `ViewWillAppear` Methode zeigt, wie diese Benachrichtigungen merken, und erstellen Sie eine Liste der Schlüssel geändert wurden:

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

Der Code kann dann eine Aktion mit der Liste der geänderten Tasten, z. B. eine lokale Kopie aktualisieren oder Aktualisierung der Benutzeroberfläche mit den neuen Werten vornehmen.

Mögliche Ursachen hierfür sind: ServerChange (0), InitialSyncChange (1) oder QuotaViolationChange (2). Sie können Zugriff auf den Grund und ggf. andere verarbeiten (angenommen, Sie müssen unter Umständen einige Schlüssel als Ergebnis des Entfernen einer *QuotaViolationChange*).

## <a name="document-storage"></a>Speicherung von Dokumenten

iCloud Dokumentspeicherung dient zum Verwalten von Daten, die wichtig für Ihre app (und für den Benutzer). Es kann zum Verwalten von Dateien und anderen Daten, die Ihre app werden, während gleichzeitig die iCloud-basierte Sicherung bereitstellen und Freigeben von Funktionen auf Geräten des Benutzers ausgeführt muss verwendet werden.

Die folgende Abbildung zeigt, wie sie alle zusammenpasst. Jedes Gerät verfügt über Daten auf lokalen Speicher (UbiquityContainer) und das Betriebssystem iCloud, Daemon übernimmt senden und Empfangen von Daten in der Cloud, gespeichert. Alle Dateizugriff auf die UbiquityContainer muss über FilePresenter/FileCoordinator, um zu verhindern, dass bei der gleichzeitigen Zugriff erfolgen. Die `UIDocument` Klasse implementiert, die für Sie; in diesem Beispiel wird gezeigt, wie UIDocument verwenden.

 [![](introduction-to-icloud-images/icloud-overview.png "Der Dokument-Speicher (Übersicht)")](introduction-to-icloud-images/icloud-overview.png#lightbox)

Das iCloudUIDoc-Beispiel implementiert einen einfachen `UIDocument` Unterklasse, die ein einzelnen Textfeld enthält. Der Text gerendert wird, einem `UITextView` Bearbeitungen durch iCloud mit anderen Geräten weitergeleitet, mit der eine Meldung in Rot dargestellt. Der Code behandelt nicht mehr erweiterte iCloud-Features wie die Auflösung des Konflikts zwischen.

Diese bildschirmabbildung zeigt die beispielanwendung - nach dem Ändern des Texts, und drücken **UpdateChangeCount** das Dokument über die iCloud mit anderen Geräten synchronisiert wird.

 [![](introduction-to-icloud-images/iclouduidoc.png "Diese bildschirmabbildung zeigt die beispielanwendung nach dem Ändern des Texts, und drücken UpdateChangeCount")](introduction-to-icloud-images/iclouduidoc.png#lightbox)

Es gibt fünf Teilen, die die Stichprobe iCloudUIDoc:

1. **Zugreifen auf die UbiquityContainer** -ermitteln, ob iCloud aktiviert ist, und wenn dies der Fall ist der Pfad zu Ihrer Anwendung iCloud Speicherbereich.

1. **Erstellen eine Unterklasse UIDocument** -erstellen Sie eine Klasse als Zwischenglied zwischen iCloud-Speicher und die Modellobjekte.

1. **Suchen und Öffnen von Dokumenten mit iCloud** -verwenden Sie `NSFileManager` und `NSPredicate` zum Suchen von Dokumenten mit iCloud, und öffnen sie.

1. **Anzeigen von Dokumenten mit iCloud** -verfügbar zu machen Eigenschaften aus der `UIDocument` , damit Sie mit UI-Steuerelementen zu interagieren können.

1. **Speichern von Dokumenten mit iCloud** -stellen Sie sicher, dass Änderungen in der Benutzeroberfläche auf Datenträger und iCloud beibehalten werden.

Alle iCloud-Vorgänge ausführen (oder ausgeführt werden soll) asynchron, damit sie nicht blockieren, während des Wartens auf ein Element durchgeführt werden soll. Sehen Sie drei verschiedene Arten der Realisierung im Beispiel ein:

 **Threads** – im `AppDelegate.FinishedLaunching` der erste Aufruf `GetUrlForUbiquityContainer` erfolgt in einem anderen Thread, um zu verhindern, dass den Hauptthread zu blockieren.

 **NotificationCenter** – registrieren für Benachrichtigungen, wenn asynchrone Vorgänge wie `NSMetadataQuery.StartQuery` abgeschlossen.

 **Abschluss Handler** – und übergeben Sie Methoden zur Ausführung auf Abschluss der asynchronen Vorgänge wie `UIDocument.Open`.

### <a name="accessing-the-ubiquitycontainer"></a>Zugreifen auf die UbiquityContainer

Der erste Schritt beim über die iCloud Dokumentspeicherung zu bestimmen, ob iCloud aktiviert ist, und wenn dies der Fall ist der Speicherort des Containers"ortsunabhängigen" (das Verzeichnis, in dem iCloud-fähigen Dateien auf dem Gerät gespeichert sind).

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

Im Beispiel nicht dies tun, Apple empfiehlt GetUrlForUbiquityContainer aufrufen, sobald eine app in den Vordergrund gestellt wird.

### <a name="creating-a-uidocument-subclass"></a>Erstellen eine Unterklasse UIDocument

Alle iCloud-Dateien und Verzeichnisse (d. h. nichts im Verzeichnis UbiquityContainer gespeichert) muss mithilfe der Implementierung des Protokolls NSFilePresenter und schreiben über eine NSFileCoordinator NSFileManager Methoden verwaltet werden.
Die einfachste Möglichkeit, dieses Vorgehen wird nicht zum Schreiben dieser selbst, aber UIDocument codemodellierung-Unterklasse für Sie.

Es gibt nur zwei Methoden, die Sie, in einer Unterklasse UIDocument zum Arbeiten mit icloud zulassen implementieren müssen:

- **LoadFromContents** -übergibt die NSData des Inhalts in Ihrem Modell Klasse/vorangestellten Entpacken Sie die Datei.

- **ContentsForType** -Anforderung aufgefordert, Ihr Modell Klasse/es die Darstellung NSData einzugeben, die auf Datenträger (und der Cloud) speichern.

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

Das Datenmodell in diesem Fall ist sehr einfach – ein Textfeld. Das Datenmodell kann so komplex wie erforderlich, z. B. ein XML-Dokument oder Binärdaten sein. Die primäre Rolle der UIDocument Implementierung ist übersetzt zwischen Ihren Modellklassen und eine NSData Darstellung, die auf dem Datenträger gespeichert/geladen werden können.

### <a name="finding-and-opening-icloud-documents"></a>Suchen und Öffnen von Dokumenten icloud zulassen

Die Beispiel-app nur befasst sich mit einer einzelnen Datei - "Test.txt" - daher den Code in **AppDelegate.cs** erstellt eine `NSPredicate` und `NSMetadataQuery` speziell für diesen Dateinamen gesucht werden soll. Die `NSMetadataQuery` wird asynchron ausgeführt und sendet eine Benachrichtigung, wenn er abgeschlossen ist. `DidFinishGathering` wird aufgerufen, durch die Benachrichtigung "Beobachter", das Beenden der Abfrage und ruft LoadDocument, der verwendet die `UIDocument.Open` Methode mit einem Abschlusshandler versucht wird, laden Sie die Datei, und zeigen Sie es in einem `MonkeyDocumentViewController`.

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

### <a name="displaying-icloud-documents"></a>Anzeigen von Dokumenten mit icloud zulassen

Anzeigen einer UIDocument sollte nicht für alle anderen Modell unterscheidet
- Eigenschaften werden im UI-Steuerelemente, die möglicherweise vom Benutzer bearbeitet, und klicken Sie dann auf das Modell zurückgeschrieben angezeigt.

Im Beispiel **iCloudUIDoc\MonkeyDocumentViewController.cs** zeigt den MonkeyDocument Text in einem `UITextView`. `ViewDidLoad` überwacht die Benachrichtigung gesendet wird, der `MonkeyDocument.LoadFromContents` Methode. `LoadFromContents` wird aufgerufen, wenn iCloud neue Daten für die Datei verfügt, damit die Benachrichtigung gibt an, dass das Dokument aktualisiert wurde.

```csharp
NSNotificationCenter.DefaultCenter.AddObserver (this,
    new Selector ("dataReloaded:"),
    new NSString ("monkeyDocumentModified"),
    null
);
```

Das Beispiel Benachrichtigung Codehandler Ruft eine Methode, um die Benutzeroberfläche – in diesem Fall ohne eine konflikterkennung oder-Lösung zu aktualisieren.

```csharp
[Export ("dataReloaded:")]
void DataReloaded (NSNotification notification)
{
    doc = (MonkeyDocument)notification.Object;
    // we just overwrite whatever was being typed, no conflict resolution for now
    docText.Text = doc.DocumentString;
}
```

### <a name="saving-icloud-documents"></a>Speichern von iCloud-Dokumente

Hinzufügen einer UIDocument zu iCloud Sie aufrufen können `UIDocument.Save` direkt (für neue Dokumente) oder verschieben Sie eine vorhandene Datei mit `NSFileManager.DefaultManager.SetUbiquitious`. Der Beispielcode erstellt ein neues Dokument in der ortsunabhängigen Container mit dem folgenden Code direkt (es gibt zwei Abschluss Handler hier eine für die `Save` Vorgang und ein anderes für das Öffnen):

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

Nachfolgende Änderungen an das Dokument sind nicht "gespeichert" direkt, stattdessen geben wir die `UIDocument` , die es geändert wurde, mit `UpdateChangeCount`, und es wird automatisch eine speichern Datenträgervorgang planen:

```csharp
doc.UpdateChangeCount (UIDocumentChangeKind.Done);
```

### <a name="managing-icloud-documents"></a>Verwalten von Dokumenten mit icloud zulassen

Benutzerverwaltung iCloud Dokumente in der **Dokumente** Verzeichnis "ortsunabhängigen Container" außerhalb Ihrer Anwendung über Einstellungen; sie können die Liste der Dateien und Wischen löschen anzeigen. Anwendungscode muss die Situation zu behandeln, in denen Dokumente vom Benutzer gelöscht werden. Speichern Sie keine interne Anwendungsdaten in die **Dokumente** Verzeichnis.

 [![](introduction-to-icloud-images/icloudstorage.png "Verwalten des Workflows für iCloud-Dokumente")](introduction-to-icloud-images/icloudstorage.png#lightbox)



Benutzer erhalten auch verschiedene Warnungen, wenn sie versuchen, eine iCloud-fähigen Anwendung von ihrem Gerät, um den Status der iCloud-Dokumente, die im Zusammenhang mit dieser Anwendung informieren zu entfernen.

 [![](introduction-to-icloud-images/icloud-delete1.png "Beispiel-Dialogfeld, wenn der Benutzer versucht, eine iCloud-fähigen Anwendung von ihrem Gerät entfernt werden.")](introduction-to-icloud-images/icloud-delete1.png#lightbox)

 [![](introduction-to-icloud-images/icloud-delete2.png "Beispiel-Dialogfeld, wenn der Benutzer versucht, eine iCloud-fähigen Anwendung von ihrem Gerät entfernt werden.")](introduction-to-icloud-images/icloud-delete2.png#lightbox)

## <a name="icloud-backup"></a>iCloud-Sicherung

Während der Sicherung auf iCloud ein Feature nicht, die von Entwicklern direkt zugegriffen wird, können Sie Ihre Anwendung Entwerfen der Benutzeroberfläche Einfluss auf die.
Apple bietet [iOS Daten Speicherrichtlinien](http://developer.apple.com/icloud/documentation/data-storage/) für Entwickler, damit Sie in ihre iOS-Anwendungen ausführen.

Der wichtigste Aspekt ist, gibt an, ob Ihre app umfangreiche Dateien gespeichert werden, die nicht vom Benutzer generierte (z. B. eine Zeitschrift Reader-Anwendung, die hundred-plus (MB) des Inhalts pro Problem speichert) sind. Apple wird bevorzugt, dass diese Art von Daten, wo es wird werden gesicherte in icloud zulassen und unnötigerweise Ausfüllen des Benutzers iCloud Kontingent, nicht gespeichert werden.

Anwendungen, die große Mengen an Daten wie folgt speichern sollten entweder speichert es in einer der in den Verzeichnissen nach Benutzer, die nicht gesichert (z. b. Caches oder Tmp), oder verwenden Sie `NSFileManager.SetSkipBackupAttribute` ein Flag zu diesen Dateien angewendet, sodass iCloud bei Sicherungsvorgängen ignoriert.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel eingeführt, die neue iCloud-Funktion, die unter iOS 5 enthalten. Er überprüft die Schritte erforderlich, um das Projekt zur Verwendung von iCloud zu konfigurieren und dann erhalten Beispiele zum iCloud-Funktionen zu implementieren.

Das Schlüssel-Wert-Speicher-Beispiel wird veranschaulicht, wie iCloud verwendet werden kann, um eine kleine Menge von Daten, die ähnlich wie bei speichern, die NSUserPreferences gespeichert sind. Das UIDocument Beispiel wurde gezeigt, wie weitere komplexen Daten gespeichert und auf mehreren Geräten über die iCloud synchronisiert werden können.

Schließlich enthalten sie eine kurze Erläuterung auf wie das Hinzufügen von iCloud Sicherung Entwurf der Anwendung auswirken.



## <a name="related-links"></a>Verwandte Links

- [Einführung zu iCloud (Beispiel)](https://developer.xamarin.com/samples/monotouch/IntroductionToiCloud)
- [iCloud Veranstaltung-Beispielcode](https://github.com/xamarin/Seminars/tree/master/2012-03-22-iCloud)
- [iCloud Veranstaltung Folien](http://www.slideshare.net/Xamarin/using-icloud-with-monotouch)
- [iCloud NSUbiquitousKeyValueStore](https://developer.apple.com/library/prerelease/ios/)
- [iCloud-Speicher](http://support.apple.com/kb/HT4847)
