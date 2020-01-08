---
title: Was ist Xamarin. Forms?
description: In diesem Artikel wird Xamarinvorgestellt. Formulare und zugehörige Bibliotheken.
ms.prod: xamarin
ms.assetid: C1E24DB9-3099-4F79-BB88-10AABF7D4614
author: profexorgeek
ms.author: jusjohns
ms.date: 09/18/2019
no-loc:
- Xamarin
- Xamarin.Forms
- Xamarin.Android
- Xamarin.Essentials
- Xamarin.iOS
- Xamarin.Mac
ms.openlocfilehash: c217acdc3bd5c007822cc29fc730df99e373ba01
ms.sourcegitcommit: 6f09bc2b760e76a61a854f55d6a87c4f421ac6c8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/02/2020
ms.locfileid: "75607840"
---
# <a name="what-is-opno-locxamarinforms"></a>Was ist Xamarin. Forms?

[![Screenshots des Beispiels [! Schel. No-Loc (xamarin)]. Formular Anwendung in IOS und Android](what-is-xamarin-forms-images/xamarin-forms-app-cropped.png)](what-is-xamarin-forms-images/xamarin-forms-app.png#lightbox)

Xamarin. Forms ist ein Open-Source-Framework für die Benutzeroberfläche. Xamarin. Mit Formularen können Entwickler Android-, IOS-und Windows-Anwendungen aus einer einzelnen freigegebenen CodeBase erstellen.

Xamarin. Mit Formularen können Entwickler Benutzeroberflächen in XAML mit Code Behind in C#erstellen. Diese Schnittstellen werden als leistungsfähige native Steuerelemente auf jeder Plattform gerendert.

## <a name="who-opno-locxamarinforms-is-for"></a>Wer Xamarin. Formulare sind für

Xamarin. Formulare sind für Entwickler mit den folgenden Zielen konzipiert:

- Freigeben von Benutzeroberflächen Layout und-Entwurf
- Teilen Sie Code, Test und Geschäftslogik plattformübergreifend.
- Schreiben Sie plattformübergreifende apps C# in mit Visual Studio.

## <a name="how-opno-locxamarinforms-works"></a>Wie Xamarin. Formulare funktionieren

![[! Schel. No-Loc (xamarin)]. Diagramm der Formular Architektur](what-is-xamarin-forms-images/xamarin-forms-architecture.png)

Xamarin. Forms bietet eine konsistente API zum plattformübergreifenden Erstellen von UI-Elementen. Diese API kann entweder in XAML oder C# implementiert werden und unterstützt Datenbindung für Muster wie Model-View-ViewModel (MVVM).

Zur Laufzeit Xamarin. In Formularen werden Platt Form Renderer verwendet, um die plattformübergreifenden Benutzeroberflächen Elemente in systemeigene Steuerelemente auf Android, IOS und UWP zu konvertieren. Ermöglicht Entwicklern das Native aussehen, das Aussehen und die Leistung, während die Vorteile der plattformübergreifenden Code Freigabe realisiert werden.

Xamarin. Formular Anwendungen bestehen normalerweise aus einer freigegebenen .NET Standard Bibliothek und einzelnen Platt Form Projekten. Die freigegebene Bibliothek enthält die XAML C# -oder-Sichten sowie sämtliche Geschäftslogik, wie z. b. Dienste, Modelle oder anderen Code. Die Platt Form Projekte enthalten plattformspezifische Logik oder Pakete, die für die Anwendung erforderlich sind.

Xamarin. Formulare verwendet Xamarin, um .NET-Anwendungen nativ plattformübergreifend auszuführen. Weitere Informationen zu Xamarinfinden Sie unter [Was ist Xamarin?](~/get-started/what-is-xamarin.md).

## <a name="additional-tools"></a>Zusätzliche Tools

Xamarin. Forms verfügt über ein umfangreiches Ökosystem mit nuget-Paketen, die Anwendungen verschiedene Funktionen hinzufügen. In diesem Abschnitt werden einige häufig verwendete nuget-Pakete beschrieben.

### <a name="opno-locxamarinessentials"></a>Xamarin. Notwendige

Xamarin. Essentials ist eine Bibliothek, die plattformübergreifende APIs für Native Gerätefunktionen bereitstellt. Wie Xamarin selbst Xamarin. Essentials ist eine Abstraktion, die den Prozess des Zugriffs auf native Hilfsprogramme vereinfacht. Einige Beispiele für Hilfsprogramme, die von Xamarinbereitgestellt werden. Zu den Essentials gehören:

- Geräteinformationen
- Dateisystem
- Beschleunigungsmesser
- Telefon Einwählprogramm
- Text-zu-Sprache
- Bildschirmsperre

Weitere Informationen finden Sie unter [Xamarin. Essentials](~/essentials/index.md).

### <a name="shell"></a>Shell

Xamarin. Mit Forms Shell wird die Komplexität der Entwicklung mobiler Anwendungen verringert, indem die grundlegenden Funktionen bereitgestellt werden, die die meisten Anwendungen erfordern. Einige Beispiele für die von der Shell bereitgestellten Features sind:

- Allgemeine Navigationsmöglichkeiten
- URI-basiertes Navigations Schema
- Integrierter Such Handler

Weitere Informationen finden Sie unter [Xamarin. Forms-Shell](~/xamarin-forms/app-fundamentals/shell/index.md)

### <a name="platform-specifics"></a>Plattformspezifische Besonderheiten

Xamarin. Forms bietet eine gemeinsame API, die native Steuerelemente plattformübergreifend rendert, aber eine bestimmte Plattform verfügt möglicherweise über Funktionen, die auf anderen Plattformen nicht vorhanden sind Die Android-Plattform verfügt z. b. über Native Funktionen für einen schnellen Bildlauf in einem `ListView` aber IOS nicht. Xamarin. Mit Formularen Platform-Besonderheiten können Sie Funktionen verwenden, die nur auf einer bestimmten Plattform verfügbar sind, ohne benutzerdefinierte Renderer oder Effekte zu erstellen.

Xamarin. Forms enthält vorgefertigte Lösungen für eine Vielzahl plattformspezifischer Funktionen. Weitere Informationen finden Sie unter: .

- [Xamarin. Formular plattformspezifische Besonderheiten](~/xamarin-forms/platform/platform-specifics/index.md)
- [Android-plattformspezifische Besonderheiten](~/xamarin-forms/platform/android/index.md)
- [IOS-Platt Form Besonderheiten](~/xamarin-forms/platform/ios/index.md)
- [Windows-Platt Form Besonderheiten](~/xamarin-forms/platform/windows/index.md)

### <a name="material-visual"></a>Visuelles Materialobjekt

Xamarin. Visuelles Formular Material dient zum Anwenden von Material Entwurfs Regeln auf Xamarin. Formular Anwendungen. Xamarin. Das visuelle Formular Material verwendet die Visual-Eigenschaft, um benutzerdefinierte Renderer selektiv auf die Benutzeroberfläche anzuwenden. Dies führt zu einer Anwendung mit einem konsistenten Erscheinungsbild für IOS und Android.

Weitere Informationen finden Sie unter [Xamarin. Visuelles Formular Material](~/xamarin-forms/user-interface/visual/material-visual.md)

## <a name="related-links"></a>Verwandte Links

- [Beginnen Sie mit Xamarin. Forms](~/xamarin-forms/index.yml)
- [Xamarin. Notwendige](~/essentials/index.md)
- [Xamarin. Forms-Shell](~/xamarin-forms/app-fundamentals/shell/index.md)
- [Xamarin. Visuelles Formular Material](~/xamarin-forms/user-interface/visual/material-visual.md)
