---
title: "Erstellen von Cross-Platform-Anwendungen (Übersicht)"
ms.topic: article
ms.prod: xamarin
ms.assetid: E442EEFB-FA9C-40E9-9668-5A3F915C8400
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 8e7b25f615939c0ae902e09c728615188d1cb86e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="building-cross-platform-applications-overview"></a>Erstellen von Cross-Platform-Anwendungen (Übersicht)

Dieses Handbuch enthält die Xamarin-Plattform und wie eine plattformübergreifende Anwendung maximieren die Wiederverwendung von Code und bieten eine systemeigene qualitativ hochwertige-Erlebnis auf allen mobilen hauptplattformen konzipiert: iOS, Android und Windows Phone.

Ansatz wird in diesem Dokument gilt im Allgemeinen für Produktivität apps und Spiele apps jedoch der Fokus auf die Produktivität und -Dienstprogramm (nicht-Spiel Anwendungen) ist. Finden Sie unter der [Einführung MonoGame Dokument](https://developer.xamarin.com/guides/cross-platform/game_development/monogame/introduction/) oder Auschecken [Visual Studio-Tools für Unity](https://docs.microsoft.com/en-us/visualstudio/cross-platform/visual-studio-tools-for-unity) plattformübergreifende Entwicklung Anleitungen.

Der Ausdruck "Schreiben-einmal ausführen, überall" wird häufig verwendet werden, um zeigen die Vorzüge einer einzelnen Codebasis, ausgeführt auf mehreren Plattformen unverändert bleiben sollen. Während Sie den Vorteil, dass Code erneut zu verwenden ist, führt diesen Ansatz häufig zu Anwendungen, die eine Funktionsgruppe niedrigsten gemeinsamen Nenner haben und eine generische aussehende-Benutzeroberfläche, die nicht zu groß ist gut in eine der Zielplattformen.

Xamarin ist nicht nur eine "Write-einmal ausführen, überall" Plattform, da auf eine der Stärken die Möglichkeit, systemeigene Benutzeroberflächen speziell für jede Plattform implementieren. Allerdings mit Bedacht vorgehen Entwurf weiterhin möglich ist, freigeben Großteil der nichtbenutzer-Schnittstelle Code und das beste von beidem zu erhalten: einmal Schreiben von Code für Ihre Daten Datenspeicher- und Business Geschäftslogik und native Benutzeroberflächen auf jeder Plattform darstellen. Dieses Dokument beschreibt eine allgemeine Architektur Vorgehensweise zu diesem Zweck.

Hier wird eine Zusammenfassung der wichtigsten Punkte zum Erstellen von Xamarin plattformübergreifende apps:

-   **Verwenden von c#** -apps in c# schreiben. Vorhandenen Code in c# geschriebene kann portiert für IOS- und Android mithilfe von Xamarin sehr einfach, und offensichtlich in Windows-apps verwendet werden.
-   **Verwenden von MVC oder MVVVM Entwurfsmuster** -Entwickeln der Benutzeroberfläche der Anwendung, die mit dem Modell/Ansicht/Controller-Muster. Ihre Anwendung mit einem Modell/Ansicht/Controller Ansatz oder ein Modell/Sicht/ViewModel-Ansatz zu entwickeln, es eine klaren Unterschied zwischen dem "Modell" und den Rest gibt. Bestimmen, welche Teile der Anwendung verwendet systemeigene Benutzeroberflächenelemente jeder Plattform (iOS, Android, Windows, Mac), und verwenden Sie diese als Richtlinie zum Aufteilen von Ihrer Anwendung in zwei Komponenten: "Core" und "Benutzeroberfläche".
-   **Erstellen Sie systemeigene Benutzeroberflächen** -jede betriebssystemspezifischen Anwendung stellt eine andere Benutzeroberfläche (implementiert in c# mit Unterstützung von systemeigenen UI-Entwurfstools):

1.  IOS verwenden Sie die UIKit-APIs zum Erstellen von Anwendungen systemeigene, optional die Xamarin iOS-Designer, um Ihre Benutzeroberfläche visuell zu erstellen.
1.  Verwenden Sie auf Android-Geräten Android.Views systemeigene Anwendungen profitieren von Xamarin UI-Designer erstellen.
1.  Unter Windows werden Sie XAML für Präsentationsschicht, erstellt Visual Studio oder Blend des UI-Designer verwenden.
1.  Auf einem Mac verwenden Sie Storyboards für die Präsentationsschicht, die in Xcode erstellt.

Xamarin.Forms-Projekte werden auf allen Plattformen unterstützt, und zulassen, dass Sie Benutzeroberflächen erstellen, die für Plattformen mit Xamarin.Forms XAML freigegeben sein können. 

Der Anteil am Code erneut verwenden hängt größtenteils wie viel Code in der gemeinsam genutzter Kern, und wie viel Code Benutzeroberfläche bestimmte beibehalten wird. Der Kerncode ist alles, das interagieren nicht direkt mit dem Benutzer, stellt Dienste für Teile der Anwendung, die erfasst und zeigt diese Informationen jedoch.

Um die Menge der Wiederverwendung von Code zu erhöhen, können Sie plattformübergreifende Komponenten verwenden, die gemeinsame Dienste auf allen diesen Systemen, z. B. bereitstellen:

1.   [SQLite-Net](https://www.nuget.org/packages/sqlite-net-pcl/) für den lokalen SQL-Speicher
1.   [Xamarin-Plugins](https://xamarin.com/plugins) für den Zugriff auf Geräte-spezifischer Funktionen einschließlich Kamera, Kontakte und Geolocation,
1.   [NuGet-Pakete](https://nuget.org) , sind kompatibel mit Xamarin-Projekten, wie z. B. [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/),
1.  Mithilfe von .NET Framework-Funktionen für Netzwerke, Webdienste, e/a- und mehr.


Einige dieser Komponenten werden implementiert, der *Tasky* Fallstudie.

 <a name="Separate_Reusable_Code_into_a_Core_Library" />


## <a name="separate-reusable-code-into-a-core-library"></a>Aufteilen von Wiederverwendbarem Code in einem-Kernbibliothek

Gemäß das Prinzip der Trennung der Verantwortung Schichten Ihrer Anwendungsarchitektur und Verschieben von Kernfunktionen, die Plattform sind in einer wiederverwendbaren Core-Bibliothek ist können Sie alle Plattformen gemeinsam verwendet, als die folgende Abbildung Code optimieren. veranschaulicht:

 ![](part-0-overview-images/layers2.png "Sie können Code alle Plattformen gemeinsam maximieren, gemäß das Prinzip der Trennung der Verantwortung Schichten Ihrer Anwendungsarchitektur und Verschieben von Kernfunktionen, die Plattform sind in einer wiederverwendbaren Core-Bibliothek ist")

 <a name="Case_Studies" />


## <a name="case-studies"></a>Fallstudien

Es wird eine Fallstudie, die mit diesem Dokument – *Tasky Pro*. Jede Fallstudie wird die Implementierung der Konzepte beschrieben, die in diesem Dokument in einem realen Beispiel erläutert. Der Code ist eine open Source und verfügbar machen [Github](https://github.com/xamarin/mobile-samples/).