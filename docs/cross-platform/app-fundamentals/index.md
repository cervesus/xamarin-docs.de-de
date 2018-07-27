---
title: Freigeben von Code auf mehreren Plattformen
description: Dieses Dokument enthält Links zu verschiedenen Leitfäden, die Techniken für die Freigabe von Code, einschließlich portable Klassenbibliotheken, gemeinsam genutzte Projekte, .NET Standard und NuGet beschreiben.
ms.prod: xamarin
ms.assetid: 7D179ACF-09A6-46EE-B49D-E27AB5F09CD4
author: conceptdev
ms.author: crdun
ms.date: 07/18/2018
ms.openlocfilehash: 3a2c3f98e3ba83db0794a68ff1d62a9845a111c0
ms.sourcegitcommit: 46bb04016d3c35d91ff434b38474e0cb8197961b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2018
ms.locfileid: "39270188"
---
# <a name="sharing-code-on-multiple-platforms"></a>Freigeben von Code auf mehreren Plattformen

In diesen Artikeln wird erläutert, die verschiedenen verfügbaren Optionen zum Freigeben von Code für Plattformen, einschließlich Windows, Android, iOS und vieles mehr.

## <a name="code-sharing-overviewcode-sharingmd"></a>[Übersicht über die Nutzung von Code](code-sharing.md)

Informationen Sie zu den anderen Code, der Freigabe-Optionen für die Xamarin-Projekte, einschließlich .NET Standard-Bibliotheken und freigegebene Projekte verfügbar. Portable Klassenbibliotheken werden ebenfalls unterstützt, aber sie gelten .NET Standard ersetzt.

## <a name="net-standardcross-platformapp-fundamentalsnet-standardmd"></a>[.NET-Standard](~/cross-platform/app-fundamentals/net-standard.md)

.NET standard ist die bevorzugte Option für die Freigabe von Code. Code wird erstellt, mit einer bestimmten Version (2.0 bietet die beste API-Kompatibilität mit vorhandenem .NET Framework-Code) und kann von anderen Projekten, die dieser Ebene unterstützen verwendeten oder höher sein. .NET standard-Projekte werden in Visual Studio 2017 und Visual Studio für Mac unterstützt.

## <a name="shared-projectscross-platformapp-fundamentalsshared-projectsmd"></a>[Freigegebene Projekte](~/cross-platform/app-fundamentals/shared-projects.md)

Freigegebene Projekte können Sie die gemeinsamen Code zu schreiben, der durch eine Reihe von verschiedenen Anwendungsprojekte verwiesen wird. Der Code wird als Teil jeder verweisende Projekt kompiliert und zählen Compiler-Direktiven können plattformspezifische Funktionalität in die gemeinsame Codebasis zu integrieren. Dieser Artikel beschreibt die Funktionsweise von Projekten mit freigegebenen und das Erstellen und diese mit Xamarin-Projekte verwenden.

## <a name="portable-class-librariescross-platformapp-fundamentalspclmd"></a>[Portable Klassenbibliotheken](~/cross-platform/app-fundamentals/pcl.md)

Portable Class Library-Projekten können Sie die erstellen und Verteilen von Assemblys, die gemeinsam verwendeten Code zur Ausführung auf mehreren Plattformen enthalten. Zum Erstellen eines Portable Class Library (oder "PCL") Wählen Sie zunächst die Plattformen als Ziel aus, und Schreiben von Code für eine untergeordnete Gruppe von .NET Framework, die im Profil definierten für diese Plattformen verfügbar ist. PCLs gelten als in den neuesten Versionen von Visual Studio veraltet sein. Entwicklern wird empfohlen, .NET Standard 2.0 verwenden.

## <a name="nuget-projects-multiplatform-libraries-for-code-sharingcross-platformapp-fundamentalsnuget-multiplatform-librariesindexmd"></a>[NuGet-Projekte: plattformübergreifende Bibliotheken für die Codefreigabe von](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)

NuGet-Pakete können von PCL oder .NET standard-Projekte automatisch generiert werden; und freigegebene Projekte kann in "lockvogel"-NuGet-Pakete, die mit dem separaten NuGet-Projekttyp gepackt werden. In diesem Abschnitt wird erläutert, wie zum Erstellen von NuGet-Paketen für jedes Szenario Freigeben von Code wird.

## <a name="manually-creating-nuget-packages-for-xamarincross-platformapp-fundamentalsnuget-manualmd"></a>[Manuelles Erstellen von NuGet-Paketen für Xamarin](~/cross-platform/app-fundamentals/nuget-manual.md)

Tipps zum Erstellen von NuGet-Pakete, die mit der Xamarin-Plattform arbeiten.
