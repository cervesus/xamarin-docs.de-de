---
title: Schreiben von Clientanwendungen reaktionsfähig
ms.prod: xamarin
ms.assetid: 452DF940-6331-55F0-D130-002822BBED55
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: b8c113b67b3fbfa57ca86c72e11ddeb0e4e1a9ab
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="writing-responsive-applications"></a>Schreiben von Clientanwendungen reaktionsfähig

Einer der Schlüssel für die Aufrechterhaltung einer reaktionsfähigen GUI ist dazu lang andauernden Aufgaben in einem Hintergrundthread auf die Benutzeroberfläche nicht blockiert werden. Angenommen, wir möchten berechnen eines Werts für den Benutzer angezeigt, jedoch, dass der Wert 5 Sekunden berechnet wird:

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

Dies funktioniert, aber die Anwendung "hängen" 5 Sekunden lang während der Wert berechnet wird. Während dieser Zeit werden die app nicht Antworten auf Eingreifen des Benutzers. Um dieses Problem umgehen möchten wir unsere Berechnungen in einem Hintergrundthread tun:

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

Jetzt berechnen wir den Wert in einem Hintergrundthread, damit bei der Berechnung unserer GUI reaktionsfähig bleibt. Jedoch, wenn die Berechnung abgeschlossen ist, abstürzt unserer app, verlassen diese in das Protokoll:

```shell
E/mono    (11207): EXCEPTION handling: Android.Util.AndroidRuntimeException: Exception of type 'Android.Util.AndroidRuntimeException' was thrown.
E/mono    (11207):
E/mono    (11207): Unhandled Exception: Android.Util.AndroidRuntimeException: Exception of type 'Android.Util.AndroidRuntimeException' was thrown.
E/mono    (11207):   at Android.Runtime.JNIEnv.CallVoidMethod (IntPtr jobject, IntPtr jmethod, Android.Runtime.JValue[] parms)
E/mono    (11207):   at Android.Widget.TextView.set_Text (IEnumerable`1 value)
E/mono    (11207):   at MonoDroidDebugging.Activity1.SlowMethod ()
```

Dies liegt daran müssen Sie die GUI aus der GUI-Thread aktualisieren. Unsere Code aktualisiert die GUI aus dem ThreadPool-Threads, die app zum Absturz verursacht. Müssen wir unsere Wert im Hintergrundthread zu berechnen, aber führen Sie dann auf unserem Update im GUI-Thread, der behandelt wird, mit [Activity.RunOnUIThread](https://developer.xamarin.com/api/member/Android.App.Activity.RunOnUiThread/(System.Action)):

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

Dieser Code funktioniert wie erwartet. Dieses GUI reaktionsfähig bleibt und ruft ordnungsgemäß aktualisiert, sobald die Berechnung komple ist.

Beachten Sie, dass diese Technik nicht nur für die Berechnung eines teuren Wert verwendet wird. Es kann für eine lang dauernde Aufgabe verwendet werden, die im Hintergrund, z. B. einem Webdienstaufruf oder Herunterladen von Daten für das Internet ausgeführt werden können.
