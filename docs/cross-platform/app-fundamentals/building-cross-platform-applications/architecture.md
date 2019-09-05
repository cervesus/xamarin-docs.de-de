---
title: 'Teil 2: Architektur'
description: In diesem Dokument werden Architekturmuster beschrieben, die zum entwickeln plattformübergreifender Anwendungen hilfreich sind. Es werden typische Anwendungsebenen (Datenschicht, Datenzugriffs Schicht usw.) und gängige Mobile Software Muster (MVVM, MVC usw.) erläutert.
ms.prod: xamarin
ms.assetid: 2176DB2D-E84A-3757-CFAB-04A586068D50
author: conceptdev
ms.author: crdun
ms.date: 03/27/2017
ms.openlocfilehash: 7657985ce14633140adb0e63a9817ddd0e48841d
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70284571"
---
# <a name="part-2---architecture"></a>Teil 2: Architektur

Ein wichtiger Grund für die Erstellung plattformübergreifender Apps ist das Erstellen einer Architektur, die sich für eine Maximierung der plattformübergreifenden Code Freigabe eignet. Die Einhaltung der folgenden objektorientierten Programmier Prinzipien hilft dabei, eine gut entworfene Anwendung zu erstellen:

- **Kapselung** – sicherstellen, dass Klassen und sogar architektonische Schichten nur eine minimale API verfügbar machen, die die erforderlichen Funktionen ausführt, und die Implementierungsdetails ausblenden. Auf Klassenebene bedeutet dies, dass Objekte sich als "schwarze Felder" Verhalten, und dass der verarbeitende Code nicht wissen muss, wie Sie Ihre Aufgaben ausführen. Auf Architekturebene bedeutet das, dass Muster wie Fassaden implementiert werden, die eine vereinfachte API fördern, die komplexere Interaktionen im Namen des Codes in abstrakter Ebene orchestriert. Dies bedeutet, dass der UI-Code (z. b.) nur für die Anzeige von Bildschirmen und die Annahme von Benutzereingaben verantwortlich sein sollte. und niemals direkt mit der Datenbank interagieren. Ebenso sollte der Datenzugriffs Code nur Lese-und Schreibzugriff auf die Datenbank, aber niemals direkt mit Schaltflächen oder Bezeichnungen interagieren.
- **Trennung von Zuständigkeiten** – stellen Sie sicher, dass jede Komponente (sowohl auf Architektur-als auch auf Klassenebene) einen klaren und klar definierten Zweck hat. Jede Komponente sollte nur die definierten Aufgaben ausführen und diese Funktionalität über eine API verfügbar machen, auf die die anderen Klassen, die Sie verwenden müssen, zugreifen können.
- **Polymorphie** – das Programmieren in eine Schnittstelle (oder eine abstrakte Klasse), die mehrere Implementierungen unterstützt, bedeutet, dass der Kerncode plattformübergreifend geschrieben und freigegeben werden kann, während er weiterhin mit plattformspezifischen Features interagiert


Das natürliche Ergebnis ist eine Anwendung, die nach realen oder abstrakten Entitäten mit separaten logischen Ebenen modelliert wird. Durch die Trennung von Code in Ebenen werden Anwendungen leichter zu verstehen, zu testen und zu warten. Es wird empfohlen, den Code in jeder Schicht physisch voneinander zu trennen (entweder in Verzeichnissen oder sogar separaten Projekten für sehr große Anwendungen) und logisch getrennt (mit Namespaces).

 <a name="Typical_Application_Layers" />


## <a name="typical-application-layers"></a>Typische Anwendungsschichten

In diesem Dokument und den Fallstudien verweisen wir auf die folgenden sechs Anwendungsebenen:

- **Datenschicht** – nicht flüchtige Daten Persistenz, wahrscheinlich eine SQLite-Datenbank, aber mit XML-Dateien oder einem anderen geeigneten Mechanismus implementiert werden.
- **Datenzugriffs Schicht** – ein Wrapper um die Datenschicht, die den Zugriff auf die Daten (Create, Read, Update, DELETE, CRUD) auf die Daten ermöglicht, ohne dem Aufrufer Implementierungsdetails verfügbar zu machen. Beispielsweise kann die DAL SQL-Anweisungen enthalten, um die Daten abzufragen oder zu aktualisieren, aber der verweisende Code muss dies nicht wissen.
- Die Geschäfts **Schicht** – (manchmal auch als Geschäftslogik Schicht oder BLL bezeichnet) enthält Geschäfts Entitäts Definitionen (das Modell) und die Geschäftslogik. Candidate for Business-Fassaden Muster.
- **Dienst Zugriffs Schicht** – wird für den Zugriff auf Dienste in der Cloud verwendet: von komplexen Webdiensten (Rest, JSON, WCF) bis hin zum einfachen Abrufen von Daten und Images von Remote Servern. Kapselt das Netzwerk Verhalten und bietet eine einfache API, die von der Anwendungs-und UI-Schicht verwendet werden soll.
- **Anwendungsschicht** – Code, der in der Regel plattformspezifisch ist (in der Regel nicht plattformübergreifend freigegeben) oder Code, der spezifisch für die Anwendung ist (nicht allgemein wiederverwendbar) Ein guter Test, ob Code in die Anwendungsschicht und die UI-Schicht platziert werden soll, ist (a), um zu bestimmen, ob die Klasse über tatsächliche Anzeige Steuerelemente oder (b), ob Sie von mehreren Bildschirmen oder Geräten gemeinsam genutzt werden kann (z. b. iPhone und iPad).
- **Benutzeroberflächen Ebene** – die Benutzer seitige Ebene, enthält Bildschirme, Widgets und die Controller, die Sie verwalten.


Eine Anwendung darf nicht unbedingt alle Ebenen enthalten – beispielsweise ist die Dienst Zugriffs Schicht nicht in einer Anwendung vorhanden, die nicht auf Netzwerkressourcen zugreift. Eine sehr einfache Anwendung kann die Datenschicht und die Datenzugriffs Ebene zusammenführen, da die Vorgänge äußerst einfach sind.

 <a name="Common_Mobile_Software_Patterns" />


## <a name="common-mobile-software-patterns"></a>Gängige Mobile Software Muster

Muster sind eine bewährte Methode, um wiederkehrende Lösungen für häufige Probleme zu erfassen. Es gibt einige wichtige Muster, die beim Aufbau verwaltbarer/verständlicher mobiler Anwendungen hilfreich sind.

- **Model, View, ViewModel (MVVM)** – das Model-View-ViewModel-Muster ist beliebt bei Frameworks, die Datenbindung unterstützen, z. b. xamarin. Forms. Sie wurde durch XAML-aktivierte sdche wie Windows Presentation Foundation (WPF) und Silverlight populär. Dabei fungiert ViewModel durch Datenbindung und Befehle als zwischen den Daten (Modell) und Benutzeroberfläche (Ansicht).
- **Model, View, Controller (MVC)** – ein gängiges und häufig falsch verstandenes Muster, wird MVC am häufigsten beim Aufbau von Benutzeroberflächen verwendet und bietet eine Trennung zwischen der tatsächlichen Definition eines Bildschirm der Benutzeroberfläche (Ansicht), der dahinter liegenden Engine, die die Interaktion verarbeitet ( Controller) und die Daten, die Sie Auffüllen (Modell). Das Modell ist tatsächlich ein vollkommen optionales Element. Daher liegt der Kern des Verständnisses dieses Musters in der Ansicht und dem Controller. MVC ist ein beliebter Ansatz für IOS-Anwendungen.
- **Business Fassaden** – auch als Manager-Muster bezeichnet, stellt einen vereinfachten Einstiegspunkt für komplexe Aufgaben bereit. Beispielsweise können Sie in einer Aufgaben nach Verfolgungs Anwendung über `TaskManager` eine-Klasse mit Methoden `GetAllTasks()` wie, `GetTask(taskID)` , `SaveTask (task)` usw. verfügen. Die `TaskManager` -Klasse stellt eine Fassade für die innere Funktionsweise des eigentlichen Speicherns/Abrufens von Aufgaben Objekten bereit.
- **Singleton** – das Singleton-Muster stellt eine Methode bereit, mit der nur eine einzelne Instanz eines bestimmten Objekts vorhanden sein kann. Wenn Sie beispielsweise sqlite in mobilen Anwendungen verwenden, benötigen Sie nur eine Instanz der Datenbank. Die Verwendung des Singleton-Musters ist eine einfache Möglichkeit, dies sicherzustellen.
- **Anbieter** – ein Muster, das von Microsoft (wohl vergleichbar mit der Strategie oder grundlegender Abhängigkeitsinjektion) geprägt ist, um die Wiederverwendung von Code für Silverlight-, WPF-und WinForms-Anwendungen zu fördern. Gemeinsam verwendeter Code kann für eine Schnittstelle oder eine abstrakte Klasse geschrieben werden, und plattformspezifische konkrete Implementierungen werden bei der Verwendung des Codes geschrieben und weitergegeben.
- **Async** – nicht zu verwechseln mit dem Async-Schlüsselwort, wird das asynchrone Muster verwendet, wenn Arbeitsaufgaben mit langer Laufzeit ausgeführt werden müssen, ohne die Benutzeroberfläche oder die aktuelle Verarbeitung zu halten. In der einfachsten Form beschreibt das Async-Muster einfach, dass Aufgaben mit langer Ausführungszeit in einem anderen Thread (oder einer ähnlichen Thread Abstraktion, z. b. einer Aufgabe) gestartet werden sollen, während der aktuelle Thread weiterhin verarbeitet und auf eine Antwort vom Hintergrundprozess lauscht. , und aktualisiert dann die Benutzeroberfläche, wenn Daten und oder Status zurückgegeben werden.


Jedes der Muster wird ausführlicher untersucht, da die praktische Verwendung in den Fallstudien veranschaulicht wird. Wikipedia bietet ausführlichere Beschreibungen der Muster [MVVM](https://en.wikipedia.org/wiki/Model–view–viewmodel), [MVC](https://en.wikipedia.org/wiki/Model–view–controller), [Fassaden](https://en.wikipedia.org/wiki/Facade_pattern), [Singleton](https://en.wikipedia.org/wiki/Singleton_pattern), [Strategie](https://en.wikipedia.org/wiki/Strategy_pattern) und [Anbieter](https://en.wikipedia.org/wiki/Provider_model) (und in der Regel [Entwurfsmuster](https://en.wikipedia.org/wiki/Design_Patterns) ).
