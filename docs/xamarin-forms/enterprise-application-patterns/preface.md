---
title: Ihm etwas voranzustellen, für die Anwendungsentwicklung für Unternehmen
description: Dieses Kapitel enthält ein Präfix zu Mustern von Unternehmensanwendungen mit Xamarin.Forms.
ms.prod: xamarin
ms.assetid: fbf32a44-1d33-4e16-a904-dc7ee5991e7c
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: fd085d2fb12e82233f6d3be2e2773a84539f837b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61298867"
---
# <a name="preface-to-enterprise-app-development"></a>Ihm etwas voranzustellen, für die Anwendungsentwicklung für Unternehmen

Dieses e-Book enthält Anleitungen zum Erstellen von plattformübergreifenden-Unternehmens-apps mit Xamarin.Forms. Xamarin.Forms ist eine plattformübergreifende Benutzeroberflächen-Toolkit, mit dem Entwickler, um ganz einfach native Benutzeroberflächenlayouts erstellen, die Plattformen, einschließlich iOS, Android und die universelle Windows-Plattform (UWP) gemeinsam verwendet werden können. Es bietet eine umfassende Lösung für Unternehmen und Mitarbeitern (B2E), Business-to-Business (B2B) und Business-to Consumer (B2C)-apps bietet die Möglichkeit, Code auf allen Zielplattformen freigeben und gleichzeitig die Gesamtbetriebskosten (TCO) senken.

Das Handbuch enthält Anleitungen zur Architektur für die Entwicklung von anpassbaren, verwaltbaren und testbaren Xamarin.Forms-Unternehmens-apps. Anleitungen zum Implementieren von MVVM, Abhängigkeitsinjektion, Navigation, Validierung und konfigurationsverwaltung und losen Kopplung gleichzeitig enthalten. Darüber hinaus besteht auch Anleitungen zum Durchführen der Authentifizierung und Autorisierung mit Identity Server, den Zugriff auf Daten aus Microservices in Containern und Komponententests.

Die Anleitung enthält Quellcode für die ["eshoponcontainers" mobile app](https://github.com/dotnet-architecture/eShopOnContainers/tree/master/src/Mobile), und der Quellcode für die ["eshoponcontainers" verweisen auf die app](https://github.com/dotnet-architecture/eShopOnContainers). Die eShopOnContainers-mobile-app ist eine plattformübergreifende Unternehmens-app, die entwickelt wurden, verwenden Xamarin.Forms, mit dem eine mit einer Reihe von Microservices in Containern bezeichnet Verbindung, da eShopOnContainers app zu verweisen. Allerdings kann die eShopOnContainers-mobile-app konfiguriert werden, um Daten aus mock-Dienste für diejenigen zu nutzen, um zu vermeiden, die Microservices in Containern bereitstellen möchten.

## <a name="whats-left-out-of-this-guides-scope"></a>Nun muss nur noch außerhalb des gültigen Bereichs von dieser Anleitung

Dieses Handbuch ist für Leser gedacht, die bereits mit Xamarin.Forms vertraut sind. Eine ausführliche Einführung in Xamarin.Forms finden Sie unter den [Xamarin.Forms Dokumentation](~/xamarin-forms/index.yml), und [Erstellen mobiler Apps mit Xamarin.Forms](https://aka.ms/xamebook).

Das Handbuch ist eine Ergänzung zu [.NET Microservices: Architektur für .NET-containeranwendungen](https://aka.ms/microservicesebook), die Entwicklung und Bereitstellung von Microservices in Containern im Mittelpunkt. Anderen Handbüchern interessante [entwerfen und entwickeln moderner Webanwendungen mit ASP.NET Core und Microsoft Azure](http://aka.ms/WebAppEbook), [Containerized Docker-Anwendungslebenszyklus mit Microsoft Platform and Tools](http://aka.ms/dockerlifecycleebook), und [Microsoft-Plattform und Tools für die Entwicklung mobiler Apps](http://aka.ms/MobAppDev/StndPDF).

## <a name="who-should-use-this-guide"></a>Wer sollte dieses Handbuch verwenden

Die Zielgruppe für dieses Handbuch ist in erster Linie Entwickler und Architekten, die Informationen zum Entwerfen und implementieren plattformübergreifende Unternehmens-apps, die Xamarin.Forms verwenden möchten.

Eine zweite Zielgruppe sind technische Entscheidungsträger, die eine Übersicht über die Architektur und Technologie bei Ihrer Entscheidung Methode für die Auswahl für die Entwicklung von plattformübergreifenden Enterprise-Apps mit Xamarin.Forms erhalten sollen.

## <a name="how-to-use-this-guide"></a>Gewusst wie: Verwenden Sie dieses Handbuch

Dieser Leitfaden konzentriert sich auf das Erstellen von plattformübergreifenden-Unternehmens-apps mit Xamarin.Forms. Daher sollten sie in ihrer Gesamtheit, geben Sie eine Grundlage für solche apps und ihrer technischen Überlegungen zu verstehen gelesen werden. Die Anleitung zusammen mit der Beispiel-app dienen auch als Punkt oder die Referenz für das Erstellen einer neuen Unternehmens-app. Verwenden Sie die zugeordneten Beispiel-app als Vorlage für die neue app, und erfahren, wie Sie ein app-Komponenten zu organisieren. Klicken Sie dann, finden Sie es in dieser Anleitung finden Sie Anleitungen zur Architektur zurück.

Dieses Handbuch für Teammitglieder, um sicherzustellen, dass ein allgemeines Verständnis der plattformübergreifende Unternehmens-app-Entwicklung mit Xamarin.Forms weiterleiten können. Wenn alle Beteiligten arbeiten einen Standardsatz an Terminologie und die zugrunde liegenden Prinzipien wird Sie eine konsistente Anwendung von Architekturmustern und-Praktiken sichergestellt.


## <a name="related-links"></a>Verwandte Links

- [E-Book (2Mb PDF-Datei) herunterladen](https://aka.ms/xamarinpatternsebook)
- ["eshoponcontainers" (GitHub) (Beispiel)](https://github.com/dotnet-architecture/eShopOnContainers)
