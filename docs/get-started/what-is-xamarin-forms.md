---
title: Was ist Xamarin.Forms?
description: In diesem Artikel werden Xamarin.Forms sowie damit verbundene Bibliotheken eingeführt.
ms.prod: xamarin
ms.assetid: C1E24DB9-3099-4F79-BB88-10AABF7D4614
author: profexorgeek
ms.author: jusjohns
ms.date: 09/18/2019
ms.openlocfilehash: aaceb6089a5b7e5f0551dafe9ef1fe50d01433d9
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "77374022"
---
# <a name="what-is-xamarinforms"></a>Was ist Xamarin.Forms?

[![Screenshots einer Xamarin.Forms-Beispielanwendung unter iOS und Android](what-is-xamarin-forms-images/xamarin-forms-app-cropped.png)](what-is-xamarin-forms-images/xamarin-forms-app.png#lightbox)

Bei Xamarin.Forms handelt es sich um ein Open-Source-Benutzeroberflächenframework. Mithilfe von Xamarin.Forms können Entwickler iOS-, Android- und Windows-Anwendungen aus einer einzigen freigegebenen CodeBase erstellen.

Mithilfe von Xamarin.Forms können Entwickler Benutzeroberflächen in XAML mit CodeBehind in C# erstellen. Diese Benutzeroberflächen werden als leistungsfähige native Steuerelemente für jede Plattform gerendert.

## <a name="who-xamarinforms-is-for"></a>Zielgruppe von Xamarin.Forms

Xamarin.Forms eignet sich für Entwickler mit den folgenden Zielen:

- Plattformübergreifendes Freigeben von Benutzeroberflächenlayout und -design
- Plattformübergreifendes Freigeben von Code, Test- und Geschäftslogik
- Schreiben von plattformübergreifenden Apps in C# mit Visual Studio

## <a name="how-xamarinforms-works"></a>Funktionsweise von Xamarin.Forms

![Architekturdiagramm für Xamarin.Forms](what-is-xamarin-forms-images/xamarin-forms-architecture.png)

Über Xamarin.Forms steht eine konsistente API zur Verfügung, um plattformübergreifende Benutzeroberflächenelemente zu erstellen. Diese API kann in XAML oder in C# implementiert werden und unterstützt Datenbindung für Muster wie Model View ViewModel (MVVM).

Zur Laufzeit verwendet Xamarin.Forms Plattformrenderer, um die plattformübergreifenden Benutzeroberflächenelemente zu nativen Steuerelementen für Android, iOS und UWP zu konvertieren. So können Entwickler natives Aussehen, native Servicequalität und native Leistung erreichen und dabei von den Vorteilen der plattformübergreifenden Codefreigabe profitieren.

Xamarin.Forms-Anwendungen bestehen normalerweise aus einer freigegebenen .NET Standard-Bibliothek und einzelnen Plattformprojekten. Die freigegebene Bibliothek enthält die XAML- und C#-Ansichten und sämtliche Geschäftslogik, wie Dienste, Modelle oder anderen Code. In den Plattformprojekten ist alle plattformspezifische Logik oder sind alle Pakete enthalten, die für die Anwendung erforderlich sind.

Xamarin.Forms verwendet Xamarin, um .NET-Anwendungen plattformübergreifend nativ auszuführen. Weitere Informationen zu Xamarin finden Sie unter [Was ist Xamarin?](~/get-started/what-is-xamarin.md).

## <a name="additional-tools"></a>Weitere Tools

Xamarin.Forms verfügt über ein großes Ökosystem an NuGet-Paketen, die in Anwendungen verschiedene Funktionen hinzufügen. In diesem Abschnitt werden einige häufig verwendeten NuGet-Pakete näher beschrieben.

### <a name="xamarinessentials"></a>Xamarin.Essentials

Bei Xamarin.Essentials handelt es sich um eine Bibliothek, die plattformübergreifende APIs für native Gerätefeatures bietet. Wie Xamarin selbst ist Xamarin.Essentials eine Abstraktion, die den Zugriffsprozess auf native Hilfsprogramme vereinfacht. Zu den von Xamarin.Essentials zur Verfügung gestellten Hilfsprogrammen gehören die folgenden:

- Geräteinformationen
- Dateisystem
- Beschleunigungsmesser
- Wählhilfe
- Text-zu-Sprache
- Bildschirmsperre

Weitere Informationen finden Sie unter [Xamarin.Essentials](~/essentials/index.md).

### <a name="shell"></a>Shell

Die Xamarin.Forms-Shell reduziert die Komplexität der Entwicklung mobiler Anwendungen, indem sie die grundlegenden Features bereitstellt, die für die meisten mobilen Anwendungen erforderlich sind. Zu den von der Shell zur Verfügung gestellten Features gehören die folgenden:

- Allgemeine Navigationsoberfläche
- URI-basiertes Navigationsschema
- Ein integrierter Suchhandler

Weitere Informationen finden Sie unter [Xamarin.Forms-Shell](~/xamarin-forms/app-fundamentals/shell/index.md).

### <a name="platform-specifics"></a>Plattformeigenschaften

Xamarin.Forms stellt eine allgemeine API zur Verfügung, die native Steuerelemente plattformübergreifend rendert. Eine bestimmte Plattform verfügt aber möglicherweise über Funktionen, die auf anderen Plattformen nicht zur Verfügung stehen. Die Android-Plattform bietet beispielsweise native Funktionen für schnelles Scrollen in der Klasse `ListView`. Unter iOS ist dies jedoch nicht möglich. Dank der für Xamarin.Forms plattformspezifischen Eigenschaften können Sie Funktionen verwenden, die nur auf einer bestimmten Plattform zur Verfügung stehen, ohne benutzerdefinierte Renderer oder Effekte erstellen zu müssen.

Xamarin.Forms beinhaltet vorgefertigte Lösungen für die unterschiedlichsten plattformspezifischen Funktionen. Weitere Informationen finden Sie unter:

- [Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md)
- [Android-Plattformeigenschaften](~/xamarin-forms/platform/android/index.md)
- [iOS-Plattformfeatures in Xamarin.Forms](~/xamarin-forms/platform/ios/index.md)
- [Windows-Plattformeigenschaften](~/xamarin-forms/platform/windows/index.md)

### <a name="material-visual"></a>Visuelles Materialobjekt

Das visuelle Material-Objekt für Xamarin.Forms wird verwendet, um Material Design-Regeln auf Xamarin.Forms-Anwendungen anzuwenden. Das visuelle Material.Objekt für Xamarin.Forms verwendet die Eigenschaft des visuellen Objekts, um selektiv benutzerdefinierte Renderer auf die Benutzeroberfläche anzuwenden. Das führt dann zu einer Anwendung mit konsistentem Aussehen und Servicequalität für iOS und Android.

Weitere Informationen finden Sie unter [Visuelles Material-Objekt für Xamarin.Forms](~/xamarin-forms/user-interface/visual/material-visual.md).

## <a name="related-links"></a>Verwandte Links

- [Xamarin.Forms-Dokumentation](~/xamarin-forms/index.yml)
- [Xamarin.Essentials](~/essentials/index.md)
- [Xamarin.Forms-Shell](~/xamarin-forms/app-fundamentals/shell/index.md)
- [Visuelles Material-Objekt für Xamarin.Forms](~/xamarin-forms/user-interface/visual/material-visual.md)
