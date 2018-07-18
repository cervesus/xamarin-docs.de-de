---
title: Exemplarische Vorgehensweise – speichern den Status der Aktivität
description: Die Theorie zustandsspeicherung in der Aktivitätenlebenszyklus Handbuch wurde beschrieben; jetzt betrachten wir ein Beispiel.
ms.prod: xamarin
ms.assetid: A6090101-67C6-4BDD-9416-F2FB74805A87
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: e282eeb8732bd5294da4ec4e3fe337e81107c8f3
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
ms.locfileid: "30767426"
---
# <a name="walkthrough---saving-the-activity-state"></a>Exemplarische Vorgehensweise – speichern den Status der Aktivität

_Die Theorie zustandsspeicherung in der Aktivitätenlebenszyklus Handbuch wurde beschrieben; jetzt betrachten wir ein Beispiel._

## <a name="activity-state-walkthrough"></a>Aktivitätszustand Exemplarische Vorgehensweise

Öffnen Sie nun die **ActivityLifecycle_Start** Projekt (in der [ActivityLifecycle](https://developer.xamarin.com/samples/monodroid/ActivityLifecycle) Beispiel), erstellen es und führen Sie es. Dies ist ein sehr einfaches Projekt, das verfügt über zwei Aktivitäten zur Veranschaulichung der Aktivitätenlebenszyklus und wie die verschiedenen Lebenszyklusmethoden aufgerufen werden. Beim Starten der Anwendung den Bildschirm `MainActivity` wird angezeigt: 

[![Bildschirm der Aktivität A](saving-state-images/01-activity-a-sml.png)](saving-state-images/01-activity-a.png#lightbox)

### <a name="viewing-state-transitions"></a>Anzeigen von Zustandsübergängen

Jede Methode in diesem Beispiel schreibt in das Ausgabefenster des IDE-Anwendung zur Angabe des Aktivitätsstatus. (Um das Fenster "Ausgabe" in Visual Studio öffnen, geben Sie **STRG, ALT--O**; klicken Sie zum Öffnen das Fenster "Ausgabe" in Visual Studio für Mac **Ansicht > füllt > Anwendungsausgabe**.)

Zuerst die app gestartet wird, zeigt das Fenster "Ausgabe" die statusänderungen von *Aktivität A*: 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnCreate
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
```

Wenn wir klicken Sie auf die **starten Aktivität B** Schaltfläche wir finden Sie unter *Aktivität A* anhalten und beenden, während *Aktivität B* durchläuft dessen Zustand ändert: 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnPause
[ActivityLifecycle.SecondActivity] Activity B - OnCreate
[ActivityLifecycle.SecondActivity] Activity B - OnStart
[ActivityLifecycle.SecondActivity] Activity B - OnResume
[ActivityLifecycle.MainActivity] Activity A - OnStop
```

Folglich *Aktivität B* gestartet wurde und die angezeigten anstelle von *Aktivität A*: 

[![Bildschirm der Aktivität B](saving-state-images/02-activity-b-sml.png)](saving-state-images/02-activity-b.png#lightbox)

Wenn wir klicken Sie auf die **wieder** Schaltfläche *Aktivität B* zerstört wird und *Aktivität A* fortgesetzt wird: 

```shell
[ActivityLifecycle.SecondActivity] Activity B - OnPause
[ActivityLifecycle.MainActivity] Activity A - OnRestart
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
[ActivityLifecycle.SecondActivity] Activity B - OnStop
[ActivityLifecycle.SecondActivity] Activity B - OnDestroy
```
### <a name="adding-a-click-counter"></a>Hinzufügen eines Click-Leistungsindikators

Als Nächstes werden wir die Anwendung zu ändern, sodass wir verfügen über eine Schaltfläche, die zählt, und zeigt die Anzahl der Häufigkeit, mit die darauf geklickt wird. Zunächst fügen wir eine `_counter` Instanzvariable auf `MainActivity`:

```csharp
int _counter = 0;
```

Als Nächstes ermöglicht das Bearbeiten der **Resource/layout/Main.axml** Layout-Datei, und fügen Sie einen neuen `clickButton` , wie oft der Benutzer hat auf die Schaltfläche geklickt anzeigt. Das resultierende **Main.axml** sollte etwa folgendermaßen aussehen: 

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

Fügen Sie den folgenden Code am Ende der [OnCreate](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) Methode im `MainActivity` &ndash; dieser Code Handles click-Ereignisse aus der `clickButton`:

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

Wenn wir erstellen, und führen Sie die app erneut, eine Schaltfläche "Neu" angezeigt wird, inkrementiert und zeigt den Wert des `_counter` bei jedem klicken:

[![Touch-Anzahl hinzufügen](saving-state-images/03-touched-sml.png)](saving-state-images/03-touched.png#lightbox)

Aber wenn wir das Gerät im Querformat drehen, wird diese Anzahl verloren:

[![Um Querformat drehen, legt die Anzahl die zurück auf NULL fest](saving-state-images/05-rotate-nosave-sml.png)](saving-state-images/05-rotate-nosave.png#lightbox)

Untersuchen die Anwendungsausgabe, sehen Sie, die *Aktivität A* wurde angehalten, beendet, gelöscht, neu erstellt, neu gestartet und dann fortgesetzt, während die Rotation von Hochformat, Querformat: 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnPause
[ActivityLifecycle.MainActivity] Activity A - OnStop
[ActivityLifecycle.MainActivity] Activity A - On Destroy

[ActivityLifecycle.MainActivity] Activity A - OnCreate
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
```

Da *Aktivität A* gelöscht und neu erstellt erneut, wenn das Gerät gedreht wird, der instanzzustand verloren gegangen ist. Danach fügen wir Code zum Speichern und Wiederherstellen des Instanzstatus.

### <a name="adding-code-to-preserve-instance-state"></a>Hinzufügen von Code zu Preserve-Instanzstatus

Fügen Sie eine Methode zum `MainActivity` um den Zustand der Instanz zu speichern. Vor dem *Aktivität A* wird zerstört, Android ruft automatisch [OnSaveInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnSaveInstanceState/p/Android.OS.Bundle/) und übergibt eine [Bundle](https://developer.xamarin.com/api/type/Android.OS.Bundle/) , wir können unsere instanzzustand zu speichern. Wir verwenden sie um unsere auf Anzahl als ganze Zahl zu speichern:

```csharp
protected override void OnSaveInstanceState (Bundle outState)
{
    outState.PutInt ("click_count", _counter);
    Log.Debug(GetType().FullName, "Activity A - Saving instance state");

    // always call the base implementation!
    base.OnSaveInstanceState (outState);    
}
```

Wenn *Aktivität A* wird neu erstellt und fortgesetzt, Android übergibt diesen `Bundle` wieder in unserer `OnCreate` Methode. Fügen Sie Code zum `OnCreate` zum Wiederherstellen der `_counter` aus dem übergebenen Wert `Bundle`. Fügen Sie den folgenden Code unmittelbar vor der Zeile, in denen `clickbutton` definiert ist: 

```csharp
if (bundle != null)
{
    _counter = bundle.GetInt ("click_count", 0);
    Log.Debug(GetType().FullName, "Activity A - Recovered instance state");
}
```

Erstellen Sie, führen Sie die app erneut aus, und klicken Sie mehrmals auf die zweite Schaltfläche. Wenn wir das Gerät im Querformat drehen, wird die Anzahl die beibehalten.

[![Zeigt den Bildschirm drehen, Anzahl der vier beibehalten](saving-state-images/06-rotate-save-sml.png)](saving-state-images/06-rotate-save.png#lightbox)


Werfen wir einen Blick auf das Ausgabefenster, um festzustellen, was passiert ist:
    
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

Vor der [OnStop](https://developer.xamarin.com/api/member/Android.App.Activity.OnStop/) Methode wurde aufgerufen, unsere neue `OnSaveInstanceState` Methode wurde aufgerufen, um das Speichern der `_counter` Wert in eine `Bundle`. Android übergeben dieser `Bundle` uns beim Aufruf unsere `OnCreate` -Methode, und wir konnten verwendet er zum Wiederherstellen der `_counter` Wert an, in dem wir wurde unterbrochen.


## <a name="summary"></a>Zusammenfassung

In diesem Walkthough haben wir Ihrer Kenntnisse des Lebenszyklus der Aktivität verwendet, um Zustandsdaten beizubehalten. 



## <a name="related-links"></a>Verwandte Links

- [ActivityLifecycle (Beispiel)](https://developer.xamarin.com/samples/monodroid/ActivityLifecycle)
- [Aktivitätslebenszyklus](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Android-Aktivität](https://developer.xamarin.com/api/type/Android.App.Activity/)
