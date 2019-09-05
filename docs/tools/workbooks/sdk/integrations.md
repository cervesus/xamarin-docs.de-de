---
title: Erweiterte Integrationsthemen
description: In diesem Dokument werden erweiterte Themen im Zusammenhang mit Xamarin Workbooks Integrationen beschrieben. Darin werden das nuget-Paket xamarin. Workbook. Integrationen und API-verfügbar in einer xamarin-Arbeitsmappe erläutert.
ms.prod: xamarin
ms.assetid: 002CE0B1-96CC-4AD7-97B7-43B233EF57A6
author: conceptdev
ms.author: crdun
ms.date: 03/30/2017
ms.openlocfilehash: f8105c8285e696f8754799c33c30e31ce5356870
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70283883"
---
# <a name="advanced-integration-topics"></a>Erweiterte Integrationsthemen

Integrationsassemblys sollten auf das [ `Xamarin.Workbooks.Integrations` nuget][nuget]verweisen. Weitere Informationen zu den ersten Schritten mit dem nuget-Paket finden Sie in unserer [Schnellstart Dokumentation](~/tools/workbooks/sdk/index.md) .

Client Integrationen werden ebenfalls unterstützt und durch das Platzieren von JavaScript-oder CSS-Dateien mit demselben Namen wie die Agent-integrationsassembly im gleichen Verzeichnis initiiert. Wenn beispielsweise die Agent-integrationsassembly (die auf den nuget verweist) `SampleExternalIntegration.dll`benannt ist `SampleExternalIntegration.js` , `SampleExternalIntegration.css` dann werden und in den Client integriert, sofern Sie vorhanden sind. Client Integrationen sind optional.

Die externe Integration selbst kann als nuget verpackt werden, bereitgestellt und direkt in der Anwendung referenziert werden, die den Agent gehostet, oder einfach neben `.workbook` einer Datei platziert werden, die Sie verwenden möchte.

Externe Integrationen (Agent und Client) in nuget-Paketen werden automatisch geladen, wenn auf das Paket verwiesen wird, und zwar gemäß der Schnellstart Dokumentation, während Integrationsassemblys, die zusammen mit der Arbeitsmappe ausgeliefert werden, wie folgt darauf verweisen müssen:

```csharp
#r "SampleExternalIntegration.dll"
```

Wenn auf diese Weise auf eine Integration verwiesen wird, wird Sie nicht sofort&mdash;vom Client geladen, sondern es muss Code aus der Integration aufgerufen werden, um Sie zu laden. Dieser Fehler wird in Zukunft behandelt.

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
> APIs, die öffentlich sein müssen, aber nicht über IntelliSense angezeigt werden sollten, können mit dem üblichen `[EditorBrowsable (EditorBrowsableState.Never)]` -Attribut gekennzeichnet werden.

[nuget]: https://nuget.org/packages/Xamarin.Workbooks.Integration
