---
title: Xamarin.Forms-Shell
description: Dieser Leitfaden erklärt, wie die Xamarin.Forms-Shell verwendet wird, und wie sich die Komplexität von Xamarin.Forms-Anwendungen reduzieren lässt, indem sie die grundlegenden Funktionen bereitstellt, die die meisten Anwendungen benötigen.
ms.prod: xamarin
ms.assetid: 85B322AA-808F-41B6-953A-5877264AE643
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/28/2019
ms.openlocfilehash: a988a99e20af76d071f55c4cd2c97b135ad077f8
ms.sourcegitcommit: 10b4ccbfcf182be940899c00fc0fecae1e199c5b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2019
ms.locfileid: "66252276"
---
# <a name="xamarinforms-shell"></a>Xamarin.Forms-Shell

## <a name="introductionintroductionmd"></a>[Introduction (Einführung)](introduction.md)

Die Xamarin.Forms-Shell reduziert die Komplexität der Entwicklung mobiler Anwendungen, indem es die grundlegenden Features bereitstellt, die die meisten mobilen Anwendungen benötigen. Dazu gehören eine gemeinsame Navigationsbenutzerumgebung, ein URI-basiertes Navigationsschema und ein integrierter Suchhandler.

## <a name="create-a-xamarinforms-shell-applicationcreatemd"></a>[Erstellt eine Xamarin.Forms-Shell-Anwendung](create.md)

Das Vorgehen zum Erstellen einer Xamarin.Forms-Shell-Anwendung besteht darin, eine XAML-Datei zu erstellen, die die `Shell`-Klasse untergliedert, die `MainPage`-Eigenschaft der `App`-Klasse der Anwendung auf das über Unterklassen verfügende `Shell`-Objekt festzulegen und dann die visuelle Hierarchie der Anwendung in der über Unterklassen verfügenden `Shell`-Klasse zu beschreiben.

## <a name="flyoutflyoutmd"></a>[Flyout](flyout.md)

Das Flyout ist das Hauptmenü für eine Shell-Anwendung, und der Zugriff erfolgt über ein Symbol oder durch Wischen von einer Seite des Bildschirms. Das Flyout besteht aus einem optionalen Header, Flyout-Elementen und optionalen Menüelementen.

## <a name="tabstabsmd"></a>[Registerkarten](tabs.md)

Nach einem Flyout ist die nächste Navigationsebene in einer Shell-Anwendung die untere Registerkartenleiste. Alternativ kann das Navigationsmuster für eine Anwendung mit unteren Registerkarten beginnen und auf die Verwendung eines Flyouts verzichten. Wenn eine untere Registerkarte mehrere Seiten enthält, sind die Seiten in beiden Fällen über Register am oberen Rand navigierbar.

## <a name="page-configurationconfigurationmd"></a>[Seitenkonfiguration](configuration.md)

Die `Shell`-Klasse definiert angefügte Eigenschaften, die verwendet werden können, um die Darstellung von Seiten in Xamarin.Forms-Shell-Anwendungen zu konfigurieren. Dies schließt das Festlegen von Seitenfarben, das Deaktivieren der Navigationsleiste, das Deaktivieren der Registerkartenleiste und das Anzeigen von Ansichten in der Navigationsleiste ein.

## <a name="navigationnavigationmd"></a>[Navigation](navigation.md)

Shell-Anwendungen können ein URI-basiertes Navigationsschema nutzen, das Routen verwendet, um zu einer beliebigen Seite in der Anwendung zu navigieren, ohne einer festen Navigationshierarchie folgen zu müssen.

## <a name="searchsearchmd"></a>[Suchen](search.md)

Shell-Anwendungen können integrierte Suchfunktionalität über ein Suchfeld nutzen, das oben auf jeder Seite hinzugefügt werden kann.

## <a name="custom-rendererscustomrenderersmd"></a>[Benutzerdefinierte Renderer](customrenderers.md)

Shell-Anwendungen sind über die von den verschiedenen Shell-Klassen bereitgestellten Eigenschaften und Methoden äußerst anpassbar. Allerdings ist es auch möglich, einen benutzerdefinierten Shell-Renderer zu erstellen, wenn komplexere plattformspezifische Anpassungen erforderlich sind.
