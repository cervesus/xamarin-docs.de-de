---
title: Was ist Xamarin?
description: In diesem Artikel werden Xamarin und verwandte Bibliotheken vorgestellt.
ms.prod: xamarin
ms.assetid: 33C83E13-F3E5-17B4-6512-207F3D3C5AB6
ms.custom: video
author: profexorgeek
ms.author: jusjohns
ms.date: 09/16/2019
no-loc:
- Xamarin
- Xamarin.Forms
- Xamarin.Android
- Xamarin.Essentials
- Xamarin.iOS
- Xamarin.Mac
ms.openlocfilehash: d4abb59cac117314239c669df454a786a3720ff5
ms.sourcegitcommit: 6f09bc2b760e76a61a854f55d6a87c4f421ac6c8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/02/2020
ms.locfileid: "75607879"
---
# <a name="what-is-opno-locxamarin"></a>Was ist Xamarin?

[![Screenshots des Beispiels [! Schel. No-Loc (xamarin)]-Anwendung in IOS und Android](what-is-xamarin-images/xamarin-app-cropped.png)](what-is-xamarin-images/xamarin-app.png#lightbox)

Xamarin ist eine Open-Source-Plattform zum Entwickeln moderner und leistungsfähiger Anwendungen für IOS, Android und Windows mit .net. Xamarin ist eine Abstraktionsschicht, die die Kommunikation zwischen frei gegebenem Code und zugrunde liegenden Platt Form Code verwaltet. Xamarin wird in einer verwalteten Umgebung ausgeführt, die Komfort wie Speicher Belegung und Garbage Collection bereitstellt.

Xamarin ermöglicht es Entwicklern, einen durchschnittlichen Anteil von 90% ihrer Anwendung plattformübergreifend zu nutzen. Dieses Muster ermöglicht es Entwicklern, Ihre gesamte Geschäftslogik in einer einzelnen Sprache zu schreiben (oder vorhandenen Anwendungscode wiederzuverwenden), aber die systemeigene Leistung, das Aussehen und Verhalten der einzelnen Plattformen zu erreichen.

Xamarin Anwendungen können auf einem PC oder Mac geschrieben und in Native Anwendungspakete (z **. b. eine APK** -Datei unter Android) oder in einer **IPA** -Datei unter IOS kompiliert werden.

> [!NOTE]
> Das Kompilieren und Bereitstellen von Anwendungen für IOS erfordert derzeit einen MacOS-Computer. Weitere Informationen zu den Entwicklungsanforderungen finden Sie unter [System Requirements (Systemanforderungen](~/cross-platform/get-started/requirements.md#macos-requirements)).

## <a name="who-opno-locxamarin-is-for"></a>Wer Xamarin für

Xamarin ist für Entwickler mit den folgenden Zielen konzipiert:

- Teilen Sie Code, Test und Geschäftslogik plattformübergreifend.
- Schreiben Sie plattformübergreifende Anwendungen C# in mit Visual Studio.

## <a name="how-opno-locxamarin-works"></a>Funktionsweise von Xamarin

![Diagramm von [! Schel. Architektur von NO-LOC (xamarin)]](what-is-xamarin-images/xamarin-architecture.png)

Das Diagramm zeigt die Gesamtarchitektur einer plattformübergreifenden Xamarin Anwendung. mit Xamarin können Sie auf jeder Plattform eine native Benutzeroberfläche erstellen und eine Geschäfts C# Logik in schreiben, die plattformübergreifend verwendet wird. In den meisten Fällen kann 80% des Anwendungs Codes mithilfe von Xamarinfreigegeben werden.

Xamarin baut auf **Mono**auf, einer Open-Source-Version des .NET Framework basierend auf den .net ECMA-Standards. Mono ist für fast so lange vorhanden wie die .NET Framework selbst und wird auf den meisten Plattformen ausgeführt, einschließlich Linux, UNIX, FreeBSD und macOS. Die Mono-Ausführungsumgebung übernimmt automatisch Aufgaben, wie z. b. Speicher Belegung, Garbage Collection und Interoperabilität mit zugrunde liegenden Plattformen.

Weitere Informationen zur plattformspezifischen Architektur finden Sie unter [Xamarin. Android](#xamarinandroid) und [Xamarin. IOS](#xamarinios).

### <a name="added-features"></a>Hinzugefügte Features

Xamarin kombiniert die Möglichkeiten nativer Plattformen und fügt eine Reihe von Features hinzu, einschließlich:

1. **Vollständige Bindung für die zugrunde liegenden sdgs** – Xamarin enthält Bindungen für fast alle zugrunde liegenden Platform sdgs in IOS und Android. Zusätzlich dazu sind diese Bindungen stark typisiert. So bieten sie einfach Navigation, lassen sich leicht nutzen und sorgen während der Entwicklung für eine stabile Typüberprüfung zur Kompilierzeit und während der Entwicklung. Stark typisierte Bindungen führen zu weniger Laufzeitfehlern und Anwendungen höherer Qualität.
1. **Ziel-c, Java, C und C++ Interop** – Xamarin stellt Funktionen zum direkten Aufrufen von Ziel-c, Java, c und C++ Bibliotheken bereit und ermöglicht Ihnen die Verwendung eines breiten Arrays von Code von Drittanbietern. Mit dieser Funktion können Sie vorhandene IOS-und Android-Bibliotheken verwenden, die in Ziel-C, JavaC++oder C/geschrieben sind. Außerdem bietet Xamarin Bindungs Projekte, die es Ihnen ermöglichen, Native Ziel-C-und Java-Bibliotheken mithilfe einer deklarativen Syntax zu binden.
1. **Moderne Sprachkonstrukte** – Xamarin Anwendungen werden in C#geschrieben, eine moderne Sprache, die bedeutende Verbesserungen gegenüber Ziel-C und Java umfasst, wie z. b. Funktionen dynamischer Sprachen, funktionale Konstrukte wie Lambdas, LINQ, parallele Programmierung, Generika usw.
1. **Robuste Basisklassen Bibliothek (Base Class Library, BCL)** – Xamarin-Anwendungen verwenden die .net-BCL, eine große Auflistung von Klassen, die über umfassende und optimierte Features wie leistungsstarke XML-, Datenbank-, Serialisierungs-, IO-, Zeichen folgen-und Netzwerkunterstützung verfügen. Vorhandener C# Code kann für die Verwendung in einer APP kompiliert werden, die den Zugriff auf Tausende von Bibliotheken ermöglicht, die über die BCL hinaus Funktionalität hinzufügen.
1. **Moderne integrierte Entwicklungsumgebung (Integrated Development Environment, IDE)** – Xamarin verwendet Visual Studio, eine moderne IDE, die Features wie die automatische Vervollständigung von Code, ein ausgereiftes Projekt-und Lösungs Verwaltungssystem, eine umfassende Projektvorlagen Bibliothek, eine integrierte Quell Code Verwaltung und vieles mehr umfasst.
1. **Mobile plattformübergreifende Unterstützung** – Xamarin bietet ausgereifte plattformübergreifende Unterstützung für die drei Hauptplattformen von IOS, Android und Windows. Anwendungen können so geschrieben werden, dass Sie bis zu 90% Ihres Codes freigeben und Xamarin. Essentials bietet eine einheitliche API für den Zugriff auf gemeinsame Ressourcen auf allen drei Plattformen. Mit frei gegebenem Code können Entwicklungskosten und die Markteinführungszeit für Mobile Entwickler erheblich reduziert werden.

### <a name="opno-locxamarinandroid"></a>Xamarin. Erhältlich

[![[! Schel. No-Loc (xamarin)]. Android-Architektur Diagramm](what-is-xamarin-images/android-architecture-cropped.png)](what-is-xamarin-images/android-architecture.png#lightbox)

Xamarin. Android-Anwendungen werden C# von in die **Intermediate Language (IL)** kompiliert. diese wird dann **Just-in-time (JIT)** in eine systemeigene Assembly kompiliert, wenn die Anwendung gestartet wird. Xamarin. Android-Anwendungen werden in der Mono-Ausführungsumgebung parallel zum virtuellen Computer der Android-Laufzeit (Art) ausgeführt. Xamarin bietet .Net-Bindungen an die Namespaces "Android. *" und "java. *". Die Mono-Ausführungsumgebung ruft diese Namespaces über die **verwalteten Callable Wrapper (MCW)** auf und stellt **Android Callable Wrapper (ACW)** für die Kunst bereit, sodass beide Umgebungen Code aufeinander aufrufen können.

Weitere Informationen finden Sie unter [Xamarin. Android-Architektur](~/android/internals/architecture.md).

### <a name="opno-locxamarinios"></a>Xamarin. IOS

[![[! Schel. No-Loc (xamarin)]. IOS-Architektur Diagramm](what-is-xamarin-images/ios-architecture-cropped.png)](what-is-xamarin-images/ios-architecture.png#lightbox)

Xamarin. IOS-Anwendungen sind vollständig **im voraus kompilierte Zeit (AOT)** , die aus C# in nativem Arm-Assemblycode kompiliert wurden. Xamarin verwendet **Selectors** , um Ziel-c für verwaltete C# und **Registrare** verfügbar zu machen C# , um verwalteten Code für "Ziel-c" verfügbar zu machen. Selektoren und Registrare werden zusammen als "Bindungen" bezeichnet und ermöglichen die Kommunikation mit C# dem Ziel-C und der Kommunikation.

Weitere Informationen finden Sie unter [Xamarin. IOS-Architektur](~/ios/internals/architecture.md).

### <a name="opno-locxamarinessentials"></a>Xamarin. Notwendige

Xamarin. Essentials ist eine Bibliothek, die plattformübergreifende APIs für Native Gerätefunktionen bereitstellt. Wie Xamarin selbst Xamarin. Essentials ist eine Abstraktion, die den Prozess des Zugriffs auf systemeigene Funktionen vereinfacht. Einige Beispiele für Funktionen, die von Xamarinbereitgestellt werden. Zu den Essentials gehören:

- Geräteinformationen
- Dateisystem
- Beschleunigungsmesser
- Telefon Einwählprogramm
- Text-zu-Sprache
- Bildschirmsperre

Weitere Informationen finden Sie unter [Xamarin. Essentials](~/essentials/index.md).

### <a name="opno-locxamarinforms"></a>Xamarin. Forms

Xamarin. Forms ist ein Open-Source-Framework für die Benutzeroberfläche. Xamarin. Formulare ermöglicht Entwicklern das Erstellen von IOS-, Android-und Windows-Anwendungen aus einer einzelnen freigegebenen CodeBase. Xamarin. Mit Formularen können Entwickler Benutzeroberflächen in XAML mit Code Behind in C#erstellen. Diese Benutzeroberflächen werden als leistungsfähige native Steuerelemente auf jeder Plattform gerendert. Einige Beispiele für Funktionen, die von Xamarinbereitgestellt werden. Zu den Formularen gehören:

- Sprache der XAML-Benutzeroberfläche
- Datenbindung
- Gesten
- Effekte
- Formatieren

Weitere Informationen finden Sie unter [Xamarin. Formulare](~/xamarin-forms/index.yml).

## <a name="get-started"></a>Erste Schritte

Die folgenden Leitfäden helfen Ihnen, Ihre erste APP mit Xamarinzu erstellen:

- [Beginnen Sie mit Xamarin. Forms](~/xamarin-forms/index.yml)
- [Beginnen Sie mit Xamarin. Erhältlich](~/android/index.yml)
- [Beginnen Sie mit Xamarin. IOS](~/ios/index.yml)
- [Beginnen Sie mit Xamarin. Mac](~/mac/index.yml)

## <a name="related-video"></a>Zugehörige Videos

> [!Video https://channel9.msdn.com/Series/Xamarin-101/What-is-Xamarin-1-of-11/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
