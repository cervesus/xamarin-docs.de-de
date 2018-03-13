---
title: Zusammenfassung der Kapitel 9. Clientplattform-spezifische API-Aufrufe
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 4FFA1BD4-B3ED-461C-9B00-06ABF70D471D
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 84650c930445172d27520129123d493253851642
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-9-platform-specific-api-calls"></a>Zusammenfassung der Kapitel 9. Clientplattform-spezifische API-Aufrufe

Manchmal ist es erforderlich, zum Ausführen von Code, der die hängt von der Plattform. In diesem Kapitel werden die Techniken.

## <a name="preprocessing-in-the-shared-asset-project"></a>Vorverarbeiten im gemeinsamen Projekt

Xamarin.Forms freigegebenes Asset Projekt können anderen Code, der für jede Plattform, die von der C#-Präprozessordirektiven ausführen `#if`, `#elif`, und `endif`. Dies wird im dargestellt [ **PlatInfoSap1**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap1):

[![Dreifacher Screenshot Variablen formatierten Absatz](images/ch09fg01-small.png "Gerätemodell und Betriebssystem")](images/ch09fg01-large.png#lightbox "Gerätemodell und Betriebssystem")

Allerdings kann der resultierende Code problematischen und schwer zu lesen sein.

## <a name="parallel-classes-in-the-shared-asset-project"></a>Parallele Klassen in dem freigegebene Asset-Projekt

Ein strukturierter Ansatz zur Ausführung plattformspezifischen Code in der SAP wird veranschaulicht, der [ **PlatInfoSap2** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap2) Beispiel. Jede Plattformprojekte enthält eine Klasse mit identischem Namen und Methoden, aber für diese bestimmte Plattform implementiert. SAP klicken Sie dann einfach die Klasse instanziiert und die Methode aufgerufen.

## <a name="dependencyservice-and-the-portable-class-library"></a>DependencyService und der portablen Klassenbibliothek

Eine Bibliothek kann nicht normal Klassen in Anwendungsprojekten zugreifen. Diese Einschränkung zu verhindern, dass die Technik in scheint **PlatInfoSap2** in einer PCL verwendet wird. Xamarin.Forms enthält jedoch eine Klasse namens [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) , die .NET Reflektion verwendet, um die PCL öffentliche Klassen im Projekt für die Anwendung zuzugreifen.

Die PCL muss definieren eine `interface` mit den Elementen, die in jede Plattform verwenden muss. Alle Plattformen enthält eine Implementierung dieser Schnittstelle. Die Klasse, die die Schnittstelle implementiert muss angegeben werden, mit einem [DependencyAttribute](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyAttribute/) auf Assemblyebene.

Die PCL verwendet dann die generische [ `Get` ](https://developer.xamarin.com/api/member/Xamarin.Forms.DependencyService.Get{T}/p/Xamarin.Forms.DependencyFetchTarget/) Methode `DependencyService` zum Abrufen einer Instanz der Klasse Plattform, die die Schnittstelle implementiert.

Dies wird dargestellt, der [ **DisplayPlatformInfo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/DisplayPlatformInfo) Beispiel.

## <a name="platform-specific-sound-generation"></a>Clientplattform-spezifische sound generation

Die [ **MonkeyTapWithSound** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/MonkeyTapWithSound) Beispiel fügt Pieptöne, um die **MonkeyTap** Programm durch den Zugriff auf die Generierung der Sound Einrichtungen in jede Plattform.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 9 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch09-Apr2016.pdf)
- [Kapitel 9-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09)
- [Abhängigkeitsdienst](~/xamarin-forms/app-fundamentals/dependency-service/index.md)
