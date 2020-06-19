---
title: Einführung in die Entwicklung von Unternehmensanwendungen
description: Dieses Kapitel bietet eine Einführung in die Entwicklung von Unternehmens-apps und führt die Mobile App eshoponcontainers ein.
ms.prod: xamarin
ms.assetid: cbce0659-fa03-447a-86ec-140438143230
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e78e7c3056d4f1e22114819f54c1df261aec70e1
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84198120"
---
# <a name="introduction-to-enterprise-app-development"></a>Einführung in die Entwicklung von Unternehmensanwendungen

Unabhängig von der Plattform stehen Entwicklern von Unternehmens-apps verschiedene Herausforderungen zur Seite:

- App-Anforderungen, die sich mit der Zeit ändern können.
- Neue Geschäftschancen und Herausforderungen.
- Fortlaufendes Feedback während der Entwicklung, das den Umfang und die Anforderungen der APP erheblich beeinflussen kann.

Vor diesem Hintergrund ist es wichtig, apps zu erstellen, die im Laufe der Zeit problemlos geändert oder erweitert werden können. Der Entwurf für diese Anpassbarkeit kann schwierig sein, da hierfür eine Architektur erforderlich ist, mit der einzelne Teile der APP unabhängig entwickelt und isoliert getestet werden können, ohne dass sich dies auf den Rest der APP auswirkt.

Viele Unternehmens-apps sind ausreichend komplex, um mehr als einen Entwickler benötigen. Es kann eine bedeutende Herausforderung sein, zu entscheiden, wie eine APP entworfen werden soll, sodass mehrere Entwickler unabhängig voneinander an verschiedenen Teilen der APP arbeiten können und gleichzeitig sicherstellen, dass die Teile nahtlos zusammenkommen, wenn Sie in die APP integriert werden.

Die herkömmliche Vorgehensweise beim Entwerfen und entwickeln einer APP führt dazu, dass Sie als *monolithische* App bezeichnet wird, wobei die Komponenten eng miteinander verbunden sind und keine klare Trennung zwischen den Komponenten besteht. Der monolithische Ansatz führt in der Regel zu apps, die schwierig und ineffizient zu verwalten sind, da es schwierig sein kann, Fehler zu beheben, ohne andere Komponenten in der APP zu unterbrechen. es kann schwierig sein, neue Funktionen hinzuzufügen oder vorhandene Features zu ersetzen.

Eine effektive Lösung für diese Herausforderungen besteht darin, eine app in diskrete, locker gekoppelte Komponenten zu partitionieren, die problemlos in eine APP integriert werden können. Ein solcher Ansatz bietet mehrere Vorteile:

- Sie ermöglicht das entwickeln, testen, erweitern und pflegen einzelner Funktionen durch verschiedene Personen oder Teams.
- Es fördert die Wiederverwendung und eine saubere Trennung von Anliegen zwischen den horizontalen Funktionen der APP, wie z. b. Authentifizierung und Datenzugriff, und den vertikalen Funktionen wie der APP-spezifischen Geschäfts Funktionalität. Dadurch können die Abhängigkeiten und Interaktionen zwischen App-Komponenten leichter verwaltet werden.
- Dadurch wird die Trennung von Rollen gewährleistet, indem verschiedene Personen oder Teams den Schwerpunkt auf eine bestimmte Aufgabe oder eine bestimmte Funktionalität entsprechend ihrer Fachkenntnisse haben. Insbesondere wird eine saubere Trennung zwischen der Benutzeroberfläche und der Geschäftslogik der App ermöglicht.

Es gibt jedoch viele Probleme, die gelöst werden müssen, wenn eine app in diskrete, locker gekoppelte Komponenten partitioniert wird. Dazu gehören:

- Die Entscheidung, wie Sie eine saubere Trennung von Anliegen zwischen den Steuerelementen der Benutzeroberfläche und ihrer Logik bereitstellen. Eine der wichtigsten Entscheidungen bei der Erstellung einer Xamarin.Forms Unternehmens-App besteht darin, ob Sie Geschäftslogik in Code Behind-Dateien platzieren oder ob Sie eine saubere Trennung der Belange zwischen den Steuerelementen der Benutzeroberfläche und der Logik erstellen möchten, damit die APP besser verwaltbar und testbar ist. Weitere Informationen finden Sie unter [Model-View-ViewModel](~/xamarin-forms/enterprise-application-patterns/mvvm.md).
- Bestimmen, ob ein Abhängigkeits Injektions Container verwendet werden soll. Abhängigkeits einschleusungs Container reduzieren die Abhängigkeits Kopplung zwischen Objekten, indem eine Möglichkeit bereitgestellt wird, Klassen Instanzen mit ihren Abhängigkeiten zu erstellen und ihre Lebensdauer basierend auf der Konfiguration des Containers zu verwalten. Weitere Informationen finden Sie unter [Abhängigkeitsinjektion](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md).
- Die Auswahl zwischen der Plattform stellt eine ereignisbasierte und lose gekoppelte Nachrichten basierte Kommunikation zwischen Komponenten bereit, die für die Verknüpfung durch Objekt-und Typverweise ungeeignet sind. Weitere Informationen finden Sie unter Einführung in die [Kommunikation zwischen lose gekoppelten Komponenten](~/xamarin-forms/enterprise-application-patterns/communicating-between-loosely-coupled-components.md).
- Entscheiden, wie zwischen Seiten navigiert werden soll, einschließlich des Aufrufs der Navigation und der Position, an der sich Navigations Logik befinden sollte. Weitere Informationen finden Sie unter [Navigation](~/xamarin-forms/enterprise-application-patterns/navigation.md).
- Festlegen, wie Benutzereingaben auf Richtigkeit überprüft werden. Die Entscheidung muss einschließen, wie Benutzereingaben überprüft werden, und wie der Benutzer über Validierungs Fehler benachrichtigt wird. Weitere Informationen finden Sie unter [Validierung](~/xamarin-forms/enterprise-application-patterns/validation.md).
- Entscheiden, wie die Authentifizierung durchgeführt werden soll und wie Ressourcen mit Autorisierung geschützt werden. Weitere Informationen finden Sie unter [Authentifizierung und Autorisierung](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md).
- Festlegen des Zugriffs auf Remote Daten von Webdiensten, einschließlich des zuverlässigen abzurufenden von Daten und Zwischenspeichern von Daten. Weitere Informationen finden Sie unter [zugreifen auf Remote Daten](~/xamarin-forms/enterprise-application-patterns/accessing-remote-data.md).
- Entscheiden, wie die APP getestet werden soll. Weitere Informationen finden Sie unter [Komponententests](~/xamarin-forms/enterprise-application-patterns/unit-testing.md).

Dieses Handbuch enthält Anleitungen zu diesen Problemen und konzentriert sich auf die Kern Muster und-Architektur zum Entwickeln einer plattformübergreifenden Unternehmens-App mit Xamarin.Forms . Die Anleitung soll dabei helfen, anpassbaren, verwaltbaren und testbaren Code zu schaffen, indem allgemeine Xamarin.Forms Szenarien für die Entwicklung von Unternehmensanwendungen behandelt werden, und durch die Unterstützung für das Model-View-ViewModel (MVVM)-Muster unterstützt wird.

## <a name="sample-application"></a>Beispielanwendung

Dieses Handbuch enthält eine Beispielanwendung, eshoponcontainers, einen Online Shop, der die folgenden Funktionen umfasst:

- Authentifizieren und autorialisieren bei einem Back-End-Dienst.
- Durchsuchen eines Katalogs von Hemden, Kaffee Mugs und anderen Marketing Elementen.
- Filtern des Katalogs.
- Anordnen von Elementen aus dem Katalog.
- Anzeigen des Auftragsverlaufs des Benutzers.
- Konfiguration von Einstellungen.

### <a name="sample-application-architecture"></a>Beispiel Anwendungsarchitektur

In Abbildung 1-1 wird eine allgemeine Übersicht über die Architektur der Beispielanwendung bereitstellt.

![](introduction-images/architecture.png "eShopOnContainers high-level architecture")

**Abbildung 1-1**: eshoponcontainers-Architektur auf hoher Ebene

Die Beispielanwendung wird mit drei Client-apps ausgeliefert:

- Eine mit ASP.net Core entwickelte MVC-Anwendung.
- Eine Single-Page-Anwendung (Spa), die mit Angular 2 und typescript entwickelt wurde. Bei diesem Ansatz für Webanwendungen wird vermieden, dass ein Roundtrip zum Server mit jedem Vorgang durchgeführt wird.
- Eine Mobile App mit entwickelt Xamarin.Forms , die IOS, Android und die universelle Windows-Plattform (UWP) unterstützt.

Weitere Informationen zu den Webanwendungen finden Sie unter Entwerfen [und entwickeln moderner Webanwendungen mit ASP.net Core und Microsoft Azure](https://aka.ms/WebAppEbook).

Die Beispielanwendung enthält die folgenden Back-End-Dienste:

- Ein Identitäts--mikrodienst, der ASP.net Core Identity und identityserver verwendet.
- Ein Katalog-mikrodienst, bei dem es sich um einen datengesteuerten Create-, Read-, Update-und DELETE-Dienst (CRUD) handelt, der mithilfe von EntityFramework Core eine SQL Server Datenbank verwendet.
- Ein Domänen Dienst, bei dem es sich um einen domänengesteuerten Dienst handelt, der Domänen gesteuerte Entwurfsmuster verwendet.
- Ein Warenkorb-microservice, bei dem es sich um einen datengesteuerten CRUD-Dienst handelt, der redis Cache verwendet.

Diese Back-End-Dienste werden als-Dienste mit ASP.net Core MVC implementiert und als eindeutige Container innerhalb eines einzelnen docker-Hosts bereitgestellt. Gemeinsam werden diese Back-End-Dienste als eshoponcontainers-Verweis Anwendung bezeichnet. Client-apps kommunizieren über eine Rest (Representational State Transfer)-Webschnittstelle mit den Back-End-Diensten. Weitere Informationen zu microservices und docker finden Sie unter [microservices in Containern](~/xamarin-forms/enterprise-application-patterns/containerized-microservices.md).

Weitere Informationen zur Implementierung der Back-End-Dienste finden Sie unter [.net-microservices: Architektur für .NET-Container Anwendungen](https://aka.ms/microservicesebook).

### <a name="mobile-app"></a>Mobile App

Dieser Leitfaden konzentriert sich auf das entwickeln plattformübergreifender Unternehmens-Apps mithilfe von Xamarin.Forms und verwendet die Mobile App "eshoponcontainers" als Beispiel. Abbildung 1-2 zeigt die Seiten aus den eshoponcontainers-Mobile App, die die zuvor beschriebenen Funktionen bereitstellen.

[![](introduction-images/screenshots.png "The eShopOnContainers mobile app")](introduction-images/screenshots-large.png#lightbox "The eShopOnContainers mobile app")

**Abbildung 1-2**: die eshoponcontainers-Mobile App

Der Mobile App nutzt die Back-End-Dienste, die von der eshoponcontainers-Referenz Anwendung bereitgestellt werden Sie kann jedoch so konfiguriert werden, dass Sie Daten von mockdiensten für diejenigen nutzt, die die Bereitstellung der Back-End-Dienste vermeiden möchten.

Die eshoponcontainers-Mobile App führt die folgenden Xamarin.Forms Funktionen aus:

- XAML
- Steuerelemente
- Bindungen
- Konver
- Stile
- Animationen
- Befehle
- Verhalten
- Trigger
- Effekte
- Benutzerdefinierte Renderer
- MessagingCenter
- Benutzerdefinierte Steuerelemente

Weitere Informationen zu dieser Funktion finden Sie in der [ Xamarin.Forms Dokumentation](~/xamarin-forms/index.yml)und im Thema zum [Erstellen Xamarin.Forms von Mobile Apps mit ](https://aka.ms/xamformsebook).

Außerdem werden Komponententests für einige der Klassen in den eshoponcontainers-Mobile App bereitgestellt.

#### <a name="mobile-app-solution"></a>Mobile App-Lösung

Die eshoponcontainers-Mobile App Lösung organisiert den Quellcode und andere Ressourcen in-Projekten. Alle Projekte verwenden Ordner, um den Quellcode und andere Ressourcen in Kategorien zu organisieren. In der folgenden Tabelle werden die Projekte beschrieben, aus denen sich die eshoponcontainers-Mobile App zusammenführen:

|Project|Beschreibung|
|--- |--- |
|eshoponcontainers. Core|Dieses Projekt ist das Projekt der portablen Klassenbibliothek (Portable Class Library, PCL), das den freigegebenen und den freigegebenen Benutzeroberflächen|
|eshoponcontainers. Droid|Dieses Projekt enthält Android-spezifischen Code und ist der Einstiegspunkt für die Android-App.|
|eshoponcontainers. IOS|Dieses Projekt enthält IOS-spezifischen Code und ist der Einstiegspunkt für die IOS-app.|
|eshoponcontainers. UWP|Dieses Projekt enthält universelle Windows-Plattform (UWP) spezifischen Code und ist der Einstiegspunkt für die Windows-app.|
|eshoponcontainers. testrauunner. Droid|Dieses Projekt ist der Android-Test Runner für das Projekt eshoponcontainers. Unittests.|
|eshoponcontainers. testrauunner. IOS|Dieses Projekt ist der IOS-Test Runner für das Projekt eshoponcontainers. Unittests.|
|eshoponcontainers. testrauunner. Windows|Dieses Projekt ist der universelle Windows-Plattform Test Runner für das Projekt eshoponcontainers. Unittests.|
|eshoponcontainers. Unittests|Dieses Projekt enthält Komponententests für das Projekt eshoponcontainers. Core.|

Die Klassen aus dem eshoponcontainers-Mobile App können in jeder Xamarin.Forms App mit geringfügigen oder ohne Änderungen wieder verwendet werden.

##### <a name="eshoponcontainerscore-project"></a>eshoponcontainers. Core-Projekt

Das PCL-Projekt "eshoponcontainers. Core" enthält die folgenden Ordner:

|Ordner|BESCHREIBUNG|
|--- |--- |
|Animationen|Enthält Klassen, die die Verwendung von Animationen in XAML ermöglichen.|
|Verhalten|Enthält Verhalten, die für Ansichts Klassen verfügbar gemacht werden.|
|Steuerelemente|Enthält von der APP verwendete benutzerdefinierte Steuerelemente.|
|Konver|Enthält Wert Konverter, die benutzerdefinierte Logik auf eine Bindung anwenden.|
|Effekte|Enthält die- `EntryLineColorEffect` Klasse, die verwendet wird, um die Rahmenfarbe bestimmter Steuerelemente zu ändern `Entry` .|
|Ausnahmen|Enthält die benutzerdefinierten `ServiceAuthenticationException` .|
|Extensions|Enthält Erweiterungs Methoden für die `VisualElement` -Klasse und die- `IEnumerable` Klasse.|
|Hilfsmethoden|Enthält Hilfsklassen für die app.|
|Modelle|Enthält die Modellklassen für die app.|
|Eigenschaften|Enthält `AssemblyInfo.cs` , eine .net-assemblymetadatendatei.|
|Dienste|Enthält Schnittstellen und Klassen, mit denen Dienste implementiert werden, die für die APP bereitgestellt werden.|
|Trigger|Enthält den- `BeginAnimation` Aufruf, mit dem eine Animation in XAML aufgerufen wird.|
|Überprüfungen|Enthält Klassen, die zum Validieren von Dateneingaben beteiligt sind.|
|ViewModels|Enthält die Anwendungslogik, die für Seiten verfügbar gemacht wird.|
|Sichten|Enthält die Seiten für die app.|

##### <a name="platform-projects"></a>Platt Form Projekte

Die Platt Form Projekte enthalten Effekt Implementierungen, benutzerdefinierte rendererimplementierungen und andere plattformspezifische Ressourcen.

## <a name="summary"></a>Zusammenfassung

Die plattformübergreifenden Mobile App Entwicklungs Tools und-Plattformen von xamarin bieten eine umfassende Lösung für Mobile B2E-, B2B-und B2C-Client-apps, die die Möglichkeit bieten, Code auf allen Zielplattformen (Ios, Android und Windows) gemeinsam zu nutzen und die Gesamtbetriebskosten zu senken. Apps können Ihre Benutzeroberfläche und ihren App-Logik Code freigeben und gleichzeitig das Aussehen und Gefühl der nativen Plattform beibehalten.

Entwickler von Unternehmens-apps stellen verschiedene Herausforderungen dar, die die Architektur der APP während der Entwicklung ändern können. Daher ist es wichtig, eine APP zu erstellen, damit Sie im Laufe der Zeit geändert oder erweitert werden kann. Der Entwurf für diese Anpassbarkeit kann schwierig sein, aber es umfasst in der Regel die Partitionierung einer APP in diskrete, lose gekoppelte Komponenten, die problemlos in eine APP integriert werden können.

## <a name="related-links"></a>Verwandte Links

- [Download-e-Book (2 MB PDF)](https://aka.ms/xamarinpatternsebook)
- [eshoponcontainers (GitHub) (Beispiel)](https://github.com/dotnet-architecture/eShopOnContainers)
