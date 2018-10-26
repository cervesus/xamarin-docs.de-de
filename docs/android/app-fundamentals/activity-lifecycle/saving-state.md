---
title: 'Exemplarische Vorgehensweise: Speichern des Aktivitätsstatus'
description: Wir haben die Theorie hinter den Status speichern, in der Aktivitätslebenszyklus Anleitung behandelt; jetzt betrachten wir ein Beispiel.
ms.prod: xamarin
ms.assetid: A6090101-67C6-4BDD-9416-F2FB74805A87
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: c8f92e55648dff469227cc3bad981ad5f6e6d0ac
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50122126"
---
# <a name="walkthrough---saving-the-activity-state"></a>Exemplarische Vorgehensweise: Speichern des Aktivitätsstatus

_Wir haben die Theorie hinter den Status speichern, in der Aktivitätslebenszyklus Anleitung behandelt; jetzt betrachten wir ein Beispiel._

## <a name="activity-state-walkthrough"></a>Aktivitätsstatus Exemplarische Vorgehensweise

Öffnen wir die **ActivityLifecycle_Start** Projekt (in der [ActivityLifecycle](https://developer.xamarin.com/samples/monodroid/ActivityLifecycle) Beispiel), erstellen es und führen Sie es. Dies ist ein sehr einfaches Projekt, das hat zwei Aktivitäten veranschaulicht, wie der aktivitätslebenszyklus und wie die verschiedenen Lebenszyklusmethoden aufgerufen werden. Beim Starten der Anwendung den Bildschirm `MainActivity` wird angezeigt: 

[![Aktivität A-Bildschirm](saving-state-images/01-activity-a-sml.png)](saving-state-images/01-activity-a.png#lightbox)

### <a name="viewing-state-transitions"></a>Anzeigen von Zustandsübergängen

Jede Methode in diesem Beispiel werden im Ausgabefenster von IDE-Anwendung an die Aktivität schreibt. (Geben Sie zum Öffnen im Ausgabefenster in Visual Studio **STRG-ALT-O**; um das Fenster "Ausgabe" in Visual Studio für Mac öffnen, klicken Sie auf **Ansicht > Pads > Anwendungsausgabe**.)

Beim ersten Start die app wird im Ausgabefenster werden die Änderungen des Ansichtszustands *Aktivität A*: 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnCreate
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
```

Klicken auf die **starten Aktivität B** Schaltfläche sehen *Aktivität A* anhalten und beenden und *Aktivität B* durchläuft dessen Zustand ändert: 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnPause
[ActivityLifecycle.SecondActivity] Activity B - OnCreate
[ActivityLifecycle.SecondActivity] Activity B - OnStart
[ActivityLifecycle.SecondActivity] Activity B - OnResume
[ActivityLifecycle.MainActivity] Activity A - OnStop
```

Daher *Aktivität B* gestartet wurde und angezeigt werden, anstelle von *Aktivität A*: 

[![Bildschirm der Aktivität B](saving-state-images/02-activity-b-sml.png)](saving-state-images/02-activity-b.png#lightbox)

Klicken auf die **wieder** Schaltfläche *Aktivität B* zerstört wird und *Aktivität A* fortgesetzt wird: 

```shell
[ActivityLifecycle.SecondActivity] Activity B - OnPause
[ActivityLifecycle.MainActivity] Activity A - OnRestart
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
[ActivityLifecycle.SecondActivity] Activity B - OnStop
[ActivityLifecycle.SecondActivity] Activity B - OnDestroy
```
### <a name="adding-a-click-counter"></a>Hinzufügen eines Click-Leistungsindikators

Als Nächstes werden wir die Anwendung zu ändern, sodass wir eine Schaltfläche verfügen, die zählt, und zeigt die Anzahl der Male, die darauf geklickt wird. Zunächst fügen wir eine `_counter` Instanzvariable hinzu `MainActivity`:

```csharp
int _counter = 0;
```

Als Nächstes ermöglicht das Bearbeiten der **Resource/layout/Main.axml** Layout-Datei, und fügen Sie einen neuen `clickButton` , das zeigt an, wie oft der Benutzer die Schaltfläche geklickt hat. Die resultierende **Main.axml** sollte etwa folgendermaßen aussehen: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">
    <Button
        android:id="@+id/myButton"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="@string/mybutton_text" />
    <Button
        android:id="@+id/clickButton"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="@string/counterbutton_text" />
</LinearLayout>
```

Fügen Sie den folgenden Code am Ende der [OnCreate](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) -Methode in der `MainActivity` &ndash; dieser Code behandelt, klicken Sie auf Ereignisse aus der `clickButton`:

```csharp
var clickbutton = FindViewById<Button> (Resource.Id.clickButton);
clickbutton.Text = Resources.GetString (
    Resource.String.counterbutton_text, _counter);
clickbutton.Click += (object sender, System.EventArgs e) =>
{
    _counter++;
    clickbutton.Text = Resources.GetString (
        Resource.String.counterbutton_text, _counter);
} ;
```

Wenn wir zum Erstellen und führen die app erneut aus, das eine neue Schaltfläche angezeigt wird, die inkrementiert, und zeigt den Wert der `_counter` bei jedem klicken:

[![Hinzufügen von Touch-Anzahl](saving-state-images/03-touched-sml.png)](saving-state-images/03-touched.png#lightbox)

Aber wenn wir das Gerät in das Querformat zu drehen, wird diese Anzahl verloren:

[![Zum Querformat zu drehen, legt die Anzahl an 0 (null) fest](saving-state-images/05-rotate-nosave-sml.png)](saving-state-images/05-rotate-nosave.png#lightbox)

Untersuchen die Anwendungsausgabe, sehen Sie, dass *Aktivität A* wurde angehalten, beendet, gelöscht, neu erstellt, neu gestartet und dann fortgesetzt, während die Rotation vom Hochformat zum Querformat: 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnPause
[ActivityLifecycle.MainActivity] Activity A - OnStop
[ActivityLifecycle.MainActivity] Activity A - On Destroy

[ActivityLifecycle.MainActivity] Activity A - OnCreate
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
```

Da *Aktivität A* zerstört und neu erstellt noch Mal, wenn das Gerät gedreht wird, wird der Zustand geht verloren. Als Nächstes werden wir Code zum Speichern und Wiederherstellen des Instanzstatus hinzufügen.

### <a name="adding-code-to-preserve-instance-state"></a>Hinzufügen von Code zum Beibehalten von Instanzstatus

Fügen Sie eine Methode, um `MainActivity` um den Zustand der Instanz zu speichern. Vor dem *Aktivität A* wird zerstört, Android ruft automatisch [OnSaveInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnSaveInstanceState/p/Android.OS.Bundle/) auf und übergibt eine [Bundle](https://developer.xamarin.com/api/type/Android.OS.Bundle/) , mit denen wir unsere instanzzustand zu speichern. Sie verwenden wir die um Anzahl der Klicks als ganzzahligen Wert zu speichern:

```csharp
protected override void OnSaveInstanceState (Bundle outState)
{
    outState.PutInt ("click_count", _counter);
    Log.Debug(GetType().FullName, "Activity A - Saving instance state");

    // always call the base implementation!
    base.OnSaveInstanceState (outState);    
}
```

Wenn *Aktivität A* neu erstellt und fortgesetzt wird, ist Android übergibt diesen `Bundle` wieder unsere `OnCreate` Methode. Fügen Sie Code zum `OnCreate` zum Wiederherstellen der `_counter` Wert aus der übergebenen `Bundle`. Fügen Sie den folgenden Code unmittelbar vor der Zeile, in denen `clickbutton` definiert ist: 

```csharp
if (bundle != null)
{
    _counter = bundle.GetInt ("click_count", 0);
    Log.Debug(GetType().FullName, "Activity A - Recovered instance state");
}
```

Erstellen Sie und führen Sie die app erneut aus, und klicken Sie mehrmals auf die zweite Schaltfläche. Wenn wir das Gerät im Querformat drehen, wird die Anzahl die beibehalten.

[![Drehen den Bildschirm zeigt die Anzahl von vier beibehalten](saving-state-images/06-rotate-save-sml.png)](saving-state-images/06-rotate-save.png#lightbox)


Werfen wir einen Blick auf das Ausgabefenster ", was passiert ist:
    
```shell
[ActivityLifecycle.MainActivity] Activity A - OnPause
[ActivityLifecycle.MainActivity] Activity A - Saving instance state
[ActivityLifecycle.MainActivity] Activity A - OnStop
[ActivityLifecycle.MainActivity] Activity A - On Destroy

[ActivityLifecycle.MainActivity] Activity A - OnCreate
[ActivityLifecycle.MainActivity] Activity A - Recovered instance state
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
``` 

Vor der [OnStop](https://developer.xamarin.com/api/member/Android.App.Activity.OnStop/) Methode wurde aufgerufen, unseres neuen `OnSaveInstanceState` Methode wurde aufgerufen, um das Speichern der `_counter` Wert in eine `Bundle`. Android übergeben dieser `Bundle` an uns, wenn sie aufgerufen unsere `OnCreate` -Methode, und wir waren in der Lage, verwendet es zum Wiederherstellen der `_counter` Wert an, wo wir aufgehört.


## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise haben wir Ihrer Kenntnisse der Aktivitätslebenszyklus verwendet, um Daten zu erhalten. 



## <a name="related-links"></a>Verwandte Links

- [ActivityLifecycle (Beispiel)](https://developer.xamarin.com/samples/monodroid/ActivityLifecycle)
- [Aktivitätslebenszyklus](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Android-Aktivität](https://developer.xamarin.com/api/type/Android.App.Activity/)
