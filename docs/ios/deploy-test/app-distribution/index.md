---
title: Übersicht über die Xamarin.iOS-App-Verteilung
description: Dieser Artikel bietet eine Übersicht über Verteilungstechniken, die für Xamarin.iOS-Anwendungen verfügbar sind, und verweist auf noch ausführlichere Dokumente zu diesem Thema.
ms.prod: xamarin
ms.assetid: 341D36DB-BB07-FA94-BCC9-5F8C0B18C179
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 20126849027f735e9ecd3599c290b4e7a57f837e
ms.sourcegitcommit: ec62e2624295aa502ec35ac782031d61d61c3aaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2020
ms.locfileid: "75886579"
---
# <a name="xamarinios-app-distribution-overview"></a>Übersicht über die Xamarin.iOS-App-Verteilung

_Dieser Artikel bietet eine Übersicht über Verteilungstechniken, die für Xamarin.iOS-Anwendungen verfügbar sind, und verweist auf noch ausführlichere Dokumente zu diesem Thema._

Nach der Entwicklung der Xamarin.iOS-App ist der nächste Schritt im Lebenszyklus der Softwareentwicklung die Verteilung der App an Benutzer. Dieser Vorgang wird im folgenden Diagramm abgebildet:

[![Nachdem die iOS-App entwickelt wurde, ist der nächste Schritt die Verteilung der App an die Benutzer, was im hervorgehobenen Abschnitt dieses Diagramms gezeigt wird.](images/publishingdiagram.png)](images/publishingdiagram.png#lightbox)

Apple bietet die folgenden Optionen für die Verteilung von iOS-Anwendungen:

- [**App Store**](#app-store-distribution)
- [**Interne Verteilung (Enterprise-Verteilung)** ](#in-house-distribution)
- [**Ad-hoc-Verteilung**](#ad-hoc-distribution)
- [**Benutzerdefinierte Apps für Unternehmen**](#custom-apps-for-business)

Für jedes dieser Szenarios müssen Anwendungen mit dem entsprechenden *Bereitstellungsprofil* bereitgestellt werden. Bereitstellungsprofile sind Dateien, die Informationen zur Codesignierung sowie die Identität der Anwendung und den beabsichtigten Verteilungsmechanismus enthalten. Bei einer Verteilung, die nicht über den App Store erfolgt, enthalten Bereitstellungsprofile auch Informationen darüber, für welche Geräte eine App bereitgestellt werden kann.

## <a name="app-store-distribution"></a>App Store-Verteilung

> [!IMPORTANT]
> Apple [hat mitgeteilt](https://developer.apple.com/ios/submit/), dass ab März 2019 alle Apps und Updates, die an den App Store gesendet werden, mit dem iOS 12.1 SDK oder höher, das in Xcode 10.1 oder höher enthalten ist, erstellt worden sein müssen.
> Apps müssen ebenso die Bildschirmgrößen des iPhone XS und des iPad Pro in 12,9" unterstützen.

Dies ist die wichtigste Methode zur Verteilung von iOS-Anwendungen an Benutzer mit iOS-Geräten. Alle Apps, die im App Store eingereicht werden, müssen von Apple genehmigt werden.

Apps werden über das Portal *iTunes Connect* im App Store eingereicht. Im Leitfaden [Configure your App in iTunes Connect (Konfigurieren Ihrer App in iTunes Connect)](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) finden Sie weitere Informationen darüber, wie Sie dieses Portal einrichten und verwenden, um eine Xamarin.iOS-App für die Veröffentlichung im App Store vorzubereiten.

Beachten Sie, dass nur Entwickler, die Teil des **Apple Developer Program** sind, Zugriff auf iTunes Connect haben. Mitglieder des **Apple Developer Enterprise Program** haben keinen Zugriff.

Weitere Informationen finden Sie im Leitfaden [App Store Distribution (App Store-Verteilung)](~/ios/deploy-test/app-distribution/app-store-distribution/index.md).

## <a name="in-house-distribution"></a>Interne Verteilung

Durch die interne Verteilung, die auch als *Enterprise-Verteilung* bezeichnet wird, können Mitglieder des **Apple Developer Enterprise Program** Apps an andere Mitglieder innerhalb derselben Organisation verteilen. Die Vorteile der internen Verteilung bestehen darin, dass kein App Store-Review erforderlich ist und keine Beschränkung für die Anzahl der Geräte vorhanden ist, auf denen eine Anwendung installiert werden kann. Beachten Sie aber, dass Mitglieder des **Apple Developer Enterprise Program** **nicht** auf iTunes Connect zugreifen können und daher der Lizenznehmer für die Verteilung der App zuständig ist.

Weitere Informationen zur internen Verteilung einer Anwendung und den hierfür erforderlichen Vorbereitungen finden Sie im [Leitfaden zur internen Verteilung](~/ios/deploy-test/app-distribution/in-house-distribution.md).

## <a name="ad-hoc-distribution"></a>Ad-hoc-Verteilung

Xamarin.iOS-Anwendungen können über die Ad-hoc-Verteilung durch Benutzer getestet werden. Mit dieser Verteilungsart, die sowohl im **Apple Developer Program** als auch im **Apple Developer Enterprise Program** verfügbar ist, können bis zu 100 iOS-Geräte getestet werden. Der ideale Anwendungsfall für die Ad-hoc-Verteilung ist die Verteilung innerhalb eines Unternehmens, wenn iTunes Connect als Option ausscheidet.

Weitere Informationen zur internen Verteilung einer Anwendung und den hierfür erforderlichen Vorbereitungen finden Sie im [Leitfaden zur Ad-hoc-Verteilung](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md).

## <a name="custom-apps-for-business"></a>Benutzerdefinierte Apps für Unternehmen

Apple ermöglicht die [benutzerdefinierte Verteilung](https://developer.apple.com/business/custom-apps/) von Apps an Unternehmen und Bildungseinrichtungen. Weitere Informationen finden Sie im [Apple Business Manager-Benutzerhandbuch](https://support.apple.com/guide/apple-business-manager/welcome/web).

## <a name="related-links"></a>Verwandte Links

- [App Store-Verteilung](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [Konfigurieren einer App in iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Veröffentlichen im App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [Interne Verteilung](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [Ad-hoc-Verteilung](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [Die Datei „iTunesMetadata.plist“](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [IPA-Unterstützung](~/ios/deploy-test/app-distribution/ipa-support.md)
- [Problembehandlung](~/ios/deploy-test/troubleshooting.md)
