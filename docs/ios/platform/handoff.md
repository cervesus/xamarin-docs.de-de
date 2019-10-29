---
title: Übergabe in xamarin. IOS
description: In diesem Artikel wird das Arbeiten mit Handoff in einer xamarin. IOS-App behandelt, um Benutzeraktivitäten zwischen apps zu übertragen, die auf den anderen Geräten eines Benutzers ausgeführt werden.
ms.prod: xamarin
ms.assetid: 405F966A-4085-4621-AA15-33D663AD15CD
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: 97a4a90b66e2c205ef8e9081e6989bf0f3650b16
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032408"
---
# <a name="handoff-in-xamarinios"></a>Übergabe in xamarin. IOS

_In diesem Artikel wird das Arbeiten mit Handoff in einer xamarin. IOS-App behandelt, um Benutzeraktivitäten zwischen apps zu übertragen, die auf den anderen Geräten eines Benutzers ausgeführt werden._

Apple hat die Übergabe in ios 8 und OS X Yosemite (10,10) eingeführt, um dem Benutzer einen allgemeinen Mechanismus zum Übertragen von Aktivitäten zu ermöglichen, die auf einem seiner Geräte gestartet werden, auf ein anderes Gerät, auf dem dieselbe APP oder eine andere app ausgeführt wird, die dieselbe Aktivität unterstützt.

[![](handoff-images/handoff02.png "An example of performing a Handoff operation")](handoff-images/handoff02.png#lightbox)

In diesem Artikel wird kurz darauf eingegangen, wie Sie die Aktivitäts Freigabe in einer xamarin. IOS-App aktivieren und das Handoff-Framework ausführlich behandeln:

## <a name="about-handoff"></a>Informationen über die Übergabe

Die Übergabe (auch als Kontinuität bezeichnet) wurde von Apple in ios 8 und OS X Yosemite (10,10) eingeführt, sodass der Benutzer eine Aktivität auf einem Ihrer Geräte (IOS oder Mac) starten und dieselbe Aktivität auf einem anderen Gerät (wie durch die iclou des Benutzers identifiziert) fortsetzen kann. d-Konto).

Die Übergabe von Handoff wurde in ios 9 erweitert, um auch neue, erweiterte Suchfunktionen zu unterstützen. Weitere Informationen finden Sie in der Dokumentation zu den [Such Erweiterungen](~/ios/platform/search/index.md) .

Der Benutzer kann z. b. eine e-Mail auf seinem iPhone starten und die e-Mail nahtlos auf seinem Mac fortsetzen, wobei die gleichen Nachrichten Informationen und der Cursor am gleichen Speicherort wie in ios eingefügt werden.

Alle apps, die dieselbe _Team-ID_ verwenden, sind für die Verwendung von Handoff zum Fortsetzen von Benutzeraktivitäten in allen apps berechtigt, sofern diese APP entweder über den iTunes App Store übermittelt oder von einem registrierten Entwickler (für Mac-, Enterprise-oder Ad-hoc-Apps) signiert wird.

Alle `NSDocument` oder `UIDocument` basierten apps verfügen automatisch über die Unterstützung von Handoff und erfordern minimale Änderungen, um die Übergabe zu unterstützen.

### <a name="continuing-user-activities"></a>Fortsetzen von Benutzeraktivitäten

Die `NSUserActivity`-Klasse (zusammen mit einigen kleinen Änderungen an `UIKit` und `AppKit`) bietet Unterstützung für das Definieren der Aktivität eines Benutzers, die auf einem anderen Gerät der Benutzer möglicherweise fortgesetzt werden kann.

Damit eine Aktivität an eine andere Geräte des Benutzers übergeben werden kann, muss Sie in einer Instanz `NSUserActivity`gekapselt werden, die als _aktuelle Aktivität_gekennzeichnet ist, deren Nutz Last festgelegt ist (die zum Ausführen der Fortsetzung verwendeten Daten), und die Aktivität muss dann an gesendet werden. das Gerät.

Handoff übergibt das Minimum an Informationen, um die fort zufügende Aktivität zu definieren, wobei größere Datenpakete über icloud synchronisiert werden.

Auf dem empfangenden Gerät erhält der Benutzer eine Benachrichtigung, dass eine Aktivität für die Fortsetzung verfügbar ist. Wenn sich der Benutzer entscheidet, die Aktivität auf dem neuen Gerät fortzusetzen, wird die angegebene App gestartet (wenn Sie nicht bereits ausgeführt wird), und die Nutzlast des `NSUserActivity` wird verwendet, um die Aktivität neu zu starten.

[![](handoff-images/handoffinteractions.png "An overview of Continuing User Activities")](handoff-images/handoffinteractions.png#lightbox)

Nur apps, die dieselbe Entwickler-Team-ID verwenden und auf einen bestimmten _Aktivitätstyp_ reagieren, sind für die Fortsetzung berechtigt. Eine APP definiert die Aktivitätstypen, die Sie unter dem `NSUserActivityTypes`-Schlüssel der Datei " **Info. plist** " unterstützt. Wenn dies der Folge ist, wählt ein fortgesetztes Gerät die APP für die Ausführung der Fortsetzung basierend auf der Team-ID, dem Aktivitätstyp und optional dem _Aktivitäts Titel_

Die empfangende App verwendet Informationen aus dem `UserInfo` Wörterbuch der `NSUserActivity`, um die Benutzeroberfläche zu konfigurieren und den Status der angegebenen Aktivität wiederherzustellen, sodass der Übergang nahtlos für den Endbenutzer angezeigt wird.

Wenn die Fortsetzung mehr Informationen erfordert, als effizient über einen `NSUserActivity`gesendet werden können, kann die fort Setz Ende APP einen aufzurufenden App-Vorgang senden und einen oder mehrere Streams einrichten, um die erforderlichen Daten zu übertragen. Wenn die Aktivität z. b. ein umfangreiches Textdokument mit mehreren Bildern bearbeitet hat, wäre das Streaming erforderlich, um die erforderlichen Informationen zu übertragen, um die Aktivität auf dem empfangenden Gerät fortzusetzen. Weitere Informationen finden Sie weiter unten im Abschnitt [unterstützende Fortsetzungs Datenströme](#supporting-continuation-streams) .

Wie oben bereits erwähnt, haben `NSDocument` oder `UIDocument` basierte Apps automatisch die Handoff-Unterstützung. Weitere Informationen finden Sie weiter unten [im Abschnitt unterstützende Übergabe in Dokument basierten apps](#supporting-handoff-in-document-based-apps) .

### <a name="the-nsuseractivity-class"></a>Die nsuseractivity-Klasse

Die `NSUserActivity`-Klasse ist das primäre Objekt in einem Übergabe Austausch und wird verwendet, um den Zustand einer Benutzeraktivität zu kapseln, die für die Fortsetzung verfügbar ist. Eine APP instanziiert eine Kopie der `NSUserActivity` für jede Aktivität, die Sie unterstützt, und möchte auf einem anderen Gerät fortgesetzt werden. Der Dokument-Editor erstellt z. b. eine Aktivität für jedes aktuell geöffnete Dokument. Allerdings ist nur das vorderste Dokument (angezeigt in der Vorderseite oder Registerkarte) die _aktuelle Aktivität_ und ist für die Fortsetzung verfügbar.

Eine Instanz von `NSUserActivity` wird durch die Eigenschaften `ActivityType` und `Title` identifiziert. Die `UserInfo` Dictionary-Eigenschaft wird zum Übertragen von Informationen über den Status der Aktivität verwendet. Legen Sie die `NeedsSave`-Eigenschaft auf `true` fest, wenn Sie die Zustandsinformationen über den Delegaten des `NSUserActivity`verzögert laden möchten. Verwenden Sie die `AddUserInfoEntries`-Methode, um neue Daten von anderen Clients in das `UserInfo` Wörterbuch zusammenzuführen, um den Status der Aktivität beizubehalten.

### <a name="the-nsuseractivitydelegate-class"></a>Die nsuseractivitydelegatklasse

Der `NSUserActivityDelegate` wird verwendet, um die Informationen im `UserInfo`-Wörterbuch eines `NSUserActivity`auf dem aktuellen Stand zu halten und mit dem aktuellen Status der Aktivität synchronisiert zu werden. Wenn das System die Informationen in der zu aktualisierenden Aktivität benötigt (z. b. vor der Fortsetzung auf einem anderen Gerät), wird die `UserActivityWillSave`-Methode des Delegaten aufgerufen.

Sie müssen die `UserActivityWillSave`-Methode implementieren und Änderungen an der `NSUserActivity` vornehmen (z. b. `UserInfo`, `Title`usw.), um sicherzustellen, dass der Status der aktuellen Aktivität weiterhin widergespiegelt wird. Wenn das System die `UserActivityWillSave`-Methode aufruft, wird das `NeedsSave`-Flag gelöscht. Wenn Sie die Daten Eigenschaften der Aktivität ändern, müssen Sie `NeedsSave` erneut `true` festlegen.

Anstatt die oben dargestellte `UserActivityWillSave`-Methode zu verwenden, können Sie optional `UIKit` oder `AppKit` die Benutzeraktivität automatisch verwalten. Legen Sie hierzu die `UserActivity`-Eigenschaft des Beantworter-Objekts fest, und implementieren Sie die `UpdateUserActivityState`-Methode. Weitere Informationen finden Sie weiter unten [im Abschnitt unterstützende Handoff in Responders](#supporting-handoff-in-responders) .

### <a name="app-framework-support"></a>App Framework-Unterstützung

Sowohl `UIKit` (IOS) als auch `AppKit` (OS X) bieten integrierte Unterstützung für die Übergabe in den Klassen `NSDocument`, Responder (`UIResponder`/`NSResponder`) und `AppDelegate`. Obwohl jedes Betriebssystem die Übergabe etwas anders implementiert, sind der grundlegende Mechanismus und die APIs identisch.

#### <a name="user-activities-in-document-based-apps"></a>Benutzeraktivitäten in Dokument basierten apps

Für Dokument basierte IOS-und OS X-Apps ist die automatische Übergabe Unterstützung integriert. Um diese Unterstützung zu aktivieren, müssen Sie für jeden `CFBundleDocumentTypes` Eintrag in der **Info. plist** -Datei der APP einen `NSUbiquitousDocumentUserActivityType`-Schlüssel und-Wert hinzufügen.

Wenn dieser Schlüssel vorhanden ist, erstellen sowohl `NSDocument` als auch `UIDocument` automatisch `NSUserActivity` Instanzen für auf icloud basierende Dokumente des angegebenen Typs. Sie müssen für jeden Dokumenttyp, der von der App unterstützt wird, einen Aktivitätstyp bereitstellen, und mehrere Dokumenttypen können denselben Aktivitätstyp verwenden. Sowohl `NSDocument` als auch `UIDocument` werden die `UserInfo`-Eigenschaft der `NSUserActivity` automatisch mit dem Wert ihrer `FileURL`-Eigenschaft aufgefüllt.

Unter OS X wird der `NSUserActivity`, der von `AppKit` verwaltet wird und Responder zugeordnet ist, automatisch zur aktuellen Aktivität, wenn das Dokument Fenster zum Hauptfenster wird. Unter IOS müssen Sie für `NSUserActivity` Objekte, die von `UIKit`verwaltet werden, entweder explizit `BecomeCurrent`-Methode oder die `UserActivity`-Eigenschaft des Dokuments auf einem `UIViewController` festlegen, wenn die app in den Vordergrund steht.

`AppKit` werden alle `UserActivity` Eigenschaften, die auf diese Weise auf OS X erstellt werden, automatisch wieder hergestellt. Dies tritt auf, wenn die `ContinueUserActivity`-Methode `false` zurückgibt oder wenn Sie nicht implementiert ist. In dieser Situation wird das Dokument mit der `OpenDocument`-Methode des `NSDocumentController` geöffnet, und es wird dann ein `RestoreUserActivityState`-Methoden Aufrufes empfangen.

Weitere Informationen finden Sie weiter unten [im Abschnitt unterstützende Übergabe von Dokument basierten apps](#supporting-handoff-in-document-based-apps) .

#### <a name="user-activities-and-responders"></a>Benutzeraktivitäten und-Responder

Sowohl `UIKit` als auch `AppKit` können eine Benutzeraktivität automatisch verwalten, wenn Sie Sie als `UserActivity` Eigenschaft eines Beantworter-Objekts festlegen. Wenn der Zustand geändert wurde, müssen Sie die `NeedsSave`-Eigenschaft der `UserActivity` des Responders auf `true`festlegen. Das System speichert die `UserActivity` bei Bedarf automatisch, nachdem die Antwortzeit zum Aktualisieren des Zustands durch Aufrufen seiner `UpdateUserActivityState`-Methode fest gegeben wurde.

Wenn mehrere Antwort Empfänger eine einzelne `NSUserActivity` Instanz gemeinsam nutzen, erhalten Sie einen `UpdateUserActivityState` Rückruf, wenn das System das Benutzer Aktivitäts Objekt aktualisiert. Der-Beantworter muss die `AddUserInfoEntries`-Methode aufrufen, um das `UserInfo` Wörterbuch des `NSUserActivity`zu aktualisieren, um den aktuellen Aktivitäts Zustand an diesem Punkt widerzuspiegeln. Das `UserInfo` Wörterbuch wird vor jedem `UpdateUserActivityState` aufgerufen.

Um die Zuordnung zu einer Aktivität aufzuheben, kann ein Beantworter seine `UserActivity`-Eigenschaft auf `null`festlegen. Wenn eine von App Framework verwaltete `NSUserActivity`-Instanz nicht mehr zugeordnete Responder oder Dokumente enthält, wird Sie automatisch ungültig.

Weitere Informationen finden Sie weiter unten [im Abschnitt unterstützende Handoff in Responders](#supporting-handoff-in-responders) .

#### <a name="user-activities-and-the-appdelegate"></a>Benutzeraktivitäten und der appdelegat

Die `AppDelegate` Ihrer APP ist der primäre Einstiegspunkt, wenn Sie eine Übergabe Fortsetzung verarbeiten. Wenn der Benutzer auf eine Übergabe Benachrichtigung antwortet, wird die entsprechende App gestartet (wenn Sie nicht bereits ausgeführt wird), und die `WillContinueUserActivityWithType`-Methode der `AppDelegate` wird aufgerufen. An diesem Punkt sollte die APP den Benutzer darüber informieren, dass die Fortsetzung gestartet wird.

Die `NSUserActivity` Instanz wird übermittelt, wenn die `ContinueUserActivity`-Methode des `AppDelegate`aufgerufen wird. An diesem Punkt sollten Sie die Benutzeroberfläche der App konfigurieren und die angegebene Aktivität fortsetzen.

Weitere Informationen finden Sie weiter unten im Abschnitt [Implementieren von Handoff](#implementing-handoff) .

## <a name="enabling-handoff-in-a-xamarin-app"></a>Aktivieren von "Handoff" in einer xamarin-App

Aufgrund der Sicherheitsanforderungen, die durch die Übergabe erzwungen werden, muss eine xamarin. IOS-APP, die das Handoff-Framework verwendet, ordnungsgemäß im Apple-Entwickler Portal und in der xamarin. IOS-Projektdatei konfiguriert werden.

Führen Sie folgende Schritte aus:

1. Melden Sie sich beim [Apple Developer Portal](https://developer.apple.com)an.
2. Klicken Sie auf **Zertifikate, Bezeichner & profile**.
3. Wenn Sie dies nicht bereits getan haben, klicken Sie auf Bezeichner **, und erstellen** Sie eine ID für Ihre APP (z. b. `com.company.appname`), und bearbeiten Sie die vorhandene ID.
4. Stellen Sie sicher, dass der **icloud** -Dienst auf die angegebene ID geprüft wurde:

    [![](handoff-images/provision01.png "Enable the iCloud service for the given ID")](handoff-images/provision01.png#lightbox)
5. Speichern Sie die Änderungen.
6. Klicken Sie auf **Bereitstellungs profile** > **Entwicklung** , und erstellen Sie ein neues Entwicklungs Bereitstellungs Profil für Ihre APP:

    [![](handoff-images/provision02.png "Create a new development provisioning profile for the app")](handoff-images/provision02.png#lightbox)
7. Sie können das neue Bereitstellungs Profil herunterladen und installieren oder das Profil mit Xcode herunterladen und installieren.
8. Bearbeiten Sie die xamarin. IOS-Projektoptionen, und stellen Sie sicher, dass Sie das soeben erstellte Bereitstellungs Profil verwenden:

    [![](handoff-images/provision03.png "Select the provisioning profile just created")](handoff-images/provision03.png#lightbox)
9. Bearbeiten Sie als nächstes die Datei " **Info. plist** ", und stellen Sie sicher, dass Sie die APP-ID verwenden, die zum Erstellen des Bereitstellungs Profils verwendet wurde:

    [![](handoff-images/provision04.png "Set App ID")](handoff-images/provision04.png#lightbox)
10. Scrollen Sie zum Abschnitt **hintergrundmodi** , und überprüfen Sie die folgenden Elemente:

    [![](handoff-images/provision05.png "Enable the required background modes")](handoff-images/provision05.png#lightbox)
11. Speichern Sie die Änderungen an allen Dateien.

Wenn diese Einstellungen vorhanden sind, kann die Anwendung jetzt auf die Handoff-Framework-APIs zugreifen. Ausführliche Informationen zur Bereitstellung finden Sie in unseren [Hand](~/ios/get-started/installation/device-provisioning/index.md) Büchern zur [Geräte Bereitstellung](~/ios/get-started/installation/device-provisioning/index.md) und-Bereitstellung.

## <a name="implementing-handoff"></a>Implementieren von Handoff

Benutzeraktivitäten können in apps fortgesetzt werden, die mit derselben Entwickler-Team-ID signiert sind und denselben Aktivitätstyp unterstützen. Zum Implementieren von Handoff in einer xamarin. IOS-App müssen Sie ein Benutzer Aktivitäts Objekt erstellen (entweder in `UIKit` oder `AppKit`), den Zustand des Objekts aktualisieren, um die Aktivität zu verfolgen, und die Aktivität auf einem empfangenden Gerät fortsetzen.

### <a name="identifying-user-activities"></a>Identifizieren von Benutzeraktivitäten

Der erste Schritt bei der Implementierung von Handoff besteht darin, die Typen von Benutzeraktivitäten zu identifizieren, die ihre App unterstützt, und zu sehen, welche dieser Aktivitäten gute Kandidaten für die Fortsetzung auf einem anderen Gerät sind. Beispiel: eine ToDo-APP unterstützt möglicherweise die Bearbeitung von Elementen als einen _Benutzer Aktivitätstyp_und unterstützt das Durchsuchen der Liste verfügbarer Elemente als andere.

Eine APP kann beliebig viele Benutzer Aktivitätstypen erstellen, und zwar eine für jede Funktion, die die APP bereitstellt. Für jeden benutzeraktivitätstyp muss die APP nachverfolgen, wann eine Aktivität des Typs beginnt und endet, und aktuelle Zustandsinformationen aufbewahren müssen, um diese Aufgabe auf einem anderen Gerät fortsetzen zu können.

Benutzeraktivitäten können für jede APP fortgesetzt werden, die mit derselben Team-ID signiert ist, ohne dass eine eins-zu-Eins-Zuordnung zwischen den sendenden und empfangenden apps besteht. Beispielsweise kann eine bestimmte App vier verschiedene Arten von Aktivitäten erstellen, die von unterschiedlichen, einzelnen apps auf einem anderen Gerät genutzt werden. Dies ist ein häufiges Vorkommen zwischen einer Mac-Version der APP (die viele Features und Funktionen aufweisen kann) und IOS-apps, wobei jede APP kleiner ist und sich auf eine bestimmte Aufgabe konzentriert.

### <a name="creating-activity-type-identifiers"></a>Erstellen von Aktivitätstyp bezeichlern

Der _Aktivitätstyp Bezeichner_ ist eine kurze Zeichenfolge, die dem `NSUserActivityTypes`-Array der **Info. plist** -Datei der app hinzugefügt wird, die zur eindeutigen Identifizierung eines bestimmten Benutzer Aktivitäts Typs verwendet wird. Für jede Aktivität, die von der App unterstützt wird, gibt es einen Eintrag im Array. Apple schlägt vor, eine Notation im Reverse-DNS-Stil für den Aktivitätstyp Bezeichner zu verwenden, um Konflikte zu vermeiden. Beispiel: `com.company-name.appname.activity` für bestimmte App-basierte Aktivitäten oder `com.company-name.activity` für Aktivitäten, die über mehrere apps hinweg ausgeführt werden können.

Der Aktivitätstyp Bezeichner wird beim Erstellen einer `NSUserActivity`-Instanz verwendet, um den Typ der Aktivität zu identifizieren. Wenn eine Aktivität auf einem anderen Gerät fortgesetzt wird, bestimmt der Aktivitätstyp (zusammen mit der Team-ID der APP), welche App gestartet werden soll, um die Aktivität fortzusetzen.

Als Beispiel erstellen wir eine Beispiel-App namens **monkeybrowser** ([hier herunterladen](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-monkeybrowser)). Diese APP zeigt vier Registerkarten an, die jeweils über eine andere URL verfügen, die in einer Webbrowser Ansicht geöffnet ist. Der Benutzer kann jede Registerkarte auf einem anderen ios-Gerät, auf dem die app ausgeführt wird, fortsetzen.

Um die erforderlichen Aktivitätstypen Bezeichner zu erstellen, um dieses Verhalten zu unterstützen, bearbeiten Sie die Datei **Info. plist** , und wechseln Sie zur **Quell** Ansicht. Fügen Sie einen `NSUserActivityTypes` Schlüssel hinzu, und erstellen Sie die folgenden Bezeichner:

[![](handoff-images/type01.png "The NSUserActivityTypes key and required identifiers in the plist editor")](handoff-images/type01.png#lightbox)

Wir haben vier neue Aktivitätstyp Bezeichner erstellt, einen für jede Registerkarte in der Beispiel-APP **monkeybrowser** . Wenn Sie eigene Apps erstellen, ersetzen Sie den Inhalt des `NSUserActivityTypes` Arrays durch die Aktivitätstyp-IDs, die für die von Ihrer APP unterstützten Aktivitäten spezifisch sind.

### <a name="tracking-user-activity-changes"></a>Nachverfolgen von Änderungen an Benutzeraktivitäten

Beim Erstellen einer neuen Instanz der `NSUserActivity` Klasse geben wir eine `NSUserActivityDelegate` Instanz an, um Änderungen am Zustand der Aktivität zu verfolgen. Der folgende Code kann z. b. zum Nachverfolgen von Zustandsänderungen verwendet werden:

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

Die `UserActivityReceivedData`-Methode wird aufgerufen, wenn ein Fortsetzungs Datenstrom Daten von einem sendenden Gerät empfangen hat. Weitere Informationen finden Sie weiter unten im Abschnitt [unterstützende Fortsetzungs Datenströme](#supporting-continuation-streams) .

Die `UserActivityWasContinued`-Methode wird aufgerufen, wenn ein anderes Gerät eine Aktivität vom aktuellen Gerät übernommen hat. Abhängig vom Typ der Aktivität, wie z. b. Hinzufügen eines neuen Elements zu einer TODO-Liste, muss die APP möglicherweise die Aktivität auf dem sendenden Gerät abbrechen.

Die `UserActivityWillSave`-Methode wird aufgerufen, bevor Änderungen an der Aktivität gespeichert und über lokal verfügbare Geräte synchronisiert werden. Mit dieser Methode können Sie Änderungen an der `UserInfo`-Eigenschaft der `NSUserActivity` Instanz vornehmen, bevor Sie gesendet wird.

### <a name="creating-a-nsuseractivity-instance"></a>Erstellen einer nsuseractivity-Instanz

Jede Aktivität, die Ihre APP bereitstellen möchte, um auf einem anderen Gerät fortzufahren, muss in einer `NSUserActivity`-Instanz gekapselt werden. Die APP kann beliebig viele Aktivitäten erstellen, und die Art dieser Aktivitäten hängt von der Funktionalität und den Features der betreffenden App ab. Eine e-Mail-app könnte z. b. eine Aktivität zum Erstellen einer neuen Nachricht und eine weitere zum Lesen einer Nachricht erstellen.

Bei der Beispiel-APP wird jedes Mal eine neue `NSUserActivity` erstellt, wenn der Benutzer in einer der Webbrowser Ansicht im Registerkarten Format eine neue URL eingibt. Der folgende Code speichert den Status einer bestimmten Registerkarte:

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

Er erstellt eine neue `NSUserActivity` mithilfe eines der oben erstellten benutzeraktivitätstyp und stellt einen lesbaren Titel für die Aktivität bereit. Sie wird an eine Instanz der oben erstellten `NSUserActivityDelegate` angefügt, um Zustandsänderungen zu überwachen, und informiert IOS darüber, dass diese Benutzeraktivität die aktuelle Aktivität ist.

### <a name="populating-the-userinfo-dictionary"></a>Auffüllen des userinfo-Wörterbuchs

Wie bereits erwähnt, ist die `UserInfo`-Eigenschaft der `NSUserActivity`-Klasse eine `NSDictionary` von Schlüssel-Wert-Paaren, die verwendet werden, um den Status einer bestimmten Aktivität zu definieren. Die in `UserInfo` gespeicherten Werte müssen einen der folgenden Typen aufweisen: `NSArray`, `NSData`, `NSDate`, `NSDictionary`, `NSNull`, `NSNumber`, `NSSet`, `NSString`oder `NSURL`. `NSURL` Datenwerte, die auf icloud-Dokumente verweisen, werden automatisch so angepasst, dass Sie auf die gleichen Dokumente auf einem empfangenden Gerät zeigen.

Im obigen Beispiel haben wir ein `NSMutableDictionary` Objekt erstellt und mit einem einzelnen Schlüssel aufgefüllt, der die URL bereitstellt, die der Benutzer zurzeit auf der angegebenen Registerkarte anzeigen hat. Die `AddUserInfoEntries`-Methode der User-Aktivität wurde verwendet, um die Aktivität mit den Daten zu aktualisieren, die zum Wiederherstellen der Aktivität auf dem empfangenden Gerät verwendet werden:

```csharp
// Update the activity when the tab's URL changes
var userInfo = new NSMutableDictionary ();
userInfo.Add (new NSString ("Url"), new NSString (url));
UserActivity.AddUserInfoEntries (userInfo);
```

Apple schlägt vor, die gesendeten Informationen an den geringsten Minimalwert zu halten, um sicherzustellen, dass die Aktivität rechtzeitig an das empfangende Gerät gesendet wird. Wenn größere Informationen erforderlich sind, wie z. b. ein an ein Dokument angefügtes Bild, das bearbeitet werden muss, müssen Sie Fortsetzungs Datenströme verwenden. Weitere Informationen finden Sie weiter unten im Abschnitt [unterstützende Fortsetzungs Datenströme](#supporting-continuation-streams) .

### <a name="continuing-an-activity"></a>Fortsetzen einer Aktivität

Bei der Übergabe werden automatisch lokale IOS-und OS X-Geräte, die sich in physischer Nähe zum ursprünglichen Gerät befinden und bei demselben icloud-Konto angemeldet sind, von der Verfügbarkeit von Fort Setz baren Benutzeraktivitäten informiert. Wenn sich der Benutzer entscheidet, eine Aktivität auf einem neuen Gerät fortzusetzen, startet das System die entsprechende app (basierend auf der Team-ID und dem Aktivitätstyp) sowie Informationen zu dessen `AppDelegate`, dass die Fortsetzung erfolgen muss.

Zuerst wird die `WillContinueUserActivityWithType`-Methode aufgerufen, sodass die APP den Benutzer darüber informieren kann, dass die Fortsetzung gerade beginnt. Der folgende Code wird in der **AppDelegate.cs** -Datei der Beispiel-App verwendet, um eine Fortsetzung zu behandeln:

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

Im obigen Beispiel wird jeder Ansichts Controller beim `AppDelegate` registriert und verfügt über eine öffentliche `PreparingToHandoff` Methode, die einen Aktivitätsindikator anzeigt, und eine Meldung, die den Benutzer darüber informiert, dass die Aktivität dem aktuellen Gerät übergeben werden soll. Beispiel:

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

Der `ContinueUserActivity` der `AppDelegate` wird aufgerufen, um die angegebene Aktivität tatsächlich fortzusetzen. Auch in unserer Beispiel-App:

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

Die öffentliche `PerformHandoff`-Methode jedes Ansichts Controllers führt den Handler tatsächlich aus und stellt die Aktivität auf dem aktuellen Gerät wieder her. Im Beispiel wird die gleiche URL auf einer bestimmten Registerkarte angezeigt, die der Benutzer auf einem anderen Gerät durchsucht hat. Beispiel:

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

Die `ContinueUserActivity`-Methode enthält eine `UIApplicationRestorationHandler`, die Sie für das Fortsetzen von Dokumenten oder Beantworter-Aktivität aufrufen können. Wenn Sie aufgerufen werden, müssen Sie eine `NSArray` oder wiederherstellbare Objekte an den Wiederherstellungs Handler übergeben. Beispiel:

```csharp
completionHandler (new NSObject[]{Tab4});
```

Für jedes Objekt, das erfolgreich ist, wird die `RestoreUserActivityState`-Methode aufgerufen. Jedes-Objekt kann dann die Daten im `UserInfo` Wörterbuch verwenden, um seinen eigenen Zustand wiederherzustellen. Beispiel:

```csharp
public override void RestoreUserActivityState (NSUserActivity activity)
{
    base.RestoreUserActivityState (activity);

    // Log activity
    Console.WriteLine ("Restoring Activity {0}", activity.Title);
}
```

Wenn Sie für Dokument basierte Apps die `ContinueUserActivity` Methode nicht implementieren oder `false`zurückgibt, kann `UIKit` oder `AppKit` die Aktivität automatisch fortsetzen. Weitere Informationen finden Sie weiter unten [im Abschnitt unterstützende Übergabe von Dokument basierten apps](#supporting-handoff-in-document-based-apps) .

### <a name="failing-handoff-gracefully"></a>Fehlerhafte Übergabe

Da bei der Übergabe von Informationen zwischen einer Sammlung lose verbundene IOS-und OS X-Geräte übertragen werden, kann der Übertragungsvorgang manchmal fehlschlagen. Sie sollten Ihre APP so entwerfen, dass diese Fehler ordnungsgemäß behandelt werden, und den Benutzer über alle auftretenden Situationen informieren.

Im Fall eines Fehlers wird die `DidFailToContinueUserActivitiy`-Methode des `AppDelegate` aufgerufen. Beispiel:

```csharp
public override void DidFailToContinueUserActivitiy (UIApplication application, string userActivityType, NSError error)
{
    // Log information about the failure
    Console.WriteLine ("User Activity {0} failed to continue. Error: {1}", userActivityType, error.LocalizedDescription);
}
```

Verwenden Sie die bereitgestellten `NSError`, um dem Benutzerinformationen über den Fehler bereitzustellen.

## <a name="native-app-to-web-browser-handoff"></a>Übergabe von System eigenem APP-zu-Webbrowser

Ein Benutzer möchte möglicherweise eine Aktivität fortsetzen, ohne dass eine entsprechende Native App auf dem gewünschten Gerät installiert sein muss. In einigen Situationen kann eine webbasierte Schnittstelle die erforderliche Funktionalität bereitstellen, und die Aktivität kann weiterhin fortgesetzt werden. Beispielsweise kann das e-Mail-Konto des Benutzers eine Web-Base-Benutzeroberfläche zum Verfassen und Lesen von Nachrichten bereitstellen.

Wenn die ursprüngliche, systemeigene APP die URL für die Weboberfläche kennt (und die erforderliche Syntax zum Identifizieren des angegebenen Elements ist), kann Sie diese Informationen in der `WebpageURL`-Eigenschaft der `NSUserActivity` Instanz codieren. Wenn auf dem empfangenden Gerät keine geeignete Native App installiert ist, um die Fortsetzung zu verarbeiten, kann die bereitgestellte Weboberfläche aufgerufen werden.

## <a name="web-browser-to-native-app-handoff"></a>Webbrowser zu nativer App-Übergabe

Wenn der Benutzer eine webbasierte Schnittstelle auf dem Ursprungs Gerät verwendet und eine native App auf dem empfangenden Gerät den Domänen Teil der `WebpageURL`-Eigenschaft beansprucht, verwendet das System diese APP, um die Fortsetzung zu verarbeiten. Das neue Gerät empfängt eine `NSUserActivity`-Instanz, die den Aktivitätstyp als `BrowsingWeb` markiert und die `WebpageURL` die URL enthält, die der Benutzer besucht hat. das `UserInfo` Wörterbuch ist leer.

Damit eine APP an dieser Art von Übergabe teilnehmen kann, muss Sie die Domäne in einer `com.apple.developer.associated-domains` Berechtigung mit dem Format `<service>:<fully qualified domain name>` beanspruchen (z. b. `activity continuation:company.com`).

Wenn die angegebene Domäne mit dem Wert einer `WebpageURL` Eigenschaft übereinstimmt, wird von Handoff eine Liste der genehmigten App-IDs von der Website in dieser Domäne heruntergeladen. Die Website muss eine Liste genehmigter IDs in einer signierten JSON-Datei mit dem Namen **Apple-App-Site-Association** (z. b. `https://company.com/apple-app-site-association`) bereitstellen.

Diese JSON-Datei enthält ein Wörterbuch, das eine Liste der APP-IDs im Formular `<team identifier>.<bundle identifier>`angibt. Beispiel:

```csharp
{
    "activitycontinuation": {
        "apps": [    "YWBN8XTPBJ.com.company.FirstApp",
            "YWBN8XTPBJ.com.company.SecondApp" ]
    }
}
```

Um die JSON-Datei zu signieren (sodass Sie die richtige `Content-Type` `application/pkcs7-mime`) hat, verwenden Sie die **Terminal** -APP und einen `openssl`-Befehl mit einem Zertifikat und einem Schlüssel, die von einer Zertifizierungsstelle ausgestellt wurden, die von IOS als vertrauenswürdig eingestuft wird (eine Liste finden Sie unter [https://support.apple.com/kb/ht5012](https://support.apple.com/kb/ht5012) Beispiel:

```csharp
echo '{"activitycontinuation":{"apps":["YWBN8XTPBJ.com.company.FirstApp",
"YWBN8XTPBJ.com.company.SecondApp"]}}' > json.txt

cat json.txt | openssl smime -sign -inkey company.com.key
-signer company.com.pem
-certfile intermediate.pem
-noattr -nodetach
-outform DER > apple-app-site-association
```

Der Befehl "`openssl`" gibt eine signierte JSON-Datei aus, die Sie auf Ihrer Website unter der **Apple-App-Site-Association-** URL platzieren. Beispiel:

```csharp
https://example.com/apple-app-site-association.
```

Die App empfängt eine beliebige Aktivität, deren `WebpageURL` Domäne `com.apple.developer.associated-domains` Berechtigung hat. Es werden nur die Protokolle `http` und `https` unterstützt, alle anderen Protokolle stellen eine Ausnahme dar.

## <a name="supporting-handoff-in-document-based-apps"></a>Unterstützende Übergabe in Dokument basierten apps

Wie oben bereits erwähnt, unterstützen Dokument basierte apps unter IOS und OS X automatisch die Übergabe von icloud-basierten Dokumenten, wenn die Datei " **Info. plist** " der APP einen `CFBundleDocumentTypes` Schlüssel `NSUbiquitousDocumentUserActivityType`enthält. Beispiel:

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

In diesem Beispiel ist die Zeichenfolge ein Reverse-DNS-App-Kenn Zeichner mit dem angefügten Namen der Aktivität. Wenn diese Methode auf diese Weise eingegeben wird, müssen die Aktivitätstypen Einträge nicht im `NSUserActivityTypes`-Array der **Info. plist** -Datei wiederholt werden.

Auf das automatisch erstellte Benutzer Aktivitäts Objekt (das über die `UserActivity`-Eigenschaft des Dokuments verfügbar ist) kann von anderen Objekten in der APP verwiesen und zum Wiederherstellen des Zustands bei der Fortsetzung verwendet werden. Beispielsweise, um die Elementauswahl und die Dokument Position zu verfolgen. Sie müssen diese Aktivitäten `NeedsSave`-Eigenschaft auf `true` festlegen, wenn sich der Status ändert, und das `UserInfo`-Wörterbuch in der `UpdateUserActivityState`-Methode aktualisieren.

Die `UserActivity`-Eigenschaft kann von jedem Thread verwendet werden und entspricht dem KVO-Protokoll (Key-Value Beobachtung). Daher kann Sie verwendet werden, um ein Dokument synchron zu halten, während es in die icloud übergeht. Die `UserActivity`-Eigenschaft wird für ungültig erklärt, wenn das Dokument geschlossen wird.

Weitere Informationen finden Sie in der Dokumentation zur Benutzeraktivität von Apple in der Dokumentation zu [Dokument basierten apps](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Handoff/HandoffFundamentals/HandoffFundamentals.html#//apple_ref/doc/uid/TP40014338-CH3-SW5) .

## <a name="supporting-handoff-in-responders"></a>Unterstützende Übergabe in Responders

Sie können Responder (geerbt von `UIResponder` unter IOS oder `NSResponder` unter OS X) zu Aktivitäten zuordnen, indem Sie deren `UserActivity` Eigenschaften festlegen. Das System speichert die `UserActivity`-Eigenschaft automatisch zu den entsprechenden Zeitpunkten, indem er die `UpdateUserActivityState`-Methode des Responders aufruft, um dem Benutzer Aktivitäts Objekt mithilfe der `AddUserInfoEntriesFromDictionary`-Methode aktuelle Daten hinzuzufügen.

## <a name="supporting-continuation-streams"></a>Unterstützende Fortsetzungs Ströme

Dies kann vorkommen, wenn die Menge der Informationen, die zum Fortsetzen einer Aktivität erforderlich sind, von der anfänglichen Handoff-Nutzlast nicht effizient übertragen werden kann. In diesen Fällen kann die empfangende APP einen oder mehrere Streams zwischen sich selbst und der Ursprungs-App zum Übertragen der Daten einrichten.

Die ursprüngliche App legt die `SupportsContinuationStreams`-Eigenschaft der `NSUserActivity` Instanz auf `true`fest. Beispiel:

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

Die empfangende App kann dann die `GetContinuationStreams`-Methode der `NSUserActivity` im `AppDelegate` zum Einrichten des Streams aufzurufen. Beispiel:

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

Auf dem Ursprungs Gerät empfängt der Benutzeraktivitäts-Delegat die Datenströme durch Aufrufen seiner `DidReceiveInputStream`-Methode, um die angeforderten Daten bereitzustellen, um die Benutzeraktivität auf dem fort Setz enden Gerät fortzusetzen.

Sie verwenden eine `NSInputStream`, um schreibgeschützten Zugriff auf Streamdaten bereitzustellen, und eine `NSOutputStream` die schreibgeschützten Zugriff bereitstellen. Die Streams sollten in Anforderungs-und Antwort Weise verwendet werden, wobei die empfangende app mehr Daten anfordert und die ursprüngliche app Sie bereitstellt. Das heißt, dass Daten, die in den Ausgabestream auf dem ursprünglichen Gerät geschrieben werden, aus dem Eingabestream auf dem fortlaufenden Gerät gelesen werden und umgekehrt.

Auch in Situationen, in denen der Fortsetzungs Datenstrom erforderlich ist, sollte die Kommunikation zwischen den beiden apps minimal sein.

Weitere Informationen finden Sie in der Dokumentation zur [Verwendung von Fort](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Handoff/AdoptingHandoff/AdoptingHandoff.html#//apple_ref/doc/uid/TP40014338-CH2-SW13) setzungen in Apple.

## <a name="handoff-best-practices"></a>Bewährte Methoden für die Übergabe

Die erfolgreiche Implementierung der nahtlosen Fortsetzung einer Benutzeraktivität über Handoff erfordert aufgrund der unterschiedlichen Komponenten einen sorgfältigen Entwurf. Apple schlägt vor, die folgenden bewährten Methoden für Ihre Handoff-aktivierten apps zu verwenden:

- Entwerfen Sie Ihre Benutzeraktivitäten so, dass die kleinste Nutzlast erforderlich ist, um den Status der Aktivität zu verknüpfen, damit Sie fortgesetzt werden kann. Je größer die Nutzlast, desto länger dauert die Fortsetzung.
- Wenn Sie große Datenmengen zur erfolgreichen Fortsetzung übertragen müssen, berücksichtigen Sie die Kosten für die Konfiguration und den Netzwerk Aufwand.
- Es ist üblich, dass eine große Mac-app Benutzeraktivitäten erstellt, die von mehreren, kleineren, aufgabenspezifischen apps auf IOS-Geräten verarbeitet werden. Die verschiedenen APP-und Betriebssystemversionen sollten so entworfen werden, dass Sie gut zusammenarbeiten oder ordnungsgemäß fehlschlagen.
- Verwenden Sie beim Angeben der Aktivitätstypen Reverse-DNS-Notation, um Konflikte zu vermeiden. Wenn eine Aktivität für eine bestimmte App spezifisch ist, sollte Ihr Name in der Typdefinition (z. b. `com.myCompany.myEditor.editing`) enthalten sein. Wenn die Aktivität über mehrere apps hinweg funktionieren kann, löschen Sie den APP-Namen aus der Definition (z. b. `com.myCompany.editing`).
- Wenn Ihre APP den Status einer Benutzeraktivität (`NSUserActivity`) aktualisieren muss, legen Sie die Eigenschaft `NeedsSave` auf `true`fest. Zu den entsprechenden Zeitpunkten wird die `UserActivityWillSave`-Methode des Delegaten aufgerufen, sodass Sie das `UserInfo` Wörterbuch nach Bedarf aktualisieren können.
- Da der Übergabeprozess auf dem empfangenden Gerät nicht sofort initialisiert werden kann, sollten Sie die `WillContinueUserActivity` des `AppDelegate`implementieren und den Benutzer darüber informieren, dass eine Fortsetzung gestartet wird.

## <a name="example-handoff-app"></a>Beispiel für eine Handoff-App

Als Beispiel für die Verwendung von Handoff in einer xamarin. IOS-APP haben wir die Beispiel-app " [**monkeybrowser**](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-monkeybrowser) " in dieses Handbuch integriert. Die APP verfügt über vier Registerkarten, mit denen der Benutzer das Web durchsuchen kann, die jeweils einen bestimmten Aktivitätstyp haben: Wetter, Favorit, Kaffeepause und Arbeit.

Wenn der Benutzer auf jeder Registerkarte eine neue URL eingibt und auf die Schaltfläche " **go** " tippt, wird für die Registerkarte eine neue `NSUserActivity` erstellt, die die URL enthält, die der Benutzer gerade durchsucht:

[![](handoff-images/handoff01.png "Example Handoff App")](handoff-images/handoff01.png#lightbox)

Wenn auf einem der Geräte des Benutzers die **monkeybrowser** -App installiert ist, mit demselben Benutzerkonto bei icloud angemeldet ist, sich im selben Netzwerk befindet und sich in der Nähe des obigen Geräts befindet, wird die Handoff-Aktivität auf dem Startbildschirm angezeigt (in der unteren linke Ecke):

[![](handoff-images/handoff02.png "The Handoff Activity displayed on the home screen in the lower left hand corner")](handoff-images/handoff02.png#lightbox)

Wenn der Benutzer das Handoff-Symbol nach oben zieht, wird die APP gestartet, und die im `NSUserActivity` angegebene Benutzeraktivität wird auf dem neuen Gerät fortgesetzt:

[![](handoff-images/handoff03.png "The User Activity continued on the new device")](handoff-images/handoff03.png#lightbox)

Wenn die Benutzeraktivität erfolgreich an ein anderes Apple-Gerät gesendet wurde, empfängt die `NSUserActivity` des sendenden Geräts einen aufzurufenden `UserActivityWasContinued`-Methode auf dem `NSUserActivityDelegate`, damit Sie informiert wird, dass die Benutzeraktivität erfolgreich auf ein anderes Gerät übertragen wurde.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel bietet eine Einführung in das Handoff-Framework, mit dem eine Benutzeraktivität zwischen mehreren der Apple-Geräte des Benutzers fortgesetzt werden kann. Als nächstes haben Sie gezeigt, wie Sie Handoff in einer xamarin. IOS-App aktivieren und implementieren. Schließlich wurden die unterschiedlichen Arten von Fortsetzungen für die Übergabe und die bewährten Methoden für die Übergabe erläutert.

## <a name="related-links"></a>Verwandte Links

- [IOS 9-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [Beispiel für monkeybrowser](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-monkeybrowser)
- [IOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [Neuerungen in ios 9,0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Homekitdeveloper-Handbuch](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html)
- [Homekit-Richtlinien zur Benutzeroberfläche](https://developer.apple.com/homekit/ui-guidelines/)
- [Referenz zum homekit-Framework](https://developer.apple.com/library/ios/home_kit_framework_ref)
