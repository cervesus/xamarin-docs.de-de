---
title: Toucheingabe in Android
ms.prod: xamarin
ms.assetid: 405A1FA0-4EFA-4AEB-B672-F36307B9CF16
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: a25a1c3be8c952536c0ef40b7f7c4a64f5748516
ms.sourcegitcommit: 7eed80186e23e6aff3ddbbf7ce5cd1fa20af1365
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2018
ms.locfileid: "51527234"
---
# <a name="touch-in-android"></a>Toucheingabe in Android

Viel wie iOS, Android erstellt ein Objekt, das Daten über physische Interaktion mit dem Bildschirm des Benutzers enthält &ndash; ein `Android.View.MotionEvent` Objekt. Dieses Objekt enthält Daten wie z. B. welche Aktion ausgeführt wird, dauerte die Berührung platzieren, wie viel Druck wurde angewendet, usw. Ein `MotionEvent` Objekt gliedert die Verschiebung in den folgenden Werten:

-  Eine Aktionscode, der den Typ der während der Übertragung, z. B. die erste Berührung, die Fingereingabe, die über den Bildschirm oder das Touch-Ende verschieben beschreibt.

-  Ein Satz von Achsenwerten, die die Position des beschreiben die `MotionEvent` und andere Eigenschaften Bewegung, z. B., in denen die Berührung stattfindet, wenn die Fingereingabe stattgefunden hat, und wie viel Druck verwendet wurde.
   Die Achsenwerten möglicherweise je nach dem Gerät, damit alle Achsenwerte nicht in die obigen Liste beschrieben werden.


Die `MotionEvent` Objekt wird an einer geeigneten Methode in einer Anwendung übergeben werden. Es gibt drei Möglichkeiten für eine Xamarin.Android-Anwendung auf ein touchereignis reagiert:

-  *Weisen Sie einen Ereignishandler an `View.Touch`*  : die `Android.Views.View` -Klasse verfügt über eine `EventHandler<View.TouchEventArgs>` welche Anwendungen können einen Handler zum zuweisen. Dies ist das normale Verhalten von .NET.

-  *Implementieren von `View.IOnTouchListener`*  -Instanzen, die dieser Schnittstelle können ein Objekt mithilfe der Ansicht zugewiesen werden. `SetOnListener` -Methode. Diese ist funktionell äquivalent zum Zuweisen von einen Ereignishandler an das `View.Touch` Ereignis. Ist es allgemeine oder gemeinsam genutzte Logik, die viele verschiedene Ansichten möglicherweise, wenn sie betroffen sind, werden effizienter, erstellen Sie eine Klasse und implementieren Sie diese Methode als zum Zuweisen einer Ansicht jeweils eines eigenen ereignishandlers.

-  *Außer Kraft setzen `View.OnTouchEvent`*  -alle Sichten in der Android-Unterklasse `Android.Views.View`. Bei eine Ansicht verwendet wird, ruft Android die `OnTouchEvent` , und übergeben sie einen `MotionEvent` -Objekt als Parameter.


> [!NOTE]
> Nicht alle Android-Geräte unterstützen Touchscreens. 

Das folgende Tag zur Manifestdatei hinzufügen bewirkt, dass Google Play nur Anzeige Ihrer app für diese Geräte mit Fingereingabe aktiviert:

```xml
<uses-configuration android:reqTouchScreen="finger" />
```

## <a name="gestures"></a>Gesten

Eine Geste ist eine Hand gezeichnetes Form auf dem Touchscreen. Eine Geste kann eine oder mehrere Striche, jeden Strich, bestehend aus einer Sequenz von Punkten, die von einem anderen Zeitpunkt der Verbindung mit dem Bildschirm erstellt haben. Android kann viele verschiedene Bewegungen von einem einfachen liaison auf dem Bildschirm auf komplexe Gesten unterstützen, bei denen Multi-Touch.

Android bietet die `Android.Gestures` Namespace speziell für das Verwalten von und reagieren auf Gesten. An das Herzstück der alle Gesten ist eine spezielle Klasse namens `Android.Gestures.GestureDetector`. Wie der Name schon sagt, um auf Gesten und Ereignisse auf Grundlage dieser Klasse Lauschen `MotionEvents` vom Betriebssystem bereitgestellt.

Um eine Erkennung von Bewegung zu implementieren, muss eine Aktivität Instanziieren einer `GestureDetector` Klasse, und stellen eine Instanz von `IOnGestureListener`wie der folgende Codeausschnitt veranschaulicht:

```csharp
GestureOverlayView.IOnGestureListener myListener = new MyGestureListener();
_gestureDetector = new GestureDetector(this, myListener);
```

Eine Aktivität muss auch die OnTouchEvent implementieren und die MotionEvent an die Erkennung von Bewegung zu übergeben. Der folgende Codeausschnitt zeigt ein Beispiel hierfür:

```csharp
public override bool OnTouchEvent(MotionEvent e)
{
    // This method is in an Activity
    return _gestureDetector.OnTouchEvent(e);
}
```

Wenn eine Instanz des `GestureDetector` identifiziert eine Bewegung von Interesse sind, sie werden benachrichtigt, die Aktivität oder die Anwendung entweder durch Auslösen eines Ereignisses oder über einen Rückruf von bereitgestellten `GestureDetector.IOnGestureListener`.
Diese Schnittstelle bietet sechs Methoden für die verschiedenen Gesten:

-  *OnDown* -wird aufgerufen, wenn ein tippen tritt auf, aber nicht freigegeben.

-  *OnFling* -wird aufgerufen, wenn eine liaison tritt auf, und stellt Daten bereit, die Start- und Endzeit Berührung, die das Ereignis ausgelöst hat.

-  *OnLongPress* -wird aufgerufen, wenn ein langes Drücken auftritt.

-  *OnScroll* -wird aufgerufen, wenn ein Bildlaufereignis auftritt.

-  *OnShowPress* : wird aufgerufen, nachdem ein OnDown aufgetreten ist, und eine Verschiebung oder Ereignis nicht ausgeführt wurde.

-  *OnSingleTapUp* -wird aufgerufen, wenn es sich bei einem einzigen fingertipp auftritt.


In vielen Fällen können Anwendungen nur eine Teilmenge der Bewegungen interessiert sein. In diesem Fall sollten Anwendungen erweitern Sie die Klasse GestureDetector.SimpleOnGestureListener und überschreiben Sie die Methoden, die die Ereignisse zu entsprechen, denen sie interessiert sind.

## <a name="custom-gestures"></a>Benutzerdefinierten Stiftbewegungen

Aktionen sind eine hervorragende Möglichkeit für Benutzer mit einer Anwendung interagieren. Die APIs, die wir eben gesehen haben für einfache Gesten ausreichen würde, aber möglicherweise etwas schwer für komplexere Gesten nachzuweisen. Damit mit komplizierteren Bewegungen, bietet Android eine andere Gruppe von APIs in der Android.Gestures-Namespace, der Teil der Last zugeordneten benutzerdefinierten stiftbewegungen erleichtern.

### <a name="creating-custom-gestures"></a>Erstellen von benutzerdefinierten Gesten

Seit Android 1.6 enthält das Android-SDK eine Anwendung, die bereits auf dem Emulator Gesten-Generator installiert. Diese Anwendung kann ein Entwickler vordefinierte Gesten erstellen, die in eine Anwendung eingebettet werden kann. Der folgende Screenshot zeigt ein Beispiel für Gesten-Generator:

[![Screenshot der Bewegungen-Generator mit Beispiel-Bewegungen](touch-in-android-images/image11.png)](touch-in-android-images/image11.png#lightbox)

Eine verbesserte Version dieser Anwendung namens Geste-Tool finden Sie Google Play. Aktion-Tool ist sehr ähnlich wie Gesten-Generator, außer dass können Sie Aktionen testen, nachdem sie erstellt wurden. Dieser nächste Screenshot zeigt die Gesten-Generator:

[![Der Bewegung Screenshottool bei den Beispiel-Bewegungen](touch-in-android-images/image12.png)](touch-in-android-images/image12.png#lightbox)

Aktion-Tool eignet sich ein wenig mehr zum Erstellen von benutzerdefinierter stiftbewegungen, da die Gesten getestet werden, wie diese erstellt werden können und ganz einfach über Google Play verfügbar ist.

Aktion-Tool ermöglicht, dass Sie eine Geste für eine erstellen, indem Sie auf dem Bildschirm zeichnen und Zuweisen eines Namens. Nachdem die Gesten erstellt wurden, werden diese in einer Binärdatei auf die SD-Karte des Geräts gespeichert. Diese Datei muss vom Gerät abgerufen und dann mit einer Anwendung in den Ordner /Resources/raw verpackt werden. Diese Datei kann über den Emulator mit Android Debug Bridge abgerufen werden. Das folgende Beispiel zeigt die Datei aus ein Galaxy Nexus auf das Ressourcenverzeichnis einer Anwendung zu kopieren:

```shell
$ adb pull /storage/sdcard0/gestures <projectdirectory>/Resources/raw
```

Nachdem Sie die Datei abgerufen haben, muss sie in Ihrer Anwendung in das Verzeichnis Systemablagen verpackte/raw. Die einfachste Möglichkeit, diese Geste-Datei verwenden, wird beim Laden der Datei in eine GestureLibrary, wie im folgenden Codeausschnitt gezeigt:

```csharp
GestureLibary myGestures = GestureLibraries.FromRawResources(this, Resource.Raw.gestures);
if (!myGestures.Load())
{
    // The library didn't load, so close the activity.
    Finish();
}
```

### <a name="using-custom-gestures"></a>Verwenden von benutzerdefinierten Gesten

Um benutzerdefinierten stiftbewegungen in einer Aktivität erkennen zu können, muss es ein Android.Gesture.GestureOverlay-Objekt, das hinzugefügt werden, um das zugehörige Layout verfügen. Der folgende Codeausschnitt zeigt, wie eine Aktivität eine GestureOverlayView programmgesteuert hinzugefügt wird:

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

Die `GestureOverlayView` verfügt über mehrere Ereignisse, die im Rahmen des Zeichnen einer Geste ausgelöst wird. Das Ereignis am interessantesten ist `GesturePerformed`. Dieses Ereignis wird ausgelöst, wenn der Benutzer zeichnen ihre Aktion abgeschlossen wurde.

Wenn dieses Ereignis ausgelöst wird, fordert die Aktivität ein `GestureLibrary` testen und die Aktion, die der Benutzer mit einem Gesten erstellt durch Bewegung Tool entsprechen. `GestureLibrary` Gibt eine Liste von Vorhersage-Objekten zurück.

Jedes Vorhersagemodell-Objekt enthält, eine Bewertung und den Namen eines der die Aktionen in der `GestureLibrary`. Je höher der Wert entspricht die Eingabeaktion, die mit dem Namen in die Vorhersage der Bewertung, desto wahrscheinlicher die Eingabeaktion, die vom Benutzer gezeichneter.
Im Allgemeinen werden Werte kleiner als 1,0 schlechter als übereinstimmend angesehen.

Der folgende Code zeigt ein Beispiel für den Abgleich eine Geste für einer:

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

Dies erfolgt ist benötigen Sie ein Verständnis dafür, wie Sie Berührung und Gesten in einer Xamarin.Android-Anwendung verwenden. Lassen Sie uns nun mit einer exemplarischen Vorgehensweise fortfahren Sie, und sehen Sie sich die Konzepte in eine funktionierende beispielanwendung.



## <a name="related-links"></a>Verwandte Links

- [Android Touch starten (Beispiel)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_start)
- [Android Touch endgültige (Beispiel)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_final)
