---
title: Weiterführende Konzepte und Interna
description: Dieses Handbuch enthält weiterführende Konzepte und Interna für Xamarin.Forms. Sie enthält derzeit Artikeln über schnelle Renderer und .NET Standard.
ms.prod: xamarin
ms.assetid: 2273a31c-4022-42ba-befe-0d23ce2ff3b5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: 8ed643619e5a22e9a1febe419eb42d45901dec63
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350744"
---
# <a name="advanced-concepts--internals"></a>Weiterführende Konzepte und Interna

## <a name="fast-renderersfast-renderersmd"></a>[Schnelle Renderer](fast-renderers.md)

Dieser Artikel stellt `Fast Renderer` vor, welche die Inflations- und renderingkosten von Xamarin.Forms-Steuerelementen für Android reduzieren, indem Sie die resultierende native Steuerelementhierarchie vereinfachen.

## <a name="net-standardnet-standardmd"></a>[.NET-Standard](net-standard.md)

In diesem Artikel wird erläutert, wie eine Xamarin.Forms-Anwendung zur Verwendung von .NET Standard 2.0 konvertiert wird.

## <a name="dependency-resolutiondependency-resolutionmd"></a>[Abhängigkeitsauflösung](dependency-resolution.md)

In diesem Artikel wird erläutert, wie eine Abhängigkeit Auflösung-Methode in Xamarin.Forms eingefügt werden, so, dass Dependency Injection-Container einer Anwendung Kontrolle über die Erstellung und Lebensdauer von benutzerdefinierten Renderern, Effekte, hat und `DependencyService` Implementierungen.
