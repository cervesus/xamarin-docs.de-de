---
title: iOS-Erweiterungen
description: "In iOS 8 eingeführt, die Erweiterungen sind Widgets, mit denen von iOS in standard Kontexten wie z. B. in der Mitteilungszentrale dargestellt werden, wenn der Benutzer eine benutzerdefinierte verwenden anfordert oder Foto werden bearbeiten. Alle Erweiterungen werden in Verbindung mit einer Container-app installiert und aus einer bestimmten Erweiterung Stelle in einer app Host aktiviert werden."
ms.topic: article
ms.prod: xamarin
ms.assetid: 3DEB3D43-3E4A-4099-8331-93C1E7A77095
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: f6e80b21c76089c0f3f7ac655584b7e18400307e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="ios-extensions"></a>iOS-Erweiterungen

_In iOS 8 eingeführt, die Erweiterungen sind Widgets, mit denen von iOS in standard Kontexten wie z. B. in der Mitteilungszentrale dargestellt werden, wenn der Benutzer eine benutzerdefinierte verwenden anfordert oder Foto werden bearbeiten. Alle Erweiterungen werden in Verbindung mit einer Container-app installiert und aus einer bestimmten Erweiterung Stelle in einer app Host aktiviert werden._

Erweiterungen, wie in iOS 8 eingeführt wurden speziell `UIViewControllers` , Clientanzahl von iOS in standard-Kontexten z. B. innerhalb der **Mitteilungszentrale**, wie spezielle benutzerdefinierte Tastaturtypen, die vom Benutzer angefordert wird, ausführen Eingabe oder anderen Kontexten wie z. B. bearbeiten ein Foto, in dem die Erweiterung Spezialeffekt Filter eingeben kann.

Alle Erweiterungen in Verbindung mit einer Container-app (mit beide Elemente, die mit den 64-Bit-Unified-APIs geschrieben) installiert sind und aus einer bestimmten Erweiterung Stelle in einer app Host aktiviert werden. Und da sie als Ergänzung zu vorhandenen Systemfunktionen verwendet werden sollen, müssen sie hohe Leistung, robust und rationell sein. 

In diesem Artikel werden die folgenden Themen behandelt:

- [Erweiterungspunkte](#Extension-Points) – Listet den Typ der Erweiterungspunkte verfügbar und den Typ der Erweiterung, die für die einzelnen Punkt erstellt werden können.
- [Einschränkungen](#Limitations) -Erweiterungen haben eine Reihe von Einschränkungen, von denen einige sind für alle Typen, während andere Arten von Erweiterung bestimmte Einschränkungen auf ihrer Verwendung haben können.
- [Verteilen, installieren und Ausführen von Erweiterungen](#Distributing-Installing-and-Running-Extensions) -Erweiterungen von innerhalb einer Container-app, die wiederum übermittelt und über den App Store vertrieben verteilt werden. Die Erweiterung mit der app verteilt sind zu diesem Zeitpunkt installiert, aber der Benutzer muss jede Erweiterung explizit aktivieren. Die verschiedenen Typen von Erweiterungen werden auf unterschiedliche Weise aktiviert.
- [Erweiterung Lebenszyklus](#Extension-Lifecycle) – eine Erweiterung des `UIViewController` instanziiert werden, und starten Sie den normalen View Controller-Lebenszyklus. Jedoch im Gegensatz zu einer normalen app werden Erweiterungen geladen, ausgeführt und beendet dann wiederholt.
- [Erstellen eine Erweiterung](#Creating-an-Extension) – Wenn Sie eine Erweiterung zu entwickeln, werden Ihre Lösungen enthalten mindestens zwei Projekte: die Container-app und ein Projekt für jede Erweiterung der Container enthält.
- [Exemplarische Vorgehensweise](#Walkthrough) – Erstellen einer einfachen deckt **heute** Widget-Erweiterung, seine Benutzeroberfläche bereitstellt, mit einem Storyboard, oder im Code wird die Erweiterung installieren und Testen Sie es im iOS-Simulator.
- [Bei der Kommunikation mit der App Host](#Communicating-with-the-Host-App) -kurz erläutert, bei der Kommunikation mit dem Host-app aus einer Erweiterung.
- [Bei der Kommunikation mit der übergeordneten App](#Communicating-with-the-Parent-App) -kurz erläutert, bei der Kommunikation mit der übergeordneten app aus einer Erweiterung.
- [Überlegungen zu und Vorsichtsmaßnahmen](#Precautions-and-Considerations) -Liste einige wissen Sicherheitsvorkehrungen und Überlegungen, die beim Entwerfen und implementieren eine Erweiterung berücksichtigt werden sollten.
 

<a name="Extension-Points" />

## <a name="extension-points"></a>Erweiterungspunkte

Es gibt mehrere Typen von Erweiterung, die im iOS 8 (und höher) erstellt werden können:

<table>
<colgroup>
<col />
<col />
<col />
</colgroup>

<thead>
<tr>
    <th >Typ</th>
    <th >Beschreibung</th>
    <th >Erweiterungspunkt</th>
    <th >Host-App</th>
</tr>
</thead>

<tbody>
<tr>
    <td >Aktion</td>
    <td >Spezialisierten Editor oder für einen bestimmten Medientyp-viewer</td>
    <td ><code>com.apple.ui-services</code></td>
    <td >Beliebig</td>
</tr>
<tr>
    <td >Dokument-Anbieter</td>
    <td >Ermöglicht der app auf einem remote Dokumentenspeicher</td>
    <td ><code>com.apple.fileprovider-ui</code></td>
    <td >Apps, die mit einem <a href="https://developer.xamarin.com/api/type/UIKit.UIDocumentPickerViewController/">UIDocumentPickerViewController</a></td>
</tr>
<tr>
    <td >Tastatur</td>
    <td >Alternative Tastaturen</td>
    <td ><code>com.apple.keyboard-service</code></td>
    <td >Beliebig</td>
</tr>
<tr>
    <td >Foto bearbeiten</td>
    <td >Foto zu bearbeiten oder zu bearbeiten</td>
    <td ><code>com.apple.photo-editing</code></td>
    <td >Photos.App-editor</td>
</tr>
<tr>
    <td >Freigeben</td>
    <td >Teilt Daten mit sozialen Netzwerken, Dienste usw. messaging.</td>
    <td ><code>com.apple.share-services</code></td>
    <td >Beliebig</td>
</tr>
<tr>
    <td >Heute</td>
    <td >"Widgets" angezeigt, die auf dem Bildschirm "heute" oder die Mitteilungszentrale</td>
    <td ><code>com.apple.widget-extensions</code></td>
    <td >Heute "und" Mitteilungszentrale</td>
</tr>
</tbody>
</table>

[Zusätzliche Erweiterungspunkte](~/ios/platform/introduction-to-ios10/index.md#app-extensions) in iOS 10 hinzugefügt wurden.

<a name="Limitations" />

## <a name="limitations"></a>Einschränkungen

Erweiterungen wurden einige Einschränkungen, von denen einige sind für alle Typen (für die Instanz, kein Typ der Erweiterung der Kameras Mikrofone zugreifen kann), während andere Arten von Erweiterung bestimmte Einschränkungen auf ihre Verwendung (z. B. benutzerdefinierte Tastaturen auswirken kann nicht für sichere Daten Eintrag Felder wie Kennwörter verwendet werden). 

Die universellen Einschränkungen sind:

- Die [Health Kit](~/ios/platform/healthkit.md) und [Ereignis Kit UI](~/ios/platform/eventkit.md) Frameworks sind nicht verfügbar
- Erweiterungen können keine [erweiterte hintergrundmodi](http://developer.xamarin.com/guides/cross-platform/application_fundamentals/backgrounding/part_3_ios_backgrounding_techniques/registering_applications_to_run_in_background/)
- Können nicht das Gerät Kameras oder ein Zugriff auf Erweiterungen (obwohl sie vorhandene Mediendateien zugreifen können)
- Erweiterungen können nicht per Funk Drop empfangen von Daten (obwohl Daten über die Drop per Funk übertragen werden können)
- [UIActionSheet](https://developer.xamarin.com/api/type/UIKit.UIActionSheet/) und [UIAlertView](https://developer.xamarin.com/api/type/UIKit.UIAlertView/) stehen nicht zur Verfügung; Verwendung von Erweiterungen müssen [UIAlertController](https://developer.xamarin.com/api/type/UIKit.UIAlertController/)
- Mehrere Mitglieder [UIApplication](https://developer.xamarin.com/api/type/UIKit.UIApplication/) sind nicht verfügbar: [UIApplication.SharedApplication](https://developer.xamarin.com/api/property/UIKit.UIApplication.SharedApplication/), `UIApplication.OpenURL`, `UIApplication.BeginIgnoringInteractionEvents` und `UIApplication.EndIgnoringInteractionEvents`
- iOS erzwingt eine Grenze für 16 MB die speicherauslastung auf heutigen Erweiterungen.
- Standardmäßig haben Tastatur Erweiterungen keinen Zugriff auf das Netzwerk. Dies wirkt sich auf Debuggen auf diesem Gerät (die Einschränkung wird nicht erzwungen, im Simulator), da Xamarin.iOS Netzwerkzugriff erfordert, für das Debuggen funktioniert. Es ist möglich, Anforderung Netzwerkzugriff durch Festlegen der `Requests Open Access` Wert in das Projekt "Info.plist" zu `Yes`. Finden Sie in der Apple- [benutzerdefinierte Tastatur Handbuch](https://developer.apple.com/library/content/documentation/General/Conceptual/ExtensibilityPG/CustomKeyboard.html) für Weitere Informationen zu Einschränkungen der Tastatur-Erweiterung.

Informationen zu einzelnen Einschränkungen finden Sie unter der Apple- [Programmierhandbuch für App-Erweiterung](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/).

<a name="Distributing-Installing-and-Running-Extensions" />

## <a name="distributing-installing-and-running-extensions"></a>Verteilen, installieren und Ausführen von Erweiterungen

Erweiterungen werden von innerhalb einer Container-app, die wiederum übermittelt und über den App Store vertrieben verteilt. Die Erweiterung mit der app verteilt sind zu diesem Zeitpunkt installiert, aber der Benutzer muss jede Erweiterung explizit aktivieren. Die verschiedenen Typen von Erweiterungen werden auf unterschiedliche Weise aktiviert. mehrere erfordern, dass den Benutzer zum Navigieren der **Einstellungen** app und von dort aus zu aktivieren. Während andere Benutzer zu verwenden, z. B. das Aktivieren einer Freigabe Erweiterung beim Senden eines Fotos aktiviert sind. 

Die app in der die Erweiterung verwendet wird (wobei der Benutzer der Erweiterungspunkt vorfindet) wird als bezeichnet den **Host app**, da es sich um die app, die die Erweiterung hostet ist, wenn er ausgeführt wird. Die App, die die Erweiterung installiert die **Container app**, da sie die app ist, die die Erweiterung enthalten, wenn er installiert wurde.  

In der Regel die Container-app beschreibt die Erweiterung und führt den Benutzer durch den Prozess aktivieren.

<a name="Extension-Lifecycle" />

## <a name="extension-lifecycle"></a>Extension-Lebenszyklus

Eine Erweiterung kann so einfach wie ein einzelnes [UIViewController](https://developer.xamarin.com/api/type/UIKit.UIViewController/) oder komplexeren Erweiterungen, die mehreren Bildschirmen der Benutzeroberfläche darstellen. Wenn der Benutzer trifft eine _Erweiterungspunkte_ (z. B. wenn Freigabe ein Bild), haben Sie Gelegenheit zur Auswahl der für diese Erweiterungspunkt registrierten Erweiterungen. 

Wenn sie eine Ihrer App auswählen, die Erweiterungen, die `UIViewController` instanziiert werden, und starten Sie den normalen View Controller-Lebenszyklus. Jedoch im Gegensatz zu einer normalen-app ist angehalten, aber nicht in der Regel beendet, wenn der Benutzer mit ihnen interagieren, werden Erweiterungen geladen, ausgeführt und beendet dann wiederholt.

Erweiterungen können kommunizieren mit ihren apps Host über eine [NSExtensionContext](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/) Objekt. Einige Erweiterungen verfügen über Vorgänge, die mit den Ergebnissen asynchrone Rückrufe zu empfangen. Diese Rückrufe werden in Hintergrundthreads ausgeführt werden und die Erweiterung muss dies berücksichtigt; z. B. mithilfe [NSObject.InvokeOnMainThread](https://developer.xamarin.com/api/member/Foundation.NSObject.InvokeOnMainThread/) , wenn sie die Benutzeroberfläche aktualisiert werden soll. Finden Sie unter der [Kommunikation mit der App Host](#Communicating-with-the-Host-App) weiter unten für weitere Details.

Standardmäßig können Erweiterungen und die Container-apps nicht kommunizieren trotz zusammen installiert wird. In einigen Fällen ist die Container-app im Wesentlichen ein leerer "shipping"-Container, dessen Zweck bedient wird, sobald die Erweiterung installiert ist. Allerdings kann ist aufgrund von Umständen, der Container-app und die Erweiterung Ressourcen über einen gemeinsamen Bereich freigeben. Darüber hinaus eine **heute Erweiterung** verlangen, dass eine Container-app, um eine URL zu öffnen. Dieses Verhalten wird angezeigt, der [weiterentwickelt Countdown Widget](http://github.com/xamarin/monotouch-samples/tree/master/ExtensionsDemo).

<a name="Creating-an-Extension" />

## <a name="creating-an-extension"></a>Erstellen eine Erweiterung

Erweiterungen (und deren Container-apps) muss auf die 64-Bit-Binärdateien und dem Erstellen mithilfe der Xamarin.iOS [APIs Unified](http://developer.xamarin.com/guides/cross-platform/macios/unified). Wenn Sie eine Erweiterung zu entwickeln, werden Ihre Lösungen enthalten mindestens zwei Projekte: der Container-app und ein Projekt für jede Erweiterung der Container enthält. 

<a name="Container-App-Project-Requirements" />

### <a name="container-app-project-requirements"></a>App-Projekt Containeranforderungen

Die Container-app verwendet, um die Erweiterung zu installieren, gelten die folgenden Anforderungen:

- Sie müssen einen Verweis auf das Erweiterungsprojekt verwalten.   
- Es muss eine vollständige-app (müssen können gestartet und erfolgreich ausgeführt werden) werden auch, wenn sie nichts mehr bieten eine Möglichkeit, eine Erweiterung zu installieren. 
- Muss eine Paket-ID, die die Grundlage für die Paket-ID der Erweiterung ist Projekt (siehe unten im Abschnitt Weitere Details).

<a name="Extension-Project-Requirements" />

### <a name="extension-project-requirements"></a>Erweiterung Projektanforderungen

Darüber hinaus hat die Erweiterung Projekt die folgenden Anforderungen:

- Er muss eine Paket-ID aufweisen, die mit den einer Container-app-Paket-ID beginnt. Beispielsweise verfügt der Container-app eine Paket-ID des `com.myCompany.ContainerApp`, Bezeichner für die Erweiterung möglicherweise `com.myCompany.ContainerApp.MyExtension`: 

    ![](extensions-images/bundleidentifiers.png) 
- Muss er den Schlüssel definieren `NSExtensionPointIdentifier`, einen entsprechenden Wert (z. B. `com.apple.widget-extension` für eine **heute** Mitteilungszentrale Widget) in seine `Info.plist` Datei.
- Müssen Sie auch definieren *entweder* der `NSExtensionMainStoryboard` Schlüssel oder die `NSPrincipalClass` -Schlüssel in seiner `Info.plist` Datei durch einen geeigneten Wert:
    - Verwenden der `NSExtensionMainStoryboard` -Taste, um den Namen des Storyboards angeben, in dem die Hauptbenutzeroberfläche für die Erweiterung dargestellt (minus `.storyboard`). Beispielsweise `Main` für die `Main.storyboard` Datei.
    - Verwenden der `NSPrincipalClass` -Taste, um die Klasse anzugeben, die initialisiert wird, wenn die Erweiterung gestartet wird. Der Wert entsprechen muss die **registrieren** Wert Ihrer `UIViewController`: 

    ![](extensions-images/registerandprincipalclass.png)

Bestimmte Arten von Erweiterungen möglicherweise zusätzliche Anforderungen. Z. B. eine **heute** oder **Mitteilungszentrale** principal Erweiterungsklasse implementieren muss [INCWidgetProviding](https://developer.xamarin.com/api/type/NotificationCenter.INCWidgetProviding/).

> [!IMPORTANT]
> **Hinweis:** , wenn Sie das Projekt mit einer der Vorlagen Erweiterungen von Visual Studio für Mac beginnen, die meisten (wenn nicht alle) diese Anforderungen werden bereitgestellt und für Sie automatisch von der Vorlage erfüllt.

<a name="Walkthrough" />

## <a name="walkthrough"></a>Exemplarische Vorgehensweise 

In der folgenden exemplarischen Vorgehensweise erstellen Sie ein Beispiel für **heute** Widget, die den Tag und die Anzahl der Tage im Jahr berechnet:

[ ![](extensions-images/carpediemscreenshot-sm.png "Ein Beispiel für heute Widget, die den Tag und die Anzahl der Tage im Jahr berechnet")](extensions-images/carpediemscreenshot.png)

<a name="Creating-the-Solution" />

### <a name="creating-the-solution"></a>Erstellen der Projektmappe

Führen Sie folgende Schritte aus, um die erforderlichen Projektmappe zu erstellen:

1. Erstellen Sie zunächst eine neue iOS **einzelne Ansicht App** Projekt, und klicken Sie auf die **Weiter** Schaltfläche: 

    [ ![](extensions-images/today01.png "Zunächst erstellen Sie eine neue iOS "," einzelne Ansicht-App-Projekt, und klicken Sie auf die Schaltfläche "Weiter"")](extensions-images/today01.png)
2. Nennen Sie das Projekt `TodayContainer` , und klicken Sie auf die **Weiter** Schaltfläche: 

    [ ![](extensions-images/today02.png "Nennen Sie das Projekt TodayContainer, und klicken Sie auf die Schaltfläche "Weiter"")](extensions-images/today02.png)
3. Überprüfen Sie die **Projektname** und **SolutionName** , und klicken Sie auf die **erstellen** Schaltfläche, um die Projektmappe zu erstellen: 

    [ ![](extensions-images/today03.png "Überprüfen Sie den Projektnamen und SolutionName, und klicken Sie auf die Schaltfläche "erstellen", um die Projektmappe zu erstellen.")](extensions-images/today03.png)
4. Im nächsten Schritt in der **Projektmappen-Explorer**mit der rechten Maustaste auf die Projektmappe, und fügen Sie einen neuen **iOS Erweiterung** Projekt aus der **heute Erweiterung** Vorlage: 

    [ ![](extensions-images/today04.png "Als Nächstes im Projektmappen-Explorer mit der rechten Maustaste auf die Projektmappe und die ein neues Projekt des iOS-Erweiterung aus der Vorlage heute Erweiterung hinzufügen")](extensions-images/today04.png)
5. Nennen Sie das Projekt `DaysRemaining` , und klicken Sie auf die **Weiter** Schaltfläche: 

    [ ![](extensions-images/today05.png "Nennen Sie das Projekt DaysRemaining, und klicken Sie auf die Schaltfläche "Weiter"")](extensions-images/today05.png)
6. Überprüfen Sie das Projekt, und klicken Sie auf die **erstellen** Schaltfläche, um ihn zu erstellen: 

    [ ![](extensions-images/today06.png "Überprüfen Sie das Projekt, und klicken Sie auf die Schaltfläche "erstellen", um ihn zu erstellen")](extensions-images/today06.png)

Diese Lösung verfügen jetzt über zwei Projekten, wie hier gezeigt:

[ ![](extensions-images/today07.png "Sich so ergebende Lösung müsste nun zwei Projekte, wie hier gezeigt.")](extensions-images/today07.png)

<a name="Creating-the-Extension-User-Interface" />

### <a name="creating-the-extension-user-interface"></a>Erstellen der Benutzeroberfläche für die Erweiterung

Als Nächstes müssen Sie die Schnittstelle zum Entwerfen Ihrer **heute** Widget. Dies kann entweder erfolgen mithilfe eines Storyboards oder durch das Erstellen der Benutzeroberflächenautomatisierungs im Code. Beide Methoden werden nachfolgend detailliert behandelt.

<a name="Using-Storyboards" />

#### <a name="using-storyboards"></a>Verwenden die Storyboards

Führen Sie folgende Schritte aus, um die Benutzeroberfläche mit einem Storyboard zu erstellen:

1. In der **Projektmappen-Explorer**, doppelklicken Sie auf des Erweiterungsprojekts `Main.storyboard` Datei zur Bearbeitung zu öffnen: 

    [ ![](extensions-images/today08.png "Doppelklicken Sie auf die Erweiterung Projekte Main.storyboard-Datei aus, um ihn zur Bearbeitung zu öffnen.")](extensions-images/today08.png)
2. Wählen Sie die Bezeichnung, die die Benutzeroberfläche von Vorlage automatisch hinzugefügt wurde, und weisen Sie ihm die **Namen** `TodayMessage` in der **Widget** auf der Registerkarte die **Eigenschaften-Explorer**: 

    [ ![](extensions-images/today09.png "Wählen Sie die Bezeichnung, die die Benutzeroberfläche von Vorlage automatisch hinzugefügt wurde, und geben Sie ihm den Namen TodayMessage in der Registerkarte "Widget" im Eigenschaften-Explorer")](extensions-images/today09.png)
3. Speichern Sie die Änderungen auf das Storyboard.

<a name="Using-Code" />

#### <a name="using-code"></a>Mithilfe von Code

Führen Sie folgende Schritte aus, um die Benutzeroberfläche im Code zu erstellen: 

1. In der **Projektmappen-Explorer**, wählen die **DaysRemaining** Projekt, fügen Sie eine neue Klasse aus, und rufen sie `CodeBasedViewController`: 

    [ ![](extensions-images/code01.png "Aelect DaysRemaining-Projekt, fügen Sie eine neue Klasse hinzu, und nennen Sie es CodeBasedViewController")](extensions-images/code01.png)
2. Auch die **Projektmappen-Explorer**, doppelklicken Sie auf der Erweiterung `Info.plist` Datei zur Bearbeitung zu öffnen: 

    [ ![](extensions-images/code02.png "Doppelklicken Sie auf die Datei Extensions "Info.plist", um sie zur Bearbeitung öffnen")](extensions-images/code02.png)
3. Wählen Sie die **Datenquellensicht** (vom unteren Rand des Bildschirms), und öffnen Sie die `NSExtension` Knoten: 

    [ ![](extensions-images/code03.png "Wählen Sie die Datenquellensicht aus dem unteren Rand des Bildschirms, und öffnen Sie den Knoten NSExtension")](extensions-images/code03.png)
4. Entfernen Sie die `NSExtensionMainStoryboard` Schlüssel und fügen eine `NSPrincipalClass` mit dem Wert `CodeBasedViewController`: 

    [ ![](extensions-images/code04.png "Entfernen Sie den Schlüssel NSExtensionMainStoryboard und fügen Sie eine NSPrincipalClass mit dem Wert CodeBasedViewController hinzu")](extensions-images/code04.png)
5. Speichern Sie die Änderungen.

Als Nächstes Bearbeiten der `CodeBasedViewController.cs` Datei, und stellen sie wie folgt aussehen:

```csharp
using System;
using Foundation;
using UIKit;
using NotificationCenter;
using CoreGraphics;

namespace DaysRemaining
{
    [Register("CodeBasedViewContoller")]
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

Beachten Sie, dass die `[Register("CodeBasedViewContoller")]` mit dem Wert übereinstimmt, die Sie angegeben haben, für die `NSPrincipalClass` oben.

<a name="Coding-the-Extension" />

### <a name="coding-the-extension"></a>Codieren die Erweiterung

Mit der Benutzeroberfläche erstellt wird, öffnen Sie entweder die `TodayViewController.cs` oder `CodeBasedViewController.cs` dateibasierten (der zum Erstellen der Benutzeroberfläche, die oben genannten Methode), Ändern der **ViewDidLoad** Methode, und stellen sie wie folgt aussehen:

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

Wenn Sie mithilfe des Codes Schnittstellenmethode Benutzer basiert, ersetzen die `// Insert code to power extension here...` Kommentar mit dem neuen Code oben. Nach dem die basisimplementierung aufrufen (und eine Bezeichnung für die Version codebasiert einfügen), mit diesem Code wird eine einfache Berechnung zum Abrufen des Tages des Jahres und wie viele Tage übrig sind. Und dann die Nachricht in der Bezeichnung angezeigt (`TodayMessage`), die Sie in das Design der Benutzeroberfläche erstellt.

Beachten Sie, wie dieser Vorgang an den normalen Prozess des Schreibens einer app ähnelt. Einer Erweiterungs `UIViewController` hat den gleichen Lebenszyklus als eine View-Controller in einer app, außer Erweiterungen keine hintergrundmodi und werden nicht angehalten werden, wenn der Benutzer abgeschlossen ist deren Verwendung. Erweiterungen sind stattdessen wiederholt initialisiert und nach Bedarf die Zuordnung aufgehoben wurde.

<a name="Creating-the-Container-App-User-Interface" />

### <a name="creating-the-container-app-user-interface"></a>Erstellen die Container-App-Benutzeroberfläche

In dieser exemplarischen Vorgehensweise werden die Container-app wird einfach als eine Methode verwendet, um liefern, und installieren Sie die Erweiterung und stellt keine Funktionalität eigene bereit. Bearbeiten Sie die TodayContainer `Main.storyboard` Datei und fügen Sie Text definieren die Erweiterung-Funktion und wie Sie sie installieren:

[ ![](extensions-images/today10.png "Bearbeiten Sie die Datei TodayContainers Main.storyboard und fügen Sie Text definieren die Extensions-Funktion und wie Sie sie installieren")](extensions-images/today10.png)

Speichern Sie die Änderungen auf das Storyboard.

<a name="Testing-the-Extension" />

### <a name="testing-the-extension"></a>Testen der Erweiterung

Um die Erweiterung im iOS-Simulator zu testen, führen die **TodayContainer** app. Die Hauptansicht des Containers wird angezeigt:

[ ![](extensions-images/run01.png "Die Hauptansicht Container werden angezeigt")](extensions-images/run01.png)

Als Nächstes erreicht die **Home** Schaltfläche im Simulator, Wischen Sie vom oberen Rand des Bildschirms öffnen die **Mitteilungszentrale**, wählen die **heute** Registerkarte, und klicken Sie auf die **Bearbeiten** Schaltfläche:

[ ![](extensions-images/run02.png "Drücken Sie die Startseite im Simulator, Wischen Sie vom oberen Rand des Bildschirms die Mitteilungszentrale öffnen, wählen Sie die Registerkarte "heute", und klicken Sie auf die Schaltfläche "Bearbeiten"")](extensions-images/run02.png)

Hinzufügen der **DaysRemaining** -Erweiterung der **heute** anzuzeigen, und klicken Sie auf die **Fertig** Schaltfläche:

[ ![](extensions-images/run03.png "Die Ansicht "heute" DaysRemaining-Erweiterung hinzu, und klicken Sie auf die Schaltfläche "Fertig"")](extensions-images/run03.png)

Das neue Widget werden hinzugefügt werden, um die **heute** anzeigen und die Ergebnisse angezeigt werden:

[ ![](extensions-images/run04.png "Das neue Widget werden hinzugefügt werden, um die Ansicht "heute" und die Ergebnisse werden angezeigt")](extensions-images/run04.png)

<a name="Communicating-with-the-Host-App" />

## <a name="communicating-with-the-host-app"></a>Bei der Kommunikation mit dem Host-App

Im Beispiel heute Erweiterung, die Sie soeben erstellt haben, kommuniziert nicht mit der Host-app (die **heute** Bildschirm). Wenn dem so wäre, würden Sie verwenden die [ExtensionContext](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/) Eigenschaft von der `TodayViewController` oder `CodeBasedViewController` Klassen. 

Für Erweiterungen, die Daten aus ihren apps Host empfängt, werden die Daten in Form eines Arrays von [NSExtensionItem](https://developer.xamarin.com/api/type/Foundation.NSExtensionItem/) in gespeicherten Objekte die [InputItems](https://developer.xamarin.com/api/property/Foundation.NSExtensionContext.InputItems/) Eigenschaft von der [ExtensionContext ](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/) die Erweiterung `UIViewController`.

Andere Erweiterung ein, z. B. Erweiterungen Foto bearbeiten kann zwischen dem Benutzer abschließen oder Abbrechen Verbrauch unterscheiden. Dies wird signalisiert werden, zurück an den Host-app über die [CompleteRequest](https://developer.xamarin.com/api/member/Foundation.NSExtensionContext.CompleteRequest/) und [CancelRequest](https://developer.xamarin.com/api/member/Foundation.NSExtensionContext.CancelRequest/) Methoden der [ExtensionContext](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/) Eigenschaft.

Weitere Informationen finden Sie in der Apple- [Programmierhandbuch für App-Erweiterung](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214-CH20-SW1).

<a name="Communicating-with-the-Parent-App" />

## <a name="communicating-with-the-parent-app"></a>Bei der Kommunikation mit der übergeordneten-App

Durch eine App-Gruppe können unterschiedliche Anwendungen (oder eine Anwendung und ihre Erweiterungen) auf einen freigegebenen Dateispeicherort zugreifen. App-Gruppen können für folgende Daten verwendet werden:

- [Apple Watch Einstellungen](~/ios/watchos/app-fundamentals/settings.md).
- [Freigegebene NSUserDefaults](~/ios/app-fundamentals/user-defaults.md).
- [Freigegebene Dateien](~/ios/watchos/app-fundamentals/parent-app.md#files).

Weitere Informationen finden Sie unter der [App-Gruppen](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) Teil unserer **arbeiten mit Funktionen** Dokumentation.

<a name="MobileCoreServices" />

## <a name="mobilecoreservices"></a>MobileCoreServices

Bei der Arbeit mit Erweiterungen verwenden Sie eine einheitliche Art Bezeichner (UTI) zur Erstellung und Bearbeitung von Daten, die zwischen der app, andere apps und/oder Diensten ausgetauscht werden.

Die `MobileCoreServices.UTType` statischen Klasse definiert die folgenden Helper-Eigenschaften, die auf der Apple-beziehen `kUTType...` Definitionen:

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

Sehen Sie im folgenden Beispiel:

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

Weitere Informationen finden Sie unter der [App-Gruppen](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) Teil unserer **arbeiten mit Funktionen** Dokumentation.


<a name="Precautions-and-Considerations" />

## <a name="precautions-and-considerations"></a>Vorsichtsmaßnahmen und Überlegungen

Erheblich weniger Arbeitsspeicher zur Verfügung stehen, als apps werden die Erweiterungen verfügen. Von ihnen erwartet zum Ausführen der schnell und mit minimalen Erkennen von Eindringlingen für den Benutzer und die app, die, der Sie in gehostet werden. Eine Erweiterung sollte auch bieten jedoch eine unterschiedliche, nützliche-Funktion, die Container-app, der sie angehören oder der verwendeten app mit einem Branding-Benutzeroberfläche, mit denen die Benutzer die Erweiterung Developer identifizieren können.

Angesichts dieser enge Anforderung, sollten Sie nur Erweiterungen bereitstellen, die gründlich getestet und für Leistung und arbeitsspeichernutzung optimiert wurden. 

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

Dieses Dokument verfügt über Erweiterungen, was sich handelt, den Typ der Erweiterungspunkte und die bekannten Einschränkungen, die für eine Erweiterung von iOS behandelt. Er erläutert erstellen, verteilen, installieren und Erweiterungen und den Lebenszyklus der Erweiterung ausgeführt. Eine exemplarische Vorgehensweise zur Erstellung einer einfaches darauf **heute** Widgets zeigt zwei Möglichkeiten, das Widget-Benutzeroberfläche mit Storyboards oder Code zu erstellen. Es wurde gezeigt, wie eine Erweiterung in der iOS-Simulator zu testen. Schließlich erläutert es kurz bei der Kommunikation mit der Host-App und einige Vorsichtsmaßnahmen und Überlegungen, die verwendet werden soll, wenn Sie eine Erweiterung entwickeln. 


## <a name="related-links"></a>Verwandte Links

- [ContainerApp (Beispiel)](https://developer.xamarin.com/samples/monotouch/intro-to-extensions)
- [Erstellen von Erweiterungen in Xamarin.iOS (video)](https://university.xamarin.com/lightninglectures/creating-extensions-in-ios)
