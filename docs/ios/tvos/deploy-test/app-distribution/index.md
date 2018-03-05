---
title: "App-Verteilung – Übersicht"
description: "Dieses Dokument bietet einen Überblick über die Verteilung Techniken, die für Xamarin.tvOS app verfügbar sind und dient als ein Zeiger auf eine ausführlichere Dokumenten für das Thema."
ms.topic: article
ms.prod: xamarin
ms.assetid: D5E0F446-C083-4E21-9788-FC84D32D00C4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: c0cfe437b03a1f0dea05a506b1dfce62a4658bb4
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="app-distribution-overview"></a>App-Verteilung – Übersicht

_Dieses Dokument bietet einen Überblick über die Verteilung Techniken, die für Xamarin.tvOS app verfügbar sind und dient als ein Zeiger auf eine ausführlichere Dokumenten für das Thema._


Nachdem Ihre app Xamarin.tvOS entwickelt wurde, ist der nächste Schritt im Lebenszyklus Softwareentwicklung, Ihre app an Benutzer verteilen, wie im Abschnitt unten stehende Abbildung hervorgehoben dargestellt:


[![Das Software Development Lifecycle (Übersicht)](images/publishingdiagram.png)](images/publishingdiagram.png)


Apple bietet die folgenden Möglichkeiten zum Verteilen einer app tvos. außerdem wurden die vom Xamarin.tvOS unterstützt werden:

1. [**App Store-Verteilung**](#Apple-TV-App-Store-Distribution)
2. [**Interne Verteilung (Enterprise-Verteilung)**](#In-House-Distribution) 
2. [**Ad-hoc-Verteilung**](#Ad_Hoc_Distribution) 

Für jedes dieser Szenarios müssen Anwendungen mit dem entsprechenden *Bereitstellungsprofil* bereitgestellt werden. Bereitstellungsprofile sind Dateien, die Informationen zur Codesignierung sowie die Identität der Anwendung und den beabsichtigten Verteilungsmechanismus enthalten. Bei einer Verteilung, die nicht über den App Store erfolgt, enthalten Bereitstellungsprofile auch Informationen darüber, für welche Geräte eine App bereitgestellt werden kann.

<a name="Apple-TV-App-Store-Distribution" />

## <a name="apple-tv-app-store-distribution"></a>Apple TV-App Store-Verteilung

Dies ist die wichtigste Methode, dass apps tvos. außerdem wurden für Consumer für Apple TV-Geräte verteilt werden. Alle apps, die auf den Apple TV-App-Store übermittelt die Genehmigung von Apple.

Apps werden über das Portal *iTunes Connect* im App Store eingereicht. Finden Sie in unserer [Ihrer App im iTunes Connect konfigurieren](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) Handbuch Informationen zu den folgenden Themen:

- Verwalten von Vereinbarungen, Steuer und Banking.
- Erstellen eine iTunes Connect-Datensatz.
- Verwalten von App-Videos und Screenshots.
- Verwalten von App-Name, Beschreibung, was neu ist Schlüsselwörter und URLs.
- Verwalten von Allgemein App-Informationen.
- Verwalten von Game Center-Informationen.
- Verwalten von Informationen für die App überprüfen.
- Verwalten von Informationen zu den Preisen.
- Verwalten von In-App-Käufe Informationen.
- Anzeigen der Anwendung überprüft.

Beim Schreiben von nicht speziell für tvos. außerdem wurden, enthält dieses Dokument Informationen zum Einrichten und verwenden Sie dieses Portal Vorbereitung der Anwendung für die Veröffentlichung im App Store Apple TV an. Viele der gleichen Schritte sind erforderlich, ob Sie eine app für iOS oder tvos. außerdem wurden einrichten.

Wenn Sie alle oben aufgeführten Schritte abgeschlossen haben, finden Sie unter unsere [Ihrer tvos. außerdem wurden App im iTunes Connect konfigurieren](~/ios/tvos/deploy-test/app-distribution/itunes-connect.md) tvos. außerdem wurden app Informationen hinzufügen, die benötigt werden.

Beachten Sie, dass nur Entwickler, die Teil des **Apple Developer Program** sind, Zugriff auf iTunes Connect haben. Mitglieder des **Apple Developer Enterprise Program** haben keinen Zugriff.

Wenn Sie beim Übermitteln der app Xamarin.tvOS auf den Apple TV-App-Store Probleme auftreten, finden Sie in unserer [Problembehandlung](~/ios/tvos/troubleshooting.md) Handbuch. Es enthält einige bekannte Probleme, die auftreten können und wie sie in der Xamarin.tvOS gelöst.

Weitere Informationen finden Sie auf der [auf den Apple TV-App-Store veröffentlichen](~/ios/tvos/deploy-test/app-distribution/app-store-publishing.md) Handbuch.

<a name="In-House-Distribution" />

## <a name="in-house-distribution"></a>Interne Verteilung

Durch die interne Verteilung, die auch als *Enterprise-Verteilung* bezeichnet wird, können Mitglieder des **Apple Developer Enterprise Program** Apps an andere Mitglieder innerhalb derselben Organisation verteilen. Die Vorteile der internen Verteilung bestehen darin, dass kein App Store-Review erforderlich ist und keine Beschränkung für die Anzahl der Geräte vorhanden ist, auf denen eine Anwendung installiert werden kann. Beachten Sie aber, dass Mitglieder des **Apple Developer Enterprise Program** **nicht** auf iTunes Connect zugreifen können und daher der Lizenznehmer für die Verteilung der App zuständig ist.

Weitere Informationen zum Abrufen der Einrichtung und die Anwendung intern zu verteilen, finden Sie auf der [internen Verteilung Handbuch](~/ios/deploy-test/app-distribution/in-house-distribution.md). Dieses Dokument ist spezifisch für iOS, jedoch die gleichen Techniken für tvos. außerdem wurden apps verwendet werden.

<a name="Ad-Hoc-Distribution" />

## <a name="ad-hoc-distribution"></a>Ad-hoc-Verteilung

Xamarin.tvOS-apps können Benutzer über ad-hoc-Verteilung, die auf beide verfügbar ist überprüft werden. die **Apple Developer Program**, und die **Apple Developer Enterprise Program**, und ermöglicht bis zu 100 Apple TV-Geräten die zu testende. Der beste Anwendungsfall für ad-hoc-Verteilung ist Verteilung innerhalb Ihres Unternehmens aus, wenn iTunes Connect nicht möglich ist.

Weitere Informationen zum Einrichten und wie Sie Ihre app zu verteilen, intern abrufen, finden Sie auf der [Ad-hoc-Verteilung Handbuch](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md). Ebenfalls dieses Dokuments bezieht sich auf iOS, jedoch die gleichen Techniken für tvos. außerdem wurden apps verwendet werden.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat eine kurze Übersicht über die Verteilungsmechanismen, die für Xamarin.tvOS-apps verfügbar sind. Es eingeführt im Apple TV-App Store Ad-hoc- und intern Bereitstellung und Links zu ausführlicheren Informationen bereitgestellt.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos. außerdem wurden Handbücher für interaktive Workflowdienste-Schnittstelle](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos. außerdem wurden](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
