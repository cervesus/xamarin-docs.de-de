---
title: Was ist Xamarin.Forms?
description: In diesem Artikel werden Xamarin.Forms und die dazugehörigen Bibliotheken eingeführt.
ms.prod: xamarin
ms.assetid: C1E24DB9-3099-4F79-BB88-10AABF7D4614
author: profexorgeek
ms.author: jusjohns
ms.date: 05/28/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: eb76da9be7fcb227c465c0a046b967b2f70b1cfb
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84198309"
---
# <a name="what-is-xamarinforms"></a>Was ist Xamarin.Forms?

[![Screenshots: Xamarin.Forms-Beispielanwendung unter iOS und Android](what-is-xamarin-forms-images/xamarin-forms-app-cropped.png)](what-is-xamarin-forms-images/xamarin-forms-app.png#lightbox)

Bei Xamarin.Forms handelt es sich um ein Open-Source-Benutzeroberflächenframework. Mithilfe von Xamarin.Forms können Entwickler Xamarin.iOS-, Xamarin.Android- und Windows-Anwendungen aus einer einzigen freigegebenen CodeBase erstellen.

Xamarin.Forms ermöglicht Entwicklern die Erstellung von Benutzeroberflächen in XAML mit CodeBehind in C#. Diese Benutzeroberflächen werden als leistungsfähige native Steuerelemente für jede Plattform gerendert.

## <a name="who-xamarinforms-is-for"></a>Zielgruppe von Xamarin.Forms

Xamarin.Forms eignet sich für Entwickler mit den folgenden Zielen:

- Plattformübergreifendes Freigeben von Benutzeroberflächenlayout und -design
- Plattformübergreifendes Freigeben von Code, Test- und Geschäftslogik
- Schreiben von plattformübergreifenden Apps in C# mit Visual Studio

## <a name="how-xamarinforms-works"></a>Funktionsweise von Xamarin.Forms

![Xamarin.Forms-Architekturdiagramm](what-is-xamarin-forms-images/xamarin-forms-architecture.png)

Über Xamarin.Forms steht eine konsistente API zur Verfügung, mit der sich plattformübergreifend Benutzeroberflächenelemente erstellen lassen. Diese API kann in XAML oder in C# implementiert werden und unterstützt Datenbindung für Muster wie Model View ViewModel (MVVM).

Zur Laufzeit verwendet Xamarin.Forms Plattformrenderer, um die plattformübergreifenden Benutzeroberflächenelemente in native Steuerelemente für Xamarin.Android, Xamarin.iOS und UWP zu konvertieren. So können Entwickler natives Aussehen, native Servicequalität und native Leistung erreichen und dabei von den Vorteilen der plattformübergreifenden Codefreigabe profitieren.

Xamarin.Forms-Anwendungen bestehen normalerweise aus einer freigegebenen .NET Standard-Bibliothek und einzelnen Plattformprojekten. Die freigegebene Bibliothek enthält die XAML- und C#-Ansichten und sämtliche Geschäftslogik, wie Dienste, Modelle oder anderen Code. In den Plattformprojekten ist alle plattformspezifische Logik oder sind alle Pakete enthalten, die für die Anwendung erforderlich sind.

Xamarin.Forms verwendet die Xamarin-Plattform, um .NET-Anwendungen plattformübergreifend nativ auszuführen. Weitere Informationen zur Xamarin-Plattform finden Sie unter [Was ist Xamarin?](~/get-started/what-is-xamarin.md).

## <a name="additional-functionality"></a>Zusätzliche Funktionen

Xamarin.Forms verfügt über ein großes Ökosystem von Bibliotheken, die Anwendungen verschiedene Funktionen hinzufügen. In diesem Abschnitt werden einige dieser zusätzlichen Funktionen beschrieben.

### Xamarin.Essentials

Bei Xamarin.Essentials handelt es sich um eine Bibliothek, die plattformübergreifende APIs für native Gerätefeatures bietet. Wie Xamarin selbst ist Xamarin.Essentials eine Abstraktion, die den Zugriff auf native Hilfsprogramme vereinfacht. Zu den von Xamarin.Essentials zur Verfügung gestellten Hilfsprogrammen gehören die folgenden:

- Geräteinformationen
- Dateisystem
- Beschleunigungsmesser
- Wählhilfe
- Text-zu-Sprache
- Bildschirmsperre

Weitere Informationen finden Sie unter [Xamarin.Essentials](~/essentials/index.md).

### <a name="shell"></a>Shell

Die Xamarin.Forms-Shell reduziert die Komplexität der Entwicklung mobiler Anwendungen, indem die grundlegenden Features bereitgestellt werden, die von den meisten Anwendungen benötigt werden. Zu den von der Shell zur Verfügung gestellten Features gehören die folgenden:

- Allgemeine Navigationsoberfläche
- URI-basiertes Navigationsschema
- Ein integrierter Suchhandler

Weitere Informationen finden Sie unter [Xamarin.Forms-Shell](~/xamarin-forms/app-fundamentals/shell/index.md).

### <a name="platform-specifics"></a>Plattformeigenschaften

Xamarin.Forms stellt eine allgemeine API zur Verfügung, die native Steuerelemente plattformübergreifend rendert. Bestimmte Plattformen können jedoch über Funktionen verfügen, die andere Plattformen nicht bieten. Die Android-Plattform bietet beispielsweise native Funktionen für schnelles Scrollen in der Klasse `ListView`. Unter iOS ist dies jedoch nicht möglich. Dank der für Xamarin.Forms plattformspezifischen Eigenschaften können Sie Funktionen verwenden, die nur auf einer bestimmten Plattform zur Verfügung stehen, ohne benutzerdefinierte Renderer oder Effekte erstellen zu müssen.

Xamarin.Forms beinhaltet vorgefertigte Lösungen für die unterschiedlichsten plattformspezifischen Funktionen. Weitere Informationen finden Sie unter:

- [Plattformspezifische Eigenschaften von Xamarin.Forms](~/xamarin-forms/platform/platform-specifics/index.md)
- [Android-Plattformeigenschaften](~/xamarin-forms/platform/android/index.md)
- [iOS-Plattformfeatures in Xamarin.Forms](~/xamarin-forms/platform/ios/index.md)
- [Windows-Plattformeigenschaften](~/xamarin-forms/platform/windows/index.md)

### <a name="material-visual"></a>Visuelles Materialobjekt

Xamarin.Forms Material Visual wird verwendet, um Material Design-Regeln auf Xamarin.Forms-Anwendungen anzuwenden. Xamarin.Forms Material Visual verwendet die Visual-Eigenschaft, um selektiv benutzerdefinierte Renderer auf die Benutzeroberfläche anzuwenden. Das Ergebnis ist ein konsistentes Erscheinungsbild der Anwendung unter iOS und Android.

Weitere Informationen finden Sie unter [Xamarin.Forms Material Visual](~/xamarin-forms/user-interface/visual/material-visual.md).

## <a name="related-links"></a>Verwandte Links

- [Erste Schritte mit Xamarin.Forms](~/xamarin-forms/index.yml)
- [Xamarin.Essentials](~/essentials/index.md)
- [Xamarin.Forms-Shell](~/xamarin-forms/app-fundamentals/shell/index.md)
- [Xamarin.Forms Material Visual](~/xamarin-forms/user-interface/visual/material-visual.md)
