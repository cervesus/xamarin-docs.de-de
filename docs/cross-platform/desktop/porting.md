---
ms.assetid: 814857C5-D54E-469F-97ED-EE1CAA0156BB
title: Portierung von Desktop-app-Leitfaden
description: Eine einfache Erklärung der Vorgehensweise zum Entkoppeln von vorhandenen Windows Forms oder WPF-apps zum Erstellen von plattformübergreifenden apps für Mac OS, iOS, Android sowie universelle Windows-Plattform/Windows 10 ausgeführt werden soll.
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: b9cfad9c046d4f2ad89506f7172a0418e90478f5
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781045"
---
# <a name="desktop-app-porting-guidance"></a>Portierung von Desktop-app-Leitfaden

Großteil des Anwendungscodes kann in einem der folgenden Bereiche eingeteilt werden:

* Code für die Benutzeroberfläche (z. b. Windows und Schaltflächen)
* 3rd Party-Steuerelemente (z. b. Diagramme)
* Geschäftslogik (z. b. Validierungsregeln)
* Lokalen datenspeicherung und-Zugriff
* Webdienste und Remotedatenzugriff

Für Windows Forms und WPF-Anwendungen, die große Mengen an die Geschäftslogik mit c# (oder Visual Basic.NET) geschrieben wurden, um lokale Daten zuzugreifen und Web-Services-Code kann über Plattformen hinweg gemeinsam verwendet werden.

## <a name="net-portability-analyzer"></a>.NET Portability Analyzer

Unterstützung von Visual Studio 2015 und 2017 die [.NET Portability Analyzer](https://docs.microsoft.com/en-us/dotnet/articles/standard/portability-analyzer) ([für Windows herunterladen](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer)) überprüfen Sie Ihre vorhandenen Anwendungen und Aufschluss darüber, wie viel Code portiert werden kann "wie besehen" für andere Plattformen können . Weitere Informationen finden sie aus diesem [Channel 9-Video](https://channel9.msdn.com/Blogs/Seth-Juarez/A-Brief-Look-at-the-NET-Portability-Analyzer).

Es gibt auch ein Befehlszeilentool kann heruntergeladen werden [Portability Analyzer auf GitHub](https://github.com/Microsoft/dotnet-apiport) und verwendet, um die gleichen Berichte bereitzustellen.

## <a name="x-of-my-code-is-portable-what-next"></a>"X % meines Codes ist übertragbar. Was tue ich jetzt?"

Hoffentlich treten bei der Analyzer wird gezeigt, eine große ist übertragbar, die Teil des Codes muss aber es ist sicherlich werden einige Teile der alle Apps, die _kann nicht_ verschoben werden, für andere Plattformen.

Andere Abschnitte des Codes fallen wahrscheinlich in einer von diesen Buckets, die im folgenden ausführlicher erläutert:

* Aktionsentwicklung portablen code
* Code, der geändert werden kann
* Code, der nicht tragbaren und erfordert ein erneutes Schreiben

### <a name="re-useable-portable-code"></a>Aktionsentwicklung portablen code

.NET Code, die auf APIs verfügbar, die auf allen Plattformen geschrieben werden kann, plattformübergreifende unverändert übernommen werden. Im Idealfall müssen Sie möglicherweise mit diesem Code in eine Portable Class Library, gemeinsam genutzte Bibliothek oder Standardbibliothek des .NET verschieben, und klicken Sie dann in Ihre vorhandene app zu testen.

Diese freigegebene Bibliothek kann Anwendungsprojekte für andere Plattformen (z. B. Android, iOS, Mac OS) dann hinzugefügt werden.

### <a name="code-that-requires-changes"></a>Code, der geändert werden kann

Einige .NET APIs möglicherweise nicht auf allen Plattformen verfügbar. Wenn diese APIs in Ihrem Code vorhanden sind, müssen Sie diese Abschnitte für die plattformübergreifende-APIs verwenden, neu zu schreiben.

Beispiele hierfür sind für die Verwendung von Reflektions-APIs, die in .NET 4.6 verfügbar sind, jedoch sind nicht auf allen Plattformen verfügbar.

Nachdem Sie den Code mithilfe von portablen APIs neu geschrieben haben, sollten Sie möglicherweise den Code in einer freigegebenen Bibliothek-Paket, und Testen Sie es in Ihre vorhandene app.

### <a name="code-that-isnt-portable-and-requires-a-re-write"></a>Code, der nicht tragbaren und erfordert ein erneutes Schreiben

Beispiele für Code, die wahrscheinlich über Plattformen hinweg nicht:

- **Benutzeroberfläche** – Windows Forms- oder WPF Bildschirme können nicht in Projekten für Android oder iOS, z. B. verwendet werden. Die Benutzeroberfläche müssen neu geschrieben werden, verwenden Sie diese Funktion [Steuerelemente Vergleich](~/cross-platform/desktop/controls/index.md) als Referenz.

- **Clientplattform-spezifische Speicher** -Code, der auf eine plattformspezifische-Technologie (z. B. eine lokale SQL Server Express-Datenbank) basiert. Sie müssen dies neu schreiben, verwenden eine plattformübergreifende Alternative (z. B. SQLite für Database Engine).
Einige Vorgänge mit dem Dateisystem möglicherweise ebenfalls angepasst werden, da universelle Windows-Plattform etwas andere APIs für Android und iOS (z. b. hat, Einige Dateisysteme Groß-/Kleinschreibung beachtet, und andere nicht).

- **3. Komponenten von Drittanbietern** – überprüfen Sie, ob 3rd Party-Komponenten in Ihren Anwendungen auf anderen Plattformen verfügbar sind. Einige, z. B. nicht sichtbare Gesamtwerte NuGet-Pakete können jedoch andere (insbesondere visual steuert, wie Diagramme oder Media Player) verfügbar sein.

## <a name="tips-for-making-code-portable"></a>Tipps zur optimalen Code portabel

- **Abhängigkeitsinjektion** – bieten verschiedene Implementierungen für jede Plattform und

- **In den Ebenen Ansatz** – ob MVVM, MVC, MVP oder einige andere Muster, das Ihnen hilft, trennen die portablen Code aus der plattformspezifischen Code.

- **Messaging** – können Sie Meldungsübergabe in Ihrem Code um deserialisiert Interaktionen zwischen verschiedenen Teilen der Anwendung zu verknüpfen.
