---
title: Einführung in Enterprise-App-Entwicklung
description: Dieses Kapitel bietet eine Einführung in Enterprise-app-Entwicklung und die mobilen Anwendung für eShopOnContainers führt.
ms.prod: xamarin
ms.assetid: cbce0659-fa03-447a-86ec-140438143230
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 9deb685c92092ceb0e1c775a1e53ac1bce5a4a57
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242959"
---
# <a name="introduction-to-enterprise-app-development"></a>Einführung in Enterprise-App-Entwicklung

Unabhängig von Plattform müssen Entwickler von Unternehmens-apps beinhaltet einige Herausforderungen stellen:

-   App-Anforderungen, die mit der Zeit ändern können.
-   Neue Geschäftschancen und Herausforderungen annehmen können.
-   Kontinuierliches Feedback während der Entwicklung, die den Umfang und die app-Anforderungen erheblich beeinflussen können.

Mit diesen Bedenken ist es wichtig, um apps zu erstellen, die leicht geändert oder mit der Zeit erweitert werden können. Entwerfen für solche Anpassungsfähigkeit kann schwierig sein, benötigt wird, eine Architektur, die einzelnen Teile der app zu unabhängig entwickelt und in Isolation getestet werden, ohne den Rest der app ermöglicht.

Viele unternehmensapps sind komplex, mehr als ein Entwickler erforderlich ist. Es kann eine erhebliche Herausforderung zu entscheiden, wie eine Anwendung so entwerfen, dass mehrere Entwickler effektiv auf andere Teile der app unabhängig voneinander arbeiten können und gleichzeitig sicherstellen, dass die einzelnen Teile zusammen nahtlos kommen, wenn die Anwendung integriert werden.

Der herkömmliche Ansatz für das Entwerfen und Erstellen einer app-Ergebnissen in Was wird als bezeichnet eine *monolithischen* app, wobei Komponenten eng mit keine klaren Unterschied zwischen diesen verbunden sind. In der Regel wird dieser monolithischen Ansatz führt zu einer apps, die schwierig und ineffizient zu verwalten sind, da es schwierig, Fehler zu beheben, ohne Unterbrechung von anderen Komponenten in der app sein kann, und kann es schwierig, neue Funktionen hinzufügen oder Ersetzen von vorhandenen Funktionen sein.

Eine effektive Rechtsmittel dieser Herausforderung besteht darin, eine app in diskrete, lose miteinander verknüpfte Komponenten zu partitionieren, die in eine app leicht miteinander integriert werden können. Ein derartiger Ansatz bietet mehrere Vorteile:

-   Sie können die einzelnen Funktionen entwickelt, getestet, erweitert und von verschiedenen Personen oder Teams verwaltet werden.
-   Stuft Wiederverwendung und eine klare Trennung von Anliegen zwischen horizontale der app-Funktionen, wie beispielsweise Authentifizierung und Datenzugriff, und die vertikale Funktionen, z. B. bestimmte Geschäftsfunktionalität app. Dies ermöglicht die Abhängigkeiten und Interaktionen zwischen Komponenten der app leichter verwaltet werden.
-   Es wird eine Trennung von Rollen zu verwalten, können von verschiedenen Personen oder Teams, um auf einen bestimmten Task oder einen Teil der Funktionalität gemäß ihrer Kenntnisse zu konzentrieren. Insbesondere stellt er eine sauberere Trennung zwischen der Benutzeroberfläche und Geschäftslogik für die app bereit.

Es gibt jedoch viele Probleme, die bei der Partitionierung einer app in diskrete, lose miteinander verknüpfte Komponenten behoben werden müssen. Dazu gehören:

-   Entscheiden, wie eine klare Trennung von Anliegen zwischen den Benutzeroberflächen-Steuerelemente und ihre Logik bereitzustellen. Einer der wichtigsten Entscheidungen für die Erstellung einer Xamarin.Forms-Unternehmens-app ist, ob Sie die Geschäftslogik im Code-Behind-Dateien platzieren oder erstellen Sie eine klare Trennung von Anliegen zwischen den Benutzeroberflächen-Steuerelemente und ihre Logik, um die app mehrere Stellen verwaltet werden und getestet werden können. Weitere Informationen finden Sie unter [Model-View-ViewModel](~/xamarin-forms/enterprise-application-patterns/mvvm.md).
-   Bestimmen, ob einen abhängigkeitseinschleusungscontainer verwendet. Dependency Injection-Containern verringern die Abhängigkeit zwischen Objekten durch Bereitstellen eine Funktion zum Erstellen von Instanzen von Klassen mit ihren Abhängigkeiten eingeschleust Kopplung, und Verwalten ihrer Lebensdauer auf Basis der Konfiguration des Containers. Weitere Informationen finden Sie unter [Abhängigkeitsinjektion](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md).
-   Auswählen zwischen Plattform bereitgestellten Ereignisse und lose gekoppelt meldungsbasierte Kommunikation zwischen Komponenten, die unpraktisch, Objekt und Typverweise verknüpft sind. Weitere Informationen finden Sie in der Einführung in die [Kommunikation zwischen lose gekoppelte Komponenten](~/xamarin-forms/enterprise-application-patterns/communicating-between-loosely-coupled-components.md).
-   Entscheiden, wie zum Navigieren zwischen Seiten, einschließlich Informationen zum Aufrufen der Navigation und auf dem sich die Navigationslogik befinden sollten. Weitere Informationen finden Sie unter [Navigation](~/xamarin-forms/enterprise-application-patterns/navigation.md).
-   Gewusst wie: Überprüfen von Benutzereingaben auf Richtigkeit wird ermittelt. Die Entscheidung muss zum Überprüfen von Benutzereingaben und wie die Benachrichtigung des Benutzers zu Überprüfungsfehlern enthalten. Weitere Informationen finden Sie unter [Überprüfung](~/xamarin-forms/enterprise-application-patterns/validation.md).
-   Entscheiden, wie zum Durchführen der Authentifizierung und zum Schützen von Ressourcen mit Autorisierung. Weitere Informationen finden Sie unter [Authentifizierung und Autorisierung](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md).
-   Auf welche Weise Zugriff auf Remotedaten von Web services, einschließlich zuverlässig Abrufen von Daten und das Zwischenspeichern von Daten. Weitere Informationen finden Sie unter [Zugriff auf Remotedaten](~/xamarin-forms/enterprise-application-patterns/accessing-remote-data.md).
-   Entscheiden, wie die app zu testen. Weitere Informationen finden Sie unter [UnitTests](~/xamarin-forms/enterprise-application-patterns/unit-testing.md).

Dieses Handbuch enthält Anweisungen für diese Probleme und konzentriert sich auf die Core-Muster und die Architektur für die Erstellung einer plattformübergreifenden Enterprise-app, die mit Xamarin.Forms. Die Anleitung zielt darauf ab, die verhindern Code anwendbare, verwaltbare und getestet werden können, behandeln Sie häufige Xamarin.Forms Enterprise app Entwicklungsszenarien durch die Trennung der Bedenken hinsichtlich der Präsentation Präsentationslogik und Entitäten über Unterstützung für erzeugen die Model-View-ViewModel (MVVM)-Muster.

## <a name="sample-application"></a>Beispielanwendung

Dieses Handbuch enthält eine beispielanwendung, eShopOnContainers, die einem Onlineshop enthält die folgende Funktionen:

-   Authentifizierung und Autorisierung gegenüber einem Back-End-Dienst.
-   Durchsuchen einen Katalog von Hemden Kaffeetassen und andere marketing Elemente.
-   Filtern den Katalog.
-   Anordnen von Elementen aus dem Katalog.
-   Anzeigen von Bestellverlauf des Benutzers ein.
-   Konfiguration der Einstellungen.

### <a name="sample-application-architecture"></a>Beispiel-Anwendungsarchitektur

Abbildung 1: 1 stellt einen allgemeinen Überblick über die Architektur der beispielanwendung bereit.

![](introduction-images/architecture.png "grundlegende Architektur von eShopOnContainers")

**Abbildung 1: 1**: eShopOnContainers allgemeine Architektur

Die beispielanwendung enthält drei Client-apps:

-   Eine MVC-Anwendung mit ASP.NET Core entwickelt wurde.
-   Eine einzelne Seite Anwendung (SPA) entwickelt mit Angular 2 und Typescript. Dieser Ansatz für Webanwendungen vermeidet einen Roundtrip zum Server mit jedem Vorgang ausführen.
-   Eine mobile app entwickelt mit Xamarin.Forms, mit dem iOS, Android und die universelle Windows-Plattform (UWP) unterstützt.

Informationen über die Webanwendungen finden Sie unter [Architecting und entwickeln moderner Webanwendungen mit ASP.NET Core und Microsoft Azure](http://aka.ms/WebAppEbook).

Die beispielanwendung enthält die folgenden Back-End-Dienste:

-   Ein Microservice Identität, die ASP.NET Core Identitäts- und IdentityServer verwendet.
-   Lesen Sie ein Microservice Katalog, d. h. Erstellen eines datengesteuerten, aktualisieren Sie und löschen Sie (CRUD)-Dienst, der eine SQL Server-Datenbank mithilfe von EntityFramework Core nutzt.
-   Eine Sortierung Microservice, die einen Dienst Domain driven ist Domain driven Design-Muster verwendet.
-   Ein Microservice Warenkorb, also einen datengesteuerten CRUD-Dienst, der Redis-Cache verwendet.

Diese Back-End-Dienste werden als Microservices mithilfe von ASP.NET Core MVC implementiert und als eindeutigen Container innerhalb eines einzelnen Docker-Hosts bereitgestellt werden. Zusammen werden diese Back-End-Dienste zu bezeichnet, wie die eShopOnContainers Anwendung verweisen. Client-apps kommunizieren mit den Back-End-Diensten über eine Weboberfläche Representational State Transfer (REST). Weitere Informationen zu Microservices und Docker finden Sie unter [Containern Microservices](~/xamarin-forms/enterprise-application-patterns/containerized-microservices.md).

Informationen zur Implementierung der Back-End-Dienste finden Sie unter [.NET Microservices: Architektur für .NET-Anwendungen in Containern](https://aka.ms/microservicesebook).

### <a name="mobile-app"></a>Verwaltungsrichtlinien für Mobile Apps

Dieses Handbuch konzentriert sich auf das Erstellen von plattformübergreifenden Unternehmens-apps, die mithilfe von Xamarin.Forms und die eShopOnContainers mobilen app als Beispiel verwendet. Abbildung 1 bis 2 zeigt die Seiten werden aus der mobilen Anwendung für eShopOnContainers, die die oben aufgeführten Funktionen bereitstellen.

[![](introduction-images/screenshots.png "Die mobile app eShopOnContainers")](introduction-images/screenshots-large.png#lightbox "der eShopOnContainers mobilen app")

**Abbildung 1 bis 2**: der eShopOnContainers mobilen app

Die mobile Anwendung nutzt die Back-End-Dienste, die von der eShopOnContainers Verweis Anwendung bereitgestellt. Es kann jedoch konfiguriert werden, um Daten aus den simulierten Dienste für diejenigen zu nutzen, die nicht das Back-End-Dienste bereitstellen möchten.

Die mobilen Anwendung für eShopOnContainers führt die folgende Xamarin.Forms-Funktionen:

-   XAML
-   Steuerelemente
-   Bindungen
-   Typkonverter
-   Stile
-   Animationen
-   Befehle
-   Verhalten
-   Trigger
-   Effekte
-   Benutzerdefinierte Renderer
-   MessagingCenter
-   Benutzerdefinierte Steuerelemente

Weitere Informationen zu dieser Funktionalität finden Sie unter der [Xamarin.Forms Dokumentation](~/xamarin-forms/index.yml), und [Erstellen mobiler Apps mit Xamarin.Forms](https://aka.ms/xamebook).

Darüber hinaus werden die Komponententests für einige der Klassen in der mobilen Anwendung für eShopOnContainers bereitgestellt.

#### <a name="mobile-app-solution"></a>Mobile App-Projektmappe

Die eShopOnContainers mobile app-Projektmappe organisiert den Quellcode und andere Ressourcen in Projekten an. Alle Projekte verwenden Ordner, um Quellcode und andere Ressourcen in Kategorien zu organisieren. Die folgende Tabelle führt die Projekte, die die mobile app eShopOnContainers bilden:

|Projekt|Beschreibung|
|--- |--- |
|eShopOnContainers.Core|Dieses Projekt ist dem Projekt portablen Klassenbibliothek (PCL), das die freigegebenen Code und eine gemeinsame Benutzeroberfläche enthält.|
|eShopOnContainers.Droid|Dieses Projekt enthält Android bestimmten Code und ist der Einstiegspunkt für die Android-app.|
|eShopOnContainers.iOS|Dieses Projekt enthält iOS bestimmten Code und ist der Einstiegspunkt für die iOS-app.|
|eShopOnContainers.UWP|Dieses Projekt enthält die universelle Windows-Plattform (UWP) bestimmten Code und ist der Einstiegspunkt für die Windows-app.|
|eShopOnContainers.TestRunner.Droid|Dieses Projekt ist die Android-Testprogramm für das Projekt eShopOnContainers.UnitTests.|
|eShopOnContainers.TestRunner.iOS|Dieses Projekt ist das iOS-Testprogramm für das Projekt eShopOnContainers.UnitTests.|
|eShopOnContainers.TestRunner.Windows|Dieses Projekt ist die universelle Windows-Plattform-Testprogramm für das Projekt eShopOnContainers.UnitTests.|
|eShopOnContainers.UnitTests|Dieses Projekt enthält Komponententests für das Projekt eShopOnContainers.Core.|

Die Klassen aus der mobilen app eShopOnContainers können in jeder app Xamarin.Forms mit geringfügigen oder ohne Änderungen erneut verwendet werden.

##### <a name="eshoponcontainerscore-project"></a>eShopOnContainers.Core Projekt

EShopOnContainers.Core PCL-Projekt enthält die folgenden Ordner:

|Ordner|Beschreibung|
|--- |--- |
|Animationen|Enthält Klassen, mit denen Animationen in XAML verwendet werden soll.|
|Verhalten|Enthält die Verhaltensweisen, die verfügbar gemacht werden, um Klassen anzuzeigen.|
|Steuerelemente|Benutzerdefinierte Steuerelemente, die von der Anwendung verwendeten enthält.|
|Typkonverter|Enthält Wertkonverter, die benutzerdefinierte Logik auf eine Bindung anzuwenden.|
|Effekte|Enthält die `EntryLineColorEffect` Klasse, die verwendet wird, so ändern Sie die Farbe des Rahmens bestimmten `Entry` Steuerelemente.|
|Ausnahmen|Enthält die benutzerdefinierte `ServiceAuthenticationException`.|
|Erweiterungen|Enthält die Erweiterungsmethoden für die `VisualElement` und `IEnumerable` Klassen.|
|Hilfsmethoden|Enthält Hilfsklassen für die app an.|
|Modelle|Enthält die Modellklassen für die app.|
|Eigenschaften|Enthält `AssemblyInfo.cs`, einen .NET Assembly-Metadatendatei.|
|Dienste|Enthält Schnittstellen und Klassen, die Implementierung von Diensten, die für die app bereitgestellt werden.|
|Trigger|Enthält die `BeginAnimation` Trigger, die zum Aufrufen einer Animation in XAML verwendet wird.|
|Validierungen|Enthält Klassen, die bei der Überprüfung von Dateneingaben beteiligt.|
|ViewModels|Enthält die Anwendungslogik, die für Seiten verfügbar gemacht wird.|
|Ansichten|Enthält die Seiten für die app an.|

##### <a name="platform-projects"></a>Plattformprojekte

Die Plattformprojekte enthalten Effekt Implementierungen, benutzerdefinierten Renderers Implementierungen und andere plattformspezifische Ressourcen.

## <a name="summary"></a>Zusammenfassung

Xamarin Entwicklungstools für plattformübergreifende mobile Apps und Plattformen bieten eine umfassende Lösung für B2E B2B und B2C mobilen Client-apps bietet die Möglichkeit, Code für alle Zielplattformen (iOS, Android und Windows) und helfen senken Freigeben der Gesamtbetriebskosten. Apps können ihre Benutzer-Schnittstelle und die app Code für Geschäftslogik, freigeben, und die systemeigene Plattform Aussehen und Verhalten beibehalten.

Entwickler von Unternehmens-apps sind mehrere Herausforderungen, die die Architektur der app während der Entwicklung ändern kann. Aus diesem Grund ist es wichtig, eine app erstellen, sodass es geändert oder mit der Zeit erweitert werden kann. Entwerfen für solche Anpassungsfähigkeit kann schwierig sein, aber in der Regel umfasst das Partitionieren von einer app in diskrete, lose miteinander verknüpfte Komponenten, die problemlos zusammen in einer Anwendung integriert werden können.


## <a name="related-links"></a>Verwandte Links

- [Download-e-Book (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (Beispiel)](https://github.com/dotnet-architecture/eShopOnContainers)
