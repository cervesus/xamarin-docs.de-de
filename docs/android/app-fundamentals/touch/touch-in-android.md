---
title: Toucheingabe in Android
ms.prod: xamarin
ms.assetid: 405A1FA0-4EFA-4AEB-B672-F36307B9CF16
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 960f75126fdfed770f79e0b4dad5641886eaf8ba
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73024320"
---
# <a name="touch-in-android"></a>Toucheingabe in Android

Ähnlich wie bei IOS erstellt Android ein Objekt, das Daten über die physische Interaktion des Benutzers mit dem Bildschirm &ndash; einem `Android.View.MotionEvent`-Objekt enthält. Dieses Objekt enthält Daten, z. b. welche Aktion ausgeführt wird, wo die Fingereingabe stattfindet, wie viel Druck aufgetreten ist usw. Ein `MotionEvent`-Objekt teilt die Verschiebung in die folgenden Werte auf:

- Ein Aktions Code, der den Typ der Bewegung beschreibt, z. b. den anfänglichen Fingerabdruck, die Fingereingabe, die sich auf dem Bildschirm bewegt, oder das touchende.

- Ein Satz von Achsen Werten, die die Position des `MotionEvent` und andere Verschiebungs Eigenschaften beschreiben, z. b. wo die Fingereingabe stattfindet, wann die Fingereingabe stattfindet und wie viel Druck verwendet wurde.
   Die Achsen Werte können sich je nach Gerät unterscheiden, sodass nicht alle Achsen Werte in der vorherigen Liste beschrieben werden.

Das `MotionEvent` Objekt wird an eine geeignete Methode in einer Anwendung übermittelt. Es gibt drei Möglichkeiten für eine xamarin. Android-Anwendung, auf ein Berührungs Ereignis zu reagieren:

- *Weisen Sie `View.Touch`einen Ereignishandler zu* : die `Android.Views.View` Klasse verfügt über einen `EventHandler<View.TouchEventArgs>` der Anwendungen einen Handler zuweisen können. Dies ist das typische .net-Verhalten.

- Das *Implementieren von `View.IOnTouchListener`* Instanzen dieser Schnittstelle kann mithilfe der-Sicht einem Ansichts Objekt zugewiesen werden. `SetOnListener`-Methode. Dies ist funktional äquivalent zum Zuweisen eines Ereignis Handlers zum `View.Touch`-Ereignis. Wenn eine gemeinsame oder gemeinsam genutzte Logik vorhanden ist, die viele verschiedene Sichten ggf. benötigen, ist es effizienter, eine Klasse zu erstellen und diese Methode zu implementieren, als jede Ansicht als eigenen Ereignishandler zuzuweisen.

- Über *schreiben Sie `View.OnTouchEvent`* alle Sichten in der Android-Unterklasse `Android.Views.View`. Wenn eine Ansicht berührt wird, ruft Android den `OnTouchEvent` auf und übergibt ihm ein `MotionEvent`-Objekt als Parameter.

> [!NOTE]
> Nicht alle Android-Geräte unterstützen Touchscreens. 

Das Hinzufügen des folgenden Tags zu ihrer Manifest-Datei bewirkt, dass Google Play Ihre APP nur auf den Geräten anzeigt, für die Toucheingabe aktiviert ist:

```xml
<uses-configuration android:reqTouchScreen="finger" />
```

## <a name="gestures"></a>Gesten

Eine Geste ist eine handgezeichnete Form auf dem Touchscreen. Eine Geste kann einen oder mehrere Striche enthalten. jeder Strich besteht aus einer Sequenz von Punkten, die von einem anderen Kontaktpunkt mit dem Bildschirm erstellt werden. Android kann viele verschiedene Arten von Gesten unterstützen, von einem einfachen Weg über den Bildschirm bis hin zu komplexen Gesten, die Multitouch betreffen.

Android bietet den `Android.Gestures`-Namespace speziell für die Verwaltung und Reaktion auf Gesten. Das Herzstück aller Gesten ist eine spezielle Klasse mit dem Namen `Android.Gestures.GestureDetector`. Wie der Name schon sagt, lauscht diese Klasse auf Gesten und Ereignisse basierend auf `MotionEvents`, die vom Betriebssystem bereitgestellt werden.

Zum Implementieren eines Gesten Detektors muss eine Aktivität eine `GestureDetector` Klasse instanziieren und eine Instanz von `IOnGestureListener`bereitstellen, wie im folgenden Code Ausschnitt veranschaulicht:

```csharp
GestureOverlayView.IOnGestureListener myListener = new MyGestureListener();
_gestureDetector = new GestureDetector(this, myListener);
```

Eine Aktivität muss auch das ontouchevent implementieren und das "mutionevent" an den Gesten Detektor übergeben. Der folgende Code Ausschnitt zeigt ein Beispiel für Folgendes:

```csharp
public override bool OnTouchEvent(MotionEvent e)
{
    // This method is in an Activity
    return _gestureDetector.OnTouchEvent(e);
}
```

Wenn eine Instanz von `GestureDetector` eine Geste von Interesse identifiziert, wird die Aktivität oder Anwendung entweder durch das Ausführen eines Ereignisses oder durch einen von `GestureDetector.IOnGestureListener`bereitgestellten Rückruf benachrichtigt.
Diese Schnittstelle stellt sechs Methoden für die verschiedenen Gesten bereit:

- *Ondown* : wird aufgerufen, wenn ein TAP auftritt, aber nicht freigegeben wird.

- *Onfling* : wird aufgerufen, wenn ein Fehler auftritt, und stellt Daten zu Beginn und Ende der Fingereingabe bereit, durch die das Ereignis ausgelöst wurde.

- *Onlongpress* : wird aufgerufen, wenn ein langer Pressvorgang durchgeführt wird.

- *OnScroll* : wird aufgerufen, wenn ein scrollereignis auftritt.

- *Onshowpress* : wird aufgerufen, nachdem ein ondown-Ereignis aufgetreten ist und kein Move-oder up-Ereignis ausgeführt wurde.

- *Onsingletapup* : wird aufgerufen, wenn eine einzelne Tap auftritt.

In vielen Fällen sind Anwendungen möglicherweise nur an einer Teilmenge von Gesten interessiert. In diesem Fall sollten Anwendungen die Klasse gesturedetector. simpleongesturelistener erweitern und die Methoden überschreiben, die den Ereignissen entsprechen, an denen Sie interessiert sind.

## <a name="custom-gestures"></a>Benutzerdefinierte Gesten

Gesten sind eine gute Möglichkeit für Benutzer, mit einer Anwendung zu interagieren. Die bisher bereits gesehenen APIs sind für einfache Gesten ausreichend, können sich jedoch für kompliziertere Gesten als etwas schwierig erweisen. Zur Unterstützung komplizierterer Gesten bietet Android einen weiteren Satz von APIs im Android. Gesten-Namespace, der einen Teil der Belastung in Zusammenhang mit benutzerdefinierten Gesten erleichtert.

### <a name="creating-custom-gestures"></a>Erstellen von benutzerdefinierten Gesten

Seit Android 1,6 enthält die Android SDK eine Anwendung, die auf dem Emulator mit dem Namen Gesten-Generator vorinstalliert ist. Diese Anwendung ermöglicht es Entwicklern, vordefinierte Gesten zu erstellen, die in eine Anwendung eingebettet werden können. Der folgende Screenshot zeigt ein Beispiel für Gesten-Generator:

[![Screenshot des Gesten-Generators mit Beispiel Gesten](touch-in-android-images/image11.png)](touch-in-android-images/image11.png#lightbox)

Eine verbesserte Version dieser Anwendung, die als Gesten Tool bezeichnet wird, finden Sie Google Play. Das Gesten Tool ähnelt dem Gesten-Generator, mit dem Unterschied, dass es Ihnen ermöglicht, Gesten zu testen, nachdem Sie erstellt wurden. Der nächste Screenshot zeigt Gesten-Generator:

[![Screenshot des Gesten Tools mit Beispiel Gesten](touch-in-android-images/image12.png)](touch-in-android-images/image12.png#lightbox)

Das Gesten Tool ist für das Erstellen von benutzerdefinierten Gesten etwas nützlicher, da die Gesten bei der Erstellung getestet werden können und problemlos über Google Play verfügbar sind.

Mit dem Gesten Tool können Sie eine Geste erstellen, indem Sie auf dem Bildschirm zeichnen und einen Namen zuweisen. Nachdem die Gesten erstellt wurden, werden Sie in einer Binärdatei auf der SD-Karte des Geräts gespeichert. Diese Datei muss vom Gerät abgerufen und dann mit einer Anwendung im Ordner/Resources/RAW. verpackt werden. Diese Datei kann mithilfe des-Android Debug Bridge aus dem Emulator abgerufen werden. Das folgende Beispiel zeigt, wie Sie die Datei aus einem Galaxy Nexus in das Ressourcenverzeichnis einer Anwendung kopieren:

```shell
$ adb pull /storage/sdcard0/gestures <projectdirectory>/Resources/raw
```

Nachdem Sie die Datei abgerufen haben, muss Sie mit Ihrer Anwendung im Verzeichnis/Resources/RAW. verpackt werden. Die einfachste Möglichkeit, diese Gesten Datei zu verwenden, besteht darin, die Datei in ein gesturelibrary zu laden, wie im folgenden Code Ausschnitt gezeigt:

```csharp
GestureLibrary myGestures = GestureLibraries.FromRawResources(this, Resource.Raw.gestures);
if (!myGestures.Load())
{
    // The library didn't load, so close the activity.
    Finish();
}
```

### <a name="using-custom-gestures"></a>Verwenden von benutzerdefinierten Gesten

Zum Erkennen von benutzerdefinierten Gesten in einer Aktivität muss dem Layout ein Android. Gesten. gestureoverlay-Objekt hinzugefügt werden. Der folgende Code Ausschnitt zeigt, wie einer Aktivität Programm gesteuert ein gestureoverlayview-Element hinzugefügt wird:

```csharp
GestureOverlayView gestureOverlayView = new GestureOverlayView(this);
gestureOverlayView.AddOnGesturePerformedListener(this);
SetContentView(gestureOverlayView);
```

Der folgende XML-Code Ausschnitt zeigt, wie Sie eine gestureoverlayview deklarativ hinzufügen:

```xml
<android.gesture.GestureOverlayView
    android:id="@+id/gestures"
    android:layout_width="match_parent "
    android:layout_height="match_parent" />
```

Der `GestureOverlayView` verfügt über mehrere Ereignisse, die während des Zeichnens einer Geste ausgelöst werden. Das interessanteste Ereignis ist `GesturePerformed`. Dieses Ereignis wird ausgelöst, wenn der Benutzer das Zeichnen der Geste abgeschlossen hat.

Wenn dieses Ereignis ausgelöst wird, fordert die-Aktivität eine `GestureLibrary` auf, die Geste zu versuchen, die dem Benutzer mit einer der Gesten Tool erstellt wurde, die von Gesten Tool erstellt wurden. `GestureLibrary` gibt eine Liste der Vorhersage Objekte zurück.

Jedes Vorhersage Objekt enthält eine Bewertung und einen Namen einer der Gesten in der `GestureLibrary`. Je höher das Ergebnis, desto wahrscheinlicher entspricht die in der Vorhersage benannte Geste der vom Benutzer gezeichneten Bewegung.
Im Allgemeinen werden Bewertungen, die niedriger als 1,0 sind, als schlechte Übereinstimmungen angesehen.

Der folgende Code zeigt ein Beispiel für die Übereinstimmung einer Geste:

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

Dabei sollten Sie sich mit der Verwendung von toucheingaben und Gesten in einer xamarin. Android-Anwendung vertraut machen. Wir fahren nun mit einer exemplarischen Vorgehensweise fort und zeigen alle Konzepte in einer funktionierenden Beispielanwendung an.

## <a name="related-links"></a>Verwandte Links

- [Android-Berührungs Start (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-touch-start)
- [Android-Berührungs Finale (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-touch-final)
