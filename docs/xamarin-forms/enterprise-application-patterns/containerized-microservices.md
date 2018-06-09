---
title: Datenvolumes Microservices
description: In diesem Kapitel wird erläutert, wie die Microservices und Container zum Erstellen agile, skalierbare und zuverlässige moderne Cloudanwendungen.
ms.prod: xamarin
ms.assetid: 5872ad92-04e0-4f1a-9691-79d5602f5683
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 33be84bc17f72c8b70d117a0742b001f1f763d3d
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242260"
---
# <a name="containerized-microservices"></a>Datenvolumes Microservices

Entwickeln von Client-/ serveranwendungen hat im Mittelpunkt stehen zum Erstellen von mehrstufigen Anwendungen, die spezielle Technologien verwendet werden, in jeder Ebene geführt. Solche Anwendungen werden häufig als bezeichnet *monolithischen* -Anwendungen und sind auf Hardware vorab skaliert für Spitzenlasten verpackt. Die wichtigsten Nachteile dieses Ansatzes Entwicklung sind die enge Kopplung zwischen Komponenten auf der jeweiligen Ebene, das, dass die einzelne Komponenten, die nicht problemlos skaliert werden können, und die Kosten für das Testen. Eine einfache Aktualisierung haben kann unvorhergesehene Auswirkungen auf die restliche der Ebene und damit eine Änderung an eine Anwendungskomponente erfordert die gesamte Ebene getestet und erneut bereitgestellt werden.

Insbesondere bezüglich im Zeitalter von der Cloud, dass Sie einzelne Komponenten problemlos können skaliert wird. Eine Anwendung aufgrund eines monolithischen enthält domänenspezifische-Funktionalität und ist in der Regel von funktionalen Ebenen, wie z. B. front-End-Geschäftslogik und Datenspeicher unterteilt. Eine Anwendung aufgrund eines monolithischen skaliert wird durch Klonen die gesamte Anwendung auf mehreren Computern wie in Abbildung 8 – 1 veranschaulicht.

![](containerized-microservices-images/monolithicapp.png "Aufgrund eines monolithischen Anwendung Skalierungsansatzes")

**Abbildung 8: 1**: aufgrund eines monolithischen Anwendung skalieren Ansatz

## <a name="microservices"></a>Microservices

Microservices bieten einen anderen Ansatz für die Anwendungsentwicklung und Bereitstellung, einen Ansatz die Flexibilität, Skalierung und Zuverlässigkeit Anforderungen für moderne Cloudanwendungen geeignet ist. Eine Anwendung Microservices wird in unabhängige Komponenten zerlegt, die zusammenarbeiten, um die gesamte Funktionalität der Anwendung zu übermitteln. Der Begriff Microservice hebt hervor, dass Anwendungen von Diensten, die klein genug ist, entsprechend der unabhängige Bedenken zusammengesetzt werden sollte, damit jeder Microservice eine einzelne Funktion implementiert. Außerdem weist jede Microservice klar definierten Verträge, damit andere Microservices kommunizieren und Daten freigeben können. Typische Beispiele für Microservices Einkaufswagen einschließen, Bestände Verarbeitung, Subsystemen und Payment-Verarbeitung.

Microservices können unabhängig voneinander und im Vergleich zu riesige monolithischen Anwendungen mit horizontaler Skalierung, die zusammen skaliert werden. Dies bedeutet, dass für ein bestimmten Funktionsbereich, der weitere Verarbeitung Power oder die Netzwerkbandbreite zur Unterstützung der Anforderung erforderlich ist, anstatt unnötigerweise dezentrale Skalierung anderen Bereichen der Anwendung skaliert werden kann. Abbildung 8 – 2 veranschaulicht diese Vorgehensweise, Microservices sind, wobei bereitgestellt und unabhängig voneinander skaliert, Erstellen von Instanzen der Dienste computerübergreifend.

![](containerized-microservices-images/microservicesapp.png "Skalierungsansatzes für Microservices-Anwendung")

**Abbildung 8-2**: Skalierung Ansatz Microservices-Anwendung

Microservice mit horizontaler Skalierung kann nahezu sofortige, ermöglicht es einer Anwendung zur Anpassung an wechselnde Lasten sein. Z. B. möglicherweise eine einzelne Microservice in der Web-seitigen Funktionen einer Anwendung der einzige Microservice in der Anwendung, die zum horizontalen Skalieren, um zusätzliche eingehenden Datenverkehr verarbeiten muss.

Das klassische Modell für die Skalierbarkeit der Anwendung ist eine Ebene mit Lastenausgleich, statuslose mit freigegebenen externen Datenspeicher zum Speichern persistenter Daten haben. Zustandsbehaftete Microservices verwalten ihre eigenen Daten, in der Regel auf den Servern, auf denen sie so platziert werden, lokal speichern, um den Aufwand für das Netzwerk zu vermeiden Zugriff und die Komplexität der Cross-Dienst Vorgänge. Dadurch können die schnellste Möglichkeit Verarbeitung von Daten und kann entfällt der Bedarf für caching Systeme. Darüber hinaus partitionieren skalierbare zustandsbehaftete Microservices in der Regel Daten für ihre Instanzen Datendurchsatz und das Übertragungsintervall verwalten, denen ein einzelner Server unterstützen kann.

Microservices unterstützen auch unabhängige Updates. Diese Kopplung zwischen Microservices bietet eine schnelle und zuverlässige Anwendung Entwicklung. Grundsätzlich unabhängige, verteilte unterstützt parallelen Updates, wobei nur eine Teilmenge der Instanzen einer einzelnen Microservice zu einem beliebigen Zeitpunkt aktualisiert wird. Aus diesem Grund ein Problem festgestellt wird, kann eine fehlerhafte Aktualisierung, Rollback werden vor allen Instanzen mit den fehlerhaften Code oder die Konfiguration zu aktualisieren. Auf ähnliche Weise verwenden Microservices in der Regel versionsverwaltung des Schemas, damit Clients eine einheitliche Version sehen, wann Updates unabhängig davon, welche Microservice Instanz mit übertragenen angewendet werden.

Aus diesem Grund haben Microservice Anwendungen viele Vorteile gegenüber monolithischen Anwendungen:

-   Jede Microservice ist relativ kleine, leicht zu verwalten und zu entwickeln.
-   Jede Microservice kann entwickelt und unabhängig von anderen Diensten bereitgestellt werden.
-   Jede Microservice kann skalierten unabhängig sein. Z. B. möglicherweise eine Catalog-Dienst oder einen Warenkorb Warenkorb-Dienst müssen mehr als eine Reihenfolge Dienst dezentral skaliert werden. Aus diesem Grund wird die resultierende Infrastruktur Ressourcen effizienter nutzen, wenn Sie out Skalierung.
-   Jede Microservice isoliert Probleme. Wenn ein Problem, in einem Dienst vorliegt wirkt er z. B. nur diesen Dienst. Die anderen Dienste können weiterhin Anforderungen zu verarbeiten.
-   Jede Microservice können die neuesten Technologien. Da Microservices autonome und zur Seite-an-Seite handelt, können die neuesten Technologien und Frameworks verwendet werden anstatt einem älteren Framework verwenden, die möglicherweise von einer Anwendung aufgrund eines monolithischen verwendet werden.

Allerdings verfügt über eine Microservice-basierten Lösung auch mögliche Nachteile:

-   Die Auswahl, wie eine Anwendung in Microservices partitionieren kann schwierig sein, wie jede Microservice vollständig autonome, End-to-End, einschließlich Verantwortung für die Datenquellen muss.
-   Entwickler müssen dienstübergreifende Kommunikation implementieren, wodurch Komplexität und die Latenz der Anwendung hinzugefügt.
-   Atomarische Transaktionen zwischen mehreren Microservices in der Regel sind nicht möglich. Aus diesem Grund müssen geschäftsanforderungen eventuellen Konsistenz zwischen Microservices nutzen.
-   In der Produktion gibt es eine operative Komplexität bei der Bereitstellung und Verwaltung von einem System viele unabhängige Dienste gefährdet.
-   Direkter Kommunikation von Client-zu-Microservice kann umgestaltet werden die Verträge des Microservices erschweren. Z. B. mit der Zeit wie das System in Dienste partitioniert wird möglicherweise ändern möchten. Ein einzelnen Dienst kann in zwei oder mehrere Dienste unterteilt, und zwei Dienste möglicherweise zusammenzuführen. Für die Kommunikation zwischen Clients direkt mit Microservices kann diese Umgestaltung Arbeit Kompatibilität mit Client-apps unterbrechen.

## <a name="containerization"></a>Containerisierung

Containerization besteht ein Ansatz für die Softwareentwicklung, in dem eine Anwendung und seine mit Versionsangabe Gruppe von Abhängigkeiten und ihre Umgebungskonfiguration, die als manifest Bereitstellungsdateien abstrahiert als ein Container-Abbild, die als Einheit getestet zusammengefasst werden, und auf einem Host-Betriebssystem bereitgestellt.

Ein Container ist eine isolierte, Ressourcen gesteuerte und portierbare betriebsumgebung, in denen eine Anwendung ohne direkten Zugriff auf die Ressourcen von anderen Containern oder der Host ausgeführt werden kann. Aus diesem Grund wird ein Container sucht und verhält sich wie einem neu installierten physischen Computer oder einem virtuellen Computer.

Gibt es einige ähnlichkeiten zwischen Containern und virtuellen Computern auf, wie in Abbildung 8 – 3 veranschaulicht.

![](containerized-microservices-images/containersvsvirtualmachines.png "Skalierungsansatzes für Microservices-Anwendung")

**Abbildung 8 – 3**: Vergleich von VMs und Containern

Ein Container ein Betriebssystem ausgeführt wird, verfügt über ein Dateisystem und kann über ein Netzwerk zugegriffen werden, als wäre er einem physischen oder virtuellen Computer. Allerdings unterscheiden sich die Technologie und Konzepte von Containern verwendete sehr von virtuellen Computern. Virtuelle Computer enthalten, die Anwendungen, die erforderlichen Abhängigkeiten und eine vollständige Gast-Betriebssystems. Container enthalten, die Anwendung und ihre Abhängigkeiten, aber andere Container, die als isolierte Prozesse ausgeführt werden, auf dem Host-Betriebssystem (abgesehen von der Hyper-V-Container in einem speziellen virtuellen Computer pro Container ausgeführten) das Betriebssystem freigegeben. Aus diesem Grund Container nutzen Ressourcen gemeinsam und erfordern normalerweise weniger Ressourcen als virtuelle Computer.

Der Vorteil, dass ein Container-orientierten Entwicklung und Bereitstellung Ansatz ist, dass die meisten Probleme beseitigt, die beim Auftreten inkonsistent Umgebung Installationen und die Probleme, die mit ihnen stammen. Darüber hinaus lassen Containern schnell skalieren Anwendungsfunktionen Instanziierung neue Container nach Bedarf.

Die grundlegenden Konzepte, die beim Erstellen und Arbeiten mit Containern sind:

-   Containerhost: Der physischen oder virtuellen Computer, die so konfiguriert, dass der Hostcontainer. Die containerhost werden ein oder mehrere Container ausgeführt.
-   Container-Image: Ein Image besteht aus einer Vereinigung der geschichteten Dateisysteme bauen aufeinander auf und ist die Grundlage für einen Container. Ein Bild verfügt nicht über die Status, und er nie ändert, wie es in verschiedenen Umgebungen bereitgestellt wird.
-   Container: Ein Container ist eine Laufzeitinstanz eines Bilds.
-   Containerbetriebssystem-Image: Container werden von Images bereitgestellt. Das Betriebssystemabbild Container ist möglicherweise viele Bildebenen, aus denen ein Container der ersten Ebene. Ein Container-Betriebssystem ist unveränderlich und kann nicht geändert werden.
-   Containerrepository: Jedes Mal, wenn ein Container-Abbild erstellt wird, werden die Images und seiner Abhängigkeiten auf in einem lokalen Repository gespeichert. Diese Images können auf dem containerhost mehrfach wiederverwendet werden. Die containerimages können auch gespeichert werden in einer öffentlichen oder privaten Registrierung, wie z. B. [Docker Hub](https://hub.docker.com/), sodass sie in verschiedenen containerhosts verwendet werden können.

Unternehmen sind zunehmend Container eingeführt, bei der Implementierung Microservice-basierte Anwendungen und Docker geworden ist der Standardcontainer-Implementierung, die von den meisten Softwareplattformen und Cloud-Anbietern verbreitet ist.

Die eShopOnContainers Verweis-Anwendung verwendet Docker vier Datenvolumes Back-End-Microservices hosten, wie in Abbildung 8-4 dargestellt.

![](containerized-microservices-images/microservicesarchitecture.png "eShopOnContainers auf Back-End-Microservices Anwendung verweisen.")

**Abbildung 8-4**: eShopOnContainers auf Back-End-Microservices Anwendung verweisen.

Die Architektur der Back-End-Dienste in der Referenz-Anwendung wird in mehrere untergeordnete autonomen Systemen in Form von zusammenarbeitenden Microservices und Container zerlegt. Jede Microservice bietet einen einzelnen Bereich von Funktionen: einen Identitätsdienst, eine Catalog-Dienst, eine Sortierung Dienst und einen Warenkorb-Dienst.

Jede Microservice verfügt über eine eigene Datenbank, sodass es vollständig von der anderen Microservices entkoppelt werden. Bei Bedarf können erfolgt die Konsistenz zwischen Datenbanken von anderen Microservices Ereignisse auf Anwendungsebene verwenden. Weitere Informationen finden Sie unter [Kommunikation zwischen Microservices](#communication_between_microservices).

Weitere Informationen zur Anwendung Referenz finden Sie unter [.NET Microservices: Architektur für .NET-Anwendungen in Containern](https://aka.ms/microservicesebook).

<a name="communication_between_client_and_microservices" />

## <a name="communication-between-client-and-microservices"></a>Kommunikation zwischen Client und Microservices

Die mobile app eShopOnContainers kommuniziert mit der Verwendung von Datenvolumes Back-End-Microservices *direkte Client-zu-Microservice* Kommunikation, die in Abbildung 8 – 5 angezeigt wird.

![](containerized-microservices-images/directclienttomicroservicecommunication.png "Skalierungsansatzes für Microservices-Anwendung")

**Abbildung 8 – 5**: direkte Kommunikation von Client-zu-Microservice

Mit direkte Kommunikation von Client-zu-Microservice stellt die mobile Anwendung Anforderungen an jede Microservice direkt über einen öffentlichen Endpunkt, mit einem anderen TCP-Port pro Microservice an. In der Produktion würde der Endpunkt in der Regel die Microservice Lastenausgleich zuzuordnen, die Anforderungen in der verfügbaren Instanzen verteilt.

> [!TIP]
> Erwägen Sie API-Gateway-Kommunikation. Direkter Kommunikation von Client-zu-Microservice möglich Nachteile beim Erstellen einer großen und komplexen Microservice Anwendung-basierten allerdings handelt es sich für eine kleine Anwendung mehr als ausreichend. Beim Entwerfen einer großen Microservice Anwendung mit Zehntausenden von Microservices basieren, sollten Sie in Erwägung ziehen, API-Gateway-Kommunikation. Weitere Informationen finden Sie unter [.NET Microservices: Architektur für .NET-Anwendungen in Containern](https://aka.ms/microservicesebook).

<a name="communication_between_microservices" />

## <a name="communication-between-microservices"></a>Kommunikation zwischen Microservices

Eine Microservices-basierten Anwendung ist ein verteiltes System, die möglicherweise auf mehreren Computern ausgeführt. Jede Dienstinstanz ist in der Regel ein Prozess. Aus diesem Grund müssen Dienste mithilfe von eine prozessübergreifende Kommunikationsprotokoll, z. B. HTTP, TCP, Advanced Message Queuing Protocol (AMQP) oder binäre Protokolle, je nach Art der einzelnen Dienste zu interagieren.

Die zwei allgemeine Ansätze zum Microservice-Microservice-Kommunikation sind HTTP-basierten REST Kommunikation bei Abfragen für Daten und einfaches asynchrones messaging bei der Kommunikation von Updates über mehrere Microservices.

Asynchroner messaging basierender ereignisgesteuerte Kommunikation ist wichtig, bei der Weitergabe von Änderungen über mehrere Microservices. Bei diesem Ansatz veröffentlicht ein Microservice ein Ereignis, wenn etwas, das wichtige, z. B. der Fall, wenn es sich um eine Geschäftseinheit aktualisiert. Andere Microservices auf diese Ereignisse abonnieren. Bei der ein Microservice ein Ereignis empfängt, aktualisiert dann eine eigene Geschäftseinheiten, die wiederum weitere Ereignisse, die zu veröffentlichenden verursachen könnten. Das Veröffentlichen-Abonnieren-Funktionalität wird in der Regel mit einem Ereignisbus erreicht.

Ein-Ereignisbus ermöglicht das Veröffentlichen / Abonnieren-Kommunikation zwischen Microservices, ohne dass die Komponenten des jeweils anderen explizit bekannt sein, wie in Abbildung 8 – 6 gezeigt.

![](containerized-microservices-images/eventbus.png "Veröffentlichen / Abonnieren Sie mit einem Ereignisbus")

**Abbildung 8 – 6:** Veröffentlichen / Abonnieren mit einem Ereignisbus

Aus Anwendungssicht, ist die Ereignisbus einfach Veröffentlichen-Abonnieren Kanal, über eine Schnittstelle verfügbar gemacht. Es kann jedoch die Möglichkeit, wie Sie, die der-Ereignisbus implementiert ist, variieren. Beispielsweise kann ein Ereignis nachrichtenbusimplementierung RabbitMQ, Azure Service Bus oder andere Service Bus wie NServiceBus und MassTransit verwenden. Abbildung 8 – 7 zeigt, wie ein Ereignisbus in der eShopOnContainers Verweis Anwendung verwendet wird.

![](containerized-microservices-images/microservicesarchitecturewitheventbus.png "Asynchrone ereignisgesteuerte Kommunikation in der Referenz-Anwendung")

**Abbildung 8 – 7:** asynchrone ereignisgesteuerte Kommunikation in der Referenz-Anwendung

1: n-asynchrone veröffentlichen-Funktionalität abonnieren bietet der eShopOnContainers-Ereignisbus mit RabbitMQ, implementiert. Dies bedeutet, dass nach dem Veröffentlichen eines Ereignisses, werden mehrere Abonnenten, die für das gleiche Ereignis lauscht. Abbildung 8 – 9 veranschaulicht diese Beziehung.

![](containerized-microservices-images/eventdrivencommunication.png "1: n-Kommunikation")

**Abbildung 8 – 9**: n-Kommunikation

Dieser Ansatz 1: n-Kommunikation verwendet Ereignisse, um Geschäftstransaktionen implementieren, die mehrere Dienste, die sicherstellen eventuellen Konsistenz zwischen den Diensten umfassen. Eine Transaktion zum endgültigen konsistent besteht aus einer Reihe von verteilten Schritten. Daher bei der die Benutzerprofil Microservice Befehls UpdateUser empfängt, aktualisiert die Benutzerdetails in seiner Datenbank und veröffentlicht das UserUpdated-Ereignis an den Ereignisbus. Die Warenkorb-Microservice und die Reihenfolge Microservice abonniert haben, um dieses Ereignis zu empfangen und Reaktion Updateinformationen ihre Käufer in der jeweils zugehörigen Datenbanken haben.

> [!NOTE]
> Der eShopOnContainers-Ereignisbus mit RabbitMQ, implementiert dient nur als ein Proof of Concept verwendet werden. Für Produktionssysteme sollte der alternative Ereignis Bus Implementierungen angesehen werden.

Informationen zu Servicebus-Implementierung von Ereignissen finden Sie unter [.NET Microservices: Architektur für .NET-Anwendungen in Containern](https://aka.ms/microservicesebook).

## <a name="summary"></a>Zusammenfassung

Microservices bieten einen Ansatz für die Anwendungsentwicklung und Bereitstellung, die für moderne Cloudanwendungen die Flexibilität, Skalierung und Zuverlässigkeit Anforderungen geeignet ist. Einer der Hauptvorteile der Microservices ist, sie dezentral skalierte unabhängig was bedeutet sein können, dass für ein bestimmten Funktionsbereich skaliert werden kann, die weitere Verarbeitung Power oder die Netzwerkbandbreite bei Bedarf, ohne unnötigerweise Skalierung Bereiche unterstützen erforderlich sind. die Anwendung, die keine erhöhten Bedarf auftreten.

Ein Container ist eine isolierte, Ressourcen gesteuerte und portierbare betriebsumgebung, in denen eine Anwendung ohne direkten Zugriff auf die Ressourcen von anderen Containern oder der Host ausgeführt werden kann. Unternehmen sind zunehmend Container eingeführt, bei der Implementierung Microservice-basierte Anwendungen und Docker geworden ist der Standardcontainer-Implementierung, die von den meisten Softwareplattformen und Cloud-Anbietern verbreitet ist.


## <a name="related-links"></a>Verwandte Links

- [Download-e-Book (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (Beispiel)](https://github.com/dotnet-architecture/eShopOnContainers)
