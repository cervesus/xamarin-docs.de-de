---
title: Schreiben von reaktionsfähigen Anwendungen
ms.prod: xamarin
ms.assetid: 452DF940-6331-55F0-D130-002822BBED55
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/15/2018
ms.openlocfilehash: e3f7d788e71718f4ca1336b7906cf3d63bf07f32
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68509196"
---
# <a name="writing-responsive-applications"></a>Schreiben von reaktionsfähigen Anwendungen

Einer der Schlüssel zum Verwalten einer reaktionsfähigen GUI ist das Ausführen von Aufgaben mit langer Ausführungszeit in einem Hintergrund Thread, damit die GUI nicht blockiert wird. Nehmen wir an, wir möchten einen Wert berechnen, der dem Benutzer angezeigt wird, aber dieser Wert benötigt 5 Sekunden für die Berechnung:

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

Dies funktioniert, aber die Anwendung bleibt 5 Sekunden lang hängen, während der Wert berechnet wird. Während dieser Zeit antwortet die APP nicht auf eine Benutzerinteraktion. Um dies zu umgehen, möchten wir unsere Berechnungen in einem Hintergrund Thread durchführen:

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

Nun berechnen wir den Wert in einem Hintergrund Thread, damit unsere GUI während der Berechnung reaktionsfähig bleibt. Wenn die Berechnung jedoch abgeschlossen ist, stürzt unsere App ab und verlässt Sie im Protokoll:

```shell
E/mono    (11207): EXCEPTION handling: Android.Util.AndroidRuntimeException: Exception of type 'Android.Util.AndroidRuntimeException' was thrown.
E/mono    (11207):
E/mono    (11207): Unhandled Exception: Android.Util.AndroidRuntimeException: Exception of type 'Android.Util.AndroidRuntimeException' was thrown.
E/mono    (11207):   at Android.Runtime.JNIEnv.CallVoidMethod (IntPtr jobject, IntPtr jmethod, Android.Runtime.JValue[] parms)
E/mono    (11207):   at Android.Widget.TextView.set_Text (IEnumerable`1 value)
E/mono    (11207):   at MonoDroidDebugging.Activity1.SlowMethod ()
```

Dies liegt daran, dass Sie die GUI über den GUI-Thread aktualisieren müssen. Der Code aktualisiert die GUI aus dem Thread Pool-Thread, wodurch die APP abstürzt. Wir müssen unseren Wert im Hintergrund Thread berechnen, aber dann das Update auf dem GUI-Thread durchführen, der mit " [Activity. runonuithread](xref:Android.App.Activity.RunOnUiThread*)" behandelt wird:

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

Dieser Code funktioniert erwartungsgemäß. Diese GUI bleibt reaktionsfähig und wird nach dem Berechnen der Berechnung ordnungsgemäß aktualisiert.

Beachten Sie, dass diese Technik nicht nur zum Berechnen eines teuren Werts verwendet wird. Sie kann für alle Aufgaben mit langer Ausführungszeit verwendet werden, die im Hintergrund ausgeführt werden können, wie z. b. ein Webdienst oder das Herunterladen von Internetdaten.
