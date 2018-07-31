---
title: iOS 9 Kompatibilität
description: Auch wenn Sie nicht sofort iOS 9-Funktionen zu Ihrer app hinzufügen möchten, sollten Sie Ihre apps mit der neuesten Version von Xamarin neu erstellen.
ms.prod: xamarin
ms.assetid: 69A05B0E-8A0A-489F-8165-B10AC46FAF3C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 2f22fdeaad1b276bb94d2b1ee5af4a6c24d22cb7
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350780"
---
# <a name="ios-9-compatibility"></a>iOS 9 Kompatibilität

_Auch wenn Sie nicht sofort iOS 9-Funktionen zu Ihrer app hinzufügen möchten, sollten Sie Ihre apps mit der neuesten Version von Xamarin neu erstellen._

> [!IMPORTANT]
> Die Informationen auf dieser Seite sind für Kunden mit bereits in der App-Store-apps für iOS 8 oder früheren Versionen, die wurden nicht bereits übermittelt Updates für iOS 9-Kompatibilität. Wenn Sie bereits, die neuesten Versionen verwenden - Xcode 7 und 9 mit Xamarin.iOS – für die app-Entwicklung finden Sie unter den [Einführung in iOS 9](~/ios/platform/introduction-to-ios9/index.md).

Wenn die erste iOS 9-Betaversionen angezeigt wurde, stellten wir zwei Probleme mit älteren Versionen von Xamarin, der als ältere apps kann nicht gestartet werden auf iOS 9 wird festgelegt.

Diese beiden Probleme (als [in unseren Foren detaillierte](http://forums.xamarin.com/discussion/comment/131529/#Comment_131529)) wurden:

- Erstellen von Apps für iOS 8 oder früher nicht in der Lage, auf 32-Bit-Geräten zu starten (einschließlich apps, die mit der [Unified API](~/cross-platform/macios/unified/index.md)).
- P/Invoke-Fehler durch den vollständigen Pfad wurde nicht angegeben.

Aktualisieren die Installation von Xamarin auf die neuesten stabilen Kanal-Version, und klicken Sie dann neu erstellen und erneute Bereitstellung Ihrer apps, werden diese beiden Probleme behebt.

_Auch wenn Sie nicht beabsichtigen, Ihrer app mit iOS 9-Features sofort aktualisiert werden, wir empfehlen Ihnen erneut erstellen, mit der neuesten Version von Xamarin und erneut an den App Store übermitteln_.



Dadurch wird sichergestellt, dass Ihre app auf iOS 9 ausgeführt wird, nach dem upgrade Ihrer Kunden.
Sie können weiterhin iOS 8 - Unterstützung neu erstellen, mit der neuesten Version wirkt sich nicht auf die Zielversion der Anwendung.

Wenn beim Testen Ihre vorhandenen apps auf iOS 9 Weitere Probleme auftreten, lesen Sie die [Verbessern der Kompatibilität](#compat) Abschnitt weiter unten.


### <a name="updating-with-visual-studio"></a>Aktualisieren mit Visual Studio

Es wird empfohlen, dass Sie explizit überprüfen Sie, dass Visual Studio auf die neueste stabile Version aktualisiert wird.

## <a name="what-about-components-nugets-and-other-libraries"></a>Wie sieht es Komponenten, NuGet-Pakete und andere Bibliotheken?

Sie **nicht** müssen warten, bis neue Versionen der Komponenten oder NuGet-Pakete Sie verwenden, um die beiden oben genannten Probleme zu behandeln.
Diese Probleme wurden behoben, indem Sie einfach neu erstellen, Ihre app mit der neuesten stabilen Version von Xamarin.iOS.

In ähnlicher Weise Komponentenlieferanten und Nuget-Autoren sind **nicht** erforderlich, um das Übermitteln von neuen Builds, um die oben genannten beiden Probleme zu beheben. Jedoch ggf. eine Komponente oder den Nuget verwendet `UICollectionView` oder Laden Sie die Sichten aus **Xib** -Dateien, ein Update *möglicherweise* erforderlich sein, die unten genannten iOS 9-Kompatibilitätsproblemen beschäftigen.


<a name="compat" />

## <a name="improving-compatibility-in-your-code"></a>Verbessern der Kompatibilität in Ihrem Code

Es gibt einige Fälle des Codes Muster *verwendet* funktioniert in älteren Versionen von iOS, die wichtige in iOS 9. Hier sind einige mögliche Probleme (und ihre Lösungen), die Fehler können auftreten, wenn Tests unter iOS 9:

### <a name="uicollectionviewcellcontentview-is-null-in-constructors"></a>UICollectionViewCell.ContentView ist null in Konstruktoren

**Ursache:** In iOS 9 die `initWithFrame:` Konstruktor ist jetzt erforderlich ist, aufgrund der verhaltensänderungen in iOS 9 als die [UICollectionView Dokumentation Zustände](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionView_class/#//apple_ref/occ/instm/UICollectionView/dequeueReusableCellWithReuseIdentifier:forIndexPath). Wenn Sie eine Klasse für den angegebenen Bezeichner registriert, und eine neue Zelle erstellt werden muss, wird die Zelle nun durch den Aufruf initialisiert seine `initWithFrame:` Methode.

**Fix:** Hinzufügen der `initWithFrame:` Konstruktor wie folgt:

```csharp
[Export ("initWithFrame:")]
public YourCellClassName (CGRect frame) : base (frame)
{
    Initialize (); // refactor initialize code into a method
}
```

Beziehen Sie Beispiele: [MotionGraph](https://github.com/xamarin/monotouch-samples/commit/3c1b7a4170c001e7290db9babb2b7a6dddeb8bcb), [TextKitDemo](https://github.com/xamarin/monotouch-samples/commit/23ea01b37326963b5ebf68bbcc1edd51c66a28d6)



### <a name="uiview-fails-to-init-with-coder-when-loading-a-view-from-a-xibnib"></a>UIView an Init mit Programmierer schlägt fehl, wenn es sich bei eine Ansicht aus einer Xib/Nib laden

**Ursache:** der `initWithCoder:` Konstruktor wird aufgerufen, wenn eine Sicht aus einer Interface Builder Xib-Datei zu laden. Wenn dieser Konstruktor nicht exportiert wird kann nicht in nicht verwalteten Code unserem verwalteten Version aufrufen. Zuvor (z. b. In iOS 8) das `IntPtr` Konstruktor wurde aufgerufen, um die Ansicht zu initialisieren.

**Fix:** erstellen und Exportieren der `initWithCoder:` Konstruktor wie folgt:

```csharp
[Export ("initWithCoder:")]
public YourClassName (NSCoder coder) : base (coder)
{
    Initialize (); // refactor initialize code into a method
}
```

Beispiel: [Chat](https://github.com/xamarin/monotouch-samples/commit/7b81138d52e5f3f1aa3769fcb08f46122e9b6a88)


### <a name="dyld-message-no-cache-image-with-name"></a>Dyld-Meldung: kein Cache-Image mit dem Namen...

Sie können einen Absturz mit den folgenden Informationen im Anwendungsprotokoll auftreten:

```csharp
Dyld Error Message:
Dyld Message: no cache image with name (/System/Library/PrivateFrameworks/JavaScriptCore.framework/JavaScriptCore)
```

**Ursache:** Dies ist ein Fehler in der Apple nativen Linker, der Fall, wenn sie ein privates Framework öffentlich machen (JavaScriptCore wurde in iOS 7, öffentlich vor, dass es sich um ein privates Framework war), und das Bereitstellungsziel der app für eine iOS-Version bei der Framework ist privat. In diesem Fall wird von Apple-Linker mit der privaten Version von Framework anstelle der öffentlichen Version verknüpfen.

**Fix:** Dies wird für iOS 9 behoben werden, aber es gibt eine einfache Lösung, die Sie selbst in der Zwischenzeit anwenden können: nur höhere iOS-Version als Ziel in Ihrem Projekt (Sie können versuchen, iOS 7 in diesem Fall). Andere Frameworks möglicherweise ähnliche Probleme aufweisen, z. B. das WebKit-Framework wurde in iOS 8 öffentlich (und, für iOS 7 zu diesem Fehler; daher sollten Sie als Ziel iOS 8 WebKit in Ihrer app verwenden).



## <a name="related-links"></a>Verwandte Links

- [iOS 9-Kompatibilität Release-Informationen](https://releases.xamarin.com/ios-hotfix-for-ios-9-preview-xcode-6/)
- [Einführung in iOS 9](~/ios/platform/introduction-to-ios9/index.md)
- [Aktualisieren Ihre Xamarin.iOS-apps auf iOS9 (video)](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
