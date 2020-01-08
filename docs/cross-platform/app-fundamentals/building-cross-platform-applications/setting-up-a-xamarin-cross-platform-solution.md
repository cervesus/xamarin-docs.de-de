---
title: 'Teil 3: Einrichten einer plattformübergreifenden xamarin-Lösung'
description: In diesem Dokument wird beschrieben, wie Sie eine plattformübergreifende Lösung in xamarin einrichten. Es werden verschiedene Strategien zur Code Freigabe erläutert, z. b. freigegebene Projekte und .NET Standard.
ms.prod: xamarin
ms.assetid: 4139A6C2-D477-C563-C1AB-98CCD0D10A93
author: davidortinau
ms.author: daortin
ms.date: 03/27/2017
ms.openlocfilehash: e7cde22115830a845ed82aa907195521f36b6866
ms.sourcegitcommit: d8af612b6b3218fea396d2f180e92071c4d4bf92
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2020
ms.locfileid: "75663272"
---
# <a name="part-3---setting-up-a-xamarin-cross-platform-solution"></a>Teil 3: Einrichten einer plattformübergreifenden xamarin-Lösung

Unabhängig davon, welche Plattformen verwendet werden, verwenden xamarin-Projekte das gleiche projektmappendateiformat (das Visual Studio **. sln** -Dateiformat). Lösungen können in Entwicklungsumgebungen gemeinsam genutzt werden, auch wenn einzelne Projekte nicht geladen werden können (z. b. ein Windows-Projekt in Visual Studio für Mac).

Beim Erstellen einer neuen plattformübergreifenden Anwendung besteht der erste Schritt darin, eine leere Projekt Mappe zu erstellen. In diesem Abschnitt wird erläutert, was als nächstes geschieht: Einrichten der Projekte zum Entwickeln von plattformübergreifenden mobilen apps.

## <a name="sharing-code"></a>Freigeben von Code

Im Dokument [Code Freigabe Optionen](~/cross-platform/app-fundamentals/code-sharing.md) finden Sie eine ausführliche Beschreibung der Implementierung der Code Freigabe auf verschiedenen Plattformen.

### <a name="net-standard"></a>.NET-Standard

[.NET Standard](~/cross-platform/app-fundamentals/net-standard.md) Projekte bieten eine einfache Möglichkeit, Code plattformübergreifend freizugeben und Assemblys zu erstellen, die auf Windows-, xamarin-Plattformen (Ios, Android, Mac) und Linux verwendet werden können.
Dies ist die empfohlene Methode zum Freigeben von Code für xamarin-Lösungen.

### <a name="other-options"></a>Weitere Optionen

In der Vergangenheit verwendete xamarin [Portable Klassenbibliotheken (Portable Class Libraries, pcls)](~/cross-platform/app-fundamentals/pcl.md)und frei [gegebene Projekte](~/cross-platform/app-fundamentals/shared-projects.md). Keines dieser Informationen wird für neue Projekte empfohlen. und Sie sollten eine Migration vorhandener apps in Erwägung haben, um .NET Standard zu verwenden

## <a name="populating-the-solution"></a>Auffüllen der Lösung

Unabhängig davon, welche Methode verwendet wird, um Code freizugeben, sollte die allgemeine Lösungs Struktur eine mehrschichtige Architektur implementieren, die die Code Freigabe fördert.
Der xamarin-Ansatz besteht darin, Code in zwei Projekttypen zu gruppieren:

- **Core (oder "Shared") Project** – schreiben Sie wiederverwendbaren Code an einem Ort, der für verschiedene Plattformen freigegeben werden kann. Verwenden Sie die Prinzipien der Kapselung, um die Implementierungsdetails möglichst auszublenden.
- **Plattformspezifische Anwendungsprojekte** – verwenden Sie den wiederverwendbaren Code mit möglichst geringem Kopplung. Plattformspezifische Features werden auf dieser Ebene hinzugefügt, die auf Komponenten basiert, die im Kernprojekt verfügbar gemacht werden.

### <a name="core-project"></a>Core-Projekt

Kernprojekte, die Code gemeinsam nutzen, sollten .NET Standard und nur Verweisassemblys sein, die plattformübergreifend verfügbar sind – d. die Common Framework-Namespaces wie `System`, `System.Core` und `System.Xml`.

Kernprojekte sollten so viele Funktionen wie möglich implementieren, die nicht der Benutzeroberfläche unterliegen. Dies kann die folgenden Ebenen einschließen:

- **Datenschicht** – Code, der die physische Datenspeicherung übernimmt, z. b. [SQLite-net](https://www.nuget.org/packages/sqlite-net-pcl/) oder sogar XML-Dateien. Die Klassen der Datenschicht werden normalerweise nur von der Datenzugriffs Ebene verwendet.
- **Datenzugriffs Schicht** – definiert eine API, die die erforderlichen Daten Vorgänge für die Funktionalität der Anwendung unterstützt, wie z. b. Methoden für den Zugriff auf Listen von Daten, einzelne Datenelemente und deren Erstellung, Bearbeitung und Löschung.
- **Dienst Zugriffsebene** – eine optionale Ebene zur Bereitstellung von Clouddiensten für die Anwendung. Enthält Code, der auf Remote Netzwerkressourcen zugreift (Webdienste, Image Downloads usw.) und möglicherweise das Zwischenspeichern der Ergebnisse.
- **Geschäfts Schicht** – Definition der Modellklassen und der Fassaden-oder Manager-Klassen, die Funktionen für die plattformspezifischen Anwendungen verfügbar machen.

### <a name="platform-specific-application-projects"></a>Plattformspezifische Anwendungsprojekte

Plattformspezifische Projekte müssen auf die Assemblys verweisen, die für die Bindung an das SDK der einzelnen Plattformen (xamarin. IOS, xamarin. Android, xamarin. Mac oder Windows) und das .NET Standard Projekt erforderlich sind.

Die plattformspezifischen Projekte sollten Folgendes implementieren:

- **Anwendungsschicht** – plattformspezifische Funktionalität und Bindung/Konvertierung zwischen den Geschäfts Schicht Objekten und der Benutzeroberfläche.
- **Benutzeroberflächen Ebene** – Bildschirme, Benutzeroberflächen-Steuerelemente, Darstellung der Validierungs Logik.

## <a name="project-references"></a>Projektverweise

Projekt Verweise entsprechen den Abhängigkeiten für ein Projekt. Core-Projekte beschränken ihre Verweise auf allgemeine Assemblys, sodass der Code einfach freigegeben werden kann.
Plattformspezifische Anwendungsprojekte verweisen auf das .NET Standard Projekt und alle anderen plattformspezifischen Assemblys, die Sie benötigen, um die Zielplattform zu nutzen.

Bestimmte Beispiele dafür, wie Projekte strukturiert werden sollten, sind in den Fallstudien angegeben.

## <a name="adding-files"></a>Hinzufügen von Dateien

### <a name="build-action"></a>Buildaktion

Es ist wichtig, die richtige Build-Aktion für bestimmte Dateitypen festzulegen. Diese Liste zeigt die Buildaktion für einige gängige Dateitypen:

- **Alle C# Dateien** – Buildaktion: Kompilieren
- **Bilder in xamarin. IOS & Windows** – Build Action: Content
- **XIb-und Storyboard-Dateien in xamarin. IOS** – Build Action: interfacedefinition
- **Bilder und XML-Layouts in Android** – Build Action: androidresource
- **XAML-Dateien in Windows-Projekten** – Buildaktion: Seite
- **Xamarin. Forms-XAML-Dateien** – Buildaktion: EmbeddedResource

Der Dateityp wird in der Regel von der IDE erkannt und die richtige Buildaktion vorgeschlagen.

### <a name="case-sensitivity"></a>Unterscheidung nach Groß-/Kleinschreibung

Denken Sie schließlich daran, dass einige Plattformen die Groß-/Kleinschreibung beachtet haben (z. b.
IOS und Android) stellen Sie sicher, dass Sie einen konsistenten Dateibenennungs Standard verwenden, und stellen Sie sicher, dass die Dateinamen, die Sie im Code verwenden, genau mit dem Datei Dies ist besonders wichtig für Images und andere Ressourcen, auf die Sie im Code verweisen.
