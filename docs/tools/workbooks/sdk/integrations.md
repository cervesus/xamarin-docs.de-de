---
title: Erweiterte Integration-Themen
description: Dieses Dokument beschreibt Erweiterte Themen im Zusammenhang mit der Xamarin-Arbeitsmappen Integrationen. Es werden die Xamarin.Workbook.Integrations NuGet-Paket und die API Offenlegung innerhalb einer Arbeitsmappe Xamarin behandelt.
ms.prod: xamarin
ms.assetid: 002CE0B1-96CC-4AD7-97B7-43B233EF57A6
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: 1aa6b5d0ca574345e1d349ea53df96f554c06bc4
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793835"
---
# <a name="advanced-integration-topics"></a>Erweiterte Integration-Themen

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
