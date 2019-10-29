---
title: Layouts im Registerkartenformat
description: Übersicht über Layouts im Registerkarten Format in Android
ms.prod: xamarin
ms.assetid: 1CFF590A-AC86-C3B3-36CA-A70248BC7F97
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/08/2017
ms.openlocfilehash: 4ca4200d0f9036ed76e20e3a1840303e7bb3b7e3
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028787"
---
# <a name="tabbed-layouts"></a>Layouts im Registerkartenformat

## <a name="overview"></a>Übersicht

Registerkarten sind aufgrund ihrer Einfachheit und Benutzerfreundlichkeit ein gängiges Muster für die Benutzeroberfläche in mobilen Anwendungen. Sie bieten eine konsistente und einfache Möglichkeit, zwischen verschiedenen Bildschirmen in einer Anwendung zu navigieren. Android verfügt über mehrere APIs für Schnittstellen im Registerkarten Format: 

- **Aktionsleiste** &ndash; dieser Teil eines neuen Satzes von APIs, der in Android 3,0 (API-Ebene 11) eingeführt wurde, mit dem Ziel, eine konsistente Navigations-und Ansichts Wechsel Schnittstelle bereitzustellen. Es wurde wieder auf Android 2,2 (API-Ebene 8) mit der [Android-Unterstützungs Bibliothek V7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)portiert. 

- **PagerTabStrip** &ndash; gibt die aktuellen, nächsten und vorherigen Seiten einer `ViewPager`an. `ViewPager` ist nur über die [Android-Unterstützungs Bibliothek V4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)verfügbar.
     Weitere Informationen zu `PagerTabStrip`finden Sie unter [viewpager](~/android/user-interface/controls/view-pager/index.md).

- Die **Symbolleiste** &ndash; `Toolbar` ist eine neuere und flexiblere Aktionsleisten Komponente, die `ActionBar`ersetzt. `Toolbar` ist in Android 5,0 Lollipop oder höher verfügbar und auch für ältere Android-Versionen über das nuget-Paket der [Android-Unterstützungs Bibliothek V7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) verfügbar. 
    `Toolbar` ist derzeit die empfohlene Aktionsleisten Komponente zur Verwendung in Android-Apps.
    Weitere Informationen finden Sie unter [Symbolleiste](~/android/user-interface/controls/tool-bar/index.md). 

## <a name="related-links"></a>Verwandte Links

- [Material Design-Registerkarten](https://material.io/guidelines/components/tabs.html)- [Aktionsleiste](https://developer.android.com/guide/topics/ui/actionbar.html)
- [Android-Unterstützungs Bibliothek V7 AppCompat nuget-Paket](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [V7 AppCompat-Bibliothek](https://developer.android.com/tools/support-library/features.html#v7-appcompat)
