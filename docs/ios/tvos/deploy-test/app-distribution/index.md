---
title: TvOS-App-Übersicht über die Softwareverteilung
description: Dieses Dokument bietet eine Übersicht über verteilungstechniken, die für die Xamarin.tvOS-app verfügbar sind, und dient als Zeiger auf noch ausführlichere Dokumente zu diesem Thema.
ms.prod: xamarin
ms.assetid: D5E0F446-C083-4E21-9788-FC84D32D00C4
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 0a40d50d02008439e81d5db19bcda0647203e2da
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61414421"
---
# <a name="tvos-app-distribution-overview"></a>TvOS-App-Übersicht über die Softwareverteilung

_Dieses Dokument bietet eine Übersicht über verteilungstechniken, die für die Xamarin.tvOS-app verfügbar sind, und dient als Zeiger auf noch ausführlichere Dokumente zu diesem Thema._


Nachdem Ihre Xamarin.tvOS-app entwickelt wurde, ist der nächste Schritt im Lebenszyklus Softwareentwicklung, die Ihre app an Benutzer verteilen, wie in der hervorgehobene Abschnitt im folgenden Diagramm dargestellt:


[![Das Software Development Lifecycle (Übersicht)](images/publishingdiagram.png)](images/publishingdiagram.png#lightbox)


Apple bietet die folgenden Möglichkeiten zum Verteilen einer TvOS-app, die von Xamarin.tvOS unterstützt werden:

1. [**App Store-Verteilung**](#Apple-TV-App-Store-Distribution)
2. [**Interne Verteilung (Enterprise-Verteilung)**](#In-House-Distribution) 
2. [**Ad-hoc-Verteilung**](#Ad_Hoc_Distribution) 

Für jedes dieser Szenarios müssen Anwendungen mit dem entsprechenden *Bereitstellungsprofil* bereitgestellt werden. Bereitstellungsprofile sind Dateien, die Informationen zur Codesignierung sowie die Identität der Anwendung und den beabsichtigten Verteilungsmechanismus enthalten. Bei einer Verteilung, die nicht über den App Store erfolgt, enthalten Bereitstellungsprofile auch Informationen darüber, für welche Geräte eine App bereitgestellt werden kann.

<a name="Apple-TV-App-Store-Distribution" />

## <a name="apple-tv-app-store-distribution"></a>Apple TV App Store-Verteilung

Dies ist die wichtigste Methode zur TvOS-apps für Kunden auf Apple TV-Geräten verteilt werden. Alle für das Apple TV App Store eingereichte apps müssen von Apple genehmigt.

Apps werden über das Portal *iTunes Connect* im App Store eingereicht. Informieren Sie sich unsere [Konfigurieren Ihrer App in iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) Anleitung finden Sie Informationen zu den folgenden Themen:

- Verwalten von Vereinbarungen, steuern und Bankverbindungen.
- Erstellen eines iTunes Connect-Datensatzes.
- Verwalten von App-Videos und Screenshots.
- Verwalten von App-Name, Beschreibung, neues, Schlüsselwörter und URLs.
- Verwalten von allgemeinen App-Informationen.
- Verwalten von Informationen im Game Center.
- Verwalten von App-Bewertungen.
- Verwalten der Preisinformationen.
- Verwalten von In-App-Käufe Informationen.
- Anzeigen von Benutzerbewertungen.

Zwar nicht speziell für TvOS geschrieben werden soll, enthält dieses Dokument Informationen zum Einrichten und verwenden Sie dieses Portal zu Ihrer app zur Veröffentlichung in der Apple TV App Store vorzubereiten. Viele der gleichen Schritte sind erforderlich, ob Sie eine IOS- oder TvOS-app einrichten.

Nachdem Sie alle oben aufgeführten Schritte abgeschlossen haben, finden Sie unter unserem [Konfigurieren Ihrer TvOS-App in iTunes Connect](~/ios/tvos/deploy-test/app-distribution/itunes-connect.md) die TvOS-app-spezifischen Informationen hinzufügen, die benötigt werden.

Beachten Sie, dass nur Entwickler, die Teil des **Apple Developer Program** sind, Zugriff auf iTunes Connect haben. Mitglieder des **Apple Developer Enterprise Program** haben keinen Zugriff.

Wenn Sie beim Übermitteln der Xamarin.tvOS-app an das Apple TV App Store Probleme auftreten, finden Sie unsere [Problembehandlung](~/ios/tvos/troubleshooting.md) Guide. Sie enthält mehrere bekannte Probleme, die auftreten können und wie Sie sie in der Xamarin.tvOS lösen.

Weitere Informationen finden Sie auf die [in der Apple TV App Store veröffentlichen](~/ios/tvos/deploy-test/app-distribution/app-store-publishing.md) Guide.

<a name="In-House-Distribution" />

## <a name="in-house-distribution"></a>Interne Verteilung

Durch die interne Verteilung, die auch als *Enterprise-Verteilung* bezeichnet wird, können Mitglieder des **Apple Developer Enterprise Program** Apps an andere Mitglieder innerhalb derselben Organisation verteilen. Die Vorteile der internen Verteilung bestehen darin, dass kein App Store-Review erforderlich ist und keine Beschränkung für die Anzahl der Geräte vorhanden ist, auf denen eine Anwendung installiert werden kann. Beachten Sie aber, dass Mitglieder des **Apple Developer Enterprise Program** **nicht** auf iTunes Connect zugreifen können und daher der Lizenznehmer für die Verteilung der App zuständig ist.

Weitere Informationen zur Einrichtung und internen Verteilung Ihrer Anwendung den finden Sie in der [Leitfaden zur internen Verteilung](~/ios/deploy-test/app-distribution/in-house-distribution.md). In diesem Dokument ist spezifisch für iOS, jedoch die gleichen Techniken für TvOS-apps verwendet werden.

<a name="Ad_Hoc_Distribution"/>

## <a name="ad-hoc-distribution"></a>Ad-hoc-Verteilung

Xamarin.tvOS-apps können Benutzer über die ad-hoc-Verteilung, die auf beiden verfügbar ist, getestet werden. die **Apple Developer Program**, und die **Apple Developer Enterprise Program**, können bis zu 100 Apple TV-Geräte getestet werden. Der ideale Anwendungsfall für die ad-hoc-Verteilung ist Verteilung innerhalb Ihres Unternehmens an, wenn iTunes Connect nicht möglich ist.

Weitere Informationen zum Abrufen von einrichten und Ihre app internen Verteilung finden Sie in der [Leitfaden zur Ad-hoc-Verteilung](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md). In diesem Fall in diesem Dokument bezieht sich auf iOS, aber die gleichen Techniken für TvOS-apps verwendet werden.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

Dieser Artikel gibt einen kurzen Überblick über die Verteilungsmechanismen, die für Xamarin.tvOS-apps verfügbar sind. Er eingeführt, die Apple TV App Store, Ad-hoc- und die interne Bereitstellung und Links zu ausführlicheren Informationen angegeben.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [TvOS Human Interface-Handbücher](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos verwendet.](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
