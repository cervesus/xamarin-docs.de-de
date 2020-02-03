---
title: Weitere Änderungen an IOS 9-Frameworks
description: In diesem Dokument werden zusätzliche in ios 9 eingeführte Framework-Änderungen beschrieben. Es erläutert AVFoundation, avkit und cloudkit.
ms.prod: xamarin
ms.assetid: CFDE1FC4-9327-402B-95A0-581D4AA0E9D5
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: fd9bced0d2185fd9bd0d18932921c101b2ed207c
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725176"
---
# <a name="additional-ios-9-frameworks-changes"></a>Weitere Änderungen an IOS 9-Frameworks

_In diesem Artikel werden zusätzliche, kleinere Änderungen oder Verbesserungen an vorhandenen Frameworks für IOS 9 behandelt._

[![](additional-framework-changes-images/ios9-sml.png "iOS 9 Logo")](additional-framework-changes-images/ios9.png#lightbox)

Zusätzlich zu den wichtigsten Änderungen an IOS hat Apple Änderungen und Verbesserungen an mehreren vorhandenen Frameworks in ios 9 vorgenommen.

## <a name="avfoundation-framework-additions"></a>Erweiterungen des AVFoundation-Frameworks

Im AVFoundation-Framework ermöglicht Ihnen die [avvoicesynsvoice](xref:AVFoundation.AVSpeechSynthesisVoice) -Klasse nun, zusätzlich zu der Sprache eine Stimme nach dem Bezeichner anzugeben.

Mit dem folgenden Code wird beispielsweise eine Liste aller verfügbaren Stimmen abgerufen:

```csharp
var voices = AVSpeechSynthesisVoice.GetSpeechVoices ();
```

Sie können dann eine der Stimmen aus der Liste verwenden, indem Sie Sie als `Voice`-Eigenschaft einer Instanz der [avspeachutterance](xref:AVFoundation.AVSpeechUtterance) -Klasse festlegen.

Die [avqueueplayer](xref:AVFoundation.AVQueuePlayer) -Klasse unterstützt jetzt eine Mischung aus Internet Streaming und Datei basiertem Medium in der Warteschlange. In früheren Versionen konnten nur Medien des gleichen Typs in die Warteschlange gestellt werden.

Weitere Informationen finden Sie in der Referenz zu [avvoicesynsvoice](https://developer.apple.com/library/prerelease/ios/documentation/AVFoundation/Reference/AVSpeechSynthesisVoice_Ref/index.html#//apple_ref/occ/cl/AVSpeechSynthesisVoice)von Apple.

## <a name="avkit-framework-additions"></a>Erweiterungen des avkit-Frameworks

Um mit dem neuen Bild-in-Bild (PIP)-Feature arbeiten zu können, enthält das avkit-Framework die neuen `AVPictureInPictureController`-und [avplayerviewcontroller](xref:AVKit.AVPlayerViewController) -Klassen:

- **Avpicturereinpicturecontroller** : Diese Klasse ermöglicht einer IOS 9-APP die Antwort auf den Benutzer, der die Wiedergabe eines Videos in einem unverankerten, in der Größe verfügbaren PIP-Fenster auf einem iPad startet.
- **Avplayerviewcontroller** : verwaltet einen `AVPlayer` Controller, der zum Darstellen eines Videos in einem unverankerten, in der Größe verwertbaren PIP-Fenster auf einem iPad verwendet wird.

Weitere Informationen finden Sie in unserer Dokumentation [zum Multitasking für iPad](~/ios/platform/introduction-to-ios9/index.md#multitasking) und der [avpicturereinpicturecontroller-Referenz](https://developer.apple.com/library/prerelease/ios/documentation/AVKit/Reference/AVPictureInPictureController_Class/index.html#//apple_ref/occ/cl/AVPictureInPictureController) von Apple und der [avplayerviewcontroller-Referenz](https://developer.apple.com/library/prerelease/ios/documentation/AVFoundation/Reference/AVPlayerViewController_Class/index.html#//apple_ref/occ/cl/AVPlayerViewController).

## <a name="introducing-cloudkit-web-services"></a>Einführung in cloudkit-Webdienste

Das cloudkit-Framework optimiert die Entwicklung von Anwendungen, die auf icloud zugreifen. Dies umfasst das Abrufen von Anwendungsdaten und assetrechten sowie die Möglichkeit, Anwendungsinformationen sicher zu speichern. Dieses Kit bietet Benutzern eine Ebene der Anonymität, indem Sie den Zugriff auf Anwendungen mit ihren icloud-IDs ermöglicht, ohne persönliche Informationen zu teilen.

Das neue _cloudkit-Webdienst_ -Framework bietet eine JavaScript-Bibliothek (cloudkit js), die in Ihre Website integriert werden kann, um den Zugriff auf die gleichen cloudkit-basierten Daten und Inhalte wie Ihre xamarin. IOS-App bereitzustellen.

> [!IMPORTANT]
> Bevor Sie mithilfe von cloudkit js auf Inhalte einer cloudkit-Datenbank zugreifen, diese präsentieren oder aktualisieren können, müssen Sie zuvor das Schema der Datenbank definiert haben.

Weitere Informationen finden Sie in den folgenden Dokumenten:

- [Einführung in cloudkit](~/ios/data-cloud/intro-to-cloudkit.md) : unsere Einführung in die Verwendung von cloudkit in einer xamarin. IOS-app.
- [Cloudkit-Schnellstart](https://developer.apple.com/library/prerelease/ios/documentation/DataManagement/Conceptual/CloudKitQuickStart/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014987) : Einführung in cloudkit von Apple.
- [Cloudkit js-Referenz](https://developer.apple.com/library/prerelease/ios/documentation/CloudKitJS/Reference/CloudKitJavaScriptReference/index.html#//apple_ref/doc/uid/TP40015359) : die cloudkit js-Dokumentation von Apple.
- [Cloudkit-Katalog: eine Einführung in cloudkit (Cocoa und JavaScript):](https://developer.apple.com/library/prerelease/ios/samplecode/CloudAtlas/Introduction/Intro.html#//apple_ref/doc/uid/TP40014599) die Beispiel-APP von Apple mithilfe von cloudkit und cloudkit js.

> [!IMPORTANT]
> Apple [stellt Tools zur Verfügung](https://developer.apple.com/support/allowing-users-to-manage-data/), die Entwickler dabei unterstützen, die Datenschutz-Grundverordnung (DSGVO) der Europäischen Union umzusetzen.

## <a name="foundation-framework-additions"></a>Foundation Framework-Ergänzungen

Apple umfasste die folgenden Änderungen am Foundation Framework in ios 9:

### <a name="changes-to-nsbundle"></a>Änderungen an NSBundle

Die folgenden Änderungen wurden an der [NSBundle](xref:Foundation.NSBundle) -Klasse für IOS 9 vorgenommen:

- `GetPreservationPriorityForTag (NSString tag)`: Ruft die aktuelle Beibehaltungs Priorität für Ressourcen mit dem angegebenen Tag ab. Gültige Werte liegen im Bereich `0.0` zu `1.0`, Ressourcen mit der niedrigsten Priorität werden zuerst gelöscht.
- `SetPreservationPriorityForTag (double priority, NSSet tags)`: legt die aktuelle Beibehaltungs Priorität für Ressourcen mit den angegebenen Tags fest. Gültige Werte liegen im Bereich `0.0` zu `1.0`, Ressourcen mit der niedrigsten Priorität werden zuerst gelöscht.

Weitere Informationen finden Sie in der Referenz zu [NSBundle](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSBundle_Class/index.html#//apple_ref/occ/cl/NSBundle)von Apple.

### <a name="changes-to-nsprocessinfo"></a>Änderungen an nsprocessinfo

Jeder Prozess, der auf einem IOS-Gerät ausgeführt wird, verfügt über einen einzelnen _Prozess Information Agent_ (PIA). Verwenden Sie die [nsprocessinfo](xref:Foundation.NSProcessInfo) -Klasse, um Informationen zur aktuellen Pia bereitzustellen und die Energie-und Wärmeverwaltung für einen bestimmten Prozess zu steuern.

Wenn Sie z. b. die automatische Beendigung eines Prozesses steuern möchten, können Sie den folgenden Code verwenden:

```csharp
// Disable automatic termination
var activity = NSProcessInfo.ProcessInfo.BeginActivity(NSActivityOptions.AutomaticTerminationDisabled, "Define reason for change here...");

// Perform the required task
...

// Return to normal operation
NSProcessInfo.ProcessInfo.EndActivity(activity);
```

Weitere Informationen finden Sie in der Referenz zu [nsprocessinfo](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSProcessInfo_Class/index.html#//apple_ref/occ/cl/NSProcessInfo)von Apple.

### <a name="reacting-to-low-power-mode"></a>Reaktion auf den Low-Power-Modus

Verwenden Sie die `LowPowerModeEnabled`-Eigenschaft der [nsprocessinfo](xref:Foundation.NSProcessInfo) -Klasse, um zu bestimmen, ob der niedrige Energie Modus auf dem IOS-Gerät aktiviert wurde, auf dem die app ausgeführt wird. Beispiel:

```csharp
// Is the device in low power mode?
if (NSProcessInfo.ProcessInfo.LowPowerModeEnabled) {
    // Reduce activity to conserve energy...
} else {
    // Return to normal activity...
}
```

## <a name="healthkit-framework-changes"></a>Healthkit-Framework-Änderungen

Apple enthielt die folgenden Änderungen am [healthkit](xref:HealthKit) -Framework in ios 9:

- Unterstützung für das Massen löschen und Löschen von Einträgen in der healthkit-Datenbank. Weitere Informationen finden Sie in der Referenz zu den [hkdeletedobject](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKDeletedObject_ClassReference/index.html#//apple_ref/occ/cl/HKDeletedObject)-, [hkanchoredobjectquery](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKAnchoredObjectQuery_Class/index.html#//apple_ref/occ/cl/HKAnchoredObjectQuery) -und [hkhealthstore-Klassen](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKHealthStore_Class/index.html#//apple_ref/doc/uid/TP40014708) von Apple.
- Der `HKQuantityTypeIdentifier`-Klasse (z. b. `UVExposure`) und der `HKCategoryTypeIdentifier`-Klasse (z. b. `OvulationTestResult`) wurden neue nach Verfolgungs Kategorien und-Eigenschaften hinzugefügt. 

Weitere Informationen zum Arbeiten mit healthkit in xamarin. IOS finden Sie in der [Einführung zur healthkit](~/ios/platform/healthkit.md) -Dokumentation.

## <a name="local-authentication-framework-changes"></a>Änderungen am lokalen Authentifizierungs Framework

Apple enthielt die folgenden Änderungen am [lokalen Authentifizierungs](xref:LocalAuthentication) Framework in ios 9:

- Wenn Sie die Methoden `EvaluateAccessControl` und `EvaluatePolicy` der [lacontext](xref:LocalAuthentication.LAContext) -Klasse verwenden, können Sie jetzt Berührungs-ID-Übereinstimmungen aus vorherigen erfolgreichen entsperrungs versuchen wieder verwenden.
- Die Möglichkeit, eine Liste der derzeit registrierten Finger zu erhalten.
- Unterstützung für die Nachverfolgung, wenn ein Finger der Authentifizierung hinzugefügt oder entfernt wird.
- Die Möglichkeit, den _Authentifizierungs Kontext_ in Keychain-aufrufen und Unterstützung zum Auswerten von Zugriffs Steuerungs Listen für Keychain zu verwenden.
- Die Möglichkeit, eine Benutzereingabe Aufforderung aus dem Code abzubrechen.

Weitere Informationen finden Sie unter Fingereingabe [-ID und Face ID mit xamarin. IOS](~/ios/platform/touch-id-face-id.md).

### <a name="lacontext-changes"></a>Lacontext-Änderungen

Die folgenden Änderungen wurden an der [lacontext](xref:LocalAuthentication.LAContext) -Klasse für IOS 9 vorgenommen:

- **Touchidauthenticationmaximumallowablereulduration** : gibt den maximalen Zeitraum zurück, in dem eine Fingereingabe-ID-Authentifizierung wieder verwendet werden kann.
- **Evaluatedpolicydomainstate** : Ruft den Status einer ausgewerteten Richtlinie ab oder legt ihn fest.
- **Maxbiometryfailure** : wurde in ios 9 abgewertet.
- **Touchidauthenticationallowablereulduration** Ruft die Zeitspanne ab, in der eine Fingerabdruck-Authentifizierung wieder verwendet werden kann, oder legt diese fest.
- **Evaluateaccesscontrol** : wertet eine Authentifizierungs Richtlinie asynchron aus.
- **Invalidate** : gibt eine angegebene Fingereingabe-ID-Authentifizierung ungültig aus.
- **Iskredentialset** -gibt `true` zurück, wenn die Anmelde Informationen momentan festgelegt sind.
- **Setkredentialtype** Legt den angegebenen Anmelde Informationstyp fest.

Weitere Informationen finden Sie in der [lacontext-Referenz](https://developer.apple.com/library/prerelease/ios/documentation/LocalAuthentication/Reference/LAContext_Class/index.html#//apple_ref/occ/instm/LAContext/evaluatePolicy:localizedReason:reply:) von Apple.

## <a name="mapkit-framework-changes"></a>Änderungen des MapKit-Frameworks

Apple enthielt die folgenden Änderungen am [MapKit](xref:MapKit) -Framework in ios 9:

- MapKit bietet jetzt Unterstützung für das direkte Starten der Zuordnungs-app in Transit Richtungen und das Abfragen der Übertragung geschätzten Ankunftszeit (ETA) mithilfe der Klassen " [mklaunchoptions](xref:MapKit.MKLaunchOptions) " und " [mkdirections](xref:MapKit.MKLaunchOptions) ".
- Die Suchergebnisse, die von MapKit und der [clgeocoder](xref:CoreLocation.CLGeocoder) -Klasse zurückgegeben werden, können auch die Zeitzone des Ergebnisses bereitstellen.
- Sie können nun die von ihrer IOS-App dargestellten Karten Anmerkungen vollständig mit der `DetailCalloutAccessoryView`-Eigenschaft der [mkannotationview](xref:MapKit.MKAnnotationView) -Klasse anpassen.

Weitere Informationen zum Arbeiten mit Zuordnungen und Anmerkungen finden Sie [in der Dokumentation zu den](https://developer.apple.com/library/prerelease/ios/documentation/CoreLocation/Reference/CLGeocoder_class/index.html#//apple_ref/occ/cl/CLGeocoder) [IOS-Karten](~/ios/user-interface/controls/ios-maps/index.md) und exemplarischen Vorgehensweisen zum Untersuchen von [Anmerkungen und Überlagerungen in MapKit](~/ios/user-interface/controls/ios-maps/ios-maps-walkthrough.md) .

## <a name="passkit-framework-additions"></a>Passkit Framework-Ergänzungen

Apple enthielt die folgenden Änderungen am [passkit](xref:PassKit) -Framework in ios 9:

- Apple Pay unterstützt jetzt sowohl das Speichern von als auch das Kreditkartenkonto. Weitere Informationen finden Sie im Abschnitt " **Payment Networks** " in der [pkpaymentrequest-Klassenreferenz](https://developer.apple.com/library/prerelease/ios/documentation/PassKit/Reference/PKPaymentRequest_Ref/index.html#//apple_ref/doc/uid/TP40014832) von Apple.
- Direkt in einer xamarin. IOS-App können Sie Apple Pay jetzt Zahlungs Netzwerke und Kartenaussteller hinzufügen. Weitere Informationen finden Sie in der [Referenz zur pkaddpaymentpassviewcontroller-Klasse](https://developer.apple.com/library/prerelease/ios/documentation/PassKit/Reference/PKAddPaymentPassViewController_Class/index.html#//apple_ref/doc/uid/TP40016116) von Apple.

Weitere Informationen zum Arbeiten mit passkit in xamarin. IOS finden Sie in der [Einführung zur passkit](~/ios/platform/passkit.md) -Dokumentation.

## <a name="safari-services-framework-additions"></a>Safari Services Framework-Ergänzungen

Apple enthielt die folgenden Änderungen am [Safari Services](xref:SafariServices) -Framework in ios 9:

- Sie können jetzt die neue [sfsafariviewcontroller](xref:SafariServices.SFSafariViewController) -Klasse verwenden, um Webinhalte in einer xamarin. IOS-App anzuzeigen. Es bietet die Möglichkeit, Website Daten und Cookies mit der Safari-App gemeinsam zu nutzen und umfasst mehrere Funktionen von Safari (z. b. Reader und AutoFill). [Sfsafariviewcontroller](xref:SafariServices.SFSafariViewController) verfügt über eine **Fertig** gestellte Schaltfläche, die Benutzer zu Ihrer APP zurückgibt, wenn Sie die Anzeige des Webinhalts abgeschlossen haben.

Da die [sfsafariviewcontroller](xref:SafariServices.SFSafariViewController) -Klasse auf die Anzeige einer einzelnen Seite von Webinhalt zugeschnitten ist, sollten Sie diese zum Ersetzen beliebiger [wkwebkit](xref:WebKit.WKWebView) -oder [UIWebView](xref:UIKit.UIWebView) -Steuerelemente in Ihren vorhandenen xamarin. IOS-Apps verwenden.

### <a name="displaying-a-website"></a>Anzeigen einer Website

Der folgende Code ist ein Beispiel für das Aufrufen eines [sfsafariviewcontroller](xref:SafariServices.SFSafariViewController) in einem anderen Ansichts Controller:

```csharp
// Create an instance of the Safari Services View Controller
var controller = new SFSafariViewController(new NSUrl("http://www.xamarin.com"));

// Display website
PresentViewController(controller, true, null);
```

## <a name="uikit-framework-changes"></a>Änderungen am UIKit-Framework

Apple hat viele Verbesserungen für mehrere Elemente des [UIKit](xref:UIKit) -Frameworks für IOS 9 integriert. Diese Änderungen werden in den folgenden Abschnitten ausführlich erläutert.

### <a name="3d-touch-events"></a>3D-Berührungs Ereignisse

Neu bei IOS 9 und der iPhone 6S und iPhone 6S Plus ist eine 3D-Toucheingabe, die ihren IOS-apps druckempfindliche Gesten hinzufügt. Wenn Ihre APP unter IOS 9 (oder höher) ausgeführt wird und das IOS-Gerät die 3D-Fingereingabe unterstützen kann, wird dadurch das `TouchesMoved` Ereignis ausgelöst.

Aufgrund dieser Änderung des Verhaltens sollten ihre IOS-apps darauf vorbereitet sein, dass das `TouchesMoved` Ereignis häufiger aufgerufen wird, selbst wenn sich die X/Y-Koordinaten nicht geändert haben.

Weitere Informationen finden Sie im Artikel [Einführung in die 3D-](~/ios/platform/3d-touch.md) Eingabe Anleitung.

### <a name="document-open-in-place-functionality"></a>Dokument-Open-in-Place-Funktionalität

Indem Sie entweder die Methoden `FinishedLaunching (application, launchOptions)` oder `WillFinishLaunching (Application, launchOptions)` der [uiapplicationdelegatklasse](xref:UIKit.UIApplicationDelegate) verwenden, können Sie jetzt ein Dokument öffnen und direkt ändern (im Gegensatz zum Arbeiten an einer Kopie).

Fügen Sie der Datei " **Info. plist** " ihrer xamarin. IOS-App mit dem Wert `YES`den `LSSupportsOpeningDocumentsInPlace`-Schlüssel hinzu, um die neue offene Funktionalität zu unterstützen.

Weitere Informationen finden Sie in der [uiapplicationdelegat-Referenz](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/index.html#//apple_ref/occ/intf/UIApplicationDelegate) von Apple.

### <a name="enhanced-touch-events"></a>Erweiterte Berührungs Ereignisse

Apple hat mehrere Verbesserungen für Berührungs Ereignisse in ios 9 bereitgestellt. Dazu gehört auch die Möglichkeit, touchvorhersage zu verwenden und Zugriff auf zwischen einblenden zwischen Anzeige Aktualisierungen zu erhalten.

Weitere Informationen finden Sie im [Leitfaden zur Ereignis Behandlung von Apple für IOS](https://developer.apple.com/library/ios/documentation/EventHandling/Conceptual/EventHandlingiPhoneOS/Introduction/Introduction.html) .

### <a name="fetching-tailored-content"></a>Abrufen von angepassten Inhalten

Die neue `NSDataAsset`-Klasse ermöglicht einer xamarin. IOS-APP das Abrufen von Inhalten, die auf die Arbeitsspeicher-und Grafikfunktionen des IOS-Geräts zugeschnitten sind, auf dem Sie derzeit ausgeführt wird.

### <a name="new-layout-anchors"></a>Neue layoutanker

Die neuen Anker Klassen `NSLayoutAnchor` und `NSLayoutDimension` Layouts arbeiten mit den neuen Anker Eigenschaften der [UIView](xref:UIKit.UIView) -Klasse (z. b. `LeadingAnchor` und `WidthAnchor`), um das Layout in ios 9 zu vereinfachen.

Weitere Informationen zum Arbeiten mit den Klassen "AutoLayout" und "size" in einer xamarin. IOS-APP und der [nslayoutanchor-Referenz](https://developer.apple.com/library/prerelease/ios/documentation/AppKit/Reference/NSLayoutAnchor_ClassReference/index.html#//apple_ref/occ/cl/NSLayoutAnchor)von Apple, [nslayoutdimension-Referenz](https://developer.apple.com/library/prerelease/ios/documentation/AppKit/Reference/NSLayoutDimension_ClassReference/index.html#//apple_ref/occ/cl/NSLayoutDimension) und [UIView-Referenz](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIView_Class/index.html#//apple_ref/occ/cl/UIView) finden Sie in der Dokumentation [zu Unified Storyboards](~/ios/user-interface/storyboards/unified-storyboards.md) .

### <a name="new-readable-content-margins"></a>Neue lesbare Inhalts Ränder

Die neue `UILayoutGuide`-Klasse kann verwendet werden, um lesbare Inhalts Ränder bereitzustellen und die Zeichnungs Bereiche für Inhalte innerhalb einer Ansicht zu definieren. Weitere Informationen finden Sie in der [uilayoutguide-Referenz](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UILayoutGuide_Class_Reference/index.html#//apple_ref/occ/cl/UILayoutGuide) von Apple.

### <a name="text-input-in-notifications-modifications"></a>Text Eingabe in Benachrichtigungs Änderungen

Die [uiusernotificationaction](xref:UIKit.UIUserNotificationAction) -Klasse verfügt über eine neue `Behavior`-Eigenschaft, die zur Unterstützung von Texteingaben aus Benachrichtigungen verwendet werden kann.

### <a name="uiapplicationdelegate-changes"></a>Uiapplicationdelegatenänderungen

Obwohl es nicht formal von Apple als veraltet markiert ist, empfehlen Sie, alle Aufrufe der `FinishedLaunching (UIApplication application)`-Methode der [uiapplicationdelegatklasse](xref:UIKit.UIApplicationDelegate) durch die Methoden `FinishedLaunching (UIApplication application, NSDictionary launchOptions)` oder `WillFinishLaunching (UIApplication application, NSDictionary launchOptions)` zu ersetzen.

Weitere Informationen finden Sie in der [uiapplicationdelegat-Referenz](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/index.html#//apple_ref/occ/intf/UIApplicationDelegate) von Apple.

### <a name="uikit-dynamics-changes"></a>UIKit Dynamics-Änderungen

Apple umfasste die folgenden Änderungen an der UIKit Dynamics in ios 9:

- Dynamics bietet jetzt Unterstützung für nicht rechteckige Kollisions Grenzen.
- Die neue, anpassbare `UIFieldBehavior`-Klasse wird verwendet, um verschiedene Feldtypen zu unterstützen.
- Der `UIAttachmentBehavior`-Klasse wurden zusätzliche Anlagentypen hinzugefügt.

Weitere Informationen finden Sie in der [uiattachment-Referenz](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIAttachmentBehavior_Class/index.html#//apple_ref/occ/cl/UIAttachmentBehavior) von Apple.

### <a name="uipickerview-and-uidatepicker-changes"></a>Uipickerview-und UIDatePicker-Änderungen

Vor IOS 9 konnten das [uipickerview](xref:UIKit.UIPickerView) -Steuerelement und das [UIDatePicker](xref:UIKit.UIDatePicker) -Steuerelement nicht in der Größe geändert werden, und die Größe des Containers wird automatisch geändert (in der Regel die Breite des IOS-Geräts, auf dem die app ausgeführt wird).

In ios 9 tritt diese automatische Größenänderung nicht mehr auf, und die Steuerelemente werden auf allen IOS-Geräten mit einer 320-Punkt Breite gerendert, unabhängig von Bildschirmgröße und Ausrichtung.

Um dieses Problem zu beheben, verwenden Sie die Klassen automatisch Layout und Größe, um die Breite des Steuer Elements an die Ränder des übergeordneten Containers (Ansicht) anzuheften und die erforderliche Höhe anzugeben. Weitere Informationen zum Arbeiten mit automatischen layoutklassen und Größenklassen in einer xamarin. IOS-App finden Sie in der Dokumentation [Introduction to Unified Storyboards](~/ios/user-interface/storyboards/unified-storyboards.md) .

### <a name="new-uitextinputassistantitem-class"></a>Neue uitextinputassistantitem-Klasse

Verwenden Sie die neue `UITextInputAssistantItem`-Klasse, um Balken Schaltflächen Gruppen in einer Verknüpfungs _Leiste_zu verwenden. Die Verknüpfungs Leiste ist ein neuer Bereich, der in der Bildschirmtastatur verfügbar ist, um Tipp Verknüpfungen bereitzustellen.

## <a name="related-links"></a>Verwandte Links

- [IOS 9-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [Einführung in iOS 9](~/ios/platform/introduction-to-ios9/index.md)
- [IOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [Neuerungen in ios 9,0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
