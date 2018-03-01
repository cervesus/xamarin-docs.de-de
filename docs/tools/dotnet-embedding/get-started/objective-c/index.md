---
title: Erste Schritte mit Objective-C
ms.topic: article
ms.prod: xamarin
ms.assetid: 4ABC0247-B608-42D4-89CB-D2E598097142
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 832ad2e6d462b39df7fa4a45739e97f7a7d6e5b5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="getting-started-with-objective-c"></a>Erste Schritte mit Objective-C

Dies ist das Abrufen gestarteten Seite für Objective-C, dem behandelt die Grundlagen für alle unterstützten Plattformen.


## <a name="requirements"></a>Anforderungen

Verwendung von .NET einbetten Objective-C benötigen Sie einen Mac ausgeführt wird:

* MacOS 10.12 (Sierra) oder höher
* Xcode 8.3.2 oder höher
* [Mono 5.0](http://www.mono-project.com/download/)

Sie können [Visual Studio für Mac](https://www.visualstudio.com/vs/visual-studio-mac/) bearbeiten und Kompilieren von C#-Code.


Notizen:

* Frühere Versionen von Mac OS, Xcode und Mono _möglicherweise_ arbeiten, sind aber nicht getestet und wird nicht unterstützt;
* Generierung von Code kann auf Windows vorgenommen werden, aber es ist nur möglich, Kompilieren auf einem Macintosh-Computer, auf dem Xcode installiert ist;


## <a name="installation"></a>Installation

Der nächste Schritt ist zum Herunterladen und installieren .NET Einbetten von auf Ihrem Mac.

* [Paket](https://dl.xamarin.com/embeddinator/Xamarin.Embeddinator-4000-0.2.0.79.pkg)
* [Anmerkungen zu dieser Version](https://github.com/mono/Embeddinator-4000/tree/master/docs/releases)

Alternativ ist es möglich, erstellen sie über unsere [Git-Repository](https://github.com/mono/Embeddinator-4000/tree/objc), finden Sie unter der [beitragen](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md) Dokument Anweisungen.

Der Installer ist ein Installationsprogramm standard Pkg Basis:

![Einführung in die Installer](images/install1.png)
![Installer-Installationstyp](images/install2.png)
![Installer-Zusammenfassung](images/install3.png)

Sobald Sie über den Installer installiert, nachdem Sie eine neue Terminalsitzung starten, können Sie die `objcgen` Befehl.
Andernfalls können Sie immer das Tool ausführen, über dessen absoluter Pfad: `/Library/Frameworks/Xamarin.Embeddinator-4000.framework/Commands/objcgen`.

## <a name="platforms"></a>Plattformen

Objective-C ist eine Programmiersprache, die am häufigsten zum Schreiben von Anwendungen für Mac OS, iOS, tvos. außerdem wurden und WatchOS verwendet wird. und die Embeddinator unterstützt alle von diesen Plattformen. Arbeiten mit jeder Plattform impliziert einige wichtige Unterschiede, und diese werden erläutert [hier](~/tools/dotnet-embedding/objective-c/platforms.md).

### <a name="macos"></a>macOS

[Erstellen einer Anwendung MacOS](~/tools/dotnet-embedding/get-started/objective-c/macos.md) ist am einfachsten, da es keine, wie viele zusätzliche Schritte erläutert beinhaltet, wie die Identität, Provisining Profile-Simulatoren und Geräte einrichten. Es wird empfohlen, vor dem Projekt für iOS mit dem Dokument MacOS zu starten.

### <a name="ios--tvos"></a>iOS / tvOS

Stellen Sie sicher, dass Sie bereits eingerichtet sind, iOS-Anwendungen zu entwickeln, bevor Sie versuchen, einen solchen mithilfe der Embeddinator erstellen. Die [Anweisungen](~/tools/dotnet-embedding/get-started/objective-c/ios.md) wird davon ausgegangen, dass Sie bereits erfolgreich erstellt und eine iOS-Anwendung auf Ihrem Computer bereitstellen.

Unterstützung für tvos. außerdem wurden erfolgt analog zur Funktionsweise iOS mithilfe tvos. außerdem wurden Projekte nur in der IDEs (Visual Studio und Xcode) anstelle von iOS-Projekten.

> [!NOTE]
> Hinweis: Unterstützung für WatchOS wird in einer zukünftigen Version verfügbar sein, und es werden iOS/tvos. außerdem wurden sehr ähnlich.


## <a name="further-reading"></a>Weiterführende Themen

* [.NET Einbetten von Funktionen, die spezifisch für Objective-C](~/tools/dotnet-embedding/objective-c/index.md)
* [Bewährte Methoden für Objective-C](~/tools/dotnet-embedding/objective-c/best-practices.md)
* [.NET Einbetten von Einschränkungen](~/tools/dotnet-embedding/limitations.md)
* [Das open-Source-Projekt verwendet werden sollen](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)
* [Fehlercodes und Beschreibungen](~/tools/dotnet-embedding/errors.md)
* [Zielplattformen](~/tools/dotnet-embedding/objective-c/platforms.md)


## <a name="related-links"></a>Verwandte Links

- [Wetter-Beispiel (iOS und Mac OS)](https://github.com/jamesmontemagno/embeddinator-weather)
