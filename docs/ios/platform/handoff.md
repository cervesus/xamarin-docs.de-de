---
title: "Übergabe"
description: "In diesem Artikel deckt arbeiten mit Übergabe in einem Xamarin.iOS-app zu übertragenden Benutzeraktivitäten zwischen apps, die für den Benutzer ausgeführt's anderer Geräte."
ms.topic: article
ms.prod: xamarin
ms.assetid: 405F966A-4085-4621-AA15-33D663AD15CD
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 25220f37433037b55f13c4de5a07c0c09173a269
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="handoff"></a>Übergabe

_In diesem Artikel deckt arbeiten mit Übergabe in einem Xamarin.iOS-app zu übertragenden Benutzeraktivitäten zwischen apps, die für den Benutzer ausgeführt's anderer Geräte._

Apple Übergabe in iOS 8 und OS X Yosemite (10.10), einen gemeinsamen Mechanismus für den Benutzer zum Übertragen von Aktivitäten, die Schritte auf einem ihrer Geräte bereitzustellen, auf ein anderes Gerät mit derselben app oder eine andere Anwendung mit der gleichen Aktivität eingeführt.

[![](handoff-images/handoff02.png "Ein Beispiel für eine Übergabe-Operation")](handoff-images/handoff02.png#lightbox)

In diesem Artikel wird nutzen einen kurzen Blick auf die Aktivität, die in einer app Xamarin.iOS Freigabe aktivieren und die Übergabe Framework im Detail behandelt:

## <a name="about-handoff"></a>Zur Übergabe

Übergabe (auch bekannt als Geschäftskontinuität) wurde von Apple in iOS 8 und OS X Yosemite (10.10) als eine Möglichkeit für den Benutzer zu eine Aktivität eines ihrer Geräte (IOS- oder Mac) beginnen und den Vorgang fortzusetzen, derselben Aktivität auf einem anderen Gerät (wie durch den Benutzer iClou identifiziert eingeführt. d Konto).

Übergabe wurde in den iOS 9 auch neue, unterstützt verbesserte Suchfunktionen erweitert. Weitere Informationen finden Sie unter unsere [Suche Erweiterungen](~/ios/platform/search/index.md) Dokumentation.

Der Benutzer kann z. B. starten eine e-Mail-Nachricht auf ihrer iPhone und nahtlos fortgesetzt werden die e-Mail-Adresse auf ihren Mac mit allen von der gleichen Nachrichteninformationen ausgefüllt und der Cursor am gleichen Speicherort, den sie sie in iOS verbleiben.

Ihre Apps, die denselben _Team ID_ sind geeignet, für die Übergabe verwenden, um Benutzeraktivitäten in apps zu fortfahren, solange diese app sind entweder über den iTunes App Store übermittelt oder durch ein registrierter Entwickler mit Vorzeichen (für Mac, Enterprise oder Ad-hoc-apps).

Alle `NSDocument` oder `UIDocument` basierend apps haben automatisch Übergabe unterstützen integriert und erfordern nur minimale Änderungen an der Übergabe unterstützen.

### <a name="continuing-user-activities"></a>Fortlaufende Benutzeraktivitäten

Die `NSUserActivity` Klasse (zusammen mit einigen kleine Änderungen an `UIKit` und `AppKit`) bietet Unterstützung für das Definieren eines Benutzers-Aktivität, die potenziell fortgesetzt werden kann, auf einem anderen von den Geräten des Benutzers.

Für eine Aktivität zu einer anderen Geräten des Benutzers über übergeben werden, müssen Sie in einer Instanz gekapselt `NSUserActivity`, außer die _aktuelle Aktivität_, dessen Nutzlast festgelegt (die Daten verwendet, um die Fortsetzung auszuführen) und die Aktivität muss dann für das Gerät übertragen werden.

Übergabe übergibt die absolute Mindestanforderungen von Informationen, die Aktivität fortgesetzt werden, mit größeren-Datenpakete synchronisiert wird, über die iCloud zu definieren.

Auf dem empfangenden Gerät wird der Benutzer eine Benachrichtigung, dass eine Aktivität für die Fortsetzung verfügbar ist. Wenn der Benutzer entscheidet, die Aktivität auf das neue Medium zu fortfahren, die angegebene app gestartet wird (wenn nicht bereits ausgeführt wird) und die Nutzlast aus der `NSUserActivity` wird verwendet, um die Aktivität zu starten.

[![](handoff-images/handoffinteractions.png "Einen Überblick über die Aktivitäten des Benutzers fortgesetzt")](handoff-images/handoffinteractions.png#lightbox)

Nur apps, die Freigabe des gleichen Entwicklers Team-ID und reagieren auf einen bestimmten _Aktivitätstyp_ für die Fortsetzung geeignet sind. Eine app definiert die Aktivität-Typen, die er unter unterstützt die `NSUserActivityTypes` -Schlüssel des seine **"Info.plist"** Datei. Angesichts wählt ein fortgesetzt Gerät zu die app die Fortsetzung, die basierend auf der Team-ID, Aktivitätstyp und optional die _Aktivitätstitel_.

Die empfangende Anwendung mithilfe von Informationen aus der `NSUserActivity`des `UserInfo` Wörterbuch, das Konfigurieren der Benutzeroberfläche und den Status der angegebenen Aktivität wiederherstellen, sodass der Übergang für den Endbenutzer eine nahtlose angezeigt wird.

Wenn die Fortsetzung mehr Informationen als effizient über gesendet werden können, erfordert eine `NSUserActivity`, wird die app fortsetzen kann einen Aufruf an die ursprüngliche app senden und herstellen eine oder mehrere Datenströme, um die erforderlichen Daten zu übertragen. Z. B. wenn die Aktivität ein großen Textdokuments mit mehrere Abbilder bearbeitet wurde, ist streaming erforderlich, um die für die Aktivität auf dem empfangenden Gerät zu fortfahren erforderlichen Informationen zu übertragen. Weitere Informationen finden Sie unter der [Unterstützung Fortsetzung Streams](#Supporting-Continuation-Streams) Abschnitt weiter unten.

Wie bereits erwähnt, `NSDocument` oder `UIDocument` basierend apps haben automatisch Übergabe, die integrierte Unterstützung. Weitere Informationen finden Sie unter der [unterstützen Übergabe in Dokument-basierten Apps](#Supporting-Handoff-in-Document-Based-Apps) Abschnitt weiter unten.

### <a name="the-nsuseractivity-class"></a>Die NSUserActivity-Klasse

Die `NSUserActivity` Klasse ist das primäre Objekt in einer Übergabe Exchange und wird verwendet, um den Status der Aktivität eines Benutzers zu kapseln, die für die Fortsetzung verfügbar ist. Eine app instanziiert dann eine Kopie des `NSUserActivity` für jede Aktivität, die es unterstützt und auf einem anderen Gerät fortsetzen möchte. Dokument-Editor ist eine Aktivität für jedes Dokument z. B. als aktuell geöffneten erstellen. Nur das vorderste Dokument (angezeigt in der vordersten Fenster oder die Registerkarte ") ist jedoch die _aktuelle Aktivität_ und für die Fortsetzung Verlaufsebene verfügbar.

Eine Instanz von `NSUserActivity` wird von beiden identifiziert die `ActivityType` und `Title` Eigenschaften. Die `UserInfo` Wörterbucheigenschaft wird verwendet, um Informationen über den Status der Aktivität auszuführen. Festlegen der `NeedsSave` Eigenschaft `true` gegebenenfalls zu verzögerten Laden Sie die Statusinformationen über die `NSUserActivity`Delegieren des. Verwenden der `AddUserInfoEntries` Methode zum Zusammenführen von neuer Daten von anderen Clients in der `UserInfo` Wörterbuch wie erforderlich, um die Aktivität Zustand erhalten bleiben.

### <a name="the-nsuseractivitydelegate-class"></a>Die NSUserActivityDelegate-Klasse

Die `NSUserActivityDelegate` wird verwendet, um die Informationen bleiben einer `NSUserActivity`des `UserInfo` Wörterbuch auf dem neuesten Stand und synchron mit dem aktuellen Status der Aktivität. Wenn das System benötigt die Informationen in der Aktivität aktualisiert werden (z. B. vor der Fortsetzung auf einem anderen Gerät), ruft er die `UserActivityWillSave` -Methode des Delegaten.

Sie implementieren müssen die `UserActivityWillSave` -Methode, und nehmen Sie alle Änderungen an der `NSUserActivity` (z. B. `UserInfo`, `Title`usw.), stellen Sie sicher, dass es immer noch den Status der aktuellen Aktivität widerspiegelt. Wenn das System Ruft den `UserActivityWillSave` -Methode, die `NeedsSave` Flag wird gelöscht. Wenn Sie die Eigenschaften für die Daten der Aktivität ändern, müssen Sie festlegen `NeedsSave` auf `true` erneut aus.

Anstatt die `UserActivityWillSave` Methode, die oben genannten, Sie können optional haben `UIKit` oder `AppKit` die Aktivitäten von Benutzern automatisch zu verwalten. Zu diesem Zweck legen Sie den Beantworter Objekt `UserActivity` Eigenschaft und Implementieren der `UpdateUserActivityState` Methode. Finden Sie unter der [unterstützen Übergabe in Responder](#Supporting-Handoff-in-Responders) weiter unten im Abschnitt Weitere Informationen.

### <a name="app-framework-support"></a>App-Framework-Unterstützung

Beide `UIKit` (iOS) und `AppKit` (OS X) bieten integrierte Unterstützung für die Übergabe in die `NSDocument`, Beantworter (`UIResponder`/`NSResponder`), und `AppDelegate` Klassen. Während jedes Betriebssystem Übergabe etwas anders implementiert, sind die APIs und der grundlegende Mechanismus identisch.

#### <a name="user-activities-in-document-based-apps"></a>Benutzeraktivitäten in Dokument-basierten Apps

Dokument-basierte IOS- und OS X-apps haben automatisch integrierte Übergabe-Unterstützung. Um Unterstützung zu aktivieren, müssen Sie zum Hinzufügen einer `NSUbiquitousDocumentUserActivityType` Schlüssel und Wert für die einzelnen `CFBundleDocumentTypes` Eintrag in der app **"Info.plist"** Datei.

Wenn dieser Schlüssel vorhanden ist, wird sowohl `NSDocument` und `UIDocument` automatisch erstellen `NSUserActivity` Instanzen für iCloud-basierte Dokumente des angegebenen Typs. Sie müssen einen Aktivitätstyp für jeden Typ von Dokument bereitgestellt wird, das app unterstützt, und mehrere Dokumenttypen können die gleichen Aktivitätstyp. Beide `NSDocument` und `UIDocument` automatisch Auffüllen der `UserInfo` Eigenschaft der `NSUserActivity` mit ihren `FileURL` den Wert der Eigenschaft.

Unter OS X die `NSUserActivity` von verwalteten `AppKit` und Responder automatisch zugeordnet werden Sie die aktuelle Aktivität aus, wenn das Fenster des Dokuments im Hauptfenster wird. Bei iOS kann für `NSUserActivity` durch verwaltete Objekte `UIKit`, müssen Sie entweder `BecomeCurrent` Methode explizit oder über des Dokuments `UserActivity` festgelegte Eigenschaft eine `UIViewController` Wenn geht die app wechselt in den Vordergrund.

`AppKit` automatisch alle wiederherstellen `UserActivity` Eigenschaft, die auf diese Weise unter OS X erstellt. Dies geschieht, wenn die `ContinueUserActivity` -Methode zurückkehrt `false` oder nicht implementiert ist. In diesem Fall wird das Dokument geöffnet, mit der `OpenDocument` Methode der `NSDocumentController` und sie erhalten dann einen `RestoreUserActivityState` -Methodenaufruf.

Finden Sie unter der [unterstützen Übergabe in Dokument-basierten Apps](#Supporting-Handoff-in-Document-Based-Apps) weiter unten im Abschnitt Weitere Informationen.

#### <a name="user-activities-and-responders"></a>Benutzeraktivitäten und -Responder

Beide `UIKit` und `AppKit` eine Benutzeraktivität können automatisch verwaltet werden, wenn Sie es als ein Responder-Objekt festgelegt, `UserActivity` Eigenschaft. Wenn der Status geändert wurde, müssen Sie festlegen der `NeedsSave` Eigenschaft den Beantworter `UserActivity` auf `true`. Speichert das System automatisch die `UserActivity` bei Bedarf nach dem Erteilen der Beantworter Zeit mit dem Status durch den Aufruf aktualisiert, ihre `UpdateUserActivityState` Methode.

Wenn mehrere Responder eine einzelne Freigabe `NSUserActivity` Instanz Erhalt einer `UpdateUserActivityState` Rückruf, wenn das System das Benutzerobjekt für die Aktivität aktualisiert. Der Beantworter aufrufen muss die `AddUserInfoEntries` Methode zum Aktualisieren der `NSUserActivity`des `UserInfo` Wörterbuch, das den aktuellen Aktivitätsstatus zu diesem Zeitpunkt wider. Die `UserInfo` Wörterbuch ist deaktiviert, vor jedem `UpdateUserActivityState` aufrufen.

Um selbst aus einer Aktivität zu trennen, kann eine Beantworter Festlegen seiner `UserActivity` Eigenschaft `null`. Wenn ein app-Framework verwalteten `NSUserActivity` Instanz hat keine mehr zugeordneten Responder oder Dokumente, wird automatisch für ungültig erklärt.

Finden Sie unter der [unterstützen Übergabe in Responder](#Supporting-Handoff-in-Responders) weiter unten im Abschnitt Weitere Informationen.

#### <a name="user-activities-and-the-appdelegate"></a>Benutzeraktivitäten und die AppDelegate

Ihre app `AppDelegate` seine primäre Einstiegspunkt ist, bei der Verarbeitung einer Fortsetzung für die Übergabe von. Wenn der Benutzer reagiert auf eine Benachrichtigung Übergabe die entsprechende app gestartet wird (wenn nicht bereits ausgeführt wird) und die `WillContinueUserActivityWithType` Methode der `AppDelegate` aufgerufen wird. An diesem Punkt sollte die app den Benutzer informiert werden, den die Fortsetzung gestartet wird.

Die `NSUserActivity` Instanz wird übermittelt, wenn die `AppDelegate`des `ContinueUserActivity` -Methode aufgerufen wird. An diesem Punkt sollten Sie konfigurieren die app-Benutzeroberfläche und die angegebene Aktivität fortfahren.

Finden Sie unter der [implementieren Übergabe](#Implementing-Handoff) weiter unten im Abschnitt Weitere Informationen.

## <a name="enabling-handoff-in-a-xamarin-app"></a>Aktivieren die Übergabe in einer Xamarin-App

Aufgrund der sicherheitsanforderungen durch Übergabe auferlegt werden muss eine Xamarin.iOS-app, die Übergabe-Framework verwendet, ordnungsgemäß in der Apple Developer Portal und in der Projektdatei Xamarin.iOS konfiguriert werden.

Führen Sie folgende Schritte aus:

1. Melden Sie sich bei der [Apple-Entwicklerportal](http://developer.apple.com).
2. Klicken Sie auf **Zertifikate "," Bezeichner "und" Profile**.
3. Wenn Sie dies noch nicht getan haben, klicken Sie auf **Bezeichner** und eine ID für Ihre app zu erstellen (z. B. `com.company.appname`), andernfalls Bearbeiten Ihrer vorhandenen ID an.
4. Sicherstellen, dass die **iCloud** Dienst für die angegebene ID Punkte überprüft wurden: 

    [![](handoff-images/provision01.png "Aktivieren Sie den iCloud-Dienst für die angegebene ID")](handoff-images/provision01.png#lightbox)
5. Speichern Sie die Änderungen.
4. Klicken Sie auf **Provisioning Profile** > **Entwicklung** und erstellen Sie ein Bereitstellungsprofil für Sie Neuentwicklungen app: 

    [![](handoff-images/provision02.png "Erstellen Sie eine neue Entwicklungen Bereitstellungsprofil für die app")](handoff-images/provision02.png#lightbox)
5. Herunterladen Sie und installieren Sie das neue Bereitstellungsprofil oder verwenden Sie Xcode herunterladen und installieren das Profil.
6. Bearbeiten Sie Ihre Xamarin.iOS Projektoptionen, und stellen Sie sicher, dass Sie das Bereitstellungsprofil verwenden, das Sie soeben erstellt haben: 

    [![](handoff-images/provision03.png "Wählen Sie das soeben erstellte Bereitstellungsprofil")](handoff-images/provision03.png#lightbox)
7. Als Nächstes bearbeiten Ihrer **"Info.plist"** Datei, und stellen Sie sicher, dass Sie die App-ID verwenden, mit denen die provisioning-Profil erstellt wurde: 

    [![](handoff-images/provision04.png "Festlegen von App-ID")](handoff-images/provision04.png#lightbox)
8. Führen Sie einen Bildlauf zu der **Hintergrundmodi** Abschnitt, und überprüfen Sie die folgenden Elemente: 

    [![](handoff-images/provision05.png "Aktivieren Sie die erforderlichen hintergrundmodi")](handoff-images/provision05.png#lightbox)
9. Speichern Sie die Änderungen auf alle Dateien.

Mit diesen Einstellungen vorhanden kann die Anwendung jetzt die Übergabe Framework-APIs zuzugreifen. Ausführliche Informationen zu Bereitstellung, finden Sie unter unsere [Gerätebereitstellung](~/ios/get-started/installation/device-provisioning/index.md) und [Bereitstellung der App](~/ios/get-started/installation/device-provisioning/index.md) Handbüchern.

## <a name="implementing-handoff"></a>Implementieren die Übergabe

Benutzeraktivitäten können zwischen apps fortgesetzt werden, die mit dem gleichen Entwickler Team ID signiert sind und Unterstützung für die gleiche Aktivität. Übergabe in einer app Xamarin.iOS implementieren, müssen Sie ein Benutzerobjekt für die Aktivität zu erstellen (entweder im `UIKit` oder `AppKit`), aktualisieren Sie den Zustand des Objekts zum Nachverfolgen der Aktivität und fortsetzen die Aktivität auf einem Gerät empfangen.

### <a name="identifying-user-activities"></a>Identifizieren von Benutzeraktivitäten

Der erste Schritt beim Implementieren der Übergabe von Benutzeraktivitäten identifizieren, die Ihrer app unterstützt werden, und sehen, welche der diese Aktivitäten eignen sich gut für die Fortsetzung auf einem anderen Gerät. Zum Beispiel: eine TODO-app bearbeiten Elemente als eine unterstützen möglicherweise _Benutzertyp-Aktivität_, und Durchsuchen der Liste Verfügbare Objekte wie eine andere zu unterstützen.

Apps kann so viele Benutzer Aktivitätstypen wie erforderlich sind, eine für jede Funktion erstellen, die die app bereitstellt. Für jeden Aktivitätstyp Benutzer müssen die app zu überwachen, wann eine Aktivität des Typs beginnt und endet, und auf dem neuesten Stand Zustandsinformationen, um diese Aufgabe auf einem anderen Gerät weiterhin verwalten muss.

Benutzeraktivitäten können auf eine beliebige app mit derselben ID Team ohne 1: 1-Zuordnung zwischen den sendenden und empfangenden apps signiert fortgesetzt werden. Beispielsweise kann eine bestimmte app vier verschiedene Typen von Aktivitäten erstellen, die von anderen, einzelne apps auf einem anderen Gerät verwendet werden. Dieser Fall kann häufiger eintreten zwischen einer Mac-Version der app (die möglicherweise viele Features und Funktionen haben) und iOS-apps ist, in dem jede app, kleinere und konzentriert sich auf eine bestimmte Aufgabe.

### <a name="creating-activity-type-identifiers"></a>Aktivität Typ-IDs erstellen

Die _Typbezeichner der Aktivität_ ist eine kurze Zeichenfolge hinzugefügt, die `NSUserActivityTypes` Array von der app **"Info.plist"** Datei, die zur eindeutigen Identifizierung einer bestimmten Aktivität Benutzertyp verwendet. Im Array für jede Aktivität, die die app unterstützt werden, wird ein Eintrag vorhanden sein. Apple empfiehlt eine Notation Reverse-DNS-Format für den Typ Aktivitätsbezeichner Konflikte zu vermeiden. Zum Beispiel: `com.company-name.appname.activity` für bestimmte app-basierte Aktivitäten oder `com.company-name.activity` für Aktivitäten, die in mehreren apps ausgeführt werden können.

Der Typbezeichner der Aktivität wird verwendet, für die Erstellung einer `NSUserActivity` Instanz, um den Typ der Aktivität zu identifizieren. Wenn eine Aktivität auf einem anderen Gerät fortgesetzt wird, bestimmt den Aktivitätstyp (zusammen mit der app-Team-ID) der app zu starten, um die Aktivität fortgesetzt.

Beispielsweise Kegel zum Erstellen einer Beispielapp aufgerufen **MonkeyBrowser** ([hier herunterladen](https://developer.xamarin.com/samples/monotouch/ios8/MonkeyBrowser/)). Diese app stellt vier Registerkarten, jede mit einer anderen URL öffnen in einer Web-Browser-Ansicht bereit. Der Benutzer wird eine der Registerkarten auf einem anderen iOS-Gerät, das Ausführen der app zu fortfahren können.

Zum Erstellen der erforderlichen Aktivität Typ-IDs zur Unterstützung dieses Verhalten Bearbeiten der **"Info.plist"** Datei, und wechseln Sie zu der **Quelle** anzeigen. Hinzufügen einer `NSUserActivityTypes` Taste, und erstellen Sie die folgenden Bezeichner:

[![](handoff-images/type01.png "Die NSUserActivityTypes Schlüssel und die erforderlichen Bezeichner in der Plist-editor")](handoff-images/type01.png#lightbox)

Vier neue Aktivität Typ-IDs, jeweils einen für die Registerkarten im Beispiel erstellten **MonkeyBrowser** app. Wenn Sie Ihre eigenen apps zu erstellen, ersetzen Sie den Inhalt von der `NSUserActivityTypes` array mit der Aktivität-Datentypbezeichner, die spezifisch für die Aktivitäten Ihrer app unterstützt.

### <a name="tracking-user-activity-changes"></a>Nachverfolgen von Änderungen für Benutzer-Aktivität

Wenn wir erstellen eine neue Instanz der der `NSUserActivity` -Klasse, geben wir eine `NSUserActivityDelegate` Instanz zum Nachverfolgen von Änderungen an der Aktivitätsstatus. Im folgenden Code kann beispielsweise verwendet werden, zum Nachverfolgen von Änderungen am:

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

Die `UserActivityReceivedData` Methode wird aufgerufen, wenn ein Fortsetzung Stream Daten von einer sendenden Gerät erhalten hat. Weitere Informationen finden Sie unter der [Unterstützung Fortsetzung Streams](#Supporting-Continuation-Streams) Abschnitt weiter unten.

Die `UserActivityWasContinued` Methode wird aufgerufen, wenn ein anderes Gerät über eine Aktivität aus das aktuelle Gerät stattgefunden hat. Je nach Typ der Aktivität, z. B. Hinzufügen eines neuen Elements zu einer TODO-Liste kann die app die Aktivität auf dem sendenden Gerät abbrechen müssen.

Die `UserActivityWillSave` Methode wird aufgerufen, bevor Änderungen an der Aktivität gespeichert und lokal verfügbaren Geräten synchronisiert werden. Verwenden Sie diese Methode alle letzten Minute verarbeitet wurden Änderungen an der `UserInfo` Eigenschaft von der `NSUserActivity` Instanz, bevor sie gesendet wird.

### <a name="creating-a-nsuseractivity-instance"></a>Erstellen einer Instanz NSUserActivity

Jede Aktivität, die Ihre app die Möglichkeit, dass Sie den Vorgang fortsetzen auf einem anderen Gerät bereitstellen möchte gekapselt werden muss, eine `NSUserActivity` Instanz. Die app kann beliebig viele Aktivitäten nach Bedarf zu erstellen und die Art dieser Aktivitäten ist abhängig von der Funktionalität und Merkmale die betreffende Anwendung. Eine e-Mail-app kann beispielsweise eine Aktivität zum Erstellen einer neuen Nachricht und ein anderes zum Lesen einer Nachricht erstellen.

Für unser Beispiel-app ein neues `NSUserActivity` wird jedes Mal, wenn der Benutzer eingibt, eine neue URL in einem der im Registerkartenformat Webbrowseransicht erstellt. Der folgende Code speichert den Zustand einer angegebenen Registerkarte:

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

Er erstellt ein neues `NSUserActivity` oben erstellten des Aktivitätstyps Benutzer verwenden, und stellt einen lesbaren Titel für die Aktivität definiert. Es fügt zu einer Instanz von der `NSUserActivityDelegate` erstellte oben zum Überwachen von Status ändert sich und iOS teilt mit, dass diese Benutzer-Aktivität die aktuelle Aktivität ist.

### <a name="populating-the-userinfo-dictionary"></a>Das Wörterbuch UserInfo Auffüllen

Wie oben gesehen haben, die `UserInfo` Eigenschaft von der `NSUserActivity` Klasse ist eine `NSDictionary` von Schlüssel-Wert-Paaren verwendet, um den Status einer bestimmten Aktivität definieren. Die Werte, die in gespeicherten `UserInfo` muss eines der folgenden Typen sein: `NSArray`, `NSData`, `NSDate`, `NSDictionary`, `NSNull`, `NSNumber`, `NSSet`, `NSString`, oder `NSURL`. `NSURL` Datenwerte, die auf iCloud Dokumente verweisen werden automatisch angepasst werden, damit sie auf die gleiche Dokumente auf einem Gerät empfangen verweisen.

Im obigen Beispiel erstellten ein `NSMutableDictionary` Objekt, und es mit einem einzelnen Schlüssel angeben, die der Benutzer gerade auf die angegebene Registerkarte anzeigt wurde URL aufgefüllt. Die `AddUserInfoEntries` -Methode der Aktivität Benutzer wurde verwendet, um die Aktivität mit den Daten zu aktualisieren, die zum Wiederherstellen der Aktivität auf dem empfangenden Gerät verwendet werden:

```csharp
// Update the activity when the tab's URL changes
var userInfo = new NSMutableDictionary ();
userInfo.Add (new NSString ("Url"), new NSString (url));
UserActivity.AddUserInfoEntries (userInfo);
```

Apple vorschlagen ständigen Schutz der Informationen, die an die minimal gesendet, um sicherzustellen, dass die Aktivität rechtzeitig an die empfangende Gerät gesendet wird. Wenn größere Informationen erforderlich ist, wie ein Bild an ein Dokument angefügt bearbeitet werden gesendet werden, sollten Sie die Fortsetzung Streams verwenden muss. Finden Sie unter der [Unterstützung Fortsetzung Streams](#Supporting-Continuation-Streams) weiter unten für weitere Details.

### <a name="continuing-an-activity"></a>Eine Aktivität fortfahren

Übergabe informiert automatisch lokalen IOS- und OS X-Geräte, die in die physische Nähe zu den ursprünglichen Gerät und in der gleichen iCloud-Konto, die Verfügbarkeit des vernachlässigbar Benutzeraktivitäten signiert. Wenn der Benutzer entscheidet, eine Aktivität auf einem neuen Gerät zu fortfahren, startet das System die entsprechende app (basierend auf dem Team-ID und Aktivitätstyp) und die Informationen der `AppDelegate` , dass die Fortsetzung erfolgen muss.

Zunächst wird die `WillContinueUserActivityWithType` Methode wird aufgerufen, damit die app den Benutzer werden, die die Fortsetzung wird informiert kann in Kürze. Wir verwenden den folgenden Code in die **AppDelegate.cs** Datei unsere beispielanwendung Ausgangspunkt Fortsetzung behandelt:

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

Im obigen Beispiel registriert jeden View-Controller mit dem `AppDelegate` und verfügt über einen öffentlichen `PreparingToHandoff` Methode, die anzeigt, einen Indikator für die Aktivität und einer Nachricht und teilt dem Benutzer, die davon ausgehen, dass die Aktivität an das aktuelle Gerät übergeben werden. Beispiel:

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

Die `ContinueUserActivity` von der `AppDelegate` wird aufgerufen, um die angegebene Aktivität tatsächlich zu fortfahren. Ebenfalls aus unserem Beispiel-app:

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

Die öffentliche `PerformHandoff` jedes View-Controller tatsächlich vorgenommen wird, die Übergabe und die Aktivität auf das aktuelle Gerät wiederhergestellt. Bei diesem Beispiel die gleiche URL in eine angegebene Registerkarte, die der Benutzer, auf einem anderen Gerät besucht wurde angezeigt. Beispiel:

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

Die `ContinueUserActivity` Methode enthält einen `UIApplicationRestorationHandler` , dass Sie, für das Dokument oder Beantworter aufrufen können basierend Aktivität fortsetzen. Sie übergeben müssen eine `NSArray` oder wiederherstellbaren Objekte an den Handler Wiederherstellung beim Aufruf. Zum Beispiel:

```csharp
completionHandler (new NSObject[]{Tab4});
```

Für jedes Objekt übergeben dessen `RestoreUserActivityState` Methode wird aufgerufen. Jedes Objekt können Sie dann die Daten in der `UserInfo` Wörterbuch, das ihr eigener Zustand wiederherstellen. Zum Beispiel:

```csharp
public override void RestoreUserActivityState (NSUserActivity activity)
{
    base.RestoreUserActivityState (activity);

    // Log activity
    Console.WriteLine ("Restoring Activity {0}", activity.Title);
}
```

Für Dokument-basierte apps, wenn Sie nicht implementieren die `ContinueUserActivity` -Methode, oder er gibt `false`, `UIKit` oder `AppKit` können die Aktivität automatisch fortgesetzt. Finden Sie unter der [unterstützen Übergabe in Dokument-basierten Apps](#Supporting-Handoff-in-Document-Based-Apps) weiter unten im Abschnitt Weitere Informationen.

### <a name="failing-handoff-gracefully"></a>Übergabe ordnungsgemäß fehlschlagen

Da die Übertragung von Informationen zwischen einem lose verbundene Auflistung IOS- und OS X-Geräte Übergabe abhängt, kann der Übertragungsprozess manchmal fehl. Entwerfen Sie Ihre app auf diese Fehler handhaben und informiert den Benutzer alle Situationen, die auftreten.

Im Fall eines Fehlers die `DidFailToContinueUserActivitiy` Methode der `AppDelegate` aufgerufen wird. Zum Beispiel:

```csharp
public override void DidFailToContinueUserActivitiy (UIApplication application, string userActivityType, NSError error)
{
    // Log information about the failure
    Console.WriteLine ("User Activity {0} failed to continue. Error: {1}", userActivityType, error.LocalizedDescription);
}
```

Verwenden Sie die angegebene `NSError` Anmeldeinformationen für den Benutzer über den Fehler.

## <a name="native-app-to-web-browser-handoff"></a>Systemeigene App zu Übergabe der Web-Browsers

Ein Benutzer möchte eventuell eine Aktivität zu fortfahren, ohne eine entsprechende systemeigene app, die auf das gewünschte Gerät installiert. In einigen Situationen eine webbasierte Schnittstelle gegebenenfalls über die erforderliche Funktionalität und die Aktivität kann trotzdem fortgesetzt werden. Beispielsweise kann die e-Mail-Konto des Benutzers eine Web-Basis-Benutzeroberfläche für verfassen und Lesen von Nachrichten bereitstellen.

Wenn die URL für die ursprüngliche und systemeigene app bekannt ist für die Webschnittstelle (und die erforderliche Syntax zum Identifizieren des angegebenen Elements fortgesetzt werden), können sie diese Informationen in Codieren der `WebpageURL` Eigenschaft von der `NSUserActivity` Instanz. Wenn die empfangende Gerät nicht über eine entsprechende systemeigene app installiert, um die Fortsetzung zu bewältigen verfügt, kann die bereitgestellten Weboberfläche aufgerufen werden.

## <a name="web-browser-to-native-app-handoff"></a>Webbrowser, um systemeigene App Übergabe

Wenn der Benutzer wurde eine webbasierte Schnittstelle auf dem ursprünglichen Gerät verwenden und eine systemeigene app auf dem empfangenden Gerät Domänenteil des Ansprüche der `WebpageURL` -Eigenschaft, und klicken Sie dann auf das System die app das Handle der Fortsetzung verwendet. Das neue Gerät erhält eine `NSUserActivity` Instanz, die den Typ der Aktivität wie markiert `BrowsingWeb` und die `WebpageURL` enthält die URL, die der Benutzer besucht wurde, die `UserInfo` Wörterbuch leer sein wird.

Für eine app zur Teilnahme an diese Art der Übergabe, müssen sie die Domäne in Anspruch eine `com.apple.developer.associated-domains` Anspruch mit dem Format `<service>:<fully qualified domain name>` (z. B.: `activity continuation:company.com`).

Wenn die angegebene Domäne entspricht einer `WebpageURL` Übergabe den Wert der Eigenschaft eine Liste der genehmigten app IDs von der Website an, dass diese Domäne heruntergeladen. Die Website muss eine Liste der genehmigten-IDs in einer signierten JSON-Datei mit dem Namen bereitstellen **Apple-app-sitezuordnung** (z. B. `https://company.com/apple-app-site-association`).

Diese JSON-Datei enthält ein Wörterbuch, das eine Liste von app-IDs in der Form soll `<team identifier>.<bundle identifier>`. Zum Beispiel:

```csharp
{
    "activitycontinuation": {
        "apps": [    "YWBN8XTPBJ.com.company.FirstApp",
            "YWBN8XTPBJ.com.company.SecondApp" ]
    }
}
```

Zum Signieren der JSON-Datei (so, dass sie den richtigen `Content-Type` von `application/pkcs7-mime`), verwenden Sie die **Terminaldienste** app und eine `openssl` Befehl mit einem Zertifikat und Schlüssel, die von einer Zertifizierungsstelle als vertrauenswürdig iOS ausgegeben (finden Sie unter [ http://support.Apple.com/kb/ht5012](http://support.apple.com/kb/ht5012) eine Liste). Zum Beispiel:

```csharp
echo '{"activitycontinuation":{"apps":["YWBN8XTPBJ.com.company.FirstApp",
"YWBN8XTPBJ.com.company.SecondApp"]}}' > json.txt

cat json.txt | openssl smime -sign -inkey company.com.key
-signer company.com.pem
-certfile intermediate.pem
-noattr -nodetach
-outform DER > apple-app-site-association
```

Die `openssl` Befehl gibt eine signierte JSON-Datei, die Sie auf Ihrer Website am Platzieren der **Apple-app-Site-Zuordnung** URL. Zum Beispiel:

```csharp
https://example.com/apple-app-site-association.
```

Erhält die app eine beliebige Aktivität, deren `WebpageURL` Domäne befindet sich in seiner `com.apple.developer.associated-domains` Anspruch. Nur die `http` und `https` Protokolle werden unterstützt, ein anderes Protokoll wird eine Ausnahme ausgelöst.

## <a name="supporting-handoff-in-document-based-apps"></a>Unterstützung von Übergabe in Dokument-basierten Apps

Wie bereits erwähnt, iOS und OS X, Dokument-basierte apps automatisch unterstützen die Übergabe von Dokumenten mit iCloud-basierte Wenn der app **"Info.plist"** -Datei enthält eine `CFBundleDocumentTypes` -Schlüssel des `NSUbiquitousDocumentUserActivityType`. Zum Beispiel:

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

In diesem Beispiel ist die Zeichenfolge ein Reverse-DNS-app-Kennzeichner, mit dem Namen der Aktivität angefügt. Wenn auf diese Weise eingegeben haben, die typeinträgen Aktivität müssen nicht wiederholt werden, in der `NSUserActivityTypes` Array von der **"Info.plist"** Datei.

Dem Benutzer Aktivitätsobjekt automatisch erstellt (über des Dokuments verfügbar `UserActivity` Eigenschaft) verwiesen, die von anderen Objekten in der app und wiederherzustellenden Zustand auf Fortsetzung verwendet werden können. Beispielsweise positionieren zum Nachverfolgen von Element Auswahl und das Dokument. Sie müssen diese Aktivitäten festlegen `NeedsSave` Eigenschaft, um `true` immer der Status ändert, und Aktualisieren der `UserInfo` Wörterbuch in die `UpdateUserActivityState` Methode.

Die `UserActivity` -Eigenschaft kann von jedem Thread verwendet werden, und das Schlüssel-Wert beachten (KVO)-Protokoll entspricht, damit es verwendet werden kann, um ein Dokument synchron halten, während sie ein-und der iCloud verschoben. Die `UserActivity` Eigenschaft wird ungültig gemacht werden, wenn das Dokument geschlossen wird.

Weitere Informationen finden Sie in der Apple- [Aktivität Benutzersupport im Dokument-basierte Apps](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Handoff/HandoffFundamentals/HandoffFundamentals.html#//apple_ref/doc/uid/TP40014338-CH3-SW5) Dokumentation.

## <a name="supporting-handoff-in-responders"></a>Unterstützen von Übergabe in Responder

Können Sie Responder zuordnen (geerbt von entweder `UIResponder` auf iOS oder `NSResponder` unter OS X) Aktivitäten durch Festlegen ihrer `UserActivity` Eigenschaften. Speichert das System automatisch die `UserActivity` Eigenschaft auf den entsprechenden Zeiten, in Aufrufen des beantworters `UpdateUserActivityState` Methode, um das Benutzeraktivität mit aktuellen Daten hinzuzufügen die `AddUserInfoEntriesFromDictionary` Methode.

## <a name="supporting-continuation-streams"></a>Unterstützende Fortsetzung Streams

Die Situationen, in denen die Menge an Informationen erforderlich, um eine Aktivität zu Fortfahren nicht effizient werden durch die erste Übergabe-Nutzlast übertragen kann. In diesen Situationen kann die empfangende Anwendung eine oder mehrere Streams zwischen sich selbst und die ursprünglichen app Übertragung die Daten herstellen.

Die Ursprungs-app wird festgelegt, der `SupportsContinuationStreams` Eigenschaft der `NSUserActivity` -Instanz, auf `true`. Zum Beispiel:

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

Die empfangende Anwendung rufen Sie anschließend die `GetContinuationStreams` Methode der `NSUserActivity` in seine `AppDelegate` herstellen den Stream. Zum Beispiel:

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

Auf dem ursprünglichen Gerät empfängt der Benutzerdelegat der Aktivität der Datenströme durch Aufrufen seiner `DidReceiveInputStream` Methode, um die Daten bereitzustellen, um die Benutzeraktivität auf dem Gerät fortsetzen zu Fortfahren angefordert.

Verwenden Sie eine `NSInputStream` nur-Lese-Zugriff Daten zu streamen, bereitstellen und eine `NSOutputStream` bieten nur-schreiben Zugriff. Die Datenströme sollte verwendet werden, in einer Anforderung und Antwort-Prinzip hasht, in denen die empfangende Anwendung mehr Daten anfordert und die ursprüngliche app wird bereitgestellt. So, dass Daten, die in den Ausgabestream geschrieben werden, auf dem ursprünglichen Gerät aus dem Eingabestream auf dem Gerät fortgesetzt gelesen werden und umgekehrt.

Auch in Situationen, in denen Fortsetzung Stream erforderlich sind, darf es eine minimale Back und Entwurfsregeln Kommunikation zwischen den zwei Web-apps.

Weitere Informationen finden Sie auf der Apple [Using Fortsetzung Streams](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Handoff/AdoptingHandoff/AdoptingHandoff.html#//apple_ref/doc/uid/TP40014338-CH2-SW13) Dokumentation.

## <a name="handoff-best-practices"></a>Übergabe bewährte Methoden

Die erfolgreiche Implementierung die nahtlose Fortsetzung einer Benutzeraktivität über Übergabe erfordert eine sorgfältige Planung aufgrund alle der verschiedenen Komponenten beteiligt. Apple empfiehlt, übernehmen die folgenden bewährten Methoden für Ihre apps Übergabe aktiviert:

- Entwerfen Sie Ihre Benutzeraktivitäten müssen Sie die kleinste Nutzlast möglich ist, beziehen den Status der Aktivität fortgesetzt wird. Je größer die Nutzlast sind, desto länger dauert die Fortsetzung zu starten.
- Wenn Sie große Mengen von Daten für erfolgreiche Fortsetzung übertragen müssen, berücksichtigen Sie die Kosten für Konfiguration und Netzwerkaufwand verringert beteiligt.
- Es ist üblich für eine große Mac-app, um Benutzeraktivitäten zu erstellen, die durch mehrere kleinere, aufgabenspezifischen apps auf iOS-Geräten verarbeitet werden. Die andere app und die Versionen des Betriebssystems sollten Zusammenwirken oder fehlschlägt, ordnungsgemäß entworfen werden.
- Wenn Ihre Aktivitätstypen angeben möchten, verwenden Sie Reverse-DNS-Notation, Konflikte zu vermeiden. Wenn eine Aktivität zu einer bestimmten app spezifisch ist, sollte der Name in der Typdefinition enthalten sein (z. B. `com.myCompany.myEditor.editing`). Wenn die Aktivität in mehreren apps arbeiten kann, legen Sie den Anwendungsnamen aus der Definition (z. B. `com.myCompany.editing`).
- Wenn Ihre app benötigt, um den Status der Aktivität eines Benutzers zu aktualisieren (`NSUserActivity`) legen Sie die `NeedsSave` Eigenschaft `true`. Zu geeigneten Zeitpunkten und Übergabe der Delegat ruft `UserActivityWillSave` Methode, sodass Sie aktualisieren können die `UserInfo` Wörterbuch nach Bedarf.
- Da es sich bei der Übergabe-Prozess auf dem empfangenden Gerät nicht sofort initialisieren kann, sollten Sie implementieren die `AppDelegate`des `WillContinueUserActivity` und informiert den Benutzer, der eine Fortsetzung wird gerade gestartet.

## <a name="example-handoff-app"></a>Übergabe-Beispielanwendung

Als Beispiel für die Übergabe in einem Xamarin.iOS-app verwenden, haben wir enthalten die [ **MonkeyBrowser** ](https://developer.xamarin.com/samples/monotouch/ios8/MonkeyBrowser/) Beispiel-app mit diesem Handbuch. Die app enthält vier Registerkarten, mit denen der Benutzer im Internet, jeweils mit einem angegebenen Aktivitätstyp Surfen: Weather "," Favoriten "," Kaffee Break "und" Arbeit.

Auf jeder Registerkarte gelangt der Benutzer eine neue URL und datenabzweigungen der **Go** Schaltfläche, ein neues `NSUserActivity` für diese Registerkarte, die die URL enthält, die der Benutzer derzeit durchsucht wird erstellt:

[![](handoff-images/handoff01.png "Übergabe-Beispielanwendung")](handoff-images/handoff01.png#lightbox)

Wenn eine andere von den Geräten des Benutzers verfügt die **MonkeyBrowser** app installiert, mit dem gleichen Benutzerkonto iCloud angemeldet ist, wird auf dem gleichen Netzwerk und in der Nähe zu den oben genannten Geräte, die Übergabe-Aktivität wird auf der Startseite angezeigt Bildschirm (in der unteren linken Ecke):

[![](handoff-images/handoff02.png "Die Übergabe-Aktivität, die auf dem Startbildschirm in der unteren linken Ecke angezeigt")](handoff-images/handoff02.png#lightbox)

Wenn der Benutzer nach oben auf das Symbol "Übergabe" zieht, die app wird gestartet, und der Benutzeraktivität angegeben wird, der `NSUserActivity` wird auf das neue Gerät fortgesetzt werden:

[![](handoff-images/handoff03.png "Die Fortsetzung auf das neue Gerät Benutzeraktivität")](handoff-images/handoff03.png#lightbox)

Wenn die Benutzeraktivität erfolgreich gesendet wurde ein anderes Apple Gerät, das Gerät senden des `NSUserActivity` erhalten einen Anruf an den `UserActivityWasContinued` Methode auf seine `NSUserActivityDelegate` , damit es weiß, dass die Aktivitäten von Benutzern zu einem anderen erfolgreich übertragen wurden das Gerät.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat eine Einführung in die Übergabe-Framework verwendet, um einer Benutzeraktivität zwischen mehreren von Apple-Geräte des Benutzers zu fortfahren gegeben. Als Nächstes wurde es gezeigt, wie aktivieren und Übergabe in einem Xamarin.iOS-app zu implementieren. Schließlich erläutert er die verschiedenen Typen von Übergabe Fortsetzungen verfügbar und die Übergabe bewährten Methoden.



## <a name="related-links"></a>Verwandte Links

- [iOS 9-Beispiele](https://developer.xamarin.com/samples/ios/iOS9/)
- [MonkeyBrowser-Beispiel](https://developer.xamarin.com/samples/monotouch/ios8/MonkeyBrowser/)
- [iOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [Was ist neu in iOS 9.0 verfügen](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [HomeKitDeveloper-Handbuch](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html)
- [HomeKit Richtlinien zur Benutzeroberfläche](https://developer.apple.com/homekit/ui-guidelines/)
- [HomeKit Frameworkverweis](https://developer.apple.com/library/ios/home_kit_framework_ref)
