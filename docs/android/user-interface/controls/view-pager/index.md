---
title: ViewPager
description: Viewpager ist ein Layoutmanager, mit dem Sie die Gestural-Navigation implementieren können. Die Gestural-Navigation ermöglicht dem Benutzer das Schwenken von Links und rechts zum Durchlaufen von Datenseiten. In diesem Handbuch wird erläutert, wie die Gestural-Navigation mit viewpager mit und ohne Fragmente implementiert wird. Außerdem wird beschrieben, wie Seiten Indikatoren mithilfe von PagerTitleStrip und PagerTabStrip hinzugefügt werden.
ms.prod: xamarin
ms.assetid: D42896C0-DE7C-4818-B171-CB2D5E5DD46A
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: d42940c466dbde7c8528d31f395e3f86e39d7c1d
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70762364"
---
# <a name="viewpager"></a>ViewPager

_Viewpager ist ein Layoutmanager, mit dem Sie die Gestural-Navigation implementieren können. Die Gestural-Navigation ermöglicht dem Benutzer das Schwenken von Links und rechts zum Durchlaufen von Datenseiten. In diesem Handbuch wird erläutert, wie die Gestural-Navigation mit viewpager mit und ohne Fragmente implementiert wird. Außerdem wird beschrieben, wie Seiten Indikatoren mithilfe von PagerTitleStrip und PagerTabStrip hinzugefügt werden._

## <a name="overview"></a>Übersicht

Ein häufiges Szenario bei der APP-Entwicklung ist die Notwendigkeit, Benutzern eine gesturale Navigation zwischen gleich geordneten Ansichten bereitzustellen. Bei dieser Vorgehensweise wird der Benutzer nach links oder rechts zum Zugriff auf Inhaltsseiten (z. b. in einem Setup-Assistenten oder einer Folien Anzeige). Sie können diese wischen-Ansichten mithilfe des Widgets `ViewPager` erstellen, das in der [Android-Unterstützungs Bibliothek V4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)verfügbar ist. Bei `ViewPager` handelt es sich um ein layoutwidget, das aus mehreren untergeordneten Sichten besteht, wobei jede untergeordnete Ansicht eine Seite im Layout darstellt: 

[![Screenshots der treepager-App mit horizontaler wischen-Beispiel](images/01-intro-sml.png)](images/01-intro.png#lightbox)

In der Regel wird `ViewPager` zusammen mit [Fragment](~/android/platform/fragments/index.md)-Klassen verwendet. In einigen Fällen sollten Sie jedoch `ViewPager` verwenden, ohne die Anwendung durch `Fragment`-Klassen komplexer zu gestalten.

`ViewPager`verwendet ein Adapter Muster, um die anzuzeigenden Ansichten anzuzeigen. Der hier verwendete Adapter ähnelt konzeptionell der von [recyclerview](~/android/user-interface/layouts/recycler-view/index.md) &ndash; verwendeten. Sie stellen eine Implementierung von `PagerAdapter` bereit, um die Seiten zu generieren `ViewPager` , die dem Benutzer angezeigt werden. Die von `ViewPager` angezeigten Seiten können e `View`oder `Fragment`s sein. Wenn `View`s angezeigt werden, ist `PagerAdapter` die Basisklasse der Adapter Unterklassen Android. Wenn `Fragment`s angezeigt werden, sind die Adapter Unterklassen `FragmentPagerAdapter`Android. Die Android-Unterstützungs Bibliothek umfasst `FragmentPagerAdapter` auch (eine Unterklasse `PagerAdapter`von), um die Details zum Verbinden `Fragment`von s mit Daten zu unterstützen. 

In diesem Leitfaden werden beide Ansätze veranschaulicht: 

- In [viewpager mit Ansichten](~/android/user-interface/controls/view-pager/viewpager-and-views.md)wird eine [treepager](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-treepager) -app entwickelt, um zu veranschaulichen, `ViewPager` wie Sie zum Anzeigen von Ansichten eines Struktur Katalogs (einem Bildkatalog von Laub-und Evergreen-Strukturen) verwenden können. 
    `PagerTabStrip`und `PagerTitleStrip` werden zum Anzeigen von Titeln verwendet, die bei der Seitennavigation helfen.

- In [viewpager mit Fragmenten](~/android/user-interface/controls/view-pager/viewpager-and-fragments.md)wird eine etwas komplexere app " [Flash cardpager](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-flashcardpager) " entwickelt, um zu veranschaulichen, `ViewPager` wie `Fragment`mit s verwendet wird, um eine APP zu erstellen, die mathematische Probleme als Flash Karten darstellt und auf Benutzereingaben antwortet. 

## <a name="requirements"></a>Anforderungen

Um in `ViewPager` Ihrem App-Projekt zu verwenden, müssen Sie das [Android-Unterstützungs Bibliothek-V4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) -Paket installieren. Weitere Informationen zum Installieren von nuget-Paketen finden [Sie unter Exemplarische Vorgehensweise: Einschließen eines nuget-Projekts in](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)Ihr Projekt. 

## <a name="architecture"></a>Architektur

Zum Implementieren der Gestural-Navigation mit `ViewPager`werden drei Komponenten verwendet:

- ViewPager
- Adapter
- Pager-Indikator

Jede dieser Komponenten wird im folgenden zusammengefasst.

### <a name="viewpager"></a>ViewPager

`ViewPager`ist ein Layoutmanager, der jeweils eine `View`Auflistung von s anzeigt. Seine Aufgabe besteht darin, die Schwenkbewegung des Benutzers zu erkennen und nach Bedarf zur nächsten oder vorherigen Ansicht zu navigieren. Der folgende Screenshot zeigt z. b. `ViewPager` einen, der den Übergang von einem Bild zum nächsten als Reaktion auf eine Benutzer Geste ermöglicht: 

[![Schließung der treepager-APP, die einen Übergang zwischen Sichten anzeigt](images/02-transition-sml.png)](images/02-transition.png#lightbox)

### <a name="adapter"></a>Adapter

`ViewPager`Ruft seine Daten von einem *Adapter*ab. Der Adapter Auftrag besteht darin, die `View`s zu erstellen `ViewPager`, die vom angezeigt werden, und Sie bei Bedarf bereitzustellen. Im folgenden Diagramm wird dieses Konzept &ndash; veranschaulicht, das vom Adapter erstellt `View`und aufgefüllt wird und der für `ViewPager`die bereitstellt wird. Da die `ViewPager` die Streifen Bewegungen des Benutzers erkennt, wird der Adapter aufgefordert, den entsprechenden `View` für die Anzeige anzugeben: 

[![Diagramm, das veranschaulicht, wie der Adapter Bilder und Namen mit dem viewpager verbindet](images/03-adapter-sml.png)](images/03-adapter.png#lightbox)

In diesem speziellen Beispiel wird jede `View` aus einem Struktur Image und einem Struktur Namen erstellt, bevor Sie an das `ViewPager`-Feld übermittelt wird. 

### <a name="pager-indicator"></a>Pager-Indikator

`ViewPager`kann verwendet werden, um ein großes Dataset anzuzeigen (z. b. kann ein Bildkatalog Hunderte von Bildern enthalten). Um dem Benutzer beim Navigieren in großen Datasets `ViewPager` zu helfen, wird häufig von einem *Pager-Indikator* begleitet, der eine Zeichenfolge anzeigt. Die Zeichenfolge kann der Bildtitel, eine Beschriftung oder einfach die Position der aktuellen Ansicht innerhalb des Datasets sein. 

Es gibt zwei Sichten, die diese Navigationsinformationen für Sie liefern können `PagerTabStrip` : `PagerTitleStrip.` und jede zeigt eine Zeichenfolge am Anfang einer `ViewPager`an `ViewPager`, und jede ruft Ihre Daten vom Adapter ab, sodass Sie immer mit dem aktuell angezeigt `View`. Der Unterschied besteht darin, `PagerTabStrip` dass einen visuellen Indikator für die "aktuelle" `PagerTitleStrip` Zeichenfolge enthält, ohne (wie in den folgenden Screenshots gezeigt): 

[![Screenshots der treepager-App mit PagerTitleStrip und PagerTabStrip](images/04-comparison-sml.png)](images/04-comparison.png#lightbox)

In dieser Anleitung `ViewPager`wird veranschaulicht, wie Sie App-Komponenten übertragen, Adaptern und Kennzahlen und diese zur Unterstützung der Gestural-Navigation integrieren. 

## <a name="related-links"></a>Verwandte Links

- [Treepager (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-treepager)
- [Flashcardpager (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-flashcardpager)
