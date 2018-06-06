---
title: Freigeben von Code auf mehreren Plattformen
description: Dieses enthält Dokumentenlinks zu verschiedenen Handbüchern, die Techniken zum Freigeben von Code, einschließlich portable Klassenbibliotheken, gemeinsam genutzte Projekte, .NET Standard und NuGet-beschreiben.
ms.prod: xamarin
ms.assetid: 7D179ACF-09A6-46EE-B49D-E27AB5F09CD4
author: asb3993
ms.author: amburns
ms.date: 02/18/2018
ms.openlocfilehash: 61377afa61e2c2006c2fdf8ef9b21fe7d567b3de
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780066"
---
# <a name="sharing-code-on-multiple-platforms"></a>Freigeben von Code auf mehreren Plattformen

Dieser Abschnitt enthält eine Anleitung für einige der häufiger Aufgaben Dinge oder Konzepte, denen Entwickler beim Entwickeln von mobilen Anwendungen berücksichtigen müssen.

## <a name="code-sharing-overviewcode-sharingmd"></a>[Übersicht über das Freigeben Code](code-sharing.md)

Informationen Sie zu den verschiedenen Code Freigabeoptionen für Xamarin-Projekte, einschließlich portablen Klassenbibliotheken (PCLs), freigegebene Projekte und Standardbibliotheken .NET verfügbar.


##  <a name="portable-class-librariescross-platformapp-fundamentalspclmd"></a>[Portable Klassenbibliotheken](~/cross-platform/app-fundamentals/pcl.md)

Portable Class Library-Projekte können beim Erstellen und Verteilen von Assemblys, die freigegebenen Code zur Ausführung auf mehreren Plattformen enthalten. Zum Erstellen eines Portable Class Library (oder "PCL") Wählen Sie zunächst die Plattformen abzielen, Sie schreiben Code für eine Teilmenge von .NET Framework, die im Profil definierten für diese Plattformen verfügbar ist. Dieses Dokument beschreibt das Erstellen und Verwenden von PCLs mit Xamarin.

##  <a name="shared-projectscross-platformapp-fundamentalsshared-projectsmd"></a>[Freigegebene Projekte](~/cross-platform/app-fundamentals/shared-projects.md)

Gemeinsam genutzte Projekte können Sie die gemeinsamen Code schreiben, der durch eine Reihe von verschiedenen Anwendungsprojekte verwiesen wird. Der Code wird als Teil jeder verweisenden Projekts kompiliert und kann Compilerdirektiven können Sie die Übernahme von Clientplattform-spezifische Funktionen in der gemeinsamen Codebasis enthalten. In diesem Artikel wird erläutert, wie freigegebene Projekte funktionieren und wie Sie erstellen und mit Xamarin-Projekten zu verwenden.

##  <a name="net-standardcross-platformapp-fundamentalsnet-standardmd"></a>[.NET-Standard](~/cross-platform/app-fundamentals/net-standard.md)

.NET standard ist eine neue Option für die Freigabe von Code. Funktioniert in ähnlicher Weise wie portablen Klassenbibliotheken; Code wird für eine bestimmte Version (derzeit 1.0 bis 1.6) erstellt und kann von anderen Projekten, die dieser Ebene unterstützen verbrauchten oder höher sein. .NET standard Projekte werden in Xamarin Studio 6.2, Visual Studio für Windows und Visual Studio für Mac unterstützt.

##  <a name="nuget-projects-multiplatform-libraries-for-code-sharingcross-platformapp-fundamentalsnuget-multiplatform-librariesindexmd"></a>[NuGet-Projekte: Multiplattform-Bibliotheken für die Freigabe von Code](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)

NuGet-Pakete können von PCL oder .NET Standardprojekten automatisch generiert werden; und freigegebene Projekte können in "Köder And Switch" NuGet-Pakete, die mit separaten NuGet-Projekttyp verpackt werden. In diesem Abschnitt erläutert das NuGet-Pakete für jedes Szenario Freigeben von Code zu erstellen.

##  <a name="manually-creating-nuget-packages-for-xamarincross-platformapp-fundamentalsnuget-manualmd"></a>[Manuelles Erstellen von NuGet-Pakete für Xamarin](~/cross-platform/app-fundamentals/nuget-manual.md)

Tipps zum Erstellen von NuGet-Pakete, die mit der Xamarin-Plattform arbeiten.
