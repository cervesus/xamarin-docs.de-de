---
title: Portieren von Java nach C# für Xamarin.Android
description: Eine dritte Möglichkeit zur Verwendung von Java in einer Xamarin.Android-Anwendung besteht darin, den Java-Quellcode nach C# zu portieren.
ms.prod: xamarin
ms.assetid: 39E528BD-010F-47FC-BE48-8E7848E30454
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/05/2016
ms.openlocfilehash: 8f96fcc4aadcd8f082d55dc568b2517f048edaf2
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027196"
---
# <a name="porting-java-to-c-for-xamarinandroid"></a>Portieren von Java nach C# für Xamarin.Android

Diese Vorgehensweise ist möglicherweise für folgende Organisationen von Interesse:

- **Organisationen, die einen Technologiewechsel von Java nach C# vornehmen.**
- **Organisationen, die eine C#- und eine Java-Version desselben Produkts unterhalten müssen.**
- **Organisationen, die über eine .NET-Version einer gängigen Java-Bibliothek verfügen möchten.**

Es gibt zwei Möglichkeiten, um Java-Code nach C# zu portieren. Die erste Möglichkeit besteht darin, den Code manuell zu portieren. Dafür sind erfahrene Entwickler notwendig, die sowohl .NET als auch Java kennen und mit den richtigen Idiomen für die jeweilige Sprache vertraut sind. Diese Vorgehensweise ist für geringe Codeumfänge oder für Organisationen, die vollständig von Java zu C# wechseln möchten, am sinnvollsten.

Die zweite Methode für die Portierung besteht darin, den Prozess mithilfe eines Codekonverters wie [Sharpen](https://github.com/mono/sharpen) zu automatisieren. [Sharpen](https://github.com/mono/sharpen) ist ein Open-Source-Konverter von Versant, der ursprünglich verwendet wurde, um den Code für *db4o* von Java nach C# zu portieren. Bei db4o handelt es sich um eine objektorientierte Datenbank, die in Java entwickelt und dann nach .NET portiert wurde. Die Verwendung eines Codekonverters kann für Projekte sinnvoll sein, die in beiden Sprachen vorhanden sein müssen und eine gewisse Gleichheit zwischen beiden erfordern.

Ein Beispiel für den Fall, dass ein automatisiertes Codekonvertierungstool Sinn macht, finden Sie im [ngit](https://github.com/mono/ngit)-Projekt.
Bei ngit handelt es sich um eine Portierung des Java-Projekts [jgit](https://eclipse.org/).
Jgit selbst ist eine Java-Implementierung des [Git](https://git-scm.com/)-Quellcodeverwaltungssystems. Zum Generieren C# von Code aus Java verwenden die ngit-Programmierer ein angepasstes automatisiertes System, um den Java-Code aus jgit zu extrahieren, einige Patches für die Durchführung des Konvertierungsprozesses anzuwenden und anschließend Sharpen auszuführen, wodurch der C#-Code generiert wird. Dadurch kann das ngit-Projekt von der kontinuierlichen Weiterentwicklung von jgit profitieren.

Das Bootstrapping eines automatisierten Codekonvertierungstools ist oftmals mit einem erheblichen Arbeitsaufwand verbunden, was eine unüberwindliche Schranke darstellen kann. In vielen Fällen ist es möglicherweise einfacher und leichter, Java manuell nach C# zu portieren.

## <a name="related-links"></a>Verwandte Links

- [Sharpen-Konvertierungstool](https://github.com/mono/sharpen)
