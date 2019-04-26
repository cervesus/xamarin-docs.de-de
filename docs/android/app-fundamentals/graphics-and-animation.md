---
title: Grafiken und Animationen
description: Android bietet ein sehr umfangreichen und verschiedenartigen-Framework für die Unterstützung von Direct2D-Grafiken und Animationen. Dieses Thema führt dieser Frameworks, und es wird erläutert, wie Sie benutzerdefinierte Grafiken und Animationen für die Verwendung in einer Xamarin.Android-Anwendung zu erstellen.
ms.prod: xamarin
ms.assetid: 80086318-6FE4-4711-9A71-5C8F8C28C754
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 7f4f7fd3af1e90307a84037f01ddf8e52b1ee030
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61020027"
---
# <a name="graphics-and-animation"></a>Grafiken und Animationen

_Android bietet ein sehr umfangreichen und verschiedenartigen-Framework für die Unterstützung von Direct2D-Grafiken und Animationen. Dieses Thema führt dieser Frameworks, und es wird erläutert, wie Sie benutzerdefinierte Grafiken und Animationen für die Verwendung in einer Xamarin.Android-Anwendung zu erstellen._


## <a name="overview"></a>Übersicht

Trotz auf Geräten, die traditionell begrenzte Möglichkeiten sind ausgeführt werden, haben die höchste Bewertung mobile Anwendungen häufig eine anspruchsvolle Benutzererfahrung (UX), vollständig mit Grafiken in hoher Qualität und Animationen, die eine intuitive, reaktionsfähige, dynamische Verhalten zu erzielen. Nutzen Sie mobile Anwendungen besser und zu verbessern, haben Benutzer mehr und mehr von Anwendungen zu erwarten begonnen.

Für uns haben moderne mobile Plattformen Glücklicherweise sehr leistungsstarke Frameworks zum Erstellen anspruchsvoller Animationen und benutzerdefinierten Grafiken und behalten Sie einfache Bedienung. Dadurch können Entwickler, umfassenden Interaktivität mit sehr geringem Aufwand hinzuzufügen.

Benutzeroberflächen-API-Frameworks unter Android können grob in zwei Kategorien unterteilt werden: Grafiken und Animationen.

Grafiken werden weiter in unterschiedliche Ansätze für die 2D-und 3D-Grafik aufgeteilt. 3D-Grafiken stehen über eine Vielzahl von Frameworks wie OpenGL ES (eine mobile bestimmte Version von OpenGL) und Drittanbieter-Frameworks wie z. B. MonoGame (eine plattformübergreifendes Toolkit kompatibel mit dem XNA-Toolkit) erstellt werden. 3D-Grafiken sind, zwar nicht in den Rahmen dieses Artikels sprengen untersuchen wir die integrierten Direct2D zeichnen Techniken.

Android bietet zwei verschiedene APIs zum Erstellen von 2D-Grafiken. Eine ist eine allgemeine deklarativen Ansatz und die andere eine programmgesteuerte Low-Level-API:

-   **Zeichenbare Ressourcen** &ndash; diese werden verwendet, um benutzerdefinierte Grafiken entweder programmgesteuert oder (in der Regel) erstellen, indem die zeichnungsanweisungen in XML-Dateien einbetten. Zeichenbare Ressourcen werden in der Regel als XML-Dateien definiert, die Anweisungen oder Aktionen für Android zum Rendern einer Direct2D-Grafik enthalten. 

-   **Canvas** &ndash; Dies ist eine low-Level-API, bei der direkt auf einer zugrunde liegenden Bitmap zeichnen. Es bietet äußerst präzise steuern, was angezeigt wird. 

Zusätzlich zu diesen Techniken 2D-Grafiken bietet Android auch mehrere Möglichkeiten, um Animationen zu erstellen:

-   **Drawable-Animationen** &ndash; Android unterstützt auch bekannt als Frame für Frame-Animationen *Drawable Animation*. Dies ist der einfachste Animations-API. Android sequenziell geladen und zeigt zeichenbare Ressourcen in der Sequenz (ähnlich wie eine Cartoon).

-   **Anzeigen von Animationen** &ndash; *Ansicht Animationen* sind die ursprüngliche Animation-APIs in Android und in allen Versionen von Android verfügbar. Diese API ist insofern eingeschränkt, es nur mit der Objekte anzeigen funktioniert und kann nur einfache Transformationen für diese Sichten ausführen.
    Ansicht Animationen werden in der Regel definiert, in XML-Dateien finden Sie in der `/Resources/anim` Ordner.

-   **Eigenschaftenanimationen** &ndash; Android 3.0 eingeführt, einen neuen Satz von Animations-API des genannt *Eigenschaftenanimationen*. Diese neue API eingeführt, eine erweiterbare und flexible System, das verwendet werden kann, um die Eigenschaften eines beliebigen Objekts animieren, nicht nur Objekte anzeigen. Dank dieser Flexibilität können Animationen in unterschiedliche Klassen gekapselt werden, die für die Codefreigabe einfacher machen.


Ansicht-Animationen sind besser geeignet für Anwendungen, die der ältere vor Android 3.0-APIs (API-Ebene 11) unterstützen müssen. Andernfalls sollten Anwendungen die neuere Eigenschaft Animations-API Gründen verwenden, die weiter oben erwähnt wurden.

Alle diese Frameworks sind geeignete Optionen, jedoch möglichst Einstellung zu Eigenschaftenanimationen, gewährt werden soll eine flexiblere API zur Arbeit mit unverändert. Eigenschaftenanimationen ermöglichen die Animationslogik auf gekapselt werden, in verschiedene Klassen, die für die Codefreigabe einfacher und Wartung des Programmcodes vereinfacht.


## <a name="accessibility"></a>Zugriff

Grafiken und Animationen können Sie Android-apps reizvoll machen und zu verwenden; Allerdings ist es wichtig, daran denken, dass einige Interaktionen über Screenreader, alternative Eingabegeräte, oder mit persönlichen Zoom auftreten.
Darüber hinaus können einige Interaktionen ohne Audiofunktionen auftreten.

Apps sind in diesen Situationen besser verwendbar, wenn sie Barrierefreiheit entwickelt wurden: Bereitstellen von Hinweisen und Unterstützung der Navigation in der Benutzeroberfläche sowie die Gewährleistung, Text-Inhalt oder Beschreibungen für grafische Elemente der Benutzeroberfläche vorhanden ist.

Finden Sie unter [Googles Accessibility Guide](https://developer.android.com/guide/topics/ui/accessibility/) für Weitere Informationen zur Verwendung von Android-Eingabehilfen-APIs nutzen können.



## <a name="2d-graphics"></a>2D-Grafiken

Drawable-Ressourcen sind eine beliebte Methode bei der Android-Anwendungen. Wie bei anderen Ressourcen können zeichenbare Ressourcen deklarative &ndash; sie in XML-Dateien definiert sind. Dieser Ansatz ermöglicht eine saubere Trennung von Code von Ressourcen. Dies kann die Entwicklung und Wartung vereinfachen, da es nicht erforderlich, Code zum Aktualisieren oder ändern die Abbildungen in einer Android-Anwendung zu ändern ist. Während zeichenbare Ressourcen für viele einfache und häufige grafische Anforderungen nützlich sind, verfügen sie jedoch die Leistungsfähigkeit und die Kontrolle über die Leinwand-API.

Das andere Verfahren, mit der [Canvas](https://developer.xamarin.com/api/type/Android.Graphics.Canvas/) Objekt, das ist sehr ähnlich anderen herkömmlichen-API-Frameworks wie z. B. "System.Drawing" oder iOS Core-Zeichnung. Verwenden das Canvas-Objekt bietet die beste Steuerung wie 2D-Grafiken werden erstellt. Es eignet sich für Situationen, in denen eine Drawable Ressource funktioniert nicht oder schwer gearbeitet. Beispielsweise ist es möglicherweise notwendig, um ein benutzerdefiniertes Schieberegler-Steuerelement, dessen Darstellung geändert wird, basierend auf Berechnungen, die im Zusammenhang mit dem Wert des Schiebereglers zu zeichnen.

Betrachten wir zunächst zeichenbare Ressourcen. Sie sind einfacher und decken die am häufigsten vorkommenden Fälle für das benutzerdefinierte zeichnen.


### <a name="drawable-resources"></a>Drawable-Ressourcen

Drawable-Ressourcen werden in eine XML-Datei im Verzeichnis definiert `/Resources/drawable`. Im Gegensatz zum Einbetten von PNG oder JPEG ist es nicht erforderlich, um Dichte prozessorspezifische Assemblyversionen zeichenbare Ressourcen bereitzustellen.
Zur Laufzeit eine Android-Anwendung lädt diese Ressourcen und verwenden die Anweisungen in diesen XML-Dateien enthalten sind, um 2D-Grafiken zu erstellen.
Android definiert verschiedene Typen von zeichenbare Ressourcen:

-   [ShapeDrawable](https://developer.android.com/guide/topics/resources/drawable-resource.html#Shape) &ndash; Dies ist ein Drawable-Objekt, zeichnet eine primitive geometrische Form, und einen begrenzten Satz von grafischen Auswirkungen auf die Form angewendet. Sie sind sehr nützlich, z. B. Schaltflächen anpassen oder den Hintergrund von TextViews festlegen. Sehen wir ein Beispiel zur Verwendung einer `ShapeDrawable` weiter unten in diesem Artikel.

-   [StateListDrawable](https://developer.android.com/guide/topics/resources/drawable-resource.html#StateList) &ndash; Dies ist eine Drawable-Ressource, deren Darstellung, die basierend auf den Zustand eines Widget-Steuerelements geändert wird. Beispielsweise kann eine Schaltfläche seine Darstellung abhängig davon, ob es oder nicht gedrückt wird ändern.

-   [LayerDrawable](https://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList) &ndash; diese Drawable-Ressource, die mehrere andere zeichenbarer Ressourcen eine übereinander gestapelt werden. Ein Beispiel für eine *LayerDrawable* wird im folgenden Screenshot dargestellt:

    ![LayerDrawable-Beispiel](graphics-and-animation-images/image1.png)

-   [TransitionDrawable](https://developer.android.com/guide/topics/resources/drawable-resource.html#Transition) &ndash; Dies ist eine *LayerDrawable* jedoch einen Unterschied. Ein *TransitionDrawable* kann nur eine Ebene angezeigt über oben einen anderen animieren.

-   [LevelListDrawable](https://developer.android.com/guide/topics/resources/drawable-resource.html#LevelList) &ndash; Dies ähnelt sehr einer *StateListDrawable* darin, dass sie ein Image basierend auf bestimmten Bedingungen angezeigt werden. Anders als bei einem *StateListDrawable*, *LevelListDrawable* zeigt ein Bild, das basierend auf einen ganzzahligen Wert. Ein Beispiel für eine *LevelListDrawable* wäre, um die Stärke eines Signals WiFi anzuzeigen. Als die Stärke der WiFi Signal Änderungen ändert die drawable, die angezeigt wird, sich entsprechend.

-   [ScaleDrawable](https://developer.android.com/guide/topics/resources/drawable-resource.html#Scale)/[ClipDrawable](https://developer.android.com/guide/topics/resources/drawable-resource.html#Clip) &ndash; wie der Name besagt, Skalierung und Funktionalität clipping diese zeichenbarer Ressourcen bereitstellen. Die *ScaleDrawable* wird skaliert, eine andere Drawable, solange die *ClipDrawable* ist beschnitten eine andere Drawable.

-   [InsetDrawable](https://developer.android.com/guide/topics/resources/drawable-resource.html#Inset) &ndash; diese Drawable gelten Abstände an den Seiten von einer anderen Drawable-Ressource. Es wird verwendet, wenn eine Ansicht ein Hintergrunds benötigt, das kleiner als die tatsächliche Begrenzungen der Ansicht ist.

-   XML [BitmapDrawable](https://developer.android.com/guide/topics/resources/drawable-resource.html#Bitmap) &ndash; diese Datei ist ein Satz von Anweisungen in XML, die auf einer realen Bitmap ausgeführt werden. Einige Aktionen, die Android ausführen können, sind verteilen, dithering und Anti-Aliasing. Eine sehr häufige Verwendungsmöglichkeiten dieses ist eine Bitmap auf dem Hintergrund eines Layouts Kachel.


#### <a name="drawable-example"></a>Drawable-Beispiel

Sehen wir uns ein schnelles Beispiel zum Erstellen einer 2D Grafik mithilfe einer `ShapeDrawable`. Ein `ShapeDrawable` können eine der vier grundlegenden Formen definieren: Rechteck, Ellipse, Linie und Ring. Es ist auch möglich, einfache Effekte wie Farbverlauf, Farbe und Größe anzuwenden. Das folgende XML ist eine `ShapeDrawable` , finden Sie unter den *AnimationsDemo* Begleit-Projekt (in der Datei `Resources/drawable/shape_rounded_blue_rect.xml`).
Es definiert ein Rechteck mit violettem Hintergrund mit Farbverlauf und abgerundete Ecken:

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

Wir können diese Drawable Ressource deklarativ in einem Layout oder andere Drawable wie in den folgenden XML-Code dargestellt verweisen:

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

Drawable-Ressourcen können auch programmgesteuert angewendet werden. Der folgende Codeausschnitt zeigt, wie den Hintergrund des eine TextView programmgesteuert festgelegt wird:

```csharp
TextView tv = FindViewById<TextView>(Resource.Id.shapeDrawableTextView);
tv.SetBackgroundResource(Resource.Drawable.shape_rounded_blue_rect);
```

Um festzustellen, wie dies aussehen würde, führen Sie die *AnimationsDemo* Projekt, und wählen Sie im Hauptmenü auf die Form Drawable-Element. Es sollte etwa wie im folgenden Screenshot angezeigt werden:

![TextView mit einem benutzerdefinierten Hintergrund mit einem Farbverlauf und abgerundeten Ecken drawable](graphics-and-animation-images/image1.png)

Weitere Informationen über die XML-Elemente und die Syntax der zeichenbare Ressourcen finden Sie in [Google Dokumentation](https://developer.android.com/guide/topics/resources/drawable-resource.html#Shape).


### <a name="using-the-canvas-drawing-api"></a>Mithilfe der Canvas-Zeichnung-API

Zeichenbarer Ressourcen sind leistungsstark, aber ihren Einschränkungen haben. Bestimmte Dinge sind nicht möglich oder sehr komplex (z. B.: Anwenden eines Filters auf ein Bild, das von einer Kamera auf dem Gerät erstellt wurde). Es wäre sehr schwierig, eines Rote-Augen-Reduzierung mithilfe einer zeichenbare Ressourcen gelten.
Stattdessen können die Leinwand-API eine Anwendung sehr präzise steuerungsmöglichkeiten für die Farben in einem bestimmten Teil des Bilds selektiv zu ändern.

Eine Klasse, die häufig mit der Canvas verwendet wird, ist die [Paint](https://developer.xamarin.com/api/type/Android.Graphics.Paint/) Klasse. Diese Klasse enthält die Farbe und den Stil-Informationen zur Verwendung von gezeichnet werden soll. Es wird verwendet, um Dinge wie eine Farbe und Transparenz bereitzustellen.

Die Leinwand-API verwendet die *übertragen des Modells* 2D-Grafiken zu zeichnen.
Vorgänge werden in aufeinander folgenden Ebenen übereinander angewendet. Jeder Vorgang wird einen Bereich des zugrunde liegenden Bitmap behandelt. Wenn der Bereich einen zuvor gezeichneten Bereich überschneidet, wird die neue Farbe wird teilweise oder vollständig verdeckt die alte. Dies ist die gleiche Weise, die viele andere Zeichnungs-APIs wie z. B. "System.Drawing" und der iOS-Core-Grafiken zu arbeiten.

Es gibt zwei Methoden zum Abrufen einer `Canvas` Objekt. Die erste Möglichkeit umfasst das Definieren einer [Bitmap](https://developer.xamarin.com/api/type/Android.Graphics.Bitmap/) -Objekt, und klicken Sie dann Instanziieren einer `Canvas` Objekt mit. Der folgende Codeausschnitt erstellt z. B. einen neuen Zeichenbereich mit einem zugrunde liegenden Bitmap an:

```csharp
Bitmap bitmap = Bitmap.CreateBitmap(100, 100, Bitmap.Config.Argb8888);
Canvas canvas = new Canvas(b);
```

Die andere Möglichkeit zum Abrufen einer `Canvas` Objekt ist, indem die [OnDraw](https://developer.xamarin.com/api/member/Android.Views.View.OnDraw/) Callback-Methode, die bereitgestellt wird die [Ansicht](https://developer.xamarin.com/api/type/Android.Views.View/) Basisklasse. Android ruft diese Methode auf, wenn er entscheidet, eine Sicht muss sich selbst zu zeichnen und übergibt eine `Canvas` Objekt für die Ansicht mit arbeiten.

Die Canvas-Klasse macht Methoden, um den Draw-Anweisungen im Programmcode angeben. Zum Beispiel:

-   [Canvas.DrawPaint](https://developer.xamarin.com/api/member/Android.Graphics.Canvas.DrawPaint/p/Android.Graphics.Paint/) &ndash; füllt den gesamten Zeichenbereich Bitmap mit der angegebenen Farbe.

-   [Canvas.DrawPath](https://developer.xamarin.com/api/member/Android.Graphics.Canvas.DrawPath/p/Android.Graphics.Path/Android.Graphics.Paint/) &ndash; zeichnet die angegebene geometrische Form, die mit der angegebenen Farbe.

-   [Canvas.DrawText](https://developer.xamarin.com/api/member/Android.Graphics.Canvas.DrawText/p/System.String/System.Single/System.Single/Android.Graphics.Paint/) &ndash; zeichnet den Text auf der Canvas mit der angegebenen Farbe. Der Text gezeichnet wird, am Speicherort `x,y` .



#### <a name="drawing-with-the-canvas-api"></a>Zeichnen mit der Canvas-API

Sehen Sie ein Beispiel für die Leinwand-API in Aktion an. Der folgende Codeausschnitt zeigt eine Ansicht zu zeichnen:

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

Diesem Code erstellt zunächst eine rote Farbe und eine grüne Farbe-Objekt. Es füllt den Inhalt des Zeichenbereichs mit roten und weist dann auf den Zeichenbereich, um ein grünes Rechteck zu zeichnen, das 25 % der Breite des Zeichenbereichs. Ein Beispiel hierfür sehen `AnimationsDemo` -Projekt, das mit dem Quellcode für diesen Artikel enthalten ist. Start der Anwendung aus, und den zeichnen-Artikel über das Hauptmenü auswählen, sollten wir einen Bildschirm ähnlich dem folgenden:

![Bildschirm mit roter Farbe und Grün Paint-Objekten](graphics-and-animation-images/image3.png)


## <a name="animation"></a>Animation

Benutzer, wie Dinge, die in ihren Anwendungen verschieben. Animationen sind eine hervorragende Möglichkeit zum Verbessern der Benutzeroberfläche einer Anwendung und es besonders hervorzuheben. Die beste Animationen sind diejenigen, die Benutzer bemerken nicht, da sie natürliche das Gefühl haben. Android bietet die folgenden drei APIs für Animationen an:

-   **Anzeigen der Animation** &ndash; Dies ist die original-API. Diese Animationen an eine bestimmte Ansicht gebunden sind und einfache Transformationen auf den Inhalt der Ansicht ausführen können. Aufgrund dessen der Einfachheit halber diese API, die nach wie vor nützlich für Dinge wie alpha Animationen, Drehungen und So weiter.

-   **Animation der Eigenschaft** &ndash; Eigenschaftenanimationen in Android 3.0 eingeführt wurden. Sie kann es sich um eine Anwendung, um nahezu alles zu animieren. Eigenschaftenanimationen können so ändern Sie jede Eigenschaft eines beliebigen Objekts verwendet werden, auch wenn dieses Objekt nicht auf dem Bildschirm sichtbar ist.

-   **Drawable-Animation** &ndash; dieses Layouts eine spezielle Drawable-Ressource, die verwendet wird, um eine sehr einfache Animation anzuwenden Auswirkungen.

Animation der Eigenschaft ist im Allgemeinen das bevorzugte System ist flexibler und weitere Features bietet.


### <a name="view-animations"></a>Anzeigen von Animationen

Ansicht-Animationen sind auf Ansichten und Animationen können nur auf Werte wie z. B. Start- und End Points, Größe, Drehung und Transparenz ausführen.
Diese Typen von Animationen in der Regel als bezeichnet *Tween Animationen*. Ansicht-Animationen können zwei Arten definiert werden &ndash; programmgesteuert im Code oder mithilfe von XML-Dateien. XML-Dateien sind die bevorzugte Methode zum Deklarieren von Animationen anzeigen, wie sie besser lesbar und einfacher zu verwalten sind.

Werden die Animation XML-Dateien gespeichert werden, der `/Resources/anim` Verzeichnis ein Xamarin.Android-Projekt. Diese Datei muss eines der folgenden Elemente als Stammelement haben:

-   `alpha` &ndash; Eine Animation dem Einblenden oder Ausblenden der videoüberlagerung benötigt.

-   `rotate` &ndash; Eine Drehungsanimation.

-   `scale` &ndash; Eine Animation, Größenänderung von Fenstern annimmt.

-   `translate` &ndash; Eine horizontale bzw. vertikale Bewegung.

-   `set` &ndash; Ein Container, der eine oder mehrere der anderen Animation Elemente enthalten kann.

Standardmäßig werden alle Animationen in eine XML-Datei gleichzeitig angewendet werden. Animationen, die sequenziell auftreten, legen fest, um die `android:startOffset` Attribut auf eine der oben beschriebenen Elemente.

Es ist möglich, die Änderungsrate in einer Animation mit Auswirkungen auf eine *Interpolators*. Ein Interpolators ermöglicht es Animationseffekten, beschleunigt, wiederholt oder decelerated werden soll. Das Android-Framework bietet mehrere Interpolators standardmäßig wie z. B. (aber nicht beschränkt auf):

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

In dieser Animation werden alle Animationen gleichzeitig ausführen. Die erste Animation für die Skalierung wird dehnen Sie das Bild horizontal und vertikal verkleinern, und klicken Sie dann das Image wird gleichzeitig 45 Grad gegen den Uhrzeigersinn gedreht und verkleinern, auf dem Bildschirm ausgeblendet.

Die Animation kann programmgesteuert auf eine Ansicht angewendet werden, durch die Animation jedoch und das anschließende Anwenden einer Ansicht verwendet wird. Android bietet die Hilfsklasse `Android.Views.Animations.AnimationUtils` , die eine Animation Ressource Vergrößerung und zurückgeben eine Instanz von `Android.Views.Animations.Animation`. Dieses Objekt wird durch Aufrufen einer Ansicht angewendet `StartAnimation` und übergeben die `Animation` Objekt. Der folgende Codeausschnitt zeigt ein Beispiel hierfür:

```csharp
Animation myAnimation = AnimationUtils.LoadAnimation(Resource.Animation.MyAnimation);
ImageView myImage = FindViewById<ImageView>(Resource.Id.imageView1);
myImage.StartAnimation(myAnimation);
```

Nun, wir haben es sich um einen grundlegenden Überblick über die Funktionsweise Ansicht Animationen, können die Eigenschaftenanimationen verschieben.


### <a name="property-animations"></a>-Animationen

Eigenschaft Animatoren sind eine neue API, die in Android 3.0 eingeführt wurde.
Sie bieten eine besser erweiterbare API, die verwendet werden kann, um eine Eigenschaft für ein beliebiges Objekt zu animieren.

Alle Eigenschaftenanimationen werden erstellt, indem Instanzen der [Animator](https://developer.xamarin.com/api/type/Android.Animation.Animator/) Unterklasse. Anwendungen verwenden diese Klasse nicht direkt, sie verwenden eine dessen Unterklassen:

-   [ValueAnimator](https://developer.xamarin.com/api/type/Android.Animation.ValueAnimator/) &ndash; diese Klasse ist die wichtigste Klasse in der gesamte Eigenschaftensatz Animations-API. Berechnet die Werte der Eigenschaften, die geändert werden müssen. Die `ViewAnimator` wird nicht direkt aktualisiert diese Werte; stattdessen löst Ereignisse, die verwendet werden können, um animierte Objekte zu aktualisieren.

-   [ObjectAnimator](https://developer.xamarin.com/api/type/Android.Animation.ObjectAnimator/) &ndash; diese Klasse ist eine Unterklasse von `ValueAnimator` . Es dient zum Vereinfachen von Animieren von Objekten durch das akzeptieren ein Zielobjekt und die zu aktualisierende Eigenschaft.

-   [AnimationSet](https://developer.xamarin.com/api/type/Android.Animation.AnimatorSet/) &ndash; diese Klasse ist zuständig für die Orchestrierung, in der Beziehung wie Animationen miteinander ausführen. Animationen können zwischen ihnen gleichzeitig, sequenziell oder mit einer angegebenen Verzögerung ausgeführt werden.


*Bewerter* sind spezielle Klassen, die von Animatoren verwendet werden, um den neuen Werten während einer Animation zu berechnen. Standardmäßig bietet Android die folgenden Auswerter:

-   [IntEvaluator](https://developer.xamarin.com/api/type/Android.Animation.IntEvaluator/) &ndash; Werte für die ganzzahligen Eigenschaften berechnet.

-   [FloatEvaluator](https://developer.xamarin.com/api/type/Android.Animation.FloatEvaluator/) &ndash; berechnet Werte für Eigenschaften von "float".

-   [ArgbEvaluator](https://developer.xamarin.com/api/type/Android.Animation.ArgbEvaluator/) &ndash; Werte für die Eigenschaften der Farbe berechnet.

Wenn die Eigenschaft, die animiert wird keine `float`, `int` oder Farbe, Anwendungen möglicherweise erstellen Sie eigene Auswertung durch die Implementierung der `ITypeEvaluator` Schnittstelle. (Implementieren von benutzerdefinierter Auswerter ist über den Rahmen dieses Themas hinaus.)

#### <a name="using-the-valueanimator"></a>Verwenden die ValueAnimator

Es gibt zwei Teilen alle Animationen: Berechnen der animierten Werte aus, und klicken Sie dann diese Werte zu Eigenschaften, die auf ein Objekt festgelegt. 
[ValueAnimator](https://developer.xamarin.com/api/type/Android.Animation.ValueAnimator/) nur die Werte berechnet, aber es werden keine Vorgänge für Objekte direkt. Stattdessen werden Objekte innerhalb eines ereignishandlers aktualisiert werden, die während der Ausführung der Animation aufgerufen wird. Dieser Entwurf ermöglicht mehrere Eigenschaften aus einer animierten Wert aktualisiert werden.

Sie erhalten eine Instanz des `ValueAnimator` durch Aufrufen einer der folgenden Factorymethoden zur Verfügung:

-  `ValueAnimator.OfInt`
-  `ValueAnimator.OfFloat`
-  `ValueAnimator.OfObject`

Ist diese erfolgen soll, die `ValueAnimator` Instanz müssen Dauer festgelegt, und klicken Sie dann es gestartet werden kann. Das folgende Beispiel zeigt, wie Sie einen Wert zwischen 0 und 1 innerhalb von 1000 Millisekunden zu animieren:

```csharp
ValueAnimator animator = ValueAnimator.OfInt(0, 100);
animator.SetDuration(1000);
animator.Start();
```

Selbst die obigen Codeausschnitt ist jedoch nicht sehr nützlich &ndash; der Animator ausgeführt, aber es ist kein Ziel für den aktualisierten Wert. Die `Animator` Klasse löst das Update-Ereignis, wenn er entscheidet sich, dass es notwendig, Listener über einen neuen Wert zu informieren. Anwendungen können es sich um einen Ereignishandler zum Reagieren auf dieses Ereignis, wie im folgenden Codeausschnitt dargestellt bereitstellen:

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

Nun, da haben wir einen Überblick über `ValueAnimator`, können Sie weitere Informationen über die `ObjectAnimator`.

#### <a name="using-the-objectanimator"></a>Verwenden die ObjectAnimator

[ObjectAnimator](https://developer.xamarin.com/api/type/Android.Animation.ObjectAnimator/) ist eine Unterklasse von `ViewAnimator` , die die zeitliche Steuerung-Engine und der Wert der Berechnung des kombiniert die `ValueAnimator` mit der Logik erforderlich, um Ereignishandler zu verknüpfen. Die `ValueAnimator` Anwendungen explizit Verknüpfen eines ereignishandlers muss &ndash; `ObjectAnimator` übernimmt diesen Schritt für uns.

Die API für `ObjectAnimator` ist sehr ähnlich, mit der API für `ViewAnimator`, erfordert jedoch, dass Sie das Objekt und den Namen des zu aktualisierenden Eigenschaft angeben. Das folgende Beispiel zeigt ein Beispiel der Verwendung von `ObjectAnimator`:

```csharp
MyCustomObject myObj = new MyCustomObject();
myObj.SomeIntegerValue = -1;

ObjectAnimator animator = ObjectAnimator.OfFloat(myObj, "SomeIntegerValue", 0, 100);
animator.SetDuration(1000);
animator.Start();
```

Wie Sie aus dem vorherigen Codeausschnitt sehen `ObjectAnimator` verringern und vereinfachen den Code, der erforderlich ist, ein Objekt animieren können.


### <a name="drawable-animations"></a>Drawable-Animationen

Der endgültige Animations-API ist der Drawable Animations-API. Drawable-Animationen Laden Sie eine Reihe von zeichenbare Ressourcen eine nach dem anderen und angezeigt werden, sequenziell eine Flip-It-Cartoon ähnelt.

Zeichenbare Ressourcen werden definiert, in eine XML-Datei mit einer `<animation-list>` -Element als Stammelement und eine Reihe von `<item>` Elemente, die jeden Frame der Animation definieren. Diese XML-Datei befindet sich in der `/Resource/drawable` Ordner der Anwendung. Das folgende XML ist ein Beispiel für eine drawable-Animation:

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

In dieser Animation wird durch sechs Frames ausgeführt. Die `android:duration` Attribut deklariert, wie lange jeder Frame angezeigt werden. Der nächste Codeausschnitt zeigt ein Beispiel für eine Drawable-Animation erstellen, und starten es, klickt der Benutzer eine Schaltfläche auf dem Bildschirm:

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

An diesem Punkt haben wir die Grundlagen der Animation APIs zur Verfügung, in einer Android-Anwendung behandelt.


## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde sehr viele neue Konzepte eingeführt und APIs helfen, einige Grafiken zu einer Android-Anwendung hinzufügen. Zunächst werden erläutert die verschiedenen 2D-Grafiken-APIs und veranschaulicht, wie Android-Anwendungen direkt auf dem Bildschirm mit einem Canvasobjekt gezeichnet werden können. Wir haben auch einige Alternative Techniken, mit die Grafiken, die deklarativ mithilfe von XML-Dateien erstellt werden können. Klicken Sie dann passiert wir besprechen der alten und neuen APIs zum Erstellen von Animationen in Android.



## <a name="related-links"></a>Verwandte Links

- [Animation-Demo (Beispiel)](https://developer.xamarin.com/samples/monodroid/AnimationDemo)
- [Animation und Grafiken](https://developer.android.com/guide/topics/graphics/index.html)
- [Verwendung von Animationen auf Ihre mobilen Apps zum Leben zu erwecken.](http://youtu.be/ikSk_ILg3d0)
- [AnimationDrawable](https://developer.xamarin.com/api/type/Android.Graphics.Drawables.AnimationDrawable/)
- [Canvas](https://developer.xamarin.com/api/type/Android.Graphics.Canvas/)
- [Objekt Animator](https://developer.xamarin.com/api/type/Android.Animation.ObjectAnimator/)
- [Wert Animator](https://developer.xamarin.com/api/type/Android.Animation.ValueAnimator/)
