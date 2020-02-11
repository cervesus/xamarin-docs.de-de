---
title: Einführung in die Xamarin.Forms-Shell
description: Die Xamarin.Forms-Shell stellt die wichtigsten Features bereit, die von den meisten Anwendungen benötigt werden, beispielsweise eine gemeinsame Benutzeroberfläche für die Navigation, ein URI-basiertes Navigationsschema und einen integrierten Suchhandler.
ms.prod: xamarin
ms.assetid: 4604DCB5-83DA-458A-8B02-6508A740BE0E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2019
ms.openlocfilehash: cb2ae3afe9db86d4db603d499ef0e75e7cbbf552
ms.sourcegitcommit: 52fb214c0e0243587d4e9ad9306b75e92a8cc8b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2020
ms.locfileid: "76940405"
---
# <a name="xamarinforms-shell-introduction"></a>Einführung in die Xamarin.Forms-Shell

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

Die Xamarin.Forms-Shell reduziert die Komplexität der Entwicklung mobiler Anwendungen, indem es die grundlegenden Features bereitstellt, die die meisten mobilen Anwendungen benötigen, einschließlich:

- Einer zentralen Stelle zur Beschreibung der visuellen Hierarchie einer Anwendung.
- Eine gemeinsame Benutzeroberfläche für die Navigation.
- Ein URI-basiertes Navigationsschema, das die Navigation zu jeder Seite in der Anwendung ermöglicht.
- Ein integrierter Suchhandler.

Darüber hinaus profitieren Shell-Anwendungen von einer höheren Renderinggeschwindigkeit und einer geringeren Arbeitsspeichernutzung.

> [!IMPORTANT]
> Bestehende Anwendungen können die Shell übernehmen und sofort von Verbesserungen in den Bereichen Navigation, Leistung und Erweiterbarkeit profitieren.

## <a name="platform-support"></a>Plattformunterstützung

Xamarin.Forms-Shell ist unter iOS und Android vollständig verfügbar, auf der universellen Windows-Plattform (UWP) jedoch nur teilweise verfügbar. Dort ist die Shell derzeit nur als experimentelle Funktion vorhanden. Wenn Sie die Shell auf der UWP verwenden möchten, fügen Sie vor dem Aufruf von `Forms.Init` die folgende Codezeile in der `App`-Klasse Ihres UWP-Projekts hinzu:

```csharp
global::Xamarin.Forms.Forms.SetFlags("Shell_UWP_Experimental");
```

Weitere Informationen zum Hinzufügen eines UWP-Projekts zu einer Xamarin.Forms-Lösung finden Sie unter [Einrichten von Windows-Projekten](~/xamarin-forms/platform/windows/installation/index.md).

## <a name="shell-navigation-experience"></a>Shell-Navigationsoberfläche

Die Shell bietet eine Benutzeroberfläche mit zahlreichen Optionen für die Navigation, basierend auf Flyouts und Registerkarten. Die oberste Ebene der Navigation in einer Shell-Anwendung ist entweder ein Flyout oder eine untere Registerkartenleiste, je nach den Navigationsanforderungen der Anwendung. Das folgende Beispiel zeigt eine Anwendung, in der die oberste Ebene der Navigation ein Flyout ist:

[![Screenshot eines Shell-Flyouts unter iOS und Android](introduction-images/flyout.png "Shell-Flyout")](introduction-images/flyout-large.png#lightbox "Shell-Flyout")

Die Auswahl eines Flyoutelements führt zu der unteren Registerkarte, die das ausgewählte Element darstellt und anzeigt:

[![Screenshot der unteren Registerkarten der Shell unter iOS und Android](introduction-images/monkeys.png "Untere Registerkarten der Shell")](introduction-images/monkeys-large.png#lightbox "Untere Registerkarten der Shell")

> [!NOTE]
> Wenn der Flyout nicht geöffnet ist, kann die untere Registerkartenleiste als oberste Ebene der Navigation in der Anwendung betrachtet werden.

Jede Registerkarte zeigt eine [`ContentPage`](xref:Xamarin.Forms.ContentPage) an. Wenn eine untere Registerkarte mehr als eine Seite enthält, sind die Seiten jedoch über die obere Registerkartenleiste navigierbar:

[![Screenshot der oberen Registerkarten der Shell unter iOS und Android](introduction-images/cats.png "Obere Registerkarten der Shell")](introduction-images/cats-large.png#lightbox "Obere Registerkarten der Shell")

Innerhalb jeder Registerkarte kann zu zusätzlichen [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekten navigiert werden:

[![Screenshot der Shell-Seitennavigation unter iOS und Android](introduction-images/cat-details.png "Shell-App-Navigation")](introduction-images/cat-details-large.png#lightbox "Shell-App-Navigation")

## <a name="related-links"></a>Verwandte Links

- [Xaminals (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
