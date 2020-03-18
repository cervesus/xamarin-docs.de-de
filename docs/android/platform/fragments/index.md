---
title: Fragmente
description: In Android 3.0 wurden Fragmente eingeführt, die zeigen, wie sich flexiblere Entwürfe für die vielen verschiedenen Bildschirmgrößen von Smartphones und Tablets unterstützen lassen. In diesem Artikel wird beschrieben, wie Sie Fragmente verwenden, um Xamarin.Android-Anwendungen zu entwickeln, und wie Sie Fragmente auf Geräten mit Versionen vor Android 3.0 (API-Ebene 11) unterstützen.
ms.prod: xamarin
ms.assetid: 1AFB4242-A337-F8E0-83D9-B8D850D7F384
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/15/2018
ms.openlocfilehash: 5d243429fe4f61768568a634b205055c1ad94297
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "73020250"
---
# <a name="fragments"></a>Fragmente

_In Android 3.0 wurden Fragmente eingeführt, die zeigen, wie sich flexiblere Entwürfe für die vielen verschiedenen Bildschirmgrößen von Smartphones und Tablets unterstützen lassen. In diesem Artikel wird beschrieben, wie Sie Fragmente verwenden, um Xamarin.Android-Anwendungen zu entwickeln, und wie Sie Fragmente auf Geräten mit Versionen vor Android 3.0 (API-Ebene 11) unterstützen._

## <a name="fragments-overview"></a>Übersicht über Fragmente

Die größeren Bildschirme der meisten Tablets haben die Entwicklung für Android schwieriger gemacht: Ein Layout, das für kleine Bildschirme entworfen wurde, funktioniert nicht notwendigerweise auch auf größeren Bildschirmen und umgekehrt. Um die dadurch entstandene Komplexität bei der Entwicklung zu mindern, wurden in Android 3.0 zwei neue Features eingeführt: *Fragmente* (Fragments) und *Unterstützungspakete* (Support Packages).

Sie können sich Fragmente als Module der Benutzeroberfläche vorstellen. Sie ermöglichen es dem Entwickler, die Benutzeroberfläche in isolierte, wiederverwendbare Abschnitte zu unterteilen, die in separaten Aktivitäten ausgeführt werden können. Zur Laufzeit entscheiden die Aktivitäten selbst, welche Fragmente verwendet werden sollen.

Unterstützungspakete wurden ursprünglich als *Compatibility Libraries* (Kompatibilitätsbibliotheken) bezeichnet und ermöglichen die Verwendung von Fragmenten auf Geräten, auf denen Android-Versionen vor Android 3.0 (API-Ebene 11) ausgeführt wird.

Die folgende Abbildung veranschaulicht, wie eine einzelne Anwendung Fragmente in verschiedenen Geräteformfaktoren verwendet.

[![Diagramm zur Verwendung von Fragmenten auf Tablets und Smartphones](images/00.png)](images/00.png#lightbox)

*Fragment A* enthält eine Liste, und *Fragment B* enthält Details zu einem in dieser Liste ausgewählten Element. Wenn die Anwendung auf einem Tablet ausgeführt wird, können beide Fragmente in derselben Aktivität angezeigt werden. Wenn dieselbe Anwendung auf einem Smartphone mit einem kleineren Bildschirm ausgeführt wird, werden die Fragmente in zwei separaten Aktivitäten gehostet. Fragment A und Fragment B sind in beiden Formfaktoren gleich, aber die Aktivitäten, die diese hosten, unterscheiden sich voneinander.

Damit eine Aktivität all diese Fragmente koordinieren und verwalten kann, wurde in Android eine neue Klasse namens *FragmentManager* eingeführt. Jede Aktivität verfügt über eine eigene Instanz von `FragmentManager`, um gehostete Fragmente hinzuzufügen, zu löschen und zu finden. Das folgende Diagramm veranschaulicht die Beziehung zwischen Fragmenten und Aktivitäten:

[![Diagramm zur Veranschaulichung der Beziehungen zwischen Aktivität, Fragment-Manager und Fragmenten](images/01.png)](images/01.png#lightbox)

In verschiedener Hinsicht können Fragmente als zusammengesetzte Steuerelemente oder Miniaktivitäten betrachtet werden. Sie bündeln Teile der Benutzeroberfläche in wiederverwendbare Module, die dann unabhängig von Entwicklern in Aktivitäten verwendet werden können. Ein Fragment verfügt genau wie eine Aktivität über eine Ansichtshierarchie, kann aber im Gegensatz zu einer Aktivität in verschiedenen Bildschirmen verwendet werden. Ansichten unterscheiden sich von Fragmenten insofern, als Fragmente einen eigenen Lebenszyklus aufweisen, was für Ansichten nicht gilt.

Eine Aktivität kann zwar ein oder mehrere Fragmente hosten, hat aber keine direkte Kenntnis der Fragmente selbst. Ebenso haben Fragmente keine direkte Kenntnis von anderen Fragmenten in der Hostaktivität. Fragmente und Aktivitäten haben jedoch Kenntnis vom `FragmentManager` in ihrer Aktivität. Durch Verwendung von `FragmentManager` kann eine Aktivität oder ein Fragment einen Verweis auf eine bestimmte Instanz eines Fragments abrufen und dann in dieser Instanz Methoden aufrufen. Auf diese Weise können Aktivitäten und Fragmente mit anderen Fragmenten kommunizieren und interagieren.

Dieser Leitfaden bietet umfassende Informationen zur Verwendung von Fragmenten, u. a. folgende:

- **Erstellen von Fragmenten**: Informationen zum Erstellen eines grundlegenden Fragments und zu den wichtigsten Methoden, die implementiert werden müssen.
- **Fragmentverwaltung und Transaktionen**: Informationen zum Ändern von Fragmenten zur Laufzeit.
- **Android-Unterstützungspaket**: Informationen zu den Bibliotheken, die eine Verwendung von Fragmenten in älteren Android-Versionen ermöglichen.

## <a name="requirements"></a>Anforderungen

Fragmente sind ab API-Ebene 11 (Android 3.0) im Android SDK verfügbar, wie im folgenden Screenshot gezeigt:

[![Auswählen der API-Ebene im Android-SDK-Manager](images/02.png)](images/02.png#lightbox)

Fragmente sind in Xamarin.Android 4.0 und höher verfügbar. Eine Xamarin.Android-Anwendung muss mindestens für die API-Ebene 11 (Android 3.0) konzipiert sein, um Fragmente verwenden zu können. Das Zielframework kann in den Projekteigenschaften festgelegt werden, wie im Folgenden gezeigt:

[![Festlegen der API-Ebene des Zielframeworks in den Projekteigenschaften](images/03-sml.png)](images/03.png#lightbox)

Mithilfe des Android-Unterstützungspakets und Xamarin.Android 4.2 oder höher ist es möglich, Fragmente in älteren Android-Versionen zu verwenden. Details dazu finden Sie in den Dokumenten dieses Abschnitts.

## <a name="related-links"></a>Verwandte Links

- [Honeycomb Gallery (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/honeycombgallery)
- [Fragmente](https://developer.android.com/guide/topics/fundamentals/fragments.html)
- [Support Package](https://developer.android.com/sdk/compatibility-library.html)
