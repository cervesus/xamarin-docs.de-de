---
title: Ich kann eine ältere Version von Xcode- oder Xamarin.iOS verwenden
description: Dieses Handbuch beschreibt die Probleme mit älteren Versionen von Xamarin.iOS oder Xcode (als die aktuelle stabile Version).
ms.prod: xamarin
ms.assetid: 27CF7EB7-9251-435F-BEA5-F20F8DD7DC17
ms.technology: xamarin-ios
author: chamons
ms.author: chhamo
ms.date: 04/16/2019
ms.openlocfilehash: 2a208d39454a33adc849bcccc66802361693e82e
ms.sourcegitcommit: 34819671c7910d29f018bdb394ddd4a4b0cd3a31
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59675888"
---
# <a name="can-i-use-an-older-version-of-xcode-or-xamarinios"></a>Kann ich eine ältere Version von Xcode- oder Xamarin.iOS verwenden?

Xamarin-Dokumentation wird davon ausgegangen, die Verwendung des neuesten Xamarin.iOS und Xcode, was empfohlen wird. Allerdings einige Kunden ältere Xamarin.iOS und/oder Xcode verwenden möchten, und möchten, dass Informationen über die folgen.

Die Versionshinweise enthalten die folgende Warnung:

> [!WARNING]
> **Verwenden eine ältere Version von Xcode**
>
> Mit einer älteren Version von Xcode (als die oben genannten [Anforderungen](https://docs.microsoft.com/xamarin/ios/release-notes/12/12.8#requirements)) ist oft möglich, aber einige Features möglicherweise nicht zur Verfügung. Auch einige Einschränkungen problemumgehungen, z. B. möglicherweise:
>
> - Die statische Registrierungsstelle erfordert Xcode-Header-Dateien zum Erstellen von Anwendungen, was zu [ `MT0091` ](https://docs.microsoft.com/xamarin/ios/troubleshooting/mtouch-errors#MT0091) oder [ `MT4109` ](https://docs.microsoft.com/xamarin/ios/troubleshooting/mtouch-errors#MT4109) Fehler, wenn APIs nicht vorhanden sind. In den meisten Fällen helfen aktivieren Sie die verwaltete (durch Entfernen der API).
> - Bitcode-Builds (für TvOS und WatchOS) können an den App Store übermittelt fehlschlagen, es sei denn, eine toolkette Xcode 9.0 und höher verwendet wird.

## <a name="further-information"></a>Weitere Informationen

Microsoft empfiehlt dringend, mit dem aktuellen Xcode und die neuesten Xamarin.iOS-Version, die beim Entwickeln und Übermitteln von Anwendungen. Apple erfordert, verwenden die neueste Xcode beim Übermitteln von Anwendungen.

Beachten Sie, dass Ihre Anwendung für ältere Versionen von iOS mit den neuesten Xcode nicht verhindert wird. Die iOS-Versionen, die Sie unterstützen, basiert Ihre **"Info.plist"** Eintrag und die APIs, die Ihre Anwendung verwendet.

Es ist möglich, installieren mehrere Versionen von Xcode Seite-an-Seite, z. B. mit unterschiedlichen Namen **Xcode101.app** und **Xcode102.app**. Wenn Sie mehrere Versionen verwenden, stellen Sie sicher, zum Festlegen von aktiven Xcode im [Visual Studio für Mac Einstellungen](~/ios/troubleshooting/questions/ios-sdk.md) und mit der [ `xcode-select` ](https://developer.apple.com/library/archive/technotes/tn2339/_index.html#//apple_ref/doc/uid/DTS40014588-CH1-HOW_DO_I_SELECT_THE_DEFAULT_VERSION_OF_XCODE_TO_USE_FOR_MY_COMMAND_LINE_TOOLS_) [Befehlszeilentool](https://developer.apple.com/library/archive/technotes/tn2339/_index.html#//apple_ref/doc/uid/DTS40014588-CH1-HOW_DO_I_SELECT_THE_DEFAULT_VERSION_OF_XCODE_TO_USE_FOR_MY_COMMAND_LINE_TOOLS_).

Seltene Fällen erfordern jedoch möglicherweise die Verwendung älterer Komponenten. In dieser Dokumentation wird beschrieben, die allgemeine Herausforderungen, die Sie möglicherweise stoßen, wenn älter als die neuesten Versionen verwenden.

Jede Version von Apple ist jedoch eindeutig, und möglicherweise andere Probleme, die hier nicht dokumentiert werden.

Diese Herausforderungen sind manchmal nicht trivialen lösen, also wenn möglich halten, auf die unterstützte Konfiguration der aktuellen Xcode und neuesten Xamarin.iOS.

## <a name="use-of-an-old-xamarinios-with-an-old-xcode"></a>Verwenden von einer alten Xamarin.iOS mit einer alten Xcode

Wird nicht aktualisiert, Xamarin.iOS und Xcode ist möglich, zumindest für eine bestimmte Zeit. Der Grenzwert ist, dass an einem bestimmten Punkt Apple eine Mindestversion von Xcode zum Übermitteln von Anwendungen benötigen. An diesem Punkt sollten Sie alle Komponenten (MacOS, Xcode und Xamarin.iOS) auf die neuesten Versionen (oder die neue, minimale Version von Xcode Apple und die entsprechende Version von Xamarin.iOS erforderlich ist) aktualisieren.

Es ist im Allgemeinen einfacher schrittweise aktualisieren und mit den Änderungen klein zu halten. Für große Projekte in der Updates mit halten schwieriger sein kann, kann das dranbleiben über bekannte Arbeitssatz einen guten Kompromiss sein.

## <a name="use-of-new-xamarinios-with-older-xcode"></a>Verwendung der neuen Xamarin.iOS mit älteren Xcode

Xamarin.iOS unterstützt im Allgemeinen ältere Xcode-Versionen, wenn irgend möglich. Einige mögliche Herausforderungen gehören:

- Die neuere Xamarin.iOS unterstützen möglicherweise einige Funktionen und APIs nicht vorhanden, in der ausgewählten Xcode. 
- Die **statische Registrierungsstelle** erfordert Xcode-Header-Dateien zum Erstellen von Anwendungen, was zu [ `MT0091` ](~/ios/troubleshooting/mtouch-errors.md#MT0091) oder [ `MT4109` ](~/ios/troubleshooting/mtouch-errors.md#MT4109)' Fehler, wenn APIs nicht vorhanden sind.
  - In den meisten Fällen aktivieren Sie die verwaltete können (durch Entfernen der verwalteten Bindungen für die neue API) bei Nichtverwendung.
- Bitcode-Builds (für TvOS und WatchOS) können an den App Store übermittelt fehlschlagen, es sei denn, eine toolkette Xcode 9.0 und höher verwendet wird.

## <a name="use-of-new-xcode-with-older-xamarinios"></a>Verwenden von neuen Xcode mit älteren Xamarin.iOS

In diesem Fall ist wesentlich schwieriger, wie Sie Xamarin.iOS ändernden Anforderungen der neuen Xcode nicht vorhergesagt werden kann. Updates von MacOS können auch zu Problemen führen, und ohne Kompatibilität Patches viele Teile von Xamarin.iOS beeinflusst werden können. 

Es gibt zahlreiche mögliche Bereiche, in denen etwas schief einschließlich gehen kann:

- Inkompatibilitäten mit `mlaunch`:
  - Simulator-Unterstützung funktioniert möglicherweise nicht ordnungsgemäß (oder überhaupt)
  - Unterstützung für Geräte funktionieren möglicherweise nicht ordnungsgemäß (oder überhaupt)
- Unbekannter Unterstützung für `mtouch` 
  - Keine Unterstützung für neue frameworks
  - Keine Unterstützung für neue Berechtigungen
  - Keine Unterstützung für neue oder aktualisierte tools
    - Dies kann auch zur codesignierung beeinträchtigen.

### <a name="new-appstore-submission-rules"></a>Neue Regeln für App Store-Übermittlung

Apple behält sich das Recht Updates für den App Store-Übermittlungen-Regeln zu einem beliebigen Zeitpunkt. Diese regeländerungen werden nur gelegentlich im Voraus angekündigt. Einige dieser Änderungen erfordern Tools Änderungen zur Unterstützung von, die eine aktualisierte Komponente für Xamarin.iOS erfordern würden.

Zusätzlich zu den regeländerungen Apple häufig übermittelte apps zusätzliche Überprüfungen hinzugefügt oder vorhandene verdichtet. Einige dieser erfordern Änderungen an unseren Tools (z. B. eine neue gesperrten Symbole). Viele dieser werden zuerst durch den Kunden gesendet werden, erkannt, da es keine Ankündigung (oder Liste) der Regeln gibt.

## <a name="summary"></a>Zusammenfassung

Nach Möglichkeit auf Nummer sicher gehen von folgenden Apple Anleitungen und entwickeln und übermitteln mit der neuesten Xcode auf dem App Store veröffentlicht.

Verwenden Sie die neueste Xamarin.iOS veröffentlicht. Dies wird die neuesten Fehlerbehebungen nachverfolgen, die wirken sich auf übermittelten Anwendungen und die Änderungen an den neuesten entsprechen.

Wenn dies nicht möglich ist, sollten erwägen Sie, eine übereinstimmenden ältere Xcode und Xamarin.iOS-Version zu verwenden. Dies kann arbeiten für einen Zeitraum, aber an einem bestimmten Punkt wird die Apple bei neueren Tools bestehen daher entsprechend planen.
