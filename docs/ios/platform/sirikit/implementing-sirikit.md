---
title: Implementieren von Sirikit in xamarin. IOS
description: In diesem Dokument werden die Schritte beschrieben, die zum Implementieren der Unterstützung von Sirikit in xamarin. IOS-apps erforderlich sind. Er erläutert Intents Extensions und Intents UI Extensions.
ms.prod: xamarin
ms.assetid: 20FFB981-EB10-48BA-BF79-40F37F0291EB
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 05/03/2018
ms.openlocfilehash: a80caca0b8c8c48a468b20f63467357300bd6de1
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031643"
---
# <a name="implementing-sirikit-in-xamarinios"></a>Implementieren von Sirikit in xamarin. IOS

_In diesem Artikel werden die Schritte beschrieben, die zum Implementieren der Unterstützung von Sirikit in xamarin. IOS-apps erforderlich sind._

Mit dem neuen IOS 10 ermöglicht das Sirikit einer xamarin. IOS-APP das Bereitstellen von Diensten, auf die der Benutzer mithilfe von Siri und der Maps-APP auf einem IOS-Gerät zugreifen kann. In diesem Artikel werden die Schritte beschrieben, die erforderlich sind, um die Unterstützung von Sirikit in den xamarin. IOS-Apps durch Hinzufügen der erforderlichen Intents-Erweiterungen, Intents-UI-Erweiterungen und

Siri arbeitet mit dem Konzept von **Domänen**, Gruppen von bekannten Aktionen für Verwandte Aufgaben. Jede Interaktion zwischen der APP und Siri muss wie folgt in eine der bekannten Dienst Domänen fallen:

- Aufrufen von Audiodaten oder Videos.
- Die Reservierung einer Fahrt.
- Verwalten von-Workouts.
- Übermittlung.
- Fotos werden gesucht.
- Senden oder empfangen von Zahlungen.

Wenn der Benutzer eine Anforderung von Siri an einen der Dienste der APP-Erweiterung sendet, sendet Sirikit der Erweiterung ein **Intent** -Objekt, das die Anforderung des Benutzers zusammen mit allen unterstützenden Daten beschreibt. Die APP-Erweiterung generiert dann das entsprechende **Antwort** Objekt für die angegebene **Absicht**und erläutert, wie die Erweiterung die Anforderung verarbeiten kann.

Dieses Handbuch enthält ein kurzes Beispiel für das Einschließen der Unterstützung von Sirikit in eine vorhandene app. Für dieses Beispiel verwenden wir die gefälschte monkeychat-App:

[![](implementing-sirikit-images/monkeychat01.png "The MonkeyChat icon")](implementing-sirikit-images/monkeychat01.png#lightbox)

Monkeychat hält ein eigenes Kontaktbuch der Freunde des Benutzers, die jeweils einem Bildschirmnamen (z. b. "Bobo") zugeordnet sind, und ermöglicht es dem Benutzer, Text Chats über den Bildschirmnamen an jeden Freund zu senden.

## <a name="extending-the-app-with-sirikit"></a>Erweitern der APP mit Sirikit

Wie im Leitfaden "Grundlegendes zu [Sirikit-Konzepten](~/ios/platform/sirikit/understanding-sirikit.md) " gezeigt, gibt es drei Hauptbestandteile bei der Erweiterung einer APP mit "Sirikit":

[![](implementing-sirikit-images/elements01.png "Extending the App with SiriKit diagram")](implementing-sirikit-images/elements01.png#lightbox)

Dazu gehören:

1. **Intents-Erweiterung** : überprüft die Benutzer Antworten, bestätigt, dass die APP die Anforderung verarbeiten kann, und führt die Aufgabe aus, um die Anforderung des Benutzers zu erfüllen.
2. **Intents UI Extension** - *optional*, stellt eine benutzerdefinierte Benutzeroberfläche für die Antworten in der SIRI-Umgebung bereit und kann die Benutzeroberfläche und das Branding von apps in Siri bringen, um die Benutzeroberfläche zu bereichern.
3. **App** : stellt benutzerspezifische vokabare für die APP bereit, um Siri bei der Arbeit mit Ihnen zu unterstützen. 

Alle diese Elemente und die Schritte, die Sie in die APP einschließen, werden in den folgenden Abschnitten ausführlich beschrieben.

## <a name="preparing-the-app"></a>Vorbereiten der APP

"Sirikit" basiert auf Erweiterungen. vor dem Hinzufügen von Erweiterungen zur APP gibt es jedoch einige Dinge, die der Entwickler bei der Einführung von Sirikit unterstützen muss.

### <a name="moving-common-shared-code"></a>Verschieben von gemeinsamem gemeinsam verwendetem Code 

Zuerst kann der Entwickler einen Teil des gemeinsamen Codes, der von der APP und den Erweiterungen gemeinsam genutzt wird, in frei _gegebene Projekte_, _Portable Klassenbibliotheken (Portable Class Libraries, pcls)_ oder _Native Bibliotheken_verschieben.

Die Erweiterungen müssen in der Lage sein, alle Aktionen auszuführen, die von der app ausgeführt werden. In den Bedingungen der monkeychat-Beispiel-APP, z. b. Suchen nach Kontakten, Hinzufügen neuer Kontakte, Senden von Nachrichten und Abrufen des Nachrichten Verlaufs

Durch das Verschieben dieses allgemeinen Codes in ein frei gegebenes Projekt, das PCL oder die native Bibliothek, ist es einfach, den Code an einem zentralen Ort zu verwalten und sicherzustellen, dass die Erweiterung und die übergeordnete App einheitliche Funktionen und Funktionen für den Benutzer bereitstellen.

Im Fall der Beispiel-App monkeychat werden die Datenmodelle und der Verarbeitungs Code, z. b. Netzwerk-und Datenbankzugriff, in eine native Bibliothek verschoben.

Führen Sie folgende Schritte aus:

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Starten Sie Visual Studio für Mac, und öffnen Sie die monkeychat-app.
2. Klicken Sie im **Lösungspad** mit der rechten Maustaste auf den Projektmappennamen, und wählen Sie > neues Projekt **Hinzufügen** **...** aus: 

    [![](implementing-sirikit-images/prep01.png "Add a new project")](implementing-sirikit-images/prep01.png#lightbox)
3. Wählen Sie **IOS** > **Bibliothek** > **Klassenbibliothek** aus, und klicken Sie auf die Schaltfläche **weiter** : 

    [![](implementing-sirikit-images/prep02.png "Select Class Library")](implementing-sirikit-images/prep02.png#lightbox)
4. Geben Sie `MonkeyChatCommon` als **Namen** ein, und klicken Sie auf die Schaltfläche **Erstellen** : 

    [![](implementing-sirikit-images/prep03.png "Enter MonkeyChatCommon for the Name")](implementing-sirikit-images/prep03.png#lightbox)
5. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner **Verweise** der Haupt-APP, und wählen Sie **Verweise bearbeiten...** aus. Überprüfen Sie das Projekt **monkeychatcommon** , und klicken Sie auf die Schaltfläche **OK** : 

    [![](implementing-sirikit-images/prep05.png "Check the MonkeyChatCommon project")](implementing-sirikit-images/prep05.png#lightbox)
6. Ziehen Sie in der **Projektmappen-Explorer**den gemeinsamen freigegebenen Code aus der Haupt-app in die native Bibliothek.
7. Ziehen Sie im Fall von monkeychat die Ordner " **datamodels** " und " **Prozessoren** " aus der Haupt-app in die native Bibliothek: 

    [![](implementing-sirikit-images/prep06.png "The DataModels and Processors folders in the Solution Explorer")](implementing-sirikit-images/prep06.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Starten Sie Visual Studio, und öffnen Sie die monkeychat-app.
2. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektmappennamen, und wählen Sie > **Neues Projekt** **Hinzufügen** ... aus.
3. Wählen **Sie C# Visual** > frei gegebenes **Projekt** aus, und klicken Sie auf **weiter** : 

    [![](implementing-sirikit-images/prep02.w157-sml.png "Select Class Library")](implementing-sirikit-images/prep02.w157.png#lightbox)
4. Geben Sie `MonkeyChatCommon` als **Namen** ein, und klicken Sie auf die Schaltfläche **Erstellen** .
5. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner **Verweise** der Haupt-APP, und wählen Sie **Verweise bearbeiten...** aus. Überprüfen Sie das Projekt **monkeychatcommon** , und klicken Sie auf die Schaltfläche **OK** : 

    [![](implementing-sirikit-images/prep05w.png "Check the MonkeyChatCommon project")](implementing-sirikit-images/prep05w.png#lightbox)
6. Ziehen Sie in der **Projektmappen-Explorer**den gemeinsamen freigegebenen Code aus der Haupt-app in das freigegebene Projekt.
7. Ziehen Sie im Fall von monkeychat die Ordner " **datamodels** " und " **Prozessoren** " aus der Haupt-app in die native Bibliothek.

-----

Bearbeiten Sie alle Dateien, die in die native Bibliothek verschoben wurden, und ändern Sie den Namespace so, dass er mit dem der Bibliothek identisch ist. Beispielsweise können Sie `MonkeyChat` in `MonkeyChatCommon`ändern:

```csharp
using System;
namespace MonkeyChatCommon
{
    /// <summary>
    /// A message sent from one user to another within a conversation.
    /// </summary>
    public class MonkeyMessage
    {
        public MonkeyMessage ()
        {
        }
        ...
    }
}
```

Kehren Sie als nächstes zur Haupt-app zurück, und fügen Sie eine `using`-Anweisung für den Namespace der systemeigenen Bibliothek hinzu, wenn die APP eine der Klassen verwendet, die verschoben wurden:

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Foundation;
using CoreGraphics;
using MonkeyChatCommon;

namespace MonkeyChat
{
    public partial class MasterViewController : UITableViewController
    {
        public DetailViewController DetailViewController { get; set; }

        DataSource dataSource;
        ...
    }
}
```

### <a name="architecting-the-app-for-extensions"></a>Entwerfen der APP für Erweiterungen

In der Regel wird eine APP für mehrere Intents registrieren, und der Entwickler muss sicherstellen, dass die APP für die entsprechende Anzahl von beabsichtigten Erweiterungen entworfen wird.

In der Situation, in der eine APP mehr als eine Absicht erfordert, kann der Entwickler die gesamte beabsichtigte Behandlung in einer beabsichtigten Erweiterung platzieren oder eine separate Intent-Erweiterung für jede Absicht erstellen.

Wenn Sie sich für die Erstellung einer separaten Intent-Erweiterung für jede Absicht entscheiden, könnte der Entwickler eine große Menge an Code Bausteinen in jeder Erweiterung duplizieren und eine große Menge an Prozessor-und Speicher Mehraufwand erzeugen.

Um die Auswahl zwischen den beiden Optionen zu erleichtern, prüfen Sie, ob eine der Intents auf natürliche Weise zueinander gehört. Beispielsweise kann eine APP, die Audio-und Videoaufrufe durchgeführt hat, beide Intents in einer einzigen Intent-Erweiterung einschließen, da Sie ähnliche Aufgaben verarbeiten und die meisten Wiederverwendbarkeit von Code bereitstellen kann.

Erstellen Sie eine neue Intent-Erweiterung in der Projekt Mappe der APP, die Sie in der Projekt Mappe der App enthalten.

### <a name="setting-the-required-entitlements"></a>Festlegen der erforderlichen Berechtigungen

Für jede xamarin. IOS-APP, die die Sirikit-Integration einschließt, müssen die richtigen Berechtigungen festgelegt sein. Wenn der Entwickler diese erforderlichen Berechtigungen nicht ordnungsgemäß festgelegt hat, kann er die APP nicht auf Real IOS 10 (oder höher) Hardware installieren oder testen. Dies ist auch erforderlich, da der IOS 10-Simulator "Sirikit" nicht unterstützt.

Führen Sie folgende Schritte aus:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Doppelklicken Sie auf die `Entitlements.plist` Datei im **Projektmappen-Explorer** , um Sie für die Bearbeitung zu öffnen.
2. Wechseln Sie zur Registerkarte **Quelle** .
3. Fügen Sie die `com.apple.developer.siri`- **Eigenschaft**hinzu, legen Sie den **Typ** auf `Boolean` und den **Wert** auf `Yes`fest: 

    [![](implementing-sirikit-images/setup01.png "Add the com.apple.developer.siri Property")](implementing-sirikit-images/setup01.png#lightbox)
4. Speichern Sie die Änderungen in der Datei.
5. Doppelklicken Sie auf die **Projektdatei** im **Projektmappen-Explorer** , um Sie zur Bearbeitung zu öffnen.
6. Wählen Sie **IOS-Bündel Signierung** aus, und vergewissern Sie sich, dass im Feld **benutzerdefinierte Berechtigungen** die Datei `Entitlements.plist` ausgewählt ist 

    [![](implementing-sirikit-images/setup02.png "Select the Entitlements.plist file in the Custom Entitlements field")](implementing-sirikit-images/setup02.png#lightbox)
7. Klicken Sie auf die Schaltfläche **OK**, um die Änderungen zu speichern.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Doppelklicken Sie auf die `Entitlements.plist` Datei im **Projektmappen-Explorer** , um Sie für die Bearbeitung zu öffnen.
2. Fügen Sie die `com.apple.developer.siri`- **Eigenschaft**hinzu, legen Sie den **Typ** auf `Boolean` und den **Wert** auf `Yes`fest: 

    [![](implementing-sirikit-images/setup01w.png "Add the com.apple.developer.siri Property")](implementing-sirikit-images/setup01w.png#lightbox)
3. Speichern Sie die Änderungen in der Datei.
4. Doppelklicken Sie auf die **Projektdatei** im **Projektmappen-Explorer** , um Sie zur Bearbeitung zu öffnen.
5. Wählen Sie **IOS-Bündel Signierung** aus, und vergewissern Sie sich, dass im Feld **benutzerdefinierte Berechtigungen** die `Entitlements.plist` Datei ausgewählt ist

-----

Wenn Sie fertig sind, sollte die `Entitlements.plist` Datei der APP wie folgt aussehen (in einem externen Editor geöffnet):

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>com.apple.developer.siri</key>
    <true/>
</dict>
</plist>
```

### <a name="correctly-provisioning-the-app"></a>Ordnungsgemäße Bereitstellung der APP

Aufgrund der strengen Sicherheit, die Apple in Bezug auf das Sirikit-Framework platziert hat, _muss_ jede xamarin. IOS-APP, die das Sirikit implementiert, über die richtige APP-ID und Berechtigungen verfügen (siehe Abschnitt oben) und muss mit einem richtigen Bereitstellungs Profil signiert werden.

Führen Sie auf Ihrem Mac die folgenden Schritte aus:

1. Navigieren Sie in einem Webbrowser zu [https://developer.apple.com](https://developer.apple.com) , und melden Sie sich bei Ihrem Konto an.
2. Klicken Sieauf Zertifikate **,** Bezeichner und **profile**.
3. Wählen Sie **Bereitstellungs profile** und **App-IDs**aus, und klicken Sie dann auf die Schaltfläche **+** .
4. Geben Sie einen **Namen** für das neue Profil ein.
5. Geben Sie nach der namens Empfehlung von Apple eine **Bündel-ID** ein.
6. Scrollen Sie nach unten zum Abschnitt **App Services** , wählen Sie die Option **Sirikit** aus, und klicken Sie auf **weiter** 

    [![](implementing-sirikit-images/setup03.png "Select SiriKit")](implementing-sirikit-images/setup03.png#lightbox)
7. Überprüfen Sie alle Einstellungen, und über **Mitteln** Sie dann die APP-ID.
8. Wählen Sie **Bereitstellungs profile** > **Entwicklung**, klicken Sie auf die Schaltfläche **+** , wählen Sie die **Apple-ID**aus, und klicken Sie auf **weiter**
9. Klicken Sie auf **alle**auswählen und dann auf **weiter**.
10. Klicken Sie erneut auf **Alles auswählen** , und klicken Sie dann auf **weiter**.
11. Geben Sie mithilfe der Namensvorschläge von Apple einen **Profilnamen** ein, und klicken Sie dann auf **weiter**.
12. Starten Sie Xcode.
13. Wählen Sie im Menü Xcode die Option **Einstellungen...**
14. Wählen Sie **Konten**aus, und klicken Sie dann auf **Details anzeigen...** gedrückt 

    [![](implementing-sirikit-images/setup04.png "Select Accounts")](implementing-sirikit-images/setup04.png#lightbox)
15. Klicken Sie in der linken unteren Ecke auf die Schaltfläche **alle Profile herunterladen** : 

    [![](implementing-sirikit-images/setup05.png "Download All Profiles")](implementing-sirikit-images/setup05.png#lightbox)
16. Stellen Sie sicher, dass das oben erstellte **Bereitstellungs Profil** in Xcode installiert wurde.
17. Öffnen Sie das Projekt, um die Unterstützung von Sirikit in Visual Studio für Mac hinzuzufügen.
18. Doppelklicken Sie auf die `Info.plist` Datei in der **Projektmappen-Explorer**.
19. Stellen Sie sicher, dass die Bündel-ID mit der **Paket** -ID übereinstimmt, die Sie oben im Entwickler Portal 

    [![](implementing-sirikit-images/setup06.png "The Bundle Identifier")](implementing-sirikit-images/setup06.png#lightbox)
20. Wählen Sie im **Projektmappen-Explorer**das **Projekt**aus.
21. Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **Optionen**
22. Wählen Sie **IOS-Bündel Signierung**aus, wählen Sie die oben erstellte **Signatur Identität** und das **Bereitstellungs Profil** aus: 

    [![](implementing-sirikit-images/setup07.png "Select the Signing Identity and Provisioning Profile")](implementing-sirikit-images/setup07.png#lightbox)
23. Klicken Sie auf die Schaltfläche **OK**, um die Änderungen zu speichern.

> [!IMPORTANT]
> Das Testen von "Sirikit" funktioniert nur auf einem echten IOS 10-Hardware Gerät und nicht im IOS 10-Simulator. Wenn bei der Installation einer mit dem Sirikit aktivierten xamarin. IOS-App auf echter Hardwareprobleme auftreten, stellen Sie sicher, dass die erforderlichen Berechtigungen, die APP-ID, der Signatur Bezeichner und das Bereitstellungs Profil ordnungsgemäß im Entwickler Portal von Apple und Visual Studio für Mac konfiguriert wurden.

### <a name="requesting-siri-authorization"></a>Siri-Autorisierung anfordern

Bevor die APP Benutzerspezifisches Vokabular hinzufügt oder die Intents-Erweiterungen eine Verbindung mit Siri herstellen, muss Sie eine Autorisierung des Benutzers anfordern, um auf Siri zuzugreifen.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Bearbeiten Sie die `Info.plist` Datei der APP, wechseln Sie zur **Quell** Ansicht, und fügen Sie den `NSSiriUsageDescription` Schlüssel mit einem Zeichen folgen Wert hinzu, in dem beschrieben wird, wie die APP Siri verwendet und welche Datentypen gesendet werden. Die monkeychat-app könnte z. b. sagen, dass monkeychat-Kontakte an Siri gesendet werden:

[![](implementing-sirikit-images/request01.png "The NSSiriUsageDescription in the Info.plist editor")](implementing-sirikit-images/request01.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Bearbeiten Sie die `Info.plist` Datei der APP, und fügen Sie den `NSSiriUsageDescription` Schlüssel mit einem Zeichen folgen Wert hinzu, in dem beschrieben wird, wie die APP Siri verwendet und welche Datentypen gesendet werden. Die monkeychat-app könnte z. b. sagen, dass monkeychat-Kontakte an Siri gesendet werden:

[![](implementing-sirikit-images/request01w.png "The NSSiriUsageDescription in the Info.plist editor")](implementing-sirikit-images/request01w.png#lightbox)

-----

Ruft die `RequestSiriAuthorization`-Methode der `INPreferences`-Klasse auf, wenn die APP zum ersten Mal gestartet wird. Bearbeiten Sie die `AppDelegate.cs`-Klasse, und legen Sie die `FinishedLaunching`-Methode wie folgt an:

```csharp
using Intents;
...

public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{

    // Request access to Siri
    INPreferences.RequestSiriAuthorization ((INSiriAuthorizationStatus status) => {
        // Respond to returned status
        switch (status) {
        case INSiriAuthorizationStatus.Authorized:
            break;
        case INSiriAuthorizationStatus.Denied:
            break;
        case INSiriAuthorizationStatus.NotDetermined:
            break;
        case INSiriAuthorizationStatus.Restricted:
            break;
        }
    });

    return true;
}
```

Wenn diese Methode zum ersten Mal aufgerufen wird, wird eine Warnung angezeigt, die den Benutzer auffordert, der APP den Zugriff auf Siri zu gestatten. Die Meldung, die der Entwickler der obigen `NSSiriUsageDescription` hinzugefügt hat, wird in dieser Warnung angezeigt. Wenn der Benutzer den Zugriff anfänglich verweigert, kann er die app " **Einstellungen** " verwenden, um Zugriff auf die APP zu gewähren.

Die APP kann jederzeit die Fähigkeit der APP überprüfen, auf Siri zuzugreifen, indem Sie die `SiriAuthorizationStatus`-Methode der `INPreferences`-Klasse aufrufen.

### <a name="localization-and-siri"></a>Lokalisierung und Siri

Auf einem IOS-Gerät kann der Benutzer eine Sprache für Siri auswählen, die sich von der Standardeinstellung des Systems unterscheidet. Beim Arbeiten mit lokalisierten Daten muss die APP die `SiriLanguageCode`-Methode der `INPreferences`-Klasse verwenden, um den Sprachcode von Siri zu erhalten. Beispiel:

```csharp
var language = INPreferences.SiriLanguageCode();

// Take action based on language
if (language == "en-US") {
    // Do something...
}
```

### <a name="adding-user-specific-vocabulary"></a>Hinzufügen von Benutzer spezifischem Vokabular

Das benutzerspezifische Vokabular dient zum Bereitstellen von Wörtern oder Ausdrücken, die für einzelne Benutzer der APP eindeutig sind. Diese werden zur Laufzeit von der Haupt-app (nicht von den App-Erweiterungen) als geordneter Satz von Begriffen bereitgestellt, die in einer signifikantesten Nutzungs Priorität für die Benutzer, mit den wichtigsten Begriffen am Anfang der Liste, bereitgestellt werden.

Benutzerspezifisches Vokabular muss zu einer der folgenden Kategorien gehören:

- Kontaktnamen (die nicht vom Contacts-Framework verwaltet werden).
- Fototags.
- Foto-Album-Namen.
- Trainings Namen.

Wenn Sie die Terminologie auswählen, die als benutzerdefiniertes Vokabular registriert werden soll, wählen Sie nur Begriffe aus, die von einem Benutzer, der mit der APP nicht vertraut Registrieren Sie niemals gängige Begriffe wie "mein Training" oder "mein Album". Die monkeychat-App registriert z. b. die jedem Kontakt zugeordneten Nick names im Adressbuch des Benutzers.

Die APP stellt das benutzerspezifische Vokabular bereit, indem Sie die `SetVocabularyStrings`-Methode der `INVocabulary`-Klasse aufrufen und eine `NSOrderedSet`-Auflistung aus der Haupt-App übergibt. Die APP sollte immer zuerst die `RemoveAllVocabularyStrings`-Methode aufzurufen, um alle vorhandenen Begriffe zu entfernen, bevor Sie neue hinzufügen. Beispiel:

```csharp
using System;
using System.Linq;
using System.Collections.Generic;
using Foundation;
using Intents;

namespace MonkeyChatCommon
{
    public class MonkeyAddressBook : NSObject
    {
        #region Computed Properties
        public List<MonkeyContact> Contacts { get; set; } = new List<MonkeyContact> ();
        #endregion

        #region Constructors
        public MonkeyAddressBook ()
        {
        }
        #endregion

        #region Public Methods
        public NSOrderedSet<NSString> ContactNicknames ()
        {
            var nicknames = new NSMutableOrderedSet<NSString> ();

            // Sort contacts by the last time used
            var query = Contacts.OrderBy (contact => contact.LastCalledOn);

            // Assemble ordered list of nicknames by most used to least
            foreach (MonkeyContact contact in query) {
                nicknames.Add (new NSString (contact.ScreenName));
            }

            // Return names
            return new NSOrderedSet<NSString> (nicknames.AsSet ());
        }

        // This method MUST only be called on a background thread!
        public void UpdateUserSpecificVocabulary ()
        {
            // Clear any existing vocabulary
            INVocabulary.SharedVocabulary.RemoveAllVocabularyStrings ();

            // Register new vocabulary
            INVocabulary.SharedVocabulary.SetVocabularyStrings (ContactNicknames (), INVocabularyStringType.ContactName);
        }
        #endregion
    }
}
```

Wenn dieser Code vorhanden ist, kann er wie folgt aufgerufen werden:

```csharp
using System;
using System.Threading;
using UIKit;
using MonkeyChatCommon;
using Intents;

namespace MonkeyChat
{
    public partial class ViewController : UIViewController
    {
        #region AppDelegate Access
        public AppDelegate ThisApp {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do we have access to Siri?
            if (INPreferences.SiriAuthorizationStatus == INSiriAuthorizationStatus.Authorized) {
                // Yes, update Siri's vocabulary
                new Thread (() => {
                    Thread.CurrentThread.IsBackground = true;
                    ThisApp.AddressBook.UpdateUserSpecificVocabulary ();
                }).Start ();
            }
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
        #endregion
    }
}
```

> [!IMPORTANT]
> Siri behandelt benutzerdefiniertes Vokabular als Hinweise und wird so viel von der Terminologie wie möglich integrieren. Allerdings ist der Speicherplatz für das benutzerdefinierte Vokabular eingeschränkt, sodass _nur_ die Terminologie registriert werden kann, die verwirrend sein kann, sodass die Gesamtzahl der registrierten Bedingungen minimal bleibt.

Weitere Informationen finden Sie in unserer [benutzerspezifischen vokabularreferenz](~/ios/platform/sirikit/understanding-sirikit.md) und in der Apple- [Referenz zum Angeben eines benutzerdefinierten Vokabulars](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SpecifyingCustomVocabulary.html#//apple_ref/doc/uid/TP40016875-CH6-SW1).

### <a name="adding-app-specific-vocabulary"></a>Hinzufügen von Anwendungs spezifischem Vokabular

Das App-spezifische Vokabular definiert die Wörter und Ausdrücke, die allen Benutzern der APP, z. b. Fahrzeugtypen oder Trainings Namen, bekannt sein werden. Da diese Teil der Anwendung sind, werden Sie als Teil des Haupt App Bundle in einer `AppIntentVocabulary.plist` Datei definiert. Außerdem sollten diese Wörter und Ausdrücke lokalisiert werden.

App-spezifische vokabularbegriffe müssen zu einer der folgenden Kategorien gehören:

- Optionen für die Fahrt.
- Trainings Namen.

Die APP-spezifische Vokabulardatei enthält zwei Schlüssel auf Stamm Ebene:

- `ParameterVocabularies` **erforderlich** : definiert die benutzerdefinierten Begriffe und Intent-Parameter der APP, für die Sie gelten.
- `IntentPhrases` **optional** : enthält Beispiel Ausdrücke, die die benutzerdefinierten Begriffe verwenden, die in der `ParameterVocabularies`definiert sind.

Jeder Eintrag in der `ParameterVocabularies` muss eine ID-Zeichenfolge, einen Begriff und die Absicht angeben, für die der Begriff gilt. Außerdem kann es vorkommen, dass ein einzelner Begriff auf mehrere Intents anwendbar ist.

Eine vollständige Liste der zulässigen Werte und der erforderlichen Dateistruktur finden Sie in der [Referenz zu den App-vokabulardateiformaten](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CustomVocabularyKeys.html#//apple_ref/doc/uid/TP40016875-CH10-SW1)von Apple.

Um dem App-Projekt eine `AppIntentVocabulary.plist`-Datei hinzuzufügen, gehen Sie wie folgt vor:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektnamen, und wählen Sie > **neue Datei** **Hinzufügen** ... > **IOS**aus:

    [![](implementing-sirikit-images/plist01.png "Add a property list")](implementing-sirikit-images/plist01.png#lightbox)
2. Doppelklicken Sie auf die `AppIntentVocabulary.plist` Datei im **Projektmappen-Explorer** , um Sie für die Bearbeitung zu öffnen.
3. Klicken Sie auf die **+** , um einen Schlüssel hinzuzufügen, legen Sie den **Namen** auf `ParameterVocabularies` und den **Typ** auf `Array`fest:

    [![](implementing-sirikit-images/plist02.png "Set the Name to ParameterVocabularies and the Type to Array")](implementing-sirikit-images/plist02.png#lightbox)
4. Erweitern Sie `ParameterVocabularies`, und klicken Sie auf die Schaltfläche **+** , und legen Sie den **Typ** auf `Dictionary`

    [![](implementing-sirikit-images/plist03.png "Set the Type to Dictionary")](implementing-sirikit-images/plist03.png#lightbox)
5. Klicken Sie auf die **+** , um einen neuen Schlüssel hinzuzufügen, legen Sie den **Namen** auf `ParameterNames` und den **Typ** auf `Array`fest:

    [![](implementing-sirikit-images/plist04.png "Set the Name to ParameterNames and the Type to Array")](implementing-sirikit-images/plist04.png#lightbox)
6. Klicken Sie auf die **+** , um einen neuen Schlüssel mit dem **Typ** `String` und dem Wert als einen der verfügbaren Parameter Namen hinzuzufügen. Beispielsweise `INStartWorkoutIntent.workoutName`:

    [![](implementing-sirikit-images/plist05.png "The INStartWorkoutIntent.workoutName key")](implementing-sirikit-images/plist05.png#lightbox)
7. Fügen Sie dem `ParameterVocabularies` Schlüssel den `ParameterVocabulary` Schlüssel mit dem `Array`**Typ** hinzu:

    [![](implementing-sirikit-images/plist06.png "Add the ParameterVocabulary key to the ParameterVocabularies key with the Type of Array")](implementing-sirikit-images/plist06.png#lightbox)
8. Fügen Sie einen neuen Schlüssel mit dem **Typ** `Dictionary`hinzu:

    [![](implementing-sirikit-images/plist07.png "Add a new key with the Type of Dictionary")](implementing-sirikit-images/plist07.png#lightbox)
9. Fügen Sie den `VocabularyItemIdentifier` Schlüssel mit dem **Typ** `String` hinzu, und geben Sie eine eindeutige ID für den Begriff an:

    [![](implementing-sirikit-images/plist08.png "Add the VocabularyItemIdentifier key with the Type of String and specify a unique ID")](implementing-sirikit-images/plist08.png#lightbox)
10. Fügen Sie den `VocabularyItemSynonyms` Schlüssel mit dem **Typ** `Array`hinzu:

    [![](implementing-sirikit-images/plist09.png "Add the VocabularyItemSynonyms key with the Type of Array")](implementing-sirikit-images/plist09.png#lightbox)
11. Fügen Sie einen neuen Schlüssel mit dem **Typ** `Dictionary`hinzu:

    [![](implementing-sirikit-images/plist10.png "Add a new key with the Type of Dictionary")](implementing-sirikit-images/plist10.png#lightbox)
12. Fügen Sie den `VocabularyItemPhrase` Schlüssel mit dem **Typ** `String` und dem Begriff hinzu, der von der APP definiert wird:

    [![](implementing-sirikit-images/plist11.png "Add the VocabularyItemPhrase key with the Type of String and the term the app are defining")](implementing-sirikit-images/plist11.png#lightbox)
13. Fügen Sie den `VocabularyItemPronunciation` Schlüssel mit dem **Typ** `String` und der phonetischen Aussprache des Begriffs hinzu:

    [![](implementing-sirikit-images/plist12.png "Add the VocabularyItemPronunciation key with the Type of String and the phonetic pronunciation of the term")](implementing-sirikit-images/plist12.png#lightbox)
14. Fügen Sie den `VocabularyItemExamples` Schlüssel mit dem **Typ** `Array`hinzu:

    [![](implementing-sirikit-images/plist13.png "Add the VocabularyItemExamples key with the Type of Array")](implementing-sirikit-images/plist13.png#lightbox)
15. Fügen Sie einige `String` Schlüssel mit Beispiel Verwendung des Begriffs hinzu:

    [![](implementing-sirikit-images/plist14.png "Add a few String keys with example uses of the term")](implementing-sirikit-images/plist14.png#lightbox)
16. Wiederholen Sie die obigen Schritte für alle anderen benutzerdefinierten Begriffe, die von der APP definiert werden müssen.
17. Reduzieren Sie die `ParameterVocabularies` Taste.
18. Fügen Sie den `IntentPhrases` Schlüssel mit dem **Typ** `Array`hinzu:

    [![](implementing-sirikit-images/plist15.png "Add the IntentPhrases key with the Type of Array")](implementing-sirikit-images/plist15.png#lightbox)
19. Fügen Sie einen neuen Schlüssel mit dem **Typ** `Dictionary`hinzu:

    [![](implementing-sirikit-images/plist16.png "Add a new key with the Type of Dictionary")](implementing-sirikit-images/plist16.png#lightbox)
20. Fügen Sie den `IntentName` Schlüssel mit dem **Typ** `String` und der Absicht für das Beispiel hinzu:

    [![](implementing-sirikit-images/plist17.png "Add the IntentName key with the Type of String and Intent for the example")](implementing-sirikit-images/plist17.png#lightbox)
21. Fügen Sie den `IntentExamples` Schlüssel mit dem **Typ** `Array`hinzu:

    [![](implementing-sirikit-images/plist18.png "Add the IntentExamples key with the Type of Array")](implementing-sirikit-images/plist18.png#lightbox)
22. Fügen Sie einige `String` Schlüssel mit Beispiel Verwendung des Begriffs hinzu:

    [![](implementing-sirikit-images/plist19.png "Add a few String keys with example uses of the term")](implementing-sirikit-images/plist19.png#lightbox)
23. Wiederholen Sie die obigen Schritte für alle Absichten, die die APP benötigt, um die Beispiel Verwendung von zu ermöglichen.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektnamen, und wählen Sie **> Neues Element hinzufügen aus. > Eigenschaften Liste von Apple > > Info. plist**:

    [![](implementing-sirikit-images/plist01.w157-sml.png "Add a new Info.plist")](implementing-sirikit-images/plist01.w157.png#lightbox)

2. Doppelklicken Sie auf die `AppIntentVocabulary.plist` Datei im **Projektmappen-Explorer** , um Sie für die Bearbeitung zu öffnen.
3. Klicken Sie auf die **+** , um einen Schlüssel hinzuzufügen, legen Sie den **Namen** auf `ParameterVocabularies` und den **Typ** auf `Array`fest:

    [![](implementing-sirikit-images/plist02w.png "Set the Name to ParameterVocabularies and the Type to Array")](implementing-sirikit-images/plist02w.png#lightbox)
4. Erweitern Sie `ParameterVocabularies`, und klicken Sie auf die Schaltfläche **+** , und legen Sie den **Typ** auf `Dictionary`

    [![](implementing-sirikit-images/plist03w.png "Set the Type to Dictionary")](implementing-sirikit-images/plist03w.png#lightbox)
5. Klicken Sie auf die **+** , um einen neuen Schlüssel hinzuzufügen, legen Sie den **Namen** auf `ParameterNames` und den **Typ** auf `Array`fest:

    [![](implementing-sirikit-images/plist04w.png "Set the Name to ParameterNames and the Type to Array")](implementing-sirikit-images/plist04w.png#lightbox)
6. Klicken Sie auf die **+** , um einen neuen Schlüssel mit dem **Typ** `String` und dem Wert als einen der verfügbaren Parameter Namen hinzuzufügen. Beispielsweise `INStartWorkoutIntent.workoutName`:

    [![](implementing-sirikit-images/plist05w.png "The INStartWorkoutIntent.workoutName key")](implementing-sirikit-images/plist05w.png#lightbox)
7. Fügen Sie dem `ParameterVocabularies` Schlüssel den `ParameterVocabulary` Schlüssel mit dem `Array`**Typ** hinzu:

    [![](implementing-sirikit-images/plist06w.png "Add the ParameterVocabulary key to the ParameterVocabularies key with the Type of Array")](implementing-sirikit-images/plist06w.png#lightbox)
8. Fügen Sie einen neuen Schlüssel mit dem **Typ** `Dictionary`hinzu:

    [![](implementing-sirikit-images/plist07w.png "Add a new key with the Type of Dictionary")](implementing-sirikit-images/plist07w.png#lightbox)
9. Fügen Sie den `VocabularyItemIdentifier` Schlüssel mit dem **Typ** `String` hinzu, und geben Sie eine eindeutige ID für den Begriff an:

    [![](implementing-sirikit-images/plist08w.png "Add the VocabularyItemIdentifier key with the Type of String and specify a unique ID for the term")](implementing-sirikit-images/plist08w.png#lightbox)
10. Fügen Sie den `VocabularyItemSynonyms` Schlüssel mit dem **Typ** `Array`hinzu:

    [![](implementing-sirikit-images/plist09w.png "Add the VocabularyItemSynonyms key with the Type of Array")](implementing-sirikit-images/plist09w.png#lightbox)
11. Fügen Sie einen neuen Schlüssel mit dem **Typ** `Dictionary`hinzu:

    [![](implementing-sirikit-images/plist10w.png "Add a new key with the Type of Dictionary")](implementing-sirikit-images/plist10w.png#lightbox)
12. Fügen Sie den `VocabularyItemPhrase` Schlüssel mit dem **Typ** `String` und dem Begriff hinzu, der von der APP definiert wird:

    [![](implementing-sirikit-images/plist11w.png "Add the VocabularyItemPhrase key with the Type of String and the term the app are defining")](implementing-sirikit-images/plist11w.png#lightbox)
13. Fügen Sie den `VocabularyItemPronunciation` Schlüssel mit dem **Typ** `String` und der phonetischen Aussprache des Begriffs hinzu:

    [![](implementing-sirikit-images/plist12w.png "Add the VocabularyItemPronunciation key with the Type of String and the phonetic pronunciation of the term")](implementing-sirikit-images/plist12w.png#lightbox)
14. Fügen Sie den `VocabularyItemExamples` Schlüssel mit dem **Typ** `Array`hinzu:

    [![](implementing-sirikit-images/plist13w.png "Add the VocabularyItemExamples key with the Type of Array")](implementing-sirikit-images/plist13w.png#lightbox)
15. Fügen Sie einige `String` Schlüssel mit Beispiel Verwendung des Begriffs hinzu:

    [![](implementing-sirikit-images/plist14w.png "Add a few String keys with example uses of the term")](implementing-sirikit-images/plist14w.png#lightbox)
16. Wiederholen Sie die obigen Schritte für alle anderen benutzerdefinierten Begriffe, die von der APP definiert werden müssen.
17. Reduzieren Sie die `ParameterVocabularies` Taste.
18. Fügen Sie den `IntentPhrases` Schlüssel mit dem **Typ** `Array`hinzu:

    [![](implementing-sirikit-images/plist15w.png "Add the IntentPhrases key with the Type of Array")](implementing-sirikit-images/plist15w.png#lightbox)
19. Fügen Sie einen neuen Schlüssel mit dem **Typ** `Dictionary`hinzu:

    [![](implementing-sirikit-images/plist16w.png "Add a new key with the Type of Dictionary")](implementing-sirikit-images/plist16w.png#lightbox)
20. Fügen Sie den `IntentName` Schlüssel mit dem **Typ** `String` und der Absicht für das Beispiel hinzu:

    [![](implementing-sirikit-images/plist17w.png "Add the IntentName key with the Type of String and Intent for the example")](implementing-sirikit-images/plist17w.png#lightbox)
21. Fügen Sie den `IntentExamples` Schlüssel mit dem **Typ** `Array`hinzu:

    [![](implementing-sirikit-images/plist18w.png "Add the IntentExamples key with the Type of Array")](implementing-sirikit-images/plist18w.png#lightbox)
22. Fügen Sie einige `String` Schlüssel mit Beispiel Verwendung des Begriffs hinzu:

    [![](implementing-sirikit-images/plist19w.png "Add a few String keys with example uses of the term")](implementing-sirikit-images/plist19w.png#lightbox)
23. Wiederholen Sie die obigen Schritte für alle Absichten, die die APP benötigt, um die Beispiel Verwendung von zu ermöglichen.

-----

> [!IMPORTANT]
> Die `AppIntentVocabulary.plist` wird während der Entwicklung bei Siri auf den Testgeräten registriert, und es kann einige Zeit dauern, bis Siri das benutzerdefinierte Vokabular einbezieht. Folglich muss der Tester einige Minuten warten, bevor er versucht, ein App-spezifisches Vokabular zu testen, wenn es aktualisiert wurde.

Weitere Informationen finden Sie in unserer [App-spezifischen vokabularreferenz](~/ios/platform/sirikit/understanding-sirikit.md) und in der Apple- [Referenz für benutzerdefinierte Vokabulare](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SpecifyingCustomVocabulary.html#//apple_ref/doc/uid/TP40016875-CH6-SW1).

## <a name="adding-an-intents-extension"></a>Hinzufügen einer Intents-Erweiterung

Nun, da die APP für die Übernahme von Sirikit vorbereitet wurde, muss der Entwickler der Projekt Mappe eine (oder mehrere) Intents-Erweiterungen hinzufügen, um die Intents zu verarbeiten, die für die Siri-Integration erforderlich sind.

Führen Sie für jede erforderliche Erweiterung die folgenden Schritte aus:

- Fügen Sie der xamarin. IOS-App-Projekt Mappe ein Intents-Erweiterungsprojekt hinzu.
- Konfigurieren Sie die Intents-Erweiterung `Info.plist` Datei.
- Ändern Sie die Intents-Erweiterungs Hauptklasse.

Weitere Informationen finden Sie in unserer [Referenz zur Intents-Erweiterung](~/ios/platform/sirikit/understanding-sirikit.md) und in [der Referenz zur Erstellung der Intents-Erweiterung](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CreatingtheIntentsExtension.html#//apple_ref/doc/uid/TP40016875-CH4-SW1)von Apple.

### <a name="creating-the-extension"></a>Erstellen der Erweiterung

Gehen Sie folgendermaßen vor, um der Projekt Mappe eine Intents-Erweiterung hinzuzufügen:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Klicken Sie im **Lösungspad** mit der rechten Maustaste auf den Projektmappennamen, und wählen Sie **Hinzufügen** >  **Neues Projekt hinzufügen**aus.
2. Wählen Sie im Dialogfeld **IOS** - > **Erweiterungen** > **Intent Extension** aus, und klicken Sie auf die Schaltfläche **weiter** : 

    [![](implementing-sirikit-images/intents05.png "Select Intent Extension")](implementing-sirikit-images/intents05.png#lightbox)
3. Geben Sie anschließend einen **Namen** für die beabsichtigte Erweiterung ein, und klicken Sie auf die Schaltfläche **weiter** : 

    [![](implementing-sirikit-images/intents06.png "Enter a Name for the Intent Extension")](implementing-sirikit-images/intents06.png#lightbox)
4. Klicken Sie abschließend auf die Schaltfläche **Erstellen** , um der APP-Lösung die Intent-Erweiterung hinzuzufügen: 

    [![](implementing-sirikit-images/intents07.png "Add the Intent Extension to the apps solution")](implementing-sirikit-images/intents07.png#lightbox)
5. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner **Verweise** der neu erstellten Intent-Erweiterung. Überprüfen Sie den Namen des gemeinsamen freigegebenen Code Bibliotheks Projekts (das oben von der App erstellt wurde), und klicken Sie auf die Schaltfläche **OK** : 

    [![](implementing-sirikit-images/intents08.png "Select the name of the common shared code library project")](implementing-sirikit-images/intents08.png#lightbox)
    
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektmappennamen, und wählen Sie **Hinzufügen** >  **Neues Projekt hinzufügen**aus.
2. Wählen Sie im Dialogfeld **Visual C# > IOS Extensions > Intent Extension** aus, und klicken Sie auf die Schaltfläche **weiter** :

    [![](implementing-sirikit-images/intents05.w157-sml.png "Select Intent Extension")](implementing-sirikit-images/intents05.w157.png#lightbox)
3. Geben Sie anschließend einen **Namen** für die beabsichtigte Erweiterung ein, und klicken Sie auf die Schaltfläche **OK** .
4. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner **Verweise** der neu erstellten Intents-Erweiterung, und wählen Sie **> Verweis hinzufügen**aus. Überprüfen Sie den Namen des gemeinsamen freigegebenen Code Bibliotheks Projekts (das oben von der App erstellt wurde), und klicken Sie auf die Schaltfläche **OK** :

    [![](implementing-sirikit-images/intents08w.png "Select the name of the common shared code library project")](implementing-sirikit-images/intents08w.png#lightbox)
    
-----

Wiederholen Sie diese Schritte für die Anzahl der Intent-Erweiterungen (basierend auf der Architektur des obigen Abschnitts " [App für Erweiterungen](#architecting-the-app-for-extensions) "), die für die APP erforderlich ist.

### <a name="configuring-the-infoplist"></a>Konfigurieren von "Info. plist"

Für jede Intents-Erweiterung, die der Projekt Mappe der app hinzugefügt wurde, muss in den `Info.plist` Dateien konfiguriert werden, damit Sie mit der APP verwendet werden kann.

Ebenso wie jede typische App-Erweiterung verfügt die APP über die vorhandenen Schlüssel `NSExtension` und `NSExtensionAttributes`. Für eine Intents-Erweiterung gibt es zwei neue Attribute, die konfiguriert werden müssen:

[![](implementing-sirikit-images/intents01.png "The two new attributes that must be configured")](implementing-sirikit-images/intents01.png#lightbox)

- **Intentssupported** : ist erforderlich und besteht aus einem Array von Intent-Klassennamen, die die APP von der Intent-Erweiterung unterstützen möchte.
- **Intentsrestrictedwhilelocked** : ist ein optionaler Schlüssel für die APP, um das Verhalten des Sperr Bildschirms der Erweiterung anzugeben. Er besteht aus einem Array von Intent-Klassennamen, die die APP für die Verwendung in der Intent-Erweiterung benötigen soll.

Um die `Info.plist` Datei der Intent-Erweiterung zu konfigurieren, doppelklicken Sie in der **Projektmappen-Explorer** auf die Datei, um Sie zur Bearbeitung zu öffnen. Wechseln Sie als nächstes zur **Quell** Ansicht, und erweitern Sie die `NSExtension`-und `NSExtensionAttributes`-Schlüssel im Editor:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![](implementing-sirikit-images/intents02.png "The NSExtension and NSExtensionAttributes keys in the editor")](implementing-sirikit-images/intents02.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](implementing-sirikit-images/intents02w.png "The NSExtension and NSExtensionAttributes keys in the editor")](implementing-sirikit-images/intents02w.png#lightbox)

-----

Erweitern Sie den `IntentsSupported` Schlüssel, und fügen Sie den Namen einer beliebigen Intent-Klasse hinzu, die diese Erweiterung unterstützt. Für die Beispiel-monkeychat-APP wird die `INSendMessageIntent`unterstützt:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![](implementing-sirikit-images/intents09.png "The INSendMessageIntent key")](implementing-sirikit-images/intents09.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](implementing-sirikit-images/intents09w.png "The INSendMessageIntent key")](implementing-sirikit-images/intents09w.png#lightbox)

-----

Wenn die APP optional erfordert, dass der Benutzer beim Gerät angemeldet ist, um eine bestimmte Absicht zu verwenden, erweitern Sie den `IntentRestrictedWhileLocked`-Schlüssel, und fügen Sie die Klassennamen der Intents mit eingeschränktem Zugriff hinzu. Für die Beispiel-monkeychat-app muss der Benutzer angemeldet sein, um eine Chat Nachricht zu senden, damit wir `INSendMessageIntent`hinzugefügt haben:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![](implementing-sirikit-images/intents10.png "The added INSendMessageIntent key")](implementing-sirikit-images/intents10.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](implementing-sirikit-images/intents10w.png "The added INSendMessageIntent key")](implementing-sirikit-images/intents10w.png#lightbox)

-----

Eine umfassende Liste der verfügbaren Intent-Domänen finden Sie in der Referenz zu den [beabsichtigten Domänen](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2)von Apple.

### <a name="configuring-the-main-class"></a>Konfigurieren der Hauptklasse

Als nächstes muss der Entwickler die Hauptklasse konfigurieren, die als Haupteinstiegspunkt für die Intent-Erweiterung in Siri fungiert. Dabei muss es sich um eine Unterklasse von `INExtension` handeln, die dem `IINIntentHandler` Delegaten entspricht. Beispiel:

```csharp
using System;
using System.Collections.Generic;

using Foundation;
using Intents;

namespace MonkeyChatIntents
{
    [Register ("IntentHandler")]
    public class IntentHandler : INExtension, IINSendMessageIntentHandling, IINSearchForMessagesIntentHandling, IINSetMessageAttributeIntentHandling
    {
        #region Constructors
        protected IntentHandler (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override NSObject GetHandler (INIntent intent)
        {
            // This is the default implementation.  If you want different objects to handle different intents,
            // you can override this and return the handler you want for that particular intent.

            return this;
        }
        #endregion
        ...
    }
}
```

Es gibt eine einzelne Methode, die die APP für die Intent Extension-Hauptklasse implementieren muss, die `GetHandler`-Methode. Dieser Methode wird eine Absicht von Sirikit übergeben, und die APP muss einen **Intent-Handler** zurückgeben, der mit dem Typ der angegebenen Absicht übereinstimmt.

Da die Beispiel-monkeychat-app nur eine Absicht behandelt, wird Sie selbst in der `GetHandler`-Methode zurückgegeben. Wenn die Erweiterung mehr als eine Absicht verarbeitet hat, würde der Entwickler eine Klasse für jeden Intent-Typ hinzufügen und hier basierend auf dem an die-Methode weiter gegebenen `Intent` eine-Instanz zurückgeben.

### <a name="handling-the-resolve-stage"></a>Behandeln der Auflösungsphase

In der Auflösungsphase werden die von Siri über gebenden Parameter von der Intent-Erweiterung verdeutlicht und überprüft und über die Konversation des Benutzers festgelegt.

Für jeden Parameter, der von Siri gesendet wird, gibt es eine `Resolve`-Methode. Diese Methode muss von der APP für jeden Parameter implementiert werden, der von der APP möglicherweise benötigt wird, um die richtige Antwort vom Benutzer zu erhalten.

Im Fall der monkeychat-Beispiel-App benötigt die Intent-Erweiterung mindestens einen Empfänger, an den die Nachricht gesendet werden soll. Für jeden Empfänger in der Liste muss die Erweiterung eine Kontaktsuche durchführen, die folgendes Ergebnis aufweisen kann:

- Genau ein übereinstimmender Kontakt wurde gefunden.
- Es wurden mindestens zwei übereinstimmende Kontakte gefunden.
- Es wurden keine übereinstimmenden Kontakte gefunden.

Darüber hinaus erfordert monkeychat Inhalt für den Nachrichtentext. Wenn der Benutzer dies nicht angegeben hat, muss Siri den Benutzer zur Eingabe des Inhalts auffordern.

Die Intent-Erweiterung muss jeden dieser Fälle ordnungsgemäß behandeln.

```csharp
[Export ("resolveRecipientsForSearchForMessages:withCompletion:")]
public void ResolveRecipients (INSendMessageIntent intent, Action<INPersonResolutionResult []> completion)
{
    var recipients = intent.Recipients;
    // If no recipients were provided we'll need to prompt for a value.
    if (recipients.Length == 0) {
        completion (new INPersonResolutionResult [] { (INPersonResolutionResult)INPersonResolutionResult.NeedsValue });
        return;
    }

    var resolutionResults = new List<INPersonResolutionResult> ();

    foreach (var recipient in recipients) {
        var matchingContacts = new INPerson [] { recipient }; // Implement your contact matching logic here to create an array of matching contacts
        if (matchingContacts.Length > 1) {
            // We need Siri's help to ask user to pick one from the matches.
            resolutionResults.Add (INPersonResolutionResult.GetDisambiguation (matchingContacts));
        } else if (matchingContacts.Length == 1) {
            // We have exactly one matching contact
            resolutionResults.Add (INPersonResolutionResult.GetSuccess (recipient));
        } else {
            // We have no contacts matching the description provided
            resolutionResults.Add ((INPersonResolutionResult)INPersonResolutionResult.Unsupported);
        }
    }

    completion (resolutionResults.ToArray ());
}

[Export ("resolveContentForSendMessage:withCompletion:")]
public void ResolveContent (INSendMessageIntent intent, Action<INStringResolutionResult> completion)
{
    var text = intent.Content;
    if (!string.IsNullOrEmpty (text))
        completion (INStringResolutionResult.GetSuccess (text));
    else
        completion ((INStringResolutionResult)INStringResolutionResult.NeedsValue);
}
```

Weitere Informationen finden Sie in der Referenz zu [den Auflösungs Stufen](~/ios/platform/sirikit/understanding-sirikit.md) und in der Referenz zu den [Auflösungs-und Behandlungs Absichten](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/ResolvingandHandlingIntents.html#//apple_ref/doc/uid/TP40016875-CH5-SW1)von Apple.

### <a name="handling-the-confirm-stage"></a>Verarbeiten der Bestätigungsphase

In der Bestätigungsphase prüft die Intent-Erweiterung, ob Sie alle Informationen zur Erfüllung der Anforderung des Benutzers enthält. Die APP möchte die Bestätigung zusammen mit allen unterstützenden Details zu den Vorgängen, die mit Siri ausgeführt werden sollen, damit Sie mit dem Benutzer bestätigt werden kann, dass dies die gewünschte Aktion ist.

```csharp
[Export ("confirmSendMessage:completion:")]
public void ConfirmSendMessage (INSendMessageIntent intent, Action<INSendMessageIntentResponse> completion)
{
    // Verify user is authenticated and the app is ready to send a message.
    ...

    var userActivity = new NSUserActivity (nameof (INSendMessageIntent));
    var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Ready, userActivity);
    completion (response);
}
```

Weitere Informationen finden Sie in [der Referenz zum Bestätigen der Stufe](~/ios/platform/sirikit/understanding-sirikit.md).

### <a name="processing-the-intent"></a>Verarbeiten der Absicht

Dies ist der Punkt, an dem die beabsichtigte Erweiterung die Aufgabe tatsächlich durchführt, um die Anforderung des Benutzers zu erfüllen und die Ergebnisse zurück an Siri zu übergeben, damit der Benutzer informiert werden kann.

```csharp
public void HandleSendMessage (INSendMessageIntent intent, Action<INSendMessageIntentResponse> completion)
{
    // Implement the application logic to send a message here.
    ...

    var userActivity = new NSUserActivity (nameof (INSendMessageIntent));
    var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Success, userActivity);
    completion (response);
}

public void HandleSearchForMessages (INSearchForMessagesIntent intent, Action<INSearchForMessagesIntentResponse> completion)
{
    // Implement the application logic to find a message that matches the information in the intent.
    ...

    var userActivity = new NSUserActivity (nameof (INSearchForMessagesIntent));
    var response = new INSearchForMessagesIntentResponse (INSearchForMessagesIntentResponseCode.Success, userActivity);

    // Initialize with found message's attributes
    var sender = new INPerson (new INPersonHandle ("sarah@example.com", INPersonHandleType.EmailAddress), null, "Sarah", null, null, null);
    var recipient = new INPerson (new INPersonHandle ("+1-415-555-5555", INPersonHandleType.PhoneNumber), null, "John", null, null, null);
    var message = new INMessage ("identifier", "I am so excited about SiriKit!", NSDate.Now, sender, new INPerson [] { recipient });
    response.Messages = new INMessage [] { message };
    completion (response);
}

public void HandleSetMessageAttribute (INSetMessageAttributeIntent intent, Action<INSetMessageAttributeIntentResponse> completion)
{
    // Implement the application logic to set the message attribute here.
    ...

    var userActivity = new NSUserActivity (nameof (INSetMessageAttributeIntent));
    var response = new INSetMessageAttributeIntentResponse (INSetMessageAttributeIntentResponseCode.Success, userActivity);
    completion (response);
}
```

Weitere Informationen finden Sie in [der Referenz zur Handle-Stufe](~/ios/platform/sirikit/understanding-sirikit.md).

## <a name="adding-an-intents-ui-extension"></a>Hinzufügen einer Intents-Benutzeroberflächen Erweiterung

Die optionale Intents-Benutzeroberflächen Erweiterung bietet die Möglichkeit, die Benutzeroberfläche und das Branding der app in die Siri-Oberfläche zu bringen und die Benutzer mit der app in Verbindung zu bringen. Mit dieser Erweiterung kann die APP die Marke sowie visuelle und andere Informationen in das Transkript einbringen.

[![](implementing-sirikit-images/intentsui01.png "An example Intents UI Extension output")](implementing-sirikit-images/intentsui01.png#lightbox)

Ebenso wie die Intents-Erweiterung führt der Entwickler den folgenden Schritt für die Intents-Benutzeroberflächen Erweiterung aus:

- Fügen Sie der xamarin. IOS-App-Projekt Mappe ein Intents-UI-Erweiterungsprojekt hinzu.
- Konfigurieren Sie die Intents-Benutzeroberflächen Erweiterung `Info.plist` Datei.
- Ändern Sie die Hauptklasse der Intents-Benutzeroberflächen Erweiterung.

Weitere Informationen finden Sie in unserer [Referenz zur Intents-Benutzeroberflächen Erweiterung](~/ios/platform/sirikit/understanding-sirikit.md) und in der Apple- [Bereitstellung eines benutzerdefinierten Schnittstellen Verweises](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/ProvidingaCustomInterface.html#//apple_ref/doc/uid/TP40016875-CH7-SW1).

### <a name="creating-the-extension"></a>Erstellen der Erweiterung

Gehen Sie folgendermaßen vor, um der Projekt Mappe eine Intents-Benutzeroberflächen Erweiterung hinzuzufügen:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Klicken Sie im **Lösungspad** mit der rechten Maustaste auf den Projektmappennamen, und wählen Sie **Hinzufügen** >  **Neues Projekt hinzufügen**aus.
2. Wählen Sie im Dialogfeld **IOS** - > **Erweiterungen** > **Intent UI Extension** aus, und klicken Sie auf die Schaltfläche **weiter** : 

    [![](implementing-sirikit-images/intents11.png "Select Intent UI Extension")](implementing-sirikit-images/intents11.png#lightbox)
3. Geben Sie anschließend einen **Namen** für die beabsichtigte Erweiterung ein, und klicken Sie auf die Schaltfläche **weiter** : 

    [![](implementing-sirikit-images/intents12.png "Enter a Name for the Intent Extension")](implementing-sirikit-images/intents12.png#lightbox)
4. Klicken Sie abschließend auf die Schaltfläche **Erstellen** , um der APP-Lösung die Intent-Erweiterung hinzuzufügen: 

    [![](implementing-sirikit-images/intents13.png "Add the Intent Extension to the apps solution")](implementing-sirikit-images/intents13.png#lightbox)
5. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner **Verweise** der neu erstellten Intent-Erweiterung. Überprüfen Sie den Namen des gemeinsamen freigegebenen Code Bibliotheks Projekts (das oben von der App erstellt wurde), und klicken Sie auf die Schaltfläche **OK** : 

    [![](implementing-sirikit-images/intents14.png "Select the name of the common shared code library project")](implementing-sirikit-images/intents14.png#lightbox)
    
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektmappennamen, und wählen Sie **Hinzufügen** >  **Neues Projekt hinzufügen...**
2. Wählen Sie im Dialogfeld **IOS** - > **Erweiterungen** > **Intent UI Extension** aus, und klicken Sie auf die Schaltfläche **weiter** .
3. Geben Sie anschließend einen **Namen** für die beabsichtigte Erweiterung ein, und klicken Sie auf die Schaltfläche **OK** .
4. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf den Ordner **Verweise** der neu erstellten Intent-Erweiterung. Überprüfen Sie den Namen des gemeinsamen freigegebenen Code Bibliotheks Projekts (das Sie oben erstellt haben), und klicken Sie auf die Schaltfläche **OK** .
    
-----

### <a name="configuring-the-infoplist"></a>Konfigurieren von "Info. plist"

Konfigurieren Sie die `Info.plist` Datei der Intents-Benutzeroberflächen Erweiterung, um mit der APP zu arbeiten.

Ebenso wie jede typische App-Erweiterung verfügt die APP über die vorhandenen Schlüssel `NSExtension` und `NSExtensionAttributes`. Für eine Intents-Erweiterung gibt es ein neues Attribut, das konfiguriert werden muss:

[![](implementing-sirikit-images/intents03.png "The one new attribute that must be configured")](implementing-sirikit-images/intents03.png#lightbox)

**Intentssupported** ist erforderlich und besteht aus einem Array von Intent-Klassennamen, die von der APP von der Intent-Erweiterung unterstützt werden sollen.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Um die `Info.plist` Datei der Intent UI-Erweiterung zu konfigurieren, doppelklicken Sie in der **Projektmappen-Explorer** auf diese, um Sie zur Bearbeitung zu öffnen. Wechseln Sie als nächstes zur **Quell** Ansicht, und erweitern Sie die `NSExtension`-und `NSExtensionAttributes`-Schlüssel im Editor:

[![](implementing-sirikit-images/intents04.png "The NSExtension and NSExtensionAttributes keys in the editor")](implementing-sirikit-images/intents04.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Um die `Info.plist` Datei der Intent UI-Erweiterung zu konfigurieren, doppelklicken Sie in der **Projektmappen-Explorer** auf diese, um Sie zur Bearbeitung zu öffnen. Erweitern Sie die `NSExtension`-und `NSExtensionAttributes`-Schlüssel im Editor:

[![](implementing-sirikit-images/intents04w.png "Tthe NSExtension and NSExtensionAttributes keys in the editor")](implementing-sirikit-images/intents04w.png#lightbox)

-----

Erweitern Sie den `IntentsSupported` Schlüssel, und fügen Sie den Namen einer beliebigen Intent-Klasse hinzu, die diese Erweiterung unterstützt. Für die Beispiel-monkeychat-APP wird die `INSendMessageIntent`unterstützt:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![](implementing-sirikit-images/intents15.png "The INSendMessageIntent key")](implementing-sirikit-images/intents15.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](implementing-sirikit-images/intents15w.png "The INSendMessageIntent key")](implementing-sirikit-images/intents15w.png#lightbox)

-----

Eine umfassende Liste der verfügbaren Intent-Domänen finden Sie in der Referenz zu den [beabsichtigten Domänen](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2)von Apple.

### <a name="configuring-the-main-class"></a>Konfigurieren der Hauptklasse

Konfigurieren Sie die Hauptklasse, die als Haupteinstiegspunkt für die beabsichtigte UI-Erweiterung in Siri fungiert. Dabei muss es sich um eine Unterklasse von `UIViewController` handeln, die der `IINUIHostedViewController`-Schnittstelle entspricht. Beispiel:

```csharp
using System;
using Foundation;
using CoreGraphics;
using Intents;
using IntentsUI;
using UIKit;

namespace MonkeyChatIntentsUI
{
    public partial class IntentViewController : UIViewController, IINUIHostedViewControlling
    {
        #region Constructors
        protected IntentViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }

        public override void DidReceiveMemoryWarning ()
        {
            // Releases the view if it doesn't have a superview.
            base.DidReceiveMemoryWarning ();

            // Release any cached data, images, etc that aren't in use.
        }
        #endregion

        #region Public Methods
        [Export ("configureWithInteraction:context:completion:")]
        public void Configure (INInteraction interaction, INUIHostedViewContext context, Action<CGSize> completion)
        {
            // Do configuration here, including preparing views and calculating a desired size for presentation.

            if (completion != null)
                completion (DesiredSize ());
        }

        [Export ("desiredSize:")]
        public CGSize DesiredSize ()
        {
            return ExtensionContext.GetHostedViewMaximumAllowedSize ();
        }
        #endregion
    }
}
```

Siri übergibt eine `INInteraction`-Klasseninstanz an die `Configure`-Methode der `UIViewController` Instanz in der Erweiterung der Intent-Benutzeroberfläche.

Das `INInteraction`-Objekt stellt drei wichtige Informationselemente für die Erweiterung bereit:

1. Das beabsichtigte Objekt, das verarbeitet wird.
2. Das Intent Response-Objekt aus den `Confirm`-und `Handle` Methoden der Intent-Erweiterung.
3. Der Interaktions Zustand, der den Zustand der Interaktion zwischen der APP und Siri definiert.

Die `UIViewController` Instanz ist die Prinzipal Klasse für die Interaktion mit Siri und erbt von `UIViewController`, da Sie Zugriff auf alle Features von UIKit hat.

Wenn Siri die `Configure`-Methode des `UIViewController` aufruft, wird ein Ansichts Kontext weitergeleitet, der besagt, dass der Ansichts Controller entweder in einer Siri-snippit-oder Maps-Karte gehostet wird.

Siri übergibt außerdem einen Abschluss Handler, den die APP benötigt, um die gewünschte Größe der Sicht zurückzugeben, nachdem die APP die Konfiguration abgeschlossen hat.

### <a name="design-the-ui-in-ios-designer"></a>Entwerfen der Benutzeroberfläche im IOS-Designer

Layout der Intents-Benutzeroberfläche der UI-Erweiterung im IOS-Designer. Doppelklicken Sie auf die `MainInterface.storyboard` Datei der Erweiterung im **Projektmappen-Explorer** , um Sie für die Bearbeitung zu öffnen. Ziehen Sie alle erforderlichen Benutzeroberflächen Elemente, um die Benutzeroberfläche zu erstellen und die Änderungen zu speichern.

> [!IMPORTANT]
> Obwohl es möglich ist, dem `UIViewController`der Intent UI-Erweiterung interaktive Elemente hinzuzufügen, z. b. `UIButtons` oder `UITextFields`, sind diese als beabsichtigte Benutzeroberfläche in nicht interaktiv unzulässig, und der Benutzer ist nicht in der Lage, mit Ihnen zu interagieren.

### <a name="wire-up-the-user-interface"></a>Übertragen Sie die Benutzeroberfläche.

`UIViewController` bearbeiten Sie im IOS-Designer die Benutzeroberfläche der Intents-Benutzeroberflächen Erweiterung, und überschreiben Sie die `Configure`-Methode wie folgt:

```csharp
[Export ("configureWithInteraction:context:completion:")]
public void Configure (INInteraction interaction, INUIHostedViewContext context, Action<CGSize> completion)
{
    // Do configuration here, including preparing views and calculating a desired size for presentation.
    ...

    // Return desired size
    if (completion != null)
        completion (DesiredSize ());
}

[Export ("desiredSize:")]
public CGSize DesiredSize ()
{
    return ExtensionContext.GetHostedViewMaximumAllowedSize ();
}
```

### <a name="overriding-the-default-siri-ui"></a>Überschreiben der standardmäßigen Siri-UI

Die Intents-Benutzeroberflächen Erweiterung wird immer zusammen mit anderen Siri-Inhalten angezeigt, z. b. dem App-Symbol und dem Namen oben auf der Benutzeroberfläche, oder auf der Grundlage der Absicht werden möglicherweise Schaltflächen (z. b. "Senden" oder "Abbrechen") unten angezeigt.

Es gibt einige Fälle, in denen die APP die Informationen ersetzen kann, die von Siri standardmäßig für den Benutzer angezeigt werden, z. b. Nachrichten oder Zuordnungen, bei denen die APP die Standarddarstellung durch eine für die APP angepasste ersetzen kann.

Wenn die Intents-Benutzeroberflächen Erweiterung Elemente der standardmäßigen Siri-Benutzeroberfläche überschreiben muss, muss die `UIViewController`-Unterklasse die `IINUIHostedViewSiriProviding`-Schnittstelle implementieren und die Anzeige eines bestimmten Schnittstellen Elements abonnieren.

Fügen Sie der `UIViewController`-Unterklasse den folgenden Code hinzu, um Siri mitzuteilen, dass die Intent UI-Erweiterung bereits den Nachrichten Inhalt anzeigt:

```csharp
public bool DisplaysMessage {
    get {return true;}
}
```

### <a name="considerations"></a>Weitere Überlegungen

Apple schlägt vor, dass der Entwickler beim Entwerfen und Implementieren der Intent-UI-Erweiterungen die folgenden Aspekte berücksichtigt:

- **Achten Sie auf die Speicherauslastung** , da beabsichtigte UI-Erweiterungen temporär sind und nur für kurze Zeit angezeigt werden. das System erzwingt strengere Speicher Einschränkungen als bei einer vollständigen APP verwendet werden.
- **Beachten Sie die minimalen und maximalen Ansichts Größen** . Stellen Sie sicher, dass die Ziel Erweiterungen der Benutzeroberfläche für alle IOS-Gerätetypen, Größe und Ausrichtung geeignet sind. Außerdem kann die gewünschte Größe, die die APP an Siri zurücksendet, möglicherweise nicht erteilt werden.
- **Verwenden Sie ein flexibles und Adaptives Layoutmuster** , um sicherzustellen, dass die Benutzeroberfläche auf jedem Gerät hervorragend aussieht.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde "Sirikit" erläutert, und es wurde gezeigt, wie es den xamarin. IOS-apps hinzugefügt werden kann, um Dienste bereitzustellen, auf die der Benutzer mit Siri und der Maps-APP auf einem IOS-Gerät zugreifen kann.

## <a name="related-links"></a>Verwandte Links

- [Elizachat-Beispiel](https://docs.microsoft.com/samples/xamarin/ios-samples/ios10-elizachat)
- [Sirikit-Programmier Handbuch](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [Intents Framework-Referenz](https://developer.apple.com/reference/intents)
- [Intents UI-frameworkreferenz](https://developer.apple.com/reference/intentsui)
- [Referenz Domänen Referenz](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2)
