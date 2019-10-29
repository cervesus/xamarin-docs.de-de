---
ms.assetid: 814857C5-D54E-469F-97ED-EE1CAA0156BB
title: Leitfaden zur Desktop-App-Portierung
description: Eine einfache Erläuterung, wie vorhandene Windows Forms oder WPF-apps entkoppelt werden, um plattformübergreifende Apps für die Ausführung unter macOS, Ios, Android und UWP/Windows 10 zu erstellen.
author: davidortinau
ms.author: daortin
ms.date: 04/26/2017
ms.openlocfilehash: 181034a4936b2da010a2fcd280ded1a3419d43ae
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016452"
---
# <a name="desktop-app-porting-guidance"></a>Leitfaden zur Desktop-App-Portierung

Der größte Anwendungscode kann in einem der folgenden Bereiche kategorisiert werden:

- Benutzeroberflächen Code (z. b. Fenster und Schaltflächen)
- Steuerelemente von Drittanbietern (z. b. Tabellen
- Geschäftslogik (z. b. Validierungsregeln)
- Lokale Datenspeicherung und-Zugriff
- Webdienste und Remote Datenzugriff

Bei Windows Forms-und WPF-Anwendungen C# , die mit (oder Visual Basic.net) geschrieben wurden, kann eine überraschende Menge an Geschäftslogik, dem lokalen Datenzugriff und dem Webdienst Code plattformübergreifend genutzt werden.

## <a name="net-portability-analyzer"></a>.Net Portabilitäts Analyse

Visual Studio 2017 und höhere Versionen unterstützen die [.net-Portabilitäts Analyse](https://docs.microsoft.com/dotnet/articles/standard/portability-analyzer) ([Download für Windows](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer)), mit der Sie Ihre vorhandenen Anwendungen untersuchen und angeben können, wie viel Code "wie besehen" auf andere Plattformen portiert werden kann. Weitere Informationen hierzu finden Sie in diesem [Channel 9-Video](https://channel9.msdn.com/Blogs/Seth-Juarez/A-Brief-Look-at-the-NET-Portability-Analyzer).

Es gibt auch ein Befehlszeilen Tool, das von der [Portabilitäts Analyse auf GitHub](https://github.com/Microsoft/dotnet-apiport) heruntergeladen und zum Bereitstellen der gleichen Berichte verwendet werden kann.

## <a name="x-of-my-code-is-portable-what-next"></a>"x% meines Codes ist portabel. Wie geht es weiter? "

Hoffentlich zeigt der Analyzer, dass ein großer Teil des Codes portabel ist, aber es gibt sicherlich einige Teile jeder APP, die nicht auf andere Plattformen verschoben werden _können_ .

Unterschiedliche Code Blöcke werden wahrscheinlich in einem dieser bucketblöcke angezeigt, die unten ausführlicher erläutert werden:

- Wiederverwendbarer portabler Code
- Code, der Änderungen erfordert
- Code, der nicht portabel ist und einen erneuten Schreibvorgang erfordert

### <a name="re-useable-portable-code"></a>Wiederverwendbarer portabler Code

.NET-Code, der für APIs geschrieben wird, die auf allen Plattformen verfügbar sind, kann plattformübergreifend unverändert genommen werden. Im Idealfall können Sie den gesamten Code in eine portable Klassenbibliothek, eine freigegebene Bibliothek oder eine .NET Standard Bibliothek verschieben und diese dann in Ihrer vorhandenen APP testen.

Diese freigegebene Bibliothek kann dann Anwendungsprojekten für andere Plattformen (z. b. Android, Ios, macOS) hinzugefügt werden.

### <a name="code-that-requires-changes"></a>Code, der Änderungen erfordert

Einige .NET-APIs sind möglicherweise nicht auf allen Plattformen verfügbar. Wenn diese APIs in Ihrem Code vorhanden sind, müssen Sie diese Abschnitte neu schreiben, um plattformübergreifende APIs zu verwenden.

Beispiele hierfür sind die Verwendung von Reflection-APIs, die in .NET 4,6 verfügbar sind, aber auf allen Plattformen nicht verfügbar sind.

Nachdem Sie den Code mithilfe von portablen APIs neu geschrieben haben, sollten Sie in der Lage sein, diesen Code in einer freigegebenen Bibliothek zu verpacken und in Ihrer vorhandenen APP zu testen.

### <a name="code-that-isnt-portable-and-requires-a-re-write"></a>Code, der nicht portabel ist und einen erneuten Schreibvorgang erfordert

Beispiele für Code, der wahrscheinlich nicht plattformübergreifend ist, sind:

- **Benutzeroberfläche** – Windows Forms-oder WPF-Bildschirme können z. b. nicht in Projekten unter Android oder IOS verwendet werden. Die Benutzeroberfläche muss neu geschrieben werden, wobei dieser Steuerelement [Vergleich](~/cross-platform/desktop/controls/index.md) als Verweis verwendet wird.

- **Plattformspezifischer Speicher** Code, der auf einer plattformspezifischen Technologie basiert (z. b. eine lokale SQL Server Express Datenbank). Sie müssen dies mithilfe einer plattformübergreifenden Alternative (z. b. sqlite für die Datenbank-Engine) neu schreiben.
Einige Dateisystem Vorgänge müssen möglicherweise auch angepasst werden, da UWP etwas andere APIs für Android und IOS hat (z. b. bei manchen Dateisystemen wird die Groß-/Kleinschreibung beachtet, andere hingegen nicht.

- **Komponenten von Drittanbietern** – überprüfen Sie, ob Komponenten von Drittanbietern in Ihren Anwendungen auf anderen Plattformen verfügbar sind. Einige, z. b. nicht visuelle nuget-Pakete, sind möglicherweise verfügbar, andere (insbesondere visuelle Steuerelemente wie Diagramme oder Media Player).

## <a name="tips-for-making-code-portable"></a>Tipps für die portablen Code Erstellung

- **Abhängigkeitsinjektion** – bietet verschiedene Implementierungen für jede Plattform und

- Mehrstufiger **Ansatz** – ob MVVM, MVC, MVP oder ein anderes Muster, das Ihnen hilft, den portablen Code vom plattformspezifischen Code zu trennen.

- **Messaging** – Sie können die Nachrichten Übergabe in Ihrem Code verwenden, um Interaktionen zwischen verschiedenen Teilen der Anwendung aufzuheben.
