---
title: Arbeiten mit Bildschirmgrößen in Xamarin.Android- und Abnutzung des Betriebssystems
ms.prod: xamarin
ms.assetid: 77831169-C663-4D42-B742-B8B556B1DA4B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/25/2018
ms.openlocfilehash: 9fc22a3c08b60a8474b006f1c9225155b9705507
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50113117"
---
# <a name="working-with-screen-sizes"></a>Arbeiten mit Bildschirmen

Android Wear-Geräte haben entweder ein rechteckiges oder eine runde Anzeige, die unterschiedlichen Größen kann auch sein.

![Screenshots der rechteckigen und runden Wear wird angezeigt.](screen-sizes-images/moyeu-wear.png)

## <a name="identifying-screen-type"></a>Identifizieren von Bildschirmtyps

Wear-Support-Bibliothek enthält einige Steuerelemente, die Ihnen helfen zu erkennen und Anpassung verschiedene Bildschirm-Formen, wie z. B. `WatchViewStub` und `BoxInsetLayout`.

Beachten Sie, dass einige der anderen Clientbibliotheks-Steuerelemente unterstützen (z. B. `GridViewPager`) *automatisch* Bildschirm Form selbst zu erkennen und sollte nicht hinzugefügt werden, wie im folgenden untergeordneten Elemente der Steuerelemente beschrieben.

### <a name="watchviewstub"></a>WatchViewStub

Finden Sie unter den [WatchViewStub](https://developer.xamarin.com/samples/WatchViewStub/) sehen Sie, wie Bildschirmtyps erkennen und ein anderes Layout für die einzelnen Typen angezeigt.

Die wichtigsten Layoutdatei enthält eine `android.support.wearable.view.WatchViewStub` das verweist verschiedene Layouts für rechteckige und runden Bildschirme, die mithilfe der `app:rectLayout` und `app:roundLayout` Attribute:

```xml
<android.support.wearable.view.WatchViewStub
    xmlns:app="http://schemas.android.com/apk/res-auto"
  android:layout_width="match_parent"
  android:layout_height="match_parent"
  android:id="@+id/stub"
  app:rectLayout="@layout/rect_layout"
  app:roundLayout="@layout/round_layout" />
```

Die Lösung enthält verschiedene Layouts für jedes Format, das zur Laufzeit aktiviert wird:

![Dateien, die unter Ressourcen/Layout angezeigt](screen-sizes-images/solution.png)


### <a name="boxinsetlayout"></a>BoxInsetLayout

Statt verschiedene Layouts für jeden Bildschirm erstellen, können Sie auch eine einzelne Ansicht erstellen, die an die rechteckige oder round Bildschirme anpasst.

Dies [Google Beispiel](https://developer.android.com/training/wearables/ui/layouts.html#same-layout) zeigt, wie die `BoxInsetLayout` dasselbe Layout auf rechteckige und runden Bildschirmen verwenden.


## <a name="wear-ui-designer"></a>Wear-Benutzeroberflächen-Designer

Der Xamarin Android-Designer unterstützt sowohl für rechteckige als auch für round-Bildschirme:

![Wählen den Android Wear-Quadrat-Bildschirm im Xamarin Android Designer](screen-sizes-images/design-screen-type.png)

Die Entwurfsoberfläche im rechteckigen Stil ist hier dargestellt:

![Die Entwurfsoberfläche im rechteckigen Stil](screen-sizes-images/design-rect.png) 

Die Entwurfsoberfläche im round-Stil ist hier dargestellt:

![Die Entwurfsoberfläche im round-Stil](screen-sizes-images/design-round.png)


## <a name="wear-simulator"></a>Wear-Simulator

Die **Google Emulator Manager** Gerätedefinitionen enthält, für beide Typen Bildschirm. Sie können rechteckige und runde von Emulatoren zum Testen Ihrer app erstellen.

![Wear Gerätedefinitionen gezeigt in den Google Emulator Manager](screen-sizes-images/emulator-devices.png)

Der Emulator wird für einen rechteckigen Bildschirm wie folgt gerendert:

![Emulator einen rechteckigen Bildschirm rendern](screen-sizes-images/recipe-2.png) 

Für ein runder Bildschirm wird wie folgt ausgegeben:

![Emulator ein runder Bildschirm rendern](screen-sizes-images/recipe-2-round.png)

## <a name="video"></a>Video

[Vollbild-apps für Android Wear](https://www.youtube.com/watch?v=naf_WbtFAlY) aus [developers.google.com](https://www.youtube.com/channel/UC_x5XG1OV2P6uZZ5FSM9Ttw).

