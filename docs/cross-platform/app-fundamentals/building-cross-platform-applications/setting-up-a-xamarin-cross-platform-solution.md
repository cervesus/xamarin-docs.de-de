---
title: Teil 3 – Einrichten einer plattformübergreifenden Xamarin-Lösung
description: Dieses Dokument beschreibt das Einrichten einer plattformübergreifenden Lösung in Xamarin. Es wird erläutert, verschiedene Strategien wie z. B. freigegebene Projekte "und".NET Standard für die Codefreigabe.
ms.prod: xamarin
ms.assetid: 4139A6C2-D477-C563-C1AB-98CCD0D10A93
author: asb3993
ms.author: amburns
ms.date: 03/27/2017
ms.openlocfilehash: d20275bab4e4ce90f902a5e72321701d94b1d416
ms.sourcegitcommit: 4a1520dee7759f8355ea65c8bb3d1bac8ba58122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66354074"
---
# <a name="part-3---setting-up-a-xamarin-cross-platform-solution"></a>Teil 3 – Einrichten einer plattformübergreifenden Xamarin-Lösung

Unabhängig davon, welche Plattformen verwendet werden, verwenden Sie Xamarin-Projekte, die alle der gleichen Projektmappe-Dateiformat (Visual Studio **sln** -Dateiformat). Lösungen können für entwicklungsumgebungen, freigegeben werden, selbst wenn einzelne Projekte (z. B. ein Windows-Projekt in Visual Studio für Mac) können nicht geladen werden.



Wenn Sie eine neue plattformübergreifende Anwendung zu erstellen, ist der erste Schritt, um eine leere Projektmappe zu erstellen. In diesem Abschnitt wird erläutert, was als Nächstes geschieht: die Projekte für das Erstellen von plattformübergreifenden mobilen apps einrichten.

 <a name="Sharing_Code" />


## <a name="sharing-code"></a>Freigeben von Code

Finden Sie in der [Code Sharing Options](~/cross-platform/app-fundamentals/code-sharing.md) Dokument für eine ausführliche Beschreibung zum Freigeben von Code über Plattformen hinweg implementieren.

 <a name="Shared_Asset_Projects" />


### <a name="shared-projects"></a>Freigegebene Projekte

Die einfachste Methode zur Freigabe von Codedateien verwendet ein [freigegebenes Projekt](~/cross-platform/app-fundamentals/shared-projects.md).

Dieser Methode können Sie den gleichen Code für verschiedene Plattformprojekte freigeben und verwenden Compiler-Direktiven, um verschiedene, plattformspezifischen Codepfade enthalten.

 <a name="Portable_Class_Libraries" />


### <a name="portable-class-libraries-pcl"></a>Portable Klassenbibliotheken

In der Vergangenheit wurde eine Projektdatei für .NET (und die resultierende Assembly) auf eine bestimmte Frameworkversion vorgesehen. Dies verhindert, dass das Projekt oder die Assembly, die von verschiedenen Frameworks genutzt wird.

Eine Portable Klassenbibliothek (PCL) ist eine besondere Art von Projekt, das auf unterschiedlichen CLI-Plattformen wie z. B. Xamarin.iOS und Xamarin.Android-als auch WPF, universelle Windows-Plattform und Xbox verwendet werden kann. Die Bibliothek kann nur eine Teilmenge der vollständigen .NET Framework beschränkt, die durch die Plattform abgezielt wird nutzen.

Erfahren Sie mehr über die Xamarin [Unterstützung für Portable Klassenbibliotheken](~/cross-platform/app-fundamentals/pcl.md) , und befolgen Sie die dort beschriebenen Anweisungen, finden Sie unter wie die [TaskyPortable Beispiel](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable) funktioniert.


### <a name="net-standard"></a>.NET-Standard

In 2016 eingeführten [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md) Projekte bieten eine einfache Möglichkeit zum Freigeben von Code erzeugen Assemblys, die verwendet werden können für Windows-Plattformen, Xamarin-Plattformen (iOS, Android, Mac) und Linux.

.NET standard-Bibliotheken können erstellt und wie PCLs verwendet werden, außer dass die APIs in jeder Version (1.0 auf 1.6) werden leichter erkannt, und jede Version abwärtskompatibel ist mit einer niedrigeren Versionsnummer.



 <a name="Populating_the_Solution" />


## <a name="populating-the-solution"></a>Füllen die Lösung

Unabhängig davon, welche Methode zum Freigeben von Code verwendet wird sollte die allgemeine Lösungsstruktur eine Schichtenarchitektur implementieren, die Freigabe von Code fördert.
Der Xamarin-Ansatz ist die Gruppencode in den zwei Projekttypen:

-   **Core-Projekt** – wieder verwendbare Programmieren an einem Ort, um auf verschiedenen Plattformen gemeinsam genutzt werden. Verwenden Sie die Prinzipien der Kapselung, um Implementierungsdetails auszublenden, wo immer dies möglich.
-   **Clientplattform-spezifische-Anwendungsprojekten** – nutzen Sie den wieder verwendbaren Code mit wenig Kopplung wie möglich. Plattformspezifische Features werden auf dieser Ebene basiert auf Komponenten, die verfügbar gemacht werden, in der Core-Projekt hinzugefügt.


 <a name="Core_Project" />


### <a name="core-project"></a>Core-Projekt

Projekte mit freigegebenem Code sollten nur auf Assemblys verweisen, die auf allen Plattformen – d. h. bereitstehen. die allgemeinen Framework-Namespaces wie `System`, `System.Core` und `System.Xml`.

Freigegebene Projekte sollten wie möglich ist, die folgenden Ebenen enthalten, werden so viele nicht-UI-Funktionen implementieren:

-   **Datenschicht** – Code, den physischen Datenspeicher z. B. übernimmt.  [SQLite-NET-](https://github.com/praeclarum/sqlite-net), wie Sie eine alternative Datenbank [Realm.io](https://realm.io/products/realm-mobile-database/) oder sogar XML-Dateien. Die Datenklassen für die Ebene sind normalerweise nur von der Datenzugriffsebene verwendet.
-   **Daten von Zugriffsschicht** – definiert eine API, die die Vorgänge erforderlichen Daten für die Funktionalität der Anwendung unterstützt, z. B. Methoden, die Zugriff auf Listen mit Daten, die Daten für die einzelnen Elemente und auch erstellen, bearbeiten und löschen.
-   **-Dienst von Zugriffsschicht** – eine optionale Ebene zu Cloud services für die Anwendung. Enthält Code, der remote-Netzwerk-Ressourcen (Web Services, Bilddownloads, usw.) zugreift, und möglicherweise die Ergebnisse werden zwischengespeichert.
-   **Geschäftsschicht** – Definition der ViewModel-Klassen und die Fassade oder -Manager-Klassen, die Funktionen, die plattformspezifische Anwendungen verfügbar zu machen.


 <a name="Platform-Specific_Application_Projects" />


### <a name="platform-specific-application-projects"></a>Clientplattform-spezifische-Anwendungsprojekten

Plattformspezifischen Projekten müssen die Assemblys erforderlich sind, zum Binden an jede Plattform-SDK (Xamarin.iOS, Xamarin.Android, Xamarin.Mac oder Windows) sowie das Core-Projekt für freigegebenen Code verweisen.

Die plattformspezifischen Projekten müssen implementieren:

-   **Anwendungsschicht** – Plattform bestimmte Funktionen und Bindung/Konvertierung zwischen den Geschäftsschicht-Objekten und die Benutzeroberfläche.
-   **UI-Schicht** – Bildschirme, die benutzerdefinierte Benutzeroberflächen-Steuerelemente, die Darstellung von Validierungslogik.


<a name="Example" />


### <a name="example"></a>Beispiel

Die Anwendungsarchitektur ist in diesem Diagramm dargestellt:

 [ ![](setting-up-a-xamarin-cross-platform-solution-images/conceptualarchitecture.png "Die Anwendungsarchitektur wird in diesem Diagramm dargestellt.")](setting-up-a-xamarin-cross-platform-solution-images/conceptualarchitecture.png#lightbox)

Dieser Screenshot zeigt ein Projektmappen-Setup mit der freigegebenen Core-Projekts, iOS und Android-Anwendungsprojekten. Das freigegebene Projekt enthält Code, die in Bezug auf die einzelnen Architekturebenen (Business "," Dienst "," Daten "und" Data Access Code):

 ![](setting-up-a-xamarin-cross-platform-solution-images/core-solution-example.png "Das freigegebene Projekt enthält Code, die in Bezug auf die einzelnen Architekturebenen (Business \",\" Dienst \",\" Daten \"und\" Data Access Code)")


 <a name="Project_References" />


## <a name="project-references"></a>Projektverweise

Projektverweise wider, die Abhängigkeiten für ein Projekt. Core-Projekte beschränken ihre Verweise auf Assemblys common, sodass der Code problemlos freigegeben werden.
Clientplattform-spezifische Anwendungsprojekte verweisen, der freigegebene Code sowie alle anderen müssen sie die Zielplattform nutzen plattformspezifische Assemblys.

Die Anwendungsprojekte jeder Verweis auf Shared-Projekt, und enthalten den UI-Code vorhanden Funktionalität für den Benutzer erforderlich ist, wie im folgenden Screenshots gezeigt:

![](setting-up-a-xamarin-cross-platform-solution-images/solution-android.png "Die Anwendung projiziert, jeden Verweis freigegebenes Projekt") ![](setting-up-a-xamarin-cross-platform-solution-images/solution-ios.png "die Anwendung projiziert, jeden Verweis freigegebenen Projekts")


Spezifische Beispiele zum Strukturieren von Projekten werden in den Fallstudien angegeben.

 <a name="Adding_Files" />


## <a name="adding-files"></a>Hinzufügen von Dateien

 <a name="Build_Action" />


### <a name="build-action"></a>Buildvorgang

Es ist wichtig, dass die richtigen Build-Aktion für bestimmte Dateitypen festgelegt. Diese Liste zeigt den Buildvorgang für einige gängige Dateitypen:

-  **Alle C# Dateien** – Buildvorgang: Compile
-   **Bilder in Xamarin.iOS & Windows** – Buildvorgang: Content
-   **XIB und Storyboard-Dateien in Xamarin.iOS** – Buildvorgang: InterfaceDefinition
-   **Bilder und AXML-Layouts in Android** – Buildvorgang: AndroidResource
-  **XAML-Dateien in Windows-Projekten** – Buildvorgang: Seite
-  **Xamarin.Forms-XAML-Dateien** – Buildvorgang: EmbeddedResource


In der Regel die IDE den Dateityp erkennt und die geeigneten Build-Aktion wird empfohlen.

 <a name="Case_Sensitivity" />


### <a name="case-sensitivity"></a>Groß- und Kleinschreibung

Denken Sie daran, dass einige Plattformen (z. b. die Groß-/Kleinschreibung Dateisysteme haben
iOS und Android) daher unbedingt einen Benennungsstandard für konsistente Datei und stellen Sie sicher, dass die Dateinamen, die Sie in Code verwenden, das Dateisystem genau übereinstimmen. Dies ist besonders wichtig für Bilder und andere Ressourcen, die Sie im Code referenzieren.
