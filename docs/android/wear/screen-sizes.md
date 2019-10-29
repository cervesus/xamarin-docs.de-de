---
title: Arbeiten mit Bildschirmgrößen in xamarin. Android und Wear-Betriebssystem
ms.prod: xamarin
ms.assetid: 77831169-C663-4D42-B742-B8B556B1DA4B
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/25/2018
ms.openlocfilehash: 86e05dc0e9cd5df325126cc5a339b36dd27c1e45
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030361"
---
# <a name="working-with-screen-sizes"></a>Arbeiten mit Bildschirmgrößen

Android Wear-Geräte können entweder eine rechteckige oder eine Runde Anzeige aufweisen, die auch unterschiedlich groß sein kann.

![Screenshots von rechteckigen und runden Verschleiß anzeigen](screen-sizes-images/moyeu-wear.png)

## <a name="identifying-screen-type"></a>Identifizieren des Bildschirmtyps

Die Bibliothek zur Unterstützung von Wear bietet einige Steuerelemente, mit denen Sie verschiedene Bildschirm Formen erkennen und anpassen können, wie z. b. `WatchViewStub` und `BoxInsetLayout`.

Beachten Sie, dass einige der anderen Steuerelemente zur Unterstützung von Bibliotheken (z. b. `GridViewPager`) die Bildschirm Form *automatisch* selbst erkennen und nicht als untergeordnete Elemente der unten beschriebenen Steuerelemente hinzugefügt werden sollen.

### <a name="watchviewstub"></a>Watchviewstub

Sehen Sie sich das [watchviewstub](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-watchviewstub) -Beispiel an, um zu erfahren, wie Sie den Bildschirmtypen erkennen und ein anderes Layout für jeden Typ anzeigen.

Die hauptlayoutdatei enthält eine `android.support.wearable.view.WatchViewStub`, die auf verschiedene Layouts für rechteckige und Runde Bildschirme mithilfe der Attribute `app:rectLayout` und `app:roundLayout` verweist:

```xml
<android.support.wearable.view.WatchViewStub
    xmlns:app="http://schemas.android.com/apk/res-auto"
  android:layout_width="match_parent"
  android:layout_height="match_parent"
  android:id="@+id/stub"
  app:rectLayout="@layout/rect_layout"
  app:roundLayout="@layout/round_layout" />
```

Die Projekt Mappe enthält verschiedene Layouts für jeden Stil, der zur Laufzeit ausgewählt wird:

![Unter Ressourcen/Layout angezeigte Dateien](screen-sizes-images/solution.png)

### <a name="boxinsetlayout"></a>Boxinsetlayout

Anstatt für jeden Bildschirmtyp verschiedene Layouts zu erstellen, können Sie auch eine einzelne Ansicht erstellen, die sich auf rechteckige oder Runde Bildschirme anpasst.

Dieses [Google-Beispiel](https://developer.android.com/training/wearables/ui/layouts.html#same-layout) zeigt, wie Sie die `BoxInsetLayout` verwenden, um das gleiche Layout sowohl auf rechteckigen als auch auf dem Bildschirm zu verwenden.

## <a name="wear-ui-designer"></a>Benutzeroberflächen-Designer

Die xamarin-Android Designer unterstützt sowohl rechteckige als auch Bildschirme:

![Wählen Sie im xamarin-Android Designer den quadratischen Bildschirm Android Wear aus.](screen-sizes-images/design-screen-type.png)

Die Entwurfs Oberfläche im rechteckigen Stil wird hier angezeigt:

![Entwurfs Oberfläche im rechteckigen Stil](screen-sizes-images/design-rect.png) 

Die Entwurfs Oberfläche im runden Stil wird hier angezeigt:

![Entwurfs Oberfläche im runden Stil](screen-sizes-images/design-round.png)

## <a name="wear-simulator"></a>Wear-Simulator

Der **Google-Emulator-Manager** enthält Geräte Definitionen für beide Bildschirmtypen. Sie können rechteckige und runden Emulatoren erstellen, um Ihre APP zu testen.

![Im Google-Emulator-Manager angezeigte Geräte Definitionen](screen-sizes-images/emulator-devices.png)

Der Emulator wird für einen rechteckigen Bildschirm wie folgt dargestellt:

![Emulator-Rendering eines rechteckigen Bildschirms](screen-sizes-images/recipe-2.png) 

Dies wird für einen roundscreen wie folgt dargestellt:

![Emulator-Rendering eines Round-Bildschirms](screen-sizes-images/recipe-2-round.png)

## <a name="video"></a>Video

[Vollbild-Apps für Android Wear](https://www.youtube.com/watch?v=naf_WbtFAlY) von [Developers.Google.com](https://www.youtube.com/channel/UC_x5XG1OV2P6uZZ5FSM9Ttw).
