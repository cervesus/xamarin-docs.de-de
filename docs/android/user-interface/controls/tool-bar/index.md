---
title: Symbolleiste
description: 'Die Symbolleiste ist eine Komponente der Action-Leiste, die bietet mehr Flexibilität als der Standard-Aktionsleiste: eine beliebige Stelle in der app platziert werden, kann seine Größe geändert werden und ein Farbschema aus, die sich von der app-Design ist kann verwendet werden. Darüber hinaus kann jede app-Bildschirm mehrere Symbolleisten verfügen.'
ms.prod: xamarin
ms.assetid: 22EE5FBD-3240-4308-AF76-EF45D72936DE
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 2c9de4058fdaee53671e65f49ad95c3af5e127d6
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50107481"
---
# <a name="toolbar"></a>Symbolleiste

_Die Symbolleiste ist eine Komponente der Action-Leiste, die bietet mehr Flexibilität als der Standard-Aktionsleiste: eine beliebige Stelle in der app platziert werden, kann seine Größe geändert werden und ein Farbschema aus, die sich von der app-Design ist kann verwendet werden. Darüber hinaus kann jede app-Bildschirm mehrere Symbolleisten verfügen._

 
## <a name="overview"></a>Übersicht

Eine wichtige Designelement des Android-Aktivität ist eine *Aktionsleiste*. Die Aktionsleiste ist die UI-Komponente, die für die Navigation, Suche, Menüs und branding in einer Android-app ist. In der Android-Versionen vor Android 5.0 Lollipop, der Aktionsleiste (auch bekannt als die *app-Leiste*) war die empfohlene Komponente für die Bereitstellung dieser Funktionalität. 

Die `Toolbar` Widget (eingeführt in Android 5.0 Lollipop) kann als eine Generalisierung der Aktion Leiste-Schnittstelle betrachtet werden &ndash; es dient zum Ersetzen der Aktionsleiste. Die `Toolbar` kann überall in einem app-Layout verwendet werden, und es ist viel mehr als eine Aktionsleiste angepasst. Der folgende Screenshot veranschaulicht die angepasste `Toolbar` Beispiel in diesem Handbuch erstellt: 

[![Beispielscreenshot, der eine Symbolleiste mit bearbeiten, speichern und Menüelemente overflow](images/01-toolbar-sml.png)](images/01-toolbar.png#lightbox)

Es gibt einige wichtige Unterschiede zwischen der `Toolbar` und in der Aktionsleiste: 

-   Ein `Toolbar` an einer beliebigen Stelle in der Benutzeroberfläche platziert werden kann.

-   Mehrere Symbolleisten können auf demselben Bildschirm angezeigt werden.

-   Wenn Fragmenten verwendet werden, jedes Fragment haben eine eigene `Toolbar`. 

-   Ein `Toolbar` kann konfiguriert werden, nur eine partielle Breite des Bildschirms erstreckt. 

-   Da die `Toolbar` nicht gebunden ist, das Farbschema der Aktivität im Fenster Decor, kann ein visuell Farbschema haben. 

-   Im Gegensatz zu der Aktionsleiste der `Toolbar` schließt sich nicht auf ein Symbol auf der linken Seite. Die Menüs auf der rechten Seite weniger Speicher zu verwenden. 

-   Die `Toolbar` Höhe ist anpassbar. 

-   Andere Ansichten können enthalten sein, in der `Toolbar`. 

Ein `Toolbar` kann eine oder mehrere der folgenden Elemente enthalten: 

-   Die Navigationsschaltfläche "

-   Ein Bild mit Branding-logo

-   Titel und Untertitel

-   Benutzerdefinierte Ansichten

-   Menü "Aktion"

-   Überlaufmenü

Google [materialentwurfsrichtlinien](https://material.google.com/) empfiehlt die Nutzung dieser Elemente apps unterschiedliche finden (und nicht ausschließlich auf ein Anwendungssymbol und Title) zu gewähren. 

Dieser Leitfaden behandelt, die am häufigsten verwendeten `Toolbar` Szenarien:

-   Ersetzen einer Aktivität Aktionsleiste "Standard" mit einem `Toolbar`. 

-   Hinzufügen einer zweiten `Toolbar` zu einer Aktivität.

-   Mithilfe der **Android Support-Bibliothek v7 AppCompat** Bibliothek (genannt *AppCompat* im Rest dieses Handbuchs) bereitstellen `Toolbar` in früheren Versionen von Android. 

 
 
## <a name="requirements"></a>Anforderungen

`Toolbar` ist auf Android 5.0 Lollipop (API 21) und höher verfügbar. Verwenden Sie bei vor Android 5.0 für Android Versionen, die [Android Support Library v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/), die bietet rückwärtskompatibel `Toolbar` in einem NuGet-Paket zu unterstützen. 
[Kompatibilität von Symbolleisten](~/android/user-interface/controls/tool-bar/toolbar-compatibility.md) wird erläutert, wie Sie diese Bibliothek verwenden. 




## <a name="related-links"></a>Verwandte Links

- [Lollipop-Symbolleiste (Beispiel)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [AppCompat-Symbolleiste (Beispiel)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
