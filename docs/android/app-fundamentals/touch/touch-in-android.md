---
title: Tippen Sie in Android
ms.topic: article
ms.prod: xamarin
ms.assetid: 405A1FA0-4EFA-4AEB-B672-F36307B9CF16
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 1d9cf345aa971c40f4132cc7970ed1244640da14
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="touch-in-android"></a>Tippen Sie in Android

Ähnlich wie iOS, Android erstellt ein Objekt, das Daten zum physischen Benutzerinteraktion mit dem Bildschirm enthält &ndash; ein `Android.View.MotionEvent` Objekt. Dieses Objekt enthält Daten, z. B. welche Aktion ausgeführt wird, wo die Fingereingabe dauerte platzieren, wie viel Druck wurde angewendet, usw. Ein `MotionEvent` Objekt gliedert die Verschiebung in den folgenden Werten:

-  Eine Aktionscode, der den Typ der Bewegung, z. B. die anfängliche Fingereingabe, die Fingereingabe auf dem Bildschirm oder die Endung Touch verschieben beschreibt.

-  Ein Satz von Achsenwerten, die die Position des beschreiben die `MotionEvent` und andere Eigenschaften verschieben, z. B., in denen die Fingereingabe stattfindet, wenn einem stattfand und wie viel Druck verwendet wurde.
   Die Achsenwerte variieren, je nachdem das Gerät möglicherweise alle Achsenwerten nicht in die obigen Liste beschrieben werden.


Die `MotionEvent` Objekt wird an einer geeigneten Methode in einer Anwendung übergeben werden. Es gibt drei Möglichkeiten für eine Anwendung Xamarin.Android Reaktion auf ein Ereignis Touch:

-  *Weisen Sie einen Ereignishandler an `View.Touch`*  – der `Android.Views.View` -Klasse verfügt über eine `EventHandler<View.TouchEventArgs>` können zuweisen, welche Anwendungen einen Ereignishandler hinzu. Dies ist das normale Verhalten für .NET.

-  *Implementieren von `View.IOnTouchListener`*  -Instanzen dieser Schnittstelle können ein Objekt in der Ansicht zugewiesen werden. `SetOnListener` Methode. Dies entspricht funktional zum Zuweisen von einen Ereignishandler an das `View.Touch` Ereignis. Einige häufige oder gemeinsame Logik, die möglicherweise viele verschiedene Ansichten berührt werden vorhanden ist, werden effizienter, erstellen Sie eine Klasse und implementieren Sie diese Methode als zum Zuweisen einer Ansicht jeweils eines eigenen ereignishandlers.

-  *Überschreiben Sie `View.OnTouchEvent`*  -alle Sichten in der Android-Unterklasse `Android.Views.View`. Android wird aufgerufen, wenn eine Sicht verweist, die `OnTouchEvent` und übergibt ihn dann ein `MotionEvent` -Objekt als Parameter.


> [!NOTE]
> Nicht alle Android-Geräte unterstützen Touchscreens. 

Hinzufügen der folgenden Tags in der Manifestdatei bewirkt, dass Google Play nur Anzeige Ihrer app für diese Geräte, die mit Toucheingabe aktiviert werden:

```xml
<uses-configuration android:reqTouchScreen="finger" />
```

## <a name="gestures"></a>Gesten

Eine Geste ist eine Form "Hand gezeichnetes" auf dem Touchscreen. Eine Geste kann eine oder mehrere Striche, jeden Strich bestehend aus einer Sequenz von Punkten, die von einem anderen Zeitpunkt der Kontakt mit dem Bildschirm erstellt haben. Android kann viele verschiedene Objekttypen Gesten, aus einer einfachen Fling auf dem Bildschirm auf komplexe Gesten unterstützen, die Multi-Touch einschließen.

Android bietet die `Android.Gestures` Namespace speziell für die Verwaltung von und reagieren auf Gesten. AT-Zentrum der allen Gesten, die eine besondere Klasse mit dem Namen steht `Android.Gestures.GestureDetector`. Wie der Name schon sagt, wird diese Klasse für Gesten und Ereignisse basierend auf Lauschen `MotionEvents` vom Betriebssystem bereitgestellt.

Um eine Geste-detektorklasse zu implementieren, muss eine Aktivität Instanziieren einer `GestureDetector` Klasse, und geben Sie eine Instanz von `IOnGestureListener`, wie der folgende Codeausschnitt veranschaulicht:

```csharp
GestureOverlayView.IOnGestureListener myListener = new MyGestureListener();
_gestureDetector = new GestureDetector(this, myListener);
```

Eine Aktivität muss auch die OnTouchEvent implementieren und die MotionEvent an die Bewegung Erkennung von übergeben. Der folgende Codeausschnitt zeigt ein Beispiel dafür:

```csharp
public override bool OnTouchEvent(MotionEvent e)
{
    // This method is in an Activity
    return _gestureDetector.OnTouchEvent(e);
}
```

Wenn eine Instanz von `GestureDetector` identifiziert eine Geste von Interesse sind, es benachrichtigt der Aktivität oder die Anwendung durch das Auslösen eines Ereignisses oder durch einen Rückruf gebotenen `GestureDetector.IOnGestureListener`.
Diese Schnittstelle bietet sechs Methoden für die verschiedenen Gesten:

-  *OnDown* -wird aufgerufen, wenn eine Tap tritt auf, aber nicht freigegeben.

-  *OnFling* -wird aufgerufen, wenn ein Fling tritt auf, und stellt Daten für die Start- und Enddatum Fingereingabe, die das Ereignis ausgelöst wurde.

-  *OnLongPress* -aufgerufen, wenn das Drücken einer langen auftritt.

-  *OnScroll* -wird aufgerufen, wenn ein Scroll-Ereignis auftritt.

-  *OnShowPress* - wird aufgerufen, nachdem ein OnDown aufgetreten ist und eine verschieben oder um Ereignis wurde nicht ausgeführt.

-  *OnSingleTapUp* -wird aufgerufen, wenn es sich bei einem einfachen tippen auftritt.


In vielen Fällen können Anwendungen nur eine Teilmenge der Gesten interessiert sein. In diesem Fall sollten Anwendungen erweitern Sie die Klasse GestureDetector.SimpleOnGestureListener und überschreiben Sie die Methoden, die die Ereignisse entsprechen, die sie interessiert sind.

## <a name="custom-gestures"></a>Benutzerdefinierten Gesten

Gesten sind eine hervorragende Möglichkeit für Benutzer mit einer Anwendung interagieren. Die APIs, die bisher gesehen haben, für einfache Gesten ausreichen würde, aber möglicherweise etwas sehr aufwändig für etwas komplizierteren Gesten kosteneffizienter. Zur Unterstützung etwas komplizierter Gesten bietet Android einen anderen Satz von APIs im Android.Gestures-Namespace, der Teil der Last zugeordneten benutzerdefinierten Gesten zu vereinfachen, wird.

### <a name="creating-custom-gestures"></a>Erstellen von benutzerdefinierten Gesten

Seit Android 1.6 bietet Android SDK eine Anwendung auf dem Emulator Gesten-Generator vorinstalliert. Diese Anwendung ermöglicht Entwicklern das vordefinierte Gesten erstellen, die in eine Anwendung eingebettet werden kann. Der folgende Screenshot zeigt ein Beispiel des Gesten-Generators:

[![Screenshot der Gesten Generator mit Beispiel-Gesten](touch-in-android-images/image11.png)](touch-in-android-images/image11.png#lightbox)

Eine verbesserte Version dieser Anwendung namens Gestenhandler-Tool kann Google Play gefunden werden. Gestenhandler-Tool ist sehr ähnlich wie Gesten-Generator, außer dass können Sie Gesten zu testen, nachdem sie erstellt wurden. Dieses nächste Screenshot zeigt Gesten-Generator:

[![Screenshot der Gestenhandler Tool mit der Beispiel-Gesten](touch-in-android-images/image12.png)](touch-in-android-images/image12.png#lightbox)

Geste Tool etwas besser eignet sich zum Erstellen von benutzerdefinierter Gesten wie die Gesten getestet werden, wie sie erstellt wird, werden dadurch und problemlos über Google Play verfügbar ist.

Geste Tool ermöglicht, dass Sie eine Geste erstellen, indem auf dem Bildschirm gezeichnet und ein Name zugewiesen. Nachdem die Gesten erstellt wurden, werden diese in einer Binärdatei auf die SD-Karte des Geräts gespeichert. Diese Datei muss vom Gerät abgerufen werden, und klicken Sie dann mit einer Anwendung in den Ordner /Resources/raw verpackt. Diese Datei kann über den Emulator über den Android Debug Bridge abgerufen werden. Das folgende Beispiel zeigt die Datei auf das Ressourcenverzeichnis einer Anwendung aus einer Galaxy Nexus kopieren:

```shell
$ adb pull /storage/sdcard0/gestures <projectdirectory>/Resources/raw
```

Nachdem Sie die Datei abgerufen haben müssen darauf in Ihrer Anwendung in das Verzeichnis Systemablagen verpackte/raw. Die einfachste Möglichkeit zum Verwenden dieser Gestenhandler-Datei wird beim Laden der Datendatei in eine GestureLibrary wie im folgenden Codeausschnitt gezeigt:

```csharp
GestureLibary myGestures = GestureLibraries.FromRawResources(this, Resource.Raw.gestures);
if (!myGestures.Load())
{
    // The library didn't load, so close the activity.
    Finish();
}
```

### <a name="using-custom-gestures"></a>Verwenden von benutzerdefinierten Gesten

Um benutzerdefinierte Aktionen in einer Aktivität erkennen zu können, muss er ein Android.Gesture.GestureOverlay-Objekt, das das zugehörige Layout hinzugefügt haben. Der folgende Codeausschnitt zeigt, wie eine Aktivität eine GestureOverlayView programmgesteuert hinzugefügt wird:

```csharp
GestureOverlayView gestureOverlayView = new GestureOverlayView(this);
gestureOverlayView.AddOnGesturePerformedListener(this);
SetContentView(gestureOverlayView);
```

Der folgende XML-Ausschnitt zeigt, wie eine GestureOverlayView deklarativ hinzugefügt wird:

```xml
<android.gesture.GestureOverlayView
    android:id="@+id/gestures"
    android:layout_width="match_parent "
    android:layout_height="match_parent" />
```

Die `GestureOverlayView` verfügt über mehrere Ereignisse, die ausgelöst werden, während des Prozesses, der eine Geste zeichnen. Das Ereignis am interessantesten ist `GesturePeformed`. Dieses Ereignis wird ausgelöst, wenn der Benutzer ihre Bewegung zeichnen abgeschlossen hat.

Wenn dieses Ereignis ausgelöst wird, fordert die Aktivität eine `GestureLibrary` zu versuchen und die Geste, die der Benutzer mit einem Gesten erstellt vom Tool Geste übereinstimmen. `GestureLibrary` Gibt eine Liste von Objekten der Vorhersage zurück.

Jedes Vorhersagemodell Objekt enthält, ein Ergebnis und den Namen einer der Aktionen in der `GestureLibrary`. Je höher entspricht die Aktion, die mit dem Namen in der Vorhersage das Ergebnis, desto wahrscheinlicher Bewegung vom Benutzer gezeichnet.
Im Allgemeinen werden Ergebnisse, die kleiner als 1,0 schlechter als übereinstimmend angesehen.

Der folgende Code zeigt ein Beispiel für eine Geste entsprechen:

```csharp
private void GestureOverlayViewOnGesturePerformed(object sender, GestureOverlayView.GesturePerformedEventArgs gesturePerformedEventArgs)
{
    // In this example _gestureLibrary was instantiated in OnCreate
    IEnumerable<Prediction> predictions = from p in _gestureLibrary.Recognize(gesturePerformedEventArgs.Gesture)
    orderby p.Score descending
    where p.Score > 1.0
    select p;
    Prediction prediction = predictions.FirstOrDefault();

    if (prediction == null)
    {
        Log.Debug(GetType().FullName, "Nothing matched the user's gesture.");
        return;
    }

    Toast.MakeText(this, prediction.Name, ToastLength.Short).Show();
}
```

Mit diesem fertig sind sollten Sie einen Überblick über die Verwendung von Touch und Gesten in einer Anwendung Xamarin.Android haben. Lassen Sie uns nun mit einer exemplarischen Vorgehensweise fortfahren und alle Konzepte, die in eine funktionierende beispielanwendung angezeigt.



## <a name="related-links"></a>Verwandte Links

- [Android berühren starten (Beispiel)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_start)
- [Android berühren endgültigen (Beispiel)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_final)
