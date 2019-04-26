---
title: Fingerabdruck Authentifizierung Anleitungen
ms.prod: xamarin
ms.assetid: B40332CC-8123-4150-B47E-996214388842
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 535eabe07cb4f4d36e6a6f918b5717efcc99185d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61023537"
---
# <a name="fingerprint-authentication-guidance"></a>Fingerabdruck Authentifizierung Anleitungen

## <a name="fingerprint-authentication-guidance"></a>Fingerabdruck Authentifizierung Anleitungen

Nun, wir, dass die Konzepte gesehen haben und APIs im Zusammenhang mit Android 6.0-Authentifizierung Fingerabdruck, sehen wir uns einige allgemeine Ratschläge für die Verwendung der Fingerprint-APIs.

1. **Verwenden Sie die Bibliothek für Android-Unterstützung v4 Kompatibilität APIs** &ndash; dies den Anwendungscode zu vereinfachen, indem Sie das Kontrollkästchen für die API aus dem Code entfernen und ermöglichen einer Anwendung, die die meisten Geräte möglichen ausgerichtet wird.
2. **Bereitstellen von alternativen für die Authentifizierung per Fingerabdruck** &ndash; Authentifizierung per Fingerabdruck ist eine hervorragende, schnelle Möglichkeit für eine Anwendung einen Benutzer authentifizieren, aber es kann nicht werden vorausgesetzt, dass es immer funktioniert oder zur Verfügung stehen. Es ist möglich, dass des fingerabdruckscanners kann fehlschlagen, die Linse möglicherweise geändert werden, kann der Benutzer nicht das Gerät, damit die Authentifizierung per Fingerabdruck verwenden konfiguriert haben oder die Fingerabdrücke da gegangen. Es ist auch möglich, dass der Benutzer keine Authentifizierung per Fingerabdruck in Ihrer Anwendung verwenden möchten. Aus diesen Gründen sollten eine Android-Anwendung einen alternative Authentifizierung-Prozess, z. B. Benutzername und Kennwort bereitstellen.
3. **Verwenden Googles fingerabdrucksymbol** &ndash; alle Anwendungen sollten verwenden das gleiche fingerabdrucksymbol, das von Google bereitgestellt wird. Die Verwendung von Standardsymbol erleichtert es Android-Benutzer, zu erkennen, wo in der apps die Authentifizierung per Fingerabdruck verwendet wird: 
    
    ![Android fingerabdrucksymbol](summary-images/ic-fp-40px.png)
    
4. **Benutzer benachrichtigen,** &ndash; zeigt eine Anwendung sollte eine Art von Benachrichtigung an den Benutzer des fingerabdruckscanners aktiv ist, und Warten auf ein Touch- oder Streifen. 

## <a name="summary"></a>Zusammenfassung

Authentifizierung per Fingerabdruck ist eine hervorragende Möglichkeit, eine Xamarin.Android-Anwendung, um schnell zu überprüfen, Benutzer, sodass es für Benutzer leichter, die sensible Funktionen interagieren, wie z. B. in-app-Käufe ermöglichen. Dieser Leitfaden erläutert die Konzepte und Code, den Android 6.0-Fingerabdruck Integrieren der API in Ihre Xamarin.Android-Anwendung erforderlich ist.

Zuerst den Fingerabdruck, der API des selbst erläutert `FingerprintManager` (und `FingerprintManagerCompat`). Wir untersucht, wie die `FingerprintManager.AuthenticationCallbacks` abstrakte Klasse muss von einer Anwendung erweitert und als Vermittler zwischen der Fingerprint-Hardware und die Anwendung selbst verwendet werden. Dann untersuchten wir überprüfen, ob die Integrität der Fingerabdruck Scanner Ergebnisse mit einer Java `Cipher` Objekt. Schließlich haben Sie Recht etwas zum Testen von beschreibt, wie einen Fingerabdruck auf einem Gerät zu registrieren und Verwenden von **Adb** eine streifbewegung Fingerabdruck in einem Emulator zu simulieren. 

Wenn Sie nicht bereits geschehen, sollten Sie zunächst die [beispielanwendung](https://github.com/xamarin/monodroid-samples/tree/master/FingerprintGuide) , dieses Handbuch begleitet. Die [Fingerabdruck Dialog Sample](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog/) wurden aus Java in Xamarin.Android portiert und enthält ein weiteres Beispiel zum Hinzufügen der Authentifizierung per Fingerabdruck zu einer Android-Anwendung.



## <a name="related-links"></a>Verwandte Links

- [Fingerabdruck-Guide-Beispiel-App](https://github.com/xamarin/monodroid-samples/tree/master/FingerprintGuide)
- [Beispiel der Fingerprint-Dialogfeld](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog/)
- [Fingerabdrucksymbol](https://raw.githubusercontent.com/xamarin/monodroid-samples/master/FingerprintGuide/FingerprintSampleApp/Resources/drawable-hdpi/ic_fp_40px.png)
