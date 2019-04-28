---
title: 'Teil 2: Architektur'
description: Dieses Dokument beschreibt die Architekturmuster, die zum Erstellen von plattformübergreifenden Anwendungen hilfreich. Es wird erläutert, typische Anwendungsschichten (Datenschicht, Datenzugriffsebene, usw.) und häufige Muster von Software für mobile Geräte (MVVM, MVC, usw.).
ms.prod: xamarin
ms.assetid: 2176DB2D-E84A-3757-CFAB-04A586068D50
author: asb3993
ms.author: amburns
ms.date: 03/27/2017
ms.openlocfilehash: cfb2bddceea7717ac87bd7a78fd9cd45e8b93144
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61285142"
---
# <a name="part-2---architecture"></a>Teil 2: Architektur

Ein wesentlicher Grundsatz beim Erstellen von plattformübergreifenden apps ist die Erstellung eine Architektur, die zu einer Maximierung der Freigabe für Benutzeroberflächencode führt. Die folgenden Prinzipien zur objektorientierten Programmierung Einhaltung erstellen unterstützt eine gut entwickelte Anwendung:

-   **Kapselung** – sicherstellen, dass Funktionen, Klassen und sogar Architekturebenen nur eine minimale API verfügbar gemacht, die die erforderlichen ausführt, und blendet die Details der Implementierung. Auf Klassenebene bedeutet dies, dass Objekte, die als "Blackbox" Verhalten sich und Nutzen von Code, nicht wissen unbedingt, wie sie ihre Aufgaben erledigen. Unter einer Architekturebene bedeutet dies, Implementieren von Mustern wie Fassade, die eine vereinfachte API zu ermutigen, die jedoch komplexe Interaktionen für den Code in etwas abstraktere Ebenen organisiert. Dies bedeutet, dass es sich bei der UI-Code (z. B.) nur für die Anzeige von Bildschirmen und Benutzereingaben akzeptieren verantwortlich sein sollte. und niemals direkt mit der Datenbank interagieren. Auf ähnliche Weise sollte der Datenzugriffscode nur lesen und Schreiben in die Datenbank, aber keine direkte Interaktion mit den Schaltflächen und Bezeichnungen.
-   **Trennung von Zuständigkeiten** – stellen Sie sicher, dass jede Komponente (beides Architektur und Ebene) hat einen klare und klar definierten Zweck. Jede Komponente sollte nur die definierten Aufgaben auszuführen und verfügbar machen diese Funktionalität über eine API, die auf die anderen Klassen zugegriffen werden kann, die sie verwenden müssen.
-   **Polymorphismus** – Programmierung auf eine Schnittstelle (oder eine abstrakte Klasse), das bedeutet, dass mehrere Implementierungen unterstützt wird, dass die Core-Code geschrieben und Plattformen, bei der immer noch Interaktion mit plattformspezifischen Features gemeinsam genutzt werden kann.


Das natürliche Ergebnis ist eine Anwendung, die nach der realen Welt oder abstrakte Entitäten mit verschiedenen logischen Schichten modelliert. Trennen Code in Schichten stellen Anwendungen einfacher zu verstehen, zu testen und zu verwalten. Es wird empfohlen, dass der Code in jeder Schicht physisch getrennte (entweder in die Verzeichnisse oder sogar getrennte Projekte für sehr große Anwendungen) sowie logisch getrennt (Verwenden von Namespaces) sein.

 <a name="Typical_Application_Layers" />


## <a name="typical-application-layers"></a>Typische Anwendungsschichten

In diesem Dokument und Fallstudien beziehen wir uns auf die folgenden sechs Anwendungsschichten:

-   **Datenschicht** – Non-Volatile-Datenpersistenz, wahrscheinlich eine SQLite-Datenbank jedoch mit XML-Dateien oder eine andere geeignete Methode implementiert werden können.
-   **Daten von Zugriffsschicht** -Wrapper für die Datenschicht, die bereitstellt, Create, Read, Update, Delete (CRUD), auf die Daten ohne Implementierungsdetails, die an den Aufrufer verfügbar zu machen. Z. B. die DAL kann SQL-Anweisungen zum Abfragen oder aktualisieren Sie die Daten enthalten, aber der verweisende Code nicht, dies zu wissen müssen.
-   **Geschäftsschicht** – (manchmal auch als Business Logic Layer oder BLL) enthält Definitionen für geschäftliche Entitäten (das Modell) und Geschäftslogik. Kandidat für Business-Fassade-Muster.
-   **-Dienst von Zugriffsschicht** – wird verwendet, um in der Cloud-Dienste: von komplexen Web Services (REST, JSON, WCF) auf den einfachen Abruf von Daten und Bilder von Remoteservern. Kapselt die Netzwerke-Verhalten, und bietet eine einfache API zur Nutzung durch die Anwendung und UI-Ebenen.
-   **Anwendungsschicht** – Code, der in der Regel plattformspezifisch (nicht in der Regel über Plattformen hinweg freigegeben) oder code, bezieht sich auf die Anwendung (im Allgemeinen nicht wiederverwendbar). Ein guter Test, der angibt, ob Code in der Anwendungsschicht und der UI-Ebene zu platzieren ist (a) zu bestimmen, ob die Klasse für alle Steuerelemente angezeigt hat, oder (b) gibt an, ob es zwischen mehreren Bildschirmen oder Geräte (z. b. gemeinsam genutzt werden können iPhone und iPad).
-   **Benutzeroberfläche (UI) der Benutzerebene** – der Ebene die benutzerseitigen enthält Bildschirme, Widgets und die Controller verwalten diese.


Eine Anwendung kann nicht notwendigerweise alle Ebenen enthalten – die Dienstebene für den Zugriff ist beispielsweise nicht vorhanden, in einer Anwendung, die nicht auf Netzwerkressourcen zugreift. Eine sehr einfache Anwendung kann die Datenschicht und die Daten von Zugriffsschicht zusammengeführt werden, weil die Vorgänge, extrem einfache sind.

 <a name="Common_Mobile_Software_Patterns" />


## <a name="common-mobile-software-patterns"></a>Häufige Muster von Software für Mobile Geräte

Muster sind eine etablierte Möglichkeit zum sich wiederholenden Lösungen für häufige Probleme zu erfassen. Es gibt einige wichtige Muster, die hilfreich, bei der Erstellung von verwaltbarem/verständlich, mobile Anwendungen zu verstehen sind.

-   **Modell, Ansicht, ViewModel (MVVM)** – das Model-View-ViewModel-Muster wird häufig mit Frameworks, die die Datenbindung, z. B. Xamarin.Forms zu unterstützen. Es wurde von XAML-fähigen SDKs wie Windows Presentation Foundation (WPF) und Silverlight bekannt gemacht; fungiert, in denen das "ViewModel" als Vermittler zwischen den Daten (Modell) und die Benutzeroberfläche (Ansicht) über die Datenbindung und Befehle ein.
-   **Modell, Ansicht, Controller (MVC)** – ein Muster häufig und häufig missverstanden MVC wird am häufigsten verwendet werden, beim Erstellen von Benutzeroberflächen und bietet eine Trennung zwischen der eigentlichen Klassendefinition einen Bildschirm der Benutzeroberfläche (Ansicht), die Engine dahinter, die verarbeitet Interaktion (Controller), und die Daten, die (Modell) aufgefüllt. Das Modell ist tatsächlich eine völlig optional, und daher der Kern der Verständnis dieses Muster liegt in der Ansicht und Controller. MVC ist ein beliebter Ansatz für iOS-Anwendungen.
-   **Business-Fassade** – auch bekannt als Muster "Manager", bietet einen vereinfachten Zugangspunkt für komplexe Aufgaben. Z. B. in einer Anwendung Nachverfolgen der Aufgabe, Sie müssen möglicherweise eine `TaskManager` Klasse mit Methoden wie z. B. `GetAllTasks()` , `GetTask(taskID)` , `SaveTask (task)` usw. Die `TaskManager` Klasse stellt eine Fassade, die interne Funktionsweise von tatsächlich speichern/Abrufen von Aufgaben-Objekte bereit.
-   **Singleton** – die Singleton-Muster bietet eine Möglichkeit, in denen nur eine einzelne Instanz eines bestimmten Objekts kann jemals vorhanden sein. Z. B. bei Verwendung von SQLite in mobilen Anwendungen Sie immer nur eine Instanz der Datenbank. Verwenden das Singleton-Muster ist eine einfache Möglichkeit, dies sicherzustellen.
-   **Anbieter** – ein Muster Abstufung von Microsoft (wohl vergleichbar mit der Strategie und grundlegende Dependency Injection) Wiederverwendungsrate für Code, die in Silverlight, WPF und WinForms-Anwendungen zu fördern. Freigegebener Code geschrieben werden kann, für eine Schnittstelle oder abstrakte Klasse, und plattformspezifische konkrete Implementierungen werden geschrieben, und übergeben werden, wenn der Code verwendet wird.
-   **Asynchrone** – nicht zu verwechseln mit dem Async-Schlüsselwort, das asynchrone Muster verwendet wird, wenn langfristig ausgeführte Arbeit werden, ohne die Benutzeroberfläche oder die aktuelle Verarbeitung aufzuhalten ausgeführt muss. In seiner einfachsten Form beschreibt das asynchrone Muster einfach, dass lang andauernde Vorgänge in einer anderen gestartet werden, sollten Thread (oder ähnliche Threads-Abstraktion, z. B. ein Task), während der aktuelle Thread weiter verarbeitet und wartet auf eine Antwort von der im Hintergrund ausgeführten Prozess , und klicken Sie dann die Benutzeroberfläche aktualisiert, wenn es sich bei Daten und Zustand zurückgegeben.


Jedes der Muster wird im Detail, wie die praktische Verwendung in den Fallstudien dargestellt wird, untersucht werden. Wikipedia hat ausführlichere Beschreibungen der [MVVM](https://en.wikipedia.org/wiki/Model–view–viewmodel), [MVC](https://en.wikipedia.org/wiki/Model–view–controller), [Fassaden](https://en.wikipedia.org/wiki/Facade_pattern), [Singleton](https://en.wikipedia.org/wiki/Singleton_pattern), [Strategie](https://en.wikipedia.org/wiki/Strategy_pattern)und [Anbieter](https://en.wikipedia.org/wiki/Provider_model) Muster (und der [Entwurfsmuster](https://en.wikipedia.org/wiki/Design_Patterns) im Allgemeinen).
