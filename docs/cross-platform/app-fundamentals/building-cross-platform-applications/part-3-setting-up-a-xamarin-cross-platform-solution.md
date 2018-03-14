---
title: "Teil 3 – Einrichten einer Xamarin-Cross-Plattformlösung"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4139A6C2-D477-C563-C1AB-98CCD0D10A93
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/27/2017
ms.openlocfilehash: 852c11eeccf347c3175cc5d8d42f55616aedfb84
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="part-3---setting-up-a-xamarin-cross-platform-solution"></a>Teil 3 – Einrichten einer Xamarin-Cross-Plattformlösung

Unabhängig davon, welche Plattformen verwendet werden, Xamarin-Projekte, die alle die gleiche Lösung-Dateiformat verwenden (Visual Studio **sln** Dateiformat). Lösungen können für entwicklungsumgebungen, freigegeben werden, selbst wenn einzelne Projekte verwenden (z. B. ein Windows-Projekt in Visual Studio für Mac) können nicht geladen werden.



Wenn Sie ein neues cross-Platform-Anwendung erstellen, ist der erste Schritt eine leere Projektmappe erstellt. In diesem Abschnitt werden, was daraufhin geschieht: Einrichten der Projekte zum Erstellen von plattformübergreifenden mobilen apps.

 <a name="Sharing_Code" />


## <a name="sharing-code"></a>Freigeben von Code

Finden Sie in der [Code Sharing Options](~/cross-platform/app-fundamentals/code-sharing.md) Dokument für eine ausführliche Beschreibung zum Freigeben von Code über Plattformen hinweg zu implementieren.

 <a name="Shared_Asset_Projects" />


### <a name="shared-projects"></a>Gemeinsam genutzte Projekte

Die einfachste Vorgehensweise zum Freigeben von Codedateien ist die Verwendung einer [freigegebenes Projekt](~/cross-platform/app-fundamentals/shared-projects.md).

Diese Methode können Sie den gleichen Code andere Plattform projektübergreifend freigeben und Compilerdirektiven verwenden, um verschiedene, plattformspezifische Codepfade zu einzuschließen.

 <a name="Portable_Class_Libraries" />


### <a name="portable-class-libraries-pcl"></a>Portable Klassenbibliotheken (PCL)

In der Vergangenheit wurde eine Projektdatei .NET (und die resultierende Assembly) auf eine spezifische Framework-Version vorgesehen. Dies verhindert, dass das Projekt oder die Assembly, die von verschiedenen Frameworks genutzt wird.

Eine Portable Klassenbibliothek (PCL) ist eine Sonderform des Projekts, das über verschiedene CLI-Plattformen wie Xamarin.iOS und Xamarin.Android, als auch WPF, universelle Windows-Plattform und Xbox hinweg verwendet werden kann. Die Bibliothek kann nur eine Teilmenge des vollständigen .NET Framework beschränkt, der als Zielframework verwendeten Plattformen nutzen.

Erfahren Sie mehr über die Xamarin [Unterstützung für Portable Klassenbibliotheken](~/cross-platform/app-fundamentals/pcl.md) und folgen Sie den Anweisungen angezeigt wie die [TaskyPortable Beispiel](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable) funktioniert.


### <a name="net-standard"></a>.NET-Standard

Eingeführt in 2016 [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md) Projekte bieten eine einfache Möglichkeit zum Freigeben von Code in der Plattformen, erzeugen Assemblys, die in Windows verwendet werden können, Xamarin-Plattformen (iOS, Android, Mac) und Linux.

.NET Standardbibliotheken können erstellt und wie PCLs, verwendet werden, außer dass die APIs in den einzelnen Versionen (von 1.0 auf 1.6) leichter ermittelt werden und jede Version abwärtskompatibel ist mit einer niedrigeren Versionsnummer erneut.



 <a name="Populating_the_Solution" />


## <a name="populating-the-solution"></a>Füllen die Projektmappe

Unabhängig davon, welche Methode zum Freigeben von Code verwendet wird sollten die allgemeine Lösungsstruktur eine mehrstufige Architektur implementieren, die Codefreigabe vertraut zu machen.
Der Xamarin-Ansatz ist die Gruppencode in zwei Projekttypen:

-   **Core-Projekt** – wieder verwendbare Code schreiben, an einem Ort, auf verschiedenen Plattformen gemeinsam genutzt werden. Verwenden Sie die Prinzipien der Kapselung, um Details zur Implementierung möglichst auszublenden.
-   **Clientplattform-spezifische Anwendungsprojekte** – die Schutzverfahren Code mit möglichst wenig Kopplung nutzen. Clientplattform-spezifische Funktionen sind auf dieser Ebene basiert auf Komponenten, die verfügbar gemacht werden, in die Core-Projekt hinzugefügt.


 <a name="Core_Project" />


### <a name="core-project"></a>Core Project

Freigegebene Codeprojekte sollten nur Assemblys verweisen, die auf allen Plattformen – d. h. zur Verfügung. die allgemeinen Framework-Namespaces wie `System`, `System.Core` und `System.Xml`.

Gemeinsam genutzte Projekte sollten viele nicht-UI-Funktionen wie möglich ist, wird die folgenden Ebenen und verlustrechnungen implementieren:

-   **Datenschicht** – Code, den physischen Datenspeicher z. B. übernimmt.  [SQLite-NET](https://github.com/praeclarum/sqlite-net), eine alternative Datenbank wie [Realm.io](https://realm.io/products/realm-mobile-database/) oder sogar XML-Dateien. Die Ebene Datenklassen sind normalerweise nur von der Datenzugriffsebene verwendet.
-   **Datenzugriffsebene** – definiert eine API, die die erforderlichen Datenvorgänge für die Funktionalität der Anwendung unterstützt, z. B. Methoden, um Zugriff auf Listen mit Daten, die Daten für die einzelnen Elemente und auch erstellen, bearbeiten und löschen Sie sie.
-   **Dienst-Datenbankzugriffsschicht** – eine optionale Ebene Bereitstellen von Cloud-Dienste an die Anwendung. Enthält Code, der remote-Netzwerkressourcen (Web Services, Bilddownloads usw.) greift auf und möglicherweise die Ergebnisse zwischenspeichern.
-   **Geschäftsebene** – Definition der das Modell und Fassade oder Manager Klassen, die Funktionalität der plattformspezifischen Anwendungen verfügbar zu machen.


 <a name="Platform-Specific_Application_Projects" />


### <a name="platform-specific-application-projects"></a>Clientplattform-spezifische-Anwendungsprojekten

Plattformspezifischen Projekte müssen auf jeder Plattform-SDK (Xamarin.iOS, Xamarin.Android, Xamarin.Mac oder Windows) sowie das Projekt mit freigegebenem Code Core binden benötigten Assemblys verweisen.

Die plattformspezifischen Projekte sollten implementieren:

-   **Anwendungsebene** – Plattform bestimmte Funktionen und Bindung/Konvertierung zwischen den Geschäftsebene-Objekten und der Benutzeroberfläche.
-   **Benutzeroberflächenebene** – Bildschirme, die benutzerdefinierte Benutzeroberflächen-Steuerelemente, die Darstellung von Validierungslogik.


<a name="Example" />


### <a name="example"></a>Beispiel

In diesem Diagramm wird die Anwendungsarchitektur veranschaulicht:

 [ ![](part-3-setting-up-a-xamarin-cross-platform-solution-images/conceptualarchitecture.png "In diesem Diagramm wird die Anwendungsarchitektur veranschaulicht.")](part-3-setting-up-a-xamarin-cross-platform-solution-images/conceptualarchitecture.png#lightbox)

Diese bildschirmabbildung zeigt ein Setup Lösung mit freigegebenen Core-Projekts, IOS- und Android-Anwendungsprojekten. Das freigegebene Projekt enthält Code in Zusammenhang mit jeder der Architekturebenen (Business "," Dienst "," Daten "und" Data Access-Code):

 ![](part-3-setting-up-a-xamarin-cross-platform-solution-images/core-solution-example.png "Das freigegebene Projekt enthält Code in Zusammenhang mit jeder der Architekturebenen (Business "," Dienst "," Daten "und" Data Access-Code)")


 <a name="Project_References" />


## <a name="project-references"></a>Projektverweise

Projektverweise wider, die Abhängigkeiten für ein Projekt. Core Projekte ihre Verweise auf Assemblys common zu beschränken, damit der Code einfach freigegeben ist.
Plattformspezifische Anwendungsprojekte verweisen auf den freigegebenen Code plus plattformspezifischen andere Assemblys, die müssen sie die Zielplattform nutzen.

Die Anwendungsprojekte jede freigegebenes Projekt verweisen, und den Benutzeroberflächen-Code erforderlich, um vorhanden Funktionalität für den Benutzer enthalten, wie im folgenden Screenshots gezeigt:

![](part-3-setting-up-a-xamarin-cross-platform-solution-images/solution-android.png "Die Anwendung projiziert, jeden Verweis freigegebenes Projekt") ![ ] (part-3-setting-up-a-xamarin-cross-platform-solution-images/solution-ios.png "die Anwendung projiziert, jeden Verweis freigegebenen Projekts")


Spezifische Beispiele für die Struktur von Projekten werden in der Fallstudien angegeben.

 <a name="Adding_Files" />


## <a name="adding-files"></a>Hinzufügen von Dateien

 <a name="Build_Action" />


### <a name="build-action"></a>Buildvorgang

Es ist wichtig, den richtigen Buildvorgang für bestimmte Dateitypen festzulegen. Diese Liste zeigt den Buildvorgang für einige gängige Dateitypen:

-  **Alle C#-Dateien** – Buildvorgang: Kompilieren
-   **Bilder in Xamarin.iOS w & indows-** – Buildvorgang: Inhalt
-   **XIB "und" Storyboard-Dateien in Xamarin.iOS** – Buildvorgang: SchnittstelleDefinition
-   **Bilder und AXML Layouts in Android** – Buildvorgang: AndroidResource
-  **XAML-Dateien im Windows-Projekte** – Buildvorgang: Seite "
-  **Xamarin.Forms XAML-Dateien** – Buildvorgang: EmbeddedResource


Im Allgemeinen die IDE erkennt den Dateityp und den richtigen Buildvorgang vorschlagen.

 <a name="Case_Sensitivity" />


### <a name="case-sensitivity"></a>Groß- und Kleinschreibung

Denken Sie daran, dass einige Plattformen (z. b. die Groß-/Kleinschreibung Dateisysteme haben
iOS und Android) Seien Sie sicher, dass einen konsistente Datei Benennungsstandard verwenden, und stellen Sie sicher, dass die Dateinamen enthalten, die Sie im Code verwenden, das Dateisystem genau übereinstimmen. Dies ist besonders wichtig für Bilder und andere Ressourcen, die Sie im Code zu verweisen.
