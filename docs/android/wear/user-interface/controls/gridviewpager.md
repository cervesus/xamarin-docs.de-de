---
title: GridViewPager
ms.prod: xamarin
ms.assetid: A1CDD5F0-049B-4DFA-A268-8A875D26A675
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/02/2018
ms.openlocfilehash: 5bdff0aee39375d8de296849056660f69a7d907f
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2019
ms.locfileid: "70198537"
---
# <a name="gridviewpager"></a>GridViewPager

Das [GridViewPager](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-gridviewpager) -Beispiel veranschaulicht, wie das 2D-Auswahl Navigationsmuster für Android Wear implementiert wird.

![Beispiel Bildschirm Abbildung von GridViewPager auf einer quadratischen Anzeige](gridviewpager-images/gridviewpager.png)

Fügen Sie zunächst das nuget-Paket [xamarin Android Wear-Unterstützung](https://www.nuget.org/packages/Xamarin.Android.Wear/) zu Ihrem Projekt hinzu.

Der Layout-XML-Code sieht wie folgt aus:

```xml
<android.support.wearable.view.GridViewPager xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/pager"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:keepScreenOn="true" />
```

Erstellen eines[`GridPagerAdapter`](https://developer.android.com/reference/android/support/wearable/view/GridPagerAdapter.html)
(oder Unterklasse wie[`FragmentGridPagerAdapter`](https://developer.android.com/reference/android/support/wearable/view/FragmentGridPagerAdapter.html)
zum Bereitstellen von Sichten, die angezeigt werden, wenn der Benutzer navigiert.

Der [Beispieladapter](https://github.com/xamarin/monodroid-samples/blob/master/wear/GridViewPager/GridViewPager/SimpleGridPagerAdapter.cs) zeigt, wie die erforderlichen Methoden implementiert werden, einschließlich über schreibungen `RowCount`für `GetColumnCount`, `GetBackground`, und.`GetFragment`

Richten Sie den Adapter wie gezeigt ein:

```csharp
pager.Adapter = new SimpleGridPagerAdapter (this, FragmentManager);
```



## <a name="related-links"></a>Verwandte Links

- [Die 2D-Auswahl Dokumentation von Google](https://developer.android.com/training/wearables/ui/2d-picker.html)
- [Android. Support. Wearable docs](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)
- [GridViewPager (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-gridviewpager)
