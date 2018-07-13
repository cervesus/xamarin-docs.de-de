---
title: Zusammenfassung der Kapitel 9. Clientplattform-spezifische API-Aufrufe
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung der Kapitel 9. Clientplattform-spezifische API-Aufrufe'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 4FFA1BD4-B3ED-461C-9B00-06ABF70D471D
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 8a035da3dec468df291a19849ca89964c6707589
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994756"
---
# <a name="summary-of-chapter-9-platform-specific-api-calls"></a>Zusammenfassung der Kapitel 9. Clientplattform-spezifische API-Aufrufe

Manchmal ist es erforderlich, zum Ausführen von Code, der die variiert je nach Plattform. In diesem Kapitel werden die Techniken.

## <a name="preprocessing-in-the-shared-asset-project"></a>In den freigegebenen Ressourcenprojekt vorverarbeitung

Eine Xamarin.Forms freigegebenen Projekts kann anderen Code für jede Plattform, die C#-Präprozessordirektiven ausführen `#if`, `#elif`, und `endif`. Dies wird im dargestellt [ **PlatInfoSap1**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap1):

[![Dreifacher Screenshot der Variable formatierten Absatz](images/ch09fg01-small.png "Gerätemodell und Betriebssystem")](images/ch09fg01-large.png#lightbox "Gerätemodell und Betriebssystem")

Der resultierende Code kann jedoch unschöne und schwer lesbar sein.

## <a name="parallel-classes-in-the-shared-asset-project"></a>Parallele Klassen in der freigegebenen Projekts

Ein strukturierter Ansatz für die plattformspezifischen Code ausführen, die von SAP wird veranschaulicht, der [ **PlatInfoSap2** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap2) Beispiel. Jedes der Plattformprojekte enthält eine Klasse mit dem gleichen Namen und die Methoden, aber für diese bestimmte Plattform implementiert. SAP klicken Sie dann einfach die Klasse instanziiert, und ruft die Methode.

## <a name="dependencyservice-and-the-portable-class-library"></a>DependencyService und der portablen Klassenbibliothek

Eine Bibliothek kann nicht normal Klassen in Anwendungsprojekten zugreifen. Diese Einschränkung ist offenbar zu verhindern, dass die Technik in **PlatInfoSap2** in einer PCL verwendet werden. Xamarin.Forms enthält jedoch eine Klasse namens [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) , verwendet .NET Reflection öffentlichen Zugriff auf Klassen im Anwendungsprojekt aus der PCL.

Definieren, muss die PCL eine `interface` mit den Elementen, die in jede Plattform verwendet werden muss. Klicken Sie dann, enthält jede der Plattformen eine Implementierung dieser Schnittstelle. Die Klasse, die die Schnittstelle implementiert muss identifiziert werden, mit einem [DependencyAttribute](xref:Xamarin.Forms.DependencyAttribute) auf Assemblyebene.

Die PCL verwendet dann den generischen [ `Get` ](xref:Xamarin.Forms.DependencyService.Get*) -Methode der `DependencyService` zum Abrufen von einer Instanz der Plattform-Klasse, die die Schnittstelle implementiert.

Dies wird veranschaulicht, der [ **DisplayPlatformInfo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/DisplayPlatformInfo) Beispiel.

## <a name="platform-specific-sound-generation"></a>Clientplattform-spezifische tongenerierung

Die [ **MonkeyTapWithSound** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/MonkeyTapWithSound) Beispiel fügt Signaltöne zu den **MonkeyTap** Programm durch den Zugriff auf Funktionen der Sound-Erstellung in jede Plattform.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 9 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch09-Apr2016.pdf)
- [Kapitel 9-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09)
- [Abhängigkeitsdienst](~/xamarin-forms/app-fundamentals/dependency-service/index.md)
