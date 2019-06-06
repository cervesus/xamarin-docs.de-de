---
title: GridViewPager
ms.prod: xamarin
ms.assetid: A1CDD5F0-049B-4DFA-A268-8A875D26A675
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/02/2018
ms.openlocfilehash: 12e7e31cda9818a3cb2e2efc331a0be5d0c334e5
ms.sourcegitcommit: d3f48bfe72bfe03aca247d47bc64bfbfad1d8071
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/06/2019
ms.locfileid: "66740850"
---
# <a name="gridviewpager"></a>GridViewPager

Die [GridViewPager](https://developer.xamarin.com/samples/monodroid/wear/GridViewPager/) Beispiel veranschaulicht, wie das Direct2D-Auswahl-navigationsmuster für Android Wear zu implementieren.

![Beispielscreenshot der GridViewPager auf einem quadratischen display](gridviewpager-images/gridviewpager.png)

Fügen Sie zuerst die [Xamarin Android Wear-Unterstützung](https://www.nuget.org/packages/Xamarin.Android.Wear/) NuGet-Paket Ihrem Projekt.

Das Layout XML sieht so aus:

```xml
<android.support.wearable.view.GridViewPager xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/pager"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:keepScreenOn="true" />
```

Erstellen Sie eine [`GridPagerAdapter`](https://developer.android.com/reference/android/support/wearable/view/GridPagerAdapter.html)
(oder wie z. B.-Unterklasse [`FragmentGridPagerAdapter`](https://developer.android.com/reference/android/support/wearable/view/FragmentGridPagerAdapter.html)
zum Bereitstellen von Ansichten auf als der Benutzer navigiert.

Die [beispieladapter](https://github.com/xamarin/monodroid-samples/blob/master/wear/GridViewPager/GridViewPager/SimpleGridPagerAdapter.cs) veranschaulicht das Implementieren Sie der erforderlichen Methoden, einschließlich Außerkraftsetzungen für `RowCount`, `GetColumnCount`, `GetBackground`, und `GetFragment`

Einrichten des Adapters verknüpfen Sie, wie gezeigt:

```csharp
pager.Adapter = new SimpleGridPagerAdapter (this, FragmentManager);
```



## <a name="related-links"></a>Verwandte Links

- [Google 2D-Auswahl-doc](https://developer.android.com/training/wearables/ui/2d-picker.html)
- [Android.Support.wearable-Dokumentation](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)
- [GridViewPager (Beispiel)](https://developer.xamarin.com/samples/monodroid/wear/GridViewPager/)
