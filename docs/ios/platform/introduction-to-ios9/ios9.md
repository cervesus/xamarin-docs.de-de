---
title: iOS 9-Kompatibilität
description: Auch wenn Sie nicht beabsichtigen, IOS 9-Features direkt zu Ihrer APP hinzuzufügen, sollten Sie Ihre apps mit der neuesten Version von xamarin neu erstellen.
ms.prod: xamarin
ms.assetid: 69A05B0E-8A0A-489F-8165-B10AC46FAF3C
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/19/2017
ms.openlocfilehash: f46b60a0567a5486a5c22a6ff36561e976d07b47
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292905"
---
# <a name="ios-9-compatibility"></a>iOS 9-Kompatibilität

_Auch wenn Sie nicht beabsichtigen, IOS 9-Features direkt zu Ihrer APP hinzuzufügen, sollten Sie Ihre apps mit der neuesten Version von xamarin neu erstellen._

> [!IMPORTANT]
> Die Informationen auf dieser Seite gelten für Kunden mit bereits im App Store als Ziel für IOS 8 oder früher, die noch keine Updates für IOS 9-Kompatibilität übermittelt haben. Wenn Sie bereits die neuesten Versionen (Xcode 7 und xamarin. IOS 9) verwenden, besuchen Sie für Ihre APP-Entwicklung die [Einführung zu IOS 9](~/ios/platform/introduction-to-ios9/index.md).

Als die ersten IOS 9-Beta Versionen aufgetreten sind, haben wir zwei Probleme mit älteren Versionen von xamarin festgestellt, die sich als ältere apps in ios 9 nicht starten können.

Diese beiden Probleme (wie [in unseren Foren ausführlich erläutert](http://forums.xamarin.com/discussion/comment/131529/#Comment_131529)) waren:

- Apps, die für IOS 8 oder früher erstellt wurden, können nicht auf 32-Bit-Geräten gestartet werden (einschließlich apps, die mit dem [Unified API](~/cross-platform/macios/unified/index.md)erstellt wurden).
- P/Aufruf fehlgeschlagen, da der vollständige Pfad nicht angegeben ist.

Wenn Sie Ihre xamarin-Installation auf die neueste stabile channelversion aktualisieren und dann Ihre apps neu erstellen und wieder bereitstellen, werden diese beiden Probleme behoben.

_Auch wenn Sie nicht beabsichtigen, Ihre APP mit den IOS 9-Features sofort zu aktualisieren, empfiehlt es sich, mit der aktuellen Version von xamarin neu zu erstellen und erneut an den App Store zu übermitteln_.



Dadurch wird sichergestellt, dass Ihre APP nach dem Upgrade der Kunden auf IOS 9 ausgeführt wird.
Sie können IOS 8 weiterhin unterstützen: die Neuerstellung mit der neuesten Version wirkt sich nicht auf die Anwendungs Zielversion aus.

Wenn Sie beim Testen vorhandener apps auf IOS 9 weitere Probleme haben, lesen Sie den Abschnitt [Verbesserung der Kompatibilität](#compat) weiter unten.


### <a name="updating-with-visual-studio"></a>Aktualisieren mit Visual Studio

Es wird empfohlen, explizit zu überprüfen, ob Visual Studio auf die neueste stabile Version aktualisiert wurde.

## <a name="what-about-components-nugets-and-other-libraries"></a>Wie sieht es mit Komponenten, nugets und anderen Bibliotheken aus?

Sie müssen **nicht** auf neue Versionen von Komponenten oder nugets warten, die Sie verwenden, um die beiden oben genannten Probleme zu beheben.
Diese Probleme wurden einfach behoben, indem Sie Ihre APP mit der neuesten stabilen Version von xamarin. IOS neu erstellen.

Ebenso sind Komponenten Anbieter und nuget-Autoren **nicht** verpflichtet, neue Builds zu übermitteln, um die beiden oben genannten Probleme zu beheben. Wenn jedoch eine Komponente oder nuget Ansichten aus `UICollectionView` **XIb** -Dateien verwendet oder lädt, ist *möglicherweise* ein Update erforderlich, um die unten erwähnten Kompatibilitätsprobleme mit IOS 9 zu beheben.


<a name="compat" />

## <a name="improving-compatibility-in-your-code"></a>Verbessern der Kompatibilität in Ihrem Code

Es gibt einige Fälle von Code Mustern, die *verwendet werden* , um in älteren Versionen von IOS-Unterbrechungen in ios 9 zu arbeiten. Im folgenden finden Sie einige mögliche Probleme (und ihre Lösungen), die beim Testen auf IOS 9 auftreten können:

### <a name="uicollectionviewcellcontentview-is-null-in-constructors"></a>Uicollectionviewcell. contentview ist in Konstruktoren NULL.

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



### <a name="uiview-fails-to-init-with-coder-when-loading-a-view-from-a-xibnib"></a>UIView kann beim Laden einer Ansicht aus einem XIb/NIB nicht mit dem Programmierer Initialisierung

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


### <a name="dyld-message-no-cache-image-with-name"></a>Dyld-Meldung: kein Cache Image mit dem Namen...

Möglicherweise kommt es zu einem Absturz mit den folgenden Informationen im Protokoll:

```csharp
Dyld Error Message:
Dyld Message: no cache image with name (/System/Library/PrivateFrameworks/JavaScriptCore.framework/JavaScriptCore)
```

**Weshalb** Dies ist ein Fehler im nativen Linker von Apple, der auftritt, wenn Sie ein privates Framework öffentlich machen (javascriptcore wurde in ios 7 öffentlich gemacht, bevor es ein privates Framework war), und das Bereitstellungs Ziel der APP für eine IOS-Version gilt, als das Framework Privat war. In diesem Fall wird der Linker von Apple mit der privaten Version des Frameworks anstelle der öffentlichen Version verknüpft.

**Zusetzen** Dies wird für IOS 9 behandelt, aber es gibt eine einfache Problem Umgehung, die Sie in der Zwischenzeit selbst anwenden können. (in diesem Fall können Sie IOS 7 testen.) Andere Frameworks weisen möglicherweise ähnliche Probleme auf, z. b. weil das WebKit-Framework in ios 8 öffentlich gemacht wurde (und das Ziel IOS 7 ist dieser Fehler. Sie sollten IOS 8 als Ziel verwenden, um webkit in Ihrer APP zu verwenden).



## <a name="related-links"></a>Verwandte Links

- [IOS 9-Kompatibilitäts Freigabe Informationen](https://releases.xamarin.com/ios-hotfix-for-ios-9-preview-xcode-6/)
- [Einführung in iOS 9](~/ios/platform/introduction-to-ios9/index.md)
- [Aktualisieren Ihrer xamarin. IOS-apps auf iOS9 (Video)](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
