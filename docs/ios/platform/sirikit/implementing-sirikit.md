---
title: Implementieren von SiriKit in Xamarin.iOS
description: Dieses Dokument beschreibt die erforderlichen Schritte zum SiriKit-Unterstützung in einem Xamarin.iOS-apps zu implementieren. Es wird erläutert, Intents Erweiterungen und Intents UI-Erweiterungen.
ms.prod: xamarin
ms.assetid: 20FFB981-EB10-48BA-BF79-40F37F0291EB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/03/2018
ms.openlocfilehash: f0e5e05828305bd3656d70105b6e2ad06f9fdc81
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788848"
---
# <a name="implementing-sirikit-in-xamarinios"></a>Implementieren von SiriKit in Xamarin.iOS

_Dieser Artikel behandelt die erforderlichen Schritte zum SiriKit-Unterstützung in einem Xamarin.iOS-apps zu implementieren._

Neue iOS 10, SiriKit ermöglicht eine Xamarin.iOS-app, um Dienste bereitzustellen, die für den Benutzer mithilfe von Siri und die Maps-app auf einem iOS-Gerät zugänglich sind. Dieser Artikel behandelt die erforderlichen Schritte zum SiriKit-Unterstützung in den Xamarin.iOS-apps zu implementieren, indem Sie die erforderlichen Intents Erweiterungen, Intents UI Extensions und das Vokabular.

Siri funktioniert mit dem Konzept der **Domänen**, Gruppen von wissen Aktionen für verwandte Aufgaben. Jeder Interaktion mit die app mit Siri muss wie folgt in einen bekannten Dienst Domäne fallen:

- Audio oder video aufrufen.
- Buchung eine fuhr an.
- Verwalten von Training.
- Messaging.
- Suchen Fotos.
- Senden oder Empfangen von Zahlungen.

Wenn der Benutzer eine Anforderung Siri im Zusammenhang mit einer der Dienste für die App-Erweiterung stellt, sendet SiriKit die Erweiterung ein **Absicht** -Objekt, das der Benutzer-Anforderung sowie alle unterstützungsdaten beschreiben. Die App-Erweiterung generiert dann das entsprechende **Antwort** -Objekt für den angegebenen **Absicht**, mit Details wie die Erweiterung für die Anforderung verarbeiten kann.

Dieses Handbuch stellt ein kurzes Beispiel, etwa SiriKit-Unterstützung in eine vorhandene app bereit. Für dieses Beispiel müssen die gefälschte MonkeyChat-app verwendet werden:

[![](implementing-sirikit-images/monkeychat01.png "Das Symbol \"MonkeyChat\"")](implementing-sirikit-images/monkeychat01.png#lightbox)

MonkeyChat behält eine eigene Adressbuch des Benutzers Freunde, jeweils eines Anzeigenamens (z. B. Bobo z. B.) zugeordnet ist, und ermöglicht es dem Benutzer nach dem Bildschirmnamen jeder Freund Chats Text an.

## <a name="extending-the-app-with-sirikit"></a>Erweitern Sie die App mit SiriKit

Entsprechend der [Grundlegendes zu SiriKit Konzepten](~/ios/platform/sirikit/understanding-sirikit.md) Guide drei Hauptteilen zusammen beteiligt, erweitern eine app mit SiriKit sind:

[![](implementing-sirikit-images/elements01.png "Erweitern Sie die App mit SiriKit-Diagramm")](implementing-sirikit-images/elements01.png#lightbox)

Dazu gehören:

1. **Intents Erweiterung** -überprüft die Benutzer-Antworten, die app kann die Anforderung verarbeiten und tatsächlich führt die Aufgabe für die Erfüllung der Anforderung des Benutzers bestätigt.
2. **Intents Benutzeroberflächenerweiterung** - *Optional*, eine angepasste Benutzerschnittstelle, um die Antworten in der Umgebung Siri bereitgestellt und können den UI-apps übernehmen und branding in Siri auf dem Benutzer weiter zu verbessern.
3. **App** -Benutzer bestimmte Vokabulare Siri mit der Arbeit bei die App bietet. 

Alle diese Elemente und die Schritte zum Einschließen in die app wird in den folgenden Abschnitten ausführlich beschrieben.


## <a name="preparing-the-app"></a>Vorbereiten der App

SiriKit basiert auf Erweiterungen, die vor dem Hinzufügen von Erweiterungen für die app aus, es gibt jedoch einige Dinge, die der Entwickler bei der Übernahme von SiriKit unterstützen muss.

### <a name="moving-common-shared-code"></a>Verschieben von gemeinsam verwendeten Code 

Der Entwickler kann verschieben Sie zunächst einige der gemeinsamen Code, der freigegeben wird zwischen der app und die Erweiterungen in einen _gemeinsam genutzte Projekte_, _portablen Klassenbibliotheken (PCLs)_ oder _Native Bibliotheken_.

Die Erweiterungen müssen in der Lage, alle Dinge, die die app ausführt. Senden von Nachrichten und meldungsverlaufs abrufen, im Hinblick auf die beispielanwendung des MonkeyChat, z. B. Suchen nach Kontakten, neue Kontakte hinzufügen.

Umgezogen dieser gemeinsamen Code in ein freigegebenes Projekt, PCL oder systemeigene Bibliothek ganz einfach, Code allgemeine zentral verwalten, und es wird sichergestellt, dass die Erweiterung und die übergeordnete app uniform Erfahrungen und Funktionen für den Benutzer bereitzustellen.

Im Fall der Beispiel-app MonkeyChat werden die Datenmodelle und Verarbeitungscode z. B. Netzwerk-und Datenbank in eine systemeigene Bibliothek verschoben werden.

Führen Sie folgende Schritte aus:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Starten Sie Visual Studio für Mac, und öffnen Sie die app MonkeyChat.
2. Mit der rechten Maustaste auf den Namen der Projektmappe in der **Lösung Pad** , und wählen Sie **hinzufügen** > **neues Projekt...** : 

    [![](implementing-sirikit-images/prep01.png "Ein neues Projekt hinzufügen")](implementing-sirikit-images/prep01.png#lightbox)
3. Wählen Sie **iOS** > **Bibliothek** > **-Klassenbibliothek** , und klicken Sie auf die **Weiter** Schaltfläche: 

    [![](implementing-sirikit-images/prep02.png "Wählen Sie die Klassenbibliothek")](implementing-sirikit-images/prep02.png#lightbox)
4. Geben Sie `MonkeyChatCommon` für die **Namen** , und klicken Sie auf die **erstellen** Schaltfläche: 

    [![](implementing-sirikit-images/prep03.png "Geben Sie ein MonkeyChatCommon")](implementing-sirikit-images/prep03.png#lightbox)
5. Mit der rechten Maustaste auf die **Verweise** Ordner der Haupt-app in der **Projektmappen-Explorer** , und wählen Sie **Verweise bearbeiten...** . Überprüfen Sie die **MonkeyChatCommon** Projekt, und klicken Sie auf die **OK** Schaltfläche: 

    [![](implementing-sirikit-images/prep05.png "Überprüfen Sie das Projekt MonkeyChatCommon")](implementing-sirikit-images/prep05.png#lightbox)
6. In der **Projektmappen-Explorer**, ziehen Sie den gemeinsamen verwendeten Code aus dem Haupt-app in die systemeigene Bibliothek.
7. Im Fall von MonkeyChat, ziehen Sie die **DataModels** und **Prozessoren** Ordner aus dem Haupt-app in die systemeigene Bibliothek: 

    [![](implementing-sirikit-images/prep06.png "Die DataModels und Prozessoren Ordner im Projektmappen-Explorer")](implementing-sirikit-images/prep06.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Starten Sie Visual Studio, und öffnen Sie die app MonkeyChat.
2. Mit der rechten Maustaste auf den Namen der Projektmappe in der **Projektmappen-Explorer** , und wählen Sie **hinzufügen** > **neues Projekt...** .
3. Wählen Sie **Visual C#-** > **freigegebenes Projekt** , und klicken Sie auf die **Weiter** Schaltfläche: 

    [![](implementing-sirikit-images/prep02.w157-sml.png "Wählen Sie die Klassenbibliothek")](implementing-sirikit-images/prep02.w157.png#lightbox)
4. Geben Sie `MonkeyChatCommon` für die **Namen** , und klicken Sie auf die **erstellen** Schaltfläche.
5. Mit der rechten Maustaste auf die **Verweise** Ordner der Haupt-app in der **Projektmappen-Explorer** , und wählen Sie **Verweise bearbeiten...** . Überprüfen Sie die **MonkeyChatCommon** Projekt, und klicken Sie auf die **OK** Schaltfläche: 

    [![](implementing-sirikit-images/prep05w.png "Überprüfen Sie das Projekt MonkeyChatCommon")](implementing-sirikit-images/prep05w.png#lightbox)
6. In der **Projektmappen-Explorer**, ziehen Sie den gemeinsamen verwendeten Code aus dem Haupt-app auf dem gemeinsamen Projekt.
7. Im Fall von MonkeyChat, ziehen Sie die **DataModels** und **Prozessoren** Ordner aus dem Haupt-app in die systemeigene Bibliothek.

-----

Bearbeiten Sie die Dateien, die in die systemeigene Bibliothek verschoben wurden, und ändern Sie den Namespace der Bibliothek an. Ändern Sie z. B. `MonkeyChat` auf `MonkeyChatCommon`:

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

Als Nächstes wechseln Sie zurück zur Haupt-app und zum Hinzufügen einer `using` -Anweisung für die systemeigene Bibliothek-Namespace eine beliebige Stelle die app verwendet eine der Klassen, die verschoben wurden:

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

### <a name="architecting-the-app-for-extensions"></a>Architektur der App für Erweiterungen

In der Regel wird eine app für mehrere Intents registrieren, und der Entwickler muss sicherstellen, dass die app für die entsprechende Anzahl von Absicht Erweiterungen ausgelegt ist.

In der Situation, in denen eine Anwendung mehrere Absicht erfordert, hat der Entwickler die Möglichkeit, alle seine beabsichtigte Behandlung oder in eine beabsichtigte-Erweiterung oder erstellen eine separate Absicht-Erweiterung für jeden Zweck.

Wenn Sie sich für eine separate Absicht-Erweiterung für jeden Zweck zu erstellen, konnte der Entwickler annehmen, eine große Menge an Standardcode in jede Erweiterung duplizieren und erstellen eine große Menge an Prozessor- und speichermehraufwand.

Hilfe bei zwischen den beiden Optionen angezeigt, wenn keines der Intents natürlich zusammen gehören. Beispielsweise eine app, die Audio- und Videofunktionen Aufrufe beider diese Intents in einer Absicht Erweiterung einschließen, wie sie ähnliche Vorgänge behandeln und kann die Wiederverwendung von den meisten Code bereitstellen.

Erstellen Sie eine neue Erweiterung für die Absicht für alle Absicht oder eine Gruppe von Intents, die nicht in einer vorhandenen Gruppe entsprechen, in der app-Projektmappe, in diese enthalten.


### <a name="setting-the-required-entitlements"></a>Die erforderlichen Berechtigungen festlegen

Jeder Xamarin.iOS-app, die SiriKit Integration umfasst, müssen die richtigen Berechtigungen festgelegt haben. Wenn der Entwickler diese erforderlichen Berechtigungen ordnungsgemäß festgelegt ist, sie werden nicht in der Lage zu installieren oder Testen der app auf echten iOS 10 (oder höher) Hardware, die Anforderung seit iOS 10 auch ist Simulator SiriKit unterstützt.

Führen Sie folgende Schritte aus:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Doppelklicken Sie auf die `Entitlements.plist` in der Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen.
2. Wechseln Sie zu der **Quelle** Registerkarte.
3. Hinzufügen der `com.apple.developer.siri` **Eigenschaft**, legen die **Typ** auf `Boolean` und **Wert** auf `Yes`: 

    [![](implementing-sirikit-images/setup01.png "Fügen Sie die com.apple.developer.siri-Eigenschaft")](implementing-sirikit-images/setup01.png#lightbox)
4. Speichern Sie die Änderungen in der Datei.
5. Doppelklicken Sie auf die **Projektdatei** in der **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen.
6. Wählen Sie **iOS Bundle Signing** und sicherstellen, dass die `Entitlements.plist` Datei ausgewählt ist, der **benutzerdefinierte Ansprüche** Feld: 

    [![](implementing-sirikit-images/setup02.png "Wählen Sie die Entitlements.plist-Datei im Feld benutzerdefinierte Berechtigungen")](implementing-sirikit-images/setup02.png#lightbox)
7. Klicken Sie auf die Schaltfläche **OK**, um die Änderungen zu speichern.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Doppelklicken Sie auf die `Entitlements.plist` in der Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen.
3. Hinzufügen der `com.apple.developer.siri` **Eigenschaft**, legen die **Typ** auf `Boolean` und **Wert** auf `Yes`: 

    [![](implementing-sirikit-images/setup01w.png "Fügen Sie die com.apple.developer.siri-Eigenschaft")](implementing-sirikit-images/setup01w.png#lightbox)
4. Speichern Sie die Änderungen in der Datei.
5. Doppelklicken Sie auf die **Projektdatei** in der **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen.
6. Wählen Sie **iOS Bundle Signing** und sicherstellen, dass die `Entitlements.plist` Datei ausgewählt ist, der **benutzerdefinierte Ansprüche** Feld.

-----

Wenn Sie fertig sind, handelt es sich bei der app `Entitlements.plist` Datei sollte wie folgt aussehen (in geöffnet in einen externen Editor):

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

### <a name="correctly-provisioning-the-app"></a>Ordnungsgemäß Bereitstellung der App

Weil die strikte Sicherheit, die von Apple um SiriKit-Framework, eine beliebige app Xamarin.iOS platziert werden, die SiriKit implementiert _müssen_ haben das richtige App-ID und der Ansprüche (Siehe obigen Abschnitt "") und muss mit einem richtigen signiert werden Provisioning-Profil.

Führen Sie auf Ihrem Mac Folgendes ein:

1. Wechseln Sie in einem Webbrowser zu [ http://developer.apple.com ](http://developer.apple.com) und melden Sie sich bei Ihrem Konto.
2. Klicken Sie auf **Zertifikate**, **Bezeichner** und **Profile**.
3. Wählen Sie **Provisioning Profile** , und wählen Sie **App-IDs**, klicken Sie dann auf die **+** Schaltfläche.
4. Geben Sie einen **Namen** für das neue Profil.
5. Geben Sie einen **Paket-ID** Apple befolgen die Empfehlung namensgebungsattribute.
6. Führen Sie einen Bildlauf nach unten zu der **Anwendungsdienste** Abschnitt **SiriKit** , und klicken Sie auf die **Fortfahren** Schaltfläche: 

    [![](implementing-sirikit-images/setup03.png "Wählen Sie SiriKit")](implementing-sirikit-images/setup03.png#lightbox)
7. Überprüfen Sie alle Einstellungen, klicken Sie dann **Absenden** der App ID.
8. Wählen Sie **Provisioning Profile** > **Entwicklung**, klicken Sie auf die **+** auswählen die **Apple-ID**, Klicken Sie dann auf **Fortfahren**.
9. Klicken Sie auf auswählen **alle**, klicken Sie dann auf **Fortfahren**.
10. Klicken Sie auf **Alles markieren** erneut aus, klicken Sie dann auf **Fortfahren**.
11. Geben Sie einen **Profilname** mithilfe von Apple die Vorschläge namensgebungsattribute, klicken Sie dann auf **Fortfahren**.
12. Starten Sie Xcode.
13. Wählen Sie in der Menüleiste Xcode **Einstellungen...**
14. Wählen Sie **Konten**, klicken Sie dann auf die **"Details anzeigen"...** Schaltfläche: 

    [![](implementing-sirikit-images/setup04.png "Wählen Sie die Konten")](implementing-sirikit-images/setup04.png#lightbox)
15. Klicken Sie auf die **alle Profile herunterladen** Schaltfläche in der unteren linken Ecke: 

    [![](implementing-sirikit-images/setup05.png "Herunterladen von allen Profilen")](implementing-sirikit-images/setup05.png#lightbox)
16. Sicherstellen, dass die **Bereitstellungsprofil** erstellt höher in Xcode installiert wurde.
17. Öffnen Sie das Projekt zum Hinzufügen von SiriKit-Support, um in Visual Studio für Mac.
18. Doppelklicken Sie auf die `Info.plist` in der Datei die **Projektmappen-Explorer**.
18. Sicherstellen, dass die **Paket-ID** im Apple-Entwicklerportal oben erstellten entspricht: 

    [![](implementing-sirikit-images/setup06.png "Die Paket-ID")](implementing-sirikit-images/setup06.png#lightbox)
18. In der **Projektmappen-Explorer**, wählen die **Projekt**.
19. Mit der rechten Maustaste des Projekts, und wählen Sie **Optionen**.
21. Wählen Sie **iOS Bundle Signing**, wählen die **Signieren Identität** und **Bereitstellungsprofil** oben erstellten: 

    [![](implementing-sirikit-images/setup07.png "Wählen Sie die Signierung von Identität und Bereitstellungsprofil")](implementing-sirikit-images/setup07.png#lightbox)
22. Klicken Sie auf die Schaltfläche **OK**, um die Änderungen zu speichern.

> [!IMPORTANT]
> Testen SiriKit funktioniert nur auf einem echten iOS 10 Hardwaregerät und nicht in der iOS-10 Simulator. Wenn Xamarin.iOS-app auf echter Hardware haben Sie Probleme bei der Installation von einem SiriKit aktiviert werden, stellen Sie sicher, dass die erforderlichen Berechtigungen, die App-ID, die Signatur-ID und die Bereitstellungsprofil ordnungsgemäß im Apple Entwicklerportal und Visual Studio für Mac konfiguriert wurden

### <a name="requesting-siri-authorization"></a>Siri Autorisierung anfordern

Bevor die app fügt alle Benutzer bestimmte Vokabular oder die Intents Erweiterungen mit Siri verbunden, muss er fordert Autorisierung vom Benutzer Siri den Zugriff auf.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Bearbeiten Sie der app `Info.plist` Datei, wechseln Sie zu der **Quelle** anzeigen und Hinzufügen der `NSSiriUsageDescription` Schlüssel mit einem Zeichenfolgenwert, der beschreibt, wie die app Siri und was verwenden soll Datentypen gesendet werden. Beispielsweise könnte die MonkeyChat app das "MonkeyChat Kontakte an Siri gesendet werden":

[![](implementing-sirikit-images/request01.png "Die NSSiriUsageDescription in der Datei \"Info.plist\"-editor")](implementing-sirikit-images/request01.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Bearbeiten Sie der app `Info.plist` und fügen die `NSSiriUsageDescription` Schlüssel mit einem Zeichenfolgenwert, der beschreibt, wie die app Siri und was verwenden soll Datentypen gesendet werden. Beispielsweise könnte die MonkeyChat app das "MonkeyChat Kontakte an Siri gesendet werden":

[![](implementing-sirikit-images/request01w.png "Die NSSiriUsageDescription in der Datei \"Info.plist\"-editor")](implementing-sirikit-images/request01w.png#lightbox)

-----

Rufen Sie die `RequestSiriAuthorization` Methode der `INPreferences` Klasse bei die app zum ersten Mal startet. Bearbeiten der `AppDelegate.cs` Klasse, und stellen die `FinishedLaunching` Methode aussehen wie folgt:


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

Diese Methode aufgerufen wird, das zum ersten Mal wird eine Warnung angezeigt, wenn der Benutzer aufgefordert, die app Siri zugreifen kann. Die Nachricht, die der Entwickler hinzugefügt der `NSSiriUsageDescription` oben in dieser Warnung angezeigt. Wenn der Benutzer zunächst Zugriff verweigert wird, können sie die **Einstellungen** app Zugriff auf die app zu gewähren.

Zu jedem Zeitpunkt sehen die app die app Zugriff auf Siri durch Aufrufen der `SiriAuthorizationStatus` Methode der `INPreferences` Klasse.

### <a name="localization-and-siri"></a>Lokalisierung und Siri

Der Benutzer kann auf einem iOS-Gerät wählen Sie eine Sprache für Siri, das sich dann dem als Standard. Bei der Arbeit mit lokalisierten Daten müssen die app mit der `SiriLanguageCode` Methode der `INPreferences` Klasse so Siri den Sprachcode ab. Zum Beispiel:

```csharp
var language = INPreferences.SiriLanguageCode();

// Take action based on language
if (language == "en-US") {
    // Do something...
}
```

### <a name="adding-user-specific-vocabulary"></a>Bestimmte Benutzer-Vokabular hinzufügen

Der Benutzer bestimmte Vokabular geht zum Bereitstellen von Wörtern oder Ausdrücken, die an einzelne Benutzer der app eindeutig sind. Diese werden als eine geordnete Menge von Begriffen, die in einem signifikantesten Priorität für die Benutzer, mit der die wichtigsten Begriffe am Anfang der Liste sortiert zur Laufzeit aus dem Haupt-app (nicht die App-Erweiterungen) angegeben werden.

Bestimmte Benutzer-Vokabular muss auf einen der folgenden Kategorien gehören:

- Wenden Sie sich an Namen (die nicht durch das Framework Kontakte verwaltet werden).
- Foto-Tags.
- Fotoalbum Namen.
- Trainings-Namen.

Beim Auswählen von Terminologie als benutzerdefinierte Vokabular zu registrieren, und wählen Sie nur Begriffe, die möglicherweise von einem Benutzer nicht vertraut sind, mit der app falsch verstanden werden. Nie Register häufig verwendete Begriffe wie "Meine Trainings" oder "Meine Album". Z. B. Registrieren der MonkeyChat-app die Spitznamen einzelnen Kontakt im Adressbuch des Benutzers zugeordnet ist.

Die app enthält die Benutzer bestimmten Vokabular, das durch Aufrufen der `SetVocabularyStrings` Methode der `INVocabulary` -Klasse und übergeben einer `NSOrderedSet` Auflistung aus dem Haupt-app. Rufen Sie die app sollte immer die `RemoveAllVocabularyStrings` Methode zuerst, entfernen Sie alle vorhandenen zustimmen, bevor Sie weitere hinzufügen. Zum Beispiel:

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

Mit diesem Code werden kann er wie folgt aufgerufen:

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
> Siri benutzerdefinierte Vokabular als Hinweise behandelt und wird so viel wie möglich-Terminologie zu integrieren. Allerdings Platz für benutzerdefinierte Vokabular beschränkt somit registrieren wichtig ist _nur_ die Terminologie, die möglicherweise verwirrend, halten daher die Gesamtzahl der registrierten Begriffe auf ein Minimum.

Weitere Informationen finden Sie unter unsere [bestimmte Vokabular Benutzerreferenz](~/ios/platform/sirikit/understanding-sirikit.md) und Apple [angeben benutzerdefinierter Vokabular Verweis](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SpecifyingCustomVocabulary.html#//apple_ref/doc/uid/TP40016875-CH6-SW1).

### <a name="adding-app-specific-vocabulary"></a>Hinzufügen von App-spezifische Vokabular

Das bestimmte App-Vokabular definiert die bestimmte Wörter und Ausdrücke, die bekannt sein muss, werden für alle Benutzer mit der app, z. B. Fahrzeugtypen oder Namen des Trainings an. Da diese Teil der Anwendung sind, werden in definiert eine `AppIntentVocabulary.plist` -Datei als Teil des Haupt-app-Pakets. Darüber hinaus sollten diese Wörter und Ausdrücke lokalisiert werden.

App-spezifische Vokabular Begriffe müssen auf einen der folgenden Kategorien gehören:

- Außer Kraft setzen und Optionen.
- Trainings-Namen.

Die App bestimmte Vokabular-Datei enthält zwei Schlüssel der Stamm-Ebene:

- `ParameterVocabularies` **Erforderliche** -definiert benutzerdefinierte Begriffe und Absicht-Parameter, die sie für gelten der app.
- `IntentPhrases` **Optionale** -Beispiel-Ausdrücken, die mit der benutzerdefinierten Begriffe enthält die `ParameterVocabularies`.

Jeder Eintrag in der `ParameterVocabularies` müssen angeben, eine ID-Zeichenfolge, Begriff und die Absicht, die für die Benennung gilt. Darüber hinaus kann ein einzelner Ausdruck mehrere Intents zuweisen.

Eine vollständige Liste der zulässigen Werte und die erforderliche Datei-Struktur finden Sie unter der Apple- [App Vokabular Format Dateiverweis](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CustomVocabularyKeys.html#//apple_ref/doc/uid/TP40016875-CH10-SW1).

Hinzufügen einer `AppIntentVocabulary.plist` -Datei in das app-Projekt, gehen Sie folgendermaßen vor:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Mit der rechten Maustaste in den Namen des Projekts die **Projektmappen-Explorer** , und wählen Sie **hinzufügen** > **neue Datei...**   >  **iOS**:

    [![](implementing-sirikit-images/plist01.png "Fügen Sie eine Liste mit Eigenschaften hinzu.")](implementing-sirikit-images/plist01.png#lightbox)
2. Doppelklicken Sie auf die `AppIntentVocabulary.plist` in der Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen.
3. Klicken Sie auf die **+** um einen Schlüssel hinzuzufügen, legen Sie die **Namen** auf `ParameterVocabularies` und die **Typ** auf `Array`:

    [![](implementing-sirikit-images/plist02.png "Legen Sie den Namen ParameterVocabularies und der Typ, Arrays")](implementing-sirikit-images/plist02.png#lightbox)
4. Erweitern Sie `ParameterVocabularies` , und klicken Sie auf die **+** Schaltfläche, und legen Sie die **Typ** auf `Dictionary`:

    [![](implementing-sirikit-images/plist03.png "Legen Sie den Typ zum Wörterbuch")](implementing-sirikit-images/plist03.png#lightbox)
5. Klicken Sie auf die **+** um einen neuen Schlüssel hinzuzufügen, legen Sie die **Namen** auf `ParameterNames` und die **Typ** auf `Array`:

    [![](implementing-sirikit-images/plist04.png "Legen Sie den Namen ParameterNames und der Typ, Arrays")](implementing-sirikit-images/plist04.png#lightbox)
6. Klicken Sie auf die **+** Hinzufügen eines neuen Schlüssels mit dem **Typ** der `String` und der Wert als eine der verfügbaren Parameternamen. Z. B. `INStartWorkoutIntent.workoutName`:

    [![](implementing-sirikit-images/plist05.png "Der Schlüssel INStartWorkoutIntent.workoutName")](implementing-sirikit-images/plist05.png#lightbox)
7. Hinzufügen der `ParameterVocabulary` um einen der `ParameterVocabularies` Schlüssel mit dem **Typ** von `Array`:

    [![](implementing-sirikit-images/plist06.png "Den Schlüssel ParameterVocabulary dem ParameterVocabularies Schlüssel mit dem Typarray hinzufügen")](implementing-sirikit-images/plist06.png#lightbox)
8. Fügen Sie einen neuen Schlüssel mit dem **Typ** von `Dictionary`:

    [![](implementing-sirikit-images/plist07.png "Fügen Sie einen neuen Schlüssel mit dem Typwörterbuch hinzu.")](implementing-sirikit-images/plist07.png#lightbox)
9. Hinzufügen der `VocabularyItemIdentifier` Schlüssel mit dem **Typ** von `String` , und geben Sie eine eindeutige ID für den Begriff:

    [![](implementing-sirikit-images/plist08.png "Fügen Sie der VocabularyItemIdentifier-Schlüssel mit der Zeichenfolge ein, und geben Sie eine eindeutige ID")](implementing-sirikit-images/plist08.png#lightbox)
10. Hinzufügen der `VocabularyItemSynonyms` Schlüssel mit dem **Typ** von `Array`:

    [![](implementing-sirikit-images/plist09.png "Fügen Sie der VocabularyItemSynonyms-Schlüssel mit dem Typarray")](implementing-sirikit-images/plist09.png#lightbox)
11. Fügen Sie einen neuen Schlüssel mit dem **Typ** von `Dictionary`:

    [![](implementing-sirikit-images/plist10.png "Fügen Sie einen neuen Schlüssel mit dem Typwörterbuch hinzu.")](implementing-sirikit-images/plist10.png#lightbox)
12. Hinzufügen der `VocabularyItemPhrase` Schlüssel mit dem **Typ** von `String` und den Begriff die app definieren:

    [![](implementing-sirikit-images/plist11.png "Fügen Sie der VocabularyItemPhrase-Schlüssel mit der Typzeichenfolge \"und\" den Begriff, die die app definieren")](implementing-sirikit-images/plist11.png#lightbox)
13. Hinzufügen der `VocabularyItemPronunciation` Schlüssel mit dem **Typ** von `String` und die phonetische Aussprache des Begriffs:

    [![](implementing-sirikit-images/plist12.png "Fügen Sie der VocabularyItemPronunciation-Schlüssel mit der Zeichenfolge und die phonetische Aussprache des Begriffs")](implementing-sirikit-images/plist12.png#lightbox)
14. Hinzufügen der `VocabularyItemExamples` Schlüssel mit dem **Typ** von `Array`:

    [![](implementing-sirikit-images/plist13.png "Fügen Sie der VocabularyItemExamples-Schlüssel mit dem Typarray")](implementing-sirikit-images/plist13.png#lightbox)
15. Fügen Sie ein Paar `String` Schlüssel mit wird mithilfe des Begriffs:

    [![](implementing-sirikit-images/plist14.png "Fügen Sie ein paar Zeichenfolgenschlüssel mit wird mithilfe des Begriffs")](implementing-sirikit-images/plist14.png#lightbox)
16. Wiederholen Sie die oben genannten Schritte für andere müssen die app definieren und benutzerdefinierte Begriffe aus.
17. Reduzieren der `ParameterVocabularies` Schlüssel.
18. Hinzufügen der `IntentPhrases` Schlüssel mit dem **Typ** von `Array`:

    [![](implementing-sirikit-images/plist15.png "Fügen Sie der IntentPhrases-Schlüssel mit dem Typarray")](implementing-sirikit-images/plist15.png#lightbox)
19. Fügen Sie einen neuen Schlüssel mit dem **Typ** von `Dictionary`:

    [![](implementing-sirikit-images/plist16.png "Fügen Sie einen neuen Schlüssel mit dem Typwörterbuch hinzu.")](implementing-sirikit-images/plist16.png#lightbox)
20. Hinzufügen der `IntentName` Schlüssel mit dem **Typ** von `String` und beabsichtigte für das Beispiel:

    [![](implementing-sirikit-images/plist17.png "Fügen Sie der IntentName-Schlüssel mit dem Typ der Zeichenfolge und die Absicht für das Beispiel")](implementing-sirikit-images/plist17.png#lightbox)
21. Hinzufügen der `IntentExamples` Schlüssel mit dem **Typ** von `Array`:

    [![](implementing-sirikit-images/plist18.png "Fügen Sie der IntentExamples-Schlüssel mit dem Typarray")](implementing-sirikit-images/plist18.png#lightbox)
22. Fügen Sie ein Paar `String` Schlüssel mit wird mithilfe des Begriffs:

    [![](implementing-sirikit-images/plist19.png "Fügen Sie ein paar Zeichenfolgenschlüssel mit wird mithilfe des Begriffs")](implementing-sirikit-images/plist19.png#lightbox)
23. Wiederholen Sie die oben genannten Schritte für alle Prioritäten müssen die app bereitstellen, Beispiele für die Nutzung von ein.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Mit der rechten Maustaste in den Namen des Projekts die **Projektmappen-Explorer** , und wählen Sie **hinzufügen > Neues Element… > Apple > Property List > "Info.plist"**:

    [![](implementing-sirikit-images/plist01.w157-sml.png "Fügen Sie eine neue Datei \"Info.plist\" hinzu.")](implementing-sirikit-images/plist01.w157.png#lightbox)

2. Doppelklicken Sie auf die `AppIntentVocabulary.plist` in der Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen.
3. Klicken Sie auf die **+** um einen Schlüssel hinzuzufügen, legen Sie die **Namen** auf `ParameterVocabularies` und die **Typ** auf `Array`:

    [![](implementing-sirikit-images/plist02w.png "Legen Sie den Namen ParameterVocabularies und der Typ, Arrays")](implementing-sirikit-images/plist02w.png#lightbox)
4. Erweitern Sie `ParameterVocabularies` , und klicken Sie auf die **+** Schaltfläche, und legen Sie die **Typ** auf `Dictionary`:

    [![](implementing-sirikit-images/plist03w.png "Legen Sie den Typ zum Wörterbuch")](implementing-sirikit-images/plist03w.png#lightbox)
5. Klicken Sie auf die **+** um einen neuen Schlüssel hinzuzufügen, legen Sie die **Namen** auf `ParameterNames` und die **Typ** auf `Array`:

    [![](implementing-sirikit-images/plist04w.png "Legen Sie den Namen ParameterNames und der Typ, Arrays")](implementing-sirikit-images/plist04w.png#lightbox)
6. Klicken Sie auf die **+** Hinzufügen eines neuen Schlüssels mit dem **Typ** der `String` und der Wert als eine der verfügbaren Parameternamen. Z. B. `INStartWorkoutIntent.workoutName`:

    [![](implementing-sirikit-images/plist05w.png "Der Schlüssel INStartWorkoutIntent.workoutName")](implementing-sirikit-images/plist05w.png#lightbox)
7. Hinzufügen der `ParameterVocabulary` um einen der `ParameterVocabularies` Schlüssel mit dem **Typ** von `Array`:

    [![](implementing-sirikit-images/plist06w.png "Den Schlüssel ParameterVocabulary dem ParameterVocabularies Schlüssel mit dem Typarray hinzufügen")](implementing-sirikit-images/plist06w.png#lightbox)
8. Fügen Sie einen neuen Schlüssel mit dem **Typ** von `Dictionary`:

    [![](implementing-sirikit-images/plist07w.png "Fügen Sie einen neuen Schlüssel mit dem Typwörterbuch hinzu.")](implementing-sirikit-images/plist07w.png#lightbox)
9. Hinzufügen der `VocabularyItemIdentifier` Schlüssel mit dem **Typ** von `String` , und geben Sie eine eindeutige ID für den Begriff:

    [![](implementing-sirikit-images/plist08w.png "Fügen Sie der VocabularyItemIdentifier-Schlüssel mit der Zeichenfolge, und geben Sie eine eindeutige ID für den Begriff")](implementing-sirikit-images/plist08w.png#lightbox)
10. Hinzufügen der `VocabularyItemSynonyms` Schlüssel mit dem **Typ** von `Array`:

    [![](implementing-sirikit-images/plist09w.png "Fügen Sie der VocabularyItemSynonyms-Schlüssel mit dem Typarray")](implementing-sirikit-images/plist09w.png#lightbox)
11. Fügen Sie einen neuen Schlüssel mit dem **Typ** von `Dictionary`:

    [![](implementing-sirikit-images/plist10w.png "Fügen Sie einen neuen Schlüssel mit dem Typwörterbuch hinzu.")](implementing-sirikit-images/plist10w.png#lightbox)
12. Hinzufügen der `VocabularyItemPhrase` Schlüssel mit dem **Typ** von `String` und den Begriff die app definieren:

    [![](implementing-sirikit-images/plist11w.png "Fügen Sie der VocabularyItemPhrase-Schlüssel mit der Typzeichenfolge \"und\" den Begriff, die die app definieren")](implementing-sirikit-images/plist11w.png#lightbox)
13. Hinzufügen der `VocabularyItemPronunciation` Schlüssel mit dem **Typ** von `String` und die phonetische Aussprache des Begriffs:

    [![](implementing-sirikit-images/plist12w.png "Fügen Sie der VocabularyItemPronunciation-Schlüssel mit der Zeichenfolge und die phonetische Aussprache des Begriffs")](implementing-sirikit-images/plist12w.png#lightbox)
14. Hinzufügen der `VocabularyItemExamples` Schlüssel mit dem **Typ** von `Array`:

    [![](implementing-sirikit-images/plist13w.png "Fügen Sie der VocabularyItemExamples-Schlüssel mit dem Typarray")](implementing-sirikit-images/plist13w.png#lightbox)
15. Fügen Sie ein Paar `String` Schlüssel mit wird mithilfe des Begriffs:

    [![](implementing-sirikit-images/plist14w.png "Fügen Sie ein paar Zeichenfolgenschlüssel mit wird mithilfe des Begriffs")](implementing-sirikit-images/plist14w.png#lightbox)
16. Wiederholen Sie die oben genannten Schritte für andere müssen die app definieren und benutzerdefinierte Begriffe aus.
17. Reduzieren der `ParameterVocabularies` Schlüssel.
18. Hinzufügen der `IntentPhrases` Schlüssel mit dem **Typ** von `Array`:

    [![](implementing-sirikit-images/plist15w.png "Fügen Sie der IntentPhrases-Schlüssel mit dem Typarray")](implementing-sirikit-images/plist15w.png#lightbox)
19. Fügen Sie einen neuen Schlüssel mit dem **Typ** von `Dictionary`:

    [![](implementing-sirikit-images/plist16w.png "Fügen Sie einen neuen Schlüssel mit dem Typwörterbuch hinzu.")](implementing-sirikit-images/plist16w.png#lightbox)
20. Hinzufügen der `IntentName` Schlüssel mit dem **Typ** von `String` und beabsichtigte für das Beispiel:

    [![](implementing-sirikit-images/plist17w.png "Fügen Sie der IntentName-Schlüssel mit dem Typ der Zeichenfolge und die Absicht für das Beispiel")](implementing-sirikit-images/plist17w.png#lightbox)
21. Hinzufügen der `IntentExamples` Schlüssel mit dem **Typ** von `Array`:

    [![](implementing-sirikit-images/plist18w.png "Fügen Sie der IntentExamples-Schlüssel mit dem Typarray")](implementing-sirikit-images/plist18w.png#lightbox)
22. Fügen Sie ein Paar `String` Schlüssel mit wird mithilfe des Begriffs:

    [![](implementing-sirikit-images/plist19w.png "Fügen Sie ein paar Zeichenfolgenschlüssel mit wird mithilfe des Begriffs")](implementing-sirikit-images/plist19w.png#lightbox)
23. Wiederholen Sie die oben genannten Schritte für alle Prioritäten müssen die app bereitstellen, Beispiele für die Nutzung von ein.

-----

> [!IMPORTANT]
> Die `AppIntentVocabulary.plist` registriert wird mit Siri auf dem Testcomputer Geräte während der Entwicklung und es können etwas dauern Siri auf das benutzerdefinierte Vokabular zu integrieren. Folglich müssen der Tester warten Sie einige Minuten, bevor Sie versuchen, bestimmte App-Vokabular zu testen, wenn Daten aktualisiert wurden.

Weitere Informationen finden Sie unter unsere [App bestimmte Vokabular Verweis](~/ios/platform/sirikit/understanding-sirikit.md) und Apple [angeben benutzerdefinierter Vokabular Verweis](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SpecifyingCustomVocabulary.html#//apple_ref/doc/uid/TP40016875-CH6-SW1).

## <a name="adding-an-intents-extension"></a>Eine Erweiterung Intents hinzufügen

Nun, dass die app SiriKit Übernahme vorbereitet wurde, müssen die Entwickler der Projektmappe, die für die Integration von Siri benötigten Absichten behandeln (mindestens) Intents Erweiterung hinzufügen.

Führen Sie für jede Intents-Erweiterung erforderlich sind folgende Schritte aus:

- Fügen Sie ein Erweiterungsprojekt Intents hinzu Xamarin.iOS-Anwendungsprojektmappe.
- Konfigurieren Sie die Erweiterung Intents `Info.plist` Datei.
- Ändern Sie die Hauptklasse Intents-Erweiterung.

Weitere Informationen finden Sie unter unsere [Erweiterungsverweis für die Intents](~/ios/platform/sirikit/understanding-sirikit.md) und Apple [erstellen den Intents erweiterungsverweis](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CreatingtheIntentsExtension.html#//apple_ref/doc/uid/TP40016875-CH4-SW1).

### <a name="creating-the-extension"></a>Erstellen die Erweiterung

Um eine Erweiterung Intents zur Projektmappe hinzuzufügen, führen Sie folgende Schritte aus:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Mit der rechten Maustaste auf die **Projektmappenname** in der **Lösung Pad** , und wählen Sie **hinzufügen** > **neues Projekt hinzufügen...** .
2. Wählen Sie aus dem Dialogfeld **iOS** > **Erweiterungen** > **Absicht Erweiterung** , und klicken Sie auf die **Weiter** Schaltfläche: 

    [![](implementing-sirikit-images/intents05.png "Beabsichtigte Erweiterung auswählen")](implementing-sirikit-images/intents05.png#lightbox)
3. Geben Sie anschließend eine **Namen** für die Absicht-Erweiterung, und klicken Sie auf die **Weiter** Schaltfläche: 

    [![](implementing-sirikit-images/intents06.png "Geben Sie einen Namen für die beabsichtigte Erweiterung")](implementing-sirikit-images/intents06.png#lightbox)
4. Klicken Sie abschließend auf die **erstellen** Schaltfläche, um die Absicht-Erweiterung der apps-Projektmappe hinzuzufügen: 

    [![](implementing-sirikit-images/intents07.png "Fügen Sie die Absicht-Erweiterung der apps-Projektmappe")](implementing-sirikit-images/intents07.png#lightbox)
5. In der **Projektmappen-Explorer**, mit der rechten Maustaste auf die **Verweise** Ordner der neu erstellten Absicht-Erweiterung. Überprüfen Sie den Namen der das Projekt mit freigegebenem Code Bibliothek gemeinsame (die die app oben erstellt haben), und klicken Sie auf die **OK** Schaltfläche: 

    [![](implementing-sirikit-images/intents08.png "Wählen Sie den Namen, der die allgemeine Bibliotheksprojekt mit freigegebenem code")](implementing-sirikit-images/intents08.png#lightbox)
    
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Mit der rechten Maustaste auf die **Projektmappenname** in der **Projektmappen-Explorer** , und wählen Sie **hinzufügen** > **neues Projekt hinzufügen...** .
2. Wählen Sie im Dialogfeld **Visual c# > iOS Extensions > Absicht Erweiterung** , und klicken Sie auf die **Weiter** Schaltfläche:

    [![](implementing-sirikit-images/intents05.w157-sml.png "Beabsichtigte Erweiterung auswählen")](implementing-sirikit-images/intents05.w157.png#lightbox)
3. Geben Sie anschließend eine **Namen** für die Absicht-Erweiterung, und klicken Sie auf die **OK** Schaltfläche.
1. In der **Projektmappen-Explorer**, mit der rechten Maustaste auf die **Verweise** der Erweiterung Intents neu erstellten Ordner, und wählen Sie **hinzufügen > Verweis**. Überprüfen Sie den Namen der das Projekt mit freigegebenem Code Bibliothek gemeinsame (die die app oben erstellt haben), und klicken Sie auf die **OK** Schaltfläche:

    [![](implementing-sirikit-images/intents08w.png "Wählen Sie den Namen, der die allgemeine Bibliotheksprojekt mit freigegebenem code")](implementing-sirikit-images/intents08w.png#lightbox)
    
-----

Wiederholen Sie diese Schritte für die Anzahl der Absicht Extensions (basierend auf [Architektur der App für Erweiterungen](#Architecting-the-App-for-Extensions) obigen Abschnitt ""), die die app ist erforderlich.

### <a name="configuring-the-infoplist"></a>Konfigurieren die Datei "Info.plist"

Für jede Intents Erweiterungen, die die app-Projektmappe hinzugefügt haben, müssen in konfiguriert werden die `Info.plist` Dateien mit der app verwendet.

Genau wie eine typische App-Erweiterung wird die app über keine der vorhandenen Schlüssel von `NSExtension` und `NSExtensionAttributes`. Es gibt zwei neue Attribute, die konfiguriert werden müssen, nach einer Erweiterung Intents:

[![](implementing-sirikit-images/intents01.png "Die beiden neuen Attribute, die konfiguriert werden müssen")](implementing-sirikit-images/intents01.png#lightbox)

- **IntentsSupported** – ist erforderlich und besteht aus einem Array von Absicht Klassennamen, die die app von der Absicht-Erweiterung unterstützen möchte.
- **IntentsRestrictedWhileLocked** -ist ein optionaler Schlüssel für die app aus, um die Erweiterung Sperre Bildschirm Verhalten anzugeben. Es besteht aus einem Array von Absicht Klassennamen, die die app möchte der Benutzer angemeldet sein, von der Absicht-Erweiterung verwenden.

So konfigurieren Sie die Absicht Erweiterung `Info.plist` file, doppelklicken Sie darauf in der **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen. Als Nächstes wechseln Sie zu der **Quelle** anzeigen und erweitern Sie dann die `NSExtension` und `NSExtensionAttributes` Schlüssel in den Editor:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![](implementing-sirikit-images/intents02.png "Die NSExtension und NSExtensionAttributes Schlüssel im editor")](implementing-sirikit-images/intents02.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](implementing-sirikit-images/intents02w.png "Die NSExtension und NSExtensionAttributes Schlüssel im editor")](implementing-sirikit-images/intents02w.png#lightbox)

-----

Erweitern Sie die `IntentsSupported` Schlüssel und fügen Sie den Namen einer beliebigen Zweck-Klasse, die diese Erweiterung unterstützt. Für die beispielanwendung MonkeyChat unterstützt die `INSendMessageIntent`:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![](implementing-sirikit-images/intents09.png "Der Schlüssel INSendMessageIntent")](implementing-sirikit-images/intents09.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](implementing-sirikit-images/intents09w.png "Der Schlüssel INSendMessageIntent")](implementing-sirikit-images/intents09w.png#lightbox)

-----

Wenn die app optional erforderlich ist, dass der Benutzer angemeldet sein, auf dem Gerät für die Verwendung von einem bestimmten Zweck, erweitern die `IntentRestrictedWhileLocked` Schlüssel und fügen Sie die Klassennamen von die Absichten, die über eingeschränkten Zugriff verfügen. Für die beispielanwendung MonkeyChat der Benutzer muss angemeldet sein, eine Chatnachricht senden, damit es hinzugefügt haben `INSendMessageIntent`:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![](implementing-sirikit-images/intents10.png "Die hinzugefügten INSendMessageIntent Schlüssel")](implementing-sirikit-images/intents10.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](implementing-sirikit-images/intents10w.png "Die hinzugefügten INSendMessageIntent Schlüssel")](implementing-sirikit-images/intents10w.png#lightbox)

-----


Eine vollständige Liste der verfügbaren Domänen der Absicht, finden Sie im Apple [Absicht Domänen Verweis](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2).

### <a name="configuring-the-main-class"></a>Konfigurieren die Hauptklasse.

Als Nächstes muss der Entwickler die Hauptklasse konfigurieren, die als der Haupteinstiegspunkt für die Erweiterung Absicht in Siri fungiert. Es muss eine Unterklasse von `INExtension` entspricht dem der `IINIntentHandler` delegieren. Zum Beispiel:

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

Es gibt eine bezugslosen-Methode, die die app, auf die Hauptklasse Absicht-Erweiterung implementieren muss, die `GetHandler` Methode. Diese Methode Priorität als SiriKit übergeben wird und die app Zurückgeben einer **Absicht Handler** , die mit dem Typ der angegebenen Absicht übereinstimmt.

Da die Beispiel-app MonkeyChat nur eine beabsichtigte verarbeitet, wird zurückgeben selbst in die `GetHandler` Methode. Wenn die Erweiterung mehr als einer Absicht behandelt, der Entwickler fügen Sie eine Klasse für jeden beabsichtigte und hier basierte auf eine Instanz zurückgegeben würde die `Intent` an die Methode übergeben.

### <a name="handling-the-resolve-stage"></a>Behandlung von der Resolve-Phase

Die Stufe zu beheben, in dem die Absicht Erweiterung verdeutlichen und Parameter überprüfen übergeben von Siri und über den Benutzer Konversation festgelegt wurden.

Für jeden Parameter, die von Siri gesendet wird, besteht eine `Resolve` Methode. So implementieren Sie diese Methode für jeden Parameter, dass die app Siris Hilfe ', um die richtige Antwort vom Benutzer erhalten möglicherweise müssen die app.

Bei der beispielanwendung MonkeyChat erfordern die Absicht Erweiterung eine einen oder mehrere Empfänger, die Nachricht zu senden. Für jeden Empfänger in der Liste müssen die Erweiterung einen Kontakt suchen, die das folgende Ergebnis haben kann:

- Genau ein übereinstimmender Kontakt gefunden wird.
- Mindestens zwei übereinstimmende Kontakte gefunden werden.
- Es wurden keine übereinstimmenden Kontakte gefunden.

Darüber hinaus erfordert MonkeyChat Inhalt für den Text der Nachricht. Wenn der Benutzer dies nicht angegeben wurde, muss Siri auffordern den Benutzer für den Inhalt.

Die Absicht Erweiterung müssen jedem dieser Fälle ordnungsgemäß behandelt werden.


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

Weitere Informationen finden Sie unter unsere [die beheben Phase Reference](~/ios/platform/sirikit/understanding-sirikit.md) und Apple [Resolving und Behandlung von Intents Verweis](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/ResolvingandHandlingIntents.html#//apple_ref/doc/uid/TP40016875-CH5-SW1).

### <a name="handling-the-confirm-stage"></a>Behandlung von der Phase bestätigen

Die Phase bestätigen ist, in dem die Absicht Erweiterung überprüft, um anzuzeigen, dass sie alle Informationen zum Erfüllen der Anforderung des Benutzers hat. Möchte, dass die app senden, dass die Bestätigung entlang wird die unterstützende Details wie bevorsteht geschehen Siri, damit sie mit der Benutzer bestätigt werden kann, dass diese die beabsichtigte Aktion ist.

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

Weitere Informationen finden Sie unter unsere [die bestätigen Phase Reference](~/ios/platform/sirikit/understanding-sirikit.md).

### <a name="processing-the-intent"></a>Die Absicht verarbeiten

Dies ist der Punkt, bei denen die Absicht-Erweiterung den Task die Anforderung des Benutzers zu erfüllen und die Ergebnisse zurück an Siri übergeben, damit der Benutzer informiert werden kann tatsächlich ausführt.


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

Weitere Informationen finden Sie unter unsere [die behandeln Phase Reference](~/ios/platform/sirikit/understanding-sirikit.md).

## <a name="adding-an-intents-ui-extension"></a>Eine Intents UI-Erweiterung hinzufügen

Die optionale Intents Benutzeroberflächenerweiterung bietet die Möglichkeit, schalten Sie der app Benutzeroberfläche und in die-Oberfläche Siri branding und stellen die Benutzer können mit der app verbunden. Mit dieser Erweiterung kann die app Brand als auch visual und andere Informationen in die Aufzeichnung bringen.

[![](implementing-sirikit-images/intentsui01.png "Eine Beispielausgabe für Intents Benutzeroberflächenerweiterung")](implementing-sirikit-images/intentsui01.png#lightbox)

Genau wie die Erweiterung Intents führt der Entwickler den folgenden Schritt für die Benutzeroberflächenerweiterung Intents Schritte aus:

- Ein Projekt Intents Benutzeroberflächenerweiterung Xamarin.iOS app-Projektmappe hinzufügen.
- Konfigurieren Sie die Benutzeroberflächenerweiterung Intents `Info.plist` Datei.
- Ändern Sie die Hauptklasse Intents Benutzeroberflächenerweiterung.

Weitere Informationen finden Sie unter unsere [der Intents Erweiterung Benutzeroberflächenreferenz](~/ios/platform/sirikit/understanding-sirikit.md) und Apple [bietet eine Referenz zur Benutzeroberfläche benutzerdefinierte](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/ProvidingaCustomInterface.html#//apple_ref/doc/uid/TP40016875-CH7-SW1).

### <a name="creating-the-extension"></a>Erstellen die Erweiterung

Um eine Erweiterung des Intents UI der Lösung hinzugefügt haben, führen Sie folgende Schritte aus:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Mit der rechten Maustaste auf die **Projektmappenname** in der **Lösung Pad** , und wählen Sie **hinzufügen** > **neues Projekt hinzufügen...** .
2. Wählen Sie aus dem Dialogfeld **iOS** > **Erweiterungen** > **Absicht Benutzeroberflächenerweiterung** , und klicken Sie auf die **Weiter** Schaltfläche: 

    [![](implementing-sirikit-images/intents11.png "Wählen Sie die beabsichtigte Benutzeroberflächenerweiterung")](implementing-sirikit-images/intents11.png#lightbox)
3. Geben Sie anschließend eine **Namen** für die Absicht-Erweiterung, und klicken Sie auf die **Weiter** Schaltfläche: 

    [![](implementing-sirikit-images/intents12.png "Geben Sie einen Namen für die beabsichtigte Erweiterung")](implementing-sirikit-images/intents12.png#lightbox)
4. Klicken Sie abschließend auf die **erstellen** Schaltfläche, um die Absicht-Erweiterung der apps-Projektmappe hinzuzufügen: 

    [![](implementing-sirikit-images/intents13.png "Fügen Sie die Absicht-Erweiterung der apps-Projektmappe")](implementing-sirikit-images/intents13.png#lightbox)
5. In der **Projektmappen-Explorer**, mit der rechten Maustaste auf die **Verweise** Ordner der neu erstellten Absicht-Erweiterung. Überprüfen Sie den Namen der das Projekt mit freigegebenem Code Bibliothek gemeinsame (die die app oben erstellt haben), und klicken Sie auf die **OK** Schaltfläche: 

    [![](implementing-sirikit-images/intents14.png "Wählen Sie den Namen, der die allgemeine Bibliotheksprojekt mit freigegebenem code")](implementing-sirikit-images/intents14.png#lightbox)
    
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Mit der rechten Maustaste auf die **Projektmappenname** in der **Projektmappen-Explorer** , und wählen Sie **hinzufügen** > **neues Projekt hinzufügen...**
2. Wählen Sie aus dem Dialogfeld **iOS** > **Erweiterungen** > **Absicht Benutzeroberflächenerweiterung** , und klicken Sie auf die **Weiter** Schaltfläche ".
3. Geben Sie anschließend eine **Namen** für die Absicht-Erweiterung, und klicken Sie auf die **OK** Schaltfläche.
5. In der **Projektmappen-Explorer**, mit der rechten Maustaste auf die **Verweise** Ordner der neu erstellten Absicht-Erweiterung. Überprüfen Sie den Namen der das Projekt mit freigegebenem Code Bibliothek gemeinsame (die die app oben erstellt haben), und klicken Sie auf die **OK** Schaltfläche.
    
-----

### <a name="configuring-the-infoplist"></a>Konfigurieren die Datei "Info.plist"

Konfigurieren Sie die Benutzeroberflächenerweiterung Intents `Info.plist` Datei, die mit der app verwendet.

Genau wie eine typische App-Erweiterung wird die app über keine der vorhandenen Schlüssel von `NSExtension` und `NSExtensionAttributes`. Nach einer Erweiterung Intents es wird ein neues Attribut, das konfiguriert werden müssen:

[![](implementing-sirikit-images/intents03.png "Die ein neues Attribut, das konfiguriert werden müssen")](implementing-sirikit-images/intents03.png#lightbox)

**IntentsSupported** ist erforderlich und besteht aus einem Array von Absicht Klassennamen, die die app aus der Absicht Erweiterung unterstützt werden soll.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

So konfigurieren Sie die Absicht Benutzeroberflächenerweiterung `Info.plist` Datei, doppelklicken Sie darauf in der **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen. Als Nächstes wechseln Sie zu der **Quelle** anzeigen und erweitern Sie dann die `NSExtension` und `NSExtensionAttributes` Schlüssel in den Editor:

[![](implementing-sirikit-images/intents04.png "Die NSExtension und NSExtensionAttributes Schlüssel im editor")](implementing-sirikit-images/intents04.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

So konfigurieren Sie die Absicht Benutzeroberflächenerweiterung `Info.plist` Datei, doppelklicken Sie darauf in der **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen. Erweitern Sie die `NSExtension` und `NSExtensionAttributes` Schlüssel in den Editor:

[![](implementing-sirikit-images/intents04w.png "Tthe NSExtension und NSExtensionAttributes Schlüssel im editor")](implementing-sirikit-images/intents04w.png#lightbox)

-----

Erweitern Sie die `IntentsSupported` Schlüssel und fügen Sie den Namen einer beliebigen Zweck-Klasse, die diese Erweiterung unterstützt. Für die beispielanwendung MonkeyChat unterstützt die `INSendMessageIntent`:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![](implementing-sirikit-images/intents15.png "Der Schlüssel INSendMessageIntent")](implementing-sirikit-images/intents15.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](implementing-sirikit-images/intents15w.png "Der Schlüssel INSendMessageIntent")](implementing-sirikit-images/intents15w.png#lightbox)

-----

Eine vollständige Liste der verfügbaren Domänen der Absicht, finden Sie im Apple [Absicht Domänen Verweis](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2).

### <a name="configuring-the-main-class"></a>Konfigurieren die Hauptklasse.

Konfigurieren Sie die Hauptklasse, die als der Haupteinstiegspunkt für die Absicht Benutzeroberflächenerweiterung in Siri fungiert. Es muss eine Unterklasse von `UIViewController` entspricht dem der `IINUIHostedViewController` Schnittstelle. Zum Beispiel:

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

Siri übergeben wird eine `INInteraction` Klasseninstanz, die die `Configure` Methode der `UIViewController` Instanz innerhalb der Absicht Benutzeroberflächenerweiterung.

Die `INInteraction` Objekt bietet drei wichtige Arten von Informationen an die Erweiterung:

1. Das beabsichtigte Objekt verarbeitet wird.
2. Die Absicht Response-Objekt aus der `Confirm` und `Handle` Methoden der Absicht-Erweiterung.
3. Der Status der Aktivität, der den Status der Interaktion zwischen der Anwendung und Siri definiert.

Die `UIViewController` Instanz ist das Prinzip-Klasse für die Interaktion mit Siri und weil sie von erbt `UIViewController`, hat sie Zugriff auf alle Funktionen der UIKit.

Siri bei Aufruf der `Configure` Methode der `UIViewController` übergibt in einem Kontext anzeigen, die besagt, dass entweder die View-Controller in einem Siri Snippit oder Zuordnungen Karte gehostet wird.

Siri wird auch ein Abschlusshandler, der die app muss die gewünschte Größe der Ansicht zurückgeben, sobald die app haben, konfigurieren, dass er übergeben.

### <a name="design-the-ui-in-ios-designer"></a>Entwerfen der Benutzeroberfläche in iOS-Designer

Layout der Intents Benutzeroberflächenerweiterung die Benutzeroberfläche in der iOS-Designer. Doppelklicken Sie auf der Erweiterungs `MainInterface.storyboard` in der Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen. Ziehen Sie in allen erforderlichen Elemente der Benutzeroberfläche zum Erstellen der Benutzeroberfläche und die Änderungen zu speichern.

> [!IMPORTANT]
> Es ist zwar möglich, interaktive Elemente hinzufügen, z. B. `UIButtons` oder `UITextFields` auf der Absicht Benutzeroberflächenerweiterung `UIViewController`, diese sind ausschließlich als die Absicht-Benutzeroberfläche in nicht interaktiven unzulässig und der Benutzer ist nicht in der Lage, Sie mit ihnen interagieren.

### <a name="wire-up-the-user-interface"></a>Über das Netzwerk von der Benutzeroberfläche

Mit der Intents Benutzeroberflächenerweiterung-Benutzeroberfläche in iOS-Designer erstellt, bearbeitet die `UIViewController` -Unterklasse und überschreiben die `Configure` Methode wie folgt:

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

### <a name="overriding-the-default-siri-ui"></a>Überschreiben der Standardeinstellung Siri-Benutzeroberfläche

Die Benutzeroberflächenerweiterung Intents immer zusammen mit anderem Inhalt Siri, beispielsweise das Symbol "app" und der Name am oberen Rand der Benutzeroberfläche angezeigt oder wird, basierend auf der Absicht, Schaltflächen (z. B. "Senden" oder "Abbrechen" ") können im unteren Bereich angezeigt werden.

Es gibt einige Instanzen, in dem die app ersetzen können, die Informationen, die Siri für den Benutzer, z. B. messaging standardmäßig angezeigt wird oder Zuordnungen, in dem die app mit einem zugeschnitten, um die app die standarderfahrung ersetzen können.

Wenn die Benutzeroberflächenerweiterung Intents Elementen des Standardwerts Siri UI, überschreiben die `UIViewController` Unterklasse implementieren müssen die `IINUIHostedViewSiriProviding` Schnittstelle und zum Anzeigen von einem bestimmten Benutzeroberflächenelement teilnehmen.

Fügen Sie folgenden Code, der `UIViewController` Unterklasse Siri informieren, dass die Absicht Benutzeroberflächenerweiterung bereits Inhalt der Nachricht anzeigt:

```csharp
public bool DisplaysMessage {
    get {return true;}
}
```

### <a name="considerations"></a>Weitere Überlegungen

Apple empfiehlt, dass der Entwickler die folgenden Aspekte berücksichtigt beim Entwerfen und implementieren die Absicht UI-Erweiterungen ausführen:

- **Bewusste der Speicherauslastung werden** – da Absicht-UI-Erweiterungen sind temporär und nur für kurze Zeit angezeigt, wird das System erzwingt engere speicherbeschränkungen als mit einer vollständigen app verwendet werden.
- **Betrachten Sie die minimale und maximale Größe der Ansicht** -stellen Sie sicher, dass die Absicht UI-Erweiterungen auf jedem iOS-Gerät-Typ, Größe und Ausrichtung gut aussieht. Darüber hinaus kann die gewünschte Größe, die die app an Siri zurücksendet nicht erteilt werden können.
- **Verwenden Sie flexibel und Adaptive Standardbildschirmlayouts** – erneut, um sicherzustellen, dass die Benutzeroberfläche auf jedem Gerät aussieht.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat SiriKit behandelt und gezeigt, wie er hinzugefügt werden kann, an den Xamarin.iOS-apps, um Dienste bereitzustellen, die für den Benutzer mithilfe von Siri und die Maps-app auf einem iOS-Gerät zugänglich sind.




## <a name="related-links"></a>Verwandte Links

- [ElizaChat-Beispiel](https://developer.xamarin.com/samples/monotouch/ios10/ElizaChat/)
- [Programmierhandbuch SiriKit](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [Intents Frameworkverweis](https://developer.apple.com/reference/intents)
- [Referenz zur Benutzeroberfläche des Framework Intents](https://developer.apple.com/reference/intentsui)
- [Beabsichtigte Domänen-Referenz](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2)
