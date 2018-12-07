---
title: Xamarin.Forms-Lokalisierung
description: Das integrierte .NET-Lokalisierungsframework kann zum Erstellen plattformübergreifender mehrsprachiger Anwendungen mit Xamarin.Forms verwendet werden. Text und Bilder können lokalisiert werden, und Anwendungen können die Leserichtung von rechts nach links unterstützen.
ms.prod: xamarin
ms.assetid: 97BF843B-BDAA-4CEA-8189-6DB54B291D7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2018
ms.openlocfilehash: 71033e935a2d3a4be88dbcc5d975938771484640
ms.sourcegitcommit: b60a37587aad8a0bfa8a522d88d22fa672002443
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2018
ms.locfileid: "51285572"
---
# <a name="xamarinforms-localization"></a>Xamarin.Forms-Lokalisierung

_Das integrierte .NET-Lokalisierungsframework kann zum Erstellen plattformübergreifender mehrsprachiger Anwendungen mit Xamarin.Forms verwendet werden._

## <a name="string-and-image-localizationtextmd"></a>[Zeichenfolgen- und Imagelokalisierung](text.md)

Der integrierte Mechanismus zum Lokalisieren von .NET-Anwendungen verwendet [RESX-Dateien](https://docs.microsoft.com/dotnet/framework/resources/creating-resource-files-for-desktop-apps#resources-in-resx-files) und die Klassen in den Namespaces `System.Resources` und `System.Globalization`. Die RESX-Dateien mit übersetzten Zeichenfolgen werden in die Xamarin.Forms-Assembly gemeinsam mit einer vom Compiler generierten Klasse eingebettet. Diese Klasse ermöglicht stark typisierten Zugriff auf die Übersetzung. Der übersetzte Code kann dann im Code abgerufen werden.

## <a name="right-to-left-localizationright-to-leftmd"></a>[Lokalisierung von rechts nach links](right-to-left.md)

Die Leserichtung ist die Richtung, in der Benutzeroberflächenelemente auf der Seite vom Auge wahrgenommen werden. Die Lokalisierung von rechts nach links fügt Unterstützung für die Leserichtung von rechts nach links in Xamarin.Forms-Anwendungen hinzu.
