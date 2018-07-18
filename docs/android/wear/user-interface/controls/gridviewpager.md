---
title: GridViewPager
ms.prod: xamarin
ms.assetid: A1CDD5F0-049B-4DFA-A268-8A875D26A675
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/02/2018
ms.openlocfilehash: 3a0b1ec9359b1c6067c253b4d04126dbdd726cc5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
ms.locfileid: "30763435"
---
# <a name="gridviewpager"></a>GridViewPager

Die [GridViewPager](https://developer.xamarin.com/samples/GridViewPager/) Beispiel wird veranschaulicht, wie die Auswahl einer 2D Navigation-Muster für Android Dach implementiert.

![Beispiel-Screenshot des GridViewPager auf eine quadratische Anzeige](gridviewpager-images/gridviewpager.png)

Fügen Sie zuerst die [Dach Xamarin Android, unterstützen](http://www.nuget.org/packages/Xamarin.Android.Wear/) NuGet-Paket zum Projekt.

Das Layout sieht XML wie folgt:

```xml
<android.support.wearable.view.GridViewPager xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/pager"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:keepScreenOn="true" />
```

Erstellen einer [ `GridPagerAdapter` ](http://developer.android.com/reference/android/support/wearable/view/GridPagerAdapter.html) (oder Unterklassen wie z. B. [ `FragmentGridPagerAdapter` ](http://developer.android.com/reference/android/support/wearable/view/FragmentGridPagerAdapter.html) Ansichten, um anzuzeigen, wie der Benutzer navigiert angeben.

Die [beispieladapter](https://github.com/xamarin/monodroid-samples/blob/master/wear/GridViewPager/GridViewPager/SimpleGridPagerAdapter.cs) wird gezeigt, wie die erforderlichen Methoden, einschließlich Außerkraftsetzungen für implementieren `RowCount`, `GetColumnCount`, `GetBackground`, und `GetFragment`

Verknüpfen des Adapters wie gezeigt:

```csharp
pager.Adapter = new SimpleGridPagerAdapter (this, FragmentManager);
```



## <a name="related-links"></a>Verwandte Links

- [Google 2D Datumsauswahl doc](https://developer.android.com/training/wearables/ui/2d-picker.html)
- [Android.Support.wearable docs](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)
- [GridViewPager (Beispiel)](https://developer.xamarin.com/samples/GridViewPager/)
