---
title: Veröffentlichen im App Store
description: Dieser Leitfaden führt Sie durch die Bereitstellung einer Xamarin.Mac-App mit Visual Studio für Mac. Darin wird beschrieben, wie Sie ein Mac-Entwicklerkonto einrichten, und Sie werden beim Erstellen der Zertifikate für die Codesignatur begleitet. Darüber hinaus wird gezeigt, wie Sie diese Zertifikate verwenden können, um Mac-Apps zu erstellen, die Sie direkt oder über den Mac App Store verteilen können.
ms.prod: xamarin
ms.assetid: D26C5E54-EAD2-5487-264D-4263AEA1EBF2
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 3b21dd0adfd96e1190660aa97b2850f968b5473f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="publishing-to-the-app-store"></a>Veröffentlichen im App Store

_Dieser Leitfaden führt Sie durch die Bereitstellung einer Xamarin.Mac-App mit Visual Studio für Mac. Darin wird beschrieben, wie Sie ein Mac-Entwicklerkonto einrichten, und Sie werden beim Erstellen der Zertifikate für die Codesignatur begleitet. Darüber hinaus wird gezeigt, wie Sie diese Zertifikate verwenden können, um Mac-Apps zu erstellen, die Sie direkt oder über den Mac App Store verteilen können._

## <a name="overview"></a>Übersicht

Xamarin.Mac-Apps können auf zwei unterschiedliche Arten verteilt werden:

- **Entwickler-ID**: Mit einer Entwickler-ID signierte Anwendungen können außerhalb des App Store verteilt werden. Sie werden jedoch von GateKeeper erkannt und sind für die Installation zulässig.
- **Mac App Store**: Für Apps ist ein Installer-Paket erforderlich. Zudem müssen App und Installer signiert sein, damit eine Übermittlung in den Mac App Store möglich ist.

In diesem Dokument wird erläutert, wie Sie mit Visual Studio für Mac und Xcode ein Apple-Entwicklerkonto einrichten und ein Xamarin.Mac-Projekt für jeden Bereitstellungstyp konfigurieren.


## <a name="mac-developer-program"></a>Mac-Entwicklerprogramm

Wenn Sie dem [Mac-Entwicklerprogramm](https://developer.apple.com/devcenter/mac/) beitreten, hat der Entwickler die Wahl, ob er einen Beitritt als Einzelperson oder als Unternehmen wünscht. Dies wird im folgenden Screenshot gezeigt:

[![Das Apple-Entwicklerportal](images/image1.png "The Apple Developer Portal")](images/image1-large.png#lightbox)

Wählen Sie den richtigen Registrierungstyp für Ihre Situation aus.

> [!NOTE]
> Die hier vorgenommene Auswahl hat Auswirkungen auf die Darstellung einiger Bildschirmanzeigen bei der Konfiguration eines Entwicklerkontos. Die Beschreibungen und Screenshots in diesem Dokument erfolgen aus der Perspektive eines Entwicklerkontos für eine **Einzelperson**. In einem **Unternehmen** sind einige Optionen nur für Benutzer mit der Rolle **Teamadministrator** verfügbar.


### <a name="certificates-and-identifiersmacdeploy-testpublishing-to-the-app-storecertificates-identifiersmd"></a>[Zertifikate und Bezeichner](~/mac/deploy-test/publishing-to-the-app-store/certificates-identifiers.md)

Dieser Leitfaden enthält Informationen zum Erstellen der notwendigen Zertifikate und Bezeichner, die für das Veröffentlichen einer Xamarin.Mac-App benötigt werden.


### <a name="create-provisioning-profilemacdeploy-testpublishing-to-the-app-storeprofilesmd"></a>[Erstellen eines Bereitstellungsprofils](~/mac/deploy-test/publishing-to-the-app-store/profiles.md)

Dieser Leitfaden enthält Informationen zum Erstellen der erforderlichen Bereitstellungsprofile, die für das Veröffentlichen einer Xamarin.Mac-App benötigt werden.


### <a name="mac-app-configurationmacdeploy-testpublishing-to-the-app-storeapp-configurationmd"></a>[Mac-App-Konfiguration](~/mac/deploy-test/publishing-to-the-app-store/app-configuration.md)

Dieser Leitfaden enthält Informationen zum Konfigurieren einer Xamarin.Mac-App für die Veröffentlichung.


### <a name="sign-with-developer-idmacdeploy-testpublishing-to-the-app-storesigningmd"></a>[Signieren mit einer Entwickler-ID](~/mac/deploy-test/publishing-to-the-app-store/signing.md)

Dieser Leitfaden enthält Informationen zur Signierung einer Xamarin.Mac-App mit einer Entwickler-ID für die Veröffentlichung.


### <a name="bundle-for-mac-app-storemacdeploy-testpublishing-to-the-app-storebundlingmd"></a>[Bündeln für den Mac App Store](~/mac/deploy-test/publishing-to-the-app-store/bundling.md)

Dieser Leitfaden enthält Informationen zum Bündeln einer Xamarin.Mac-App für die Veröffentlichung im Mac App Store.


### <a name="upload-to-mac-app-storemacdeploy-testpublishing-to-the-app-storeuploadingmd"></a>[Hochladen in den Mac App Store](~/mac/deploy-test/publishing-to-the-app-store/uploading.md)

Dieser Leitfaden enthält Informationen zum Hochladen einer Xamarin.Mac-App für die Veröffentlichung im Mac App Store.


## <a name="related-links"></a>Verwandte Links

- [Installation](/visualstudio/mac/installation/)
- [„Hallo, Mac“-Beispiel](~/mac/get-started/hello-mac.md)
- [Developer ID and GateKeeper (Entwickler-ID und Gatekeeper)](https://developer.apple.com/resources/developer-id/)
