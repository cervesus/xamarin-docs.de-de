---
title: Erstellen eines Multi-Plattform CocosSharp-Projekts
description: 'In dieser exemplarischen Vorgehensweise wird das Erstellen einer neuen Multi-Plattform CocosSharp Lösung veranschaulicht. Das Ergebnis dieser exemplarischen Vorgehensweise ist eine Visual Studio für Mac-Lösung umfasst drei Projekte: ein Projekt der portablen Bibliothek, ein Android-spezifische-Projekt und ein iOS-spezifische-Projekt. Das daraus resultierende Projekt wird einen leeren schwarzen Bildschirm bei der Ausführung angezeigt.'
ms.topic: article
ms.prod: xamarin
ms.assetid: 37C97693-B0A8-4064-97B6-A6FAB5BA4FB7
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 2906035ce9bd44d111b89ccfe7443896775315b7
ms.sourcegitcommit: 7b88081a979381094c771421253d8a388b2afc16
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2018
---
# <a name="creating-a-multi-platform-cocossharp-project"></a>Erstellen eines Multi-Plattform CocosSharp-Projekts

_In dieser exemplarischen Vorgehensweise wird das Erstellen einer neuen Multi-Plattform CocosSharp Lösung veranschaulicht. Das Ergebnis dieser exemplarischen Vorgehensweise ist eine Visual Studio für Mac-Lösung umfasst drei Projekte: ein Projekt der portablen Bibliothek, ein Android-spezifische-Projekt und ein iOS-spezifische-Projekt. Das daraus resultierende Projekt wird einen leeren schwarzen Bildschirm bei der Ausführung angezeigt._

Die CocosSharp 2D Spiele-Engine ermöglicht es, Code und der Inhalt auf mehreren Plattformen gemeinsam genutzt werden. Diese exemplarische Vorgehensweise zeigt, wie ein Projekt erstellen, die IOS- und Android-Entwicklung unterstützen kann. In dieser exemplarischen Vorgehensweise werden insbesondere in den folgenden Themen behandelt:

 - Installieren von CocosSharp
 - Eine neue Projektmappe erstellen
 - `LoadGame`-Methode

# <a name="installing-cocossharp"></a>Installieren von CocosSharp

Erstens fügen CocosSharp zu Visual Studio für Mac wir Wenn auf einem Mac ausgeführt wird, wählen **Visual Studio für Mac** > **Add-In-Manager...**  . Wenn unter Windows ausgeführt wird, wählen Sie **Tools** > **Add-In-Manager...**  . Klicken Sie auf die **Katalog** Registerkarte, erweitern Sie die **CocosSharp Element**Option **CocosSharp-Projektvorlagen**, und klicken Sie dann auf **installieren...**  .

![CocosSharp-Add-in](part1-images/xamarinstudioaddinsmac.png "")

# <a name="creating-a-new-solution"></a>Erstellen einer neuen Projektmappe

Nach der Installation CocosSharp, erstellen wir eine Lösung. Wählen Sie in Visual Studio für Mac **Datei** > **neu** > **Lösung...** . Wählen Sie die **App** Option "" unter der **plattformübergreifende** Abschnitt **CocosSharp leeres Projekt**, und klicken Sie dann auf **Weiter**:

![](part1-images/image1.png "Wählen Sie die App-Option im Abschnitt über Plattformen hinweg wählen Sie CocosSharp leeres Projekt aus, und klicken Sie dann auf Weiter")

Geben Sie den Namen **BouncingGame** für die **Projektname**, klicken Sie dann auf **erstellen**:

![](part1-images/image2.png "Geben Sie den Namen BouncingGame für den Projektnamen, und klicken Sie auf Erstellen")

Nachdem das Projekt erstellt wurde, und Visual Studio für Mac, die wir kompilieren und ausführen, um einen grauen Hintergrund anzeigen können: 

![](part1-images/image3.png "Nachdem das Projekt wurde erstellt und Visual Studio für Mac, kompilieren und ausführen, um einen grauen Hintergrund anzeigen")


# <a name="loadgame-method"></a>LoadGame-Methode

CocosSharp Standardprojekt enthält Klassen, die spezifisch für iOS und Android zum Einrichten einer `CCGameView`, der verwendet wird, CocosSharp starten. Die `CCGameView` Instanz auf eine Weise plattformspezifischen erstellt wird: das iOS-Projekt erstellt die `CCGameView` in der `Main.storyboard` Datei, während Android erstellt die `CCGameView` in der `Main.axml` Datei. Jede Plattform verwendet die CCGameView-Instanz in eine `LoadGame` Methode, die einige grundlegende-Setup führt. Obwohl wir dieser Code ändert, wird nicht, werfen wir einen Blick auf einige wichtige Details:

 - Der Code legt die `gameView.DesignResolution` auf 1024 x 768 Pixel. Dies standardisiert Positionierung auf Geräten unabhängig von der Seitenverhältnis, die physischen Auflösung oder die Ausrichtung des Geräts. 
 - Der Code fügt einige Suchpfade. Suchpfade können auch Inhalte ohne verzeichnispräfixe geladen werden. Angenommen, seit die `"Sounds"` Pfad wird als einem Suchpfad, und klicken Sie dann auf eine Datei im Verzeichnis hinzugefügt `"Content/Sounds/mysound.xnb"` einfach als geladen werden `"mysound.xnb"`. Suchpfade ähneln `using` Anweisungen in Code – Reduzierung der Code, aber sie können auch zu Mehrdeutigkeit führen.
 - Der Code erstellt ein `GameLayer` Instanz. `GameLayer` ist eine `CCLayer`-Klasse, in denen wir werden alle unsere Spiellogik hinzufügen werden, erben. Größere Spiele dauert möglicherweise mehrere `CCLayer` Instanzen oder sogar mehreren `CCScene` Instanzen (können auch mehrere enthalten `CCLayer` Instanzen), aber wir müssen auf einen einzelnen Speicherstick `CCLayer` für dieses Spiel.

#  <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise behandelt, wie eine plattformübergreifende CocosSharp-Projekt mit Visual Studio für Mac erstellt Das Ergebnis ist, einen leeren Bildschirm als Ausgangspunkt für jedes Spiel Projekt verwendet werden kann.