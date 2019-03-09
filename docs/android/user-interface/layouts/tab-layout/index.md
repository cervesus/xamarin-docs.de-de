---
title: Layouts im Registerkartenformat
description: Eine Übersicht über Layouts im Registerkartenformat in Android
ms.prod: xamarin
ms.assetid: 1CFF590A-AC86-C3B3-36CA-A70248BC7F97
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/08/2017
---

# <a name="tabbed-layouts"></a>Layouts im Registerkartenformat


## <a name="overview"></a>Übersicht

Registerkarten sind eine beliebte User Interface-Antimuster in mobilen Anwendungen aufgrund ihrer Einfachheit und benutzerfreundlichkeit. Sie bieten eine konsistente und einfache Möglichkeit zum Navigieren zwischen verschiedenen Bildschirme in einer Anwendung. Android hat mehrere APIs mit internetskalierung für im Registerkartenformat Schnittstellen: 

-   **ActionBar** &ndash; Dies ist Teil der einen neuen Satz von APIs, die in Android 3.0 (API-Ebene 11), mit dem Ziel eingeführt wurde für die Bereitstellung eines konsistenten Navigation und Ansicht-switching-Schnittstelle. Es ist wieder auf Android 2.2 (API-Ebene 8) mit portiert wurde die [Android Support-Bibliothek v7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/). 

-   **PagerTabStrip** &ndash; gibt die aktuelle, nächsten und vorherigen Seiten eine `ViewPager`. `ViewPager` steht nur über [Android Support-Bibliothek v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/).
     Weitere Informationen zu `PagerTabStrip`, finden Sie unter [ViewPager](~/android/user-interface/controls/view-pager/index.md).

-   **Symbolleiste** &ndash; `Toolbar` ist eine neuere und flexibler Aktion Leiste-Komponente, die ersetzt `ActionBar`. `Toolbar` ist in Android 5.0 Lollipop oder höher verfügbar, und es ist auch verfügbar für ältere Versionen von Android über den [Android Support-Bibliothek v7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) NuGet-Paket. 
    `Toolbar` befindet sich derzeit die empfohlene Aktion Leiste-Komponente, die in der Android-apps verwenden.
    Weitere Informationen finden Sie unter [Symbolleiste](~/android/user-interface/controls/tool-bar/index.md). 



## <a name="related-links"></a>Verwandte Links

- [Material Design - Registerkarten](https://material.io/guidelines/components/tabs.html)- [ActionBar](https://developer.android.com/guide/topics/ui/actionbar.html)
- [NuGet-Paket für Android-Unterstützung Bibliothek v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [V7 Appcompat-Bibliothek](https://developer.android.com/tools/support-library/features.html#v7-appcompat)
