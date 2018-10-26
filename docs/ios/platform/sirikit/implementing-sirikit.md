---
title: Implementieren von SiriKit in Xamarin.iOS
description: Dieses Dokument beschreibt die erforderlichen Schritte zum Implementieren von SiriKit-Unterstützung in einem Xamarin.iOS-apps. Intents-Erweiterungen und Intents UI-Erweiterungen werden erörtert.
ms.prod: xamarin
ms.assetid: 20FFB981-EB10-48BA-BF79-40F37F0291EB
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/03/2018
ms.openlocfilehash: ea037aaaac97d9f326f1a2fbcb28d97c9d8a9b45
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50110250"
---
# <a name="implementing-sirikit-in-xamarinios"></a>Implementieren von SiriKit in Xamarin.iOS

_Dieser Artikel behandelt die erforderlichen Schritte zum Implementieren von SiriKit-Unterstützung in einem Xamarin.iOS-apps._

Neue iOS 10, SiriKit kann eine Xamarin.iOS-app, um Dienste bereitzustellen, die für den Benutzer mithilfe von Siri und die Karten-app auf einem iOS-Gerät zugänglich sind. Dieser Artikel behandelt die Schritte zum Implementieren von SiriKit-Unterstützung in den Xamarin.iOS-apps durch Hinzufügen der erforderlichen Intents-Erweiterungen, die Intents UI-Erweiterungen und Vokabular.

Siri arbeitet mit dem Konzept der **Domänen**, wissen, Gruppen von Aktionen für verwandte Aufgaben. Jede Interaktion mit die app mit Siri muss wie folgt in eine Domäne bekannten Diensts liegen:

- Audio- oder Videoanruf.
- Buchung einer Tour an.
- Verwalten von fitnessaktivitäten aus.
- Messaging.
- Durchsuchen von Fotos.
- Senden oder Empfangen von Zahlungen.

Wenn der Benutzer eine Anforderung von Siri im Zusammenhang mit einer der Dienste für die App-Erweiterung stellt, sendet SiriKit die Erweiterung einer **Absicht** Objekt, das die Anforderung des Benutzers sowie alle unterstützungsdaten beschreibt. Die App-Erweiterung generiert dann das entsprechende **Antwort** -Objekt für den angegebenen **Absicht**, mit Informationen, wie die Erweiterung für die Anforderung verarbeiten kann.

Dieses Handbuch stellt ein kurzes Beispiel einschließlich SiriKit-Unterstützung in eine vorhandene app dar. Dieses Beispiel wird die gefälschte MonkeyChat-app verwendet werden:

[![](implementing-sirikit-images/monkeychat01.png "Das Symbol \"MonkeyChat\"")](implementing-sirikit-images/monkeychat01.png#lightbox)

MonkeyChat behält eine eigene wenden Sie sich an Buchs für die Freunde des Benutzers, die jeweils einem Bildschirmnamen (z. B. Bobo z. B.) zugeordnet, und ermöglicht dem Benutzer jeder Freund von seinem Bildschirmnamen Chats von Text an.

## <a name="extending-the-app-with-sirikit"></a>Erweitern Sie die App mit SiriKit

Siehe die [Grundlegendes zu SiriKit-Konzepten](~/ios/platform/sirikit/understanding-sirikit.md) Guide, es gibt drei Hauptkomponenten beteiligt, erweitern eine app mit SiriKit:

[![](implementing-sirikit-images/elements01.png "Erweitern der App durch SiriKit-Diagramm")](implementing-sirikit-images/elements01.png#lightbox)

Dazu gehören:

1. **Intents-Erweiterung** -überprüft die Antworten der Benutzer, bestätigt die app kann die Anforderung verarbeiten und tatsächlich wird die Aufgabe zum Erfüllen der Anforderung des Benutzers.
2. **Intents-Benutzeroberflächenerweiterung** - *Optional*, stellt eine benutzerdefinierte Benutzeroberfläche, die Antworten in der Umgebung Siri und die UI-apps integrieren und branding in Siri des Benutzers zu bereichern.
3. **App** -Benutzer bestimmte Vokabulare bei Siri, bei der Arbeit mit der sie die App bietet. 

Alle diese Elemente und die Schritte zum Einschließen in die app wird in den folgenden Abschnitten ausführlich behandelt.


## <a name="preparing-the-app"></a>Vorbereiten der App

SiriKit basiert auf Erweiterungen, bevor die app keine Erweiterungen hinzufügen, es gibt jedoch einige Dinge, die der Entwickler, um Hilfe bei der Übernahme von SiriKit muss.

### <a name="moving-common-shared-code"></a>Verschieben von gemeinsam verwendeten Code 

Der Entwickler kann verschieben Sie zunächst einige der gemeinsamen Code, der freigegeben wird zwischen der app und die Erweiterungen in einer _freigegebene Projekte_, _Portable Class Libraries (PCLs)_ oder _Native Bibliotheken_.

Die Erweiterungen müssen in der Lage alles tun, die die app ausführt. Hinsichtlich der MonkeyChat beispielanwendung Dinge wie das Suchen nach Kontakten, Hinzufügen neuer Kontakte und Senden von Nachrichten, und Nachrichtenverlauf abrufen.

Durch das Verschieben von diesen gemeinsamen Codes in ein freigegebenes Projekt, eine PCL oder eine Native Bibliothek ganz einfach, Code in einem gemeinsamen Ort zu verwalten, und es wird sichergestellt, dass die Erweiterung und die übergeordnete app einheitliche Umgebungen und Funktionalität für den Benutzer bereitzustellen.

Im Falle der Beispiel-app MonkeyChat werden für Datenmodelle und von Verarbeitungscode wie z. B. Zugriff auf Netzwerk und Datenbank in eine Native Bibliothek verschoben werden.

Führen Sie folgende Schritte aus:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Starten Sie Visual Studio für Mac, und öffnen Sie die app MonkeyChat.
2. Mit der rechten Maustaste auf den Namen der Projektmappe in die **Lösungspad** , und wählen Sie **hinzufügen** > **neues Projekt...** : 

    [![](implementing-sirikit-images/prep01.png "Hinzufügen eines neuen Projekts")](implementing-sirikit-images/prep01.png#lightbox)
3. Wählen Sie **iOS** > **Bibliothek** > **Klassenbibliothek** , und klicken Sie auf die **Weiter** Schaltfläche: 

    [![](implementing-sirikit-images/prep02.png "Wählen Sie die Klassenbibliothek")](implementing-sirikit-images/prep02.png#lightbox)
4. Geben Sie `MonkeyChatCommon` für die **Namen** , und klicken Sie auf die **erstellen** Schaltfläche: 

    [![](implementing-sirikit-images/prep03.png "Geben Sie ein MonkeyChatCommon")](implementing-sirikit-images/prep03.png#lightbox)
5. Mit der rechten Maustaste auf die **Verweise** Ordner der Haupt-app in der **Projektmappen-Explorer** , und wählen Sie **Verweise bearbeiten...** . Überprüfen Sie die **MonkeyChatCommon** Projekt, und klicken Sie auf die **OK** Schaltfläche: 

    [![](implementing-sirikit-images/prep05.png "Überprüfen Sie das Projekt MonkeyChatCommon")](implementing-sirikit-images/prep05.png#lightbox)
6. In der **Projektmappen-Explorer**, ziehen Sie den gemeinsamen verwendeten Code aus der Haupt-app, die Native Bibliothek.
7. Im Fall von MonkeyChat, ziehen Sie die **DataModels** und **Prozessoren** Ordner aus der Haupt-app in der nativen Bibliothek: 

    [![](implementing-sirikit-images/prep06.png "Die DataModels und Prozessoren Ordner im Projektmappen-Explorer")](implementing-sirikit-images/prep06.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Starten Sie Visual Studio, und öffnen Sie die app MonkeyChat.
2. Mit der rechten Maustaste auf den Namen der Projektmappe in die **Projektmappen-Explorer** , und wählen Sie **hinzufügen** > **neues Projekt...** .
3. Wählen Sie **Visual C#**   >  **freigegebenes Projekt** , und klicken Sie auf die **Weiter** Schaltfläche: 

    [![](implementing-sirikit-images/prep02.w157-sml.png "Wählen Sie die Klassenbibliothek")](implementing-sirikit-images/prep02.w157.png#lightbox)
4. Geben Sie `MonkeyChatCommon` für die **Namen** , und klicken Sie auf die **erstellen** Schaltfläche.
5. Mit der rechten Maustaste auf die **Verweise** Ordner der Haupt-app in der **Projektmappen-Explorer** , und wählen Sie **Verweise bearbeiten...** . Überprüfen Sie die **MonkeyChatCommon** Projekt, und klicken Sie auf die **OK** Schaltfläche: 

    [![](implementing-sirikit-images/prep05w.png "Überprüfen Sie das Projekt MonkeyChatCommon")](implementing-sirikit-images/prep05w.png#lightbox)
6. In der **Projektmappen-Explorer**, ziehen Sie den gemeinsamen verwendeten Code aus der Haupt-app auf das freigegebene Projekt.
7. Im Fall von MonkeyChat, ziehen Sie die **DataModels** und **Prozessoren** Ordner aus der Haupt-app in der nativen Bibliothek.

-----

Bearbeiten Sie die Dateien, die an die Native Bibliothek verschoben wurden, und ändern Sie den Namespace der Bibliothek entsprechen. Ändern Sie z. B. `MonkeyChat` zu `MonkeyChatCommon`:

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

Als Nächstes wechseln Sie zurück zur Haupt-app und Hinzufügen einer `using` -Anweisung für die Native Bibliothek-Namespace eine beliebige Stelle die app verwendet eine der Klassen, die verschoben wurden:

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

### <a name="architecting-the-app-for-extensions"></a>Architektur der App-Erweiterungen

In der Regel wird eine app für mehrere Intents registrieren, und der Entwickler muss sicherstellen, dass die app für die entsprechende Anzahl von Erweiterungen der Absicht entworfen wird.

In der Situation, in denen eine app mehr als eine Absicht erfordert, hat der Entwickler die Möglichkeit, platzieren alle seine beabsichtigte Behandlung in eine Ziel-Erweiterung oder das Erstellen einer separaten Intent-Erweiterung für jeden Zweck.

Wenn separate Absicht Erweiterung für jeden Zweck erstellen, kann der Entwickler am Ende eine große Menge an Standardcode in jede Erweiterung duplizieren und erstellen eine große Menge an Prozessor- und speichermehraufwand.

Hilfe bei zwischen den beiden Optionen angezeigt, wenn keines der Intent-Elemente auf natürliche Weise zusammengehören. Beispielsweise eine app, Audio-und Videoanrufe kann beide diese Intent-Elemente in einer Erweiterung Absicht einschließen, wie sie ähnliche Aufgaben behandeln und kann die meisten Wiederverwendung von Code bereitstellen.

Erstellen Sie eine neue Erweiterung für beabsichtigte Lesevorgänge für alle Absicht oder eine Gruppe von Intent-Elemente, die nicht in einer vorhandenen Gruppe entsprechen, in der app-Projektmappe, in diese enthalten.


### <a name="setting-the-required-entitlements"></a>Festlegen der erforderlichen Berechtigungen

Alle Xamarin.iOS-app, die Integration von SiriKit umfasst, müssen die richtigen Berechtigungen festgelegt haben. Wenn der Entwickler diese erforderlichen Berechtigungen nicht ordnungsgemäß festgelegt nicht ist, sie ist nicht möglich zu installieren, oder testen Sie die app auf echte iOS 10 (oder höher) Hardware ist erforderlich, da das iOS 10-Simulator nicht SiriKit unterstützt.

Führen Sie folgende Schritte aus:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Doppelklicken Sie auf die `Entitlements.plist` Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen.
2. Wechseln Sie zu der **Quelle** Registerkarte.
3. Hinzufügen der `com.apple.developer.siri` **Eigenschaft**legen die **Typ** zu `Boolean` und **Wert** zu `Yes`: 

    [![](implementing-sirikit-images/setup01.png "Fügen Sie die Eigenschaft com.apple.developer.siri")](implementing-sirikit-images/setup01.png#lightbox)
4. Speichern Sie die Änderungen in der Datei.
5. Doppelklicken Sie auf die **Projektdatei** in die **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen.
6. Wählen Sie **iOS Bundle-Signierung** und sicherstellen, dass die `Entitlements.plist` Datei ausgewählt ist, der **benutzerdefinierte Berechtigungen** Feld: 

    [![](implementing-sirikit-images/setup02.png "Wählen Sie die Datei \"Entitlements.plist\" in das Feld \"benutzerdefinierte Berechtigungen\"")](implementing-sirikit-images/setup02.png#lightbox)
7. Klicken Sie auf die Schaltfläche **OK**, um die Änderungen zu speichern.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Doppelklicken Sie auf die `Entitlements.plist` Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen.
3. Hinzufügen der `com.apple.developer.siri` **Eigenschaft**legen die **Typ** zu `Boolean` und **Wert** zu `Yes`: 

    [![](implementing-sirikit-images/setup01w.png "Fügen Sie die Eigenschaft com.apple.developer.siri")](implementing-sirikit-images/setup01w.png#lightbox)
4. Speichern Sie die Änderungen in der Datei.
5. Doppelklicken Sie auf die **Projektdatei** in die **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen.
6. Wählen Sie **iOS Bundle-Signierung** und sicherstellen, dass die `Entitlements.plist` Datei ausgewählt ist, der **benutzerdefinierte Berechtigungen** Feld.

-----

Wenn Sie fertig sind, handelt es sich bei der app `Entitlements.plist` Datei sollte wie folgt aussehen (in in einem externen Editor geöffnet):

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

### <a name="correctly-provisioning-the-app"></a>Ordnungsgemäße Bereitstellen der App

Da die strikte Sicherheit, die Apple rund um die SiriKit-Framework, einer Xamarin.iOS-app platziert werden, die SiriKit implementiert _müssen_ verfügen über die richtige App-ID und Berechtigungen (siehe Abschnitt oben) und muss mit einem richtigen signiert sein Bereitstellungsprofil.

Führen Sie auf Ihrem Mac Folgendes ein:

1. Wechseln Sie in einem Webbrowser zu [ http://developer.apple.com ](http://developer.apple.com) und melden Sie sich bei Ihrem Konto.
2. Klicken Sie auf **Zertifikate**, **Bezeichner** und **Profile**.
3. Wählen Sie **Bereitstellungsprofile** , und wählen Sie **App-IDs**, klicken Sie dann auf die **+** Schaltfläche.
4. Geben Sie einen **Namen** für das neue Profil.
5. Geben Sie einen **Bündel-ID** folgenden Apple die Empfehlung benennen.
6. Führen Sie einen Bildlauf nach unten, um die **App Services** wählen Sie im Abschnitt **SiriKit** , und klicken Sie auf die **Weiter** Schaltfläche: 

    [![](implementing-sirikit-images/setup03.png "Auswählen von SiriKit")](implementing-sirikit-images/setup03.png#lightbox)
7. Überprüfen Sie alle Einstellungen, klicken Sie dann **senden** der App ID.
8. Wählen Sie **Bereitstellungsprofile** > **Entwicklung**, klicken Sie auf die **+** Klicken die **Apple-ID**, Klicken Sie dann auf **Weiter**.
9. Klicken Sie auf auswählen **alle**, klicken Sie dann auf **Weiter**.
10. Klicken Sie auf **Alles markieren** erneut aus, klicken Sie dann auf **Weiter**.
11. Geben Sie einen **Profilname** mithilfe von Apple, der Vorschläge benennen, klicken Sie dann auf **Weiter**.
12. Starten Sie Xcode.
13. Wählen Sie aus der Xcode-Menü **Einstellungen...**
14. Wählen Sie **Konten**, klicken Sie dann auf die **Details anzeigen...** Schaltfläche: 

    [![](implementing-sirikit-images/setup04.png "Wählen Sie Konten")](implementing-sirikit-images/setup04.png#lightbox)
15. Klicken Sie auf die **alle Profile herunterladen** Schaltfläche in der unteren linken Ecke: 

    [![](implementing-sirikit-images/setup05.png "Alle Profile herunterladen")](implementing-sirikit-images/setup05.png#lightbox)
16. Sicherstellen, dass die **Bereitstellungsprofil** erstellt über Xcode installiert wurde.
17. Öffnen Sie das Projekt zum Hinzufügen von SiriKit-Support, um in Visual Studio für Mac.
18. Doppelklicken Sie auf die `Info.plist` Datei die **Projektmappen-Explorer**.
18. Sicherstellen, dass die **Bündel-ID** entspricht, die im Apple Developer Portal oben erstellt haben: 

    [![](implementing-sirikit-images/setup06.png "Die Bündel-ID")](implementing-sirikit-images/setup06.png#lightbox)
18. In der **Projektmappen-Explorer**, wählen die **Projekt**.
19. Mit der rechten Maustaste in des Projekts, und wählen Sie **Optionen**.
21. Wählen Sie **iOS Bundle-Signierung**, wählen die **Signierungsidentität** und **Bereitstellungsprofil** oben erstellt haben: 

    [![](implementing-sirikit-images/setup07.png "Wählen Sie Signieridentität und Bereitstellungsprofil")](implementing-sirikit-images/setup07.png#lightbox)
22. Klicken Sie auf die Schaltfläche **OK**, um die Änderungen zu speichern.

> [!IMPORTANT]
> Testen von SiriKit kann nur für eine echte iOS 10 Hardwaregerät und nicht in das iOS 10 Simulator. Wenn Probleme bei der Installation einer SiriKit Xamarin.iOS-app auf echter Hardware aktiviert werden, stellen Sie sicher, dass die erforderlichen Berechtigungen, die App-ID, die Signatur-ID und die Bereitstellungsprofil ordnungsgemäß im Apple Entwicklerportal anmelden und Visual Studio für Mac konfiguriert wurden

### <a name="requesting-siri-authorization"></a>Siri-Autorisierung anfordern

Bevor die app fügt spezifische Programmierterminologie für Benutzer oder die Intents-Erweiterungen, die eine Verbindung herstellt, Siri, muss es fordert die Autorisierung des Benutzers Siri den Zugriff auf.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Bearbeiten der app `Info.plist` Datei, wechseln Sie zu der **Quelle** anzeigen und Hinzufügen der `NSSiriUsageDescription` -Schlüssel mit dem String-Wert, der beschreibt, wie die app Siri und was verwendet Datentypen gesendet werden. Beispielsweise könnte die MonkeyChat-app das "MonkeyChat Kontakte an Siri gesendet werden":

[![](implementing-sirikit-images/request01.png "Die NSSiriUsageDescription in der Datei \"Info.plist\"-editor")](implementing-sirikit-images/request01.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Bearbeiten der app `Info.plist` -Datei und fügen die `NSSiriUsageDescription` -Schlüssel mit dem String-Wert, der beschreibt, wie die app Siri und was verwendet Datentypen gesendet werden. Beispielsweise könnte die MonkeyChat-app das "MonkeyChat Kontakte an Siri gesendet werden":

[![](implementing-sirikit-images/request01w.png "Die NSSiriUsageDescription in der Datei \"Info.plist\"-editor")](implementing-sirikit-images/request01w.png#lightbox)

-----

Rufen Sie die `RequestSiriAuthorization` Methode der `INPreferences` Klasse beim ersten der app Start. Bearbeiten der `AppDelegate.cs` Klasse, und stellen die `FinishedLaunching` Methode sehen wie folgt:


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

Beim ersten, die diese Methode aufgerufen wird, wird eine Warnung angezeigt, wenn der Benutzer aufgefordert, die app Siri zugreifen kann. Die Meldung, die vom Entwickler hinzugefügt. die `NSSiriUsageDescription` oben in dieser Warnung angezeigt. Wenn der Benutzer zunächst den Zugriff ablehnt, können sie die **Einstellungen** app Zugriff auf die app zu gewähren.

Zu jedem Zeitpunkt kann die app überprüfen, die app Zugriff auf Siri durch Aufrufen der `SiriAuthorizationStatus` Methode der `INPreferences` Klasse.

### <a name="localization-and-siri"></a>Lokalisierung und Siri

Der Benutzer kann auf einem iOS-Gerät wählen Sie eine Sprache für Siri, die andere ist dann die Standardeinstellung des Systems. Bei der Arbeit mit lokalisierten Daten die app zu verwenden, muss die `SiriLanguageCode` -Methode der der `INPreferences` Klasse, um den Sprachcode von Siri abzurufen. Zum Beispiel:

```csharp
var language = INPreferences.SiriLanguageCode();

// Take action based on language
if (language == "en-US") {
    // Do something...
}
```

### <a name="adding-user-specific-vocabulary"></a>Hinzufügen von Benutzer und spezifische Programmierterminologie

Benutzer spezifische Programmierterminologie wird Geben Sie Wörter oder Ausdrücke, die für einzelne Benutzer der app eindeutig sind. Diese werden zur Laufzeit aus der Haupt-app (nicht der App-Erweiterungen) als einen geordneten Satz von Bedingungen, die in einer wichtigsten Nutzung Priorität für die Benutzer mit den wichtigsten Begriffen am Anfang der Liste sortiert angegeben werden.

Benutzer spezifische Programmierterminologie muss auf einen der folgenden Kategorien gehören:

- Wenden Sie sich an die Namen (die durch das Framework Kontakte nicht verwaltet werden).
- Fototags.
- Fotoalbum Namen.
- Trainings-Namen.

Bei der Terminologie als angepassten vokabularanpassung registrieren Sie ausgewählt haben, wählen Sie nur Begriffe, die möglicherweise von einer Person nicht vertraut sind, mit der app missverstanden werden. Nie Register allgemeine Begriffe wie "Meine Trainings" oder "Mein Album". Die MonkeyChat-app wird z. B. die einzelnen Kontakte im Adressbuch des Benutzers zugeordneten Spitznamen registriert.

Die app bietet spezifische Programmierterminologie Benutzer durch Aufrufen der `SetVocabularyStrings` Methode der `INVocabulary` Klasse und übergeben ein `NSOrderedSet` Auflistung aus der Haupt-app. Rufen Sie die app sollte immer die `RemoveAllVocabularyStrings` Methode zunächst zum Entfernen aller vorhandenen Bestimmungen, die vor dem Hinzufügen von neuen Knoten. Zum Beispiel:

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

Mit diesem Code werden können sie wie folgt aufgerufen werden:

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
> Siri angepassten vokabularanpassung als Hinweise behandelt und wird so großen Teil der Terminologie wie möglich zu integrieren. Jedoch Speicherplatz für das angepassten vokabularanpassung beschränkt, sodass es wichtig, zu registrieren ist _nur_ die Terminologie, die möglicherweise verwirrend, halten daher die Gesamtzahl der registrierten Bedingungen auf ein Minimum.

Weitere Informationen finden Sie unsere [bestimmten Vokabular Benutzerverweis](~/ios/platform/sirikit/understanding-sirikit.md) und Apple [angeben von benutzerdefinierten Vokabular Verweis](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SpecifyingCustomVocabulary.html#//apple_ref/doc/uid/TP40016875-CH6-SW1).

### <a name="adding-app-specific-vocabulary"></a>Hinzufügen von App-spezifische Vokabular

Der App-spezifische Programmierterminologie definiert die bestimmte Wörter und Ausdrücke an, die bekannt sind, an alle Benutzer der app, wie z. B. Fahrzeugtypen oder Trainings-Namen. Da diese Teil der Anwendung sind, werden sie in definiert eine `AppIntentVocabulary.plist` -Datei als Teil des Haupt-app-Pakets. Darüber hinaus sollte diese Wörter und Ausdrücke lokalisiert werden.

App-spezifische Vokabular Begriffe müssen auf einen der folgenden Kategorien gehören:

- Zeigt Optionen an.
- Trainings-Namen.

Die App-spezifische Programmierterminologie-Datei enthält zwei Schlüssel für die Stamm-Ebene:

- `ParameterVocabularies` **Erforderliche** -definiert benutzerdefinierten Bedingungen und Intent-Parameter, die sie für gelten der app.
- `IntentPhrases` **Optionale** -enthält Beispiel-Ausdrücke, die mit den benutzerdefinierten Bedingungen definiert, der `ParameterVocabularies`.

Jeder Eintrag in der `ParameterVocabularies` müssen eine ID-Zeichenfolge, Laufzeit und die Absicht, die der Begriff bezieht sich auf angeben. Darüber hinaus kann ein einzelner Ausdruck auf mehrere Intent-Elemente anwenden.

Eine vollständige Liste der zulässigen Werte und die erforderliche Datei-Struktur, finden Sie in Apple [App Vokabular Datei Formatreferenz für Blobüberwachungsprotokolle](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CustomVocabularyKeys.html#//apple_ref/doc/uid/TP40016875-CH10-SW1).

Hinzufügen einer `AppIntentVocabulary.plist` -Datei in das app-Projekt, gehen Sie folgendermaßen vor:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Mit der rechten Maustaste in den Namen des Projekts die **Projektmappen-Explorer** , und wählen Sie **hinzufügen** > **neue Datei...**   >  **iOS**:

    [![](implementing-sirikit-images/plist01.png "Fügen Sie eine Eigenschaftenliste hinzu.")](implementing-sirikit-images/plist01.png#lightbox)
2. Doppelklicken Sie auf die `AppIntentVocabulary.plist` Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen.
3. Klicken Sie auf die **+** um einen Schlüssel hinzuzufügen, legen Sie die **Namen** zu `ParameterVocabularies` und **Typ** zu `Array`:

    [![](implementing-sirikit-images/plist02.png "Legen Sie den Namen ParameterVocabularies und der Typ, Arrays")](implementing-sirikit-images/plist02.png#lightbox)
4. Erweitern Sie `ParameterVocabularies` , und klicken Sie auf die **+** Schaltfläche, und legen Sie die **Typ** zu `Dictionary`:

    [![](implementing-sirikit-images/plist03.png "Legen Sie den Typ zum Wörterbuch")](implementing-sirikit-images/plist03.png#lightbox)
5. Klicken Sie auf die **+** um einen neuen Schlüssel hinzuzufügen, legen Sie die **Namen** zu `ParameterNames` und **Typ** zu `Array`:

    [![](implementing-sirikit-images/plist04.png "Legen Sie den Namen ParameterNames und der Typ, Arrays")](implementing-sirikit-images/plist04.png#lightbox)
6. Klicken Sie auf die **+** , fügen einen neuen Schlüssel mit dem **Typ** von `String` und den Wert als eine der verfügbaren Parameter. Z. B. `INStartWorkoutIntent.workoutName`:

    [![](implementing-sirikit-images/plist05.png "Der Schlüssel INStartWorkoutIntent.workoutName")](implementing-sirikit-images/plist05.png#lightbox)
7. Hinzufügen der `ParameterVocabulary` -Schlüssel in der `ParameterVocabularies` Schlüssel, der **Typ** von `Array`:

    [![](implementing-sirikit-images/plist06.png "Fügen Sie den ParameterVocabulary-Schlüssel auf den ParameterVocabularies Schlüssel mit dem Typ von Array")](implementing-sirikit-images/plist06.png#lightbox)
8. Fügen Sie einen neuen Schlüssel mit dem **Typ** von `Dictionary`:

    [![](implementing-sirikit-images/plist07.png "Fügen Sie einen neuen Schlüssel mit dem Typ von-Wörterbuch hinzu.")](implementing-sirikit-images/plist07.png#lightbox)
9. Hinzufügen der `VocabularyItemIdentifier` -Schlüssel mit dem **Typ** von `String` , und geben Sie eine eindeutige ID für den Begriff:

    [![](implementing-sirikit-images/plist08.png "Fügen Sie den VocabularyItemIdentifier Schlüssel mit dem Typ der Zeichenfolge ein, und geben Sie eine eindeutige ID")](implementing-sirikit-images/plist08.png#lightbox)
10. Hinzufügen der `VocabularyItemSynonyms` -Schlüssel mit dem **Typ** von `Array`:

    [![](implementing-sirikit-images/plist09.png "Fügen Sie den VocabularyItemSynonyms Schlüssel mit dem Typ von Array")](implementing-sirikit-images/plist09.png#lightbox)
11. Fügen Sie einen neuen Schlüssel mit dem **Typ** von `Dictionary`:

    [![](implementing-sirikit-images/plist10.png "Fügen Sie einen neuen Schlüssel mit dem Typ von-Wörterbuch hinzu.")](implementing-sirikit-images/plist10.png#lightbox)
12. Hinzufügen der `VocabularyItemPhrase` -Schlüssel mit dem **Typ** von `String` und den Begriff die app definieren:

    [![](implementing-sirikit-images/plist11.png "Fügen Sie den VocabularyItemPhrase Schlüssel mit dem Typ der Zeichenfolge und der Begriff, die die app definieren")](implementing-sirikit-images/plist11.png#lightbox)
13. Hinzufügen der `VocabularyItemPronunciation` Schlüssel, der **Typ** von `String` und die phonetische Aussprache der Dauer:

    [![](implementing-sirikit-images/plist12.png "Fügen Sie den VocabularyItemPronunciation Schlüssel mit dem Typ der Zeichenfolge und die phonetische Aussprache des Begriffs")](implementing-sirikit-images/plist12.png#lightbox)
14. Hinzufügen der `VocabularyItemExamples` -Schlüssel mit dem **Typ** von `Array`:

    [![](implementing-sirikit-images/plist13.png "Fügen Sie den VocabularyItemExamples Schlüssel mit dem Typ von Array")](implementing-sirikit-images/plist13.png#lightbox)
15. Fügen Sie ein Paar `String` Schlüssel für die Verwendung von der Laufzeit:

    [![](implementing-sirikit-images/plist14.png "Fügen Sie einige Zeichenfolgenschlüssel mit für die Verwendung von der Laufzeit hinzu")](implementing-sirikit-images/plist14.png#lightbox)
16. Wiederholen Sie die oben genannten Schritte für sonstige benutzerdefinierte Bedingungen definieren, müssen die app ein.
17. Reduzieren der `ParameterVocabularies` Schlüssel.
18. Hinzufügen der `IntentPhrases` -Schlüssel mit dem **Typ** von `Array`:

    [![](implementing-sirikit-images/plist15.png "Fügen Sie den IntentPhrases Schlüssel mit dem Typ von Array")](implementing-sirikit-images/plist15.png#lightbox)
19. Fügen Sie einen neuen Schlüssel mit dem **Typ** von `Dictionary`:

    [![](implementing-sirikit-images/plist16.png "Fügen Sie einen neuen Schlüssel mit dem Typ von-Wörterbuch hinzu.")](implementing-sirikit-images/plist16.png#lightbox)
20. Hinzufügen der `IntentName` Schlüssel, der **Typ** von `String` und beabsichtigte für das Beispiel:

    [![](implementing-sirikit-images/plist17.png "Fügen Sie den IntentName Schlüssel mit dem Typ der Zeichenfolge und die beabsichtigte Lesevorgänge für das Beispiel")](implementing-sirikit-images/plist17.png#lightbox)
21. Hinzufügen der `IntentExamples` -Schlüssel mit dem **Typ** von `Array`:

    [![](implementing-sirikit-images/plist18.png "Fügen Sie den IntentExamples Schlüssel mit dem Typ von Array")](implementing-sirikit-images/plist18.png#lightbox)
22. Fügen Sie ein Paar `String` Schlüssel für die Verwendung von der Laufzeit:

    [![](implementing-sirikit-images/plist19.png "Fügen Sie einige Zeichenfolgenschlüssel mit für die Verwendung von der Laufzeit hinzu")](implementing-sirikit-images/plist19.png#lightbox)
23. Wiederholen Sie die oben genannten Schritte für alle Intent-Elemente der app Beispiel für die Verwendung von bereitstellen müssen.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Mit der rechten Maustaste in den Namen des Projekts die **Projektmappen-Explorer** , und wählen Sie **hinzufügen > Neues Element… > Apple > Eigenschaftenliste > "Info.plist"**:

    [![](implementing-sirikit-images/plist01.w157-sml.png "Fügen Sie eine neue Datei \"Info.plist\" hinzu.")](implementing-sirikit-images/plist01.w157.png#lightbox)

2. Doppelklicken Sie auf die `AppIntentVocabulary.plist` Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen.
3. Klicken Sie auf die **+** um einen Schlüssel hinzuzufügen, legen Sie die **Namen** zu `ParameterVocabularies` und **Typ** zu `Array`:

    [![](implementing-sirikit-images/plist02w.png "Legen Sie den Namen ParameterVocabularies und der Typ, Arrays")](implementing-sirikit-images/plist02w.png#lightbox)
4. Erweitern Sie `ParameterVocabularies` , und klicken Sie auf die **+** Schaltfläche, und legen Sie die **Typ** zu `Dictionary`:

    [![](implementing-sirikit-images/plist03w.png "Legen Sie den Typ zum Wörterbuch")](implementing-sirikit-images/plist03w.png#lightbox)
5. Klicken Sie auf die **+** um einen neuen Schlüssel hinzuzufügen, legen Sie die **Namen** zu `ParameterNames` und **Typ** zu `Array`:

    [![](implementing-sirikit-images/plist04w.png "Legen Sie den Namen ParameterNames und der Typ, Arrays")](implementing-sirikit-images/plist04w.png#lightbox)
6. Klicken Sie auf die **+** , fügen einen neuen Schlüssel mit dem **Typ** von `String` und den Wert als eine der verfügbaren Parameter. Z. B. `INStartWorkoutIntent.workoutName`:

    [![](implementing-sirikit-images/plist05w.png "Der Schlüssel INStartWorkoutIntent.workoutName")](implementing-sirikit-images/plist05w.png#lightbox)
7. Hinzufügen der `ParameterVocabulary` -Schlüssel in der `ParameterVocabularies` Schlüssel, der **Typ** von `Array`:

    [![](implementing-sirikit-images/plist06w.png "Fügen Sie den ParameterVocabulary-Schlüssel auf den ParameterVocabularies Schlüssel mit dem Typ von Array")](implementing-sirikit-images/plist06w.png#lightbox)
8. Fügen Sie einen neuen Schlüssel mit dem **Typ** von `Dictionary`:

    [![](implementing-sirikit-images/plist07w.png "Fügen Sie einen neuen Schlüssel mit dem Typ von-Wörterbuch hinzu.")](implementing-sirikit-images/plist07w.png#lightbox)
9. Hinzufügen der `VocabularyItemIdentifier` -Schlüssel mit dem **Typ** von `String` , und geben Sie eine eindeutige ID für den Begriff:

    [![](implementing-sirikit-images/plist08w.png "Fügen Sie den VocabularyItemIdentifier Schlüssel mit dem Typ der Zeichenfolge ein, und geben Sie eine eindeutige ID für den Begriff")](implementing-sirikit-images/plist08w.png#lightbox)
10. Hinzufügen der `VocabularyItemSynonyms` -Schlüssel mit dem **Typ** von `Array`:

    [![](implementing-sirikit-images/plist09w.png "Fügen Sie den VocabularyItemSynonyms Schlüssel mit dem Typ von Array")](implementing-sirikit-images/plist09w.png#lightbox)
11. Fügen Sie einen neuen Schlüssel mit dem **Typ** von `Dictionary`:

    [![](implementing-sirikit-images/plist10w.png "Fügen Sie einen neuen Schlüssel mit dem Typ von-Wörterbuch hinzu.")](implementing-sirikit-images/plist10w.png#lightbox)
12. Hinzufügen der `VocabularyItemPhrase` -Schlüssel mit dem **Typ** von `String` und den Begriff die app definieren:

    [![](implementing-sirikit-images/plist11w.png "Fügen Sie den VocabularyItemPhrase Schlüssel mit dem Typ der Zeichenfolge und der Begriff, die die app definieren")](implementing-sirikit-images/plist11w.png#lightbox)
13. Hinzufügen der `VocabularyItemPronunciation` Schlüssel, der **Typ** von `String` und die phonetische Aussprache der Dauer:

    [![](implementing-sirikit-images/plist12w.png "Fügen Sie den VocabularyItemPronunciation Schlüssel mit dem Typ der Zeichenfolge und die phonetische Aussprache des Begriffs")](implementing-sirikit-images/plist12w.png#lightbox)
14. Hinzufügen der `VocabularyItemExamples` -Schlüssel mit dem **Typ** von `Array`:

    [![](implementing-sirikit-images/plist13w.png "Fügen Sie den VocabularyItemExamples Schlüssel mit dem Typ von Array")](implementing-sirikit-images/plist13w.png#lightbox)
15. Fügen Sie ein Paar `String` Schlüssel für die Verwendung von der Laufzeit:

    [![](implementing-sirikit-images/plist14w.png "Fügen Sie einige Zeichenfolgenschlüssel mit für die Verwendung von der Laufzeit hinzu")](implementing-sirikit-images/plist14w.png#lightbox)
16. Wiederholen Sie die oben genannten Schritte für sonstige benutzerdefinierte Bedingungen definieren, müssen die app ein.
17. Reduzieren der `ParameterVocabularies` Schlüssel.
18. Hinzufügen der `IntentPhrases` -Schlüssel mit dem **Typ** von `Array`:

    [![](implementing-sirikit-images/plist15w.png "Fügen Sie den IntentPhrases Schlüssel mit dem Typ von Array")](implementing-sirikit-images/plist15w.png#lightbox)
19. Fügen Sie einen neuen Schlüssel mit dem **Typ** von `Dictionary`:

    [![](implementing-sirikit-images/plist16w.png "Fügen Sie einen neuen Schlüssel mit dem Typ von-Wörterbuch hinzu.")](implementing-sirikit-images/plist16w.png#lightbox)
20. Hinzufügen der `IntentName` Schlüssel, der **Typ** von `String` und beabsichtigte für das Beispiel:

    [![](implementing-sirikit-images/plist17w.png "Fügen Sie den IntentName Schlüssel mit dem Typ der Zeichenfolge und die beabsichtigte Lesevorgänge für das Beispiel")](implementing-sirikit-images/plist17w.png#lightbox)
21. Hinzufügen der `IntentExamples` -Schlüssel mit dem **Typ** von `Array`:

    [![](implementing-sirikit-images/plist18w.png "Fügen Sie den IntentExamples Schlüssel mit dem Typ von Array")](implementing-sirikit-images/plist18w.png#lightbox)
22. Fügen Sie ein Paar `String` Schlüssel für die Verwendung von der Laufzeit:

    [![](implementing-sirikit-images/plist19w.png "Fügen Sie einige Zeichenfolgenschlüssel mit für die Verwendung von der Laufzeit hinzu")](implementing-sirikit-images/plist19w.png#lightbox)
23. Wiederholen Sie die oben genannten Schritte für alle Intent-Elemente der app Beispiel für die Verwendung von bereitstellen müssen.

-----

> [!IMPORTANT]
> Die `AppIntentVocabulary.plist` registriert mit Siri auf dem Testcomputer Geräte während der Entwicklung, und es können etwas dauern für Siri den angepassten vokabularanpassung zu integrieren. Daher müssen die Tester warten Sie einige Minuten, bevor Sie versuchen, App-spezifische Programmierterminologie zu testen, wenn sie aktualisiert wurde.

Weitere Informationen finden Sie unsere [App bestimmten Vokabular Verweis](~/ios/platform/sirikit/understanding-sirikit.md) und Apple [angeben von benutzerdefinierten Vokabular Verweis](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SpecifyingCustomVocabulary.html#//apple_ref/doc/uid/TP40016875-CH6-SW1).

## <a name="adding-an-intents-extension"></a>Eine Intents-Erweiterung hinzufügen

Nun, da die app SiriKit übernehmen vorbereitet wurde, muss der Entwickler die Lösung für die Integration von Siri benötigten Intents verarbeiten (mindestens) Intents-Erweiterungen hinzu.

Führen Sie für jede Intents-Erweiterung, die erforderlich sind folgende Schritte aus:

- Fügen Sie ein Projekt für die Intents-Erweiterung auf die Xamarin.iOS-app-Projektmappe hinzu.
- Konfigurieren Sie die Intents-Erweiterung `Info.plist` Datei.
- Ändern Sie die Hauptklasse für die Intents-Erweiterung.

Weitere Informationen finden Sie unsere [Erweiterungsverweis für die Intents](~/ios/platform/sirikit/understanding-sirikit.md) und Apple [Erstellen des Verweises Intents-Erweiterung](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CreatingtheIntentsExtension.html#//apple_ref/doc/uid/TP40016875-CH4-SW1).

### <a name="creating-the-extension"></a>Erstellen die Erweiterung

Um eine Intents-Erweiterung der Projektmappe hinzuzufügen, führen Sie folgende Schritte aus:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Mit der rechten Maustaste auf die **Projektmappenname** in die **Lösungspad** , und wählen Sie **hinzufügen** > **neues Projekt hinzufügen...** .
2. Wählen Sie im Dialogfeld **iOS** > **Erweiterungen** > **Absicht Erweiterung** , und klicken Sie auf die **Weiter** Schaltfläche: 

    [![](implementing-sirikit-images/intents05.png "Wählen Sie Intent-Erweiterung")](implementing-sirikit-images/intents05.png#lightbox)
3. Geben Sie als Nächstes eine **Namen** Intent-Erweiterung, und Sie auf die **Weiter** Schaltfläche: 

    [![](implementing-sirikit-images/intents06.png "Geben Sie einen Namen für die Anwendungsabsicht-Erweiterung")](implementing-sirikit-images/intents06.png#lightbox)
4. Klicken Sie abschließend auf die **erstellen** , um die Absicht-Erweiterung mit der Lösung für die apps hinzuzufügen: 

    [![](implementing-sirikit-images/intents07.png "Hinzufügen der Intent-Erweiterung für die apps-Lösung")](implementing-sirikit-images/intents07.png#lightbox)
5. In der **Projektmappen-Explorer**, mit der rechten Maustaste auf die **Verweise** Ordner, der neu erstellten Intent-Erweiterung. Überprüfen Sie den Namen des gemeinsamen freigegebenen Code Library-Projekts (die über die app erstellt haben), und klicken Sie auf die **OK** Schaltfläche: 

    [![](implementing-sirikit-images/intents08.png "Wählen Sie den Namen des gemeinsamen freigegebenen Code Library-Projekts")](implementing-sirikit-images/intents08.png#lightbox)
    
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Mit der rechten Maustaste auf die **Projektmappenname** in die **Projektmappen-Explorer** , und wählen Sie **hinzufügen** > **neues Projekt hinzufügen...** .
2. Wählen Sie im Dialogfeld **Visual C# > iOS-Erweiterungen > Absicht Erweiterung** , und klicken Sie auf die **Weiter** Schaltfläche:

    [![](implementing-sirikit-images/intents05.w157-sml.png "Wählen Sie Intent-Erweiterung")](implementing-sirikit-images/intents05.w157.png#lightbox)
3. Geben Sie als Nächstes eine **Namen** Intent-Erweiterung, und Sie auf die **OK** Schaltfläche.
1. In der **Projektmappen-Explorer**, mit der rechten Maustaste auf die **Verweise** die Intents-Erweiterung für den neu erstellten Ordner, und wählen Sie **hinzufügen > Verweis**. Überprüfen Sie den Namen des gemeinsamen freigegebenen Code Library-Projekts (die über die app erstellt haben), und klicken Sie auf die **OK** Schaltfläche:

    [![](implementing-sirikit-images/intents08w.png "Wählen Sie den Namen des gemeinsamen freigegebenen Code Library-Projekts")](implementing-sirikit-images/intents08w.png#lightbox)
    
-----

Wiederholen Sie diese Schritte für die Anzahl der Intent-Erweiterungen (basierend auf [Architektur der App für Erweiterungen](#Architecting-the-App-for-Extensions) weiter oben), die die app benötigt.

### <a name="configuring-the-infoplist"></a>Konfigurieren die Datei "Info.plist"

Für jeden die Intents-Erweiterungen, die der Projektmappe der app hinzugefügt haben, müssen in konfiguriert werden die `Info.plist` Dateien mit der app verwendet.

Genau wie alle typischen App-Erweiterungen, die app wird von der vorhandenen Schlüssel haben `NSExtension` und `NSExtensionAttributes`. Es gibt zwei neue Attribute, die konfiguriert werden müssen, für eine Intents-Erweiterung:

[![](implementing-sirikit-images/intents01.png "Die beiden neuen Attribute, die konfiguriert werden müssen")](implementing-sirikit-images/intents01.png#lightbox)

- **IntentsSupported** – ist erforderlich und besteht aus einem Array von Klassennamen Absicht, die die app über die Erweiterung für beabsichtigte Lesevorgänge unterstützen soll.
- **IntentsRestrictedWhileLocked** -ist ein optionaler Schlüssel für die app an der Erweiterung Bildschirm Sperrverhalten. Es besteht aus einem Array von Klassennamen Absicht, die die app möchte der Benutzer angemeldet sein, von der Absicht-Erweiterung verwenden.

Zum Konfigurieren der Intent-Erweiterung `Info.plist` doppelt, in der **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen. Als Nächstes wechseln Sie zu der **Quelle** anzeigen und erweitern Sie dann die `NSExtension` und `NSExtensionAttributes` Schlüssel in den Editor:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![](implementing-sirikit-images/intents02.png "Die NSExtension und NSExtensionAttributes Schlüssel im editor")](implementing-sirikit-images/intents02.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](implementing-sirikit-images/intents02w.png "Die NSExtension und NSExtensionAttributes Schlüssel im editor")](implementing-sirikit-images/intents02w.png#lightbox)

-----

Erweitern Sie die `IntentsSupported` Taste, und fügen Sie den Namen einer beliebigen Intent-Klasse, die diese Erweiterung unterstützt. Für die Beispiel-app MonkeyChat unterstützt die `INSendMessageIntent`:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![](implementing-sirikit-images/intents09.png "Der Schlüssel INSendMessageIntent")](implementing-sirikit-images/intents09.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](implementing-sirikit-images/intents09w.png "Der Schlüssel INSendMessageIntent")](implementing-sirikit-images/intents09w.png#lightbox)

-----

Wenn die app optional erforderlich ist, dass der Benutzer angemeldet sein, auf dem Gerät um einen bestimmten Zweck verwenden, erweitern die `IntentRestrictedWhileLocked` Taste, und fügen Sie die Namen der Klasse von der Intent-Elemente, die über eingeschränkten Zugriff verfügen. Für die Beispiel-app MonkeyChat der Benutzer muss angemeldet sein, um eine Chatnachricht zu senden, damit wir hinzugefügt haben `INSendMessageIntent`:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![](implementing-sirikit-images/intents10.png "Die hinzugefügten INSendMessageIntent-Schlüssel")](implementing-sirikit-images/intents10.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](implementing-sirikit-images/intents10w.png "Die hinzugefügten INSendMessageIntent-Schlüssel")](implementing-sirikit-images/intents10w.png#lightbox)

-----


Eine vollständige Liste der verfügbaren Domänen der Absicht, finden Sie in Apple [Absicht Domänen Verweis](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2).

### <a name="configuring-the-main-class"></a>Konfigurieren von "Main"-Klasse

Als Nächstes müssen die Entwickler so konfigurieren Sie die Hauptklasse, die als der Haupteinstiegspunkt für die Ziel-Erweiterung in Siri fungiert. Es muss eine Unterklasse von `INExtension` die entspricht der `IINIntentHandler` delegieren. Zum Beispiel:

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

Es gibt eine einzelne Methode, die die app, auf die Hauptklasse Intent-Erweiterung implementieren muss, die `GetHandler` Methode. Diese Methode wird ein Intent-Objekt durch SiriKit übergeben und muss die app zurückgeben. ein **Intent-Handler** , entspricht den Typ des angegebenen Ziel.

Da die Beispiel-app MonkeyChat nur eine beabsichtigte behandelt, gibt es selbst in die `GetHandler` Methode. Wenn die Erweiterung mehr als eine Absicht behandelt, der Entwickler fügen Sie eine Klasse für jeden beabsichtigten und hier basierte auf eine Instanz zurückgeben würde die `Intent` an die Methode übergeben.

### <a name="handling-the-resolve-stage"></a>Behandeln der Resolve-Phase

Die Phase zu beheben, in dem die Absicht-Erweiterung zu verdeutlichen und Überprüfen der Parameter wird übergeben von Siri und über des Benutzers Konversation festgelegt wurden.

Für jeden Parameter, die von Siri gesendet wird, besteht eine `Resolve` Methode. Die app muss diese Methode für jeden Parameter implementiert, dass die app Siris-Hilfe, um die richtige Antwort vom Benutzer erhalten möglicherweise.

Im Falle der Beispiel-app MonkeyChat benötigen die Absicht-Erweiterung eine einen oder mehrere Empfänger, die Nachricht zu senden. Die Erweiterung müssen für jeden Empfänger in der Liste wenden Sie sich an Suche durchgeführt werden soll, die das folgende Ergebnis haben kann:

- Es wurde genau ein übereinstimmender Kontakt gefunden.
- Mindestens zwei übereinstimmende Kontakte gefunden werden.
- Es werden keine übereinstimmenden Kontakte gefunden.

Darüber hinaus erfordert MonkeyChat Inhalt für den Text der Nachricht. Wenn der Benutzer dies nicht angegeben hat, muss Siri mit benutzeraufforderung für den Inhalt an.

Die Absicht-Erweiterung müssen jedem dieser Fälle zurechtzukommen.


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

Weitere Informationen finden Sie unter unserem [der beheben Phase Verweis](~/ios/platform/sirikit/understanding-sirikit.md) und Apple [Resolving und Verarbeiten von Intents-Verweis](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/ResolvingandHandlingIntents.html#//apple_ref/doc/uid/TP40016875-CH5-SW1).

### <a name="handling-the-confirm-stage"></a>Behandeln von der Phase bestätigen

Die Phase bestätigen ist, in dem der Intent-Erweiterung überprüft, dass sie alle Informationen, um die Anforderung des Benutzers zu erfüllen hat. Die app will senden, dass die Bestätigung zusammen werden die unterstützenden Details der Funktion wird in Kürze geschieht siri, damit sie mit der Benutzer bestätigt werden kann, das von diesem ist, dass die beabsichtigte Aktion.

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

Weitere Informationen finden Sie unserem [der bestätigen Phase Verweis](~/ios/platform/sirikit/understanding-sirikit.md).

### <a name="processing-the-intent"></a>Die Absicht verarbeiten

Dies ist der Punkt, der die Absicht-Erweiterung führt, in dem tatsächlich die Aufgabe zum Erfüllen der Anforderung des Benutzers und die Ergebnisse zurück an Siri übergeben, damit der Benutzer informiert werden kann.


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

Weitere Informationen finden Sie unserem [der behandeln Phase Verweis](~/ios/platform/sirikit/understanding-sirikit.md).

## <a name="adding-an-intents-ui-extension"></a>Hinzufügen eine Intents-Benutzeroberflächenerweiterung

Der optionale Intents-Benutzeroberflächenerweiterung bietet die Möglichkeit, schalten Sie der app Benutzeroberfläche und in die Oberfläche Siri branding und stellen die Verbindung mit der app können Benutzer. Mit dieser Erweiterung kann die app sowohl die Marke als auch visual und andere Informationen in die Aufzeichnung erhöhen.

[![](implementing-sirikit-images/intentsui01.png "Eine Beispielausgabe für die Intents-Benutzeroberflächenerweiterung")](implementing-sirikit-images/intentsui01.png#lightbox)

Genau wie die Intents-Erweiterung wird der Entwickler die folgende Schritte für die Intents-Benutzeroberflächenerweiterung führen:

- Die Xamarin.iOS-app-Projektmappe wird eine Intents-Benutzeroberflächenerweiterung-Projekt hinzugefügt.
- Konfigurieren Sie die Intents-Benutzeroberflächenerweiterung `Info.plist` Datei.
- Ändern Sie die Hauptklasse für die Intents-Benutzeroberflächenerweiterung.

Weitere Informationen finden Sie unsere [die Intents-Erweiterung Benutzeroberflächenreferenz](~/ios/platform/sirikit/understanding-sirikit.md) und Apple [benutzerdefinierte Schnittstelle verweisen](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/ProvidingaCustomInterface.html#//apple_ref/doc/uid/TP40016875-CH7-SW1).

### <a name="creating-the-extension"></a>Erstellen die Erweiterung

Um eine Intents-Benutzeroberflächenerweiterung zur Projektmappe hinzuzufügen, führen Sie folgende Schritte aus:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Mit der rechten Maustaste auf die **Projektmappenname** in die **Lösungspad** , und wählen Sie **hinzufügen** > **neues Projekt hinzufügen...** .
2. Wählen Sie im Dialogfeld **iOS** > **Erweiterungen** > **Intent-Benutzeroberflächenerweiterung** , und klicken Sie auf die **Weiter** Schaltfläche: 

    [![](implementing-sirikit-images/intents11.png "Wählen Sie die beabsichtigte Benutzeroberflächenerweiterung")](implementing-sirikit-images/intents11.png#lightbox)
3. Geben Sie als Nächstes eine **Namen** Intent-Erweiterung, und Sie auf die **Weiter** Schaltfläche: 

    [![](implementing-sirikit-images/intents12.png "Geben Sie einen Namen für die Anwendungsabsicht-Erweiterung")](implementing-sirikit-images/intents12.png#lightbox)
4. Klicken Sie abschließend auf die **erstellen** , um die Absicht-Erweiterung mit der Lösung für die apps hinzuzufügen: 

    [![](implementing-sirikit-images/intents13.png "Hinzufügen der Intent-Erweiterung für die apps-Lösung")](implementing-sirikit-images/intents13.png#lightbox)
5. In der **Projektmappen-Explorer**, mit der rechten Maustaste auf die **Verweise** Ordner, der neu erstellten Intent-Erweiterung. Überprüfen Sie den Namen des gemeinsamen freigegebenen Code Library-Projekts (die über die app erstellt haben), und klicken Sie auf die **OK** Schaltfläche: 

    [![](implementing-sirikit-images/intents14.png "Wählen Sie den Namen des gemeinsamen freigegebenen Code Library-Projekts")](implementing-sirikit-images/intents14.png#lightbox)
    
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Mit der rechten Maustaste auf die **Projektmappenname** in die **Projektmappen-Explorer** , und wählen Sie **hinzufügen** > **neues Projekt hinzufügen...**
2. Wählen Sie im Dialogfeld **iOS** > **Erweiterungen** > **Intent-Benutzeroberflächenerweiterung** , und klicken Sie auf die **Weiter** Schaltfläche ".
3. Geben Sie als Nächstes eine **Namen** Intent-Erweiterung, und Sie auf die **OK** Schaltfläche.
5. In der **Projektmappen-Explorer**, mit der rechten Maustaste auf die **Verweise** Ordner, der neu erstellten Intent-Erweiterung. Überprüfen Sie den Namen des gemeinsamen freigegebenen Code Library-Projekts (die über die app erstellt haben), und klicken Sie auf die **OK** Schaltfläche.
    
-----

### <a name="configuring-the-infoplist"></a>Konfigurieren die Datei "Info.plist"

Konfigurieren Sie die Intents-Benutzeroberflächenerweiterung `Info.plist` Datei, die mit der app verwendet.

Genau wie alle typischen App-Erweiterungen, die app wird von der vorhandenen Schlüssel haben `NSExtension` und `NSExtensionAttributes`. Für eine Intents-Erweiterung gibt es ist ein neues Attribut, das konfiguriert werden müssen:

[![](implementing-sirikit-images/intents03.png "Die ein neues Attribut aus, das konfiguriert werden müssen")](implementing-sirikit-images/intents03.png#lightbox)

**IntentsSupported** ist erforderlich und besteht aus einem Array von Klassennamen Absicht, die die app aus der Intent-Erweiterung unterstützt werden soll.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

So konfigurieren Sie die Absicht-Benutzeroberflächenerweiterung `Info.plist` doppelt, in der **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen. Als Nächstes wechseln Sie zu der **Quelle** anzeigen und erweitern Sie dann die `NSExtension` und `NSExtensionAttributes` Schlüssel in den Editor:

[![](implementing-sirikit-images/intents04.png "Die NSExtension und NSExtensionAttributes Schlüssel im editor")](implementing-sirikit-images/intents04.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

So konfigurieren Sie die Absicht-Benutzeroberflächenerweiterung `Info.plist` doppelt, in der **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen. Erweitern Sie die `NSExtension` und `NSExtensionAttributes` Schlüssel in den Editor:

[![](implementing-sirikit-images/intents04w.png "Tthe NSExtension und NSExtensionAttributes Schlüssel im editor")](implementing-sirikit-images/intents04w.png#lightbox)

-----

Erweitern Sie die `IntentsSupported` Taste, und fügen Sie den Namen einer beliebigen Intent-Klasse, die diese Erweiterung unterstützt. Für die Beispiel-app MonkeyChat unterstützt die `INSendMessageIntent`:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![](implementing-sirikit-images/intents15.png "Der Schlüssel INSendMessageIntent")](implementing-sirikit-images/intents15.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](implementing-sirikit-images/intents15w.png "Der Schlüssel INSendMessageIntent")](implementing-sirikit-images/intents15w.png#lightbox)

-----

Eine vollständige Liste der verfügbaren Domänen der Absicht, finden Sie in Apple [Absicht Domänen Verweis](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2).

### <a name="configuring-the-main-class"></a>Konfigurieren von "Main"-Klasse

Konfigurieren Sie die Hauptklasse, die als der Haupteinstiegspunkt für die beabsichtigte Benutzeroberfläche-Erweiterung in Siri fungiert. Es muss eine Unterklasse von `UIViewController` die entspricht der `IINUIHostedViewController` Schnittstelle. Zum Beispiel:

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

Siri übergibt eine `INInteraction` Klasseninstanz der `Configure` -Methode der der `UIViewController` Instanz in der Absicht Benutzeroberflächenerweiterung.

Die `INInteraction` Objekt bietet drei wichtige Bereichen der Informationen, die die Erweiterung:

1. Das Intent-Objekt verarbeitet wird.
2. Das Intent-Response-Objekt, aus der `Confirm` und `Handle` Methoden der Intent-Erweiterung.
3. Der Zustand der Interaktion, der den Zustand der Interaktion zwischen der app und Siri definiert.

Die `UIViewController` Instanz ist das Prinzip-Klasse für die Interaktion mit Siri und weil es erbt `UIViewController`, hat Sie Zugriff auf alle Funktionen von UIKit.

Siri bei Aufruf der `Configure` Methode der `UIViewController` übergibt einen Kontext anzeigen, die besagt, dass der Ansichtscontroller entweder in einer Siri Snippit oder Maps-Karte gehostet wird.

Siri auch übergibt einen Abschlusshandler die app, die die gewünschte Größe der Ansicht zurückgegeben, sobald die app haben, konfigurieren müssen.

### <a name="design-the-ui-in-ios-designer"></a>Entwerfen der Benutzeroberflächenautomatisierungs in iOS-Designer

Layout der Intents-Benutzeroberflächenerweiterung die Benutzeroberfläche im iOS-Designer. Doppelklicken Sie auf der Erweiterungs `MainInterface.storyboard` Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen. Ziehen Sie alle erforderlichen Elemente der Benutzeroberfläche der Benutzeroberfläche erstellen und speichern Sie die Änderungen.

> [!IMPORTANT]
> Es ist zwar möglich, interaktive Elemente hinzufügen, wie z. B. `UIButtons` oder `UITextFields` auf der Absicht-Benutzeroberflächenerweiterung `UIViewController`, diesen sind streng verboten, als die beabsichtigte Benutzeroberfläche im nicht interaktiven und der Benutzer ist nicht in der Lage, Sie mit ihnen interagieren.

### <a name="wire-up-the-user-interface"></a>Über das Netzwerk von der Benutzeroberfläche

Bearbeiten Sie die Intents-Benutzeroberflächenerweiterung-Benutzeroberfläche in iOS-Designer erstellt, die `UIViewController` -Unterklasse und überschreiben die `Configure` -Methode wie folgt:

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

### <a name="overriding-the-default-siri-ui"></a>Überschreiben der standardmäßigen Siri-Benutzeroberfläche

Die Intents-Benutzeroberflächenerweiterung immer zusammen mit anderem Inhalt Siri, z. B. die app-Symbol und Name am oberen Rand der Benutzeroberfläche angezeigt oder wird, basierend auf der Absicht, Schaltflächen (z. B. Sende- oder "Abbrechen") am unteren Rand angezeigt werden können.

Es gibt einige Fälle, in dem die app ersetzen können, die Informationen, die Siri standardmäßig, z. B. messaging, die dem Benutzer angezeigt wird oder die Zuordnungen, in dem die app mit einem maßgeschneidert an die app die standarderfahrung ersetzen können.

Wenn die Intents-Benutzeroberflächenerweiterung Elemente des Siri UI, überschreiben die `UIViewController` Unterklasse müssen implementieren die `IINUIHostedViewSiriProviding` Schnittstelle und sich für eine bestimmte Schnittstelle-Element angezeigt.

Fügen Sie den folgenden Code der `UIViewController` Unterklasse Siri mitteilen, dass die beabsichtigte Benutzeroberfläche-Erweiterung bereits Inhalt der Nachricht angezeigt wird:

```csharp
public bool DisplaysMessage {
    get {return true;}
}
```

### <a name="considerations"></a>Weitere Überlegungen

Apple empfiehlt, dass der Entwickler die folgenden Aspekte berücksichtigt beim Entwerfen und implementieren die Ziel-UI-Erweiterungen annehmen:

- **Bewusste der Speicherauslastung werden** – da Intent-UI-Erweiterungen sind temporär und nur für kurze Zeit angezeigt wird, erzwingt das System eine engere arbeitsspeichereinschränkungen als mit einer vollständigen app verwendet werden.
- **Erwägen Sie, minimale und maximale Größe der Ansicht** -stellen Sie sicher, dass die Ziel-UI-Erweiterungen auf jedem iOS-Gerät-Typ, Größe und Ausrichtung in Ordnung ist. Darüber hinaus möglicherweise die gewünschte Größe, die die app an Siri zurücksendet nicht erteilt werden können.
- **Verwenden Sie Flexible und Adaptive Layout Muster** – in diesem Fall um sicherzustellen, dass die Benutzeroberfläche auf jedem Gerät großartig aussieht.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel verfügt über SiriKit beschrieben und gezeigt, wie sie hinzugefügt werden kann, an die Xamarin.iOS-apps, um Dienste bereitzustellen, die für den Benutzer mithilfe von Siri und die Karten-app auf einem iOS-Gerät zugänglich sind.




## <a name="related-links"></a>Verwandte Links

- [ElizaChat-Beispiel](https://developer.xamarin.com/samples/monotouch/ios10/ElizaChat/)
- [SiriKit-Programmierhandbuch](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [Frameworkverweis Intents](https://developer.apple.com/reference/intents)
- [Referenz für Intents UI-Framework](https://developer.apple.com/reference/intentsui)
- [Beabsichtigte Domänen-Referenz](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2)
