---
title: Voranstellen der Entwicklung von Unternehmensanwendungen
description: Dieses Kapitel enthält einen Präfix für Unternehmens Anwendungs Muster mithilfe von Xamarin.Forms .
ms.prod: xamarin
ms.assetid: fbf32a44-1d33-4e16-a904-dc7ee5991e7c
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 6ea63fc483025fc6f9b0c7f379b6dfdc6ca30de8
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84198085"
---
# <a name="preface-to-enterprise-app-development"></a>Voranstellen der Entwicklung von Unternehmensanwendungen

Dieses e-Book enthält Anleitungen zum entwickeln plattformübergreifender Unternehmensanwendungen mit Xamarin.Forms . Xamarin.Formsist ein plattformübergreifendes Toolkit für die Benutzeroberfläche, mit dem Entwickler problemlos Native Benutzeroberflächen Layouts erstellen können, die plattformübergreifend freigegeben werden können, einschließlich IOS, Android und der universelle Windows-Plattform (UWP). Es bietet eine umfassende Lösung für Business-to-Employee-(B2E), Business-to-Business-(B2B) und Business-to-Consumer (B2C)-apps, die die Möglichkeit bieten, Code auf allen Zielplattformen gemeinsam zu nutzen und die Gesamtbetriebskosten zu senken.

Das Handbuch enthält Informationen zur Architektur für die Entwicklung anpassbarer, verwaltbarer und Test fähiger Xamarin.Forms Unternehmens-apps. Anleitungen zum Implementieren von MVVM, Abhängigkeitsinjektion, Navigation, Validierung und Konfigurations Verwaltung bei gleichzeitiger Beibehaltung der losen Kopplung. Außerdem gibt es Anleitungen zum Durchführen von Authentifizierung und Autorisierung mit identityserver, zum Zugreifen auf Daten aus containerisierten microservices und Unittests.

Das Handbuch enthält den Quellcode für die [eshoponcontainers-Mobile App](https://github.com/dotnet-architecture/eShopOnContainers/tree/master/src/Mobile)und den Quellcode für die [eshoponcontainers-Referenz-App](https://github.com/dotnet-architecture/eShopOnContainers). Der eshoponcontainers-Mobile App ist eine plattformübergreifende Unternehmens-APP Xamarin.Forms , die mit entwickelt wurde und eine Verbindung mit einer Reihe von microservices in Containern herstellt, die als eshoponcontainers-Referenz-App bezeichnet werden. Allerdings können die eshoponcontainers-Mobile APP so konfiguriert werden, dass Sie Daten von Pseudo Diensten für diejenigen verwendet, die die Bereitstellung der in Containern enthaltenen microservices vermeiden möchten.

## <a name="whats-left-out-of-this-guides-scope"></a>Der Umfang dieses Handbuchs ist ausgelassen.

Dieses Handbuch richtet sich an Leser, die bereits mit vertraut sind Xamarin.Forms . Eine ausführliche Einführung in finden Sie in Xamarin.Forms der- [ Xamarin.Forms Dokumentation](~/xamarin-forms/index.yml)und unter [Erstellen von Xamarin.Forms Mobile Apps mit ](https://aka.ms/xamformsebook).

Das Handbuch ist eine Ergänzung zu [.net-microservices: Architektur für .NET-Container Anwendungen](https://aka.ms/microservicesebook)mit Schwerpunkt auf der Entwicklung und Bereitstellung von microservices in Containern. Andere Anleitungen, die Sie lesen sollten, sind das Entwerfen [und entwickeln moderner Webanwendungen mit ASP.net Core und Microsoft Azure](https://aka.ms/WebAppEbook), der [containerisierte docker-Anwendungslebenszyklus mit Microsoft-Plattform und-Tools](https://aka.ms/dockerlifecycleebook)sowie der [Microsoft-Plattform und Tools für die Entwicklung mobiler apps](https://aka.ms/MobAppDev/StndPDF).

## <a name="who-should-use-this-guide"></a>Verwendung dieses Handbuchs

Die Zielgruppe für dieses Handbuch ist hauptsächlich Entwickler und Architekten, die erfahren möchten, wie Sie plattformübergreifende Unternehmens-Apps mithilfe von entwickeln und implementieren können Xamarin.Forms .

Bei einer sekundären Zielgruppe handelt es sich um technische Entscheidungsträger, die eine Architektur und Technologie Übersicht erhalten möchten, bevor Sie entscheiden, welche Methode Sie für die plattformübergreifende Entwicklung von Unternehmens-Apps verwenden möchten Xamarin.Forms .

## <a name="how-to-use-this-guide"></a>Zur Verwendungsweise dieses Handbuchs

Dieser Leitfaden konzentriert sich auf das entwickeln plattformübergreifender Unternehmensanwendungen mit Xamarin.Forms . Daher sollte Sie vollständig gelesen werden, um eine Grundlage für das Verständnis solcher apps und ihrer technischen Aspekte zu bieten. Das Handbuch kann zusammen mit seiner Beispiel-APP auch als Ausgangspunkt oder Verweis zum Erstellen einer neuen Unternehmens-App dienen. Verwenden Sie die zugehörige Beispiel-App als Vorlage für die neue APP, oder um zu erfahren, wie die Komponenten Komponenten einer APP organisiert werden. Weitere Informationen zur Architektur finden Sie in diesem Handbuch.

Sie können diese Anleitung an Teammitglieder weiterleiten, um ein gängiges Verständnis der plattformübergreifenden Entwicklung von Unternehmens-apps mit zu gewährleisten Xamarin.Forms . Wenn alle von einem gemeinsamen Satz von Terminologien und zugrunde liegenden Prinzipien aus arbeiten, wird eine konsistente Anwendung von Architekturmustern und-Praktiken sichergestellt.

## <a name="related-links"></a>Verwandte Links

- [Download-e-Book (2 MB PDF)](https://aka.ms/xamarinpatternsebook)
- [eshoponcontainers (GitHub) (Beispiel)](https://github.com/dotnet-architecture/eShopOnContainers)
