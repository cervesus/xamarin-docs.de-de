---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a05090c18039f9d3a7f9376285ce2863e0482903
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139255"
---
# <a name="containerized-microservices"></a>Containermicroservices

Die Entwicklung von Client-Server-Anwendungen führte zu einem Schwerpunkt auf der Erstellung mehrstufiger Anwendungen, die bestimmte Technologien in den einzelnen Ebenen verwenden. Solche Anwendungen werden häufig als *monolithische* Anwendungen bezeichnet und sind auf Hardware verpackt, die für Spitzenlasten vorab skaliert ist. Die Haupt Nachteile dieses Entwicklungsansatzes sind die enge Kopplung zwischen den Komponenten innerhalb der einzelnen Ebenen, dass einzelne Komponenten nicht problemlos skaliert werden können und die Kosten für Tests. Ein einfaches Update kann auf den Rest der Ebene unvorhergesehene Auswirkungen haben. Daher muss für eine Änderung an einer Anwendungs Komponente die gesamte Ebene erneut getestet und erneut bereitgestellt werden.

Insbesondere in Bezug auf das Alter der Cloud besteht darin, dass einzelne Komponenten nicht problemlos skaliert werden können. Eine monolithische Anwendung enthält domänenspezifische Funktionen und wird in der Regel durch funktionale Ebenen wie Front-End, Geschäftslogik und Datenspeicher aufgeteilt. Eine monolithische Anwendung wird skaliert, indem die gesamte Anwendung auf mehreren Computern geklont wird, wie in Abbildung 8-1 dargestellt.

![](containerized-microservices-images/monolithicapp.png "Monolithic application scaling approach")

**Abbildung 8-1**: monolithischer Ansatz bei der Anwendungs Skalierung

## <a name="microservices"></a>Microservices

Microservices bieten einen anderen Ansatz für die Entwicklung und Bereitstellung von Anwendungen, einen Ansatz, der für die Agilität, Skalierbarkeit und Zuverlässigkeit von modernen cloudanwendungen geeignet ist. Eine-Dienst Anwendung wird in unabhängige Komponenten zerlegt, die zusammenarbeiten, um die gesamte Funktionalität der Anwendung bereitzustellen. Der Begriff "mikroservice" unterstreicht, dass Anwendungen aus Diensten bestehen, die klein genug sind, um unabhängige Aspekte widerzuspiegeln, sodass jeder-Dienst eine einzelne Funktion implementiert. Außerdem verfügt jeder-Webdienst über klar definierte Verträge, damit andere-Dienste Daten miteinander kommunizieren und freigeben können. Typische Beispiele für die-Dienste sind Einkaufswagen, Bestands Verarbeitung, Kauf Subsysteme und Zahlungs Verarbeitung.

Im Vergleich zu riesigen monolithischen Anwendungen, die eine gemeinsame Skalierung durchgeführt haben, kann die Skalierung von-Diensten unabhängig voneinander Dies bedeutet, dass ein bestimmter Funktionsbereich, der eine höhere Verarbeitungsleistung oder Netzwerkbandbreite erfordert, zur Unterstützung von Anforderungen skaliert werden kann, anstatt unnötige horizontale Skalierung für andere Bereiche der Anwendung zu erfordern. In Abbildung 8-2 wird dieser Ansatz veranschaulicht, bei dem microservices unabhängig bereitgestellt und skaliert werden, indem Instanzen von Diensten auf mehreren Computern erstellt werden.

![](containerized-microservices-images/microservicesapp.png "Microservices application scaling approach")

**Abbildung 8-2**: Skalieren der Skalierungs Methode für microservices

Das horizontale hochskalieren von Webdiensten kann nahezu unmittelbar erfolgen, sodass eine Anwendung sich an veränderliche Lasten anpassen kann. Beispielsweise kann ein einzelner-mikrodienst in der webseitigen Funktionalität einer Anwendung der einzige in der Anwendung benötigte-Dienst sein, der horizontal hochskaliert werden muss, um zusätzlichen eingehenden Datenverkehr zu verarbeiten.

Das klassische Modell für Anwendungs Skalierbarkeit besteht darin, eine Zustands lose Ebene mit Lastenausgleich und einem freigegebenen externen Datenspeicher zum Speichern von persistenten Daten zu erhalten. Zustands behaftete Mikro Dienste verwalten ihre eigenen persistenten Daten, speichern Sie normalerweise lokal auf den Servern, auf denen Sie platziert werden, um den mehr Aufwand für den Netzwerk Zugriff und die Komplexität von Dienst übergreifenden Vorgängen zu vermeiden. Dadurch wird die schnellstmögliche Verarbeitung von Daten ermöglicht, und die Notwendigkeit von Cache Systemen entfällt. Außerdem Partitionieren skalierbare Zustands behaftete Mikro Dienste normalerweise Daten zwischen ihren Instanzen, um die Datengröße und den Übertragungs Durchsatz zu verwalten, die über einen einzelnen Server hinaus unterstützt werden können.

Außerdem unterstützen die-Dienste unabhängige Updates. Diese lose Kopplung zwischen den-Diensten bietet eine schnelle und zuverlässige Anwendungsentwicklung. Ihre unabhängige, verteilte Natur unterstützt parallele Updates, bei denen nur eine Teilmenge der Instanzen eines einzelnen-mikrodienstanzen zu einem bestimmten Zeitpunkt aktualisiert wird. Wenn ein Problem festgestellt wird, kann ein fehlerhaftes Update zurückgesetzt werden, bevor alle Instanzen mit dem fehlerhaften Code oder der Konfiguration aktualisiert werden. Auf ähnliche Weise verwenden die-mikrodienste die Schema Versionsverwaltung, sodass Clients eine konsistente Version sehen, wenn Updates angewendet werden, unabhängig davon, mit welcher Instanz des-Diensts kommuniziert wird.

Daher haben Anwendungen für den Einsatz von Anwendungen viele Vorteile gegenüber monolithischen Anwendungen:

- Jeder Mikro Dienst ist relativ klein und leicht zu verwalten und zu entwickeln.
- Jeder-Dienst kann unabhängig von anderen Diensten entwickelt und bereitgestellt werden.
- Jeder-Dienst kann unabhängig voneinander horizontal hochskaliert werden. Beispielsweise kann es vorkommen, dass ein Katalog Dienst oder ein waren Korb Dienst mehr als einen Bestell Dienst horizontal hochskaliert werden muss. Aus diesem Grund werden Ressourcen beim horizontalen hochskalieren von der resultierenden Infrastruktur effizienter verbraucht.
- Jeder-mikrodienst isoliert alle Probleme. Wenn z. b. ein Problem in einem Dienst vorliegt, wirkt sich dies nur auf diesen Dienst aus. Die anderen Dienste können weiterhin Anforderungen verarbeiten.
- Jeder-Dienst kann die neuesten Technologien nutzen. Da es sich um unabhängige und parallel ausgewendende-Technologien handelt, können die neuesten Technologien und Frameworks verwendet werden, anstatt ein älteres Framework zu verwenden, das von einer monolithischen Anwendung verwendet werden kann.

Eine auf einem Dienst basierende Lösung bietet jedoch auch mögliche Nachteile:

- Die Entscheidung, wie eine Anwendung in mikrodienste partitioniert werden soll, kann eine Herausforderung darstellen, da jeder der einzelnen mikrodienste vollständig autonom sein muss, einschließlich der Verantwortung für seine Datenquellen.
- Entwickler müssen die Kommunikation zwischen Diensten implementieren, wodurch die Komplexität und Latenz der Anwendung erhöht wird.
- Atomarische Transaktionen zwischen mehreren Mikro Diensten sind in der Regel nicht möglich. Daher müssen Geschäftsanforderungen die letztliche Konsistenz zwischen microservices berücksichtigen.
- In der Produktionsumgebung gibt es eine funktionale Komplexität bei der Bereitstellung und Verwaltung eines Systems, das viele unabhängige Dienste kompromittiert hat.
- Die direkte Kommunikation zwischen Client und verwaltdienst erschwert das Umgestalten der Verträge von-Diensten. Im Laufe der Zeit muss sich z. b. die Art der Partitionierung des Systems in Dienste ändern. Ein einzelner Dienst kann in zwei oder mehr Dienste aufgeteilt werden, und zwei Dienste können zusammengeführt werden. Wenn Clients direkt mit den-Webdiensten kommunizieren, kann diese Umgestaltung die Kompatibilität mit Client-apps unterbrechen.

## <a name="containerization"></a>Containerisierung

Containerisierung ist ein Ansatz für die Softwareentwicklung, bei dem eine Anwendung und deren Versions abhängige Abhängigkeiten sowie die Umgebungs Konfiguration, die als Bereitstellungs Manifest-Dateien abstrahiert ist, als Container Image zusammengefasst, als Einheit getestet und auf einem Host Betriebssystem bereitgestellt werden.

Ein Container ist eine isolierte, Ressourcen gesteuerte und tragbare Betriebsumgebung, in der eine Anwendung ausgeführt werden kann, ohne die Ressourcen anderer Container oder den Host zu berühren. Ein Container sieht daher wie ein neu installierter physischer Computer oder eine virtuelle Maschine aus.

Es gibt viele Ähnlichkeiten zwischen Containern und virtuellen Computern, wie in Abbildung 8-3 dargestellt.

![](containerized-microservices-images/containersvsvirtualmachines.png "Microservices application scaling approach")

**Abbildung 8-3**: Vergleich von virtuellen Computern und Containern

Ein Container führt ein Betriebssystem aus, verfügt über ein Dateisystem und kann über ein Netzwerk aufgerufen werden, als ob es sich um einen physischen oder einen virtuellen Computer handelt. Die von Containern verwendeten Technologien und Konzepte unterscheiden sich jedoch stark von virtuellen Computern. Zu den virtuellen Computern zählen die Anwendungen, die erforderlichen Abhängigkeiten und ein vollständiges Gast Betriebssystem. Container enthalten die Anwendung und ihre Abhängigkeiten, geben jedoch das Betriebssystem für andere Container frei, die als isolierte Prozesse auf dem Host Betriebssystem ausgeführt werden (abgesehen von Hyper-V-Containern, die in einem speziellen virtuellen Computer pro Container ausgeführt werden). Daher verwenden Container Ressourcen gemeinsam und benötigen in der Regel weniger Ressourcen als virtuelle Computer.

Der Vorteil eines Container orientierten Entwicklungs-und Bereitstellungs Ansatzes besteht darin, dass die meisten der Probleme, die aus inkonsistenten Umgebungs Setups entstehen, und die darin enthaltenen Probleme beseitigt werden. Außerdem ermöglichen Container eine schnelle Anwendungs Skalierung, indem neue Container nach Bedarf instanziert werden.

Die wichtigsten Konzepte beim Erstellen und arbeiten mit Containern sind:

- Container Host: der physische oder virtuelle Computer, der zum Hosten von Containern konfiguriert ist. Der Container Host führt einen oder mehrere Container aus.
- Container Image: ein Bild besteht aus einer Vereinigung von geschichteten Dateisystemen, die aufeinander gestapelt sind, und ist die Grundlage für einen Container. Ein Image weist keinen Status auf und ändert sich nie, wenn es in unterschiedlichen Umgebungen bereitgestellt wird.
- Container: ein Container ist eine Lauf Zeit Instanz eines Bilds.
- Containerbetriebssystem-Image: Container werden mithilfe von Images bereitgestellt. Das Betriebssystem Abbild für den Container ist die erste Ebene in potenziell vielen Bildebenen, aus denen ein Container besteht. Ein Container Betriebssystem ist unveränderlich und kann nicht geändert werden.
- Containerrepository: jedes Mal, wenn ein Container Image erstellt wird, werden das Image und seine Abhängigkeiten in einem lokalen Repository gespeichert. Diese Images können auf dem Containerhost mehrfach wiederverwendet werden. Die Container Images können auch in einer öffentlichen oder privaten Registrierung, wie z. [b. docker Hub](https://hub.docker.com/), gespeichert werden, damit Sie auf verschiedenen Container Hosts verwendet werden können.

Unternehmen nehmen bei der Implementierung von auf einem Dienst basierenden Anwendungen immer mehr Container in Betrieb, und docker wurde zur Standardcontainer Implementierung, die von den meisten Softwareplattformen und cloudanbietern übernommen wurde.

Die eshoponcontainers-Referenz Anwendung verwendet docker zum Hosten von vier containerisierten Back-End-microservices, wie in Abbildung 8-4 dargestellt.

![](containerized-microservices-images/microservicesarchitecture.png "eShopOnContainers reference application back-end microservices")

**Abbildung 8-4**: eshoponcontainers Referenz für Anwendungs-Back-End-mikrodienste

Die Architektur der Back-End-Dienste in der Referenz Anwendung wird in mehrere autonome Subsysteme in Form von zusammengesetzten microservices und Containern zerlegt. Jeder microservice stellt einen einzelnen Funktionsbereich bereit: einen Identitäts Dienst, einen Katalog Dienst, einen Bestell Dienst und einen waren Korb Dienst.

Jeder-mikrodienst verfügt über eine eigene Datenbank, sodass er vollständig von den anderen-Diensten entkoppelt werden kann. Bei Bedarf wird die Konsistenz zwischen Datenbanken aus unterschiedlichen-Diensten mithilfe von Ereignissen auf Anwendungsebene erreicht. Weitere Informationen finden Sie unter [Kommunikation zwischen-Diensten](#communication_between_microservices).

Weitere Informationen zur Referenz Anwendung finden Sie unter [.net-microservices: Architektur für .NET-Container Anwendungen](https://aka.ms/microservicesebook).

<a name="communication_between_client_and_microservices" />

## <a name="communication-between-client-and-microservices"></a>Kommunikation zwischen Client und-Dienst

Der eshoponcontainers-Mobile App kommuniziert mit den containerisierten Back-End-microservices mithilfe der direkten Kommunikation zwischen *Client und microservice* , wie in Abbildung 8-5 dargestellt.

![](containerized-microservices-images/directclienttomicroservicecommunication.png "Microservices application scaling approach")

**Abbildung 8-5**: direkte Kommunikation zwischen Client und verwaltdienst

Bei der direkten Kommunikation zwischen Client und verwaltdienst stellt die Mobile App Anforderungen an jeden einzelnen mikrodienst direkt über seinen öffentlichen Endpunkt, mit einem anderen TCP-Port pro mikrodienst. In der Produktionsumgebung wird der Endpunkt in der Regel dem Load Balancer des-subdienstanzen zugeordnet, der Anforderungen über die verfügbaren Instanzen verteilt.

> [!TIP]
> Verwenden Sie die API-gatewaykommunikation. Die direkte Kommunikation zwischen Client und verwaltedienst kann bei der Erstellung einer großen und komplexen, auf einem Dienst basierenden Anwendung Nachteile haben, ist aber für eine kleine Anwendung mehr als ausreichend. Wenn Sie eine umfangreiche auf einem-Dienst basierende Anwendung mit Dutzenden von-Diensten entwickeln, empfiehlt es sich, die API-gatewaykommunikation Weitere Informationen finden Sie unter [.net-microservices: Architektur für .NET-Container Anwendungen](https://aka.ms/microservicesebook).

<a name="communication_between_microservices" />

## <a name="communication-between-microservices"></a>Kommunikation zwischen den-Diensten

Eine auf einem-Dienst basierende Anwendung ist ein verteiltes System, das möglicherweise auf mehreren Computern ausgeführt wird. Jede Dienstinstanz ist in der Regel ein Prozess. Daher müssen Dienste in Abhängigkeit von der Art der einzelnen Dienste mithilfe eines prozessübergreifenden Kommunikationsprotokolls wie http, TCP, Advance Message Queueing Protocol (AMQP) oder Binärprotokolle interagieren.

Die beiden gängigen Ansätze für die Kommunikation zwischen dem zwischen Dienst und dem mikrodienst sind HTTP-basierte Rest-Kommunikation beim Abfragen von Daten und einfachem asynchrones Messaging bei der Kommunikation von Updates über mehrere mikrodienste hinweg.

Die asynchrone Messaging basierte ereignisgesteuerte Kommunikation ist wichtig, wenn Änderungen über mehrere mikrodienste verteilt werden. Bei diesem Ansatz veröffentlicht ein microservice ein Ereignis, wenn etwas wichtiges passiert, z. b. beim Aktualisieren einer Geschäftseinheit. Diese Ereignisse werden von anderen-Diensten abonniert. Wenn ein microservice ein Ereignis empfängt, aktualisiert er dann seine eigenen Geschäfts Entitäten, was wiederum dazu führen kann, dass weitere Ereignisse veröffentlicht werden. Diese Funktion zum Veröffentlichen und abonnieren wird in der Regel mit einem Ereignisbus erreicht.

Ein Ereignisbus ermöglicht das Veröffentlichen/Abonnieren der Kommunikation zwischen-Diensten, ohne dass die Komponenten einander explizit erkennen müssen, wie in Abbildung 8-6 dargestellt.

![](containerized-microservices-images/eventbus.png "Publish-subscribe with an event bus")

**Abbildung 8-6:** Veröffentlichen: abonnieren mit einem Ereignisbus

Aus Anwendungs Sicht ist der Ereignisbus einfach ein veröffentlichen-abonnieren-Kanal, der über eine Schnittstelle verfügbar gemacht wird. Die Art und Weise, in der der Ereignisbus implementiert ist, kann jedoch variieren. Beispielsweise könnte eine Ereignisbus Implementierung rabbitmq, Azure Service Bus oder andere Dienst Busse wie nServiceBus und masstransit verwenden. In Abbildung 8-7 wird gezeigt, wie ein Ereignisbus in der eshoponcontainers-Referenz Anwendung verwendet wird.

![](containerized-microservices-images/microservicesarchitecturewitheventbus.png "Asynchronous event-driven communication in the reference application")

**Abbildung 8-7:** Asynchrone ereignisgesteuerte Kommunikation in der Referenz Anwendung

Der mit rabbitmq implementierte eshoponcontainers-Ereignisbus bietet eine 1: n-Funktion für asynchrone Veröffentlichung/abonnieren. Dies bedeutet, dass nach dem Veröffentlichen eines Ereignisses mehrere Abonnenten auf dasselbe Ereignis lauschen können. In Abbildung 8-9 wird diese Beziehung veranschaulicht.

![](containerized-microservices-images/eventdrivencommunication.png "One-to-many communication")

**Abbildung 8-9**: 1: n-Kommunikation

Dieser 1: n-Kommunikationsansatz verwendet Ereignisse, um Geschäftstransaktionen zu implementieren, die sich über mehrere Dienste erstrecken, um die letztliche Konsistenz zwischen den Diensten sicherzustellen. Eine letztlich konsistente Transaktion besteht aus einer Reihe von verteilten Schritten. Wenn der Benutzerprofil-mikrodienst den UpdateUser-Befehl empfängt, aktualisiert er daher die Details des Benutzers in seiner Datenbank und veröffentlicht das useraktualisierte-Ereignis im Ereignisbus. Sowohl der Warenkorb-microservice als auch der microservice für Bestellungen haben das Empfangen dieses Ereignisses abonniert und die Käufer Informationen in der Antwort in den jeweiligen Datenbanken aktualisiert.

> [!NOTE]
> Der mit rabbitmq implementierte eshoponcontainers-Ereignisbus soll nur als Proof of Concept verwendet werden. Für Produktionssysteme sollten alternative Ereignisbus Implementierungen berücksichtigt werden.

Weitere Informationen zur Ereignisbus Implementierung finden Sie unter [.net-microservices: Architektur für .NET-Container Anwendungen](https://aka.ms/microservicesebook).

## <a name="summary"></a>Zusammenfassung

Microservices bieten einen Ansatz für die Anwendungsentwicklung und-Bereitstellung, der sich auf die Flexibilität, Skalierbarkeit und Zuverlässigkeit von modernen cloudanwendungen eignet. Einer der Hauptvorteile von-Funktionen besteht darin, dass Sie unabhängig voneinander horizontal hochskaliert werden können. Dies bedeutet, dass ein bestimmter Funktionsbereich skaliert werden kann, der eine höhere Verarbeitungsleistung oder Netzwerkbandbreite erfordert, um die Nachfrage zu unterstützen, ohne dass Bereiche der Anwendung unnötig skaliert werden, die nicht mehr benötigt werden.

Ein Container ist eine isolierte, Ressourcen gesteuerte und tragbare Betriebsumgebung, in der eine Anwendung ausgeführt werden kann, ohne die Ressourcen anderer Container oder den Host zu berühren. Unternehmen nehmen bei der Implementierung von auf einem Dienst basierenden Anwendungen immer mehr Container in Betrieb, und docker wurde zur Standardcontainer Implementierung, die von den meisten Softwareplattformen und cloudanbietern übernommen wurde.

## <a name="related-links"></a>Verwandte Links

- [Download-e-Book (2 MB PDF)](https://aka.ms/xamarinpatternsebook)
- [eshoponcontainers (GitHub) (Beispiel)](https://github.com/dotnet-architecture/eShopOnContainers)
