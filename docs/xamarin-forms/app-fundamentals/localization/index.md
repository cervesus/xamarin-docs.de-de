---
title: Xamarin.Forms-Lokalisierung
description: Integrierte .NET Framework Lokalisierung kann zum Erstellen von plattformübergreifenden mehrsprachiger Anwendungen mit Xamarin.Forms verwendet werden. Text und Bildern lokalisiert werden können, und Anwendungen können eine flussrichtung von rechts-nach-links unterstützen.
ms.prod: xamarin
ms.assetid: 97BF843B-BDAA-4CEA-8189-6DB54B291D7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2018
ms.openlocfilehash: 71033e935a2d3a4be88dbcc5d975938771484640
ms.sourcegitcommit: b60a37587aad8a0bfa8a522d88d22fa672002443
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2018
ms.locfileid: "51285572"
---
# <a name="xamarinforms-localization"></a>Xamarin.Forms-Lokalisierung

_Integrierte .NET Framework Lokalisierung kann zum Erstellen von plattformübergreifenden mehrsprachiger Anwendungen mit Xamarin.Forms verwendet werden._

## <a name="string-and-image-localizationtextmd"></a>[Zeichenfolgen- und Imagelokalisierung](text.md)

Der integrierte Mechanismus zum Lokalisieren von .NET Applications verwendet [RESX-Dateien](https://docs.microsoft.com/dotnet/framework/resources/creating-resource-files-for-desktop-apps#resources-in-resx-files) und die Klassen in der `System.Resources` und `System.Globalization` Namespaces. Die RESX-Dateien, die mit der übersetzten Zeichenfolgen werden in der Xamarin.Forms-Assembly zusammen mit einer vom Compiler generierte Klasse eingebettet, die stark typisierten Zugriff auf die Übersetzungen bereitstellt. Der übersetzte Text können Sie dann im Code abgerufen werden.

## <a name="right-to-left-localizationright-to-leftmd"></a>[Lokalisierung von rechts nach links](right-to-left.md)

Flussrichtung ist die Richtung, in der die Elemente der Benutzeroberfläche auf der Seite vom Auge gescannt werden. Rechts-nach-links-Lokalisierung bietet Unterstützung für flussrichtung von rechts-nach-Links zu Xamarin.Forms-Anwendungen.
