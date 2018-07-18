---
title: CardView
description: Das Widget Cardview ist eine Benutzeroberflächenkomponente, die Text- und Image-Inhalt in den Ansichten bereitgestellt werden, die Karten ähneln. Dieses Handbuch erläutert das verwenden und Anpassen von CardView in Xamarin.Android Anwendungen Aufrechterhaltung der Abwärtskompatibilität mit früheren Versionen von Android.
ms.prod: xamarin
ms.assetid: CF12FE85-D03A-4E64-95D2-D7115061A500
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 21e2a2e8ef04936664344cb4fb758bc2af3b4d05
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
ms.locfileid: "30771833"
---
# <a name="cardview"></a>CardView

_Das Widget Cardview ist eine Benutzeroberflächenkomponente, die Text- und Image-Inhalt in den Ansichten bereitgestellt werden, die Karten ähneln. Dieses Handbuch erläutert das verwenden und Anpassen von CardView in Xamarin.Android Anwendungen Aufrechterhaltung der Abwärtskompatibilität mit früheren Versionen von Android._


## <a name="overview"></a>Übersicht

Die `Cardview` Widget "", eingeführt in Android 5.0.x (Lollipop), wird eine Benutzeroberflächenkomponente, die Text- und Image-Inhalt in den Ansichten bereitgestellt werden, die Karten ähneln. `CardView` wird als implementiert eine `FrameLayout` Widget mit abgerundeten Ecken und Schatten. In der Regel eine `CardView` dient zum Darstellen einer einzeiligen Element in einem `ListView` oder `GridView` Gruppe "Ansicht". Beispielsweise dem folgenden Screenshot ist ein Beispiel für eine Reise Reservierung-app, die implementiert `CardView`-basierte Reisen Ziel Karten in einem bildlauffähigen `ListView`:

![Beispiel-app mit einer CardView für die einzelnen Ziele Reisen](card-view-images/01-cardview-example.png)

Dieses Handbuch erläutert das Hinzufügen der `CardView` Paket in Ihr Xamarin.Android-Projekt, zum Hinzufügen von `CardView` das Layout und wie die Darstellung anpassen `CardView` in Ihrer app. Dieses Handbuch enthält außerdem eine ausführliche Liste der `CardView` Attribute, die Sie ändern können, einschließlich der Attribute zur Verwendung `CardView` Android-Versionen vor Android 5.0 Lollipop.

<a name="requirements" />

## <a name="requirements"></a>Anforderungen

Folgendes ist erforderlich, um die neue Android 5.0 und höher Funktionen verwenden (einschließlich `CardView`) in die Xamarin-basierte apps:

-  **Xamarin.Android** &ndash; Xamarin.Android 4.20 or later must be installed and configured with either Visual Studio or Visual Studio for Mac.

-  **Android SDK** &ndash; Android 5.0.x (API 21) oder höher muss installiert sein über den Android SDK Manager.

-  **Java JDK 1.8** &ndash; JDK 1.7 genutzt werden, wenn Sie ausdrücklich Anwendung-API-Ebene 23 und früher sind. JDK 1.8 steht über [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).

Ihre app umfasst auch die `Xamarin.Android.Support.v7.CardView` Paket. Hinzufügen der `Xamarin.Android.Support.v7.CardView` Paket in Visual Studio für Mac:

1. Öffnen Sie das Projekt, mit der rechten Maustaste **Pakete** , und wählen Sie **Hinzufügen von Paketen**.

2. In der **Hinzufügen von Paketen** Dialog, suchen Sie nach **CardView**.

3. Wählen Sie **Xamarin-Unterstützungsbibliothek v7 CardView**, klicken Sie dann auf **Paket hinzufügen**.

Hinzufügen der `Xamarin.Android.Support.v7.CardView` Paket in Visual Studio:

1. Öffnen Sie das Projekt der rechten Maustaste auf die **Verweise** Knoten (in der **Projektmappen-Explorer** Bereich), und wählen Sie **NuGet-Pakete verwalten...** .

2. Wenn die **NuGet-Pakete verwalten** Dialogfeld angezeigt wird, geben Sie **CardView** in das Suchfeld.

3. Wenn **Xamarin-Unterstützungsbibliothek v7 CardView** angezeigt wird, klicken Sie auf **installieren**.

So konfigurieren Sie ein Android 5.0-app-Projekt finden Sie unter [Einrichten einer Android 5.0-Projekt](~/android/platform/lollipop.md).
Weitere Informationen zum Installieren von NuGet-Pakete finden Sie unter [Exemplarische Vorgehensweise: einschließlich eines NuGet in Ihrem Projekt](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).


## <a name="introducing-cardview"></a>Einführung in CardView

Die Standardeinstellung `CardView` ähnelt eine weiße Karte mit minimal abgerundeten Ecken und einen leichten Schatten. Im folgenden Beispiel **Main.axml** Layout zeigt eine einzelne `CardView` Widget, der enthält einem `TextView`:

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

Wenn Sie diese XML-Daten verwenden, um den vorhandenen Inhalt der ersetzen **Main.axml**, achten Sie darauf, dass Sie im Code auskommentieren **MainActivity.cs** , die auf Ressourcen in der vorherigen XML-Code verweist.

In diesem Beispiel Layout erstellt einen Standard- `CardView` mit einer Textzeile wie im folgenden Screenshot gezeigt:

[![Screenshot der CardView mit weißem Hintergrund und Textzeile](card-view-images/02-basic-cardview-sml.png)](card-view-images/02-basic-cardview.png#lightbox)

In diesem Beispiel wird das app-Format das helle Design "Material" festgelegt ist (`Theme.Material.Light`), damit die `CardView` Schatten und Rändern leichter zu erkennen sind. Weitere Informationen zu Designumgebung Android 5.0-apps, finden Sie unter [Design "Material"](~/android/user-interface/material-theme.md). Im nächsten Abschnitt erfahren wir zum Anpassen `CardView` für eine Anwendung.


## <a name="customizing-cardview"></a>Anpassen von CardView

Sie können ändern, die grundlegende `CardView` zum Anpassen der Darstellung der Attribute der `CardView` in Ihrer app. Z. B. die Erhöhung von der `CardView` erhöht werden kann, um eine größere Schatten geworfen wird (wodurch die Karte in einen Gleitkommawert höher über dem Hintergrund scheint kann). Darüber hinaus kann die Eckradius erhöht werden, stellen die Ecken der Karte Weitere gerundet.

Im nächsten Layout Beispiel eines benutzerdefinierten `CardView` wird verwendet, um eine Simulation der drucken Foto ("Momentaufnahme") zu erstellen. Ein `ImageView` hinzugefügt wird die `CardView` für das Bild anzuzeigen und eine `TextView` befindet sich unterhalb der `ImageView` für die Anzeige des Titels des Bilds.
In diesem Beispiellayout der `CardView` verfügt über folgende Anpassungen möglich:

-  Die `cardElevation` auf 4dp zu einen größeren Schatten geworfen erhöht.
-  Die `cardCornerRadius` wird erweitert, dass 5dp, stellen die Ecken mehr gerundet angezeigt werden.

Da `CardView` erfolgt durch die Unterstützungsbibliothek von Android v7 seiner Attribute nicht verfügbar sind die `android:` Namespace. Aus diesem Grund müssen Sie eigene XML-Namespace definieren und verwenden Sie diesen Namespace als dem `CardView` attributpräfix. In folgenden Layout-Beispiel verwenden wir die folgende Zeile einen Namespace namens definieren `cardview`:

```xml
    xmlns:cardview="http://schemas.android.com/apk/res-auto"
```

Sie können diesen Namespace Aufrufen `card_view` oder sogar `myapp` auf Wunsch (es ist nur innerhalb des Bereichs dieser Datei zugegriffen werden kann). Was Sie auswählen, diesen Namespace aufrufen, müssen Sie es als Präfix für die `CardView` -Attribut, das Sie ändern möchten. In diesem Beispiel Layout der `cardview` Namespace ist das Präfix für `cardElevation` und `cardCornerRadius`:

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

Wenn in diesem Beispiel Layout verwendet wird, um ein Bild in einer app Fotos anzeigen anzuzeigen die `CardView` die Darstellung einer Momentaufnahme Foto hat, wie im folgenden Screenshot dargestellt:

[![CardView mit einem Bild und Beschriftung unter dem Bild](card-view-images/03-photo-cardview-sml.png)](card-view-images/03-photo-cardview.png#lightbox)

Diesem Screenshot stammt aus der [RecyclerViewer](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer) Beispiel-app verwendet einen `RecyclerView` Widgets zum Präsentieren von einer bildlauffähigen Liste mit `CardView` Bilder für die Anzeige von Fotos. Weitere Informationen zu `RecyclerView`, finden Sie unter der [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md) Handbuch.

Beachten Sie, dass eine `CardView` mehr als eine untergeordnete Ansicht in seiner im Inhaltsbereich angezeigt werden können. Z. B. in der obigen Foto anzeigen von app-Beispiel Inhaltsbereichs besteht aus einem `ListView` , enthält eine `ImageView` und eine `TextView`. Obwohl `CardView` Instanzen werden häufig vertikal angeordnet, Sie können auch anordnen horizontal (finden Sie unter [erstellen eine benutzerdefinierte Ansicht Formatvorlage](~/android/user-interface/material-theme.md#customview) für eine Beispiel-Screenshot).


### <a name="cardview-layout-options"></a>CardView Layoutoptionen

`CardView` Layouts können angepasst werden, indem Sie einem oder mehreren Attributen, die Auswirkungen auf die Auffüllung, Höhe, Eckradius und Hintergrundfarbe festlegen:

[![Diagramm der CardView-Attribute](card-view-images/04-attributes-sml.png)](card-view-images/04-attributes.png#lightbox)

Jedes Attribut kann auch geändert werden, dynamisch durch Aufrufen einer Entsprechung `CardView` Methode (Weitere Informationen zu `CardView` Methoden, finden Sie unter der [CardView-Klassenreferenz](https://developer.android.com/reference/android/support/v7/widget/CardView.html)).
Beachten Sie, dass diese Attribute (mit Ausnahme von Background-Farbe) einen Dimensionswert, der eine Dezimalzahl übernehmen, gefolgt von der Einheit ist. Beispielsweise `11.5dp` 11.5 Dichte typunabhängig Pixel angibt.


#### <a name="padding"></a>Textabstand
`
CardView` bietet fünf Auffüllung-Attribute, um Inhalt in die Karte zu positionieren. Sie können sie festlegen, in dem Layout-XML oder Sie können analoge Methoden im Code aufrufen:

[![Diagramm der CardView padding-Attribute](card-view-images/05-padding-sml.png)](card-view-images/05-padding.png#lightbox)

Die Auffüllung Attribute werden wie folgt erläutert:

-  `contentPadding` &ndash; Auffüllung zwischen den untergeordneten Ansichten der inneren der `CardView` und allen Rändern der Karte.

-  `contentPaddingBottom` &ndash; Auffüllung zwischen den untergeordneten Ansichten der inneren der `CardView` und dem unteren Rand der Karte.

-  `contentPaddingLeft` &ndash; Auffüllung zwischen den untergeordneten Ansichten der inneren der `CardView` und dem linken Rand der Karte.

-  `contentPaddingRight` &ndash; Auffüllung zwischen den untergeordneten Ansichten der inneren der `CardView` und dem rechten Rand der Karte.

-  `contentPaddingTop` &ndash; Auffüllung zwischen den untergeordneten Ansichten der inneren der `CardView` und dem oberen Rand der Karte.

Inhalt Auffüllung Attribute sind relativ zu den Grenzen des Inhaltsbereichs und nicht auf alle angegebenen Widget innerhalb des Inhaltsbereichs.
Z. B. wenn `contentPadding` wurden in der app Fotos anzeigen ausreichend erhöht die `CardView` würde schneiden Sie das Bild und Text auf der Karte angezeigt.



#### <a name="elevation"></a>Erhöhung der Rechte

`CardView` bietet zwei Erhöhung-Attribute, um zu steuern, die Erhöhung der Rechte und demzufolge die Schattengröße:

[![Diagramm der CardView Erhöhung Attribute](card-view-images/06-elevation-sml.png)](card-view-images/06-elevation.png#lightbox)

Die Erhöhung der Rechte Attribute werden wie folgt erläutert:

-  `cardElevation` &ndash; Die Erhöhung von der `CardView` (seine Z-Achse darstellt).

-  `cardMaxElevation` &ndash; Der Maximalwert der der `CardView`der Erhöhung der Rechte.

Größere Werte von `cardElevation` vergrößern Sie den Schatten vornehmen `CardView` "float" über dem Hintergrund höher scheinen. Die `cardElevation` Attribut bestimmt außerdem die Zeichnungsreihenfolge des überlappende Ansichten, d. h., die `CardView` unter einer anderen überlappende Ansicht mit einer höheren Einstellung für die Erhöhung der Rechte und höher überlappenden Ansichten mit einer niedrigeren Einstellung für die Erhöhung der Rechte gezeichnet wird.
Die `cardMaxElevation` Einstellung eignet sich für die bei Ihrer app Erhöhung der Rechte kann dynamisch Änderungen in &ndash; verhindert, dass den Schatten Erweiterung über die Begrenzung, die Sie mit dieser Einstellung definieren.


#### <a name="corner-radius-and-background-color"></a>Eckradius und Hintergrundfarbe

`CardView` bietet die Attribute, die Sie verwenden können, um seine Eckradius und die Hintergrundfarbe zu steuern. Diese beiden Eigenschaften können Sie das allgemeine Format des Ändern der `CardView`:

[![Diagramm der CardView Ecke radious und Farbe Hintergrundattribute](card-view-images/07-radius-bgcolor-sml.png)](card-view-images/07-radius-bgcolor.png#lightbox)

Diese Attribute werden wie folgt erläutert:

-  `cardCornerRadius` &ndash; Die Eckradius aller Ecken des der `CardView`.

-  `cardBackgroundColor` &ndash; Die Hintergrundfarbe der `CardView`.

In diesem Diagramm `cardCornerRadius` festgelegt ist, um eine weitere abgerundeten 10dp und `cardBackgroundColor` festgelegt ist, um `"#FFFFCC"` (hellgelb).


## <a name="compatibility"></a>Kompatibilität

Sie können `CardView` Android-Versionen vor Android 5.0 Lollipop. Da `CardView` ist Bestandteil des Android v7-Unterstützungsbibliothek, können Sie `CardView` mit Android 2.1 (API-Ebene 7) und höher.
Allerdings müssen Sie installieren die `Xamarin.Android.Support.v7.CardView` Paket wie beschrieben in [Anforderungen](#requirements)oben.

`CardView` etwas anders verhält auf Geräten vor Lollipop (API-Ebene 21):

-  `CardView` verwendet eine programmgesteuerte Shadowimplementierung, die zusätzlichen Abstands aus hinzufügt.

-  `CardView` nicht beschnitten untergeordnete Ansichten, die die Überschneidung mit den `CardView`des abgerundete Ecken.

Als Hilfe bei der Verwaltung von Kompatibilität Unterschiede `CardView` enthält mehrere zusätzliche Attribute, die Sie in das Layout konfigurieren können:

-   `cardPreventCornerOverlap` &ndash; Legen Sie dieses Attribut auf `true` Auffüllung hinzugefügt, wenn Ihre app auf zuvor Android-Version (API-Ebene 20 und früher) ausgeführt wird. Diese Einstellung wird verhindert, dass `CardView` mit sich überschneidenden Inhalt der `CardView`des abgerundete Ecken.

-   `cardUseCompatPadding` &ndash; Legen Sie dieses Attribut auf `true` Auffüllung hinzugefügt, wenn die app in Versionen von Android auf oder größer als die API-Ebene 21 ausgeführt wird. Wenn Sie verwenden möchten `CardView` auf vor Lollipop-Geräten und mit ihr Erscheinungsbild ist dasselbe Lollipop (oder höher), legen Sie dieses Attribut auf `true`. Wenn dieses Attribut aktiviert ist, `CardView` fügt zusätzlichen Abstands aus, um Schatten gezeichnet werden soll, wenn er vor Lollipop-Geräten ausgeführt wird. Dadurch werden die Unterschiede in der Auffüllung überwunden werden, die eingefügt werden, wenn Implementierungen der programmgesteuerten Schatten vor Lollipop wirksam sind.

Weitere Informationen zum Verwalten der Kompatibilität mit früheren Versionen von Android finden Sie unter [verwalten Kompatibilität](https://developer.android.com/training/material/compatibility.html).


## <a name="summary"></a>Zusammenfassung

Dieses Handbuch eingeführten neuen `CardView` Widget in Android 5.0.x (Lollipop) enthalten. Es veranschaulicht die Standardeinstellung `CardView` Darstellung und erläutert, wie anpassen `CardView` durch eine Änderung der Erhöhung der Rechte, die Ecken, content-Padding und Hintergrundfarbe. Aufgeführt der `CardView` Layoutattribute (mit Verweis Diagramme) und erläutert, wie verwenden `CardView` auf Android-Geräten vor Android 5.0 Lollipop. Weitere Informationen zu `CardView`, finden Sie unter der [CardView-Klassenreferenz](https://developer.android.com/reference/android/support/v7/widget/CardView.html).


## <a name="related-links"></a>Verwandte Links

- [RecyclerView (Beispiel)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [Einführung in die Lollipop](~/android/platform/lollipop.md)
- [CardView-Klassenreferenz](https://developer.android.com/reference/android/support/v7/widget/CardView.html)
