---
title: Fragmente
description: Fragmente, die zeigt, wie eine flexiblere Entwürfe für viele verschiedene Bildschirmgrößen finden Sie auf Smartphones und Tablets zu unterstützen, Android 3.0 eingeführt. Dieser Artikel behandelt wie Fragmente verwenden, um Xamarin.Android-Anwendungen zu entwickeln, und wie Fragmente auf vor Android 3.0 (API-Ebene 11)-Geräten unterstützt.
ms.prod: xamarin
ms.assetid: 1AFB4242-A337-F8E0-83D9-B8D850D7F384
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/15/2018
ms.openlocfilehash: bc4441c7ee0c36af990297bad1b0c2f0e77123f3
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50113286"
---
# <a name="fragments"></a>Fragmente

_Fragmente, die zeigt, wie eine flexiblere Entwürfe für viele verschiedene Bildschirmgrößen finden Sie auf Smartphones und Tablets zu unterstützen, Android 3.0 eingeführt. Dieser Artikel behandelt wie Fragmente verwenden, um Xamarin.Android-Anwendungen zu entwickeln, und wie Fragmente auf vor Android 3.0 (API-Ebene 11)-Geräten unterstützt._

## <a name="fragments-overview"></a>Übersicht über die Fragmente

Der größere der Bildschirm finden Sie auf die meisten Tablets Android-Entwicklung ein zusätzliches Maß an Komplexität hinzugefügt, ein Layout für den kleine Bildschirm nicht unbedingt auch für größere Bildschirme und umgekehrt funktioniert entwickelt. Um die Anzahl von Komplikationen zu reduzieren, die dies eingeführt, Android 3.0 zwei neue Features hinzugefügt *Fragmente* und *Supportpakete*.

Fragmente können als Schnittstelle Benutzermodul betrachtet werden. Sie können die Entwickler der Benutzeroberfläche in isolierten, wiederverwendbare Teile unterteilen, die in separaten Aktivitäten ausgeführt werden können. Zur Laufzeit entscheidet der Aktivitäten können entweder der Fragmente zu verwenden.

Supportpakete aufgerufen wurden ursprünglich *Kompatibilität Bibliotheken* und Fragmente, die auf Geräten verwendet werden, auf denen Versionen von Android vor Android 3.0 (API-Ebene 11) ausgeführt.

Die folgende Abbildung zeigt beispielsweise, wie eine einzelne Anwendung Fragmente auf verschiedenen geräteausführungen verwendet.

[![Diagramm des wie Fragmente in Tablets und -Handsets verwendet werden](images/00.png)](images/00.png#lightbox)

*Fragment ein* enthält eine Liste, während *Fragment B* enthält Details für ein Element in der Liste ausgewählt. Wenn die Anwendung auf einem Tablet PC ausgeführt wird, können sie beide Fragmente in der gleichen Aktivität anzeigen. Wenn dieselbe Anwendung auf einem Mobilgerät (mit der kleinere Bildschirmgröße) ausgeführt wird, werden die Fragmente in zwei separaten Aktivitäten gehostet. Fragment ein und Fragment B sind für beide Formfaktoren identisch, jedoch unterscheiden sich die Aktivitäten, die sie hosten.

Damit können eine Aktivität, koordinieren und verwalten alle diese Fragmente, eingeführt Android eine neue Klasse namens der *FragmentManager*. Jede Aktivität verfügt über eine eigene Instanz von einem `FragmentManager` für das Hinzufügen, löschen und Suchen von Fragmenten gehostet. Das folgende Diagramm veranschaulicht die Beziehung zwischen Fragmenten und Aktivitäten:

[![Diagramm zur Veranschaulichung der Beziehungen zwischen Aktivitäten, Fragment-Manager und Fragmente](images/01.png)](images/01.png#lightbox)

In einigen Punkten können Fragmente als zusammengesetzte Steuerelemente oder Mini-Aktivitäten betrachtet werden. Sie fassen die Teile der Benutzeroberfläche in wiederverwendbare Module, die unabhängig von Entwicklern in Aktivitäten verwendet werden können. Ein Fragment besitzt eine Hierarchie von Inhaltsansichten – genau wie eine Aktivität, aber im Gegensatz zu einer Aktivität können sie für Bildschirme genutzt werden. Ansichten unterscheiden sich in gleicher Weise von Fragmenten Fragmente enthalten, die ihre eigenen Lebenszyklus. Ansichten nicht der Fall ist.

Während die Aktivität auf einem Host auf einen oder mehrere Fragmente ist, ist es nicht direkt zur Kenntnis der Fragmente selbst. Fragmente sind ebenso nicht direkt zur Kenntnis der anderen Fragmente in der hosting-Aktivität. Fragmente und Aktivitäten sind jedoch über die `FragmentManager` in ihrer Aktivität. Mithilfe der `FragmentManager`, es ist möglich, dass eine Aktivität oder ein Fragment, rufen Sie einen Verweis auf eine bestimmte Instanz eines Fragments, und rufen Sie dann die Methoden in dieser Instanz. Auf diese Weise können die Aktivität oder Fragmente zu kommunizieren und interagieren mit anderen Fragmenten.

Dieses Handbuch enthält ausführliche Informationen zur Verwendung von Fragmenten, einschließlich:

-   **Erstellen von Fragmenten** – Vorgehensweise: Erstellen Sie eine grundlegende Fragment und Schlüsselmethoden, die implementiert werden müssen.
-   **Fragment Verwaltungs- und Transaktionen** – Gewusst wie: Bearbeiten von Fragmenten zur Laufzeit.
-   **Android-Unterstützungspakets** : wie die Bibliotheken verwenden, mit denen Fragmente, die unter älteren Versionen von Android verwendet werden.


## <a name="requirements"></a>Anforderungen

Fragmente sind verfügbar, in das Android SDK, API-Ebene 11 (Android 3.0) ab, wie im folgenden Screenshot gezeigt:

[![Wählen die API-Ebene im Android SDK-Manager](images/02.png)](images/02.png#lightbox)

Fragmente sind in Xamarin.Android 4.0 und höher verfügbar. Eine Xamarin.Android-Anwendung muss als Ziel mindestens API-Ebene 11 (Android 3.0) oder höher, um Fragmente zu verwenden. Das Zielframework kann in den Projekteigenschaften wie unten dargestellt festgelegt werden:

[![Festlegen der Ziel-Framework-API-Ebene in den Projektoptionen](images/03-sml.png)](images/03.png#lightbox)

Es ist möglich, Fragmente in älteren Versionen von Android unter Verwendung des Android-Unterstützungspakets und Xamarin.Android 4.2 oder höher verwenden. Hierzu wird in den Dokumenten in diesem Abschnitt noch ausführlicher behandelt.


## <a name="related-links"></a>Verwandte Links

- [Honeycomb-Katalog (Beispiel)](https://developer.xamarin.com/samples/monodroid/HoneycombGallery)
- [Fragmente](http://developer.android.com/guide/topics/fundamentals/fragments.html)
- [Supportpaket](http://developer.android.com/sdk/compatibility-library.html)
- [MOTODEV-Webinar: Einführung in Fragmenten](http://motodev.adobeconnect.com/p9h1aqk3ttn/)
