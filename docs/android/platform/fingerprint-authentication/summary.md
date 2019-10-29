---
title: Leitfaden für Fingerabdruckauthentifizierung
ms.prod: xamarin
ms.assetid: B40332CC-8123-4150-B47E-996214388842
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: e955d4f96724bd5682e7d0e6db2c36fa1b7810f4
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027433"
---
# <a name="fingerprint-authentication-guidance"></a>Leitfaden für Fingerabdruckauthentifizierung

## <a name="fingerprint-authentication-guidance"></a>Leitfaden für Fingerabdruckauthentifizierung

Nachdem wir nun die Konzepte und APIs für die Fingerabdruckauthentifizierung von Android 6,0 kennengelernt haben, erörtern wir einige allgemeine Ratschläge für die Verwendung der Fingerabdruck-APIs.

1. **Verwenden der Android-Unterstützungs Bibliothek V4-Kompatibilitäts-APIs** &ndash; dadurch wird der Anwendungscode vereinfacht, indem die API-Prüfung aus dem Code entfernt und eine Anwendung auf die meisten Geräte abzielen kann.
2. **Stellen Sie Alternativen für die Fingerabdruckauthentifizierung bereit** &ndash; Fingerabdruckauthentifizierung ist eine großartige und schnelle Möglichkeit für eine Anwendung, einen Benutzer zu authentifizieren. es kann jedoch nicht davon ausgegangen werden, dass er immer funktioniert oder verfügbar ist. Möglicherweise schlägt der Fingerabdruckscanner fehl, der Fokus ist möglicherweise fehlerhaft, der Benutzer hat das Gerät möglicherweise nicht für die Verwendung der Fingerabdruckauthentifizierung konfiguriert, oder die Fingerabdrücke sind nicht mehr vorhanden. Es ist auch möglich, dass der Benutzer möglicherweise keine Fingerabdruckauthentifizierung mit Ihrer Anwendung verwendet. Aus diesen Gründen sollte eine Android-Anwendung einen alternativen Authentifizierungsprozess bereitstellen, z. b. Benutzername und Kennwort.
3. **Verwenden Sie das Fingerabdruck Symbol von Google** &ndash; alle Anwendungen dasselbe Fingerabdruck Symbol verwenden sollten, das von Google bereitgestellt wird. Durch die Verwendung eines Standard Symbols können Android-Benutzer leicht erkennen, wo in Apps die Fingerabdruckauthentifizierung verwendet wird: 
    
    ![Android-Fingerabdruck Symbol](summary-images/ic-fp-40px.png)
    
4. **Benachrichtigen Sie den Benutzer** &ndash; eine Anwendung eine Benachrichtigung für den Benutzer anzeigen soll, dass der Fingerabdruckscanner aktiv ist und auf einen touchabdruck oder Streifen wartet. 

## <a name="summary"></a>Zusammenfassung

Die Fingerabdruckauthentifizierung ist eine hervorragend Möglichkeit, eine xamarin. Android-Anwendung zu ermöglichen, um Benutzer schnell zu überprüfen, sodass Benutzer mit sensiblen Features wie z. b. in-App-Käufe leichter interagieren können. In diesem Handbuch wurden die Konzepte und der Code erläutert, die erforderlich sind, um die Android 6,0-Fingerabdruck-APIs in Ihre xamarin. Android-Anwendung einzubinden.

Zuerst haben wir die Fingerabdruck-API selbst, `FingerprintManager` (und `FingerprintManagerCompat`) erörtert. Wir haben untersucht, wie die `FingerprintManager.AuthenticationCallbacks` abstrakte Klasse von einer Anwendung erweitert und als Vermittler zwischen der Fingerabdruck Hardware und der Anwendung selbst verwendet werden muss. Anschließend wird erläutert, wie die Integrität der Fingerabdruckscanner mithilfe eines Java-`Cipher` Objekts überprüft wird. Schließlich haben wir uns beim Testen etwas mit der Vorgehensweise zum Registrieren eines Fingerabdrucks auf einem Gerät und dem Verwenden von ADB zum Simulieren eines Fingerabdrucks auf einem Emulator mit **ADB** bedendert. 

Wenn Sie dies nicht bereits getan haben, sollten Sie sich die [Beispielanwendung](https://github.com/xamarin/monodroid-samples/tree/master/FingerprintGuide) ansehen, die dieser Anleitung folgt. Das Beispiel für den [Fingerabdruck Dialogfeld](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-m-fingerprintdialog) wurde von Java zu xamarin. Android portiert und bietet ein weiteres Beispiel zum Hinzufügen der Fingerabdruckauthentifizierung zu einer Android-Anwendung.

## <a name="related-links"></a>Verwandte Links

- [Beispiel-App für Fingerabdruck Handbuch](https://github.com/xamarin/monodroid-samples/tree/master/FingerprintGuide)
- [Beispiel für Fingerabdruck](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-m-fingerprintdialog)
- [Fingerabdruck Symbol](https://raw.githubusercontent.com/xamarin/monodroid-samples/master/FingerprintGuide/FingerprintSampleApp/Resources/drawable-hdpi/ic_fp_40px.png)
