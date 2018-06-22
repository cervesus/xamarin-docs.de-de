---
title: Grafiken und Animationen
description: Android bietet ein sehr umfangreiches und verschiedene-Framework zur Unterstützung von 2D Grafiken und Animationen. Dieses Thema führt diese Frameworks und erläutert, wie benutzerdefinierte Grafiken und Animationen für die Verwendung in einer Anwendung Xamarin.Android zu erstellen.
ms.prod: xamarin
ms.assetid: 80086318-6FE4-4711-9A71-5C8F8C28C754
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 85a7cd2ac5094a4760c5465bd099ce2e3beeae79
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
ms.locfileid: "30773786"
---
# <a name="graphics-and-animation"></a>Grafiken und Animationen

_Android bietet ein sehr umfangreiches und verschiedene-Framework zur Unterstützung von 2D Grafiken und Animationen. Dieses Thema führt diese Frameworks und erläutert, wie benutzerdefinierte Grafiken und Animationen für die Verwendung in einer Anwendung Xamarin.Android zu erstellen._


## <a name="overview"></a>Übersicht

Trotz ausgeführt auf Geräten, die bisher von Power beschränkt sind, haben die höchste Bewertung mobilen Anwendungen häufig eine anspruchsvolle User Experience (UX), vollständige mit qualitativ hochwertigen Grafiken und Animationen, die eine intuitive, reaktionsfähig, dynamischen Verhalten bereitstellen. Abrufen von mobile Anwendungen mehr und komplexere, haben Benutzer mehr und mehr von Anwendungen erwarten begonnen.

Glücklicherweise haben moderne mobile Plattformen für uns sehr leistungsfähig Frameworks zum Erstellen anspruchsvoller Animationen und benutzerdefinierten Grafiken Beibehaltung der Bedienung. Dies ermöglicht Entwicklern mit sehr geringem Aufwand umfassenden Interaktivität hinzuzufügen.

UI-API-Frameworks in Android können grob in zwei Kategorien aufgeteilt werden: Grafiken und Animationen.

Grafiken werden weiter in unterschiedliche Ansätze für die 2D-und 3D-Grafik geteilt. 3D-Grafiken stehen über diverse in Frameworks wie z. B. OpenGL ES (eine mobile bestimmte Version von OpenGL) und Drittanbieter-Frameworks wie z. B. MonoGame (eine plattformübergreifende Toolkit kompatibel mit dem XNA-Toolkit) erstellt werden. Obwohl 3D-Grafiken nicht innerhalb des Bereichs dieses Artikels sind, werden die integrierten 2D zeichnen Techniken untersucht.

Android bietet zwei verschiedene APIs zum Erstellen von 2D Grafiken. Ein ist ein auf hoher Ebene deklarativer Ansatz und die andere eine programmgesteuerte Low-Level-API:

-   **Zeichenbaren Ressourcen** &ndash; diese werden verwendet, um benutzerdefinierte Grafiken entweder programmgesteuert oder (in der Regel) erstellen, durch das Einbetten von Zeichnen-Anweisungen in XML-Dateien. Zeichenbare Ressourcen werden in der Regel als XML-Dateien definiert, die Anweisungen oder Aktionen für Android zum Rendern einer 2D Grafik enthalten. 

-   **Zeichenbereich** &ndash; Dies ist eine low-Level-API, die direkt auf eine zugrunde liegende Bitmap zeichnen umfasst. Es bietet sehr eine präzisere Kontrolle darüber, was angezeigt wird. 

Zusätzlich zu diesen Verfahren 2D Grafiken bietet Android auch verschiedene Methoden zum Erstellen von Animationen:

-   **Zeichenbaren Animationen** &ndash; Android unterstützt auch bekannt als Frame nach dem anderen-Animationen *zeichenbaren Animation*. Dies ist die einfachste Animations-API. Android nacheinander lädt und zeigt die zeichenbare Ressourcen in der Sequenz (ähnlich wie eine Cartoon).

-   **Animationen anzeigen,** &ndash; *Ansicht Animationen* werden die ursprünglichen Animation-APIs in Android und in allen Versionen von Android verfügbar sind. Diese API ist beschränkt, funktioniert nur mit Sichtobjekte und können nur einfache Transformationen für diese Sichten ausführen.
    Ansicht Animationen werden in der Regel definiert, in XML-Dateien in den `/Resources/anim` Ordner.

-   **Eigenschaftenanimationen** &ndash; Android 3.0 eingeführten einen neuen Satz von der Animations-API des genannt *Eigenschaftenanimationen*. Diese neue API eingeführt, ein erweiterbares und flexibles System, die verwendet werden kann, um die Eigenschaften eines beliebigen Objekts animieren, nicht nur die Objekte anzeigen. Diese Flexibilität ermöglicht Animationen in verschiedene Klassen gekapselt werden, die für die Codefreigabe einfacher machen.


Anzeigen von Fenstern sind besser geeignet, für Anwendungen, die der älteren vor Android 3.0-APIs (API-Ebene 11) unterstützen müssen. Andernfalls sollten Anwendungen die neuere Eigenschaft Animations-API des Gründen verwenden, die oben erwähnt wurden.

Alle diese Frameworks sind geeignete Optionen jedoch möglichst Voreinstellung Eigenschaft Animationen, gewährt werden soll eine flexiblere-API zum Arbeiten mit unverändert. Eigenschaftenanimationen ermöglichen Animationslogik in verschiedene Klassen gekapselt werden, mit der für die Codefreigabe einfacher und Wartung des Programmcodes vereinfacht.


## <a name="accessibility"></a>Zugriff

Grafiken und Animationen verbessern von Android-apps ansprechend und Fun zu verwenden. Allerdings ist es wichtig zu beachten, dass einige Aktivitäten über Screenreaders Alternative Eingabegeräte, oder mit unterstützte Zoom auftreten.
Darüber hinaus können einige Aktivitäten ohne audio-Funktionen auftreten.

Apps in diesen Fällen mehr verwendet werden, wenn sie mit Eingabehilfen Bedenken entwickelt wurden: Bereitstellen von Hinweisen und Unterstützung der Navigation in der Benutzeroberfläche und das sicherstellen, Text-Inhalt oder Beschreibungen für grafische Elemente der Benutzeroberfläche vorhanden ist.

Verweisen auf [Googles Accessibility Guide](http://developer.android.com/guide/topics/ui/accessibility/) für Weitere Informationen zum Zugriff auf Android APIs nutzen.



## <a name="2d-graphics"></a>2D Grafiken

Zeichenbare Ressourcen handelt es sich um eine beliebte Technik in Android-Anwendungen. Da mit anderen Ressourcen zeichenbaren Ressourcen deklarative werden &ndash; sie in XML-Dateien definiert sind. Dieser Ansatz ermöglicht eine saubere Trennung des Codes von Ressourcen. Dies kann die Entwicklung und Verwaltung vereinfachen, da es nicht erforderlich ist, ändern Sie Code zum Aktualisieren oder Ändern von Grafiken in einer Android-Anwendung. Während zeichenbaren Ressourcen für viele einfache und allgemeine Grafik Anforderungen nützlich sind, fehlt sie jedoch die Leistungsfähigkeit und die Kontrolle über die Canvas-API.

Das andere Verfahren, mit der [Zeichenbereich](https://developer.xamarin.com/api/type/Android.Graphics.Canvas/) Objekt, das andere herkömmliche API-Frameworks wie z. B. "System.Drawing" oder des iOS-Core-Zeichnung sehr ähnlich ist. Mit dem Zeichenbereich-Objekt stellt die bestmögliche Kontrolle wie 2D Grafiken werden erstellt. Es eignet sich für Situationen, in denen eine zeichenbaren Ressource funktioniert nicht oder schwieriger zu arbeiten. Beispielsweise kann es erforderlich sein, zeichnen ein benutzerdefiniertes Schieberegler-Steuerelement, dessen Darstellung geändert wird, basierend auf Berechnungen im Zusammenhang mit dem Wert des Schiebereglers.

Betrachten wir zunächst zeichenbaren Ressourcen. Sie sind einfacher und decken die am häufigsten vorkommenden Fälle für das benutzerdefinierte zeichnen.


### <a name="drawable-resources"></a>Zeichenbaren Ressourcen

Zeichenbare Ressourcen werden in eine XML-Datei im Verzeichnis definiert `/Resources/drawable`. Im Gegensatz zum Einbetten von PNG oder JPEG, ist es nicht notwendig, Dichte-spezifische Versionen zeichenbaren Ressourcen bereitzustellen.
Zur Laufzeit eine Android-Anwendung lädt diese Ressourcen und verwenden die Anweisungen in diesen XML-Dateien enthalten sind, um 2D Grafiken erstellen.
Android definiert verschiedene Typen von zeichenbaren Ressourcen:

-   [ShapeDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Shape) &ndash; Dies ist ein zeichenbaren-Objekt, das eine primitive geometrische Form zeichnet und wendet einen eingeschränkten Satz von grafischen Auswirkungen auf die Form. Sie sind sehr hilfreich, z. B. Anpassen von Schaltflächen oder den Hintergrund des TextViews festlegen. Sehen wir ein Beispiel zum Verwenden einer `ShapeDrawable` weiter unten in diesem Artikel.

-   [StateListDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#StateList) &ndash; Dies ist eine zeichenbaren Ressource, deren Darstellung, die anhand des Zustands eines Widget-Steuerelements geändert wird. Beispielsweise kann eine Schaltfläche seine Darstellung abhängig davon, ob es oder nicht gedrückt wird ändern.

-   [LayerDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList) &ndash; diese zeichenbaren Ressource, die andere Drawables eine auf einem anderen aufeinander gestapelt werden. Ein Beispiel für eine *LayerDrawable* wird im folgenden Screenshot gezeigt:

    ![LayerDrawable-Beispiel](graphics-and-animation-images/image1.png)

-   [TransitionDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Transition) &ndash; Dies ist eine *LayerDrawable* jedoch mit einem Unterschied. Ein *TransitionDrawable* kann zum Animieren einer angezeigt über oben eine andere Ebene.

-   [LevelListDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#LevelList) &ndash; Dies ist vergleichbar mit einem *StateListDrawable* insofern, dass sie ein Image anhand bestimmter Bedingungen angezeigt werden. Anders als bei einem *StateListDrawable*, die *LevelListDrawable* zeigt ein Bild, das basierend auf einen ganzzahligen Wert. Ein Beispiel für eine *LevelListDrawable* würde die Stärke eines Signals WiFi angezeigt werden. Als die Stärke des WiFi Signal ändert wird die zeichenbaren, die angezeigt wird entsprechend geändert werden.

-   [ScaleDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Scale)/[ClipDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Clip) &ndash; wie der Name besagt, geben diese Drawables Skalierung und Funktionalität kürzen. Die *ScaleDrawable* eine andere zeichenbaren, While skaliert die *ClipDrawable* wird eine andere Drawable zugeschnitten werden soll.

-   [InsetDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Inset) &ndash; dieses zeichenbaren gelten Abstände auf den Seiten einer anderen zeichenbaren Ressource. Wird verwendet, wenn eine Sicht einen Hintergrund benötigt, der kleiner als die tatsächliche Grenzen für die Sicht ist.

-   XML [BitmapDrawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#Bitmap) &ndash; diese Datei ist ein Satz von Anweisungen in XML, die für eine tatsächliche Bitmap ausgeführt werden. Einige Aktionen, die Android ausführen kann, sind verteilen, dithering und Anti-Aliasing. Eine sehr häufig Verwendungen dieses ist eine Bitmap auf den Hintergrund eines Layouts Kachel.


#### <a name="drawable-example"></a>Zeichenbaren-Beispiel

Betrachten wir ein kurzes Beispiel zum Erstellen einer 2D Grafik mithilfe einer `ShapeDrawable`. Ein `ShapeDrawable` kann eine der vier grundlegenden Formen definieren: Rechteck, Oval, Linien- und Ring. Es ist auch möglich, Anwenden von grundlegenden Effekten, z. B. Farbverlauf, Farbe und Größe. Das folgende XML ist ein `ShapeDrawable` , finden Sie in der *AnimationsDemo* Begleit-Projekt (in der Datei `Resources/drawable/shape_rounded_blue_rect.xml`).
Definiert ein Rechteck mit einem lila Farbverlaufshintergrund und abgerundete Ecken:

```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android" android:shape="rectangle">
<!-- Specify a gradient for the background -->
<gradient android:angle="45"
          android:startColor="#55000066"
          android:centerColor="#00000000"
          android:endColor="#00000000"
          android:centerX="0.75" />

<padding android:left="5dp"
          android:right="5dp"
          android:top="5dp"
          android:bottom="5dp" />

<corners android:topLeftRadius="10dp"
          android:topRightRadius="10dp"
          android:bottomLeftRadius="10dp"
          android:bottomRightRadius="10dp" />
</shape>
```

Wir können diese zeichenbaren Ressource deklarativ in einem Layout oder andere Drawable wie in der folgenden XML-Code dargestellt verweisen:

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:background="#33000000">
    <TextView android:layout_width="wrap_content"
              android:layout_height="wrap_content"
              android:layout_centerInParent="true"
              android:background="@drawable/shape_rounded_blue_rect"
              android:text="@string/message_shapedrawable" />
</RelativeLayout>
```

Zeichenbare Ressourcen können auch programmgesteuert angewendet werden. Der folgende Codeausschnitt zeigt, wie den Hintergrund einer TextView programmgesteuert festgelegt wird:

```csharp
TextView tv = FindViewById<TextView>(Resource.Id.shapeDrawableTextView);
tv.SetBackgroundResource(Resource.Drawable.shape_rounded_blue_rect);
```

Um anzuzeigen, wie dies aussehen würde, führen Sie die *AnimationsDemo* Projekt, und wählen Sie das Shape zeichenbaren Element über das Hauptmenü. Es sollte ähnlich dem folgenden Screenshot angezeigt werden:

![Mit einem benutzerdefinierten Hintergrund mit einem Farbverlauf und abgerundeten Ecken zeichenbaren TextView](graphics-and-animation-images/image1.png)

Weitere Informationen zu den XML-Elemente und die Syntax des zeichenbaren Ressourcen, finden Sie in [Google Dokumentation](http://developer.android.com/guide/topics/resources/drawable-resource.html#Shape).


### <a name="using-the-canvas-drawing-api"></a>Mithilfe der Canvas-Zeichnung-API

Drawables sind leistungsstark, aber ihren Einschränkungen aufweisen. Bestimmte Aspekte sind nicht möglich oder sehr komplexen (zum Beispiel: Anwenden eines Filters auf ein Bild, das von einer Kamera auf dem Gerät übernommen wurde). Es wäre sehr schwer zu Eye Reduction mithilfe einer zeichenbaren Ressource gelten.
Stattdessen ermöglicht die Canvas-API eine Anwendung sehr eine präzisere Kontrolle selektiv Farben in einem bestimmten Teil des Bilds zu ändern.

Eine Klasse, die mit dem Zeichenbereich häufig verwendet wird, ist die [Paint](https://developer.xamarin.com/api/type/Android.Graphics.Paint/) Klasse. Diese Klasse enthält die Farbe und den Stil Informationen dazu, wie gezeichnet werden soll. Es wird verwendet, um Dinge bereitzustellen, z. B. Farbe und Transparenz.

Die Canvas-API verwendet die *übertragen des Modells* 2D Grafiken gezeichnet werden soll.
Vorgänge werden in aufeinander folgenden Ebenen bauen aufeinander auf angewendet. Jeder Vorgang werden einige der Bitmap für die zugrunde liegenden Bereich behandelt. Wenn Bereich überschneidet sich mit einem zuvor gezeichnete Bereich, wird die neue Farbe wird teilweise oder vollständig verdeckt die alte. Dies ist die gleiche Weise, die viele andere zeichnen APIs, z. B. "System.Drawing" und des iOS-Core-Grafiken arbeiten.

Es gibt zwei Möglichkeiten zum Abrufen einer `Canvas` Objekt. Die erste Möglichkeit umfasst das Definieren einer [Bitmap](https://developer.xamarin.com/api/type/Android.Graphics.Bitmap/) -Objekt, und klicken Sie dann instanziiert einen `Canvas` Objekt mit. Der folgende Codeausschnitt erstellt z. B. einen neuen Zeichenbereich mit einer zugrunde liegenden Bitmap:

```csharp
Bitmap bitmap = Bitmap.CreateBitmap(100, 100, Bitmap.Config.Argb8888);
Canvas canvas = new Canvas(b);
```

Die Möglichkeit zum Abrufen einer `Canvas` Objekt ist, indem die [OnDraw](https://developer.xamarin.com/api/member/Android.Views.View.OnDraw/) Rückrufmethode, die bereitgestellt wird die [Ansicht](https://developer.xamarin.com/api/type/Android.Views.View/) Basisklasse. Android ruft diese Methode auf, wenn er entscheidet, eine Sicht muss selbst zeichnen und übergibt ein `Canvas` Objekt für die Ansicht arbeiten.

Das Canvas-Klasse macht Methoden, um den Draw-Anweisungen programmgesteuert bereitzustellen. Zum Beispiel:

-   [Canvas.DrawPaint](https://developer.xamarin.com/api/member/Android.Graphics.Canvas.DrawPaint/p/Android.Graphics.Paint/) &ndash; füllt den gesamten Zeichenbereich Bitmap mit der angegebenen Farbe.

-   [Canvas.DrawPath](https://developer.xamarin.com/api/member/Android.Graphics.Canvas.DrawPath/p/Android.Graphics.Path/Android.Graphics.Paint/) &ndash; zeichnet die angegebene geometrische Form, die mit der angegebenen Farbe.

-   [Canvas.DrawText](https://developer.xamarin.com/api/member/Android.Graphics.Canvas.DrawText/p/System.String/System.Single/System.Single/Android.Graphics.Paint/) &ndash; zeichnet den Text auf der Canvas mit der angegebenen Farbe. Der Text gezeichnet wird, am Speicherort `x,y` .



#### <a name="drawing-with-the-canvas-api"></a>Zeichnen mit dem Zeichenbereich API

Sehen Sie ein Beispiel für das Canvas-API in Aktion an. Der folgende Codeausschnitt zeigt, wie eine Sicht gezeichnet werden soll:

```csharp
public class MyView : View
{
    protected override void OnDraw(Canvas canvas)
    {
        base.OnDraw(canvas);
        Paint green = new Paint {
            AntiAlias = true,
            Color = Color.Rgb(0x99, 0xcc, 0),
        };
        green.SetStyle(Paint.Style.FillAndStroke);

        Paint red = new Paint {
            AntiAlias = true,
            Color = Color.Rgb(0xff, 0x44, 0x44)
        };
        red.SetStyle(Paint.Style.FillAndStroke);

        float middle = canvas.Width * 0.25f;
        canvas.DrawPaint(red);
        canvas.DrawRect(0, 0, middle, canvas.Height, green);
    }
}
```

Dieser Code oben erstellt zuerst eine rote Farbe und ein grüner Paint-Objekt. Er füllt den Inhalt des Zeichenbereichs mit roten und dann wird angewiesen, den Zeichenbereich ein grünes Rechteck gezeichnet werden soll, die 25 % der Breite des Zeichenbereichs. Ein Beispiel hierfür kann anzeigen, indem im `AnimationsDemo` -Projekt, das mit dem Quellcode für diesen Artikel enthalten ist. Start der Anwendung und den Zeichnung-Artikel über das Hauptmenü auswählen, sollten wir einen Bildschirm, der etwa wie folgt:

![Bildschirm mit roter Farbe und Grün Paint-Objekten](graphics-and-animation-images/image3.png)


## <a name="animation"></a>Animation

Benutzer, z. B. Aufgaben, die in ihren Anwendungen verschieben. Animationen sind eine hervorragende Möglichkeit, die Benutzeroberfläche einer Anwendung zu verbessern und erleichtern hervorzuheben. Die besten Animationen sind diejenigen Argumente, die Benutzer beachten nicht, da sie natural fühlen. Android bietet der folgenden drei APIs für Animationen an:

-   **Anzeigen der Animation** &ndash; Dies ist die ursprüngliche API. Diese Animationen werden mit einer bestimmten Ansicht gebunden und können einfache Transformationen auf den Inhalt der Sicht ausführen. Aufgrund der Einfachheit halber wird diese API, die immer noch nützlich für Angaben wie alpha Animationen, Drehungen und So weiter.

-   **Eigenschaft Animation** &ndash; Eigenschaftenanimationen in Android 3.0 eingeführt wurden. Sie ermöglichen eine Anwendung, die fast alles dem animiert werden soll. Eigenschaftenanimationen können so ändern Sie eine Eigenschaft eines beliebigen Objekts verwendet werden, auch wenn dieses Objekt nicht auf dem Bildschirm sichtbar ist.

-   **Zeichenbaren Animation** &ndash; dies eine spezielle zeichenbare Ressource, die verwendet wird, um eine sehr einfache Animation anzuwenden, Layouts wirksam.

Eigenschaft Animation ist im Allgemeinen die bevorzugte System verwenden, wie es flexibler ist und weitere Features bietet.


### <a name="view-animations"></a>Anzeigen von Fenstern

Anzeigen von Fenstern sind mit Ansichten begrenzt und Animationen können nur auf Werte wie z. B. Start- und End Points, Größe, Drehung und Transparenz ausführen.
Diese Typen von Animationen in der Regel als bezeichnet *Tween Animationen*. Anzeigen von Fenstern können auf zwei Arten definiert werden &ndash; programmgesteuert im Code oder mithilfe von XML-Dateien. XML-Dateien sind die bevorzugte Methode zum Anzeigen von Fenstern, deklarieren, da sie besser lesbar und einfacher zu verwalten sind.

Die Animation XML-Dateien gespeichert werden, der `/Resources/anim` eines Projekts Xamarin.Android Verzeichnis. Diese Datei muss einen der folgenden Elemente als Stammelement aufweisen:

-   `alpha` &ndash; Eine Animation auf- oder Ausblenden der videoüberlagerung benötigt.

-   `rotate` &ndash; Eine Rotation der Animation.

-   `scale` &ndash; Eine Größe Animation.

-   `translate` &ndash; Eine horizontale bzw. vertikale Bewegung.

-   `set` &ndash; Ein Container, der eine oder mehrere der anderen Animation Elemente enthalten kann.

Standardmäßig werden alle Animationen in einer XML-Datei gleichzeitig angewendet werden. Damit Animationen sequenziell auftreten, setzen die `android:startOffset` Attribut auf eines der oben definierten Elemente.

Es ist möglich, um zu beeinflussen, die Änderungsrate in einer Animation mit einer *Interpolators*. Einer Interpolators ermöglicht Animationseffekte beschleunigt, wiederholt oder decelerated. Das Android-Framework bietet mehrere Interpolators ausgegeben, z. B. (aber nicht beschränkt auf):

-   `AccelerateInterpolator` / `DecelerateInterpolator` &ndash; Diese Interpolators erhöhen oder verringern die Änderungsrate in einer Animation.

-   `BounceInterpolator` &ndash; die Änderung am Ende animiert werden soll.

-   `LinearInterpolator` &ndash; die Rate der Änderungen ist konstant.


Das folgende XML zeigt ein Beispiel für eine Animationsdatei, die einige dieser Elemente kombiniert:

```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android=http://schemas.android.com/apk/res/android
     android:shareInterpolator="false">

    <scale android:interpolator="@android:anim/accelerate_decelerate_interpolator"
           android:fromXScale="1.0"
           android:toXScale="1.4"
           android:fromYScale="1.0"
           android:toYScale="0.6"
           android:pivotX="50%"
           android:pivotY="50%"
           android:fillEnabled="true"
           android:fillAfter="false"
           android:duration="700" />

    <set android:interpolator="@android:anim/accelerate_interpolator">
        <scale android:fromXScale="1.4"
               android:toXScale="0.0"
               android:fromYScale="0.6"
               android:toYScale="0.0"
               android:pivotX="50%"
               android:pivotY="50%"
               android:fillEnabled="true"
               android:fillBefore="false"
               android:fillAfter="true"
               android:startOffset="700"
               android:duration="400" />

        <rotate android:fromDegrees="0"
                android:toDegrees="-45"
                android:toYScale="0.0"
                android:pivotX="50%"
                android:pivotY="50%"
                android:fillEnabled="true"
                android:fillBefore="false"
                android:fillAfter="true"
                android:startOffset="700"
                android:duration="400" />
    </set>
</set>
```

Diese Animation werden alle Animationen gleichzeitig ausführen. Der ersten Skala Animation wird dehnen Sie das Bild horizontal und vertikal verkleinern, und klicken Sie dann das Bild wird gleichzeitig 45 Grad gegen den Uhrzeigersinn gedreht und verkleinern, verschwinden aus dem Bildschirm.

Die Animation kann programmgesteuert auf eine Ansicht angewendet werden, durch die Animation überhöhte und diese dann auf eine Sicht angewendet. Android bietet die Hilfsklasse `Android.Views.Animations.AnimationUtils` wird, die eine Animation Ressource vergrößern und zurückgeben eine Instanz von `Android.Views.Animations.Animation`. Dieses Objekt wird durch Aufrufen einer Ansicht angewendet `StartAnimation` und übergibt die `Animation` Objekt. Der folgende Codeausschnitt zeigt ein Beispiel dafür:

```csharp
Animation myAnimation = AnimationUtils.LoadAnimation(Resource.Animation.MyAnimation);
ImageView myImage = FindViewById<ImageView>(Resource.Id.imageView1);
myImage.StartAnimation(myAnimation);
```

Nun, wir eine grundlegende Kenntnisse über die Funktionsweise von Animationen anzeigen haben, können die Eigenschaftenanimationen verschieben.


### <a name="property-animations"></a>Eigenschaftenanimationen

Eigenschaft Animatoren sind eine neue API, die in Android 3.0 eingeführt wurde.
Sie bieten eine mehr erweiterbare API, die verwendet werden kann, um eine beliebige Eigenschaft für jedes Objekt animiert werden soll.

Alle Eigenschaftenanimationen werden erstellt, indem die Instanzen der [Animator](https://developer.xamarin.com/api/type/Android.Animation.Animator/) Unterklasse. Anwendungen diese Klasse nicht direkt verwendet wird, wird stattdessen eine der zugehörigen Unterklassen:

-   [ValueAnimator](https://developer.xamarin.com/api/type/Android.Animation.ValueAnimator/) &ndash; diese Klasse ist die wichtigste Klasse in der gesamten Eigenschaft Animations-API. Berechnet die Werte der Eigenschaften, die geändert werden müssen. Die `ViewAnimator` wird nicht direkt aktualisiert diese Werte; stattdessen löst Ereignisse, die zum Aktualisieren der animierten Objekte verwendet werden können.

-   [ObjectAnimator](https://developer.xamarin.com/api/type/Android.Animation.ObjectAnimator/) &ndash; diese Klasse ist eine Unterklasse von `ValueAnimator` . Es bedeutet, dass den Prozess von Animieren von Objekten zu vereinfachen, indem Sie ein Zielobjekt und die zu aktualisierende Eigenschaft akzeptiert.

-   [AnimationSet](https://developer.xamarin.com/api/type/Android.Animation.AnimatorSet/) &ndash; diese Klasse ist verantwortlich für die Orchestrierung wie Animationen in Bezug aufeinander ausgeführt. Animationen möglicherweise gleichzeitig, sequenziell oder mit einer angegebenen Verzögerung zwischen ihnen ausführen.


*Bewerter* sind spezielle Klassen, die von Animatoren verwendet werden, um die neuen Werte während einer Animation zu berechnen. Standardmäßig bietet Android die folgenden Auswerter:

-   [IntEvaluator](https://developer.xamarin.com/api/type/Android.Animation.IntEvaluator/) &ndash; Werte für die ganzzahligen Eigenschaften berechnet.

-   [FloatEvaluator](https://developer.xamarin.com/api/type/Android.Animation.FloatEvaluator/) &ndash; berechnet Werte für Eigenschaften von "float".

-   [ArgbEvaluator](https://developer.xamarin.com/api/type/Android.Animation.ArgbEvaluator/) &ndash; Werte für die Farbe Eigenschaften berechnet.

Wenn die Eigenschaft, die animiert wird keine `float`, `int` oder Farbe, Anwendungen erstellen ihre eigenen Ausdrucksauswertungsfehler durch Implementieren der `ITypeEvaluator` Schnittstelle. (Implementieren von benutzerdefinierten Bewerter ist Gegenstand des in diesem Thema.)

#### <a name="using-the-valueanimator"></a>Verwenden die ValueAnimator

Es gibt zwei Komponenten für Animationen: Berechnen der animierten Werte, und klicken Sie dann diese Werte für Eigenschaften, die auf ein Objekt festgelegt. 
[ValueAnimator](https://developer.xamarin.com/api/type/Android.Animation.ValueAnimator/) wird nur die Werte berechnet, sondern es funktioniert nicht für Objekte direkt. Stattdessen werden Objekte innerhalb eines ereignishandlers aktualisiert werden, die während der Ausführung der Animation aufgerufen wird. Dadurch können mehrere Eigenschaften von einem animierten Wert aktualisiert werden.

Sie rufen Sie eine Instanz des `ValueAnimator` durch Aufrufen einer der folgenden Factorymethoden:

-  `ValueAnimator.OfInt`
-  `ValueAnimator.OfFloat`
-  `ValueAnimator.OfObject`

Ist diese "Fertig" ändert, die `ValueAnimator` Instanz benötigen Dauer festgelegt und kann dann gestartet werden. Im folgende Beispiel wird gezeigt, wie auf einen Wert zwischen 0 und 1 über den Zeitraum von 1000 Millisekunden animieren wird:

```csharp
ValueAnimator animator = ValueAnimator.OfInt(0, 100);
animator.SetDuration(1000);
animator.Start();
```

Aber selbst, der obige Codeausschnitt ist nicht sehr sinnvoll &ndash; der Animator zwar ausgeführt, aber es ist kein Ziel für den aktualisierten Wert. Die `Animator` Klasse löst die Update-Ereignis aus, wenn er entscheidet sich, dass es notwendig um Listener über einen neuen Wert zu informieren. Anwendungen gegebenenfalls über einen Ereignishandler zum Reagieren auf dieses Ereignis, wie im folgenden Codeausschnitt gezeigt:

```csharp
MyCustomObject myObj = new MyCustomObject();
myObj.SomeIntegerValue = -1;

animator.Update += (object sender, ValueAnimator.AnimatorUpdateEventArgs e) =>
{
    int newValue = (int) e.Animation.AnimatedValue;
    // Apply this new value to the object being animated.
    myObj.SomeIntegerValue = newValue;
};
```

Nun mit dem einen Überblick über `ValueAnimator`, können Sie weitere Informationen zu den `ObjectAnimator`.

#### <a name="using-the-objectanimator"></a>Verwenden die ObjectAnimator

[ObjectAnimator](https://developer.xamarin.com/api/type/Android.Animation.ObjectAnimator/) ist eine Unterklasse von `ViewAnimator` , die die zeitliche Steuerung-Modul und der Wert der Berechnung des kombiniert die `ValueAnimator` mit Logik zum Verknüpfen von Ereignishandlern. Die `ValueAnimator` Anwendungen explizit einen Ereignishandler von Netzwerkdaten erfordert &ndash; `ObjectAnimator` übernimmt diesen Schritt für uns.

Die API für `ObjectAnimator` ist vergleichbar mit der API für `ViewAnimator`, erfordert jedoch, dass Sie das Objekt und den Namen der zu aktualisierende Eigenschaft angeben. Das folgende Beispiel zeigt ein Beispiel der Verwendung von `ObjectAnimator`:

```csharp
MyCustomObject myObj = new MyCustomObject();
myObj.SomeIntegerValue = -1;

ObjectAnimator animator = ObjectAnimator.OfFloat(myObj, "SomeIntegerValue", 0, 100);
animator.SetDuration(1000);
animator.Start();
```

Wie Sie aus dem vorherigen Codeausschnitt sehen `ObjectAnimator` können zu verringern und vereinfachen Sie den Code, der zum Animieren eines Objekts erforderlich ist.


### <a name="drawable-animations"></a>Zeichenbaren Animationen

Die endgültige Animations-API wird der zeichenbaren Animations-API. Zeichenbare Animationen eine Reihe von zeichenbaren Ressourcen eine nach der anderen geladen und sequenziell, Anzeigen einer Flip-It-Cartoon ähnelt.

Zeichenbare Ressourcen werden in eine XML-Datei mit definiert ein `<animation-list>` -Element als Stammelement und eine Reihe von `<item>` Elemente, die alle Frames in die Animation definieren. Diese XML-Datei befindet sich in der `/Resource/drawable` Ordner der Anwendung. Das folgende XML ist ein Beispiel für eine zeichenbaren Animation:

```xml
<animation-list xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:drawable="@drawable/asteroid01" android:duration="100" />
  <item android:drawable="@drawable/asteroid02" android:duration="100" />
  <item android:drawable="@drawable/asteroid03" android:duration="100" />
  <item android:drawable="@drawable/asteroid04" android:duration="100" />
  <item android:drawable="@drawable/asteroid05" android:duration="100" />
  <item android:drawable="@drawable/asteroid06" android:duration="100" />
</animation-list>
```

Diese Animation wird durch sechs Frames ausgeführt. Die `android:duration` -Attribut deklariert, wie lange jedes Bild angezeigt wird. Die nächste Codeausschnitt zeigt ein Beispiel zum Erstellen einer Animation zeichenbaren und klickt der Benutzer eine Schaltfläche auf dem Bildschirm gestartet:

```csharp
AnimationDrawable _asteroidDrawable;

protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);

    _asteroidDrawable = (Android.Graphics.Drawables.AnimationDrawable)
    Resources.GetDrawable(Resource.Drawable.spinning_asteroid);

    ImageView asteroidImage = FindViewById<ImageView>(Resource.Id.imageView2);
    asteroidImage.SetImageDrawable((Android.Graphics.Drawables.Drawable) _asteroidDrawable);

    Button asteroidButton = FindViewById<Button>(Resource.Id.spinAsteroid);
    asteroidButton.Click += (sender, e) =>
    {
        _asteroidDrawable.Start();
    };
}
```

Zu diesem Zeitpunkt haben wir die Grundlagen der Animation APIs zur Verfügung, in einer Android-Anwendung behandelt.


## <a name="summary"></a>Zusammenfassung

In diesem Artikel eingeführt wurden viele neue Konzepte und zu den API einige Grafiken zu einer Android-Anwendung hinzufügen können. Zunächst werden erläutert die verschiedenen 2D Grafik-APIs und veranschaulicht, wie Android-Anwendungen direkt auf dem Bildschirm mithilfe eines Canvas-Objekts gezeichnet werden kann. Wir haben auch einige Alternativen Techniken, mit die Grafiken, deklarativ mithilfe von XML-Dateien erstellt werden können. AnonymousMethod wir auf der alten und neuen APIs zum Erstellen von Animationen in Android erläutert.



## <a name="related-links"></a>Verwandte Links

- [Animation-Demo (Beispiel)](https://developer.xamarin.com/samples/monodroid/AnimationDemo)
- [Animation und Grafiken](http://developer.android.com/guide/topics/graphics/index.html)
- [Verwenden Animationen auf Ihre Mobile Apps lebendig werden lassen](http://youtu.be/ikSk_ILg3d0)
- [AnimationDrawable](https://developer.xamarin.com/api/type/Android.Graphics.Drawables.AnimationDrawable/)
- [Canvas](https://developer.xamarin.com/api/type/Android.Graphics.Canvas/)
- [Objekt Animator](https://developer.xamarin.com/api/type/Android.Animation.ObjectAnimator/)
- [Wert Animator](https://developer.xamarin.com/api/type/Android.Animation.ValueAnimator/)
