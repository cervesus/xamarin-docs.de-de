---
title: Übersicht über die plattformübergreifenden Anwendungen
description: Dieses Dokument enthält eine allgemeine Übersicht über das Entwickeln von plattformübergreifenden Anwendungen. Er erläutert den Wert von C#, Entwurfsmuster wie MVC/MVVM und Native Benutzeroberflächen.
ms.prod: xamarin
ms.assetid: E442EEFB-FA9C-40E9-9668-5A3F915C8400
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: 7d839e0141f14f4ba86897b128bf2a8c0a79548d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016913"
---
# <a name="building-cross-platform-applications-overview"></a>Übersicht über die plattformübergreifenden Anwendungen

Dieses Handbuch bietet eine Einführung in die xamarin-Plattform und erläutert, wie Sie eine plattformübergreifende Anwendung erstellen, um die Wiederverwendung von Code zu maximieren, und eine hochwertige Native Oberfläche für alle wichtigen mobilen Plattformen bereitzustellen: IOS, Android und Windows phone.

Der Ansatz, der in diesem Dokument verwendet wird, gilt in der Regel für Produktivitäts-apps und Spiele-Apps, der Schwerpunkt liegt jedoch auf der Produktivität und dem Hilfsprogramm (Anwendungen ohne Spiel). Weitere Informationen finden Sie [in der Einführung in das monogame-Dokument](~/graphics-games/monogame/introduction/index.md) oder unter [Visual Studio-Tools für Unity](https://docs.microsoft.com/visualstudio/cross-platform/visual-studio-tools-for-unity) für plattformübergreifende Spiele Entwicklungs Anleitungen.

Der Ausdruck "Write-Once, Run Everywhere" wird häufig verwendet, um die Vorzüge einer einzelnen Codebasis zu verfassen, die auf mehreren Plattformen unverändert ausgeführt wird. Dies hat den Vorteil, dass die Wiederverwendung von Code möglich ist. diese Vorgehensweise führt jedoch häufig zu Anwendungen, die eine featuremenge mit dem niedrigsten gemeinsamen Nenner und einer allgemein aussehenden Benutzeroberfläche haben, die sich nicht gut in eine der Zielplattformen einfügt.

Xamarin ist nicht nur eine "Write-Once, Run Everywhere"-Plattform, denn eine seiner Stärken ist die Möglichkeit, Native Benutzeroberflächen speziell für jede Plattform zu implementieren. Bei einem durchdachten Entwurf ist es jedoch immer noch möglich, den größten Teil des Codes der Nichtbenutzer Oberfläche zu nutzen und das Beste aus beiden Welten zu erstellen: Schreiben Sie Ihren Datenspeicher und Ihren Code für die Geschäftslogik einmal, und stellen Sie auf jeder Plattform Native Benutzeroberflächen dar. In diesem Dokument wird ein allgemeiner Architekturansatz erläutert, um dieses Ziel zu erreichen.

Hier finden Sie eine Zusammenfassung der wichtigsten Punkte zum Erstellen von plattformübergreifenden xamarin-apps:

- **Verwenden C#**  : Schreiben Sie Ihre apps C#in. Vorhandener Code, C# der in geschrieben ist, kann mit xamarin sehr einfach zu IOS und Android portiert werden und wird offensichtlich in Windows-Apps verwendet.
- **Verwenden von MVC-oder MVVM-Entwurfsmustern** : entwickeln Sie die Benutzeroberfläche Ihrer Anwendung mit dem mustermodell/Ansicht/Controller. Entwerfen Sie Ihre Anwendung mit einem Modell-/Ansicht-/controlleransatz oder einem Modell-/Ansicht-/viewmodell-Ansatz, bei dem es eine klare Trennung zwischen dem "Modell" und dem Rest gibt. Legen Sie fest, welche Teile der Anwendung Native Benutzeroberflächen Elemente jeder Plattform (Ios, Android, Windows, Mac) verwenden, und verwenden Sie diese als Richtlinie, um Ihre Anwendung in zwei Komponenten aufzuteilen: "Core" und "User-Interface".
- **Native** Benutzeroberflächen erstellen: jede betriebssystemspezifische Anwendung stellt eine andere Benutzeroberflächen Ebene bereit (Implementiert in C# mit der Unterstützung von nativen UI-Entwurfs Tools):

1. Verwenden Sie unter IOS die UIKit-APIs, um nativ aussehende Anwendungen zu erstellen. verwenden Sie optional den IOS-Designer von xamarin, um die Benutzeroberfläche visuell zu erstellen.
1. Unter Android können Sie mithilfe von Android. views systemeigene Anwendungen erstellen und den Benutzeroberflächen-Designer von xamarin nutzen.
1. Unter Windows verwenden Sie XAML für die Darstellungs Ebene, die in Visual Studio oder im Benutzeroberflächen-Designer von Blend erstellt wurde.
1. Auf dem Mac verwenden Sie Storyboards für die Darstellungs Ebene, die in Xcode erstellt wird.

Xamarin. Forms-Projekte werden auf allen Plattformen unterstützt und ermöglichen es Ihnen, Benutzeroberflächen zu erstellen, die plattformübergreifend mithilfe von xamarin. Forms-XAML freigegeben werden können. 

Der Umfang der erneuten Verwendung von Code hängt größtenteils davon ab, wie viel Code im gemeinsamen Kern beibehalten wird und wie viel Code Benutzeroberflächen spezifisch ist. Der Kerncode ist alles, was nicht direkt mit dem Benutzer interagiert, sondern Dienste für Teile der Anwendung bereitstellt, die diese Informationen erfassen und anzeigen.

Um die Wiederverwendung von Code zu erhöhen, können Sie plattformübergreifende Komponenten übernehmen, die allgemeine Dienste für all diese Systeme bereitstellen, wie z. b.:

1. [SQLite-net](https://www.nuget.org/packages/sqlite-net-pcl/) für lokalen SQL-Speicher,
1. [Xamarin](https://xamarin.com/plugins) -Plug-Ins für den Zugriff auf gerätespezifische Funktionen wie Kamera, Kontakte und geolozierung,
1. [Nuget-Pakete](https://nuget.org) , die mit xamarin-Projekten kompatibel sind, z. b. [JSON.net](https://www.nuget.org/packages/Newtonsoft.Json/)
1. Verwenden von .NET Framework-Features für Netzwerke, Webdienste, e/a und mehr.

Einige dieser Komponenten werden in der *Tasky* -Fallstudie implementiert.

 <a name="Separate_Reusable_Code_into_a_Core_Library" />

## <a name="separate-reusable-code-into-a-core-library"></a>Trennen von wiederverwendbarem Code in eine Kernbibliothek

Indem Sie das Prinzip der Trennung von Zuständigkeiten durch die Schichtung ihrer Anwendungsarchitektur und das anschließende Verschieben von Kernfunktionen, die plattformunabhängig in eine wiederverwendbare Kernbibliothek sind, befolgen, können Sie die Code Freigabe plattformübergreifend maximieren (siehe Abbildung unten). Illustri

 ![](overview-images/layers2.png "By following the principle of separation of responsibility by layering your application architecture and then moving core functionality that is platform agnostic into a reusable core library, you can maximize code sharing across platforms")

 <a name="Case_Studies" />

## <a name="case-studies"></a>Fallstudien

Es gibt eine Fallstudie, die dieses Dokument begleitet – *Tasky pro*. Jede Fallstudie erläutert die Implementierung der in diesem Dokument beschriebenen Konzepte in einem realen Beispiel. Der Code ist Open Source und auf [GitHub](https://github.com/xamarin/mobile-samples/)verfügbar.
