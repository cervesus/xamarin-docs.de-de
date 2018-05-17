---
title: Im Registerkartenformat Layouts
description: Einen Überblick über die im Registerkartenformat Layouts in Android
ms.prod: xamarin
ms.assetid: 1CFF590A-AC86-C3B3-36CA-A70248BC7F97
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/08/2017
ms.openlocfilehash: 53ed5f91583d43839e96388194aea8c0d6ac5315
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2018
---
# <a name="tabbed-layouts"></a>Im Registerkartenformat Layouts


## <a name="overview"></a>Übersicht

Registerkarten sind ein beliebter Benutzer Schnittstelle Muster in mobilen Anwendungen aufgrund ihrer Einfachheit und Verwendbarkeit. Sie bieten eine konsistente und einfache Möglichkeit zum Navigieren zwischen verschiedenen Bildschirmen in einer Anwendung. Android bietet mehrere APIs für im Registerkartenformat Schnittstellen: 

-   **ActionBar** &ndash; Dies ist Teil der einen neuen Satz von APIs, die in Android 3.0 (API-Ebene 11) mit Ziel bietet eine einheitliche eingeführte Navigations- und Ansicht-switching-Schnittstelle. Es wurde wieder zu Android 2.2 (API-Ebene 8) mit portiert wurde die [Android Unterstützungsbibliothek v7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/). 

-   **PagerTabStrip** &ndash; gibt die aktuelle, nächsten und vorhergehenden Seiten eine `ViewPager`. `ViewPager` steht nur über [Android Unterstützungsbibliothek v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/).
     Weitere Informationen zu `PagerTabStrip`, finden Sie unter [ViewPager](~/android/user-interface/controls/view-pager/index.md).

-   **Symbolleiste** &ndash; `Toolbar` ist eine neuere und flexiblere Aktion Leiste-Komponente, die ersetzt `ActionBar`. `Toolbar` in Lollipop für Android 5.0 oder höher, verfügbar ist und es steht auch für ältere Versionen von Android, über die [Android Unterstützungsbibliothek v7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) NuGet-Paket. 
    `Toolbar` ist derzeit die empfohlene Aktion Leiste-Komponente, die in der Android-apps verwendet.
    Weitere Informationen finden Sie unter [Symbolleiste](~/android/user-interface/controls/tool-bar/index.md). 



## <a name="related-links"></a>Verwandte Links

- [Material Design - Registerkarten](https://material.io/guidelines/components/tabs.html)- [ActionBar](http://developer.android.com/guide/topics/ui/actionbar.html)
- [NuGet-Paket für Android, unterstützen Bibliothek v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [V7 Appcompat-Bibliothek](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
