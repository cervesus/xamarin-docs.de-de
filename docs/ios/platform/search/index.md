---
title: APIs in xamarin. IOS suchen
description: In diesem Artikel wird die Verwendung der neuen App Search-APIs beschrieben, die von IOS 9 bereitgestellt werden, damit Benutzer in ihren xamarin. IOS-apps nach Informationen und Funktionen suchen können.
ms.prod: xamarin
ms.assetid: 7323EB3D-A78F-4BF0-9990-3160C7E83CF0
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/20/2017
ms.openlocfilehash: eb88f7c1de12eee59ea4c2a271079e6b96c29b09
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70286603"
---
# <a name="search-apis-in-xamarinios"></a>APIs in xamarin. IOS suchen

_In diesem Artikel wird die Verwendung der APP-Suche-APIs von IOS 9 erläutert, mit denen Benutzer in ihren xamarin. IOS-apps nach Informationen und Funktionen suchen können._

Die Suche wurde in ios 9 erweitert und bietet hervorragend neue Möglichkeiten für den Zugriff auf Informationen und Features in einer xamarin. IOS-app. Mithilfe der neuen App-Such-APIs können App-Inhalte durch Spotlight-und Safari-Suchergebnisse, Übergabe und Siri-Erinnerungen und Vorschläge durchsucht werden. Dadurch können Benutzer schnell auf Aktivitäten und Informationen innerhalb ihrer App zugreifen.

Außerdem vereinfachen die neuen Such-APIs das Integrieren der Suche in Ihre APP, ohne dass eine vorherige Such Implementierung möglich ist. Daher beansprucht Apple in der Regel einige Stunden, bis der Inhalt einer IOS 9-APP über die APP-Suche universell durchsuchbar ist.

[![](images/intro01.png "Beispiel für IOS 9-APP-Inhalte, die universell mithilfe der APP-Suche durchsucht werden")](images/intro01.png#lightbox)

Die APP-Suche besteht aus drei separaten APIs:

1. [**Nsuseractivity**](nsuseractivity.md) : Dies ist eine Erweiterung der Handoff-API, die Apple in ios 8 veröffentlicht hat. Sie wird verwendet, um den Verlauf der APP-Interaktion vom Benutzer öffentlich und privat durchsuchbar zu machen.

2. [**Kernspotlight**](corespotlight.md) : ermöglicht einer APP, ihren Inhalt zu indizieren, sodass Sie in den Suchergebnissen angezeigt wird. Es funktioniert wie eine Datenbank-API, in der Elemente hinzugefügt und entfernt werden können, und dies ist die beste Möglichkeit, private Inhalte innerhalb einer APP zu indizieren.

3. [**Webmarkup**](web-markup.md) : für apps, die Zugriff auf Ihre Inhalte über eine Weboberfläche ermöglichen (nicht nur innerhalb der APP). Webinhalte können mit speziellen Links gekennzeichnet werden, die von Apple durchforstet werden, und die Deep Linking für Ihre APP auf dem IOS 9-Gerät des Benutzers bereitstellen.

## <a name="selecting-an-app-search-approach"></a>Auswählen eines App-Such Ansatzes

Die Entscheidung, welche dieser Methoden implementiert werden soll, hängt von den Interaktions Typen ab, die von Ihrer APP bereitgestellt werden

Verwenden Sie die folgenden Richtlinien:

- [**Nsuseractivity**](nsuseractivity.md) – verwenden Sie dieses Framework, um die Suchbarkeit sowohl für öffentliche als auch für private Inhalte sowie durch die durch Suchbarkeit von Navigationspunkten innerhalb Ihrer APP zu ermöglichen.

- [**Kernspotlight**](corespotlight.md) – verwenden Sie dieses Framework, um durch Suchbarkeit für private, auf dem Gerät gespeicherte Daten bereitzustellen.

- [**Webmarkup**](web-markup.md) – verwenden Sie dieses Framework, um Suchfunktionen für apps bereitzustellen, die ihre Inhalte nicht nur innerhalb der APP, sondern auch über die Website der APP präsentieren.

Alle App-Such Ansätze sind unterschiedlich und können einzeln verwendet werden, aber Apple hat Sie für die Zusammenarbeit entworfen. Wenn Sie mehr als einen Ansatz verwenden, um ein bestimmtes Element zu indizieren, stellen Sie sicher, dass Sie bei jedem Ansatz dieselbe **Element-ID** verwenden, damit einzelne Verknüpfungen zusammenarbeiten.

Wenn Sie mehr als einen Ansatz verwenden, können Sie nicht nur sicherstellen, dass Ihre Inhalte vom Endbenutzer gefunden werden, sondern auch die Rangfolge Ihrer Elemente in der Suche verbessern.

Während der Rang Folgeprozess für den Entwickler größtenteils transparent ist, wiegt die Benutzerinteraktion mit einem bestimmten Element stark von diesem Rang (z. b. wenn der Benutzer einen Link tippen).
Durch die Bereitstellung umfangreicher, informativer Elemente können Sie sicherstellen, dass ein Benutzer für die Interaktion mit ihren Inhalten verwendet wird. dadurch erhöht sich die Rangfolge.

## <a name="what-content-to-index"></a>Zu indizierenden Inhalt

Apple bietet die folgenden Vorschläge, welche Inhalte und Aktionen für Such Indizes in Ihrer APP bereitgestellt werden sollen:

- Alle Inhalte, die vom Benutzer in ihrer App angezeigt, erstellt oder vom Benutzer erstellt wurden.
- Navigationspunkte und Features in der app.
- Dinge wie neue Nachrichten, Inhalte oder andere Typen von Elementen, die von ihrer App angezeigt werden und vor kurzem auf das Gerät heruntergeladen wurden.

## <a name="app-search-enhancements"></a>Verbesserungen bei der App-Suche

Kernspotlight in ios 10 bietet verschiedene Verbesserungen bei der APP-Suche, wie z.b.:

- **Crowdsource Deep-Link-Beliebtheit (mit differenziellen Datenschutz)** : bietet eine Möglichkeit zum herauf Stufen von Deep-verknüpften App-Inhalten in den Suchergebnissen.
- **In-App-Suche** : Verwenden Sie `CSSearchQuery` die neue-Klasse, um in-App-Spotlight-Suchfunktionen wie die Funktionsweise der Mail-, Nachrichten-und Notes-apps bereitzustellen
- **Such Fortsetzung** : ermöglicht einem Benutzer das Starten einer Suche in Spotlight oder Safari, das Öffnen einer APP und das Fortsetzen der Suche.
- **Visualisierung der Überprüfungs Ergebnisse** : das [Validierungs Tool der App Search-API](https://search.developer.apple.com/appsearch-validation-tool) von Apple zeigt nun eine visuelle Darstellung des Markups einer Website und die Deep-Verknüpfung bei der Vorbildung von Tests an.
- **Nachrichten-APP-Image Freigabe** : ermöglicht beliebte in-App-Images, die für die Freigabe in Nachrichten (über eine Nachrichten-APP-Erweiterung) bereitgestellt werden, um Sie in Spotlight

Weitere Informationen finden Sie im Leitfaden zur [App-Suche-Erweiterungen](~/ios/platform/search/app-search-enhancements.md) .

### <a name="proactive-suggestions"></a>Proaktive Vorschläge

IOS 10 bietet neue Möglichkeiten, um die Einbindung in eine APP zu fördern, indem es dem System ermöglicht, den Benutzern proaktiv nützliche Informationen zu den richtigen Zeiten zu präsentieren. Ebenso wie IOS 9 die Möglichkeit bot, der App mithilfe von Spotlight-, Handoff-und Siri-Vorschlägen Deep Search hinzuzufügen, kann eine APP mit IOS 10 Funktionen verfügbar machen, die dem Benutzer vom System aus den folgenden Speicherorten angezeigt werden können:

- Der APP-Switcher
- Der Sperrbildschirm
- Carplay
- Karten
- Siri-Interaktionen
- Quick Type-Vorschläge 

Eine APP macht diese Funktionalität für das System mithilfe einer Sammlung von Technologien verfügbar, wie z. b. [nsuseractivity](xref:Foundation.NSUserActivity), Web Markup, Core Spotlight, MapKit, Media Player und UIKit.

Weitere Informationen finden Sie in unserem Leitfaden für [proaktive Vorschläge](~/ios/platform/search/proactive-suggestions.md) .

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die neuen Such-API-Features behandelt, die IOS 9 für xamarin. IOS-apps bereitstellt. Es wurden die Methoden [nsuseractivity](nsuseractivity.md), [Core Spotlight](corespotlight.md) und [Web Markup](web-markup.md) für die Indizierung von Inhalten behandelt. Es wurde eine kurze Erläuterung dazu verwendet, wann ein bestimmter suchansatz verwendet werden sollte und welche Arten von Inhalten indiziert werden sollen.



## <a name="related-links"></a>Verwandte Links

- [IOS 9-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [IOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Programmier Handbuch für die APP-Suche](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
