---
title: UrhoSharp Android-Unterstützung
description: Bestimmte Android-Setup und Funktionen für UrhoSharp.
ms.prod: xamarin
ms.assetid: 8409BD81-B1A6-4F5D-AE11-6BBD3F7C6327
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: 8d6abdac258e7a5b66e12367751652c5769405b0
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2018
---
# <a name="urhosharp-android-support"></a>UrhoSharp Android-Unterstützung

_Bestimmte Android-Setup und Funktionen_

Während Urho eine portable Klassenbibliothek ist, und ermöglicht die gleiche API über die verschiedenen Plattform verwendet werden, für die Spiellogik Sie weiterhin Urho in Ihren jeweiligen Treiber, Plattform und in einigen Fällen zu initialisieren müssen, sollten Sie bestimmte Funktionen der Plattform nutzen .

In den nachfolgenden Seiten wird angenommen, dass `MyGame` ist eine Unterklasse von der `Application` Klasse.

## <a name="architectures"></a>Architekturen

**Unterstützte Architekturen**: X86 Armeabi, Armeabi v7a

## <a name="create-a-project"></a>Erstellen eines Projekts

Erstellen eines Android-Projekts, und fügen Sie der UrhoSharp NuGet-Paket.

Hinzufügen von Daten, die Ihre Medienobjekte auf enthält die **Bestand** Verzeichnis und stellen Sie sicher, dass alle Dateien haben **AndroidAsset** als die **Buildvorgang**.

![Projekt Setup](android-images/image-3.png "Daten hinzufügen, enthält die Ressourcen in das Verzeichnis Bestand")

## <a name="configure-and-launching-urho"></a>Konfigurieren und starten Sie Urho

Hinzufügen von using-Anweisungen für die `Urho` und `Urho.Android` Namespaces, und fügen Sie diesen Code für das Initialisieren von Urho sowie das Starten der Anwendungsstatus hinzu.

Die einfachste Möglichkeit, führen Sie ein Spiel, wie in der MyGame-Klasse implementiert wird, aufgerufen

```csharp
UrhoSurface.RunInActivity<MyGame>();
```

Dadurch wird eine Vollbild-Aktivität mit dem Spiel als ein Inhalt geöffnet.

## <a name="custom-embedding-of-urho"></a>Benutzerdefinierte Einbetten von Urho

Sie können auch mit dem Fehlen eines Urho werden über den Bildschirm für die gesamte Anwendung, und um es als eine Komponente der Anwendung verwenden, erstellen Sie eine `SurfaceView` über:

```csharp
var surface = UrhoSurface.CreateSurface<MyGame>(activity)
```

Sie müssen auch einiger von Ihnen Aktivität UrhoSharp, Weiterleiten von Ereignissen an z. B.:

```csharp
protected override void OnPause()
{
    UrhoSurface.OnPause();
    base.OnPause();
}
```

Sie müssen die gleiche für: `OnResume`, `OnPause`, `OnLowMemory`, `OnDestroy`, `DispatchKeyEvent` und `OnWindowFocusChanged`.

Dies zeigt eine typischen Aktivität, die das Spiel startet:

```csharp
[Activity(Label = "MyUrhoApp", MainLauncher = true,
    Icon = "@drawable/icon", Theme = "@android:style/Theme.NoTitleBar.Fullscreen",
    ConfigurationChanges = ConfigChanges.KeyboardHidden | ConfigChanges.Orientation,
    ScreenOrientation = ScreenOrientation.Portrait)]
public class MainActivity : Activity
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        var mLayout = new AbsoluteLayout(this);
        var surface = UrhoSurface.CreateSurface<MyUrhoApp>(this);
        mLayout.AddView(surface);
        SetContentView(mLayout);
    }

    protected override void OnResume()
    {
        UrhoSurface.OnResume();
        base.OnResume();
    }

    protected override void OnPause()
    {
        UrhoSurface.OnPause();
        base.OnPause();
    }

    public override void OnLowMemory()
    {
        UrhoSurface.OnLowMemory();
        base.OnLowMemory();
    }

    protected override void OnDestroy()
    {
        UrhoSurface.OnDestroy();
        base.OnDestroy();
    }

    public override bool DispatchKeyEvent(KeyEvent e)
    {
        if (!UrhoSurface.DispatchKeyEvent(e))
            return false;
        return base.DispatchKeyEvent(e);
    }

    public override void OnWindowFocusChanged(bool hasFocus)
    {
        UrhoSurface.OnWindowFocusChanged(hasFocus);
        base.OnWindowFocusChanged(hasFocus);
    }
}
```

