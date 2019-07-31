---
title: 'Exemplarische Vorgehensweise: Speichern des Aktivitätsstatus'
description: Wir haben die Theorie hinter dem Speichern des Zustands im Aktivitäts Lebenszyklus Handbuch behandelt. sehen wir uns nun ein Beispiel an.
ms.prod: xamarin
ms.assetid: A6090101-67C6-4BDD-9416-F2FB74805A87
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 9fafc6965c5d2dec79f440579a5cf3746a545bae
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68644393"
---
# <a name="walkthrough---saving-the-activity-state"></a>Exemplarische Vorgehensweise: Speichern des Aktivitätsstatus

_Wir haben die Theorie hinter dem Speichern des Zustands im Aktivitäts Lebenszyklus Handbuch behandelt. sehen wir uns nun ein Beispiel an._

## <a name="activity-state-walkthrough"></a>Exemplarische Vorgehensweise zum Aktivitäts Zustand

Öffnen Sie das Projekt **ActivityLifecycle_Start** (im Beispiel [activitylifecycle](https://docs.microsoft.com/samples/xamarin/monodroid-samples/activitylifecycle) ), erstellen Sie es, und führen Sie es aus. Dabei handelt es sich um ein sehr einfaches Projekt, das zwei Aktivitäten umfasst, um den Aktivitäts Lebenszyklus zu veranschaulichen, und wie die verschiedenen Lebenszyklus Methoden aufgerufen werden. Wenn Sie die Anwendung starten, wird der Bild `MainActivity` Schirm von angezeigt:

[![Aktivität A Bildschirm](saving-state-images/01-activity-a-sml.png)](saving-state-images/01-activity-a.png#lightbox)

### <a name="viewing-state-transitions"></a>Anzeigen von Zustands Übergängen

Jede Methode in diesem Beispiel schreibt in das Fenster der IDE-Anwendungs Ausgabe, um den Aktivitäts Zustand anzugeben. (Um das Fenster Ausgabe in Visual Studio zu öffnen, geben Sie **STRG + ALT + O**; ein, um das Ausgabefenster in Visual Studio für Mac zu öffnen, und klicken Sie auf **> Pads > Anwendungs Ausgabe anzeigen**.)

Wenn die APP zum ersten Mal gestartet wird, zeigt das Fenster Ausgabe die Zustandsänderungen von *Aktivität A*an: 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnCreate
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
```

Wenn Sie auf die Schaltfläche **Start Aktivität b** klicken, wird *Aktivität a* anhalten und beenden angezeigt, während *Aktivität b* die Statusänderungen durchläuft: 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnPause
[ActivityLifecycle.SecondActivity] Activity B - OnCreate
[ActivityLifecycle.SecondActivity] Activity B - OnStart
[ActivityLifecycle.SecondActivity] Activity B - OnResume
[ActivityLifecycle.MainActivity] Activity A - OnStop
```

Daher wird *Aktivität B* gestartet und anstelle von *Aktivität a*angezeigt: 

[![Bildschirm für Aktivität B](saving-state-images/02-activity-b-sml.png)](saving-state-images/02-activity-b.png#lightbox)

Wenn Sie auf die Schaltfläche " **zurück** " klicken, wird *Aktivität B* zerstört und *Aktivität a* wird fortgesetzt: 

```shell
[ActivityLifecycle.SecondActivity] Activity B - OnPause
[ActivityLifecycle.MainActivity] Activity A - OnRestart
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
[ActivityLifecycle.SecondActivity] Activity B - OnStop
[ActivityLifecycle.SecondActivity] Activity B - OnDestroy
```
### <a name="adding-a-click-counter"></a>Hinzufügen eines Klick Zählers

Als nächstes ändern wir die Anwendung so, dass eine Schaltfläche angezeigt wird, die anzeigt, wie oft auf Sie geklickt wird. Fügen Sie zunächst eine `_counter` `MainActivity`Instanzvariable hinzu:

```csharp
int _counter = 0;
```

Als nächstes bearbeiten wir die Layoutdatei " **Resource/Layout/Main. axml** " und fügen `clickButton` eine neue hinzu, die anzeigt, wie oft der Benutzer auf die Schaltfläche geklickt hat. Die resultierende **Main. axml** sollte in etwa wie folgt aussehen: 

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

Fügen Sie den folgenden Code am Ende der [OnCreate](xref:Android.App.Activity.OnCreate*) -Methode in `MainActivity` &ndash; den Code Handles Click-Ereignisse aus der `clickButton`-Datei ein:

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

Wenn Sie die APP erneut erstellen und ausführen, wird eine neue Schaltfläche angezeigt, in der der Wert von `_counter` bei jedem Mausklick erhöht und angezeigt wird:

[![Berührungs Anzahl hinzufügen](saving-state-images/03-touched-sml.png)](saving-state-images/03-touched.png#lightbox)

Wenn wir das Gerät jedoch in den Querformat drehen, gehen diese Anzahl verloren:

[![Beim Drehen in das Querformat wird die Anzahl auf 0 zurückgesetzt.](saving-state-images/05-rotate-nosave-sml.png)](saving-state-images/05-rotate-nosave.png#lightbox)

Wenn Sie die Anwendungs Ausgabe untersuchen, sehen wir, dass *Aktivität A* angehalten, beendet, zerstört, neu erstellt, neu gestartet und anschließend während der Drehung vom Hochformat in den Querformat Modus wieder aufgenommen wurde: 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnPause
[ActivityLifecycle.MainActivity] Activity A - OnStop
[ActivityLifecycle.MainActivity] Activity A - On Destroy

[ActivityLifecycle.MainActivity] Activity A - OnCreate
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
```

Da *Aktivität A* beim Drehen des Geräts zerstört und erneut erstellt wird, geht der Instanzzustand verloren. Als Nächstes fügen Sie Code hinzu, um den Instanzzustand zu speichern und wiederherzustellen.

### <a name="adding-code-to-preserve-instance-state"></a>Hinzufügen von Code zum Beibehalten des Instanzstatus

Fügen Sie eine Methode `MainActivity` hinzu, um den Instanzzustand zu speichern. Bevor *Aktivität A* zerstört wird, ruft Android automatisch [onsaveinstancestate](xref:Android.App.Activity.OnSaveInstanceState*) auf und übergibt ein [Bündel](xref:Android.OS.Bundle) , mit dem wir den Instanzzustand speichern können. Wir verwenden es zum Speichern der Klick Anzahl als ganzzahliger Wert:

```csharp
protected override void OnSaveInstanceState (Bundle outState)
{
    outState.PutInt ("click_count", _counter);
    Log.Debug(GetType().FullName, "Activity A - Saving instance state");

    // always call the base implementation!
    base.OnSaveInstanceState (outState);    
}
```

Wenn *Aktivität A* neu erstellt und fortgesetzt wird, übergibt Android `Bundle` diese wieder an `OnCreate` unsere Methode. Fügen Sie Code `OnCreate` hinzu, um den `_counter` Wert `Bundle`aus dem weiter gegebenen wiederherzustellen. Fügen Sie den folgenden Code direkt vor der Zeile `clickbutton` ein, in der definiert ist: 

```csharp
if (bundle != null)
{
    _counter = bundle.GetInt ("click_count", 0);
    Log.Debug(GetType().FullName, "Activity A - Recovered instance state");
}
```

Erstellen Sie die APP erneut, und führen Sie Sie aus, und klicken Sie dann mehrmals auf die zweite Schaltfläche. Wenn wir das Gerät in den Querformat drehen, wird die Anzahl beibehalten.

[![Drehen des Bildschirms zeigt die Anzahl von vier beibehaltenen](saving-state-images/06-rotate-save-sml.png)](saving-state-images/06-rotate-save.png#lightbox)

Sehen wir uns das Ausgabefenster an, um zu sehen, was passiert ist:

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

Bevor die [onstoppt](xref:Android.App.Activity.OnStop) -Methode aufgerufen wurde, wurde `OnSaveInstanceState` die neue-Methode aufgerufen, `_counter` um den Wert `Bundle`in einem zu speichern. Android hat diese `Bundle` `OnCreate` Methode an uns zurückgegeben, als die Methode aufgerufen wurde, und wir konnten Sie verwenden, um `_counter` den Wert an der Stelle wiederherzustellen, an der wir aufgehört haben.

## <a name="summary"></a>Zusammenfassung

In dieser exemplentyp Anleitung haben wir unsere Kenntnisse des Aktivitäts Lebenszyklus verwendet, um Zustandsdaten beizubehalten.

## <a name="related-links"></a>Verwandte Links

- [Activitylifecycle (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/activitylifecycle)
- [Aktivitätslebenszyklus](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Android-Aktivität](xref:Android.App.Activity)
