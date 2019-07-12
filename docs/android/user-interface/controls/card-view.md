---
title: CardView
description: Das CardView--Widget ist eine UI-Komponente, die Text- und Image-Inhalt in den Ansichten enthält, die Karten ähneln. Dieses Handbuch wird erläutert, wie verwenden, und passen CardView in Xamarin.Android-Anwendungen, während die Abwärtskompatibilität mit früheren Versionen von Android aufrechterhalten wird.
ms.prod: xamarin
ms.assetid: CF12FE85-D03A-4E64-95D2-D7115061A500
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: d145a8a3cd8bc321f0fce76a8831fca681ad29a0
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67830574"
---
# <a name="cardview"></a>CardView

_Das CardView--Widget ist eine UI-Komponente, die Text- und Image-Inhalt in den Ansichten enthält, die Karten ähneln. Dieses Handbuch wird erläutert, wie verwenden, und passen CardView in Xamarin.Android-Anwendungen, während die Abwärtskompatibilität mit früheren Versionen von Android aufrechterhalten wird._


## <a name="overview"></a>Übersicht

Die `Cardview` Widget, eingeführt in Android 5.0 (Lollipop), ist eine UI-Komponente, die Text- und Image-Inhalt in den Ansichten enthält, die Karten ähneln. `CardView` wird als implementiert eine `FrameLayout` Widget mit abgerundeten Ecken und einen Schatten. In der Regel eine `CardView` wird verwendet, um eine einzelne Zeilenelement im zu präsentieren einer `ListView` oder `GridView` Gruppe anzeigen. Im folgenden Screenshot wird z. B. ein Beispiel für eine Reservierung Reise-app, die implementiert `CardView`-basierte Travel Destination-Karten in einem bildlauffähigen `ListView`:

![Beispiel-app, die mit einer CardView für die einzelnen Ziele Reisen](card-view-images/01-cardview-example.png)

Dieses Handbuch erläutert das Hinzufügen der `CardView` Paket zu Ihrer Xamarin.Android-Projekt, das Hinzufügen `CardView` Ihr Layout und Anpassen der Darstellung von `CardView` in Ihrer app. Dieses Handbuch enthält außerdem eine ausführliche Liste der `CardView` Attribute, die Sie ändern können, einschließlich der Attribute, die Informationen zur Verwendung von, `CardView` auf Android-Versionen vor Android 5.0 Lollipop.

<a name="requirements" />

## <a name="requirements"></a>Anforderungen

Folgendes ist erforderlich, um die neue Android 5.0 und höher Funktionen verwenden (einschließlich `CardView`) in Xamarin basierenden apps:

-  **Xamarin.Android** &ndash; Xamarin.Android 4.20 or later must be installed and configured with either Visual Studio or Visual Studio for Mac.

-  **Android SDK** &ndash; Android 5.0 (API 21) oder höher muss installiert sein über den Android SDK Manager.

-  **Java JDK 1.8** &ndash; JDK 1.7 kann verwendet werden, wenn Sie speziell für die API-Ebene 23 und früher sind. JDK 1.8 steht [Oracle](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).

Ihre app muss außerdem enthalten die `Xamarin.Android.Support.v7.CardView` Paket. Hinzufügen der `Xamarin.Android.Support.v7.CardView` Paket in Visual Studio für Mac:

1. Öffnen Sie das Projekt, mit der rechten Maustaste **Pakete** , und wählen Sie **Pakete hinzufügen**.

2. In der **Pakete hinzufügen** Dialogfeld, und suchen Sie **CardView**.

3. Wählen Sie **Xamarin-Support-Bibliothek v7 CardView-** , klicken Sie dann auf **Paket hinzufügen**.

Hinzufügen der `Xamarin.Android.Support.v7.CardView` Paket in Visual Studio:

1. Öffnen Sie das Projekt der rechten Maustaste auf die **Verweise** Knoten (in der **Projektmappen-Explorer** Bereich), und wählen Sie **NuGet-Pakete verwalten...** .

2. Wenn die **NuGet-Pakete verwalten** Dialogfeld angezeigt wird, geben Sie **CardView** in das Suchfeld.

3. Wenn **Xamarin-Support-Bibliothek v7 CardView-** angezeigt wird, klicken Sie auf **installieren**.

Vorgehensweise: Konfigurieren Sie ein Android 5.0-app-Projekt finden Sie unter [sich ein Android 5.0-Projekt](~/android/platform/lollipop.md).
Weitere Informationen zum Installieren von NuGet-Pakete finden Sie unter [Exemplarische Vorgehensweise: Einschließen ein NuGet in Ihr Projekt](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).


## <a name="introducing-cardview"></a>Einführung in CardView

Der Standardwert `CardView` ähnelt eine weiße Karte mit minimal abgerundeten Ecken und einen leichten Schatten. Im folgenden Beispiel **Main.axml** Layout zeigt eine einzelne `CardView` Widgets, die enthält eine `TextView`:

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

Wenn Sie diesen XML-Code verwenden, ersetzen Sie den vorhandenen Inhalt der **Main.axml**, achten Sie darauf, dass Sie keinen Code in auskommentieren **"mainactivity.cs"** , die auf Ressourcen in der vorherigen XML-Code verweist.

In diesem Layout-Beispiel erstellt ein Standard- `CardView` mit einer einzelnen Textzeile an wie im folgenden Screenshot gezeigt:

[![Screenshot der CardView-mit weißem Hintergrund und Textzeile](card-view-images/02-basic-cardview-sml.png)](card-view-images/02-basic-cardview.png#lightbox)

In diesem Beispiel ist das app-Format festgelegt ist, auf das Licht Material Design (`Theme.Material.Light`), damit die `CardView` Schatten und Ränder sind leichter zu erkennen. Weitere Informationen zu Designs mit Android 5.0-apps finden Sie unter [Material Design](~/android/user-interface/material-theme.md). Im nächsten Abschnitt erfahren wir, wie Sie die anpassen `CardView` für eine Anwendung.


## <a name="customizing-cardview"></a>Anpassen von CardView

Sie können ändern, die grundlegende `CardView` Attribute, die die Darstellung Anpassen der `CardView` in Ihrer app. Z. B. die Höhe der `CardView` kann erhöht werden, um einen größeren Schatten werfen (wodurch die Karte auf "float" über dem Hintergrund höher erscheinen kann). Darüber hinaus kann der Eckradius erhöht werden, um die Ecken der Karte Weitere gerundet zu machen.

Im nächsten Layout Beispiel eine benutzerdefinierte `CardView` wird verwendet, um eine Simulation eines drucken Fotos (einen "Snapshot") zu erstellen. Ein `ImageView` hinzugefügt wird die `CardView` für die Anzeige der Abbildung und eine `TextView` befindet sich unterhalb der `ImageView` zum Anzeigen des Titels des Bilds.
In diesem Beispiellayout der `CardView` hat die folgenden Anpassungen:

-  Die `cardElevation` auf 4dp auf einen größeren Schatten werfen erhöht.
-  Die `cardCornerRadius` erhöht auf 5dp, stellen die Ecken mehr gerundet angezeigt werden.

Da `CardView` wird bereitgestellt durch die Unterstützungsbibliothek von Android v7, dessen Attribute nicht mehr verfügbar sind die `android:` Namespace. Aus diesem Grund müssen Sie eigene XML-Namespace definieren und verwenden Sie diesen Namespace als dem `CardView` attributpräfix. In folgenden Layout Beispiel verwenden wir diese Zeile einen Namespace mit dem Namen definieren `cardview`:

```xml
    xmlns:cardview="http://schemas.android.com/apk/res-auto"
```

Sie können diesen Namespace Aufrufen `card_view` oder sogar `myapp` Wunsch (es ist nur innerhalb des Bereichs dieser Datei zugegriffen werden kann). Was Sie zum Aufrufen dieser Namespace auswählen, müssen Sie es Präfix für die `CardView` -Attribut, das Sie ändern möchten. In diesem Beispiel Layout der `cardview` Namespace ist das Präfix für `cardElevation` und `cardCornerRadius`:

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

Wenn in diesem Beispiel Layout, zum Anzeigen eines Bilds in einer Anzeige Foto-app verwendet wird die `CardView` verfügt über die Darstellung einer Foto-Momentaufnahme, wie im folgenden Screenshot dargestellt:

[![CardView mit einem Bild und Beschriftung unterhalb der Abbildung](card-view-images/03-photo-cardview-sml.png)](card-view-images/03-photo-cardview.png#lightbox)

In diesem Screenshot stammt aus der [RecyclerViewer](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer) Beispiel-app verwendet eine `RecyclerView` Widgets zum Darstellen einer scrollbaren Liste mit `CardView` Bilder zum Anzeigen von Fotos. Weitere Informationen zu `RecyclerView`, finden Sie unter den [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md) Guide.

Beachten Sie, dass eine `CardView` können mehr als eine untergeordnete Ansicht in der Inhaltsbereich angezeigt. Z. B. die im oben genannten Foto-Beispiel-app anzeigen, der Inhaltsbereich besteht aus einem `ListView` , enthält ein `ImageView` und ein `TextView`. Obwohl `CardView` Instanzen werden häufig vertikal angeordnet, Sie können auch ordnen sie horizontal (finden Sie unter [erstellen eine benutzerdefinierte Ansichtsformatvorlage](~/android/user-interface/material-theme.md#customview) für eine beispielscreenshot).


### <a name="cardview-layout-options"></a>CardView-Layoutoptionen

`CardView` Layouts können angepasst werden, durch Festlegen von ein oder mehrere Attribute, die die Auffüllung, erhöhte Rechte, Eckradius und Hintergrundfarbe beeinflussen:

[![Diagramm der CardView--Attribute](card-view-images/04-attributes-sml.png)](card-view-images/04-attributes.png#lightbox)

Jedes Attribut kann auch geändert werden, dynamisch durch den Aufruf einer Entsprechung `CardView` Methode (Weitere Informationen zu `CardView` Methoden finden Sie unter der [CardView-Klassenverweis](https://developer.android.com/reference/android/support/v7/widget/CardView.html)).
Beachten Sie, dass diese Attribute (mit Ausnahme der Hintergrundfarbe) eine Dimensionswert angegeben werden,. dieser ist eine Dezimalzahl übernehmen, gefolgt von der Einheit. Z. B. `11.5dp` 11,5 Dichte unabhängigen Pixeln angibt.


#### <a name="padding"></a>Abstand

`CardView` bietet fünf Textabstandsattribute um Inhalt auf der Karte zu positionieren. Sie können sie festlegen, in dem Layout-XML oder Sie können analoge Methoden in Ihrem Code aufrufen:

[![Diagramm der CardView padding Attribute](card-view-images/05-padding-sml.png)](card-view-images/05-padding.png#lightbox)

Die Auffüllung-Attribute werden wie folgt erläutert:

-  `contentPadding` &ndash; Innenabstand zwischen der untergeordneten Ansichten der `CardView` und allen Rändern der Karte.

-  `contentPaddingBottom` &ndash; Innenabstand zwischen der untergeordneten Ansichten der `CardView` und dem unteren Rand der Karte.

-  `contentPaddingLeft` &ndash; Innenabstand zwischen der untergeordneten Ansichten der `CardView` und dem linken Rand der Karte.

-  `contentPaddingRight` &ndash; Innenabstand zwischen der untergeordneten Ansichten der `CardView` und dem rechten Rand der Karte.

-  `contentPaddingTop` &ndash; Innenabstand zwischen der untergeordneten Ansichten der `CardView` und dem oberen Rand der Karte.

Inhalt Textabstandsattribute sind relativ zur Grenze des Inhaltsbereichs und nicht auf alle angegebenen Widget befindet sich innerhalb des Inhaltsbereichs.
Z. B. wenn `contentPadding` wurden in der Foto-app anzeigen ausreichend erhöht die `CardView` würde zuschneiden, sowohl für das Bild als auch für den Text, der auf der Karte angezeigt.



#### <a name="elevation"></a>Erhöhte Rechte

`CardView` bietet zwei Erhöhung der Rechte-Attribute, um zu steuern, eine solche Erweiterung und als Ergebnis wird die Größe der Volumeschattenkopie:

[![Diagramm der CardView-Erhöhung der Rechte Attribute](card-view-images/06-elevation-sml.png)](card-view-images/06-elevation.png#lightbox)

Die Attribute für erhöhte Rechte werden wie folgt erläutert:

-  `cardElevation` &ndash; Die Höhe der `CardView` (steht für die Z-Achse).

-  `cardMaxElevation` &ndash; Der maximale Wert, der die `CardView`die Erhöhung der Rechte.

Größere Werte von `cardElevation` erhöhen Sie die Schattengröße zu `CardView` "float" über dem Hintergrund höher scheinen. Die `cardElevation` Attribut bestimmt außerdem die Zeichnungsreihenfolge der überlappende Ansichten, d. h. die `CardView` wird unter einer anderen überlappenden Ansicht mit einer höheren Einstellung für die Erhöhung der Rechte und höher überlappenden Ansichten mit einer niedrigeren Einstellung für die Erhöhung der Rechte gezeichnet werden.
Die `cardMaxElevation` Einstellung ist nützlich für Sie bei Ihrer app Erhöhung der Rechte dynamisch ändert &ndash; verhindert, dass den Schatten erweitern, die innerhalb der Grenze, die Sie mit dieser Einstellung definieren.


#### <a name="corner-radius-and-background-color"></a>Eckradius und Hintergrundfarbe

`CardView` bietet Attribute, die Sie verwenden können, um seine Eckradius und seine Hintergrundfarbe zu steuern. Diese beiden Eigenschaften können Sie ändern, der die allgemeine Darstellung der `CardView`:

[![Diagramm der CardView-Ecke radious und Background-Color-Attribute](card-view-images/07-radius-bgcolor-sml.png)](card-view-images/07-radius-bgcolor.png#lightbox)

Diese Attribute werden wie folgt erläutert:

-  `cardCornerRadius` &ndash; Der Eckradius für alle Ecken des der `CardView`.

-  `cardBackgroundColor` &ndash; Die Farbe des Hintergrunds der `CardView`.

In diesem Diagramm `cardCornerRadius` festgelegt ist, um eine weitere abgerundeten 10dp und `cardBackgroundColor` nastaven NA hodnotu `"#FFFFCC"` (hellgelb).


## <a name="compatibility"></a>Kompatibilität

Sie können `CardView` auf Android-Versionen vor Android 5.0 Lollipop. Da `CardView` ist Teil von die Unterstützungsbibliothek von Android v7 können `CardView` mit Android 2.1 (API-Ebene 7) und höher.
Allerdings müssen Sie installieren die `Xamarin.Android.Support.v7.CardView` Paket wie in beschrieben [Anforderungen](#requirements)weiter oben.

`CardView` ist das etwas anderes Verhalten auf Geräten vor Lollipop (API Level 21):

-  `CardView` verwendet eine programmgesteuerte Shadowimplementierung, die zusätzliche Auffüllung hinzufügt.

-  `CardView` nicht beschnitten untergeordneten Ansichten, die die Überschneidung mit den `CardView`der abgerundeten Ecken.

Als Hilfe bei der Verwaltung dieser kompatibilitätsunterschiede `CardView` enthält mehrere zusätzliche Attribute, die Sie in Ihrem Layout konfigurieren können:

-   `cardPreventCornerOverlap` &ndash; Legen Sie dieses Attribut auf `true` Füllzeichen hinzufügen, wenn Ihre app auf zuvor Android-Versionen (API-Ebene 20 und früher) ausgeführt wird. Diese Einstellung wird verhindert, dass `CardView` Inhalt aus sich überschneiden, mit der `CardView`der abgerundeten Ecken.

-   `cardUseCompatPadding` &ndash; Legen Sie dieses Attribut auf `true` Auffüllung hinzugefügt werden soll, wenn Ihre app in Versionen von Android auf oder größer als API-Ebene 21 ausgeführt wird. Wenn Sie verwenden möchten `CardView` auf vor Lollipop-Geräten und lassen Sie sie sehen gleich Lollipop (oder höher), legen Sie dieses Attribut auf `true`. Wenn dieses Attribut aktiviert ist, `CardView` Anstieg zum Zeichnen von Schatten gezeichnet werden soll, bei der Ausführung auf vor Lollipop Geräten hinzugefügt. Dadurch wird um die Unterschiede bei der Auffüllung zu umgehen, die eingeführt werden, bei der programmgesteuerten vor Lollipop-Volumeschattenkopie-Implementierungen in Kraft sind.

Weitere Informationen zum Verwalten der Kompatibilität mit früheren Versionen von Android finden Sie unter [verwalten Kompatibilität](https://developer.android.com/training/material/compatibility.html).


## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die neuen `CardView` Widgets in Android 5.0 (Lollipop) enthalten. Es veranschaulicht die Standardeinstellung `CardView` Darstellung und erläutert, wie anpassen `CardView` ändern Sie die Erhöhung der Rechte, die Ecken, Auffüllung-Inhalt verwenden und Hintergrundfarbe. Installationsprogramm der `CardView` Layoutattribute (mit Verweis Diagramme), und erläutert, wie mit `CardView` auf Android-Geräten vor Android 5.0 Lollipop. Weitere Informationen zu `CardView`, finden Sie unter den [CardView-Klassenverweis](https://developer.android.com/reference/android/support/v7/widget/CardView.html).


## <a name="related-links"></a>Verwandte Links

- [RecyclerView-(Beispiel)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [Einführung in die Lollipop](~/android/platform/lollipop.md)
- [CardView--Klassenreferenz](https://developer.android.com/reference/android/support/v7/widget/CardView.html)
