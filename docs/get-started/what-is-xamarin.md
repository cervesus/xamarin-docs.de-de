---
title: Was ist Xamarin?
description: In diesem Artikel werden die integrierte Entwicklungsumgebung Xamarin sowie verbundene Bibliotheken eingeführt.
ms.prod: xamarin
ms.assetid: 33C83E13-F3E5-17B4-6512-207F3D3C5AB6
ms.custom: video
author: profexorgeek
ms.author: jusjohns
ms.date: 09/16/2019
ms.openlocfilehash: 34763804e9833224721ea32f9c7e6200dd5faba7
ms.sourcegitcommit: 3f0e4f10e5def19122588bb05f26ab2baa9df6eb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2020
ms.locfileid: "75607879"
---
# <a name="what-is-xamarin"></a>Was ist Xamarin?

[![Screenshots einer Xamarin-Beispielanwendung unter iOS und Android](what-is-xamarin-images/xamarin-app-cropped.png)](what-is-xamarin-images/xamarin-app.png#lightbox)

Bei Xamarin handelt es sich um eine Open-Source-Plattform für das Erstellen moderner und leistungsfähiger Anwendungen für iOS, Android und Windows mit .NET. Xamarin ist eine Abstraktionsebene, die die Kommunikation zwischen freigegebenem Code und dem zugrunde liegenden Plattformcode verwaltet. Xamarin wird in einer verwalteten Umgebung ausgeführt, die Vorteile wie Speicherbelegung und Garbage Collection bietet.

Mithilfe von Xamarin können Entwickler durchschnittlich 90 Prozent Ihrer Anwendung plattformübergreifend freigeben. Dies ermöglicht es Entwicklern, die gesamte Geschäftslogik in einer einzelnen Programmiersprache zu schreiben (oder vorhandenen Anwendungscode wiederzuverwenden), und dennoch native Leistung, natives Aussehen und eine native Servicequalität auf jeder Plattform bieten zu können.

Xamarin-Anwendungen können auf einem Computer oder einem Mac geschrieben werden und werden in native Anwendungspakete kompiliert, z. B. als **APK-Datei** unter Android oder als **IPA-Datei** unter iOS.

> [!NOTE]
> Für die Kompilierung und die Bereitstellung von Anwendungen für iOS ist momentan ein macOS-Computer erforderlich. Weitere Informationen zu Entwicklungsanforderungen finden Sie unter [macOS-Anforderungen](~/cross-platform/get-started/requirements.md#macos-requirements).

## <a name="who-xamarin-is-for"></a>Zielgruppe von Xamarin

Xamarin eignet sich für Entwickler mit den folgenden Zielen:

- Plattformübergreifendes Freigeben von Code, Test- und Geschäftslogik
- Schreiben von plattformübergreifenden Anwendungen in C# mit Visual Studio

## <a name="how-xamarin-works"></a>Funktionsweise von Xamarin

![Diagramm der Xamarin-Architektur](what-is-xamarin-images/xamarin-architecture.png)

In diesem Diagramm sehen Sie die Gesamtarchitektur einer plattformübergreifenden Xamarin-Anwendung. Mithilfe von Xamarin können Sie native Benutzeroberflächen für jede Plattform erstellen und eine Geschäftslogik in C# schreiben, die plattformübergreifend freigegeben wird. In den meisten Fällen können 80 Prozent des Anwendungscodes mithilfe von Xamarin freigegeben werden.

Xamarin baut auf **Mono** auf, einer Open Source-Version von .NET Framework basierend auf den Standards von .NET Ecma International. Mono existiert bereits fast so lange wie .NET Framework selbst und kann auf nahezu allen Plattformen ausgeführt werden, einschließlich Linux, Unix, FreeBSD und macOS. Die Mono-Ausführungsumgebung verarbeitet Aufgaben wie Speicherbelegung, Garbage Collection und Interoperabilität mit zugrunde liegenden Plattformen automatisch.

Weitere Informationen zu plattformspezifischen Architekturen finden Sie unter [Xamarin.Android](#xamarinandroid) und [Xamarin.iOS](#xamarinios).

### <a name="added-features"></a>Hinzugefügte Features

Xamarin kombiniert alle Vorteile der nativen Plattformen und ergänzt diese um eine Reihe weiterer Features, wie beispielweise die folgenden:

1. **Vollständige Bindung für die zugrundeliegenden SDKs:** Xamarin enthält Bindungen für fast alle zugrunde liegenden Plattform-SDKs unter iOS und Android. Zusätzlich dazu sind diese Bindungen stark typisiert. So bieten sie einfach Navigation, lassen sich leicht nutzen und sorgen während der Entwicklung für eine stabile Typüberprüfung zur Kompilierzeit und während der Entwicklung. Stark typisierte Bindungen führen zu weniger Runtimefehlern und zu Anwendungen mit höherer Qualität.
1. **Objective-C, Java, C und C++ Interop:** Xamarin stellt Funktionen zum direkten Aufrufen von Bibliotheken mit Objective-C, Java, C und C++ bereit, sodass Sie verschiedensten Drittanbietercode verwenden können. Dadurch können Sie die vorhandenen iOS- und Android-Bibliotheken nutzen, die in Objective-C, Java oder C/C++ geschrieben sind. Darüber hinaus bietet Xamarin Bindungsprojekte an, mit denen Sie native Objective-C- und Java-Bibliotheken binden können, indem Sie eine deklarative Syntax verwenden.
1. **Moderne Sprachkonstrukte:** Xamarin-Anwendungen werden in C# geschrieben, einer modernen Sprache, die gegenüber Objective-C und Java bedeutende Verbesserungen beinhaltet, z. B. dynamische Sprachfeatures, funktionale Konstrukte wie Lambdas und LINQ, Funktionen für die parallele Programmierung und Generics.
1. **Solide Basisklassenbibliothek (Base Class Library, BCL):** Xamarin-Anwendungen verwenden die .NET-BCL, eine große Sammlung von Klassen mit umfassenden und optimierten Features, z B. Unterstützung für leistungsstarkes XML, Datenbanken, Serialisierung, E/A, Zeichenfolgen und Netzwerke. Vorhandener C#-Code kann zur Verwendung in einer App kompiliert werden. So erhalten Sie Zugriff auf Tausende Bibliotheken, die Ihnen zusätzlich zur BCL Funktionen bieten.
1. **Moderne integrierte Entwicklungsumgebung (Integrated Development Environment, IDE):** Xamarin verwendet Visual Studio, eine moderne IDE, die Features beinhaltet wie die automatische Codevervollständigung, ein durchdachtes Projekt- und Projektmappenverwaltungssystem, eine umfassende Projektvorlagenbibliothek und integrierte Quellcodeverwaltung.
1. **Unterstützung für plattformübergreifende Entwicklung von mobilen Anwendungen:** Xamarin bietet hochmoderne, plattformübergreifende Unterstützung für die drei wichtigsten Plattformen: iOS, Android und Windows. Anwendungen lassen sich so schreiben, dass bis zu 90 Prozent des Codes freigegeben werden können, und in Xamarin.Essentials steht eine vereinheitlichte API zur Verfügung, mit der auf allen drei Plattformen auf häufig verwendete Ressourcen zugegriffen werden kann. Freigegebener Code kann sowohl die Kosten für die Entwicklung als auch die Zeit bis zur Markteinführung für Entwickler mobiler Anwendungen reduzieren.

### <a name="xamarinandroid"></a>Xamarin.Android

[![Architekturdiagramm für Xamarin.Android](what-is-xamarin-images/android-architecture-cropped.png)](what-is-xamarin-images/android-architecture.png#lightbox)

Xamarin.Android-Anwendungen kompilieren von C# zu einer **Zwischensprache (Intermediate Language, IL)** , die dann bei Anwendungsstart **Just-In-Time (JIT)** in eine native Assembly kompiliert wird. Xamarin.Android-Anwendungen werden in der Mono-Ausführungsumgebung ausgeführt, parallel mit der Android Runtime-VM (ART). Xamarin bietet Android.*- und Java.*-Namespaces für .NET-Bindungen. Die Mono-Ausführungsumgebung führt Aufrufe in diese Namespaces über **verwaltete Callable Wrapper (Managed Callable Wrappers, MCWs)** aus und stellt ART **Callable Wrappers für Android (Android Callable Wrappers, ACW)** zur Verfügung, sodass in beiden Umgebungen Code voneinander aufgerufen werden kann.

Weitere Informationen finden Sie unter [Architektur](~/android/internals/architecture.md).

### <a name="xamarinios"></a>Xamarin.iOS

[![Architekturdiagramm für Xamarin.iOS](what-is-xamarin-images/ios-architecture-cropped.png)](what-is-xamarin-images/ios-architecture.png#lightbox)

Xamarin.iOS-Anwendungen werden vollständig von C# zu nativem ARM-Assemblycode **Ahead-of-time (AOT)** -kompiliert. Xamarin verwendet **Selektoren**, um Objective-C für verwalteten C#-Code verfügbar zu machen und **Registratoren**, um verwalteten C#-Code für Objective-C verfügbar zu machen. Selektoren und Registratoren werden zusammen auch als „Bindungen“ bezeichnet und ermöglichen die Kommunikation zwischen Objective-C und C#.

Weitere Informationen finden Sie unter [Xamarin.iOS-Architektur](~/ios/internals/architecture.md).

### <a name="xamarinessentials"></a>Xamarin.Essentials

Bei Xamarin.Essentials handelt es sich um eine Bibliothek, die plattformübergreifende APIs für native Gerätefeatures bietet. Wie Xamarin selbst ist Xamarin.Essentials eine Abstraktion, die den Zugriffsprozess auf native Funktionen vereinfacht. Zu den von Xamarin.Essentials zur Verfügung gestellten Funktionen gehören die folgenden:

- Geräteinformationen
- Dateisystem
- Beschleunigungsmesser
- Wählhilfe
- Text-zu-Sprache
- Bildschirmsperre

Weitere Informationen finden Sie unter [Xamarin.Essentials](~/essentials/index.md).

### <a name="xamarinforms"></a>Xamarin.Forms

Bei Xamarin.Forms handelt es sich um ein Open-Source-Benutzeroberflächenframework. Mithilfe von Xamarin.Forms können Entwickler iOS-, Android- und Windows-Anwendungen aus einer einzigen freigegebenen CodeBase erstellen. Mithilfe von Xamarin.Forms können Entwickler Benutzeroberflächen in XAML mit CodeBehind in C# erstellen. Diese Benutzeroberflächen werden als leistungsfähige native Steuerelemente für jede Plattform gerendert. Zu den von Xamarin.Forms zur Verfügung gestellten Features gehören die folgenden:

- XAML als Programmiersprache für Benutzeroberflächen
- Datenbindung
- Gesten
- Effekte
- Format

Weitere Informationen finden Sie auf der Seite [Xamarin.Forms-Dokumentation](~/xamarin-forms/index.yml).

## <a name="get-started"></a>Erste Schritte

Die folgenden Anleitungen unterstützen Sie bei der Erstellung Ihrer ersten App mithilfe von Xamarin:

- [Xamarin.Forms-Dokumentation](~/xamarin-forms/index.yml)
- [Xamarin.Android](~/android/index.yml)
- [Xamarin.iOS](~/ios/index.yml)
- [Xamarin.Mac](~/mac/index.yml)

## <a name="related-video"></a>Zugehörige Videos

> [!Video https://channel9.msdn.com/Series/Xamarin-101/What-is-Xamarin-1-of-11/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
