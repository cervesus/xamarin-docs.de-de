---
title: Von UrhoSharp Android-Unterstützung
description: Dieses Dokument beschreibt die Android-spezifische Setup und Feature-bezogene Informationen für die von UrhoSharp. Insbesondere erläutert unterstützten Architekturen, wie Sie ein Projekt konfiguriert und startet Urho und benutzerdefinierte Einbettung von Urho erstellen.
ms.prod: xamarin
ms.assetid: 8409BD81-B1A6-4F5D-AE11-6BBD3F7C6327
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: 3377602b8d793be8274ed150ebbd124c9d165e82
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67832489"
---
# <a name="urhosharp-android-support"></a>Von UrhoSharp Android-Unterstützung

_Android-spezifische Setup und Features_

Obwohl Urho eine portable Klassenbibliothek ist und ermöglicht die gleiche API für die verschiedenen Plattformen verwendet werden, für die Spiele-Logik, Sie dennoch Urho in Ihren jeweiligen Treiber, Plattform und in einigen Fällen zu initialisieren müssen, sollten Sie bestimmte Features der Plattform nutzen .

In den nachfolgenden Seiten wird angenommen, dass `MyGame` ist eine Unterklasse von der `Application` Klasse.

## <a name="architectures"></a>Architekturen

**Unterstützte Architekturen**: X86, Armeabi, Armeabi-v7a

## <a name="create-a-project"></a>Erstellen eines Projekts

Erstellen Sie ein Android-Projekt, und fügen Sie das UrhoSharp NuGet-Paket.

Hinzufügen von Daten, die mit Ihrer Ressourcen, die **Assets** Verzeichnis und stellen Sie sicher, dass alle Dateien haben **AndroidAsset** als die **Buildvorgang**.

![Setup Project](android-images/image-3.png "Hinzufügen von Daten, die die Objekte in das Verzeichnis für die Objekte enthält.")

## <a name="configure-and-launching-urho"></a>Konfigurieren und starten Sie Urho

Hinzufügen von using-Anweisungen für die `Urho` und `Urho.Android` Namespaces, und fügen Sie diesen Code für Urho initialisieren, sowie Ihre Anwendung zu starten.

Die einfachste Möglichkeit, ein Spiel, ausgeführt wird, wie in der MyGame-Klasse implementiert wird, aufgerufen

```csharp
UrhoSurface.RunInActivity<MyGame>();
```

Dadurch wird eine Aktivität im Vollbildmodus mit dem Spiel als ein Inhalt geöffnet.

## <a name="custom-embedding-of-urho"></a>Benutzerdefinierte Einbetten von Urho

Sie können auch mit dem Urho verwendet werden, den Bildschirm für die gesamte Anwendung, und um sie als eine Komponente Ihrer Anwendung verwenden, können Sie erstellen eine `SurfaceView` über:

```csharp
var surface = UrhoSurface.CreateSurface<MyGame>(activity)
```

Sie müssen auch einiger von Ihnen Aktivität von UrhoSharp, Weiterleiten von Ereignissen an z.B.:

```csharp
protected override void OnPause()
{
    UrhoSurface.OnPause();
    base.OnPause();
}
```

Sie müssen auch erreichen, für die: `OnResume`, `OnPause`, `OnLowMemory`, `OnDestroy`, `DispatchKeyEvent` und `OnWindowFocusChanged`.

Dies zeigt eine typische-Aktivität, die das Spiel startet:

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
