---
title: Die Übergabe in Xamarin.iOS
description: In diesem Artikel behandelt, die Arbeit mit Übergabe in einer Xamarin.iOS-app zum Übertragen von anderen Geräte des Benutzeraktivitäten zwischen apps, die für den Benutzer ausgeführt wird.
ms.prod: xamarin
ms.assetid: 405F966A-4085-4621-AA15-33D663AD15CD
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: 084b9924af467459a017413a958ec2e46ff219fc
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2019
ms.locfileid: "67865309"
---
# <a name="handoff-in-xamarinios"></a>Die Übergabe in Xamarin.iOS

_In diesem Artikel behandelt, die Arbeit mit Übergabe in einer Xamarin.iOS-app zum Übertragen von anderen Geräte des Benutzeraktivitäten zwischen apps, die für den Benutzer ausgeführt wird._

Apple führte Übergabe in iOS 8 und OS X Yosemite (10.10), geben Sie einen allgemeinen Mechanismus für den Benutzer zum Übertragen von Aktivitäten, die Schritte auf einem ihrer Geräte, auf ein anderes Gerät mit der gleichen app oder einer anderen app, die von der gleichen Aktivität unterstützt.

[![](handoff-images/handoff02.png "Ein Beispiel für eine Übergabe-Operation")](handoff-images/handoff02.png#lightbox)

In diesem Artikel übernimmt einen kurzen Blick auf die Aktivität, die Freigabe in einer Xamarin.iOS-app aktivieren und das Framework Handoff ausführlich behandelt:

## <a name="about-handoff"></a>Informationen zu Übergabeanimationen

Übergabe (auch bekannt als Geschäftskontinuität) wurde von Apple iOS 8 und OS X Yosemite (10.10) als eine Möglichkeit für den Benutzer an eine Aktivität eines ihrer Geräte (IOS- oder Mac) beginnen, und weiterhin diese gleichen Aktivität auf einem anderen Geräte (wie anhand des Benutzers iClou identifiziert eingeführt. (d Konto).

Übergabe wurde in den iOS 9, um neue, unterstützen auch erweiterte Suchfunktionen erweitert. Weitere Informationen finden Sie unserem [Verbesserungen bei der Suche](~/ios/platform/search/index.md) Dokumentation.

Der Benutzer kann z. B. starten eine e-Mail an eigene iPhones und nahtlos fortgesetzt werden die e-Mail-Adresse auf ihren Mac mit der gleichen Nachrichteninformationen aufgefüllt wurden und der Cursor am gleichen Speicherort, der sie es in iOS beibehalten.

Eine der apps, die denselben _Team-ID_ sind berechtigt, für die Übergabe verwenden, um Benutzeraktivitäten über apps hinweg fortzusetzen, solange diese app sind entweder über den iTunes App Store übermittelt oder durch ein registrierter Entwickler signierte (für Mac Enterprise oder Ad-hoc-apps).

Alle `NSDocument` oder `UIDocument` basierend apps haben, automatisch Handoff integrierte unterstützen und erfordert nur minimale Änderungen zur Unterstützung der Übergabe.

### <a name="continuing-user-activities"></a>Fortsetzen von Benutzeraktivitäten

Die `NSUserActivity` Klasse (sowie einige kleine Änderungen an `UIKit` und `AppKit`) bietet Unterstützung für das Definieren eines Benutzers-Aktivität, die möglicherweise weiterhin ausgeführt werden kann, auf einem der anderen Geräte des Benutzers.

Eine Aktivität an einem der anderen Geräte des Benutzers übergeben werden, müssen sie in einer Instanz gekapselt werden `NSUserActivity`, außer die _aktuelle Aktivität_, dessen Nutzlast festgelegt (die Daten verwendet, um die Fortsetzung auszuführen) und die Aktivität muss dann für das Gerät übertragen werden.

Übergabe übergibt das absolute Minimum der Informationen, die definieren, der Aktivität, die fortgesetzt werden, mit größeren Datenpakete, die über die iCloud synchronisiert werden.

Auf dem empfangenden Gerät wird der Benutzer eine Benachrichtigung, dass eine Aktivität für die Fortsetzung verfügbar ist. Bei Einmaliger Betätigung die Aktivität auf das neue Gerät fortsetzen, die angegebene app gestartet wird (sofern nicht bereits ausgeführt wird) und die Nutzlast aus der `NSUserActivity` wird verwendet, um die Aktivität neu zu starten.

[![](handoff-images/handoffinteractions.png "Einen Überblick über die Benutzeraktivitäten fortsetzen")](handoff-images/handoffinteractions.png#lightbox)

Nur apps, die Freigabe des gleichen Entwicklers Team-ID von und reagieren auf einen bestimmten _Aktivitätstyp_ kommen für eine Fortsetzung. Eine app definiert die Aktivitätstypen, die sie unter unterstützt die `NSUserActivityTypes` Schlüssel, der die **"Info.plist"** Datei. Deshalb wählt ein Gerät fortsetzen die app, führen Sie die Fortsetzung, die basierend auf der Team-ID, Aktivitätstyp und optional die _Aktivität Titel_.

Die empfangende app mithilfe von Informationen aus der `NSUserActivity`des `UserInfo` Wörterbuch, das Konfigurieren der Benutzeroberfläche und den Status der angegebenen Aktivität wiederherstellen, sodass der Übergang nahtlos für den Endbenutzer angezeigt wird.

Wenn die Fortsetzung mehr Informationen als effizient über gesendet werden kann, erfordert eine `NSUserActivity`, das Fortsetzen der app senden einen Aufruf auf die ursprüngliche Anwendung und herstellen eine oder mehrere Datenströme, um die erforderlichen Daten zu übertragen. Z. B. wenn die Aktivität ein großer Text-Dokument mit mehreren Abbildern bearbeitet wurde, ist streaming erforderlich, um die erforderlichen Informationen zum Fortsetzen der Aktivitäts auf dem empfangenden Gerät übertragen. Weitere Informationen finden Sie unter den [unterstützen Fortsetzung Streams](#supporting-continuation-streams) Abschnitt weiter unten.

Wie bereits erwähnt, `NSDocument` oder `UIDocument` basierend apps haben, automatisch übergeben, die integrierte Unterstützung. Weitere Informationen finden Sie unter den [unterstützen Übergabe in Dokument-basierten Apps](#supporting-handoff-in-document-based-apps) Abschnitt weiter unten.

### <a name="the-nsuseractivity-class"></a>Die NSUserActivity-Klasse

Die `NSUserActivity` -Klasse ist das primäre Objekt in einer Exchange Übergabeanimationen und wird verwendet, um den Status der Aktivität eines Benutzers zu kapseln, die für die Fortsetzung verfügbar ist. Instanziieren Sie eine app wird eine Kopie des `NSUserActivity` für jede Aktivität unterstützt und auf einem anderen Gerät fortsetzen möchte. Dokument-Editor wird eine Aktivität für jedes Dokument z. B. als aktuell geöffneten erstellen. Nur der vordersten-Dokument (angezeigt in der vordersten Fenster oder der Registerkarte ") ist jedoch die _aktuelle Aktivität_ und deshalb für die Fortsetzung verfügbar.

Eine Instanz von `NSUserActivity` wird von beiden identifiziert die `ActivityType` und `Title` Eigenschaften. Die `UserInfo` Eigenschaft "Dictionary" wird verwendet, um Informationen über den Status der Aktivität enthalten. Festlegen der `NeedsSave` Eigenschaft, um `true` Wunsch zu verzögerten Laden Sie die Statusinformationen über die `NSUserActivity`des Delegaten. Verwenden der `AddUserInfoEntries` Methode zum Zusammenführen von neuer Daten von anderen Clients in der `UserInfo` Wörterbuch erforderlich, um die Aktivität den Status beizubehalten.

### <a name="the-nsuseractivitydelegate-class"></a>Die NSUserActivityDelegate-Klasse

Die `NSUserActivityDelegate` wird verwendet, um Informationen zu halten, einem `NSUserActivity`des `UserInfo` Wörterbuch auf dem neuesten Stand und synchronisiert mit dem aktuellen Status der Aktivität. Wenn das System benötigt die Informationen in der Aktivität aktualisiert werden (z. B. vor der Fortsetzung, die auf einem anderen Gerät), ruft der `UserActivityWillSave` Methode des Delegaten.

Sie benötigen zum Implementieren der `UserActivityWillSave` -Methode, und nehmen Sie alle Änderungen an der `NSUserActivity` (wie z. B. `UserInfo`, `Title`usw.) um sicherzustellen, dass es immer noch den Status der aktuellen Aktivität reflektiert. Wenn das System Ruft die `UserActivityWillSave` -Methode, die `NeedsSave` Flag wird gelöscht. Wenn Sie die Eigenschaften der Daten der Aktivität ändern, müssen Sie festlegen `NeedsSave` zu `true` erneut aus.

Anstatt die `UserActivityWillSave` Methode, die oben aufgeführten, Sie können optional haben `UIKit` oder `AppKit` automatisch, die Aktivitäten von Benutzern zu verwalten. Zu diesem Zweck legen Sie den Beantworter des Objekts `UserActivity` -Eigenschaft und Implementieren der `UpdateUserActivityState` Methode. Finden Sie unter den [unterstützen Übergabe in Responder](#supporting-handoff-in-responders) Informationen weiter unten im Abschnitt.

### <a name="app-framework-support"></a>App-Framework-Unterstützung

Beide `UIKit` (iOS) und `AppKit` (OS X) bieten integrierte Unterstützung für die Übergabe in die `NSDocument`, Responder (`UIResponder`/`NSResponder`), und `AppDelegate` Klassen. Während jedes Betriebssystem Handoff etwas anders implementiert, sind der grundlegende Mechanismus und die APIs identisch.

#### <a name="user-activities-in-document-based-apps"></a>Benutzeraktivitäten in Dokument-basierten Apps

Dokumentbasierte IOS- und OS X-apps haben automatisch integrierte Handoff-Unterstützung. Um diese Unterstützung zu aktivieren, müssen Sie zum Hinzufügen einer `NSUbiquitousDocumentUserActivityType` Schlüssel und Wert für die einzelnen `CFBundleDocumentTypes` Eintrag in der app **"Info.plist"** Datei.

Wenn dieser Schlüssel vorhanden ist, ist sowohl `NSDocument` und `UIDocument` automatisch `NSUserActivity` -Instanzen für die iCloud-basierten Dokumenten des angegebenen Typs. Sie müssen einen Aktivitätstyp für jeden Typ von Dokument bereitgestellt wird, die app unterstützt, und mehrere Dokumenttypen können den gleichen Aktivitätstyp verwenden. Beide `NSDocument` und `UIDocument` automatisch Auffüllen der `UserInfo` Eigenschaft der `NSUserActivity` mit ihre `FileURL` Eigenschaftswert.

Auf OS X die `NSUserActivity` von verwalteten `AppKit` und Responder automatisch zugeordnet werden Sie die aktuelle Aktivität aus, wenn das Fenster des Dokuments im Hauptfenster wird. Unter iOS für `NSUserActivity` vom verwalteten Objekte `UIKit`, müssen Sie entweder Aufruf `BecomeCurrent` Methode explizit oder des Dokuments `UserActivity` -Eigenschaftensatz am ein `UIViewController` Wenn die app stammt, in den Vordergrund gestellt.

`AppKit` wird automatisch wiederhergestellt, alle `UserActivity` Eigenschaft, die auf diese Weise unter OS X erstellt. Dies geschieht, wenn die `ContinueUserActivity` Methodenrückgabe `false` oder nicht implementiert ist. In diesem Fall das Dokument geöffnet ist, mit der `OpenDocument` Methode der `NSDocumentController` und sie erhalten dann eine `RestoreUserActivityState` Methodenaufruf.

Finden Sie unter den [unterstützen Übergabe in Dokument-basierten Apps](#supporting-handoff-in-document-based-apps) Informationen weiter unten im Abschnitt.

#### <a name="user-activities-and-responders"></a>Benutzeraktivitäten und Responder

Beide `UIKit` und `AppKit` kann automatisch eine Benutzeraktivität verwalten, wenn Sie sie als Beantworter Objekt festlegen `UserActivity` Eigenschaft. Wenn der Zustand geändert wurde, müssen Sie festlegen der `NeedsSave` Eigenschaft für des beantworters des `UserActivity` zu `true`. Das System automatisch speichert die `UserActivity` bei Bedarf die Zeit der Beantworter mit dem Status durch den Aufruf aktualisiert joshi, dessen `UpdateUserActivityState` Methode.

Wenn mehrere Responder einer einzelnen Freigabe `NSUserActivity` -Instanz, die sie erhalten eine `UpdateUserActivityState` Rückruf, wenn das System das Benutzerobjekt der Aktivität aktualisiert. Aufrufen der Antwortdienst muss die `AddUserInfoEntries` -Methode zum Aktualisieren der `NSUserActivity`des `UserInfo` Wörterbuch an diesem Punkt entsprechend den aktuellen Aktivitätsstatus. Die `UserInfo` Wörterbuch deaktiviert ist, vor jedem `UpdateUserActivityState` aufrufen.

Wenn sich aus einer Aktivität trennen möchten, kann ein Antwortdienst Festlegen der `UserActivity` Eigenschaft `null`. Wenn ein app-Framework verwaltet `NSUserActivity` Instanz verfügt über keine weitere zugeordneten Responder oder Dokumente, die automatisch für ungültig erklärt werden kann.

Finden Sie unter den [unterstützen Übergabe in Responder](#supporting-handoff-in-responders) Informationen weiter unten im Abschnitt.

#### <a name="user-activities-and-the-appdelegate"></a>Benutzeraktivitäten und appdelegate-Element

Ihrer app `AppDelegate` ist der primäre Einstiegspunkt, bei der Verarbeitung einer Fortsetzung übergeben. Wenn der Benutzer reagiert auf eine Übergabe-Benachrichtigung, die entsprechenden app gestartet wird (sofern nicht bereits ausgeführt wird) und die `WillContinueUserActivityWithType` Methode der `AppDelegate` aufgerufen wird. An diesem Punkt sollte die app den Benutzer zu informieren, den die Fortsetzung gestartet wird.

Die `NSUserActivity` Instanz wird bereitgestellt, wenn die `AppDelegate`des `ContinueUserActivity` Methode wird aufgerufen. An diesem Punkt sollte die app Benutzeroberfläche konfigurieren und weiterhin die angegebene Aktivität.

Finden Sie unter den [implementieren Handoff](#implementing-handoff) Informationen weiter unten im Abschnitt.

## <a name="enabling-handoff-in-a-xamarin-app"></a>Aktivieren die Übergabe in einer Xamarin-App

Aufgrund der sicherheitsanforderungen durch Übergabe auferlegt wird muss eine Xamarin.iOS-app, die Übergabe-Framework verwendet, ordnungsgemäß in das Apple Developer Portal und in der Xamarin.iOS-Projektdatei konfiguriert werden.

Führen Sie folgende Schritte aus:

1. Melden Sie sich bei der [Apple-Entwicklerportal](https://developer.apple.com).
2. Klicken Sie auf **Zertifikate, Bezeichner & Profile**.
3. Wenn Sie dies noch nicht getan haben, klicken Sie auf **Bezeichner** und erstellen Sie eine ID für Ihre app (z. B. `com.company.appname`), andernfalls bearbeiten Sie Ihre vorhandenen-ID.
4. Sicherstellen, dass die **iCloud** Dienst für die angegebene ID überprüft wurden:

    [![](handoff-images/provision01.png "Aktivieren Sie den iCloud-Dienst für die angegebene ID")](handoff-images/provision01.png#lightbox)
5. Speichern Sie die Änderungen.
6. Klicken Sie auf **Bereitstellungsprofile** > **Entwicklung** und erstellen Sie ein Bereitstellungsprofil für Sie Neuentwicklungen-app:

    [![](handoff-images/provision02.png "Erstellen Sie ein neues entwicklungsbereitstellungsprofil für die app")](handoff-images/provision02.png#lightbox)
7. Herunterladen Sie und installieren Sie das neue Bereitstellungsprofil oder verwenden Sie Xcode herunterladen und installieren das Profil.
8. Bearbeiten Sie die Optionen der Xamarin.iOS-Projekt, und stellen Sie sicher, dass Sie das Bereitstellungsprofil verwenden, das Sie gerade erstellt haben:

    [![](handoff-images/provision03.png "Wählen Sie das soeben erstellte Bereitstellungsprofil")](handoff-images/provision03.png#lightbox)
9. Als Nächstes bearbeiten Ihrer **"Info.plist"** Datei, und stellen Sie sicher, dass Sie die App-ID verwenden, die verwendet wurde, um das Bereitstellungsprofil zu erstellen:

    [![](handoff-images/provision04.png "Legen Sie die App-ID")](handoff-images/provision04.png#lightbox)
10. Scrollen Sie zu der **Background Modes** Abschnitt, und überprüfen Sie die folgenden Elemente:

    [![](handoff-images/provision05.png "Die erforderliche hintergrundmodi aktivieren")](handoff-images/provision05.png#lightbox)
11. Speichern Sie die Änderungen auf alle Dateien an.

Mit diesen Einstellungen vorhanden ist die Anwendung nun bereit für die Übergabe Framework-APIs zugreifen. Ausführliche Informationen zur Bereitstellung finden Sie unter unseren [Gerätebereitstellung](~/ios/get-started/installation/device-provisioning/index.md) und [Bereitstellung Ihrer App](~/ios/get-started/installation/device-provisioning/index.md) Anleitungen.

## <a name="implementing-handoff"></a>Implementieren von Übergabeanimationen

Benutzeraktivitäten können mehrere apps fortgesetzt werden, die mit der Entwickler Team-ID angemeldet sind und der gleichen Aktivitätstyp zu unterstützen. Übergabe in einer Xamarin.iOS-app zu implementieren, müssen Sie Sie ein Benutzerobjekt für die Aktivität zu erstellen (entweder im `UIKit` oder `AppKit`), aktualisieren Sie den Zustand des Objekts zum Nachverfolgen der Aktivität und fortsetzen die Aktivität auf einem Gerät empfangen.

### <a name="identifying-user-activities"></a>Identifizieren von Benutzeraktivitäten

Der erste Schritt bei der Implementierung von Übergabeanimationen zum Identifizieren von Benutzeraktivitäten, die Ihrer app unterstützt wird, und wird angezeigt, der diese Aktivitäten sind gute Kandidaten für die Fortsetzung auf einem anderen Gerät. Zum Beispiel: eine ToDo-app möglicherweise unterstützt Bearbeitung Elemente als eine _Aktivitätstyp Benutzer_, und Durchsuchen die Liste der verfügbaren Elemente wie eine andere zu unterstützen.

Eine app kann beliebig viele Benutzer Aktivitätstypen wie erforderlich sind, eine für jede Funktion erstellen, die die app bereitstellt. Für jeden Aktivitätstyp von Benutzer benötigt die app zu überwachen, wann eine Aktivität des Typs beginnt und endet, und muss über aktuelle Zustandsinformationen, um diese Aufgabe auf einem anderen Gerät weiterhin verwalten.

Benutzeraktivitäten können auf jede app, die mit der gleichen Team-ID ohne 1: 1-Zuordnung zwischen den sendenden und empfangenden apps signiert fortgesetzt werden. Beispielsweise kann eine bestimmte app vier verschiedene Typen von Aktivitäten, erstellen, die von anderen, einzelne apps auf einem anderen Gerät verwendet werden. Dies ist häufig auftreten zwischen einem Mac-Version der app (die zahlreichen Features und Funktionen möglicherweise) und iOS-apps bei jeder app kleiner und konzentriert sich auf eine bestimmte Aufgabe.

### <a name="creating-activity-type-identifiers"></a>Aktivität-Typ-IDs erstellen

Die _Typbezeichner der Aktivität_ wird eine kurze Zeichenfolge hinzugefügt die `NSUserActivityTypes` Array von der app **"Info.plist"** Datei, die zur eindeutigen Identifizierung eines bestimmten Typs des Benutzer-Aktivität verwendet. Es werden ein Eintrag im Array für jede Aktivität, die die app unterstützt. Apple empfiehlt eine Notation Reverse-DNS-Format für den Typ des Bezeichners der Aktivität verwenden, um Konflikte zu vermeiden. Zum Beispiel: `com.company-name.appname.activity` für bestimmte app-basierte Aktivitäten oder `com.company-name.activity` für Aktivitäten, die für mehrere apps ausgeführt werden können.

Typ des Bezeichners der Aktivität wird verwendet, beim Erstellen einer `NSUserActivity` Instanz, um den Typ der Aktivität zu identifizieren. Wenn eine Aktivität auf einem anderen Gerät fortgesetzt wird, bestimmt der Aktivitätstyp (zusammen mit der app-Team-ID) der app zu starten, um die Aktivität den Vorgang fortzusetzen.

Beispielsweise werden wir zum Erstellen einer Beispielapp aufgerufen **MonkeyBrowser** ([hier herunterladen](https://developer.xamarin.com/samples/monotouch/ios8/MonkeyBrowser/)). Diese app stellt vier Registerkarten, von denen jede eine andere URL geöffnet, in einer Web-Browser-Ansicht dar. Der Benutzer wird weiterhin eine beliebige Registerkarte kann auf ein iOS-Gerät, das Ausführen der app sein.

Um die erforderliche Aktivität Typ-IDs zur Unterstützung dieses Verhaltens zu erstellen, bearbeiten die **"Info.plist"** Datei, und wechseln Sie zu der **Quelle** anzeigen. Hinzufügen einer `NSUserActivityTypes` gedrückt, und erstellen Sie die folgenden Bezeichner:

[![](handoff-images/type01.png "Die NSUserActivityTypes Schlüssel und die erforderlichen-IDs in der Plist-editor")](handoff-images/type01.png#lightbox)

Vier neue Aktivität Typ-IDs, eine für jede der Registerkarten im Beispiel erstellten **MonkeyBrowser** app. Wenn Sie Ihre eigenen apps zu erstellen, ersetzen Sie den Inhalt von der `NSUserActivityTypes` array mit der Aktivität-Typ-IDs für die Aktivitäten Ihrer app unterstützt.

### <a name="tracking-user-activity-changes"></a>Nachverfolgen von Änderungen für Benutzer-Aktivität

Wenn wir erstellen eine neue Instanz der der `NSUserActivity` -Klasse geben wir eine `NSUserActivityDelegate` Instanz zum Nachverfolgen von Änderungen in der Aktivität Zustand. Beispielsweise kann der folgende Code verwendet werden, um Änderungen am Ansichtszustand nachzuverfolgen:

```csharp
using System;
using CoreGraphics;
using Foundation;
using UIKit;

namespace MonkeyBrowse
{
    public class UserActivityDelegate : NSUserActivityDelegate
    {
        #region Constructors
        public UserActivityDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void UserActivityReceivedData (NSUserActivity userActivity, NSInputStream inputStream, NSOutputStream outputStream)
        {
            // Log
            Console.WriteLine ("User Activity Received Data: {0}", userActivity.Title);
        }

        public override void UserActivityWasContinued (NSUserActivity userActivity)
        {
            Console.WriteLine ("User Activity Was Continued: {0}", userActivity.Title);
        }

        public override void UserActivityWillSave (NSUserActivity userActivity)
        {
            Console.WriteLine ("User Activity will be Saved: {0}", userActivity.Title);
        }
        #endregion
    }
}
```

Die `UserActivityReceivedData` Methode wird aufgerufen, wenn eine Fortsetzung Stream Daten von einem sendenden Gerät empfangen hat. Weitere Informationen finden Sie unter den [unterstützen Fortsetzung Streams](#supporting-continuation-streams) Abschnitt weiter unten.

Die `UserActivityWasContinued` Methode wird aufgerufen, wenn ein anderes Gerät über eine Aktivität aus dem aktuellen Gerät stattgefunden hat. Je nach Art der Aktivität, wie das Hinzufügen eines neuen Elements zu einer ToDo-Liste kann die app die Aktivität auf dem sendenden Gerät abbrechen müssen.

Die `UserActivityWillSave` Methode wird aufgerufen, bevor Änderungen an der Aktivität gespeichert und lokal verfügbare geräteübergreifend synchronisiert werden. Können Sie diese Methode alle letzten Minute Änderungen an der `UserInfo` Eigenschaft der `NSUserActivity` Instanz, bevor sie gesendet wird.

### <a name="creating-a-nsuseractivity-instance"></a>Erstellen einer Instanz NSUserActivity

Jede Aktivität, die Ihre app die Möglichkeit, weiterhin auf einem anderen Gerät bereitstellen möchte gekapselt werden muss, einem `NSUserActivity` Instanz. Erstellen Sie die app kann beliebig viele Aktivitäten nach Bedarf, und die Art dieser Aktivitäten ist abhängig von der Funktionalität und Merkmale die betreffende Anwendung. Beispielsweise kann eine e-Mail-app eine Aktivität zum Erstellen einer neuen Nachricht und eine andere zum Lesen einer Nachricht erstellen.

Für die Beispiel-app eine neue `NSUserActivity` erstellt jedes Mal, wenn der Benutzer eine neue URL eines der im Registerkartenformat Webbrowseransicht eingibt. Der folgende Code speichert den Zustand einer angegebene Registerkarte:

```csharp
public NSString UserActivityTab1 = new NSString ("com.xamarin.monkeybrowser.tab1");
public NSUserActivity UserActivity { get; set; }
...

UserActivity = new NSUserActivity (UserActivityTab1);
UserActivity.Title = "Weather Tab";
UserActivity.Delegate = new UserActivityDelegate ();

// Update the activity when the tab's URL changes
var userInfo = new NSMutableDictionary ();
userInfo.Add (new NSString ("Url"), new NSString (url));
UserActivity.AddUserInfoEntries (userInfo);

// Inform Activity that it has been updated
UserActivity.BecomeCurrent ();
```

Er erstellt ein neues `NSUserActivity` Verwenden des Aktivitätstyps Benutzer oben erstellt haben, und gibt einen lesbaren Titel für die Aktivität. Es fügt zu einer Instanz von der `NSUserActivityDelegate` oben, sehen Sie sich für Zustand ändert, und informiert iOS, dass diese Benutzer-Aktivität der aktuellen Aktivität wird erstellt.

### <a name="populating-the-userinfo-dictionary"></a>Das Wörterbuch UserInfo Auffüllen

Wie wir oben gesehen haben die `UserInfo` Eigenschaft der `NSUserActivity` -Klasse ist eine `NSDictionary` von Schlüssel-Wert-Paaren verwendet, um den Status einer bestimmten Aktivität zu definieren. Die in gespeicherten Werte `UserInfo` muss einer der folgenden Typen sein: `NSArray`, `NSData`, `NSDate`, `NSDictionary`, `NSNull`, `NSNumber`, `NSSet`, `NSString`, oder `NSURL`. `NSURL` Datenwerte, die auf iCloud-Dokumente verweisen werden automatisch angepasst, damit sie auf den gleichen Dokumenten auf einem empfangenden Gerät verweisen.

Im obigen Beispiel, erstellt es eine `NSMutableDictionary` Objekt aus, und füllen Sie ihn mit einem einzigen Schlüssel mit der Angabe der URL, die der Benutzer gerade auf die angegebene Registerkarte anzeigt wurde. Die `AddUserInfoEntries` Methode, die Aktivitäten des Benutzers wurde verwendet, um die Aktivität mit den Daten zu aktualisieren, die zum Wiederherstellen der Aktivität auf dem empfangenden Gerät verwendet werden:

```csharp
// Update the activity when the tab's URL changes
var userInfo = new NSMutableDictionary ();
userInfo.Add (new NSString ("Url"), new NSString (url));
UserActivity.AddUserInfoEntries (userInfo);
```

Apple empfiehlt, halten die Informationen, die an die minimal notwendige gesendet, um sicherzustellen, dass die Aktivität rechtzeitig auf dem empfangenden Gerät gesendet wird. Wenn mehr Informationen erforderlich ist, wie ein Bild an ein Dokument angefügt bearbeitet werden muss gesendet werden soll, sollten Sie die Fortsetzung Datenströme verwenden. Finden Sie unter den [unterstützen Fortsetzung Streams](#supporting-continuation-streams) Informationen weiter unten im Abschnitt.

### <a name="continuing-an-activity"></a>Fortsetzen einer Aktivitäts

Übergabe informiert automatisch lokale IOS- und OS X-Geräte, die aufgrund der physischen Nähe auf dem sendenden Gerät und bei demselben iCloud-Konto, über die Verfügbarkeit von vernachlässigbar Benutzeraktivitäten angemeldet. Wenn der Benutzer auswählt, um eine Aktivität auf einem neuen Gerät den Vorgang fortzusetzen, startet das System die entsprechende app (basierend auf den Team-ID und die Art der Aktivität) und die Informationen der `AppDelegate` , dass die Fortsetzung ausgeführt werden soll.

Zunächst wird die `WillContinueUserActivityWithType` Methode wird aufgerufen, damit die app den Benutzer darüber informieren kann, die die Fortsetzung begonnen wird. Wir verwenden den folgenden Code in die **Datei "appdelegate.cs"** Datei unserer Beispiel-App, eine Fortsetzung ab behandeln:

```csharp
public NSString UserActivityTab1 = new NSString ("com.xamarin.monkeybrowser.tab1");
public NSString UserActivityTab2 = new NSString ("com.xamarin.monkeybrowser.tab2");
public NSString UserActivityTab3 = new NSString ("com.xamarin.monkeybrowser.tab3");
public NSString UserActivityTab4 = new NSString ("com.xamarin.monkeybrowser.tab4");
...

public FirstViewController Tab1 { get; set; }
public SecondViewController Tab2 { get; set;}
public ThirdViewController Tab3 { get; set; }
public FourthViewController Tab4 { get; set; }
...

public override bool WillContinueUserActivity (UIApplication application, string userActivityType)
{
    // Report Activity
    Console.WriteLine ("Will Continue Activity: {0}", userActivityType);

    // Take action based on the user activity type
    switch (userActivityType) {
    case "com.xamarin.monkeybrowser.tab1":
        // Inform view that it's going to be modified
        Tab1.PreparingToHandoff ();
        break;
    case "com.xamarin.monkeybrowser.tab2":
        // Inform view that it's going to be modified
        Tab2.PreparingToHandoff ();
        break;
    case "com.xamarin.monkeybrowser.tab3":
        // Inform view that it's going to be modified
        Tab3.PreparingToHandoff ();
        break;
    case "com.xamarin.monkeybrowser.tab4":
        // Inform view that it's going to be modified
        Tab4.PreparingToHandoff ();
        break;
    }

    // Inform system we handled this
    return true;
}
```

Im obigen Beispiel registriert sich jedes View Controller mit dem `AppDelegate` und verfügt über eine öffentliche `PreparingToHandoff` Methode, die anzeigt, ein Indikator für die Aktivität und eine Meldung, die den Benutzer darüber informiert, dass die Aktivität wird an das aktuelle Gerät übergeben werden. Beispiel:

```csharp
private void ShowBusy(string reason) {

    // Display reason
    BusyText.Text = reason;

    //Define Animation
    UIView.BeginAnimations("Show");
    UIView.SetAnimationDuration(1.0f);

    Handoff.Alpha = 0.5f;

    //Execute Animation
    UIView.CommitAnimations();
}
...

public void PreparingToHandoff() {
    // Inform caller
    ShowBusy ("Continuing Activity...");
}
```

Die `ContinueUserActivity` von der `AppDelegate` wird aufgerufen, um die angegebene Aktivität tatsächlich weiterhin. In diesem Fall aus unserem Beispiel-app:

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Report Activity
    Console.WriteLine ("Continuing User Activity: {0}", userActivity.ToString());

    // Get input and output streams from the Activity
    userActivity.GetContinuationStreams ((NSInputStream arg1, NSOutputStream arg2, NSError arg3) => {
        // Send required data via the streams
        // ...
    });

    // Take action based on the Activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.monkeybrowser.tab1":
        // Preform handoff
        Tab1.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab1});
        break;
    case "com.xamarin.monkeybrowser.tab2":
        // Preform handoff
        Tab2.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab2});
        break;
    case "com.xamarin.monkeybrowser.tab3":
        // Preform handoff
        Tab3.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab3});
        break;
    case "com.xamarin.monkeybrowser.tab4":
        // Preform handoff
        Tab4.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab4});
        break;
    }

    // Inform system we handled this
    return true;
}
```

Die öffentliche `PerformHandoff` jedes View Controller tatsächlich führt die Übergabe und die Aktivität auf dem aktuellen Gerät wiederhergestellt. Bei diesem Beispiel wird die gleiche URL in einer angegebenen Registerkarte, die der Benutzer, auf einem anderen Gerät besucht wurde angezeigt. Beispiel:

```csharp
private void HideBusy() {

    //Define Animation
    UIView.BeginAnimations("Hide");
    UIView.SetAnimationDuration(1.0f);

    Handoff.Alpha = 0f;

    //Execute Animation
    UIView.CommitAnimations();
}
...

public void PerformHandoff(NSUserActivity activity) {

    // Hide busy indicator
    HideBusy ();

    // Extract URL from dictionary
    var url = activity.UserInfo ["Url"].ToString ();

    // Display value
    URL.Text = url;

    // Display the give webpage
    WebView.LoadRequest(new NSUrlRequest(NSUrl.FromString(url)));

    // Save activity
    UserActivity = activity;
    UserActivity.BecomeCurrent ();

}
```

Die `ContinueUserActivity` Methode enthält einen `UIApplicationRestorationHandler` , die Sie aufrufen können, für das Dokument oder -Responder-basierten Aktivität fortsetzen. Müssen Sie übergeben eine `NSArray` wiederherstellbaren Objekte, die beim Aufruf der Wiederherstellung-Handler. Beispiel:

```csharp
completionHandler (new NSObject[]{Tab4});
```

Für jedes Objekt übergeben dessen `RestoreUserActivityState` aufgerufene Methode. Jedes Objekt können Sie dann die Daten in die `UserInfo` Wörterbuch, das seinen eigenen Status wiederherstellen. Beispiel:

```csharp
public override void RestoreUserActivityState (NSUserActivity activity)
{
    base.RestoreUserActivityState (activity);

    // Log activity
    Console.WriteLine ("Restoring Activity {0}", activity.Title);
}
```

Für Dokument-basierte apps, wenn Sie nicht implementieren die `ContinueUserActivity` -Methode, oder es gibt `false`, `UIKit` oder `AppKit` können die Aktivität automatisch fortgesetzt. Finden Sie unter den [unterstützen Übergabe in Dokument-basierten Apps](#supporting-handoff-in-document-based-apps) Informationen weiter unten im Abschnitt.

### <a name="failing-handoff-gracefully"></a>Fehler bei Übergabe ordnungsgemäß

Da die Übergabe der Übertragung von Informationen zwischen einer Sammlung lose verbundenen iOS und OS X-Geräte verwendet, Fehler der Übertragungsprozess treten ggf. Entwerfen Sie Ihre app an diese Fehler ordnungsgemäß behandeln und informiert den Benutzer über alle Situationen, die auftreten können.

Bei einem Fehler der `DidFailToContinueUserActivitiy` Methode der `AppDelegate` aufgerufen wird. Beispiel:

```csharp
public override void DidFailToContinueUserActivitiy (UIApplication application, string userActivityType, NSError error)
{
    // Log information about the failure
    Console.WriteLine ("User Activity {0} failed to continue. Error: {1}", userActivityType, error.LocalizedDescription);
}
```

Verwenden Sie die angegebene `NSError` Informationen für den Benutzer zu diesem Fehler bereitstellen.

## <a name="native-app-to-web-browser-handoff"></a>Native Übergabe der Web-Browser-App

Ein Benutzer möchte eine Aktivität fortgesetzt wird, ohne eine entsprechende native app, die auf das gewünschte Gerät installiert. In einigen Fällen eine webbasierte Schnittstelle kann die erforderliche Funktionalität bereitstellen, und die Aktivität trotzdem fortgesetzt werden kann. Beispielsweise kann die e-Mail-Konto des Benutzers eine Basis-Web-Benutzeroberfläche für die erstellen und Lesen von Nachrichten bereitstellen.

Wenn die URL für die ursprüngliche, systemeigene app bekannt ist für die Web-Schnittstelle (und die erforderliche Syntax für die Identifizierung des angegebenen Elements fortgesetzt wird), können sie diese Informationen im Codieren der `WebpageURL` Eigenschaft der `NSUserActivity` Instanz. Wenn empfangende Gerät nicht über eine entsprechende systemeigene app installiert, um die Fortsetzung bewältigen verfügt, kann die bereitgestellten Webschnittstelle aufgerufen werden.

## <a name="web-browser-to-native-app-handoff"></a>Webbrowser, um die Übergabe der nativen App

Wenn der Benutzer wurde eine webbasierte Schnittstelle auf dem sendenden Gerät und eine native app auf dem empfangenden Gerät der Domänenteil des Ansprüche der `WebpageURL` -Eigenschaft, und klicken Sie dann auf das System die app das Handle der Fortsetzung verwendet. Das neue Gerät erhält ein `NSUserActivity` -Instanz, die als Typ markiert `BrowsingWeb` und `WebpageURL` enthält die URL der Benutzer das Unternehmen besuchen wurde, die `UserInfo` Wörterbuch leer sein wird.

Für eine app, bei dieser Art der Übergabe teilzunehmen, müssen sie die Domäne in Anspruch eine `com.apple.developer.associated-domains` Berechtigung mit dem Format `<service>:<fully qualified domain name>` (z. B.: `activity continuation:company.com`).

Wenn die angegebene Domäne entspricht einer `WebpageURL` übergeben den Wert der Eigenschaft lädt eine Liste der genehmigten app IDs von der Website bei dieser Domäne. Geben Sie die Website muss eine Liste der genehmigten-IDs in eine signierte JSON-Datei mit dem Namen **Apple-app-Site-Zuordnung** (z. B. `https://company.com/apple-app-site-association`).

Diese JSON-Datei enthält ein Wörterbuch, das eine Liste von app-IDs in der Form soll `<team identifier>.<bundle identifier>`. Beispiel:

```csharp
{
    "activitycontinuation": {
        "apps": [    "YWBN8XTPBJ.com.company.FirstApp",
            "YWBN8XTPBJ.com.company.SecondApp" ]
    }
}
```

Zum Signieren der JSON-Datei (so, dass sie den richtigen `Content-Type` von `application/pkcs7-mime`), verwenden Sie die **Terminal** app und eine `openssl` Befehl mit einem Zertifikat und Schlüssel, die von einer Zertifizierungsstelle vertrauenswürdig iOS ausgegeben (finden Sie unter [ https://support.apple.com/kb/ht5012 ](https://support.apple.com/kb/ht5012) eine Liste). Beispiel:

```csharp
echo '{"activitycontinuation":{"apps":["YWBN8XTPBJ.com.company.FirstApp",
"YWBN8XTPBJ.com.company.SecondApp"]}}' > json.txt

cat json.txt | openssl smime -sign -inkey company.com.key
-signer company.com.pem
-certfile intermediate.pem
-noattr -nodetach
-outform DER > apple-app-site-association
```

Die `openssl` Befehl gibt eine signierte JSON-Datei, die Sie für Ihre Website in Platzieren der **Apple-app-Site-Zuordnung** URL. Beispiel:

```csharp
https://example.com/apple-app-site-association.
```

Die app erhält jede Aktivität, deren `WebpageURL` Domäne befindet sich in der `com.apple.developer.associated-domains` Berechtigung. Nur die `http` und `https` Protokolle werden unterstützt, ein anderes Protokoll wird eine Ausnahme ausgelöst.

## <a name="supporting-handoff-in-document-based-apps"></a>Unterstützung der Übergabe in Dokument-basierten Apps

Wie bereits erwähnt, unter iOS und OS X, Dokument-basierte apps werden automatisch unterstützen, Übergabe von Dokumenten mit iCloud-basierten app **"Info.plist"** -Datei enthält eine `CFBundleDocumentTypes` -Schlüssel der `NSUbiquitousDocumentUserActivityType`. Beispiel:

```xml
<key>CFBundleDocumentTypes</key>
<array>
    <dict>
        <key>CFBundleTypeName</key>
        <string>NSRTFDPboardType</string>
        . . .
        <key>LSItemContentTypes</key>
        <array>
        <string>com.myCompany.rtfd</string>
        </array>
        . . .
        <key>NSUbiquitousDocumentUserActivityType</key>
        <string>com.myCompany.myEditor.editing</string>
    </dict>
</array>
```

In diesem Beispiel ist die Zeichenfolge ein Reverse-DNS-app-Kennzeichner mit dem Namen der Aktivität angefügt. Wenn auf diese Weise eingegeben haben, die typeinträgen Aktivität müssen nicht wiederholt werden, in der `NSUserActivityTypes` Array mit den **"Info.plist"** Datei.

Das automatisch erstellte Benutzeraktivität-Objekt (des Dokuments erhältlich `UserActivity` Eigenschaft) verwiesen wird, von anderen Objekten in der app und zum Wiederherstellen der Status in der Fortsetzung verwendet werden kann. Z. B. positionieren zum Nachverfolgen von Element Auswahl und das Dokument. Müssen Sie diese Aktivitäten `NeedsSave` Eigenschaft `true` jedes Mal, wenn der Status ändert, und aktualisieren Sie die `UserInfo` Wörterbuch in der `UpdateUserActivityState` Methode.

Die `UserActivity` Eigenschaft kann von jedem Thread verwendet werden, und entspricht das beobachten (KVO)-Protokoll für Schlüssel-Wert, damit sie verwendet werden kann, um ein Dokument synchron halten, wie ein-und der iCloud verschoben wird. Die `UserActivity` Eigenschaft wird ungültig gemacht werden, wenn das Dokument geschlossen wird.

Weitere Informationen finden Sie unter Apple [Aktivität-Unterstützung für Benutzer in Dokument-basierten Apps](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Handoff/HandoffFundamentals/HandoffFundamentals.html#//apple_ref/doc/uid/TP40014338-CH3-SW5) Dokumentation.

## <a name="supporting-handoff-in-responders"></a>Unterstützung von Übergabe in Responder

Können Sie Responder zuordnen (geerbt von entweder `UIResponder` unter iOS oder `NSResponder` unter OS X) Aktivitäten durch Festlegen ihrer `UserActivity` Eigenschaften. Speichert das System automatisch die `UserActivity` Eigenschaft auf den entsprechenden-Mal ab, der Beantworter Aufrufen `UpdateUserActivityState` Methode, um die Benutzeraktivität mit aktuelle Daten hinzuzufügen der `AddUserInfoEntriesFromDictionary` Methode.

## <a name="supporting-continuation-streams"></a>Unterstützung der Fortsetzung Streams

Die Situationen, in denen die Menge an Informationen erforderlich, um eine Aktivität weiterhin nicht effizient die Nutzlast der ersten Übergabe übertragen werden. In diesen Fällen kann die empfangende app ein oder mehrere Streams zwischen sich selbst und die ursprüngliche Anwendung zum Übertragen von Daten herstellen.

Legen Sie die ursprüngliche Anwendung wird die `SupportsContinuationStreams` Eigenschaft der `NSUserActivity` -Instanz, auf `true`. Beispiel:

```csharp
// Create a new user Activity to support this tab
UserActivity = new NSUserActivity (ThisApp.UserActivityTab1){
    Title = "Weather Tab",
    SupportsContinuationStreams = true
};
UserActivity.Delegate = new UserActivityDelegate ();

// Update the activity when the tab's URL changes
var userInfo = new NSMutableDictionary ();
userInfo.Add (new NSString ("Url"), new NSString (url));
UserActivity.AddUserInfoEntries (userInfo);

// Inform Activity that it has been updated
UserActivity.BecomeCurrent ();
```

Die empfangende app kann dann aufrufen, die `GetContinuationStreams` -Methode der der `NSUserActivity` in seine `AppDelegate` zu, um den Stream herzustellen. Beispiel:

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Report Activity
    Console.WriteLine ("Continuing User Activity: {0}", userActivity.ToString());

    // Get input and output streams from the Activity
    userActivity.GetContinuationStreams ((NSInputStream arg1, NSOutputStream arg2, NSError arg3) => {
        // Send required data via the streams
        // ...
    });

    // Take action based on the Activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.monkeybrowser.tab1":
        // Preform handoff
        Tab1.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab1});
        break;
    case "com.xamarin.monkeybrowser.tab2":
        // Preform handoff
        Tab2.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab2});
        break;
    case "com.xamarin.monkeybrowser.tab3":
        // Preform handoff
        Tab3.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab3});
        break;
    case "com.xamarin.monkeybrowser.tab4":
        // Preform handoff
        Tab4.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab4});
        break;
    }

    // Inform system we handled this
    return true;
}
```

Auf dem sendenden Gerät empfängt der Benutzerdelegat der Aktivität die Datenströme durch Aufrufen der `DidReceiveInputStream` Methode, um die Daten bereitzustellen angefordert wird, um die Benutzeraktivität auf dem Gerät fortsetzen den Vorgang fortzusetzen.

Verwenden Sie eine `NSInputStream` zu nur-Lese-Zugriff zum Übertragen von Daten und ein `NSOutputStream` lesegeschützten Zugriff bereitzustellen. In denen die empfangende app mehr Daten anfordert und die ursprüngliche Anwendung bereitgestellt., sollte die Datenströme in einer Weise Anforderungs- und Antwortnachrichten verwendet werden. So, dass Daten, die in den Ausgabestream geschrieben werden, auf dem sendenden Gerät aus dem Eingabestream auf dem Gerät fortsetzen gelesen werden und umgekehrt.

Auch in Situationen, in denen Fortsetzung Stream erforderlich sind, dürfte eine minimale zurück und vor der Kommunikation zwischen den beiden apps.

Weitere Informationen finden Sie auf der Apple [Using Fortsetzung Streams](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Handoff/AdoptingHandoff/AdoptingHandoff.html#//apple_ref/doc/uid/TP40014338-CH2-SW13) Dokumentation.

## <a name="handoff-best-practices"></a>Übergabe bewährte Methoden

Die erfolgreiche Implementierung die nahtlose Fortsetzung eine Benutzeraktivität über Handoff erfordert sorgfältigen Entwurf aufgrund alle der verschiedenen beteiligten Komponenten. Apple empfiehlt, übernehmen die folgenden bewährten Methoden für Ihre apps Handoff aktiviert:

- Entwerfen Sie Ihrer Benutzeraktivitäten müssen die kleinste Nutzlast möglich, beziehen den Status der Aktivität auf fortgesetzt werden. Je größer die Nutzlast, desto länger dauert die Fortsetzung zu starten.
- Wenn Sie große Mengen von Daten für erfolgreiche Fortsetzung übertragen müssen, berücksichtigen Sie Konfiguration und Netzwerkaufwand verringert die Kosten beteiligt.
- Es kommt häufig bei einer großen Mac-app, um Benutzeraktivitäten zu erstellen, die von mehreren, kleiner, aufgabenspezifische apps auf iOS-Geräten verarbeitet werden. Die andere app und die Versionen des Betriebssystems sollten so entworfen werden, zu gut zusammen funktionieren ordnungsgemäß.
- Wenn Sie die Typen der Aktivität angeben, verwenden Sie Reverse-DNS-Notation, um Konflikte zu vermeiden. Wenn eine Aktivität für eine bestimmte app spezifisch ist, der Namen in der Typdefinition enthalten sein (z. B. `com.myCompany.myEditor.editing`). Wenn die Aktivität über mehrere apps arbeiten kann, legen Sie den Namen der app aus der Definition (z. B. `com.myCompany.editing`).
- Wenn Ihre app benötigt, um den Status der Aktivität eines Benutzers zu aktualisieren (`NSUserActivity`) legen Sie die `NeedsSave` Eigenschaft `true`. Zu geeigneten Zeitpunkten und Übergabe des Delegaten aufrufen wird `UserActivityWillSave` Methode, sodass Sie aktualisieren können, die `UserInfo` Wörterbuch nach Bedarf.
- Da es sich bei der Übergabe-Prozess nicht sofort auf dem empfangenden Gerät initialisieren kann, sollten Sie implementieren die `AppDelegate`des `WillContinueUserActivity` und informiert den Benutzer, die eine Fortsetzung zu starten.

## <a name="example-handoff-app"></a>Übergabe-Beispiel-App

Als ein Beispiel für die Übergabe in einer Xamarin.iOS-app verwenden, haben wir enthalten die [ **MonkeyBrowser** ](https://developer.xamarin.com/samples/monotouch/ios8/MonkeyBrowser/) Beispiel-app mit diesem Handbuch. Die app verfügt über vier Registerkarten, mit denen der Benutzer im Web, jeweils mit einer bestimmten Aktivitätstyp zu durchsuchen: Weather "," Favoriten "," Kaffeepause "und" Arbeit ".

Auf eine beliebige Registerkarte, wenn der Benutzer, eine neue URL und Taps eingibt der **wechseln** eine neue Schaltfläche `NSUserActivity` für diese Registerkarte, die die URL enthält, die der Benutzer zurzeit navigiert wird erstellt:

[![](handoff-images/handoff01.png "Übergabe-Beispiel-App")](handoff-images/handoff01.png#lightbox)

Wenn einem der anderen Geräte des Benutzers verfügt das **MonkeyBrowser** -app installiert, iCloud, die mit dem gleichen Benutzerkonto angemeldet ist, wird auf dem gleichen Netzwerk und in unmittelbarer Nähe ausrichten, mit dem oben genannten Gerät, die Übergabe Aktivität unter "Start" angezeigt der Bildschirm (in der unteren linken Ecke):

[![](handoff-images/handoff02.png "Die Übergabe-Aktivität, die auf dem Startbildschirm in der unteren linken Ecke angezeigt")](handoff-images/handoff02.png#lightbox)

Wenn der Benutzer nach oben auf das Symbol für die Übergabe zieht, die app wird gestartet, und die Aktivitäten von Benutzern angegeben, der `NSUserActivity` wird auf dem neuen Gerät fortgesetzt werden:

[![](handoff-images/handoff03.png "Die Benutzer-Aktivität fortgesetzt wird, auf dem neuen Gerät")](handoff-images/handoff03.png#lightbox)

Wenn die Benutzeraktivität erfolgreich gesendet wurde auf einem anderen Apple-Gerät, dem sendenden Gerät die `NSUserActivity` erhalten einen Anruf an die `UserActivityWasContinued` Methode für die `NSUserActivityDelegate` , damit es weiß, dass die Aktivitäten von Benutzern zu einem anderen erfolgreich übertragen wurden das Gerät.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel erhält eine Einführung in die Übergabe-Framework verwendet, um eine Benutzeraktivität zwischen mehreren von Apple-Geräten des Benutzers, den Vorgang fortzusetzen. Als Nächstes wird aufgezeigt, wie Sie aktivieren und die Übergabe in einer Xamarin.iOS-app zu implementieren. Abschließend erläutert er die verschiedenen Typen von Übergabeanimationen Fortsetzungen verfügbar und die bewährten Methoden der Übergabe an.



## <a name="related-links"></a>Verwandte Links

- [iOS 9-Beispiele](https://developer.xamarin.com/samples/ios/iOS9/)
- [MonkeyBrowser-Beispiel](https://developer.xamarin.com/samples/monotouch/ios8/MonkeyBrowser/)
- [iOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [Was ist neu in iOS 9.0 verfügen](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [HomeKitDeveloper Guide](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html)
- [HomeKit Richtlinien zur Benutzeroberfläche](https://developer.apple.com/homekit/ui-guidelines/)
- [Referenz zu HomeKit-Framework](https://developer.apple.com/library/ios/home_kit_framework_ref)
