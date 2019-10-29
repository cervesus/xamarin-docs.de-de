---
title: ViewPager
description: Viewpager ist ein Layoutmanager, mit dem Sie die Gestural-Navigation implementieren können. Die Gestural-Navigation ermöglicht dem Benutzer das Schwenken von Links und rechts zum Durchlaufen von Datenseiten. In diesem Handbuch wird erläutert, wie die Gestural-Navigation mit viewpager mit und ohne Fragmente implementiert wird. Außerdem wird beschrieben, wie Seiten Indikatoren mithilfe von PagerTitleStrip und PagerTabStrip hinzugefügt werden.
ms.prod: xamarin
ms.assetid: D42896C0-DE7C-4818-B171-CB2D5E5DD46A
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 600a94a0ee9eb5bcf06dc19d95cf9e77132a2e81
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029055"
---
# <a name="viewpager"></a>ViewPager

_viewpager ist ein Layoutmanager, mit dem Sie die Gestural-Navigation implementieren können. Die Gestural-Navigation ermöglicht dem Benutzer das Schwenken von Links und rechts zum Durchlaufen von Datenseiten. In diesem Handbuch wird erläutert, wie die Gestural-Navigation mit viewpager mit und ohne Fragmente implementiert wird. Außerdem wird beschrieben, wie Seiten Indikatoren mithilfe von PagerTitleStrip und PagerTabStrip hinzugefügt werden._

## <a name="overview"></a>Übersicht

Ein häufiges Szenario bei der APP-Entwicklung ist die Notwendigkeit, Benutzern eine gesturale Navigation zwischen gleich geordneten Ansichten bereitzustellen. Bei dieser Vorgehensweise wird der Benutzer nach links oder rechts zum Zugriff auf Inhaltsseiten (z. b. in einem Setup-Assistenten oder einer Folien Anzeige). Sie können diese Schwenk Ansichten mit dem `ViewPager`-Widget erstellen, das in der [Android-Unterstützungs Bibliothek V4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)verfügbar ist. Der `ViewPager` ist ein layoutwidget, das aus mehreren untergeordneten Sichten besteht, wobei jede untergeordnete Ansicht eine Seite im Layout darstellt: 

[![Screenshots der treepager-App mit horizontaler wischen-Beispiel](images/01-intro-sml.png)](images/01-intro.png#lightbox)

In der Regel wird `ViewPager` zusammen mit [Fragment](~/android/platform/fragments/index.md)-Klassen verwendet. In einigen Fällen sollten Sie jedoch `ViewPager` verwenden, ohne die Anwendung durch `Fragment`-Klassen komplexer zu gestalten.

in `ViewPager` wird ein Adapter Muster verwendet, um die anzuzeigenden Ansichten anzuzeigen. Der hier verwendete Adapter ähnelt konzeptionell der von [recyclerview](~/android/user-interface/layouts/recycler-view/index.md) &ndash; Sie eine Implementierung von `PagerAdapter` bereitstellen, um die Seiten zu generieren, die `ViewPager` dem Benutzer angezeigt werden. Die von `ViewPager` angezeigten Seiten können `View`s oder `Fragment`s sein. Wenn `View`s angezeigt werden, werden die `PagerAdapter` Basisklasse der Adapter Unterklassen von Android Unterklassen angezeigt. Wenn `Fragment`s angezeigt werden, werden die `FragmentPagerAdapter`des Adapters Unterklassen von Android. Die Android-Unterstützungs Bibliothek umfasst auch `FragmentPagerAdapter` (eine Unterklasse von `PagerAdapter`), um die Details zum Verbinden von `Fragment`s mit Daten zu unterstützen. 

In diesem Leitfaden werden beide Ansätze veranschaulicht: 

- In [viewpager mit Ansichten](~/android/user-interface/controls/view-pager/viewpager-and-views.md)wird eine [treepager](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-treepager) -app entwickelt, um zu veranschaulichen, wie `ViewPager` zum Anzeigen von Ansichten eines Struktur Katalogs (einem Bildkatalog von Laub-und Evergreen-Strukturen) verwendet wird. 
    `PagerTabStrip` und `PagerTitleStrip` werden zum Anzeigen von Titeln verwendet, die bei der Seitennavigation helfen.

- In [viewpager mit Fragmenten](~/android/user-interface/controls/view-pager/viewpager-and-fragments.md)wird eine etwas komplexere app " [Flash cardpager](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-flashcardpager) " entwickelt, um zu veranschaulichen, wie `ViewPager` mit `Fragment`s verwendet wird, um eine APP zu erstellen, die mathematische Probleme als Flash Karten darstellt und auf Benutzereingaben antwortet. 

## <a name="requirements"></a>Voraussetzungen

Wenn Sie `ViewPager` im App-Projekt verwenden möchten, müssen Sie das Paket für die [Android-Unterstützungs Bibliothek V4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) installieren. Weitere Informationen zum Installieren von nuget-Paketen finden Sie unter [Exemplarische Vorgehensweise: Das Einschließen eines nuget-](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)in Ihr Projekt. 

## <a name="architecture"></a>Architektur

Zum Implementieren der Gestural-Navigation mit `ViewPager`werden drei Komponenten verwendet:

- ViewPager
- Adapter
- Pager-Indikator

Jede dieser Komponenten wird im folgenden zusammengefasst.

### <a name="viewpager"></a>ViewPager

`ViewPager` ist ein Layoutmanager, der eine Auflistung von `View`s nacheinander anzeigt. Seine Aufgabe besteht darin, die Schwenkbewegung des Benutzers zu erkennen und nach Bedarf zur nächsten oder vorherigen Ansicht zu navigieren. Der folgende Screenshot zeigt z. b. eine `ViewPager`, die als Reaktion auf eine Benutzer Geste den Übergang von einem Bild zum nächsten ermöglicht: 

[![closeup der treepager-APP, die einen Übergang zwischen Sichten anzeigt](images/02-transition-sml.png)](images/02-transition.png#lightbox)

### <a name="adapter"></a>Adapter

`ViewPager` ruft seine Daten von einem *Adapter*ab. Der Adapter Auftrag besteht darin, die `View`s zu erstellen, die vom `ViewPager`angezeigt werden, und Sie bei Bedarf bereitzustellen. Dieses Konzept wird im folgenden Diagramm veranschaulicht, &ndash; der Adapter `View`s erstellt und auffüllt und dem `ViewPager`bereitstellt. Da der `ViewPager` die Schwenk Gesten des Benutzers erkennt, fordert er den Adapter auf, die entsprechenden `View` anzugeben, die angezeigt werden sollen: 

[![Diagramm, das veranschaulicht, wie der Adapter Bilder und Namen mit dem viewpager verbindet](images/03-adapter-sml.png)](images/03-adapter.png#lightbox)

In diesem speziellen Beispiel wird jede `View` aus einem Struktur Image und einem Struktur Namen erstellt, bevor Sie an den `ViewPager`übermittelt wird. 

### <a name="pager-indicator"></a>Pager-Indikator

`ViewPager` kann verwendet werden, um ein großes Dataset anzuzeigen (z. b. kann ein Bildkatalog Hunderte von Bildern enthalten). Um dem Benutzer beim Navigieren in großen Datasets zu helfen, wird `ViewPager` häufig von einem *Pager-Indikator* begleitet, der eine Zeichenfolge anzeigt. Die Zeichenfolge kann der Bildtitel, eine Beschriftung oder einfach die Position der aktuellen Ansicht innerhalb des Datasets sein. 

Es gibt zwei Sichten, die diese Navigationsinformationen für Sie liefern können: `PagerTabStrip` und `PagerTitleStrip.` jeweils eine Zeichenfolge am oberen Rand eines `ViewPager`anzeigt, und jede Daten aus dem Adapter des `ViewPager`zieht, sodass Sie immer mit dem aktuell angezeigten synchronisiert werden `View`. Der Unterschied besteht darin, dass `PagerTabStrip` einen visuellen Indikator für die "aktuelle" Zeichenfolge enthält, während `PagerTitleStrip` nicht (wie in den folgenden Screenshots gezeigt): 

[![Screenshots der treepager-App mit PagerTitleStrip und PagerTabStrip](images/04-comparison-sml.png)](images/04-comparison.png#lightbox)

In dieser Anleitung wird veranschaulicht, wie Sie `ViewPager`-, Adapter-und Indikator-App-Komponenten immplement und diese zur Unterstützung der Gestural-Navigation integrieren. 

## <a name="related-links"></a>Verwandte Links

- [Treepager (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-treepager)
- [Flashcardpager (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-flashcardpager)
