---
title: Erweiterte Integrationsthemen
description: In diesem Dokument werden erweiterte Themen im Zusammenhang mit Xamarin Workbooks Integrationen beschrieben. Darin werden das nuget-Paket xamarin. Workbook. Integrationen und API-verfügbar in einer xamarin-Arbeitsmappe erläutert.
ms.prod: xamarin
ms.assetid: 002CE0B1-96CC-4AD7-97B7-43B233EF57A6
author: davidortinau
ms.author: daortin
ms.date: 03/30/2017
ms.openlocfilehash: 159fecbe1385c091c56a6ececb61bf7d020dfc1b
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029599"
---
# <a name="advanced-integration-topics"></a>Erweiterte Integrationsthemen

Integrationsassemblys sollten auf die [`Xamarin.Workbooks.Integrations` nuget][nuget]verweisen. Weitere Informationen zu den ersten Schritten mit dem nuget-Paket finden Sie in unserer [Schnellstart Dokumentation](~/tools/workbooks/sdk/index.md) .

Client Integrationen werden ebenfalls unterstützt und durch das Platzieren von JavaScript-oder CSS-Dateien mit demselben Namen wie die Agent-integrationsassembly im gleichen Verzeichnis initiiert. Wenn beispielsweise die Agent-integrationsassembly (die auf den nuget verweist) `SampleExternalIntegration.dll`benannt ist, werden `SampleExternalIntegration.js` und `SampleExternalIntegration.css` in den Client integriert, sofern Sie vorhanden sind. Client Integrationen sind optional.

Die externe Integration selbst kann als nuget verpackt werden, bereitgestellt und direkt in der Anwendung referenziert werden, die als Host für den Agent dient, oder Sie wird einfach neben einer `.workbook` Datei platziert, die Sie verwenden möchte.

Externe Integrationen (Agent und Client) in nuget-Paketen werden automatisch geladen, wenn auf das Paket verwiesen wird, und zwar gemäß der Schnellstart Dokumentation, während Integrationsassemblys, die zusammen mit der Arbeitsmappe ausgeliefert werden, wie folgt darauf verweisen müssen:

```csharp
#r "SampleExternalIntegration.dll"
```

Wenn auf diese Weise auf eine Integration verwiesen wird, wird Sie nicht sofort vom Client geladen,&mdash;Sie Code von der Integration abrufen müssen, damit Sie geladen werden kann. Dieser Fehler wird in Zukunft behandelt.

Die `Xamarin.Interactive` PCL bietet einige wichtige Integrations-APIs. Jede Integration muss mindestens einen Integrations Einstiegspunkt bereitstellen:

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

Nachdem auf die integrationsassembly verwiesen wurde, lädt der Client dann implizit JavaScript-und CSS-Integrations Dateien.

## <a name="apis"></a>APIs

Wie bei allen Assemblys, auf die von einer Arbeitsmappe oder einer Live Prüfungssitzung verwiesen wird, ist eine ihrer öffentlichen APIs für die Sitzung zugänglich. Daher ist es wichtig, dass Sie über eine sichere und sinnvolle API-Oberfläche verfügen, die Benutzer untersuchen können.

Die integrationsassembly ist praktisch eine Brücke zwischen einer Anwendung oder einem SDK, die von Interesse ist, und der Sitzung. Sie kann neue APIs bereitstellen, die speziell im Kontext einer Arbeitsmappe oder einer Live Prüfungssitzung sinnvoll sind. Sie können auch keine öffentlichen APIs bereitstellen und einfach "im Hintergrund" Aufgaben ausführen, wie das Bereitstellen von Objekt [Darstellungen](~/tools/workbooks/sdk/representations.md).

> [!NOTE]
> APIs, die öffentlich sein müssen, aber nicht über IntelliSense angezeigt werden sollten, können mit dem üblichen `[EditorBrowsable (EditorBrowsableState.Never)]`-Attribut gekennzeichnet werden.

[nuget]: https://nuget.org/packages/Xamarin.Workbooks.Integration
