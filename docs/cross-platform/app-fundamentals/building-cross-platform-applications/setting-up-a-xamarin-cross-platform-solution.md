---
title: 'Teil 3: Einrichten einer plattformübergreifenden xamarin-Lösung'
description: In diesem Dokument wird beschrieben, wie Sie eine plattformübergreifende Lösung in xamarin einrichten. Es werden verschiedene Strategien zur Code Freigabe erläutert, z. b. freigegebene Projekte und .NET Standard.
ms.prod: xamarin
ms.assetid: 4139A6C2-D477-C563-C1AB-98CCD0D10A93
author: asb3993
ms.author: amburns
ms.date: 03/27/2017
ms.openlocfilehash: a33c924df3da8642f4b765868f213e6e196f7866
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69526811"
---
# <a name="part-3---setting-up-a-xamarin-cross-platform-solution"></a>Teil 3: Einrichten einer plattformübergreifenden xamarin-Lösung

Unabhängig davon, welche Plattformen verwendet werden, verwenden xamarin-Projekte das gleiche projektmappendateiformat (das Visual Studio **. sln** -Dateiformat). Lösungen können in Entwicklungsumgebungen gemeinsam genutzt werden, auch wenn einzelne Projekte nicht geladen werden können (z. b. ein Windows-Projekt in Visual Studio für Mac).



Beim Erstellen einer neuen plattformübergreifenden Anwendung besteht der erste Schritt darin, eine leere Projekt Mappe zu erstellen. In diesem Abschnitt wird erläutert, was als nächstes geschieht: Einrichten der Projekte zum Entwickeln von plattformübergreifenden mobilen apps.

 <a name="Sharing_Code" />


## <a name="sharing-code"></a>Freigeben von Code

Im Dokument [Code Freigabe Optionen](~/cross-platform/app-fundamentals/code-sharing.md) finden Sie eine ausführliche Beschreibung der Implementierung der Code Freigabe auf verschiedenen Plattformen.

 <a name="Shared_Asset_Projects" />


### <a name="shared-projects"></a>Freigegebene Projekte

Der einfachste Ansatz für die Freigabe von Code Dateien ist die Verwendung eines frei [gegebenen Projekts](~/cross-platform/app-fundamentals/shared-projects.md).

Mit dieser Methode können Sie denselben Code für verschiedene Platt Form Projekte verwenden und Compilerdirektiven verwenden, um unterschiedliche plattformspezifische Codepfade einzuschließen.

 <a name="Portable_Class_Libraries" />


### <a name="portable-class-libraries-pcl"></a>Portable Klassenbibliotheken

In der Vergangenheit wurde eine .net-Projektdatei (und die resultierende Assembly) auf eine bestimmte Framework-Version ausgerichtet. Dadurch wird verhindert, dass das Projekt oder die Assembly von anderen Frameworks gemeinsam verwendet wird.

Eine portable Klassenbibliothek (Portable Class Library, PCL) ist ein spezieller Projekttyp, der auf unterschiedlichen CLI-Plattformen wie xamarin. IOS und xamarin. Android sowie WPF, universelle Windows-Plattform und Xbox verwendet werden kann. Die Bibliothek kann nur eine Teilmenge des gesamten .NET-Frameworks verwenden, das durch die Zielplattformen eingeschränkt ist.

Weitere Informationen zur Unterstützung von xamarin finden Sie [Unterstützung für Portable Klassenbibliotheken](~/cross-platform/app-fundamentals/pcl.md) . Befolgen Sie hierzu die Anweisungen, um zu erfahren, wie das [taskyportable-Beispiel](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable) funktioniert.


### <a name="net-standard"></a>.NET-Standard

[.NET Standard](~/cross-platform/app-fundamentals/net-standard.md) Projekte wurden in 2016 eingeführt und stellen eine einfache Möglichkeit dar, Code plattformübergreifend freizugeben und Assemblys zu erstellen, die auf Windows-, xamarin-Plattformen (Ios, Android, Mac) und Linux verwendet werden können.

.NET Standard Bibliotheken können wie pcls erstellt und verwendet werden, mit dem Unterschied, dass die in jeder Version verfügbaren APIs (von 1,0 bis 1,6) leichter erkannt werden und jede Version abwärts kompatibel mit niedrigeren Versionsnummern ist.



 <a name="Populating_the_Solution" />


## <a name="populating-the-solution"></a>Auffüllen der Lösung

Unabhängig davon, welche Methode verwendet wird, um Code freizugeben, sollte die allgemeine Lösungs Struktur eine mehrschichtige Architektur implementieren, die die Code Freigabe fördert.
Der xamarin-Ansatz besteht darin, Code in zwei Projekttypen zu gruppieren:

- **Kernprojekt** – schreiben Sie wiederverwendbaren Code an einem Ort, der für verschiedene Plattformen freigegeben werden kann. Verwenden Sie die Prinzipien der Kapselung, um die Implementierungsdetails möglichst auszublenden.
- **Plattformspezifische Anwendungsprojekte** – verwenden Sie den wiederverwendbaren Code mit möglichst geringem Kopplung. Plattformspezifische Features werden auf dieser Ebene hinzugefügt, die auf Komponenten basiert, die im Kernprojekt verfügbar gemacht werden.


 <a name="Core_Project" />


### <a name="core-project"></a>Core-Projekt

Freigegebene Code Projekte sollten nur auf Assemblys verweisen, die plattformübergreifend verfügbar sind – d. a. die allgemeinen Framework-Namespaces `System`wie `System.Core` , `System.Xml`und.

Freigegebene Projekte sollten so viele Funktionen wie möglich implementieren, die nicht der Benutzeroberfläche unterliegen. Dies kann die folgenden Ebenen einschließen:

- **Datenschicht** – Code, der die physische Datenspeicherung übernimmt, z. b.  [SQLite-net](https://github.com/praeclarum/sqlite-net), eine alternative Datenbank wie [Realm.IO](https://realm.io/products/realm-mobile-database/) oder sogar XML-Dateien. Die Klassen der Datenschicht werden normalerweise nur von der Datenzugriffs Ebene verwendet.
- **Datenzugriffs Schicht** – definiert eine API, die die erforderlichen Daten Vorgänge für die Funktionalität der Anwendung unterstützt, wie z. b. Methoden für den Zugriff auf Listen von Daten, einzelne Datenelemente und deren Erstellung, Bearbeitung und Löschung.
- **Dienst Zugriffsebene** – eine optionale Ebene zur Bereitstellung von Clouddiensten für die Anwendung. Enthält Code, der auf Remote Netzwerkressourcen zugreift (Webdienste, Image Downloads usw.) und möglicherweise das Zwischenspeichern der Ergebnisse.
- **Geschäfts Schicht** – Definition der Modellklassen und der Fassaden-oder Manager-Klassen, die Funktionen für die plattformspezifischen Anwendungen verfügbar machen.


 <a name="Platform-Specific_Application_Projects" />


### <a name="platform-specific-application-projects"></a>Plattformspezifische Anwendungsprojekte

Plattformspezifische Projekte müssen auf die Assemblys verweisen, die für die Bindung an das SDK der einzelnen Plattformen (xamarin. IOS, xamarin. Android, xamarin. Mac oder Windows) und das freigegebene Kern Code Projekt erforderlich sind.

Die plattformspezifischen Projekte sollten Folgendes implementieren:

- **Anwendungsschicht** – plattformspezifische Funktionalität und Bindung/Konvertierung zwischen den Geschäfts Schicht Objekten und der Benutzeroberfläche.
- **Benutzeroberflächen Ebene** – Bildschirme, Benutzeroberflächen-Steuerelemente, Darstellung der Validierungs Logik.


<a name="Example" />


### <a name="example"></a>Beispiel

Die Anwendungsarchitektur ist in diesem Diagramm dargestellt:

 [![](setting-up-a-xamarin-cross-platform-solution-images/conceptualarchitecture.png "Die Anwendungsarchitektur ist in diesem Diagramm dargestellt.")](setting-up-a-xamarin-cross-platform-solution-images/conceptualarchitecture.png#lightbox)

Dieser Screenshot zeigt eine projektmappeneinrichtung mit den Projekten Shared Core Project, IOS und Android Application. Das freigegebene Projekt enthält Code, der sich auf die einzelnen Architektur Ebenen bezieht (Geschäfts-, Dienst-, Daten-und Datenzugriffs Code):

 ![](setting-up-a-xamarin-cross-platform-solution-images/core-solution-example.png "Das freigegebene Projekt enthält Code, der sich auf die einzelnen Architektur Ebenen bezieht (Geschäfts-, Dienst-, Daten-und Datenzugriffs Code).")


 <a name="Project_References" />


## <a name="project-references"></a>Projektverweise

Projekt Verweise entsprechen den Abhängigkeiten für ein Projekt. Core-Projekte beschränken ihre Verweise auf allgemeine Assemblys, sodass der Code einfach freigegeben werden kann.
Plattformspezifische Anwendungsprojekte verweisen auf den freigegebenen Code und alle anderen plattformspezifischen Assemblys, die Sie benötigen, um die Zielplattform zu nutzen.

Die Anwendung projiziert jedes freigegebene Verweis Projekt und enthält den Code für die Benutzeroberfläche, der zum Darstellen der Funktionalität für den Benutzer erforderlich ist, wie in den folgenden Screenshots gezeigt:

![](setting-up-a-xamarin-cross-platform-solution-images/solution-android.png "Die Anwendung projiziert, jeden Verweis freigegebenes Projekt") ![](setting-up-a-xamarin-cross-platform-solution-images/solution-ios.png "die Anwendung projiziert, jeden Verweis freigegebenen Projekts")


Bestimmte Beispiele dafür, wie Projekte strukturiert werden sollten, sind in den Fallstudien angegeben.

 <a name="Adding_Files" />


## <a name="adding-files"></a>Hinzufügen von Dateien

 <a name="Build_Action" />


### <a name="build-action"></a>Buildvorgang

Es ist wichtig, die richtige Build-Aktion für bestimmte Dateitypen festzulegen. Diese Liste zeigt die Buildaktion für einige gängige Dateitypen:

- **Alle C# Dateien** – Buildaktion: Compile
- **Bilder in xamarin. IOS & Windows** – Build Action: Inhalt
- **XIb-und Storyboard-Dateien in xamarin. IOS** – Build Action: Interfacedefinition
- **Images und axml-Layouts in Android** – Build Action: AndroidResource
- **XAML-Dateien in Windows-Projekten** – Buildaktion: Seite
- **Xamarin. Forms-XAML-Dateien** – Buildaktion: EmbeddedResource


Der Dateityp wird in der Regel von der IDE erkannt und die richtige Buildaktion vorgeschlagen.

 <a name="Case_Sensitivity" />


### <a name="case-sensitivity"></a>Groß- und Kleinschreibung

Denken Sie schließlich daran, dass einige Plattformen die Groß-/Kleinschreibung beachtet haben (z. b.
IOS und Android) stellen Sie sicher, dass Sie einen konsistenten Dateibenennungs Standard verwenden, und stellen Sie sicher, dass die Dateinamen, die Sie im Code verwenden, genau mit dem Datei Dies ist besonders wichtig für Images und andere Ressourcen, auf die Sie im Code verweisen.
