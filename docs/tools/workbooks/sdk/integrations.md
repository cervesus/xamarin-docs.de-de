---
title: Erweiterte Integrationsthemen
description: Dieses Dokument beschreibt die erweiterten Themen, die im Zusammenhang mit Xamarin Workbooks-Integrationen. Es erläutert die Xamarin.Workbook.Integrations NuGet-Paket und die API-Anfälligkeit in einer Xamarin-Arbeitsmappe.
ms.prod: xamarin
ms.assetid: 002CE0B1-96CC-4AD7-97B7-43B233EF57A6
author: lobrien
ms.author: laobri
ms.date: 03/30/2017
ms.openlocfilehash: 56ee709b78b8587c2717dc9d25a6357041812d23
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61382244"
---
# <a name="advanced-integration-topics"></a>Erweiterte Integrationsthemen

Integrationsassemblys verweisen sollten die [ `Xamarin.Workbooks.Integrations` NuGet][nuget]. Sehen Sie sich unsere [Dokumentation für den Schnellstart](~/tools/workbooks/sdk/index.md) für Weitere Informationen zu den ersten Schritten mit dem NuGet-Paket.

Client-Integrationen werden ebenfalls unterstützt, und platzieren Sie JavaScript- oder CSS-Dateien mit dem gleichen Namen wie die Agent-Integration-Assembly im gleichen Verzeichnis initiiert. Z. B., wenn die Agent-Integration-Assembly (das im NuGet Verweise auf) heißt `SampleExternalIntegration.dll`, klicken Sie dann `SampleExternalIntegration.js` und `SampleExternalIntegration.css` wird in den Client auch integriert werden, wenn sie vorhanden sind. Client-Integrationen sind optional.

Die externe Integration selbst kann als ein NuGet-Paket verpackt, bereitgestellt und direkt innerhalb der Anwendung, die den Agent hostet, oder einfach nebeneinander platziert eine `.workbook` -Datei, die es nutzen möchte.

Externe Integrationen in NuGet-Paketen (Agent und Client) werden automatisch geladen, wenn das Paket, wie in der Dokumentation für den Schnellstart verwiesen hat, wird während integrationsassemblys versendet, zusammen mit der Arbeitsmappe benötigen, also als darauf zu verweisen:

```csharp
#r "SampleExternalIntegration.dll"
```

Wenn Sie eine Integration auf diese Weise verweisen, es wird nicht geladen vom Client sofort&mdash;müssen Sie das Aufrufen von Code über die Integration zu laden. Wir werden diesen Fehler in der Zukunft Punkte eingehen.

Die `Xamarin.Interactive` PCL bietet einige wichtige-Integration-APIs. Jeder Integration muss mindestens einen Einstiegspunkt für die Integration bereitstellen:

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

An diesem Punkt nach die Integration Assembly verwiesen wird, lädt der Client implizit JavaScript und CSS-Integration-Dateien.

## <a name="apis"></a>APIs

Wie mit einer beliebigen Assembly, die auf die verwiesen wird von einer Arbeitsmappe oder live-Sitzung ansehen, werden alle seine öffentlichen APIs für die Sitzung zugegriffen werden kann. Aus diesem Grund ist es wichtig, damit eine sichere und sinnvolle API-Oberfläche für Benutzer untersuchen.

Die Assembly für die Integration ist tatsächlich eine Brücke zwischen einer Anwendung oder SDK von Interesse sind und die Sitzung. Sie können neue APIs, die sinnvoll, insbesondere im Kontext einer Arbeitsmappe oder live-Sitzung zu überprüfen, oder geben keine öffentlichen APIs, und führen Sie einfach "hinter den Kulissen"-Aufgaben, z.B. die Rückgabe des Objekts bereitstellen [Darstellungen](~/tools/workbooks/sdk/representations.md).

> [!NOTE]
> APIs, die müssen öffentlich sein, aber nicht über IntelliSense verfügbar gemacht werden soll, können mit der üblichen markiert werden `[EditorBrowsable (EditorBrowsableState.Never)]` Attribut.

[nuget]: https://nuget.org/packages/Xamarin.Workbooks.Integration
