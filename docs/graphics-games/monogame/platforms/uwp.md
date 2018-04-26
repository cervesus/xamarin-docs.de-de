---
title: Erstellen eines MonoGame UWP-Projekts
description: MonoGame kann zum Erstellen von Spielen und apps für universelle Windows-Plattform, abzielt, die mehrere Geräte mit einer Codebasis und einen Satz von Inhalten verwendet werden.
ms.prod: xamarin
ms.assetid: C6B99E44-00C1-4139-A1B7-FCFBE8749AB1
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: e28823165188d1046142e31490967367d3246422
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/26/2018
---
# <a name="creating-a-monogame-uwp-project"></a>Erstellen eines MonoGame UWP-Projekts

_MonoGame kann zum Erstellen von Spielen und apps für universelle Windows-Plattform, abzielt, die mehrere Geräte mit einer Codebasis und einen Satz von Inhalten verwendet werden._

In dieser exemplarischen Vorgehensweise werden die projekterstellung MonoGame universelle Windows-Plattform (UWP) und Laden von Inhalt behandelt. Uwp-apps können auf alle Windows 10-Geräte, einschließlich Desktops, Tablets, Windows-Telefone und Xbox One ausführen.

In dieser exemplarischen Vorgehensweise erstellt ein leeres Projekt angezeigt ein *Cornflower blauen* Hintergrund (die herkömmlichen Hintergrundfarbe von XNA-apps).

## <a name="requirements"></a>Anforderungen

Entwickeln von MonoGame UWP-apps benötigen Sie Folgendes:

- Windows 10-Betriebssystem
- Eine beliebige Version von Visual Studio 2015
- Windows 10-Entwicklertools
- Einstellung Gerät Entwicklermodus
- [MonoGame 3.5 für Visual Studio](http://www.monogame.net/2016/03/17/monogame-3-5/) oder höher

Weitere Informationen finden Sie in diesem [Seite einrichten für Windows 10-UWP-Entwicklung](https://msdn.microsoft.com/windows/uwp/get-started/get-set-up).

Xbox One Spiele können auf Xbox One Hardware Retail entwickelt werden. Für die Entwicklung von PCs und Xbox One ist zusätzliche Software erforderlich. Informationen zum Konfigurieren einer Xbox One für 3D-Spielentwicklung, finden Sie auf dieser Seite [Einrichten einer Xbox One](https://msdn.microsoft.com/windows/uwp/xbox-apps/index).

## <a name="creating-an-empty-template"></a>Erstellen eine leere Vorlage

Nachdem alle erforderliche Ressourcen installiert wurden und Entwicklermodus auf dem Windows 10-Computer aktiviert wurde, können wir ein neues MonoGame-Projekt mit Visual Studio folgendermaßen erstellen:

1. Wählen Sie **Datei** > **neue** > **Projekt...**
1. Wählen Sie die **installiert** > **Vorlagen** > **Visual C#-** > **MonoGame** Kategorie: 

    ![](uwp-images/image1.png "MonoGame-Kategorie")

1. Wählen Sie die **universelles MonoGame Windows 10-Projekt** Option: 

    ![](uwp-images/image2.png "Wählen Sie die Option universelles MonoGame Windows 10-Projekt")

1. Geben Sie einen Namen für das neue Projekt, und klicken Sie auf **OK**.
Wenn Visual Studio keine Fehler angezeigt, nach dem Klicken auf OK, stellen Sie sicher, dass Windows 10-Tools installiert sind und dass das Gerät im Entwicklermodus befindet.

Sobald Visual Studio abgeschlossen ist, die Vorlage erstellen, können wir ihn Beiträgen des leeren Projekts ausführen ausführen:

![](uwp-images/image3.png "Sobald Visual Studio abgeschlossen ist, die Vorlage erstellen, führen Sie es zum des leeren Projekts ausführen finden Sie unter")

Die Zahlen in den Ecken Bereitstellen von Diagnoseinformationen. Diese Informationen kann entfernt werden, löschen Sie den Code in `App.xaml.cs` in der `DEBUG` -block in der `OnLaunched` Methode:


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

## <a name="running-on-xbox-one"></a>Auf Xbox One ausgeführt wird

Uwp-Projekte können auf jedem Windows 10-Gerät aus demselben Projekt bereitstellen. Nach dem Einrichten von Windows 10-Entwicklungscomputer und Xbox One, können uwp-apps bereitgestellt werden, wechseln das Ziel auf dem Remotecomputer, und geben die Xbox One IP-Adresse:

![](uwp-images/remote.png "Uwp-apps können bereitgestellt werden, wechseln das Ziel auf dem Remotecomputer, und geben die Xbox diejenigen IP-Adresse")

Auf Xbox One repräsentiert den weiße Rahmen Bereich nicht sichere für Fernsehgeräte. Weitere Informationen finden Sie unter der [abgesicherten Bereich Abschnitt](#Safe_Area_on_Xbox_One).

![](uwp-images/safearea.png "Auf Xbox One stellt weißen Rahmens Bereich nicht sichere für Fernseher")

### <a name="safe-area-on-xbox-one"></a>Auf Xbox One abgesicherten Bereich

Entwickeln von Spielen für Konsolen erfordert die Berücksichtigung der abgesicherten Bereichs einen Bereich in der Mitte des Bildschirms ist, die alle kritischen visuellen Elemente (z. B. UI oder HUD) enthalten soll. Der Bereich außerhalb des sicheren Bereichs ist nicht unbedingt auf alle Fernsehgeräte sichtbar sein, sodass Visualisierungen, die in diesem Bereich platziert teilweise oder vollständig unsichtbar für einige zeigt möglicherweise.

Die Vorlage MonoGame für Xbox One abgesicherten Bereich berücksichtigt und rendert ihn als einen weißen Rahmen. Dieser Bereich wird auch zur Laufzeit in des Spiels wiedergegeben `Window.ClientBounds` Eigenschaft wie in dieser Abbildung Überwachungsfenster in Visual Studio dargestellt. Beachten Sie, dass die Höhe der Begrenzungen Client 1016, ungeachtet der Auflösung von 1920 x 1080 ist:

![](uwp-images/clientbounds.png "Beachten Sie, dass die Höhe der Begrenzungen Client 1016, ungeachtet der Auflösung von 1920 x 1080 ist")

## <a name="referencing-content-in-uwp-projects"></a>Verweisen auf Inhalt in uwp-Projekten

Inhalt in Projekten MonoGame verwiesen werden kann, direkt aus der Datei oder über die [MonoGame Content Pipeline](~/graphics-games/cocossharp/content-pipeline/index.md). Kleine Spiel Projekte profitieren möglicherweise von der Einfachheit beim Laden einer Datei. Größere Projekte profitieren von der Verwendung der Content-Pipeline zur Optimierung von Inhalten zur Reduzierung der Größe und Ladezeiten. Im Gegensatz zu XNA auf der Xbox 360 die `System.IO.File` Klasse auf eine Xbox-uwp-apps verfügbar ist.

Weitere Informationen zum Laden von Inhalt mithilfe der Content-Pipeline finden Sie unter der [Inhalt Pipeline Handbuch](~/graphics-games/cocossharp/content-pipeline/index.md). 

### <a name="loading-content-from-file"></a>Laden des Inhalts aus der Datei

Im Gegensatz zu iOS und Android können uwp-Projekte, Dateien relativ zu der ausführbaren Datei verweisen. Einfache Spiele können diese Technik laden Inhalt ohne ändern und erstellen Sie das pipelineprojekt Content.

Beim Laden einer `Texture2D` aus Datei:

1. Fügen Sie eine PNG-Datei in den Inhaltsordner in uwp-Projekt hinzu. Zum Hinzufügen von Inhalten in den Inhaltsordner ist eine Konvention in XNA und MonoGame.
1. Mit der rechten Maustaste auf die neu hinzugefügten PNG-Datei, und wählen Sie Eigenschaften aus.
1. Ändern der **in Ausgabeverzeichnis kopieren** auf **kopieren, wenn neuer**.
1. Fügen Sie den folgenden Code, um das Spiel Initialize-Methode zum Laden einer `Texture2D`:

    ```csharp
    Texture2D texture;
    using (var stream = System.IO.File.OpenRead("Content/YourPngName.png"))
    {
        texture = Texture2D.FromStream(graphics.GraphicsDevice, stream);
    }
    ```

Weitere Informationen zur Verwendung einer `Texture2D`, finden Sie unter der [Intro MonoGame Handbuch](~/graphics-games/monogame/introduction/index.md).

## <a name="summary"></a>Zusammenfassung

Dieses Handbuch erläutert, wie ein neues uwp-Projekt und universelle Windows-Plattform spezifischen Überlegungen beim Laden von Dateien zu erstellen. Entwickler, die sich bei der Erstellung der vollständigen uwp-Spiele können erfahren Sie mehr über MonoGame in der [Einführung MonoGame Handbuch](~/graphics-games/monogame/introduction/index.md).