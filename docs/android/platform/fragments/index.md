---
title: Fragmente
description: "Android 3.0 eingeführten Fragmente mit wie vielen unterschiedlichen Bildschirmgrößen auf Telefonen und Tablets gefunden flexiblere Entwürfe unterstützt. In diesem Artikel befasst sich mit Fragmenten Xamarin.Android Anwendungen entwickeln, sowie die Informationen zur Unterstützung von Fragmenten vorab Android 3.0 (API-Ebene 11)-Geräte."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1AFB4242-A337-F8E0-83D9-B8D850D7F384
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/15/2018
ms.openlocfilehash: 0a9a1f41810fe113ac3d88d2533411ac537840ab
ms.sourcegitcommit: 028936cd2fe547963c1cf82343c3ee16f658089a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2018
---
# <a name="fragments"></a>Fragmente

_Android 3.0 eingeführten Fragmente mit wie vielen unterschiedlichen Bildschirmgrößen auf Telefonen und Tablets gefunden flexiblere Entwürfe unterstützt. In diesem Artikel befasst sich mit Fragmenten Xamarin.Android Anwendungen entwickeln, sowie die Informationen zur Unterstützung von Fragmenten vorab Android 3.0 (API-Ebene 11)-Geräte._

## <a name="fragments-overview"></a>Übersicht über die Fragmente

Die größere Bildschirmgrößen finden Sie auf den meisten Tablet-PCs eine zusätzliche Ebene der Komplexität, Android-Entwicklung hinzugefügt – ein Layout für den kleine Bildschirm nicht notwendigerweise auch für größere Bildschirme und umgekehrt funktioniert entwickelt. Um die Anzahl der Komplikationen zu reduzieren, die dies eingeführt, Android 3.0 zwei neue Features hinzugefügt, *Fragmente* und *Supportpakete*.

Fragmente können als Benutzer Schnittstellenmodule betrachtet werden. Sie können den Entwickler, die die Benutzeroberfläche in isolierten, wiederverwendbare Teile zu unterteilen, die in separaten Aktivitäten ausgeführt werden kann. Zur Laufzeit entscheidet der Aktivitäten der Fragmente zu verwenden.

Supportpakete aufgerufen wurden ursprünglich *Kompatibilität Bibliotheken* dürfen von Fragmenten auf Geräten verwendet werden, auf denen Versionen von Android vor Android 3.0 (API-Ebene 11) ausgeführt.

Die folgende Abbildung veranschaulicht z. B. wie eine einzelne Anwendung unterschiedliche Gerät Formfaktoren Fragmente vewendet.

[![Diagramm der Fragmente Tablets und Handsets Verwendung](images/00.png)](images/00.png#lightbox)

*Fragment ein* enthält eine Liste während *Fragment B* enthält Details für ein Element in der Liste ausgewählt. Wenn die Anwendung auf einem Tablet-PC ausgeführt wird, können sie beiden Fragmenten auf der gleichen Aktivität angezeigt. Wenn dieselbe Anwendung auf einem Mobilgerät (mit seiner kleinere Bildschirmgröße) ausgeführt wird, werden die Fragmente in zwei separaten Aktivitäten gehostet. Fragment A und B Fragment für beide Formfaktoren identisch sind, aber die Aktivitäten, die sie hosten unterscheiden.

Zur Behebung einer Aktivität zu koordinieren und verwalten alle diese Fragmente eingeführt Android eine neue Klasse mit dem Namen der *FragmentManager*. Jede Aktivität verfügt über eine eigene Instanz von einem `FragmentManager` zum Hinzufügen, löschen und Suchen von Fragmente gehostet. Das folgende Diagramm veranschaulicht die Beziehung zwischen Fragmente und Aktivitäten:

[![Diagramm zur Veranschaulichung der Beziehungen zwischen Aktivitäten, Fragment-Manager und Fragmente](images/01.png)](images/01.png#lightbox)

In einigen Bezug können Fragmente als zusammengesetzte Steuerelemente oder Mini-Aktivitäten betrachtet werden. Sie bündeln, um Teile der Benutzeroberfläche in wieder verwendbaren Module, die dann unabhängig von Entwicklern in Aktivitäten verwendet werden können. Ein Fragment verfügt über eine Hierarchie anzeigen – ebenso wie eine Aktivität, aber im Gegensatz zu einer Aktivität kann es über Bildschirme freigegeben werden. Ansichten unterscheiden sich von Fragmenten, Fragmente enthalten, die ihre eigenen Lebenszyklus; Ansichten nicht der Fall ist.

Während die Aktivität auf einem Host für eine oder mehrere Fragmente ist, ist es nicht unmittelbare Kenntnis der Fragmente selbst. Ebenso kennen die Fragmente nicht direkt von anderen Fragmenten auf der hosting-Aktivität. Allerdings Fragmente und Aktivitäten bewusst werden die `FragmentManager` in ihre Aktivität. Mithilfe der `FragmentManager`, es ist möglich, dass eine Aktivität oder ein Fragment, rufen Sie einen Verweis auf eine bestimmte Instanz eines Fragments, und klicken Sie dann Methoden in dieser Instanz. Auf diese Weise können die Aktivität oder Fragmente kommunizieren und Interaktion mit anderen Fragmenten.

Dieses Handbuch enthält eine umfassende Erläuterung zur Verwendung von Fragmenten, einschließlich:

-   **Erstellen von Fragmenten** – zum Erstellen einer grundlegenden Fragment und wichtige Methoden, die implementiert werden müssen.
-   **Verwaltungs- und Transaktionen des Fragments** – wie Fragmente zur Laufzeit zu bearbeiten.
-   **Android Supportpaket** – wie die Bibliotheken zu verwenden, die es ermöglichen Fragmente, die unter älteren Versionen von Android verwendet werden.


## <a name="requirements"></a>Anforderungen

Fragmente sind im Android SDK, beginnend mit der API-Ebene 11 (Android 3.0) verfügbar, wie im folgenden Screenshot gezeigt:

[![Auswählen von API-Ebene im Android SDK Manager](images/02.png)](images/02.png#lightbox)

Fragmente sind in Xamarin.Android 4.0 und höher verfügbar. Eine Xamarin.Android Anwendung muss mindestens erforderliche API-Ebene 11 (Android 3.0) oder höher, um Fragmente zu verwenden. Das Zielframework kann in den Projekteigenschaften wie folgt festgelegt werden:

[![Festlegen der Ziel-Framework-API-Ebene in der Projektoptionen](images/03-sml.png)](images/03.png#lightbox)

Es ist möglich, Fragmente in früheren Versionen von Android mithilfe des Android Supportpaket und Xamarin.Android 4.2 oder höher verwenden. Hierzu wird in den Dokumenten in diesem Abschnitt ausführlicher behandelt.


## <a name="related-links"></a>Verwandte Links

- [Wabe Gallery (Beispiel)](https://developer.xamarin.com/samples/monodroid/HoneycombGallery)
- [Fragmente](http://developer.android.com/guide/topics/fundamentals/fragments.html)
- [Supportpaket](http://developer.android.com/sdk/compatibility-library.html)
- [MOTODEV Webinar: Einführung in Fragmente](http://motodev.adobeconnect.com/p9h1aqk3ttn/)
