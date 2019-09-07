---
title: Verwenden von icloud mit xamarin. IOS
description: In diesem Dokument werden icloud und die Verwendung in xamarin. IOS-Anwendungen beschrieben. Er erläutert den Schlüssel-Wert-Speicher, den Dokumentenspeicher und die icloud-Sicherung.
ms.prod: xamarin
ms.assetid: C6F3B87C-C195-4434-EF14-D66E63894F09
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 06/09/2016
ms.openlocfilehash: df91699e0880bfae780b69f4b30be6667e8d64d9
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70763107"
---
# <a name="using-icloud-with-xamarinios"></a>Verwenden von icloud mit xamarin. IOS

Mit der icloud-Speicher-API in ios 5 können Anwendungen Benutzer Dokumente und anwendungsspezifische Daten an einem zentralen Speicherort speichern und von allen Geräten des Benutzers aus auf diese Elemente zugreifen.

Es stehen vier Arten von Speicher zur Verfügung:

- **Schlüssel-Wert-Speicher** : um kleine Datenmengen mit Ihrer Anwendung auf den anderen Geräten eines Benutzers freizugeben.

- **Uidocument-Speicher** : zum Speichern von Dokumenten und anderen Daten im icloud-Konto des Benutzers mithilfe einer Unterklasse von uidocument.

- **CoreData** -SQLite-Daten Bank Speicher.

- **Einzelne Dateien und Verzeichnisse** : zum Verwalten von vielen unterschiedlichen Dateien direkt im Dateisystem.

In diesem Dokument werden die ersten beiden Typen: Schlüssel-Wert-Paare und uidocument-Unterklassen erläutert und erläutert, wie diese Funktionen in xamarin. IOS verwendet werden.

> [!IMPORTANT]
> Apple [stellt Tools zur Verfügung](https://developer.apple.com/support/allowing-users-to-manage-data/), die Entwickler dabei unterstützen, die Datenschutz-Grundverordnung (DSGVO) der Europäischen Union umzusetzen.

## <a name="requirements"></a>Anforderungen

- Die neueste stabile Version von xamarin. IOS
- Xcode 10
- Visual Studio für Mac oder Visual Studio 2019.

## <a name="preparing-for-icloud-development"></a>Vorbereiten der icloud-Entwicklung

Anwendungen müssen für die Verwendung von icloud sowohl im [Apple-Bereitstellungs Portal](https://developer.apple.com/account/ios/overview.action) als auch im Projekt selbst konfiguriert werden. Führen Sie die folgenden Schritte aus, bevor Sie für icloud entwickeln (oder die Beispiele ausprobieren).

So konfigurieren Sie eine Anwendung für den Zugriff auf icloud ordnungsgemäß:

- **Suchen Sie Ihre** Team-ID für [Developer.Apple.com](https://developer.apple.com) , und besuchen Sie das **Mitglied Center > Ihrem Konto > Entwicklerkonto Zusammenfassung** , um Ihre Team-ID (oder die individuelle ID für einzelne Entwickler) zu erhalten. Dabei handelt es sich um eine Zeichenfolge mit 10 Zeichen (z. b. **A93A5CM278** ), die Teil des "Container Bezeichners" ist.

- **Erstellen einer neuen APP-ID** : um eine APP-ID zu erstellen, führen Sie die Schritte aus, die im [Abschnitt Bereitstellung für Store-Technologien im Handbuch zur Geräte Bereitstellung](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md)beschrieben werden, und stellen Sie sicher, dass Sie **icloud** als zulässigen Dienst überprüfen:

 [![](introduction-to-icloud-images/icloud-sml.png "Icloud als zulässigen Dienst überprüfen")](introduction-to-icloud-images/icloud.png#lightbox)

- **Erstellen eines neuen Bereitstellungs Profils** : führen Sie zum Erstellen eines Bereitstellungs Profils die im [Handbuch zur Geräte Bereitstellung](~/ios/get-started/installation/device-provisioning/index.md#provisioning-your-device) beschriebenen Schritte aus.

- **Fügen Sie den Container Bezeichner zu "Berechtigungen. plist" hinzu** - `TeamID.BundleID`das Container-Bezeichnerformat ist. Weitere Informationen finden Sie im Handbuch [Arbeiten mit Berechtigungen](~/ios/deploy-test/provisioning/entitlements.md) .

- **Konfigurieren Sie die Projekteigenschaften** : Stellen Sie in der Datei "Info. plist" sicher, dass die **Bündel** -ID mit der **Paket-ID** übereinstimmt, wenn Sie [eine APP](~/ios/deploy-test/provisioning/capabilities/index.md) Beim Signieren des IOS-Pakets wird ein **Bereitstellungs Profil** verwendet, das eine APP-ID mit dem icloud-App Service und die ausgewählte Datei mit den **benutzerdefinierten Berechtigungen** enthält. Dies kann in Visual Studio unter dem Projekteigenschaften Bereich erfolgen.

- **Aktivieren von icloud auf Ihrem Gerät** : Wechseln Sie zu **Einstellungen > icloud** , und stellen Sie sicher, dass das Gerät angemeldet ist.
Aktivieren und aktivieren Sie die Option **Dokumente & Daten** .

- **Sie müssen ein Gerät zum Testen von icloud verwenden** . es funktioniert nicht im Simulator.
Tatsächlich benötigen Sie tatsächlich zwei oder mehr Geräte, die alle mit derselben Apple-ID angemeldet sind, um die icloud in Aktion zu sehen.

## <a name="key-value-storage"></a>Schlüssel-Wert-Speicher

Der Schlüssel-Wert-Speicher ist für kleine Datenmengen vorgesehen, die ein Benutzer auf Geräten beibehalten kann, z. b. auf der letzten Seite, die in einem Buch oder Magazin angezeigt wird. Der Schlüssel-Wert-Speicher sollte nicht zum Sichern von Daten verwendet werden.

Bei der Verwendung von Schlüssel-Wert-Speicher sind einige Einschränkungen zu beachten:

- **Maximale Schlüsselgröße** : Schlüsselnamen dürfen nicht länger als 64 Bytes sein.

- **Maximale Werte Größe** : Sie können nicht mehr als 64 Kilobyte in einem einzelnen Wert speichern.

- **Die maximale Größe des Schlüsselwert Speicher für eine APP** -Anwendung kann insgesamt bis zu 64 Kilobyte an Schlüssel-Wert-Daten speichern. Versuche, über diesen Grenzwert hinausgehende Schlüssel festzulegen, schlagen fehl, und der vorherige Wert bleibt erhalten.

- **Datentypen** : nur grundlegende Typen wie Zeichen folgen, Zahlen und boolesche Werte können gespeichert werden.

Das **icloudkeyvalue** -Beispiel veranschaulicht, wie es funktioniert. Der Beispielcode erstellt einen Schlüssel mit dem Namen für jedes Gerät: Sie können diesen Schlüssel auf einem Gerät festlegen und beobachten, wie der Wert an andere Personen weitergegeben wird. Außerdem wird ein Schlüssel mit dem Namen "Shared" erstellt, der auf jedem Gerät bearbeitet werden kann. Wenn Sie auf vielen Geräten gleichzeitig bearbeiten, entscheidet icloud, welcher Wert "WINS" (mit einem Zeitstempel für die Änderung) und weitergegeben wird.

Dieser Screenshot zeigt das verwendete Beispiel. Wenn Änderungs Benachrichtigungen von icloud empfangen werden, werden Sie in der Bild Lauf Textansicht am unteren Bildschirmrand gedruckt und in den Eingabefeldern aktualisiert.

 [![](introduction-to-icloud-images/icloud-kv-arrows.png "Der Nachrichtenfluss zwischen Geräten")](introduction-to-icloud-images/icloud-kv-arrows.png#lightbox)

### <a name="setting-and-retrieving-data"></a>Festlegen und Abrufen von Daten

In diesem Code wird gezeigt, wie ein Zeichen folgen Wert festgelegt wird.

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
store.SetString("testkey", "VALUE IN THE CLOUD");  // key and value
store.Synchronize();
```

Durch den Aufruf von Synchronisierung wird sichergestellt, dass der Wert nur im lokalen Speicher gespeichert wird Die Synchronisierung mit icloud erfolgt im Hintergrund und kann vom Anwendungscode nicht "erzwungen" werden. Bei einer guten Netzwerk Konnektivität wird die Synchronisierung oft innerhalb von 5 Sekunden durchgeführt. wenn das Netzwerk jedoch schlecht (oder getrennt) ist, kann ein Update erheblich länger dauern.

Mit diesem Code können Sie einen Wert abrufen:

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
display.Text = store.GetString("testkey");
```

Der Wert wird aus dem lokalen Datenspeicher abgerufen. diese Methode versucht nicht, sich an icloud-Server zu wenden, um den Wert "Latest" zu erhalten. der lokale Datenspeicher wird von icloud gemäß einem eigenen Zeitplan aktualisiert.

### <a name="deleting-data"></a>Löschen von Daten

Um ein Schlüssel-Wert-Paar vollständig zu entfernen, verwenden Sie die Remove-Methode wie folgt:

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
store.Remove("testkey");
store.Synchronize();
```

### <a name="observing-changes"></a>Beobachten von Änderungen

Eine Anwendung kann auch Benachrichtigungen empfangen, wenn die Werte von icloud geändert werden, indem der `NSNotificationCenter.DefaultCenter`ein Beobachter hinzugefügt wird.
Der folgende Code der **KeyValueViewController.cs** `ViewWillAppear` -Methode zeigt, wie Sie diese Benachrichtigungen überwachen und eine Liste der Schlüssel erstellen, die geändert wurden:

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

Der Code kann dann einige Aktionen mit der Liste der geänderten Schlüssel durchführen, z. b. das Aktualisieren einer lokalen Kopie oder das Aktualisieren der Benutzeroberfläche mit den neuen Werten.

Mögliche Änderungs Gründe: ServerChange (0), initialsyncchange (1) oder quotaviolationchange (2). Sie können auf den Grund zugreifen und ggf. eine andere Verarbeitung durchführen (z. b. müssen Sie einige Schlüssel aufgrund einer *quotaviolationchange*entfernen).

## <a name="document-storage"></a>Dokument Speicher

der icloud-Dokument Speicher dient der Verwaltung von Daten, die für Ihre APP (und für den Benutzer) wichtig sind. Sie kann verwendet werden, um Dateien und andere Daten zu verwalten, die Ihre APP ausführen muss, und gleichzeitig die icloud-basierte Sicherungs-und Freigabe Funktionalität auf allen Geräten des Benutzers bereitzustellen.

Dieses Diagramm zeigt, wie alles zueinander passt. Jedes Gerät verfügt über Daten, die im lokalen Speicher (dem "ubiquitycontainer") gespeichert sind, und der icloud-Daemon des Betriebssystems übernimmt das Senden und empfangen von Daten in der Cloud. Der gesamte Dateizugriff auf den ubiquitycontainer muss über filepresenter/filecoordinator erfolgen, um den gleichzeitigen Zugriff zu verhindern. Die `UIDocument` -Klasse implementiert diese für Sie. in diesem Beispiel wird die Verwendung von uidocument veranschaulicht.

 [![](introduction-to-icloud-images/icloud-overview.png "Übersicht über den Dokument Speicher")](introduction-to-icloud-images/icloud-overview.png#lightbox)

Das iclouduidoc-Beispiel implementiert eine `UIDocument` einfache Unterklasse, die ein einzelnes Textfeld enthält. Der Text wird in einem `UITextView` gerendert, und bearbeitvorgänge werden von icloud an andere Geräte weitergegeben, wobei eine Benachrichtigungs Meldung rot angezeigt wird. Im Beispielcode werden erweiterte icloud-Funktionen wie die Konfliktlösung behandelt.

Dieser Screenshot zeigt die Beispielanwendung: nach dem Ändern des Texts und dem Drücken von **updatechangecount** wird das Dokument über icloud mit anderen Geräten synchronisiert.

 [![](introduction-to-icloud-images/iclouduidoc.png "Dieser Screenshot zeigt die Beispielanwendung nach dem Ändern des Texts und dem Drücken von updatechangecount.")](introduction-to-icloud-images/iclouduidoc.png#lightbox)

Das iclouduidoc-Beispiel besteht aus fünf Teilen:

1. **Zugreifen auf den ubiquitycontainer** : bestimmen Sie, ob icloud aktiviert ist, und wenn dies der Pfad zum icloud-Speicherbereich Ihrer Anwendung ist.

1. Erstellen **einer uidocument-Unterklasse** : Erstellen Sie eine Klasse, um zwischen dem icloud-Speicher und den Modell Objekten zu Zwischenspeichern.

1. Suchen **und Öffnen von icloud** -Dokumenten `NSFileManager` : `NSPredicate` verwenden Sie und, um icloud-Dokumente zu suchen und zu öffnen.

1. **Anzeigen von icloud-Dokumenten** : machen Sie `UIDocument` Eigenschaften aus dem verfügbar, damit Sie mit UI-Steuerelementen interagieren können.

1. **Speichern von icloud-Dokumenten** : Stellen Sie sicher, dass an der Benutzeroberfläche vorgenommene Änderungen auf dem Datenträger und in der icloud

Alle icloud-Vorgänge werden asynchron ausgeführt (oder sollten ausgeführt werden), sodass Sie nicht blockiert werden, während auf einen Vorgang gewartet wird. Im Beispiel werden drei verschiedene Möglichkeiten angezeigt, dies zu erreichen:

 **Threads** : beim `AppDelegate.FinishedLaunching` ersten `GetUrlForUbiquityContainer` -Vorgang wird in einem anderen Thread ausgeführt, um zu verhindern, dass der Haupt Thread blockiert wird.

 **Notificationcenter** : Registrierung für Benachrichtigungen, wenn asynchrone Vorgänge wie `NSMetadataQuery.StartQuery` z. b. beendet werden.

 **Abschluss Handler** : übergeben Sie Methoden, die bei Abschluss von asynchronen Vorgängen wie `UIDocument.Open`ausgeführt werden.

### <a name="accessing-the-ubiquitycontainer"></a>Zugreifen auf den ' ubiquitycontainer '

Der erste Schritt bei der Verwendung des icloud-Dokumenten Speichers besteht darin, zu bestimmen, ob icloud aktiviert ist. wenn dies der Fall ist, wird der Speicherort des "ubiquity-Containers" (das Verzeichnis, in dem icloud-aktivierte Dateien auf dem Gerät gespeichert sind)

Dieser Code ist in der `AppDelegate.FinishedLaunching` -Methode des Beispiels.

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

Obwohl das Beispiel dies nicht bewirkt, empfiehlt Apple, immer dann geturlforubiquitycontainer zu aufrufen, wenn eine APP im Vordergrund steht.

### <a name="creating-a-uidocument-subclass"></a>Erstellen einer uidocument-Unterklasse

Alle icloud-Dateien und-Verzeichnisse (d.h. alles, was im Verzeichnis "ubiquitycontainer" gespeichert ist) müssen mithilfe von NSFileManager-Methoden verwaltet werden. dabei wird das nsfilepresenter-Protokoll implementiert und über einen nsfilecoordinator geschrieben.
Die einfachste Möglichkeit, dies zu tun, ist nicht, Sie selbst zu schreiben, sondern Unterklasse uidocument, was alles für Sie erledigt.

Es gibt nur zwei Methoden, die Sie in einer uidocument-Unterklasse implementieren müssen, um mit icloud arbeiten zu können:

- **Loadfromcontent** : übergibt die NSData des Datei Inhalts, damit Sie Ihre Modellklassen entpacken können.

- **Contentsfortype** : fordern Sie an, dass Sie die NSData-Darstellung Ihrer Modell Klasse/es bereitstellen, die auf dem Datenträger (und in der Cloud) gespeichert werden soll.

Dieser Beispielcode von **iclouduidoc\monkeydocument.cs** zeigt, wie uidocument implementiert wird.

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

Das Datenmodell ist in diesem Fall sehr einfach: ein einzelnes Textfeld. Das Datenmodell kann so komplex wie erforderlich sein, z. b. ein XML-Dokument oder Binärdaten. Die primäre Rolle der uidocument-Implementierung besteht darin, zwischen den Modellklassen und einer NSData-Darstellung zu übersetzen, die auf dem Datenträger gespeichert/geladen werden kann.

### <a name="finding-and-opening-icloud-documents"></a>Suchen und Öffnen von icloud-Dokumenten

Die Beispiel-APP ist nur mit einem einzelnen File-Test. txt-Code zu tun. daher erstellt `NSPredicate` der `NSMetadataQuery` Code in **AppDelegate.cs** ein und dient zum speziellen Suchen des Datei namens. Die `NSMetadataQuery` wird asynchron ausgeführt und sendet eine Benachrichtigung, sobald Sie abgeschlossen ist. `DidFinishGathering`wird vom Benachrichtigungs Beobachter aufgerufen, beendet die Abfrage und ruft LoadDocument auf, das die `UIDocument.Open` -Methode mit einem Vervollständigungs Handler verwendet, um zu versuchen, die Datei zu `MonkeyDocumentViewController`laden und in einem anzuzeigen.

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

### <a name="displaying-icloud-documents"></a>Anzeigen von icloud-Dokumenten

Das Anzeigen eines uidocument sollte sich nicht auf eine andere Modell Klasse unterscheiden.
- Eigenschaften werden in UI-Steuerelementen angezeigt, die möglicherweise vom Benutzer bearbeitet und dann zurück in das Modell geschrieben werden.

Im Beispiel zeigt **iclouduidoc\monkeydocumentviewcontroller.cs** den monkeydocument-Text in einem `UITextView`an. `ViewDidLoad`lauscht auf die Benachrichtigung, die `MonkeyDocument.LoadFromContents` in der-Methode gesendet wurde. `LoadFromContents`wird aufgerufen, wenn icloud neue Daten für die Datei enthält, sodass diese Benachrichtigung anzeigt, dass das Dokument aktualisiert wurde.

```csharp
NSNotificationCenter.DefaultCenter.AddObserver (this,
    new Selector ("dataReloaded:"),
    new NSString ("monkeyDocumentModified"),
    null
);
```

Der Beispielcode-Benachrichtigungs Handler Ruft eine-Methode auf, um die Benutzeroberfläche zu aktualisieren (in diesem Fall ohne Konflikterkennung oder-Lösung).

```csharp
[Export ("dataReloaded:")]
void DataReloaded (NSNotification notification)
{
    doc = (MonkeyDocument)notification.Object;
    // we just overwrite whatever was being typed, no conflict resolution for now
    docText.Text = doc.DocumentString;
}
```

### <a name="saving-icloud-documents"></a>Speichern von icloud-Dokumenten

Wenn Sie icloud ein uidocument hinzufügen möchten, `UIDocument.Save` können Sie direkt (nur für neue Dokumente) oder eine vorhandene Datei `NSFileManager.DefaultManager.SetUbiquitious`mithilfe von verschieben. Der Beispielcode erstellt ein neues Dokument direkt im ortsunabhängigen Erreichbarkeit-Container mit diesem Code (es gibt zwei Abschluss Handler, einen für den `Save` Vorgang und einen für den geöffneten):

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

Nachfolgende Änderungen am Dokument werden nicht direkt "gespeichert", stattdessen wird die `UIDocument` Meldung angezeigt, dass Sie mit `UpdateChangeCount`geändert wurde, und es wird automatisch ein Vorgang zum Speichern auf Datenträger geplant:

```csharp
doc.UpdateChangeCount (UIDocumentChangeKind.Done);
```

### <a name="managing-icloud-documents"></a>Verwalten von icloud-Dokumenten

Benutzer können icloud-Dokumente im Verzeichnis " **Dokumente** " des "ubiquity-Containers" außerhalb Ihrer Anwendung über Einstellungen verwalten. Sie können die Datei Liste anzeigen und zum Löschen schwenken. Anwendungscode sollte in der Lage sein, die Situation zu behandeln, in der Dokumente vom Benutzer gelöscht werden. Speichern Sie keine internen Anwendungsdaten im Verzeichnis " **Dokumente** ".

 [![](introduction-to-icloud-images/icloudstorage.png "Verwalten von icloud-Dokumenten Workflow")](introduction-to-icloud-images/icloudstorage.png#lightbox)

Benutzer erhalten auch andere Warnungen, wenn Sie versuchen, eine icloud-fähige Anwendung von Ihrem Gerät zu entfernen, um Sie über den Status von icloud-Dokumenten in Bezug auf diese Anwendung zu informieren.

 [![](introduction-to-icloud-images/icloud-delete1.png "Beispiel Dialogfeld, wenn der Benutzer versucht, eine icloud-fähige Anwendung von seinem Gerät zu entfernen")](introduction-to-icloud-images/icloud-delete1.png#lightbox)

 [![](introduction-to-icloud-images/icloud-delete2.png "Beispiel Dialogfeld, wenn der Benutzer versucht, eine icloud-fähige Anwendung von seinem Gerät zu entfernen")](introduction-to-icloud-images/icloud-delete2.png#lightbox)

## <a name="icloud-backup"></a>icloud-Sicherung

Das Sichern in icloud ist kein Feature, auf das von Entwicklern direkt zugegriffen wird. die Art und Weise, wie Sie Ihre Anwendung entwerfen, kann sich auf die Benutzer Funktionalität auswirken.
Apple bietet [IOS-Daten Speicherungs Richtlinien](https://developer.apple.com/icloud/documentation/data-storage/) für Entwickler, die in ihren IOS-Anwendungen befolgt werden sollen.

Der wichtigste Aspekt ist, ob in Ihrer APP große Dateien gespeichert werden, die nicht vom Benutzer generiert werden (z. b. eine Magazine Reader-Anwendung, die hundert Plus Megabyte an Inhalten pro Problem speichert). Apple bevorzugt, dass Sie diese Art von Daten nicht speichern, wenn Sie in der icloud gesichert werden, und das icloud-Kontingent des Benutzers unnötig auffüllen.

Anwendungen, die große Datenmengen wie diese speichern, sollten Sie entweder in einem der nicht gesicherten Benutzerverzeichnisse speichern (z. b. Caches oder tmp) oder verwenden `NSFileManager.SetSkipBackupAttribute` Sie, um ein Flag auf diese Dateien anzuwenden, damit icloud diese während Sicherungs Vorgängen ignoriert.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde das neue icloud-Feature eingeführt, das in ios 5 enthalten ist. Es wurden die Schritte erläutert, die erforderlich sind, um Ihr Projekt für die Verwendung von icloud zu konfigurieren und dann Beispiele für die Implementierung von icloud-Funktionen bereitzustellen

Mit dem Beispiel für den Schlüssel-Wert-Speicher wurde veranschaulicht, wie icloud verwendet werden kann, um eine kleine Menge von Daten zu speichern, die der Art der Speicherung von nsuserpreferences ähneln. Im uidocument-Beispiel wurde gezeigt, wie komplexere Daten auf mehreren Geräten über icloud gespeichert und synchronisiert werden können.

Schließlich wurde eine kurze Erläuterung dazu hinzugefügt, wie sich das Hinzufügen der icloud-Sicherung auf den Anwendungs Entwurf auswirken sollte.

## <a name="related-links"></a>Verwandte Links

- [Einführung in icloud (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/introductiontoicloud)
- [Beispiel Code für icloud-Seminare](https://github.com/xamarin/Seminars/tree/master/2012-03-22-iCloud)
- [icloud-Seminar Folien](https://www.slideshare.net/Xamarin/using-icloud-with-monotouch)
- [iCloud NSUbiquitousKeyValueStore](https://developer.apple.com/library/prerelease/ios/)
- [icloud-Speicher](https://support.apple.com/kb/HT4847)
