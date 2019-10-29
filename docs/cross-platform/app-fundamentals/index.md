---
title: Freigeben von Code auf mehreren Plattformen
description: Dieses Dokument enthält Links zu verschiedenen Leitfäden, in denen Techniken für die Freigabe von Code beschrieben werden, einschließlich portabler Klassenbibliotheken, frei gegebener Projekte, .NET Standard und nuget.
ms.prod: xamarin
ms.assetid: 7D179ACF-09A6-46EE-B49D-E27AB5F09CD4
author: davidortinau
ms.author: daortin
ms.date: 07/18/2018
ms.openlocfilehash: a91fba3cd1fba3bcf2317e8f9cb25631c62491cb
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016830"
---
# <a name="sharing-code-on-multiple-platforms"></a>Freigeben von Code auf mehreren Plattformen

In diesen Artikeln werden die verschiedenen Optionen erläutert, die für die plattformübergreifende Freigabe von Code verfügbar sind, einschließlich Windows, Android, IOS und mehr.

## <a name="code-sharing-overviewcode-sharingmd"></a>[Übersicht über die Code Freigabe](code-sharing.md)

Erfahren Sie mehr über die verschiedenen Optionen für die Code Freigabe, die für xamarin-Projekte verfügbar sind, einschließlich .NET Standard Bibliotheken und frei gegebener Projekte Portable Klassenbibliotheken werden ebenfalls unterstützt, werden jedoch als veraltet betrachtet, um .NET Standard zu bevorzugen.

## <a name="net-standardcross-platformapp-fundamentalsnet-standardmd"></a>[.NET-Standard](~/cross-platform/app-fundamentals/net-standard.md)

.NET Standard ist die bevorzugte Option für die plattformübergreifende Freigabe von Code. Der Code wird für eine bestimmte Version erstellt (2,0 bietet die beste API-Kompatibilität mit vorhandenem .NET Framework Code) und kann dann von anderen Projekten genutzt werden, die diese Ebene oder höher unterstützen. .NET Standard Projekte werden sowohl in Visual Studio 2019 als auch in Visual Studio 2019 für Mac unterstützt.

## <a name="shared-projectscross-platformapp-fundamentalsshared-projectsmd"></a>[Freigegebene Projekte](~/cross-platform/app-fundamentals/shared-projects.md)

Mit freigegebenen Projekten können Sie allgemeinen Code schreiben, auf den von einer Reihe verschiedener Anwendungsprojekte verwiesen wird. Der Code wird als Teil jedes verweisenden Projekts kompiliert und kann Compilerdirektiven einschließen, um die plattformspezifische Funktionalität in die freigegebene Codebasis zu integrieren. In diesem Artikel wird erläutert, wie freigegebene Projekte funktionieren und wie Sie mit xamarin-Projekten erstellt und verwendet werden.

## <a name="portable-class-librariescross-platformapp-fundamentalspclmd"></a>[Portable Klassenbibliotheken](~/cross-platform/app-fundamentals/pcl.md)

Projekte für Portable Klassenbibliotheken ermöglichen das Erstellen und Verteilen von Assemblys, die freigegebenen Code enthalten, die auf mehreren Plattformen ausgeführt werden Um eine portable Klassenbibliothek (oder "PCL") zu erstellen, wählen Sie zuerst die Zielplattformen aus, und schreiben Sie dann Code für eine Teilmenge der .NET Framework, die im für diese Plattformen definierten Profil verfügbar ist. Pcls werden in den neuesten Versionen von Visual Studio als veraltet eingestuft. Entwicklern wird empfohlen, stattdessen .NET Standard 2,0 zu verwenden.

## <a name="nuget-projects-multiplatform-libraries-for-code-sharingcross-platformapp-fundamentalsnuget-multiplatform-librariesindexmd"></a>[Nuget-Projekte: Multiplatform-Bibliotheken für die Code Freigabe](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)

Nuget-Pakete können automatisch aus PCL-oder .NET Standard-Projekten generiert werden. und freigegebene Projekte können mit dem separaten nuget-Projekttyp in "Köder und Switch"-nuget-Pakete verpackt werden. In diesem Abschnitt wird erläutert, wie Sie nuget-Pakete für jedes Code Freigabe Szenario erstellen.

## <a name="manually-creating-nuget-packages-for-xamarincross-platformapp-fundamentalsnuget-manualmd"></a>[Manuelles Erstellen von nuget-Paketen für xamarin](~/cross-platform/app-fundamentals/nuget-manual.md)

Tipps zum Erstellen von nuget-Paketen, die mit der xamarin-Plattform funktionieren.

## <a name="use-cc-libraries-in-cross-platform-xamarin-projectscross-platformcppindexmd"></a>[Verwenden von CC++ /Bibliotheken in plattformübergreifenden xamarin-Projekten](~/cross-platform/cpp/index.md)

Mit dieser Technik können Sie die Entwicklung ihrer C/C++ Bibliotheken, eine C# Bindung in einem nuget-und ihre xamarin-Anwendungen entkoppeln. Die Funktionalität wird von der nativen Plattform C/C++ Library bereitgestellt, aber der gesamte plattformspezifische Code ist von der endgültigen xamarin-Anwendung (en) isoliert und ermöglicht so eine höchstmögliche Leistung ohne Code Duplizierung. 
