---
title: Erstellen von plattformübergreifenden Anwendungen Plattformübersicht
description: Dieses Dokument enthält einen allgemeinen Überblick über das Erstellen von plattformübergreifenden Anwendungen. Es wird erläutert, den Wert der C#, Entwurfsmuster, z. B. MVC/MVVM und systemeigenen Benutzeroberflächen.
ms.prod: xamarin
ms.assetid: E442EEFB-FA9C-40E9-9668-5A3F915C8400
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 1eb308e0095c29d8ab0d0bdf1f74b807fd2ab97f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61275623"
---
# <a name="building-cross-platform-applications-overview"></a>Erstellen von plattformübergreifenden Anwendungen Plattformübersicht

Dieser Leitfaden beschreibt die Xamarin-Plattform und Gewusst wie: entwerfen eine plattformübergreifenden Anwendung maximieren Wiederverwendungsrate für Code und bieten eine qualitativ hochwertige systemeigene Erfahrung auf allen mobilen hauptplattformen: iOS, Android und Windows Phone.

Der in diesem Dokument verwendete Ansatz gilt im Allgemeinen für sowohl Produktivitäts-apps und Spiele-apps, aber der Schwerpunkt auf Produktivität und -Hilfsprogramm (nicht-Game-Anwendungen). Finden Sie unter den [Einführung in MonoGame Dokument](~/graphics-games/monogame/introduction/index.md) oder sehen Sie sich [Visual Studio-Tools für Unity](https://docs.microsoft.com/visualstudio/cross-platform/visual-studio-tools-for-unity) plattformübergreifende Spieleentwicklung Anleitungen.

Der Ausdruck "Schreiben-einmal ausführen, überall" häufig verwendet, um jedoch die Vorzüge einer einzelnen Codebasis, die Ausführungen auf mehreren Plattformen unverändert. Während Sie den Vorteil, dass Code erneut zu verwenden ist, führt dieser Ansatz häufig Anwendungen, die eine featuresammlung niedrigsten gemeinsamen Nenner haben und eine generische aussehende Benutzeroberfläche, die nicht passen gut in einer der Zielplattformen.

Xamarin ist nicht nur eine "Write-überall einmal ausgeführt"-Plattform, da eine seiner Stärken die Möglichkeit, native Benutzeroberflächen, die speziell für jede Plattform zu implementieren ist. Allerdings mit durchdachter Entwurf, es weiterhin möglich ist, teilen, die meisten der nichtbenutzer-Schnittstelle Code und erhalten das beste aus beiden Welten: Ihre Daten Datenspeicher- und Business Logic-Code einmal schreiben und native Benutzeroberflächen auf jeder Plattform darstellen. In diesem Dokument wird erläutert, einen allgemeinen architektonischen Ansatz zur Erreichung dieses Ziels.

Hier ist eine Zusammenfassung der wichtigsten Punkte zum Erstellen von plattformübergreifenden Xamarin-apps:

-   **Verwendung C#**  – entwickeln Sie Ihre apps in C#. Code in der C# portiert für IOS- und Android mithilfe von Xamarin sehr einfach und natürlich in Windows-apps verwendet werden kann.
-   **Nutzen Sie MVC- oder MVVM-Entwurfsmuster** -Entwickeln Ihrer Anwendungsverzeichnis die Benutzeroberfläche mit dem Modell/Ansicht/Controller-Muster. Entwerfen Sie Ihre Anwendung mit einem Modell/Ansicht/Controller-Ansatz oder ein Modell/View/ViewModel-Ansatz ist es eine klare Trennung zwischen der "Model" und den Rest. Bestimmen Sie, welche Teile der Anwendung verwendet systemeigenen Elemente der Benutzeroberfläche für jede Plattform (iOS, Android, Windows, Mac), und verwenden Sie diese als Richtlinie zum Aufteilen Ihrer Anwendung in zwei Komponenten: "Core" und "Benutzeroberfläche".
-   **Erstellen Sie native Benutzeroberflächen** -jede Anwendung für ein bestimmtes Betriebssystem stellt eine andere Benutzeroberfläche bereit (in implementiert C# mit Unterstützung des nativen Benutzeroberflächen-Entwurfstools):

1.  Verwenden Sie unter iOS die UIKit-APIs zum Erstellen von Anwendungen nativ aussehende, optional mit Xamarin iOS-Designer, um die Benutzeroberfläche visuell zu erstellen.
1.  Verwenden Sie für Android Android.Views systemeigene Anwendungen nutzen die Xamarin Benutzeroberflächen-Designer erstellen.
1.  Auf Windows werden XAML verwenden Sie für die Darstellungsschicht, in Visual Studio oder Blend-Benutzeroberflächen-Designer erstellt.
1.  Unter Mac verwenden Sie Storyboards für die Darstellungsschicht, die in Xcode erstellt.

Xamarin.Forms-Projekte, die auf allen Plattformen unterstützt werden, und zulassen, dass Sie Benutzeroberflächen erstellen, die auf Plattformen mit Xamarin.Forms XAML gemeinsam genutzt werden können. 

Die Menge der Wiederverwendungsrate für Code, hängt größtenteils wie viel Code in gemeinsam genutzten Kerns, und wie viel Code Benutzeroberfläche bestimmte gehalten wird. Der Kerncode ist etwas, die interagiert nicht direkt mit dem Benutzer, sondern stattdessen bietet Dienste zum Teilen der Anwendung, die erfasst und diese Informationen anzeigen.

Um die Menge der Wiederverwendungsrate für Code zu erhöhen, können Sie Cross-Platform-Komponenten verwenden, die gemeinsame Dienste auf allen diesen Systemen, z. B. bereitstellen:

1.   [SQLite-Net-](https://www.nuget.org/packages/sqlite-net-pcl/) für den lokalen SQL-Speicher
1.   [Xamarin-Plugins](https://xamarin.com/plugins) für den Zugriff auf die gerätespezifische Funktionen wie z.B. die Kamera und Kontakte Geolocation
1.   [NuGet-Pakete](https://nuget.org) , z. B. mit Xamarin-Projekten kompatibel sind [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/),
1.  Mithilfe von .NET Framework-Funktionen für Netzwerke, Webdienste, e/a und mehr.


Einige dieser Komponenten sind in implementiert die *Tasky* Fallstudie.

 <a name="Separate_Reusable_Code_into_a_Core_Library" />


## <a name="separate-reusable-code-into-a-core-library"></a>Trennen von Wiederverwendbarem Code in eine Core-Bibliothek

Befolgen Sie das Prinzip der Trennung der Verantwortungsbereiche Schichten der Architektur Ihrer Anwendung, und klicken Sie dann verschieben Kernfunktionen, die plattformunabhängig in eine wiederverwendbare Core-Bibliothek ist, können Sie die Freigabe für Benutzeroberflächencode, wie die Abbildung unten optimieren. veranschaulicht:

 ![](overview-images/layers2.png "Befolgen Sie das Prinzip der Trennung der Verantwortungsbereiche Schichten der Architektur Ihrer Anwendung, und klicken Sie dann verschieben Kernfunktionen, die plattformunabhängig in eine wiederverwendbare Core-Bibliothek ist, können Sie die Freigabe für Benutzeroberflächencode maximieren.")

 <a name="Case_Studies" />


## <a name="case-studies"></a>Fallstudien

Es gibt eine Fallstudie, die in diesem Dokument – mit *Tasky Pro*. Jede Fallstudie wird erläutert, die Implementierung der in diesem Dokument in ein praktisches Beispiel beschriebenen Konzepte. Der Code ist open Source und verfügbar [Github](https://github.com/xamarin/mobile-samples/).
