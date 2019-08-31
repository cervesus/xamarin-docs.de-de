---
title: Xamarin. IOS 9 – Problembehandlung
description: Dieser Artikel bietet verschiedene Tipps zur Problembehandlung bei der Arbeit mit IOS 9 in xamarin. IOS. Tipps beziehen sich auf XML-Parsing, Simulatoren, Layouteinschränkungen, Netzwerkprobleme und viele andere Themen.
ms.prod: xamarin
ms.assetid: DCE83E36-CBD9-4D96-8E7F-384CB8A54563
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: e6e264d9f1cd959c95a27597649d2ec23d832b1c
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2019
ms.locfileid: "70200067"
---
# <a name="xamarinios-9--troubleshooting"></a>Xamarin. IOS 9 – Problembehandlung

_Dieser Artikel enthält einige Tipps zur Problembehandlung bei der Arbeit mit IOS 9 in xamarin. IOS-apps._

## <a name="there-was-a-problem-parsing-the-xml"></a>Beim XML-Code ist ein Problem aufgetreten.

Der xamarin IOS-Designer unterstützt noch keine Xcode 7-Features. Storyboards können beim Versuch, neue IOS 9-Designer Elemente (Xcode 7) wie StackView zu verwenden, nicht in _den_ -Designer geladen werden.

Die IOS-Designer-Unterstützung für Xcode 7-Features ist für die bevorstehende Version von Cycle 6 vorgesehen. Die Vorschauversion von Cycle 6 ist derzeit im Alpha Kanal verfügbar und bietet eingeschränkte Unterstützung für die neuen Funktionen von Xcode 7.

Teil Problem Umgehung für Visual Studio für Mac: Klicken Sie mit der rechten Maustaste auf das Storyboard, und wählen Sie **mit** > **Xcode Interface Builder**öffnen.

## <a name="where-are-the-ios-8-simulators"></a>Wo sind die IOS 8-Simulatoren?

Wenn Sie Xcode 7 (oder höher) installiert haben, werden alle IOS 8-Simulatoren standardmäßig automatisch mit IOS 9-Simulatoren ersetzt. Wenn Sie weiterhin unter IOS 8 testen müssen, können Sie Xcode starten und dann die IOS 8-Simulatoren herunterladen und installieren.

Wählen Sie in Xcode das Menü **Xcode** und dann **Einstellungen...**  >  **Downloads**:

[![](troubleshooting-images/ios8.png "Downloads für IOS 8-Simulatoren")](troubleshooting-images/ios8.png#lightbox)

Klicken Sie auf die Schaltfläche **jetzt überprüfen und installieren** , um die IOS 8-Simulatoren erneut zu installieren.

## <a name="layout-constraint-with-leftright-attribute-errors"></a>Layouteinschränkung mit linken/rechten Attribut Fehlern

In ios 8 (und früheren Versionen) können Benutzeroberflächen Elemente in Storyboards eine Mischung aus beiden **Rechts** & **linken** Attributen (`NSLayoutAttributeRight` & `NSLayoutAttributeLeft`) und **führenden** & **nachfolg** enden Attributen verwenden ( )imgleichenLayout`NSLayoutAttributeLeading`.  &  `NSLayoutAttributeTrailing`

Wenn das gleiche Storyboard in ios 9 ausgeführt wird, führt dies zu einer Ausnahme in der folgenden Form:

> Das Beenden der APP aufgrund einer nicht abgefangenen Ausnahme "nsinvalidargumentexception", Ursache: "* * * + [nslayouteinschränkung einschränintwithitem: Attribute: relatedby: toitem: Attribute: Multiplikator: Konstante:]: Eine Einschränkung kann nicht zwischen einem führenden/nachfolgenden Attribut und einem right/left-Attribut vorgenommen werden. Verwenden Sie "Leading/Trailing" für beides oder keines von beiden.

IOS 9 erzwingt Layouts für die Verwendung von **Rechts** & **linken** _oder_ **führenden** & **nachfolg** enden Attributen, aber *nicht* für beides. Um dieses Problem zu beheben, ändern Sie alle Layouteinschränkungen so, dass der gleiche Attribut Satz in der storyboarddatei verwendet wird.

Weitere Informationen finden Sie unter [IOS 9](https://stackoverflow.com/questions/32692841/ios-9-constraint-error) -Einschränkungs Fehler Stack Overflow Erörterung.

## <a name="error-itms-90535-unexpected-cfbundleexecutable-key"></a>FEHLER-ITMS-90535: Unerwarteter cfbundleexe-Schlüssel.

Nach dem Wechsel zu IOS 9 verwendet eine APP von einer APP Drittanbieter Komponenten (insbesondere unsere vorhandene Google Maps-Komponente), die auf IOS 8 (oder früher) kompiliert und ausgeführt wurden. Wenn Sie versuchen, den neuen Build an iTunes Connect zu übermitteln, erhalten Sie einen Fehler in der folgenden Form:

> FEHLER-ITMS-90535: Unerwarteter cfbundleexe-Schlüssel. Das Bündel bei ' Payload/App-Name. app/Component. Bundle ' enthält keine ausführbare Paketdatei...

Diese Probleme können normalerweise gelöst werden `Info.plist` , indem Sie das benannte Bündel im Projekt suchen, und zwar genau wie in der Fehlermeldung vorgeschlagen wird, indem Sie den `CFBundleExecutable` Schlüssel entfernen. Der `CFBundlePackageType` Schlüssel sollte auch auf `BNDL` festgelegt werden.

Nachdem Sie diese Änderungen vorgenommen haben, müssen Sie das gesamte Projekt bereinigen und neu erstellen. Nachdem Sie diese Änderungen vorgenommen haben, sollten Sie ohne Problem an iTunes Connect übermitteln können.

Weitere Informationen finden Sie in dieser [Stack Overflow](https://stackoverflow.com/questions/32096130/unexpected-cfbundleexecutable-key) Erörterung.

## <a name="cfnetwork-sslhandshake-failed--9824-error"></a>Fehler beim CFNetwork-sslhandshake (-9824).

Wenn Sie versuchen, eine Verbindung mit dem Internet (entweder direkt oder über eine Webansicht in ios 9) herzustellen, erhalten Sie möglicherweise eine Fehlermeldung in der Form:

```csharp
2015-09-04 14:38:05.757 FormsWebViewiOS[2553:30362] CFNetwork SSLHandshake failed (-9824)
2015-09-04 14:38:05.758 FormsWebViewiOS[2553:30363] NSURLSession/NSURLConnection HTTP load failed (kCFStreamErrorDomainSSL, -9824)
```

Oder in der Form:

```csharp
2015-09-04 14:39:17.881 FormsWebViewiOS[2568:30974] App Transport Security has blocked a cleartext HTTP (http://) resource load since it is insecure.
Temporary exceptions can be configured via your app's Info.plist file.
```

In iOS9 erzwingt App-Transport Sicherheit (app Transport Security, ATS) sichere Verbindungen zwischen Internetressourcen (z. b. dem Back-End-Server der APP) und ihrer app. Darüber hinaus erfordert die Kommunikation mit dem `HTTPS` Protokoll und der übergeordneten API-Kommunikation, um mit der TLS-Version 1,2 mit vorwärts Geheimnisse verschlüsselt zu werden.

Da ATS in apps, die für IOS 9 und OS X 10,11 (El Capitan) erstellt werden, standardmäßig aktiviert ist `NSURLConnection`, `CFURL` gelten `NSURLSession` für alle Verbindungen, die verwenden, oder die Sicherheitsanforderungen. Wenn Ihre Verbindungen diese Anforderung nicht erfüllen, schlagen Sie mit einer Ausnahme fehl.

Informationen dazu, wie Sie dieses Problem beheben können, finden Sie im Abschnitt " [Ablehnen von ATS](~/ios/app-fundamentals/ats.md) " des [App-Transport-Sicherheits](~/ios/app-fundamentals/ats.md) Handbuchs.

## <a name="my-existing-apps-dont-run-on-ios-9"></a>Meine vorhandenen apps werden nicht unter IOS 9 ausgeführt

Anweisungen zum erneuten Erstellen und erneuten Bereitstellen vorhandener apps für die unter IOS 9 finden Sie in den Informationen zu den [IOS 9-Kompatibilitätsinformationen](~/ios/platform/introduction-to-ios9/ios9.md) .

<a name="UICollectionViewCell.ContentView-is-null-in-constructors" />

## <a name="uicollectionviewcellcontentview-is-null-in-constructors"></a>Uicollectionviewcell. contentview ist in Konstruktoren NULL.

**Weshalb** In ios 9 ist `initWithFrame:` der Konstruktor jetzt aufgrund von Verhaltensänderungen in ios 9 wie in den [uicollectionview-Dokumentations Zuständen](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionView_class/#//apple_ref/occ/instm/UICollectionView/dequeueReusableCellWithReuseIdentifier:forIndexPath)erforderlich. Wenn Sie eine Klasse für den angegebenen Bezeichner registriert haben und eine neue Zelle erstellt werden muss, wird die Zelle nun durch Aufrufen `initWithFrame:` der zugehörigen-Methode initialisiert.

**Zusetzen** Fügen Sie `initWithFrame:` den Konstruktor wie folgt hinzu:

```csharp
[Export ("initWithFrame:")]
public YourCellClassName (CGRect frame) : base (frame)
{
    Initialize (); // refactor initialize code into a method
}
```

Verwandte Beispiele: [MotionGraph](https://github.com/xamarin/monotouch-samples/commit/3c1b7a4170c001e7290db9babb2b7a6dddeb8bcb), [TextKitDemo](https://github.com/xamarin/monotouch-samples/commit/23ea01b37326963b5ebf68bbcc1edd51c66a28d6)

<a name="UIView-fails-to-Init-with-Coder-when-Loading-a-View-from-a-Xib/Nib" />

## <a name="uiview-fails-to-init-with-coder-when-loading-a-view-from-a-xibnib"></a>UIView kann beim Laden einer Ansicht aus einem XIb/NIB nicht mit dem Coder Initialisierung

**Weshalb** Der `initWithCoder:` Konstruktor wird aufgerufen, wenn eine Ansicht aus einer Interface Builder XIb-Datei geladen wird. Wenn dieser Konstruktor nicht exportiert wird, kann nicht verwalteter Code unsere verwaltete Version von ihm nicht abrufen. Früher (z. b. in ios 8) wurde `IntPtr` der Konstruktor aufgerufen, um die Ansicht zu initialisieren.

**Zusetzen** Erstellen und exportieren Sie `initWithCoder:` den Konstruktor wie folgt:

```csharp
[Export ("initWithCoder:")]
public YourClassName (NSCoder coder) : base (coder)
{
    Initialize (); // refactor initialize code into a method
}
```

Verwandte Stichprobe: [Video](https://github.com/xamarin/monotouch-samples/commit/7b81138d52e5f3f1aa3769fcb08f46122e9b6a88)

## <a name="dyld-message-no-cache-image-with-name"></a>Dyld-Meldung: Kein Cache Image mit dem Namen...

Möglicherweise kommt es zu einem Absturz mit den folgenden Informationen im Protokoll:

```csharp
Dyld Error Message:
Dyld Message: no cach image with name (/System/Library/PrivateFrameworks/JavaScriptCore.framework/JavaScriptCore)
```

**Weshalb** Dies ist ein Fehler im nativen Linker von Apple, der auftritt, wenn Sie ein privates Framework öffentlich machen (javascriptcore wurde in ios 7 öffentlich gemacht, bevor es ein privates Framework war), und das Bereitstellungs Ziel der APP für eine IOS-Version gilt, als das Framework Privat war. In diesem Fall wird der Linker von Apple mit der privaten Version des Frameworks anstelle der öffentlichen Version verknüpft.

**Zusetzen** Dies wird für IOS 9 behandelt, aber es gibt eine einfache Problem Umgehung, die Sie in der Zwischenzeit selbst anwenden können. (in diesem Fall können Sie IOS 7 testen.) Andere Frameworks weisen möglicherweise ähnliche Probleme auf, z. b. weil das WebKit-Framework in ios 8 öffentlich gemacht wurde (und das Ziel IOS 7 ist dieser Fehler. Sie sollten IOS 8 als Ziel verwenden, um webkit in Ihrer APP zu verwenden).

## <a name="untrusted-enterprise-developer"></a>Nicht vertrauenswürdiger Unternehmensentwickler

Wenn Sie versuchen, die IOS 9-Version der xamarin. IOS-App auf der realen IOS-Hardware auszuführen, erhalten Sie möglicherweise eine Meldung, die besagt, dass Ihr Entwicklerkonto auf dem Gerät nicht vertrauenswürdig ist. Beispiel:

[![](troubleshooting-images/untrusted01.png "Warnung zu nicht vertrauenswürdigem Unternehmensentwickler")](troubleshooting-images/untrusted01.png#lightbox)

Gehen Sie folgendermaßen vor, um dieses Problem zu beheben:

1. Starten Sie Xcode (die neueste Beta Version) auf dem Entwicklungs-Mac.
2. Wählen Sie im Menü **Fenster** die Option **Geräte** aus, um das Fenster Geräte zu öffnen: 

    [![](troubleshooting-images/untrusted02.png "Das Fenster \"Geräte\"")](troubleshooting-images/untrusted02.png#lightbox)
3. Wählen Sie unter dem Bereich **Geräte** das Gerät aus, klicken Sie mit der rechten Maustaste, und wählen Sie **Bereitstellungs Profile anzeigen...** aus: 

    [![](troubleshooting-images/untrusted03.png "SShow-Bereitstellungs profile")](troubleshooting-images/untrusted03.png#lightbox)
4. Wählen Sie alle Bereitstellungs Profile auf dem Gerät aus, und **-** klicken Sie auf die Schaltfläche, um Sie zu löschen: 

    [![](troubleshooting-images/untrusted04.png "Löschen eines Bereitstellungs Profils")](troubleshooting-images/untrusted04.png#lightbox)
5. Wählen Sie im Menü **Xcode** die Option **Einstellungen...** und **Konten**: 

    [![](troubleshooting-images/untrusted05.png "Xcode-Kontoeinstellungen")](troubleshooting-images/untrusted05.png#lightbox)
6. Klicken Sie auf die Schaltfläche **Details anzeigen...** und anschließend auf die Schaltfläche **alle herunterladen** : 

    [![](troubleshooting-images/untrusted06.png "Alle Profile herunterladen")](troubleshooting-images/untrusted06.png#lightbox)
7. Wenn die Aktualisierung der Liste abgeschlossen ist, klicken Sie auf die Schaltfläche **Fertig** , und schließen Sie das Fenster Einstellungen.
8. Entfernen Sie die vorhandene Version der xamarin. IOS-APP, die Sie auf dem IOS-Gerät testen möchten.
9. Kehren Sie zu Visual Studio für Mac zurück, führen Sie einen sauberen Build aus, und versuchen Sie, die APP auf dem Gerät erneut auszuführen.

Möglicherweise müssen Sie Visual Studio für Mac abbrechen und neu starten, bevor die von Xcode geladenen neuen Bereitstellungs Profile angezeigt werden. Möglicherweise müssen Sie auch die **IOS-Bündel Signierungs** Optionen für Ihre xamarin. IOS-App anpassen, um die neuen Bereitstellungs Profile auszuwählen.

## <a name="launch-screen-issues"></a>Probleme beim Startbildschirm

IOS 9 erzwingt nun die Startbildschirm Anforderungen, damit das gleiche Start Abbild nicht mehr wieder verwendet werden kann, um unterschiedliche Schnittstellen Ausrichtungen zu unterstützen. Weitere Informationen finden Sie in der Referenz zu " [uilanchimage](https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW28) " von Apple.

Optional können Sie eine storyboarddatei verwenden, um den Startbildschirm Ihrer APP zu präsentieren, anstatt einen Satz von **PNG** -Bilddateien zu verwenden. Dies ist nun der bevorzugte Weg von Apple, Startbildschirme darzustellen. Weitere Informationen finden Sie im Leitfaden [Introduction to Unified Storyboards](~/ios/user-interface/storyboards/unified-storyboards.md) .

Schließlich muss Ihre APP eine storyboarddatei für Ihren Startbildschirm verwenden und alle vier Schnittstellen Ausrichtungen unterstützen (hoch-nach-unten-Hochformat, Querformat links und Querformat rechts), um die Ausführung in einer Folie über Panel oder im Split-View-Modus zu berücksichtigen. Weitere Informationen zu den neuen Multitasking-Funktionen von IOS 9 finden Sie in unserem Handbuch [zum Multitasking für iPad](~/ios/platform/multitasking.md) .

## <a name="nsinternalinconsistencyexception-exception"></a>NSInternalInconsistencyException-Ausnahme

Wenn Sie eine vorhandene xamarin. IOS-App für IOS 9 kompilieren und ausführen, erhalten Sie möglicherweise eine Fehlermeldung in der folgenden Form:

> Die ausgelöste Ziel-C-Ausnahme.  Name: NSInternalInconsistencyException Grund: Es wird erwartet, dass Anwendungsfenster am Ende des Anwendungs Starts einen Root View Controller haben.

Dieser Fehler wird ausgelöst, weil App-Fenster am Ende des Anwendungs Starts einen Root View Controller aufweisen und Ihre vorhandene APP nicht.

Es gibt mindestens zwei mögliche Problem Umgehungen für dieses Problem:

1. Aktualisieren Sie die APP, sodass Sie die storyboarddatei anstelle von `xib` Dateien verwendet, um die Benutzeroberfläche zu definieren. Dies erfordert sehr viel Zeit in Abhängigkeit von der Größe Ihrer APP und der Verwendung des IOS-Designers (oder des Interface Builder von Xcode) zum layoutstoryboards. Weitere Informationen finden [Sie in unserer Einführung in die Dokumentation zu Unified Storyboards](~/ios/user-interface/storyboards/unified-storyboards.md) .
2. Setup `RootViewController` Eigenschaft des App-Fensters `FinishedLaunching` in der `AppDelegate` -Methode in der-Klasse, die auf einen Ansichts Controller in der Benutzeroberfläche Ihrer APP verweist.

## <a name="when-to-initialize-views-and-view-controllers"></a>Initialisieren von Sichten und Anzeigen von Controllern

Mit xamarin. IOS ist es möglich, die Controller Initialisierung innerhalb von Konstruktoren anzuzeigen oder anzuzeigen, die aufgerufen werden, wenn etwas in verwaltetem Code verfügbar gemacht wird, aber das IOS-Design unterbricht.

Im Allgemeinen sollten Sie nichts initialisieren, das den Ziel-C-Code aus dem Konstruktor aufrufen kann, da Sie nicht sicher sein können, wann er aufgerufen wird. Dies bedeutet auch, dass es eine bessere Stelle (andere. ctor) oder Aufrufe der außer Kraft Setzung gibt (da "Ziel-C" keine Ereignisse aufweist), in denen diese Initialisierung durchgeführt werden sollte.

## <a name="related-links"></a>Verwandte Links

- [IOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [Neuerungen in ios 9,0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Aktualisieren Ihrer xamarin. IOS-apps auf iOS9 (Video)](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
