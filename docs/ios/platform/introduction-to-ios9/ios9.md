---
title: "iOS 9 Kompatibilität"
description: "Auch wenn Sie nicht sofort iOS 9-Funktionen zu Ihrer app hinzufügen möchten, sollten Sie Ihre apps mit der neuesten Version von Xamarin neu erstellen."
ms.topic: article
ms.prod: xamarin
ms.assetid: 69A05B0E-8A0A-489F-8165-B10AC46FAF3C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: bfdf0c73226eec472eb671d5543f5ce124919ab8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="ios-9-compatibility"></a>iOS 9 Kompatibilität

_Auch wenn Sie nicht sofort iOS 9-Funktionen zu Ihrer app hinzufügen möchten, sollten Sie Ihre apps mit der neuesten Version von Xamarin neu erstellen._

> [!IMPORTANT]
> **Hinweis:** diese Informationen auf dieser Seite sind für Kunden mit bereits Zielplattform iOS 8 oder früher, App Store-apps, die Updates für iOS 9-Kompatibilität noch nicht gesendet haben. Wenn Sie bereits, die neuesten Versionen verwenden - Xcode 7 und Xamarin.iOS 9 – für app-Entwicklung finden Sie auf der [Einführung in iOS 9](~/ios/platform/introduction-to-ios9/index.md).

Beim ersten iOS 9-Betaversionen angezeigt wurde, identifiziert es zwei Probleme mit älteren Versionen von Xamarin, die als ältere apps wird nicht auf iOS 9 gestartet angezeigt.

Diese beiden Probleme (als [in unseren Foren detaillierte](http://forums.xamarin.com/discussion/comment/131529/#Comment_131529)) wurden:

- Erstellen von Apps für iOS 8 oder früher nicht auf 32-Bit-Geräten gestartet wird (z. B. apps, die mit der [einheitliche API](~/cross-platform/macios/unified/index.md)).
- P/Invoke durch den vollständigen Pfad fehlschlägt, ist nicht angegeben.

Aktualisieren die Xamarin-Installation auf die neuesten Channel stabile Version, und klicken Sie dann das Neuerstellen und erneutes Bereitstellen Ihrer apps diese beiden Probleme behoben werden.

_Auch wenn Sie planen werden nicht auf Ihre app mit iOS 9-Funktionen sofort zu aktualisieren, es wird empfohlen erneut erstellen, mit der neuesten Version von Xamarin und erneut auf den App Store übermitteln_.



Dadurch wird sichergestellt, dass Ihre app unter iOS 9 ausgeführt wird, nachdem Ihre Kunden aktualisieren.
Sie können weiterhin iOS 8 - Unterstützung mit der neuesten Version Neuerstellen wirkt sich nicht auf die Zielversion der Anwendung.

Wenn beim Testen Ihrer vorhandenen apps unter iOS 9 Weitere Probleme auftreten, lesen Sie die [Verbessern der Kompatibilität](#compat) Abschnitt weiter unten.


### <a name="updating-with-visual-studio"></a>Aktualisieren mit Visual Studio

Es wird empfohlen, dass Sie explizit überprüfen Sie, dass Visual Studio auf die neueste stabile Version aktualisiert wird.

## <a name="what-about-components-nugets-and-other-libraries"></a>Welche Rolle spielt Komponenten sowie Nugets und andere Bibliotheken?

Führen Sie **nicht** für neue Versionen von Komponenten oder Sie verwenden die beiden oben genannten Probleme Nugets warten zu müssen.
Diese Probleme werden behoben, indem Sie einfach erneut erstellen Ihrer app mit dem die neueste stabile Version von Xamarin.iOS.

Auf ähnliche Weise Komponente Lieferanten und NuGet-Autoren sind **nicht** erforderlich, um das Senden des neuen Builds aus, um die oben genannten beiden Probleme zu beheben. Jedoch ggf. eine Komponente oder ein NuGet-verwendet `UICollectionView` oder Laden Sie die Sichten aus **Xib** -Dateien, die ein Update *möglicherweise* erforderlich sein, um die unten aufgeführten iOS 9-Kompatibilitätsprobleme zu beheben.


<a name="compat" />

## <a name="improving-compatibility-in-your-code"></a>Verbessern der Kompatibilität in Ihrem Code

Besteht der wenige Fällen Codezeilen Muster, die *verwendet* in früheren Versionen von iOS in iOS 9 wichtige arbeiten. Hier sind einige mögliche Probleme (und ihre Lösungen), die beim Testen von IOS 9 auftreten:

### <a name="uicollectionviewcellcontentview-is-null-in-constructors"></a>UICollectionViewCell.ContentView is null in constructors

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



### <a name="uiview-fails-to-init-with-coder-when-loading-a-view-from-a-xibnib"></a>UIView kann Init mit Programmierer beim Laden von einer Sicht aus einer Xib/Nib

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


### <a name="dyld-message-no-cache-image-with-name"></a>Dyld Nachricht: keine Cache-Image mit dem Namen...

Sie können ein Absturz (Crash) mit den folgenden Informationen im Anwendungsprotokoll auftreten:

```csharp
Dyld Error Message:
Dyld Message: no cache image with name (/System/Library/PrivateFrameworks/JavaScriptCore.framework/JavaScriptCore)
```

**Ursache:** Dies ist ein Fehler in die systemeigene Apple-Linker, der Fall, wenn sie eine private Framework als öffentliches Profil festzulegen (JavaScriptCore erfolgte in iOS 7, öffentlich vor, dass es sich um eine private Framework war), und das Bereitstellungsziel der app für iOS-Version wird bei der Framework wurde die private. In diesem Fall wird der Apple-Linker mit der privaten Version des Frameworks statt die öffentliche Version verknüpft werden.

**Fix:** wird dies für iOS 9 adressiert werden, aber es ist eine einfache Lösung, die Sie selbst in der Zwischenzeit anwenden können: nur als Ziel eine höhere iOS Version in Ihrem Projekt (Sie können versuchen, iOS 7 in diesem Fall). Andere Frameworks möglicherweise ähnliche Probleme aufweisen, z. B. das WebKit-Framework wurde in iOS 8 öffentlich gemacht (und, Zielplattform iOS 7 zu diesem Fehler; daher sollten Sie als Ziel iOS 8 WebKit in Ihrer app verwenden).



## <a name="related-links"></a>Verwandte Links

- [iOS 9-Kompatibilität Version info](https://releases.xamarin.com/ios-hotfix-for-ios-9-preview-xcode-6/)
- [Einführung in iOS 9](~/ios/platform/introduction-to-ios9/index.md)
- [Aktualisieren Ihre apps Xamarin.iOS auf iOS9 (video)](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
