---
title: ViewPager
description: ViewPager handelt es sich um ein Layout-Manager, der gestural Navigation implementiert werden kann. Gestural Navigation kann der Benutzer streichen Sie nach links und rechts zum schrittweisen Durchlaufen von Datenseiten. Dieses Handbuch erläutert, wie gestural ViewPager, Navigation mit und ohne Fragmente implementiert. Außerdem wird zum Hinzufügen von Seite Indikatoren mit PagerTitleStrip und PagerTabStrip beschrieben.
ms.prod: xamarin
ms.assetid: D42896C0-DE7C-4818-B171-CB2D5E5DD46A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: bd687175048bb6a19dde21e66619667511a76796
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
ms.locfileid: "30769002"
---
# <a name="viewpager"></a>ViewPager

_ViewPager handelt es sich um ein Layout-Manager, der gestural Navigation implementiert werden kann. Gestural Navigation kann der Benutzer streichen Sie nach links und rechts zum schrittweisen Durchlaufen von Datenseiten. Dieses Handbuch erläutert, wie gestural ViewPager, Navigation mit und ohne Fragmente implementiert. Außerdem wird zum Hinzufügen von Seite Indikatoren mit PagerTitleStrip und PagerTabStrip beschrieben._

 
## <a name="overview"></a>Übersicht

Ein häufiges Szenario, in der app-Entwicklung ist die Anforderung, dass Benutzer gestural Navigation zwischen gleichgeordneten Ansichten bereitgestellt. Bei dieser Vorgehensweise Kundenkarte der Benutzer nach links oder rechts, um Zugriff auf Seiten von Inhalten (z. B. im Setup-Assistenten oder in einer Diashow). Sie können diese Wischen Ansichten erstellen, mit der `ViewPager` Widget "", verfügbar im [Android Unterstützungsbibliothek v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/). Die `ViewPager` wird ein Layout-Widget setzt sich aus mehreren untergeordneten Ansichten, in dem jedes untergeordnete Ansicht eine Seite im Layout besteht: 

[![Screenshots der TreePager-app mit horizontalen Streifen-Beispiel](images/01-intro-sml.png)](images/01-intro.png#lightbox)

In der Regel `ViewPager` dient in Verbindung mit [Fragmente](https://developer.xamarin.com/guides/android/platform_features/fragments/), aber es gibt einige Situationen, in denen Sie möglicherweise verwenden möchten `ViewPager` ohne komplexere `Fragment`s.

`ViewPager` verwendet ein Muster für die Adapter diesen angeben, die Ansichten anzuzeigen. Der hier verwendete Adapter gleicht konzeptionell verwendet [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md) &ndash; Sie angeben, dass eine Implementierung der `PagerAdapter` zum Generieren der Seiten, die die `ViewPager` zeigt dem Benutzer an. Die Seiten angezeigt werden, indem `ViewPager` kann `View`s oder `Fragment`s. Wenn `View`s werden angezeigt, die Adapter Unterklassen Android des `PagerAdapter` Basisklasse. Wenn `Fragment`s werden angezeigt, die Adapter Unterklassen Android des `FragmentPagerAdapter`. Enthält die Bibliothek für Android, unterstützen auch `FragmentPagerAdapter` (eine Unterklasse von `PagerAdapter`) können mit den Details der Verbindung `Fragment`s an Daten. 

Dieses Handbuch werden beide Ansätze veranschaulicht: 

-   In [Viewpager mit Sichten](~/android/user-interface/controls/view-pager/viewpager-and-views.md), [TreePager](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager/) app wurde entwickelt, um die Verwendung von `ViewPager` Sichten ein Struktur-Katalog (einer imagekatalog Laubwerfender und-evergreen Strukturen) anzuzeigen. 
    `PagerTabStrip`  und `PagerTitleStrip` werden verwendet, um den Titel angezeigt, mit denen diverse Seitennavigation.

-   In [Viewpager mit Fragmenten](~/android/user-interface/controls/view-pager/viewpager-and-fragments.md), eine etwas komplexere [FlashCardPager](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager/) app wurde entwickelt, um die Verwendung von `ViewPager` mit `Fragment`s, um eine app zu erstellen, in dem Mathematikaufgaben als dargestellt. Flash-Karten und auf Benutzereingaben reagiert. 


## <a name="requirements"></a>Anforderungen

Mit `ViewPager` in app-Projekts, installieren Sie die [Android Unterstützungsbibliothek v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) Paket. Weitere Informationen zum Installieren von NuGet-Pakete finden Sie unter [Exemplarische Vorgehensweise: einschließlich eines NuGet in Ihrem Projekt](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough). 

 
## <a name="architecture"></a>Architektur

Drei Komponenten werden zum Implementieren der gestural Navigation mit `ViewPager`:

-   ViewPager
-   Adapter
-   Pager-Indikator

Jede dieser Komponenten wird im folgenden zusammengefasst.



### <a name="viewpager"></a>ViewPager

`ViewPager` ist ein Layout-Manager, die eine Auflistung von zeigt `View`s einer zu einem Zeitpunkt. Ihre Aufgabe besteht darin die Benutzeraktion Streifen zu erkennen, und navigieren Sie zur nächsten oder vorherigen Ansicht nach Bedarf. Beispielsweise der folgende Screenshot zeigt einen `ViewPager` Übergang aus einem Bild zur nächsten als Antwort auf eine Benutzeraktion: 

[![Nahaufnahme der TreePager-app, die einen Übergang zwischen Ansichten anzeigen](images/02-transition-sml.png)](images/02-transition.png#lightbox)


### <a name="adapter"></a>Adapter

`ViewPager` Ruft die Daten aus einer *Adapter*. Auftrag des Adapters ist die Erstellung der `View`s angezeigt wird die `ViewPager`, diese nach Bedarf bereitstellen. Das folgende Diagramm veranschaulicht dieses Konzept &ndash; der Adapter erstellt und füllt `View`s und bietet ihnen die `ViewPager`. Als die `ViewPager` erkennt die Benutzergesten Wischen sie fragt den Adapter die entsprechende bereitstellen `View` angezeigt: 

[![Diagramm zur Veranschaulichung, wie der Adapter mit dem ViewPager Bilder und Namen verbunden](images/03-adapter-sml.png)](images/03-adapter.png#lightbox)

In diesem speziellen Beispiel jede `View` aus einer Struktur-Abbild und einen Strukturnamen erstellt wird, vor der Übergabe an die `ViewPager`. 



### <a name="pager-indicator"></a>Pager-Indikator

`ViewPager` kann verwendet werden, um ein großes Dataset anzeigen (z. B. ein Image-Katalog möglicherweise Hunderte von Images enthalten). Können Benutzer große Datasets zu navigieren `ViewPager` wird häufig zusammen mit einer *Pager Indikator* , die eine Zeichenfolge anzeigt. Die Zeichenfolge kann den Image-Titel, eine Beschriftung oder einfach der aktuellen Ansicht Position innerhalb des Datasets sein. 

Stehen zwei Ansichten, die diese Navigationsinformationen für Sie erstellt werden können: `PagerTabStrip` und `PagerTitleStrip.` zeigt jeweils eine Zeichenfolge am Anfang einer `ViewPager`, und jede zieht die Daten aus der `ViewPager`des Adapter so, dass die It stets mit synchron bleibt der aktuell angezeigten `View`. Der Unterschied besteht darin, die `PagerTabStrip` enthält einen visuellen Indikator für die "Aktuell" Zeichenfolge beim `PagerTitleStrip` ist nicht (wie im folgenden Screenshots dargestellt): 

[![Screenshots der app TreePager mit PagerTitleStrip und PagerTabStrip](images/04-comparison-sml.png)](images/04-comparison.png#lightbox)

Diese Anleitung wird veranschaulicht, wie Immplement `ViewPager`, Adapter und Indikator app-Komponenten sowie zu integrieren, um die gestural Navigation zu unterstützen. 



## <a name="related-links"></a>Verwandte Links

- [TreePager (Beispiel)](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager)
- [FlashCardPager (sample)](https://developer.xamarin.com/samples/monodroid/UserInterface/FlashCardPager)
