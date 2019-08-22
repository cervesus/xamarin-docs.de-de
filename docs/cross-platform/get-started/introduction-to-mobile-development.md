---
title: Was ist Xamarin?
description: Dieser Artikel enthält eine Einführung in die Entwicklung mobiler Anwendungen, die Funktionsweisen von Xamarin und die Anwendungen, die ausgegeben werden.
ms.prod: xamarin
ms.assetid: 33C83E13-F3E5-17B4-6512-207F3D3C5AB6
author: conceptdev
ms.author: crdun
ms.date: 07/16/2019
ms.openlocfilehash: 372aee9d48866ac49f34b9550fdfa37b7cc0b646
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69526664"
---
# <a name="what-is-xamarin"></a>Was ist Xamarin?

Das Erstellen mobiler Anwendungen kann so einfach sein: Öffnen Sie die IDE, schreiben und testen Sie eine App, und übermitteln Sie das Ergebnis dann an den App Store. All dies ist an einem Nachmittag erledigt. Es kann jedoch sehr komplex sein, weil gründliche Vorabentwürfe, Nutzbarkeitstests, QA-Tests auf tausenden Geräten, ein vollständiger Beta-Lebenszyklus und anschließend die Bereitstellung mit einer Reihe verschiedener Methoden nötig ist.

In diesem Dokument wird die Xamarin-Plattform vorgestellt. Weitere Informationen über den *Prozess* zur Entwicklung mobiler Anwendungen vom Entwurf bis zum Test finden Sie unter [Einführung in den Lebenszyklus der mobilen Softwareentwicklung](~/cross-platform/get-started/introduction-to-mobile-sdlc.md).

Vergewissern Sie sich, dass Ihr System die [Systemanforderungen](~/cross-platform/get-started/requirements.md#macos-requirements) erfüllt.

## <a name="introduction-to-xamarin"></a>Einführung in Xamarin

Wenn es um die Art der Erstellung von Android- und iOS-Anwendungen geht, denken viele Menschen, dass die nativen Sprachen Objective-C, Swift, Java und Kotlin die einzigen Möglichkeiten sind.

Xamarin ermöglicht Ihnen die Entwicklung in C#, mit einer Klassenbibliothek und einer Runtime, die auf allen wesentlichen Plattformen funktioniert, einschließlich iOS, Android und Windows, und dennoch native (nicht interpretierte) Anwendungen kompilieren kann, die auch für anspruchsvolle Spiele genügend Leistung bieten.

Xamarin kombiniert alle Vorteile der nativen Plattformen und ergänzt diese um eine Reihe von eigenen leistungsstarken Features, wie beispielweise die folgenden:

1. **Vollständige Bindung für die zugrundeliegenden SDKs**: Xamarin enthält Bindungen für fast alle zugrundeliegenden Plattform SDKs unter iOS und Android. Zusätzlich dazu sind diese Bindungen stark typisiert. So bieten sie einfach Navigation, lassen sich leicht nutzen und sorgen während der Entwicklung für eine stabile Typüberprüfung zur Kompilierzeit und während der Entwicklung. Dies führt zu weniger Laufzeitfehlern und zu Apps mit höherer Qualität.
1. **Objective-C, Java, C und C++ Interop**: Xamarin stellt Funktionen zum direkten Aufrufen von Bibliotheken mit Objective-C, Java, C und C++ bereit, sodass Sie verschiedenen Drittanbietercode verwenden können, der bereits erstellt wurde. Dadurch können Sie die vorhandenen iOS- und Android-Bibliotheken nutzen, die in Objective-C, Java oder C/C++ geschrieben sind. Darüber hinaus bietet Xamarin Bindungsprojekte an, mit denen Sie ganz einfach native Objective-C- und Java-Bibliotheken binden können, indem Sie eine deklarative Syntax verwenden.
1. **Moderne Sprachkonstrukte**: Xamarin-Anwendungen werden in C# geschrieben, einer modernen Sprache, die gegenüber Objective-C und Java bedeutende Verbesserungen beinhaltet, z.B. *dynamische Sprachfunktionen, *funktionale Konstrukte* wie *Lambdas und *LINQ, Funktionen für die *parallele Programmierung*, anspruchsvolle *Generics und viele mehr.
1. **Überzeugende Basisklassenbibliothek (Base Class Library, BCL)** : Xamarin-Anwendungen verwenden die .NET-BCL, eine große Sammlung von Klassen mit umfassenden und optimierten Funktionen, z.B. Unterstützung für leistungsstarkes XML, Datenbanken, Serialisierung, E/A, Zeichenfolgen und Netzwerk, um nur einige zu nennen. Vorhandener C#-Code kann für die Verwendung in einer App kompiliert werden, wodurch Sie Zugriff auf Tausende von Bibliotheken zur Ausführung von Aufgaben erhalten, die nicht in der BCL abgedeckt werden.
1. **Moderne integrierte Entwicklungsumgebung (IDE)** : Xamarin verwendet Visual Studio für Mac unter macOS und Visual Studio unter Windows. Beides sind moderne IDEs, die unter anderem folgende Funktionen enthalten: automatische Codevervollständigung, ein anspruchsvolles Projekt- und Projektmappenverwaltungssystem, eine umfassende Projektvorlagenbibliothek, eine integrierte Quellcodeverwaltung und viele mehr.
1. **Mobile plattformübergreifende Unterstützung**: Xamarin bietet hochmoderne, plattformübergreifende Unterstützung für die drei wichtigsten mobilen Plattformen – iOS, Android und Windows. Bis zu 90 % des Codes einer Anwendung können wiederverwendet werden, und unsere Xamarin.Mobile-Bibliothek bietet eine einheitliche API, um von allen drei Plattformen aus auf allgemeine Ressourcen zuzugreifen. Dadurch können Entwickler, die für die drei beliebtesten mobilen Plattformen entwickeln, signifikant Entwicklungskosten einsparen und die Anwendungen schneller auf den Markt bringen.

Durch die leistungsstarken und umfassenden Funktionen von Xamarin wird eine Lücke für Anwendungsentwickler ausgefüllt, die eine moderne Sprache und Plattform verwenden möchten, um plattformübergreifende mobile Anwendungen zu entwickeln.

> [!NOTE]
> In der Reihe „Erste Schritte“ werden die ersten Schritte beim Erstellen von iOS- und Android-Anwendungen beleuchtet. Microsoft stellt Informationen zur [UWP-Entwicklung (Universelle Windows-Plattform)](https://docs.microsoft.com/windows/uwp/develop/) für Tablets und Desktop-PCs zur Verfügung. Weitere Informationen zur plattformübergreifenden Entwicklung mit Xamarin (einschließlich UWP-Anwendungen für Windows) finden Sie in der Anleitung zum [Erstellen plattformübergreifender Anwendungen](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md).

## <a name="how-does-xamarin-work"></a>Wie funktioniert Xamarin?

Xamarin bietet zwei kommerzielle Produkte an: Xamarin.iOS und Xamarin.Android. Beide bauen auf *Mono* auf, einer Open Source-Version von .NET Framework basierend auf den veröffentlichten Standards von .NET ECMA International. Mono existiert bereits fast so lange wie das .NET Framework selbst und kann auf nahezu jeder erdenklichen Plattform ausgeführt werden, einschließlich Linux, Unix, FreeBSD und macOS.

Unter iOS kompiliert der *Ahead-of-Time-Compiler* (*AOT*) die Xamarin.iOS-Anwendungen direkt in nativen ARM-Assemblycode. Unter Android kompiliert der Compiler von Xamarin in die *Zwischensprache* (*IL*), welche dann beim Start der Anwendung *Just-In-Time* (*JIT*) in eine native Assembly kompiliert wird.

In beiden Fällen verwenden die Xamarin-Anwendungen eine Runtime, die automatisch Aufgaben handhabt, wie die Speicherbelegung, die automatische Speicherbereinigung, die zugrundeliegende Technologie der Plattform-Interop usw.

### <a name="xamariniosdll-and-monoandroiddll"></a>Xamarin.iOS.dll und Mono.Android.dll

Xamarin-Anwendungen werden mit einem Teil der .NET-BCL erstellt, die als das mobile Xamarin-Profil bekannt ist. Dieses Profil wurde speziell für mobile Anwendungen erstellt und in der „Xamarin.iOS.dll“ und der „Mono.Android.dll“ (für iOS bzw. Android) verpackt. Dies entspricht der Art, wie Silverlight- und Moonlight-Anwendungen mit dem Silverlight/Moonlight .NET-Profil erstellt werden. In der Tat gleicht das mobile Xamarin-Profil dem Profil von Silverlight 4.0. Zahlreiche BCL-Klassen wurden wieder hinzugefügt.

Eine vollständige Lister der verfügbaren Assemblys und Klassen finden Sie unter [Xamarin.iOS-Assemblyliste](~/cross-platform/internals/available-assemblies.md?context=xamarin/ios) und [Xamarin.Android-Assemblyliste](~/cross-platform/internals/available-assemblies.md?context=xamarin/android).

Zusätzlich zur BCL enthalten diese .dlls Wrapper für nahezu das gesamte iOS SDK und Android SDK, sodass die zugrundeliegenden SDK-APIs direkt aus C# aufgerufen werden können.

### <a name="application-output"></a>Anwendungsausgabe

Wenn Xamarin-Anwendungen kompiliert werden, entsteht ein Anwendungspaket – entweder eine APP-Datei unter iOS oder eine APK-Datei unter Android. Diese Dateien lassen sich nicht von den Anwendungspaketen unterscheiden, die mit den Standard-IDEs der Plattform erstellt wurden, und sie können auf die gleiche Art und Weise bereitgestellt werden.

## <a name="next-steps"></a>Nächste Schritte

Sie haben nun etwas dazu erfahren, wie Xamarin funktioniert. Beginnen Sie nun als nächsten Schritt mit der Erstellung einer App mithilfe einer dieser Leitfäden:

- [**Erste Schritte mit Xamarin.Forms**](~/get-started/index.yml)
- [**Erste Schritte mit Xamarin.iOS**](~/ios/get-started/hello-ios/index.md)
- [**Erste Schritte mit Xamarin.Android**](~/android/get-started/hello-android/index.md)
- [**Erste Schritte mit Xamarin.Mac**](~/mac/get-started/hello-mac.md)
