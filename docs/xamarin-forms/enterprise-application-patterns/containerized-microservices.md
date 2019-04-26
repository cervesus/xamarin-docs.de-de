---
title: Containermicroservices
description: In diesem Kapitel wird erläutert, wie Sie Microservices und Container verwenden, erstellen Sie flexible, skalierbare und zuverlässige moderne Cloudanwendungen.
ms.prod: xamarin
ms.assetid: 5872ad92-04e0-4f1a-9691-79d5602f5683
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 33be84bc17f72c8b70d117a0742b001f1f763d3d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61300763"
---
# <a name="containerized-microservices"></a>Containermicroservices

Entwicklung von Client-/ serveranwendungen führte in schwerpunktmäßig im Aufbau mehrstufiger Anwendungen, die auf jeder Ebene spezielle Technologien verwendet werden. Solche Anwendungen werden häufig als bezeichnet *monolithische* Anwendungen und sind verpackt werden, auf Hardware, die bereits skaliert für Spitzenlasten. Die größere Nachteile dieses Ansatzes für die Entwicklung sind die enge Kopplung zwischen Komponenten in den einzelnen Tarifen, dass einzelne Komponenten nicht problemlos skaliert werden können, und die Kosten für den Test. Eine einfache Aktualisierung haben dann unvorhergesehene Auswirkungen auf den Rest der Ebene, und eine Änderung an einer Anwendungskomponente erforderlich, dass die gesamte Ebene erneut getestet und bereitgestellt werden.

Besonders zu im Zeitalter der Cloud, dass einzelne Komponenten problemlos möglich skaliert wird. Eine monolithische Anwendung enthält domänenspezifische Funktionen und ist Funktionsebenen wie Front-End, Geschäftslogik und die Speicherung von Daten in der Regel unterteilt. Eine monolithische Anwendung wird durch Klonen die gesamte Anwendung auf mehreren Computern skaliert, wie in Abbildung 8-1 dargestellt.

![](containerized-microservices-images/monolithicapp.png "Skalierungsansatz monolithischen Anwendung")

**Abbildung 8-1**: Skalierungsansatz monolithischen Anwendung

## <a name="microservices"></a>Microservices

Microservices bieten einen anderen Ansatz für die Anwendungsentwicklung und-Bereitstellung, einen Ansatz, der geeignet ist, um die Flexibilität, Skalierung und zuverlässigkeitsanforderungen moderne Cloudanwendungen. Eine Microservices-Anwendung wird in unabhängige Komponenten zerlegt, die zusammenarbeiten, um die Funktionalität der Anwendung insgesamt zu übermitteln. Der Begriff Microservice hebt hervor, dass Anwendungen Dienste klein genug, um unabhängige Belangen unterstützen, entsprechend zusammengesetzt werden sollte, damit jeder Microservice über eine einzelne Funktion implementiert. Darüber hinaus verfügt jeder Microservice klar definierte Verträge, damit andere Microservices kommunizieren und Daten freigeben können. Typische Beispiele für Microservices enthalten, Einkaufswagen, verarbeiten inventarisieren, erwerben, Subsysteme und Zahlungsabwicklung.

Microservices können unabhängig voneinander und im Vergleich zur riesigen monolithischen Anwendungen Horizontales Skalieren, die zusammen skaliert werden. Dies bedeutet, dass für ein bestimmten Funktionsbereich, der mehr verarbeitungsleistung oder Netzwerkbandbreite zum Erfüllen der Nachfrage erforderlich ist, unnötig dezentrale Skalierung, andere Bereiche der Anwendung skaliert werden kann. Abbildung 8 – 2 veranschaulicht diese Vorgehensweise, wobei Microservices bereitgestellt und unabhängig voneinander skaliert werden, Erstellen von Instanzen der Dienste über Computer hinweg.

![](containerized-microservices-images/microservicesapp.png "Skalierungsansatz für Microservices-Anwendung")

**Abbildung 8-2**: Skalierungsansatz für Microservices-Anwendung

Microservice-Skalierung kann nahezu sofortige, sodass eine Anwendung in Anpassung an wechselnde Lasten. Beispielsweise kann ein einzelner Microservice in den Webzugriff Funktionalität einer Anwendung der einzige Microservice in der Anwendung sein, horizontal hochskalieren, um zusätzliche eingehenden Datenverkehr zu verarbeiten.

Das klassische Modell für die Skalierbarkeit der Anwendung ist, um einen Lastenausgleich, zustandslose Tarif mit externen freigegebenen Datenspeicher zum Speichern persistenter Daten zu erhalten. Zustandsbehaftete Microservices verwalten ihre eigenen persistenten Daten, die in der Regel auf den Servern, auf denen sie gespeichert werden, lokal speichern, um den Aufwand für das Netzwerk zu vermeiden Zugriff und die Komplexität der Cross-Service-Vorgänge. Dies ermöglicht die schnellste Möglichkeit Verarbeitung von Daten und kann nicht mehr erforderlich für die Zwischenspeicherung von Systemen. Darüber hinaus Partitionieren Sie skalierbare zustandsbehaftete Microservices in der Regel Daten für ihre Instanzen Datendurchsatz Datengröße und zu verwalten, jenseits derer ein einzelner Server unterstützen kann.

Microservices unterstützen auch unabhängige Updates. Diese lose Kopplung zwischen Microservices stellt eine Weiterentwicklung von schnelle und zuverlässige Anwendungen bereit. Unabhängige, verteilten Natur unterstützt Rollierende Updates, die, in denen nur eine Teilmenge von Instanzen von einem einzelnen Microservice zu jedem Zeitpunkt aktualisiert wird. Aus diesem Grund, wenn ein Problem erkannt wird, kann ein fehlerhaften Updates, zurückgesetzt werden, bevor alle Instanzen mit der fehlerhaften Code oder Konfiguration zu aktualisieren. Auf ähnliche Weise verwenden Microservices in der Regel versionsverwaltung des Schemas, damit Clients eine einheitliche Version sehen, wann Updates unabhängig davon, welche Microservice Instanz mit übermittelt wird, werden angewendet werden.

Aus diesem Grund haben microserviceanwendungen monolithischer Anwendungen viele Vorteile:

-   Microservices sind relativ klein und leicht zu verwalten und zu entwickeln.
-   Jeder Microservice kann entwickelt und unabhängig von anderen Diensten bereitgestellt werden.
-   Jeder Microservice kann unabhängig voneinander horizontal skalierten sein werden. Z. B. eine Catalog-Dienst oder den Einkaufswagen Warenkorbdienst müssen möglicherweise horizontal skalierten ein bestelldienst enthalten sein. Aus diesem Grund wird die resultierende Infrastruktur Ressourcen effizienter nutzen, beim horizontalen hochskalieren.
-   Jeder Microservice isoliert Probleme. Beispielsweise liegt ein Problem in einem Dienst es wirkt sich nur auf diesen Dienst. Die anderen Dienste können weiterhin Anforderungen verarbeiten.
-   Jeder Microservice können die neuesten Technologien. Da Microservices autonomen und zur Seite-an-Seite handelt, können die neuesten Technologien und Frameworks verwendet werden anstatt ein älteres Framework verwenden, das möglicherweise von einer monolithischen Anwendung verwendet werden.

Eine auf microservices basierende Lösung hat jedoch auch mögliche Nachteile:

-   Auswählen, wie eine Anwendung in Microservices partitionieren kann schwierig sein, da jeder Microservice ist vollkommen unabhängig, End-to-End, einschließlich der Verantwortung für die Datenquellen.
-   Entwickler müssen Kommunikation zwischen Diensten, implementieren, die Komplexität und Latenz der Anwendung hinzugefügt.
-   Atomarische Transaktionen zwischen mehreren Microservices in der Regel nicht möglich sind. Aus diesem Grund müssen die geschäftlichen Anforderungen letztlichen Konsistenz zwischen Microservices nutzen.
-   Es gibt eine operative Komplexität bei der Bereitstellung und Verwaltung eines Systems, die von vielen unabhängigen Diensten gefährdet, in der Produktion.
-   Direkter Client-zu-Microservice-Kommunikation kann auf die Verträge von Microservices Umgestalten erschweren. Z. B. im Laufe der Zeit wie das System in Dienste partitioniert wird möglicherweise ändern möchten. Ein einzelner Dienst kann in zwei oder mehr Dienste aufgeteilt, und zwei Dienste zusammenführen können. Wenn Clients direkt mit Microservices kommunizieren zu können, kann diese umgestaltungsarbeit Kompatibilität mit Client-apps unterbrechen.

## <a name="containerization"></a>Containerisierung

Das containerisieren ist ein Ansatz für Softwareentwicklung, die in dem eine Anwendung und die versionierter Satz von Abhängigkeiten sowie die Konfiguration der Umgebung abstrahiert als Bereitstellungsmanifestdateien, als ein Container-Abbild, das als Einheit getestet zusammengefasst werden und auf einem Host-Betriebssystem bereitgestellt.

Ein Container ist eine isolierte, Ressourcen gesteuerte und portierbare betriebsumgebung, in denen eine Anwendung ohne direkten Zugriff auf die Ressourcen der anderen Containern oder der Host ausgeführt werden kann. Aus diesem Grund wird ein Container sieht aus, und verhält sich wie ein neu installierter physischen Computer oder einen virtuellen Computer.

Es gibt viele ähnlichkeiten zwischen Containern und virtuellen Computern, wie in Abbildung 8-3 dargestellt.

![](containerized-microservices-images/containersvsvirtualmachines.png "Skalierungsansatz für Microservices-Anwendung")

**Abbildung 8-3**: Vergleich der virtuelle Computer und Container

Ein Container ein Betriebssystem ausgeführt wird, verfügt über ein Dateisystem und kann über ein Netzwerk zugegriffen werden, als wäre er einem physischen oder virtuellen Computer. Allerdings unterscheiden sich die Technologie und die Konzepte von Container sehr von virtuellen Computern. Virtuelle Computer enthalten, die Anwendungen, die erforderlichen Abhängigkeiten und ein vollständiges Gastbetriebssystem. Container enthalten die Anwendung und ihre Abhängigkeiten, aber andere Container, die als isolierte Prozesse ausgeführt werden, auf dem Host-Betriebssystem (abgesehen von der Hyper-V-Container die innerhalb eines speziellen virtuellen Computers pro Container ausgeführt werden.) das Betriebssystem freigegeben. Aus diesem Grund werden die Container Freigeben von Ressourcen und erfordern in der Regel weniger Ressourcen als bei virtuellen Computern.

Der Vorteil eines Container-orientierten Entwicklung und Bereitstellung-Ansatzes ist, dass es die meisten Probleme beseitigt, die auftreten, die von inkonsistenten Umgebung Setups und die Probleme, die sie die im Lieferumfang. Darüber hinaus ermöglichen Container schnell hochskalieren Anwendungsfunktionalität, durch die Instanziierung der neue Container nach Bedarf.

Die wichtigsten Konzepte beim Erstellen und Arbeiten mit Containern sind:

-   Container-Host: Die physischen oder virtuellen Computer zum Hosten von Containern konfiguriert. Die Container-Host wird auf einen oder mehrere Container ausgeführt.
-   Containerimage: Ein Image besteht aus einer Union von Ebenen übereinander gestapelt Dateisysteme und bildet die Grundlage eines Containers. Ein Bild enthält keinen Zustand, und es nie ändert, wie es in verschiedenen Umgebungen bereitgestellt wird.
-   Container: Ein Container ist eine Laufzeitinstanz eines Bilds.
-   Containerbetriebssystem-Image: Container werden aus Images bereitgestellt. Das Betriebssystemabbild Container ist der ersten Ebene möglicherweise viele Bildebenen, aus denen ein Container besteht. Ein Container-Betriebssystem ist unveränderlich und kann nicht geändert werden.
-   Container-Repository: Jedes Mal, wenn ein containerimage erstellt wurde, werden die Images und seiner Abhängigkeiten in einem lokalen Repository gespeichert. Diese Images können oft auf dem containerhost wiederverwendet werden. Die containerimages können auch wie z. B. in einer öffentlichen oder privaten Registrierung gespeichert [Docker Hub](https://hub.docker.com/), sodass sie über die verschiedenen containerhosts verwendet werden können.

Unternehmen setzen zunehmend Container bei der Implementierung von Microservice-basierte Anwendungen und Docker seit der Standardcontainer-Implementierung, die von den meisten Softwareplattformen und Cloud-Anbieter sehr geschätzt wird.

Der referenzanwendung "eshoponcontainers" verwendet Docker, um das Hosten von Microservices in Containern Back-End-vier, wie in Abbildung 8-4 dargestellt.

![](containerized-microservices-images/microservicesarchitecture.png "\"eshoponcontainers\" verweisen auf die Anwendung-Back-End-microservices")

**Abbildung 8-4**: "eshoponcontainers" verweisen auf die Anwendung-Back-End-Microservices

Die Architektur der Back-End-Dienste in der referenzanwendung wird in mehreren autonomen untergeordneten Systemen in Form von zusammenarbeitenden Microservices und Containern zerlegt. Jeder Microservice stellt einen einzelnen Bereich der Funktionalität bereit: einen Identitätsdienst, eine Catalog-Dienst, ein bestelldienst enthalten und ein Warenkorbdienst.

Jeder Microservice verfügt über seine eigene Datenbank, sodass sie vollständig von anderen Microservices entkoppelt werden. Bei Bedarf erfolgt die Konsistenz zwischen Datenbanken von anderen Microservices über Ereignisse auf Anwendungsebene. Weitere Informationen finden Sie unter [Kommunikation zwischen Microservices](#communication_between_microservices).

Weitere Informationen zu der referenzanwendung, finden Sie unter [.NET Microservices: NET Microservices: Architecture for Containerized .NET Applications (.NET Microservices: Architektur für .NET-Containeranwendungen)](https://aka.ms/microservicesebook).

<a name="communication_between_client_and_microservices" />

## <a name="communication-between-client-and-microservices"></a>Kommunikation zwischen Client und Microservices

Die eShopOnContainers-mobile-app kommuniziert mit dem Back-End-Microservices in Containern mit *direkte Client-zu-Microservice* Kommunikation, die in Abbildung 8-5 gezeigt wird.

![](containerized-microservices-images/directclienttomicroservicecommunication.png "Skalierungsansatz für Microservices-Anwendung")

**Abbildung 8-5**: Direkte Kommunikation zwischen Client und Microservice

Bei der direkten Kommunikation von Client-zu-Microservice stellt die mobile app Anforderungen für jeden Microservice direkt über seinen öffentlichen Endpunkt mit einem anderen TCP-Port pro Microservice. In einer produktionsumgebung würde der Endpunkt in der Regel dem Microservice für den Load Balancer zuzuordnen, die Anforderungen über die verfügbaren Instanzen verteilt.

> [!TIP]
> Erwägen Sie die Verwendung von API-Gateway-Kommunikation. Direkter Client-zu-Microservice-Kommunikation haben Nachteile beim Erstellen eines großen und komplexen microservices Anwendung basierenden, aber es mehr als ausreichend für eine kleine Anwendung ist. Beim Entwerfen eines großen-microservices-Anwendung mit Dutzenden microservices basieren, sollten erwägen Sie, API-Gateway-Kommunikation zu verwenden. Weitere Informationen finden Sie unter [.NET Microservices: NET Microservices: Architecture for Containerized .NET Applications (.NET Microservices: Architektur für .NET-Containeranwendungen)](https://aka.ms/microservicesebook).

<a name="communication_between_microservices" />

## <a name="communication-between-microservices"></a>Kommunikation zwischen Microservices

Eine-basierten microserviceanwendung ist ein verteiltes System, das möglicherweise auf mehreren Computern ausgeführt werden. Jede Dienstinstanz ist in der Regel ein Prozess. Aus diesem Grund müssen Dienste interagieren mit der ein prozessinternes Kommunikationsprotokoll wie HTTP, TCP, Advanced Message Queuing Protocol (AMQP) oder binäre Protokolle, je nach Art des jeweiligen Diensts.

Die zwei gängige Vorgehensweisen für die Microservice-zu-Microservice-Kommunikation sind HTTP-basierten REST-Kommunikation beim Abfragen von Daten und asynchrones lightweight-messaging bei der Kommunikation von Updates über mehrere Microservices.

Asynchrone messaging basierend ereignisgesteuerte Kommunikation ist wichtig, beim Weitergeben von Änderungen in mehreren Microservices. Bei diesem Ansatz veröffentlicht ein Microservice ein Ereignis, wenn etwas, beachten Sie, z. B. geschieht, wenn es sich um eine Geschäftseinheit aktualisiert. Andere Microservices abonnieren diese Ereignisse aus. Wenn ein Microservice ein Ereignis empfängt, aktualisiert sie klicken Sie dann die eigenen Geschäftseinheiten, die wiederum führen könnten, um weitere Ereignisse veröffentlicht werden. Dieser Veröffentlichen / Abonnieren-Funktionen in der Regel mit einem Ereignisbus erreicht ist.

Der Ereignisbus ermöglicht Veröffentlichen / Abonnieren-Kommunikation zwischen Microservices, ohne die Komponenten, explizit beachtet werden, wie in Abbildung 8-6 gezeigt.

![](containerized-microservices-images/eventbus.png "Veröffentlichen / Abonnieren Sie mit einem Ereignisbus")

**Abbildung 8-6:** Veröffentlichen / Abonnieren Sie mit einem Ereignisbus

Aus Anwendungssicht ist im Ereignisbus einfach eine Veröffentlichen-Abonnieren-Kanal, über eine Schnittstelle verfügbar gemacht. Die Art die Implementierung der Ereignisbus kann jedoch variieren. Beispielsweise kann eine Implementierung von ereignisbussen RabbitMQ, Azure Service Bus oder andere Service Busse wie NServiceBus und MassTransit verwenden. Abbildung 8-7 zeigt, wie ein Ereignisbus in der referenzanwendung "eshoponcontainers" verwendet wird.

![](containerized-microservices-images/microservicesarchitecturewitheventbus.png "Asynchrone ereignisgesteuerte Kommunikation in der referenzanwendung")

**Abbildung 8-7:** Asynchrone ereignisgesteuerte Kommunikation in der referenzanwendung

1: n-asynchronen Funktionalität Veröffentlichungs-und bietet im "eshoponcontainers" Ereignisbus mit RabbitMQ, implementiert. Dies bedeutet, dass nach dem Veröffentlichen eines Ereignisses, es kann mehrere Abonnenten, die für das gleiche Ereignis lauscht. Abbildung 8-9 zeigt diese Beziehung.

![](containerized-microservices-images/eventdrivencommunication.png "1: n Kommunikation")

**Abbildung 8-9**: 1: n Kommunikation

Dieser Ansatz 1: n Kommunikation verwendet Ereignisse, um Geschäftstransaktionen zu implementieren, die sicherstellen von letztlichen Konsistenz zwischen den Diensten mehrere Dienste erstrecken. Eine letztlich konsistente Transaktion besteht aus einer Reihe von verteilten Schritten. Aus diesem Grund bei der Benutzerprofil Microservice der UpdateUser-Befehl empfängt, aktualisiert die Details des Benutzers in der Datenbank und das UserUpdated-Ereignis im Ereignisbus veröffentlicht. Sowohl die Warenkorb-Microservice und der Microservice für Bestellungen haben zum Empfangen dieses Ereignisses und aktualisieren ihre Käufer Informationen in ihren jeweiligen Datenbanken als Reaktion abonniert.

> [!NOTE]
> Im "eshoponcontainers" Ereignisbus mit RabbitMQ, implementiert wird nur als Proof of Concept verwendet werden soll. Für Produktionssysteme sollte alternative Event Bus Implementierungen angesehen werden.

Informationen über die Implementierung von ereignisbussen, finden Sie unter [.NET Microservices: NET Microservices: Architecture for Containerized .NET Applications (.NET Microservices: Architektur für .NET-Containeranwendungen)](https://aka.ms/microservicesebook).

## <a name="summary"></a>Zusammenfassung

Microservices bieten einen Ansatz zur Anwendungsentwicklung und-Bereitstellung, die für die Anforderungen für Agilität, Skalierbarkeit und Zuverlässigkeit von modernen Cloudanwendungen geeignet ist. Einer der wichtigsten Vorteile von Microservices ist, sie horizontal hochskalierte unabhängig Dies bedeutet sein können, dass für ein bestimmten Funktionsbereich skaliert werden kann, die mehr verarbeitungsleistung oder Netzwerkbandbreite zum Erfüllen der Nachfrage, ohne unnötig Skalierung Bereichen erforderlich sind die Anwendung, auf denen höhere Nachfrage nicht auftreten.

Ein Container ist eine isolierte, Ressourcen gesteuerte und portierbare betriebsumgebung, in denen eine Anwendung ohne direkten Zugriff auf die Ressourcen der anderen Containern oder der Host ausgeführt werden kann. Unternehmen setzen zunehmend Container bei der Implementierung von Microservice-basierte Anwendungen und Docker seit der Standardcontainer-Implementierung, die von den meisten Softwareplattformen und Cloud-Anbieter sehr geschätzt wird.


## <a name="related-links"></a>Verwandte Links

- [E-Book (2Mb PDF-Datei) herunterladen](https://aka.ms/xamarinpatternsebook)
- ["eshoponcontainers" (GitHub) (Beispiel)](https://github.com/dotnet-architecture/eShopOnContainers)
