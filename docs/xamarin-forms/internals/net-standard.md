---
title: .NET Standard 2,0-Unterstützung in xamarin. Forms
description: In diesem Artikel wird erläutert, wie Sie eine xamarin. Forms-Anwendung für die Verwendung von .NET Standard 2,0 konvertieren. .NET Standard ist eine Spezifikation von .NET APIs, die auf allen Implementierungen von .NET verfügbar sein sollen.
ms.prod: xamarin
ms.assetid: 95805355-63a7-44e7-a3c6-6487a6276ab2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: eaef607e3e6095ccc7dfb1f5831d2384d11cab5f
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70760061"
---
# <a name="net-standard-20-support-in-xamarinforms"></a>.NET Standard 2,0-Unterstützung in xamarin. Forms

_In diesem Artikel wird erläutert, wie eine Xamarin.Forms-Anwendung konvertiert werden kann, um .NET Standard 2.0 zu verwenden._

.NET Standard ist eine Spezifikation von .NET APIs, die auf allen Implementierungen von .NET verfügbar sein sollen. Dies erleichtert das Teilen von Code zwischen Desktopanwendungen, mobilen Apps und Spielen sowie Clouddiensten, indem identische APIs auf verschiedenen Plattformen verwendet werden. Informationen zu den von .NET Standard unterstützten Plattformen finden Sie unter [Unterstützung der .NET-Implementierung](/dotnet/standard/net-standard#net-implementation-support).

.NET Standard-Bibliotheken sind der Ersatz für portable Klassenbibliotheken (PCL). Eine Bibliothek, die .NET Standard anzielt, ist jedoch immer noch eine PCL und wird als eine auf .NET Standard basierende PCL bezeichnet. Bestimmte PCL-Profile sind einer .NET Standard-Version zugeordnet. Bei diesen Profilen können die beiden Bibliothekstypen aufeinander verweisen. Weitere Informationen finden Sie unter [PCL Kompatibilität](/dotnet/standard/net-standard#pcl-compatibility).

Ab der Version Xamarin.Forms 2.4 können Xamarin.Forms-Anwendungen .NET Standard 2.0 verwenden, indem sie die portable Klassenbibliothek mit einer .NET Standard 2.0-Bibliothek ersetzen. Dies kann wie folgt erreicht werden:

- Stellen Sie sicher, dass [.NET Core 2.0](https://www.microsoft.com/net/download/core) installiert ist.
- Aktualisieren Sie die Xamarin.Forms-Projektmappe, um Xamarin.Forms 2.4 oder höher zu verwenden.
- Fügen Sie der Projektmappe für .NET Standard 2.0 eine .NET Standard-Bibliothek hinzu.
- Löschen Sie die Klasse, die der .NET Standard-Bibliothek hinzugefügt wurde.
- Fügen Sie der .NET Standard-Bibliothek das NuGet-Paket für Xamarin.Forms 2.4 oder höher hinzu.
- Fügen Sie in den Plattformprojekten einen Verweis auf die .NET Standard-Bibliothek hinzu, und entfernen Sie den Verweis auf das PCL-Projekt, das die Logik für die Xamarin.Forms-Benutzeroberfläche enthält.
- Kopieren Sie die Dateien aus dem PCL-Projekt in die .NET Standard-Bibliothek.
- Entfernen Sie das PCL-Projekt, das die Logik der Xamarin.Forms-Benutzeroberfläche enthält.

## <a name="related-links"></a>Verwandte Links

- [.NET-Standard](~/cross-platform/app-fundamentals/net-standard.md)
- [Optionen für die Codefreigabe](~/cross-platform/app-fundamentals/code-sharing.md)
