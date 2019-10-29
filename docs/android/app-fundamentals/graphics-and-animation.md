---
title: Grafiken und Animationen
description: Android bietet ein sehr umfangreiches und vielfältiges Framework für die Unterstützung von 2D-Grafiken und-Animationen. In diesem Thema werden diese Frameworks vorgestellt und erläutert, wie benutzerdefinierte Grafiken und Animationen für die Verwendung in einer xamarin. Android-Anwendung erstellt werden.
ms.prod: xamarin
ms.assetid: 80086318-6FE4-4711-9A71-5C8F8C28C754
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 1781503d214b959d31223cbe8f55fd6afa0fef44
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73019292"
---
# <a name="android-graphics-and-animation"></a>Android-Grafiken und-Animationen

_Android bietet ein sehr umfangreiches und vielfältiges Framework für die Unterstützung von 2D-Grafiken und-Animationen. In diesem Thema werden diese Frameworks vorgestellt und erläutert, wie benutzerdefinierte Grafiken und Animationen für die Verwendung in einer xamarin. Android-Anwendung erstellt werden._

## <a name="overview"></a>Übersicht

Trotz der Ausführung auf Geräten, die in der Regel nur eine begrenzte Leistung aufweisen, verfügen die Anwendungen mit der höchsten Bewertung häufig über eine ausgereifte Benutzererfahrung (UX), die mit qualitativ hochwertigen Grafiken und Animationen ausgestattet ist, die ein intuitives, reaktionsfähiges dynamisches Verhalten bieten. Wenn mobile Anwendungen immer komplexer werden, haben die Benutzer bereits mehr und mehr von Anwendungen erwartet.

Glücklicherweise bieten moderne Mobile Plattformen sehr leistungsfähige Frameworks zum Erstellen von ausgereiften Animationen und benutzerdefinierten Grafiken und sorgen gleichzeitig für eine einfache Verwendung. Dadurch können Entwickler mit sehr geringem Aufwand eine umfangreiche Interaktivität hinzufügen.

UI-API-Frameworks in Android können ungefähr in zwei Kategorien unterteilt werden: Grafiken und Animationen.

Grafiken werden weiter in verschiedene Ansätze zur Durchführung von 2D-und 3D-Grafiken aufgeteilt. 3D-Grafiken sind über eine Reihe integrierter Frameworks verfügbar, wie z. b. OpenGL es (eine Mobile spezifische Version von OpenGL) und Frameworks von Drittanbietern (ein plattformübergreifendes Toolkit, das mit dem XNA-Toolkit kompatibel ist). Obwohl sich 3D-Grafiken nicht im Rahmen dieses Artikels befinden, werden die integrierten 2D-zeichnungstechniken untersucht.

Android bietet zwei verschiedene APIs zum Erstellen von 2D-Grafiken. Eine ist eine allgemeine deklarative Herangehensweise und die andere eine programmatische Low-Level-API:

- **Drawable-Ressourcen** &ndash; diese werden verwendet, um benutzerdefinierte Grafiken entweder Programm gesteuert oder (in der Regel) durch Einbetten von Zeichnungs Anweisungen in XML-Dateien zu erstellen. Drawable-Ressourcen werden in der Regel als XML-Dateien definiert, die Anweisungen oder Aktionen für Android zum Rendering einer 2D-Grafik enthalten. 

- **Canvas** &ndash; Dies ist eine API auf niedriger Ebene, die das Zeichnen direkt auf eine zugrunde liegende Bitmap einschließt. Er bietet eine präzise Kontrolle über das, was angezeigt wird. 

Zusätzlich zu diesen 2D-Grafiktechniken bietet Android auch mehrere verschiedene Möglichkeiten zum Erstellen von Animationen:

- **Drawable-Animationen** &ndash; Android unterstützt auch Frame-by-Frame-Animationen, die als *drawable Animation*bezeichnet werden. Dies ist die einfachste Animations-API. Android lädt und zeigt drawable-Ressourcen nacheinander an (ähnlich wie eine Zeichenfolge).

- **Anzeigen von Animationen** &ndash; *Anzeigen von Animationen* sind die ursprünglichen Animations-APIs in Android und sind in allen Versionen von Android verfügbar. Diese API ist darauf beschränkt, dass Sie nur mit Ansichts Objekten funktioniert und nur einfache Transformationen für diese Ansichten ausführen kann.
    Ansichts Animationen werden in der Regel in XML-Dateien im Ordner "`/Resources/anim`" definiert.

- **Eigenschafts Animationen** &ndash; Android 3,0 haben einen neuen Satz von Animations-APIs eingeführt, die als *Eigenschafts Animationen*bezeichnet werden. Diese neue API hat ein erweiterbares und flexibles System eingeführt, das zum Animieren der Eigenschaften eines beliebigen Objekts verwendet werden kann, nicht nur zum Anzeigen von Objekten. Dank dieser Flexibilität können Animationen in unterschiedlichen Klassen gekapselt werden, um die Code Freigabe zu vereinfachen.

Die Anzeige von Animationen eignet sich besser für Anwendungen, die ältere Pre-Android 3,0-APIs (API-Ebene 11) unterstützen müssen. Andernfalls sollten Anwendungen die neueren Eigenschaften der Animations-API aus den oben erwähnten Gründen verwenden.

Alle diese Frameworks sind geeignete Optionen, aber wenn möglich, sollten Sie den Eigenschafts Animationen bevorzugen, da es sich um eine flexiblere API handelt, mit der Sie arbeiten können. Eigenschafts Animationen ermöglichen das Kapseln von Animations Logik in unterschiedlichen Klassen, die die Code Freigabe vereinfachen und die Code Verwaltung vereinfachen.

## <a name="accessibility"></a>Zugriff

Grafiken und Animationen helfen Ihnen, die Verwendung von Android-Apps zu optimieren. Es ist jedoch wichtig zu beachten, dass einige Interaktionen über Screenreader, Alternative Eingabegeräte oder mit unterstütztem Zoom erfolgen.
Außerdem können einige Interaktionen ohne Audiofunktionen auftreten.

Apps sind in diesen Situationen besser verwendbar, wenn Sie mit Barrierefreiheit entworfen wurden: Bereitstellung von Hinweisen und Navigationsunterstützung in der Benutzeroberfläche und sicherstellen, dass Textinhalte oder Beschreibungen für Bildelemente der Benutzeroberfläche vorhanden sind.

Weitere Informationen zur Verwendung von Android-Barrierefreiheits-APIs finden Sie im [Handbuch zur Barrierefreiheit von Google](https://developer.android.com/guide/topics/ui/accessibility/) .

## <a name="2d-graphics"></a>2D-Grafiken

Drawable-Ressourcen sind eine beliebte Technik in Android-Anwendungen. Wie bei anderen Ressourcen sind drawable-Ressourcen deklarativ &ndash; Sie in XML-Dateien definiert sind. Dieser Ansatz ermöglicht eine saubere Trennung von Code von Ressourcen. Dies kann die Entwicklung und Wartung vereinfachen, da es nicht erforderlich ist, Code zu ändern, um die Grafiken in einer Android-Anwendung zu aktualisieren oder zu ändern. Obwohl drawable-Ressourcen für viele einfache und gängige grafische Anforderungen nützlich sind, fehlen Ihnen die Leistungsfähigkeit und Kontrolle der Canvas-API.

Die andere Technik, die das [Canvas](xref:Android.Graphics.Canvas) -Objekt verwendet, ähnelt anderen herkömmlichen API-Frameworks, wie z. b. System. Drawing oder der Kern Zeichnung von IOS. Die Verwendung des Canvas-Objekts bietet die größte Kontrolle über die Erstellung von 2D-Grafiken. Dies ist für Situationen geeignet, in denen eine drawable-Ressource nicht funktioniert oder die Arbeit schwierig ist. Beispielsweise kann es erforderlich sein, ein benutzerdefiniertes Schieberegler-Steuerelement zu zeichnen, dessen Darstellung sich auf der Grundlage der auf dem Wert des Schiebereglers bezogenen Berechnungen ändert.

Betrachten wir zuerst drawable-Ressourcen. Sie sind einfacher und behandeln die gängigsten benutzerdefinierten Zeichnungs Fälle.

### <a name="drawable-resources"></a>Drawable-Ressourcen

Drawable-Ressourcen werden in einer XML-Datei im Verzeichnis `/Resources/drawable`definiert. Im Gegensatz zum Einbetten von PNG oder JPEG ist es nicht notwendig, Dichte spezifische Versionen von drawable-Ressourcen bereitzustellen.
Zur Laufzeit lädt eine Android-Anwendung diese Ressourcen und verwendet die in diesen XML-Dateien enthaltenen Anweisungen zum Erstellen von 2D-Grafiken.
Android definiert mehrere verschiedene Typen von drawable-Ressourcen:

- [Shapedrawable](https://developer.android.com/guide/topics/resources/drawable-resource.html#Shape) &ndash; dieses Objekt ist ein drawable-Objekt, das eine primitive geometrische Form zeichnet und eine begrenzte Menge grafischer Effekte auf diese Form anwendet. Sie sind sehr nützlich für Dinge wie das Anpassen von Schaltflächen oder das Festlegen des Hintergrunds von Text Ansichten. Ein Beispiel für die Verwendung einer `ShapeDrawable` finden Sie weiter unten in diesem Artikel.

- [Statelistdrawable](https://developer.android.com/guide/topics/resources/drawable-resource.html#StateList) &ndash; dies eine drawable-Ressource ist, die die Darstellung basierend auf dem Zustand eines Widgets/Steuer Elements ändert. Beispielsweise kann eine Schaltfläche die Darstellung ändern, je nachdem, ob Sie gedrückt wird.

- [Layerdrawable](https://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList) &ndash; diese drawable-Ressource, die mehrere andere drawables eines anderen Stapels stapelt. Ein Beispiel für ein *layerdrawable* -Element ist im folgenden Screenshot zu sehen:

    ![Layerdrawable-Beispiel](graphics-and-animation-images/image1.png)

- [Transitiondrawable](https://developer.android.com/guide/topics/resources/drawable-resource.html#Transition) &ndash; dies ein *layerdrawable* -Element, jedoch mit einem Unterschied. Ein *transitiondrawable* -Element kann eine Ebene animieren, die über dem oberen Rand angezeigt wird.

- [Levellistdrawable](https://developer.android.com/guide/topics/resources/drawable-resource.html#LevelList) &ndash; Dies ist sehr vergleichbar mit einem " *Status* ", da es ein Bild auf der Grundlage bestimmter Bedingungen anzeigt. Anders als bei einer *Status-drawable*zeigt das *levellistdrawable* -Element jedoch ein Bild an, das auf einem ganzzahligen Wert basiert. Ein Beispiel für ein *levellistdrawable* -Element wäre, die Stärke eines WiFi-Signals anzuzeigen. Wenn die Stärke des WLAN-Signals geändert wird, ändert sich der angezeigte drawable entsprechend.

- [Scaledrawable](https://developer.android.com/guide/topics/resources/drawable-resource.html#Scale)/[clipdrawable](https://developer.android.com/guide/topics/resources/drawable-resource.html#Clip) &ndash; wie der Name schon sagt, bieten diese drawables sowohl Skalierungs-als auch clippingfunktionen. Der *scaledrawable* skaliert einen anderen drawable-Wert, während der *clipdrawable* einen anderen drawable Ausschneide.

- [Insetdrawable](https://developer.android.com/guide/topics/resources/drawable-resource.html#Inset) &ndash; diese drawable wendet insets auf den Seiten einer anderen drawable-Ressource an. Sie wird verwendet, wenn eine Sicht einen Hintergrund benötigt, der kleiner als die tatsächlichen Begrenzungen der Sicht ist.

- XML [bitmapdrawable](https://developer.android.com/guide/topics/resources/drawable-resource.html#Bitmap) &ndash; diese Datei besteht aus einer Reihe von Anweisungen in XML, die auf einer tatsächlichen Bitmap ausgeführt werden sollen. Zu den Aktionen, die Android ausführen kann, gehören tiult, Dithering und Antialiasing. Eine der häufigsten Verwendungsmöglichkeiten besteht darin, eine Bitmap auf dem Hintergrund eines Layouts zu Kacheln.

#### <a name="drawable-example"></a>Beispiel für drawable

Sehen wir uns ein kurzes Beispiel für das Erstellen einer 2D-Grafik mit einem `ShapeDrawable`an. Eine `ShapeDrawable` kann eine der vier grundlegenden Formen definieren: Rechteck, oval, Linie und Ring. Es ist auch möglich, grundlegende Effekte wie Farbverlauf, Farbe und Größe anzuwenden. Der folgende XML-Code ist eine `ShapeDrawable`, die im *animationsdemo* -Begleitprojekt (in der Datei `Resources/drawable/shape_rounded_blue_rect.xml`) enthalten sein können.
Es definiert ein Rechteck mit einem lila Farbverlaufs Hintergrund und abgerundeten Ecken:

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

Wir können diese drawable-Ressource deklarativ in einem Layout oder einem anderen drawable-Element referenzieren, wie im folgenden XML-Code dargestellt:

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

Drawable-Ressourcen können auch Programm gesteuert angewendet werden. Der folgende Code Ausschnitt zeigt, wie der Hintergrund einer TextView Programm gesteuert festgelegt wird:

```csharp
TextView tv = FindViewById<TextView>(Resource.Id.shapeDrawableTextView);
tv.SetBackgroundResource(Resource.Drawable.shape_rounded_blue_rect);
```

Um zu sehen, wie dies aussehen würde, führen Sie das *animationsdemo* -Projekt aus, und wählen Sie das Element Form drawable aus dem Hauptmenü aus. Es sollte etwas angezeigt werden, das dem folgenden Screenshot ähnelt:

![TextView mit einem benutzerdefinierten Hintergrund, drawable mit einem Farbverlauf und abgerundeten Ecken](graphics-and-animation-images/image1.png)

Weitere Informationen zu den XML-Elementen und zur Syntax von drawable-Ressourcen finden Sie in [der Google-Dokumentation](https://developer.android.com/guide/topics/resources/drawable-resource.html#Shape).

### <a name="using-the-canvas-drawing-api"></a>Verwenden der Canvas-Zeichnungs-API

Drawables sind leistungsstark, haben aber ihre Einschränkungen. Bestimmte Dinge sind entweder nicht möglich oder sehr komplex (z. b. das Anwenden eines Filters auf ein Bild, das von einer Kamera auf dem Gerät übernommen wurde). Es wäre sehr schwierig, die Red-Eye-Reduzierung mithilfe einer drawable-Ressource anzuwenden.
Stattdessen kann eine Anwendung mit der Canvas-API eine sehr differenzierte Steuerung aufweisen, um Farben in einem bestimmten Teil des Bilds selektiv zu ändern.

Eine Klasse, die häufig mit der Canvas verwendet wird, ist die [Paint](xref:Android.Graphics.Paint) -Klasse. Diese Klasse enthält Farb-und Stil Informationen zum Zeichnen von. Es wird verwendet, um Dinge wie Farbe und Transparenz bereitzustellen.

Die Canvas-API verwendet das *Modell des Malers* zum Zeichnen von 2D-Grafiken.
Vorgänge werden aufeinander aufeinander folgende Ebenen angewendet. Jeder Vorgang deckt einen Bereich der zugrunde liegenden Bitmap ab. Wenn sich der Bereich mit einem zuvor gezeichneten Bereich überlappt, wird die alte Farbe teilweise oder vollständig verdeckt. Dies ist dieselbe Art und Weise, wie viele andere Zeichnungs-APIs, wie z. b. System. Drawing und IOS-Kern Grafiken, funktionieren.

Es gibt zwei Möglichkeiten, ein `Canvas` Objekt zu erhalten. Die erste Möglichkeit besteht darin, ein [Bitmap](xref:Android.Graphics.Bitmap) -Objekt zu definieren und dann ein `Canvas` Objekt mit diesem Objekt zu instanziieren. Der folgende Code Ausschnitt erstellt z. b. eine neue Canvas mit einer zugrunde liegenden Bitmap:

```csharp
Bitmap bitmap = Bitmap.CreateBitmap(100, 100, Bitmap.Config.Argb8888);
Canvas canvas = new Canvas(b);
```

Die andere Möglichkeit zum Abrufen eines `Canvas` Objekts ist die [OnDraw](xref:Android.Views.View.OnDraw*) -Rückruf Methode, die die [View](xref:Android.Views.View) -Basisklasse bereitstellt. Android ruft diese Methode auf, wenn Sie entscheidet, dass sich eine Ansicht selbst zeichnen muss und ein `Canvas` Objekt übergibt, damit die Ansicht verwendet werden kann.

Die Canvas-Klasse macht Methoden verfügbar, um die Draw-Anweisungen Programm gesteuert bereitzustellen. Beispiel:

- [Canvas. drawpaint](xref:Android.Graphics.Canvas.DrawPaint*) &ndash; füllt die Bitmap der gesamten Canvas mit der angegebenen Farbe aus.

- [Canvas. DrawPath](xref:Android.Graphics.Canvas.DrawPath*) &ndash; zeichnet die angegebene geometrische Form mit der angegebenen Farbe.

- [Canvas. DrawText](xref:Android.Graphics.Canvas.DrawText*) &ndash; den Text auf der Canvas mit der angegebenen Farbe zeichnet. Der Text wird an der Position `x,y` gezeichnet.

#### <a name="drawing-with-the-canvas-api"></a>Zeichnen mit der Canvas-API

Sehen wir uns ein Beispiel für die Canvas-API in Aktion an. Der folgende Code Ausschnitt zeigt, wie Sie eine Ansicht zeichnen:

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

Dieser Code oben erstellt zunächst eine rote und ein grünes Farb Objekt. Er füllt den Inhalt der Canvas mit rot und weist dann den Zeichenbereich an, ein grünes Rechteck zu zeichnen, das 25% der Breite des Zeichen Bereichs entspricht. Ein Beispiel hierfür finden Sie in `AnimationsDemo` Projekt, das im Quellcode für diesen Artikel enthalten ist. Wenn Sie die Anwendung starten und das Zeichnungs Element im Hauptmenü auswählen, sollte ein Bildschirm ähnlich dem folgenden angezeigt werden:

![Bildschirm mit roten Paint-und grünen Paint-Objekten](graphics-and-animation-images/image3.png)

## <a name="animation"></a>Animation

Benutzer wie die Anwendungen, die in Ihre Anwendungen verschoben werden. Animationen sind eine gute Möglichkeit, um die Benutzer Leistung einer Anwendung zu verbessern und Sie zu unterstützen. Die besten Animationen sind diejenigen, die Benutzern nicht angezeigt werden, da Sie sich in natürlicher Form fühlen. Android bietet die folgenden drei APIs für Animationen:

- **Animation anzeigen** &ndash; Dies ist die ursprüngliche API. Diese Animationen sind an eine bestimmte Ansicht gebunden und können einfache Transformationen für den Inhalt der Ansicht ausführen. Aus Gründen der Einfachheit ist diese API auch für Dinge wie Alpha Animationen, Rotationen usw. nützlich.

- **Eigenschafts Animation** &ndash; Eigenschafts Animationen wurden in Android 3,0 eingeführt. Sie ermöglichen es einer Anwendung, fast alles zu animieren. Eigenschafts Animationen können verwendet werden, um jede beliebige Eigenschaft eines beliebigen Objekts zu ändern, auch wenn dieses Objekt nicht auf dem Bildschirm sichtbar ist.

- **Drawable Animation** &ndash; dies eine spezielle drawable-Ressource, mit der ein sehr einfacher Animationseffekt auf Layouts angewendet wird.

Im Allgemeinen ist die Eigenschafts Animation das bevorzugte System, das verwendet werden kann, da Sie flexibler ist und mehr Features bietet.

### <a name="view-animations"></a>Animationen anzeigen

Ansichts Animationen sind auf Sichten beschränkt und können nur Animationen für Werte wie Start-und Endpunkte, Größe, Drehung und Transparenz ausführen.
Diese Animations Typen werden in der Regel als *Tween-Animationen*bezeichnet. Die Anzeige von Animationen kann auf zwei Arten definiert werden, &ndash; Programm gesteuert im Code oder mithilfe von XML-Dateien. XML-Dateien sind die bevorzugte Methode zum Deklarieren von Ansichts Animationen, da Sie besser lesbar und einfacher zu verwalten sind.

Die XML-Animationsdateien werden im `/Resources/anim`-Verzeichnis eines xamarin. Android-Projekts gespeichert. Diese Datei muss eines der folgenden Elemente als Stamm Element aufweisen:

- `alpha` &ndash; eine Animation zum Einblenden oder ausblenden.

- `rotate` &ndash; eine Rotations Animation.

- `scale` &ndash; eine Animation zum Ändern der Größe.

- `translate` &ndash; eine horizontale und/oder vertikale Bewegung.

- `set` &ndash; einen Container an, der ein oder mehrere der anderen Animations Elemente enthalten kann.

Standardmäßig werden alle Animationen in einer XML-Datei gleichzeitig angewendet. Um Animationen sequenziell zu erstellen, legen Sie das `android:startOffset`-Attribut für eines der oben definierten Elemente fest.

Es ist möglich, die Änderungs Rate in einer Animation mithilfe eines *interpolators*zu beeinflussen. Ein interpolators ermöglicht die Beschleunigung, Wiederholung oder Verlangsamung von Animationseffekten. Das Android-Framework bietet standardmäßig mehrere Interpolatoren, z. b. (aber nicht beschränkt auf):

- `AccelerateInterpolator` / `DecelerateInterpolator` &ndash; diese Interpolatoren die Änderungs Rate in einer Animation erhöhen oder verringern.

- `BounceInterpolator` &ndash; die Änderungs springt am Ende.

- `LinearInterpolator` &ndash; die Änderungs Rate konstant ist.

Der folgende XML-Code zeigt ein Beispiel für eine Animations Datei, die einige dieser Elemente kombiniert:

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

Diese Animation führt alle Animationen gleichzeitig aus. Die erste Skalierungs Animation dehnt das Bild horizontal und verkleinert es vertikal. Anschließend wird das Bild gleichzeitig um 45 Grad gegen den Uhrzeigersinn gedreht und verkleinert, was vom Bildschirm verschwindet.

Die Animation kann Programm gesteuert auf eine Ansicht angewendet werden, indem die Animation vergrößert und dann auf eine Ansicht angewendet wird. Android stellt die Hilfsklasse `Android.Views.Animations.AnimationUtils` bereit, mit der eine Animations Ressource vergrößert und eine Instanz von `Android.Views.Animations.Animation`zurückgegeben werden kann. Dieses Objekt wird auf eine Ansicht angewendet, indem `StartAnimation` aufgerufen und das `Animation`-Objekt übergeben wird. Der folgende Code Ausschnitt zeigt ein Beispiel für Folgendes:

```csharp
Animation myAnimation = AnimationUtils.LoadAnimation(Resource.Animation.MyAnimation);
ImageView myImage = FindViewById<ImageView>(Resource.Id.imageView1);
myImage.StartAnimation(myAnimation);
```

Nachdem wir nun über ein grundlegendes Verständnis der Funktionsweise von Ansichts Animationen verfügen, können Sie zu Eigenschafts Animationen wechseln.

### <a name="property-animations"></a>Eigenschafts Animationen

Eigenschaftanimatoren sind eine neue API, die in Android 3,0 eingeführt wurde.
Sie stellen eine erweiterbare API bereit, die verwendet werden kann, um jede beliebige Eigenschaft eines beliebigen Objekts zu animieren.

Alle Eigenschafts Animationen werden von Instanzen der [Animator](xref:Android.Animation.Animator) -Unterklasse erstellt. Anwendungen verwenden diese Klasse nicht direkt, sondern verwenden stattdessen eine ihrer Unterklassen:

- [Valueanimator](xref:Android.Animation.ValueAnimator) &ndash; diese Klasse ist die wichtigste Klasse in der gesamten Eigenschafts Animations-API. Die Werte von Eigenschaften, die geändert werden müssen, werden berechnet. Diese Werte werden vom `ViewAnimator` nicht direkt aktualisiert. Stattdessen werden Ereignisse ausgelöst, die zum Aktualisieren animierter Objekte verwendet werden können.

- [Objectanimator](xref:Android.Animation.ObjectAnimator) &ndash; diese Klasse ist eine Unterklasse von `ValueAnimator`. Es soll den Prozess der Animation von Objekten vereinfachen, indem ein Zielobjekt und eine zu Aktualisier Ende Eigenschaft akzeptiert werden.

- [Animationset](xref:Android.Animation.AnimatorSet) &ndash; diese Klasse ist dafür verantwortlich, zu orchestrieren, wie Animationen in Beziehung zueinander ausgeführt werden. Animationen können gleichzeitig, sequenziell oder mit einer festgelegten Verzögerung zwischen Ihnen ausgeführt werden.

*Evaluatoren* sind spezielle Klassen, die von Animatoren verwendet werden, um die neuen Werte während einer Animation zu berechnen. Android bietet standardmäßig die folgenden Evaluatoren:

- [Intevaluator](xref:Android.Animation.IntEvaluator) &ndash; berechnet Werte für ganzzahlige Eigenschaften.

- [Floatevaluator](xref:Android.Animation.FloatEvaluator) &ndash; berechnet Werte für float-Eigenschaften.

- [Argbevaluator](xref:Android.Animation.ArgbEvaluator) &ndash; berechnet Werte für Farbeigenschaften.

Wenn es sich bei der zu animierenden Eigenschaft nicht um einen `float`, `int` oder eine Farbe handelt, können Anwendungen durch Implementieren der `ITypeEvaluator`-Schnittstelle ihre eigene Auswertung erstellen. (Die Implementierung benutzerdefinierter Evaluatoren geht über den Rahmen dieses Themas hinaus.)

#### <a name="using-the-valueanimator"></a>Verwenden von valueanimator

Jede Animation besteht aus zwei Teilen: der Berechnung animierter Werte und dem anschließenden Festlegen dieser Werte für die Eigenschaften eines Objekts. 
[Valueanimator](xref:Android.Animation.ValueAnimator) berechnet nur die Werte, wird jedoch nicht direkt für Objekte ausgeführt. Stattdessen werden Objekte innerhalb von Ereignis Handlern aktualisiert, die während der Lebensdauer der Animation aufgerufen werden. Dieser Entwurf ermöglicht es, mehrere Eigenschaften von einem animierten Wert aus zu aktualisieren.

Sie erhalten eine Instanz von `ValueAnimator`, indem Sie eine der folgenden Factorymethoden aufrufen:

- `ValueAnimator.OfInt`
- `ValueAnimator.OfFloat`
- `ValueAnimator.OfObject`

Sobald dies abgeschlossen ist, muss die Dauer der `ValueAnimator` Instanz festgelegt sein, und Sie kann dann gestartet werden. Im folgenden Beispiel wird gezeigt, wie Sie einen Wert zwischen 0 und 1 über dem Bereich von 1000 Millisekunden animieren:

```csharp
ValueAnimator animator = ValueAnimator.OfInt(0, 100);
animator.SetDuration(1000);
animator.Start();
```

Der obige Code Ausschnitt ist jedoch nicht sehr nützlich, &ndash; der Animator ausgeführt wird, aber kein Ziel für den aktualisierten Wert ist. Die `Animator`-Klasse gibt das Update-Ereignis aus, wenn Sie entscheidet, dass Sie Listener über einen neuen Wert informieren müssen. Anwendungen können einen Ereignishandler zur Reaktion auf dieses Ereignis bereitstellen, wie im folgenden Code Ausschnitt gezeigt:

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

Nachdem wir uns mit `ValueAnimator`vertraut sind, erfahren Sie hier mehr über die `ObjectAnimator`.

#### <a name="using-the-objectanimator"></a>Verwenden von objectanimator

[Objectanimator](xref:Android.Animation.ObjectAnimator) ist eine Unterklasse von `ViewAnimator`, die die Zeit Steuerungs-Engine und die Wert Berechnung der `ValueAnimator` mit der zum Verknüpfen von Ereignis Handlern erforderlichen Logik kombiniert. Der `ValueAnimator` erfordert, dass Anwendungen einen Ereignishandler explizit einbinden, &ndash; `ObjectAnimator` für uns diesen Schritt übernimmt.

Die API für `ObjectAnimator` ähnelt der API für `ViewAnimator`, erfordert jedoch, dass Sie das-Objekt und den Namen der zu aktualisierenden Eigenschaft bereitstellen. Das folgende Beispiel zeigt ein Beispiel für die Verwendung von `ObjectAnimator`:

```csharp
MyCustomObject myObj = new MyCustomObject();
myObj.SomeIntegerValue = -1;

ObjectAnimator animator = ObjectAnimator.OfFloat(myObj, "SomeIntegerValue", 0, 100);
animator.SetDuration(1000);
animator.Start();
```

Wie Sie im vorherigen Code Ausschnitt sehen können, können `ObjectAnimator` den Code verringern und vereinfachen, der zum Animieren eines Objekts erforderlich ist.

### <a name="drawable-animations"></a>Drawable-Animationen

Die abschließende Animations-API ist die drawable Animation API. Drawable-Animationen laden eine Reihe von drawable-Ressourcen nacheinander und zeigen Sie nacheinander an, ähnlich wie bei einer Flip-IT-Karikatur.

Drawable-Ressourcen werden in einer XML-Datei mit einem `<animation-list>`-Element als Stamm Element und einer Reihe von `<item>` Elementen definiert, die jeden Frame in der Animation definieren. Diese XML-Datei wird im `/Resource/drawable` Ordner der Anwendung gespeichert. Der folgende XML-Code ist ein Beispiel für eine drawable-Animation:

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

Diese Animation wird sechs Frames überlaufen. Das `android:duration`-Attribut deklariert, wie lange die einzelnen Frames angezeigt werden. Der nächste Code Ausschnitt zeigt ein Beispiel für das Erstellen einer drawable-Animation und das starten, wenn der Benutzer auf eine Schaltfläche auf dem Bildschirm klickt:

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

An dieser Stelle haben wir die Grundlagen der in einer Android-Anwendung verfügbaren Animations-APIs behandelt.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden viele neue Konzepte und APIs vorgestellt, mit denen Sie einer Android-Anwendung einige Grafiken hinzufügen können. Zuerst wurden die verschiedenen 2D-Grafik-APIs erläutert und veranschaulicht, wie Android Anwendungen mithilfe eines Canvas-Objekts direkt auf den Bildschirm zeichnen kann. Wir haben auch einige alternative Techniken gesehen, mit denen Grafiken deklarativ mithilfe von XML-Dateien erstellt werden können. Anschließend haben wir uns mit den alten und neuen APIs zum Erstellen von Animationen in Android vertraut gemacht.

## <a name="related-links"></a>Verwandte Links

- [Animations Demo (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/animationdemo)
- [Animation und Grafiken](https://developer.android.com/guide/topics/graphics/index.html)
- [Verwenden von Animationen zum bringen ihrer Mobile Apps](https://youtu.be/ikSk_ILg3d0)
- [Animationdrawable](xref:Android.Graphics.Drawables.AnimationDrawable)
- [Canvas](xref:Android.Graphics.Canvas)
- [Objektanimator](xref:Android.Animation.ObjectAnimator)
- [Wert Animator](xref:Android.Animation.ValueAnimator)
