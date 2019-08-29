---
title: Android Wear-Steuerelemente
ms.prod: xamarin
ms.assetid: 5B62A5F8-5E55-4B3C-BFC4-E21CDB27C08B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: db240fa91c7eb99bd628fbb04c7c9352fb615e83
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70119949"
---
# <a name="android-wear-controls"></a>Android Wear-Steuerelemente

Android Wear-Apps können viele der gleichen Steuerelemente verwenden, die bereits für reguläre Android-Apps `Button`verwendet `TextView`werden, einschließlich, und Bild drawables. Layout-Steuer `ScrollView`Elemente `LinearLayout`, einschließlich `RelativateLayout` , und, können ebenfalls verwendet werden.

Diese Seite ist mit den Android-Wear-spezifischen Steuerelementen aus der [tragbaren UI Library](https://developer.android.com/training/wearables/apps/layouts.html#UiLibrary) verknüpft, die in xamarin-Projekten über das nuget-Paket " [tragbaren Support](https://www.nuget.org/packages/Xamarin.Android.Wear/) " verfügbar ist. Diese Steuerelemente umfassen Folgendes:

- **GridViewPager** Erstellen Sie eine zweidimensionale Navigationsschnittstelle, bei der der Benutzer einen Bildlauf nach unten durchführt, um eine Auswahl zu treffen (Weitere Informationen finden Sie unter [GridViewPager):](~/android/wear/user-interface/controls/gridviewpager.md) &ndash;

    ![Beispiel Bildschirm eines GridViewPager](images/gridviewpager.png)

Andere wichtige Steuerelemente für Wear-apps sind:

- `BoxInsetLayout`(siehe [Arbeiten mit Bildschirmgrößen](~/android/wear/screen-sizes.md)),

- `WatchViewStub`(siehe [Arbeiten mit Bildschirmgrößen](~/android/wear/screen-sizes.md)),

- `CardFrame`(siehe [Android-Erstellen von Karten](https://developer.android.com/training/wearables/ui/cards.html)),

- `CardScrollView`(siehe [Android-Erstellen von Karten](https://developer.android.com/training/wearables/ui/cards.html)),

- `WearableListView`(siehe [Android](https://developer.android.com/training/wearables/ui/lists.html)-Erstellungs Listen).


## <a name="related-links"></a>Verwandte Links

- [Android. Support. Wearable docs](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)
