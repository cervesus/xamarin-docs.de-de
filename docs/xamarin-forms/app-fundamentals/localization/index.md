---
title: Xamarin.Forms Lokalisierung
description: Integrierte Lokalisierung .NET Framework kann verwendet werden, um plattformübergreifende mehrsprachige Clientanwendungen mit Xamarin.Forms zu erstellen.
ms.prod: xamarin
ms.assetid: 97BF843B-BDAA-4CEA-8189-6DB54B291D7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/13/2018
ms.openlocfilehash: 29624eeccc8542b3296774f6b77480b664bee478
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="localization"></a>Lokalisierung

_Integrierte Lokalisierung .NET Framework kann verwendet werden, um plattformübergreifende mehrsprachige Clientanwendungen mit Xamarin.Forms zu erstellen._

## <a name="string-and-image-localizationtextmd"></a>[Zeichenfolgen- und Image-Lokalisierung](text.md)

Die integrierten Mechanismen zum Lokalisieren von .NET Applications verwendet [RESX-Dateien](http://msdn.microsoft.com/library/ekyft91f(v=vs.90).aspx) und die Klassen in der `System.Resources` und `System.Globalization` Namespaces. Die RESX-Dateien, die mit übersetzten Zeichenfolgen werden in die Xamarin.Forms-Assembly zusammen mit einer vom Compiler generierte Klasse eingebettet, die stark typisierten Zugriff auf die Übersetzungen bereitstellt. Der übersetzte Text können Sie dann im Code abgerufen werden.

## <a name="right-to-left-localizationright-to-leftmd"></a>[Lokalisierung von rechts nach links](right-to-left.md)

Flussrichtung ist die Richtung, in der die Elemente der Benutzeroberfläche auf der Seite vom Auge gescannt werden. Lokalisierung von rechts nach links bietet Unterstützung für rechts-nach-links-flussrichtung Xamarin.Forms-Anwendungen.
