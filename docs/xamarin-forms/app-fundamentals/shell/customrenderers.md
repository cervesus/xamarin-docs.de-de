---
title: Benutzerdefinierte Xamarin.Forms-Shell-Renderer
description: Xamarin.Forms-Shell-Anwendungen sind über die von den verschiedenen Shell-Klassen bereitgestellten Eigenschaften und Methoden äußerst anpassbar. Allerdings ist es auch möglich, einen benutzerdefinierten Shell-Renderer zu erstellen, wenn komplexere plattformspezifische Anpassungen erforderlich sind.
ms.prod: xamarin
ms.assetid: 3B1A6AE8-1D1E-4C34-B9AB-48F4444FEF32
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: ecb68d662c64b65346ffd04f0d3d3cd525533151
ms.sourcegitcommit: 6ad272c2c7b0c3c30e375ad17ce6296ac1ce72b2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/23/2019
ms.locfileid: "66178035"
---
# <a name="xamarinforms-shell-custom-renderers"></a>Benutzerdefinierte Xamarin.Forms-Shell-Renderer

Einer der Vorteile von Xamarin.Forms-Shell-Anwendungen besteht darin, dass ihr Aussehen und ihr Verhalten über die von den verschiedenen Shell-Klassen bereitgestellten Eigenschaften und Methoden äußerst anpassbar ist. Allerdings ist es auch möglich, einen benutzerdefinierten Shell-Renderer zu erstellen, wenn komplexere plattformspezifische Anpassungen erforderlich sind. Wie bei anderen benutzerdefinierten Renderern können Sie einen benutzerdefinierten Shell-Renderer zu nur einem Plattformprojekt hinzufügen, um Aussehen und Verhalten anzupassen, während auf der anderen Plattform das Standardverhalten möglich ist. Oder Sie fügen zu jedem Plattformprojekt einen anderen benutzerdefinierten Shell-Renderer hinzu, um Aussehen und Verhalten unter iOS und Android anzupassen.

Shell-Anwendungen werden mit der `ShellRenderer`-Klasse unter iOS und Android gerendert. Unter iOS finden Sie die `ShellRenderer`-Klasse im `Xamarin.Forms.Platform.iOS`-Namespace. Unter Android finden Sie die `ShellRenderer`-Klasse im `Xamarin.Forms.Platform.Android`-Namespace.

Der Prozess zur Erstellung eines benutzerdefinierten Shell-Renderers sieht folgendermaßen aus:

1. Erstellen Sie Unterklassen für die `Shell`-Klasse. Dies wird bereits in Ihrer Shell-Anwendung durchgeführt.
1. Verarbeiten Sie die `Shell`-Klasse mit Unterklassen. Dies wird bereits in Ihrer Shell-Anwendung durchgeführt.
1. Erstellen Sie auf den erforderlichen Plattformen eine benutzerdefinierte Rendererklasse, die von der `ShellRenderer`-Klasse abgeleitet ist.

## <a name="create-a-custom-renderer-class"></a>Erstellen einer benutzerdefinierten Renderer-Klasse

Gehen Sie folgendermaßen vor, um eine Klasse für einen benutzerdefinierten Shell-Renderer zu erstellen:

1. Erstellen Sie eine Unterklasse der `ShellRenderer`-Klasse.
1. Überschreiben Sie die erforderlichen Methoden, um die erforderliche Anpassung durchzuführen.
1. Fügen Sie ein `ExportRendererAttribute`-Objekt zur `ShellRenderer`-Unterklasse hinzu, um anzugeben, dass es zum Rendern der Shell-Anwendung verwendet wird. Dieses Attribut wird verwendet, um den benutzerdefinierten Renderer bei Xamarin.Forms zu registrieren.

> [!NOTE]
> Optional kann ein benutzerdefinierter Shell-Renderer in jedem Plattformprojekt bereitgestellt werden. Wenn kein benutzerdefinierter Renderer registriert wurde, wird die `ShellRenderer`-Standardklasse verwendet.

Die `ShellRenderer`-Klasse stellt die folgenden überschreibbaren Methoden zur Verfügung:

| iOS | Android |
| --- | --- |
| `SetElementSize`<br />`CreateFlyoutRenderer`<br />`CreateNavBarAppearanceTracker`<br />`CreatePageRendererTracker`<br />`CreateShellFlyoutContentRenderer`<br />`CreateShellItemRenderer`<br />`CreateShellItemTransition`<br />`CreateShellSearchResultsRenderer`<br />`CreateShellSectionRenderer`<br />`CreateTabBarAppearanceTracker`<br />`Dispose`<br />`OnCurrentItemChanged`<br />`OnElementPropertyChanged`<br />`OnElementSet`<br />`UpdateBackgroundColor` | `CreateFragmentForPage`<br />`CreateShellFlyoutContentRenderer`<br />`CreateShellFlyoutRenderer`<br />`CreateShellItemRenderer`<br />`CreateShellSectionRenderer`<br />`CreateTrackerForToolbar`<br />`CreateToolbarAppearanceTracker`<br />`CreateTabLayoutAppearanceTracker`<br />`CreateBottomNavViewAppearanceTracker`<br />`OnElementPropertyChanged`<br />`OnElementSet`<br />`SwitchFragment`<br />`Dispose` |

Die Klassen `FlyoutItem` und `TabBar` sind Aliase für die `ShellItem`-Klasse, und die `Tab`-Klasse ist ein Alias für die `ShellSection`-Klasse. Aus diesem Grund sollte die `CreateShellItemRenderer`-Methode beim Erstellen eines benutzerdefinierten Renderers für `FlyoutItem`-Objekte die `CreateShellSectionRenderer`-Methode beim Erstellen eines benutzerdefinierten Renderers für `Tab`-Objekte überschrieben werden.

> [!IMPORTANT]
> Es gibt zusätzliche Klassen für Shell-Renderer, z.B. `ShellSectionRenderer` und `ShellItemRenderer` unter iOS und Android. Diese zusätzlichen Rendererklassen werden jedoch durch Überschreibungen in der Klasse `ShellRenderer` erzeugt. Daher kann das Anpassen des Verhaltens dieser zusätzlichen Rendererklassen erreicht werden, indem sie in Unterklassen aufgeteilt werden und eine Instanz der Unterklasse in der entsprechenden Überschreibung in der in Unterklassen aufgeteilten `ShellRenderer`-Klasse erzeugt wird.

### <a name="ios-example"></a>iOS-Beispiel

Das folgende Codebeispiel zeigt ein in Unterklassen aufgeteiltes `ShellRenderer`-Objekt für iOS, das ein Hintergrundbild für die Navigationsleiste der Shell-Anwendung festlegt:

```csharp
using UIKit;
using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;

[assembly: ExportRenderer(typeof(Xaminals.AppShell), typeof(Xaminals.iOS.MyShellRenderer))]
namespace Xaminals.iOS
{
    public class MyShellRenderer : ShellRenderer
    {
        protected override IShellSectionRenderer CreateShellSectionRenderer(ShellSection shellSection)
        {
            var renderer = base.CreateShellSectionRenderer(shellSection);
            if (renderer != null)
            {
                (renderer as ShellSectionRenderer).NavigationBar.SetBackgroundImage(UIImage.FromFile("monkey.png"), UIBarMetrics.Default);
            }
            return renderer;
        }
    }
}
```

Die `MyShellRenderer`-Klasse überschreibt die `CreateShellSectionRenderer`-Methode und ruft den von der Basisklasse erzeugten Renderer ab. Anschließend wird der Renderer durch Festlegen eines Hintergrundbilds für die Navigationsleiste geändert, bevor der Renderer zurückgegeben wird.

### <a name="android-example"></a>Android-Beispiel

Das folgende Codebeispiel zeigt ein in Unterklassen aufgeteiltes `ShellRenderer`-Objekt für Android, das ein Hintergrundbild für die Navigationsleiste der Shell-Anwendung festlegt:

```csharp
using Android.Content;
using Xamarin.Forms;
using Xamarin.Forms.Platform.Android;

[assembly: ExportRenderer(typeof(Xaminals.AppShell), typeof(Xaminals.Droid.MyShellRenderer))]
namespace Xaminals.Droid
{
    public class MyShellRenderer : ShellRenderer
    {
        public MyShellRenderer(Context context) : base(context)
        {
        }

        protected override IShellToolbarAppearanceTracker CreateToolbarAppearanceTracker()
        {
            return new MyShellToolbarAppearanceTracker(this);
        }
    }
}
```

Die `MyShellRenderer`-Klasse überschreibt die `CreateToolbarAppearanceTracker`-Methode, und gibt eine Instanz der `MyShellToolbarAppearanceTracker`-Klasse zurück. Im folgenden Beispiel wird die `MyShellToolbarAppearanceTracker`-Klasse veranschaulicht, die von der `ShellToolbarAppearanceTracker`-Klasse abgeleitet wird:

```csharp
using Android.Support.V7.Widget;
using Xamarin.Forms;
using Xamarin.Forms.Platform.Android;

namespace Xaminals.Droid
{
    public class MyShellToolbarAppearanceTracker : ShellToolbarAppearanceTracker
    {
        public MyShellToolbarAppearanceTracker(IShellContext context) : base(context)
        {
        }

        public override void SetAppearance(Toolbar toolbar, IShellToolbarTracker toolbarTracker, ShellAppearance appearance)
        {
            base.SetAppearance(toolbar, toolbarTracker, appearance);
            toolbar.SetBackgroundResource(Resource.Drawable.monkey);
        }
    }
}
```

Die `MyShellToolbarAppearanceTracker`-Klasse überschreibt die `SetAppearance`-Methode und ändert die Symbolleiste, indem sie ein Hintergrundbild dafür festlegt.

> [!IMPORTANT]
> Die `ExportRendererAttribute`-Klasse muss nur einem benutzerdefinierten Renderer hinzugefügt werden, der von der `ShellRenderer`-Klasse abgeleitet ist. Zusätzliche Shell-Rendererklassen mit Unterklassen werden von der in Unterklassen unterteilten `ShellRenderer`-Klasse erstellt.

## <a name="related-links"></a>Verwandte Links

- [Benutzerdefinierte Xamarin.Forms-Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
