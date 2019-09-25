---
title: Was ist Xamarin?
description: In diesem Artikel werden xamarin und verwandte Bibliotheken vorgestellt.
ms.prod: xamarin
ms.assetid: 33C83E13-F3E5-17B4-6512-207F3D3C5AB6
author: profexorgeek
ms.author: jusjohns
ms.date: 09/16/2019
ms.openlocfilehash: 8213eeb18ec23e79f0cc2a82c22b50d77b6d4931
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "71256618"
---
# <a name="what-is-xamarin"></a>Was ist Xamarin?

[![Screenshots der Beispiel-xamarin-Anwendung in IOS und Android](what-is-xamarin-images/xamarin-app-cropped.png)](what-is-xamarin-images/xamarin-app.png#lightbox)

Xamarin ist eine Open-Source-Plattform zum Entwickeln moderner und leistungsfähiger Anwendungen für IOS, Android und Windows mit .net. Xamarin ist eine Abstraktions Ebene, die die Kommunikation zwischen frei gegebenem Code und zugrunde liegenden Platt Form Code verwaltet. Xamarin wird in einer verwalteten Umgebung ausgeführt, die Komfort wie Speicher Belegung und Garbage Collection bereitstellt.

Xamarin ermöglicht Entwicklern, einen durchschnittlichen Anteil von 90% ihrer Anwendung plattformübergreifend zu nutzen. Dieses Muster ermöglicht es Entwicklern, Ihre gesamte Geschäftslogik in einer einzelnen Sprache zu schreiben (oder vorhandenen Anwendungscode wiederzuverwenden), aber die systemeigene Leistung, das Aussehen und Verhalten der einzelnen Plattformen zu erreichen.

Xamarin-Anwendungen können auf einem PC oder Mac geschrieben und in Native Anwendungspakete (z. b **. eine APK** -Datei unter Android) oder in einer **IPA** -Datei unter IOS kompiliert werden.

> [!NOTE]
> Das Kompilieren und Bereitstellen von Anwendungen für IOS erfordert derzeit einen MacOS-Computer. Weitere Informationen zu den Entwicklungsanforderungen finden Sie unter [System Requirements (Systemanforderungen](~/cross-platform/get-started/requirements.md#macos-requirements)).

## <a name="who-xamarin-is-for"></a>Wer xamarin ist für

Xamarin ist für Entwickler mit den folgenden Zielen konzipiert:

- Teilen Sie Code, Test und Geschäftslogik plattformübergreifend.
- Schreiben Sie plattformübergreifende Anwendungen C# in mit Visual Studio.

## <a name="how-xamarin-works"></a>Funktionsweise von xamarin

![Diagramm der xamarin-Architektur](what-is-xamarin-images/xamarin-architecture.png)

Das Diagramm zeigt die Gesamtarchitektur einer plattformübergreifenden xamarin-Anwendung. Mit xamarin können Sie auf jeder Plattform eine native Benutzeroberfläche erstellen und eine Geschäfts C# Logik in schreiben, die plattformübergreifend verwendet wird. In den meisten Fällen kann 80% des Anwendungs Codes mithilfe von xamarin freigegeben werden.

Xamarin baut auf **Mono**auf, einer Open Source-Version des .NET Framework basierend auf den .net ECMA-Standards. Mono ist für fast so lange vorhanden wie die .NET Framework selbst und wird auf den meisten Plattformen ausgeführt, einschließlich Linux, UNIX, FreeBSD und macOS. Die Mono-Ausführungsumgebung übernimmt automatisch Aufgaben, wie z. b. Speicher Belegung, Garbage Collection und Interoperabilität mit zugrunde liegenden Plattformen.

Weitere Informationen zur plattformspezifischen Architektur finden Sie unter [xamarin. Android](#xamarinandroid) und [xamarin. IOS](#xamarinios).

### <a name="added-features"></a>Hinzugefügte Features

Xamarin kombiniert die Möglichkeiten nativer Plattformen und bietet eine Reihe von Features, darunter:

1. **Vollständige Bindung für die zugrunde liegenden sdgs** – xamarin enthält Bindungen für fast alle zugrunde liegenden Plattform-sdgs in IOS und Android. Zusätzlich dazu sind diese Bindungen stark typisiert. So bieten sie einfach Navigation, lassen sich leicht nutzen und sorgen während der Entwicklung für eine stabile Typüberprüfung zur Kompilierzeit und während der Entwicklung. Stark typisierte Bindungen führen zu weniger Laufzeitfehlern und Anwendungen höherer Qualität.
1. **Ziel-c, Java, C und C++ Interop** – xamarin bietet Funktionen zum direkten Aufrufen von Ziel-c, Java, c und Bibliotheken und C++ bietet Ihnen die Möglichkeit, ein breites Array von Code von Drittanbietern zu verwenden. Mit dieser Funktion können Sie vorhandene IOS-und Android-Bibliotheken verwenden, die in Ziel-C, JavaC++oder C/geschrieben sind. Außerdem bietet xamarin Bindungs Projekte, mit denen Sie Native Ziel-C-und Java-Bibliotheken mithilfe einer deklarativen Syntax binden können.
1. **Moderne Sprachkonstrukte** – xamarin-Anwendungen werden C#in geschrieben, eine moderne Sprache, die bedeutende Verbesserungen gegenüber Ziel-C und Java enthält, wie z. b. dynamische sprach Features, funktionale Konstrukte wie Lambdas, LINQ, parallel Programmierung, Generika usw.
1. **Robuste Basisklassen Bibliothek (Base Class Library, BCL)** – xamarin-Anwendungen verwenden die .net-BCL, eine große Auflistung von Klassen, die über umfassende und optimierte Features wie leistungsstarke XML-, Datenbank-, Serialisierungs-, e/a-, Zeichen folgen-und Netzwerkunterstützung verfügen. Vorhandener C# Code kann für die Verwendung in einer APP kompiliert werden, die den Zugriff auf Tausende von Bibliotheken ermöglicht, die über die BCL hinaus Funktionalität hinzufügen.
1. **Moderne integrierte Entwicklungsumgebung (Integrated Development Environment, IDE)** – xamarin verwendet Visual Studio, eine moderne IDE, die Funktionen wie die automatische Vervollständigung von Code, ein anspruchsvolles Projekt-und projektmappenverwaltungssystem, eine umfassende Projektvorlagen Bibliothek, enthält. integrierte Quell Code Verwaltung und mehr.
1. **Mobile plattformübergreifende Unterstützung** – xamarin bietet ausgereifte plattformübergreifende Unterstützung für die drei Hauptplattformen von IOS, Android und Windows. Anwendungen können so geschrieben werden, dass Sie bis zu 90% Ihres Codes freigeben können, und xamarin. Essentials bietet eine einheitliche API für den Zugriff auf gemeinsame Ressourcen auf allen drei Plattformen. Mit frei gegebenem Code können Entwicklungskosten und die Markteinführungszeit für Mobile Entwickler erheblich reduziert werden.

### <a name="xamarinandroid"></a>Xamarin.Android

[![Xamarin. Android-Architektur Diagramm](what-is-xamarin-images/android-architecture-cropped.png)](what-is-xamarin-images/android-architecture.png#lightbox)

Xamarin. Android-Anwendungen kompilieren C# aus in **Intermediate Language (IL)** , was dann **Just-in-time (JIT)** in eine systemeigene Assembly kompiliert wird, wenn die Anwendung gestartet wird. Xamarin. Android-Anwendungen werden in der Mono-Ausführungsumgebung parallel zum virtuellen Computer der Android-Laufzeit (Art) ausgeführt. Xamarin stellt .Net-Bindungen für die Namespaces "Android. *" und "java. *" bereit. Die Mono-Ausführungsumgebung ruft diese Namespaces über die **verwalteten Callable Wrapper (MCW)** auf und stellt **Android Callable Wrapper (ACW)** für die Kunst bereit, sodass beide Umgebungen Code aufeinander aufrufen können.

Weitere Informationen finden Sie unter [xamarin. Android-Architektur](~/android/internals/architecture.md).

### <a name="xamarinios"></a>Xamarin.iOS

[![Xamarin. IOS-Architektur Diagramm](what-is-xamarin-images/ios-architecture-cropped.png)](what-is-xamarin-images/ios-architecture.png#lightbox)

Xamarin. IOS-Anwendungen sind vollständig **im Vorfeld (AOT)** , die aus C# in nativem Arm-Assemblycode kompiliert wurden. Xamarin verwendet **Selectors** , um Ziel-c für verwaltete C# und **Registrare** verfügbar zu machen C# , um verwalteten Code für "Ziel-c" verfügbar zu machen. Selektoren und Registrare werden zusammen als "Bindungen" bezeichnet und ermöglichen die Kommunikation mit C# dem Ziel-C und der Kommunikation.

Weitere Informationen finden Sie unter [xamarin. IOS-Architektur](~/ios/internals/architecture.md).

### <a name="xamarinessentials"></a>Xamarin.Essentials

Xamarin. Essentials ist eine Bibliothek, die plattformübergreifende APIs für Native Gerätefunktionen bereitstellt. Wie xamarin selbst ist xamarin. Essentials eine Abstraktion, die den Prozess des Zugriffs auf systemeigene Funktionen vereinfacht. Einige Beispiele für die von xamarin. Essentials bereitgestellte Funktionalität sind:

- Geräteinformationen
- Dateisystem
- Beschleunigungsmesser
- Telefon Einwählprogramm
- Text-zu-Sprache
- Bildschirmsperre

Weitere Informationen finden Sie unter [xamarin. Essentials](~/essentials/index.md).

### <a name="xamarinforms"></a>Xamarin.Forms

Xamarin. Forms ist ein Open-Source-Framework für die Benutzeroberfläche. Xamarin. Forms ermöglicht Entwicklern das Erstellen von IOS-, Android-und Windows-Anwendungen aus einer einzelnen freigegebenen CodeBase. Xamarin. Forms ermöglicht Entwicklern das Erstellen von Benutzeroberflächen in XAML mit Code Behind in C#. Diese Benutzeroberflächen werden als leistungsfähige native Steuerelemente auf jeder Plattform gerendert. Einige Beispiele für die von xamarin. Forms bereitgestellten Features sind:

- Sprache der XAML-Benutzeroberfläche
- Datenbindung
- Gesten
- Effekte
- Format

Weitere Informationen finden Sie unter [xamarin. Forms](~/xamarin-forms/index.yml).

## <a name="get-started"></a>Erste Schritte

Die folgenden Leitfäden helfen Ihnen beim Erstellen Ihrer ersten App mithilfe von xamarin:

- [Beginnen Sie mit xamarin. Forms](~/xamarin-forms/index.yml)
- [Beginnen Sie mit xamarin. Android](~/android/index.yml)
- [Beginnen Sie mit xamarin. IOS](~/ios/index.yml)
- [Beginnen Sie mit xamarin. Mac](~/mac/index.yml)
