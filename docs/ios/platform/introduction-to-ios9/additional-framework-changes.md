---
title: "Zusätzliche iOS 9-Frameworks Änderungen"
description: "Dieser Artikel umfasst zusätzliche, kleinere Änderungen oder Erweiterungen von vorhandenen Frameworks für iOS 9."
ms.topic: article
ms.prod: xamarin
ms.assetid: CFDE1FC4-9327-402B-95A0-581D4AA0E9D5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 773df1eec7c8694143ad6c31044ce281c1265282
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="additional-ios-9-frameworks-changes"></a>Zusätzliche iOS 9-Frameworks Änderungen

_Dieser Artikel umfasst zusätzliche, kleinere Änderungen oder Erweiterungen von vorhandenen Frameworks für iOS 9._

[![](additional-framework-changes-images/ios9-sml.png "iOS 9 Logo")](additional-framework-changes-images/ios9.png#lightbox)

Zusätzlich zu den wichtigen Änderungen für iOS machte Apple Änderungen und Verbesserungen für mehrere vorhandene Frameworks in iOS 9.

## <a name="av-foundation-framework-additions"></a>AV-Foundation Framework Ergänzungen

In den AV-Foundation-Framework die [AVSpeechSynthesisVoice](https://developer.xamarin.com/api/type/AVFoundation.AVSpeechSynthesisVoice/) Klasse ermöglicht Ihnen die Angabe eine Stimme durch Bezeichner zusätzlich zur Sprache jetzt.

Der folgende Code ruft z. B. eine Liste aller verfügbaren stimmen:

```csharp
var voices = AVSpeechSynthesisVoice.GetSpeechVoices ();
```

Anschließend können Sie eine der stimmen aus der Liste festlegen als die `Voice` -Eigenschaft einer Instanz von der [AVSpeachUtterance](https://developer.xamarin.com/api/type/AVFoundation.AVSpeechUtterance/) Klasse.

Die [AVQueuePlayer](https://developer.xamarin.com/api/type/AVFoundation.AVQueuePlayer/) -Klasse unterstützt nun eine Mischung von Internet-streaming und einen dateibasierten Medien in der Warteschlange. Frühere Versionen konnte nur Warteschlange Medien desselben Typs.

Weitere Informationen finden Sie in der Apple- [AVSpeechSynthesisVoice Verweis](https://developer.apple.com/library/prerelease/ios/documentation/AVFoundation/Reference/AVSpeechSynthesisVoice_Ref/index.html#//apple_ref/occ/cl/AVSpeechSynthesisVoice).

## <a name="avkit-framework-additions"></a>AVKit Framework Ergänzungen

Um mit der neuen Funktion für den Bild-im-Bild (PIP) arbeiten, das AVKit-Framework beinhaltet die neue `AVPictureInPictureController` und [AVPlayerViewController](https://developer.xamarin.com/api/type/AVKit.AVPlayerViewController/) Klassen:

- **AVPictureInPictureController** – diese Klasse ermöglicht es eine iOS 9-app für den Benutzer, der die Wiedergabe eines Videos in einem schwebenden, in der Größe veränderbaren PIP-Fenster auf einem iPad starten reagieren.
- **AVPlayerViewController** -verwaltet eine `AVPlayer` Controller verwendet, um ein Video in ein Gleitkomma, in der Größe veränderbaren PIP-Fenster auf einem iPad anzuzeigen.

Weitere Informationen finden Sie unter unsere [Multitasking für iPad](~/ios/platform/introduction-to-ios9/index.md#multitasking) Dokumentation und Apple [AVPictureInPictureController Verweis](https://developer.apple.com/library/prerelease/ios/documentation/AVKit/Reference/AVPictureInPictureController_Class/index.html#//apple_ref/occ/cl/AVPictureInPictureController) und [AVPlayerViewController Verweis](https://developer.apple.com/library/prerelease/ios/documentation/AVFoundation/Reference/AVPlayerViewController_Class/index.html#//apple_ref/occ/cl/AVPlayerViewController).

## <a name="introducing-cloudkit-web-services"></a>Einführung zum CloudKit-Webdienste

Das Framework CloudKit optimiert der Entwicklung von Anwendungen, Zugriff iCloud. Dies schließt den Abruf von Anwendungsdaten und Asset Rechte als auch die Möglichkeit zum sicheren Speichern von Anwendungsinformationen. Dieses Kit bietet Benutzern eine Ebene Anonymität durch Zulassen des Zugriffs auf Anwendungen mit ihren iCloud IDs ohne die persönlichen Informationen gemeinsam nutzen.

Die neue _CloudKit Webdienste_ Framework stellt eine JavaScript-Bibliothek (CloudKit JS), die in Ihrer Website für den Zugriff auf die gleichen CloudKit Basis-Daten und Inhalt wie Ihre app Xamarin.iOS eingebunden werden können.

> [!IMPORTANT]
> **Hinweis:** vor dem Zugriff, vorhanden, oder Aktualisieren von Inhalt aus einer CloudKit-Datenbank, die mit CloudKit JS, Sie müssen zuvor definiertes Schema für diese Datenbank.




Weitere Informationen finden Sie unter den folgenden Dokumenten:

- [Einführung in die CloudKit](~/ios/data-cloud/intro-to-cloudkit.md) -unsere Einführung in die Verwendung von CloudKit in einem Xamarin.iOS-app.
- [Schnellstart CloudKit](https://developer.apple.com/library/prerelease/ios/documentation/DataManagement/Conceptual/CloudKitQuickStart/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014987) -Einführung in die Apple CloudKit.
- [CloudKit JS Verweis](https://developer.apple.com/library/prerelease/ios/documentation/CloudKitJS/Reference/CloudKitJavaScriptReference/index.html#//apple_ref/doc/uid/TP40015359) -Apple CloudKit JS-Dokumentation.
- [Referenz zu CloudKit Web Services](https://developer.apple.com/library/prerelease/ios/documentation/DataManagement/Conceptual/CloutKitWebServicesReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40015240) -Apple-Referenz, die die HTTP-Schnittstelle, um CloudKit beschreibt.
- [CloudKit Katalog: Eine Einführung in CloudKit (Kakao und JavaScript)](https://developer.apple.com/library/prerelease/ios/samplecode/CloudAtlas/Introduction/Intro.html#//apple_ref/doc/uid/TP40014599) -Apple Beispiel-app, die mithilfe von CloudKit und CloudKit JS.

## <a name="foundation-framework-additions"></a>Foundation-Framework-Erweiterungen

Apple enthalten die folgenden Änderungen an der Foundation-Framework in iOS 9:

### <a name="changes-to-nsbundle"></a>Änderungen an NSBundle

Die folgenden Änderungen wurden an die [NSBundle](https://developer.xamarin.com/api/type/Foundation.NSBundle/) Klasse für iOS 9:

* `GetPreservationPriorityForTag (NSString tag)` -Ruft die aktuelle Priorität für die Beibehaltung für Ressourcen, mit dem angegebenen Tag. Gültige Werte liegen im Bereich von `0.0` zu `1.0`, Ressourcen, mit der niedrigsten Priorität werden zuerst gelöscht.
* `SetPreservationPriorityForTag (double priority, NSSet tags)` -Legt die aktuelle Beibehaltung der Priorität für Ressourcen mit bestimmten Tags. Gültige Werte liegen im Bereich von `0.0` zu `1.0`, Ressourcen, mit der niedrigsten Priorität werden zuerst gelöscht.

Weitere Informationen finden Sie in der Apple- [NSBundle Verweis](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSBundle_Class/index.html#//apple_ref/occ/cl/NSBundle).

### <a name="changes-to-nsprocessinfo"></a>Änderungen an NSProcessInfo

Jeder Prozess auf einem iOS-Gerät ausgeführt wird, hat einen einzigen, _Prozess Informationen Agent_ (PIA). Verwenden der [NSProcessInfo](https://developer.xamarin.com/api/type/Foundation.NSProcessInfo/) Klasse, um Informationen über die aktuellen PIA und Steuerelement-Stromversorgung und Kühlung für einen bestimmten Prozess bereitzustellen.

Um zu steuern, das automatische Beenden eines Prozesses können Sie z. B. den folgenden Code verwenden:

```csharp
// Disable automatic termination
var activity = NSProcessInfo.ProcessInfo.BeginActivity(NSActivityOptions.AutomaticTerminationDisabled, "Define reason for change here...");

// Perform the required task
...

// Return to normal operation
NSProcessInfo.ProcessInfo.EndActivity(activity);
```

Weitere Informationen finden Sie in der Apple- [NSProcessInfo Verweis](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSProcessInfo_Class/index.html#//apple_ref/occ/cl/NSProcessInfo).

### <a name="reacting-to-low-power-mode"></a>Das reagieren auf Energiesparmodus

Verwenden der `LowPowerModeEnabled` Eigenschaft von der [NSProcessInfo](https://developer.xamarin.com/api/type/Foundation.NSProcessInfo/) Klasse, um zu bestimmen, ob die Low-Power-Modus auf dem iOS-Gerät aktiviert wurde, die die app ausgeführt wird. Zum Beispiel:

```csharp
// Is the device in low power mode?
if (NSProcessInfo.ProcessInfo.LowPowerModeEnabled) {
    // Reduce activity to conserve energy...
} else {
    // Return to normal activity...
}
```

## <a name="healthkit-framework-changes"></a>HealthKit Framework Changes

Apple enthalten die folgenden Änderungen an der [HealthKit](https://developer.xamarin.com/api/namespace/HealthKit/) Framework in iOS 9:

- Unterstützung für Massenlöschvorgang und Überwachung von Löschen von Einträgen in der Datenbank HealthKit. Finden Sie in der Apple- [HKDeletedObject](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKDeletedObject_ClassReference/index.html#//apple_ref/occ/cl/HKDeletedObject), [HKAnchoredObjectQuery](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKAnchoredObjectQuery_Class/index.html#//apple_ref/occ/cl/HKAnchoredObjectQuery) und [HKHealthStore-Klassenreferenz](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKHealthStore_Class/index.html#//apple_ref/doc/uid/TP40014708) für Weitere Informationen.
- Neue Überwachung Kategorien und Eigenschaften wurden hinzugefügt, um die `HKQuantityTypeIdentifier` Klasse (z. B. `UVExposure`) und die `HKCategoryTypeIdentifier` Klasse (z. B. `OvulationTestResult`). Finden Sie in der Apple- [HealthKit Konstanten Verweis](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HealthKit_Constants/index.html#//apple_ref/doc/uid/TP40014710) für Weitere Informationen.

Finden Sie in unserer [Einführung in HealthKit](~/ios/platform/healthkit.md) Dokumentation weitere Informationen zum Arbeiten mit HealthKit in Xamarin.iOS.

## <a name="local-authentication-framework-changes"></a>Änderungen des lokalen Framework

Apple enthalten die folgenden Änderungen an der [lokale Authentifizierung](https://developer.xamarin.com/api/namespace/LocalAuthentication/) Framework in iOS 9:

- Mithilfe der `EvaluateAccessControl` und `EvaluatePolicy` Methoden die [LAContext](https://developer.xamarin.com/api/type/LocalAuthentication.LAContext/) -Klasse, können Sie nun die Wiederverwendung von Touch ID, aus der vorherigen erfolgreichen entsperren übereinstimmt versucht.
- Die Fähigkeit, eine Liste der derzeit registrierten Finger abzurufen.
- Unterstützung für die Verfolgung, wenn ein Finger hinzugefügt oder von der Authentifizierung entfernt.
- Die Möglichkeit, mit _Authentifizierungskontext_ in Schlüsselbund Aufrufe und Unterstützung für die Bewertung von Keychain-Access Control aufgeführt.
- Die Fähigkeit, eine benutzereingabeaufforderung aus Code "Abbrechen".

Finden Sie in unserer [Einführung in Touch ID](~/ios/platform/touchid.md) Dokumentation weitere Informationen zum Arbeiten mit Touch ID in Xamarin.iOS.

### <a name="lacontext-changes"></a>LAContext Änderungen

Die folgenden Änderungen wurden an die [LAContext](https://developer.xamarin.com/api/type/LocalAuthentication.LAContext/) Klasse für iOS 9:

- **TouchIdAuthenticationMaximumAllowableReuseDuration** - Returns the maximum amount of time that a touch ID authentication can be reused.
- **EvaluatedPolicyDomainState** - Ruft ab oder legt den Zustand einer ausgewerteten Richtlinie fest.
- **MaxBiometryFailures** -wurde in iOS 9 Abschreibung wurde.
- **TouchIdAuthenticationAllowableReuseDuration** Gets or sets the amount of time that a touch ID authentication can be reused.
- **EvaluateAccessControl** – asynchron eine Authentifizierungsrichtlinie ergibt.
- **Für ungültig zu erklären** -erklärt einen bestimmten Touch ID Authentifizierung.
- **IsCredentialSet** -gibt `true` , wenn die Anmeldeinformationen für derzeit festgelegt sind.
- **SetCredentialType** legt den angegebene Anmeldeinformationstyp.

Finden Sie in der Apple- [LAContext Verweis](https://developer.apple.com/library/prerelease/ios/documentation/LocalAuthentication/Reference/LAContext_Class/index.html#//apple_ref/occ/instm/LAContext/evaluatePolicy:localizedReason:reply:) Weitere Details.

## <a name="mapkit-framework-changes"></a>MapKit Framework ändert.

Apple enthalten die folgenden Änderungen an der [MapKit](https://developer.xamarin.com/api/namespace/MapKit/) Framework in iOS 9:

- MapKit bietet jetzt Unterstützung für die Zuordnung app direkt in die Übertragung Richtungen starten und zum Abfragen von während der Übertragung geschätzte Zeit der Ankunft (ETA) mithilfe der [MKLaunchOptions](https://developer.xamarin.com/api/type/MapKit.MKLaunchOptions/) und [MKDirections](https://developer.xamarin.com/api/type/MapKit.MKLaunchOptions/) Klassen.
- Die Suchergebnisse von MapKit zurückgegeben und die [CLGeocoder](https://developer.xamarin.com/api/type/CoreLocation.CLGeocoder/) Klasse kann auch das Ergebnis Zeitzone angeben.
- Map-Anmerkungen, präsentiert von Ihrer iOS-app verwenden können nun vollständig Anpassen der `DetailCalloutAccessoryView` Eigenschaft von der [MKAnnotationView](https://developer.xamarin.com/api/type/MapKit.MKAnnotationView/) Klasse.

Finden Sie in unserer [iOS Maps](~/ios/user-interface/controls/ios-maps/index.md) und [Exemplarische Vorgehensweise – durchsuchen, Anmerkungen und für Überlagerungen in MapKit](~/ios/user-interface/controls/ios-maps/ios-maps-walkthrough.md) Dokumentation weitere Informationen zum Arbeiten mit Karten und Anmerkungen in Xamarin.iOS und Apple [CLGeocoder Verweis](https://developer.apple.com/library/prerelease/ios/documentation/CoreLocation/Reference/CLGeocoder_class/index.html#//apple_ref/occ/cl/CLGeocoder) für Weitere Informationen.

## <a name="passkit-framework-additions"></a>PassKit Framework Ergänzungen

Apple enthalten die folgenden Änderungen an der [PassKit](https://developer.xamarin.com/api/namespace/PassKit/) Framework in iOS 9:

- Apple Pay unterstützt jetzt Store Debit- und Kreditkarten zusammen mit Discover-Karten. Finden Sie unter der **Zahlung Netzwerke** Abschnitt der Apple [PKPaymentRequest-Klassenreferenz](https://developer.apple.com/library/prerelease/ios/documentation/PassKit/Reference/PKPaymentRequest_Ref/index.html#//apple_ref/doc/uid/TP40014832) für Weitere Informationen.
- Von können direkt in eine app Xamarin.iOS Sie jetzt Zahlung Netzwerke und -Aussteller Apple Pay hinzufügen. Finden Sie in der Apple- [PKAddPaymentPassViewController-Klassenreferenz](https://developer.apple.com/library/prerelease/ios/documentation/PassKit/Reference/PKAddPaymentPassViewController_Class/index.html#//apple_ref/doc/uid/TP40016116) Weitere Details.

Finden Sie in unserer [Einführung in PassKit](~/ios/platform/passkit.md) Dokumentation weitere Informationen zum Arbeiten mit PassKit in Xamarin.iOS.

## <a name="safari-services-framework-additions"></a>Safari Services Framework-Erweiterungen

Apple enthalten die folgenden Änderungen an der [Safari Services](https://developer.xamarin.com/api/namespace/SafariServices/) Framework in iOS 9:

- Jetzt können Sie die neue [SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) zum Anzeigen von Webinhalten in einem Xamarin.iOS-app. Es bietet die Möglichkeit, die app Safari Websitedaten und Cookies freigeben und enthält mehrere der Safari Funktionen (z. B. Reader und ausfüllen). [SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) Funktionen eine **Fertig** Schaltfläche, die Benutzer auf Ihre app zurückgegeben wird, wenn sie nach dem Anzeigen der Web-Inhalte sind.

Da die [SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) Klasse zum Anzeigen einer einzelnen Seite Webinhalte zugeschnitten ist, sollten Sie verwenden, um alle ersetzen [WKWebKit](https://developer.xamarin.com/api/type/WebKit.WKWebView/) oder [UIWebView](https://developer.xamarin.com/api/type/UIKit.UIWebView/)Steuerelemente in Ihre vorhandenen Xamarin.iOS-apps.

### <a name="displaying-a-website"></a>Anzeigen von einer Website

Der folgende Code ist ein Beispiel eines Aufrufs einer [SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) von innerhalb einer anderen Ansicht-Controller:

```csharp
// Create an instance of the Safari Services View Controller
var controller = new SFSafariViewController(new NSUrl("http://www.xamarin.com"));

// Display website
PresentViewController(controller, true, null);
```

## <a name="uikit-framework-changes"></a>UIKit Framework Changes

Apple hat zahlreiche Verbesserungen auf mehrere Elemente enthalten die [UIKit](https://developer.xamarin.com/api/namespace/UIKit/) Framework für iOS 9. In den folgenden Abschnitten werden diese Änderungen ausführlich beschrieben.

### <a name="3d-touch-events"></a>3D Touch-Ereignisse

Neue iOS 9 und dem iPhone 6 s und iPhone 6 s Plus, fügt 3D Touch vertrauliche Gesten Druck auf Ihre iOS-apps. Daher, wenn Ihre app unter iOS 9 (oder höher) ausgeführt wird und das iOS-Gerät kann von unterstützenden 3D Touch, Änderungen in Druck führt dazu, dass die `TouchesMoved` Ereignis ausgelöst wurde. 

Aufgrund dieser Änderung im Verhalten, sollte Ihre iOS-apps für vorbereitet werden die `TouchesMoved` aufzurufenden Ereignisses häufiger, selbst wenn das "X" / Y-Koordinaten, nicht geändert wurden. 

Weitere Informationen finden Sie unter unsere [Einführung in 3D Touch](~/ios/platform/3d-touch.md) Handbuch.

### <a name="document-open-in-place-functionality"></a>Dokument öffnen-in-Place-Funktion

Indem Sie entweder die `FinishedLaunching (application, launchOptions)` oder `WillFinishLaunching (Application, launchOptions)` Methoden die [UIApplicationDelegate](https://developer.xamarin.com/api/type/UIKit.UIApplicationDelegate/) -Klasse, Sie nun ein Dokument zu öffnen und vorhanden (im Gegensatz zu einer Kopie arbeiten) zu ändern.

Um die neuen öffnen-in-Place-Funktionen zu unterstützen, fügen die `LSSupportsOpeningDocumentsInPlace` um Ihrer app Xamarin.iOS **"Info.plist"** Datei mit einem Wert von `YES`.

Finden Sie in der Apple- [UIApplicationDelegate Verweis](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/index.html#//apple_ref/occ/intf/UIApplicationDelegate) Weitere Details.

### <a name="enhanced-touch-events"></a>Verbesserte Berührungsereignisse

Apple hat mehrere Erweiterungen auf Ereignisse berühren in iOS 9 bereitgestellt werden. Dazu gehören die Möglichkeit zur Verwendung von Touch Vorhersage und zum Zugriff auf intermediate Fingereingaben zwischen den Aktualisierungen anzeigen.

Finden Sie in der Apple- [Ereignis behandeln Handbuch für iOS](https://developer.apple.com/library/ios/documentation/EventHandling/Conceptual/EventHandlingiPhoneOS/Introduction/Introduction.html) Weitere Details.

### <a name="fetching-tailored-content"></a>Angepasste Inhalt abrufen

Die neue `NSDataAsset` Klasse ermöglicht eine Xamarin.iOS-app zum Abrufen von Inhalt auf den Arbeitsspeicher und Grafikfunktionen des iOS-Geräts, das er gerade ausgeführt wird, auf zugeschnitten sind.

### <a name="new-layout-anchors"></a>Neue Layout Anker

Die neue `NSLayoutAnchor` und `NSLayoutDimension` Layout Anker Klassen arbeiten mit den neuen Anker Eigenschaften von der [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/) Klasse (z. B. `LeadingAnchor` und `WidthAnchor`) um das Layout in iOS 9 vereinfachen.

Finden Sie in unserer [Einführung in Storyboards Unified](~/ios/user-interface/storyboards/unified-storyboards.md) Weitere Informationen zum Arbeiten mit AutoLayout und Größenklassen in einem Xamarin.iOS-app und Apple Dokumentation [NSLayoutAnchor Verweis](https://developer.apple.com/library/prerelease/ios/documentation/AppKit/Reference/NSLayoutAnchor_ClassReference/index.html#//apple_ref/occ/cl/NSLayoutAnchor), [ NSLayoutDimension Verweis](https://developer.apple.com/library/prerelease/ios/documentation/AppKit/Reference/NSLayoutDimension_ClassReference/index.html#//apple_ref/occ/cl/NSLayoutDimension) und [UIView Verweis](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIView_Class/index.html#//apple_ref/occ/cl/UIView) für Weitere Informationen.

### <a name="new-readable-content-margins"></a>Neue lesbare Ränder des Inhalts

Die neue `UILayoutGuide` Klasse kann lesbare Inhalt Ränder und definieren die Regionen Zeichnen-Befehl für den Inhalt innerhalb einer Ansicht verwendet werden. Finden Sie in der Apple- [UILayoutGuide Verweis](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UILayoutGuide_Class_Reference/index.html#//apple_ref/occ/cl/UILayoutGuide) für Weitere Informationen.

### <a name="text-input-in-notifications-modifications"></a>Texteingabe in Benachrichtigungen Änderungen

Die [UIUserNotificationAction](https://developer.xamarin.com/api/type/UIKit.UIUserNotificationAction/) -Klasse verfügt über ein neues `Behavior` -Eigenschaft, die zur Unterstützung von Texteingabe von Benachrichtigungen verwendet werden kann.

### <a name="uiapplicationdelegate-changes"></a>UIApplicationDelegate Änderungen

Während offiziell nicht von Apple veraltet, sie vorschlagen und Ersetzen Sie dabei alle Aufrufe an die `FinishedLaunching (UIApplication application)` Methode der [UIApplicationDelegate](https://developer.xamarin.com/api/type/UIKit.UIApplicationDelegate/) Klasse entweder mit der `FinishedLaunching (UIApplication application, NSDictionary launchOptions)` oder `WillFinishLaunching (UIApplication application, NSDictionary launchOptions)` Methoden.

Finden Sie in der Apple- [UIApplicationDelegate Verweis](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/index.html#//apple_ref/occ/intf/UIApplicationDelegate) Weitere Details.

### <a name="uikit-dynamics-changes"></a>UIKit Dynamics Änderungen

Apple enthalten die folgenden Änderungen an der Dynamik UIKit in iOS 9:

- Dynamics bietet jetzt Unterstützung für viereckig Kollision Grenzen.
- Die neuen, anpassbaren `UIFieldBehavior` Klasse dient zur Unterstützung verschiedener Feldtypen.
- Zusätzliche Typen wurden hinzugefügt, um die `UIAttachmentBehavior` Klasse.

Finden Sie in der Apple- [UIAttachment Verweis](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIAttachmentBehavior_Class/index.html#//apple_ref/occ/cl/UIAttachmentBehavior) Weitere Details.

### <a name="uipickerview-and-uidatepicker-changes"></a>UIPickerView und UIDatePicker Änderungen

Vor iOS 9 die [UIPickerView](https://developer.xamarin.com/api/type/UIKit.UIPickerView/) und [UIDatePicker](https://developer.xamarin.com/api/type/UIKit.UIDatePicker/) Steuerelemente wurden nicht veränderbare Größen und würde zum Ausfüllen der Breite des Containers (i. d. r. die Breite des iOS-Gerät, die die app wurde automatisch angepasst ausgeführt).

In iOS 9 tritt diese automatische Größenänderung nicht mehr auf, und die Steuerelemente mit einer Breite 320 zeigen auf alle iOS-Geräten unabhängig von der Bildschirmgröße und Ausrichtung gerendert werden.

Um dieses Problem zu beheben, verwenden Sie Automatisches Layout und Klassen Größe heften Sie die Breite des Steuerelements an den Rändern des übergeordneten Containers (Ansicht), und geben Sie die erforderlichen Höhe ein. Finden Sie in unserer [Einführung in Storyboards Unified](~/ios/user-interface/storyboards/unified-storyboards.md) Dokumentation weitere Informationen zum Arbeiten mit Automatisches Layout und Größenklassen in einem Xamarin.iOS-app.

### <a name="new-uitextinputassistantitem-class"></a>Neue UITextInputAssistantItem-Klasse

Verwenden Sie die neue `UITextInputAssistantItem` Klasse Layout Leiste Schaltfläche Gruppen in einer _Shortcut-Leiste_. Die Leiste Verknüpfung ist ein neuer Bereich, der in die Bildschirmtastatur Typisierung Tastenkombinationen bereitstellen verfügbar ist.



## <a name="related-links"></a>Verwandte Links

- [iOS 9-Beispiele](https://developer.xamarin.com/samples/ios/iOS9/)
- [Einführung in iOS 9](~/ios/platform/introduction-to-ios9/index.md)
- [iOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [Was ist neu in iOS 9.0 verfügen](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
