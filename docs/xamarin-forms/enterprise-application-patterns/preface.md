---
title: Voranstellen der Entwicklung von Unternehmensanwendungen
description: Dieses Kapitel enthält einen Präfix für Unternehmens Anwendungs Muster mithilfe von xamarin. Forms.
ms.prod: xamarin
ms.assetid: fbf32a44-1d33-4e16-a904-dc7ee5991e7c
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 4ce04ec5216872cb56424e8847eec357a5b3ac0e
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70770745"
---
# <a name="preface-to-enterprise-app-development"></a>Voranstellen der Entwicklung von Unternehmensanwendungen

Dieses e-Book bietet Anleitungen zum entwickeln plattformübergreifender Unternehmens-apps mit xamarin. Forms. Xamarin. Forms ist ein plattformübergreifendes Toolkit für die Benutzeroberfläche, mit dem Entwickler problemlos Native Benutzeroberflächen Layouts erstellen können, die plattformübergreifend freigegeben werden können, einschließlich IOS, Android und der universelle Windows-Plattform (UWP). Es bietet eine umfassende Lösung für Business-to-Employee-(B2E), Business-to-Business-(B2B) und Business-to-Consumer (B2C)-apps, die die Möglichkeit bieten, Code auf allen Zielplattformen gemeinsam zu nutzen und die Gesamtbetriebskosten zu senken.

Das Handbuch enthält Informationen zur Architektur für die Entwicklung anpassbarer, verwaltbarer und testbarer xamarin. Forms-Unternehmens-apps. Anleitungen zum Implementieren von MVVM, Abhängigkeitsinjektion, Navigation, Validierung und Konfigurations Verwaltung bei gleichzeitiger Beibehaltung der losen Kopplung. Außerdem gibt es Anleitungen zum Durchführen von Authentifizierung und Autorisierung mit identityserver, zum Zugreifen auf Daten aus containerisierten microservices und Unittests.

Das Handbuch enthält den Quellcode für die [eshoponcontainers-Mobile App](https://github.com/dotnet-architecture/eShopOnContainers/tree/master/src/Mobile)und den Quellcode für die [eshoponcontainers-Referenz-App](https://github.com/dotnet-architecture/eShopOnContainers). Der eshoponcontainers-Mobile App ist eine plattformübergreifende Unternehmens-APP, die mit xamarin. Forms entwickelt wurde und die eine Verbindung mit einer Reihe von microservices in Containern herstellt, die als eshoponcontainers-Referenz-App bezeichnet werden. Allerdings können die eshoponcontainers-Mobile APP so konfiguriert werden, dass Sie Daten von Pseudo Diensten für diejenigen verwendet, die die Bereitstellung der in Containern enthaltenen microservices vermeiden möchten.

## <a name="whats-left-out-of-this-guides-scope"></a>Der Umfang dieses Handbuchs ist ausgelassen.

Dieses Handbuch richtet sich an Leser, die bereits mit xamarin. Forms vertraut sind. Eine ausführliche Einführung in xamarin. Forms finden Sie in der [Dokumentation zu xamarin. Forms](~/xamarin-forms/index.yml)und [Erstellen von Mobile Apps mit xamarin. Forms](https://aka.ms/xamebook).

Das Handbuch ist eine Ergänzung [zu .net-mikrodiensten: Architektur für .NET-Container Anwendungen](https://aka.ms/microservicesebook)mit Schwerpunkt auf der Entwicklung und Bereitstellung von microservices in Containern. Andere Anleitungen, die Sie lesen sollten, sind das Entwerfen [und entwickeln moderner Webanwendungen mit ASP.net Core und Microsoft Azure](https://aka.ms/WebAppEbook), der [containerisierte docker-Anwendungslebenszyklus mit Microsoft-Plattform und-Tools](https://aka.ms/dockerlifecycleebook)und der [Microsoft-Plattform und Tools für die Entwicklung mobiler apps](https://aka.ms/MobAppDev/StndPDF).

## <a name="who-should-use-this-guide"></a>Verwendung dieses Handbuchs

Die Zielgruppe für dieses Handbuch ist hauptsächlich Entwickler und Architekten, die erfahren möchten, wie Sie plattformübergreifende Enterprise-Apps mithilfe von xamarin. Forms entwickeln und implementieren.

Bei einer sekundären Zielgruppe handelt es sich um technische Entscheidungsträger, die eine Architektur und Technologie Übersicht erhalten möchten, bevor Sie entscheiden, welchen Ansatz Sie für die plattformübergreifende Entwicklung von Unternehmens-apps mit xamarin. Forms treffen möchten.

## <a name="how-to-use-this-guide"></a>Verwendung dieses Handbuchs

Dieser Leitfaden konzentriert sich auf das entwickeln plattformübergreifender Unternehmens-apps mit xamarin. Forms. Daher sollte Sie vollständig gelesen werden, um eine Grundlage für das Verständnis solcher apps und ihrer technischen Aspekte zu bieten. Das Handbuch kann zusammen mit seiner Beispiel-APP auch als Ausgangspunkt oder Verweis zum Erstellen einer neuen Unternehmens-App dienen. Verwenden Sie die zugehörige Beispiel-App als Vorlage für die neue APP, oder um zu erfahren, wie die Komponenten Komponenten einer APP organisiert werden. Weitere Informationen zur Architektur finden Sie in diesem Handbuch.

Sie können diese Anleitung an Teammitglieder weiterleiten, um ein gängiges Verständnis der plattformübergreifenden Entwicklung von Unternehmens-Apps mithilfe von xamarin. Forms zu gewährleisten. Wenn alle von einem gemeinsamen Satz von Terminologien und zugrunde liegenden Prinzipien aus arbeiten, wird eine konsistente Anwendung von Architekturmustern und-Praktiken sichergestellt.

## <a name="related-links"></a>Verwandte Links

- [Download-e-Book (2 MB PDF)](https://aka.ms/xamarinpatternsebook)
- [eshoponcontainers (GitHub) (Beispiel)](https://github.com/dotnet-architecture/eShopOnContainers)
