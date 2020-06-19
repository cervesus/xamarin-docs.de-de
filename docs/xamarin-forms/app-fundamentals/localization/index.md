---
title: Xamarin.Forms-Lokalisierung
description: Das integrierte .NET-Lokalisierungsframework kann zum Erstellen plattformübergreifender mehrsprachiger Anwendungen mit Xamarin.Forms verwendet werden. Text und Bilder können lokalisiert werden, und Anwendungen können die Leserichtung von rechts nach links unterstützen.
ms.prod: xamarin
ms.assetid: 97BF843B-BDAA-4CEA-8189-6DB54B291D7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: dddb80e8fb683547d2bf6ba0b74e870fe240695c
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84137565"
---
# <a name="xamarinforms-localization"></a>Xamarin.Forms-Lokalisierung

_Das integrierte .NET-Lokalisierungsframework kann zum Erstellen plattformübergreifender mehrsprachiger Anwendungen mit Xamarin.Forms verwendet werden._

## <a name="xamarinforms-string-and-image-localizationtextmd"></a>[Zeichenfolgen- und Bildlokalisierung für Xamarin.Forms](text.md)

Der integrierte Mechanismus zum Lokalisieren von .NET-Anwendungen verwendet [RESX-Dateien](https://docs.microsoft.com/dotnet/framework/resources/creating-resource-files-for-desktop-apps#resources-in-resx-files) und die Klassen in den Namespaces `System.Resources` und `System.Globalization`. Die RESX-Dateien mit übersetzten Zeichenfolgen werden gemeinsam mit einer vom Compiler generierten Klasse in die Xamarin.Forms-Assembly eingebettet. Diese Klasse ermöglicht stark typisierten Zugriff auf die Übersetzungen. Der übersetzte Text kann dann im Code abgerufen werden.

## <a name="right-to-left-localization"></a>[Lokalisierung von rechts nach links](right-to-left.md)

Die Leserichtung ist die Richtung, in der Benutzeroberflächenelemente auf einer Seite vom Auge wahrgenommen werden. Durch die Lokalisierung von rechts nach links kann Text in Xamarin.Forms-Anwendungen in der Leserichtung von rechts nach links dargestellt werden.
