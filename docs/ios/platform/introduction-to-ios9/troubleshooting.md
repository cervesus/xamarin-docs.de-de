---
title: Problembehandlung
description: Dieser Artikel enthält einige Tipps zur Problembehandlung für das Arbeiten mit iOS 9 in Xamarin.iOS-apps.
ms.prod: xamarin
ms.assetid: DCE83E36-CBD9-4D96-8E7F-384CB8A54563
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 1b335fc6b19d87a46059511baf866433691b1b4d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="troubleshooting"></a>Problembehandlung

_Dieser Artikel enthält einige Tipps zur Problembehandlung für das Arbeiten mit iOS 9 in Xamarin.iOS-apps._

## <a name="there-was-a-problem-parsing-the-xml"></a>Es wurde ein Problem beim Analysieren des XML-Codes

Xamarin iOS-Designer unterstützt noch keine Xcode 7-Funktionen. Storyboards werden nicht im Designer mit geladen _"Ist ein Problem mit dem XML-Analyse"_ beim Versuch, neue iOS 9 (Xcode 7) zu verwenden, z. B. StackView designerelementen.

iOS-Designer-Unterstützung für Funktionen Xcode 7 ist für die anstehenden Zyklus 6 Feature-Version gedacht. Die Preview-Version des Zyklus 6 ist zurzeit in den Alpha-Kanal verfügbar und bietet eingeschränkte Unterstützung für die neuen Funktionen von Xcode 7.

Partielle problemumgehung für Visual Studio für Mac: das Storyboard Maustaste und wählen Sie **Öffnen mit** > **Xcode Schnittstelle-Generator**.

## <a name="where-are-the-ios-8-simulators"></a>Wo sind die iOS 8-Simulatoren?

Wenn Sie Xcode 7 (oder höher) installiert haben, wird dieser automatisch, ersetzen Sie aller iOS 8-Simulatoren standardmäßig mit iOS 9-Simulatoren. Wenn Sie weiterhin auf iOS 8 prüfen müssen, können Sie Xcode starten und dann herunterladen und Installieren von iOS 8-Simulatoren.

Wählen Sie in Xcode den **Xcode** klicken Sie dann im Menü **Einstellungen...**   >  **Downloads**:

[![](troubleshooting-images/ios8.png "iOS 8-Simulatoren Downloads")](troubleshooting-images/ios8.png#lightbox)

Klicken Sie auf die **Check "und" jetzt installieren** Schaltfläche, um die iOS 8-Simulatoren neu installieren.

## <a name="layout-constraint-with-leftright-attribute-errors"></a>Layout-Einschränkung mit Fehlern links/nach-rechts-Attribut

In iOS 8 (und früher) mithilfe von Elementen der Benutzeroberfläche in Storyboards eine Mischung aus beiden **rechts** & **Links** Attribute (`NSLayoutAttributeRight` & `NSLayoutAttributeLeft`) und  **Führende** & **nachgestellten** Attribute (`NSLayoutAttributeLeading` & `NSLayoutAttributeTrailing`) in dasselbe Layout.

Wenn in der gleiche Storyboard in iOS 9 ausführen zu können, führt dies zu einer Ausnahme in der folgenden Form:

> Beenden die app als aufgrund nicht abgefangene Ausnahme "NSInvalidArgumentException", Grund: "*** + [NSLayoutConstraint ConstraintWithItem:attribute:relatedBy:toItem:attribute:multiplier:constant:]: eine Einschränkung kann nicht vorgenommen werden, zwischen einem führende/nachfolgende Attribut und einem rechts /-Attribut. Verwenden Sie für beide oder keines von beiden führende/nachfolgende. "

iOS 9 erzwingt Layouts Verwendung entweder **rechts** & **Links** _oder_ **führende**  &   **Nachfolgende** Attribute jedoch *nicht* beide. Um dieses Problem zu beheben, ändern Sie alle Layout-Einschränkungen, um das gleiche Attribut festgelegt, die in Ihrer Storyboard-Datei verwenden.

Weitere Informationen finden Sie unter der [iOS 9-Einschränkungsfehler](http://stackoverflow.com/questions/32692841/ios-9-constraint-error) Diskussion Stapelüberlauf.

## <a name="error-itms-90535-unexpected-cfbundleexecutable-key"></a>Fehler ITMS-90535: Unerwarteter CFBundleExecutable-Schlüssel

Nach dem Umstellen einer app für iOS 9, 3rd Party-Komponenten (insbesondere unsere vorhandene Komponente Google Maps) verwendet werden, die kompiliert und IOS 8 (oder früher) ausgeführt wird, beim Übermitteln des neuen Builds iTunes verbinden, die Sie in der Form eine Fehlermeldung erhalten können:

> Fehler ITMS-90535: Unerwarteter CFBundleExecutable-Taste. Das Paket an "Payload/app-name.app/component.bundle" enthält keine ausführbare Bundle...

Diese Probleme können in der Regel werden gelöst, indem das benannte Paket im Projekt suchen und dann - ebenso wie die Fehlermeldung vorgeschlagen - bearbeitet die `Info.plist` , die sich auf das Paket durch das Entfernen der `CFBundleExecutable` Schlüssel. Die `CFBundlePackageType` Schlüssel sollte festgelegt werden, um `BNDL` ebenfalls.

Führen Sie nachdem diese Änderungen vorgenommen wurden eine Neuinstallation aus, und das gesamte Projekt neu. Sie sollten nach diesen Änderungen iTunes Connect problemlos übermitteln können.

Weitere Informationen finden Sie in der [Stack Overflow](http://stackoverflow.com/questions/32096130/unexpected-cfbundleexecutable-key) Erläuterung.

## <a name="cfnetwork-sslhandshake-failed--9824-error"></a>CFNetwork SSLHandshake Fehler (-9824)

Bei einem Verbindungsversuch mit dem Internet, entweder direkt oder über eine iOS 9-Webansicht, erhalten Sie möglicherweise einen Fehler in der Form:

```csharp
2015-09-04 14:38:05.757 FormsWebViewiOS[2553:30362] CFNetwork SSLHandshake failed (-9824)
2015-09-04 14:38:05.758 FormsWebViewiOS[2553:30363] NSURLSession/NSURLConnection HTTP load failed (kCFStreamErrorDomainSSL, -9824)
```

Oder in der Form:

```csharp
2015-09-04 14:39:17.881 FormsWebViewiOS[2568:30974] App Transport Security has blocked a cleartext HTTP (http://) resource load since it is insecure.
Temporary exceptions can be configured via your app's Info.plist file.
```

In iOS9 erzwingt App Transport Sicherheit (ATS) sichere Verbindungen zwischen Internet-Ressourcen (z. B. die app-Back-End-Server) und Ihre app. Darüber hinaus ATS erfordert Kommunikation unter Verwendung der `HTTPS` Protokoll und allgemeine API-Kommunikation mit TLS Version 1.2 mit forward Secrecy verschlüsselt werden.

Da ATS in apps für iOS 9 und OS X 10.11 (El Capitan), alle Verbindungen mit erstellt standardmäßig aktiviert ist. `NSURLConnection`, `CFURL` oder `NSURLSession` unterliegen ATS sicherheitsanforderungen sein wird. Wenn Ihre Verbindungen diese Anforderung nicht erfüllen, werden sie mit einer Ausnahme fehl.

Finden Sie unter der [Opting-Out-of ATS](~/ios/app-fundamentals/ats.md) Teil unserer [App Transportsicherheit](~/ios/app-fundamentals/ats.md) Handbuch Informationen zur Behebung dieses Problems.

## <a name="my-existing-apps-dont-run-on-ios-9"></a>Meine vorhandene apps ausführen nicht auf iOS 9.

Finden Sie in unserer [iOS 9-Kompatibilitätsinformationen](~/ios/platform/introduction-to-ios9/ios9.md) Anweisungen neu erstellen und erneut bereitstellen Ihrer vorhandenen apps auf iOS 9 ausgeführt.

<a name="UICollectionViewCell.ContentView-is-null-in-constructors" />

## <a name="uicollectionviewcellcontentview-is-null-in-constructors"></a>UICollectionViewCell.ContentView ist Null in Konstruktoren

**Ursache:** In iOS 9 die `initWithFrame:` Konstruktor ist mittlerweile erforderlich ist, aufgrund der verhaltensänderungen in iOS 9 als die [UICollectionView Dokumentation Zustände](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionView_class/#//apple_ref/occ/instm/UICollectionView/dequeueReusableCellWithReuseIdentifier:forIndexPath). Wenn Sie eine Klasse für den angegebenen Bezeichner registriert, und eine neue Zelle erstellt werden muss, wird die Zelle jetzt durch Aufrufen von initialisiert seine `initWithFrame:` Methode.

**Fix:** Hinzufügen der `initWithFrame:` Konstruktor wie folgt:

```csharp
[Export ("initWithFrame:")]
public YourCellClassName (CGRect frame) : base (frame)
{
    Initialize (); // refactor initialize code into a method
}
```

Im Zusammenhang Beispiele: [MotionGraph](https://github.com/xamarin/monotouch-samples/commit/3c1b7a4170c001e7290db9babb2b7a6dddeb8bcb), [TextKitDemo](https://github.com/xamarin/monotouch-samples/commit/23ea01b37326963b5ebf68bbcc1edd51c66a28d6)

<a name="UIView-fails-to-Init-with-Coder-when-Loading-a-View-from-a-Xib/Nib" />

## <a name="uiview-fails-to-init-with-coder-when-loading-a-view-from-a-xibnib"></a>UIView kann Init mit Programmierer beim Laden von einer Sicht aus einer Xib/Nib

**Ursache:** der `initWithCoder:` Konstruktor wird aufgerufen, wenn eine Sicht aus einer Schnittstelle-Generator Xib-Datei zu laden. Nicht verwalteter Code kann nicht unsere verwaltete Version der aufrufen, wenn dieser Konstruktor nicht exportiert wird. Zuvor (z. b. In iOS 8) das `IntPtr` Konstruktor wurde aufgerufen, um die Ansicht zu initialisieren.

**Fix:** erstellen und Exportieren der `initWithCoder:` Konstruktor wie folgt:

```csharp
[Export ("initWithCoder:")]
public YourClassName (NSCoder coder) : base (coder)
{
    Initialize (); // refactor initialize code into a method
}
```

Beispiel: [Chat](https://github.com/xamarin/monotouch-samples/commit/7b81138d52e5f3f1aa3769fcb08f46122e9b6a88)

## <a name="dyld-message-no-cache-image-with-name"></a>Dyld Nachricht: Keine Cache-Image mit dem Namen...

Sie können ein Absturz (Crash) mit den folgenden Informationen im Anwendungsprotokoll auftreten:

```csharp
Dyld Error Message:
Dyld Message: no cach image with name (/System/Library/PrivateFrameworks/JavaScriptCore.framework/JavaScriptCore)
```

**Ursache:** Dies ist ein Fehler in die systemeigene Apple-Linker, der Fall, wenn sie eine private Framework als öffentliches Profil festzulegen (JavaScriptCore erfolgte in iOS 7, öffentlich vor, dass es sich um eine private Framework war), und das Bereitstellungsziel der app für iOS-Version wird bei der Framework wurde die private. In diesem Fall wird der Apple-Linker mit der privaten Version des Frameworks statt die öffentliche Version verknüpft werden.

**Fix:** wird dies für iOS 9 adressiert werden, aber es ist eine einfache Lösung, die Sie selbst in der Zwischenzeit anwenden können: nur als Ziel eine höhere iOS Version in Ihrem Projekt (Sie können versuchen, iOS 7 in diesem Fall). Andere Frameworks möglicherweise ähnliche Probleme aufweisen, z. B. das WebKit-Framework wurde in iOS 8 öffentlich gemacht (und, Zielplattform iOS 7 zu diesem Fehler; daher sollten Sie als Ziel iOS 8 WebKit in Ihrer app verwenden).

## <a name="untrusted-enterprise-developer"></a>Untrusted Enterprise Developer

Beim Versuch, den iOS 9-Version Ihrer App Xamarin.iOS auf echten iOS Hardware ausführen, erhalten Sie möglicherweise eine Meldung mit Ihrem Entwicklerkonto nicht auf dem Gerät als vertrauenswürdig eingestuft worden ist. Zum Beispiel:

[![](troubleshooting-images/untrusted01.png "Nicht vertrauenswürdige Unternehmensentwickler-Warnung")](troubleshooting-images/untrusted01.png#lightbox)

Um dieses Problem zu beheben, führen Sie folgende Schritte aus:

1. Starten Sie Xcode (die neueste Betaversion) bei der Entwicklung Mac.
2. Wählen Sie **Geräte** aus der **Fenster** Menü zum Öffnen des Fensters Geräte: 

    [![](troubleshooting-images/untrusted02.png "Fenster \"Geräte\"")](troubleshooting-images/untrusted02.png#lightbox)
3. Klicken Sie unter der **Geräte** side Bereich, wählen Sie Ihr Gerät, mit der rechten Maustaste und wählen Sie **Provisioning Profile anzeigen...** : 

    [![](troubleshooting-images/untrusted03.png "SShow Provisioning Profile")](troubleshooting-images/untrusted03.png#lightbox)
4. Wählen Sie jede Bereitstellungsprofil derzeit auf dem Gerät und auf die **-** Schaltfläche zu löschen: 

    [![](troubleshooting-images/untrusted04.png "Löschen ein Bereitstellungsprofil")](troubleshooting-images/untrusted04.png#lightbox)
5. Aus der **Xcode** klicken Sie im Menü **Einstellungen...**  und **Konten**: 

    [![](troubleshooting-images/untrusted05.png "Xcode-kontoeinstellungen")](troubleshooting-images/untrusted05.png#lightbox)
6. Klicken Sie auf die **Details anzeigen...**  und anschließend auf die **können Sie alle herunterladen** Schaltfläche: 

    [![](troubleshooting-images/untrusted06.png "Herunterladen von allen Profilen")](troubleshooting-images/untrusted06.png#lightbox)
7. Wenn die Liste aktualisiert wurde, klicken Sie auf die **Fertig** Schaltfläche aus, und schließen Sie das Fenster Voreinstellungen.
8. Entfernen Sie die vorhandene Version der Xamarin.iOS-app, die Sie versucht haben, von dem iOS-Gerät zu testen.
9. Zurück zu Visual Studio für Mac, führen Sie einen bereinigten Build, und versuchen Sie es, um die app auf dem Gerät erneut auszuführen.

Sie müssen möglicherweise beenden und starten Sie Visual Studio für Mac, bevor die neue Bereitstellung Profile von Xcode geladen angezeigt werden. Sie müssen möglicherweise auch entsprechend anpassen der **iOS Bundle Signing** Optionen für Ihre app Xamarin.iOS, um die neue Bereitstellung Profile auszuwählen.

## <a name="launch-screen-issues"></a>Starten des Bildschirm-Probleme

iOS 9 erzwingt jetzt die Anforderungen des Bildschirms starten, damit dasselbe Abbild starten nicht mehr wiederverwendet werden kann, um andere Schnittstelle Ausrichtungen unterstützen. Finden Sie in der Apple- [UILanchImage Verweis](https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW28) für Weitere Informationen.

Optional können Sie eine Storyboarddatei Ihrer app starten Bildschirm im Gegensatz zur Verwendung der eines Satzes von präsentieren **PNG** Bilddateien. Dies ist nun die bevorzugte Apple bis hin zum Präsentieren von Bildschirmen starten. Finden Sie in unserer [Einführung in Storyboards Unified](~/ios/user-interface/storyboards/unified-storyboards.md) Weitere Informationen.

Schließlich muss Ihrer app verwenden Sie eine Storyboarddatei für ihre Bildschirm starten und unterstützen alle vier Schnittstelle Ausrichtungen (Hochformat, nach unten zeigende Hochformat, Querformat links und Querformat rechts), für die Ausführung in einem Bereich Folie über oder im Modus für geteilte Ansicht berücksichtigt werden. Um weitere Informationen über die neuen Funktionen von Multitasking von iOS 9 zu erhalten, finden Sie unter unsere [Multitasking für iPad](~/ios/platform/multitasking.md) Handbuch.

## <a name="nsinternalinconsistencyexception-exception"></a>NSInternalInconsistencyException-Ausnahme

Beim Kompilieren und Ausführen einer vorhandenen Xamarin.iOS-app für iOS 9 erhalten Sie möglicherweise einen Fehler in der Form:

> Objective-C-Ausnahme ausgelöst wird.  Name: NSInternalInconsistencyException Ursache: Anwendungsfenster erwartungsgemäß ein Stamm-modellansichtcontroller am Ende der Anwendungsstart aufweisen.

Dies ist Fehler daran, dass app-Fenster erwartungsgemäß ein Stamm-View-Controller am Ende der Anwendungsstart aufweisen und Ihre vorhandene app nicht ausgelöst wird.

Es sind mindestens zwei mögliche problemumgehungen für dieses Problem:

1. Update-app für die Verwendung von Storyboarddatei anstelle von `xib` Dateien, um die Benutzeroberfläche zu definieren. Dieser erfordert sehr viel Zeit je nach Größe der Anwendung und Kenntnisse der iOS-Designer (oder Xcodes Benutzeroberflächen-Generator) verwenden, mit Layout Storyboards. Weitere Informationen finden Sie in unserer [Einführung in Storyboards Unified](~/ios/user-interface/storyboards/unified-storyboards.md) Dokumentation.
2. Setup `RootViewController` Eigenschaft von app-Fenster in `FinishedLaunching` Methode in `AppDelegate` Klasse, um auf eine View-Controller in Ihrem app-Benutzeroberfläche zu verweisen.

## <a name="when-to-initialize-views-and-view-controllers"></a>Die Initialize-Ansichten und View-Controller

Mit Xamarin.iOS ist es möglich, Sicht oder View Controller-Initialisierung innerhalb Konstruktoren stellen die aufgerufen werden, wenn etwas in verwaltetem Code verfügbar gemacht wird jedoch den iOS-Entwurf.

Im Allgemeinen muss nicht initialisiert werden, die Rückruf können Objective-C-Code aus dem Konstruktor, da Sie nicht sicher sein können beim aufgerufen wird. Die auch bedeutet, dass ein geeigneter (andere ".ctor") oder Aufrufe von außer Kraft setzen (wie Objective-C keine Ereignisse aufweist), in denen diese Initialisierung durchgeführt werden soll.



## <a name="related-links"></a>Verwandte Links

- [iOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [Was ist neu in iOS 9.0 verfügen](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Aktualisieren Ihre apps Xamarin.iOS auf iOS9 (video)](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
