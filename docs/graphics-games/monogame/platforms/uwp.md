---
title: Erstellen eines monogame-UWP-Projekts
description: Monogame kann verwendet werden, um Spiele und Apps für universelle Windows-Plattform zu erstellen, die auf mehrere Geräte mit einer Codebasis und einem Satz von Inhalten abzielen.
ms.prod: xamarin
ms.assetid: C6B99E44-00C1-4139-A1B7-FCFBE8749AB1
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 367f44104b0d6049b8d65ab6e3b3f38c703cdb3f
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2019
ms.locfileid: "70198711"
---
# <a name="creating-a-monogame-uwp-project"></a>Erstellen eines monogame-UWP-Projekts

_Monogame kann verwendet werden, um Spiele und Apps für universelle Windows-Plattform zu erstellen, die auf mehrere Geräte mit einer Codebasis und einem Satz von Inhalten abzielen._

In dieser exemplarischen Vorgehensweise werden die Projekt Erstellung für monogame universelle Windows-Plattform (UWP) und das Laden von Inhalten behandelt UWP-Apps können auf allen Windows 10-Geräten, einschließlich Desktops, Tablets, Windows Phone und Xbox One, ausgeführt werden.

In dieser exemplarischen Vorgehensweise wird ein leeres Projekt erstellt, das einen *blaublumen blauen* Hintergrund (die herkömmliche Hintergrundfarbe von XNA-Apps) anzeigt.

## <a name="requirements"></a>Anforderungen

Die Entwicklung von monogame UWP-apps erfordert Folgendes:

- Windows 10-Betriebssystem
- Beliebige Version von Visual Studio 2017
- Windows 10-Entwicklertools
- Festlegen des Geräts auf den Entwicklermodus
- [Monogame 3.7.1 für Visual Studio](http://community.monogame.net/t/monogame-3-7-1-release/11173) oder neuer

Weitere Informationen finden [Sie auf dieser Seite zum Einrichten der Windows 10-UWP-Entwicklung](https://msdn.microsoft.com/windows/uwp/get-started/get-set-up).

Xbox One-Spiele können auf der Einzelhandel-Xbox One-Hardware entwickelt werden. Auf dem Entwicklungs Computer und der Xbox One ist zusätzliche Software erforderlich. Informationen zum Konfigurieren einer Xbox One für die Spieleentwicklung finden Sie auf dieser Seite zum [Einrichten einer Xbox One](https://msdn.microsoft.com/windows/uwp/xbox-apps/index).

## <a name="creating-an-empty-template"></a>Erstellen einer leeren Vorlage

Nachdem alle erforderlichen Ressourcen installiert wurden und der Entwicklermodus auf dem Windows 10-Computer aktiviert wurde, können Sie ein neues monogame-Projekt mithilfe von Visual Studio erstellen, indem Sie die folgenden Schritte ausführen:

1. Wählen Sie **Datei** > **neu** > **Projekt... aus.**
1. Wählen Sie >  > die**Visual C#**  monogame-KategorieinstallierteVorlagen > aus:

    ![](uwp-images/image1.png "Monogame-Kategorie")

1. Wählen Sie die Option **monogame Windows 10 Universal Project** aus:

    ![](uwp-images/image2.png "Wählen Sie die Option monogame Windows 10 Universal Project aus.")

1. Geben Sie einen Namen für das neue Projekt ein, und klicken Sie auf **OK**.
Wenn Visual Studio nach dem Klicken auf "OK" Fehler anzeigt, vergewissern Sie sich, dass die Windows 10-Tools installiert sind und dass sich das Gerät im Entwicklermodus befindet.

Nachdem Visual Studio die Erstellung der Vorlage abgeschlossen hat, können wir Sie ausführen, damit das leere Projekt ausgeführt wird:

![](uwp-images/image3.png "Nachdem Visual Studio die Erstellung der Vorlage abgeschlossen hat, führen Sie Sie aus, um das leere Projekt zu sehen.")

Die Zahlen in den Ecken stellen Diagnoseinformationen bereit. Diese Informationen können entfernt werden, indem Sie den Code `App.xaml.cs` im `DEBUG` -Block in der `OnLaunched` -Methode löschen:


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

## <a name="running-on-xbox-one"></a>Ausführung auf Xbox One

UWP-Projekte können auf jedem Windows 10-Gerät über das gleiche Projekt bereitgestellt werden. Nachdem Sie den Windows 10-Entwicklungs Computer und die Xbox One eingerichtet haben, können Sie UWP-apps bereitstellen, indem Sie das Ziel auf den Remote Computer umstellen und die IP-Adresse der Xbox One eingeben:

![](uwp-images/remote.png "UWP-Apps können bereitgestellt werden, indem Sie das Ziel auf den Remote Computer umstellen und die Xbox-IP-Adresse eingeben.")

Auf Xbox One stellt der weiße Rahmen den nicht sicheren Bereich für Fernsehgeräte dar. Weitere Informationen finden Sie im [Abschnitt sicherer Bereich](#safe-area-on-xbox-one).

![](uwp-images/safearea.png "Auf Xbox One stellt der weiße Rahmen den nicht sicheren Bereich für Fernsehgeräte dar.")

### <a name="safe-area-on-xbox-one"></a>Sicherer Bereich auf Xbox One

Die Entwicklung von Spielen für Konsolen erfordert die Berücksichtigung des sicheren Bereichs, bei dem es sich um einen Bereich in der Mitte des Bildschirms handelt, der alle kritischen visuellen Elemente (z. b. UI oder HUD) enthalten sollte. Es ist nicht garantiert, dass der Bereich außerhalb des sicheren Bereichs auf allen Fernsehgeräten sichtbar ist, sodass die in diesem Bereich platzierten visuellen Elemente in manchen anzeigen möglicherweise teilweise oder vollständig unsichtbar sind.

Die monogame-Vorlage für Xbox One berücksichtigt den sicheren Bereich und rendert ihn als weißen Rahmen. Dieser Bereich wird auch zur Laufzeit in der- `Window.ClientBounds` Eigenschaft des Spiels reflektiert, wie in dieser Abbildung des Fensters Überwachen in Visual Studio gezeigt. Beachten Sie, dass die Höhe der Client Begrenzungen trotz der Anzeige Auflösung von 1920 x 1080 1016 ist:

![](uwp-images/clientbounds.png "Beachten Sie, dass die Höhe der Client Begrenzungen trotz der Anzeige Auflösung von 1920 x 1080 1016 ist.")

## <a name="referencing-content-in-uwp-projects"></a>Verweisen auf Inhalte in UWP-Projekten

Auf Inhalte in monogame-Projekten kann direkt aus einer Datei oder über die [monogame-Inhalts Pipeline](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/content-pipeline/introduction.md)verwiesen werden. Kleine Spielprojekte können von der Einfachheit des Ladens aus einer Datei profitieren. Größere Projekte profitieren von der Verwendung der Inhalts Pipeline zum Optimieren von Inhalten, um die Größe und Ladezeiten zu verringern. Anders als XNA auf der Xbox 360 ist `System.IO.File` die-Klasse auf Xbox One UWP-apps verfügbar.

Weitere Informationen zum Laden von Inhalten mithilfe der Inhalts Pipeline finden Sie im [Leitfaden zur Inhalts Pipeline](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/content-pipeline/introduction.md).

### <a name="loading-content-from-file"></a>Inhalt wird aus einer Datei geladen.

Im Gegensatz zu IOS und Android können UWP-Projekte auf Dateien verweisen, die relativ zur ausführbaren Datei sind. Einfache Spiele können diese Technik zum Laden von Inhalten verwenden, ohne das Inhalts Pipelineprojekt ändern und erstellen zu müssen.

So laden Sie `Texture2D` eine aus einer Datei:

1. Fügen Sie dem Inhalts Ordner im UWP-Projekt eine PNG-Datei hinzu. Das Hinzufügen von Inhalt zum Inhalts Ordner ist eine Konvention in XNA und monogame.
1. Klicken Sie mit der rechten Maustaste auf das neu hinzugefügte PNG und wählen Sie Eigenschaften aus.
1. Ändern Sie das **Verzeichnis in Ausgabeverzeichnis kopieren** auf **kopieren, wenn neuer**.
1. Fügen Sie den folgenden Code in die Initialize-Methode Ihres Spiels ein `Texture2D`, um einen zu laden:

    ```csharp
    Texture2D texture;
    using (var stream = System.IO.File.OpenRead("Content/YourPngName.png"))
    {
        texture = Texture2D.FromStream(graphics.GraphicsDevice, stream);
    }
    ```

Weitere Informationen zur Verwendung von finden `Texture2D`Sie im [Handbuch Einführung in monogame](~/graphics-games/monogame/introduction/index.md).

## <a name="summary"></a>Zusammenfassung

In dieser Anleitung wird erläutert, wie ein neues UWP-Projekt und UWP-spezifische Überlegungen beim Laden von Dateien erstellt werden. Entwickler, die an der Erstellung von vollständigen UWP-spielen interessiert sind, können sich im [Leitfaden Einführung in monogame](~/graphics-games/monogame/introduction/index.md)ausführlicher über monogame informieren.
