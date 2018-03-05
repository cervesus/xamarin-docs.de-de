---
title: "App-Verteilung – Übersicht"
description: "Dieser Artikel bietet eine Übersicht über Verteilungstechniken, die für Xamarin.iOS-Anwendungen verfügbar sind, und verweist auf noch ausführlichere Dokumente zu diesem Thema."
ms.topic: article
ms.prod: xamarin
ms.assetid: 341D36DB-BB07-FA94-BCC9-5F8C0B18C179
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: eec352264d918730e68a925f2a1e3796d9125c88
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="app-distribution-overview"></a>App-Verteilung – Übersicht

_Dieser Artikel bietet eine Übersicht über Verteilungstechniken, die für Xamarin.iOS-Anwendungen verfügbar sind, und verweist auf noch ausführlichere Dokumente zu diesem Thema._

Nach der Entwicklung der Xamarin.iOS-App ist der nächste Schritt im Lebenszyklus der Softwareentwicklung die Verteilung der App an Benutzer. Dieser Vorgang wird im folgenden Diagramm abgebildet:


[![](images/publishingdiagram.png "Nachdem die iOS-App entwickelt wurde, muss die App im nächsten Schritt an die Benutzer verteilt werden. Dies zeigt der markierte Bereich im Diagramm")](images/publishingdiagram.png)


Apple bietet zum Verteilen einer iOS-Anwendung mehrere Möglichkeiten, die von Xamarin.iOS unterstützt werden:

1. [**App Store-Verteilung**](#App_Store_Distribution)
2. [**Interne Verteilung (Enterprise-Verteilung)**](#In-House_Distribution)
2. [**Ad-hoc-Verteilung**](#Ad_Hoc_Distribution)

Für jedes dieser Szenarios müssen Anwendungen mit dem entsprechenden *Bereitstellungsprofil* bereitgestellt werden. Bereitstellungsprofile sind Dateien, die Informationen zur Codesignierung sowie die Identität der Anwendung und den beabsichtigten Verteilungsmechanismus enthalten. Bei einer Verteilung, die nicht über den App Store erfolgt, enthalten Bereitstellungsprofile auch Informationen darüber, für welche Geräte eine App bereitgestellt werden kann.

## <a name="app-store-distribution"></a>App Store-Verteilung

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

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde eine kurze Übersicht über die Verteilungsmechanismen gegeben, die für Xamarin.iOS-Anwendungen verfügbar sind. Außerdem wurden der iTunes App Store, die Ad-hoc-Bereitstellung und die interne Bereitstellung einführend dargestellt sowie Links zu ausführlicheren Informationen bereitgestellt.

## <a name="related-links"></a>Verwandte Links

- [App Store-Verteilung](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [Konfigurieren einer App in iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Veröffentlichen im App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [Interne Verteilung](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [Ad-hoc-Verteilung](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [Die Datei „iTunesMetadata.plist“](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [IPA-Unterstützung](~/ios/deploy-test/app-distribution/ipa-support.md)
- [Problembehandlung](~/ios/deploy-test/troubleshooting.md)
