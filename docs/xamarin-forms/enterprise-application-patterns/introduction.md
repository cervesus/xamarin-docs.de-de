---
title: Einführung in die Entwicklung von Unternehmens-Apps
description: Dieses Kapitel bietet eine Einführung in Enterprise-app-Entwicklung, und führt die "eshoponcontainers" mobile app.
ms.prod: xamarin
ms.assetid: cbce0659-fa03-447a-86ec-140438143230
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 9deb685c92092ceb0e1c775a1e53ac1bce5a4a57
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61299979"
---
# <a name="introduction-to-enterprise-app-development"></a>Einführung in die Entwicklung von Unternehmens-Apps

Unabhängig von der Plattform konfrontiert Entwickler von Unternehmens-apps für verschiedene Herausforderungen:

-   App-Anforderungen, die im Laufe der Zeit ändern können.
-   Neue Geschäftschancen und Herausforderungen.
-   Kontinuierliches Feedback während der Entwicklung, die sich erheblich auf den Bereich und die Anforderungen der app auswirken kann.

Mit diesen Denken Sie daran ist es wichtig, um apps zu erstellen, die leicht geändert oder im Laufe der Zeit erweitert werden können. Entwerfen für solche Anpassungsfähigkeit kann schwierig sein, da es sich um eine Architektur, die einzelnen Teile der app werden unabhängig voneinander entwickelt und in Isolation getestet werden erfordert, ohne den Rest der app ermöglicht.

Viele unternehmensanwendungen sind ausreichend komplex ist, mehr als ein Entwickler erforderlich ist. Es kann sein, dass eine erhebliche Herausforderung dar, zu entscheiden, wie Sie eine app entwerfen, sodass mehrere Entwickler effektiv auf verschiedene Teile der app unabhängig voneinander arbeiten können und gleichzeitig sicherstellen, dass die einzelnen Teile miteinander nahtlos kommen, wenn in der app integriert.

Der herkömmliche Ansatz zum Entwickeln und erstellen eine app führt zu Was wird als bezeichnet ein *monolithische* -app, in denen Komponenten mit keine klare Trennung zwischen ihnen eng gekoppelt sind. In der Regel wird dieser monolithischen Ansatz führt zu apps, die schwierig und nicht effizient zu verwalten sind, da es schwierig, um Fehler zu beheben, ohne andere Komponenten in der app sein kann, und kann es schwierig, zum Hinzufügen neuer Funktionen oder Ersetzen der vorhandene Funktionen sein.

Ein wirksames Gegenmittel für diese Herausforderungen ist, eine app in diskrete, lose gekoppelte Komponenten aufzuteilen, die in eine app ganz einfach zusammen integriert werden können. Dieser Ansatz bietet mehrere Vorteile:

-   Sie können die einzelnen Funktionen entwickelt, getestet, erweitert und von verschiedenen Personen oder Teams verwaltet werden.
-   Es fördert die Wiederverwendung und eine klare Trennung von Anliegen zwischen der app horizontal Funktionen wie Authentifizierung und Datenzugriff, und die vertikale Funktionen, z. B. bestimmte Geschäftsfunktionalität app. Dadurch können die Abhängigkeiten und die Interaktionen zwischen Komponenten der app leichter verwaltet werden.
-   Sie können eine Trennung von Rollen zu verwalten, indem Sie ermöglicht die verschiedenen Personen oder Teams, auf einen bestimmten Task oder einen Teil der Funktionalität gemäß ihrer Kenntnisse konzentrieren. Insbesondere wird eine sauberere Trennung zwischen der Benutzeroberfläche und die Geschäftslogik der app.

Es gibt jedoch viele Probleme, die behoben werden müssen, wenn Sie eine app in diskrete, lose gekoppelte Komponenten. Dazu gehören:

-   Entscheiden, wie eine saubere Trennung von Bereichen zwischen den Steuerelementen der Benutzeroberfläche und ihre Logik bereitzustellen. Einer der wichtigsten Entscheidungen beim Erstellen einer Xamarin.Forms-Enterprise-app ist, ob Sie von Geschäftslogik im Code-Behind-Dateien platzieren, oder erstellen Sie eine klare Trennung von Anliegen zwischen der Steuerelemente der Benutzeroberfläche und ihre Logik, um die app mehr Stellen wartbarer und testbarer. Weitere Informationen finden Sie unter [Model-View-ViewModel](~/xamarin-forms/enterprise-application-patterns/mvvm.md).
-   Bestimmt, ob einen Container für Abhängigkeitsinjektion verwenden. Dependency Injection-Containern verringern die Abhängigkeit, die Kopplung zwischen Objekten durch die Bereitstellung einer Funktion zum Erstellen von Instanzen von Klassen mit den zugehörigen Abhängigkeiten eingefügt und deren Lebensdauer auf Basis der Konfiguration des Containers zu verwalten. Weitere Informationen finden Sie unter [Dependency Injection](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md).
-   Auswählen zwischen der Plattform bereitgestellten Ereignisse und lose gekoppelt nachrichtenbasierte Kommunikation zwischen Komponenten, die unpraktisch, Verknüpfen von Objekt und den Typ verweisen. Weitere Informationen finden Sie in der Einführung in die [Kommunikation zwischen lose gekoppelte Komponenten](~/xamarin-forms/enterprise-application-patterns/communicating-between-loosely-coupled-components.md).
-   Entscheiden, wie zum Navigieren zwischen Seiten, einschließlich Navigation, aufrufen und, in denen Navigationslogik befinden. Weitere Informationen finden Sie unter [Navigation](~/xamarin-forms/enterprise-application-patterns/navigation.md).
-   Bestimmen, wie Benutzereingaben auf Richtigkeit zu überprüfen. Die Entscheidung muss wie die Benutzereingabe überprüft, und zur Benachrichtigung des Benutzers zu Validierungsfehlern enthalten. Weitere Informationen finden Sie unter [Überprüfung](~/xamarin-forms/enterprise-application-patterns/validation.md).
-   Entscheiden, wie Sie die Authentifizierung durchzuführen, und Schützen von Ressourcen mit Autorisierung. Weitere Informationen finden Sie unter [Authentifizierung und Autorisierung](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md).
-   Bestimmen, wie Zugriff auf Remotedaten aus Web services, einschließlich zuverlässig Abrufen von Daten und das Zwischenspeichern von Daten. Weitere Informationen finden Sie unter [den Zugriff auf Remotedaten](~/xamarin-forms/enterprise-application-patterns/accessing-remote-data.md).
-   Entscheiden, wie die app zu testen. Weitere Informationen finden Sie unter [Unit Testing](~/xamarin-forms/enterprise-application-patterns/unit-testing.md).

Dieses Handbuch enthält Anleitungen zu diesen Problemen und konzentriert sich auf die wichtigsten Muster und die Architektur zum Erstellen einer plattformübergreifenden Enterprise-app mit Xamarin.Forms. Die Anweisungen, die zielt darauf ab, die dabei helfen, um anpassbaren, verwaltbaren und testbaren Code zu erstellen, durch die Adressierung von allgemeinen Xamarin.Forms-app-Entwicklung Unternehmensszenarios und trennen die Aspekte der Darstellung, Darstellungslogik und Entitäten, die durch die Unterstützung für die Model-View-ViewModel (MVVM)-Muster.

## <a name="sample-application"></a>Beispielanwendung

Dieses Handbuch enthält eine beispielanwendung eShopOnContainers, der eine online-Store ist, die Folgendes enthält:

-   Authentifizierung und Autorisierung gegenüber einem Back-End-Dienst.
-   Einen Katalog nach Shirts, kaffeebechern und anderen marketingartikeln Durchsuchen von.
-   Filtern des Katalogs an.
-   Anordnen von Elementen aus dem Katalog.
-   Anzeigen von Bestellverlauf des Benutzers ein.
-   Die Konfiguration der Einstellungen.

### <a name="sample-application-architecture"></a>Beispiel-Anwendungsarchitektur

Abbildung 1 – 1 bietet einen allgemeinen Überblick über die Architektur der beispielanwendung.

![](introduction-images/architecture.png "eShopOnContainers: Allgemeine Architektur")

**Abbildung 1-1**: Allgemeine Architektur von eShopOnContainers

Die beispielanwendung im Lieferumfang von drei Client-apps:

-   Eine MVC-Anwendung, die mit ASP.NET Core entwickelt wurden.
-   Eine Single-Page Application (SPA) entwickelt, Typescript mit Angular 2. Dieser Ansatz für Webanwendungen wird vermieden, einen Roundtrip zum Server mit jedem Vorgang ausführen.
-   Eine mobile app entwickelt, mit Xamarin.Forms, mit dem iOS, Android und die universelle Windows-Plattform (UWP) unterstützt.

Weitere Informationen über die Webanwendungen finden Sie unter [entwerfen und entwickeln moderner Webanwendungen mit ASP.NET Core und Microsoft Azure](http://aka.ms/WebAppEbook).

Die beispielanwendung enthält die folgenden Back-End-Dienste:

-   Ein Identitäts-Microservice, der ASP.NET Core Identity "und" IdentityServer verwendet.
-   Ein katalogmicroservice handelt es sich eine datengesteuerte erstellen, lesen, aktualisieren, löschen (CRUD)-Dienst, der eine SQL Server-Datenbank mithilfe von EntityFramework Core nutzt.
-   Ein Microservice für Bestellungen, einen Dienst Domain-driven, der ddd-Mustern verwendet.
-   Eine Warenkorb-Microservice, ein Data-driven-CRUD-Dienst, der redis Cache verwendet.

Diese Back-End-Dienste werden als Microservices mithilfe von ASP.NET Core MVC implementiert und als eindeutige Container innerhalb eines einzelnen Docker-Hosts bereitgestellt werden. Zusammen werden diese Back-End-Dienste bezeichnet wie der Bestellung von eShopOnContainers Anwendung verweisen. Client-apps mit den Back-End-Diensten über eine Weboberfläche Representational State Transfer (REST) kommunizieren. Weitere Informationen zu Microservices und Docker, finden Sie unter [Microservices in Containern](~/xamarin-forms/enterprise-application-patterns/containerized-microservices.md).

Weitere Informationen zur Implementierung der Back-End-Dienste, finden Sie unter [.NET Microservices: NET Microservices: Architecture for Containerized .NET Applications (.NET Microservices: Architektur für .NET-Containeranwendungen)](https://aka.ms/microservicesebook).

### <a name="mobile-app"></a>Mobile Apps

Dieses Handbuch konzentriert sich auf plattformübergreifende Unternehmens-apps, die mit Xamarin.Forms erstellen, und die "eshoponcontainers" mobile app als Beispiel dient. Abbildung 1-2 zeigt die Seiten werden aus der eShopOnContainers-mobile-app, die zuvor beschriebene Funktionalität bieten.

[![](introduction-images/screenshots.png "Die \"eshoponcontainers\" mobile app")](introduction-images/screenshots-large.png#lightbox "die eShopOnContainers-mobile-app")

**Abbildung 1 – 2**: Die eShopOnContainers-mobile-app

Die mobile app nutzt die Back-End-Dienste, die von der referenzanwendung "eshoponcontainers" bereitgestellt. Sie können jedoch konfiguriert werden, um Daten aus mock-Dienste für diejenigen zu nutzen, vermeiden Sie das Back-End-Dienste bereitstellen möchten.

Die eShopOnContainers-mobile-app führt die folgende Xamarin.Forms-Funktionen:

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

Weitere Informationen zu dieser Funktion finden Sie unter den [Xamarin.Forms Dokumentation](~/xamarin-forms/index.yml), und [Erstellen mobiler Apps mit Xamarin.Forms](https://aka.ms/xamebook).

Darüber hinaus werden die Komponententests für einige der Klassen in der eShopOnContainers-mobile-app bereitgestellt.

#### <a name="mobile-app-solution"></a>Mobile App-Lösung

Die mobile app-Projektmappe "eshoponcontainers" organisiert den Quellcode und andere Ressourcen in-Projekte. Alle Projekte mithilfe von Ordnern auf den Quellcode und andere Ressourcen in Kategorien organisieren. In der folgende Tabelle wird beschrieben, die Projekte, aus denen die eShopOnContainers-mobile-app:

|Projekt|Beschreibung|
|--- |--- |
|eShopOnContainers.Core|Dieses Projekt ist das portable Klassenbibliotheksprojekt (PCL) hinzu, das den freigegebenen Code und die freigegebene Benutzeroberfläche enthält.|
|eShopOnContainers.Droid|Dieses Projekt enthält Android-spezifischen Code und ist der Einstiegspunkt für die Android-app.|
|eShopOnContainers.iOS|Dieses Projekt enthält iOS-spezifischen Code und ist der Einstiegspunkt für die iOS-app.|
|eShopOnContainers.UWP|Dieses Projekt enthält universelle Windows-Plattform (UWP)-spezifischen Code und ist der Einstiegspunkt für die Windows-app.|
|eShopOnContainers.TestRunner.Droid|Dieses Projekt ist Android Test Runner für das Projekt eShopOnContainers.UnitTests.|
|eShopOnContainers.TestRunner.iOS|Dieses Projekt ist für das Projekt eShopOnContainers.UnitTests iOS Test Runner.|
|eShopOnContainers.TestRunner.Windows|Dieses Projekt ist das Testprogramm für die universelle Windows-Plattform für das Projekt eShopOnContainers.UnitTests.|
|eShopOnContainers.UnitTests|Dieses Projekt enthält Komponententests für das Projekt eShopOnContainers.Core.|

Die Klassen aus der mobilen app von eShopOnContainers können erneut verwendet, in dem alle Xamarin.Forms-app mit nur wenig oder gar keinen Änderungen sein.

##### <a name="eshoponcontainerscore-project"></a>eShopOnContainers.Core Projekt

EShopOnContainers.Core PCL-Projekt enthält die folgenden Ordner:

|Ordner|Beschreibung|
|--- |--- |
|Animationen|Enthält Klassen, mit die Animationen in XAML verwendet werden können.|
|Verhalten|Verhalten, die bereitgestellt werden, zum Anzeigen von Klassen enthält.|
|Steuerelemente|Benutzerdefinierte Steuerelemente, die von der Anwendung verwendeten enthält.|
|Typkonverter|Enthält Wertkonverter, die benutzerdefinierten Logik für eine Bindung zu gelten.|
|Effekte|Enthält die `EntryLineColorEffect` -Klasse, die verwendet wird, so ändern Sie die Farbe des Rahmens bestimmten `Entry` Steuerelemente.|
|Ausnahmen|Enthält die benutzerdefinierte `ServiceAuthenticationException`.|
|Erweiterungen|Enthält Erweiterungsmethoden für die `VisualElement` und `IEnumerable` Klassen.|
|Hilfsmethoden|Enthält Hilfsklassen für die app.|
|Modelle|Enthält die Modellklassen für die app an.|
|Eigenschaften|Enthält `AssemblyInfo.cs`, eine Metadatendatei der .NET-Assembly.|
|Dienste|Enthält Schnittstellen und Klassen, die Dienste zu implementieren, die für die app bereitgestellt werden.|
|Trigger|Enthält die `BeginAnimation` Trigger, die zum Aufrufen einer Animation in XAML verwendet wird.|
|Validierungen|Enthält Klassen, die bei der Überprüfung von Dateneingaben beteiligt.|
|ViewModels|Enthält die Anwendungslogik, die für Seiten verfügbar gemacht wird.|
|Ansichten|Enthält die Seiten für die app.|

##### <a name="platform-projects"></a>Plattform-Projekten

Die Plattformprojekte enthalten die Implementierungen der Effekt, benutzerdefinierten Renderer-Implementierungen und andere plattformspezifische Ressourcen.

## <a name="summary"></a>Zusammenfassung

Xamarin plattformübergreifende mobile app-Entwicklungstools und -Plattformen bieten eine umfassende Lösung für B2E, B2B und B2C mobilen Client-apps bietet die Möglichkeit zum Teilen von Code für alle Plattformen (iOS, Android und Windows) und was dazu beiträgt, senken die Gesamtbetriebskosten. Apps können die Benutzeroberfläche und Logik Benutzercode, und behalten Sie das Aussehen und Verhalten von nativen Plattform nutzen.

Entwickler von Unternehmens-apps stehen vor verschiedene Herausforderungen, die während der Entwicklung die Architektur der app ändern können. Aus diesem Grund ist es wichtig, um eine app erstellen, damit sie geändert oder im Laufe der Zeit erweitert werden kann. Entwerfen für solche Anpassungsfähigkeit kann schwierig sein, aber in der Regel umfasst partitionieren eine app in diskrete, lose gekoppelte Komponenten, die in eine app ganz einfach miteinander integriert werden können.


## <a name="related-links"></a>Verwandte Links

- [E-Book (2Mb PDF-Datei) herunterladen](https://aka.ms/xamarinpatternsebook)
- ["eshoponcontainers" (GitHub) (Beispiel)](https://github.com/dotnet-architecture/eShopOnContainers)
