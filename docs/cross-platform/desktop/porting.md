---
ms.assetid: 814857C5-D54E-469F-97ED-EE1CAA0156BB
title: Portierungsleitfadens Desktop-app
description: Eine einfache Erklärung der Vorgehensweise zum Entkoppeln von vorhandenen Windows Forms oder WPF-apps zum Erstellen von plattformübergreifenden apps auf MacOS, iOS, Android, sowie UWP/Windows 10 ausgeführt werden soll.
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: 4bf1dea170bd6b63209693963d54cc2e16163eea
ms.sourcegitcommit: 650458de1d362cd7de174cacef7838f0e74426f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2019
ms.locfileid: "58071098"
---
# <a name="desktop-app-porting-guidance"></a>Portierungsleitfadens Desktop-app

Großteil des Anwendungscodes kann in einer der folgenden Bereiche eingeteilt werden:

* Der Code der Benutzeroberfläche (z. b. Fenster und Schaltflächen)
* 3rd Party-Steuerelemente (z. b. Diagramme)
* Geschäftslogik (z. b. Validierungsregeln)
* Lokale datenspeicherung und-Zugriff
* Web Services "und" Remotedatenzugriff

Für Windows Forms und WPF-Anwendungen mit C# (oder Visual Basic.NET) überraschend viel von der Geschäftslogik, die Zugriffe auf lokale Daten und die Web Services-Code plattformübergreifend gemeinsam genutzt werden kann.

## <a name="net-portability-analyzer"></a>.NET Portability Analyzer

Visual Studio 2017 und höher, unterstützt die [.NET Portability Analyzer](https://docs.microsoft.com/dotnet/articles/standard/portability-analyzer) ([für Windows herunterladen](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer)) die können Ihre vorhandenen Anwendungen zu untersuchen und teilt Ihnen mit wie viel Code portiert werden kann, "wie besehen" an andere Plattformen. Mehr darüber erfahren Sie aus diesem [Channel 9-Video](https://channel9.msdn.com/Blogs/Seth-Juarez/A-Brief-Look-at-the-NET-Portability-Analyzer).

Es gibt auch ein Befehlszeilentool kann heruntergeladen werden [Portability Analyzer auf GitHub](https://github.com/Microsoft/dotnet-apiport) und verwendet, um die gleichen Berichte bereitzustellen.

## <a name="x-of-my-code-is-portable-what-next"></a>"X % meines Codes ist übertragbar. Was geschieht nun?"

Hoffentlich wird der Analyzer eine große, Teil des Codes ist übertragbar, aber sicherlich also einige Teile der einzelnen Apps, die _kann nicht_ auf anderen Plattformen verschoben werden.

Verschiedene Blöcke von Code greift wahrscheinlich in einer von diesen Buckets, die im folgenden ausführlicher erläutert:

* Aktionsentwicklung portablen code
* Code, der geändert werden kann
* Code, der nicht portabel ist, und erfordert ein erneutes Schreiben

### <a name="re-useable-portable-code"></a>Aktionsentwicklung portablen code

.NET Code, der auf allen Plattformen verfügbaren APIs erstellt wurde, kann plattformübergreifende unverändert übernommen werden. Im Idealfall werden der gesamte Code in eine Portable Class Library, freigegebene Bibliothek oder .NET Standard-Bibliothek verschieben und anschließend in Ihre vorhandene app testen können.

Diese freigegebenen Bibliothek kann Anwendungsprojekte für andere Plattformen (z. B. Android, iOS, MacOS) klicken Sie dann hinzugefügt werden.

### <a name="code-that-requires-changes"></a>Code, der geändert werden kann

Einige .NET APIs möglicherweise nicht auf allen Plattformen verfügbar. Wenn diese APIs in Ihrem Code vorhanden ist, müssen Sie diese Abschnitte für die plattformübergreifende APIs verwenden, neu zu schreiben.

Beispiele hierfür sind für die Verwendung von Reflektions-APIs, die in .NET 4.6 verfügbar sind, jedoch sind nicht auf allen Plattformen verfügbar.

Nachdem Sie den Code mithilfe von portablen APIs erneut geschrieben haben, sollten Sie in der Lage, diesen Code in einer freigegebenen Bibliothek und Testen Sie es in Ihrer vorhandenen app.

### <a name="code-that-isnt-portable-and-requires-a-re-write"></a>Code, der nicht portabel ist, und erfordert ein erneutes Schreiben

Beispiele für Code, der es sich wahrscheinlich um plattformübergreifende nicht enthalten:

- **Benutzeroberfläche** – Windows Forms- oder WPF-Bildschirmen können nicht in Projekten auf Android oder iOS, z. B. verwendet werden. Ihre Benutzeroberfläche müssen neu geschrieben werden, mithilfe dieser [Vergleich der Steuerelemente](~/cross-platform/desktop/controls/index.md) als Verweis.

- **Clientplattform-spezifische Storage** -Code, der auf eine bestimmte Technologie (z. B. eine lokale SQL Server Express-Datenbank) basiert. Sie müssen diese neu schreiben, verwenden eine plattformübergreifende Alternative (z. B. SQLite für die Datenbank-Engine).
Einige Dateisystemvorgänge müssen möglicherweise auch angepasst werden, da UWP etwas andere APIs für Android und iOS (z.B.) verfügt, Einige Dateisysteme Groß-/Kleinschreibung beachtet werden, und andere nicht).

- **3. Komponenten von Drittanbietern** – überprüfen, ob 3. Komponenten von Drittanbietern in Ihren Anwendungen auf anderen Plattformen verfügbar sind. Einige, wie z. B. nicht visuelle NuGet-Pakete, möglicherweise jedoch andere (insbesondere visuelle Steuerelemente wie Diagrammen oder Media Player) verfügbar sein.

## <a name="tips-for-making-code-portable"></a>Tipps zum machen Code portabel

- **Abhängigkeitsinjektion** – Geben Sie unterschiedliche Implementierungen für jede Plattform und

- **Ebenenansatz** – ob es sich bei MVVM, MVC, MVP oder einige andere Muster, das Ihnen hilft, trennen übertragbaren Code im plattformspezifischen Code.

- **Messaging** – können Sie Meldungsübergabe in Ihrem Code um Interaktionen zwischen verschiedenen Teilen der Anwendung aufheben zu koppeln.
