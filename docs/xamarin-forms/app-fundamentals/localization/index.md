---
title: Xamarin.Forms-Lokalisierung
description: Das integrierte .NET-Lokalisierungsframework kann zum Erstellen plattformübergreifender mehrsprachiger Anwendungen mit Xamarin.Forms verwendet werden. Text und Bilder können lokalisiert werden, und Anwendungen können die Leserichtung von rechts nach links unterstützen.
ms.prod: xamarin
ms.assetid: 97BF843B-BDAA-4CEA-8189-6DB54B291D7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2018
ms.openlocfilehash: b580c6e41aa689ff8fcea698c40d7aaf5f2ca050
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "74135259"
---
# <a name="xamarinforms-localization"></a>Xamarin.Forms-Lokalisierung

_Das integrierte .NET-Lokalisierungsframework kann zum Erstellen plattformübergreifender mehrsprachiger Anwendungen mit Xamarin.Forms verwendet werden._

## <a name="xamarinforms-string-and-image-localization"></a>[Zeichenfolgen- und Imagelokalisierung für Xamarin.Forms](text.md)

Der integrierte Mechanismus zum Lokalisieren von .NET-Anwendungen verwendet [RESX-Dateien](https://docs.microsoft.com/dotnet/framework/resources/creating-resource-files-for-desktop-apps#resources-in-resx-files) und die Klassen in den Namespaces `System.Resources` und `System.Globalization`. Die RESX-Dateien mit übersetzten Zeichenfolgen werden in die Xamarin.Forms-Assembly gemeinsam mit einer vom Compiler generierten Klasse eingebettet. Diese Klasse ermöglicht stark typisierten Zugriff auf die Übersetzung. Der übersetzte Text kann dann im Code abgerufen werden.

## <a name="right-to-left-localization"></a>[Lokalisierung von rechts nach links](right-to-left.md)

Die Leserichtung ist die Richtung, in der Benutzeroberflächenelemente auf einer Seite vom Auge wahrgenommen werden. Die Lokalisierung von rechts nach links fügt Unterstützung für die Leserichtung von rechts nach links in Xamarin.Forms-Anwendungen hinzu.
