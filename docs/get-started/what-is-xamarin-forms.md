---
title: Was ist Xamarin.Forms?
description: In diesem Artikel werden xamarin. Forms und zugehörige Bibliotheken vorgestellt.
ms.prod: xamarin
ms.assetid: C1E24DB9-3099-4F79-BB88-10AABF7D4614
author: profexorgeek
ms.author: jusjohns
ms.date: 09/18/2019
ms.openlocfilehash: 8bd530e743330ab8058f13cb7ba09f95a30ee886
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "71256606"
---
# <a name="what-is-xamarinforms"></a>Was ist Xamarin.Forms?

[![Screenshots der xamarin. Forms-Beispielanwendung in IOS und Android](what-is-xamarin-forms-images/xamarin-forms-app-cropped.png)](what-is-xamarin-forms-images/xamarin-forms-app.png#lightbox)

Xamarin. Forms ist ein Open-Source-Framework für die Benutzeroberfläche. Xamarin. Forms ermöglicht Entwicklern das Erstellen von Android-, IOS-und Windows-Anwendungen aus einer einzelnen freigegebenen CodeBase.

Xamarin. Forms ermöglicht Entwicklern das Erstellen von Benutzeroberflächen in XAML mit Code Behind in C#. Diese Schnittstellen werden als leistungsfähige native Steuerelemente auf jeder Plattform gerendert.

## <a name="who-xamarinforms-is-for"></a>Wer xamarin. Forms ist für

Xamarin. Forms ist für Entwickler mit den folgenden Zielen konzipiert:

- Freigeben von Benutzeroberflächen Layout und-Entwurf
- Teilen Sie Code, Test und Geschäftslogik plattformübergreifend.
- Schreiben Sie plattformübergreifende apps C# in mit Visual Studio.

## <a name="how-xamarinforms-works"></a>Funktionsweise von xamarin. Forms

![Xamarin. Forms-Architektur Diagramm](what-is-xamarin-forms-images/xamarin-forms-architecture.png)

Xamarin. Forms bietet eine konsistente API zum plattformübergreifenden Erstellen von UI-Elementen. Diese API kann entweder in XAML oder C# implementiert werden und unterstützt Datenbindung für Muster wie Model-View-ViewModel (MVVM).

Zur Laufzeit verwendet xamarin. Forms Platt Form Renderer, um die plattformübergreifenden Benutzeroberflächen Elemente in systemeigene Steuerelemente auf Android, IOS und UWP zu konvertieren. Ermöglicht Entwicklern das Native aussehen, das Aussehen und die Leistung, während die Vorteile der plattformübergreifenden Code Freigabe realisiert werden.

Xamarin. Forms-Anwendungen bestehen normalerweise aus einer freigegebenen .NET Standard Bibliothek und einzelnen Platt Form Projekten. Die freigegebene Bibliothek enthält die XAML C# -oder-Sichten sowie sämtliche Geschäftslogik, wie z. b. Dienste, Modelle oder anderen Code. Die Platt Form Projekte enthalten plattformspezifische Logik oder Pakete, die für die Anwendung erforderlich sind.

Xamarin. Forms verwendet xamarin, um .NET-Anwendungen nativ plattformübergreifend auszuführen. Weitere Informationen zu xamarin finden Sie unter [Was ist xamarin?](~/get-started/what-is-xamarin.md).

## <a name="additional-tools"></a>Weitere Tools

Xamarin. Forms verfügt über ein umfangreiches Ökosystem mit nuget-Paketen, die Anwendungen verschiedene Funktionen hinzufügen. In diesem Abschnitt werden einige häufig verwendete nuget-Pakete beschrieben.

### <a name="xamarinessentials"></a>Xamarin.Essentials

Xamarin. Essentials ist eine Bibliothek, die plattformübergreifende APIs für Native Gerätefunktionen bereitstellt. Wie xamarin selbst ist xamarin. Essentials eine Abstraktion, die den Prozess des Zugriffs auf native Hilfsprogramme vereinfacht. Einige Beispiele für Hilfsprogramme, die von xamarin. Essentials bereitgestellt werden, sind:

- Geräteinformationen
- Dateisystem
- Beschleunigungsmesser
- Telefon Einwählprogramm
- Text-zu-Sprache
- Bildschirmsperre

Weitere Informationen finden Sie unter [xamarin. Essentials](~/essentials/index.md).

### <a name="shell"></a>Shell

Xamarin. Forms Shell reduziert die Komplexität der Entwicklung mobiler Anwendungen durch die Bereitstellung der grundlegenden Features, die für die meisten Anwendungen erforderlich sind. Einige Beispiele für die von der Shell bereitgestellten Features sind:

- Allgemeine Navigationsmöglichkeiten
- URI-basiertes Navigations Schema
- Integrierter Such Handler

Weitere Informationen finden Sie unter [xamarin. Forms Shell.](~/xamarin-forms/app-fundamentals/shell/index.md)

### <a name="platform-specifics"></a>Plattformspezifische Besonderheiten

Xamarin. Forms stellt eine gemeinsame API bereit, mit der native Steuerelemente plattformübergreifend gerendert werden, aber eine bestimmte Plattform verfügt möglicherweise über Funktionen, die auf anderen Plattformen Die Android-Plattform verfügt z. b. über native Funktionalität für schnelles `ListView` Scrollen in, aber IOS nicht. Xamarin. Forms Platform-Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne benutzerdefinierte Renderer oder Effekte zu erstellen.

Xamarin. Forms enthält vorgefertigte Lösungen für eine Vielzahl plattformspezifischer Funktionen. Weitere Informationen finden Sie unter:

- [Xamarin. Forms-plattformspezifische Besonderheiten](~/xamarin-forms/platform/platform-specifics/index.md)
- [Android-plattformspezifische Besonderheiten](~/xamarin-forms/platform/android/index.md)
- [IOS-Platt Form Besonderheiten](~/xamarin-forms/platform/ios/index.md)
- [Windows-Platt Form Besonderheiten](~/xamarin-forms/platform/windows/index.md)

### <a name="material-visual"></a>Visuelles Materialobjekt

Xamarin. Forms Material Visual dient zum Anwenden von Material Entwurfs Regeln auf xamarin. Forms-Anwendungen. Das visuelle xamarin. Forms-Material verwendet die Visual-Eigenschaft, um benutzerdefinierte Renderer selektiv auf die Benutzeroberfläche anzuwenden. Dies führt zu einer Anwendung mit einem konsistenten Erscheinungsbild über IOS und Android.

Weitere Informationen finden Sie unter [xamarin. Forms Material Visual.](~/xamarin-forms/user-interface/visual/material-visual.md)

## <a name="related-links"></a>Verwandte Links

- [Beginnen Sie mit xamarin. Forms](~/xamarin-forms/index.yml)
- [Xamarin.Essentials](~/essentials/index.md)
- [Xamarin.Forms-Shell](~/xamarin-forms/app-fundamentals/shell/index.md)
- [Visuelles xamarin. Forms-Material](~/xamarin-forms/user-interface/visual/material-visual.md)
