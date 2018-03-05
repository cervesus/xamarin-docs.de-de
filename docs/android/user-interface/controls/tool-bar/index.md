---
title: Symbolleiste
description: "Die Symbolleiste ist eine Aktion Leiste-Komponente, die bietet mehr Flexibilität als der Standard-Aktionsleiste: an einer beliebigen Stelle in der app platziert werden und seine Größe kann geändert werden, können sie ein Farbschema aus, die von der app-Design unterscheidet. Darüber hinaus kann jede app-Bildschirm mehrere Symbolleisten verfügen."
ms.topic: article
ms.prod: xamarin
ms.assetid: 22EE5FBD-3240-4308-AF76-EF45D72936DE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/22/2018
ms.openlocfilehash: cf2211a572d45b7c29018d00f36cb8408484483f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="toolbar"></a>Symbolleiste

_Die Symbolleiste ist eine Aktion Leiste-Komponente, die bietet mehr Flexibilität als der Standard-Aktionsleiste: an einer beliebigen Stelle in der app platziert werden und seine Größe kann geändert werden, können sie ein Farbschema aus, die von der app-Design unterscheidet. Darüber hinaus kann jede app-Bildschirm mehrere Symbolleisten verfügen._


<a name="overview" />
 
## <a name="overview"></a>Übersicht

Ein Schlüssel Designelement einer Android-Aktivität ist eine *Aktionsleiste*. Die Aktionsleiste ist die UI-Komponente, die für die Navigation, Suche, Menüs und branding in einer Android-app ist. In der Android-Versionen vor Android 5.0 Lollipop, der Aktionsleiste (auch bekannt als die *app-Leiste*) wurde der empfohlenen Komponenten für die Bereitstellung dieser Funktionalität. 

Die `Toolbar` Widget (eingeführt in Android 5.0 Lollipop) kann als eine Generalisierung der Aktion Leiste Schnittstelle betrachtet werden &ndash; die Aktionsleiste ersetzen soll. Die `Toolbar` überall in einem app-Layout verwendet werden kann, und es viel mehr als eine Aktionsleiste anpassbar ist. Der folgende Screenshot zeigt den angepassten `Toolbar` Beispiel in diesem Handbuch erstellt: 

[![Beispiel-Screenshot, der eine Symbolleiste mit bearbeiten, speichern und Menüelemente "Überlauf"](images/01-toolbar-sml.png)](images/01-toolbar.png)

Es gibt einige wichtige Unterschiede zwischen der `Toolbar` und der Aktionsleiste: 

-   Ein `Toolbar` an einer beliebigen Stelle in der Benutzeroberfläche platziert werden kann.

-   Mehrere Symbolleisten können auf einem Bildschirm angezeigt werden.

-   Wenn Fragmente verwendet werden, jedes Fragment kann einen eigenen besitzen `Toolbar`. 

-   Ein `Toolbar` kann konfiguriert werden, damit nur eine teilweise Breite des Bildschirms umfassen. 

-   Da die `Toolbar` nicht gebunden ist, das Farbschema der Aktivität Fenster Dekoration, kann es ein visuell Farbschema aufweisen. 

-   Im Gegensatz zu der Aktionsleiste der `Toolbar` enthält kein Symbol auf der linken Seite. Die Menüs auf der rechten Seite weniger Speicher zu verwenden. 

-   Die `Toolbar` Höhe überhaupt angepasst werden kann. 

-   Andere Sichten eingeschlossen werden können, innerhalb der `Toolbar`. 

Ein `Toolbar` kann eine oder mehrere der folgenden Elemente enthalten: 

-   Die Navigationsschaltfläche "

-   Ein Logobild Branding

-   Titel und Untertitel

-   Benutzerdefinierte Ansichten

-   Aktionsmenü

-   Menü "Überlauf"

Google [Material Entwurfsrichtlinien](https://material.google.com/) empfiehlt nutzen eines dieser Elemente aus, um apps distinct sehen (statt ausschließlich auf ein Symbol "Anwendung" und Title) zu gewähren. 

Dieser Leitfaden behandelt, die am häufigsten verwendeten `Toolbar` Szenarien:

-   Ersetzen eine Aktivität Standard Aktionsleiste mit einem `Toolbar`. 

-   Hinzufügen einer zweiten `Toolbar` zu einer Aktivität.

-   Mithilfe der **Android Unterstützungsbibliothek v7 AppCompat** Bibliothek (genannt *AppCompat* im Rest dieses Handbuchs) bereitzustellende `Toolbar` in früheren Versionen von Android. 

 
<a name="requirements" />
 
## <a name="requirements"></a>Anforderungen

`Toolbar` ist auf Android 5.0 Lollipop (API 21) und höher verfügbar. Wenn Android Zielgruppenadressierung vor Android 5.0 freigibt, verwenden die [Bibliothek für Android unterstützt v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/), bietet eine abwärtskompatibel `Toolbar` in einem NuGet-Paket zu unterstützen. 
[Symbolleiste Kompatibilität](~/android/user-interface/controls/tool-bar/toolbar-compatibility.md) wird erläutert, wie diese Bibliothek verwendet. 




## <a name="related-links"></a>Verwandte Links

- [Lollipop-Symbolleiste (Beispiel)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [AppCompat-Symbolleiste (Beispiel)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
