---
title: "Anmelden ich kann nicht warum Xamarin in Visual Studio oder Visual Studio für Mac?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6EF2B553-5DF9-41CC-B68F-77A7F64572FA
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: b50969e4c1d75c0bd79c08223dd959241dcb229a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="why-cant-i-log-into-xamarin-in-visual-studio-or-visual-studio-for-mac"></a>Anmelden ich kann nicht warum Xamarin in Visual Studio oder Visual Studio für Mac?

> [!IMPORTANT]
> Dieses Handbuch gilt nicht für die meisten Benutzer von MSDN, da sie nicht erforderlich sind, besitzen oder Xamarin-Konten anmelden, es sei denn, die [Xamarin-Komponenten speichern](https://components.xamarin.com/) oder [Visual Studio für Mac (Mac)](~/cross-platform/get-started/requirements.md). MSDN-Lizenz Inhaber sollten finden in diesem [-Lizenzierungsoptionen geführt](~/cross-platform/get-started/requirements.md) stattdessen.



## <a name="overview"></a>Übersicht
Es gibt einige häufige Ursachen, die Sie bei Ihrer Xamarin-Konto in der IDE verhindern können. Bekannte Probleme und Updates werden nachfolgend beschrieben.

### <a name="finding-the-login-screen"></a>Suchen den Anmeldebildschirm

Zu Referenzzwecken werden die Bildschirme Anmeldung finden Sie hier:

- Visual Studio für Mac
   - Oben rechts im Willkommensbildschirm
   - **Visual Studio für Mac > Konto** (Mac)
   - **Extras > Konto** (Windows)
- Visual Studio
   - **Extras > Xamarin-Konto**

## <a name="the-ide-is-connecting-but-the-account-screen-isnt-showing-correct-login-information"></a>Die IDE stellt eine Verbindung her, aber die Konto-Bildschirm nicht die richtigen Anmeldeinformationen angezeigt

Dieses Problem wird in der Regel durch eine manuelle Lizenz neusynchronisierung aufgelöst.
Folgen Sie den Anweisungen in diesem Artikel: [wie kann ich manuell Resyncronize Xamarin-Lizenzen?](~/cross-platform/troubleshooting/legacy-licenses/resync-licenses.md)

## <a name="invalid-account-information"></a>Ungültige Kontoinformationen

Wenn Sie mit der Xamarin-Website fort [Anmeldeseite](https://store.xamarin.com/Login?from=%2faccount%2f), Sie können die Protokollierung versuchen, sich mit Ihren Anmeldeinformationen des aktuellen Kontos.
Die Seite enthält außerdem Links zum Zurücksetzen Ihres Kennworts und ein neues Konto zu erstellen.

## <a name="account-is-valid-but-the-ide-cant-connect"></a>Konto ist gültig, aber die IDE kann keine Verbindung herstellen.

Dies ist normalerweise darauf zurückzuführen Firewall oder andere Sicherheitseinstellungen die IDE aus den Zugriff auf die erforderlichen Endpunkte blockieren.
Die Aktivierung Server müssen auf Folgendes zugreifen:

> Activation.xamarin.com store.xamarin.com auth.xamarin.com

Mehrere andere Endpunkte sind jedoch wichtig für allgemeine Entwicklung-Prozessen, z. B. Updates, die NuGet-Pakete, usw. abrufen. Aus diesem Grund ist es wird empfohlen, stellen Sie sicher, dass *alle* Endpunkte hinzufügen aus der [Xamarin-Firewall für die Basiskonfiguration](~/cross-platform/get-started/installation/firewall.md).

### <a name="ios-in-xamarin-studio-windows"></a>iOS in Xamarin Studio-Fenstern
iOS-Entwicklung wird in Xamarin Studio für Windows nicht unterstützt. Beim Zugriff auf den Anmeldebildschirm Hier werden die iOS-Lizenz nicht angezeigt.

Stattdessen, melden Sie sich über Xamarin Studio (Mac) oder Visual Studio mit einer älteren Lizenz. Beachten Sie, dass MSDN-Benutzer unter Windows nicht benötigen, um sich anzumelden.

## <a name="additional-support"></a>Zusätzliche Unterstützung

Wenn die oben genannten Szenarien Ihre Situation beschreiben nicht oder das Problem zu beheben, finden Sie in diesen [Supportoptionen](https://www.xamarin.com/support).
