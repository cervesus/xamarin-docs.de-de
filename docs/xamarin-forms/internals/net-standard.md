---
title: .NET 2.0 Standardsupport in Xamarin.Forms
description: In diesem Artikel wird erläutert, wie eine Xamarin.Forms-Anwendung für die Verwendung von Standard 2.0 von .NET konvertiert wird.
ms.prod: xamarin
ms.assetid: 95805355-63a7-44e7-a3c6-6487a6276ab2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: 8685f1e10b5094e6f58e8efea51e6dd216dfa000
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="net-standard-20-support-in-xamarinforms"></a>.NET 2.0 Standardsupport in Xamarin.Forms

_In diesem Artikel wird erläutert, wie eine Xamarin.Forms-Anwendung für die Verwendung von Standard 2.0 von .NET konvertiert wird._

.NET standard ist eine Spezifikation von .NET APIs, die auf alle Implementierungen von .NET verfügbar sein sollen. Dies erleichtert das Freigeben von Code in desktop-Anwendungen, mobile apps und Spiele und cloud-Dienste, durch das Kombinieren von identische APIs auf verschiedenen Plattformen. Informationen zu den von .NET Standard unterstützten Plattformen finden Sie unter [.NET implementierungsunterstützung](/dotnet/standard/net-standard#net-implementation-support/).

.NET Standardbibliotheken sind der Ersatz für Portable Klasse Bibliotheken (PCL). Eine Bibliothek, die auf .NET Standard abzielt ist immer noch eine PCL und wird als eine standardmäßige .NET basierende PCL bezeichnet. .NET Standardversionen bestimmte PCL-Profilen zugeordnet sind, und für Profile, die Zuordnung auf, die zwei Bibliothekstypen werden aufeinander verweisen können. Weitere Informationen finden Sie unter [PCL Kompatibilität](/dotnet/standard/net-standard#pcl-compatibility).

Xamarin.Forms 2.4 können Xamarin.Forms Anwendungen Ziel .NET Standard 2.0 durch Ersetzen der PCL mit einer .NET Standard 2.0-Bibliothek. Dies kann wie folgt erreicht werden:

- Stellen Sie sicher [.NET Core 2.0](https://www.microsoft.com/net/download/core) installiert ist.
- Aktualisieren Sie das Xamarin.Forms-Projektmappe, um Xamarin.Forms 2.4 oder höher verwenden.
- Fügen Sie der Projektmappe, die auf .NET Standard 2.0 .NET Standardbibliothek hinzu.
- Löschen Sie die Klasse, die die .NET Standardbibliothek hinzugefügt wird.
- Fügen Sie der Standardbibliothek .NET Xamarin.Forms 2.4 (oder höher) NuGet-Paket hinzu.
- Fügen Sie in den plattformprojekten einen Verweis auf die .NET Standardbibliothek, und entfernen Sie den Verweis zum PCL-Projekt, das die Xamarin.Forms-Benutzeroberflächenlogik enthält.
- Kopieren Sie die Dateien aus dem PCL-Projekt der Standardbibliothek .NET.
- Entfernen Sie das PCL-Projekt, das xamarin.Forms Benutzeroberflächenlogik enthält.


## <a name="related-links"></a>Verwandte Links

- [.NET-Standard](~/cross-platform/app-fundamentals/net-standard.md)
- [Optionen für die Codefreigabe](~/cross-platform/app-fundamentals/code-sharing.md)
