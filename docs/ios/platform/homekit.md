---
title: HomeKit
description: "HomeKit ist Apple Framework zum home Automatisierungsgeräten steuern. Dieser Artikel führt HomeKit und Konfigurieren von Test-Zubehör im Simulator HomeKit Zubehör und eine einfache Xamarin.iOS app schreiben, für die Interaktion mit diesen Zubehör behandelt."
ms.topic: article
ms.prod: xamarin
ms.assetid: 90C0C553-916B-46B1-AD52-1E7332792283
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 1a49c3a3181b477b777de74b0eb53f5e0da6f041
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="homekit"></a>HomeKit

_HomeKit ist Apple Framework zum home Automatisierungsgeräten steuern. Dieser Artikel führt HomeKit und Konfigurieren von Test-Zubehör im Simulator HomeKit Zubehör und eine einfache Xamarin.iOS app schreiben, für die Interaktion mit diesen Zubehör behandelt._

[ ![](homekit-images/accessory01.png "Ein Beispiel für HomeKit-fähiger Apps")](homekit-images/accessory01.png)

Apple hypervisorbasierte iOS 8 als eine Möglichkeit, mehrere home Automation-Geräte aus einer Vielzahl Lieferanten in eine einzelne Einheit kohärente nahtlos integrieren HomeKit. Durch das Höherstufen eines gemeinsamen Protokolls für die Ermittlung, ermöglicht konfigurieren und Steuern der home Automation-Geräte HomeKit Geräten nicht zugehörigen Hersteller, ohne Aufwand koordinieren müssen einzelnen Lieferanten zusammenarbeiten.

Mit HomeKit können Sie eine Xamarin.iOS-app erstellen, die einem beliebigen Gerät aktiviert HomeKit steuert, ohne über Anbieter-APIs oder apps bereitgestellt. Verwenden HomeKit, können Sie Folgendes tun:

- Erkennen Sie neue HomeKit aktiviert home Automation Geräte, und fügen Sie sie in einer Datenbank, die auf allen iOS-Geräte des Benutzers beibehält.
- Einrichten, konfigurieren, anzeigen und steuern alle Geräte mit der HomeKit _Home Konfigurationsdatenbank_.
- Mit allen vorkonfiguriert, dass HomeKit Geräten kommunizieren und diese einzelne Aktionen ausführen, oder arbeiten zusammen, wie die Aktivierung von alle Leuchten in der Kitchen Befehl.

Zusätzlich zu Ihrem Zweck Geräte in der Konfigurationsdatenbank Startseite an apps HomeKit aktiviert, bietet HomeKit Zugriff auf Sprachbefehle Siri. Angesichts einer entsprechend konfigurierten HomeKit-Setup, die Benutzer kann ausgeben Sprachbefehle wie z. B. "Siri, Leuchten im Raum lebendes aktivieren."

<a name="Home-Configuration-Database" />

## <a name="the-home-configuration-database"></a>Der Home-Konfigurationsdatenbank

HomeKit werden alle Automation-Geräte in einer angegebenen Position in einer Auflistung Startseite organisiert. Diese Auflistung bietet eine Möglichkeit für den Benutzer zum Gruppieren von ihren Automatisierungsgeräten home in logisch angeordnete Einheiten mit aussagekräftigen, lesbare Bezeichnungen.

Der Home-Auflistung wird in einer Home-Konfigurationsdatenbank gespeichert, die automatisch gesichert und synchronisierten auf allen iOS-Geräte des Benutzers sein. HomeKit bietet die folgenden Klassen für die Arbeit mit der Home-Konfigurationsdatenbank:

- `HMHome` -Dies ist der Container der obersten Ebene, der alle Informationen und Konfigurationen für alle Geräte der home-Automatisierung, in einem einzigen physischen Speicherort (z. b enthält. eine einzelne Familie Wohnorts). Der Benutzer möglicherweise mehrere Wohnort, z. B. ihre wichtigsten Startseite "und" ein Haus Urlaub. Oder haben möglicherweise verschiedene "enthält" auf die gleiche Eigenschaft, z. B. die wichtigsten House und ein Haus Gast über die Garage. In beiden Fällen mindestens `HMHome` Objekt _müssen_ eingerichtet werden und gespeichert, bevor andere HomeKit Informationen eingegeben werden kann.
- `HMRoom` -While optional, eine `HMRoom` kann der Benutzer bestimmte Räume innerhalb eines Hauses definieren (`HMHome`) wie z. B.: Kitchen Bad, Garage oder dynamisches Platz. Der Benutzer kann alle Geräte home Automatisierung in einer bestimmten Position in dem Haus in die Gruppe eine `HMRoom` und auf sie reagieren, als eine Einheit. Stellen Sie z. B. Siri Garage Leuchten deaktivieren.
- `HMAccessory` -Dies steht für ein einzelnes, physischen HomeKit aktiviert Automation-Gerät, das in der Benutzer Wohnorts (z. B. ein intelligentes Thermostat) installiert wurde. Jede `HMAccessory` zugewiesen ist eine `HMRoom`. Wenn der Benutzer alle Räume noch nicht konfiguriert haben, weist HomeKit Zubehör zu einem speziellen Standard-Raum.
- `HMService` -Stellt einen Dienst bereitgestellt werden, indem Sie einen bestimmten `HMAccessory`, z. B. den Status ein-oder auszuschalten, der eine helle oder dessen Farbe (wenn es sich um eine Farbe ändern unterstützt wird). Jede `HMAccessory` können mehr als ein Dienst, z. B. eine Garage Tür dateiöffnungsfunktion, die auch eine helle haben. Darüber hinaus eine bestimmte `HMAccessory` möglicherweise Dienste, z. B. Firmware-Aktualisierung, die durch Benutzer gesteuert werden.
- `HMZone` – Ermöglicht es den Benutzer, die eine Auflistung von Gruppe `HMRoom` Objekte in logische Zonen, z. B. sollten, unteren Stock oder Untergeschoss. Zwar optional, können diese für Aktivitäten wie Siri Frage aller das Licht aus unteren Stock aktivieren.

<a name="Provisioning-a-HomeKit-App" />

## <a name="provisioning-a-homekit-app"></a>Eine HomeKit-App-Bereitstellung

Aufgrund der sicherheitsanforderungen durch HomeKit auferlegt werden muss eine Xamarin.iOS-app, die das Framework HomeKit verwendet ordnungsgemäß in der Apple Developer Portal und in der Projektdatei Xamarin.iOS konfiguriert werden.

Führen Sie folgende Schritte aus:

1. Melden Sie sich bei der [Apple-Entwicklerportal](http://developer.apple.com).
2. Klicken Sie auf **Zertifikate "," Bezeichner "und" Profile**.
3. Wenn Sie dies noch nicht getan haben, klicken Sie auf **Bezeichner** und eine ID für Ihre app zu erstellen (z. B. `com.company.appname`), andernfalls Bearbeiten Ihrer vorhandenen ID an.
4. Sicherstellen, dass die **HomeKit** Dienst für die angegebene ID Punkte überprüft wurden: 

    [ ![](homekit-images/provision01.png "Aktivieren Sie den Dienst HomeKit für die angegebene ID")](homekit-images/provision01.png)
5. Speichern Sie die Änderungen.
4. Klicken Sie auf **Provisioning Profile** > **Entwicklung** und eine neue Entwicklungen Bereitstellungsprofil für Ihre app zu erstellen: 

    [ ![](homekit-images/provision02.png "Erstellen Sie eine neue Entwicklungen Bereitstellungsprofil für die app")](homekit-images/provision02.png)
5. Herunterladen Sie und installieren Sie das neue Bereitstellungsprofil oder verwenden Sie Xcode herunterladen und installieren das Profil.
6. Bearbeiten Sie Ihre Xamarin.iOS Projektoptionen, und stellen Sie sicher, dass Sie das Bereitstellungsprofil verwenden, das Sie soeben erstellt haben: 

    [ ![](homekit-images/provision03.png "Wählen Sie die erstellte provisioning-Profil")](homekit-images/provision03.png)
7. Als Nächstes bearbeiten Ihrer **"Info.plist"** Datei, und stellen Sie sicher, dass Sie die App-ID verwenden, mit denen die provisioning-Profil erstellt wurde: 

    [ ![](homekit-images/provision04.png "Legen Sie die App-ID ")](homekit-images/provision04.png)
8. Schließlich Bearbeiten Ihrer **Entitlements.plist** Datei und sicherstellen, dass die **HomeKit** Anspruch ausgewählt wurde: 

    [ ![](homekit-images/provision05.png "Aktivieren der HomeKit-Berechtigung")](homekit-images/provision05.png)
9. Speichern Sie die Änderungen auf alle Dateien.

Mit diesen Einstellungen vorhanden kann die Anwendung jetzt HomeKit-Framework-APIs zuzugreifen. Ausführliche Informationen zu Bereitstellung, finden Sie unter unsere [Gerätebereitstellung](~/ios/get-started/installation/device-provisioning/index.md) und [Bereitstellung der App](~/ios/get-started/installation/device-provisioning/index.md) Handbüchern.

> [!IMPORTANT]
> **Hinweis:** Testen einer app HomeKit aktiviert eine echte iOS-Gerät, das ordnungsgemäß bereitgestellt wurde für die Entwicklung erfordert. HomeKit kann nicht von iOS-Simulators getestet werden.

## <a name="the-homekit-accessory-simulator"></a>Der Simulator HomeKit Zubehör

Erstellt Apple bieten eine Möglichkeit, alle möglichen home Automation Geräte und Dienste, testen, ohne ein physisches Gerät, damit die _HomeKit Zubehör Simulator_. Mit diesem Simulator, können Sie einrichten und Konfigurieren von virtuellen HomeKit-Geräte.

### <a name="installing-the-simulator"></a>Installieren den Simulator

Apple bietet HomeKit Zubehör Simulator als separater Download von Xcode, daher Sie es installieren, bevor Sie fortfahren müssen.

Führen Sie folgende Schritte aus:

1. Finden Sie in einem Webbrowser auf [Downloads für Apple-Entwickler](https://developer.apple.com/download/more/?name=for%20Xcode)
2. Herunterladen der **zusätzliche Tools für Xcode Xxx** (wobei Xxx ist eine der Version von Xcode, die Sie installiert haben): 

    [ ![](homekit-images/simulator01.png "Die zusätzlichen Tools für Xcode herunterladen")](homekit-images/simulator01.png)
3. Öffnen Sie die Datenträger-Image, und installieren Sie die Tools in Ihrem **Anwendungen** Verzeichnis.

Die HomeKit Zubehör Emulator installiert können virtuelle Zubehör zu Testzwecken erstellt werden.

### <a name="creating-virtual-accessories"></a>Erstellen von virtuellen Zubehör

Starten Simulator HomeKit Zubehör, und einige virtuelle Zubehör erstellen, führen Sie folgende Schritte aus:

1. Starten Sie aus dem Ordner Anwendungen HomeKit Zubehör Simulator aus: 

    [ ![](homekit-images/simulator02.png "Der Simulator HomeKit Zubehör")](homekit-images/simulator02.png)
2. Klicken Sie auf die  **+**  Schaltfläche und wählen Sie **neue Zubehör...** : 

    [ ![](homekit-images/simulator03.png "Hinzufügen einer neuen Zubehör")](homekit-images/simulator03.png)
3. Füllen Sie die Informationen zu den neuen Zubehör, und klicken Sie auf die **Fertig stellen** Schaltfläche: 

    [ ![](homekit-images/simulator04.png "Füllen Sie die Informationen zu den neuen Accessor")](homekit-images/simulator04.png)
4. Klicken Sie auf die **Dienst hinzufügen...** Schaltfläche, und wählen Sie aus der Dropdownliste einen Diensttyp: 

    [ ![](homekit-images/simulator05.png "Wählen Sie aus der Dropdownliste einen Diensttyp")](homekit-images/simulator05.png)
5. Geben Sie einen **Namen** für den Dienst und klicken Sie auf die **Fertig stellen** Schaltfläche: 

    [ ![](homekit-images/simulator06.png "Geben Sie einen Namen für den Dienst")](homekit-images/simulator06.png)
6. Sie können optionale Eigenschaften für einen Dienst bereitstellen, indem Sie auf die **hinzufügen Merkmal** Schaltfläche und die erforderlichen Einstellungen konfigurieren: 

    [ ![](homekit-images/simulator07.png "Konfigurieren die erforderlichen Einstellungen")](homekit-images/simulator07.png)
7. Wiederholen Sie die obengenannten Schritte, um eine der virtuellen home Automation Gerätetyp erstellen, die HomeKit unterstützt.

Mit einigen Beispiel virtuelle HomeKit Zubehör erstellt und konfiguriert können Sie jetzt nutzen und diese Geräte von Ihrer app Xamarin.iOS steuern.

## <a name="configuring-the-infoplist-file"></a>Konfigurieren die Datei "Info.plist"

Für iOS 10 neue (und höher), muss der Entwickler zum Hinzufügen der `NSHomeKitUsageDescription` Schlüssel für der app `Info.plist` Datei, und geben Sie eine Zeichenfolge deklarieren warum die app des Benutzers HomeKit Datenbank zugreifen möchte. Diese Zeichenfolge auf die Benutzer bei der ersten Zeit erhält Ausführen der app zu erstellen:

[ ![](homekit-images/info01.png "Das Dialogfeld "Berechtigung HomeKit"")](homekit-images/info01.png)

Um diesen Schlüssel festzulegen, führen Sie folgende Schritte aus:

1. Doppelklicken Sie auf die `Info.plist` in der Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen.
2. Am unteren Rand des Bildschirms, wechseln Sie zu der **Quelle** anzeigen.
3. Fügen Sie einen neuen **Eintrag** der Liste.
4. Wählen Sie aus der Dropdown-Liste **Datenschutz - Beschreibung der Verwendung HomeKit**: 

    [ ![](homekit-images/info02.png "Wählen Sie die Datenschutz - Beschreibung HomeKit Verwendung")](homekit-images/info02.png)
5. Geben Sie eine Beschreibung für die Gründe die app des Benutzers HomeKit Datenbank zugreifen möchte ein: 

    [ ![](homekit-images/info03.png "Geben Sie eine Beschreibung")](homekit-images/info03.png)
6. Speichern Sie die Änderungen in der Datei.

> [!IMPORTANT]
> **Hinweis:** Fehler beim Festlegen der `NSHomeKitUsageDescription` -Schlüssel in der `Info.plist` Datei führt in der app _im Hintergrund fehlerhaften_ (wird vom System zur Laufzeit geschlossen) ohne Fehler bei der Ausführung im iOS 10 (oder höher).

## <a name="connecting-to-homekit"></a>Herstellen einer Verbindung mit HomeKit

Die Kommunikation mit HomeKit Ihre app Xamarin.iOS zunächst eine Instanz instanziieren muss die `HMHomeManager` Klasse. Der Start-Manager ist die zentralen Einstiegspunkt in HomeKit und ist verantwortlich für die Bereitstellung einer Liste der verfügbaren greift aktualisieren und Verwalten dieser Liste des Benutzers zurückgeben _primären Home_.

Die `HMHome` Objekt enthält alle Informationen über eine erteilen Home, z. B. alle Räume, Gruppen oder Zonen, die sie zusammen mit jeder home Automation Zubehör enthalten können, die installiert wurden. Damit keine Vorgänge können, in HomeKit, mindestens eine ausgeführt werden `HMHome` erstellt und als die primäre Home zugewiesen werden muss.

Ihrer app ist verantwortlich für überprüft, ob eine primäre Home vorhanden ist und das Erstellen und einen zuweisen, wenn dies nicht der Fall.

### <a name="adding-a-home-manager"></a>Einen Home-Manager hinzufügen

Um eine app Xamarin.iOS HomeKit Awareness hinzuzufügen, bearbeiten die **AppDelegate.cs** Datei bearbeiten, und stellen sie wie folgt aussehen:

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

Wenn die Anwendung zuerst ausgeführt wird, wird der Benutzer aufgefordert werden, wenn sie ihm ermöglichen, ihre HomeKit Informationen zugreifen möchten:

[ ![](homekit-images/home01.png "Benutzer werden aufgefordert werden, wenn sie ihm ermöglichen, ihre HomeKit Informationen zugreifen möchten")](homekit-images/home01.png)

Wenn der Benutzer annimmt **OK**, und klicken Sie dann die Anwendung mit ihren HomeKit Zubehör arbeiten kann andernfalls wird es nicht, und alle Aufrufe von HomeKit mit einem Fehler fehl.

Mit der Home-Manager an neben müssen die Anwendung festzustellen, ob eine primäre Home konfiguriert wurde, und falls nicht, bieten eine Möglichkeit für den Benutzer erstellen und Zuweisen eines.

### <a name="accessing-the-primary-home"></a>Zugreifen auf das primäre Home-Verzeichnis

Wie oben erwähnt, muss eine primäre Home erstellt und konfiguriert sein, bevor HomeKit verfügbar ist, und es handelt sich um die app-Verantwortung, bieten eine Möglichkeit für den Benutzer erstellen und Zuweisen einer primären Home, sofern nicht bereits vorhanden ist.

Wenn Ihre app wird zuerst gestartet wird oder vom Hintergrund zurückgibt, muss Sie zum Überwachen der `DidUpdateHomes` -Ereignis für die `HMHomeManager` Klasse, um das Vorhandensein eines Hauses primären überprüfen. Wenn noch nicht vorhanden ist, sollten sie eine Schnittstelle für den Benutzer erstellen, bereitstellen.

Der folgende Code kann auf einen Domänencontroller Sicht für die häusliche primären überprüfen hinzugefügt werden:

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

Wenn der Start-Manager eine Verbindung mit HomeKit, herstellt der `DidUpdateHomes` Ereignis wird ausgelöst, werden alle vorhandenen greift in der Manager-Auflistung von Haushalten geladen und die Startseite der primären geladen, sofern verfügbar.

### <a name="adding-a-primary-home"></a>Hinzufügen einer primären-Startseite

Wenn die `PrimaryHome` Eigenschaft der `HMHomeManager` ist `null` nach einer `DidUpdateHomes` Ereignis müssen Sie bieten eine Möglichkeit für den Benutzer erstellen und primären Heim-zuweisen, bevor Sie fortfahren.

Die app wird in der Regel ein Formular für den Benutzer um ein neues Zuhause zu benennen, die dann für den Home Setup als das primäre Home-Verzeichnis-Manager übergeben ruft vorgelegt. Für die **HomeKitIntro** Beispiel-app, eine modale Sicht wurde in der IOS-Designer erstellt und aufgerufen werden, indem die `AddHomeSegue` segue aus die Hauptkomponente der Benutzeroberfläche der app.

Es enthält ein Textfeld für den Benutzer zur Eingabe eines Namens für die neue Startseite "und" eine Schaltfläche auf der Startseite hinzufügen. Wenn der Benutzer tippt der **Add Home** Schaltfläche, der folgende Code Ruft den Home-Manager, um das Home-Verzeichnis hinzuzufügen:

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

Die `AddHome` Methode versucht, neue Startseite erstellen und an die angegebenen Rückrufroutine zurückgeben. Wenn die `error` Eigenschaft ist nicht `null`, ein Fehler aufgetreten, und es sollte dem Benutzer angezeigt werden. Die häufigsten Fehler sind durch einen nicht eindeutigen home-Namen oder den Home-Manager, nicht mehr für die Kommunikation mit HomeKit verursacht.

Wenn das Home-Verzeichnis wurde erfolgreich erstellt wurde, müssen Sie zum Aufrufen der `UpdatePrimaryHome` Methode, um die neue Startseite als Startseite der primären einzurichten. Wenn die `error` Eigenschaft ist nicht `null`, ein Fehler aufgetreten, und es sollte dem Benutzer angezeigt werden.

Überwachen Sie auch die Startseite des Managers `DidAddHome` und `DidRemoveHome` Ereignisse, und aktualisieren Sie die app-Benutzeroberfläche nach Bedarf.

> [!IMPORTANT]
> **Hinweis:** der `AlertView.PresentOKAlert` im obigen Beispielcode verwendete Methode ist eine Hilfsklasse, die in der HomeKitIntro-Anwendung, das Arbeiten mit iOS Warnungen einfacher macht.


## <a name="finding-new-accessories"></a>Suchen von neuen Zubehör

Sobald eine primäre Home definiert oder vom Home-Manager geladen wurde, kann Ihre app Xamarin.iOS Aufrufen der `HMAccessoryBrowser` finden alle neuen home Automation Zubehör und fügen Sie eine Startseite hinzu.

Rufen Sie die `StartSearchingForNewAccessories` -Methode zum Starten der Suche nach neuen Zubehör und die `StopSearchingForNewAccessories` Methode, wenn Sie fertig sind.

> [!IMPORTANT]
> **Hinweis:** `StartSearchingForNewAccessories` sollten nicht bleiben für längere Zeit ausgeführt werden, da er sowohl die Akkulaufzeit als auch die Leistung des iOS-Geräts beeinträchtigt wird. Apple empfiehlt Aufrufen `StopSearchingForNewAccessories` nach einer Minute oder nur zu suchen, wenn die Zubehör-Benutzeroberfläche suchen, die dem Benutzer angezeigt wird.

Die `DidFindNewAccessory` Ereignis wird aufgerufen, wenn neue Zubehör werden ermittelt, und sie werden zur hinzugefügt werden die `DiscoveredAccessories` Liste im Browser Zubehör.

Die `DiscoveredAccessories` Liste enthält eine Auflistung von `HMAccessory` Objekten, die einem angegebenen HomeKit definieren aktiviert home Automation-Gerät und die verfügbaren Dienste, z. B. Leuchten oder Garage Tür Steuerelemente.

Nachdem die neue Zugriffsmethode gefunden wurde, es sollte dem Benutzer angezeigt werden und daher können sie es auswählen und ein Haus hinzugefügt. Beispiel:

[ ![](homekit-images/accessory01.png "Suchen einen neuen Zubehör")](homekit-images/accessory01.png)

Rufen Sie die `AddAccessory` Methode, um die ausgewählten Zubehör, Startseite der Auflistung hinzugefügt. Zum Beispiel:

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

Wenn die `err` Eigenschaft ist nicht `null`, ein Fehler aufgetreten, und es sollte dem Benutzer angezeigt werden. Andernfalls wird der Benutzer aufgefordert werden, durch den Setupcode für das Gerät hinzuzufügende eingeben:

[ ![](homekit-images/accessory02.png "Geben Sie den Setupcode für das Gerät hinzufügen")](homekit-images/accessory02.png)

Im Simulator Zubehör HomeKit diese Nummer finden Sie unter der **Setupcode** Feld:

[ ![](homekit-images/accessory03.png "Das Feld Setupcode im Simulator Zubehör HomeKit")](homekit-images/accessory03.png)

Für echte HomeKit Zubehör wird durch den Setupcode entweder auf eine Bezeichnung auf dem Gerät selbst, auf die im Produkt oder in den Accessor-Benutzerhandbuch gedruckt werden.

Überwachen Sie die zusätzlichen Browser `DidRemoveNewAccessory` Ereignis und Aktualisieren der Benutzer der Benutzeroberfläche Zubehör aus der Liste der verfügbaren entfernen, nachdem der Benutzer auf ihre Startseite-Auflistung hinzugefügt wurde.

## <a name="working-with-accessories"></a>Arbeiten mit Zubehör

Einmal primären Heim- und eingerichtet wurde und Zubehör wurden hinzugefügt, um es, Sie können eine Liste von Zubehör (und optional Räume) für den Benutzer zur Bearbeitung vorhanden.

Die `HMRoom` Objekt enthält alle Informationen zu einem bestimmten Platz und alle Zubehör, der sie angehören. Räume können optional in einer oder mehreren Zonen organisiert werden. Ein `HMZone` enthält alle Informationen über eine bestimmte Zone und alle der Räume, der sie angehören.

Für dieses Beispiel fügen wir Dinge einfach und das Arbeiten mit ein Haus Zubehör direkt, anstatt Sie sie in Räume oder Zonen organisieren beibehalten werden.

Die `HMHome` Objekt enthält eine Liste der zugewiesenen Zubehör, die dem Benutzer angezeigt werden, kann seine `Accessories` Eigenschaft. Zum Beispiel:

[ ![](homekit-images/accessory04.png "Beispiel für Zubehör")](homekit-images/accessory04.png)

Hier bilden, kann der Benutzer wählen Sie einen bestimmten Zubehör und Arbeiten mit den Diensten, die es bereitstellt.

## <a name="working-with-services"></a>Arbeiten mit Diensten

Wenn der Benutzer mit einem bestimmten HomeKit aktiviert home Automation Gerät interagiert, ist es in der Regel über die Dienste, die es bereitstellt. Die `Services` Eigenschaft von der `HMAccessory` Klasse enthält eine Auflistung von `HMService` -Objekten, die Dienste definieren, ein Geräts bietet.

Dienste sind z. B. Leuchten, Thermostate, Garage Tür Dosenöffner, Switches oder sperren. Einige Geräte (z. B. eine Garage Tür dateiöffnungsfunktion) stellt mehrere Dienste, z. B. eine helle und die Berechtigung zum Öffnen oder Schließen einer Tür bereit.

Zusätzlich zu den bestimmte Dienste, die einen bestimmten Zubehör bereitstellt, jedes Zubehör enthalten eine `Information Service` , definiert die Eigenschaften wie Name, Hersteller, Modell und Seriennummer.

### <a name="accessory-service-types"></a>Zubehörs Diensttypen

Die folgenden Diensttypen stehen über die `HMServiceType` Enum:

- **AccessoryInformation** – enthält Informationen zu dem angegebenen home Automation-Gerät (Zubehör).
- **AirQualitySensor** -einen per Funk Qualität Sensor definiert.
- **Akku** -definiert den Status des Akkus ein Zubehör.
- **CarbonDioxideSensor** -definiert eine CO2 Sensor hin.
- **CarbonMonoxideSensor** -Sensor Kohlenmonoxid definiert.
- **ContactSensor** -definiert einen Kontakten Sensor (z. B. ein Fenster geöffnet bzw. geschlossen wird).
- **Die Tür** -Sensor Zustand Tür (z. B. geöffnet oder geschlossen) definiert.
- **Lüfter** -remote gesteuerten Lüfter definiert.
- **GarageDoorOpener** -eine Garage Tür dateiöffnungsfunktion definiert.
- **HumiditySensor** -Sensor Luftfeuchtigkeit definiert.
- **LeakSensor** -einen Speicherverlust Sensor (wie für eine Durchlauferhitzer oder Gerätes) definiert.
- **Glühbirne** -definiert eine eigenständige Anwendung oder ein Licht, die Teil einer anderen Zubehör (z. B. eine Garage Tür dateiöffnungsfunktion) ist.
- **LightSensor** -light Sensor definiert.
- **LockManagement** -definiert einen Dienst, der eine Sperre für die automatisierte Tür verwaltet.
- **LockMechanism** -definiert eine remote gesteuerte Sperre (z. B. eine Sperre Tür).
- **MotionSensor** -einen Bewegungssensor definiert.
- **OccupancySensor** -definiert einen Sensor erforderlich sind.
- **Steckdose** -definiert eine remote gesteuerten Steckdose.
- **SecuritySystem** -ein home Sicherheitssystem definiert.
- **StatefulProgrammableSwitch** -definiert einen programmierbaren Schalter an, der in einem angegebenen Zustand einmal ausgelöst (z. B. ein Flip-Switch) bleibt.
- **StatelessProgrammableSwitch** -definiert eine programmierbare Switches, die auf ihren ursprünglichen Zustand zurückgibt, nach dem (z. B. eine Schaltfläche) ausgelöst wird.
- **SmokeSensor** -daraus Sensor definiert.
- **Wechseln Sie** -ein-/Ausschalter wie ein standard Wall-Switch definiert.
- **Temperatursensor** -Temperatursensors definiert.
- **Thermostat** -ein intelligentes Thermostat zum Steuern von einem HKL-System definiert.
- **Fenster** -definiert eine automatisierte Fenster Zuckerrohr Remote geöffnet oder geschlossen werden.
- **WindowCovering** -definiert eine Remote gesteuerten Fenster verdeckt werden, z. B. blenden, der geöffnet oder geschlossen werden können.

### <a name="displaying-service-information"></a>Anzeigen von Serviceinformationen zu

Nach dem Laden einer `HMAccessory` können Sie die einzelnen Abfragen `HNService` Objekte, er bietet, und diese Informationen für den Benutzer anzeigen:

[ ![](homekit-images/accessory05.png "Anzeigen von Serviceinformationen zu")](homekit-images/accessory05.png)

Sollten Sie immer überprüfen sollten die `Reachable` Eigenschaft eine `HMAccessory` bevor Sie versuchen, um damit zu arbeiten. Zubehör kann nicht erreichbar. der Benutzer nicht innerhalb des Geräts ist oder wenn nicht angeschlossen sein.

Nachdem ein Dienst ausgewählt wurde, kann, die diesem Dienst Remoteüberwachung oder-Steuerung einer angegebenen home Automation-Gerät eine oder mehrere Merkmale der Benutzer anzeigen oder ändern.

<a name="Working-with-Characteristics" />

## <a name="working-with-characteristics"></a>Arbeiten mit Eigenschaften

Jede `HMService` -Objekt enthalten kann eine Auflistung von `HMCharacteristic` Objekte, die Aufschluss darüber geben den Status des Diensts (z. B. einer Tür geöffnet bzw. geschlossen wird) oder ermöglicht dem Benutzer einen Zustand (z. B. das Festlegen der Farbe für das Licht) anpassen können.

`HMCharacteristic` nicht nur enthält Informationen über ein Merkmal und seinen Status, jedoch bietet auch Methoden für die Arbeit mit dem Status über _Merkmal Metadaten_ (`HMCharacteristisMetadata`). Diese Metadaten kann Eigenschaften (z. B. Wertebereiche der minimalen und maximalen) bereitstellen, die hilfreich beim Anzeigen von Informationen auf den Benutzer oder ihnen ermöglicht sind, um den Status zu ändern.

Die `HMCharacteristicType` Enum bietet eine Reihe von Merkmal Metadatenwerte, die definiert, oder wie folgt geändert werden können:

 - AdminOnlyAccess
 - AirParticulateDensity
 - AirParticulateSize
 - AirQuality
 - AudioFeedback
 - BatteryLevel
 - Helligkeit
 - CarbonDioxideDetected
 - CarbonDioxideLevel
 - CarbonDioxidePeakLevel
 - CarbonMonoxideDetected
 - CarbonMonoxideLevel
 - CarbonMonoxidePeakLevel
 - ChargingState
 - ContactState
 - CoolingThreshold
 - CurrentDoorState
 - CurrentHeatingCooling
 - CurrentHorizontalTilt
 - CurrentLightLevel
 - CurrentLockMechanismState
 - CurrentPosition
 - CurrentRelativeHumidity
 - CurrentSecuritySystemState
 - CurrentTemperature
 - CurrentVerticalTilt
 - FirmwareVersion
 - HardwareVersion
 - HeatingCoolingStatus
 - HeatingThreshold
 - HoldPosition
 - Farbton
 - Identifizieren (Identify)
 - InputEvent
 - LeakDetected
 - LockManagementAutoSecureTimeout
 - LockManagementControlPoint
 - LockMechanismLastKnownAction
 - Protokolle
 - Hersteller
 - Modell
 - MotionDetected
 - name
 - ObstructionDetected
 - OccupancyDetected
 - OutletInUse
 - OutputState
 - PositionState
 - PowerState
 - RotationDirection
 - RotationSpeed
 - Sättigung
 - Seriennummer
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
 - TargetTemperature
 - TargetVerticalTilt
 - TemperatureUnits
 - Version

### <a name="working-with-a-characteristics-value"></a>Arbeiten mit ein Merkmal Wert

Aufrufen, um sicherzustellen, dass Ihre app wurde den aktuellen Status von einem bestimmten Merkmal, das `ReadValue` Methode der `HMCharacteristic` Klasse. Wenn die `err` Eigenschaft ist nicht `null`, Fehler und möglicherweise noch nicht dem Benutzer angezeigt werden kann.

Des Merkmals `Value` Eigenschaft enthält den aktuellen Status des angegebenen Merkmals als ein `NSObject`, und kann nicht als solche direkt in c# mit bearbeitet werden.

Um den Wert zu lesen, wurde die folgende Hilfsklasse hinzugefügt, um die **HomeKitIntro** beispielanwendung:

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

Die `NSObjectConverter` wird verwendet, wenn die Anwendung den aktuellen Status des ein Merkmal lesen muss. Beispiel:

```csharp
var value = NSObjectConverter.ToFloat (characteristic.Value);
```

Die obige Zeile konvertiert den Wert in einer `float` , klicken Sie dann im Xamarin C#-Code verwendet werden kann.

So ändern Sie eine `HMCharacteristic`, rufen Sie die `WriteValue` Methode und umschließen Sie den neuen Wert in eine `NSObject.FromObject` aufrufen. Beispiel:

```csharp
Characteristic.WriteValue(NSObject.FromObject(value),(err) =>{
    // Was there an error?
    if (err!=null) {
        // Yes, inform user
        AlertView.PresentOKAlert("Update Error",err.LocalizedDescription,Controller);
    }
});
```

Wenn die `err` Eigenschaft ist nicht `null`, ein Fehler aufgetreten ist und dem Benutzer angezeigt werden soll.

### <a name="testing-characteristic-value-changes"></a>Charakteristische Wertänderungen testen

Bei der Arbeit mit `HMCharacteristics` und "simulierten Zubehör", "Änderungen an der `Value` Eigenschaft innerhalb der HomeKit Zubehör Simulator überwacht werden kann.

Mit der **HomeKitIntro** real IOS-Gerätehardware, Änderungen an ein Merkmal Wert Ausführen einer app im Simulator Zubehör HomeKit fast sofort angezeigt werden sollte. Z. B. Ändern des Status von Licht in der iOS-app:

[ ![](homekit-images/test01.png "Ändern des Status von Licht in einer iOS-app")](homekit-images/test01.png)

Der Status der Lichtquelle im Simulator Zubehör HomeKit sollte geändert werden. Wenn der Wert nicht ändern, überprüfen Sie den Status der Fehlermeldung beim Schreiben von neuen Merkmal Werte, und stellen Sie sicher, dass die Zugriffsmethode weiterhin erreichbar ist.

## <a name="advanced-homekit-features"></a>Erweiterte HomeKit-Funktionen

In diesem Artikel wurden die grundlegenden Funktionen zum Arbeiten mit HomeKit Zubehör in einer app Xamarin.iOS erforderlich behandelt. Es gibt jedoch einige erweiterte Funktionen des HomeKit, die nicht in dieser Einführung abgedeckt werden:

- **Räume** -HomeKit aktiviert Zubehör können optional in Räume vom Endbenutzer organisiert. Dadurch können HomeKit vorhanden Zubehör auf eine Weise, die für den Benutzer zu verstehen und Arbeiten mit einfach ist. Weitere Informationen zum Erstellen und Verwalten von Räume finden Sie in der Apple [HMRoom](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMRoom_Class/index.html#//apple_ref/occ/cl/HMRoom) Dokumentation.
- **Zonen** -Räume können optional in Zonen organisiert werden, durch den Endbenutzer. Eine Zone bezieht sich auf eine Auflistung von Räume, die der Benutzer als einzelne Einheit behandelt werden. Zum Beispiel: sollten, unteren Stock oder Untergeschoss. Erneut, können diese HomeKit und Arbeiten mit Zubehör in einer Weise, die für den Endbenutzer sinnvoll ist. Weitere Informationen zum Erstellen und Verwalten von Zonen finden Sie in der Apple [HMZone](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMZone_Class/index.html#//apple_ref/occ/cl/HMZone) Dokumentation.
- **Aktionen und Aktion Legt** -Aktionen Zubehörs Dienst Merkmale ändern und zu Sätzen zusammengefasst werden können. Aktion Legt fungieren als Skripts zum Steuern des einer Gruppenstatus von Zubehör und ihre Aktionen zu koordinieren. Beispielsweise kann ein "Fernsehen"-Skript schließen die blenden dim Leuchten und Fernsehgerät und seine sound System aktivieren. Weitere Informationen zum Erstellen und Verwalten von Aktionen und Aktion Legt finden Sie in der Apple [HMAction](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMAction_Class/index.html#//apple_ref/occ/cl/HMAction) und [HMActionSet](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMActionSet_Class/index.html#//apple_ref/occ/cl/HMActionSet) Dokumentation.
- **Trigger** : kann keinen Trigger aktivieren, eine oder mehrere Aktion festgelegt, wenn eine bestimmte Gruppe von Bedingungen erfüllt. Z. B. das Licht Portch aktivieren, und alle externen Türen bei dunklen außerhalb herankommt gesperrt. Weitere Informationen zum Erstellen und Verwalten von Triggern finden Sie in der Apple [HMTrigger](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMTrigger_Class/index.html#//apple_ref/occ/cl/HMTrigger) Dokumentation.

Da diese Funktionen die gleichen Techniken, die oben genannten verwenden, sollten sie einfach zu implementieren, indem Sie folgenden Apple werden [HomeKitDeveloper Handbuch](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html), [Richtlinien zur Benutzeroberfläche HomeKit](https://developer.apple.com/homekit/ui-guidelines/) und [ HomeKit Frameworkverweis](https://developer.apple.com/library/ios/home_kit_framework_ref).

## <a name="homekit-app-review-guidelines"></a>App-Rezensionsrichtlinien HomeKit

Vor der Übermittlung eine HomeKit aktivierte Xamarin.iOS app iTunes Connect für die Version im iTunes App Store stellen Sie sicher, führen Sie die Apple Richtlinien für apps HomeKit aktiviert:

 - Die app dienen in erster Linie _müssen_ home Automation werden, wenn HomeKit Framework verwenden.
 - Marketing Text für die app muss die Benutzer benachrichtigen, HomeKit verwendet wird, müssen sie eine Datenschutzrichtlinie angeben.
 - Sammeln von Benutzerinformationen oder mithilfe von HomeKit für Werbung ist unzulässig.

Überprüfen Sie für die vollständige Richtlinien, finden Sie in der Apple- [App Store überprüfen Guidelines](https://developer.apple.com/app-store/review/guidelines/).

## <a name="whats-new-in-ios-9"></a>Was ist neu in iOS 9

Apple hat die folgenden Änderungen und Ergänzungen an HomeKit für iOS 9 vorgenommen:

- **Verwalten von vorhandenen Objekten** – Wenn Sie vorhandene Zubehör geändert wurde, ist der Start-Manager (`HMHomeManager`) informiert Sie über das bestimmte Element, das geändert wurde.
- **Dauerhafte Bezeichner** -alle relevante HomeKit Klassen enthalten nun einen `UniqueIdentifier` Eigenschaft zur eindeutigen Identifizierung der ein bestimmtes Element über HomeKit aktiviert apps (oder Instanzen derselben App).
- **Benutzerverwaltung** -eine integrierte Ansicht-Controllers zur benutzerverwaltung Benutzer bereitzustellen, die über Zugriff auf die Geräte HomeKit im Stamm des primären Benutzers hinzugefügt.
- **Benutzeroptionen** - HomeKit Benutzer hat eine Gruppe von Berechtigungen, die steuern, welche Funktionen sie in HomeKit verwenden können und HomeKit aktiviert Zubehör. Ihre app sollten relevante Funktionen nur für den aktuellen Benutzer angezeigt werden. Beispielsweise sollte nur eine "Administratoren" auf andere Benutzer verwalten können.
- **Vordefinierte Szenen** -vordefinierten Szenen für vier allgemeine Ereignisse, die auftreten, für den durchschnittlichen HomeKit Benutzer erstellt wurden: aktiv, lassen Sie, zurück, wechseln Sie zu Bett. Diese vordefinierten Szenen können nicht aus einem nicht gelöscht werden.
- **Im Hintergrund beim Übersetzen und Siri** -Siri hat eine umfassendere Unterstützung für Szenen in iOS 9 und kann den Namen des jede Szene in HomeKit definierten erkennen. Ein Benutzer kann eine Szene ausführen, durch den Namen in Siri sprechen.
- **Zusätzliche Kategorien** -eine Reihe von vordefinierten Kategorien für alle Zubehör und hilft bei der Identifizierung des Typs des hinzuzufügenden ein Haus Zubehör hinzugefügt oder aus innerhalb Ihrer app bearbeitet wurde. Diese neuen Kategorien sind verfügbar, während des Setups Zubehör.
- **Unterstützung von Apple Watch** - HomeKit ist jetzt für WatchOS verfügbar und werden der Apple Watch-HomeKit aktivierten Geräten ohne iPhone wird in der Nähe der Apple Watch steuern können. HomeKit für WatchOS unterstützt die folgenden Funktionen: Anzeigen von greift, steuern Zubehör und Szenen ausführen.
- **Neuen Ereignistyp Trigger** : Zusätzlich zu den Typ timertrigger unterstützt iOS 8, iOS 9 nun unterstützt Ereignistriggern auf Zubehör Status (z. B. Sensordaten) oder Geolocation basieren. Verwenden von Ereignistriggern `NSPredicates` Bedingungen für ihre Ausführung festzulegen.
- **Remotezugriff** -mit Remotezugriff auf der Benutzer kann nun Steuerelement ihre HomeKit Startseite Automation Zubehör aktiviert, wenn sie nicht die House an einem Remotestandort sind. In iOS 8 wurde dies nur unterstützt, wenn der Benutzer eine 3. Generation Apple TV in das Home-Verzeichnis hat. In iOS 9 diese Einschränkung wird aufgehoben, und der Remotezugriff über die iCloud und die HomeKit Zubehör-Protokoll (HAP) unterstützt wird.
- **Neue Bluetooth geringem Energieverbrauch (aktivieren) Fähigkeiten** -HomeKit unterstützt jetzt mehr Zubehörs Typen, die über das Protokoll Bluetooth geringem Energieverbrauch (aktivieren) kommunizieren können. HAP Secure Tunneling verwenden, können eine HomeKit Zubehör verfügbar machen eine andere Bluetooth Zubehör über Wi-Fi (wenn es außerhalb des gültigen Bereichs Bluetooth ist). In iOS 9 haben des Zubehör vollständige Unterstützung für Benachrichtigungen und Metadaten.
- **Neue Kategorien für Zubehör** -Apple iOS 9 die folgenden neuen Zubehör-Kategorien hinzugefügt: Fenster aufgeführten, Motor Türen und Windows, Alarmsysteme, Sensoren und programmierbaren Switches.

Weitere Informationen zu den neuen Funktionen der HomeKit in iOS 9, finden Sie im Apple [HomeKit Index](https://developer.apple.com/homekit/) und [Neuigkeiten in HomeKit](https://developer.apple.com/videos/wwdc/2015/?id=210) video.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde eingeführt, Apple HomeKit home Automatisierungs-Frameworks. Einrichten und Konfigurieren des HomeKit Zubehör Simulators Testgeräte und zum Erstellen einer einfachen Xamarin.iOS-app, um zu ermitteln, mit kommunizieren und steuern home Automatisierungsgeräten mithilfe von HomeKit bleiben.



## <a name="related-links"></a>Verwandte Links

- [iOS 9-Beispiele](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [Was ist neu in iOS 9.0 verfügen](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [HomeKitDeveloper-Handbuch](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html)
- [HomeKit Richtlinien zur Benutzeroberfläche](https://developer.apple.com/homekit/ui-guidelines/)
- [HomeKit Frameworkverweis](https://developer.apple.com/library/ios/home_kit_framework_ref)
