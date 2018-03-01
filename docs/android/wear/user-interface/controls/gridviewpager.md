---
title: GridViewPager
ms.topic: article
ms.prod: xamarin
ms.assetid: A1CDD5F0-049B-4DFA-A268-8A875D26A675
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/02/2018
ms.openlocfilehash: f156d802d807c4087331ca5796b046f8f5f2fa0d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
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
