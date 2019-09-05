---
title: Homekit in xamarin. IOS
description: Homekit ist das Apple-Framework zum Steuern von Home Automation-Geräten. In diesem Artikel wird homekit vorgestellt, und es wird erläutert, wie Sie Test Zubehör im homekit-Zubehör Simulator konfigurieren und eine einfache xamarin. IOS-app schreiben, um mit diesen Zubehör zu interagieren.
ms.prod: xamarin
ms.assetid: 90C0C553-916B-46B1-AD52-1E7332792283
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/22/2017
ms.openlocfilehash: f98cd3110719827d8cfeceef4dc9e73776c79f3f
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292715"
---
# <a name="homekit-in-xamarinios"></a>Homekit in xamarin. IOS

_Homekit ist das Apple-Framework zum Steuern von Home Automation-Geräten. In diesem Artikel wird homekit vorgestellt, und es wird erläutert, wie Sie Test Zubehör im homekit-Zubehör Simulator konfigurieren und eine einfache xamarin. IOS-app schreiben, um mit diesen Zubehör zu interagieren._

[![](homekit-images/accessory01.png "Ein Beispiel für eine homekit-aktivierte App")](homekit-images/accessory01.png#lightbox)

Apple hat homekit in ios 8 eingeführt, um mehrere Home-Automatisierungsgeräte nahtlos von verschiedenen Anbietern in eine einzelne, kohärente Einheit zu integrieren. Dank der herauf Stufung eines allgemeinen Protokolls zum ermitteln, konfigurieren und Steuern von Home Automation-Geräten können Geräte von nicht verbundenen Anbietern zusammenarbeiten, ohne dass die einzelnen Lieferanten die Anstrengungen koordinieren müssen.

Mit homekit können Sie eine xamarin. IOS-app erstellen, die alle homekit-aktivierten Geräte steuert, ohne vom Hersteller bereitgestellte APIs oder apps zu verwenden. Mit homekit können Sie folgende Aufgaben ausführen:

- Entdecken Sie neue homekit-aktivierte Home Automation-Geräte, und fügen Sie Sie einer Datenbank hinzu, die auf allen IOS-Geräten des Benutzers persistent gespeichert wird.
- Einrichten, konfigurieren, anzeigen und Steuern beliebiger Geräte in der homekit- _Start Konfigurations Datenbank_.
- Kommunizieren Sie mit jedem vorkonfigurierten homekit-Gerät, und führen Sie die Befehle aus, um einzelne Aktionen auszuführen oder gemeinsam zu arbeiten, wie z. b. das Einschalten aller Lichter in der Küche.

Homekit ermöglicht nicht nur die Bereitstellen von Geräten in der Datenbank der Basiskonfiguration für homekit-aktivierte apps, sondern auch den Zugriff auf Siri-Sprachbefehle. Bei einer ordnungsgemäß konfigurierten homekit-Einrichtung kann der Benutzer Sprachbefehle ausgeben, z. b. "Siri, schalten Sie die Lichter im Wohnraum ein."

<a name="Home-Configuration-Database" />

## <a name="the-home-configuration-database"></a>Die Start Konfigurations Datenbank

Homekit organisiert alle Automatisierungsgeräte an einem bestimmten Ort in einer Home-Sammlung. Diese Sammlung bietet dem Benutzer die Möglichkeit, Ihre Heim Automatisierungsgeräte in logisch anordnen Einheiten mit aussagekräftigen, lesbaren Bezeichnungen zu gruppieren.

Die Sammlung "Home" wird in einer Basis Konfigurationsdatenbank gespeichert, die automatisch auf allen IOS-Geräten des Benutzers gesichert und synchronisiert wird. Homekit bietet die folgenden Klassen für die Arbeit mit der Basis Konfigurationsdatenbank:

- `HMHome`-Dies ist der Container der obersten Ebene, der alle Informationen und Konfigurationen für alle Heim Automatisierungsgeräte an einem einzigen physischen Speicherort enthält (z. b. ein einzelner Familienwohnsitz). Der Benutzer verfügt möglicherweise über mehr als einen Wohnsitz, wie z. b. seinen hauptheim-und Urlaubs Haus. Oder Sie verfügen möglicherweise über unterschiedliche "Häuser" für dieselbe Eigenschaft, z. b. das Haupt Haus und ein Gasthaus über der Garage. In jedem Fall _muss_ mindestens ein `HMHome` Objekt eingerichtet und gespeichert werden, bevor weitere homekit-Informationen eingegeben werden können.
- `HMRoom`-Obwohl `HMRoom` es optional ist, ermöglicht es dem Benutzer, bestimmte Räume innerhalb einer Startseite`HMHome`() zu definieren, wie z. b.: Küche, Bad, Garage oder Wohnraum. Der Benutzer kann alle Home Automation-Geräte an einem bestimmten Ort in seinem Haus in eine `HMRoom` gruppieren und als Einheit darauf reagieren. Beispielsweise wird Siri zum Ausschalten der Garagen Leuchten aufgefordert.
- `HMAccessory`-Dies stellt ein einzelnes, physisches homekit-fähiges Automatisierungsgerät dar, das im Besitz des Benutzers (z. b. einem intelligenten Thermostat) installiert wurde. Jeder `HMAccessory` wird einem `HMRoom`zugewiesen. Wenn der Benutzer keine Räume konfiguriert hat, weist homekit Zubehör einem besonderen Standard Raum zu.
- `HMService`-Stellt einen Dienst dar, der von `HMAccessory`einem angegebenen bereitgestellt wird, z. b. den ein-/aus-Zustand eines Lichts oder seiner Farbe (wenn Farbänderung unterstützt wird). Jede `HMAccessory` kann über mehrere Dienste verfügen, z. b. einen Garage-Türöffner, der ebenfalls ein Licht umfasst. Darüber hinaus kann eine `HMAccessory` angegebene über Dienste wie Firmwareupdates verfügen, die außerhalb der Benutzersteuerung liegen.
- `HMZone`: Ermöglicht es dem Benutzer, eine Auflistung von `HMRoom` Objekten in logische Zonen zu gruppieren, z. b. in der Mitte, nach unten oder im durch Netz Obwohl es optional ist, ermöglicht dies Interaktionen wie das Anfordern von Siri, das gesamte Licht zu deaktivieren.

<a name="Provisioning-a-HomeKit-App" />

## <a name="provisioning-a-homekit-app"></a>Bereitstellen einer homekit-App

Aufgrund der Sicherheitsanforderungen, die von homekit erhoben werden, muss eine xamarin. IOS-APP, die das homekit-Framework verwendet, ordnungsgemäß im Apple-Entwickler Portal und in der xamarin. IOS-Projektdatei konfiguriert werden.

Führen Sie folgende Schritte aus:

1. Melden Sie sich beim [Apple Developer Portal](https://developer.apple.com)an.
2. Klicken Sie auf **Zertifikate, Bezeichner & profile**.
3. Wenn Sie dies noch nicht getan haben, klicken Sie auf Bezeichner **, und erstellen** Sie eine ID für Ihre `com.company.appname`app (z. b.), und bearbeiten Sie andernfalls Ihre vorhandene ID.
4. Stellen Sie sicher, dass der **homekit** -Dienst auf die angegebene ID geprüft wurde: 

    [![](homekit-images/provision01.png "Aktivieren Sie den homekit-Dienst für die angegebene ID.")](homekit-images/provision01.png#lightbox)
5. Speichern Sie die Änderungen.
6. Klicken Sie auf **Bereitstellungs profile** > **Entwicklung** , und erstellen Sie ein neues Entwicklungs Bereitstellungs Profil für Ihre APP: 

    [![](homekit-images/provision02.png "Erstellen eines neuen Entwicklungs Bereitstellungs Profils für die APP")](homekit-images/provision02.png#lightbox)
7. Sie können das neue Bereitstellungs Profil herunterladen und installieren oder das Profil mit Xcode herunterladen und installieren.
8. Bearbeiten Sie die xamarin. IOS-Projektoptionen, und stellen Sie sicher, dass Sie das soeben erstellte Bereitstellungs Profil verwenden: 

    [![](homekit-images/provision03.png "Wählen Sie das soeben erstellte Bereitstellungs Profil aus.")](homekit-images/provision03.png#lightbox)
9. Bearbeiten Sie als nächstes die Datei " **Info. plist** ", und stellen Sie sicher, dass Sie die APP-ID verwenden, die zum Erstellen des Bereitstellungs Profils verwendet wurde: 

    [![](homekit-images/provision04.png "Festlegen der APP-ID")](homekit-images/provision04.png#lightbox)
10. Bearbeiten Sie abschließend die Datei "Berechtigungsdatei" **. plist** , und stellen Sie sicher, dass die **homekit** -Berechtigung ausgewählt wurde: 

    [![](homekit-images/provision05.png "Aktivieren der homekit-Berechtigung")](homekit-images/provision05.png#lightbox)
11. Speichern Sie die Änderungen an allen Dateien.

Wenn diese Einstellungen vorhanden sind, kann die Anwendung jetzt auf die homekit-Framework-APIs zugreifen. Ausführliche Informationen zur Bereitstellung finden Sie in unseren [Hand](~/ios/get-started/installation/device-provisioning/index.md) Büchern zur [Geräte Bereitstellung](~/ios/get-started/installation/device-provisioning/index.md) und-Bereitstellung.

> [!IMPORTANT]
> Zum Testen einer homekit-aktivierten APP ist ein echtes IOS-Gerät erforderlich, das für die Entwicklung ordnungsgemäß bereitgestellt wurde. Homekit kann nicht vom IOS-Simulator getestet werden.

## <a name="the-homekit-accessory-simulator"></a>Der homekit-Zubehör Simulator

Um eine Möglichkeit zum Testen aller möglichen Home Automation-Geräte und-Dienste zu bieten, ohne ein physisches Gerät zu haben, hat Apple den _homekit-Zubehör Simulator_erstellt. Mithilfe dieses Simulators können Sie Virtual homekit-Geräte einrichten und konfigurieren.

### <a name="installing-the-simulator"></a>Installieren des Simulators

Apple stellt den homekit-Zubehör Simulator als separaten Download von Xcode bereit. Sie müssen ihn also installieren, bevor Sie fortfahren.

Führen Sie folgende Schritte aus:

1. Besuchen Sie in einem Webbrowser [Downloads für Apple-Entwickler](https://developer.apple.com/download/more/?name=for%20Xcode) .
2. Laden Sie die **zusätzlichen Tools für Xcode xxx** herunter (wobei xxx die Version von Xcode ist, die Sie installiert haben): 

    [![](homekit-images/simulator01.png "Zusätzliche Tools für XCode herunterladen")](homekit-images/simulator01.png#lightbox)
3. Öffnen Sie das Datenträger Image, und installieren Sie die Tools im **Anwendungs** Verzeichnis.

Beim Installieren des homekit-Zubehör Simulators können virtuelle Zubehör zum Testen erstellt werden.

### <a name="creating-virtual-accessories"></a>Erstellen virtueller Zubehör

Gehen Sie folgendermaßen vor, um den Simulator für homekit-Zubehör zu starten und einige virtuelle Zubehör zu erstellen:

1. Starten Sie im Ordner "Applications" den Simulator für homekit-Zubehör: 

    [![](homekit-images/simulator02.png "Der homekit-Zubehör Simulator")](homekit-images/simulator02.png#lightbox)
2. Klicken Sie **+** auf die Schaltfläche, und wählen Sie **Neues Zubehör...** : 

    [![](homekit-images/simulator03.png "Neues Zubehör hinzufügen")](homekit-images/simulator03.png#lightbox)
3. Füllen Sie die Informationen zum neuen Zubehör aus, und klicken Sie auf die Schaltfläche **Fertig** stellen: 

    [![](homekit-images/simulator04.png "Füllen Sie die Informationen zum neuen Zubehör aus.")](homekit-images/simulator04.png#lightbox)
4. Klicken Sie auf **Dienst hinzufügen.** aus, und wählen Sie einen Diensttyp aus der Dropdown Liste aus: 

    [![](homekit-images/simulator05.png "Wählen Sie einen Diensttyp aus der Dropdown Liste aus.")](homekit-images/simulator05.png#lightbox)
5. Geben Sie einen **Namen** für den Dienst an, und klicken Sie auf **Fertig** stellen: 

    [![](homekit-images/simulator06.png "Geben Sie einen Namen für den Dienst ein.")](homekit-images/simulator06.png#lightbox)
6. Sie können optionale Eigenschaften für einen Dienst bereitstellen, indem Sie auf die Schaltfläche " **Merkmal hinzufügen** " klicken und die erforderlichen Einstellungen konfigurieren: 

    [![](homekit-images/simulator07.png "Konfigurieren der erforderlichen Einstellungen")](homekit-images/simulator07.png#lightbox)
7. Wiederholen Sie die obigen Schritte, um einen der einzelnen Arten von Virtual Home Automation-Geräten zu erstellen, die homekit unterstützt.

Nachdem Sie einige Virtual homekit-Beispiel Zubehör erstellt und konfiguriert haben, können Sie diese Geräte nun in ihrer xamarin. IOS-App nutzen und steuern.

## <a name="configuring-the-infoplist-file"></a>Konfigurieren der Datei "Info. plist"

Neu für IOS 10 (und höher): der Entwickler muss den `NSHomeKitUsageDescription` Schlüssel der APP- `Info.plist` Datei hinzufügen und eine Zeichenfolge angeben, die erklärt, warum die APP auf die homekit-Datenbank des Benutzers zugreifen möchte. Diese Zeichenfolge wird dem Benutzer beim ersten Ausführen der App angezeigt:

[![](homekit-images/info01.png "Das homekit-Berechtigung-Dialogfeld")](homekit-images/info01.png#lightbox)

Gehen Sie folgendermaßen vor, um diesen Schlüssel festzulegen:

1. Doppelklicken Sie auf `Info.plist` die Datei im **Projektmappen-Explorer** , um Sie für die Bearbeitung zu öffnen.
2. Wechseln Sie am unteren Bildschirmrand zur **Quell** Ansicht.
3. Fügen Sie der Liste einen neuen **Eintrag** hinzu.
4. Wählen Sie in der Dropdown Liste die Option **Datenschutz-homekit-Nutzungs Beschreibung**: 

    [![](homekit-images/info02.png "Auswählen der Datenschutz-homekit-Nutzungs Beschreibung")](homekit-images/info02.png#lightbox)
5. Geben Sie eine Beschreibung für den Grund für den Zugriff der APP auf die homekit-Datenbank des Benutzers ein: 

    [![](homekit-images/info03.png "Beschreibung eingeben")](homekit-images/info03.png#lightbox)
6. Speichern Sie die Änderungen in der Datei.

> [!IMPORTANT]
> Wenn der `NSHomeKitUsageDescription` Schlüssel in der `Info.plist` Datei nicht festgelegt wird, führt dies dazu, dass die APP beim Ausführen in ios 10 (oder höher) ohne Fehler in dem Hintergrund _ausfällt_ (wird vom System zur Laufzeit geschlossen).

## <a name="connecting-to-homekit"></a>Herstellen einer Verbindung mit homekit

Um mit homekit zu kommunizieren, muss Ihre xamarin. IOS-App zunächst eine Instanz der `HMHomeManager` -Klasse instanziieren. Der Home Manager ist der zentrale Einstiegspunkt in homekit und ist dafür verantwortlich, eine Liste der verfügbaren Wohnungen bereitzustellen, diese Liste zu aktualisieren und zu verwalten und die _primäre Homepage_des Benutzers zurückzugeben.

Das `HMHome` -Objekt enthält alle Informationen zu einem Zuhause, einschließlich aller Räume, Gruppen oder Zonen, die es enthalten kann, sowie aller installierten Home Automation-Zubehör. Bevor Vorgänge im homekit ausgeführt werden können, muss mindestens eine `HMHome` erstellt und als das primäre zuhause zugewiesen werden.

Ihre APP ist dafür verantwortlich, zu überprüfen, ob ein primäres zuhause vorhanden ist, und erstellt und weist Sie zu.

### <a name="adding-a-home-manager"></a>Hinzufügen eines Home-Managers

Wenn Sie homekit-Informationen zu einer xamarin. IOS-app hinzufügen möchten, bearbeiten Sie die Datei **AppDelegate.cs** , und führen Sie Sie wie folgt aus:

```csharp
using HomeKit;
...

public HMHomeManager HomeManager { get; set; }
...

public override void FinishedLaunching (UIApplication application)
{
    // Attach to the Home Manager
    HomeManager = new HMHomeManager ();
    Console.WriteLine ("{0} Home(s) defined in the Home Manager", HomeManager.Homes.Count());

    // Wire-up Home Manager Events
    HomeManager.DidAddHome += (sender, e) => {
        Console.WriteLine("Manager Added Home: {0}",e.Home);
    };

    HomeManager.DidRemoveHome += (sender, e) => {
        Console.WriteLine("Manager Removed Home: {0}",e.Home);
    };
    HomeManager.DidUpdateHomes += (sender, e) => {
        Console.WriteLine("Manager Updated Homes");
    };
    HomeManager.DidUpdatePrimaryHome += (sender, e) => {
        Console.WriteLine("Manager Updated Primary Home");
    };
}
```

Wenn die Anwendung zum ersten Mal ausgeführt wird, wird der Benutzer gefragt, ob er dem Zugriff auf die homekit-Informationen gestatten soll:

[![](homekit-images/home01.png "Der Benutzer wird gefragt, ob er den Zugriff auf seine homekit-Informationen erlauben möchte.")](homekit-images/home01.png#lightbox)

Wenn der Benutzer auf " **OK**" antwortet, kann die Anwendung mit dem homekit-Zubehör arbeiten, andernfalls ist dies nicht der Fall, und bei Aufrufen von homekit tritt ein Fehler auf.

Wenn der Home Manager eingerichtet ist, muss die Anwendung als nächstes feststellen, ob ein primäres zuhause konfiguriert wurde, und wenn nicht, kann der Benutzer eine Methode erstellen und zuweisen.

### <a name="accessing-the-primary-home"></a>Zugreifen auf das primäre zuhause

Wie bereits erwähnt, muss vor der Verfügbarkeit des homekits ein primäres zuhause erstellt und konfiguriert werden, und die APP ist dafür zuständig, dem Benutzer die Möglichkeit zu geben, ein primäres Zuhause zu erstellen und zuzuweisen, wenn noch nicht vorhanden.

Wenn Ihre APP erstmalig startet oder aus dem Hintergrund zurückkehrt, muss Sie `DidUpdateHomes` das-Ereignis `HMHomeManager` der-Klasse überwachen, um zu überprüfen, ob ein primäres zuhause vorhanden ist. Wenn kein solcher vorhanden ist, sollte er eine Schnittstelle für den Benutzer bereitstellen, um einen zu erstellen.

Der folgende Code kann einem Ansichts Controller hinzugefügt werden, um die primäre Startseite zu überprüfen:

```csharp
using HomeKit;
...

public AppDelegate ThisApp {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
...

// Wireup events
ThisApp.HomeManager.DidUpdateHomes += (sender, e) => {

    // Was a primary home found?
    if (ThisApp.HomeManager.PrimaryHome == null) {
        // Ask user to add a home
        PerformSegue("AddHomeSegue",this);
    }
};
```

Wenn der Home Manager eine Verbindung mit homekit herstellt, wird `DidUpdateHomes` das Ereignis ausgelöst, alle vorhandenen Häuser werden in die Sammlung von Haus Häusern des Managers geladen, und das primäre Zuhause wird geladen, falls verfügbar.

### <a name="adding-a-primary-home"></a>Hinzufügen einer primären Startseite

Wenn die `PrimaryHome` -Eigenschaft `HMHomeManager` des nach `null` einem `DidUpdateHomes` -Ereignis ist, müssen Sie dem Benutzer die Möglichkeit geben, ein primäres Zuhause zu erstellen und zuzuweisen, bevor Sie fortfahren.

In der Regel stellt die APP ein Formular für den Benutzer zur Verfügung, um eine neue Startseite zu benennen, die dann an den Home Manager als primäres zuhause weitergeleitet wird. Für die **homekitintro** -Beispiel-App wurde im IOS-Designer eine modale Ansicht erstellt und von `AddHomeSegue` der Hauptschnittstelle der app aufgerufen.

Sie stellt ein Textfeld bereit, in dem der Benutzer einen Namen für die neue Startseite und eine Schaltfläche zum Hinzufügen der Startseite eingeben kann. Wenn der Benutzer auf die Schaltfläche **Home hinzufügen** tippt, ruft der folgende Code den Home Manager auf, um die Startseite hinzuzufügen:

```csharp
// Add new home to HomeKit
ThisApp.HomeManager.AddHome(HomeName.Text,(home,error) =>{
    // Did an error occur
    if (error!=null) {
        // Yes, inform user
        AlertView.PresentOKAlert("Add Home Error",string.Format("Error adding {0}: {1}",HomeName.Text,error.LocalizedDescription),this);
        return;
    }

    // Make the primary house
    ThisApp.HomeManager.UpdatePrimaryHome(home,(err) => {
        // Error?
        if (err!=null) {
            // Inform user of error
            AlertView.PresentOKAlert("Add Home Error",string.Format("Unable to make this the primary home: {0}",err.LocalizedDescription),this);
            return ;
        }
    });

    // Close the window when the home is created
    DismissViewController(true,null);
});
```

Die `AddHome` Methode versucht, eine neue Startseite zu erstellen und Sie an die angegebene Rückruf Routine zurückzugeben. Wenn die `error` -Eigenschaft nicht `null`ist, ist ein Fehler aufgetreten, der dem Benutzer angezeigt werden soll. Die häufigsten Fehler werden durch einen nicht eindeutigen Home-Namen verursacht, oder der Home Manager ist nicht in der Lage, mit dem homekit zu kommunizieren.

Wenn die Startseite erfolgreich erstellt wurde, muss die `UpdatePrimaryHome` -Methode aufgerufen werden, um die neue Startseite als primäres zuhause festzulegen. Auch wenn die `error` -Eigenschaft nicht `null`ist, ist ein Fehler aufgetreten, der dem Benutzer angezeigt werden sollte.

Außerdem sollten Sie die-und `DidAddHome` `DidRemoveHome` -Ereignisse von Home Manager überwachen und die Benutzeroberfläche der APP nach Bedarf aktualisieren.

> [!IMPORTANT]
> Die `AlertView.PresentOKAlert` im obigen Beispielcode verwendete Methode ist eine Hilfsklasse in der homekitintro-Anwendung, die das Arbeiten mit den IOS-Warnungen vereinfacht.


## <a name="finding-new-accessories"></a>Auffinden von neuem Zubehör

Nachdem ein primäres Zuhause vom Home Manager definiert oder geladen wurde, kann die xamarin. IOS-App den `HMAccessoryBrowser` zum Auffinden neuer Home Automation-Zubehör und zum Hinzufügen zu einem Zuhause anrufen.

Wenden Sie `StartSearchingForNewAccessories` die-Methode an, um zu beginnen, `StopSearchingForNewAccessories` nach dem neuen Zubehör zu suchen, und die-Methode

> [!IMPORTANT]
> `StartSearchingForNewAccessories`sollte nicht länger ausgeführt werden, da es sich negativ auf die Akku Lebensdauer und die Leistung des IOS-Geräts auswirkt. Apple schlägt `StopSearchingForNewAccessories` vor eine Minute oder nur die Suche vor, wenn dem Benutzer die Benutzeroberfläche zum Suchen von Zubehör angezeigt wird.

Das `DidFindNewAccessory` Ereignis wird aufgerufen, wenn neue Zubehör gefunden werden, und Sie werden der `DiscoveredAccessories` Liste im Zubehör Browser hinzugefügt.

Die `DiscoveredAccessories` Liste enthält eine Auflistung von `HMAccessory` -Objekten, die ein für homekit aktiviertes Home Automation-Gerät und seine verfügbaren Dienste definieren, wie z. b. Lights oder Garage Door Control.

Nachdem das neue Zubehör gefunden wurde, sollte es dem Benutzer angezeigt werden, damit es es auswählen und zu einem Zuhause hinzufügen kann. Beispiel:

[![](homekit-images/accessory01.png "Suchen nach einem neuen Zubehör")](homekit-images/accessory01.png#lightbox)

Ruft die `AddAccessory` -Methode auf, um das ausgewählte Zubehör der Home-Auflistung hinzuzufügen. Beispiel:

```csharp
// Add the requested accessory to the home
ThisApp.HomeManager.PrimaryHome.AddAccessory (_controller.AccessoryBrowser.DiscoveredAccessories [indexPath.Row], (err) => {
    // Did an error occur
    if (err !=null) {
        // Inform user of error
        AlertView.PresentOKAlert("Add Accessory Error",err.LocalizedDescription,_controller);
    }
});
```

Wenn die `err` -Eigenschaft nicht `null`ist, ist ein Fehler aufgetreten, der dem Benutzer angezeigt werden soll. Andernfalls wird der Benutzer aufgefordert, den Setup Code für das hinzu zufügende Gerät einzugeben:

[![](homekit-images/accessory02.png "Geben Sie den Setup Code für das hinzu zufügende Gerät ein.")](homekit-images/accessory02.png#lightbox)

Im homekit-Zubehör Simulator befindet sich diese Zahl im Feld " **Setup Code** ":

[![](homekit-images/accessory03.png "Das Feld \"Setup Code\" im homekit-Zubehör Simulator")](homekit-images/accessory03.png#lightbox)

Bei echten homekit-Zubehör wird der Setup Code entweder auf einer Bezeichnung auf dem Gerät selbst, in der Produkt Box oder im Benutzerhandbuch des Handlers gedruckt.

Sie sollten das `DidRemoveNewAccessory` Ereignis des Zubehör Browsers überwachen und die Benutzeroberfläche aktualisieren, um ein Zubehör aus der Liste der verfügbaren Benutzer zu entfernen, sobald der Benutzer es der eigenen Sammlung hinzugefügt hat.

## <a name="working-with-accessories"></a>Arbeiten mit Zubehör

Nachdem ein primäres zuhause eingerichtet und das Zubehör hinzugefügt wurde, können Sie eine Liste der Zubehör (und optional Räume) präsentieren, mit denen der Benutzer arbeiten kann.

Das `HMRoom` -Objekt enthält alle Informationen zu einem bestimmten Raum und Zubehör, die zu diesem gehören. Räume können optional in einer oder mehreren Zonen organisiert werden. Ein `HMZone` enthält alle Informationen über eine bestimmte Zone und alle zu diesem gehörenden Räume.

Im Rahmen dieses Beispiels werden wir die Dinge einfach aufbewahren und direkt mit dem Zubehör eines Hauses arbeiten, anstatt Sie in Räume oder Zonen zu organisieren.

Das `HMHome` -Objekt enthält eine Liste Zugewiesener Zubehör, die dem Benutzer in seiner `Accessories` -Eigenschaft angezeigt werden können. Beispiel:

[![](homekit-images/accessory04.png "Ein Beispiel für ein Zubehör")](homekit-images/accessory04.png#lightbox)

Formular hier können Benutzer ein bestimmtes Zubehör auswählen und mit den Diensten arbeiten, die es bereitstellt.

## <a name="working-with-services"></a>Arbeiten mit Diensten

Wenn der Benutzer mit einem bestimmten homekit-aktivierten Home Automation-Gerät interagiert, liegt es in der Regel über die Dienste, die es bereitstellt. Die `Services` -Eigenschaft `HMAccessory` der-Klasse enthält eine Auflistung `HMService` von-Objekten, die die Dienste definieren, die von einem Gerät angeboten werden.

Dienste sind z. b. Beleuchtung, Thermostats, öffnungstüren, Schalter oder Sperren der Garage. Einige Geräte (z. b. ein Garagen Türöffner) bieten mehr als einen Dienst, z. b. ein Licht und die Möglichkeit, eine Tür zu öffnen oder zu schließen.

Zusätzlich zu den spezifischen Diensten, die ein bestimmtes Zubehör bereitstellt, enthält jedes Zubehör `Information Service` ein, das Eigenschaften wie Name, Hersteller, Modell und Seriennummer definiert.

### <a name="accessory-service-types"></a>Zubehör Dienst Typen

Die folgenden Dienst Typen sind über die `HMServiceType` Enumeration verfügbar:

- **Accessoryinformation** : enthält Informationen über das angegebene Home Automation-Gerät (Zubehör).
- **Airqualitysensor** : definiert einen Klima Qualitäts Sensor.
- **Akku** : definiert den Zustand des Akkus eines Zubehör.
- **Carbondioxidesensor** : definiert einen Kohlendioxid Sensor.
- **Carbonmonoxidesensor** : definiert einen Kohlenmonoxid-Sensor.
- **Contactsensor** : definiert einen Kontakt Sensor (z. b. ein Fenster, das geöffnet oder geschlossen wird).
- **Door** : definiert einen Türzustands Sensor (z. b. geöffnet oder geschlossen).
- **Lüfter** : definiert einen Remote gesteuerten Lüfter.
- **Garagedooropener** : definiert einen Garage Door Opener.
- **Humiditysensor** : definiert einen Feuchtigkeitssensor.
- **Leaksensor** : definiert einen undichten Sensor (z. b. für einen Heißwasser-oder Waschmaschine).
- **Lightbulb** : definiert eine eigenständige Beleuchtung oder ein Licht, das Teil eines anderen Zubehör ist (z. b. eine Garage Türöffner).
- **Lightsensor** : definiert einen Lichtsensor.
- **Lockmanagement** : definiert einen Dienst, der eine automatisierte Türsperre verwaltet.
- **Lockmechanism** : definiert eine Remote gesteuerte Sperre (z. b. eine Türsperre).
- " **Mutionsensor** ": definiert einen Bewegungssensor.
- **Occupancysensor** : definiert einen Auslastungs Sensor.
- **Outlet** : definiert eine Remote gesteuerte Wall-Outlets.
- **Securitysystem** : definiert ein Heim Sicherheitssystem.
- **Statefulprogrammableswitch** : definiert einen programmierbaren Switch, der nach dem auslösen (z. b. einem Flip-Switch) in einem zustandsart verbleibt.
- **Statelessprogrammableswitch** : definiert einen programmierbaren Switch, der nach dem Auslösen in seinen ursprünglichen Zustand zurückkehrt (z. b. eine Schaltfläche "Push").
- **Smokesensor** : definiert einen Rauch Sensor.
- **Switch** : definiert einen ein-/ausschalten-Schalter wie einen Standard-Wall Switch.
- Temperatur **Sensor** : definiert einen Temperatursensor.
- **Thermostat** : definiert einen intelligenten Thermostat, der zum Steuern eines HVAC-Systems verwendet wird.
- **Fenster** : definiert ein automatisiertes Fenster, in dem ein Rohr Remote geöffnet oder geschlossen wird.
- **Windowcover** : definiert eine Remote gesteuerte Fenster Abdeckung, wie z. b. Jalousien, die geöffnet oder geschlossen werden können.

### <a name="displaying-service-information"></a>Anzeigen von Dienst Informationen

Nachdem Sie einen `HMAccessory` geladen haben, können Sie die `HNService` einzelnen bereitgestellten Objekte Abfragen und diese Informationen dem Benutzer anzeigen:

[![](homekit-images/accessory05.png "Anzeigen von Dienst Informationen")](homekit-images/accessory05.png#lightbox)

Sie sollten die `Reachable` -Eigenschaft `HMAccessory` von immer überprüfen, bevor Sie versuchen, damit zu arbeiten. Ein Zubehör kann nicht erreichbar sein, weil sich der Benutzer nicht innerhalb des Bereichs des Geräts befindet oder wenn er getrennt wurde.

Nachdem ein Dienst ausgewählt wurde, kann der Benutzer eine oder mehrere Eigenschaften dieses Dienstanbieter anzeigen oder ändern, um ein bestimmtes Home Automation-Gerät zu überwachen oder zu steuern.

<a name="Working-with-Characteristics" />

## <a name="working-with-characteristics"></a>Arbeiten mit Merkmalen

Jedes `HMService` -Objekt kann eine Auflistung von `HMCharacteristic` -Objekten enthalten, die entweder Informationen über den Zustand des Dienstanbieter bereitstellen (z. b. eine Tür, die geöffnet oder geschlossen wird) oder dem Benutzer das Anpassen eines Zustands (z. b. die Farbe eines Lichts) erlauben.

`HMCharacteristic`bietet nicht nur Informationen zu einem Merkmal und seinem Zustand, sondern stellt auch Methoden zum Arbeiten mit dem Zustand über _Merkmals Metadaten_ (`HMCharacteristisMetadata`) bereit. Diese Metadaten können Eigenschaften (z. b. Minimal-und Maximalwerte Bereiche) bereitstellen, die hilfreich sind, wenn dem Benutzerinformationen angezeigt werden, oder Sie können die Zustände ändern.

Die `HMCharacteristicType` Enumeration stellt einen Satz von Merkmals Metadatenwerten bereit, die wie folgt definiert oder geändert werden können:

- AdminOnlyAccess
- Airpartiatedensity
- Airparticulatesize
- AirQuality
- AudioFeedback
- Akku Stufe
- Helligkeit
- Carbondioxideerkannte
- CarbonDioxideLevel
- CarbonDioxidePeakLevel
- CarbonMonoxideDetected
- CarbonMonoxideLevel
- CarbonMonoxidePeakLevel
- ChargingState
- ContactState
- Coolingthreshold
- CurrentDoorState
- CurrentHeatingCooling
- CurrentHorizontalTilt
- Currentlightlevel
- CurrentLockMechanismState
- CurrentPosition
- CurrentRelativeHumidity
- CurrentSecuritySystemState
- Currenttemperatur
- CurrentVerticalTilt
- FirmwareVersion
- HardwareVersion
- HeatingCoolingStatus
- Heatingthreshold
- HoldPosition
- Farbton
- Identifizieren (Identify)
- InputEvent
- Lecks erkannt
- LockManagementAutoSecureTimeout
- LockManagementControlPoint
- LockMechanismLastKnownAction
- Protokolle
- Bauers
- Modell
- "Mutionerkannt"
- Name
- Blockierungen erkannt
- Aufgespürt
- OutletInUse
- Outputstate
- PositionState
- PowerState
- RotationDirection
- Rotationspeed
- Sättigung
- SerialNumber
- SmokeDetected
- SoftwareVersion
- StatusActive
- StatusFault
- StatusJammed
- StatusLowBattery
- StatusTampered
- TargetDoorState
- TargetHeatingCooling
- TargetHorizontalTilt
- TargetLockMechanismState
- TargetPosition
- TargetRelativeHumidity
- TargetSecuritySystemState
- Targettemperatur
- TargetVerticalTilt
- Temperatur Einheiten
- Version

### <a name="working-with-a-characteristics-value"></a>Arbeiten mit dem Wert eines Merkmals

Um sicherzustellen, dass Ihre APP über den aktuellen Zustand eines bestimmten Merkmals verfügt `ReadValue` , müssen Sie `HMCharacteristic` die-Methode der-Klasse aufzurufen. Wenn die `err` -Eigenschaft nicht `null`ist, ist ein Fehler aufgetreten, der dem Benutzer angezeigt oder nicht angezeigt werden kann.

Die-Eigenschaft `Value` des-Objekts enthält den aktuellen Zustand des angegebenen Merkmals `NSObject`als und kann daher nicht direkt in C#verarbeitet werden.

Um den Wert zu lesen, wurde die folgende Hilfsklasse zur **homekitintro** -Beispielanwendung hinzugefügt:

```csharp
using System;
using Foundation;
using System.Globalization;
using CoreGraphics;

namespace HomeKitIntro
{
    /// <summary>
    /// NS object converter is a helper class that helps to convert NSObjects into
    /// C# objects
    /// </summary>
    public static class NSObjectConverter
    {
        #region Static Methods
        /// <summary>
        /// Converts to an object.
        /// </summary>
        /// <returns>The object.</returns>
        /// <param name="nsO">Ns o.</param>
        /// <param name="targetType">Target type.</param>
        public static Object ToObject (NSObject nsO, Type targetType)
        {
            if (nsO is NSString) {
                return nsO.ToString ();
            }

            if (nsO is NSDate) {
                var nsDate = (NSDate)nsO;
                return DateTime.SpecifyKind ((DateTime)nsDate, DateTimeKind.Unspecified);
            }

            if (nsO is NSDecimalNumber) {
                return decimal.Parse (nsO.ToString (), CultureInfo.InvariantCulture);
            }

            if (nsO is NSNumber) {
                var x = (NSNumber)nsO;

                switch (Type.GetTypeCode (targetType)) {
                case TypeCode.Boolean:
                    return x.BoolValue;
                case TypeCode.Char:
                    return Convert.ToChar (x.ByteValue);
                case TypeCode.SByte:
                    return x.SByteValue;
                case TypeCode.Byte:
                    return x.ByteValue;
                case TypeCode.Int16:
                    return x.Int16Value;
                case TypeCode.UInt16:
                    return x.UInt16Value;
                case TypeCode.Int32:
                    return x.Int32Value;
                case TypeCode.UInt32:
                    return x.UInt32Value;
                case TypeCode.Int64:
                    return x.Int64Value;
                case TypeCode.UInt64:
                    return x.UInt64Value;
                case TypeCode.Single:
                    return x.FloatValue;
                case TypeCode.Double:
                    return x.DoubleValue;
                }
            }

            if (nsO is NSValue) {
                var v = (NSValue)nsO;

                if (targetType == typeof(IntPtr)) {
                    return v.PointerValue;
                }

                if (targetType == typeof(CGSize)) {
                    return v.SizeFValue;
                }

                if (targetType == typeof(CGRect)) {
                    return v.RectangleFValue;
                }

                if (targetType == typeof(CGPoint)) {
                    return v.PointFValue;
                }
            }

            return nsO;
        }

        /// <summary>
        /// Convert to string
        /// </summary>
        /// <returns>The string.</returns>
        /// <param name="nsO">Ns o.</param>
        public static string ToString(NSObject nsO) {
            return (string)ToObject (nsO, typeof(string));
        }

        /// <summary>
        /// Convert to date time
        /// </summary>
        /// <returns>The date time.</returns>
        /// <param name="nsO">Ns o.</param>
        public static DateTime ToDateTime(NSObject nsO){
            return (DateTime)ToObject (nsO, typeof(DateTime));
        }

        /// <summary>
        /// Convert to decimal number
        /// </summary>
        /// <returns>The decimal.</returns>
        /// <param name="nsO">Ns o.</param>
        public static decimal ToDecimal(NSObject nsO){
            return (decimal)ToObject (nsO, typeof(decimal));
        }

        /// <summary>
        /// Convert to boolean
        /// </summary>
        /// <returns><c>true</c>, if bool was toed, <c>false</c> otherwise.</returns>
        /// <param name="nsO">Ns o.</param>
        public static bool ToBool(NSObject nsO){
            return (bool)ToObject (nsO, typeof(bool));
        }

        /// <summary>
        /// Convert to character
        /// </summary>
        /// <returns>The char.</returns>
        /// <param name="nsO">Ns o.</param>
        public static char ToChar(NSObject nsO){
            return (char)ToObject (nsO, typeof(char));
        }

        /// <summary>
        /// Convert to integer
        /// </summary>
        /// <returns>The int.</returns>
        /// <param name="nsO">Ns o.</param>
        public static int ToInt(NSObject nsO){
            return (int)ToObject (nsO, typeof(int));
        }

        /// <summary>
        /// Convert to float
        /// </summary>
        /// <returns>The float.</returns>
        /// <param name="nsO">Ns o.</param>
        public static float ToFloat(NSObject nsO){
            return (float)ToObject (nsO, typeof(float));
        }

        /// <summary>
        /// Converts to double
        /// </summary>
        /// <returns>The double.</returns>
        /// <param name="nsO">Ns o.</param>
        public static double ToDouble(NSObject nsO){
            return (double)ToObject (nsO, typeof(double));
        }
        #endregion
    }
}
```

`NSObjectConverter` Wird immer dann verwendet, wenn die Anwendung den aktuellen Zustand eines Merkmals lesen muss. Beispiel:

```csharp
var value = NSObjectConverter.ToFloat (characteristic.Value);
```

Die obige Zeile konvertiert den Wert in einen `float` , der dann im xamarin C# -Code verwendet werden kann.

Um einen `HMCharacteristic`zu ändern, müssen `WriteValue` Sie seine-Methode aufzurufen und den `NSObject.FromObject` neuen Wert in einen-Befehl einschließen. Beispiel:

```csharp
Characteristic.WriteValue(NSObject.FromObject(value),(err) =>{
    // Was there an error?
    if (err!=null) {
        // Yes, inform user
        AlertView.PresentOKAlert("Update Error",err.LocalizedDescription,Controller);
    }
});
```

Wenn die `err` -Eigenschaft nicht `null`ist, ist ein Fehler aufgetreten, der dem Benutzer angezeigt werden soll.

### <a name="testing-characteristic-value-changes"></a>Testen von Merkmalen Wertänderungen

Beim Arbeiten mit `HMCharacteristics` und simulierten Zubehör können Änderungen an `Value` der Eigenschaft innerhalb des homekit-Zubehör Simulators überwacht werden.

Wenn die **homekitintro** -App auf echter IOS-Geräte Hardware ausgeführt wird, sollten Änderungen am Wert eines Merkmals im homekit-Zubehör Simulator fast sofort angezeigt werden. Beispiel: Ändern des Zustands eines Lichts in der IOS-App:

[![](homekit-images/test01.png "Ändern des Zustands eines Lichts in einer IOS-App")](homekit-images/test01.png#lightbox)

Sollte den Zustand des Lichts im homekit-Zubehör Simulator ändern. Wenn der Wert nicht geändert wird, überprüfen Sie den Status der Fehlermeldung, wenn Sie neue Merkmals Werte schreiben, und stellen Sie sicher, dass das Zubehör noch erreichbar ist.

## <a name="advanced-homekit-features"></a>Erweiterte homekit-Features

In diesem Artikel wurden die grundlegenden Features behandelt, die für die Arbeit mit homekit-Zubehör in einer xamarin. IOS-App erforderlich sind. Es gibt jedoch mehrere erweiterte Features von homekit, die in dieser Einführung nicht behandelt werden:

- **Räume** -das homekit-fähige Zubehör kann optional vom Endbenutzer in Räume angeordnet werden. Auf diese Weise kann das homekit Zubehör auf eine Weise präsentieren, die dem Benutzer leicht verständlich ist und verwendet werden kann. Weitere Informationen zum Erstellen und warten von Räumen finden Sie in der [hmroom](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMRoom_Class/index.html#//apple_ref/occ/cl/HMRoom) -Dokumentation von Apple.
- **Zones** : Räume können optional vom Endbenutzer in Zonen organisiert werden. Eine Zone bezieht sich auf eine Sammlung von Räumen, die der Benutzer als einzelne Einheit behandeln kann. Beispiel: Nach oben, nach unten oder im Untergeschoss. Dies ermöglicht es homekit, Zubehör auf eine Weise darzustellen und zu bearbeiten, die für den Endbenutzer sinnvoll ist. Weitere Informationen zum Erstellen und Verwalten von Zonen finden Sie in der Dokumentation zu [hmzone](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMZone_Class/index.html#//apple_ref/occ/cl/HMZone) von Apple.
- **Aktionen und Aktions Sätze** : Aktionen ändern die Eigenschaften des Zubehör Dienstanbieter und können in Mengen gruppiert werden. Aktions Sätze fungieren als Skripts, um eine Gruppe von Zubehör zu steuern und ihre Aktionen zu koordinieren. Beispielsweise kann ein "Watch TV"-Skript die Jalousien schließen, die Lichter ausblenden und das Fernsehen und sein Soundsystem einschalten. Weitere Informationen zum Erstellen und Verwalten von Aktionen und Aktions Sätzen finden Sie in der Dokumentation zu [hmaction](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMAction_Class/index.html#//apple_ref/occ/cl/HMAction) und [hmactionset](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMActionSet_Class/index.html#//apple_ref/occ/cl/HMActionSet) von Apple.
- **Trigger** : ein Trigger kann einen oder mehrere Aktions Sätze aktivieren, wenn ein bestimmter Satz von Bedingungen erfüllt ist. Aktivieren Sie z. b. den portch-Licht, und Sperren Sie alle externen Türen, wenn Sie sich außerhalb von befinden. Weitere Informationen zum Erstellen und Verwalten von Triggern finden Sie in der [hmtrigger](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMTrigger_Class/index.html#//apple_ref/occ/cl/HMTrigger) -Dokumentation von Apple.

Da diese Features die oben dargestellten Techniken verwenden, sollten Sie einfach zu implementieren sein, indem Sie das [homekitdeveloper-Handbuch](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html)von Apple, die [homekit-Richtlinien zur Benutzeroberfläche](https://developer.apple.com/homekit/ui-guidelines/) und die [homekit-frameworkreferenz](https://developer.apple.com/library/ios/home_kit_framework_ref)verwenden.

## <a name="homekit-app-review-guidelines"></a>Homekit-App-Überprüfungs Richtlinien

Bevor Sie eine homekit-aktivierte xamarin. IOS-APP an iTunes Connect für die Veröffentlichung im iTunes App Store senden, müssen Sie die Apple-Richtlinien für homekit-aktivierte apps befolgen:

- Der primäre Zweck der APP _muss_ Home Automation sein, wenn Sie das homekit-Framework verwenden.
- Der Marketing Text der APP muss die Benutzer benachrichtigen, dass das homekit verwendet wird und eine Datenschutzrichtlinie bereitstellen muss.
- Das Sammeln von Benutzerinformationen oder die Verwendung von homekit für Werbung ist streng unzulässig.

Die vollständigen Überprüfungs Richtlinien finden Sie in den [Richtlinien für den App Store-Review](https://developer.apple.com/app-store/review/guidelines/)von Apple.

## <a name="whats-new-in-ios-9"></a>Neuerungen in ios 9

Apple hat die folgenden Änderungen und Ergänzungen für homekit für IOS 9 vorgenommen:

- **Beibehalten vorhandener Objekte** : Wenn ein vorhandenes Zubehör geändert wird, informiert der`HMHomeManager`Home Manager () Sie über das geänderte Element, das geändert wurde.
- **Persistente** Bezeichner: alle relevanten homekit-Klassen enthalten `UniqueIdentifier` jetzt eine Eigenschaft, mit der ein bestimmtes Element in homekit-aktivierten Apps (oder Instanzen der gleichen APP) eindeutig identifiziert werden können.
- **Benutzerverwaltung** : Es wurde ein integrierter Ansichts Controller hinzugefügt, um Benutzerverwaltung für Benutzer bereitzustellen, die auf die homekit-Geräte auf der Startseite des primären Benutzers zugreifen können.
- **Benutzer Funktionen** : homekit-Benutzer verfügen jetzt über eine Reihe von Berechtigungen, die Steuern, welche Funktionen Sie in homekit-und homekit-aktiviertem Zubehör verwenden können. Ihre APP sollte nur für den aktuellen Benutzer relevante Funktionen anzeigen. Beispielsweise sollten nur Administratoren in der Lage sein, andere Benutzer zu verwalten.
- **Vordefinierte Szenen** : vordefinierte Szenen wurden für vier häufige Ereignisse erstellt, die für den durchschnittlichen homekit-Benutzer auftreten: Einrichten, verlassen, zurückkehren und zum Bett wechseln. Diese vordefinierten Szenen können nicht aus einer Startseite gelöscht werden.
- **Szenen und Siri** -Siri haben eine tiefere Unterstützung für Szenen in ios 9 und können den Namen einer beliebigen Szene erkennen, die im homekit definiert ist. Ein Benutzer kann eine Szene ausführen, indem er einfach seinen Namen an Siri äußert.
- **Zubehör Kategorien** : Es wurde ein Satz vordefinierter Kategorien zu allen Zubehör hinzugefügt, mit denen der Typ des Zubehör identifiziert wird, das zu einem Zuhause hinzugefügt oder in der APP bearbeitet werden kann. Diese neuen Kategorien sind während der Zubehör Einrichtung verfügbar.
- **Apple Watch Support** -homekit ist jetzt für watchos verfügbar, und die Apple Watch können homekit-aktivierte Geräte steuern, ohne dass ein iPhone in der Nähe der Uhr ist. Homekit für watchos unterstützt die folgenden Funktionen: Anzeigen von Wohnheimen, Steuern von Zubehör und Ausführen von Szenen.
- **Neuer ereignistriggertyp** : Zusätzlich zu den in ios 8 unterstützten Triggertypen unterstützt IOS 9 jetzt Ereignis Trigger basierend auf dem Zubehör Zustand (z. b. Sensordaten) oder Geolokation. Ereignis Trigger verwenden `NSPredicates` , um Bedingungen für ihre Ausführung festzulegen.
- **Remote Zugriff** : mit Remote Zugriff kann der Benutzer nun das homekit-aktivierte Home Automation-Zubehör steuern, wenn Sie sich nicht an einem Remote Standort befinden. In ios 8 wurde dies nur unterstützt, wenn der Benutzer in der Startseite eine Apple-TV-Dritt Generation besaß. In ios 9 wird diese Einschränkung angehoben, und der Remote Zugriff wird über icloud und das homekit-Zubehör Protokoll (HAP) unterstützt.
- **Neue Bluetooth Low Energy (BLE)-Fähigkeiten** : homekit unterstützt jetzt weitere Zubehör Typen, die über das Bluetooth Low Energy (BLE)-Protokoll kommunizieren können. Mit dem sicheren Tunnelingmodus kann ein homekit-Zubehör ein weiteres Bluetooth-Zubehör über Wi-Fi verfügbar machen (wenn es sich nicht im Bluetooth-Bereich befindet). In ios 9 haben ble-Zubehör vollständige Unterstützung für Benachrichtigungen und Metadaten.
- **Neue Zubehör Kategorien** : Apple hat die folgenden neuen Zubehör Kategorien in ios 9 hinzugefügt: Fenster Beläge, motorische Türen und Fenster, Alarm Systeme, Sensoren und programmierbare Switches.

Weitere Informationen zu den neuen Features von homekit in ios 9 finden Sie im homekit- [Index](https://developer.apple.com/homekit/) von Apple und im Video zu den [Neuerungen im](https://developer.apple.com/videos/wwdc/2015/?id=210) homekit-Video.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde das homekit Home Automation-Framework von Apple eingeführt. Es wurde gezeigt, wie Sie Testgeräte mithilfe des homekit-Zubehör Simulators einrichten und konfigurieren, und wie Sie eine einfache xamarin. IOS-app erstellen, mit der Sie Home Automation-Geräte mithilfe von homekit ermitteln, mit Ihnen kommunizieren und steuern können.



## <a name="related-links"></a>Verwandte Links

- [IOS 9-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [IOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [Neuerungen in ios 9,0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Homekitdeveloper-Handbuch](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html)
- [Homekit-Richtlinien zur Benutzeroberfläche](https://developer.apple.com/homekit/ui-guidelines/)
- [Referenz zum homekit-Framework](https://developer.apple.com/library/ios/home_kit_framework_ref)
