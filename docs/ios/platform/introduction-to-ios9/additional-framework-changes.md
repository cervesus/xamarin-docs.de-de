---
title: Weitere iOS 9-Frameworks Änderungen
description: Dieses Dokument beschreibt zusätzliche eingeführte, die unter iOS 9-frameworkänderungen. Es wird erläutert, AVFoundation AVKit und CloudKit.
ms.prod: xamarin
ms.assetid: CFDE1FC4-9327-402B-95A0-581D4AA0E9D5
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.openlocfilehash: da7064997b8a10d4a4604861a405e13dd23a08cf
ms.sourcegitcommit: a1a58afea68912c79d16a3f64de9a0c1feb2aeb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55233912"
---
# <a name="additional-ios-9-frameworks-changes"></a>Weitere iOS 9-Frameworks Änderungen

_Dieser Artikel enthält zusätzliche, kleinere Änderungen oder Verbesserungen an vorhandenen Frameworks für iOS 9._

[![](additional-framework-changes-images/ios9-sml.png "iOS 9 Logo")](additional-framework-changes-images/ios9.png#lightbox)

Zusätzlich zu den wichtigsten Änderungen an IOS-hat Apple Änderungen und Verbesserungen an mehrere vorhandene Frameworks in iOS 9.

## <a name="avfoundation-framework-additions"></a>AVFoundation Framework-Erweiterungen

Im Framework AVFoundation der [AVSpeechSynthesisVoice](https://developer.xamarin.com/api/type/AVFoundation.AVSpeechSynthesisVoice/) Klasse nun können Sie eine Stimme Bezeichner neben der Sprache angeben.

Der folgende Code ruft beispielsweise eine Liste mit allen verfügbaren stimmen:

```csharp
var voices = AVSpeechSynthesisVoice.GetSpeechVoices ();
```

Anschließend können Sie eine der stimmen zurück, die aus der Liste durch Festlegung als die `Voice` Eigenschaft einer Instanz von der [AVSpeachUtterance](https://developer.xamarin.com/api/type/AVFoundation.AVSpeechUtterance/) Klasse.

Die [AVQueuePlayer](https://developer.xamarin.com/api/type/AVFoundation.AVQueuePlayer/) -Klasse unterstützt nun eine Mischung aus Internet-streaming und dateibasierte Medien in der Warteschlange. Frühere Versionen konnte nur Warteschlange Medien desselben Typs.

Weitere Informationen finden Sie unter Apple [AVSpeechSynthesisVoice Verweis](https://developer.apple.com/library/prerelease/ios/documentation/AVFoundation/Reference/AVSpeechSynthesisVoice_Ref/index.html#//apple_ref/occ/cl/AVSpeechSynthesisVoice).

## <a name="avkit-framework-additions"></a>AVKit Framework-Erweiterungen

Um mit dem neuen Feature für den Bild-im-Bild (PIP) zu arbeiten, das AVKit-Framework beinhaltet die neue `AVPictureInPictureController` und [AVPlayerViewController](https://developer.xamarin.com/api/type/AVKit.AVPlayerViewController/) Klassen:

- **AVPictureInPictureController** – diese Klasse ermöglicht eine iOS 9-app, auf den Benutzer beim Starten der Wiedergabe eines Videos in eine Gleitkommazahl, in der Größe veränderbaren PIP-Fenster auf einem iPad zu reagieren.
- **AVPlayerViewController** -verwaltet eine `AVPlayer` Controller verwendet, um ein Video in eine Gleitkommazahl, in der Größe veränderbaren PIP-Fenster auf einem iPad zu präsentieren.

Weitere Informationen finden Sie unserem [Multitasking für iPad](~/ios/platform/introduction-to-ios9/index.md#multitasking) Dokumentation und Apple [AVPictureInPictureController Verweis](https://developer.apple.com/library/prerelease/ios/documentation/AVKit/Reference/AVPictureInPictureController_Class/index.html#//apple_ref/occ/cl/AVPictureInPictureController) und [AVPlayerViewController Verweis](https://developer.apple.com/library/prerelease/ios/documentation/AVFoundation/Reference/AVPlayerViewController_Class/index.html#//apple_ref/occ/cl/AVPlayerViewController).

## <a name="introducing-cloudkit-web-services"></a>Einführung in CloudKit-Webdienste

Das CloudKit-Framework wird der Entwicklung von Anwendungen, iCloud Zugriff optimiert. Dies schließt den Abruf von Anwendungsdaten und Asset Rechte als auch Möglichkeit zum sicheren Speichern von Anwendungsinformationen. Dieses Kit, erhalten Benutzer eine Ebene der Anonymität durch Gewähren des Zugriffs auf Anwendungen mit ihren iCloud IDs ohne Weitergabe persönlicher Informationen.

Die neue _CloudKit Webdienste_ Framework bietet eine JavaScript-Bibliothek (CloudKit JS), die auf Ihrer Website Zugriff auf dieselben CloudKit-basierten Daten und denselben Inhalt wie für Ihre Xamarin.iOS-app integriert werden kann.

> [!IMPORTANT]
> Vor dem Zugriff auf, vorhanden, oder Aktualisieren von Inhalten aus einer CloudKit-Datenbank mithilfe von CloudKit JS, müssen Sie zuvor, Datenbankschema definiert haben.




Weitere Informationen finden Sie in den folgenden Dokumenten:

- [Einführung in CloudKit](~/ios/data-cloud/intro-to-cloudkit.md) – Einführung in CloudKit in einer Xamarin.iOS-app verwenden.
- [Schnellstart CloudKit](https://developer.apple.com/library/prerelease/ios/documentation/DataManagement/Conceptual/CloudKitQuickStart/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014987) – Einführung in CloudKit von Apple.
- [CloudKit JS Verweis](https://developer.apple.com/library/prerelease/ios/documentation/CloudKitJS/Reference/CloudKitJavaScriptReference/index.html#//apple_ref/doc/uid/TP40015359) -Apple CloudKit JS-Dokumentation.
- [Referenz zu Web Services CloudKit](https://developer.apple.com/library/prerelease/ios/documentation/DataManagement/Conceptual/CloutKitWebServicesReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40015240) -Apple-Referenz, die die HTTP-Schnittstelle cloudkit beschreibt.
- [CloudKit-Katalog: Eine Einführung in CloudKit (Cocoa und JavaScript)](https://developer.apple.com/library/prerelease/ios/samplecode/CloudAtlas/Introduction/Intro.html#//apple_ref/doc/uid/TP40014599) -Apple Beispiel-app mithilfe von CloudKit und CloudKit JS.

> [!IMPORTANT]
> Apple [stellt Tools zur Verfügung](https://developer.apple.com/support/allowing-users-to-manage-data/), die Entwickler dabei unterstützen, die Datenschutz-Grundverordnung (DSGVO) der Europäischen Union umzusetzen.

## <a name="foundation-framework-additions"></a>Foundation-Framework-Erweiterungen

Die folgenden Änderungen an der Foundation-Framework wird von Apple in iOS 9 enthalten:

### <a name="changes-to-nsbundle"></a>Änderungen an NSBundle

Die folgenden Änderungen wurden an die [NSBundle](xref:Foundation.NSBundle) -Klasse für iOS 9:

* `GetPreservationPriorityForTag (NSString tag)` -Ruft die aktuelle Priorität für die Beibehaltung für Ressourcen, mit dem angegebenen Tag. Gültige Werte liegen im Bereich `0.0` zu `1.0`, Ressourcen, mit der niedrigsten Priorität werden zuerst gelöscht werden.
* `SetPreservationPriorityForTag (double priority, NSSet tags)` -Legt die aktuelle Priorität der Beibehaltung für Ressourcen mit den angegebenen Tags. Gültige Werte liegen im Bereich `0.0` zu `1.0`, Ressourcen, mit der niedrigsten Priorität werden zuerst gelöscht werden.

Weitere Informationen finden Sie unter Apple [NSBundle Verweis](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSBundle_Class/index.html#//apple_ref/occ/cl/NSBundle).

### <a name="changes-to-nsprocessinfo"></a>Änderungen an NSProcessInfo

Jeder Prozess auf einem iOS-Gerät ausgeführt wird, hat einen einzigen, _Prozess Informationen Agent_ (PIA). Verwenden der [NSProcessInfo](xref:Foundation.NSProcessInfo) Klasse, um Informationen zu den aktuellen PIA und Steuerelement Power und zur Verwaltung für einen bestimmten Prozess bereitzustellen.

Um zu steuern, die automatische Beendigung eines Prozesses können Sie z. B. den folgenden Code verwenden:

```csharp
// Disable automatic termination
var activity = NSProcessInfo.ProcessInfo.BeginActivity(NSActivityOptions.AutomaticTerminationDisabled, "Define reason for change here...");

// Perform the required task
...

// Return to normal operation
NSProcessInfo.ProcessInfo.EndActivity(activity);
```

Weitere Informationen finden Sie unter Apple [NSProcessInfo Verweis](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSProcessInfo_Class/index.html#//apple_ref/occ/cl/NSProcessInfo).

### <a name="reacting-to-low-power-mode"></a>Reagieren auf niedriger Energiestatus

Verwenden der `LowPowerModeEnabled` Eigenschaft der [NSProcessInfo](xref:Foundation.NSProcessInfo) Klasse, um zu bestimmen, ob die mit niedriger Energiestatus auf iOS-Geräten aktiviert wurde, die die app ausgeführt wird. Zum Beispiel:

```csharp
// Is the device in low power mode?
if (NSProcessInfo.ProcessInfo.LowPowerModeEnabled) {
    // Reduce activity to conserve energy...
} else {
    // Return to normal activity...
}
```

## <a name="healthkit-framework-changes"></a>HealthKit-Frameworkänderungen

Apple enthalten die folgenden Änderungen an der [HealthKit](https://developer.xamarin.com/api/namespace/HealthKit/) -Framework in iOS 9:

- Unterstützung für massenlöschung und nachverfolgung von Löschen von Einträgen in der Datenbank HealthKit. Finden Sie unter Apple [HKDeletedObject](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKDeletedObject_ClassReference/index.html#//apple_ref/occ/cl/HKDeletedObject), [HKAnchoredObjectQuery](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKAnchoredObjectQuery_Class/index.html#//apple_ref/occ/cl/HKAnchoredObjectQuery) und [HKHealthStore Class Reference](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKHealthStore_Class/index.html#//apple_ref/doc/uid/TP40014708) für Weitere Informationen.
- Neue Nachverfolgungskategorien und Eigenschaften hinzugefügt wurden die `HKQuantityTypeIdentifier` Klasse (z. B. `UVExposure`) und die `HKCategoryTypeIdentifier` Klasse (z. B. `OvulationTestResult`). Finden Sie unter Apple [HealthKit Konstanten Verweis](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HealthKit_Constants/index.html#//apple_ref/doc/uid/TP40014710) für Weitere Informationen.

Informieren Sie sich unsere [Einführung in HealthKit](~/ios/platform/healthkit.md) Dokumentation für Weitere Informationen zum Arbeiten mit HealthKit in Xamarin.iOS.

## <a name="local-authentication-framework-changes"></a>Lokale Authentifizierung-Frameworkänderungen

Apple enthalten die folgenden Änderungen an der [lokale Authentifizierung](https://developer.xamarin.com/api/namespace/LocalAuthentication/) -Framework in iOS 9:

- Mithilfe der `EvaluateAccessControl` und `EvaluatePolicy` Methoden der [LAContext](https://developer.xamarin.com/api/type/LocalAuthentication.LAContext/) -Klasse, Sie haben jetzt die Wiederverwendung von Touch ID, aus der vorherigen erfolgreichen entsperren übereinstimmt versucht.
- Die Fähigkeit, eine Liste der derzeit registrierten Finger zu erhalten.
- Unterstützung für die nachverfolgung, wenn ein Finger hinzugefügt oder von der Authentifizierung entfernt.
- Die Möglichkeit, _Authentifizierungskontext_ in Keychain-Aufrufe und Support für Ihre Evaluierung von Keychain-Access Control werden aufgelistet.
- Die Fähigkeit, eine Eingabeaufforderung aus Code abzubrechen.

Informieren Sie sich unsere [Einführung in die Touch ID](~/ios/platform/touchid.md) Dokumentation für Weitere Informationen zum Arbeiten mit Touch ID in Xamarin.iOS.

### <a name="lacontext-changes"></a>LAContext Änderungen

Die folgenden Änderungen wurden an die [LAContext](https://developer.xamarin.com/api/type/LocalAuthentication.LAContext/) -Klasse für iOS 9:

- **TouchIdAuthenticationMaximumAllowableReuseDuration** - Returns the maximum amount of time that a touch ID authentication can be reused.
- **EvaluatedPolicyDomainState** : Ruft ab oder legt den Zustand einer ausgewertete Richtlinie fest.
- **MaxBiometryFailures** -wurde unter iOS 9 abgeschrieben wurden.
- **TouchIdAuthenticationAllowableReuseDuration** Gets or sets the amount of time that a touch ID authentication can be reused.
- **EvaluateAccessControl** – asynchron eine Authentifizierungsrichtlinie ergibt.
- **Für ungültig erklären** -erklärt ein bestimmten Touch-ID-Authentifizierung.
- **IsCredentialSet** -gibt `true` , wenn die Anmeldeinformationen momentan festgelegt sind.
- **SetCredentialType** legt den angegebenen Anmeldeinformationen fest.

Informieren Sie sich von Apple [LAContext Verweis](https://developer.apple.com/library/prerelease/ios/documentation/LocalAuthentication/Reference/LAContext_Class/index.html#//apple_ref/occ/instm/LAContext/evaluatePolicy:localizedReason:reply:) Weitere Details.

## <a name="mapkit-framework-changes"></a>MapKit-Frameworkänderungen

Apple enthalten die folgenden Änderungen an der [MapKit](https://developer.xamarin.com/api/namespace/MapKit/) -Framework in iOS 9:

- MapKit bietet jetzt Unterstützung für den Start der Karten-app direkt in der Übertragung Anweisungen und zum Abfragen von während der Übertragung geschätzte Zeit der Ankunft (ETA) Verwendung der [MKLaunchOptions](https://developer.xamarin.com/api/type/MapKit.MKLaunchOptions/) und [MKDirections](https://developer.xamarin.com/api/type/MapKit.MKLaunchOptions/) Klassen.
- Die Suchergebnisse von MapKit zurückgegeben und die [CLGeocoder](https://developer.xamarin.com/api/type/CoreLocation.CLGeocoder/) Klasse kann auch das Ergebnis der Zeitzone angeben.
- Sie können jetzt vollständig anpassen, Map-Anmerkungen, präsentiert von Ihrer iOS-app über die `DetailCalloutAccessoryView` Eigenschaft der [MKAnnotationView](https://developer.xamarin.com/api/type/MapKit.MKAnnotationView/) Klasse.

Informieren Sie sich unsere [iOS Maps](~/ios/user-interface/controls/ios-maps/index.md) und [Exemplarische Vorgehensweise: Untersuchen von Anmerkungen und Überlagerungen in MapKit](~/ios/user-interface/controls/ios-maps/ios-maps-walkthrough.md) Dokumentation für Weitere Informationen zum Arbeiten mit Karten und Anmerkungen in Xamarin.iOS- und Apple [CLGeocoder Verweis](https://developer.apple.com/library/prerelease/ios/documentation/CoreLocation/Reference/CLGeocoder_class/index.html#//apple_ref/occ/cl/CLGeocoder) für Weitere Informationen.

## <a name="passkit-framework-additions"></a>PassKit-Framework-Erweiterungen

Apple enthalten die folgenden Änderungen an der [PassKit](https://developer.xamarin.com/api/namespace/PassKit/) -Framework in iOS 9:

- Apple Pay unterstützt jetzt sowohl für Speicher soll-als auch für Kreditkarten zusammen mit der Discover-Karten. Finden Sie unter den **Zahlung Netzwerke** Abschnitt des Apple [PKPaymentRequest Class Reference](https://developer.apple.com/library/prerelease/ios/documentation/PassKit/Reference/PKPaymentRequest_Ref/index.html#//apple_ref/doc/uid/TP40014832) für Weitere Informationen.
- Von können direkt in einer Xamarin.iOS-app, Sie jetzt Zahlung-Netzwerken und belastungsrisiken Apple Pay hinzufügen. Finden Sie unter Apple [PKAddPaymentPassViewController Class Reference](https://developer.apple.com/library/prerelease/ios/documentation/PassKit/Reference/PKAddPaymentPassViewController_Class/index.html#//apple_ref/doc/uid/TP40016116) Weitere Details.

Informieren Sie sich unsere [Einführung in PassKit](~/ios/platform/passkit.md) Dokumentation für Weitere Informationen zum Arbeiten mit PassKit in Xamarin.iOS.

## <a name="safari-services-framework-additions"></a>Safari Services Framework-Erweiterungen

Apple enthalten die folgenden Änderungen an der [Safari Services](https://developer.xamarin.com/api/namespace/SafariServices/) -Framework in iOS 9:

- Nun können Sie die neue [SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) Klasse, um Webinhalt in einer Xamarin.iOS-app anzuzeigen. Es bietet die Möglichkeit, die Websitedaten und Cookies für die Safari-app freigeben und mehrere von Safari-Features (z. B. "Reader" und "AutoAusfüllen") enthält. [SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) Funktionen eine **Fertig** Schaltfläche, die Benutzer auf Ihre app zurückgegeben wird, wenn sie nach dem Anzeigen der Inhalt des Webserver sind.

Da die [SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) Klasse für die Anzeige einer einzelnen Seite des Web-Inhalte angepasst wird, sollten Sie verwenden, ersetzen [WKWebKit](xref:WebKit.WKWebView) oder [UIWebView](xref:UIKit.UIWebView)Steuerelemente in Ihre vorhandenen Xamarin.iOS-apps.

### <a name="displaying-a-website"></a>Anzeigen von einer Website

Der folgende Code ist ein Beispiel eines Aufrufs einer [SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) aus in einer anderen ansichtscontroller:

```csharp
// Create an instance of the Safari Services View Controller
var controller = new SFSafariViewController(new NSUrl("http://www.xamarin.com"));

// Display website
PresentViewController(controller, true, null);
```

## <a name="uikit-framework-changes"></a>Änderungen von UIKit-Framework

Apple hat viele Verbesserungen an mehrere Elemente enthalten die [UIKit](xref:UIKit) Framework für iOS 9. In den folgenden Abschnitten werden diese Änderungen beschreiben.

### <a name="3d-touch-events"></a>3D Touch-Ereignissen

Noch nicht mit iOS 9 und das iPhone 6 s und iPhone 6 s Plus, 3D Touch hinzugefügt Druck vertrauliche Gesten Ihre iOS-apps. Daher, wenn Ihre app unter iOS 9 (oder höher) ausgeführt wird, und das iOS-Gerät ist in der Lage, von der unterstützenden 3D Touch, Änderungen in der speicherauslastung führt dazu, dass die `TouchesMoved` Ereignis ausgelöst wurde. 

Aufgrund dieser Änderung im Verhalten Ihrer iOS-apps vorbereitet sein sollte der `TouchesMoved` aufzurufenden Ereignisses häufiger, selbst wenn das X / Y-Koordinaten nicht geändert wurden. 

Weitere Informationen finden Sie unserem [Einführung in 3D Touch](~/ios/platform/3d-touch.md) Guide.

### <a name="document-open-in-place-functionality"></a>Dokument öffnen-in-Place-Funktion

Indem Sie entweder die `FinishedLaunching (application, launchOptions)` oder `WillFinishLaunching (Application, launchOptions)` Methoden der [UIApplicationDelegate](xref:UIKit.UIApplicationDelegate) -Klasse, Sie können nun ein Dokument öffnen und ändern Sie sie an der Stelle (im Gegensatz zum Arbeiten an einer Kopie).

Um die neuen Open-in-Place-Funktionen zu unterstützen, fügen die `LSSupportsOpeningDocumentsInPlace` zu Ihrer Xamarin.iOS-app Key **"Info.plist"** Datei mit einem Wert von `YES`.

Informieren Sie sich von Apple [UIApplicationDelegate Verweis](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/index.html#//apple_ref/occ/intf/UIApplicationDelegate) Weitere Details.

### <a name="enhanced-touch-events"></a>Verbesserte Touch-Ereignissen

Apple hat mehrere Verbesserungen von Touch-Ereignissen unter iOS 9 bereitgestellt werden. Dazu gehören die Möglichkeit, die Touch-Vorhersage verwenden und für den Zugriff auf fortgeschrittene Workflows zwischen der Anzeige aktualisiert.

Informieren Sie sich von Apple [Event Handling Guide für iOS](https://developer.apple.com/library/ios/documentation/EventHandling/Conceptual/EventHandlingiPhoneOS/Introduction/Introduction.html) Weitere Details.

### <a name="fetching-tailored-content"></a>Maßgeschneiderte Inhalt abrufen

Die neue `NSDataAsset` -Klasse ermöglicht eine Xamarin.iOS-app zum Abrufen von Inhalt auf den Arbeitsspeicher und Grafikfunktionen kennen, die iOS-Geräts, die er zurzeit ausgeführt wird, auf zugeschnitten sind.

### <a name="new-layout-anchors"></a>Neues Layout Anker

Die neue `NSLayoutAnchor` und `NSLayoutDimension` Layout Anker Klassen arbeiten mit den neuen Anchor-Eigenschaften von der [UIView](xref:UIKit.UIView) Klasse (z. B. `LeadingAnchor` und `WidthAnchor`) zum Layout in iOS 9 zu vereinfachen.

Finden Sie unsere [Einführung in Storyboards Unified](~/ios/user-interface/storyboards/unified-storyboards.md) Dokumentation für Weitere Informationen zum Arbeiten mit AutoLayout und Größenklassen in einem Xamarin.iOS-app und das Apple [NSLayoutAnchor Verweis](https://developer.apple.com/library/prerelease/ios/documentation/AppKit/Reference/NSLayoutAnchor_ClassReference/index.html#//apple_ref/occ/cl/NSLayoutAnchor), [ NSLayoutDimension Verweis](https://developer.apple.com/library/prerelease/ios/documentation/AppKit/Reference/NSLayoutDimension_ClassReference/index.html#//apple_ref/occ/cl/NSLayoutDimension) und [UIView-Verweis](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIView_Class/index.html#//apple_ref/occ/cl/UIView) für Weitere Informationen.

### <a name="new-readable-content-margins"></a>Neue lesbare Ränder des Inhalts

Die neue `UILayoutGuide` Klasse kann verwendet werden, um bieten lesbare Ränder des Inhalts und der Draw-Bereiche für Inhalt innerhalb einer Ansicht zu definieren. Finden Sie unter Apple [UILayoutGuide Verweis](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UILayoutGuide_Class_Reference/index.html#//apple_ref/occ/cl/UILayoutGuide) für Weitere Informationen.

### <a name="text-input-in-notifications-modifications"></a>Texteingabe Benachrichtigungen Änderungen

Die [UIUserNotificationAction](xref:UIKit.UIUserNotificationAction) -Klasse verfügt über eine neue `Behavior` -Eigenschaft, die verwendet werden kann, um die Texteingabe von Benachrichtigungen zu unterstützen.

### <a name="uiapplicationdelegate-changes"></a>UIApplicationDelegate Änderungen

Während offiziell nicht von Apple veraltet, sie vorschlagen und Ersetzen Sie dabei alle Aufrufe an die `FinishedLaunching (UIApplication application)` -Methode der der [UIApplicationDelegate](xref:UIKit.UIApplicationDelegate) Klasse entweder mit der `FinishedLaunching (UIApplication application, NSDictionary launchOptions)` oder `WillFinishLaunching (UIApplication application, NSDictionary launchOptions)` Methoden.

Informieren Sie sich von Apple [UIApplicationDelegate Verweis](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/index.html#//apple_ref/occ/intf/UIApplicationDelegate) Weitere Details.

### <a name="uikit-dynamics-changes"></a>UIKit Dynamics-Änderungen

Apple enthalten die folgenden Änderungen an der Dynamik UIKit in iOS 9:

- Dynamics bietet jetzt Unterstützung für nicht rechteckige Kollisionen Grenzen.
- Die neuen, anpassbaren `UIFieldBehavior` Klasse dient zur Unterstützung verschiedener Feldtypen.
- Zusätzliche Arten von hinzugefügt wurden die `UIAttachmentBehavior` Klasse.

Informieren Sie sich von Apple [UIAttachment Verweis](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIAttachmentBehavior_Class/index.html#//apple_ref/occ/cl/UIAttachmentBehavior) Weitere Details.

### <a name="uipickerview-and-uidatepicker-changes"></a>UIPickerView und UIDatePicker-Änderungen

Vor iOS 9 die [UIPickerView](xref:UIKit.UIPickerView) und [UIDatePicker](xref:UIKit.UIDatePicker) Steuerelemente wurden nicht veränderbare Größen und würde die Größe automatisch zum Ausfüllen der Breite ihres Containers (in der Regel die Breite des iOS-Geräts die app wurde ausgeführt).

In iOS 9 tritt diese automatische Größenänderung nicht mehr auf, und die Steuerelemente mit einer Breite 320 zeigen auf alle iOS-Geräten unabhängig von der Bildschirmgröße und-Ausrichtung gerendert werden.

Um diesen Fehler zu beheben, verwenden Sie Automatisches Layout und Größenklassen heften Sie die Breite des Steuerelements an den Kanten des übergeordneten Containers (Ansicht), und geben Sie die erforderliche Höhe ein. Informieren Sie sich unsere [Einführung in Storyboards Unified](~/ios/user-interface/storyboards/unified-storyboards.md) Dokumentation für Weitere Informationen zum Arbeiten mit Automatisches Layout und Größe-Klassen in einer Xamarin.iOS-app.

### <a name="new-uitextinputassistantitem-class"></a>Neue UITextInputAssistantItem-Klasse

Verwenden Sie die neue `UITextInputAssistantItem` Klasse Layout Leiste Schaltfläche Gruppen in einer _Shortcut-Leiste_. Die Verknüpfung Leiste befindet es sich um einen neuen Bereich, der in die Bildschirmtastatur Typisierung Tastenkombinationen bereitstellen verfügbar ist.



## <a name="related-links"></a>Verwandte Links

- [iOS 9-Beispiele](https://developer.xamarin.com/samples/ios/iOS9/)
- [Einführung in iOS 9](~/ios/platform/introduction-to-ios9/index.md)
- [iOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [Was ist neu in iOS 9.0 verfügen](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
