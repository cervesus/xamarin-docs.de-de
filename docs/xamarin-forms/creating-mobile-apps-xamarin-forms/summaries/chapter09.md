---
title: 'title: "Zusammenfassung von Kapitel 9: Plattformspezifische API-Aufrufe" description: "Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung von Kapitel 9.'
description: 'Plattformspezifische API-Aufrufe" ms.prod: xamarin ms.technology: xamarin-forms ms.assetid: 4FFA1BD4-B3ED-461C-9B00-06ABF70D471D author: davidbritch ms.author: dabritch ms.date: 07/19/2018 no-loc: [Xamarin.Forms, Xamarin.Essentials] Zusammenfassung von Kapitel 9.'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 4FFA1BD4-B3ED-461C-9B00-06ABF70D471D
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e8feb636057f1e11c7df90236dee44697203d51c
ms.sourcegitcommit: ea9269b5d9e3d68b61bb428560a10034117ee457
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2020
ms.locfileid: "84136853"
---
# <a name="summary-of-chapter-9-platform-specific-api-calls"></a>Plattformspezifische API-Aufrufe [![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09)

In den Anmerkungen auf dieser Seite wird beschrieben, inwiefern die Angaben innerhalb des Buchs heute nicht mehr für Xamarin.Forms gelten.

> [!NOTE] 
> In einigen Fällen muss Code ausgeführt werden, der abhängig von der Plattform variiert.

In diesem Kapitel werden die Verfahren erläutert. Vorverarbeitung im Projekt mit freigegebenen Anlagen

## <a name="preprocessing-in-the-shared-asset-project"></a>Ein Projekt mit freigegebenen Anlagen in Xamarin.Forms kann mithilfe der C#-Präprozessoranweisungen (`#if`, `#elif` und `endif`) unterschiedlichen Code für die verschiedenen Plattformen ausführen.

Dies wird in [**PlatInfoSap1**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap1) veranschaulicht: [![Screenshot des unterschiedlich formatierten Absatzes](images/ch09fg01-small.png "Gerätemodell und Betriebssystem")](images/ch09fg01-large.png#lightbox "Gerätemodell und Betriebssystem")

Der resultierende Code kann jedoch unschön und schwer lesbar sein.

Parallele Klassen im Projekt mit freigegebenen Anlagen

## <a name="parallel-classes-in-the-shared-asset-project"></a>Ein strukturierteres Verfahren zum Ausführen von plattformspezifischem Code in SAP wird im Beispiel [**PlatInfoSap2**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap2) gezeigt.

Alle Plattformprojekte umfassen eine Klasse und Methoden mit identischem Namen. Die Implementierung erfolgt jedoch plattformspezifisch. In SAP wird die Klasse dann lediglich instanziiert, und die Methode wird aufgerufen. DependencyService und die portable Klassenbibliothek

## <a name="dependencyservice-and-the-portable-class-library"></a>Portable Klassenbibliotheken wurden durch .NET Standard-Bibliotheken ersetzt.

> [!NOTE] 
> Der gesamte Beispielcode innerhalb des Buchs wurde aktualisiert und verwendet jetzt die .NET Standard-Bibliotheken. Eine Bibliothek kann normalerweise nicht auf Klassen in Anwendungsprojekten zugreifen.

Aufgrund dieser Einschränkung scheint es nicht möglich zu sein, das in **PlatInfoSap2** gezeigte Verfahren in einer Bibliothek zu verwenden. Xamarin.Forms enthält aber eine Klasse mit dem Namen [`DependencyService`](xref:Xamarin.Forms.DependencyService), die eine .NET-Reflexion verwendet, um aus der Bibliothek auf öffentliche Klassen im Anwendungsprojekt zuzugreifen. Die Bibliothek muss ein `interface` mit den Membern definieren, die für jede Plattform verwendet werden müssen.

Jede Plattform enthält dann eine Implementierung dieser Schnittstelle. Die Klasse, die die Schnittstelle implementiert, muss mit einem [DependencyAttribute](xref:Xamarin.Forms.DependencyAttribute) auf Assemblyebene gekennzeichnet sein. Die Bibliothek verwendet dann die generische Methode [`Get`](xref:Xamarin.Forms.DependencyService.Get*) von `DependencyService`, um eine Instanz der Plattformklasse abzurufen, die die Schnittstelle implementiert.

Dies wird im Beispiel [**DisplayPlatformInfo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/DisplayPlatformInfo) veranschaulicht.

Plattformspezifische Tonerzeugung

## <a name="platform-specific-sound-generation"></a>Mit dem Beispiel [**MonkeyTapWithSound**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/MonkeyTapWithSound) werden Töne zum **MonkeyTap**-Programm hinzugefügt, indem auf Funktionen zur Tonerzeugung der jeweiligen Plattform zugegriffen wird.

Verwandte Links

## <a name="related-links"></a>[Kapitel 9 – vollständiger Text (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch09-Apr2016.pdf)

- [Kapitel 9 – Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09)
- [Abhängigkeitsdienst](~/xamarin-forms/app-fundamentals/dependency-service/index.md)
- Gerätemodell und Betriebssystem
