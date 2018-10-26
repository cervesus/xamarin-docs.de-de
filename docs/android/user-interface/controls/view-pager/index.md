---
title: ViewPager
description: ViewPager handelt es sich um ein Layout-Manager, der gestural Navigation implementiert werden kann. Gestural Navigation kann der Benutzer Wischen nach links und rechts zum schrittweisen Durchlaufen von Datenseiten. Dieses Handbuch wird erläutert, wie gestural Navigation mit ViewPager, mit und ohne die Fragmente zu implementieren. Es wird beschrieben, wie mithilfe von PagerTitleStrip und PagerTabStrip Seite-Indikatoren hinzufügen.
ms.prod: xamarin
ms.assetid: D42896C0-DE7C-4818-B171-CB2D5E5DD46A
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: bb9795eb1e77a48b01556c553ae19613d6ab6de6
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50115938"
---
# <a name="viewpager"></a>ViewPager

_ViewPager handelt es sich um ein Layout-Manager, der gestural Navigation implementiert werden kann. Gestural Navigation kann der Benutzer Wischen nach links und rechts zum schrittweisen Durchlaufen von Datenseiten. Dieses Handbuch wird erläutert, wie gestural Navigation mit ViewPager, mit und ohne die Fragmente zu implementieren. Es wird beschrieben, wie mithilfe von PagerTitleStrip und PagerTabStrip Seite-Indikatoren hinzufügen._

 
## <a name="overview"></a>Übersicht

In der Appentwicklung ein gängiges Szenario ist die Notwendigkeit, Benutzern gestural Navigation zwischen gleichgeordneten Ansichten bereitzustellen. Bei diesem Ansatz mit einer wischbewegung der Benutzer nach links oder rechts, um Zugriff auf Seiten des Inhalts (z. B. in einem Setup-Assistenten oder einer Bildschirmpräsentation nacheinander anzuzeigen). Sie können diese Wischen Ansichten erstellen, mit der `ViewPager` Widget, verfügbar in [Android Support-Bibliothek v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/). Die `ViewPager` ist ein Layout-Widget setzt sich aus mehreren untergeordneten Ansichten, in dem jeder untergeordneten Ansicht eine Seite in das Layout besteht: 

[![Screenshots der TreePager-app mit horizontalen Wischen-Beispiel](images/01-intro-sml.png)](images/01-intro.png#lightbox)

In der Regel `ViewPager` dient in Verbindung mit [Fragmente](https://developer.xamarin.com/guides/android/platform_features/fragments/), aber es gibt einige Situationen, in denen Sie möglicherweise verwenden möchten `ViewPager` ohne die zusätzliche Komplexität der `Fragment`s.

`ViewPager` verwendet eine Muster für die Adapter der anzuzeigenden Ansichten bereitstellen. Hier verwendeten Adapters ist vom Konzept her ähnelt, die von verwendet [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md) &ndash; Sie angeben, dass eine Implementierung von `PagerAdapter` zum Generieren der Seiten, die die `ViewPager` zeigt dem Benutzer an. Die Seiten angezeigt werden, indem `ViewPager` kann `View`s oder `Fragment`s. Wenn `View`s werden angezeigt, der Adapter Unterklassen Android `PagerAdapter` Basisklasse. Wenn `Fragment`s werden angezeigt, der Adapter Unterklassen Android des `FragmentPagerAdapter`. Die Android Support-Bibliothek enthält auch `FragmentPagerAdapter` (eine Unterklasse von `PagerAdapter`) können mit den Details der Verbindung `Fragment`s, um Daten. 

Diese Anleitung veranschaulicht beide Ansätze: 

-   In [Viewpager mit Ansichten](~/android/user-interface/controls/view-pager/viewpager-and-views.md), [TreePager](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager/) app entwickelt wurde veranschaulichen `ViewPager` Ansichten eines Katalogs Struktur (einen imagekatalog Laubwerfender Erstellung langlebiger und dauerhafter Strukturen) anzeigen. 
    `PagerTabStrip`  und `PagerTitleStrip` werden verwendet, um den Titel angezeigt, die bei der Seitennavigation unterstützen.

-   In [Viewpager mit Fragmenten](~/android/user-interface/controls/view-pager/viewpager-and-fragments.md), eine etwas komplexere [FlashCardPager](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager/) app entwickelt wurde veranschaulichen `ViewPager` mit `Fragment`s, um eine app erstellen, in dem Mathematikaufgaben als dargestellt Flash-Karten und reagiert auf Benutzereingaben. 


## <a name="requirements"></a>Anforderungen

Mit `ViewPager` in Ihrem app-Projekt müssen Sie installieren die [Android Support-Bibliothek v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) Paket. Weitere Informationen zum Installieren von NuGet-Pakete finden Sie unter [Exemplarische Vorgehensweise: Einschließen eines NuGet in Ihrem Projekt](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough). 

 
## <a name="architecture"></a>Architektur

Drei Komponenten dienen zum Implementieren von gestural Navigation mit `ViewPager`:

-   ViewPager
-   Adapter
-   Pager-Indikator

Jede dieser Komponenten wird unten zusammengefasst.



### <a name="viewpager"></a>ViewPager

`ViewPager` wird ein Layout-Manager, der eine Auflistung von anzeigt `View`ehört zu einem Zeitpunkt. Seine Aufgabe besteht darin streifbewegung des Benutzers zu erkennen, und navigieren Sie zur nächsten oder vorherigen Ansicht nach Bedarf. Der folgende Screenshot zeigt beispielsweise, wie eine `ViewPager` der Übergang aus einem Bild zur nächsten als Reaktion auf eine Benutzeraktion: 

[![Nahaufnahme der TreePager-app, die einen Übergang zwischen Ansichten anzeigen](images/02-transition-sml.png)](images/02-transition.png#lightbox)


### <a name="adapter"></a>Adapter

`ViewPager` Ruft die Daten aus einer *Adapter*. Der Adapter die Aufgabe ist die Erstellung der `View`s angezeigt, indem die `ViewPager`, bereit, je nach Bedarf. Das folgende Diagramm veranschaulicht dieses Konzept &ndash; der Adapter erstellt und füllt `View`s und bietet ihnen die `ViewPager`. Als die `ViewPager` erkennt wischbewegungen des Benutzers, fordert er den Adapter zu den entsprechenden `View` angezeigt: 

[![Diagramm zur Veranschaulichung, wie der Adapter Bilder und Namen mit den ViewPager verbunden wird](images/03-adapter-sml.png)](images/03-adapter.png#lightbox)

In diesem Beispiel ist jede `View` aus einem Tree-Image und einen Strukturnamen erstellt wird, vor der Übergabe an die `ViewPager`. 



### <a name="pager-indicator"></a>Pager-Indikator

`ViewPager` kann verwendet werden, um ein großes DataSet anzuzeigen (z. B. ein Image-Katalog möglicherweise Hunderte von Abbildern enthalten). Den Benutzer, die große Datasets, navigieren Sie helfen `ViewPager` wird häufig zusammen mit einer *Pager Indikator* , die eine Zeichenfolge anzeigt. Die Zeichenfolge möglicherweise den Image-Titel, eine Beschriftung oder einfach die aktuelle Ansicht Position innerhalb des Datasets. 

Umfassen zwei Ansichten, die diese Navigationsinformationen für Sie erstellt werden können: `PagerTabStrip` und `PagerTitleStrip.` jedes zeigt eine Zeichenfolge am Anfang einer `ViewPager`, jede Ruft die Daten aus der die `ViewPager`des Adapter so, dass die It verbleibt immer synchron mit der aktuell angezeigten `View`. Der Unterschied besteht darin, die `PagerTabStrip` enthält einen visuellen Indikator für die Zeichenfolge "current", während `PagerTitleStrip` nicht (wie in den folgenden Screenshots gezeigt): 

[![Screenshots der app mit PagerTitleStrip und PagerTabStrip TreePager](images/04-comparison-sml.png)](images/04-comparison.png#lightbox)

Diese Anleitung veranschaulicht, wie Immplement `ViewPager`, Adapter und Indikator app-Komponenten und integrieren Sie sie um die gestural Navigation unterstützen. 



## <a name="related-links"></a>Verwandte Links

- [TreePager (Beispiel)](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager)
- [FlashCardPager (Beispiel)](https://developer.xamarin.com/samples/monodroid/UserInterface/FlashCardPager)
