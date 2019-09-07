---
title: CardView
description: Das CardView-Widget ist eine Benutzeroberflächen Komponente, die Text-und Bildinhalte in Sichten darstellt, die Karten ähneln. In diesem Handbuch wird erläutert, wie CardView in xamarin. Android-Anwendungen verwendet und angepasst wird, während die Abwärtskompatibilität mit früheren Versionen von Android gewahrt bleibt.
ms.prod: xamarin
ms.assetid: CF12FE85-D03A-4E64-95D2-D7115061A500
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 2051c7c904dedf8b41f405d3ec7b9c1a003b7fd5
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70758778"
---
# <a name="xamarinandroid-cardview"></a>Xamarin. Android-Kartenansicht

_Das CardView-Widget ist eine Benutzeroberflächen Komponente, die Text-und Bildinhalte in Sichten darstellt, die Karten ähneln. In diesem Handbuch wird erläutert, wie CardView in xamarin. Android-Anwendungen verwendet und angepasst wird, während die Abwärtskompatibilität mit früheren Versionen von Android gewahrt bleibt._

## <a name="overview"></a>Übersicht

Das `Cardview` in Android 5,0 (Lollipop) eingeführte Widget ist eine Benutzeroberflächen Komponente, die Text-und Bildinhalte in Sichten darstellt, die Karten ähneln. `CardView`wird als `FrameLayout` Widget mit abgerundeten Ecken und einem Schatten implementiert. In der Regel `CardView` wird ein-Element verwendet, um ein einzelnes Zeilen `ListView` Element `GridView` in einer-oder-Ansichts Gruppe darzustellen. Der folgende Screenshot ist beispielsweise ein Beispiel für eine Reise Reservierungs-APP, die- `CardView`basierte Reise zielkarten in einem scrollbaren `ListView`implementiert:

![Beispiel-APP, die eine CardView für jedes Reise Ziel verwendet](card-view-images/01-cardview-example.png)

In diesem Handbuch wird erläutert, wie `CardView` Sie das Paket Ihrem xamarin. Android-Projekt hinzufügen `CardView` , wie Sie das Layout hinzufügen und wie Sie die `CardView` Darstellung von in Ihrer APP anpassen. Darüber hinaus enthält dieses Handbuch eine ausführliche Liste der Attribute `CardView` , die Sie ändern können, einschließlich der Attribute, die Ihnen `CardView` bei der Verwendung von Android-Versionen vor Android 5,0 Lollipop helfen.

<a name="requirements" />

## <a name="requirements"></a>Anforderungen

Folgendes ist erforderlich, um neue Features von Android 5,0 und höher (einschließlich `CardView`) in xamarin-basierten apps zu verwenden:

- **Xamarin. Android** &ndash; xamarin. Android 4,20 oder höher muss entweder mit Visual Studio oder mit Visual Studio für Mac installiert und konfiguriert werden.

- **Android SDK** &ndash; Android 5,0 (API 21) oder höher muss über den Android SDK-Manager installiert werden.

- **Java JDK 1,8** &ndash; JDK 1,7 kann verwendet werden, wenn Sie genau die API-Ebene 23 und früher verwenden. JDK 1,8 ist in [Oracle](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)verfügbar.

Ihre APP muss auch das `Xamarin.Android.Support.v7.CardView` Paket enthalten. So fügen Sie `Xamarin.Android.Support.v7.CardView` das Paket in Visual Studio für Mac hinzu:

1. Öffnen Sie das Projekt, klicken Sie mit der rechten Maustaste auf **Pakete** , und wählen Sie **Pakete hinzu**

2. Suchen Sie im Dialogfeld **Pakete hinzufügen** nach **CardView**.

3. Wählen Sie **xamarin-Unterstützungs Bibliothek V7 CardView**aus, und klicken Sie dann auf **Paket hinzufügen**.

So fügen Sie `Xamarin.Android.Support.v7.CardView` das Paket in Visual Studio hinzu:

1. Öffnen Sie das Projekt, klicken Sie mit der rechten Maustaste auf den Knoten **Verweise** (im Bereich **Projektmappen-Explorer** ), und wählen Sie **nuget-Pakete verwalten...** aus.

2. Wenn das Dialogfeld **nuget-Pakete verwalten** angezeigt wird, geben Sie **CardView** in das Suchfeld ein.

3. Wenn die **xamarin-Support Bibliothek V7 CardView** angezeigt wird, klicken Sie auf **Installieren**.

Informationen zum Konfigurieren eines Android 5,0-App-Projekts finden Sie unter [Einrichten eines Android 5,0-Projekts](~/android/platform/lollipop.md).
Weitere Informationen zum Installieren von nuget-Paketen finden [Sie unter Exemplarische Vorgehensweise: Einschließen eines nuget-Projekts in](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)Ihr Projekt.

## <a name="introducing-cardview"></a>Einführung in CardView

Der Standard `CardView` ähnelt einer weißen Karte mit minimal abgerundeten Ecken und einem leichten Schatten. Im folgenden Beispiel für das **Main. axml** -Layout `CardView` wird ein einzelnes Widget `TextView`angezeigt, das eine enthält:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:gravity="center_horizontal"
    android:padding="5dp">
    <android.support.v7.widget.CardView
        android:layout_width="fill_parent"
        android:layout_height="245dp"
        android:layout_gravity="center_horizontal">
        <TextView
            android:text="Basic CardView"
            android:layout_marginTop="0dp"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:gravity="center"
            android:layout_centerVertical="true"
            android:layout_alignParentRight="true"
            android:layout_alignParentEnd="true" />
    </android.support.v7.widget.CardView>
</LinearLayout>
```

Wenn Sie diesen XML-Code verwenden, um den vorhandenen Inhalt von **Main. axml**zu ersetzen, stellen Sie sicher, dass Sie jeden Code in **MainActivity.cs** auskommentieren, der sich auf Ressourcen in der vorherigen XML-Datei bezieht.

In diesem Layoutbeispiel wird `CardView` ein Standard mit einer einzelnen Textzeile erstellt, wie im folgenden Screenshot zu sehen:

[![Screenshot von CardView mit weißem Hintergrund und Textzeile](card-view-images/02-basic-cardview-sml.png)](card-view-images/02-basic-cardview.png#lightbox)

In diesem Beispiel wird der APP-Stil auf das helle Material Design (`Theme.Material.Light`) festgelegt, sodass die `CardView` Schatten und Kanten leichter zu sehen sind. Weitere Informationen zum Design von Android 5,0-apps finden Sie unter [Material Theme (Material](~/android/user-interface/material-theme.md)Design). Im nächsten Abschnitt erfahren Sie, wie Sie für eine Anwendung `CardView` anpassen.

## <a name="customizing-cardview"></a>Anpassen von CardView

Sie können die grundlegenden `CardView` Attribute ändern, um die `CardView` Darstellung der in Ihrer APP anzupassen. So `CardView` kann z. b. die Erhöhung des erhöht werden, um einen größeren Schatten umzuwandeln (wodurch die Karte zu einer höheren Position über dem Hintergrund wird). Außerdem kann der Eckradius erweitert werden, damit die Ecken der Karte abgerundet werden.

Im nächsten Layoutbeispiel wird eine angepasste `CardView` zum Erstellen einer Simulation eines Druck Fotos (eine "Momentaufnahme") verwendet. Der `ImageView` `TextView` `ImageView` zum Anzeigen des Bilds wird ein hinzugefügt, und ein wird unter dem positioniert, um den Titel des Bilds anzuzeigen. `CardView`
In diesem Beispiel Layout `CardView` verfügt über die folgenden Anpassungen:

- Der `cardElevation` wird auf 4DP vergrößert, um einen größeren Schatten umzuwandeln.
- Der `cardCornerRadius` wird auf 5DP angehoben, damit die Ecken gerundet werden.

Da `CardView` von der Android V7-Unterstützungs Bibliothek bereitgestellt wird, sind seine Attribute im `android:` -Namespace nicht verfügbar. Daher müssen Sie einen eigenen XML-Namespace definieren und diesen Namespace als `CardView` Attribut Präfix verwenden. Im folgenden Layoutbeispiel wird diese Zeile verwendet, um einen Namespace mit dem Namen `cardview`zu definieren:

```xml
    xmlns:cardview="http://schemas.android.com/apk/res-auto"
```

Sie können diesen Namespace `card_view` oder auch `myapp` dann, wenn Sie auswählen (nur innerhalb des Gültigkeits Bereichs dieser Datei zugänglich), abrufen. Wenn Sie diesen Namespace auswählen, müssen Sie ihn verwenden, um das `CardView` Attribut als Präfix zuzuweisen, das Sie ändern möchten. In diesem Layoutbeispiel stellt `cardview` der Namespace das Präfix `cardElevation` für `cardCornerRadius`und dar:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:cardview="http://schemas.android.com/apk/res-auto"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:gravity="center_horizontal"
    android:padding="5dp">
    <android.support.v7.widget.CardView
        android:layout_width="fill_parent"
        android:layout_height="245dp"
        android:layout_gravity="center_horizontal"
        cardview:cardElevation="4dp"
        cardview:cardCornerRadius="5dp">
        <LinearLayout
            android:layout_width="fill_parent"
            android:layout_height="240dp"
            android:orientation="vertical"
            android:padding="8dp">
            <ImageView
                android:layout_width="fill_parent"
                android:layout_height="190dp"
                android:id="@+id/imageView"
                android:scaleType="centerCrop" />
            <TextView
                android:layout_width="fill_parent"
                android:layout_height="wrap_content"
                android:textAppearance="?android:attr/textAppearanceMedium"
                android:textColor="#333333"
                android:text="Photo Title"
                android:id="@+id/textView"
                android:layout_gravity="center_horizontal"
                android:layout_marginLeft="5dp" />
        </LinearLayout>
    </android.support.v7.widget.CardView>
</LinearLayout>
```

Wenn dieses Layoutbeispiel zum Anzeigen eines Bilds in einer Foto-APP verwendet wird `CardView` , hat das Aussehen einer Foto Momentaufnahme, wie im folgenden Screenshot dargestellt:

[![CardView mit einem Bild und einer Beschriftung unterhalb des Bilds](card-view-images/03-photo-cardview-sml.png)](card-view-images/03-photo-cardview.png#lightbox)

Dieser Screenshot stammt aus der [recyclerviewer](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer) -Beispiel-APP, die ein `RecyclerView` Widget verwendet, um eine Bildlauf `CardView` -Liste mit Bildern zum Anzeigen von Fotos darzustellen. Weitere Informationen zu `RecyclerView`finden Sie im [recyclerview](~/android/user-interface/layouts/recycler-view/index.md) -Handbuch.

Beachten Sie, `CardView` dass ein in seinem Inhalts Bereich mehr als eine untergeordnete Ansicht anzeigen kann. Beispielsweise besteht der Inhalts Bereich im obigen Beispiel für die Foto Anzeige der APP aus einer `ListView` , die eine `ImageView` und eine `TextView`enthält. Obwohl `CardView` Instanzen oft vertikal angeordnet sind, können Sie Sie auch horizontal anordnen (Weitere Informationen finden Sie unter [Erstellen eines benutzerdefinierten Ansichts Stils](~/android/user-interface/material-theme.md#customview) für einen Beispiel Bildschirm).

### <a name="cardview-layout-options"></a>CardView-Layoutoptionen

`CardView`Layouts können angepasst werden, indem ein oder mehrere Attribute festgelegt werden, die sich auf die Auffüll-, Erweiterungs-, Eckradius-und Hintergrundfarbe auswirken:

[![Diagramm der Karten Ansichts Attribute](card-view-images/04-attributes-sml.png)](card-view-images/04-attributes.png#lightbox)

Jedes Attribut kann auch dynamisch geändert werden, indem eine gegenmethode `CardView` aufgerufen wird (Weitere Informationen zu Methoden finden Sie in `CardView` der [CardView-Klassenreferenz](https://developer.android.com/reference/android/support/v7/widget/CardView.html)).
Beachten Sie, dass diese Attribute (mit Ausnahme von Hintergrundfarbe) einen Dimensions Wert akzeptieren, bei dem es sich um eine Dezimalzahl, gefolgt von der-Einheit handelt. `11.5dp` Gibt z. b. 11,5 Dichte unabhängige Pixel an.

#### <a name="padding"></a>Abstand

`CardView`bietet fünf Auffüll Attribute zum Positionieren von Inhalten in der Karte. Sie können Sie im Layout-XML festlegen, oder Sie können analog Methoden in Ihrem Code aufzurufen:

[![Diagramm der CardView-Auffüll Attribute](card-view-images/05-padding-sml.png)](card-view-images/05-padding.png#lightbox)

Die Auffüll Attribute werden wie folgt erläutert:

- `contentPadding`Innere Auffüll Zeichen zwischen den untergeordneten Sichten `CardView` von und allen Rändern der Karte. &ndash;

- `contentPaddingBottom`Innere Auffüll Zeichen zwischen den untergeordneten Ansichten `CardView` von und dem unteren Rand der Karte. &ndash;

- `contentPaddingLeft`Innere Auffüll Zeichen zwischen den untergeordneten Ansichten `CardView` von und dem linken Rand der Karte. &ndash;

- `contentPaddingRight`Innere Auffüll Zeichen zwischen den untergeordneten Ansichten `CardView` von und dem rechten Rand der Karte. &ndash;

- `contentPaddingTop`Innere Auffüll Zeichen zwischen den untergeordneten Ansichten `CardView` von und dem oberen Rand der Karte. &ndash;

Die Attribute für die Inhalts Auffüll Zeichen sind relativ zur Grenze des Inhalts Bereichs und nicht zu einem beliebigen Widget innerhalb des Inhalts Bereichs.
Wenn `contentPadding` z. b. in der Foto-Anzeige-App ausreichend erweitert `CardView` wurde, würde das Bild und der Text, der auf der Karte angezeigt wird, von zuschneiden.

#### <a name="elevation"></a>Trie

`CardView`bietet zwei Attribute zur Rechte Erweiterung, um deren Erhöhung und somit die Größe des Schattens zu steuern:

[![Diagramm der Karten Ansichts Attribute](card-view-images/06-elevation-sml.png)](card-view-images/06-elevation.png#lightbox)

Die Attribute für die Rechte Erweiterung werden wie folgt erläutert:

- `cardElevation`&ndash; Die Höhe`CardView` von (stellt die Z-Achse dar).

- `cardMaxElevation`Der Höchstwert für die `CardView`Erhöhung der Rechte. &ndash;

Größere Werte von `cardElevation` vergrößern die Schatten Größe `CardView` , sodass Sie zu einer höheren Position über dem Hintergrund fließen. Das `cardElevation` -Attribut bestimmt auch die Zeichnungsreihenfolge von überlappenden Sichten `CardView` , d. h., das wird unter einer anderen überlappenden Ansicht mit einer höheren Erweiterungs Einstellung und überlappenden Sichten mit einer niedrigeren Erweiterungs Einstellung gezeichnet.
Diese Einstellung ist nützlich, wenn Ihre APP die Rechte Erweiterung &ndash; dynamisch ändert. Dadurch wird verhindert, dass der Schatten das Limit überschreitet, das Sie mit dieser Einstellung definieren. `cardMaxElevation`

#### <a name="corner-radius-and-background-color"></a>Eckradius und Hintergrundfarbe

`CardView`bietet Attribute, die Sie verwenden können, um den Eckradius und seine Hintergrundfarbe zu steuern. Mit diesen beiden Eigenschaften können Sie den allgemeinen Stil von `CardView`ändern:

[![Diagramm der Karten Ansichts Ecke und der Hintergrundfarben Attribute](card-view-images/07-radius-bgcolor-sml.png)](card-view-images/07-radius-bgcolor.png#lightbox)

Diese Attribute werden wie folgt erläutert:

- `cardCornerRadius`Der Eckradius aller Ecken `CardView`des. &ndash;

- `cardBackgroundColor`Die Hintergrundfarbe `CardView`des. &ndash;

In diesem Diagramm ist `cardCornerRadius` auf einen gerundeten 10dp festgelegt, `cardBackgroundColor` und wird auf `"#FFFFCC"` (hellgelb) festgelegt.

## <a name="compatibility"></a>Kompatibilität

Sie können in `CardView` früheren Versionen von Android als Android 5,0 Lollipop verwenden. Da `CardView` Teil der Android V7-Unterstützungs Bibliothek ist, können Sie mit `CardView` Android 2,1 (API-Ebene 7) und höher verwenden.
Allerdings müssen Sie das `Xamarin.Android.Support.v7.CardView` Paket wie unter [Anforderungen](#requirements)beschrieben installieren.

`CardView`zeigt ein geringfügig anderes Verhalten auf Geräten vor Lollipop (API-Ebene 21):

- `CardView`verwendet eine programmgesteuerte Schatten Implementierung, die zusätzliche Auffüll Zeichen hinzufügt.

- `CardView`schneidet keine untergeordneten Ansichten ab, die sich mit `CardView`den abgerundeten Ecken des Bilds schneiden.

Zur Unterstützung der Verwaltung dieser Kompatibilitäts `CardView` Unterschiede bietet einige zusätzliche Attribute, die Sie im Layout konfigurieren können:

- `cardPreventCornerOverlap`Legen Sie dieses Attribut `true` auf fest, um Auffüll Zeichen hinzuzufügen, wenn Ihre APP unter früheren Android-Versionen (API-Ebene 20 und früher) ausgeführt wird. &ndash; Diese Einstellung verhindert `CardView` , dass Inhalte mit den `CardView`gerundeten Ecken überschneidet werden.

- `cardUseCompatPadding`Legen Sie dieses Attribut `true` auf fest, um Auffüll Zeichen hinzuzufügen, wenn Ihre APP in Android-Versionen auf API-Ebene 21 oder höher ausgeführt wird. &ndash; Wenn Sie auf Pre- `CardView` Lollipop-Geräten verwenden möchten und es bei Lollipop (oder höher) genauso aussehen soll, legen Sie dieses Attribut auf `true`fest. Wenn dieses Attribut aktiviert ist, `CardView` fügt zusätzliche Auffüll Zeichen hinzu, um Schatten zu zeichnen, wenn es auf Pre-Lollipop-Geräten ausgeführt wird. Dies trägt dazu bei, die Unterschiede bei der Auffüll Funktion zu überwinden, die eingeführt werden, wenn die programmatischen Schatten Implementierungen vor Lollipop wirksam sind.

Weitere Informationen zur Aufrechterhaltung der Kompatibilität mit früheren Versionen von Android finden Sie unter [beibehalten der Kompatibilität](https://developer.android.com/training/material/compatibility.html).

## <a name="summary"></a>Zusammenfassung

In diesem Leitfaden wurde das `CardView` neue Widget eingeführt, das in Android 5,0 (Lollipop) enthalten ist. Es wurde die Standard `CardView` Darstellung demonstriert und erläutert, wie `CardView` Sie anpassen können, indem Sie die Rechte Erweiterung, die eckweite, die Inhalts Auffüll-und die Hintergrundfarbe ändern. Dabei wurden die `CardView` Layoutattribute (mit Referenz Diagrammen) aufgelistet und erläutert, wie `CardView` Sie auf Android-Geräten vor Android 5,0 Lollipop verwenden können. Weitere Informationen zu `CardView`finden Sie in der [CardView-Klassenreferenz](https://developer.android.com/reference/android/support/v7/widget/CardView.html).

## <a name="related-links"></a>Verwandte Links

- [Recyclerview (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer)
- [Einführung in Lollipop](~/android/platform/lollipop.md)
- [CardView-Klassenreferenz](https://developer.android.com/reference/android/support/v7/widget/CardView.html)
