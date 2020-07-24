---
title: 'E-Book zu Mustern von Unternehmensanwendungen mithilfe von :::no-loc(Xamarin.Forms):::'
description: 'Dieses e-Book bietet Architektur Anleitungen zum entwickeln anpassbarer, verwaltebarer und testbarer :::no-loc(Xamarin.Forms)::: Unternehmensanwendungen.'
ms.prod: xamarin
ms.assetid: 28cfed6c-6175-4223-a8cc-798d40bf0832
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
no-loc:
- ':::no-loc(Xamarin.Forms):::'
- ':::no-loc(Xamarin.Essentials):::'
ms.openlocfilehash: 549fe496cdb1d68d091d5fb3ed247ccef5a111a8
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "86996161"
---
# <a name="enterprise-application-patterns-using-no-locxamarinforms-ebook"></a>E-Book zu Mustern von Unternehmensanwendungen mithilfe von :::no-loc(Xamarin.Forms):::

_Architektur Leit Faden für die Entwicklung anpassbarer, verwaltebarer und testbarer :::no-loc(Xamarin.Forms)::: Unternehmensanwendungen_

![Unternehmens Anwendungs Muster mit::: NO-LOC (xamarin. Forms)::: eBook](images/cover-sml.png)

Dieses e-Book enthält Anleitungen dazu, wie das Model-View-ViewModel (MVVM)-Muster, die Abhängigkeitsinjektion, Navigation, Validierung und Konfigurations Verwaltung implementiert wird, während die lose Kopplung beibehalten wird. Außerdem gibt es Anleitungen zum Durchführen von Authentifizierung und Autorisierung mit identityserver, zum Zugreifen auf Daten aus containerisierten microservices und Unittests.

## <a name="preface"></a>[Einleitung](preface.md)

In diesem Kapitel werden Zweck und Umfang des Handbuchs erläutert.

## <a name="introduction"></a>[Einführung](introduction.md)

Entwickler von Unternehmens-apps stellen verschiedene Herausforderungen dar, die die Architektur der APP während der Entwicklung ändern können. Daher ist es wichtig, eine APP zu erstellen, damit Sie im Laufe der Zeit geändert oder erweitert werden kann. Der Entwurf für diese Anpassbarkeit kann schwierig sein, aber es umfasst in der Regel die Partitionierung einer APP in diskrete, lose gekoppelte Komponenten, die problemlos in eine APP integriert werden können.

## <a name="mvvm"></a>[MVVM](mvvm.md)

Das Model-View-ViewModel (MVVM)-Muster hilft bei der ordnungsgemäßen Trennung der Geschäfts-und Präsentationslogik einer Anwendung von der Benutzeroberfläche (User Interface, UI). Wenn Sie eine saubere Trennung zwischen Anwendungslogik und Benutzeroberfläche gewährleisten, können Sie zahlreiche Entwicklungsprobleme beheben und eine Anwendung leichter testen, warten und entwickeln. Sie kann auch die Möglichkeiten zur Wiederverwendung von Code erheblich verbessern und ermöglicht Entwicklern und UI-Designern, bei der Entwicklung ihrer jeweiligen Teile einer APP leichter zusammenzuarbeiten.

## <a name="dependency-injection"></a>[Abhängigkeitsinjektion](dependency-injection.md)

Die Abhängigkeitsinjektion ermöglicht das Entkoppeln von konkreten Typen aus dem Code, der von diesen Typen abhängig ist. In der Regel wird ein Container verwendet, der eine Liste von Registrierungen und Zuordnungen Zwischenschnitt stellen und abstrakten Typen enthält, sowie die konkreten Typen, die diese Typen implementieren oder erweitern.

Abhängigkeits einschleusungs Container reduzieren die Kopplung zwischen Objekten, indem Sie eine Funktion zum Instanziieren von Klassen Instanzen und zum Verwalten Ihrer Lebensdauer basierend auf der Konfiguration des Containers bereitstellen. Während der Objekt Erstellung fügt der Container alle Abhängigkeiten ein, die das Objekt benötigt. Wenn diese Abhängigkeiten noch nicht erstellt wurden, werden die Abhängigkeiten vom Container erstellt und aufgelöst.

## <a name="communicating-between-loosely-coupled-components"></a>[Kommunikation zwischen lose gekoppelten Komponenten](communicating-between-loosely-coupled-components.md)

Die :::no-loc(Xamarin.Forms):::-Klasse [`MessagingCenter`](xref::::no-loc(Xamarin.Forms):::.MessagingCenter) implementiert das Veröffentlichen-Abonnieren-Muster und ermöglicht so eine nachrichtenbasierte Kommunikation zwischen Komponenten, für die eine Verknüpfung über Objekt- und Typverweise ungünstig ist. Dieser Mechanismus ermöglicht es Verlegern und Abonnenten, ohne einen Verweis aufeinander zu kommunizieren, sodass Abhängigkeiten zwischen Komponenten reduziert werden können, während Komponenten unabhängig entwickelt und getestet werden können.

## <a name="navigation"></a>[Navigation](navigation.md)

:::no-loc(Xamarin.Forms):::bietet Unterstützung für die Seitennavigation, die in der Regel aus der Interaktion des Benutzers mit der Benutzeroberfläche oder aus der APP selbst resultiert, weil interne, Logik gesteuerte Zustandsänderungen auftreten. Allerdings kann die Navigation in apps, die das MVVM-Muster verwenden, komplex sein.

In diesem Kapitel `NavigationService` wird eine-Klasse vorgestellt, die verwendet wird, um die Ansicht Model-First-Navigation von Ansichts Modellen auszuführen. Das Platzieren der Navigations Logik in Ansichts Modellklassen bedeutet, dass die Logik durch automatisierte Tests ausgeführt werden kann. Außerdem kann das Ansichts Modell eine Logik zum Steuern der Navigation implementieren, um sicherzustellen, dass bestimmte Geschäftsregeln erzwungen werden.

## <a name="validation"></a>[Überprüfung](validation.md)

Jede APP, die Eingaben von Benutzern akzeptiert, sollte sicherstellen, dass die Eingabe gültig ist. Ohne Validierung kann ein Benutzerdaten bereitstellen, die dazu führen, dass die APP fehlschlägt. Die Validierung erzwingt Geschäftsregeln und verhindert, dass ein Angreifer schädliche Daten eingibt.

Im Zusammenhang mit dem Model-View-ViewModel (MVVM)-Muster ist häufig ein Ansichts Modell oder-Modell erforderlich, um die Datenüberprüfung auszuführen und Validierungs Fehler an die Sicht zu senden, damit Sie vom Benutzer korrigiert werden kann.

## <a name="configuration-management"></a>[Konfigurations Verwaltung](configuration-management.md)

Einstellungen ermöglichen die Trennung von Daten, die das Verhalten einer App aus dem Code konfigurieren, sodass das Verhalten geändert werden kann, ohne die APP neu zu erstellen. App-Einstellungen sind Daten, die von einer App erstellt und verwaltet werden, und Benutzereinstellungen sind die anpassbaren Einstellungen einer APP, die sich auf das Verhalten der APP auswirken und keine häufige erneute Anpassung erfordern.

## <a name="containerized-microservices"></a>[Containermicroservices](containerized-microservices.md)

Microservices bieten einen Ansatz für die Anwendungsentwicklung und-Bereitstellung, der sich auf die Flexibilität, Skalierbarkeit und Zuverlässigkeit von modernen cloudanwendungen eignet. Einer der Hauptvorteile von-Funktionen besteht darin, dass Sie unabhängig voneinander horizontal hochskaliert werden können. Dies bedeutet, dass ein bestimmter Funktionsbereich skaliert werden kann, der eine höhere Verarbeitungsleistung oder Netzwerkbandbreite erfordert, um die Nachfrage zu unterstützen, ohne dass Bereiche der Anwendung unnötig skaliert werden, die nicht mehr benötigt werden.

## <a name="authentication-and-authorization"></a>[Authentifizierung und Autorisierung](authentication-and-authorization.md)

Es gibt viele Ansätze für die Integration von Authentifizierung und Autorisierung in eine- :::no-loc(Xamarin.Forms)::: app, die mit einer ASP.NET MVC-Webanwendung kommuniziert. Hier werden Authentifizierung und Autorisierung mit einem containerisierten Identitäts-microservice ausgeführt, der "identityserver 4" verwendet. Identityserver ist ein OpenID Connect-und OAuth 2,0-Open-Source-Framework für ASP.net Core, die in ASP.net Core Identity integriert ist, um bearertokenauthentifizierung auszuführen.

## <a name="accessing-remote-data"></a>[Zugreifen auf Remotedaten](accessing-remote-data.md)

Viele moderne webbasierte Lösungen nutzen Webdienste, die von Webservern gehostet werden, um Funktionen für Remote Client Anwendungen bereitzustellen. Die von einem Webdienst verfügbar gemachten Vorgänge bilden eine Web-API, und Client-apps sollten in der Lage sein, die Web-API zu verwenden, ohne zu wissen, wie die von der API verfügbar gemachten Daten oder Vorgänge implementiert werden.

## <a name="unit-testing"></a>[Komponententests](unit-testing.md)

Das Testen von Modellen und Anzeigen von Modellen von MVVM-Anwendungen ist identisch mit dem Testen anderer Klassen, und es können dieselben Tools und Techniken verwendet werden. Es gibt jedoch einige Muster, die typisch für das Modellieren und Anzeigen von Modellklassen sind, die von speziellen Verfahren für Unittests profitieren können.

## <a name="community-site"></a>Communitysite

Dieses Projekt verfügt über eine Community-Website, auf der Sie Fragen stellen und Feedback geben können. Die Community-Website befindet sich auf [GitHub](https://github.com/dotnet-architecture/eShopOnContainers). Alternativ können Sie Feedback zu dem e-book per e-Mail senden [dotnet-architecture-ebooks-feedback@service.microsoft.com](mailto:dotnet-architecture-ebooks-feedback@service.microsoft.com) .

## <a name="related-links"></a>Verwandte Links

- [Download-e-Book (2 MB PDF)](https://aka.ms/xamarinpatternsebook)
- [eshoponcontainers (GitHub) (Beispiel)](https://github.com/dotnet-architecture/eShopOnContainers)
