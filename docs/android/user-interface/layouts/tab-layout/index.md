---
title: Layouts im Registerkartenformat
description: Übersicht über Layouts im Registerkarten Format in Android
ms.prod: xamarin
ms.assetid: 1CFF590A-AC86-C3B3-36CA-A70248BC7F97
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/08/2017
ms.openlocfilehash: 1e0632eb921b25fef40b8f0483ab80d62c9e1235
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70762195"
---
# <a name="tabbed-layouts"></a>Layouts im Registerkartenformat

## <a name="overview"></a>Übersicht

Registerkarten sind aufgrund ihrer Einfachheit und Benutzerfreundlichkeit ein gängiges Muster für die Benutzeroberfläche in mobilen Anwendungen. Sie bieten eine konsistente und einfache Möglichkeit, zwischen verschiedenen Bildschirmen in einer Anwendung zu navigieren. Android verfügt über mehrere APIs für Schnittstellen im Registerkarten Format: 

- **Aktionsleiste** &ndash; Dies ist Teil eines neuen Satzes von APIs, der in Android 3,0 (API-Ebene 11) eingeführt wurde, mit dem Ziel, eine konsistente Navigations-und Ansichts Wechsel Schnittstelle bereitzustellen. Es wurde wieder auf Android 2,2 (API-Ebene 8) mit der [Android-Unterstützungs Bibliothek V7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)portiert. 

- **PagerTabStrip** Gibt die aktuellen, nächsten und vorherigen Seiten `ViewPager`eines an. &ndash; `ViewPager`ist nur über die [Android-Unterstützungs Bibliothek V4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)verfügbar.
     Weitere Informationen zu `PagerTabStrip`finden Sie unter [viewpager](~/android/user-interface/controls/view-pager/index.md).

- **Symbolleiste** ist eine neuere und flexiblere Aktionsleisten Komponente, die ersetzt `ActionBar`. &ndash; `Toolbar` `Toolbar`ist in Android 5,0 Lollipop oder höher verfügbar und auch für ältere Android-Versionen über das nuget-Paket der [Android-Unterstützungs Bibliothek V7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) verfügbar. 
    `Toolbar`ist derzeit die empfohlene Aktionsleisten Komponente zur Verwendung in Android-Apps.
    Weitere Informationen finden Sie unter [Symbolleiste](~/android/user-interface/controls/tool-bar/index.md). 

## <a name="related-links"></a>Verwandte Links

- [Material Design-](https://material.io/guidelines/components/tabs.html)- [Aktionsleiste](https://developer.android.com/guide/topics/ui/actionbar.html) für Registerkarten
- [Android-Unterstützungs Bibliothek V7 AppCompat nuget-Paket](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [V7 AppCompat-Bibliothek](https://developer.android.com/tools/support-library/features.html#v7-appcompat)
