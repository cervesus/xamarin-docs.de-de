---
title: iOS-Erweiterungen in Xamarin.iOS
description: Dieses Dokument beschreibt die Erweiterungen, die Widgets, die von iOS in standard-Kontext wie z. B. in der Mitteilungszentrale angezeigt werden. Es wird erläutert, wie eine Erweiterung zu erstellen und mit ihm kommunizieren, von der übergeordneten-app.
ms.prod: xamarin
ms.assetid: 3DEB3D43-3E4A-4099-8331-93C1E7A77095
ms.technology: xamarin-ios
ms.custom: xamu-video
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: 25b27765a35310c5cdbaf5ae19902b1d19eff6ea
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "58870273"
---
# <a name="ios-extensions-in-xamarinios"></a>iOS-Erweiterungen in Xamarin.iOS

> [!VIDEO https://youtube.com/embed/Sd0-ch9Udmk]

**Erstellen von Erweiterungen in iOS durch [Xamarin University](https://university.xamarin.com/)**

Erweiterungen in iOS 8 eingeführten sind speziell `UIViewControllers` , sind präsentiert von iOS in standard-Kontexten z. B. wie in der **Mitteilungszentrale**, wie spezielle selbstdefinierten Typen, die vom Benutzer angefordert wird, ausführen Eingabe oder eine andere Kontexte wie bearbeiten ein Foto, in dem die Erweiterung Spezialeffekt Filter bereitstellen kann.

Alle Erweiterungen werden in Verbindung mit einer app für Container (mit beiden Elementen, die mit der 64-Bit-Unified-APIs geschrieben wurden) installiert und sind einem bestimmten Zeitpunkt der Erweiterung in einer Host-app aktiviert. Und da sie als Ergänzungen für vorhandene Systemfunktionen verwendet werden, müssen sie hohe Leistung, schlanke und stabile sein. 

## <a name="extension-points"></a>Erweiterungspunkte

|Typ|Beschreibung|Erweiterungspunkt|Host-App|
|--- |--- |--- |--- |
|Aktion|Spezialisierten Editor oder für einen bestimmten Medientyp-viewer|`com.apple.ui-services`|Beliebig|
|Dokument-Anbieter|Ermöglicht der app Store Remotedokumenten verwenden|`com.apple.fileprovider-ui`|Apps mit einer [UIDocumentPickerViewController](xref:UIKit.UIDocumentPickerViewController)|
|Tastatur|Alternative Tastaturen|`com.apple.keyboard-service`|Beliebig|
|Foto bearbeiten|Bildbearbeitung und bearbeiten|`com.apple.photo-editing`|Photos.App-editor|
|Freigeben|Gibt Daten für soziale Netzwerke, messaging usw. frei.|`com.apple.share-services`|Beliebig|
|Heute|"Widgets" angezeigt, auf dem Bildschirm "heute" oder die Mitteilungszentrale|`com.apple.widget-extensions`|Heute und Mitteilungszentrale|

[Zusätzliche Erweiterungspunkte](~/ios/platform/introduction-to-ios10/index.md#app-extensions) in iOS 10 hinzugefügt wurden.

## <a name="limitations"></a>Einschränkungen

Erweiterungen haben eine Reihe von Einschränkungen, von denen einige gelten für alle Typen (für die Instanz, die kein Typ der Erweiterung der Kameras Mikrofon zugreifen kann), während andere Arten von Erweiterung spezifische Einschränkungen auf ihrer Verwendung (z. B. benutzerdefinierte Tastaturen möglicherweise nicht werden für die Eingabefelder für sichere Daten wie z. B. Kennwörter verwendet). 

Die universelle Einschränkungen sind:

- Die [Integrität Kit](~/ios/platform/healthkit.md) und [Ereignis Kit UI](~/ios/platform/eventkit.md) Frameworks sind nicht verfügbar
- Erweiterungen können keine [erweiterte hintergrundmodi](https://developer.xamarin.com/guides/cross-platform/application_fundamentals/backgrounding/part_3_ios_backgrounding_techniques/registering_applications_to_run_in_background/)
- Kameras oder Mikrofon des Geräts Erweiterungen können nicht zugegriffen werden (obwohl sie vorhandene Mediendateien zugreifen können)
- Erweiterungen können nicht Air & Drop-Daten empfangen werden, (obwohl Daten über die Drop per Funk übertragen werden können)
- [UIActionSheet](xref:UIKit.UIActionSheet) und [UIAlertView](xref:UIKit.UIAlertView) sind nicht verfügbar; Erweiterungen müssen verwenden [UIAlertController](xref:UIKit.UIAlertController)
- Mehrere Member der [UIApplication](xref:UIKit.UIApplication) sind nicht verfügbar: [UIApplication.SharedApplication](xref:UIKit.UIApplication.SharedApplication), [UIApplication.OpenUrl](xref:UIKit.UIApplication.OpenUrl(Foundation.NSUrl)), [UIApplication.BeginIgnoringInteractionEvents](xref:UIKit.UIApplication.BeginIgnoringInteractionEvents) and [UIApplication.EndIgnoringInteractionEvents](xref:UIKit.UIApplication.EndIgnoringInteractionEvents)
- iOS erzwingt ein 16 MB Limit für arbeitsspeichernutzung zu heutigen-Erweiterungen.
- Standardmäßig haben Tastatur Erweiterungen keinen Zugriff auf das Netzwerk. Dies wirkt sich auf debugging auf Gerät (die Einschränkung wird im Simulator nicht erzwungen), da für Xamarin.iOS Netzwerkzugriff erforderlich ist, für das Funktionieren des Remotedebuggens. Es ist möglich, den Netzwerkzugriff für die Anforderung, durch Festlegen der `Requests Open Access` Wert in das Projekt die Datei "Info.plist" zu `Yes`. Informieren Sie sich von Apple [benutzerdefinierte Tastatur Handbuch](https://developer.apple.com/library/content/documentation/General/Conceptual/ExtensibilityPG/CustomKeyboard.html) für Weitere Informationen zu Einschränkungen der Tastatur-Erweiterung.

Einzelne Einschränkungen finden Sie in Apple [Programmierhandbuch für App-Erweiterung](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/).

## <a name="distributing-installing-and-running-extensions"></a>Verteilen, installieren und Ausführen von Erweiterungen

Erweiterungen werden über verteilt, in einer Container-app, die wiederum übermittelt und über den App Store verteilt. Die Erweiterungen, die mit der app verteilt werden zu diesem Zeitpunkt installiert, aber der Benutzer muss jede Erweiterung explizit aktivieren. Die verschiedenen Typen von Erweiterungen, die auf unterschiedliche Weise aktiviert sind, mehrere erfordern, dass den Benutzer zum Navigieren der **Einstellungen** app, und aktivieren Sie sie von dort aus. Während die anderen Zeitpunkt der Verwendung, z. B. eine Freigabe-Erweiterung wird aktiviert, beim Senden von Fotos aktiviert werden. 

Die app, in denen die Erweiterung verwendet wird (wobei der Benutzer mit dem Erweiterungspunkt findet) wird als bezeichnet die **Host-app**, da es sich um die app, auf dem die Erweiterung gehostet wird ist, wenn er ausgeführt wird. Die app, die die Erweiterung installiert, die **-Container-app**, da es sich um die app handelt, die die Erweiterung enthalten, wenn er installiert wurde.  

In der Regel wird die Container-app beschreibt die Erweiterung und führt den Benutzer durch den Prozess des Aktivierens es.

## <a name="extension-lifecycle"></a>Erweiterung-Anwendungslebenszyklus

Eine Erweiterung kann so einfach sein wie ein einzelnes [UIViewController](xref:UIKit.UIViewController) oder komplexere Erweiterungen, die mehreren Bildschirmen der Benutzeroberfläche darstellen. Wenn der Benutzer trifft eine _Erweiterungspunkte_ (z. B. wenn ein Bild freigeben), haben Sie Gelegenheit, wählen Sie die Erweiterungen, die für diesen Punkt Erweiterung registriert. 

Wenn sie Ihrer App auswählen, die Erweiterungen, die `UIViewController` instanziiert, und starten Sie den normalen View Controller-Lebenszyklus. Allerdings anders als eine normale app, die angehalten, aber nicht beendet werden, wenn der Benutzer abgeschlossen ist, mit ihnen interagieren, werden Erweiterungen geladen, ausgeführt und wiederholt dann beendet.

Erweiterungen können kommunizieren mit ihren apps Host über eine [NSExtensionContext](xref:Foundation.NSExtensionContext) Objekt. Einige Erweiterungen verfügen über Vorgänge, die asynchrone Rückrufe mit den Ergebnissen zu erhalten. Diese Rückrufe in Hintergrundthreads ausgeführt, und die Erweiterung muss ist dies zu berücksichtigen; z. B. mithilfe von [NSObject.InvokeOnMainThread](xref:Foundation.NSObject.InvokeOnMainThread*) , wenn die Benutzeroberfläche aktualisiert werden sollen. Finden Sie unter den [Kommunikation mit der App-Host](#communicating-with-the-host-app) Informationen weiter unten im Abschnitt.

Standardmäßig können Erweiterungen und die Container-apps nicht kommunizieren trotz zusammen installiert wird. In einigen Fällen ist die Container-app im Wesentlichen eine leere "shipping"-Container, deren Zweck bereitgestellt wird, sobald die Erweiterung installiert ist. Wenn jedoch Situationen geben, kann der Container-app und die Erweiterung Ressourcen über einen gemeinsamen Wissensbereich freigeben. Darüber hinaus eine **heute-Erweiterung** können anfordern, die Container-app, um eine URL zu öffnen. Dieses Verhalten wird angezeigt, der [weiterentwickeln Widget "Countdown"](https://github.com/xamarin/monotouch-samples/tree/master/ExtensionsDemo).

## <a name="creating-an-extension"></a>Erstellen einer Erweiterung

Erweiterungen (und ihre Container-apps) muss 64-Bit-Binärdateien und erstellt mithilfe der Xamarin.iOS [Unified-APIs](https://developer.xamarin.com/guides/cross-platform/macios/unified). Wenn Sie eine Erweiterung entwickeln zu können, enthält Ihre Lösungen über mindestens zwei Projekte: der Container-app und ein Projekt für jede Erweiterung des Containers enthält. 

### <a name="container-app-project-requirements"></a>Anforderungen für Container-app-Projekts

Die Container-app verwendet, um die Erweiterung zu installieren, gelten die folgenden Anforderungen:

- Sie müssen einen Verweis auf das Erweiterungsprojekt verwalten.   
- Es muss eine vollständige app (müssen können gestartet und erfolgreich ausgeführt werden) sein, wenn sie nichts mehr bieten eine Möglichkeit, eine Erweiterung zu installieren. 
- Eine Bündel-ID, die die Grundlage für die Bündel-ID der Erweiterung muss Projekt (siehe weiter unten im Abschnitt Weitere Details).

### <a name="extension-project-requirements"></a>Erweiterung von projektanforderungen

Darüber hinaus hat die Erweiterung des Projekts die folgenden Anforderungen:

- Sie benötigen eine Bündel-ID, die mit der Container-app-Bündel-ID beginnt. Wenn der Container-app eine Bündel-ID der hat z. B. `com.myCompany.ContainerApp`, Bezeichner von der Erweiterung möglicherweise `com.myCompany.ContainerApp.MyExtension`: 

    ![](extensions-images/bundleidentifiers.png) 
- Muss er den Schlüssel definieren `NSExtensionPointIdentifier`, einen entsprechenden Wert (z. B. `com.apple.widget-extension` für eine **heute** Mitteilungszentrale Widget) in die `Info.plist` Datei.
- Muss er auch definieren *entweder* der `NSExtensionMainStoryboard` Schlüssel oder `NSExtensionPrincipalClass` -Schlüssel in der `Info.plist` Datei durch einen passenden Wert:
    - Verwenden der `NSExtensionMainStoryboard` -Taste, um den Namen des Storyboards anzugeben, die die Benutzeroberfläche für die Erweiterung darstellt (abzüglich `.storyboard`). Z. B. `Main` für die `Main.storyboard` Datei.
    - Verwenden der `NSExtensionPrincipalClass` -Taste, um die Klasse anzugeben, die initialisiert wird, wenn die Erweiterung gestartet wird. Der Wert muss übereinstimmen der **registrieren** Wert Ihrer `UIViewController`: 

    ![](extensions-images/registerandprincipalclass.png)

Bestimmte Arten von Erweiterungen möglicherweise zusätzliche Anforderungen. Z. B. eine **heute** oder **Mitteilungszentrale** Erweiterung für eine Klasse implementieren muss [INCWidgetProviding](xref:NotificationCenter.INCWidgetProviding).

> [!IMPORTANT]
> Wenn Sie Ihr Projekt mit einem die Vorlagen Erweiterungen von Visual Studio für Mac starten, werden die meisten (wenn nicht alle) diese Anforderungen bereitgestellt und erfüllt, damit Sie automatisch von der Vorlage.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise 

In der folgenden exemplarischen Vorgehensweise erstellen Sie ein Beispiel für **heute** Widgets, die den Tag und die Anzahl der Tage im Jahr verbleiben berechnet:

[![](extensions-images/carpediemscreenshot-sm.png "Eine Beispiel-heute-Widget, die den Tag und die Anzahl der verbleibenden Tage in das Jahr berechnet")](extensions-images/carpediemscreenshot.png#lightbox)

### <a name="creating-the-solution"></a>Erstellen der Lösung

Um die erforderliche Lösung zu erstellen, führen Sie folgende Schritte aus:

1. Erstellen Sie zunächst ein neues iOS, **Einzelansicht-App** Projekt, und klicken Sie auf die **Weiter** Schaltfläche: 

    [![](extensions-images/today01.png "Zunächst erstellen Sie ein neues iOS, Einzelansicht-App-Projekt, und klicken Sie auf die Schaltfläche \"Weiter\"")](extensions-images/today01.png#lightbox)
2. Nennen Sie das Projekt `TodayContainer` , und klicken Sie auf die **Weiter** Schaltfläche: 

    [![](extensions-images/today02.png "Nennen Sie das Projekt TodayContainer, und klicken Sie auf die Schaltfläche \"Weiter\"")](extensions-images/today02.png#lightbox)
3. Überprüfen Sie die **Projektname** und **SolutionName** , und klicken Sie auf die **erstellen** Schaltfläche, um die Projektmappe zu erstellen: 

    [![](extensions-images/today03.png "Überprüfen Sie den Projektnamen und SolutionName aus, und klicken Sie auf die Schaltfläche \"erstellen\" zum Erstellen der Projektmappe")](extensions-images/today03.png#lightbox)
4. Anschließend in der **Projektmappen-Explorer**mit der rechten Maustaste auf die Projektmappe, und fügen Sie einen neuen **iOS Erweiterung** Projekt aus der **heute-Erweiterung** Vorlage: 

    [![](extensions-images/today04.png "Als Nächstes im Projektmappen-Explorer mit der rechten Maustaste auf die Projektmappe und die fügen Sie ein neues iOS-Erweiterungsprojekt aus der Vorlage für die heute-Erweiterung hinzu.")](extensions-images/today04.png#lightbox)
5. Nennen Sie das Projekt `DaysRemaining` , und klicken Sie auf die **Weiter** Schaltfläche: 

    [![](extensions-images/today05.png "Nennen Sie das Projekt DaysRemaining, und klicken Sie auf die Schaltfläche \"Weiter\"")](extensions-images/today05.png#lightbox)
6. Überprüfen Sie das Projekt, und klicken Sie auf die **erstellen** klicken, um es zu erstellen: 

    [![](extensions-images/today06.png "Überprüfen Sie das Projekt, und klicken Sie auf die Schaltfläche \"erstellen\", um ihn zu erstellen")](extensions-images/today06.png#lightbox)

Die resultierende Projektmappe verfügen nun über zwei Projekten, wie hier gezeigt:

[![](extensions-images/today07.png "Die resultierende Projektmappe verfügen jetzt zwei Projekte, wie hier gezeigt")](extensions-images/today07.png#lightbox)

### <a name="creating-the-extension-user-interface"></a>Erstellen der Benutzeroberfläche für die Erweiterung

Als Nächstes müssen Sie die Schnittstelle für das Entwerfen Ihrer **heute** Widget. Dies kann entweder erfolgen Verwendung eines Storyboards, oder indem Sie die Benutzeroberfläche im Code erstellen. Beide Methoden werden unten im Detail behandelt werden.

#### <a name="using-storyboards"></a>Mithilfe von storyboards

Führen Sie folgende Schritte aus, um die Benutzeroberfläche mit einem Storyboard zu erstellen:

1. In der **Projektmappen-Explorer**, doppelklicken Sie auf des Erweiterungsprojekt `Main.storyboard` Datei, die sie für die Bearbeitung zu öffnen: 

    [![](extensions-images/today08.png "Doppelklicken Sie auf die Erweiterung Projekte Datei \"Main.storyboard\", um sie für die Bearbeitung zu öffnen")](extensions-images/today08.png#lightbox)
2. Wählen Sie die Bezeichnung, die die Benutzeroberfläche von Vorlage automatisch hinzugefügt wurde, und geben Sie ihm die **Namen** `TodayMessage` in die **Widget** Registerkarte die **Eigenschaften-Explorer**: 

    [![](extensions-images/today09.png "Wählen Sie die Bezeichnung, die die Benutzeroberfläche von Vorlage automatisch hinzugefügt wurde, und weisen Sie ihm den Namen TodayMessage in der Registerkarte \"Widget\" im Eigenschaften-Explorer")](extensions-images/today09.png#lightbox)
3. Speichern Sie die Änderungen auf das Storyboard an.

#### <a name="using-code"></a>Mithilfe von code

Führen Sie folgende Schritte aus, um die Benutzeroberfläche in Code zu erstellen: 

1. In der **Projektmappen-Explorer**, wählen die **DaysRemaining** Projekt, fügen Sie eine neue Klasse und nenne `CodeBasedViewController`: 

    [![](extensions-images/code01.png "Aelect das DaysRemaining-Projekt, fügen Sie eine neue Klasse und nenne CodeBasedViewController")](extensions-images/code01.png#lightbox)
2. Auch hier gilt die **Projektmappen-Explorer**, doppelklicken Sie auf der Erweiterung `Info.plist` Datei, die sie für die Bearbeitung zu öffnen: 

    [![](extensions-images/code02.png "Doppelklicken Sie auf Extensions \"Info.plist\"-Datei, um sie für die Bearbeitung zu öffnen.")](extensions-images/code02.png#lightbox)
3. Wählen Sie die **Datenquellensicht** (aus dem unteren Rand des Bildschirms), und öffnen Sie die `NSExtension` Knoten: 

    [![](extensions-images/code03.png "Wählen Sie die Datenquellensicht aus dem unteren Rand des Bildschirms, und öffnen Sie den Knoten NSExtension")](extensions-images/code03.png#lightbox)
4. Entfernen Sie die `NSExtensionMainStoryboard` Schlüssel, und fügen eine `NSExtensionPrincipalClass` mit dem Wert `CodeBasedViewController`: 

    [![](extensions-images/code04.png "Entfernen Sie den Schlüssel NSExtensionMainStoryboard, und fügen Sie eine NSExtensionPrincipalClass mit dem Wert CodeBasedViewController hinzu.")](extensions-images/code04.png#lightbox)
5. Speichern Sie die Änderungen.

Als Nächstes bearbeiten Sie die `CodeBasedViewController.cs` Datei, und stellen sie wie folgt aussehen:

```csharp
using System;
using Foundation;
using UIKit;
using NotificationCenter;
using CoreGraphics;

namespace DaysRemaining
{
    [Register("CodeBasedViewController")]
    public class CodeBasedViewController : UIViewController, INCWidgetProviding
    {
        public CodeBasedViewController ()
        {
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Add label to view
            var TodayMessage = new UILabel (new CGRect (0, 0, View.Frame.Width, View.Frame.Height)) {
                TextAlignment = UITextAlignment.Center
            };

            View.AddSubview (TodayMessage);
            
            // Insert code to power extension here...

        }
    }
}
```

Beachten Sie, dass die `[Register("CodeBasedViewController")]` entspricht dem Wert, den Sie, für angegeben die `NSExtensionPrincipalClass` oben.

### <a name="coding-the-extension"></a>Codieren die Erweiterung

Die Benutzeroberfläche erstellt wurde, öffnen Sie entweder die `TodayViewController.cs` oder `CodeBasedViewController.cs` dateibasierte (der zum Erstellen der Benutzeroberfläche, die oben genannten Methode), Ändern der **ViewDidLoad** Methode und legen Sie ihn wie folgt aussehen:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Calculate the values
    var dayOfYear = DateTime.Now.DayOfYear;
    var leapYearExtra = DateTime.IsLeapYear (DateTime.Now.Year) ? 1 : 0;
    var daysRemaining = 365 + leapYearExtra - dayOfYear;

    // Display the message
    if (daysRemaining == 1) {
        TodayMessage.Text = String.Format ("Today is day {0}. There is one day remaining in the year.", dayOfYear);
    } else {
        TodayMessage.Text = String.Format ("Today is day {0}. There are {1} days remaining in the year.", dayOfYear, daysRemaining);
    }
}
```

Wenn mit dem Code-Benutzeroberfläche-Methode basieren, ersetzt die `// Insert code to power extension here...` Kommentar mit dem neuen Code von oben. Nach dem Aufrufen der basisimplementierung (und eine Bezeichnung für die Version des Codes aufgrund der einfügen), dieser Code führt eine einfache Berechnung rufen Sie den Tag des Jahres und wie viele Tage übrig sind. Anschließend wird die Nachricht in der Bezeichnung (`TodayMessage`), die Sie in das Design der Benutzeroberfläche erstellt haben.

Beachten Sie, wie dieser Prozess auf den normalen Vorgang des Schreibens von einer app ähnelt. Einer Erweiterungs des `UIViewController` hat den gleichen Lebenszyklus als eine View-Controller in einer app, außer Erweiterungen keine hintergrundmodi und werden nicht angehalten werden, wenn der Benutzer abgeschlossen ist deren Verwendung. Erweiterungen sind stattdessen wiederholt initialisiert und nach Bedarf die Zuordnung aufgehoben wurde.

### <a name="creating-the-container-app-user-interface"></a>Erstellen der Container-app-Benutzeroberfläche

In dieser exemplarischen Vorgehensweise wird die Container-app dient einfach als eine Methode zu versenden, und installieren Sie die Erweiterung und bietet keine Funktionalität. Bearbeiten Sie die TodayContainer des `Main.storyboard` Datei, und fügen Sie Text definieren, der Extension-Funktion und zur Installation:

[![](extensions-images/today10.png "Bearbeiten Sie die Datei \"Main.Storyboard TodayContainers\", und fügen Sie Text definieren, die Extensions-Funktion und zur Installation")](extensions-images/today10.png#lightbox)

Speichern Sie die Änderungen auf das Storyboard an.

### <a name="testing-the-extension"></a>Testen der Erweiterung

Führen Sie zum Testen Ihrer Erweiterungs im iOS-Simulator die **TodayContainer** app. Die Hauptansicht des Containers wird angezeigt:

[![](extensions-images/run01.png "Die Hauptansicht der Container werden angezeigt")](extensions-images/run01.png#lightbox)

Drücken Sie dann die **Startseite** Schaltfläche im Simulator eine streifbewegung vom oberen Bildschirmrand zu öffnen der **Mitteilungszentrale**, wählen die **heute** Registerkarte, und klicken Sie auf die **Bearbeiten** Schaltfläche:

[![](extensions-images/run02.png "Auf die Schaltfläche \"Start\" im Simulator eine streifbewegung vom oberen Bildschirmrand, um die Mitteilungszentrale zu öffnen, wählen Sie die Registerkarte \"heute\", und klicken Sie auf die Schaltfläche \"Bearbeiten\"")](extensions-images/run02.png#lightbox)

Hinzufügen der **DaysRemaining** Erweiterung der **heute** anzuzeigen, und klicken Sie auf die **Fertig** Schaltfläche:

[![](extensions-images/run03.png "Die heute-Ansicht der DaysRemaining-Erweiterung hinzu, und klicken Sie auf die Schaltfläche \"Fertig\"")](extensions-images/run03.png#lightbox)

Das neue Widget wird hinzugefügt, die **heute** anzeigen und die Ergebnisse angezeigt:

[![](extensions-images/run04.png "Das neue Widget wird auf die heute-Ansicht hinzugefügt werden, und die Ergebnisse angezeigt")](extensions-images/run04.png#lightbox)

## <a name="communicating-with-the-host-app"></a>Kommunikation mit dem Host-app

Das Beispiel noch heute die Erweiterung, die Sie soeben haben erstellt, kommuniziert nicht mit der Host-app (die **heute** Bildschirm). Wenn dies der Fall ist, verwenden sie die [ExtensionContext](xref:Foundation.NSExtensionContext) Eigenschaft der `TodayViewController` oder `CodeBasedViewController` Klassen. 

Für Erweiterungen, die Daten aus ihren apps Host empfängt, werden die Daten in Form eines Arrays von [NSExtensionItem](xref:Foundation.NSExtensionItem) im gespeicherten Objekte die [InputItems](xref:Foundation.NSExtensionContext.InputItems) Eigenschaft der [ExtensionContext ](xref:Foundation.NSExtensionContext) von der Erweiterungs `UIViewController`.

Andere Erweiterung, z. B. Foto bearbeiten-Erweiterungen kann zwischen dem Benutzer, abgeschlossen oder abgebrochen wird die Nutzung unterscheiden. Dies signalisiert werden zurück an die Host-app über die [CompleteRequest](xref:Foundation.NSExtensionContext.CompleteRequest*) und [CancelRequest](xref:Foundation.NSExtensionContext.CancelRequest*) Methoden [ExtensionContext](xref:Foundation.NSExtensionContext) Eigenschaft.

Weitere Informationen finden Sie unter Apple [Programmierhandbuch für App-Erweiterung](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214-CH20-SW1).

## <a name="communicating-with-the-parent-app"></a>Kommunikation mit die übergeordnete app

Durch eine App-Gruppe können unterschiedliche Anwendungen (oder eine Anwendung und ihre Erweiterungen) auf einen freigegebenen Dateispeicherort zugreifen. App-Gruppen können für folgende Daten verwendet werden:

- [Apple Watch-Einstellungen](~/ios/watchos/app-fundamentals/settings.md).
- [Freigegebene NSUserDefaults](~/ios/app-fundamentals/user-defaults.md).
- [Freigegebene Dateien](~/ios/watchos/app-fundamentals/parent-app.md#files).

Weitere Informationen finden Sie unter den [App-Gruppen](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) Teil unserer **arbeiten mit Funktionen** Dokumentation.

## <a name="mobilecoreservices"></a>MobileCoreServices

Bei der Arbeit mit Erweiterungen verwenden Sie eine einheitliche Art Bezeichner (UTI) zum Erstellen und Bearbeiten von Daten, die zwischen der app, andere apps und/oder Diensten ausgetauscht werden.

Die `MobileCoreServices.UTType` statischen Klasse definiert die folgenden Helper-Eigenschaften, die im Zusammenhang mit der Apple `kUTType...` Definitionen:

- `kUTTypeAlembic` - `Alembic`
- `kUTTypeAliasFile` - `AliasFile`
- `kUTTypeAliasRecord` - `AliasRecord`
- `kUTTypeAppleICNS` - `AppleICNS`
- `kUTTypeAppleProtectedMPEG4Audio` - `AppleProtectedMPEG4Audio`
- `kUTTypeAppleProtectedMPEG4Video` - `AppleProtectedMPEG4Video`
- `kUTTypeAppleScript` - `AppleScript`
- `kUTTypeApplication` - `Application`
- `kUTTypeApplicationBundle` - `ApplicationBundle`
- `kUTTypeApplicationFile` - `ApplicationFile`
- `kUTTypeArchive` - `Archive`
- `kUTTypeAssemblyLanguageSource` - `AssemblyLanguageSource`
- `kUTTypeAudio` - `Audio`
- `kUTTypeAudioInterchangeFileFormat` - `AudioInterchangeFileFormat`
- `kUTTypeAudiovisualContent` - `AudiovisualContent`
- `kUTTypeAVIMovie` - `AVIMovie`
- `kUTTypeBinaryPropertyList` - `BinaryPropertyList`
- `kUTTypeBMP` - `BMP`
- `kUTTypeBookmark` - `Bookmark`
- `kUTTypeBundle` - `Bundle`
- `kUTTypeBzip2Archive` - `Bzip2Archive`
- `kUTTypeCalendarEvent` - `CalendarEvent`
- `kUTTypeCHeader` - `CHeader`
- `kUTTypeCommaSeparatedText` - `CommaSeparatedText`
- `kUTTypeCompositeContent` - `CompositeContent`
- `kUTTypeConformsToKey` - `ConformsToKey`
- `kUTTypeContact` - `Contact`
- `kUTTypeContent` - `Content`
- `kUTTypeCPlusPlusHeader` - `CPlusPlusHeader`
- `kUTTypeCPlusPlusSource` - `CPlusPlusSource`
- `kUTTypeCSource` - `CSource`
- `kUTTypeData` - `Database`
- `kUTTypeDelimitedText` - `DelimitedText`
- `kUTTypeDescriptionKey` - `DescriptionKey`
- `kUTTypeDirectory` - `Directory`
- `kUTTypeDiskImage` - `DiskImage`
- `kUTTypeElectronicPublication` - `ElectronicPublication`
- `kUTTypeEmailMessage` - `EmailMessage`
- `kUTTypeExecutable` - `Executable`
- `kUTExportedTypeDeclarationsKey` - `ExportedTypeDeclarationsKey`
- `kUTTypeFileURL` - `FileURL`
- `kUTTypeFlatRTFD` - `FlatRTFD`
- `kUTTypeFolder` - `Folder`
- `kUTTypeFont` - `Font`
- `kUTTypeFramework` - `Framework`
- `kUTTypeGIF` - `GIF`
- `kUTTypeGNUZipArchive` - `GNUZipArchive` 
- `kUTTypeHTML` - `HTML`
- `kUTTypeICO` - `ICO`
- `kUTTypeIconFileKey` - `IconFileKey`
- `kUTTypeIdentifierKey` - `IdentifierKey`
- `kUTTypeImage` - `Image`
- `kUTImportedTypeDeclarationsKey` - `ImportedTypeDeclarationsKey`
- `kUTTypeInkText` - `InkText`
- `kUTTypeInternetLocation` - `InternetLocation`
- `kUTTypeItem` - `Item`
- `kUTTypeJavaArchive` - `JavaArchive`
- `kUTTypeJavaClass` - `JavaClass`
- `kUTTypeJavaScript` - `JavaScript`
- `kUTTypeJavaSource` - `JavaSource`
- `kUTTypeJPEG` - `JPEG`
- `kUTTypeJPEG2000` - `JPEG2000`
- `kUTTypeJSON` - `JSON`
- `kUTType3dObject` - `k3dObject`
- `kUTTypeLivePhoto` - `LivePhoto`
- `kUTTypeLog` - `Log` 
- `kUTTypeM3UPlaylist` - `M3UPlaylist`
- `kUTTypeMessage` - `Message`
- `kUTTypeMIDIAudio` - `MIDIAudio`
- `kUTTypeMountPoint` - `MountPoint`
- `kUTTypeMovie` - `Movie`
- `kUTTypeMP3` - `MP3`
- `kUTTypeMPEG` - `MPEG`
- `kUTTypeMPEG2TransportStream` - `MPEG2TransportStream`
- `kUTTypeMPEG2Video` - `MPEG2Video`
- `kUTTypeMPEG4` - `MPEG4`
- `kUTTypeMPEG4Audio` - `MPEG4Audio`
- `kUTTypeObjectiveCPlusPlusSource` - `ObjectiveCPlusPlusSource`
- `kUTTypeObjectiveCSource` - `ObjectiveCSource`
- `kUTTypeOSAScript` - `OSAScript`
- `kUTTypeOSAScriptBundle` - `OSAScriptBundle`
- `kUTTypePackage` - `Package`
- `kUTTypePDF` - `PDF`
- `kUTTypePerlScript` - `PerlScript`
- `kUTTypePHPScript` - `PHPScript`
- `kUTTypePICT` - `PICT`
- `kUTTypePKCS12` - `PKCS12`
- `kUTTypePlainText` - `PlainText`
- `kUTTypePlaylist` - `Playlist`
- `kUTTypePluginBundle` - `PluginBundle`
- `kUTTypePNG` - `PNG`
- `kUTTypePolygon` - `Polygon`
- `kUTTypePresentation` - `Presentation`
- `kUTTypePropertyList` - `PropertyList`
- `kUTTypePythonScript` - `PythonScript`
- `kUTTypeQuickLookGenerator` - `QuickLookGenerator`
- `kUTTypeQuickTimeImage` - `QuickTimeImage`
- `kUTTypeQuickTimeMovie` - `QuickTimeMovie` 
- `kUTTypeRawImage` - `RawImage`
- `kUTTypeReferenceURLKey` - `ReferenceURLKey`
- `kUTTypeResolvable` - `Resolvable`
- `kUTTypeRTF` - `RTF`
- `kUTTypeRTFD` - `RTFD`
- `kUTTypeRubyScript` - `RubyScript`
- `kUTTypeScalableVectorGraphics` - `ScalableVectorGraphics`
- `kUTTypeScript` - `Script`
- `kUTTypeShellScript` - `ShellScript`
- `kUTTypeSourceCode` - `SourceCode`
- `kUTTypeSpotlightImporter` - `SpotlightImporter`
- `kUTTypeSpreadsheet` - `Spreadsheet`
- `kUTTypeStereolithography` - `Stereolithography`
- `kUTTypeSwiftSource` - `SwiftSource`
- `kUTTypeSymLink` - `SymLink`
- `kUTTypeSystemPreferencesPane` - `SystemPreferencesPane`
- `kUTTypeTabSeparatedText` - `TabSeparatedText`
- `kUTTagClassFilenameExtension` - `TagClassFilenameExtension`
- `kUTTagClassMIMEType` - `TagClassMIMEType`
- `kUTTypeTagSpecificationKey` - `TagSpecificationKey`
- `kUTTypeText` - `Text`
- `kUTType3DContent` - `ThreeDContent`
- `kUTTypeTIFF` - `TIFF`
- `kUTTypeToDoItem` - `ToDoItem`
- `kUTTypeTXNTextAndMultimediaData` - `TXNTextAndMultimediaData`
- `kUTTypeUniversalSceneDescription` - `UniversalSceneDescription`
- `kUTTypeUnixExecutable` - `UnixExecutable`
- `kUTTypeURL` - `URL` 
- `kUTTypeURLBookmarkData` - `URLBookmarkData`
- `kUTTypeUTF16ExternalPlainText` - `UTF16ExternalPlainText`
- `kUTTypeUTF16PlainText` - `UTF16PlainText`
- `kUTTypeUTF8PlainText` - `UTF8PlainText`
- `kUTTypeUTF8TabSeparatedText` - `UTF8TabSeparatedText`
- `kUTTypeVCard` - `VCard`
- `kUTTypeVersionKey` - `VersionKey` 
- `kUTTypeVideo` - `Video` 
- `kUTTypeVolume` - `Volume` 
- `kUTTypeWaveformAudio` - `WaveformAudio`
- `kUTTypeWebArchive` - `WebArchive`
- `kUTTypeWindowsExecutable` - `WindowsExecutable`
- `kUTTypeX509Certificate` - `X509Certificate`
- `kUTTypeXML` - `XML`
- `kUTTypeXMLPropertyList` - `XMLPropertyList`
- `kUTTypeXPCService` - `XPCService`
- `kUTTypeZipArchive` - `ZipArchive`

Finden Sie im folgende Beispiel aus:

```csharp
using MobileCoreServices;
...

NSItemProvider itemProvider = new NSItemProvider ();
itemProvider.LoadItem(UTType.PropertyList ,null, (item, err) => {
    if (err == null) {
        NSDictionary results = (NSDictionary )item;
        NSString baseURI =
results.ObjectForKey("NSExtensionJavaScriptPreprocessingResultsKey");
    }
});
```

Weitere Informationen finden Sie unter den [App-Gruppen](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) Teil unserer **arbeiten mit Funktionen** Dokumentation.

## <a name="precautions-and-considerations"></a>Sicherheitsvorkehrungen und Überlegungen

Erweiterungen müssen wesentlich weniger Speicher zur Verfügung, als mit apps. Sie sollten führen schnell und mit minimalen Intrusion für den Benutzer und die app, die, der Sie in gehostet werden. Allerdings sollte eine Erweiterung auch eine unterschiedliche, nützliche Funktion an die verarbeitende app mit einer mit Branding-Benutzeroberfläche, mit denen die Benutzer zum Identifizieren der Erweiterung Entwickler oder Container-app ebenfalls bereitstellen.

Wenn diese eine enge Anforderung, sollten Sie nur Erweiterungen bereitstellen, die gründlich getestet und für die Leistung und arbeitsspeichernutzung optimiert wurde. 

## <a name="summary"></a>Zusammenfassung

In diesem Dokument wurden Erweiterungen, was sie sind, den Typ der Erweiterungspunkte und die bekannten Einschränkungen, die von einer Erweiterung von iOS behandelt. Er erläutert das Erstellen, verteilen, installieren und Ausführen von Erweiterungen und die Erweiterung-Anwendungslebenszyklus. Es eine exemplarische Vorgehensweise zum Erstellen einer einfaches bereitgestellt **heute** Widget zeigt zwei Möglichkeiten, das Widget die Benutzeroberfläche mit Storyboards oder Code zu erstellen. Es wurde gezeigt, wie eine Erweiterung in der iOS-Simulator testen. Zum Schluss bereits kurz kommuniziert mit Host-App und ein paar Vorsichtsmaßnahmen und Überlegungen, die ausgeführt werden soll, wenn Sie eine Erweiterung zu entwickeln. 

## <a name="related-links"></a>Verwandte Links

- [ContainerApp (Beispiel)](https://developer.xamarin.com/samples/monotouch/intro-to-extensions)
- [Erstellen von Erweiterungen in Xamarin.iOS (video)](https://university.xamarin.com/lightninglectures/creating-extensions-in-ios)
