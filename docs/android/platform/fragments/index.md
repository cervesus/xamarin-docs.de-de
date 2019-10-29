---
title: Fragmente
description: In Android 3,0 wurden Fragmente eingeführt, die zeigen, wie flexiblere Entwürfe für viele verschiedene Bildschirmgrößen auf Smartphones und Tablets unterstützt werden. In diesem Artikel wird beschrieben, wie Sie mithilfe von Fragmenten xamarin. Android-Anwendungen entwickeln und Fragmente auf Geräten mit Pre-Android 3,0 (API-Ebene 11) unterstützen.
ms.prod: xamarin
ms.assetid: 1AFB4242-A337-F8E0-83D9-B8D850D7F384
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/15/2018
ms.openlocfilehash: 5d243429fe4f61768568a634b205055c1ad94297
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73020250"
---
# <a name="fragments"></a>Fragmente

_In Android 3,0 wurden Fragmente eingeführt, die zeigen, wie flexiblere Entwürfe für viele verschiedene Bildschirmgrößen auf Smartphones und Tablets unterstützt werden. In diesem Artikel wird beschrieben, wie Sie mithilfe von Fragmenten xamarin. Android-Anwendungen entwickeln und Fragmente auf Geräten mit Pre-Android 3,0 (API-Ebene 11) unterstützen._

## <a name="fragments-overview"></a>Übersicht über Fragmente

Die größeren Bildschirmgrößen auf den meisten Tablets haben zur Android-Entwicklung eine zusätzliche Komplexitäts Ebene hinzugefügt – ein für den kleinen Bildschirm konzipiertes Layout funktioniert nicht notwendigerweise auch für größere Bildschirme und umgekehrt. Um die Anzahl der von diesem Dienst eingeführten Komplikationen zu reduzieren, wurden von Android 3,0 zwei neue Features, *Fragmente* und *Unterstützungspakete*hinzugefügt.

Fragmente können als Benutzeroberflächen Module betrachtet werden. Sie ermöglichen es dem Entwickler, die Benutzeroberfläche in isolierte, wiederverwendbare Teile aufzuteilen, die in separaten Aktivitäten ausgeführt werden können. Zur Laufzeit entscheiden die Aktivitäten selbst, welche Fragmente verwendet werden sollen.

Unterstützungspakete wurden ursprünglich als *Kompatibilitäts Bibliotheken* bezeichnet, und zulässige Fragmente können auf Geräten verwendet werden, auf denen Android-Versionen vor Android 3,0 (API-Ebene 11) ausgeführt werden.

Beispielsweise wird in der folgenden Abbildung veranschaulicht, wie eine einzelne Anwendung Fragmente über verschiedene Geräte Formfaktoren hinweg verwendet.

[![Diagramm der Verwendung von Fragmenten in Tablets und Handsets](images/00.png)](images/00.png#lightbox)

*Fragment a* enthält eine Liste, während *Fragment B* Details für ein in dieser Liste ausgewähltes Element enthält. Wenn die Anwendung auf einem Tablet ausgeführt wird, können beide Fragmente in der gleichen Aktivität angezeigt werden. Wenn die gleiche Anwendung auf einem Mobilgerät (mit der geringeren Bildschirmgröße) ausgeführt wird, werden die Fragmente in zwei separaten Aktivitäten gehostet. Fragment a und Fragment B sind in beiden Formfaktoren identisch, aber die Aktivitäten, die Sie hosten, unterscheiden sich.

Zur Unterstützung von Aktivitäts Koordinaten und-Verwaltung all dieser Fragmente hat Android eine neue Klasse mit dem Namen " *fragmentmanager*" eingeführt. Jede Aktivität verfügt über eine eigene Instanz eines `FragmentManager` zum Hinzufügen, löschen und Auffinden von gehosteten Fragmenten. Das folgende Diagramm veranschaulicht die Beziehung zwischen Fragmenten und Aktivitäten:

[![Diagramm zum Veranschaulichen der Beziehungen zwischen Aktivität, fragmentmanager und Fragmenten](images/01.png)](images/01.png#lightbox)

In einigen Hinsicht können Fragmente als zusammengesetzte Steuerelemente oder als Mini Aktivitäten betrachtet werden. Sie bündeln Teile der Benutzeroberfläche in wiederverwendbare Module, die dann unabhängig von Entwicklern in Aktivitäten verwendet werden können. Ein Fragment verfügt über eine Ansichts Hierarchie – genau wie eine-Aktivität – aber im Gegensatz zu einer-Aktivität kann es für Bildschirme freigegeben werden. Sichten unterscheiden sich von Fragmenten, in denen Fragmente ihren eigenen Lebenszyklus aufweisen. Sichten nicht.

Obwohl es sich bei der Aktivität um einen Host für ein oder mehrere Fragmente handelt, sind Sie nicht direkt auf die Fragmente selbst aufmerksam. Ebenso sind Fragmente nicht direkt auf andere Fragmente in der hostingaktivität aufmerksam. Fragmente und Aktivitäten kennen jedoch den `FragmentManager` in Ihrer Aktivität. Mithilfe der `FragmentManager`ist es möglich, dass eine Aktivität oder ein Fragment einen Verweis auf eine bestimmte Instanz eines Fragments erhält und dann Methoden für diese Instanz aufruft. Auf diese Weise kann die-Aktivität oder die-Fragmente kommunizieren und mit anderen Fragmenten interagieren.

Dieses Handbuch enthält umfassende Informationen zur Verwendung von Fragmenten, einschließlich:

- **Erstellen von Fragmenten** – Erstellen von grundlegenden fragmentmethoden und Schlüsselmethoden, die implementiert werden müssen.
- **Fragmentverwaltung und Transaktionen** – Gewusst wie: Bearbeiten von Fragmenten zur Laufzeit.
- **Android-Unterstützungspaket** – verwenden Sie die Bibliotheken, die die Verwendung von Fragmenten für ältere Android-Versionen ermöglichen.

## <a name="requirements"></a>Anforderungen

Fragmente sind in der Android SDK verfügbar, beginnend mit API-Ebene 11 (Android 3,0), wie im folgenden Screenshot zu sehen:

[![auswählen der API-Ebene im Android SDK Manager](images/02.png)](images/02.png#lightbox)

Fragmente sind in xamarin. Android 4,0 und höher verfügbar. Eine xamarin. Android-Anwendung muss mindestens API Level 11 (Android 3,0) oder höher als Ziel haben, um Fragmente verwenden zu können. Das Ziel Framework kann wie unten gezeigt in den Projekteigenschaften festgelegt werden:

[![Festlegen der Ziel Framework-API-Ebene in den Projektoptionen](images/03-sml.png)](images/03.png#lightbox)

Es ist möglich, Fragmente in älteren Versionen von Android mit dem Android-Unterstützungspaket und xamarin. Android 4,2 oder höher zu verwenden. Diese Vorgehensweise wird in den Dokumenten dieses Abschnitts ausführlicher behandelt.

## <a name="related-links"></a>Verwandte Links

- [Honeycomb-Katalog (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/honeycombgallery)
- [Fragmente](https://developer.android.com/guide/topics/fundamentals/fragments.html)
- [Unterstützungspaket](https://developer.android.com/sdk/compatibility-library.html)
