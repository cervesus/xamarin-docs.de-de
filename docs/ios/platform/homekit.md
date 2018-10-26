---
title: HomeKit in Xamarin.iOS
description: HomeKit ist Apple Framework zum Steuern der heimautomatisierungsgeräte. Dieser Artikel führt HomeKit und Konfigurieren von Test-Zubehör in HomeKit-Zubehör Simulator und eine einfache Xamarin.iOS-app schreiben, für die Interaktion mit diesen Zubehör behandelt.
ms.prod: xamarin
ms.assetid: 90C0C553-916B-46B1-AD52-1E7332792283
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: 22b6fd101c0b983fe7b4a7d0891dc4674a11a02a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121190"
---
# <a name="homekit-in-xamarinios"></a>HomeKit in Xamarin.iOS

_HomeKit ist Apple Framework zum Steuern der heimautomatisierungsgeräte. Dieser Artikel führt HomeKit und Konfigurieren von Test-Zubehör in HomeKit-Zubehör Simulator und eine einfache Xamarin.iOS-app schreiben, für die Interaktion mit diesen Zubehör behandelt._

[![](homekit-images/accessory01.png "Ein Beispiel für HomeKit-fähigen App")](homekit-images/accessory01.png#lightbox)

Apple führte HomeKit IOS 8 als eine Möglichkeit, mehrere heimautomatisierungsgeräte aus einer Vielzahl Hersteller nahtlos in eine einzelne, zusammenhängende Einheit zu integrieren. Durch das Heraufstufen von eines gemeinsamen Protokolls für die Ermittlung von ermöglicht konfigurieren und Steuern von heimautomatisierungsgeräte, HomeKit Geräte von nicht-bezogene Anbietern, ohne zum Koordinieren der Arbeit mit einzelnen Lieferanten zusammenarbeiten.

Mit HomeKit können Sie eine Xamarin.iOS-app erstellen, die einem beliebigen Gerät aktiviert HomeKit steuert, ohne mit Anbieter-APIs oder apps bereitgestellt. HomeKit können, Sie folgende Möglichkeiten:

- Erkennen Sie neue HomeKit aktiviert heimautomatisierungsgeräte, und fügen Sie sie in einer Datenbank, die für alle iOS-Geräte des Benutzers beibehalten werden.
- Einrichtung, Konfiguration, anzeigen und steuern Sie ein Gerät in die HomeKit _Startseite Konfigurationsdatenbank_.
- Kommunikation mit dem alle vorkonfiguriert, dass HomeKit-Gerät, und diese einzelne Aktionen oder arbeiten zusammen, wie die Aktivierung aller Lichter in der Küche Befehl.

Zusätzlich zur Bereitstellung von Geräten in der Konfigurationsdatenbank Startseite aktiviert HomeKit-Apps, stehen HomeKit Sprachbefehle Siri. Angesichts eine entsprechend konfigurierte HomeKit-Konfiguration, die Benutzer kann Befehle Voice wie z. B. "Siri, aktivieren Sie die Lichter im Wohnzimmer."

<a name="Home-Configuration-Database" />

## <a name="the-home-configuration-database"></a>Der Home-Konfigurationsdatenbank

HomeKit werden alle Automation-Geräte in einem bestimmten Standort in einer Auflistung Startseite organisiert. Diese Auflistung bietet eine Möglichkeit für den Benutzer ihre Geräte für die heimautomatisierung in logisch angeordnete Einheiten mit sinnvollen, aussagekräftigen lesbare Bezeichnungen gruppiert.

Die Home-Sammlung wird in einer Start-Konfigurationsdatenbank gespeichert, die automatisch gesichert und Synchronisierung auf allen iOS-Geräte des Benutzers. HomeKit bietet die folgenden Klassen für die Arbeit mit der Konfigurationsdatenbank Startseite:

- `HMHome` – Dies ist ein Container mit der obersten Ebene, die alle Informationen und Konfigurationen für alle heimautomatisierungsgeräte in einem einzigen physischen Ort (z. b. ein einzelnes Familie Wohnort). Der Benutzer möglicherweise mehr als eine Wohnort, z. B. ihre wichtigsten Startseite "und" eines Hauses Urlaub. Oder haben möglicherweise verschiedene "enthält" für die gleiche Eigenschaft, z. B. das wichtigste Haus und ein Haus Gast über die Garage selbst. In beiden Fällen mindestens `HMHome` Objekt _müssen_ eingerichtet werden und gespeichert, bevor alle anderen HomeKit-Infos eingegeben werden kann.
- `HMRoom` -While optional, einen `HMRoom` ermöglicht dem Benutzer, die bestimmte Zimmer innerhalb eines Hauses zu definieren (`HMHome`) wie z. B.: Küche Badezimmer, Garage oder Wohnzimmer. Der Benutzer kann alle heimautomatisierungsgeräte an einem bestimmten Ort in ihrer Haus in die Gruppe eine `HMRoom` und als Einheit reagieren. Stellen Sie z. B. Siri die Garage Lichter zu deaktivieren.
- `HMAccessory` -Dies steht für eine Person, die physischen HomeKit aktiviert Automation-Gerät, das des Benutzers Wohnort (z. B. ein intelligentes Thermostat) installiert wurde. Jede `HMAccessory` zugewiesen ist eine `HMRoom`. Wenn der Benutzer Räume nicht konfiguriert werden, wird ein spezieller standardraum von HomeKit Zubehör zugewiesen.
- `HMService` – Stellt einen Dienst von einer bestimmten `HMAccessory`, z. B. den ein/aus-Status, der ein Licht oder seine Farbe (wenn es sich um eine Farbe ändern unterstützt wird). Jede `HMAccessory` haben mehr als ein Dienst, z. B. eine Garage Tür dateiöffnungsfunktion, die auch ein Licht enthält. Darüber hinaus eine bestimmte `HMAccessory` möglicherweise die Dienste, z. B. Firmwareupdate, die außerhalb von Benutzersteuerelement sind.
- `HMZone` : Ermöglicht dem Benutzer eine Auflistung von gruppieren `HMRoom` Objekte in logische Zonen, z. B. sollten, unteren Stock oder Keller. Zwar optional, ermöglicht dies Interaktionen wie Siri stellen alle das Licht aus unteren Stock aktivieren.

<a name="Provisioning-a-HomeKit-App" />

## <a name="provisioning-a-homekit-app"></a>Bereitstellen einer HomeKit-App

Aufgrund der sicherheitsanforderungen durch HomeKit auferlegt wird muss eine Xamarin.iOS-app, die das HomeKit-Framework verwendet ordnungsgemäß in das Apple Developer Portal und in der Xamarin.iOS-Projektdatei konfiguriert werden.

Führen Sie folgende Schritte aus:

1. Melden Sie sich bei der [Apple-Entwicklerportal](http://developer.apple.com).
2. Klicken Sie auf **Zertifikate, Bezeichner & Profile**.
3. Wenn Sie dies noch nicht getan haben, klicken Sie auf **Bezeichner** und erstellen Sie eine ID für Ihre app (z. B. `com.company.appname`), andernfalls bearbeiten Sie Ihre vorhandenen-ID.
4. Sicherstellen, dass die **HomeKit** Dienst für die angegebene ID überprüft wurden: 

    [![](homekit-images/provision01.png "Aktivieren Sie den Dienst \"HomeKit\" für die angegebene ID")](homekit-images/provision01.png#lightbox)
5. Speichern Sie die Änderungen.
4. Klicken Sie auf **Bereitstellungsprofile** > **Entwicklung** , und erstellen Sie ein neues entwicklungsbereitstellungsprofil für Ihre app: 

    [![](homekit-images/provision02.png "Erstellen Sie ein neues entwicklungsbereitstellungsprofil für die app")](homekit-images/provision02.png#lightbox)
5. Herunterladen Sie und installieren Sie das neue Bereitstellungsprofil oder verwenden Sie Xcode herunterladen und installieren das Profil.
6. Bearbeiten Sie die Optionen der Xamarin.iOS-Projekt, und stellen Sie sicher, dass Sie das Bereitstellungsprofil verwenden, das Sie gerade erstellt haben: 

    [![](homekit-images/provision03.png "Wählen Sie die soeben erstellte Bereitstellungsprofil")](homekit-images/provision03.png#lightbox)
7. Als Nächstes bearbeiten Ihrer **"Info.plist"** Datei, und stellen Sie sicher, dass Sie die App-ID verwenden, die verwendet wurde, um das Bereitstellungsprofil zu erstellen: 

    [![](homekit-images/provision04.png "Legen Sie die App-ID ")](homekit-images/provision04.png#lightbox)
8. Bearbeiten Sie abschließend Ihre **"Entitlements.plist"** Datei, und stellen sicher, dass die **HomeKit** Berechtigung ausgewählt wurde: 

    [![](homekit-images/provision05.png "Aktivieren Sie die HomeKit-Berechtigung")](homekit-images/provision05.png#lightbox)
9. Speichern Sie die Änderungen auf alle Dateien an.

Mit diesen Einstellungen vorhanden ist die Anwendung nun auf die HomeKit-Framework-APIs zugreifen. Ausführliche Informationen zur Bereitstellung finden Sie unter unseren [Gerätebereitstellung](~/ios/get-started/installation/device-provisioning/index.md) und [Bereitstellung Ihrer App](~/ios/get-started/installation/device-provisioning/index.md) Anleitungen.

> [!IMPORTANT]
> Testen einer aktiviert HomeKit-app erfordert ein realer iOS-Gerät, das ordnungsgemäß bereitgestellt wurde, für die Entwicklung. HomeKit kann nicht aus dem iOS-Simulators getestet werden.

## <a name="the-homekit-accessory-simulator"></a>Der Simulator HomeKit-Zubehör

Eine Möglichkeit, alle möglichen heimautomatisierungsgeräte und Dienste testen, ohne ein physisches Gerät, damit Apple erstellt die _HomeKit-Zubehör Simulator_. Dieser Simulator wird verwenden, können Sie einrichten und Konfigurieren von virtuellen HomeKit-Geräte.

### <a name="installing-the-simulator"></a>Installieren den Simulator

Apple bietet HomeKit-Zubehör Simulator als separater Download von Xcode, daher Sie es installieren, bevor Sie fortfahren müssen.

Führen Sie folgende Schritte aus:

1. Finden Sie in einem Webbrowser auf [Downloads für Apple-Entwickler](https://developer.apple.com/download/more/?name=for%20Xcode)
2. Herunterladen der **zusätzliche Tools für die Xcode-Xxx** (wobei Xxx die Version von Xcode, die Sie installiert ist): 

    [![](homekit-images/simulator01.png "Die zusätzlichen Tools für Xcode herunterladen")](homekit-images/simulator01.png#lightbox)
3. Öffnen Sie die Datenträger-Image, und installieren Sie die Tools in Ihre **Anwendungen** Verzeichnis.

Mit dem Simulator von HomeKit-Zubehör installiert können virtuelle Zubehör zu Testzwecken erstellt werden.

### <a name="creating-virtual-accessories"></a>Erstellen von virtuellen Zubehör

Um die HomeKit-Zubehör-Simulator starten, und erstellen einige virtuelle Zubehör, führen Sie folgende Schritte aus:

1. Starten Sie aus Ordner "Programme" den Simulator HomeKit-Zubehör: 

    [![](homekit-images/simulator02.png "Der Simulator HomeKit-Zubehör")](homekit-images/simulator02.png#lightbox)
2. Klicken Sie auf die **+** Schaltfläche, und wählen **neue Zubehör...** : 

    [![](homekit-images/simulator03.png "Hinzufügen einer neuen Zubehör")](homekit-images/simulator03.png#lightbox)
3. Füllen Sie die Informationen zu den neuen Zubehör, und klicken Sie auf die **Fertig stellen** Schaltfläche: 

    [![](homekit-images/simulator04.png "Füllen Sie die Informationen über die neuen Zugriffsmethode")](homekit-images/simulator04.png#lightbox)
4. Klicken Sie auf die **Dienst hinzufügen...** Schaltfläche, und wählen Sie aus der Dropdownliste einen Diensttyp: 

    [![](homekit-images/simulator05.png "Einen Diensttyp wird in der Dropdownliste auswählen.")](homekit-images/simulator05.png#lightbox)
5. Geben Sie einen **Namen** für den Dienst und klicken Sie auf die **Fertig stellen** Schaltfläche: 

    [![](homekit-images/simulator06.png "Geben Sie einen Namen für den Dienst")](homekit-images/simulator06.png#lightbox)
6. Sie können optionale Eigenschaften für einen Dienst bereitstellen, indem Sie auf die **hinzufügen Merkmal** Schaltfläche und die erforderlichen Einstellungen konfigurieren: 

    [![](homekit-images/simulator07.png "Die erforderlichen Einstellungen konfigurieren")](homekit-images/simulator07.png#lightbox)
7. Wiederholen Sie die oben beschriebenen Schritte zum Erstellen eines virtuellen heimautomatisierung Gerätetyp, die HomeKit unterstützt.

Mit einigen Beispiel virtuelle HomeKit-Zubehör erstellt und konfiguriert haben können Sie jetzt nutzen und diese Geräte in Ihrer Xamarin.iOS-app zu steuern.

## <a name="configuring-the-infoplist-file"></a>Konfigurieren die Datei "Info.plist"

Für iOS 10 (und höher), muss der Entwickler zum Hinzufügen der `NSHomeKitUsageDescription` -Schlüssel in der app `Info.plist` Datei, und geben Sie eine Zeichenfolge deklarieren warum die app des Benutzers HomeKit-Datenbank zugreifen will. Diese Zeichenfolge wird der Benutzer bei der ersten Zeit angezeigt, die Benutzer die app ausführen:

[![](homekit-images/info01.png "Das Dialogfeld \"HomeKit-Berechtigung\"")](homekit-images/info01.png#lightbox)

Um diesen Schlüssel festzulegen, führen Sie folgende Schritte aus:

1. Doppelklicken Sie auf die `Info.plist` Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen.
2. Wechseln Sie am unteren Rand des Bildschirms, um die **Quelle** anzeigen.
3. Fügen Sie einen neuen **Eintrag** der Liste.
4. Wählen Sie aus der Dropdown-Liste **Datenschutz – HomeKit-Nutzungsbeschreibung**: 

    [![](homekit-images/info02.png "Wählen Sie auf Datenschutz – HomeKit-Nutzungsbeschreibung")](homekit-images/info02.png#lightbox)
5. Geben Sie eine Beschreibung für warum die app des Benutzers HomeKit-Datenbank zugreifen will: 

    [![](homekit-images/info03.png "Geben Sie eine Beschreibung")](homekit-images/info03.png#lightbox)
6. Speichern Sie die Änderungen in der Datei.

> [!IMPORTANT]
> Fehler beim Festlegen der `NSHomeKitUsageDescription` -Schlüssel in der `Info.plist` Datei führt in der app _im Hintergrund fehlerhaften_ (wird vom System zur Laufzeit geschlossen) ohne Fehler bei der Ausführung im iOS 10 (oder höher).

## <a name="connecting-to-homekit"></a>Herstellen einer Verbindung mit HomeKit

Ihre Xamarin.iOS-app mit HomeKit kommunizieren kann, muss zunächst eine Instanz Instanziieren der `HMHomeManager` Klasse. Der Start-Manager ist der zentrale Einstiegspunkt in HomeKit und dient zur Angabe einer Liste der verfügbaren Häuser, aktualisieren und Verwalten der Liste aus, und Zurückgeben des Benutzers _primären Home_.

Die `HMHome` Objekt enthält alle Informationen über ein bieten Zuhause, einschließlich aller Räume, Gruppen oder Zonen, die es enthalten kann, sowie alle heimautomatisierung Zubehör, die installiert wurden. Damit keine Vorgänge können, in HomeKit, mindestens eine ausgeführt werden `HMHome` erstellt und als primäre Anlaufstelle zugewiesen werden muss.

Ihre app ist verantwortlich für das Überprüfen, ob ein Zuhause primären vorhanden ist, erstellen und Zuweisen von eine Falls dies nicht der Fall.

### <a name="adding-a-home-manager"></a>Hinzufügen eines Home-Managers

Um eine Xamarin.iOS-app HomeKit-Awareness hinzuzufügen, bearbeiten die **Datei "appdelegate.cs"** Datei bearbeiten, und stellen sie wie folgt aussehen:

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

Wenn die Anwendung zuerst ausgeführt wird, wird der Benutzer aufgefordert werden, wenn sie zulassen, damit ihre HomeKit-Informationen zugreifen möchten:

[![](homekit-images/home01.png "Der Benutzer wird aufgefordert werden, wenn sie zulassen, damit ihre HomeKit-Informationen zugreifen möchten")](homekit-images/home01.png#lightbox)

Wenn die Antwort lautet **OK**, und klicken Sie dann die Anwendung funktioniert mit ihren HomeKit-Zubehör kann andernfalls wird es nicht, und alle Aufrufe von HomeKit werden beim Auftreten eines Fehlers.

Mit der Start-Manager vorhanden als Nächstes muss die Anwendung zu ermitteln, ob ein Zuhause primären konfiguriert wurde, und falls nicht, geben eine Möglichkeit für den Benutzer erstellen und Zuweisen einer.

### <a name="accessing-the-primary-home"></a>Zugreifen auf die primäre-Homepage

Wie bereits erwähnt, muss ein Zuhause primären erstellt und konfiguriert, damit HomeKit verfügbar ist, und es ist, dass der Zuständigkeit der app eine Möglichkeit für den Benutzer erstellen und Zuweisen einer primären-Startseite, sofern nicht bereits vorhanden ist.

Wenn Ihre app zuerst gestartet oder im Hintergrund gibt, muss zum Überwachen der `DidUpdateHomes` Ereignis die `HMHomeManager` Klasse, um das Vorhandensein eines Hauses primären überprüfen. Wenn noch nicht vorhanden ist, sollten sie eine Schnittstelle für den Benutzer zum Erstellen bereitstellen.

Der folgende Code kann einen ansichtscontroller für die primäre Startseite überprüfen hinzugefügt werden:

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

Wenn der Start-Manager eine Verbindung zu HomeKit, stellt die `DidUpdateHomes` Ereignis wird ausgelöst, werden alle vorhandenen Häuser der Manager Sammlung von Benutzern gebahnt geladen und die primären-Startseite wird geladen, falls verfügbar.

### <a name="adding-a-primary-home"></a>Hinzufügen einer primären-Startseite

Wenn die `PrimaryHome` Eigenschaft der `HMHomeManager` ist `null` nach einer `DidUpdateHomes` -Ereignis, Sie bieten eine Möglichkeit für den Benutzer erstellen und Zuweisen einer primären Home vor dem Fortfahren müssen.

In der Regel stellen die app ein Formular für den Benutzer um ein neues Zuhause zu benennen, die dann an den Home-Manager mit der Einrichtung als primäre Anlaufstelle übergeben wird. Für die **HomeKitIntro** Beispiel-app, eine modale View wurde in der IOS-Designer erstellt und aufgerufen werden, indem die `AddHomeSegue` segue aus der Hauptbenutzeroberfläche der app.

Es bietet es sich um ein Textfeld für den Benutzer einen Namen für die neue Startseite und eine Schaltfläche zum Hinzufügen der Startseite eingeben. Wenn der Benutzer tippt der **hinzufügen Home** Schaltfläche der folgende Code Ruft den Home-Manager, um die Startseite hinzuzufügen:

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

Die `AddHome` Methode versucht, neue Startseite erstellen und an die angegebenen Rückrufroutine zurückgeben. Wenn die `error` Eigenschaft ist nicht `null`, ein Fehler ist aufgetreten, und es sollte dem Benutzer angezeigt werden. Die häufigsten Fehler werden durch einen nicht eindeutigen home-Namen oder den Home-Manager nicht die Möglichkeit, für die Kommunikation mit HomeKit verursacht.

Wenn der Start erfolgreich erstellt wurde, müssen Sie zum Aufrufen der `UpdatePrimaryHome` Methode, um die neue Startseite als primäre Anlaufstelle festzulegen. Erneut, wenn die `error` Eigenschaft ist nicht `null`, ein Fehler ist aufgetreten, und es sollte dem Benutzer angezeigt werden.

Sie sollten auch des Start-Managers Überwachen `DidAddHome` und `DidRemoveHome` Ereignisse, und aktualisieren Sie die app Benutzeroberfläche nach Bedarf.

> [!IMPORTANT]
> Die `AlertView.PresentOKAlert` in den obigen Beispielcode verwendete Methode ist eine Hilfsklasse, in der HomeKitIntro-Anwendung, mit der Arbeit mit dem iOS-Warnungen vereinfacht.


## <a name="finding-new-accessories"></a>Suchen nach neuen Zubehör

Sobald ein Zuhause primären definiert oder vom Home-Manager geladen wurde, kann Ihre Xamarin.iOS-app Aufrufen der `HMAccessoryBrowser` , suchen alle neuen heimautomatisierung Zubehör, und fügen sie ein Zuhause hinzu.

Rufen Sie die `StartSearchingForNewAccessories` Methode zum Starten der Suche nach neuen Zubehör und `StopSearchingForNewAccessories` Methode, wenn Sie fertig.

> [!IMPORTANT]
> `StartSearchingForNewAccessories` sollte nicht bleiben für lange Zeit ausgeführt werden, da es sowohl Akkulaufzeit und Leistung des iOS-Geräts beeinträchtigt wird. Apple empfiehlt Aufrufen `StopSearchingForNewAccessories` nach einer Minute oder nur suchen, wenn die Zubehör suchen, die dem Benutzer angezeigt wird.

Die `DidFindNewAccessory` Ereignis wird aufgerufen, wenn neue Zubehör ermittelt werden und sie hinzugefügt werden sollen die `DiscoveredAccessories` Liste im Browser-Zubehör.

Die `DiscoveredAccessories` Liste enthält eine Auflistung von `HMAccessory` Objekte, die eine bieten HomeKit definieren aktiviert heimautomatisierung-Gerät und die verfügbaren Dienste, z. B. Licht oder Garage Tür-Steuerelement.

Sobald die neue Zugriffsmethode gefunden wurde, es sollte dem Benutzer angezeigt werden, und daher können sie es auswählen und ein Zuhause hinzufügen. Beispiel:

[![](homekit-images/accessory01.png "Suchen eine neue Zubehör")](homekit-images/accessory01.png#lightbox)

Rufen Sie die `AddAccessory` Methode, um die ausgewählten Zugriffsmethode für den Start der Sammlung hinzuzufügen. Zum Beispiel:

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

Wenn die `err` Eigenschaft ist nicht `null`, ein Fehler ist aufgetreten, und es sollte dem Benutzer angezeigt werden. Andernfalls wird der Benutzer aufgefordert werden, um den Setupcode für das Gerät hinzufügen einzugeben:

[![](homekit-images/accessory02.png "Geben Sie den Setupcode für das Gerät hinzufügen")](homekit-images/accessory02.png#lightbox)

Im Simulator HomeKit-Zubehör diese Nummer finden Sie unter den **Setupcode** Feld:

[![](homekit-images/accessory03.png "Das Setup-Code-Feld im Simulator HomeKit-Zubehör")](homekit-images/accessory03.png#lightbox)

Für real HomeKit-Zubehör wird durch den Setupcode entweder auf eine Bezeichnung auf dem Gerät selbst, auf der produktverpackung oder in die Zugriffsmethode Benutzerhandbuch gedruckt werden.

Sie sollten überwachen, den Zubehör Browser `DidRemoveNewAccessory` Ereignisses und Aktualisieren der Benutzer-Schnittstelle aus der Liste der verfügbaren Zubehör entfernen, nachdem der Benutzer auf ihrer Startseite-Auflistung hinzugefügt wurde.

## <a name="working-with-accessories"></a>Arbeiten mit Zubehör

Einmal primären Heim- und eingerichtet wurde und Zubehör, wurden hinzugefügt, Sie können eine Liste von Zubehör (und optional Räume) darstellen, für den Benutzer zum Arbeiten mit.

Die `HMRoom` Objekt enthält alle Informationen über einen bestimmten Raum und alle Zubehör, die sie angehören. Räume können optional in einer oder mehreren Zonen organisiert werden. Ein `HMZone` enthält alle Informationen über eine bestimmte Zone und alle von den Räumen, die sie angehören.

Dieses Beispiel müssen wir Dinge einfach und arbeiten Sie mit der ein Zuhause Zubehör direkt, anstatt sie in den Räumen oder an Zonen organisieren beibehalten werden.

Die `HMHome` Objekt enthält eine Liste der zugewiesenen Zubehör, die für dem Benutzer in angezeigt werden, kann die `Accessories` Eigenschaft. Zum Beispiel:

[![](homekit-images/accessory04.png "Ein Beispiel-Zubehör")](homekit-images/accessory04.png#lightbox)

Hier bilden, kann der Benutzer wählen Sie einen bestimmten Zubehör und Arbeiten mit den Diensten, die er bereitstellt.

## <a name="working-with-services"></a>Arbeiten mit Diensten

Wenn der Benutzer ein bestimmtes HomeKit aktiviert heimautomatisierung Gerät interagiert, ist es in der Regel über die Dienste, die er bereitstellt. Die `Services` Eigenschaft der `HMAccessory` Klasse enthält eine Auflistung von `HMService` Objekte, die die Dienste zu definieren, ein Gerät bietet.

Dienste sind Beleuchtung, Thermostaten stammen, Türöffnern für Garagen, Switches oder sperren. Einige Geräte (z. B. eine Garage Tür dateiöffnungsfunktion) bietet mehr als ein Dienst, z. B. ein Licht und die Möglichkeit zum Öffnen oder schließen eine Tür.

Zusätzlich zu dem bestimmte Dienste, die einen bestimmten Zubehör bereitstellt, jedes Zubehör enthält ein `Information Service` , definiert die Eigenschaften wie Name, Hersteller, Modell und Seriennummer.

### <a name="accessory-service-types"></a>Zubehör Diensttypen

Die folgenden Arten von stehen über die `HMServiceType` Enumeration:

- **AccessoryInformation** -bietet Informationen über das Gerät bestimmte heimautomatisierung (Zubehör).
- **AirQualitySensor** -definiert einen Air-Quality-Sensor.
- **Akku** -definiert den Zustand des Zubehör Akku.
- **CarbonDioxideSensor** -definiert eine Carbon Kohlendioxid Sensor.
- **CarbonMonoxideSensor** -einen Sensor Kohlenmonoxid definiert.
- **ContactSensor** -definiert einen Kontakt Sensortyp (z. B. ein Fenster geöffnet oder geschlossen wird).
- **Die Tür** -definiert einen Tür-Status-Sensor (z. B. geöffnet oder geschlossen).
- **Lüfter** -remote gesteuerten Fan definiert.
- **GarageDoorOpener** -eine Garage Tür dateiöffnungsfunktion definiert.
- **HumiditySensor** -Feuchtigkeitssensor definiert.
- **LeakSensor** -definiert einen Speicherverlust Sensor (z.B. für eine Ebene "heiß" Water Heater oder an Computer).
- **Glühbirne** -definiert eine eigenständige oder ein Licht, die Teil einer anderen Zubehör (z. B. eine Garage Tür dateiöffnungsfunktion) ist.
- **LightSensor** -einen lichtsensor definiert.
- **LockManagement** -definiert einen Dienst, der eine automatisierte Türschloss verwaltet.
- **LockMechanism** -definiert eine remote gesteuerte Sperre (z. B. ein Türschloss).
- **MotionSensor** -einen Bewegungssensor definiert.
- **OccupancySensor** -definiert einen Belegung Sensor.
- **Outlet** -definiert eine remote gesteuerten Steckdose.
- **SecuritySystem** -ein home Sicherheitssystem definiert.
- **StatefulProgrammableSwitch** -definiert einen programmierbaren Switch, der in einem einmal ausgelöst (z. B. eine Flip-Switch) bieten-Zustand bleibt.
- **StatelessProgrammableSwitch** -definiert einen programmierbaren Switch, der auf seinen ursprünglichen Zustand zurückgegeben, nachdem (z. B. eine Schaltfläche) ausgelöst wird.
- **SmokeSensor** -definiert einen Rauch-Sensor.
- **Wechseln Sie** -definiert eine Ein-/Ausschalter beispielsweise ein standard Wall-Schalter.
- **Temperatursensor** -definiert ein Temperatursensors überwacht.
- **Thermostat** -definiert ein intelligentes Thermostat verwendet, um ein HVAC-System zu steuern.
- **Fenster** -definiert eine automatisierte Fenster, dass Zuckerstangen Remote geöffnet oder geschlossen werden.
- **WindowCovering** -definiert ein Remote gesteuerten Fenster verdeckt werden, z. B. blenden, der geöffnet oder geschlossen werden können.

### <a name="displaying-service-information"></a>Anzeigen von Informationen

Nach dem Laden einer `HMAccessory` können Sie die einzelnen Abfragen `HNService` Objekte, es bietet, und diese Informationen für den Benutzer angezeigt:

[![](homekit-images/accessory05.png "Anzeigen von Informationen")](homekit-images/accessory05.png#lightbox)

Sie sollten immer überprüfen sollten die `Reachable` Eigenschaft eine `HMAccessory` vor dem Versuch, damit zu arbeiten. Zubehör kann sein, nicht erreichbar. der Benutzer nicht im Bereich des Geräts ist oder wenn es nicht angeschlossen wurde.

Nachdem ein Dienst ausgewählt wurde, kann ein oder mehrere Merkmale des Diensts zum Überwachen oder Steuern eines Geräts für die angegebene heimautomatisierung die Benutzer anzeigen oder ändern.

<a name="Working-with-Characteristics" />

## <a name="working-with-characteristics"></a>Arbeiten mit Eigenschaften

Jede `HMService` Objekt kann eine Auflistung von enthalten `HMCharacteristic` Objekte, die Aufschluss über den Status des Diensts (z. B. eine Tür geöffnet oder geschlossen wird) oder ermöglicht dem Benutzer, die einen Zustand (z. B. das Festlegen der Farbe eines Lichts) anpassen können.

`HMCharacteristic` bietet nicht nur Informationen über ein Merkmal und den Zustand, bietet jedoch auch Methoden zum Arbeiten mit dem Status über _Merkmal Metadaten_ (`HMCharacteristisMetadata`). Diese Metadaten kann Eigenschaften (z. B. Wertebereiche der minimalen und maximalen) bereitstellen, die hilfreich sind, bei der Anzeige von Informationen für den Benutzer oder können, um den Status zu ändern.

Die `HMCharacteristicType` Enum bietet eine Reihe von Metadatenwerten Merkmal, das definiert, oder wie folgt geändert werden können:

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
 - currentPosition
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
 - Name
 - ObstructionDetected
 - OccupancyDetected
 - OutletInUse
 - OutputState
 - PositionState
 - PowerState
 - RotationDirection
 - RotationSpeed
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
 - TargetTemperature
 - TargetVerticalTilt
 - TemperatureUnits
 - Version

### <a name="working-with-a-characteristics-value"></a>Arbeiten mit dem ein Merkmal des Wert

Um sicherzustellen, dass Ihre app verfügt über den aktuellen Status eines bestimmten Merkmals, rufen Sie die `ReadValue` -Methode der der `HMCharacteristic` Klasse. Wenn die `err` Eigenschaft ist nicht `null`, ein Fehler ist aufgetreten und möglicherweise oder kann nicht für den Benutzer angezeigt werden.

Des Merkmals des `Value` Eigenschaft enthält den aktuellen Status der die angegebene Eigenschaft als eine `NSObject`, und als solche kann nicht umgangen werden mit direkt im C#.

Um den Wert zu lesen, die folgende Hilfsklasse hinzugefügt wurde die **HomeKitIntro** beispielanwendung:

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

Die `NSObjectConverter` wird verwendet, wenn die Anwendung den aktuellen Status eines Merkmals lesen muss. Beispiel:

```csharp
var value = NSObjectConverter.ToFloat (characteristic.Value);
```

Die obige Zeile konvertiert den Wert in eine `float` , klicken Sie dann in Xamarin verwendet werden kann C# Code.

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

Wenn die `err` Eigenschaft ist nicht `null`, ein Fehler ist aufgetreten, und dem Benutzer angezeigt werden soll.

### <a name="testing-characteristic-value-changes"></a>Testen von Änderungen der charakteristischen Wert

Bei der Arbeit mit `HMCharacteristics` und simulierte Zubehör und Änderungen an der `Value` Eigenschaft innerhalb der HomeKit-Zubehör Simulator überwacht werden kann.

Mit der **HomeKitIntro** auf echte iOS-Gerätehardware, die Änderungen ein Merkmal des Werts ausgeführte app im Simulator HomeKit-Zubehör nahezu sofort angezeigt werden sollte. Z. B. Ändern des Status eines Lichts in der iOS-app:

[![](homekit-images/test01.png "Ändern des Status von einem Licht in einer iOS-app")](homekit-images/test01.png#lightbox)

Der Status des Lichts im Simulator HomeKit-Zubehör sollte geändert werden. Wenn der Wert nicht geändert wird, überprüfen Sie den Status der Fehlermeldung beim Schreiben von neuen charakteristische Werte ein, und stellen Sie sicher, dass die Zugriffsmethode weiterhin erreichbar ist.

## <a name="advanced-homekit-features"></a>Erweiterte HomeKit-Funktionen

In diesem Artikel wurden die grundlegenden Features für die Arbeit mit HomeKit-Zubehör in einer Xamarin.iOS-app für behandelt. Es gibt jedoch einige erweiterte Features von HomeKit, die in dieser Einführung nicht behandelt werden:

- **Räume** -aktiviert HomeKit-Zubehör können optional in Räume vom Endbenutzer organisiert. Dadurch können HomeKit vorhanden Zubehör auf eine Weise, die einfach für den Benutzer zu verstehen und Arbeiten mit. Weitere Informationen zu erstellen und Verwalten von Räumen, finden Sie unter Apple [HMRoom](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMRoom_Class/index.html#//apple_ref/occ/cl/HMRoom) Dokumentation.
- **Zonen** -Räume können optional in Zonen organisiert werden, durch den Endbenutzer. Eine Zone bezieht sich auf eine Auflistung von Räumen, die der Benutzer als einzelne Einheit behandeln kann. Zum Beispiel: sollten, unteren Stock oder Keller. In diesem Fall können HomeKit und Arbeiten mit Zubehör in einer Weise, die für den Endbenutzer Sinn macht. Weitere Informationen zum Erstellen und Verwalten von Zonen finden Sie unter Apple [HMZone](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMZone_Class/index.html#//apple_ref/occ/cl/HMZone) Dokumentation.
- **Aktionen und Aktionsmengen** -Aktionen Zubehör Diensteigenschaften ändern und in Gruppen zusammengefasst werden können. Aktion Legt fungieren als Skripts zum Steuern einer Gruppenstatus von Zubehör und ihre Aktionen zu koordinieren. Z. B. ein "Fernsehen" Skript möglicherweise schließen die blenden dim Lichter und dem Fernsehen und das sound-System aktivieren. Weitere Informationen zum Erstellen und Verwalten von Aktionen und Aktion Legt finden Sie unter Apple [HMAction](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMAction_Class/index.html#//apple_ref/occ/cl/HMAction) und [HMActionSet](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMActionSet_Class/index.html#//apple_ref/occ/cl/HMActionSet) Dokumentation.
- **Trigger** – kann keinen Trigger aktivieren, eine oder mehrere Aktion festlegen, wenn eine bestimmte Gruppe von Bedingungen erfüllt. Beispielsweise wird das Licht Portch aktivieren Sie, und Sperren Sie alle externe Türen zu, wenn es außerhalb dunkel erhält. Weitere Informationen zum Erstellen und Verwalten von Triggern finden Sie unter Apple [HMTrigger](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMTrigger_Class/index.html#//apple_ref/occ/cl/HMTrigger) Dokumentation.

Da diese Funktionen die gleichen Techniken, die oben aufgeführten verwenden, werden sollten einfach zu implementieren, indem Sie folgenden Apple [HomeKitDeveloper Handbuch](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html), [HomeKit Richtlinien zur Benutzeroberfläche](https://developer.apple.com/homekit/ui-guidelines/) und [ Referenz zu HomeKit-Framework](https://developer.apple.com/library/ios/home_kit_framework_ref).

## <a name="homekit-app-review-guidelines"></a>Richtlinien für die Überprüfung der HomeKit-App

Vor der Übermittlung einer HomeKit aktiviert Xamarin.iOS-app in iTunes Connect für die Veröffentlichung im iTunes App Store, stellen Sie sicher, müssen Sie Apple Richtlinien für apps mit HomeKit aktiviert:

 - Hauptzweck der app _müssen_ heimautomatisierung werden, wenn das HomeKit-Framework verwenden.
 - Der app-marketing-Text muss Benutzer benachrichtigen, dass HomeKit verwendet wird, und sie müssen eine Datenschutzrichtlinie bereitstellen.
 - Sammeln von Benutzerinformationen oder HomeKit für Werbung ist streng untersagt.

Überprüfen Sie Richtlinien für das vollständige, informieren Sie sich von Apple [App Store überprüfen Richtlinien](https://developer.apple.com/app-store/review/guidelines/).

## <a name="whats-new-in-ios-9"></a>Neuigkeiten in iOS 9

Apple hat die folgenden Änderungen und Ergänzungen an HomeKit für iOS 9 vorgenommen:

- **Verwalten von vorhandenen Objekten** : bei vorhandenen Zubehör geändert wird, wird der Start-Manager (`HMHomeManager`) informiert Sie über das bestimmte Element, das geändert wurde.
- **Dauerhafte Bezeichner** -alle relevante HomeKit-Klassen enthalten nun einen `UniqueIdentifier` aktiviert Eigenschaft, um ein bestimmtes Element in HomeKit eindeutig zu identifizieren, apps (oder Instanzen der gleichen app).
- **Benutzerverwaltung** -einen integrierten View-Controller, um benutzerverwaltung Benutzer bereitzustellen, die Zugriff auf die HomeKit-Geräte in der primären Wohnung des Benutzers hinzugefügt.
- **Benutzeroptionen** – HomeKit-Benutzer haben jetzt eine Gruppe von Berechtigungen, die steuern, welche Funktionen sie in HomeKit verwenden können und HomeKit aktiviert Zubehör. Ihre app sollte relevante Funktionen nur für den aktuellen Benutzer angezeigt werden. Beispielsweise sollte nur einen Administratoren um andere Benutzer verwalten können.
- **Vordefinierte Szenen** -vordefinierte Szenen für vier allgemeine Ereignisse, die auftreten, für den durchschnittlichen HomeKit-Benutzer erstellt wurden: einzurichten, lassen, zurückgeben, wechseln Sie zu konzentrieren. Diese vordefinierten Szenen können nicht aus einer Startseite nicht gelöscht werden.
- **Im Hintergrund und Siri** -Siri hat umfassendere Unterstützung für die Erkennung von Szenen in iOS 9 und kann des Namens jeder Szene in HomeKit definiert. Ein Benutzer kann eine Szene ausführen, indem Sie einfach den Namen in Siri sprechen.
- **Zubehör Kategorien** -eine Reihe von vordefinierten Kategorien für alle Zubehör und hilft bei der Identifizierung des Typs des Zubehör ein Zuhause hinzugefügt wird hinzugefügt oder aus Ihrer App bearbeitet wurde. Diese neuen Kategorien sind während des Setups von Zubehör verfügbar.
- **Unterstützung für Apple Watch-** – HomeKit steht jetzt für WatchOS und der Apple Watch-werden HomeKit-fähige Geräte ohne ein iPhone wird in der Nähe der Apple Watch steuern können. HomeKit für WatchOS unterstützt die folgenden Funktionen: Anzeigen von Häusern, Steuern von Zubehör und Szenen ausführen.
- **Neuen Trigger-Ereignistyp** : Zusätzlich zu der Typ-Trigger mit Timer unterstützt IOS 8, iOS 9 nun unterstützt Ereignistriggern auf Zubehör Zustand (z. B. Daten von triebwerksensoren) oder Geolocation basieren. Verwenden von Ereignistriggern `NSPredicates` Bedingungen für ihre Ausführung festzulegen.
- **Remotezugriff** -mit Remotezugriff, die der Benutzer kann nun steuern ihrer HomeKit Startseite Automation Zubehör aktiviert, wenn sie nicht im Haus an einem Remotestandort sind. IOS 8 war dies nur unterstützt, wenn der Benutzer eine 3. Generation Apple TV zu Hause hat. In iOS 9 diese Einschränkung wird aufgehoben, und remote-Zugriff wird über die iCloud und das HomeKit-Zubehör-Protokoll (HAP) unterstützt.
- **Neue Bluetooth Low Energy (BLE) Fähigkeiten** -HomeKit unterstützt jetzt mehrere Zubehör Typen, die per Bluetooth Low Energy (BLE)-Protokoll kommunizieren können. HAP Secure Tunneling verwenden, kann ein HomeKit-Zubehör verfügbar machen eine andere Bluetooth-Zubehör über Wi-Fi (sofern es sich außerhalb des gültigen Bereichs von Bluetooth ist). In iOS 9 haben BLE-Zubehör vollständige Unterstützung für Benachrichtigungen und Metadaten.
- **Neue Kategorien für Zubehör** -Apple iOS 9 die folgenden neuen Zubehör-Kategorien hinzugefügt: Fenster aufgeführten, Motor Türen und Windows, Alarmsysteme, Sensoren und programmierbare Switches.

Weitere Informationen zu den neuen Features der HomeKit in iOS 9, finden Sie unter Apple [HomeKit Index](https://developer.apple.com/homekit/) und [Neuigkeiten in HomeKit](https://developer.apple.com/videos/wwdc/2015/?id=210) video.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde von Apple HomeKit heimautomatisierung Framework eingeführt. Es wurde gezeigt, wie zum Einrichten und Konfigurieren von Testgeräten, die mit dem Simulator HomeKit-Zubehör und erstellen Sie eine einfache Xamarin.iOS-app zum entdecken, kommunizieren mit und heimautomatisierungsgeräte mit HomeKit steuern.



## <a name="related-links"></a>Verwandte Links

- [iOS 9-Beispiele](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [Was ist neu in iOS 9.0 verfügen](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [HomeKitDeveloper-Handbuch](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html)
- [HomeKit Richtlinien zur Benutzeroberfläche](https://developer.apple.com/homekit/ui-guidelines/)
- [Referenz zu HomeKit-Framework](https://developer.apple.com/library/ios/home_kit_framework_ref)
