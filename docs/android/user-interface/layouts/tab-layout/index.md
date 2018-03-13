---
title: Im Registerkartenformat Layouts
description: "Einen Überblick über die im Registerkartenformat Layouts in Android"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1CFF590A-AC86-C3B3-36CA-A70248BC7F97
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/23/2017
ms.openlocfilehash: 02a425c8276524accc088b53c1099e7c2e28d828
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="tabbed-layouts"></a>Im Registerkartenformat Layouts


## <a name="overview"></a>Übersicht

Registerkarten sind ein beliebter Benutzer Schnittstelle Muster in mobilen Anwendungen aufgrund ihrer Einfachheit und Verwendbarkeit. Sie bieten eine konsistente und einfache Möglichkeit zum Navigieren zwischen verschiedenen Bildschirmen in einer Anwendung. Android bietet mehrere APIs für im Registerkartenformat Schnittstellen: 

-   **TabHost** &ndash; Dies ist die ursprüngliche API zum Erstellen von Benutzeroberflächen im Registerkartenformat. Ein `TabHost` Widget wird hinzugefügt, ein Layout und fungiert als Container für Ansichten im Registerkartenformat. Diese API ist inzwischen veraltet und dessen Verwendung wird abgeraten. 

-   **ActionBar** &ndash; Dies ist Teil der einen neuen Satz von APIs, die in Android 3.0 (API-Ebene 11) mit Ziel bietet eine einheitliche eingeführte Navigations- und Ansicht-switching-Schnittstelle. Es wurde wieder zu Android 2.2 (API-Ebene 8) mit portiert wurde die [Android Unterstützungsbibliothek v7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/). 

-   **PagerTabStrip** &ndash; gibt die aktuelle, nächsten und vorhergehenden Seiten eine `ViewPager`. `ViewPager` steht nur über [Android Unterstützungsbibliothek v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/).
     Weitere Informationen zu `PagerTabStrip`, finden Sie unter [ViewPager](~/android/user-interface/controls/view-pager/index.md).

-   **Symbolleiste** &ndash; `Toolbar` ist eine neuere und flexiblere Aktion Leiste-Komponente, die ersetzt `ActionBar`. `Toolbar` in Lollipop für Android 5.0 oder höher, verfügbar ist und es steht auch für ältere Versionen von Android, über die [Android Unterstützungsbibliothek v7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) NuGet-Paket. 
    `Toolbar` ist derzeit die empfohlene Aktion Leiste-Komponente, die in der Android-apps verwendet.
    Weitere Informationen finden Sie unter [Symbolleiste](~/android/user-interface/controls/tool-bar/index.md). 


Diese APIs visuell sehr unterschiedlich sind, und sind nicht miteinander kompatibel. Im folgenden Bildschirm Abbildung `TabHost` und `ActionBar` Side-by-Side: 

![Screenshots der TabHost auf der linken Seite und ActionBar auf der rechten Seite](images/image01.png)

Diese inkompatiblen API vorhanden aufgrund von erheblichen Änderungen der Benutzeroberfläche seit Android 3.0 (API-Ebene 11). Wurde eines dieser Änderungen der Benutzeroberfläche der [Aktion Strich Entwurfsmuster](http://www.androidpatterns.com/uap_pattern/action-bar), ein Musters einfachen Zugriff auf die Navigation und Schlüssel-Funktionalität in einer Anwendung bereitstellen soll. Die `ActionBar` API wurde eingeführt, um dieses Muster unterstützen. 

Die `ActionBar` -API ist einfacher und wohl bietet eine bessere benutzererfahrung. Es wurden wieder auf Android 2.2 portiert und ist die bevorzugte Lösung für Xamarin.Android Anwendungen. 

Die `TabHost` -API kompatibel ist, in allen Versionen von Android jedoch aufwändiger verwendet und ist nicht konsistent mit dem aktuellen [Android Richtlinien zur Benutzeroberfläche](http://developer.android.com/design/index.html). Entwickler sind davon abgeraten, diese API verwenden und sollte die neuere ActionBar für Xamarin.Android-Anwendungen zu fördern. 



## <a name="actionbarsherlock"></a>ActionBarSherlock

Bevor die ActionBar-API mehr auf Android 2.2, Entwickler wurden, sollte das neuere Aussehen und Verhalten der ActionBar-API, aber konnte eine Bibliothek eines Drittanbieters verwenden [ActionBarSherlock](http://actionbarsherlock.com). ActionBarSherlock ist eine Erweiterung der Android-Unterstützungsbibliothek Backport der Aktion Leiste Entwurfsmuster Android 2.x entworfen. Bei der Ausführung von Android 3.0 oder höher verwenden ActionBarSherlock automatisch das systemeigene `ActionBar` von Android bereitgestellten-API. Ältere Versionen von Android verwenden eine benutzerdefinierte Implementierung, die das Aussehen und Verhalten von der neueren imitieren wird `ActionBar` API. Die [ActionBarSherlock Komponente](https://www.nuget.org/packages/xamstore-XamarinActionBarSherlock/) erleichtert es einer Anwendung Xamarin.Android ActionBarSherlock hinzu. 



## <a name="related-links"></a>Verwandte Links

- [TabHost-Übersicht](tab-host.md)
- [TabHost Exemplarische Vorgehensweise](~/android/user-interface/layouts/tab-layout/creating-a-tabbed-ui.md)
- [ActionBar](http://developer.android.com/guide/topics/ui/actionbar.html)
- [NuGet-Paket für Android, unterstützen Bibliothek v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [V7 Appcompat-Bibliothek](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
