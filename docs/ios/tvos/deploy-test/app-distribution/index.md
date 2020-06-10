---
title: Übersicht über tvos-App-Verteilung
description: Dieses Dokument enthält eine Übersicht über die Verteilungs Techniken, die für die xamarin. tvos-app verfügbar sind, und dient als Zeiger auf ausführlichere Dokumente zu diesem Thema.
ms.prod: xamarin
ms.assetid: D5E0F446-C083-4E21-9788-FC84D32D00C4
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: b311f3121ea6a58975d41b9690e31a44daa0951e
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84573702"
---
# <a name="tvos-app-distribution-overview"></a>Übersicht über tvos-App-Verteilung

_Dieses Dokument enthält eine Übersicht über die Verteilungs Techniken, die für die xamarin. tvos-app verfügbar sind, und dient als Zeiger auf ausführlichere Dokumente zu diesem Thema._

Nachdem Sie Ihre xamarin. tvos-app entwickelt haben, besteht der nächste Schritt im Lebenszyklus der Softwareentwicklung darin, Ihre APP an Benutzer zu verteilen, wie im folgenden hervorgehobenen Abschnitt der Abbildung dargestellt:

[![Übersicht über den Lebenszyklus der Softwareentwicklung](images/publishingdiagram.png)](images/publishingdiagram.png#lightbox)

Apple bietet die folgenden Möglichkeiten, um eine tvos-APP zu verteilen, die von xamarin. tvos unterstützt wird:

1. [**App Store-Verteilung**](#Apple-TV-App-Store-Distribution)
2. [**Intern (Enterprise)**](#In-House-Distribution) 
3. [**Ad-hoc**](#Ad_Hoc_Distribution) 

Für jedes dieser Szenarios müssen Anwendungen mit dem entsprechenden *Bereitstellungsprofil* bereitgestellt werden. Bereitstellungsprofile sind Dateien, die Informationen zur Codesignierung sowie die Identität der Anwendung und den beabsichtigten Verteilungsmechanismus enthalten. Bei einer Verteilung, die nicht über den App Store erfolgt, enthalten Bereitstellungsprofile auch Informationen darüber, für welche Geräte eine App bereitgestellt werden kann.

<a name="Apple-TV-App-Store-Distribution"></a>

## <a name="apple-tv-app-store-distribution"></a>Apple TV-App Store-Verteilung

Dies ist die Hauptmethode, mit der tvos-apps auf Apple TV-Geräten an Consumer verteilt werden. Alle apps, die an den Apple TV App Store übermittelt werden, erfordern eine Genehmigung durch Apple.

Apps werden über das Portal *iTunes Connect* im App Store eingereicht. Informationen zu den folgenden Themen finden Sie im Handbuch [configure your app in iTunes Connect (Konfigurieren ihrer app in iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) ):

- Verwalten von Vereinbarungen, Steuern und Banken.
- Erstellen eines iTunes Connect-Datensatzes
- Verwalten von App-Videos und Screenshots.
- Verwalten von App-Name, Beschreibung, Neuerungen, Schlüsselwörtern und URLs.
- Verwalten allgemeiner app-Informationen.
- Verwalten von Game Center Informationen
- Verwalten von App-Überprüfungs Informationen.
- Preisinformationen werden beibehalten.
- Verwalten von in-App-Kauf Informationen.
- Anzeigen von Anwendungs Überprüfungen.

Dieses Dokument enthält zwar nicht speziell für tvos geschrieben, aber dieses Dokument enthält Informationen zum Einrichten und verwenden dieses Portals zum Vorbereiten Ihrer APP für die Veröffentlichung im Apple TV App Store. Viele der gleichen Schritte sind erforderlich, unabhängig davon, ob Sie eine IOS-oder tvos-App einrichten.

Nachdem Sie alle oben aufgeführten Schritte ausgeführt haben, finden Sie weitere Informationen unter [Konfigurieren ihrer tvos-app in iTunes Connect](~/ios/tvos/deploy-test/app-distribution/itunes-connect.md) .

Beachten Sie, dass nur Entwickler, die Teil des **Apple Developer Program** sind, Zugriff auf iTunes Connect haben. Mitglieder des **Apple Developer Enterprise Program** haben keinen Zugriff.

Wenn Sie Probleme beim Einreichen Ihrer xamarin. tvos-APP an den Apple TV App Store haben, finden Sie weitere Informationen im Handbuch zur [Problem](~/ios/tvos/troubleshooting.md) Behandlung. Sie enthält mehrere bekannte Probleme, die auftreten können, und wie Sie in xamarin. tvos gelöst werden.

Weitere Informationen finden Sie im Leitfaden [Veröffentlichen im Apple TV App Store](~/ios/tvos/deploy-test/app-distribution/app-store-publishing.md) .

<a name="In-House-Distribution"></a>

## <a name="in-house-distribution"></a>Interne Verteilung

Durch die interne Verteilung, die auch als *Enterprise-Verteilung* bezeichnet wird, können Mitglieder des **Apple Developer Enterprise Program** Apps an andere Mitglieder innerhalb derselben Organisation verteilen. Die Vorteile der internen Verteilung bestehen darin, dass kein App Store-Review erforderlich ist und keine Beschränkung für die Anzahl der Geräte vorhanden ist, auf denen eine Anwendung installiert werden kann. Beachten Sie aber, dass Mitglieder des **Apple Developer Enterprise Program****nicht** auf iTunes Connect zugreifen können und daher der Lizenznehmer für die Verteilung der App zuständig ist.

Weitere Informationen zum Einrichten der Einrichtung und zur internen Verteilung Ihrer Anwendung finden Sie im [Handbuch zur internen Verteilung](~/ios/deploy-test/app-distribution/in-house-distribution.md). Dieses Dokument ist für IOS spezifisch, aber die gleichen Techniken werden auch für tvos-Apps verwendet.

<a name="Ad_Hoc_Distribution"></a>

## <a name="ad-hoc-distribution"></a>Ad-hoc-Verteilung

Xamarin. tvos-Apps können über die Ad-hoc-Verteilung Benutzer getestet werden, die sowohl im **Apple Developer Program**als auch im **Apple Developer Enterprise Program**verfügbar ist und bis zu 100 Apple TV-Geräte getestet werden können. Der beste Anwendungsfall für die Ad-hoc-Verteilung ist die Verteilung innerhalb Ihres Unternehmens, wenn iTunes Connect keine Option ist.

Weitere Informationen zum Einrichten und zur internen Verteilung ihrer App finden Sie im Leitfaden für die [Ad-hoc-Verteilung](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md). Dieses Dokument ist zwar für IOS spezifisch, aber die gleichen Techniken werden auch für tvos-Apps verwendet.

<a name="Summary"></a>

## <a name="summary"></a>Zusammenfassung

Dieser Artikel stellt einen kurzen Überblick über die Verteilungsmechanismen bereit, die für xamarin. tvos-apps verfügbar sind. Dabei wurden der Apple TV App Store, die Ad-hoc-Bereitstellung und die interne Bereitstellung eingeführt, und es wurden Links zu ausführlicheren Informationen bereitgestellt.

## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos Human Interface Guides](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Leitfaden zur APP-Programmierung für tvos](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
