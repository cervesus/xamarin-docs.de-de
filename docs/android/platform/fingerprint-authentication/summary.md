---
title: 'Fingerabdruckauthentifizierung: Leitfaden'
ms.prod: xamarin
ms.assetid: B40332CC-8123-4150-B47E-996214388842
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: e955d4f96724bd5682e7d0e6db2c36fa1b7810f4
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027433"
---
# <a name="fingerprint-authentication-guidance"></a>Fingerabdruckauthentifizierung: Leitfaden

## <a name="fingerprint-authentication-guidance"></a>Fingerabdruckauthentifizierung: Leitfaden

Nachdem wir nun die Konzepte und APIs für die Fingerabdruckauthentifizierung von Android 6.0 kennengelernt haben, stellen wir einige allgemeine Ratschläge für die Verwendung der Fingerabdruck-APIs vor.

1. **Verwenden der Kompatibilitäts-APIs der Android-Unterstützungsbibliothek v4** &ndash; Dadurch wird der Anwendungscode vereinfacht, indem die API-Überprüfung aus dem Code entfernt wird und eine Anwendung die meisten möglichen Geräte als Ziele verwenden kann.
2. **Bereitstellen von Alternativen zur Fingerabdruckauthentifizierung** &ndash; Fingerabdruckauthentifizierung ist eine großartige und schnelle Möglichkeit für eine Anwendung, einen Benutzer zu authentifizieren. Es kann jedoch nicht davon ausgegangen werden, dass sie immer funktioniert oder verfügbar ist. Möglicherweise fällt der Fingerabdruckscanner aus, die Linse ist verschmutzt, der Benutzer hat das Gerät möglicherweise nicht für die Verwendung von Fingerabdruckauthentifizierung konfiguriert oder die Fingerabdrücke sind nicht mehr vorhanden. Es ist auch möglich, dass der Benutzer keine Fingerabdruckauthentifizierung mit Ihrer Anwendung verwenden möchte. Aus diesen Gründen sollte eine Android-Anwendung einen alternativen Authentifizierungsprozess bereitstellen, z. B. einen Benutzernamen und ein Kennwort.
3. **Verwenden des Fingerabdrucksymbols von Google** &ndash; Alle Anwendungen sollten dasselbe Fingerabdrucksymbol verwenden, das von Google bereitgestellt wird. Durch die Verwendung eines Standardsymbols können Android-Benutzer leicht erkennen, wo in Apps Fingerabdruckauthentifizierung verwendet wird: 
    
    ![Android-Fingerabdrucksymbol](summary-images/ic-fp-40px.png)
    
4. **Benachrichtigen des Benutzers** &ndash; Eine Anwendung sollte eine Benachrichtigung für den Benutzer anzeigen soll, dass der Fingerabdruckscanner aktiv ist und auf eine Berührung oder ein Wischen wartet. 

## <a name="summary"></a>Zusammenfassung

Fingerabdruckauthentifizierung ist eine hervorragend Möglichkeit, einer Xamarin.Android-Anwendung das schnelle Überprüfen von Benutzern zu ermöglichen, sodass Benutzer mit sensiblen Features wie z. B. In-App-Käufen leichter interagieren können. In diesem Leitfaden wurden die Konzepte und der Code erläutert, die erforderlich sind, um die Fingerabdruck-APIs von Android 6.0 in Ihre Xamarin.Android-Anwendung einzubinden.

Zuerst haben wir die Fingerabdruck-APIs selbst vorgestellt: `FingerprintManager` (und `FingerprintManagerCompat`). Wir haben untersucht, wie die abstrakte Klasse `FingerprintManager.AuthenticationCallbacks` von einer Anwendung erweitert und als Vermittler zwischen der Fingerabdruckhardware und der Anwendung selbst verwendet werden muss. Anschließend haben wir untersucht, wie die Integrität der Ergebnisse des Fingerabdruckscanners mithilfe eines Java-`Cipher`-Objekts überprüft wird. Schließlich haben wir uns noch ein wenig mit dem Testen befasst, indem wir beschrieben haben, wie ein Fingerabdruck auf einem Gerät registriert und mit **ADB** ein Fingerabdruck-Wischvorgang auf einem Emulator simuliert wird. 

Wenn noch nicht geschehen, sollten Sie sich das [Beispielprogramm](https://github.com/xamarin/monodroid-samples/tree/master/FingerprintGuide) ansehen, das diesen Leitfaden begleitet. Das [Fingerabdruck-Dialogfeldbeispiel](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-m-fingerprintdialog) wurde von aus Java nach Xamarin.Android portiert und bietet ein weiteres Beispiel für das Hinzufügen von Fingerabdruckauthentifizierung zu einer Android-Anwendung.

## <a name="related-links"></a>Verwandte Links

- [Beispiel-App für Fingerabdruckleitfaden](https://github.com/xamarin/monodroid-samples/tree/master/FingerprintGuide)
- [Beispiel für Fingerabdruck-Dialogfeld](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-m-fingerprintdialog)
- [Fingerabdrucksymbol](https://raw.githubusercontent.com/xamarin/monodroid-samples/master/FingerprintGuide/FingerprintSampleApp/Resources/drawable-hdpi/ic_fp_40px.png)
