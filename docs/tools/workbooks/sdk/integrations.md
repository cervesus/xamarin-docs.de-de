---
title: Erweiterte Integration-Themen
ms.prod: xamarin
ms.assetid: 002CE0B1-96CC-4AD7-97B7-43B233EF57A6
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: fc108c3d7f6f4c0fbd948182b4e60a3eee0aea0a
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
---
# <a name="external-integrations"></a>Externe Integrationen

Integrationsassemblys verweisen soll die [ `Xamarin.Workbooks.Integrations` NuGet][nuget]. Sehen Sie sich unsere [Schnellstart-Dokumentation](~/tools/workbooks/sdk/index.md) für Weitere Informationen zu den ersten Schritten mit dem NuGet-Paket.

Client-Integrationen werden ebenfalls unterstützt und durch Platzieren von JavaScript- oder CSS-Dateien mit dem gleichen Namen wie die Agent-Integration-Assembly im selben Verzeichnis initiiert. Wenn die Agent-Integration-Assembly (die verweist auf die NuGet) heißt beispielsweise `SampleExternalIntegration.dll`, klicken Sie dann `SampleExternalIntegration.js` und `SampleExternalIntegration.css` wird in der Client als auch integriert werden, wenn sie vorhanden sind. Client-Integrationen sind optional.

Die externe Integration selbst werden als als NuGet-Paket, bereitgestellt und können verwiesen wird, direkt in der Anwendung, die den Agent hostet, oder einfach platziert eine `.workbook` -Datei, die sie verarbeiten möchte.

Externe Integrationen in NuGet-Pakete (Agent und Client) werden automatisch geladen, wenn das Paket gemäß der Dokumentation Schnellstart-, referenziert wird während integrationsassemblys ausgeliefert, die zusammen mit der Arbeitsmappe benötigen, als darauf zu verweisen:

```csharp
#r "SampleExternalIntegration.dll"
```

Wenn eine Integration auf diese Weise verwiesen wird, es wird nicht geladen werden vom Client sofort&mdash;müssen Teil des Codes von der Integration in das Laden aufrufen. Wir werden diesen Fehler in der Zukunft adressiert werden.

Die `Xamarin.Interactive` PCL bietet einige wichtige Integration APIs. Jede Integration muss über mindestens einen Einstiegspunkt für die Integration ermöglichen:

```csharp
using Xamarin.Interactive;

[assembly: AgentIntegration (typeof (AgentIntegration))]

class AgentIntegration : IAgentIntegration
{
    const string TAG = nameof (AgentIntegration);

    public void IntegrateWith (IAgent agent)
    {
        // hook into IAgent APIs
    }
}
```

An diesem Punkt nach die Integration Assembly verwiesen wird, lädt der Client implizit JavaScript- und CSS-Integration Dateien.

## <a name="apis"></a>APIs

Wie mit jeder beliebigen Assembly, die verwiesen wird, indem Sie eine Arbeitsmappe oder live Sitzung zu überprüfen, sind seine öffentlichen APIs zugegriffen werden kann, um die Sitzung. Aus diesem Grund ist es wichtig, dass eine sichere und sinnvolle API-Oberfläche für Benutzer zu durchsuchen.

Die Integration-Assembly wird effektiv eine Brücke zwischen einer Anwendung oder SDK von Interesse sind und die Sitzung. Sie können neue APIs, die sinnvoll insbesondere im Kontext einer Arbeitsmappe oder live überprüfen die Sitzung, oder geben Sie keine öffentlichen APIs und einfach "im Hintergrund" aufgeführten Aufgaben ausführen wie das Objekt stellt bereitstellen [Darstellungen](~/tools/workbooks/sdk/representations.md).

> [!NOTE]
> APIs, die müssen öffentlich sein, aber nicht über IntelliSense angefügt werden soll kann gekennzeichnet werden, mit der üblichen `[EditorBrowsable (EditorBrowsableState.Never)]` Attribut.

[nuget]: https://nuget.org/packages/Xamarin.Workbooks.Integration
