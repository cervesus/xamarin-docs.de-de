---
title: Verwenden Xamarin.Forms-e-Book Enterprise-Anwendungsmuster
description: "Architekturrichtlinien für die Entwicklung von Xamarin.Forms-unternehmensanwendungen anwendbare, verwaltbare und getestet werden können"
ms.topic: article
ms.prod: xamarin
ms.assetid: 28cfed6c-6175-4223-a8cc-798d40bf0832
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 7ed546ac975ce1956d94d509486e4cfb25d28100
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="enterprise-application-patterns-using-xamarinforms-ebook"></a>Verwenden Xamarin.Forms-e-Book Enterprise-Anwendungsmuster

_Architekturrichtlinien für die Entwicklung von Xamarin.Forms-unternehmensanwendungen anwendbare, verwaltbare und getestet werden können_

![](images/cover-sml.png "Verwenden Xamarin.Forms-e-Book Enterprise-Anwendungsmuster")

Diese e-Book enthält Anleitungen zum Implementieren von der Model-View-ViewModel (MVVM)-Muster, Abhängigkeitsinjektion, Navigation, Validierung und konfigurationsverwaltung, Beibehaltung lose Verbindung. Darüber hinaus besteht auch Anweisungen zum Durchführen der Authentifizierung und Autorisierung mit IdentityServer, den Zugriff auf Daten von Datenvolumes Microservices und Komponententests.

## <a name="prefaceprefacemd"></a>[Einleitung](preface.md)

In diesem Kapitel erklärt den Zweck und Umfang der im Handbuch und wen angestrebt wird.

## <a name="introductionintroductionmd"></a>[Introduction (Einführung)](introduction.md)

Entwickler von Unternehmens-apps sind mehrere Herausforderungen, die die Architektur der app während der Entwicklung ändern kann. Aus diesem Grund ist es wichtig, eine app erstellen, sodass es geändert oder mit der Zeit erweitert werden kann. Entwerfen für solche Anpassungsfähigkeit kann schwierig sein, aber in der Regel umfasst das Partitionieren von einer app in diskrete, lose miteinander verknüpfte Komponenten, die problemlos zusammen in einer Anwendung integriert werden können.

## <a name="mvvmmvvmmd"></a>[MVVM](mvvm.md)

Das Model-View-ViewModel (MVVM)-Muster hilft, um die Logik für die Business und Präsentation von einer Anwendung über die Benutzeroberfläche (UI) ordnungsgemäß zu trennen. Eine saubere Trennung zwischen der Anwendungslogik und der Benutzeroberfläche verwalten können Sie um zahlreiche Entwicklungsprobleme zu beheben, und erleichtert werden kann eine Anwendung zu testen, verwalten und entwickeln. Sie können Code wiederverwenden Verkaufschancen erheblich verbessern und erlaubt Entwicklern und UI-Designer mehr einfacher zusammenarbeiten, bei der Entwicklung ihrer jeweiligen Teile einer app.

## <a name="dependency-injectiondependency-injectionmd"></a>[Abhängigkeitsinjektion](dependency-injection.md)

Abhängigkeitsinjektion ermöglicht die Entkopplung der konkrete Typen aus dem Code, von der diese Typen abhängt. In der Regel verwendet ein Container, der eine Liste der Registrierungen und Zuordnungen zwischen Schnittstellen und abstrakte Typen enthält, und die konkrete Typen, die implementieren oder erweitern diese Typen.

Dependency Injection-Containern reduzieren die Kopplung zwischen Objekten durch eine Klasseninstanzen zu instanziieren und verwalten ihre Lebensdauer auf Basis der Konfiguration des Containers ermöglicht. Während der Erstellung Objekte fügt der Container keine Abhängigkeiten, die das Objekt benötigt hinein. Wenn diese Abhängigkeiten noch nicht erstellt wurden, wird der Container erstellt und deren Abhängigkeiten zuerst aufgelöst.

## <a name="communicating-between-loosely-coupled-componentscommunicating-between-loosely-coupled-componentsmd"></a>[Kommunikation zwischen lose gekoppelten Komponenten](communicating-between-loosely-coupled-components.md)

Der Xamarin.Forms [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) Klasse implementiert die Veröffentlichen-Abonnieren-Muster, sodass meldungsbasierte Kommunikation zwischen Komponenten, die unpraktisch, Objekt und Typverweise verknüpft sind. Dieser Mechanismus erlaubt Verlegern und Abonnenten zu kommunizieren, ohne einen Verweis zu "other" und helfen, reduzieren die Abhängigkeiten zwischen Komponenten, und lässt außerdem Komponenten unabhängig entwickelt und getestet werden.

## <a name="navigationnavigationmd"></a>[Navigation](navigation.md)

Xamarin.Forms umfasst Unterstützung für die Seitennavigation, was in der Regel aus der Benutzerinteraktion mit der Benutzeroberfläche oder aus der app selbst aufgrund interner Geschäftslogik gesteuerte Zustandsänderungen führt. Navigation kann jedoch komplex, um in apps implementieren, die das MVVM-Muster verwenden.

Dieses Kapitel bietet eine `NavigationService` -Klasse, die verwendet wird, um die Navigation in der Model First aus Ansichtsmodelle ausführen. Platzieren die Navigationslogik in der Sicht Modellklassen bedeutet, dass, dass die Logik über automatisierte Tests ausgeführt werden kann. Darüber hinaus kann das Ansichtsmodell dann Logik zum Steuerelement Navigationsbereich, um sicherzustellen, dass bestimmte Geschäftsregeln erzwungen werden implementieren.

## <a name="validationvalidationmd"></a>[Validierung](validation.md)

Jede app, die Benutzereingaben akzeptiert, sollten sicherstellen, dass die Eingabe gültig ist. Ohne Validierung kann ein Benutzer Daten angeben, die bewirkt, die app dass zu Fehlern. Validierung erzwingt Geschäftsregeln, und verhindert, dass einen Angreifer Räumen schädliche Daten.

Im Kontext des Modell-ViewModel-Modell (MVVM), das einem Ansichtsmodell Muster oder Modell müssen häufig zum Ausführen von datenvalidierung und Validierungsfehler in die Ansicht zu signalisieren, damit der Benutzer, die sie korrigieren kann.

## <a name="configuration-managementconfiguration-managementmd"></a>[Konfigurationsverwaltung](configuration-management.md)

Einstellungen können Sie die Trennung von Daten, die Verhalten einer App aus dem Code konfiguriert, das Verhalten geändert werden, ohne die app neu zu erstellen. App-Einstellungen sind Daten, die eine app erstellt und verwaltet und benutzereinstellungen werden die anpassbare Einstellungen einer App, die beeinflussen das Verhalten der app und häufig erneute Anpassung erfordern.

## <a name="containerized-microservicescontainerized-microservicesmd"></a>[Containermicroservices](containerized-microservices.md)

Microservices bieten einen Ansatz für die Anwendungsentwicklung und Bereitstellung, die für moderne Cloudanwendungen die Flexibilität, Skalierung und Zuverlässigkeit Anforderungen geeignet ist. Einer der Hauptvorteile der Microservices ist, sie dezentral skalierte unabhängig was bedeutet sein können, dass für ein bestimmten Funktionsbereich skaliert werden kann, die weitere Verarbeitung Power oder die Netzwerkbandbreite bei Bedarf, ohne unnötigerweise Skalierung Bereiche unterstützen erforderlich sind. die Anwendung, die keine erhöhten Bedarf auftreten.

## <a name="authentication-and-authorizationauthentication-and-authorizationmd"></a>[Authentifizierung und Autorisierung](authentication-and-authorization.md)

Es gibt viele Ansätze zum Integrieren von Authentifizierung und Autorisierung in einer Xamarin.Forms-app, die mit einer ASP.NET MVC-Web-Anwendung kommuniziert. Hier werden Authentifizierung und Autorisierung mit einem Microservice Datenvolumes Identität ausgeführt, die IdentityServer 4 verwendet. IdentityServer ist ein open Source-OpenID Connect und OAuth 2.0-Framework für ASP.NET Core, die mit ASP.NET Core Identität zum Ausführen von trägertokenauthentifizierung integriert wird.

## <a name="accessing-remote-dataaccessing-remote-datamd"></a>[Zugreifen auf Remotedaten](accessing-remote-data.md)

Viele moderne webbasierte Lösungen stellen Gebrauch von Webdiensten, gehostet von Webservern, um Funktionen für remote-Client-Anwendungen bereitzustellen. Die Vorgänge, die einen Webdienst verfügbar macht, bilden eine Web-API und Clientanwendungen sollten in der Lage, die Web-API nutzen, ohne zu wissen, wie die Daten oder Vorgänge, die die API verfügbar macht implementiert werden.

## <a name="unit-testingunit-testingmd"></a>[Komponententests](unit-testing.md)

Testen von Modellen und Modelle anzeigen von MVVM Anwendungen ist identisch mit Tests andere Klassen bilden, und die gleichen Tools und Techniken können verwendet werden. Es gibt jedoch einige Muster, die typisch für Modell sind und die Ansicht Modellklassen, die von bestimmten Einheit Testverfahren profitieren können.

## <a name="feedback"></a>Feedback

Dieses Projekt bietet eine Community-Website, auf der können Sie Fragen stellen und Feedback. Community-Website befindet sich auf [GitHub](https://github.com/dotnet-architecture/eShopOnContainers). Alternativ können Sie Feedback über das e-Book kann per e-Mail an [ dotnet-architecture-ebooks-feedback@service.microsoft.com ](mailto:dotnet-architecture-ebooks-feedback@service.microsoft.com).


## <a name="related-links"></a>Verwandte Links

- [Download-e-Book (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (sample)](https://github.com/dotnet-architecture/eShopOnContainers)
