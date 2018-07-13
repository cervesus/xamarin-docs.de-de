---
title: Xamarin.Forms-e-Book mit Mustern von Unternehmensanwendungen
description: Dieses e-Book bietet Anleitungen zur Architektur für die Entwicklung von anpassbaren, verwaltbaren und testbaren Xamarin.Forms-unternehmensanwendungen.
ms.prod: xamarin
ms.assetid: 28cfed6c-6175-4223-a8cc-798d40bf0832
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: ecfe99f66e16eafabc3117036ff065e3a35259c3
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994347"
---
# <a name="enterprise-application-patterns-using-xamarinforms-ebook"></a>Xamarin.Forms-e-Book mit Mustern von Unternehmensanwendungen

_Architekturrichtlinien für die Entwicklung von anpassbaren, verwaltbaren und testbaren Xamarin.Forms-unternehmensanwendungen_

![](images/cover-sml.png "Xamarin.Forms-e-Book mit Mustern von Unternehmensanwendungen")

Dieses e-Book enthält Anleitungen dazu, wie die Model-View-ViewModel (MVVM) Muster, Abhängigkeitsinjektion, Navigation, Validierung und konfigurationsverwaltung, implementieren und gleichzeitig die losen Kopplung. Darüber hinaus besteht auch Anleitungen zum Durchführen der Authentifizierung und Autorisierung mit Identity Server, den Zugriff auf Daten aus Microservices in Containern und Komponententests.

## <a name="prefaceprefacemd"></a>[Einleitung](preface.md)

In diesem Kapitel erläutert Zweck und Umfang des Handbuchs und, die es gedacht ist.

## <a name="introductionintroductionmd"></a>[Introduction (Einführung)](introduction.md)

Entwickler von Unternehmens-apps stehen vor verschiedene Herausforderungen, die während der Entwicklung die Architektur der app ändern können. Aus diesem Grund ist es wichtig, um eine app erstellen, damit sie geändert oder im Laufe der Zeit erweitert werden kann. Entwerfen für solche Anpassungsfähigkeit kann schwierig sein, aber in der Regel umfasst partitionieren eine app in diskrete, lose gekoppelte Komponenten, die in eine app ganz einfach miteinander integriert werden können.

## <a name="mvvmmvvmmd"></a>[MVVM](mvvm.md)

Das Model-View-ViewModel (MVVM)-Muster hilft, sauber trennen Sie die Logik für die Business und Präsentation von einer Anwendung über die Benutzeroberfläche (UI). Eine saubere Trennung zwischen der Anwendungslogik und die Benutzeroberfläche verwalten kann kann, um zahlreiche Probleme bei der Entwicklung zu beheben und eine Anwendung einfacher zu testen, verwalten und entwickeln. Sie können Code wiederverwenden Verkaufschancen erheblich verbessern und ermöglicht es Entwicklern und Benutzeroberflächen-Designer eine problemlose Zusammenarbeit ermöglichen, bei der Entwicklung ihrer jeweiligen Teile einer app.

## <a name="dependency-injectiondependency-injectionmd"></a>[Abhängigkeitsinjektion](dependency-injection.md)

Abhängigkeitsinjektion ermöglicht die Entkopplung der konkrete Typen aus dem Code, von der diese Typen abhängt. In der Regel verwendet ein Container, der eine Liste der Registrierungen und Zuordnungen zwischen Schnittstellen und abstrakten Typen enthält, und die konkrete Typen, die implementieren oder erweitern diese Typen.

Dependency Injection-Containern verringern Sie die Kopplung zwischen Objekten durch eine Funktion zum Instanziieren von Klasseninstanzen und verwalten ihre Lebensdauer, die auf Basis der Konfiguration des Containers bereitstellen. Während der Erstellung Objekte fügt der Container um Abhängigkeiten, die das Objekt erfordert hinein. Wenn diese Abhängigkeiten noch nicht erstellt wurden, wird der Container erstellt und ihre Abhängigkeiten zuerst aufgelöst.

## <a name="communicating-between-loosely-coupled-componentscommunicating-between-loosely-coupled-componentsmd"></a>[Kommunikation zwischen lose gekoppelten Komponenten](communicating-between-loosely-coupled-components.md)

Der Xamarin.Forms [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) -Klasse implementiert, das Veröffentlichen-Abonnieren-Muster, die nachrichtenbasierte Kommunikation zwischen Komponenten, die unpraktisch, die vom Objekt und Typverweise verknüpfen können. Dieser Mechanismus ermöglicht es, Verlegern und Abonnenten zu kommunizieren, ohne einen Verweis auf, beim Reduzieren von Abhängigkeiten zwischen Komponenten, gleichzeitig Komponenten unabhängig voneinander entwickelt und getestet werden können.

## <a name="navigationnavigationmd"></a>[Navigation](navigation.md)

Xamarin.Forms umfasst Unterstützung für die Seitennavigation, was in der Regel über die Interaktion des Benutzers über die Benutzeroberfläche oder über die app selbst aufgrund von internen Zustand der gesteuerte Änderungen führt. Navigation kann jedoch komplex, um in apps zu implementieren, die das MVVM-Muster verwenden.

In diesem Kapitel wird eine `NavigationService` -Klasse, die zum Ausführen der Navigation in der Model First von ViewModels verwendet wird. Modellklassen Navigationslogik in der Ansicht platzieren bedeutet, dass die Logik durch automatisierte Tests ausgeführt werden kann. Darüber hinaus kann das Ansichtsmodell implementieren Logik zum Steuerelement-Navigation, um sicherzustellen, dass ein bestimmter Geschäftsregeln erzwungen werden.

## <a name="validationvalidationmd"></a>[Validierung](validation.md)

Jede app, die Eingaben von Benutzern akzeptiert sorgen dafür, dass die Eingabe gültig ist. Ohne Validierung kann ein Benutzer Daten bereitstellen, die bewirkt, dass die app fehl. Überprüfung von Geschäftsregeln erzwingt und verhindert, dass einen Angreifer schädliche Daten einfügt.

Klicken Sie im Kontext des Modell-ViewModel-Modell (MVVM) Muster, ein Ansichtsmodell oder Modell ist häufig erforderlich, um eine datenüberprüfung durchführen und Validierungsfehler an die Ansicht zu signalisieren, sodass der Benutzer, die sie korrigieren kann.

## <a name="configuration-managementconfiguration-managementmd"></a>[Konfigurationsverwaltung](configuration-management.md)

Einstellungen können Sie die Trennung von Daten, die das Verhalten einer App aus dem Code, konfiguriert das Verhalten geändert werden, ohne die app neu zu erstellen. App-Einstellungen sind Daten, die eine app erstellt und verwaltet und benutzereinstellungen sind die anpassbare Einstellungen einer App, die Auswirkungen auf das Verhalten der app und häufig erneute Anpassung ist nicht erforderlich.

## <a name="containerized-microservicescontainerized-microservicesmd"></a>[Containermicroservices](containerized-microservices.md)

Microservices bieten einen Ansatz zur Anwendungsentwicklung und-Bereitstellung, die für die Anforderungen für Agilität, Skalierbarkeit und Zuverlässigkeit von modernen Cloudanwendungen geeignet ist. Einer der wichtigsten Vorteile von Microservices ist, sie horizontal hochskalierte unabhängig Dies bedeutet sein können, dass für ein bestimmten Funktionsbereich skaliert werden kann, die mehr verarbeitungsleistung oder Netzwerkbandbreite zum Erfüllen der Nachfrage, ohne unnötig Skalierung Bereichen erforderlich sind die Anwendung, auf denen höhere Nachfrage nicht auftreten.

## <a name="authentication-and-authorizationauthentication-and-authorizationmd"></a>[Authentifizierung und Autorisierung](authentication-and-authorization.md)

Es gibt viele Ansätze zum Integrieren von Authentifizierung und Autorisierung in einer Xamarin.Forms-app, die mit einer ASP.NET MVC-Web-Anwendung kommuniziert. Hier werden Authentifizierung und Autorisierung mit einem Container Identitäts-Microservice ausgeführt, die von Identity Server 4 verwendet. Identity Server ist ein open-Source-Framework für OpenID Connect und OAuth 2.0 für ASP.NET Core, die in ASP.NET Core-Identität zum Ausführen von Bearer-token-Authentifizierung integriert.

## <a name="accessing-remote-dataaccessing-remote-datamd"></a>[Zugreifen auf Remotedaten](accessing-remote-data.md)

Viele moderne webbasierte Lösungen stellen Verwendung von Webdiensten, Webservern gehostete Webdienste, um Funktionen für remote-Client-Anwendungen bereitzustellen. Die Vorgänge, die einen Webdienst verfügbar macht, bilden eine Web-API und Client-apps muss die Web-API nutzen, ohne zu wissen, wie die Daten oder Vorgänge, die die-API implementiert werden können.

## <a name="unit-testingunit-testingmd"></a>[Komponententests](unit-testing.md)

Testen der Modelle und Ansichtsmodelle von MVVM-Anwendungen ist identisch mit dem Testen andere Klassen bilden, und die gleichen Tools und Techniken können verwendet werden. Es gibt jedoch einige Muster, die typisch für Modell und ViewModel-Klassen, die von bestimmten Einheit Testverfahren profitieren können.

## <a name="feedback"></a>Feedback

Dieses Projekt enthält eine Community-Website, auf der können Sie Fragen und Feedback geben. Community-Website befindet sich auf [GitHub](https://github.com/dotnet-architecture/eShopOnContainers). Sie können auch Feedback zu den e-Book kann per e-Mail an [ dotnet-architecture-ebooks-feedback@service.microsoft.com ](mailto:dotnet-architecture-ebooks-feedback@service.microsoft.com).


## <a name="related-links"></a>Verwandte Links

- [E-Book (2Mb PDF-Datei) herunterladen](https://aka.ms/xamarinpatternsebook)
- ["eshoponcontainers" (GitHub) (Beispiel)](https://github.com/dotnet-architecture/eShopOnContainers)
