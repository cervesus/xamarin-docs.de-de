---
title: Schreiben von-reaktionsschneller Anwendungen
ms.prod: xamarin
ms.assetid: 452DF940-6331-55F0-D130-002822BBED55
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/15/2018
ms.openlocfilehash: a1642c4cbb790cf09d2a31e629408afc61d5b7ab
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121775"
---
# <a name="writing-responsive-applications"></a>Schreiben von-reaktionsschneller Anwendungen

Einer der Schlüssel zur Aufrechterhaltung der Reaktionsfähigkeit einer grafischen Benutzeroberfläche ist dafür lang andauernden Aufgaben in einem Hintergrundthread die grafischen Benutzeroberfläche nicht blockiert werden. Angenommen, wir möchten einen Wert für den Benutzer angezeigt, aber der Wert 5 Sekunden berechnen dauert zu berechnen:

```csharp
public class ThreadDemo : Activity
{
    TextView textview;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Create a new TextView and set it as our view
        textview = new TextView (this);
        textview.Text = "Working..";

        SetContentView (textview);

        SlowMethod ();
    }

    private void SlowMethod ()
    {
        Thread.Sleep (5000);
        textview.Text = "Method Complete";
    }
}
```

Dies funktioniert, aber die Anwendung "hängen" 5 Sekunden während der Wert berechnet wird. Während dieses Zeitraums wird die app nicht auf keine Benutzerinteraktion reagieren. Um dies zu umgehen, möchten wir unsere Berechnungen in einem Hintergrundthread ausgeführt werden:

```csharp
public class ThreadDemo : Activity
{
    TextView textview;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Create a new TextView and set it as our view
        textview = new TextView (this);
        textview.Text = "Working..";

        SetContentView (textview);

        ThreadPool.QueueUserWorkItem (o => SlowMethod ());
    }

    private void SlowMethod ()
    {
        Thread.Sleep (5000);
        textview.Text = "Method Complete";
    }
}
```

Nun berechnen wir den Wert in einem Hintergrundthread, damit unsere GUI während der Berechnung weiterhin reaktionsfähig bleibt. Aber wenn die Berechnung abgeschlossen ist, stürzt ab, unsere app, verlassen diese in das Protokoll:

```shell
E/mono    (11207): EXCEPTION handling: Android.Util.AndroidRuntimeException: Exception of type 'Android.Util.AndroidRuntimeException' was thrown.
E/mono    (11207):
E/mono    (11207): Unhandled Exception: Android.Util.AndroidRuntimeException: Exception of type 'Android.Util.AndroidRuntimeException' was thrown.
E/mono    (11207):   at Android.Runtime.JNIEnv.CallVoidMethod (IntPtr jobject, IntPtr jmethod, Android.Runtime.JValue[] parms)
E/mono    (11207):   at Android.Widget.TextView.set_Text (IEnumerable`1 value)
E/mono    (11207):   at MonoDroidDebugging.Activity1.SlowMethod ()
```

Dies ist, da die grafischen Benutzeroberfläche von Thread der grafischen Benutzeroberfläche aktualisiert werden muss. Unser Code aktualisiert die GUI aus dem ThreadPool-Thread, die app zum Absturz verursachen. Wir müssen unsere Wert im Hintergrundthread zu berechnen, aber führen Sie dann auf unserem Update für den GUI-Thread, der behandelt wird, mit [Activity.RunOnUIThread](https://developer.xamarin.com/api/member/Android.App.Activity.RunOnUiThread/(System.Action)):

```csharp
public class ThreadDemo : Activity
{
    TextView textview;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Create a new TextView and set it as our view
        textview = new TextView (this);
        textview.Text = "Working..";

        SetContentView (textview);

        ThreadPool.QueueUserWorkItem (o => SlowMethod ());
    }

    private void SlowMethod ()
    {
        Thread.Sleep (5000);
        RunOnUiThread (() => textview.Text = "Method Complete");
    }
}
```

Dieser Code funktioniert wie erwartet. Dieses GUI bleibt reaktionsfähig und ruft ordnungsgemäß aktualisiert, sobald die Berechnung komple ist.

Beachten Sie, dass diese Technik nicht einfach eine teure Wert zu berechnen verwendet. Sie können für eine lang ausgeführte Aufgabe verwendet werden, die im Hintergrund, wie bei einem Aufruf des Webdiensts oder heruntergeladen Daten für das Internet ausgeführt werden können.
