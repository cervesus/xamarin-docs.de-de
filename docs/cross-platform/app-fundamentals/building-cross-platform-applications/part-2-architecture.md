---
title: Teil 2 - Architektur
ms.topic: article
ms.prod: xamarin
ms.assetid: 2176DB2D-E84A-3757-CFAB-04A586068D50
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/27/2017
ms.openlocfilehash: 25cd374ef2b3026f5ac7242ee076c8593ce806da
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="part-2---architecture"></a>Teil 2 - Architektur

Ein wesentlicher Grundsatz von Erstellen von plattformübergreifenden apps besteht darin, eine Architektur erstellen, die für eine Maximierung des Codes, die alle Plattformen gemeinsam geradezu anbietet. Einhaltung der folgenden Prinzipien Objekt Oriented Programming build unterstützt eine gut entwickelte Anwendung:

-   **Kapselung** – sicherstellen, dass Klassen und sogar Architekturebenen nur eine minimale API verfügbar machen, die ihre erforderlichen führt Funktionen und blendet die Implementierungsdetails. Auf der Ebene einer Klasse bedeutet dies, dass Objekte, die als "schwarzes Kästchen" Verhalten sich und Nutzung von Code muss nicht wissen, wie sie ihre Aufgaben erledigen. Unter einer Architekturebene bedeutet dies, Implementieren von Mustern, z. B. Fassade, die eine vereinfachte API zu empfehlen, die komplexere Interaktionen für den Code in abstrakter Ebenen organisiert. Dies bedeutet, dass der Code der Benutzeroberfläche (z. B.) nur für die Anzeige von Bildschirmen und Benutzereingaben akzeptieren verantwortlich sein sollte; und nie direkt mit der Datenbank interagiert. Auf ähnliche Weise sollten der Datenzugriffs-Code nur lesen und Schreiben in die Datenbank, jedoch nie direkte Interaktion mit den Schaltflächen oder Bezeichnungen.
-   **Trennung von Aufgaben** – stellen Sie sicher, dass jede Komponente (beide zur Architektur und der Klassen-Ebene) hat einen klare und klar definierten Zweck. Jede Komponente sollte nur seine definierten Vorgänge ausführen und diese Funktionalität über eine API, die für die anderen Klassen zugänglich ist, die sie benötigen verfügbar machen.
-   **Polymorphismus** – Programmierung auf eine Schnittstelle (oder eine abstrakte Klasse), die mehrere Implementierungen bedeutet unterstützt, Kerncode plattformübergreifend freigegeben wird, bei der Interaktion weiterhin mit Clientplattform-spezifische Funktionen und geschrieben werden kann.


Das natürliche Ergebnis ist eine Anwendung, die nach der realen Welt oder abstrakte Entitäten mit separaten logischen Ebenen modelliert. Trennen Code in Ebenen stellen Anwendungen einfacher zu verstehen, zu testen und zu verwalten. Es wird empfohlen, dass der Code in jeder Ebene physisch getrennt (entweder in die Verzeichnisse oder sogar getrennte Projekte für sehr große Anwendungen) als auch logisch getrennt (Verwenden von Namespaces) sein.

 <a name="Typical_Application_Layers" />


## <a name="typical-application-layers"></a>Typische Anwendungsebenen

In diesem Dokument und die Fallstudien finden wir die folgenden sechs Anwendungsebenen:

-   **Datenschicht** – Non-Volatile-Datenpersistenz, wahrscheinlich eine SQLite-Datenbank jedoch mit XML-Dateien oder durch einen anderen geeigneten Mechanismus implementiert werden konnte.
-   **Datenzugriffsebene** – Wrapper um die Datenschicht, die bereitstellt, Create, Read, Update, löschen (CRUD) Zugriff auf die Daten ohne Implementierungsdetails an den Aufrufer verfügbar zu machen. Z. B. die DAL möglicherweise SQL-Anweisungen zum Abfragen oder aktualisieren Sie die Daten enthalten, aber der verweisende Code nicht dadurch wissen müssen.
-   **Geschäftsebene** – (manchmal auch als Business Logic Layer oder BLL) Business Entitätsdefinitionen (Modell) und Geschäftslogik enthält. Kandidat für BusinessRules-Muster.
-   **Dienst-Datenbankzugriffsschicht** – wird verwendet, um in der Cloud-Dienste: von komplexen Web Services (REST, JSON, WCF) zum einfachen Abruf von Daten und Bilder von Remoteservern. Kapselt die Netzwerke Verhalten, und bietet eine einfache API, die von der Anwendung und Benutzeroberfläche Ebenen verwendet werden.
-   **Anwendungsebene** – Code, der in der Regel plattformspezifisch (nicht in der Regel über Plattformen hinweg freigegeben) ist oder Codes, bezieht sich auf die Anwendung (im Allgemeinen nicht wiederverwendbar). Ein guter Test, ob Code in der Anwendungsschicht im Vergleich zu den Benutzeroberflächenebene platziert ist (a) können Sie feststellen, ob die Klasse tatsächliche Anzeigesteuerelemente verfügt oder (b) gibt an, ob es mehrere Bildschirme oder Geräte (z. b. gemeinsam genutzt werden kann iPhone und iPad).
-   **Benutzer-Benutzeroberfläche (UI)-Ebene** – die Ebene benutzerseitige enthält Bildschirme, Widgets und Controller verwaltet werden.


Eine Anwendung möglicherweise nicht unbedingt alle Ebenen – z. B. die Dienstebene für den Zugriff in einer Anwendung, die nicht den Zugriff auf Netzwerkressourcen ist nicht vorhanden wäre. Eine sehr einfache Anwendung möglicherweise die Datenschicht und die Datenzugriffsebene zusammengeführt werden, da die Vorgänge äußerst einfache sind.

 <a name="Common_Mobile_Software_Patterns" />


## <a name="common-mobile-software-patterns"></a>Häufige Muster von Software für Mobile Geräte

Muster sind eine eingerichtete Weg, um wiederkehrende Lösungen für gängige Probleme zu erfassen. Es gibt einige Muster, die sich bei der Erstellung von verwaltbarem/verständlich mobiler Anwendungen vertraut machen.

-   **Modell, Ansicht ViewModel (MVVM)** – The Model-View-ViewModel-Muster wird häufig mit Frameworks, die Unterstützung der Datenbindung, z. B. Xamarin.Forms. Es wurde von XAML-fähigen SDKs wie Windows Presentation Foundation (WPF) und Silverlight wurde; Das ViewModel, in dem als Vermittler zwischen den Daten (Modell) und die Benutzeroberfläche (Ansicht) über die Datenbindung und Befehle fungiert.
-   **Modell, Ansicht Controller (MVC)** – ein Muster allgemeine und häufig missverstanden MVC werden am häufigsten verwendet werden, beim Erstellen von Benutzeroberflächen und bietet eine Trennung zwischen der eigentlichen Klassendefinition ein UI-Bildschirm (Ansicht), das Modul dahinter, die behandelt Interaktion (Domänencontroller), und die Daten, die (Modell) aufgefüllt. Das Modell ist tatsächlich ein vollständig optionalen Teil und daher der Kern Verständnis dieses Muster liegt in der Ansicht und Controller. MVC ist eine beliebte Ansatz für iOS-Anwendungen.
-   **Für BusinessRules** – AKA Manager Muster bietet einen vereinfachten Zugriffspunkt für komplexe Aufgabe. Z. B. in einer Anwendung Task verfolgen Sie möglicherweise eine `TaskManager` Klasse mit Methoden wie z. B. `GetAllTasks()` , `GetTask(taskID)` , `SaveTask (task)` usw. Die `TaskManager` -Klasse bietet eine Fassade auf der internen Funktionsweise von tatsächlich speichern/Abrufen von Aufgaben-Objekten.
-   **Singleton** – die Singleton-Muster bietet eine Möglichkeit, in denen nur eine einzelne Instanz eines bestimmten Objekts kann jemals vorhanden sein. Beispielsweise soll bei Verwendung von SQLite in mobilen Anwendungen immer nur eine Instanz des Datenbankmoduls. Verwenden die Singleton-Muster ist eine einfache Möglichkeit, dies sicherzustellen.
-   **Anbieter** – ein Muster prägte von Microsoft (wohl ähnlich wie Strategie oder grundlegende Abhängigkeitsinjektion) zur Förderung der Wiederverwendung von Code für Silverlight, WPF- und WinForms-Anwendungen. Freigegebene Code geschrieben werden kann, für eine Schnittstelle oder eine abstrakte Klasse, und plattformspezifischen konkrete Implementierungen werden geschrieben, und übergeben, wenn der Code verwendet wird.
-   **Asynchrone** – nicht zu verwechseln mit der Async-Schlüsselwort, das asynchrone Muster verwendet wird, wenn lang ausgeführte Aufgabe werden, ohne die Benutzeroberfläche oder die aktuelle Verarbeitung aufhält ausgeführt muss. In der einfachsten Form beschreibt das asynchrone Muster einfach, dass lang andauernden Aufgaben in einer anderen Releasebuild werden Thread (oder ähnliche Thread Abstraktion z. B. ein Task) während der aktuelle Thread die Verarbeitung fortsetzt und wartet auf eine Antwort von dem im Hintergrund ausgeführten Prozess , und aktualisiert dann die Benutzeroberfläche aus, wenn Daten und oder ein Status zurückgegeben wird.


Jedes der Muster wird im Detail, wie ihre praktische Verwendung in der Fallstudien dargestellt wird, untersucht werden. Wikipedia enthält weitere ausführliche Beschreibungen der [MVVM](https://en.wikipedia.org/wiki/Model–view–viewmodel), [MVC](https://en.wikipedia.org/wiki/Model–view–controller), [Fassaden](http://en.wikipedia.org/wiki/Facade_pattern), [Singleton](http://en.wikipedia.org/wiki/Singleton_pattern), [Strategie](http://en.wikipedia.org/wiki/Strategy_pattern)und [Anbieter](http://en.wikipedia.org/wiki/Provider_model) Muster (und der [Entwurfsmuster](http://en.wikipedia.org/wiki/Design_Patterns) im Allgemeinen).
