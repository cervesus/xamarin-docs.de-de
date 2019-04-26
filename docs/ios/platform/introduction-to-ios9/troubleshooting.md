---
title: Xamarin.iOS 9 – Problembehandlung
description: Dieser Artikel enthält verschiedene Tipps für die Problembehandlung für das Arbeiten mit iOS 9 in Xamarin.iOS. Tipps behandelt XML-Analyse, Simulatoren, Layout, Einschränkungen, Netzwerkprobleme und vielen anderen Themen.
ms.prod: xamarin
ms.assetid: DCE83E36-CBD9-4D96-8E7F-384CB8A54563
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: f8fae79af654339b54a8df0d2ea32eef38f34adb
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61425221"
---
# <a name="xamarinios-9--troubleshooting"></a>Xamarin.iOS 9 – Problembehandlung

_Dieser Artikel enthält mehrere Problembehandlungstipps für das Arbeiten mit iOS 9 in Xamarin.iOS-apps._

## <a name="there-was-a-problem-parsing-the-xml"></a>Es wurde ein Problem beim Analysieren des XML-Codes

Xamarin.IOS-Designer unterstützt noch keine Xcode 7-Features. Storyboards können nicht im Designer geladen _"Gab es ein Problem beim Analysieren des XML-Codes"_ beim Versuch, neue iOS 9 (Xcode 7) zu verwenden wie z. B. StackView designerelementen.

iOS-Designer-Unterstützung für Xcode 7-Features ist für die bevorstehende Veröffentlichung des Zyklus 6-Feature vorgesehen. Die Vorschauversion des Zyklus 6 ist derzeit in der Alpha-Kanal verfügbar und bietet eingeschränkte Unterstützung für die neuen Xcode 7-Features.

Partielle problemumgehung für Visual Studio für Mac: Mit der rechten Maustaste in des Storyboards aus, und wählen Sie **Öffnen mit** > **Xcode Interface Builder**.

## <a name="where-are-the-ios-8-simulators"></a>Wo befinden sich die iOS 8-Simulatoren?

Wenn Sie Xcode 7 (oder höher) installiert haben, automatisch werden, alle die iOS 8-Simulatoren mit iOS 9-Simulatoren ersetzt, in der Standardeinstellung. Wenn Sie immer noch unter iOS 8 testen möchten, können Sie starten Sie Xcode, und klicken Sie dann herunterladen und installieren die iOS 8-Simulatoren.

Wählen Sie in Xcode die **Xcode** Menü dann **Einstellungen...**   >  **Downloads**:

[![](troubleshooting-images/ios8.png "iOS 8-Simulatoren Downloads")](troubleshooting-images/ios8.png#lightbox)

Klicken Sie auf die **überprüfen und installieren Sie jetzt** Schaltfläche, um die iOS 8-Simulatoren neu installieren.

## <a name="layout-constraint-with-leftright-attribute-errors"></a>Layout-Einschränkung mit Fehlern aufgrund links/rechts

In iOS 8 (und früher), Benutzeroberflächenelemente in Storyboards mithilfe eine Mischung aus beiden **rechts** & **Links** Attribute (`NSLayoutAttributeRight` & `NSLayoutAttributeLeft`) und  **Führende** & **nachgestellten** Attribute (`NSLayoutAttributeLeading` & `NSLayoutAttributeTrailing`) in das gleiche Layout.

Wenn das gleiche Storyboard im unter iOS 9 ausgeführt wird, führt dies zu einer Ausnahme in der folgenden Form:

> Terminating app due to uncaught exception 'NSInvalidArgumentException', reason: '*** +[NSLayoutConstraint constraintWithItem:attribute:relatedBy:toItem:attribute:multiplier:constant:]: Eine Einschränkung kann nicht zwischen einem führende/nachgestellte-Attribut und ein Attribut des Links bzw. rechts vorgenommen werden. Führende/nachgestellte für beide oder keines von beiden verwenden. "

iOS 9 erzwingt Layouts für die Verwendung **rechts** & **Links** _oder_ **führende**  &   **Nachfolgende** Attribute jedoch *nicht* beide. Um dieses Problem zu beheben, ändern Sie alle Layout-Einschränkungen, um die gleichen Attribute in Ihre Storyboarddatei zu verwenden.

Weitere Informationen finden Sie unter den [iOS 9-Einschränkungsfehler](https://stackoverflow.com/questions/32692841/ios-9-constraint-error) Diskussion auf Stack Overflow.

## <a name="error-itms-90535-unexpected-cfbundleexecutable-key"></a>ERROR ITMS-90535: Unerwarteter CFBundleExecutable-Schlüssel

Von einer app verwendet nach der Umstellung auf iOS 9 3.-Komponenten (insbesondere unserer vorhandenen Komponente von Google Maps), die kompiliert und ausgeführt wurde beim Versuch, den neuen Build in iTunes Connect, die Sie eine Fehlermeldung erhalten können, in der Form senden unter iOS 8 (oder früher):

> ERROR ITMS-90535: Unerwarteter CFBundleExecutable-Schlüssel. Das Paket unter "Payload/app-name.app/component.bundle" enthält keine Paket-exe-Datei...

Diese Probleme können in der Regel werden gelöst, indem das benannte Paket im Projekt suchen dann – genau wie die Fehlermeldung vorgeschlagen - bearbeitet die `Info.plist` , die sich in das Paket durch das Entfernen der `CFBundleExecutable` Schlüssel. Die `CFBundlePackageType` Schlüssel sollte festgelegt werden, um `BNDL` ebenfalls.

Nach diesen Änderungen durchführen, ist eine saubere, und das gesamte Projekt neu erstellen. Sie sollten in iTunes Connect ohne Probleme zu übermitteln, nach diesen Änderungen durchführen können.

Weitere Informationen finden Sie in diesem [Stack Overflow](https://stackoverflow.com/questions/32096130/unexpected-cfbundleexecutable-key) Diskussion.

## <a name="cfnetwork-sslhandshake-failed--9824-error"></a>CFNetwork SSLHandshake Fehler (-9824)

Wenn Sie versuchen, eine Verbindung mit dem Internet herstellen, entweder direkt oder über eine Webansicht in iOS 9, erhalten Sie möglicherweise einen Fehler in der Form:

```csharp
2015-09-04 14:38:05.757 FormsWebViewiOS[2553:30362] CFNetwork SSLHandshake failed (-9824)
2015-09-04 14:38:05.758 FormsWebViewiOS[2553:30363] NSURLSession/NSURLConnection HTTP load failed (kCFStreamErrorDomainSSL, -9824)
```

Oder in der Form:

```csharp
2015-09-04 14:39:17.881 FormsWebViewiOS[2568:30974] App Transport Security has blocked a cleartext HTTP (http://) resource load since it is insecure.
Temporary exceptions can be configured via your app's Info.plist file.
```

In iOS9 erzwingt App Transport Security (ATS) über sichere Verbindungen zwischen Ressourcen für Internet (z. B. die app Back-End-Server) und Ihrer app. Darüber hinaus ATS erfordert Kommunikation unter Verwendung der `HTTPS` -Protokoll und allgemeine Kommunikation zwischen forward Secrecy mit Version 1.2 von TLS verschlüsselt werden.

Da ATS, wird standardmäßig in apps aktiviert ist, die für iOS 9 und OS X 10.11 (El Capitan), alle Verbindungen mit `NSURLConnection`, `CFURL` oder `NSURLSession` ATS-sicherheitsanforderungen unterliegen. Wenn Ihre Verbindungen diese Anforderung nicht erfüllen, werden sie mit einer Ausnahme fehlschlagen.

Finden Sie unter der [Opting skalierten ATS](~/ios/app-fundamentals/ats.md) Teil unserer [App Transport Security](~/ios/app-fundamentals/ats.md) Anleitung finden Sie Informationen dazu, wie dieses Problem zu lösen.

## <a name="my-existing-apps-dont-run-on-ios-9"></a>Meine vorhandene apps nicht auf iOS 9 ausgeführt.

Finden Sie in unserer [Informationen zur Kompatibilität mit iOS 9](~/ios/platform/introduction-to-ios9/ios9.md) Anweisungen neu erstellen und erneute Bereitstellung Ihre vorhandenen apps auf iOS 9 ausgeführt.

<a name="UICollectionViewCell.ContentView-is-null-in-constructors" />

## <a name="uicollectionviewcellcontentview-is-null-in-constructors"></a>UICollectionViewCell.ContentView ist Null in Konstruktoren

**Reason:** In iOS 9 die `initWithFrame:` Konstruktor ist jetzt erforderlich ist, aufgrund der verhaltensänderungen in iOS 9 als die [UICollectionView Dokumentation Zustände](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionView_class/#//apple_ref/occ/instm/UICollectionView/dequeueReusableCellWithReuseIdentifier:forIndexPath). Wenn Sie eine Klasse für den angegebenen Bezeichner registriert, und eine neue Zelle erstellt werden muss, wird die Zelle nun durch den Aufruf initialisiert seine `initWithFrame:` Methode.

**Fix:** Hinzufügen der `initWithFrame:` Konstruktor wie folgt:

```csharp
[Export ("initWithFrame:")]
public YourCellClassName (CGRect frame) : base (frame)
{
    Initialize (); // refactor initialize code into a method
}
```

Verwandte Beispiele: [MotionGraph](https://github.com/xamarin/monotouch-samples/commit/3c1b7a4170c001e7290db9babb2b7a6dddeb8bcb), [TextKitDemo](https://github.com/xamarin/monotouch-samples/commit/23ea01b37326963b5ebf68bbcc1edd51c66a28d6)

<a name="UIView-fails-to-Init-with-Coder-when-Loading-a-View-from-a-Xib/Nib" />

## <a name="uiview-fails-to-init-with-coder-when-loading-a-view-from-a-xibnib"></a>UIView an Init mit Programmierer schlägt fehl, wenn es sich bei eine Ansicht aus einer Xib/Nib laden

**Reason:** Die `initWithCoder:` Konstruktor wird aufgerufen, wenn eine Sicht aus einer Interface Builder Xib-Datei zu laden. Wenn dieser Konstruktor nicht exportiert wird kann nicht in nicht verwalteten Code unserem verwalteten Version aufrufen. Zuvor (z. b. In iOS 8) das `IntPtr` Konstruktor wurde aufgerufen, um die Ansicht zu initialisieren.

**Fix:** Erstellen und Exportieren der `initWithCoder:` Konstruktor wie folgt:

```csharp
[Export ("initWithCoder:")]
public YourClassName (NSCoder coder) : base (coder)
{
    Initialize (); // refactor initialize code into a method
}
```

Beispiel: [Chat](https://github.com/xamarin/monotouch-samples/commit/7b81138d52e5f3f1aa3769fcb08f46122e9b6a88)

## <a name="dyld-message-no-cache-image-with-name"></a>Dyld-Meldung: Kein Cachebild mit dem Namen...

Sie können einen Absturz mit den folgenden Informationen im Anwendungsprotokoll auftreten:

```csharp
Dyld Error Message:
Dyld Message: no cach image with name (/System/Library/PrivateFrameworks/JavaScriptCore.framework/JavaScriptCore)
```

**Reason:** Dies ist ein Fehler in der Apple nativen Linker, der Fall, wenn sie ein privates Framework öffentlich machen (JavaScriptCore wurde in iOS 7, öffentlich vor, dass es sich um ein privates Framework war), und das Bereitstellungsziel der app für eine iOS-Version, wenn das Framework privat ist. In diesem Fall wird von Apple-Linker mit der privaten Version von Framework anstelle der öffentlichen Version verknüpfen.

**Fix:** Dies wird für iOS 9 behoben werden, aber es gibt eine einfache Lösung, die Sie selbst in der Zwischenzeit anwenden können: nur höhere iOS-Version als Ziel in Ihrem Projekt (Sie können versuchen, iOS 7 in diesem Fall). Andere Frameworks möglicherweise ähnliche Probleme aufweisen, z. B. das WebKit-Framework wurde in iOS 8 öffentlich (und, für iOS 7 zu diesem Fehler; daher sollten Sie als Ziel iOS 8 WebKit in Ihrer app verwenden).

## <a name="untrusted-enterprise-developer"></a>Nicht vertrauenswürdigen Unternehmensentwickler

Beim Versuch, den iOS 9-Version Ihrer Xamarin.iOS-app auf echte iOS-Hardware ausführen, erhalten Sie möglicherweise eine Meldung angezeigt, dass Ihr Developer-Konto auf dem Gerät nicht vertrauenswürdig eingestuft wurde hat. Zum Beispiel:

[![](troubleshooting-images/untrusted01.png "Warnung für nicht vertrauenswürdigen Unternehmensentwickler")](troubleshooting-images/untrusted01.png#lightbox)

Um dieses Problem zu beheben, führen Sie folgende Schritte aus:

1. Starten Sie Xcode (die neueste Betaversion) auf der Mac-Entwicklung
2. Wählen Sie **Geräte** aus der **Fenster** Menü, um das Geräte-Fenster zu öffnen: 

    [![](troubleshooting-images/untrusted02.png "Fenster \"Geräte\"")](troubleshooting-images/untrusted02.png#lightbox)
3. Unter den **Geräte** Seite Panel, wählen Sie Ihr Gerät, mit der rechten Maustaste und wählen Sie **Provisioning Profile anzeigen...** : 

    [![](troubleshooting-images/untrusted03.png "SShow-Bereitstellungsprofile")](troubleshooting-images/untrusted03.png#lightbox)
4. Wählen Sie jede Bereitstellungsprofil derzeit auf dem Gerät und klicken Sie auf die **-** Schaltfläche, um ihn zu löschen: 

    [![](troubleshooting-images/untrusted04.png "Löschen eines bereitstellungsprofils")](troubleshooting-images/untrusted04.png#lightbox)
5. Von der **Xcode** , wählen Sie im Menü **Einstellungen...**  und **Konten**: 

    [![](troubleshooting-images/untrusted05.png "Xcode-kontoeinstellungen")](troubleshooting-images/untrusted05.png#lightbox)
6. Klicken Sie auf die **Details anzeigen...**  Schaltfläche, und klicken Sie dann die **alle herunterladen** Schaltfläche: 

    [![](troubleshooting-images/untrusted06.png "Alle Profile herunterladen")](troubleshooting-images/untrusted06.png#lightbox)
7. Wenn die Liste aktualisiert wurde, klicken Sie auf die **Fertig** Schaltfläche, und schließen Sie das Fenster "Einstellungen".
8. Entfernen Sie die vorhandene Version von Xamarin.iOS-app, die Sie versucht haben, von dem iOS-Gerät zu testen.
9. Zurück zu Visual Studio für Mac, führen Sie einen bereinigten Build aus, und versuchen Sie es, um die app auf dem Gerät erneut auszuführen.

Sie müssen möglicherweise beenden und starten Sie Visual Studio für Mac neu, bevor die neue bereitstellungsprofile, die von Xcode geladen angezeigt werden. Sie müssen möglicherweise auch Anpassen der **iOS Bundle-Signierung** Optionen für Ihre Xamarin.iOS-app, wählen Sie die neue bereitstellungsprofile.

## <a name="launch-screen-issues"></a>Probleme mit einem starten

iOS 9 erzwingt jetzt die Startbildschirm-Anforderungen, sodass dieselbe startbilds, das nicht mehr wiederverwendet werden kann, um verschiedene schnittstellenausrichtungen zu unterstützen. Finden Sie unter Apple [UILanchImage Verweis](https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW28) für Weitere Informationen.

Optional können Sie eine Storyboard-Datei verwenden, zur Darstellung der Startbildschirm der app, im Gegensatz zur Verwendung der eines Satzes von **PNG** Bilddateien. Dies ist jetzt die bevorzugte Apple zu Startbildschirme vorhanden. Informieren Sie sich unsere [Einführung in Storyboards Unified](~/ios/user-interface/storyboards/unified-storyboards.md) Anleitung finden Sie weitere Informationen.

Schließlich muss Ihre app verwenden Sie eine Storyboard-Datei für die Startbildschirm und unterstützen alle vier schnittstellenausrichtungen (Hochformat, nach unten zeigende Hochformat, Querformat links und Querformat rechts), für die Ausführung in einem Panel Folie über oder im Modus für geteilte Ansicht berücksichtigt werden. Um mehr über die neuen Multitasking-Funktionen von iOS 9 zu suchen, finden Sie unserem [Multitasking für iPad](~/ios/platform/multitasking.md) Guide.

## <a name="nsinternalinconsistencyexception-exception"></a>NSInternalInconsistencyException-Ausnahme

Beim Kompilieren und Ausführen einer vorhandenen Xamarin.iOS-app für iOS 9 erhalten Sie möglicherweise einen Fehler in der Form:

> Objective-C-Ausnahme ausgelöst wird.  Name: NSInternalInconsistencyException Ursache: Anwendungsfenster werden erwartet, einem stammansichtscontroller am Ende der Anwendung starten

Dies ist die Fehler, da app Windows voraussichtlich eine Root View Controller am Ende der Anwendungsstart haben und Ihre vorhandene app nicht ausgelöst wird.

Es sind mindestens zwei mögliche problemumgehungen für dieses Problem:

1. Update-app mit Storyboard-Datei anstelle von `xib` Dateien in der Benutzeroberfläche zu definieren. Diese ist sehr viel Zeit je nach Größe Ihrer App und wissen, wie die iOS-Designer (oder ein Interface Builder von Xcode) verwenden zu Layout-Storyboards erforderlich. Weitere Informationen finden Sie unserem [Einführung in Storyboards Unified](~/ios/user-interface/storyboards/unified-storyboards.md) Dokumentation.
2. Setup `RootViewController` -Eigenschaft von app-Fenster in `FinishedLaunching` -Methode in der `AppDelegate` Klasse, um auf eine View-Controller in die UI Ihrer app zu verweisen.

## <a name="when-to-initialize-views-and-view-controllers"></a>Wann Initialize-Ansichten und Ansichtscontroller

Mit Xamarin.iOS ist es möglich, Sicht oder View Controller-Initialisierung innerhalb von Konstruktoren vorzunehmen, die aufgerufen werden, wenn iOS-Entwurf beeinträchtigt jedoch etwas in verwaltetem Code verfügbar gemacht wird.

Im Allgemeinen sollten Sie nicht initialisieren alle Elemente, die wieder aufrufen können Objective-C-Code aus dem Konstruktor, da Sie nicht sicher sein können beim aufgerufen wird. Die auch bedeutet, dass ein geeigneter (andere ".ctor") oder Aufrufe von außer Kraft setzen (wie Objective-C keine Ereignisse aufweist), in denen diese Initialisierung durchgeführt werden soll.

## <a name="related-links"></a>Verwandte Links

- [iOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [Was ist neu in iOS 9.0 verfügen](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Aktualisieren Ihre Xamarin.iOS-apps auf iOS9 (video)](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
