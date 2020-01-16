---
title: Veröffentlichen von Xamarin.Mac-Apps im Mac App Store
description: In diesem Dokument wird die Bereitstellung einer Xamarin.Mac-App mit Visual Studio für Mac beschrieben. Dabei wird die Einrichtung eines Mac-Entwicklerkontos, das Erstellen von Zertifikaten für die Codesignierung und das Verwenden dieser Zertifikate für das Erstellen von Mac-Apps, die direkt oder über den Mac App Store verteilt werden können, erläutert.
ms.prod: xamarin
ms.assetid: D26C5E54-EAD2-5487-264D-4263AEA1EBF2
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 2b6dcd53a9761ec3f030f3f5bf81894e9faa8b1f
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75487021"
---
# <a name="publishing-xamarinmac-apps-to-the-mac-app-store"></a>Veröffentlichen von Xamarin.Mac-Apps im Mac App Store

## <a name="overview"></a>Übersicht

Xamarin.Mac-Apps können auf zwei unterschiedliche Arten verteilt werden:

- **Entwickler-ID**: Mit einer Entwickler-ID signierte Anwendungen können außerhalb des App Store verteilt werden. Sie werden jedoch von GateKeeper erkannt und sind für die Installation zulässig.
- **Mac App Store**: Für Apps ist ein Installer-Paket erforderlich. Zudem müssen App und Installer signiert sein, damit eine Übermittlung in den Mac App Store möglich ist.

In diesem Dokument wird erläutert, wie Sie mit Visual Studio für Mac und Xcode ein Apple-Entwicklerkonto einrichten und ein Xamarin.Mac-Projekt für jeden Bereitstellungstyp konfigurieren.

## <a name="mac-developer-program"></a>Mac-Entwicklerprogramm

Wenn Sie dem [Mac-Entwicklerprogramm](https://developer.apple.com/devcenter/mac/) beitreten, hat der Entwickler die Wahl, ob er einen Beitritt als Einzelperson oder als Unternehmen wünscht. Dies wird im folgenden Screenshot gezeigt:

[![Das Apple Developer Portal](images/image1.png "Das Apple Developer Portal")](images/image1-large.png#lightbox)

Wählen Sie den richtigen Registrierungstyp für Ihre Situation aus.

> [!NOTE]
> Die hier vorgenommene Auswahl hat Auswirkungen auf die Darstellung einiger Bildschirmanzeigen bei der Konfiguration eines Entwicklerkontos. Die Beschreibungen und Screenshots in diesem Dokument erfolgen aus der Perspektive eines Entwicklerkontos für eine **Einzelperson**. In einem **Unternehmen** sind einige Optionen nur für Benutzer mit der Rolle **Teamadministrator** verfügbar.

### <a name="certificates-and-identifiersmacdeploy-testpublishing-to-the-app-storecertificates-identifiersmd"></a>[Zertifikate und Bezeichner](~/mac/deploy-test/publishing-to-the-app-store/certificates-identifiers.md)

Dieser Leitfaden enthält Informationen zum Erstellen der notwendigen Zertifikate und Bezeichner, die für das Veröffentlichen einer Xamarin.Mac-App benötigt werden.

### <a name="create-provisioning-profilemacdeploy-testpublishing-to-the-app-storeprofilesmd"></a>[Erstellen eines Bereitstellungsprofils](~/mac/deploy-test/publishing-to-the-app-store/profiles.md)

Dieser Leitfaden enthält Informationen zum Erstellen der erforderlichen Bereitstellungsprofile, die für das Veröffentlichen einer Xamarin.Mac-App benötigt werden.

### <a name="mac-app-configurationmacdeploy-testpublishing-to-the-app-storeapp-configurationmd"></a>[Konfiguration einer Mac-App](~/mac/deploy-test/publishing-to-the-app-store/app-configuration.md)

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
