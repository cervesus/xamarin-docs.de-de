---
title: Fingerabdruck Authentifizierung Anleitung
ms.prod: xamarin
ms.assetid: B40332CC-8123-4150-B47E-996214388842
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 2b66c3660f6d8af9217089a7615784957fcc6ed7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
ms.locfileid: "30763305"
---
# <a name="fingerprint-authentication-guidance"></a>Fingerabdruck Authentifizierung Anleitung

## <a name="fingerprint-authentication-guidance"></a>Fingerabdruck Authentifizierung Anleitung

Wir haben gesehen, dass die Konzepte und APIs umgebenden Android 6.0 Fingerabdruck-Authentifizierung, betrachten wir einige allgemeine Ratschläge für die Verwendung der APIs Fingerabdruck.

1. **Verwenden Sie die Android-Unterstützungsbibliothek v4 Kompatibilität APIs** &ndash; dies den Anwendungscode zu vereinfachen, indem Sie die Überprüfung der API aus dem Code entfernt und damit eine Anwendung die meisten Geräte möglichen abzielen.
2. **Bereitstellen von alternativen für Fingerabdruckauthentifizierung** &ndash; Fingerabdruckauthentifizierung ist eine hervorragende, schnelle Möglichkeit für eine Anwendung für einen Benutzer zu authentifizieren, jedoch davon kann nicht werden ausgegangen, dass es immer funktioniert oder verfügbar. Es ist möglich, dass der Fingerabdruck-Scanner kann fehlschlagen, Linse möglicherweise geändert werden, der Benutzer möglicherweise nicht, das Gerät konfiguriert, um den Fingerabdruckauthentifizierung verwenden oder die Fingerabdrücke haben zwischenzeitlich verloren gegangen. Es ist auch möglich, dass der Benutzer möglicherweise nicht zum Fingerabdruckauthentifizierung mit Ihrer Anwendung verwendet. Aus diesen Gründen sollten eine Android-Anwendung einen Alternative Authentifizierungsprozess, z. B. Benutzername und Kennwort angeben.
3. **Mit Google Symbol "Fingerabdruck"** &ndash; alle Anwendungen sollten verwenden das gleiche Fingerabdruck-Symbol, das von Google bereitgestellt wird. Die Verwendung eines standard-Symbols ganz einfach für Android-Benutzer zu erkennen, wo in apps Fingerabdruckauthentifizierung verwendet wird: 
    
    ![Symbol für Android-Fingerabdruck](summary-images/ic-fp-40px.png)
    
4. **Benutzer benachrichtigen,** &ndash; eine Anwendung sollte angezeigt werden eine Art von Benachrichtigung an den Benutzer, dass der Fingerabdruck Scanner aktiv ist und dem erwarten einer Touch oder Streifen. 

## <a name="summary"></a>Zusammenfassung

Fingerabdruckauthentifizierung ist eine hervorragende Möglichkeit, damit eine Anwendung Xamarin.Android schnell zu überprüfen, Benutzer, die für Benutzer für die Interaktion mit vertrauliche Features, z. B. in app-Käufe einfacher zu machen. Diese Anleitung erläutert die Konzepte und Code, der den Fingerabdruck Android 6.0 integriert API in Ihrer Anwendung Xamarin.Android der erforderlich ist.

Zuerst die API des selbst Fingerabdruck besprochen `FingerprintManager` (und `FingerprintManagerCompat`). Wir untersucht, wie die `FingerprintManager.AuthenticationCallbacks` abstrakte Klasse muss von einer Anwendung erweitert und als Mittler zwischen der Hardware Fingerabdruck und die Anwendung selbst verwendet werden. Dann untersucht überprüfen, ob die Integrität der Fingerabdruck Scanner Ergebnisse mit einem-Java `Cipher` Objekt. Schließlich wir berührt etwas beschreiben, wie einen Fingerabdruck auf einem Gerät zu registrieren und Verwenden von Tests **Adb** einen Fingerabdruck Wischen in einem Emulator simuliert. 

Wenn Sie dies nicht bereits getan haben Sie Hauptprotokoll der [beispielanwendung](https://github.com/xamarin/monodroid-samples/tree/master/FingerprintGuide) , die dieser Anleitung begleitet. Die [Fingerabdruck Dialog Sample](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog/) aus Java Xamarin.Android portiert wurde und ein weiteres Beispiel zum Hinzufügen von Fingerabdruckauthentifizierung zu einer Android-Anwendung enthält.



## <a name="related-links"></a>Verwandte Links

- [Fingerabdruck Handbuch Beispiel-App](https://github.com/xamarin/monodroid-samples/tree/master/FingerprintGuide)
- [Fingerabdruck Dialogfeld-Beispiel](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog/)
- [Symbol "Fingerabdruck"](https://developer.android.comhttps://developer.xamarin.com/samples/FingerprintDialog/res/drawable-hdpi/ic_fp_40px.html)
