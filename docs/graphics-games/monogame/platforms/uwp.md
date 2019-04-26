---
title: Erstellen eines MonoGame UWP-Projekts
description: MonoGame kann zum Erstellen von Spielen und apps für universelle Windows-Plattform als Ziel, die mehrere Geräte mit einer Codebasis und einen Satz von Inhalten verwendet werden.
ms.prod: xamarin
ms.assetid: C6B99E44-00C1-4139-A1B7-FCFBE8749AB1
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 9f39580d282defed354f3b9e5cbe4eb1cdec4796
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61161146"
---
# <a name="creating-a-monogame-uwp-project"></a>Erstellen eines MonoGame UWP-Projekts

_MonoGame kann zum Erstellen von Spielen und apps für universelle Windows-Plattform als Ziel, die mehrere Geräte mit einer Codebasis und einen Satz von Inhalten verwendet werden._

In dieser exemplarischen Vorgehensweise werden die projekterstellung MonoGame universelle Windows-Plattform (UWP) und Laden von Inhalt behandelt. UWP-apps können auf alle Windows 10-Geräte, einschließlich Desktops, Tablets, Windows-Smartphones und Xbox One ausführen.

In dieser exemplarischen Vorgehensweise erstellt ein leeres Projekt angezeigt ein *Cornflower blaue* Hintergrund (die herkömmlichen Hintergrundfarbe der XNA-apps).

## <a name="requirements"></a>Anforderungen

Entwickeln von MonoGame UWP-apps erfordert:

- Windows 10-Betriebssystem
- Jede Version von Visual Studio 2017
- Windows 10-Entwicklertools
- Einstellung Gerät, um den Entwicklermodus
- [MonoGame 3.7.1 für Visual Studio](http://community.monogame.net/t/monogame-3-7-1-release/11173) oder höher

Weitere Informationen finden Sie in diesem [Seite zum Einrichten von Windows 10-UWP-Entwicklung](https://msdn.microsoft.com/windows/uwp/get-started/get-set-up).

Xbox One-Spiele können auf der Xbox One Retail-Hardware entwickelt werden. Für die Entwicklung von PC- und der Xbox One ist zusätzliche Software erforderlich. Informationen zum Konfigurieren eine Xbox One, für die Entwicklung von Spielen, finden Sie auf dieser Seite [zum Einrichten einer Xbox One](https://msdn.microsoft.com/windows/uwp/xbox-apps/index).

## <a name="creating-an-empty-template"></a>Erstellen einer leeren Vorlage

Sobald alle erforderliche Ressourcen installiert wurden und auf dem Windows 10-Computer den Entwicklermodus aktiviert wurde, können wir eine neue MonoGame-Projekts mit Visual Studio mit folgenden Schritten erstellen:

1. Wählen Sie **Datei** > **neue** > **Projekt...**
1. Wählen Sie die **installiert** > **Vorlagen** > **Visual C#**   >  **MonoGame** Kategorie:

    ![](uwp-images/image1.png "MonoGame-Kategorie")

1. Wählen Sie die **MonoGame Windows 10 Universal-Projekt** Option:

    ![](uwp-images/image2.png "Wählen Sie die Option MonoGame Windows 10 Universal-Projekt")

1. Geben Sie einen Namen für das neue Projekt, und klicken Sie auf **OK**.
Wenn Visual Studio keine Fehler angezeigt, nach dem Klicken auf OK, stellen Sie sicher, dass Windows 10-Tools installiert sind und das Gerät im Entwicklermodus befindet.

Sobald Visual Studio abgeschlossen ist, der die Vorlage erstellt, können wir sie, um das leere Projekt ausgeführt wird, finden Sie unter ausführen:

![](uwp-images/image3.png "Sobald Visual Studio abgeschlossen ist, der die Vorlage erstellt, führen Sie aus, um das leere Projekt ausgeführt wird, finden Sie unter")

Die Zahlen in den Ecken bieten Diagnoseinformationen. Diese Informationen entfernt werden kann, löschen Sie den Code in `App.xaml.cs` in die `DEBUG` -block in der `OnLaunched` Methode:


```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
#if DEBUG
    if (System.Diagnostics.Debugger.IsAttached)
    {
        this.DebugSettings.EnableFrameRateCounter = true;
    }
#endif
    ...
```

## <a name="running-on-xbox-one"></a>Auf der Xbox One

UWP-Projekte können aus demselben Projekt auf Windows 10-Geräten bereitstellen. Nach dem Festlegen der Windows 10-Entwicklungscomputer und der Xbox One, können UWP-apps bereitgestellt werden, wechseln Sie das Ziel, auf dem Remotecomputer und der Xbox One IP-Adresse eingeben:

![](uwp-images/remote.png "UWP-apps können bereitgestellt werden, wechseln Sie das Ziel auf Remotecomputer, und die Xbox zu IP-Adresse eingeben")

Für Xbox One repräsentiert den weiße Rahmen Bereich nicht threadsichere für TV-Geräte. Weitere Informationen finden Sie unter den [Sicherheitsbereich Abschnitt](#safe-area-on-xbox-one).

![](uwp-images/safearea.png "Für Xbox One stellt die weiße Rahmen Bereich nicht threadsichere für TV-Geräte")

### <a name="safe-area-on-xbox-one"></a>Sichere Bereich auf der Xbox One

Entwickeln von Spielen für Konsolen erfordert die Berücksichtigung des sicheren Bereichs, der einen Bereich in der Mitte des Bildschirms ist, die alle wichtigen Visuals (z.B. UI oder HUD) enthalten soll. Der Bereich außerhalb des sicheren Bereichs wird nicht unbedingt alle Fernseher angezeigt werden, daher teilweise oder vollständig unsichtbar für einige zeigt Visualisierungen, die in diesem Bereich abgelegt.

Die MonoGame-Vorlage für Xbox One der sicheren Bereich berücksichtigt und stellt sie als einem weißen Rahmen. Dieser Bereich wird auch zur Laufzeit in des Spiels wiedergegeben `Window.ClientBounds` Eigenschaft wie in dieser Abbildung des Überwachungsfensters in Visual Studio dargestellt. Beachten Sie, dass die Höhe der Client-Grenzen 1016, trotz einer Auflösung von 1920 x 1080 beträgt:

![](uwp-images/clientbounds.png "Beachten Sie, dass die Höhe der Begrenzungen Client 1016, trotz einer Auflösung von 1920 x 1080")

## <a name="referencing-content-in-uwp-projects"></a>Verweisen auf Inhalt in UWP-Projekten

Inhalte in MonoGame-Projekten verwiesen werden kann, direkt aus der Datei oder über die [Pipeline für Bildinhalte MonoGame](~/graphics-games/cocossharp/content-pipeline/index.md). Kleine-Spiel-Projekte profitieren möglicherweise von der Einfachheit beim Laden einer Datei. Größere Projekte profitieren von der Verwendung von der Pipeline für Bildinhalte zum Optimieren von Inhalten zu verkleinern und zu laden. Im Gegensatz zu XNA auf der Xbox 360 die `System.IO.File` -Klasse ist für Xbox One-UWP-apps verfügbar.

Weitere Informationen zum Laden von Inhalt mithilfe der Pipeline für Bildinhalte finden Sie unter den [Content Pipeline Handbuch](~/graphics-games/cocossharp/content-pipeline/index.md).

### <a name="loading-content-from-file"></a>Laden Inhalte aus Datei

Im Gegensatz zu iOS und Android können UWP-Projekte, Dateien im Verhältnis zur ausführbaren Datei verweisen. Einfache Spiele können diese Technik Load Inhalte ohne ändern und erstellen Sie das Projekt für die Pipeline für Bildinhalte.

Beim Laden einer `Texture2D` aus Datei:

1. Fügen Sie in den Inhaltsordner im UWP-Projekt eine PNG-Datei hinzu. Hinzufügen von Inhalten in den Inhaltsordner ist eine Konvention in XNA "und" MonoGame ".
1. Mit der rechten Maustaste auf die neu hinzugefügte PNG-Datei, und wählen Sie Eigenschaften.
1. Ändern der **in Ausgabeverzeichnis kopieren** zu **kopieren, wenn neuer**.
1. Fügen Sie den folgenden Code, um des Spiels "Initialize"-Methode zum Laden einer `Texture2D`:

    ```csharp
    Texture2D texture;
    using (var stream = System.IO.File.OpenRead("Content/YourPngName.png"))
    {
        texture = Texture2D.FromStream(graphics.GraphicsDevice, stream);
    }
    ```

Weitere Informationen zur Verwendung einer `Texture2D`, finden Sie unter den [Einführung in MonoGame-Handbuch](~/graphics-games/monogame/introduction/index.md).

## <a name="summary"></a>Zusammenfassung

Dieser Leitfaden wird beschrieben, wie Sie ein neues UWP-Projekt und UWP-spezifische Überlegungen zu erstellen, beim Laden von Dateien. Entwickler, die vollständige UWP-Spiele erstellen möchten, sind erfahren Sie mehr über MonoGame in die [Einführung in MonoGame-Handbuch](~/graphics-games/monogame/introduction/index.md).
