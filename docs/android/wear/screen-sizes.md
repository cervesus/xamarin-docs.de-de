---
title: Arbeiten mit Bildschirmgrößen in Xamarin.Android und Abnutzung des Betriebssystems
ms.prod: xamarin
ms.assetid: 77831169-C663-4D42-B742-B8B556B1DA4B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/25/2018
ms.openlocfilehash: 40e7850ffe239b0ede43e4d0cd3c6da08bce3a40
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/03/2018
ms.locfileid: "32436075"
---
# <a name="working-with-screen-sizes"></a>Arbeiten mit Bildschirmgrößen

Android Abnutzung-Geräte möglich, ein rechteckiges oder round Anzeige kann auch unterschiedlich lang sein.

![Screenshots rechteckige und round Abnutzung wird angezeigt.](screen-sizes-images/moyeu-wear.png)

## <a name="identifying-screen-type"></a>Identifizieren von Bildschirmtyps

Die Abnutzung Unterstützungsbibliothek stellt einige Steuerelemente, mit denen Sie erkennen, und passen Sie an anderen Bildschirm Formen, z. B. `WatchViewStub` und `BoxInsetLayout`.

Beachten Sie, dass einige der anderen Bibliothek Steuerelemente unterstützen (z. B. `GridViewPager`) *automatisch* Bildschirm Form selbst zu erkennen und darf keine untergeordneten Elemente der Steuerelemente unten beschriebener hinzugefügt werden.

### <a name="watchviewstub"></a>WatchViewStub

Finden Sie unter der [WatchViewStub](https://developer.xamarin.com/samples/WatchViewStub/) Beispiel finden Sie unter Bildschirmtyps erkennen und ein anderes Layout für die einzelnen Typen angezeigt.

Die Haupt-Layout-Datei enthält eine `android.support.wearable.view.WatchViewStub` die verweist auf unterschiedliche Layouts für rechteckige und round Bildschirme, die mithilfe der `app:rectLayout` und `app:roundLayout` Attribute:

```xml
<android.support.wearable.view.WatchViewStub
    xmlns:app="http://schemas.android.com/apk/res-auto"
  android:layout_width="match_parent"
  android:layout_height="match_parent"
  android:id="@+id/stub"
  app:rectLayout="@layout/rect_layout"
  app:roundLayout="@layout/round_layout" />
```

Die Projektmappe enthält unterschiedliche Layouts für jedes Format, das zur Laufzeit aktiviert wird:

![Dateien unter Ressourcen/Layout angezeigt](screen-sizes-images/solution.png)


### <a name="boxinsetlayout"></a>BoxInsetLayout

Anstatt verschiedene Layouts für jeden Bildschirm erstellen, können Sie auch auf rechteckigen oder runden Seiten passt eine einzige Ansicht erstellen.

Dies [Google-Beispiel](https://developer.android.com/training/wearables/ui/layouts.html#same-layout) zeigt, wie die `BoxInsetLayout` dasselbe Layout auf Bildschirme rechteckigen und round verwenden.


## <a name="wear-ui-designer"></a>Tragen Sie die UI-Designer

Xamarin Android-Designer unterstützt die rechteckige und round Bildschirme:

![Der Bildschirm Android Abnutzung Quadrat auswählen im Xamarin Android-Designer](screen-sizes-images/design-screen-type.png)

Die Entwurfsoberfläche im rechteckigen Stil wird hier gezeigt:

![Entwurfsoberfläche rechteckigen-Formats](screen-sizes-images/design-rect.png) 

Die Entwurfsoberfläche im round-Stil wird hier gezeigt:

![Die Entwurfsoberfläche im round-Format](screen-sizes-images/design-round.png)


## <a name="wear-simulator"></a>Simulator Dach

Die **Google-Emulator-Manager** Gerätedefinitionen für beide Typen Bildschirm enthält. Sie können die rechteckige und round Emulatoren zum Testen Ihrer app erstellen.

![Tragen Sie Gerätedefinitionen in der Google-Emulator-Manager angezeigt](screen-sizes-images/emulator-devices.png)

Der Emulator wird für einen rechteckigen Bildschirm wie folgt gerendert:

![Emulator einen rechteckigen Bildschirm das rendering](screen-sizes-images/recipe-2.png) 

Für einen round Bildschirm wird wie folgt gerendert:

![Emulator Rendern eines round-Bildschirms](screen-sizes-images/recipe-2-round.png)

## <a name="video"></a>Video

[Vollbild-apps für Android Abnutzung](https://www.youtube.com/watch?v=naf_WbtFAlY) aus [developers.google.com](https://www.youtube.com/channel/UC_x5XG1OV2P6uZZ5FSM9Ttw).

