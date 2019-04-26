---
title: Zusammenfassung der Kapitel 9. Clientplattform-spezifische API-Aufrufe
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung der Kapitel 9. Clientplattform-spezifische API-Aufrufe'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 4FFA1BD4-B3ED-461C-9B00-06ABF70D471D
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: 3aec84ec6598a45bb989d4bbc1705fd797382755
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61334562"
---
# <a name="summary-of-chapter-9-platform-specific-api-calls"></a>Zusammenfassung der Kapitel 9. Clientplattform-spezifische API-Aufrufe

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09)

> [!NOTE] 
> Anmerkungen zu dieser Version auf dieser Seite Geben Sie Bereiche, in denen Xamarin.Forms aus den Informationen im Buch abweichend hat, an.

Manchmal ist es erforderlich, zum Ausführen von Code, der die variiert je nach Plattform. In diesem Kapitel werden die Techniken.

## <a name="preprocessing-in-the-shared-asset-project"></a>In den freigegebenen Ressourcenprojekt vorverarbeitung

Eine Xamarin.Forms freigegebenen Projekts kann anderen Code für jede Plattform, die C#-Präprozessordirektiven ausführen `#if`, `#elif`, und `endif`. Dies wird im dargestellt [ **PlatInfoSap1**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap1):

[![Dreifacher Screenshot der Variable formatierten Absatz](images/ch09fg01-small.png "Gerätemodell und Betriebssystem")](images/ch09fg01-large.png#lightbox "Gerätemodell und Betriebssystem")

Der resultierende Code kann jedoch unschöne und schwer lesbar sein.

## <a name="parallel-classes-in-the-shared-asset-project"></a>Parallele Klassen in der freigegebenen Projekts

Ein strukturierter Ansatz für die plattformspezifischen Code ausführen, die von SAP wird veranschaulicht, der [ **PlatInfoSap2** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap2) Beispiel. Jedes der Plattformprojekte enthält eine Klasse mit dem gleichen Namen und die Methoden, aber für diese bestimmte Plattform implementiert. SAP klicken Sie dann einfach die Klasse instanziiert, und ruft die Methode.

## <a name="dependencyservice-and-the-portable-class-library"></a>DependencyService und der portablen Klassenbibliothek

> [!NOTE] 
> Portable Klassenbibliotheken wurden von .NET Standard-Bibliotheken ersetzt. Der Beispielcode aus dem Buch wurde zur Verwendung von .NET standard-Bibliotheken konvertiert.

Eine Bibliothek kann nicht normal Klassen in Anwendungsprojekten zugreifen. Diese Einschränkung ist offenbar zu verhindern, dass die Technik in **PlatInfoSap2** in einer Bibliothek verwendet wird. Xamarin.Forms enthält jedoch eine Klasse namens [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) , verwendet .NET Reflection öffentlichen Zugriff auf Klassen im Anwendungsprojekt aus der Bibliothek.

Definieren Sie die Bibliothek muss ein `interface` mit den Elementen, die in jede Plattform verwendet werden muss. Klicken Sie dann, enthält jede der Plattformen eine Implementierung dieser Schnittstelle. Die Klasse, die die Schnittstelle implementiert muss identifiziert werden, mit einem [DependencyAttribute](xref:Xamarin.Forms.DependencyAttribute) auf Assemblyebene.

Die Bibliothek verwendet dann den generischen [ `Get` ](xref:Xamarin.Forms.DependencyService.Get*) -Methode der `DependencyService` zum Abrufen von einer Instanz der Plattform-Klasse, die die Schnittstelle implementiert.

Dies wird veranschaulicht, der [ **DisplayPlatformInfo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/DisplayPlatformInfo) Beispiel.

## <a name="platform-specific-sound-generation"></a>Clientplattform-spezifische tongenerierung

Die [ **MonkeyTapWithSound** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/MonkeyTapWithSound) Beispiel fügt Signaltöne zu den **MonkeyTap** Programm durch den Zugriff auf Funktionen der Sound-Erstellung in jede Plattform.

## <a name="related-links"></a>Verwandte Links

- [Kapitel 9 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch09-Apr2016.pdf)
- [Kapitel 9-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09)
- [Abhängigkeitsdienst](~/xamarin-forms/app-fundamentals/dependency-service/index.md)
